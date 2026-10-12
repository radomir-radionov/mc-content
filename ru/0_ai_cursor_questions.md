# Cursor — Questions

Вопросы и ответы по навыкам использования курсора, правилам, плагинам и командам слэша.

### Related docs

- [Core](./0_ai_cursor_core.md) · [Навыки](./0_ai_cursor_skills.md) · [Правила](./0_ai_cursor_rules.md) · [Плагины](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Слэш-команды](./0_ai_cursor_slash.md) · [Рабочий процесс AI](./0_ai.md)

### Sections

- [Questions & Answers](#questions-answers)

---

## Questions & Answers

<details>
<summary>Skills vs rules — what’s the difference, and when to use each?</summary>

**Definition:** **Правила** — это короткие постоянные рекомендации, внедренные в контекст агента (всегда/глобальные/интеллектуальные/интеллектуальные). `@mention`). **Навыки** – это пошаговые инструкции (`SKILL.md` + дополнительные скрипты/ссылки), вызываемые с помощью `/`, выбрано автоматически `description`или сохранен в качестве пользовательского режима.

**How it works:**

| | **Правила**    | **Навыки**                                     |
| | --------------- | ---------------------------------------------- | ------------------------------------------------------ |
| | **Цель**       | Ограничения и соглашения по кодированию        | Повторяемые процедуры                                                                    |
| | **Форма**      | `.mdc` /Пользователь/Команда/ `AGENTS.md`      | Папка + `SKILL.md` (+ скрипты/ссылки)                                                    |
| | **Применение** | Всегда /шарики /умные/ `@mention`              | Авто по `description`, `/skill-name`, `@`, Пользовательский режим                        |
| | **Пример**     | «Используйте TypeScript для всех новых файлов» | «Развертывание в промежуточном режиме: тестирование → сборка → развертывание → проверка» |

**When to use:**

- **Правило** — стиль, архитектура, безопасность («никогда не раскрывайте секреты»), все, что должно соблюдаться всегда или для сопоставления файлов.
- **Навыки** — контрольные списки, которым агент должен следовать шаг за шагом (развертывание, проверка, обновление), особенно при работе со сценариями.

**Thoughts:** Правила = «всегда/при сопоставлении файлов». Навыки = «как выполнять эту работу». Полные примечания: [Что такое навыки](./0_ai_cursor_skills.md#what-are-skills) · [Четыре источника правил](./0_ai_cursor_rules.md#four-rule-sources).

**Sources:** [Помощь по навыкам — против правил](https://cursor.com/help/customization/skills#when-should-i-use-skills-instead-of-rules) · [Правила](https://cursor.com/docs/rules) · [Навыки](https://cursor.com/docs/skills.md)

</details>

<details>
<summary>What are the four rule sources?</summary>

**Definition:** Курсор загружает правила из **Правил проекта** (`.cursor/rules/*.mdc`), **Правила пользователя** (Настройка → Правила), **Правила группы** (панель управления, Команда/Предприятие) и простой **AGENTS.md**.

**How it works:** В конфликте побеждает более ранний: **Команда → Проект → Пользователь**. Все применимые правила объединяются в контекст агента. Правила применяются только к агенту (чату), а не к Tab или Cmd/Ctrl+K.

**Thoughts:** Полные примечания: [Четыре источника правил](./0_ai_cursor_rules.md#four-rule-sources).

**Sources:** [Правила](https://cursor.com/docs/rules)

</details>

<details>
<summary>Why is my `.cursor/rules/foo.md` ignored?</summary>

**Definition:** Правила проекта должны быть **`.mdc`** файлы с оформлением YAML (`description` / `globs` / `alwaysApply`). Равнина `.md` in `.cursor/rules/` игнорируется системой правил.

**When to use:** Предпочитать `AGENTS.md` для простых инструкций по уценке или переименуйте в `.mdc` и установите режим применения.

**Sources:** [Правила проекта](https://cursor.com/docs/rules#project-rules)

</details>

<details>
<summary>Do User Rules affect Tab completion?</summary>

**Definition:** Нет. Правила (включая правила пользователя) применяются только к **Агенту (Чат)**, а не к вкладке курсора, встроенному редактированию (Cmd/Ctrl+K) или Bugbot.

**Sources:** [Часто задаваемые вопросы по правилам](https://cursor.com/docs/rules#do-rules-impact-cursor-tab-or-other-ai-features)

</details>

<details>
<summary>Where do project skills live?</summary>

**Definition:** Навыки проекта живут в `.cursor/skills/<name>/SKILL.md` or `.agents/skills/<name>/SKILL.md` (также вложенные пути пакетов, такие как `apps/web/.cursor/skills/`).

**How it works:** Личные навыки имеют значение `~/.cursor/skills/` (дополнительная синхронизация облачных агентов). Имя навыка — это папка, содержащая `SKILL.md`.

**Sources:** [Каталоги навыков](https://cursor.com/docs/skills.md#skill-directories)

</details>

<details>
<summary>Agent Plugin vs Cursor Plugin?</summary>

**Definition:** **Плагин агента** = root `plugin.json`, только портативные навыки + MCP. **Плагин курсора** = `.cursor-plugin/plugin.json` плюс правила, агенты, команды, перехватчики и переменные.

**Thoughts:** Полные примечания: [Плагины](./0_ai_cursor_plugins.md).

**Sources:** [Плагины](https://cursor.com/docs/plugins)

</details>

<details>
<summary>Can I import only rules from a GitHub repo?</summary>

**Definition:** Нет — упакуйте их в **плагин** с манифестом торговой площадки (`.cursor-plugin/marketplace.json`), затем установите плагин. Правила/навыки приходят вместе с плагином.

**Sources:** [Импорт правил из репозитория](https://cursor.com/docs/rules#importing-rules-from-a-repository)

</details>

<details>
<summary>Where do custom `/foo` commands live?</summary>

**Definition:** Команды проекта: `.cursor/commands/foo.md`. Пользовательские команды: `~/.cursor/commands/foo.md`. Предпочитайте **навыки** для новой многоэтапной работы; мигрировать с `/migrate-to-skills`.

**Sources:** [Слэш-команды](./0_ai_cursor_slash.md) · [Перейти к навыкам](https://cursor.com/docs/skills.md#migrating-rules-and-commands-to-skills)

</details>

<details>
<summary>Name five high-value built-in slash entries</summary>

**Definition:** Общие встроенные модули: `/create-rule`, `/create-skill`, `/review`, `/loop`, `/migrate-to-skills` (плюс `/summarize` для долгих разговоров).

**Thoughts:** Полный список: [Встроенные навыки](./0_ai_cursor_skills.md#built-in-skills) · [Слэш-команды](./0_ai_cursor_slash.md).

**Sources:** [Встроенные навыки курсора](https://cursor.com/docs/skills.md#built-in-cursor-skills)

</details>
