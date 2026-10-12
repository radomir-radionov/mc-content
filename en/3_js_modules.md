# JS — Modules

How ECMAScript splits code into files with their own scope — module scripts in the browser and ESM in Node.

[← Modules hub](./3_js_modules_hub.md) · [JS hub](./3_js.md)

### Related docs

- [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Language](./3_js_core.md) · [Async / Await](./3_js_async_await.md#top-level-await) · [Node.js — Modules](./8_nodejs_modules.md) · [TypeScript — Modules](./5_ts_modules.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area              | Topics                                                 |
| ----------------- | ------------------------------------------------------ |
| **Module system** | Scope, scripts vs modules, evaluation, ESM vs CommonJS |
| **Runtime**       | `<script type="module">`, strict mode, CORS, bundlers  |

### Sections

- [Modules](#modules)
- [Interview](./3_js_interview.md)

Sibling Modules topics: [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md).

<a name="modules"></a>

## Modules

<details>
  <summary>Modules</summary>

### Modules

**Definition:** A **module** is a file (or script) treated as its own **module scope**: top-level bindings are not globals unless explicitly exported. Modern JavaScript uses **ES modules (ESM)** with static `import` / `export`. Node and older codebases may still use **CommonJS** (`require` / `module.exports`).

**How it works:**

- **Module scope:** `let`, `const`, `function`, and `class` at the top level stay private to the file unless exported.
- **Strict mode:** module code runs in strict mode automatically (no `"use strict"` needed).
- **Scripts vs modules:** a classic `<script>` shares the global object; `<script type="module">` is deferred, runs in module scope, and can use `import` / `export`.
- **Evaluation:** module body runs **once** on first load; later imports reuse the same bindings (live exports — see [Export and Import](./3_js_import_export.md#export-and-import)).
- **Graph:** static `import` statements form a dependency graph resolved at parse time; the loader fetches and evaluates in dependency order.
- **Browser:** cross-origin module scripts need CORS; bare specifiers like `"lodash"` need an import map or a bundler.
- **Node:** `.mjs`, `"type": "module"` in `package.json`, or `.js` with ESM when configured; `require` is CommonJS.
- **Import maps:** in HTML, `<script type="importmap">` maps bare specifiers to URLs for native browser ESM without a bundler ([MDN — import maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap)).

**When to use:**

| ESM (modules)                     | CommonJS (`require`)          |
| --------------------------------- | ----------------------------- |
| New browser and Node applications | Legacy Node-only codebases    |
| Bundled front-end                 | Some tooling still CJS-only   |
| Static graph for tree-shaking     | Sync config loads in old Node |

Use [Dynamic imports](./3_js_dynamic_import.md) when loading must be conditional or lazy.

**Pluses and minuses:**

| +                          | −                                           |
| -------------------------- | ------------------------------------------- |
| Clear dependency graph     | CORS on cross-origin scripts in the browser |
| No accidental globals      | No synchronous `require` in the browser     |
| Tree-shaking with bundlers | ESM/CJS interop differs in Node             |

**Examples:**

```html
<script type="module">
  import { init } from "./app.js";
  init();
</script>
```

```js
// app.js — top-level bindings are module-local
const secret = 42;
export function init() {
  console.log(secret);
}
```

**Sources:** [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) · [javascript.info/modules-intro](https://javascript.info/modules-intro)

</details>
