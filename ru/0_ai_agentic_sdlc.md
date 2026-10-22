# Агентный SDLC

ИИ на всём конвейере поставки ПО — не только в IDE. **Сначала классический SDLC:** [принципы](./7_special_sdlc_principles.md) и [шесть этапов](./7_special_sdlc_phases.md) — затем роли агентов, human gates, инфраструктура (трекер, песочница, RAG, MCP) и lead time.

### Related docs

- [AI workflow hub](./0_ai.md) · [AI-first engineering & SDD](./0_ai_engineering.md) · [Cursor Subagents](./0_ai_cursor_subagents.md) · [SDLC principles](./7_special_sdlc_principles.md) · [SDLC phases](./7_special_sdlc_phases.md) · [SDLC & Agile](./7_special_sdlc.md) · [Full-stack delivery](./0_engineering_fullstack.md) · [CI/CD](./10_build_ci.md)

### Main concepts

| Area            | Topics                                                                                                                                                                                                                  |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Foundations** | [Principles (on this page)](#principles-for-agentic-delivery), [Phases (on this page)](#phases-for-agentic-delivery) — полные темы: [principles](./7_special_sdlc_principles.md) · [phases](./7_special_sdlc_phases.md) |
| **Basics**      | [AI in SDLC](#ai-in-sdlc), [Lead time vs coding speed](#lead-time-vs-coding-speed)                                                                                                                                      |
| **Maturity**    | [AI-assisted vs agentic SDLC maturity](#ai-assisted-vs-agentic-sdlc-maturity)                                                                                                                                           |
| **Roles**       | [Multi-agent SDLC roles](#multi-agent-sdlc-roles), [Skeptic agent (critic loop)](#skeptic-agent-critic-loop)                                                                                                            |
| **People**      | [Human gateways in agentic SDLC](#human-gateways-in-agentic-sdlc)                                                                                                                                                       |
| **Platform**    | [Agentic SDLC infrastructure](#agentic-sdlc-infrastructure)                                                                                                                                                             |
| **Quality**     | [Measuring agentic SDLC](#measuring-agentic-sdlc), [Agentic SDLC anti-patterns](#agentic-sdlc-anti-patterns)                                                                                                            |

### Sections

- [Principles for agentic delivery](#principles-for-agentic-delivery)
- [Phases for agentic delivery](#phases-for-agentic-delivery)
- [AI in SDLC](#ai-in-sdlc)
- [Lead time vs coding speed](#lead-time-vs-coding-speed)
- [AI-assisted vs agentic SDLC maturity](#ai-assisted-vs-agentic-sdlc-maturity)
- [Multi-agent SDLC roles](#multi-agent-sdlc-roles)
- [Skeptic agent (critic loop)](#skeptic-agent-critic-loop)
- [Human gateways in agentic SDLC](#human-gateways-in-agentic-sdlc)
- [Agentic SDLC infrastructure](#agentic-sdlc-infrastructure)
- [Measuring agentic SDLC](#measuring-agentic-sdlc)
- [Agentic SDLC anti-patterns](#agentic-sdlc-anti-patterns)
- [Interview Practice](#interview-practice)

---

## Principles for agentic delivery

<details>
<summary>Principles for agentic delivery</summary>

**Definition:** Прежде чем агенты ведут поставку, команде нужны общие **принципы SDLC**. Полный разбор: [SDLC principles](./7_special_sdlc_principles.md) (соседняя тема в **AI → SDLC**).

**How it works:**

| Principle               | Why it matters for agents                                                   |
| ----------------------- | --------------------------------------------------------------------------- |
| **Cyclicity**           | Контекст передаётся между релизами, а не только в одном чате                |
| **Systematic approach** | У каждой фазы — артефакты, которые агенты читают и обновляют                |
| **Quality orientation** | [Skeptic loops](#skeptic-agent-critic-loop) + CI, а не «смержили diff»      |
| **Flexibility**         | Те же фазы при Waterfall или Agile — агенты следуют трекеру                 |
| **Transparency**        | Статус задачи и вложения — source of truth для агентов                      |
| **Risk management**     | Human [gateways](#human-gateways-in-agentic-sdlc) на architecture и release |
| **User focus**          | Требования агентов должны вести к acceptance criteria                       |

**When to use:** Зашли на **Home → AI → Agentic SDLC** без классических заметок — начните здесь, затем [SDLC principles](./7_special_sdlc_principles.md).

**Pluses and minuses:**

| +                            | −                                                   |
| ---------------------------- | --------------------------------------------------- |
| Защита от «агентов на хаосе» | Без принципов — быстрая несовместимая автоматизация |

**Thoughts:** Agentic SDLC — слой **над** дисциплиной, не замена — см. [anti-patterns](#agentic-sdlc-anti-patterns).

**Sources:** [SDLC principles](./7_special_sdlc_principles.md) · [SDLC & Agile](./7_special_sdlc.md)

</details>

## Phases for agentic delivery

<details>
<summary>Phases for agentic delivery</summary>

**Definition:** **Agentic SDLC** проходит **шесть классических этапов**; агенты подставляются в роли внутри каждого. Определения и deliverables: [SDLC phases](./7_special_sdlc_phases.md).

**How it works:**

| Phase                                                                     | Typical agent role             | Human gateway (examples) |
| ------------------------------------------------------------------------- | ------------------------------ | ------------------------ |
| [Planning](./7_special_sdlc_phases.md#planning)                           | Черновик scope/priority        | Бюджет, milestones       |
| [Requirements](./7_special_sdlc_phases.md#requirements-analysis)          | **Analyst** — spec, acceptance | Апрув ТЗ / scope         |
| [Design](./7_special_sdlc_phases.md#design)                               | **Architect** — design, tasks  | Апрув архитектуры        |
| [Development](./7_special_sdlc_phases.md#development)                     | **Developer** — code           | PR review                |
| [Testing & deployment](./7_special_sdlc_phases.md#testing-and-deployment) | **Tester**, **Deploy**         | Апрув релиза             |
| [Support](./7_special_sdlc_phases.md#support)                             | Triage, patch drafts           | Ownership prod           |

В Agile фазы **перекрываются**; агенты привязаны к артефактам. Схема: [overview](./7_special_sdlc_phases.md#overview).

**When to use:** Сопоставление [ролей](#multi-agent-sdlc-roles) с колонками трекера; собес про AI in SDLC.

**Pluses and minuses:**

| +                                | −                                        |
| -------------------------------- | ---------------------------------------- |
| Одна таблица «классика → агенты» | Названия фаз в компании могут отличаться |

**Examples:** Analyst → architect; skeptic до architecture gate человека.

**Thoughts:** Ассистенты в IDE — в основном **development**; ROI agentic — requirements → deploy ([lead time](#lead-time-vs-coding-speed)).

**Sources:** [SDLC phases](./7_special_sdlc_phases.md) · [AI augmentation of SDLC phases](./7_special_sdlc.md#ai-augmentation-of-sdlc-phases)

</details>

---

## AI in SDLC

<details>
<summary>AI in SDLC</summary>

**Definition:** **AI in SDLC** (Software Development Life Cycle) — применение ИИ к **управлению и созданию артефактов** на всём конвейере: требования, проектные документы, код, тесты, шаги релиза — а не только автодополнение кода в редакторе одного разработчика.

**How it works:**

- Классические [фазы SDLC](./7_special_sdlc_phases.md) сохраняются (требования → проектирование → разработка → тест → деплой → сопровождение); меняется **кто производит артефакт** и **как быстро передаётся контекст** между этапами.
- В зрелой модели агенты ведут **структурированные передачи**: спецификация, архитектура, реализация, ревью, доказательства тестов — привязанные к одной задаче в трекере.
- [Spec-driven development (SDD)](./0_ai_engineering.md#spec-driven-development-sdd) — контракт между намерением человека и результатом агента на каждом этапе.

**When to use:** Когда релизы тормозят согласования, архитектура и очереди QA, а не скорость набора кода. Полную автоматизацию конвейера не нужно для однострочных правок.

**Pluses and minuses:**

| +                                                                 | −                                                                     |
| ----------------------------------------------------------------- | --------------------------------------------------------------------- |
| Бьёт в сквозной **lead time**, а не в локальную скорость кодинга. | Бесполезно на хаотичном репо без стандартов и доступа к инструментам. |
| Артефакты — проверяемые шаги, а не только история чата.           | Нужны трекер, CI и контекст (RAG/MCP).                                |

**Thoughts:** «ИИ в SDLC» — не слоган продукта, а операционная модель. Сочетайте с [AI-first engineering](./0_ai_engineering.md#ai-first-engineering): человек задаёт цели и апрувит ворота; агенты работают в рамках спецификации.

**Sources:** [Model Context Protocol](https://modelcontextprotocol.io/) · [Doka — Agile](https://doka.guide/tools/agile/)

</details>

## Lead time vs coding speed

<details>
<summary>Lead time vs coding speed</summary>

**Definition:** **Lead time** (или **cycle time**) — время от **создания задачи** до **ценности в проде** (или до заранее определённого «done»). **Coding speed** — как быстро появляется diff. Более быстрый код не сокращает lead time, если узкое место на других этапах.

**How it works:**

- В enterprise-командах реализация часто занимает **небольшую долю** календарного времени; остальное — требования, согласование архитектуры, ревью, интеграция, регресс, релизная политика.
- Ассистенты в IDE ускоряют только **срез реализации**; они не планируют архитектора, не прогоняют CI и не деплоят в prod.
- Агентный SDLC пытается **параллелить и автоматизировать** артефакты до/после кода (спека, тесты, деплой), сохраняя **human gateways** там, где риск высок.

**When to use:** Объяснить, почему Copilot/Cursor не сдвинул дату релиза; выбрать метрики для AI-инициатив (lead time, а не строки/час).

**Pluses and minuses:**

| +                                                  | −                                            |
| -------------------------------------------------- | -------------------------------------------- |
| Связывает инвестиции в ИИ с TTM бизнеса            | Легко принять маркетинговые «80/20» за закон |
| Объясняет парадокс «быстрый код — медленный релиз» | Нужны свои baselines по этапам               |

**Examples:**

- Ассистент сделал компонент за час; merge ждёт неделю архитектурного ревью → lead time не изменился.
- Агентный поток: аналитик → апрув архитектуры человеком → dev/review/test → апрув релиза → lead time падает, если ворота остаются тонкими.

**Thoughts:** «Кодинг — 20% времени» трактуйте **качественно**, не как формулу бюджета. Мерьте **свой** lead time и время этапов.

**Sources:** [DORA metrics — Google Cloud](https://cloud.google.com/devops/state-of-devops)

</details>

## AI-assisted vs agentic SDLC maturity

<details>
<summary>AI-assisted vs agentic SDLC maturity</summary>

**Definition:** Трёхуровневая зрелость ИИ в поставке: **(1) AI-assisted** (разработчик + подсказки IDE), **(2) AI pair** (модель пишет крупные куски, человек правит), **(3) agentic SDLC** (специализированные агенты закрывают конвейер; человек оркестрирует и апрувит).

**How it works:**

| Level          | Who does the work               | Human role              | What speeds up             | Main limit                         |
| -------------- | ------------------------------- | ----------------------- | -------------------------- | ---------------------------------- |
| 1 AI-assisted  | Developer with completions/chat | Executor (Developer)    | Boilerplate, syntax lookup | Only the coding step               |
| 2 AI pair      | Model drafts; developer edits   | Reviewer / co-author    | Single features, refactors | Context often one task / few files |
| 3 Agentic SDLC | Agent team (spec → deploy)      | Orchestrator / approver | End-to-end delivery slice  | Needs formal process + infra + MCP |

Уровень 3 — [роли агентов](#multi-agent-sdlc-roles) и [инфраструктура](#agentic-sdlc-infrastructure). Уровни 1–2 — [AI-assisted workflow](./0_ai.md#ai-assisted-workflow-cursor-copilot-claude-code).

**When to use:** Roadmap и собеседования («где мы по зрелости ИИ?»). Большинству команд нужен сильный уровень 2 + CI до заявленного уровня 3.

**Pluses and minuses:**

| +                                 | −                                            |
| --------------------------------- | -------------------------------------------- |
| Общий язык для лидов и IC         | Уровень 3 — не «ещё лицензии Copilot»        |
| Честные ожидания по prerequisites | Легко убрать ворота и назвать это «агентами» |

**Thoughts:** Уровень 3 без [SDD](./0_ai_engineering.md#spec-driven-development-sdd) и [verification loops](./0_ai_cursor_core.md#test-driven-agent-loops) — быстрее неправильный код в масштабе.

**Sources:** [Cursor Agent overview](https://cursor.com/docs/agent/overview)

</details>

## Multi-agent SDLC roles

<details>
<summary>Multi-agent SDLC roles</summary>

**Definition:** В **agentic SDLC** работа строится вокруг **ролей** (часто отдельные агенты или профили subagent), каждая обогащает общую задачу артефактами — а не только передачи между отделами.

**How it works:**

| Role          | Input                      | Output                                                                    |
| ------------- | -------------------------- | ------------------------------------------------------------------------- |
| **Analyst**   | Raw idea / ticket stub     | Requirements, use cases, acceptance criteria                              |
| **Architect** | Approved spec              | Technical design, data/API impact, subtask breakdown                      |
| **Developer** | Design + tasks             | Code, migrations, UI — aligned to standards                               |
| **Reviewer**  | Diff                       | Style, security, logic findings                                           |
| **Tester**    | Acceptance criteria        | Unit/integration tests, run results, failure logs                         |
| **Deploy**    | Green tests + merge policy | Build artifact, deploy to staging/prod per pipeline                       |
| **Skeptic**   | Others’ artifacts          | Critical feedback loops (see [Skeptic agent](#skeptic-agent-critic-loop)) |

Роли могут идти **последовательно** или **перекрываться** (Agile-срез). Контекст живёт в **задаче** (трекер + репо + ссылки), а не только в чате.

В Cursor роли часто изолируют через [Subagents](./0_ai_cursor_subagents.md), родительский Agent оркестрирует.

**When to use:** Крупные фичи, расширения платформы, повторяющийся конвейер.

**Pluses and minuses:**

| +                                    | −                                            |
| ------------------------------------ | -------------------------------------------- |
| Совпадает с мышлением сильных команд | Раздувание числа агентов без схем артефактов |
| Параллельное ревью и реализация      | Провал handoff без шаблонов спеки/дизайна    |

**Examples:**

- **Дашборд зависимостей:** аналитик — UX графа → архитектор — API → разработчик — UI + скрипт → скептик — производительность глубоких деревьев → человек апрувит архитектуру и merge.
- **Мультискоринг бэклога:** аналитик — фреймворки → архитектор — таблицы и пересчёт → разработчик — схема + триггеры → тестировщик — краевые значения → человек апрувит UX оценки.

**Thoughts:** Кейсы «недели → дни» часто на **единой low-code платформе** (одна модель данных, автоген UI). Для зоопарка микросервисов множитель другой — мерьте локально.

**Sources:** [Subagents](https://cursor.com/docs/subagents)

</details>

## Skeptic agent (critic loop)

<details>
<summary>Skeptic agent (critic loop)</summary>

**Definition:** **Skeptic agent** (критик) **не** реализует фичи. Он **оспаривает** результаты других агентов — дыры в архитектуре, небезопасный код, слабые тесты — и возвращает работу до порога качества или **лимита итераций**.

**How it works:**

- **Feedback loop** с аналитиком/архитектором/разработчиком/тестировщиком до человека.
- Проверки: производительность, покрытие acceptance criteria, соответствие конвенциям репо, базовая безопасность.
- Аналог — readonly [custom subagents](./0_ai_cursor_subagents.md#example-subagents) (`task-verifier`, auditors).

**When to use:** Когда скорость иначе съедает глубину ревью. Для тривиальных задач — опционально.

**Pluses and minuses:**

| +                                 | −                                                 |
| --------------------------------- | ------------------------------------------------- |
| Ловит «правдоподобно, но неверно» | Стоимость токенов при длинных спорах агентов      |
| Меньше мелочи на человеке         | Плохие промпты → высокий reject rate без качества |

**Examples:**

- Скептик: «Неограниченная глубина дерева зависимостей заблокирует UI — пагинация или виртуализация».
- Скептик: «Нет теста на нулевой вес / переполнение в калькуляторе score».

**Thoughts:** Следите за **reject rate** скептика — слишком высокий: слабая спека или [контекст](./0_ai.md#context-window-llm-context); слишком низкий: критик для галочки.

**Sources:** [Subagents vs skills](./0_ai_cursor_subagents.md#subagents-vs-skills)

</details>

## Human gateways in agentic SDLC

<details>
<summary>Human gateways in agentic SDLC</summary>

**Definition:** **Human gateways** — обязательные точки, где **человек** (лид, архитектор, владелец релиза) принимает риск до продолжения конвейера, даже если артефакты сделали агенты.

**How it works:**

Типичные ворота:

1. **Spec / scope** — верны intent и критерии приёмки.
2. **Architecture** — дизайн укладывается в систему, данные и NFR.
3. **Release** — merge/deploy в prod (или customer-facing) с планом отката.

Человек несёт **юридическую и операционную ответственность** — агенты не «авторы» инцидентов в проде ([Coding agents on real projects](./0_ai_engineering.md#coding-agents-on-real-projects)).

**When to use:** Enterprise и регулируемые домены; строгость ворот — по blast radius (auth, платежи, PII, миграции).

**Pluses and minuses:**

| +                              | −                                         |
| ------------------------------ | ----------------------------------------- |
| Сохраняет продуктовое суждение | Лишние ворота снова создают очереди       |
| Нужно для compliance           | «Апрув непрочитанного diff» — антипаттерн |

**Thoughts:** Цель — **меньше** касаний человека, каждое **выше сигнал**, не «ноль людей».

**Sources:** [AI-first engineering — ownership](./0_ai_engineering.md#ai-first-engineering)

</details>

## Agentic SDLC infrastructure

<details>
<summary>Agentic SDLC infrastructure</summary>

**Definition:** Минимальная **платформа** для агентного SDLC: структурированные задачи, среда исполнения, индексированный корпоративный контекст и **интеграции**, чтобы агенты действовали в реальных системах, а не только генерировали текст.

**How it works:**

| Pillar                   | Purpose                                                                                |
| ------------------------ | -------------------------------------------------------------------------------------- |
| **Task tracker**         | Единая точка входа: описание, статус, комментарии, артефакты (спека, CI)               |
| **Execution sandbox**    | Сборка, тесты, логи (IDE, CLI, CI), итеративные исправления                            |
| **Context (RAG + repo)** | Схемы, API, ADR, история коммитов                                                      |
| **Integrations (MCP)**   | [Model Context Protocol](https://modelcontextprotocol.io/) — трекеры, БД, деплой, доки |

Low-code платформы уменьшают «клей», но не обязательны; дисциплина + MCP + CI достаточны для generic-репо.

**When to use:** Перед масштабированием за [уровень 2 зрелости](#ai-assisted-vs-agentic-sdlc-maturity) на prod-коде.

**Pluses and minuses:**

| +                       | −                            |
| ----------------------- | ---------------------------- |
| Агенты как операторы    | Стоимость интеграции upfront |
| Воспроизводимые прогоны | Риск утечек при плохом MCP   |

**Examples:**

- MCP: ветка, PR, issue, read-only schema, workflow тестов.
- MyConspect: трекер = issues/PR; sandbox = CI `mc-app`; контекст = `mc-content` + skills; скептики = `.cursor/agents/*`.

**Thoughts:** «Агенты на хаос» → несовместимый код быстрее — сначала этапы в трекере ([anti-patterns](#agentic-sdlc-anti-patterns)).

**Sources:** [Model Context Protocol — specification](https://modelcontextprotocol.io/specification/2025-03-26)

</details>

## Measuring agentic SDLC

<details>
<summary>Measuring agentic SDLC</summary>

**Definition:** Метрики агентного SDLC про **сквозную поставку и качество**, не про символы в минуту.

**How it works:**

| Metric                            | What it tells you                                                               |
| --------------------------------- | ------------------------------------------------------------------------------- |
| **Lead time / cycle time**        | Задача → prod (главная бизнес-метрика)                                          |
| **Idea → architecture time**      | Скорость появления утверждённого дизайна                                        |
| **Skeptic reject rate**           | Качество спеки/контекста vs исполнители                                         |
| **Human touchpoints per feature** | Сколько раз лид вмешался (цель — меньше, не ноль)                               |
| **AI code churn**                 | Доля переписанного агентом кода скоро после merge (~20% на старте — свой тренд) |
| **Defect escape / rollback rate** | Скорость без качества = неверная настройка                                      |
| **Token cost per task**           | Длинные дебаты агентов съедают экономию времени                                 |

Сочетайте с частотой деплоя и change failure rate в духе [DORA](https://cloud.google.com/devops/state-of-devops).

**When to use:** До/после пилотов; настройка лимитов итераций скептика.

**Pluses and minuses:**

| +                   | −                        |
| ------------------- | ------------------------ |
| Не «купили Copilot» | Нужен baseline до старта |

**Thoughts:** Lead time упал, escapes выросли — чините тесты и [скептика](#skeptic-agent-critic-loop), не добавляйте агентов.

**Sources:** [DORA metrics — Google Cloud](https://cloud.google.com/devops/state-of-devops)

</details>

## Agentic SDLC anti-patterns

<details>
<summary>Agentic SDLC anti-patterns</summary>

**Definition:** Типичные провалы при внедрении «AI SDLC» без готовности процесса и платформы.

**How it works:**

| Anti-pattern                  | Consequence                                 | Mitigation                                               |
| ----------------------------- | ------------------------------------------- | -------------------------------------------------------- |
| Agents on **chaotic process** | Галлюцинации процесса, неверная архитектура | Сначала трекер и стандарты ([SDLC](./7_special_sdlc.md)) |
| **No skeptic**                | Быстрый мусор, безопасность, долг           | Обязательные critic loops                                |
| **No human gateways**         | Ответственность, дрейф видения              | Апрув spec, architecture, release                        |
| **Isolated context**          | Ломает соседние модули                      | Repo context, MCP, RAG, CI                               |
| **Coding-only KPI**           | TTM не двигается                            | [Lead time](#lead-time-vs-coding-speed)                  |
| **Unbounded agent loops**     | Счёт за токены, нет релиза                  | Лимит итераций + эскалация человеку                      |

**When to use:** Readiness review, post-mortem, собес «как раскатывать агентов?».

**Pluses and minuses:**

| +               | −                |
| --------------- | ---------------- |
| Дешёвый чеклист | Не исчерпывающий |

**Thoughts:** Агентный SDLC — **автоматизация процесса**; мусор на входе → мусор быстрее.

**Sources:** [AI-first engineering](./0_ai_engineering.md#ai-first-engineering)

</details>

## Interview Practice

### Questions & Answers

**Q: Чем «AI in SDLC» отличается от Copilot в IDE?**  
A: SDLC — артефакты и координация от требований до деплоя; IDE в основном ускоряет реализацию. См. [AI in SDLC](#ai-in-sdlc) и [уровень 1 зрелости](#ai-assisted-vs-agentic-sdlc-maturity).

**Q: Почему код пишется быстрее, а релизы — нет?**  
A: Узкие места в ревью, архитектуре, тестах и релизе — мерьте [lead time](#lead-time-vs-coding-speed).

**Q: Зачем skeptic agent?**  
A: Adversarial-проверка до human gates — как readonly [subagents](./0_ai_cursor_subagents.md) в Cursor.

**Q: Где человек обязан остаться в контуре?**  
A: Апрув spec, architecture, production release и ответственность за merge ([Human gateways](#human-gateways-in-agentic-sdlc)).

**Q: Какая инфраструктура нужна кроме LLM API?**  
A: Трекер, sandbox/CI, контекст, MCP — [Agentic SDLC infrastructure](#agentic-sdlc-infrastructure).

**Q: Одна метрика успеха agentic SDLC?**  
A: Lead time от бэклога до prod при стабильном или лучшем defect escape / rollback rate.

**Q: Какой классический SDLC прочитать перед agentic SDLC?**  
A: [Принципы](#principles-for-agentic-delivery) и [шесть этапов](#phases-for-agentic-delivery) — полностью в [principles](./7_special_sdlc_principles.md) и [phases](./7_special_sdlc_phases.md).

**Q: Какую фазу в основном ускоряет coding assistant?**  
A: Только **development** — agentic SDLC бьёт по всем фазам в [таблице этапов](#phases-for-agentic-delivery).
