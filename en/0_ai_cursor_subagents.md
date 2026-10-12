# Cursor — Subagents

Specialized assistants the main Agent delegates to via the **Task** tool. Each runs in its own context window — useful for parallel work, noisy research, and independent verification.

### Related docs

- [AI workflow](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Hooks](./0_ai_cursor_hooks.md) · [Plugins](./0_ai_cursor_plugins.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area         | Topics                                                                                                      |
| ------------ | ----------------------------------------------------------------------------------------------------------- |
| **Basics**   | [What are subagents](#what-are-subagents), [Built-in subagents](#built-in-subagents)                        |
| **Custom**   | [File locations](#file-locations), [Frontmatter fields](#frontmatter-fields), [Model config](#model-config) |
| **Workflow** | [How to invoke](#how-to-invoke), [Parallel & background](#parallel-and-background), [Resume](#resume)       |
| **Compare**  | [Subagents vs skills](#subagents-vs-skills)                                                                 |
| **Examples** | [Example subagents](#example-subagents)                                                                     |

### Sections

- [What are subagents](#what-are-subagents)
- [Built-in subagents](#built-in-subagents)
- [File locations](#file-locations)
- [Frontmatter fields](#frontmatter-fields)
- [Model config](#model-config)
- [How to invoke](#how-to-invoke)
- [Parallel and background](#parallel-and-background)
- [Resume](#resume)
- [Subagents vs skills](#subagents-vs-skills)
- [Example subagents](#example-subagents)
- [Interview Practice](#interview-practice)

---

## What are subagents

<details>
<summary>What are subagents</summary>

**Definition:** A **subagent** is a specialized AI assistant the parent Agent spawns through the **Task** tool. It gets its own context window, works on a focused prompt, and returns a summary to the parent. Subagents work in the editor, CLI, and [Cloud Agents](https://cursor.com/docs/cloud-agent).

**How it works:**

- Parent Agent decides when to delegate (reads custom `description` fields like skills).
- Subagent runs tools (Read, Shell, Grep, MCP, …) inside its window — intermediate noise stays there.
- Parent sees the final result and continues orchestration.
- Custom subagents are markdown files with YAML frontmatter; built-ins need no setup.

**When to use:** Long research, parallel exploration, skeptical verification, or domain experts (security, tests, locale audit). Skip for one-line edits — Tab, Cmd+K, or a [skill](./0_ai_cursor_skills.md) is enough.

**Pluses and minuses:**

| +                                            | −                                                           |
| -------------------------------------------- | ----------------------------------------------------------- |
| Keeps main chat context clean.               | Extra latency and tokens vs inline work.                    |
| Parallel subagents speed multi-area tasks.   | Subagent does not see full parent chat — prompt explicitly. |
| `readonly` mode for safer audit-only passes. | Over-delegation if descriptions are vague.                  |

**Thoughts:** Think “hire a specialist for one job” — not a second full Agent chat you micromanage.

**Sources:** [Subagents](https://cursor.com/docs/subagents)

</details>

## Built-in subagents

<details>
<summary>Built-in subagents</summary>

**Definition:** Cursor ships subagents for the noisiest, most context-heavy operations. You do not configure them — the parent Agent picks them when appropriate.

**How it works:**

| Subagent                  | Role                                   | Why isolated                                       |
| ------------------------- | -------------------------------------- | -------------------------------------------------- |
| `explore`                 | Codebase search, file/symbol discovery | Many file snippets; uses a faster model by default |
| `shell` / `bash`          | Terminal commands, long command output | Raw logs would flood parent context                |
| `browser` / `browser_use` | Browser automation via MCP             | Page snapshots and interaction traces are large    |

Built-ins share traits: noisy intermediate output, specialized prompts/tools, and high token use. Running them as subagents gives **context isolation**, **model flexibility** (e.g. fast explore), and **lower cost** when the parent uses a heavier model.

CLI/ACP also lists types such as `generalPurpose`, `computer_use`, `video_review`, `vm_setup_helper` — the parent or integration picks the type for the Task call.

**When to use:** Automatically — when the parent needs broad search, shell-heavy work, or browser automation. You can still ask explicitly: “use explore to find where `parseTopicMarkdown` is called.”

**Thoughts:** Explore is often the first subagent you notice — ten parallel searches finish faster than one main-agent grep marathon.

**Sources:** [Subagents — built-in](https://cursor.com/docs/subagents) · [Task tool (CLI)](https://cursor.com/docs/cli/acp)

</details>

## File locations

<details>
<summary>File locations</summary>

**Definition:** Custom subagents live as `.md` files with YAML frontmatter. Project files override user files when names conflict.

**How it works:**

| Scope   | Path                | Notes                              |
| ------- | ------------------- | ---------------------------------- |
| Project | `.cursor/agents/`   | Preferred; committed with the repo |
| Project | `.claude/agents/`   | Claude compatibility               |
| Project | `.codex/agents/`    | Codex compatibility                |
| User    | `~/.cursor/agents/` | All projects for this user         |
| User    | `~/.claude/agents/` | Claude compatibility               |
| User    | `~/.codex/agents/`  | Codex compatibility                |

Precedence: `.cursor/` over `.claude/` / `.codex/`; project over user when names match.

Plugin packs can ship agents under `agents/` in a [Cursor Plugin](./0_ai_cursor_plugins.md). Scaffold with `/create-subagent`.

**When to use:** Project agents for repo-specific auditors (locale parity, parser checks). User agents for personal workflows across repos.

**Examples:**

```text
.cursor/agents/
├── locale-parity-auditor.md
├── verifier.md
└── security-auditor.md
```

**Sources:** [Subagents — file locations](https://cursor.com/docs/subagents)

</details>

## Frontmatter fields

<details>
<summary>Frontmatter fields</summary>

**Definition:** YAML frontmatter at the top of each subagent file configures identity, delegation hints, model, and permissions.

**How it works:**

| Field           | Type    | Default       | Purpose                                                                                   |
| --------------- | ------- | ------------- | ----------------------------------------------------------------------------------------- |
| `name`          | string  | From filename | Identifier; lowercase and hyphens                                                         |
| `description`   | string  | —             | Shown in Task hints; **parent reads this to decide delegation**                           |
| `model`         | string  | `inherit`     | `inherit` or a specific model ID — see [Model config](#model-config)                      |
| `readonly`      | boolean | `false`       | No file edits; no state-changing shell                                                    |
| `is_background` | boolean | `false`       | Run without blocking the parent — see [Parallel and background](#parallel-and-background) |

Body markdown = system instructions for the subagent (what to do when invoked, output format, constraints).

**When to use:** Always set a sharp `description` with “Use when …” — same discipline as skill `description`. Use `readonly: true` for auditors and reviewers.

**Examples:**

```markdown
---
name: verifier
description: Validates completed work. Use after tasks are marked done to confirm implementations work.
model: inherit
readonly: true
---

You are a skeptical validator…
```

**Thoughts:** `description` is the trigger. “Debugger” alone is weak; “Debugging specialist for errors and test failures. Use when encountering issues.” is delegatable.

**Sources:** [Subagents — configuration fields](https://cursor.com/docs/subagents)

</details>

## Model config

<details>
<summary>Model config</summary>

**Definition:** The `model` frontmatter field controls which model the subagent runs on — independent of the parent when you set a specific ID.

**How it works:**

| Value             | Behavior                                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------------------- |
| `inherit`         | Same model as parent Agent (default)                                                                          |
| Specific model ID | Fixed model, e.g. `composer-2`, `gpt-5.6-sol` — see [models docs](https://cursor.com/docs/models-and-pricing) |

- **`inherit`** — subagent needs the same reasoning as parent (code review on subtle changes).
- **Specific ID** — cheap/fast explore-style work, or a model with a capability the parent lacks.
- Built-in **explore** uses a faster model by default; configurable in CLI via `/config`.

**When to use:** Match parent for judgment calls; pick a cheaper/faster model for search-only or formatting subagents.

**Examples:**

```markdown
---
name: reasoning-agent
description: Handles complex architectural decisions.
model: gpt-5.6-sol
---
```

**Sources:** [Subagents — model configuration](https://cursor.com/docs/subagents)

</details>

## How to invoke

<details>
<summary>How to invoke</summary>

**Definition:** You usually **do not** call the Task tool yourself — you instruct the parent Agent, which spawns subagents when the task fits.

**How it works:**

1. **Natural language:** “Use a subagent to explore `lib/parse` and report how concepts are split.”
2. **Named custom agent:** “Run the `locale-parity-auditor` subagent on `resources/en` vs `resources/ru`.”
3. **Slash scaffold:** `/create-subagent` — generates `.cursor/agents/<name>.md`.
4. **Explicit parallel:** “Launch explore and shell subagents in parallel — one for auth files, one for test output.”
5. **CLI/ACP:** integrations send `cursor/task` with `subagentType` (`explore`, `shell`, or `{ custom: "your-type" }`).

Parent receives the subagent summary and continues. In the UI you can drill into the full subagent transcript (prompt, thinking, tool calls, response).

**When to use:** Any task that would dump hundreds of lines into the main chat. For a 3-line fix, stay in Cmd+K.

**Examples:**

```text
After you finish the parser change, run the verifier subagent before we commit.
```

**Sources:** [Subagents](https://cursor.com/docs/subagents) · [CLI changelog — subagents](https://cursor.com/docs/cli/changelog)

</details>

## Parallel and background

<details>
<summary>Parallel and background</summary>

**Definition:** The parent can run **multiple subagents at once** or mark a subagent **`is_background: true`** so the parent is not blocked.

**How it works:**

- **Parallel:** One message can launch several Task calls (e.g. explore `lib/nav` + explore `lib/parse` simultaneously). Each has its own context; parent merges summaries.
- **Background (`is_background: true`):** Subagent runs while parent continues other work. Completion can trigger follow-ups in interactive and headless sessions.
- **Foreground (default):** Parent waits for the subagent result before the next step.
- **Headless/CI:** Single-turn runs wait for delegated subagents to finish (including background shells) before exiting.

Use [hooks](./0_ai_cursor_hooks.md) `subagentStart` to allow/deny types when parallel workers are a policy concern.

**When to use:** Parallel for independent research tracks. Background for long-running shell/dev-server tasks you do not want blocking the main loop.

**Pluses and minuses:**

| +                                         | −                                                    |
| ----------------------------------------- | ---------------------------------------------------- |
| Cuts wall-clock time on multi-area tasks. | Harder to debug when many workers fail silently.     |
| Background keeps the parent responsive.   | You must merge conflicting subagent advice yourself. |

**Examples:**

```markdown
---
name: test-runner
description: Test automation expert. Use proactively to run tests and fix failures.
is_background: false
---
```

```text
In parallel: subagent A audits SEO routes, subagent B audits parser tests. Summarize both when done.
```

**Sources:** [Subagents](https://cursor.com/docs/subagents) · [Hooks — subagentStart](https://cursor.com/docs/hooks)

</details>

## Resume

<details>
<summary>Resume</summary>

**Definition:** Subagent runs return an **`agentId`**. Pass it on a later Task call to **resume** the same subagent with prior context preserved.

**How it works:**

- First invocation → subagent works → returns `agentId` in the Task result.
- Resume: parent (or you via prompt) passes `agentId` to continue the same thread.
- Background subagents persist state as they run; you can resume after completion for follow-up questions.
- Useful when an investigation spans multiple user messages or a long-running audit.

**When to use:** Multi-step debugging, iterative security review, or locale audit across sessions. Skip for one-shot explore — a fresh subagent is simpler.

**Examples:**

```text
Resume subagent agentId abc-123 and check whether the RU twin for 0_ai_cursor_hooks.md exists.
```

**Sources:** [Subagents — resuming](https://cursor.com/docs/subagents)

</details>

## Subagents vs skills

<details>
<summary>Subagents vs skills</summary>

**Definition:** **Subagents** delegate to a separate context window; **skills** are playbooks loaded into the parent Agent.

**How it works:**

|              | **Subagents**                                 | **Skills**                                        |
| ------------ | --------------------------------------------- | ------------------------------------------------- |
| **Context**  | Own window                                    | Parent window                                     |
| **Best for** | Long research, parallel streams, verification | Single-purpose repeatable procedures              |
| **Trigger**  | Parent delegates via Task + `description`     | `/skill`, auto by `description`, Custom Mode      |
| **Output**   | Summary back to parent                        | Parent executes steps inline                      |
| **Examples** | Explore repo, locale audit, security review   | Deploy checklist, update-conspect, format imports |

Official guidance: if you would create a subagent for a one-shot chore (“generate changelog”, “format imports”), use a [skill](./0_ai_cursor_skills.md) instead.

**When to use:**

- **Subagent** — context isolation, parallel work, independent skeptic.
- **Skill** — step-by-step workflow the parent should follow directly.

**Thoughts:** MyConspect pattern: `readonly` subagents report drift; `update-conspect` / `sync-locales` skills fix notes.

**Sources:** [Subagents — when to use](https://cursor.com/docs/subagents) · [Skills](./0_ai_cursor_skills.md)

</details>

## Example subagents

<details>
<summary>Example subagents</summary>

**Definition:** Reference patterns for project subagents — adapt frontmatter and body to your repo.

**How it works:**

**Locale auditor (readonly):**

```markdown
---
name: locale-parity-auditor
description: Audits EN/RU study note parity in resources/. Use when checking locale drift or before translation.
model: inherit
readonly: true
---

You audit EN/RU parity. Report mismatches in a table; do not rewrite files.
Follow .cursor/skills/sync-locales/SKILL.md for check rules.
```

**Security (readonly):**

```markdown
---
name: security-auditor
description: Security specialist. Use when implementing auth, payments, or handling sensitive data.
model: inherit
readonly: true
---

Check injection, XSS, auth bypass, hardcoded secrets, input validation.
Report Critical / High / Medium.
```

**Test runner (write-capable):**

```markdown
---
name: test-runner
description: Test automation expert. Use proactively to run tests and fix failures.
---

When code changes: run tests, analyze failures, fix while preserving intent, re-run.
```

**Debugger:**

```markdown
---
name: debugger
description: Debugging specialist for errors and test failures. Use when encountering issues.
---

Capture stack trace, reproduce, isolate, minimal fix, verify.
```

**When to use:** Copy structure; rewrite `description` and body for your stack. Commit under `.cursor/agents/` for team sharing.

**Sources:** [Subagents — examples](https://cursor.com/docs/subagents)

</details>

## Interview Practice

### Questions & Answers

**Q: What is a Cursor subagent?**  
A: A specialized assistant the parent Agent spawns via Task — own context window, focused work, summary returned to parent.

**Q: Name three built-in subagents.**  
A: `explore` (codebase search), `shell`/`bash` (terminal), `browser`/`browser_use` (browser automation). No setup required.

**Q: Where do custom subagents live?**  
A: `.cursor/agents/*.md` (project) or `~/.cursor/agents/` (user). YAML frontmatter + markdown instructions.

**Q: Subagents vs skills?**  
A: Subagents = separate context, parallel/multi-step delegation. Skills = repeatable playbook in the parent Agent. One-shot chores → skill.

**Q: What does `readonly: true` do?**  
A: Subagent cannot edit files or run state-changing shell — good for audits and security review.

**Q: How do you resume a subagent?**  
A: Pass the `agentId` from a previous Task result to continue with preserved context.
