# JS — Browser

DOM, events, windows, files, network APIs from page scripts, animation, and Web Components. Each section links to a dedicated topic page.

[← Back to JS hub](./3_js.md)

### Related docs

- [JS Core](./3_js_core.md) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [HTML browser & rendering](./1_html.md) · [Web API](./6_web_api.md) · [CSS motion](./2_css_motion.md)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| **Document** | DOM tree, BOM, forms, script loading |
| **Events** | Bubbling, delegation, lifecycle |
| **Windows** | Frames, popups, `postMessage`, clickjacking |
| **Files & binary** | `Blob`, `ArrayBuffer`, `FormData`, `FileReader` |
| **Network** | `fetch`, XHR, realtime transports (see also Web API) |
| **Animation** | `requestAnimationFrame`, easing, CSS vs JS |
| **Web Components** | Custom elements, shadow DOM, slots |
| **More APIs** | Storage, CORS, observers, History, URL utilities |

### Sections

- [Document](#document)
- [Events](#events)
- [Windows & frames](#windows--frames)
- [Files & binary](#files--binary)
- [Network from JavaScript](#network-from-javascript)
- [Animation](#animation)
- [Web Components](#web-components)
- [Browser APIs (more)](#browser-apis-more)

---

<a name="document"></a>

## Document

<details>
  <summary>Document</summary>

### Document

**Definition:** The **document** is the page’s DOM tree plus browser APIs for querying, mutating, and measuring it.

**How it works:** Script loading, DOM navigation, styles, geometry, BOM, and forms live on the dedicated topic page. Shadow DOM basics are summarized there; full Web Components coverage is on [Web Components](./3_js_web_components.md).

**When to use:** Any in-page UI work before reaching for a framework.

**Full topic:** [JS Browser — Document](./3_js_browser.md)

**Sources:** [javascript.info/document](https://javascript.info/document) · [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)

</details>

<a name="events"></a>

## Events

<details>
  <summary>Events</summary>

### Events

**Definition:** User input, network, timer, and lifecycle notifications delivered through the DOM event system.

**How it works:** Registration, propagation phases, delegation, and custom events are covered on the events topic page.

**Full topic:** [JS Browser — Events](./3_js_events.md)

**Sources:** [javascript.info/events](https://javascript.info/events) · [MDN — Event](https://developer.mozilla.org/en-US/docs/Web/API/Event)

</details>

<a name="windows--frames"></a>

## Windows & frames

<details>
  <summary>Windows & frames</summary>

### Windows & frames

**Definition:** Each tab (and each `iframe`) has its own `window`. Scripts coordinate through the same-origin policy, popups, and `postMessage`.

**How it works:** Frame hierarchy (`parent`, `top`, `frames`), `window.open`, cross-window messaging, and clickjacking mitigations.

**Full topic:** [JS Browser — Windows & frames](./3_js_browser_windows.md)

**Sources:** [javascript.info/frames-and-windows](https://javascript.info/frames-and-windows) · [javascript.info/popup-windows](https://javascript.info/popup-windows) · [javascript.info/cross-window-communication](https://javascript.info/cross-window-communication)

</details>

<a name="files--binary"></a>

## Files & binary

<details>
  <summary>Files & binary</summary>

### Files & binary

**Definition:** Browser APIs for user-selected files, in-memory binary data, and multipart uploads.

**How it works:** `Blob`, `File`, `ArrayBuffer`, typed arrays, `TextEncoder` / `TextDecoder`, `FormData`, and `FileReader` (or `blob.text()` / `arrayBuffer()`).

**Full topic:** [JS Browser — Files & binary](./3_js_browser_files.md)

**Sources:** [javascript.info/binary](https://javascript.info/binary) · [javascript.info/file](https://javascript.info/file) · [javascript.info/blob](https://javascript.info/blob)

</details>

<a name="network-from-javascript"></a>

## Network from JavaScript

<details>
  <summary>Network from JavaScript</summary>

### Network from JavaScript

**Definition:** Page scripts load and send data over HTTP without a full navigation — historically AJAX / `XMLHttpRequest`, now mostly `fetch`, plus push-style transports.

**How it works:** Request/response with `fetch` and XHR; download progress via streams or XHR events; abort with `AbortController`; cross-origin reads need CORS. Realtime: long polling, SSE (`EventSource`), WebSocket. Deep HTTP semantics: [Web API — HTTP](./6_web_api_http.md).

**Full topic:** [JS Browser — Network](./3_js_browser_network.md)

**Sources:** [javascript.info/network](https://javascript.info/network) · [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

</details>

<a name="animation"></a>

## Animation

<details>
  <summary>Animation</summary>

### Animation

**Definition:** Motion on the web via CSS, JavaScript-driven styles, or canvas — timed to the display refresh when smoothness matters.

**How it works:** CSS transitions/keyframes ([CSS motion](./2_css_motion.md)), easing curves (`cubic-bezier`), and `requestAnimationFrame` loops for JS-driven animation.

**Full topic:** [JS Browser — Animation](./3_js_browser_animation.md)

**Sources:** [javascript.info/animation](https://javascript.info/animation) · [MDN — Window.requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)

</details>

<a name="web-components"></a>

## Web Components

<details>
  <summary>Web Components</summary>

### Web Components

**Definition:** Standard APIs for reusable custom elements with encapsulated shadow trees.

**How it works:** `customElements.define`, `attachShadow`, `<template>`, slots, shadow styling (`::part`, CSS variables), and event retargeting.

**Full topic:** [JS — Web Components](./3_js_web_components.md)

**Sources:** [javascript.info/web-components](https://javascript.info/web-components) · [MDN — Web components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components)

</details>

<a name="browser-apis-more"></a>

## Browser APIs (more)

<details>
  <summary>Browser APIs (more)</summary>

### Browser APIs (more)

**Definition:** Storage, security boundaries, observers, SPA history, URL helpers, and UI timing utilities beyond the document tree.

**How it works:** `localStorage` / `sessionStorage` / IndexedDB, CORS vs same-origin policy, `IntersectionObserver`, `history.pushState`, `URL` / `URLSearchParams`, debounce/throttle.

**Full topic:** [JS Browser — Additional APIs](./3_js_browser_extra.md)

**Sources:** [javascript.info/localstorage](https://javascript.info/localstorage) · [javascript.info/history-api](https://javascript.info/history-api)

</details>
