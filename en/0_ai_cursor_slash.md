# Cursor — Slash commands

Type `/` in Agent chat to run built-in skills, custom commands, and migrated skills. Prefer skills for new workflows; keep this page as the everyday cheat sheet.

### Related docs

- [AI workflow](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area            | Topics                                                                        |
| --------------- | ----------------------------------------------------------------------------- |
| **Mechanics**   | [How `/` works](#how--works), [Custom commands](#custom-commands)             |
| **Cheat sheet** | [Most-used built-ins](#most-used-built-ins), [Migration](#commands-to-skills) |

### Sections

- [How `/` works](#how--works)
- [Custom commands](#custom-commands)
- [Most-used built-ins](#most-used-built-ins)
- [Commands to skills](#commands-to-skills)
- [Interview Practice](#interview-practice)

---

## How `/` works

<details>
<summary>How `/` works</summary>

**Definition:** In Agent chat, typing `/` opens a menu of **skills** and **commands**. Selecting one attaches that playbook to the next message (or starts a Custom Mode if you Option/Alt+Enter).

**How it works:**

- Enter on a skill → attaches to **one** message, then fades.
- Option+Enter (Mac) / Alt+Enter (Windows) → keep it as a **Custom Mode** for the whole session.
- You can also `@`-mention a skill to attach it as context.
- Long chats can use `/summarize` to compress context on demand (in addition to automatic summarization near the context limit).

**When to use:** Anytime you want a named, repeatable prompt instead of retyping. Skip `/` when a one-line natural-language ask is enough.

**Sources:** [Prompting agents](https://cursor.com/docs/agent/prompting.md) · [Agent Skills](https://cursor.com/docs/skills.md)

</details>

## Custom commands

<details>
<summary>Custom commands</summary>

**Definition:** Custom slash commands are reusable prompt markdown files under `.cursor/commands/` (project) or `~/.cursor/commands/` (user).

**How it works:**

- File name becomes the command: `.cursor/commands/deploy.md` → `/deploy`.
- Discovered in Agent IDE and Cursor CLI (`agent`) when the working directory is the project.
- Plugins can also ship a `commands/` folder.
- New workflows should prefer **skills** (`SKILL.md`); commands remain supported and migrate cleanly.

**Examples:**

```markdown
## <!-- .cursor/commands/pr-ready.md -->

name: pr-ready
description: Prep this branch for review

---

1. Run the test suite and fix failures
2. Summarize the diff vs main
3. Draft a PR title and test plan
```

**When to use:** Team one-shot prompts you always invoke explicitly. Convert to a skill with `disable-model-invocation: true` if you want the same behavior under Skills.

**Sources:** [Changelog — slash commands](https://cursor.com/changelog/1-6) · [Plugins — commands](https://cursor.com/docs/reference/plugins#commands-format)

</details>

## Most-used built-ins

<details>
<summary>Most-used built-ins</summary>

**Definition:** Cursor ships built-in skills that appear in the `/` menu. These are the ones you will hit most often day to day.

**How it works:**

| Command              | What it does                                                |
| -------------------- | ----------------------------------------------------------- |
| `/create-rule`       | Scaffold a project rule (`.mdc`) with the right frontmatter |
| `/create-skill`      | Scaffold an Agent Skill (`SKILL.md` + folder)               |
| `/create-hook`       | Create hooks and update `hooks.json`                        |
| `/create-subagent`   | Define a focused custom subagent                            |
| `/review`            | Pick and run the appropriate code-review agent              |
| `/review-bugbot`     | Bug/regression-oriented review (Bugbot)                     |
| `/review-security`   | Security-focused review                                     |
| `/canvas`            | Interactive React artifact beside the chat                  |
| `/loop`              | Re-run a prompt or skill on an interval                     |
| `/split-to-prs`      | Split a large change into smaller PRs                       |
| `/migrate-to-skills` | Convert eligible rules/commands into skills                 |
| `/shell`             | Run the following text as a literal shell command           |
| `/automate`          | Create Automations (schedule, Slack, GitHub events, …)      |
| `/autopilot`         | Watch a PR and handle feedback / CI / follow-ups            |
| `/summarize`         | Compress the current chat context on demand                 |

**When to use:** Reach for `/create-*` when authoring Cursor config; `/review*` before merge; `/loop` for polling; `/summarize` when the thread is huge but you want to keep it.

**Thoughts:** Built-ins may also auto-run when your request clearly matches — you do not always need to type `/`.

**Sources:** [Built-in Cursor skills](https://cursor.com/docs/skills.md#built-in-cursor-skills) · [Changelog — `/summarize`](https://cursor.com/changelog/1-6)

</details>

## Commands to skills

<details>
<summary>Commands to skills</summary>

**Definition:** `/migrate-to-skills` turns eligible dynamic rules and `.cursor/commands` into skills under `.cursor/skills/`.

**How it works:**

- Slash commands become skills with `disable-model-invocation: true` (still explicit `/` only).
- Intelligent rules (no globs) become normal auto skills.
- Always-apply and glob rules stay as rules.

**When to use:** After Cursor 2.4+, when you want one Skills UI instead of mixed commands + rules. Review generated files before deleting old commands.

**Sources:** [Migrating rules and commands to skills](https://cursor.com/docs/skills.md#migrating-rules-and-commands-to-skills)

</details>

## Interview Practice

Q&A moved to [Cursor — Questions](./0_ai_cursor_questions.md).
