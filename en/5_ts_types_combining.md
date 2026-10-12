# TypeScript — Types — Combining types

Unions, literal types, and enums — ways to express “one of several” types.

[← Types hub](./5_ts_types.md)

### Related docs

[Types hub](./5_ts_types.md) · [Narrowing](./5_ts_narrowing.md) · [Enums](./5_ts_enums.md) · [Object shapes](./5_ts_types_shapes.md) · [Type inference](./5_ts_type_inference.md)

### Main concepts

| Topic             | Examples                                |
| ----------------- | --------------------------------------- |
| Unions            | `string \| number`                      |
| Literal types     | `"open"`, `42`                          |
| Literal inference | `const` keeps `"Ada"`; `let` → `string` |
| Enums             | `enum Status { Active }`                |

### Sections

- [Union types](#union-types)
- [Literal types](#literal-types)
- [Enums](#enums)

---

## Union types

<details>
  <summary>Union types</summary>

**Definition:**

A value that may be **one of several types**, written with `|`: `string | number` means “either a string or a number, not both at once.”

**How it works:**

- Each member is an alternative; the value must be assignable to **at least one** member.
- Without [narrowing](./5_ts_narrowing.md), you may only use members **common to every** alternative (`string | number` → `.length` is OK; `.toUpperCase` is not until you narrow).
- **Literal unions** restrict to specific values — see [Literal types](#literal-types): `type Status = "pending" | "done"`.
- Unions combine with objects for discriminated shapes (`kind: "circle" | "square"`) — see [Narrowing](./5_ts_narrowing.md) and [Interview — unions](./5_ts_interview.md).

**When to use:**

Parameters that accept multiple shapes (`string | string[]`), enums-as-literals, nullable unions (`string | null`), and API fields with a fixed set of variants.

**Examples:**

```typescript
type WindowState = "open" | "closed" | "minimized";

function padLeft(value: string, padding: string | number) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + value;
  }
  return padding + value;
}

function printId(id: string | number) {
  console.log("ID:", id);
}
```

**Sources:** [TypeScript handbook — Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) · [Working with union types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#working-with-union-types)

</details>

---

## Literal types

<details>
  <summary>Literal types</summary>

**Definition:**

A **literal type** is a type for one exact value — a specific string, number, boolean, or `bigint` — not the whole primitive domain. `type E = "east"` means only the string `"east"`, not every `string`.

**How it works:**

- String literals: `"hello"`, `'open'`.
- Numeric / boolean / bigint literals: `42`, `true`, `100n`.
- **Unions of literals:** `type Direction = "N" | "E" | "S" | "W"` — common alternative to `enum` for fixed sets.

**Examples:**

```typescript
// One exact string — not every string
type E = "east";
const heading: E = "east";
// heading = "west"; // Error: Type '"west"' is not assignable to type '"east"'

// Other literal kinds
type Roll = 1 | 2 | 3 | 4 | 5 | 6;
type YesNo = true | false;
type BigZero = 0n;

// Union of string literals (compass)
type Direction = "N" | "E" | "S" | "W";

function move(d: Direction) {
  console.log(d);
}

move("N");
move("X"); // Error: Argument of type '"X"' is not assignable to parameter of type 'Direction'
```

### Literal inference

**Definition:**

How TypeScript chooses a **literal type** vs a **wider primitive** when you initialize a variable without writing an explicit type annotation.

**How it works:**

| Declaration                            | Inferred type   | Why                                                                                        |
| -------------------------------------- | --------------- | ------------------------------------------------------------------------------------------ |
| `let x = "Hello World"`                | `string`        | `let` can be reassigned — TS **widens** string literals to `string`                        |
| `const x = "Hello World"`              | `"Hello World"` | `const` cannot be reassigned — TS keeps the **exact literal**                              |
| `let x: "Hello World" = "Hello World"` | `"Hello World"` | You **annotate** the literal type on `let` when you need a fixed value but mutable binding |

The same idea applies to numbers and booleans: `const n = 42` → `42`, `let n = 42` → `number`, `const ok = true` → `true`, `let ok = true` → `boolean`.

**`as const`** goes further on **objects and arrays**: property values stay literal, properties become `readonly`, and array literals often become **readonly tuples** — see examples below and [Type inference](./5_ts_type_inference.md).

**When to use:**

- **Literal types:** API flags, theme names, Redux `type` fields, discriminated unions (`kind: "circle" | "square"`). Prefer literal unions over numeric `enum` when you do not need reverse mapping.
- **Literal inference:** use **`const`** or **`as const`** so values stay narrow; use **`let` + annotation** (or a literal union type) when you need reassignment without widening to `string` / `number` / `boolean`.

**Examples:**

**Literal inference** (`let` widens, `const` does not):

```typescript
let helloWorld = "Hello World";
helloWorld = "Hello"; // OK — type is string

const helloWorldConst = "Hello World";
// helloWorldConst = "Hello"; // Error — type is "Hello World"

let pinned: "Hello World" = "Hello World";
pinned = "Hello World"; // OK — still the same literal
// pinned = "Hello"; // Error — not assignable to "Hello World"
```

**Literal unions and `as const`:**

```typescript
type Cardinal = "north" | "south" | "east" | "west";

function setDirection(d: Cardinal) {
  console.log(d);
}

const home = "north";
setDirection(home); // OK — "north" is assignable to Cardinal

let loose = "north";
// setDirection(loose); // Error — string is not assignable to Cardinal

const config = {
  theme: "dark",
  version: 1,
} as const;
// config.theme is "dark", not string; config is readonly
```

**Sources:** [TypeScript handbook — Literal Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) · [Literal Inference](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-inference) · [const assertions](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)

</details>

---

## Enums

<details>
  <summary>Enums</summary>

**Definition:**

An **`enum`** declares a set of named constants. TypeScript emits a **runtime object** (unlike most types), so `Status.Active` exists in JavaScript after compile.

**How it works:**

- **Numeric enum:** members auto-increment from `0` unless you set values; supports reverse mapping `Enum[0]`.
- **String enum:** each member needs an explicit string; better for logs and JSON.
- **`const enum`:** inlined at compile time — no runtime object, but less flexible across modules.
- **Alternatives:** literal union (`type Status = "active" | "inactive"`) or `as const` object — no extra bundle size; see [Literal types](#literal-types) and [Enums](./5_ts_enums.md).

**When to use:**

Legacy codebases and APIs that already use enums. In new app code, default to literal unions or `as const` unless you need enum-specific tooling.

**Examples:**

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

Direction.Up; // 0 at runtime

enum HttpCode {
  OK = 200,
  NotFound = 404,
}

// Literal union — no runtime enum object
type Direction2 = "Up" | "Down" | "Left" | "Right";
```

**Sources:** [TypeScript handbook — Enums](https://www.typescriptlang.org/docs/handbook/enums.html) · [Enums](./5_ts_enums.md)

</details>
