# JS Browser — Document

DOM tree, querying, mutation, BOM, and script loading.

[← Back to JS hub](./3_js.md)

### Related docs

- [JS Core](./3_js_core.md) · [JS Array](./3_js_array.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Async](./3_js_async.md)
- [Browser hub](./3_js_browser_hub.md) · [Events](./3_js_events.md) · [Web Components](./3_js_web_components.md) · [Browser APIs](./3_js_browser_extra.md) · [JS Interview](./3_js_interview.md)
- [HTML browser & rendering](./1_html.md) · [Web API](./6_web_api.md) · [OS files & file systems](./8_nodejs_modules.md#files--file-systems)
- [Jest DOM tests (`jsdom`)](./7_special_testing_tooling.md#jest--first-tests-node-vs-jsdom)

### Main concepts

| Area            | Topics                                                          |
| --------------- | --------------------------------------------------------------- |
| **Environment** | Browser host, specs, script loading, BOM                        |
| **DOM**         | Tree, navigation, search, node/attribute APIs, mutation, styles |
| **Geometry**    | Element/window size, scroll, coordinates                        |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Script loading](#script-loading)
- [Document](#document) — **Environment** · **DOM tree** · **Modifying the DOM** · **Size, scroll & coordinates** · **BOM, forms & observers**
- [Events](./3_js_events.md)
- [Interview](./3_js_interview.md)

---

## Script loading

<details>
  <summary>Script loading</summary>

### Script tags & loading

**Definition:** `<script>` loads and runs JavaScript. By default it **blocks HTML parsing** until the script finishes.

**How it works:**

- Parser hits `<script>` without `async`/`defer` → stops HTML parse, fetches (if `src`), executes, then resumes parse.
- `defer`: download parallel, execute in order after HTML parsed, before `DOMContentLoaded`.
- `async`: download parallel, execute when ready (order not guaranteed), may block parse at execution.
- Dynamically created `<script>` defaults to **async** behavior; set `script.async = false` to keep insertion order (like `defer`).
- `type="module"`: deferred by default, strict mode, `import`/`export`.
- A tag with **`src` ignores any inline body** — use a second `<script>` for inline code. Do not mix `src` and content in one tag.
- Old markup: `language` is obsolete; `type="text/javascript"` is unnecessary for classic scripts. Modern `type` is for modules / import maps / data blocks.

**When to use:**

App bundle → `defer`; analytics/ads → `async`; ES app → `type="module"`.

#### Common pitfalls

Two `async` scripts depending on each other; inline script before deferred external assuming it ran; module CORS requirements for cross-origin `src`; expecting inline code inside `<script src="…">` to run (it is ignored).

**Pluses and minuses:**

| +                                     | −                                     |
| ------------------------------------- | ------------------------------------- |
| `defer` preserves order, non-blocking | Still executes on main thread         |
| `async` for independent scripts       | Order unpredictable                   |
| Modules tree-shake, strict            | Older browsers need nomodule fallback |

**Examples:**

```html
<script src="app.js" defer></script>
<script src="analytics.js" async></script>

<!-- WRONG: alert never runs -->
<script src="file.js">
  alert(1);
</script>

<!-- OK: external + separate inline -->
<script src="file.js"></script>
<script>
  alert(1);
</script>
```

**Thoughts:** Place non-critical scripts at the end of `<body>` or use `async` / `defer` to avoid blocking render.

### async & defer

| Attribute       | Download       | Execution order                           | Best for                            |
| --------------- | -------------- | ----------------------------------------- | ----------------------------------- |
| _(none)_        | blocks parsing | document order                            | legacy inline scripts               |
| `defer`         | parallel       | document order, before `DOMContentLoaded` | app bundles needing full DOM        |
| `async`         | parallel       | first-ready-first-run                     | ads, analytics, independent scripts |
| `type="module"` | parallel       | defer-like, strict mode by default        | ES modules                          |

**Examples:**

```html
<script src="a.js" defer></script>
<script src="b.js" defer></script>
<script src="tracker.js" async></script>
```

**Sources:** [javascript.info/hello-world](https://javascript.info/hello-world) · [javascript.info/script-async-defer](https://javascript.info/script-async-defer) · [MDN `<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script)

</details>

<a name="document"></a>

## Document

<details>
  <summary>Browser environment</summary>

### Browser environment

**Definition:** In the browser, JavaScript runs inside a **host environment**: the `window` global, web platform specs (HTML, DOM, CSSOM), and browser-only APIs — separate from the ECMAScript language itself.

**How it works:**

- **ECMAScript** — language (`Array`, `Promise`, syntax).
- **Web platform** — page APIs (`document`, `fetch`, `localStorage`) defined by HTML/DOM/CSSOM specs.
- **`window`** — global object in browsers; `document`, `location`, `history`, `navigator` are properties of `window`.
- **`globalThis`** — same global in any realm (browser `window`, Node `global`, workers).

| Spec layer | Examples                                   |
| ---------- | ------------------------------------------ |
| ECMAScript | `Map`, `async/await`                       |
| DOM        | `document`, `querySelector`, events        |
| HTML       | `<script>`, `<form>`, `history`            |
| CSSOM      | `document.styleSheets`, `getComputedStyle` |

**When to use:**

Know which APIs are language vs browser; feature-detect host APIs before calling them in shared isomorphic code.

#### Common pitfalls

Assuming every global exists in Node; treating BOM objects as part of ECMAScript; mixing up `window` vs `document` entry points.

**Pluses and minuses:**

| +                                      | −                                           |
| -------------------------------------- | ------------------------------------------- |
| One `window` entry for browser APIs    | Not portable to non-browser hosts           |
| Specs document cross-browser contracts | Vendor prefixes / legacy quirks still exist |

**Examples:**

```js
globalThis === window; // true in a browser page
window.document === document;
typeof fetch; // "function" in modern browsers, undefined in older Node without polyfill
```

**See also:** [BOM](#bom) · [Script loading](#script-loading)

**Sources:** [javascript.info/browser-environment](https://javascript.info/browser-environment) · [MDN Window](https://developer.mozilla.org/en-US/docs/Web/API/Window)

</details>

<details>
  <summary>DOM tree</summary>

### DOM tree

**Definition:**

The DOM is a live **node tree** the browser builds from HTML/XML. JS reads and mutates it through `document` — it is not the source file.

**Sequence** (parse → mutate):

```
1. Parse    HTML → node tree (`document` root)
2. Traverse parent/child/sibling links or search APIs
3. Read     node type, tag, contents
4. Mutate   createElement / append / writes → layout/paint
```

`window` → `document` → `<html>` (`document.documentElement`). Tags, text, and comments are all nodes.

| Node     | `nodeType`          | Example         |
| -------- | ------------------- | --------------- |
| Document | `DOCUMENT_NODE` (9) | `document`      |
| Element  | `ELEMENT_NODE` (1)  | `<div>`         |
| Text     | `TEXT_NODE` (3)     | `"Hello"`       |
| Comment  | `COMMENT_NODE` (8)  | `<!-- note -->` |

**How it works:**

Writes update the live tree immediately; mixed read/write loops can layout-thrash. Live collections change as the tree changes.

**When to use:**

Mount UI; understand document structure before querying or mutating.

#### Common pitfalls

`innerHTML` + user input (XSS); assuming `firstChild` is an element; confusing the HTML file with the live tree.

**Pluses and minuses:**

| +                  | −                                    |
| ------------------ | ------------------------------------ |
| Standard live tree | Verbose vs VDOM; live lists surprise |

**Examples:**

```js
document.title = "My App";
document.documentElement === document.querySelector("html");
document.body.append(document.createElement("p"));
```

**Thoughts:** Mutate the **runtime tree**, not the HTML file.

**See also:** [DOM navigation](#dom-navigation) · [Searching elements](#searching-elements) · [Node properties](#node-properties) · [HTMLCollection vs NodeList](#htmlcollection-vs-nodelist) · [innerHTML vs outerHTML](#innerhtml-vs-outerhtml)

**Sources:** [javascript.info/dom-nodes](https://javascript.info/dom-nodes) · [MDN DOM](https://developer.mozilla.org/en-US/docs/Glossary/DOM)

### shadow dom

**Definition:** A private, isolated DOM subtree attached to a host element — separate from the page's normal (light) DOM.

**How it works:** `elem.attachShadow({ mode: "open" | "closed" })` creates a shadow root; global `querySelector` does not see inside. Full coverage: slots, styling, events, and custom elements → [Web Components](./3_js_web_components.md).

**When to use:** Web Components and embeddable widgets (framework apps usually encapsulate differently).

**Sources:** [javascript.info/shadow-dom](https://javascript.info/shadow-dom) · [JS — Web Components](./3_js_web_components.md)

### Page lifecycle events

**Definition:** `DOMContentLoaded`, `load`, `beforeunload`, and per-resource `load`/`error` are covered in [Events](./3_js_events.md#page-lifecycle-events).

**See also:** [Page lifecycle events](./3_js_events.md#page-lifecycle-events) · [Script async and defer](./3_js_events.md#script-async-and-defer) · [Resource onload and onerror](./3_js_events.md#resource-onload-and-onerror)

### Creating & appending elements

**Examples:**

```js
const li = document.createElement("li");
li.textContent = "New item";
li.classList.add("active");
document.querySelector("ul").append(li);
```

### HTMLCollection vs NodeList

|              | HTMLCollection     | NodeList                            |
| ------------ | ------------------ | ----------------------------------- |
| **Live?**    | yes (auto-updates) | usually static (`querySelectorAll`) |
| **Contains** | elements only      | any node type                       |
| **Access**   | index, name, id    | index only                          |
| **From**     | `getElementsBy*`   | `querySelectorAll`, `childNodes`    |

**Examples:**

```js
const live = document.getElementsByClassName("item"); // live
const snap = document.querySelectorAll(".item"); // static snapshot

document.querySelector(".item").remove();
live.length; // decreased
snap.length; // unchanged
```

**Note:** Neither is a real array — use `Array.from()` or spread to iterate with array methods.

### innerHTML vs outerHTML

| Property    | Returns / sets                              |
| ----------- | ------------------------------------------- |
| `innerHTML` | HTML **inside** the element (children only) |
| `outerHTML` | HTML **including** the element itself       |

**Examples:**

```html
<div id="box"><span>Hi</span></div>
```

```js
box.innerHTML; // '<span>Hi</span>'
box.outerHTML; // '<div id="box"><span>Hi</span></div>'
```

**Warning:** `innerHTML` parses HTML strings — sanitize user input to prevent XSS. Types and encoding: [Special — XSS](./7_special_security.md#xss-cross-site-scripting).

### window vs document

|            | `window`                            | `document`                                         |
| ---------- | ----------------------------------- | -------------------------------------------------- |
| **Role**   | Browser global object               | DOM entry point (child of `window`)                |
| **Access** | implicit global                     | `document` or `window.document`                    |
| **APIs**   | `alert()`, `location`, `setTimeout` | `getElementById`, `createElement`, `querySelector` |

**Examples:**

```js
window.innerWidth; // viewport size
document.querySelector("h1"); // DOM query
```

</details>

<details>
  <summary>DOM navigation</summary>

### DOM navigation

**Definition:** Traverse the DOM tree using parent/child/sibling properties and query methods.

**How it works:**

- Tree links: `parentNode`, `childNodes`, `nextSibling`, etc.; element-only variants skip text nodes.
- For search APIs see [Searching elements](#searching-elements).

| Direction   | All nodes         | Elements only            |
| ----------- | ----------------- | ------------------------ |
| Parent      | `parentNode`      | `parentElement`          |
| Children    | `childNodes`      | `children`               |
| First child | `firstChild`      | `firstElementChild`      |
| Last child  | `lastChild`       | `lastElementChild`       |
| Previous    | `previousSibling` | `previousElementSibling` |
| Next        | `nextSibling`     | `nextElementSibling`     |

**When to use:**

Walk to parent form; find next sibling element; scoped `el.querySelector('.title')`.

#### Common pitfalls

`firstChild` vs `firstElementChild`; modifying live collection while looping; assuming `children[0]` exists.

**Pluses and minuses:**

| +                                         | −                                        |
| ----------------------------------------- | ---------------------------------------- |
| Direct traversal fast for known structure | `childNodes` includes text nodes         |
| `querySelector` flexible                  | Live collections update during iteration |
| `closest()` for delegation targets        | Deep walks manual and brittle            |

**Examples:**

```js
const list = document.querySelector(".list");
list.children; // element children only
list.querySelectorAll("li.active"); // static NodeList
list.getElementsByTagName("li"); // live HTMLCollection
```

**Thoughts:** Prefer **element-only** properties (`children`, `firstElementChild`) when you do not need text nodes.

**Also:** `elem.matches(css)` — does this element match? `elem.closest(css)` — nearest ancestor (incl. self). `elemA.contains(elemB)` — is `elemB` inside `elemA`?

**See also:** [Searching elements](#searching-elements)

**Sources:** [javascript.info/dom-navigation](https://javascript.info/dom-navigation)

</details>

<details>
  <summary>Searching elements</summary>

### Searching elements

**Definition:** Find elements in the document or a subtree with `getElement*` and `querySelector*` APIs (plus related helpers on elements).

**How it works:**

| Method                        | Scope    | Returns               | Live?  |
| ----------------------------- | -------- | --------------------- | ------ |
| `getElementById(id)`          | document | element or `null`     | —      |
| `getElementsByClassName(cls)` | element  | HTMLCollection        | yes    |
| `getElementsByTagName(tag)`   | element  | HTMLCollection        | yes    |
| `querySelector(sel)`          | element  | first match or `null` | —      |
| `querySelectorAll(sel)`       | element  | NodeList              | static |

- `querySelector(All)` runs the CSS selector engine on a subtree.
- `getElementsBy*` only matches descendants and returns **live** `HTMLCollection`s.
- `dataset` maps `data-*` attributes to camelCase JS properties.
- `forms` collection indexes `<form>` elements; `requestSubmit()` fires submit event + validation.

#### `.forms` property

Returns an `HTMLCollection` of all `<form>` elements in the document.

```js
document.forms[0].submit();
```

#### `.submit()` vs `.requestSubmit()`

| Method             | Fires `submit` event | Runs validation |
| ------------------ | -------------------- | --------------- |
| `.submit()`        | no                   | no              |
| `.requestSubmit()` | yes                  | yes             |

```js
form.requestSubmit(); // preferred — triggers validation & handlers
```

#### `.dataset` property

Reads `data-*` attributes as a camelCase object.

```html
<div data-user-id="42" data-role="admin"></div>
```

```js
el.dataset.userId; // "42"
el.dataset.role; // "admin"
el.dataset.score = "100"; // sets data-score="100"
```

**When to use:**

Read `data-id` for delegation; programmatic form submit with validation; find first matching child.

#### Common pitfalls

`dataset` sets string only; `getAttribute('value')` vs `input.value` differ; multiple forms — `document.forms[name]`.

**Pluses and minuses:**

| +                                      | −                                         |
| -------------------------------------- | ----------------------------------------- |
| `dataset` clean custom data API        | All `data-*` values are strings           |
| `requestSubmit()` respects constraints | `submit()` bypasses handlers              |
| Scoped query on elements               | `getElementsBy*` live collections confuse |

**Examples:**

```js
document.getElementById("app");
document.getElementsByClassName("card");
document.querySelector(".card.active");
document.querySelectorAll("li");
```

**`querySelector` vs `getElementsBy*`:** `querySelector(All)` works on any element and accepts full CSS selectors; `getElementsBy*` only searches descendants and returns live collections.

**Sources:** [javascript.info/searching-elements-dom](https://javascript.info/searching-elements-dom)

</details>

<details>
  <summary>Node properties</summary>

### Node properties

**Definition:** Every DOM node exposes **type**, **name/tag**, and **contents** through standard properties — how you read what a node is without serializing it.

**How it works:**

| Property                  | Role                                                                       |
| ------------------------- | -------------------------------------------------------------------------- |
| `nodeType`                | Numeric kind (`1` element, `3` text, `9` document, …)                      |
| `nodeName`                | Tag name for elements (HTML: uppercase, e.g. `"DIV"`)                      |
| `tagName`                 | Same as `nodeName` for elements only                                       |
| `textContent`             | Concatenated text of node + descendants                                    |
| `innerHTML` / `outerHTML` | HTML serialization (see [innerHTML vs outerHTML](#innerhtml-vs-outerhtml)) |

| API                               | Returns                           |
| --------------------------------- | --------------------------------- |
| `children` / `nextElementSibling` | elements only                     |
| `childNodes` / `nextSibling`      | all nodes (often whitespace text) |

**When to use:**

Branch on `nodeType`; read tag via `tagName`; set plain text with `textContent` (not `innerHTML` for user strings).

#### Common pitfalls

Using `node.toString()` instead of `tagName`; `childNodes` includes whitespace; `innerHTML` on user input (XSS).

**Examples:**

```js
const app = document.getElementById("app");
app.children[0].tagName; // "P"
app.firstChild?.nodeType === Node.TEXT_NODE;
app.textContent = "Safe label";
```

**See also:** [Attributes and properties](#attributes-and-properties) · [DOM tree](#dom-tree)

**Sources:** [javascript.info/basic-dom-node-properties](https://javascript.info/basic-dom-node-properties)

</details>

<details>
  <summary>Attributes and properties</summary>

### Attributes and properties

**Definition:** HTML **attributes** (markup) and JS **properties** (object fields) usually mirror each other for standard elements, but they are not always the same value or type.

**How it works:**

|      | Attribute (`getAttribute`)  | Property (e.g. `el.id`)                                                      |
| ---- | --------------------------- | ---------------------------------------------------------------------------- |
| Type | always string               | typed (`checked` → boolean, `style` → object)                                |
| Name | case-insensitive            | case-sensitive                                                               |
| Sync | most standard attrs ↔ props | exceptions: `input.value` updates property from attr, not attr from property |

- Non-standard attrs → no property; use `getAttribute` / `setAttribute`, or `data-*` → `elem.dataset`.
- `href` attribute = as written; `.href` = absolute URL.
- Prefer DOM properties for normal work; attributes when you need the HTML-as-written value or `data-*`.

**When to use:**

Toggle `checked` / read `value` via properties; persist custom metadata with `data-*` / `dataset`; reflect ARIA in attributes when needed.

#### Common pitfalls

`getAttribute('checked')` vs `input.checked`; expecting attribute write after changing `.value`; `dataset` values are always strings.

**Examples:**

```js
const box = document.querySelector("input[type=checkbox]");
box.checked = false;
box.getAttribute("checked"); // still "checked" if it was in HTML

el.dataset.userId = "42"; // data-user-id="42"
```

**See also:** [Searching elements](#searching-elements) · [Changing the DOM](#changing-the-dom)

**Sources:** [javascript.info/dom-attributes-and-properties](https://javascript.info/dom-attributes-and-properties)

</details>

<details>
  <summary>Changing the DOM</summary>

### Changing the DOM

**Definition:** Create, insert, modify, and remove nodes to update the page dynamically.

**How it works:**

- `createElement` / `createTextNode` build nodes; insert via `append`, `before`, `after`, `replaceWith`.
- Moving existing node removes it from old parent — single parent rule.
- `innerHTML` parses HTML string; `textContent` escapes and is safe for text.

appending an existing node **moves** it (doesn't copy).

#### Legacy methods (old code)

`appendChild`, `removeChild`, `insertBefore`, `replaceChild` — still work but prefer modern methods above.

`hasAttribute`, `getAttribute`, `setAttribute`, `removeAttribute` — see [Attributes and properties](#attributes-and-properties).

**When to use:**

Build list items; replace banner; sanitize then `textContent` for user text.

#### Common pitfalls

`innerHTML` with user strings; appending in loop causes N reflows (use `DocumentFragment`); `appendChild` same as move not copy.

#### Creating nodes

**Pluses and minuses:**

| +                              | −                                      |
| ------------------------------ | -------------------------------------- |
| Modern insert methods readable | Many reflows if inserting one-by-one   |
| `replaceWith` atomic swap      | `innerHTML` XSS if untrusted           |
| `cloneNode(true)` deep copy    | Moved nodes leave old references stale |

**Examples:**

```js
const div = document.createElement("div");
const text = document.createTextNode("Hello");
const comment = document.createComment("note");
```

#### Inserting nodes

| Method                  | Inserts              |
| ----------------------- | -------------------- |
| `append(...nodes)`      | after last child     |
| `prepend(...nodes)`     | before first child   |
| `before(...nodes)`      | before the element   |
| `after(...nodes)`       | after the element    |
| `replaceWith(...nodes)` | replaces the element |

```js
parent.append(newLi);
parent.prepend(header);
item.before(divider);
item.after(footer);
```

#### Inserting HTML

| Method                            | Parses HTML?   | XSS risk                   |
| --------------------------------- | -------------- | -------------------------- |
| `textContent`                     | no (safe text) | low                        |
| `innerHTML`                       | yes            | high — sanitize user input |
| `insertAdjacentHTML(where, html)` | yes            | high                       |

```js
el.textContent = "Safe text";
el.innerHTML = "<strong>Bold</strong>"; // only trusted HTML
el.insertAdjacentHTML("beforeend", "<li>Item</li>");
```

#### Removing & cloning

```js
node.remove(); // remove from DOM
node.cloneNode(true); // deep clone (with children)
```

**Sources:** [javascript.info/modifying-document](https://javascript.info/modifying-document)

</details>

<details>
  <summary>Styles and classes</summary>

### Styles and classes

**Definition:** Change how elements look via **CSS classes** (preferred) or **inline** `style`, and read resolved styles with `getComputedStyle`.

**How it works:**

Prefer **CSS classes** over inline styles. Use `style` only for values computed at runtime (e.g. coordinates).

| API                                    | Role                                                                |
| -------------------------------------- | ------------------------------------------------------------------- |
| `className`                            | whole `class` string (replaces all)                                 |
| `classList.add/remove/toggle/contains` | one class at a time                                                 |
| `elem.style.prop`                      | inline style only (camelCase: `backgroundColor`)                    |
| `elem.style.cssText`                   | rewrite entire inline style string                                  |
| `getComputedStyle(elem)`               | resolved styles after cascade (read-only; geometry usually in `px`) |

**When to use:**

Toggle UI state with `classList`; position overlays with inline `left`/`top`; read computed margin/size when CSS sets the value.

#### Common pitfalls

Setting `style.display` and forgetting to clear it; reading geometry from `el.style` when the rule lives in a stylesheet.

**Examples:**

```js
el.classList.add("active");
el.classList.toggle("hidden");
el.style.left = `${x}px`;
el.style.display = "";
getComputedStyle(el).marginTop;
```

**See also:** [Element size and scroll](#element-size-and-scroll) · [Coordinates](#coordinates)

**Sources:** [javascript.info/styles-and-classes](https://javascript.info/styles-and-classes)

</details>

<details>
  <summary>Element size and scroll</summary>

### Element size and scroll

**Definition:** DOM elements expose `offsetWidth`, `clientWidth`, `scrollWidth`, and related properties for layout size and scrollable overflow.

**How it works:**

- `offset*` includes border; `client*` content + padding, no border/scrollbar; `scroll*` full scrollable size.
- `getBoundingClientRect()` returns a viewport-relative box; add `scrollY` for document coordinates.
- `scrollIntoView()` scrolls ancestors to reveal the element.

| Property family | Includes border | Includes scrollbar | Content only         |
| --------------- | --------------- | ------------------ | -------------------- |
| `offset*`       | yes             | no                 | no                   |
| `client*`       | no              | no                 | no                   |
| `scroll*`       | —               | —                  | full scrollable area |

**When to use:**

Sticky header offset; infinite scroll near bottom; tooltip positioning; scroll-to-section.

#### Common pitfalls

Reading layout in a write loop (thrashing); `offsetHeight` includes border, `clientHeight` does not; rect changes on scroll without re-read.

**Examples:**

```js
const el = document.querySelector(".panel");
const rect = el.getBoundingClientRect();
const belowFold = rect.top > window.innerHeight;
el.scrollIntoView({ behavior: "smooth", block: "start" });
```

**See also:** [Window size and scroll](#window-size-and-scroll) · [Coordinates](#coordinates)

**Sources:** [javascript.info/size-and-scroll](https://javascript.info/size-and-scroll)

</details>

<details>
  <summary>Window size and scroll</summary>

### Window size and scroll

**Definition:** The viewport and document expose sizes and scroll position via `window` and `document.documentElement` / `document.body`.

**How it works:**

- Prefer `document.documentElement.clientWidth/Height` for the content area (excludes scrollbar). `window.innerWidth` includes the scrollbar.
- Full document height is unreliable on one property — use `Math.max` of `body` / `documentElement` scroll/offset/client heights.
- Read scroll with `window.scrollY` / `scrollX` (aliases `pageYOffset` / `pageXOffset`).
- Scroll with `scrollTo` / `scrollBy` / `elem.scrollIntoView`.

**When to use:**

Parallax, scroll restoration, “back to top”, detecting scroll past a threshold.

#### Common pitfalls

Using `window.innerHeight` vs `clientHeight` interchangeably; assuming `document.height` exists.

**Examples:**

```js
const scrolled = window.scrollY;
window.scrollTo({ top: 0, behavior: "smooth" });
const viewportH = document.documentElement.clientHeight;
```

**See also:** [Element size and scroll](#element-size-and-scroll) · [Coordinates](#coordinates)

**Sources:** [javascript.info/size-and-scroll-window](https://javascript.info/size-and-scroll-window)

</details>

<details>
  <summary>Coordinates</summary>

### Coordinates

**Definition:** Pointer and element positions use **viewport** coords (`clientX`, `getBoundingClientRect`) or **document** coords (`pageX`, scroll-adjusted rects).

**How it works:**

- Mouse events: `clientX/Y` viewport; `pageX/Y` = client + scroll offset.
- `getBoundingClientRect()` gives element box vs viewport.
- `document.elementFromPoint(x, y)` — topmost element at a viewport point.

| Method                         | Coordinate system            |
| ------------------------------ | ---------------------------- |
| `getBoundingClientRect()`      | Viewport (changes on scroll) |
| `clientX` / `clientY` (events) | Viewport                     |
| `pageX` / `pageY`              | Document (includes scroll)   |

Convert viewport → document: add `window.scrollY` / `scrollX` to `getBoundingClientRect()` edges.

**When to use:**

Drag-drop; canvas hit testing; tooltips at the cursor.

#### Common pitfalls

Using client coords for document-positioned elements without scroll; ignoring `transform` on parent; touch `touches[0]` vs `changedTouches`.

**Examples:**

```js
document.addEventListener("click", (e) => {
  console.log("viewport:", e.clientX, e.clientY);
  console.log("document:", e.pageX, e.pageY);
});
```

**Sources:** [javascript.info/coordinates](https://javascript.info/coordinates)

</details>

<details>
  <summary>BOM</summary>

### BOM

**Definition:** Browser Object Model — browser-provided objects **outside** the document: `window`, `location`, `navigator`, `history`, `screen`, timers.

**How it works:**

- `window` is global object in browsers; `document`, `location`, `history`, `navigator` are properties.
- BOM APIs are browser-specific (not in ECMAScript spec); DOM is separate but accessed via `window.document`.

| Object      | Purpose                              |
| ----------- | ------------------------------------ |
| `window`    | Global scope, dialogs, viewport size |
| `location`  | Current URL, redirect                |
| `navigator` | Browser/OS info (`userAgent`)        |
| `history`   | Back/forward navigation              |
| `screen`    | Screen dimensions                    |

**When to use:**

Redirect via `location.href`; read `navigator.userAgent` (feature detect better); `history.pushState` for SPA.

#### Common pitfalls

Treating BOM as part of ECMAScript; `window.open` blocked by pop-up blockers; `navigator` fields spoofable.

**Pluses and minuses:**

| +                            | −                                             |
| ---------------------------- | --------------------------------------------- |
| Unified `window` entry point | Not available in Node (different globals)     |
| `location` for navigation    | Direct `location` assignment full page reload |
| `history` for SPA routing    | History state size limits                     |

**Examples:**

```js
location.href = "https://example.com";
navigator.userAgent;
history.back();
```

**See also:** [Browser environment](#browser-environment)

<a name="dom-vs-bom"></a>

### DOM vs BOM

|                 | DOM                                   | BOM                                |
| --------------- | ------------------------------------- | ---------------------------------- |
| **Scope**       | Document content (page)               | Browser environment                |
| **Entry point** | `document`                            | `window`                           |
| **Examples**    | `querySelector`, `createElement`      | `location`, `navigator`, `alert()` |
| **Spec**        | DOM spec (works outside browsers too) | HTML spec (browser-only)           |

</details>

<details>
  <summary>Text, forms, and dialogs</summary>

### innerText vs textContent

**Definition:** `textContent` and `innerText` read element text differently; forms expose live `.value`; `alert`/`confirm`/`prompt` are blocking modal dialogs.

**How it works:**

- `textContent` returns all text nodes including hidden; `innerText` layout-aware (visible text only, triggers reflow).
- Form controls use `.value` property (live state), not `innerHTML`.
- `requestSubmit()` fires submit event and runs validation; `submit()` does not.

|                  | `textContent`           | `innerText`              |
| ---------------- | ----------------------- | ------------------------ |
| **Returns**      | All text (incl. hidden) | Visible text only        |
| **Layout-aware** | no                      | yes (respects CSS)       |
| **Performance**  | faster                  | slower (triggers reflow) |
| **Use for**      | reading/writing text    | display text to user     |

**When to use:** Set label text → `textContent`; read displayed text → `innerText`; read input → `.value`; confirm delete → `confirm` or custom modal.

**Pluses and minuses:**

| +                                   | −                          |
| ----------------------------------- | -------------------------- |
| `textContent` fast and predictable  | `innerText` slower         |
| `.value` correct for inputs         | Blocking dialogs freeze UI |
| `requestSubmit()` proper validation | `alert` not stylable       |

**Examples:**

```js
el.textContent = "Hello"; // preferred for setting text
```

### Forms

Form controls, focus, value events, and submit are covered in [Events — Forms](./3_js_events.md#form-properties-and-methods).

**See also:** [Form properties and methods](./3_js_events.md#form-properties-and-methods) · [Focus and blur](./3_js_events.md#focus-and-blur) · [change, input, and clipboard events](./3_js_events.md#change-input-and-clipboard-events) · [Form submit](./3_js_events.md#form-submit)

**Sources:** [javascript.info/alert-prompt-confirm](https://javascript.info/alert-prompt-confirm)

### Dialog popups

| Method                 | Returns          |
| ---------------------- | ---------------- |
| `alert(msg)`           | `undefined`      |
| `confirm(msg)`         | `true` / `false` |
| `prompt(msg, default)` | string or `null` |

All three are **modal**: they pause script execution and block the rest of the page until dismissed. Look and position are browser-controlled.

**Common pitfalls:** `textarea.innerHTML` vs `textarea.value`; using `innerText` in hot loops; `confirm` in async code blocks main thread.

</details>

<details>
  <summary>MutationObserver</summary>

### MutationObserver

**Definition:** Built-in API to watch DOM changes and react via callback.

**How it works:**

- Observer callback receives batch of `MutationRecord`s after DOM changes in microtask.
- `observe(target, { childList, attributes, subtree, characterData })` filters what to watch.
- `disconnect()` stops observation; `takeRecords()` flushes pending.

child nodes added/removed, attribute changes, text content changes.

**When to use:**

Widget injected by ad script; contenteditable sync; attribute changes on custom element.

third-party scripts modifying DOM, lazy-loaded content, Web Components.

#### MutationObserver vs ResizeObserver

|                 | MutationObserver                               | ResizeObserver                |
| --------------- | ---------------------------------------------- | ----------------------------- |
| **Watches**     | DOM tree changes (nodes, attrs, text)          | Element size changes          |
| **When to use** | Dynamic content injection, third-party widgets | Responsive components, charts |
| **Performance** | Can fire often — narrow `subtree`/`attributes` | Fires on layout changes only  |

See also [IntersectionObserver and ResizeObserver](#intersection-resize-observer).

#### Common pitfalls

Observing entire `document.body` with `subtree: true`; infinite loop if callback mutates observed subtree; forgetting `disconnect`.

**Pluses and minuses:**

| +                                | −                                         |
| -------------------------------- | ----------------------------------------- |
| React to third-party DOM changes | Can fire very frequently                  |
| Async batched — not synchronous  | Narrow options or performance suffers     |
| No polling needed                | Not for size changes — use ResizeObserver |

**Examples:**

```js
const observer = new MutationObserver((mutations) => {
  for (const m of mutations) {
    console.log(m.type, m.target);
  }
});

observer.observe(document.querySelector("#app"), {
  childList: true,
  attributes: true,
  subtree: true,
});

observer.disconnect(); // cleanup when done
```

**Sources:** [javascript.info/mutation-observer](https://javascript.info/mutation-observer)

</details>

<details>
  <summary>Forms vs dialogs</summary>

### Form vs modal

**Definition:** Forms collect and submit structured data; modals interrupt the user flow for focused actions or confirmations.

**How it works:**

- Forms use native controls + validation API; submit navigates or is intercepted with `preventDefault` + fetch.
- Modals trap focus, block background interaction; native `<dialog>.showModal()` or custom overlay pattern.

|             | Form                           | Modal                              |
| ----------- | ------------------------------ | ---------------------------------- |
| **Purpose** | Collect & submit data          | Interrupt workflow for action/info |
| **Use for** | Registration, checkout, search | Confirmations, errors, quick input |
| **UX**      | Part of page flow              | Blocks page until dismissed        |

**When to use:**

Checkout → form; delete confirm → modal or `confirm`; search bar → inline form.

#### Common pitfalls

Modal without focus trap; nested `<form>` invalid; form in modal without proper `aria-modal`.

**Pluses and minuses:**

| +                          | −                             |
| -------------------------- | ----------------------------- |
| Semantic HTML, accessible  | Modals overused hurt UX       |
| Native validation          | Custom modals need a11y work  |
| Fits multi-field workflows | Forms heavy for single yes/no |

**Examples:**

```js
// Form submit via fetch
form.addEventListener("submit", async (e) => {
  e.preventDefault();
  await fetch("/api/signup", { method: "POST", body: new FormData(form) });
});

// Native dialog
const dlg = document.querySelector("dialog");
dlg.showModal();
dlg.close("confirmed");
```

**Sources:** [form vs modal](https://thecontentauthority.com/blog/form-vs-modal)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
