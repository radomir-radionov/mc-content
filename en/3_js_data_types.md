# JS Core — Data Types

Primitives, coercion, and the memory model (stack vs heap). Bindings and structured data live in sibling Core topics.

[← Back to JS hub](./3_js.md)

### Related docs

- [Variables](./3_js_variables.md) · [Closures](./3_js_functions.md#closures) · [Data Structures](./3_js_data_structures.md) · [JS Language](./3_js_core.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [JS Arrays](./3_js_array.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Interview](./3_js_interview.md)
- [Iterables](./3_js_loops.md#iterators) · [Computer science](./7_special_cs.md)

### Main concepts

| Area         | Topics                                                       |
| ------------ | ------------------------------------------------------------ |
| **Types**    | Each type in short, then `undefined` … `symbol` and `object` |
| **Common**   | Memory, checks, conversions, template literals               |
| **Advanced** | `Object.is`, abstract coercion, wrappers, clone vs JSON      |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

- [Types](#types)
- [Common](#common)
- [Advanced](#advanced)
- [Each type in short](#each-type-in-short) · [undefined](#undefined) · [null](#null) · [boolean](#boolean) · [Numbers](#numbers) · [BigInt](#bigint) · [Strings](#strings) · [Symbol](#symbol) · [object](#object)
- [Stack and heap](#memory-stack-heap) · [typeof](#typeof) · [Falsy values](#falsy-values) · [Object keys](#object-keys) · [Type conversions](#type-conversions) · [Template literals](#template-literals)
- [Object.is & SameValueZero](#object-is-and-samevaluezero) · [ToPrimitive & abstract conversions](#toprimitive-and-abstract-conversions) · [Primitive wrappers](#primitive-wrappers) · [JSON vs structuredClone](#json-vs-structuredclone)

- [Interview](./3_js_interview.md)

Sibling Core topics: [Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md)

## Types

### Types

<a name="data-types"></a>
<a name="each-type-in-short"></a>

<details>
  <summary>Each type in short</summary>

### Each type in short

**Definition:** ECMAScript has **7 primitive types** and **one non-primitive**: **`object`**. Primitives are immutable and copied by value; objects are mutable and copied by reference. **Functions** are objects; only `typeof` labels them `'function'`. This block is the overview and cheat sheet; per-type essays follow in this section.

**How it works:**

**Quick reference (interview)**

| Type        | Example          | `typeof`                  | One line                                                     |
| ----------- | ---------------- | ------------------------- | ------------------------------------------------------------ |
| `undefined` | `let x;`         | `'undefined'`             | “Not assigned”; only value is `undefined`                    |
| `null`      | `null`           | `'object'` ⚠️             | Intentional empty reference; use `=== null`                  |
| `boolean`   | `true` / `false` | `'boolean'`               | Logical values only; coerces in `if` / `&&` / `\|\|`         |
| `number`    | `42`, `NaN`      | `'number'`                | IEEE-754 float64; see [Numbers](#numbers)                    |
| `bigint`    | `123n`           | `'bigint'`                | Arbitrary integers; no mix with `number` without conversion  |
| `string`    | `'hi'`           | `'string'`                | Immutable UTF-16; copied by value                            |
| `symbol`    | `Symbol('id')`   | `'symbol'`                | Always-unique identifier; hidden from `Object.keys` / JSON   |
| `object`    | `{}`, `[]`, `fn` | `'object'` / `'function'` | Mutable reference type; `===` is identity, not deep equality |

**Primitives (7)** — stored directly; assignment copies the value; `===` compares values:

| Primitive   | Example        | Deep topic (this file)  |
| ----------- | -------------- | ----------------------- |
| `undefined` | `let x;`       | [undefined](#undefined) |
| `null`      | `null`         | [null](#null)           |
| `boolean`   | `true`         | [boolean](#boolean)     |
| `number`    | `42`, `NaN`    | [Numbers](#numbers)     |
| `bigint`    | `123n`         | [BigInt](#bigint)       |
| `string`    | `'hi'`         | [Strings](#strings)     |
| `symbol`    | `Symbol('id')` | [Symbol](#symbol)       |

**Non-primitive — `object`** — a **reference** to a heap object; assignment copies the pointer; `===` compares identity. Deep topic → [object](#object):

| Kind (all `typeof` → `'object'` except functions)       | Example                       |
| ------------------------------------------------------- | ----------------------------- |
| Plain object                                            | `{ a: 1 }`                    |
| Array                                                   | `[1, 2]`                      |
| `Date`, `RegExp`, `Map`, `Set`, …                       | `new Date()`                  |
| **Function** (callable object; `typeof` → `'function'`) | `() => {}`, `function f() {}` |

`null` is a **primitive** — see [typeof](#typeof) for why `typeof null` is misleading.

- **Primitives** — assign/copy by value; `===` compares values.
- **Objects** (arrays, functions, dates, …) — assign/copy by reference; `===` compares reference identity.
- **Boxing:** method calls on primitives use temporary wrappers (`'hi'.toUpperCase()`). Avoid `new String()` / `new Number()` — wrappers break `===` ([Object keys](#object-keys)).

Reliable checks → [typeof](#typeof). Falsy list → [Falsy values](#falsy-values). Memory teaching model → [Memory: stack and heap](#memory-stack-heap).

**Pluses and minuses:**

| + (primitives)                    | − (primitives)               |
| --------------------------------- | ---------------------------- |
| Predictable copy semantics        | No methods without boxing    |
| Thread-safe in single-threaded JS | `number` has IEEE-754 limits |

| + (objects)               | − (objects)                      |
| ------------------------- | -------------------------------- |
| Mutable, extensible       | Shared references cause bugs     |
| Rich APIs (arrays, dates) | `typeof` too coarse for subtypes |

**Examples:**

```js
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x); // 2 (shared reference)

const s1 = "hi";
const s2 = s1;
// independent — strings are immutable primitives
```

**Sources:** [javascript.info/types](https://javascript.info/types) · [MDN typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<a name="undefined"></a>

<details>
  <summary>undefined</summary>

### undefined

**Definition:** **`undefined`** means a variable has been declared but not assigned, a missing object property, or a function with no `return`. It is a primitive; `typeof` → `'undefined'`.

**How it works:**

- Only one value: `undefined` (global `undefined` is non-writable in modern environments).
- `let x;` / `var x;` → `x === undefined`. Missing key: `({}).foo === undefined`.
- `void expression` always evaluates to `undefined` (useful for IIFE / bookmarklets).
- `undefined` is **falsy** but distinct from `null` (`null === undefined` is false).
- `Number(undefined)` → `NaN`; `String(undefined)` → `'undefined'`.

**When to use:** Prefer **not** assigning `undefined` explicitly — use `null` for “empty on purpose” or omit the property. Use `value === undefined` (or `value == null` with both nullish) instead of `typeof` when you mean “missing binding value”.

**Examples:**

```js
let a;
a === undefined; // true
void 0; // undefined
({}).missing; // undefined
Number(undefined); // NaN
```

**Sources:** [MDN undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) · [javascript.info/types](https://javascript.info/types)

</details>

<a name="null"></a>

<details>
  <summary>null</summary>

### null

**Definition:** **`null`** is a primitive meaning “no object” or an intentional empty value. It is not the same as `undefined`.

**How it works:**

- Literal only: `null`. `typeof null` → `'object'` (legacy bug) — always use `value === null`.
- `null` is **falsy**. `null == undefined` is true; `null === undefined` is false.
- `Number(null)` → `0`; `String(null)` → `'null'`.
- JSON: `JSON.stringify({ x: null })` keeps `null`; `undefined` properties are omitted.

**When to use:** APIs that mean “explicitly empty” (DOM `getElementById` miss → `null`). Prefer `??` for nullish fallback. Do not use `null` and `undefined` interchangeably in public APIs.

**Examples:**

```js
document.getElementById("missing"); // null (in browser)
value == null; // null OR undefined
value === null; // null only
JSON.stringify({ a: undefined, b: null }); // '{"b":null}'
```

**Sources:** [MDN null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null) · [MDN typeof — null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<a name="boolean"></a>

<details>
  <summary>boolean</summary>

### boolean

**Definition:** **`boolean`** is the primitive with exactly two values: `true` and `false`.

**How it works:**

- `typeof true` → `'boolean'`. Only `true` and `false` are booleans (not `0` / `1` unless coerced).
- Logical ops: `&&`, `||`, `!` return booleans or the **last evaluated operand** (short-circuit).
- `Boolean(x)` / `!!x` / `if (x)` use the [falsy list](#falsy-values) — only eight values are falsy.
- `Number(true)` → `1`, `Number(false)` → `0`; `String(true)` → `'true'`.

**When to use:** Flags and conditions. Prefer explicit `Boolean(x)` when the return type must be boolean; avoid `if (arr.length)` when `0` is valid — compare explicitly.

**Examples:**

```js
Boolean("0"); // true — string is truthy
Boolean(0); // false
!!"hello"; // true
true && "ok"; // 'ok' (operand, not always boolean)
```

**Sources:** [MDN Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) · [javascript.info/type-conversions](https://javascript.info/type-conversions)

</details>

<a name="numbers"></a>

<details>
  <summary>Numbers</summary>

### Numbers

**Definition:** Regular JS numbers are IEEE-754 **float64** doubles (safe integers ±`2^53−1`). `BigInt` covers larger integers. Write large/small literals with `_` separators, `e` notation, or `0x` / `0b` / `0o` prefixes.

**How it works:**

- Literals: `1_000_000_000`, `1e9`, `1e-6`, `0xff`, `0b11111111`, `0o377`.
- `num.toString(base)` — base `2`…`36` (hex colors, short IDs). Call on a literal with two dots: `255..toString(16)` or `(255).toString(16)`.
- Rounding: `Math.floor` / `ceil` / `round` / `trunc`; precision via multiply-divide or `toFixed(n)` (**returns a string**).
- Float gotcha: `0.1 + 0.2 !== 0.3` — compare with epsilon or round money (`toFixed` / integer cents).
- Checks: prefer `Number.isNaN` / `Number.isFinite` / `Number.isInteger` / `Number.isSafeInteger` over global `isNaN` / `isFinite` (those coerce).
- Parsing: `parseInt(str, radix)` / `parseFloat(str)` stop at first invalid char; `Number(str)` / `+str` require the whole string to be numeric (after trim).
- Arbitrary integers → [BigInt](#bigint).

**When to use:** Arithmetic, IDs, measurements. Money → round carefully or use integer minor units / `BigInt` / a decimal library. Never use `new Number()`.

#### Common pitfalls

```js
0.1 + 0.2; // 0.30000000000000004
(12.34).toFixed(1); // "12.3" — string, not number
parseInt("100px", 10); // 100
Number("100px"); // NaN
isNaN("hello"); // true — coerces
Number.isNaN("hello"); // false
(123456).toString(36); // "2n9c"
```

**Pluses and minuses:**

| +                             | −                                                 |
| ----------------------------- | ------------------------------------------------- |
| One number type for most math | Binary float imprecision                          |
| Rich `Math.*` + radix convert | `toFixed` returns string                          |
| Safe integer helpers          | Integers beyond `MAX_SAFE_INTEGER` lose precision |

**Examples:**

```js
Math.round(1.23456 * 100) / 100; // 1.23
+(1.005).toFixed(2); // often 1 — float trap; prefer cents

Number.isFinite(Infinity); // false
Number.isSafeInteger(2 ** 53); // false
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

**Thoughts:** _Treat float equality as approximate; treat `parseInt` vs `Number` as “prefix vs whole-string.”_

**Sources:** [javascript.info/number](https://javascript.info/number) · [MDN Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) · [MDN Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

</details>

<a name="bigint"></a>

<details>
  <summary>BigInt</summary>

### BigInt

**Definition:** `bigint` stores arbitrary integers (`123n` or `BigInt("123")`). It is a separate primitive from `number` (IEEE-754 float).

**How it works:**

- Suffix `n` or `BigInt(...)`. Operations stay in BigInt (`5n / 2n === 2n` — integer division).
- **Cannot mix** with `number` (`1n + 2` throws) — convert with `Number()` / `BigInt()` explicitly.
- `==` can coerce (`1 == 1n`); `===` does not. Unary `+` on bigint is invalid.
- `typeof 1n` → `'bigint'`. `0n` is falsy.

**When to use:** IDs or math beyond `Number.MAX_SAFE_INTEGER`; not for money floats (still use integers in minor units or a decimal library).

**Examples:**

```js
1n + 2n; // 3n
// 1n + 2; // TypeError
Number(1n) + 2; // 3
1 == 1n; // true
1 === 1n; // false
```

**Sources:** [javascript.info/bigint](https://javascript.info/bigint) · [MDN BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

</details>

<a name="strings"></a>

<details>
  <summary>Strings</summary>

### Strings

**Definition:** `string` is an immutable UTF-16 primitive. Methods return new strings; the value is copied by assignment.

**How it works:**

- Common methods: `slice`, `includes`, `startsWith`, `trim`, `split`, `replace`, `replaceAll`.
- Strings are **iterable** (`for...of` / `Array.from`) and handle surrogate pairs correctly — index/`length` still count UTF-16 code units.
- Surrogate pairs (emoji, many CJK): `'😂'.length === 2`; use `codePointAt` / `fromCodePoint` / `for...of`, not `charAt` / `charCodeAt` alone.
- Visually identical strings may differ by mark order — `str.normalize()` (NFC) before compare/store.
- Escape sequences: `\n`, `\t`, `\u{1F600}`.

**When to use:** User-facing text, URL building, `split`/`join` parsing, regex on strings. Use `trim()` on form input; `includes` for simple search before RegExp.

**Pluses and minuses:**

| +                         | −                                                        |
| ------------------------- | -------------------------------------------------------- |
| Immutable — safe to share | Concat in loops can be slow (use `join` or array + join) |
| Rich string API           | UTF-16 — surrogate pairs affect `.length`                |

**Examples:**

```js
"  hi  ".trim(); // 'hi'
"a-b-c".split("-"); // ['a','b','c']
["a", "b"].join("-"); // 'a-b'
"hello".includes("ell"); // true
"hello".replaceAll("l", "L"); // 'heLLo'
[..."𝒳😂"]; // ['𝒳', '😂']
"S\u0307\u0323".normalize() === "S\u0323\u0307".normalize(); // true
"𝒳".codePointAt(0).toString(16); // '1d4b3'
```

Interpolation and multiline text → [Template literals](#template-literals).

**Sources:** [javascript.info/string](https://javascript.info/string) · [javascript.info/unicode](https://javascript.info/unicode) · [javascript.info/primitives-methods](https://javascript.info/primitives-methods)

</details>

<a name="symbol"></a>

<details>
  <summary>Symbol</summary>

### Symbol

**Definition:** `symbol` is a primitive for **unique identifiers** — every `Symbol()` is distinct even with the same description.

**How it works:**

- Created with `Symbol('description')` or `Symbol.for('globalKey')` (global registry).
- `Symbol.keyFor(sym)` returns the registry key for **global** symbols only (`undefined` for local ones); use `sym.description` for any symbol’s label.
- Used as object keys that won't collide with string keys or other libraries.
- Symbol keys are **omitted** from `Object.keys`, `JSON.stringify`, and `for...in`.
- Use `Object.getOwnPropertySymbols(obj)` or `Reflect.ownKeys` to list them.
- `Object.assign` **does** copy symbol properties (unlike `for...in` / `Object.keys`).
- Well-known symbols: `Symbol.iterator`, `Symbol.toPrimitive`, `Symbol.toStringTag`, `Symbol.hasInstance`.

**When to use:** Private-ish object metadata, library extension points, defining custom iterables. Prefer `class` private fields `#x` for true class privacy in modern code.

**Pluses and minuses:**

| +                         | −                       |
| ------------------------- | ----------------------- |
| Guaranteed unique keys    | Not JSON-serializable   |
| Hides internal properties | Harder to debug/inspect |
| Powers iteration protocol | Easy to overuse         |

**Examples:**

```js
const id = Symbol("id");
const user = { name: "Ann", [id]: 42 };

Object.keys(user); // ['name']
user[id]; // 42
Symbol.for("app") === Symbol.for("app"); // true — global registry
Symbol.keyFor(Symbol.for("app")); // "app"
Symbol.keyFor(Symbol("app")); // undefined — not global

Object.assign({}, user)[id]; // 42 — symbols are copied
```

**Sources:** [javascript.info/symbol](https://javascript.info/symbol)

</details>

<a name="object"></a>

<details>
  <summary>object</summary>

### object

**Definition:** **`object`** is the non-primitive type: collections of properties. Arrays, functions, dates, and plain `{}` are objects. Assignment copies **references**; `===` compares identity.

**How it works:**

- Plain object: `{ key: value }`. Keys (except `Symbol`) stringify — [Object keys](#object-keys).
- **Arrays** are objects with indexed properties and `length`; use `Array.isArray`.
- **Functions** are callable objects; `typeof fn` → `'function'` (not `'object'`).
- `null` is **not** an object despite `typeof null` — it is a primitive sentinel.
- Mutations are visible through every reference: `const o = { a: 1 }; const p = o; p.a = 2` → `o.a === 2`.
- Deep compare is not built-in — `JSON.stringify` (limited) or a library / recursive compare for tests.

**When to use:** Records, caches, instances, and APIs. Prefer `const` for bindings (the reference is fixed, properties may still mutate). Structured data patterns → [Data Structures](./3_js_data_structures.md).

**Pluses and minuses:**

| +                         | −                                   |
| ------------------------- | ----------------------------------- |
| Mutable, extensible       | Shared references cause subtle bugs |
| Rich builtins (Map, Date) | `typeof` too coarse for subtypes    |

**Examples:**

```js
const a = { n: 1 };
const b = a;
b.n = 2;
a.n; // 2
Array.isArray([]); // true
typeof (() => {}); // 'function'
```

**Sources:** [javascript.info/object](https://javascript.info/object) · [MDN Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

</details>

## Common

### Common

<details>
  <summary>Stack and heap</summary>

<a name="memory-stack-heap"></a>

### Memory: stack and heap

**Definition:** A process gets an isolated **virtual** address space. Fast **stack** memory holds call frames (LIFO); the **heap** holds values whose size is unknown or must outlive a function. JS engines GC the heap; you do not `malloc`/`free`. Knowing this model makes DevTools (stack traces, heap snapshots) and interview questions about assignment / calls easier.

**How it works:**

- **Mental CPU model:** von Neumann — instructions and data share one store (one bus). Harvard splits them. Real chips are a hybrid (e.g. separate L1 I-cache / D-cache). Memory is numbered **cells**; the number is a fixed-width **address** (random access, not only sequential). Values larger than one cell span several. 64-bit width → 2⁶⁴ addresses (≈ 16 EiB of _address space_, not “you have 18 EB of RAM”).
- **Virtual vs physical:** the CPU maps virtual addresses to RAM. The OS hands the process **pages** (often 4–8 KB), many of them.
- **Process vs thread:** the OS isolates processes (no access to another process unless it allows it). A process may have several threads, each with its own stack. POSIX threads are one real API — hosts differ (browser tab vs Node). Scheduler, context switch, signals: [Processes, multitasking & interrupts](./0_system_design_core.md#processes-multitasking). Node layer: [Process & threads](./8_nodejs_process.md#process--threads).
- **Process image:** into that space go program **code**, **globals**, then **stack** and **heap**.
- **Stack:** grow/shrink by moving the **stack pointer** (CPU register) — cheap. You can **read** stack data; you **write** only at the top (push). You cannot free a hole in the middle; rewind the pointer to drop everything above it. Exhaustion → **stack overflow**. Teaching diagram: heap grows “up”, stack “down”, they meet in the free gap.
- **Stack frame:** one call’s args, locals, and “what to run after return”. A **frame pointer** marks the active frame. Nested calls: `logDogs` frame → `countDogs(20)` → pop → `countDogs(9)` → pop → pop `logDogs`.
- **Unwind:** `throw` pops frames until a `catch` (or the program dies). The **stack trace** is those frames in reverse call order.
- **Heap:** needed when size is not known at frame-build time (arrays that `.push`, objects, closures) or data must outlive the function. The stack slot stores a **fixed-size reference** (address); the payload lives on the heap. Modify the array via that address. GC later reclaims unreachable heap objects — [Garbage collection](./3_js_objects.md#garbage-collection). MDN: JS allocates on create and frees unused objects; unlike C there is no `malloc`/`free`.
- **JS caveat:** dynamic types mean the engine _guesses_ how much stack to reserve; guesses can be wrong (deopt). “Primitives on stack, objects on heap” is a teaching model, not a guaranteed V8 layout (small ints may be tagged pointers; large strings are heap).

**When to use:**

Why `Maximum call stack size exceeded`; why objects/arrays are shared by reference; why locals vanish after return; reading a stack trace.

|       | Stack                                                   | Heap                                        |
| ----- | ------------------------------------------------------- | ------------------------------------------- |
| **+** | Tiny alloc/free (move a pointer); cache-friendly frames | Arbitrary size; data can outlive the caller |
| **−** | Fixed-ish size; LIFO only; overflow                     | Slower alloc; GC; leaks via leftover refs   |

#### Common pitfalls

Treating the diagram as physical RAM layout; assuming JS always knows stack slot sizes; putting huge or growing data “on the stack” in your head — it is a heap object plus a reference; confusing call-stack overflow with heap OOM (`JavaScript heap out of memory` in Node).

**Examples:**

```js
function countDogs(happyDogs) {
  const sadCoefficient = 0.1;
  if (happyDogs < 10) {
    throw new Error("too few happy dogs");
  }
  return happyDogs + sadCoefficient * happyDogs;
}

function logDogs() {
  console.log(countDogs(20), countDogs(9));
}

logDogs();
// frames: logDogs → countDogs(20) → pop → countDogs(9) → throw → unwind
```

```js
function createDogArray() {
  const dogs = ["🐶", "🐶", "🐶"]; // payload on heap; `dogs` on the frame is a ref
  if (Math.random() > 0.5) dogs.push("🐶"); // size unknown at compile time
}
```

**Thoughts:** _Frames are cheap LIFO; anything with unknown or lasting size is a heap object behind a pointer — GC, not `free()`._

**Sources:** [Doka — how memory works](https://doka.guide/js/how-the-memory-works/) · [MDN Memory management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Memory_management) · [javascript.info/garbage-collection](https://javascript.info/garbage-collection)

</details>

<a name="typeof"></a>

<details>
  <summary>typeof</summary>

### typeof

**Definition:** `typeof` returns a string label for a value’s type. Use it for primitives; pair it with `Array.isArray`, `Number.isNaN`, and strict `===` for reliable checks. Which types are primitive vs object → [Each type in short](#each-type-in-short).

**How it works:**

| Type        | `typeof`      | Notes                                         |
| ----------- | ------------- | --------------------------------------------- |
| `undefined` | `'undefined'` | Unassigned variable                           |
| `null`      | `'object'` ⚠️ | Intentional empty value — legacy `typeof` bug |
| `boolean`   | `'boolean'`   | `true` / `false`                              |
| `number`    | `'number'`    | IEEE-754 double; `NaN`, `±Infinity`           |
| `bigint`    | `'bigint'`    | Arbitrary integers (`123n`)                   |
| `string`    | `'string'`    | Immutable UTF-16 sequence                     |
| `symbol`    | `'symbol'`    | Unique identifier                             |
| `object`    | `'object'`    | Arrays, dates, plain objects, etc.            |
| `function`  | `'function'`  | Callable object — only `typeof` distinguishes |

| Situation             | Use                                                       |
| --------------------- | --------------------------------------------------------- |
| Is it `null`?         | `value === null` (not `typeof`)                           |
| Is it an array?       | `Array.isArray(value)`                                    |
| Is it `NaN`?          | `Number.isNaN(value)`                                     |
| Is it a plain object? | `Object.prototype.toString.call(v) === '[object Object]'` |

- `typeof null` → `'object'` (legacy bug) — use `value === null`.
- `typeof []` → `'object'` — use `Array.isArray(value)`.
- `typeof (() => {})` → `'function'` (only callable objects get this).
- `typeof` on an undeclared binding → `ReferenceError`; on declared but unassigned `let` → `'undefined'`.
- `NaN` is `typeof 'number'`; `NaN === NaN` is false — use `Number.isNaN` or `Object.is(NaN, NaN)`.
- `+0 === -0` is true; `Object.is(+0, -0)` is false.

**When to use:** Quick primitive checks; never rely on `typeof` alone for `null`, arrays, or `NaN`.

**Examples:**

```js
value === null; // null only (not undefined)
value == null; // null OR undefined
Array.isArray(value); // arrays (typeof is 'object')
Number.isNaN(value); // NaN only (typeof NaN is 'number')
Number.isFinite(value); // finite number (excludes NaN, ±Infinity)
Object.prototype.toString.call(v); // '[object Array]', '[object Date]', …

typeof null; // 'object' — use value === null
typeof []; // 'object' — use Array.isArray([])
typeof (() => {}); // 'function'
let x;
typeof x; // 'undefined'

typeof NaN; // 'number'
NaN === NaN; // false
Object.is(NaN, NaN); // true
Number.isNaN(NaN); // true
isNaN("hello"); // true — coerces first (footgun)
Number.isNaN("hello"); // false
+0 === -0; // true
Object.is(+0, -0); // false
```

**Sources:** [javascript.info/types](https://javascript.info/types) · [MDN typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<a name="falsy-values"></a>

<details>
  <summary>Falsy values</summary>

### Falsy values

**Definition:** Eight values coerce to `false` in boolean context (`if`, `&&`, `||`, `Boolean()`). Everything else is **truthy**, including `[]`, `{}`, and `'0'`.

**How it works:**

Falsy: `false`, `0`, `-0`, `0n`, `''`, `null`, `undefined`, `NaN`.

Implicit coercion drives `if (value)` and `value || fallback`; prefer `??` for nullish-only fallback.

**When to use:** Guard clauses and defaults; explicit `Boolean(x)` when the API must return a boolean.

**Examples:**

```js
Boolean([]); // true
Boolean("0"); // true
Boolean(0); // false
Boolean(""); // false
```

Surprising `+` / `==` coercion → [Type conversions](#type-conversions).

**Sources:** [javascript.info/type-conversions](https://javascript.info/type-conversions)

</details>

<a name="object-keys"></a>

<details>
  <summary>Object keys</summary>

### Object keys

**Definition:** Plain object keys (except `Symbol`) stringify via `ToString`. Wrapper objects from `new String()` / `new Number()` are real objects and break `===` with primitives.

**How it works:**

- `{[obj]: 1}` uses `obj.toString()` — often `'[object Object]'`.
- `Symbol` keys are omitted from `Object.keys` / `JSON.stringify` — see [Symbol](#symbol).
- `Map` accepts any key type; plain objects suit JSON DTOs and literal shapes.

| Use `Map`           | Use plain `Object`          |
| ------------------- | --------------------------- |
| Keys of any type    | JSON serialization needed   |
| Frequent add/delete | Literal config / DTO shapes |
| `.size` needed      | Prototype chain / methods   |

**When to use:** `Map` for non-string keys or frequent add/delete; `{}` for serializable records.

**Examples:**

```js
const obj = {};
const key = { id: 1 };
obj[key] = "value";
Object.keys(obj); // ['[object Object]']

const s = new String("hi");
typeof s; // 'object'
s === "hi"; // false — wrapper vs primitive
```

**Sources:** [javascript.info/map-set](https://javascript.info/map-set) · [javascript.info/primitives-methods](https://javascript.info/primitives-methods)

</details>

<a name="type-conversions"></a>

<details>
  <summary>Type conversions</summary>

### Type conversions

**Definition:** Values convert between types **explicitly** (`String()`, `Number()`, `Boolean()`) or **implicitly** during operations (`+`, `==`, logical contexts). Prefer `===` and explicit conversions in application code.

**How it works:**

- **To string:** `String(x)`, template literals, `+` with a string operand.
- **To number:** `Number(x)`, unary `+x`, arithmetic operators (except `+` with string).
- **To boolean:** `Boolean(x)`, `!!x`, or any logical/`if` context — uses falsy list.
- **`==`** coerces both sides before compare; **`===`** does not.
- `null` and `undefined` are only equal to each other with `==`.

| Use                                           | Avoid                                |
| --------------------------------------------- | ------------------------------------ |
| `Number(value)` for user input                | `parseInt` without radix             |
| `value == null` for null/undefined check      | `==` for general equality            |
| `Boolean(x)` or `!!x` when you need a boolean | Relying on implicit coercion in APIs |
| `String(x)` before concatenation              | `x + ''` in critical code paths      |

| Conversion           | Result / rule                               |
| -------------------- | ------------------------------------------- |
| `Number(undefined)`  | `NaN`                                       |
| `Number(null)`       | `0`                                         |
| `Number('')`         | `0`                                         |
| `Number(' 42 ')`     | `42` (trims whitespace)                     |
| `Number(true)`       | `1`                                         |
| `Boolean(0)`         | `false` — see [Falsy values](#falsy-values) |
| `String(123)`        | `'123'`                                     |
| `parseInt('08', 10)` | `8` — always pass radix                     |

**When to use:**

#### Common pitfalls

```js
[] + []; // ''
[] + {}; // '[object Object]'
"5" + 1; // '51' — string concat wins
"5" - 1; // 4 — numeric coercion
isNaN("hello"); // true — coerces first (footgun)
Number.isNaN("hello"); // false — no coercion
```

**Pluses and minuses:**

| Approach                    | +                     | −                             |
| --------------------------- | --------------------- | ----------------------------- |
| `===` + explicit `Number()` | Predictable           | More verbose                  |
| `==`                        | Shorter null checks   | Surprising coercion           |
| `??`                        | Only nullish fallback | Must parenthesize with `\|\|` |

**Examples:**

```js
String(true); // 'true'
Number("123"); // 123
Number(""); // 0
Boolean([]); // true — only 8 values are falsy
Boolean("0"); // true
null == undefined; // true
null === undefined; // false
"" == 0; // true
"" === 0; // false
```

**Sources:** [javascript.info/type-conversions](https://javascript.info/type-conversions) · [javascript.info/comparison](https://javascript.info/comparison)

</details>

<a name="template-literals"></a>

<details>
  <summary>Template literals</summary>

### Template literals

**Definition:** **Template literals** use backticks for string interpolation (`${expr}`) and multiline text without `\n` escapes.

**How it works:**

- `` `Hello ${name}` `` embeds expressions; nested templates allowed.
- Tagged templates (`fn\`...\``) pass segments and values to a function — advanced (see MDN).
- No automatic HTML escaping — sanitize user input before embedding in markup.

**When to use:** Dynamic messages, SQL fragments (with parameterization), multiline config snippets.

**Examples:**

```js
const name = "Ann";
const greeting = `Hello, ${name}!`;
const multiline = `line 1
line 2`;
```

**Sources:** [javascript.info/string](https://javascript.info/string) · [MDN Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

</details>

## Advanced

### Advanced

<a name="object-is-and-samevaluezero"></a>

<details>
  <summary>Object.is & SameValueZero</summary>

### Object.is & SameValueZero

**Definition:** JS has several **equality** relations. Application code mostly uses `===`; engines also use **SameValue** (`Object.is`) and **SameValueZero** (`Map`/`Set` keys, `Array.includes`).

**How it works:**

| Relation      | `NaN`     | `+0` / `-0`   | Coercion |
| ------------- | --------- | ------------- | -------- |
| `==`          | not equal | equal         | yes      |
| `===`         | not equal | equal         | no       |
| `Object.is`   | equal     | **not** equal | no       |
| SameValueZero | equal     | equal         | no       |

- `Object.is(a, b)` — SameValue: like `===` except `Object.is(NaN, NaN)` is true and `Object.is(+0, -0)` is false.
- **SameValueZero** — SameValue but treats `+0` and `-0` as equal; used internally for `Map`/`Set` and `includes`.
- Prefer `Number.isNaN(x)` for a single-value NaN test (no coercion). Full comparison guide → [Object.is & equality](./3_js_operators.md#object-is-and-equality).

**When to use:** `Object.is` when you care about signed zero or NaN sameness; never use `==` for general equality.

**Examples:**

```js
Object.is(NaN, NaN); // true
NaN === NaN; // false
Object.is(+0, -0); // false
+0 === -0; // true

const m = new Map([[NaN, "ok"]]);
m.get(NaN); // 'ok' — SameValueZero
[NaN].includes(NaN); // true
```

**Sources:** [MDN Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) · [MDN Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness)

</details>

<a name="toprimitive-and-abstract-conversions"></a>

<details>
  <summary>ToPrimitive & abstract conversions</summary>

### ToPrimitive & abstract conversions

**Definition:** Implicit coercion in `+`, `==`, and `if` is not magic — the spec runs **abstract operations** such as `ToPrimitive`, `ToNumber`, and `ToString` on operands. Objects participate through `Symbol.toPrimitive` or legacy `valueOf` / `toString`.

**How it works:**

- **`ToPrimitive(input, hint)`** — if the value is an object, call `[Symbol.toPrimitive](hint)` when present; else `valueOf` / `toString` order depends on `hint` (`'number'`, `'string'`, `'default'`).
- **`ToNumber`** / **`ToString`** — used by arithmetic and concatenation after primitives are obtained.
- **`ToBoolean`** — drives truthiness ([Falsy values](#falsy-values)); only eight values are false.
- `hint === 'default'` is what `==` and `+` often use — source of `{} + 1` surprises.

| Hint      | Try first  | Then       |
| --------- | ---------- | ---------- |
| `number`  | `valueOf`  | `toString` |
| `string`  | `toString` | `valueOf`  |
| `default` | `valueOf`  | `toString` |

User-facing rules → [Type conversions](#type-conversions). Object hooks in depth → [Objects to primitives](./3_js_objects.md#objects-to-primitives).

**Examples:**

```js
const obj = {
  [Symbol.toPrimitive](hint) {
    return hint === "number" ? 42 : "hi";
  },
};
obj + 1; // 43
String(obj); // 'hi'

{
}
+1; // often '[object Object]1' in sloppy script contexts
```

**Sources:** [javascript.info/type-conversions](https://javascript.info/type-conversions) · [MDN Symbol.toPrimitive](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)

</details>

<a name="primitive-wrappers"></a>

<details>
  <summary>Primitive wrappers</summary>

### Primitive wrappers

**Definition:** Primitives are not objects, but the language provides **wrapper objects** (`String`, `Number`, `Boolean`, `Symbol`, `BigInt`) for rare cases. Calling a method on a primitive **boxes** it temporarily; `new String()` creates a real object that breaks `===`.

**How it works:**

- `'hi'.toUpperCase()` — engine boxes `'hi'` in a temporary `String` object, calls the method, discards the wrapper.
- `new String('hi')` — persistent object; `typeof` is `'object'`; `===` with the primitive is false.
- **Never** use `new String()` / `new Number()` / `new Boolean()` in application code.
- `Symbol` and `BigInt` have no `new` without `new` syntax quirks — use `Symbol()` / `123n` only.

Tied to key stringification → [Object keys](#object-keys). Overview → [Each type in short](#each-type-in-short).

**Examples:**

```js
typeof "x"; // 'string'
typeof new String("x"); // 'object'
new String("x") === "x"; // false

(123).toFixed(2); // boxing on literal via (123)
```

**Sources:** [javascript.info/primitives-methods](https://javascript.info/primitives-methods) · [MDN String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

</details>

<a name="json-vs-structuredclone"></a>

<details>
  <summary>JSON vs structuredClone</summary>

### JSON vs structuredClone

**Definition:** Copying and sending data crosses **type** boundaries. `JSON.stringify` / `JSON.parse` only support JSON-safe data; `structuredClone` uses the structured clone algorithm and preserves more built-in types (at the cost of stricter rules).

**How it works:**

| Approach          | Cycles | `undefined` / `function` | `Date`, `Map`, `Set`      |
| ----------------- | ------ | ------------------------ | ------------------------- |
| `JSON.stringify`  | throws | omitted / dropped        | `Date` → string; no `Map` |
| `structuredClone` | OK     | supported in clone       | cloned when supported     |

- JSON: keys are strings; `undefined` values in objects are omitted; `NaN`/`Infinity` become `null` in JSON.
- `structuredClone` cannot clone DOM nodes or some host objects; functions are not cloneable.
- Shallow copy of plain records: spread / `Object.assign` — see [Shallow vs deep copy](./3_js_objects.md#shallow-vs-deep-copy).

**When to use:** APIs and `localStorage` → JSON; in-memory snapshots of app state with `Date`/`Map` → `structuredClone`; deep plain POJOs → JSON round-trip or clone.

**Examples:**

```js
JSON.stringify({ a: 1, b: undefined }); // '{"a":1}'
JSON.parse(JSON.stringify({ d: new Date() })); // date becomes string

const src = { m: new Map([[1, 2]]) };
structuredClone(src).m.get(1); // 2
```

**Sources:** [MDN structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone) · [MDN JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)

</details>
