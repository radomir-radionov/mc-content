# React — Observability

Frontend **logs**, **error tracking** (Sentry and alternatives), and **monitoring** in production.

[← Back to hub](./4_react.md)

### Related docs

[DevOps — Observability](./15_devops.md) · [Build — Source maps](./10_build_bundlers.md) · [Build — CI](./10_build_ci.md) · [Resilience](./4_react_resilience.md) · [Performance — Production checklist](./4_react_perf.md#production-checklist)

### Main concepts

| Topic                                                   | Covers                             |
| ------------------------------------------------------- | ---------------------------------- |
| [Overview](#overview)                                   | Logs vs errors vs RUM              |
| [Browser logging](#browser-logging)                     | Console, structured client logs    |
| [Sentry and error tracking](#sentry-and-error-tracking) | SDK, breadcrumbs, releases         |
| [RUM and session tools](#rum-and-session-tools)         | Datadog, LogRocket, Highlight      |
| [Operating in production](#operating-in-production)     | Triage workflow, source maps in CI |

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

**Definition:** **Frontend observability** is how you **detect**, **diagnose**, and **fix** user-facing failures — JavaScript exceptions, failed API calls, slow interactions — using logs, error reports, and real-user metrics.

**How it works:**

| Signal               | What it answers                 | Typical tool                                                  |
| -------------------- | ------------------------------- | ------------------------------------------------------------- |
| **Error events**     | What threw? stack? user path?   | Sentry, Bugsnag, Rollbar                                      |
| **Logs**             | What happened before the throw? | Sentry breadcrumbs, browser console → forwarder               |
| **RUM / Web Vitals** | Is it slow for real users?      | Sentry Performance, Datadog RUM, web-vitals                   |
| **Traces**           | Which API span failed?          | OpenTelemetry browser + backend ([15_devops](./15_devops.md)) |

**When to use:** Any production React app; interview expectation: name **Sentry** and describe **release + source maps + user context**.

**Sources:** [OpenTelemetry — browser](https://opentelemetry.io/docs/languages/js/getting-started/browser/)

</details>

---

## Browser logging

<details>
<summary>Browser logging</summary>

**Definition:** **Client logging** captures diagnostic messages in the browser — historically `console.*`, increasingly **structured** JSON sent to a collector in staging/production.

**How it works:**

- **Development:** `console.log` / React DevTools; avoid logging PII
- **Production:** strip verbose logs in build; keep `console.error` for truly unexpected paths or route to SDK
- **Structured:** `{ level, message, userId, route, correlationId }` — same id as backend `X-Request-Id`
- **Global handlers:** `window.onerror`, `unhandledrejection` → forward to Sentry ([Promises — monitoring](./3_js_async_promises.md))

**When to use:** Debugging “works on my machine” — compare **session replay** or breadcrumb trail with server logs.

**Pluses and minuses:**

| +                         | −                                           |
| ------------------------- | ------------------------------------------- |
| Cheap in dev              | Users can open devtools — never log secrets |
| Breadcrumbs enrich errors | Log volume without sampling costs money     |

**Sources:** [MDN — Console API](https://developer.mozilla.org/en-US/docs/Web/API/console)

</details>

---

## Sentry and error tracking

<details>
<summary>Sentry and error tracking</summary>

**Definition:** **Sentry** (and peers: **Bugsnag**, **Rollbar**, **Honeybadger**) collect **exceptions**, **breadcrumbs**, **release version**, and **user context** from the browser SDK.

**How it works:**

| Feature                                      | Purpose                                                       |
| -------------------------------------------- | ------------------------------------------------------------- |
| `@sentry/react` + `ErrorBoundary`            | Capture React render errors with component stack              |
| `Sentry.init({ dsn, release, environment })` | Tag events by deploy                                          |
| **Breadcrumbs**                              | Navigation, clicks, XHR/fetch before crash                    |
| **Source maps**                              | Map minified stack to TS lines (upload in CI, not public CDN) |
| **Sampling**                                 | `tracesSampleRate` / `replaysSessionSampleRate` control cost  |

**When to use:** Default answer for “how do you track frontend errors in production?”

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

**Thoughts:** Upload maps in CI then delete from public artifact → [Build — Bundlers — source maps](./10_build_bundlers.md).

**Sources:** [Sentry — React](https://docs.sentry.io/platforms/javascript/guides/react/) · [Sentry — Source maps](https://docs.sentry.io/platforms/javascript/sourcemaps/)

</details>

---

## RUM and session tools

<details>
<summary>RUM and session tools</summary>

**Definition:** **Real User Monitoring (RUM)** measures performance and errors from **actual sessions**; **session replay** records DOM (with privacy masking) for reproduction.

**How it works:**

| Product                         | Strength                                    |
| ------------------------------- | ------------------------------------------- |
| **Sentry Performance + Replay** | Errors + traces + replay in one vendor      |
| **Datadog RUM**                 | Full-stack if backend already on Datadog    |
| **LogRocket / Highlight**       | Replay-first debugging                      |
| **web-vitals** library          | Send LCP/INP/CLS to your analytics endpoint |

**When to use:** INP regressions after a release; “user clicked Checkout twice” support tickets.

**Thoughts:** Mask inputs and PII in replay config; align with GDPR/consent banners.

**Sources:** [web.dev — Web Vitals](https://web.dev/articles/vitals) · [Performance — checklist](./4_react_perf.md#production-checklist)

</details>

---

## Operating in production

<details>
<summary>Operating in production</summary>

**Definition:** **Operating** frontend observability is the loop: alert → triage → fix → verify on next **release**.

**How it works:**

1. **Triage:** filter by `release`, `environment`, `url`, `browser`
2. **Reproduce:** replay or breadcrumb path; reproduce with same feature flags
3. **Fix:** code + test; link Sentry issue to PR
4. **Verify:** mark release healthy when error rate drops; watch Web Vitals dashboard

| CI step                                  | Why                             |
| ---------------------------------------- | ------------------------------- |
| `npm run build` with `SENTRY_AUTH_TOKEN` | Upload source maps to Sentry    |
| E2E smoke on preview                     | Catch broken deploy before prod |
| Git tag = Sentry `release`               | Match errors to commit          |

**When to use:** On-call or rotation for customer-facing SPAs; pair with [GitLab CI / GitHub Actions](./10_build_ci.md).

**Sources:** [Build — CI/CD pipeline stages](./10_build_ci.md#cicd-pipeline-stages)

</details>
