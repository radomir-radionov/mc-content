# GRASP

Nine responsibility-assignment patterns (Craig Larman) for deciding which class owns which job.

[← Back to hub](./0_system_design.md)

### Related docs

- [System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [System design — Interview](./0_system_design_interview.md) · [Computer science](./7_special_cs.md)

### Main concepts

| Area       | Topics                                                            |
| ---------- | ----------------------------------------------------------------- |
| Assignment | Expert, Creator, Controller                                       |
| Quality    | Low Coupling, High Cohesion                                       |
| Variation  | Polymorphism, Indirection, Protected Variations, Pure Fabrication |

### Sections

- [GRASP](#grasp)

---

## GRASP

<details>
  <summary>GRASP principles</summary>

**Definition:** General Responsibility Assignment Software Patterns — nine principles for assigning responsibilities to classes and objects (Craig Larman).

| Principle                | Definition                                                                         | When to apply                                    |
| ------------------------ | ---------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Expert**               | Assign responsibility to the class that has the information needed to fulfill it   | Order calculates its own total                   |
| **Creator**              | Class A creates class B if A contains/records/uses/closely relates to B            | Order creates OrderLine                          |
| **Controller**           | Object handles system events, delegating to domain objects                         | OrderController handles HTTP, delegates to Order |
| **Low Coupling**         | Minimize dependencies between classes                                              | Inject services, avoid direct concrete deps      |
| **High Cohesion**        | Keep related responsibilities together in one class                                | PaymentValidator validates, doesn't send emails  |
| **Polymorphism**         | Use polymorphism for type-varying behavior                                         | PaymentProcessor interface, not switch on type   |
| **Indirection**          | Assign responsibility to an intermediate object to decouple                        | Repository between service and DB                |
| **Protected Variations** | Shield elements from changes behind a stable interface                             | Adapter around a third-party API                 |
| **Pure Fabrication**     | Create a class that is not a domain concept to keep coupling low and cohesion high | Repository, Factory, DTO                         |

**When to use:** Responsibility-assignment questions (“who owns this?”). Skip as a checklist on a 50-line script.

**Thoughts:** GRASP is more practical than memorizing all 23 GoF patterns for day-to-day design. Expert + Creator + Controller cover most assignment questions. Domain modeling below. GoF catalog: [Design patterns](./7_special_patterns.md#gof-design-patterns--catalog).

**Sources:** [Wikipedia — GRASP](<https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)>)

### Entities, relations & design

**Definition:** Design decides two things: how the program talks to the outside world (data in/out), and how **entities** talk to each other. Entities are the main actors; their relations are the business rules.

**How it works:**

- Start from the **goal**, then name entities. Budget app: `Budget`, `History`, `Record` (a spend or income).
- Draw **relations** (UML): History _contains_ Records; DailyAmount _depends on_ Budget + History. Arrow direction = who depends on whom — keep it matching the domain, avoid cycles.
- Express entities as **objects that send and receive messages** (method calls), not as loose bags of data.
- A class that needs other objects is a **dependent**. Popular strategy: **constructor injection** — pass collaborators in, store them as fields.

Проектирование = внешний мир + общение сущностей. Стрелки UML — кто от кого зависит. Внедрение через конструктор соединяет сущности без циклов.

**When to use:** Any domain with more than one concept that must stay consistent (budget + ledger, order + lines, cart + items). Skip heavy diagrams for a one-file script.

**Pluses and minuses:**

| +                                             | −                                                        |
| --------------------------------------------- | -------------------------------------------------------- |
| Code structure matches the domain             | Upfront modeling cost                                    |
| Changes stay in the entity that owns the rule | Easy to invent entities that don't exist in the business |
| Dependency arrows make cycles visible         | UML can outgrow the actual problem                       |

**Examples:**

```js
class Budget {
  constructor(amount, daysCount) {
    this.amount = amount;
    this.startDate = Date.now();
    this.daysCount = daysCount;
  }
  get perDay() {
    return this.amount / this.daysCount;
  }
}

class History {
  records = [];
  addRecord(record) {
    this.records.push(record);
  }
  get spentToday() {
    return this.records.reduce((sum, r) => sum + r.amount, 0); // filter with isToday(r.dateTime) in real code
  }
}

class DailyAmount {
  constructor(budget, history) {
    this.budget = budget;
    this.history = history;
  }
  get valueOf() {
    return this.budget.perDay - this.history.spentToday;
  }
}
```

**Thoughts:** `DailyAmount` does not create Budget/History — it receives them. That keeps dependency direction one-way and makes tests swap fakes. Full class syntax: [Классы](./3_js_classes.md). SOLID mapping: [§9](./7_special_engineering.md#9-oop-and-solid).

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/)

</details>
