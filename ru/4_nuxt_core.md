# Nuxt — Core

Модель фреймворка, стратегии рендеринга и принципы проектирования.

[← Вернуться в hub](./4_nuxt.md)

### Related docs

- [Nuxt — Ядро](./4_nuxt_core.md) · [Nuxt — Данные](./4_nuxt_data.md) · [Nuxt — Маршрутизация](./4_nuxt_routing.md) · [Nuxt — Платформа](./4_nuxt_platform.md) · [Nuxt — Интервью](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [1. Framework overview](#1-framework-overview)
- [2. Nuxt vs Vue SPA](#2-nuxt-vs-vue-spa)
- [3. Project structure](#3-project-structure)
- [4. Rendering strategies](#4-rendering-strategies)
- [5. Server & client boundaries](#5-server-client-boundaries)
- [6. Streaming & Suspense](#6-streaming-suspense)
- [7. Design principles](#7-design-principles)

---

<a name="topic-template"></a>

### Topic template

Каждая крупная тема в секциях `<details>` следует шаблону:

**Definition** → **How it works** → **When to use** → **Pluses and minuses** → **Examples** → **Thoughts** → **Sources**

---

<a name="1-framework-overview"></a>

## 1. Framework overview

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Nuxt overview</summary>

### Nuxt overview

**Definition:** Nuxt — это **фреймворк Vue** для создания full-stack веб-приложений и сайтов. Вы пишете `.vue` файлы и composables; Nuxt добавляет файловую маршрутизацию, SSR по умолчанию, auto-imports, утилиты загрузки данных и сервер Nitro — сборщики и конфиг TypeScript уже включены.

**When to use:**

- Маркетинговые сайты, блоги и документация с требованиями SEO
- E-commerce, дашборды и SaaS с аутентифицированными и публичными страницами
- BFF (Backend for Frontend) — API-маршруты рядом с UI через `server/api/`
- Команды, которым нужен универсальный деплой (Node, serverless, edge, static) из одной кодовой базы
- Vue-команды, желающие conventions вместо ручной настройки Vite + Vue Router

**When not to use:**

- Чисто статические сайты с минимальным JS — рассмотрите **Astro**
- Мобильные приложения — **Expo / React Native** или нативные стеки
- Тяжёлый кастомный long-lived WebSocket hub — отдельный backend + Vite SPA могут подойти лучше
- Команды, привязанные к React — используйте **Next.js**

**Pluses and minuses:**

| + | − |
|---|---|
| Батарейки в комплекте: маршрутизация, SSR, API, auto-imports | Мнение: структура каталогов |
| Универсальный деплой Nitro (15+ presets) | Кривая обучения для payload, кэширования и гибридных `routeRules` |
| TypeScript и Vite HMR из коробки | Меньший hiring pool, чем React/Next на некоторых рынках |
| SEO-friendly SSR/SSG из коробки | Cold starts на serverless; настройка под preset |
| Сильная экосистема Vue (Pinia, VueUse modules) | Мажорные апгрейды (2→3) требуют миграции |

**Examples:**

```bash
npm create nuxt@latest my-app
cd my-app && npm run dev
```

```vue
<!-- app/pages/index.vue -->
<script setup lang="ts">
const { data: posts } = await useFetch('/api/posts')
</script>

<template>
  <main>
    <h1>Posts</h1>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </main>
</template>
```

**Thoughts:** Nuxt **не** заменяет знание Vue — это слой поверх. Формулировка для интервью: Vue = UI-библиотека; Nuxt = фреймворк (маршрутизация, рендеринг, данные, деплой). Новые проекты должны использовать **Nuxt 3/4** (пакет `nuxt`). Nuxt 2 — legacy.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>Vue vs Nuxt</summary>

### Vue vs Nuxt

**Definition:**

Vue рендерит UI; Nuxt оркестрирует, как и где этот UI строится, кэшируется и доставляется.


| | Vue (Vite SPA) | Nuxt |
| | --- | --- |
| | **Role** | UI library | Full-stack framework |
| | **Routing** | Vue Router (ручная настройка) | File-based `app/pages/` |
| | **Data fetching** | Client hooks / ручной SSR | `useFetch`, `useAsyncData`, payload |
| | **Rendering** | CSR по умолчанию | SSR, SSG, hybrid через `routeRules` |
| | **API** | Отдельный backend | Nitro routes `server/api/` |
| | **Deployment** | Static CDN или custom | Nitro presets (Node, Vercel, Cloudflare, static) |

**Thoughts:** Для общего Vue SSR, гидрации и механики реактивности см. [Vue — SSR](./4_vue_ssr.md). Этот документ охватывает **Nuxt-специфичные** API и семантику Nitro.

**Sources:** [vuejs.org](https://vuejs.org/) · [nuxt.com/docs](https://nuxt.com/docs)

</details>

<details>
  <summary>Nuxt vs alternatives</summary>

### Nuxt vs alternatives

**Definition:**

Сравнение Nuxt с другими full-stack и meta-framework решениями.

| Framework | Strength | Trade-off |
|-----------|----------|-----------|
| **Vite + Vue Router** | Максимальная гибкость, SPA-first | Ручной SSR, нет встроенного API-слоя |
| **Next.js** | Экосистема React, RSC, Vercel | Другая UI-парадигма (React) |
| **SvelteKit** | Производительность, простота | Меньшая Vue-aligned экосистема |
| **Remix** | Web standards, loaders/actions | Только React |
| **Astro** | Контентные сайты, islands | Менее подходит для тяжёлой клиентской интерактивности |

**When to use Nuxt:** SEO-чувствительные Vue-приложения, универсальный деплой, стандартизация команды на Vue 3 Composition API.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>History (compressed)</summary>

### History (compressed)

**Definition:**

Хронология основных релизов Nuxt и архитектурных вех.

| Version | Milestone |
|---------|-----------|
| **2016** | Nuxt 1 — Vue 2, webpack, universal mode |
| **v2** | Улучшенный CLI, экосистема модулей, static `generate` |
| **v3** | Vue 3, Vite, Nitro server, каталог `app/`, Composition API first |
| **v4** | `app/` как default `srcDir`; `shared/` для isomorphic utils/types; более чистый upgrade path; docs aligned с [nuxt.com](https://nuxt.com/) |
| **Nitro** | Standalone server engine (UnJS); питает Nuxt server и deploy presets |

**Sources:** [nuxt.com/blog](https://nuxt.com/blog)

</details>

<a name="2-nuxt-vs-vue-spa"></a>

## 2. Nuxt vs Vue SPA

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>When to choose Nuxt over Vite SPA</summary>

### When to choose Nuxt over Vite SPA

**Definition:** Vite + Vue SPA отдаёт HTML-оболочку + клиентский бандл; Nuxt может SSR/SSG те же Vue-компоненты и коллоцировать серверную логику.

**How it works:** Nuxt запускает ваш `setup()` на сервере для начального запроса, сериализует состояние в **payload** и гидратирует на клиенте. Обычный SPA загружает данные только после загрузки JS.

**When to use Nuxt:** SEO, быстрый first paint, общие auth cookies на сервере, API-маршруты без второго репозитория.

**When to use Vite SPA:** Внутренние admin-инструменты, встраиваемые виджеты, максимальная клиентская простота.

**Pluses and minuses:**

| + (Nuxt) | − (Nuxt) |
|----------|----------|
| First meaningful paint с данными | Больше движущихся частей, чем SPA |
| `server/api` без Express boilerplate | SSR-баги (hydration mismatch) возможны |
| Гибридный per-route рендеринг | Концепции build/deploy нужно изучить |

**Thoughts:** SSR можно отключить глобально (`ssr: false`) и всё равно использовать Nuxt для маршрутизации, модулей и auto-imports — но вы теряете большую часть SEO-преимуществ.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<a name="3-project-structure"></a>

## 3. Project structure

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Directory layout</summary>

### Directory layout

**Definition:** Nuxt 4 использует **`app/` как default `srcDir`** — pages, layouts, middleware и `app.vue` живут под `app/`. **`shared/`** в project root хранит isomorphic code (types, utils), importable из `app/` и `server/`. `nuxt.config.ts` может задать `srcDir: '.'` для Nuxt 3-style flat layout.

**When to use:** Следуйте Nuxt 4 defaults для новых проектов. Используйте `shared/` для кода, нужного UI и Nitro handlers. Monorepos или design systems могут использовать `layers/` или custom `srcDir`.

**Pluses and minuses:**

| + | − |
|---|---|
| Предсказуемый onboarding | Глубокие `pages/` деревья буквально отражают URL |
| Коллокация pages, layouts, middleware | Перемещение файлов меняет маршруты |
| `server/` для API отдельно от UI | Два понятия middleware (route vs server) |

**Examples:**

```bash
my-nuxt-app/
├── app/
│   ├── app.vue              # root shell
│   ├── pages/               # file-based routes
│   │   ├── index.vue
│   │   └── posts/[id].vue
│   ├── layouts/
│   │   └── default.vue
│   ├── components/          # auto-imported
│   └── middleware/          # route middleware
├── server/
│   ├── api/                 # Nitro API routes
│   └── middleware/          # server middleware
├── composables/             # auto-imported (root or app/)
├── shared/                  # isomorphic types/utils (Nuxt 4)
│   └── types/
├── plugins/                 # or app/plugins/ — defineNuxtPlugin
├── nuxt.config.ts
└── package.json
```

**Thoughts:** `server/` и `shared/` остаются в project root; только UI-facing dirs по умолчанию внутри `app/`. См. [defineNuxtPlugin](./4_nuxt_platform.md#definenuxtplugin).

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure](https://nuxt.com/docs/4.x/guide/directory-structure) · [nuxt.com/docs/4.x/getting-started/upgrade](https://nuxt.com/docs/4.x/getting-started/upgrade)

</details>

<details>
  <summary>Auto-imports</summary>

### Auto-imports

**Definition:** Nuxt auto-imports Vue API, composables из `composables/`, компоненты из `components/` и утилиты из `utils/` — ручные import statements не нужны. Tree-shaking удаляет неиспользуемые экспорты из клиентского бандла.

**How it works:** Nuxt сканирует настроенные каталоги на этапе сборки и генерирует type stubs в `.nuxt/imports.d.ts`. Компоненты используют path-based naming (`BaseButton.vue` → `<BaseButton />`).

**When to use:** По умолчанию везде; отключайте per-component явными imports только при избежании коллизий имён.

**Pluses and minuses:**

| + | − |
|---|---|
| Меньше boilerplate, быстрее feature work | Magic — новички могут не знать, откуда символы |
| Type-safe с generated types | Коллизии имён в nested `components/` |
| HMR для composables и компонентов | Custom dirs нуждаются в `imports.dirs` config |

**Examples:**

```vue
<!-- No imports needed -->
<script setup lang="ts">
const count = ref(0)
const { data } = await useFetch('/api/count')
</script>

<template>
  <BaseCounter :count="count" />
</template>
```

**Thoughts:** Предпочитайте выносить переиспользуемую логику в `composables/useX.ts` — всё равно auto-imported и тестируемо.

**Sources:** [nuxt.com/docs/4.x/guide/concepts/auto-imports](https://nuxt.com/docs/4.x/guide/concepts/auto-imports)

</details>

<details>
  <summary>app.vue root shell</summary>

### app.vue root shell

**Definition:** `app.vue` — корневой компонент, оборачивающий каждую страницу. Обычно рендерит `<NuxtLayout>` и `<NuxtPage />` — Nuxt-эквиваленты persistent shell плюс routed content.

**Examples:**

```vue
<!-- app/app.vue -->
<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
```

**When to use:** Глобальные providers, `useHead` title templates или приложение с одним layout. Для одного layout некоторые команды инлайнят разметку в `app.vue` вместо `layouts/default.vue`.

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/app](https://nuxt.com/docs/4.x/guide/directory-structure/app)

</details>

<a name="4-rendering-strategies"></a>

## 4. Rendering strategies

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>SSR (default)</summary>

### SSR (default)

**Definition:** Nuxt рендерит Vue-компоненты в HTML **на каждый запрос** (или при первом визите для кэшированных маршрутов). Браузер получает заполненный HTML, затем гидратирует в интерактивное приложение.

**How it works:** Nitro вызывает Vue renderer; `useFetch` / `useAsyncData` выполняются на сервере; результаты встраиваются в HTML payload для гидрации.

**When to use:** Персонализированные страницы, auth-gated контент, всегда свежие данные, SEO с динамическими данными.

**Pluses and minuses:**

| + | − |
|---|---|
| SEO и быстрый first paint | Серверные вычисления на запрос (если не кэшировано) |
| Секреты остаются на сервере | Hydration mismatch, если server/client HTML различается |

**Examples:**

```ts
// nuxt.config.ts — SSR is default (ssr: true)
export default defineNuxtConfig({
  ssr: true,
})
```

**Thoughts:** Механика SSR (гидрация, `onServerPrefetch`) → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>SSG and prerendering</summary>

### SSG and prerendering

**Definition:** Static Site Generation предрендерит маршруты **на этапе сборки** в HTML (+ `_payload.json`), отдаваемый со static hosting. Запускается через `nuxt generate` или `nuxt build --prerender`.

**How it works:** Nitro crawler загружает `/`, сохраняет HTML, следует `<a href>` ссылкам, повторяет до исчерпания. Ручные маршруты через `nitro.prerender.routes` или `routeRules`.

**When to use:** Маркетинг, docs, блоги, каталоги с известными URL.

**Pluses and minuses:**

| + | − |
|---|---|
| CDN-fast, дешёвый hosting | Данные устаревают до rebuild (если нет ISR/SWR) |
| Нет сервера в runtime | Несвязанные dynamic routes не auto-discovered |
| Эмитит `200.html` / `404.html` SPA fallbacks | Crawler находит только anchor links |

**Examples:**

```bash
npx nuxt generate
# deploy .output/public to static host
```

```ts
export default defineNuxtConfig({
  routeRules: {
    '/': { prerender: true },
    '/blog/**': { prerender: true },
  },
})
```

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>

<details>
  <summary>CSR (SPA mode)</summary>

### CSR (SPA mode)

**Definition:** Отключите SSR глобально с `ssr: false`. Nuxt отдаёт пустую оболочку; Vue монтируется и загружает данные только на клиенте.

**When to use:** Внутренние инструменты, auth-walled приложения без SEO, или гибридные приложения с `<ClientOnly>` для отдельных виджетов.

**Pluses and minuses:**

| + | − |
|---|---|
| Проще mental model (без гидрации) | Плохой SEO и медленный first paint |
| Деплой как static SPA | Теряются SSR payload benefits |

**Examples:**

```ts
export default defineNuxtConfig({
  ssr: false,
})
```

**Thoughts:** Предпочитайте гибридные `routeRules` вместо глобального `ssr: false`, когда только часть маршрутов нуждается в CSR.

**Sources:** [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment)

</details>

<details>
  <summary>Hybrid rendering (routeRules)</summary>

### Hybrid rendering (routeRules)

**Definition:** Per-route правила в `nuxt.config.ts` настраивают режим рендеринга, кэширование, редиректы и другое. Nuxt компилирует правила; Nitro применяет server-side поведение.

**How it works:** Правила вроде `prerender`, `ssr`, `cache`, `isr`, `swr`, `redirect` мапятся на Nitro handlers на этапе build или request time.

**When to use:** Статический маркетинг + динамический dashboard в одном приложении; кэширование API; редиректы legacy URL.

**Examples:**

```ts
export default defineNuxtConfig({
  routeRules: {
    '/': { prerender: true },
    '/admin/**': { ssr: true },
    '/api/**': { cache: { maxAge: 3600 } },
    '/old-page': { redirect: { to: '/new-page', statusCode: 301 } },
  },
})
```

**Pluses and minuses:**

| + | − |
|---|---|
| Одна кодовая база, смешанные стратегии | Misconfigured rules → неожиданное cache/stale поведение |
| CDN cache для API routes | Документируйте, какие маршруты static vs dynamic |

**Sources:** [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server)

</details>

<a name="5-server-client-boundaries"></a>

## 5. Server & client boundaries

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Universal vs client-only code</summary>

### Universal vs client-only code

**Definition:** Nuxt запускает Vue `setup()` на **сервере и клиенте**, если не ограничено. Browser API (`window`, `localStorage`) и некоторые lifecycle hooks работают только на клиенте.

**How it works:** Код в `setup` выполняется на сервере для SSR, затем снова на клиенте при гидрации. Side effects должны быть идемпотентны или обёрнуты в `onMounted` / `import.meta.client`.

**When to use server:** Загрузка данных с `useFetch`, чтение cookies через `useRequestHeaders`, Nitro `server/` utilities.

**When to use client:** DOM manipulation, `localStorage`, third-party виджеты без SSR support.

**Pluses and minuses:**

| + | − |
|---|---|
| Одно дерево компонентов для SSR + CSR | Легко сослаться на `window` и сломать SSR |
| `<ClientOnly>` escape hatch | Дублирование логики без composables |

**Examples:**

```vue
<script setup lang="ts">
if (import.meta.client) {
  console.log('Runs only in browser')
}
</script>

<template>
  <ClientOnly>
    <BrowserOnlyChart />
  </ClientOnly>
</template>
```

**Thoughts:** Deep dive по hydration mismatches и `onServerPrefetch` → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>server/api vs app code</summary>

### server/api vs app code

**Definition:** `server/` выполняется **только на Nitro** — никогда не попадает в клиентский бандл. Vue-код `app/` может выполняться на сервере и клиенте.

**When to use `server/api`:** Доступ к БД, секреты, webhooks, REST endpoints для внешних клиентов.

**When to use composables + `useFetch`:** Чтение данных в Vue pages с SSR payload forwarding.

**Examples:**

```ts
// server/api/posts.get.ts
export default defineEventHandler(async () => {
  return await db.post.findMany()
})
```

```vue
<script setup lang="ts">
const { data: posts } = await useFetch('/api/posts')
</script>
```

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server)

</details>

<a name="6-streaming-suspense"></a>

## 6. Streaming & Suspense

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Suspense and navigation blocking</summary>

### Suspense and navigation blocking

**Definition:** Nuxt использует Vue `<Suspense>` под капотом. `useFetch` / `useAsyncData` блокируют навигацию, пока данные не разрешатся (если не `lazy`).

**How it works:** При server render Nuxt ждёт async setup в любом случае перед отправкой HTML. При **client navigation** `await useFetch` держит пользователя на текущей странице, пока данные не готовы (опционально `<NuxtLoadingIndicator>`).

**When to use blocking fetch:** SEO-страницы, которые должны загружаться полностью заполненными.

**When to use `lazy` / `useLazyFetch`:** Dashboard widgets, below-the-fold данные, мгновенная навигация с ручным loading UI.

**Examples:**

```vue
<script setup lang="ts">
// Blocks navigation until resolved (default)
const { data } = await useFetch('/api/posts')

// Non-blocking — handle status yourself
const { data: comments, status } = useLazyFetch('/api/comments')
</script>
```

**Pluses and minuses:**

| + | − |
|---|---|
| Нет half-rendered SEO pages | Медленный API → медленные route transitions |
| `lazy` даёт fine control | `lazy` + `await` на клиенте ведёт себя тонко иначе |

**Sources:** [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching)

</details>

<details>
  <summary>NuxtLoadingIndicator</summary>

### NuxtLoadingIndicator

**Definition:** Встроенный компонент, показывающий progress bar между page navigations, пока загружаются async data и code-split chunks.

**Examples:**

```vue
<!-- app/app.vue -->
<template>
  <NuxtLoadingIndicator />
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
```

**Sources:** [nuxt.com/docs/4.x/api/components/nuxt-loading-indicator](https://nuxt.com/docs/4.x/api/components/nuxt-loading-indicator)

</details>

<a name="7-design-principles"></a>

## 7. Design principles

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Conventions over configuration</summary>

### Conventions over configuration

**Definition:** Nuxt автоматизирует рутинные задачи через directory conventions — маршруты из `pages/`, API из `server/api/`, layouts из `layouts/`. `nuxt.config.ts` кастомизирует, когда defaults недостаточны.

**When to use:** Доверяйте defaults сначала; обращайтесь к config при деплое на edge, разделении layers или настройке `routeRules`.

**Pluses and minuses:**

| + | − |
|---|---|
| Быстрый greenfield setup | Нестандартные архитектуры борются с conventions |
| Modules расширяют без forking | Over-customization снижает benefits фреймворка |

**Thoughts:** Выносите domain logic в `composables/` и `server/utils/` — держите `pages/` тонкими, как Next.js `page.tsx`.

**Sources:** [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction)

</details>

<details>
  <summary>SOLID in Nuxt</summary>

### SOLID in Nuxt

**Definition:** SOLID принципы применяются к **Nuxt pages, composables, server handlers и `server/utils/`** — не только к классам.

| Principle | Nuxt example |
|-----------|--------------|
| **S** — Single Responsibility | `pages/` compose · `composables/useX` fetches · `server/api` mutates |
| **O** — Open/Closed | Добавить payment module — не редактировать checkout handler для каждого provider |
| **L** — Liskov Substitution | Layouts всегда рендерят `<slot />` консистентно |
| **I** — Interface Segregation | Передавать `{ readUser }`, а не весь DB client |
| **D** — Dependency Inversion | `placeOrder(input, store)` — inject repository в тестах |

**Examples:**

```ts
// S — good: thin page, composable owns fetch
// pages/checkout.vue
const { cart } = await useCart()

// composables/useCart.ts
export function useCart() {
  return useFetch('/api/cart')
}
```

```ts
// S — bad: page calls DB, sends email, renders form inline
```

**Thoughts:** См. также [SOLID in React](./4_react_core.md#solid-in-react) для параллельных паттернов.

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure](https://nuxt.com/docs/4.x/guide/directory-structure)

</details>
