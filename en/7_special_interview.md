# Interview leftovers

Interview Q&A plus leftover tooling notes.

[← Back to hub](./7_special.md)

### Related docs

- [Computer science](./7_special_cs.md) · [Architecture patterns](./7_special_architecture.md) · [SOLID](./7_special_engineering.md) · [Design patterns](./7_special_patterns.md) · [SDLC](./7_special_sdlc.md) · [Testing](./7_special_testing.md) · [Web security](./7_special_security.md) · [Interview leftovers](./7_special_interview.md)

### Sections

- [Interview Practice](#interview-practice)
- [Questions & Answers](#questions-answers)

---

## Interview Practice

### Tooling notes

<details>
  <summary>Polyfill</summary>

### Polyfill

**Definition:** Code that implements a modern API on older environments that lack it natively.

**When to use:** Target older browsers; fill `Promise`, `fetch`, `Array.prototype.at`, etc.

**Pluses and minuses:**

| +                          | −                                     |
| -------------------------- | ------------------------------------- |
| Use new APIs with fallback | Bundle size                           |
| Progressive enhancement    | May not match native behavior exactly |

**Examples:** `core-js` polyfills `Promise` on IE11.

**Sources:**

[learn.javascript.ru/polyfills](https://learn.javascript.ru/polyfills)

---

</details>

<details>
  <summary>Transpiler (Babel)</summary>

### Transpiler (Babel)

**Definition:** Rewrites source code to equivalent code in older syntax (ES2024 → ES5) without changing runtime semantics (mostly).

**When to use:** Any production build targeting browsers without full ES support.

**Pluses and minuses:**

| +                           | −                     |
| --------------------------- | --------------------- |
| Write modern JS             | Build step required   |
| Tooling ecosystem (plugins) | Debug via source maps |

**Thoughts:**

Babel ≠ bundler. Babel transforms syntax; webpack/Vite bundle modules. Polyfill fills missing APIs; transpiler rewrites syntax.

---

</details>

<details>
  <summary>Structural programming</summary>

### Structural programming

**Definition:** Control flow via structured blocks (`if`, `while`, functions) — no arbitrary `goto`.

**When to use:**

Default in all modern code. Avoid spaghetti jumps.

---

**Pluses and minuses:**

| +                              | −   |
| ------------------------------ | --- |
| Readable control flow          |     |
| Foundation of modern languages |     |

</details>

<a name="16-resources"></a>

## Questions & Answers

<details>
  <summary>Imperative vs declarative — how vs what?</summary>

### Imperative vs declarative — how vs what?

**Definition:** Imperative lists _commands in order_. Declarative states _the result_; another layer (engine, `filter`, SQL optimizer) chooses the steps.

**How it works:**

Same “odd numbers in an array”: a `for` + `push` vs `array.filter`. Difference is who owns details. Use imperative for a specific algorithm; declarative at business-logic height so you do not hold _what_ and _how_ in working memory at once.

_Как_ (команды) vs _что_ (результат). Алгоритм — императивно; бизнес — декларативно.

**Thoughts:** _How_ vs _what_ — hide the how one layer down.

**See also:** [Imperative vs declarative](./7_special_cs.md#programming-paradigm)

</details>

<details>
  <summary>What is a pure function?</summary>

### What is a pure function?

**Definition:** Same arguments → same return value; **no side effects** (no writes/reads of hidden outer state, no I/O inside the function).

**How it works:**

`double(x) { return x * 2 }` is pure. Mutating a global, calling `fetch`, `Date.now()`, `Math.random()`, or depending on outer mutable state is impure — even **reading** external methods can hide mutations. Purity buys **referential transparency**, easy tests, and parallelism; copies instead of in-place updates can use more memory. Real apps isolate effects at the edges ([functional core, imperative shell](./7_special_architecture.md#functional-core-imperative-shell)).

- **First-class functions (JS):** assign to variable, pass as argument, return from function — enables HOFs.
- **`compose(f, g)(x) === f(g(x))`:** `(...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x)`.
- **HOF:** takes and/or returns a function — `map`, `filter`, decorators.

Чистая = те же аргументы → тот же результат, без эффектов. `fetch`/DOM/глобали/`Date.now()` — нечисто. Эффекты на краю.

**Thoughts:** _Arguments in, value out — the rest of the universe stays untouched._

**See also:** [Pure functions & side effects](./3_js_functions.md#pure-functions--side-effects) · [Function composition](./3_js_functions.md#function-composition) · [HOF](./3_js_functions.md#higher-order-functions-hof)

</details>

<details>
  <summary>Why are JS functions first-class? Implement compose.</summary>

### Why are JS functions first-class? Implement compose.

**First-class (first-class citizen):** A function can be (1) assigned to a variable, (2) passed as an argument, (3) returned from another function.

```js
const multipleTwo = (n) => n * 2;
loadData(getData);
function makeAdder(x) {
  return (y) => x + y;
}
```

**Compose:** `compose(f, g, ...)(x) === f(g(...(x)))` — right to left.

```js
const compose =
  (...fns) =>
  (x) =>
    fns.reduceRight((acc, fn) => fn(acc), x);
```

**HOF:** Function that accepts and/or returns another function — e.g. `filter(list, predicate)`, `timed(fn)`.

**See also:** [Function composition](./3_js_functions.md#function-composition) · [HOF](./3_js_functions.md#higher-order-functions-hof)

</details>

<details>
  <summary>Two Sum — why Map, not a plain object?</summary>

### Two Sum — why Map, not a plain object?

**Definition:** Unsorted Two Sum is a complement lookup: for each number, ask “have I already seen `target - x`?”

**How it works:** One `Map` of value → index, one pass, O(n) time. A plain object coerces keys to strings, which collides for numbers and is wrong for non-string keys. Sorted input can drop the map and use two pointers.

**Thoughts:** _`Map` is the O(n) interview solution; objects stringify keys._

**See also:** [Two Sum with Map](./7_special_algorithms_patterns.md#two-sum-with-map)

</details>

<details>
  <summary>Walk a tree without overflowing the stack?</summary>

### Walk a tree without overflowing the stack?

**Definition:** DFS can be recursive (call stack) or iterative (explicit stack). Skip `null` children.

**How it works:** Recursion is fine for shallow React trees (menus). Unbounded depth (API trees, files) → `while` + array stack, or flatten first. BFS uses a queue (level order).

**Thoughts:** _Same visit order as recursion if you simulate the stack; heap memory instead of call stack._

**See also:** [Tree traversal](./7_special_data_structures_trees.md#tree-traversal-recursive-vs-iterative)

</details>

<details>
  <summary>What are classes (Classes)?</summary>

### What are classes (Classes)?

**Short answer:** A **class** is a blueprint for creating objects: fields hold data, methods hold behavior, `constructor` runs on `new`. In JS it is **syntax sugar over prototypes**.

**How it works:**

- `new History()` creates an **instance**. Methods use `this` so each instance has its own `records`.
- Same shape without `class`: a **factory** function returning object literals — fine for small DTOs.
- Classes shine when you have many instances and want knowledge (how to `addRecord`) kept on the entity.
- Not a replacement for design: still name entities, relations, and dependencies first.

```js
class History {
  records = [];
  addRecord(record) {
    this.records.push(record);
  }
}

const history = new History();
history.addRecord({ amount: 100, dateTime: Date.now() });
```

**Interview tip:** Pair with "class vs constructor function" and "composition vs inheritance". Mention JS has no interfaces — TypeScript supplies contracts. Full answer: [JS interview](./3_js_interview.md).

**See also:** [Object-Oriented Programming](./7_special_engineering.md#object-oriented-programming-oop) · [Entities, relations & design](./7_special_engineering.md#object-oriented-programming-oop)

</details>

<details>
  <summary>SOLID in JavaScript?</summary>

### SOLID in JavaScript?

**Short answer:** Same five OOP principles — applied to **modules, functions, and components**, not only `class`.

**See also:** [JS — Language — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — Language — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [SOLID in React](./4_react_core.md#solid-in-react)

</details>

<details>
  <summary>Observer vs Pub/Sub?</summary>

### Observer vs Pub/Sub?

**Definition:**

Both are one-to-many notification patterns, but differ in **coupling** and **topology**.

|              | Observer                                                       | Pub/Sub                                                   |
| ------------ | -------------------------------------------------------------- | --------------------------------------------------------- |
| **Coupling** | Subject **knows** observers directly                           | Publishers and subscribers **don't know** each other      |
| **Mediator** | None — direct registration                                     | **Broker / event bus** in between                         |
| **Delivery** | Synchronous callback list                                      | Often async (message queue)                               |
| **Examples** | DOM `addEventListener`, Redux `subscribe`, Node `EventEmitter` | Redis pub/sub, RabbitMQ, MQTT, browser `BroadcastChannel` |

**How it works:**

- **Observer:** `subject.subscribe(observer)` → subject holds observer refs → on change, `observers.forEach(fn => fn(data))`.
- **Pub/Sub:** `publisher.emit('order.created', payload)` → broker routes to all channels named `order.created` → subscribers receive without knowing publisher.

`EventEmitter` is **observer-style** (same process, direct). Redis/RabbitMQ is **true pub/sub** across processes. Mention **unsubscribe/cleanup** — memory leaks are the #1 observer bug (React `useEffect` return cleans up).

**When to use:**

- **Observer:** In-process UI updates, state store subscriptions, component lifecycle — when all parties live in same runtime.
- **Pub/Sub:** Cross-service events, microservices decoupling, real-time fan-out at scale, offline consumers.

**Examples:**

```js
const listeners = new Set();
function subscribe(fn) {
  listeners.add(fn);
  return () => listeners.delete(fn);
}
function notify(data) {
  listeners.forEach((fn) => fn(data));
}
```

```js
// Event bus — publishers don't hold subscriber refs
const bus = {
  channels: new Map(),
  on(topic, fn) { (this.channels.get(topic) ??= new Set()).add(fn); },
  emit(topic, data) { this.channels.get(topic)?.forEach(fn => fn(data)); },
};
```

</details>

<details>
  <summary>Architecture vs design patterns?</summary>

### Architecture vs design patterns?

**Architecture** = whole-system rules for how modules talk (MVC roles, domain vs infra layers, Flux/Redux one-way flow, monolith vs many apps). **Patterns** = named local recipes (Adapter, Strategy, Observer). House plan vs how you pour concrete. Not a goal — skip on a prototype; add when change, tests, and onboarding hurt.

**See also:** [Architecture vs design patterns](./7_special_architecture.md#architecture-vs-design-patterns) · [Interview Q&A](./0_system_design_interview.md#architecture-vs-design-patterns)

</details>

<details>
  <summary>MVC vs client–server?</summary>

### MVC vs client–server?

**Definition:** MVC splits **data / display / input**. Client–server splits **browser / backend / DB**. Early web: View = HTML, Controller = server, Model = DB. Modern SPAs keep MVC (or Flux) **on the client** so the document is JSON-shaped state, not HTML with inline styles.

**How it works:** [MVC](./7_special_architecture.md#mvc-model-view-controller) · [0_system_design.md — Client–server](./0_system_design_core.md#clientserver-architecture-why-a-backend)

</details>

<details>
  <summary>MVC data flow, fat vs thin controller, MVC vs MVP vs MVVM?</summary>

### MVC data flow, fat vs thin controller, MVC vs MVP vs MVVM?

**Definition:** Classic MVC is a **circle**: User → View → Controller → Model → View redraws. Model is independent; View shows it; Controller translates intent.

**How it works:**

- **View vs controller:** no universal rule — either the view binds DOM and calls `toggle()`, or the controller reads `e.target`. One convention per project.
- **Fat controller:** `selectColor(event)` maps `daylight` → `"blue"`; model is a store. Risk: god object.
- **Thin controller:** `updateColor("blue")`; model owns `toggle()`. Risk: model dumps.
- **MVP:** **line** — `presenter.onPower()` mutates **and** `view.show(...)`. View never reads the model.
- **MVVM:** ViewModel + **binder** (`v-model`) so you do not write `redraw()`.

[MVC](./7_special_architecture.md#mvc-model-view-controller) · [MVC vs MVP vs MVVM](./7_special_architecture.md#mvc-vs-mvp-vs-mvvm)

**Examples:** “Click Power → controller flips `model.isOn` → view adds class `is-on`. I put button→color mapping in the view so the controller stays `updateColor(string)`.”

**Thoughts:** Interview = name the flow, then say where _your_ team put event mapping and transforms.

</details>

<details>
  <summary>One-way vs two-way data flow?</summary>

### One-way vs two-way data flow?

**Definition:** Data flow is how modules pass intermediate results. **One-way:** between two parts you only send _or_ receive — direction is fixed (Flux/Redux). **Two-way:** the same pair can update each other (typical: two-way binding on a form field).

**How it works:**

Redux cycle — view reads store → user event → `dispatch(action)` → reducers return next state → subscribers re-render. The store does not paint the UI. Two-way binding skips explicit actions for a field (`v-model` = `:value` + `@input`). **Reactivity** is different: derived values recompute when sources change (`computed`); plain `let a3 = a1 + a2` does not.

Однонаправленный = труба. Двунаправленный = поле ↔ модель. Реактивность = Excel: изменил ячейку — сумма пересчиталась сама.

[Data flow](./7_special_architecture.md#data-flow--one-way-vs-two-way)

**Examples:** Click “+” in React: child calls `onInc`, parent `setQty`, child re-renders with new `qty` — it never writes `qty` itself. Click “+” in Redux: `{ type: 'cart/inc' }` → reducer returns `{ qty: 2 }` → UI re-reads.

**Thoughts:** Large/complex/many people → one-way (less magic). Small/forms → two-way saves boilerplate.

</details>

<details>
  <summary>DRY, KISS, YAGNI?</summary>

### DRY, KISS, YAGNI?

**Definition:**

Three complementary principles for managing complexity — reduce duplication, keep solutions simple, and avoid speculative features.

| Principle | Full name                | Core idea                                                                    |
| --------- | ------------------------ | ---------------------------------------------------------------------------- |
| **DRY**   | Don't Repeat Yourself    | Single source of truth for **knowledge** — not character count               |
| **KISS**  | Keep It Simple, Stupid   | Simplest solution that works; add complexity only when measured need appears |
| **YAGNI** | You Aren't Gonna Need It | Don't build features or abstractions until they're actually required         |

**How it works:**

- **DRY** — extract shared validation, constants, business rules. Two similar loops with different intent? **Don't** merge — that's accidental duplication.
- **KISS** — prefer readable `if/else` over pattern framework for one case. Refactor when complexity hurts, not preemptively.
- **YAGNI** — resist "future-proof" plugin systems nobody asked for. Design **seams** (interfaces) without building unused implementations.

Duplicate twice, extract on the third occurrence — when the **pattern** is clear, not coincidental.

Balance all three — "DRY is about knowledge, KISS is default, YAGNI stops gold-plating." Mention premature abstraction is worse than duplication.

**Pluses and minuses:**

| + (together)                        | − / traps                                                    |
| ----------------------------------- | ------------------------------------------------------------ |
| Less bug surface, faster onboarding | DRY too early → wrong abstraction                            |
| Focus on current user value         | KISS can block needed scale prep                             |
| Avoid dead code maintenance         | YAGNI vs "we know this is coming next sprint" — use judgment |

**Examples:**

```js
// ❌ DRY violation — same tax rule in checkout AND invoice
// ✅ Extract once
const TAX_RATE = 0.2;
function withTax(amount) {
  return amount * (1 + TAX_RATE);
}

// ❌ YAGNI — PaymentStrategy interface for one provider
// ✅ KISS — single function until second provider is real
function charge(amount) {
  return stripe.charge(amount);
}
```

**See also:** [DRY, KISS, YAGNI](./0_system_design_principles.md)

</details>

<details>
  <summary>Creational patterns — factory vs AF vs builder vs singleton?</summary>

### Creational patterns — factory vs AF vs builder vs singleton?

**Definition:** Creational patterns hide object construction. The four you will name in interviews: **Factory** (recipe), **Abstract Factory** (family), **Builder** (steps), **Singleton** (one instance). See [Creational cheat sheet](./7_special_patterns.md#creational-patterns--cheat-sheet).

**How it works:**

| Problem                                      | Pattern                                     |
| -------------------------------------------- | ------------------------------------------- |
| Construction logic / defaults in many places | Factory (`createGuitar(6)`)                 |
| Related products + `instanceof` explosion    | Abstract Factory                            |
| Many optional parts / telescoping ctor       | Builder + `build()`                         |
| Must be unique in the process                | Singleton — usually ESM export, not a class |

Name the _problem_ first. Simple factory ≠ Factory Method ≠ Abstract Factory. Singleton pattern ≠ DI singleton scope.

```js
const guitar = createGuitar(6); // factory — recipe in one fn
reserve(violinKit); // abstract factory — family, no instanceof
new DrinkBuilder().addMilk().build();
new Sun() === new Sun(); // singleton — usually export const instead
```

Using Singleton for convenience (global config in every module) — fails tests and OCP.

**Sources:** [Doka — Creational patterns](https://doka.guide/tools/design-patterns-creational/)

</details>

<details>
  <summary>Common patterns (interview)?</summary>

### Common patterns (interview)?

**Quick catalog** — name, category, one-line use case. Architecture (MVC, Flux, layers) is a larger cut; these are local tools. See [Architecture vs design patterns](./7_special_architecture.md#architecture-vs-design-patterns).

| Pattern                     | Category        | When                                             |
| --------------------------- | --------------- | ------------------------------------------------ |
| **Singleton**               | Creational      | One shared instance (DB pool) — caution in tests |
| **Factory**                 | Creational      | Hide recipe / `new Concrete()` behind creator    |
| **Abstract Factory**        | Creational      | Family of products behind one interface          |
| **Builder**                 | Creational      | Optional steps, then `build()`                   |
| **Strategy**                | Behavioral      | Swap algorithms at runtime (payment, sort)       |
| **Observer**                | Behavioral      | One-to-many notify on state change               |
| **Chain of Responsibility** | Behavioral      | Middleware / handler pipeline                    |
| **Adapter**                 | Structural      | Wrap incompatible API to match client interface  |
| **Decorator**               | Structural      | Add behavior via wrappers (HOCs, middleware)     |
| **Facade**                  | Structural      | Simple API over complex subsystem                |
| **Proxy**                   | Structural      | Same interface; gate access (lazy, auth, cache)  |
| **Repository**              | Data            | Collection-like persistence abstraction          |
| **Module**                  | Creational (JS) | `export` — encapsulation via closure/ESM         |

**Interview flow:** State problem → name pattern → trade-off (pros/cons) → JS example.

**Example one-liners:**

- Factory: `createGuitar(6)` — recipe in one place
- Abstract Factory: `reserve(factory)` — no `instanceof` per instrument
- Builder: `new DrinkBuilder().addMilk().build()`
- Strategy: `strategies[type](data)` instead of giant `switch`
- Adapter: map `{ user_name }` → `{ name }` for legacy API
- Facade: `heatWater()` / `getUser()` hides many SDK calls
- Decorator: `withAuth(withLogging(Component))` or `debounce(fn)`
- Proxy: `new Proxy(obj, { get })` — auth / lazy / cache, original untouched
- Repository: `userRepo.findById()` — domain never sees SQL

**Sources:** [refactoring.guru catalog](https://refactoring.guru/design-patterns/catalog)

</details>

<details>
  <summary>Why write tests? What is AAA?</summary>

### Why write tests? What is AAA?

**Definition:** A test is code that checks assumptions about other code. It documents the contract, catches regressions, and makes refactors and dependency upgrades safer.

**How it works:** Arrange (inputs/fakes) → Act (run the scenario) → Assert (one assumption). Tests should be independent. Skip only for throwaway prototypes.

**See also:** [Why write tests (AAA)](./7_special_testing_fundamentals.md#tests-and-aaa) · [Test pyramid](./7_special_testing_fundamentals.md#12-testing)

</details>

<details>
  <summary>How do you set up Jest for Node and the browser?</summary>

### How do you set up Jest for Node and the browser?

**Definition:** Default `testEnvironment` is **`node`**. For DOM, use **`jsdom`** globally or per file via `/** @jest-environment jsdom */`. Mock `fetch` with `jest.fn` returning `{ json }`; mock modules with `jest.mock`.

**Thoughts:** Node for logic; jsdom + mocks for DOM/network — do not hit the real API in unit tests.

**See also:** [Jest — first tests](./7_special_testing_tooling.md#jest--first-tests-node-vs-jsdom) · [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies)

</details>

<details>
  <summary>Stub vs mock vs spy?</summary>

### Stub vs mock vs spy?

**Definition:** All three are **test doubles**. A **stub** replaces a dependency with a canned answer and does not check how it was used. A **mock** also records calls/args (and often resulting state) so you can assert collaboration. A **spy** wraps a real method (`jest.spyOn`) — same job as a mock, without building a fake object by hand; restore with `mockRestore` / `jest.restoreAllMocks`.

**How it works:**

`add(40, 2)` needs no double. `addRandom` needs a stubbed `Math.random`. `toggleTheme` returns nothing — assert `toHaveBeenCalledWith`. Prefer injecting the dependency over replacing globals. If the suite needs a forest of mocks, the API is too coupled.

Стаб — заглушка без проверок вызовов. Мок/шпион — замена + контроль вызовов/аргументов. Не мокай, если можно не мокать.

**Thoughts:** Stub = replace. Mock/spy = replace **and** verify usage. Don't mock if the unit can stay pure.

**See also:** [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies)

</details>

<details>
  <summary>Unit tests passed but the app is broken — what next?</summary>

### Unit tests passed but the app is broken — what next?

**Definition:** Units never prove **wiring**. **Integration** tests a group of modules (still not the whole product). **System** tests the assembled app: E2E (user flow), screenshots (UI pixels), performance (Web Vitals / Lighthouse), security (escape, cookies, API).

**How it works:** Pyramid stays: many units, fewer integration, few E2E. Incremental integration localizes failures better than “big bang”. RTL/MSW = integration in jsdom, not E2E. Playwright/Cypress = real browser.

**Thoughts:** Green units ≠ working product — add seam tests, then a handful of real-user flows.

**See also:** [Integration](./7_special_testing_layers.md#integration-testing) · [System](./7_special_testing_layers.md#system-testing) · [E2E](./7_special_testing_layers.md#e2e-testing)

</details>

<details>
  <summary>XSS vs stored XSS — how do you mitigate?</summary>

### XSS vs stored XSS — how do you mitigate?

**Definition:** XSS runs attacker script **in your origin**. **Reflected** — in that response (often a query string). **Stored** — saved and replayed for later viewers. **DOM** — client JS writes untrusted strings into the DOM.

**How it works:** Encode output for the context (HTML entities, not `encodeURI` for markup). Use `textContent` over `innerHTML`. Sanitize if you must allow HTML. CSP `script-src` (nonces, no `'unsafe-inline'`) is backup. HttpOnly cookies stop **reading** the session cookie, not acting as the user.

**Thoughts:** _Untrusted data is never HTML/JS. Stored XSS needs a DB cleanup too._

**See also:** [XSS](./7_special_security.md#xss-cross-site-scripting) · [Stored XSS](./7_special_security.md#stored-xss--xss-driven-keylogging) · [CSP](./7_special_security.md#content-security-policy-csp)

</details>

<details>
  <summary>CSRF vs clickjacking vs MITM?</summary>

### CSRF vs clickjacking vs MITM?

**Definition:** Three different failures. CSRF — browser **auto-sends cookies** on a forged request. Clickjacking — user clicks **your** UI inside a foreign iframe. MITM — attacker **reads/alters** the wire.

**How it works:** CSRF → token + `SameSite=Lax/Strict` + Origin check (`HttpOnly` does not stop CSRF). Clickjacking → CSP `frame-ancestors` + `X-Frame-Options` (`ALLOW-FROM` is obsolete). MITM → HTTPS/TLS + HSTS; do not ignore cert warnings.

**Thoughts:** _Cookie intent vs UI redress vs transport — different headers for each._

**See also:** [CSRF](./7_special_security.md#csrf-cross-site-request-forgery) · [Clickjacking](./7_special_security.md#clickjacking) · [MITM](./7_special_security.md#man-in-the-middle-mitm)

</details>

<details>
  <summary>Why is SQL injection not XSS?</summary>

### Why is SQL injection not XSS?

**Definition:** SQLi changes **SQL structure** (concatenated input). XSS changes **HTML/JS** in the browser. Same class of bug (untrusted data treated as code), different language.

**How it works:** Prevent SQLi with parameterized queries / ORM bind — not frontend escaping. Prevent XSS with context encoding + CSP.

**Thoughts:** _Bind SQL parameters; encode HTML. Do not mix the two mitigations._

**See also:** [SQL injection](./7_special_security.md#sql-injection) · [Database — SQL injection](./9_database_sql.md#sql-injection)

</details>

<details>
  <summary>Symmetric vs asymmetric vs hash?</summary>

### Symmetric vs asymmetric vs hash?

**Definition:** **Symmetric** — one shared secret encrypts and decrypts (AES). **Asymmetric** — public encrypts / private decrypts (or private signs / public verifies). **Hash** — one-way fixed digest; you compare hashes, you do not decrypt.

**How it works:** TLS uses asymmetric (or ECDHE) only to **agree** a session key, then symmetric for the bytes. Passwords: slow KDF (Argon2id/bcrypt) + salt — not MD5, not reversible encryption.

**Thoughts:** _Fast shared-key for data; public-key for setup and signatures; hashes for integrity and password storage._

**See also:** [Encryption](./7_special_security.md#encryption) · [HTTPS](./6_web_api_http.md#https)

</details>

<details>
  <summary>Why methodologies / Waterfall vs Agile?</summary>

### Why methodologies / Waterfall vs Agile?

**Definition:** A methodology is how the team coordinates delivery under uncertainty. **Waterfall** freezes a full spec and runs phases once. **Agile** repeats a short full cycle so hypotheses can be killed or grown quickly.

**How it works:** Software changes cheaper than physical engineering, so a multi-year TZ often ships late, obsolete, and hard to amend. Manifesto (2001): people, working software, customer collaboration, responding to change — _over_ (not instead of) tools, docs, contracts, and plans. Scrum/Kanban are frameworks; Agile is the mindset. Not a silver bullet — match process to contract and risk.

**See also:** [Software design methodology](./7_special_sdlc.md#software-design-methodology) · [Waterfall](./7_special_sdlc.md#waterfall) · [Agile](./7_special_sdlc.md#agile) · [Not a silver bullet](./7_special_sdlc.md#not-a-silver-bullet)

</details>

<details>
  <summary>Scrum vs Kanban?</summary>

### Scrum vs Kanban?

**Definition:** **Scrum** — timeboxed Sprints (≤ 1 month), PO / SM / Developers, Increment + Definition of Done, Planning / Daily / Review / Retro. **Kanban** — pull board, WIP limits, optimize flow; no required Sprint.

**How it works:** Scrum for a product goal and shared cadence. Kanban when each item walks design → dev → QA, or interrupt-heavy queues; look at which column piles up (bottleneck). Hybrids (Scrumban) are common.

**See also:** [Scrum](./7_special_sdlc.md#scrum) · [Kanban](./7_special_sdlc.md#kanban)

</details>

---

<details>
  <summary>Middle — General</summary>

### Middle — General

1. Describe the testing pyramid.

2. What automated test types have you written? Which libraries did you use? What tools do you prefer and why?

3. What are unit tests? Where do they sit in the testing pyramid?

3b. How do you make tests cheap to write? (Pure core / impureim sandwich, DIP, independent AAA tests, don't unit-test libraries, shared fixtures, Jest `.only`/`.skip`/`-o`.) → [Making tests easy](./7_special_testing_practices.md#making-tests-easy)

4. What is code coverage? Is 100% coverage required?

5. How do you prevent the browser from serving a cached HTTP response?

6. What is XSS (Cross-Site Scripting)? How does stored XSS differ from reflected? How do you mitigate it?

6b. CSRF vs clickjacking vs MITM — what fails, and which headers/cookies help?

6c. Why is SQL injection not XSS? How do you prevent each?

7. Explain Observer vs Pub/Sub. What's the difference? Give examples in frameworks, libraries, or browser APIs.

8. What is `self.addEventListener('fetch', event => {})` used for in a service worker?

9. What is the event loop? Explain microtasks and macrotasks.

</details>

<details>
  <summary>General</summary>

### General

1. Describe functional programming. (Red → green: purity, immutability, composition, HOF, functional core / imperative shell.)

2. Why are JavaScript functions first-class? Implement `compose` where `compose(f, g)(x) === f(g(x))`.

3. What are higher-order functions? Give examples from JS and React.

4. What are TDD (Test-Driven Development) and BDD (Behavior-Driven Development)? Stub vs mock vs spy?

5. Explain how HTTPS works in detail.

</details>

<details>
  <summary>Middle — Practical tasks (process)</summary>

### Middle — Practical tasks (process)

148. If you have a project with tight deadlines and non-critical performance requirements...

---

</details>

</details>
