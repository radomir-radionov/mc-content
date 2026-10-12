# Nuxt — Interview

Interview Q&A and coding tasks.

[← Back to hub](./4_nuxt.md)

### Related docs

- [Nuxt — Core](./4_nuxt_core.md) · [Nuxt — Data](./4_nuxt_data.md) · [Nuxt — Routing](./4_nuxt_routing.md) · [Nuxt — Platform](./4_nuxt_platform.md) · [Nuxt — Interview](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [Interview Practice](#interview-practice)
- [Questions & Answers](#questions-answers)
- [Coding tasks](#coding-tasks)

---

## Interview Practice

Questions for Nuxt interviews. Try answering before opening solutions.

**Definition:** Curated Q&A and coding tasks covering SSR/SSG, data fetching composables, Nitro server routes, middleware, and deployment.

_Interview answers follow: Definition → comparison table → When to use → Example → Notes → Source._

### Question index

| Topic                                    | Answer |
| ---------------------------------------- | ------ |
| SSR vs SSG vs CSR vs hybrid              | Q&A    |
| `useFetch` vs `useAsyncData` vs `$fetch` | Q&A    |
| Route vs server middleware               | Q&A    |
| `routeRules` and prerendering            | Q&A    |
| `useSeoMeta` and SEO                     | Q&A    |
| Nitro and deployment presets             | Q&A    |
| Coding tasks 1–3                         | Tasks  |

---

## Questions & Answers

<details>
  <summary>SSR vs SSG vs CSR vs hybrid rendering?</summary>

<a name="ssr-vs-ssg-vs-csr-vs-hybrid-rendering"></a>

### SSR vs SSG vs CSR vs hybrid rendering?

**Definition:** Four ways HTML reaches the browser — the interview tests whether you know **when** each runs and which Nuxt API triggers it.

| Mode                | When HTML is built        | Nuxt trigger                                                    | Use case                           |
| ------------------- | ------------------------- | --------------------------------------------------------------- | ---------------------------------- |
| **CSR**             | Client after JS loads     | `ssr: false` globally or `routeRules: { '/x': { ssr: false } }` | Internal tools, no SEO             |
| **SSR**             | Every request on server   | Default (`ssr: true`)                                           | Personalized, session-based pages  |
| **SSG**             | Build time                | `nuxt generate`, `routeRules.prerender`, `nitro.prerender`      | Marketing, docs, blogs             |
| **Hybrid**          | Mix per route             | `routeRules` (`prerender`, `isr`, `swr`, `ssr`)                 | Static landing + dynamic dashboard |
| **ISR/SWR** (bonus) | Cached + periodic refresh | `routeRules: { '/blog/**': { isr: 3600 } }`                     | Catalogs that change occasionally  |

| Pros                        | Cons                               |
| --------------------------- | ---------------------------------- |
| SSG — fastest CDN delivery  | Stale until rebuild (unless ISR)   |
| SSR — always current        | Server cost per request            |
| CSR — simplest client model | Poor SEO, slow first paint         |
| Hybrid — best of both       | `routeRules` misconfig → surprises |

**When to use:**

- Marketing `/` → **prerender**
- User dashboard → **SSR** (cookies/session)
- Admin chart polling → **CSR** leaf or `server: false` fetch
- Blog with hourly freshness → **ISR** on `/blog/**`

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    "/": { prerender: true },
    "/dashboard/**": { ssr: true },
    "/blog/**": { isr: 3600 },
  },
});
```

**Thoughts:** Nuxt serializes `useFetch` data into **payload** for all modes except pure client fetches. Mention `_payload.json` for static/ISR navigations.

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>useFetch vs useAsyncData vs $fetch?</summary>

<a name="usefetch-vs-useasyncdata-vs-fetch"></a>

### useFetch vs useAsyncData vs $fetch?

**Definition:** Three data APIs — interview answer should state SSR payload behavior and when each is appropriate.

|                 | `$fetch`                   | `useFetch`                         | `useAsyncData`                 |
| --------------- | -------------------------- | ---------------------------------- | ------------------------------ |
| **Wrapper**     | ofetch directly            | `useAsyncData` + `$fetch`          | Generic async handler          |
| **SSR payload** | No (alone)                 | Yes                                | Yes                            |
| **Typical use** | Event handlers, server/api | REST in `setup`                    | CMS, DB, parallel custom logic |
| **Key**         | N/A                        | URL-based (override with `key`)    | Explicit string/computed key   |
| **Returns**     | Promise                    | `{ data, status, error, refresh }` | Same                           |

| Pros                               | Cons                                          |
| ---------------------------------- | --------------------------------------------- |
| `useFetch` — one-liner SSR pages   | Double fetch if you use raw `$fetch` in setup |
| `useAsyncData` — any async source  | Must choose stable keys                       |
| `$fetch` — simple imperative calls | No loading refs without wrapper               |

**When to use:**

- Page initial data → **`await useFetch('/api/x')`**
- Prisma/CMS in setup → **`useAsyncData('key', () => …)`**
- Button submit → **`$fetch` with method/body**

**Examples:**

```vue
<script setup lang="ts">
const { data } = await useFetch("/api/posts");

async function submit() {
  await $fetch("/api/posts", { method: "POST", body: form });
  await refreshNuxtData();
}
</script>
```

**Thoughts:** `useFetch(url)` ≈ `useAsyncData(url, () => $fetch(url))`. Hydration details → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Server Actions vs Nitro routes?</summary>

<a name="server-actions-vs-nitro-routes"></a>

### Server Actions vs Nitro routes?

**Answer:** Nuxt has **no** React-style **Server Actions** (`"use server"` colocated in components). Mutations use **`server/api`** Nitro handlers + **`$fetch`** from the client, or form posts to API routes.

|                 | Next.js Server Actions                 | Nuxt pattern                                     |
| --------------- | -------------------------------------- | ------------------------------------------------ |
| **Definition**  | RPC-like functions in React components | HTTP handlers in `server/api/`                   |
| **Colocation**  | Same file as UI                        | Separate `server/` tree                          |
| **Client call** | `action()` prop or `useActionState`    | `$fetch('/api/todos', { method: 'POST', body })` |
| **SSR data**    | RSC + Server Actions                   | `useFetch` / `useAsyncData` + payload            |

**When to use Nitro routes:** CRUD APIs, webhooks, DB access, anything that must not ship to the client bundle. Refresh UI with `refreshNuxtData()` or mutate local state after `$fetch` succeeds.

**See also:** [Next.js — Server Actions](./4_nextjs_data.md#server-actions-vs-route-handlers) · [server/api endpoints](./4_nuxt_data.md#serverapi-endpoints)

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server)

</details>

<details>
  <summary>Route middleware vs server middleware?</summary>

<a name="route-middleware-vs-server-middleware"></a>

### Route middleware vs server middleware?

**Definition:** Two middleware systems with similar names — runs in different runtimes for different purposes.

|                      | Route middleware              | Server middleware               |
| -------------------- | ----------------------------- | ------------------------------- |
| **Path**             | `app/middleware/`             | `server/middleware/`            |
| **Runtime**          | Vue app (SSR + client nav)    | Nitro only                      |
| **Runs on `/api/*`** | No                            | Yes                             |
| **API**              | `defineNuxtRouteMiddleware`   | `defineEventHandler`            |
| **Redirect**         | `return navigateTo('/login')` | `sendRedirect(event, '/login')` |
| **Typical use**      | UX auth gate                  | API JWT, logging, CORS          |

| Pros                                  | Cons                              |
| ------------------------------------- | --------------------------------- |
| Route MW — smooth client navigations  | Does not secure API alone         |
| Server MW — real request interception | Not run on CDN-served static HTML |

**When to use both:** Route middleware for redirecting unauthenticated users; server middleware + handler checks for API authorization.

**Examples:**

```ts
// app/middleware/auth.ts
export default defineNuxtRouteMiddleware(() => {
  if (!useUser().value) return navigateTo("/login");
});
```

```ts
// server/middleware/auth.ts
export default defineEventHandler((event) => {
  if (!getHeader(event, "authorization")) {
    throw createError({ statusCode: 401 });
  }
});
```

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>routeRules and prerendering?</summary>

<a name="routerules-and-prerendering"></a>

### routeRules and prerendering?

**Definition:** Declarative per-route rendering and caching in `nuxt.config.ts`, compiled by Nitro.

**How it works:**

- `prerender: true` — static HTML at build (crawler or explicit routes)
- `nuxt generate` — crawl from `/`, follow `<a href>`, emit `_payload.json`
- Unlinked dynamic routes need `nitro.prerender.routes` or `prerender:routes` hook

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
      routes: ["/sitemap.xml"],
      ignore: ["/admin"],
    },
  },
  routeRules: {
    "/blog/**": { prerender: true },
  },
});
```

**Thoughts:** Crawler only discovers `<a href>` links — JS-only navigation graphs won't prerender.

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>useSeoMeta and SEO in Nuxt?</summary>

### useSeoMeta and SEO in Nuxt?

**Definition:** Type-safe SEO composable (Unhead) for title, description, Open Graph, and Twitter tags — reactive and SSR-friendly.

**How it works:** Tags render into server HTML on SSR/prerender so crawlers see meta without executing JS.

| Tool                     | Use                             |
| ------------------------ | ------------------------------- |
| `nuxt.config` `app.head` | Static defaults (lang, favicon) |
| `useHead` in `app.vue`   | `titleTemplate`                 |
| `useSeoMeta` in pages    | Per-route SEO                   |
| `<Head>` components      | Template-style head tags        |

**Examples:**

```vue
<script setup lang="ts">
const title = ref("Product Page");
useSeoMeta({
  title,
  description: "Buy the best widgets",
  ogImage: "https://example.com/og.png",
});
</script>
```

**Thoughts:** Pair with prerender/SSR for content sites; CSR-only pages hurt SEO.

**Sources:** [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta)

</details>

<details>
  <summary>Nitro and deployment presets?</summary>

### Nitro and deployment presets?

**Definition:** Nitro builds `.output/` for Node, static, or serverless/edge presets.

| Target     | Command / preset             | Output                     |
| ---------- | ---------------------------- | -------------------------- |
| Node VPS   | default / `node-server`      | `.output/server/index.mjs` |
| Static CDN | `nuxt generate`              | `.output/public/`          |
| Vercel     | `NITRO_PRESET=vercel`        | Serverless functions       |
| Cloudflare | `cloudflare-pages` / Workers | Worker-compatible bundle   |

**When to use:** Match preset to host; set `app.baseURL` for subpath deploys.

**Examples:**

```bash
NODE_ENV=production node .output/server/index.mjs
NITRO_PRESET=vercel nuxt build
```

**Thoughts:** No vendor lock-in — same Nuxt app can move presets with rebuild. Mention `runtimeConfig` for env-specific secrets per environment.

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

---

## Coding tasks

<details>
  <summary>Task 1 — Fix double fetch on hydration</summary>

### Task 1 — Fix double fetch on hydration

**Definition:** Practice scenario — product page fetches twice (network tab shows duplicate `/api/products` on load).

**Scenario:** Developer used `$fetch` directly in `setup`. HTML renders, then client fetches again during hydration.

| Symptom                         | Likely cause                                 |
| ------------------------------- | -------------------------------------------- |
| Duplicate API call on page load | `$fetch` in setup without payload forwarding |
| Flash of empty state            | Client refetch after populated SSR           |

**What to change:**

```vue
<!-- ❌ Before -->
<script setup lang="ts">
const products = ref([]);
products.value = await $fetch("/api/products");
</script>
```

```vue
<!-- ✅ After -->
<script setup lang="ts">
const { data: products } = await useFetch("/api/products");
</script>
```

<details>
  <summary>Answer</summary>

**Root cause:** `$fetch` does not serialize results into the Nuxt **payload**, so the client repeats the request during hydration.

**Fix:** Replace with `useFetch` or `useAsyncData('products', () => $fetch('/api/products'))`. Verify in Nuxt DevTools Payload tab — data should appear once.

**Follow-up interview points:** When is `$fetch` still correct? (event handlers, `server/api` handlers, inside `useAsyncData` handler).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

</details>

<details>
  <summary>Task 2 — Protected dashboard route</summary>

### Task 2 — Protected dashboard route

**Definition:** Practice scenario — `/dashboard` must redirect unauthenticated users to `/login` on both SSR and client navigation; `/api/dashboard` must reject missing tokens.

**Scenario:** Implement route middleware + server API guard.

| Requirement           | Implementation                                        |
| --------------------- | ----------------------------------------------------- |
| Redirect guests       | `app/middleware/auth.ts` + `definePageMeta`           |
| SSR-safe session read | `useRequestHeaders(['cookie'])` or session composable |
| Secure API            | `server/middleware` or per-handler check              |

**Examples:**

```ts
// app/middleware/auth.ts
export default defineNuxtRouteMiddleware(() => {
  const user = useUser();
  if (!user.value) return navigateTo("/login");
});
```

```vue
<!-- app/pages/dashboard.vue -->
<script setup lang="ts">
definePageMeta({ middleware: "auth" });
</script>
```

```ts
// server/api/dashboard.get.ts
export default defineEventHandler((event) => {
  const token = getCookie(event, "session");
  if (!token) throw createError({ statusCode: 401 });
  return { stats: [] };
});
```

<details>
  <summary>Answer</summary>

Route middleware handles **UX redirects** for page navigations. API routes need **server-side** checks — middleware alone is insufficient because `/api/dashboard` is callable directly.

Populate `useUser()` from a composable that reads session via `useFetch('/api/me')` on SSR (cookies forwarded automatically).

**Security note:** Never trust client-only guards for sensitive data.

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

</details>

<details>
  <summary>Task 3 — Prerender blog with dynamic slugs</summary>

### Task 3 — Prerender blog with dynamic slugs

**Definition:** Practice scenario — blog posts live at `/blog/[slug]` from a CMS. `nuxt generate` only prerenders `/blog` because slugs are not linked yet.

| Tool                     | Scope                                    |
| ------------------------ | ---------------------------------------- |
| Crawler                  | Only follows existing `<a href>` in HTML |
| `nitro.prerender.routes` | Explicit paths at build time             |
| `prerender:routes` hook  | Fetch slugs from CMS API during build    |

**Examples:**

```ts
export default defineNuxtConfig({
  hooks: {
    async "prerender:routes"(ctx) {
      const { posts } = await $fetch("https://cms.example.com/posts");
      for (const post of posts) {
        ctx.routes.add(`/blog/${post.slug}`);
      }
    },
  },
  routeRules: {
    "/blog/**": { prerender: true },
  },
});
```

<details>
  <summary>Answer</summary>

The Nitro crawler cannot invent CMS slugs — register them in the **`prerender:routes`** hook (or static `nitro.prerender.routes` list). Ensure `/blog` page links to posts for crawl-based discovery as a backup.

After `nuxt generate`, confirm `.output/public/blog/<slug>/index.html` and `_payload.json` exist.

**Interview follow-up:** What happens to blog data on client navigation for static deploy? Payload from build time — data stale until rebuild unless using ISR.

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

</details>

---
