# React — Web & mobile platforms

**Архитектура web- и mobile-приложений** — SPA, PWA, native, hybrid — с точки зрения frontend.

[← Назад к хабу](./4_react.md)

### Related docs

[Web apps — Types](./6_web_api_apps.md#types-of-web-apps) · [System design — Rendering](./0_system_design_architecture.md#rendering-strategies-when-html-is-built) · [React 19 — SPA vs SSR](./4_react_server.md#spa-vs-ssr-vs-rsc) · [Next.js rendering](./4_nextjs_rendering.md) · [React Native interview](./4_react_native_interview.md)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Типы клиентов и trade-offs |
| [Web — SPA & MPA](#web--spa--mpa) | CSR, routing, SEO |
| [Web — SSR & hybrid](#web--ssr--hybrid) | Next, Nuxt, RSC |
| [PWA & installable web](#pwa--installable-web) | Offline, manifest, service worker |
| [Mobile — native vs hybrid](#mobile--native-vs-hybrid) | RN, WebView, общий API layer |

### Sections

- [Overview](#overview)
- [Web — SPA & MPA](#web--spa--mpa)
- [Web — SSR & hybrid](#web--ssr--hybrid)
- [PWA & installable web](#pwa--installable-web)
- [Mobile — native vs hybrid](#mobile--native-vs-hybrid)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **Application architecture** здесь — **где выполняется код** (browser, server, device) и **как HTML/JS доходит до пользователя**, а не структура папок ([React — Architecture](./4_react_architecture.md)).

**How it works:**

| Platform | Runtime | Typical stack |
| -------- | ------- | ------------- |
| **Web SPA** | Только browser после первой загрузки | React + Vite + client router |
| **Web SSR/SSG** | Server строит HTML + hydrate | Next.js, Remix, Nuxt |
| **PWA** | Browser + service worker | SPA/SSR + manifest + SW |
| **Mobile native** | OS SDK (Swift/Kotlin) | Отдельно от React web |
| **Mobile hybrid** | JS на устройстве | **React Native**, Expo, Capacitor |

**When to use:** Интервью «SPA vs SSR vs mobile?» — сопоставьте продукт (SEO, offline, app store) с таблицей.

**Sources:** [6_web_api_apps — Types of web apps](./6_web_api_apps.md#types-of-web-apps)

</details>

---

## Web — SPA & MPA

<details>
<summary>Web — SPA & MPA</summary>

**Definition:** **Single Page Application (SPA)** грузит одну оболочку и меняет view через JS routing. **Multi-Page Application (MPA)** запрашивает полные HTML-документы при каждой навигации.

**How it works:**

| | SPA | MPA |
| - | --- | --- |
| Navigation | Client router (`react-router`) | Full page load |
| First paint | Часто ждёт JS bundle | HTML может отрисоваться раньше |
| SEO | Нужен SSR/prerender или meta strategy | Естественно per URL |
| Backend | Обычно REST/GraphQL API | Может быть server templates |

**When to use:** **SPA** для authenticated dashboards; **MPA** или **SSR framework** для marketing и SEO-critical сайтов.

**Thoughts:** [Client routing — Ecosystem](./4_react_ecosystem.md#client-routing) · [Code splitting](./4_react_build.md#code-splitting-in-react).

**Sources:** [React — Core — rendering model](./4_react_core.md)

</details>

---

## Web — SSR & hybrid

<details>
<summary>Web — SSR & hybrid</summary>

**Definition:** **Server-side rendering** генерирует HTML на server per request; **static generation (SSG)** строит HTML при deploy; **RSC** по умолчанию выполняет React components на server в Next App Router.

**How it works:**

```text
Request → server renders HTML + payload → browser hydrates → SPA-like navigation
```

| Mode | Best for |
| ---- | -------- |
| **CSR-only SPA** | Internal tools, heavy client interactivity |
| **SSR** | Персонализированный, свежий HTML |
| **SSG/ISR** | Docs, catalog с periodic rebuild |
| **RSC** | Меньше client JS; fetch на server |

**When to use:** Публичный контент + auth app в одном продукте → hybrid (marketing SSG + app SPA island).

**Sources:** [0_system_design — Rendering strategies](./0_system_design_architecture.md#rendering-strategies-when-html-is-built) · [4_react_server.md](./4_react_server.md)

</details>

---

## PWA & installable web

<details>
<summary>PWA & installable web</summary>

**Definition:** **Progressive Web App** — web app с installability/offline: **manifest**, **HTTPS**, **service worker**.

**How it works:**

- **Manifest** — name, icons, `display: standalone`
- **Service worker** — cache static assets; optional offline shell
- **Limitations** — не полноценный app store без wrapper (TWA, Capacitor)

**When to use:** Field apps с нестабильной сетью; избегать, когда нужны deep OS API без wrapper.

**Sources:** [6_web_api_apps — PWA](./6_web_api_apps.md#pwa-progressive-web-app)

</details>

---

## Mobile — native vs hybrid

<details>
<summary>Mobile — native vs hybrid</summary>

**Definition:** **Native** apps используют platform UI toolkits. **Hybrid** — web tech или **React Native** (native views на JS) в app shell.

**How it works:**

| Approach | Code sharing with React web | Trade-off |
| -------- | --------------------------- | --------- |
| **React Native / Expo** | Паттерны, часть logic; не DOM | Отдельные navigation/components |
| **WebView wrapper** | Высокий reuse web SPA | Performance, UX gaps |
| **Fully native** | Только API contracts | Лучший platform UX, две команды |

**When to use:** **React Native**, когда команда знает React и нужны iOS+Android почти native; общий **API client** ([REST client](./4_react_api_client.md)) и design tokens где возможно.

**Thoughts:** Mobile Q&A → [React Native interview](./4_react_native_interview.md); backend по-прежнему **BFF/gateway** для mobile ([Backend integration](./4_react_backend_integration.md)).

**Sources:** [reactnative.dev](https://reactnative.dev/)

</details>
