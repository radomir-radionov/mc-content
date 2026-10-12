# TypeScript — Namespaces

Legacy `namespace` organization and ambient declarations.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Namespaces](#namespaces)

---

## Namespaces

<details>
  <summary>Namespaces</summary>

### Namespaces

**Definition:**

Legacy way to group code in global scope: `namespace Utils { export function f() {} }`.

| When to use                                                                                           | Notes                                                                                                                                                                            |
| ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Prefer ES modules (`import`/`export`) in modern code. Namespaces still appear in older `.d.ts` files. | `module X` is legacy alias for `namespace X`. Prefer **ES modules** (`import`/`export`) in modern code — namespaces pollute the global scope and complicate dependency tracking. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Patterns

**Definition:**

Utils.getPass("Yauhen", 31); // "Yauhen31"

Utils.isEmpty(Utils.SECRET); // false

| Aspect                                | Detail                                     |
| ------------------------------------- | ------------------------------------------ |
| Summary                               | Utils.getPass("Yauhen", 31); // "Yauhen31" |
|                                       | Utils.isEmpty(Utils.SECRET); // false      |

## **Ambient namespaces** (`declare namespace D3 { ... }`) describe global libraries loaded via `<script>`.

**Examples:**

```typescript
// Legacy namespace — encapsulates globals; must export to expose
namespace Utils {
  export const SECRET = "123321";
  const PI = 3.14; // private to namespace — not accessible outside
  export const getPass = (name: string, age: number) => `${name}${age}`;
  export const isEmpty = <T>(data: T) => !data;
}

Utils.getPass("Yauhen", 31); // "Yauhen31"
Utils.isEmpty(Utils.SECRET); // false

// Modern equivalent — ES modules (no namespace wrapper needed)
// export const SECRET = "123321";
// export const getPass = ...
// import { getPass, SECRET } from "./Utils";

namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }
  export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
      return s.length === 5 && /^\d+$/.test(s);
    }
  }
}

// Alias — import q = x.y.z (not the same as import from "module")
import polygons = Shapes.Polygons;
let sq = new polygons.Square();

// Multi-file: same namespace name merges across files
// Use /// <reference path="..." /> to order compilation
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>
