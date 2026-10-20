# Full-stack engineer — competency map

What a strong **full-stack** role expects: backend and **SQL**, **HTTP/API** and **integrations**, confident **frontend** changes when the contract shifts, **production diagnosis** (logs, metrics, behavior), **delivery hygiene** (Git, CI/CD, Docker, DB migrations), **secrets and access**, and **design patterns** applied in real code. Linked topics hold the deep essays — here you get **order of study**, checklists, and interview-level detail.

**Suggested study order:** (1) [Full-stack delivery scope](#full-stack-delivery-scope) → (2) one backend track ([SQL](./9_database_sql.md) + [Node HTTP](./8_nodejs_http.md)) → (3) [API client & integration](./4_react_api_client.md) → (4) [Delivery hygiene](#delivery-hygiene-git-ci-docker-migrations) + [Secrets](#secrets-and-access-control) → (5) [Production diagnosis](#production-diagnosis) → (6) [Design patterns](#design-patterns-in-practice) → (7) [AI-first engineering & agents](./0_ai_engineering.md).

### Related docs

- [AI-first engineering & SDD](./0_ai_engineering.md) · [AI-assisted workflow (Claude Code, Codex)](./0_ai.md#ai-assisted-workflow-cursor-copilot-claude-code) · [MVP & judgment](./0_mvp.md) · [Behavior interview](./0_behavior_questions.md) · [Interview hub](./0_interview.md)

### Main concepts

| Area           | Topics                                                         |
| -------------- | -------------------------------------------------------------- |
| **Scope**      | [Full-stack delivery scope](#full-stack-delivery-scope)        |
| **Production** | [Production diagnosis](#production-diagnosis)                  |
| **Platform**   | [Delivery hygiene](#delivery-hygiene-git-ci-docker-migrations) |
| **Security**   | [Secrets and access control](#secrets-and-access-control)      |
| **Design**     | [Design patterns in practice](#design-patterns-in-practice)    |

### Sections

- [Full-stack delivery scope](#full-stack-delivery-scope)
- [Production diagnosis](#production-diagnosis)
- [Delivery hygiene (Git, CI, Docker, migrations)](#delivery-hygiene-git-ci-docker-migrations)
- [Secrets and access control](#secrets-and-access-control)
- [Design patterns in practice](#design-patterns-in-practice)
- [Interview Practice](#interview-practice)

---

## Full-stack delivery scope

<details>
<summary>Full-stack delivery scope</summary>

**Definition:** **Full-stack delivery** means you can own a **vertical slice**: change **persistence** (SQL/schema), **server behavior** (API, auth, integrations), and **client UI** (React/Next) so the feature ships end-to-end — without treating the other layer as a black box.

**How it works:**

**Typical slice (example: “add discount code to checkout”):**

1. **Data** — table/column or JSON field; migration up/down ([Migrations & seeds](./9_database_core.md#migrations--seeds)); constraints and indexes in [SQL](./9_database_sql.md).
2. **Backend** — validate code, apply price rules, idempotent POST; status codes and error shape ([REST](./6_web_api_rest.md), [Node HTTP](./8_nodejs_http.md)).
3. **Contract** — OpenAPI or shared types ([TS — API contracts](./5_ts_api_contracts.md)); versioning rules if mobile/web share API.
4. **Integration** — payment or catalog service via HTTP or queue ([Message brokers](./12_message_broker.md), [Backend integration](./4_react_backend_integration.md)).
5. **Frontend** — form state, loading/error, optimistic UI if needed ([API client](./4_react_api_client.md), [React core](./4_react_core.md)).
6. **Verify** — unit tests on rules, integration test on API, E2E or manual on UI ([Testing pyramid](./7_special_testing.md)).

| Skill area                  | Handbook depth                                                                                                      |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| SQL, transactions, schema   | [DB — SQL](./9_database_sql.md) · [DB — Core](./9_database_core.md) · [ORM](./9_database_orm.md)                    |
| REST/HTTP, auth, CORS       | [HTTP](./6_web_api_http.md) · [REST](./6_web_api_rest.md) · [Auth](./6_web_api_auth.md)                             |
| Node/Express-style APIs     | [Node — HTTP](./8_nodejs_http.md) · [Express Q&A](./8_express_interview.md)                                         |
| Gateway, BFF, microservices | [React — Backend integration](./4_react_backend_integration.md) · [Architecture](./0_system_design_architecture.md) |
| Real-time / async           | [Web API — apps](./6_web_api_apps.md) · [Brokers](./12_message_broker.md)                                           |

**When to use:** JD lines about backend **and** necessary frontend changes; bugs that cross API boundary; greenfield features from DB to UI.

**Pluses and minuses:**

| +                                              | −                                                     |
| ---------------------------------------------- | ----------------------------------------------------- |
| Fewer handoffs; faster root-cause on bugs      | Breadth — schedule time to go deep on one track       |
| Better API design when you feel UI constraints | Easy to skip migrations or tests on “small” UI tweaks |

**Examples:** API returns `409` for expired code → frontend maps to inline field error (not generic toast); migration adds nullable column first, backfill, then NOT NULL — zero-downtime pattern.

**Thoughts:** “Full-stack” here = **ship the slice**, not memorize every framework. Use the six-step loop as a mental checklist on every ticket.

**Sources:** [MDN — HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

</details>

## Production diagnosis

<details>
<summary>Production diagnosis</summary>

**Definition:** **Production diagnosis** is explaining user-visible **failures or slowness** using **logs**, **metrics**, **traces**, and **runtime behavior** — correlating browser, API, and data layer — not only `localhost`.

**How it works:**

**Triage playbook:**

| Step           | You do                                                     | Signals / tools                                                       |
| -------------- | ---------------------------------------------------------- | --------------------------------------------------------------------- |
| 1. Scope       | Time range, env (`prod`/`staging`), deploy version, region | Release tag in Sentry, CI deploy log                                  |
| 2. User impact | Error rate, p95 latency, ticket count                      | APM dashboard, [DevOps — Observability](./15_devops.md#observability) |
| 3. Frontend    | Stack, route, breadcrumbs, replay                          | [React — Sentry/RUM](./4_react_observability.md), Web Vitals          |
| 4. API         | Status codes, upstream timeouts, `trace_id`                | Gateway/access logs, OpenTelemetry spans                              |
| 5. Data        | Slow queries, pool exhaustion, deadlocks                   | DB logs, `EXPLAIN`, connection metrics                                |
| 6. Fix + guard | Patch, test, alert on recurrence                           | New monitor; link issue → PR                                          |

**Correlation:** Propagate one id (`X-Request-Id`, W3C `traceparent`) from browser → BFF → services → DB so one support ticket maps to one log trail.

**Behavioral clues:** “Spinner forever” often = unresolved promise or missing error state; “works after refresh” = stale cache or auth token edge case — reproduce with same cache headers and flags.

**When to use:** On-call, post-release regressions, “only in Safari”, payment failures, INP/LCP spikes.

**Pluses and minuses:**

| +                                | −                                          |
| -------------------------------- | ------------------------------------------ |
| Correlation ids shorten MTTR     | Alert fatigue without SLO-based thresholds |
| Metrics catch silent degradation | No prod log access blocks backend steps    |

**Examples:** Checkout 502 → gateway log shows upstream timeout → order service thread pool saturated → increase pool + queue depth metric + load test.

**Thoughts:** Learn [Operating in production](./4_react_observability.md#operating-in-production) for SPA loop; pair with backend observability — half of full-stack incidents sit on the wire or in SQL.

**Sources:** [OpenTelemetry](https://opentelemetry.io/docs/) · [Sentry — docs](https://docs.sentry.io/) · [web.dev — Web Vitals](https://web.dev/articles/vitals)

</details>

## Delivery hygiene (Git, CI, Docker, migrations)

<details>
<summary>Delivery hygiene (Git, CI, Docker, migrations)</summary>

**Definition:** **Delivery hygiene** is shipping through repeatable **Git** workflow, **CI/CD** gates, **containers**, and **versioned schema migrations** — so prod matches what reviewers approved in PR.

**How it works:**

**Git (daily):**

- Branch from `main`/`dev`; small commits; PR description = mini-spec ([SDD](./0_ai_engineering.md#spec-driven-development-sdd)).
- Rebase or merge per team rule; never force-push shared branches ([Branching](./0_git_branching.md), [Team](./0_git_team.md)).

**CI/CD (every merge):**

- Install with lockfile (`npm ci`); lint + unit + build; optional E2E on preview ([CI/CD](./10_build_ci.md)).
- Failed pipeline blocks deploy; artifacts (Docker image, static bundle) tagged with commit SHA.
- Deploy stages: test → staging → prod with manual or automated promotion ([Pipeline stages](./10_build_ci.md#cicd-pipeline-stages)).

**Docker (when services run in containers):**

- Multi-stage build; non-root user; `.dockerignore`; healthcheck ([Dockerfile](./10_build_docker.md)).
- Compose for local stack (app + Postgres + Redis); same env var names as prod where possible.

**DB migrations (never “just run SQL in prod”):**

- One migration per logical change; reviewed like code; applied by CI/CD or release job ([Migrations & seeds](./9_database_core.md#migrations--seeds)).
- Prefer backward-compatible steps: add column nullable → deploy app reading both → backfill → enforce NOT NULL in later migration.
- Tools: Prisma migrate, Sequelize CLI, Knex, Liquibase — same idea: ordered changelog, track applied versions ([ORM](./9_database_orm.md)).

| Risk                  | Prevention                                          |
| --------------------- | --------------------------------------------------- |
| Drift staging/prod    | Same migration job in all envs                      |
| Long table lock       | Online migration patterns; run heavy DDL off-peak   |
| Rollback without plan | Know whether app rollback also needs migration down |

**When to use:** Every professional repo; any interview question on “how does code reach prod?”

**Pluses and minuses:**

| +                                    | −                                       |
| ------------------------------------ | --------------------------------------- |
| Auditable history (git + migrations) | Pipeline YAML and Docker learning curve |
| CI catches break before users        | Bad migration still needs DBA review    |

**Thoughts:** Full-stack engineers touch migrations and CI when adding fields or env vars — coordinate with platform team, but **own** the slice through green pipeline.

**Sources:** [GitHub Actions — workflow](https://docs.github.com/en/actions/using-workflows) · [Liquibase — concepts](https://docs.liquibase.com/concepts/introduction-to-liquibase.html)

</details>

## Secrets and access control

<details>
<summary>Secrets and access control</summary>

**Definition:** **Secrets and access control** means credentials (API keys, DB URLs, tokens, private keys) and **permissions** (IAM, repo access, production roles) are stored, rotated, and scoped so they never leak via git, logs, chat, or client bundles.

**How it works:**

| Secret type         | Store                                                         | Never                                                          |
| ------------------- | ------------------------------------------------------------- | -------------------------------------------------------------- |
| CI deploy tokens    | Platform secret store (`secrets.*` in [CI](./10_build_ci.md)) | Hardcode in workflow YAML committed values                     |
| App runtime config  | Env vars / Secrets Manager / Vercel env                       | Commit `.env.production`                                       |
| DB password         | Secret store; inject at runtime                               | `ENV` in Dockerfile layer with real password                   |
| OAuth client secret | Server only                                                   | `NEXT_PUBLIC_*`, mobile/web bundle                             |
| User session        | HttpOnly cookie / server session                              | `localStorage` for bearer tokens ([Auth](./6_web_api_auth.md)) |

**Access (least privilege):**

- Production DB: role per service, not shared admin; read-only replicas for analytics.
- Cloud IAM: engineer SSO; break-glass for prod; audit trail ([DevOps — AWS secrets](./15_devops.md)).
- Git: branch protection; required reviews; no long-lived PAT in agent prompts ([AI agents](./0_ai_engineering.md#coding-agents-on-real-projects)).

**Rotation:** When key leaks or person leaves — rotate secret, invalidate old, redeploy; check audit logs.

**Agent-specific:** Do not paste `.env`, SSH keys, or customer PII into Claude Code/Codex/Cursor chat; use redacted examples in specs.

**When to use:** Every feature touching payments, auth, or prod data; compliance questions; incident “key exposed on GitHub”.

**Pluses and minuses:**

| +                               | −                                        |
| ------------------------------- | ---------------------------------------- |
| Scoped creds limit blast radius | Rotation breaks forgotten cron jobs      |
| Secret stores integrate with CI | Misconfigured `NEXT_PUBLIC_` still ships |

**Examples:** Stripe secret key only in server Route Handler; frontend gets publishable key; webhook signing secret verified server-side only.

**Thoughts:** Treat **access** like **secrets** — if every engineer has prod DB write, one bad migration or agent run can drop data.

**Sources:** [GitHub — encrypted secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) · [OWASP — Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)

</details>

## Design patterns in practice

<details>
<summary>Design patterns in practice</summary>

**Definition:** **Design patterns** name recurring **design problems** and proven structures (GoF: creational, structural, behavioral). **Applying** them means you match problem → pattern → code shape — not decorating classes with pattern names.

**How it works:**

**Problem → pattern (common in full-stack work):**

| Problem                                          | Pattern / approach                | Where in handbook                                                               |
| ------------------------------------------------ | --------------------------------- | ------------------------------------------------------------------------------- |
| Create object without coupling to concrete class | Factory / Abstract Factory        | [GoF — creational](./7_special_patterns.md)                                     |
| Wrap legacy or third-party API                   | Adapter, Facade                   | [GoF — structural](./7_special_patterns.md)                                     |
| Swap algorithms at runtime                       | Strategy                          | [GoF — behavioral](./7_special_patterns.md)                                     |
| Notify many listeners                            | Observer (often → events/pub-sub) | [GoF — behavioral](./7_special_patterns.md) · [Brokers](./12_message_broker.md) |
| Single shared instance (use sparingly)           | Singleton                         | Prefer DI / module scope in Node                                                |
| UI composition without prop drilling             | Compound components, context      | [React patterns](./4_react_patterns.md)                                         |
| Layer boundaries                                 | MVC, hexagonal, BFF               | [Architecture](./7_special_architecture.md)                                     |

**Process:** (1) Name the smell (duplicate branching, tight coupling). (2) Check SOLID ([Engineering](./7_special_engineering.md)). (3) Pick smallest pattern that clarifies intent. (4) Add test at new seam.

**When to use:** Refactors, code review, system design interviews; avoid introducing patterns in code that will never change.

**Pluses and minuses:**

| +                                         | −                                               |
| ----------------------------------------- | ----------------------------------------------- |
| Shared vocabulary across backend/frontend | Over-abstraction before second use case         |
| Test doubles easier with Strategy/DI      | Singleton/global state hides dependencies in JS |

**Examples:** Payment providers (Stripe vs Adyen) → Strategy behind `PaymentProcessor` interface; webhook parsers → Adapter per vendor payload shape.

**Thoughts:** Frontend “patterns” are often **composition**; backend “patterns” often **decouple domain from IO** — read both [React patterns](./4_react_patterns.md) and [GoF catalog](./7_special_patterns.md).

**Sources:** [Refactoring Guru — Design Patterns](https://refactoring.guru/design-patterns) · [patterns.dev — React](https://www.patterns.dev/react/)

</details>

## Interview Practice

### Questions & Answers

**Q: What does “full-stack” mean in this handbook?**  
A: Own a vertical slice — SQL/schema, API/integration, frontend — with links to deep tracks, not one giant essay.

**Q: How do you add a DB column and ship safely?**  
A: Versioned migration in PR, backward-compatible deploy order, CI applies migration; see [Delivery hygiene](#delivery-hygiene-git-ci-docker-migrations) and [Migrations & seeds](./9_database_core.md#migrations--seeds).

**Q: Where do secrets go?**  
A: [Secrets and access control](#secrets-and-access-control) — server/CI store only; never client bundle or agent chat.

**Q: How do you debug prod without local repro?**  
A: [Production diagnosis](#production-diagnosis) — correlate release, trace id, Sentry + server logs + DB metrics.

**Q: Strategy vs if/else chain?**  
A: Strategy when behaviors vary as a set and you test in isolation; if/else fine for two stable branches — see [Design patterns](#design-patterns-in-practice).

**Q: Claude Code / Codex on real projects?**  
A: [Coding agents on real projects](./0_ai_engineering.md#coding-agents-on-real-projects) — branch, spec, CI, no secrets in prompts.

**Q: AI-first engineering vs this map?**  
A: This map is **stack and ops** literacy; [AI-first engineering](./0_ai_engineering.md) is **how you delegate and verify** with agents on that stack.
