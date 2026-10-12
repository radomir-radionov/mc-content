# Cursor — Core

Everyday Agent habits: shortcuts, Run Modes, verification loops, and community configs. Pair with [Skills](./0_ai_cursor_skills.md), [Rules](./0_ai_cursor_rules.md), [Plugins](./0_ai_cursor_plugins.md), [Subagents](./0_ai_cursor_subagents.md), and [Hooks](./0_ai_cursor_hooks.md) for the deeper feature pages.

### Related docs

- [AI workflow](./0_ai.md) · [Context window](./0_ai.md#context-window-llm-context) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area          | Topics                                                                                   |
| ------------- | ---------------------------------------------------------------------------------------- |
| **Habits**    | [Everyday shortcuts](#everyday-shortcuts), [Agent autonomy](#agent-autonomy-run-modes)   |
| **Verify**    | [Test-driven agent loops](#test-driven-agent-loops), [Debug with logs](#debug-with-logs) |
| **Share**     | [Cursor Directory](#cursor-directory)                                                    |
| **Reference** | [Sources](#sources)                                                                      |

### Sections

- [Everyday shortcuts](#everyday-shortcuts)
- [Agent autonomy (Run Modes)](#agent-autonomy-run-modes)
- [Test-driven agent loops](#test-driven-agent-loops)
- [Debug with logs](#debug-with-logs)
- [Cursor Directory](#cursor-directory)
- [Sources](#sources)
- [Interview Practice](#interview-practice)

---

## Everyday shortcuts

<details>
<summary>Everyday shortcuts</summary>

**Definition:** Fast Cursor habits for inline edit, Agent chat, Tab completions, Source Control commit messages, and optional bug review — before you reach for a full multi-file Agent run.

**How it works:**

- **Cmd/Ctrl+K (editor):** inline edit on the selection. Prefer when the change is local and you do not need tools, MCP, or a long plan.
- **Cmd/Ctrl+I:** open Agent (side pane). Selection is attached as context. Prefer for multi-file work, shell, and verification loops.
- **Cmd/Ctrl+K (terminal):** natural-language → shell. Useful for one-off git/list/filter commands you do not want to memorize.
- **Tab:** accept gray ghost-text; a second Tab often jumps to the next edit point (feel differs from plain autocomplete).
- **Commit message:** Source Control → wand icon generates a draft from the staged diff. Treat it as a starting point, not final history.
- **Bug finder:** Command Palette → “bug finder” — compares your branch to main for likely regressions. Not a substitute for tests/CI.

**When to use:** Daily micro-edits and commits. Skip Agent for a three-line rename when Cmd+K or Tab is enough.

**Pluses and minuses:**

| +                                                | −                                                               |
| ------------------------------------------------ | --------------------------------------------------------------- |
| Faster loop for local tweaks.                    | Agent rules/MCP often do not apply the same way to Tab / Cmd+K. |
| Commit wand + bug finder catch weak diffs early. | Generated messages and bug finder are hints — still review.     |

**Thoughts:** Prefer the smallest surface that finishes the job — Tab → Cmd+K → Agent — so you keep context and cost under control.

**Sources:** [Cursor Agent overview](https://cursor.com/docs/agent/overview) · [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Agent autonomy (Run Modes)

<details>
<summary>Agent autonomy (Run Modes)</summary>

**Definition:** **Run Modes** (Settings → Agents → Approvals & Execution) control how much shell, MCP, and Fetch the Agent may execute without asking. Community posts still say “YOLO”; that maps closest to **Run Everything**, while **Auto-review** is the safer default for long unattended loops.

**How it works:**

| Mode               | Behavior                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **Auto-review**    | Allowlisted calls run now; sandboxed shell when possible; other calls go through a classifier. Recommended default. |
| **Allowlist**      | Only listed actions skip approval (optional sandbox). Deterministic “trusted commands only.”                        |
| **Run Everything** | Every tool call runs with no prompts. Highest risk; use when you accept full autonomy.                              |

- Steer Auto-review with natural-language `autoRun.allow_instructions` / `block_instructions` in `~/.cursor/permissions.json` or `.cursor/permissions.json` (project).
- Extra guards (browser, file-deletion, external-file) can still require approval even in a permissive mode.
- Cloud Agents do **not** use local Run Modes — they run on an isolated machine.

**When to use:** Auto-review for “run tests / tsc / build until green” loops. Allowlist for a small set of known-safe commands. Run Everything only when you are watching closely and accept the risk. Never allow destructive git/network without review.

**Pluses and minuses:**

| +                                                                   | −                                             |
| ------------------------------------------------------------------- | --------------------------------------------- |
| Agent can iterate on failures without babysitting every `npm test`. | Classifier mistakes; not a security boundary. |
| Project `permissions.json` can be shared with the team.             | Team/model policy can disable Auto-review.    |

**Examples:**

Prompt shape that pairs well with Auto-review / allowlisted test+build:

```text
any kind of tests are always ok (vitest, npm test, nr test, …).
basic build commands (build, tsc) are ok. mkdir/touch are ok.
```

**Thoughts:** Older “YOLO mode” write-ups describe the same idea — let the agent run verification commands. Prefer Auto-review + explicit allow/block instructions over blind Run Everything.

**Sources:** [Run Modes](https://cursor.com/docs/agent/security/run-modes) · [permissions.json](https://cursor.com/docs/reference/permissions) · [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Test-driven agent loops

<details>
<summary>Test-driven agent loops</summary>

**Definition:** For non-trivial Agent work, ask for **tests first**, then implementation, then **run until green** — so correctness comes from the suite, not from one-shot generation.

**How it works:**

- Add one line to the task: “Write tests first, then the code, then run the tests and update the code until tests pass.”
- With Auto-review (or an allowlist that includes your test/build commands), the Agent can iterate without you approving every `npm test`.
- Grow robustness from real failures: paste a failing input/log → “write a test for this, then fix until the suite passes.”
- End a session with a fast gate: “run `tsc` / lint / pre-PR script until it passes” (keep a project `pre-PR` that is CI-shaped but faster).

**When to use:** Converters, parsers, tricky business rules, anything that fails a one-shot. Skip for pure copy/rename when a TypeScript error already defines “done.”

**Pluses and minuses:**

| +                                              | −                                                |
| ---------------------------------------------- | ------------------------------------------------ |
| Agent gets a concrete stop condition.          | Bad tests lock in wrong behavior — review cases. |
| Reuses existing suite as a regression harness. | Still stop/redirect when the Agent drifts.       |

**Examples:**

```text
Create a function that converts markdown to HTML.
Write tests first, then the code, then run the tests and update until they pass.
```

```text
I've got build errors. Run nr build, fix them, then run build until it passes.
```

**Thoughts:** You still babysit — hit stop when it goes off track — but the loop replaces “QA by chat.”

**Sources:** [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips) · [Cursor Agent overview](https://cursor.com/docs/agent/overview)

</details>

## Debug with logs

<details>
<summary>Debug with logs</summary>

**Definition:** When static reasoning stalls, have the Agent **instrument** the code, you **run** it, then paste **raw logs** back so the next fix is driven by runtime evidence.

**How it works:**

1. Ask: “Add logs at key points so we can see what is going on. I will run the code and paste the output.”
2. Run the app/repro yourself (or let Agent run it if safe under your Run Mode).
3. Paste the log output: “Here is the log. What is causing the issue, and how do we fix it?”
4. Repeat with tighter logs if needed; remove temporary logging when done.

**When to use:** Heisenbugs, wrong branch taken, “works in my head / fails at runtime.” Skip when a failing unit test already pins the bug.

**Thoughts:** Treat the Agent like a junior who is great at adding instrumentation and reading dumps — you own the environment and the final judgment.

**Sources:** [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Cursor Directory

<details>
<summary>Cursor Directory</summary>

**Definition:** [cursor.directory](https://cursor.directory/) is a **community** catalog of Cursor rules, MCP servers, and related plugins ranked by what developers are installing — a discovery layer next to Cursor’s own marketplace.

**How it works:**

- Browse trending **rules** (Next.js, React Native, NestJS, …), **MCP servers** (GitHub, Supabase, Stripe, observability, …), and community plugins.
- Copy or install into your project as [rules](./0_ai_cursor_rules.md) / [plugins](./0_ai_cursor_plugins.md) / MCP config — treat community packs as starting points, not trusted by default.
- Official install/publish path for full Cursor Plugins remains Cursor’s marketplace and `.cursor-plugin` layout (see Plugins note).

**When to use:** Bootstrapping rules or MCP for a stack you do not want to invent from scratch. Prefer official Cursor marketplace / team kits for production-shared plugins; use the Directory to survey what the community ships.

**Pluses and minuses:**

| +                                               | −                                                                                                                           |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Fast discovery of stack-specific rules and MCP. | Community quality varies — audit before committing secrets or broad permissions.                                            |
| Complements official marketplace.               | Not a substitute for project [rules](./0_ai_cursor_rules.md) and [skills](./0_ai_cursor_skills.md) tailored to _your_ repo. |

**Thoughts:** Steal structure from popular rules; rewrite them to match this handbook’s ownership and safety constraints.

**Sources:** [cursor.directory](https://cursor.directory/) · [Cursor Plugins](https://cursor.com/docs/reference/plugins) · [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Sources

<details>
<summary>Sources</summary>

**Definition:** Official help, external guides, and catalogs that informed the tips on this page.

**Sources:**

- [Cursor Help Center](https://cursor.com/help)
- [Builder.io — How I use Cursor (+ my best tips)](https://www.builder.io/blog/cursor-tips)
- [cursor.directory — community rules, MCP, and plugins](https://cursor.directory/)

</details>

## Interview Practice

### Questions & Answers

**Q: What replaced “YOLO mode” in current Cursor?**  
A: **Run Modes** under Approvals & Execution — prefer **Auto-review**; **Run Everything** is the closest to old unattended YOLO.

**Q: Why ask the Agent to write tests before code?**  
A: Non-trivial tasks rarely one-shot correctly; a suite gives an objective loop (“run until green”) instead of manual QA by chat.

**Q: What is cursor.directory for?**  
A: Community discovery of rules, MCP, and plugins — audit before install; official marketplace is still the publish/install path for Cursor Plugins.

**Q: Subagents vs skills — when to use which?**  
A: Full notes: [Subagents](./0_ai_cursor_subagents.md#subagents-vs-skills).

**Q: What do Cursor hooks do?**  
A: Full notes: [Hooks](./0_ai_cursor_hooks.md#what-are-hooks).
