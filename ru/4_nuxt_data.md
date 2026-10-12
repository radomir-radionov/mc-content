# Nuxt — Data

`useFetch`, `useAsyncData`, кэширование и серверные маршруты.

[← Вернуться в hub](./4_nuxt.md)

### Related docs

- [Nuxt — Ядро](./4_nuxt_core.md) · [Nuxt — Данные](./4_nuxt_data.md) · [Nuxt — Маршрутизация](./4_nuxt_routing.md) · [Nuxt — Платформа](./4_nuxt_platform.md) · [Nuxt — Интервью](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [7. Data fetching](#7-data-fetching)
- [8. Caching & payload](#8-caching-payload)
- [9. Server routes](#9-server-routes)

---

<a name="7-data-fetching"></a>

## 7. Data fetching

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>$fetch</summary>

### $fetch

**Definition:** Nuxt auto-imports **`$fetch`** — тонкую обёртку над [ofetch](https://github.com/unjs/ofetch) для HTTP-запросов. Работает в Vue setup, Nitro handlers и plugins.

**How it works:** В отличие от `useFetch`, `$fetch` **не** передаёт серверные результаты в клиентский payload. Вызов напрямую в `setup` во время SSR может вызвать **double fetch** (сервер + client hydration).

**When to use:**

- Client-side event handlers (submit формы, клик кнопки)
- Nitro `server/api` handlers, вызывающие внешние API
- Внутри `useAsyncData` для SSR-safe начальной загрузки

**Pluses and minuses:**

| + | − |
|---|---|
| Простой imperative API | Нет автоматической SSR deduplication сам по себе |
| Typed responses с generics | Легко вызвать hydration double-fetch |
| Работает изоморфно | Нет shared loading `status` ref |

**Examples:**

```vue
<script setup lang="ts">
async function addTodo() {
  await $fetch('/api/todos', {
    method: 'POST',
    body: { title: 'Learn Nuxt' },
  })
}
</script>
```

```ts
// server/api/echo.get.ts
export default defineEventHandler((event) => {
  return { path: getRequestURL(event).pathname }
})
```

**Thoughts:** Правило: **`useFetch` / `useAsyncData` для начальных данных в setup**; **`$fetch` для user-triggered запросов**.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>useFetch</summary>

### useFetch

**Definition:** SSR-safe composable, оборачивающий `$fetch`. Гарантирует, что данные, загруженные на сервере, **сериализуются в payload**, чтобы клиент не refetch при гидрации.

**How it works:** При server render Nuxt выполняет fetch, сохраняет результат в `useNuxtApp().payload` и гидратирует `data` ref на клиенте без второго сетевого вызова (для того же key/URL).

**When to use:** Default для загрузки REST/JSON в pages из `server/api` или внешних URL.

**Pluses and minuses:**

| + | − |
|---|---|
| One-liner SSR + client parity | URL-based keys различаются per call site без explicit `key` |
| Возвращает `data`, `status`, `error`, `refresh` | Меньше контроля, чем `useAsyncData` для non-HTTP sources |
| Auto proxies cookies на сервере через `useRequestFetch` | External API header proxying требует внимания |

**Examples:**

```vue
<script setup lang="ts">
const { data: count } = await useFetch('/api/count')
</script>

<template>
  <p>Visits: {{ count }}</p>
</template>
```

```vue
<script setup lang="ts">
const route = useRoute()
const { data: post } = await useFetch(`/api/posts/${route.params.id}`)
</script>
```

**Thoughts:** `useFetch(url)` ≈ `useAsyncData(url, () => $fetch(url))`. Предпочитайте explicit keys при sharing данных между компонентами.

**Sources:** [nuxt.com/docs/4.x/api/composables/use-fetch](https://nuxt.com/docs/4.x/api/composables/use-fetch)

</details>

<details>
  <summary>useAsyncData</summary>

### useAsyncData

**Definition:** Lower-level composable, оборачивающий **любую async логику** — CMS clients, GraphQL, параллельные `$fetch`, database queries через server utilities.

**How it works:** Первый аргумент — **cache key** (string, computed или omitted для auto-key). Второй — async handler. Передаёт результат в payload как `useFetch`.

**When to use:**

- Non-HTTP data sources (Prisma в server plugin, headless CMS SDK)
- Параллельные fetches с custom merge logic
- Обёртка third-party query layers

**Pluses and minuses:**

| + | − |
|---|---|
| Полный контроль над handler | Нужно вручную задавать stable keys |
| Можно batch `Promise.all` внутри | Не для Pinia side effects (используйте `callOnce`) |
| Тот же return API, что `useFetch` | Чуть больше boilerplate |

**Examples:**

```vue
<script setup lang="ts">
const { data: user } = await useAsyncData(
  'user-profile',
  () => $fetch('/api/me'),
)
</script>
```

```vue
<script setup lang="ts">
const { id } = useRoute().params

const { data } = await useAsyncData(`user:${id}`, () =>
  $fetch(`/api/users/${id}`),
)
</script>
```

```vue
<script setup lang="ts">
const { data: discounts } = await useAsyncData('cart-discount', async (_nuxtApp, { signal }) => {
  const [coupons, offers] = await Promise.all([
    $fetch('/api/coupons', { signal }),
    $fetch('/api/offers', { signal }),
  ])
  return { coupons, offers }
})
</script>
```

**Thoughts:** **Не** используйте `useAsyncData` для trigger Pinia actions — может re-run с nullish values. Используйте `callOnce` для one-time client init.

**Sources:** [nuxt.com/docs/4.x/api/composables/use-async-data](https://nuxt.com/docs/4.x/api/composables/use-async-data)

</details>

<details>
  <summary>useLazyFetch and lazy option</summary>

### useLazyFetch and lazy option

**Definition:** Non-blocking варианты, которые **не** держат навигацию, пока данные не разрешатся. Loading обрабатываете через `status` (`'idle' | 'pending' | 'success' | 'error'`).

**When to use:** Below-the-fold контент, tabs, modals, non-critical виджеты.

**Examples:**

```vue
<script setup lang="ts">
const { data: posts, status } = useLazyFetch('/api/posts')
</script>

<template>
  <div v-if="status === 'pending'">Loading…</div>
  <ul v-else>
    <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
  </ul>
</template>
```

**Thoughts:** `lazy: true` на `useFetch` эквивалентен `useLazyFetch`. На клиенте `await` на lazy call разрешается сразу — обрабатывайте `status` сами.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>server and client-only fetching</summary>

### server and client-only fetching

**Definition:** Опции, контролирующие **где** выполняется handler.

| Option | Behavior |
|--------|----------|
| Default | Fetch на сервере (SSR) и reuse payload на клиенте |
| `server: false` | Пропустить server fetch; выполнить после гидрации только на клиенте |
| `lazy: true` | Не блокировать навигацию; ручной loading UI |

**When to use `server: false`:** Non-SEO виджеты, browser-only API, analytics panels.

**Examples:**

```ts
const articles = await useFetch('/api/articles')

const { data: comments } = useFetch('/api/comments', {
  lazy: true,
  server: false,
})
```

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>callOnce</summary>

### callOnce

**Definition:** **`callOnce`** запускает async function **один раз на запрос** на сервере и **один раз на клиенте** после hydration — без дублирования работы при повторных `useAsyncData` re-runs.

**How it works:**

- Используйте для Pinia store init, analytics bootstrap или plugin side effects, которые не должны запускаться при каждом `useAsyncData` refresh
- Отличается от `useFetch` caching — `callOnce` для imperative setup, не для HTTP payload
- Сочетайте с `useState`, когда результат должен быть reactive и SSR-serialized

**When to use:** One-time client-only init после hydration; server-only setup в plugins. **Не** заменяйте `useFetch` для page data.

**Examples:**

```ts
await callOnce('init-cart', async () => {
  const cart = useCartStore()
  await cart.hydrateFromSession()
})
```

**See also:** [useAsyncData](#useasyncdata) · [useState](#usestate)

**Sources:** [nuxt.com/docs/4.x/api/utils/call-once](https://nuxt.com/docs/4.x/api/utils/call-once)

</details>

<a name="8-caching-payload"></a>

## 8. Caching & payload

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Payload and hydration</summary>

### Payload and hydration

**Definition:** Во время SSR Nuxt сериализует результаты `useFetch` / `useAsyncData` и `useState` в **payload** (`useNuxtApp().payload`), встроенный в HTML или извлечённый в `_payload.json`.

**How it works:** Client hydration читает payload вместо refetch. С **payload extraction** (default) prerendered и ISR routes эмитят `_payload.json` для быстрых client navigations.

**When to use:** Всегда понимайте payload при отладке "double fetch" или stale navigation data.

**Pluses and minuses:**

| + | − |
|---|---|
| Устраняет hydration refetch | Большой payload раздувает HTML TTFB |
| `_payload.json` CDN-cacheable | Static sites: данные устаревают до rebuild |

**Examples:**

```ts
export default defineNuxtConfig({
  experimental: {
    payloadExtraction: 'client', // or true / false
  },
})
```

**Thoughts:** Используйте `pick` или `transform` для уменьшения payload. Custom class instances нуждаются в payload plugins (devalue reducers).

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>Keys and shared state</summary>

### Keys and shared state

**Definition:** `useAsyncData` / `useFetch` кэшируют по **key**. Один key → shared `data`, `error`, `status` refs между компонентами.

**How it works:**

- `useFetch` auto-generates key из URL + options + call location
- `useAsyncData('my-key', handler)` использует explicit key
- `useNuxtData('my-key')` читает cached data без refetch

**When to use explicit keys:** Share fetch между layout и page; refresh конкретных данных через `refreshNuxtData('my-key')`.

**Pluses and minuses:**

| + | − |
|---|---|
| Deduplication в рамках запроса | Mismatched options на одном key → dev warnings |
| Reactive keys через `computed` | Wrong key → duplicate requests |

**Examples:**

```ts
// Share data across components
const { data: users } = useAsyncData('users', () => $fetch('/api/users'))

// Reactive key refetches when id changes
const userId = ref('123')
const { data: user } = useAsyncData(
  computed(() => `user-${userId.value}`),
  () => $fetch(`/api/users/${userId.value}`),
)
```

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>pick, transform, and refresh</summary>

### pick, transform, and refresh

**Definition:** Опции для минимизации размера payload и reshape результатов без изменения upstream API.

| Option | Purpose |
|--------|---------|
| `pick: ['title']` | Только выбранные поля попадают в payload |
| `transform: (data) => …` | Map/filter перед payload serialization |
| `refresh()` / `execute()` | Ручной refetch |
| `clearNuxtData(key)` | Invalidate cached refs глобально |

**Examples:**

```ts
const { data: mountain } = await useFetch('/api/mountains/everest', {
  pick: ['title', 'description'],
})

const { refresh } = await useFetch('/api/posts')
// after mutation:
await refresh()
```

**Thoughts:** `pick`/`transform` не пропускают сетевой fetch — они только уменьшают serialized payload size.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>useState</summary>

### useState

**Definition:** **`useState`** создаёт **SSR-safe reactive state**, keyed по строке — shared между компонентами и serialized в Nuxt payload на сервере, затем rehydrated на клиенте.

**How it works:**

- `const count = useState('counter', () => 0)` — factory выполняется один раз на запрос (server) или один раз на app (client)
- Один key → один ref через layout, page и composables
- Предпочитайте вместо `ref` на module scope (leaks между SSR requests) или raw `localStorage` (без SSR)
- Для server-fetched data предпочитайте `useFetch` / `useAsyncData`; `useState` для UI flags, wizard steps, cross-layout state

**When to use:** Theme toggle, sidebar open state, multi-step form progress, sharing non-HTTP state между layout и page.

**Examples:**

```ts
// composables/useSidebar.ts
export function useSidebar() {
  return useState('sidebar-open', () => false)
}
```

**Pluses and minuses:**

| + | − |
|---|---|
| Payload-serialized, без hydration refetch | Key collisions при generic names |
| Работает в composables без Pinia | Не замена global domain stores |

**Sources:** [nuxt.com/docs/4.x/getting-started/state-management](https://nuxt.com/docs/4.x/getting-started/state-management)

</details>

<details>
  <summary>useCookie</summary>

### useCookie

**Definition:** **`useCookie`** — reactive ref, backed HTTP cookie — readable на server и client, с SSR forwarding из incoming request.

**How it works:**

- `const token = useCookie('auth-token', { maxAge: 60 * 60 * 24 })`
- Server render видит cookie из `Cookie` header; client updates синхронизируются с `document.cookie`
- Options: `httpOnly` (server-only через Nitro), `secure`, `sameSite`, `path`, `domain`
- Для secrets предпочитайте `httpOnly` cookies, установленные в `server/api` handlers, не client-writable tokens

**When to use:** Locale preference, session id, consent flags. Сочетайте с route middleware для auth gates.

**Examples:**

```ts
const locale = useCookie('locale', { default: () => 'en' })

if (import.meta.server) {
  // locale.value available from request cookies during SSR
}
```

**See also:** [Route middleware](./4_nuxt_routing.md#route-middleware) · [Runtime config](./4_nuxt_data.md#runtime-config-and-secrets)

**Sources:** [nuxt.com/docs/4.x/api/composables/use-cookie](https://nuxt.com/docs/4.x/api/composables/use-cookie)

</details>

<details>
  <summary>Route-level caching (ISR / SWR)</summary>

### Route-level caching (ISR / SWR)

**Definition:** Nitro `routeRules` могут кэшировать rendered HTML и API responses — по духу похоже на Next.js ISR.

**How it works:** `isr: 3600` или `swr: 60` на route rule говорит Nitro кэшировать response и revalidate в фоне после TTL.

**When to use:** Полустатические страницы (каталог продуктов, блог) на hybrid deployment без полного static rebuild.

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    '/blog/**': { isr: 3600 },
    '/api/products/**': { swr: 300 },
  },
})
```

**Pluses and minuses:**

| + | − |
|---|---|
| Свежий-ish контент без rebuild | CDN + payload cache complexity |
| Работает на serverless hosts | Нужна настройка TTL |

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server)

</details>

<a name="9-server-routes"></a>

## 9. Server routes

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>server/api endpoints</summary>

### server/api endpoints

**Definition:** Файлы в `server/api/` становятся **Nitro HTTP handlers**, auto-mapped на `/api/*`. Построены на **h3** через `defineEventHandler`.

**How it works:** Filename convention задаёт method и path — `hello.get.ts` → `GET /api/hello`; `users/[id].put.ts` → `PUT /api/users/:id`.

**When to use:** BFF endpoints, webhooks, DB access, file uploads — всё, что никогда не должно попасть в клиентский бандл.

**Pluses and minuses:**

| + | − |
|---|---|
| Коллокация с frontend, HMR в dev | Не замена public API docs, если consumed externally |
| Auto-imports `defineEventHandler` | Serverless cold starts |
| Возвращает JSON, text, streams, redirects | Long-lived WebSockets нуждаются в custom Nitro config |

**Examples:**

```ts
// server/api/hello.get.ts
export default defineEventHandler(() => {
  return { message: 'Hello from Nitro' }
})
```

```ts
// server/api/todos.post.ts
export default defineEventHandler(async (event) => {
  const body = await readBody(event)
  const todo = await db.todo.create({ data: body })
  return todo
})
```

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/hello')
</script>
```

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server)

</details>

<details>
  <summary>Server middleware</summary>

### Server middleware

**Definition:** Файлы в `server/middleware/` выполняются на **каждый matching server request** перед route handlers — auth, logging, CORS.

**How it works:** В отличие от **route middleware** (Vue router, client + SSR navigation), server middleware выполняется только в Nitro — включая `/api/*`, но не при static file serving с CDN.

**When to use:** JWT verification для APIs, request IDs, global error shaping.

**Examples:**

```ts
// server/middleware/auth.ts
export default defineEventHandler((event) => {
  const token = getHeader(event, 'authorization')
  if (!token) {
    throw createError({ statusCode: 401, message: 'Unauthorized' })
  }
})
```

**Thoughts:** Route middleware → [Nuxt — Routing §11](./4_nuxt_routing.md#11-special-files). Не путайте их на интервью.

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server)

</details>

<details>
  <summary>useRequestFetch and forwarded headers</summary>

### useRequestFetch and forwarded headers

**Definition:** На сервере `useFetch` проксирует relevant **client headers и cookies** во internal API calls, чтобы `server/api` видел ту же сессию, что браузер.

**How it works:** `useRequestFetch` оборачивает `$fetch` с incoming request context. Для manual `$fetch` из setup передавайте `useRequestHeaders(['cookie'])` или используйте `useRequestFetch()`.

**When to use:** SSR page, вызывающая `/api/me`, которая читает session cookies.

**Examples:**

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/echo') // cookies forwarded automatically
</script>
```

```ts
// Manual forwarding when not using useFetch
const headers = useRequestHeaders(['cookie'])
const user = await $fetch('/api/me', { headers })
```

**Thoughts:** Никогда не проксируйте все headers во external APIs — `host`, `content-length` и CDN headers unsafe для forwarding.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Runtime config and secrets</summary>

### Runtime config and secrets

**Definition:** `runtimeConfig` в `nuxt.config.ts` экспонирует **public** config клиенту (`runtimeConfig.public`) и **private** server-only значения (API keys, DB URLs).

**When to use:** Любой секрет — никогда не хардкодьте в Vue components, попадающих в браузер.

**Examples:**

```ts
export default defineNuxtConfig({
  runtimeConfig: {
    apiSecret: '', // server only — override with NUXT_API_SECRET env
    public: {
      apiBase: '/api',
    },
  },
})
```

```ts
// server/api/data.get.ts
export default defineEventHandler(() => {
  const config = useRuntimeConfig()
  return $fetch('https://api.example.com', {
    headers: { Authorization: `Bearer ${config.apiSecret}` },
  })
})
```

**Sources:** [nuxt.com/docs/4.x/guide/going-further/runtime-config](https://nuxt.com/docs/4.x/guide/going-further/runtime-config)

</details>
