# Nuxt — Core

Framework model, rendering strategies, and design principles.

[← Back to hub](./4_nuxt.md)

### Related docs

- [Nuxt — Core](./4_nuxt_core.md) · [Nuxt — Data](./4_nuxt_data.md) · [Nuxt — Routing](./4_nuxt_routing.md) · [Nuxt — Platform](./4_nuxt_platform.md) · [Nuxt — Interview](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [1. Framework overview](#1-framework-overview)
- [2. Nuxt vs Vue SPA](#2-nuxt-vs-vue-spa)
- [3. Project structure](#3-project-structure)
- [4. Rendering strategies](#4-rendering-strategies)
- [5. Server & client boundaries](#5-server-client-boundaries)
- [6. Streaming & Suspense](#6-streaming-suspense)
- [7. Design principles](#7-design-principles)

---

<a name="topic-template"></a>

### Topic template

Every major topic in `<details>` sections follows:

**Definition** → **How it works** → **When to use** → **Pluses and minuses** → **Examples** → **Thoughts** → **Sources**

---

<a name="1-framework-overview"></a>

## 1. Framework overview

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Nuxt overview</summary>

### Nuxt overview

**Definition:** Nuxt is a **Vue framework** for building full-stack web applications and websites. You write `.vue` files and composables; Nuxt adds file-based routing, SSR by default, auto-imports, data-fetching utilities, and a Nitro server — bundlers and TypeScript config included.

**When to use:**

- Marketing sites, blogs, and documentation with SEO requirements
- E-commerce, dashboards, and SaaS with authenticated and public pages
- BFF (Backend for Frontend) — colocate API routes with UI via `server/api/`
- Teams wanting universal deployment (Node, serverless, edge, static) from one codebase
- Vue teams that want conventions over manual Vite + Vue Router setup

**When not to use:**

- Pure static sites with minimal JS — consider **Astro**
- Mobile apps — use **Expo / React Native** or native stacks
- Heavy custom long-lived WebSocket hub — dedicated backend + Vite SPA may fit better
- Teams committed to React — use **Next.js** instead

**Pluses and minuses:**

| +                                                   | −                                                            |
| --------------------------------------------------- | ------------------------------------------------------------ |
| Batteries-included: routing, SSR, API, auto-imports | Opinionated directory structure                              |
| Nitro universal deployment (15+ presets)            | Learning curve for payload, caching, and hybrid `routeRules` |
| Zero-config TypeScript and Vite HMR                 | Smaller hiring pool than React/Next in some markets          |
| SEO-friendly SSR/SSG out of the box                 | Cold starts on serverless; tune per preset                   |
| Strong Vue ecosystem (Pinia, VueUse modules)        | Major version upgrades (2→3) require migration effort        |

**Examples:**

```bash
npm create nuxt@latest my-app
cd my-app && npm run dev
```

```vue
<!-- app/pages/index.vue -->
<script setup lang="ts">
const { data: posts } = await useFetch("/api/posts");
</script>

<template>
  <main>
    <h1>Posts</h1>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </main>
</template>
```

**Thoughts:** Nuxt is **not** a replacement for Vue knowledge — it is a layer on top. Interview framing: Vue = UI library; Nuxt = framework (routing, rendering, data, deployment). New projects should use **Nuxt 3/4** (`nuxt` package). Nuxt 2 is legacy.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>Vue vs Nuxt</summary>

### Vue vs Nuxt

**Definition:**

Vue renders UI; Nuxt orchestrates how and where that UI is built, cached, and delivered.

|                   | Vue (Vite SPA)            | Nuxt                                             |
| ----------------- | ------------------------- | ------------------------------------------------ |
| **Role**          | UI library                | Full-stack framework                             |
| **Routing**       | Vue Router (manual setup) | File-based `app/pages/`                          |
| **Data fetching** | Client hooks / manual SSR | `useFetch`, `useAsyncData`, payload              |
| **Rendering**     | CSR by default            | SSR, SSG, hybrid via `routeRules`                |
| **API**           | Separate backend          | `server/api/` Nitro routes                       |
| **Deployment**    | Static CDN or custom      | Nitro presets (Node, Vercel, Cloudflare, static) |

**Thoughts:** For generic Vue SSR, hydration, and reactivity mechanics see [Vue — SSR](./4_vue_ssr.md). This document covers **Nuxt-specific** APIs and Nitro semantics.

**Sources:** [vuejs.org](https://vuejs.org/) · [nuxt.com/docs](https://nuxt.com/docs)

</details>

<details>
  <summary>Nuxt vs alternatives</summary>

### Nuxt vs alternatives

**Definition:**

Comparison of Nuxt against other full-stack and meta-framework choices.

| Framework             | Strength                       | Trade-off                                  |
| --------------------- | ------------------------------ | ------------------------------------------ |
| **Vite + Vue Router** | Maximum flexibility, SPA-first | Manual SSR, no built-in API layer          |
| **Next.js**           | React ecosystem, RSC, Vercel   | Different UI paradigm (React)              |
| **SvelteKit**         | Performance, simplicity        | Smaller Vue-aligned ecosystem              |
| **Remix**             | Web standards, loaders/actions | React-only                                 |
| **Astro**             | Content sites, islands         | Less suited for heavy client interactivity |

**When to use Nuxt:** SEO-sensitive Vue apps, universal deployment, team standardization on Vue 3 Composition API.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>History (compressed)</summary>

### History (compressed)

**Definition:**

Timeline of major Nuxt releases and architecture milestones.

| Version   | Milestone                                                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **2016**  | Nuxt 1 — Vue 2, webpack, universal mode                                                                                                 |
| **v2**    | Improved CLI, module ecosystem, static `generate`                                                                                       |
| **v3**    | Vue 3, Vite, Nitro server, optional `app/` directory, Composition API first                                                             |
| **v4**    | `app/` as default `srcDir`; `shared/` for isomorphic utils/types; cleaner upgrade path; docs aligned with [nuxt.com](https://nuxt.com/) |
| **Nitro** | Standalone server engine (UnJS); powers Nuxt server and deploy presets                                                                  |

**Sources:** [nuxt.com/blog](https://nuxt.com/blog)

</details>

<a name="2-nuxt-vs-vue-spa"></a>

## 2. Nuxt vs Vue SPA

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>When to choose Nuxt over Vite SPA</summary>

### When to choose Nuxt over Vite SPA

**Definition:** A Vite + Vue SPA ships HTML shell + client bundle; Nuxt can SSR/SSG the same Vue components and colocate server logic.

**How it works:** Nuxt runs your `setup()` on the server for the initial request, serializes state into a **payload**, and hydrates on the client. A plain SPA fetches data only after JS loads.

**When to use Nuxt:** SEO, faster first paint, shared auth cookies on server, API routes without a second repo.

**When to use Vite SPA:** Internal admin tools, embedded widgets, maximum client-only simplicity.

**Pluses and minuses:**

| + (Nuxt)                                 | − (Nuxt)                               |
| ---------------------------------------- | -------------------------------------- |
| First meaningful paint with data         | More moving parts than SPA             |
| `server/api` without Express boilerplate | SSR bugs (hydration mismatch) possible |
| Hybrid per-route rendering               | Build/deploy concepts to learn         |

**Thoughts:** You can disable SSR globally (`ssr: false`) and still use Nuxt for routing, modules, and auto-imports — but you lose most SEO benefits.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<a name="3-project-structure"></a>

## 3. Project structure

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Directory layout</summary>

### Directory layout

**Definition:** Nuxt 4 uses **`app/` as the default `srcDir`** — pages, layouts, middleware, and `app.vue` live under `app/`. **`shared/`** at project root holds isomorphic code (types, utils) importable from both `app/` and `server/`. `nuxt.config.ts` can set `srcDir: '.'` for Nuxt 3-style flat layout.

**When to use:** Follow Nuxt 4 defaults for new projects. Use `shared/` for code used by UI and Nitro handlers. Monorepos or design systems may use `layers/` or custom `srcDir`.

**Pluses and minuses:**

| +                                        | −                                         |
| ---------------------------------------- | ----------------------------------------- |
| Predictable onboarding                   | Deep `pages/` trees mirror URLs literally |
| Colocation of pages, layouts, middleware | Moving files changes routes               |
| `server/` for API separate from UI       | Two middleware concepts (route vs server) |

**Examples:**

```bash
my-nuxt-app/
├── app/
│   ├── app.vue              # root shell
│   ├── pages/               # file-based routes
│   │   ├── index.vue
│   │   └── posts/[id].vue
│   ├── layouts/
│   │   └── default.vue
│   ├── components/          # auto-imported
│   └── middleware/          # route middleware
├── server/
│   ├── api/                 # Nitro API routes
│   └── middleware/          # server middleware
├── composables/             # auto-imported (root or app/)
├── shared/                  # isomorphic types/utils (Nuxt 4)
│   └── types/
├── plugins/                 # or app/plugins/ — defineNuxtPlugin
├── nuxt.config.ts
└── package.json
```

**Thoughts:** `server/` and `shared/` stay at project root; only UI-facing dirs default inside `app/`. See [defineNuxtPlugin](./4_nuxt_platform.md#definenuxtplugin).

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure](https://nuxt.com/docs/4.x/guide/directory-structure) · [nuxt.com/docs/4.x/getting-started/upgrade](https://nuxt.com/docs/4.x/getting-started/upgrade)

</details>

<details>
  <summary>Auto-imports</summary>

### Auto-imports

**Definition:** Nuxt auto-imports Vue APIs, composables from `composables/`, components from `components/`, and utilities from `utils/` — no manual import statements required. Tree-shaking removes unused exports from the client bundle.

**How it works:** Nuxt scans configured directories at build time and generates type stubs in `.nuxt/imports.d.ts`. Components use path-based naming (`BaseButton.vue` → `<BaseButton />`).

**When to use:** Default everywhere; disable per-component with explicit imports only when avoiding naming collisions.

**Pluses and minuses:**

| +                                     | −                                                      |
| ------------------------------------- | ------------------------------------------------------ |
| Less boilerplate, faster feature work | Magic — newcomers may not know where symbols come from |
| Type-safe with generated types        | Name collisions across nested `components/` folders    |
| HMR for composables and components    | Custom dirs need `imports.dirs` config                 |

**Examples:**

```vue
<!-- No imports needed -->
<script setup lang="ts">
const count = ref(0);
const { data } = await useFetch("/api/count");
</script>

<template>
  <BaseCounter :count="count" />
</template>
```

**Thoughts:** Prefer extracting reusable logic to `composables/useX.ts` — still auto-imported and testable.

**Sources:** [nuxt.com/docs/4.x/guide/concepts/auto-imports](https://nuxt.com/docs/4.x/guide/concepts/auto-imports)

</details>

<details>
  <summary>app.vue root shell</summary>

### app.vue root shell

**Definition:** `app.vue` is the root component wrapping every page. Typically renders `<NuxtLayout>` and `<NuxtPage />` — the Nuxt equivalents of a persistent shell plus routed content.

**Examples:**

```vue
<!-- app/app.vue -->
<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
```

**When to use:** Global providers, `useHead` title templates, or a single-layout app. For one layout only, some teams inline markup in `app.vue` instead of `layouts/default.vue`.

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/app](https://nuxt.com/docs/4.x/guide/directory-structure/app)

</details>

<a name="4-rendering-strategies"></a>

## 4. Rendering strategies

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>SSR (default)</summary>

### SSR (default)

**Definition:** Nuxt renders Vue components to HTML on **each request** (or on first visit for cached routes). The browser receives populated HTML, then hydrates into an interactive app.

**How it works:** Nitro invokes the Vue renderer; `useFetch` / `useAsyncData` run on the server; results are embedded in the HTML payload for hydration.

**When to use:** Personalized pages, auth-gated content, always-fresh data, SEO with dynamic data.

**Pluses and minuses:**

| +                        | −                                                |
| ------------------------ | ------------------------------------------------ |
| SEO and fast first paint | Server compute per request (unless cached)       |
| Secrets stay on server   | Hydration mismatch if server/client HTML differs |

**Examples:**

```ts
// nuxt.config.ts — SSR is default (ssr: true)
export default defineNuxtConfig({
  ssr: true,
});
```

**Thoughts:** SSR mechanics (hydration, `onServerPrefetch`) → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>SSG and prerendering</summary>

### SSG and prerendering

**Definition:** Static Site Generation pre-renders routes at **build time** into HTML (+ `_payload.json`) served from static hosting. Triggered via `nuxt generate` or `nuxt build --prerender`.

**How it works:** Nitro crawler loads `/`, saves HTML, follows `<a href>` links, repeats until exhausted. Manual routes via `nitro.prerender.routes` or `routeRules`.

**When to use:** Marketing, docs, blogs, catalogs with known URLs.

**Pluses and minuses:**

| +                                           | −                                           |
| ------------------------------------------- | ------------------------------------------- |
| CDN-fast, cheap hosting                     | Data stale until rebuild (unless ISR/SWR)   |
| No server at runtime                        | Unlinked dynamic routes not auto-discovered |
| Emits `200.html` / `404.html` SPA fallbacks | Crawler only finds anchor links             |

**Examples:**

```bash
npx nuxt generate
# deploy .output/public to static host
```

```ts
export default defineNuxtConfig({
  routeRules: {
    "/": { prerender: true },
    "/blog/**": { prerender: true },
  },
});
```

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>CSR (SPA mode)</summary>

### CSR (SPA mode)

**Definition:** Disable SSR globally with `ssr: false`. Nuxt ships an empty shell; Vue mounts and fetches data client-side only.

**When to use:** Internal tools, auth-walled apps with no SEO need, or hybrid apps using `<ClientOnly>` for specific widgets.

**Pluses and minuses:**

| +                                   | −                             |
| ----------------------------------- | ----------------------------- |
| Simpler mental model (no hydration) | Poor SEO and slow first paint |
| Deploy as static SPA                | Loses SSR payload benefits    |

**Examples:**

```ts
export default defineNuxtConfig({
  ssr: false,
});
```

**Thoughts:** Prefer hybrid `routeRules` over global `ssr: false` when only some routes need CSR.

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<details>
  <summary>Hybrid rendering (routeRules)</summary>

### Hybrid rendering (routeRules)

**Definition:** Per-route rules in `nuxt.config.ts` customize rendering mode, caching, redirects, and more. Nuxt compiles rules; Nitro applies server-side behavior.

**How it works:** Rules like `prerender`, `ssr`, `cache`, `isr`, `swr`, `redirect` map to Nitro handlers at build or request time.

**When to use:** Marketing static + dynamic dashboard in one app; API caching; legacy URL redirects.

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    "/": { prerender: true },
    "/admin/**": { ssr: true },
    "/api/**": { cache: { maxAge: 3600 } },
    "/old-page": { redirect: { to: "/new-page", statusCode: 301 } },
  },
});
```

**Pluses and minuses:**

| +                              | −                                                     |
| ------------------------------ | ----------------------------------------------------- |
| One codebase, mixed strategies | Misconfigured rules → surprising cache/stale behavior |
| CDN cache for API routes       | Document which routes are static vs dynamic           |

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server)

</details>

<a name="5-server-client-boundaries"></a>

## 5. Server & client boundaries

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Universal vs client-only code</summary>

### Universal vs client-only code

**Definition:** Nuxt runs Vue `setup()` on **server and client** unless gated. Browser APIs (`window`, `localStorage`) and some lifecycle hooks only work on the client.

**How it works:** Code in `setup` runs on server for SSR, then again on client during hydration. Side effects must be idempotent or wrapped in `onMounted` / `import.meta.client`.

**When to use server:** Data fetching with `useFetch`, reading cookies via `useRequestHeaders`, Nitro `server/` utilities.

**When to use client:** DOM manipulation, `localStorage`, third-party widgets without SSR support.

**Pluses and minuses:**

| +                                   | −                                        |
| ----------------------------------- | ---------------------------------------- |
| Single component tree for SSR + CSR | Easy to reference `window` and break SSR |
| `<ClientOnly>` escape hatch         | Duplicate logic if not using composables |

**Examples:**

```vue
<script setup lang="ts">
if (import.meta.client) {
  console.log("Runs only in browser");
}
</script>

<template>
  <ClientOnly>
    <BrowserOnlyChart />
  </ClientOnly>
</template>
```

**Thoughts:** Deep dive on hydration mismatches and `onServerPrefetch` → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>server/api vs app code</summary>

### server/api vs app code

**Definition:** `server/` runs **only on Nitro** — never bundled to the client. `app/` Vue code can run on both server and client.

**When to use `server/api`:** Database access, secrets, webhooks, REST endpoints for external clients.

**When to use composables + `useFetch`:** Reading data into Vue pages with SSR payload forwarding.

**Examples:**

```ts
// server/api/posts.get.ts
export default defineEventHandler(async () => {
  return await db.post.findMany();
});
```

```vue
<script setup lang="ts">
const { data: posts } = await useFetch("/api/posts");
</script>
```

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server)

</details>

<a name="6-streaming-suspense"></a>

## 6. Streaming & Suspense

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Suspense and navigation blocking</summary>

### Suspense and navigation blocking

**Definition:** Nuxt uses Vue `<Suspense>` under the hood. `useFetch` / `useAsyncData` block navigation until data resolves (unless `lazy`).

**How it works:** On server render, Nuxt waits for async setup either way before sending HTML. On **client navigation**, `await useFetch` keeps the user on the current page until data is ready (optional `<NuxtLoadingIndicator>`).

**When to use blocking fetch:** SEO pages that should land fully populated.

**When to use `lazy` / `useLazyFetch`:** Dashboard widgets, below-the-fold data, instant navigation with manual loading UI.

**Examples:**

```vue
<script setup lang="ts">
// Blocks navigation until resolved (default)
const { data } = await useFetch("/api/posts");

// Non-blocking — handle status yourself
const { data: comments, status } = useLazyFetch("/api/comments");
</script>
```

**Pluses and minuses:**

| +                          | −                                                     |
| -------------------------- | ----------------------------------------------------- |
| No half-rendered SEO pages | Slow API → slow route transitions                     |
| `lazy` gives fine control  | `lazy` + `await` on client behaves subtly differently |

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>NuxtLoadingIndicator</summary>

### NuxtLoadingIndicator

**Definition:** Built-in component showing a progress bar between page navigations while async data and code-split chunks load.

**Examples:**

```vue
<!-- app/app.vue -->
<template>
  <NuxtLoadingIndicator />
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
```

**Sources:** [nuxt.com/docs/4.x/api/components/nuxt-loading-indicator](https://nuxt.com/docs/4.x/api/components/nuxt-loading-indicator)

</details>

<a name="7-design-principles"></a>

## 7. Design principles

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Conventions over configuration</summary>

### Conventions over configuration

**Definition:** Nuxt automates repetitive tasks via directory conventions — routes from `pages/`, API from `server/api/`, layouts from `layouts/`. `nuxt.config.ts` customizes when defaults are insufficient.

**When to use:** Trust defaults first; reach for config when deploying to edge, splitting layers, or tuning `routeRules`.

**Pluses and minuses:**

| +                              | −                                             |
| ------------------------------ | --------------------------------------------- |
| Fast greenfield setup          | Non-standard architectures fight conventions  |
| Modules extend without forking | Over-customization reduces framework benefits |

**Thoughts:** Extract domain logic to `composables/` and `server/utils/` — keep `pages/` thin like Next.js `page.tsx`.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>SOLID in Nuxt</summary>

### SOLID in Nuxt

**Definition:** SOLID principles applied to **Nuxt pages, composables, server handlers, and `server/utils/`** — not only classes.

| Principle                     | Nuxt example                                                         |
| ----------------------------- | -------------------------------------------------------------------- |
| **S** — Single Responsibility | `pages/` compose · `composables/useX` fetches · `server/api` mutates |
| **O** — Open/Closed           | Add payment module — don't edit checkout handler for each provider   |
| **L** — Liskov Substitution   | Layouts always render `<slot />` consistently                        |
| **I** — Interface Segregation | Pass `{ readUser }` not entire DB client                             |
| **D** — Dependency Inversion  | `placeOrder(input, store)` — inject repository in tests              |

**Examples:**

```ts
// S — good: thin page, composable owns fetch
// pages/checkout.vue
const { cart } = await useCart();

// composables/useCart.ts
export function useCart() {
  return useFetch("/api/cart");
}
```

```ts
// S — bad: page calls DB, sends email, renders form inline
```

**Thoughts:** See also [SOLID in React](./4_react_core.md#solid-in-react) for parallel patterns.

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure](https://nuxt.com/docs/4.x/guide/directory-structure)

</details>
