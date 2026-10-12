# Vue — Интервью

Вопросы и ответы для интервью и задачи по коду.

[← Вернуться в центр](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Экосистема](./4_vue_ecosystem.md) · [Vue — Интервью](./4_vue_interview.md)

### Sections

- [Questions & Answers](#questions-answers)
- [Coding tasks](#coding-tasks)
- [Sources](#sources)

---

## Questions & Answers

What is Vue?

<details>
  <summary>Что такое Vue?</summary>

### Что такое Vue?

**Answer:** Vue — это прогрессивный JavaScript **фреймворк** для создания пользовательских интерфейсов с компонентной моделью, шаблонами или render-функциями и системой реактивности на основе Proxy. Vue 3 использует Composition API (`ref`, `computed`, `watch`) и компилирует SFC на этапе сборки. Официальные спутники: Vue Router, Pinia, Vite.

**See also:** [Vue overview](./4_vue_core.md#vue-overview)

Vue library or framework?

</details>

<details>
  <summary>В чём разница между computed и watch?</summary>

### В чём разница между computed и watch?

**Answer:** Ядро Vue — это прогрессивная UI-библиотека, которую вы монтируете через `createApp`. С Vue Router + Pinia + Vite она ведёт себя как стек фреймворка. **Nuxt** — полноценный full-stack фреймворк поверх Vue. На интервью: «Vue прогрессивен — библиотека в ядре, экосистема дополняет стек».

**See also:** [Library vs framework](./4_vue_core.md#library-vs-framework) · [Nuxt — Core](./4_nuxt_core.md)

State vs props?

</details>

<details>
  <summary>В чём разница между state и props?</summary>

### В чём разница между state и props?

**Answer:** **Props** — это read-only входы от родителя. **State** (`ref`/`reactive`) принадлежит компоненту. Дочерний компонент эмитит события; родитель обновляет состояние → новые props текут вниз. Соседи делят состояние через подъём state, provide/inject или Pinia.

**See also:** [State vs props](./4_vue_core.md#state-vs-props)

ref vs reactive?

</details>

<details>
  <summary>В чём разница между ref и reactive?</summary>

### В чём разница между ref и reactive?

**Answer:** `ref` оборачивает любое значение — в script обращайтесь через `.value`, в template авто-разворачивается. `reactive` оборачивает только объекты — свойства меняются напрямую. Деструктуризация `reactive` теряет отслеживание — используйте `toRefs`. По умолчанию для примитивов и замены целого значения берите `ref`.

**See also:** [ref](./4_vue_core.md#ref) · [reactive](./4_vue_core.md#reactive)

computed vs watch?

</details>

<details>
  <summary>В чём разница между computed и watch?</summary>

### В чём разница между computed и watch?

**Answer:** `computed` = **кэшированное производное значение** для template (без побочных эффектов). `watch` / `watchEffect` = **побочные эффекты** при изменении источников (API-вызовы, логирование). Если синхронизируете UI из state — `computed`; если реагируете на изменения с async-работой — `watch`.

**See also:** [computed](./4_vue_core.md#computed) · [watch & watchEffect](./4_vue_core.md#watch-watcheffect)

What is script setup?

</details>

<details>
  <summary>Что такое script setup?</summary>

### Что такое script setup?

**Answer:** `<script setup>` — compile-time sugar: top-level bindings автоматически доступны в template. Макросы `defineProps`, `defineEmits`, `defineModel`, `defineExpose` не требуют import. Выполняется один раз на инстанс до рендера — эквивалент `setup()` с return.

**See also:** [script setup](./4_vue_composition.md#script-setup)

What is defineModel?

</details>

<details>
  <summary>Что такое defineModel?</summary>

### Что такое defineModel?

**Answer:** Макрос (3.4+), возвращающий ref для `v-model`. Компилятор разворачивает в prop `modelValue` + emit `update:modelValue`. Именованный: `defineModel('title')` для `v-model:title`. Заменяет ручной prop + emit.

**See also:** [defineModel](./4_vue_composition.md#definemodel)

Composition API vs Options API?

</details>

<details>
  <summary>В чём разница между Composition API и Options API?</summary>

### В чём разница между Composition API и Options API?

**Answer:** **Options API** делит логику по `data`, `methods`, `computed`, `watch`. **Composition API** группирует логику фичи в `setup` / `<script setup>` и выносит composables. Vue 3 рекомендует Composition для нового кода; Options остаётся для поддержки и знакомства с Vue 2.

**See also:** [Overview](./4_vue_composition.md#overview) · [Options API](./4_vue_ecosystem.md#options-api)

What is a composable?

</details>

<details>
  <summary>Что такое composable?</summary>

### Что такое composable?

**Answer:** Функция `use*`, инкапсулирующая stateful-логику через Composition API. Каждый вызов в компоненте получает своё состояние. Как custom hooks в React — вынос fetch, форм, media queries. Для реактивных props передавайте геттеры: `useUser(() => props.id)`.

**See also:** [Custom composables](./4_vue_composition.md#custom-composables)

v-if vs v-show?

</details>

<details>
  <summary>v-if vs v-show?</summary>

### v-if vs v-show?

**Answer:** `v-if` **монтирует/размонтирует** DOM (лениво, выше стоимость переключения). `v-show` переключает `display: none` (элемент остаётся в DOM). Используйте `v-if` для редких/тяжёлых поддеревьев; `v-show` для частых переключений (вкладки).

**See also:** [Directives](./4_vue_core.md#directives)

Why list keys? Index as key?

</details>

<details>
  <summary>Почему list keys? Index as key?</summary>

### Почему list keys? Index as key?

**Answer:** `:key` даёт VNode стабильную идентичность между рендерами. Индекс допустим только для **статических** списков. Reorder/filter + index → неверное переиспользование DOM и устаревшее состояние input. Используйте уникальные id (`item.id`).

**See also:** [Keys](./4_vue_core.md#keys)

How does Vue reactivity work?

</details>

<details>
  <summary>Как Vue Reactivity work?</summary>

### Как Vue Reactivity work?

**Answer:** Vue 3 использует `Proxy` для **отслеживания** чтений свойств (`get`) и **триггера** подписчиков при записи (`set`). Render effects перезапускаются, когда меняются отслеженные reactive-данные. `ref` оборачивает `.value`; `reactive` глубоко отслеживает объекты. Scheduler батчит обновления в microtask.

**See also:** [Reactivity system](./4_vue_core.md#reactivity-system)

Pinia vs Vuex?

</details>

<details>
  <summary>Pinia vs Vuex?</summary>

### Pinia vs Vuex?

**Answer:** Pinia — официальный store для Vue 3: без mutations, проще API, лучше TS, модульные stores. Vuex 4 только в режиме поддержки. Новые проекты: Pinia. Мигрируйте stores по одному.

**See also:** [Pinia](./4_vue_ecosystem.md#pinia) · [Vuex (legacy)](./4_vue_ecosystem.md#vuex-legacy)

Vue Router navigation guards?

</details>

<details>
  <summary>Vue Router navigation guards?</summary>

### Vue Router navigation guards?

**Answer:** `beforeEach` (глобальный), `beforeEnter` (на маршрут), `beforeRouteEnter` (в компоненте) могут редиректить или отменять навигацию. Для auth: если !token → `next('/login')`. Async guards могут await fetch перед разрешением маршрута.

**See also:** [Vue Router](./4_vue_ecosystem.md#vue-router)

SPA vs SSR in Vue?

</details>

<details>
  <summary>SPA vs SSR в Vue?</summary>

### SPA vs SSR в Vue?

**Answer:** SPA: пустая оболочка + клиентский `createApp().mount()`. SSR: сервер `renderToString(createSSRApp)` → HTML; клиент гидратирует через `createSSRApp().mount()`. SSR требует совпадения server/client разметки. Nuxt автоматизирует SSR — см. доки Nuxt, не глубину core Vue на интервью.

**See also:** [SPA vs SSR](./4_vue_ssr.md#spa-vs-ssr) · [Hydration](./4_vue_ssr.md#hydration)

What is hydration mismatch?

</details>

<details>
  <summary>Что такое hydration mismatch?</summary>

### Что такое hydration mismatch?

**Answer:** HTML сервера ≠ первый клиентский рендер → Vue предупреждает и может отбросить server DOM. Причины: `Date.now()`, `Math.random()`, browser-only API в setup, невалидная вложенность HTML. Исправление: guard через `onMounted`, `ClientOnly` (Nuxt) или `data-allow-mismatch` для намеренных отличий.

**See also:** [Hydration](./4_vue_ssr.md#hydration)

Vue 2 vs Vue 3 main differences?

</details>

<details>
  <summary>Vue 2 vs Vue 3 — основные различия?</summary>

### Vue 2 vs Vue 3 — основные различия?

**Answer:** Proxy-реактивность (vs `defineProperty`), Composition API, `createApp` (vs `new Vue`), fragments, Teleport, удалённые filters, новая конвенция `v-model`, Vue Router 4, Pinia вместо Vuex, Vite вместо Vue CLI. Vue 2 EOL — декабрь 2023.

**See also:** [Vue 3 vs Vue 2](./4_vue_core.md#vue-3-vs-vue-2) · [Vue 2 → 3 migration](./4_vue_ecosystem.md#vue-2-3-migration)

Teleport — what and why?

</details>

<details>
  <summary>Teleport — что и зачем?</summary>

### Teleport — что и зачем?

**Answer:** Рендерит детей в другой DOM-узел (например `body`), сохраняя то же дерево компонентов — события и provide/inject работают. Исправляет модалки, застрявшие в `overflow: hidden` или z-index контекстах.

**See also:** [Teleport](./4_vue_composition.md#teleport)

provide/inject vs Pinia?

</details>

<details>
  <summary>provide/inject vs Pinia?</summary>

### provide/inject vs Pinia?

**Answer:** provide/inject = DI от предка к потомку для темы, form context, внутренностей библиотек. Pinia = глобальное app state между маршрутами. Не используйте provide для cart/auth, нужных на каждой странице — берите Pinia.

**See also:** [provide / inject](./4_vue_core.md#provide-inject) · [State management patterns](./4_vue_ecosystem.md#state-management-patterns)

How to optimize Vue performance?

</details>

<details>
  <summary>Как optimize Vue performance?</summary>

### Как optimize Vue performance?

**Answer:** Сначала профилируйте. Lazy routes/components, `v-memo` на тяжёлых списках, `shallowRef` для больших immutable-данных, виртуализация длинных списков, SSR/SSG для LCP. Компилятор уже поднимает статические узлы — не размазывайте `computed` везде преждевременно.

**See also:** [Performance](./4_vue_ecosystem.md#10-performance)

</details>

---

## Coding tasks

Task 1 — Lost reactivity on destructure

<details>
  <summary>Task 1 — Lost reactivity при destructure</summary>

### Task 1 — Lost reactivity при destructure

**Problem:** Счётчик перестаёт обновляться в template после деструктуризации.

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

**Answer:** Деструктуризация извлекает обычное число — не отслеживается. Исправление:

```vue
<script setup>
import { reactive } from "vue";
const state = reactive({ count: 0 });
</script>
<template>
  <button @click="state.count++">{{ state.count }}</button>
</template>
```

Или `const { count } = toRefs(state)` и `count.value` в script.

**See also:** [reactive](./4_vue_core.md#reactive) · [Reactivity utilities](./4_vue_composition.md#reactivity-utilities)

</details>

Task 2 — v-for without key

<details>
  <summary>Task 2 — v-for без key</summary>

### Task 2 — v-for без key

**Problem:** Список todo теряет состояние редактирования после reorder.

```vue
<li v-for="(todo, index) in todos" :key="index">
  <input v-model="todo.text" />
</li>
```

**Answer:** Ключи по индексу ломаются при reorder — Vue переиспользует неверные DOM-узлы. Используйте стабильный id:

```vue
<li v-for="todo in todos" :key="todo.id">
  <input v-model="todo.text" />
</li>
```

**See also:** [Keys](./4_vue_core.md#keys)

</details>

Task 3 — watch without getter on prop

<details>
  <summary>Task 3 — watch без getter на prop</summary>

### Task 3 — watch без getter на prop

**Problem:** Watcher не срабатывает при изменении prop (Vue 3.5+).

```vue
<script setup>
const { userId } = defineProps<{ userId: string }>()
watch(userId, fetchUser) // compile warning / wrong
</script>
```

**Answer:** Передайте геттер, чтобы `watch` отслеживал reactive-источник:

```ts
watch(() => userId, fetchUser, { immediate: true });
```

Или следите за `() => props.userId`, если props не деструктурированы.

**See also:** [Reactive props destructure](./4_vue_ssr.md#reactive-props-destructure)

</details>

Task 4 — Child v-model implementation

<details>
  <summary>Task 4 — Child v-model implementation</summary>

### Task 4 — Child v-model implementation

**Problem:** Реализуйте переиспользуемый `SearchInput` с `v-model` через `defineModel`.

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

Родитель: `<SearchInput v-model="query" />`

**See also:** [defineModel](./4_vue_composition.md#definemodel) · [v-model](./4_vue_core.md#v-model)

</details>

Task 5 — Fetch on route param change

<details>
  <summary>Task 5 — Fetch при изменении route param</summary>

### Task 5 — Fetch при изменении route param

**Problem:** Загружайте пользователя при смене `/users/:id` без remount родителя.

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

Альтернатива: `:key="route.params.id"` на дочернем компоненте для принудительного remount.

**See also:** [Vue Router](./4_vue_ecosystem.md#vue-router) · [watch](./4_vue_core.md#watch-watcheffect)

</details>

## Sources

<details>
  <summary>Источники</summary>

**Definition:** Внешние списки вопросов для интервью, флешкарты и практические материалы по Vue.js.

**Sources:**

- [ilavista.by — вопросы на собеседовании по Vue.js](https://ilavista.by/blog/voprosy-na-sobesedovanii-vuejs)
- [GitHub — 300 вопросов по Vue.js (RU)](https://github.com/magisters-cc/vuejs-interview-questions-russian)
- [itanddigital.ru — 50 основных вопросов по Vue.js](https://itanddigital.ru/bloghrconsulting/tpost/o9gce6b1b1-50-osnovnih-voprosov-i-otvetov-na-sobese)
- [itflashcards.com — флешкарты Vue.js](https://www.itflashcards.com/vuejs)
- [zerocoder.ru — практические вопросы для собеседования по Vue.js](https://ya.zerocoder.ru/pgt-prakticheskie-voprosy-dlya-sobesedovaniya-po-vue-js-s-otvetami/)
- [YouTube — собеседование по Vue.js (видео)](https://www.youtube.com/watch?v=umHjpWydn8I&t=1s)
- [Medium — Practical Vue.js interview questions](https://medium.com/front-end-weekly/practical-vue-js-interview-questions-2252b7050b9d)

</details>
