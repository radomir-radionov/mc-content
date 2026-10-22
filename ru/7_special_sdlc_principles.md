# Принципы SDLC

Базовые принципы жизненного цикла разработки ПО — как команда сохраняет структуру, качество и адаптивность при Waterfall, итерациях и Agile.

### Related docs

- [SDLC & Agile](./7_special_sdlc.md) · [SDLC phases](./7_special_sdlc_phases.md) · [Agentic SDLC (AI)](./0_ai_agentic_sdlc.md) · [Testing fundamentals](./7_special_testing_fundamentals.md) · [MVP & judgment](./0_mvp.md)

### Main concepts

| Area           | Topics                                                                                                                                                                                                                                              |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Principles** | [Overview](#overview)                                                                                                                                                                                                                               |
| **By theme**   | [Cyclicity](#cyclicity) · [Systematic approach](#systematic-approach) · [Quality orientation](#quality-orientation) · [Flexibility](#flexibility) · [Transparency](#transparency) · [Risk management](#risk-management) · [User focus](#user-focus) |

### Sections

- [Overview](#overview)
- [Cyclicity](#cyclicity)
- [Systematic approach](#systematic-approach)
- [Quality orientation](#quality-orientation)
- [Flexibility](#flexibility)
- [Transparency](#transparency)
- [Risk management](#risk-management)
- [User focus](#user-focus)
- [Interview Practice](#interview-practice)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **SDLC principles** — сквозные правила, без которых модель жизненного цикла не работает: команда может идти [Waterfall](./7_special_sdlc.md#waterfall), итерациями или [Agile](./7_special_sdlc.md#agile). Принципы относятся к **каждой** [фазе SDLC](./7_special_sdlc_phases.md).

**How it works:**

| Principle                                   | One-line meaning                                                      |
| ------------------------------------------- | --------------------------------------------------------------------- |
| [Cyclicity](#cyclicity)                     | Поставка повторяется; продукт эволюционирует цикл за циклом           |
| [Systematic approach](#systematic-approach) | У каждой фазы — цели, задачи и результаты                             |
| [Quality orientation](#quality-orientation) | Проверка и тесты на протяжении всего цикла                            |
| [Flexibility](#flexibility)                 | Те же фазы, разный порядок и перекрытие (Agile vs последовательность) |
| [Transparency](#transparency)               | Stakeholders видят статус и выходы фаз                                |
| [Risk management](#risk-management)         | Риски выявляют и снижают на каждой фазе                               |
| [User focus](#user-focus)                   | Решения опираются на потребности пользователей                        |

**When to use:** Онбординг, проектирование процесса, собеседования, перед масштабированием [agentic SDLC](./0_ai_agentic_sdlc.md) (агентам нужны явные стандарты).

**Pluses and minuses:**

| +                                 | −                                             |
| --------------------------------- | --------------------------------------------- |
| Общий язык для ролей              | Принципы без инструментов остаются на слайдах |
| Связка классического SDLC и Agile | Под дедлайном принципы легко «забыть»         |

**Thoughts:** Принципы объясняют **зачем** существуют [шесть фаз](./7_special_sdlc_phases.md); заметки по методологиям — **как** их планировать.

**Sources:** [Doka — Agile](https://doka.guide/tools/agile/) · [SDLC phases](./7_special_sdlc_phases.md)

</details>

## Cyclicity

<details>
<summary>Cyclicity</summary>

**Definition:** **Cyclicity (цикличность)** — разработка не одноразовая линия: продукт многократно проходит [планирование → поддержку](./7_special_sdlc_phases.md), когда меняются требования, технологии и пользователи.

**How it works:**

- После релиза начинается новый виток: обратная связь и инциденты снова попадают в [requirements](./7_special_sdlc_phases.md#requirements-analysis) и [design](./7_special_sdlc_phases.md#design).
- [Agile-спринты](./7_special_sdlc.md#agile) — короткие циклы с полным срезом (plan → build → test → review).
- Поддержка — часть SDLC, а не «после проекта».

**When to use:** Объяснить, почему support и change requests — внутри SDLC.

**Pluses and minuses:**

| +                               | −                                                        |
| ------------------------------- | -------------------------------------------------------- |
| Соответствует эволюции продукта | Без версионирования циклы сливаются в бесконечные правки |

**Thoughts:** Цикличность — причина мерить [lead time](./0_ai_agentic_sdlc.md#lead-time-vs-coding-speed) по серии релизов.

**Sources:** [SDLC phases — Support](./7_special_sdlc_phases.md#support)

</details>

## Systematic approach

<details>
<summary>Systematic approach</summary>

**Definition:** **Systematic approach (систематический подход)** — структурирование работы на фазы с **определёнными целями, задачами и deliverables**, чтобы команда шла предсказуемо, а не хаотично.

**How it works:**

- Каждая [фаза](./7_special_sdlc_phases.md) даёт артефакты (ТЗ, design doc, код, отчёт о тестах, release notes).
- Критерии «фаза завершена» связаны с [SDD](./0_ai_engineering.md#spec-driven-development-sdd).
- [Waterfall](./7_special_sdlc.md#waterfall) максимизирует последовательность; Agile сохраняет систему, перекрывая фазы в итерациях.

**When to use:** Наведение порядка в хаотичной команде; ворота перед автоматизацией агентами.

**Pluses and minuses:**

| +                           | −                                       |
| --------------------------- | --------------------------------------- |
| Меньше хаоса и дублирования | Избыточная бюрократия для мелких фиксов |

**Sources:** [Software design methodology](./7_special_sdlc.md#software-design-methodology)

</details>

## Quality orientation

<details>
<summary>Quality orientation</summary>

**Definition:** **Quality orientation (ориентация на качество)** — проверка качества **на всех этапах**, а не только в финальном тестировании: ревью, автотесты, сверка с требованиями.

**How it works:**

- Явная [фаза testing & deployment](./7_special_sdlc_phases.md#testing-and-deployment), плюс design review, code review, CI на каждый commit.
- Раннее обнаружение дефектов дешевле правок в проде ([Testing fundamentals](./7_special_testing_fundamentals.md)).
- [Agentic SDLC](./0_ai_agentic_sdlc.md#skeptic-agent-critic-loop) добавляет critic loop до human release gates.

**When to use:** Definition of Done, test strategy, ответ «когда вы тестируете?»

**Pluses and minuses:**

| +                                | −                                          |
| -------------------------------- | ------------------------------------------ |
| Меньше дорогих инцидентов в prod | Качество требует времени в каждой итерации |

**Sources:** [Testing fundamentals](./7_special_testing_fundamentals.md)

</details>

## Flexibility

<details>
<summary>Flexibility</summary>

**Definition:** **Flexibility (гибкость и адаптивность)** — SDLC подстраивается под **тип проекта и методологию**: жёсткая последовательность для контрактов, перекрытие фаз для Agile.

**How it works:**

- Те же [шесть фаз](./7_special_sdlc_phases.md); разное **расписание**: Waterfall, Scrum, Kanban.
- Акценты фаз различаются (enterprise design vs startup iteration).
- Часто **гибриды** (фикс-price снаружи, Agile внутри команды).

**When to use:** Выбор процесса на собеседовании и в КП.

**Pluses and minuses:**

| +                                   | −                                                 |
| ----------------------------------- | ------------------------------------------------- |
| Одна лексика фаз для разных моделей | «Гибкий» процесс без правил = отсутствие процесса |

**Sources:** [Agile](./7_special_sdlc.md#agile) · [Kanban](./7_special_sdlc.md#kanban)

</details>

## Transparency

<details>
<summary>Transparency</summary>

**Definition:** **Transparency (прозрачность процессов)** — stakeholders видят **прогресс, текущую фазу, риски и результаты фаз** и принимают решения по фактам.

**How it works:**

- Бэклог, roadmap, burndown, release notes привязаны к [фазам SDLC](./7_special_sdlc_phases.md).
- Границы фаз показывают, чего не хватает до деплоя (например, нет sign-off тестов).
- Поддерживает [human gateways](./0_ai_agentic_sdlc.md#human-gateways-in-agentic-sdlc).

**When to use:** Коммуникация PM/лида; аудит и compliance.

**Pluses and minuses:**

| +                           | −                           |
| --------------------------- | --------------------------- |
| Доверие и быстрая эскалация | Фиктивные статусы в трекере |

**Sources:** [Doka — Agile](https://doka.guide/tools/agile/)

</details>

## Risk management

<details>
<summary>Risk management</summary>

**Definition:** **Risk management (управление рисками)** — выявление, оценка и снижение технических, сроковых и требовательных рисков **на каждой фазе**, не только на старте проекта.

**How it works:**

- [Planning](./7_special_sdlc_phases.md#planning) и [requirements](./7_special_sdlc_phases.md#requirements-analysis) — риски scope и feasibility.
- [Design](./7_special_sdlc_phases.md#design) — архитектура и безопасность.
- Тесты и поэтапный деплой; [Support](./7_special_sdlc_phases.md#support) — операционные риски после запуска.

**When to use:** Enterprise, регулируемые домены, design review «что может пойти не так».

**Pluses and minuses:**

| +                        | −                                                               |
| ------------------------ | --------------------------------------------------------------- |
| Меньше сюрпризов в конце | Работа с рисками кажется медленной, пока кризис не предотвращён |

**Sources:** [Spiral model](./7_special_sdlc.md#spiral-model)

</details>

## User focus

<details>
<summary>User focus</summary>

**Definition:** **User focus (фокус на пользователе)** — потребности **конечных пользователей** остаются в центре от [requirements](./7_special_sdlc_phases.md#requirements-analysis) до [support](./7_special_sdlc_phases.md#support).

**How it works:**

- Requirements: персоны, jobs-to-be-done, acceptance criteria — не только внутренний wish list.
- Design: usability и [a11y](./1_html_a11y.md); development — завершённые пользовательские сценарии.
- Support замыкает цикл обратной связью ([MVP](./0_mvp.md)).

**When to use:** Product-led команды; спор «делаем по ТЗ без пользователей».

**Pluses and minuses:**

| +             | −                                        |
| ------------- | ---------------------------------------- |
| Выше adoption | Нужен доступ к пользователям и аналитике |

**Thoughts:** Без user focus Agile всё равно быстро поставит не то — см. [Agile](./7_special_sdlc.md#agile).

**Sources:** [MVP & judgment](./0_mvp.md)

</details>

## Interview Practice

### Questions & Answers

**Q: Назовите три принципа SDLC и зачем они нужны.**  
A: Например: quality orientation, cyclicity, user focus — см. [overview](#overview).

**Q: Чем гибкость отличается от «без процесса»?**  
A: Гибкость меняет **расписание** тех же фаз (Waterfall vs Agile), но deliverables и ворота остаются.

**Q: Где управление рисками кроме planning?**  
A: На каждой фазе — архитектура, тесты, эксплуатация.

**Q: Как принципы связаны с фазами SDLC?**  
A: Фазы — **что** делать; принципы — **как** делать каждую фазу хорошо. См. [SDLC phases](./7_special_sdlc_phases.md).
