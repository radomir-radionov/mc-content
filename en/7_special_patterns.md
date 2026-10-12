# Design patterns

GoF catalog: creational, structural, and behavioral patterns. SOLID, DRY/KISS/YAGNI, and GRASP live in sibling Principles topics. TDD/BDD: [Tests](./7_special_testing.md). Constructor injection: [Entities, relations & design](./0_system_design_grasp.md#entities-relations--design).

[← Back to hub](./0_system_design.md)

### Related docs

- [System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [System design — Interview](./0_system_design_interview.md)

### Main concepts

| Area         | Topics                                               |
| ------------ | ---------------------------------------------------- |
| Catalog      | 23 GoF patterns (creational, structural, behavioral) |
| Cheat sheets | Frontend-oriented grouping                           |
| Extra        | Repository (not GoF)                                 |

### Sections

- [10. Design patterns](#10-design-patterns)
- [GOF design patterns — catalog](#gof-design-patterns--catalog)
- [Creational patterns — cheat sheet](#creational-patterns--cheat-sheet)
- [Structural patterns — cheat sheet](#structural-patterns--cheat-sheet)
- [Behavioral patterns — cheat sheet](#behavioral-patterns--cheat-sheet)

---

<a name="10-design-patterns"></a>

## 10. Design patterns

<details>
  <summary>Design pattern (general)</summary>

### Design pattern (general)

**Definition:**

A reusable solution to a recurring design problem — not copy-paste code, but a structure and collaboration pattern.

| Category       | Examples                                                                 |
| -------------- | ------------------------------------------------------------------------ |
| **Creational** | Factory, Factory Method, Abstract Factory, Builder, Singleton, Prototype |
| **Structural** | Adapter, Decorator, Facade, Proxy                                        |
| **Behavioral** | Chain of Responsibility, Observer, Strategy, Command, Iterator           |

**When to use:** When problem matches catalog; name patterns in docs and reviews.

**Pluses and minuses:**

| +                        | −                                  |
| ------------------------ | ---------------------------------- |
| Shared vocabulary        | Over-application ("pattern fever") |
| Battle-tested structures | Can obscure simple code            |

**Thoughts:** See full catalog and frontend grouping in [GOF catalog](#gof-design-patterns--catalog). Responsibility assignment (Expert, Creator, Controller): [GRASP](./0_system_design_grasp.md#grasp-principles). Creational shortlist: [cheat sheet](#creational-patterns--cheat-sheet). Structural shortlist: [cheat sheet](#structural-patterns--cheat-sheet). Behavioral shortlist: [cheat sheet](#behavioral-patterns--cheat-sheet). Patterns are not the goal of a PR.

**Sources:**

[refactoring.guru](https://refactoring.guru/design-patterns/catalog) · [Doka — Architecture and design patterns](https://doka.guide/tools/architecture-and-design-patterns/)

---

</details>

<details>
  <summary>GOF design patterns — catalog</summary>

<a name="gof-design-patterns--catalog"></a>

### GOF design patterns — catalog

**Definition:**

Gang of Four (Gamma, Helm, Johnson, Vlissides) — 23 reusable OOP design patterns from _Design Patterns: Elements of Reusable Object-Oriented Software_ (1994).

| Creational       | Structural | Behavioral              |
| ---------------- | ---------- | ----------------------- |
| Factory Method   | Adapter    | Chain of Responsibility |
| Abstract Factory | Bridge     | Command                 |
| Builder          | Composite  | Interpreter             |
| Prototype        | Decorator  | Iterator                |
| Singleton        | Facade     | Mediator                |
|                  | Flyweight  | Memento                 |
|                  | Proxy      | Observer                |
|                  |            | State                   |
|                  |            | Strategy                |
|                  |            | Template Method         |
|                  |            | Visitor                 |

**Thoughts:**

Patterns are vocabulary, not goals. "We need a Factory" is the wrong starting point — identify the problem first, then name the pattern. Deep JS/Doka notes: [Creational cheat sheet](#creational-patterns--cheat-sheet).

Doka’s frontend shortlist (not the full 23):

| Kind           | Job                                                       | Names to know                                                        |
| -------------- | --------------------------------------------------------- | -------------------------------------------------------------------- |
| **Creational** | Create objects / families without duplication             | Simple Factory (informal), Factory Method, Abstract Factory, Builder |
| **Structural** | Combine entities (“how to assemble a component”)          | Adapter, Decorator, Facade, Proxy                                    |
| **Behavioral** | Who does what, how they talk (“how to organize behavior”) | Chain of Responsibility, Strategy, Command, Observer                 |

Refactoring Guru: creational = creation mechanisms; structural = assemble objects/classes; behavioral = algorithms and responsibility. **Simple Factory** is a common name for a function that `switch`es on type — useful, but not a separate GoF pattern (Factory Method is). Shortlists: [structural](#structural-patterns--cheat-sheet) · [behavioral](#behavioral-patterns--cheat-sheet).

**Sources:** [Refactoring Guru — Catalog](https://refactoring.guru/design-patterns/catalog) · [Doka — Architecture and design patterns](https://doka.guide/tools/architecture-and-design-patterns/) · [ByteByteGo — OOP Design Patterns](https://blog.bytebytego.com/i/162154465/oop-design-patterns-you-must-know)

</details>

<details>
  <summary>Creational patterns — cheat sheet</summary>

<a name="creational-patterns--cheat-sheet"></a>

### Creational patterns — cheat sheet

**Definition:**

Creational patterns hide **how objects are born** — fields, defaults, extra data, which class to instantiate — so callers do not repeat construction and do not depend on concrete types.

Doka’s everyday four (not the full GoF list): **Factory** (simple), **Abstract Factory**, **Builder**, **Singleton**. GoF also has **Factory Method** and **Prototype**. Doka’s “factory” is the informal **simple factory** function — not Factory Method.

| Pattern              | One line                                                           | Mental model                                          |
| -------------------- | ------------------------------------------------------------------ | ----------------------------------------------------- |
| **Factory** (simple) | Function owns the recipe; caller passes intent (`createGuitar(6)`) | Guitar shop vs milling a neck                         |
| **Factory Method**   | Creator subclasses override `createX()` — which _class_ is born    | Department.hire → Programmer vs Accountant            |
| **Abstract Factory** | Interface that creates a **family** of related products            | Orchestra reservation: instrument + musician, one API |
| **Builder**          | Optional steps, then `build()`; chain returns `this`               | Coffee: espresso + milk + sugar                       |
| **Singleton**        | At most one instance; later `new` returns the first                | One Sun — usually redesign instead                    |

**How it works:**

- **Factory vs Abstract Factory** — Factory returns **one** product. Abstract Factory is a factory of factories: same _operations_, different _families_ (Violin+Violinist vs Cello+Cellist). Goal is dropping `instanceof` trees.
- **Factory vs Builder** — Factory: “give me a complete object, maybe with a couple of args.” Builder: many **optional steps**, order matters, avoid a constructor with ten booleans.
- **Simple Factory vs Factory Method** — `createX(type)` + `switch` is an idiom. Factory Method is when a _class_ defers `new` to subclasses (Refactoring Guru).
- **Pattern Singleton vs DI “singleton”** — Pattern = restrict count. DI lifetime = reuse the same injected instance. Different jobs (Doka).

| Use                                                                 | Pattern                               |
| ------------------------------------------------------------------- | ------------------------------------- |
| Construction > 1–2 lines, or needs calc / extra data                | Factory                               |
| Related products must stay compatible; client must not name classes | Abstract Factory                      |
| Many optional features; telescoping constructors                    | Builder                               |
| Domain truly allows only one (rare)                                 | Singleton — prefer ESM `export const` |

```js
// Factory — recipe + a calculation. Call sites stay createGuitar(6) if fields change.
function createGuitar(stringsCount = 6, maxWeight = 5) {
  const fretBoardMaterial = maxWeight <= 5 ? "fir" : "cedar";
  return { stringsCount, fretsCount: 24, fretBoardMaterial };
}
createGuitar(6);
createGuitar(7, 8);
```

```js
// Abstract Factory — same two methods; violin vs cello family. reserve() never instanceofs.
const violinKit = {
  reserveInstrument: () => ({ play: (n) => `violin ${n}` }),
  notifyPlayer: () => ({ play: (piece) => piece.map((n) => `violin ${n}`) }),
};
const celloKit = {
  reserveInstrument: () => ({ play: (n) => `cello ${n}` }),
  notifyPlayer: () => ({ play: (piece) => piece.map((n) => `cello ${n}`) }),
};
function reserve(kit) {
  kit.notifyPlayer();
  return kit.reserveInstrument();
}
reserve(violinKit).play("A"); // 'violin A'
reserve(celloKit).play("A"); // 'cello A'
```

```js
// Builder — optional steps, then build(). addX returns this so you can chain.
class DrinkBuilder {
  settings = { base: "espresso" };
  addMilk() {
    this.settings.milk = true;
    return this;
  }
  addSugar() {
    this.settings.sugar = true;
    return this;
  }
  build() {
    return { ...this.settings };
  }
}
new DrinkBuilder().addMilk().build(); // { base: 'espresso', milk: true }
new DrinkBuilder().addSugar().addMilk().build();
```

```js
// Singleton — second new returns the first object. Prefer export const in real JS.
class Sun {
  static #instance = null;
  constructor() {
    if (Sun.#instance) return Sun.#instance;
    Sun.#instance = this;
  }
}
new Sun() === new Sun(); // true
// Idiomatic: export const sun = Object.freeze({ mass: 1.989e30 });
```

**See also:** [Factory](#factory--factory-method) · [Abstract Factory](#abstract-factory) · [Builder](#builder) · [Singleton](#singleton-caution) · [GOF catalog](#gof-design-patterns--catalog) · [JS factory](./3_js_objects.md#factory-and-module-patterns)

**Sources:**

[Doka — Creational patterns](https://doka.guide/tools/design-patterns-creational/) (Sasha Bespoyasov, updated 2024-02-29) · [Refactoring Guru — Catalog](https://refactoring.guru/design-patterns/catalog) · [Factory comparison](https://refactoring.guru/design-patterns/factory-comparison)

---

</details>

<details>
  <summary>Structural patterns — cheat sheet</summary>

<a name="structural-patterns--cheat-sheet"></a>

### Structural patterns — cheat sheet

**Definition:**

Structural patterns answer “how do we assemble a component so it can compose with others?” — they manage **object composition**, not creation (creational) or who talks to whom (behavioral).

| Pattern       | One line                                                                     | Mental model                           |
| ------------- | ---------------------------------------------------------------------------- | -------------------------------------- |
| **Adapter**   | Translate incompatible API / data shape into what the client already expects | Travel plug                            |
| **Facade**    | One simple method over many subsystem steps                                  | “Heat water” vs 8 atomic machine calls |
| **Decorator** | Wrap to **add** behavior; keep the same interface                            | Phone + case; `debounce(fn)`           |
| **Proxy**     | Same interface; **gate** access (lazy, auth, cache, intercept)               | Receptionist; JS `Proxy`               |

**How they differ** (Refactoring Guru):

- **Adapter vs Decorator** — Adapter provides a _different_ interface. Decorator keeps (or extends) the original and supports stacking wrappers.
- **Adapter vs Facade** — Adapter wraps _one_ object to match a target interface. Facade gives a _new, simpler_ interface over a _subsystem_ (many objects / many calls).
- **Decorator vs Proxy** — both wrap and usually keep the same interface. Decorator _adds_ responsibilities (log, retry, encrypt). Proxy _controls_ access (lazy init, protection, remote stub, cache) and does not exist to enrich the public API.
- **JS `Proxy` vs the pattern** — `new Proxy(target, handler)` is a language tool that intercepts operations (`get`, `set`, …). You can implement a GoF Proxy _or_ a Decorator with it. Vue 3 reactivity uses `Proxy`.

**How it works:**

| Use                                                                            | Pattern   |
| ------------------------------------------------------------------------------ | --------- |
| Server `snake_case` / nested payload ≠ app `camelCase` DTOs                    | Adapter   |
| Hexagonal / ports: Prisma, Stripe, Express live in the outer **adapter** layer | Adapter   |
| Same multi-step SDK dance copied in several places                             | Facade    |
| Cross-cutting extras: log, metrics, cache, `throttle` / `debounce`             | Decorator |
| Check / delay / deny before the real object (auth, lazy, DOM insert guard)     | Proxy     |

```js
// Adapter — foreign shape → our shape. Callers never see snake_case.
function toUser(dto) {
  return { id: dto.ID, name: dto.user_name };
}
toUser({ ID: "1", user_name: "Ada" }); // { id: '1', name: 'Ada' }
```

```js
// Facade — one function runs several subsystem steps. UI calls brew(), not 4 SDK methods.
function brew(machine) {
  machine.grind();
  machine.heat();
  machine.pour();
  return machine.cup();
}
```

```js
// Decorator — same function, extra behavior before/after. update itself stays clean.
const withLog =
  (fn) =>
  (...args) => {
    console.log("call", args);
    return fn(...args);
  };
const save = (user) => {
  /* write */
};
const saveLogged = withLog(save);
saveLogged({ id: 1 }); // logs, then save
```

```js
// Proxy — same property names; trap decides what happens. original.secret stays readable.
const user = { name: "Ada", secret: "x" };
const gate = new Proxy(user, {
  get(t, key, r) {
    if (key === "secret") return undefined;
    return Reflect.get(t, key, r);
  },
});
gate.name; // 'Ada'
gate.secret; // undefined
```

**Thoughts:** Adapter **translates**. Facade **simplifies**. Decorator **adds**. Proxy **gates**.

**See also:** [Adapter](#adapter) · [Facade](#facade) · [Decorator](#decorator) · [Proxy](#proxy) · [JS `Proxy`](./3_js_objects.md#proxy-and-reflect) · [Hexagonal adapters](./7_special_architecture.md#clean-architecture--hexagonal)

**Sources:**

[Refactoring Guru — Adapter relations](https://refactoring.guru/design-patterns/adapter) · [Decorator](https://refactoring.guru/design-patterns/decorator) · [Facade](https://refactoring.guru/design-patterns/facade) · [Proxy](https://refactoring.guru/design-patterns/proxy) · [MDN `Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

---

</details>

<details>
  <summary>Behavioral patterns — cheat sheet</summary>

<a name="behavioral-patterns--cheat-sheet"></a>

### Behavioral patterns — cheat sheet

**Definition:**

Behavioral patterns answer “who does what, and how do they talk?” — they split **responsibility** and **communication**, not object creation or assembly.

| Pattern                     | One line                                                  | Swap in your head                |
| --------------------------- | --------------------------------------------------------- | -------------------------------- |
| **Strategy**                | Same job, different algorithm; pick (and swap) at runtime | Navigator: walk / bike / drive   |
| **Chain of Responsibility** | Request walks handlers until **one** can handle it        | Charge USD vs EUR vs RUB         |
| **Command**                 | Action + data become an object you can queue, log, undo   | Redux action `{ type, payload }` |
| **Observer**                | Subject notifies whoever subscribed; list can change      | Vacancy → matching applicants    |

**How it works:**

- **Strategy vs CoR** — Strategy: caller _chooses_ the algorithm. CoR: caller _does not know_ who will handle; order of handlers decides.
- **CoR vs `switch`** — `switch` works when all cases are known and owned here (Redux reducers). CoR wins when handlers, order, or variants are unknown or plugged in later — the dispatcher does not import every handler.
- **CoR vs middleware** — Doka: middleware lets **every** link run (auth → log → handler). Canonical CoR: **one** handler consumes the request and stops. Refactoring Guru still treats middleware as CoR with inverted “always call `next()` unless you stop.”
- **Null Object** — same interface, does nothing. Use as `switch` `default` or chain terminator so callers skip `if (handler)`.
- **Command vs Observer** — Command is a _one-shot_ encapsulated request (invoker → `execute`). Observer is _ongoing_ subscription (`attach` / `detach` / `notify`).

| Use                                                               | Pattern  |
| ----------------------------------------------------------------- | -------- |
| Algorithm may change while the app runs                           | Strategy |
| Unknown / plugin handlers; don’t couple dispatcher to all of them | CoR      |
| Undo, queue, history, extra payload on an action                  | Command  |
| Unknown how many listeners; subscription is temporary             | Observer |

**Thoughts:** Name the _problem_ (swap algorithm / pass until handled / reify an action / notify many). The pattern is vocabulary, not a class count.

**Sources:** [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/) · [Refactoring Guru — Catalog](https://refactoring.guru/design-patterns/catalog)

</details>

<details>
  <summary>Observer</summary>

<a name="observer"></a>

### Observer

**Definition:** One object changes → all **subscribers** get told. Also called **Listener** or **Event-Subscriber** (GoF behavioral pattern).

**Simple idea:** A YouTube channel posts a video → every subscriber gets notified. They don't check the channel every hour.

**How it works:**

| Piece                     | Job                           |
| ------------------------- | ----------------------------- |
| **Subject** (publisher)   | Owns data + list of listeners |
| **Observer** (subscriber) | Callback that runs on change  |

1. **Subscribe** — listener joins the list.
2. **Change** — subject updates its state.
3. **Notify** — subject calls every listener.
4. **Unsubscribe** — listener leaves (important — avoids memory leaks).

**Push vs pull:** **Push** = subject sends new data (`notify(25)`). **Pull** = subject says "I changed"; listener reads `getState()`.

| Not the same as | Difference                                      |
| --------------- | ----------------------------------------------- |
| **Pub/Sub**     | Goes through a broker/topic, often async        |
| **Mediator**    | Central router, not a direct fan-out            |
| **Command**     | One action object — not an ongoing subscription |

See also: [Mediator](#mediator) · [Command](#command).

**When to use:** Button clicks, Redux `subscribe`, `EventEmitter`, any time **many parts** must react to **one** change.

**Pluses and minuses:**

| +                                            | −                                          |
| -------------------------------------------- | ------------------------------------------ |
| Add listeners without editing the subject    | Easy to forget unsubscribe                 |
| Subscribe/unsubscribe at runtime             | Harder to trace who fired what             |
| Subject doesn't know concrete listener types | Listeners can trigger each other in a loop |

#### Common pitfalls

- No cleanup → memory leak (`removeEventListener`, `useEffect` return, store unsubscribe).
- Listener changes subject → infinite notify loop.
- One callback? Just pass a function — Observer is for **many** listeners.

**Examples:**

```js
// 1) Smallest store — subscribe, change, notify
const listeners = new Set();
let count = 0;

function subscribe(fn) {
  listeners.add(fn);
  return () => listeners.delete(fn); // unsubscribe
}

function setCount(next) {
  count = next;
  listeners.forEach((fn) => fn(count)); // tell everyone
}

const off = subscribe((n) => console.log("count is", n));
setCount(1); // logs: count is 1
off(); // stop listening
```

```js
// 2) You already use Observer — DOM events
const btn = document.querySelector("button");
const onClick = () => console.log("clicked");

btn.addEventListener("click", onClick); // subscribe
btn.removeEventListener("click", onClick); // unsubscribe
```

```js
// 3) Weather station — one source, many displays
class WeatherStation {
  #listeners = [];
  #temp = 20;

  subscribe(fn) {
    this.#listeners.push(fn);
  }

  setTemp(c) {
    this.#temp = c;
    this.#listeners.forEach((fn) => fn(this.#temp));
  }
}

const station = new WeatherStation();
station.subscribe((t) => console.log("Phone:", t + "°C"));
station.subscribe((t) => console.log("TV:", t + "°C"));
station.setTemp(25);
// Phone: 25°C
// TV: 25°C
```

**Thoughts:** Ask: "Who needs to know when this changes?" React `useEffect` cleanup = unsubscribe. Redux `store.subscribe` = Observer.

**Sources:** [Refactoring Guru — Observer](https://refactoring.guru/design-patterns/observer) · [MDN — EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget) · [GeeksforGeeks — Observer](https://www.geeksforgeeks.org/system-design/observer-pattern-set-1-introduction/) · [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)

---

</details>

<details>
  <summary>Factory / Factory Method</summary>

<a name="factory--factory-method"></a>

### Factory / Factory Method

**Definition:** Encapsulate object creation so the caller does not know field names, defaults, extra lookups, or whether the result is a plain object or `new Class()`.

**How it works:**

A **simple factory** is a function that returns same-shaped objects (`createGuitar(6)`, `createSpending(amount)`). **Factory Method** (GoF) defers `new` to a subclass. Both keep construction rules in one place — if the product schema changes, only the factory changes.

Doka: use a factory when creation is more than 1–2 lines, especially if you **compute** something first (fretboard wood from max weight). Call sites stay `createGuitar(6)` even if you later return `new Guitar(...)`.

**When to use:**

Construction logic / defaults / validation; many similar objects; `createParser('json')`; test doubles. Skip for `{ id }` literals.

#### Common pitfalls

Wrapping every object in a factory (“pattern fever”). Confusing simple factory with Factory Method / Abstract Factory.

**Pluses and minuses:**

| +                                                  | −                                                    |
| -------------------------------------------------- | ---------------------------------------------------- |
| Hides construction complexity                      | Extra indirection                                    |
| Centralizes defaults (e.g. `dateTime: Date.now()`) | Over-factorying simple DTOs                          |
| Fewer typos than duplicating object literals       | No `instanceof` unless you share a prototype/`class` |

**Examples:**

```js
// 1) Construction logic — Doka: compute a field, hide the rest of the recipe
function createGuitar(stringsCount = 6, maxWeight = 5) {
  const fretBoardMaterial = maxWeight <= 5 ? "fir" : "cedar";
  return {
    stringsCount,
    fretsCount: 24,
    fretBoardMaterial,
    boardMaterial: "maple",
  };
}
createGuitar(6); // light guitar → fir
createGuitar(7, 9); // heavy → cedar
```

```js
// 2) Many similar objects — defaults (timestamp, kind) in one place
function createSpending(amount) {
  return { amount, dateTime: Date.now(), kind: "spending" };
}
const spending1 = createSpending(250);
const spending2 = createSpending(100);
```

```js
// 3) Pick implementation by type — simple factory (not Factory Method)
function createParser(kind) {
  if (kind === "json") return (text) => JSON.parse(text);
  if (kind === "csv") return (text) => text.split(",").map((s) => s.trim());
  throw new Error(`Unknown parser: ${kind}`);
}
createParser("json")('{"a":1}'); // { a: 1 }
createParser("csv")("a, b"); // ['a', 'b']
```

```js
// 4) Test double — same factory, fake in tests
function createLogger(env) {
  if (env === "test") return { info: () => {} };
  return { info: (msg) => console.log(msg) };
}
createLogger("prod").info("up");
createLogger("test").info("silent");
```

```js
// 5) Factory Method (GoF) — subclass decides which class is born
class Department {
  hire(id) {
    const employee = this.createEmployee(id);
    employee.start();
    return employee;
  }
  createEmployee(id) {
    throw new Error("override me");
  }
}
class ITDepartment extends Department {
  createEmployee(id) {
    return { id, role: "programmer", start() {} };
  }
}
class AccountingDepartment extends Department {
  createEmployee(id) {
    return { id, role: "accountant", start() {} };
  }
}
new ITDepartment().hire(1).role; // 'programmer'
new AccountingDepartment().hire(2).role; // 'accountant'
```

**Thoughts:**

Factories are enough for small objects. Grow into a `class` when you need `this` or methods that must stay on the entity. See [Factory patterns](./3_js_objects.md#factory-and-module-patterns).

Knowledge of _how_ to build lives in one function.

**Sources:**

[Doka — Creational patterns](https://doka.guide/tools/design-patterns-creational/) · [Doka — OOP](https://doka.guide/tools/oop/) · [Refactoring Guru — Factory comparison](https://refactoring.guru/design-patterns/factory-comparison)

---

</details>

<details>
  <summary>Abstract Factory</summary>

<a name="abstract-factory"></a>

### Abstract Factory

**Definition:** An interface for creating a **family** of related products without naming concrete classes — “a factory of factories.” It returns abstractions (`Instrument`, `Musician`), not one hardcoded type.

**How it works:**

Shared contract (`ReservationFactory { reserveInstrument(); notifyPlayer(); }`). Each family (violin, cello) implements that contract. Client `reserve(factory)` never `instanceof`s. Add a viola → new class, not a new branch in `reserve()`.

Refactoring Guru: one factory produces matching products (Windows button + Windows checkbox). Harder to add a _new product type_ (every factory gains a method).

**When to use:**

Related-but-not-identical objects with the same creation steps (reserve + notify; themed UI kits).

#### Common pitfalls

Union types + `instanceof` instead of a shared factory interface — fragile when the orchestra grows.

**Pluses and minuses:**

| +                                 | −                                               |
| --------------------------------- | ----------------------------------------------- |
| Compatible product families       | Many interfaces / classes                       |
| Client code stays on abstractions | Adding a new product kind touches all factories |
| No `instanceof` trees             | Overkill for one product type                   |

**Examples:**

```js
// 1) Orchestra family — same API, different products. Client has no instanceof.
class Violin {
  playNote(note) {
    return `violin ${note}`;
  }
}
class Cello {
  playNote(note) {
    return `cello ${note}`;
  }
}
class Violinist {
  instrument = new Violin();
  play(piece) {
    return piece.map((n) => this.instrument.playNote(n));
  }
}
class Cellist {
  instrument = new Cello();
  play(piece) {
    return piece.map((n) => this.instrument.playNote(n));
  }
}

class ViolinReservation {
  reserveInstrument() {
    return new Violin();
  }
  notifyPlayer() {
    return new Violinist();
  }
}
class CelloReservation {
  reserveInstrument() {
    return new Cello();
  }
  notifyPlayer() {
    return new Cellist();
  }
}

function reserve(factory) {
  const player = factory.notifyPlayer();
  factory.reserveInstrument();
  return player;
}
reserve(new ViolinReservation()).play(["A", "C"]); // ['violin A', 'violin C']
reserve(new CelloReservation()).play(["A", "C"]); // ['cello A', 'cello C']
```

```js
// 2) Themed UI kit — button + input stay in the same family (dark vs light)
const lightUi = {
  button: (label) => ({ kind: "button", theme: "light", label }),
  input: (name) => ({ kind: "input", theme: "light", name }),
};
const darkUi = {
  button: (label) => ({ kind: "button", theme: "dark", label }),
  input: (name) => ({ kind: "input", theme: "dark", name }),
};

function loginForm(ui) {
  return { submit: ui.button("Log in"), email: ui.input("email") };
}
loginForm(lightUi).submit.theme; // 'light'
loginForm(darkUi).email.theme; // 'dark'
```

**Thoughts:** You create _types of products_, not `new Violin()` in the client.

**Sources:**

[Doka — Creational patterns](https://doka.guide/tools/design-patterns-creational/) · [Refactoring Guru — Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory)

---

</details>

<details>
  <summary>Builder</summary>

<a name="builder"></a>

### Builder

**Definition:** Construct a complex object **step by step**, with optional parts, then `build()`. Same construction process can yield different representations.

**How it works:**

Builder holds settings. Each `addX()` / `setX()` mutates and **returns `this`** (fluent chain). `build()` produces the product. Refactoring Guru: kills **telescoping constructors** (`Pizza(size)`, `Pizza(size, cheese)`, `Pizza(size, cheese, pepperoni)`…). Director (optional) runs a fixed sequence of steps.

Unlike other creational patterns, Builder can produce products that **do not share** one interface (Car vs Manual from the same steps).

**When to use:**

Many optional ingredients/fields (coffee drink, HTTP request, query, test data). Process splits into named steps.

#### Common pitfalls

Builder when a factory with two args is enough. Forgetting `return this` breaks chaining.

**Pluses and minuses:**

| +                                | −                                           |
| -------------------------------- | ------------------------------------------- |
| Readable optional configuration  | Verbose for 2-field objects                 |
| Avoids boolean-flag constructors | Mutable builder if reused without `reset()` |
| Same steps, different products   |                                             |

**Examples:**

```js
// 1) Coffee drink — optional ingredients, fluent chain (Doka)
class DrinkBuilder {
  settings = { base: "espresso" };
  addMilk() {
    this.settings.milk = true;
    return this;
  }
  addSugar() {
    this.settings.sugar = true;
    return this;
  }
  addCream() {
    this.settings.cream = true;
    return this;
  }
  build() {
    return { ...this.settings };
  }
}
const latte = new DrinkBuilder().addMilk().build();
const extra = new DrinkBuilder().addSugar().addCream().build();
```

```js
// 2) HTTP request — skip unused options instead of fetch(url, { method, headers, body, … })
class RequestBuilder {
  #url = "";
  #init = { method: "GET", headers: {} };
  url(value) {
    this.#url = value;
    return this;
  }
  method(value) {
    this.#init.method = value;
    return this;
  }
  header(key, value) {
    this.#init.headers[key] = value;
    return this;
  }
  json(body) {
    this.#init.body = JSON.stringify(body);
    return this.header("Content-Type", "application/json").method("POST");
  }
  build() {
    return { url: this.#url, ...this.#init };
  }
}
new RequestBuilder().url("/api/users").json({ name: "Ada" }).build();
// { url: '/api/users', method: 'POST', headers: { 'Content-Type': 'application/json' }, body: '{"name":"Ada"}' }
```

```js
// 3) Query / test data — named steps, no telescoping constructor
class UserBuilder {
  #user = { name: "anon", role: "user", active: true };
  name(value) {
    this.#user.name = value;
    return this;
  }
  admin() {
    this.#user.role = "admin";
    return this;
  }
  inactive() {
    this.#user.active = false;
    return this;
  }
  build() {
    return { ...this.#user };
  }
}
new UserBuilder().name("Ada").admin().build();
// { name: 'Ada', role: 'admin', active: true }
```

**Thoughts:** Steps first, object last — `build()` is the only `new`.

**Sources:**

[Doka — Creational patterns](https://doka.guide/tools/design-patterns-creational/) · [Refactoring Guru — Builder](https://refactoring.guru/design-patterns/builder)

---

</details>

<details>
  <summary>Strategy</summary>

<a name="strategy"></a>

### Strategy

**Definition:** Swap **how** something is done without a giant `if/else`. Each option is a separate strategy; the caller picks one.

**Simple idea:** Checkout lets you pay by card or PayPal — same `pay()` button, different logic behind it.

**How it works:**

1. Define a common shape (e.g. `calculate(price)`).
2. Put each variant in its own function/class.
3. Context calls `strategy.calculate(price)` — no `switch` inside checkout.

| vs [State](#state) | Strategy                    | State                  |
| ------------------ | --------------------------- | ---------------------- |
| Who picks          | You, from outside           | Object switches itself |
| Example            | `vip` vs `regular` discount | order: draft → sent    |

**When to use:** Payment methods, discount rules, sort comparators, validators — anywhere you'd write `if (type === 'a') … else if …`.

**Pluses and minuses:**

| +                                         | −                                  |
| ----------------------------------------- | ---------------------------------- |
| Add a new option without editing old code | More small functions/classes       |
| Swap at runtime                           | Overkill for only 2 fixed branches |

**Examples:**

```js
// 1) Object map — pick by key (most common in JS)
const discounts = {
  regular: (price) => price,
  vip: (price) => price * 0.8,
  sale: (price) => price * 0.5,
};

function checkout(price, customerType) {
  return discounts[customerType](price);
}

checkout(100, "vip"); // 80
checkout(100, "sale"); // 50
```

```js
// 2) Same idea with payment providers
const pay = {
  stripe: (amount) => chargeStripe(amount),
  paypal: (amount) => chargePayPal(amount),
};

pay[provider](total); // no if-chain in the caller
```

**Thoughts:** If the object **changes its own mode** over time → [State](#state). If **you** pick the algorithm → Strategy.

**Sources:** [Refactoring Guru — Strategy](https://refactoring.guru/design-patterns/strategy) · [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)

---

</details>

<details>
  <summary>Singleton (caution)</summary>

<a name="singleton-caution"></a>

### Singleton (caution)

**Definition:** Creational pattern ensuring **only one instance** exists; a later create-attempt returns the existing object. Provides a global access point.

**How it works:**

- Typical class form: static private `#instance`; constructor returns existing instance (Doka `Sun`) or private ctor + `getInstance()`.
- In JS, **module scope** (`export const db = createPool()`) naturally gives one instance per process — idiomatic singleton without ceremony.
- Risk: hidden global mutable state shared across tests and requests. Refactoring Guru: same drawbacks as globals — less modular, unit tests need the singleton present.

Doka: may violate **open/closed**, raise coupling — sometimes treated as an antipattern. Prefer a design that does not need it.

Pattern Singleton (limit count) vs DI **singleton lifetime** (same instance injected into dependents). Different mechanisms (Doka).

**When to use:** Rarely in frontend — prefer module scope or DI container. Server: DB pool, config, logger. **Avoid** business-logic singletons with mutable session state. Domain “only one Sun” is the textbook yes — still think twice.

**Pluses and minuses:**

| +                                   | −                                                 |
| ----------------------------------- | ------------------------------------------------- |
| Controlled global access point      | Hidden global state — hard to test                |
| Lazy init saves resources           | Tight coupling to concrete instance               |
| One connection pool / config object | Concurrency issues in multi-threaded envs (Java)  |
|                                     | Violates DIP if callers can't inject alternatives |

**Examples:**

```js
// 1) Domain uniqueness — Doka Sun: later new returns the same object
class Sun {
  static #instance = null;
  constructor() {
    if (Sun.#instance) return Sun.#instance;
    Sun.#instance = this;
  }
}
const a = new Sun();
const b = new Sun();
a === b; // true
```

```js
// 2) Idiomatic JS — one module export per process (config / logger / pool)
// config.js
export const config = Object.freeze({ apiUrl: process.env.API_URL ?? "/api" });
// importers share the same binding — no class Singleton needed
```

```js
// 3) Logger registry — one shared object; still prefer injecting it in tests
class Logger {
  static #instance = null;
  #lines = [];
  constructor() {
    if (Logger.#instance) return Logger.#instance;
    Logger.#instance = this;
  }
  info(msg) {
    this.#lines.push(msg);
  }
  dump() {
    return this.#lines.slice();
  }
}
new Logger().info("boot");
new Logger().dump(); // ['boot'] — same instance
```

```js
// 4) Not the same as DI singleton — container reuses one instance when injecting
function createApp({ logger = new Logger() } = {}) {
  return { log: (msg) => logger.info(msg) };
}
const logger = new Logger();
const appA = createApp({ logger });
const appB = createApp({ logger });
// both apps share logger because we passed it — not because of a global getInstance()
```

**Thoughts:**

GoF class Singleton is often an **anti-pattern** in tests — inject dependencies instead. Interview: testability is the main objection.

One instance is a last resort, not a default.

**Sources:**

[Doka — Creational patterns](https://doka.guide/tools/design-patterns-creational/) · [Refactoring Guru — Singleton](https://refactoring.guru/design-patterns/singleton)

---

</details>

<details>
  <summary>Facade</summary>

<a name="facade"></a>

### Facade

**Definition:** Structural pattern providing a **simplified interface** over a complex subsystem — one entry point hides many internal classes/APIs.

**How it works:**

- Client calls `facade.doThing()` instead of orchestrating `auth()`, `retry()`, `parse()`, `cache()` itself.
- Facade **delegates** to subsystem components — it doesn't duplicate their logic.
- Can wrap legacy APIs, third-party SDKs, or microservice calls behind one stable contract.
- Risk: facade grows into a god object if it accumulates business logic instead of delegating.

Callers write `getUser('42')` or `heatWater(machine)` instead of repeating token refresh + fetch + parse, or `turnOn` / `getWater` / heater steps. The facade **owns the sequence**; the coffee machine / HTTP stack stay dumb.

**When to use:** `apiClient.getUser()` wrapping auth refresh + fetch + retry + JSON parse; `paymentService.charge()` over Stripe; a coffee-machine `heatWater()` over `turnOn` / `getWater` / `turnOnHeater`; React hooks that bundle several context reads.

**Pluses and minuses:**

| +                                               | −                                        |
| ----------------------------------------------- | ---------------------------------------- |
| Easier API for consumers                        | Can become god facade if logic creeps in |
| Hides legacy or complex internals               | Extra layer to maintain                  |
| Stable public surface while subsystem refactors | May hide needed low-level control        |

**Examples:**

```js
// Facade over fetch + auth + error handling
export async function getUser(id) {
  const token = await auth.refreshIfNeeded();
  const res = await fetch(`/api/users/${id}`, {
    headers: { Authorization: `Bearer ${token}` },
  });
  if (!res.ok) throw new ApiError(res.status);
  return res.json();
}

const user = await getUser("42");

// Same idea: many atomic SDK calls → one use case
function heatWater(machine) {
  machine.turnOn();
  while (machine.getWaterLevel() < 1000) machine.getWater();
  machine.turnOnHeater();
  if (machine.getTemperature() >= 90) machine.turnOffHeater();
}
```

**Thoughts:** Facade ≠ Adapter (changes interface to match client) or Proxy (controls access). Facade simplifies **many** calls into **one**. It can make an awkward API usable, but that is a side effect — the job is encapsulation of a repeated workflow. Don't let it become a god object.

**Sources:**

[refactoring.guru — Facade](https://refactoring.guru/design-patterns/facade)

---

</details>

<details>
  <summary>Adapter</summary>

<a name="adapter"></a>

### Adapter

**Definition:** Structural pattern that converts one interface into another **expected by the client** — wraps incompatible API so existing code works without changes.

**How it works:**

- **Target** — interface the client expects.
- **Adaptee** — legacy or third-party API with different shape.
- **Adapter** — translates calls: `target.method()` → `adaptee.oldMethod()` with arg/response mapping.
- Object adapter (composition) vs class adapter (inheritance) — composition preferred in JS.

`fakeAPI()` is the adaptee (nested `entries`, `user_name`, `ID`). `responseToWantedAdapter` is the only place that knows that mapping. The rest of the app receives `{ userName, email, id }[]` — if the server renames a field, you change the adapter, not every screen.

**When to use:** Server JSON uses `user_name` / `ID` while the UI wants `camelCase`; wrap old REST when migrating to GraphQL; adapt Node `fs` callbacks to Promises; normalize payment providers to `PaymentResult`; `axios` adapter for `fetch`-shaped code.

**Pluses and minuses:**

| +                                       | −                                 |
| --------------------------------------- | --------------------------------- |
| Reuse existing code with new interfaces | Thin adapters can pile up         |
| Integrate third-party SDKs cleanly      | Mapping layer may lag API updates |
| Isolate legacy from modern modules      | Extra indirection                 |

**Examples:**

```js
function fakeAPI() {
  return {
    entries: [{ user_name: "Ada", email_address: "a@x.com", ID: "1" }],
  };
}

function responseToWantedAdapter(response) {
  return response.entries.map((entry) => ({
    userName: entry.user_name,
    email: entry.email_address,
    id: entry.ID,
  }));
}

const compatible = responseToWantedAdapter(fakeAPI());
// [{ userName: 'Ada', email: 'a@x.com', id: '1' }]
```

**Thoughts:** Adapter changes **interface**; Decorator adds **behavior** on the same interface. In Clean/Hexagonal architecture, adapters occupy the outer layer (DB, HTTP, UI) so the domain never depends on third-party shapes.

**Sources:**

[refactoring.guru — Adapter](https://refactoring.guru/design-patterns/adapter)

---

</details>

<details>
  <summary>Decorator</summary>

<a name="decorator"></a>

### Decorator

**Definition:** Structural pattern that **wraps** an object to add behavior dynamically without subclassing — same interface, enhanced functionality.

**How it works:**

- Decorator implements the same interface as the wrapped object.
- Delegates to inner object, adding logic before/after (logging, caching, auth).
- Stack multiple decorators: `withAuth(withCache(withRetry(api)))`.
- In React: **Higher-Order Components (HOCs)** and wrapper components are decorator-like.

`loggingDecorator` is a HOF: it takes `update` and returns `wrapped`. `wrapped` logs, then calls the original with the same args. You did **not** edit `update`. You can stack more wrappers (`debounce(loggingDecorator(update))`) the same way.

**When to use:** Logging/metrics middleware; `memoize()`; React HOC `withAuth(Component)`; Express/Redux middleware; caching on repositories; **`throttle` / `debounce`** as function wrappers ([JS Functions](./3_js_functions.md)).

**Pluses and minuses:**

| +                                               | −                                        |
| ----------------------------------------------- | ---------------------------------------- |
| Open/closed — extend without modifying original | Many wrappers → hard to trace call stack |
| Compose behavior at runtime                     | Order of decorators matters              |
| Alternative to deep inheritance trees           | Can obscure what's actually running      |

**Examples:**

```js
function loggingDecorator(fn) {
  return function wrapped(...args) {
    console.log(`log: ${args.join(", ")}`);
    return fn(...args);
  };
}

const user = { name: "Ada", email: "a@x.com" };
function update(name, email) {
  user.name = name;
  user.email = email;
}

const updateWithLogging = loggingDecorator(update);
updateWithLogging("Maria", "m@x.com");
// console: "log: Maria, m@x.com" — then user becomes { name: 'Maria', email: 'm@x.com' }
```

**Thoughts:** TypeScript `experimentalDecorators` / stage-3 decorators formalize this for classes — the _pattern_ is older and works as a HOF in JS. Decorator vs Proxy: decorator is an explicit wrapper that _adds_ behavior; a GoF Proxy _gates_ the original. Stacking (`EncryptionDecorator(CompressionDecorator(file))`) is the GoF picture.

**Sources:**

[refactoring.guru — Decorator](https://refactoring.guru/design-patterns/decorator)

---

</details>

<details>
  <summary>Proxy</summary>

<a name="proxy"></a>

### Proxy

**Definition:** Structural pattern that provides a **surrogate** with the **same interface** as the real object, so the client talks to the proxy and the proxy decides when (and whether) to reach the original.

**How it works:**

- Client depends on the target interface; the proxy forwards, delays, caches, or denies.
- Common kinds: **virtual** (lazy create), **protection** (auth), **remote** (RPC stub), **caching**.
- In JS, `new Proxy(target, handler)` intercepts operations via traps. `handler.get(target, property, receiver)` runs on `proxy.foo`. Use `Reflect.get(target, property, receiver)` for default `[[Get]]` behavior (MDN).
- Original object can stay untouched; only the proxy is “weird.”

Reads go through `get`. The trap blocks `ssn` on the **proxy only**. `Reflect.get` is the default `[[Get]]` (MDN) so other fields still come from `original`. That is a **protection proxy**: same keys, gated access — not a new DTO (adapter) and not extra logging wrapped around a function (decorator).

**When to use:** Hide `ssn` on a user object; lazy-init a heavy module; cache `getUser`; check conditions before inserting a DOM node; Vue reactivity.

**Pluses and minuses:**

| +                                                                          | −                                                            |
| -------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Lazy load, access control, cache, logging without changing the real object | Extra hop; debugging through traps is harder                 |
| Transparent to callers that already use the target API                     | `Proxy` traps have invariants (e.g. non-writable data props) |
| Native `Proxy` is how Vue 3 tracks reads                                   | Overhead on hot paths                                        |

**Examples:**

```js
const original = { name: "Maria", email: "hi@site.com", ssn: "000" };

const proxied = new Proxy(original, {
  get(target, prop, receiver) {
    if (prop === "ssn") throw new Error("Access denied");
    console.log(`get ${String(prop)}`);
    return Reflect.get(target, prop, receiver);
  },
});

proxied.name; // logs "get name", then 'Maria'
proxied.ssn; // throws 'Access denied'
original.ssn; // still '000' — the real object is not mutated
```

**Thoughts:** A GoF Proxy does not exist to _change the product’s public behavior_ the way a Decorator does — it _mediates_. The article-style demo that returns `'YOU HAVE BEEN PWND!'` for every field is interception, not a typical production proxy. Language `Proxy` ≠ always the design pattern.

**Sources:**

[refactoring.guru — Proxy](https://refactoring.guru/design-patterns/proxy) · [MDN `Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) · [MDN `handler.get`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/get)

---

</details>

<details>
  <summary>Mediator</summary>

### Mediator

**Definition:** One **middle object** routes messages between parts. Components talk to the mediator — not to each other.

**Simple idea:** Chat room — you send a message to the room; the room delivers it to everyone else. Users don't hold references to every other user.

**How it works:**

1. Components register with the mediator.
2. Component A calls `mediator.notify("message", data)`.
3. Mediator decides who gets it and forwards.

| vs [Observer](#observer) | Mediator                 | Observer                    |
| ------------------------ | ------------------------ | --------------------------- |
| Shape                    | Hub routes between peers | One source fans out to many |
| Example                  | Chat room                | Store `subscribe`           |

**When to use:** Chat, complex forms with linked fields, Redux store (actions → reducers), NestJS event bus.

**Pluses and minuses:**

| +                                 | −                                |
| --------------------------------- | -------------------------------- |
| No spaghetti of A→B, A→C, B→C     | Mediator can become a god-object |
| One place to change routing rules | Extra layer                      |

**Examples:**

```js
// Chat room — mediator delivers messages
const chatRoom = {
  users: [],

  join(user) {
    this.users.push(user);
  },

  send(from, text) {
    for (const user of this.users) {
      if (user !== from) user.onMessage(text);
    }
  },
};

const alice = {
  name: "Alice",
  onMessage(t) {
    console.log("Alice got:", t);
  },
};
const bob = {
  name: "Bob",
  onMessage(t) {
    console.log("Bob got:", t);
  },
};

chatRoom.join(alice);
chatRoom.join(bob);
chatRoom.send(alice, "Hi!"); // Bob got: Hi!
```

**Thoughts:** Many-to-many talk → Mediator. One thing changes, many listen → [Observer](#observer).

**Sources:** [Refactoring Guru — Mediator](https://refactoring.guru/design-patterns/mediator)

</details>

<details>
  <summary>Chain of Responsibility</summary>

<a name="chain-of-responsibility"></a>

### Chain of Responsibility

**Definition:** Pass a request down a line of handlers. Each one says **"not me, next"** or **"I got it, stop"**.

**Simple idea:** Support ticket: bot tries first → if stuck, junior agent → if stuck, senior agent.

**How it works:**

1. Request hits handler A.
2. A handles it **or** passes to B.
3. Stops when someone handles it (or the chain ends).

**Classic CoR** = first match wins. **Middleware** (Express `next()`) = every layer runs — similar idea, different stop rule.

**When to use:** You don't know all handlers upfront, or you want to plug them in without rewriting the dispatcher. If it's always the same 3 cases in one file — a `switch` is fine.

**Pluses and minuses:**

| +                                        | −                                         |
| ---------------------------------------- | ----------------------------------------- |
| Add handlers without touching the caller | Request can reach the end with no handler |
| Each handler does one job                | Order matters — easy to get wrong         |

**Examples:**

```js
// Handlers as simple functions — pass to next if you can't handle
function usdHandler(currency, amount, next) {
  if (currency === "USD") return console.log(`Charged $${amount}`);
  return next?.(currency, amount);
}

function eurHandler(currency, amount, next) {
  if (currency === "EUR") return console.log(`Charged €${amount}`);
  return next?.(currency, amount);
}

function run(currency, amount) {
  usdHandler(currency, amount, (c, a) =>
    eurHandler(c, a, () => console.log("No handler for", c)),
  );
}

run("USD", 50); // Charged $50
run("GBP", 50); // No handler for GBP
```

**Thoughts:** Middleware runs **all** layers (`next()`). Classic CoR stops at the **first** match. See [Behavioral cheat sheet](#behavioral-patterns--cheat-sheet).

**Sources:** [Doka — Behavioral](https://doka.guide/tools/design-patterns-behaviorial/) · [Refactoring Guru — Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)

</details>

<details>
  <summary>Command</summary>

<a name="command"></a>

### Command

**Definition:** Wrap an action in an **object** so you can run it later, queue it, log it, or **undo** it.

**Simple idea:** Text editor Ctrl+Z — each edit is saved as a command; undo reverses the last one.

**How it works:**

| Piece        | Job                                                  |
| ------------ | ---------------------------------------------------- |
| **Command**  | `{ do(), undo() }` — the action + its reverse        |
| **Invoker**  | Button, menu, queue — calls `do()`                   |
| **Receiver** | The thing that actually changes (editor, API, store) |

Not [Observer](#observer) (ongoing subscription). Not [Strategy](#strategy) (pick an algorithm — Command is one specific action).

**When to use:** Undo/redo, job queues, audit logs, Redux actions.

**Pluses and minuses:**

| +                         | −                               |
| ------------------------- | ------------------------------- |
| Undo and history for free | Extra objects for simple clicks |
| Queue or delay work       | Easy to over-engineer           |

**Examples:**

```js
// 1) Undo stack — the core idea in plain JS
const history = [];
const editor = { text: "" };

const addHello = {
  do() {
    editor.text += "hello";
  },
  undo() {
    editor.text = editor.text.slice(0, -5);
  },
};

function run(cmd) {
  cmd.do();
  history.push(cmd);
}

function undo() {
  history.pop()?.undo();
}

run(addHello); // editor.text = "hello"
undo(); // editor.text = ""
```

```js
// 2) Redux — action is command data; reducer is the receiver
const action = { type: "SET_NAME", payload: "Alex" };

function reducer(state, action) {
  if (action.type === "SET_NAME") return { ...state, name: action.payload };
  return state;
}

reducer({ name: "" }, action); // { name: "Alex" }
```

**Thoughts:** Redux `dispatch(action)` = send a command. The reducer does the work. Contrast [Observer](#observer) (many listeners, ongoing) and [Chain of Responsibility](#chain-of-responsibility) (pass until handled).

**Sources:** [Doka — Behavioral](https://doka.guide/tools/design-patterns-behaviorial/) · [Refactoring Guru — Command](https://refactoring.guru/design-patterns/command)

</details>

<details>
  <summary>State</summary>

<a name="state"></a>

### State

**Definition:** Object **behaves differently** depending on its current mode. Instead of `if (status === 'draft') … else if …`, each mode is its own object.

**Simple idea:** Order is `draft` → you can submit. Once `sent` → submit throws "already sent". Same button, different rules per mode.

**How it works:**

1. Object holds `state` (current mode).
2. Methods call `this.state.submit()` — not a big switch.
3. State object decides what happens **and** which mode comes next.

| vs [Strategy](#strategy) | State                  | Strategy                |
| ------------------------ | ---------------------- | ----------------------- |
| Who picks                | Object switches itself | You pick from outside   |
| Example                  | draft → sent → done    | vip vs regular discount |

**When to use:** Order lifecycle, media player (play/pause/stop), wizard steps, connection status.

**Pluses and minuses:**

| +                            | −                           |
| ---------------------------- | --------------------------- |
| No giant `if/else` on status | More objects for each mode  |
| Invalid actions are obvious  | Overkill for 2 simple flags |

**Examples:**

```js
// 1) Order — each state knows what's allowed
const sent = {
  submit() {
    throw new Error("Already sent");
  },
};

const draft = {
  submit(order) {
    order.state = sent;
    console.log("Sent to warehouse");
  },
};

const order = { state: draft };

order.state.submit(order); // ok → sent
order.state.submit(order); // throws: Already sent
```

```js
// 2) Traffic light — behavior changes with color
const green = { next: () => yellow };
const yellow = { next: () => red };
const red = { next: () => green };

let light = green;
light = light.next(); // yellow
light = light.next(); // red
```

**Thoughts:** Object **changes its own mode** → State. **You** pick the algorithm → [Strategy](#strategy).

**Sources:** [Refactoring Guru — State](https://refactoring.guru/design-patterns/state) · [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)

</details>

<details>
  <summary>Repository</summary>

### Repository

**Definition:** Pattern that **mediates between domain logic and data mapping** — collection-like interface for domain objects, hiding persistence details (SQL, ORM, API).

**How it works:**

- Domain/service code calls `userRepo.findById(id)` — not raw SQL or Prisma directly.
- Repository implements CRUD + domain queries (`findByEmail`, `save`).
- Swappable backends: Postgres repo, in-memory repo for tests, mock API repo.
- Often one repository per aggregate root (User, Order).

**When to use:** Service layer in backend; frontend data access modules (`userApi` with consistent interface); testing business rules without database; Prisma/TypeORM repository wrappers.

**Pluses and minuses:**

| +                                          | −                                |
| ------------------------------------------ | -------------------------------- |
| Testable domain logic with in-memory fakes | Extra abstraction over ORM       |
| Centralized query logic                    | Generic repos become god objects |
| Persistence technology can change          | Mapping entity ↔ DB row overhead |

**Examples:**

```ts
type User = { id: string; email: string };
type UserRepository = {
  findById(id: string): Promise<User | null>;
  save(user: User): Promise<void>;
};

class InMemoryUserRepo implements UserRepository {
  #store = new Map<string, User>();
  async findById(id) {
    return this.#store.get(id) ?? null;
  }
  async save(user) {
    this.#store.set(user.id, user);
  }
}

async function inviteUser(email: string, repo: UserRepository) {
  const existing = await repo.findById(email); // domain doesn't know storage
  if (existing) throw new Error("Already exists");
  await repo.save({ id: crypto.randomUUID(), email });
}
```

**Thoughts:** Pairs with DIP. Don't wrap every ORM call blindly — repository shines when domain logic is rich and persistence varies (tests, caching layer, multiple sources).

**Sources:**

[refactoring.guru — Repository](https://refactoring.guru/design-patterns/repository)

---

</details>
