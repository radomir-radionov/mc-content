# Vue — Composition

Справочник Composition API и аварийные люки.

[← Вернуться в центр](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Экосистема](./4_vue_ecosystem.md) · [Vue — Интервью](./4_vue_interview.md)

### Sections

- [5. Composition API reference](#5-composition-api-reference)
- [6. Composables & escape hatches](#6-composables-escape-hatches)

---

## 5. Composition API reference

Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** **Composition API** группирует логику по фичам в `setup()` или `<script setup>` с помощью `ref`, `reactive`, `computed`, lifecycle hooks и composables — вместо разделения по `data`, `methods`, `computed` и `watch`.

**How it works:**

1. `setup()` выполняется **до** mount, один раз на инстанс (или `<script setup>` компилируется в тело setup)
2. Return object или `<script setup>` — bindings автоматически доступны в template
3. Lifecycle hooks регистрируются во время setup (`onMounted` и т.д.)
4. Composables выносят переиспользуемую логику (`useMouse`, `useFetch`)

**When to use:** По умолчанию для нового кода Vue 3. Options API остаётся для поддержки и команд, предпочитающих object syntax.

**Pluses and minuses:**

| +                                    | −                                      |
| ------------------------------------ | -------------------------------------- |
| Логика фичи рядом                    | Отступы `setup` в больших компонентах  |
| Легко выносить в composables         | Два API для изучения (Options + Composition) |
| Лучший вывод TypeScript              | —                                      |

**See also:** [script setup](#script-setup) · [Custom composables](#custom-composables)

**Sources:**

[vuejs.org/guide/extras/composition-api-faq.html](https://vuejs.org/guide/extras/composition-api-faq.html)

script setup

</details>

<details>
  <summary>TypeScript with Vue</summary>

### TypeScript with Vue

**Definition:** Vue 3 SFC поддерживают TypeScript в `<script setup lang="ts">` с compile-time макросами (`defineProps`, `defineEmits`, `defineModel`) и tooling через **Volar** (IDE) + **`vue-tsc`** (CLI typecheck).

**How it works:**

- `defineProps<{ id: number }>()` — type-only props (стираются в runtime)
- `defineProps<Props>()` + `withDefaults` для значений по умолчанию (до 3.5)
- Generic components: `<script setup lang="ts" generic="T">` (3.3+)
- `ComponentPublicInstance` / `InstanceType<typeof MyComp>` для типизации ref
- Запускайте `vue-tsc --noEmit` в CI вместе с `tsc`; Vite сам не typecheck-ит templates

**When to use:** Все production Vue-приложения. Сочетайте с [TypeScript notes](./5_ts_objects.md) для shared domain types.

**Examples:**

```vue
<script setup lang="ts" generic="T extends { id: string }">
defineProps<{
  items: T[]
  selectedId?: string
}>()
</script>
```

**See also:** [defineProps](#defineprops) · [Typing objects in TS](./5_ts_objects.md)

**Sources:** [vuejs.org/guide/typescript/overview.html](https://vuejs.org/guide/typescript/overview.html)

script setup

</details>

<details>
  <summary>script setup</summary>

### script setup

**Definition:** `<script setup>` — compile-time sugar над `setup()` — top-level bindings автоматически доступны в template. Imports и макросы (`defineProps`, `defineEmits`) работают на top level.

**How it works:**

- Выполняется один раз на инстанс, до рендера
- `return` не нужен — `const x = ref(0)` доступен в template как `x`
- `defineProps` / `defineEmits` / `defineExpose` / `defineModel` — **compiler macros** (без import)
- `defineOptions({ name: 'MyComp' })` задаёт имя компонента (3.3+)

**When to use:** Все новые SFC, если нет причин оставаться на Options API.

**Examples:**

```vue
<script setup lang="ts">
import { ref } from 'vue'
import Child from './Child.vue'

const count = ref(0)
function increment() { count.value++ }
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

**Definition:** Compile-time макрос для объявления props с опциональными TypeScript-типами или runtime-валидаторами.

**How it works:**

- Type-based: `defineProps<{ id: number; label?: string }>()`
- Defaults (3.5+): `const { label = 'Untitled' } = defineProps<{ label?: string }>()`
- Runtime: `defineProps({ id: { type: Number, required: true } })`
- `withDefaults(defineProps<Props>(), { label: 'Untitled' })` для сложных defaults (паттерн до 3.5)

**When to use:** Каждый компонент с входом от родителя. Предпочитайте type-based props с TS.

**Examples:**

```vue
<script setup lang="ts">
interface Props {
  items: string[]
  max?: number
}
const { max = 10 } = defineProps<Props>()
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

**Definition:** Объявляет события, которые компонент может эмитить родителю. Type-safe с tuple syntax.

**Examples:**

```vue
<script setup lang="ts">
const emit = defineEmits<{
  save: [payload: { id: number }]
  cancel: []
}>()

function onSave() {
  emit('save', { id: 1 })
}
</script>
```

**When to use:** Связь child → parent. Родитель слушает `@save="handler"`.

**Sources:**

[vuejs.org/guide/components/events.html](https://vuejs.org/guide/components/events.html)

defineModel

</details>

<details>
  <summary>defineModel</summary>

### defineModel

**Definition:** Макрос (Vue 3.4+) создаёт ref для двусторонней привязки `v-model`. Компилятор разворачивает в prop `modelValue` + emit `update:modelValue`.

**How it works:**

- `const model = defineModel<string>()` — default `v-model`
- `const title = defineModel<string>('title')` — `v-model:title`
- Опции: `required`, `default`, custom `get`/`set` для modifiers
- Возвращает ref — меняйте `model.value`, чтобы эмитить наверх

**Examples:**

```vue
<script setup lang="ts">
const model = defineModel<string>({ required: true })
const [value, modifiers] = defineModel<string>()
</script>

<template>
  <input v-model="model" />
</template>
```

**When to use:** Предпочтительнее ручного prop + emit для form controls и редактируемого child state.

**Sources:**

[vuejs.org/api/sfc-script-setup.html#definemodel](https://vuejs.org/api/sfc-script-setup.html#definemodel)

defineExpose

</details>

<details>
  <summary>defineExpose</summary>

### defineExpose

**Definition:** Явно открывает свойства/методы на инстансе компонента для parent **template refs**. Компоненты `<script setup>` по умолчанию закрыты.

**Examples:**

```vue
<script setup>
import { ref } from 'vue'

const input = ref<HTMLInputElement | null>(null)
function focus() { input.value?.focus() }

defineExpose({ focus })
</script>
```

```vue
<!-- Parent -->
<script setup>
import { useTemplateRef, onMounted } from 'vue'
const child = useTemplateRef('child')
onMounted(() => child.value?.focus())
</script>
<template><Child ref="child" /></template>
```

**When to use:** Редко — предпочитайте props/emits. OK для focus management, императивных chart API.

**Sources:**

[vuejs.org/api/sfc-script-setup.html#defineexpose](https://vuejs.org/api/sfc-script-setup.html#defineexpose)

Lifecycle hooks

</details>

<details>
  <summary>Lifecycle hooks</summary>

### Lifecycle hooks

**Definition:** Lifecycle-функции Composition API регистрируют callbacks во время `setup`. Они зеркалят Options API hooks.

**How it works:**

| Composition API      | Options API        | Когда                          |
| -------------------- | ------------------ | ----------------------------- |
| `onBeforeMount`      | `beforeMount`      | До первой вставки в DOM       |
| `onMounted`          | `mounted`          | После mount в DOM             |
| `onBeforeUpdate`     | `beforeUpdate`     | До re-render                  |
| `onUpdated`          | `updated`          | После patch DOM               |
| `onBeforeUnmount`    | `beforeDestroy`    | До teardown                   |
| `onUnmounted`        | `unmounted`        | После удаления                |
| `onErrorCaptured`    | `errorCaptured`    | Проброс ошибки child          |
| `onActivated`        | `activated`        | KeepAlive компонент показан   |
| `onDeactivated`      | `deactivated`      | KeepAlive компонент скрыт     |

**When to use:** `onMounted` для DOM access, подписок; `onUnmounted` для cleanup (таймеры, listeners).

**Examples:**

```ts
import { onMounted, onUnmounted } from 'vue'

onMounted(() => window.addEventListener('resize', onResize))
onUnmounted(() => window.removeEventListener('resize', onResize))
```

**Sources:**

[vuejs.org/api/composition-api-lifecycle.html](https://vuejs.org/api/composition-api-lifecycle.html)

Reactivity utilities

</details>

<details>
  <summary>Reactivity utilities</summary>

### Reactivity utilities

**Definition:** Вспомогательные функции для конвертации, клонирования или ограничения глубины реактивности.

| API            | Назначение                                      |
| -------------- | ----------------------------------------------- |
| `toRef`        | Reactive-свойство → ref                         |
| `toRefs`       | Reactive-объект → ref на каждый ключ            |
| `toValue`      | Нормализовать ref/getter/raw (3.3+)             |
| `unref`        | `val.value ?? val`                              |
| `isRef`        | Type guard                                      |
| `shallowRef`   | Отслеживается `.value`, deep — нет              |
| `triggerRef`   | Принудительное обновление shallowRef            |
| `readonly`     | Immutable proxy                                 |
| `markRaw`      | Opt out из реактивности                         |

**When to use:** `toRefs` при деструктуризации `reactive` state; `shallowRef` для больших immutable-объектов; `readonly` при передаче state вниз без mutation.

**Sources:**

[vuejs.org/api/reactivity-utilities.html](https://vuejs.org/api/reactivity-utilities.html)

</details>

<details>
  <summary>effectScope</summary>

### effectScope

**Definition:** **`effectScope`** группирует reactive effects (`watch`, `watchEffect`, `computed`), чтобы их можно было **остановить вместе** — полезно в composables и plugins, регистрирующих несколько watchers.

**How it works:**

```ts
import { effectScope, watchEffect } from 'vue'

const scope = effectScope()
scope.run(() => {
  watchEffect(() => console.log(count.value))
})
// later:
scope.stop() // disposes all effects in this scope
```

- `onScopeDispose(fn)` регистрирует cleanup внутри активного scope (как `onUnmounted` для effects)
- Vue использует scopes внутренне; авторы composables применяют это для library-grade cleanup

**When to use:** Custom composables, создающие watchers вне component lifecycle; test teardown. Редко в app code — предпочитайте `onUnmounted` в компонентах.

**Sources:** [vuejs.org/api/reactivity-advanced.html#effectscope](https://vuejs.org/api/reactivity-advanced.html#effectscope)

</details>

---

## 6. Composables & escape hatches

Custom composables

<details>
  <summary>Custom composables</summary>

### Custom composables

**Definition:** **Composable** — функция с конвенциями Composition API (имя `use*`), инкапсулирующая stateful-логику для переиспользования между компонентами.

**How it works:**

1. Вызывайте Vue API внутри (`ref`, `watch`, lifecycle)
2. Возвращайте refs/methods, которые caller деструктурирует
3. Каждый вызов в компоненте получает **своё** состояние (как custom hooks в React)
4. Принимайте геттеры для reactive props: `useUser(() => props.id)`

**When to use:** Общая fetch-логика, валидация форм, media queries, keyboard shortcuts.

**Examples:**

```ts
// composables/useCounter.ts
import { ref, computed } from 'vue'

export function useCounter(initial = 0) {
  const count = ref(initial)
  const doubled = computed(() => count.value * 2)
  function inc() { count.value++ }
  return { count, doubled, inc }
}
```

```vue
<script setup>
const { count, inc } = useCounter(10)
</script>
```

**Pluses and minuses:**

| +                              | −                                    |
| ------------------------------ | ------------------------------------ |
| DRY без mixins/HOC             | Коллизии имён без namespace          |
| Тестируется изолированно       | Неявные зависимости в огромном composable |

**Sources:**

[vuejs.org/guide/reusability/composables.html](https://vuejs.org/guide/reusability/composables.html)

Template refs

</details>

<details>
  <summary>Template refs</summary>

### Template refs

**Definition:** Доступ к DOM-элементам или child component instances после mount через атрибут `ref` на узлах template.

**How it works:**

- **Vue 3.5+:** `useTemplateRef('id')` соответствует `ref="id"` (поддерживает dynamic refs)
- **До 3.5:** объявите `const input = ref(null)` с совпадающим именем `ref="input"`
- Component refs открывают только то, что разрешил `defineExpose`
- `v-for` + ref → массив элементов после mount

**When to use:** Focus input, измерение элемента, интеграция не-Vue библиотек (charts). Предпочитайте декларативные props, когда возможно.

**Examples:**

```vue
<script setup>
import { useTemplateRef, onMounted } from 'vue'

const input = useTemplateRef<HTMLInputElement>('search')
onMounted(() => input.value?.focus())
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

**Definition:** `<Teleport to="body">` рендерит детей в другом месте DOM, сохраняя то же дерево компонентов (события, provide/inject, реактивность).

**When to use:** Модалки, toasts, tooltips, которым нужно выйти из `overflow: hidden` или z-index stacking contexts.

**Examples:**

```vue
<Teleport to="body">
  <div v-if="open" class="modal">...</div>
</Teleport>
```

**Pluses and minuses:**

| +                         | −                              |
| ------------------------- | ------------------------------ |
| Исправляет stacking/overflow | SSR: target должен существовать |
| Тот же логический компонент | `to` selector должен быть валидным |

**Sources:**

[vuejs.org/guide/built-ins/teleport.html](https://vuejs.org/guide/built-ins/teleport.html)

KeepAlive

</details>

<details>
  <summary>KeepAlive</summary>

### KeepAlive

**Definition:** Обёртка **кэширует** неактивные инстансы компонентов вместо уничтожения — сохраняет state при переключении вкладок или маршрутов.

**How it works:**

- Props `include` / `exclude` фильтруют по `name` компонента
- `max` ограничивает кэш (LRU)
- Кэшированные компоненты вызывают `onActivated` / `onDeactivated` вместо mount/unmount

**Examples:**

```vue
<KeepAlive :include="['TabA', 'TabB']" :max="5">
  <component :is="activeTab" />
</KeepAlive>
```

**When to use:** Панели вкладок, шаги wizard, когда пользователь возвращается с сохранённой формой.

**Sources:**

[vuejs.org/guide/built-ins/keep-alive.html](https://vuejs.org/guide/built-ins/keep-alive.html)

Transition & TransitionGroup

</details>

<details>
  <summary>Transition & TransitionGroup</summary>

### Transition & TransitionGroup

**Definition:** Встроенные обёртки, применяющие **enter/leave** CSS transitions или animations при mount/unmount дочерних элементов. `<Transition>` оборачивает один элемент или компонент; `<TransitionGroup>` анимирует списки (`v-for`) с move transitions.

**How it works:**

- `<Transition name="fade">` переключает классы: `fade-enter-from`, `fade-enter-active`, `fade-leave-to` и т.д.
- `mode="out-in"` — leave завершается до enter (часто для route/tab switches)
- `<TransitionGroup tag="ul">` — каждый keyed child получает enter/leave/move классы
- Работает с Vue Router `<router-view v-slot>` для page transitions; Nuxt использует те же примитивы в layouts

**When to use:** Modals, dropdowns, tab panels, list add/remove. Предпочитайте CSS transitions вместо JS, когда возможно.

**Pluses and minuses:**

| + | − |
|---|---|
| Декларативно, animation library не нужна | Требует стабильный `:key` на list items |
| Сочетается с `<KeepAlive>` для tab UX | Nested transitions требуют аккуратного timing |

**Examples:**

```vue
<Transition name="fade">
  <p v-if="show">Hello</p>
</Transition>

<style>
.fade-enter-active, .fade-leave-active { transition: opacity 0.3s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
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

**Definition:** Компоненты, загружаемые по требованию через dynamic `import()` — уменьшают начальный bundle.

**How it works:**

```ts
import { defineAsyncComponent } from 'vue'

const AdminPanel = defineAsyncComponent(() => import('./AdminPanel.vue'))
```

- Сочетайте с `<Suspense>` для loading fallback
- `defineAsyncComponent` принимает `loadingComponent`, `errorComponent`, `delay`, `timeout`

**When to use:** Code splitting на уровне маршрута (Vue Router `() => import()`), тяжёлые модалки, admin-секции.

**See also:** [Suspense](./4_vue_ssr.md#suspense) · [Vue Router lazy routes](./4_vue_ecosystem.md#vue-router)

**Sources:**

[vuejs.org/guide/components/async.html](https://vuejs.org/guide/components/async.html)

Suspense

</details>

<details>
  <summary>Suspense</summary>

### Suspense

**Definition:** Встроенный компонент со слотами `#default` и `#fallback`, показывающий fallback, пока async-зависимости в поддереве резолвятся (async components, async `setup`, server data).

**Examples:**

```vue
<Suspense>
  <AsyncDashboard />
  <template #fallback>Loading dashboard…</template>
</Suspense>
```

**When to use:** Согласованный loading UI для async-деревьев. SSR streaming использует границы Suspense.

**See also:** [SSR & hydration](./4_vue_ssr.md#8-ssr-hydration)

**Sources:**

[vuejs.org/guide/built-ins/suspense.html](https://vuejs.org/guide/built-ins/suspense.html)

You might not need a watcher

</details>

<details>
  <summary>You might not need a watcher</summary>

### You might not need a watcher

**Definition:** Многие паттерны, где в Options API использовали `watch`, лучше решать через `computed`, обработчики событий или `watchEffect` с ясным intent.

**How it works:**

| Вместо watch…              | Предпочитайте…                              |
| ------------------------------ | ------------------------------------ |
| Синхронизация derived UI state | `computed`                           |
| Реакция на клик            | `@click` handler                     |
| Сброс child при смене prop | `:key="id"` на child для remount      |
| Fetch при mount            | `onMounted` + function или composable |

**When to use:** Берите `watch`, когда нужно сравнение **old vs new**, debounced side effects или async-работа, привязанная к конкретному источнику.

**Sources:**

[vuejs.org/guide/essentials/watchers.html](https://vuejs.org/guide/essentials/watchers.html)

getCurrentInstance (caution)

</details>

<details>
  <summary>getCurrentInstance (caution)</summary>

### getCurrentInstance (caution)

**Definition:** `getCurrentInstance()` возвращает внутренний инстанс компонента только во время `setup`. **Не** часть публичного documented API для app-кода — для продвинутого authoring библиотек или отладки.

**When to use:** Почти никогда в application code. Предпочитайте `inject`, props и composables. Если нужен component proxy — вы, вероятно, боретесь с фреймворком.

**Thoughts:** Красный флаг на интервью при злоупотреблении. Легитимные случаи: авторы плагинов, test utilities.

**Sources:**

[vuejs.org/api/composition-api-helpers.html#getcurrentinstance](https://vuejs.org/api/composition-api-helpers.html#getcurrentinstance)

</details>
