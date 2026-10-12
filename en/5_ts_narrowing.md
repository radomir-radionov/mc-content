# TypeScript — Narrowing types

Control-flow analysis and type guards — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/narrowing.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Types](./5_ts_types.md) · [Functions](./5_ts_functions.md) · [Object Types](./5_ts_objects.md) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                   | Covers                                             |
| ------------------------------------------------------- | -------------------------------------------------- |
| [Type narrowing](#type-narrowing)                       | Overview — unions, type guards, control-flow paths |
| [typeof type guards](#typeof-type-guards)               | `typeof x === "string"` and primitive unions       |
| [Truthiness narrowing](#truthiness-narrowing)           | `if (x)`, `!x`, falsy values                       |
| [Equality narrowing](#equality-narrowing)               | `===`, `== null`, comparing two values             |
| [The in operator narrowing](#the-in-operator-narrowing) | `"key" in obj` on object unions                    |
| [instanceof narrowing](#instanceof-narrowing)           | `x instanceof Date`                                |
| [Assignments](#assignments)                             | Observed vs declared type after `=`                |
| [Control flow analysis](#control-flow-analysis)         | `return`, branches, reachability                   |
| [Using type predicates](#using-type-predicates)         | `pet is Fish` user-defined guards                  |
| [Assertion functions](#assertion-functions)             | `asserts val is Type`                              |
| [Discriminated unions](#discriminated-unions)           | Literal `kind` / `type` fields                     |
| [The never type](#the-never-type)                       | Bottom type — no valid values                      |
| [Exhaustiveness checking](#exhaustiveness-checking)     | `default: never` in `switch`                       |

### Sections

- [Type narrowing](#type-narrowing)
- [typeof type guards](#typeof-type-guards)
- [Truthiness narrowing](#truthiness-narrowing)
- [Equality narrowing](#equality-narrowing)
- [The in operator narrowing](#the-in-operator-narrowing)
- [instanceof narrowing](#instanceof-narrowing)
- [Assignments](#assignments)
- [Control flow analysis](#control-flow-analysis)
- [Using type predicates](#using-type-predicates)
- [Assertion functions](#assertion-functions)
- [Discriminated unions](#discriminated-unions)
- [The never type](#the-never-type)
- [Exhaustiveness checking](#exhaustiveness-checking)

---

## Type narrowing

<details>
<summary>Type narrowing</summary>

**Definition:**

**Narrowing** is how TypeScript refines a value from a wider type (usually a union) to a more specific type inside a branch of code. You write ordinary JavaScript checks; the compiler tracks which types are still possible on each path. A check the compiler understands is called a **type guard**.

**How it works:**

| Technique                                               | Summary                                                           | Example                                    |
| ------------------------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------ |
| [typeof type guards](#typeof-type-guards)               | Narrow primitive unions with runtime `typeof`                     | `if (typeof x === "string")`               |
| [Truthiness narrowing](#truthiness-narrowing)           | Drop falsy values (`null`, `undefined`, `""`, `0`, …) in `if (x)` | `if (value) { … }`                         |
| [Equality narrowing](#equality-narrowing)               | Compare to literals or another value with `===` / `== null`       | `if (x === "ok")` · `if (v != null)`       |
| [The in operator narrowing](#the-in-operator-narrowing) | Pick an object-union member by property name                      | `if ("swim" in pet)`                       |
| [instanceof narrowing](#instanceof-narrowing)           | Narrow class instances via the prototype chain                    | `if (x instanceof Date)`                   |
| [Assignments](#assignments)                             | Observed type follows the last assignment on `let`                | `data = 100` then `data.toFixed()`         |
| [Control flow analysis](#control-flow-analysis)         | `return` / `throw` / branches refine types per path               | `if (…) return;` — rest is narrowed        |
| [Using type predicates](#using-type-predicates)         | Custom guard: `param is Type`                                     | `function isFish(p): p is Fish`            |
| [Assertion functions](#assertion-functions)             | Throw if check fails; caller scope narrows                        | `asserts val is number`                    |
| [Discriminated unions](#discriminated-unions)           | Shared literal field (`kind`) selects the variant                 | `switch (shape.kind) { case "circle": … }` |
| [The never type](#the-never-type)                       | No values left after all variants handled                         | `const _: never = value`                   |
| [Exhaustiveness checking](#exhaustiveness-checking)     | `default: never` errors when a union grows                        | `default: { const _: never = shape }`      |

Walk through one function — `padLeft(padding: number | string, …)`:

1. **Start wide.** `padding` is typed as `number | string`. TypeScript must assume it could be either until you prove otherwise.
2. **Before any check**, you may only use APIs that work on **both** types. `" ".repeat(padding)` is an error here: `repeat` needs a `number`, but `padding` might still be a `string`.
3. **After a check**, TypeScript remembers the result **for that branch only**:
   - In `if (typeof padding === "number") { … }`, `padding` is treated as `number` inside the `{ … }` block.
   - After the `if` (or in the `else`), it is treated as `string` — the other possibility.
4. **Early exit** works the same way: if you `return` or `throw` when `padding` is a `number`, the code below no longer has to consider `number` — only what is left (here, `string`). You do not need an `else` for that.
5. **Two names for the same variable:**
   - **Declared type** — what you wrote in the annotation (`number | string`). You still cannot assign `true` to `padding` later.
   - **Observed type** — what TypeScript thinks `padding` is **on this line** after checks and assignments. It can be narrower (`number` or `string`) until you assign again.

```typescript
// Steps 1–2: padding is `number | string` — too wide for `.repeat()`
function padLeft(padding: number | string, input: string): string {
  // return " ".repeat(padding) + input; // error: repeat expects number

  // Step 3: each branch gets a narrower observed type
  if (typeof padding === "number") {
    return " ".repeat(padding) + input; // padding: number
  }
  return padding + input; // padding: string
}

// Step 4: early return — no `else` needed; code below is only `string`
function padLeftEarly(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input; // padding: string
}

// Step 5: declared type vs observed type on `let`
function declaredVsObserved() {
  let padding: number | string = 4;
  padding.toFixed(0); // observed: number

  padding = "  ";
  padding.trim(); // observed: string

  // padding = true; // error — declared type is still number | string
}
```

Narrowing happens only in the type checker — your JavaScript runs unchanged. Unlike `as`, you do not tell the compiler to trust you; it infers from the checks you already wrote.

**When to use:**

- A parameter or variable is a **union** and you need a method or property that exists on only one member.
- A value may be **`null` / `undefined`** and you must prove it is present before use.
- You handle **discriminated variants** (`kind`, `type`) and need fields that exist on only one variant.
- Skip narrowing when the type is already a single concrete type, or when every member of the union shares the property you need (e.g. `.slice()` on `string | string[]`).

**Pluses and minuses:**

| +                                                                 | −                                                                             |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Stays close to normal JavaScript — no extra runtime library       | Only works where the compiler can follow control flow                         |
| Catches misuse of union members at compile time                   | Truthiness checks treat `0`, `""`, `NaN` as false — easy to miss valid values |
| Refactors stay safe when unions grow (with `never` checks)        | Closures and reassignment can widen types again after a guard                 |
| No false confidence — unlike `as`, narrowing reflects real checks | `typeof null === "object"` — arrays and `null` both narrow to `object`        |

**Examples:**

```typescript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}

function printId(id: number | string) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

**Thoughts:**

Interview framing: narrowing is **control-flow analysis**, not a cast. Contrast with `as`, which does not run any check.

**Sources:** [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

---

## typeof type guards

<details>
<summary>typeof type guards</summary>

**Definition:** Checking `typeof value === "..."` is a type guard because TypeScript encodes JavaScript's `typeof` results.

**How it works:**

- Recognized strings: `"string"`, `"number"`, `"bigint"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, `"function"`.
- `typeof null` is `"object"` — a `typeof x === "object"` branch still includes `null`.
- Arrays are objects; `typeof arr === "object"` does not prove `T[]` by itself.

**When to use:** Primitive unions (`string | number`, `string | undefined`). Do not use `typeof` to filter `null` — use `=== null`.

**Examples:**

```typescript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}

function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    // still `string[] | null` — arrays and null are both objects
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```

**Sources:** [TypeScript handbook — typeof type guards](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#typeof-type-guards)

</details>

---

## Truthiness narrowing

<details>
<summary>Truthiness narrowing</summary>

**Definition:** Conditions that coerce to boolean (`if (value)`, `!value`, `&&` / `||`) narrow away falsy variants.

**How it works:**

- Falsy: `false`, `0`, `0n`, `""`, `null`, `undefined`, `NaN`. Everything else is truthy.
- `if (x)` drops `null`/`undefined` (and other falsy primitives) from the true branch.
- `Boolean(x)` infers `boolean`; `!!x` infers the literal `true` when `x` is always truthy.

**When to use:** Guarding against `null`/`undefined` on objects and arrays. Skip when `0` or `""` are valid values — those are also falsy.

**Examples:**

```typescript
function printLength(str: string | null | undefined) {
  if (str) {
    console.log(`Length: ${str.length}`);
  } else {
    console.log("No valid string provided");
  }
}

function checkCount(count: number | undefined) {
  if (count) {
    // skipped when count is 0
    console.log(`Count is ${count}`);
  }
}
```

**Sources:** [TypeScript handbook — Truthiness narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#truthiness-narrowing)

</details>

---

## Equality narrowing

<details>
<summary>Equality narrowing</summary>

**Definition:** `===`, `!==`, `==`, `!=`, and `switch` narrow when TypeScript can prove which union members remain.

**How it works:**

- Strict equality (`===` / `!==`) compares type and value.
- Loose equality: `== null` / `!= null` is the usual shortcut for both `null` and `undefined`.
- Comparing two unions keeps only the overlapping type in the true branch.

**When to use:** Literal tags, `null`/`undefined` checks, and matching two variables that share a type. Prefer `===` except for the `== null` idiom.

**Examples:**

```typescript
function processInput(x: string | number, y: string | boolean) {
  if (x === y) {
    console.log(x.toUpperCase());
    console.log(y.toUpperCase());
  }
}

function printValue(value: string | null | undefined) {
  if (value != null) {
    console.log(value.trim());
  }
}
```

**Sources:** [TypeScript handbook — Equality narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#equality-narrowing)

</details>

---

## The in operator narrowing

<details>
<summary>The in operator narrowing</summary>

**Definition:** `"key" in obj` narrows an object union to members that declare that property.

**How it works:**

- True branch: types that have the key (required or optional).
- False branch: types that do **not** have the key as a required property.
- If the key is optional (`?`) on one variant, that variant can remain in the `else` branch.

**When to use:** Structurally different objects (`Fish | Bird`) when you do not share a discriminant field. Prefer a literal `kind` field when you control both types.

**Examples:**

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) animal.swim();
  else animal.fly();
}
```

**Sources:** [TypeScript handbook — The in operator narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-in-operator-narrowing)

</details>

---

## instanceof narrowing

<details>
<summary>instanceof narrowing</summary>

**Definition:** `value instanceof Ctor` narrows to that constructor's instance type using the prototype chain at runtime.

**How it works:**

- Works with classes and constructor functions (`Date`, `Error`, custom classes).
- Interfaces and type aliases are erased — `instanceof User` is a compile error if `User` is only a type.
- `Symbol.hasInstance` with a type predicate can customize what `instanceof` narrows to.

**When to use:** Values you constructed with `new`, or built-ins with constructors. Use `in` / predicates / discriminants for plain objects.

**Examples:**

```typescript
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
  } else {
    console.log(x.toUpperCase());
  }
}
```

**Sources:** [TypeScript handbook — instanceof narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#instanceof-narrowing)

</details>

---

## Assignments

<details>
<summary>Assignments</summary>

**Definition:** Assigning a more specific value narrows a variable's _observed_ type until the next assignment.

**How it works:**

- _Declared type_ is the annotation (what you may assign later).
- _Observed type_ is the narrowed type after the latest assignment.
- You still cannot assign a value outside the declared type.

**When to use:** `let` variables that start as a union and get a concrete value. `const` is already the assigned literal/type.

**Examples:**

```typescript
let data: string | number = Math.random() < 0.5 ? 42 : "hello";

data = 100;
console.log(data.toFixed(2));

data = "goodbye";
console.log(data.toUpperCase());
```

**Sources:** [TypeScript handbook — Assignments](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#assignments)

</details>

---

## Control flow analysis

<details>
<summary>Control flow analysis</summary>

**Definition:** TypeScript tracks how `if`, `return`, `throw`, `break`, and loops change types on each path, then re-merges them when paths join.

**How it works:**

- An early `return`/`throw` removes that variant from the rest of the function — no `else` required.
- Assignments inside branches split the type; after the `if`/`else`, remaining possibilities are unioned.
- Reachability matters: code after a never-returning call is unreachable.

**When to use:** Guard clauses and multi-branch functions. Do not assume a type stays narrowed after a callback that might assign later.

**Examples:**

```typescript
function process(input: string | number) {
  if (typeof input === "number") {
    return input * 2;
  }
  return input.toUpperCase();
}

function getStatus() {
  let result: string | number | boolean = true;

  if (Math.random() < 0.5) {
    result = "Success";
  } else {
    result = 500;
  }

  return result; // string | number
}
```

**Sources:** [TypeScript handbook — Control flow analysis](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#control-flow-analysis)

</details>

---

## Using type predicates

<details>
<summary>Using type predicates</summary>

**Definition:** A user-defined type guard is a function whose return type is `parameterName is Type`.

**How it works:**

- On `true`, the argument is narrowed to that type at the call site.
- On `false`, that type is excluded from the remaining union.
- The body is _not_ verified — a wrong `return true` will lie to the type checker.

**When to use:** When built-in guards are not enough (payload validation, `Array.isArray`-style helpers). Keep the runtime check honest.

**Examples:**

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) pet.swim();
  else pet.fly();
}

const zoo: (Fish | Bird)[] = [getSmallPet(), getSmallPet()];
const underWater: Fish[] = zoo.filter(isFish);
```

**Sources:** [TypeScript handbook — Using type predicates](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates)

</details>

---

## Assertion functions

<details>
<summary>Assertion functions</summary>

**Definition:** A function with return type `asserts condition` or `asserts val is Type` narrows by throwing if the check fails.

**How it works:**

- `asserts condition` — if the function returns, `condition` is `true` for the rest of the scope.
- `asserts val is Type` — after return, `val` has that type.
- Works with generics, e.g. `asserts val is NonNullable<T>`.

**When to use:** Preconditions at function entry when failure should abort. Prefer `if` + predicates when both branches need to continue.

**Examples:**

```typescript
function assertIsNumber(val: unknown): asserts val is number {
  if (typeof val !== "number") {
    throw new Error("Not a number!");
  }
}

function assertNonNull<T>(val: T): asserts val is NonNullable<T> {
  if (val == null) {
    throw new Error("Value cannot be null or undefined");
  }
}
```

**Sources:** [TypeScript handbook — Assertion functions](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#assertion-functions)

</details>

---

## Discriminated unions

<details>
<summary>Discriminated unions</summary>

**Definition:** A union of object types that share a literal discriminant field (`kind`, `type`) so checks on that field narrow the whole object.

**How it works:**

- Each variant has the same key with a different string/number/boolean literal.
- `if (shape.kind === "circle")` makes variant-specific fields (`radius`) available.
- Optional fields on one shared interface do **not** discriminate — TypeScript will not prove `radius` exists from `kind` alone.

**When to use:** Closed sets of variants you own (events, AST nodes, API results). Prefer separate interfaces per variant over one bag of optional properties.

**Examples:**

```typescript
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
  }
}
```

**Sources:** [TypeScript handbook — Discriminated unions](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#discriminated-unions)

</details>

---

## The never type

<details>
<summary>The never type</summary>

**Definition:** `never` is the **bottom type** — the type of values that never occur. No value is assignable to `never` except `never` itself.

**How it works:**

- Functions that always `throw` or loop forever have return type `never`.
- After every member of a union is ruled out, the remaining type is `never`.
- `never` is assignable **to** every type (it will never actually be produced).

**When to use:** Impossible branches, exhaustiveness checks, and functions that do not return. Pair with [Exhaustiveness checking](#exhaustiveness-checking) in `switch`.

**Examples:**

```typescript
function fail(msg: string): never {
  throw new Error(msg);
}

function infinite(): never {
  while (true) {}
}

type Result = "ok" | "error";
function handle(r: Result) {
  if (r === "ok") return;
  if (r === "error") return;
  const remaining: never = r; // r: never — all cases handled
}
```

**Sources:** [TypeScript handbook — The never type](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#the-never-type)

</details>

---

## Exhaustiveness checking

<details>
<summary>Exhaustiveness checking</summary>

**Definition:** A pattern that uses `never` in a `switch` `default` branch so TypeScript errors when a new union member is not handled.

**How it works:**

- Handle every discriminant in `case` branches.
- In `default`, assign `const _check: never = value`.
- Adding a new variant makes `value` no longer `never` — compile fails until you add a `case`.

**When to use:** Discriminated unions that should stay closed as the codebase evolves.

**Pluses and minuses:**

| +                                                           | −                                                                  |
| ----------------------------------------------------------- | ------------------------------------------------------------------ |
| New variants become compile errors, not silent runtime bugs | Slightly more boilerplate in every `switch`                        |
| Documents that the union is meant to be complete            | `default` with `never` is easy to forget without a team convention |

**Examples:**

```typescript
interface Circle {
  kind: "circle";
  radius: number;
}
interface Square {
  kind: "square";
  sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default: {
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
    }
  }
}
```

**Thoughts:** Same idea as exhaustive `switch` in application code: unhandled variants should fail at compile time.

**Sources:** [TypeScript handbook — Exhaustiveness checking](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking)

</details>
