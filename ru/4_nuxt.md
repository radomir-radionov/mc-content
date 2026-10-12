# Nuxt — hub

Файловая маршрутизация, SSR/SSG, загрузка данных, сервер Nitro и платформа Nuxt. Теория живёт в файлах тем ниже.

### Related docs

[Nuxt — Ядро](./4_nuxt_core.md) · [Nuxt — Данные](./4_nuxt_data.md) · [Nuxt — Маршрутизация](./4_nuxt_routing.md) · [Nuxt — Платформа](./4_nuxt_platform.md) · [Nuxt — Интервью](./4_nuxt_interview.md) · [Vue — SSR](./4_vue_ssr.md)

### Navigation

| Topic | Covers |
|------|--------|
| [Nuxt — Ядро](./4_nuxt_core.md) | Модель фреймворка, стратегии рендеринга и принципы проектирования. |
| [Nuxt — Данные](./4_nuxt_data.md) | `useFetch`, `useAsyncData`, кэширование и серверные маршруты. |
| [Nuxt — Маршрутизация](./4_nuxt_routing.md) | Файловые маршруты, layouts, middleware, навигация и SEO. |
| [Nuxt — Платформа](./4_nuxt_platform.md) | Nitro, модули, `nuxt.config`, деплой, миграция и отладка. |
| [Nuxt — Интервью](./4_nuxt_interview.md) | Вопросы и ответы на интервью и задачи по кодированию. |

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

*Темы следуют: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nuxt_core.md#topic-template)).*

<details>
  <summary>Primary references</summary>

### Primary references

**Definition:**

Официальная документация и руководства по платформе Nuxt и Nitro.

- [nuxt.com/docs](https://nuxt.com/docs) — официальная документация
- [nuxt.com/docs/4.x/getting-started/introduction](https://nuxt.com/docs/4.x/getting-started/introduction) — обзор фреймворка
- [nuxt.com/docs/4.x/getting-started/data-fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching) — `useFetch`, `useAsyncData`, `$fetch`
- [nuxt.com/docs/4.x/getting-started/server](https://nuxt.com/docs/4.x/getting-started/server) — эндпоинты сервера Nitro
- [nuxt.com/docs/4.x/getting-started/prerendering](https://nuxt.com/docs/4.x/getting-started/prerendering) — SSG и crawl-based prerender
- [nuxt.com/docs/4.x/getting-started/deployment](https://nuxt.com/docs/4.x/getting-started/deployment) — Node, static, serverless presets
- [nitro.build](https://nitro.build/) — серверный движок Nitro

**Sources:** [nuxt.com/docs](https://nuxt.com/docs)

</details>

<details>
  <summary>Deep dives</summary>

### Deep dives

**Definition:**

Тематические руководства по маршрутизации, SEO и гибридному рендерингу.

- [nuxt.com/docs/4.x/getting-started/routing](https://nuxt.com/docs/4.x/getting-started/routing) — файловая маршрутизация и middleware
- [nuxt.com/docs/4.x/getting-started/seo-meta](https://nuxt.com/docs/4.x/getting-started/seo-meta) — `useHead`, `useSeoMeta`
- [nuxt.com/docs/4.x/guide/directory-structure/layouts](https://nuxt.com/docs/4.x/guide/directory-structure/layouts) — соглашения по layouts
- [nuxt.com/docs/4.x/guide/directory-structure/server](https://nuxt.com/docs/4.x/guide/directory-structure/server) — `server/api`, `server/middleware`
- [nuxt.com/docs/4.x/guide/concepts/auto-imports](https://nuxt.com/docs/4.x/guide/concepts/auto-imports) — auto-import composables и компонентов

**Thoughts:** Для общего Vue SSR, гидрации и реактивности на сервере см. [Vue — SSR](./4_vue_ssr.md). Этот hub охватывает **Nuxt-специфичные** соглашения и Nitro.

**Sources:** [nuxt.com/docs/4.x](https://nuxt.com/docs/4.x)

</details>
