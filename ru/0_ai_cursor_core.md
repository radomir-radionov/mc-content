# Cursor — Core

Повседневные привычки агента: шорткаты, Run Modes, циклы проверки и community-конфиги. Глубже — [Skills](./0_ai_cursor_skills.md), [Rules](./0_ai_cursor_rules.md), [Plugins](./0_ai_cursor_plugins.md), [Subagents](./0_ai_cursor_subagents.md), [Hooks](./0_ai_cursor_hooks.md).

### Related docs

- [AI workflow](./0_ai.md) · [Context window](./0_ai.md#context-window-llm-context) · [Skills](./0_ai_cursor_skills.md) · [Rules](./0_ai_cursor_rules.md) · [Plugins](./0_ai_cursor_plugins.md) · [Subagents](./0_ai_cursor_subagents.md) · [Hooks](./0_ai_cursor_hooks.md) · [Slash commands](./0_ai_cursor_slash.md) · [Questions](./0_ai_cursor_questions.md)

### Main concepts

| Area          | Topics                                                                                   |
| ------------- | ---------------------------------------------------------------------------------------- |
| **Habits**    | [Everyday shortcuts](#everyday-shortcuts), [Agent autonomy](#agent-autonomy-run-modes)   |
| **Verify**    | [Test-driven agent loops](#test-driven-agent-loops), [Debug with logs](#debug-with-logs) |
| **Share**     | [Cursor Directory](#cursor-directory)                                                    |
| **Reference** | [Sources](#sources)                                                                      |

### Sections

- [Everyday shortcuts](#everyday-shortcuts)
- [Agent autonomy (Run Modes)](#agent-autonomy-run-modes)
- [Test-driven agent loops](#test-driven-agent-loops)
- [Debug with logs](#debug-with-logs)
- [Cursor Directory](#cursor-directory)
- [Sources](#sources)
- [Interview Practice](#interview-practice)

---

## Everyday shortcuts

<details>
<summary>Everyday shortcuts</summary>

**Definition:** Быстрые привычки Cursor для inline-правок, Agent chat, Tab, сообщений коммита в Source Control и опционального bug review — до полноценного multi-file Agent.

**How it works:**

- **Cmd/Ctrl+K (editor):** inline-правка по выделению. Когда изменение локальное и не нужны tools, MCP или длинный план.
- **Cmd/Ctrl+I:** открыть Agent (боковая панель). Выделение уходит в контекст. Для multi-file, shell и циклов проверки.
- **Cmd/Ctrl+K (terminal):** естественный язык → shell. Одноразовые git/list/filter без заучивания команд.
- **Tab:** принять серый ghost-text; второй Tab часто прыгает к следующей точке правки (отличается от обычного autocomplete).
- **Commit message:** Source Control → иконка «волшебной палочки» генерирует черновик по staged diff. Это старт, не финальная история.
- **Bug finder:** Command Palette → “bug finder” — сравнивает ветку с main на вероятные регрессии. Не замена тестам/CI.

**When to use:** Ежедневные микроправки и коммиты. Не гоняй Agent ради трёхстрочного rename, если хватает Cmd+K или Tab.

**Pluses and minuses:**

| +                                                  | −                                                                      |
| -------------------------------------------------- | ---------------------------------------------------------------------- |
| Быстрее цикл на локальных правках.                 | Правила/MCP агента часто иначе работают для Tab / Cmd+K.               |
| Commit wand и bug finder ловят слабые diff раньше. | Сгенерированные сообщения и bug finder — подсказки, их нужно ревьюить. |

**Thoughts:** Бери минимальную поверхность, которая закрывает задачу — Tab → Cmd+K → Agent — чтобы держать контекст и стоимость под контролем.

**Sources:** [Cursor Agent overview](https://cursor.com/docs/agent/overview) · [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Agent autonomy (Run Modes)

<details>
<summary>Agent autonomy (Run Modes)</summary>

**Definition:** **Run Modes** (Settings → Agents → Approvals & Execution) задают, сколько shell, MCP и Fetch агент может выполнить без спроса. В постах всё ещё пишут “YOLO”; ближе всего к этому **Run Everything**, а безопасный дефолт для длинных циклов — **Auto-review**.

**How it works:**

| Mode               | Behavior                                                                                                      |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| **Auto-review**    | Allowlist выполняется сразу; sandboxed shell когда можно; остальное — через classifier. Рекомендуемый дефолт. |
| **Allowlist**      | Без approval только перечисленные действия (sandbox опционален). Детерминированный набор доверенных команд.   |
| **Run Everything** | Каждый tool call без промптов. Максимальный риск; только если принимаешь полную автономию.                    |

- Направляй Auto-review через `autoRun.allow_instructions` / `block_instructions` в `~/.cursor/permissions.json` или `.cursor/permissions.json` (проект).
- Доп. защиты (browser, file-deletion, external-file) всё равно могут требовать approval.
- Cloud Agents **не** используют локальные Run Modes — они на изолированной машине.

**When to use:** Auto-review для циклов “гоняй tests / tsc / build до зелёного”. Allowlist — для узкого набора известных команд. Run Everything — только под присмотром. Не разрешай разрушительный git/network без ревью.

**Pluses and minuses:**

| +                                                            | −                                                     |
| ------------------------------------------------------------ | ----------------------------------------------------- |
| Агент итерирует по ошибкам без approve на каждый `npm test`. | Ошибки classifier; это не security boundary.          |
| Проектный `permissions.json` можно шарить с командой.        | Политика команды/моделей может отключить Auto-review. |

**Examples:**

Форма промпта, которая хорошо стыкуется с Auto-review / allowlist test+build:

```text
any kind of tests are always ok (vitest, npm test, nr test, …).
basic build commands (build, tsc) are ok. mkdir/touch are ok.
```

**Thoughts:** Старые гайды про “YOLO mode” описывают ту же идею — дать агенту verification-команды. Предпочитай Auto-review + явные allow/block, а не слепой Run Everything.

**Sources:** [Run Modes](https://cursor.com/docs/agent/security/run-modes) · [permissions.json](https://cursor.com/docs/reference/permissions) · [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Test-driven agent loops

<details>
<summary>Test-driven agent loops</summary>

**Definition:** Для нетривиальной работы агента проси **сначала тесты**, потом код, потом **гонять до зелёного** — корректность даёт suite, а не одноразовый generate.

**How it works:**

- Добавь строку в задачу: “Write tests first, then the code, then run the tests and update the code until tests pass.”
- С Auto-review (или allowlist с test/build) агент может итерировать без approve на каждый `npm test`.
- Расти устойчивость из реальных фейлов: вставь failing input/log → “write a test for this, then fix until the suite passes.”
- Закрывай сессию быстрым gate: “run `tsc` / lint / pre-PR until it passes” (держи в проекте `pre-PR`, похожий на CI, но быстрее).

**When to use:** Конвертеры, парсеры, сложные бизнес-правила — всё, что плохо one-shot. Пропускай чистый rename, когда TypeScript уже задаёт “done.”

**Pluses and minuses:**

| +                                                 | −                                                                  |
| ------------------------------------------------- | ------------------------------------------------------------------ |
| У агента есть конкретный stop condition.          | Плохие тесты закрепляют неверное поведение — ревьюй кейсы.         |
| Существующий suite становится regression harness. | Всё равно останавливай/перенаправляй, если агент уходит в сторону. |

**Examples:**

```text
Create a function that converts markdown to HTML.
Write tests first, then the code, then run the tests and update until they pass.
```

```text
I've got build errors. Run nr build, fix them, then run build until it passes.
```

**Thoughts:** Ты всё ещё babysit — жми stop при уходе в сторону — но цикл заменяет “QA через чат.”

**Sources:** [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips) · [Cursor Agent overview](https://cursor.com/docs/agent/overview)

</details>

## Debug with logs

<details>
<summary>Debug with logs</summary>

**Definition:** Когда статический разбор буксует, попроси агента **инструментировать** код, ты **запусти** его, потом вставь **сырые логи** — следующий фикс опирается на runtime evidence.

**How it works:**

1. Скажи: “Add logs at key points so we can see what is going on. I will run the code and paste the output.”
2. Запусти app/repro сам (или дай агенту, если безопасно под твоим Run Mode).
3. Вставь лог: “Here is the log. What is causing the issue, and how do we fix it?”
4. Повтори с более узкими логами при необходимости; убери временный logging в конце.

**When to use:** Heisenbugs, неверная ветка, “в голове работает / в рантайме нет.” Пропускай, если failing unit test уже фиксирует баг.

**Thoughts:** Относись к агенту как к джуну, который хорошо ставит instrumentation и читает дампы — окружение и финальный вывод за тобой.

**Sources:** [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Cursor Directory

<details>
<summary>Cursor Directory</summary>

**Definition:** [cursor.directory](https://cursor.directory/) — **community**-каталог правил Cursor, MCP-серверов и связанных плагинов, ранжированный по установкам — слой discovery рядом с официальным marketplace Cursor.

**How it works:**

- Смотри trending **rules** (Next.js, React Native, NestJS, …), **MCP** (GitHub, Supabase, Stripe, observability, …) и community plugins.
- Копируй/ставь в проект как [rules](./0_ai_cursor_rules.md) / [plugins](./0_ai_cursor_plugins.md) / MCP — community-пакеты это стартовая точка, не trusted by default.
- Официальный publish/install для полных Cursor Plugins — marketplace Cursor и layout `.cursor-plugin` (см. Plugins).

**When to use:** Быстрый старт правил/MCP под стек, который не хочется изобретать. Для production-shared плагинов предпочитай официальный marketplace / team kits; Directory — чтобы понять, что уже шлёт community.

**Pluses and minuses:**

| +                                             | −                                                                                                               |
| --------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Быстрый discovery stack-specific rules и MCP. | Качество community разное — аудируй до секретов и широких permissions.                                          |
| Дополняет официальный marketplace.            | Не замена проектным [rules](./0_ai_cursor_rules.md) и [skills](./0_ai_cursor_skills.md) под _твой_ репозиторий. |

**Thoughts:** Бери структуру у популярных rules; переписывай под ownership и safety этого handbook.

**Sources:** [cursor.directory](https://cursor.directory/) · [Cursor Plugins](https://cursor.com/docs/reference/plugins) · [Builder.io — How I use Cursor](https://www.builder.io/blog/cursor-tips)

</details>

## Sources

<details>
<summary>Sources</summary>

**Definition:** Официальная справка, внешние гайды и каталоги, на которых основаны советы на этой странице.

**Sources:**

- [Cursor Help Center](https://cursor.com/help)
- [Builder.io — How I use Cursor (+ my best tips)](https://www.builder.io/blog/cursor-tips)
- [cursor.directory — community rules, MCP, and plugins](https://cursor.directory/)

</details>

## Interview Practice

### Questions & Answers

**Q: What replaced “YOLO mode” in current Cursor?**  
A: **Run Modes** в Approvals & Execution — предпочитай **Auto-review**; **Run Everything** ближе всего к старому unattended YOLO.

**Q: Why ask the Agent to write tests before code?**  
A: Нетривиальные задачи редко one-shot; suite даёт объективный цикл (“run until green”) вместо QA через чат.

**Q: What is cursor.directory for?**  
A: Community discovery rules, MCP и plugins — аудируй перед install; официальный marketplace по-прежнему путь publish/install для Cursor Plugins.

**Q: Subagents vs skills — when to use which?**  
A: Полные заметки: [Subagents](./0_ai_cursor_subagents.md#subagents-vs-skills).

**Q: What do Cursor hooks do?**  
A: Полные заметки: [Hooks](./0_ai_cursor_hooks.md#what-are-hooks).
