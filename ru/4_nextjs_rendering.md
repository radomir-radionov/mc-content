# Next.js — Rendering

Стратегии рендеринга, границы Server/Client и потоковая передача.

[ ← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [1. Rendering strategies](#1-rendering-strategies)
- [2. Server & Client boundaries](#2-server-client-boundaries)
- [3. Streaming & Suspense](#3-streaming-suspense)

---

<a name="1-rendering-strategies"></a>

## 1. Rendering strategies

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Overview</summary>

### Overview

**Definition:**

Next.js поддерживает несколько режимов рендеринга для каждого маршрута. Маршрутизатор приложений по умолчанию использует статическое поведение, где это возможно, и выбирает динамическое поведение, когда маршрут считывает данные времени запроса (`cookies`, `headers`, `searchParams`, некэшированный `fetch`).

| Стратегия | Когда создается HTML                    | Свежесть данных        | SEO            |
| --------- | --------------------------------------- | ---------------------- | -------------- |
| **CSR**   | Браузер (только Client Components)      | Всегда актуально       | Слабый без SSR |
| **SSR**   | Сервер при каждом запросе               | Всегда свежий          | Сильный        |
| **SSG**   | Во время сборки                         | Статично до пересборки | Сильный        |
| **ISR**   | Сборка + периодическая revalidation     | Stale-while-revalidate | Сильный        |
| **PPR**   | Статическая оболочка + потоковые «дыры» | Смешанно по сегментам  | Сильный        |

**Examples:**

```tsx
// SSG (по умолчанию) — HTML при сборке; fetch с force-cache, если не переопределён
// app/about/page.tsx
export default async function About() {
  const data = await fetch("https://api.example.com/about").then((r) =>
    r.json(),
  );
  return <main>{data.title}</main>;
}

// SSR — свежий HTML на каждый запрос (достаточно одного триггера)
// app/dashboard/page.tsx
import { cookies } from "next/headers";

export const dynamic = "force-dynamic";

export default async function Dashboard() {
  const session = (await cookies()).get("session");
  return <h1>Hello, {session?.value ?? "guest"}</h1>;
}

// ISR — SSG + обновление по таймеру (Next.js 16, cacheComponents: true)
// app/products/page.tsx
import { cacheLife, cacheTag } from "next/cache";

async function getProducts() {
  "use cache";
  cacheTag("products");
  cacheLife({ revalidate: 3600 });
  return fetch("https://api.example.com/products").then((r) => r.json());
}

export default async function ProductsPage() {
  const products = await getProducts();
  return (
    <ul>
      {products.map((p: { id: string; title: string }) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}

// CSR — только в браузере; для этого компонента нет серверного HTML
// app/counter.tsx
("use client");

import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}

// PPR — статическая оболочка + потоковые динамические сегменты (Next.js 16, cacheComponents: true)
// app/page.tsx
import { Suspense } from "react";
import { cookies } from "next/headers";

async function Greeting() {
  const name = (await cookies()).get("user")?.value;
  return <p>Hi, {name ?? "guest"}</p>;
}

export default function Page() {
  return (
    <section>
      <h1>Welcome</h1>
      <Suspense fallback={<p>Loading…</p>}>
        <Greeting />
      </Suspense>
    </section>
  );
}
```

**Thoughts:** См. [4_react.md §8 — Серверный рендеринг и RSC](./4_react_server.md#8-server-rendering--rsc) для SPA, SSR и RSC. Таксономия (MPA против SPA, CSR/SSR/SSG/PWA): [6_web_api.md — Типы веб-приложений](./6_web_api_apps.md#types-of-web-apps). Next.js реализует их на уровне **сегмента маршрута**. Подробные разборы с таблицами и flow: [SSG](#static-generation-ssg) · [SSR](#server-side-rendering-ssr) · [ISR](#incremental-static-regeneration-isr) · [PPR](#partial-prerendering-ppr-cache-components) · [Combining strategies](#combining-rendering-strategies). Примеры Next.js 16 предполагают `cacheComponents: true`.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching)

</details>

<details>
  <summary>Static Generation (SSG)</summary>

### Static Generation (SSG)

**Definition:** Static Site Generation (SSG) предварительно рендерит **HTML и RSC payload** во **время сборки** (`next build`), а не при каждом запросе. Один и тот же результат отдаётся всем посетителям до следующей сборки или истечения окна ISR revalidation.

В App Router SSG — **поведение по умолчанию** для любого сегмента маршрута, который:

- Использует Server Components (без `"use client"` в корне `page`)
- **Не** читает request-time API — `cookies()`, `headers()`, `searchParams` или `connection()`
- Использует `fetch` с кэшем по умолчанию (`force-cache`) или помечает данные директивой `"use cache"`

Next.js сохраняет результат в **Full Route Cache** (пререндеренный HTML + RSC payload на маршрут). CDN или статический хостинг могут отдавать страницу без Node-сервера на каждый просмотр. Эквивалент Pages Router: `getStaticProps` (+ `getStaticPaths` для динамических маршрутов). Эквивалент App Router: кэшированный `fetch` по умолчанию + опциональный `generateStaticParams()` для сегментов `[slug]`.

**How it works:**

| Этап       | Что происходит                                                               |
| ---------- | ---------------------------------------------------------------------------- |
| Сборка     | Server Components выполняются; данные загружаются; генерируются HTML + RSC   |
| Деплой     | Кэш маршрута уходит вместе со сборкой — готов к CDN                          |
| Запрос     | Браузер сразу получает HTML с контентом; Client Components гидратируются     |
| Обновление | Полная пересборка или переход на [ISR](#incremental-static-regeneration-isr) |

Маршрут **выходит из SSG**, если читает request-time данные, использует `fetch(..., { cache: "no-store" })` или задаёт `export const dynamic = "force-dynamic"`.

**When to use:** Маркетинговые страницы, блоги, документация, каталоги — контент общий для всех пользователей и допускает свежесть на уровне сборки или ISR.

**Pluses and minuses:**

| +                                                          | −                                                             |
| ---------------------------------------------------------- | ------------------------------------------------------------- |
| Самая быстрая доставка CDN, самая низкая стоимость сервера | Время сборки увеличивается с увеличением количества страниц   |
| Предсказуемая производительность                           | Не подходит для индивидуального контента без загрузки клиента |

**Examples:**

У каждой части своя роль — они работают вместе, но ни одна сама по себе не означает «SSG»:

| Piece                    | Роль при сборке (`next build`)                                               | Роль после деплоя                                 |
| ------------------------ | ---------------------------------------------------------------------------- | ------------------------------------------------- |
| `generateStaticParams()` | **Какие URL** рендерить (`/blog/a`, `/blog/b`, …)                            | Известные slug по-прежнему из кэша                |
| `"use cache"`            | Помечает вывод `getPost` как **сохраняемый** (HTML + данные)                 | Переиспользует кэш вместо повторного fetch        |
| `cacheLife("days")`      | Срок жизни **достаточно длинный**, чтобы включить в сборку (не dynamic hole) | Revalidate ~1 день; `expire` ~1 неделя            |
| `cacheTag("posts")`      | Тег для записи кэша                                                          | `revalidateTag("posts")` обновляет при публикации |

```tsx
// next.config.ts — шаг 0: включить Cache Components (Next.js 16+)
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  cacheComponents: true,
};

export default nextConfig;
```

```tsx
// app/blog/[slug]/page.tsx
import { cacheLife, cacheTag } from "next/cache";

type Post = { slug: string; title: string; body: string };

// Шаги 2–4: кэшируемая загрузка данных
async function getPost(slug: string): Promise<Post> {
  "use cache"; // вывод можно сохранить и переиспользовать

  cacheTag("posts"); // имя для on-demand сброса (шаг 5)
  cacheLife("days"); // revalidate ~1 день, expire ~1 неделя — достаточно для включения в сборку
  // cacheLife("seconds") слишком короткий → dynamic hole, fetch при каждом запросе

  const res = await fetch(`https://api.example.com/posts/${slug}`);
  if (!res.ok) throw new Error("Post not found");
  return res.json();
}

// Шаг 1: какие страницы /blog/[slug] генерировать при сборке
export async function generateStaticParams() {
  const posts: Post[] = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return posts.map((post) => ({ slug: post.slug }));
  // → next build запускает PostPage для каждого slug и кладёт HTML в Full Route Cache
}

export default async function PostPage({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const post = await getPost(slug); // закэшировано при сборке; переиспользуется позже

  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </article>
  );
}
```

```tsx
// app/blog/actions.ts — шаг 5: обновить кэш после публикации в CMS (НЕ вызывает сборку)
"use server";

import { revalidateTag } from "next/cache";

export async function publishPost() {
  await saveToCms();
  revalidateTag("posts", "max"); // сброс по тегу; следующий визит перегенерирует getPost
}
```

**Build flow:**

```text
next build
  → generateStaticParams() возвращает [{ slug: "hello" }, { slug: "world" }]
  → для каждого slug: PostPage → getPost(slug) выполняется один раз
  → "use cache" + cacheLife("days") → результат в Full Route Cache
  → деплой: CDN отдаёт готовый HTML (без рендера на сервере на каждого посетителя)
```

**Thoughts:** `cacheLife` **не** запускает пререндер — он только задаёт срок жизни кэша. Пререндер происходит, потому что `generateStaticParams` перечисляет URL, а маршрут остаётся статическим (без `cookies()` / `headers()`). Короткие профили вроде `cacheLife("seconds")` **исключают** контент из сборки. Подробнее о таймере и on-demand обновлении — [ISR](#incremental-static-regeneration-isr).

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache) · [generateStaticParams](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

</details>

<details>
  <summary>Server-Side Rendering (SSR)</summary>

### Server-Side Rendering (SSR)

**Definition:** Server-Side Rendering (SSR) генерирует **свежий HTML при каждом запросе**. Сервер выполняет Server Components, загружает данные и отдаёт новую страницу — ничего не переиспользуется из **Full Route Cache**, собранного при деплое.

В отличие от [SSG](#static-generation-ssg), нет общего HTML для всех посетителей. В отличие от [PPR](#partial-prerendering-ppr-cache-components), весь маршрут (или динамическое поддерево) рендерится в момент запроса.

**When to use:** Авторизованные дашборды, корзина, админки, данные в реальном времени — всё, что должно отражать текущего пользователя или момент.

**Pluses and minuses:**

| +                         | −                                      |
| ------------------------- | -------------------------------------- |
| Всегда свежие данные      | Более высокий TTFB, нагрузка на сервер |
| Персонализация по запросу | По умолчанию нет кэша CDN для HTML     |

**Examples:**

Достаточно **одного** триггера (они суммируются):

| Trigger                                                                | Что означает                                |
| ---------------------------------------------------------------------- | ------------------------------------------- |
| `await cookies()` / `await headers()` / чтение `searchParams` в `page` | Нужны данные конкретного запроса            |
| `fetch(..., { cache: "no-store" })`                                    | Данные не должны кэшироваться               |
| `export const dynamic = "force-dynamic"`                               | Явный «всегда SSR»                          |
| `await connection()`                                                   | WebSockets, SSE или другие request-only API |

```tsx
// app/dashboard/page.tsx — SSR: HTML перегенерируется на каждый запрос
import { cookies } from "next/headers";

export const dynamic = "force-dynamic"; // опционально — cookies() уже включает SSR

export default async function Dashboard() {
  // Шаг 1: request API → маршрут динамический → Full Route Cache пропускается
  const session = (await cookies()).get("session")?.value;

  // Шаг 2: no-store → Data Cache тоже пропускается
  const stats = await fetch("https://api.example.com/stats", {
    cache: "no-store",
  }).then((r) => r.json());

  return (
    <div>
      <h1>Welcome, {session ?? "guest"}</h1>
      <p>Active users: {stats.active}</p>
    </div>
  );
}
```

**Request flow:**

```text
приходит запрос
  → Next.js видит cookies() / force-dynamic → пропуск Full Route Cache
  → сервер выполняет Dashboard + fetch (no-store)
  → свежий HTML + RSC payload в браузер
  → повторяется на каждом визите (без CDN HTML reuse)
```

**Thoughts:** SSR — противоположность SSG: нет переиспользования HTML из сборки. `cookies()` в `layout.tsx` делает динамическим **всё поддерево** — ограничивайте проверку auth листом, которому она нужна. Для в основном статичных страниц с маленьким персональным блоком лучше [PPR](#partial-prerendering-ppr-cache-components), а не SSR всего маршрута.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [connection](https://nextjs.org/docs/app/api-reference/functions/connection) · [route segment config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)

</details>

<details>
  <summary>Incremental Static Regeneration (ISR)</summary>

### Incremental Static Regeneration (ISR)

**Definition:** Incremental Static Regeneration (ISR) — это **SSG + обновление по таймеру или по требованию**. Страницы по-прежнему пререндерены и быстры с CDN, но Next.js **перегенерирует** их в фоне после истечения TTL — или сразу при вызове `revalidateTag` / `revalidatePath`.

Stale-while-revalidate: посетители видят старую страницу, пока сервер собирает новую в фоне.

**When to use:** Каталоги, новости, документация — общий контент, который меняется иногда, но не требует свежести на каждый запрос.

**Pluses and minuses:**

| +                                    | −                                                                       |
| ------------------------------------ | ----------------------------------------------------------------------- |
| CDN-быстро + eventual freshness      | Пользователи могут видеть устаревший контент во время окна revalidation |
| Масштабируется лучше, чем чистый SSR | Нужна стратегия срочного сброса кэша                                    |

**Examples:**

ISR = кэширование [SSG](#static-generation-ssg) **плюс** окно revalidation:

| Piece                                          | Роль при сборке / первом запросе      | Роль после истечения TTL                        |
| ---------------------------------------------- | ------------------------------------- | ----------------------------------------------- |
| `"use cache"` + `cacheLife({ revalidate: N })` | Хранит HTML + данные с TTL            | Отдаёт stale, регенерирует в фоне               |
| `cacheTag("products")`                         | Тег записи кэша                       | `revalidateTag` сбрасывает при публикации в CMS |
| `generateStaticParams()`                       | Пререндер известных URL (опционально) | Те же slug, свежее содержимое после revalidate  |

```tsx
// app/products/page.tsx — ISR через окно revalidate в cacheLife
// requires cacheComponents: true in next.config.ts
import { cacheLife, cacheTag } from "next/cache";

type Product = { id: string; name: string };

async function getProducts(): Promise<Product[]> {
  "use cache";
  cacheTag("products");
  cacheLife({ stale: 300, revalidate: 3600, expire: 86400 });
  // revalidate: 3600 → через 1 ч следующий запрос запускает фоновое обновление
  // stale: 300     → клиент 5 мин может показывать кэш без re-check

  const res = await fetch("https://api.example.com/products");
  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

```tsx
// app/products/actions.ts — on-demand ISR (без полной пересборки)
"use server";

import { revalidateTag } from "next/cache";

export async function addProduct() {
  await db.product.create({ data: { name: "New item" } });
  revalidateTag("products", "max"); // сброс по тегу → следующий визит перегенерирует
}
```

**Request flow:**

```text
первая сборка / первый запрос
  → getProducts() выполняется → в Full Route Cache + Data Cache
в пределах revalidate (например 1 ч)
  → мгновенная отдача кэшированного HTML (CDN-fast)
после истечения revalidate
  → посетитель A получает STALE страницу сразу
  → сервер регенерирует в фоне
  → посетитель B получает свежую версию
срочная публикация в CMS
  → revalidateTag("products") → не ждём TTL
```

**Thoughts:** ISR между SSG (заморожено до rebuild) и SSR (всегда свежо). Один `cacheLife("days")` ближе к чистому SSG; добавьте `revalidate` или `revalidateTag`, если контент должен обновляться чаще. Слои кэша: [Next.js — Data § Caching model](./4_nextjs_data.md#caching-model-overview).

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [cacheLife](https://nextjs.org/docs/app/api-reference/functions/cacheLife) · [revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag)

</details>

<details>
  <summary>Partial Prerendering (PPR) / Cache Components</summary>

### Partial Prerendering (PPR) / Cache Components

**Definition:** Partial Prerendering (PPR) — через **Cache Components** (`cacheComponents: true`) — делит один маршрут на две зоны **на одной странице**:

- **Static shell** — пререндер при сборке, CDN-fast (`"use cache"` + длинный `cacheLife`)
- **Dynamic holes** — на каждый запрос внутри `<Suspense>` (`cookies()`, короткий `cacheLife` или uncached `fetch`)

Пользователь быстро видит оболочку, пока персональные или live-данные стримятся.

**When to use:** Страницы с преимущественно публичным контентом + небольшие персональные блоки (шапка, badge корзины, «Welcome, {name}»).

**Pluses and minuses:**

| +                                             | −                                  |
| --------------------------------------------- | ---------------------------------- |
| Лучшее из статики и динамики в одном маршруте | Требует дисциплины `use cache`     |
| Быстрый LCP из static shell                   | Новый API — нужно обучение команды |

**Examples:**

| Piece                                | При сборке (`next build`) | При запросе              |
| ------------------------------------ | ------------------------- | ------------------------ |
| `cacheComponents: true`              | Включает модель PPR       | —                        |
| `"use cache"` + `cacheLife("hours")` | Входит в **static shell** | Отдаётся из кэша         |
| `cookies()` внутри `<Suspense>`      | Остаётся **dynamic hole** | Стримит реальный контент |
| `<Suspense fallback>`                | Fallback в оболочке       | Пока hole не разрешился  |

```tsx
// next.config.ts — шаг 0: включить Cache Components (PPR)
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  cacheComponents: true,
};

export default nextConfig;
```

```tsx
// app/dashboard/page.tsx
import { Suspense } from "react";
import { cacheLife, cacheTag } from "next/cache";
import { cookies } from "next/headers";

// Static shell — кэш при сборке, длинный cacheLife → в пререндере
async function ProductsList() {
  "use cache";
  cacheTag("products-list");
  cacheLife("hours");

  const products = await fetch("https://api.example.com/products").then((r) =>
    r.json(),
  );
  return (
    <ul>
      {products.map((p: { id: string; name: string }) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}

// Dynamic hole — cookies() нельзя внутри "use cache"
async function Greeting() {
  const user = (await cookies()).get("user")?.value;
  return <p>Welcome back, {user ?? "guest"}</p>;
}

export default function Page() {
  return (
    <section>
      <h1>Dashboard</h1> {/* static shell */}
      <ProductsList /> {/* static shell */}
      <Suspense fallback={<p>Loading…</p>}>
        <Greeting /> {/* dynamic hole — стрим при запросе */}
      </Suspense>
    </section>
  );
}
```

**Build + request flow:**

```text
next build
  → <h1> + ProductsList в static shell
  → слот Greeting сохранён как dynamic hole (граница Suspense)
запрос
  → CDN отдаёт static shell сразу (быстрый LCP)
  → браузер показывает <h1>, список и fallback "Loading…"
  → Greeting разрешается (cookies()) → стримится в hole
  → пользователь видит "Welcome back, alice"
```

**Thoughts:** PPR — не третий режим сам по себе, а **сочетание** [SSG](#static-generation-ssg) shell и [SSR](#server-side-rendering-ssr) holes. Без `<Suspense>` вызов `cookies()` может сделать динамическим **весь** маршрут. Оборачивайте каждый request-time блок. См. также [Streaming SSR](#streaming-ssr).

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache) · [vercel.com/docs/partial-prerendering](https://vercel.com/docs/partial-prerendering)

</details>

<details>
  <summary>Combining rendering strategies</summary>

### Combining rendering strategies

**Definition:** В реальных приложениях редко используют один режим рендеринга везде. Next.js позволяет **смешивать SSG, ISR, SSR, PPR и CSR** на трёх уровнях:

| Уровень                       | Что различается                                  | Типичный паттерн                                                 |
| ----------------------------- | ------------------------------------------------ | ---------------------------------------------------------------- |
| **App (дерево маршрутов)**    | Каждый сегмент URL выбирает свой режим           | Marketing = SSG, blog = ISR, dashboard = SSR                     |
| **Page (один URL)**           | Static shell + dynamic holes на одном маршруте   | [PPR](#partial-prerendering-ppr-cache-components) с `<Suspense>` |
| **Component (одна страница)** | Серверный HTML для общих данных + client islands | Server Component fetch + `"use client"` leaves                   |

Цель: **CDN-fast и SEO там, где можно**, свежесть или персонализация — только где нужно.

**When to use:** Любое production-приложение с публичными страницами, полустатичным контентом и авторизованными/персональными зонами.

**Pluses and minuses:**

| +                                                     | −                                                              |
| ----------------------------------------------------- | -------------------------------------------------------------- |
| Оптимизация стоимости и производительности по секциям | Больше границ для проектирования и тестов                      |
| Один кодбаз, смешанные стратегии                      | Легко «протечь» `cookies()` в layout и динамифицировать лишнее |

**Examples:**

Три паттерна, которые на практике часто сочетаются в одном приложении:

| Паттерн                 | Где                    | Статическая часть   | Динамическая часть             |
| ----------------------- | ---------------------- | ------------------- | ------------------------------ |
| **1. Route split**      | Разные папки в `app/`  | `/about`, `/blog`   | `/dashboard`                   |
| **2. Page split (PPR)** | Один `page.tsx`        | блоки `"use cache"` | `<Suspense>` + `cookies()`     |
| **3. Component split**  | Один Server `page.tsx` | Server fetch + HTML | Client leaves (`"use client"`) |

```text
app/
├── (marketing)/
│   └── about/page.tsx          → SSG  (use cache + cacheLife("max"))
├── blog/
│   └── [slug]/page.tsx         → ISR  (use cache + revalidate window)
├── dashboard/
│   └── page.tsx                → SSR  (cookies / force-dynamic)
└── page.tsx                    → PPR  (static shell + Suspense hole)
```

**1. Route-level hybrid** — каждый сегмент выбирает свой режим:

```tsx
// app/(marketing)/about/page.tsx — SSG
import { cacheLife } from "next/cache";

async function getAbout() {
  "use cache";
  cacheLife("max");
  return fetch("https://api.example.com/about").then((r) => r.json());
}

export default async function AboutPage() {
  const data = await getAbout();
  return <main>{data.title}</main>;
}
```

```tsx
// app/dashboard/page.tsx — SSR (только это поддерево — per-request)
import { cookies } from "next/headers";

export const dynamic = "force-dynamic";

export default async function DashboardPage() {
  const session = (await cookies()).get("session")?.value;
  return <h1>Welcome, {session ?? "guest"}</h1>;
}
```

**2. Page-level hybrid (PPR)** — статика + динамика на **одном URL** (см. [PPR](#partial-prerendering-ppr-cache-components)):

```tsx
// app/page.tsx — requires cacheComponents: true
import { Suspense } from "react";
import { cacheLife, cacheTag } from "next/cache";
import { cookies } from "next/headers";

async function PublicFeed() {
  "use cache";
  cacheTag("feed");
  cacheLife("hours");
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return (
    <ul>
      {posts.map((p: { id: string; title: string }) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}

async function UserBadge() {
  const name = (await cookies()).get("user")?.value;
  return <span>{name ?? "Sign in"}</span>;
}

export default function HomePage() {
  return (
    <main>
      <header>
        <Suspense fallback={<span>…</span>}>
          <UserBadge />
        </Suspense>
      </header>
      <PublicFeed />
    </main>
  );
}
```

**3. Component-level hybrid** — серверный HTML + client islands (без `cookies()` на странице):

```tsx
// app/posts/page.tsx — Server Component: SSG/ISR data
import { cacheLife, cacheTag } from "next/cache";
import { LikeButton } from "./like-button";

async function getPosts() {
  "use cache";
  cacheTag("posts");
  cacheLife({ revalidate: 3600 });
  return fetch("https://api.example.com/posts").then((r) => r.json());
}

export default async function PostsPage() {
  const posts = await getPosts();
  return (
    <ul>
      {posts.map((p: { id: string; title: string }) => (
        <li key={p.id}>
          {p.title}
          <LikeButton postId={p.id} /> {/* CSR island — hooks/events */}
        </li>
      ))}
    </ul>
  );
}
```

```tsx
// app/posts/like-button.tsx — только CSR leaf
"use client";

import { useState } from "react";

export function LikeButton({ postId }: { postId: string }) {
  const [liked, setLiked] = useState(false);
  return (
    <button onClick={() => setLiked((v) => !v)}>
      {liked ? "♥" : "♡"} {postId}
    </button>
  );
}
```

**Decision flow:**

```text
Контент одинаков для всех посетителей?
  да → SSG или ISR (use cache + cacheLife)
  нет → Нужны request-данные только в маленьком блоке?
          да → PPR: остальное в кэше, Suspense + cookies() для hole
          нет → SSR для всего маршрута (или CSR fetch в client leaf)
```

**Thoughts:** Выбирайте **минимальную динамическую границу** — Client leaf или Suspense hole лучше, чем `force-dynamic` на всём приложении. Держите `cookies()` вне root `layout.tsx`, если не каждая страница это требует. Route groups `(marketing)` / `(app)` помогают организовать hybrid-дерево без смены URL.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache) · [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Route segment config</summary>

### Route segment config

**Definition:**

Экспорт на уровне маршрута в `layout.tsx`, `page.tsx`, or `route.ts` которые переопределяют настройки Next.js по умолчанию для кэширования, рендеринга и среды выполнения для этого сегмента и его дочерних элементов (если дочерний элемент не переопределяет).

| Экспорт           | Ценности                                                                               | Эффект                                                                         |
| ----------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| `dynamic`         | `'auto'`, `'force-static'`, `'force-dynamic'`, `'error'`                               | Статический и динамический рендеринг для сегмента                              |
| `dynamicParams`   | `true` (по умолчанию), `false`                                                         | Разрешить генерацию по требованию невидимых динамических параметров вместо 404 |
| `revalidate`      | `false`, `0`, `number` (секунды)                                                       | Интервал ISR для сегмента                                                      |
| `fetchCache`      | `'auto'`, `'default-no-store'`, `'only-no-store'`, `'force-no-store'`, `'force-cache'` | Переопределить значение по умолчанию `fetch` кэширование в сегменте            |
| `runtime`         | `'nodejs'` (по умолчанию), `'edge'`                                                    | Среда выполнения серверных компонентов и обработчиков маршрутов                |
| `preferredRegion` | строка или `string[]`                                                                  | Закрепить бессерверное выполнение за регионами                                 |
| `maxDuration`     | число (секунды)                                                                        | Ограничение тайм-аута бессерверной функции                                     |

**When to use:**

- `export const revalidate = 60` — контент, который периодически меняется (блог, каталог)
- `export const dynamic = 'force-static'` — юридические/маркетинговые страницы, которые никогда не должны читать данные запроса
- `export const dynamicParams = false` — только слизни из `generateStaticParams` действительны
- `export const runtime = 'edge'` — обработчик маршрутов с низкой задержкой на границе (см. [Edge vs Node](./4_nextjs_platform.md#edge-vs-node-runtime))

**When not to use:**

- `force-dynamic` в корне `layout.tsx` когда только одному листу нужны файлы cookie — вместо этого читайте API запроса на этом листе
- `fetchCache = 'force-no-store'` когда `use cache` + `revalidateTag` уже модели недействительны

**Pluses and minuses:**

| +                                            | −                                                          |
| -------------------------------------------- | ---------------------------------------------------------- |
| Хирургический контроль по дереву маршрутов   | Легко накрыть одеялом `force-dynamic` и уничтожить кеш CDN |
| Дети наследуют родительскую конфигурацию     | Может бороться с декларативностью `use cache` границы      |
| Нет необходимости оборачивать каждый `fetch` | Некоторые экспорты устарели или сужены в Next.js 16        |

**Examples:**

```tsx
// app/blog/[slug]/page.tsx
export const revalidate = 3600;
export const dynamicParams = true;

export async function generateStaticParams() {
  const slugs = await getSlugs();
  return slugs.map((slug) => ({ slug }));
}

export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const post = await getPost(slug);
  return <article>{post.title}</article>;
}
```

```tsx
// app/dashboard/layout.tsx — authenticated subtree
export const dynamic = "force-dynamic";

export default async function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const session = await getSession();
  if (!session) redirect("/login");
  return <>{children}</>;
}
```

**Thoughts:**

- Чтение `cookies()`, `headers()`, or `connection()` в серверном компоненте выбирает маршрут для **динамического рендеринга**, если только PPR/`use cache` предоставляет статическую оболочку.
- `dynamic = 'error'` — сборка завершается сбоем, если что-либо делает сегмент динамическим (защитное ограждение для маршрутов, которые должны быть статическими).
- Next.js 16: для ISR предпочтительнее `use cache` + `cacheLife` / `cacheTag`; `export const revalidate` всё ещё работает, но это legacy-настройка на уровне сегмента.
- Предпочитайте декларативное кэширование вместо blanket `force-dynamic` на layout.
- Pages Router: `getStaticProps` + `revalidate` → `cacheLife({ revalidate: N })` или `revalidate` export; `getServerSideProps` → dynamic через request APIs.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/route-segment-config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)

</details>

<a name="2-server-client-boundaries"></a>

## 2. Server & Client boundaries

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Server Components</summary>

### Server Components

**Definition:** Компоненты в `app/` — **серверные по умолчанию**; директива не нужна, пока не включаете клиент. Они выполняются на сервере, могут быть `async`, обращаться к БД/файловой системе и не отправляют JS компонента на клиент.

**How it works:**

- В файле нет `"use client"` — React выполняет его только на сервере
- Может быть `async` — `await` запросов к БД, `fetch` или чтения файлов прямо в теле компонента
- Результат — **RSC payload** (и часто пререндеренный HTML); серверная логика не попадает в клиентский бандл
- Может импортировать **Client Components** как листья — передавайте сериализуемые props или `children` для интерактивности

**Not supported:**

- React-хуки (`useState`, `useEffect` и т. д.)
- Browser API (`window`, `document`, `localStorage`)
- Обработчики событий (`onClick`, `onChange`)

**When to use:** Получение данных, макеты, статический контент, передача данных в интерактивные листья.

**Pluses and minuses:**

| +                                                                         | −                                                          |
| ------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Нулевой клиентский JS для пользовательского интерфейса только для сервера | Нет `useState`, `useEffect`, обработчики событий           |
| Прямые секреты и доступ к БД                                              | Свойства клиентских компонентов должны быть сериализуемыми |
| Автоматическое разделение кода на границе импорта                         | Для отладки требуются журналы сервера                      |

**Examples:**

```tsx
// app/products/page.tsx — no "use client" directive
export default async function ProductsPage() {
  const products = await db.product.findMany();
  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Client Components</summary>

### Client Components

**Definition:** Файлы с `"use client"` наверху. Гидрат в браузере; поддержка перехватчиков, состояния и событий.

**When to use:** Кнопки, формы с мгновенной проверкой, диаграммы и многое другое. `useState` / `useEffect`.

**Pluses and minuses:**

| +                                             | −                                         |
| --------------------------------------------- | ----------------------------------------- |
| Полная интерактивность                        | Увеличивает размер клиентского пакета     |
| API браузера, сторонние клиентские библиотеки | Невозможно импортировать серверные модули |

**Examples:**

```tsx
"use client";

import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}
```

**Thoughts:** Толкать `"use client"` **уходит**, а не корни страниц. Одиночный `"use client"` наверху большого дерева извлекает весь импорт в клиентский пакет.

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Composition pattern</summary>

### Composition pattern

**Definition:** **Композиция** — способ связать Server и Client Components в App Router. Базовая форма: **родитель Server Component** загружает данные и рендерит разметку; **листья Client Component** отвечают за состояние, события и browser API. Server Components могут импортировать Client Components; Client Components **не могут** импортировать Server Components — передавайте серверный UI через `children` или другие props.

**How it works:**

| Паттерн                 | Направление                      | Правило                                                                                                                                              |
| ----------------------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Leaf import**         | Server → Client                  | Server `page`/`layout` импортирует компоненты с `"use client"`; props должны быть [сериализуемыми](#serialization-rules)                             |
| **Слот `children`**     | Server → Client                  | Клиентская оболочка (`Modal`, `Tabs`) получает Server Components как `children` — серверное дерево рендерится первым, затем передаётся через границу |
| **Context provider**    | Client оборачивает server-дерево | Provider — Client Component; Server `layout` оборачивает `{children}` — context работает только ниже provider                                        |
| **Обёртка third-party** | Server → тонкий Client re-export | Компонент библиотеки без `"use client"` получает однострочную Client-обёртку; Server-страницы импортируют обёртку                                    |

```text
app/posts/page.tsx          Server — fetch + разметка
  └── LikeButton.tsx        Client leaf — hooks / onClick
app/layout.tsx              Server
  └── ThemeProvider.tsx     Client — context; children остаются Server
      └── {children}
```

**When to use:**

- Странице нужны и серверные данные (БД, секреты, `fetch`), и интерактивные острова (формы, переключатели, графики)
- Клиентской оболочке нужен серверный контент внутри (`Modal` + `Cart`, `Tabs` + серверные панели)
- Клиентский context на всё приложение (тема, client store i18n) должен обернуть часть дерева

**When not to use:**

- Вся страница интерактивна — Client page с fetch на клиенте может быть проще (но теряются серверный доступ к данным и SEO по умолчанию)
- Передача огромных объектов в props — сначала маппинг в DTO до Client-границы
- Импорт Server Component внутри Client-файла — используйте `children` или опустите Client leaf ниже

**Pluses and minuses:**

| +                                                           | −                                                                     |
| ----------------------------------------------------------- | --------------------------------------------------------------------- |
| Минимальный клиентский бандл — гидратируются только листья  | Ментальная модель: два module graph, одна RSC-граница                 |
| Секреты и БД остаются на сервере                            | Props и слоты `children` должны соответствовать правилам сериализации |
| Серверный HTML + клиентская интерактивность на одном URL    | Легко случайно пометить большое поддерево `"use client"`              |
| Паттерн `children` делает Client-оболочки переиспользуемыми | Context providers добавляют client-границу над поддеревом             |

**Examples:**

**1. Leaf import** — серверная страница, клиентская кнопка:

```tsx
// app/posts/page.tsx — Server Component
import { LikeButton } from "./like-button";

export default async function PostsPage() {
  const posts = await getPosts();
  return (
    <ul>
      {posts.map((p) => (
        <li key={p.id}>
          {p.title}
          <LikeButton postId={p.id} initialLikes={p.likes} />
        </li>
      ))}
    </ul>
  );
}
```

```tsx
// app/posts/like-button.tsx — Client leaf
"use client";

import { useState } from "react";

export function LikeButton({
  postId,
  initialLikes,
}: {
  postId: string;
  initialLikes: number;
}) {
  const [likes, setLikes] = useState(initialLikes);
  return (
    <button onClick={() => setLikes((n) => n + 1)}>
      ♥ {likes} ({postId})
    </button>
  );
}
```

**2. Слот `children`** — клиентская оболочка, серверный контент внутри:

```tsx
// app/ui/modal.tsx — Client shell (state, focus trap, portal)
"use client";

import { useState } from "react";

export function Modal({ children }: { children: React.ReactNode }) {
  const [open, setOpen] = useState(false);
  if (!open) {
    return (
      <button type="button" onClick={() => setOpen(true)}>
        Open cart
      </button>
    );
  }
  return (
    <dialog open>
      {children}
      <button type="button" onClick={() => setOpen(false)}>
        Close
      </button>
    </dialog>
  );
}
```

```tsx
// app/cart/page.tsx — Server Component передаёт server UI как children
import { Modal } from "../ui/modal";
import { Cart } from "../ui/cart"; // Server — загружает корзину на сервере

export default function CartPage() {
  return (
    <Modal>
      <Cart />
    </Modal>
  );
}
```

**3. Context provider** — клиентский provider, server layout оборачивает children:

```tsx
// app/theme-provider.tsx
"use client";

import { createContext, useContext } from "react";

const ThemeContext = createContext<"light" | "dark">("light");
export const useTheme = () => useContext(ThemeContext);

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>;
}
```

```tsx
// app/layout.tsx — Server layout; provider оборачивает только {children}, не <html>
import { ThemeProvider } from "./theme-provider";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  );
}
```

**Thoughts:**

- `"use client"` относится к **module graph** — всё, что импортирует Client-файл, становится клиентским кодом. Server Components, переданные как `children`, **не** импортируются в этот graph; они рендерятся на сервере первыми.
- Держите providers **как можно глубже** — оборачивайте `{children}`, а не весь `<html>`, чтобы больше дерева оставалось server-only.
- Та же идея на уровне маршрута: [Combining rendering strategies](#combining-rendering-strategies) — серверный fetch + CSR-листья без `force-dynamic` на всей странице.
- Помечайте server-only хелперы `import "server-only"`, чтобы случайный Client-импорт падал на сборке.

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components) · [Serialization rules](#serialization-rules)

</details>

<details>
  <summary>Next.js 16 async request APIs</summary>

### Next.js 16 async request APIs

**Definition:** В Next.js 15+ все **request-time** входы стали **async**. Props маршрута (`params`, `searchParams`) приходят как `Promise`; `cookies()`, `headers()` и `draftMode()` из `next/headers` тоже возвращают Promise. В Server Components и Server Functions **`await` внутри `async`** — обязательный паттерн. Синхронный доступ ещё работает для миграции, но deprecated — в Next.js 16 считайте API **только async**.

**How it works:**

| API            | Где                                                  | Server Component                          | Client Component                                             |
| -------------- | ---------------------------------------------------- | ----------------------------------------- | ------------------------------------------------------------ |
| `params`       | `page`, `layout`, `generateMetadata`, Route Handlers | `const { slug } = await params`           | `useParams()` или `use(params)` на prop страницы             |
| `searchParams` | только `page`                                        | `const { q } = await searchParams`        | `useSearchParams()` или `use(searchParams)` на prop страницы |
| `cookies()`    | Server Components, Actions, Route Handlers           | `const store = await cookies()`           | Недоступно — используйте Server Actions                      |
| `headers()`    | Server Components, Actions, Route Handlers           | `const list = await headers()`            | Недоступно                                                   |
| `draftMode()`  | Server Components, Actions, Route Handlers           | `const { isEnabled } = await draftMode()` | Недоступно                                                   |

```text
Запрос приходит
  → Next.js отдаёт params / searchParams / cookies / headers как Promise
  → Server Component делает await внутри async-функции
  → Значения известны только в runtime запроса → маршрут может стать dynamic
```

**When to use:**

- Динамические маршруты — `app/blog/[slug]/page.tsx` требует `await params`
- Фильтры, пагинация, сортировка — `await searchParams` на listing-страницах
- Auth/session — `await cookies()` перед защищённым UI (подробнее: [cookies() & headers()](#cookies-headers))
- CMS preview — `await draftMode()` для обхода static cache
- Типизированные маршруты — `PageProps<'/blog/[slug]'>` после `next dev` / `next build`

**When not to use:**

- Внутри тел `"use cache"`, где данные должны оставаться static — request API включают dynamic rendering
- В Client Components через `next/headers` — ошибка сборки; читайте request на сервере
- `searchParams` в root `layout`, если нужен только одному дочернему маршруту — ограничьте сегмент

**Pluses and minuses:**

| +                                                    | −                                                                     |
| ---------------------------------------------------- | --------------------------------------------------------------------- |
| Ясная граница: request data всегда async             | Breaking change относительно sync props в Next.js 14                  |
| Единообразно в pages, layouts, metadata, handlers    | Пропущенный `await` даёт тонкие баги или ошибки типов                 |
| `PageProps<'/route'>` даёт autocomplete для `params` | `searchParams` на page переводит **всю страницу** в dynamic rendering |
| Client pages читают promises через React `use()`     | Sync fallback уберут — мигрируйте заранее                             |

**Examples:**

**1. Server page** — все четыре request-входа:

```tsx
import { cookies, headers } from "next/headers";

export default async function Page({
  params,
  searchParams,
}: {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ q?: string; page?: string }>;
}) {
  const { id } = await params;
  const { q = "", page = "1" } = await searchParams;
  const session = (await cookies()).get("session")?.value;
  const country = (await headers()).get("x-vercel-ip-country") ?? "US";

  const item = await getItem(id, { q, page, session, country });
  return <div>{item.title}</div>;
}
```

**2. Layout + metadata** — `params` async не только в `page`:

```tsx
// app/blog/[slug]/layout.tsx
export default async function BlogLayout({
  children,
  params,
}: {
  children: React.ReactNode;
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  return (
    <section>
      <nav>Series: {slug}</nav>
      {children}
    </section>
  );
}
```

```tsx
// app/blog/[slug]/page.tsx
import type { Metadata } from "next";

export async function generateMetadata({
  params,
}: {
  params: Promise<{ slug: string }>;
}): Promise<Metadata> {
  const { slug } = await params;
  const post = await getPost(slug);
  return { title: post.title };
}
```

**3. Типизированный `PageProps`** — литерал маршрута из generated types:

```tsx
export default async function Page(props: PageProps<"/blog/[slug]">) {
  const { slug } = await props.params;
  const { tag } = await props.searchParams;
  return (
    <h1>
      {slug} — {tag}
    </h1>
  );
}
```

**4. Client page** — не может быть `async`; разворачивайте через React `use()`:

```tsx
"use client";

import { use } from "react";

export default function Page({
  params,
  searchParams,
}: {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ q?: string }>;
}) {
  const { id } = use(params);
  const { q } = use(searchParams);
  return (
    <p>
      ID: {id}, q: {q}
    </p>
  );
}
```

**5. Client leaf (не page)** — хуки вместо props:

```tsx
"use client";

import { useParams, useSearchParams } from "next/navigation";

export function Filters() {
  const { slug } = useParams<{ slug: string }>();
  const searchParams = useSearchParams();
  const q = searchParams.get("q");
  return (
    <span>
      {slug} — {q}
    </span>
  );
}
```

| Пример маршрута                  | URL             | `await params` даёт                  |
| -------------------------------- | --------------- | ------------------------------------ |
| `app/shop/[slug]/page.tsx`       | `/shop/hat`     | `{ slug: "hat" }`                    |
| `app/shop/[...slug]/page.tsx`    | `/shop/a/b`     | `{ slug: ["a", "b"] }`               |
| `app/shop/page.tsx` + `?a=1&b=2` | `/shop?a=1&b=2` | `searchParams`: `{ a: "1", b: "2" }` |

**Thoughts:**

- **Триггер dynamic rendering** — `searchParams`, `cookies()`, `headers()` и `connection()` переводят маршрут в dynamic на каждый запрос, если не изолированы PPR или static shell. См. [SSR](#server-side-rendering-ssr) и [cookies() & headers()](#cookies-headers).
- **Миграция с sync** — `npx @next/codemod@latest next-async-request-api .` для кодовых баз Next.js 14.
- **Не смешивайте паттерны** — в Server Components `await`; в Client leaves `useParams` / `useSearchParams`; на Client **pages** — `use(params)` / `use(searchParams)`.
- Preview/draft: [draftMode](./4_nextjs_data.md#draftmode) в теме Data.

**Sources:** [page.js — params & searchParams](https://nextjs.org/docs/app/api-reference/file-conventions/page) · [cookies](https://nextjs.org/docs/app/api-reference/functions/cookies) · [headers](https://nextjs.org/docs/app/api-reference/functions/headers) · [draftMode](https://nextjs.org/docs/app/api-reference/functions/draft-mode)

</details>

<details>
  <summary>cookies() & headers() in Server Components</summary>

<a name="cookies-headers"></a>

### cookies() & headers() in Server Components

**Definition:**

`cookies()` и `headers()` от `next/headers` прочитать входящий HTTP-запрос на сервере. В Next.js 15+ оба возвращают **Promises** и должны быть `await`ред. При их использовании маршрут переключается на **динамический рендеринг** (если не изолирован с помощью PPR/`use cache: private`).

| API         | Читать                            | Написать          | Типичное использование                                                        |
| ----------- | --------------------------------- | ----------------- | ----------------------------------------------------------------------------- |
| `cookies()` | `get`, `getAll`, `has`            | `set`, `delete`   | Токены сеанса, настройки, сегменты A/B                                        |
| `headers()` | `get`, `getAll`, `has`, `entries` | Только для чтения | носитель аутентификации, `user-agent`, гео, пользовательские заголовки прокси |

**When to use:**

- Проверка сеанса в серверном компоненте перед отрисовкой защищенного пользовательского интерфейса.

- Чтение `Accept-Language` or `Authorization` для персонализации на стороне сервера
- Установка файлов cookie аутентификации в действиях сервера после входа в систему (`cookies().set(...)`)

**When not to use:**

- Чтение файлов cookie в корневом макете только тогда, когда `/dashboard` требуется авторизация — область действия макета информационной панели
- Хранение больших полезных данных в файлах cookie (ограничение 4 КБ, отправляется при каждом запросе)

**Pluses and minuses:**

| +                                                                       | −                                                                                                              |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Никакого раскрытия конфиденциальных токенов клиенту                     | Вызывает динамический рендеринг при использовании в дереве страниц                                             |
| Работает с серверными компонентами, действиями, обработчиками маршрутов | Невозможно использовать в клиентских компонентах — используйте `document.cookie` экономно или действия сервера |
| `httpOnly` куки не используй `window`                                   | Чрезмерное чтение файлов cookie в макете влияет на все поддерево                                               |

**Examples:**

```tsx
import { cookies, headers } from "next/headers";
import { redirect } from "next/navigation";

export default async function AccountPage() {
  const cookieStore = await cookies();
  const session = cookieStore.get("session")?.value;

  if (!session) redirect("/login");

  const headersList = await headers();
  const country = headersList.get("x-vercel-ip-country") ?? "US";

  return <p>Welcome — region: {country}</p>;
}
```

```tsx
"use server";

import { cookies } from "next/headers";

export async function login(formData: FormData) {
  const token = await authenticate(formData);
  const cookieStore = await cookies();
  cookieStore.set("session", token, {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "lax",
    path: "/",
    maxAge: 60 * 60 * 24 * 7,
  });
}
```

**Thoughts:**

- Клиентские компоненты: используйте `useSearchParams()` / `useParams()` для состояния URL; никогда не импортируйте `next/headers` in `'use client'` файлы.
- `headers()` доступен только для чтения — установите заголовки ответов в обработчиках маршрутов (`NextResponse`) or `next.config` `headers`.
- Сопряжение с [Шаблонами аутентификации](./4_nextjs_platform.md#authentication-patterns) — проверка сеанса на уровне данных, а не только на прокси-сервере.

**Sources:** [nextjs.org/docs/app/api-reference/functions/cookies](https://nextjs.org/docs/app/api-reference/functions/cookies) · [headers](https://nextjs.org/docs/app/api-reference/functions/headers)

</details>

<details>
  <summary>RSC payload</summary>

### RSC payload

**Definition:**

**RSC payload** (React Server Component Payload) — компактное сериализованное представление отрендеренного дерева Server Components, которое Next.js отправляет с сервера в браузер. Это **не** HTML и не клиентский JavaScript — это **wire-формат Flight**, с помощью которого React согласует UI на клиенте.

| Часть                               | Назначение                                            |
| ----------------------------------- | ----------------------------------------------------- |
| Результат рендера Server Components | Структура UI и текст, которые должен показать браузер |
| Placeholder'ы Client Components     | Где монтируются интерактивные острова                 |
| Ссылки на client chunks             | Какие JS-бандлы скачать                               |
| Сериализуемые props                 | Данные Server → Client через границу                  |

**How it works:**

**На сервере:** Next.js рендерит сегменты маршрута (layouts + pages). Server Components превращаются в RSC payload; Client Components только **ссылаются** в payload (не выполняются на сервере). Вместе с кодом Client Components Next.js может также пререндерить HTML.

**Первый заход (браузер):**

1. **HTML** — быстрый неинтерактивный preview (FCP)
2. **RSC payload** — согласование server/client дерева на клиенте
3. **Client JS** — гидратация Client Components (подключение обработчиков)

**Клиентская навигация (`<Link>`):** Next.js prefetch/fetch свежего RSC payload для целевого маршрута. **Router Cache** хранит недавние payload в сессии; **Full Route Cache** — HTML + payload для статических маршрутов при сборке/revalidation. См. [модель кэширования](./4_nextjs_data.md#caching-model-overview).

**When to use:**

Понимание RSC payload помогает при отладке:

- запросов `?_rsc=` или ответов `text/x-component` в DevTools
- устаревшего UI после мутаций (`router.refresh()` перезапрашивает payload)
- большого TTFB из-за раздутых сериализованных props

**Pluses and minuses:**

| +                                                       | −                                                          |
| ------------------------------------------------------- | ---------------------------------------------------------- |
| Логика Server Components не попадает в клиентский бандл | Непрозрачный формат — сложнее инспектировать, чем JSON API |
| Меньше client JS, чем у классического SSR               | Ошибки props проявляются как ошибки сериализации           |
| Поддержка streaming и частичных обновлений              | Размер payload напрямую влияет на скорость навигации       |

**Examples:**

```tsx
// app/posts/[id]/page.tsx — Server Component
import { LikeButton } from "@/components/like-button";

export default async function PostPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  const post = await db.post.findUnique({ where: { id } });

  return (
    <article>
      <h1>{post.title}</h1> {/* → rendered output в RSC payload */}
      <LikeButton likes={post.likes} />{" "}
      {/* → placeholder + chunk ref + props */}
    </article>
  );
}
```

```
Network tab (client navigation):
  GET /posts/42?_rsc=abc123
  Content-Type: text/x-component   ← RSC payload (Flight)
```

**Thoughts:**

- Один маршрут при первом document request может вернуть **HTML + RSC payload**; при навигации часто приходит **только payload**.
- Обрезайте props — каждый байт payload уходит на клиент. Правила DTO: [Serialization rules](#serialization-rules).
- Общая теория RSC и гидратации: [4_react_server.md §8](./4_react_server.md#8-server-rendering--rsc). Безопасность — считайте props для Client публичными: [RSC security](./4_nextjs_platform.md#rsc-security).

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components) · [React Server Components](https://react.dev/reference/rsc/server-components)

</details>

<details>
  <summary>Serialization rules</summary>

### Serialization rules

**Definition:**

Реквизиты, передаваемые из Server Components в Client Components, пересекают **границу Flight RSC** — они должны быть JSON-сериализуемы. Next.js сериализует их в [RSC payload](#rsc-payload), отправляемый в браузер.

| Разрешено                                                      | Запрещено                                                           |
| -------------------------------------------------------------- | ------------------------------------------------------------------- |
| Примитивы (`string`, `number`, `boolean`, `null`)              | Функции (кроме ссылок на действия сервера)                          |
| Обычные объекты и массивы                                      | Экземпляры класса (`new Date()` — вместо этого передать строку ISO) |
| `undefined` (исключено из полезной нагрузки)                   | `Map`, `Set`, `Symbol`, `BigInt`                                    |
| Ссылки на функции действий сервера                             | Неисчислимые или круговые структуры                                 |
| Реагируйте на элементы из серверных компонентов как `children` | Узлы DOM, ссылки на серверные объекты                               |

**When to use:**

- Передача **DTO** (простых объектов) из выборки сервера в клиентский пользовательский интерфейс. `{ id, title, publishedAt: post.createdAt.toISOString() }`
  — Передача действий сервера в качестве реквизита компонентам формы клиента — `action={updateTodo}` без вызова внутренних кэшированных тел
- Используйте `children` composition — передайте `<ClientChart data={serialized} />` от Server parent

**When not to use:**

- Передача моделей ORM с методами/скрытыми полями — сначала сопоставьте их с простыми объектами
  — Передача обратных вызовов с сервера на клиент — вместо этого используйте действия сервера или обработчики на стороне клиента.

**Pluses and minuses:**

| +                                                   | −                                            |
| --------------------------------------------------- | -------------------------------------------- |
| Небольшие, предсказуемые полезные данные клиента    | Легко случайно пройти мимо огромных объектов |
| Очистить контракт сервера/клиента                   | `Date`/`Map` требуют ручного преобразования  |
| Действия сервера, которые можно считать реквизитами | Отладка ошибок сериализации непрозрачна      |

**Examples:**

```tsx
// app/posts/page.tsx — Server Component
import { LikeButton } from "./like-button";
import { toggleLike } from "./actions";

export default async function PostsPage() {
  const posts = await db.post.findMany();
  const serialized = posts.map((p) => ({
    id: p.id,
    title: p.title,
    createdAt: p.createdAt.toISOString(), // Date → string
  }));

  return (
    <ul>
      {serialized.map((p) => (
        <li key={p.id}>
          {p.title}
          <LikeButton postId={p.id} onLike={toggleLike} />
        </li>
      ))}
    </ul>
  );
}
```

```tsx
// WRONG — class instance won't serialize
<ClientCard user={await db.user.findUnique({ where: { id } })} />

// RIGHT — plain DTO
<ClientCard user={{ id: user.id, name: user.name }} />
```

**Thoughts:**

- Действия сервера, передаваемые как реквизиты, являются **ссылками** — не вызывайте их внутри. `'use cache'` кэшированные тела функций; передать ссылку.
- Размер [RSC payload](#rsc-payload) влияет на TTFB — обрезайте props до того, что клиент реально рендерит.
- `children` Шаблон слота позволяет избежать сериализации больших деревьев в качестве реквизита — сервер отображает статическую оболочку, клиент оборачивает интерактивные листья.

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<a name="3-streaming-suspense"></a>

## 3. Streaming & Suspense

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Streaming SSR</summary>

### Streaming SSR

**Definition:** Сервер отправляет HTML частями по мере разрешения асинхронных серверных компонентов. Медленные разделы не блокируют быстрые разделы.

**When to use:** Страницы с независимыми областями медленных/быстрых данных.

**Pluses and minuses:**

| +                                                  | −                                               |
| -------------------------------------------------- | ----------------------------------------------- |
| Улучшение TTFB и воспринимаемая производительность | Более сложное проектирование состояния загрузки |
| Естественное соответствие вложенным данным         | Отладка потоковых ответов становится сложнее    |

**Examples:**

```tsx
import { Suspense } from "react";

async function SlowData() {
  const data = await fetch("https://api.example.com/slow").then((r) =>
    r.json(),
  );
  return <div>{data.value}</div>;
}

export default function Page() {
  return (
    <div>
      <h1>Fast header</h1>
      <Suspense fallback={<p>Loading…</p>}>
        <SlowData />
      </Suspense>
    </div>
  );
}
```

**Sources:** [nextjs.org/docs/app/getting-started/linking-and-navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating)

</details>

<details>
  <summary>loading.tsx</summary>

### loading.tsx

**Definition:** Файл соглашения, который помещает сегмент в режим ожидания с мгновенным возвратом во время навигации.

**Pluses and minuses:**

| +                                              | −                                                                      |
| ---------------------------------------------- | ---------------------------------------------------------------------- |
| Нулевой шаблонный саспенс на каждом маршруте   | Грубозернистый — один пользовательский интерфейс загрузки на сегмент   |
| Мгновенная обратная связь по навигации клиента | Не заменяет компонентную приостановку для частичной потоковой передачи |

**Examples:**

```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <div className="skeleton">Loading dashboard…</div>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/loading](https://nextjs.org/docs/app/api-reference/file-conventions/loading)

</details>

<details>
  <summary>CSR bailout</summary>

### CSR bailout

**Definition:** Какие-то крючки(`useSearchParams`, `usePathname` в некоторых настройках) приводит к тому, что ближайшая граница приостановки выводится на клиентский рендеринг, если она не перенесена.

**When to use:** Оберните компоненты, используя `useSearchParams()` in `<Suspense>`.

**Examples:**

```tsx
import { Suspense } from "react";
import { SearchResults } from "./search-results";

export default function Page() {
  return (
    <Suspense fallback={<p>Loading search…</p>}>
      <SearchResults />
    </Suspense>
  );
}
```

**Sources:** [nextjs.org/docs/messages/missing-suspense-with-csr-bailout](https://nextjs.org/docs/messages/missing-suspense-with-csr-bailout)

</details>

<details>
  <summary>connection()</summary>

### connection()

**Definition:** API для отключения статического предварительного рендеринга маршрута — требуется для WebSockets, SSE или других функций, работающих только во время запроса.

**Examples:**

```tsx
import { connection } from "next/server";

export async function GET() {
  await connection();
  // WebSocket upgrade or dynamic handler
  return new Response("OK");
}
```

**Thoughts:**

См. [Архитектура React Suspense SSR] (https://github.com/reactwg/react-18/discussions/37) для получения информации о базовой конструкции потоковой передачи React 18.

---

**Sources:** [nextjs.org/docs/app/api-reference/functions/connection](https://nextjs.org/docs/app/api-reference/functions/connection)

</details>
