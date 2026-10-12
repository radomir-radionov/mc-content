# Cursor — Plugins

Distributable bundles that package skills, rules, agents, commands, hooks, and MCP servers for Cursor (and portable Agent Plugins for skills + MCP).

### Related docs

- [AI workflow](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area         | Topics                                                                             |
| ------------ | ---------------------------------------------------------------------------------- |
| **Formats**  | [Two plugin formats](#two-plugin-formats), [Manifest](#cursor-plugin-manifest)     |
| **Contents** | [Component discovery](#component-discovery), [MCP & variables](#mcp-and-variables) |
| **Share**    | [Marketplace & install](#marketplace-and-install)                                  |

### Sections

- [Two plugin formats](#two-plugin-formats)
- [Cursor Plugin manifest](#cursor-plugin-manifest)
- [Component discovery](#component-discovery)
- [MCP and variables](#mcp-and-variables)
- [Marketplace and install](#marketplace-and-install)
- [Interview Practice](#interview-practice)

---

## Two plugin formats

<details>
<summary>Two plugin formats</summary>

**Definition:** Cursor loads plugins as either **Agent Plugins** (open standard) or **Cursor Plugins** (full Cursor component set), detected by where the manifest lives.

**How it works:**

| Format         | Manifest                     | Components                                             |
| -------------- | ---------------------------- | ------------------------------------------------------ |
| Agent Plugins  | `plugin.json` at plugin root | Skills, MCP servers                                    |
| Cursor Plugins | `.cursor-plugin/plugin.json` | Skills, MCP, rules, agents, commands, hooks, variables |

**When to use:** Agent Plugin when you only need portable skills/MCP. Cursor Plugin when you also ship rules, hooks, or slash commands.

**Sources:** [Plugins reference](https://cursor.com/docs/reference/plugins)

</details>

## Cursor Plugin manifest

<details>
<summary>Cursor Plugin manifest</summary>

**Definition:** Every Cursor Plugin requires `.cursor-plugin/plugin.json` with at least a kebab-case `name`.

**How it works:**

- Required: `name` (lowercase kebab-case, alphanumerics/hyphens/periods).
- Optional metadata: `description`, `version`, `author`, `homepage`, `repository`, `license`, `keywords`, `logo`.
- Optional path overrides: `rules`, `skills`, `agents`, `commands`, `hooks`, `mcpServers`, `variables`.
- Typical layout: `rules/`, `skills/*/SKILL.md`, `agents/`, `commands/`, `hooks/hooks.json`, `mcp.json`, `assets/`.

**Examples:**

```json
{
  "name": "enterprise-plugin",
  "version": "1.2.0",
  "description": "Security scanning and compliance checks",
  "author": { "name": "ACME DevTools" },
  "keywords": ["enterprise", "security"]
}
```

**Sources:** [Cursor Plugin manifest](https://cursor.com/docs/reference/plugins#cursor-plugin-manifest)

</details>

## Component discovery

<details>
<summary>Component discovery</summary>

**Definition:** If the manifest omits a component path, Cursor discovers standard folders under the plugin root.

**How it works:**

| Component  | Default            | Discovery                                     |
| ---------- | ------------------ | --------------------------------------------- |
| Skills     | `skills/`          | Subdirs with `SKILL.md`                       |
| Rules      | `rules/`           | `.md` / `.mdc` / `.markdown`                  |
| Agents     | `agents/`          | markdown / mdc                                |
| Commands   | `commands/`        | markdown / text                               |
| Hooks      | `hooks/hooks.json` | Hook event names                              |
| MCP        | `mcp.json`         | Server entries                                |
| Root skill | `SKILL.md` at root | Only if no `skills/` and no manifest `skills` |

- An explicit manifest field (e.g. `"skills": "./my-skills/"`) **replaces** default discovery for that type.
- Multi-plugin repos use `.cursor-plugin/marketplace.json` listing each plugin `source` directory.

**When to use:** Stick to defaults unless you need a non-standard layout.

**Sources:** [Component discovery](https://cursor.com/docs/reference/plugins#cursor-plugin-component-discovery)

</details>

## MCP and variables

<details>
<summary>MCP and variables</summary>

**Definition:** Plugins can ship MCP servers via `mcp.json`. Secrets are declared as `variables` in the manifest and filled in the dashboard — never committed.

**How it works:**

- Use `${VAR}` placeholders in `mcp.json`; declare matching names under `variables` (JSON Schema) in `plugin.json`.
- Team admins set values under Plugins → Configure.
- Cursor expands `${CURSOR_PLUGIN_ROOT}` / `${CLAUDE_PLUGIN_ROOT}` for the install path in commands/args/env/cwd.
- Agent Plugins’ `${PLUGIN_ROOT}` / `${PLUGIN_DATA}` are **not** expanded in Cursor — use `${CURSOR_PLUGIN_ROOT}`.

**Examples:**

```json
{
  "mcpServers": {
    "example-api": {
      "url": "https://mcp.example.com/mcp",
      "headers": { "Authorization": "Bearer ${API_TOKEN}" }
    }
  }
}
```

**Thoughts:** Plugin-managed MCP config is read-only in the dashboard once installed.

**Sources:** [Variables](https://cursor.com/docs/reference/plugins#variables) · [MCP servers](https://cursor.com/docs/reference/plugins#mcp-servers)

</details>

## Marketplace and install

<details>
<summary>Marketplace and install</summary>

**Definition:** Plugins are installed from Cursor marketplaces (official, team, or GitHub repos with `.cursor-plugin/marketplace.json`). Skills/rules from a repo arrive **only** packaged inside a plugin. For community browsing of rules/MCP (not the official publish path), see [Cursor Directory](./0_ai_cursor_core.md#cursor-directory).

**How it works:**

- Customize → import **From GitHub Repository** (repo needs marketplace.json), or add a team marketplace.
- Publish public plugins via [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).
- Installed skills/rules appear in Customize alongside local ones.

**When to use:** Share team kits (e.g. cursor-team-kit, Prisma, Vercel plugins) instead of copying `.mdc` files by hand. Skip for one-off personal skills — use `~/.cursor/skills/`.

**Sources:** [Submitting a plugin](https://cursor.com/docs/reference/plugins#submitting-a-plugin) · [Importing rules](https://cursor.com/docs/rules#importing-rules-from-a-repository) · [cursor.directory](https://cursor.directory/)

</details>

## Interview Practice

Q&A moved to [Cursor — Questions](./0_ai_cursor_questions.md).
