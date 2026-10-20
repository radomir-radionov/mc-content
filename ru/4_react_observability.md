# React — Observability

**Логи**, **трекинг ошибок** (Sentry и аналоги) и **мониторинг** фронтенда в production.

[← Назад к хабу](./4_react.md)

### Related docs

[DevOps — Observability](./15_devops.md) · [Build — Source maps](./10_build_bundlers.md) · [Build — CI](./10_build_ci.md) · [Resilience](./4_react_resilience.md) · [Performance — Production checklist](./4_react_perf.md#production-checklist)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Логи vs ошибки vs RUM |
| [Browser logging](#browser-logging) | Console, структурированные client logs |
| [Sentry and error tracking](#sentry-and-error-tracking) | SDK, breadcrumbs, releases |
| [RUM and session tools](#rum-and-session-tools) | Datadog, LogRocket, Highlight |
| [Operating in production](#operating-in-production) | Triage, source maps в CI |

### Sections

- [Overview](#overview)
- [Browser logging](#browser-logging)
- [Sentry and error tracking](#sentry-and-error-tracking)
- [RUM and session tools](#rum-and-session-tools)
- [Operating in production](#operating-in-production)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **Frontend observability** — как **обнаруживать**, **диагностировать** и **чинить** пользовательские сбои: JS-исключения, failed API, медленные взаимодействия — через логи, error reports и real-user метрики.

**How it works:**

| Signal | What it answers | Typical tool |
| ------ | --------------- | ------------ |
| **Error events** | Что упало? stack? путь пользователя? | Sentry, Bugsnag, Rollbar |
| **Logs** | Что было до throw? | Sentry breadcrumbs, console → forwarder |
| **RUM / Web Vitals** | Медленно ли у реальных пользователей? | Sentry Performance, Datadog RUM, web-vitals |
| **Traces** | Какой API span упал? | OpenTelemetry browser + backend ([15_devops](./15_devops.md)) |

**When to use:** Любое production React-приложение; на интервью — назвать **Sentry** и описать **release + source maps + user context**.

**Sources:** [OpenTelemetry — browser](https://opentelemetry.io/docs/languages/js/getting-started/browser/)

</details>

---

## Browser logging

<details>
<summary>Browser logging</summary>

**Definition:** **Client logging** фиксирует диагностические сообщения в браузере — исторически `console.*`, всё чаще **структурированный** JSON в collector на staging/production.

**How it works:**

- **Development:** `console.log` / React DevTools; не логировать PII
- **Production:** убрать verbose logs в build; оставить `console.error` для неожиданных путей или слать в SDK
- **Structured:** `{ level, message, userId, route, correlationId }` — тот же id, что backend `X-Request-Id`
- **Global handlers:** `window.onerror`, `unhandledrejection` → Sentry ([Promises — monitoring](./3_js_async_promises.md))

**When to use:** «У меня работает» — сравнить **session replay** или breadcrumb trail с server logs.

**Pluses and minuses:**

| + | − |
| - | - |
| Дёшево в dev | Пользователь видит devtools — не логировать секреты |
| Breadcrumbs обогащают errors | Объём логов без sampling стоит денег |

**Sources:** [MDN — Console API](https://developer.mozilla.org/en-US/docs/Web/API/console)

</details>

---

## Sentry and error tracking

<details>
<summary>Sentry and error tracking</summary>

**Definition:** **Sentry** (и аналоги: **Bugsnag**, **Rollbar**, **Honeybadger**) собирают **исключения**, **breadcrumbs**, **release version** и **user context** из browser SDK.

**How it works:**

| Feature | Purpose |
| ------- | ------- |
| `@sentry/react` + `ErrorBoundary` | React render errors с component stack |
| `Sentry.init({ dsn, release, environment })` | Теги событий по деплою |
| **Breadcrumbs** | Navigation, clicks, XHR/fetch до crash |
| **Source maps** | Minified stack → строки TS (upload в CI, не на public CDN) |
| **Sampling** | `tracesSampleRate` / `replaysSessionSampleRate` контролируют cost |

**When to use:** Стандартный ответ на «как отслеживаете frontend errors в production?».

**Examples:**

```tsx
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: import.meta.env.VITE_SENTRY_DSN,
  release: import.meta.env.VITE_APP_RELEASE,
  integrations: [Sentry.browserTracingIntegration()],
  tracesSampleRate: 0.1,
});

export const App = Sentry.withErrorBoundary(Root, {
  fallback: <p>Something went wrong.</p>,
});
```

**Thoughts:** Upload maps в CI, затем удалить из public artifact → [Build — Bundlers — source maps](./10_build_bundlers.md).

**Sources:** [Sentry — React](https://docs.sentry.io/platforms/javascript/guides/react/) · [Sentry — Source maps](https://docs.sentry.io/platforms/javascript/sourcemaps/)

</details>

---

## RUM and session tools

<details>
<summary>RUM and session tools</summary>

**Definition:** **Real User Monitoring (RUM)** измеряет performance и ошибки в **реальных сессиях**; **session replay** записывает DOM (с маскированием PII) для воспроизведения.

**How it works:**

| Product | Strength |
| ------- | -------- |
| **Sentry Performance + Replay** | Errors + traces + replay в одном vendor |
| **Datadog RUM** | Full-stack, если backend уже на Datadog |
| **LogRocket / Highlight** | Replay-first debugging |
| **web-vitals** library | LCP/INP/CLS на ваш analytics endpoint |

**When to use:** Регрессии INP после релиза; тикеты «пользователь дважды нажал Checkout».

**Thoughts:** Маскируйте inputs и PII в replay; согласуйте с GDPR/consent banners.

**Sources:** [web.dev — Web Vitals](https://web.dev/articles/vitals) · [Performance — checklist](./4_react_perf.md#production-checklist)

</details>

---

## Operating in production

<details>
<summary>Operating in production</summary>

**Definition:** **Operating** frontend observability — цикл: alert → triage → fix → verify на следующем **release**.

**How it works:**

1. **Triage:** фильтр по `release`, `environment`, `url`, `browser`
2. **Reproduce:** replay или breadcrumb path; reproduce с теми же feature flags
3. **Fix:** код + test; связать Sentry issue с PR
4. **Verify:** пометить release healthy, когда error rate падает; смотреть Web Vitals dashboard

| CI step | Why |
| ------- | --- |
| `npm run build` с `SENTRY_AUTH_TOKEN` | Upload source maps в Sentry |
| E2E smoke на preview | Поймать broken deploy до prod |
| Git tag = Sentry `release` | Сопоставить errors с commit |

**When to use:** On-call для customer-facing SPA; в паре с [GitLab CI / GitHub Actions](./10_build_ci.md).

**Sources:** [Build — CI/CD pipeline stages](./10_build_ci.md#cicd-pipeline-stages)

</details>
