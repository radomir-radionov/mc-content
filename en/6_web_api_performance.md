# Web APIs — Performance

Loading metrics, resource hints, and browser performance APIs. Core Web Vitals overview → [HTML — Performance](./1_html_performance.md#core-web-vitals).

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Performance](./6_web_api_performance.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [14. Advanced topics](#14-advanced-topics)
- [API and client performance optimization](#api-and-client-performance-optimization)

---

<a name="14-advanced-topics"></a>

## 14. Advanced topics

<details>
  <summary>Service Workers</summary>

### Service Workers

**Definition:**

A service worker is a background script that intercepts network requests and enables offline caching, push notifications, and background sync.

| Capability         | Use case           |
| ------------------ | ------------------ |
| Fetch interception | Offline-first apps |
| Cache API          | Asset precaching   |
| Push API           | Notifications      |

**Examples:**

```js
navigator.serviceWorker.register("/sw.js");

// sw.js
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((r) => r || fetch(event.request)),
  );
});
```

**Sources:**

[MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)

---

</details>

<details>
  <summary>Web Workers</summary>

### Web Workers

**Definition:**

Web Workers run JavaScript on a background thread so CPU-heavy work does not block the main UI thread.

| Type           | Scope                         |
| -------------- | ----------------------------- |
| Dedicated      | Single owning script          |
| Shared         | Multiple scripts/windows      |
| Service Worker | Proxy between app and network |

**Examples:**

```js
const worker = new Worker("hash-worker.js");
worker.postMessage(largeBuffer);
worker.onmessage = (e) => console.log("hash:", e.data);
```

**Sources:**

[MDN — Web Workers API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API)

---

</details>

<details>
  <summary>Worklets</summary>

### Worklets

**Definition:**

Worklets are lightweight parallel execution contexts for specific tasks like audio processing (`AudioWorklet`) or painting (`PaintWorklet`).

| Worklet          | Purpose                   |
| ---------------- | ------------------------- |
| AudioWorklet     | Real-time audio DSP       |
| PaintWorklet     | Programmatic CSS painting |
| AnimationWorklet | Scroll-linked animations  |

**Examples:**

```js
await audioContext.audioWorklet.addModule("processor.js");
const node = new AudioWorkletNode(audioContext, "gain-processor");
```

**Sources:**

[MDN — Worklet](https://developer.mozilla.org/en-US/docs/Web/API/Worklet)

---

</details>

<details>
  <summary>History API</summary>

### History API

**Definition:**

The History API lets SPAs manipulate the browser URL and history stack without full page reloads.

| Method           | Effect                      |
| ---------------- | --------------------------- |
| `pushState`      | Add history entry           |
| `replaceState`   | Replace current entry       |
| `popstate` event | User navigates back/forward |

**Examples:**

```js
history.pushState({ page: "settings" }, "", "/settings");
window.addEventListener("popstate", (e) => {
  renderPage(e.state?.page ?? "home");
});
```

**Sources:**

[MDN — History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API)

---

</details>

<details>
  <summary>TCP three-way handshake</summary>

### TCP three-way handshake

**Definition:**

TCP opens a connection with SYN → SYN-ACK → ACK so both sides agree on initial sequence numbers before the byte stream.

| Step | Packet  | Meaning                       |
| ---- | ------- | ----------------------------- |
| 1    | SYN     | Client ISN `x`                |
| 2    | SYN-ACK | Server ISN `y`, ack `x + 1`   |
| 3    | ACK     | Client ack `y + 1`; connected |

Full UDP comparison, TFO vs `initcwnd`, `preconnect`, STUN/TURN → [TCP vs UDP](./6_web_api_interview.md#tcp-vs-udp).

**Examples:**

```
Client → SYN (seq=100)
Server → SYN-ACK (seq=300, ack=101)
Client → ACK (ack=301)
→ Data can flow both ways
```

**Sources:**

[MDN — TCP handshake](https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake)

---

</details>

<details>
  <summary>Throttling & debouncing</summary>

### Throttling & debouncing

**Definition:** Rate-limit noisy handlers. Full notes and examples: [Debounce](./3_js_browser_extra.md#debounce) · [Throttle](./3_js_browser_extra.md#throttle).

**Sources:** [MDN Glossary — Debounce](https://developer.mozilla.org/en-US/docs/Glossary/Debounce) · [MDN Glossary — Throttle](https://developer.mozilla.org/en-US/docs/Glossary/Throttle)

</details>

<details>
  <summary>Axios interceptors</summary>

### Axios interceptors

**Definition:**

Axios interceptors are middleware hooks that transform or inspect requests and responses before they reach your application code.

| Interceptor | Typical use                      |
| ----------- | -------------------------------- |
| Request     | Attach auth token, log           |
| Response    | Unwrap data, handle 401 globally |

**Examples:**

```js
import axios from "axios";

axios.interceptors.request.use((config) => {
  config.headers.Authorization = `Bearer ${getToken()}`;
  return config;
});

axios.interceptors.response.use(
  (res) => res,
  (err) => {
    if (err.response?.status === 401) logout();
    return Promise.reject(err);
  },
);
```

**Sources:**

[Axios — Interceptors](https://axios-http.com/docs/interceptors)

---

</details>

<details>
  <summary>Progressive rendering</summary>

### Progressive rendering

**Definition:**

Progressive rendering displays meaningful content before all assets finish loading, improving perceived performance.

| Technique                      | Effect                       |
| ------------------------------ | ---------------------------- |
| Lazy images (`loading="lazy"`) | Defer off-screen images      |
| Critical CSS                   | Inline above-the-fold styles |
| Code splitting                 | Load JS chunks on demand     |

**Examples:**

```html
<img src="hero.jpg" loading="lazy" alt="Hero" />
<link rel="preload" href="font.woff2" as="font" crossorigin />
```

**Sources:**

[web.dev — Fast load times](https://web.dev/explore/fast)

---

</details>

<details>
  <summary>SPA, SSR, PWA & SSG</summary>

### SPA, SSR, PWA & SSG

**Definition:** Full taxonomy (MPA static/dynamic, SPA, CSR, SSR, SSG, PWA, compare) lives in [Types of web apps](./6_web_api_apps.md#12-types-of-web-apps).

**How it works:** SPA = one page, JS routes. CSR = first paint in the browser. SSR = first HTML from the server. SSG = HTML at build. **PWA** = progressive enhancement (install + APIs). Chromium install = HTTPS + manifest; Service Worker is for offline/push, not required for the install prompt. Full notes: [PWA](./6_web_api_apps.md#pwa-progressive-web-app).

**Examples:**

```json
{
  "name": "My App",
  "start_url": "/",
  "display": "standalone",
  "icons": [{ "src": "/icon-192.png", "sizes": "192x192" }]
}
```

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/) · [MDN — Progressive Web Apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)

---

</details>

<details>
  <summary>LCP (Largest Contentful Paint)</summary>

### LCP (Largest Contentful Paint)

**Definition:**

LCP is the time from navigation start until the **largest content element in the viewport** is painted. The browser picks the element (hero image, article cover, large text block, video poster) — you cannot assign it by hand. Inspect it in PageSpeed Insights or the Performance panel.

Good: **≤ 2.5 s**. Poor: **> 4 s**. Field “pass” is p75 (see [Core Web Vitals](./1_html_performance.md#core-web-vitals)). Until that paint, the user cannot tell they landed on the right page; slow LCP raises bounce risk.

**How it works:**

Google splits LCP into four consecutive sub-parts (no gaps). On a **well-optimized** page the _typical_ share is ~40% TTFB, &lt;10% resource load delay, ~40% resource load duration, &lt;10% element render delay — real sites vary; fix the slice that is large in _your_ attribution (`web-vitals/attribution`: `timeToFirstByte`, `resourceLoadDelay`, `resourceLoadDuration`, `elementRenderDelay`).

| Sub-part                   | What it is                                          | Typical share (optimized) |
| -------------------------- | --------------------------------------------------- | ------------------------- |
| **TTFB**                   | Navigation start → first byte of HTML               | ~40%                      |
| **Resource load delay**    | First HTML byte → start of the LCP resource request | &lt;10%                   |
| **Resource load duration** | LCP resource download                               | ~40%                      |
| **Element render delay**   | Resource ready → paint on screen                    | &lt;10%                   |

Text-only LCP (no extra file) has **no** resource delay/duration — only TTFB + render delay.

Extra redirects, slow origin, no CDN/cache on HTML. Details: [TTFB](#ttfb-time-to-first-byte). Until the first byte, the browser cannot parse or discover the LCP URL.

Put the LCP image in HTML as `<img src>` so the preload scanner finds it — not JS-injected, not `data-src`. Do **not** use `loading="lazy"` on it. Set `fetchpriority="high"` on **1–2** resources (in-viewport images otherwise start Low until layout). Cross-origin: `<link rel="preconnect">`, or same origin. CSS `background-image` is discovered only after CSS — prefer `<img>`, or preload:

```html
<link rel="preload" as="image" href="hero.avif" fetchpriority="high" />
<link
  rel="preload"
  as="image"
  imagesrcset="hero-800.avif 800w, hero-1600.avif 1600w"
  imagesizes="100vw"
  fetchpriority="high"
/>
```

Next.js: `priority` on `next/image` disables lazy loading and raises fetch priority; in **Next.js 16** prefer `preload` (`priority` is deprecated). → [4_nextjs.md](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).

Photos → raster **AVIF → WebP → JPEG** via `<picture>`, not SVG. Serve `srcset`/`sizes`. Image CDNs can pick format/size from Client Hints / UA. Long `Cache-Control` on static files; sort/strip cache-busting query junk; avoid `Set-Cookie` on cacheable responses. Demote competing work: `fetchpriority="low"` on non-LCP carousel slides; `defer`/`async` third-party scripts. Tiny icons: `data:` URL or **inline `<svg>`** (SVGO); base64 is ~⅓ larger and shares the document cache — not for heroes. Field data often shows TTFB/delays dominate — check before encoding marathons.

Shrink render-blocking CSS (critical inline, Coverage for unused). Avoid sync JS in `<head>` (`defer`/`async` or end of `body`). SSR/SSG so the LCP node is in the first HTML. Long Tasks after the image arrives delay paint. Text LCP: `font-display: swap` + preload the critical font.

Async non-critical CSS (FOUC risk):

```html
<link
  rel="preload"
  href="non-critical.css"
  as="style"
  onload="this.onload=null;this.rel='stylesheet'"
/>
<noscript><link rel="stylesheet" href="non-critical.css" /></noscript>
```

[Speculation Rules](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/speculationrules) can prerender a likely next URL so LCP on that navigation is near-zero (similar to **bfcache** restore). Chromium-only at writing; Safari/Firefox lag. `eagerness`: `immediate` / `eager` / `moderate` / `conservative` — earlier speculation wastes more if the user never clicks.

```html
<script type="speculationrules">
  {
    "prerender": [
      { "where": { "href_matches": "/*" }, "eagerness": "moderate" }
    ]
  }
</script>
```

**When to use:**

Product hero, article cover, marketing landing. Confirm the weak slice with CrUX / attribution before shrinking files “just in case.”

#### Common pitfalls

- `loading="lazy"` or JS/`data-src` on the hero (preload scanner never sees a real `src`).
- `fetchpriority="high"` on many assets (the signal becomes noise).
- CSS-only hero with no `preload`.
- Preloading everything and starving the real LCP request.
- Optimizing file size when attribution shows TTFB or render delay.

**Pluses and minuses:**

| +                                       | −                                         |
| --------------------------------------- | ----------------------------------------- |
| Maps to a visible “main content” moment | LCP element changes with viewport / cache |
| Four named slices for tickets           | Lab cold cache ≠ field warm cache         |

**Examples:**

```html
<img
  src="hero.avif"
  alt="Cover"
  width="1200"
  height="630"
  fetchpriority="high"
/>

<picture>
  <source
    type="image/avif"
    srcset="hero-640.avif 640w, hero-1280.avif 1280w"
    sizes="100vw"
  />
  <source
    type="image/webp"
    srcset="hero-640.webp 640w, hero-1280.webp 1280w"
    sizes="100vw"
  />
  <img
    src="hero-1280.jpg"
    srcset="hero-640.jpg 640w, hero-1280.jpg 1280w"
    sizes="100vw"
    alt="Cover"
    width="1280"
    height="720"
    fetchpriority="high"
  />
</picture>
```

**Thoughts:** You do not pick the LCP node — you make whatever the browser chose appear fast by cutting TTFB, discovery delay, bytes, then main-thread/CSS/font wait.

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [TTFB](#ttfb-time-to-first-byte) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · HTML → [1_html.md](./1_html_performance.md#core-web-vitals--html-developer-impact) · [img](./1_html_media.md#img-element-and-responsive-images) · [DevTools](./11_vsc_browser.md#devtools--core-web-vitals)

**Sources:**

[web.dev — Optimize LCP](https://web.dev/articles/optimize-lcp) · [web.dev — LCP](https://web.dev/articles/lcp) · [web.dev — Fetch Priority](https://web.dev/articles/fetch-priority)

---

</details>

<details>
  <summary>First Contentful Paint (FCP)</summary>

### First Contentful Paint (FCP)

**Definition:** FCP is the time from navigation start until the browser paints the first **content**: text, an image, SVG, or a non-empty `<canvas>`. It means “the blank screen ended,” not “the page is ready.”

**How it works:**

- FCP is a **Web Vital** used to diagnose load. It is **not** a Core Web Vital (those are LCP, INP, CLS) and does **not** directly affect ranking.
- A slow FCP usually means a slow **LCP** as well: both wait on the same early path — [TTFB](#ttfb-time-to-first-byte) and **render-blocking** CSS/JS before first paint.
- Score at the **75th percentile**, mobile and desktop separately. **Good:** ≤ 1.8 s. **Needs improvement:** 1.8–3 s. **Poor:** > 3 s.
- A good FCP can still leave the user waiting: the main content (**LCP**) and input snappiness (**INP**) may arrive much later.

- Cut render-blocking CSS/JS; minify CSS; drop unused CSS.
- Lower TTFB; add `<link rel="preconnect">` (DNS + TCP + TLS); remove extra redirects.
- `<link rel="preload">` for files needed for first paint; optimize fonts (`font-display`, subset unused glyphs).
- Smaller page weight; HTTP caching.

**When to use:**

Long white screen. When LCP is bad, FCP tells you “nothing painted yet” vs “wrong largest element.” Same early levers as LCP/TTFB.

#### Common pitfalls

- Calling FCP a Core Web Vital or treating it as “page ready.”
- Optimizing a splash logo so FCP looks good while LCP stays late.

**Pluses and minuses:**

| +                                 | −                                        |
| --------------------------------- | ---------------------------------------- |
| Earliest user-visible load signal | Not Core Web Vitals / not ranking        |
| Shares the critical path with LCP | First logo or line of text ≠ usable page |

**Examples:**

```js
import { onFCP } from "web-vitals";
onFCP(console.log);
```

Prefer `web-vitals` over a raw `PerformanceObserver` on `paint`: field FCP also handles background tabs, bfcache restores, iframes, and prerender (`activationStart`).

**Thoughts:** FCP = first content pixels. Fix TTFB and blocking resources; still measure LCP and INP.

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [TTFB](#ttfb-time-to-first-byte) · [INP](#inp-interaction-to-next-paint) · [preload, prefetch, preconnect](#preload-prefetch-preconnect--prerender) · [HTML — first paint](./1_html_rendering.md#first-render--full-pipeline)

**Sources:**

[web.dev — FCP](https://web.dev/articles/fcp) · [web.dev — Vitals](https://web.dev/articles/vitals)

---

</details>

<details>
  <summary>TTFB (Time to First Byte)</summary>

### TTFB (Time to First Byte)

**Definition:** TTFB is the time from starting navigation until the browser receives the **first byte** of the response. It is a **diagnostic** metric (not a Core Web Vital) for connection setup and origin/CDN responsiveness.

**How it works:**

TTFB is the sum of:

- redirects (each 301/302 is another round trip);
- service worker startup, if one controls the page;
- DNS lookup;
- TCP connect + TLS handshake;
- server work until the first response byte.

Until that byte arrives, the browser has nothing to parse, so **FCP and LCP wait**. A high TTFB often drags other metrics down. A **low** TTFB does **not** guarantee good LCP/FCP — resources, JS, and paint still follow.

| Rating ([web.dev](https://web.dev/articles/optimize-ttfb)) | TTFB      |
| ---------------------------------------------------------- | --------- |
| Good                                                       | ≤ 0.8 s   |
| Needs improvement                                          | 0.8–1.8 s |
| Poor                                                       | > 1.8 s   |

Measure in PageSpeed Insights, DevTools **Network** (Waiting / TTFB), `web-vitals` `onTTFB`, or field data via **CrUX**. Typical causes: **network** (distance, no CDN) or **origin** (HTML render, DB, third-party APIs).

**Network**

- CDN for static assets and, when possible, HTML — usually also HTTP/2, HTTP/3, TLS 1.3. See [CDN](./6_web_api_core.md#cdn-content-delivery-network).
- Cache HTML with `Cache-Control`; invalidate on deploy; avoid unique query strings that bust CDN/shared caches. See [HTTP caching](./6_web_api_http.md#http-caching).
- Drop redirects (correct scheme and trailing slash). `Strict-Transport-Security` removes HTTP→HTTPS on **repeat** visits; first visit still redirects unless the host is on the **HSTS preload list**. See [SSL/TLS handshake](./6_web_api_http.md#ssltls--https-handshake).

**Server**

- Stream HTML (`renderToPipeableStream` + `<Suspense>` in React) so the browser can parse chunks while slow data is still loading. See [system design — streaming SSR](./0_system_design_architecture.md#rendering-strategies-when-html-is-built).
- Next.js: SSG / [ISR](./4_nextjs_rendering.md#incremental-static-regeneration-isr) for pages that do not need per-visitor HTML — then a CDN can serve it. Levers: [Core Web Vitals on Vercel](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).
- Service worker **stale-while-revalidate** can return an app shell from cache immediately; **do not** use it for per-request personalized HTML (it would serve a stale document). See [Service Workers](#service-workers).
- **103 Early Hints:** while the origin prepares the final response, send `Link` hints so the browser can start critical assets. Some CDNs (e.g. Cloudflare) emit 103 before origin HTML exists. Adoption is still uneven.
- Origin health (memory, runtime). Break down origin time with `Server-Timing`; those marks show up in the browser via **Navigation Timing** (`PerformanceNavigationTiming`).

**When to use:**

Slow LCP with a late document; users far from origin; SSR that waits on DB/API before sending any HTML.

#### Common pitfalls

Unique analytics query params bypassing CDN cache. Extra 301/302 hops. Edge SSR next to a distant database (worse TTFB than regional Node). Assuming cached personalized HTML is safe. Treating TTFB as a substitute for LCP.

**Pluses and minuses:**

| +                                            | −                                                                          |
| -------------------------------------------- | -------------------------------------------------------------------------- |
| Explains stalled FCP/LCP before parse        | Not a Core Web Vital; chasing TTFB alone misses JS/images                  |
| Attribution splits DNS vs connect vs request | 103 Early Hints can make “first byte” the interim response, not final HTML |

**Examples:**

```js
import { onTTFB } from "web-vitals";

onTTFB((metric) => {
  console.log("TTFB:", metric.value, "ms", metric.rating);
  const nav = metric.entries[0];
  if (nav) {
    console.log("DNS:", nav.domainLookupEnd - nav.domainLookupStart);
    console.log("Connect:", nav.connectEnd - nav.connectStart);
    console.log("Request→first byte:", nav.responseStart - nav.requestStart);
  }
});
```

```http
Server-Timing: db;dur=53, app;dur=47.2;desc="render"
103 Early Hints
Link: </hero.webp>; rel=preload; as=image
```

**Thoughts:** First byte unblocks parse. Fix CDN, redirects, and origin work — then still optimize LCP after HTML starts.

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · [CDN](./6_web_api_core.md#cdn-content-delivery-network)

**Sources:**

[web.dev — TTFB](https://web.dev/articles/ttfb) · [web.dev — Optimize TTFB](https://web.dev/articles/optimize-ttfb) · [web-vitals `onTTFB`](https://github.com/GoogleChrome/web-vitals) · [MDN — Server-Timing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server-Timing) · [MDN — 103 Early Hints](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/103)

---

</details>

<details>
  <summary>INP (Interaction to Next Paint)</summary>

### INP (Interaction to Next Paint)

**Definition:**

Field metric for responsiveness: time from a click, tap, or key press until the browser paints the next frame that can show a visible response. **Good** ≤ 200 ms, **needs improvement** ≤ 500 ms, **poor** > 500 ms — at the **75th percentile** of page views, mobile and desktop separately.

INP does **not** wait for network or later async UI. It measures how long the **next paint is blocked**. Slow feedback looks like the action failed (double-add to cart, abandoned checkout).

**How it works:**

INP observes **all** qualifying interactions for the page lifetime. The value is usually the **worst** interaction, ignoring outliers (one highest dropped per 50 interactions). Hover, scroll, and zoom do **not** count unless they also include a click/tap/key. Iframe interactions count for users (CrUX) but JS RUM often cannot see cross-origin iframes.

An interaction is one gesture (`pointerdown` + `click`, or `keydown` + `keyup`). Latency is the longest event in that group, from start until the next paint is possible.

Three phases (sum = interaction latency):

| Phase                   | What                               | Typical cause                            |
| ----------------------- | ---------------------------------- | ---------------------------------------- |
| **Input delay**         | User action → first handler starts | Main thread busy (JS, Long Tasks)        |
| **Processing duration** | Handlers + other sync work         | Heavy click handlers, large React commit |
| **Presentation delay**  | Handlers done → updated frame      | Layout/paint, third-party JS, huge DOM   |

FID (Core Vital until **March 2024**) measured only the **first** interaction and only **input delay**. INP replaced it: all clicks/taps/keys, full path to next paint. Lab Lighthouse with no user reports **TBT**, not INP.

What makes INP worse:

- **Long Tasks** (> 50 ms) on the main thread.
- Heavy handlers / large client DOM updates.
- **Layout thrashing** — write styles → read `offsetHeight` → write again (forced sync layout).
- Large / deep DOM (Lighthouse warns ~800 nodes, severe ~1400).
- Client `innerHTML` of big markup (one long parse/insert; preload scanner misses nested resources → also LCP).

`useTransition` / `useDeferredValue` mark heavy updates as non-urgent. Split hydration with `<Suspense>`; RSC skip hydration. React **19.2** `<Activity mode="hidden">` keeps state/DOM, tears down Effects, hides with `display: none`. Details — [4_react.md](./4_react_ecosystem.md#web-vitals-in-react-apps).

**Yield long work** after paint-critical UI. `scheduler.yield()` (Chrome 129+, Firefox 142+; **not Safari**) continues with Scheduler priority; `setTimeout(0)` goes to the end of the macrotask queue. Do **not** rely on `navigator.scheduling.isInputPending()` (can be `false` after a click).

```js
function yieldToMain() {
  if (globalThis.scheduler?.yield) return scheduler.yield();
  return new Promise((resolve) => setTimeout(resolve, 0));
}

async function saveSettings() {
  validateForm();
  showSpinner();
  updateUI();
  await yieldToMain();
  saveToLocalStorage();
  sendAnalytics();
}

async function runJobs(jobQueue, deadline = 50) {
  let lastYield = performance.now();
  for (const job of jobQueue) {
    job();
    if (performance.now() - lastYield > deadline) {
      await yieldToMain();
      lastYield = performance.now();
    }
  }
}
```

Web Workers for **pure** CPU (sort, parse) — no DOM. Comlink wraps `postMessage`; large payloads: transfer `ArrayBuffer`.

often dominate presentation delay. Workers cannot run GTM as-is (needs DOM). **Partytown** proxies DOM to the main thread; Next `next/script` `strategy="worker"` needs `experimental.nextScriptWorkers`. Prefer `lazyOnload`; drop unused tags. Tag cost: pixels (HTTP only) < custom templates < Custom HTML (layout). Keep the GTM container small (hard cap 300 KB). Facade for embeds (`lite-youtube-embed`); `loading="lazy"` + `width`/`height` on iframes (CLS).

**When to use:**

Search, cart, checkout, filters, autocomplete. Measure field first (`web-vitals` attribution: `inputDelay`, `processingDuration`, `presentationDelay`).

**Fix order — ship less JS first.** Parse + execute happens on the main thread. Big bundles hurt INP and can delay LCP.

1. **Measure** — `webpack-bundle-analyzer` / `@next/bundle-analyzer`.
2. **Code split** — per route; `import()` / `React.lazy` / `next/dynamic` for charts, editors, modals. See [10_build.md](./10_build_bundlers.md#code-splitting).
3. **Tree-shake** — named ESM (`lodash-es`, `date-fns`), not `import _ from 'lodash'` / Moment. [10_build.md](./10_build_bundlers.md#tree-shaking).
4. **Barrel files** — `export *` from `index.ts` can pull a whole UI kit. Next: [`optimizePackageImports`](./4_nextjs_optimizations.md#optimizepackageimports) (Next docs still mark it experimental; Turbopack often analyzes imports itself).
5. **Server work** — RSC instead of client Markdown, etc. Best JS is JS never sent. Then lighter libs, Brotli, CI bundle budget.

Other techniques:

- Debounce `input`; throttle `scroll`/`resize`.
- Abort stale `fetch` with `AbortController` — [AbortController](./3_js_async_apis.md#abortcontroller).
- Prefer one-shot `setTimeout` over leftover `setInterval`; move heavy timer work to a Worker.
- Animate `transform`/`opacity` in CSS; JS on `width`/`top`/`box-shadow` forces layout — [2_css.md](./2_css.md).
- Batch DOM: **all reads, then all writes**.
- Smaller DOM: fragments, flatter trees, don’t render hidden tabs until needed; `content-visibility: auto` — [2_css.md](./2_css_architecture.md#contain--content-visibility).
- Cheaper selectors (long chains cost more style recalc on a big DOM).
- Smaller JS files (~100 KB as a rule of thumb); `type="module"` parses as separate chunks so input can run between them.
- `touch-action: manipulation` on buttons — skips the historical ~300 ms double-tap-zoom wait.

#### Common pitfalls

Yield/Workers before cutting unused JS. Treating FID or TBT as INP. Counting scroll/hover as INP. `isInputPending()` instead of yielding. GTM Custom HTML for above-the-fold UI. Duplicate tags in HTML **and** the tag manager.

**Pluses and minuses:**

| +                                          | −                                           |
| ------------------------------------------ | ------------------------------------------- |
| Matches post-load UX (most of the session) | No INP if nobody clicked/tapped/typed       |
| Attribution names control + phase          | Lab INP depends on which clicks you perform |
| Shared language with SEO                   | Scroll/hover jank is out of scope           |

**Examples:**

```tsx
import dynamic from "next/dynamic";

const HeavyChart = dynamic(() => import("./heavy-chart"), { ssr: false });
```

```js
let controller;
input.addEventListener("input", async (e) => {
  const query = e.target.value.trim();
  controller?.abort();
  if (!query) return;
  controller = new AbortController();
  const res = await fetch(`/search?q=${encodeURIComponent(query)}`, {
    signal: controller.signal,
  });
});
```

```js
// bad — layout each iteration
boxes.forEach((box) => {
  const height = box.offsetHeight;
  box.style.height = `${height * 2}px`;
});
// good — one layout
const heights = boxes.map((box) => box.offsetHeight);
boxes.forEach((box, i) => {
  box.style.height = `${heights[i] * 2}px`;
});
```

**Thoughts:** Paint a response **now** (spinner, optimistic UI); keep the main thread free of long JS.

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [4_react.md — Web Vitals](./4_react_ecosystem.md#web-vitals-in-react-apps) · [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel)

**Sources:**

[web.dev — INP](https://web.dev/articles/inp) · [web.dev — Optimize long tasks](https://web.dev/articles/optimize-long-tasks) · [MDN — Scheduler.yield](https://developer.mozilla.org/en-US/docs/Web/API/Scheduler/yield) · [React `<Activity>`](https://react.dev/reference/react/Activity)

---

</details>

<details>
  <summary>preload, prefetch, preconnect & prerender</summary>

### preload, prefetch, preconnect & prerender

**Definition:**

Resource hints tell the browser to fetch or connect early, speeding up current or future navigation.

| Hint         | When              | Purpose                          |
| ------------ | ----------------- | -------------------------------- |
| `preload`    | Current page      | Critical resource needed now     |
| `prefetch`   | Future navigation | Likely next page resource        |
| `preconnect` | Early             | DNS + TCP + TLS to origin        |
| `prerender`  | Future page       | Render entire page in background |

**Examples:**

```html
<link rel="preload" href="/app.js" as="script" />
<link rel="prefetch" href="/next-page.js" as="script" />
<link rel="preconnect" href="https://fonts.googleapis.com" />
```

**Sources:**

[MDN — rel=preload](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/preload)

---

</details>

<details>
  <summary>PRPL pattern</summary>

### PRPL pattern

**Definition:**

PRPL (Push, Render, Pre-cache, Lazy-load) is a performance pattern for fast interactive web apps.

| Step      | Goal                           |
| --------- | ------------------------------ |
| Push      | Send critical resources early  |
| Render    | Show content ASAP              |
| Pre-cache | Cache likely-needed assets     |
| Lazy-load | Defer non-critical code/assets |

**Examples:**

```html
<!-- Push critical route -->
<link rel="preload" href="/route-home.js" as="script" />
<!-- Lazy-load below-fold images -->
<img loading="lazy" src="/gallery/photo-9.jpg" />
```

**Sources:**

[web.dev — PRPL pattern](https://web.dev/articles/apply-instant-loading-with-prpl)

---

---

</details>

<a name="api-and-client-performance-optimization"></a>

<details>
  <summary>API and client performance optimization</summary>

### API and client performance optimization

**Definition:** End-to-end speed work on **backend APIs** (latency, throughput, payload size) and the **client** (bundle, main-thread work, perceived load). Measure in production first; optimize the slowest layer in the critical path.

**How it works:**

| Layer               | Typical bottlenecks                               | Levers                                                                                                                      |
| ------------------- | ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **API / origin**    | Slow DB, N+1 queries, cold starts, oversized JSON | Indexes, eager loading, pagination/cursors, field selection, compression, cache headers, Redis, async jobs, `Server-Timing` |
| **Network**         | Waterfalls, too many round trips, no CDN          | HTTP/2+, parallel fetches, BFF/aggregated endpoints, CDN/edge cache, `preconnect` / `preload`                               |
| **Client runtime**  | Long tasks, re-renders, huge lists                | Code splitting, lazy routes/components, memo/virtualization, defer third-party JS                                           |
| **Load experience** | Late LCP/FCP, high INP/CLS                        | SSR/SSG/RSC, optimized images/fonts, stable layout, break up work (`scheduler.yield`)                                       |

Workflow: **profile** (RUM + APM/traces + Lighthouse/DevTools) → find p95/p99 → fix one bottleneck → **regression guard** (budgets, CI Lighthouse, load tests).

**When to use:** JD bullets like “optimize API and client performance”, slow dashboards, high TTFB, poor Core Web Vitals in Search Console, or API p99 above SLO.

**Pluses and minuses:**

| Approach                      | Pro                           | Con                                 |
| ----------------------------- | ----------------------------- | ----------------------------------- |
| Cache (HTTP/Redis/CDN)        | Huge win on read-heavy APIs   | Stale data, invalidation complexity |
| Pagination + sparse JSON      | Smaller payloads, faster TTFB | More client logic                   |
| Client memo/code split        | Smoother INP, smaller bundles | Harder to debug, easy to over-memo  |
| Premature micro-optimizations | —                             | Wasted time without metrics         |

**Thoughts:** Client CWV and API latency are linked — slow TTFB blocks FCP/LCP; heavy client JS hurts INP even when the API is fast. Full React/Next/Vue client playbooks → [§10 Performance](./4_react_ecosystem.md#10-performance) · [Next optimizations](./4_nextjs_optimizations.md) · DB/query tuning → [N+1](./9_database_orm.md#n1-problem) · [Connection pooling](./9_database_orm.md#connection-pooling) · Node stack → [8_nodejs interview Q29](./8_nodejs_interview.md#interview-practice).

**Sources:** [web.dev — Vitals](https://web.dev/articles/vitals) · [web.dev — Optimize TTFB](https://web.dev/articles/optimize-ttfb) · [MDN — Server-Timing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server-Timing) · [web.dev — Optimize long tasks](https://web.dev/articles/optimize-long-tasks)

---

</details>
