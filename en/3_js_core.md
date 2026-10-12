# JS Core — Language

Covers ECMAScript runtime fundamentals: engines and the language runtime. ES modules live in [Modules](./3_js_modules.md). Bindings, types, operators, and structures live in sibling Core topics ([Variables](./3_js_variables.md), [Data Types](./3_js_data_types.md), [Data Structures](./3_js_data_structures.md), [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Arrays](./3_js_array.md) · [Functions](./3_js_functions.md)).

[← Back to JS hub](./3_js.md)

### Related docs

- [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Stack & heap](./3_js_data_types.md#memory-stack-heap) · [globalThis](./3_js_variables.md#globalthis) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Error handling](./3_js_loops.md#error-handling) · [Iterators](./3_js_loops.md#iterators) · [JS Arrays](./3_js_array.md) · [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [JS Additional](./3_js_extra.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Document](./3_js_browser.md) · [JS Events](./3_js_events.md) · [JS Browser APIs](./3_js_browser_extra.md) · [CORS / same-origin](./3_js_browser_extra.md#cors-same-origin) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md) · [Call stack](./3_js_async_event_loop.md#call-stack)
- [TypeScript](./5_ts.md) · [Web API](./6_web_api.md) · [Build — polyfills & transpilers](./10_build_bundlers.md#polyfills-vs-transpilers) · [YAML](./10_build_ci.md#yaml) · [Node.js](./8_nodejs.md) · [Process & threads](./8_nodejs_process.md#process--threads) · [React](./4_react.md) · [Vue](./4_vue.md) · [OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop) · [Computer science](./7_special_cs.md) · [SDLC](./7_special_sdlc.md#software-design-methodology)

### Main concepts

| Area        | Topics                                                                           |
| ----------- | -------------------------------------------------------------------------------- |
| **Runtime** | JS vs ECMAScript, JIT, polyfills & transpilers, strict mode, console, code style |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Core](#core)
- [Design principles](#design-principles)
- [Interview](./3_js_interview.md)

Stack & heap → [Data Types](./3_js_data_types.md#memory-stack-heap). Error handling → [Loops](./3_js_loops.md#error-handling). Iterators & generators → [Loops](./3_js_loops.md#iterators).

Sibling Core topics: [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Array](./3_js_array.md) · [Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md).

Topics also covered: **JIT**, **polyfills / transpilers**, **strict mode**. **ESM** → [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md). Types / stack-heap → [3_js_data_types.md](./3_js_data_types.md); objects / JSON → [3_js_data_structures.md](./3_js_data_structures.md); variables / scope / `globalThis` / closures → [3_js_variables.md](./3_js_variables.md); Operators → [3_js_operators.md](./3_js_operators.md); loops / errors / iterators → [3_js_loops.md](./3_js_loops.md); Arrays → [3_js_array.md](./3_js_array.md); Intl / RegExp / Signals → [3_js_extra.md](./3_js_extra.md); event loop → [JS Async](./3_js_async_await.md); GC / Proxy / deep copy → [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md); paradigms → [7_special_cs.md](./7_special_cs.md).

<a name="core"></a>

## Core

<details>
  <summary>JavaScript and ECMAScript</summary>

### JavaScript & ECMAScript

**Definition:** JavaScript is a high-level scripting language created to make web pages interactive. Programs are plain-text **scripts** (often embedded in HTML) that run without a separate compile step. **ECMAScript** (ECMA-262) is the language standard; “JavaScript” is the everyday name for that language plus **host APIs** (DOM in browsers, `fs` / network in Node, etc.). It has no technical relation to Java — the name came from marketing when it was briefly called LiveScript.

| Trait     | Detail                                                                                             |
| --------- | -------------------------------------------------------------------------------------------------- |
| Paradigms | Event-driven, functional, OOP, imperative                                                          |
| Typing    | Dynamic — types checked at runtime                                                                 |
| Execution | Engine parses → compiles → runs (JIT optimizations); see [Engine and runtime](#engine-and-runtime) |
| Modules   | `.mjs` or `type="module"` for ESM                                                                  |
| Hosts     | Browser, Node/Deno/Bun, embedded engines on other devices                                          |

**How it works:**

- **Engine (VM):** each host embeds an engine. Common codenames: **V8** (Chrome, Opera, Edge, Node), **SpiderMonkey** (Firefox), **JavaScriptCore** / Nitro / SquirrelFish (Safari). Legacy: **Chakra** (IE / early Edge). Feature support is often discussed by engine name (“supported in V8”).
- **Pipeline (basics):** parse script → compile to machine code → run; engines keep optimizing hot paths. Deeper JIT/AOT distinction → [Engine and runtime](#engine-and-runtime).
- **In-browser capabilities:** manipulate HTML/CSS; react to user input; network requests (XHR/`fetch`, historically AJAX/COMET); cookies / dialogs; client storage (`localStorage`, etc.). What the language _can_ do depends on the **host**, not on ECMAScript alone.
- **In-browser limits (safety):** no arbitrary disk / OS access; file/camera/mic need explicit user gestures or permission; cross-tab/window DOM access blocked across different origins (**Same-Origin Policy** — scheme + host + port); cross-origin network reads need the remote side’s agreement (CORS headers). Outside the browser (Node, extensions with granted permissions) those sandbox rules do not apply the same way. SOP/CORS detail → [CORS and same-origin policy](./3_js_browser_extra.md#cors-same-origin).
- **Why JS won the browser:** full HTML/CSS integration, simple tasks stay simple, shipping enabled by default in every major browser — unique combination among browser technologies. Also used for servers, mobile (RN/Hermes), desktop (Electron), etc.
- **Languages “over” JS:** many languages **transpile** to JavaScript (TypeScript, CoffeeScript, Flow, Dart→JS, Brython, Kotlin/JS, …). You still need JS to debug what the browser/Node actually runs. Typing layer today → [TypeScript](./5_ts.md).

**When to use:** Browser UI and any host that embeds a JS engine. Prefer TypeScript (or similar) for large typed codebases, but learn JS first. Skip assuming browser APIs exist on the server (and vice versa).

**Pluses and minuses:**

| +                                                               | −                                                                           |
| --------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Only ubiquitous browser language with HTML/CSS built in.        | Name confuses people with Java; dynamic typing can hide bugs until runtime. |
| Same language on client and many servers (Node, Deno, Bun).     | Capability set is host-dependent — security sandboxes differ.               |
| Huge ecosystem; engines converge on ECMAScript yearly editions. | Transpile-to-JS stack adds tools; must still reason about emitted JS.       |

**Examples:**

```html
<!-- Script in the page — runs as the document loads -->
<script>
  document.body.textContent = "Hello from a script";
</script>
```

```js
// ECMAScript module — strict by default; host APIs come from the environment
export function greet(name) {
  return `Hello, ${name}`;
}
```

**Thoughts:** Interview framing: ECMAScript = language spec; JavaScript = that language + host APIs. “What can JS do?” always means “in which environment?” Browser sandbox (SOP, no arbitrary FS) is a feature, not a bug. Brendan Eich’s first engine was SpiderMonkey (1995); modern engines implement the same ECMA-262 with minor edge-case differences.

**Sources:** [javascript.info — Intro](https://javascript.info/intro) · [MDN — What is JavaScript?](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript) · [MDN — JavaScript technologies overview](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/JavaScript_technologies_overview) · [ECMA-262](https://tc39.es/ecma262/)

</details>

<details>
  <summary>Strict mode</summary>

### Strict mode

**Definition:**

`"use strict"` opts into stricter parsing and runtime behavior: no accidental globals, duplicate params forbidden, `delete` on non-configurable properties throws.

| Restricted            | Effect                                  |
| --------------------- | --------------------------------------- |
| Undeclared assignment | `ReferenceError` instead of global leak |
| Duplicate param names | SyntaxError                             |
| Octal literals        | SyntaxError                             |
| `this` in plain calls | `undefined` (not `globalThis`)          |

**How it works:**

- Place `"use strict";` (or `'use strict';`) at the **top** of a script or as the first statement in a function. Only comments may appear above it — otherwise it is ignored.
- There is **no** `"no use strict"` — once enabled, you cannot cancel it for that scope.
- **Modules** (`import`/`export`, `type="module"`) and **class** bodies enable strict mode automatically — no directive needed.
- The browser **console** is not strict by default; wrap snippets in an IIFE with `'use strict'` when testing strict-only behavior.

**When to use:** All new code. Prefer modules/classes so strict mode is implicit; otherwise put the directive at the top of classic scripts.

**Examples:**

```js
"use strict";
function demo() {
  // this === undefined in strict mode
}

// Modules / classes are already strict:
export function f() {
  /* strict */
}
class C {
  method() {
    /* strict */
  }
}
```

**Sources:** [javascript.info/strict-mode](https://javascript.info/strict-mode) · [MDN Strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

</details>

<details>
  <summary>Engine and runtime</summary>

### JavaScript engine pipeline

**Definition:** A JS engine parses source into an AST, interprets to bytecode, profiles hot paths, and JIT-compiles optimized machine code. That mix is why “is JavaScript interpreted?” has no yes/no answer.

**How it works:**

- **AOT compiled** (C, Go): translate the whole program to machine code _before_ run. Catches many errors early; slower to start iterating.
- **Interpreted** (classic scripts): translate and run line by line. Fast to start; a typo can appear only when that line runs.
- **JS in practice:** parse + compile happen _at runtime_ (MDN still calls JS “interpreted” for that reason). Syntax is checked for the **whole** script first — a `SyntaxError` on line 2 prevents line 1 from running. A `ReferenceError` from a misspelled identifier is a **runtime** error: earlier lines already ran.
- Function **declarations** are hoisted, so a call can appear above the `function` in source — not line-by-line interpretation.
- **JIT:** interpreter for cold code; if a function runs often with stable types, an optimizing compiler emits machine code (and may deoptimize if types change). Babel/TypeScript emit older JS _before_ the engine — not the same as JIT.

| Stage               | Role                      |
| ------------------- | ------------------------- |
| Parser              | Tokenizes and builds AST  |
| Interpreter         | Runs bytecode quickly     |
| Profiler            | Finds hot functions       |
| Optimizing compiler | Emits faster machine code |

**When to use:**

Interview “interpreted vs compiled”; explain why syntax errors vs `ReferenceError` differ; why hot loops get faster after warmup.

#### Common pitfalls

Calling JS “just interpreted” or “just compiled”; mixing engine JIT with Babel transpile.

**Pluses and minuses:**

| +                                  | −                                         |
| ---------------------------------- | ----------------------------------------- |
| Fast startup + later peak speed    | Type speculation can deoptimize           |
| Syntax errors fail the whole parse | “Interpreted” / “compiled” labels confuse |

**Examples:**

```text
Source → Parser → AST → Interpreter (bytecode) → Profiler → Compiler (optimized code)
```

```js
console.log("this never runs");
console.log("oops" "); // SyntaxError — parse fails first

const a = 0;
console.log("ran");
console.log(a2); // ReferenceError after "ran"

praise("Pushkin"); // works — function declaration is hoisted
function praise(name) {
  console.log(`Ай да, ${name}!`);
}
```

**Thoughts:**

JIT combines fast startup (interpreter) with peak performance (compiler). Transpilers and polyfills run **before** or **alongside** the engine — see [Polyfills and transpilers](#polyfills-and-transpilers); they are not engine JIT.

| Engine         | Host               |
| -------------- | ------------------ |
| V8             | Chrome, Node, Deno |
| SpiderMonkey   | Firefox            |
| JavaScriptCore | Safari             |

**Sources:** [javascript.info/intro](https://javascript.info/intro) · [MDN — interpreted vs compiled](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript) · [MDN JIT](https://developer.mozilla.org/en-US/docs/Glossary/Just_in_time_compilation) · [V8 blog](https://v8.dev/blog)

</details>

<details>
  <summary>Polyfills and transpilers</summary>

### Polyfills and transpilers

**Definition:** A **transpiler** (source-to-source compiler) rewrites **ECMAScript syntax** a target engine cannot parse into older syntax it understands. A **polyfill** is runtime JavaScript that **implements a missing API** (methods, constructors, globals) so call sites stay unchanged. Neither replaces the **engine** (V8, SpiderMonkey, …) and neither is **JIT** — see [Engine and runtime](#engine-and-runtime). Bundler and Babel preset detail → [Polyfills vs transpilers](./10_build_bundlers.md#polyfills-vs-transpilers).

**How it works:**

- **Syntax gap:** `class`, `async`/`await`, optional chaining (`?.`), nullish coalescing (`??`) — an old parser throws `SyntaxError` before any line runs. Fix with **transpile** (Babel, TypeScript emit, esbuild, SWC), not a polyfill.
- **API gap:** `Promise`, `Array.prototype.at`, `Object.hasOwn` — the engine parses your file but the method is missing. Fix with a **polyfill** (`core-js`, or `if (!globalThis.X) { … }`) or avoid that API on the target.
- **Typical pipeline:** author modern source → transpile to the ECMAScript version your Browserslist (or Node `engines` field) needs → add polyfills only where the runtime lacks an API. Vite/esbuild transpile by default; polyfills are deliberate (`core-js`, `@babel/preset-env` `useBuiltIns`).
- **Bundlers** (webpack, Rollup, Vite) merge modules; they may invoke a transpiler but are not transpilers themselves.

**When to use:** You support browsers or Node versions that lack a language feature or built-in you rely on. Check the gap (MDN compatibility tables, [caniuse.com](https://caniuse.com/), [compat-table](https://compat-table.github.io/compat-table/es6/)). Prefer native implementations when they exist — MDN notes polyfills are slower and may not replicate every native behavior.

**Pluses and minuses:**

| +                                        | −                                              |
| ---------------------------------------- | ---------------------------------------------- |
| One codebase for mixed runtimes          | `core-js` and careless presets inflate bundles |
| Polyfills keep call sites readable       | Syntax cannot be polyfilled — only APIs        |
| Transpilers unlock new syntax on old VMs | Stack traces often show emitted JS, not source |

**Examples:**

```js
// API gap — polyfill (feature-detect first)
if (!Math.trunc) {
  Math.trunc = (n) => (n < 0 ? Math.ceil(n) : Math.floor(n));
}

// Syntax gap — transpile before the engine runs the file
// const size = config?.width ?? 100;
// → older JS with explicit null/undefined checks
```

**Thoughts:** Interview: “Babel vs polyfill?” — transpiler rewrites **grammar**; polyfill supplies **runtime objects/functions**. “Polyfill `async`?” — not as a one-liner; transpile to generators/Promises (or don’t support that engine). TypeScript → JavaScript is transpilation; types are erased and are never polyfilled. jQuery v1 was an early **API** polyfill story (one surface over divergent browsers) — see MDN glossary.

**Sources:** [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill) · [javascript.info — Polyfills](https://javascript.info/polyfills) · [core-js](https://github.com/zloirock/core-js)

</details>

<details>
  <summary>Console</summary>

### Console API

**Definition:** Browser/Node debugging API for logging, timing, and inspecting objects. Pair with the Sources debugger when you need to pause — see [Debugging JS (Sources)](./11_vsc_browser.md#browser-devtools).

**How it works:**

- `console.log` / `table` / `dir` / `time` / `timeEnd` for output without stopping execution.
- Prefer a **breakpoint** or `debugger` when you need live locals and the call stack.

**Examples:**

```js
console.log("info", { a: 1 });
console.table([{ id: 1 }, { id: 2 }]);
console.time("fetch");
// await fetch(...)
console.timeEnd("fetch");
console.dir(document.body, { depth: 1 });
```

**Sources:** [MDN console](https://developer.mozilla.org/en-US/docs/Web/API/console) · [javascript.info/debugging-chrome](https://javascript.info/debugging-chrome)

</details>

<details>
  <summary>Code style and comments</summary>

### Code style & comments

**Definition:** Style rules exist to make code readable and to avoid footguns (ASI, missing braces). Comments should explain **why** and architecture — not narrate **what** the next line does.

**How it works:**

- Prefer Egyptian braces (`if (cond) {`), spaces around operators, and a team line-length (often 80–120). Always put `;` after statements — see [ASI](./3_js_operators.md).
- Reduce nesting with early `return` / `continue` instead of deep `else` trees.
- Function placement: often **call site first**, helpers below — readers learn _what happens_ before _how_.
- **Good comments:** architecture overview, non-obvious decisions, JSDoc for public APIs. **Bad comments:** restating `i++` or naming what a well-named function already says — factor into a function instead.
- Anti-patterns (“ninja code”): one-letter names, vague `data`/`obj`, inconsistent synonyms (`show` vs `display` for the same thing), silent side effects in `is*`/`check*` functions, reusing variables for unrelated values mid-function.

**When to use:** Agree on a style guide (Airbnb, Standard, Google, or Prettier + ESLint) and enforce with a linter — see [ESLint](./10_build_bundlers.md).

**Examples:**

```js
// Prefer early return over nested else
function pow(x, n) {
  if (n < 0) return NaN;
  let result = 1;
  for (let i = 0; i < n; i++) result *= x;
  return result;
}

/**
 * Returns x raised to the n-th power.
 * @param {number} x
 * @param {number} n Natural number
 * @return {number}
 */
function documentedPow(x, n) {
  /* ... */
}
```

**Thoughts:** Readable names beat clever one-liners. If you need a comment to explain _what_, rewrite the code.

**Sources:** [javascript.info/coding-style](https://javascript.info/coding-style) · [javascript.info/comments](https://javascript.info/comments) · [javascript.info/ninja-code](https://javascript.info/ninja-code)

</details>

<details>
  <summary>Glossary</summary>

### Glossary

**Definition:**

Common terms used throughout JS core documentation.

| Term          | Meaning                                          |
| ------------- | ------------------------------------------------ |
| Allocate      | Reserve memory for a value                       |
| Decomposition | Break a problem into smaller parts               |
| Memoization   | Cache function results to avoid recomputation    |
| Identifier    | Name of a variable, function, or label           |
| Linter        | Tool that enforces code style and catches issues |
| Normalizer    | Function that standardizes data shape            |
| Minification  | Remove whitespace/comments to shrink bundle size |

**Sources:** [MDN Glossary](https://developer.mozilla.org/en-US/docs/Glossary)

</details>

<a name="design-principles"></a>

## Design principles

<details>
  <summary>SOLID in Classes</summary>

### SOLID in Classes

**Definition:** **SOLID** is five design principles from Robert C. Martin ("Uncle Bob") for code that stays easy to **change, test, and extend**. In JavaScript — especially as ES6 **`class`** usage grows — they help avoid **spaghetti code**. On **`class` hierarchies**, each letter maps to prototypes, `extends`, and constructor injection. In JS, `class` is **syntax sugar over prototypes** — methods sit on the prototype, `extends` wires the chain — so **Liskov** and **composition** matter whenever you subclass. Pure JS has no built-in `interface` (unlike TypeScript or Java) — some principles adapt through **function composition** or **duck typing** (JSDoc `@typedef`, TS `implements`). Module/function mapping: [SOLID in Functions](#solid-in-functions).

| Letter | Principle             | What problem it targets (classes)                            |
| ------ | --------------------- | ------------------------------------------------------------ |
| **S**  | Single Responsibility | God classes that mix unrelated jobs                          |
| **O**  | Open/Closed           | Endless `if/else` edits inside `applyDiscount()`             |
| **L**  | Liskov Substitution   | Subclasses that break callers of the parent type             |
| **I**  | Interface Segregation | Fat class APIs that force unused methods on consumers        |
| **D**  | Dependency Inversion  | Classes that `new EmailService()` or call `fetch` internally |

| Principle                     | Meaning                                                                                                                                                                                                                                     | JS `class` example                                                                              |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **S** — Single Responsibility | A class should do **one job** and have **one reason to change**. Validation rules live in `UserValidator`; persistence in `UserRepository`. Mixing validate + DB + HTML in one `User` class means a UI tweak can break storage logic.       | `UserValidator` validates; `UserRepository` saves — not one `User` class doing both             |
| **O** — Open/Closed           | Open for **extension**, closed for **modification**. Add a new discount type by creating a subclass (`PercentDiscount`), not by editing `applyDiscount()` with another `if/else` branch. Existing callers keep working.                     | Add `FixedDiscount` without editing `applyDiscount()` — plug a new `Discount` subclass          |
| **L** — Liskov Substitution   | If code accepts `Bird`, **every subclass must work there** without extra checks. If `Ostrich` makes `fly()` throw, callers need `try/catch` — you cannot swap it in safely.                                                                 | `releaseBird(new Ostrich())` crashes, but `releaseBird(new Sparrow())` works — LSP broken       |
| **I** — Interface Segregation | Clients should not depend on methods they **never use**. A read-only lister should not implement `delete()` or `update()` just because a bigger `UserRepository` class exposes all of them.                                                 | `UserReader` lists users — don't force `delete()` on a read-only class                          |
| **D** — Dependency Inversion  | High-level modules (`OrderService`) depend on **abstractions** (anything with `send()`), not concrete `EmailService` details. Pass the mailer in via `constructor` so production, tests, and future channels swap without rewriting orders. | `OrderService` takes `mailer` in `constructor` — inject `EmailService` or `SmsService` in tests |

**How it works:**

Each letter matches the **Examples** below — look for `// good` and `// bad`:

1. **S** — one class, one job (`UserValidator` vs `UserRepository`).
2. **O** — new discount = new subclass (`PercentDiscount`), not a new `if` inside `applyDiscount()`.
3. **L** — `releaseBird(bird)` must work for every bird; ground birds need a different contract, not a subclass that throws on `fly()`.
4. **I** — `UserReader` lists only; `UserWriter` mutates — don't inherit unused methods.
5. **D** — pass `mailer` into `constructor` (`new OrderService(mailer)`); don't `new EmailService()` inside `OrderService`.

**When to use:** Refactoring god classes; plugin-style providers; interview answers; code review when `extends` chains grow.

**Examples:**

```js
// S — good: one job per class
class UserValidator {
  validate(data) {
    return Boolean(data.email && data.name);
  }
}

class UserRepository {
  save(user) {
    return db.insert("users", user);
  }
}

// S — bad: god class mixes validate, DB, and HTML
// class User {
//   validate(data) { return Boolean(data.email); }
//   save(user) { return db.insert("users", user); }
//   toHtml(user) { return `<p>${user.name}</p>`; }
// }

// O — good: new discount type = new subclass (no edit to applyDiscount)
class Discount {
  apply(total) {
    throw new Error("Subclass must implement apply()");
  }
}

class PercentDiscount extends Discount {
  constructor(rate) {
    super();
    this.rate = rate;
  }
  apply(total) {
    return total * (1 - this.rate);
  }
}

class FixedDiscount extends Discount {
  constructor(amount) {
    super();
    this.amount = amount;
  }
  apply(total) {
    return Math.max(0, total - this.amount);
  }
}

function applyDiscount(total, discount) {
  return discount.apply(total);
}

// O — bad: edit applyDiscount for every new discount type
// function applyDiscount(total, type, value) {
//   if (type === "percent") return total * (1 - value);
//   if (type === "fixed") return Math.max(0, total - value);
// }

// I — good: reader lists; writer mutates — separate APIs
class UserReader {
  findAll() {
    return db.query("SELECT * FROM users");
  }
}

class UserWriter {
  update(id, data) {
    return db.query("UPDATE users SET ? WHERE id = ?", [data, id]);
  }
}

// I — bad: read-only reader forced to stub methods it never uses
// class UserRepository {
//   findAll() { return db.query("SELECT * FROM users"); }
//   update() { throw new Error("Read-only"); }
// }
// class UserReader extends UserRepository {}

// L — one caller; every "bird" must work here
function releaseBird(bird) {
  return bird.fly(); // expects a string — not a crash
}

// L — good: fly() works for flying birds
class Sparrow {
  fly() {
    return "flying";
  }
}
releaseBird(new Sparrow()); // "flying"

// L — bad: still a Bird, but fly() throws — releaseBird breaks
class Bird {
  fly() {
    return "flying";
  }
}
class Ostrich extends Bird {
  fly() {
    throw new Error("Cannot fly");
  }
}
// releaseBird(new Ostrich()); // crash — caller needs try/catch

// L — good fix: separate contract for ground birds (composition, not broken extends)
class Ostrich {
  run() {
    return "running";
  }
}

// D — good: inject mailer — swap EmailService or SmsService
class EmailService {
  send(userId, message) {
    return `email to ${userId}: ${message}`;
  }
}

class SmsService {
  send(userId, message) {
    return `sms to ${userId}: ${message}`;
  }
}

class OrderService {
  #mailer;
  constructor(mailer) {
    this.#mailer = mailer;
  }
  place(order) {
    this.#mailer.send(order.userId, `Order ${order.id} placed`);
    return { ok: true, orderId: order.id };
  }
}

new OrderService(new EmailService());
new OrderService(new SmsService());

// D — bad: hardcoded concrete mailer inside order logic
// class OrderService {
//   place(order) {
//     const mailer = new EmailService();
//     mailer.send(order.userId, `Order ${order.id} placed`);
//   }
// }
```

**Thoughts:** SOLID is a **guide**, not a law — a 10-line script does not need five classes. Apply when change pain appears. Plain functions and modules: [SOLID in Functions](#solid-in-functions). Class syntax details: [Classes](./3_js_classes.md#classes-full). Framework mapping: [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Full OOP reference: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [SOLID in Functions](#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

</details>

<details>
  <summary>SOLID in Functions</summary>

### SOLID in Functions

**Definition:** The same five **SOLID** principles apply to **plain functions**, **closures**, and **ESM modules** — the style most JavaScript uses day to day. They help avoid **spaghetti code** in function-first codebases. A module boundary or exported function can be a unit with one job, open for extension via new functions, and testable when dependencies are passed in. Pure JS has no built-in `interface` — adapt **I** and **D** through **composition** (object spread, small mixin functions) and **duck typing**. Class/prototype mapping: [SOLID in Classes](#solid-in-classes).

| Letter | Principle             | What problem it targets (functions / modules)                                        |
| ------ | --------------------- | ------------------------------------------------------------------------------------ |
| **S**  | Single Responsibility | God modules mixing parse, format, and I/O                                            |
| **O**  | Open/Closed           | Growing one function with another `if (paymentType)` branch                          |
| **L**  | Liskov Substitution   | Callbacks that throw or return inconsistent shapes                                   |
| **I**  | Interface Segregation | Fat config objects and barrel files with unused exports                              |
| **D**  | Dependency Inversion  | Domain functions that call `fetch`, `localStorage`, or `new EmailService()` directly |

| Principle                     | Meaning                                                                                                                                                                                                                                | JS function example                                                                 |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **S** — Single Responsibility | One function, **one reason to change**. Parsing raw JSON belongs in `parseUser()`; display formatting belongs in `formatUserLabel()` — not one function doing both.                                                                    | `parseUser()` vs `formatUserLabel()` — not one `handleUser()` for everything        |
| **O** — Open/Closed           | Open for **extension**, closed for **modification**. Add a new payment path by passing a new strategy function, not by editing `processOrder()` with another branch.                                                                   | `processOrder(order, payWithCard)` — add `payWithWallet` without editing core logic |
| **L** — Liskov Substitution   | Any callback passed where a contract is expected must **behave like the others**. If `normalizeUser()` usually returns `{ id, name }`, a branch that throws or returns a string breaks every caller — same LSP idea without `extends`. | Broken `normalize` callback breaks `mapUsers()` callers                             |
| **I** — Interface Segregation | Split **large config objects** into role-specific shapes. Prefer **composition** (mixin functions) over inheriting unused methods. Export **small, focused surfaces** — not one barrel with everything.                                | `canRead(store)` for list-only consumers — not a fat repo with `write()` stubs      |
| **D** — Dependency Inversion  | High-level functions depend on **injected collaborators** (HTTP client, mailer, storage), not globals. Pass `http` or `notify` as an argument so tests swap fakes without rewriting business logic.                                    | `placeOrder(order, notify)` — inject `emailService` or `smsService` in tests        |

**How it works:**

Each letter matches the **Examples** below — look for `// good` and `// bad`:

1. **S** — `parseUser()` and `formatUserLabel()` stay separate; not one `handleUser()`.
2. **O** — new payment = new function (`payWithWallet`), not a new `if` in `processOrder()`.
3. **L** — callbacks passed to `mapUsers()` always return the same shape; not sometimes a string.
4. **I** — compose `canRead(store)` for list-only code; split fat configs; not one `userUtils.js` with everything.
5. **D** — pass `notify` into `placeOrder()` and `http` into `loadProfile()`; don't hardcode `EmailService` or `fetch` inside.

**When to use:** Utility modules, Node services, data layers, refactoring god files; most day-to-day JS is functions-first.

**Examples:**

```js
// S — good: parse and format are separate functions
function parseUser(raw) {
  return JSON.parse(raw);
}

function formatUserLabel(user) {
  return `${user.name} <${user.email}>`;
}

// S — bad: one function parses and formats (two reasons to change)
// function handleUser(raw) {
//   const user = JSON.parse(raw);
//   return `${user.name} <${user.email}>`;
// }

// O + D — good: new payment = new function, processOrder stays closed
function processOrder(order, pay) {
  return pay(order.total);
}

function payWithCard(amount) {
  return { ok: true, method: "card", amount };
}

function payWithWallet(amount) {
  return { ok: true, method: "wallet", amount };
}

// O + D — bad: edit processOrder for every new payment type
// function processOrder(order) {
//   if (order.payment === "card") return { ok: true, method: "card" };
//   if (order.payment === "wallet") return { ok: true, method: "wallet" };
// }

// L — good: normalize always returns the same shape
function mapUsers(users, normalize = (u) => ({ id: u.id, name: u.name })) {
  return users.map(normalize);
}

// L — bad: callback sometimes returns a string — breaks mapUsers callers
// mapUsers(users, (u) => (u.active ? { id: u.id, name: u.name } : "skipped"));

// I — good: compose behavior from small mixin functions (no fat inheritance)
const canRead = (base) => ({
  ...base,
  read: (id) => base.store.get(id),
});
const canWrite = (base) => ({
  ...base,
  write: (id, data) => base.store.set(id, data),
});

const store = {
  data: new Map(),
  get: (id) => store.data.get(id),
  set: (id, v) => store.data.set(id, v),
};
const reader = canRead({ store }); // list-only consumer — no write/delete stubs

// I — bad: fat config forces unused keys on every consumer
// function initApp({ readUser, writeUser, deleteUser, auditUser, emailUser }) { ... }
// listPageOnlyNeeds({ readUser }); // still depends on write/delete/email shape

// D — good: inject notify — swap email or SMS in tests
function placeOrder(order, notify) {
  notify.send(order.userId, `Order ${order.id} placed`);
  return { ok: true, orderId: order.id };
}

const emailService = { send: (userId, msg) => `email: ${msg}` };
const smsService = { send: (userId, msg) => `sms: ${msg}` };
placeOrder({ id: "42", userId: "u1" }, emailService);
placeOrder({ id: "43", userId: "u2" }, smsService);

// D — bad: hardcoded EmailService inside domain function
// function placeOrder(order) {
//   const mailer = { send: (id, msg) => console.log(`email: ${msg}`) };
//   mailer.send(order.userId, `Order ${order.id} placed`);
// }

// D — good: inject http — swap fake in tests
async function loadProfile(userId, http) {
  return http.get(`/api/users/${userId}`);
}
// loadProfile("1", { get: async () => ({ id: "1", name: "Ada" }) });

// D — bad: hardcoded fetch — hard to test, tied to browser/Node global
// async function loadProfile(userId) {
//   const res = await fetch(`/api/users/${userId}`);
//   return res.json();
// }
```

**Thoughts:** Most JS is functions and modules, not classes — start here for everyday code. `class` / `extends`: [SOLID in Classes](#solid-in-classes). Framework mapping: [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Full reference: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [SOLID in Classes](#solid-in-classes) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

</details>
