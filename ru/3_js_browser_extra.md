# JS Browser — Additional APIs

Хранилище, CORS, наблюдатели, History, URL и UI-тайминг. Окна, файлы, сеть, анимация и Web Components — в отдельных темах.

[← Назад в JS hub](./3_js.md) · [Browser hub](./3_js_browser_hub.md)

### Related docs

- [Document](./3_js_browser.md) · [Events](./3_js_events.md) · [Windows](./3_js_browser_windows.md) · [Files](./3_js_browser_files.md) · [Network](./3_js_browser_network.md) · [Animation](./3_js_browser_animation.md) · [Web Components](./3_js_web_components.md)
- [JS Core](./3_js_core.md) · [JSON](./3_js_data_structures.md#json) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [HTML browser & rendering](./1_html.md) · [Web API](./6_web_api.md) · [HTTP cookies](./6_web_api_http.md#http-cookies) · [CORS](./6_web_api_security.md#cors)

### Main concepts

| Area     | Topics                                                                                                                                    |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **UI**   | Debounce, throttle, idle callbacks (size/scroll → [Document](./3_js_browser.md#document); rAF → [Animation](./3_js_browser_animation.md)) |
| **APIs** | Storage, CORS, observers, History API, `URL` / `URLSearchParams`                                                                          |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Media queries](#media-queries-in-javascript)
- [Browser APIs & UI patterns](#browser-apis--ui-patterns)
- [Debounce](#debounce) · [Throttle](#throttle) · [requestIdleCallback](#request-idle-callback)
- [Browser storage](#browser-storage) · [CORS](#cors-same-origin) · [Observers](#intersection-resize-observer) · [History API](#history-api) · [URL / URLSearchParams](#url--urlsearchparams)
- [Interview](./3_js_interview.md)

---

## Browser APIs & UI patterns

<details>
  <summary>Debounce</summary>

### Debounce

**Definition:** Debounce **waits until calls stop** for a delay, then runs the function **once** (usually on the trailing edge). Rapid calls are collapsed into one invocation.

**How it works:**

- Each call `clearTimeout`s the previous timer and starts a new `setTimeout`.
- The function runs only after `ms` milliseconds with **no new calls**.
- Typical debounce is trailing-edge (search after typing pauses). Leading-edge debounce exists but is less common.
- Return the wrapper from a HOF; keep the same wrapper for `removeEventListener`.
- Cancel the timeout on unmount or you may run `fn` after the UI is gone.

**When to use:** Search/autocomplete, form validation, “resize ended” layout. Skip for work that must keep up while the user is still moving (use [Throttle](#throttle)).

**Pluses and minuses:**

| +                                      | −                                    |
| -------------------------------------- | ------------------------------------ |
| Cuts noisy `input` down to one request | Feels laggy if `ms` is too large     |
| Simple: `setTimeout` + `clearTimeout`  | Continuous events may **never** fire |

**Examples:**

```js
function debounce(fn, ms) {
  let t;
  return (...args) => {
    clearTimeout(t);
    t = setTimeout(() => fn(...args), ms);
  };
}

const search = debounce((q) => {
  fetch(`/api/search?q=${encodeURIComponent(q)}`).then((r) => r.json());
}, 300);

input.addEventListener("input", (e) => search(e.target.value));
```

**Thoughts:** Interview line: “wait for quiet, then run once.”

**See also:** [Throttle](#throttle)

**Sources:** [MDN Glossary — Debounce](https://developer.mozilla.org/en-US/docs/Glossary/Debounce) · [MDN Window.setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout) · [MDN Window.clearTimeout](https://developer.mozilla.org/en-US/docs/Web/API/Window/clearTimeout)

</details>

<details>
  <summary>Throttle</summary>

### Throttle

**Definition:** Throttle runs a function at most **once per time window** while events keep firing. Unlike debounce, it does **not** wait for a pause — it keeps up at a capped rate.

**How it works:**

- **Leading edge:** first call in a window runs immediately; later calls in that window are ignored until the timer clears (simple `blocked` flag).
- **Trailing / measured timeout:** MDN’s `scroll` pattern stores the latest value and uses `setTimeout` so work runs at most every N ms.
- `requestAnimationFrame` is **not** a throttle for `scroll` — rAF and scroll fire at similar rates; measure the delay yourself (`setTimeout`) or use `IntersectionObserver`.

**When to use:** `scroll`, `mousemove`, live position sync. Skip when you only care about the **final** value after a pause (use [Debounce](#debounce)).

**Pluses and minuses:**

| +                              | −                                       |
| ------------------------------ | --------------------------------------- |
| UI stays updated during motion | May skip the last sample (leading-only) |
| Caps expensive handlers        | Easy to confuse with debounce           |

**Examples:**

```js
function throttle(fn, ms) {
  let blocked = false;
  return (...args) => {
    if (blocked) return;
    fn(...args);
    blocked = true;
    setTimeout(() => {
      blocked = false;
    }, ms);
  };
}

window.addEventListener(
  "scroll",
  throttle(() => {
    console.log(window.scrollY);
  }, 100),
);

// MDN-style: at most every 20ms, always with latest scrollY
let lastY = 0;
let ticking = false;
document.addEventListener("scroll", () => {
  lastY = window.scrollY;
  if (ticking) return;
  ticking = true;
  setTimeout(() => {
    console.log(lastY);
    ticking = false;
  }, 20);
});
```

**Thoughts:** Interview line: “still runs during the storm, but not more often than N ms.”

**See also:** [Debounce](#debounce) · [requestAnimationFrame](./3_js_browser_animation.md#requestanimationframe)

**Sources:** [MDN Glossary — Throttle](https://developer.mozilla.org/en-US/docs/Glossary/Throttle) · [MDN Document scroll event](https://developer.mozilla.org/en-US/docs/Web/API/Document/scroll_event) · [MDN Window.setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)

</details>

<a name="request-idle-callback"></a>

<details>
  <summary>requestIdleCallback</summary>

### requestIdleCallback

**Definition:** Schedules a callback for a browser **idle** period — leftover time in a frame after input, layout, and paint. Distinct from `requestAnimationFrame` (must run before paint) and from microtasks (run before the next macrotask and can delay paint).

**How it works:**

- Callback gets `IdleDeadline`: `timeRemaining()`, `didTimeout`.
- Optional `{ timeout }` forces a run if idle never comes.
- Cooperative: do a slice of work, return, schedule again.

|      | rAF          | requestIdleCallback    | Microtasks                    |
| ---- | ------------ | ---------------------- | ----------------------------- |
| When | Before paint | When idle (or timeout) | After sync, before next macro |
| Use  | Animations   | Analytics, prefetch    | Promise reactions             |

**When to use:** Non-urgent work. Do **not** use for drag/scroll animation.

**Thoughts:** _rAF = this frame’s pixels; idle = leftover time; microtask storms never leave leftover time._

**Sources:** [MDN requestIdleCallback](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback)

</details>

---

<details>
  <summary>Element and window dimensions & scroll</summary>

**Definition:** Element and window size, scroll position, and `scrollIntoView` live in [Document](./3_js_browser.md#element-size-and-scroll).

**See also:** [Element size and scroll](./3_js_browser.md#element-size-and-scroll) · [Window size and scroll](./3_js_browser.md#window-size-and-scroll)

</details>

<details>
  <summary>Coordinates</summary>

**Definition:** Viewport vs document coordinates live in [Document](./3_js_browser.md#coordinates).

**See also:** [Coordinates](./3_js_browser.md#coordinates)

</details>

<details>
  <summary>Selection and Range API</summary>

### Selection and Range API

**Definition:** `Selection` represents user text selection; `Range` is a DOM fragment between two boundary points.

**How it works:**

- `window.getSelection()` returns user selection; `Range` defines start/end containers and offsets.
- `range.selectNodeContents(el)` selects all children; `addRange` applies to selection.

**When to use:**

Editor toolbar bold; copy custom format; highlight search matches.

WYSIWYG editors, copy/custom highlight, find-in-page.

#### Common pitfalls

Selection cleared on DOM mutation; conflating selection with input `selectionStart`; not collapsing range after use.

**Pluses and minuses:**

| +                               | −                                  |
| ------------------------------- | ---------------------------------- |
| Rich text editor building block | Cross-browser quirks in edge cases |
| Programmatic copy/highlight     | Complex nested text nodes          |
| Inspect user selection          | Mobile selection API limited       |

| +                        | −                                     |
| ------------------------ | ------------------------------------- |
| Rich text editor support | Browser inconsistencies in edge cases |
| Programmatic selection   | Complex API surface                   |

**Examples:**

```js
const selection = window.getSelection();
const range = document.createRange();
range.selectNodeContents(document.querySelector("article"));
selection.removeAllRanges();
selection.addRange(range);
```

**Sources:** [javascript.info/selection-range](https://javascript.info/selection-range)

---

</details>

<a name="browser-storage"></a>

<details>
  <summary>Browser storage (cookies, Web Storage, IndexedDB, localForage)</summary>

### Browser storage

**Definition:** Client-side persistence APIs with different size, lifetime, sync vs async, and whether data is sent to the server — pick the smallest tool that fits.

**How it works:**

- **Cookies** — HTTP, not a general store. Server sets `Set-Cookie`; browser sends a `Cookie` header on later requests. Default is a **session cookie** (no `Max-Age`/`Expires` → gone when the browser session ends). Flags: `Path`, `Domain`, `Secure`, `HttpOnly`, `SameSite`. ~4 KB per cookie; encode values (`encodeURIComponent`). Awkward `document.cookie` string → libraries are common. Tracking cookies vs privacy: browsers restrict this; prefer Web Storage / IndexedDB for client-only data. Full HTTP flags: [HTTP cookies](./6_web_api_http.md#http-cookies).
- **Web Storage** — `localStorage` and `sessionStorage`: same sync string KV API (`setItem`/`getItem`/`removeItem`/`clear`/`key`/`length`), different lifetime. Prefer methods over property access (avoids colliding with built-ins; `storage` event only fires for method writes). `sessionStorage` is per tab and dies when the tab closes. `localStorage` is origin-scoped and _intended_ to last until cleared, but browsers still evict. MDN: **5 MiB local + 5 MiB session** per origin; over quota → `QuotaExceededError` (`try/catch`). Writes block the main thread — avoid frequent/large writes. Any same-origin script (including a malicious extension) can read it — **not for tokens**. Private/incognito: `localStorage` behaves like session storage and is wiped when the private session ends. Third-party iframes may be denied storage if third-party cookies are off.
- **IndexedDB** — async transactional object DB (also in workers). `indexedDB.open(name, version)` → `onupgradeneeded` to create stores/indexes. Stores structured-cloneable values (objects, blobs), key ranges, much larger quotas (disk-based, browser-specific). Same-origin. Overkill for a theme flag; right for offline caches, drafts, large datasets. Raw API is event/`IDBRequest`-heavy — wrappers (`idb`, localForage) are common.
- **localForage** — `localStorage`-like API that is **async**. Default driver: IndexedDB (WebSQL as a legacy option); falls back to `localStorage`. Can store JS objects/blobs; JSON stringify/parse is automatic on the localStorage backend.

|                | Cookies                         | sessionStorage | localStorage           | IndexedDB              |
| -------------- | ------------------------------- | -------------- | ---------------------- | ---------------------- |
| Lifetime       | `Expires` / `Max-Age` / session | tab            | until clear / eviction | until clear / eviction |
| Scope          | domain + path                   | origin + tab   | origin (all tabs)      | origin                 |
| Sent to server | yes (every matching request)    | no             | no                     | no                     |
| Capacity       | ~4 KB / cookie                  | ~5 MiB         | ~5 MiB                 | large (quota)          |
| API            | HTTP + `document.cookie`        | sync string KV | sync string KV         | async transactions     |

Safari / WebKit ITP (when tracking prevention is on): **script-writable** data (JS cookies, Web Storage, IndexedDB, SW cache, …) can be deleted if the origin had **no user interaction** (click/tap) in the last **7 days of browser use**. Server-set cookies are exempt from that proactive eviction. Home-screen web apps skip the 7-day cap.

**When to use:**

Auth session the **server** must see → `HttpOnly` + `Secure` + `SameSite` cookie. Theme / locale / small UI prefs → `localStorage`. Wizard / tab-only draft → `sessionStorage`. Offline notes, files, queryable lists → IndexedDB (or localForage). Client-only data → never cookies (they bloat every request).

#### Common pitfalls

JWT / tokens in Web Storage (XSS, extensions). Assuming overflow _wipes_ Web Storage — it **throws**, it does not auto-clear. Cookie table “sent unless HttpOnly” is wrong: HttpOnly is still sent to the server, just not readable from JS. `storage` event does not fire in the writing tab. Safari 7-day rule is **no interaction**, not “did not call `getItem`”. Do not treat localStorage as durable forever.

**Pluses and minuses:**

| +                                              | −                                       |
| ---------------------------------------------- | --------------------------------------- |
| Cookies: server round-trip, `HttpOnly` for XSS | Size; sent every request; clumsy JS API |
| Web Storage: tiny API                          | Sync; strings; XSS-readable; ~5 MiB     |
| IndexedDB: async, structured, large            | Verbose API                             |
| localForage: one API, driver fallback          | Extra dependency                        |

**Examples:**

```js
try {
  localStorage.setItem("user", JSON.stringify({ id: 1 }));
  const user = JSON.parse(localStorage.getItem("user") ?? "null");
  sessionStorage.setItem("draft", formText);
} catch (e) {
  // QuotaExceededError, disabled storage, or private-mode quirks
}

document.cookie = "pref=dark; Path=/; Max-Age=31536000; SameSite=Lax";

const req = indexedDB.open("notes", 1);
req.onupgradeneeded = (e) => {
  e.target.result.createObjectStore("notes", { autoIncrement: true });
};
req.onsuccess = (e) => {
  const db = e.target.result;
  const tx = db.transaction("notes", "readwrite");
  tx.objectStore("notes").add({ text: "read storage notes", ts: Date.now() });
};

await localforage.setItem("user", { id: 1 });
const fromForage = await localforage.getItem("user");
```

**Thoughts:** Cookies for what the **server** needs on the next request; Web Storage for small client prefs; IndexedDB (or localForage) for large/async data.

**Sources:** [javascript.info/cookie](https://javascript.info/cookie) · [javascript.info/localstorage](https://javascript.info/localstorage) · [javascript.info/indexeddb](https://javascript.info/indexeddb) · [MDN — Web Storage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) · [MDN — quotas & eviction](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria) · [MDN — cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies) · [MDN — IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) · [localForage](https://github.com/localForage/localForage)

</details>

<a name="cors-same-origin"></a>

<details>
  <summary>CORS and same-origin policy</summary>

### CORS and same-origin policy

**Definition:** **Same-origin policy** stops a document from reading another origin’s DOM, storage, and (by default) HTTP responses. **CORS** is the HTTP opt-in so `fetch`/`XHR` can read a cross-origin response.

**How it works:** SOP also covers iframes and `document` access — not only APIs. Headers, preflight, credentials, `*`, and `Vary: Origin` live in [Web API — CORS](./6_web_api_security.md#cors).

**When to use:** Cross-origin `fetch` from a page; module scripts / fonts that need CORS; same-origin calls need no CORS headers.

**Pluses and minuses:**

| +                               | −                                               |
| ------------------------------- | ----------------------------------------------- |
| Isolates tabs/iframes by origin | Easy to confuse SOP (DOM) with CORS (HTTP read) |

**Examples:** see [Web API — CORS](./6_web_api_security.md#cors).

**Thoughts:** SOP is the default isolate; CORS is server headers that let **page JS** read a response — not a server firewall.

**Sources:** [javascript.info/fetch-crossorigin](https://javascript.info/fetch-crossorigin) · [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

</details>

<a name="intersection-resize-observer"></a>

<details>
  <summary>IntersectionObserver and ResizeObserver</summary>

### IntersectionObserver and ResizeObserver

**Definition:** `IntersectionObserver` detects when an element enters/leaves viewport (or root); `ResizeObserver` reports element content-box size changes.

**How it works:**

- **IntersectionObserver** — callback when target visibility vs root (viewport or ancestor) crosses thresholds; async, batched, off main polling loop.
- **ResizeObserver** — callback when observed element's size changes; fires before paint (layout already computed).
- Both use `observe(el)` / `unobserve(el)` / `disconnect()`.

|         | IntersectionObserver                        | ResizeObserver                |
| ------- | ------------------------------------------- | ----------------------------- |
| Watches | visibility / intersection ratio             | width / height changes        |
| Use for | lazy load, infinite scroll, ads viewability | responsive components, charts |
| Root    | viewport or custom element                  | element itself                |

**When to use:**

Lazy-load images (`data-src` when intersecting); sticky analytics "50% visible"; textarea auto-grow; canvas resize.

#### Common pitfalls

Not `unobserve` after lazy load; ResizeObserver callback changing size infinitely; expecting IO during `display:none` (no intersection).

**Pluses and minuses:**

| +                                 | −                                                           |
| --------------------------------- | ----------------------------------------------------------- |
| Better perf than scroll listeners | Callback timing async — not synchronous with DOM            |
| Lazy loading standard pattern     | ResizeObserver loop limit errors if callback mutates layout |
| Fine-grained thresholds           | Older browsers need polyfill for IO                         |

**Examples:**

```js
const io = new IntersectionObserver(
  (entries) => {
    for (const e of entries) {
      if (e.isIntersecting) {
        e.target.src = e.target.dataset.src;
        io.unobserve(e.target);
      }
    }
  },
  { rootMargin: "100px" },
);
document.querySelectorAll("img[data-src]").forEach((img) => io.observe(img));

const ro = new ResizeObserver((entries) => {
  for (const { contentRect, target } of entries) {
    target.style.setProperty("--w", `${contentRect.width}px`);
  }
});
ro.observe(document.querySelector(".panel"));
```

**Sources:** [MDN IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) · [MDN ResizeObserver](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver)

</details>

<a name="history-api"></a>

<details>
  <summary>History API and navigation</summary>

### History API and navigation

**Definition:** `history` object manages session history stack for SPA routing without full page reloads via `pushState` / `replaceState` and `popstate` event.

**How it works:**

- `history.pushState(state, '', url)` — adds entry; URL bar updates; no navigation request.
- `history.replaceState(state, '', url)` — replaces current entry.
- `popstate` fires on back/forward (and `history.back()`); `event.state` is the state object (or null).
- `history.length`, `back()`, `forward()`, `go(n)` navigate the stack.
- Full navigation: `location.assign(href)`, `location.href = url`, or `<a>` click — loads new document.

**When to use:**

SPA route change; wizard steps; modal deep-link; replaceState to strip temp query params.

#### Common pitfalls

Forgetting `popstate` listener — back button breaks; pushing same URL duplicates stack; state too large; confusing `hashchange` (`#/page`) with History API.

**Pluses and minuses:**

| +                                     | −                                              |
| ------------------------------------- | ---------------------------------------------- |
| SPA URLs without reload               | Must sync app state with history manually      |
| State object for restore              | `state` structured-clone limits (no DOM nodes) |
| Works with router libs (React Router) | User expects back button to undo app state     |

**Examples:**

```js
function navigate(path) {
  history.pushState({ route: path }, "", path);
  renderRoute(path);
}

window.addEventListener("popstate", (e) => {
  renderRoute(e.state?.route ?? location.pathname);
});

// initial load
renderRoute(location.pathname);
```

**Sources:** [javascript.info/history-api](https://javascript.info/history-api) · [MDN History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)

</details>

<a name="url--urlsearchparams"></a>

<details>
  <summary>URL and URLSearchParams</summary>

### URL and URLSearchParams

**Definition:** `URL` parses/builds a full address; `URLSearchParams` reads and writes the query string (`application/x-www-form-urlencoded`: space → `+`). URL _shape_ (scheme, host, fragment): [URI, URL & URN](./6_web_api_core.md#uri-url--urn). Blob URLs: [createObjectURL](#url-createobjecturl).

**How it works:**

- `new URL(url, base)` — `base` resolves relative `url` (MDN).
- Useful fields: `protocol`, `hostname`, `port` (empty if default), `pathname`, `search`, `searchParams`, `hash`, `href`, `origin` (scheme + host + non-default port).
- `searchParams.get` / `getAll` / `set` / `append` / `delete` / `toString()` (no leading `?`). Duplicate keys are allowed; `append` adds, `set` replaces.
- Do not split `?`/`&` by hand — encoding breaks (`привет`, `+` vs space).

**When to use:**

Build search links; read `location.search`; Node/Next `new URL(request.url)`.

#### Common pitfalls

`toString()` omits `?`; constructing `URLSearchParams` from a raw interpolated string can mishandle `+`; `username`/`password` exist on `URL` but putting credentials in a href is deprecated.

**Examples:**

```js
const url = new URL("/search", "https://doka.guide");
url.searchParams.set("query", "привет");
url.searchParams.append("category", "html");
url.searchParams.append("category", "js");
url.hash = "results";
url.href;
// https://doka.guide/search?query=%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D1%82&category=html&category=js#results

url.searchParams.getAll("category"); // ['html', 'js']
```

**Thoughts:** Parse with `URL`; mutate query with `searchParams`, never with string concat.

**Sources:** [javascript.info/url](https://javascript.info/url) · [MDN — URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) · [MDN — URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)

</details>

<details>
  <summary>Related</summary>

### Related

- [Browser hub](./3_js_browser_hub.md) · [Windows](./3_js_browser_windows.md) · [Files](./3_js_browser_files.md) · [Network](./3_js_browser_network.md)
- HTTP, Fetch → [6_web_api.md](./6_web_api.md) · Document → [3_js_browser.md](./3_js_browser.md) · Events → [3_js_events.md](./3_js_events.md)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
