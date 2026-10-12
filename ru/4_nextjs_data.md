# Next.js — Data

Извлечение, кэширование, повторная проверка, действия сервера и обработчики маршрутов.

[ ← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [7. Data fetching](#7-data-fetching)
- [8. Caching & revalidation](#8-caching-revalidation)
- [9. Server Actions vs Route Handlers](#9-server-actions-vs-route-handlers)

---

## 7. Data fetching

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Server Component fetching</summary>

### Server Component fetching

**Definition:** Серверные компоненты извлекают данные напрямую — `async` функции, `fetch()` или клиенты баз данных. Нет `useEffect`-водопадов на клиенте.

**How it works:**

- `page.tsx` / `layout.tsx` помечают как `async` и `await` данных **до** возврата JSX. Код выполняется на сервере раз на запрос (или на сборке для static routes).
- **Мемоизация `fetch`** в одном render pass убирает дубли (один URL + опции).
- В RSC payload уходит только нужный минимум; в Client Components (`"use client"`) передаются сериализуемые props.
- Ошибки fetch часто попадают в `error.tsx` сегмента, если не обработать их в компоненте.
- **Чтение** — здесь по умолчанию; **мутации** — Server Actions или Route Handlers, не в render.

**When to use:**

- По умолчанию для всех **read** в App Router.
- Данные для **SEO**, первого кадра или auth до показа UI.
- Любые секреты (ключи API, URL БД) не должны попадать в браузер.

**Pluses and minuses:**

| +                                                              | −                                                          |
| -------------------------------------------------------------- | ---------------------------------------------------------- |
| Доступны данные о первой покраске                              | Тесная связь со средой выполнения сервера                  |
| Секреты остаются на сервере                                    | Ошибки появляются во время рендеринга                      |
| Автоматическая дедупликация идентичных `fetch` в одном запросе | ORM в бессерверной работе — наблюдайте за пулом соединений |

**Examples:**

```tsx
async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    next: { tags: ["products"] },
  });
  if (!res.ok) throw new Error("Failed to fetch products");
  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();
  return (
    <ul>
      {products.map((p: { id: string; name: string }) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** Сначала **источник** (SaaS, БД в приложении, отдельный сервис), затем **слой**. См. [Choosing a data fetch path](#choosing-a-data-fetch-path) · [Fetching from a remote API](#fetching-from-a-remote-api) · [Colocated backend in Next.js](#colocated-backend-in-nextjs) · [Fetching from a separate backend](#fetching-from-a-separate-backend).

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>Choosing a data fetch path</summary>

### Choosing a data fetch path

**Definition:** Как выбрать **где лежат данные** и **какой слой Next.js** их загружает — до выбора SWR, Prisma или Route Handler.

**How it works:**

| Вопрос                                  | Если да →          | Паттерн                                                     |
| --------------------------------------- | ------------------ | ----------------------------------------------------------- |
| Данные у **вендора** (CMS, Stripe)?     | Удалённый HTTP API | `fetch`/SDK в **Server Component**, секреты в env           |
| Данные в **вашей БД** в том же деплое?  | Colocated backend  | `lib/` + ORM в RSC, без self-`fetch` на `/api`              |
| Данные за **другим вашим сервисом**?    | Отдельный backend  | `fetch(API_URL)` на сервере; браузер — **Route Handler**    |
| UI **часто обновляется** без навигации? | Клиентское чтение  | `fetch('/api/...')`, SWR, TanStack Query в Client Component |
| Это **мутация** (форма)?                | Запись на сервер   | **Server Action** или `POST` Route Handler                  |

**Layer rules:** RSC — read при render; Route Handler — HTTP для браузера/webhook; Server Action — мутации, не read API; Client fetch — после hydration.

**When to use:** Новая страница, миграция с `getServerSideProps`, разделение monolith на Next + API.

**Thoughts:** Антипаттерн: RSC `fetch('http://localhost:3000/api/...')` на static build. Антипаттерн: `DATABASE_URL` в Client Component.

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend) · [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>Fetching from a remote API</summary>

### Fetching from a remote API

**Definition:** Чтение данных из **стороннего HTTP API** (headless CMS, платежи, публичный REST/GraphQL) в Server Components, Server Actions или Route Handlers — через `fetch` или SDK с ключом только на сервере.

**How it works:**

- Вызов на **сервере** при рендере; секреты в `process.env`, не в клиентском бандле.
- Нужен **абсолютный URL**; к GET через `fetch` применяется кэш Next.js (`cache`, `next.revalidate`, `next.tags`).
- При **SSG** удалённый API должен быть доступен на этапе сборки, иначе нужен dynamic rendering.
- Server Components **не** должны читать данные через свой `app/api/...` — лишний HTTP и проблемы на build ([Backend for Frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)).

**When to use:**

- Headless **CMS** (Sanity, Contentful, Strapi).
- **Платежи**, доставка, каталоги (Stripe и т.д.) — ключи только на сервере.
- **Карты, валидация, ML** — когда канонические данные не у вас.
- Публичные **REST/GraphQL** без своего backend.

**Pluses and minuses:**

| +                                | −                                      |
| -------------------------------- | -------------------------------------- |
| Не нужен свой backend для чтения | Задержка и лимиты вендора              |
| Теги кэша для vendor GET         | Падение вендора ломает рендер страницы |
| Ключи API только на сервере      | Версии и схема API меняются снаружи    |

**Common pitfalls:**

- Вызов вендора из **браузера** — утечка ключей; только server proxy.
- Обёртка vendor GET в Route Handler **только** для RSC — лишний hop; вызывайте вендора из RSC.
- SSG при недоступном CMS — `force-dynamic`, `no-store` или ISR с fallback.
- Нет проверки `!res.ok` — Next не бросает на 404/500.

**Examples:**

```tsx
async function getPosts() {
  const res = await fetch("https://cms.example.com/api/posts", {
    headers: { Authorization: `Bearer ${process.env.CMS_TOKEN}` },
    next: { tags: ["cms-posts"], revalidate: 60 },
  });
  if (!res.ok) throw new Error("CMS unavailable");
  return res.json();
}
```

**Thoughts:** Связывайте `next.tags` с webhook CMS и `revalidateTag`, чтобы публикации без полного redeploy.

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data) · [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)

</details>

<details>
  <summary>Colocated backend in Next.js</summary>

### Colocated backend in Next.js

**Definition:** Данные в **том же приложении Next.js** — Postgres через Prisma/Drizzle, Redis, `lib/`. Server Components вызывают БД или доменные функции **напрямую**, без `fetch("http://localhost:3000/api/...")`.

**How it works:**

- Запросы в `lib/data.ts` или репозиториях; импорт в `async` Server Components.
- **Route Handlers** — для HTTP-клиентов (браузер, webhook), не для чтения Server Component через HTTP к самому себе.
- **Server Actions** — для **мутаций**, не как API чтения (очередь, последовательность).
- На serverless — пул соединений (pooler, serverless driver).

**When to use:**

- **Full-stack Next** — один репозиторий и деплой.
- **Postgres/SQLite** с Prisma, Drizzle, Kysely.
- **Сессия/auth** в layout до children.
- Внутренние админки в одном приложении.

**Pluses and minuses:**

| +                                                                 | −                                                 |
| ----------------------------------------------------------------- | ------------------------------------------------- |
| Быстрее — без self-HTTP                                           | Связка с runtime Next                             |
| ORM и транзакции в одном процессе                                 | server-only код нельзя тащить в Client Components |
| Для БД — `use cache` / `unstable_noStore`, не обязательно `fetch` | Позже API могут вынести в отдельный сервис        |

**Common pitfalls:**

- Импорт `lib/db` в Client Component.
- Новое соединение с БД на каждый запрос без pooler на serverless.
- Route Handlers как единственный слой данных + RSC fetch на `/api` — медленно и ломает SSG.
- Неверный кэш ORM — `use cache`, `unstable_noStore`, не `fetch` cache.

**Examples:**

```tsx
// lib/products.ts
import { db } from "@/lib/db";

export async function listProducts() {
  return db.product.findMany({ orderBy: { createdAt: "desc" } });
}

// app/products/page.tsx
import { listProducts } from "@/lib/products";

export default async function ProductsPage() {
  const products = await listProducts();
  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** Доменные модули (`lib/orders.ts`) упростят будущий вынос API. Polling после hydration — [Client-side fetching](#client-side-fetching) и [Route Handlers](#route-handlers).

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend) · [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)

</details>

<details>
  <summary>Fetching from a separate backend</summary>

### Fetching from a separate backend

**Definition:** UI в Next.js, а **данные** — с **другого вашего сервиса** (Express, Nest, Go, legacy) по HTTP.

**How it works:**

- Server Components или Route Handlers делают `fetch` на `API_URL` с серверной авторизацией (service token, JWT).
- Те же правила абсолютных URL, таймаутов и кэша, что для внешних API.
- **Client Components** — через Next Route Handler или публичный API, без service token в браузере.
- CORS важен для браузера; server-to-server `fetch` CORS не ограничивает.

**When to use:**

- **Legacy REST** уже в проде; Next — новый frontend.
- **Микросервисы** с отдельным деплоем.
- **Mobile + web** на одном backend.
- **Polyglot** (Go/Java API + Next UI).

**Pluses and minuses:**

| +                            | −                                     |
| ---------------------------- | ------------------------------------- |
| Независимые деплой и масштаб | Сеть и отказы                         |
| Чёткая граница сервиса       | Мост сессии/auth                      |
| Next как UI + BFF            | Нужны контракты и версионирование API |

**Common pitfalls:**

- Service token в Client Component или `NEXT_PUBLIC_*`.
- Вызов internal API из браузера без BFF — CORS и утечки.
- Нет timeout/retry на server `fetch` — зависший render.
- Ожидание, что cookie Next автоматически auth на backend — обычно нужен обмен токеном.

**Examples:**

```tsx
async function getOrders(userId: string) {
  const res = await fetch(`${process.env.API_URL}/v1/users/${userId}/orders`, {
    headers: { Authorization: `Bearer ${process.env.API_SERVICE_TOKEN}` },
    cache: "no-store",
  });
  if (!res.ok) throw new Error("Orders API error");
  return res.json();
}
```

```tsx
// app/api/orders/route.ts
import { auth } from "@/lib/auth";

export async function GET() {
  const session = await auth();
  if (!session) return new Response("Unauthorized", { status: 401 });
  const orders = await getOrders(session.user.id);
  return Response.json(orders);
}
```

**Thoughts:** Next часто играет **BFF**: сессия, service auth, DTO для UI. Сравните с [Colocated backend in Next.js](#colocated-backend-in-nextjs). Мутации — один вызов из Server Action, без лишних hop client → Next → backend.

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend) · [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>fetch cache options</summary>

### fetch cache options

**Definition:**

Next.js расширяет `fetch` с семантикой кэширования, которая соответствует SSG/SSR/ISR.

| Вариант                               | Поведение                               | Pages Эквивалент маршрутизатора  |
| ------------------------------------- | --------------------------------------- | -------------------------------- |
| `cache: 'force-cache'` (по умолчанию) | Кэш до аннулирования                    | `getStaticProps`                 |
| `cache: 'no-store'`                   | Получить каждый запрос                  | `getServerSideProps`             |
| `next: { revalidate: N }`             | ISR — повторная проверка через N секунд | `revalidate` in `getStaticProps` |
| `next: { tags: ['tag'] }`             | Тег для аннулирования по требованию     | Н/Д                              |

**Examples:**

```tsx
const staticData = await fetch(url, { cache: "force-cache" });
const dynamicData = await fetch(url, { cache: "no-store" });
const revalidatedData = await fetch(url, { next: { revalidate: 10 } });
```

**Thoughts:** `fetch` кэширование применяется к запросам **GET**, расширенным Next.js. Использование прямых вызовов БД `use cache` or `unstable_noStore()` для контроля кэша.

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching)

</details>

<details>
  <summary>Avoiding waterfalls</summary>

### Avoiding waterfalls

**Definition:** Последовательный `await` в одном компоненте создается водопад. Распараллелить независимые выборки.

**Examples:**

```tsx
export default async function Page() {
  const [user, posts] = await Promise.all([getUser(), getPosts()]);
  return <Dashboard user={user} posts={posts} />;
}
```

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>Client-side fetching</summary>

### Client-side fetching

**Definition:** Извлекайте клиентские компоненты через SWR или TanStack Query, когда данные должны обновляться без навигации.

Паттерны TanStack Query: SSR prefetch, `HydrationBoundary`, persistent cache — [React — TanStack](./4_react_tanstack.md#ssr-prefetch--hydration).

**When to use:** Бесконечная прокрутка, виджеты в реальном времени, персонализация после гидратации.

**Pluses and minuses:**

| +                                                      | −                                               |
| ------------------------------------------------------ | ----------------------------------------------- |
| Постоянные обновления, опросы, оптимистичный интерфейс | Клиентский водопад при чрезмерном использовании |
| Работает с любым клиентом REST/GraphQL                 | Худшее SEO для критического контента            |

**Examples:**

```tsx
"use client";

import useSWR from "swr";

export function LiveMetrics() {
  const { data, isLoading } = useSWR(
    "/api/metrics",
    (url) => fetch(url).then((r) => r.json()),
    {
      refreshInterval: 5000,
    },
  );
  if (isLoading) return <p>Loading…</p>;
  return <div>{data.value}</div>;
}
```

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>draftMode</summary>

<a name="draftmode"></a>

### draftMode

**Definition:** `draftMode()` от `next/headers` включает рендеринг **Предварительный просмотр/Черновик** — обходит статическую генерацию и извлекает некэшированный черновой контент из CMS. Асинхронность в Next.js 15+.

**When to use:**

Contentful, Sanity, Strapi или пользовательские URL-адреса предварительного просмотра CMS для редакторов.

Регулярный предварительный просмотр приложений с проверкой подлинности — вместо этого используйте аутентификацию сеанса + динамический рендеринг.

**Pluses and minuses:**

| +                                                           | −                                                              |
| ----------------------------------------------------------- | -------------------------------------------------------------- |
| Редакторы просматривают неопубликованный контент            | Необходимо защитить конечную точку включения с помощью секрета |
| Работает с любой безголовой CMS                             | Отключает статическую оптимизацию, пока она активна            |
| На основе файлов cookie — без специального клиентского кода | Легко забыть отключить во время производственных тестов        |

**Examples:**

```tsx
// app/api/draft/route.ts
import { draftMode } from "next/headers";
import { redirect } from "next/navigation";

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const secret = searchParams.get("secret");
  const slug = searchParams.get("slug");

  if (secret !== process.env.DRAFT_SECRET) {
    return new Response("Invalid token", { status: 401 });
  }

  const draft = await draftMode();
  draft.enable();
  redirect(slug ?? "/");
}
```

```tsx
// app/blog/[slug]/page.tsx
import { draftMode } from "next/headers";

export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const { isEnabled } = await draftMode();

  const post = isEnabled
    ? await fetchDraftPost(slug)
    : await fetchPublishedPost(slug);

  return <article>{post.body}</article>;
}
```

**Thoughts:**

- `draftMode().enable()` устанавливает `__prerender_bypass` печенье; `disable()` очищает это.
- Маршрут становится динамическим, пока активен режим черновика (ожидается для предварительного просмотра).
- Защищать `/api/draft` с `DRAFT_SECRET` — никогда не раскрывайте URL-адрес включения публично.

**Sources:** [nextjs.org/docs/app/api-reference/functions/draft-mode](https://nextjs.org/docs/app/api-reference/functions/draft-mode)

</details>

<a name="8-caching--revalidation"></a>

## 8. Caching & revalidation

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Caching model overview</summary>

### Caching model overview

**Definition:**

Next.js поддерживает **четыре уровня кэша** между вашим источником данных и браузером. Понимание того, какой уровень обслуживает ответ — и какой API аннулирования его очищает — является источником ошибок №1.

| Кэш                       | Что он хранит                                                  | Пожизненная                                       | Аннулирование                                         |
| ------------------------- | -------------------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------------- |
| **Запросить запоминание** | Дедупликация `fetch` / результаты функции за один рендер       | Единый запрос                                     | Автомат — новый запрос                                |
| **Кэш данных**            | Настойчивый `fetch` и `use cache` результаты                   | До повторной проверки TTL или по требованию       | `revalidateTag`, `revalidatePath`, `updateTag`        |
| **Полный кэш маршрутов**  | Визуализация HTML + RSC Полезная нагрузка для каждого маршрута | Сборка или до повторной проверки                  | `revalidatePath`, `revalidateTag`, `router.refresh()` |
| **Кэш маршрутизатора**    | Полезная нагрузка RSC при навигации **на стороне клиента**     | Сеанс (мягкий) / ~30 с по умолчанию (статический) | `router.refresh()`, полная перезагрузка страницы      |

**When to use:**

- Статический по умолчанию `fetch` — маркетинговые страницы, публичные каталоги
- `cache: 'no-store'` or `connection()` — информационные панели, данные по каждому пользователю
- `next: { revalidate: N }` — ISR-контент
- `use cache` + `cacheTag` — явные границы кэша в Next.js 16

**When not to use:**

- Предполагая `revalidatePath` мгновенно очищает кэш маршрутизатора клиента — в паре с `router.refresh()` в клиентских компонентах при необходимости
- В процессе `Map` кэши на бессерверных серверах — используйте `'use cache: remote'` или внешний магазин

**Pluses and minuses:**

| +                                                                             | −                                                                                            |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Огромный выигрыш в производительности для приложений с большим объемом чтения | Устаревший пользовательский интерфейс после мутаций, если вы забыли сделать недействительной |
| Запоминание запроса предотвращает дублирование обращений к базе данных        | Кэш маршрутизатора может отображать старую полезную нагрузку RSC после навигации             |
| CDN-дружественные статические оболочки с PPR                                  | Ментальная модель сложнее, чем «всегда свежая ССР»                                           |

**Examples:**

```
Browser request
  → Full Route Cache hit? → return cached HTML/RSC
  → miss → render Server Components
      → Request memoization (same fetch twice = one call)
      → Data Cache hit? → return cached data
      → miss → run fetch / use cache → store in Data Cache
  → store in Full Route Cache (if static)
Client navigation (Link)
  → Router Cache hit? → instant RSC payload
  → miss → server fetch → update Router Cache
```

```tsx
"use server";

import { revalidateTag, revalidatePath } from "next/cache";

export async function updateProduct(id: string, data: FormData) {
  await db.product.update({
    where: { id },
    data: {
      /* ... */
    },
  });
  revalidateTag(`product-${id}`, "max"); // Data Cache
  revalidatePath("/products"); // Full Route Cache
}
```

**Thoughts:**

- **Статический по умолчанию** в App Router — бывшие SSR-страницы Pages Router могут удивить вас кэшированным выводом.
- `cookies()` / `headers()` / `searchParams` включить динамический рендеринг — обходит статический полный кэш маршрутов, если не используется оболочка PPR.
- `unstable_noStore()` / `connection()` — аварийные люки для полностью динамического чтения данных.
- См. [Распространенная ошибка: устаревший пользовательский интерфейс после мутации](#common-pitfall-stale-ui-after-mutation) и [Заголовки кэша Vercel CDN](#vercel-cdn-cache-headers).

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching)

</details>

<details>
  <summary>use cache directive</summary>

### use cache directive

**Definition:**

Директива Next.js 16, обозначающая тело функции или компонента как кэшируемое. Включает компоненты кэша и PPR.

| Вариант                | Область применения                                          |
| ---------------------- | ----------------------------------------------------------- |
| `'use cache'`          | Кэш компонентов/функций по умолчанию                        |
| `'use cache: private'` | Индивидуальный кеш (использует файлы cookie/заголовки)      |
| `'use cache: remote'`  | Кэш времени выполнения — общий для бессерверных экземпляров |

**Examples:**

```tsx
import { cacheLife, cacheTag } from "next/cache";

async function getProducts() {
  "use cache: remote";
  cacheTag("products-list");
  cacheLife({ expire: 3600 });

  const res = await fetch("https://api.example.com/products");
  return res.json();
}
```

```ts
// next.config.ts
const nextConfig = { cacheComponents: true };
export default nextConfig;
```

**Sources:** [nextjs.org/docs/app/api-reference/directives/use-cache](https://nextjs.org/docs/app/api-reference/directives/use-cache)

</details>

<details>
  <summary>revalidateTag</summary>

### revalidateTag

**Definition:** Аннулирование по требованию по тегу. В Next.js 16 в качестве второго аргумента требуется **профиль**.

**Examples:**

```tsx
"use server";

import { revalidateTag } from "next/cache";

export async function updateArticle(articleId: string) {
  await db.article.update({ where: { id: articleId }, data: {} });
  revalidateTag(`article-${articleId}`, "max");
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag)

</details>

<details>
  <summary>revalidatePath</summary>

### revalidatePath

**Definition:** Очищает полный кэш маршрутов для определенного пути.

**Examples:**

```tsx
"use server";

import { revalidatePath } from "next/cache";

export async function createPost(formData: FormData) {
  await db.post.create({ data: { title: formData.get("title") as string } });
  revalidatePath("/blog");
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/revalidatePath](https://nextjs.org/docs/app/api-reference/functions/revalidatePath)

</details>

<details>
  <summary>updateTag</summary>

### updateTag

**Definition:** Немедленно истекает срок действия тега кэша — семантика чтения и записи.

**Examples:**

```tsx
"use server";

import { updateTag } from "next/cache";

export async function updateProfile(userId: string, data: ProfileData) {
  await db.user.update({ where: { id: userId }, data });
  updateTag(`user-${userId}`);
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/updateTag](https://nextjs.org/docs/app/api-reference/functions/updateTag)

</details>

<details>
  <summary>router.refresh()</summary>

### router.refresh()

**Definition:** Клиентский API, который повторно извлекает серверные компоненты для текущего маршрута без полной перезагрузки страницы.

**Pluses and minuses:**

| +                           | −                                                             |
| --------------------------- | ------------------------------------------------------------- |
| Сохраняет состояние клиента | **Не** делает недействительным кэш данных сервера сам по себе |

**Examples:**

```tsx
"use client";

import { useRouter } from "next/navigation";

export function RefreshButton() {
  const router = useRouter();
  return <button onClick={() => router.refresh()}>Refresh</button>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/use-router](https://nextjs.org/docs/app/api-reference/functions/use-router)

</details>

<details>
  <summary>unstable_noStore</summary>

### unstable_noStore

**Definition:** Отключает серверный компонент или функцию от статического кэширования и предварительной обработки — принудительно выполняет каждый запрос в динамическом режиме.

**Pluses and minuses:**

| +                                                  | −                                                        |
| -------------------------------------------------- | -------------------------------------------------------- |
| Простой аварийный люк для динамических данных      | Отключает статическую оптимизацию для этого дерева       |
| Работает с прямыми вызовами БД (не только `fetch`) | Предпочитаю явное `cache: 'no-store'` когда это возможно |

**Examples:**

```tsx
import { unstable_noStore as noStore } from "next/cache";

export default async function LiveClock() {
  noStore();
  const now = await db.query("SELECT NOW() as time");
  return <time>{now}</time>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/unstable_noStore](https://nextjs.org/docs/app/api-reference/functions/unstable_noStore)

</details>

<details>
  <summary>Common pitfall: stale UI after mutation</summary>

### Common pitfall: stale UI after mutation

**Definition:**

Только звонки `router.refresh()` без `revalidatePath` / `revalidateTag` / `updateTag` часто оставляет кэшированные данные устаревшими.

| Симптом                                                | Исправить                                                                        |
| ------------------------------------------------------ | -------------------------------------------------------------------------------- |
| Список не обновляется после создания                   | `revalidatePath('/items')` or `revalidateTag('items', 'max')` в действии сервера |
| Пользователь видит старый профиль после редактирования | `updateTag('user-${id}')`                                                        |
| CDN обслуживает старый HTML                            | Проверять `Cache-Control` + `revalidatePath`                                     |

**Thoughts:** [Обсуждение кэширования № 54075] (https://github.com/vercel/next.js/discussions/54075). [Предварительный рендеринг и гидратация] (https://www.freecodecamp.org/news/what-are-pre-rendering-and-гидратация-in-web-dev/).

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching)

</details>

<details>
  <summary>Vercel CDN cache headers</summary>

### Vercel CDN cache headers

**Definition:** Заголовки кэша для каждого ответа независимо управляют слоями пограничного кэша браузера, CDN и Vercel.

**Examples:**

```ts
export async function GET() {
  return new Response("Cached response", {
    headers: {
      "Cache-Control": "max-age=10",
      "CDN-Cache-Control": "max-age=60",
      "Vercel-CDN-Cache-Control": "max-age=3600",
    },
  });
}
```

**Sources:** [vercel.com/docs/headers/cache-control-headers](https://vercel.com/docs/headers/cache-control-headers)

</details>

<details>
  <summary>Deprecated patterns (Next.js 16)</summary>

### Deprecated patterns (Next.js 16)

**Definition:**

API-интерфейсы, удаленные или измененные в Next.js 16, и их рекомендуемые замены.

| Устарело                                | Замена                                   |
| --------------------------------------- | ---------------------------------------- |
| `unstable_cache()`                      | `'use cache'` + `cacheTag` / `cacheLife` |
| `experimental.ppr`                      | `cacheComponents: true`                  |
| `revalidateTag(tag)` одиночный аргумент | `revalidateTag(tag, 'max')`              |
| `cacheHandler` (единственное число)     | `cacheHandlers` (множественное число)    |

---

**Sources:** [nextjs.org/docs/app/guides/upgrading/version-16](https://nextjs.org/docs/app/guides/upgrading/version-16)

</details>

<a name="9-server-actions-vs-route-handlers"></a>

## 9. Server Actions vs Route Handlers

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Server Actions</summary>

### Server Actions

**Definition:** Асинхронные функции, отмеченные значком `"use server"` вызываемый из клиентских компонентов или `<form action>`. Next.js сериализует их как запросы **POST** — мост RPC между браузером и сервером.

**When to use:** Отправка форм, мутации, внутренняя логика приложения.

**Pluses and minuses:**

| +                                        | −                                                          |
| ---------------------------------------- | ---------------------------------------------------------- |
| Совмещено с пользовательским интерфейсом | Не общедоступный HTTP API                                  |
| Прогрессивное улучшение                  | POST-только под капотом                                    |
| Типобезопасность с TypeScript            | Необходимо проверить все входные данные на стороне сервера |

**Examples:**

```tsx
// app/actions.ts
"use server";

import { revalidatePath } from "next/cache";

export async function createTodo(formData: FormData) {
  const title = formData.get("title") as string;
  await db.todo.create({ data: { title } });
  revalidatePath("/todos");
}
```

**Sources:** [nextjs.org/docs/app/getting-started/mutating-data](https://nextjs.org/docs/app/getting-started/mutating-data)

</details>

<details>
  <summary>Route Handlers</summary>

### Route Handlers

**Definition:** `route.ts` файлы, экспортирующие функции метода HTTP. Стандартный API веб-запросов/ответов.

**Pluses and minuses:**

| +                                                    | −                                                   |
| ---------------------------------------------------- | --------------------------------------------------- |
| Настоящий HTTP — веб-перехватчики, общедоступные API | Нет прогрессивного улучшения                        |
| Любой метод HTTP                                     | Не может сосуществовать с `page.tsx` в той же папке |

**Examples:**

```tsx
// app/api/webhook/route.ts
import { NextRequest } from "next/server";

export async function POST(request: NextRequest) {
  const body = await request.json();
  return Response.json({ received: true });
}
```

**Sources:** [nextjs.org/docs/app/getting-started/route-handlers](https://nextjs.org/docs/app/getting-started/route-handlers)

</details>

<details>
  <summary>Comparison table</summary>

### Comparison table

**Definition:**

Матрица решений для выбора действий сервера или обработчиков маршрутов на основе модели вызова и потребителей.

| | Действия сервера | Обработчики маршрутов |
| | -------------------------- | ------------------------- | ------------------------------------------------- |
| | **Призыв** | РПК / `<form action>` | HTTP к URL |
| | **Протокол** | ПОСТ (завернутый) | ПОЛУЧИТЬ, ОТПРАВИТЬ, ПОЛОЖИТЬ, ИСПРАВИТЬ, УДАЛИТЬ |
| | **Потребители** | Только приложение Next.js | Любой HTTP-клиент |
| | **Сценарий использования** | Мутации, формы | Вебхуки, общедоступные API |

**Thoughts:** Обработчики маршрутов присутствуют **только** в `app/`. Внешние системы используют обработчики маршрутов, а не действия сервера.

**Sources:** [nextjs.org/docs/app/getting-started/route-handlers](https://nextjs.org/docs/app/getting-started/route-handlers)

</details>

<details>
  <summary>CSRF with Server Actions</summary>

<a name="csrf-with-server-actions"></a>

### CSRF with Server Actions

**Definition:**

Действия сервера вызываются через **POST** с идентификатором действия, сгенерированным сервером. Next.js включает **встроенную защиту CSRF** — проверку заголовка источника и зашифрованные идентификаторы действий — поэтому вам не нужно вручную добавлять токены CSRF для сообщений формы того же происхождения.

| Защита                     | Как это работает                                                                       |
| -------------------------- | -------------------------------------------------------------------------------------- |
| **Проверка происхождения** | Запрос `Origin` должен соответствовать `Host` (или разрешенный хост развертывания)     |
| **Идентификатор действия** | Непрозрачный зашифрованный идентификатор связывает POST с правильным действием сервера |
| **Только POST**            | Действия нельзя вызвать через GET — нет тега img CSRF                                  |

**How it works:**

- Обработчики общедоступных маршрутов (`route.ts`) — реализовать токены CSRF или подписанные запросы.
- Встраивание вашего приложения в сторонние iframe — обзор `allowedOrigins` in `next.config`
- Аутентификация только для прокси/промежуточного программного обеспечения — по-прежнему проверяется в теле действия сервера (см. [RSC-безопасность](./4_nextjs_platform.md#rsc-security))

**When to use:** По умолчанию для всех изменений в приложении с помощью действий сервера и `<form action>`.

**Pluses and minuses:**

| +                                                      | −                                                                              |
| ------------------------------------------------------ | ------------------------------------------------------------------------------ |
| Нулевой шаблон для форм одного и того же происхождения | Вызовы действий сервера между источниками должны быть явными. `allowedOrigins` |
| Прогрессивное улучшение безопасно по умолчанию         | Обработчикам маршрутов по-прежнему нужна собственная стратегия CSRF            |
| Работает с `<form action={...}>`                       | Обычай `fetch` действия должны содержать правильные заголовки/тело             |

**Examples:**

```ts
// next.config.ts — allow preview domain to invoke actions
const nextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ["localhost:3000", "preview.example.com"],
    },
  },
};
```

```tsx
"use server";

import { revalidatePath } from "next/cache";
import { z } from "zod";

const schema = z.object({ title: z.string().min(1).max(200) });

export async function createTodo(formData: FormData) {
  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Invalid title" };

  const session = await getSession(); // always re-validate auth in action
  if (!session) throw new Error("Unauthorized");

  await db.todo.create({
    data: { title: parsed.data.title, userId: session.userId },
  });
  revalidatePath("/todos");
}
```

**Thoughts:**

- Встроенный CSRF **не** заменяет **авторизацию** — всегда проверяйте права пользователя внутри действия.
- Для обработчиков маршрутов, принимающих POST браузера, используйте файлы cookie двойной отправки, файлы cookie SameSite или промежуточное программное обеспечение CSRF платформы.
- См. [Веб-API — CSRF](./6_web_api_security.md#csrf-cross-site-request-forgery) и [Special — CSRF vs AuthZ](./7_special_security.md#csrf-cross-site-request-forgery).

**Sources:** [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)

</details>

<a name="10-file-based-routing"></a>
