# OOP & SOLID

OOP pillars and the five SOLID principles for modules and APIs.

[← Back to hub](./0_system_design.md)

### Related docs

- [System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [System design — Interview](./0_system_design_interview.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md)

### Sections

- [9. OOP & SOLID](#9-oop-and-solid)

## 9. OOP & SOLID

<details>
  <summary>Object-Oriented Programming (OOP)</summary>

### Object-Oriented Programming (OOP)

**Definition:** OOP is a programming paradigm that models programs as **objects** — live instances built from a **class** (blueprint). Each object bundles **state** (fields) and **behavior** (methods). OOP is based on four key concepts — encapsulation, inheritance, polymorphism, abstraction. In JavaScript, `class` is syntactic sugar over prototypes — see [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**How it works:**

| Step        | Idea                                                             |
| ----------- | ---------------------------------------------------------------- |
| **Class**   | Blueprint — defines fields and methods once                      |
| **Object**  | Instance — `new Animal("Rex")` creates its own copy of state     |
| **Message** | Call a method — `dog.speak()`; the object decides how to respond |

Four **pillars** explain how objects cooperate:

| Pillar            | What it means                                                                                               |
| ----------------- | ----------------------------------------------------------------------------------------------------------- |
| **Encapsulation** | Hiding internal logic and data from direct external access                                                  |
| **Inheritance**   | Child class from parent — reuse shared code without duplication                                             |
| **Polymorphism**  | Objects with different structure use the same method name; behavior depends on context                      |
| **Abstraction**   | Highlight what matters; hide implementation — e.g. `video.play()` without knowing how the stream is decoded |

Focused essays: [Encapsulation](#encapsulation) · [Inheritance](#inheritance) · [Polymorphism](#polymorphism) · [Abstraction](#abstraction).

JS code (`#` fields, `extends`/`super()`, `speak()` polymorphism, `video.play()`): [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**Composition vs inheritance:** a smart kettle with a timer is **has-a** (`this.kettle`, `this.timer`), not **is-a** teapot _and_ timer. JavaScript allows only single `extends` — when “both parent types” apply, compose objects instead.

**When to use:** Rich domain models (users, orders, ledgers), UI component trees, SDKs with shared base types. Prefer functions/modules for one-off scripts or mostly data pipelines.

**Pluses and minuses:**

| +                                                       | −                                                                                       |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Entities and relations map cleanly to business language | Shared mutable state + many writers → races (locks, CRDT, or immutability still needed) |
| UML, ER, DFD grew around object thinking                | Shallow “is-a” trees rarely match real “has both” objects                               |
| Well-studied patterns and failure modes                 | Deep hierarchies become hard to change (fragile base class)                             |

**Thoughts:** This page covers **what** OOP is and **when** to use it; JS syntax and pillar code examples live in [OOP in JavaScript](./3_js_objects.md#oop-in-javascript). Encapsulation does not fix concurrent edits (two users, one doc) — you still need OT/CRDT or locks. Good code is a **map of the territory**; OOP helps when entities and messages stay aligned with that map.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/) · [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript) · [Classes (TypeScript)](./5_ts_classes.md)

---

</details>

<details>
  <summary>Encapsulation</summary>

### Encapsulation

**Definition:** Hiding an object's internal logic and data from direct external access. Outside code uses a small API; state changes only through methods that enforce rules.

**How it works:**

- **Object level:** data and the only legal ways to change it live inside one object — callers call `deposit()`, not `account.balance = -100`.
- **Module level:** a `History` class hides how records are stored; callers use `addRecord()`, not `history.records.push(...)`. Change storage later without rewriting every caller.
- **In JS:** `#` creates fully private fields; closures and ES modules also hide internals. See [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** Domain objects with invariants (balance, permissions, cart totals). Any type with more than one instance — prefer methods over public arrays/maps.

**Pluses and minuses:**

| +                                         | −                                          |
| ----------------------------------------- | ------------------------------------------ |
| One place to change storage or rules      | Over-private APIs hinder testing           |
| Callers stay stable when internals change | Getters/setters can leak the model         |
| Prevents accidental mutation              | Easy to wrap every field and call it "OOP" |

**Examples:**

```js
// bad: public field — caller bypasses validation
class BankAccount {
  balance = 0;
}
const account = new BankAccount();
account.balance = -100; // no rules, invariant broken

// good: private state; only methods can change it
class BankAccount {
  #balance = 0;

  constructor(owner) {
    this.owner = owner;
  }

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount("Ivan");
account.deposit(100);
console.log(account.getBalance()); // 100
// account.#balance → SyntaxError
```

**Thoughts:** `public` fields in TS/JS are convention-only unless `#` or closures enforce privacy. Chrome DevTools can read `#` in the console for debugging — that is **not** app-code access.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Inheritance</summary>

### Inheritance

**Definition:** A subclass acquires properties and methods from a parent (superclass), enabling code reuse and specialization. Lets you create a child class from a parent without duplicating shared code.

**How it works:**

- Inheritance is **is-a**: every `Manager` _is_ an `Employee`.
- If something is both a kettle and a timer — prefer **has-a** (composition), not two parents.
- In JavaScript: `extends` links prototypes; `super()` calls the parent constructor or method. See [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** True **is-a** relationships with stable base behavior. Prefer shallow hierarchies (≤2 levels).

**Pluses and minuses:**

| +                       | −                                                   |
| ----------------------- | --------------------------------------------------- |
| DRY for shared behavior | Tight coupling to parent implementation             |
| Natural "is-a" modeling | LSP violations when subclass breaks parent contract |

**Examples:**

```js
// bad: duplicate name — change Employee constructor → fix Manager by hand
class Employee {
  constructor(name) {
    this.name = name;
  }
  getRole() {
    return "employee";
  }
}
class Manager {
  constructor(name) {
    this.name = name;
  } // copied
  getRole() {
    return "manager";
  }
}

// good: shared code in parent; child calls super() and adds only what differs
class Employee {
  constructor(name) {
    this.name = name;
  }
  getRole() {
    return "employee";
  }
}
class Manager extends Employee {
  constructor(name, department) {
    super(name);
    this.department = department;
  }
  getRole() {
    return "manager";
  }
}

const bob = new Manager("Bob", "Engineering");
console.log(bob.name); // inherited from Employee
console.log(bob.department); // Manager-only field
```

Polymorphism (`staff.map(e => e.getRole())`) — see [Polymorphism](#polymorphism). JS `Dog extends Animal` with `super()`: [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**Thoughts:** Favor composition when behavior varies by combination, not taxonomy. "Prefer composition over inheritance" — Gang of Four.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Polymorphism</summary>

### Polymorphism

**Definition:** Objects with different structure can use methods with the same name that behave differently depending on context. Callers depend on a shared shape (method name), not a concrete class.

| Type                      | Meaning                                                            |
| ------------------------- | ------------------------------------------------------------------ |
| **Subtype (inheritance)** | Subclass usable wherever parent is expected                        |
| **Parametric (generics)** | Same code works on many types (`T[]`)                              |
| **Ad-hoc (overloading)**  | Same name, different signatures (limited in JS)                    |
| **Structural (JS/TS)**    | If it has the methods, it fits — duck typing / TS structural types |

**How it works:**

- One call site — different behavior per object: `animals.forEach(a => a.speak())`.
- Code against a **shared contract** (`speak()`, `send()`, `render()`), not `if (type === "dog")`.
- **In JS:** no `interface` at runtime — duck typing, JSDoc, or TypeScript. Full examples: [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** Strategy-like swaps; plugin/notifier lists; rendering different component types from one array; any time a `switch` on type keeps growing.

**Pluses and minuses:**

| +                                                     | −                                                       |
| ----------------------------------------------------- | ------------------------------------------------------- |
| Fewer `if/else` on concrete types                     | Need a real shared contract, not a random `type` string |
| Lower coupling — change one variant, not every caller | JS won't check the contract at runtime                  |
| Plugin lists, renderers, strategies                   | Fake "polymorphism" via flags can become a god `switch` |

**Examples:**

```js
// bad: switch grows with every new type — edit every caller
function animalSound(type, name) {
  if (type === "dog") console.log(`${name} barks: Woof!`);
  else if (type === "cat") console.log(`${name} meows: Meow!`);
  // add Bird → another branch here and in every copy
}

// good: same method call — new class = new behavior, callers unchanged
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  speak() {
    console.log(`${this.name} (breed: ${this.breed}) barks: Woof!`);
  }
}

class Cat extends Animal {
  speak() {
    console.log(`${this.name} meows: Meow!`);
  }
}

const animals = [new Dog("Rex", "Shepherd"), new Cat("Fluffy")];
animals.forEach((animal) => animal.speak());
// Rex (breed: Shepherd) barks: Woof!
// Fluffy meows: Meow!
```

**Thoughts:** Prefer a real `interface` in TS when variants diverge in behavior. Subtype polymorphism needs a sensible parent — see [Inheritance](#inheritance). Composition + contract is often safer than deep trees.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Abstraction</summary>

### Abstraction

**Definition:** Highlighting what matters and hiding implementation details. We create an interaction interface without forcing the user to understand complex internal mechanisms.

**How it works:**

- Callers use a **small, stable API** — they ask _what_ (`play()`, `charge()`, `getUser()`), not _how_ (decode stream, Stripe HTTP, SQL queries).
- Internals can change (PayPal instead of Stripe, IndexedDB instead of `localStorage`) while the public API stays the same.
- Too **few** layers leak complexity; too **many** obscure the flow — both hurt.
- **In JS:** no built-in `interface` at runtime — classes, modules, or functions hide details. See [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** APIs, repositories, storage adapters, payment providers, any boundary where callers should not depend on implementation.

**Pluses and minuses:**

| +                                | −                                      |
| -------------------------------- | -------------------------------------- |
| Simplifies mental model          | Wrong abstraction is expensive to undo |
| Enables swapping implementations | Too many layers obscure flow           |

**Examples:**

```js
// bad: caller knows Stripe URL, auth header, and payload shape
async function checkout(cart) {
  const res = await fetch("https://api.stripe.com/v1/charges", {
    method: "POST",
    headers: { Authorization: "Bearer sk_..." },
    body: JSON.stringify({ amount: cart.total * 100, currency: "usd" }),
  });
  return res.json();
}

// good: checkout asks _what_ (charge total); gateway hides _how_
class PaymentGateway {
  async charge(amount) {
    return fetch("/api/charge", {
      method: "POST",
      body: JSON.stringify({ amount }),
    });
  }
}
async function checkout(cart, gateway) {
  return gateway.charge(cart.total); // swap Stripe → PayPal without touching checkout
}

// good: video.play() — no decode/buffer details
const video = document.querySelector("video");
video.play();
```

**Thoughts:** `interface` in TS is compile-time abstraction. In JS, document contracts via JSDoc or tests. Do not confuse abstraction with encapsulation — encapsulation hides _state_; abstraction hides _how work is done_.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Cohesion vs coupling</summary>

### Cohesion vs coupling

**Definition:** **Cohesion** (связность) is how closely a module's internals belong together. **Coupling** (сцепление) is how much one module knows about another. Want high cohesion, low coupling.

**How it works:**

- Encapsulation raises cohesion: spent-today lives on History, per-day on Budget — `DailyAmount` only subtracts.
- Polymorphism lowers coupling: callers depend on a contract, not on `Spending` vs `Income`.
- Duplicate "spent today" logic in three files is low cohesion spread thin — a rule change misses a copy.

|              | High (good / bad)                         | Low                                                      |
| ------------ | ----------------------------------------- | -------------------------------------------------------- |
| **Cohesion** | Good — related code in one module         | Bad — a class does budget _and_ HTTP _and_ UI            |
| **Coupling** | Bad — many modules break when one changes | Good — swap History storage without touching DailyAmount |

Связность — близкие задачи в одном модуле. Сцепление / зацепление — сколько модули знают друг о друге. Цель: высокая связность, низкое сцепление. High coupling → many mocks just to boot a unit test; see [Making tests easy](./7_special_testing_practices.md#making-tests-easy).

**When to use:** Refactor when you copy the same formula; when a UI tweak forces a domain-class edit.

**Thoughts:** Hide internals (encapsulation) + talk through contracts (polymorphism) = cohesive modules that barely know each other.

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/)

---

</details>

<details>
  <summary>Composition vs inheritance</summary>

### Composition vs inheritance

**Definition:** **Inheritance** builds "is-a" hierarchies. **Composition** builds "has-a" by embedding objects and delegating behavior.

**How it works:**

Inheritance designs **from general to specific** (Animal → Dog). Composition designs **from parts to whole** (Human = skeleton + immune + cardio). Feature sets group into contracts; a class picks the set it needs — no single taxonomy. Biology has several overlapping trees; software usually does too.

Наследование — «является» (иерархия). Композиция — «имеет» (набор фич). Иерархий может быть несколько сразу — композиция не ломается.

**When to use:** Default to composition. Inheritance only for stable, shallow "is-a" (e.g. `Error` subclasses).

**Pluses and minuses:**

| + (composition)                           | −                           |
| ----------------------------------------- | --------------------------- |
| Loose coupling; swap behaviors at runtime | More boilerplate wiring     |
| Avoids fragile base class problem         | Many small objects to track |
| Aligns with LSP and OCP                   |                             |

| + (inheritance)                   | −                                |
| --------------------------------- | -------------------------------- |
| Fast to prototype shared behavior | Changes in base break subclasses |
| Clear taxonomy                    | Deep trees resist refactoring    |

**Examples:**

```js
// Composition — mix feature objects (JS has no `implements`)
const cardio = {
  pumpBlood() {},
};
const immune = {
  fightInfection() {},
};

const human = { ...cardio, ...immune };
const worm = { ...cardio, ...immune }; // same features, no shared parent class
```

**Thoughts:** React favors composition (`children`, render props, hooks). SOLID: OCP via composition; LSP applies to inheritance. Mixins/spread work but stay untyped without TS interfaces. See [Mixins](./3_js_classes.md#mixins).

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/) · [refactoring.guru — composition over inheritance](https://refactoring.guru/design-patterns/composite)

---

</details>

<details>
  <summary>SOLID (overview)</summary>

### SOLID (overview)

**Definition:** **SOLID** — five design principles from Robert C. Martin ("Uncle Bob") for code that stays easy to **change, test, and extend**. Named in OOP, they apply to **classes**, **modules**, and **functions**. A **guide when change pain shows up** — not a rule that every short script needs five abstractions.

**How it works:**

| Letter | Principle             | Plain one-liner                                   |
| ------ | --------------------- | ------------------------------------------------- |
| **S**  | Single Responsibility | One module / class / function = one job           |
| **O**  | Open/Closed           | Extend with **new** code — don't edit stable code |
| **L**  | Liskov Substitution   | Every subtype must work wherever the parent works |
| **I**  | Interface Segregation | Don't expose methods callers never use            |
| **D**  | Dependency Inversion  | Pass dependencies in — don't hardcode them inside |

Each letter fixes a different coupling problem. Per-principle essays on this page: [SRP](#single-responsibility-srp) · [OCP](#open-closed-ocp) · [LSP](#liskov-substitution-lsp) · [ISP](#interface-segregation-isp) · [DIP](#dependency-inversion-dip).

**When to use:** God classes or god modules; plugin-style providers; code review; interviews. Skip heavy abstraction on a one-off script.

**Pluses and minuses:**

| +                                           | −                                           |
| ------------------------------------------- | ------------------------------------------- |
| Easier testing, refactoring, team ownership | Over-application creates abstraction sprawl |
| Shared vocabulary in code reviews           | "SOLID" debates distract from shipping      |
| Scales with codebase size                   | Subjective boundaries (especially SRP)      |

**Examples:**

```js
// S — good: engine logic separate from dashboard UI
class CarEngine {
  start() {
    return "engine on";
  }
}

class CarDashboard {
  render({ speed }) {
    return `${speed} km/h`;
  }
}

// S — bad: one class mixes engine + UI (two reasons to change)
// class Car {
//   start() { return "engine on"; }
//   renderDashboard() { return `${this.speed} km/h`; }
// }

// O — good: new engine = new class, Car unchanged
class GasEngine {
  start() {
    return "gas started";
  }
}

class Car {
  constructor(engine) {
    this.engine = engine;
  }
  start() {
    return this.engine.start();
  }
}

new Car(new GasEngine());

// O — bad: edit Car.start() for every new engine type
// class Car {
//   start() {
//     if (this.type === "gas") return "gas started";
//     if (this.type === "electric") return "motor started";
//   }
// }

// L — good: goToWork(car) works for any car that returns a result
function goToWork(car) {
  return car.drive(10);
}

class RoadCar {
  drive(km) {
    return { km, status: "ok" };
  }
}

goToWork(new RoadCar());

// L — bad: subclass throws — goToWork crashes without try/catch
class OutOfFuelCar extends RoadCar {
  drive(km) {
    throw new Error("No fuel");
  }
}
// goToWork(new OutOfFuelCar());

// I — good: showroom lists; workshop repairs — separate APIs
class CarLot {
  listAvailable() {
    return ["sedan", "suv"];
  }
}

class CarWorkshop {
  repair(carId) {
    return `repaired ${carId}`;
  }
}

// I — bad: lot forced to stub repair() it never uses
// class Garage {
//   listAvailable() { return ["sedan", "suv"]; }
//   repair() { throw new Error("We don't repair here"); }
// }
// class CarLot extends Garage {}

// D — good: inject logger — business logic does not own console
function processOrder(order, logger) {
  logger.log(`Processing ${order.id}`);
}

// D — bad: hardcoded console inside domain code
// function processOrder(order) {
//   console.log(`Processing ${order.id}`);
// }
```

**Thoughts:** Stack-specific walkthroughs with `// good` / `// bad` for all five letters: [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Complements [DRY, KISS, YAGNI](./0_system_design_principles.md) — don't abstract until a second use case appears.

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/) · [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Single Responsibility (SRP)</summary>

### Single Responsibility (SRP)

**Definition:** A module (class, function, file) should have **one reason to change** — one cohesive job or axis of change.

**How it works:**

- Identify **who** would request changes: UI team, API team, billing rules, etc.
- If two unrelated stakeholders trigger edits in the same module, split it.
- SRP is about **cohesion**, not "one method per class" — a `UserValidator` with three validation methods still has one job.
- In React: separate data fetching, formatting, and presentation.

**When to use:** Split god components; `UserRepository` (DB) vs `UserPresenter` (UI); separate email template rendering from send logic; extract validation from route handlers.

**Pluses and minuses:**

| +                                       | −                                      |
| --------------------------------------- | -------------------------------------- |
| Easier test, refactor, and reason about | Over-splitting creates file sprawl     |
| Clear ownership per module              | "One reason" is subjective             |
| Smaller diffs, fewer merge conflicts    | Can feel bureaucratic for tiny scripts |

**Examples:**

```js
// ❌ Two reasons to change: DB schema AND API response shape
class UserService {
  async getUser(id) {
    const row = await db.query('SELECT * FROM users WHERE id = ?', [id]);
    return { fullName: `${row.first} ${row.last}`, avatar: row.img_url };
  }
}

// ✅ Split responsibilities
class UserRepository { findById(id) { return db.query(...); } }
class UserMapper { toDto(row) { return { fullName: `${row.first} ${row.last}`, ... }; } }
```

**Thoughts:**

Interview: SRP ≠ "one function" — it's one **axis of change**. A change in tax law shouldn't force editing UI code.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Open/Closed (OCP)</summary>

### Open/Closed (OCP)

**Definition:** Software entities should be **open for extension** but **closed for modification** — add behavior via new code, not by editing stable modules.

**How it works:**

- Define **extension points**: strategy interfaces, middleware hooks, plugin registries.
- New variants implement the interface or register with the registry.
- Core logic calls the abstraction — never a growing `if/else` or `switch` on types.
- In JS: object maps, higher-order functions, and middleware pipelines achieve OCP without inheritance.

**When to use:** Payment providers, export formats (CSV/JSON/PDF), validation rules, Express/Koa middleware, ESLint rules, React render props / compound components.

**Pluses and minuses:**

| +                                         | −                                        |
| ----------------------------------------- | ---------------------------------------- |
| Safer additions — existing code untouched | Abstraction upfront cost                 |
| Plugin and middleware architectures       | Wrong extension points are hard to fix   |
| Fewer regression bugs on extend           | Can over-engineer for one implementation |

**Examples:**

```js
// bad: edit checkout every time a new payment provider is added
function checkout(provider, amount) {
  if (provider === "stripe") return chargeStripe(amount);
  if (provider === "paypal") return chargePayPal(amount);
  // add Apple Pay → change this function again (open for modification)
}

// good: checkout stays closed; new providers register in the map
const paymentStrategies = {
  stripe: (amount) => chargeStripe(amount),
  paypal: (amount) => chargePayPal(amount),
};

function checkout(provider, amount) {
  const pay = paymentStrategies[provider];
  if (!pay) throw new Error("Unknown provider");
  return pay(amount);
}

// extend elsewhere — no edit to checkout
paymentStrategies.applePay = (amount) => chargeApplePay(amount);
```

**Thoughts:** OCP pairs with Strategy and Factory patterns. A `switch` on a closed enum is fine if the set is truly fixed and owned by one team.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Liskov Substitution (LSP)</summary>

### Liskov Substitution (LSP)

**Definition:** Subtypes must be **substitutable** for their base types — callers using the parent contract must not break when given a subclass.

**How it works:**

- Subclass must honor **preconditions** (not strengthen), **postconditions** (not weaken), and **invariants** of the parent.
- If code expects `Rectangle.setWidth(w)` to leave height unchanged, `Square extends Rectangle` violates LSP when `setWidth` also changes height.
- Prefer composition when "is-a" taxonomy doesn't hold behaviorally.
- In JS/TS: duck typing doesn't save you — behavioral contracts still matter.

**When to use:** Review any `extends` — does subclass throw on parent methods? Narrow input types? Return incompatible shapes? Plugin systems where plugins must not break host assumptions.

**Pluses and minuses:**

| +                                 | −                                                 |
| --------------------------------- | ------------------------------------------------- |
| Reliable polymorphism             | Violations are subtle and discovered late         |
| Safe refactoring with inheritance | Pushes toward composition (good, but more wiring) |
| Clear contracts in hierarchies    | Hard to formalize in untyped JS                   |

**Examples:**

```js
// bad: Penguin is-a Bird but breaks callers that expect fly() to work
class Bird {
  fly() {
    return "flying";
  }
}
class Penguin extends Bird {
  fly() {
    throw new Error("Cannot fly");
  }
}

function makeBirdFly(bird) {
  return bird.fly(); // works for Bird, throws for Penguin — LSP violation
}
makeBirdFly(new Penguin());

// good: separate capability — caller depends only on what it needs
class Penguin {
  swim() {
    return "swimming";
  }
}

function makeSwimmer(animal) {
  return animal.swim();
}
makeSwimmer(new Penguin());
```

**Thoughts:** Classic interview trap: `Square extends Rectangle` — `setWidth` on a square changes height too. Fix: shared `Shape` with `area()`, not a false **is-a** hierarchy.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Interface Segregation (ISP)</summary>

### Interface Segregation (ISP)

**Definition:** Clients should not be forced to depend on methods they don't use — prefer **small, focused interfaces** over fat ones.

**How it works:**

- Split large interfaces into role-specific ones (`Readable`, `Writable`, `Deletable`).
- Callers depend only on the methods they need — easier to mock and implement.
- In TS: `interface` segregation; in JS: pass minimal object shapes or use JSDoc `@typedef` for contracts.
- Fat React component props that force unused callbacks violate ISP spirit.

**When to use:** Split `File` god interface into `read()`, `write()`, `watch()`; API clients that only need `get` shouldn't require `delete`; segregate admin vs read-only service interfaces.

**Pluses and minuses:**

| +                                     | −                                        |
| ------------------------------------- | ---------------------------------------- |
| Smaller, focused contracts            | More types/interfaces to maintain        |
| Easier mocks in tests                 | Can fragment APIs if overdone            |
| Implementers only stub what they need | Discovery — "which interface do I need?" |

**Examples:**

```ts
// bad: fat DbClient — read-only code must depend on write/delete too
class DbClient {
  read(id: string) {
    /* ... */
  }
  write(data: Data) {
    /* ... */
  }
  delete(id: string) {
    /* ... */
  }
}

function generateReport(db: DbClient) {
  return db.read("summary"); // only needs read
}

// good: small contract — caller takes only what it uses
type Readable = { read(id: string): Promise<Data> };
type Writable = { write(data: Data): Promise<void> };

function generateReport(source: Readable) {
  return source.read("summary");
}

const readOnlyDb: Readable = { read: (id) => fetchReport(id) };
generateReport(readOnlyDb);
```

**Thoughts:** ISP reduces coupling between modules. In frontend, fat props that force unused callbacks are an ISP smell — pass only needed fields.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Dependency Inversion (DIP)</summary>

### Dependency Inversion (DIP)

**Definition:** High-level modules depend on **abstractions**; low-level modules **implement** those abstractions — both depend on interfaces, not on each other directly.

**How it works:**

1. High-level policy (`OrderService`) defines what it needs (`PaymentGateway`, `OrderRepo`).
2. Low-level details (`StripeGateway`, `PostgresOrderRepo`) implement those abstractions.
3. **Inject** dependencies via constructor, factory, or DI container — don't `import` concrete implementations inside business logic.
4. Tests swap in fakes/mocks at the abstraction boundary — kinds: [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies).

**When to use:** Inject `HttpClient`, `Storage`, `Logger`; repository pattern over raw ORM in services; React context/providers for testable dependencies; avoid `fetch` calls inside pure domain functions.

**Pluses and minuses:**

| +                                                | −                                   |
| ------------------------------------------------ | ----------------------------------- |
| Testable via mocks/stubs                         | Indirection and wiring overhead     |
| Swappable implementations (DB, payment, storage) | DI containers can hide dependencies |
| Business logic isolated from frameworks          | Learning curve for junior devs      |

**Examples:**

```ts
// bad: high-level code hard-coded to Postgres — hard to test or swap storage
import { PostgresUserRepo } from "./postgres";

function getUserProfile(id: string) {
  const repo = new PostgresUserRepo();
  return repo.findById(id);
}

// good: depend on abstraction; inject implementation at the edge
type UserRepo = { findById(id: string): Promise<User> };

function getUserProfile(id: string, repo: UserRepo) {
  return repo.findById(id);
}

// production: getUserProfile(id, new PostgresUserRepo())
// test:       getUserProfile(id, { findById: async () => mockUser })
```

**Thoughts:** DIP ≠ every function takes 10 params. Module-level defaults with injectable overrides is idiomatic in JS. Pairs with Factory and Repository patterns. Constructor injection works in plain JS too (`new DailyAmount(budget, history)`); typed abstractions help in TS. See [Entities, relations & design](./0_system_design_grasp.md#entities-relations--design).

**See also:** Swappable fakes at the interface = less mock archaeology — [Making tests easy](./7_special_testing_practices.md#making-tests-easy).

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/) · [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>
