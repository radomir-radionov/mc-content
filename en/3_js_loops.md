# JS Core — Loops

Control flow (`if` / loops / `switch`), error handling (`try`/`catch`), and iterators / generators.

[← Back to JS hub](./3_js.md)

### Related docs

- [JS Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Operators](./3_js_operators.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [JS Arrays](./3_js_array.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [Computer science](./7_special_cs.md)

### Main concepts

| Area             | Topics                                                    |
| ---------------- | --------------------------------------------------------- |
| **Control flow** | `if` / ternary, loops, `for...in` vs `for...of`, `switch` |
| **Errors**       | `try`/`catch`, custom `Error` subclasses                  |
| **Iterators**    | Iterable protocol, `function*` generators                 |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Control flow](#control-flow)
- [Error handling](#error-handling)
- [Iterators](#iterators)
- **Control flow:** [Conditional branching](#conditional-branching) · [Loops, break, continue & switch](#loops-break-continue-switch) · [for...in vs for...of](#for-in-vs-for-of-extended) · [switch — extended notes](#switch-fall-through-types)
- **Error handling:** [Error handling](#error-handling) · [try...catch](#try-catch) · [Custom errors, extending Error](#custom-errors-extending-error)
- **Iterators:** [Advanced iteration](#advanced-iteration) · [Generators](#generators)
- [Interview](./3_js_interview.md)

Sibling Core topics: [Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Operators](./3_js_operators.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Array](./3_js_array.md) · [Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md).

<a name="control-flow"></a>

## Control flow

<details>
  <summary>if, else, and ternary</summary>

### Conditional branching

**Definition:** `if`/`else` run statements based on truthy/falsy values; ternary `cond ? a : b` is an **expression** that returns a value.

**How it works:**

- Condition coerces to boolean (falsy: `false`, `0`, `''`, `null`, `undefined`, `NaN`, `0n`).
- `else if` chains multiple conditions; first match wins.
- Ternary nests but prefer `if` for readability when complex.
- `switch` uses `===` against each `case` (see loops section).

**When to use:** `if` for multi-line branches; ternary for simple assign/render; early `return` to flatten logic.

**Pluses and minuses:**

| +                                | −                                      |
| -------------------------------- | -------------------------------------- |
| Clear control flow               | Deep nesting hard to read              |
| Ternary handy in JSX/assignments | Easy to omit braces and introduce bugs |

**Examples:**

```js
const age = 20;
const label = age < 18 ? "minor" : "adult";

if (age >= 18) {
  console.log("adult");
} else if (age >= 13) {
  console.log("teen");
} else {
  console.log("child");
}
```

**Sources:** [javascript.info/ifelse](https://javascript.info/ifelse)

</details>

<details>
  <summary>Loops and switch</summary>

### Loops, break, continue & switch

**Definition:** Loops repeat code; `break` exits a loop/switch; `continue` skips to next iteration. `switch` matches `case` with `===`.

**How it works:**

- `while` — test before body; may run zero times.
- `do...while` — body at least once.
- `for (init; cond; step)` — classic counter; use `let` for per-iteration binding.
- `for...of` — iterates **values** of iterables.
- `for...in` — iterates **keys** (including inherited); filter with `Object.hasOwn`.
- `switch` — **always `===`**; fall-through without `break`.
- Labels: `outer: for (...) { ... break outer; }` exits the labeled loop (needed for nested loops). `continue outer` jumps to the next iteration of that loop.

| Construct    | Use                                 |
| ------------ | ----------------------------------- |
| `while`      | Unknown iterations, condition-first |
| `do...while` | At least one run                    |
| `for`        | Index counters                      |
| `for...of`   | Arrays, strings, Map, Set           |
| `for...in`   | Object keys (with `hasOwn`)         |
| `switch`     | Many discrete equal values          |

**When to use:** Arrays → `for...of` or `map`; objects → `Object.entries` + `for...of`; enums → `switch`; custom/`function*` sequences → [Iterators](#iterators); async loops → see [JS Async](./3_js_async_await.md).

**Pluses and minuses:**

| +                               | −                                                |
| ------------------------------- | ------------------------------------------------ |
| `for...of` clean on iterables   | `for...in` on arrays is buggy                    |
| `switch` readable for enums     | Fall-through footguns; `===` vs `prompt` strings |
| `break`/`continue` control flow | Labels rarely needed                             |

**Examples:**

```js
for (let i = 0; i < 3; i++) console.log(i);

for (const v of ["a", "b"]) console.log(v);

for (const key in obj) {
  if (!Object.hasOwn(obj, key)) continue;
  console.log(key, obj[key]);
}

outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (!input) break outer; // leave both loops
  }
}

switch (status) {
  case "active":
    handleActive();
    break;
  case "draft":
  case "pending":
    handleDraft();
    break;
  default:
    handleUnknown();
}
```

**Sources:** [javascript.info/while-for](https://javascript.info/while-for) · [javascript.info/switch](https://javascript.info/switch)

</details>

<details>
  <summary>for...in vs for...of — extended</summary>

### for...in vs for...of

**Definition:** `for...in` iterates **enumerable property keys** (strings), including inherited ones on the prototype chain. `for...of` iterates **values** from objects that implement the **iterable protocol** (`[Symbol.iterator]`).

**How it works:**

- `for...in` on arrays yields indices as strings (`"0"`, `"1"`, …) plus any enumerable keys added to the array object — usually wrong for dense arrays; prefer `for...of` or `forEach`.
- Filter object keys with `Object.hasOwn(obj, key)` (or `Object.keys`) — skip inherited properties like `toString`.
- `for...of` on `Map` yields `[key, value]` entries; on `Set` yields values; on `String` yields UTF-16 code units.
- **Array-likes** without `Symbol.iterator` need `Array.from` before `for...of`. Details → [Advanced iteration](#advanced-iteration).

**When to use:** Objects → `for...in` + `hasOwn` or `Object.entries` + `for...of`. Arrays/strings/collections → `for...of`.

**Examples:**

```js
const obj = { a: 1, b: 2 };
for (const key in obj) {
  if (Object.hasOwn(obj, key)) console.log(key, obj[key]);
}

for (const ch of "hi") console.log(ch); // 'h', 'i'

const arr = ["x", "y"];
for (const index in arr) console.log(index); // "0", "1" — usually avoid
for (const value of arr) console.log(value); // 'x', 'y'
```

**Sources:** [javascript.info/array#loops](https://javascript.info/array#loops) · [Advanced iteration](#advanced-iteration)

</details>

<details>
  <summary>switch — extended notes</summary>

### switch — fall-through & types

**Definition:** Without `break`, execution falls through to the next `case`. Use `break` unless fall-through is intentional. Matching is always **strict** (`===`) — types must match.

**How it works:**

- Grouping cases (`case 2: case 3: ...`) shares one body via intentional fall-through.
- `prompt` / form values are **strings**. `case 3:` never matches `"3"` from `prompt` — use `case '3':` or coerce (`+prompt(...)`) first.

**Examples:**

```js
switch (x) {
  case 1:
    console.log("one");
    break;
  case 2:
  case 3:
    console.log("two or three"); // grouped
    break;
  default:
    console.log("other");
}

const arg = prompt("Enter a value?"); // string | null
switch (arg) {
  case "0":
  case "1":
    console.log("one or zero");
    break;
  case 3:
    console.log("never runs for prompt — types differ");
    break;
  default:
    console.log("unknown");
}
```

**Sources:** [javascript.info/switch](https://javascript.info/switch)

</details>

---

<a name="error-handling"></a>

## Error handling

### Errors and exceptions

<details>
  <summary>Error handling</summary>

### Error handling

**Definition:** When something goes wrong, JavaScript **throws** a value (prefer `Error` instances). The runtime unwinds the **call stack** until a matching `catch` runs or the process / global handler reports an **uncaught** error.

**How it works:**

- `throw value` stops normal execution in the current function; callers can catch with `try/catch`.
- Built-in errors: `Error`, `TypeError`, `ReferenceError`, `SyntaxError`, … — all subclass `Error`.
- Uncaught sync errors: browser `window.onerror` / Node `process.on('uncaughtException')` (last resort).
- Promise rejections without `.catch` / `await` → [Unhandled promise rejections](./3_js_async_promises.md#unhandled-promise-rejections).
- Syntax errors in parsed code cannot be caught around the `eval` of the same script tag in all cases — fix at compile time.

**When to use:** Expected failures at boundaries (parse, validate); let programming bugs propagate or crash in dev.

**Pluses and minuses:**

| +                     | −                                      |
| --------------------- | -------------------------------------- |
| Explicit failure path | Easy to over-catch and hide bugs       |
| `Error` carries stack | Throwing non-`Error` loses stack hints |

**Sources:** [javascript.info/try-catch](https://javascript.info/try-catch) · [try...catch](#try-catch) · [Custom errors](#custom-errors-extending-error)

</details>

<details>
  <summary>try...catch</summary>

### try...catch

**Definition:** `try { ... } catch (e) { ... } finally { ... }` handles **synchronous** throws in the `try` block. `finally` runs on both success and failure (unless the process exits).

**How it works:**

- `catch (err)` binds the thrown value; use `instanceof Error` before reading `.message`.
- Optional catch binding: `catch { ... }` when you do not need the error object.
- Rethrow: `catch (e) { if (cannotRecover) throw e; }` — do not swallow unknown errors.
- `finally` is for cleanup (close files, release locks); avoid `return` in `finally` (overrides `try`/`catch` return).
- **Does not catch:** throws inside `setTimeout` / event handlers scheduled later; rejected promises — use [try...catch with promises](./3_js_async_promises.md#try-catch-with-promises) or `async`/`await` `try`.

**When to use:** `JSON.parse`, file read wrappers, user input validation, local recovery.

**Examples:**

```js
try {
  JSON.parse("{ invalid");
} catch (err) {
  console.error(err instanceof Error ? err.message : err);
} finally {
  console.log("always runs");
}

try {
  risky();
} catch {
  console.log("failed — error object optional");
}
```

**Sources:** [javascript.info/try-catch](https://javascript.info/try-catch) · [MDN try...catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch)

</details>

<details>
  <summary>Custom errors, extending Error</summary>

### Custom errors, extending Error

**Definition:** Domain code throws **`class MyError extends Error`** (or deeper hierarchies) so callers distinguish failure kinds with `instanceof` and read extra fields (`status`, `property`, …).

**How it works:**

- Call `super(message)` in constructor; set `this.name = "MyError"` or `this.name = this.constructor.name` for clear stacks.
- **`cause`:** `new Error("wrap", { cause: original })` (ES2022) — chain low-level failures.
- **Wrapping:** catch `SyntaxError` in `readUser`, throw `ReadError` with `cause` — API surface stays one error type.
- Subclass checks: `err instanceof ValidationError` walks the prototype chain.
- Class syntax details: [Classes — Extending built-in classes](./3_js_classes.md#extending-built-in-classes) (`HttpError extends Error`).

**When to use:** Validation layers, HTTP/API errors, library public APIs.

**Examples:**

```js
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = this.constructor.name;
  }
}

class ValidationError extends MyError {}
class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super(`No property: ${property}`);
    this.property = property;
  }
}

class ReadError extends Error {
  constructor(message, cause) {
    super(message, { cause });
    this.name = "ReadError";
  }
}
```

**Sources:** [javascript.info/custom-errors](https://javascript.info/custom-errors) · [Classes — Extending built-in classes](./3_js_classes.md#extending-built-in-classes)

</details>

---

<a name="iterators"></a>

## Iterators

### Iteration protocols

<details>
  <summary>Advanced iteration</summary>

### Advanced iteration

**Definition:** An **iterable** is any object with `[Symbol.iterator]()` returning an **iterator**. Iterators implement `.next()` → `{ value, done }`. Built-ins and custom collections use this for `for...of`, spread (`...`), and destructuring.

**How it works:**

- `for (const x of iterable)` calls `iterable[Symbol.iterator]()` once, then `.next()` until `done: true`.
- **Iterator** is stateful (cursor); **iterable** can produce many independent iterators (`[1,2][Symbol.iterator]()` each time).
- `String` is iterable by **code units** (surrogate pairs may split — use `Intl.Segmenter` or libraries for graphemes if needed).
- `Map` iterates `[key, value]` pairs; `Set` iterates values.
- **Array-likes** (`{ length, 0: 'a' }`, `NodeList`) are not always iterable — `Array.from(list)` or `[...list]` when `Symbol.iterator` exists.
- Implement custom iterables for tree walks, paged APIs, or internal collections.

| Built-in iterable | `for...of` yields      |
| ----------------- | ---------------------- |
| `Array`           | elements               |
| `String`          | UTF-16 code units      |
| `Map`             | `[key, value]` entries |
| `Set`             | values                 |

**When to use:** Uniform loops over arrays/strings/maps; custom data sources; avoid manual index loops when `for...of` suffices.

**Examples:**

```js
const iterable = {
  [Symbol.iterator]() {
    let step = 0;
    return {
      next() {
        step++;
        return step <= 3 ? { value: step, done: false } : { done: true };
      },
    };
  },
};

for (const n of iterable) console.log(n); // 1, 2, 3

Array.from("hi"); // ['h', 'i']
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
const [first, ...rest] = iterable; // uses iterator protocol
```

**Sources:** [javascript.info/iterable](https://javascript.info/iterable) · [MDN: Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) · [Generators](#generators)

</details>

<details>
  <summary>Generators</summary>

### Generators

**Definition:** A **generator** is a function declared with `function*` that returns an iterator. `yield` pauses execution and emits a value; `.next()` resumes until the next `yield` or `return`.

**How it works:**

- `function* gen() { yield 1; yield 2; }` — calling `gen()` returns a generator object (iterator + generator methods).
- `yield* otherIterable` delegates iteration to another generator or iterable.
- **Two-way flow:** `const x = yield value` — argument to `.next(arg)` becomes the result of `yield` inside the generator.
- `generator.return(v)` forces completion; `generator.throw(err)` injects into the paused `yield` (or propagates if uncaught).
- Generators are **lazy** — work happens per `.next()`; infinite sequences are possible (do not spread infinite gens without limits).
- Async generators: [Async iterators](./3_js_async_await.md#async-iterators-and-for-awaitof).

**When to use:** Lazy sequences, custom iterators with less boilerplate, delegating iteration; state machines.

**Pluses and minuses:**

| +                                           | −                            |
| ------------------------------------------- | ---------------------------- |
| Less boilerplate than hand-rolled `.next()` | Harder to debug paused state |
| Composable via `yield*`                     | Easy to leak infinite loops  |

**Examples:**

```js
function* idGenerator() {
  let id = 1;
  while (true) yield id++;
}

const gen = idGenerator();
gen.next().value; // 1
[...idGenerator()].slice(0, 3); // [1, 2, 3]

function* range(start, end) {
  for (let i = start; i <= end; i++) yield i;
}

function* delegate() {
  yield* range(1, 3);
  yield "done";
}
```

**Sources:** [javascript.info/generators](https://javascript.info/generators) · [Advanced iteration](#advanced-iteration)

</details>
