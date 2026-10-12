# Vue — Core

Library model, rendering, templates, and state.

[← Back to hub](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Ecosystem](./4_vue_ecosystem.md) · [Vue — Interview](./4_vue_interview.md)

### Sections

- [1. Library overview](#1-library-overview)
- [2. Rendering model](#2-rendering-model)
- [3. Describing the UI](#3-describing-the-ui)
- [4. State & interactivity](#4-state-interactivity)

---

## 1. Library overview

Vue overview

<details>
  <summary>Vue overview</summary>

### Vue overview

**Definition:** Vue is a **progressive** JavaScript framework for building user interfaces. It focuses on the view layer but scales to full SPAs with official routing (Vue Router) and state (Pinia). Vue 3 uses a proxy-based reactivity system and a compiler that optimizes templates at build time.

**When to use:** SPAs, dashboards, design systems, embedded widgets (`createApp` on a node), SSR apps (Vite SSR, or meta-frameworks like [Nuxt](./4_nuxt_core.md)).

**Pluses and minuses:**

| +                                                             | −                                                           |
| ------------------------------------------------------------- | ----------------------------------------------------------- |
| Gentle learning curve; templates feel like HTML               | Two APIs (Options + Composition) can confuse newcomers      |
| Excellent docs and official tooling (Vite, Vue Router)        | Smaller ecosystem than React for some niches (RSC, RN)      |
| Fine-grained reactivity — updates track dependencies          | Template compiler adds build step (SFC) vs pure JSX runtime |
| `<script setup>` + TypeScript inference is first-class        | Vue 2 legacy codebases need migration planning              |
| Progressive adoption — drop a widget on a page or go full SPA | Nuxt-specific patterns are separate from core Vue           |

**Examples:**

```vue
<script setup>
import { ref } from "vue";

const count = ref(0);
</script>

<template>
  <button @click="count++">{{ count }}</button>
</template>
```

```ts
import { createApp } from "vue";
import App from "./App.vue";

createApp(App).mount("#app");
```

**Thoughts:** Vue is often described as a framework because the core team ships Router, Pinia, and Vite integration — but at its heart Vue is still a **view library** you mount yourself. For file-based routing and Nitro server, use [Nuxt](./4_nuxt_core.md) instead of hand-rolling.

**Sources:**

[vuejs.org](https://vuejs.org/)

Vue vs Vue DOM

</details>

<details>
  <summary>Vue vs Vue DOM</summary>

### Vue vs Vue DOM

**Definition:** `vue` = core (reactivity, components, compiler). `@vue/runtime-dom` (re-exported via `vue`) = web renderer that patches the real DOM. Same major version for all `@vue/*` packages.

**How it works:**

| Package / import       | Role                                        |
| ---------------------- | ------------------------------------------- |
| `vue`                  | Reactivity, components, `createApp`         |
| `vue` (runtime-dom)    | `mount`, `patch`, event listeners on DOM    |
| `@vue/server-renderer` | `renderToString`, SSR context               |
| `@vue/compiler-sfc`    | Compile `.vue` SFCs (Vite plugin uses this) |

`createApp` creates an app instance; `mount('#app')` triggers the first render and attaches listeners. Updates flow through the reactivity → render → patch pipeline.

**When to use:** Always `vue` on web. SSR adds `@vue/server-renderer`. Custom renderers exist for native (`@vue/runtime-native`) but are niche.

**Examples:**

```ts
import { createApp } from "vue";
import { createSSRApp } from "vue"; // SSR entry

createApp(App).mount("#app");
```

**Thoughts:** Unlike React's explicit `react` + `react-dom` split in imports, most Vue apps import everything from `vue`. The separation still exists under the hood.

**See also:** [Render pipeline](#render-pipeline) · [SSR & hydration](./4_vue_ssr.md#8-ssr-hydration)

**Sources:**

[vuejs.org/api/application.html](https://vuejs.org/api/application.html)

Library vs framework

</details>

<details>
  <summary>Library vs framework</summary>

### Library vs framework

**Definition:** A **library** solves a focused problem (UI rendering); you control architecture. A **framework** inverts control — it defines project structure, routing, and data conventions.

|               | Library (Vue core)    | Framework (Nuxt)                    |
| ------------- | --------------------- | ----------------------------------- |
| Control       | You compose the stack | Framework prescribes conventions    |
| Routing       | Bring Vue Router      | File-based routes built-in          |
| Data fetching | Bring your own        | `useFetch`, server routes via Nitro |
| SSR           | Manual Vite SSR setup | SSR/SSG by default                  |

**Examples:**

```ts
// Library — you wire routing yourself
import { createRouter, createWebHistory } from "vue-router";

// Framework — Nuxt owns file-based routes and server data
// See 4_nuxt_routing.md, 4_nuxt_data.md
```

**Thoughts:** Interview answer: Vue **core** is a progressive UI library. **Nuxt** is the full-stack Vue framework. Vue Router + Pinia + Vite is a common "framework-like" stack without Nuxt.

**Sources:**

[vuejs.org/guide/extras/ways-of-using-vue.html](https://vuejs.org/guide/extras/ways-of-using-vue.html)

Philosophy

</details>

<details>
  <summary>Philosophy</summary>

### Philosophy

**Definition:** Vue's design goals: **approachable** (HTML-like templates), **versatile** (widget → SPA), **performant** (compiler hints + proxy reactivity).

**How it works:**

1. **Templates** compile to render functions that call reactivity-tracked reads
2. **Single-file components** colocate template, logic, and scoped styles
3. **Progressive enhancement** — use as script tag, Vite SPA, or Nuxt full-stack
4. **Composition API** (Vue 3) groups logic by feature, not option type

**When to use:** Teams that want declarative templates with less JSX ceremony; gradual adoption in legacy pages.

**Pluses and minuses:**

| +                                   | −                                           |
| ----------------------------------- | ------------------------------------------- |
| HTML-familiar templates             | Template + script two-language mental model |
| Official style guide and tooling    | Options API vs Composition API duality      |
| Compiler can optimize static hoists | SFC requires build pipeline                 |

**Thoughts:** Vue 3 recommends Composition API + `<script setup>` for new code. Options API remains supported for maintenance and gradual migration.

**Sources:**

[vuejs.org/guide/introduction.html#what-is-vue](https://vuejs.org/guide/introduction.html#what-is-vue)

Vue 3 vs Vue 2

</details>

<details>
  <summary>Vue 3 vs Vue 2</summary>

### Vue 3 vs Vue 2

**Definition:** Vue 3 (2020) rewrote reactivity with **Proxies** (replacing `Object.defineProperty`), introduced the **Composition API**, **Fragments**, **Teleport**, better TypeScript support, and a smaller tree-shaken bundle.

**How it works:**

| Area           | Vue 2                         | Vue 3                              |
| -------------- | ----------------------------- | ---------------------------------- |
| Reactivity     | `defineProperty`, array hacks | `Proxy`-based, Map/Set native      |
| API            | Options API primary           | Composition API + Options API      |
| Multiple roots | Single root required          | Fragments (multiple root nodes)    |
| Global API     | `Vue.use`, `Vue.component`    | `createApp` — per-app registration |
| State          | Vuex 3/4                      | Pinia (official)                   |
| Build          | Vue CLI (webpack)             | Vite (official)                    |

**When to use:** All new projects on Vue 3. Vue 2 reached EOL Dec 2023 — migrate or use `@vue/compat` migration build temporarily.

**See also:** [Legacy & migration](./4_vue_ecosystem.md#11-legacy-migration)

**Sources:**

[vuejs.org/guide/introduction.html#vue-2-vs-vue-3](https://vuejs.org/guide/introduction.html#vue-2-vs-vue-3)

</details>

---

## 2. Rendering model

Render pipeline

<details>
  <summary>Render pipeline</summary>

### Render pipeline

**Definition:** Vue turns component state into a **VNode tree**, diffs it against the previous tree, and **patches** the real DOM with minimal operations.

**How it works:**

1. **Reactive dependency** changes (e.g. `count.value++`)
2. **Scheduler** queues the component's render effect
3. **Render function** (from template compiler or `render()`) runs → new VNodes
4. **Patch algorithm** compares old vs new VNodes (same type + `key` → update; type change → replace)
5. **DOM update** applies property/event/text changes

```
state change → effect → render() → VNode tree → patch → DOM
```

**When to use:** Understanding this explains why only accessed reactive data triggers updates, and why `key` matters in `v-for`.

**Pluses and minuses:**

| +                                                    | −                                        |
| ---------------------------------------------------- | ---------------------------------------- |
| Compiler static hoisting skips diff for static nodes | Abstractions hide when re-render happens |
| Block tree (Vue 3) speeds dynamic children           | Deep component trees still cost render   |

**See also:** [Virtual DOM & patch](#virtual-dom-patch) · [Reactivity system](#reactivity-system)

**Sources:**

[vuejs.org/guide/extras/rendering-mechanism.html](https://vuejs.org/guide/extras/rendering-mechanism.html)

Virtual DOM & patch

</details>

<details>
  <summary>Virtual DOM & patch</summary>

### Virtual DOM & patch

**Definition:** A **VNode** (virtual node) is a plain object describing a DOM element or component (`type`, `props`, `children`, `key`). Vue does not diff the live DOM — it diffs VNode trees.

**How it works:**

- **Element nodes** — tag, props, children
- **Component nodes** — component constructor + props + slots
- **Text / comment / Fragment** — lightweight node types
- **Patch flags** (compiler-generated) tell runtime which props actually changed — skip full prop diff

**When to use:** Debug unnecessary re-renders; understand hydration mismatches (server VNode ≠ client VNode).

**Examples:**

```js
// Conceptual VNode shape (simplified)
{ type: 'div', props: { id: 'app' }, children: [...] }
{ type: MyComponent, props: { msg: 'hi' } }
```

**Thoughts:** Vue 3's compiler marks dynamic bindings with patch flags — this is why SFC templates can outperform hand-written render functions that over-diff.

**See also:** [Keys](#keys)

**Sources:**

[vuejs.org/guide/extras/rendering-mechanism.html#virtual-dom](https://vuejs.org/guide/extras/rendering-mechanism.html)

Reactivity system

</details>

<details>
  <summary>Reactivity system</summary>

### Reactivity system

**Definition:** Vue 3 tracks **property access** at runtime via `Proxy` wrappers. When tracked data is read during render, the render effect subscribes; when data is written, subscribers re-run.

**How it works:**

1. `ref()` / `reactive()` wrap values in reactive proxies
2. **Track** on `get` (during render, `computed`, `watch` getters)
3. **Trigger** on `set` → schedule component/effect re-run
4. `effect` batches updates in a microtask queue (same tick coalesced)

| API          | Wraps                  | Access in script                    |
| ------------ | ---------------------- | ----------------------------------- |
| `ref`        | Any value (object too) | `.value`                            |
| `reactive`   | Objects only           | direct property                     |
| `computed`   | Derived read-only ref  | `.value` or auto-unwrap in template |
| `shallowRef` | One-level ref          | `.value` — deep changes not tracked |

**When to use:** Default state primitive is `ref`. Use `reactive` for form objects you mutate in place. Use `computed` for derived values.

**See also:** [ref](#ref) · [reactive](#reactive) · [computed](#computed)

**Sources:**

[vuejs.org/guide/essentials/reactivity-fundamentals.html](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)

Compiler vs runtime

</details>

<details>
  <summary>Compiler vs runtime</summary>

### Compiler vs runtime

**Definition:** Vue SFC templates are **compiled at build time** into render functions. The **runtime** executes those functions and patches the DOM. You can use Vue without a compiler via `runtime-only` build + `render()` functions.

**How it works:**

| Build                                 | Includes compiler | Use case                           |
| ------------------------------------- | ----------------- | ---------------------------------- |
| `vue` (full)                          | Yes               | In-browser template compile (rare) |
| `vue/dist/vue.runtime.esm-bundler.js` | No                | Vite/Vue CLI — SFC pre-compiled    |

Compiler optimizations: static hoisting, patch flags, block trees, event handler caching.

**When to use:** Always pre-compile SFCs in production (Vite does this). Runtime-only is smaller.

**Sources:**

[vuejs.org/guide/scaling-up/tooling.html](https://vuejs.org/guide/scaling-up/tooling.html)

Keys

</details>

<details>
  <summary>Keys</summary>

### Keys

**Definition:** The `:key` attribute on `v-for` items gives each VNode a **stable identity** across re-renders so Vue can reuse/move DOM nodes instead of destroying and recreating them.

**How it works:**

- Same `type` + same `key` → update in place
- Missing or duplicate keys → unpredictable reuse, state bugs
- Index as key → OK only for **static** lists that never reorder

**When to use:** Always use a unique stable id (`item.id`) for dynamic lists (filter, sort, paginate).

**Examples:**

```vue
<li v-for="todo in todos" :key="todo.id">{{ todo.text }}</li>
<!-- Avoid :key="index" when list mutates order -->
```

**Pluses and minuses:**

| +                                    | −                          |
| ------------------------------------ | -------------------------- |
| Preserves input focus/state in lists | Wrong key → subtle UI bugs |
| Enables efficient list diff          | —                          |

**See also:** [Conditional rendering & lists](#conditional-rendering-lists)

**Sources:**

[vuejs.org/guide/essentials/list.html#maintaining-state-with-key](https://vuejs.org/guide/essentials/list.html)

</details>

---

## 3. Describing the UI

Single-File Components (SFC)

<details>
  <summary>Single-File Components (SFC)</summary>

### Single-File Components (SFC)

**Definition:** A `.vue` file combines `<template>`, `<script>`, and optional `<style scoped>` in one module. Vite (via `@vitejs/plugin-vue`) compiles it to JavaScript the browser runs.

**How it works:**

1. `<script setup>` runs once per instance — sets up reactive state and registers bindings for template
2. `<template>` compiles to a `render` function
3. `<style scoped>` adds data attributes so CSS targets only this component

**When to use:** Default for all Vue 3 apps. Colocates markup, logic, and styles.

**Examples:**

```vue
<script setup lang="ts">
import { ref } from "vue";
const title = ref("Hello");
</script>

<template>
  <h1>{{ title }}</h1>
</template>

<style scoped>
h1 {
  color: var(--brand);
}
</style>
```

**Pluses and minuses:**

| +                            | −                               |
| ---------------------------- | ------------------------------- |
| IDE support (Volar), HMR     | Requires build step             |
| Scoped CSS without CSS-in-JS | Template is not plain HTML file |

**Sources:**

[vuejs.org/guide/scaling-up/sfc.html](https://vuejs.org/guide/scaling-up/sfc.html)

Template syntax

</details>

<details>
  <summary>Template syntax</summary>

### Template syntax

**Definition:** Vue templates extend HTML with **interpolations**, **directives** (`v-*`), and **event** / **binding** shorthand.

**How it works:**

| Syntax                  | Meaning                           |
| ----------------------- | --------------------------------- |
| `{{ expr }}`            | Text interpolation (HTML-escaped) |
| `v-bind:attr` / `:attr` | Bind attribute to expression      |
| `v-on:event` / `@event` | DOM event listener                |
| `v-if` / `v-else`       | Conditional render                |
| `v-for`                 | List render (`:key` required)     |
| `v-model`               | Two-way binding on form inputs    |
| `v-slot` / `#`          | Named / scoped slots              |

**When to use:** Default UI layer in SFCs. Use `render()` only for highly dynamic programmatic trees.

**Examples:**

```vue
<input :placeholder="msg" @input="onInput" />
<p v-if="show">{{ user.name }}</p>
<ul><li v-for="n in nums" :key="n">{{ n }}</li></ul>
```

**Sources:**

[vuejs.org/guide/essentials/template-syntax.html](https://vuejs.org/guide/essentials/template-syntax.html)

Components

</details>

<details>
  <summary>Components</summary>

### Components

**Definition:** Components are reusable Vue instances with a name, props interface, events, and slots. They compose into a tree; parent passes data down via props, child signals up via emits.

**How it works:**

1. Register globally (`app.component`) or locally (`import Child from './Child.vue'`)
2. In `<script setup>`, imported components are auto-registered in template
3. PascalCase in template (`<MyButton />`) recommended; kebab-case also works

**Examples:**

```vue
<script setup>
import UserCard from "./UserCard.vue";
</script>

<template>
  <UserCard :user="currentUser" @select="onSelect" />
</template>
```

**See also:** [Props](#props) · [Slots](#slots)

**Sources:**

[vuejs.org/guide/essentials/component-basics.html](https://vuejs.org/guide/essentials/component-basics.html)

Props

</details>

<details>
  <summary>Props</summary>

### Props

**Definition:** **Props** are read-only inputs from the parent. The child must not mutate props directly — emit an event and let the parent update.

**How it works:**

- Declared with `defineProps()` in `<script setup>` (compile-time macro)
- Type-based: `defineProps<{ id: number; title?: string }>()`
- Runtime: `defineProps({ id: Number, title: String })`
- Vue 3.5+: destructured props from `defineProps` are reactive (compiler rewrites to `props.foo`)

**When to use:** All parent → child data flow. Default values via destructuring (3.5+) or `withDefaults`.

**Examples:**

```vue
<script setup lang="ts">
const { count = 0 } = defineProps<{ count?: number }>();
</script>
```

**Pluses and minuses:**

| +                              | −                                                     |
| ------------------------------ | ----------------------------------------------------- |
| One-way data flow, predictable | Prop drilling in deep trees → provide/inject or Pinia |
| TS inference with defineProps  | Many boolean props → consider slots                   |

**See also:** [State vs props](#state-vs-props) · [defineProps](./4_vue_composition.md#defineprops)

**Sources:**

[vuejs.org/guide/components/props.html](https://vuejs.org/guide/components/props.html)

inheritAttrs & fallthrough attributes

</details>

<details>
  <summary>inheritAttrs & fallthrough attributes</summary>

### inheritAttrs & fallthrough attributes

**Definition:** Attributes passed to a component that are **not** declared as props or emits **fall through** to the root element in the template. `inheritAttrs: false` (Options API) or `defineOptions({ inheritAttrs: false })` stops automatic fallthrough so you can forward attrs manually (wrapper components).

**How it works:**

- Parent: `<BaseInput class="wide" data-test="email" label="Email" />`
- If `label` is a prop and `class`/`data-test` are not, `class` and `data-test` apply to the child's root element
- Wrapper pattern: bind `$attrs` to inner input — `<input v-bind="$attrs" />`
- In `<script setup>`, `useAttrs()` returns reactive fallthrough attrs when `inheritAttrs: false`

**When to use:** Input/button wrappers, design-system primitives that pass native HTML attrs to the inner element. Disable inheritAttrs when the root is not the element that should receive attrs.

**Pluses and minuses:**

| +                                                                 | −                                                |
| ----------------------------------------------------------------- | ------------------------------------------------ |
| Ergonomic wrapper components without re-declaring every HTML attr | Easy to forget which attrs land on root vs inner |
| `$attrs` excludes declared props and emits                        | `class`/`style` merge on root by default         |

**Examples:**

```vue
<script setup lang="ts">
defineOptions({ inheritAttrs: false });
const attrs = useAttrs();
</script>

<template>
  <label class="field">
    <span>{{ attrs.label }}</span>
    <input v-bind="attrs" />
  </label>
</template>
```

**See also:** [Props](#props) · [Components](#components)

**Sources:**

[vuejs.org/guide/components/attrs.html](https://vuejs.org/guide/components/attrs.html)

Slots

</details>

<details>
  <summary>Slots</summary>

### Slots

**Definition:** **Slots** let parents inject markup into a child's template — composition without prop explosion.

**How it works:**

- **Default slot** — `<slot />` fallback content
- **Named slots** — `<slot name="header" />` → `<template #header>`
- **Scoped slots** — child passes data to slot: `<slot :item="row" />` → `#default="{ item }"`

**When to use:** Layout components (`<Card>`, `<Modal>`), tables, list renderers.

**Examples:**

```vue
<!-- Child -->
<slot name="title" />
<slot :items="list" />

<!-- Parent -->
<Child>
  <template #title>Settings</template>
  <template #default="{ items }">
    <li v-for="i in items" :key="i.id">{{ i.name }}</li>
  </template>
</Child>
```

**Sources:**

[vuejs.org/guide/components/slots.html](https://vuejs.org/guide/components/slots.html)

Directives

</details>

<details>
  <summary>Directives</summary>

### Directives

**Definition:** Directives are `v-*` attributes that apply reactive side effects to DOM elements. Built-ins cover most cases; **custom directives** register app-wide or locally for low-level DOM behavior.

**How it works:**

| Directive   | Purpose                                 |
| ----------- | --------------------------------------- |
| `v-bind`    | Bind attribute or object of attrs       |
| `v-on`      | Attach event listeners                  |
| `v-model`   | Two-way form binding                    |
| `v-if/else` | Conditional mount/unmount               |
| `v-show`    | Toggle `display` (element stays in DOM) |
| `v-for`     | Render list                             |
| `v-memo`    | Skip update if deps unchanged (perf)    |
| `v-once`    | Render once, skip future updates        |

**Custom directive hooks** (on the bound element):

| Hook            | When                    |
| --------------- | ----------------------- |
| `created`       | Before attrs applied    |
| `beforeMount`   | Before element inserted |
| `mounted`       | Element inserted        |
| `beforeUpdate`  | Before parent re-render |
| `updated`       | After parent re-render  |
| `beforeUnmount` | Before unmount          |
| `unmounted`     | Element removed         |

Register: `app.directive('focus', { mounted(el) { el.focus() } })` or `const vFocus = { mounted(el) { el.focus() } }` in `<script setup>`.

**When to use:** Built-ins for 95% of UI. Custom directives for focus traps, tooltips, or third-party DOM plugins — prefer composables when logic is not DOM-specific.

**Sources:**

[vuejs.org/api/built-in-directives.html](https://vuejs.org/api/built-in-directives.html) · [vuejs.org/guide/reusability/custom-directives.html](https://vuejs.org/guide/reusability/custom-directives.html)

Conditional rendering & lists

</details>

<details>
  <summary>Conditional rendering & lists</summary>

### Conditional rendering & lists

**Definition:** `v-if` / `v-else-if` / `v-else` conditionally **create/destroy** DOM. `v-for` renders collections; must include `:key`.

**How it works:**

- `v-if` on `<template>` wraps multiple elements without extra DOM
- `v-for` can destructure: `v-for="{ id, name } in users"`
- Never combine `v-if` and `v-for` on the same element — use computed filter or wrapper `<template>`

**Examples:**

```vue
<template v-if="loggedIn">
  <Dashboard />
</template>
<template v-else>
  <LoginForm />
</template>

<li v-for="user in activeUsers" :key="user.id">{{ user.name }}</li>
```

**Sources:**

[vuejs.org/guide/essentials/conditional.html](https://vuejs.org/guide/essentials/conditional.html)

Class & style binding

</details>

<details>
  <summary>Class & style binding</summary>

### Class & style binding

**Definition:** `:class` and `:style` accept objects, arrays, or strings for dynamic styling. Vue merges with static `class` attributes.

**Examples:**

```vue
<div :class="{ active: isActive, 'text-danger': hasError }" />
<div :class="[baseClass, isActive && 'active']" />
<p :style="{ color: activeColor, fontSize: size + 'px' }" />
```

**When to use:** Prefer component props + scoped CSS over inline styles for maintainability.

**Sources:**

[vuejs.org/guide/essentials/class-and-style.html](https://vuejs.org/guide/essentials/class-and-style.html)

</details>

---

## 4. State & interactivity

ref

<details>
  <summary>ref</summary>

### ref

**Definition:** `ref()` wraps any value in a reactive object with a `.value` property. Primitives and object references both work.

**How it works:**

- Reading/writing `.value` in script tracks and triggers updates
- Templates **auto-unwrap** refs — use `count` not `count.value`
- `ref` on a DOM element (or `useTemplateRef`) holds the element after mount

**When to use:** Default for scalars, flags, async results, and when you need to replace the whole value (`user.value = newUser`).

**Examples:**

```ts
import { ref } from "vue";

const count = ref(0);
count.value++;

const el = ref<HTMLInputElement | null>(null);
```

**Pluses and minuses:**

| +                              | −                                      |
| ------------------------------ | -------------------------------------- |
| Works with primitives          | `.value` ceremony in script            |
| Reassign entire object cleanly | Easy to forget `.value` in TS handlers |

**See also:** [reactive](#reactive) · [ref vs reactive](./4_vue_interview.md#ref-vs-reactive)

**Sources:**

[vuejs.org/api/reactivity-core.html#ref](https://vuejs.org/api/reactivity-core.html#ref)

reactive

</details>

<details>
  <summary>reactive</summary>

### reactive

**Definition:** `reactive()` returns a **Proxy** of an object (or array) where nested property access is tracked. Cannot wrap primitives.

**How it works:**

- Mutate in place: `state.count++` — no `.value`
- Destructuring breaks reactivity — use `toRefs()` or keep the reactive object intact
- `readonly()` / `shallowReactive()` variants for immutable or shallow tracking

**When to use:** Form state objects, grouped settings, records you update field-by-field.

**Examples:**

```ts
import { reactive } from "vue";

const state = reactive({ count: 0, nested: { ok: true } });
state.count++;
```

**Pluses and minuses:**

| +                          | −                                                    |
| -------------------------- | ---------------------------------------------------- |
| Natural object mutation    | Destructuring loses reactivity                       |
| Deep reactivity by default | Replacing root needs `Object.assign` or new reactive |

**Sources:**

[vuejs.org/api/reactivity-core.html#reactive](https://vuejs.org/api/reactivity-core.html#reactive)

computed

</details>

<details>
  <summary>computed</summary>

### computed

**Definition:** `computed()` creates a **cached derived ref** that re-evaluates only when dependencies change.

**How it works:**

- Getter runs lazily on read; result cached until deps change
- Computed refs are **readonly** by default; writable computed uses `{ get, set }`
- Used in templates without `.value`

**When to use:** Derived lists (filtered todos), formatted labels, expensive calculations — not for side effects (use `watch`).

**Examples:**

```ts
import { ref, computed } from "vue";

const items = ref([{ done: false, text: "a" }]);
const remaining = computed(() => items.value.filter((i) => !i.done).length);
```

**Sources:**

[vuejs.org/guide/essentials/computed.html](https://vuejs.org/guide/essentials/computed.html)

watch & watchEffect

</details>

<details>
  <summary>watch & watchEffect</summary>

### watch & watchEffect

**Definition:** `watch` runs a callback when a specific reactive source changes. `watchEffect` auto-tracks dependencies in its synchronous body and re-runs immediately.

**How it works:**

| API           | Tracks             | Runs on mount           | Use for                             |
| ------------- | ------------------ | ----------------------- | ----------------------------------- |
| `watchEffect` | Implicit (reads)   | Yes                     | Sync to external systems            |
| `watch`       | Explicit source(s) | No (unless `immediate`) | Compare old/new, async side effects |

- `watch(source, callback, { flush: 'post' })` runs after DOM update
- `watchEffect` return value is `stop()` handle

**When to use:** API calls on filter change (`watch`), document title sync (`watchEffect`). Prefer `computed` for derived UI state.

**Examples:**

```ts
watch(() => route.params.id, fetchUser);
watchEffect(() => {
  document.title = `${count.value} items`;
});
```

**Sources:**

[vuejs.org/guide/essentials/watchers.html](https://vuejs.org/guide/essentials/watchers.html)

v-model

</details>

<details>
  <summary>v-model</summary>

### v-model

**Definition:** `v-model` is syntactic sugar for a prop + event pair: `:modelValue` + `@update:modelValue`. Supports multiple named models (`v-model:title`).

**How it works:**

- On native inputs, `v-model` binds `value` / `checked` and listens to `input` / `change`
- On components: child exposes `modelValue` prop and emits `update:modelValue`
- Vue 3.4+: `defineModel()` macro simplifies child implementation

**Examples:**

```vue
<!-- Parent -->
<CustomInput v-model="search" />
<CustomInput v-model:title="pageTitle" />

<!-- Child (manual) -->
<script setup>
const props = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue"]);
</script>
<template>
  <input
    :value="modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  />
</template>
```

**See also:** [defineModel](./4_vue_composition.md#definemodel)

**Sources:**

[vuejs.org/guide/components/v-model.html](https://vuejs.org/guide/components/v-model.html)

State vs props

</details>

<details>
  <summary>State vs props</summary>

### State vs props

**Definition:** **Props** flow down from parent (read-only in child). **State** (`ref`/`reactive`) is owned by the component and triggers re-render on change.

**How it works:**

- Share sibling state → lift to parent or use Pinia
- Props update when parent re-renders with new values
- Child emits events; parent updates its state → new props flow down

**When to use:** Props for configuration and data; local state for UI that parent does not need (open/closed, draft text).

**See also:** [provide / inject](#provide-inject) · [Pinia](./4_vue_ecosystem.md#pinia)

**Sources:**

[vuejs.org/guide/components/props.html](https://vuejs.org/guide/components/props.html)

provide / inject

</details>

<details>
  <summary>provide / inject</summary>

### provide / inject

**Definition:** `provide(key, value)` registers a value on the ancestor chain; `inject(key, default)` reads it in descendants — dependency injection without prop drilling.

**How it works:**

- Keys can be `Symbol` or string
- Provide reactive refs/computed so descendants stay updated
- Useful for theme, locale, form context — not a replacement for global app state

**Examples:**

```ts
// Parent
import { provide, ref } from "vue";
const theme = ref("dark");
provide("theme", theme);

// Descendant
import { inject } from "vue";
const theme = inject<Ref<string>>("theme", ref("light"));
```

**When to use:** Component library internals, deeply nested trees. Prefer Pinia for cross-route shared state.

**Sources:**

[vuejs.org/guide/components/provide-inject.html](https://vuejs.org/guide/components/provide-inject.html)

</details>
