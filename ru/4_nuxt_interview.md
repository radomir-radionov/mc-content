# Nuxt — Interview

Вопросы и ответы на интервью и задачи по кодированию.

[← Вернуться в hub](./4_nuxt.md)

### Related docs

- [Nuxt — Ядро](./4_nuxt_core.md) · [Nuxt — Данные](./4_nuxt_data.md) · [Nuxt — Маршрутизация](./4_nuxt_routing.md) · [Nuxt — Платформа](./4_nuxt_platform.md) · [Nuxt — Интервью](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [Interview Practice](#interview-practice)
- [Questions & Answers](#questions-answers)
- [Coding tasks](#coding-tasks)

---

## Interview Practice

Вопросы для Nuxt-интервью. Попробуйте ответить, прежде чем открывать решения.

**Definition:** Подобранные Q&A и coding tasks, охватывающие SSR/SSG, data fetching composables, Nitro server routes, middleware и deployment.

*Interview answers follow: Definition → comparison table → When to use → Example → Notes → Source.*

### Question index

| Topic | Answer |
|-------|--------|
| SSR vs SSG vs CSR vs hybrid | Q&A |
| `useFetch` vs `useAsyncData` vs `$fetch` | Q&A |
| Route vs server middleware | Q&A |
| `routeRules` and prerendering | Q&A |
| `useSeoMeta` and SEO | Q&A |
| Nitro and deployment presets | Q&A |
| Coding tasks 1–3 | Tasks |

---

## Questions & Answers

<details>
  <summary>Interview Practice</summary>

<a name="ssr-vs-ssg-vs-csr-vs-hybrid-rendering"></a>

### Interview Practice

**Definition:** Четыре способа доставки HTML в браузер — на интервью проверяют, знаете ли вы **когда** каждый выполняется и какой Nuxt API его запускает.

| Mode | When HTML is built | Nuxt trigger | Use case |
|------|-------------------|--------------|----------|
| **CSR** | Client after JS loads | `ssr: false` globally or `routeRules: { '/x': { ssr: false } }` | Internal tools, no SEO |
| **SSR** | Every request on server | Default (`ssr: true`) | Personalized, session-based pages |
| **SSG** | Build time | `nuxt generate`, `routeRules.prerender`, `nitro.prerender` | Marketing, docs, blogs |
| **Hybrid** | Mix per route | `routeRules` (`prerender`, `isr`, `swr`, `ssr`) | Static landing + dynamic dashboard |
| **ISR/SWR** (bonus) | Cached + periodic refresh | `routeRules: { '/blog/**': { isr: 3600 } }` | Catalogs that change occasionally |

| Pros | Cons |
|------|------|
| SSG — fastest CDN delivery | Stale until rebuild (unless ISR) |
| SSR — always current | Server cost per request |
| CSR — simplest client model | Poor SEO, slow first paint |
| Hybrid — best of both | `routeRules` misconfig → surprises |

**When to use:**

- Marketing `/` → **prerender**
- User dashboard → **SSR** (cookies/session)
- Admin chart polling → **CSR** leaf or `server: false` fetch
- Blog with hourly freshness → **ISR** на `/blog/**`

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    '/': { prerender: true },
    '/dashboard/**': { ssr: true },
    '/blog/**': { isr: 3600 },
  },
})
```

**Thoughts:** Nuxt сериализует данные `useFetch` в **payload** для всех режимов, кроме pure client fetches. Упомяните `_payload.json` для static/ISR navigations.

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>Question index</summary>

<a name="usefetch-vs-useasyncdata-vs-fetch"></a>

### Question index

**Definition:** Три data API — ответ на интервью должен указать SSR payload behavior и когда каждый уместен.


| | `$fetch` | `useFetch` | `useAsyncData` |
| | --- | ---------- | ------------ |
| | **Wrapper** | ofetch directly | `useAsyncData` + `$fetch` | Generic async handler |
| | **SSR payload** | No (alone) | Yes | Yes |
| | **Typical use** | Event handlers, server/api | REST в `setup` | CMS, DB, parallel custom logic |
| | **Key** | N/A | URL-based (override with `key`) | Explicit string/computed key |
| | **Returns** | Promise | `{ data, status, error, refresh }` | Same |

| Pros | Cons |
|------|------|
| `useFetch` — one-liner SSR pages | Double fetch если raw `$fetch` в setup |
| `useAsyncData` — any async source | Нужно выбирать stable keys |
| `$fetch` — simple imperative calls | No loading refs without wrapper |

**When to use:**

- Page initial data → **`await useFetch('/api/x')`**
- Prisma/CMS в setup → **`useAsyncData('key', () => …)`**
- Button submit → **`$fetch` with method/body**

**Examples:**

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/posts')

async function submit() {
  await $fetch('/api/posts', { method: 'POST', body: form })
  await refreshNuxtData()
}
</script>
```

**Thoughts:** `useFetch(url)` ≈ `useAsyncData(url, () => $fetch(url))`. Hydration details → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>SSR vs SSG vs CSR vs hybrid rendering?</summary>

<a name="server-actions-vs-nitro-routes"></a>

### SSR vs SSG vs CSR vs hybrid rendering?

**Answer:** В Nuxt **нет** React-style **Server Actions** (`"use server"` colocated в components). Mutations используют **`server/api`** Nitro handlers + **`$fetch`** с клиента, или form posts на API routes.


| | Next.js Server Actions | Nuxt pattern |
| | --- | ------------------------ |
| | **Definition** | RPC-like functions в React components | HTTP handlers в `server/api/` |
| | **Colocation** | Same file as UI | Separate `server/` tree |
| | **Client call** | `action()` prop или `useActionState` | `$fetch('/api/todos', { method: 'POST', body })` |
| | **SSR data** | RSC + Server Actions | `useFetch` / `useAsyncData` + payload |

**When to use Nitro routes:** CRUD APIs, webhooks, DB access, всё, что не должно попасть в client bundle. Refresh UI через `refreshNuxtData()` или mutate local state после успешного `$fetch`.

**See also:** [Next.js — Server Actions](./4_nextjs_data.md#server-actions-vs-route-handlers) · [server/api endpoints](./4_nuxt_data.md#serverapi-endpoints)

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server)

</details>

<details>
  <summary>useFetch vs useAsyncData vs $fetch?</summary>

<a name="route-middleware-vs-server-middleware"></a>

### useFetch vs useAsyncData vs $fetch?

**Definition:** Две middleware-системы с похожими именами — выполняются в разных runtime для разных целей.


| | Route middleware | Server middleware |
| | --- | ------------------ |
| | **Path** | `app/middleware/` | `server/middleware/` |
| | **Runtime** | Vue app (SSR + client nav) | Nitro only |
| | **Runs on `/api/*`** | No | Yes |
| | **API** | `defineNuxtRouteMiddleware` | `defineEventHandler` |
| | **Redirect** | `return navigateTo('/login')` | `sendRedirect(event, '/login')` |
| | **Typical use** | UX auth gate | API JWT, logging, CORS |

| Pros | Cons |
|------|------|
| Route MW — smooth client navigations | Does not secure API alone |
| Server MW — real request interception | Not run on CDN-served static HTML |

**When to use both:** Route middleware для redirect неаутентифицированных users; server middleware + handler checks для API authorization.

**Examples:**

```ts
// app/middleware/auth.ts
export default defineNuxtRouteMiddleware(() => {
  if (!useUser().value) return navigateTo('/login')
})
```

```ts
// server/middleware/auth.ts
export default defineEventHandler((event) => {
  if (!getHeader(event, 'authorization')) {
    throw createError({ statusCode: 401 })
  }
})
```

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>Server Actions vs Nitro routes?</summary>

<a name="routerules-and-prerendering"></a>

### Server Actions vs Nitro routes?

**Definition:** Declarative per-route rendering и caching в `nuxt.config.ts`, скомпилированные Nitro.

**How it works:**

- `prerender: true` — static HTML на build (crawler или explicit routes)
- `nuxt generate` — crawl от `/`, follow `<a href>`, emit `_payload.json`
- Unlinked dynamic routes нуждаются в `nitro.prerender.routes` или `prerender:routes` hook

**When to use:**

- Known static paths → `prerender` or `generate`
- CMS slugs unknown at config time → `prerender:routes` hook fetching slugs
- Skip sensitive paths → `nitro.prerender.ignore`

**Examples:**

```ts
export default defineNuxtConfig({
  nitro: {
    prerender: {
      crawlLinks: true,
      routes: ['/sitemap.xml'],
      ignore: ['/admin'],
    },
  },
  routeRules: {
    '/blog/**': { prerender: true },
  },
})
```

**Thoughts:** Crawler находит только `<a href>` links — JS-only navigation graphs не prerender.

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>Route middleware vs server middleware?</summary>

### Route middleware vs server middleware?

**Definition:** Type-safe SEO composable (Unhead) для title, description, Open Graph и Twitter tags — reactive и SSR-friendly.

**How it works:** Tags рендерятся в server HTML при SSR/prerender, чтобы crawlers видели meta без выполнения JS.

| Tool | Use |
|------|-----|
| `nuxt.config` `app.head` | Static defaults (lang, favicon) |
| `useHead` в `app.vue` | `titleTemplate` |
| `useSeoMeta` в pages | Per-route SEO |
| `<Head>` components | Template-style head tags |

**Examples:**

```vue
<script setup lang="ts">
const title = ref('Product Page')
useSeoMeta({
  title,
  description: 'Buy the best widgets',
  ogImage: 'https://example.com/og.png',
})
</script>
```

**Thoughts:** Pair с prerender/SSR для content sites; CSR-only pages hurt SEO.

**Sources:** [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta)

</details>

<details>
  <summary>routeRules и prerendering?</summary>

### routeRules и prerendering?

**Definition:** Nitro собирает `.output/` для Node, static или serverless/edge presets.

| Target | Command / preset | Output |
|--------|------------------|--------|
| Node VPS | default / `node-server` | `.output/server/index.mjs` |
| Static CDN | `nuxt generate` | `.output/public/` |
| Vercel | `NITRO_PRESET=vercel` | Serverless functions |
| Cloudflare | `cloudflare-pages` / Workers | Worker-compatible bundle |

**When to use:** Match preset к host; set `app.baseURL` для subpath deploys.

**Examples:**

```bash
NODE_ENV=production node .output/server/index.mjs
NITRO_PRESET=vercel nuxt build
```

**Thoughts:** No vendor lock-in — тот же Nuxt app может сменить presets с rebuild. Упомяните `runtimeConfig` для env-specific secrets per environment.

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

---

## Coding tasks

<details>
  <summary>useSeoMeta и SEO в Nuxt?</summary>

### useSeoMeta и SEO в Nuxt?

**Definition:** Practice scenario — product page fetches twice (network tab показывает duplicate `/api/products` on load).

**Scenario:** Developer used `$fetch` directly в `setup`. HTML renders, then client fetches again during hydration.

| Symptom | Likely cause |
|---------|--------------|
| Duplicate API call on page load | `$fetch` в setup without payload forwarding |
| Flash of empty state | Client refetch after populated SSR |

**What to change:**

```vue
<!-- ❌ Before -->
<script setup lang="ts">
const products = ref([])
products.value = await $fetch('/api/products')
</script>
```

```vue
<!-- ✅ After -->
<script setup lang="ts">
const { data: products } = await useFetch('/api/products')
</script>
```

<details>
  <summary>Ответ</summary>

**Root cause:** `$fetch` не сериализует results в Nuxt **payload**, поэтому client повторяет request during hydration.

**Fix:** Replace с `useFetch` или `useAsyncData('products', () => $fetch('/api/products'))`. Verify в Nuxt DevTools Payload tab — data should appear once.

**Follow-up interview points:** When is `$fetch` still correct? (event handlers, `server/api` handlers, inside `useAsyncData` handler).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

</details>

<details>
  <summary>Nitro и deployment presets?</summary>

### Nitro и deployment presets?

**Definition:** Practice scenario — `/dashboard` must redirect unauthenticated users to `/login` on both SSR and client navigation; `/api/dashboard` must reject missing tokens.

**Scenario:** Implement route middleware + server API guard.

| Requirement | Implementation |
|-------------|----------------|
| Redirect guests | `app/middleware/auth.ts` + `definePageMeta` |
| SSR-safe session read | `useRequestHeaders(['cookie'])` or session composable |
| Secure API | `server/middleware` or per-handler check |

**Examples:**

```ts
// app/middleware/auth.ts
export default defineNuxtRouteMiddleware(() => {
  const user = useUser()
  if (!user.value) return navigateTo('/login')
})
```

```vue
<!-- app/pages/dashboard.vue -->
<script setup lang="ts">
definePageMeta({ middleware: 'auth' })
</script>
```

```ts
// server/api/dashboard.get.ts
export default defineEventHandler((event) => {
  const token = getCookie(event, 'session')
  if (!token) throw createError({ statusCode: 401 })
  return { stats: [] }
})
```

<details>
  <summary>Ответ</summary>

Route middleware handles **UX redirects** для page navigations. API routes need **server-side** checks — middleware alone is insufficient because `/api/dashboard` is callable directly.

Populate `useUser()` from composable, который reads session via `useFetch('/api/me')` on SSR (cookies forwarded automatically).

**Security note:** Never trust client-only guards for sensitive data.

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

</details>

<details>
  <summary>routeRules и prerendering?</summary>

### routeRules и prerendering?

**Definition:** Practice scenario — blog posts live at `/blog/[slug]` from CMS. `nuxt generate` only prerenders `/blog` because slugs are not linked yet.

| Tool | Scope |
|------|-------|
| Crawler | Only follows existing `<a href>` in HTML |
| `nitro.prerender.routes` | Explicit paths at build time |
| `prerender:routes` hook | Fetch slugs from CMS API during build |

**Examples:**

```ts
export default defineNuxtConfig({
  hooks: {
    async 'prerender:routes'(ctx) {
      const { posts } = await $fetch('https://cms.example.com/posts')
      for (const post of posts) {
        ctx.routes.add(`/blog/${post.slug}`)
      }
    },
  },
  routeRules: {
    '/blog/**': { prerender: true },
  },
})
```

<details>
  <summary>Ответ</summary>

Nitro crawler cannot invent CMS slugs — register them в **`prerender:routes`** hook (or static `nitro.prerender.routes` list). Ensure `/blog` page links to posts for crawl-based discovery as backup.

After `nuxt generate`, confirm `.output/public/blog/<slug>/index.html` and `_payload.json` exist.

**Interview follow-up:** What happens to blog data on client navigation for static deploy? Payload from build time — data stale until rebuild unless using ISR.

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

</details>

---
