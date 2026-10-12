# TypeScript — Types in code

Writing types on variables, asserting when needed, and typing functions at everyday level.

[← Types hub](./5_ts_types.md)

### Related docs

[Types hub](./5_ts_types.md) · [Functions](./5_ts_functions.md) · [Assertions & satisfies](./5_ts_assertions.md) · [Safety](./5_ts_types_safety.md)

### Main concepts

| Topic       | Examples                                                      |
| ----------- | ------------------------------------------------------------- |
| Annotations | `let x: number`, `greet(name: string)`, `(): Promise<number>` |
| Assertions  | `value as string`                                             |
| Functions   | `(x: number) => string`                                       |

### Sections

- [Type annotations](#type-annotations)
- [Type assertions](#type-assertions)
- [Functions](#functions)

---

## Type annotations

<details>
  <summary>Type annotations</summary>

**Definition:**

An explicit type you write after a name — on a variable (`let x: string`), a function parameter (`name: string`), or a return type (`(): number`). Annotations tell the checker what you intend; they are erased at compile time and do not change runtime behavior.

**How it works:**

**On variables** — with `let`, `const`, or `var`, the annotation comes **after** the binding name, not in C/Java style (`int x`):

```typescript
let myName: string = "Alice";
```

Usually you **omit** the annotation: TypeScript **infers** the type from the initializer (`let myName = "Alice"` → `string`). You do not need to memorize every inference rule; start with fewer annotations than you expect and add them where the checker or your API contract needs help.

**On functions** — parameters are annotated after each name; return types go **after** the parameter list. TypeScript still checks **argument count** even when parameters are untyped (with [`noImplicitAny`](./5_ts_types_safety.md#any) off, missing parameter types become `any`).

| Placement    | Syntax                                                  |
| ------------ | ------------------------------------------------------- |
| Parameter    | `function greet(name: string) { ... }`                  |
| Return type  | `function getFavoriteNumber(): number { return 26; }`   |
| Async return | `async function load(): Promise<number> { return 26; }` |

Return types are often inferred from `return` statements; an explicit return type documents the contract, prevents accidental signature drift, or matches team style. Deeper topics (optional params, overloads, `this`, generics) live on [Functions](./5_ts_functions.md).

**Contextual typing** — in an **anonymous** function or arrow callback, TypeScript may infer parameter types from **where** the function is used (the surrounding call signature and argument types), so you can omit annotations:

```typescript
const names = ["Alice", "Bob", "Eve"];

names.forEach(function (s) {
  console.log(s.toUpperCase()); // `s` inferred as string
});

names.forEach((s) => {
  console.log(s.toUpperCase());
});
```

**When to use:**

- **Variables:** when inference is wrong (too narrow/wide), you declare without an initializer, or you want a readable public contract.
- **Functions:** on parameters at module boundaries; on return types when the inferred type is too wide or you want a stable exported API.
- **Skip** when inference or contextual typing already gives the right type — especially inside short callbacks and local `const` bindings.

**Examples:**

**Variable annotations** (`let`, `const`, `var`):

```typescript
let myName: string = "Alice";
const count: number = 0;
var legacy: boolean = true; // prefer let/const in modern code

// Inference — no annotation needed
let inferred = "Alice"; // type: string

// No initializer — annotation required (or assign on next line)
let pending: string;
pending = "ready";
```

**Parameter type annotations:**

```typescript
function greet(name: string) {
  console.log("Hello, " + name.toUpperCase() + "!!");
}

greet("Alice");
greet(42); // Error: Argument of type 'number' is not assignable to parameter of type 'string'

// Arity is still checked when parameters are untyped
function log(message: string, level: number) {
  console.log(level, message);
}
log("ok"); // Error: Expected 2 arguments, but got 1
```

**Return type annotations:**

```typescript
function getFavoriteNumber(): number {
  return 26;
}

// Inferred the same way without `: number` — explicit form documents the API
function double(n: number): number {
  return n * 2;
}
```

**Async functions — annotate with `Promise<T>`:**

```typescript
async function getFavoriteNumber(): Promise<number> {
  return 26;
}

async function fetchId(): Promise<string> {
  const res = await fetch("/api/me");
  const data = (await res.json()) as { id: string };
  return data.id;
}
```

**Contextual typing** (parameters inferred from the call site):

```typescript
const names = ["Alice", "Bob", "Eve"];

names.forEach(function (s) {
  console.log(s.toUpperCase()); // `s: string` from array + forEach signature
});

names.forEach((s) => {
  console.log(s.toUpperCase());
});

// Same idea on a typed callback parameter
type StringHandler = (value: string) => void;
const handle: StringHandler = (value) => {
  console.log(value.length); // `value: string` from StringHandler
};
```

**Sources:** [TypeScript handbook — Type Annotations on Variables](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-annotations-on-variables) · [Parameter Type Annotations](https://www.typescriptlang.org/docs/handbook/2/functions.html#parameter-type-annotations) · [Return Type Annotations](https://www.typescriptlang.org/docs/handbook/2/functions.html#return-type-annotations) · [Functions Which Return Promises](https://www.typescriptlang.org/docs/handbook/2/functions.html#functions-which-return-promises) · [Anonymous Functions (contextual typing)](https://www.typescriptlang.org/docs/handbook/2/functions.html#anonymous-functions)

</details>

---

## Type assertions

<details>
  <summary>Type assertions</summary>

**Definition:**

A **type assertion** tells the compiler to treat a value as a specific type. It is erased when JavaScript runs — no runtime check or conversion. You are overriding inference when you know more than TypeScript can prove.

**How it works:**

- **`as` syntax:** `const el = document.getElementById("app") as HTMLDivElement`. In `.tsx`, prefer `as` over angle brackets (`<Type>value`) because of JSX.
- **Overlap rule:** Source and target types must overlap; `"hello" as number` is an error. For intentional double casts: `value as unknown as Target`.
- **Non-null assertion `!`:** `user!.name` removes `null` and `undefined` from the type — also no runtime check.
- **Not a substitute for narrowing:** Prefer `typeof`, `in`, or type guards when you can validate at runtime — see [Narrowing](./5_ts_narrowing.md).
- **`satisfies`:** Validates shape **without** widening literals (different operator) — [Assertions & satisfies](./5_ts_assertions.md).

**When to use:**

DOM nodes, migration from untyped JS, or tests where you control the value. After [unknown](#unknown) boundaries, narrow first; assert only when a guard is impractical.

**Examples:**

```typescript
const canvas = document.getElementById("main") as HTMLCanvasElement;
const ctx = canvas.getContext("2d");

function load(raw: unknown) {
  const data = raw as { id: string };
  return data.id;
}

const maybeName: string | undefined = getName();
const len = maybeName!.length; // non-null assertion — risky if null
```

**Sources:** [TypeScript handbook — Type Assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions) · [Non-null assertion](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#non-null-assertion-operator-postfix-) · [Assertions & satisfies](./5_ts_assertions.md)

</details>

---

## Functions

<details>
  <summary>Functions</summary>

**Definition:**

Functions are values with a **call signature**: parameter types and a return type. You annotate declarations, or describe a function type with `(params) => ReturnType`.

**How it works:**

- Declaration: `function add(a: number, b: number): number { return a + b; }`
- Expression type: `type BinaryOp = (a: number, b: number) => number`
- Callback parameters: pass a function that matches the expected signature; TypeScript checks arity and types at the call site.
- Optional/default parameters, rest `...args`, generics, overloads, and `this` typing live on the dedicated [Functions](./5_ts_functions.md) page.

**When to use:**

Annotate public APIs and callbacks when inference is unclear. Prefer a **union parameter** over overloads when one implementation handles every variant — see [Functions — Function Overloads](./5_ts_functions.md#function-overloads).

**Examples:**

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

type StringPredicate = (s: string) => boolean;

function filterStrings(items: string[], pred: StringPredicate): string[] {
  return items.filter(pred);
}

const isLong: StringPredicate = (s) => s.length > 5;
```

**Sources:** [TypeScript handbook — Function Type Expressions](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-type-expressions) · [Functions](./5_ts_functions.md)

</details>
