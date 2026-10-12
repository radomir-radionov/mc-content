# JS Browser — Windows & frames

Tabs, `iframe`s, popups, cross-window messaging, and clickjacking defenses.

[← Back to JS hub](./3_js.md) · [Browser hub](./3_js_browser_hub.md)

### Related docs

- [Document](./3_js_browser.md) · [Events](./3_js_events.md) · [Browser APIs](./3_js_browser_extra.md)
- [Web API — security](./6_web_api_security.md) · [Clickjacking](./7_special_security.md#clickjacking)
- [JS Interview](./3_js_interview.md)

### Main concepts

| Area          | Topics                                             |
| ------------- | -------------------------------------------------- |
| **Frames**    | `iframe`, `window` hierarchy, `sandbox`            |
| **Popups**    | `window.open`, `opener`, user gesture              |
| **Messaging** | `postMessage`, `BroadcastChannel`, `storage` event |
| **Security**  | Same-origin policy, clickjacking headers           |

### Sections

- [Frames and windows](#frames-and-windows)
- [Popups and window methods](#popups-and-window-methods)
- [Cross-window communication](#cross-window-communication)
- [Clickjacking](#clickjacking)

---

<a name="frames-and-windows"></a>

## Frames and windows

<details>
  <summary>Frames and windows</summary>

### Frames and windows

**Definition:** A **browsing context** is a `window` object: one per tab, popup, or `iframe`. Nested frames form a tree linked by `parent`, `top`, and `frames`.

**How it works:**

- **`window` / `self`:** global object in a page; each frame has its own.
- **`iframe`:** embeds another document; `iframe.contentWindow` is always available; `contentDocument` only when **same-origin** (otherwise `null`).
- **Hierarchy:** `window.parent` (immediate parent), `window.top` (root), `window.frames` (collection of child windows by index/name).
- **Same-origin policy:** scripts in one frame cannot read another frame’s DOM, `localStorage`, or most properties when origins differ (scheme + host + port).
- **`sandbox` attribute:** restricts scripts, forms, top navigation, and same-origin access unless allowlisted (`allow-scripts`, `allow-same-origin`, …).
- **`window === window.top`:** `true` only in the top-level tab (not inside a nested frame).

**When to use:** Embeds (widgets, payment iframes), OAuth flows, micro-frontends, legacy “frameset” layouts (prefer flex/grid today).

#### Common pitfalls

Assuming `contentDocument` works cross-origin; forgetting `sandbox` blocks same-origin storage; `target="_blank"` without `rel="noopener"` on untrusted links (see popups).

**Pluses and minuses:**

| +                         | −                                      |
| ------------------------- | -------------------------------------- |
| Isolates third-party UI   | Cross-origin DOM access blocked        |
| `sandbox` tightens embeds | iframe layout/CLS if size not reserved |

**Examples:**

```js
const frame = document.querySelector("iframe.widget");
const childWin = frame.contentWindow;
// childWin.document — only if same origin

if (window !== window.top) {
  console.log("Running inside a frame");
}
```

**Sources:** [javascript.info/frames-and-windows](https://javascript.info/frames-and-windows) · [MDN — Window](https://developer.mozilla.org/en-US/docs/Web/API/Window) · [MDN — iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/iframe)

</details>

<a name="popups-and-window-methods"></a>

## Popups and window methods

<details>
  <summary>Popups and window methods</summary>

### Popups and window methods

**Definition:** `window.open` creates a new browsing context (popup or tab). The opener holds a reference; the popup sees `window.opener`.

**How it works:**

- **`window.open(url, name, features)`:** returns a `Window` reference or `null` if blocked. Popup vs tab depends on browser and `features` (size/position often imply popup).
- **User gesture:** modern browsers block `open` unless called from a trusted user activation (e.g. `click` handler).
- **`opener`:** same-origin parent may call functions on `opener`; cross-origin access is limited — prefer `postMessage`.
- **Lifecycle:** `popup.close()`, check `popup.closed`; `window.close()` usually only works for windows opened by script.
- **Focus:** `focus()` / `blur()` are restricted; do not rely on stealing focus.
- **Security:** for untrusted `target="_blank"` links use `rel="noopener noreferrer"` so `window.opener` is not exposed.

**When to use:** OAuth login window, print preview, small tool palette; avoid for ads (blocked).

#### Common pitfalls

Calling `open` without a click → `null`; leaving `opener` on external links; polling `closed` without clearing intervals.

**Examples:**

```js
button.addEventListener("click", () => {
  const auth = window.open(
    "https://id.example.com/oauth",
    "oauth",
    "width=500,height=600",
  );
  if (!auth) alert("Allow popups for this site");
});

window.addEventListener("message", (e) => {
  if (e.origin !== "https://id.example.com") return;
  if (e.data.type === "OAUTH_DONE") window.location.reload();
});
```

**Sources:** [javascript.info/popup-windows](https://javascript.info/popup-windows) · [MDN — window.open](https://developer.mozilla.org/en-US/docs/Web/API/Window/open) · [MDN — rel=noopener](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/rel/noopener)

</details>

<a name="cross-window-communication"></a>

## Cross-window communication

<details>
  <summary>Cross-window communication</summary>

### Cross-window communication

**Definition:** Tabs, windows, and frames exchange data through several APIs. Pick by **origin**, **1:1 vs broadcast**, and **message vs shared state**.

**How it works:**

```
1. Origin    same-origin? BroadcastChannel / storage / opener
             cross-origin? postMessage only — never otherWindow.document
2. Send      postMessage / channel.postMessage / localStorage.setItem
3. Receive   message | storage — check event.origin; storage skips the writer
```

| Method                     | Same-origin only | Cross-origin        | Best for              |
| -------------------------- | ---------------- | ------------------- | --------------------- |
| `postMessage`              | no               | yes                 | iframes, OAuth popups |
| `BroadcastChannel`         | yes              | no                  | logout all tabs       |
| `localStorage` + `storage` | yes              | no                  | simple shared flags   |
| `window.opener`            | partial          | `postMessage` safer | popup result          |

- **`postMessage(data, targetOrigin)`** — receiver listens with `addEventListener("message", …)`; **always** validate `event.origin` and prefer a concrete `targetOrigin` (not `*`).
- **`BroadcastChannel(name)`** — pub/sub between same-origin tabs; `close()` when done.
- **`storage` event** — fires in **other** tabs on `localStorage` writes; strings only; not a reliable message bus.

**When to use:** Cross-origin embed → `postMessage`. Same-app tab sync → `BroadcastChannel` or `storage`.

#### Common pitfalls

Skipping `event.origin`; expecting `storage` in the writing tab; using `sessionStorage` for cross-tab sync.

**Examples:**

```js
parent.postMessage({ type: "READY" }, "https://parent.example.com");

window.addEventListener("message", (e) => {
  if (e.origin !== "https://child.example.com") return;
  console.log(e.data);
});

const channel = new BroadcastChannel("app-events");
channel.postMessage({ type: "LOGOUT" });
```

**See also:** [Browser APIs — storage](./3_js_browser_extra.md#browser-storage)

**Sources:** [javascript.info/cross-window-communication](https://javascript.info/cross-window-communication) · [MDN — Window.postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) · [MDN — BroadcastChannel](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel)

</details>

<a name="clickjacking"></a>

## Clickjacking

<details>
  <summary>Clickjacking</summary>

### Clickjacking

**Definition:** An attacker overlays your site in a transparent `iframe` so the user clicks your controls while believing they interact with a decoy page.

**How it works:**

- Mitigate on **HTTP response headers:** `Content-Security-Policy: frame-ancestors 'self'` (or `'none'`) and/or `X-Frame-Options: DENY` / `SAMEORIGIN`.
- `<meta>` CSP cannot set `frame-ancestors` reliably — set headers on the server.
- `SameSite` cookies reduce some session risk in third-party frames but do **not** stop UI redress by themselves.
- Frame-busting `if (top !== self) top.location = …` is bypassable — not a substitute for headers.

**When to use:** Banking, admin, OAuth consent — any authenticated UI that must not be embedded.

**Full essay:** [Special — Clickjacking](./7_special_security.md#clickjacking)

**Sources:** [javascript.info/clickjacking](https://javascript.info/clickjacking) · [MDN — CSP frame-ancestors](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/frame-ancestors)

</details>
