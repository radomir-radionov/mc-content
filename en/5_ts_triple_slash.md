# TypeScript — Triple-Slash Directives

Compiler directives, ambient declarations, and `declare`.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Triple-Slash Directives](#triple-slash-directives)

---

## Triple-Slash Directives

<details>
  <summary>Triple-Slash Directives</summary>

### Triple-Slash Directives

**Definition:**

Single-line `///` comments at file top that instruct the compiler.

| When to use                                                                                          | Notes                                                                                                                |
| ---------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `/// <reference path="./types.d.ts" />` — legacy; prefer `import` and `tsconfig` `references` today. | As of TS 5.5, the compiler does **not** emit handwritten triple-slash directives to output unless `preserve="true"`. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Common directives

**Definition:**

/// <amd-module name="NamedModule"/> // Name AMD module in output

|

- **`reference path`** — relative file dependency; depth-first resolution
- **`reference types`** — like `import` for declaration packages (`@types/node`); prefer `"types"` in `tsconfig.json` for `.ts` files
- **`reference lib`** — pull in DOM/ES lib slices
- **`--noResolve`** — ignores all reference directives

**How it works:**

`/// <amd-dependency path="..." />` — use `import "moduleName"` instead.

---

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | /// <amd-module name="NamedModule"/>    // Name AMD module in output
```

```typescript
/// <reference path="./utils.ts" />     // Add file to compilation (ordering)
/// <reference types="node" />          // Include @types package
/// <reference lib="es2017.string" />   // Include built-in lib (like --lib)
/// <reference no-default-lib="true"/>  // Skip default lib.d.ts

/// <amd-module name="NamedModule"/>    // Name AMD module in output
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Ambient Declarations and `declare`

**Definition:**

Type-only descriptions of code that exists elsewhere (JS libs, globals, `.d.ts` shims).

| When to use                                                                | Notes                                                                                                                      |
| -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `declare const $: JQueryStatic` or `declare module "legacy"`. Emits no JS. | `declare` does not create a value at runtime. Pair with `@types/*` packages or hand-written `.d.ts` for untyped libraries. |

**Examples:**

```typescript
// Global variable loaded via <script>
declare const jQuery: (selector: string) => unknown;

// Ambient module — shape of an untyped npm package
declare module "legacy-lib" {
  export function doSomething(x: number): string;
}

// Ambient namespace — global library
declare namespace D3 {
  export function select(selector: string): unknown;
}

// declare class/function/var — types only, no JS emit
declare function greet(name: string): void;
declare class LegacyWidget {
  constructor(el: string);
  render(): void;
}

// Asset imports — tell TS non-TS files exist (Vite/Webpack)
declare module "*.svg" {
  const content: string;
  export default content;
}
declare module "*.png" {
  const src: string;
  export default src;
}
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### `noImplicitAny` and `strictNullChecks` — interview definitions

**Definition:**

Both are enabled by `"strict": true` in `tsconfig.json`.

---

| Aspect  | Detail                                                   |
| ------- | -------------------------------------------------------- |
| Summary | Both are enabled by `"strict": true` in `tsconfig.json`. |

**`noImplicitAny`** — when TS cannot infer a type, it would default to `any`. This flag makes that an **error**, forcing explicit annotations. Prevents silent loss of type safety in function parameters and untyped variables.

**`strictNullChecks`** — by default `null` and `undefined` assign to any type. This flag makes them distinct — you must handle nullability explicitly. Catches the most common production bug class.

## Both are enabled by `"strict": true` in `tsconfig.json`.

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>
