# Cursor — Hooks

Shell-скрипты, которые Cursor запускает на этапах жизненного цикла Agent, Tab и workspace. Проверяют, разрешают, запрещают или переписывают действия через JSON на stdin/stdout.

### Related docs

- [AI workflow](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Subagents](./0_ai_cursor_subagents.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area         | Topics                                                                                                                |
| ------------ | --------------------------------------------------------------------------------------------------------------------- |
| **Basics**   | [What are hooks](#what-are-hooks), [Setup](#setup)                                                                    |
| **Events**   | [Agent lifecycle events](#agent-lifecycle-events), [Shell & MCP hooks](#shell-and-mcp-hooks), [Tab hooks](#tab-hooks) |
| **Control**  | [Matchers](#matchers), [preToolUse & postToolUse](#pretooluse-and-posttooluse), [Exit codes](#exit-codes)             |
| **Advanced** | [Session & stop hooks](#session-and-stop-hooks), [Subagent hooks](#subagent-hooks), [RTK proxy](#rtk-proxy)           |

### Sections

- [What are hooks](#what-are-hooks)
- [Setup](#setup)
- [Agent lifecycle events](#agent-lifecycle-events)
- [Shell and MCP hooks](#shell-and-mcp-hooks)
- [Tab hooks](#tab-hooks)
- [Matchers](#matchers)
- [preToolUse and postToolUse](#pretooluse-and-posttooluse)
- [Exit codes](#exit-codes)
- [Session and stop hooks](#session-and-stop-hooks)
- [Subagent hooks](#subagent-hooks)
- [RTK proxy](#rtk-proxy)
- [Interview Practice](#interview-practice)

---

## What are hooks

<details>
<summary>What are hooks</summary>

**Definition:** A **hook** is an executable script (usually bash) that Cursor invokes at defined lifecycle points. The script reads **JSON from stdin**, writes **JSON to stdout**, and can allow, deny, or modify Agent/Tab actions **outside the model**.

**How it works:**

- Declared in `hooks.json` (`"version": 1`).
- Project hooks: `.cursor/hooks.json` — scoped to the repo, shareable in git.
- User hooks: `~/.cursor/hooks.json` — all projects for this user.
- Both can be active; project hooks suit team policy, user hooks suit personal RTK/formatting.
- Scaffold with `/create-hook`. [Plugins](./0_ai_cursor_plugins.md) can bundle `hooks/hooks.json`.

**When to use:** Deterministic guardrails — block `rm -rf`, rewrite `git` → `rtk git`, auto-format after edits, audit compliance, gate [subagent](./0_ai_cursor_subagents.md) types. Skip when [Run Modes](./0_ai_cursor_core.md#agent-autonomy-run-modes) + [rules](./0_ai_cursor_rules.md) are enough — hooks are code you maintain.

**Pluses and minuses:**

| +                                                   | −                                              |
| --------------------------------------------------- | ---------------------------------------------- |
| Enforce policy regardless of model behavior.        | Bad JSON or bugs can block or confuse runs.    |
| `updated_input` rewrites tool args (RTK, `npm ci`). | Not every UI action goes through hooks.        |
| Auditable shell trail for enterprise.               | Team must review scripts like production code. |

**Sources:** [Hooks](https://cursor.com/docs/hooks)

</details>

## Setup

<details>
<summary>Setup</summary>

**Definition:** Minimal hook setup = `hooks.json` + executable script(s).

**How it works:**

**Project layout:**

```text
.cursor/
├── hooks.json
└── hooks/
    ├── audit.sh
    ├── format.sh
    └── rtk-rewrite.sh
```

**`hooks.json` (project):**

```json
{
  "version": 1,
  "hooks": {
    "afterFileEdit": [{ "command": ".cursor/hooks/format.sh" }]
  }
}
```

**Script skeleton:**

```bash
#!/bin/bash
input=$(cat)
# parse with jq …
echo '{"permission":"allow"}'
exit 0
```

```bash
chmod +x .cursor/hooks/format.sh
```

**User-level:** same shape under `~/.cursor/`; paths relative to `~/.cursor/` (e.g. `./hooks/format.sh`).

**When to use:** Start with one event you care about (`afterFileEdit`, `beforeShellExecution`, or `preToolUse`); expand after observing real Agent commands.

**Sources:** [Hooks — project & user setup](https://cursor.com/docs/hooks)

</details>

## Agent lifecycle events

<details>
<summary>Agent lifecycle events</summary>

**Definition:** Hooks that fire around Agent sessions, tools, compaction, and responses.

**How it works:**

| Event                | When it fires                                    | Typical use                        |
| -------------------- | ------------------------------------------------ | ---------------------------------- |
| `sessionStart`       | New Composer conversation                        | Inject context, set `env` vars     |
| `sessionEnd`         | Conversation ends                                | Audit log, cleanup                 |
| `preToolUse`         | Before any tool (Shell, Read, Write, Task, MCP…) | Allow/deny/rewrite tool input      |
| `postToolUse`        | After successful tool                            | Audit, inject `additional_context` |
| `postToolUseFailure` | After failed tool                                | Alerting, fallback context         |
| `beforeSubmitPrompt` | Before user prompt sent                          | Block/redact prompts               |
| `preCompact`         | Before context compaction                        | Audit what gets summarized         |
| `stop`               | Agent loop ends                                  | Auto `followup_message` (bounded)  |
| `afterAgentResponse` | After agent message                              | Logging, metrics                   |
| `afterAgentThought`  | After thinking block                             | Compliance on reasoning traces     |
| `workspaceOpen`      | Workspace opened                                 | Register plugins, env setup        |

Permission-style events (`preToolUse`, `beforeShellExecution`, `beforeMCPExecution`, `beforeReadFile`, `subagentStart`, Tab read hooks): invalid JSON or schema mismatch **blocks** the action on exit `0`; exit `2` = deny.

**When to use:** `preToolUse` for broad tool policy; `sessionStart` for repo-specific system context; `stop` for agentic loops with `loop_limit`.

**Sources:** [Hooks](https://cursor.com/docs/hooks) · [Plugin hooks reference](https://cursor.com/docs/reference/plugins)

</details>

## Shell and MCP hooks

<details>
<summary>Shell and MCP hooks</summary>

**Definition:** Finer-grained hooks for shell commands and MCP tool calls — in addition to generic `preToolUse` / `postToolUse`.

**How it works:**

| Event                  | When it fires             | Typical use                                 |
| ---------------------- | ------------------------- | ------------------------------------------- |
| `beforeShellExecution` | Before shell runs         | Block `git`, `curl`, `rm -rf`; ask for `gh` |
| `afterShellExecution`  | After shell completes     | Parse exit code, log duration               |
| `beforeMCPExecution`   | Before MCP tool           | Deny risky MCP servers                      |
| `afterMCPExecution`    | After MCP tool            | Redact secrets from MCP output              |
| `beforeReadFile`       | Before Agent reads a file | Block `.env`, keys, credentials paths       |
| `afterFileEdit`        | After Agent edits a file  | Run formatter, linter                       |

`beforeShellExecution` input includes the command string; output uses `permission: "allow" | "deny" | "ask"` (ask = prompt user).

**When to use:** `beforeShellExecution` when you only care about terminal strings; `preToolUse` with `matcher: "Shell"` when you want one script for all Shell tool calls including rewritten input.

**Examples:**

```json
{
  "beforeShellExecution": [
    {
      "command": ".cursor/hooks/block-git.sh",
      "matcher": "git "
    }
  ],
  "afterMCPExecution": [{ "command": ".cursor/hooks/redact-mcp.sh" }]
}
```

**Sources:** [Hooks — shell execution](https://cursor.com/docs/hooks)

</details>

## Tab hooks

<details>
<summary>Tab hooks</summary>

**Definition:** Hooks for **Cursor Tab** inline completions — separate from Agent chat.

**How it works:**

| Event               | When it fires                 | Typical use                     |
| ------------------- | ----------------------------- | ------------------------------- |
| `beforeTabFileRead` | Before Tab reads file context | Redact secrets from Tab context |
| `afterTabFileEdit`  | After Tab applies an edit     | Format Tab-generated patches    |

Tab hooks use the same stdin/stdout JSON pattern. Useful when Agent rules do not apply to Tab but you still need secret redaction or formatting.

**When to use:** Repos with credentials in paths Tab might touch; teams that want Prettier on Tab edits.

**Thoughts:** Agent [rules](./0_ai_cursor_rules.md) explicitly do **not** apply to Tab — Tab hooks are the policy layer for completions.

**Sources:** [Hooks — Tab](https://cursor.com/docs/hooks)

</details>

## Matchers

<details>
<summary>Matchers</summary>

**Definition:** Optional **regex** on hook entries to run a script only for matching tools, subagents, or shell commands.

**How it works:**

| Hook event             | `matcher` matches against                             |
| ---------------------- | ----------------------------------------------------- |
| `preToolUse`           | Tool name (`Shell`, `Read`, `Write`, `Task`, …)       |
| `subagentStart`        | Subagent type (`explore`, `locale-parity-auditor`, …) |
| `beforeShellExecution` | Full command string                                   |

- Empty string or `*` → match everything.
- Multiple hooks can stack; order matters for your policy design.

**Examples:**

```json
{
  "preToolUse": [{ "command": "./validate-shell.sh", "matcher": "Shell" }],
  "subagentStart": [
    { "command": "./validate-explore.sh", "matcher": "explore|shell" }
  ],
  "beforeShellExecution": [
    { "command": "./approve-network.sh", "matcher": "curl|wget|nc " }
  ]
}
```

**When to use:** One fat script for all events vs small focused scripts per matcher — prefer focused scripts for easier testing.

**Sources:** [Hooks — matchers](https://cursor.com/docs/hooks)

</details>

## preToolUse and postToolUse

<details>
<summary>preToolUse and postToolUse</summary>

**Definition:** The most common Agent hooks — run **before** or **after** any tool invocation.

**How it works:**

**`preToolUse` input (abridged):** `tool_name`, `tool_input`, `tool_use_id`, `cwd`, `model`, `agent_message`.

**`preToolUse` output:**

```json
{
  "permission": "allow",
  "user_message": "shown when denied",
  "agent_message": "fed back to agent when denied",
  "updated_input": { "command": "rtk git status" }
}
```

- `permission`: `"allow"` | `"deny"` (`"ask"` in schema but not enforced today for this hook).
- `updated_input` replaces tool args — classic RTK rewrite pattern.

**`postToolUse` output:**

```json
{
  "additional_context": "Test coverage report attached.",
  "updated_mcp_tool_output": { "modified": "output" }
}
```

- `additional_context` — injected into conversation after the tool result.
- `updated_mcp_tool_output` — **MCP tools only**; replaces what the model sees.

**When to use:** `preToolUse` for rewrite/deny; `postToolUse` for audit trails and enriching context without changing the tool itself.

**Sources:** [Hooks — preToolUse](https://cursor.com/docs/hooks#pretooluse) · [postToolUse](https://cursor.com/docs/hooks)

</details>

## Exit codes

<details>
<summary>Exit codes</summary>

**Definition:** Hook scripts communicate success, denial, and failure through **exit code** + stdout JSON.

**How it works:**

| Exit code | Meaning                                                         |
| --------- | --------------------------------------------------------------- |
| `0`       | Success — parse stdout JSON                                     |
| `2`       | Block action (same as `permission: "deny"` on permission hooks) |
| Other     | Hook failed — action **proceeds** (fail-open by default)        |

On permission hooks (`preToolUse`, `beforeShellExecution`, `beforeMCPExecution`, `beforeReadFile`, `subagentStart`, Tab read hooks): **invalid JSON on exit 0 blocks the action**.

**When to use:** Exit `2` for quick deny without crafting JSON. Log to `/tmp/hooks.log` while debugging — malformed stdout is the #1 failure mode.

**Examples:**

```bash
#!/bin/bash
cat > /dev/null
exit 2   # deny without JSON
```

**Thoughts:** Fail-open on script crashes is deliberate — broken hook should not brick the IDE forever; still fix hooks quickly.

**Sources:** [Hooks — exit codes](https://cursor.com/docs/hooks)

</details>

## Session and stop hooks

<details>
<summary>Session and stop hooks</summary>

**Definition:** Hooks for conversation bootstrapping and **agent loop continuation**.

**How it works:**

**`sessionStart`** (fire-and-forget):

```json
// Input
{ "session_id": "…", "is_background_agent": false, "composer_mode": "agent" }

// Output
{
  "env": { "NODE_ENV": "test" },
  "additional_context": "This repo uses vitest. Run npm test after edits."
}
```

**`stop`** — when the agent loop ends:

```json
// Input
{ "status": "completed", "loop_count": 0 }

// Output
{ "followup_message": "Run npm run check-order -- --strict on touched files." }
```

- `followup_message` auto-submits as the next user message.
- `loop_limit` on the hook entry caps iterations (e.g. `10`) to prevent infinite loops.

**When to use:** `sessionStart` for repo conventions every new chat should know; `stop` for “keep going until checks pass” automation (use carefully).

**Sources:** [Hooks — sessionStart](https://cursor.com/docs/hooks) · [stop hook](https://cursor.com/docs/hooks)

</details>

## Subagent hooks

<details>
<summary>Subagent hooks</summary>

**Definition:** `subagentStart` and `subagentStop` run when the parent spawns or finishes a [subagent](./0_ai_cursor_subagents.md) via Task.

**How it works:**

**`subagentStart` input:**

```json
{
  "subagent_type": "generalPurpose",
  "task": "Explore the authentication flow",
  "subagent_model": "claude-sonnet-4-20250514",
  "is_parallel_worker": false
}
```

**`subagentStart` output:**

```json
{ "permission": "allow" }
```

- Only `"allow"` | `"deny"` — `"ask"` is treated as deny.
- Use `matcher` to allow only `explore` or only `readonly` custom auditors.

**`subagentStop`:** audit duration, log parallel worker results, compliance.

**When to use:** Enterprise policy (“no browser subagent on prod branches”), or only permit known custom auditors.

**Examples:**

```json
{
  "subagentStart": [
    {
      "command": ".cursor/hooks/allow-readonly-auditors.sh",
      "matcher": "locale-parity-auditor|verifier|security-auditor"
    }
  ]
}
```

**Sources:** [Hooks — subagentStart](https://cursor.com/docs/hooks)

</details>

## RTK proxy

<details>
<summary>RTK proxy</summary>

**Definition:** [RTK](./0_ai.md#rtk-rust-token-killer) (Rust Token Killer) compresses noisy `git` / `rg` / test output. It hooks into Cursor via **`preToolUse`** rewriting Shell commands to `rtk …`.

**How it works:**

1. Install: `rtk init --global --agent cursor` — registers hook behavior.
2. Agent types `git status` → hook or RTK rewrites to `rtk git status` before execution.
3. Model sees compact output; more context room for the real task.
4. Bypass: `RTK_DISABLED=1 git status` or `exclude_commands` in `~/.config/rtk/config.toml`.
5. **Read / Grep / Glob** tools do **not** pass through the Bash hook — use shell `rg` or `rtk grep` when you need compression there.

**When to use:** Long diffs, huge test logs, busy `git status`. Skip when you need raw full output for debugging.

**Examples:**

```json
{
  "preToolUse": [
    {
      "command": ".cursor/hooks/rtk-rewrite.sh",
      "matcher": "Shell"
    }
  ]
}
```

**Thoughts:** If the agent “misses” files from `git status`, check whether RTK collapsed the listing.

**Sources:** [RTK](https://github.com/RTK-AI/rtk) · [Hooks](https://cursor.com/docs/hooks) · [AI workflow — RTK](./0_ai.md#rtk-rust-token-killer)

</details>

## Interview Practice

### Questions & Answers

**Q: What are Cursor hooks?**  
A: Executable scripts on lifecycle events that read JSON stdin and return JSON stdout to allow, deny, or rewrite Agent/Tab actions.

**Q: Where is `hooks.json`?**  
A: Project: `.cursor/hooks.json`. User (all repos): `~/.cursor/hooks.json`.

**Q: Name three common hook events.**  
A: `preToolUse` (before any tool), `beforeShellExecution` (before terminal), `afterFileEdit` (after Agent edits a file).

**Q: What does `updated_input` do?**  
A: On `preToolUse`, replaces tool arguments before execution — e.g. rewrite `git status` to `rtk git status`.

**Q: Exit code 2 on a permission hook?**  
A: Blocks the action — equivalent to `permission: "deny"`.

**Q: How do hooks relate to subagents?**  
A: `subagentStart` / `subagentStop` fire when Task spawns or finishes a subagent; use matchers to allow only specific types.

**Q: Do hooks apply to Tab?**  
A: Agent rules do not; Tab has its own `beforeTabFileRead` and `afterTabFileEdit` hooks.
