# Nuxt — Routing

Файловые маршруты, layouts, middleware, навигация, metadata и SEO.

[← Вернуться в hub](./4_nuxt.md)

### Related docs

- [Nuxt — Ядро](./4_nuxt_core.md) · [Nuxt — Данные](./4_nuxt_data.md) · [Nuxt — Маршрутизация](./4_nuxt_routing.md) · [Nuxt — Платформа](./4_nuxt_platform.md) · [Nuxt — Интервью](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [10. File-based routing](#10-file-based-routing)
- [11. Special files](#11-special-files)
- [12. Navigation](#12-navigation)
- [13. Metadata & SEO](#13-metadata-seo)

---

<a name="10-file-based-routing"></a>

## 10. File-based routing

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Pages directory conventions</summary>

### Pages directory conventions

**Definition:** Каждый Vue-файл в `app/pages/` становится **vue-router** маршрутом, выведенным из пути. Nuxt генерирует route table на этапе сборки — без ручного массива `routes`.

**How it works:** Dynamic segments используют bracket syntax; nested folders создают nested paths. Code-splitting загружает только chunk для запрошенного маршрута.

**When to use:** Default для всего URL-driven UI. Override через `pages:extend` hook только для edge cases.

**Pluses and minuses:**

| + | − |
|---|---|
| URL structure видна в filesystem | Переименование файлов меняет public URLs |
| Automatic lazy imports per page | Deep nesting может стать verbose |
| Type-safe `useRoute()` params | Catch-all routes требуют careful validation |

**Examples:**

```bash
app/pages/
├── index.vue          → /
├── about.vue          → /about
├── posts/
│   ├── index.vue      → /posts
│   └── [id].vue       → /posts/:id
└── [...slug].vue      → catch-all
```

```vue
<!-- app/pages/posts/[id].vue -->
<script setup lang="ts">
const route = useRoute()
// /posts/42 → route.params.id === '42'
</script>

<template>
  <h1>Post {{ route.params.id }}</h1>
</template>
```

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>Dynamic and catch-all routes</summary>

### Dynamic and catch-all routes

**Definition:** Bracket filenames мапятся на vue-router dynamic segments.

| File | URL | `route.params` |
|------|-----|----------------|
| `[id].vue` | `/posts/42` | `{ id: '42' }` |
| `[...slug].vue` | `/docs/a/b` | `{ slug: ['a', 'b'] }` |
| `[[...slug]].vue` | `/shop` or `/shop/a` | optional catch-all |

**Examples:**

```vue
<script setup lang="ts">
definePageMeta({
  validate(route) {
    return typeof route.params.id === 'string' && /^\d+$/.test(route.params.id)
  },
})
</script>
```

**Thoughts:** `validate` в `definePageMeta` возвращает `false` для 404 — предпочтительнее anonymous middleware для param shape checks.

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>Nested routes and parent pages</summary>

### Nested routes and parent pages

**Definition:** Vue-файл рядом с папкой с тем же именем создаёт **parent route** с child `<NuxtPage />` outlet — похоже на vue-router nested routes.

**Examples:**

```bash
app/pages/
└── parent/
    ├── home.vue       → /parent/home
    ├── child.vue      → /parent/child
    └── parent.vue     # layout wrapper with <NuxtPage />
```

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<a name="11-special-files"></a>

## 11. Special files

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Layouts</summary>

### Layouts

**Definition:** Переиспользуемые UI shells в `app/layouts/`. Pages рендерятся внутри layout `<slot />`. Включается через `<NuxtLayout>` в `app.vue`.

**How it works:** Default layout — `default.vue`. Задаётся per page через `definePageMeta({ layout: 'admin' })` или динамически через `setPageLayout('admin')`. Имена нормализуются в kebab-case.

**When to use:** Shared nav/footer, auth vs marketing shells, dashboard chrome.

**Pluses and minuses:**

| + | − |
|---|---|
| Избегаете дублирования chrome per page | Layout root должен быть single element (не `<slot>` alone) |
| Async-loaded when used | Transitions требуют stable root element |
| `routeRules.appLayout` для config-driven layouts (v4.3+) | |

**Examples:**

```vue
<!-- app/layouts/default.vue -->
<template>
  <div>
    <AppHeader />
    <slot />
    <AppFooter />
  </div>
</template>
```

```vue
<script setup lang="ts">
definePageMeta({ layout: 'admin' })
</script>
```

**Sources:** [nuxt.com/docs/4.x/guide/directory-structure/layouts](https://nuxt.com/docs/4.x/guide/directory-structure/layouts)

</details>

<details>
  <summary>Route middleware</summary>

### Route middleware

**Definition:** Код, выполняемый **перед route navigation** в Vue app — auth guards, redirects, analytics. **Не** то же самое, что `server/middleware/`.

**How it works:** Три вида:

1. **Named** — `app/middleware/auth.ts`, referenced в `definePageMeta({ middleware: 'auth' })`
2. **Global** — `app/middleware/auth.global.ts` на каждой навигации
3. **Anonymous** — inline function в `definePageMeta`

**When to use:** Client-side auth checks, role gates, locale redirects.

**Pluses and minuses:**

| + | − |
|---|---|
| Работает и на client navigations | Не защищает `server/api` сам по себе |
| `navigateTo()` для redirects | Не выполняется для pure API requests |

**Examples:**

```ts
// app/middleware/auth.ts
export default defineNuxtRouteMiddleware(() => {
  const user = useUser()
  if (!user.value) {
    return navigateTo('/login')
  }
})
```

```vue
<script setup lang="ts">
definePageMeta({ middleware: 'auth' })
</script>
```

**Thoughts:** Комбинируйте с server-side session checks в `server/api` для реальной безопасности. См. [Nuxt — Data §9](./4_nuxt_data.md#9-server-routes).

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>error.vue and loading states</summary>

### error.vue and loading states

**Definition:** `app/error.vue` — глобальная error page для fatal render errors и `createError` throws. Loading UX использует `status` из data composables, `<NuxtLoadingIndicator>`, или route-level `lazy` fetches.

**When to use:** Branded 404/500 pages; granular loading через `useLazyFetch` + `status`.

**Examples:**

```vue
<!-- app/error.vue -->
<script setup lang="ts">
const error = useError()
</script>

<template>
  <div>
    <h1>{{ error?.statusCode }}</h1>
    <p>{{ error?.message }}</p>
    <button @click="clearError({ redirect: '/' })">Go home</button>
  </div>
</template>
```

**Sources:** [nuxt.com/docs/4.x/getting-started/error-handling](https://nuxt.com/docs/4.x/getting-started/error-handling)

</details>

<details>
  <summary>definePageMeta</summary>

### definePageMeta

**Definition:** Compile-time macro для metadata страницы — layout, middleware, `validate`, static `title`, route rules (experimental).

**Examples:**

```vue
<script setup lang="ts">
definePageMeta({
  layout: 'dashboard',
  middleware: ['auth', 'analytics'],
  title: 'Dashboard',
})
</script>
```

**Thoughts:** `title` здесь build-time static — dynamic titles используют `useHead` / `useSeoMeta`.

**Sources:** [nuxt.com/docs/4.x/api/utils/define-page-meta](https://nuxt.com/docs/4.x/api/utils/define-page-meta)

</details>

<a name="12-navigation"></a>

## 12. Navigation

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>NuxtLink</summary>

### NuxtLink

**Definition:** Declarative navigation component, оборачивающий `<a>`. Client-side navigations обновляют URL без full reload; prefetch linked pages, когда ссылки входят в viewport.

**How it works:** После гидрации vue-router обрабатывает transitions. Nuxt prefetch route components и payload для быстрых subsequent navigations.

**When to use:** Все internal links — предпочитайте над raw `<a href>` для in-app routes.

**Pluses and minuses:**

| + | − |
|---|---|
| Prefetch + payload reuse | External URLs всё ещё нуждаются в normal `<a>` |
| Active class helpers | |
| Работает с `baseURL` deployments | |

**Examples:**

```vue
<template>
  <nav>
    <NuxtLink to="/">Home</NuxtLink>
    <NuxtLink to="/posts/1">Post 1</NuxtLink>
    <NuxtLink :to="{ name: 'posts-id', params: { id: '2' } }">Post 2</NuxtLink>
  </nav>
</template>
```

**Sources:** [nuxt.com/docs/4.x/api/components/nuxt-link](https://nuxt.com/docs/4.x/api/components/nuxt-link)

</details>

<details>
  <summary>navigateTo and programmatic routing</summary>

### navigateTo and programmatic routing

**Definition:** `navigateTo(to)` выполняет imperative navigation — используется в middleware, event handlers и после form success.

**Examples:**

```ts
await navigateTo('/dashboard')
await navigateTo({ path: '/search', query: { q: 'nuxt' } })
return navigateTo('/login') // in middleware — aborts current navigation
```

```ts
const router = useRouter()
router.push('/about')
```

**Thoughts:** В middleware **return** результат `navigateTo`, чтобы корректно abort navigation.

**Sources:** [nuxt.com/docs/4.x/api/utils/navigate-to](https://nuxt.com/docs/4.x/api/utils/navigate-to)

</details>

<details>
  <summary>useRoute and useRouter</summary>

### useRoute and useRouter

**Definition:** Composables для current route state и programmatic navigation — re-exported vue-router APIs с Nuxt SSR awareness.

| Composable | Use |
|------------|-----|
| `useRoute()` | Read `params`, `query`, `meta`, `path` |
| `useRouter()` | `push`, `replace`, `back` |

**Examples:**

```vue
<script setup lang="ts">
const route = useRoute()
const router = useRouter()

const page = computed(() => Number(route.query.page ?? 1))

function goNext() {
  router.push({ query: { ...route.query, page: page.value + 1 } })
}
</script>
```

**Sources:** [nuxt.com/docs/4.x/api/composables/use-route](https://nuxt.com/docs/4.x/api/composables/use-route)

</details>

<a name="13-metadata-seo"></a>

## 13. Metadata & SEO

_Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>useSeoMeta</summary>

### useSeoMeta

**Definition:** Type-safe composable для SEO meta tags — `title`, `description`, Open Graph, Twitter cards. Powered by **Unhead**.

**How it works:** Flat keys мапятся на правильные `name` vs `property` attributes — избегает common `og:` typos.

**When to use:** Каждая public page, которая должна ранжироваться или появляться в social previews.

**Pluses and minuses:**

| + | − |
|---|---|
| Full TypeScript autocomplete | Reactive refs нужно передавать ref, не `.value` в object |
| Dedupes tags intelligently | |
| Pairs with `useHead` for non-SEO tags | |

**Examples:**

```vue
<script setup lang="ts">
useSeoMeta({
  title: 'My Amazing Site',
  description: 'Learn Nuxt with practical examples.',
  ogTitle: 'My Amazing Site',
  ogDescription: 'Learn Nuxt with practical examples.',
  ogImage: 'https://example.com/og.png',
  twitterCard: 'summary_large_image',
})
</script>
```

**Sources:** [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta)

</details>

<details>
  <summary>useHead and app.head</summary>

### useHead and app.head

**Definition:** `useHead` управляет всеми document head tags reactively. `app.head` в `nuxt.config.ts` задаёт **static** defaults (site title fallback, `lang`, favicon).

**When to use:**

- `nuxt.config` — global defaults, которые редко меняются
- `useHead` в `app.vue` — `titleTemplate`, CDN-aware favicon
- `useHead` / `useSeoMeta` в pages — per-route SEO

**Examples:**

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  app: {
    head: {
      title: 'My App',
      htmlAttrs: { lang: 'en' },
      link: [{ rel: 'icon', href: '/favicon.ico' }],
    },
  },
})
```

```vue
<!-- app/app.vue -->
<script setup lang="ts">
useHead({
  titleTemplate: (title) => (title ? `${title} — My App` : 'My App'),
})
</script>
```

**Thoughts:** Nuxt задаёт default `charset` и `viewport` — override в config только при необходимости.

**Sources:** [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta)

</details>

<details>
  <summary>Head components</summary>

### Head components

**Definition:** Template alternatives: `<Head>`, `<Title>`, `<Meta>`, `<Link>`, `<Style>`, `<Script>` — capitalized, чтобы не конфликтовать с native HTML elements.

**When to use:** Команды, предпочитающие head tags в `<template>` вместо composables.

**Examples:**

```vue
<template>
  <div>
    <Head>
      <Title>{{ title }}</Title>
      <Meta name="description" :content="description" />
    </Head>
    <h1>{{ title }}</h1>
  </div>
</template>
```

**Sources:** [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta)

</details>

<details>
  <summary>SEO with SSR and prerender</summary>

### SEO with SSR and prerender

**Definition:** Поисковики получают полный HTML при SSR или prerender — meta tags из `useSeoMeta` в initial response.

**How it works:** CSR-only routes (`server: false` fetches, `ssr: false`) могут отдавать empty shells — плохо для SEO. Prerender crawlers нуждаются в `<a href>` links для discovery pages.

**When to use:** Prerender marketing + SSR blog; избегайте global `ssr: false` для content sites.

**Pluses and minuses:**

| + | − |
|---|---|
| SSR meta в first HTML | Dynamic OG images нуждаются в server route или build step |
| `routeRules.prerender` для static SEO pages | Client-only data → empty preview для bots |

**Thoughts:** Generic crawler behavior и Core Web Vitals → [Web API notes](./6_web_api_security.md). Vue hydration details → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>
