# Vue — Ecosystem

Router, Pinia, Vite, performance, and migration.

[← Back to hub](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Ecosystem](./4_vue_ecosystem.md) · [Vue — Interview](./4_vue_interview.md) · [Architecture patterns](./7_special_architecture.md)

### Sections

- [9. Ecosystem & architecture](#9-ecosystem-architecture)
- [10. Performance](#10-performance)
- [11. Legacy & migration](#11-legacy-migration)

---

## 9. Ecosystem & architecture

Vue Router

<details>
  <summary>Vue Router</summary>

### Vue Router

**Definition:** **Vue Router** is the official client-side router for Vue SPAs. It maps URL paths to components, supports nested routes, navigation guards, lazy loading, and HTML5 history mode.

**How it works:**

1. Define route records: `{ path, component, children, meta }`
2. `createRouter({ history: createWebHistory(), routes })`
3. `<router-view />` renders matched component; `<router-link>` for navigation
4. **Guards:** `beforeEach`, `beforeEnter` — auth, redirects, data prefetch
5. Lazy routes: `component: () => import('./Page.vue')` — code splitting

**When to use:** Any multi-view Vue SPA. For file-based routing and SSR integration, use [Nuxt routing](./4_nuxt_routing.md).

**Examples:**

```ts
import { createRouter, createWebHistory } from "vue-router";

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/", component: Home },
    { path: "/users/:id", component: User, props: true },
  ],
});
```

**Pluses and minuses:**

| +                          | −                                      |
| -------------------------- | -------------------------------------- |
| Official, well-documented  | Manual route config vs file-based Nuxt |
| Navigation guards for auth | Data loading patterns vary by team     |

**Sources:**

[router.vuejs.org](https://router.vuejs.org/)

Router composables

</details>

<details>
  <summary>Router composables</summary>

### Router composables

**Definition:** **vue-router** exports Composition API helpers to react to navigation **inside** a component — complementing global `beforeEach` guards.

**How it works:**

| Composable            | Fires when                                                   |
| --------------------- | ------------------------------------------------------------ |
| `onBeforeRouteLeave`  | Component is about to be left (unsaved form guard)           |
| `onBeforeRouteUpdate` | Same component, different params (`/users/1` → `/users/2`)   |
| `useRoute()`          | Reactive access to current route (`params`, `query`, `meta`) |
| `useRouter()`         | Programmatic `push`, `replace`, `back`                       |

Must be called during `setup` or `<script setup>` (same rules as lifecycle hooks).

**When to use:** Per-page leave confirmation, refetch when `:id` changes, scroll restoration tied to one view. Global auth → `beforeEach` in router setup.

**Examples:**

```vue
<script setup>
import { onBeforeRouteLeave, onBeforeRouteUpdate } from "vue-router";

onBeforeRouteLeave((to, from) => {
  if (hasUnsavedChanges.value && !confirm("Discard changes?")) return false;
});

onBeforeRouteUpdate(async (to) => {
  user.value = await fetchUser(to.params.id);
});
</script>
```

**See also:** [Vue Router](#vue-router) · [Nuxt `useRoute` / `useRouter`](./4_nuxt_routing.md#useroute-and-userouter)

**Sources:**

[router.vuejs.org/guide/advanced/composition-api.html](https://router.vuejs.org/guide/advanced/composition-api.html)

Pinia

</details>

<details>
  <summary>Pinia</summary>

### Pinia

**Definition:** **Pinia** is the official Vue state store (Vuex successor). Stores are functions defining `state`, `getters`, and `actions` with full TypeScript support and DevTools integration.

**How it works:**

```ts
// stores/counter.ts
import { defineStore } from "pinia";

export const useCounterStore = defineStore("counter", {
  state: () => ({ count: 0 }),
  getters: {
    doubled: (s) => s.count * 2,
  },
  actions: {
    increment() {
      this.count++;
    },
  },
});
```

- Setup syntax: `defineStore('id', () => { const count = ref(0); return { count } })`
- No mutations — mutate `state` directly or in actions
- SSR: `createPinia()` per request; hydrate state from server payload

**When to use:** Cross-route shared state (auth user, cart, UI preferences). Local component state stays in `ref`/`reactive`.

**Pluses and minuses:**

| +                                | −                                                 |
| -------------------------------- | ------------------------------------------------- |
| Simpler than Vuex (no mutations) | Overkill for tiny apps — props/composables enough |
| Modular stores, code splitting   | Global store can become a god object              |

**See also:** [Pinia vs Vuex](#vuex-legacy)

**Sources:**

[pinia.vuejs.org](https://pinia.vuejs.org/)

Vite

</details>

<details>
  <summary>Vite</summary>

### Vite

**Definition:** **Vite** is the official build tool for Vue 3 — dev server with native ESM HMR and Rollup production builds. `@vitejs/plugin-vue` compiles SFCs.

**How it works:**

- Dev: serve source files on demand; compile `.vue` per request
- HMR updates SFC blocks independently (script/template/style)
- Production: tree-shake, code-split, asset hashing
- `import.meta.env` for env variables; `import.meta.glob` for file-based features

**When to use:** All new Vue projects. Replaces Vue CLI (webpack).

**Examples:**

```ts
// vite.config.ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";

export default defineConfig({
  plugins: [vue()],
});
```

**Thoughts:** Nuxt is built on Vite + Nitro — see [Nuxt platform](./4_nuxt_platform.md) for framework config, not duplicated here.

**Sources:**

[vitejs.dev](https://vitejs.dev/)

Vue plugins

</details>

<details>
  <summary>Vue plugins</summary>

### Vue plugins

**Definition:** A **plugin** is an object with `install(app, options)` (or a function) registered via `app.use(plugin, options)`. Plugins add global properties, directives, components, or provide/inject keys to the whole app.

**How it works:**

1. `app.use(i18n)` runs `install` once per app instance
2. Typical uses: router (`app.use(router)`), Pinia, i18n, telemetry, global `$filters` (legacy)
3. Prefer **composables** for reusable logic without global pollution
4. In `<script setup>`, access plugin APIs via imports (`useI18n`) not `this.$t`

**When to use:** Third-party libraries that ship as plugins; app-wide setup (router, pinia). Avoid plugins for feature logic that belongs in composables.

**Pluses and minuses:**

| +                            | −                                                                      |
| ---------------------------- | ---------------------------------------------------------------------- |
| Standard Vue extension point | Global state on `app.config.globalProperties` hurts TS and testability |
| One-line setup in `main.ts`  | Order of `app.use` calls can matter (router before mount)              |

**Examples:**

```ts
// plugins/telemetry.ts
import type { App } from "vue";

export default {
  install(app: App) {
    app.provide("track", (event: string) => console.log(event));
  },
};

// main.ts
import { createApp } from "vue";
import App from "./App.vue";
import telemetry from "./plugins/telemetry";

createApp(App).use(telemetry).mount("#app");
```

**Thoughts:** Nuxt wraps Vue plugins in `defineNuxtPlugin` — see [Nuxt plugins](./4_nuxt_platform.md#definenuxtplugin).

**Sources:**

[vuejs.org/guide/reusability/plugins.html](https://vuejs.org/guide/reusability/plugins.html)

v-html & XSS security

</details>

<details>
  <summary>v-html & XSS security</summary>

### v-html & XSS security

**Definition:** `v-html` renders raw HTML strings into the DOM. **Never** pass untrusted user input — it enables **XSS** (script injection). Vue templates escape text interpolations by default; `v-html` opts out.

**How it works:**

- `{{ userBio }}` — escaped text
- `<div v-html="userBio" />` — parsed as HTML; `<script>` or `onerror=` can execute
- SSR: serialized state in `__INITIAL_STATE__` must be JSON-escaped (Vue/Nuxt handle this; custom inline scripts do not)
- Sanitize on the server if you must render rich text (DOMPurify, CMS HTML whitelist)

**When to use:** Trusted CMS content, markdown rendered to HTML on the server. For user comments, use plain text or a sanitizer — not raw `v-html`.

**Pluses and minuses:**

| +                                 | −                                  |
| --------------------------------- | ---------------------------------- |
| Rich content from trusted sources | One mistake → stored XSS           |
| Simpler than manual `innerHTML`   | No automatic CSP bypass protection |

**See also:** [XSS in browser security](./6_web_api_security.md#cross-site-scripting-xss) · [SSR context](./4_vue_ssr.md#ssr-context)

**Sources:**

[vuejs.org/guide/best-practices/security.html](https://vuejs.org/guide/best-practices/security.html)

Forms

</details>

<details>
  <summary>Forms</summary>

### Forms

**Definition:** Vue handles forms with `v-model` on inputs, component `v-model`, and validation libraries.

**How it works:**

| Approach         | Notes                                 |
| ---------------- | ------------------------------------- |
| Native `v-model` | `text`, `checkbox`, `select`, `radio` |
| `defineModel`    | Custom inputs with two-way binding    |
| VeeValidate      | Schema + field-level validation       |
| FormKit          | Form builder components               |

**When to use:** Start with native `v-model` + computed validation. Add VeeValidate for complex multi-step forms.

**Examples:**

```vue
<script setup>
import { reactive } from "vue";
const form = reactive({ email: "", agree: false });
</script>
<template>
  <input v-model="form.email" type="email" />
  <input v-model="form.agree" type="checkbox" />
</template>
```

**Sources:**

[vuejs.org/guide/essentials/forms.html](https://vuejs.org/guide/essentials/forms.html)

Testing

</details>

<details>
  <summary>Testing</summary>

### Testing

**Definition:** Vue apps are tested with **Vitest** (unit), **Vue Test Utils** (component), and **Playwright/Cypress** (E2E).

**How it works:**

- `@vue/test-utils` — `mount(Component)`, `getByText`, `trigger`, `setProps`
- Stub child components, mock `provide`, test emitted events
- Pinia: `createTestingPinia()` for isolated stores

**Examples:**

```ts
import { mount } from "@vue/test-utils";
import Counter from "./Counter.vue";

const wrapper = mount(Counter);
await wrapper.get("button").trigger("click");
expect(wrapper.text()).toContain("1");
```

**Sources:**

[test-utils.vuejs.org](https://test-utils.vuejs.org/)

i18n

</details>

<details>
  <summary>i18n</summary>

### i18n

**Definition:** **vue-i18n** provides locale messages, pluralization, datetime/number formats, and Composition API (`useI18n`).

**When to use:** Multi-language SPAs. SSR apps need locale in server context — Nuxt has `@nuxtjs/i18n` module.

**Examples:**

```vue
<script setup>
import { useI18n } from "vue-i18n";
const { t, locale } = useI18n();
</script>
<template>
  <p>{{ t("welcome", { name: "Vue" }) }}</p>
</template>
```

**Sources:**

[vue-i18n.intlify.dev](https://vue-i18n.intlify.dev/)

State management patterns

</details>

<details>
  <summary>State management patterns</summary>

### State management patterns

**Definition:** Choose the smallest state container that fits the scope.

| Scope           | Tool                          |
| --------------- | ----------------------------- |
| Component local | `ref` / `reactive`            |
| Parent ↔ child  | props + emit / `v-model`      |
| Deep tree       | `provide` / `inject`          |
| App-wide client | Pinia                         |
| Server cache    | TanStack Query / custom fetch |
| URL shareable   | Vue Router query params       |

**When to use:** Don't put everything in Pinia — URL filters belong in route query; server lists belong in query cache.

**See also:** [provide / inject](./4_vue_core.md#provide-inject)

**Sources:**

[pinia.vuejs.org/core-concepts/](https://pinia.vuejs.org/core-concepts/)

</details>

---

## 10. Performance

Reactivity tuning

<details>
  <summary>Reactivity tuning</summary>

### Reactivity tuning

**Definition:** Avoid unnecessary reactive depth and subscribers when profiling shows cost.

**How it works:**

| Technique         | When                                    |
| ----------------- | --------------------------------------- |
| `shallowRef`      | Large immutable data replaced wholesale |
| `shallowReactive` | One-level object tracking               |
| `markRaw`         | Third-party class instances in state    |
| `v-once`          | Static subtree never updates            |
| `v-memo`          | Skip child update if deps unchanged     |

**Examples:**

```vue
<div v-memo="[item.id, item.selected]">
  <HeavyChild :item="item" />
</div>
```

**When to use:** After measuring — not preemptively. Vue 3 compiler already hoists static nodes.

**Sources:**

[vuejs.org/api/built-in-directives.html#v-memo](https://vuejs.org/api/built-in-directives.html#v-memo)

Async components & code splitting

</details>

<details>
  <summary>Async components & code splitting</summary>

### Async components & code splitting

**Definition:** Split bundles with dynamic `import()` on routes and heavy components so initial JS stays small.

**How it works:**

- Vue Router: `component: () => import('./views/Admin.vue')`
- `defineAsyncComponent` with loading/error UI
- Vite manual chunks in `build.rollupOptions.output.manualChunks`

**When to use:** Admin panels, charts, editors loaded on demand.

**See also:** [Async components](./4_vue_composition.md#async-components)

**Sources:**

[vuejs.org/guide/best-practices/performance.html](https://vuejs.org/guide/best-practices/performance.html)

List virtualization

</details>

<details>
  <summary>List virtualization</summary>

### List virtualization

**Definition:** Render only visible rows in long lists (10k+ items) using windowing libraries.

**How it works:**

- **vue-virtual-scroller** — official community pattern
- Fixed or dynamic row heights
- Pair with stable `:key` and avoid re-creating row components

**When to use:** Tables, feeds, log viewers with thousands of items.

**Sources:**

[vuejs.org/guide/best-practices/performance.html#virtualize-large-lists](https://vuejs.org/guide/best-practices/performance.html)

Web Vitals in Vue apps

</details>

<details>
  <summary>Web Vitals in Vue apps</summary>

### Web Vitals in Vue apps

**Definition:** Measure **LCP**, **INP**, **CLS** in production Vue apps — same as any SPA, with Vue-specific levers.

**How it works:**

| Metric  | Vue lever                                                                                |
| ------- | ---------------------------------------------------------------------------------------- |
| **LCP** | SSR/SSG (Nuxt) or prerender hero; lazy routes; `fetchpriority="high"` on hero `<img>`    |
| **INP** | Avoid long sync work in `watch`/`computed`; `v-memo`, `defineAsyncComponent`, route lazy |
| **CLS** | `width`/`height` on images; reserve slots for async banners; no DOM insert above fold    |

- Analyze bundle: `rollup-plugin-visualizer` / Nuxt DevTools
- Prefer SSR/SSG for marketing and content pages — pure SPA hurts LCP
- Lazy-load route components in Vue Router / Nuxt pages
- INP phases and attribution → [6_web_api.md — INP](./1_html_performance.md#inp-interaction-to-next-paint)
- CLS causes in HTML/CSS → [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift)

**When to use:** Production RUM on Nuxt/Vite builds; Lighthouse CI budgets before release.

**Examples:**

Field RUM — register once at app bootstrap (Nuxt plugin or `main.js`). Flush on `visibilitychange` → `hidden` via `sendBeacon`. Full pattern → [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals).

```ts
// plugins/web-vitals.client.ts (Nuxt)
import { onCLS, onINP, onLCP } from "web-vitals";

export default defineNuxtPlugin(() => {
  const send = (metric) =>
    navigator.sendBeacon("/analytics", JSON.stringify(metric));
  onCLS(send);
  onINP(send);
  onLCP(send);
});
```

```js
// main.js (Vite SPA)
import { createApp } from "vue";
import { onCLS, onINP, onLCP } from "web-vitals";
import App from "./App.vue";

const send = (metric) =>
  navigator.sendBeacon("/analytics", JSON.stringify(metric));
onCLS(send);
onINP(send);
onLCP(send);

createApp(App).mount("#app");
```

**LCP** — lazy route chunks keep the first paint small:

```js
// router.js
const routes = [
  { path: "/dashboard", component: () => import("./pages/Dashboard.vue") },
];
```

**INP** — skip re-rendering unchanged list rows:

```vue
<li v-for="item in list" :key="item.id" v-memo="[item.id, item.active]">
  {{ item.name }}
</li>
```

**CLS** — reserve hero space before data arrives:

```vue
<img
  v-if="hero"
  :src="hero"
  width="1200"
  height="630"
  alt=""
  fetchpriority="high"
/>
<div v-else style="width: 1200px; height: 630px" aria-hidden />
```

**Thoughts:** _Same field metrics as any SPA — Vue levers are SSR, route splitting, and stable list rendering. Measure production, not dev HMR._

**Sources:**

[web.dev/articles/vitals](https://web.dev/articles/vitals) · [web-vitals](https://github.com/GoogleChrome/web-vitals) · [vuejs.org — performance](https://vuejs.org/guide/best-practices/performance.html)

</details>

---

## 11. Legacy & migration

Options API

<details>
  <summary>Options API</summary>

### Options API

**Definition:** Vue 2-style component options: `data`, `methods`, `computed`, `watch`, lifecycle hooks on the component object. Still fully supported in Vue 3.

**How it works:**

```js
export default {
  data() {
    return { count: 0 };
  },
  computed: {
    doubled() {
      return this.count * 2;
    },
  },
  methods: {
    inc() {
      this.count++;
    },
  },
  mounted() {
    console.log("mounted");
  },
};
```

**When to use:** Maintaining legacy codebases. New features should use Composition API + `<script setup>`.

**Pluses and minuses:**

| +                      | −                                     |
| ---------------------- | ------------------------------------- |
| Familiar to Vue 2 devs | Logic split by option type            |
| No `.value` for data   | Harder TS inference than script setup |

**Sources:**

[vuejs.org/guide/introduction.html#api-styles](https://vuejs.org/guide/introduction.html#api-styles)

Vuex (legacy)

</details>

<details>
  <summary>Vuex (legacy)</summary>

### Vuex (legacy)

**Definition:** **Vuex 4** works with Vue 3 but is in maintenance mode. **Pinia** is the official replacement — no mutations, better TS, smaller API.

**How it works:**

- Vuex: `state`, `mutations` (sync), `actions` (async), `getters`, modules
- Pinia: direct state mutation in actions, simpler store definition

**When to use:** Migrate Vuex → Pinia incrementally (store-by-store). New projects: Pinia only.

**Sources:**

[pinia.vuejs.org/cookbook/migration-vuex.html](https://pinia.vuejs.org/cookbook/migration-vuex.html)

Vue 2 → 3 migration

</details>

<details>
  <summary>Vue 2 → 3 migration</summary>

### Vue 2 → 3 migration

**Definition:** Migrating from Vue 2 EOL to Vue 3 involves breaking changes in global API, reactivity, filters, events, and third-party library updates.

**How it works:**

| Breaking change               | Migration action                   |
| ----------------------------- | ---------------------------------- |
| `new Vue()`                   | `createApp()`                      |
| `Vue.use` / global APIs       | `app.use()` per app instance       |
| Filters removed               | computed or methods                |
| `$on` / `$off` / `$once`      | external mitt or Pinia             |
| `v-model` on components       | `modelValue` + `update:modelValue` |
| Functional / async components | new APIs                           |
| Vue Router 3 → 4              | new `createRouter` API             |
| Vuex → Pinia                  | rewrite stores                     |

**Migration build:** `@vue/compat` flags Vue 2 patterns with runtime warnings while you fix code incrementally.

**When to use:** Any remaining Vue 2 app. Plan dependency audit (UI libs, test utils) before starting.

**Sources:**

[v3-migration.vuejs.org](https://v3-migration.vuejs.org/)

Migration build (@vue/compat)

</details>

<details>
  <summary>Migration build (@vue/compat)</summary>

### Migration build (@vue/compat)

**Definition:** `@vue/compat` is a Vue 3 build with Vue 2 compatibility mode — logs deprecation warnings and supports gradual migration.

**How it works:**

1. Swap `vue` → `@vue/compat` in package.json
2. Configure compat flags in `vite.config` / webpack alias
3. Fix warnings file by file
4. Remove compat when clean

**When to use:** Large Vue 2 codebases that cannot big-bang migrate.

**Sources:**

[v3-migration.vuejs.org/migration-build.html](https://v3-migration.vuejs.org/migration-build.html)

Vue CLI (legacy)

</details>

<details>
  <summary>Vue CLI (legacy)</summary>

### Vue CLI (legacy)

**Definition:** **Vue CLI** (webpack-based) scaffolded Vue 2/3 projects with `vue create`. Official recommendation is now **create-vue** (Vite template).

**When to use:** Maintain existing Vue CLI projects. New projects: `npm create vue@latest`.

**Examples:**

```bash
npm create vue@latest
# Options: TypeScript, Router, Pinia, Vitest, ESLint
```

**Sources:**

[github.com/vuejs/create-vue](https://github.com/vuejs/create-vue)

Migration checklist

</details>

<details>
  <summary>Migration checklist</summary>

### Migration checklist

**Definition:** Practical order for Vue 2 → 3 production migration.

**How it works:**

1. Upgrade tooling to Vue 3 + Vite (or compat build on webpack first)
2. Replace global API (`createApp`, `app.component`)
3. Update Vue Router 4, Pinia instead of Vuex
4. Fix `v-model` / emit renames on shared components
5. Remove filters, `$listeners` merges into `$attrs`
6. Update tests (`@vue/test-utils` v2)
7. Audit UI libraries (Vuetify 3, Element Plus, etc.)
8. Enable SSR only after client app is stable — or migrate to [Nuxt](./4_nuxt_core.md)

**Thoughts:** Run `npm run build` and fix compat warnings in CI. Don't mix Options and Composition in one component without reason.

**Sources:**

[v3-migration.vuejs.org/migration-strategy.html](https://v3-migration.vuejs.org/migration-strategy.html)

</details>
