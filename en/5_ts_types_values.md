# TypeScript — Types — Values

Primitive types, arrays, and absence (`null` / `undefined`) from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html).

[← Types hub](./5_ts_types.md)

### Related docs

[Types hub](./5_ts_types.md) · [Object shapes](./5_ts_types_shapes.md) · [Combining types](./5_ts_types_combining.md) · [Object Types](./5_ts_objects.md) · [Narrowing](./5_ts_narrowing.md) · [Strictness & tsconfig](./5_ts_basics.md#strictness--tsconfig)

### Main concepts

| Topic      | Examples                                          |
| ---------- | ------------------------------------------------- |
| Primitives | `string`, `number`, `boolean`, `bigint`, `symbol` |
| Arrays     | `number[]`, `Array<string>`                       |
| Absence    | `null`, `undefined`                               |

### Sections

- [Primitives](#primitives)
- [Arrays](#arrays)
- [null and undefined](#null-and-undefined)

---

## Primitives

<details>
  <summary>Primitives</summary>

**Definition:**

JavaScript value types — `string`, `number`, `boolean`, `bigint`, and `symbol` — with matching lowercase TypeScript types (same names as `typeof`).

**How it works:**

- `string` — text like `"Hello, world"`.
- `number` — all numeric values; JavaScript has no separate `int` / `float`.
- `boolean` — `true` or `false`.
- `bigint` — arbitrary-precision integers (`100n`).
- `symbol` — unique opaque keys (`Symbol("id")`); see also [Symbols](./5_ts_symbols.md).
- `String`, `Number`, and `Boolean` (capitalized) are rare wrapper types; almost always use lowercase primitives.

**When to use:**

Annotate when inference is wrong; otherwise let TypeScript infer (`let city = "Berlin"` → `string`).

**Examples:**

```typescript
let name: string = "Alice";
let age: number = 42;
let active: boolean = true;

const hex: number = 0xf00d;
const binary: number = 0b1010;

let city = "Berlin"; // inferred: string
```

**Sources:** [TypeScript handbook — Everyday Types: primitives](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean)

</details>

---

## Arrays

<details>
  <summary>Arrays</summary>

**Definition:**

An ordered list where every element shares one type — written `number[]` or `Array<number>`.

**How it works:**

`T[]` and `Array<T>` mean the same thing. `[number]` is a **tuple** (fixed length), not a growable array.

**When to use:**

`const ids: number[] = [1, 2, 3]` for homogeneous lists.

**Examples:**

```typescript
const scores: number[] = [95, 87, 72];
const tags: Array<string> = ["typescript", "javascript"];
```

**Sources:** [TypeScript handbook — Everyday Types: arrays](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#arrays)

</details>

---

## null and undefined

<details>
  <summary>null and undefined</summary>

**Definition:**

Types for JavaScript's absent values — `null` (intentional empty) and `undefined` (unset).

**How it works:**

In JavaScript, `null` and `undefined` are real values. TypeScript’s **`strictNullChecks`** compiler option controls whether the checker forces you to handle them.

**`strictNullChecks` off** — values that might be `null` or `undefined` can still be used without checks. `null` and `undefined` can be assigned to properties typed as `string`, `number`, and so on. Behavior is similar to Java or C# **without** null-safe types. That looseness is a common source of runtime bugs; turn **`strictNullChecks` on** when you can (via `"strict": true` or `"strictNullChecks": true` — see [Strictness & tsconfig](./5_ts_basics.md#strictness--tsconfig)).

**`strictNullChecks` on** — `null` and `undefined` are only assignable where the type explicitly allows them (`string | null`, optional `prop?`, etc.). Before calling methods or reading properties on a value that might be missing, **narrow** the type (same idea as optional properties):

```typescript
function doSomething(x: string | null) {
  if (x === null) {
    // do nothing
  } else {
    console.log("Hello, " + x.toUpperCase());
  }
}
```

Also use optional chaining (`?.`), nullish coalescing (`??`), and patterns from [Narrowing](./5_ts_narrowing.md).

**Non-null assertion (`!`)** — postfix `!` tells the compiler to drop `null` and `undefined` from the type **without** a runtime check (a type assertion, not a guard):

```typescript
function liveDangerously(x?: number | null) {
  console.log(x!.toFixed()); // no error — you asserted x is defined
}
```

Only use `!` when you are sure the value is not `null` or `undefined`; otherwise you can still crash at runtime. Prefer narrowing; see [Type assertions](./5_ts_types_code.md#type-assertions).

**When to use:**

Keep **`strictNullChecks` enabled** on new code. Annotate APIs that may be empty: `string | null`, `User | undefined`, optional fields with `?`.

**Examples:**

```typescript
let title: string | null = null;
let config: string | undefined;

// strictNullChecks: ON — narrow before use
function greet(name: string | null) {
  if (name === null) return;
  console.log(name.toUpperCase());
}

function doSomething(x: string | null) {
  if (x === null) {
    return;
  }
  console.log("Hello, " + x.toUpperCase());
}

// strictNullChecks: OFF — compiles; may throw at runtime
function getLength(s: string) {
  return s.length;
}
// getLength(null);

// Non-null assertion — compile-time only
function logFixed(x: number | null) {
  if (x !== null) {
    console.log(x.toFixed());
  }
  // Or risky shortcut:
  // console.log(x!.toFixed());
}
```

**Sources:** [TypeScript handbook — null and undefined](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#null-and-undefined) · [strictNullChecks](https://www.typescriptlang.org/tsconfig#strictNullChecks) · [Non-null assertion operator](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#non-null-assertion-operator-postfix-)

</details>
