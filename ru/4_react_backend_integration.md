# React — Backend integration

Как **фронтенд** взаимодействует с **микросервисами**: шлюзы, BFF, контракты и режимы отказов.

[← Назад к хабу](./4_react.md)

### Related docs

[REST API client](./4_react_api_client.md) · [TypeScript — API contracts](./5_ts_api_contracts.md) · [System design — Microservices](./0_system_design_architecture.md#microservices-architecture) · [API Gateway](./7_special_architecture.md#api-gateway) · [BFF](./7_special_architecture.md#bff-backend-for-frontend) · [Web APIs — REST](./6_web_api_rest.md)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Один UI, много backend-сервисов |
| [API gateway & BFF](#api-gateway--bff) | Единая точка входа vs API под UI |
| [Microservices from the browser](#microservices-from-the-browser) | Чего SPA никогда не делает |
| [Contracts & versioning](#contracts--versioning) | OpenAPI, breaking changes |
| [Partial failure & resilience](#partial-failure--resilience) | Таймауты, деградированный UI |

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

**Definition:** **Backend integration** для React — это способ, которым UI получает **бизнес-данные**, когда серверная часть разбита на **сервисы** (заказы, каталог, биллинг), а не один монолитный API.

**How it works:**

```text
Browser (React) → HTTPS → [Gateway or BFF] → service A / B / C
                              ↑
                    auth, routing, aggregation
```

| Pattern | Frontend sees | Typical owner |
| ------- | ------------- | ------------- |
| **Monolith API** | Один base URL, много ресурсов | Небольшой продукт |
| **API gateway** | Один публичный хост; маршрутизация по path/header | Platform team |
| **BFF** | Один API **на клиент** (web vs mobile) | Пара frontend + backend |
| **Direct multi-service** | Несколько base URL из env | В браузере по возможности избегать |

**When to use:** На собеседовании и в проде ожидают объяснение **почему** SPA обычно ходит в **одну** backend-поверхность, а не в пять портов микросервисов из браузера.

**Thoughts:** Saga, события и DB-per-service — заботы backend; для контекста см. [System design — Saga](./0_system_design_core.md), но не реализацию в React.

**Sources:** [Microservices.io — patterns](https://microservices.io/patterns/index.html)

</details>

---

## API gateway & BFF

<details>
<summary>API gateway & BFF</summary>

**Definition:** **API gateway** — reverse proxy, который маршрутизирует, аутентифицирует и rate-limit’ит трафик к внутренним сервисам. **BFF (Backend for Frontend)** — **выделенный backend**, который формирует DTO и агрегирует вызовы под один UI (web dashboard vs mobile app).

**How it works:**

| Concern | Gateway | BFF |
| ------- | ------- | --- |
| TLS termination | Да | Часто за gateway |
| JWT validation | Да | Может делегировать или повторять |
| **Response shape for UI** | Обычно passthrough | **Под UI** (меньше round-trip’ов) |
| GraphQL federation | Иногда на edge | Часто слой BFF |

**When to use:** **BFF**, когда web-приложению нужен один «screen API» (dashboard) из 3 сервисов; **gateway**, когда много клиентов делят одну REST-поверхность.

**Examples:**

```text
GET /api/v1/dashboard  →  BFF aggregates:
  - GET catalog-service/products?limit=5
  - GET billing-service/invoices?status=open
  → single JSON for React Query
```

**Thoughts:** **Route Handlers** Next.js или server actions могут быть лёгким BFF без отдельного Node-сервиса — [Next.js data](./4_nextjs_data.md).

**Sources:** [7_special — API Gateway](./7_special_architecture.md#api-gateway) · [7_special — BFF](./7_special_architecture.md#bff-backend-for-frontend)

</details>

---

## Microservices from the browser

<details>
<summary>Microservices from the browser</summary>

**Definition:** Браузер — **ненадёжный, высоколатентный** клиент: он не должен хранить секреты service discovery и fan-out’ить запросы на множество internal-хостов.

**How it works:**

| Do | Don't |
| -- | ----- |
| Вызывать **один публичный API** (`VITE_API_URL`) | Светить internal `.svc.cluster.local` URL |
| Передавать **correlation id** (`X-Request-Id`) для поддержки | Вшивать service-to-service API keys в JS |
| **CORS** только на публичном edge | Полагаться на CORS, чтобы «спрятать» микросервисы |
| Кэш через TanStack Query / RTK Query | N+1 последовательных fetch на экран без BFF |

**When to use:** Любой production SPA за микросервисами; на system design интервью — с **границы клиента**.

**Thoughts:** **Module Federation** делит **frontend**-команды, а не backend-сервисы — [Architecture — MFE](./4_react_architecture.md#micro-frontends-mfe).

**Sources:** [REST client — Overview](./4_react_api_client.md#overview)

</details>

---

## Contracts & versioning

<details>
<summary>Contracts & versioning</summary>

**Definition:** **API contracts** (OpenAPI, JSON Schema) задают формы request/response, чтобы frontend и backend эволюционировали без тихого drift.

**How it works:**

- Генерировать TypeScript-клиенты (`orval`, OpenAPI Generator) или валидировать **Zod** на границе
- Версия в URL (`/v2/users`) или header — один стандарт на организацию
- Breaking changes: согласованный порядок деплоя или **параллельные** endpoint’ы на миграции

**When to use:** Несколько сервисов у одного React-приложения; contract tests в CI ([TypeScript — API contracts](./5_ts_api_contracts.md)).

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

**Definition:** В микросервисах **падение одной зависимости** не должно белить весь экран — показывайте **деградированные** секции, retry и понятные ошибки.

**How it works:**

| Technique | UI behavior |
| --------- | ----------- |
| Per-query error в TanStack Query | Каталог грузится; «Recommendations unavailable» |
| Timeouts на `fetch`/axios | Fail fast; retry с backoff |
| Circuit breaker (server/BFF) | Stale cache + banner |
| Error boundary | Ловит render throws от неверных допущений |

**When to use:** Dashboards с несколькими сервисами; checkout с payment + inventory.

**Thoughts:** [Resilience — error boundaries](./4_react_resilience.md) · [Observability — Sentry](./4_react_observability.md#sentry-and-error-tracking).

**Sources:** [REST client — Error handling](./4_react_api_client.md#error-handling)

</details>
