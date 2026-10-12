# AI-assisted development

How to use Cursor, Copilot, Claude Code, and doc tools without shipping hallucinated APIs. Keep raw links below.

### Related docs

- [Cursor — Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md) · [Interview hub](./0_interview.md) · [MVP & judgment](./0_mvp.md) · [TypeScript](./5_ts.md) · [Next.js](./4_nextjs.md)

### Main concepts

| Area         | Topics                                                                                                                                                                                                                                                                                                     |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Product**  | [AI-first approach](#ai-first-approach) vs AI-enabled vs AI-augmented                                                                                                                                                                                                                                      |
| **Workflow** | [AI-assisted editors](#ai-assisted-workflow-cursor-copilot-claude-code), [Context window](#context-window-llm-context), [Context7 / Ref](#context7-and-reftools), [RTK](#rtk-rust-token-killer)                                                                                                            |
| **Cursor**   | [Core](./0_ai_cursor_core.md) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md) |

### Sections

- [AI-first approach](#ai-first-approach)
- [AI-assisted workflow](#ai-assisted-workflow-cursor-copilot-claude-code)
- [Context window (LLM context)](#context-window-llm-context)
- [Context7 and Ref.tools](#context7-and-reftools)
- [RTK](#rtk-rust-token-killer)
- [Interview Practice](#interview-practice)

---

## AI-first approach

<details>
<summary>AI-first approach</summary>

**Definition:** An **AI-first** product is designed around a model from the start — remove the model and there is no product. **AI-enabled** means the base product still works and AI is a feature. **AI-augmented** means the _team_ uses AI to ship faster; the product itself may not contain a model.

**How it works:**

- **AI-first (product):** the core loop is a model (search ranking, assistant, codegen). Google used this phrasing for the shift after mobile-first (Pichai, 2016 Founders’ Letter: “We will move from mobile first to an AI first world”).
- **AI-enabled (product):** CRUD/UI still works offline from the model; chat, summaries, or suggestions are add-ons.
- **AI-augmented (team):** Cursor, Copilot, Claude Code, Context7/Ref, RTK — the handbook you are reading is this bucket unless the _product_ is itself a model.

**When to use:** Product interviews (“is this AI-first?”), architecture reviews, and when deciding whether to wrap an LLM as the product vs as a coding aid. Skip the slogan when you only mean “we use ChatGPT at work.”

**Pluses and minuses:**

| +                                                              | −                                                              |
| -------------------------------------------------------------- | -------------------------------------------------------------- |
| Forces a clear product thesis: what dies if the model is down? | Easy to confuse team tooling with the customer-facing product. |
| Matches how large platforms talk about assistants vs screens.  | “AI-first” is marketing unless the model _is_ the loop.        |

**Thoughts:** In this repo, most of the AI track is **AI-augmented development**. Call a note **AI-first** only when the shipped product cannot exist without the model.

**Sources:** [Google Founders’ Letter 2016](https://blog.google/alphabet/this-years-founders-letter/)

</details>

## AI-assisted workflow (Cursor, Copilot, Claude Code)

<details>
<summary>AI-assisted workflow (Cursor, Copilot, Claude Code)</summary>

**Definition:** AI-assisted editors run a coding **agent** (tools: read, edit, shell, search) and/or **inline completions**. The agent is not a source of truth — official docs and the compiler are.

**How it works:**

- **Cursor Agent (this chat):** can edit the workspace, run the terminal, and call MCP. User Rules apply to Agent chat, not to Tab or Cmd/Ctrl+K inline edit. Project rules (`.cursor/rules`), [skills](./0_ai_cursor_skills.md), and [slash commands](./0_ai_cursor_slash.md) load here.
- **Cursor Tab / Cmd+K:** completions and inline edits. Faster for local tweaks; they do not follow the same Agent rules/MCP loop.
- **Cursor CLI (`agent`):** same class of agent in a terminal/CI (`agent -p "…"`). Use when there is no editor window.
- **GitHub Copilot:** inline completions plus Copilot Chat (and Agent mode in current VS Code). Chat uses open files / repo context; optional `.github/copilot-instructions.md`. Org admins can disable chat or agent.
- **Claude Code:** terminal agent (`claude`). Tools (Read, Edit, Bash, Grep, …) make it agentic; without tools it only talks. Headless: `claude -p`. Built-in Read/Grep/Glob do **not** pass through a Bash hook — RTK only rewrites shell commands unless you call `rtk read` / `rtk grep` yourself.

**When to use:** Multi-file handbook/conspect work → Cursor Agent. One-hunk edits → Tab/Cmd+K. SSH/CI → Cursor CLI or `claude -p`. Stay in Copilot if that is the team standard.

**Pluses and minuses:**

| +                                              | −                                                                                       |
| ---------------------------------------------- | --------------------------------------------------------------------------------------- |
| Agent can grep, patch, and verify in the repo. | Training data hallucinates APIs; always fetch docs (Context7/Ref) before writing facts. |
| CLI agents automate CI and remote boxes.       | CLI does not give editor diffs, Tab, or the same GUI approvals.                         |

**Examples:**

```bash
# Cursor CLI (no editor)
agent -p "Fix the failing lint on this branch"

# Claude Code headless
claude -p "Explain why parseTopicMarkdown returns 0 concepts"
```

**Thoughts:** For MyConspect day to day, stay in this Agent panel. Treat Copilot/Claude Code as the same _pattern_ (completions vs tools), not as extra magic.

**Sources:** [Cursor rules](https://cursor.com/docs/rules) · [GitHub Copilot Chat in IDEs](https://docs.github.com/en/copilot/how-tos/chat-with-copilot/chat-in-ide) · [Claude Code tools](https://code.claude.com/docs/en/glossary)

</details>

## Context window (LLM context)

<details>
<summary>Context window (LLM context)</summary>

**Definition:** In an LLM or coding agent, **context** is everything the model can read in the current conversation: system instructions, your messages, attached files, tool definitions, and tool results. It is measured in **tokens** and capped by the model’s **context window**. When the window fills, older material is summarized or dropped — the model does not have perfect memory of the whole thread.

**How it works:**

- **Fixed budget:** each chat shares one context window with the chosen model. Bigger windows cost more and are not infinite.
- **Context types (context ring):** Cursor splits the budget into categories. Open the ring beside the prompt to see which ones are heavy.

| Category                    | What it is                                                              | Loads when                                                                 |
| --------------------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **System prompt**           | Cursor’s base Agent instructions                                        | Every Agent chat                                                           |
| **Tools**                   | Schemas for Read, Edit, shell, search, Task, …                          | Every Agent chat                                                           |
| **Rules**                   | Project / user / team `.mdc` constraints                                | Matching rules each turn ([precedence](./0_ai_cursor_rules.md#precedence)) |
| **Skills**                  | `SKILL.md` playbooks (description in system context; body when invoked) | Relevant skills per turn, or pinned in a Custom Mode                       |
| **MCP**                     | Connected MCP server catalogs + tool docs                               | When MCP servers are enabled                                               |
| **Subagents**               | Built-in + custom subagent type docs for Task                           | When subagents are available                                               |
| **Conversation**            | Your messages, agent replies, tool results                              | Grows every turn                                                           |
| **Summarized conversation** | Compressed older turns                                                  | After the window nears capacity                                            |

- **`@` attachments (per message, on top of the table):** `@Files` / `@Folders`, `@Terminals`, `@Chats`, `@Commit` / `@Branch` (git diff), `@Browser`. Agent search can replace `@` when you do not know the path yet.
- **MyConspect repo (project context):**

| Layer           | What you have                                                                                                                | Typical load                                                   |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **Rules**       | `handbook-app`, `study-notes-structure`, `locale-sync`, `resources-official-docs` in `.cursor/rules/`                        | Intelligent — matched by prompt (app vs conspect vs RU sync)   |
| **Skills**      | `update-conspect`, `sync-locales`, `develop-handbook`, `check-order`, `check-conspect` in `.cursor/skills/`                  | Intelligent — conspect edit, locale sync, handbook app, audits |
| **Agent files** | `AGENTS.md` / `CLAUDE.md` (Next.js bundled agent rules from `next dev`)                                                      | When the agent reads project instructions                      |
| **MCP**         | Not committed in-repo — usually Context7, Ref, GitHub, Vercel, browser, … from installed [plugins](./0_ai_cursor_plugins.md) | Per your Cursor settings                                       |
| **User Rules**  | Personal Cursor settings (e.g. TypeScript style, commit policy)                                                              | Every Agent chat — keep short                                  |

Full rules-vs-skills sizing: [Context optimization](./0_ai_cursor_rules.md#context-optimization).

- **Visibility:** the context ring beside the prompt shows how full the window is. Open it for a per-category breakdown (rules vs conversation vs tools, etc.).
- **Near the limit:** Cursor compresses older turns into summaries so new work can continue. Details from early in a long chat may be lossy.
- **Attach on purpose:** `@file`, `@folder`, `@Branch`, `@Terminals`, `@Chats` add specific material. If you are unsure which files matter, skip `@` — the agent can search the repo.
- **Isolate heavy work:** [subagents](./0_ai_cursor_subagents.md) run in their own window and return a summary; [RTK](#rtk-rust-token-killer) shrinks noisy shell output before the model sees it.
- **Do not confuse with Context7:** Context7 is an MCP doc-fetch tool named “context”; this section is about the model’s token budget.

**When to use:** Explaining why long Agent chats “forget” earlier decisions, why subagents/RTK exist, or interview questions on LLM limits. Start a **new chat** for unrelated tasks; use `@Chats` to pull selective history instead of pasting whole threads.

**Pluses and minuses:**

| +                                                             | −                                                                 |
| ------------------------------------------------------------- | ----------------------------------------------------------------- |
| On-demand search means you rarely need to `@` the whole repo. | Tool output and big diffs burn tokens fast.                       |
| Summarization keeps very long sessions usable.                | Summaries can drop nuance — re-attach key files if quality drops. |
| Subagents isolate exploration from the main thread.           | Parallel subagents multiply token spend.                          |

**Examples:**

```text
# Cursor — attach only what you know matters
@lib/parse/load.ts fix the slugify edge case for Cyrillic

# Heavy grep/log work — delegate or compress
Task explore subagent: map all search-index callers
# or: rtk git diff   (see RTK section)
```

**Thoughts:** Treat context like RAM: keep always-on material ([rules](./0_ai_cursor_rules.md)) short, load playbooks ([skills](./0_ai_cursor_skills.md)) when relevant, and offload noisy work to subagents or RTK. If answers get vague late in a chat, restore a checkpoint or start fresh with `@Chats` + the files that matter.

**Sources:** [Cursor — prompting & context usage](https://cursor.com/docs/agent/prompting) · [Subagents](https://cursor.com/docs/context/subagents) · [Agent best practices](https://cursor.com/blog/agent-best-practices)

</details>

## Context7 and Ref.tools

<details>
<summary>Context7 and Ref.tools</summary>

**Definition:** Context7 and Ref are **MCP documentation tools**. They pull current library docs into the agent so it does not invent APIs from training data. The Next.js handbook never calls them — only the agent that edits `resources/` does.

**How it works:**

- **Context7 (two steps):** `resolve-library-id` (`libraryName` + `query`) → pick `/org/project` (prefer official ID, High reputation, version when the note names one) → `query-docs` (`libraryId` + **one** concept per call). Combined queries dilute ranking.
- **When to use Context7:** library/framework/SDK/API/CLI/cloud docs — even React/Next you think you know. **Do not** use it for generic refactoring, code review, or non-library concepts.
- **Ref.tools:** `ref_search_documentation(query)` then `ref_read_url` on a result URL (include the `#hash`). Better for a specific method/signature page. HTTP MCP: `https://api.ref.tools/mcp`.
- **Fallback:** web fetch (MDN, WHATWG, vendor docs) only if both miss.
- **In notes:** put canonical URLs on `**Sources:**`. This site is SSG; visitors do not “sync with MCP.”

**When to use:** Before adding or changing facts in `resources/*.md` (see `.cursor/rules/resources-official-docs.mdc`). Skip for personal hubs with no APIs (`0_me.md`) unless you add a factual claim.

**Pluses and minuses:**

| +                                              | −                                                   |
| ---------------------------------------------- | --------------------------------------------------- |
| Version-specific snippets beat memorized APIs. | Quota/auth failures — then Ref or official URLs.    |
| Ref is token-cheap search → targeted read.     | Context7 is the wrong tool for “rewrite this file.” |

**Thoughts:** “MCP synced” on the public site would be a lie. Truthful signal is `**Sources:**` plus a weekly `npm run check-conspect` URL audit.

**Sources:** [Context7](https://context7.com/) · [Context7 MCP](https://github.com/upstash/context7) · [Ref MCP tools](https://docs.ref.tools/context/mcp/tools)

</details>

## RTK (Rust Token Killer)

<details>
<summary>RTK (Rust Token Killer)</summary>

**Definition:** **RTK** is a CLI proxy that compresses `git` / `rg` / test / Docker output **before** the agent’s LLM sees it (often 60–90% fewer tokens). It is not Cursor CLI.

**How it works:**

- A hook rewrites `git status` → `rtk git status` (Cursor: `preToolUse` / `updated_input`). You still type in Agent chat; the shell tool is rewritten.
- Install for Cursor: `rtk init --global --agent cursor` (or `rtk init -g --agent cursor`), then restart Cursor.
- Compact forms: `rtk git status`, `rtk git diff`, `rtk git log -n 10`. Tests often collapse to failures only.
- Cursor Agent **Read / Grep / Glob** do not go through the Bash hook. For compact search/read, use shell `rg`/`cat` or `rtk grep` / `rtk read`.
- Bypass once: `RTK_DISABLED=1 git status`. Permanent skips: `exclude_commands` in `~/.config/rtk/config.toml`.
- Prompt-only agents (no hook) get instructions to call `rtk …` but do not intercept.

**When to use:** Long diffs, noisy test logs, big `git status`. Skip when you need the raw full output.

**Pluses and minuses:**

| +                                         | −                                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------ |
| More room in context for the actual task. | Hook bugs can silently pass through (treat non-zero `rtk rewrite` as failure). |
| One binary, many ecosystems.              | Built-in editor search tools are not rewritten.                                |

**Examples:**

```bash
rtk init --global --agent cursor
rtk git status
RTK_DISABLED=1 git diff
```

**Thoughts:** If the agent “misses” files that `git status` listed, check whether RTK collapsed the output — or run raw git once.

**Sources:** [rtk-ai/rtk](https://github.com/RTK-AI/rtk) · [Supported agents](https://www.rtk-ai.app/docs/getting-started/supported-agents/)

</details>

## Interview Practice

**Q: What is an AI-first approach?**

Design the product around a model from the start. **AI-first** = remove the model and there is no product. **AI-enabled** = base product still works (AI is a feature). **AI-augmented** = the _team_ uses AI to build faster. Google used “AI-first” for the shift after mobile-first (Pichai, 2016).

**Q: What is “context” in a coding agent?**

Everything the model sees in one conversation — system instructions, messages, attached files, tool definitions, and tool results — measured in **tokens** inside a fixed **context window**. Cursor’s context ring splits spend into **System prompt**, **Tools**, **Rules**, **Skills**, **MCP**, **Subagents**, **Conversation**, and **Summarized conversation**. In MyConspect, project rules are four `.mdc` files and skills are five playbooks under `.cursor/`. Manage budget with targeted `@` mentions, [subagents](./0_ai_cursor_subagents.md), [RTK](#rtk-rust-token-killer), short rules, and new chats for unrelated work.

**Q: Why fetch docs instead of trusting the model?**

Training cutoffs invent APIs. Context7 (`resolve-library-id` → `query-docs`) and Ref (`ref_search_documentation` → `ref_read_url`) read current official pages. Put those URLs in **Sources:**.

**Q: Cursor chat vs Cursor CLI vs RTK?**

Chat/Agent in the IDE is the full agent with diffs and MCP. Cursor CLI is the same agent without the editor. RTK only shrinks shell output after `rtk init --global --agent cursor`.

**Q: Skills vs rules in Cursor — when to use which?**

**Rules** = short persistent constraints; **Skills** = multi-step playbooks. Full Q&A: [Cursor — Questions](./0_ai_cursor_questions.md).
