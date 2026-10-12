# Vue — SSR

Vue 3.5+ features, server rendering, and hydration.

[← Back to hub](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Ecosystem](./4_vue_ecosystem.md) · [Vue — Interview](./4_vue_interview.md) · [Nuxt — Core](./4_nuxt_core.md)

### Sections

- [7. Vue 3.5+ features](#7-vue-35-features)
- [8. SSR & hydration](#8-ssr-hydration)

---

## 7. Vue 3.5+ features

Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** Vue 3.5 (2024) stabilizes **reactive props destructure**, adds **`useTemplateRef()`**, improves SSR hydration performance, and ships reactivity system optimizations (memory, array tracking).

**How it works:**

| Feature                    | What changed                                      |
| -------------------------- | ------------------------------------------------- |
| Reactive props destructure | `const { foo = 1 } = defineProps()` stays tracked |
| `useTemplateRef()`         | Runtime string-id template refs                   |
| SSR hydration              | Faster attribute/prop hydration, fewer mismatches |
| Reactivity                 | ~56% less memory for large reactive arrays (3.5)  |

**When to use:** Upgrade Vue 3.4+ projects to 3.5 for DX wins. No breaking changes for typical apps.

**Thoughts:** Nuxt 3.13+ bundles Vue 3.5 — see [Nuxt platform notes](./4_nuxt_platform.md) for framework-level SSR, not duplicated here.

**Sources:**

[blog.vuejs.org/posts/vue-3-5](https://blog.vuejs.org/posts/vue-3-5)

Reactive props destructure

</details>

<details>
  <summary>Reactive props destructure</summary>

### Reactive props destructure

**Definition:** In Vue 3.5+, variables destructured from `defineProps()` are **reactive** — the compiler rewrites reads to `props.foo`.

**How it works:**

- Before 3.5: `const { foo } = defineProps()` — `foo` was a frozen snapshot
- 3.5+: `watchEffect(() => console.log(foo))` re-runs when prop changes
- Passing to `watch(foo, …)` still errors — pass `() => foo` or use `toValue` in composables
- Default values use native JS syntax: `const { count = 0 } = defineProps<{ count?: number }>()`

**Examples:**

```vue
<script setup lang="ts">
const { title = 'Untitled' } = defineProps<{ title?: string }>()

watch(() => title, (t) => console.log('title:', t))
</script>
```

**When to use:** Cleaner props with defaults without `withDefaults`.

**Sources:**

[vuejs.org/guide/components/props.html#reactive-props-destructure](https://vuejs.org/guide/components/props.html#reactive-props-destructure)

useTemplateRef

</details>

<details>
  <summary>useTemplateRef</summary>

### useTemplateRef

**Definition:** Vue 3.5 API to obtain template refs by string id. Works with dynamic `ref` bindings; pairs with Volar autocomplete.

**How it works:**

- `const el = useTemplateRef<HTMLInputElement>('my-input')` ↔ `<input ref="my-input">`
- Value is `null` until mounted; use `onMounted` for DOM APIs
- Replaces pattern of declaring `ref(null)` with matching variable name (still valid pre-3.5)

**Examples:**

```vue
<script setup>
import { useTemplateRef, onMounted } from 'vue'

const input = useTemplateRef('search')
onMounted(() => input.value?.focus())
</script>

<template>
  <input ref="search" type="search" />
</template>
```

**See also:** [Template refs](./4_vue_composition.md#template-refs)

**Sources:**

[vuejs.org/guide/essentials/template-refs.html](https://vuejs.org/guide/essentials/template-refs.html)

defineModel (stable)

</details>

<details>
  <summary>defineModel (stable)</summary>

### defineModel (stable)

**Definition:** `defineModel()` became the recommended `v-model` implementation in Vue 3.4+ and is the default pattern in 3.5 docs. Replaces verbose prop + emit boilerplate.

**How it works:**

- Expands to `modelValue` + `update:modelValue` (or named variant)
- Supports modifiers via destructuring: `const [model, modifiers] = defineModel()`
- Custom `get`/`set` for transform (e.g. trim, capitalize)

**Examples:**

```vue
<script setup>
const model = defineModel<string>()
const [text, modifiers] = defineModel('text')
</script>
```

**Sources:**

[vuejs.org/guide/components/v-model.html](https://vuejs.org/guide/components/v-model.html)

App config & perf (3.5)

</details>

<details>
  <summary>App config & perf (3.5)</summary>

### App config & perf (3.5)

**Definition:** Vue 3.5 improves internal scheduler and hydration without new public APIs for most apps. `app.config` options remain the extension point.

**How it works:**

- `app.config.errorHandler` — global render errors
- `app.config.warnHandler` — custom warning filter
- `app.config.performance` — perf marks in dev
- `app.config.compilerOptions` — custom elements, whitespace

**When to use:** Centralized error reporting (Sentry), suppress known warnings in legacy migration.

**Sources:**

[vuejs.org/api/application.html#app-config](https://vuejs.org/api/application.html#app-config)

</details>

---

## 8. SSR & hydration

SPA vs SSR

<details>
  <summary>SPA vs SSR</summary>

### SPA vs SSR

**Definition:** **SPA** ships empty HTML + JS bundle; browser renders everything client-side. **SSR** renders HTML on the server so users see content before JS loads; client **hydrates** to make it interactive.

**How it works:**

| Mode | First paint        | SEO           | Complexity |
| ---- | ------------------ | ------------- | ---------- |
| SPA  | Shell + loading    | Needs prerender/meta handling | Low (Vite SPA) |
| SSR  | Full HTML content  | Better crawlers | Higher (dual env, hydration) |
| SSG  | Pre-built HTML     | Excellent     | Build-time render |

**When to use:** SSR for marketing + app hybrid, authenticated dashboards with SEO pages. Pure admin SPA often stays client-only.

**Thoughts:** For file-based SSR, data fetching, and Nitro server routes, use [Nuxt](./4_nuxt_core.md) — don't duplicate Nuxt APIs here.

**See also:** [Hydration](#hydration) · [Nuxt — Core](./4_nuxt_core.md)

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html](https://vuejs.org/guide/scaling-up/ssr.html)

createSSRApp & renderToString

</details>

<details>
  <summary>createSSRApp & renderToString</summary>

### createSSRApp & renderToString

**Definition:** Server entry uses `createSSRApp` instead of `createApp`. `@vue/server-renderer` exposes `renderToString` / `renderToNodeStream` to produce HTML from the VNode tree.

**How it works:**

1. Server creates app per request (or uses cached template)
2. `renderToString(app)` returns HTML string + teleports content
3. Client calls `createSSRApp` + `app.mount('#app')` — **hydration** attaches listeners
4. Vite SSR: separate `entry-server.ts` and `entry-client.ts` bundles

**Examples:**

```ts
// server
import { createSSRApp } from 'vue'
import { renderToString } from 'vue/server-renderer'
import App from './App.vue'

const app = createSSRApp(App)
const html = await renderToString(app)
```

```ts
// client
import { createSSRApp } from 'vue'
createSSRApp(App).mount('#app')
```

**When to use:** Custom SSR with Vite. Prefer Nuxt for production SSR stack.

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html#rendering-an-app](https://vuejs.org/guide/scaling-up/ssr.html)

Hydration

</details>

<details>
  <summary>Hydration</summary>

### Hydration

**Definition:** **Hydration** is the client-side process of reusing server-rendered DOM, attaching event listeners, and reconciling the client VNode tree with existing HTML.

**How it works:**

1. Server HTML must match what client first render would produce
2. Mismatch → Vue warns and may **discard** server DOM and re-render (bad for perf/UX)
3. Common causes: `Date.now()`, `Math.random()`, browser-only APIs in setup, invalid HTML nesting
4. `data-allow-mismatch` (3.5+) for intentional differences (e.g. timestamps)

**When to use:** Debug hydration warnings in dev before shipping SSR.

**Pluses and minuses:**

| +                              | −                                    |
| ------------------------------ | ------------------------------------ |
| Fast interactive first paint   | Strict server/client parity required |
| SEO-friendly content in HTML   | Mismatch recovery is expensive       |

**Examples:**

```vue
<!-- Avoid in SSR setup -->
<script setup>
// BAD: different on server vs client
const id = Math.random()
</script>
```

**See also:** [SSR context](#ssr-context)

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html#hydration-mismatch](https://vuejs.org/guide/scaling-up/ssr.html)

SSR context

</details>

<details>
  <summary>SSR context</summary>

### SSR context

**Definition:** Per-request data passed from server render to client via **`payload`** or inlined `window.__INITIAL_STATE__` — avoids refetching on hydration.

**How it works:**

- Server: `renderToString(app, { idPrefix })` or Vite SSR `context`
- Attach state to HTML as serialized JSON (escape carefully — XSS risk)
- Client: `app.mount` reads payload and initializes Pinia / reactive state
- Nuxt: `useState`, `useAsyncData` handle this — see [Nuxt data](./4_nuxt_data.md)

**When to use:** User session, CMS page data, cart snapshot on first paint.

**Thoughts:** Never `JSON.stringify` user-controlled strings into `<script>` without sanitization.

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html#data-prefetching](https://vuejs.org/guide/scaling-up/ssr.html)

Suspense & streaming

</details>

<details>
  <summary>Suspense & streaming</summary>

### Suspense & streaming

**Definition:** SSR can **stream** HTML as async components resolve, sending fallbacks first and replacing when data arrives — coordinated by `<Suspense>` boundaries.

**How it works:**

1. Server hits async `setup()` or async component
2. Renders `#fallback` slot HTML immediately
3. When Promise resolves, streams the resolved segment
4. Client hydrates incrementally as chunks arrive

**When to use:** Slow API sections on otherwise fast pages. Nuxt and Vite SSR plugins implement streaming pipelines.

**See also:** [Suspense](./4_vue_composition.md#suspense)

**Sources:**

[vuejs.org/guide/built-ins/suspense.html#ssr-usage](https://vuejs.org/guide/built-ins/suspense.html)

Client-only components

</details>

<details>
  <summary>Client-only components</summary>

### Client-only components

**Definition:** Components or subtrees that must **not** render on server (browser APIs, charts, maps). Wrapped to render placeholder on server, real content on client.

**How it works:**

- `<ClientOnly>` wrapper in Nuxt — see [Nuxt platform](./4_nuxt_platform.md)
- Manual pattern: `import.meta.env.SSR` guard in `setup`
- `onMounted`-only render avoids hydration mismatch

**When to use:** `window`, `localStorage`, WebGL, third-party widgets without SSR support.

**Examples:**

```vue
<script setup>
import { ref, onMounted } from 'vue'
const show = ref(false)
onMounted(() => { show.value = true })
</script>
<template>
  <Chart v-if="show" />
  <div v-else class="chart-placeholder" />
</template>
```

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html#bootstrapping-client-applications](https://vuejs.org/guide/scaling-up/ssr.html)

</details>
