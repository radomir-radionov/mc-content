# Nuxt — hub

File-based routing, SSR/SSG, data fetching, Nitro server, and the Nuxt platform. Theory lives in the topic files below.

### Related docs

[Nuxt — Core](./4_nuxt_core.md) · [Nuxt — Data](./4_nuxt_data.md) · [Nuxt — Routing](./4_nuxt_routing.md) · [Nuxt — Platform](./4_nuxt_platform.md) · [Nuxt — Interview](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Navigation

| Topic | Covers |
|------|--------|
| [Nuxt — Core](./4_nuxt_core.md) | Framework model, rendering strategies, and design principles. |
| [Nuxt — Data](./4_nuxt_data.md) | `useFetch`, `useAsyncData`, caching, and server routes. |
| [Nuxt — Routing](./4_nuxt_routing.md) | File-based routes, layouts, middleware, navigation, and SEO. |
| [Nuxt — Platform](./4_nuxt_platform.md) | Nitro, modules, `nuxt.config`, deployment, migration, and debugging. |
| [Nuxt — Interview](./4_nuxt_interview.md) | Interview Q&A and coding tasks. |

### Interview practice

| Topic | File |
|-------|------|
| SSR vs SSG vs CSR vs hybrid | [Interview](./4_nuxt_interview.md#ssr-vs-ssg-vs-csr-vs-hybrid-rendering) |
| `useFetch` vs `useAsyncData` vs `$fetch` | [Interview](./4_nuxt_interview.md#usefetch-vs-useasyncdata-vs-fetch) |
| Route vs server middleware | [Interview](./4_nuxt_interview.md#route-middleware-vs-server-middleware) |
| `routeRules` and prerendering | [Interview](./4_nuxt_interview.md#routerules-and-prerendering) |
| Coding tasks 1–3 | [Interview](./4_nuxt_interview.md#coding-tasks) |

---

## 20. Resources

*Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template)).*

<details>
  <summary>Primary references</summary>

### Primary references

**Definition:**

Official documentation and platform guides for Nuxt and Nitro.

- [nuxt.com/docs](https://nuxt.com/docs) — official documentation
- [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction) — framework overview
- [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching) — `useFetch`, `useAsyncData`, `$fetch`
- [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server) — Nitro server endpoints
- [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering) — SSG and crawl-based prerender
- [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment) — Node, static, serverless presets
- [nitro.build](https://nitro.build/) — Nitro server engine

**Sources:** [nuxt.com/docs](https://nuxt.com/docs)

</details>

<details>
  <summary>Deep dives</summary>

### Deep dives

**Definition:**

Topic-specific guides for routing, SEO, and hybrid rendering.

- [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing) — file-based routing and middleware
- [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta) — `useHead`, `useSeoMeta`
- [nuxt.com/docs/4.x/guide/directory-structure/layouts](https://nuxt.com/docs/4.x/guide/directory-structure/layouts) — layout conventions
- [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server) — `server/api`, `server/middleware`
- [nuxt.com/docs/4.x/guide/concepts/auto-imports](https://nuxt.com/docs/4.x/guide/concepts/auto-imports) — auto-imported composables and components

**Thoughts:** For generic Vue SSR, hydration, and reactivity on the server, see [Vue — SSR](./4_vue_ssr.md). This hub covers **Nuxt-specific** conventions and Nitro.

**Sources:** [nuxt.com/docs/4.x](https://nuxt.com/docs/4.x)

</details>
