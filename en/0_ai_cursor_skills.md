# Cursor — Skills

Reusable multi-step workflows that teach Agent how to handle a specific task. More detailed than rules; invoked with `/` or applied automatically when relevant.

### Related docs

- [AI workflow](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area             | Topics                                                                                                                                 |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Basics**       | [What are skills](#what-are-skills), [Directories](#skill-directories), [SKILL.md](#skillmd-format), [How to use](#how-to-use-a-skill) |
| **Control**      | [Automatic vs slash](#automatic-vs-slash-invocation), [paths scoping](#scoping-with-paths), [Custom Mode](#custom-mode)                |
| **Distribution** | [Cloud sync](#cloud-agents-sync), [Publish to team](#publish-to-team), [View & install](#viewing-and-installing-skills)                |
| **Ops**          | [Built-in skills](#built-in-skills), [Convert a rule](#convert-rule-to-skill), [Migrate rules/commands](#migrate-to-skills)            |

### Sections

- [What are skills](#what-are-skills)
- [Skill directories](#skill-directories)
- [SKILL.md format](#skillmd-format)
- [How to use a skill](#how-to-use-a-skill)
- [Automatic vs slash invocation](#automatic-vs-slash-invocation)
- [Scoping with paths](#scoping-with-paths)
- [Custom Mode](#custom-mode)
- [Cloud Agents sync](#cloud-agents-sync)
- [Publish to team](#publish-to-team)
- [Viewing and installing skills](#viewing-and-installing-skills)
- [Built-in skills](#built-in-skills)
- [Convert rule to skill](#convert-rule-to-skill)
- [Migrate to skills](#migrate-to-skills)
- [Interview Practice](#interview-practice)

---

## What are skills

<details>
<summary>What are skills</summary>

**Definition:** A **skill** is a portable, version-controlled package that teaches Agent how to perform a domain-specific task. It is a folder with a required `SKILL.md` (YAML frontmatter + markdown instructions) and optional `scripts/`, `references/`, and `assets/` that Agent can act on with its tools. Skills are more detailed than [rules](./0_ai_cursor_rules.md) and are built for multi-step, repeatable workflows.

**How it works:**

| Pillar                 | Meaning                                                                             |
| ---------------------- | ----------------------------------------------------------------------------------- |
| **Portable**           | Works on any agent that implements [agentskills.io](https://agentskills.io)         |
| **Version-controlled** | Files in git — project skills in `.cursor/skills/`, personal in `~/.cursor/skills/` |
| **Actionable**         | Optional `scripts/` Agent can run; templates in `assets/`                           |
| **Progressive**        | Deep docs in `references/` — loaded on demand, not every turn                       |

- **Discovery:** On startup, Cursor scans skill directories (project + user; also Claude/Codex paths) and lists every folder with `SKILL.md`. See [Skill directories](#skill-directories).
- **Relevance:** Agent sees available skills and picks one when the request matches the skill’s `description` (WHAT it does + WHEN to use it). A vague description → the skill rarely auto-applies.
- **Manual invoke:** Type `/` in Agent chat and choose the skill (or `@` attach). That attaches the skill to **one** message. For a whole session, promote it to a [Custom Mode](#custom-mode) (Option+Enter / Alt+Enter).
- **Progressive context:** Keep `SKILL.md` focused; put deep docs in `references/` and templates in `assets/`.
- **Actionable:** Reference `scripts/` with relative paths — Bash, Python, JS, etc.
- **Built-ins:** Cursor ships [built-in skills](#built-in-skills) (`/create-skill`, `/review`, …) next to yours.

**When to use:**

|             | **Rules**                                    | **Skills**                                                              |
| ----------- | -------------------------------------------- | ----------------------------------------------------------------------- |
| Purpose     | Short coding guidelines and constraints      | Multi-step workflows and procedures                                     |
| Length      | A few lines to a few hundred lines           | Often longer, with step-by-step instructions                            |
| How applied | Included in every (or matching) conversation | Invoked with `/skill-name`, `@skill-name`, or auto-picked when relevant |
| Example     | “Use TypeScript for all new files”           | “Deploy to staging: run tests, build, deploy, verify”                   |

Use a **rule** when a short instruction is enough. Use a **skill** when Agent needs a detailed, repeatable process (deploy, audit, update conspect, land a PR).

**Common pitfalls:** Writing “helps with docs” as the description (too vague); dumping a novel into `SKILL.md` instead of progressive `references/`; using a skill for style that belongs in a [rule](./0_ai_cursor_rules.md); duplicating the same checklist in a rule **and** a skill — see [Context optimization](./0_ai_cursor_rules.md#context-optimization).

**Pluses and minuses:**

| +                                                    | −                                                         |
| ---------------------------------------------------- | --------------------------------------------------------- |
| Shareable, git-tracked playbooks for the team.       | Bad `description` → never auto-applied.                   |
| Scripts + references + assets, loaded progressively. | Long always-on skills burn context.                       |
| Works with `/`, auto-pick, Custom Mode, and plugins. | Overlap with rules if you mix “always style” into skills. |

**Examples:**

Minimal skill (folder `.cursor/skills/deploy-staging/SKILL.md`):

```markdown
---
name: deploy-staging
description: >
  Deploy the app to staging. Use when the user asks to deploy, ship to staging,
  or verify a staging release.
---

# Deploy to staging

1. Run the test suite
2. Build the production bundle
3. Deploy to the staging environment
4. Verify the deployment health check
```

Skill with scripts (Agent follows relative paths from the skill root):

```markdown
---
name: deploy-app
description: >
  Deploy the application to staging or production. Use when deploying code
  or when the user mentions deployment, releases, or environments.
---

# Deploy App

Run `scripts/deploy.sh <environment>` where `<environment>` is `staging` or `production`.
Before deploying, run `python scripts/validate.py`.
```

Create one interactively: type `/create-skill` and describe the workflow — Cursor’s built-in skill scaffolds name, structure, and save location.

**Thoughts:** Rules = “always / when matching files.” Skills = “how to do this job.” If you would paste the same checklist into chat every week, make it a skill.

**Sources:** [Agent Skills](https://cursor.com/docs/skills.md) · [What are skills?](https://cursor.com/docs/skills.md#what-are-skills) · [Skills help](https://cursor.com/help/customization/skills) · [agentskills.io](https://agentskills.io)

</details>

## Skill directories

<details>
<summary>Skill directories</summary>

**Definition:** Cursor loads skills from project and user folders; each skill is a directory that contains `SKILL.md`.

**How it works:**

| Location            | Scope                                  |
| ------------------- | -------------------------------------- |
| `.agents/skills/`   | Project                                |
| `.cursor/skills/`   | Project                                |
| `~/.agents/skills/` | User (local machine)                   |
| `~/.cursor/skills/` | User (local; can sync to Cloud Agents) |

- Also discovers Claude/Codex dirs: `.claude/skills/`, `.codex/skills/`, and the `~/` variants.
- Cursor walks each skills root **recursively** — category subfolders are fine (e.g. `.cursor/skills/shipping/deploy-staging/SKILL.md`). The skill **name** is the folder that contains `SKILL.md`, not the parent category.
- Monorepos: any nested `.cursor/skills/` or `.agents/skills/` under the repo is discovered (e.g. `apps/web/.cursor/skills/`). Skills there are **auto-scoped** to files in that directory — like setting `paths`, without extra frontmatter.
- `~/.agents/skills/` and unsynced `~/.cursor/skills/` stay on your machine; they are **not** copied to Cloud Agents, Agents Window remote SSH, or self-hosted workers (use repo skills or bake skills into the worker image).

**When to use:** Put team workflows in `.cursor/skills/` (commit them). Put personal habits in `~/.cursor/skills/` (optionally sync for Cloud Agents — see [Cloud Agents sync](#cloud-agents-sync)).

**Sources:** [Skill directories](https://cursor.com/docs/skills.md#skill-directories)

</details>

## SKILL.md format

<details>
<summary>SKILL.md format</summary>

**Definition:** Every skill is a `SKILL.md` file with YAML frontmatter (`name`, `description`) and markdown instructions.

**How it works:**

| Field                      | Required | Role                                                                   |
| -------------------------- | -------- | ---------------------------------------------------------------------- |
| `name`                     | Yes      | Kebab-case id; **must match** parent folder name                       |
| `description`              | Yes      | WHAT + WHEN — drives auto-relevance                                    |
| `paths`                    | No       | Glob scope (list or comma-separated string)                            |
| `disable-model-invocation` | No       | `true` → slash-only, no auto-pick                                      |
| `icon` / `color`           | No       | Custom Mode badge (`code`, `beaker`, `shield`, … / `green`, `blue`, …) |
| `metadata`                 | No       | Arbitrary key-value map                                                |

Optional folders next to `SKILL.md`:

| Dir           | Purpose                        |
| ------------- | ------------------------------ |
| `scripts/`    | Executable code Agent runs     |
| `references/` | Extra docs loaded on demand    |
| `assets/`     | Templates, images, static data |

Body: markdown instructions — often `# Title`, `## When to Use`, `## Instructions`. Keep the main file short; move depth to `references/`.

**Examples:**

```markdown
---
name: update-conspect
description: Add or correct study notes in resources/. Use when the user asks to edit conspect.
---

# Update conspect

1. Open files.md and pick the canonical file
2. Fetch official docs before writing facts
3. Edit one full details block; patch siblings with pointers
```

**Sources:** [SKILL.md file format](https://cursor.com/docs/skills.md#skillmd-file-format)

</details>

## How to use a skill

<details>
<summary>How to use a skill</summary>

**Definition:** Run a skill by typing `/` plus the skill name in Agent chat, or attach it with `@` so Agent reads the `SKILL.md` and follows its instructions.

**How it works:**

- **`/` invoke:** Type `/write-tests` (or pick from the menu). Attaches the skill to **one** message unless you promote it to a [Custom Mode](#custom-mode).
- **`@` attach:** Type `@` and select the skill to add it as context for the current turn.
- **Auto-pick:** When model invocation is enabled (default), Agent may apply a skill when your request matches its `description`.

**When to use:** `/` or `@` for explicit runs; auto-pick for domain playbooks you want Agent to notice without typing the skill name every time.

**Examples:**

```text
/write-tests
@deploy-staging
```

**Sources:** [How do I use a skill?](https://cursor.com/help/customization/skills#how-do-i-use-a-skill)

</details>

## Automatic vs slash invocation

<details>
<summary>Automatic vs slash invocation</summary>

**Definition:** By default Agent may apply a skill when the request matches its description; with `disable-model-invocation: true` it only runs when you type `/skill-name`.

**How it works:**

- Default: model-invocation on — skill behaves like a smart playbook.
- `disable-model-invocation: true` — classic slash-command behavior (explicit only).
- Typing `/` attaches the skill to **one** message unless you promote it to a Custom Mode.

**When to use:** Auto for domain playbooks you want Agent to pick up. Slash-only for expensive or dangerous workflows you must opt into.

**Sources:** [Disabling automatic invocation](https://cursor.com/docs/skills.md#disabling-automatic-invocation)

</details>

## Scoping with paths

<details>
<summary>Scoping with paths</summary>

**Definition:** The `paths` frontmatter field limits when a skill is surfaced to files matching glob patterns.

**How it works:**

- Accepts a list or a comma-separated string of globs.
- Unset `paths` → available regardless of open files.
- Legacy `globs` still works; new skills should use `paths`.

**Examples:**

```markdown
---
name: react-component-patterns
description: Conventions for writing React components in this codebase.
paths:
  - "**/*.tsx"
  - "packages/ui/**/*.ts"
---
```

**Sources:** [Scoping a skill to specific files](https://cursor.com/docs/skills.md#scoping-a-skill-to-specific-files)

</details>

## Custom Mode

<details>
<summary>Custom Mode</summary>

**Definition:** A Custom Mode keeps a skill in context for the whole session (badge in the chat input), instead of attaching it to a single message.

**How it works:**

- Pick the skill from `/`, then Option+Enter (Mac) / Alt+Enter (Windows), or **Use as Mode**.
- Optional `icon` (`code`, `terminal`, `bug`, `git-branch`, `book-open`, `beaker`, `shield`, `rocket`, …) and `color` (`default`, `green`, `cyan`, `blue`, `purple`, `magenta`, `orange`, `yellow`, `red`, `brand`). Unknown values fall back to the default lightning badge.
- Available in the Agents Window and the CLI.

**When to use:** Long sessions that should stay on one playbook (TDD, code review checklist). Skip for one-shot tasks.

**Sources:** [Using a skill as a Custom Mode](https://cursor.com/docs/skills.md#using-a-skill-as-a-custom-mode) · [Prompting agents](https://cursor.com/docs/agent/prompting.md)

</details>

## Cloud Agents sync

<details>
<summary>Cloud Agents sync</summary>

**Definition:** Personal skills in `~/.cursor/skills/` stay on your machine until you turn on **Sync Skills for Cloud Agents** so Cloud Agents can use the same private playbooks.

**How it works:**

1. Open **Settings → Agents**.
2. Under **Context and Tools**, turn on **Sync Skills for Cloud Agents** (you can also start sync from **Customize → Skills**).
3. Cursor copies `~/.cursor/skills/` for your Cloud Agents. Synced skills stay **private to you** — not shared with teammates.
4. Turn sync off to move synced skills back to your machine.
5. **Only** `~/.cursor/skills/` syncs — not project skills, not `~/.agents/skills/`, not other local files.
6. On **Teams / Enterprise**, admins can disable sync for everyone under **Team Settings → Security & Identity**.

**When to use:** You rely on personal skills locally and want the same workflows on Cloud Agents. To share with teammates, [publish to team](#publish-to-team) instead of sync.

**Sources:** [Use personal skills with Cloud Agents](https://cursor.com/docs/skills.md#use-personal-skills-with-cloud-agents) · [Skills help — Cloud Agents](https://cursor.com/help/customization/skills#how-do-i-use-personal-skills-with-cloud-agents)

</details>

## Publish to team

<details>
<summary>Publish to team</summary>

**Definition:** **Publish** puts a personal skill on your team’s **Default marketplace** so teammates can install it. Publishing is different from [Cloud sync](#cloud-agents-sync): sync is private; publish is team-wide sharing.

**How it works:**

1. Open **Customize → Skills**.
2. Open a personal skill.
3. Choose **Publish** — Cursor adds it to the team Default marketplace.
4. Teammates install it from the marketplace; Cursor stores a copy in a repository hosted for your team.

**When to use:** A workflow should be standard for the whole team (deploy checklist, review playbook). Keep one-off personal habits in `~/.cursor/skills/` with sync instead.

**Sources:** [Publish a skill to your team](https://cursor.com/help/customization/skills#how-do-i-publish-a-skill-to-my-team)

</details>

## Viewing and installing skills

<details>
<summary>Viewing and installing skills</summary>

**Definition:** **Customize → Skills** lists discovered skills (project, personal, plugin). Project skills also appear under **Agent Decides** next to rules.

**How it works:**

- **Browse:** Sidebar **Customize → Skills** — see what Cursor loaded from [skill directories](#skill-directories).
- **From GitHub:** Skills are **not** imported standalone. Package them in a [plugin](./0_ai_cursor_plugins.md) with `.cursor-plugin/marketplace.json`, import via **From GitHub Repository** or a team marketplace, then install the plugin.
- **Personal → Cloud:** [Cloud sync](#cloud-agents-sync). **Personal → team:** [Publish to team](#publish-to-team).

**When to use:** Audit what Agent can auto-pick before debugging a skill that never fires. Use plugins when sharing skills from a repo.

**Sources:** [Viewing skills](https://cursor.com/docs/skills.md#viewing-skills) · [Installing skills from a repository](https://cursor.com/docs/skills.md#installing-skills-from-a-repository)

</details>

## Built-in skills

<details>
<summary>Built-in skills</summary>

**Definition:** Cursor ships built-in skills (managed by Cursor) in the `/` menu. Agent may also auto-use some when your request clearly matches.

**How it works:**

| Skill                     | What it does                                       |
| ------------------------- | -------------------------------------------------- |
| `/automate`               | Cursor Automations (schedules, Slack, GitHub, …)   |
| `/autopilot`              | Monitor a PR — feedback, conflicts, CI, follow-ups |
| `/canvas`                 | Interactive React artifact beside chat             |
| `/create-hook`            | Hooks + `hooks.json`                               |
| `/create-rule`            | Project/user rules with correct scope              |
| `/create-skill`           | Scaffold `SKILL.md` and folder structure           |
| `/create-subagent`        | Custom subagents with focused roles                |
| `/cursor-blame`           | Trace AI-authored changes and prompts              |
| `/loop`                   | Re-run a prompt/skill on an interval               |
| `/migrate-to-skills`      | Convert eligible rules/commands → skills           |
| `/review`                 | Pick and run code-review agent                     |
| `/review-bugbot`          | Bug/regression review                              |
| `/review-security`        | Security review                                    |
| `/sdk`                    | Cursor SDK integrations                            |
| `/shell`                  | Run literal shell command                          |
| `/split-to-prs`           | Split large changes into smaller PRs               |
| `/statusline`             | CLI status line config                             |
| `/update-cli-config`      | Edit `~/.cursor/cli-config.json`                   |
| `/update-cursor-settings` | Find/update Cursor or VS Code settings             |

**When to use:** Prefer built-ins for scaffolding (rules, skills, hooks) and reviews — don’t reinvent prompts. Everyday slash list: [Slash commands](./0_ai_cursor_slash.md). Deeper notes: [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md).

**Sources:** [Built-in Cursor skills](https://cursor.com/docs/skills.md#built-in-cursor-skills)

</details>

## Convert rule to skill

<details>
<summary>Convert rule to skill</summary>

**Definition:** Turn one **Apply Intelligently** project rule into a skill when the content is a multi-step playbook rather than a short constraint.

**How it works:**

1. Type `/create-skill` and ask Agent to convert the rule (e.g. “turn `@my-rule` into a skill”).
2. Review the generated `.cursor/skills/<name>/SKILL.md`.
3. Delete the original `.cursor/rules/<name>.mdc` if you no longer need it.

**When to use:** A single rule outgrew “always-on guidance” and needs scripts, references, or explicit `/` invocation. For bulk conversion of rules **and** slash commands, use [Migrate to skills](#migrate-to-skills).

**Sources:** [Convert a rule into a skill](https://cursor.com/help/customization/skills#how-do-i-convert-a-rule-into-a-skill)

</details>

## Migrate to skills

<details>
<summary>Migrate to skills</summary>

**Definition:** `/migrate-to-skills` (Cursor 2.4+) converts eligible dynamic rules and slash commands into Agent Skills.

**How it works:**

- Converts “Apply Intelligently” rules (`alwaysApply: false` / unset, no `globs`) → normal skills.
- Converts user/workspace `.cursor/commands` → skills with `disable-model-invocation: true`.
- Does **not** migrate `alwaysApply: true`, glob-scoped rules, or User Rules (not on disk).

**Examples:**

```text
# In Agent chat
/migrate-to-skills
```

**Sources:** [Migrating rules and commands to skills](https://cursor.com/docs/skills.md#migrating-rules-and-commands-to-skills)

</details>

## Interview Practice

Q&A moved to [Cursor — Questions](./0_ai_cursor_questions.md).
