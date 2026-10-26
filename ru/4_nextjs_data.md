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

| Вопрос                                  | Если да →          | Паттерн                                                        |
| --------------------------------------- | ------------------ | -------------------------------------------------------------- |
| Данные у **вендора** (CMS, Stripe)?     | Удалённый HTTP API | `fetch`/SDK в **Server Component**, секреты в env              |
| Данные в **вашей БД** в том же деплое?  | Colocated backend  | `lib/` + ORM в RSC, без self-`fetch` на `/api`                 |
| Данные за **другим вашим сервисом**?    | Отдельный backend  | `fetch(API_URL)` на сервере; браузер — **Route Handler**       |
| UI **часто обновляется** без навигации? | Клиентское чтение  | `fetch('/api/...')`, SWR, TanStack Query в Client Component    |
| Это **мутация** (форма)?                | Запись на сервер   | [Handling mutations in Next.js](#handling-mutations-in-nextjs) |

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
// File: lib/products.ts — только сервер (не импортировать из "use client")
import { db } from "@/lib/db";

export async function listProducts() {
  return db.product.findMany({ orderBy: { createdAt: "desc" } });
}

// File: app/products/page.tsx  →  URL: /products (чтение в RSC, без /api)
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
// File: lib/orders.ts — только сервер; вызов из RSC или Route Handlers
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
// File: app/api/orders/route.ts  →  URL: GET /api/orders
// Client: fetch("/api/orders") — без API_SERVICE_TOKEN в браузере
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

Паттерны SWR (`mutate`, опции ревалидации) → [SWR (stale-while-revalidate)](./4_react_ecosystem.md#swr-stale-while-revalidate). Карта уровней → [Revalidation and invalidation across the stack](#revalidation-and-invalidation-across-the-stack).

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
  <summary>Revalidation and invalidation across the stack</summary>

### Revalidation and invalidation across the stack

**Definition:** «Опыт кэширования» в проде обычно охватывает **несколько уровней**. Каждый кэширует своё и имеет свой API **ревалидации** или **инвалидации**. Частая ошибка — чинить не тот слой.

**How it works:**

| Уровень            | Что кэшируется                    | Ревалидация / инвалидация                                                                                      |
| ------------------ | --------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **HTTP / CDN**     | Тела ответов, статика             | `Cache-Control`, `ETag` / `304`, purge CDN — [HTTP caching](./6_web_api_http.md#http-caching)                  |
| **Next.js server** | Data Cache, Full Route Cache, RSC | `revalidateTag`, `revalidatePath`, `updateTag`, `router.refresh()` — ниже                                      |
| **SWR (client)**   | JSON в памяти по `key`            | `mutate`, focus/reconnect, `refreshInterval` — [SWR](./4_react_ecosystem.md#swr-stale-while-revalidate)        |
| **TanStack Query** | Кэш запросов по `queryKey`        | `invalidateQueries`, `onSuccess` мутации — [TanStack Query](./4_react_tanstack.md#working-with-tanstack-query) |

**When to use:**

- На собеседовании: «после сохранения данные устарели» — назовите **какой кэш** отдал старый UI и **какой API** обновили.
- Новый Next.js: серверные кэши и теги для общих чтений; SWR/TanStack — только для клиентских виджетов.

**Pluses and minuses:**

| +                           | −                                          |
| --------------------------- | ------------------------------------------ |
| Ясные слои упрощают отладку | Путают HTTP-директиву SWR и библиотеку SWR |

**Thoughts:** Route Handlers могут задавать [Vercel CDN cache headers](#vercel-cdn-cache-headers) для HTTP, а кэши App Router — для RSC/HTML; уровни **складываются**. См. [Common pitfall: stale UI after mutation](#common-pitfall-stale-ui-after-mutation).

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching) · [MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

</details>

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
  <summary>Handling mutations in Next.js</summary>

<a name="handling-mutations-in-nextjs"></a>

### Handling mutations in Next.js

**Definition:** Повторяемый способ **создать, обновить или удалить** данные в App Router: выбрать поверхность ([Server Actions](#server-actions) или [Route Handlers](#route-handlers)), выполнить одни и те же шаги на сервере, затем **обновить кэши**, чтобы списки и детали показывали новые данные.

**How it works:**

| Step                  | What to do                                                                                                                                    |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Choose surface** | Форма/кнопка в Next UI → **Server Action** (по умолчанию). Webhook, мобилка, публичный REST → **Route Handler**.                              |
| **2. Auth**           | Внутри action/handler: сессия и права (не только layout).                                                                                     |
| **3. Validate**       | `FormData` или JSON; ошибки полей (`useActionState`) или `4xx` (handlers).                                                                    |
| **4. Write**          | Один вызов: `lib/db`, ORM или один `fetch(API_URL)` — без длинных цепочек.                                                                    |
| **5. Revalidate**     | `revalidatePath` / `revalidateTag` / `updateTag` для страниц, откуда UI читает ([stale UI pitfall](#common-pitfall-stale-ui-after-mutation)). |
| **6. Respond**        | Сериализуемый state, `redirect()` или `Response.json` — см. **Examples**.                                                                     |

| Who calls the write?                         | Use                                                                   |
| -------------------------------------------- | --------------------------------------------------------------------- |
| Ваш `<form>` / кнопка в этом Next-приложении | [Server Actions](#server-actions)                                     |
| Stripe, GitHub, партнёрский `POST` на URL    | [Webhooks in Route Handlers](#webhooks-in-route-handlers)             |
| Browser `fetch` после hydration (дашборд)    | [BFF Route Handler](#bff-route-handler) или action                    |
| Server Component при первом paint            | **Без мутации** — чтение в render; запись только через action/handler |

**When to use:** Сохранение настроек, создание строки, удаление комментария, корзина.

**When not to use:** Загрузка для SSR ([Server Component fetching](#server-component-fetching)); фоновые задачи (очередь).

**Pluses and minuses:**

| +                                   | −                                      |
| ----------------------------------- | -------------------------------------- |
| Один чеклист для команды            | Легко пропустить шаг 5 — устаревший UI |
| Actions + revalidate для форм и RSC | У handlers свой CSRF для browser POST  |

**Examples:**

```tsx
// File: app/actions/todos.ts — типичный workflow Server Action (шаги 2–6)
"use server";

import { revalidatePath } from "next/cache";
import { auth } from "@/lib/auth";
import { z } from "zod";

const schema = z.object({ title: z.string().min(1).max(200) });

export async function createTodo(
  _prev: { error?: string },
  formData: FormData,
) {
  const session = await auth();
  if (!session?.user) throw new Error("Unauthorized");

  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Invalid title" };

  await db.todo.create({
    data: { title: parsed.data.title, userId: session.user.id },
  });

  revalidatePath("/todos");
  return { error: undefined };
}
```

```tsx
// File: app/todos/TodoForm.tsx — UI + useActionState ("use client")
"use client";

import { useActionState } from "react";
import { createTodo } from "@/app/actions/todos";

export function TodoForm() {
  const [state, action, pending] = useActionState(createTodo, {
    error: undefined,
  });

  return (
    <form action={action}>
      <input name="title" required />
      {state.error && <p role="alert">{state.error}</p>}
      <button disabled={pending}>Add</button>
    </form>
  );
}
```

```tsx
// File: app/api/todos/route.ts  →  URL: POST /api/todos
// Когда нужен HTTP JSON для не-Next клиента — те же шаги 2–4, ответ Response
import { NextRequest } from "next/server";
import { auth } from "@/lib/auth";

export async function POST(request: NextRequest) {
  const session = await auth();
  if (!session)
    return Response.json({ error: "Unauthorized" }, { status: 401 });

  const body = await request.json();
  const title = String(body.title ?? "").trim();
  if (!title) return Response.json({ error: "Invalid title" }, { status: 400 });

  await db.todo.create({ data: { title, userId: session.user.id } });
  return Response.json({ ok: true }, { status: 201 });
}
```

```tsx
// File: app/actions/orders.ts — отдельный backend: один fetch внутри action
"use server";

import { revalidatePath } from "next/cache";
import { auth } from "@/lib/auth";

export async function cancelOrder(formData: FormData) {
  const session = await auth();
  if (!session) throw new Error("Unauthorized");

  const orderId = String(formData.get("orderId"));
  const res = await fetch(`${process.env.API_URL}/orders/${orderId}/cancel`, {
    method: "POST",
    headers: { Authorization: `Bearer ${process.env.API_SERVICE_TOKEN}` },
  });
  if (!res.ok) throw new Error("Cancel failed");

  revalidatePath("/orders");
  revalidatePath(`/orders/${orderId}`);
}
```

**Thoughts:** Для UI по умолчанию: **action + `revalidatePath`**. Сравнение: [Comparison table](#comparison-table). Чтение — RSC, [Choosing a data fetch path](#choosing-a-data-fetch-path).

**Sources:**

- [nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)
- [nextjs.org/docs/app/getting-started/mutating-data](https://nextjs.org/docs/app/getting-started/mutating-data)

</details>

<details>
  <summary>Server Actions</summary>

<a name="server-actions"></a>

### Server Actions

**Definition:** `async`-функция с `"use server"`, которая **меняет данные на сервере** (создать, обновить, удалить). Доступны БД, секреты, `cookies()` — код не уходит в браузер. Файлы, вызов и transport — в комментариях к **Examples**.

**Server Actions — для:** сохранения форм, удаления строки, переключения состояния — **запись** из UI Next.js. **Не для:** загрузки списка товаров или текста статьи ([Server Component fetching](#server-component-fetching)); публичных API для мобилок или webhooks ([Route Handlers](#route-handlers)).

**Безопасность (важно):** любой, кто открыл сайт, может **попытаться** вызвать экспортированный action (как POST API). Проверяйте «залогинен ли пользователь?» и «можно ли ему это?» **внутри каждого action**, а не только в layout или middleware. Подробнее: [CSRF with Server Actions](#csrf-with-server-actions) · [Server Functions](./4_react_rsc.md#server-functions).

**How it works:**

| Placement  | Pattern                                        |
| ---------- | ---------------------------------------------- |
| **Module** | файл с `'use server'` — первый **Example**     |
| **Inline** | action в Server Component — второй **Example** |

| Wire protocol | Behavior                                                                                                                                                                                    |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Method**    | `POST` на маршрут страницы, зарегистрировавшей action (или текущий сегмент при навигации)                                                                                                   |
| **Headers**   | `Next-Action` — opaque action ID; браузер шлёт cookies для same-origin сессии                                                                                                               |
| **Body**      | `multipart/form-data` для `<form>`; сериализованные аргументы при программном вызове — лимит по умолчанию **1 MB** ([`serverActions.bodySizeLimit`](./4_nextjs_platform.md#next-config-ts)) |

1. Клиент вызывает `action` на `<form>`, `formAction` на кнопке или импортирует reference в `startTransition` / `useActionState` — см. [formAction and programmatic invocation](#formaction-and-programmatic-invocation).
2. Next.js POSTит с action ID и **сериализуемыми** аргументами — правила в [Serialization and return values](#serialization-and-return-values).
3. Код только на сервере (БД, секреты, `cookies()` / `headers()` — в Next.js 15+ **await**) — [Cookies and headers in Server Actions](#cookies-and-headers-in-server-actions).
4. После успешной записи сбросьте кэши UI (`revalidatePath`, `revalidateTag`, `updateTag`) — см. [Common pitfall: stale UI after mutation](#common-pitfall-stale-ui-after-mutation).
5. Опционально `redirect()` после мутации; он **бросает** — **вне** `try/catch`, обычно **после** revalidation.
6. Доп. ID из UI (например `userId`) — предпочитайте `action.bind(null, id)` скрытым полям — [Server Action arguments (bind)](#server-action-arguments-bind).

Модель React (директивы, сериализация): [Server Functions](./4_react_rsc.md#server-functions) · ['use server'](./4_react_rsc.md#use-server).

**When to use:**

- Отправка форм и **мутации** из Server или Client Components.
- Progressive enhancement: `<form action={action}>` работает **без** клиентского JS.
- Colocated writes, когда потребитель только ваше Next-приложение — не мобильные клиенты и webhooks ([Route Handlers](#route-handlers)).

**When not to use:**

- **Чтение** / списки и детали — `async` Server Components или `fetch` в RSC ([Server Component fetching](#server-component-fetching)).
- Публичные HTTP API, webhooks, произвольные методы — Route Handlers.
- Долгие фоновые задачи — очередь + worker; action остаётся в рамках запроса.

**Pluses and minuses:**

| +                                                | −                                                                                                     |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| Рядом с UI; вызов из `<form action>`             | Не REST-контракт для сторонних систем                                                                 |
| Progressive enhancement (JS не обязателен)       | Только POST; не кэшируется как GET                                                                    |
| Типобезопасный импорт из Client Components       | Любой аргумент валидировать на сервере (не доверять `FormData`)                                       |
| Встроенная same-origin CSRF-модель               | Cross-origin — `serverActions.allowedOrigins` — [CSRF with Server Actions](#csrf-with-server-actions) |
| Сочетается с `useActionState` для ошибок/pending | Легко забыть `revalidatePath` / `revalidateTag` после записи                                          |

#### Common pitfalls

- Использовать actions как **query** API — они в очереди и последовательны; чтение через RSC.
- `as string` у `formData.get` без проверок — Zod или аналог в action.
- Только `router.refresh()` после мутации — часто мало; явно revalidate серверные кэши.
- `redirect()` в `catch` или до revalidation — навигация обрывает поток; сначала revalidate.
- Мутации в **render** Server Component — только в actions или Route Handlers.

**Examples:**

```tsx
// File: app/actions.ts — module pattern ('use server' в начале файла)
// Called from: import { createTodo } → <form action={createTodo}>
// Transport: POST + action id (не REST-путь /api/todos)
// Arg: FormData — <input name="title"> → formData.get("title")
"use server";

import { revalidatePath } from "next/cache";
import { redirect } from "next/navigation";
import { auth } from "@/lib/auth";

export async function createTodo(formData: FormData) {
  const session = await auth();
  if (!session?.user) throw new Error("Unauthorized");

  const title = String(formData.get("title") ?? "").trim();
  if (!title) return { error: "Title required" };

  const todo = await db.todo.create({
    data: { title, userId: session.user.id },
  });

  revalidatePath("/todos");
  redirect(`/todos/${todo.id}`);
}
```

```tsx
// File: app/todos/new/page.tsx — inline pattern (Server Component; без "use client")
// Только эта страница; <form action={create}> работает без JS
export default function NewTodoPage() {
  async function create(formData: FormData) {
    "use server";
    const title = String(formData.get("title"));
    await db.todo.create({ data: { title } });
    revalidatePath("/todos");
  }

  return (
    <form action={create}>
      <input name="title" required />
      <button type="submit">Add</button>
    </form>
  );
}
```

```tsx
// File: app/actions.ts — useActionState: (prevState, formData) => nextState
export async function createTodoWithState(
  _prev: { error?: string },
  formData: FormData,
) {
  const title = String(formData.get("title") ?? "").trim();
  if (!title) return { error: "Title required" };
  await db.todo.create({ data: { title } });
  revalidatePath("/todos");
  return { error: undefined };
}
```

```tsx
// File: app/todos/TodoForm.tsx — "use client"; pending и ошибки из return state
"use client";

import { useActionState } from "react";
import { createTodoWithState } from "@/app/actions";

const initialState = { error: undefined as string | undefined };

export function TodoForm() {
  const [state, formAction, pending] = useActionState(
    createTodoWithState,
    initialState,
  );

  return (
    <form action={formAction}>
      <input name="title" required />
      {state?.error && <p role="alert">{state.error}</p>}
      <button type="submit" disabled={pending}>
        Save
      </button>
    </form>
  );
}
```

**Thoughts:** Держите actions тонкими — auth, validate, один вызов `lib/db` или `fetch(API_URL)` ([Colocated backend](#colocated-backend-in-nextjs) · [Separate backend](#fetching-from-a-separate-backend)). Ошибки полей — сериализуемый state для `useActionState`; throw или `redirect` при жёстких сбоях. Сравнение: [Comparison table](#comparison-table). Pending без prop drilling: [useFormStatus - 19](./4_react_hooks.md#useformstatus-19).

**Sources:**

- [nextjs.org/docs/app/getting-started/mutating-data](https://nextjs.org/docs/app/getting-started/mutating-data)
- [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)
- [nextjs.org/docs/app/guides/server-actions](https://nextjs.org/docs/app/guides/server-actions)

</details>

<details>
  <summary>Server Action arguments (bind)</summary>

<a name="server-action-arguments-bind"></a>

### Server Action arguments (bind)

**Definition:** Передача **дополнительных аргументов** в Server Action помимо `FormData` через bound server reference: `Function.prototype.bind`.

**How it works:**

- Сигнатура: `(id, formData)` или `(id, prevState, formData)` с `useActionState`.
- `updateUser.bind(null, userId)` фиксирует первый аргумент; поля формы — в `FormData`.
- Работает из Client и Server Components; `<form action={boundAction}>` сохраняет progressive enhancement.
- **Валидируйте** bound ID на сервере (формат, владелец) — как любой недоверенный ввод.

**When to use:**

- Один модуль actions, много строк (`deleteTodo.bind(null, todo.id)`).
- Константы/enum, которые не должны быть редактируемыми полями.

**When not to use:**

- Большие или чувствительные данные — bound args уходят с reference; лучше lookup по ID из сессии.
- Секреты — только `process.env` на сервере.

**Pluses and minuses:**

| +                                                | −                                     |
| ------------------------------------------------ | ------------------------------------- |
| Меньше подделываемых hidden `<input>` для ID     | Bound values должны быть сериализуемы |
| Один паттерн для `formAction` и `useActionState` | Легко забыть authZ для bound ID       |

**Examples:**

```tsx
// File: app/actions.ts — bind задаёт 1-й аргумент (userId); FormData — 2-й при submit
"use server";

export async function updateUser(userId: string, formData: FormData) {
  const session = await getSession();
  if (!session || session.userId !== userId) throw new Error("Forbidden");

  const name = String(formData.get("name") ?? "");
  await db.user.update({ where: { id: userId }, data: { name } });
  revalidatePath(`/users/${userId}`);
}
```

```tsx
// File: app/users/[id]/UserProfile.tsx — "use client"
// bind(null, userId) — всё равно сверяйте userId с сессией в action
"use client";

import { updateUser } from "@/app/actions";

export function UserProfile({ userId }: { userId: string }) {
  const updateUserWithId = updateUser.bind(null, userId);

  return (
    <form action={updateUserWithId}>
      <input name="name" />
      <button type="submit">Save</button>
    </form>
  );
}
```

**Thoughts:** Hidden fields допустимы для same-origin форм, но **авторизация** не должна опираться на `userId` с клиента без проверки сессии.

**Sources:** [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)

</details>

<details>
  <summary>Closures and encryption</summary>

<a name="closures-and-encryption"></a>

### Closures and encryption

**Definition:** **Inline** Server Actions внутри Server Component **замыкают** переменные render-прохода (версия, флаг, snapshot draft ID).

**How it works:**

- Захваченные значения уходят клиенту с server reference и возвращаются при вызове action.
- Next.js **шифрует** closed-over values, чтобы сырой серверный контекст не светился в бандле.
- Ключи **на билд** — action со старого деплоя не воспроизвести на новом после redeploy.
- Не замыкайте секреты (URL БД, ключи) — читайте из `process.env` в теле action.

**When to use:**

- Оптимистичная конкуренция: snapshot `publishVersion` при render, повторная проверка в `publish()`.
- Snapshot feature flag, когда action должен соответствовать тому, что пользователь видел при render.

**Pluses and minuses:**

| +                                  | −                                      |
| ---------------------------------- | -------------------------------------- |
| Без лишнего round-trip за контекст | Растёт payload при больших capture     |
| Шифрование на проводе              | Не замена authZ — проверять при invoke |

**Examples:**

```tsx
// File: app/editor/page.tsx — Server Component
// publishVersion замыкается при render; Next.js шифрует в payload action
export default async function EditorPage() {
  const publishVersion = await getLatestVersion();

  async function publish() {
    "use server";
    if (publishVersion !== (await getLatestVersion())) {
      throw new Error("Version changed — refresh and try again");
    }
    await saveDraft();
    revalidatePath("/editor");
  }

  return (
    <form>
      <button formAction={publish}>Publish</button>
    </form>
  );
}
```

**Thoughts:** Авторитетные ID лучше читать из БД в action; closures — для **snapshot**, не для trust boundary.

**Sources:** [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)

</details>

<details>
  <summary>Serialization and return values</summary>

<a name="serialization-and-return-values"></a>

### Serialization and return values

**Definition:** Server Actions принимают и возвращают только **React-сериализуемые** значения через server boundary.

**How it works:**

| Direction  | Allowed (typical)                                                             | Avoid                                                                           |
| ---------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Args**   | `FormData`, строки, числа, boolean, plain objects/arrays, `null`, `undefined` | Функции, symbols, большинство class instances, `Date` (ISO string), `Map`/`Set` |
| **Return** | То же; ошибки полей для `useActionState`                                      | `throw` для **ожидаемой** валидации — лучше `{ error: "…" }`                    |

- **Ожидаемые ошибки** → сериализуемый state + `useActionState`.
- **Неожиданные** → `throw` / `error.tsx`; для auth — `unauthorized()` / `forbidden()` при `authInterrupts`.
- Программный вызов из handlers — оборачивать в `startTransition`.

**When to use:**

- `useActionState` для inline ошибок и pending без полной навигации.
- `redirect()` / `throw` для другой страницы или жёсткого сбоя.

**Pluses and minuses:**

| +                                | −                                               |
| -------------------------------- | ----------------------------------------------- |
| Типизируемый return для форм     | Легко вернуть несериализуемое                   |
| Разделение expected / unexpected | `redirect()` бросает — не ловить как validation |

**Examples:**

```tsx
// File: app/actions.ts — return { message } для валидации; unauthorized() для auth
"use server";

import { unauthorized } from "next/navigation";

export async function updateProfile(
  _prev: { message?: string },
  formData: FormData,
) {
  const session = await verifySession();
  if (!session) unauthorized();

  const title = String(formData.get("title") ?? "").trim();
  if (!title) return { message: "Title is required" };

  await db.post.create({ data: { title, userId: session.userId } });
  revalidatePath("/posts");
  return { message: "" };
}
```

**Thoughts:** Как API handler: validate → authorize → write → revalidate → return или redirect.

**Sources:**

- [nextjs.org/docs/app/getting-started/10-error-handling](https://nextjs.org/docs/app/getting-started/error-handling)
- [nextjs.org/docs/app/api-reference/functions/unauthorized](https://nextjs.org/docs/app/api-reference/functions/unauthorized)

</details>

<details>
  <summary>Cookies and headers in Server Actions</summary>

<a name="cookies-and-headers-in-server-actions"></a>

### Cookies and headers in Server Actions

**Definition:** Server Actions выполняются в **контексте запроса** и могут читать/менять **cookies** и читать **headers** через `next/headers`.

**How it works:**

- `const cookieStore = await cookies()` в Next.js 15+.
- `cookieStore.set` / `delete` вызывает **server re-render** текущего дерева маршрутов (locale, cart id и т.д.).
- `headers()` — входящие заголовки (user-agent, proxy); для сессии предпочитайте `cookies()`.
- Установка cookie не заменяет проверку auth в action.

**When to use:**

- Login/logout с session cookie.
- Настройки в cookie без отдельного Route Handler.

**Pluses and minuses:**

| +                               | −                                             |
| ------------------------------- | --------------------------------------------- |
| Одна мутация для формы + cookie | Cookie mutation → RSC refresh — продумайте UX |
| Те же API, что у Route Handlers | `headers()` → dynamic rendering               |

**Examples:**

```tsx
// File: app/actions.ts — <form action={setLocale}> с полем name="locale"
// cookies().set → re-render layout с новой локалью
"use server";

import { cookies } from "next/headers";
import { revalidatePath } from "next/cache";

export async function setLocale(formData: FormData) {
  const locale = String(formData.get("locale") ?? "en");
  const cookieStore = await cookies();
  cookieStore.set("locale", locale, { path: "/", maxAge: 60 * 60 * 24 * 365 });
  revalidatePath("/", "layout");
}
```

**Thoughts:** Auth-библиотеки — их server helpers внутри actions; см. [auth](./4_nextjs_platform.md#rsc-security).

**Sources:** [nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)

</details>

<details>
  <summary>formAction and programmatic invocation</summary>

<a name="formaction-and-programmatic-invocation"></a>

### formAction and programmatic invocation

**Definition:** Вызов Server Actions из **форм** (`action`, `formAction`) или **напрямую** из client event handlers.

**How it works:**

| Pattern              | API                                             | Progressive enhancement  |
| -------------------- | ----------------------------------------------- | ------------------------ |
| Submit всей формы    | `<form action={action}>`                        | Да                       |
| Кнопка в форме       | `<button formAction={saveDraft}>Draft</button>` | Да                       |
| Кнопка в client tree | `<button formAction={createPost}>`              | Да, если внутри `<form>` |
| Click / custom       | `startTransition(() => action(arg))`            | Нет — нужен JS           |

- Несколько actions в одной форме — разные `formAction` (publish vs draft).
- Импорт actions в Client только из модуля `"use server"`.

**When to use:**

- `formAction` — несколько типов мутации в одной форме.
- Программный вызов — delete, toggle, шаги wizard без native submit.

**Pluses and minuses:**

| +                                 | −                                           |
| --------------------------------- | ------------------------------------------- |
| HTML multi-submit                 | Программный путь без HTML validation        |
| `startTransition` — отзывчивый UI | Pending/errors вручную без `useActionState` |

**Examples:**

```tsx
// File: components/DeleteButton.tsx — "use client"
// Programmatic (без <form>); deleteTodo из app/actions.ts
"use client";

import { startTransition } from "react";
import { deleteTodo } from "@/app/actions";

export function DeleteButton({ id }: { id: string }) {
  return (
    <button
      type="button"
      onClick={() => {
        startTransition(() => {
          void deleteTodo(id);
        });
      }}
    >
      Delete
    </button>
  );
}
```

```tsx
// File: app/posts/edit/page.tsx — Server Component; publish и saveDraft — Server Actions
// Submit по умолчанию → publish; вторая кнопка → saveDraft через formAction
<form action={publish}>
  <textarea name="body" required />
  <button type="submit">Publish</button>
  <button type="submit" formAction={saveDraft}>
    Save draft
  </button>
</form>
```

**Thoughts:** Формы + `useActionState` для a11y ошибок; programmatic — для мелких не-form взаимодействий.

**Sources:**

- [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)
- [nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)

</details>

<details>
  <summary>Optimistic UI with Server Actions</summary>

<a name="optimistic-ui-with-server-actions"></a>

### Optimistic UI with Server Actions

**Definition:** **Мгновенный** отклик UI, пока Server Action выполняется, с согласованием после ответа сервера.

**How it works:**

- `useOptimistic(baseState, reducer)` — временное client state.
- В client `form action`: `addOptimistic(...)`, затем `await serverAction(...)`.
- При ошибке — `router.refresh()`, сброс optimistic или сообщение; auto-rollback нет.
- `revalidatePath` в action, чтобы RSC совпал с optimistic view.

**When to use:**

- Чат, лайки, чеклисты при заметной задержке.
- Не замена `useActionState` — можно комбинировать.

**Pluses and minuses:**

| +              | −                                      |
| -------------- | -------------------------------------- |
| Быстрый UX     | Явная обработка ошибки action          |
| С form actions | Дублирование, если optimistic ≠ server |

**Examples:**

```tsx
// File: app/chat/Thread.tsx — "use client"
// Client form action: addOptimistic, затем await sendMessage() из ./actions.ts
"use client";

import { useOptimistic } from "react";
import { sendMessage } from "./actions";

type Message = { id: string; text: string };

export function Thread({ messages }: { messages: Message[] }) {
  const [optimistic, addOptimistic] = useOptimistic(
    messages,
    (state, text: string) => [...state, { id: `tmp-${state.length}`, text }],
  );

  async function onSubmit(formData: FormData) {
    const text = String(formData.get("text") ?? "");
    addOptimistic(text);
    await sendMessage(text);
  }

  return (
    <div>
      {optimistic.map((m) => (
        <p key={m.id}>{m.text}</p>
      ))}
      <form action={onSubmit}>
        <input name="text" />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

**Thoughts:** Временные ID до revalidation с реальными ID из БД.

**Sources:** [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)

</details>

<details>
  <summary>Route Handlers</summary>

<a name="route-handlers"></a>

### Route Handlers

**Definition:** **Route Handler** — серверный файл `route.ts` (или `route.js`) в папке `app/`, который отвечает на **настоящие HTTP-запросы** по URL, как небольшой API внутри Next.js. Экспортируете функции с именами методов: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`. Функция получает **`Request`** (или `NextRequest`) и возвращает **`Response`** (часто `Response.json({ ... })`).

**Route Handlers — когда** HTTP нужен **вне** React UI: **webhooks** Stripe/GitHub, **мобильное приложение**, **браузер с другого домена** (CORS), **Client Component** с `fetch("/api/metrics")` после загрузки. **Не используйте** как основной способ чтения БД для **Server Components** — вызывайте `lib/db` напрямую ([Colocated backend](#colocated-backend-in-nextjs)). Для форм только вашего Next UI — чаще [Server Actions](#server-actions), если не нужен стабильный публичный URL.

**Безопасность:** handler — **публичный URL**. Проверяйте API-ключи, подписи webhook и сессию **внутри handler**. Same-origin `fetch("/api/...")` всё равно требует auth — cookies можно отправить, но URL доступен и через curl.

**How it works:**

| Piece       | Rule                                                                                                                                   |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **File**    | только `route.ts` под `app/` (не `pages/api` — это Pages Router)                                                                       |
| **URL**     | путь папки под `app/` = URL — см. комментарии в **Examples**                                                                           |
| **Methods** | экспортируйте только нужные методы; остальные → **405**                                                                                |
| **Body**    | `await request.json()`, `request.text()`, `request.formData()` — без `bodyParser` из Pages Router                                      |
| **Cookies** | `request.cookies` на `NextRequest`; установка — на возвращаемом `Response`                                                             |
| **Config**  | те же segment options, что у pages: `dynamic`, `revalidate`, `runtime` — см. [Caching GET Route Handlers](#caching-get-route-handlers) |

**When to use:**

- Webhooks и колбэки ([Webhooks](#webhooks-in-route-handlers)).
- BFF для браузера (`/api/...`) без service token в клиенте ([BFF Route Handler](#bff-route-handler)).
- Публичный REST/JSON для не-Next клиентов.

**When not to use:**

- Обычное **чтение** для RSC — Server Components + `fetch` / БД.
- **Мутации форм** только для вашего UI — [Server Actions](#server-actions).

**Pluses and minuses:**

| +                                 | −                                          |
| --------------------------------- | ------------------------------------------ |
| Настоящий HTTP — любой клиент     | Нет progressive enhancement через `<form>` |
| Стабильный документируемый URL    | CORS, CSRF, rate limit, auth — на вас      |
| Webhooks и мобильные клиенты      | Лишний hop, если RSC дергает свой `/api`   |
| `GET` можно кэшировать (с config) | Нельзя `page.tsx` в той же папке           |

**Examples:**

```tsx
// File: app/api/users/route.ts
// URL:  GET/POST … https://your-site.com/api/users
// Rule: в этой папке НЕ должно быть page.tsx — сегмент: либо страница, либо route.ts
export async function GET() {
  const users = await db.user.findMany();
  return Response.json(users);
}
```

```tsx
// File: app/api/health/route.ts  →  URL: /api/health
export async function GET() {
  return Response.json({ ok: true });
}
```

```tsx
// File: app/api/posts/[id]/route.ts
// URL:  GET https://your-site.com/api/posts/123  →  params.id === "123"
// Next.js 15+: params — Promise, нужен await
export async function GET(
  _request: Request,
  { params }: { params: Promise<{ id: string }> },
) {
  const { id } = await params;
  const post = await db.post.findUnique({ where: { id } });
  if (!post) return Response.json({ error: "Not found" }, { status: 404 });
  return Response.json(post);
}
```

```tsx
// File: app/api/webhook/route.ts  →  URL: POST /api/webhook
export async function POST(request: Request) {
  const raw = await request.text();
  // verify signature on raw body
  return new Response("ok", { status: 200 });
}
```

**Thoughts:** Cross-origin в браузере → [Handling CORS in Next.js](#handling-cors-in-nextjs). Same-origin `fetch("/api/...")` — без CORS. Сравнение с [Server Actions](#server-actions) — [Comparison table](#comparison-table).

**Sources:**

- [nextjs.org/docs/app/getting-started/route-handlers](https://nextjs.org/docs/app/getting-started/route-handlers)
- [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>BFF Route Handler</summary>

<a name="bff-route-handler"></a>

### BFF Route Handler

**Definition:** Route Handler **между браузером и бэкендом**: UI вызывает **ваш** `/api/...`, handler проверяет сессию, подставляет **service token** и ходит дальше в API или БД.

**How it works:** Браузер → ваш `/api/...`; handler → бэкенд с серверными credentials — см. комментарии в **Examples**. RSC обычно без hop ([Fetching from a separate backend](#fetching-from-a-separate-backend)).

**When to use:**

- Дашборды с poll/мутациями после hydration ([Client-side fetching](#client-side-fetching)).
- Скрыть токены, но оставить browser `fetch`.

**Pluses and minuses:**

| +                          | −                                 |
| -------------------------- | --------------------------------- |
| Ясный HTTP-контракт для UI | Задержка vs прямой server `fetch` |
| Единая точка authZ         | Дублирование с actions            |

**Examples:**

```tsx
// File: app/api/orders/route.ts  →  URL: GET /api/orders
// Client: fetch("/api/orders") — same origin, cookie сессии
// Server: fetch(API_URL/orders) с service token — не в NEXT_PUBLIC_*
import { NextRequest } from "next/server";
import { getSession } from "@/lib/auth";

export async function GET(_request: NextRequest) {
  const session = await getSession();
  if (!session)
    return Response.json({ error: "Unauthorized" }, { status: 401 });

  const res = await fetch(`${process.env.API_URL}/orders`, {
    headers: { Authorization: `Bearer ${session.accessToken}` },
  });
  const data = await res.json();
  return Response.json(data, { status: res.status });
}
```

**Thoughts:** Same-origin BFF **обходит CORS в браузере** — если клиент только ваш Next. Партнёрский SPA на другом host → [Handling CORS in Next.js](#handling-cors-in-nextjs). GET TTL — [Vercel CDN cache headers](#vercel-cdn-cache-headers).

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)

</details>

<details>
  <summary>Handling CORS in Next.js</summary>

<a name="handling-cors-in-nextjs"></a>

### Handling CORS in Next.js

**Definition:** **CORS** — правило **браузера**: JS на `https://app.example.com` может читать ответ с `https://api.example.com` только если Route Handler отдаёт заголовки `Access-Control-*` (и отвечает на **`OPTIONS`**, когда нужен preflight). Server Components, Server Actions и server `fetch` **не** используют CORS — только cross-origin **browser** `fetch` / XHR.

**How it works:**

| Step                  | What to do                                                                                                     |
| --------------------- | -------------------------------------------------------------------------------------------------------------- |
| **1. Need CORS?**     | Ваш Next на **том же origin** (`fetch("/api/...")`) → **без CORS**. Другой сайт/порт → **да**.                 |
| **2. Prefer no CORS** | [BFF Route Handler](#bff-route-handler): браузер бьёт в ваш домен; сервер ходит в API с секретами.             |
| **3. Pick layer**     | Один публичный API → заголовки в `route.ts`. Много `/api/*` → `next.config` `headers()` или `proxy.ts`.        |
| **4. Preflight**      | Cross-origin «непростые» запросы шлют **`OPTIONS`** — экспорт `OPTIONS` или обработка в `proxy.ts`.            |
| **5. Credentials**    | С cookies / `credentials: 'include'`: конкретный `Origin` (не `*`) + `Access-Control-Allow-Credentials: true`. |
| **6. Auth**           | CORS **не** заменяет auth — проверяйте токен/сессию в handler.                                                 |

| Approach                      | When                                      |
| ----------------------------- | ----------------------------------------- |
| **Route Handler headers**     | Один endpoint партнёра, своя политика     |
| **`next.config` `headers()`** | Фиксированный allowlist для `/api/:path*` |
| **`proxy.ts`**                | Динамический Origin + общий preflight     |
| **BFF (same origin)**         | Только ваш UI — CORS не нужен             |

**When to use:** SPA партнёра, dev `localhost:3001` → `localhost:3000`, расширение, вызывающее API.

**When not to use:** Вместо auth; `Allow-Origin: *` с credentials; CORS на routes, которые ваши страницы и так дергают same-origin.

**Pluses and minuses:**

| +                               | −                           |
| ------------------------------- | --------------------------- |
| Полный контроль на route        | Легко забыть `OPTIONS`      |
| `headers()` / `proxy` — DRY     | Неверный matcher ломает API |
| BFF — проще для своего frontend | Доп. hop на сервере         |

**Examples:**

```ts
// File: app/api/public/health/route.ts  →  URL: GET /api/public/health
// Caller: https://partner.example (cross-origin) — те же headers на GET и OPTIONS
const cors = {
  "Access-Control-Allow-Origin": "https://partner.example",
  "Access-Control-Allow-Methods": "GET, OPTIONS",
  "Access-Control-Allow-Headers": "Content-Type, Authorization",
};

export async function OPTIONS() {
  return new Response(null, { status: 204, headers: cors });
}

export async function GET() {
  return Response.json({ ok: true }, { headers: cors });
}
```

```ts
// File: next.config.ts — одна CORS-политика для всех /api/* (статический allowlist)
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  async headers() {
    return [
      {
        source: "/api/:path*",
        headers: [
          {
            key: "Access-Control-Allow-Origin",
            value: "https://partner.example",
          },
          {
            key: "Access-Control-Allow-Methods",
            value: "GET, POST, PUT, DELETE, OPTIONS",
          },
          {
            key: "Access-Control-Allow-Headers",
            value: "Content-Type, Authorization",
          },
        ],
      },
    ];
  },
};

export default nextConfig;
```

```ts
// File: proxy.ts — matcher: /api/:path* — динамический allowlist Origin + OPTIONS
import { NextRequest, NextResponse } from "next/server";

const allowedOrigins = ["https://acme.com", "http://localhost:3001"];

export function proxy(request: NextRequest) {
  const origin = request.headers.get("origin") ?? "";
  const allowed = allowedOrigins.includes(origin);

  if (request.method === "OPTIONS") {
    const headers: Record<string, string> = {
      "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE, OPTIONS",
      "Access-Control-Allow-Headers": "Content-Type, Authorization",
    };
    if (allowed) headers["Access-Control-Allow-Origin"] = origin;
    return NextResponse.json({}, { headers });
  }

  const response = NextResponse.next();
  if (allowed) response.headers.set("Access-Control-Allow-Origin", origin);
  return response;
}

export const config = { matcher: "/api/:path*" };
```

**Thoughts:** Подробнее: [CORS in Next.js](./4_nextjs_platform.md#cors-in-nextjs). Теория: [Web API — CORS](./6_web_api_security.md#cors). [Server Actions](#server-actions) — CSRF фреймворка для same-origin форм, не CORS.

**Sources:**

- [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)
- [nextjs.org/docs/app/api-reference/config/next-config-js/headers](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)

</details>

<details>
  <summary>Webhooks in Route Handlers</summary>

<a name="webhooks-in-route-handlers"></a>

### Webhooks in Route Handlers

**Definition:** HTTP **POST** (иногда GET), который **внешний сервис** вызывает при событии — оплата, merge PR, доставка письма.

**How it works:** Публичный POST URL, raw body для подписи, быстрый 2xx, дедуп event id — см. комментарии в **Examples**.

**When to use:**

- Вендор пишет «webhook на ваш URL» — не [Server Actions](#server-actions).

**Pluses and minuses:**

| +                   | −                           |
| ------------------- | --------------------------- |
| Стандарт интеграций | Подписи проверяете сами     |
| Работает на Vercel  | Повторы / дубликаты событий |

**Examples:**

```tsx
// File: app/api/stripe/webhook/route.ts  →  URL: POST /api/stripe/webhook
// Caller: серверы Stripe (не React) — URL в dashboard Stripe
// request.text() до JSON.parse при проверке Stripe-Signature
export async function POST(request: Request) {
  try {
    const payload = await request.text();
    const sig = request.headers.get("stripe-signature");
    // stripe.webhooks.constructEvent(payload, sig, webhookSecret)
    return new Response("ok", { status: 200 });
  } catch {
    return new Response("Webhook error", { status: 400 });
  }
}
```

**Thoughts:** Browser POST с вашего сайта — CSRF; webhooks — **shared secret**, не cookies.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>Caching GET Route Handlers</summary>

<a name="caching-get-route-handlers"></a>

### Caching GET Route Handlers

**Definition:** Как **`GET`** Route Handlers связаны с кэшем Next.js и CDN — отдельно от RSC `fetch` cache.

**How it works:**

- В новых версиях **`GET` handlers по умолчанию динамические** (не кэшируются автоматически), пока не включите явно.
- Segment config в файле, например `export const dynamic = 'force-static'` для безопасного build-time кэша.
- `revalidate` или `Cache-Control` на `Response` — [Vercel CDN cache headers](#vercel-cdn-cache-headers).
- `POST`/`PUT` не кэшируются статически.

**When to use:**

- Публичный read-only JSON с известным TTL.
- Не для персональных `GET` без `private, no-store` и проверки сессии.

**Pluses and minuses:**

| +                        | −                                             |
| ------------------------ | --------------------------------------------- |
| Edge/CDN                 | Устаревшие данные при долгом TTL              |
| Те же options, что pages | Риск закэшировать private без `Cache-Control` |

**Examples:**

```tsx
// File: app/api/public/config/route.ts  →  URL: GET /api/public/config
// dynamic = 'force-static' — явное кэширование GET (по умолчанию dynamic в новых Next.js)
export const dynamic = "force-static";

export async function GET() {
  const data = await getPublicConfig();
  return Response.json(data);
}
```

**Thoughts:** Персональные API → `Cache-Control: private, no-store` + auth на каждый запрос.

**Sources:**

- [nextjs.org/docs/app/guides/upgrading/version-15](https://nextjs.org/docs/app/guides/upgrading/version-15)
- [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>Comparison table</summary>

### Comparison table

**Definition:**

Матрица решений для выбора Server Actions или Route Handlers по модели вызова и потребителям.

|                | Server Actions        | Route Handlers                |
| -------------- | --------------------- | ----------------------------- |
| **Invocation** | RPC / `<form action>` | HTTP to URL                   |
| **Protocol**   | POST (wrapped)        | GET, POST, PUT, PATCH, DELETE |
| **Consumers**  | Next.js app only      | Any HTTP client               |
| **Use case**   | Mutations, forms      | Webhooks, public APIs         |

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
// File: next.config.ts — preview host может вызывать actions с другого origin
const nextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ["localhost:3000", "preview.example.com"],
    },
  },
};
```

```tsx
// File: app/actions.ts — CSRF для same-origin форм на фреймворке; authZ — на вас
"use server";

import { revalidatePath } from "next/cache";
import { z } from "zod";

const schema = z.object({ title: z.string().min(1).max(200) });

export async function createTodo(formData: FormData) {
  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Invalid title" };

  const session = await getSession();
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
