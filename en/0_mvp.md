# MVP & Engineering judgment — product vs architecture

When to ship fast, when to invest in structure, and how engineering decisions affect teams and scale.

### Related docs

- [System design](./0_system_design.md) · [Interview hub](./0_interview.md) · [AI-first approach](./0_ai.md#ai-first-approach) · [TypeScript](./5_ts.md) · [React architecture](./4_react_ecosystem.md#9-ecosystem--architecture)

### Main concepts

| Area            | Topics                                       |
| --------------- | -------------------------------------------- |
| **Product**     | MVP, validation, time-to-market              |
| **Engineering** | Data contracts, scalability, typing in teams |
| **Judgment**    | Tool selection, working code vs good code    |

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions on engineering judgment — common in senior and full-stack interviews.

### Question index

| Topic                     | Answer |
| ------------------------- | ------ |
| Data contract             | Q&A    |
| Scalability               | Q&A    |
| Typing in teams           | Q&A    |
| Choosing tools            | Q&A    |
| MVP vs architecture       | Q&A    |
| Working code vs good code | Q&A    |
| Technical documentation   | Q&A    |

---

## Questions & Answers

<details>
  <summary>What is a data contract?</summary>

### What is a data contract?

**Definition:** A **data contract** is the agreed shape and semantics of data exchanged between systems — API request/response, event payloads, database schema.

**How it works:**

- Frontend and backend share types (OpenAPI → TypeScript codegen, shared package).
- Versioning rules: additive changes are safe; removing/renaming fields is breaking.
- Runtime validation (Zod) at boundaries where types are erased.

**When to use:** REST API DTOs · Kafka event schemas · GraphQL schema as contract.

**Thoughts:** _Contract = both sides know what to send and expect — types and docs enforce it._

**See also:** [REST, Principles & Contract](./6_web_api_rest.md#rest-principles-and-contract) · [5_ts.md](./5_ts.md)

</details>

<details>
  <summary>Why does scalability matter?</summary>

### Why does scalability matter?

**Definition:** Scalability is the ability to handle growth — more users, data, traffic, or features — without proportional cost or quality collapse.

**Why it matters:**

- Traffic spikes (launch, marketing) can overwhelm unscaled systems.
- Data growth affects query performance without indexing/partitioning.
- Team growth needs modular architecture to parallelize work.

**Interview framing:** Distinguish **vertical** (bigger server) vs **horizontal** (more instances + load balancer). Mention caching, CDN, async queues, DB read replicas.

**Thoughts:** _Scale when you have evidence of need — but design so you're not blocked when growth arrives._

**See also:** [0_system_design.md — Load balancing, caching](./0_system_design_interview.md#interview-practice)

</details>

<details>
  <summary>Why is typing important in a team?</summary>

### Why is typing important in a team?

**Definition:** Static types (TypeScript) act as machine-checked documentation and refactor safety across module boundaries.

| Benefit                | Team impact                                  |
| ---------------------- | -------------------------------------------- |
| Safer refactors        | Rename field → compiler finds all usages     |
| Clear APIs             | Function signatures replace tribal knowledge |
| Fewer integration bugs | Frontend/backend share generated types       |
| Faster onboarding      | IDE autocomplete explains the codebase       |

**Thoughts:** _Types scale communication — critical when more than one person touches the code._

**See also:** [5_ts.md — Why TypeScript in large projects](./5_ts_interview.md#interview-practice)

</details>

<details>
  <summary>How do you choose tools for a project?</summary>

### How do you choose tools for a project?

**Definition:** Tool selection balances requirements, team skills, ecosystem, and maintenance cost.

**Framework:**

1. **Requirements** — SEO? Real-time? Mobile? Team size?
2. **Team familiarity** — fastest delivery with known stack
3. **Ecosystem** — hiring, libraries, community, LTS support
4. **Operational cost** — hosting, monitoring, CI complexity
5. **Exit cost** — how hard to migrate away later

**Examples:**

| Need                | Reasonable choice                   |
| ------------------- | ----------------------------------- |
| SEO marketing site  | Next.js or Nuxt SSR/SSG             |
| Internal admin SPA  | Vite + React or Vue                 |
| Typed API boundary  | OpenAPI + orval + Zod               |
| Simple global state | Zustand over Redux if team is small |

**Thoughts:** _Best tool = fits constraints and team — not the trendiest name on Twitter._

</details>

<details>
  <summary>When does MVP beat architecture?</summary>

### When does MVP beat architecture?

**Definition:** An **MVP** (Minimum Viable Product) validates a hypothesis with the smallest build that delivers learning — architecture can wait until the idea is proven.

**MVP wins when:**

- Market fit is unknown — speed of learning > perfect code
- Resources are limited — one developer, short runway
- Feature set is exploratory — requirements will change radically

**Invest in architecture when:**

- Product is validated and scaling
- Multiple developers need clear module boundaries
- Compliance, security, or uptime SLAs apply
- The bet is **[AI-first](./0_ai.md#ai-first-approach)** — a clickable UI without a model loop + evals is a demo, not an MVP

**Thoughts:** _MVP = validate; architecture = sustain — don't gold-plate before you know what to build._

</details>

<details>
  <summary>Working code vs good code?</summary>

### Working code vs good code?

**Definition:** **Working code** passes tests and ships features. **Good code** is also readable, maintainable, testable, and changeable by the next developer.

| Working code        | Good code                                |
| ------------------- | ---------------------------------------- |
| Solves today's task | Solves today's task + tomorrow's changes |
| May hide complexity | Names and structure reveal intent        |
| Copy-paste OK once  | DRY where duplication hurts              |
| No tests            | Tests for critical paths                 |
| Tight coupling      | Clear boundaries (layers, modules)       |

**Interview answer:** _"Working code ships the feature; good code lets the team ship the next ten features without fear."_

**Principles:** SOLID, small functions, meaningful names, tests at boundaries, delete dead code.

**Thoughts:** _Good code is working code your teammate can change confidently._

**See also:** [SOLID](./7_special_engineering.md#9-oop-and-solid)

</details>

<details>
  <summary>What technical documentation do you actually write?</summary>

### What technical documentation do you actually write?

**Definition:** Docs that a teammate can use without Slack: **README** (run locally), **ADRs** (why we chose X), **API contract** (OpenAPI), **runbooks** (how to deploy/rollback), **inline types** (TS as docs).

**How it works:** Write the decision and the command. Prefer a 20-line ADR over a wiki novel. Codegen (orval) keeps API docs from rotting. Update the runbook in the same PR as the pipeline change.

| Pros                              | Cons                                |
| --------------------------------- | ----------------------------------- |
| Onboarding and incident speed     | Docs rot if not next to the change  |
| ADRs stop re-litigating decisions | Over-documenting an MVP wastes time |

**When to use:** New hire day-one. 3am rollback. Interview: “I write ADRs for irreversible choices and keep OpenAPI as the contract.”

**Thoughts:** Docs live next to the code that they describe.

</details>
