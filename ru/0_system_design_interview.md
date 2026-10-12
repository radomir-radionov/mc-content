# System design — Interview

Interview Q&A for system design.

[← Back to hub](./0_system_design.md)

### Related docs

- [System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [System design — Interview](./0_system_design_interview.md)

### Sections

- [Questions & Answers](#questions-answers)

---

## Questions & Answers

<details>
  <summary>Почему does a web app need a backend?</summary>

### Почему does a web app need a backend?

The browser only paints HTML/CSS/JS. User data must be stored, authorized, and processed under **business rules** — that is the server. The client names the action; the server talks to the DB and may run jobs (index, email). Architecture name: **client–server**. See [Client–server architecture](./0_system_design_core.md#clientserver-architecture-why-a-backend).

</details>

<details>
  <summary>Что такое Saga pattern?</summary>

### Что такое Saga pattern?

Distributed system can guarantee at most **two** of: **C**onsistency, **A**vailability, **P**artition tolerance. Under network partition, choose CP (strong consistency) or AP (availability + eventual consistency).

</details>

<details>
  <summary>Что такое Mediator pattern?</summary>

### Что такое Mediator pattern?

Round-robin, least connections, consistent hashing (sticky sessions). L7 (HTTP path/host) vs L4 (TCP). Health checks remove bad nodes.

</details>

<details>
  <summary>Слои caching?</summary>

### Слои caching?

Browser → CDN → API gateway → app memory (Redis) → DB query cache. Cache invalidation: TTL, write-through, write-behind, cache-aside. Stale-while-revalidate for UX.

</details>

<details>
  <summary>Когда message queue?</summary>

### Когда message queue?

Decouple producers/consumers, absorb traffic spikes, async jobs (email, exports), event-driven microservices. Trade-off: complexity, eventual consistency, ordering guarantees.

</details>

<details>
  <summary>Event-driven architecture — что и где?</summary>

### Event-driven architecture — что и где?

**Definition:** Services communicate by **publishing and consuming events** asynchronously instead of synchronous HTTP chains.

**How it works:**

1. Service A completes action → publishes `OrderPlaced` event to broker (Kafka, RabbitMQ, SNS).
2. Multiple subscribers react independently — inventory, payment, email, analytics.
3. No direct coupling — new consumer added without changing producer.

E-commerce order flows, notifications, data pipelines, microservice integration, CQRS read-model updates.

Versioned event contracts, **idempotent consumers** (at-least-once delivery), observability (trace_id across services).

Sync HTTP when user waits for result; events when work can happen later or fan-out to many systems.

**Pluses and minuses:**

| +                                   | −                                            |
| ----------------------------------- | -------------------------------------------- |
| Loose coupling, independent scaling | Eventual consistency                         |
| Natural audit trail (event log)     | Harder debugging (distributed traces needed) |
| Buffers traffic spikes in queue     | Schema evolution / versioning required       |

**Examples:**

```
OrderService --[order.placed]--> Kafka --> InventoryService (reserve)
                                      --> PaymentService (charge)
                                      --> NotificationService (email)
```

**Sources:** [Microservices.io — Event-driven architecture](https://microservices.io/patterns/data/event-driven-architecture.html)

**See also:** [Event-driven architecture](./7_special_architecture.md#event-driven-architecture)

</details>

<details>
  <summary>Saga pattern?</summary>

### Saga pattern?

**Definition:** Manages **distributed transactions** across microservices as a sequence of **local transactions**, each with a **compensating action** if a later step fails.

**How it works:**

```
1. Create order (status: pending)
2. Charge payment     → fail → cancel order (compensate)
3. Reserve inventory  → fail → refund payment + cancel order
4. Confirm order
```

2PC blocks and doesn't scale across microservices — sagas trade isolation for availability.

**When to use:**

Order + payment + inventory, travel booking, multi-step onboarding.

See [Distributed transactions & Saga](./0_system_design_core.md#distributed-transactions--saga) in section 2.

#### Pitfalls

Compensations are business logic (`refund` ≠ undo `ship`). Not all steps are reversible.

**Pluses and minuses:**

| Style             | How                                       | +                         | −                            |
| ----------------- | ----------------------------------------- | ------------------------- | ---------------------------- |
| **Orchestration** | Central coordinator calls each service    | Clear flow, easy to trace | Coordinator = SPOF if not HA |
| **Choreography**  | Services react to events, no central boss | Decoupled                 | Hard to see full picture     |

</details>

<details>
  <summary>Mediator pattern?</summary>

### Mediator pattern?

**Definition:** Central object coordinates communication between components so they don't reference each other directly.

**How it works:**

Observer — subject knows observers directly. **Mediator** — only mediator knows everyone.

Mediator usually in-process; pub/sub uses external broker for cross-service async messaging.

See [Mediator](./7_special_patterns.md#mediator) in section 7.

**When to use:** Chat rooms, UI dialog coordinators, NestJS `EventBus`/`CommandBus`, Redux store.

</details>

<details>
  <summary>BFF vs one shared API?</summary>

### BFF vs one shared API?

A **BFF** is a frontend-owned adapter that returns the exact payload one UI needs. Use it when web vs mobile (or TV vs phone) would otherwise over-fetch or under-fetch from one generic API. Skip it with a **single** client — extra hop + extra SPOF. Domain logic stays downstream; BFF aggregates and shapes. Origin: SoundCloud (~2013), documented by Calçado / Newman.

See [BFF](./7_special_architecture.md#bff-backend-for-frontend).

</details>

<details>
  <summary>API Gateway vs BFF?</summary>

### API Gateway vs BFF?

**Gateway** = one shared door: route to microservices, one public contract, often auth/cache/rate-limit. The browser still calls `/catalog/…` and `/reviews/…` separately; JSON stays service-shaped.

**BFF** = that idea plus **per-client composition** (Doka: BFF includes a gateway). Web: `GET /bff/pdp/123` → `{ title, price, reviews, inStock }`. Mobile BFF returns `{ name, price, thumbnail }`. Kong/NGINX for the whole company ≠ BFF. Stack: UI → BFF → gateway → services. Skip BFF if every client wants the same API.

See [API Gateway](./7_special_architecture.md#api-gateway) · [BFF](./7_special_architecture.md#bff-backend-for-frontend).

</details>

<details>
  <summary>SSR vs SSG vs ISR?</summary>

### SSR vs SSG vs ISR?

**SSR** — HTML per request (personalized, live). **SSG** — HTML at build, CDN (fastest, frozen until deploy). **ISR** — SSG + rebuild after `revalidate` (stale-while-revalidate). RSC/islands: only interactive leaves ship JS. The variable is _when HTML is built_.

See [Rendering strategies](./0_system_design_architecture.md#rendering-strategies-when-html-is-built) · [4_nextjs.md](./4_nextjs.md).

</details>

<details>
  <summary>Является ли edge rendering dead?</summary>

### Является ли edge rendering dead?

No — the **default** changed. Vercel deprecated standalone Edge Functions; Next.js `runtime = 'edge'` is deprecated. Put **Node compute next to the database**. Edge still wins for **static shells**, geo/auth middleware, and apps whose **data is global** (e.g. Cloudflare Workers + distributed storage). PPR: shell from the edge, dynamic from regional compute.

See [Edge rendering vs data locality](./0_system_design_architecture.md#edge-rendering-vs-data-locality).

</details>

<details>
  <summary>Когда micro-frontends?</summary>

### Когда micro-frontends?

When **team deploy coordination** is the bottleneck, typically 50+ engineers with clear domains — not because the SPA is "complex." Under ~15: modular monolith. 15–50: BFF + modular monolith is usually enough. Real failure mode: **two React versions on one page** (dependency drift), not mixing frameworks.

See [Micro-frontends](./7_special_architecture.md#micro-frontends).

</details>

<details>
  <summary>Monolith vs microservices?</summary>

### Monolith vs microservices?

**Monolith:** one deployable, modules share a process. **Microservices:** many processes, each a business capability, independently deployable over HTTP/RPC/queues.

**Why split:** independent scale of a hotspot, reuse (auth/mail in several products), team ownership when ≳10–15 people — not because small HTTP servers are cheaper.

**Interview example:** Shop checkout. Monolith: `orders.create` → `payments.charge` → `mail.send` in one process; a pay fix redeploys everything; Black Friday clones the whole app. Microservices: `POST /checkout` → Orders → `POST /payments`; Mail is a separate process (often via a queue) shared with other products; scale 20 payment boxes and 2 catalog boxes.

**Stay monolith** until pain is real and you have CI/CD + tracing. Fowler: most successful splits started from a monolith that got too big. Doka’s “cheaper to maintain” is the optimistic case after ops is mature.

See [Microservices architecture](./0_system_design_architecture.md#microservices-architecture).

</details>

<details>
  <summary>Архитектура против design patterns?</summary>

### Архитектура против design patterns?

**Architecture** = decisions about how _the whole system_ is cut and how modules talk (MVC roles, layers vs infra, Flux/Redux one-way flow, monolith vs many apps). **Patterns** = named fixes for _local_ problems (Adapter maps `{ some_data: ["Name", "Lastname"] }` → `{ someData: "Name Lastname" }`; Factory hides `new`; Strategy swaps `format.usd` / `format.eur`; Observer `subscribe`/`notify`). Same photo app: `applySepia` is domain, `readCamera` is infra, `dispatch(setFilter)` is one-way state — that is architecture; the Adapter is a pattern inside it. Neither is the goal — skip ceremony on a prototype.

See [Architecture vs design patterns](./7_special_architecture.md#architecture-vs-design-patterns).

</details>

<details>
  <summary>Three-layer / Clean Architecture?</summary>

### Three-layer / Clean Architecture?

**Three layers (Doka):** **domain** (pure business data + transforms), **application** (use cases + port _interfaces_), **ports & adapters** (UI, HTTP, DB glue). **Clean / Hexagonal / Onion** are the same family: Uncle Bob adds a fourth outer ring (frameworks & drivers).

**Dependency rule:** source dependencies only point **inward**. Domain knows nothing outer. Application never calls Stripe/`fetch` directly — it calls a port; the adapter implements it.

**Port vs adapter:** port = the API _we_ want (`saveProduct`); adapter = make a real service look like that. A rename wrapper is not an adapter until you can swap the vendor without changing application code.

**Driving vs driven:** UI/HTTP/CLI _drive_ the app; storage/email/payments are _driven_ by the app.

**Start cheap:** extract the domain first; full layers when multiple UIs or replaceable I/O actually hurt.

**Place this line:** `Math.round(cents * 0.9)` → domain. `await storage.save(userId, cart)` → application. `body.product_id` → `productId` → adapter. `onClick` → driving adapter.

See [Three-layer architecture](./7_special_architecture.md#three-layer-architecture-doka) · [Clean Architecture & Hexagonal](./7_special_architecture.md#clean-architecture--hexagonal).

</details>

<details>
  <summary>Behavioral patterns — Strategy, CoR, Command, Observer?</summary>

### Behavioral patterns — Strategy, CoR, Command, Observer?

**Strategy** — same interface, swap the algorithm (even at runtime). **Chain of Responsibility** — request walks handlers until one handles it; dispatcher need not know the full set (`switch` is fine when it does). **Command** — action + data as an object (queue/undo); Redux actions are the data half. **Observer** — subscribe/notify when you don’t know how many listeners. Don’t mix CoR with middleware: middleware usually lets _every_ layer run.

See [Behavioral patterns — cheat sheet](./7_special_patterns.md#behavioral-patterns--cheat-sheet).

</details>

<details>
  <summary>Что такое Classes? constructor injection?</summary>

### Что такое Classes? constructor injection?

A **class** is a blueprint: fields, methods, `constructor`. In JS it is prototype sugar. **Constructor injection** passes collaborators in (`new DailyAmount(budget, history)`) so the high-level type does not `new` its tools — tests swap fakes, UML arrows stay one-way.

See [Entities, relations & design](./0_system_design_grasp.md#entities-relations--design) · [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md) · [7_special.md — What are classes?](./7_special_interview.md#interview-practice)

</details>

---

### Tasks

### Task 1 — Design URL shortener

High-level components for read-heavy short URL service.

<details><summary>Outline</summary>

API (redirect + create), hash/id generator, SQL/NoSQL store mapping short→long, Redis cache hot links, CDN for 301 redirects, rate limit create endpoint, analytics async via queue.

</details>

---

<details>
  <summary>Middle — Общие (architecture)</summary>

### Middle — Общие (architecture)

7. Расскажите о паттернах Observer, Pub / Sub. Какая между ними разница? Приведите примеры реализации этих паттернов в известных фреймворках / библиотеках / браузерных API.

</details>

<details>
  <summary>Общие (architecture)</summary>

### Общие (architecture)

4. Какой стек технологий можно выбрать для реализации клона какого-нибудь известного проекту и почему?

5. Имеется проект на старых технологиях, необходимо в него вносить изменения. Как это сделать лучше всего?

6. Если у кандидата есть опыт работы с несколькими фреймворками: какой будете использовать для следующего проекта? Какие факторы будут влиять на выбор?

</details>

</details>
