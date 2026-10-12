# TypeScript — Type Manipulation

Creating types from types — generics, type operators, conditional and mapped types — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Object Types](./5_ts_objects.md#generic-object-types) · [Functions](./5_ts_functions.md#generic-functions) · [Utility Types](./5_ts_utility_types.md) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                   | Covers                                                       |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| [Overview](#overview)                                   | What Type Manipulation is and handbook topic map             |
| [Creating Types from Types](#creating-types-from-types) | Compose types from existing types                            |
| [Generics](#generics)                                   | Type parameters, constraints, generic classes and interfaces |
| [Keyof Type Operator](#keyof-type-operator)             | Union of object keys — `keyof T`                             |
| [`typeof` Type Operator](#typeof-type-operator)         | Type of a value in type position                             |
| [Indexed Access Types](#indexed-access-types)           | Look up property types — `T["key"]`                          |
| [Conditional Types](#conditional-types)                 | Type-level if/else — `T extends U ? X : Y`                   |
| [Mapped Types](#mapped-types)                           | Transform every property — `{ [K in keyof T]: ... }`         |
| [Template Literal Types](#template-literal-types)       | String types from unions and literals                        |
| [How the tools combine](#how-the-tools-combine)         | Patterns chaining operators                                  |
| [Map Pattern](#map-pattern)                             | Union → object lookup table                                  |
| [Branded / Nominal Types](#branded--nominal-types)      | Interview pattern for distinct primitives                    |

### Sections

- [Overview](#overview)
- [Creating Types from Types](#creating-types-from-types)
- [Generics](#generics)
- [Keyof Type Operator](#keyof-type-operator)
- [`typeof` Type Operator](#typeof-type-operator)
- [Indexed Access Types](#indexed-access-types)
- [Conditional Types](#conditional-types)
- [Mapped Types](#mapped-types)
- [Template Literal Types](#template-literal-types)
- [How the tools combine](#how-the-tools-combine)
- [Map Pattern](#map-pattern)
- [Branded / Nominal Types](#branded--nominal-types)

---

## Overview

<details>
<summary>Type Manipulation</summary>

**Definition:** **Type Manipulation** is the handbook track for building **new types from existing types** — generics, type operators, and type-level transforms. You describe relationships between types instead of duplicating shapes by hand.

**How it works:**

| Topic                                             | Covers                                            | Example                           |
| ------------------------------------------------- | ------------------------------------------------- | --------------------------------- |
| [Generics](#generics)                             | Type parameters on functions, interfaces, classes | `function id<T>(x: T): T`         |
| [Keyof Type Operator](#keyof-type-operator)       | Union of keys — `keyof T`                         | `type K = keyof { a: 1 }` → `"a"` |
| [`typeof` Type Operator](#typeof-type-operator)   | Type of a value in type position                  | `type S = typeof myString`        |
| [Indexed Access Types](#indexed-access-types)     | Property type lookup                              | `type Age = Person["age"]`        |
| [Conditional Types](#conditional-types)           | Type-level if/else                                | `T extends U ? X : Y`             |
| [Mapped Types](#mapped-types)                     | Transform each property                           | `{ [K in keyof T]: boolean }`     |
| [Template Literal Types](#template-literal-types) | String types from unions                          | `` `${"a" \| "b"}_id` ``          |

**Prerequisites:** Everyday types ([Types](./5_ts_types.md)), object shapes ([Object Types](./5_ts_objects.md)), and function generics basics ([Functions — Generic Functions](./5_ts_functions.md#generic-functions)). Built-in helpers built on these patterns: [Utility Types](./5_ts_utility_types.md).

**When to use:** Reusable APIs (`fetchJson<T>`), safe property access (`keyof` + `extends`), deriving types from values (`typeof`, indexed access), and custom utility types (conditionals, mapped types, template literals).

**Thoughts:** Read in handbook order above. Start with **Generics**, then **keyof** / **typeof** / **indexed access**; add **conditional** and **mapped** types when you need `infer`, `Partial`-style transforms, or key remapping. Extra patterns at the end: [How the tools combine](#how-the-tools-combine), [Map Pattern](#map-pattern), [Branded types](#branded--nominal-types).

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) · [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Creating Types from Types

<details>
<summary>Creating Types from Types</summary>

**Definition:** Techniques to build **new types from existing types** without duplicating shapes — the core of advanced TypeScript typing.

**How it works:**

- **Generics** — parameterize types and functions (`Box<T>`).
- **Type operators** — `keyof`, `typeof`, indexed access (`T[K]`).
- **Type transforms** — conditional types, mapped types, template literal types.
- These tools combine: e.g. `ReturnType<typeof fn>` uses `typeof` + conditional + `infer`.

**When to use:** When a runtime value or base interface already encodes the shape you need, derive types instead of maintaining parallel definitions.

**Examples:**

```typescript
type Box<T> = { value: T };
type Keys = keyof Box<{ id: number }>; // "value"
type Id = Box<{ id: number }>["value"]; // number
```

**Thoughts:** Start with generics and `keyof`/`typeof`; add conditionals and mapped types when utility types or API wrappers need them. See also [Utility Types](./5_ts_utility_types.md) for built-ins built on these patterns.

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>

---

## Generics

<details>
<summary>Generics</summary>

**Definition:** Type parameters (`<T>`) let one function, class, or interface work over many types while preserving type information — unlike `any`, which erases it.

**How it works:**

| Topic                 | Example                                             |
| --------------------- | --------------------------------------------------- |
| Identity function     | `function identity<T>(arg: T): T { return arg; }`   |
| Inference             | `identity("hi")` → `string`                         |
| Generic arrays        | `function first<T>(arr: T[]): T { return arr[0]; }` |
| Generic function type | `let fn: <T>(x: T) => T`                            |
| Generic interface     | `interface Fn<T> { (x: T): T }`                     |
| Generic class         | `class Box<T> { value: T }`                         |
| `extends` constraint  | `<T extends { length: number }>(x: T)`              |
| `keyof` constraint    | `<T, K extends keyof T>(obj: T, key: K)`            |
| Class factory         | `function create<T>(c: new () => T): T`             |
| Default type param    | `<T = HTMLDivElement>(el?: T)`                      |

### Hello World — the identity function

- `function identity<Type>(arg: Type): Type` captures the argument type and returns the same type.
- Call with explicit type arg: `identity<string>("myString")`.
- Or rely on **type argument inference**: `identity("myString")` → `string`.

```typescript
function identity<Type>(arg: Type): Type {
  return arg;
}
const a = identity("myString"); // string
```

### Working with generic type variables

- `T` stands for any type — you cannot assume `.length` unless constrained.
- Use `T[]` or `Array<T>` when the argument is an array.

```typescript
function firstElement<T>(arr: T[]): T {
  return arr[0];
}
```

### Generic types (function types and interfaces)

- Function type: `let fn: <T>(arg: T) => T`.
- Generic interface: `interface GenericIdentityFn<Type> { (arg: Type): Type }`.

```typescript
interface GenericIdentityFn<Type> {
  (arg: Type): Type;
}
let fn: GenericIdentityFn<number> = (n) => n;
```

### Generic classes

- Type parameter on the class applies to **instance** members, not static members.

```typescript
class GenericNumber<NumType> {
  zeroValue!: NumType;
  add!: (x: NumType, y: NumType) => NumType;
}
```

### Generic constraints

- `extends` limits what `T` may be; combine with `keyof` for safe property access.

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}
```

### Using class types in generics

- Pass a constructor type to build instances in a factory.

```typescript
function create<Type>(c: { new (): Type }): Type {
  return new c();
}
```

### Generic parameter defaults

- Omitted type arguments fall back to defaults; required params come first.

```typescript
declare function tag<T extends HTMLElement = HTMLDivElement>(el?: T): T;
```

**When to use:** Reusable containers, API wrappers, React hooks/components, and any API where input and output types should stay linked. Function-level basics: [Generic Functions](./5_ts_functions.md#generic-functions). Object-level: [Generic Object Types](./5_ts_objects.md#generic-object-types).

**Thoughts:** Interview pattern — generic API wrapper: `async function fetchJson<T>(url: string): Promise<ApiResponse<T>>` with `ApiResponse<T> = { success: true; data: T } | { success: false; error: string }`.

**Sources:** [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Keyof Type Operator

<details>
<summary>Keyof Type Operator</summary>

**Definition:** `keyof T` produces a union of `T`'s keys — `"a" | "b"` for `{ a: 1; b: 2 }`.

**How it works:**

- With index signatures, `keyof { [k: string]: boolean }` → `string | number` (numeric keys stringify).
- Combine with `extends` for safe property access: `get<T, K extends keyof T>(obj: T, key: K): T[K]`.
- Foundation for mapped types and many utility types.

**Examples:**

```typescript
type Point = { x: number; y: number };
type P = keyof Point; // "x" | "y"

type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish; // number

type Mapish = { [k: string]: boolean };
type M = keyof Mapish; // string | number

function pick<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

**Sources:** [TypeScript handbook — Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

</details>

---

## `typeof` Type Operator

<details>
<summary>`typeof` Type Operator</summary>

**Definition:** In **type position**, `typeof` refers to the **type** of a value — not the runtime `typeof` string.

**How it works:**

- Values and types live in separate namespaces — use `typeof myVar` to get a type from a value.
- `ReturnType<typeof fn>` needs `typeof` because `fn` is a value, not a type.
- Works on identifiers and their properties; not arbitrary call expressions.

**Examples:**

```typescript
let s = "hello";
let n: typeof s; // string

function f() {
  return { x: 10, y: 3 };
}

type P = ReturnType<typeof f>; // { x: number; y: number }

// type P = ReturnType<f>; // Error — f is a value

const STATUS_CONFIG = {
  idle: { label: "Idle", retry: false },
  loading: { label: "Loading…", retry: false },
} as const;

type Status = keyof typeof STATUS_CONFIG;
```

**Sources:** [TypeScript handbook — Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)

</details>

---

## Indexed Access Types

<details>
<summary>Indexed Access Types</summary>

**Definition:** Look up a property's type from another type — `Person["age"]` → `number`, or `T[K]` where `K extends keyof T`.

**How it works:**

- Index must be a **type**, not a runtime variable — use `typeof key` with `as const` for value keys.
- `Person[keyof Person]` — union of all property types.
- `typeof MyArray[number]` — element type of an array value.

**Examples:**

```typescript
type Person = { age: number; name: string; alive: boolean };

type Age = Person["age"]; // number
type I1 = Person["age" | "name"]; // string | number
type I2 = Person[keyof Person]; // string | number | boolean

const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
];
type PersonFromArray = (typeof MyArray)[number];
type AgeFromArray = (typeof MyArray)[number]["age"]; // number

type key = "age";
type Good = Person[key]; // number
```

**Sources:** [TypeScript handbook — Indexed Access Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)

</details>

---

## Conditional Types

<details>
<summary>Conditional Types</summary>

**Definition:** Type-level if/else — `SomeType extends OtherType ? TrueType : FalseType`.

**How it works:**

- **Distributive** over naked type parameters in unions: `ToArray<string | number>` → `string[] | number[]`.
- Wrap in `[T]` to disable distribution: `[Type] extends [any] ? Type[] : never`.
- `infer` extracts types inside conditional branches — powers `ReturnType`, `Parameters`, custom unwrap helpers.

**Examples:**

```typescript
type Example1 = Dog extends Animal ? number : string; // number

type NameOrId<T extends number | string> = T extends number
  ? IdLabel
  : NameLabel;

type MessageOf<T> = T extends { message: unknown } ? T["message"] : never;

type Flatten<T> = T extends Array<infer Item> ? Item : T;

type Str = Flatten<string[]>; // string

type GetReturnType<T> = T extends (...args: never[]) => infer Return
  ? Return
  : never;

type Num = GetReturnType<() => number>; // number
```

**Sources:** [TypeScript handbook — Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)

</details>

---

## Mapped Types

<details>
<summary>Mapped Types</summary>

**Definition:** Transform every property of a type by iterating keys — `{ [Property in keyof T]: NewType }`.

**How it works:**

- Mapping modifiers: `readonly`, `?`, and removers `-readonly`, `-?`.
- Key remapping with `as` (TS 4.1+): `[K in keyof T as NewKey]: T[K]`.
- Filter keys by mapping to `never`.
- Base for `Partial`, `Record`, `Pick`, and custom flag objects.

**Examples:**

```typescript
type OptionsFlags<T> = {
  [Property in keyof T]: boolean;
};

type Features = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<Features>;
// { darkMode: boolean; newUserProfile: boolean }

type CreateMutable<T> = {
  -readonly [P in keyof T]: T[P];
};

type Concrete<T> = {
  [P in keyof T]-?: T[P];
};

type UpdatedPerson = {
  [P in keyof Person]: P extends "name" ? string : number;
};
```

**Sources:** [TypeScript handbook — Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

</details>

---

## Template Literal Types

<details>
<summary>Template Literal Types</summary>

**Definition:** String types built from string literals and unions — `` `hello ${World}` ``, `` `${A | B}_id` ``.

**How it works:**

- Unions in template positions **cross-multiply**: `` `${"a" | "b"}_${"x" | "y"}` `` → four combinations.
- Combine with mapped types for event names: `` `${K}Changed` ``.
- Built-in intrinsics (not locale-aware): `Uppercase`, `Lowercase`, `Capitalize`, `Uncapitalize`.

**Examples:**

```typescript
type World = "world";
type Greeting = `hello ${World}`; // "hello world"

type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;

type Shouty = Uppercase<"hello">; // "HELLO"
type Cap = Capitalize<"hello world">; // "Hello world"

type PropEventSource<T> = {
  on<K extends string & keyof T>(
    eventName: `${K}Changed`,
    callback: (newValue: T[K]) => void,
  ): void;
};
```

**Sources:** [TypeScript handbook — Template Literal Types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)

</details>

---

## How the tools combine

<details>
<summary>How the tools combine</summary>

**Definition:** Real-world types chain operators — rarely used in isolation.

**Examples:**

```typescript
// keyof + indexed access + mapped + template literal
type EventMap<T> = {
  [K in keyof T as `${string & K}Changed`]: T[K];
};

// typeof + conditional + infer (like ReturnType)
type MyReturn<T> = T extends (...args: never[]) => infer R ? R : never;

// Generic + keyof + extends (typed getProperty)
function pick<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>

---

## Map Pattern

<details>
<summary>Map Pattern</summary>

**Definition:** Replace a large union of incompatible shapes with an object keyed by a discriminant — type-safe lookup without impossible intersections.

**When to use:** `type Status = "idle" | "loading" | "error"` works until each status needs different metadata. A `Record` or `as const` object scales better.

**Examples:**

```typescript
const STATUS_CONFIG = {
  idle: { label: "Idle", retry: false },
  loading: { label: "Loading…", retry: false },
  success: { label: "Done", retry: false },
  error: { label: "Failed", retry: true },
} as const;

type Status = keyof typeof STATUS_CONFIG;

function getConfig(s: Status) {
  return STATUS_CONFIG[s]; // autocomplete for all keys
}
```

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>

---

## Branded / Nominal Types

<details>
<summary>Branded / Nominal Types</summary>

**Definition:** Make structurally identical types incompatible by adding a phantom brand field — common interview pattern.

**Examples:**

```typescript
type UserId = string & { readonly __brand: unique symbol };
type OrderId = string & { readonly __brand: unique symbol };

function userId(id: string): UserId {
  return id as UserId;
}

function getUser(id: UserId) {
  /* ... */
}

const uid = userId("u-123");
const oid = userId("o-456") as OrderId;
// getUser(oid); // Error — OrderId is not UserId
getUser(uid); // OK
```

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>
