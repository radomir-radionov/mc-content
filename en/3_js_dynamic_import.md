# JS — Dynamic imports

Load modules on demand with `import()` — conditional paths, code splitting, and lazy routes.

[← Modules hub](./3_js_modules_hub.md) · [JS hub](./3_js.md)

### Related docs

- [Modules hub](./3_js_modules_hub.md) · [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Async / Await](./3_js_async_await.md) · [Language](./3_js_core.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area        | Topics                                             |
| ----------- | -------------------------------------------------- |
| **Syntax**  | `import(specifier)`, promise namespace, `.default` |
| **Runtime** | Conditional load, classic scripts, bundler chunks  |

### Sections

- [Dynamic imports](#dynamic-imports)
- [Interview](./3_js_interview.md)

Sibling Modules topics: [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md).

<a name="dynamic-imports"></a>

## Dynamic imports

<details>
  <summary>Dynamic imports</summary>

### Dynamic imports

**Definition:** **`import(specifier)`** is a function-like expression that loads a module when it runs and returns a **Promise** of the module namespace object. Unlike static `import`, it can appear inside functions, branches, and classic (non-module) scripts.

**How it works:**

- **Return value:** `const ns = await import("./heavy.js")` — named exports are properties on `ns`; default export is `ns.default`.
- **Timing:** fetch and evaluation start at the call; the module is not part of the initial static graph until then.
- **Specifier:** often a relative path `./chart.js`; may be computed (`import(\`./locales/${lang}.js\`)`). In browsers, bare names (`import("lodash")`) throw unless an **import map** resolves them; Node resolves bare specifiers from `node_modules`.
- **Bundlers:** many tools emit a separate **chunk** for each dynamic `import()` target (code splitting).
- **Errors:** failed fetch or evaluation rejects the promise — handle with `try` / `catch` or `.catch()`.
- **Relation to static import:** static `import` is hoisted and always loads with the module graph; use `import()` for routes, locales, editors, or other heavy features not needed on first paint.

**When to use:**

| Static `import`            | Dynamic `import()`                    |
| -------------------------- | ------------------------------------- |
| Always-needed dependencies | Route-based or feature-flagged code   |
| Top of module file         | Inside event handlers or `async` init |
| Parse-time graph           | After user interaction or idle time   |

**Pluses and minuses:**

| +                              | −                                          |
| ------------------------------ | ------------------------------------------ |
| Smaller initial bundle         | Extra network round trip when first used   |
| Works in non-module scripts    | Harder to trace dependencies statically    |
| Natural with `async` / `await` | Overuse can fragment loading unpredictably |

**Examples:**

```js
button.addEventListener("click", async () => {
  const { openEditor } = await import("./editor.js");
  openEditor();
});
```

```js
const locale = "en";
const messages = await import(`./i18n/${locale}.js`);
```

```js
// Classic script (not type="module")
(async () => {
  const lodash = await import("https://cdn.example.com/lodash-es.js");
  lodash.default.chunk([1, 2, 3], 2);
})();
```

**Sources:** [MDN — import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) · [javascript.info/modules-dynamic-imports](https://javascript.info/modules-dynamic-imports) · [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

</details>
