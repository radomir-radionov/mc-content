# SDLC

Waterfall, Agile, Scrum, and Kanban. Canonical **principles** and **six phases** live in sibling topics — this file focuses on **methodology models**.

### Related docs

- [SDLC principles](./7_special_sdlc_principles.md) · [SDLC phases](./7_special_sdlc_phases.md) · [Computer science](./7_special_cs.md) · [Architecture patterns](./7_special_architecture.md) · [SOLID](./7_special_engineering.md) · [Design patterns](./7_special_patterns.md) · [Agentic SDLC (AI)](./0_ai_agentic_sdlc.md) · [Testing](./7_special_testing.md) · [Interview leftovers](./7_special_interview.md) · [CI/CD](./10_build_interview.md#what-is-cicd) · [DevOps](./15_devops.md)

### Main concepts

| Area            | Topics                                                                                                |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| **Foundations** | [SDLC principles](./7_special_sdlc_principles.md) · [SDLC phases](./7_special_sdlc_phases.md)         |
| **Process**     | [Software design methodology](#software-design-methodology), [Waterfall](#waterfall), [Agile](#agile) |
| **AI**          | [AI augmentation of SDLC phases](#ai-augmentation-of-sdlc-phases)                                     |

### Sections

- [11. SDLC & Agile](#11-sdlc-agile)
- [AI augmentation of SDLC phases](#ai-augmentation-of-sdlc-phases)
- [Frontend team in Agile](#frontend-team-in-agile)
- [Definition of Ready (DoR) and Definition of Done (DoD)](#definition-of-ready-dor-and-definition-of-done-dod)
- [GitFlow branching model](#gitflow-branching-model)
- [RACI and stakeholders](#raci-and-stakeholders)
- [Technical debt](#technical-debt)
- [Code refactoring](#code-refactoring)
- [SDLC — interview checklist](#sdlc--interview-checklist)

---

<a name="11-sdlc--agile"></a>

## 11. SDLC & Agile

<details>
  <summary>Software design methodology</summary>

### Software design methodology

**Definition:** A shared way to plan, build, ship, and improve software so the team can coordinate work, manage risk, and match how the business actually changes.

**How it works:**

Companies need to **test hypotheses fast** — drop the wrong ones and grow the ones that work. A methodology is the operating system for that: who decides priority, how work flows, when users see value, and how the team reacts when requirements move.

Requirements → Design → Implementation → Testing → Deployment → Maintenance — see [Phases overview](./7_special_sdlc_phases.md#overview) for definitions and deliverables per stage. Guiding rules: [SDLC principles](./7_special_sdlc_principles.md).

Same phases, different **order and feedback**. Waterfall runs them once, end-to-end. Agile repeats a short slice of the whole cycle so the product can change before years of work are wasted.

**When to use:** Any team larger than one person; interviews (“how does your team ship?”); choosing Waterfall vs Scrum vs Kanban from **business constraints**, not fashion.

**Pluses and minuses:**

| + (formal methodology)                      | −                                                      |
| ------------------------------------------- | ------------------------------------------------------ |
| Shared language for status, roles, and risk | Cargo-cult process (“we do standups”) without shipping |
| Checkpoints before irreversible spend       | Overhead if the process is heavier than the product    |

**Thoughts:** _Methodologies exist so delivery matches uncertainty — software changes cheaper than bridges, so process should allow change._

**Sources:**

[Doka — Agile](https://doka.guide/tools/agile/)

---

</details>

<details>
  <summary>Development methodologies</summary>

### Development methodologies

**Definition:** Methodologies are structured practices for organizing code and teams — distinct from language paradigms (how you express logic).

| Category         | Examples                                                | Notes                                                                                                     |
| ---------------- | ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| CSS architecture | BEM, SMACSS, OOCSS, Atomic Design                       | [CSS Architecture](./2_css_architecture.md)                                                               |
| App structure    | MVC, MVVM, Flux, Redux                                  | [Architecture patterns](./7_special_architecture.md)                                                      |
| Process          | TDD, BDD, DDD                                           | [Testing](./7_special_testing_fundamentals.md#12-testing)                                                 |
| Principles       | DRY, KISS, SOLID, YAGNI                                 | [SOLID](./7_special_engineering.md#9-oop-and-solid) · [DRY, KISS, YAGNI](./0_system_design_principles.md) |
| Frontend         | Component-driven, mobile-first, progressive enhancement | Product/UX conventions                                                                                    |

**How it works:** Paradigms answer _how to express logic_ (FP vs OOP). Methodologies answer _how to structure a codebase and collaborate_ (MVC, TDD, BEM). Use both: paradigm for the problem, methodology for maintainability at scale.

**Thoughts:** Do not file this under JavaScript language notes — JS is multi-paradigm; these practices apply across stacks. Paradigms map: [Programming paradigms](./7_special_cs.md#programming-paradigm).

**Sources:** [Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/) · [Doka — Agile](https://doka.guide/tools/agile/) · [getbem.com](http://getbem.com/)

</details>

<details>
  <summary>Waterfall</summary>

### Waterfall

**Definition:** Linear **sequential** SDLC model — each phase (requirements → design → implementation → testing → deployment → maintenance) completes before the next begins.

**How it works:**

1. **Requirements** — full spec documented upfront (often a huge TZ / contract).
2. **Design** — architecture and UI frozen.
3. **Implementation** — build to spec.
4. **Verification** — test against the requirements doc.
5. **Deployment** — release once (or rarely).
6. **Maintenance** — fixes only; new work restarts change-control.

Copied from construction / electronics / manufacturing. Software is more dynamic: changing an export format is not like changing the number of piers on a railway bridge — so a frozen multi-year spec ages badly.

**Classic failure modes** (why Agile spread in the 1990s–2000s):

- **Missed dates** — the longer the horizon, the harder to hit; slipped years become business conflict.
- **Obsolete product** — market and users move while the team is still in “implementation.”
- **Painful change** — even a small addition reopens the whole approval chain, often slower than coding the change.

**When to use:** Fixed requirements, compliance-heavy domains (medical devices, aerospace, government contracts), hardware-coupled software, **agency / fixed-price** work where the client pays for a specified result.

**Pluses and minuses:**

| +                                     | −                                                |
| ------------------------------------- | ------------------------------------------------ |
| Clear milestones and documentation    | Late feedback — users see the product at the end |
| Works for fixed scope and budgets     | Expensive to change after design                 |
| Traceability for regulated industries | Assumes requirements are knowable upfront        |
| Predictable for contract bidding      | Long time without shippable value                |

**Examples:**

| Step               | What happens                                    |
| ------------------ | ----------------------------------------------- |
| **Requirements**   | Capture and sign off the full spec before build |
| **Design**         | Freeze architecture and UI against that spec    |
| **Implementation** | Build to the document                           |
| **Testing**        | Verify against the requirements doc             |
| **Deployment**     | Release (often once per major version)          |
| **Maintenance**    | Fixes and controlled change requests            |

One pass, left to right — the next big change usually restarts requirements, not a tight weekly loop.

**Thoughts:** Rarely pure waterfall in modern **product** teams — often hybrid (iterative chunks inside phases). Still common in **outsourcing**: client pays for a result; many studios still use Agile **internally** (range estimate, bill actual time). Contrast with [Agile](#agile).

**Sources:**

[Doka — Agile](https://doka.guide/tools/agile/) · [SDLC methodologies](https://habr.com/ru/companies/edison/articles/269789/)

---

</details>

<details>
  <summary>Iterative / Incremental</summary>

### Iterative / Incremental

**Definition:** Repeated cycles of build → feedback → refine. Incremental delivers usable slices each cycle.

**When to use:**

Most modern products; foundation of Agile.

---

**Pluses and minuses:**

| +                     | −                              |
| --------------------- | ------------------------------ |
| Early feedback        | Scope creep without discipline |
| Reduced big-bang risk |                                |

</details>

<details>
  <summary>Spiral model</summary>

### Spiral model

**Definition:** Cycles of plan → risk analysis → build → evaluate; risk-driven.

**When to use:**

Large, high-risk systems (aerospace, enterprise).

---

**Pluses and minuses:**

| +                        | −                        |
| ------------------------ | ------------------------ |
| Explicit risk management | Heavy for small projects |
|                          |                          |

</details>

<details>
  <summary>V-Model</summary>

### V-Model

**Definition:** Each development phase paired with a testing phase (requirements ↔ acceptance tests).

**When to use:**

Safety-critical, medical, embedded with strict validation.

---

**Pluses and minuses:**

| +                   | −     |
| ------------------- | ----- |
| Test planning early | Rigid |
| Traceability        |       |

</details>

<details>
  <summary>RAD (Rapid Application Development)</summary>

### RAD (Rapid Application Development)

**Definition:** Prototype fast, iterate on user feedback, minimize upfront design.

**When to use:**

UI-heavy apps, uncertain requirements, internal tools.

---

**Pluses and minuses:**

| +                 | −                          |
| ----------------- | -------------------------- |
| Fast time to demo | Can sacrifice architecture |
| User-driven       |                            |

</details>

<details>
  <summary>Agile</summary>

### Agile

**Definition:**

A **mindset** (not a single process): values and principles from the 2001 Agile Manifesto. Frameworks like Scrum and Kanban implement them. Goal: people, working software, and **fast delivery of features** so hypotheses can be validated.

**Four values** (left side valued _more_, not “instead of” the right):

| We value more                | Than                        |
| ---------------------------- | --------------------------- |
| Individuals and interactions | Processes and tools         |
| Working software             | Comprehensive documentation |
| Customer collaboration       | Contract negotiation        |
| Responding to change         | Following a plan            |

**How it works:**

- Short cycles that include analysis, build, test, and a **usable** slice — not years of spec-then-code.
- **Backlog** ordered by value (and tech debt); the team pulls the top items.
- Stakeholders and developers work together continuously; change is expected, even late.
- **Working software** is the primary measure of progress; face-to-face (or equivalent) communication is preferred.
- Teams are trusted, self-organizing; they inspect how they work and adjust (retros).

**Twelve principles** (compressed from [agilemanifesto.org/principles](https://agilemanifesto.org/principles.html)):

1. Highest priority: satisfy the customer via **early and continuous** delivery of valuable software.
2. Welcome changing requirements, even late.
3. Deliver working software frequently (weeks to months; prefer shorter).
4. Business people and developers work together **daily**.
5. Build around motivated people; give support and trust.
6. Most efficient communication: conversation (face-to-face as the default).
7. Working software = primary progress metric.
8. Sustainable pace indefinitely (sponsors, developers, users).
9. Continuous attention to technical excellence and good design.
10. Simplicity — maximize the work **not** done.
11. Best designs emerge from self-organizing teams.
12. Regular reflection; tune behavior.

**When to use:** Uncertain requirements, product/SaaS, startups. Implementations: [Scrum](#scrum) (sprints), [Kanban](#kanban) (flow), XP (engineering practices). CI/CD shortens the same loop — [10_build.md — CI/CD](./10_build_interview.md#what-is-cicd).

**Pluses and minuses:**

| +                                  | −                                                   |
| ---------------------------------- | --------------------------------------------------- |
| Frequent value and learning        | Needs an engaged product owner / customer           |
| Adapts to market and user feedback | “Agile theater” = ceremonies without shipping       |
| Reduces big-bang release risk      | Awkward for rigid fixed-price/fixed-scope contracts |
| Continuous improvement via retros  | Architecture can drift without discipline           |

**Examples:**

| Step              | What happens                                                          |
| ----------------- | --------------------------------------------------------------------- |
| **Plan**          | Pick a small slice of value for this iteration (backlog, sprint goal) |
| **Build**         | Ship **working** software toward that slice                           |
| **Review**        | Demo to stakeholders; learn what to do next                           |
| **Retrospective** | Improve how the team works (process, quality, collaboration)          |

After **Review** and **Retrospective**, the loop starts again at **Plan** — same idea as [cyclicity](./7_special_sdlc_principles.md#cyclicity), but on a **short** cadence (weeks, not years).

**Thoughts:** Agile ≠ no docs — **just enough**, living docs. The word is often **marketing** on job posts; ask how the team actually ships. Not a silver bullet — see [Not a silver bullet](#not-a-silver-bullet).

**Sources:**

[Agile Manifesto](https://agilemanifesto.org/) · [Principles](https://agilemanifesto.org/principles.html) · [Doka — Agile](https://doka.guide/tools/agile/)

---

</details>

<details>
  <summary>Scrum</summary>

### Scrum

**Definition:** Lightweight Agile **framework** (Scrum Guide 2020) for complex work: a small Scrum Team delivers a usable **Increment** every **Sprint**. Empiricism + lean thinking; pillars **transparency, inspection, adaptation**. Values: **commitment, focus, openness, respect, courage**.

**How it works:**

The **Sprint** is the container for all other events — a fixed-length iteration (**one month or less**, commonly 1–4 weeks). A new Sprint starts immediately after the previous one. The Increment should be **usable** (Definition of Done); it _may_ be released before the Sprint Review (the Review is not a release gate).

| Accountability    | Role                                                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Product Owner** | One person (not a committee). Maximizes product value; orders the Product Backlog; Product Goal.                          |
| **Scrum Master**  | Accountable for Scrum as defined and for team effectiveness: coach, remove impediments, keep events useful and timeboxed. |
| **Developers**    | Cross-functional people who create the Increment; own Sprint Backlog and Definition of Done.                              |

Typical team size: **10 or fewer**, self-managing, no sub-teams.

| Artifact            | Commitment                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Product Backlog** | Emergent ordered list of everything that might improve the product (features **and** tech debt). **Product Goal**. Refined ongoing (break down, add detail, size). |
| **Sprint Backlog**  | Sprint Goal (why) + selected items (what) + plan (how). Owned by Developers; updated during the Sprint.                                                            |
| **Increment**       | Additive, verified, usable. **Definition of Done** — if not Done, it is not an Increment (back to backlog).                                                        |

| Event                    | Purpose (Guide)                                                                                              |
| ------------------------ | ------------------------------------------------------------------------------------------------------------ |
| **Sprint Planning**      | Why valuable → Sprint Goal; what can be Done; how. Timebox ≤ 8h for a 1-month Sprint.                        |
| **Daily Scrum**          | 15 min, Developers: inspect progress toward Sprint Goal, adapt the plan for the next day.                    |
| **Sprint Review**        | Inspect Increment with stakeholders; adapt Product Backlog. Working session, not a slide deck. ≤ 4h / month. |
| **Sprint Retrospective** | Inspect people, process, tools, DoD; pick improvements. Ends the Sprint. ≤ 3h / month.                       |

During the Sprint: do not endanger the Sprint Goal; quality does not drop; backlog can be **refined**; scope can be renegotiated with the PO as learning happens. Only the PO may **cancel** a Sprint if the Goal is obsolete.

Rugby-style “scrum” popularized in a 1986 HBR paper (Takeuchi/Nonaka). Schwaber & Sutherland developed software Scrum in the early 1990s (OOPSLA 1995); first Scrum Guide 2010, current **November 2020**. Official: [scrumguides.org](https://scrumguides.org/scrum-guide.html) / [scrum.org](https://www.scrum.org/).

**When to use:** Product squads with changing priorities and a dedicated cross-functional team. Heavy if the work is a pure interrupt queue — consider [Kanban](#kanban).

**Pluses and minuses:**

| +                                                    | −                                                                          |
| ---------------------------------------------------- | -------------------------------------------------------------------------- |
| Cadence, transparency, regular inspect/adapt         | Ceremony cost; cargo-cult “Scrum” if events are skipped or empty           |
| Frequent stakeholder feedback on a working Increment | Needs a real PO; PO bottleneck if they are a committee in disguise         |
| Makes process problems visible                       | Guide: partial Scrum is **not** Scrum — many teams still tailor (Scrumban) |

**Examples:**

One **Sprint** (fixed length, often 1–4 weeks), in plain order:

1. **Product Backlog** — ordered work and Product Goal (ongoing refinement).
2. **Sprint Planning** — pick Sprint Goal + Sprint Backlog for this Sprint.
3. **Daily Scrum** — 15 min inspect/adapt toward the Sprint Goal.
4. **Increment** — Done, usable product slice (Definition of Done).
5. **Sprint Review** — inspect Increment with stakeholders; update backlog.
6. **Sprint Retrospective** — improve team and process; then the **next Sprint** starts.

Details and timeboxes: table above in this section.

**Thoughts:**

Scrum is first a pattern of **interaction** aimed at regular, quality delivery — not a meeting checklist. Many teams drop events; others hire a Scrum Master to run the framework well.

_Sprint = timeboxed inspect-and-adapt loop that produces a Done Increment toward a Product Goal._

**Sources:**

[Scrum Guide 2020](https://scrumguides.org/scrum-guide.html) · [Doka — Agile](https://doka.guide/tools/agile/)

---

</details>

<details>
  <summary>Kanban</summary>

### Kanban

**Definition:** Flow method: **visualize** the pipeline, **limit WIP**, **pull** work when a person (or column) has capacity. No required Sprint. Originated at **Toyota** (1960s, manufacturing); adapted to software in the 2000s.

**How it works:**

1. **Visualize** — columns match real handoffs (e.g. Ready for design → Ready for dev → Review → Done).
2. **Pull** — any designer takes from “ready for design”; any developer from “ready for development” — not a manager pushing a whole sprint of mixed work.
3. **Limit WIP** — cap items per person or per column so queues do not hide overload.
4. **Find bottlenecks** — if cards pile up in “Ready for development,” **dev capacity** is the constraint of the whole team; fix the bottleneck, do not start more design.
5. **Measure flow** — cycle time, throughput, cumulative flow (forecast without a Sprint Goal).

**When to use:** Work that must pass **several roles** in sequence (design → code → QA); support/on-call; DevOps/SRE; maintenance mixed with interrupts. Often combined with Scrum (**Scrumban**).

**Pluses and minuses:**

| +                                             | −                                                             |
| --------------------------------------------- | ------------------------------------------------------------- |
| Transparent progress; bottlenecks are visible | No built-in planning rhythm or Sprint Goal                    |
| Pull + WIP limits cut multitasking            | Weak for a greenfield “we need a shared cadence” product      |
| Fits interrupt-driven work                    | Dates are forecasts from flow metrics, not sprint commitments |
| Continuous delivery, no sprint boundary       | Infinite WIP if limits are ignored                            |

**Examples:**

| Ready for design | Ready for dev | In progress (WIP ≤ 3) | Review  | Done |
| ---------------- | ------------- | --------------------- | ------- | ---- |
| Landing v2       | Auth API      | Feature A             | PR #101 | v2.1 |
|                  | Bug #42       |                       |         |      |

**Thoughts:**

Kanban ≠ “no planning.” A Jira/GitHub board is only Kanban if **WIP limits and pull** are real.

_Make the queue visible; the column that fills up is the team’s bottleneck._

**Sources:**

[Doka — Agile](https://doka.guide/tools/agile/) · [Kanban Guide](https://kanbanguides.org/)

---

</details>

---

## AI augmentation of SDLC phases

<details>
<summary>AI augmentation of SDLC phases</summary>

**Definition:** **AI augmentation of SDLC phases** means the classic phase model (requirements → design → build → test → deploy → maintain) stays, but **agents** can produce and iterate on phase artifacts while humans approve at **gateways**.

**How it works:**

- Maps each classical phase to [multi-agent SDLC roles](./0_ai_agentic_sdlc.md#multi-agent-sdlc-roles): analyst (requirements), architect (design), developer (build), reviewer/tester (quality), deploy (release).
- Differs from “Copilot only in build”: [lead time](./0_ai_agentic_sdlc.md#lead-time-vs-coding-speed) improves only when upstream/downstream work is automated too.
- [Agile](#agile) and [Kanban](#kanban) still govern cadence; agentic SDLC governs **who executes** inside each slice.

**When to use:** Connecting process interviews (Waterfall/Scrum) with modern AI delivery talk; scoping a pilot on one phase vs full pipeline.

**Pluses and minuses:**

| +                                       | −                                                                                        |
| --------------------------------------- | ---------------------------------------------------------------------------------------- |
| Reuses familiar SDLC vocabulary         | Not a replacement for team agreements                                                    |
| Clear place for human architecture gate | Full pipeline needs [infrastructure](./0_ai_agentic_sdlc.md#agentic-sdlc-infrastructure) |

**Thoughts:** Canonical deep dive: [Agentic SDLC](./0_ai_agentic_sdlc.md). This note is the bridge from **process methodology** to **AI operations**.

**Sources:** [Agentic SDLC — AI in SDLC](./0_ai_agentic_sdlc.md#ai-in-sdlc)

</details>

---

## Frontend team in Agile

<details>
  <summary>Frontend team in Agile</summary>

### Frontend team in Agile

**Definition:** **Frontend work in Scrum/Kanban** means the UI team shares the same **cadence, WIP limits, and Definition of Done** as backend — with deliverables that are **shippable UI slices**, not “all screens done at the end.”

**How it works:**

| Scrum artifact         | Frontend angle                                                   |
| ---------------------- | ---------------------------------------------------------------- |
| **Product Backlog**    | Vertical slices (login flow, checkout), not “all CSS”            |
| **Sprint Goal**        | Demoable UI + API contract, not hidden integration week          |
| **Definition of Done** | Lint, types, unit/RTL tests, a11y spot-check, preview URL        |
| **Daily Scrum**        | Blockers: API not ready, design tokens, flaky E2E                |
| **Kanban columns**     | Design → Ready for dev → In progress (WIP limit) → Review → Done |

**When to use:** Job ads “Scrum/Kanban experience” — describe **your role** (estimates, demos, DoD, pairing with QA), not only ceremony names.

**Pluses and minuses:**

| +                               | −                                                   |
| ------------------------------- | --------------------------------------------------- |
| Early feedback on UI in reviews | Backend lag breaks vertical slices                  |
| Preview deploys per MR          | “Frontend-only sprint” without integrated increment |

**Thoughts:** Ceremonies in depth → [Scrum](#scrum) · [Kanban](#kanban). CI gates on MR → [GitLab CI](./10_build_ci.md#gitlab-ci-and-gitlab-runner) · [Testing pyramid — React](./4_react_testing.md#testing-pyramid-react).

**Sources:** [Scrum Guide](https://scrumguides.org/scrum-guide.html) · [Doka — Agile](https://doka.guide/tools/agile/)

</details>

---

<details>
  <summary>Definition of Ready (DoR) and Definition of Done (DoD)</summary>

### Definition of Ready (DoR) and Definition of Done (DoD)

**Definition:**

- **Definition of Ready (DoR)** — checklist before a backlog item enters a sprint: clear acceptance criteria, dependencies resolved, design/API agreed, estimable.
- **Definition of Done (DoD)** — checklist before work is **done**: code reviewed, tests pass, docs updated, deployed to agreed environment, no known sev-1 bugs.

**How it works:**

DoR prevents “half-specified” stories from blocking the team mid-sprint. DoD is shared by **all** disciplines (frontend includes a11y, perf budget, i18n if in scope). DoD ≠ “developer finished coding” — it is **shippable** per team contract.

**When to use:** Scrum/Kanban interviews; align DoD with [CI/CD](./10_build_ci.md#continuous-integration-ci) gates.

**Sources:** [Scrum Guide — Increment](https://scrumguides.org/scrum-guide.html)

---

</details>

<details>
  <summary>GitFlow branching model</summary>

### GitFlow branching model

**Definition:** **GitFlow** — long-lived **`main`** (production) and **`develop`**, plus short-lived **`feature/*`**, **`release/*`**, and **`hotfix/*`** branches.

**How it works:**

Features branch from `develop`, merge back; releases stabilize on `release/*` then merge to `main` and back to `develop`; hotfixes branch from `main`. Contrasts with **trunk-based** / **GitHub Flow** (short-lived branches off `main` + PR). Pick flow by release cadence and team size.

**See also:** [Pull requests / MRs](./0_git_team.md) · [Git interview](./0_git_interview.md)

**Sources:** [Atlassian — Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

---

</details>

<details>
  <summary>RACI and stakeholders</summary>

### RACI and stakeholders

**Definition:**

- **Stakeholder** — anyone affected by or influencing the product (users, business, legal, ops).
- **RACI** — responsibility matrix: **R**esponsible (does the work), **A**ccountable (one approver), **C**onsulted, **I**nformed.

**How it works:**

Use RACI on cross-team initiatives (launch, migration) to avoid “everyone thought someone else owned it.” In Agile, Product Owner is often **Accountable** for backlog; devs **Responsible** for increment; stakeholders **Consulted/Informed** in reviews.

**Sources:** [PMI — RACI overview](https://www.pmi.org/learning/library/raci-matrix-responsibility-assignment-6039)

---

</details>

<details>
  <summary>Technical debt</summary>

### Technical debt

**Definition:** **Technical debt** — implied future cost from shortcuts (missing tests, tangled modules, outdated deps) taken to ship faster now.

**How it works:**

Like financial debt: you can borrow for speed if you **pay interest** (slower features, bugs, onboarding pain). Manage with visible backlog items, [refactoring](#code-refactoring) after green tests, and [Quality Gates](./7_special_testing_fundamentals.md#tests-and-aaa) in CI — not endless “cleanup sprints” without product value.

**See also:** [Code review](./0_git_team.md) · [CI/CD interview](./10_build_interview.md#what-is-cicd)

---

</details>

<details>
  <summary>Not a silver bullet</summary>

### Not a silver bullet

**Definition:** No methodology (including Agile) fits every company or project. Pick the process that serves **business needs and the team** — not the job-ad keyword.

**How it works:**

- Waterfall / heavy spec still fits when scope is truly fixed or legally frozen.
- Many teams use **only some** Agile principles (short demos, backlog, WIP limits) without full Scrum — that can be healthy.
- **Product** companies almost always want frequent delivery and a changeable result.
- **Agency / outsourcing** often sells a result (classic contract) but may run Scrum/Kanban **inside** (estimate range, invoice time).
- “Agile” on a vacancy ≠ inspect-and-adapt in practice (**Agile theater**).

**When to use:** Interview question “Waterfall vs Agile?” — answer with **uncertainty, feedback cost, and contract shape**, then name Scrum vs Kanban.

**Pluses and minuses:**

| + of mixing                                   | − of dogma                                                                          |
| --------------------------------------------- | ----------------------------------------------------------------------------------- |
| Process matches risk and contract type        | Forcing Scrum ceremonies on a support queue (or Waterfall on a startup) wastes time |
| Honest hybrids (Scrumban, time-and-materials) | Manifesto values without shipping working software                                  |

**Thoughts:** _Agile is a means to validate hypotheses and ship; if it does not serve that, drop or shrink it._

**Sources:**

[Doka — Agile](https://doka.guide/tools/agile/)

---

</details>

<details>
  <summary>DevOps</summary>

### DevOps

**Definition:** Culture and practice uniting development and operations — automation, CI/CD, and shared ownership of reliability.

Canonical notes live in [DevOps](./15_devops.md). Pipeline tooling: [CI/CD](./10_build_interview.md#what-is-cicd).

---

</details>

<details>
  <summary>Code refactoring</summary>

### Code refactoring

**Definition:** Refactoring improves internal structure, readability, and maintainability **without changing external behavior**.

**When to use:** After green tests; small steps; before adding features to tangled code.

**Pluses and minuses:**

| +                               | −                                                  |
| ------------------------------- | -------------------------------------------------- |
| Reduces tech debt incrementally | Risky without tests                                |
| Enables safer feature work      | Can become endless "cleanup" without product value |

**Examples:** Extract duplicated validation into `validateUser(input)` — same API, clearer modules.

**Thoughts:** "Refactor" ≠ rewrite. Boy Scout rule: leave code slightly better. Never refactor and change behavior in one commit.

**Sources:**

[refactoring.com](https://refactoring.com/)

---

</details>

---

<a name="sdlc--interview-checklist"></a>

## SDLC — interview checklist

Compact answers for common **process** interview lists. **Theory:** sections above in this file · [MVP & POC](./0_mvp.md) · [Git team workflow](./0_git_team.md) · [CI/CD](./10_build_interview.md#what-is-cicd).

<details>
<summary>What is SDLC?</summary>

### What is SDLC?

Software Development Life Cycle — phases from idea to maintenance. Models: [Waterfall](#waterfall) · [Agile](#agile) · [Scrum](#scrum) · [Kanban](#kanban). Principles: [7_special_sdlc_principles.md](./7_special_sdlc_principles.md).

</details>

<details>
<summary>What is Agile? Code review? CI/CD? Technical debt?</summary>

### What is Agile? Code review? CI/CD? Technical debt?

[Agile](#agile) · [Code review / PRs](./0_git_team.md) · [CI/CD](./10_build_interview.md#what-is-cicd) · [Technical debt](#technical-debt)

</details>

<details>
<summary>Functional vs non-functional requirements?</summary>

### Functional vs non-functional requirements?

**Functional** — what the system **does** (features). **Non-functional** — **how well** (performance, security, a11y, scalability). NFRs often become DoD items (CWV budget, WCAG level).

</details>

<details>
<summary>POC vs Prototype vs MVP? DoD vs DoR?</summary>

### POC vs Prototype vs MVP? DoD vs DoR?

[POC vs Prototype vs MVP](./0_mvp.md#poc-vs-prototype-vs-mvp) · [DoR and DoD](#definition-of-ready-dor-and-definition-of-done-dod)

</details>

<details>
<summary>GitFlow? Stakeholder and RACI? Mentoring vs coaching?</summary>

### GitFlow? Stakeholder and RACI?

[GitFlow](#gitflow-branching-model) · [RACI](#raci-and-stakeholders). **Mentoring** — senior guides craft; **coaching** — asks questions so the person finds their own solution (often external coach).

</details>
