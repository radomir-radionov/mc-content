# Cursor — Questions

Q&A for Cursor skills, rules, plugins, and slash commands.

### Related docs

- [Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [AI workflow](./0_ai.md)

### Sections

- [Questions & Answers](#questions-answers)

---

## Questions & Answers

<details>
<summary>Skills vs rules — what’s the difference, and when to use each?</summary>

**Definition:** **Rules** are short persistent guidelines injected into Agent context (always / globs / intelligent / `@mention`). **Skills** are multi-step playbooks (`SKILL.md` + optional scripts/references) invoked with `/`, auto-picked by `description`, or held as a Custom Mode.

**How it works:**

|                 | **Rules**                                 | **Skills**                                             |
| --------------- | ----------------------------------------- | ------------------------------------------------------ |
| **Purpose**     | Coding constraints and conventions        | Repeatable procedures                                  |
| **Shape**       | `.mdc` / User / Team / `AGENTS.md`        | Folder + `SKILL.md` (+ scripts/references)             |
| **How applied** | Always / globs / intelligent / `@mention` | Auto by `description`, `/skill-name`, `@`, Custom Mode |
| **Example**     | “Use TypeScript for all new files”        | “Deploy to staging: test → build → deploy → verify”    |

**When to use:**

- **Rule** — style, architecture, safety (“never commit secrets”), anything that should ride along always or for matching files.
- **Skill** — checklists Agent must follow step-by-step (deploy, review, update conspect), especially with scripts.

**Thoughts:** Rules = “always / when matching files.” Skills = “how to do this job.” Full notes: [What are skills](./0_ai_cursor_skills.md#what-are-skills) · [Four rule sources](./0_ai_cursor_rules.md#four-rule-sources).

**Sources:** [Skills help — vs rules](https://cursor.com/help/customization/skills#when-should-i-use-skills-instead-of-rules) · [Rules](https://cursor.com/docs/rules) · [Skills](https://cursor.com/docs/skills.md)

</details>

<details>
<summary>What are the four rule sources?</summary>

**Definition:** Cursor loads rules from **Project Rules** (`.cursor/rules/*.mdc`), **User Rules** (Customize → Rules), **Team Rules** (dashboard, Team/Enterprise), and plain **AGENTS.md**.

**How it works:** On conflict, earlier wins: **Team → Project → User**. All applicable rules merge into Agent context. Rules apply to Agent (Chat) only — not Tab or Cmd/Ctrl+K.

**Thoughts:** Full notes: [Four rule sources](./0_ai_cursor_rules.md#four-rule-sources).

**Sources:** [Rules](https://cursor.com/docs/rules)

</details>

<details>
<summary>Why is my `.cursor/rules/foo.md` ignored?</summary>

**Definition:** Project rules must be **`.mdc`** files with YAML frontmatter (`description` / `globs` / `alwaysApply`). A plain `.md` in `.cursor/rules/` is ignored by the rules system.

**When to use:** Prefer `AGENTS.md` for plain markdown instructions, or rename to `.mdc` and set apply mode.

**Sources:** [Project rules](https://cursor.com/docs/rules#project-rules)

</details>

<details>
<summary>Do User Rules affect Tab completion?</summary>

**Definition:** No. Rules (including User Rules) apply to **Agent (Chat) only** — not Cursor Tab, Inline Edit (Cmd/Ctrl+K), or Bugbot.

**Sources:** [Rules FAQ](https://cursor.com/docs/rules#do-rules-impact-cursor-tab-or-other-ai-features)

</details>

<details>
<summary>Where do project skills live?</summary>

**Definition:** Project skills live in `.cursor/skills/<name>/SKILL.md` or `.agents/skills/<name>/SKILL.md` (also nested package paths like `apps/web/.cursor/skills/`).

**How it works:** Personal skills go in `~/.cursor/skills/` (optional Cloud Agents sync). The skill name is the folder that contains `SKILL.md`.

**Sources:** [Skill directories](https://cursor.com/docs/skills.md#skill-directories)

</details>

<details>
<summary>Agent Plugin vs Cursor Plugin?</summary>

**Definition:** **Agent Plugin** = root `plugin.json`, portable skills + MCP only. **Cursor Plugin** = `.cursor-plugin/plugin.json` plus rules, agents, commands, hooks, and variables.

**Thoughts:** Full notes: [Plugins](./0_ai_cursor_plugins.md).

**Sources:** [Plugins](https://cursor.com/docs/plugins)

</details>

<details>
<summary>Can I import only rules from a GitHub repo?</summary>

**Definition:** No — package them in a **plugin** with a marketplace manifest (`.cursor-plugin/marketplace.json`), then install the plugin. Rules/skills arrive with the plugin.

**Sources:** [Importing rules from a repository](https://cursor.com/docs/rules#importing-rules-from-a-repository)

</details>

<details>
<summary>Where do custom `/foo` commands live?</summary>

**Definition:** Project commands: `.cursor/commands/foo.md`. User commands: `~/.cursor/commands/foo.md`. Prefer **skills** for new multi-step work; migrate with `/migrate-to-skills`.

**Sources:** [Slash commands](./0_ai_cursor_slash.md) · [Migrate to skills](https://cursor.com/docs/skills.md#migrating-rules-and-commands-to-skills)

</details>

<details>
<summary>Name five high-value built-in slash entries</summary>

**Definition:** Common built-ins: `/create-rule`, `/create-skill`, `/review`, `/loop`, `/migrate-to-skills` (plus `/summarize` for long chats).

**Thoughts:** Full list: [Built-in skills](./0_ai_cursor_skills.md#built-in-skills) · [Slash commands](./0_ai_cursor_slash.md).

**Sources:** [Built-in Cursor skills](https://cursor.com/docs/skills.md#built-in-cursor-skills)

</details>
