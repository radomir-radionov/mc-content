# TypeScript — Namespaces and Modules

When to use ES modules vs legacy namespaces.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Namespaces and Modules](#namespaces-and-modules)

---

## Namespaces and Modules

<details>
  <summary>Namespaces and Modules</summary>

### Namespaces and Modules

**Definition:**

How TS organizes code — ES modules (files) vs legacy namespaces (global).

| When to use                                                                                | Notes                                                                                                                             |
| ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| Use `import`/`export` per file. Namespaces only for global augmentation or legacy interop. | Don't `/// <reference path="foo.ts">` when you mean `import` — references add files to compilation; imports are the module graph. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Common pitfalls

**Definition:**

// GOOD

export class Triangle {}

| Aspect                   | Detail  |
| ------------------------ | ------- |
| Summary                  | // GOOD |
| export class Triangle {} |

**How it works:** ---

**Examples:**

```typescript
// BAD — needless namespace wrapper in a module
export namespace Shapes {
  export class Triangle {}
}
// Consumer: new shapes.Shapes.Triangle() 😬

// GOOD
export class Triangle {}

// Modules can't use outFile with commonjs/umd — only amd/system
```

```typescript
// myModules.d.ts (not a module)
declare module "SomeModule" {
  export function fn(): string;
}

// myOtherModule.ts
/// <reference path="myModules.d.ts" />
import * as m from "SomeModule";
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>
