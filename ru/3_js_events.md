# JS Browser — Events

События браузера: handlers, phases, delegation, UI input, forms, and loading.

[ ← Назад в JS-хаб](./3_js.md)

### Related docs

- [Document](./3_js_browser.md) · [Browser additional](./3_js_browser_extra.md) · [JS Interview](./3_js_interview.md)
- [JS Core](./3_js_core.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Async](./3_js_async.md)
- [HTML](./1_html.md) · [Web API](./6_web_api.md)

### Main concepts

| Area       | Topics                                                        |
| ---------- | ------------------------------------------------------------- |
| **Basics** | Intro, bubbling, delegation, defaults, custom events          |
| **UI**     | Mouse, pointer, keyboard, scroll                              |
| **Forms**  | Properties, focus, input/change, submit                       |
| **Load**   | Page lifecycle, script timing, resource `load` / `error`        |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Events](#events) — **Event basics** · **UI events** · **Forms** · **Document loading**
- [Interview](./3_js_interview.md)

---

<a name="events"></a>

## Events

<details>
  <summary>Introduction to browser events</summary>

### Introduction to browser events

**Definition:** Browser UI is event-driven. An `EventTarget` (`Element`, `document`, `window`) receives named events; you subscribe with `addEventListener` and unsubscribe with `removeEventListener` using the **same function reference**.

**How it works:**

- `addEventListener(type, listener, options?)` — `type` is case-sensitive (`"click"`). The third argument may be `{ capture, once, passive }` or a boolean (`true` = capture).
- `once: true` removes the listener after the first call. `passive: true` promises the listener will not call `preventDefault()` (used for scroll/touch performance).
- Prefer `addEventListener` over `elem.onclick = ...` — assignment keeps only one handler and overwrites the previous one.
- `this` inside a classic-function listener is the element the listener is attached to (`currentTarget`). Arrow functions do not rebind `this`.

**When to use:** Clicks, input, submit, keyboard, load. Skip property handlers (`onclick`) when you need more than one listener or `{ once, capture, passive }`.

**Pluses and minuses:**

| +                                       | −                                         |
| --------------------------------------- | ----------------------------------------- |
| Many listeners per type; options object | Must keep the same function to remove     |
| Works on any `EventTarget`              | Forgetting `removeEventListener` can leak |

**Examples:**

```js
function onSave(e) {
  console.log(e.type, e.currentTarget);
}

button.addEventListener("click", onSave);
button.addEventListener("click", onSave, { once: true });
button.removeEventListener("click", onSave);
```

**Thoughts:** One named function + `addEventListener`/`removeEventListener` is the interview-safe pattern.

**Sources:** [MDN EventTarget.addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) · [javascript.info/introduction-browser-events](https://javascript.info/introduction-browser-events)

</details>

<details>
  <summary>Bubbling and capturing</summary>

### Bubbling and capturing

**Definition:** A DOM event travels in three phases: **capturing** (root → target), **at target**, then **bubbling** (target → root) when `event.bubbles` is true. `event.eventPhase` is `CAPTURING_PHASE` (1), `AT_TARGET` (2), or `BUBBLING_PHASE` (3).

**How it works:**

- Capture listeners run on the way **down** (`addEventListener(type, fn, true)` or `{ capture: true }`). Bubble listeners run on the way **up** (default).
- `event.target` is the node where the event started. `event.currentTarget` is the node whose listener is running now.
- `stopPropagation()` stops further capturing/bubbling. It does **not** cancel default actions — use `preventDefault()` (and the listener must not be `{ passive: true }`).
- `stopImmediatePropagation()` also skips remaining listeners on the **same** node.
- If `bubbles` is false, the flow ends after the target phase.

**When to use:** Capture for “run before children” (e.g. focus). `preventDefault()` on `submit`/`click` to override native behavior. Avoid `stopPropagation()` unless you must isolate a widget.

**Pluses and minuses:**

| +                                       | −                                                       |
| --------------------------------------- | ------------------------------------------------------- |
| One model for nested UI                 | `stopPropagation` hides bugs and breaks other listeners |
| `target` vs `currentTarget` is explicit | Capture + bubble on the same node can surprise          |

**Examples:**

```js
parent.addEventListener("click", (e) => {
  console.log(e.target.id, e.currentTarget.id, e.eventPhase);
});

link.addEventListener("click", (e) => {
  e.preventDefault();
});
```

**Thoughts:** Default is bubble. Capture is the exception you name in the third argument.

**Sources:** [MDN Event.eventPhase](https://developer.mozilla.org/en-US/docs/Web/API/Event/eventPhase) · [javascript.info/bubbling-and-capturing](https://javascript.info/bubbling-and-capturing)

</details>

<details>
  <summary>Event delegation</summary>

### Event delegation

**Definition:** **Delegation** attaches **one** listener to a common ancestor. Inside the handler, `event.target` is the origin node; `event.currentTarget` is the ancestor. Use `closest(selector)` so clicks on inner text/icons still match the intended control.

**How it works:**

- Bubbling delivers child events to the parent, so dynamically added children are handled without re-binding.
- Check the real control: `const item = e.target.closest("li[data-id]")`. If `!item` or `!e.currentTarget.contains(item)`, ignore.
- Do not style `e.target` blindly — it may be a nested `<span>`, not the `<li>`.
- Pair with `data-*` attributes for ids instead of inline `onclick`.

**When to use:** Lists, tables, menus, any UI that adds/removes rows. Direct listeners on each row when the set is small and static.

**Pluses and minuses:**

| +                                    | −                                         |
| ------------------------------------ | ----------------------------------------- |
| One listener; works for future nodes | Need `closest` / ignore unmatched clicks  |
| Less memory than N row listeners     | Nested widgets may need `stopPropagation` |

**Examples:**

```js
list.addEventListener("click", (e) => {
  const item = e.target.closest("li[data-id]");
  if (!item || !list.contains(item)) return;
  item.remove();
});
```

**Thoughts:** Interview answer: “listen on the parent, identify the child with `closest`.”

**See also:** [DOM navigation — `closest`](./3_js_browser.md#dom-navigation)

**Sources:** [javascript.info/event-delegation](https://javascript.info/event-delegation)

</details>

<details>
  <summary>Browser default actions</summary>

### Browser default actions

**Definition:** Many events trigger a built-in browser action (link navigation, form submit, text selection, context menu). Cancel it with `event.preventDefault()` — or `return false` only from `on*` property/attribute handlers.

**How it works:**

- `preventDefault()` works if the event is `cancelable` and the listener is not `{ passive: true }`.
- `event.defaultPrevented` is `true` after cancel — prefer checking this over `stopPropagation()` so outer handlers (analytics, document menus) still see the event.
- `passive: true` tells the browser you will not cancel (smoother scroll/touch); default is often passive for `touchstart`/`touchmove` in Chrome/Firefox.
- Preventing an early event can suppress a follow-up (e.g. cancel `mousedown` → no focus).

**When to use:** Custom SPA links, form validation before submit, custom context menus, drag without native image drag.

**Examples:**

```js
menu.addEventListener("click", (e) => {
  const a = e.target.closest("a");
  if (!a) return;
  e.preventDefault();
  route(a.getAttribute("href"));
});
```

**Thoughts:** `stopPropagation` ≠ `preventDefault`. Cancel the default; let the event bubble unless you have a strong reason not to.

**Sources:** [javascript.info/default-browser-action](https://javascript.info/default-browser-action)

</details>

<details>
  <summary>Dispatching custom events</summary>

### Dispatching custom events

**Definition:** `CustomEvent` is an `Event` you create and `dispatchEvent` yourself. The optional `detail` payload is any value (often an object) read as `event.detail`.

**How it works:**

- `new CustomEvent(type, { detail, bubbles, cancelable, composed })`. Event names are case-sensitive.
- `dispatchEvent` runs listeners **synchronously** with the same capturing/bubbling rules as native events.
- Default `bubbles` is `false` — ancestors will not hear it unless you set `bubbles: true`.
- `composed: true` lets the event cross shadow DOM (web components).

**When to use:** Decouple widgets (`"cart:add"`, `"modal:close"`) without a global event bus. Skip for app-wide state — a store or callback is clearer.

**Examples:**

```js
item.addEventListener("cart:add", (e) => {
  console.log(e.detail.id);
});

item.dispatchEvent(
  new CustomEvent("cart:add", {
    bubbles: true,
    detail: { id: "sku-1" },
  }),
);
```

**Sources:** [javascript.info/dispatch-events](https://javascript.info/dispatch-events)

</details>

<details>
  <summary>Mouse events</summary>

### Mouse events

**Definition:** Mouse input uses a small family of events: press/release, click, movement, and context menu.

**How it works:**

- Typical sequence: `mousedown` → `mouseup` → `click` (primary button). Also `dblclick`, `contextmenu`, `mousemove`.
- Modifiers: `ctrlKey` / `metaKey` (Mac Cmd) / `shiftKey` / `altKey`.
- Coordinates: `clientX/Y` (viewport), `pageX/Y` (document). `event.button`: 0 left, 1 middle, 2 right.

**When to use:** Buttons and links (`click`); drawing (`mousemove`); right-click menus (`contextmenu`).

**Examples:**

```js
box.addEventListener("click", (e) => {
  console.log(e.button, e.clientX, e.clientY);
});
```

**See also:** [Mouseover and mouseenter](#mouseover-and-mouseenter) · [Mouse drag and drop](#mouse-drag-and-drop)

**Sources:** [javascript.info/mouse-events-basics](https://javascript.info/mouse-events-basics)

</details>

<details>
  <summary>Mouseover and mouseenter</summary>

### Mouseover and mouseenter

**Definition:** Hover detection can use bubbling `mouseover`/`mouseout` or non-bubbling `mouseenter`/`mouseleave`.

**How it works:**

- `mouseover` / `mouseout` bubble and fire when the pointer crosses **child** boundaries (noisy on nested markup).
- `mouseenter` / `mouseleave` fire when entering/leaving the element as a whole — better for “hover this card”.
- `relatedTarget` on `mouseover`/`mouseout` is the other element involved in the transition.

**When to use:** Tooltips and highlight on a container → `mouseenter`/`mouseleave`. Tracking every nested hit → `mouseover` (rare).

**Examples:**

```js
card.addEventListener("mouseenter", () => card.classList.add("hover"));
card.addEventListener("mouseleave", () => card.classList.remove("hover"));
```

**Sources:** [javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave](https://javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave)

</details>

<details>
  <summary>Mouse drag and drop</summary>

### Mouse drag and drop

**Definition:** Custom drag UIs listen for pointer/mouse down, move, and up — often with capture — instead of the HTML5 drag-and-drop API alone.

**How it works:**

- `pointerdown` → track `pointermove` → `pointerup` (or the mouse equivalents).
- Store offset from pointer to element corner (`shiftX`/`shiftY`).
- Cancel native image drag (`ondragstart = () => false`); use `touch-action: none` to reduce `pointercancel`.
- Hit-test drop targets with `document.elementFromPoint` (hide the dragged ghost first if needed).
- `setPointerCapture(pointerId)` keeps receiving moves even if the pointer leaves the element.

**When to use:** Sliders, kanban cards, custom DnD. Native `draggable` when you need OS-level file drops.

**Examples:**

```js
thumb.onpointerdown = (e) => {
  thumb.setPointerCapture(e.pointerId);
  const startX = e.clientX;
  thumb.onpointermove = (ev) => {
    thumb.style.left = `${ev.clientX - startX}px`;
  };
  thumb.onpointerup = () => {
    thumb.onpointermove = null;
  };
};
```

**Sources:** [javascript.info/mouse-drag-and-drop](https://javascript.info/mouse-drag-and-drop)

</details>

<details>
  <summary>Pointer events</summary>

### Pointer events

**Definition:** **Pointer Events** unify mouse, touch, and pen under `pointerdown`, `pointermove`, `pointerup`, `pointercancel`.

**How it works:**

- `pointerId` identifies the contact; `pointerType` is `"mouse"`, `"pen"`, or `"touch"`.
- `isPrimary` marks the first touch in a multi-touch gesture.
- Same coordinate and button fields as mouse events; prefer pointer APIs for new cross-device UI.

**When to use:** Any control that must work on touch and desktop with one code path.

**See also:** [Mouse drag and drop](#mouse-drag-and-drop)

**Sources:** [javascript.info/pointer-events](https://javascript.info/pointer-events)

</details>

<details>
  <summary>Keyboard events</summary>

### Keyboard events

**Definition:** `keydown` and `keyup` report physical keys and modifiers; use them for shortcuts, not for reading field text.

**How it works:**

- `event.key` = character/meaning (`"z"`, `"Enter"`); `event.code` = physical key (`"KeyZ"`) — layout-independent for hotkeys.
- Auto-repeat sets `event.repeat` on held keys.
- Filter shortcuts with `code` + `ctrlKey || metaKey`. Call `preventDefault()` when overriding browser shortcuts.
- Do **not** validate typed text with keyboard alone — paste and speech skip keys; use `input` on fields.

**When to use:** Undo/redo, save, modal Escape. Field value → [change, input, and clipboard events](#change-input-and-clipboard-events).

**Examples:**

```js
document.addEventListener("keydown", (e) => {
  if (e.code === "KeyZ" && (e.ctrlKey || e.metaKey)) {
    e.preventDefault();
    undo();
  }
});
```

**Sources:** [javascript.info/keyboard-events](https://javascript.info/keyboard-events)

</details>

<details>
  <summary>Scrolling</summary>

### Scrolling

**Definition:** The `scroll` event fires on scrollable elements and the document **after** the scroll position changes.

**How it works:**

- You cannot `preventDefault()` on `scroll` — block scrolling with CSS (`overflow: hidden`) or cancel the causing key/gesture earlier.
- Read position with `element.scrollTop` / `window.scrollY`. Geometry: [Element size and scroll](./3_js_browser.md#element-size-and-scroll).
- Throttle heavy scroll handlers; consider `IntersectionObserver` instead of scroll listeners for visibility.

**When to use:** Sticky headers, infinite scroll, scroll progress. Not for blocking scroll at the last moment.

**Sources:** [javascript.info/onscroll](https://javascript.info/onscroll)

</details>

<details>
  <summary>Form properties and methods</summary>

### Form properties and methods

**Definition:** Forms expose collections and methods for navigating controls and submitting without a full page reload.

**How it works:**

- `document.forms[name]` / `form.elements[name]` — live collections; each control has `.form` back to its `<form>`.
- Use `.value` on inputs and `<textarea>` — **not** `innerHTML` (especially for textarea).
- `<select>`: `value`, `selectedIndex`, `option.selected`; create options with `new Option(text, value, defaultSelected, selected)`.
- `.submit()` sends the form **without** firing `submit` or running constraint validation; `.requestSubmit()` does both.

**When to use:** Read/write control state; programmatic submit with validation → `requestSubmit`.

**See also:** [Form submit](#form-submit) · [Searching elements — `.forms`](./3_js_browser.md#searching-elements)

**Sources:** [javascript.info/form-elements](https://javascript.info/form-elements)

</details>

<details>
  <summary>Focus and blur</summary>

### Focus and blur

**Definition:** Focus moves between focusable elements; `focus` / `blur` do not bubble, but `focusin` / `focusout` do.

**How it works:**

- `element.focus()` / `element.blur()` — programmatic focus.
- `tabindex="0"` — tab order; `tabindex="-1"` — programmatic only.
- `document.activeElement` — currently focused node.
- Listen on a container with `focusin`/`focusout` for delegation-style focus tracking.

**When to use:** Trap focus in modals; highlight focused row; restore focus after closing a dialog.

**Sources:** [javascript.info/focus-blur](https://javascript.info/focus-blur)

</details>

<details>
  <summary>change, input, and clipboard events</summary>

### change, input, and clipboard events

**Definition:** Field value changes fire `input` and `change`; clipboard actions fire `cut`, `copy`, and `paste`.

**How it works:**

- `input` — every value change (typing, paste, speech, drag text in).
- `change` — when editing **finishes** (blur for text fields; immediately for checkbox/radio/select).
- `cut` / `copy` / `paste` — `ClipboardEvent`; `preventDefault()` can block the action.

**When to use:** Live search/filter → `input`. Submit-on-blur validation → `change`. Custom paste handling → `paste`.

**Examples:**

```js
input.addEventListener("input", () => filterList(input.value));
select.addEventListener("change", () => loadChart(select.value));
```

**Sources:** [javascript.info/events-change-input](https://javascript.info/events-change-input)

</details>

<details>
  <summary>Form submit</summary>

### Form submit

**Definition:** Submitting a form fires a cancelable `submit` event on the `<form>` before navigation.

**How it works:**

- Activate via submit button, `Enter` in a field, or `form.requestSubmit()`.
- `form.addEventListener("submit", (e) => { e.preventDefault(); … })` for SPA / `fetch` submit.
- `form.submit()` bypasses the `submit` event and HTML5 validation — prefer `requestSubmit()`.

**When to use:** Always intercept `submit` for client-side apps; call `preventDefault()` then send with `fetch` or a router.

**Examples:**

```js
form.addEventListener("submit", async (e) => {
  e.preventDefault();
  await fetch("/api/save", { method: "POST", body: new FormData(form) });
});
```

**Sources:** [javascript.info/forms-submit](https://javascript.info/forms-submit)

</details>

<details>
  <summary>Page lifecycle events</summary>

### Page lifecycle events

**Definition:** The browser fires lifecycle events as HTML is parsed, resources finish loading, and the user leaves the page.

| Event              | Fires when                                     | Target     |
| ------------------ | ---------------------------------------------- | ---------- |
| `DOMContentLoaded` | HTML parsed, DOM ready (images may still load) | `document` |
| `load`             | All resources loaded (images, styles, scripts) | `window`   |
| `beforeunload`     | User is leaving the page                       | `window`   |
| `unload`           | Page is being unloaded                         | `window`   |

**How it works:**

- `DOMContentLoaded` waits for classic/deferred scripts; `async` scripts do not block it. See [Script async and defer](#script-async-and-defer).
- `document.readyState`: `"loading"` → `"interactive"` → `"complete"`.
- `beforeunload`: set `event.returnValue = ""` for a leave confirm — custom message text is ignored by browsers.
- `unload`: only fire-and-forget work (`navigator.sendBeacon`).

**Examples:**

```js
document.addEventListener("DOMContentLoaded", () => initApp());
window.addEventListener("load", () => hideLoader());
window.addEventListener("beforeunload", (e) => {
  if (formDirty) e.returnValue = "";
});
```

**Sources:** [javascript.info/onload-ondomcontentloaded](https://javascript.info/onload-ondomcontentloaded)

</details>

<details>
  <summary>Script async and defer</summary>

### Script async and defer

**Definition:** `async` and `defer` on `<script>` change **when** external scripts run relative to HTML parsing and `DOMContentLoaded`.

**How it works:**

| Attribute       | Blocks parse? | Order              | vs `DOMContentLoaded`      |
| --------------- | ------------- | ------------------ | -------------------------- |
| _(none)_        | yes           | document order     | inline blocks parse        |
| `defer`         | no (fetch)    | document order     | runs before DCL            |
| `async`         | no (fetch)    | first-ready-first  | may run before or after    |
| `type="module"` | no            | defer-like default | typically before DCL       |

- Dynamically inserted `<script>` behaves like `async` unless `script.async = false`.

**When to use:** App bundle → `defer`; independent tags → `async`. Full tag rules → [Script loading](./3_js_browser.md#script-loading).

**Sources:** [javascript.info/script-async-defer](https://javascript.info/script-async-defer)

</details>

<details>
  <summary>Resource onload and onerror</summary>

### Resource onload and onerror

**Definition:** Individual elements (`<img>`, `<script>`, `<link>`, …) fire `load` or `error` when that **resource** finishes or fails — separate from `window.load`.

**How it works:**

- Assign `img.onload` / `img.onerror` or use `addEventListener` on the element.
- `new Image()` — set handlers before `src` to avoid missing a cached load.
- Script parse errors inside a loaded file need `window.onerror` / `error` event — not `script.onerror` alone for inline exceptions.
- Cross-origin script errors stay opaque unless `crossorigin` + CORS headers.

**Examples:**

```js
const img = new Image();
img.onload = () => console.log(img.width);
img.onerror = () => console.log("failed");
img.src = "/hero.png";
```

**Sources:** [javascript.info/onload-onerror](https://javascript.info/onload-onerror)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
