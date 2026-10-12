# Vue — Composition

Composition API reference and escape hatches.

[← Back to hub](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Ecosystem](./4_vue_ecosystem.md) · [Vue — Interview](./4_vue_interview.md)

### Sections

- [5. Composition API reference](#5-composition-api-reference)
- [6. Composables & escape hatches](#6-composables-escape-hatches)

---

## 5. Composition API reference

Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** The **Composition API** groups logic by feature in `setup()` or `<script setup>` using `ref`, `reactive`, `computed`, lifecycle hooks, and composables — instead of splitting across `data`, `methods`, `computed`, and `watch` options.

**How it works:**

1. `setup()` runs **before** mount, once per instance (or `<script setup>` is compiled to setup body)
2. Return object or use `<script setup>` — bindings auto-exposed to template
3. Lifecycle hooks register during setup (`onMounted`, etc.)
4. Composables extract reusable logic (`useMouse`, `useFetch`)

**When to use:** Default for Vue 3 new code. Options API remains for maintenance and teams preferring object syntax.

**Pluses and minuses:**

| +                              | −                                         |
| ------------------------------ | ----------------------------------------- |
| Logic colocated by feature     | `setup` indentation in large components   |
| Easy extraction to composables | Two APIs to learn (Options + Composition) |
| Better TypeScript inference    | —                                         |

**See also:** [script setup](#script-setup) · [Custom composables](#custom-composables)

**Sources:**

[vuejs.org/guide/extras/composition-api-faq.html](https://vuejs.org/guide/extras/composition-api-faq.html)

TypeScript with Vue

</details>

<details>
  <summary>TypeScript with Vue</summary>

### TypeScript with Vue

**Definition:** Vue 3 SFCs support TypeScript in `<script setup lang="ts">` with compile-time macros (`defineProps`, `defineEmits`, `defineModel`) and tooling via **Volar** (IDE) + **`vue-tsc`** (CLI typecheck).

**How it works:**

- `defineProps<{ id: number }>()` — type-only props (erased at runtime)
- `defineProps<Props>()` + `withDefaults` for default values (pre-3.5)
- Generic components: `<script setup lang="ts" generic="T">` (3.3+)
- `ComponentPublicInstance` / `InstanceType<typeof MyComp>` for ref typing
- Run `vue-tsc --noEmit` in CI alongside `tsc`; Vite does not typecheck templates alone

**When to use:** All production Vue apps. Pair with [TypeScript notes](./5_ts_objects.md) for shared domain types.

**Examples:**

```vue
<script setup lang="ts" generic="T extends { id: string }">
defineProps<{
  items: T[];
  selectedId?: string;
}>();
</script>
```

**See also:** [defineProps](#defineprops) · [Typing objects in TS](./5_ts_objects.md)

**Sources:** [vuejs.org/guide/typescript/overview.html](https://vuejs.org/guide/typescript/overview.html)

script setup

</details>

<details>
  <summary>script setup</summary>

### script setup

**Definition:** `<script setup>` is compile-time sugar over `setup()` — top-level bindings are automatically exposed to the template. Imports and macros (`defineProps`, `defineEmits`) work at top level.

**How it works:**

- Runs once per instance, before render
- No `return` needed — `const x = ref(0)` is available in template as `x`
- `defineProps` / `defineEmits` / `defineExpose` / `defineModel` are **compiler macros** (no import)
- `defineOptions({ name: 'MyComp' })` sets component name (3.3+)

**When to use:** All new SFCs unless you need Options API for legacy reasons.

**Examples:**

```vue
<script setup lang="ts">
import { ref } from "vue";
import Child from "./Child.vue";

const count = ref(0);
function increment() {
  count.value++;
}
</script>

<template>
  <Child :count="count" @update="increment" />
</template>
```

**Sources:**

[vuejs.org/api/sfc-script-setup.html](https://vuejs.org/api/sfc-script-setup.html)

defineProps

</details>

<details>
  <summary>defineProps</summary>

### defineProps

**Definition:** Compile-time macro to declare component props with optional TypeScript types or runtime validators.

**How it works:**

- Type-based: `defineProps<{ id: number; label?: string }>()`
- Defaults (3.5+): `const { label = 'Untitled' } = defineProps<{ label?: string }>()`
- Runtime: `defineProps({ id: { type: Number, required: true } })`
- `withDefaults(defineProps<Props>(), { label: 'Untitled' })` for complex defaults (pre-3.5 pattern)

**When to use:** Every component that accepts parent input. Prefer type-based props with TS.

**Examples:**

```vue
<script setup lang="ts">
interface Props {
  items: string[];
  max?: number;
}
const { max = 10 } = defineProps<Props>();
</script>
```

**See also:** [Props](./4_vue_core.md#props)

**Sources:**

[vuejs.org/guide/typescript/composition-api.html#syntax-limitations](https://vuejs.org/guide/typescript/composition-api.html)

defineEmits

</details>

<details>
  <summary>defineEmits</summary>

### defineEmits

**Definition:** Declares events the component can emit to its parent. Type-safe with tuple syntax.

**Examples:**

```vue
<script setup lang="ts">
const emit = defineEmits<{
  save: [payload: { id: number }];
  cancel: [];
}>();

function onSave() {
  emit("save", { id: 1 });
}
</script>
```

**When to use:** Child → parent communication. Parent listens with `@save="handler"`.

**Sources:**

[vuejs.org/guide/components/events.html](https://vuejs.org/guide/components/events.html)

defineModel

</details>

<details>
  <summary>defineModel</summary>

### defineModel

**Definition:** Macro (Vue 3.4+) that creates a two-way binding ref for `v-model`. Compiler expands to `modelValue` prop + `update:modelValue` emit.

**How it works:**

- `const model = defineModel<string>()` — default `v-model`
- `const title = defineModel<string>('title')` — `v-model:title`
- Options: `required`, `default`, custom `get`/`set` for modifiers
- Returns a ref — mutate `model.value` to emit upstream

**Examples:**

```vue
<script setup lang="ts">
const model = defineModel<string>({ required: true });
const [value, modifiers] = defineModel<string>();
</script>

<template>
  <input v-model="model" />
</template>
```

**When to use:** Preferred over manual prop + emit for form controls and editable child state.

**Sources:**

[vuejs.org/api/sfc-script-setup.html#definemodel](https://vuejs.org/api/sfc-script-setup.html#definemodel)

defineExpose

</details>

<details>
  <summary>defineExpose</summary>

### defineExpose

**Definition:** Explicitly exposes properties/methods on the component instance for parent **template refs**. `<script setup>` components are closed by default.

**Examples:**

```vue
<script setup>
import { ref } from "vue";

const input = (ref < HTMLInputElement) | (null > null);
function focus() {
  input.value?.focus();
}

defineExpose({ focus });
</script>
```

```vue
<!-- Parent -->
<script setup>
import { useTemplateRef, onMounted } from "vue";
const child = useTemplateRef("child");
onMounted(() => child.value?.focus());
</script>
<template><Child ref="child" /></template>
```

**When to use:** Rare — prefer props/emits. OK for focus management, imperative chart APIs.

**Sources:**

[vuejs.org/api/sfc-script-setup.html#defineexpose](https://vuejs.org/api/sfc-script-setup.html#defineexpose)

Lifecycle hooks

</details>

<details>
  <summary>Lifecycle hooks</summary>

### Lifecycle hooks

**Definition:** Composition API lifecycle functions register callbacks during `setup`. They mirror Options API hooks.

**How it works:**

| Composition API   | Options API     | When                       |
| ----------------- | --------------- | -------------------------- |
| `onBeforeMount`   | `beforeMount`   | Before first DOM insert    |
| `onMounted`       | `mounted`       | After DOM mounted          |
| `onBeforeUpdate`  | `beforeUpdate`  | Before re-render           |
| `onUpdated`       | `updated`       | After DOM patched          |
| `onBeforeUnmount` | `beforeDestroy` | Before teardown            |
| `onUnmounted`     | `unmounted`     | After removed              |
| `onErrorCaptured` | `errorCaptured` | Child error propagation    |
| `onActivated`     | `activated`     | KeepAlive component shown  |
| `onDeactivated`   | `deactivated`   | KeepAlive component hidden |

**When to use:** `onMounted` for DOM access, subscriptions; `onUnmounted` for cleanup (timers, listeners).

**Examples:**

```ts
import { onMounted, onUnmounted } from "vue";

onMounted(() => window.addEventListener("resize", onResize));
onUnmounted(() => window.removeEventListener("resize", onResize));
```

**Sources:**

[vuejs.org/api/composition-api-lifecycle.html](https://vuejs.org/api/composition-api-lifecycle.html)

Reactivity utilities

</details>

<details>
  <summary>Reactivity utilities</summary>

### Reactivity utilities

**Definition:** Helper functions to convert, clone, or limit reactivity depth.

| API           | Purpose                                  |
| ------------- | ---------------------------------------- |
| `toRef`       | Reactive property → ref                  |
| `toRefs`      | Reactive object → refs per key           |
| `toValue`     | Normalize ref/getter/raw (3.3+)          |
| `unref`       | `val.value ?? val`                       |
| `isRef`       | Type guard                               |
| `shallowRef`  | `.value` tracked, deep not               |
| `triggerRef`  | Force update on shallowRef               |
| `readonly`    | Immutable proxy                          |
| `markRaw`     | Opt out of reactivity                    |
| `effectScope` | Group effects; dispose all on scope stop |

**When to use:** `toRefs` when destructuring `reactive` state; `shallowRef` for large immutable objects; `readonly` when passing state down without mutation.

effectScope

</details>

<details>
  <summary>effectScope</summary>

### effectScope

**Definition:** **`effectScope`** groups reactive effects (`watch`, `watchEffect`, `computed`) so they can be **stopped together** — useful in composables and plugins that register multiple watchers.

**How it works:**

```ts
import { effectScope, watchEffect } from "vue";

const scope = effectScope();
scope.run(() => {
  watchEffect(() => console.log(count.value));
});
// later:
scope.stop(); // disposes all effects in this scope
```

- `onScopeDispose(fn)` registers cleanup inside an active scope (like `onUnmounted` for effects)
- Vue uses scopes internally; composable authors use this for library-grade cleanup

**When to use:** Custom composables that create watchers outside component lifecycle; test teardown. Rare in app code — prefer `onUnmounted` in components.

**Sources:** [vuejs.org/api/reactivity-advanced.html#effectscope](https://vuejs.org/api/reactivity-advanced.html#effectscope)

</details>

---

## 6. Composables & escape hatches

Custom composables

<details>
  <summary>Custom composables</summary>

### Custom composables

**Definition:** A **composable** is a function using Composition API conventions (`use*` name) that encapsulates stateful logic reusable across components.

**How it works:**

1. Call Vue APIs inside (`ref`, `watch`, lifecycle)
2. Return refs/methods the caller destructures
3. Each component call gets **its own** state (like custom hooks in React)
4. Accept getters for reactive props: `useUser(() => props.id)`

**When to use:** Shared fetch logic, form validation, media queries, keyboard shortcuts.

**Examples:**

```ts
// composables/useCounter.ts
import { ref, computed } from "vue";

export function useCounter(initial = 0) {
  const count = ref(initial);
  const doubled = computed(() => count.value * 2);
  function inc() {
    count.value++;
  }
  return { count, doubled, inc };
}
```

```vue
<script setup>
const { count, inc } = useCounter(10);
</script>
```

**Pluses and minuses:**

| +                             | −                                   |
| ----------------------------- | ----------------------------------- |
| DRY logic without mixins/HOCs | Naming collisions if not namespaced |
| Testable in isolation         | Implicit deps if composable is huge |

**Sources:**

[vuejs.org/guide/reusability/composables.html](https://vuejs.org/guide/reusability/composables.html)

Template refs

</details>

<details>
  <summary>Template refs</summary>

### Template refs

**Definition:** Access DOM elements or child component instances after mount via `ref` attribute on template nodes.

**How it works:**

- **Vue 3.5+:** `useTemplateRef('id')` matches `ref="id"` (supports dynamic refs)
- **Before 3.5:** declare `const input = ref(null)` with matching `ref="input"` name
- Component refs expose only what `defineExpose` allows
- `v-for` + ref → array of elements after mount

**When to use:** Focus input, measure element, integrate non-Vue libraries (charts). Prefer declarative props when possible.

**Examples:**

```vue
<script setup>
import { useTemplateRef, onMounted } from "vue";

const input = useTemplateRef < HTMLInputElement > "search";
onMounted(() => input.value?.focus());
</script>

<template>
  <input ref="search" />
</template>
```

**See also:** [useTemplateRef (3.5+)](./4_vue_ssr.md#usetemplateref)

**Sources:**

[vuejs.org/guide/essentials/template-refs.html](https://vuejs.org/guide/essentials/template-refs.html)

Teleport

</details>

<details>
  <summary>Teleport</summary>

### Teleport

**Definition:** `<Teleport to="body">` renders children in a different DOM location while keeping them in the same component tree (events, provide/inject, reactivity).

**When to use:** Modals, toasts, tooltips that must escape `overflow: hidden` or z-index stacking contexts.

**Examples:**

```vue
<Teleport to="body">
  <div v-if="open" class="modal">...</div>
</Teleport>
```

**Pluses and minuses:**

| +                       | −                            |
| ----------------------- | ---------------------------- |
| Fixes stacking/overflow | SSR must match target exists |
| Same logical component  | `to` selector must be valid  |

**Sources:**

[vuejs.org/guide/built-ins/teleport.html](https://vuejs.org/guide/built-ins/teleport.html)

KeepAlive

</details>

<details>
  <summary>KeepAlive</summary>

### KeepAlive

**Definition:** Wrapper that **caches** inactive component instances instead of destroying them — preserves state when toggling tabs or routes.

**How it works:**

- `include` / `exclude` props filter by component `name`
- `max` limits cached instances (LRU)
- Cached components fire `onActivated` / `onDeactivated` instead of mount/unmount

**Examples:**

```vue
<KeepAlive :include="['TabA', 'TabB']" :max="5">
  <component :is="activeTab" />
</KeepAlive>
```

**When to use:** Tab panels, wizard steps where users return to prior step with form intact.

**Sources:**

[vuejs.org/guide/built-ins/keep-alive.html](https://vuejs.org/guide/built-ins/keep-alive.html)

Transition & TransitionGroup

</details>

<details>
  <summary>Transition & TransitionGroup</summary>

### Transition & TransitionGroup

**Definition:** Built-in wrappers that apply **enter/leave** CSS transitions or animations when children mount or unmount. `<Transition>` wraps a single element or component; `<TransitionGroup>` animates lists (`v-for`) with move transitions.

**How it works:**

- `<Transition name="fade">` toggles classes: `fade-enter-from`, `fade-enter-active`, `fade-leave-to`, etc.
- `mode="out-in"` — leave finishes before enter (common for route/tab switches)
- `<TransitionGroup tag="ul">` — each keyed child gets enter/leave/move classes
- Works with Vue Router `<router-view v-slot>` for page transitions; Nuxt uses the same primitives in layouts

**When to use:** Modals, dropdowns, tab panels, list add/remove. Prefer CSS transitions over JS when possible.

**Pluses and minuses:**

| +                                          | −                                      |
| ------------------------------------------ | -------------------------------------- |
| Declarative, no animation library required | Requires stable `:key` on list items   |
| Pairs with `<KeepAlive>` for tab UX        | Nested transitions need careful timing |

**Examples:**

```vue
<Transition name="fade">
  <p v-if="show">Hello</p>
</Transition>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
```

```vue
<TransitionGroup name="list" tag="ul">
  <li v-for="item in items" :key="item.id">{{ item.text }}</li>
</TransitionGroup>
```

**See also:** [KeepAlive](#keepalive) · [Nuxt routing transitions](./4_nuxt_routing.md#layouts)

**Sources:**

[vuejs.org/guide/built-ins/transition.html](https://vuejs.org/guide/built-ins/transition.html)

Async components

</details>

<details>
  <summary>Async components</summary>

### Async components

**Definition:** Components loaded on demand via dynamic `import()` — reduces initial bundle size.

**How it works:**

```ts
import { defineAsyncComponent } from "vue";

const AdminPanel = defineAsyncComponent(() => import("./AdminPanel.vue"));
```

- Pair with `<Suspense>` for loading fallback
- `defineAsyncComponent` accepts `loadingComponent`, `errorComponent`, `delay`, `timeout`

**When to use:** Route-level code splitting (Vue Router `() => import()`), heavy modals, admin sections.

**See also:** [Suspense](./4_vue_ssr.md#suspense) · [Vue Router lazy routes](./4_vue_ecosystem.md#vue-router)

**Sources:**

[vuejs.org/guide/components/async.html](https://vuejs.org/guide/components/async.html)

Suspense

</details>

<details>
  <summary>Suspense</summary>

### Suspense

**Definition:** Built-in component with `#default` and `#fallback` slots that shows fallback while async dependencies in the subtree resolve (async components, async `setup`, server data).

**Examples:**

```vue
<Suspense>
  <AsyncDashboard />
  <template #fallback>Loading dashboard…</template>
</Suspense>
```

**When to use:** Coordinated loading UI for async trees. SSR streaming uses Suspense boundaries.

**See also:** [SSR & hydration](./4_vue_ssr.md#8-ssr-hydration)

**Sources:**

[vuejs.org/guide/built-ins/suspense.html](https://vuejs.org/guide/built-ins/suspense.html)

You might not need a watcher

</details>

<details>
  <summary>You might not need a watcher</summary>

### You might not need a watcher

**Definition:** Many patterns that used `watch` in Options API are better solved with `computed`, event handlers, or `watchEffect` with clear intent.

**How it works:**

| Instead of watch…             | Prefer…                               |
| ----------------------------- | ------------------------------------- |
| Sync derived UI state         | `computed`                            |
| React to user click           | `@click` handler                      |
| Reset child when prop changes | `:key="id"` on child to remount       |
| Fetch on mount                | `onMounted` + function, or composable |

**When to use:** Reach for `watch` when you need **old vs new** comparison, debounced side effects, or async work tied to a specific source.

**Sources:**

[vuejs.org/guide/essentials/watchers.html](https://vuejs.org/guide/essentials/watchers.html)

getCurrentInstance (caution)

</details>

<details>
  <summary>getCurrentInstance (caution)</summary>

### getCurrentInstance (caution)

**Definition:** `getCurrentInstance()` returns the internal component instance during `setup` only. **Not** part of the public documented API for app code — use for advanced library authoring or debugging.

**When to use:** Almost never in application code. Prefer `inject`, props, and composables. If you need the component proxy, you're likely fighting the framework.

**Thoughts:** Interview red flag if overused. Legitimate uses: plugin authors, test utilities.

**Sources:**

[vuejs.org/api/composition-api-helpers.html#getcurrentinstance](https://vuejs.org/api/composition-api-helpers.html#getcurrentinstance)

</details>
