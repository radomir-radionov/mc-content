# TypeScript — Enums

Overview on [Types — Combining — Enums](./5_ts_types_combining.md#enums). Below: numeric and string enums, `const enum`, and `as const` alternatives.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Enums](#enums)

---

## Enums

<details>
  <summary>Enums</summary>

### Enums

**Definition:**

A set of named numeric or string constants that **exist at runtime** as a JS object.

| When to use                                                                                                                                             | Notes                                                                                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Declare `enum Status { Active, Inactive }`, use `Status.Active`. Prefer `as const` objects or literal unions in modern code to avoid extra bundle size. | Enums are one of the few TS features that add runtime code. Many teams prefer `as const` objects or literal unions instead — same type safety, no extra JS output. Know they exist; don't reach for them |

**Examples:**

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

Direction.Up; // 0
Direction.Down; // 1

enum Color {
  Red,
  Green,
  Blue,
}
Color.Green; // 1 (auto-increment from 0)
Color.Green === 1; // true — numeric enums are numbers at runtime

// Custom start values — auto-increment continues
enum Dir2 {
  Up = 2,
  Down = 4,
  Left = 6,
  Right, // 7
}

// Reverse mapping (numeric enums only)
enum Dir3 {
  Up,
}
Dir3[0]; // "Up"

// String enums — no reverse mapping, better for serialization
enum Links {
  Youtube = "https://youtube.com/",
  Vk = "https://vk.com/",
}

// Literal union alternative (no runtime enum object):
type Direction2 = "Up" | "Down" | "Left" | "Right";
```

**Sources:** [TypeScript handbook — everyday-types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### React bundle size — prefer `as const` over enum

**Definition:**

Regular enums emit a runtime object. For frontend code, `as const` objects avoid extra JS:

| Aspect  | Detail                                                                                     |
| ------- | ------------------------------------------------------------------------------------------ |
| Summary | Regular enums emit a runtime object. For frontend code, `as const` objects avoid extra JS: |

Regular enums emit a runtime |

Regular enums emit a runtime object. For frontend code, `as const` objects avoid extra JS:

**Examples:**

```typescript
// enum — emits runtime object + lookup function
enum Status {
  Pending = "Pending",
}

// Preferred in React/modern code — zero runtime overhead
const Status2 = { Pending: "Pending" } as const;
type Status2 = (typeof Status2)[keyof typeof Status2];

import { useState } from "react";
const [status, setStatus] = useState(Status2.Pending);
```

**Sources:** [TypeScript handbook — everyday-types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### `const enum` — inlined at compile time

**Definition:**

// Unused const enum — compiled output may be nearly empty:

// "use strict";
// (no enum object emitted)

| Aspect  | Detail                                                      |
| ------- | ----------------------------------------------------------- |
| Summary | // Unused const enum — compiled output may be nearly empty: |

// "use strict";
// (no enum object emitted) |

**Examples:**

```typescript
const enum Links {
  Vk = "https://vk.com/",
}

const arr = [Links.Vk]; // Links.Vk replaced with literal at compile time

// Unused const enum — compiled output may be nearly empty:
// "use strict";
// (no enum object emitted)

// Used in .d.ts with isolatedModules → can break consumers — prefer regular enum or as const
```

**Sources:** [TypeScript handbook — everyday-types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### Enums

**Definition:**

Named constants that compile to a real JavaScript object (numeric or string members).

| When to use                                                                                                                                                           | Notes                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `enum Color { Red, Green }` then `Color.Red`. Numeric enums support reverse lookup; string enums don't. See Everyday Types → Enums for React/`as const` alternatives. | Modern code often prefers `as const` objects over enums to stay closer to plain JS. Numeric enums get **reverse mappings** (`Enum[0] === "A"`); string enums do not. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Numeric enums

**Definition:**

// Without initializer: Up = 0, Down = 1, ...

enum Dir { Up, Down, Left, Right }
|

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // Without initializer: Up = 0, Down = 1, ...
enum Dir { Up, Down, Left, Right }
```

```typescript
enum Direction {
  Up = 1,
  Down, // 2 — auto-increment
  Left, // 3
  Right, // 4
}

// Without initializer: Up = 0, Down = 1, ...
enum Dir {
  Up,
  Down,
  Left,
  Right,
}
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### String enums

**Definition:**

Serialize/debug better than opaque numeric values. No auto-increment.

| Aspect  | Detail                                                                |
| ------- | --------------------------------------------------------------------- |
| Summary | Serialize/debug better than opaque numeric values. No auto-increment. |

Serialize/debug better than opaque numeric values. No auto-increment.

**Examples:**

```typescript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Union enums & const enums

**Definition:**

// const enum — inlined at compile time, no runtime object

const enum Direction { Up, Down, Left, Right }
let dirs = [Direction.Up, Direction.Down]; // → [0, 1] in JS

| Operator | Meaning           |
| -------- | ----------------- |
| `A \| B` | One of A or B     |
| `A & B`  | Must satisfy both |

**How it works:**

use `keyof typeof MyEnum`, not `keyof MyEnum`.

---

**Examples:**

```typescript
// Literal enum members → enum members become types too
enum ShapeKind {
  Circle,
  Square,
}
interface Circle {
  kind: ShapeKind.Circle;
  radius: number;
}

// const enum — inlined at compile time, no runtime object
const enum Direction {
  Up,
  Down,
  Left,
  Right,
}
let dirs = [Direction.Up, Direction.Down]; // → [0, 1] in JS

// Pitfall: ambient const enums in .d.ts break isolatedModules consumers
```

```typescript
const ODirection = { Up: 0, Down: 1, Left: 2, Right: 3 } as const;
type Direction = (typeof ODirection)[keyof typeof ODirection];
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>
