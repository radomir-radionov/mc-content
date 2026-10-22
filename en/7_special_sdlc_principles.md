# SDLC principles

Foundational principles of the software development life cycle — how teams keep delivery structured, quality-focused, and adaptable across Waterfall, iterative, and Agile models.

### Related docs

- [SDLC & Agile](./7_special_sdlc.md) · [SDLC phases](./7_special_sdlc_phases.md) · [Agentic SDLC (AI)](./0_ai_agentic_sdlc.md) · [Testing fundamentals](./7_special_testing_fundamentals.md) · [MVP & judgment](./0_mvp.md)

### Main concepts

| Area           | Topics                                                                                                                                                                                                                                                                                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Principles** | [Overview](#overview)                                                                                                                                                                                                                                                                                                                                        |
| **By theme**   | [Cyclicity](#cyclicity) · [Systematic approach](#systematic-approach) · [Quality orientation](#quality-orientation) · [Flexibility](#flexibility) · [Transparency](#transparency) · [Risk management](#risk-management) · [User focus](#user-focus) |

### Sections

- [Overview](#overview)
- [Cyclicity](#cyclicity)
- [Systematic approach](#systematic-approach)
- [Quality orientation](#quality-orientation)
- [Flexibility](#flexibility)
- [Transparency](#transparency)
- [Risk management](#risk-management)
- [User focus](#user-focus)
- [Interview Practice](#interview-practice)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **SDLC principles** are the cross-cutting rules that make a life-cycle model work — whether the team runs [Waterfall](./7_special_sdlc.md#waterfall), iterative cycles, or [Agile](./7_special_sdlc.md#agile). They apply to every [SDLC phase](./7_special_sdlc_phases.md).

**How it works:**

| Principle                                                  | One-line meaning                                           |
| ---------------------------------------------------------- | ---------------------------------------------------------- |
| [Cyclicity](#cyclicity)                     | Delivery repeats; products evolve through many passes      |
| [Systematic approach](#systematic-approach) | Each phase has goals, tasks, and outputs                   |
| [Quality orientation](#quality-orientation) | Test and verify against requirements continuously          |
| [Flexibility](#flexibility)                 | Same phases, different order/overlap (Agile vs sequential) |
| [Transparency](#transparency)               | Stakeholders see status and phase results                  |
| [Risk management](#risk-management)         | Find and reduce risks early each phase                     |
| [User focus](#user-focus)                   | Needs of end users drive decisions                         |

**When to use:** Onboarding, process design, interviews (“what makes SDLC more than a checklist?”), before scaling [agentic SDLC](./0_ai_agentic_sdlc.md) (agents need explicit standards).

**Pluses and minuses:**

| +                                     | −                                                             |
| ------------------------------------- | ------------------------------------------------------------- |
| Shared vocabulary across roles        | Principles without tooling become slideware                   |
| Bridges classic SDLC and modern Agile | Easy to cite principles but skip them under deadline pressure |

**Thoughts:** Principles describe **why** the [six phases](./7_special_sdlc_phases.md) exist; methodology notes describe **how** teams schedule them.

**Sources:** [Doka — Agile](https://doka.guide/tools/agile/) · [SDLC phases](./7_special_sdlc_phases.md)

</details>

## Cyclicity

<details>
<summary>Cyclicity</summary>

**Definition:** **Cyclicity** means software development is not a one-way line — the product passes through [planning → support](./7_special_sdlc_phases.md) many times as requirements, technology, and users change.

**How it works:**

- A “finished” release starts the next cycle: feedback, incidents, and new ideas re-enter [requirements](./7_special_sdlc_phases.md#requirements-analysis) and [design](./7_special_sdlc_phases.md#design).
- [Agile sprints](./7_special_sdlc.md#agile) are short cycles that repeat the whole slice (plan → build → test → review).
- Maintenance is not an afterthought — it feeds the next planning phase.

**When to use:** Explaining why support and change requests belong inside SDLC, not “after the project ended.”

**Pluses and minuses:**

| +                              | −                                                  |
| ------------------------------ | -------------------------------------------------- |
| Matches real product evolution | Without versioning, cycles blur into endless churn |

**Thoughts:** Cyclicity is why [lead time](./0_ai_agentic_sdlc.md#lead-time-vs-coding-speed) is measured over many releases, not one big bang.

**Sources:** [SDLC phases — Support](./7_special_sdlc_phases.md#support)

</details>

## Systematic approach

<details>
<summary>Systematic approach</summary>

**Definition:** A **systematic approach** structures work into phases with **defined objectives, activities, and deliverables** so the team progresses in a predictable order instead of ad hoc hacking.

**How it works:**

- Each [phase](./7_special_sdlc_phases.md) produces artifacts (spec, design doc, code, test report, release notes).
- Handoffs use clear “phase done when” criteria — related to [spec-driven development](./0_ai_engineering.md#spec-driven-development-sdd).
- [Waterfall](./7_special_sdlc.md#waterfall) maximizes sequential discipline; Agile keeps the system but overlaps phases inside iterations.

**When to use:** Fixing chaotic teams; defining gates before AI agents automate handoffs.

**Pluses and minuses:**

| +                                | −                                  |
| -------------------------------- | ---------------------------------- |
| Reduces chaos and duplicate work | Over-bureaucracy slows small fixes |

**Sources:** [Software design methodology](./7_special_sdlc.md#software-design-methodology)

</details>

## Quality orientation

<details>
<summary>Quality orientation</summary>

**Definition:** **Quality orientation** embeds verification **throughout** SDLC — not only in a final test phase — via reviews, automated tests, and checks against requirements.

**How it works:**

- [Testing & deployment phase](./7_special_sdlc_phases.md#testing-and-deployment) is explicit, but quality activities also happen during design reviews, code review, and CI on every commit.
- Early defect detection lowers cost versus fixing in production ([Testing fundamentals](./7_special_testing_fundamentals.md)).
- [Agentic SDLC](./0_ai_agentic_sdlc.md#skeptic-agent-critic-loop) adds automated critic loops before human release gates.

**When to use:** Building Definition of Done, test strategy, and interview answers on “when do you test?”

**Pluses and minuses:**

| +                                  | −                                                 |
| ---------------------------------- | ------------------------------------------------- |
| Fewer expensive production defects | Quality work needs time budget in every iteration |

**Sources:** [Testing fundamentals](./7_special_testing_fundamentals.md)

</details>

## Flexibility

<details>
<summary>Flexibility</summary>

**Definition:** **Flexibility** means SDLC adapts to **project type and methodology** — strict sequencing for fixed contracts, overlapping phases for Agile products.

**How it works:**

- Same six [phases](./7_special_sdlc_phases.md); different **scheduling**: sequential (Waterfall), repeated mini-cycles (Scrum), flow with WIP limits (Kanban).
- Teams may emphasize different phases (heavy design in enterprise, heavy iteration in startups).
- Modern practice often **hybridizes** (e.g. fixed-price contract with Agile delivery internally).

**When to use:** Choosing or defending a process model in interviews and proposals.

**Pluses and minuses:**

| +                                        | −                                                   |
| ---------------------------------------- | --------------------------------------------------- |
| One lifecycle vocabulary for many models | “Flexible” process without rules becomes no process |

**Sources:** [Agile](./7_special_sdlc.md#agile) · [Kanban](./7_special_sdlc.md#kanban)

</details>

## Transparency

<details>
<summary>Transparency</summary>

**Definition:** **Transparency** gives stakeholders a **clear view of progress**, current phase, risks, and phase outputs — so decisions are based on status, not guesswork.

**How it works:**

- Visible backlogs, roadmaps, burndowns, and release notes map work to [SDLC phases](./7_special_sdlc_phases.md).
- Phase boundaries make it obvious what is missing before deploy (e.g. no tests signed off).
- Supports [human gateways](./0_ai_agentic_sdlc.md#human-gateways-in-agentic-sdlc) — approvers know what they are signing.

**When to use:** PM/lead communication; audit and compliance contexts.

**Pluses and minuses:**

| +                           | −                                    |
| --------------------------- | ------------------------------------ |
| Trust and faster escalation | Metrics theater if status fields lie |

**Sources:** [Doka — Agile](https://doka.guide/tools/agile/)

</details>

## Risk management

<details>
<summary>Risk management</summary>

**Definition:** **Risk management** in SDLC means **identifying, assessing, and mitigating** technical, schedule, and requirement risks **at each phase**, not only at project kickoff.

**How it works:**

- [Planning](./7_special_sdlc_phases.md#planning) and [requirements](./7_special_sdlc_phases.md#requirements-analysis) expose scope and feasibility risks early.
- [Design](./7_special_sdlc_phases.md#design) addresses architectural and security risks.
- Testing and staged deployment reduce release risk; [Support](./7_special_sdlc_phases.md#support) handles operational risk after launch.

**When to use:** Enterprise proposals, regulated domains, and “what could go wrong?” design reviews.

**Pluses and minuses:**

| +                                         | −                                                 |
| ----------------------------------------- | ------------------------------------------------- |
| Fewer surprise failures late in the cycle | Risk work can feel slow until a crisis is avoided |

**Sources:** [Spiral model](./7_special_sdlc.md#spiral-model) (risk-driven cycles)

</details>

## User focus

<details>
<summary>User focus</summary>

**Definition:** **User focus** keeps **end-user needs and expectations** central from [requirements](./7_special_sdlc_phases.md#requirements-analysis) through [support](./7_special_sdlc_phases.md#support) — so the shipped product solves real problems.

**How it works:**

- Requirements phase captures personas, jobs-to-be-done, and acceptance criteria — not only internal wish lists.
- Design validates usability and accessibility ([a11y](./1_html_a11y.md)); development implements behavior users can complete.
- Support closes the loop with feedback into the next cycle ([MVP & judgment](./0_mvp.md)).

**When to use:** Product-led teams; pushing back on “build the spec” without user evidence.

**Pluses and minuses:**

| +                                | −                                                     |
| -------------------------------- | ----------------------------------------------------- |
| Higher adoption and satisfaction | Needs ongoing user access (research, beta, analytics) |

**Thoughts:** User focus is the bridge between SDLC and product discovery — without it, [Agile](./7_special_sdlc.md#agile) cadence still ships the wrong thing fast.

**Sources:** [MVP & judgment](./0_mvp.md)

</details>

## Interview Practice

### Questions & Answers

**Q: Name three SDLC principles and why they matter.**  
A: Examples: quality orientation (catch defects early), cyclicity (software keeps evolving), user focus (build the right thing). See [overview](#overview).

**Q: How does flexibility differ from “no process”?**  
A: Flexibility adapts **scheduling** of the same phases to Waterfall vs Agile — it still requires deliverables and gates.

**Q: Where does risk management happen besides planning?**  
A: Every phase — architecture risk in design, regression risk in testing, operational risk in support.

**Q: How do principles relate to SDLC phases?**  
A: Phases are **what** you do in order; principles are **how** you should run any phase well. See [SDLC phases](./7_special_sdlc_phases.md).
