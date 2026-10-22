# Agentic SDLC

AI across the software delivery pipeline — not only in the IDE. **Start with classical SDLC:** [principles](./7_special_sdlc_principles.md) and [six phases](./7_special_sdlc_phases.md) — then multi-agent roles, human release gates, infrastructure (tracker, sandbox, RAG, MCP), and end-to-end lead time.

### Related docs

- [AI workflow hub](./0_ai.md) · [AI-first engineering & SDD](./0_ai_engineering.md) · [Cursor Subagents](./0_ai_cursor_subagents.md) · [SDLC principles](./7_special_sdlc_principles.md) · [SDLC phases](./7_special_sdlc_phases.md) · [SDLC & Agile](./7_special_sdlc.md) · [Full-stack delivery](./0_engineering_fullstack.md) · [CI/CD](./10_build_ci.md)

### Main concepts

| Area            | Topics                                                                                                                                                                                                                  |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Foundations** | [Principles (on this page)](#principles-for-agentic-delivery), [Phases (on this page)](#phases-for-agentic-delivery) — full topics: [principles](./7_special_sdlc_principles.md) · [phases](./7_special_sdlc_phases.md) |
| **Basics**      | [AI in SDLC](#ai-in-sdlc), [Lead time vs coding speed](#lead-time-vs-coding-speed)                                                                                                                                      |
| **Maturity**    | [AI-assisted vs agentic SDLC maturity](#ai-assisted-vs-agentic-sdlc-maturity)                                                                                                                                           |
| **Roles**       | [Multi-agent SDLC roles](#multi-agent-sdlc-roles), [Skeptic agent (critic loop)](#skeptic-agent-critic-loop)                                                                                                            |
| **People**      | [Human gateways in agentic SDLC](#human-gateways-in-agentic-sdlc)                                                                                                                                                       |
| **Platform**    | [Agentic SDLC infrastructure](#agentic-sdlc-infrastructure)                                                                                                                                                             |
| **Quality**     | [Measuring agentic SDLC](#measuring-agentic-sdlc), [Agentic SDLC anti-patterns](#agentic-sdlc-anti-patterns)                                                                                                            |

### Sections

- [Principles for agentic delivery](#principles-for-agentic-delivery)
- [Phases for agentic delivery](#phases-for-agentic-delivery)
- [AI in SDLC](#ai-in-sdlc)
- [Lead time vs coding speed](#lead-time-vs-coding-speed)
- [AI-assisted vs agentic SDLC maturity](#ai-assisted-vs-agentic-sdlc-maturity)
- [Multi-agent SDLC roles](#multi-agent-sdlc-roles)
- [Skeptic agent (critic loop)](#skeptic-agent-critic-loop)
- [Human gateways in agentic SDLC](#human-gateways-in-agentic-sdlc)
- [Agentic SDLC infrastructure](#agentic-sdlc-infrastructure)
- [Measuring agentic SDLC](#measuring-agentic-sdlc)
- [Agentic SDLC anti-patterns](#agentic-sdlc-anti-patterns)
- [Interview Practice](#interview-practice)

---

## Principles for agentic delivery

<details>
<summary>Principles for agentic delivery</summary>

**Definition:** Before agents orchestrate delivery, the team needs shared **SDLC principles** — the rules that keep any lifecycle model honest. Canonical deep dive: [SDLC principles](./7_special_sdlc_principles.md) (sibling topic under **AI → SDLC**).

**How it works:**

| Principle               | Why it matters for agents                                                     |
| ----------------------- | ----------------------------------------------------------------------------- |
| **Cyclicity**           | Agents must hand context across releases, not one-shot chats                  |
| **Systematic approach** | Each phase needs artifacts agents can read and update                         |
| **Quality orientation** | [Skeptic loops](#skeptic-agent-critic-loop) + CI, not “ship the diff”         |
| **Flexibility**         | Same phases under Waterfall or Agile — agents follow your tracker             |
| **Transparency**        | Task status and attachments = agent source of truth                           |
| **Risk management**     | Human [gateways](#human-gateways-in-agentic-sdlc) on architecture and release |
| **User focus**          | Requirements agents must trace to acceptance criteria                         |

**When to use:** Read this block when you land on **Home → AI → Agentic SDLC** without classical SDLC notes open; then open [SDLC principles](./7_special_sdlc_principles.md) for interview-level detail per principle.

**Pluses and minuses:**

| +                          | −                                                  |
| -------------------------- | -------------------------------------------------- |
| Prevents “agents on chaos” | Skipping principles → fast incompatible automation |

**Thoughts:** Agentic SDLC is layer **on top of** discipline, not a substitute for it — see [anti-patterns](#agentic-sdlc-anti-patterns).

**Sources:** [SDLC principles](./7_special_sdlc_principles.md) · [SDLC & Agile](./7_special_sdlc.md)

</details>

## Phases for agentic delivery

<details>
<summary>Phases for agentic delivery</summary>

**Definition:** **Agentic SDLC** still walks the **six classical phases**; agents swap in for role-based work inside each phase. Full definitions and deliverables: [SDLC phases](./7_special_sdlc_phases.md).

**How it works:**

| Phase                                                                     | Typical agent role                       | Human gateway (examples)   |
| ------------------------------------------------------------------------- | ---------------------------------------- | -------------------------- |
| [Planning](./7_special_sdlc_phases.md#planning)                           | Scope/priority drafts                    | Budget, milestone sign-off |
| [Requirements](./7_special_sdlc_phases.md#requirements-analysis)          | **Analyst** — spec, acceptance criteria  | Approve scope / TZ         |
| [Design](./7_special_sdlc_phases.md#design)                               | **Architect** — solution design, tasks   | Approve architecture       |
| [Development](./7_special_sdlc_phases.md#development)                     | **Developer** — code, migrations         | PR review (sample or full) |
| [Testing & deployment](./7_special_sdlc_phases.md#testing-and-deployment) | **Tester**, **Deploy** — tests, pipeline | Release approval           |
| [Support](./7_special_sdlc_phases.md#support)                             | Triage summaries, patch proposals        | Production ownership       |

Phases **overlap** in Agile; agents still map to artifacts, not only to org chart titles. Diagram: [Phases overview](./7_special_sdlc_phases.md#overview).

**When to use:** Mapping [multi-agent SDLC roles](#multi-agent-sdlc-roles) to your tracker columns; explaining AI in SDLC in interviews.

**Pluses and minuses:**

| +                                             | −                                                          |
| --------------------------------------------- | ---------------------------------------------------------- |
| One table from classical SDLC → agentic model | Phase names in your company may differ — align the tracker |

**Examples:** Requirements agent output feeds architect agent; skeptic runs before human architecture gate.

**Thoughts:** Coding assistants only touch **development**; agentic ROI needs requirements → deploy covered — [lead time](#lead-time-vs-coding-speed).

**Sources:** [SDLC phases](./7_special_sdlc_phases.md) · [AI augmentation of SDLC phases](./7_special_sdlc.md#ai-augmentation-of-sdlc-phases)

</details>

---

## AI in SDLC

<details>
<summary>AI in SDLC</summary>

**Definition:** **AI in SDLC** (Software Development Life Cycle) means applying AI to **manage and produce artifacts** across the delivery pipeline — requirements, design docs, code, tests, release steps — not only to autocomplete code inside one developer’s editor.

**How it works:**

- Classic [SDLC phases](./7_special_sdlc_phases.md) still apply (requirements → design → build → test → deploy → maintain); AI changes **who produces each artifact** and **how fast context moves** between stages.
- In a mature setup, agents own **structured handoffs**: spec from analysis, architecture notes, implementation, review feedback, test evidence — linked to the same task in a tracker.
- [Spec-driven development (SDD)](./0_ai_engineering.md#spec-driven-development-sdd) is the contract between human intent and agent output at each stage.

**When to use:** When release dates are blocked by coordination (reviews, architecture, QA queues), not by typing speed. Skip full pipeline automation for one-line fixes.

**Pluses and minuses:**

| +                                                         | −                                                           |
| --------------------------------------------------------- | ----------------------------------------------------------- |
| Targets end-to-end **lead time**, not local coding speed. | Useless on chaotic repos without standards and tool access. |
| Artifacts become auditable steps, not chat-only history.  | Requires investment in tracker, CI, and context (RAG/MCP).  |

**Thoughts:** “AI in SDLC” is not a product slogan — it is an operating model. Pair with [AI-first engineering](./0_ai_engineering.md#ai-first-engineering): humans set goals and approve gates; agents execute within specs.

**Sources:** [Model Context Protocol](https://modelcontextprotocol.io/) · [Doka — Agile](https://doka.guide/tools/agile/)

</details>

## Lead time vs coding speed

<details>
<summary>Lead time vs coding speed</summary>

**Definition:** **Lead time** (or **cycle time**) is elapsed time from **task creation** to **value in production** (or a defined “done” environment). **Coding speed** is how fast someone (or an assistant) produces a diff. Faster coding does not automatically shorten lead time if other stages are the bottleneck.

**How it works:**

- In many enterprise systems, implementation is a **minor share** of calendar time; the rest is requirements churn, architecture sign-off, code review queues, integration, regression, and release governance.
- IDE assistants optimize the **implementation** slice only; they do not schedule the architect, run CI for you, or approve production deploy.
- Agentic SDLC tries to **parallelize and automate** upstream/downstream artifacts (spec, tests, deploy scripts) while keeping **human gateways** where risk is high.

**When to use:** Explaining why Copilot/Cursor did not move the release date; choosing metrics for AI initiatives (measure lead time, not lines/hour).

**Pluses and minuses:**

| +                                              | −                                            |
| ---------------------------------------------- | -------------------------------------------- |
| Aligns AI investment with business TTM         | Easy to misread vendor “80/20” ratios as law |
| Explains the “fast code, slow release” paradox | Still need team-specific baselines           |

**Examples:**

- Assistant generates a component in an hour; merge waits a week for architecture review → lead time unchanged.
- Agentic flow: analyst agent drafts spec → human approves architecture → dev/review/test agents iterate → human approves release → lead time drops if gates stay thin.

**Thoughts:** Treat published “coding is 20% of time” as **directional**, not a budget formula. Measure **your** lead time and stage times.

**Sources:** [DORA metrics — Google Cloud](https://cloud.google.com/devops/state-of-devops)

</details>

## AI-assisted vs agentic SDLC maturity

<details>
<summary>AI-assisted vs agentic SDLC maturity</summary>

**Definition:** A three-level maturity model for AI in delivery: **(1) AI-assisted** (developer + IDE hints), **(2) AI pair** (model writes large chunks, human co-edits), **(3) agentic SDLC** (specialized agents cover the pipeline; human orchestrates and approves).

**How it works:**

| Level          | Who does the work               | Human role              | What speeds up             | Main limit                         |
| -------------- | ------------------------------- | ----------------------- | -------------------------- | ---------------------------------- |
| 1 AI-assisted  | Developer with completions/chat | Executor (Developer)    | Boilerplate, syntax lookup | Only the coding step               |
| 2 AI pair      | Model drafts; developer edits   | Reviewer / co-author    | Single features, refactors | Context often one task / few files |
| 3 Agentic SDLC | Agent team (spec → deploy)      | Orchestrator / approver | End-to-end delivery slice  | Needs formal process + infra + MCP |

Level 3 maps to [multi-agent SDLC roles](#multi-agent-sdlc-roles) and [infrastructure](#agentic-sdlc-infrastructure). Level 1–2 map to [AI-assisted workflow](./0_ai.md#ai-assisted-workflow-cursor-copilot-claude-code).

**When to use:** Roadmaps and interviews (“where are we on AI maturity?”). Most teams should master level 2 + strong CI before claiming level 3.

**Pluses and minuses:**

| +                                    | −                                       |
| ------------------------------------ | --------------------------------------- |
| Clear vocabulary for leaders and ICs | Level 3 is not “more Copilot licenses”  |
| Sets expectations on prerequisites   | Easy to skip gates and call it “agents” |

**Thoughts:** Level 3 without [SDD](./0_ai_engineering.md#spec-driven-development-sdd) and [verification loops](./0_ai_cursor_core.md#test-driven-agent-loops) is faster wrong code at scale.

**Sources:** [Cursor Agent overview](https://cursor.com/docs/agent/overview)

</details>

## Multi-agent SDLC roles

<details>
<summary>Multi-agent SDLC roles</summary>

**Definition:** In **agentic SDLC**, work is organized by **roles** (often separate agents or subagent profiles), each enriching a shared task with artifacts — not by handoffs between siloed departments only.

**How it works:**

| Role          | Input                      | Output                                                                    |
| ------------- | -------------------------- | ------------------------------------------------------------------------- |
| **Analyst**   | Raw idea / ticket stub     | Requirements, use cases, acceptance criteria                              |
| **Architect** | Approved spec              | Technical design, data/API impact, subtask breakdown                      |
| **Developer** | Design + tasks             | Code, migrations, UI — aligned to standards                               |
| **Reviewer**  | Diff                       | Style, security, logic findings                                           |
| **Tester**    | Acceptance criteria        | Unit/integration tests, run results, failure logs                         |
| **Deploy**    | Green tests + merge policy | Build artifact, deploy to staging/prod per pipeline                       |
| **Skeptic**   | Others’ artifacts          | Critical feedback loops (see [Skeptic agent](#skeptic-agent-critic-loop)) |

Roles can run **sequentially** (waterfall-like handoffs) or **overlapped** (Agile slice). Context must travel on the **task** (tracker + repo + linked docs), not only in chat.

In Cursor repos, role-like isolation often uses [Subagents](./0_ai_cursor_subagents.md) (`explore`, custom auditors) while the parent Agent orchestrates.

**When to use:** Large features, platform extensions, or repeated delivery patterns where the same pipeline runs many times.

**Pluses and minuses:**

| +                                        | −                                           |
| ---------------------------------------- | ------------------------------------------- |
| Mirrors how senior teams already think   | Role explosion if every step is a new agent |
| Enables parallel review + implementation | Handoffs fail without artifact schemas      |

**Examples:**

- **Dependency dashboard:** analyst defines graph UX and click behavior → architect defines API for task graph → developer implements UI + server script → skeptic flags deep-tree performance → human approves architecture and merge.
- **Multi-score backlog:** analyst lists scoring frameworks → architect designs metric tables and recalc rules → developer adds schema + triggers → tester checks edge math → human approves scoring UX.

**Thoughts:** Vendor case studies that claim **weeks → days** usually assume a **unified platform** (single data model, generated UI, in-platform scripts). Generic microservice repos rarely get the same multiplier — measure locally.

**Sources:** [Subagents](https://cursor.com/docs/subagents)

</details>

## Skeptic agent (critic loop)

<details>
<summary>Skeptic agent (critic loop)</summary>

**Definition:** A **skeptic agent** (critic) does **not** implement features. It **challenges** other agents’ outputs — architecture gaps, unsafe code, weak tests — and sends work back until quality thresholds or an **iteration limit** is hit.

**How it works:**

- Runs in a **feedback loop** with analyst/architect/developer/tester agents before artifacts reach a human reviewer.
- Checks include: performance traps, missing acceptance coverage, inconsistent design with repo conventions, security basics.
- Maps cleanly to **readonly** [custom subagents](./0_ai_cursor_subagents.md#example-subagents) (`task-verifier`, `locale-parity-auditor`, security review) and built-in review modes.

**When to use:** Any agentic pipeline where speed would otherwise skip review depth. Optional for trivial, well-tested chores.

**Pluses and minuses:**

| +                                          | −                                                       |
| ------------------------------------------ | ------------------------------------------------------- |
| Catches “plausible but wrong” at scale     | Token cost if skeptic/dev argue many rounds             |
| Reduces human nitpicking on obvious issues | Bad prompts → high **reject rate** without quality gain |

**Examples:**

- Skeptic returns dashboard work: “Rendering unbounded dependency depth will block the main thread — paginate or virtualize.”
- Skeptic returns tests: “No case for zero weight / overflow in score calculator.”

**Thoughts:** Track **skeptic reject rate** — too high means weak specs or missing [context](./0_ai.md#context-window-llm-context); too low may mean a rubber-stamp critic.

**Sources:** [Subagents vs skills](./0_ai_cursor_subagents.md#subagents-vs-skills)

</details>

## Human gateways in agentic SDLC

<details>
<summary>Human gateways in agentic SDLC</summary>

**Definition:** **Human gateways** are mandatory approval points where a **person** (tech lead, architect, release owner) accepts risk before the pipeline continues — even when agents produced the artifacts.

**How it works:**

Typical gates:

1. **Spec / scope** — business intent and acceptance criteria are right.
2. **Architecture** — design fits the system, data model, and non-functional requirements.
3. **Release** — merge/deploy to production (or customer-facing env) with rollback plan.

Humans also retain **legal and operational accountability** — agents are not authors of record for production incidents ([Coding agents on real projects](./0_ai_engineering.md#coding-agents-on-real-projects)).

**When to use:** Always in enterprise and regulated contexts; scale gate strictness with blast radius (auth, payments, PII, migrations).

**Pluses and minuses:**

| +                                 | −                                                |
| --------------------------------- | ------------------------------------------------ |
| Preserves product judgment        | Gates can reintroduce bottlenecks if too many    |
| Required for compliance and trust | “Human approves unread diff” defeats the purpose |

**Thoughts:** Goal is **fewer** human touchpoints, each **higher signal** — not zero humans.

**Sources:** [AI-first engineering — ownership](./0_ai_engineering.md#ai-first-engineering)

</details>

## Agentic SDLC infrastructure

<details>
<summary>Agentic SDLC infrastructure</summary>

**Definition:** Minimum **platform** for reliable agentic SDLC: structured tasks, executable environment, indexed corporate context, and **tool integrations** so agents act on real systems — not only generate text.

**How it works:**

| Pillar                   | Purpose                                                                                                               |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| **Task tracker**         | Single entry: description, status, comments, attached artifacts (spec, links, CI results)                             |
| **Execution sandbox**    | Agent can build, test, read logs (IDE agent, CLI, CI runner) and fix failures iteratively                             |
| **Context (RAG + repo)** | Schema docs, API catalogs, ADRs, prior commits — architect/dev agents need truth, not guesses                         |
| **Integrations (MCP)**   | [Model Context Protocol](https://modelcontextprotocol.io/) servers expose trackers, DB, deploy, docs to agents safely |

Low-code / unified platforms reduce glue (one data model, generated CRUD UI) but are optional; generic repos can reach the same bar with discipline + MCP + CI.

**When to use:** Before scaling past level-2 [maturity](#ai-assisted-vs-agentic-sdlc-maturity) on production codebases.

**Pluses and minuses:**

| +                       | −                                     |
| ----------------------- | ------------------------------------- |
| Agents become operators | Up-front integration cost             |
| Reproducible runs in CI | Secret sprawl if MCP is misconfigured |

**Examples:**

- MCP: create branch, open PR, fetch issue, query read-only schema, run test workflow.
- MyConspect-style: tracker = git issues/PRs; sandbox = `mc-app` CI; context = `mc-content` + skills; skeptics = `.cursor/agents/*`.

**Thoughts:** “Drop agents on a chaotic repo” is the fastest path to incompatible code — formalize stages in the tracker first ([anti-patterns](#agentic-sdlc-anti-patterns)).

**Sources:** [Model Context Protocol — specification](https://modelcontextprotocol.io/specification/2025-03-26)

</details>

## Measuring agentic SDLC

<details>
<summary>Measuring agentic SDLC</summary>

**Definition:** Metrics for agentic SDLC focus on **end-to-end delivery and quality**, not characters typed per minute.

**How it works:**

| Metric                            | What it tells you                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Lead time / cycle time**        | Task created → production (primary business metric)                                                   |
| **Idea → architecture time**      | How fast raw input becomes an approved design                                                         |
| **Skeptic reject rate**           | Quality of specs/context vs executor agents                                                           |
| **Human touchpoints per feature** | How often a lead must intervene mid-pipeline (goal: down, not zero)                                   |
| **AI code churn**                 | Share of agent-written code rewritten soon after merge (early adoption often ~20% — track your trend) |
| **Defect escape / rollback rate** | Speed without quality is misconfiguration                                                             |
| **Token cost per task**           | Deep agent debates can erase time savings                                                             |

Pair with [DORA](https://cloud.google.com/devops/state-of-devops)-style deployment frequency and change failure rate where available.

**When to use:** Before/after pilots; executive reviews; tuning skeptic iteration limits.

**Pluses and minuses:**

| +                                 | −                                   |
| --------------------------------- | ----------------------------------- |
| Stops “we bought Copilot” theater | Requires baseline measurement first |

**Thoughts:** If lead time drops but escapes rise, fix tests and [skeptic loops](#skeptic-agent-critic-loop) before adding more agents.

**Sources:** [DORA metrics — Google Cloud](https://cloud.google.com/devops/state-of-devops)

</details>

## Agentic SDLC anti-patterns

<details>
<summary>Agentic SDLC anti-patterns</summary>

**Definition:** Common failure modes when organizations adopt “AI SDLC” without process or platform readiness.

**How it works:**

| Anti-pattern                  | Consequence                              | Mitigation                                                                 |
| ----------------------------- | ---------------------------------------- | -------------------------------------------------------------------------- |
| Agents on **chaotic process** | Hallucinated process, wrong architecture | Formalize tracker stages and standards first ([SDLC](./7_special_sdlc.md)) |
| **No skeptic**                | Fast junk, security holes, debt          | Mandatory critic loops before human review                                 |
| **No human gateways**         | Liability, vision drift                  | Approve spec, architecture, release                                        |
| **Isolated context**          | Code breaks neighbors                    | Repo-wide context, MCP, RAG, CI                                            |
| **Coding-only KPI**           | Investment with no TTM gain              | Measure [lead time](#lead-time-vs-coding-speed)                            |
| **Unbounded agent loops**     | High token bill, no ship date            | Iteration caps + escalate to human                                         |

**When to use:** Readiness reviews, post-mortems, interview “how would you roll out agents?”

**Pluses and minuses:**

| +               | −              |
| --------------- | -------------- |
| Cheap checklist | Not exhaustive |

**Thoughts:** Agentic SDLC is **process automation** — garbage in, garbage out at machine speed.

**Sources:** [AI-first engineering](./0_ai_engineering.md#ai-first-engineering)

</details>

## Interview Practice

### Questions & Answers

**Q: How is “AI in SDLC” different from using Copilot in the IDE?**  
A: SDLC scope covers requirements through deploy artifacts and coordination; IDE tools mainly speed implementation. See [AI in SDLC](#ai-in-sdlc) vs [maturity level 1](#ai-assisted-vs-agentic-sdlc-maturity).

**Q: Why did coding get faster but releases did not?**  
A: Bottlenecks moved to review, architecture, testing, and release — measure [lead time](#lead-time-vs-coding-speed), not typing.

**Q: What is a skeptic agent for?**  
A: Adversarial quality pass on other agents’ work before human gates — like readonly [subagents](./0_ai_cursor_subagents.md) in Cursor.

**Q: Where must humans stay in the loop?**  
A: Spec approval, architecture approval, and production release at minimum — plus accountability for merges ([Human gateways](#human-gateways-in-agentic-sdlc)).

**Q: What infrastructure do agents need beyond an LLM API?**  
A: Tracker, sandbox/CI, indexed context, MCP (or equivalent) integrations — [Agentic SDLC infrastructure](#agentic-sdlc-infrastructure).

**Q: Name one metric that proves agentic SDLC works.**  
A: Lead time from backlog to production, with stable or improved defect escape / rollback rate.

**Q: Which classical SDLC pieces should you read before agentic SDLC?**  
A: [Principles](#principles-for-agentic-delivery) and [six phases](#phases-for-agentic-delivery) — full notes under [principles](./7_special_sdlc_principles.md) and [phases](./7_special_sdlc_phases.md).

**Q: Which phase does a “coding assistant” mainly accelerate?**  
A: **Development** only — agentic SDLC targets all phases in [the phase map](#phases-for-agentic-delivery).
