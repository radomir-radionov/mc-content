# Vue — Экосистема

Router, Pinia, Vite, производительность и миграция.

[← Вернуться в центр](./4_vue.md)

### Related docs

- [Vue — Core](./4_vue_core.md) · [Vue — Composition](./4_vue_composition.md) · [Vue — SSR](./4_vue_ssr.md) · [Vue — Экосистема](./4_vue_ecosystem.md) · [Vue — Интервью](./4_vue_interview.md) · [Architecture patterns](./7_special_architecture.md)

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

**Definition:** **Vue Router** — официальный клиентский роутер для Vue SPA. Сопоставляет URL-пути с компонентами, поддерживает nested routes, navigation guards, lazy loading и HTML5 history mode.

**How it works:**

1. Определите route records: `{ path, component, children, meta }`
2. `createRouter({ history: createWebHistory(), routes })`
3. `<router-view />` рендерит matched component; `<router-link>` для навигации
4. **Guards:** `beforeEach`, `beforeEnter` — auth, redirects, data prefetch
5. Lazy routes: `component: () => import('./Page.vue')` — code splitting

**When to use:** Любое multi-view Vue SPA. Для file-based routing и SSR integration используйте [Nuxt routing](./4_nuxt_routing.md).

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

| +                                  | −                                             |
| ---------------------------------- | --------------------------------------------- |
| Официальный, хорошо документирован | Ручной route config vs file-based Nuxt        |
| Navigation guards для auth         | Паттерны data loading различаются по командам |

**Sources:**

[router.vuejs.org](https://router.vuejs.org/)

Router composables

</details>

<details>
  <summary>Router composables</summary>

### Router composables

**Definition:** **vue-router** экспортирует Composition API helpers для реакции на navigation **внутри** компонента — дополняют глобальные `beforeEach` guards.

**How it works:**

| Composable            | Fires when                                                  |
| --------------------- | ----------------------------------------------------------- |
| `onBeforeRouteLeave`  | Компонент собирается покинуть route (unsaved form guard)    |
| `onBeforeRouteUpdate` | Тот же компонент, другие params (`/users/1` → `/users/2`)   |
| `useRoute()`          | Reactive доступ к current route (`params`, `query`, `meta`) |
| `useRouter()`         | Programmatic `push`, `replace`, `back`                      |

Вызывать во время `setup` или `<script setup>` (те же правила, что у lifecycle hooks).

**When to use:** Per-page leave confirmation, refetch при смене `:id`, scroll restoration, привязанный к одному view. Global auth → `beforeEach` в router setup.

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

**Definition:** **Pinia** — официальный Vue state store (преемник Vuex). Stores — функции с `state`, `getters` и `actions`, полная поддержка TypeScript и DevTools.

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
- Без mutations — меняйте `state` напрямую или в actions
- SSR: `createPinia()` на запрос; hydrate state из server payload

**When to use:** Shared state между маршрутами (auth user, cart, UI preferences). Локальный component state остаётся в `ref`/`reactive`.

**Pluses and minuses:**

| +                                | −                                                 |
| -------------------------------- | ------------------------------------------------- |
| Проще Vuex (без mutations)       | Overkill для tiny apps — хватит props/composables |
| Модульные stores, code splitting | Global store может стать god object               |

**See also:** [Pinia vs Vuex](#vuex-legacy)

**Sources:**

[pinia.vuejs.org](https://pinia.vuejs.org/)

Vite

</details>

<details>
  <summary>Vite</summary>

### Vite

**Definition:** **Vite** — официальный build tool для Vue 3: dev server с native ESM HMR и Rollup production builds. `@vitejs/plugin-vue` компилирует SFC.

**How it works:**

- Dev: отдаёт source files по требованию; компилирует `.vue` на запрос
- HMR обновляет блоки SFC независимо (script/template/style)
- Production: tree-shake, code-split, asset hashing
- `import.meta.env` для env variables; `import.meta.glob` для file-based features

**When to use:** Все новые Vue-проекты. Заменяет Vue CLI (webpack).

**Examples:**

```ts
// vite.config.ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";

export default defineConfig({
  plugins: [vue()],
});
```

**Thoughts:** Nuxt построен на Vite + Nitro — см. [Nuxt platform](./4_nuxt_platform.md) для framework config, здесь не дублируем.

**Sources:**

[vitejs.dev](https://vitejs.dev/)

Vue plugins

</details>

<details>
  <summary>Vue plugins</summary>

### Vue plugins

**Definition:** **Plugin** — объект с `install(app, options)` (или функция), регистрируемый через `app.use(plugin, options)`. Plugins добавляют global properties, directives, components или provide/inject keys во всё приложение.

**How it works:**

1. `app.use(i18n)` запускает `install` один раз на app instance
2. Типичные uses: router (`app.use(router)`), Pinia, i18n, telemetry, global `$filters` (legacy)
3. Предпочитайте **composables** для reusable logic без global pollution
4. В `<script setup>` доступ к plugin APIs через imports (`useI18n`), не `this.$t`

**When to use:** Third-party libraries, поставляемые как plugins; app-wide setup (router, pinia). Избегайте plugins для feature logic, которая принадлежит composables.

**Pluses and minuses:**

| +                                | −                                                                     |
| -------------------------------- | --------------------------------------------------------------------- |
| Стандартная точка расширения Vue | Global state на `app.config.globalProperties` вредит TS и testability |
| One-line setup в `main.ts`       | Порядок `app.use` calls может иметь значение (router before mount)    |

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

**Thoughts:** Nuxt оборачивает Vue plugins в `defineNuxtPlugin` — см. [Nuxt plugins](./4_nuxt_platform.md#definenuxtplugin).

**Sources:**

[vuejs.org/guide/reusability/plugins.html](https://vuejs.org/guide/reusability/plugins.html)

v-html & XSS security

</details>

<details>
  <summary>v-html & XSS security</summary>

### v-html & XSS security

**Definition:** `v-html` рендерит raw HTML strings в DOM. **Никогда** не передавайте untrusted user input — это открывает **XSS** (script injection). Vue templates экранируют text interpolations по умолчанию; `v-html` отключает это.

**How it works:**

- `{{ userBio }}` — escaped text
- `<div v-html="userBio" />` — parsed as HTML; `<script>` или `onerror=` могут выполниться
- SSR: serialized state в `__INITIAL_STATE__` должен быть JSON-escaped (Vue/Nuxt обрабатывают это; custom inline scripts — нет)
- Sanitize на сервере, если нужно рендерить rich text (DOMPurify, CMS HTML whitelist)

**When to use:** Trusted CMS content, markdown, отрендеренный в HTML на сервере. Для user comments используйте plain text или sanitizer — не raw `v-html`.

**Pluses and minuses:**

| +                               | −                                        |
| ------------------------------- | ---------------------------------------- |
| Rich content из trusted sources | Одна ошибка → stored XSS                 |
| Проще, чем manual `innerHTML`   | Нет автоматической CSP bypass protection |

**See also:** [XSS in browser security](./6_web_api_security.md#cross-site-scripting-xss) · [SSR context](./4_vue_ssr.md#ssr-context)

**Sources:**

[vuejs.org/guide/best-practices/security.html](https://vuejs.org/guide/best-practices/security.html)

Forms

</details>

<details>
  <summary>Forms</summary>

### Forms

**Definition:** Vue обрабатывает формы через `v-model` на inputs, component `v-model` и библиотеки валидации.

**How it works:**

| Подход           | Заметки                                |
| ---------------- | -------------------------------------- |
| Native `v-model` | `text`, `checkbox`, `select`, `radio`  |
| `defineModel`    | Custom inputs с двусторонней привязкой |
| VeeValidate      | Schema + field-level validation        |
| FormKit          | Form builder components                |

**When to use:** Начните с native `v-model` + computed validation. Добавьте VeeValidate для сложных multi-step форм.

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

**Definition:** Vue-приложения тестируют **Vitest** (unit), **Vue Test Utils** (component) и **Playwright/Cypress** (E2E).

**How it works:**

- `@vue/test-utils` — `mount(Component)`, `getByText`, `trigger`, `setProps`
- Stub child components, mock `provide`, тест emitted events
- Pinia: `createTestingPinia()` для изолированных stores

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

**Definition:** **vue-i18n** предоставляет locale messages, pluralization, datetime/number formats и Composition API (`useI18n`).

**When to use:** Многоязычные SPA. SSR-приложениям нужна locale в server context — у Nuxt модуль `@nuxtjs/i18n`.

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

**Definition:** Выбирайте минимальный контейнер state под scope.

| Scope           | Tool                          |
| --------------- | ----------------------------- |
| Component local | `ref` / `reactive`            |
| Parent ↔ child  | props + emit / `v-model`      |
| Deep tree       | `provide` / `inject`          |
| App-wide client | Pinia                         |
| Server cache    | TanStack Query / custom fetch |
| URL shareable   | Vue Router query params       |

**When to use:** Не кладите всё в Pinia — URL filters в route query; server lists в query cache.

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

**Definition:** Избегайте лишней reactive depth и подписчиков, когда профилирование показывает cost.

**How it works:**

| Technique         | Когда                                         |
| ----------------- | --------------------------------------------- |
| `shallowRef`      | Большие immutable data, заменяемые целиком    |
| `shallowReactive` | One-level object tracking                     |
| `markRaw`         | Third-party class instances в state           |
| `v-once`          | Статическое поддерево без обновлений          |
| `v-memo`          | Пропуск child update, если deps не изменились |

**Examples:**

```vue
<div v-memo="[item.id, item.selected]">
  <HeavyChild :item="item" />
</div>
```

**When to use:** После измерений — не превентивно. Компилятор Vue 3 уже поднимает static nodes.

**Sources:**

[vuejs.org/api/built-in-directives.html#v-memo](https://vuejs.org/api/built-in-directives.html#v-memo)

Async components & code splitting

</details>

<details>
  <summary>Async components & code splitting</summary>

### Async components & code splitting

**Definition:** Делите bundles через dynamic `import()` на маршрутах и тяжёлых компонентах, чтобы начальный JS оставался маленьким.

**How it works:**

- Vue Router: `component: () => import('./views/Admin.vue')`
- `defineAsyncComponent` с loading/error UI
- Vite manual chunks в `build.rollupOptions.output.manualChunks`

**When to use:** Admin panels, charts, editors по требованию.

**See also:** [Async components](./4_vue_composition.md#async-components)

**Sources:**

[vuejs.org/guide/best-practices/performance.html](https://vuejs.org/guide/best-practices/performance.html)

List virtualization

</details>

<details>
  <summary>List virtualization</summary>

### List virtualization

**Definition:** Рендерьте только видимые строки в длинных списках (10k+ items) через windowing libraries.

**How it works:**

- **vue-virtual-scroller** — официальный community pattern
- Fixed или dynamic row heights
- Сочетайте со стабильным `:key` и избегайте пересоздания row components

**When to use:** Таблицы, ленты, log viewers с тысячами элементов.

**Sources:**

[vuejs.org/guide/best-practices/performance.html#virtualize-large-lists](https://vuejs.org/guide/best-practices/performance.html)

Web Vitals in Vue apps

</details>

<details>
  <summary>Web Vitals in Vue apps</summary>

### Web Vitals in Vue apps

**Definition:** Измеряйте **LCP**, **INP**, **CLS** в production Vue apps — как в любом SPA, с Vue-специфичными рычагами.

**How it works:**

| Metric  | Vue lever                                                                                    |
| ------- | -------------------------------------------------------------------------------------------- |
| **LCP** | SSR/SSG (Nuxt) или prerender hero; lazy routes; `fetchpriority="high"` на hero `<img>`       |
| **INP** | Избегайте long sync work в `watch`/`computed`; `v-memo`, `defineAsyncComponent`, lazy routes |
| **CLS** | `width`/`height` на изображениях; слоты для async-баннеров; без вставки DOM над fold         |

- Анализ бандла: `rollup-plugin-visualizer` / Nuxt DevTools
- Предпочитайте SSR/SSG для маркетинга и контента — чистый SPA ухудшает LCP
- Lazy-load route components в Vue Router / Nuxt pages
- Фазы INP и attribution → [6_web_api.md — INP](./1_html_performance.md#inp-interaction-to-next-paint)
- Причины CLS в HTML/CSS → [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift)

**When to use:** Полевой RUM на Nuxt/Vite production builds; Lighthouse CI budgets перед релизом.

**Examples:**

Полевой RUM — регистрируйте один раз при bootstrap (Nuxt plugin или `main.js`). Отправка на `visibilitychange` → `hidden` через `sendBeacon`. Полный паттерн → [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals).

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

**LCP** — lazy route chunks уменьшают первую отрисовку:

```js
// router.js
const routes = [
  { path: "/dashboard", component: () => import("./pages/Dashboard.vue") },
];
```

**INP** — не перерисовывайте неизменённые строки списка:

```vue
<li v-for="item in list" :key="item.id" v-memo="[item.id, item.active]">
  {{ item.name }}
</li>
```

**CLS** — резервируйте место hero до прихода данных:

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

**Thoughts:** _Те же полевые метрики, что у любого SPA — рычаги Vue: SSR, route splitting и стабильный рендер списков. Меряйте production, не dev HMR._

**Sources:**

[web.dev/articles/vitals](https://web.dev/articles/vitals) · [web-vitals](https://github.com/GoogleChrome/web-vitals) · [vuejs.org — performance](https://vuejs.org/guide/best-practices/performance.html)

</details>

---

## 11. Legacy & migration

Options API

<details>
  <summary>Options API</summary>

### Options API

**Definition:** Vue 2-style опции компонента: `data`, `methods`, `computed`, `watch`, lifecycle hooks на объекте компонента. Полностью поддерживается в Vue 3.

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

**When to use:** Поддержка legacy codebases. Новые фичи — Composition API + `<script setup>`.

**Pluses and minuses:**

| +                           | −                                   |
| --------------------------- | ----------------------------------- |
| Знакомо разработчикам Vue 2 | Логика разбита по типу опции        |
| Нет `.value` для data       | Хуже TS inference, чем script setup |

**Sources:**

[vuejs.org/guide/introduction.html#api-styles](https://vuejs.org/guide/introduction.html#api-styles)

Vuex (legacy)

</details>

<details>
  <summary>Vuex (legacy)</summary>

### Vuex (legacy)

**Definition:** **Vuex 4** работает с Vue 3, но в maintenance mode. **Pinia** — официальная замена: без mutations, лучше TS, меньше API.

**How it works:**

- Vuex: `state`, `mutations` (sync), `actions` (async), `getters`, modules
- Pinia: прямая mutation state в actions, проще определение store

**When to use:** Мигрируйте Vuex → Pinia постепенно (store-by-store). Новые проекты: только Pinia.

**Sources:**

[pinia.vuejs.org/cookbook/migration-vuex.html](https://pinia.vuejs.org/cookbook/migration-vuex.html)

Vue 2 → 3 migration

</details>

<details>
  <summary>Vue 2 → 3 migration</summary>

### Vue 2 → 3 migration

**Definition:** Миграция с Vue 2 EOL на Vue 3 включает breaking changes в global API, реактивности, filters, events и обновление сторонних библиотек.

**How it works:**

| Breaking change               | Действие миграции                  |
| ----------------------------- | ---------------------------------- |
| `new Vue()`                   | `createApp()`                      |
| `Vue.use` / global APIs       | `app.use()` на инстанс приложения  |
| Filters removed               | computed или methods               |
| `$on` / `$off` / `$once`      | external mitt или Pinia            |
| `v-model` на components       | `modelValue` + `update:modelValue` |
| Functional / async components | new APIs                           |
| Vue Router 3 → 4              | new `createRouter` API             |
| Vuex → Pinia                  | rewrite stores                     |

**Migration build:** `@vue/compat` помечает Vue 2 patterns runtime warnings, пока вы исправляете код постепенно.

**When to use:** Любое оставшееся Vue 2 app. Спланируйте audit зависимостей (UI libs, test utils) до старта.

**Sources:**

[v3-migration.vuejs.org](https://v3-migration.vuejs.org/)

Migration build (@vue/compat)

</details>

<details>
  <summary>Migration build (@vue/compat)</summary>

### Migration build (@vue/compat)

**Definition:** `@vue/compat` — сборка Vue 3 с режимом совместимости Vue 2: логирует deprecation warnings и поддерживает постепенную миграцию.

**How it works:**

1. Замените `vue` → `@vue/compat` в package.json
2. Настройте compat flags в `vite.config` / webpack alias
3. Исправляйте warnings файл за файлом
4. Уберите compat, когда чисто

**When to use:** Большие Vue 2 codebases без big-bang migrate.

**Sources:**

[v3-migration.vuejs.org/migration-build.html](https://v3-migration.vuejs.org/migration-build.html)

Vue CLI (legacy)

</details>

<details>
  <summary>Vue CLI (legacy)</summary>

### Vue CLI (legacy)

**Definition:** **Vue CLI** (webpack-based) создавал Vue 2/3 проекты через `vue create`. Официальная рекомендация сейчас — **create-vue** (Vite template).

**When to use:** Поддерживайте существующие Vue CLI проекты. Новые: `npm create vue@latest`.

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

**Definition:** Практический порядок production migration Vue 2 → 3.

**How it works:**

1. Обновите tooling до Vue 3 + Vite (или compat build на webpack сначала)
2. Замените global API (`createApp`, `app.component`)
3. Обновите Vue Router 4, Pinia вместо Vuex
4. Исправьте `v-model` / emit renames на shared components
5. Удалите filters, `$listeners` сливается в `$attrs`
6. Обновите тесты (`@vue/test-utils` v2)
7. Audit UI libraries (Vuetify 3, Element Plus и т.д.)
8. Включайте SSR только после стабильного client app — или мигрируйте на [Nuxt](./4_nuxt_core.md)

**Thoughts:** Запускайте `npm run build` и чините compat warnings в CI. Не смешивайте Options и Composition в одном компоненте без причины.

**Sources:**

[v3-migration.vuejs.org/migration-strategy.html](https://v3-migration.vuejs.org/migration-strategy.html)

</details>
