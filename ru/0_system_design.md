# System design — hub

Основы, распределенные системы, архитектура интерфейса, принципы и шаблоны. Теория находится в файлах тем ниже.

### Related docs

[Проектирование системы — Ядро](./0_system_design_core.md) · [Проектирование системы — Архитектура](./0_system_design_architecture.md) · [Шаблоны архитектуры](./7_special_architecture.md) · [Проектирование системы — Данные](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, ПОЦЕЛУЙ, ЯГНИ](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Шаблоны проектирования](./7_special_patterns.md) · [Проектирование системы — Iинтервью](./0_system_design_interview.md)

### Navigation

| Тема                                                                      | Обложки                                                                |
| ------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [Проектирование системы — ядро](./0_system_design_core.md)                | Основы и основы распределенных систем.                                 |
| [Проектирование системы — Архитектура](./0_system_design_architecture.md) | Стили архитектуры и где построен HTML.                                 |
| [Архитектурные шаблоны](./7_special_architecture.md)                      | Композиция, семейство MVC, уровни/DDD и структура внешнего интерфейса. |
| [Проектирование системы — Данные](./0_system_design_data.md)              | Строительные блоки данных, кэширования и инфраструктуры.               |
| [ТВЕРДО](./7_special_engineering.md)                                      | Пять принципов ООП-проектирования модулей и API.                       |
| [СУХОЙ, ПОЦЕЛУЙ, ЯГНИ](./0_system_design_principles.md)                   | Не повторяйтесь, будьте проще, вам это не понадобится.                 |
| [GRASP](./0_system_design_grasp.md)                                       | Девять шаблонов назначения ответственности объектам.                   |
| [Шаблоны проектирования](./7_special_patterns.md)                         | Каталог GoF: творческие, структурные и поведенческие модели.           |
| [Проектирование системы — Интервью](./0_system_design_interview.md)       | Интервью вопросов и ответов по проектированию систем.                  |
| [OOP — Интервью](./7_oop_interview.md)                                    | Вопросы по OOP, ответы не заполнены.                                   |
| [API Design — собеседование](./0_sd_api_design_interview.md) | Вопросы по API Design, ответы не заполнены. |
| [Databases — собеседование](./0_sd_databases_interview.md) | Вопросы по Databases, ответы не заполнены. |
| [Microservices — собеседование](./0_sd_microservices_interview.md) | Вопросы по Microservices, ответы не заполнены. |
| [Availability & Reliability — собеседование](./0_sd_availability_interview.md) | Вопросы по Availability & Reliability, ответы не заполнены. |
| [Docker — собеседование](./0_sd_docker_interview.md) | Вопросы по Docker, ответы не заполнены. |
| [NoSQL — собеседование](./0_sd_nosql_interview.md) | Вопросы по NoSQL, ответы не заполнены. |
| [Caching — собеседование](./0_sd_caching_interview.md) | Вопросы по Caching, ответы не заполнены. |
| [Domain-Driven Design — собеседование](./0_sd_ddd_interview.md) | Вопросы по Domain-Driven Design, ответы не заполнены. |
| [Reactive Systems — собеседование](./0_sd_reactive_interview.md) | Вопросы по Reactive Systems, ответы не заполнены. |
| [Reactive Programming — собеседование](./0_sd_reactive_programming_interview.md) | Вопросы по Reactive Programming, ответы не заполнены. |
| [CAP Theorem — собеседование](./0_sd_cap_interview.md) | Вопросы по CAP Theorem, ответы не заполнены. |
| [Kubernetes — собеседование](./0_sd_kubernetes_interview.md) | Вопросы по Kubernetes, ответы не заполнены. |
| [SOA — собеседование](./0_sd_soa_interview.md) | Вопросы по SOA, ответы не заполнены. |
| [Concurrency — собеседование](./0_sd_concurrency_interview.md) | Вопросы по Concurrency, ответы не заполнены. |
| [Layering & Middleware — собеседование](./0_sd_layering_interview.md) | Вопросы по Layering & Middleware, ответы не заполнены. |
| [Software Architecture — собеседование](./0_sd_architecture_interview.md) | Вопросы по Software Architecture, ответы не заполнены. |
| [Cryptography — собеседование](./0_sd_cryptography_interview.md) | Вопросы по Cryptography, ответы не заполнены. |
| [Load Balancing — собеседование](./0_sd_load_balancing_interview.md) | Вопросы по Load Balancing, ответы не заполнены. |
| [XML — собеседование](./0_sd_xml_interview.md) | Вопросы по XML, ответы не заполнены. |
| [Hadoop — собеседование](./0_sd_hadoop_interview.md) | Вопросы по Hadoop, ответы не заполнены. |
| [Apache Spark — собеседование](./0_sd_spark_interview.md) | Вопросы по Apache Spark, ответы не заполнены. |


### Interview practice

Вопросы и ответы находятся в теме интервью по этой теме.

---

## 8. Resources

<details>
  <summary>Primary references</summary>

- [ByteByteGo — 20 лучших концепций системного проектирования] (https://blog.bytebytego.com/p/ep160-top-20-system-design-concepts)
- [ByteByteGo — шаблоны ООП-проектирования] (https://blog.bytebytego.com/i/162154465/oop-design-patterns-you-must-know)
- [Гуру рефакторинга — Каталог шаблонов проектирования](https://refactoring.guru/design-patterns/catalog)\* [Doka — Архитектура и шаблоны проектирования](https://doka.guide/tools/architecture-and-design-patterns/)
- [Doka — Поведенческие шаблоны](https://doka.guide/tools/design-patterns-behaviorial/)
- [Дока — ООП](https://doka.guide/tools/oop/)
- [Функциональный дизайн](https://feature-sliced.design/)
- [SOLID — freeCodeCamp](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/)\* [Инверсия зависимостей — Stackify](https://stackify.com/dependent-inversion-principle/)
- [SOLID в JavaScript](https://javascript.plainenglish.io/empowering-experience-engineers-with-solid-principles-016f8f1f0ed4)
- [Шаблоны дизайна — Банда четырёх (1994)](https://en.wikipedia.org/wiki/Design_Patterns)\* [Кальсадо — лучшие друзья](https://philcalcado.com/2015/09/18/the_back_end_for_front_end_pattern_bff.html) · [Ньюман — лучшие друзья](https://samnewman.io/patterns/architectural/bff/)
- [Канджария — шаблоны архитектуры внешнего интерфейса](https://medium.com/@TusharKanjariya/every-frontend-architecture-pattern-explained-a7ec07e7bbbb) (рамка интервью; факты перепроверены с документацией Next.js/Vercel/React)

</details>

<details>
  <summary>Related repo docs</summary>

### Related repo docs

- [Вопросы для собеседования](./0_interview.md) — ООП, SOLID, DRY, KISS, YAGNI · [Практика собеседования](./0_system_design_interview.md#interview-practice)
- [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md) · [Специальные — ООП и SOLID](./7_special_engineering.md#объектно-ориентированное-программирование-oop)
- [React — экосистема и архитектура](./4_react.md)
- [Next.js — SSR, SSG, ISR, PPR, среда выполнения](./4_nextjs.md)\* [Сборка и развертывание — monolith, nginx, RabbitMQ, Docker](./10_build.md)
- [Система решения проблем](./0_problem.md)

</details>

---

<a name="interview-practice"></a>

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Вопросы от [0_interview.md](0_interview.md). Попробуйте ответить, прежде чем открывать решения.

### Question index

| Тема                                                          | Ответ            |
| ------------------------------------------------------------- | ---------------- |
| Теорема CAP                                                   | Вопросы и ответы |
| Балансировка нагрузки                                         | Вопросы и ответы |
| Кэширование слоев                                             | Вопросы и ответы |
| Очереди сообщений                                             | Вопросы и ответы |
| Событийно-ориентированная архитектура                         | Вопросы и ответы |
| Узор Сага                                                     | Вопросы и ответы |
| Шаблон Медиатор                                               | Вопросы и ответы |
| Дизайн сокращения URL-адресов                                 | Задача 1         |
| Почему бэкенд/клиент-сервер                                   | Вопросы и ответы |
| BFF против одного API                                         | Вопросы и ответы |
| API-шлюз против BFF                                           | Вопросы и ответы |
| ССР против SSG против ISR                                     | Вопросы и ответы |
| Краевой рендеринг                                             | Вопросы и ответы |
| Когда микро-интерфейсы                                        | Вопросы и ответы |
| Монолит против микросервисов                                  | Вопросы и ответы |
| Архитектура против шаблонов                                   | Вопросы и ответы |
| Трехслойная/Чистая архитектура                                | Вопросы и ответы |
| Поведенческие модели (Стратегия/CoR/Командование/Наблюдатель) | Вопросы и ответы |
| Что такое классы/внедрение конструктора                       | Вопросы и ответы |

---
