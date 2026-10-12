# Cursor — Rules

Persistent system-level instructions for Agent. Shorter than [skills](./0_ai_cursor_skills.md); scoped always, by file globs, by relevance, or by `@mention`.

### Related docs

- [AI workflow](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area         | Topics                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------ |
| **Types**    | [Four rule sources](#four-rule-sources), [Apply modes](#apply-modes)                       |
| **Files**    | [Project rules (.mdc)](#project-rules-mdc), [AGENTS.md](#agentsmd), [CLAUDE.md](#claudemd) |
| **Team**     | [Team Rules](#team-rules), [Precedence](#precedence)                                       |
| **Optimize** | [Context optimization](#context-optimization)                                              |
| **Migrate**  | [Migrating from .cursorrules](#migrating-from-cursorrules)                                 |

### Sections

- [Four rule sources](#four-rule-sources)
- [Apply modes](#apply-modes)
- [Project rules (.mdc)](#project-rules-mdc)
- [AGENTS.md](#agentsmd)
- [CLAUDE.md](#claudemd)
- [Team Rules](#team-rules)
- [Precedence](#precedence)
- [Context optimization](#context-optimization)
- [Migrating from .cursorrules](#migrating-from-cursorrules)
- [Rule troubleshooting](#rule-troubleshooting)
- [Interview Practice](#interview-practice)

---

## Four rule sources

<details>
<summary>Four rule sources</summary>

**Definition:** A **rule** is a persistent set of instructions Agent follows while working in your project. You can scope rules globally, per project, per org, or to specific files. Cursor has **four** rule sources: **Project Rules**, **User Rules**, **Team Rules**, and plain **AGENTS.md** (plus `CLAUDE.md` — see [CLAUDE.md](#claudemd)). Rules are shorter than [skills](./0_ai_cursor_skills.md); skills are multi-step playbooks, rules are ongoing constraints.

**How it works:**

- When a rule applies, its contents are included at the **start** of the model context for that Agent turn.
- Rules apply to **Agent (Chat) only** — not Cursor Tab, Inline Edit (Cmd/Ctrl+K), or [Bugbot](https://cursor.com/docs/bugbot) PR reviews.
- On conflict, precedence is **Team → Project → User** (see [Precedence](#precedence)).

| Source              | Where                                                                           | Scope                      | Shape                                                                                |
| ------------------- | ------------------------------------------------------------------------------- | -------------------------- | ------------------------------------------------------------------------------------ |
| **Project Rules**   | `.cursor/rules/*.mdc` (git)                                                     | This repo                  | Frontmatter: `alwaysApply`, `description`, `globs` — see [Apply modes](#apply-modes) |
| **User Rules**      | **Customize → Rules** (syncs with your Cursor account across devices)           | All your projects          | Free-form preferences (tone, conventions)                                            |
| **User rule files** | `~/.cursor/rules` (Windows: `%USERPROFILE%\.cursor\rules`)                      | Machine only — **no sync** | Same as user rules, but local files                                                  |
| **Team Rules**      | [Team dashboard](https://cursor.com/dashboard/team-content) (Team / Enterprise) | Org-wide                   | Free-form text; optional globs; **Enforced** or optional                             |
| **AGENTS.md**       | Project root and/or nested dirs                                                 | That directory tree        | Plain markdown, no frontmatter                                                       |

**Storage notes:** User rules in Settings sync when you sign in on another machine. User rule files and team rules are **not** included in profile exports.

**When to use:**

| Choose…                    | When…                                                                                                            |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Project Rules** (`.mdc`) | Repo conventions the team shares; need globs, always-on, intelligent, or `@mention` apply modes.                 |
| **User Rules**             | Personal tone or habits across every repo — not team policy.                                                     |
| **Team Rules**             | Org standards / compliance; use **Enforce** when members must not opt out.                                       |
| **AGENTS.md**              | Simple readable instructions without frontmatter; nested package notes. Prefer `.mdc` when you need apply modes. |

**Common pitfalls:** Plain `.md` in `.cursor/rules/` (ignored — must be `.mdc`); expecting User Rules to affect Tab / Cmd+K; multi-step deploy checklists in rules instead of a [skill](./0_ai_cursor_skills.md); duplicating linter style guides into always-on rules.

**Pluses and minuses:**

| +                                                        | −                                                             |
| -------------------------------------------------------- | ------------------------------------------------------------- |
| Persistent guidance without re-pasting prompts.          | Only Agent Chat — Tab / Inline Edit ignore rules.             |
| Four layers: personal, project, org, simple markdown.    | Too many always-on rules waste context.                       |
| Project `.mdc` + Team Rules are shareable / enforceable. | Wrong source (User for team policy) → inconsistent teammates. |

**Examples:**

User Rule (Customize → Rules):

```md
Reply in a concise style. Avoid unnecessary repetition.
```

Project Rule (`.cursor/rules/react-patterns.mdc`) — always apply:

```md
---
alwaysApply: true
---

- Use TypeScript for all new files
- Prefer functional components in React
- Use snake_case for database columns
```

Create a project rule: **Cmd/Ctrl+Shift+P** → “New Cursor Rule”, or `/create-rule` in chat, or Customize → Rules → Add Rule.

**Thoughts:** Pick the **narrowest** source that fits — personal tone in User, repo conventions in Project, org policy in Team, “just markdown” in AGENTS.md.

**Sources:** [Rules help](https://cursor.com/help/customization/rules) · [Rules reference](https://cursor.com/docs/rules)

</details>

## Apply modes

<details>
<summary>Apply modes</summary>

**Definition:** Each project rule uses frontmatter (`alwaysApply`, `description`, `globs`) — set via the **type dropdown** when you create or edit a rule — to decide when it is injected into Agent context.

**How it works:**

| UI type                     | Frontmatter behavior                                                           |
| --------------------------- | ------------------------------------------------------------------------------ |
| **Always Apply**            | `alwaysApply: true` — every chat; globs/description ignored                    |
| **Apply to Specific Files** | `alwaysApply: false` + `globs` — when a matching file is in context            |
| **Apply Intelligently**     | `alwaysApply: false` + `description`, no globs — Agent decides by relevance    |
| **Apply Manually**          | `alwaysApply: false`, no description, no globs — only via `@rule-name` in chat |

**When to use:** Always Apply for hard safety (“never commit secrets”). Globs for React/API folders. Intelligent for occasional domain tips (write a clear `description`). Manual for rare checklists you `@mention` on demand.

**Examples:**

```md
---
globs: src/components/**/*.tsx
alwaysApply: false
---

- Use named exports, not default exports
- Keep components under 200 lines
```

**Sources:** [Rules help — create a project rule](https://cursor.com/help/customization/rules) · [Rule anatomy](https://cursor.com/docs/rules#rule-anatomy)

</details>

## Project rules (.mdc)

<details>
<summary>Project rules (.mdc)</summary>

**Definition:** Project rules are `.mdc` files under `.cursor/rules` with YAML frontmatter plus a markdown body. Plain `.md` in that folder is **ignored** (no frontmatter).

**How it works:**

- **Create:** Cmd/Ctrl+Shift+P → “New Cursor Rule”, `/create-rule` in chat, or Customize → Rules → Add Rule.
- **Organize:** keep rules in `.cursor/rules/`; nested subfolders work, but a **flat** directory with descriptive names (`react-patterns.mdc`, `api-validation.mdc`) is simpler.
- **Same filename, different folders:** Cursor identifies rules by **full path** — two `components.mdc` files in different folders both apply when their conditions match; filenames alone do not override each other.
- **Best practices:** under ~500 lines; split large topics; reference files with `@filename.ts` instead of pasting whole guides; start small and add rules when Agent repeats the same mistake; check into git for the team.
- **Avoid:** copying entire style guides (use a linter), rare edge cases, duplicating code already in the repo.

**When to use:** Encode domain knowledge, style decisions, and conventions the whole team should share.

**Pluses and minuses:**

| +                                              | −                                              |
| ---------------------------------------------- | ---------------------------------------------- |
| Version-controlled; consistent Agent behavior. | Over-broad always-on rules waste context.      |
| Globs keep guidance local to a folder.         | Wrong extension (`.md`) silently does nothing. |

**Sources:** [Rules help — organize rule files](https://cursor.com/help/customization/rules) · [Project rules](https://cursor.com/docs/rules#project-rules)

</details>

## AGENTS.md

<details>
<summary>AGENTS.md</summary>

**Definition:** `AGENTS.md` is a plain markdown file of agent instructions in the project root (or nested directories) — a simple alternative to structured `.cursor/rules`.

**How it works:**

- Cursor picks it up automatically; no frontmatter or apply-mode dropdown.
- Nested `AGENTS.md` files apply when working under that directory; more specific nested instructions take precedence over parents.
- For conditional application (globs, intelligent apply, manual `@mention`), use project rules in `.cursor/rules/` instead.

**When to use:** Small projects or package-level notes where readable markdown is enough.

**Examples:**

```markdown
# Project instructions

- Use TypeScript for all new files
- Follow the repository pattern for data access
- Keep components under 200 lines
```

**Sources:** [Rules help — AGENTS.md](https://cursor.com/help/customization/rules) · [AGENTS.md](https://cursor.com/docs/rules#agentsmd)

</details>

## CLAUDE.md

<details>
<summary>CLAUDE.md</summary>

**Definition:** `CLAUDE.md` is read by Cursor the same way as `AGENTS.md` — plain markdown agent instructions, typically in the project root.

**How it works:**

- Place `CLAUDE.md` in the project root (nested support mirrors `AGENTS.md`).
- **Always applied to every conversation**, regardless of any `alwaysApply` frontmatter — for compatibility with projects that also use Claude Code.
- Need conditional rules? Use `.cursor/rules/*.mdc` with apply modes instead.

**When to use:** Repos shared between Cursor and Claude Code, or when you already maintain a `CLAUDE.md` playbook.

**Thoughts:** Treat `CLAUDE.md` as “always-on AGENTS.md”; keep it short or split detailed guidance into scoped `.mdc` rules.

**Sources:** [Rules help — CLAUDE.md](https://cursor.com/help/customization/rules)

</details>

## Team Rules

<details>
<summary>Team Rules</summary>

**Definition:** Team and [Enterprise](https://cursor.com/docs/enterprise) plans create and manage org-wide rules from the [Cursor dashboard](https://cursor.com/dashboard/team-content).

**How it works:**

- Free-form text (not the `.cursor/rules` folder layout); optional **globs** (e.g. `**/*.py`) scope a rule to matching files.
- **Enforced rules** — required for all members; cannot be turned off in Customize.
- **Optional rules** — enabled by default; members can disable them under **Customize → Rules → Team Rules**.
- Import rules from a GitHub repo only via a **plugin** marketplace (rules alone are not imported) — see [Plugins](./0_ai_cursor_plugins.md).

**When to use:** Company coding standards and compliance baselines. Do not treat AI rules as the only security control.

**Sources:** [Rules help — team rules](https://cursor.com/help/customization/rules) · [Team Rules](https://cursor.com/docs/rules#team-rules)

</details>

## Precedence

<details>
<summary>Precedence</summary>

**Definition:** When guidance conflicts, Cursor merges applicable rules in this order: **Team Rules → Project Rules → User Rules** (earlier wins).

**How it works:**

- All matching rules are included; Team overrides Project overrides User on conflict.
- Rules never apply to Tab, Inline Edit, or Bugbot PR reviews.

**Thoughts:** Put irreversible safety in enforced Team or always-apply Project rules; keep personal tone in User Rules.

**Sources:** [Rules help — team rules](https://cursor.com/help/customization/rules) · [Team Rules — precedence](https://cursor.com/docs/rules#format-and-how-team-rules-are-applied)

</details>

## Context optimization

<details>
<summary>Context optimization</summary>

**Definition:** Rules and [skills](./0_ai_cursor_skills.md) compete for the same Agent context window. Optimize by **splitting concerns**: rules = short constraints that apply when files match; skills = multi-step workflows loaded on demand.

**How it works:**

| Tactic        | Rule                                                                               | Skill                                                 |
| ------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------- |
| **Size**      | Aim for &lt;50 lines; split above ~500                                             | Keep `SKILL.md` focused; depth in `references/`       |
| **Scope**     | `globs` or narrow `description`                                                    | `paths` glob + sharp `description` (WHAT + WHEN)      |
| **Overlap**   | One concern per rule — don’t repeat the skill checklist                            | Point at rules for shape; skill owns the workflow     |
| **Always-on** | Reserve for safety/compliance only                                                 | Use Custom Mode or `/skill-name` for long playbooks   |
| **Migrate**   | “Apply Intelligently” + steps → [skill](./0_ai_cursor_skills.md#migrate-to-skills) | `disable-model-invocation: true` for slash-only tools |

**When to use:** After Agent feels slow or ignores guidance — audit always-on rules first, then duplicate rule+skill pairs. Convert repeated intelligent rules with `/migrate-to-skills`.

**Examples (MyConspect pattern):**

| Layer | File                                             | Loads when                                    |
| ----- | ------------------------------------------------ | --------------------------------------------- |
| Rule  | `study-notes-structure.mdc`                      | Intelligent — conspect edit/structure prompts |
| Rule  | `locale-sync.mdc`, `resources-official-docs.mdc` | Intelligent — RU sync / fact edits            |
| Skill | `update-conspect`                                | Intelligent — conspect edit prompts           |
| Skill | `sync-locales`                                   | Intelligent — translate / EN↔RU sync          |

**Thoughts:** If you paste the same 10-step checklist every week, it belongs in a skill — not three always-on rules.

**Sources:** [Rules — best practices](https://cursor.com/docs/rules) · [Skills — progressive disclosure](https://cursor.com/docs/skills.md#optional-directories) · [Skills vs rules](https://cursor.com/help/customization/skills#when-should-i-use-skills-instead-of-rules)

</details>

## Migrating from .cursorrules

<details>
<summary>Migrating from .cursorrules</summary>

**Definition:** A `.cursorrules` file in the project root is **legacy** and will be deprecated. Migrate to a project rule in `.cursor/rules/`.

**How it works:**

1. Cmd/Ctrl+Shift+P → “New Cursor Rule” (or `/create-rule` in chat).
2. Copy content from `.cursorrules` into the new `.mdc` file.
3. Set type to **Always Apply** (matches old always-on behavior).
4. Delete `.cursorrules` from the project root.

**When to use:** As soon as you see `.cursorrules` in a repo you maintain — before Cursor removes support.

**Sources:** [Rules help — migrate from .cursorrules](https://cursor.com/help/customization/rules)

</details>

## Rule troubleshooting

<details>
<summary>Rule troubleshooting</summary>

**Definition:** Quick checks when a rule does not seem to affect Agent behavior.

**How it works:**

- Confirm you are in **Agent (Chat)** — rules do not apply to Tab, Inline Edit, or Bugbot.
- **Apply Intelligently:** add a clear `description` so Agent knows when the rule is relevant.
- **Apply to Specific Files:** verify `globs` match the files in context (e.g. `src/components/**/*.tsx`).
- **Apply Manually:** `@mention` the rule name in chat.
- Two rules with the **same filename** in different folders both apply by full path — not a name collision.

**When to use:** After creating or editing a rule and Agent still ignores it.

**Sources:** [Rules help — rule doesn't apply](https://cursor.com/help/customization/rules)

</details>

## Interview Practice

Q&A moved to [Cursor — Questions](./0_ai_cursor_questions.md).
