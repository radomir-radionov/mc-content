# AI-first engineering & SDD

How developers delegate work to AI agents, verify outcomes, and write specs before code. Pair with [product AI-first vs augmented](./0_ai.md#ai-first-approach) when the interview asks about the _product_, not the _team_.

### Related docs

- [AI workflow hub](./0_ai.md) · [Agentic SDLC](./0_ai_agentic_sdlc.md) · [Full-stack competency map](./0_engineering_fullstack.md) · [Cursor — verification loops](./0_ai_cursor_core.md#test-driven-agent-loops) · [Cursor Skills & rules](./0_ai_cursor_skills.md) · [SDLC & Agile](./7_special_sdlc.md) · [MVP & judgment](./0_mvp.md)

### Main concepts

| Area        | Topics                                                            |
| ----------- | ----------------------------------------------------------------- |
| **Mindset** | [AI-first engineering](#ai-first-engineering)                     |
| **Process** | [Spec-driven development (SDD)](#spec-driven-development-sdd)     |
| **Agents**  | [Coding agents on real projects](#coding-agents-on-real-projects) |

### Sections

- [AI-first engineering](#ai-first-engineering)
- [Spec-driven development (SDD)](#spec-driven-development-sdd)
- [Coding agents on real projects](#coding-agents-on-real-projects)
- [Interview Practice](#interview-practice)

---

## AI-first engineering

<details>
<summary>AI-first engineering</summary>

**Definition:** **AI-first engineering** is how a developer works when agents and models are part of the toolchain: you decide what to delegate to AI, how to verify the result, and you remain accountable for the shipped solution. It is **not** the same as an **AI-first product** (where the model _is_ the product) — see [AI-first approach](./0_ai.md#ai-first-approach).

**How it works:**

1. **Scope delegation** — split a large task into steps you can check independently (one behavior change, one file group, one command output).
2. **Context** — give the agent the spec, relevant files, constraints, and acceptance criteria; remove noise that steers the model wrong.
3. **Steering** — watch for wrong assumptions early (wrong API, wrong repo, scope creep) and correct course before a huge diff lands.
4. **Verification** — read the diff, challenge claims, run tests/linters/build, and understand blast radius (types, migrations, security).
5. **Tooling** — pick model and tools for quality, latency, and cost (Agent vs Tab, subagents, MCP docs, smaller model for grep-only tasks).
6. **Ownership** — the agent proposes; the developer approves what merges and what ships.

**When to use:** Any non-trivial feature, refactor, or production fix with Cursor/Copilot/Claude Code. Skip formal steps for a one-line typo — keep the habit for work that affects users or teammates.

**Pluses and minuses:**

| +                                                          | −                                                          |
| ---------------------------------------------------------- | ---------------------------------------------------------- |
| Faster iteration when verification is disciplined.         | Easy to “approve” unread code and inherit subtle bugs.     |
| Forces explicit acceptance criteria before implementation. | Without specs, agents optimize for plausible-looking code. |
| Teaches when _not_ to use the heaviest Agent run.          | Cost and context limits punish vague mega-prompts.         |

**Examples:**

- **Good step:** “Add `Topic` for `0_ai_engineering.md` in catalog; acceptance: `npm run build` green, EN+RU twins exist.”
- **Weak step:** “Make the handbook better with AI.”
- **Good verification:** Run tests, read migration SQL, confirm no `NEXT_PUBLIC_` secret added.
- **Weak verification:** “Agent said it works.”

**Thoughts:** AI-first engineering is **AI-augmented delivery with human gatekeeping**. If you cannot explain what changed and how you verified it, you are not done — regardless of model confidence.

**Sources:** [Cursor Agent overview](https://cursor.com/docs/agent/overview) · [Test-driven agent loops](./0_ai_cursor_core.md#test-driven-agent-loops)

</details>

## Spec-driven development (SDD)

<details>
<summary>Spec-driven development (SDD)</summary>

**Definition:** **Spec-driven development (SDD)** means you define behavior, constraints, and **acceptance criteria** before (or in lockstep with) implementation. The spec is the contract you give AI agents and the checklist you use to reject wrong output.

**How it works:**

1. **Specify** — user-visible behavior, edge cases, non-goals, data contracts, and “done when” checks (tests, screenshots, metrics).
2. **Task the agent** — break the spec into ordered, verifiable prompts; one agent run should map to one slice of the spec when possible.
3. **Implement & verify** — code and tests must trace back to spec items; failing tests mean spec or code is wrong — fix the right layer.
4. **Sync on change** — when requirements shift, update the spec first (or in the same PR), then code and tests so all three stay aligned.

SDD complements [SDLC & Agile](./7_special_sdlc.md): Agile decides _cadence_; SDD decides _what “done” means_ before the agent writes the diff. For **multi-agent** handoffs across stages (spec → architecture → code → test), see [Agentic SDLC](./0_ai_agentic_sdlc.md).

**When to use:** New features, API changes, handbook topics, refactors that move behavior, and any work you would hand to another engineer with a ticket. Optional for trivial fixes only.

**Pluses and minuses:**

| +                                                           | −                                                    |
| ----------------------------------------------------------- | ---------------------------------------------------- |
| Agents produce less scope drift when criteria are explicit. | Up-front spec time feels slow until rework drops.    |
| Reviewers can audit spec → tests → code in one pass.        | Stale specs become lies — treat them like code debt. |
| Works with AI-first engineering: spec is the guardrail.     | “Spec in chat only” does not count — write it down.  |

**Examples:**

Handbook topic spec:

```text
Behavior: new sidebar topic 0_ai_engineering.md (EN + RU).
Constraints: conspect slot markers; catalog row in mc-app.
Acceptance: check-order --strict (en+ru); build passes.
Out of scope: changing parser rules.
```

Full-stack API spec (illustrative):

```text
Behavior: POST /discounts/apply returns 200 + updated totals, 409 if expired.
Data: migration add discounts.code (unique, indexed).
Frontend: inline error on 409; disable submit while pending.
Acceptance: integration test for 409; migration runs in CI.
Non-goals: admin UI for codes.
```

**Thoughts:** SDD is mandatory for professional work with agents: the model will always _produce_ code; only a spec tells you whether it is _correct_.

**Sources:** [Agile — Doka](https://doka.guide/tools/agile/) · [AI-first engineering](#ai-first-engineering)

</details>

## Coding agents on real projects

<details>
<summary>Coding agents on real projects</summary>

**Definition:** **Coding agents on real projects** means practical use of **Claude Code**, **OpenAI Codex** (CLI/IDE), **Cursor Agent/CLI**, or similar tool-using assistants on a **production codebase** — with the same bar as human work: branches, review, CI, and rollback awareness.

**How it works:**

| Tool                   | Typical surface                             | Real-project fit                                                                                      |
| ---------------------- | ------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Cursor Agent / CLI** | Editor + `agent -p`                         | Multi-file refactors, MCP docs, handbook-style repos                                                  |
| **Claude Code**        | Terminal `claude`, headless `-p`            | SSH boxes, scripted loops; `.claude/skills/`                                                          |
| **OpenAI Codex**       | IDE/CLI, `.codex/skills/`, `.codex/agents/` | Same agent pattern; align skills with Cursor where team uses both ([Skills](./0_ai_cursor_skills.md)) |
| **GitHub Copilot**     | Inline + Chat/Agent in VS Code              | Team standard IDE; `.github/copilot-instructions.md`                                                  |

Shared **agent pattern:** read / edit / bash / grep / MCP — details in [AI-assisted workflow](./0_ai.md#ai-assisted-workflow-cursor-copilot-claude-code).

**Session checklist (production repo):**

1. Branch from main; paste **spec + acceptance** ([SDD](#spec-driven-development-sdd)), not a vague goal.
2. Attach only needed files; exclude secrets ([Secrets](./0_engineering_fullstack.md#secrets-and-access-control)).
3. Ask for smallest diff that satisfies one spec item; review every hunk.
4. Run locally what CI runs: `lint`, `test`, `build` (same commands as [CI/CD](./10_build_ci.md)).
5. Open PR yourself; agent text is not a substitute for description or tests.
6. If agent touched **migrations**, **auth**, or **API contracts**, re-read blast radius ([Full-stack scope](./0_engineering_fullstack.md#full-stack-delivery-scope)).

**When to use:** Job requirements citing Claude Code, Codex, or “AI-native” delivery; daily feature work once [Cursor Core](./0_ai_cursor_core.md) habits exist.

**Pluses and minuses:**

| +                                            | −                                                      |
| -------------------------------------------- | ------------------------------------------------------ |
| Large refactors across modules faster        | Demo-quality code without CI is a liability            |
| Shared skills/rules across Cursor/Codex dirs | Each product has different approval and context limits |

**Thoughts:** Interviewers care that you shipped **with** agents on real repos — not that you watched a tutorial. Point to verification and SDD, not model names alone.

**Sources:** [Claude Code — glossary/tools](https://code.claude.com/docs/en/glossary) · [Cursor Agent overview](https://cursor.com/docs/agent/overview)

</details>

## Interview Practice

### Questions & Answers

**Q: How is AI-first _engineering_ different from AI-first _product_?**  
A: Product = the app cannot exist without the model. Engineering = the _team_ uses AI to build; the developer still owns verification and merge decisions. See [AI-first approach](./0_ai.md#ai-first-approach) vs [AI-first engineering](#ai-first-engineering).

**Q: What belongs in a spec before you ask an agent to code?**  
A: Behavior, constraints, acceptance criteria, and explicit non-goals — enough that a reviewer can say pass/fail without guessing intent.

**Q: Who is responsible when agent-generated code breaks production?**  
A: The developer who merged it — agents are tools, not authors of record.

**Q: When should you update the spec vs only the code?**  
A: When requirements or “done when” changes; never let the spec drift while code and tests move ahead.

**Q: Does “Claude Code / Codex experience” mean knowing one vendor?**  
A: No — it means tool-using agents on real repos: spec, context, CI verification, and accountable merge. See [Coding agents on real projects](#coding-agents-on-real-projects).
