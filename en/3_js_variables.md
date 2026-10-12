# JS Core — Variables

`let` / `const` / `var`, lexical scope, hoisting, `globalThis`, and destructuring (rest/spread).

[← Back to JS hub](./3_js.md)

### Related docs

- [Variables](./3_js_variables.md) · [Closures](./3_js_functions.md#closures) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [JS Functions](./3_js_functions.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [JS Language](./3_js_core.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area         | Topics                          |
| ------------ | ------------------------------- |
| **Bindings** | `let` / `const` / `var`, naming |
| **Scope**    | Global, function, block         |
| **Hoisting** | TDZ, function declarations      |
| **Patterns** | Rest, spread, destructuring     |

### Sections

- [Bindings & scope](#bindings-and-scope)
- [Rest, spread & destructuring](#rest-spread-and-destructuring)
- **Bindings & scope:** [globalThis](#globalthis) · [Variables](#variables) · [Scope](#scope) · [Hoisting](#hoisting)
- **Rest, spread & destructuring:** [Rest](#rest) · [Spread](#spread) · [Destructuring](#destructuring)
- [Interview](./3_js_interview.md)

Sibling Core topics: [Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Array](./3_js_array.md).

<a name="scope-closures"></a>

## Bindings & scope

### Bindings & scope

<details>
  <summary>globalThis</summary>

### globalThis

**Definition:** Standard way to access the global object in any environment (browser `window`, Node `global`, workers `self`).

**How it works:**

- `globalThis` is the same object across realms in a given JS environment.
- In browsers the global is also `window`; in Node `global`. Prefer `globalThis` for portable code.
- `var` globals and function declarations in classic scripts become properties of the global object; `let`/`const` do **not**.
- In strict mode / modules, bare `this` at top level is `undefined` — use `globalThis` when you truly need global.
- Prefer module scope or explicit exports over polluting global; attach only true polyfills (`if (!globalThis.Promise) ...`).

**When to use:** Library UMD bundles, test setup, rare polyfills. Avoid attaching app state to `globalThis`.

**Pluses and minuses:**

| +                     | −                             |
| --------------------- | ----------------------------- |
| Cross-environment API | Global pollution anti-pattern |
| Polyfill-friendly     |                               |

**Examples:**

```js
globalThis.setTimeout(fn, 0); // works in browser and Node

// classic script only — modules don't put let on global:
// var gVar = 5; → window.gVar === 5
// let gLet = 5; → window.gLet === undefined
```

**Sources:** [MDN globalThis](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) · [javascript.info/global-object](https://javascript.info/global-object)

</details>

<a name="variables"></a>

<details>
  <summary>Variables</summary>

### Variables

**Definition:** **Variables** are named bindings (`let`, `const`, `var`) stored in a lexical environment. `const` prevents rebinding the variable; object/array contents remain mutable.

**How it works:**

|                         | `var`       | `let` | `const`      |
| ----------------------- | ----------- | ----- | ------------ |
| Scope                   | function    | block | block        |
| Hoisting                | `undefined` | TDZ   | TDZ          |
| Redeclare in same scope | yes         | no    | no           |
| Reassign                | yes         | yes   | no (binding) |

Scope rules → [Scope](#scope). Hoisting and TDZ → [Hoisting](#hoisting).

**When to use:**

| Use                                                | Avoid                                |
| -------------------------------------------------- | ------------------------------------ |
| `const` by default                                 | `var` in new code                    |
| `let` when rebinding (`for` counter, accumulators) | `const` for values you will reassign |
| Block scope in `if`/`for` for temporary bindings   | Relying on hoisting for logic flow   |

**Naming:**

- Prefer camelCase (`userName`, `shoppingCart`); letters, digits, `$`, `_` only; not starting with a digit.
- Declare a binding **once** — re-`let` / re-`const` the same name in one scope is a `SyntaxError`.
- **Uppercase `CONST_NAME`:** only for hard-coded aliases known before run (`COLOR_RED = "#F00"`). Runtime-calculated constants stay camelCase (`const age = someCode(birthday)`).

```js
const COLOR_ORANGE = "#FF7F00"; // hard-coded → UPPER_SNAKE
const pageLoadTime = performance.now(); // computed once → camelCase
```

**Pluses and minuses:**

| + (`const`/`let`)                   | − (`var`)                            |
| ----------------------------------- | ------------------------------------ |
| Block scope limits leaks            | Function scope ignores `{}` blocks   |
| TDZ catches use-before-declare bugs | Hoisted `undefined` hides mistakes   |
| No accidental redeclaration         | Loop closure bugs with `var` + async |

**Definition (`var` loop pitfall vs `let`):** `var` in `for` creates one binding for the whole loop; `let` creates a new binding per iteration.

**How it works:**

- `var i` is function-scoped — all closures share the same `i` after loop ends.
- `let i` is block-scoped per iteration — each async callback captures its own `i`.

**When to use:** Always `let` in `for` loops with closures; use `for...of` when you don't need index.

**Examples:**

```js
// var — one shared binding
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); // 3, 3, 3
}

// let — new binding per iteration
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); // 0, 1, 2
}
```

**Sources:** [javascript.info/variables](https://javascript.info/variables) · [javascript.info/let-const](https://javascript.info/let-const)

</details>

<a name="scope"></a>

<details>
  <summary>Scope</summary>

### Scope

**Definition:** **Scope** determines where a binding is visible. JavaScript has three visibility levels — global, function, and block.

**How it works:**

- Each scope (global, function, block) has an Environment Record holding bindings.
- Inner scopes link to outer via `[[OuterEnv]]` — this chain enables [closures](./3_js_functions.md#closures).
- **Global** — module/script top level; in browsers, `var` also attaches to `window`.
- **Function** — parameters and `var`/`let`/`const` inside function body.
- **Block** — `let`/`const` inside `{}`, `if`, `for`, `while`.

**When to use:** Use block scope for temporaries; function scope only via `var` (legacy).

| Scope    | Created by                        | Example                         |
| -------- | --------------------------------- | ------------------------------- |
| Global   | Script/module top level           | `const API_URL = '...'`         |
| Function | `function` declaration/expression | `function f() { const x = 1; }` |
| Block    | `{ }` with `let`/`const`          | `if (ok) { const tmp = 1; }`    |

**Examples:**

```js
const global = 1;

function outer() {
  const local = 2;
  if (true) {
    const block = 3;
    // block visible here only
  }
  // console.log(block); // ReferenceError
}
```

**Pluses and minuses:**

| + (block scope)               | −                           |
| ----------------------------- | --------------------------- |
| Limits variable lifetime      | Must choose correct keyword |
| Prevents leak from `if`/`for` |                             |

**Sources:** [javascript.info/closure](https://javascript.info/closure)

</details>

<a name="hoisting"></a>

<details>
  <summary>Hoisting</summary>

### Hoisting

**Definition:** **Hoisting** registers declarations before the line runs. `var` is hoisted as `undefined`; `let`/`const` stay in the **temporal dead zone (TDZ)** until initialized. Function **declarations** hoist with their full body; **expressions** do not.

**How it works:**

- Parsing phase records bindings; execution phase assigns values.
- `var` bindings exist as `undefined` from start of scope.
- Accessing `let`/`const` before declaration → `ReferenceError` (TDZ).
- `typeof undeclaredVar` is safe; `typeof letBeforeDeclare` in TDZ throws.

Each function call gets a context (variable environment, `this`, call stack frame). The **lexical environment** is where closures get outer bindings — determined by **where code is written**, not where it runs.

**When to use:** Never use variables before declaration; use function declarations if you need hoisted helpers.

| Declaration            | Hoisted as          |
| ---------------------- | ------------------- |
| `var`                  | `undefined`         |
| `function` declaration | Full function       |
| `let`/`const`          | TDZ until line runs |

**Examples:**

```js
sayHi(); // works — declaration hoisted
function sayHi() {
  console.log("Hi");
}

console.log(a); // undefined — var hoisted
var a = 1;

// console.log(b); // ReferenceError — TDZ
let b = 2;
```

**Pluses and minuses:**

| + (TDZ)            | −                                         |
| ------------------ | ----------------------------------------- |
| Catches bugs early | Surprises developers from other languages |

**Sources:** [javascript.info/variables](https://javascript.info/variables) · [javascript.info/let-const](https://javascript.info/let-const)

</details>

## Rest, spread & destructuring

### Rest, spread & destructuring

<a name="rest"></a>

<details>
  <summary>Rest</summary>

### Rest

**Definition:** **Rest** (`...name`) collects the remaining elements of an array or properties of an object into a new array or object. In functions, **rest parameters** gather extra arguments into an array.

**How it works:**

- In destructuring, rest must be the **last** binding in the pattern.
- Creates a **shallow** copy of the “leftover” slice — nested objects stay shared.
- Function rest parameters replace the legacy `arguments` object in modern code.
- Same `...` token as spread, but position decides meaning: collect vs expand.

**When to use:**

| Feature         | Use when                                           |
| --------------- | -------------------------------------------------- |
| Rest in arrays  | `const [head, ...tail] = items`                    |
| Rest in objects | Omit fields (`const { password, ...safe } = user`) |
| Rest parameters | Variadic functions (`sum(...nums)`)                |

**Examples:**

```js
const [first, second, ...rest] = [1, 2, 3, 4];
// first=1, second=2, rest=[3, 4]

function sum(...nums) {
  return nums.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3); // 6

const { id, ...metadata } = { id: 1, a: 1, b: 2 };
// metadata = { a: 1, b: 2 }
```

**Pluses and minuses:**

| +                    | −                         |
| -------------------- | ------------------------- |
| Replaces `arguments` | Must be last in a pattern |
| Clear variadic APIs  | Shallow copy of leftovers |

**Sources:** [javascript.info/rest-parameters-spread](https://javascript.info/rest-parameters-spread)

</details>

<a name="spread"></a>

<details>
  <summary>Spread</summary>

### Spread

**Definition:** **Spread** (`...iterable`) expands an iterable into comma-separated values in an array literal, or copies enumerable own properties into an object literal.

**How it works:**

- In `[...arr, x]` and `{ ...obj, key: val }`, spread **expands** into the new literal.
- Produces **shallow** copies — nested objects/arrays are still shared references.
- Order matters in object spread: later keys overwrite earlier ones.

**When to use:**

| Feature       | Use when                          |
| ------------- | --------------------------------- |
| Array spread  | Copy, concat, prepend/append      |
| Object spread | Immutable merge / override fields |

#### Common pitfalls

```js
const inner = { x: 1 };
const nested = { data: inner };
const clone = { ...nested };
clone.data.x = 99;
inner.x; // 99 — shared nested reference
```

**Examples:**

```js
const copy = [...arr, newItem];
const merged = { ...defaults, ...userOverrides };
```

**Pluses and minuses:**

| +                               | −                         |
| ------------------------------- | ------------------------- |
| Concise immutable-style updates | Shallow only              |
| Readable merges                 | Easy to assume deep clone |

**Sources:** [javascript.info/rest-parameters-spread](https://javascript.info/rest-parameters-spread)

</details>

<a name="destructuring"></a>

<details>
  <summary>Destructuring</summary>

### Destructuring

**Definition:** **Destructuring** unpacks array or object values into distinct variables using pattern syntax.

**How it works:**

- Array patterns match by **position**; object patterns by **property name** (with rename and defaults).
- Default values apply when the source is `undefined` (not when the key is missing vs `null` — check each case).
- **Smart parameters:** `function show({ title = "Menu", width = 100 } = {})` — default `= {}` so a bare call works.
- Assignment without `let`/`const`: wrap in parens — `({ title, width } = options)` — otherwise `{` is parsed as a block.
- Rest in a pattern collects leftovers → [Rest](#rest).

**When to use:**

| Feature              | Use when                         |
| -------------------- | -------------------------------- |
| Array destructuring  | Swap values, tuple-like returns  |
| Object destructuring | Props, API responses, options    |
| Options object param | Many optional args with defaults |

#### Common pitfalls

```js
const {
  address: { city },
} = {}; // TypeError — nested destructuring, undefined parent

let title, width;
({ title, width } = { title: "Menu", width: 200 }); // needs parens
```

**Examples:**

```js
let a = 1,
  b = 2;
[a, b] = [b, a]; // swap

const { years: age, isAdmin = false } = { name: "John", years: 30 };

function showMenu({ title = "Untitled", width = 200, height = 100 } = {}) {
  return `${title} ${width}x${height}`;
}
showMenu();
```

**Pluses and minuses:**

| +                   | −                                   |
| ------------------- | ----------------------------------- |
| Concise unpacking   | Easy to shadow outer variable names |
| Readable parameters | Nested patterns fail on `undefined` |

**Sources:** [javascript.info/destructuring-assignment](https://javascript.info/destructuring-assignment)

</details>
