# SDLC phases

The six classic phases of the software development life cycle — from planning through support — and how they map to Agile iterations and AI-augmented delivery.

### Related docs

- [SDLC principles](./7_special_sdlc_principles.md) · [SDLC & Agile](./7_special_sdlc.md) · [Agentic SDLC (AI)](./0_ai_agentic_sdlc.md) · [Testing fundamentals](./7_special_testing_fundamentals.md) · [CI/CD](./10_build_ci.md) · [DevOps](./15_devops.md)

### Main concepts

| Area       | Topics                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Phases** | [Overview](#overview)                                                                                                                                                                               |
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

**Definition:** **SDLC phases** are the major stages every software product passes through: **planning, requirements analysis, design, development, testing & deployment, and support**. In [Waterfall](./7_special_sdlc.md#waterfall) they run largely sequential; in [Agile](./7_special_sdlc.md#agile) they **overlap and repeat** inside short iterations.

**How it works:**

| Step                | What you're doing                                                 |
| ------------------- | ----------------------------------------------------------------- |
| **Planning**        | Goals, scope, timeline, team, budget — _what initiative is this?_ |
| **Requirements**    | What the product must do — user needs, acceptance criteria        |
| **Design**          | How it will work — architecture, APIs, UI, data model             |
| **Development**     | Build it — code, config, integration                              |
| **Test and deploy** | Verify quality, release to users                                  |
| **Support**         | Run it in production — bugs, incidents, small fixes, monitoring   |

Read top to bottom as the classic chain: **plan → specify → design → build → ship → operate**. After **Support**, [**cyclicity**](./7_special_sdlc_principles.md#cyclicity) loops back to **Planning** — production feedback starts the next cycle of work.

Each phase has typical **management focus**: scope/time/cost (planning), traceability (requirements), architecture & UX (design), implementation (development), quality & release (testing/deployment), operations & change (support).

Guiding ideas: [SDLC principles](./7_special_sdlc_principles.md). AI mapping: [AI augmentation of SDLC phases](./7_special_sdlc.md#ai-augmentation-of-sdlc-phases).

**When to use:** Interviews, documentation templates, and aligning team vocabulary with [methodology choices](./7_special_sdlc.md#software-design-methodology).

**Pluses and minuses:**

| +                                     | −                                            |
| ------------------------------------- | -------------------------------------------- |
| Universal map for any methodology     | Names and boundaries differ by company       |
| Easy to attach deliverables per phase | Agile teams may resist “phase gate” language |

**Thoughts:** “Testing & deployment” is one phase in many textbooks; some split **testing** and **release** — keep your team’s Definition of Done explicit.

**Sources:** [Doka — Agile](https://doka.guide/tools/agile/) · [Cyclicity](./7_special_sdlc_principles.md#cyclicity)

</details>

## Planning

<details>
<summary>Planning</summary>

**Definition:** **Planning** is the **project-management** phase: define goals, scope, schedule, budget, resources, team roles, and a high-level roadmap before deep requirements work.

**How it works:**

- Outputs: project plan, milestone schedule, resource allocation, communication plan, initial risk register ([risk management principle](./7_special_sdlc_principles.md#risk-management)).
- Feeds [requirements analysis](#requirements-analysis) with priorities and constraints (deadlines, compliance, platforms).
- In Agile, planning is **continuous** (release planning, sprint planning) — not only day one.

**When to use:** New products, major initiatives, fixed-price contracts; lighter planning for small iterative fixes.

**Pluses and minuses:**

| +                                  | −                                                   |
| ---------------------------------- | --------------------------------------------------- |
| Aligns stakeholders on feasibility | Over-planning before learning (Lean/Agile pushback) |

**Examples:** Choose Scrum vs Kanban; pick MVP scope for first release ([MVP](./0_mvp.md)).

**Sources:** [Software design methodology](./7_special_sdlc.md#software-design-methodology)

</details>

## Requirements analysis

<details>
<summary>Requirements analysis</summary>

**Definition:** **Requirements analysis** collects, clarifies, and documents **what** the system must do for users and the business — the basis for design, tests, and acceptance.

**How it works:**

- Activities: interviews, workshops, analysis of existing systems, user stories, use cases, non-functional requirements (performance, security).
- Deliverables: requirements spec, acceptance criteria, trace matrix to tests later.
- Ties to [user focus](./7_special_sdlc_principles.md#user-focus) and [SDD](./0_ai_engineering.md#spec-driven-development-sdd).
- In [agentic SDLC](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles), an **analyst agent** drafts this artifact from a ticket stub; a human approves at a [gateway](./0_ai_agentic_sdlc.md#human-gateways-in-agentic-sdlc).

**When to use:** Every non-trivial feature; regulated domains need strong traceability.

**Pluses and minuses:**

| +                                   | −                                       |
| ----------------------------------- | --------------------------------------- |
| Prevents building the wrong feature | Analysis paralysis if scope is unstable |

**Thoughts:** Bad requirements poison every later phase — cheapest place to fix mistakes.

**Sources:** [Spec-driven development (SDD)](./0_ai_engineering.md#spec-driven-development-sdd)

</details>

## Design

<details>
<summary>Design</summary>

**Definition:** **Design** covers **technical architecture** and **user-facing design**: data structures, interfaces, components, algorithms, UI/UX, and specifications developers implement against.

**How it works:**

- Technical: APIs, database schema, integration points, security model — see [Architecture patterns](./7_special_architecture.md).
- UX/UI: wireframes, prototypes, design system tokens, accessibility targets ([HTML a11y](./1_html_a11y.md)).
- Deliverables: solution design, ADRs, UI mocks, API contracts.
- [Architect agent](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles) role maps here; human **architecture gateway** before coding at scale.

**When to use:** New subsystems, cross-team contracts, performance-sensitive features.

**Pluses and minuses:**

| +                             | −                                              |
| ----------------------------- | ---------------------------------------------- |
| Reduces rework in development | Big upfront design ages poorly in fast markets |

**Sources:** [Architecture patterns](./7_special_architecture.md)

</details>

## Development

<details>
<summary>Development</summary>

**Definition:** **Development** (implementation / coding) is active construction of software according to requirements and design — modules, services, UI, scripts, and configuration.

**How it works:**

- Developers (or **developer agents**) implement features, follow coding standards, use [version control](./0_git_core.md), and open PRs for review.
- Includes low-level tasks: migrations, feature flags, instrumentation.
- AI **assistants** mostly accelerate this phase; [lead time](./0_ai_agentic_sdlc.md#lead-time-vs-coding-speed) only drops if other phases keep pace.

**When to use:** After approved design (or alongside it in Agile slices); always under CI checks.

**Pluses and minuses:**

| +                                 | −                                                     |
| --------------------------------- | ----------------------------------------------------- |
| Visible progress (commits, demos) | Temptation to skip design/requirements under pressure |

**Sources:** [Coding agents on real projects](./0_ai_engineering.md#coding-agents-on-real-projects)

</details>

## Testing and deployment

<details>
<summary>Testing and deployment</summary>

**Definition:** **Testing and deployment** verifies quality (unit, integration, functional, non-functional), then **releases** the product to target environments — staging, production, or distribution to users.

**How it works:**

- Testing: plan from requirements, automate where possible ([Testing fundamentals](./7_special_testing_fundamentals.md)), regression before release.
- Deployment: build artifacts, [CI/CD pipelines](./10_build_ci.md), migrations, smoke tests, monitoring hooks ([DevOps](./15_devops.md)).
- [V-Model](./7_special_sdlc.md#v-model) pairs each dev phase with a test level; Agile runs tests **continuously** inside sprints.
- Agent roles: **tester** + **deploy** in [multi-agent SDLC](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles).

**When to use:** Every release; staged rollouts for high-risk changes.

**Pluses and minuses:**

| +                               | −                                                          |
| ------------------------------- | ---------------------------------------------------------- |
| Catches defects before users do | Manual release steps become bottlenecks without automation |

**Thoughts:** Merge “testing” and “deployment” in interviews but separate **quality sign-off** from **production push** in runbooks.

**Sources:** [CI/CD](./10_build_ci.md) · [DevOps](./15_devops.md)

</details>

## Support

<details>
<summary>Support</summary>

**Definition:** **Support** (maintenance / operations) keeps software **running and evolving** after release: bug fixes, updates, performance tuning, user support, and handling change requests.

**How it works:**

- Incident response, patches, dependency updates, observability ([DevOps](./15_devops.md)).
- User feedback and analytics feed the next [planning](#planning) and [requirements](#requirements-analysis) cycle — [cyclicity](./7_special_sdlc_principles.md#cyclicity).
- Technical debt paydown and [refactoring](./7_special_sdlc.md#code-refactoring) often live here.

**When to use:** Always for production systems; define SLAs and ownership before launch.

**Pluses and minuses:**

| +                                   | −                                         |
| ----------------------------------- | ----------------------------------------- |
| Closes the feedback loop with users | Neglected support starves future features |

**Sources:** [Code refactoring](./7_special_sdlc.md#code-refactoring)

</details>

## Interview Practice

### Questions & Answers

**Q: List the six SDLC phases in order.**  
A: Planning → requirements analysis → design → development → testing & deployment → support (then cycle repeats). See [overview](#overview).

**Q: How do Agile teams treat these phases?**  
A: Same phases, but **overlap** within each sprint/iteration instead of one long sequential pass.

**Q: Which phase produces acceptance criteria?**  
A: Primarily **requirements analysis**; tests trace back to them in testing/deployment.

**Q: Why is support a phase, not “after SDLC”?**  
A: Maintenance and feedback **restart** the lifecycle — [cyclicity](./7_special_sdlc_principles.md#cyclicity).

**Q: Where does AI change SDLC most?**  
A: Artifact production in every phase if context and gates exist — see [Agentic SDLC](./0_ai_agentic_sdlc.md), not only development.
