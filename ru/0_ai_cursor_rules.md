# Cursor — Rules

Постоянные инструкции системного уровня для Agent. Короче, чем [skills](./0_ai_cursor_skills.md); область — always, file globs, relevance или `@mention`.

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

**Definition:** **Правило** — постоянный набор инструкций, которым Agent следует в проекте. Область: глобально, на проект, на организацию или на конкретные файлы. В Cursor **четыре** источника: **Project Rules**, **User Rules**, **Team Rules** и простой **AGENTS.md** (плюс `CLAUDE.md` — см. [CLAUDE.md](#claudemd)). Правила короче [skills](./0_ai_cursor_skills.md); skills — многошаговые playbooks, rules — постоянные ограничения.

**How it works:**

- Когда правило применяется, его текст попадает в **начало** контекста модели на этом ходе Agent.
- Правила только для **Agent (Chat)** — не Tab, Inline Edit (Cmd/Ctrl+K) и не PR-ревью [Bugbot](https://cursor.com/docs/bugbot).
- При конфликте: **Team → Project → User** (см. [Precedence](#precedence)).

| Source              | Where                                                                           | Scope                        | Shape                                                                                |
| ------------------- | ------------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------ |
| **Project Rules**   | `.cursor/rules/*.mdc` (git)                                                     | Этот репозиторий             | Frontmatter: `alwaysApply`, `description`, `globs` — см. [Apply modes](#apply-modes) |
| **User Rules**      | **Customize → Rules** (синхронизация аккаунта Cursor между устройствами)        | Все проекты                  | Свободный текст (тон, конвенции)                                                     |
| **User rule files** | `~/.cursor/rules` (Windows: `%USERPROFILE%\.cursor\rules`)                      | Только машина — **без sync** | Как user rules, но локальные файлы                                                   |
| **Team Rules**      | [Team dashboard](https://cursor.com/dashboard/team-content) (Team / Enterprise) | Организация                  | Свободный текст; опциональные globs; **Enforced** или optional                       |
| **AGENTS.md**       | Корень и/или вложенные каталоги                                                 | Дерево каталогов             | Plain markdown, без frontmatter                                                      |

**Storage notes:** User rules в Settings синхронизируются при входе на другой машине. User rule files и team rules **не** входят в profile exports.

**When to use:**

| Choose…                    | When…                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| **Project Rules** (`.mdc`) | Конвенции репозитория для команды; нужны globs, always-on, intelligent или `@mention`.       |
| **User Rules**             | Личный тон или привычки во всех репо — не team policy.                                       |
| **Team Rules**             | Стандарты организации / compliance; **Enforce**, если нельзя отключать.                      |
| **AGENTS.md**              | Простые читаемые инструкции без frontmatter; nested package notes. Для apply modes — `.mdc`. |

**Common pitfalls:** Plain `.md` в `.cursor/rules/` (игнорируется — нужен `.mdc`); ожидание User Rules для Tab / Cmd+K; multi-step deploy в rules вместо [skill](./0_ai_cursor_skills.md); дублирование style guide в always-on rules.

**Pluses and minuses:**

| +                                                    | −                                                       |
| ---------------------------------------------------- | ------------------------------------------------------- |
| Постоянные инструкции без повторного paste промптов. | Только Agent Chat — Tab / Inline Edit игнорируют rules. |
| Четыре слоя: личный, проект, org, простой markdown.  | Слишком много always-on rules съедают контекст.         |
| Project `.mdc` + Team Rules шарятся и enforce.       | User rules для team policy → рассинхрон в команде.      |

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

Создать project rule: **Cmd/Ctrl+Shift+P** → “New Cursor Rule”, или `/create-rule` в чате, или Customize → Rules → Add Rule.

**Thoughts:** Бери **самый узкий** источник: тон — User, конвенции репо — Project, org policy — Team, «просто markdown» — AGENTS.md.

**Sources:** [Rules help](https://cursor.com/help/customization/rules) · [Rules reference](https://cursor.com/docs/rules)

</details>

## Apply modes

<details>
<summary>Apply modes</summary>

**Definition:** У каждого project rule frontmatter (`alwaysApply`, `description`, `globs`) — задаётся через **type dropdown** при создании/редактировании — решает, когда правило попадает в контекст Agent.

**How it works:**

| UI type                     | Frontmatter behavior                                                        |
| --------------------------- | --------------------------------------------------------------------------- |
| **Always Apply**            | `alwaysApply: true` — каждый чат; globs/description игнорируются            |
| **Apply to Specific Files** | `alwaysApply: false` + `globs` — когда matching file в контексте            |
| **Apply Intelligently**     | `alwaysApply: false` + `description`, без globs — Agent решает по relevance |
| **Apply Manually**          | `alwaysApply: false`, без description и globs — только `@rule-name` в чате  |

**When to use:** Always Apply для жёсткой безопасности. Globs для React/API папок. Intelligent для редких domain tips (чёткий `description`). Manual для чеклистов по `@mention`.

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

**Definition:** Project rules — файлы `.mdc` в `.cursor/rules` с YAML frontmatter и markdown-телом. Plain `.md` в этой папке **игнорируется** (нет frontmatter).

**How it works:**

- **Create:** Cmd/Ctrl+Shift+P → “New Cursor Rule”, `/create-rule` в чате, или Customize → Rules → Add Rule.
- **Organize:** держи rules в `.cursor/rules/`; вложенные подпапки работают, но **плоская** структура с понятными именами (`react-patterns.mdc`, `api-validation.mdc`) проще.
- **Same filename, different folders:** Cursor идентифицирует rule по **полному пути** — два `components.mdc` в разных папках оба применяются, если условия совпали; имя файла само по себе не override.
- **Best practices:** до ~500 строк; дроби большие темы; `@filename.ts` вместо paste целых гайдов; добавляй rules, когда Agent повторяет одну ошибку; коммить в git для команды.
- **Avoid:** копировать целые style guides (линтер), редкие edge cases, дублировать код из репозитория.

**When to use:** Domain knowledge, style decisions и конвенции, которыми делится вся команда.

**Pluses and minuses:**

| +                                            | −                                        |
| -------------------------------------------- | ---------------------------------------- |
| Version control; стабильное поведение Agent. | Широкие always-on rules тратят контекст. |
| Globs локализуют guidance по папке.          | Расширение `.md` молча не работает.      |

**Sources:** [Rules help — organize rule files](https://cursor.com/help/customization/rules) · [Project rules](https://cursor.com/docs/rules#project-rules)

</details>

## AGENTS.md

<details>
<summary>AGENTS.md</summary>

**Definition:** `AGENTS.md` — plain markdown с инструкциями для агента в корне проекта (или во вложенных каталогах) — простая альтернатива `.cursor/rules`.

**How it works:**

- Cursor подхватывает автоматически; без frontmatter и apply-mode dropdown.
- Вложенные `AGENTS.md` действуют при работе под этим каталогом; более конкретные инструкции перекрывают родительские.
- Для условного применения (globs, intelligent, manual `@mention`) — project rules в `.cursor/rules/`.

**When to use:** Небольшие проекты или package-level notes, где хватает читаемого markdown.

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

**Definition:** `CLAUDE.md` Cursor читает так же, как `AGENTS.md` — plain markdown с инструкциями, обычно в корне проекта.

**How it works:**

- Положи `CLAUDE.md` в корень (nested support как у `AGENTS.md`).
- **Всегда применяется к каждому разговору**, независимо от `alwaysApply` во frontmatter — для совместимости с Claude Code.
- Нужны условные rules? Используй `.cursor/rules/*.mdc` с apply modes.

**When to use:** Репозитории, общие для Cursor и Claude Code, или уже есть `CLAUDE.md` playbook.

**Thoughts:** Считай `CLAUDE.md` «always-on AGENTS.md»; держи коротким или выноси детали в scoped `.mdc` rules.

**Sources:** [Rules help — CLAUDE.md](https://cursor.com/help/customization/rules)

</details>

## Team Rules

<details>
<summary>Team Rules</summary>

**Definition:** Планы Team и [Enterprise](https://cursor.com/docs/enterprise) создают и управляют org-wide rules из [Cursor dashboard](https://cursor.com/dashboard/team-content).

**How it works:**

- Свободный текст (не layout `.cursor/rules`); опциональные **globs** (напр. `**/*.py`) ограничивают rule matching-файлами.
- **Enforced rules** — обязательны для всех; нельзя отключить в Customize.
- **Optional rules** — по умолчанию включены; участники могут отключить в **Customize → Rules → Team Rules**.
- Импорт rules из GitHub только через marketplace **plugin** (сами rules не импортируются) — см. [Plugins](./0_ai_cursor_plugins.md).

**When to use:** Стандарты кодирования и compliance. AI rules — не единственный security control.

**Sources:** [Rules help — team rules](https://cursor.com/help/customization/rules) · [Team Rules](https://cursor.com/docs/rules#team-rules)

</details>

## Precedence

<details>
<summary>Precedence</summary>

**Definition:** При конфликте Cursor мержит applicable rules в порядке: **Team Rules → Project Rules → User Rules** (раньше в списке — сильнее).

**How it works:**

- Все matching rules включаются; Team перекрывает Project, Project — User.
- Rules не действуют на Tab, Inline Edit и Bugbot PR reviews.

**Thoughts:** Необратимую безопасность — в enforced Team или always-apply Project rules; личный тон — в User Rules.

**Sources:** [Rules help — team rules](https://cursor.com/help/customization/rules) · [Team Rules — precedence](https://cursor.com/docs/rules#format-and-how-team-rules-are-applied)

</details>

## Context optimization

<details>
<summary>Context optimization</summary>

**Definition:** Rules и [skills](./0_ai_cursor_skills.md) делят одно context window Agent. Оптимизация — **разделить роли**: rules = короткие constraints при matching files; skills = multi-step workflows по demand.

**How it works:**

| Tactic        | Rule                                                                          | Skill                                               |
| ------------- | ----------------------------------------------------------------------------- | --------------------------------------------------- |
| **Size**      | Цель &lt;50 строк; split выше ~500                                            | Короткий `SKILL.md`; глубина в `references/`        |
| **Scope**     | `globs` или узкий `description`                                               | `paths` + sharp `description` (WHAT + WHEN)         |
| **Overlap**   | Один concern на rule — не дублируй checklist skill                            | Shape → rules; workflow → skill                     |
| **Always-on** | Только safety/compliance                                                      | Custom Mode или `/skill-name` для длинных playbooks |
| **Migrate**   | Intelligent rule + steps → [skill](./0_ai_cursor_skills.md#migrate-to-skills) | `disable-model-invocation: true` для slash-only     |

**When to use:** Agent медленный или игнорирует guidance — сначала audit always-on rules, потом duplicate rule+skill. Повторяющиеся intelligent rules — `/migrate-to-skills`.

**Examples (MyConspect):**

| Layer | File                                             | Loads when                                    |
| ----- | ------------------------------------------------ | --------------------------------------------- |
| Rule  | `study-notes-structure.mdc`                      | Intelligent — conspect edit/structure prompts |
| Rule  | `locale-sync.mdc`, `resources-official-docs.mdc` | Intelligent — RU sync / fact edits            |
| Skill | `update-conspect`                                | Intelligent — conspect edit prompts           |
| Skill | `sync-locales`                                   | Intelligent — translate / EN↔RU sync          |

**Thoughts:** Один и тот же 10-step checklist каждую неделю — skill, не три always-on rules.

**Sources:** [Rules — best practices](https://cursor.com/docs/rules) · [Skills — progressive disclosure](https://cursor.com/docs/skills.md#optional-directories) · [Skills vs rules](https://cursor.com/help/customization/skills#when-should-i-use-skills-instead-of-rules)

</details>

## Migrating from .cursorrules

<details>
<summary>Migrating from .cursorrules</summary>

**Definition:** Файл `.cursorrules` в корне проекта — **legacy**, будет deprecated. Мигрируй в project rule в `.cursor/rules/`.

**How it works:**

1. Cmd/Ctrl+Shift+P → “New Cursor Rule” (или `/create-rule` в чате).
2. Скопируй содержимое `.cursorrules` в новый `.mdc`.
3. Тип **Always Apply** (как старое always-on поведение).
4. Удали `.cursorrules` из корня.

**When to use:** Как только видишь `.cursorrules` в поддерживаемом репо — до снятия поддержки в Cursor.

**Sources:** [Rules help — migrate from .cursorrules](https://cursor.com/help/customization/rules)

</details>

## Rule troubleshooting

<details>
<summary>Rule troubleshooting</summary>

**Definition:** Быстрые проверки, если rule не влияет на поведение Agent.

**How it works:**

- Убедись, что ты в **Agent (Chat)** — rules не для Tab, Inline Edit и Bugbot.
- **Apply Intelligently:** добавь чёткий `description`, когда rule уместен.
- **Apply to Specific Files:** проверь, что `globs` совпадают с файлами в контексте (напр. `src/components/**/*.tsx`).
- **Apply Manually:** `@mention` имени rule в чате.
- Два rule с **одинаковым именем файла** в разных папках оба применяются по полному пути — это не collision по имени.

**When to use:** После создания или правки rule, если Agent всё ещё его игнорирует.

**Sources:** [Rules help — rule doesn't apply](https://cursor.com/help/customization/rules)

</details>

## Interview Practice

Q&A moved to [Cursor — Questions](./0_ai_cursor_questions.md).
