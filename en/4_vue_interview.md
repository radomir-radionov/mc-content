# Vue — Interview

Interview Q&A and coding tasks.

[← Back to hub](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Ecosystem](./4_vue_ecosystem.md) · [Vue — Interview](./4_vue_interview.md)

### Sections

- [Questions & Answers](#questions-answers)
- [Coding tasks](#coding-tasks)
- [Sources](#sources)

---

## Questions & Answers

What is Vue?

<details>
  <summary>What is Vue?</summary>

### What is Vue?

**Answer:** Vue is a progressive JavaScript **framework** for building UIs with a component model, template or render-function views, and a proxy-based reactivity system. Vue 3 uses the Composition API (`ref`, `computed`, `watch`) and compiles SFCs at build time. Official companions: Vue Router, Pinia, Vite.

**See also:** [Vue overview](./4_vue_core.md#vue-overview)

Vue library or framework?

</details>

<details>
  <summary>Vue library or framework?</summary>

### Vue library or framework?

**Answer:** Vue **core** is a progressive UI library you mount with `createApp`. With Vue Router + Pinia + Vite it behaves like a framework stack. **Nuxt** is a full-stack framework on top of Vue. Interview: "Vue is progressive — library at the core, ecosystem completes the stack."

**See also:** [Library vs framework](./4_vue_core.md#library-vs-framework) · [Nuxt — Core](./4_nuxt_core.md)

State vs props?

</details>

<details>
  <summary>State vs props?</summary>

### State vs props?

**Answer:** **Props** are read-only inputs from parent. **State** (`ref`/`reactive`) is owned by the component. Child emits events; parent updates state → new props flow down. Siblings share via lifted state, provide/inject, or Pinia.

**See also:** [State vs props](./4_vue_core.md#state-vs-props)

ref vs reactive?

</details>

<details>
  <summary>ref vs reactive?</summary>

### ref vs reactive?

**Answer:** `ref` wraps any value — access `.value` in script, auto-unwrap in template. `reactive` wraps objects only — mutate properties directly. Destructuring `reactive` loses tracking — use `toRefs`. Default to `ref` for primitives and when replacing whole values.

**See also:** [ref](./4_vue_core.md#ref) · [reactive](./4_vue_core.md#reactive)

computed vs watch?

</details>

<details>
  <summary>computed vs watch?</summary>

### computed vs watch?

**Answer:** `computed` = **cached derived value** for the template (no side effects). `watch` / `watchEffect` = **side effects** when sources change (API calls, logging). If you're syncing UI from state, use `computed`; if reacting to changes with async work, use `watch`.

**See also:** [computed](./4_vue_core.md#computed) · [watch & watchEffect](./4_vue_core.md#watch-watcheffect)

What is script setup?

</details>

<details>
  <summary>What is script setup?</summary>

### What is script setup?

**Answer:** `<script setup>` is compile-time sugar: top-level bindings are auto-exposed to template. Macros `defineProps`, `defineEmits`, `defineModel`, `defineExpose` need no import. Runs once per instance before render — equivalent to `setup()` return.

**See also:** [script setup](./4_vue_composition.md#script-setup)

What is defineModel?

</details>

<details>
  <summary>What is defineModel?</summary>

### What is defineModel?

**Answer:** Macro (3.4+) returning a ref for `v-model`. Compiler expands to `modelValue` prop + `update:modelValue` emit. Named: `defineModel('title')` for `v-model:title`. Replaces manual prop + emit boilerplate.

**See also:** [defineModel](./4_vue_composition.md#definemodel)

Composition API vs Options API?

</details>

<details>
  <summary>Composition API vs Options API?</summary>

### Composition API vs Options API?

**Answer:** **Options API** splits logic across `data`, `methods`, `computed`, `watch`. **Composition API** colocates feature logic in `setup` / `<script setup>` and extracts composables. Vue 3 recommends Composition for new code; Options remains for maintenance and Vue 2 familiarity.

**See also:** [Overview](./4_vue_composition.md#overview) · [Options API](./4_vue_ecosystem.md#options-api)

What is a composable?

</details>

<details>
  <summary>What is a composable?</summary>

### What is a composable?

**Answer:** A `use*` function encapsulating stateful logic with Composition API. Each component call gets its own state. Like React custom hooks — extract fetch, forms, media queries. Pass getters for reactive props: `useUser(() => props.id)`.

**See also:** [Custom composables](./4_vue_composition.md#custom-composables)

v-if vs v-show?

</details>

<details>
  <summary>v-if vs v-show?</summary>

### v-if vs v-show?

**Answer:** `v-if` **mounts/unmounts** DOM (lazy, higher toggle cost). `v-show` toggles `display: none` (element stays in DOM). Use `v-if` for rare/expensive subtrees; `v-show` for frequent toggles (tabs).

**See also:** [Directives](./4_vue_core.md#directives)

Why list keys? Index as key?

</details>

<details>
  <summary>Why list keys? Index as key?</summary>

### Why list keys? Index as key?

**Answer:** `:key` gives VNodes stable identity across renders. Index is OK only for **static** lists. Reorder/filter + index → wrong DOM reuse and stale input state. Use unique ids (`item.id`).

**See also:** [Keys](./4_vue_core.md#keys)

How does Vue reactivity work?

</details>

<details>
  <summary>How does Vue reactivity work?</summary>

### How does Vue reactivity work?

**Answer:** Vue 3 uses `Proxy` to **track** property reads (`get`) and **trigger** subscribers on writes (`set`). Render effects re-run when accessed reactive data changes. `ref` wraps `.value`; `reactive` deep-tracks objects. Scheduler batches updates in a microtask.

**See also:** [Reactivity system](./4_vue_core.md#reactivity-system)

Pinia vs Vuex?

</details>

<details>
  <summary>Pinia vs Vuex?</summary>

### Pinia vs Vuex?

**Answer:** Pinia is official for Vue 3 — no mutations, simpler API, better TS, modular stores. Vuex 4 is maintenance-only. New projects: Pinia. Migrate stores one at a time.

**See also:** [Pinia](./4_vue_ecosystem.md#pinia) · [Vuex (legacy)](./4_vue_ecosystem.md#vuex-legacy)

Vue Router navigation guards?

</details>

<details>
  <summary>Vue Router navigation guards?</summary>

### Vue Router navigation guards?

**Answer:** `beforeEach` (global), `beforeEnter` (per route), `beforeRouteEnter` (in-component) can redirect or cancel navigation. Use for auth: if !token → `next('/login')`. Async guards can await fetch before allowing route.

**See also:** [Vue Router](./4_vue_ecosystem.md#vue-router)

SPA vs SSR in Vue?

</details>

<details>
  <summary>SPA vs SSR in Vue?</summary>

### SPA vs SSR in Vue?

**Answer:** SPA: empty shell + client `createApp().mount()`. SSR: server `renderToString(createSSRApp)` → HTML; client hydrates with `createSSRApp().mount()`. SSR needs server/client markup match. Nuxt automates SSR — see Nuxt docs, not core Vue interview depth.

**See also:** [SPA vs SSR](./4_vue_ssr.md#spa-vs-ssr) · [Hydration](./4_vue_ssr.md#hydration)

What is hydration mismatch?

</details>

<details>
  <summary>What is hydration mismatch?</summary>

### What is hydration mismatch?

**Answer:** Server HTML ≠ client first render → Vue warns and may discard server DOM. Causes: `Date.now()`, `Math.random()`, browser-only APIs in setup, invalid HTML nesting. Fix: guard with `onMounted`, `ClientOnly` (Nuxt), or `data-allow-mismatch` for intentional diffs.

**See also:** [Hydration](./4_vue_ssr.md#hydration)

Vue 2 vs Vue 3 main differences?

</details>

<details>
  <summary>Vue 2 vs Vue 3 main differences?</summary>

### Vue 2 vs Vue 3 main differences?

**Answer:** Proxy reactivity (vs `defineProperty`), Composition API, `createApp` (vs `new Vue`), fragments, Teleport, removed filters, new `v-model` convention, Vue Router 4, Pinia over Vuex, Vite over Vue CLI. Vue 2 EOL Dec 2023.

**See also:** [Vue 3 vs Vue 2](./4_vue_core.md#vue-3-vs-vue-2) · [Vue 2 → 3 migration](./4_vue_ecosystem.md#vue-2-3-migration)

Teleport — what and why?

</details>

<details>
  <summary>Teleport — what and why?</summary>

### Teleport — what and why?

**Answer:** Renders children in another DOM node (e.g. `body`) while keeping same component tree — events and provide/inject work. Fixes modals trapped in `overflow: hidden` or z-index contexts.

**See also:** [Teleport](./4_vue_composition.md#teleport)

provide/inject vs Pinia?

</details>

<details>
  <summary>provide/inject vs Pinia?</summary>

### provide/inject vs Pinia?

**Answer:** provide/inject = ancestor → descendant DI for theme, form context, library internals. Pinia = global app state across routes. Don't use provide for cart/auth that every page needs — use Pinia.

**See also:** [provide / inject](./4_vue_core.md#provide-inject) · [State management patterns](./4_vue_ecosystem.md#state-management-patterns)

How to optimize Vue performance?

</details>

<details>
  <summary>How to optimize Vue performance?</summary>

### How to optimize Vue performance?

**Answer:** Profile first. Lazy routes/components, `v-memo` on heavy lists, `shallowRef` for large immutable data, virtualize long lists, SSR/SSG for LCP. Compiler already hoists static nodes — avoid premature `computed` everywhere.

**See also:** [Performance](./4_vue_ecosystem.md#10-performance)

</details>

---

## Coding tasks

Task 1 — Lost reactivity on destructure

<details>
  <summary>Task 1 — Lost reactivity on destructure</summary>

### Task 1 — Lost reactivity on destructure

**Problem:** Counter stops updating in template after destructuring.

```vue
<script setup>
import { reactive } from "vue";
const state = reactive({ count: 0 });
const { count } = state;
</script>
<template>
  <button @click="count++">{{ count }}</button>
</template>
```

**Answer:** Destructuring extracts a plain number — not tracked. Fix:

```vue
<script setup>
import { reactive } from "vue";
const state = reactive({ count: 0 });
</script>
<template>
  <button @click="state.count++">{{ state.count }}</button>
</template>
```

Or `const { count } = toRefs(state)` and use `count.value` in script.

**See also:** [reactive](./4_vue_core.md#reactive) · [Reactivity utilities](./4_vue_composition.md#reactivity-utilities)

</details>

Task 2 — v-for without key

<details>
  <summary>Task 2 — v-for without key</summary>

### Task 2 — v-for without key

**Problem:** Todo list loses edit state after reorder.

```vue
<li v-for="(todo, index) in todos" :key="index">
  <input v-model="todo.text" />
</li>
```

**Answer:** Index keys break when list reorders — Vue reuses wrong DOM nodes. Use stable id:

```vue
<li v-for="todo in todos" :key="todo.id">
  <input v-model="todo.text" />
</li>
```

**See also:** [Keys](./4_vue_core.md#keys)

</details>

Task 3 — watch without getter on prop

<details>
  <summary>Task 3 — watch without getter on prop</summary>

### Task 3 — watch without getter on prop

**Problem:** Watcher never fires when prop changes (Vue 3.5+).

```vue
<script setup>
const { userId } = defineProps<{ userId: string }>()
watch(userId, fetchUser) // compile warning / wrong
</script>
```

**Answer:** Pass a getter so `watch` tracks reactive source:

```ts
watch(() => userId, fetchUser, { immediate: true });
```

Or watch `() => props.userId` if using non-destructured props.

**See also:** [Reactive props destructure](./4_vue_ssr.md#reactive-props-destructure)

</details>

Task 4 — Child v-model implementation

<details>
  <summary>Task 4 — Child v-model implementation</summary>

### Task 4 — Child v-model implementation

**Problem:** Implement a reusable `SearchInput` with `v-model` using `defineModel`.

**Answer:**

```vue
<script setup lang="ts">
const model = defineModel<string>({ default: "" });
</script>

<template>
  <input
    :value="model"
    @input="model = ($event.target as HTMLInputElement).value"
    type="search"
    placeholder="Search…"
  />
</template>
```

Parent: `<SearchInput v-model="query" />`

**See also:** [defineModel](./4_vue_composition.md#definemodel) · [v-model](./4_vue_core.md#v-model)

</details>

Task 5 — Fetch on route param change

<details>
  <summary>Task 5 — Fetch on route param change</summary>

### Task 5 — Fetch on route param change

**Problem:** Fetch user when `/users/:id` changes without remounting parent.

**Answer:**

```vue
<script setup>
import { watch } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const user = ref(null)

async function load(id: string) {
  user.value = await fetch(`/api/users/${id}`).then(r => r.json())
}

watch(() => route.params.id, (id) => load(id as string), { immediate: true })
</script>
```

Alternative: `:key="route.params.id"` on child to force remount.

**See also:** [Vue Router](./4_vue_ecosystem.md#vue-router) · [watch](./4_vue_core.md#watch-watcheffect)

</details>

## Sources

<details>
  <summary>Sources</summary>

**Definition:** External interview question lists, flashcards, and practice materials for Vue.js.

**Sources:**

- [ilavista.by — Vue.js interview questions](https://ilavista.by/blog/voprosy-na-sobesedovanii-vuejs)
- [GitHub — 300 Vue.js interview questions (RU)](https://github.com/magisters-cc/vuejs-interview-questions-russian)
- [itanddigital.ru — 50 Vue.js interview questions](https://itanddigital.ru/bloghrconsulting/tpost/o9gce6b1b1-50-osnovnih-voprosov-i-otvetov-na-sobese)
- [itflashcards.com — Vue.js flashcards](https://www.itflashcards.com/vuejs)
- [zerocoder.ru — Practical Vue.js interview questions](https://ya.zerocoder.ru/pgt-prakticheskie-voprosy-dlya-sobesedovaniya-po-vue-js-s-otvetami/)
- [YouTube — Vue.js interview (video)](https://www.youtube.com/watch?v=umHjpWydn8I&t=1s)
- [Medium — Practical Vue.js interview questions](https://medium.com/front-end-weekly/practical-vue-js-interview-questions-2252b7050b9d)

</details>
