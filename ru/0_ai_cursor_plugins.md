# Cursor — Plugins

Распространяемые пакеты, в которые входят навыки, правила, агенты, команды, перехватчики и серверы MCP для Cursor (и переносимые плагины агентов для навыков + MCP).

### Related docs

- [Рабочий процесс AI](./0_ai.md) · [Core](./0_ai_cursor_core.md) · [Навыки](./0_ai_cursor_skills.md) · [Правила](./0_ai_cursor_rules.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash-команды](./0_ai_cursor_slash.md) · [Вопросы](./0_ai_cursor_questions.md)

### Main concepts

| Площадь        | Темы                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------ |
| **Форматы**    | [Два формата плагина](#two-plugin-formats), [Манифест](#cursor-plugin-manifest)            |
| **Содержание** | [Обнаружение компонентов](#comComponent-discovery), [MCP и переменные](#mcp-and-variables) |
| **Поделиться** | [Торговая площадка и установка](#торговая площадка-и-установка)                            |

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

**Definition:** Cursor загружает плагины либо как **Плагины агента** (открытый стандарт), либо как **Плагины Cursor** (полный набор компонентов Cursor), определяемые по месту расположения манифеста.

**How it works:**

| Формат           | Манифест                      | Компоненты                                                      |
| ---------------- | ----------------------------- | --------------------------------------------------------------- |
| Плагины агентов  | `plugin.json` в корне плагина | Навыки, серверы MCP                                             |
| Плагины курсоров | `.cursor-plugin/plugin.json`  | Навыки, MCP, правила, агенты, команды, перехватчики, переменные |

**When to use:** Плагин агента, когда вам нужны только портативные навыки/MCP. Плагин курсора, когда вы также отправляете правила, перехватчики или команды слэша.

**Sources:** [Справочник по плагинам](https://cursor.com/docs/reference/plugins)

</details>

## Cursor Plugin manifest

<details>
<summary>Cursor Plugin manifest</summary>

**Definition:** Для каждого плагина курсора требуется `.cursor-plugin/plugin.json` хотя бы с чехлом для шашлыка `name`.

**How it works:**

- Необходимый: `name` (строчные буквы для кебаба, буквы и цифры/дефисы/точки).
- Необязательные метаданные: `description`, `version`, `author`, `homepage`, `repository`, `license`, `keywords`, `logo`.
- Необязательные переопределения пути: `rules`, `skills`, `agents`, `commands`, `hooks`, `mcpServers`, `variables`.
- Типовая планировка: `rules/`, `skills/*/SKILL.md`, `agents/`, `commands/`, `hooks/hooks.json`, `mcp.json`, `assets/`.

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

**Sources:** [Манифест плагина курсора](https://cursor.com/docs/reference/plugins#cursor-plugin-manifest)

</details>

## Component discovery

<details>
<summary>Component discovery</summary>

**Definition:** Если в манифесте не указан путь к компоненту, Cursor обнаруживает стандартные папки в корне плагина.

**How it works:**

| Компонент      | По умолчанию       | Открытие                                           |
| -------------- | ------------------ | -------------------------------------------------- |
| Навыки         | `skills/`          | Подкаталоги с `SKILL.md`                           |
| Правила        | `rules/`           | `.md` / `.mdc` / `.markdown`                       |
| Агенты         | `agents/`          | уценка / mdc                                       |
| Команды        | `commands/`        | уценка / текст                                     |
| Крючки         | `hooks/hooks.json` | Названия событий перехвата                         |
| МКП            | `mcp.json`         | Записи сервера                                     |
| Корневой навык | `SKILL.md` в корне | Только если нет `skills/` и нет манифеста `skills` |

- Явное поле манифеста (например. `"skills": "./my-skills/"`) **заменяет** обнаружение по умолчанию для этого типа.
- Использование репозиториев с несколькими плагинами `.cursor-plugin/marketplace.json` список каждого плагина `source` каталог.

**When to use:** Придерживайтесь значений по умолчанию, если вам не нужен нестандартный макет.

**Sources:** [Обнаружение компонентов](https://cursor.com/docs/reference/plugins#cursor-plugin-comComponent-discovery)

</details>

## MCP and variables

<details>
<summary>MCP and variables</summary>

**Definition:** Плагины могут отправлять серверы MCP через `mcp.json`. Секреты объявляются как `variables` в манифесте и заполненной информационной панели — никогда не фиксировалось.

**How it works:**

- Использовать `${VAR}` заполнители в `mcp.json`; объявить совпадающие имена под `variables` (Схема JSON) в `plugin.json`.
- Администраторы команды устанавливают значения в разделе «Плагины» → «Настроить».
- Курсор расширяется `${CURSOR_PLUGIN_ROOT}` / `${CLAUDE_PLUGIN_ROOT}` для пути установки в командах/args/env/cwd.
- Плагины агентов `${PLUGIN_ROOT}` / `${PLUGIN_DATA}` **не** раскрываются в курсоре — используйте `${CURSOR_PLUGIN_ROOT}`.

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

**Thoughts:** Конфигурация MCP, управляемая плагином, после установки доступна только для чтения на панели управления.

**Sources:** [Переменные](https://cursor.com/docs/reference/plugins#variables) · [серверы MCP](https://cursor.com/docs/reference/plugins#mcp-servers)

</details>

## Marketplace and install

<details>
<summary>Marketplace and install</summary>

**Definition:** Плагины устанавливаются с торговых площадок Cursor (официальные, командные репозитории или репозитории GitHub с `.cursor-plugin/marketplace.json`). Навыки/правила из репозитория поставляются **только** упакованными в плагин. Для community-обзора rules/MCP (не официальный publish path) см. [Cursor Directory](./0_ai_cursor_core.md#cursor-directory).

**How it works:**

- Настройте → импортируйте **Из репозитория GitHub** (для репозитория требуется файл marketplace.json) или добавьте торговую площадку группы.
- Публикуйте общедоступные плагины через [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).
- Установленные навыки/правила отображаются в настройке наряду с локальными.

**When to use:** Делитесь наборами команд (например, курсором-team-kit, плагинами Prisma, Vercel) вместо копирования. `.mdc` файлы вручную. Пропустите одноразовые личные навыки — используйте `~/.cursor/skills/`.

**Sources:** [Отправка плагина](https://cursor.com/docs/reference/plugins#submitting-a-plugin) · [Правила импорта](https://cursor.com/docs/rules#importing-rules-from-a-repository) · [cursor.directory](https://cursor.directory/)

</details>

## Interview Practice

Вопросы и ответы перемещены в [Курсор — Вопросы](./0_ai_cursor_questions.md).
