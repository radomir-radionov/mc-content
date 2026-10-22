# AI-first engineering & SDD

Как разработчик делегирует работу AI-агентам, проверяет результат и прорабатывает спецификацию до кода. Для вопроса про _продукт_ (модель как ядро), а не про _команду_, см. [AI-first vs augmented](./0_ai.md#ai-first-approach).

### Related docs

- [Хаб AI workflow](./0_ai.md) · [Agentic SDLC](./0_ai_agentic_sdlc.md) · [Full-stack competency map](./0_engineering_fullstack.md) · [Cursor — циклы проверки](./0_ai_cursor_core.md#test-driven-agent-loops) · [Cursor Skills & rules](./0_ai_cursor_skills.md) · [SDLC & Agile](./7_special_sdlc.md) · [MVP и суждение](./0_mvp.md)

### Main concepts

| Area        | Topics                                                            |
| ----------- | ----------------------------------------------------------------- |
| **Mindset** | [AI-first engineering](#ai-first-engineering)                     |
| **Process** | [Spec-driven development (SDD)](#spec-driven-development-sdd)     |
| **Agents**  | [Coding agents on real projects](#coding-agents-on-real-projects) |

### Sections

- [AI-first engineering](#ai-first-engineering)
- [Spec-driven development (SDD)](#spec-driven-development-sdd)
- [Coding agents on real projects](#coding-agents-on-real-projects)
- [Interview Practice](#interview-practice)

---

## AI-first engineering

<details>
<summary>AI-first engineering</summary>

**Definition:** **AI-first engineering** — это то, _как разработчик работает_, когда агенты и модели в toolchain: ты решаешь, что поручить AI, как проверить результат, и остаёшься ответственным за итоговое решение. Это **не** то же самое, что **AI-first product** (где модель _и есть_ продукт) — см. [AI-first approach](./0_ai.md#ai-first-approach).

**How it works:**

1. **Делегирование** — определи, какую часть задачи можно поручить AI и как проверить результат; разбей большую задачу на проверяемые шаги.
2. **Контекст** — передай агенту спецификацию, нужные файлы, ограничения и критерии приёмки; убери шум, который уводит модель в сторону.
3. **Курс** — замечай, когда агент пошёл в неверную сторону (неверный API, не тот репозиторий, раздувание scope), и корректируй до огромного diff.
4. **Проверка** — читай полученный код, проверяй предположения, запускай тесты, линтер и сборку; разбирайся в последствиях изменений (типы, миграции, безопасность).
5. **Инструменты** — выбирай модель и инструменты под задачу с учётом качества, скорости и стоимости (Agent vs Tab, subagents, MCP-доки, более лёгкая модель для узких задач).
6. **Ответственность** — агент предлагает; разработчик решает, что попадает в merge и в прод.

**When to use:** Любая нетривиальная фича, рефакторинг или прод-фикс с Cursor/Copilot/Claude Code. Для однострочной опечатки формальный SDD не обязателен — привычка нужна там, где страдают пользователи или команда.

**Pluses and minuses:**

| +                                            | −                                                                    |
| -------------------------------------------- | -------------------------------------------------------------------- |
| Быстрее итерации при дисциплине проверки.    | Легко «принять» непрочитанный код и унаследовать тонкие баги.        |
| Критерии приёмки задаются до реализации.     | Без спецификации агент оптимизирует правдоподобный, а не верный код. |
| Понятно, когда _не_ запускать тяжёлый Agent. | Размытые мега-промпты бьют по контексту и бюджету.                   |

**Examples:**

- **Хороший шаг:** «Добавить `Topic` для `0_ai_engineering.md` в catalog; приёмка: `npm run build` зелёный, есть EN+RU twins.»
- **Слабый шаг:** «Сделай handbook лучше с AI.»
- **Хорошая verify:** тесты, прочитать migration SQL, нет secret в `NEXT_PUBLIC_`.
- **Слабая verify:** «агент сказал, что работает».

**Thoughts:** AI-first engineering — это **AI-augmented delivery с человеческим gatekeeping**. Если не можешь объяснить, что изменилось и как проверил — работа не завершена, независимо от уверенности модели.

**Sources:** [Cursor Agent overview](https://cursor.com/docs/agent/overview) · [Test-driven agent loops](./0_ai_cursor_core.md#test-driven-agent-loops)

</details>

## Spec-driven development (SDD)

<details>
<summary>Spec-driven development (SDD)</summary>

**Definition:** **Spec-driven development (SDD)** — до написания кода (или в одном темпе с ним) ты прорабатываешь спецификацию: поведение системы, ограничения и **критерии приёмки**. Спецификация — контракт для AI-агентов и чеклист, по которому отклоняешь неверный output.

**How it works:**

1. **Specify** — поведение для пользователя, краевые случаи, non-goals, контракты данных и «готово когда» (тесты, скриншоты, метрики).
2. **Task the agent** — на основе спеки ставишь задачи AI-агентам упорядоченными, проверяемыми промптами; один прогон агента по возможности = один срез спеки.
3. **Implement & verify** — код и тесты должны трассироваться к пунктам спеки; падающие тесты означают ошибку в спеке или коде — чини нужный слой.
4. **Sync on change** — при изменении требований поддерживаешь спецификацию, код и тесты в согласованном состоянии (spec и код в одном PR, если меняется «done when»).

SDD дополняет [SDLC & Agile](./7_special_sdlc.md): Agile задаёт _ритм_; SDD задаёт, _что значит «готово»_, до того как агент напишет diff. Для **мультиагентных** передач между этапами (spec → architecture → code → test) см. [Agentic SDLC](./0_ai_agentic_sdlc.md).

**When to use:** Новые фичи, изменения API, темы handbook, рефакторинг с переносом поведения — любая работа, которую ты отдал бы другому инженеру с тикетом. Только тривиальные фиксы можно без формальной спеки.

**Pluses and minuses:**

| +                                                    | −                                                          |
| ---------------------------------------------------- | ---------------------------------------------------------- |
| Меньше scope drift у агентов при явных критериях.    | Время на spec кажется медленным, пока не падает переделка. |
| Ревьюер проходит spec → tests → code за один проход. | Устаревшая spec врёт — это такой же долг, как мёртвый код. |
| Стыкуется с AI-first engineering: spec — guardrail.  | «Spec только в чате» не считается — зафиксируй письменно.  |

**Examples:**

Фрагмент минимальной спеки для темы handbook:

```text
Behavior: новый sidebar topic 0_ai_engineering.md (EN + RU).
Constraints: conspect slot markers; catalog row в mc-app.
Acceptance: check-order --strict (en+ru); build passes.
Out of scope: менять правила parser.
```

Full-stack API (иллюстрация):

```text
Behavior: POST /discounts/apply → 200 + totals, 409 if expired.
Data: migration discounts.code (unique, indexed).
Frontend: inline error на 409; disable submit while pending.
Acceptance: integration test 409; migration в CI.
Non-goals: admin UI для codes.
```

**Thoughts:** SDD обязателен для профессиональной работы с агентами: модель всегда _выдаст_ код; только spec говорит, _верный_ ли он.

**Sources:** [Agile — Doka](https://doka.guide/tools/agile/) · [AI-first engineering](#ai-first-engineering)

</details>

## Coding agents on real projects

<details>
<summary>Coding agents on real projects</summary>

**Definition:** **Coding agents on real projects** — практический опыт с **Claude Code**, **OpenAI Codex** (CLI/IDE), **Cursor Agent/CLI** или аналогичными tool-using ассистентами на **боевом codebase**, с той же планкой, что у человека: ветки, review, CI, понимание rollback.

**How it works:**

| Tool                   | Surface                                     | Real-project fit                                                                |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------------------------- |
| **Cursor Agent / CLI** | Editor + `agent -p`                         | Multi-file, MCP docs, handbook repos                                            |
| **Claude Code**        | Terminal `claude`, `-p`                     | SSH, scripted loops; `.claude/skills/`                                          |
| **OpenAI Codex**       | IDE/CLI, `.codex/skills/`, `.codex/agents/` | Тот же agent pattern; общие skills с Cursor ([Skills](./0_ai_cursor_skills.md)) |
| **GitHub Copilot**     | Inline + Chat/Agent                         | Стандарт IDE; `.github/copilot-instructions.md`                                 |

Общий **agent pattern:** read / edit / bash / grep / MCP — [AI-assisted workflow](./0_ai.md#ai-assisted-workflow-cursor-copilot-claude-code).

**Session checklist (production repo):**

1. Ветка от main; **spec + acceptance** ([SDD](#spec-driven-development-sdd)), не размытая цель.
2. Только нужные файлы; без secrets ([Secrets](./0_engineering_fullstack.md#secrets-and-access-control)).
3. Минимальный diff на один пункт spec; review каждого hunk.
4. Локально то же, что CI: `lint`, `test`, `build` ([CI/CD](./10_build_ci.md)).
5. PR от вас; текст агента ≠ description и tests.
6. Если agent трогал **migrations**, **auth**, **API** — blast radius ([Full-stack scope](./0_engineering_fullstack.md#full-stack-delivery-scope)).

**When to use:** Вакансия про Claude Code, Codex, AI-native delivery; после базы [Cursor Core](./0_ai_cursor_core.md).

**Pluses and minuses:**

| +                                          | −                                              |
| ------------------------------------------ | ---------------------------------------------- |
| Быстрее крупные рефакторинги               | Код «как в демо» без CI — риск                 |
| Общие skills/rules между Cursor/Codex dirs | Разные лимиты контекста и approval у продуктов |

**Thoughts:** На интервью важно, что ты **ship’ил** с агентами на реальных репо — не что смотрел tutorial. Говори про verification и SDD, не только про название модели.

**Sources:** [Claude Code — glossary/tools](https://code.claude.com/docs/en/glossary) · [Cursor Agent overview](https://cursor.com/docs/agent/overview)

</details>

## Interview Practice

### Questions & Answers

**Q: Чем AI-first _engineering_ отличается от AI-first _product_?**  
A: Product — приложение без модели не существует. Engineering — команда строит с AI; разработчик отвечает за проверку и merge. См. [AI-first approach](./0_ai.md#ai-first-approach) и [AI-first engineering](#ai-first-engineering).

**Q: Что должно быть в spec до того, как просишь агента писать код?**  
A: Поведение, ограничения, критерии приёмки и явные non-goals — чтобы ревьюер мог сказать pass/fail без догадок.

**Q: Кто отвечает, если agent-generated код ломает прод?**  
A: Разработчик, который смержил — агент инструмент, не автор решения.

**Q: Когда обновлять spec, а не только код?**  
A: Когда меняются требования или «done when»; не допускай drift spec, пока код и тесты уже уехали вперёд.

**Q: «Опыт Claude Code / Codex» — это один вендор?**  
A: Нет — tool-using агенты на реальных репо: spec, context, CI, ответственный merge. См. [Coding agents on real projects](#coding-agents-on-real-projects).
