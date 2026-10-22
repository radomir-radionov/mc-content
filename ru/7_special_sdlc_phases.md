# Этапы SDLC

Шесть классических этапов жизненного цикла разработки ПО — от планирования до поддержки — и связь с Agile и AI-augmented delivery.

### Related docs

- [SDLC principles](./7_special_sdlc_principles.md) · [SDLC & Agile](./7_special_sdlc.md) · [Agentic SDLC (AI)](./0_ai_agentic_sdlc.md) · [Testing fundamentals](./7_special_testing_fundamentals.md) · [CI/CD](./10_build_ci.md) · [DevOps](./15_devops.md)

### Main concepts

| Area       | Topics                                                                                                                                                                                                                            |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Phases** | [Overview](#overview)                                                                                                                                                                                                             |
| **Stages** | [Planning](#planning) · [Requirements analysis](#requirements-analysis) · [Design](#design) · [Development](#development) · [Testing and deployment](#testing-and-deployment) · [Support](#support) |

### Sections

- [Overview](#overview)
- [Planning](#planning)
- [Requirements analysis](#requirements-analysis)
- [Design](#design)
- [Development](#development)
- [Testing and deployment](#testing-and-deployment)
- [Support](#support)
- [Interview Practice](#interview-practice)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **SDLC phases (этапы SDLC)** — основные стадии: **planning, requirements analysis, design, development, testing & deployment, support**. В [Waterfall](./7_special_sdlc.md#waterfall) они идут последовательно; в [Agile](./7_special_sdlc.md#agile) **перекрываются и повторяются** в коротких итерациях.

**How it works:**

| Step                | What you're doing                                                       |
| ------------------- | ----------------------------------------------------------------------- |
| **Planning**        | Цели, scope, сроки, команда, бюджет — _какая инициатива и зачем?_       |
| **Requirements**    | Что продукт должен делать — потребности пользователей, критерии приёмки |
| **Design**          | Как это устроено — архитектура, API, UI, модель данных                  |
| **Development**     | Реализация — код, конфигурация, интеграции                              |
| **Test and deploy** | Проверка качества и выкладка пользователям                              |
| **Support**         | Эксплуатация — баги, инциденты, мелкие доработки, мониторинг            |

Сверху вниз — классическая цепочка: **plan → specify → design → build → ship → operate**. После **Support** срабатывает [**cyclicity**](./7_special_sdlc_principles.md#cyclicity): цикл возвращается к **Planning**, обратная связь из prod запускает следующую итерацию работ.

У каждой фазы свой фокус управления: scope/time/cost (planning), traceability (requirements), architecture & UX (design), implementation (development), quality & release (testing/deployment), operations (support).

Опора: [принципы SDLC](./7_special_sdlc_principles.md). ИИ: [AI augmentation of SDLC phases](./7_special_sdlc.md#ai-augmentation-of-sdlc-phases).

**When to use:** Собеседования, шаблоны документации, согласование терминов с [методологией](./7_special_sdlc.md#software-design-methodology).

**Pluses and minuses:**

| +                                         | −                                                   |
| ----------------------------------------- | --------------------------------------------------- |
| Универсальная карта для любой методологии | Названия и границы фаз различаются по компаниям     |
| Удобно привязать deliverables             | Agile-командам может не нравиться язык «phase gate» |

**Thoughts:** В учебниках testing и deployment часто в одной фазе; в runbook лучше разделить **sign-off качества** и **push в prod**.

**Sources:** [Doka — Agile](https://doka.guide/tools/agile/) · [Cyclicity](./7_special_sdlc_principles.md#cyclicity)

</details>

## Planning

<details>
<summary>Planning</summary>

**Definition:** **Planning (планирование)** — фаза **управления проектом**: цели, scope, сроки, бюджет, ресурсы, роли команды и высокоуровневый график до детальной проработки требований.

**How it works:**

- Результаты: план проекта, milestones, распределение ресурсов, коммуникации, начальный реестр рисков ([risk management](./7_special_sdlc_principles.md#risk-management)).
- Питает [requirements analysis](#requirements-analysis) приоритетами и ограничениями.
- В Agile planning **непрерывен** (release/sprint planning).

**When to use:** Новые продукты, крупные инициативы, fixed-price; для мелких фиксов — облегчённый planning.

**Pluses and minuses:**

| +                        | −                                              |
| ------------------------ | ---------------------------------------------- |
| Согласование feasibility | Over-planning до learning (критика Lean/Agile) |

**Examples:** Выбор Scrum vs Kanban; scope MVP первого релиза ([MVP](./0_mvp.md)).

**Sources:** [Software design methodology](./7_special_sdlc.md#software-design-methodology)

</details>

## Requirements analysis

<details>
<summary>Requirements analysis</summary>

**Definition:** **Requirements analysis (анализ требований)** — сбор и документирование **что** система должна делать для пользователей и бизнеса; основа для design, тестов и приёмки.

**How it works:**

- Интервью, воркшопы, анализ as-is, user stories, use cases, NFR (performance, security).
- Deliverables: ТЗ/спека, acceptance criteria, трассировка к тестам.
- Связь с [user focus](./7_special_sdlc_principles.md#user-focus) и [SDD](./0_ai_engineering.md#spec-driven-development-sdd).
- В [agentic SDLC](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles) **analyst agent** черновит артефакт; человек апрувит на [gateway](./0_ai_agentic_sdlc.md#human-gateways-in-agentic-sdlc).

**When to use:** Любая нетривиальная фича; regulated — сильная traceability.

**Pluses and minuses:**

| +                            | −                                         |
| ---------------------------- | ----------------------------------------- |
| Меньше «не того» функционала | Analysis paralysis при нестабильном scope |

**Thoughts:** Ошибки требований — самые дешёвые для исправления здесь, самые дорогие в prod.

**Sources:** [Spec-driven development (SDD)](./0_ai_engineering.md#spec-driven-development-sdd)

</details>

## Design

<details>
<summary>Design</summary>

**Definition:** **Design (проектирование и дизайн)** — **техническая архитектура** и **UX/UI**: данные, интерфейсы, компоненты, алгоритмы, спецификации для разработки.

**How it works:**

- Technical: API, схема БД, интеграции, security — [Architecture patterns](./7_special_architecture.md).
- UX/UI: wireframes, прототипы, design system, [a11y](./1_html_a11y.md).
- Deliverables: solution design, ADR, макеты, API contracts.
- Роль [architect agent](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles); **architecture gateway** человека перед массовым кодингом.

**When to use:** Новые подсистемы, cross-team контракты, performance-sensitive фичи.

**Pluses and minuses:**

| +                              | −                                               |
| ------------------------------ | ----------------------------------------------- |
| Меньше переделок в development | Big design upfront устаревает на быстрых рынках |

**Sources:** [Architecture patterns](./7_special_architecture.md)

</details>

## Development

<details>
<summary>Development</summary>

**Definition:** **Development (разработка)** — активное создание ПО по требованиям и design: модули, сервисы, UI, скрипты, конфигурация.

**How it works:**

- Разработчики или **developer agents**, [Git](./0_git_core.md), PR, code review.
- Миграции, feature flags, observability.
- ИИ-ассистенты в основном ускоряют эту фазу; [lead time](./0_ai_agentic_sdlc.md#lead-time-vs-coding-speed) падает, если остальные фазы не тормозят.

**When to use:** После согласованного design (или параллельно в Agile-срезе); всегда с CI.

**Pluses and minuses:**

| +                                 | −                                         |
| --------------------------------- | ----------------------------------------- |
| Видимый прогресс (commits, demos) | Давление «кодить без design/requirements» |

**Sources:** [Coding agents on real projects](./0_ai_engineering.md#coding-agents-on-real-projects)

</details>

## Testing and deployment

<details>
<summary>Testing and deployment</summary>

**Definition:** **Testing and deployment (тестирование и интеграция / развёртывание)** — проверка качества (unit, integration, functional, NFR) и **выпуск** на staging/prod или дистрибуция пользователям.

**How it works:**

- План тестов от requirements, автоматизация ([Testing fundamentals](./7_special_testing_fundamentals.md)), регресс перед релизом.
- [CI/CD](./10_build_ci.md), миграции, smoke tests, мониторинг ([DevOps](./15_devops.md)).
- [V-Model](./7_special_sdlc.md#v-model) парует фазы dev и test; Agile гоняет тесты **постоянно**.
- Роли tester + deploy в [multi-agent SDLC](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles).

**When to use:** Каждый релиз; staged rollout для рискованных изменений.

**Pluses and minuses:**

| +                        | −                                         |
| ------------------------ | ----------------------------------------- |
| Дефекты до пользователей | Ручной деплой — bottleneck без automation |

**Thoughts:** На собесе можно объединять testing и deployment; в runbook разделяйте **quality sign-off** и **production push**.

**Sources:** [CI/CD](./10_build_ci.md) · [DevOps](./15_devops.md)

</details>

## Support

<details>
<summary>Support</summary>

**Definition:** **Support (поддержка / сопровождение)** — эксплуатация после релиза: исправления, обновления, tuning, поддержка пользователей, change requests.

**How it works:**

- Инциденты, патчи, зависимости, observability ([DevOps](./15_devops.md)).
- Обратная связь → следующие [planning](#planning) и [requirements](#requirements-analysis) ([cyclicity](./7_special_sdlc_principles.md#cyclicity)).
- Tech debt и [refactoring](./7_special_sdlc.md#code-refactoring).

**When to use:** Любой prod; SLAs и ownership до launch.

**Pluses and minuses:**

| +                                | −                                  |
| -------------------------------- | ---------------------------------- |
| Замыкание цикла с пользователями | Игнор support голодит будущие фичи |

**Sources:** [Code refactoring](./7_special_sdlc.md#code-refactoring)

</details>

## Interview Practice

### Questions & Answers

**Q: Перечислите шесть этапов SDLC по порядку.**  
A: Planning → requirements → design → development → testing & deployment → support (цикл повторяется). См. [overview](#overview).

**Q: Как Agile относится к этим этапам?**  
A: Те же этапы, но **перекрываются** внутри спринта/итерации.

**Q: На каком этапе появляются acceptance criteria?**  
A: В основном **requirements analysis**; тесты трассируются к ним.

**Q: Почему support — этап SDLC, а не «после SDLC»?**  
A: Поддержка **перезапускает** цикл — [cyclicity](./7_special_sdlc_principles.md#cyclicity).

**Q: Где ИИ меняет SDLC сильнее всего?**  
A: Производство артефактов на каждой фазе при контексте и воротах — [Agentic SDLC](./0_ai_agentic_sdlc.md), не только development.
