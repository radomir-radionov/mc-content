# Vue — Core

Модель библиотеки, рендеринг, шаблоны и состояние.

[← Вернуться в центр](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Экосистема](./4_vue_ecosystem.md) · [Vue — Интервью](./4_vue_interview.md)

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

**Definition:** Vue — это **прогрессивный** JavaScript-фреймворк для создания пользовательских интерфейсов. Он фокусируется на view layer, но масштабируется до полноценных SPA с официальным routing (Vue Router) и state (Pinia). Vue 3 использует proxy-based систему реактивности и компилятор, оптимизирующий шаблоны на этапе сборки.

**When to use:** SPA, dashboards, design systems, встраиваемые виджеты (`createApp` на узле), SSR-приложения (Vite SSR или meta-frameworks вроде [Nuxt](./4_nuxt_core.md)).

**Pluses and minuses:**

| +                                                              | −                                                                |
| -------------------------------------------------------------- | ---------------------------------------------------------------- |
| Мягкая кривая обучения; шаблоны похожи на HTML                 | Два API (Options + Composition) путают новичков                  |
| Отличная документация и официальный tooling (Vite, Vue Router) | Меньшая экосистема, чем у React в некоторых нишах (RSC, RN)      |
| Тонкая реактивность — обновления отслеживают зависимости       | Template compiler добавляет build step (SFC) vs pure JSX runtime |
| `<script setup>` + вывод TypeScript из коробки                 | Legacy Vue 2 codebases требуют план миграции                     |
| Прогрессивное внедрение — виджет на странице или full SPA      | Nuxt-специфичные паттерны отдельно от core Vue                   |

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

**Thoughts:** Vue часто называют фреймворком, потому что core team поставляет Router, Pinia и интеграцию с Vite — но по сути Vue остаётся **view library**, которую вы монтируете сами. Для file-based routing и Nitro server используйте [Nuxt](./4_nuxt_core.md) вместо ручной сборки.

**Sources:**

[vuejs.org](https://vuejs.org/)

Vue vs Vue DOM

</details>

<details>
  <summary>Vue vs Vue DOM</summary>

### Vue vs Vue DOM

**Definition:** `vue` = ядро (реактивность, компоненты, компилятор). `@vue/runtime-dom` (реэкспорт через `vue`) = web renderer, патчащий реальный DOM. Одна major-версия для всех пакетов `@vue/*`.

**How it works:**

| Package / import       | Роль                                     |
| ---------------------- | ---------------------------------------- |
| `vue`                  | Reactivity, components, `createApp`      |
| `vue` (runtime-dom)    | `mount`, `patch`, event listeners на DOM |
| `@vue/server-renderer` | `renderToString`, SSR context            |
| `@vue/compiler-sfc`    | Компиляция `.vue` SFC (Vite plugin)      |

`createApp` создаёт app instance; `mount('#app')` запускает первый рендер и подключает listeners. Обновления идут через pipeline реактивность → render → patch.

**When to use:** Всегда `vue` на web. SSR добавляет `@vue/server-renderer`. Custom renderers для native (`@vue/runtime-native`) — нишевые.

**Examples:**

```ts
import { createApp } from "vue";
import { createSSRApp } from "vue"; // SSR entry

createApp(App).mount("#app");
```

**Thoughts:** В отличие от явного split `react` + `react-dom` в imports, большинство Vue apps импортирует всё из `vue`. Разделение всё равно есть под капотом.

**See also:** [Render pipeline](#render-pipeline) · [SSR & hydration](./4_vue_ssr.md#8-ssr-hydration)

**Sources:**

[vuejs.org/api/application.html](https://vuejs.org/api/application.html)

Library vs framework

</details>

<details>
  <summary>Library vs framework</summary>

### Library vs framework

**Definition:** **Library** решает узкую задачу (UI rendering); архитектуру контролируете вы. **Framework** инвертирует управление — задаёт структуру проекта, routing и data conventions.


| | Library (Vue core)    | Framework (Nuxt)                      |
| | -------------  |  ---------------------  |
| | Control       | Вы собираете стек     | Framework задаёт conventions          |
| | Routing       | Приносите Vue Router  | File-based routes из коробки          |
| | Data fetching | Bring your own        | `useFetch`, server routes через Nitro |
| | SSR           | Ручной Vite SSR setup | SSR/SSG по умолчанию                  |

**Examples:**

```ts
// Library — вы сами подключаете routing
import { createRouter, createWebHistory } from "vue-router";

// Framework — Nuxt владеет file-based routes и server data
// See 4_nuxt_routing.md, 4_nuxt_data.md
```

**Thoughts:** Ответ на интервью: Vue **core** — прогрессивная UI library. **Nuxt** — full-stack Vue framework. Vue Router + Pinia + Vite — частый «framework-like» стек без Nuxt.

**Sources:**

[vuejs.org/guide/extras/ways-of-using-vue.html](https://vuejs.org/guide/extras/ways-of-using-vue.html)

Philosophy

</details>

<details>
  <summary>Philosophy</summary>

### Philosophy

**Definition:** Цели дизайна Vue: **approachable** (HTML-like templates), **versatile** (виджет → SPA), **performant** (compiler hints + proxy reactivity).

**How it works:**

1. **Templates** компилируются в render functions с reactivity-tracked reads
2. **Single-file components** совмещают template, logic и scoped styles
3. **Progressive enhancement** — script tag, Vite SPA или Nuxt full-stack
4. **Composition API** (Vue 3) группирует логику по фиче, а не по типу опции

**When to use:** Команды, которым нужны декларативные templates с меньшей JSX-церемонией; постепенное внедрение в legacy pages.

**Pluses and minuses:**

| +                                     | −                                         |
| ------------------------------------- | ----------------------------------------- |
| HTML-знакомые templates               | Двуязычная mental model template + script |
| Официальный style guide и tooling     | Дуальность Options API vs Composition API |
| Компилятор оптимизирует static hoists | SFC требует build pipeline                |

**Thoughts:** Vue 3 рекомендует Composition API + `<script setup>` для нового кода. Options API остаётся для поддержки и постепенной миграции.

**Sources:**

[vuejs.org/guide/introduction.html#what-is-vue](https://vuejs.org/guide/introduction.html#what-is-vue)

Vue 3 vs Vue 2

</details>

<details>
  <summary>Vue 3 vs Vue 2</summary>

### Vue 3 vs Vue 2

**Definition:** Vue 3 (2020) переписал реактивность на **Proxies** (вместо `Object.defineProperty`), ввёл **Composition API**, **Fragments**, **Teleport**, лучшую поддержку TypeScript и меньший tree-shaken bundle.

**How it works:**

| Area           | Vue 2                         | Vue 3                              |
| -------------- | ----------------------------- | ---------------------------------- |
| Reactivity     | `defineProperty`, array hacks | `Proxy`-based, Map/Set native      |
| API            | Options API primary           | Composition API + Options API      |
| Multiple roots | Single root required          | Fragments (multiple root nodes)    |
| Global API     | `Vue.use`, `Vue.component`    | `createApp` — per-app registration |
| State          | Vuex 3/4                      | Pinia (official)                   |
| Build          | Vue CLI (webpack)             | Vite (official)                    |

**When to use:** Все новые проекты на Vue 3. Vue 2 EOL — декабрь 2023; мигрируйте или временно используйте migration build `@vue/compat`.

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

**Definition:** Vue превращает state компонента в **VNode tree**, сравнивает с предыдущим деревом и **патчит** реальный DOM минимальными операциями.

**How it works:**

1. Меняется **reactive dependency** (например `count.value++`)
2. **Scheduler** ставит render effect компонента в очередь
3. **Render function** (из template compiler или `render()`) создаёт новые VNodes
4. **Patch algorithm** сравнивает old vs new VNodes (тот же type + `key` → update; смена type → replace)
5. **DOM update** применяет изменения property/event/text

```
state change → effect → render() → VNode tree → patch → DOM
```

**When to use:** Понимание объясняет, почему обновляются только затронутые reactive data и зачем `key` в `v-for`.

**Pluses and minuses:**

| +                                                         | −                                               |
| --------------------------------------------------------- | ----------------------------------------------- |
| Compiler static hoisting пропускает diff для static nodes | Абстракции скрывают момент re-render            |
| Block tree (Vue 3) ускоряет dynamic children              | Глубокие component trees всё равно стоят render |

**See also:** [Virtual DOM & patch](#virtual-dom-patch) · [Reactivity system](#reactivity-system)

**Sources:**

[vuejs.org/guide/extras/rendering-mechanism.html](https://vuejs.org/guide/extras/rendering-mechanism.html)

Virtual DOM & patch

</details>

<details>
  <summary>Virtual DOM & patch</summary>

### Virtual DOM & patch

**Definition:** **VNode** (virtual node) — plain object, описывающий DOM element или component (`type`, `props`, `children`, `key`). Vue не diff-ит live DOM — diff-ит VNode trees.

**How it works:**

- **Element nodes** — tag, props, children
- **Component nodes** — component constructor + props + slots
- **Text / comment / Fragment** — лёгкие типы узлов
- **Patch flags** (compiler-generated) говорят runtime, какие props реально изменились — пропуск полного prop diff

**When to use:** Отладка лишних re-render; понимание hydration mismatch (server VNode ≠ client VNode).

**Examples:**

```js
// Conceptual VNode shape (simplified)
{ type: 'div', props: { id: 'app' }, children: [...] }
{ type: MyComponent, props: { msg: 'hi' } }
```

**Thoughts:** Компилятор Vue 3 помечает dynamic bindings patch flags — поэтому SFC templates могут обгонять hand-written render functions с избыточным diff.

**See also:** [Keys](#keys)

**Sources:**

[vuejs.org/guide/extras/rendering-mechanism.html#virtual-dom](https://vuejs.org/guide/extras/rendering-mechanism.html)

Reactivity system

</details>

<details>
  <summary>Reactivity system</summary>

### Reactivity system

**Definition:** Vue 3 отслеживает **property access** в runtime через `Proxy` wrappers. Когда tracked data читается во время render, render effect подписывается; при записи subscribers перезапускаются.

**How it works:**

1. `ref()` / `reactive()` оборачивают значения в reactive proxies
2. **Track** на `get` (во время render, `computed`, getters `watch`)
3. **Trigger** на `set` → schedule component/effect re-run
4. `effect` батчит обновления в microtask queue (same tick coalesced)

| API          | Wraps                  | Access in script                    |
| ------------ | ---------------------- | ----------------------------------- |
| `ref`        | Any value (object too) | `.value`                            |
| `reactive`   | Objects only           | direct property                     |
| `computed`   | Derived read-only ref  | `.value` or auto-unwrap in template |
| `shallowRef` | One-level ref          | `.value` — deep changes not tracked |

**When to use:** Default state primitive — `ref`. `reactive` для form objects с in-place mutation. `computed` для derived values.

**See also:** [ref](#ref) · [reactive](#reactive) · [computed](#computed)

**Sources:**

[vuejs.org/guide/essentials/reactivity-fundamentals.html](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)

Compiler vs runtime

</details>

<details>
  <summary>Compiler vs runtime</summary>

### Compiler vs runtime

**Definition:** Vue SFC templates **компилируются на build time** в render functions. **Runtime** выполняет эти functions и патчит DOM. Vue без компилятора — `runtime-only` build + `render()` functions.

**How it works:**

| Build                                 | Includes compiler | Use case                           |
| ------------------------------------- | ----------------- | ---------------------------------- |
| `vue` (full)                          | Yes               | In-browser template compile (rare) |
| `vue/dist/vue.runtime.esm-bundler.js` | No                | Vite/Vue CLI — SFC pre-compiled    |

Compiler optimizations: static hoisting, patch flags, block trees, event handler caching.

**When to use:** Всегда pre-compile SFC в production (Vite делает это). Runtime-only меньше по размеру.

**Sources:**

[vuejs.org/guide/scaling-up/tooling.html](https://vuejs.org/guide/scaling-up/tooling.html)

Keys

</details>

<details>
  <summary>Keys</summary>

### Keys

**Definition:** Атрибут `:key` на элементах `v-for` даёт каждому VNode **стабильную идентичность** между re-render, чтобы Vue переиспользовал/перемещал DOM nodes вместо destroy/recreate.

**How it works:**

- Тот же `type` + тот же `key` → update in place
- Отсутствующие или дублирующие keys → непредсказуемое reuse, state bugs
- Index as key → OK только для **статических** списков без reorder

**When to use:** Всегда unique stable id (`item.id`) для dynamic lists (filter, sort, paginate).

**Examples:**

```vue
<li v-for="todo in todos" :key="todo.id">{{ todo.text }}</li>
<!-- Avoid :key="index" when list mutates order -->
```

**Pluses and minuses:**

| +                                     | −                             |
| ------------------------------------- | ----------------------------- |
| Сохраняет focus/state input в списках | Неверный key → тонкие UI bugs |
| Эффективный list diff                 | —                             |

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

**Definition:** Файл `.vue` объединяет `<template>`, `<script>` и опциональный `<style scoped>` в одном модуле. Vite (через `@vitejs/plugin-vue`) компилирует его в JavaScript для браузера.

**How it works:**

1. `<script setup>` выполняется один раз на инстанс — настраивает reactive state и регистрирует bindings для template
2. `<template>` компилируется в `render` function
3. `<style scoped>` добавляет data attributes, чтобы CSS таргетил только этот компонент

**When to use:** По умолчанию для всех Vue 3 apps. Совмещает markup, logic и styles.

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

| +                        | −                             |
| ------------------------ | ----------------------------- |
| IDE support (Volar), HMR | Требует build step            |
| Scoped CSS без CSS-in-JS | Template — не plain HTML file |

**Sources:**

[vuejs.org/guide/scaling-up/sfc.html](https://vuejs.org/guide/scaling-up/sfc.html)

Template syntax

</details>

<details>
  <summary>Template syntax</summary>

### Template syntax

**Definition:** Vue templates расширяют HTML **interpolations**, **directives** (`v-*`) и shorthand для **events** / **bindings**.

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

**When to use:** Default UI layer в SFC. `render()` только для highly dynamic programmatic trees.

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

**Definition:** Components — переиспользуемые Vue instances с именем, props interface, events и slots. Складываются в дерево; parent передаёт data через props, child сигналит вверх через emits.

**How it works:**

1. Register globally (`app.component`) или locally (`import Child from './Child.vue'`)
2. В `<script setup>` imported components auto-registered в template
3. PascalCase в template (`<MyButton />`) рекомендуется; kebab-case тоже работает

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

**Definition:** **Props** — read-only входы от родителя. Child не должен мутировать props напрямую — emit event, родитель обновляет.

**How it works:**

- Объявление через `defineProps()` в `<script setup>` (compile-time macro)
- Type-based: `defineProps<{ id: number; title?: string }>()`
- Runtime: `defineProps({ id: Number, title: String })`
- Vue 3.5+: destructured props из `defineProps` реактивны (компилятор переписывает в `props.foo`)

**When to use:** Весь parent → child data flow. Defaults через destructuring (3.5+) или `withDefaults`.

**Examples:**

```vue
<script setup lang="ts">
const { count = 0 } = defineProps<{ count?: number }>();
</script>
```

**Pluses and minuses:**

| +                              | −                                                     |
| ------------------------------ | ----------------------------------------------------- |
| One-way data flow, predictable | Prop drilling в deep trees → provide/inject или Pinia |
| TS inference с defineProps     | Много boolean props → рассмотрите slots               |

**See also:** [State vs props](#state-vs-props) · [defineProps](./4_vue_composition.md#defineprops)

**Sources:**

[vuejs.org/guide/components/props.html](https://vuejs.org/guide/components/props.html)

inheritAttrs & fallthrough attributes

</details>

<details>
  <summary>inheritAttrs & fallthrough attributes</summary>

### inheritAttrs & fallthrough attributes

**Definition:** Атрибуты, переданные компоненту, которые **не** объявлены как props или emits, **fall through** на root element в template. `inheritAttrs: false` (Options API) или `defineOptions({ inheritAttrs: false })` останавливает автоматический fallthrough, чтобы вы могли вручную пробросить attrs (wrapper components).

**How it works:**

- Parent: `<BaseInput class="wide" data-test="email" label="Email" />`
- Если `label` — prop, а `class`/`data-test` — нет, `class` и `data-test` применяются к root element child
- Wrapper pattern: bind `$attrs` на inner input — `<input v-bind="$attrs" />`
- В `<script setup>` `useAttrs()` возвращает reactive fallthrough attrs при `inheritAttrs: false`

**When to use:** Input/button wrappers, design-system primitives, пробрасывающие native HTML attrs на inner element. Отключайте inheritAttrs, когда root — не тот элемент, который должен получать attrs.

**Pluses and minuses:**

| +                                                               | −                                                   |
| --------------------------------------------------------------- | --------------------------------------------------- |
| Ergonomic wrapper components без re-declaring каждого HTML attr | Легко забыть, какие attrs попадают на root vs inner |
| `$attrs` исключает declared props и emits                       | `class`/`style` merge на root по умолчанию          |

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

**Definition:** **Slots** позволяют родителю вставлять markup в template child — composition без prop explosion.

**How it works:**

- **Default slot** — `<slot />` fallback content
- **Named slots** — `<slot name="header" />` → `<template #header>`
- **Scoped slots** — child передаёт data в slot: `<slot :item="row" />` → `#default="{ item }"`

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

**Definition:** Directives — атрибуты `v-*`, применяющие reactive side effects к DOM elements. Built-ins покрывают большинство случаев; **custom directives** регистрируются глобально или локально для low-level DOM behavior.

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

**Custom directive hooks** (на bound element):

| Hook            | When                    |
| --------------- | ----------------------- |
| `created`       | Before attrs applied    |
| `beforeMount`   | Before element inserted |
| `mounted`       | Element inserted        |
| `beforeUpdate`  | Before parent re-render |
| `updated`       | After parent re-render  |
| `beforeUnmount` | Before unmount          |
| `unmounted`     | Element removed         |

Register: `app.directive('focus', { mounted(el) { el.focus() } })` или `const vFocus = { mounted(el) { el.focus() } }` в `<script setup>`.

**When to use:** Built-ins для 95% UI. Custom directives для focus traps, tooltips или third-party DOM plugins — предпочитайте composables, когда логика не DOM-specific.

**Sources:**

[vuejs.org/api/built-in-directives.html](https://vuejs.org/api/built-in-directives.html) · [vuejs.org/guide/reusability/custom-directives.html](https://vuejs.org/guide/reusability/custom-directives.html)

Conditional rendering & lists

</details>

<details>
  <summary>Conditional rendering & lists</summary>

### Conditional rendering & lists

**Definition:** `v-if` / `v-else-if` / `v-else` условно **создают/уничтожают** DOM. `v-for` рендерит коллекции; обязателен `:key`.

**How it works:**

- `v-if` на `<template>` оборачивает несколько элементов без лишнего DOM
- `v-for` может деструктурировать: `v-for="{ id, name } in users"`
- Не комбинируйте `v-if` и `v-for` на одном элементе — computed filter или wrapper `<template>`

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

**Definition:** `:class` и `:style` принимают objects, arrays или strings для dynamic styling. Vue сливает со static `class` attributes.

**Examples:**

```vue
<div :class="{ active: isActive, 'text-danger': hasError }" />
<div :class="[baseClass, isActive && 'active']" />
<p :style="{ color: activeColor, fontSize: size + 'px' }" />
```

**When to use:** Предпочитайте component props + scoped CSS вместо inline styles для maintainability.

**Sources:**

[vuejs.org/guide/essentials/class-and-style.html](https://vuejs.org/guide/essentials/class-and-style.html)

</details>

---

## 4. State & interactivity

ref

<details>
  <summary>ref</summary>

### ref

**Definition:** `ref()` оборачивает любое значение в reactive object со свойством `.value`. Работают и primitives, и object references.

**How it works:**

- Чтение/запись `.value` в script track и trigger updates
- Templates **auto-unwrap** refs — используйте `count`, не `count.value`
- `ref` на DOM element (или `useTemplateRef`) держит элемент после mount

**When to use:** По умолчанию для scalars, flags, async results и когда нужно заменить целое значение (`user.value = newUser`).

**Examples:**

```ts
import { ref } from "vue";

const count = ref(0);
count.value++;

const el = ref<HTMLInputElement | null>(null);
```

**Pluses and minuses:**

| +                          | −                                   |
| -------------------------- | ----------------------------------- |
| Работает с primitives      | Ceremony `.value` в script          |
| Чистая замена всего object | Легко забыть `.value` в TS handlers |

**See also:** [reactive](#reactive) · [ref vs reactive](./4_vue_interview.md#ref-vs-reactive)

**Sources:**

[vuejs.org/api/reactivity-core.html#ref](https://vuejs.org/api/reactivity-core.html#ref)

reactive

</details>

<details>
  <summary>reactive</summary>

### reactive

**Definition:** `reactive()` возвращает **Proxy** объекта (или array), где nested property access отслеживается. Не оборачивает primitives.

**How it works:**

- Mutate in place: `state.count++` — без `.value`
- Destructuring ломает reactivity — используйте `toRefs()` или держите reactive object целиком
- `readonly()` / `shallowReactive()` для immutable или shallow tracking

**When to use:** Form state objects, grouped settings, records с field-by-field update.

**Examples:**

```ts
import { reactive } from "vue";

const state = reactive({ count: 0, nested: { ok: true } });
state.count++;
```

**Pluses and minuses:**

| +                            | −                                              |
| ---------------------------- | ---------------------------------------------- |
| Естественная object mutation | Destructuring теряет reactivity                |
| Deep reactivity по умолчанию | Замена root — `Object.assign` или new reactive |

**Sources:**

[vuejs.org/api/reactivity-core.html#reactive](https://vuejs.org/api/reactivity-core.html#reactive)

computed

</details>

<details>
  <summary>computed</summary>

### computed

**Definition:** `computed()` создаёт **cached derived ref**, который пересчитывается только при изменении dependencies.

**How it works:**

- Getter выполняется lazily on read; результат кэшируется до смены deps
- Computed refs **readonly** по умолчанию; writable computed — `{ get, set }`
- В templates без `.value`

**When to use:** Derived lists (filtered todos), formatted labels, expensive calculations — не для side effects (используйте `watch`).

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

**Definition:** `watch` запускает callback при изменении конкретного reactive source. `watchEffect` auto-tracks dependencies в синхронном теле и перезапускается сразу.

**How it works:**

| API           | Tracks             | Runs on mount           | Use for                             |
| ------------- | ------------------ | ----------------------- | ----------------------------------- |
| `watchEffect` | Implicit (reads)   | Yes                     | Sync to external systems            |
| `watch`       | Explicit source(s) | No (unless `immediate`) | Compare old/new, async side effects |

- `watch(source, callback, { flush: 'post' })` после DOM update
- Return value `watchEffect` — handle `stop()`

**When to use:** API calls при смене filter (`watch`), document title sync (`watchEffect`). Для derived UI state предпочитайте `computed`.

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

**Definition:** `v-model` — syntactic sugar для пары prop + event: `:modelValue` + `@update:modelValue`. Поддерживает несколько named models (`v-model:title`).

**How it works:**

- На native inputs `v-model` биндит `value` / `checked` и слушает `input` / `change`
- На components: child exposes `modelValue` prop и emits `update:modelValue`
- Vue 3.4+: макрос `defineModel()` упрощает child implementation

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

**Definition:** **Props** текут вниз от родителя (read-only в child). **State** (`ref`/`reactive`) принадлежит компоненту и trigger re-render при изменении.

**How it works:**

- Share sibling state → lift to parent или Pinia
- Props обновляются, когда parent re-renders с новыми values
- Child emits events; parent обновляет state → новые props вниз

**When to use:** Props для configuration и data; local state для UI, который parent не нужен (open/closed, draft text).

**See also:** [provide / inject](#provide-inject) · [Pinia](./4_vue_ecosystem.md#pinia)

**Sources:**

[vuejs.org/guide/components/props.html](https://vuejs.org/guide/components/props.html)

provide / inject

</details>

<details>
  <summary>provide / inject</summary>

### provide / inject

**Definition:** `provide(key, value)` регистрирует value в ancestor chain; `inject(key, default)` читает в descendants — dependency injection без prop drilling.

**How it works:**

- Keys могут быть `Symbol` или string
- Provide reactive refs/computed, чтобы descendants оставались updated
- Полезно для theme, locale, form context — не замена global app state

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

**When to use:** Внутренности component library, deeply nested trees. Для cross-route shared state предпочитайте Pinia.

**Sources:**

[vuejs.org/guide/components/provide-inject.html](https://vuejs.org/guide/components/provide-inject.html)

</details>
