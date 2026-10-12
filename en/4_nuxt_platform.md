# Nuxt — Platform

Nitro, modules, config, deployment, migration, and debugging.

[← Back to hub](./4_nuxt.md)

### Related docs

- [Nuxt — Core](./4_nuxt_core.md) · [Nuxt — Data](./4_nuxt_data.md) · [Nuxt — Routing](./4_nuxt_routing.md) · [Nuxt — Platform](./4_nuxt_platform.md) · [Nuxt — Interview](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [14. Nitro server engine](#14-nitro-server-engine)
- [15. Middleware & route rules](#15-middleware-route-rules)
- [16. Configuration](#16-configuration)
- [17. Deployment](#17-deployment)
- [18. Migration & pitfalls](#18-migration-pitfalls)
- [19. Performance & debugging](#19-performance-debugging)

---

<a name="14-nitro-server-engine"></a>

## 14. Nitro server engine

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Nitro overview</summary>

### Nitro overview

**Definition:** **Nitro** is Nuxt's server engine (UnJS / h3). It builds the server/API layer, handles SSR, prerendering, and deployment presets — usable outside Nuxt.

**How it works:** In dev, Rollup + Node workers isolate server code. In production, Nitro emits a minimal `.output/` bundle (tree-shaken, no full `node_modules`). Reads `server/api/` and `server/middleware/` to generate routes.

**When to use:** Always — it's the server layer for every Nuxt app (even static `generate` uses Nitro at build time).

**Pluses and minuses:**

| +                                            | −                                            |
| -------------------------------------------- | -------------------------------------------- |
| Universal deploy (Node, Workers, serverless) | Preset-specific quirks (edge runtime limits) |
| Fast cold starts (minimal bundle)            | Advanced features need Nitro docs            |
| HMR for server routes in dev                 |                                              |
| Hybrid rendering via `routeRules`            |                                              |

**Examples:**

```bash
nuxt build
# .output/server/index.mjs — Node entry
# .output/public/ — static assets + prerendered HTML
```

**Thoughts:** Think of Nitro as Nuxt's equivalent to Next.js's custom server compiler + deployment adapter.

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server) · [nitro.build](https://nitro.build/)

</details>

<details>
  <summary>Modules ecosystem</summary>

### Modules ecosystem

**Definition:** Nuxt **modules** extend the framework — auto-register plugins, components, runtime config, and build hooks. Install via `nuxt.config` `modules` array.

**How it works:** Modules run at build time (`@nuxt/kit` helpers) and can inject server routes, Vue plugins, or Vite config.

**When to use:** `@nuxtjs/tailwindcss`, `@pinia/nuxt`, `@vueuse/nuxt`, `@nuxt/image`, `@nuxt/content`, i18n, auth.

**Pluses and minuses:**

| +                                  | −                                      |
| ---------------------------------- | -------------------------------------- |
| One-line integrations              | Module quality varies                  |
| Huge ecosystem at nuxt.com/modules | Version pins must match Nuxt major     |
| Local modules via `modules/` dir   | Over-module-ing obscures what app does |

**Examples:**

```ts
export default defineNuxtConfig({
  modules: ["@pinia/nuxt", "@nuxtjs/tailwindcss", "@vueuse/nuxt"],
});
```

**Thoughts:** Curated official modules worth knowing:

| Module             | Role                                                                 |
| ------------------ | -------------------------------------------------------------------- |
| `@nuxt/ui`         | Tailwind-based UI primitives (Reka UI + Tailwind CSS v4)             |
| `@nuxt/content`    | File-based CMS — Markdown/YAML/JSON in `content/`                    |
| `@nuxt/image`      | Responsive images, lazy loading, provider integrations               |
| `@nuxt/eslint`     | Project-aware ESLint flat config                                     |
| `@nuxt/test-utils` | Mount Nuxt apps in Vitest — see [@nuxt/test-utils](#nuxt-test-utils) |

**Sources:** [nuxt.com/modules](https://nuxt.com/modules)

</details>

<details>
  <summary>defineNuxtPlugin</summary>

### defineNuxtPlugin

**Definition:** Nuxt **plugins** run during app initialization — server, client, or both. Files in `app/plugins/` (or `plugins/` at root) export `defineNuxtPlugin((nuxtApp) => { … })`.

**How it works:**

| Pattern                       | Behavior                                             |
| ----------------------------- | ---------------------------------------------------- |
| Default plugin                | Runs on server + client                              |
| `*.client.ts` suffix          | Client-only (browser APIs)                           |
| `*.server.ts` suffix          | Server-only (DB, secrets)                            |
| `provide` / `nuxtApp.provide` | Inject helpers — access via `useNuxtApp().$myHelper` |
| `enforce: 'pre'`              | Run before other plugins (order)                     |

Distinct from **modules** (build-time framework extension) — plugins are runtime app setup.

**When to use:** Global error handlers, third-party Vue plugins (`vueApp.use`), provide API clients, client-only analytics.

**Examples:**

```ts
// app/plugins/api.client.ts
export default defineNuxtPlugin(() => {
  const api = $fetch.create({ baseURL: "/api" });
  return {
    provide: {
      api,
    },
  };
});
```

```vue
<script setup>
const { $api } = useNuxtApp();
const data = await $api("/users");
</script>
```

**See also:** [Vue plugins](./4_vue_ecosystem.md#vue-plugins) · [Modules ecosystem](#modules-ecosystem)

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/plugins](https://nuxt.com/docs/4.x/guide/directory-structure/plugins)

</details>

<details>
  <summary>Built-in optimizations</summary>

### Built-in optimizations

**Definition:** Nuxt ships performance defaults — Vite bundling, route-level code splitting, payload extraction, and optional `@nuxt/image` for responsive images.

**When to use:** Enable `@nuxt/image` for LCP images; tune `experimental.payloadExtraction` for CDN payload caching.

**Examples:**

```ts
export default defineNuxtConfig({
  modules: ["@nuxt/image"],
  image: {
    domains: ["cdn.example.com"],
  },
});
```

```vue
<template>
  <NuxtImg src="/hero.jpg" width="1200" height="630" priority />
</template>
```

**Sources:** [image.nuxt.com](https://image.nuxt.com/)

</details>

<a name="15-middleware-route-rules"></a>

## 15. Middleware & route rules

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Route rules reference</summary>

### Route rules reference

**Definition:** `routeRules` in `nuxt.config.ts` — per-path Nitro + Nuxt behavior compiled at build time.

| Rule                | Effect                               |
| ------------------- | ------------------------------------ |
| `prerender: true`   | Static HTML at build                 |
| `ssr: false`        | Client-only route                    |
| `cache: { maxAge }` | Cache response                       |
| `isr` / `swr`       | Incremental / stale-while-revalidate |
| `redirect`          | HTTP redirect                        |
| `appLayout`         | Set layout by config (v4.3+)         |
| `cors`              | CORS headers on route                |

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    "/": { prerender: true },
    "/api/**": { cors: true, cache: { maxAge: 60 } },
    "/legacy": { redirect: { to: "/new", statusCode: 301 } },
    "/spa/**": { ssr: false },
  },
});
```

**Sources:** [nuxt.com/docs/4.x/guide/concepts/rendering](https://nuxt.com/docs/4.x/guide/concepts/rendering)

</details>

<details>
  <summary>Route vs server middleware (summary)</summary>

### Route vs server middleware (summary)

**Definition:**

|                 | Route middleware                    | Server middleware         |
| --------------- | ----------------------------------- | ------------------------- |
| **Location**    | `app/middleware/`                   | `server/middleware/`      |
| **Runs on**     | Vue navigations + SSR page requests | All Nitro server requests |
| **API routes**  | No                                  | Yes                       |
| **Typical use** | Auth redirect, analytics            | JWT on `/api/*`, logging  |

**Thoughts:** Full detail → [Routing §11](./4_nuxt_routing.md#route-middleware) and [Data §9](./4_nuxt_data.md#server-middleware).

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<a name="16-configuration"></a>

## 16. Configuration

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>nuxt.config.ts</summary>

### nuxt.config.ts

**Definition:** Central config file — `defineNuxtConfig({ ... })` controls SSR, modules, `runtimeConfig`, `routeRules`, Vite/Nitro options, and `app` settings.

**How it works:** Nuxt merges config from layers (if used), applies modules, generates `.nuxt/` build artifacts. Env vars override `runtimeConfig` with `NUXT_` prefix.

**When to use:** Any non-default behavior — deployment preset, base URL, module registration, hybrid rendering.

**Pluses and minuses:**

| +                      | −                                             |
| ---------------------- | --------------------------------------------- |
| Single source of truth | Large configs benefit from `extends` / layers |
| Type-safe with schema  | Some options moved between majors             |
| Env-based overrides    |                                               |

**Examples:**

```ts
export default defineNuxtConfig({
  devtools: { enabled: true },
  ssr: true,
  app: {
    baseURL: "/app/",
    head: { title: "My App" },
  },
  runtimeConfig: {
    dbUrl: "",
    public: { apiBase: "/api" },
  },
  nitro: {
    preset: "node-server",
    prerender: { crawlLinks: true },
  },
  routeRules: {
    "/": { prerender: true },
  },
  compatibilityDate: "2025-01-01",
});
```

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/nuxt-config](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config)

</details>

<details>
  <summary>Layers and extends</summary>

### Layers and extends

**Definition:** **Layers** share config, components, and pages across projects — monorepo design systems or white-label apps.

**Examples:**

```ts
export default defineNuxtConfig({
  extends: ["../base-layer"],
});
```

**When to use:** Shared UI kit + per-brand Nuxt apps; internal platform teams.

**Sources:** [nuxt.com/docs/4.x/guide/going-further/layers](https://nuxt.com/docs/4.x/guide/going-further/layers)

</details>

<details>
  <summary>Environment variables</summary>

### Environment variables

**Definition:** `.env` files feed `runtimeConfig`. Only keys under `runtimeConfig.public` are exposed to the client bundle.

| Pattern                    | Access                                  |
| -------------------------- | --------------------------------------- |
| `NUXT_API_SECRET=...`      | `useRuntimeConfig().apiSecret` (server) |
| `NUXT_PUBLIC_API_BASE=...` | `useRuntimeConfig().public.apiBase`     |

**Examples:**

```env
NUXT_API_SECRET=super-secret
NUXT_PUBLIC_SITE_URL=https://example.com
```

**Thoughts:** Never put secrets in `public` or Vue components.

**Sources:** [nuxt.com/docs/4.x/guide/going-further/runtime-config](https://nuxt.com/docs/4.x/guide/going-further/runtime-config)

</details>

<a name="17-deployment"></a>

## 17. Deployment

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Node.js server</summary>

### Node.js server

**Definition:** Default Nitro output — run `.output/server/index.mjs` on any Node host.

**How it works:** `nuxt build` without static preset produces server + client assets. Respect `PORT`, `HOST`, `NITRO_*` env vars.

**Examples:**

```bash
NODE_ENV=production node .output/server/index.mjs
# default port 3000
```

```js
// ecosystem.config.cjs — PM2
module.exports = {
  apps: [
    {
      name: "NuxtApp",
      script: "./.output/server/index.mjs",
      instances: "max",
      exec_mode: "cluster",
      env: { NODE_ENV: "production" },
    },
  ],
};
```

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<details>
  <summary>Static hosting</summary>

### Static hosting

**Definition:** Deploy `.output/public` after `nuxt generate` or prerender rules. Includes `200.html` / `404.html` SPA fallbacks and `_payload.json` per route.

**When to use:** Netlify, Cloudflare Pages, S3 + CloudFront, GitHub Pages (with `baseURL`).

**Examples:**

```bash
npx nuxt generate
npx serve .output/public
```

**Pluses and minuses:**

| +                | −                                                               |
| ---------------- | --------------------------------------------------------------- |
| Cheapest hosting | No server routes at runtime (API must be external or functions) |
| CDN-global       | Dynamic auth pages need SSR host or edge functions              |

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<details>
  <summary>Serverless and edge presets</summary>

### Serverless and edge presets

**Definition:** Nitro **presets** target Vercel, Netlify Functions, Cloudflare Workers, AWS Lambda, etc.

**How it works:** Set `nitro.preset` or `NITRO_PRESET` env at build time. Each preset adapts output format and entry handler.

**Examples:**

```bash
NITRO_PRESET=vercel nuxt build
NITRO_PRESET=cloudflare-pages nuxt build
```

```ts
export default defineNuxtConfig({
  nitro: { preset: "cloudflare-module" },
});
```

**Thoughts:** Edge presets may lack Node APIs — verify DB drivers and file system usage.

**Sources:** [nitro.build/deploy](https://nitro.build/deploy)

</details>

<details>
  <summary>CDN and reverse proxy pitfalls</summary>

### CDN and reverse proxy pitfalls

**Definition:** Misconfigured CDNs break hydration — Cloudflare Rocket Loader, email obfuscation, and HTML minification can inject scripts or alter markup.

**When to use:** Always audit CDN "optimization" settings for production Nuxt apps.

**Examples:**

Disable on Cloudflare:

1. Speed → Rocket Loader
2. Security → Email Address Obfuscation

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<a name="18-migration-pitfalls"></a>

## 18. Migration & pitfalls

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Nuxt 2 → 3 migration</summary>

### Nuxt 2 → 3 migration

**Definition:** Major rewrite — Vue 3, Composition API, Nitro instead of legacy server, `pages/` may live under `app/`, new data fetching composables.

| Nuxt 2                     | Nuxt 3/4                    |
| -------------------------- | --------------------------- |
| `asyncData` / `fetch` hook | `useAsyncData` / `useFetch` |
| `context.$axios`           | `$fetch` / `useFetch`       |
| `nuxtServerInit` (Vuex)    | Pinia + server plugins      |
| `modules` webpack build    | Vite + Nitro                |
| `middleware/` root         | `app/middleware/`           |
| `static/`                  | `public/`                   |

**When to use:** Incremental migration via [nuxt.com/docs/migration](https://nuxt.com/docs/migration) checklist; consider `@nuxt/bridge` only for interim Nuxt 2 codebases.

**Thoughts:** Greenfield should start on Nuxt 3/4 directly — bridge is a stepping stone, not a destination.

**Sources:** [nuxt.com/docs/migration/overview](https://nuxt.com/docs/migration/overview)

</details>

<details>
  <summary>Common pitfalls</summary>

### Common pitfalls

**Definition:** Frequent production bugs in Nuxt apps.

| Pitfall                              | Symptom                          | Fix                                               |
| ------------------------------------ | -------------------------------- | ------------------------------------------------- |
| `$fetch` in setup without `useFetch` | Double fetch, hydration flash    | Use `useFetch` / `useAsyncData`                   |
| `window` in setup                    | SSR crash                        | `import.meta.client`, `onMounted`, `<ClientOnly>` |
| Route middleware only for auth       | API still open                   | Add `server/middleware` or handler checks         |
| Prerender without links              | Missing routes in static build   | `nitro.prerender.routes` or sitemap crawl         |
| Huge payload                         | Slow TTFB                        | `pick`, `transform`, smaller DTOs                 |
| Mismatched `useAsyncData` keys       | Dev warnings, stale shared state | Consistent `handler` + options per key            |
| `ssr: false` globally                | No SEO                           | Hybrid `routeRules` instead                       |

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Vue SPA → Nuxt migration</summary>

### Vue SPA → Nuxt migration

**Definition:** Moving Vite + Vue Router SPA into Nuxt conventions.

**Steps (compressed):**

1. Move routes to `app/pages/` matching URL structure
2. Replace `axios` calls in `onMounted` with `useFetch` in `setup`
3. Extract shared layout to `layouts/default.vue`
4. Move API to `server/api/` or keep external API
5. Add `useSeoMeta` per page
6. Configure `routeRules` for static marketing pages

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<a name="19-performance-debugging"></a>

## 19. Performance & debugging

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Nuxt DevTools</summary>

### Nuxt DevTools

**Definition:** Built-in debugging UI — inspect payload, components, routes, server tasks, and build analysis.

**How it works:** Enable in config (`devtools: { enabled: true }`). Payload tab shows serialized `useFetch` data — essential for double-fetch debugging.

**Examples:**

```ts
export default defineNuxtConfig({
  devtools: { enabled: true },
});
```

**Sources:** [devtools.nuxt.com](https://devtools.nuxt.com/)

</details>

<details>
  <summary>@nuxt/test-utils</summary>

### @nuxt/test-utils

**Definition:** Official testing helpers to mount Nuxt apps in **Vitest** with auto-imports, plugins, and route context — without starting a full production server.

**How it works:**

- `setup({ rootDir: '.' })` — boot Nuxt test environment once per suite
- `mountSuspended(Component)` — render with Suspense/async setup resolved
- `registerEndpoint('/api/x', handler)` — mock Nitro routes in tests
- Pairs with `@vue/test-utils` for component assertions

**When to use:** Integration tests for pages/composables that depend on `useFetch`, `useRoute`, or `useNuxtApp`. E2E still needs Playwright/Cypress.

**Examples:**

```ts
import { setup, $fetch } from "@nuxt/test-utils/e2e";

await setup({ rootDir: "." });
const html = await $fetch("/");
expect(html).toContain("Hello");
```

**See also:** [Vue Test Utils](./4_vue_ecosystem.md#testing) · [Testing notes](./7_special_testing.md)

**Sources:** [nuxt.com/docs/4.x/getting-started/testing](https://nuxt.com/docs/4.x/getting-started/testing)

</details>

<details>
  <summary>Hydration mismatch debugging</summary>

### Hydration mismatch debugging

**Definition:** Server HTML ≠ client VDOM on hydrate — Vue warns in console; users may see flicker or broken UI.

**Common causes:**

- Random IDs or dates rendered differently server vs client
- Browser-only APIs in universal setup
- Invalid HTML nesting in templates
- Third-party scripts mutating DOM before hydrate

**When to use:** Wrap client-only widgets in `<ClientOnly>`; defer randomness to `onMounted`.

**Thoughts:** Deep hydration theory → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Performance checklist</summary>

### Performance checklist

**Definition:** Practical levers for Nuxt production performance.

| Area           | Action                                                    |
| -------------- | --------------------------------------------------------- |
| **LCP**        | `@nuxt/image` + `priority` on hero; prerender landing     |
| **JS bundle**  | Route code-splitting (default); audit client-only imports |
| **TTFB**       | `pick`/`transform` payload; ISR/SWR for semi-static       |
| **Navigation** | `NuxtLink` prefetch; payload extraction for CDN           |
| **API**        | `routeRules` cache on read-heavy `server/api`             |
| **Build**      | `nuxt analyze` / Vite bundle visualizer                   |

**Examples:**

```bash
nuxt analyze
```

**Sources:** [nuxt.com/docs/4.x/guide/concepts/rendering](https://nuxt.com/docs/4.x/guide/concepts/rendering)

</details>

<details>
  <summary>Logging and server errors</summary>

### Logging and server errors

**Definition:** Server errors in `server/api` return h3 error responses; use `createError({ statusCode, message })` for consistent JSON errors.

**Examples:**

```ts
export default defineEventHandler((event) => {
  const id = getRouterParam(event, "id");
  if (!id) {
    throw createError({ statusCode: 400, statusMessage: "Missing id" });
  }
  return { id };
});
```

**Sources:** [nuxt.com/docs/4.x/getting-started/error-handling](https://nuxt.com/docs/4.x/getting-started/error-handling)

</details>
