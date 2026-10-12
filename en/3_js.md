# JavaScript — hub

ECMAScript language, object model, browser runtime (DOM/BOM/events), and asynchronous programming. Theory lives in the Core, Browser, and Async topics. Interview questions live in [Interview](./3_js_interview.md).

## Resources & links

<details>
  <summary>Primary references</summary>

- [Modern JavaScript Tutorial (javascript.info)](https://learn.javascript.ru/)
- [MDN — JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [ECMA-262 specification](https://tc39.es/ecma262/)
- [V8 engine blog](https://v8.dev/)
- [Modern JS Cheatsheet](https://github.com/mbeaudru/modern-js-cheatsheet)
- [33 JavaScript concepts](https://github.com/leonardomso/33-js-concepts)
- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)
- [WTFJS](https://github.com/denysdovhan/wtfjs)
- [JavaScript design patterns](https://refactoring.guru/design-patterns/catalog)
- [Doka — OOP](https://doka.guide/tools/oop/)
- [Doka — how memory works](https://doka.guide/js/how-the-memory-works/)
- [How does JavaScript work (ByteByteGo)](https://blog.bytebytego.com/p/ep106-how-does-javascript-work)

</details>

<details>
  <summary>Tools & practice</summary>

### Tools & practice

- [JSV9000 — Event loop visualizer](https://www.jsv9000.app/)
- [Loupe — Event loop](http://latentflip.com/loupe/)
- [AST Explorer](https://astexplorer.net/)
- [Codewars](https://www.codewars.com/dashboard)
- [Best of JS projects](https://bestofjs.org/projects?sort=total&page=2)

</details>

<details>
  <summary>Related docs in this repo</summary>

### Related docs in this repo

- [HTML — browser & rendering](1_html.md)
- [CSS](2_css.md)
- [React](4_react.md)
- [TypeScript](5_ts.md)
- [Web API — HTTP, storage, security](6_web_api.md)
- [Node.js](8_nodejs.md)
- [DB](9_database.md)
- [Interview hub](0_interview.md) · [JS — Interview](./3_js_interview.md)

</details>

---

## JavaScript — overview

Covers the ECMAScript language, object model, browser runtime (DOM/BOM/events), and asynchronous programming.

### Main concepts

| Area           | Topics                                                                                                                                                                                                                                                                                                                                                                           |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Core**       | [Hub](./3_js_core_hub.md) — [Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Arrays](./3_js_array.md) · [Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md)         |
| **Modules**    | [Modules](./3_js_modules.md), [export / import](./3_js_import_export.md), [dynamic `import()`](./3_js_dynamic_import.md) → [additional](./3_js_extra.md)                                                                                                                                                                                                                         |
| **Browser**    | [Hub](./3_js_browser_hub.md) — [Document](./3_js_browser.md), [Events](./3_js_events.md), [Windows](./3_js_browser_windows.md), [Files](./3_js_browser_files.md), [Network](./3_js_browser_network.md), [Animation](./3_js_browser_animation.md), [Web Components](./3_js_web_components.md), [More APIs](./3_js_browser_extra.md) · [Web API](./6_web_api_core.md#uri-url--urn) |
| **Async**      | [Event loop](./3_js_async_event_loop.md), [callbacks](./3_js_async_callbacks.md), [promises](./3_js_async_promises.md), [`async/await`](./3_js_async_await.md), [APIs](./3_js_async_apis.md) · [hub](./3_js_async.md)                                                                                                                                                            |
| **Additional** | `Intl`, RegExp, Signals (TC39), `eval` → [additional](./3_js_extra.md)                                                                                                                                                                                                                                                                                                           |

### Navigation

| Part 1                                                                                                                   | Part 2                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| [Core — language](./3_js_core.md) · [variables](./3_js_variables.md) · [data types](./3_js_data_types.md)                | [Core — data structures](./3_js_data_structures.md) · [operators](./3_js_operators.md) · [loops](./3_js_loops.md)                      |
| [Core — array](./3_js_array.md) · [functions](./3_js_functions.md) (incl. [closures](./3_js_functions.md#closures))      | [Core — objects](./3_js_objects.md) · [classes](./3_js_classes.md)                                                                     |
| [Modules](./3_js_modules.md) · [export / import](./3_js_import_export.md) · [dynamic import](./3_js_dynamic_import.md)   | [Browser — hub](./3_js_browser_hub.md) · [Document](./3_js_browser.md) · [Events](./3_js_events.md)                                    |
| [Browser — Windows](./3_js_browser_windows.md) · [Files](./3_js_browser_files.md) · [Network](./3_js_browser_network.md) | [Browser — Animation](./3_js_browser_animation.md) · [Web Components](./3_js_web_components.md) · [More APIs](./3_js_browser_extra.md) |
| [Async — event loop](./3_js_async_event_loop.md)                                                                         |                                                                                                                                        |
| [Async — callbacks](./3_js_async_callbacks.md)                                                                           | [Async — promises](./3_js_async_promises.md)                                                                                           |
| [Async — await](./3_js_async_await.md)                                                                                   | [Async — APIs](./3_js_async_apis.md)                                                                                                   |
| [Additional](./3_js_extra.md)                                                                                            | [Interview — questions](./3_js_interview.md)                                                                                           |
| [Interview hub](./0_interview.md)                                                                                        |                                                                                                                                        |

### Topic template

Every major topic in the sub-files follows:

**Definition** → **Pros / Cons** → **When to use** → **Example** → **Notes** → **Source** (javascript.info).
