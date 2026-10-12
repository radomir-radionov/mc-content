# TypeScript — Object Types

Object shapes, property modifiers, index signatures, extending and combining types, generics on objects, arrays, and tuples — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/objects.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Types](./5_ts_types.md) · [Narrowing](./5_ts_narrowing.md) · [Functions](./5_ts_functions.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [Classes](./5_ts_classes.md) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                                        | Covers                                        |
| ---------------------------------------------------------------------------- | --------------------------------------------- |
| [Quick Reference](#quick-reference)                                          | Index of object-type topics in handbook order |
| [Property Modifiers](#property-modifiers)                                    | `?` and `readonly` on properties              |
| [Optional Properties](#optional-properties)                                  | Keys that may be absent                       |
| [`readonly` Properties](#readonly-properties)                                | Shallow immutability on fields                |
| [Index Signatures](#index-signatures)                                        | Dynamic string/number keys                    |
| [Excess Property Checks](#excess-property-checks)                            | Fresh literal typos                           |
| [Extending Types](#extending-types)                                          | `interface` `extends`                         |
| [Intersection Types](#intersection-types)                                    | Combine shapes with `&`                       |
| [Interface Extension vs. Intersection](#interface-extension-vs-intersection) | Merge vs `never` on conflicts                 |
| [Generic Object Types](#generic-object-types)                                | `Box<T>` and object generics                  |
| [The `Array` Type](#the-array-type)                                          | `T[]` and `Array<T>`                          |
| [The `ReadonlyArray` Type](#the-readonlyarray-type)                          | `readonly T[]` / `ReadonlyArray<T>`           |
| [Tuple Types](#tuple-types)                                                  | Fixed-length, typed positions                 |
| [`readonly` Tuple Types](#readonly-tuple-types)                              | Immutable tuple elements                      |

### Sections

- [Quick Reference](#quick-reference)
- [Property Modifiers](#property-modifiers)
- [Optional Properties](#optional-properties)
- [`readonly` Properties](#readonly-properties)
- [Index Signatures](#index-signatures)
- [Excess Property Checks](#excess-property-checks)
- [Extending Types](#extending-types)
- [Intersection Types](#intersection-types)
- [Interface Extension vs. Intersection](#interface-extension-vs-intersection)
- [Generic Object Types](#generic-object-types)
- [The `Array` Type](#the-array-type)
- [The `ReadonlyArray` Type](#the-readonlyarray-type)
- [Tuple Types](#tuple-types)
- [`readonly` Tuple Types](#readonly-tuple-types)

### Topic groups

- **Property Modifiers** — Property Modifiers, Optional Properties, `readonly` Properties, Index Signatures
- **Generic Object Types** — Generic Object Types, The `Array` Type, The `ReadonlyArray` Type, Tuple Types, `readonly` Tuple Types

Top-level (no section header): Quick Reference, Excess Property Checks, Extending Types, Intersection Types, Interface Extension vs. Intersection.

---

## Quick Reference

<details>
<summary>Quick Reference</summary>

**Definition:** One-page map of the [TypeScript handbook — Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html) topics in this file.

**How it works:**

| Topic                                                                        | Summary                                 |
| ---------------------------------------------------------------------------- | --------------------------------------- |
| [Property Modifiers](#property-modifiers)                                    | `?` optional keys and `readonly` fields |
| [Optional Properties](#optional-properties)                                  | `nick?: string` — key may be missing    |
| [`readonly` Properties](#readonly-properties)                                | Cannot reassign the property            |
| [Index Signatures](#index-signatures)                                        | `[key: string]: T` dynamic keys         |
| [Excess Property Checks](#excess-property-checks)                            | Extra keys on object literals           |
| [Extending Types](#extending-types)                                          | `interface B extends A`                 |
| [Intersection Types](#intersection-types)                                    | `A & B` — must satisfy both             |
| [Interface Extension vs. Intersection](#interface-extension-vs-intersection) | How conflicts are reported              |
| [Generic Object Types](#generic-object-types)                                | `interface Box<T> { contents: T }`      |
| [The `Array` Type](#the-array-type)                                          | `T[]` / `Array<T>`                      |
| [The `ReadonlyArray` Type](#the-readonlyarray-type)                          | `readonly T[]`                          |
| [Tuple Types](#tuple-types)                                                  | `[string, number]` fixed slots          |
| [`readonly` Tuple Types](#readonly-tuple-types)                              | `readonly` tuple elements               |

For inline shapes, `interface`, and `type` aliases on everyday objects, see [Types — Object shapes](./5_ts_types_shapes.md).

**Sources:** [TypeScript handbook — Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html)

</details>

---

## Property Modifiers

<details>
<summary>Property Modifiers</summary>

**Definition:** Keywords that change how **properties** behave on object types — mainly **`?` (optional)** and **`readonly` (assign-once)**.

**How it works:**

- **Optional (`?`)** — the key may be omitted; under `strictNullChecks`, reads are `T | undefined`. See [Optional Properties](#optional-properties).
- **`readonly`** — the property cannot be reassigned after initialization; nested objects may still mutate. See [`readonly` Properties](#readonly-properties).
- Modifiers apply per property on `interface` and `type` object shapes; they do not change runtime behavior.

**Examples:**

```typescript
interface PaintOptions {
  shape: Shape;
  xPos?: number;
  readonly id: string;
}
```

**Sources:** [TypeScript handbook — Property Modifiers](https://www.typescriptlang.org/docs/handbook/2/objects.html#property-modifiers)

</details>

---

## Optional Properties

<details>
<summary>Optional Properties</summary>

**Definition:** Properties that may be absent, marked with `?` on an interface or type alias.

**How it works:**

- `xPos?: number` means the key may be missing.
- Under `strictNullChecks`, reading an optional property gives `T | undefined`.
- Defaults via destructuring (`{ xPos = 0 }`) narrow to `number` inside the function.
- In destructuring, `{ shape: Shape }` **renames** `shape` to variable `Shape` — it does not add a type. Put the type after the whole pattern: `({ shape, xPos = 100 }: PaintOptions)`.

**Examples:**

```typescript
interface PaintOptions {
  shape: Shape;
  xPos?: number;
  yPos?: number;
}

paintShape({ shape });
paintShape({ shape, xPos: 100 });

function paintShape(opts: PaintOptions) {
  let xPos = opts.xPos; // number | undefined
  let xPos2 = opts.xPos === undefined ? 0 : opts.xPos; // number

  function paint({ shape, xPos = 0, yPos = 0 }: PaintOptions) {
    console.log(xPos, yPos); // both number
  }
}

// Correct — type after the pattern
function draw({ shape, xPos = 100 }: { shape: Shape; xPos?: number }) {}
```

**Sources:** [TypeScript handbook — Optional Properties](https://www.typescriptlang.org/docs/handbook/2/objects.html#optional-properties)

</details>

---

## `readonly` Properties

<details>
<summary>`readonly` Properties</summary>

**Definition:** Object properties that cannot be reassigned after initialization. Compile-time only — **shallow** immutability.

**How it works:**

- `readonly prop: string` blocks reassignment of `prop`, not mutation of nested objects.
- A mutable object assigned to a `readonly` view can still change through the mutable alias.
- `readonly` is ignored when checking assignability via aliasing in some cases — treat `readonly` as a usage contract, not a runtime guarantee.

**Examples:**

```typescript
interface SomeType {
  readonly prop: string;
}

function doSomething(obj: SomeType) {
  console.log(obj.prop); // OK
  // obj.prop = "hello"; // Error
}

interface Home {
  readonly resident: { name: string; age: number };
}

function visitForBirthday(home: Home) {
  home.resident.age++; // OK — nested mutation
  // home.resident = { ... }; // Error
}

let writable: Person = { name: "Alice", age: 42 };
let readonly: ReadonlyPerson = writable;
writable.age++; // mutates shared object — readonly view sees 43
```

**Sources:** [TypeScript handbook — `readonly` Properties](https://www.typescriptlang.org/docs/handbook/2/objects.html#readonly-properties)

</details>

---

## Index Signatures

<details>
<summary>Index Signatures</summary>

**Definition:** Describe objects with dynamic keys: `{ [key: string]: number }` or `{ [index: number]: string }`.

**How it works:**

- All named properties must be assignable to the index signature value type.
- Numeric index values must be a subtype of string index values (`obj[100]` === `obj["100"]` in JS).
- Use a union value type when some keys need different types.
- `readonly [index: number]: string` makes index assignment illegal.

**Examples:**

```typescript
interface StringArray {
  [index: number]: string;
}

const arr: StringArray = getStringArray();
const item = arr[1]; // string

interface NumberDictionary {
  [index: string]: number;
  length: number; // OK
  // name: string; // Error
}

interface FlexibleDict {
  [index: string]: number | string;
  length: number;
  name: string;
}

interface ReadonlyStringArray {
  readonly [index: number]: string;
}
```

**Sources:** [TypeScript handbook — Index Signatures](https://www.typescriptlang.org/docs/handbook/2/objects.html#index-signatures)

</details>

---

## Excess Property Checks

<details>
<summary>Excess Property Checks</summary>

**Definition:** TypeScript rejects **fresh object literals** with properties not declared on the target type — catches typos like `colour` vs `color`.

**How it works:**

- Applies to object literals passed directly to a typed parameter.
- Does **not** apply when assigning through a variable first (structural check only).
- Workarounds: type assertion (`as SquareConfig`), widen with a string index signature, or fix the typo — prefer fixing.

**Examples:**

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig) {
  /* ... */
}

// Error — 'colour' is not in SquareConfig
// createSquare({ colour: "red", width: 100 });

createSquare({ width: 100, opacity: 0.5 } as SquareConfig);

interface LooseConfig {
  color?: string;
  width?: number;
  [prop: string]: unknown;
}

let options = { colour: "red", width: 100 };
createSquare(options); // OK — not a fresh literal
```

**Sources:** [TypeScript handbook — Excess Property Checks](https://www.typescriptlang.org/docs/handbook/2/objects.html#excess-property-checks)

</details>

---

## Extending Types

<details>
<summary>Extending Types</summary>

**Definition:** Add fields to an existing interface with `extends` — stack specialized shapes on a base.

**How it works:**

- `interface Admin extends User { role: string }` — single or multiple parents: `extends A, B`.
- For type aliases, use intersection (`&`) instead of `extends`.
- Signals intent and avoids copy-paste when building variants.

**Examples:**

```typescript
interface BasicAddress {
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string;
}

interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}
interface ColorfulCircle extends Colorful, Circle {}

const cc: ColorfulCircle = { color: "red", radius: 42 };
```

**Sources:** [TypeScript handbook — Extending Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#extending-types)

</details>

---

## Intersection Types

<details>
<summary>Intersection Types</summary>

**Definition:** Combine types with `&` — a value must satisfy **all** members of every part.

**How it works:**

- `type ColorfulCircle = Colorful & Circle` — must have both `color` and `radius`.
- Conflicting property types on the same key become `never` (often silently until you use the property).
- Good for composing object shapes from type aliases; use `extends` for interface hierarchies.

**Examples:**

```typescript
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;

function draw(circle: ColorfulCircle) {
  console.log(circle.color, circle.radius);
}

draw({ color: "blue", radius: 42 });
// draw({ color: "red", raidus: 42 }); // Error — typo caught

type Person = { name: string; age: number };
type Employee = { company: string; position: string };
type EmployeePerson = Person & Employee;
```

**Sources:** [TypeScript handbook — Intersection Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types)

</details>

---

## Interface Extension vs. Intersection

<details>
<summary>Interface Extension vs. Intersection</summary>

**Definition:** Two ways to combine object types — they handle **property conflicts** differently.

**How it works:**

- Duplicate `interface` declarations with the same name **merge** if properties are compatible; incompatible merge → compile error.
- Intersection of types with conflicting property types → property type becomes `never`, file still compiles.
- Prefer `extends` for interface hierarchies when you want conflicts to error early.

**Examples:**

```typescript
interface Person {
  name: string;
}
// interface Person { name: number; } // Error — incompatible merge

interface Person1 {
  name: string;
}
interface Person2 {
  name: number;
}
type Staff = Person1 & Person2;
declare const staffer: Staff;
// staffer.name is type `never`
```

**Sources:** [TypeScript handbook — Interface Extension vs. Intersection](https://www.typescriptlang.org/docs/handbook/2/objects.html#interface-extension-vs-intersection)

</details>

---

## Generic Object Types

<details>
<summary>Generic Object Types</summary>

**Definition:** Object types parameterized by other types — `Box<Type>` substitutes `Type` everywhere in the shape.

**How it works:**

- `Box<string>` is equivalent to `{ contents: string }` after substitution.
- Generics work on `interface` and `type` aliases.
- Type aliases can build helpers: `type OrNull<T> = T | null`.
- Avoid `any` on generic containers — use a type parameter instead.

**Examples:**

```typescript
interface Box<Type> {
  contents: Type;
}

let box: Box<string> = { contents: "hello" };

type AppleBox = Box<Apple>;

function setContents<Type>(box: Box<Type>, newContents: Type) {
  box.contents = newContents;
}

type OrNull<Type> = Type | null;
type OneOrMany<Type> = Type | Type[];
type OneOrManyOrNull<Type> = OrNull<OneOrMany<Type>>;
```

**See also:** [Generics](./5_ts_type_manipulation.md#generics) — constraints, generic classes, and type-parameter defaults.

**Sources:** [TypeScript handbook — Generic Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#generic-object-types)

</details>

---

## The `Array` Type

<details>
<summary>The `Array` Type</summary>

**Definition:** Typed lists — `Array<T>` and `T[]` are equivalent notations for a mutable array of `T`.

**Examples:**

```typescript
function process(values: Array<string>) {
  /* ... */
}
function process2(values: string[]) {
  /* ... */
} // same

let nums: number[] = [1, 2, 3];
nums.push(4); // OK
```

**Sources:** [TypeScript handbook — The `Array` Type](https://www.typescriptlang.org/docs/handbook/2/objects.html#the-array-type)

</details>

---

## The `ReadonlyArray` Type

<details>
<summary>The `ReadonlyArray` Type</summary>

**Definition:** Immutable array views — `ReadonlyArray<T>` and `readonly T[]` are equivalent; disallow `push`, `pop`, `splice`, etc.

**How it works:**

- No `ReadonlyArray` constructor — assign from a regular array.
- Mutable `string[]` assignable to `readonly string[]`; not the reverse.
- `readonly T[]` is syntactic sugar; `ReadonlyArray<T>` reads clearer in generic code.

**Examples:**

```typescript
function readOnly(values: readonly string[]) {
  values.slice(); // OK
  // values.push("x"); // Error
}

let x: readonly string[] = [];
let y: string[] = [];
x = y; // OK
// y = x; // Error
```

**Sources:** [TypeScript handbook — The `ReadonlyArray` Type](https://www.typescriptlang.org/docs/handbook/2/objects.html#the-readonlyarray-type)

</details>

---

## Tuple Types

<details>
<summary>Tuple Types</summary>

**Definition:** Fixed-length arrays with a specific type at each index — order matters.

**How it works:**

- `[string, number]` is not assignable from `[number, string]`.
- Optional elements: `[number, number, number?]` — `length` is `2 | 3`.
- Rest in tuples: `[string, number, ...boolean[]]`.
- Tuple params mirror rest params: `(...args: [string, number, ...boolean[]])`.

**Examples:**

```typescript
type StringNumberPair = [string, number];

let x: StringNumberPair = ["hello", 42];
// let bad: StringNumberPair = [42, "hello"]; // Error

function doSomething(pair: [string, number]) {
  const a = pair[0]; // string
  const b = pair[1]; // number
}

type Either2dOr3d = [number, number, number?];

type StringNumberBooleans = [string, number, ...boolean[]];
const b: StringNumberBooleans = ["world", 2, true, false];

function readButtonInput(...args: [string, number, ...boolean[]]) {
  const [name, version, ...input] = args;
}
```

**Sources:** [TypeScript handbook — Tuple Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#tuple-types)

</details>

---

## `readonly` Tuple Types

<details>
<summary>`readonly` Tuple Types</summary>

**Definition:** Tuple types whose elements cannot be reassigned — `readonly [string, number]`.

**How it works:**

- `readonly [3, 4]` from `as const` is not assignable to mutable `[number, number]` — callee might mutate.
- Mark tuple parameters `readonly` when callers pass `as const` tuples or you will not mutate.

**Examples:**

```typescript
function doSomething(pair: readonly [string, number]) {
  // pair[0] = "hello"; // Error
}

let point = [3, 4] as const; // readonly [3, 4]

function distanceFromOrigin([x, y]: [number, number]) {
  return Math.sqrt(x ** 2 + y ** 2);
}
// distanceFromOrigin(point); // Error

function distanceFromOrigin2([x, y]: readonly [number, number]) {
  return Math.sqrt(x ** 2 + y ** 2);
}
distanceFromOrigin2(point); // OK
```

**Sources:** [TypeScript handbook — `readonly` Tuple Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#readonly-tuple-types)

</details>
