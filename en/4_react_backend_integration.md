# React — Backend integration

How a **frontend** talks to **microservices**: gateways, BFF, contracts, and failure modes.

[← Back to hub](./4_react.md)

### Related docs

[REST API client](./4_react_api_client.md) · [TypeScript — API contracts](./5_ts_api_contracts.md) · [System design — Microservices](./0_system_design_architecture.md#microservices-architecture) · [API Gateway](./7_special_architecture.md#api-gateway) · [BFF](./7_special_architecture.md#bff-backend-for-frontend) · [Web APIs — REST](./6_web_api_rest.md)

### Main concepts

| Topic                                                             | Covers                        |
| ----------------------------------------------------------------- | ----------------------------- |
| [Overview](#overview)                                             | One UI, many backend services |
| [API gateway & BFF](#api-gateway--bff)                            | Single entry vs UI-shaped API |
| [Microservices from the browser](#microservices-from-the-browser) | What the SPA must never do    |
| [Contracts & versioning](#contracts--versioning)                  | OpenAPI, breaking changes     |
| [Partial failure & resilience](#partial-failure--resilience)      | Timeouts, degraded UI         |

### Sections

- [Overview](#overview)
- [API gateway & BFF](#api-gateway--bff)
- [Microservices from the browser](#microservices-from-the-browser)
- [Contracts & versioning](#contracts--versioning)
- [Partial failure & resilience](#partial-failure--resilience)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **Backend integration** for React is how the UI reaches **business data** when the server side is split into **services** (orders, catalog, billing) instead of one monolith API.

**How it works:**

```text
Browser (React) → HTTPS → [Gateway or BFF] → service A / B / C
                              ↑
                    auth, routing, aggregation
```

| Pattern                  | Frontend sees                          | Typical owner                  |
| ------------------------ | -------------------------------------- | ------------------------------ |
| **Monolith API**         | One base URL, many resources           | Small product                  |
| **API gateway**          | One public host; routes by path/header | Platform team                  |
| **BFF**                  | One API **per client** (web vs mobile) | Frontend + backend pair        |
| **Direct multi-service** | Several base URLs from env             | Avoid in browser when possible |

**When to use:** Interview and real jobs expect you to explain **why** the SPA usually calls **one** backend surface, not five microservice ports from the browser.

**Thoughts:** Saga, events, and DB-per-service are backend concerns — link [System design — Saga](./0_system_design_core.md) for context, not implementation in React.

**Sources:** [Microservices.io — patterns](https://microservices.io/patterns/index.html)

</details>

---

## API gateway & BFF

<details>
<summary>API gateway & BFF</summary>

**Definition:** An **API gateway** is a reverse proxy that routes, authenticates, and rate-limits traffic to internal services. A **BFF (Backend for Frontend)** is a **dedicated backend** that shapes DTOs and aggregates calls for one UI (web dashboard vs mobile app).

**How it works:**

| Concern                   | Gateway             | BFF                              |
| ------------------------- | ------------------- | -------------------------------- |
| TLS termination           | Yes                 | Often behind gateway             |
| JWT validation            | Yes                 | Can delegate or repeat           |
| **Response shape for UI** | Usually passthrough | **Tailored** (fewer round-trips) |
| GraphQL federation        | Sometimes at edge   | Often the BFF layer              |

**When to use:** **BFF** when the web app needs one “screen API” (dashboard) built from 3 services; **gateway** when many clients share the same REST surface.

**Examples:**

```text
GET /api/v1/dashboard  →  BFF aggregates:
  - GET catalog-service/products?limit=5
  - GET billing-service/invoices?status=open
  → single JSON for React Query
```

**Thoughts:** Next.js **Route Handlers** or server actions can act as a lightweight BFF without a separate Node service — [Next.js data](./4_nextjs_data.md).

**Sources:** [7_special — API Gateway](./7_special_architecture.md#api-gateway) · [7_special — BFF](./7_special_architecture.md#bff-backend-for-frontend)

</details>

---

## Microservices from the browser

<details>
<summary>Microservices from the browser</summary>

**Definition:** The browser is an **untrusted, high-latency** client — it should not hold service discovery secrets or fan out to many internal hosts.

**How it works:**

| Do                                                   | Don't                                         |
| ---------------------------------------------------- | --------------------------------------------- |
| Call **one public API** (`VITE_API_URL`)             | Expose internal `.svc.cluster.local` URLs     |
| Pass **correlation id** (`X-Request-Id`) for support | Embed service-to-service API keys in JS       |
| Use **CORS** only on the public edge                 | Rely on CORS to hide microservices            |
| Cache with TanStack Query / RTK Query                | N+1 sequential fetches per screen without BFF |

**When to use:** Any production SPA behind microservices; explain in system design interviews from the **client boundary**.

**Thoughts:** **Module Federation** splits **frontend** teams, not backend services — [Architecture — MFE](./4_react_architecture.md#micro-frontends-mfe).

**Sources:** [REST client — Overview](./4_react_api_client.md#overview)

</details>

---

## Contracts & versioning

<details>
<summary>Contracts & versioning</summary>

**Definition:** **API contracts** (OpenAPI, JSON Schema) define request/response shapes so frontend and backend evolve without silent drift.

**How it works:**

- Generate TypeScript clients (`orval`, OpenAPI Generator) or validate with **Zod** at the boundary
- Version in URL (`/v2/users`) or header — pick one org standard
- Breaking changes: coordinate deploy order or run **parallel** endpoints during migration

**When to use:** Multiple services consumed by one React app; CI contract tests ([TypeScript — API contracts](./5_ts_api_contracts.md)).

**Examples:**

```ts
// Generated or hand-written — single import surface for the app
import { getDashboard } from "@/api/generated/bff";
```

**Sources:** [5_ts_api_contracts.md](./5_ts_api_contracts.md) · [6_web_api_rest.md — contract](./6_web_api_rest.md#rest-principles-and-contract)

</details>

---

## Partial failure & resilience

<details>
<summary>Partial failure & resilience</summary>

**Definition:** In microservices, **one dependency failing** should not white-screen the whole app — show **degraded** sections, retries, and clear errors.

**How it works:**

| Technique                         | UI behavior                                  |
| --------------------------------- | -------------------------------------------- |
| Per-query error in TanStack Query | Catalog loads; “Recommendations unavailable” |
| Timeouts on `fetch`/axios         | Fail fast; retry with backoff                |
| Circuit breaker (server/BFF)      | Cached stale data + banner                   |
| Error boundary                    | Catch render throws from bad assumptions     |

**When to use:** Dashboards aggregating multiple services; checkout flows with payment + inventory.

**Thoughts:** [Resilience — error boundaries](./4_react_resilience.md) · [Observability — Sentry](./4_react_observability.md#sentry-and-error-tracking).

**Sources:** [REST client — Error handling](./4_react_api_client.md#error-handling)

</details>
