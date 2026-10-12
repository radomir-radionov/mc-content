# Nuxt — Platform

Nitro, модули, config, деплой, миграция и отладка.

[← Вернуться в hub](./4_nuxt.md)

### Related docs

- [Nuxt — Ядро](./4_nuxt_core.md) · [Nuxt — Данные](./4_nuxt_data.md) · [Nuxt — Маршрутизация](./4_nuxt_routing.md) · [Nuxt — Платформа](./4_nuxt_platform.md) · [Nuxt — Интервью](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

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

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Nitro overview</summary>

### Nitro overview

**Definition:** **Nitro** — серверный движок Nuxt (UnJS / h3). Собирает server/API слой, обрабатывает SSR, prerendering и deployment presets — usable вне Nuxt.

**How it works:** В dev Rollup + Node workers изолируют server code. В production Nitro эмитит minimal `.output/` bundle (tree-shaken, без полного `node_modules`). Читает `server/api/` и `server/middleware/` для генерации routes.

**When to use:** Всегда — это server layer для каждого Nuxt app (даже static `generate` использует Nitro на этапе build).

**Pluses and minuses:**

| +                                            | −                                            |
| -------------------------------------------- | -------------------------------------------- |
| Universal deploy (Node, Workers, serverless) | Preset-specific quirks (edge runtime limits) |
| Fast cold starts (minimal bundle)            | Advanced features нуждаются в Nitro docs     |
| HMR для server routes в dev                  |                                              |
| Hybrid rendering через `routeRules`          |                                              |

**Examples:**

```bash
nuxt build
# .output/server/index.mjs — Node entry
# .output/public/ — static assets + prerendered HTML
```

**Thoughts:** Думайте о Nitro как об эквиваленте Nuxt custom server compiler + deployment adapter Next.js.

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server) · [nitro.build](https://nitro.build/)

</details>

<details>
  <summary>Modules ecosystem</summary>

### Modules ecosystem

**Definition:** Nuxt **modules** расширяют фреймворк — auto-register plugins, components, runtime config и build hooks. Устанавливаются через `modules` array в `nuxt.config`.

**How it works:** Modules выполняются на этапе build (`@nuxt/kit` helpers) и могут inject server routes, Vue plugins или Vite config.

**When to use:** `@nuxtjs/tailwindcss`, `@pinia/nuxt`, `@vueuse/nuxt`, `@nuxt/image`, `@nuxt/content`, i18n, auth.

**Pluses and minuses:**

| +                                  | −                                        |
| ---------------------------------- | ---------------------------------------- |
| One-line integrations              | Качество модулей различается             |
| Huge ecosystem на nuxt.com/modules | Version pins должны match Nuxt major     |
| Local modules через `modules/` dir | Over-module-ing скрывает, что делает app |

**Examples:**

```ts
export default defineNuxtConfig({
  modules: ["@pinia/nuxt", "@nuxtjs/tailwindcss", "@vueuse/nuxt"],
});
```

**Thoughts:** Curated official modules, которые стоит знать:

| Module             | Role                                                                |
| ------------------ | ------------------------------------------------------------------- |
| `@nuxt/ui`         | Tailwind-based UI primitives (Reka UI + Tailwind CSS v4)            |
| `@nuxt/content`    | File-based CMS — Markdown/YAML/JSON в `content/`                    |
| `@nuxt/image`      | Responsive images, lazy loading, provider integrations              |
| `@nuxt/eslint`     | Project-aware ESLint flat config                                    |
| `@nuxt/test-utils` | Mount Nuxt apps в Vitest — см. [@nuxt/test-utils](#nuxt-test-utils) |

**Sources:** [nuxt.com/modules](https://nuxt.com/modules)

</details>

<details>
  <summary>defineNuxtPlugin</summary>

### defineNuxtPlugin

**Definition:** Nuxt **plugins** запускаются во время app initialization — server, client или оба. Файлы в `app/plugins/` (или `plugins/` в root) экспортируют `defineNuxtPlugin((nuxtApp) => { … })`.

**How it works:**

| Pattern                       | Behavior                                             |
| ----------------------------- | ---------------------------------------------------- |
| Default plugin                | Runs on server + client                              |
| `*.client.ts` suffix          | Client-only (browser APIs)                           |
| `*.server.ts` suffix          | Server-only (DB, secrets)                            |
| `provide` / `nuxtApp.provide` | Inject helpers — access via `useNuxtApp().$myHelper` |
| `enforce: 'pre'`              | Run before other plugins (order)                     |

Отличается от **modules** (build-time framework extension) — plugins это runtime app setup.

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

**Definition:** Nuxt поставляет performance defaults — Vite bundling, route-level code splitting, payload extraction и optional `@nuxt/image` для responsive images.

**When to use:** Включите `@nuxt/image` для LCP images; настройте `experimental.payloadExtraction` для CDN payload caching.

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

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Route rules reference</summary>

### Route rules reference

**Definition:** `routeRules` в `nuxt.config.ts` — per-path Nitro + Nuxt поведение, скомпилированное на этапе build.

| Rule                | Effect                               |
| ------------------- | ------------------------------------ |
| `prerender: true`   | Static HTML на build                 |
| `ssr: false`        | Client-only route                    |
| `cache: { maxAge }` | Cache response                       |
| `isr` / `swr`       | Incremental / stale-while-revalidate |
| `redirect`          | HTTP redirect                        |
| `appLayout`         | Set layout by config (v4.3+)         |
| `cors`              | CORS headers на route                |

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


| | Route middleware                    | Server middleware         |
| | ---------------  |  -----------------------------------  |
| | **Location**    | `app/middleware/`                   | `server/middleware/`      |
| | **Runs on**     | Vue navigations + SSR page requests | All Nitro server requests |
| | **API routes**  | No                                  | Yes                       |
| | **Typical use** | Auth redirect, analytics            | JWT на `/api/*`, logging  |

**Thoughts:** Полная детализация → [Routing §11](./4_nuxt_routing.md#route-middleware) и [Data §9](./4_nuxt_data.md#server-middleware).

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<a name="16-configuration"></a>

## 16. Configuration

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>nuxt.config.ts</summary>

### nuxt.config.ts

**Definition:** Central config file — `defineNuxtConfig({ ... })` контролирует SSR, modules, `runtimeConfig`, `routeRules`, Vite/Nitro options и `app` settings.

**How it works:** Nuxt merge config из layers (если используются), применяет modules, генерирует `.nuxt/` build artifacts. Env vars override `runtimeConfig` с префиксом `NUXT_`.

**When to use:** Любое non-default поведение — deployment preset, base URL, module registration, hybrid rendering.

**Pluses and minuses:**

| +                      | −                                              |
| ---------------------- | ---------------------------------------------- |
| Single source of truth | Large configs выигрывают от `extends` / layers |
| Type-safe with schema  | Некоторые options переехали между majors       |
| Env-based overrides    |                                                |

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

**Definition:** **Layers** делят config, components и pages между проектами — monorepo design systems или white-label apps.

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

**Definition:** `.env` files питают `runtimeConfig`. Только ключи под `runtimeConfig.public` exposed в client bundle.

| Pattern                    | Access                                  |
| -------------------------- | --------------------------------------- |
| `NUXT_API_SECRET=...`      | `useRuntimeConfig().apiSecret` (server) |
| `NUXT_PUBLIC_API_BASE=...` | `useRuntimeConfig().public.apiBase`     |

**Examples:**

```env
NUXT_API_SECRET=super-secret
NUXT_PUBLIC_SITE_URL=https://example.com
```

**Thoughts:** Никогда не кладите секреты в `public` или Vue components.

**Sources:** [nuxt.com/docs/4.x/guide/going-further/runtime-config](https://nuxt.com/docs/4.x/guide/going-further/runtime-config)

</details>

<a name="17-deployment"></a>

## 17. Deployment

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Node.js server</summary>

### Node.js server

**Definition:** Default Nitro output — запуск `.output/server/index.mjs` на любом Node host.

**How it works:** `nuxt build` без static preset производит server + client assets. Учитывайте `PORT`, `HOST`, `NITRO_*` env vars.

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

**Definition:** Деплой `.output/public` после `nuxt generate` или prerender rules. Включает `200.html` / `404.html` SPA fallbacks и `_payload.json` per route.

**When to use:** Netlify, Cloudflare Pages, S3 + CloudFront, GitHub Pages (с `baseURL`).

**Examples:**

```bash
npx nuxt generate
npx serve .output/public
```

**Pluses and minuses:**

| +                | −                                                                    |
| ---------------- | -------------------------------------------------------------------- |
| Cheapest hosting | Нет server routes в runtime (API должен быть external или functions) |
| CDN-global       | Dynamic auth pages нуждаются в SSR host или edge functions           |

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<details>
  <summary>Serverless and edge presets</summary>

### Serverless and edge presets

**Definition:** Nitro **presets** target Vercel, Netlify Functions, Cloudflare Workers, AWS Lambda и др.

**How it works:** Задайте `nitro.preset` или `NITRO_PRESET` env на этапе build. Каждый preset адаптирует output format и entry handler.

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

**Thoughts:** Edge presets могут не иметь Node APIs — проверяйте DB drivers и file system usage.

**Sources:** [nitro.build/deploy](https://nitro.build/deploy)

</details>

<details>
  <summary>CDN and reverse proxy pitfalls</summary>

### CDN and reverse proxy pitfalls

**Definition:** Misconfigured CDNs ломают гидрацию — Cloudflare Rocket Loader, email obfuscation и HTML minification могут inject scripts или alter markup.

**When to use:** Всегда аудируйте CDN "optimization" settings для production Nuxt apps.

**Examples:**

Disable on Cloudflare:

1. Speed → Rocket Loader
2. Security → Email Address Obfuscation

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<a name="18-migration-pitfalls"></a>

## 18. Migration & pitfalls

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Nuxt 2 → 3 migration</summary>

### Nuxt 2 → 3 migration

**Definition:** Major rewrite — Vue 3, Composition API, Nitro вместо legacy server, `pages/` может жить под `app/`, новые data fetching composables.

| Nuxt 2                     | Nuxt 3/4                    |
| -------------------------- | --------------------------- |
| `asyncData` / `fetch` hook | `useAsyncData` / `useFetch` |
| `context.$axios`           | `$fetch` / `useFetch`       |
| `nuxtServerInit` (Vuex)    | Pinia + server plugins      |
| `modules` webpack build    | Vite + Nitro                |
| `middleware/` root         | `app/middleware/`           |
| `static/`                  | `public/`                   |

**When to use:** Incremental migration через [nuxt.com/docs/migration](https://nuxt.com/docs/migration) checklist; consider `@nuxt/bridge` только для interim Nuxt 2 codebases.

**Thoughts:** Greenfield должен стартовать на Nuxt 3/4 напрямую — bridge это stepping stone, не destination.

**Sources:** [nuxt.com/docs/migration/overview](https://nuxt.com/docs/migration/overview)

</details>

<details>
  <summary>Common pitfalls</summary>

### Common pitfalls

**Definition:** Частые production bugs в Nuxt apps.

| Pitfall                         | Symptom                          | Fix                                               |
| ------------------------------- | -------------------------------- | ------------------------------------------------- |
| `$fetch` в setup без `useFetch` | Double fetch, hydration flash    | Use `useFetch` / `useAsyncData`                   |
| `window` в setup                | SSR crash                        | `import.meta.client`, `onMounted`, `<ClientOnly>` |
| Route middleware only для auth  | API всё ещё open                 | Add `server/middleware` или handler checks        |
| Prerender без links             | Missing routes в static build    | `nitro.prerender.routes` или sitemap crawl        |
| Huge payload                    | Slow TTFB                        | `pick`, `transform`, smaller DTOs                 |
| Mismatched `useAsyncData` keys  | Dev warnings, stale shared state | Consistent `handler` + options per key            |
| `ssr: false` globally           | No SEO                           | Hybrid `routeRules` instead                       |

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Vue SPA → Nuxt migration</summary>

### Vue SPA → Nuxt migration

**Definition:** Перенос Vite + Vue Router SPA в Nuxt conventions.

**Steps (compressed):**

1. Move routes в `app/pages/` matching URL structure
2. Replace `axios` calls в `onMounted` с `useFetch` в `setup`
3. Extract shared layout в `layouts/default.vue`
4. Move API в `server/api/` или keep external API
5. Add `useSeoMeta` per page
6. Configure `routeRules` для static marketing pages

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<a name="19-performance-debugging"></a>

## 19. Performance & debugging

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Nuxt DevTools</summary>

### Nuxt DevTools

**Definition:** Built-in debugging UI — inspect payload, components, routes, server tasks и build analysis.

**How it works:** Enable в config (`devtools: { enabled: true }`). Payload tab показывает serialized `useFetch` data — essential для double-fetch debugging.

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

**Definition:** Official testing helpers для mount Nuxt apps в **Vitest** с auto-imports, plugins и route context — без запуска full production server.

**How it works:**

- `setup({ rootDir: '.' })` — boot Nuxt test environment один раз на suite
- `mountSuspended(Component)` — render с Suspense/async setup resolved
- `registerEndpoint('/api/x', handler)` — mock Nitro routes в tests
- Сочетается с `@vue/test-utils` для component assertions

**When to use:** Integration tests для pages/composables, зависящих от `useFetch`, `useRoute` или `useNuxtApp`. E2E всё ещё нужен Playwright/Cypress.

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

**Definition:** Server HTML ≠ client VDOM при hydrate — Vue warns в console; users могут видеть flicker или broken UI.

**Common causes:**

- Random IDs или dates rendered differently server vs client
- Browser-only APIs в universal setup
- Invalid HTML nesting в templates
- Third-party scripts mutating DOM before hydrate

**When to use:** Wrap client-only widgets в `<ClientOnly>`; defer randomness в `onMounted`.

**Thoughts:** Deep hydration theory → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>Performance checklist</summary>

### Performance checklist

**Definition:** Практические рычаги для Nuxt production performance.

| Area           | Action                                                    |
| -------------- | --------------------------------------------------------- |
| **LCP**        | `@nuxt/image` + `priority` на hero; prerender landing     |
| **JS bundle**  | Route code-splitting (default); audit client-only imports |
| **TTFB**       | `pick`/`transform` payload; ISR/SWR для semi-static       |
| **Navigation** | `NuxtLink` prefetch; payload extraction для CDN           |
| **API**        | `routeRules` cache на read-heavy `server/api`             |
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

**Definition:** Server errors в `server/api` return h3 error responses; use `createError({ statusCode, message })` для consistent JSON errors.

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
