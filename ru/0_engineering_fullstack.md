# Full-stack engineer — competency map

Что обычно ждут от сильной **full-stack** роли: backend и **SQL**, **HTTP/API** и **интеграции**, уверенные правки во **frontend** при смене контракта, **диагностика в проде** (логи, метрики, поведение), **гигиена поставки** (Git, CI/CD, Docker, миграции БД), **секреты и доступы**, **паттерны проектирования** в реальном коде. Глубокие темы — в связанных заметках; здесь — **порядок обучения**, чеклисты и деталь уровня интервью.

**Suggested study order:** (1) [Full-stack delivery scope](#full-stack-delivery-scope) → (2) backend ([SQL](./9_database_sql.md) + [Node HTTP](./8_nodejs_http.md)) → (3) [API client & integration](./4_react_api_client.md) → (4) [Delivery hygiene](#delivery-hygiene-git-ci-docker-migrations) + [Secrets](#secrets-and-access-control) → (5) [Production diagnosis](#production-diagnosis) → (6) [Design patterns](#design-patterns-in-practice) → (7) [AI-first engineering & agents](./0_ai_engineering.md).

### Related docs

- [AI-first engineering & SDD](./0_ai_engineering.md) · [AI-assisted workflow (Claude Code, Codex)](./0_ai.md#ai-assisted-workflow-cursor-copilot-claude-code) · [MVP & judgment](./0_mvp.md) · [Behavior interview](./0_behavior_questions.md) · [Interview hub](./0_interview.md) · [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [Frontend performance map](./6_web_api_performance.md#frontend-performance-techniques-study-map)

### Main concepts

| Area           | Topics                                                                                                                                                                                                               |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Scope**      | [Full-stack delivery scope](#full-stack-delivery-scope)                                                                                                                                                              |
| **Production** | [Production diagnosis](#production-diagnosis)                                                                                                                                                                        |
| **Platform**   | [Delivery hygiene](#delivery-hygiene-git-ci-docker-migrations)                                                                                                                                                       |
| **Security**   | [Secrets and access control](#secrets-and-access-control)                                                                                                                                                            |
| **Design**     | [Design patterns in practice](#design-patterns-in-practice)                                                                                                                                                          |
| **Career**     | [Frontend developer demand (2026)](#frontend-developer-demand-2026) · [How long to learn frontend](#how-long-to-learn-frontend-development) · [Senior → staff engineer](#senior-frontend-engineer-to-staff-engineer) |

### Sections

- [Full-stack delivery scope](#full-stack-delivery-scope)
- [Production diagnosis](#production-diagnosis)
- [Delivery hygiene (Git, CI, Docker, migrations)](#delivery-hygiene-git-ci-docker-migrations)
- [Secrets and access control](#secrets-and-access-control)
- [Design patterns in practice](#design-patterns-in-practice)
- [Frontend developer demand (2026)](#frontend-developer-demand-2026)
- [How long to learn frontend development](#how-long-to-learn-frontend-development)
- [Senior frontend engineer to staff engineer](#senior-frontend-engineer-to-staff-engineer)
- [Interview Practice](#interview-practice)

---

## Full-stack delivery scope

<details>
<summary>Full-stack delivery scope</summary>

**Definition:** **Full-stack delivery** — ты ведёшь **вертикальный срез**: меняешь **persistence** (SQL/схема), **поведение сервера** (API, auth, интеграции) и **клиентский UI** (React/Next), чтобы фича доехала end-to-end — без «чёрного ящика» на другой стороне.

**How it works:**

**Типичный срез (пример: «промокод в checkout»):**

1. **Data** — таблица/поле; migration up/down ([Migrations & seeds](./9_database_core.md#migrations--seeds)); constraints и indexes в [SQL](./9_database_sql.md).
2. **Backend** — валидация кода, правила цены, идempotent POST; коды ошибок ([REST](./6_web_api_rest.md), [Node HTTP](./8_nodejs_http.md)).
3. **Contract** — OpenAPI или shared types ([TS — API contracts](./5_ts_api_contracts.md)); versioning, если API общий для web/mobile.
4. **Integration** — payment/catalog через HTTP или очередь ([Brokers](./12_message_broker.md), [Backend integration](./4_react_backend_integration.md)).
5. **Frontend** — form state, loading/error, optimistic UI при необходимости ([API client](./4_react_api_client.md), [React core](./4_react_core.md)).
6. **Verify** — unit на правила, integration на API, E2E или manual на UI ([Testing pyramid](./7_special_testing.md)).

| Skill area                  | Глубина в handbook                                                                                                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| SQL, transactions, schema   | [DB — SQL](./9_database_sql.md) · [DB — Core](./9_database_core.md) · [ORM](./9_database_orm.md)                    |
| REST/HTTP, auth, CORS       | [HTTP](./6_web_api_http.md) · [REST](./6_web_api_rest.md) · [Auth](./6_web_api_auth.md)                             |
| Node/Express API            | [Node — HTTP](./8_nodejs_http.md) · [Express Q&A](./8_express_interview.md)                                         |
| Gateway, BFF, microservices | [React — Backend integration](./4_react_backend_integration.md) · [Architecture](./0_system_design_architecture.md) |
| Real-time / async           | [Web API — apps](./6_web_api_apps.md) · [Brokers](./12_message_broker.md)                                           |

**When to use:** Вакансии с backend **и** нужным frontend; баги на границе API; фичи от БД до UI.

**Pluses and minuses:**

| +                                  | −                                                   |
| ---------------------------------- | --------------------------------------------------- |
| Меньше handoff, быстрее root-cause | Широта — закладывай время на deep dive в один track |
| Лучше API при понимании UI         | Легко пропустить migration/tests на «мелком» UI     |

**Examples:** API отдаёт `409` для просроченного кода → frontend показывает ошибку у поля; migration: nullable column → backfill → NOT NULL позже.

**Thoughts:** Full-stack здесь = **ship the slice**, не «знать все фреймворки». Шесть шагов — чеклист на каждый тикет.

**Sources:** [MDN — HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

</details>

## Production diagnosis

<details>
<summary>Production diagnosis</summary>

**Definition:** **Production diagnosis** — объяснить **сбои или тормоза** у пользователей по **логам**, **метрикам**, **трейсам** и **поведению runtime**, связав browser, API и data layer — не только `localhost`.

**How it works:**

**Triage playbook:**

| Step           | Действие                                 | Сигналы / tools                                               |
| -------------- | ---------------------------------------- | ------------------------------------------------------------- |
| 1. Scope       | Окно времени, env, версия deploy, регион | Release в Sentry, лог CI                                      |
| 2. Impact      | Error rate, p95, тикеты                  | APM, [DevOps — Observability](./15_devops.md#observability)   |
| 3. Frontend    | Stack, route, breadcrumbs, replay        | [React observability](./4_react_observability.md), Web Vitals |
| 4. API         | Status, timeouts, `trace_id`             | Gateway logs, OpenTelemetry                                   |
| 5. Data        | Slow queries, pool, deadlocks            | DB logs, `EXPLAIN`, metrics                                   |
| 6. Fix + guard | Patch, test, alert                       | Monitor; issue → PR                                           |

**Correlation:** один id (`X-Request-Id`, W3C `traceparent`) от browser → BFF → services → DB.

**Behavioral clues:** «Вечный spinner» — часто unresolved promise; «помогает refresh» — cache или token — воспроизводи с теми же headers и flags.

**When to use:** On-call, регресс после релиза, «только Safari», payment, INP/LCP.

**Pluses and minuses:**

| +                              | −                                      |
| ------------------------------ | -------------------------------------- |
| Correlation id сокращает MTTR  | Alert fatigue без SLO                  |
| Метрики ловят тихую деградацию | Без prod logs backend шаг заблокирован |

**Examples:** Checkout 502 → timeout upstream → thread pool order service → scale pool + metric + load test.

**Thoughts:** [Operating in production](./4_react_observability.md#operating-in-production) для SPA; половина инцидентов — wire или SQL.

**Sources:** [OpenTelemetry](https://opentelemetry.io/docs/) · [Sentry](https://docs.sentry.io/) · [web.dev — Web Vitals](https://web.dev/articles/vitals)

</details>

## Delivery hygiene (Git, CI, Docker, migrations)

<details>
<summary>Delivery hygiene (Git, CI, Docker, migrations)</summary>

**Definition:** **Delivery hygiene** — поставка через повторяемый **Git**, **CI/CD**, **контейнеры** и **версионируемые миграции**, чтобы prod совпадал с тем, что одобрили в PR.

**How it works:**

**Git (ежедневно):**

- Ветка от `main`/`dev`; мелкие commits; PR = mini-spec ([SDD](./0_ai_engineering.md#spec-driven-development-sdd)).
- Rebase/merge по правилам команды; без force-push на shared ([Branching](./0_git_branching.md), [Team](./0_git_team.md)).

**CI/CD (каждый merge):**

- Lockfile (`npm ci`); lint + unit + build; E2E на preview при необходимости ([CI/CD](./10_build_ci.md)).
- Красный pipeline блокирует deploy; артефакты с SHA commit.
- Стадии: test → staging → prod ([Pipeline stages](./10_build_ci.md#cicd-pipeline-stages)).

**Docker:**

- Multi-stage; non-root; `.dockerignore`; healthcheck ([Dockerfile](./10_build_docker.md)).
- Compose локально (app + Postgres + Redis); те же имена env, что в prod, где возможно.

**DB migrations:**

- Одна migration на логическое изменение; review как код; применение через CI/release job ([Migrations & seeds](./9_database_core.md#migrations--seeds)).
- Backward-compatible: nullable column → deploy читает оба → backfill → NOT NULL позже.
- Prisma, Sequelize, Knex, Liquibase — ordered changelog ([ORM](./9_database_orm.md)).

| Risk               | Prevention                                       |
| ------------------ | ------------------------------------------------ |
| Drift staging/prod | Один migration job во всех env                   |
| Long lock          | Online migration; тяжёлый DDL off-peak           |
| Rollback           | Понимать, нужен ли migration down при откате app |

**When to use:** Любой prod repo; «как код попадает в prod?»

**Pluses and minuses:**

| +                          | −                                       |
| -------------------------- | --------------------------------------- |
| История в git + migrations | Кривая обучения CI/Docker               |
| CI ловит до пользователей  | Плохая migration нуждается в DBA review |

**Thoughts:** Full-stack трогает migrations и CI при новых полях/env — **own** slice до зелёного pipeline.

**Sources:** [GitHub Actions — workflow](https://docs.github.com/en/actions/using-workflows) · [Liquibase — concepts](https://docs.liquibase.com/concepts/introduction-to-liquibase.html)

</details>

## Secrets and access control

<details>
<summary>Secrets and access control</summary>

**Definition:** **Secrets and access control** — credentials (API keys, DB URLs, tokens, private keys) и **permissions** (IAM, доступ к repo, prod roles) хранятся, ротируются и ограничиваются так, чтобы не утекли через git, logs, chat или client bundle.

**How it works:**

| Secret type         | Store                                 | Never                                                 |
| ------------------- | ------------------------------------- | ----------------------------------------------------- |
| CI deploy tokens    | Secret store ([CI](./10_build_ci.md)) | Значения в committed YAML                             |
| App runtime         | Env / Secrets Manager / Vercel env    | Commit `.env.production`                              |
| DB password         | Secret store at runtime               | Пароль в слое Dockerfile                              |
| OAuth client secret | Только server                         | `NEXT_PUBLIC_*`, bundle                               |
| User session        | HttpOnly cookie / server session      | Bearer в `localStorage` ([Auth](./6_web_api_auth.md)) |

**Access (least privilege):**

- Prod DB: role на service, не общий admin; read replica для analytics.
- Cloud IAM: SSO; break-glass; audit ([DevOps](./15_devops.md)).
- Git: branch protection, review; без PAT в agent prompts ([Agents](./0_ai_engineering.md#coding-agents-on-real-projects)).

**Rotation:** утечка или уход человека — rotate, invalidate, redeploy; audit logs.

**Agent-specific:** не вставлять `.env`, SSH keys, PII в Claude Code/Codex/Cursor; redacted examples в spec.

**When to use:** Payments, auth, prod data; compliance; «key on GitHub».

**Pluses and minuses:**

| +                               | −                                 |
| ------------------------------- | --------------------------------- |
| Scoped creds ограничивают ущерб | Rotation ломает забытые cron      |
| Store интегрируется с CI        | `NEXT_PUBLIC_` всё равно публичен |

**Examples:** Stripe secret только в server Route Handler; publishable key на client; webhook secret проверяется server-side.

**Thoughts:** **Access** как **secrets** — если у всех write в prod DB, один bad migration или agent run может уничтожить данные.

**Sources:** [GitHub — encrypted secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) · [OWASP — Secrets Management](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)

</details>

## Design patterns in practice

<details>
<summary>Design patterns in practice</summary>

**Definition:** **Design patterns** именуют повторяющиеся **проблемы проектирования** и проверенные структуры (GoF). **Применение** — problem → pattern → shape кода, не декорация именами.

**How it works:**

**Problem → pattern:**

| Problem                                | Pattern / approach           | Handbook                                                           |
| -------------------------------------- | ---------------------------- | ------------------------------------------------------------------ |
| Создание без coupling к concrete class | Factory / Abstract Factory   | [GoF — creational](./7_special_patterns.md)                        |
| Legacy / third-party API               | Adapter, Facade              | [GoF — structural](./7_special_patterns.md)                        |
| Swap algorithms                        | Strategy                     | [GoF — behavioral](./7_special_patterns.md)                        |
| Много listeners                        | Observer → events/pub-sub    | [GoF](./7_special_patterns.md) · [Brokers](./12_message_broker.md) |
| Shared instance (осторожно)            | Singleton                    | Prefer DI / module scope в Node                                    |
| UI без prop drilling                   | Compound components, context | [React patterns](./4_react_patterns.md)                            |
| Границы слоёв                          | MVC, hexagonal, BFF          | [Architecture](./7_special_architecture.md)                        |

**Process:** (1) Smell. (2) SOLID ([Engineering](./7_special_engineering.md)). (3) Минимальный pattern. (4) Test на новом seam.

**When to use:** Refactor, review, system design; не pattern до второго use case.

**Pluses and minuses:**

| +                           | −                                 |
| --------------------------- | --------------------------------- |
| Общий язык backend/frontend | Over-abstraction до второго кейса |
| Test doubles с Strategy/DI  | Singleton/global state в JS       |

**Examples:** Payment providers → Strategy `PaymentProcessor`; webhooks → Adapter per vendor.

**Thoughts:** Frontend — **composition**; backend — **decouple domain from IO** — [React patterns](./4_react_patterns.md) + [GoF](./7_special_patterns.md).

**Sources:** [Refactoring Guru](https://refactoring.guru/design-patterns) · [patterns.dev — React](https://www.patterns.dev/react/)

</details>

## Frontend developer demand (2026)

<details>
<summary>Frontend developer demand (2026)</summary>

**Definition:** **Обзор с источниками** о рынке frontend в середине 2020‑х: суммарный спрос растёт, но **junior и generalist mid** проходят путь сложнее, чем senior и узкие специализации.

**How it works:**

| Signal                                                                                                           | What it measures                                                                    | Takeaway                                                                                  |
| ---------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| [BLS — Software developers](https://www.bls.gov/ooh/computer-and-information-technology/software-developers.htm) | Рост занятости в США ~**15%** **2024–2034**                                         | Долгосрочный спрос на разработчиков выше среднего — не «frontend умер».                   |
| [Stanford Digital Economy Lab](https://digitaleconomy.stanford.edu/publication/canaries-in-the-coal-mine/)       | Payroll ADP: 22–25 лет в **AI‑exposed** ролях ~**19% ниже** тренда vs менее exposed | **Относительный** разрыв для junior в software‑подобных ролях, не «−19% всех jobs».       |
| [JetBrains DevEco 2025](https://www.jetbrains.com/lp/devecosystem-2025/)                                         | ~**85%** используют AI; ~**44%** — реальная интеграция в workflow                   | Высокое **adoption ≠** полная перестройка процесса; **TS/React/тесты/CI** остаются базой. |

**Barbell (комментарии рекрутеров, не одно исследование):** конкуренция на **junior generalist**, слабее спрос на **mid generalist**, плотнее рынок **senior + specialists** (perf, a11y, design systems, full‑stack slice).

**When to use:** Выбор фокуса подготовки; интерпретация новостей о layoffs; depth vs смена фреймворков.

**Pluses and minuses:**

| +                                               | −                                                                   |
| ----------------------------------------------- | ------------------------------------------------------------------- |
| Именованные исследования лучше viral‑заголовков | Зарплатные «премии» в агрегаторах часто **ориентир**, не один аудит |
| Специализации востребованы                      | Доля US‑вакансий **≠** глобальная картина                           |

**Thoughts:** Держите в голове **и рост BLS, и squeeze junior**. Показывайте **ownership** (фича, тесты, perf, a11y), а не длинный список фреймворков.

**See also:** [How long to learn frontend](#how-long-to-learn-frontend-development) · [Senior → staff](#senior-frontend-engineer-to-staff-engineer) · [Performance map](./6_web_api_performance.md#frontend-performance-techniques-study-map)

**Sources:** [BLS](https://www.bls.gov/ooh/computer-and-information-technology/software-developers.htm) · [Stanford](https://digitaleconomy.stanford.edu/publication/canaries-in-the-coal-mine/) · [JetBrains](https://www.jetbrains.com/lp/devecosystem-2025/)

</details>

## How long to learn frontend development

<details>
<summary>How long to learn frontend development</summary>

**Definition:** **Честного одного числа нет** — зависит от **часов в неделю**, **bootcamp vs self‑taught** и цели: «первая страница» vs **junior job‑ready** (TS, React, тесты, a11y, портфолио).

**How it works:**

| Path                 | Typical elapsed time                    | Hours / week | Source                                                                                         |
| -------------------- | --------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------- |
| Full‑time bootcamp   | ~**2–7 мес.** (программы **6–28 нед.**) | ~**40+**     | [Course Report](https://www.coursereport.com/)                                                 |
| Part‑time bootcamp   | ~**6–9 мес.**                           | ~**20–30**   | Same                                                                                           |
| Self‑taught (median) | Нет метрики «до оффера»                 | ~**8**       | [freeCodeCamp 2021 New Coder Survey](https://www.freecodecamp.org/news/2021-new-coder-survey/) |

**Stages:** (1) fundamentals → (2) проекты без фреймворка → (3) framework + TS → (4) production + portfolio (здесь self‑taught чаще «растягивается»).

**Illustrative math:** ~**560 h** bootcamp ≈ **70 нед.** при **8 h/week** — если время так же сфокусировано, как в программе.

**Thoughts:** Stage **4** — с серьёзностью bootcamp. Рынок junior → [Demand (2026)](#frontend-developer-demand-2026).

**See also:** [Interview hub](./0_interview.md) · [CWV](./1_html_performance.md#core-web-vitals)

**Sources:** [Course Report](https://www.coursereport.com/) · [freeCodeCamp survey](https://www.freecodecamp.org/news/2021-new-coder-survey/)

</details>

## Senior frontend engineer to staff engineer

<details>
<summary>Senior frontend engineer to staff engineer</summary>

**Definition:** **Staff** — **другая роль**, не «senior дольше»: шире **техническое направление**, **стандарты**, **кросс‑командный** эффект и **доказанный** scope следующего уровня (лестницы компаний различаются).

**How it works:**

| Theme    | Senior-ish       | Staff-ish                                                                          |
| -------- | ---------------- | ---------------------------------------------------------------------------------- |
| Scope    | Цели команды     | Multi‑team / multi‑quarter (читайте **свою** лестницу)                             |
| Coding   | Много фич        | Меньше кода; review, standards, glue ([staffeng.com](https://staffeng.com/guides)) |
| Evidence | Сильная delivery | **Задокументированный** impact; не чеклист «надеемся вырастет»                     |

**Archetypes (Larson):** Tech lead, Architect (область, напр. frontend stack), Solver, Right hand. TL — часто **первая** staff‑форма, но TL‑обязанности **≠** staff impact.

**Thoughts:** Glue work делайте **видимым** ([Reilly — glue](https://noidea.dog/glue)). Frontend staff возможен через **организационный** рычаг, не только PR throughput.

**See also:** [Full-stack scope](#full-stack-delivery-scope) · [Behavior — career](./0_behavior_questions.md#how-could-you-have-improved-your-career-progress)

**Sources:** [staffeng.com](https://staffeng.com/guides) · [GitLab career framework](https://handbook.gitlab.com/job-families/engineering/development/)

</details>

## Interview Practice

### Questions & Answers

**Q: Что здесь «full-stack»?**  
A: Vertical slice — SQL, API, frontend — с ссылками на deep tracks.

**Q: Как безопасно добавить колонку в БД?**  
A: Migration в PR, backward-compatible deploy, CI applies — [Delivery hygiene](#delivery-hygiene-git-ci-docker-migrations), [Migrations](./9_database_core.md#migrations--seeds).

**Q: Куда класть secrets?**  
A: [Secrets and access control](#secrets-and-access-control) — server/CI; не bundle и не agent chat.

**Q: Debug prod без local repro?**  
A: [Production diagnosis](#production-diagnosis) — release, trace id, Sentry + server + DB.

**Q: Strategy vs if/else?**  
A: Strategy при наборе вариантов и тестах; if/else для двух стабильных веток — [Design patterns](#design-patterns-in-practice).

**Q: Claude Code / Codex на проектах?**  
A: [Coding agents](./0_ai_engineering.md#coding-agents-on-real-projects) — branch, spec, CI, без secrets в prompts.

**Q: AI-first vs эта карта?**  
A: Карта — **stack и ops**; [AI-first engineering](./0_ai_engineering.md) — **делегирование и verify** с агентами.
