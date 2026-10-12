# Vue — SSR

Возможности Vue 3.5+, серверный рендеринг и гидратация.

[← Вернуться в центр](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Экосистема](./4_vue_ecosystem.md) · [Vue — Интервью](./4_vue_interview.md) · [Nuxt — Core](./4_nuxt_core.md)

### Sections

- [7. Vue 3.5+ features](#7-vue-35-features)
- [8. SSR & hydration](#8-ssr-hydration)

---

## 7. Vue 3.5+ features

Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** Vue 3.5 (2024) стабилизирует **reactive props destructure**, добавляет **`useTemplateRef()`**, улучшает производительность SSR-гидратации и оптимизирует систему реактивности (память, отслеживание массивов).

**How it works:**

| Feature                    | Что изменилось                                      |
| -------------------------- | ------------------------------------------------- |
| Reactive props destructure | `const { foo = 1 } = defineProps()` остаётся tracked |
| `useTemplateRef()`         | Template refs по string-id на runtime             |
| SSR hydration              | Быстрее attribute/prop hydration, меньше mismatch |
| Reactivity                 | ~56% меньше памяти для больших reactive arrays (3.5) |

**When to use:** Обновляйте проекты Vue 3.4+ до 3.5 за DX. Для типичных приложений без breaking changes.

**Thoughts:** Nuxt 3.13+ включает Vue 3.5 — см. [Nuxt platform notes](./4_nuxt_platform.md) для SSR на уровне фреймворка, здесь не дублируем.

**Sources:**

[blog.vuejs.org/posts/vue-3-5](https://blog.vuejs.org/posts/vue-3-5)

Reactive props destructure

</details>

<details>
  <summary>Reactive props destructure</summary>

### Reactive props destructure

**Definition:** В Vue 3.5+ переменные, деструктурированные из `defineProps()`, **реактивны** — компилятор переписывает чтения в `props.foo`.

**How it works:**

- До 3.5: `const { foo } = defineProps()` — `foo` был frozen snapshot
- 3.5+: `watchEffect(() => console.log(foo))` перезапускается при смене prop
- Передача в `watch(foo, …)` всё ещё ошибка — передайте `() => foo` или используйте `toValue` в composables
- Defaults — нативный JS: `const { count = 0 } = defineProps<{ count?: number }>()`

**Examples:**

```vue
<script setup lang="ts">
const { title = 'Untitled' } = defineProps<{ title?: string }>()

watch(() => title, (t) => console.log('title:', t))
</script>
```

**When to use:** Чище props с defaults без `withDefaults`.

**Sources:**

[vuejs.org/guide/components/props.html#reactive-props-destructure](https://vuejs.org/guide/components/props.html#reactive-props-destructure)

useTemplateRef

</details>

<details>
  <summary>useTemplateRef</summary>

### useTemplateRef

**Definition:** API Vue 3.5 для получения template refs по string id. Работает с dynamic `ref` bindings; сочетается с автодополнением Volar.

**How it works:**

- `const el = useTemplateRef<HTMLInputElement>('my-input')` ↔ `<input ref="my-input">`
- Значение `null` до mount; для DOM API используйте `onMounted`
- Заменяет паттерн `ref(null)` с совпадающим именем переменной (до 3.5 всё ещё валидно)

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

**Definition:** `defineModel()` стал рекомендуемой реализацией `v-model` в Vue 3.4+ и default-паттерном в доках 3.5. Заменяет многословный prop + emit.

**How it works:**

- Разворачивается в `modelValue` + `update:modelValue` (или именованный вариант)
- Modifiers через деструктуризацию: `const [model, modifiers] = defineModel()`
- Custom `get`/`set` для transform (trim, capitalize)

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

**Definition:** Vue 3.5 улучшает внутренний scheduler и гидратацию без новых публичных API для большинства приложений. `app.config` остаётся точкой расширения.

**How it works:**

- `app.config.errorHandler` — глобальные ошибки рендера
- `app.config.warnHandler` — кастомный фильтр предупреждений
- `app.config.performance` — perf marks в dev
- `app.config.compilerOptions` — custom elements, whitespace

**When to use:** Централизованный error reporting (Sentry), подавление известных warnings при legacy migration.

**Sources:**

[vuejs.org/api/application.html#app-config](https://vuejs.org/api/application.html#app-config)

</details>

---

## 8. SSR & hydration

SPA vs SSR

<details>
  <summary>SPA vs SSR</summary>

### SPA vs SSR

**Definition:** **SPA** отдаёт пустой HTML + JS bundle; браузер рендерит всё на клиенте. **SSR** рендерит HTML на сервере, чтобы пользователь видел контент до загрузки JS; клиент **гидратирует** для интерактивности.

**How it works:**

| Mode | First paint        | SEO           | Complexity |
| ---- | ------------------ | ------------- | ---------- |
| SPA  | Shell + loading    | Нужен prerender/meta | Low (Vite SPA) |
| SSR  | Полный HTML контент | Лучше для crawlers | Higher (dual env, hydration) |
| SSG  | Pre-built HTML     | Excellent     | Build-time render |

**When to use:** SSR для marketing + app hybrid, authenticated dashboards с SEO-страницами. Pure admin SPA часто остаётся client-only.

**Thoughts:** Для file-based SSR, data fetching и Nitro server routes используйте [Nuxt](./4_nuxt_core.md) — не дублируйте Nuxt API здесь.

**See also:** [Hydration](#hydration) · [Nuxt — Core](./4_nuxt_core.md)

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html](https://vuejs.org/guide/scaling-up/ssr.html)

createSSRApp & renderToString

</details>

<details>
  <summary>createSSRApp & renderToString</summary>

### createSSRApp & renderToString

**Definition:** Server entry использует `createSSRApp` вместо `createApp`. `@vue/server-renderer` предоставляет `renderToString` / `renderToNodeStream` для HTML из VNode tree.

**How it works:**

1. Сервер создаёт app на запрос (или кэширует template)
2. `renderToString(app)` возвращает HTML string + teleport content
3. Клиент вызывает `createSSRApp` + `app.mount('#app')` — **hydration** подключает listeners
4. Vite SSR: отдельные bundles `entry-server.ts` и `entry-client.ts`

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

**When to use:** Кастомный SSR с Vite. Для production SSR stack предпочитайте Nuxt.

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html#rendering-an-app](https://vuejs.org/guide/scaling-up/ssr.html)

Hydration

</details>

<details>
  <summary>Hydration</summary>

### Hydration

**Definition:** **Hydration** — клиентский процесс переиспользования server-rendered DOM, подключения event listeners и согласования client VNode tree с существующим HTML.

**How it works:**

1. Server HTML должен совпадать с тем, что дал бы первый клиентский рендер
2. Mismatch → Vue предупреждает и может **отбросить** server DOM и перерендерить (плохо для perf/UX)
3. Частые причины: `Date.now()`, `Math.random()`, browser-only API в setup, невалидная вложенность HTML
4. `data-allow-mismatch` (3.5+) для намеренных отличий (например timestamps)

**When to use:** Отлаживайте hydration warnings в dev до релиза SSR.

**Pluses and minuses:**

| +                              | −                                    |
| ------------------------------ | ------------------------------------ |
| Быстрый interactive first paint | Строгое server/client parity        |
| SEO-friendly контент в HTML    | Восстановление после mismatch дорого |

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

**Definition:** Per-request данные с server render на клиент через **`payload`** или inlined `window.__INITIAL_STATE__` — избегает refetch при hydration.

**How it works:**

- Server: `renderToString(app, { idPrefix })` или Vite SSR `context`
- Прикрепляйте state к HTML как serialized JSON (осторожно с escape — риск XSS)
- Client: `app.mount` читает payload и инициализирует Pinia / reactive state
- Nuxt: `useState`, `useAsyncData` — см. [Nuxt data](./4_nuxt_data.md)

**When to use:** User session, CMS page data, snapshot корзины на first paint.

**Thoughts:** Никогда не вставляйте `JSON.stringify` user-controlled строк в `<script>` без sanitization.

**Sources:**

[vuejs.org/guide/scaling-up/ssr.html#data-prefetching](https://vuejs.org/guide/scaling-up/ssr.html)

Suspense & streaming

</details>

<details>
  <summary>Suspense & streaming</summary>

### Suspense & streaming

**Definition:** SSR может **стримить** HTML по мере резолва async components — сначала fallbacks, затем resolved сегменты — через границы `<Suspense>`.

**How it works:**

1. Сервер встречает async `setup()` или async component
2. Сразу рендерит HTML слота `#fallback`
3. Когда Promise резолвится, стримит resolved сегмент
4. Клиент гидратирует инкрементально по мере прихода chunks

**When to use:** Медленные API-секции на иначе быстрых страницах. Nuxt и Vite SSR plugins реализуют streaming pipelines.

**See also:** [Suspense](./4_vue_composition.md#suspense)

**Sources:**

[vuejs.org/guide/built-ins/suspense.html#ssr-usage](https://vuejs.org/guide/built-ins/suspense.html)

Client-only components

</details>

<details>
  <summary>Client-only components</summary>

### Client-only components

**Definition:** Компоненты или поддеревья, которые **не должны** рендериться на сервере (browser APIs, charts, maps). Обёртка рендерит placeholder на сервере, реальный контент на клиенте.

**How it works:**

- Обёртка `<ClientOnly>` в Nuxt — см. [Nuxt platform](./4_nuxt_platform.md)
- Ручной паттерн: guard `import.meta.env.SSR` в `setup`
- Рендер только в `onMounted` избегает hydration mismatch

**When to use:** `window`, `localStorage`, WebGL, сторонние виджеты без SSR.

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
