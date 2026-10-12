# TypeScript — Types — Object shapes

Naming and structuring object types — inline shapes, `interface`, `type` aliases, and when to pick each.

[← Types hub](./5_ts_types.md)

### Related docs

[Types hub](./5_ts_types.md) · [Values](./5_ts_types_values.md) · [Object Types](./5_ts_objects.md) · [Combining types](./5_ts_types_combining.md)

### Main concepts

| Topic | Examples |
| ----- | -------- |
| Object types | `{ name: string }` |
| Interfaces | `interface User { id: number }` |
| Type aliases | `type Point = { x: number; y: number }` |
| Type vs Interface | merge vs unions |

### Sections

- [Object types](#object-types)
- [Interfaces](#interfaces)
- [Type aliases](#type-aliases)
- [Type vs Interface](#type-vs-interface)

---

## Object types

<details>
  <summary>Object types</summary>

**Definition:**

A value with named properties — described with an inline `{ }` type. TypeScript checks **structure**, not a class name: if the shape matches, the type matches. To **name** a shape, use an [Interface](#interfaces) or [Type alias](#type-aliases).

**How it works:**

- Inline `{ id: number; title: string }` — quick for one-off parameters and anonymous object literals.
- Extra properties on a fresh literal may trigger **excess property checks** — see [Object Types — Excess Property Checks](./5_ts_objects.md#excess-property-checks).
- Optional properties use `?`: `{ nick?: string }` means the key may be missing (not the same as “may be `undefined`” in every context — see [Object Types — Optional Properties](./5_ts_objects.md#optional-properties)).

**When to use:**

One-off argument shapes. When the same shape is reused, promote it to [Interfaces](#interfaces) or [Type aliases](#type-aliases).

**Examples:**

```typescript
function printUser(user: { name: string; age: number }) {
  console.log(user.name, user.age);
}

function setConfig(config: { theme: "light" | "dark"; debug?: boolean }) {
  // ...
}
```

**Sources:** [TypeScript handbook — Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html) · [Object Types](./5_ts_objects.md)

</details>

---

## Interfaces

<details>
  <summary>Interfaces</summary>

**Definition:**

An **`interface`** names an object type — a contract for which properties (and call/index signatures) a value must have. Any object with a matching shape is assignable; the compiler does not care about the interface name at runtime.

**How it works:**

- **Syntax:** `interface User { id: number; name: string }`.
- **Optional / readonly:** `nick?: string`, `readonly id: number` — details in [Object Types](./5_ts_objects.md).
- **Extension:** `interface Employee extends Person { employeeId: number }` copies members from another interface.
- **Classes:** `class Admin implements User { ... }` — the class must satisfy the interface shape.
- **Declaration merging:** Multiple `interface Foo { ... }` blocks with the same name merge (used for augmenting globals and some library typings). A `type` alias cannot be merged — see [Type vs Interface](#type-vs-interface).
- **Call / index signatures:** methods and dynamic keys on the same interface — [Object Types](./5_ts_objects.md).

**When to use:**

Public object contracts (component props, domain models, plugin APIs) that may grow or be augmented across files. Default handbook choice for object shapes until you need a `type`-only form.

**Examples:**

```typescript
interface Point {
  x: number;
  y: number;
}

function printPoint(p: Point) {
  console.log(p.x, p.y);
}

printPoint({ x: 3, y: 7 }); // OK — structural match

interface Named {
  name: string;
}

interface Employee extends Named {
  employeeId: number;
}

class Manager implements Employee {
  name = "Ada";
  employeeId = 1;
}
```

**Sources:** [TypeScript handbook — Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces) · [Object Types](./5_ts_objects.md)

</details>

---

## Type aliases

<details>
  <summary>Type aliases</summary>

**Definition:**

A **type alias** gives a name to any type with `type Name = ...` — object shapes, unions, tuples, primitives, function types, or combinations built with `&` and generics. The alias is **transparent**: `type UserId = string` is still `string` for assignability (not a runtime brand).

**How it works:**

- **Syntax:** `type ID = number | string`; `type Point = { x: number; y: number }`; `type Handler = (e: Event) => void`.
- **One declaration:** You cannot declare the same `type` name twice (unlike `interface`, which can merge).
- **Object shapes:** `type Point = { x: number; y: number }` is equivalent to an interface for assignability; choosing between them is covered in [Type vs Interface](#type-vs-interface).
- **Generics on aliases:** `type Box<T> = { value: T }` — same idea as generic interfaces.

**When to use:**

- Reuse a complex type (union of literals, tuple, function signature) without repeating it.
- Build helper types: `type Nullable<T> = T | null`.
- When you need a form only `type` can express (union of object variants, template literal types — see [Type Manipulation](./5_ts_type_manipulation.md)).

**Examples:**

```typescript
type UserId = string | number;

type Point = {
  x: number;
  y: number;
};

type Status = "pending" | "done" | "failed";

type Coords = [number, number];

type StringMap = Record<string, string>;

function move(pt: Point, delta: Coords): Point {
  return { x: pt.x + delta[0], y: pt.y + delta[1] };
}
```

**Sources:** [TypeScript handbook — Type Aliases](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases) · [Type vs Interface](#type-vs-interface)

</details>

---

## Type vs Interface

<details>
  <summary>Type vs Interface</summary>

**Definition:**

`interface` and `type` are two ways to **name** types. For object shapes they behave almost the same at check time (structural typing). They differ in what forms they can express and in declaration merging.

**How it works:**

|                                               | `interface`                                     | `type` (`type Name = ...`)             |
| --------------------------------------------- | ----------------------------------------------- | -------------------------------------- |
| Object property shapes                        | yes                                             | yes (`type Point = { x: number }`)     |
| Unions, primitives, tuples                    | no (use intersections / separate types)         | yes (`type Id = string \| number`)     |
| Mapped / conditional / template literal types | no                                              | yes                                    |
| Extend / combine shapes                       | `extends` (chains interfaces)                   | `&` intersection                       |
| Same name declared twice                      | merges declarations                             | error — duplicate identifier           |
| `implements` on a class                       | yes                                             | only when the alias is an object shape |
| Compiler note                                 | `extends` is often cheaper than huge `&` chains | required for non-object type forms     |

**Handbook guidance:** For object shapes, start with `interface` until you need a feature only `type` provides (union alias, tuple, utility composition). See [Interfaces](#interfaces) and [Type aliases](#type-aliases).

**When to use:**

- **`interface`** — exported props/models, libraries you may augment, inheritance with `extends` / `implements`.
- **`type`** — union of variants, function/tuple aliases, `Pick`/`Omit` compositions, branded-style helpers built from intersections.

**Examples:**

```typescript
// Equivalent object contracts
interface IUser {
  id: number;
}
type TUser = { id: number };

// Only type can name this union cleanly
type Result = { ok: true; data: string } | { ok: false; error: string };

// Interface merging — same name, members combine
interface Box {
  height: number;
  width: number;
}
interface Box {
  scale: number;
}
// Box is { height, width, scale }

// type cannot be re-opened:
// type Window = { a: 1 };
// type Window = { b: 2 }; // Error: Duplicate identifier
```

**Sources:** [TypeScript handbook — Differences Between Type Aliases and Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces) · [Interface Extension vs. Intersection](https://www.typescriptlang.org/docs/handbook/2/objects.html#interface-extension-vs-intersection)

</details>
