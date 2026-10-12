# Nuxt — Routing

File-based routes, layouts, middleware, navigation, metadata, and SEO.

[← Back to hub](./4_nuxt.md)

### Related docs

- [Nuxt — Core](./4_nuxt_core.md) · [Nuxt — Data](./4_nuxt_data.md) · [Nuxt — Routing](./4_nuxt_routing.md) · [Nuxt — Platform](./4_nuxt_platform.md) · [Nuxt — Interview](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Sections

- [10. File-based routing](#10-file-based-routing)
- [11. Special files](#11-special-files)
- [12. Navigation](#12-navigation)
- [13. Metadata & SEO](#13-metadata-seo)

---

<a name="10-file-based-routing"></a>

## 10. File-based routing

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Pages directory conventions</summary>

### Pages directory conventions

**Definition:** Every Vue file in `app/pages/` becomes a **vue-router** route derived from its path. Nuxt generates the route table at build time — no manual `routes` array.

**How it works:** Dynamic segments use bracket syntax; nested folders create nested paths. Code-splitting loads only the chunk for the requested route.

**When to use:** Default for all URL-driven UI. Override with `pages:extend` hook only for edge cases.

**Pluses and minuses:**

| + | − |
|---|---|
| URL structure visible in filesystem | Renaming files changes public URLs |
| Automatic lazy imports per page | Deep nesting can get verbose |
| Type-safe `useRoute()` params | Catch-all routes need careful validation |

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

**Definition:** Bracket filenames map to vue-router dynamic segments.

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

**Thoughts:** `validate` in `definePageMeta` returns `false` for 404 — prefer over anonymous middleware for param shape checks.

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>Nested routes and parent pages</summary>

### Nested routes and parent pages

**Definition:** A Vue file alongside a folder with the same name creates a **parent route** with child `<NuxtPage />` outlet — similar to vue-router nested routes.

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

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>Layouts</summary>

### Layouts

**Definition:** Reusable UI shells in `app/layouts/`. Pages render inside the layout `<slot />`. Enable with `<NuxtLayout>` in `app.vue`.

**How it works:** Default layout is `default.vue`. Set per page via `definePageMeta({ layout: 'admin' })` or dynamically with `setPageLayout('admin')`. Names normalize to kebab-case.

**When to use:** Shared nav/footer, auth vs marketing shells, dashboard chrome.

**Pluses and minuses:**

| + | − |
|---|---|
| Avoid duplicating chrome per page | Layout root must be single element (not `<slot>` alone) |
| Async-loaded when used | Transitions require stable root element |
| `routeRules.appLayout` for config-driven layouts (v4.3+) | |

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

**Definition:** Code that runs **before route navigation** in the Vue app — auth guards, redirects, analytics. **Not** the same as `server/middleware/`.

**How it works:** Three kinds:

1. **Named** — `app/middleware/auth.ts`, referenced in `definePageMeta({ middleware: 'auth' })`
2. **Global** — `app/middleware/auth.global.ts` runs on every navigation
3. **Anonymous** — inline function in `definePageMeta`

**When to use:** Client-side auth checks, role gates, locale redirects.

**Pluses and minuses:**

| + | − |
|---|---|
| Runs on client navigations too | Does not protect `server/api` alone |
| `navigateTo()` for redirects | Not executed for pure API requests |

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

**Thoughts:** Combine with server-side session checks in `server/api` for real security. See [Nuxt — Data §9](./4_nuxt_data.md#9-server-routes).

**Sources:** [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing)

</details>

<details>
  <summary>error.vue and loading states</summary>

### error.vue and loading states

**Definition:** `app/error.vue` is the global error page for fatal render errors and `createError` throws. Loading UX uses `status` from data composables, `<NuxtLoadingIndicator>`, or route-level `lazy` fetches.

**When to use:** Branded 404/500 pages; granular loading via `useLazyFetch` + `status`.

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

**Definition:** Compile-time macro to attach metadata to a page — layout, middleware, `validate`, static `title`, route rules (experimental).

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

**Thoughts:** `title` here is build-time static — dynamic titles use `useHead` / `useSeoMeta`.

**Sources:** [nuxt.com/docs/4.x/api/utils/define-page-meta](https://nuxt.com/docs/4.x/api/utils/define-page-meta)

</details>

<a name="12-navigation"></a>

## 12. Navigation

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>NuxtLink</summary>

### NuxtLink

**Definition:** Declarative navigation component wrapping `<a>`. Client-side navigations update URL without full reload; prefetches linked pages when links enter the viewport.

**How it works:** After hydration, vue-router handles transitions. Nuxt prefetches route components and payload for faster subsequent navigations.

**When to use:** All internal links — prefer over raw `<a href>` for in-app routes.

**Pluses and minuses:**

| + | − |
|---|---|
| Prefetch + payload reuse | External URLs still need normal `<a>` |
| Active class helpers | |
| Works with `baseURL` deployments | |

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

**Definition:** `navigateTo(to)` performs imperative navigation — used in middleware, event handlers, and after form success.

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

**Thoughts:** In middleware, **return** the result of `navigateTo` to abort navigation properly.

**Sources:** [nuxt.com/docs/4.x/api/utils/navigate-to](https://nuxt.com/docs/4.x/api/utils/navigate-to)

</details>

<details>
  <summary>useRoute and useRouter</summary>

### useRoute and useRouter

**Definition:** Composables for current route state and programmatic navigation — re-exported vue-router APIs with Nuxt SSR awareness.

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

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template))._

<details>
  <summary>useSeoMeta</summary>

### useSeoMeta

**Definition:** Type-safe composable for SEO meta tags — `title`, `description`, Open Graph, Twitter cards. Powered by **Unhead**.

**How it works:** Flat keys map to correct `name` vs `property` attributes — avoids common `og:` typos.

**When to use:** Every public page that should rank or appear in social previews.

**Pluses and minuses:**

| + | − |
|---|---|
| Full TypeScript autocomplete | Reactive refs need passing ref, not `.value` in object |
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

**Definition:** `useHead` manages all document head tags reactively. `app.head` in `nuxt.config.ts` sets **static** defaults (site title fallback, `lang`, favicon).

**When to use:**

- `nuxt.config` — global defaults that rarely change
- `useHead` in `app.vue` — `titleTemplate`, CDN-aware favicon
- `useHead` / `useSeoMeta` in pages — per-route SEO

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

**Thoughts:** Nuxt sets default `charset` and `viewport` — override in config only when needed.

**Sources:** [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta)

</details>

<details>
  <summary>Head components</summary>

### Head components

**Definition:** Template alternatives: `<Head>`, `<Title>`, `<Meta>`, `<Link>`, `<Style>`, `<Script>` — capitalized to avoid clashing with native HTML elements.

**When to use:** Teams that prefer head tags in `<template>` over composables.

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

**Definition:** Search engines receive full HTML when SSR or prerender is enabled — meta tags from `useSeoMeta` are in the initial response.

**How it works:** CSR-only routes (`server: false` fetches, `ssr: false`) may ship empty shells — bad for SEO. Prerender crawlers need `<a href>` links to discover pages.

**When to use:** Prerender marketing + SSR blog; avoid global `ssr: false` for content sites.

**Pluses and minuses:**

| + | − |
|---|---|
| SSR meta in first HTML | Dynamic OG images need server route or build step |
| `routeRules.prerender` for static SEO pages | Client-only data → empty preview for bots |

**Thoughts:** Generic crawler behavior and Core Web Vitals → [Web API notes](./6_web_api_security.md). Vue hydration details → [Vue — SSR](./4_vue_ssr.md).

**Sources:** [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering)

</details>
