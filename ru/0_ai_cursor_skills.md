# Cursor — Skills

Многоэтапные рабочие процессы многократного использования, которые учат агента выполнять конкретную задачу. Более подробный, чем правила; вызывается с `/` или применяется автоматически, когда это необходимо.

### Related docs

- [Рабочий процесс AI](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Правила](./0_ai_cursor_rules.md) · [Плагины](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash-команды](./0_ai_cursor_slash.md) · [Вопросы](./0_ai_cursor_questions.md)

### Main concepts

| Площадь             | Темы                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Основы**          | [Что такое навыки](#what-are-skills), [Каталоги](#skill-directories), [SKILL.md](#skillmd-format), [Как использовать](#how-to-use-a-skill) |
| **Управление**      | [Автоматический vs slash](#automatic-vs-slash-invocation), [область paths](#scoping-with-paths), [Custom Mode](#custom-mode)               |
| **Распространение** | [Cloud sync](#cloud-agents-sync), [Publish в команду](#publish-to-team), [Просмотр и установка](#viewing-and-installing-skills)            |
| **Операции**        | [Встроенные навыки](#built-in-skills), [Конвертация rule](#convert-rule-to-skill), [Миграция rules/commands](#migrate-to-skills)           |

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

**Definition:** **Навык** — это переносимый пакет с контролем версий, который учит агента выполнять задачи, специфичные для предметной области. Это папка с необходимым `SKILL.md` (вставка YAML + инструкции по уценке) и необязательно `scripts/`, `references/`, и `assets/` этот Агент может действовать с помощью своих инструментов. Навыки более детализированы, чем [rules](./0_ai_cursor_rules.md), и созданы для многоэтапных повторяемых рабочих процессов.

**How it works:**

| Pillar                 | Meaning                                                                   |
| ---------------------- | ------------------------------------------------------------------------- |
| **Portable**           | Работает на любом agent с [agentskills.io](https://agentskills.io)        |
| **Version-controlled** | Файлы в git — project в `.cursor/skills/`, personal в `~/.cursor/skills/` |
| **Actionable**         | Опциональный `scripts/` для запуска Agent; шаблоны в `assets/`            |
| **Progressive**        | Глубина в `references/` — подгрузка по demand, не каждый turn             |

- **Discovery:** при старте Cursor сканирует skill directories (project + user; Claude/Codex paths) и находит папки с `SKILL.md`. См. [Skill directories](#skill-directories).
- **Relevance:** Agent выбирает skill, когда запрос совпадает с `description` (WHAT + WHEN). Расплывчатое описание → редкий auto-pick.
- **Manual invoke:** `/` в чате или `@` attach — skill на **одно** сообщение; для сессии — [Custom Mode](#custom-mode) (Option+Enter / Alt+Enter).
- **Progressive:** держи `SKILL.md` коротким; глубину — в `references/`, шаблоны — в `assets/`.
- **Actionable:** относительные пути к `scripts/` — Bash, Python, JS и т.д.
- **Built-ins:** [встроенные skills](#built-in-skills) (`/create-skill`, `/review`, …) рядом с твоими.

**When to use:**

| | **Rules**   | **Skills**                         |
| | ----------- | ---------------------------------- | ------------------------------------------------------------------ |
| | Purpose     | Короткие guidelines и ограничения  | Многошаговые workflows и процедуры                                 |
| | Length      | От нескольких строк до сотен       | Часто длиннее, с пошаговыми инструкциями                           |
| | How applied | В каждом (или matching) чате       | Явно: `/skill-name`, `@skill-name`; или auto-pick по `description` |
| | Example     | «TypeScript для всех новых файлов» | «Deploy to staging: tests, build, deploy, verify»                  |

**Rule** — когда хватает короткой инструкции. **Skill** — когда Agent нужен детальный повторяемый процесс (deploy, audit, conspect, PR).

**Common pitfalls:** «helps with docs» в `description` (слишком vague); роман в `SKILL.md` вместо `references/`; skill для style, который belongs в [rule](./0_ai_cursor_rules.md); дублирование checklist в rule **и** skill — см. [Context optimization](./0_ai_cursor_rules.md#context-optimization).

**Pluses and minuses:**

| +                                                            | −                                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------------- |
| Общедоступные, отслеживаемые git плейбуки для команды.       | Плохой `description` → никогда не применяется автоматически.        |
| Скрипты+ссылки+ассеты, загружаются постепенно.               | Долгие постоянные навыки сжигают контекст.                          |
| Работает с `/`, автовыбор, пользовательский режим и плагины. | Правила пересекаются, если вы смешиваете «всегда стиль» с навыками. |

**Examples:**

Минимальный навык (папка `.cursor/skills/deploy-staging/SKILL.md`):

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

Навык со скриптами (Агент следует по относительным путям от корня навыка):

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

Создайте его в интерактивном режиме: введите `/create-skill` и опишите рабочий процесс — имя, структура и место сохранения встроенных в Cursor шаблонов навыков.

**Thoughts:** Правила = «всегда/при сопоставлении файлов». Навыки = «как выполнять эту работу». Если вы будете вставлять в чат один и тот же контрольный список каждую неделю, сделайте это навыком.

**Sources:** [Навыки агента](https://cursor.com/docs/skills.md) · [Что такое навыки?](https://cursor.com/docs/skills.md#what-are-skills) · [Помощь по навыкам](https://cursor.com/help/customization/skills) · [agentskills.io](https://agentskills.io)

</details>

## Skill directories

<details>
<summary>Skill directories</summary>

**Definition:** Курсор загружает навыки из папок проекта и пользователя; каждый навык представляет собой каталог, содержащий `SKILL.md`.

**How it works:**

| Расположение        | Область применения                                                      |
| ------------------- | ----------------------------------------------------------------------- |
| `.agents/skills/`   | Проект                                                                  |
| `.cursor/skills/`   | Проект                                                                  |
| `~/.agents/skills/` | Пользователь (локальный компьютер)                                      |
| `~/.cursor/skills/` | Пользователь (локальный; может синхронизироваться с облачными агентами) |

- Также обнаруживает каталоги Claude/Codex: `.claude/skills/`, `.codex/skills/` и варианты в `~/`.
- Cursor обходит каждый skills root **рекурсивно** — категорийные подпапки OK (например `.cursor/skills/shipping/deploy-staging/SKILL.md`). **Имя** skill — папка с `SKILL.md`, не родительская категория.
- Monorepo: любой вложенный `.cursor/skills/` или `.agents/skills/` в репо подхватывается (например `apps/web/.cursor/skills/`). Skills там **auto-scoped** к файлам этой директории — как `paths`, без extra frontmatter.
- `~/.agents/skills/` и несинхронизированный `~/.cursor/skills/` остаются на машине; **не** копируются в Cloud Agents, Agents Window remote SSH или self-hosted workers (используй repo skills или bake в worker image).

**When to use:** Командные workflows — в `.cursor/skills/` (commit). Личные привычки — в `~/.cursor/skills/` (опционально sync для Cloud Agents — см. [Cloud Agents sync](#cloud-agents-sync)).

**Sources:** [Каталоги навыков](https://cursor.com/docs/skills.md#skill-directories)

</details>

## SKILL.md format

<details>
<summary>SKILL.md format</summary>

**Definition:** Каждый навык – это `SKILL.md` файл с заголовком YAML (`name`, `description`) и инструкции по уценке.

**How it works:**

| Field                      | Required | Role                                                                   |
| -------------------------- | -------- | ---------------------------------------------------------------------- |
| `name`                     | Yes      | Kebab-case id; **должен совпадать** с именем родительской папки        |
| `description`              | Yes      | WHAT + WHEN — auto-relevance                                           |
| `paths`                    | No       | Glob scope (list или comma-separated string)                           |
| `disable-model-invocation` | No       | `true` → только slash, без auto-pick                                   |
| `icon` / `color`           | No       | Badge Custom Mode (`code`, `beaker`, `shield`, … / `green`, `blue`, …) |
| `metadata`                 | No       | Произвольная key-value map                                             |

Optional folders рядом с `SKILL.md`:

| Dir           | Purpose                           |
| ------------- | --------------------------------- |
| `scripts/`    | Исполняемый код для Agent         |
| `references/` | Доп. docs по demand               |
| `assets/`     | Шаблоны, изображения, static data |

Body: markdown — часто `# Title`, `## When to Use`, `## Instructions`. Глубину — в `references/`.

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

**Sources:** [Формат файла SKILL.md](https://cursor.com/docs/skills.md#skillmd-file-format)

</details>

## How to use a skill

<details>
<summary>How to use a skill</summary>

**Definition:** Запуск skill — `/` + имя в чате Agent или attach через `@`; Agent читает `SKILL.md` и следует инструкциям.

**How it works:**

- **`/` invoke:** `/write-tests` (или из меню). Прикрепляет skill к **одному** сообщению, пока не переведёшь в [Custom Mode](#custom-mode).
- **`@` attach:** `@` → выбери skill как контекст для текущего turn.
- **Auto-pick:** При включённом model invocation (default) Agent может применить skill, если запрос совпадает с `description`.

**When to use:** `/` или `@` для явного запуска; auto-pick — когда domain playbook должен подхватываться без ввода имени каждый раз.

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

**Definition:** По умолчанию агент может применить навык, если запрос соответствует его описанию; с `disable-model-invocation: true` он запускается только тогда, когда вы печатаете `/skill-name`.

**How it works:**

- По умолчанию: вызов модели включен — навык ведет себя как умная книга действий.
- `disable-model-invocation: true` — классическое поведение слэш-команды (только явное).
- Ввод текста `/` прикрепляет навык к **одному** сообщению, если вы не переведете его в пользовательский режим.

**When to use:** Автоматически для плейбуков домена, которые агент должен получить. Slash-только для дорогостоящих или опасных рабочих процессов, на которые вы должны согласиться.

**Sources:** [Отключение автоматического вызова](https://cursor.com/docs/skills.md#disabling-automatic-infection)

</details>

## Scoping with paths

<details>
<summary>Scoping with paths</summary>

**Definition:** The `paths` Поле frontmatter ограничивает отображение навыка файлами, соответствующими шаблонам glob.

**How it works:**

- Принимает список или строку переменных, разделенных запятыми.
- Сброшено `paths` → доступно независимо от открытых файлов.
- Наследие `globs` все еще работает; новые навыки следует использовать `paths`.

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

**Sources:** [Охват навыка конкретными файлами](https://cursor.com/docs/skills.md#scoping-a-skill-to-specified-files)

</details>

## Custom Mode

<details>
<summary>Custom Mode</summary>

**Definition:** Пользовательский режим сохраняет навык в контексте на протяжении всего сеанса (значок во входных данных в чате), а не прикрепляет его к одному сообщению.

**How it works:**

- Выберите skill из `/`, затем Option+Enter (Mac) / Alt+Enter (Windows) или **Use as Mode**.
- Опционально `icon` (`code`, `terminal`, `bug`, `git-branch`, `book-open`, `beaker`, `shield`, `rocket`, …) и `color` (`default`, `green`, `cyan`, `blue`, `purple`, `magenta`, `orange`, `yellow`, `red`, `brand`). Неизвестные значения → default lightning badge.
- Доступно в Agents Window и CLI.

**When to use:** Длительные сеансы, которые должны оставаться в одном playbook (TDD, контрольный список проверки кода). Пропустите одноразовые задачи.

**Sources:** [Использование навыка в качестве пользовательского режима](https://cursor.com/docs/skills.md#using-a-skill-as-a-custom-mode) · [Агенты подсказок](https://cursor.com/docs/agent/prompting.md)

</details>

## Cloud Agents sync

<details>
<summary>Cloud Agents sync</summary>

**Definition:** Личные skills в `~/.cursor/skills/` остаются на машине, пока не включишь **Sync Skills for Cloud Agents** — тогда Cloud Agents используют те же private playbooks.

**How it works:**

1. **Settings → Agents**.
2. **Context and Tools** → включи **Sync Skills for Cloud Agents** (можно начать из **Customize → Skills**).
3. Cursor копирует `~/.cursor/skills/` для Cloud Agents. Synced skills **private** — не shared с командой.
4. Выключи sync — skills вернутся на машину.
5. Sync идёт **только** для `~/.cursor/skills/` — не project skills, не `~/.agents/skills/`, не прочие local files.
6. **Teams / Enterprise:** admins могут отключить sync для всех в **Team Settings → Security & Identity**.

**When to use:** Личные skills локально и те же workflows на Cloud Agents. Чтобы поделиться с командой — [Publish to team](#publish-to-team), не sync.

**Sources:** [Use personal skills with Cloud Agents](https://cursor.com/docs/skills.md#use-personal-skills-with-cloud-agents) · [Skills help — Cloud Agents](https://cursor.com/help/customization/skills#how-do-i-use-personal-skills-with-cloud-agents)

</details>

## Publish to team

<details>
<summary>Publish to team</summary>

**Definition:** **Publish** кладёт personal skill в **Default marketplace** команды; teammates устанавливают его. Publish ≠ [Cloud sync](#cloud-agents-sync): sync private, publish — team-wide.

**How it works:**

1. **Customize → Skills**.
2. Открой personal skill.
3. **Publish** — skill попадает в Default marketplace команды.
4. Teammates ставят из marketplace; Cursor хранит копию в repo, hosted для команды.

**When to use:** Workflow должен быть стандартом для всей команды. One-off личные привычки — `~/.cursor/skills/` + sync.

**Sources:** [Publish a skill to your team](https://cursor.com/help/customization/skills#how-do-i-publish-a-skill-to-my-team)

</details>

## Viewing and installing skills

<details>
<summary>Viewing and installing skills</summary>

**Definition:** **Customize → Skills** показывает обнаруженные skills (project, personal, plugin). Project skills также в **Agent Decides** рядом с rules.

**How it works:**

- **Browse:** Sidebar **Customize → Skills** — что Cursor загрузил из [skill directories](#skill-directories).
- **From GitHub:** Skills **не** импортируются отдельно. Упакуй в [plugin](./0_ai_cursor_plugins.md) с `.cursor-plugin/marketplace.json`, import через **From GitHub Repository** или team marketplace, затем install plugin.
- **Personal → Cloud:** [Cloud sync](#cloud-agents-sync). **Personal → team:** [Publish to team](#publish-to-team).

**When to use:** Проверить, что Agent может auto-pick, если skill «не срабатывает». Из repo — через plugins.

**Sources:** [Viewing skills](https://cursor.com/docs/skills.md#viewing-skills) · [Installing skills from a repository](https://cursor.com/docs/skills.md#installing-skills-from-a-repository)

</details>

## Built-in skills

<details>
<summary>Built-in skills</summary>

**Definition:** Cursor ships встроенные skills в меню `/`. Agent может auto-use некоторые, когда запрос явно совпадает.

**How it works:**

| Skill                     | What it does                                        |
| ------------------------- | --------------------------------------------------- |
| `/automate`               | Cursor Automations (schedules, Slack, GitHub, …)    |
| `/autopilot`              | Мониторинг PR — feedback, conflicts, CI, follow-ups |
| `/canvas`                 | React artifact рядом с чатом                        |
| `/create-hook`            | Hooks + `hooks.json`                                |
| `/create-rule`            | Rules с корректным scope                            |
| `/create-skill`           | Scaffold `SKILL.md` и структуры папки               |
| `/create-subagent`        | Custom subagents с focused roles                    |
| `/cursor-blame`           | Trace AI-изменений и prompts                        |
| `/loop`                   | Повтор prompt/skill по interval                     |
| `/migrate-to-skills`      | Eligible rules/commands → skills                    |
| `/review`                 | Выбор и запуск code-review agent                    |
| `/review-bugbot`          | Bug/regression review                               |
| `/review-security`        | Security review                                     |
| `/sdk`                    | Cursor SDK integrations                             |
| `/shell`                  | Literal shell command                               |
| `/split-to-prs`           | Разбить большой diff на PRs                         |
| `/statusline`             | CLI status line config                              |
| `/update-cli-config`      | Edit `~/.cursor/cli-config.json`                    |
| `/update-cursor-settings` | Cursor / VS Code settings                           |

**When to use:** Built-ins для scaffolding (rules, skills, hooks) и reviews — не изобретай prompts. Список slash: [Slash commands](./0_ai_cursor_slash.md). Подробнее: [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md).

**Sources:** [Встроенные навыки курсора](https://cursor.com/docs/skills.md#built-in-cursor-skills)

</details>

## Convert rule to skill

<details>
<summary>Convert rule to skill</summary>

**Definition:** Превратить один project rule **Apply Intelligently** в skill, когда контент — multi-step playbook, а не короткое ограничение.

**How it works:**

1. `/create-skill` и попроси конвертировать rule (например «turn `@my-rule` into a skill»).
2. Проверь `.cursor/skills/<name>/SKILL.md`.
3. Удали `.cursor/rules/<name>.mdc`, если rule больше не нужен.

**When to use:** Один rule перерос always-on guidance и нужны scripts, references или явный `/`. Bulk rules **и** slash commands — [Migrate to skills](#migrate-to-skills).

**Sources:** [Convert a rule into a skill](https://cursor.com/help/customization/skills#how-do-i-convert-a-rule-into-a-skill)

</details>

## Migrate to skills

<details>
<summary>Migrate to skills</summary>

**Definition:** `/migrate-to-skills` (Курсор 2.4+) преобразует подходящие динамические правила и косые команды в навыки агента.

**How it works:**

- Преобразует правила «Применять разумно» (`alwaysApply: false` / не установлено, нет `globs`) → обычные навыки.
- Преобразует пользователя/рабочее пространство `.cursor/commands` → навыки работы с `disable-model-invocation: true`.
- **не** мигрирует `alwaysApply: true`, глобальные правила или пользовательские правила (не на диске).

**Examples:**

```text
# In Agent chat
/migrate-to-skills
```

**Sources:** [Перенос правил и команд в навыки](https://cursor.com/docs/skills.md#migrating-rules-and-commands-to-skills)

</details>

## Interview Practice

Вопросы и ответы перемещены в [Курсор — Вопросы](./0_ai_cursor_questions.md).
