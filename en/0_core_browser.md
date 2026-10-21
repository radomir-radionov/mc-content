# Core — Browser platform

**Browser work principles** and how modern browsers run pages: navigation, rendering pipeline, JavaScript on the main thread, multi-process isolation, and security.

[← Core index](./0_core.md)

### Related docs

- [Accessibility](./1_html_a11y.md) · [Web security](./7_special_security.md) · [Browser security](./6_web_api_security.md)
- [HTML — Rendering (CRP stages)](./1_html_rendering.md#from-url-to-pixels) · [JS — Browser environment](./3_js_browser.md#browser-environment) · [JS — Language (V8)](./3_js_core.md#v8-engine-architecture) · [Event loop (full)](./3_js_async_event_loop.md#event-loop) · [JS — Browser hub](./3_js_browser_hub.md)

### Main concepts

| Area           | Topics                                                                                 |
| -------------- | -------------------------------------------------------------------------------------- |
| **Principles** | 8 subtopics: navigation → DOM → CSSOM → layout → JS → event loop → processes → sandbox |
| **Processes**  | Browser, renderer, GPU; site isolation                                                 |
| **Engine**     | V8, JIT, generational GC                                                               |
| **Rendering**  | DOM/CSSOM, main thread; stage detail → [HTML — Rendering](./1_html_rendering.md)       |
| **Security**   | Same-origin, CORS, XSS, CSP                                                            |
| **Deep dives** | CRP step-by-step (HTML track) · V8/GC (JS Core) · event loop (JS Async)                |

### Sections

- [Browser work principles](#browser-work-principles)
- [Browser platform](#browser-platform)

---

<a name="browser-work-principles"></a>

## Browser work principles

<details>
  <summary>Browser work principles</summary>

### Browser work principles

**Definition:** **Browser work principles** are the stable rules of how a **user agent** loads, renders, runs script, and isolates sites. Expand each subtopic below for interview depth.

**How it works:**

| Step | Subtopic                                                                | Focus                    |
| ---- | ----------------------------------------------------------------------- | ------------------------ |
| 1    | [Navigation and fetching](#navigation-and-fetching)                     | URL → response bytes     |
| 2    | [HTML parsing and the DOM](#html-parsing-and-the-dom)                   | Markup → live tree       |
| 3    | [CSSOM and style computation](#cssom-and-style-computation)             | Cascade → computed style |
| 4    | [Layout, paint, and compositing](#layout-paint-and-compositing)         | Geometry → GPU frame     |
| 5    | [JavaScript engine and main thread](#javascript-engine-and-main-thread) | ECMAScript + DOM APIs    |
| 6    | [Event loop and task queues](#event-loop-and-task-queues)               | Microtasks vs macrotasks |
| 7    | [Multi-process architecture](#multi-process-architecture)               | Renderer isolation       |
| 8    | [Sandbox and same-origin policy](#sandbox-and-same-origin-policy)       | Trust boundaries         |

```text
URL → fetch → parse HTML → DOM ──┐
       ↓                         ├→ render tree → layout → paint → composite
     fetch CSS → CSSOM ──────────┘
                       ↑ JS on main thread (mutates → style/layout)
```

**When to use:** Job text like «понимание принципов работы браузеров» — overview first, then each accordion.

**Thoughts:** This page is the **Frontend → Core** map. Longer essays live in sibling topics — do not duplicate them here.

| Topic                            | Canonical file                                                                                                                 |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| URL → pixels (DNS…composite)     | [HTML — Rendering](./1_html_rendering.md#from-url-to-pixels)                                                                   |
| Layout/paint stages, dirty nodes | [HTML — Rendering](./1_html_rendering.md#layout-reflow) · [Progressive rendering](./1_html_rendering.md#progressive-rendering) |
| `window`, spec layers, workers   | [JS Browser — Document](./3_js_browser.md#browser-environment)                                                                 |
| V8, JIT, GC                      | [JS Core — V8](./3_js_core.md#v8-engine-architecture)                                                                          |
| Microtasks, macrotasks, rAF      | [JS Async — Event loop](./3_js_async_event_loop.md#event-loop)                                                                 |

**Sources:** [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web)

</details>

<a name="navigation-and-fetching"></a>

<details>
  <summary>Navigation and fetching</summary>

### Navigation and fetching

**Definition:** **Navigation** is everything from opening a URL until the browser has an **HTTP response stream** to parse: DNS, TCP/TLS, HTTP, redirects, and **caching**.

**How it works:**

| Phase        | What happens                                                   |
| ------------ | -------------------------------------------------------------- |
| **URL**      | Scheme, host, port, path, query; `#fragment` stays client-side |
| **DNS**      | Resolve host → IP (hosts file → browser cache → OS → resolver) |
| **Connect**  | TCP handshake; TLS for HTTPS (cert check)                      |
| **HTTP**     | Request/response headers + body stream; cookies attached       |
| **Cache**    | Memory/disk cache may skip network (`Cache-Control`, `ETag`)   |
| **Redirect** | `3xx` + `Location` starts new navigation                       |

**Navigation kinds (same tab):**

| Kind               | What changes                      | Typical API / trigger                          |
| ------------------ | --------------------------------- | ---------------------------------------------- |
| **Cross-document** | New document in renderer (often)  | Click `<a href>`, `location.assign`, `301/302` |
| **Same-document**  | URL/history state, no full reload | `#hash`, `history.pushState`, `popstate`       |
| **Reload**         | Revalidate or bypass cache        | F5, `location.reload()`                        |
| **Back/forward**   | May restore from **bfcache**      | History traversal                              |

- **Document vs subresource:** HTML navigation drives the **main frame** lifecycle; `<img>`, scripts, and `fetch()` are separate requests with their own cache keys and priorities.
- **HTTP/2+:** one connection **multiplexes** many streams (fewer TCP/TLS handshakes); **HTTP/3** uses QUIC over UDP — still visible as TTFB + download in DevTools **Network**.
- **Early hints (`103`):** server can push **preload** link headers before the full response body — reduces critical-path latency when supported.
- **Preload scanner:** while the HTML parser is blocked on a sync script, a secondary scanner still discovers `<link rel="stylesheet">`, `<script src>`, `<img>` so fetches can start early.
- **bfcache** restores whole pages from memory on back/forward (not every page qualifies — `unload` listeners, `Cache-Control: no-store`, etc. can opt out).
- **Prefetch / preconnect** (`rel=preconnect`, `dns-prefetch`, `modulepreload`) → [HTML — Performance](./1_html_performance.md).
- **Service Worker** can intercept navigations and `fetch` → [Web API](./6_web_api.md).

**Examples:** In **Performance** → **Navigation timing**: `domainLookupStart` → `connectStart` → `requestStart` → `responseStart` (TTFB) → `domInteractive` separates network from parse.

**When to use:** Slow TTFB, stale documents, redirect loops, SPA “soft nav” vs full reload, waterfall analysis in DevTools.

#### Common pitfalls

Ignoring connection setup cost. Assuming every navigation bypasses cache.

**See also:** [From URL to pixels](./1_html_rendering.md#from-url-to-pixels) · [HTTP caching](./6_web_api_http.md)

**Sources:** [MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching) · [Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_API)

</details>

<a name="html-parsing-and-the-dom"></a>

<details>
  <summary>HTML parsing and the DOM</summary>

### HTML parsing and the DOM

**Definition:** The **HTML parser** builds a live **DOM** tree from bytes. Scripts and CSS interact with this tree — not with the original source file.

**How it works:**

- **Pipeline:** bytes → **tokenizer** (tags, text, comments) → **tree builder** (open elements, foster parenting) → live **DOM** nodes wired to JS bindings.
- **Streaming parse:** DOM grows while HTML still downloads; the engine can **layout and paint incrementally** before `</html>`.
- **Error recovery:** invalid markup is fixed per HTML rules (e.g. auto-closing tags, `<table>` foster parenting) — do not rely on errors to “fail safe” in production.
- **Blocking stylesheet:** a `<link rel="stylesheet">` in `<head>` blocks **script execution** that follows until the sheet is loaded and CSSOM is built (unless `media` does not match or `disabled`).
- **Script impact on parse:**

| Script type                 | Parse blocking?                  | Run order                              |
| --------------------------- | -------------------------------- | -------------------------------------- |
| Sync `<script src>` in body | Yes, while fetch+run             | Document order                         |
| `defer`                     | No (except order vs other defer) | After parse, before `DOMContentLoaded` |
| `async`                     | Only while script runs           | First-ready-first-run                  |
| `type="module"`             | Deferred default                 | Module graph order                     |

- **`DOMContentLoaded`:** HTML parsed; deferred/module scripts finished — safe to bind UI that needs a complete DOM (not all images).
- **`load`:** Subresources (images, stylesheets, iframes) loaded — full “page ready” for metrics.
- **`document.write`:** while parsing, can inject markup; after parse completes it can **replace** the whole document — legacy pattern, avoid.

**Examples:**

```html
<!-- Blocks parse + first paint until downloaded and executed -->
<script src="/heavy-sync.js"></script>
<!-- Parser continues; runs in order after parse -->
<script defer src="/app.js"></script>
```

**When to use:** Blank page until bundle, parser blocked by sync script in `<head>`, “flash” before hydration.

#### Common pitfalls

Large sync script before first paint. Using `document.write` after load (can wipe document).

**See also:** [Script loading](./3_js_browser.md#script-loading) · [DOM tree](./3_js_browser.md#dom-tree)

**Sources:** [HTML parsing](https://html.spec.whatwg.org/multipage/parsing.html) · [MDN — DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

</details>

<a name="cssom-and-style-computation"></a>

<details>
  <summary>CSSOM and style computation</summary>

### CSSOM and style computation

**Definition:** The **CSSOM** holds stylesheet rules. The engine **cascades** (origin, importance, specificity, order), then **computes** used values per element.

**How it works:**

- Sources: `<link rel="stylesheet">`, `<style>`, inline `style=""`, `@import` (discovered only after parent sheet loads — avoid deep chains).
- **Cascade (simplified order):** origin (author vs user vs browser) → **`@layer`** order → importance (`!important`) → **specificity** → source order.
- **Render-blocking:** CSS in `<head>` often delays **first render** until required CSSOM exists — inline **critical CSS** or split bundles to shorten blocking window.
- **Render tree:** DOM nodes + computed styles; subtrees with `display: none` (and similar) usually skip layout; `visibility: hidden` still participates in layout.
- **Computed vs used:** inheritance and `%`/`em` resolve to **computed** values; **used** values are what layout reads (e.g. used `width` after flex/grid rules).
- **Invalidation:** class/DOM/style changes mark dirty nodes; engine recalculates **incrementally** (not full-page style every keystroke).
- **Performance hooks:** `content-visibility: auto` skips off-screen layout/paint; `contain: layout style paint` limits invalidation blast radius.

**Examples:** Changing `element.className` triggers **recalc style** → possible **layout** → **paint** → **composite** in DevTools **Performance** panel.

**When to use:** FOUC, specificity/`@layer` wars, critical CSS, “why does this selector win?”

#### Common pitfalls

`@import` chains delaying CSS discovery. Animating layout-heavy properties every frame.

**See also:** [CSS Core](./2_css_core.md) · [Critical rendering path](./1_html_rendering.md#first-render--full-pipeline)

**Sources:** [MDN — CSSOM](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)

</details>

<a name="layout-paint-and-compositing"></a>

<details>
  <summary>Layout, paint, and compositing</summary>

### Layout, paint, and compositing

**Definition:** **Layout** (reflow) computes geometry; **paint** fills pixels; **compositing** merges **layers** on the GPU.

**How it works:**

| Stage         | Triggers (examples)                                      | Cost   |
| ------------- | -------------------------------------------------------- | ------ |
| **Layout**    | DOM insert, text change, read layout metrics after write | High   |
| **Paint**     | Color, text, shadows in affected region                  | Medium |
| **Composite** | `transform`, `opacity` on promoted layer                 | Lower  |

- **Layout outputs:** box geometry (position, size) and **fragment** trees for inline text; **flex/grid** resolve before block flow in modern engines.
- **Paint:** fills **display lists** (text, backgrounds, borders, shadows) per layer; **repaint** skips layout when geometry unchanged.
- **Layers / compositing:** promoted layers (e.g. `will-change: transform`, `<video>`, `<canvas>`, some animations) upload to GPU; **composite** merges them each frame.
- **Stacking contexts:** `z-index`, `opacity`, `transform`, and `isolation` create ordered paint groups — “z-index only works among siblings in the same context.”
- **Layout thrashing:** alternate read (`offsetWidth`, `getBoundingClientRect`) and write in a loop forces sync layout — **batch reads, then writes** (or read in `requestAnimationFrame` after writes from previous frame).
- **CLS:** unexpected layout shifts — reserve space for images (`width`/`height`, `aspect-ratio`) and web fonts (`font-display`, preload).
- **CRP:** minimize blocking CSS/JS for first paint → [First render — full pipeline](./1_html_rendering.md#first-render--full-pipeline).
- **Incremental layout:** engines mark **dirty** subtrees after DOM/style changes instead of reflowing the whole page → [Global vs incremental layout](./1_html_rendering.md#global-vs-incremental-layout-dirty-nodes).
- **Progressive rendering:** paint can happen before the full document arrives → [Progressive rendering](./1_html_rendering.md#progressive-rendering).
- **`requestAnimationFrame`:** runs before the next **paint**; good for visual updates; still on the main thread.

**When to use:** Jank, animation tuning, Core Web Vitals (LCP/CLS/INP), DevTools **Rendering** → “Layer borders”.

**Examples:**

```js
const w = els.map((el) => el.offsetWidth);
els.forEach((el, i) => {
  el.style.width = w[i] + 10 + "px";
});
```

**Sources:** [MDN — Rendering](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Rendering) · [web.dev — Rendering performance](https://web.dev/articles/rendering-performance)

</details>

<a name="javascript-engine-and-main-thread"></a>

<details>
  <summary>JavaScript engine and main thread</summary>

### JavaScript engine and main thread

**Definition:** The **JS engine** (V8, SpiderMonkey, JavaScriptCore) runs ECMAScript on the renderer **main thread** together with parsing, style, and layout — unless you offload to **Web Workers**.

**How it works:**

- **Compile pipeline:** parse → bytecode → **JIT optimized code** (inline caches, hidden classes) → occasional **deopt** back to interpreter when types change → [V8 architecture](./3_js_core.md#v8-engine-architecture).
- **Realms:** each **window** / **iframe** origin gets an isolated JS **realm**; same-origin iframes share browsing-context rules — security is still enforced at host bindings.
- **Host bindings:** `document`, `fetch`, timers, DOM APIs bridge into browser C++ (Blink/WebKit/Gecko); crossing the boundary has cost — batch DOM work.
- **Long tasks:** >50ms JS on the main thread delays input delivery and frame production → poor **INP**; break up work (`scheduler.postTask`, chunks, workers).
- **Workers:** **Dedicated** / **Shared** workers run JS off-thread; `postMessage` + **Transferable** objects; **no DOM** — use **OffscreenCanvas** for some graphics offload.
- **WebAssembly:** runs in the same renderer process with sandboxed linear memory — not a second event loop on the main thread unless you `await` back to JS.
- **GC:** generational collectors reclaim unreachable objects; **GC pauses** can jank frames → [Garbage collection](./3_js_objects.md#garbage-collection).

**Examples:** DevTools **Performance** — purple “Scripting” blocks during input show main-thread contention; **Coverage** shows unused JS on cold load.

**When to use:** Main-thread profiling, moving CPU work off-thread, explaining “async doesn’t mean parallel on main thread”.

#### Common pitfalls

Heavy computation with `async/await` still on main thread. Leaks via detached DOM nodes + closures.

**See also:** [Event loop](#event-loop-and-task-queues) · [Browser environment](./3_js_browser.md#browser-environment)

**Sources:** [MDN — JS engines](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/JavaScript_technologies_overview#javascript_engines)

</details>

<a name="event-loop-and-task-queues"></a>

<details>
  <summary>Event loop and task queues</summary>

### Event loop and task queues

**Definition:** The **event loop** schedules JS on the main thread: run script → drain **microtasks** → run one **macrotask** → (maybe) render → repeat.

**How it works:**

| Kind           | Examples                                                                                                  |
| -------------- | --------------------------------------------------------------------------------------------------------- |
| **Microtasks** | `Promise.then`, `queueMicrotask`, `MutationObserver`                                                      |
| **Macrotasks** | `setTimeout`, `setInterval`, I/O, user input, `postMessage`, `requestAnimationFrame` callbacks (as tasks) |
| **Rendering**  | Browser may run **style/layout/paint** between macrotasks when the frame deadline requires it             |

**One turn (simplified):**

1. Run JS until the call stack is empty.
2. **Drain microtasks** completely (each `Promise.then` can enqueue more microtasks).
3. Run **one macrotask** from the queue.
4. Optionally **update rendering** (rAF callbacks run before paint for that frame).
5. Repeat.

```js
console.log("1");
setTimeout(() => console.log("4"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("2");
// 1 → 2 → 3 → 4
```

```js
// Microtask starvation — blocks paint and input until loop ends
function spin() {
  queueMicrotask(spin);
}
spin();
```

- **`async/await`:** each `await` resumes via **microtasks** — deep async chains still monopolize the main thread between macrotasks.
- **Node vs browser:** Node adds **libuv** phases; **browser** ties rendering to the **HTML event loop** — do not assume identical ordering in SSR vs client.

**When to use:** Task ordering bugs; starving UI with microtask loops; “why didn’t the DOM update before my log?”

**See also:** [Event loop (full)](./3_js_async_event_loop.md#event-loop)

**Sources:** [MDN — Event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop)

</details>

<a name="multi-process-architecture"></a>

<details>
  <summary>Multi-process architecture</summary>

### Multi-process architecture

**Definition:** Browsers split work into **processes** for crash recovery and **site isolation**.

**How it works:**

| Process      | Role                                                         |
| ------------ | ------------------------------------------------------------ |
| **Browser**  | UI chrome, profile, permissions, coordinates renderers       |
| **Renderer** | DOM, JS, layout, paint (per **site** / **site group**)       |
| **GPU**      | Compositing, GL/Vulkan for layers                            |
| **Network**  | HTTP stack, cookies, cache (Chromium — not in every browser) |

- **Site isolation:** cross-site pages get **separate renderer processes** (or strict **site groups**) so compromised renderer memory cannot read another origin’s data.
- **OOPIF:** **out-of-process iframes** — cross-site `<iframe>` may live in its own renderer; `postMessage` / `MessageChannel` for cooperation; parent cannot touch cross-origin DOM.
- **IPC:** processes talk via **Mojo** / IPC channels — copying handles and serialized messages; keep cross-frame chatter small.
- **Tab crash:** renderer dies; browser process survives and shows “Aw, snap!” — other tabs keep running.
- **Utility / plugin** processes (vendor-specific): audio, PDF, extensions — treat as extra isolation boundaries in Chromium-shaped stacks.

**When to use:** “Why can’t I read iframe DOM?”; security architecture questions; memory per tab in Task Manager.

**See also:** [Browser platform architecture](#browser-platform-architecture)

**Sources:** [Chromium multi-process](https://www.chromium.org/developers/design-documents/multi-process-architecture/) · [Site isolation](https://www.chromium.org/Home/chromium-security/site-isolation/)

</details>

<a name="sandbox-and-same-origin-policy"></a>

<details>
  <summary>Sandbox and same-origin policy</summary>

### Sandbox and same-origin policy

**Definition:** **Sandbox** limits OS access from web content. **Same-origin policy** restricts cross-origin DOM/storage access. **Origin** = scheme + host + port.

**How it works:**

- **SOP:** no reading another origin’s DOM, `localStorage`, or most properties on cross-origin `Window`/`Document` from your script.
- **CORS:** server headers allow **reading** cross-origin **HTTP responses** in JS (`fetch`/XHR) — the browser still **sends** the request; it may hide the body from script without `Access-Control-Allow-Origin`.
- **`<iframe sandbox>`:** token list (`allow-scripts`, `allow-same-origin`, …) strips capabilities even for same-origin embeds when combined correctly.
- **Permissions Policy** (`Permissions-Policy` header): disables APIs (camera, geolocation, `payment`) per frame or origin.
- **Mixed content:** active HTTP subresources on HTTPS pages blocked or upgraded — protects integrity of secure pages.
- **Cross-origin isolation:** `Cross-Origin-Opener-Policy` + `Cross-Origin-Embedder-Policy` enable `SharedArrayBuffer` and tighter timing isolation — see [Web API security](./6_web_api_security.md).
- **CSP / XSS / cookies:** → [Security model overview](#security-model-overview).

**Examples:** `fetch('https://api.other.com')` from `https://app.example` — network succeeds, JS throws unless API sends CORS allow headers; embedding `https://bank.com` in `<iframe>` does not let your page read its DOM.

**When to use:** Threat modeling; CORS console errors; sandboxing third-party widgets; why `<iframe>` cannot read parent cross-origin.

**Sources:** [MDN — SOP](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Same-origin_policy)

</details>

<a name="browser-platform"></a>

## Browser platform

<details>
  <summary>Browser platform architecture</summary>

### Browser platform architecture

**Definition:** A **browser** is a multi-process host: HTML/CSS/JS for one site usually run in a **renderer** process with a dedicated **JavaScript engine** (V8 in Chrome/Edge). Parsing, layout, and paint share the renderer **main thread** with your scripts.

**How it works:**

| Layer                 | Role                                          | Deep dive                                                                                                               |
| --------------------- | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Browser process**   | Tabs, permissions, network coordination       | [Chromium multi-process](https://www.chromium.org/developers/design-documents/multi-process-architecture/)              |
| **Renderer + engine** | DOM, JS, layout, paint; **isolate** per realm | [Browser environment](./3_js_browser.md#browser-environment) · [V8 architecture](./3_js_core.md#v8-engine-architecture) |
| **GPU process**       | Compositing layers to the screen              | [HTML — Rendering](./1_html_rendering.md)                                                                               |
| **Scheduling**        | Sync code → microtasks → macrotasks → render  | [Event loop](./3_js_async_event_loop.md#event-loop)                                                                     |

- **Site isolation:** separate renderer processes (or strict site groups) so one compromised tab cannot read another site’s memory.
- **Not the same subsystem:** V8 executes ECMAScript; **Blink** (in Chromium) builds the DOM and runs layout — interview answers should name both.

**When to use:** Frontend **Core** fundamentals before a11y/security details; “how the browser works” questions that span HTML, JS, and HTTP.

#### Common pitfalls

Blaming **V8** for layout bugs. Treating **CORS** as DOM policy (it governs whether **page JS** may read a cross-origin **HTTP** response). Forgetting that long JS tasks block parsing and paint on the main thread.

**Pluses and minuses:**

| +                                            | −                                                |
| -------------------------------------------- | ------------------------------------------------ |
| One map for HTML, JS, and network interviews | Engine/process details differ by browser vendor  |
| Links out to full API and attack essays      | Not a replacement for hands-on DevTools practice |

**Thoughts:** Start with [Browser work principles](#browser-work-principles) subtopics. Core siblings: [Accessibility](./1_html_a11y.md) · [Web security](./7_special_security.md) · [JS Browser](./3_js_browser_hub.md).

**Sources:** [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web) · [V8.dev](https://v8.dev/)

</details>

<details>
  <summary>Web platform stack (HTML, CSS, JavaScript)</summary>

### Web platform stack (HTML, CSS, JavaScript)

**Definition:** The **web platform** splits responsibilities: **HTML** — structure and meaning; **CSS** — presentation; **JavaScript** — behavior and I/O from the page.

**How it works:**

| Layer          | Spec / role                      | Runtime artifact                              |
| -------------- | -------------------------------- | --------------------------------------------- |
| **HTML**       | Semantics, forms, media elements | **DOM** + accessibility tree                  |
| **CSS**        | Cascade, layout modes, paint     | **CSSOM** + used styles                       |
| **JavaScript** | Logic, fetch, workers, WASM      | V8/SpiderMonkey/JSC on main thread or workers |

- The parser builds a **DOM** from HTML; CSS builds a **CSSOM**; scripts mutate the tree through **host bindings** (not by reparsing source files on each change).
- **Web APIs** (fetch, storage, observers) are **not** ECMAScript — they are browser-provided objects wired into the JS engine.
- Scripts load with `defer`, `async`, or `type="module"` — each affects parse and first paint differently → [HTML — Performance](./1_html_performance.md).
- **Progressive enhancement:** meaningful HTML first; CSS and JS enhance — aligns with [Accessibility](./1_html_a11y.md) (works without JS where possible).

**When to use:** Explaining how markup, styles, and logic fit together on **Core** before diving into attack surfaces.

**See also:** [HTML — Core](./1_html_core.md#web-platform-layers) · [JS — Core language](./3_js_core.md)

**Sources:** [MDN — How the web works](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works)

</details>

<a name="security-model-overview"></a>

<details>
  <summary>Security model overview</summary>

### Security model overview

**Definition:** Browsers **sandbox** pages: limited file/OS access, **same-origin** isolation for DOM and storage, and HTTP mechanisms (**CORS**, **CSP**, cookies flags) that complement safe markup and JS.

**How it works:**

| Control                | Stops / limits                                                        | Topic                                                      |
| ---------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------- |
| **Same-origin policy** | Cross-origin DOM reads                                                | [CORS / SOP](./3_js_browser_extra.md#cors-same-origin)     |
| **CORS**               | Cross-origin **response read** from `fetch`/XHR unless server opts in | [Web API — CORS](./6_web_api_security.md#cors)             |
| **XSS**                | Injected script running as your origin                                | [XSS](./7_special_security.md#xss-cross-site-scripting)    |
| **CSP**                | Which scripts/styles/frames may run or load                           | [CSP](./7_special_security.md#content-security-policy-csp) |

- **CORS ≠ CSRF protection** for cookie-authenticated forms; **XSS** defeats same-origin because attacker code runs **as** the victim page.

**When to use:** Bridge from platform architecture to Core **Security** cluster essays and interview prep.

**Sources:** [OWASP — XSS Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) · [MDN — CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)

</details>

<a name="critical-rendering-path-detailed-stages"></a>

<details>
  <summary>Critical rendering path (detailed stages)</summary>

### Critical rendering path (detailed stages)

**Definition:** The **critical rendering path (CRP)** is the ordered work from first bytes of HTML/CSS through DOM, CSSOM, render tree, layout, paint, and compositing. **Core** summarizes principles above; **HTML — Rendering** owns step-by-step depth.

**How it works:** Follow the dedicated topic for DNS, fetching HTML, HTML parsing, CSSOM, render tree, layout, paint, compositing, layout vs paint vs composite, reflow/animation frames, and first-render checklists.

**When to use:** HTML track interviews, DevTools Performance tracing, optimizing LCP/CLS on markup and asset order.

**Full topic:** [HTML — Rendering](./1_html_rendering.md#from-url-to-pixels) · [First render — full pipeline](./1_html_rendering.md#first-render--full-pipeline)

**Sources:** [MDN — Critical rendering path](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path)

</details>

<a name="browser-environment-javascript-host"></a>

<details>
  <summary>Browser environment (JavaScript host)</summary>

### Browser environment (JavaScript host)

**Definition:** From JavaScript, the **browser host** is `globalThis` / `window`, DOM/HTML/CSSOM bindings, and permission-gated APIs — distinct from ECMAScript and from the rendering engine’s C++ layout code.

**How it works:** ECMAScript vs web platform specs, `document` vs `window`, feature detection, and script-facing security live on the **JS Browser — Document** topic. Process model and main-thread rendering → [Multi-process architecture](#multi-process-architecture) and [JavaScript engine and main thread](#javascript-engine-and-main-thread) above.

**When to use:** Isomorphic code, “why is `fetch` undefined in Node?”, mapping interview answers to **language vs host vs network**.

**Full topic:** [JS Browser — Browser environment](./3_js_browser.md#browser-environment) · hub map [JS — Browser](./3_js_browser_hub.md)

**Sources:** [MDN — JavaScript technologies overview](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/JavaScript_technologies_overview)

</details>
