# TypeScript — Modules

`import`/`export`, CommonJS, module resolution, and emit targets — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/modules.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [The Basics](./5_ts_basics.md) · [Namespaces](./5_ts_namespaces.md) · [Node.js modules](./8_nodejs_modules.md) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                                            | Covers                                            |
| -------------------------------------------------------------------------------- | ------------------------------------------------- |
| [Overview](#overview)                                                            | Handbook topic map and three module concerns      |
| [How JavaScript Modules are Defined](#how-javascript-modules-are-defined)        | Top-level `import`/`export` makes a file a module |
| [Non-modules](#non-modules)                                                      | Scripts, global scope, `export {}`                |
| [Modules in TypeScript](#modules-in-typescript)                                  | Syntax, resolution, and emit — three concerns     |
| [ES Module Syntax](#es-module-syntax)                                            | `export default`, named exports, imports          |
| [Additional Import Syntax](#additional-import-syntax)                            | Renames, namespaces, side effects, `import type`  |
| [CommonJS Syntax](#commonjs-syntax)                                              | `module.exports`, `require`                       |
| [CommonJS and ES Modules interop](#commonjs-and-es-modules-interop)              | `esModuleInterop`, default vs namespace imports   |
| [TypeScript's Module Resolution Options](#typescripts-module-resolution-options) | Classic vs Node, `paths`, `baseUrl`               |
| [TypeScript's Module Output Options](#typescripts-module-output-options)         | `module` + `target` emitted JS shapes             |
| [TypeScript namespaces](#typescript-namespaces)                                  | Legacy `namespace` — prefer ES modules            |

### Sections

- [Overview](#overview)
- [How JavaScript Modules are Defined](#how-javascript-modules-are-defined)
- [Non-modules](#non-modules)
- [Modules in TypeScript](#modules-in-typescript)
- [ES Module Syntax](#es-module-syntax)
- [Additional Import Syntax](#additional-import-syntax)
- [CommonJS Syntax](#commonjs-syntax)
- [CommonJS and ES Modules interop](#commonjs-and-es-modules-interop)
- [TypeScript's Module Resolution Options](#typescripts-module-resolution-options)
- [TypeScript's Module Output Options](#typescripts-module-output-options)
- [TypeScript namespaces](#typescript-namespaces)

### Topic groups

Handbook sections shown as grouped index on the topic page and in the sidebar:

- **How JavaScript Modules are Defined** — How JavaScript Modules are Defined, Non-modules
- **Modules in TypeScript** — Modules in TypeScript, ES Module Syntax, Additional Import Syntax, CommonJS Syntax, CommonJS and ES Modules interop, TypeScript's Module Resolution Options, TypeScript's Module Output Options, TypeScript namespaces

Top-level (no section header): Modules (overview).

---

## Overview

<details>
<summary>Modules</summary>

**Definition:** **Modules** are how JavaScript splits code into files with explicit dependencies. Modern TypeScript projects use **ES Modules** (`import` / `export`); many npm packages still ship **CommonJS** (`require` / `module.exports`).

**How it works — three concerns:**

| Concern        | Question                              | Handbook section                                                           |
| -------------- | ------------------------------------- | -------------------------------------------------------------------------- |
| **Syntax**     | How do I import and export?           | [ES Module Syntax](#es-module-syntax), [CommonJS Syntax](#commonjs-syntax) |
| **Resolution** | How does `"./math.js"` map to a file? | [Module Resolution](#typescripts-module-resolution-options)                |
| **Output**     | What JS does `tsc` emit?              | [Module Output](#typescripts-module-output-options)                        |

**Prerequisites:** [The Basics](./5_ts_basics.md) (`tsconfig`, `module`, `moduleResolution`). Legacy `namespace` details: [Namespaces](./5_ts_namespaces.md) · [Namespaces and Modules](./5_ts_namespaces_modules.md).

**Sources:** [TypeScript handbook — Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html)

</details>

---

## How JavaScript Modules are Defined

<details>
<summary>How JavaScript Modules are Defined</summary>

**Definition:** In TypeScript (and ECMAScript), any file with a top-level `import` or `export` is a **module**.

**How it works:**

- Modules run in their own scope — declarations are not global unless exported.
- To use something from another file, **export** it and **import** it.
- Files without top-level `import`/`export`/`await` are **scripts** (global scope). See [Non-modules](#non-modules).

**Sources:** [TypeScript handbook — How JavaScript Modules are Defined](https://www.typescriptlang.org/docs/handbook/2/modules.html#how-javascript-modules-are-defined)

</details>

---

## Non-modules

<details>
<summary>Non-modules</summary>

**Definition:** A file with no top-level `import`, `export`, or top-level `await` is a **script** — its types and values live in the shared global scope.

**How it works:**

- Legacy setups used multiple `<script>` tags or `outFile` to share globals.
- To force module semantics on a file with no exports yet, add:

```typescript
export {};
```

**When to use:** Almost never in modern apps — prefer real `import`/`export`. `export {}` is useful when a file must be isolated but has nothing to export yet.

**Sources:** [TypeScript handbook — Non-modules](https://www.typescriptlang.org/docs/handbook/2/modules.html#non-modules)

</details>

---

## Modules in TypeScript

<details>
<summary>Modules in TypeScript</summary>

**Definition:** Module-based TypeScript code is organized around three handbook concerns — **syntax** (how you `import`/`export`), **resolution** (how import paths map to files), and **output** (what JS `tsc` emits).

**How it works:**

| Concern        | Question                              | Section in this file                                                        |
| -------------- | ------------------------------------- | --------------------------------------------------------------------------- |
| **Syntax**     | How do I import and export?           | [ES Module Syntax](#es-module-syntax) · [CommonJS Syntax](#commonjs-syntax) |
| **Resolution** | How does `"./math.js"` map to a file? | [Module Resolution](#typescripts-module-resolution-options)                 |
| **Output**     | What JS does `tsc` emit?              | [Module Output](#typescripts-module-output-options)                         |

TypeScript extends ES modules with **type-only** imports (`import type`, inline `type` in imports) and **`import = require`** for CommonJS-aligned emit. Legacy **`namespace`** is covered at the end — prefer ES modules for app code.

**Prerequisites:** [The Basics](./5_ts_basics.md) (`tsconfig`, `module`, `moduleResolution`). Deep `namespace` topics: [Namespaces](./5_ts_namespaces.md) · [Namespaces and Modules](./5_ts_namespaces_modules.md).

**Sources:** [TypeScript handbook — Modules in TypeScript](https://www.typescriptlang.org/docs/handbook/2/modules.html#modules-in-typescript)

</details>

---

## ES Module Syntax

<details>
<summary>ES Module Syntax</summary>

**Definition:** Standard `import` / `export` syntax — the default for browser and modern Node.js.

**How it works:**

- **Default export** — one primary export per file: `export default function hello() {}`.
- **Named exports** — multiple bindings: `export const pi = 3.14`, `export function abs(n: number) {}`.
- **Import default** — `import hello from "./hello.js"`.
- **Import named** — `import { pi, abs } from "./maths.js"`.
- **Export types** — `export type Cat = { ... }`, `export interface Dog { ... }` (same syntax as values).

**Examples:**

```typescript
// hello.ts
export default function helloWorld() {
  console.log("Hello, world!");
}

// maths.ts
export const pi = 3.14;
export function absolute(num: number) {
  return num < 0 ? num * -1 : num;
}

// app.ts
import helloWorld from "./hello.js";
import { pi, absolute } from "./maths.js";
```

**Sources:** [TypeScript handbook — ES Module Syntax](https://www.typescriptlang.org/docs/handbook/2/modules.html#es-module-syntax)

</details>

---

## Additional Import Syntax

<details>
<summary>Additional Import Syntax</summary>

**Definition:** Variants of `import` for renaming, namespaces, side effects, and type-only symbols.

**How it works:**

- **Rename** — `import { pi as π } from "./maths.js"`.
- **Default + named** — `import RandomNumberGenerator, { pi as π } from "./maths.js"`.
- **Namespace import** — `import * as math from "./maths.js"`.
- **Side-effect import** — `import "./maths.js"` (runs module, binds nothing).
- **`import type`** — imports only types; erased at emit. Cannot use as values.
- **Inline `type` imports** — `import { createCatName, type Cat } from "./animal.js"`.
- **`import = require`** — TS syntax aligned with CommonJS emit: `import fs = require("fs")`.

**Examples:**

```typescript
import { pi as π } from "./maths.js";
import * as math from "./maths.js";
import "./side-effects.js";

import type { Cat, Dog } from "./animal.js";
import { createCatName, type Cat } from "./animal.js";

import fs = require("fs");
```

**Thoughts:** With `verbatimModuleSyntax`, use `import type` / `export type` explicitly for type-only symbols. See [Compiler & tsconfig](./5_ts_basics.md#compiler-tsconfig).

**Sources:** [TypeScript handbook — Additional Import Syntax](https://www.typescriptlang.org/docs/handbook/2/modules.html#additional-import-syntax)

</details>

---

## CommonJS Syntax

<details>
<summary>CommonJS Syntax</summary>

**Definition:** Node's legacy module format — most npm packages still publish CommonJS. `module.exports` and `require`.

**How it works:**

- Export via `module.exports = { ... }` or `exports.foo = ...`.
- Import via `const maths = require("./maths")` or destructuring `const { squareTwo } = require("./maths")`.
- TypeScript types `require` results loosely (`any`) unless typings or `esModuleInterop` help.

**Examples:**

```typescript
function absolute(num: number) {
  return num < 0 ? num * -1 : num;
}

module.exports = {
  pi: 3.14,
  absolute,
};

const maths = require("./maths");
const { squareTwo } = require("./maths");
```

**Sources:** [TypeScript handbook — CommonJS Syntax](https://www.typescriptlang.org/docs/handbook/2/modules.html#commonjs-syntax)

</details>

---

## CommonJS and ES Modules interop

<details>
<summary>CommonJS and ES Modules interop</summary>

**Definition:** CommonJS and ES Modules disagree on **default export** vs **module namespace** imports. TypeScript's `esModuleInterop` (and `allowSyntheticDefaultImports`) reduces friction.

**How it works:**

- Without interop, `import fs from "fs"` may error; `import * as fs from "fs"` works.
- With `esModuleInterop: true`, default imports from CJS modules are synthesized when safe.
- Modern Node can import CJS with `import foo from "./foo.cjs"` when `"moduleResolution": "NodeNext"`.

**When to use:** Enable `esModuleInterop` in most Node + bundler projects consuming mixed CJS/ESM dependencies.

**Sources:** [TypeScript handbook — CommonJS and ES Modules interop](https://www.typescriptlang.org/docs/handbook/2/modules.html#commonjs-and-es-modules-interop) · [TSConfig — esModuleInterop](https://www.typescriptlang.org/tsconfig#esModuleInterop)

</details>

---

## TypeScript's Module Resolution Options

<details>
<summary>TypeScript's Module Resolution Options</summary>

**Definition:** **Module resolution** maps the string in `import "./foo.js"` to a file on disk.

**How it works:**

- **Classic** — legacy; default when `module` is not `commonjs`.
- **Node** / **Node10** / **Node16** / **NodeNext** — follow Node resolution (+ `.ts`, `.tsx`, `.d.ts`).
- **Bundler** — for Vite, webpack, esbuild; respects `package.json` `exports`.
- Related `tsconfig`: `baseUrl`, `paths`, `rootDirs`, `moduleResolution`.

| Setting                                                | Typical use                         |
| ------------------------------------------------------ | ----------------------------------- |
| `"module": "ESNext"` + `"moduleResolution": "bundler"` | Vite, webpack, Next.js              |
| `"moduleResolution": "NodeNext"`                       | Node.js ESM with `"type": "module"` |

**Sources:** [TypeScript handbook — Module Resolution Options](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescripts-module-resolution-options) · [Module Resolution reference](https://www.typescriptlang.org/docs/handbook/modules/reference.html#the-moduleresolution-compiler-option)

</details>

---

## TypeScript's Module Output Options

<details>
<summary>TypeScript's Module Output Options</summary>

**Definition:** `target` and `module` in `tsconfig.json` control downleveling and how emitted files call each other.

**How it works:**

- **`target`** — which JS language level to emit (browser/Node baseline).
- **`module`** — module loader format in output: `ES2020`, `CommonJS`, `NodeNext`, `ESNext`, `UMD`, etc.
- Same TS source can emit ESM `import`/`export` or CJS `require`/`exports` depending on `module`.

**Examples:**

```typescript
// index.ts
import { valueOfPi } from "./constants.js";
export const twoPi = valueOfPi * 2;

// module: ES2020 → import/export preserved
// module: CommonJS → require + exports.twoPi
```

**Thoughts:** Match `module` to your runtime or bundler. Node ESM projects often use `"module": "NodeNext"` with `"moduleResolution": "NodeNext"`.

**Sources:** [TypeScript handbook — Module Output Options](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescripts-module-output-options) · [TSConfig — module](https://www.typescriptlang.org/tsconfig#module)

</details>

---

## TypeScript namespaces

<details>
<summary>TypeScript namespaces</summary>

**Definition:** TypeScript's pre-ESM `namespace` format — still used in some `.d.ts` on DefinitelyTyped, but **ES modules are recommended** for application code.

**How it works:**

- `namespace` can nest types and values for declaration merging and complex `.d.ts`.
- In module files, prefer `export` / `import` instead of wrapping exports in `namespace`.

**Examples:**

```typescript
// Prefer ES modules
export class Triangle {}

// Avoid in application modules
export namespace Shapes {
  export class Triangle {}
}
```

**See also:** [Namespaces](./5_ts_namespaces.md) · [Namespaces and Modules](./5_ts_namespaces_modules.md) · [Declaration Merging](./5_ts_declaration_merging.md)

**Sources:** [TypeScript handbook — TypeScript namespaces](https://www.typescriptlang.org/docs/handbook/2/modules.html#typescript-namespaces) · [Namespaces and Modules](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)

</details>
