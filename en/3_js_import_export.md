# JS — Export and Import

Static `export` and `import` syntax, re-exports, and live bindings between modules.

[← Modules hub](./3_js_modules_hub.md) · [JS hub](./3_js.md)

### Related docs

- [Modules hub](./3_js_modules_hub.md) · [Modules](./3_js_modules.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Language](./3_js_core.md) · [Node.js — Modules](./8_nodejs_modules.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area          | Topics                                                  |
| ------------- | ------------------------------------------------------- |
| **Export**    | Named, default, re-export, `export from`                |
| **Import**    | Named, default, namespace, side-effect imports          |
| **Semantics** | Live bindings, hoisting, top-level-only static `import` |

### Sections

- [Export and Import](#export-and-import)
- [Interview](./3_js_interview.md)

Sibling Modules topics: [Modules](./3_js_modules.md) · [Dynamic imports](./3_js_dynamic_import.md).

<a name="export-and-import"></a>

## Export and Import

<details>
  <summary>Export and Import</summary>

### Export and Import

**Definition:** **`export`** marks bindings other modules may import; **`import`** reads those bindings from another module’s URL or path. Static forms are resolved at parse time and must appear at the top level of the module (not inside `if`).

**How it works:**

- **Named export:** `export const x = 1;` or `export { x };` → `import { x } from "./mod.js"`.
- **Default export:** at most one per file → `export default fn;` → `import fn from "./mod.js"` (any identifier; no braces).
- **Mixed:** `import multiply, { PI, add } from "./math.js"` — default first, then named in `{ }`.
- **Re-export:** `export { x } from "./mod.js"` or `export { default as User } from "./user.js"` — aggregate public API from one entry file.
- **Namespace import:** `import * as math from "./math.js"` — module namespace object (includes `default` when present).
- **Side-effect import:** `import "./polyfills.js"` — runs the module for initialization only.
- **Live bindings:** imported `let` / `const` / `class` / `function` names track the exporter; importers cannot assign to imported bindings (read-only view).
- **Hoisting:** static `import` is evaluated before the rest of the module body runs.
- **`import.meta`:** `import.meta.url` is this module’s URL (ESM in browsers and Node).
- **Conditional loading:** use dynamic `import()` — see [Dynamic imports](./3_js_dynamic_import.md).

#### Common pitfalls

```js
// Static import cannot be inside if — use import() instead
// if (flag) import { x } from "./mod.js";

// import { x } from "./mod.js"; // x is a live binding, not a copy of a snapshot
```

**When to use:**

| Pattern            | Use case                                      |
| ------------------ | --------------------------------------------- |
| Named exports      | Multiple public APIs from one file            |
| Default export     | Single main export (component, factory)       |
| Barrel re-export   | One `index.js` for a package folder           |
| Side-effect import | Register plugins, polyfills, global CSS hooks |

**Pluses and minuses:**

| +                                | −                                     |
| -------------------------------- | ------------------------------------- |
| Explicit public surface          | Default vs named import style debates |
| Enables static analysis          | Must use `import()` for branch loads  |
| Live bindings avoid stale copies | Importers cannot reassign imports     |

**Examples:**

```js
// math.js
export const PI = 3.14;
export function add(a, b) {
  return a + b;
}
export default function multiply(a, b) {
  return a * b;
}

// app.js
import multiply, { PI, add } from "./math.js";
```

```js
// api/index.js — barrel
export { User } from "./user.js";
export { listPosts } from "./posts.js";
```

**Sources:** [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) · [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) · [MDN — import.meta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta) · [javascript.info/import-export](https://javascript.info/import-export)

</details>
