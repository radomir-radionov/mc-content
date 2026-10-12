# JS Core — Operators

Expressions, operators, comparisons, optional chaining, and `?.` / `??`. Control flow and iterators live in [Loops](./3_js_loops.md).

[← Back to JS hub](./3_js.md)

### Related docs

- [JS Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Loops](./3_js_loops.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [JS Arrays](./3_js_array.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Document](./3_js_browser.md) · [JS Events](./3_js_events.md) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [Computer science](./7_special_cs.md)

### Main concepts

| Area            | Topics                                                                                 |
| --------------- | -------------------------------------------------------------------------------------- |
| **Expressions** | Statement vs expression, ASI, expression kinds                                         |
| **Operators**   | Arithmetic, comparison, `Object.is`, logical, bitwise, optional chaining, nullish `??` |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Expressions](#expressions)
- [Operators](#operators)
- [Interview](./3_js_interview.md)

Sibling Core topics: [Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Loops](./3_js_loops.md) · [Array](./3_js_array.md) · [Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md).

<a name="expressions"></a>

## Expressions

<details>
  <summary>Statement vs expression</summary>

### Statement vs expression

**Definition:**

A **statement** performs an action (assign, loop, declare). An **expression** evaluates to a value (`42`, `a + b`, `fn()`).

|               | Statement                  | Expression                                     |
| ------------- | -------------------------- | ---------------------------------------------- |
| Returns value | No                         | Yes                                            |
| Examples      | `if`, `for`, `const x = 1` | `1 + 2`, ternary, arrow fn body without braces |

**When to use:** Prefer expressions in functional pipelines; statements for control flow and side effects at boundaries.

**Examples:**

```js
const doubled = items.map((x) => x * 2); // expression-bodied callback
if (items.length === 0) return []; // statement
```

**Thoughts:**

`const x = (y = 1)` — assignment inside expression is valid but hurts readability.

---

</details>

<details>
  <summary>ASI (automatic semicolon insertion)</summary>

### ASI (automatic semicolon insertion)

**Definition:** When a line break separates statements, the engine often inserts an implicit `;`. It does **not** always do so — some line starts continue the previous expression.

**How it works:**

- Most newlines act like a statement end: `alert('Hello')\nalert('World')` is fine.
- A newline after an incomplete expression does **not** insert `;` (e.g. ending with `+`).
- A newline before `[...]` does **not** insert `;` — the next line can be parsed as property access on the previous expression.
- Same class of footgun: bare `return` then a newline returns `undefined` (ASI inserts `;` after `return`).

**When to use:** Put an explicit `;` after every statement (community default). Extra `;` after blocks/`for` is harmless; missing ones before `[`, `(`, `` ` ``, `/` are the dangerous cases.

**Examples:**

```js
alert("Hello");
[1, 2].forEach(alert); // Hello, then 1, then 2

alert("Hello")[(1, 2)].forEach(alert); // treated as alert("Hello")[1, 2].forEach(alert) — broken
```

**Thoughts:** ASI is why style guides prefer semicolons — the bugs are rare but hard to spot.

**Sources:** [javascript.info/structure](https://javascript.info/structure) · [MDN ASI](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#automatic_semicolon_insertion)

</details>

<details>
  <summary>Expressions</summary>

### Expressions

**Definition:** An **expression** evaluates to a value; a **statement** performs an action and does not produce a usable value on its own (though it may contain expressions).

**How it works:**

- Expression kinds: literals, identifiers, operators (`a + b`), calls (`fn()`), `new`, `await`, arrows, ternary, template literals, object/array literals.
- **Assignment** is an expression: `x = y = 1` chains because `=` returns the assigned value.
- **Statement** examples: `if`, `for`, `return`, `const x = 1` (declaration statement; initializer is an expression).
- Distinguish from [Statement vs expression](#statement-vs-expression) for ASI and control flow.

| Expression type | Example                   |
| --------------- | ------------------------- |
| Arithmetic      | `3 + 4` → `7`             |
| String          | `'Hello ' + 'World'`      |
| Logical         | `true && false` → `false` |
| Assignment      | `x = 10` → `10`           |

**When to use:** Expression-bodied arrows and `map` callbacks; statements at boundaries (I/O, mutations).

**Examples:**

```js
const ok = items.length > 0 && items[0].id; // expression
if (!ok) return null; // statement

const label = user ? user.name : "Guest"; // ternary expression
```

**Thoughts:** Every expression produces a value (except thrown errors). Prefer clarity over clever nested ternaries.

**Sources:** [MDN — Expressions and operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators)

</details>

<a name="operators"></a>

## Operators

<details>
  <summary>Operators</summary>

### Operators

**Definition:** **Operators** combine or transform operands — unary (`!x`), binary (`a + b`), or ternary (`cond ? a : b`). Precedence and associativity determine evaluation order without extra parentheses.

**How it works:**

- **Arithmetic / string:** `+` concatenates if either side is a string; other math ops coerce to number.
- **Comparison:** prefer `===` / `!==`; `==` applies coercion rules ([Data Types — coercion](./3_js_data_types.md)).
- **Logical:** `&&` / `||` short-circuit; `??` only replaces `null`/`undefined`.
- **Optional chaining:** `?.` / `??` — see dedicated concepts below in this topic.
- Deep dives: [Math operators](#math-operators) · [Comparisons](#comparisons) · [Logical operators](#logical-operators) · [Bitwise](#bitwise-operators) · [Optional chaining](#optional-chaining).

| Type       | Operators                              |
| ---------- | -------------------------------------- |
| Arithmetic | `+`, `-`, `*`, `/`, `%`, `**`          |
| Assignment | `=`, `+=`, `-=`, `??=`                 |
| Comparison | `==`, `===`, `>`, `<`, `!==`, `!=`     |
| Logical    | `&&`, `\|\|`, `!`, `??`                |
| Bitwise    | `&`, `\|`, `^`, `~`, `<<`, `>>`, `>>>` |
| Ternary    | `cond ? a : b`                         |

**When to use:** Use the narrowest operator (`??` vs `||`); parenthesize when precedence is unclear.

**Examples:**

```js
const n = value ?? 0;
const name = user?.profile?.name;
const active = Boolean(flags & 1);
```

**Sources:** [MDN Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)

</details>

<details>
  <summary>Math operators</summary>

### Math operators

**Definition:** Arithmetic and unary operators on numbers; `+` also concatenates strings when either operand is a string.

**How it works:**

- `+` with string → string concat; otherwise numeric add (with coercion).
- `-`, `*`, `/`, `%`, `**` convert operands to numbers.
- Unary `+x` coerces to number; unary `-x` negates.
- `++`/`--` prefix returns new value; postfix returns old value then updates.
- `%` is remainder, not mathematical modulo for negatives.
- Assignment `=` is an operator: it writes then **returns** the assigned value — so `a = b = c = 2` chains right-to-left. Prefer separate lines for readability.
- Comma `,` evaluates left-to-right and returns the **last** value (`(1 + 2, 3 + 4)` → `7`). Rare outside `for` init; know it when reading library code.

**When to use:** Use `Number(x)` or `+x` for explicit coerce; always pass radix to `parseInt(s, 10)`.

**Pluses and minuses:**

| +                       | −                                  |
| ----------------------- | ---------------------------------- |
| `**` for exponentiation | `+` dual role causes bugs          |
| Unary `+` quick coerce  | Floating-point rounding (IEEE-754) |

**Examples:**

```js
"5" + 1; // '51' (string concat)
"5" - 1; // 4 (numeric coercion)
+"42"; // 42
parseInt("08", 10); // 8
Number.isFinite(42); // true
1n + 2n; // 3n — bigint, cannot mix with number without conversion

let a, b, c;
a = b = c = 2 + 2; // all 4
let x = (1 + 2, 3 + 4); // 7 — comma returns last
```

**Sources:** [javascript.info/operators](https://javascript.info/operators)

</details>

<details>
  <summary>Comparisons</summary>

### Comparison operators

**Definition:** Comparison operators return booleans (or drive `sort` comparators). `===` avoids coercion; `==` coerces first.

**How it works:**

- `===` / `!==` — no type conversion; different types are always unequal.
- `==` / `!=` — apply abstract equality rules (surprising with `''`, `0`, `false`).
- Relational `>`, `<`, `>=`, `<=` — usually convert to numbers; strings compare lexicographically when both are strings.
- `Object.is` for edge cases (`NaN`, signed zero) — see [Object.is & equality](#object-is-and-equality).
- **`null` / `undefined` quirks:** `null == undefined` is `true`, but neither equals any other value with `==`. Relational ops convert `null` → `0` and `undefined` → `NaN`, so `null >= 0` is `true` while `null > 0` and `null == 0` are both `false`.

**When to use:** Default `===`; `value == null` for null/undefined; numeric sort comparators return negative/zero/positive. Do **not** use `>`, `<`, `>=`, `<=` on a value that may be `null`/`undefined` — check those separately.

**Pluses and minuses:**

| Approach    | +                   | −                    |
| ----------- | ------------------- | -------------------- |
| `===`       | Predictable         | Verbose null checks  |
| `==`        | `null == undefined` | Footguns (`'' == 0`) |
| `Object.is` | Correct `NaN`       | Niche                |

**Examples:**

```js
null === undefined; // false
null == undefined; // true
"2" > "12"; // true — string compare
"2" > 12; // false — number compare

null > 0; // false
null == 0; // false — == has a special null/undefined rule
null >= 0; // true  — >= converts null → 0
undefined > 0; // false (NaN)
undefined == 0; // false
```

**Sources:** [javascript.info/comparison](https://javascript.info/comparison)

</details>

<details>
  <summary>Object.is & equality</summary>

### Object.is & equality

**Definition:** Multiple equality mechanisms: `==` (coercing), `===` (strict), `Object.is` (SameValue), and `SameValueZero` (used by `Map`/`Set`).

**How it works:**

- `===` — same type and value; `NaN !== NaN`, `+0 === -0`.
- `Object.is` — like `===` but `Object.is(NaN, NaN)` is `true`, `Object.is(+0, -0)` is `false`.
- `SameValueZero` — `Map`/`Set` key lookup; `NaN` equals `NaN`.
- `==` — coerces operands (avoid except `value == null`).

| Comparison  | `NaN === NaN` | `+0 === -0` | Coercion |
| ----------- | ------------- | ----------- | -------- |
| `===`       | false         | true        | no       |
| `Object.is` | true          | false       | no       |
| `==`        | false         | true        | yes      |

**When to use:** `Object.is` for `NaN` checks; `===` everywhere else; `Number.isNaN(x)` for single-value `NaN` test without coercion.

**Pluses and minuses:**

| + (`Object.is`)         | −                                       |
| ----------------------- | --------------------------------------- |
| Correct `NaN` checks    | Rarely needed vs `===`                  |
| Distinguishes `+0`/`-0` | Not used by `Map`/`Set` (SameValueZero) |

**Examples:**

```js
NaN === NaN; // false
Object.is(NaN, NaN); // true
Object.is(+0, -0); // false
+0 === -0; // true

const m = new Map();
m.set(NaN, "val");
m.get(NaN); // 'val' — SameValueZero
```

**Sources:** [MDN Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

</details>

<details>
  <summary>Logical operators</summary>

### Logical operators

**Definition:** `&&`, `||`, and `!` use **short-circuit** evaluation — they return the deciding operand, not always a boolean.

**How it works:**

- `a || b` — if `a` is truthy, return `a`; else `b`.
- `a && b` — if `a` is falsy, return `a`; else `b`.
- `!a` — boolean negation after coercion.
- Common pattern: `user && greet(user)`; `name || 'Guest'`.
- Nullish defaults (`??`) → [Nullish coalescing operator](#nullish-coalescing-operator).

| Operator | Returns                             |
| -------- | ----------------------------------- |
| `\|\|`   | First truthy (or last if all falsy) |
| `&&`     | First falsy (or last if all truthy) |
| `!`      | Boolean                             |

**When to use:** `||` when any falsy should fallback; `&&` for conditional execution.

**Pluses and minuses:**

| +                               | −                                   |
| ------------------------------- | ----------------------------------- |
| Concise defaults and guards     | `\|\|` treats `0` / `''` as missing |
| Short-circuit avoids extra work | Easy to nest without explicit `if`  |

**Examples:**

```js
0 || "default"; // 'default'
name && greet(name);
user || createGuest();
```

**Sources:** [javascript.info/logical-operators](https://javascript.info/logical-operators)

</details>

<details>
  <summary>Bitwise operators</summary>

### Bitwise operators (basics)

**Definition:** Operators that treat numbers as 32-bit integers: `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`.

**How it works:**

- Operands convert to 32-bit signed integers before the operation.
- `&` AND, `|` OR, `^` XOR, `~` NOT, shifts move bits left/right.
- `>>>` unsigned right shift fills with zero.

**When to use:** Permission flags, hash tricks, performance-critical libs. Most app code never needs bitwise ops.

**Pluses and minuses:**

| +                    | −                           |
| -------------------- | --------------------------- |
| Fast flags and masks | Rare in typical app code    |
| Low-level bit tricks | 32-bit limit on bitwise ops |

**Examples:**

```js
5 & 1; // 1 — odd check
1 << 3; // 8 — multiply by 8 via shift
```

**Sources:** [javascript.info/bitwise-operators](https://javascript.info/bitwise-operators)

</details>

<details>
  <summary>Optional chaining</summary>

### Optional chaining (`?.`)

**Definition:** `?.` short-circuits property access, indexing, and calls when the value before `?.` is `null` or `undefined`, instead of throwing.

**How it works:**

- `obj?.prop`, `obj?.[expr]`, `fn?.()` — stops at the first nullish step.
- Short-circuits: `user?.sayHi(x++)` does **not** evaluate `x++` if `user` is nullish.
- Variable before `?.` must be **declared** — undeclared `user?.x` is still a `ReferenceError`.
- **Read/delete only** — `user?.name = "John"` is invalid.
- Pair with defaults: `user?.profile?.name ?? "Anonymous"` — chaining → [Nullish coalescing operator](#nullish-coalescing-operator).

**When to use:** Optional API fields and nested config; avoid masking a missing required root object.

**Pluses and minuses:**

| +                                    | −                                       |
| ------------------------------------ | --------------------------------------- |
| Safe deep access without nested `if` | Can hide data bugs (silent `undefined`) |

**Examples:**

```js
const user = null;
user?.profile?.name; // undefined — no throw
user?.admin?.(); // call only if method exists
user?.[key];
```

**Sources:** [javascript.info/optional-chaining](https://javascript.info/optional-chaining)

</details>

<details>
  <summary>Nullish coalescing operator</summary>

### Nullish coalescing operator (`??`)

**Definition:** `??` returns the right-hand operand only when the left is `null` or `undefined` — not for other falsy values (`0`, `''`, `false`).

**How it works:**

- `a ?? b` — if `a` is nullish, result is `b`; otherwise `a`.
- Differs from `||`: `0 ?? 1` is `0`; `0 || 1` is `1`.
- Combine with care: `a ?? b || c` and `a || b ?? c` need parentheses (language rules forbid mixing without grouping).
- Works with optional chaining: `config.port ?? 3000`, `response?.data ?? []`.

**When to use:** Config defaults when `0` or `''` are valid values; prefer `??` over `||` for “missing only” semantics.

**Pluses and minuses:**

| +                         | −                                          |
| ------------------------- | ------------------------------------------ |
| Preserves `0` and `''`    | Mixing `??` and `\|\|` needs parens        |
| Clear “nullish” semantics | Easy to overuse instead of explicit checks |

**Examples:**

```js
0 || "default"; // 'default'
0 ?? "default"; // 0

const port = config.port ?? 3000;
const title = settings.title ?? "Untitled";
```

**Sources:** [javascript.info/nullish-coalescing-operator](https://javascript.info/nullish-coalescing-operator)

</details>
