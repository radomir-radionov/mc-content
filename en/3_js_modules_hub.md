# JS — Modules

ES module scope, static `import` / `export`, and dynamic `import()`. Use the accordions below for a structured overview; each section links to a dedicated topic page for the full write-up.

[← Back to JS hub](./3_js.md)

### Related docs

- [Intl API](./3_js_extra.md) · [Language](./3_js_core.md) · [Async / Await — top-level await](./3_js_async_await.md#top-level-await) · [Node.js — Modules](./8_nodejs_modules.md) · [TypeScript — Modules](./5_ts_modules.md) · [Build — bundlers](./10_build_bundlers.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Topic              | Covers                                               |
| ------------------ | ---------------------------------------------------- |
| **Modules**        | Module scope, scripts vs modules, ESM vs CommonJS    |
| **Export/Import**  | Named and default bindings, re-export, live bindings |
| **Dynamic import** | Lazy `import()`, code splitting, conditional paths   |

### Sections

- [Modules](#modules)
- [Export and Import](#export-and-import)
- [Dynamic imports](#dynamic-imports)

---

<a name="modules"></a>

## Modules

<details>
  <summary>Modules</summary>

### Modules

**Definition:** A **module** is a unit of JavaScript with its own **top-level scope**. Bindings declared at the top level are not properties of the global object unless you explicitly attach them. The standard module system in modern code is **ECMAScript modules (ESM)** (`import` / `export`). **CommonJS** (`require` / `module.exports`) remains common in older Node packages.

**How it works:**

- **Strict mode** applies to module code automatically.
- **Classic script** (`<script>` without `type="module"`): shares the global object; `var` and function declarations can become globals.
- **Module script** (`<script type="module">`): deferred like `defer`, runs once per module URL, supports `import` / `export`, and does not add top-level declarations to `window`.
- **Evaluation:** the engine builds a **module graph** from static `import` / `export`. Each module body runs **once**; later imports reuse the same module instance and **live** export bindings.
- **Specifiers:** in the browser, relative URLs (`./app.js`) and absolute URLs work; **bare** names (`"lodash"`) need an [import map](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap) or a bundler. Node resolves bare specifiers via `node_modules`.
- **Node ESM:** `"type": "module"` in `package.json`, `.mjs` files, or configured ESM `.js`; `require()` is CommonJS and is not available inside ESM without `createRequire`.
- **Interop:** bundlers and Node can consume both ESM and CJS; default export interop rules differ — see [Node.js — Modules](./8_nodejs_modules.md).

| Classic `<script>`     | `<script type="module">`      |
| ---------------------- | ----------------------------- |
| Global scope           | Module scope                  |
| No `import`/`export`   | `import` / `export` allowed   |
| Runs immediately       | Deferred (document order)     |
| `document.write` risky | Stricter parsing (often CORS) |

**When to use:**

| Choose ESM                         | Keep CommonJS (for now)       |
| ---------------------------------- | ----------------------------- |
| New browser apps and modern Node   | Unmigrated legacy Node libs   |
| Tree-shaking with a bundler        | Sync `require` in old tooling |
| Shared code between browser + Node | Scripts that only `require()` |

Use [Dynamic imports](./3_js_dynamic_import.md) when the dependency is optional, route-based, or too heavy for the first load.

**Pluses and minuses:**

| +                                   | −                                     |
| ----------------------------------- | ------------------------------------- |
| Explicit dependencies at parse time | Cross-origin module scripts need CORS |
| No accidental globals               | No sync `require` in the browser      |
| Enables dead-code elimination       | ESM/CJS mixing needs care in Node     |

**Examples:**

```html
<script type="module">
  import { boot } from "./app.js";
  boot();
</script>
```

```js
// counter.js
let count = 0;
export function inc() {
  return ++count;
}
// main.js — both importers share one `count`
import { inc } from "./counter.js";
```

**Full topic:** [Modules](./3_js_modules.md)

**Sources:** [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) · [MDN — import maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap) · [javascript.info/modules-intro](https://javascript.info/modules-intro)

</details>

<a name="export-and-import"></a>

## Export and Import

<details>
  <summary>Export and Import</summary>

### Export and Import

**Definition:** **`export`** declares which bindings a module exposes; **`import`** binds names from another module’s specifier. Static `import` and `export` are **syntactic** — they are resolved before the module body runs and must appear at **module top level** (not inside `if`, loops, or functions).

**How it works:**

- **Named export / import:** `export const id = 1;` · `export { id };` → `import { id } from "./mod.js"`.
- **Renaming:** `export { id as userId };` · `import { id as userId } from "./mod.js"`.
- **Default export:** at most **one** `export default` per module → `import anyName from "./mod.js"` (no `{ }`).
- **Combined:** `import def, { a, b } from "./mod.js"` — default first, then named.
- **Namespace:** `import * as ns from "./mod.js"` — object whose properties are exports (`ns.default` if present).
- **Side-effect only:** `import "./init.js"` — runs the module, no bindings.
- **Re-export:** `export { a } from "./a.js";` · `export { default as B } from "./b.js";` · `export * from "./c.js"` (does not re-export default).
- **Live bindings:** `import { x }` tracks the exporter’s `let`/`const`/`function`/`class` binding; assigning to `x` in the importer is a `TypeError`. Values are not snapshots copied at import time.
- **Hoisting:** imports are evaluated first in dependency order, then the module body.
- **`import.meta`:** in modules, `import.meta.url` is the module’s URL (useful for resolving relative assets in browsers and Node ESM).

#### Common pitfalls

```js
// SyntaxError — static import is not allowed inside blocks
// if (dev) import { debug } from "./debug.js";

// Live binding — if exporter later does count++, importers see the update
import { count } from "./state.js";
```

**When to use:**

| Pattern             | Typical use                                       |
| ------------------- | ------------------------------------------------- |
| Named exports       | Utilities, multiple components, constants         |
| Default export      | One primary export (React component, plugin)      |
| Barrel (`index.js`) | Public API surface for a package folder           |
| Side-effect import  | Polyfills, CSS-in-JS registration, telemetry init |

**Pluses and minuses:**

| +                        | −                                            |
| ------------------------ | -------------------------------------------- |
| Static graph for tooling | Default vs named style needs team convention |
| Tree-shaking friendly    | Conditional deps need `import()`             |
| Clear module public API  | Cannot reassign imported bindings            |

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

**Full topic:** [Export and Import](./3_js_import_export.md)

**Sources:** [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) · [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) · [MDN — import.meta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta) · [javascript.info/import-export](https://javascript.info/import-export)

</details>

<a name="dynamic-imports"></a>

## Dynamic imports

<details>
  <summary>Dynamic imports</summary>

### Dynamic imports

**Definition:** **`import(specifier)`** is an expression that loads a module **when it runs** and returns a **Promise** that fulfills with a **module namespace object**. Unlike static `import`, it can appear in functions, branches, and in classic (non-module) scripts.

**How it works:**

- **Result:** `const m = await import("./chart.js")` — use `m.default` for default export; named exports are `m.namedExport`.
- **Timing:** download and evaluation happen at the call site; the target is not in the initial static module graph until then.
- **Specifier:** string or template literal; bundlers may still analyze some patterns for chunk splitting.
- **Browser bare imports:** `import("pkg")` throws `TypeError` unless an import map maps `"pkg"` to a URL.
- **Caching:** after the first successful load, the same module instance is reused (same as static import).
- **Errors:** network or parse failures **reject** the promise — use `try` / `catch` or `.catch()` in UI code.
- **Top-level await:** in ES modules, `await import(...)` at module top level is allowed ([Top-level await](./3_js_async_await.md#top-level-await)); do not confuse with dynamic import inside functions.
- **Bundlers:** Vite, webpack, Rollup, etc. often emit **async chunks** for each dynamic `import()` target (code splitting).

**When to use:**

| Static `import`                  | Dynamic `import()`                                 |
| -------------------------------- | -------------------------------------------------- |
| Boot-critical code               | Route-based UI (dashboard, editor, admin)          |
| Shared types/utils always needed | Locale/message packs loaded by language            |
| Declared dependency graph        | Feature flags, A/B experiments                     |
| Simpler mental model             | `click` / `idle` / `IntersectionObserver` triggers |

**Pluses and minuses:**

| +                            | −                                        |
| ---------------------------- | ---------------------------------------- |
| Smaller first payload        | Extra latency on first use of the chunk  |
| Works outside module scripts | Harder static analysis / dead-code hints |
| Pairs with `async` / `await` | Too many chunks → many small requests    |

**Examples:**

```js
button.addEventListener("click", async () => {
  const { openEditor } = await import("./editor.js");
  openEditor();
});
```

```js
// Route-style lazy load (pattern used by many frameworks)
const locale = document.documentElement.lang || "en";
const { messages } = await import(`./i18n/${locale}.js`);
```

```js
async function loadChart() {
  try {
    const chart = await import("./chart.js");
    chart.render("#app");
  } catch (err) {
    console.error("Chart failed to load", err);
  }
}
```

**Full topic:** [Dynamic imports](./3_js_dynamic_import.md)

**Sources:** [MDN — import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) · [javascript.info/modules-dynamic-imports](https://javascript.info/modules-dynamic-imports) · [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

</details>
