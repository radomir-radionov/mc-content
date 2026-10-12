# JS Browser — Network

HTTP and realtime APIs callable from page scripts: `fetch`, XHR, and push-style transports.

[← Back to JS hub](./3_js.md) · [Browser hub](./3_js_browser_hub.md)

### Related docs

- [Async APIs — AbortController](./3_js_async_apis.md#abortcontroller) · [Callbacks — AJAX](./3_js_async_callbacks.md#ajax)
- [Files & binary](./3_js_browser_files.md) · [CORS](./3_js_browser_extra.md#cors-same-origin)
- [Web API — HTTP](./6_web_api_http.md) · [Web API — Apps](./6_web_api_apps.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area             | Topics                                       |
| ---------------- | -------------------------------------------- |
| **HTTP**         | `fetch`, `XMLHttpRequest`, `FormData` bodies |
| **Control**      | Abort, download progress, resumable upload   |
| **Cross-origin** | CORS, credentials                            |
| **Realtime**     | Long polling, SSE, WebSocket                 |

### Sections

- [Network requests](#network-requests)
- [Fetch](#fetch)
- [Fetch: download progress](#fetch-download-progress)
- [Fetch: Abort](#fetch-abort)
- [Fetch: cross-origin](#fetch-cross-origin)
- [Fetch API](#fetch-api)
- [URL objects](#url-objects)
- [XMLHttpRequest](#xmlhttprequest)
- [Resumable file upload](#resumable-file-upload)
- [Long polling](#long-polling)
- [WebSocket](#websocket)
- [Server Sent Events](#server-sent-events)

---

<a name="network-requests"></a>

## Network requests

<details>
  <summary>Network requests</summary>

### Network requests

**Definition:** **AJAX** (historical term) means updating data via JavaScript without a full page navigation. Today the default API is **`fetch`**; **`XMLHttpRequest`** remains for upload/download progress and legacy code.

**How it works:** Browser initiates HTTP from the page; cookies and CORS apply. Responses are parsed with `.json()`, `.text()`, `.blob()`, or `.arrayBuffer()`. Full stack detail: [What happens on fetch](./6_web_api_interview.md) · [HTTP in JavaScript](./6_web_api_http.md#http-in-javascript).

**When to use:** REST JSON, file upload, polling feeds. Bidirectional high-frequency chat → [WebSocket](#websocket).

**Sources:** [javascript.info/network](https://javascript.info/network) · [MDN — Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

</details>

<a name="fetch"></a>

## Fetch

<details>
  <summary>Fetch</summary>

### Fetch

**Definition:** **`fetch(url, options)`** returns a `Promise<Response>` for an HTTP request.

**How it works:**

- Network failure **rejects**; HTTP 404/500 **fulfill** — check `response.ok` or `response.status`.
- Body is read once: `await res.json()` | `text()` | `blob()` | `arrayBuffer()`.
- `method`, `headers`, `body`, `credentials`, `mode`, `cache`, `signal` in `options`.

**Examples:**

```js
const res = await fetch("/api/notes");
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const notes = await res.json();
```

**Full detail:** [HTTP — Fetch](./6_web_api_http.md#http-in-javascript)

**Sources:** [javascript.info/fetch](https://javascript.info/fetch) · [MDN — fetch](https://developer.mozilla.org/en-US/docs/Web/API/Window/fetch)

</details>

<a name="fetch-download-progress"></a>

## Fetch: download progress

<details>
  <summary>Fetch: download progress</summary>

### Fetch: download progress

**Definition:** The Fetch API has **no** `onprogress` on the response. Track download progress by reading **`response.body`** as a stream.

**How it works:**

- `const reader = res.body.getReader()` — loop `read()` until `done`.
- Sum `value.byteLength`; compare to `Content-Length` when the server sends it (may be missing on chunked encoding).
- **Upload** progress is **not** available on `fetch` — use [XMLHttpRequest](#xmlhttprequest) `upload.onprogress`.

**Examples:**

```js
const res = await fetch(url);
const total = Number(res.headers.get("Content-Length")) || 0;
const reader = res.body.getReader();
let loaded = 0;
while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  loaded += value.byteLength;
  if (total) console.log(Math.round((loaded / total) * 100));
}
```

**Sources:** [javascript.info/fetch-progress](https://javascript.info/fetch-progress) · [MDN — ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

</details>

<a name="fetch-abort"></a>

## Fetch: Abort

<details>
  <summary>Fetch: Abort</summary>

### Fetch: Abort

**Definition:** **`AbortController`** supplies an **`AbortSignal`** to cancel in-flight `fetch` calls.

**How it works:** `const ac = new AbortController(); fetch(url, { signal: ac.signal });` then `ac.abort()` → promise rejects with `AbortError`. Use `AbortSignal.timeout(ms)` for timeouts.

**When to use:** User navigates away; debounced search (abort stale request); request timeout.

**Full topic:** [JS Async APIs — AbortController](./3_js_async_apis.md#abortcontroller) · [HTTP — AbortController & FormData](./6_web_api_http.md#fetch-api-abortcontroller-and-formdata)

**Sources:** [javascript.info/fetch-abort](https://javascript.info/fetch-abort) · [MDN — AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

</details>

<a name="fetch-cross-origin"></a>

## Fetch: cross-origin

<details>
  <summary>Fetch: cross-origin</summary>

### Fetch: cross-origin

**Definition:** By default, page JavaScript may **not** read cross-origin HTTP responses. **CORS** response headers opt in.

**How it works:**

- Simple GET may succeed without preflight; `POST` with JSON or custom headers often triggers **preflight** `OPTIONS`.
- `fetch(url, { credentials: "include" })` sends cookies only when the server allows credentials (`Access-Control-Allow-Credentials: true`).
- Confuse **SOP** (DOM / storage between frames) with **CORS** (reading HTTP response bodies) — see [CORS and same-origin](./3_js_browser_extra.md#cors-same-origin).

**Full detail:** [Web API — CORS](./6_web_api_security.md#cors)

**Sources:** [javascript.info/fetch-crossorigin](https://javascript.info/fetch-crossorigin) · [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

</details>

<a name="fetch-api"></a>

## Fetch API

<details>
  <summary>Fetch API</summary>

### Fetch API

**Definition:** The **Fetch API** includes `fetch`, `Request`, `Response`, `Headers`, and streaming bodies.

**How it works:**

- `new Request(url, init)` — reusable request object.
- `response.headers.get("etag")`; `response.redirected`, `response.url`.
- `keepalive: true` for small beacons on `pagehide` (size limits apply).
- `integrity` for Subresource Integrity checks.

**When to use:** Service workers intercept `fetch`; custom retry wrappers; cloning responses (`res.clone()`).

**Sources:** [javascript.info/fetch-api](https://javascript.info/fetch-api) · [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

</details>

<a name="url-objects"></a>

## URL objects

<details>
  <summary>URL objects</summary>

### URL objects

**Definition:** **`URL`** and **`URLSearchParams`** parse and build addresses in JavaScript (including in Node and workers).

**How it works:** `new URL("/search", "https://example.com")`; mutate `url.searchParams`. Do not hand-parse query strings.

**Canonical page:** [Browser APIs — URL / URLSearchParams](./3_js_browser_extra.md#url--urlsearchparams) · [URI anatomy](./6_web_api_core.md#uri-url--urn)

**Sources:** [javascript.info/url](https://javascript.info/url) · [MDN — URL](https://developer.mozilla.org/en-US/docs/Web/API/URL)

</details>

<a name="xmlhttprequest"></a>

## XMLHttpRequest

<details>
  <summary>XMLHttpRequest</summary>

### XMLHttpRequest

**Definition:** **`XMLHttpRequest`** (XHR) is the legacy AJAX API with event-based progress and fine-grained control.

**How it works:**

- `open(method, url)` → `send(body)` → `onload` / `onerror`.
- `xhr.upload.onprogress` — **upload** progress (use when `fetch` is not enough).
- `xhr.onprogress` — download progress events.
- `responseType`: `""`, `"json"`, `"blob"`, `"arraybuffer"`.
- `withCredentials = true` for cross-origin cookies (server must allow).

**When to use:** Upload progress bars; very old browsers; libraries not yet on `fetch`.

**Examples:**

```js
const xhr = new XMLHttpRequest();
xhr.open("POST", "/upload");
xhr.upload.onprogress = (e) => {
  if (e.lengthComputable) console.log(e.loaded / e.total);
};
xhr.onload = () => console.log(xhr.status, xhr.response);
xhr.send(fd);
```

**Sources:** [javascript.info/xmlhttprequest](https://javascript.info/xmlhttprequest) · [MDN — XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

</details>

<a name="resumable-file-upload"></a>

## Resumable file upload

<details>
  <summary>Resumable file upload</summary>

### Resumable file upload

**Definition:** **Resumable upload** continues after failure by sending only the remaining bytes, coordinated with server-side offset tracking.

**How it works:**

1. Client asks server for bytes already stored (`X-File-Id` / `Range` / custom API).
2. `file.slice(startByte)` — `Blob` slice is cheap.
3. Upload chunk with `X-Start-Byte` or `Content-Range`; server appends.
4. Often implemented with XHR for per-chunk progress; `fetch` works for chunks without upload progress.

**When to use:** Large files on flaky networks; not needed for small avatar uploads.

**Sources:** [javascript.info/resume-upload](https://javascript.info/resume-upload) · [MDN — Blob.slice](https://developer.mozilla.org/en-US/docs/Web/API/Blob/slice)

</details>

<a name="long-polling"></a>

## Long polling

<details>
  <summary>Long polling</summary>

### Long polling

**Definition:** The client opens an HTTP request; the **server holds** the connection until data exists (or timeout), then responds; the client immediately opens the next request.

**How it works:** Near-real-time with plain HTTP; many pending connections on the server. Superseded for one-way push by [SSE](#server-sent-events); for duplex chat by [WebSocket](#websocket).

**Full comparison:** [Polling, long polling, SSE & WebSocket](./6_web_api_apps.md#polling-long-polling-sse--websocket)

**Sources:** [javascript.info/long-polling](https://javascript.info/long-polling)

</details>

<a name="websocket"></a>

## WebSocket

<details>
  <summary>WebSocket</summary>

### WebSocket

**Definition:** **`WebSocket`** is a persistent, full-duplex channel after an HTTP `101 Switching Protocols` handshake.

**How it works:** `const ws = new WebSocket("wss://…")`; events `open`, `message`, `error`, `close`; `ws.send(data)`. Prefer **`wss:`** in production.

**When to use:** Chat, games, collaborative editing — many small bidirectional messages.

**Full topic:** [Web API — WebSocket](./6_web_api_apps.md#websocket) · [WebSockets interview](./6_web_api_websockets_interview.md)

**Sources:** [javascript.info/websocket](https://javascript.info/websocket) · [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

</details>

<a name="server-sent-events"></a>

## Server Sent Events

<details>
  <summary>Server Sent Events</summary>

### Server Sent Events

**Definition:** **Server-Sent Events (SSE)** stream **server → client** text events over a long-lived HTTP connection (`text/event-stream`).

**How it works:**

- `const es = new EventSource("/api/events")` — automatic reconnect in browsers.
- Listen: `es.onmessage` or `addEventListener("stock-update", …)` for named `event:` lines.
- One-way only; send data to server with separate `fetch` calls.

**When to use:** Live notifications, dashboards, stock tickers — simpler than WebSocket when the client does not push over the same socket.

**Full topic:** [Polling, long polling, SSE & WebSocket](./6_web_api_apps.md#polling-long-polling-sse--websocket)

**Sources:** [javascript.info/server-sent-events](https://javascript.info/server-sent-events) · [MDN — EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)

</details>
