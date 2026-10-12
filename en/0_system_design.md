# System design — hub

Foundations, distributed systems, frontend architecture, principles, and patterns. Theory lives in the topic files below.

### Related docs

[System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [System design — Interview](./0_system_design_interview.md)

### Navigation

| Topic                                                             | Covers                                                           |
| ----------------------------------------------------------------- | ---------------------------------------------------------------- |
| [System design — Core](./0_system_design_core.md)                 | Foundations and distributed-systems basics.                      |
| [System design — Architecture](./0_system_design_architecture.md) | Architecture styles and where HTML is built.                     |
| [Architecture patterns](./7_special_architecture.md)              | Composition, MVC family, layers/DDD, and frontend structure.     |
| [System design — Data](./0_system_design_data.md)                 | Data, caching, and infrastructure building blocks.               |
| [SOLID](./7_special_engineering.md)                               | Five OOP design principles for modules and APIs.                 |
| [DRY, KISS, YAGNI](./0_system_design_principles.md)               | Don't Repeat Yourself, Keep It Simple, You Aren't Gonna Need It. |
| [GRASP](./0_system_design_grasp.md)                               | Nine patterns for assigning responsibilities to objects.         |
| [Design patterns](./7_special_patterns.md)                        | GoF catalog: creational, structural, and behavioral patterns.    |
| [System design — Interview](./0_system_design_interview.md)       | Interview Q&A for system design.                                 |
| [OOP — Interview](./7_oop_interview.md)                           | OOP interview questions, answers not filled in.                  |
| [API Design — Interview](./0_sd_api_design_interview.md) | API Design interview questions, answers not filled in. |
| [Databases — Interview](./0_sd_databases_interview.md) | Databases interview questions, answers not filled in. |
| [Microservices — Interview](./0_sd_microservices_interview.md) | Microservices interview questions, answers not filled in. |
| [Availability & Reliability — Interview](./0_sd_availability_interview.md) | Availability & Reliability interview questions, answers not filled in. |
| [Docker — Interview](./0_sd_docker_interview.md) | Docker interview questions, answers not filled in. |
| [NoSQL — Interview](./0_sd_nosql_interview.md) | NoSQL interview questions, answers not filled in. |
| [Caching — Interview](./0_sd_caching_interview.md) | Caching interview questions, answers not filled in. |
| [Domain-Driven Design — Interview](./0_sd_ddd_interview.md) | Domain-Driven Design interview questions, answers not filled in. |
| [Reactive Systems — Interview](./0_sd_reactive_interview.md) | Reactive Systems interview questions, answers not filled in. |
| [Reactive Programming — Interview](./0_sd_reactive_programming_interview.md) | Reactive Programming interview questions, answers not filled in. |
| [CAP Theorem — Interview](./0_sd_cap_interview.md) | CAP Theorem interview questions, answers not filled in. |
| [Kubernetes — Interview](./0_sd_kubernetes_interview.md) | Kubernetes interview questions, answers not filled in. |
| [SOA — Interview](./0_sd_soa_interview.md) | SOA interview questions, answers not filled in. |
| [Concurrency — Interview](./0_sd_concurrency_interview.md) | Concurrency interview questions, answers not filled in. |
| [Layering & Middleware — Interview](./0_sd_layering_interview.md) | Layering & Middleware interview questions, answers not filled in. |
| [Software Architecture — Interview](./0_sd_architecture_interview.md) | Software Architecture interview questions, answers not filled in. |
| [Cryptography — Interview](./0_sd_cryptography_interview.md) | Cryptography interview questions, answers not filled in. |
| [Load Balancing — Interview](./0_sd_load_balancing_interview.md) | Load Balancing interview questions, answers not filled in. |
| [XML — Interview](./0_sd_xml_interview.md) | XML interview questions, answers not filled in. |
| [Hadoop — Interview](./0_sd_hadoop_interview.md) | Hadoop interview questions, answers not filled in. |
| [Apache Spark — Interview](./0_sd_spark_interview.md) | Apache Spark interview questions, answers not filled in. |


### Interview practice

Q&A lives in the Interview topic for this subject.

---

## 8. Resources

<details>
  <summary>Primary references</summary>

- [ByteByteGo — Top 20 System Design Concepts](https://blog.bytebytego.com/p/ep160-top-20-system-design-concepts)
- [ByteByteGo — OOP Design Patterns](https://blog.bytebytego.com/i/162154465/oop-design-patterns-you-must-know)
- [Refactoring Guru — Design Patterns Catalog](https://refactoring.guru/design-patterns/catalog)
- [Doka — Architecture and design patterns](https://doka.guide/tools/architecture-and-design-patterns/)
- [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)
- [Doka — OOP](https://doka.guide/tools/oop/)
- [Feature-Sliced Design](https://feature-sliced.design/)
- [SOLID — freeCodeCamp](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/)
- [Dependency Inversion — Stackify](https://stackify.com/dependency-inversion-principle/)
- [SOLID in JavaScript](https://javascript.plainenglish.io/empowering-experience-engineers-with-solid-principles-016f8f1f0ed4)
- [Design Patterns — Gang of Four (1994)](https://en.wikipedia.org/wiki/Design_Patterns)
- [Calçado — BFF](https://philcalcado.com/2015/09/18/the_back_end_for_front_end_pattern_bff.html) · [Newman — BFFs](https://samnewman.io/patterns/architectural/bff/)
- [Kanjariya — frontend architecture patterns](https://medium.com/@TusharKanjariya/every-frontend-architecture-pattern-explained-a7ec07e7bbbb) (interview frame; facts cross-checked with Next.js / Vercel / React docs)

</details>

<details>
  <summary>Related repo docs</summary>

### Related repo docs

- [Interview questions](./0_interview.md) — OOP, SOLID, DRY, KISS, YAGNI · [Interview Practice](./0_system_design_interview.md#interview-practice)
- [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [Special — OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop)
- [React — Ecosystem & architecture](./4_react.md)
- [Next.js — SSR, SSG, ISR, PPR, runtimes](./4_nextjs.md)
- [Build & deploy — monolith, nginx, RabbitMQ, Docker](./10_build.md)
- [Problem solving framework](./0_problem.md)

</details>

---

<a name="interview-practice"></a>

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Questions from [0_interview.md](0_interview.md). Try answering before opening solutions.

### Question index

| Topic                                                     | Answer |
| --------------------------------------------------------- | ------ |
| CAP theorem                                               | Q&A    |
| Load balancing                                            | Q&A    |
| Caching layers                                            | Q&A    |
| Message queues                                            | Q&A    |
| Event-driven architecture                                 | Q&A    |
| Saga pattern                                              | Q&A    |
| Mediator pattern                                          | Q&A    |
| URL shortener design                                      | Task 1 |
| Why a backend / client–server                             | Q&A    |
| BFF vs one API                                            | Q&A    |
| API Gateway vs BFF                                        | Q&A    |
| SSR vs SSG vs ISR                                         | Q&A    |
| Edge rendering                                            | Q&A    |
| When micro-frontends                                      | Q&A    |
| Monolith vs microservices                                 | Q&A    |
| Architecture vs patterns                                  | Q&A    |
| Three-layer / Clean Architecture                          | Q&A    |
| Behavioral patterns (Strategy / CoR / Command / Observer) | Q&A    |
| What are classes / constructor injection                  | Q&A    |

---
