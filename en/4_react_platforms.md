# React — Web & mobile platforms

**Architecture of web and mobile apps** — SPA, PWA, native, hybrid — from a frontend perspective.

[← Back to hub](./4_react.md)

### Related docs

[Web apps — Types](./6_web_api_apps.md#types-of-web-apps) · [System design — Rendering](./0_system_design_architecture.md#rendering-strategies-when-html-is-built) · [React 19 — SPA vs SSR](./4_react_server.md#spa-vs-ssr-vs-rsc) · [Next.js rendering](./4_nextjs_rendering.md) · [React Native interview](./4_react_native_interview.md)

### Main concepts

| Topic                                                  | Covers                            |
| ------------------------------------------------------ | --------------------------------- |
| [Overview](#overview)                                  | Client types and trade-offs       |
| [Web — SPA & MPA](#web--spa--mpa)                      | CSR, routing, SEO                 |
| [Web — SSR & hybrid](#web--ssr--hybrid)                | Next, Nuxt, RSC                   |
| [PWA & installable web](#pwa--installable-web)         | Offline, manifest, service worker |
| [Mobile — native vs hybrid](#mobile--native-vs-hybrid) | RN, WebView, shared API layer     |

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

**Definition:** **Application architecture** here means **where code runs** (browser, server, device) and **how HTML/JS reaches the user** — not folder structure ([React — Architecture](./4_react_architecture.md)).

**How it works:**

| Platform          | Runtime                       | Typical stack                     |
| ----------------- | ----------------------------- | --------------------------------- |
| **Web SPA**       | Browser only after first load | React + Vite + client router      |
| **Web SSR/SSG**   | Server builds HTML + hydrates | Next.js, Remix, Nuxt              |
| **PWA**           | Browser + service worker      | SPA/SSR + manifest + SW           |
| **Mobile native** | OS SDK (Swift/Kotlin)         | Separate from React web           |
| **Mobile hybrid** | JS on device                  | **React Native**, Expo, Capacitor |

**When to use:** Interview “SPA vs SSR vs mobile?” — map product needs (SEO, offline, app store) to this table.

**Sources:** [6_web_api_apps — Types of web apps](./6_web_api_apps.md#types-of-web-apps)

</details>

---

## Web — SPA & MPA

<details>
<summary>Web — SPA & MPA</summary>

**Definition:** A **Single Page Application (SPA)** loads one shell and swaps views via JS routing. A **Multi-Page Application (MPA)** requests full HTML documents per navigation.

**How it works:**

|             | SPA                                  | MPA                     |
| ----------- | ------------------------------------ | ----------------------- |
| Navigation  | Client router (`react-router`)       | Full page load          |
| First paint | Often waits on JS bundle             | HTML can paint early    |
| SEO         | Needs SSR/prerender or meta strategy | Natural per URL         |
| Backend     | Usually REST/GraphQL API             | Can be server templates |

**When to use:** **SPA** for authenticated dashboards; **MPA** or **SSR framework** for marketing + SEO-critical sites.

**Thoughts:** [Client routing — Ecosystem](./4_react_ecosystem.md#client-routing) · [Code splitting](./4_react_build.md#code-splitting-in-react).

**Sources:** [React — Core — rendering model](./4_react_core.md)

</details>

---

## Web — SSR & hybrid

<details>
<summary>Web — SSR & hybrid</summary>

**Definition:** **Server-side rendering** generates HTML on the server per request; **static generation (SSG)** builds HTML at deploy time; **RSC** runs React components on the server by default in Next App Router.

**How it works:**

```text
Request → server renders HTML + payload → browser hydrates → SPA-like navigation
```

| Mode             | Best for                                   |
| ---------------- | ------------------------------------------ |
| **CSR-only SPA** | Internal tools, heavy client interactivity |
| **SSR**          | Personalized, fresh HTML                   |
| **SSG/ISR**      | Docs, catalog with periodic rebuild        |
| **RSC**          | Reduce client JS; fetch on server          |

**When to use:** Public content + auth app in one product → hybrid (marketing SSG + app SPA island).

**Sources:** [0_system_design — Rendering strategies](./0_system_design_architecture.md#rendering-strategies-when-html-is-built) · [4_react_server.md](./4_react_server.md)

</details>

---

## PWA & installable web

<details>
<summary>PWA & installable web</summary>

**Definition:** A **Progressive Web App** is a web app that meets installability/offline criteria: **manifest**, **HTTPS**, **service worker**.

**How it works:**

- **Manifest** — name, icons, `display: standalone`
- **Service worker** — cache static assets; optional offline shell
- **Limitations** — not full app-store distribution unless wrapped (TWA, Capacitor)

**When to use:** Field apps with flaky network; avoid when you need deep OS APIs without a wrapper.

**Sources:** [6_web_api_apps — PWA](./6_web_api_apps.md#pwa-progressive-web-app)

</details>

---

## Mobile — native vs hybrid

<details>
<summary>Mobile — native vs hybrid</summary>

**Definition:** **Native** apps use platform UI toolkits. **Hybrid** uses web tech or **React Native** (native views driven by JS) inside an app shell.

**How it works:**

| Approach                | Code sharing with React web   | Trade-off                      |
| ----------------------- | ----------------------------- | ------------------------------ |
| **React Native / Expo** | Patterns, some logic; not DOM | Separate navigation/components |
| **WebView wrapper**     | High reuse of web SPA         | Performance, UX gaps           |
| **Fully native**        | API contracts only            | Best platform UX, two teams    |

**When to use:** **React Native** when team knows React and needs iOS+Android with near-native feel; share **API client** layer ([REST client](./4_react_api_client.md)) and design tokens where possible.

**Thoughts:** Deep mobile Q&A → [React Native interview](./4_react_native_interview.md); backend still **BFF/gateway** for mobile ([Backend integration](./4_react_backend_integration.md)).

**Sources:** [reactnative.dev](https://reactnative.dev/)

</details>
