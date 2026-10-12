# Nuxt — Data

`useFetch`, `useAsyncData`, caching, and server routes.

[← Back to hub](./4_nuxt.md)

### Related docs

- [Nuxt — Core](./4_nuxt_core.md) · [Nuxt — Data](./4_nuxt_data.md) · [Nuxt — Routing](./4_nuxt_routing.md) · [Nuxt — Platform](./4_nuxt_platform.md) · [Nuxt — Interview](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [7. Data fetching](#7-data-fetching)
- [8. Caching & payload](#8-caching-payload)
- [9. Server routes](#9-server-routes)

---

<a name="7-data-fetching"></a>

## 7. Data fetching

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>$fetch</summary>

### $fetch

**Definition:** Nuxt auto-imports **`$fetch`** — a thin wrapper around [ofetch](https://github.com/unjs/ofetch) for HTTP requests. Works in Vue setup, Nitro handlers, and plugins.

**How it works:** Unlike `useFetch`, `$fetch` does **not** forward server results to the client payload. Calling it directly in `setup` during SSR can cause **double fetch** (server + client hydration).

**When to use:**

- Client-side event handlers (form submit, button click)
- Nitro `server/api` handlers calling external APIs
- Combined inside `useAsyncData` for SSR-safe initial load

**Pluses and minuses:**

| +                             | −                                    |
| ----------------------------- | ------------------------------------ |
| Simple imperative API         | No automatic SSR deduplication alone |
| Typed responses with generics | Easy to cause hydration double-fetch |
| Works isomorphically          | No shared loading `status` ref       |

**Examples:**

```vue
<script setup lang="ts">
async function addTodo() {
  await $fetch("/api/todos", {
    method: "POST",
    body: { title: "Learn Nuxt" },
  });
}
</script>
```

```ts
// server/api/echo.get.ts
export default defineEventHandler((event) => {
  return { path: getRequestURL(event).pathname };
});
```

**Thoughts:** Rule of thumb: **`useFetch` / `useAsyncData` for setup initial data**; **`$fetch` for user-triggered requests**.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>useFetch</summary>

### useFetch

**Definition:** SSR-safe composable wrapping `$fetch`. Ensures data fetched on the server is **serialized into the payload** so the client does not refetch during hydration.

**How it works:** On server render, Nuxt runs the fetch, stores result in `useNuxtApp().payload`, and hydrates `data` ref on the client without a second network call (for the same key/URL).

**When to use:** Default for loading REST/JSON into pages from `server/api` or external URLs.

**Pluses and minuses:**

| +                                                    | −                                                         |
| ---------------------------------------------------- | --------------------------------------------------------- |
| One-liner SSR + client parity                        | URL-based keys differ per call site unless explicit `key` |
| Returns `data`, `status`, `error`, `refresh`         | Less control than `useAsyncData` for non-HTTP sources     |
| Auto proxies cookies on server via `useRequestFetch` | External API header proxying needs care                   |

**Examples:**

```vue
<script setup lang="ts">
const { data: count } = await useFetch("/api/count");
</script>

<template>
  <p>Visits: {{ count }}</p>
</template>
```

```vue
<script setup lang="ts">
const route = useRoute();
const { data: post } = await useFetch(`/api/posts/${route.params.id}`);
</script>
```

**Thoughts:** `useFetch(url)` ≈ `useAsyncData(url, () => $fetch(url))`. Prefer explicit keys when sharing data across components.

**Sources:** [nuxt.com/docs/4.x/api/composables/use-fetch](https://nuxt.com/docs/4.x/api/composables/use-fetch)

</details>

<details>
  <summary>useAsyncData</summary>

### useAsyncData

**Definition:** Lower-level composable wrapping **any async logic** — CMS clients, GraphQL, multiple parallel `$fetch` calls, database queries via server utilities.

**How it works:** First argument is a **cache key** (string, computed, or omitted for auto-key). Second argument is the async handler. Forwards result to payload like `useFetch`.

**When to use:**

- Non-HTTP data sources (Prisma in server plugin, headless CMS SDK)
- Parallel fetches with custom merge logic
- Wrapping third-party query layers

**Pluses and minuses:**

| +                              | −                                           |
| ------------------------------ | ------------------------------------------- |
| Full control over handler      | Must supply stable keys manually            |
| Can batch `Promise.all` inside | Not for Pinia side effects (use `callOnce`) |
| Same return API as `useFetch`  | Slightly more boilerplate                   |

**Examples:**

```vue
<script setup lang="ts">
const { data: user } = await useAsyncData("user-profile", () =>
  $fetch("/api/me"),
);
</script>
```

```vue
<script setup lang="ts">
const { id } = useRoute().params;

const { data } = await useAsyncData(`user:${id}`, () =>
  $fetch(`/api/users/${id}`),
);
</script>
```

```vue
<script setup lang="ts">
const { data: discounts } = await useAsyncData(
  "cart-discount",
  async (_nuxtApp, { signal }) => {
    const [coupons, offers] = await Promise.all([
      $fetch("/api/coupons", { signal }),
      $fetch("/api/offers", { signal }),
    ]);
    return { coupons, offers };
  },
);
</script>
```

**Thoughts:** Do **not** use `useAsyncData` to trigger Pinia actions — can re-run with nullish values. Use `callOnce` for one-time client init instead.

**Sources:** [nuxt.com/docs/4.x/api/composables/use-async-data](https://nuxt.com/docs/4.x/api/composables/use-async-data)

</details>

<details>
  <summary>useLazyFetch and lazy option</summary>

### useLazyFetch and lazy option

**Definition:** Non-blocking variants that do **not** hold navigation until data resolves. You handle loading via `status` (`'idle' | 'pending' | 'success' | 'error'`).

**When to use:** Below-the-fold content, tabs, modals, non-critical widgets.

**Examples:**

```vue
<script setup lang="ts">
const { data: posts, status } = useLazyFetch("/api/posts");
</script>

<template>
  <div v-if="status === 'pending'">Loading…</div>
  <ul v-else>
    <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
  </ul>
</template>
```

**Thoughts:** `lazy: true` on `useFetch` is equivalent to `useLazyFetch`. On client, `await` on a lazy call resolves immediately — handle `status` yourself.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>server and client-only fetching</summary>

### server and client-only fetching

**Definition:** Options controlling **where** the handler runs.

| Option          | Behavior                                              |
| --------------- | ----------------------------------------------------- |
| Default         | Fetch on server (SSR) and reuse payload on client     |
| `server: false` | Skip server fetch; run after hydration on client only |
| `lazy: true`    | Don't block navigation; manual loading UI             |

**When to use `server: false`:** Non-SEO widgets, browser-only APIs, analytics panels.

**Examples:**

```ts
const articles = await useFetch("/api/articles");

const { data: comments } = useFetch("/api/comments", {
  lazy: true,
  server: false,
});
```

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>callOnce</summary>

### callOnce

**Definition:** **`callOnce`** runs an async function **once per request** on the server and **once on the client** after hydration — without duplicating work across `useAsyncData` re-runs.

**How it works:**

- Use for Pinia store init, analytics bootstrap, or plugin side effects that must not run on every `useAsyncData` refresh
- Distinct from `useFetch` caching — `callOnce` is for imperative setup, not HTTP payload
- Pair with `useState` when the result should be reactive and SSR-serialized

**When to use:** One-time client-only init after hydration; server-only setup in plugins. **Do not** replace `useFetch` for page data.

**Examples:**

```ts
await callOnce("init-cart", async () => {
  const cart = useCartStore();
  await cart.hydrateFromSession();
});
```

**See also:** [useAsyncData](#useasyncdata) · [useState](#usestate)

**Sources:** [nuxt.com/docs/4.x/api/utils/call-once](https://nuxt.com/docs/4.x/api/utils/call-once)

</details>

<a name="8-caching-payload"></a>

## 8. Caching & payload

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Payload and hydration</summary>

### Payload and hydration

**Definition:** During SSR, Nuxt serializes `useFetch` / `useAsyncData` results and `useState` into a **payload** (`useNuxtApp().payload`) embedded in HTML or extracted to `_payload.json`.

**How it works:** Client hydration reads payload instead of refetching. With **payload extraction** enabled (default), prerendered and ISR routes emit `_payload.json` for faster client navigations.

**When to use:** Always understand payload when debugging "double fetch" or stale navigation data.

**Pluses and minuses:**

| +                             | −                                      |
| ----------------------------- | -------------------------------------- |
| Eliminates hydration refetch  | Large payload inflates HTML TTFB       |
| `_payload.json` CDN-cacheable | Static sites: data stale until rebuild |

**Examples:**

```ts
export default defineNuxtConfig({
  experimental: {
    payloadExtraction: "client", // or true / false
  },
});
```

**Thoughts:** Use `pick` or `transform` to shrink payload. Custom class instances need payload plugins (devalue reducers).

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>Keys and shared state</summary>

### Keys and shared state

**Definition:** `useAsyncData` / `useFetch` cache by **key**. Same key → shared `data`, `error`, `status` refs across components.

**How it works:**

- `useFetch` auto-generates key from URL + options + call location
- `useAsyncData('my-key', handler)` uses explicit key
- `useNuxtData('my-key')` reads cached data without refetching

**When to use explicit keys:** Share fetch between layout and page; refresh specific data with `refreshNuxtData('my-key')`.

**Pluses and minuses:**

| +                            | −                                             |
| ---------------------------- | --------------------------------------------- |
| Deduplication within request | Mismatched options on same key → dev warnings |
| Reactive keys via `computed` | Wrong key → duplicate requests                |

**Examples:**

```ts
// Share data across components
const { data: users } = useAsyncData("users", () => $fetch("/api/users"));

// Reactive key refetches when id changes
const userId = ref("123");
const { data: user } = useAsyncData(
  computed(() => `user-${userId.value}`),
  () => $fetch(`/api/users/${userId.value}`),
);
```

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>pick, transform, and refresh</summary>

### pick, transform, and refresh

**Definition:** Options to minimize payload size and reshape results without changing the upstream API.

| Option                    | Purpose                                 |
| ------------------------- | --------------------------------------- |
| `pick: ['title']`         | Only selected fields enter payload      |
| `transform: (data) => …`  | Map/filter before payload serialization |
| `refresh()` / `execute()` | Manually refetch                        |
| `clearNuxtData(key)`      | Invalidate cached refs globally         |

**Examples:**

```ts
const { data: mountain } = await useFetch("/api/mountains/everest", {
  pick: ["title", "description"],
});

const { refresh } = await useFetch("/api/posts");
// after mutation:
await refresh();
```

**Thoughts:** `pick`/`transform` do not skip the network fetch — they only reduce serialized payload size.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>useState</summary>

### useState

**Definition:** **`useState`** creates **SSR-safe reactive state** keyed by string — shared across components and serialized into the Nuxt payload on the server, then rehydrated on the client.

**How it works:**

- `const count = useState('counter', () => 0)` — factory runs once per request (server) or once per app (client)
- Same key → same ref across layout, page, and composables
- Prefer over `ref` at module scope (leaks between SSR requests) or raw `localStorage` (no SSR)
- For server-fetched data, prefer `useFetch` / `useAsyncData`; use `useState` for UI flags, wizard steps, cross-layout state

**When to use:** Theme toggle, sidebar open state, multi-step form progress, sharing non-HTTP state between layout and page.

**Examples:**

```ts
// composables/useSidebar.ts
export function useSidebar() {
  return useState("sidebar-open", () => false);
}
```

**Pluses and minuses:**

| +                                        | −                                          |
| ---------------------------------------- | ------------------------------------------ |
| Payload-serialized, no hydration refetch | Key collisions if names are generic        |
| Works in composables without Pinia       | Not a replacement for global domain stores |

**Sources:** [nuxt.com/docs/4.x/getting-started/state-management](https://nuxt.com/docs/4.x/getting-started/state-management)

</details>

<details>
  <summary>useCookie</summary>

### useCookie

**Definition:** **`useCookie`** is a reactive ref backed by an HTTP cookie — readable on server and client, with SSR forwarding from the incoming request.

**How it works:**

- `const token = useCookie('auth-token', { maxAge: 60 * 60 * 24 })`
- Server render sees cookie from `Cookie` header; client updates sync to `document.cookie`
- Options: `httpOnly` (server-only via Nitro), `secure`, `sameSite`, `path`, `domain`
- For secrets, prefer `httpOnly` cookies set in `server/api` handlers, not client-writable tokens

**When to use:** Locale preference, session id, consent flags. Pair with route middleware for auth gates.

**Examples:**

```ts
const locale = useCookie("locale", { default: () => "en" });

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

**Definition:** Nitro `routeRules` can cache rendered HTML and API responses — similar in spirit to Next.js ISR.

**How it works:** `isr: 3600` or `swr: 60` on a route rule tells Nitro to cache the response and revalidate in the background after TTL.

**When to use:** Semi-static pages (product catalog, blog) on a hybrid deployment without full static rebuild.

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    "/blog/**": { isr: 3600 },
    "/api/products/**": { swr: 300 },
  },
});
```

**Pluses and minuses:**

| +                                 | −                              |
| --------------------------------- | ------------------------------ |
| Fresh-ish content without rebuild | CDN + payload cache complexity |
| Works on serverless hosts         | TTL tuning required            |

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server)

</details>

<a name="9-server-routes"></a>

## 9. Server routes

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>server/api endpoints</summary>

### server/api endpoints

**Definition:** Files in `server/api/` become **Nitro HTTP handlers** auto-mapped to `/api/*`. Built on **h3** via `defineEventHandler`.

**How it works:** Filename convention sets method and path — `hello.get.ts` → `GET /api/hello`; `users/[id].put.ts` → `PUT /api/users/:id`.

**When to use:** BFF endpoints, webhooks, DB access, file uploads — anything that must never ship to the client bundle.

**Pluses and minuses:**

| +                                      | −                                                           |
| -------------------------------------- | ----------------------------------------------------------- |
| Colocated with frontend, HMR in dev    | Not a substitute for public API docs if consumed externally |
| Auto-imports `defineEventHandler`      | Serverless cold starts                                      |
| Returns JSON, text, streams, redirects | Long-lived WebSockets need custom Nitro config              |

**Examples:**

```ts
// server/api/hello.get.ts
export default defineEventHandler(() => {
  return { message: "Hello from Nitro" };
});
```

```ts
// server/api/todos.post.ts
export default defineEventHandler(async (event) => {
  const body = await readBody(event);
  const todo = await db.todo.create({ data: body });
  return todo;
});
```

```vue
<script setup lang="ts">
const { data } = await useFetch("/api/hello");
</script>
```

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server)

</details>

<details>
  <summary>Server middleware</summary>

### Server middleware

**Definition:** Files in `server/middleware/` run on **every matching server request** before route handlers — auth, logging, CORS.

**How it works:** Unlike **route middleware** (Vue router, client + SSR navigation), server middleware runs in Nitro only — including `/api/*` but not during static file serving from CDN.

**When to use:** JWT verification for APIs, request IDs, global error shaping.

**Examples:**

```ts
// server/middleware/auth.ts
export default defineEventHandler((event) => {
  const token = getHeader(event, "authorization");
  if (!token) {
    throw createError({ statusCode: 401, message: "Unauthorized" });
  }
});
```

**Thoughts:** Route middleware → [Nuxt — Routing §11](./4_nuxt_routing.md#11-special-files). Do not confuse the two in interviews.

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server)

</details>

<details>
  <summary>useRequestFetch and forwarded headers</summary>

### useRequestFetch and forwarded headers

**Definition:** On the server, `useFetch` proxies relevant **client headers and cookies** to internal API calls so `server/api` sees the same session as the browser.

**How it works:** `useRequestFetch` wraps `$fetch` with incoming request context. For manual `$fetch` from setup, pass `useRequestHeaders(['cookie'])` or use `useRequestFetch()`.

**When to use:** SSR page calling `/api/me` that reads session cookies.

**Examples:**

```vue
<script setup lang="ts">
const { data } = await useFetch("/api/echo"); // cookies forwarded automatically
</script>
```

```ts
// Manual forwarding when not using useFetch
const headers = useRequestHeaders(["cookie"]);
const user = await $fetch("/api/me", { headers });
```

**Thoughts:** Never proxy all headers to external APIs — `host`, `content-length`, and CDN headers are unsafe to forward.

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Runtime config and secrets</summary>

### Runtime config and secrets

**Definition:** `runtimeConfig` in `nuxt.config.ts` exposes **public** config to client (`runtimeConfig.public`) and **private** server-only values (API keys, DB URLs).

**When to use:** Any secret — never hardcode in Vue components shipped to the browser.

**Examples:**

```ts
export default defineNuxtConfig({
  runtimeConfig: {
    apiSecret: "", // server only — override with NUXT_API_SECRET env
    public: {
      apiBase: "/api",
    },
  },
});
```

```ts
// server/api/data.get.ts
export default defineEventHandler(() => {
  const config = useRuntimeConfig();
  return $fetch("https://api.example.com", {
    headers: { Authorization: `Bearer ${config.apiSecret}` },
  });
});
```

**Sources:** [nuxt.com/docs/4.x/guide/going-further/runtime-config](https://nuxt.com/docs/4.x/guide/going-further/runtime-config)

</details>
