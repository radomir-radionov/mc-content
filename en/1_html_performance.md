# HTML — Performance

Script loading, resource hints, Core Web Vitals, and FOUC.

[← Back to hub](./1_html.md)

### Related docs

- [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Performance & resource loading](#performance-resource-loading)

---

## Performance & resource loading

<details>
  <summary>async vs defer vs module</summary>

### async vs defer vs module

**Definition:**

Script loading attributes control when JavaScript downloads and executes relative to HTML parsing.

| Attribute       | Download      | Execute     | Order                  |
| --------------- | ------------- | ----------- | ---------------------- |
| (none)          | Blocks parser | Immediately | Sequential             |
| `defer`         | Parallel      | After parse | Preserved              |
| `async`         | Parallel      | When ready  | Not guaranteed         |
| `type="module"` | Parallel      | After parse | Preserved; strict mode |

**How it works:**

- Default `<script>` blocks HTML parse during download + execute.
- `defer` — parallel download, execute in order after parse.
- `async` — parallel download, execute when ready (unordered).
- `type="module"` — deferred, strict, `import`/`export`.

| Attribute | Order          | Blocks parse?            |
| --------- | -------------- | ------------------------ |
| `defer`   | Preserved      | No (execute after parse) |
| `async`   | Not guaranteed | At execute time          |
| `module`  | Preserved      | No                       |

**When to use:**

App bundle → `defer` · analytics → `async` · ES app → `module`.

#### Common pitfalls

- Two `async` scripts with dependencies.
- Module scripts need CORS on cross-origin `src`.
- Inline script assuming deferred external already ran.

**Examples:**

```html
<script src="app.js" defer></script>
<script src="analytics.js" async></script>
<script type="module" src="app.mjs"></script>
```

**Thoughts:** Full script loading guide → [3_js_browser.md](./3_js_browser.md).

**Sources:** [MDN — Script loading](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)

</details>

<details>
  <summary>Page load optimization (HTML-level)</summary>

### Page load optimization (HTML-level)

**Definition:**

HTML-level optimizations reduce transfer size, prevent layout shift, and avoid parser-blocking resources.

| Technique                  | Benefit                   |
| -------------------------- | ------------------------- |
| `srcset` / modern formats  | Smaller images            |
| `width`/`height` on images | Prevent CLS               |
| `defer` / `module` scripts | Non-blocking JS           |
| Resource hints             | Faster critical resources |

**How it works:**

- Minimize render-blocking resources in critical path.
- Preload LCP assets; lazy-load below-fold images.
- `defer`/`module` for app scripts; `async` for independent tags.

| Technique               | Benefit               |
| ----------------------- | --------------------- |
| `srcset`/modern formats | Smaller images        |
| `defer` scripts         | Non-blocking JS       |
| Resource hints          | Faster critical fetch |

**When to use:**

Landing page LCP · blog with many images · third-party script budgeting.

#### Common pitfalls

- Preloading too many assets.
- Render-blocking CSS at bottom.
- Third-party scripts without `async`/`defer`.

**Examples:**

```html
<link rel="preconnect" href="https://cdn.example.com" />
<img src="photo.webp" alt="Photo" width="600" height="400" loading="lazy" />
<script src="/app.js" defer></script>
```

**Sources:** [web.dev — Performance](https://web.dev/explore/performance)

</details>

<details>
  <summary>Core Web Vitals</summary>

### Core Web Vitals

**Definition:**

Google’s page-experience metrics (2020+). **Core** set is **LCP** (main content painted), **INP** (click/tap/key → next paint), **CLS** (unexpected layout jump). A URL **passes** when **all three** are “good” at **p75**, **mobile and desktop scored separately**. **FCP** and **TTFB** are diagnostic, not Core, not ranking.

| Metric   | Question                                          | 🟢 Good  | 🟡 Needs improvement | 🔴 Poor  |
| -------- | ------------------------------------------------- | -------- | -------------------- | -------- |
| **LCP**  | When did I see the main content?                  | ≤ 2.5 s  | 2.5–4.0 s            | > 4.0 s  |
| **INP**  | After I pressed, did the UI update soon?          | ≤ 200 ms | 200–500 ms           | > 500 ms |
| **CLS**  | Did things jump so I might tap the wrong control? | ≤ 0.1    | 0.1–0.25             | > 0.25   |
| **FCP**  | When did _anything_ appear?                       | ≤ 1.8 s  | 1.8–3.0 s            | > 3.0 s  |
| **TTFB** | How soon did the server start the response?       | ≤ 0.8 s  | 0.8–1.8 s            | > 1.8 s  |

Thresholds are the same on phone and desktop; **samples are not**. Ranking uses **CrUX field** data (28-day window), not a lab Lighthouse run. **CLS** is unitless; others are time. **FID** was Core until **12 March 2024** — **INP** replaced it (all clicks/taps/keys, full path to paint). Lighthouse with no user reports **TBT**, not INP.

|       | **Lab**                          | **Field** (RUM)                               |
| ----- | -------------------------------- | --------------------------------------------- |
| Tools | Lighthouse, PSI lab, Performance | CrUX, PSI field, Search Console, `web-vitals` |
| Use   | Reproduce _what to fix_          | p75 of real visits — _what ranking uses_      |

Lab ≠ field: INP needs real input; CLS in the field includes scroll/banners; LCP element and cache differ. Workflow: field first → lab slice (not “make LCP better”) → re-check all three. Slow hero → [LCP](#lcp-largest-contentful-paint); dead buttons → [INP](#inp-interaction-to-next-paint); jump → [CLS](#cls-cumulative-layout-shift); blank screen → [FCP](#first-contentful-paint-fcp) / [TTFB](#ttfb-time-to-first-byte). Start **mobile**, high-traffic URLs.

**When to use:**

SEO ranking, incident triage, Lighthouse CI budgets, tickets with a numeric p75 target.

**Example** — field (`web-vitals`). Flush on `visibilitychange` → `hidden` via `sendBeacon` (`unload` breaks **bfcache**). Report **p75**, not the average.

```js
import { onCLS, onINP, onLCP } from "web-vitals";

function send(metric) {
  navigator.sendBeacon("/analytics", JSON.stringify(metric));
}

onCLS(send);
onINP(send);
onLCP(send);
```

```js
import { onLCP, onINP, onCLS } from "web-vitals/attribution";
onLCP(({ attribution }) => console.log(attribution)); // TTFB, load delay/duration, render delay
onINP(({ attribution }) =>
  console.log(attribution.inputDelay, attribution.processingDuration),
);
onCLS(({ attribution }) => console.log(attribution.largestShiftTarget));
```

Next.js: [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel). Metric deep-dives: [LCP](#lcp-largest-contentful-paint) · [INP](#inp-interaction-to-next-paint) · [CLS](#cls-cumulative-layout-shift). DevTools: [11_vsc_browser.md](./11_vsc_browser.md#devtools--core-web-vitals).

#### Common pitfalls

Trusting Lighthouse while Search Console is red. Expecting CrUX next day. Optimizing the **mean**. Calling **FID** Core. Treating **TBT** as INP. Preloading everything. Analytics on `unload`.

**Pluses and minuses:**

| +                                    | −                                    |
| ------------------------------------ | ------------------------------------ |
| Shared language with SEO             | Lab green ≠ Search Console green     |
| Attribution names the element/script | 28-day CrUX lag; mean hides the tail |

**Thoughts:** Lab = how to reproduce; field p75 = whether users (and ranking) care.

**See also:**

[LCP](#lcp-largest-contentful-paint) · [FCP](#first-contentful-paint-fcp) · [TTFB](#ttfb-time-to-first-byte) · [INP](#inp-interaction-to-next-paint) · [Next.js CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel)

---

**Sources:** [web.dev — Vitals](https://web.dev/articles/vitals) · [web.dev — INP](https://web.dev/articles/inp) · [Search — CWV](https://developers.google.com/search/docs/appearance/core-web-vitals) · [web-vitals](https://github.com/GoogleChrome/web-vitals)

</details>

<details>
  <summary>LCP (Largest Contentful Paint)</summary>

### LCP (Largest Contentful Paint)

**Definition:**

LCP is the time from navigation start until the **largest content element in the viewport** is painted. The browser picks the element (hero image, article cover, large text block, video poster) — you cannot assign it by hand. Inspect it in PageSpeed Insights or the Performance panel.

Good: **≤ 2.5 s**. Poor: **> 4 s**. Field “pass” is p75 (see [Core Web Vitals](#core-web-vitals)). Until that paint, the user cannot tell they landed on the right page; slow LCP raises bounce risk.

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

**See also:** [Core Web Vitals](#core-web-vitals) · [TTFB](#ttfb-time-to-first-byte) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · [img](./1_html_media.md#img-element-and-responsive-images) · [DevTools](./11_vsc_browser.md#devtools--core-web-vitals)

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

**See also:** [Core Web Vitals](#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [TTFB](#ttfb-time-to-first-byte) · [INP](#inp-interaction-to-next-paint) · [preload, prefetch, preconnect](./6_web_api_performance.md#preload-prefetch-preconnect--prerender) · [HTML — first paint](./1_html_rendering.md#first-render--full-pipeline)

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
- Service worker **stale-while-revalidate** can return an app shell from cache immediately; **do not** use it for per-request personalized HTML (it would serve a stale document). See [Service Workers](./6_web_api_performance.md#service-workers).
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

**See also:** [Core Web Vitals](#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · [CDN](./6_web_api_core.md#cdn-content-delivery-network)

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

**See also:** [Core Web Vitals](#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [4_react.md — Web Vitals](./4_react_ecosystem.md#web-vitals-in-react-apps) · [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel)

**Sources:**

[web.dev — INP](https://web.dev/articles/inp) · [web.dev — Optimize long tasks](https://web.dev/articles/optimize-long-tasks) · [MDN — Scheduler.yield](https://developer.mozilla.org/en-US/docs/Web/API/Scheduler/yield) · [React `<Activity>`](https://react.dev/reference/react/Activity)

---

</details>

</details>

<details>
  <summary>CLS (Cumulative Layout Shift)</summary>

### CLS (Cumulative Layout Shift)

**Definition:** CLS is the Core Web Vital for visual stability: how much already-visible content unexpectedly moves during (and after) load. Good **≤ 0.1**, needs improvement **0.1–0.25**, poor **> 0.25** (75th percentile).

**How it works:**

- A **layout shift** is recorded when a visible element changes position between frames without a user-driven reason. Classic case: you aim at “Buy”, a banner loads above, content jumps, you tap “Cancel”.
- Score of one shift: **impact fraction × distance fraction** — viewport share that shifted × how far it moved (as a viewport fraction). Bigger area + longer travel = worse.
- Shifts group into **session windows**: a new window starts if the gap between shifts is **≥ 1s**; a window lasts at most **5s**. Page CLS is the **worst window**, not the sum of every shift on a long-lived tab.
- **Expected** shifts are excluded: discrete input (click/tap) has a **500ms** grace (`hadRecentInput`). Continuous gestures (scroll, pinch) do **not** get that exclusion.
- Lab (Lighthouse) ≠ field (CrUX). Debug: DevTools **Performance → Layout Shifts**. Measurement / RUM: [Core Web Vitals](#core-web-vitals).

- Images/video without `width`/`height` (or CSS `aspect-ratio`) — laid out as empty, then neighbors jump when the file arrives.
- Ads, widgets, iframes with unknown height, especially async.
- Inserts **above** already-visible content: cookie banners, recommendation rails, error toasts at render. Same-size insert near the **top** of the viewport scores worse than one near the bottom.
- Web fonts: fallback metrics ≠ custom metrics → reflow. **FOIT** (text hidden until the font loads) vs **FOUT** (fallback text, then swap). CSS: [Font loading](./2_css_visual.md#font-loading).
- Animating `top` / `left` / `margin` / `width` instead of `transform` — layout recalc can move siblings. CSS: [transform](./2_css_motion.md#transform).

- Always a generic fallback: `"Google Sans", sans-serif` — never a lone custom family (browser-default metrics diverge more).
- Match fallback to the webfont with `@font-face` **`size-adjust`**, `ascent-override`, `descent-override`, `line-gap-override`.
- Preload critical fonts (`<link rel="preload" as="font" … crossorigin>`). Prefer **WOFF2**, variable fonts, `unicode-range` subsets (Cyrillic-only download for Cyrillic text).
- `next/font` self-hosts (no Google CDN hop) and tunes fallback `size-adjust`.

```tsx
import { Geist } from "next/font/google";

const geist = Geist({ subsets: ["latin"] });

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html className={geist.className}>
      <body>{children}</body>
    </html>
  );
}
```

- Container `min-height` or `aspect-ratio` for the **likely** slot; media queries for breakpoint sizes. `<iframe width height>` when known.
- Cookie banners: reserve the slot (or overlay). On mobile they can also become the **LCP** element. `preconnect` / `dns-prefetch` if third-party.
- Do not inject above the viewport without a user gesture. Prefer below-the-fold, overlay, or a fixed-size slot.
- Infinite feed: swap inside a **fixed-size** carousel; prefetch **before** the user reaches the end (500ms post-interaction exclusion); or **“Load more”** so the change is expected.

animate `transform` (`translate` / `scale` / `rotate`) and `opacity`. Those run after layout.

back/forward cache restores the whole page instantly; LCP/FCP/CLS on that navigation are often ~0. web.dev: ~**10%** of desktop and ~**20%** of mobile navigations are back/forward. Common blockers: `unload` (use `pagehide`) and `Cache-Control: no-store`. DevTools → Application → Back/forward cache → Test. [11_vsc_browser.md](./11_vsc_browser.md#devtools--core-web-vitals).

**When to use:**

Late media, third-party embeds, consent UI, infinite scroll, custom fonts.

#### Common pitfalls

- Treating CLS as a sum of all shifts (it is the **max session window**).
- Counting shifts after a click as unexpected (500ms exclusion).
- In-flow cookie bar on mobile (CLS **and** LCP).
- `font-family` without a generic fallback; `font-display: block` FOIT.
- Auto-appending infinite-scroll items in view without reserved space.

**Pluses and minuses:**

| +                                                         | −                                                               |
| --------------------------------------------------------- | --------------------------------------------------------------- |
| Stable tap targets, fewer misclicks                       | Cookie/ad slots must exist before first paint                   |
| Reserving space is cheaper than chasing a bad field score | Overlay vs in-flow: overlays avoid CLS but can hide LCP/content |

**Examples:**

```html
<img src="cat.jpg" alt="Cat" width="800" height="600" />
<style>
  img {
    height: auto;
    width: 100%;
  }
</style>
```

`srcset` / `<picture>`: keep the **same aspect ratio** across candidates; set `width`/`height` on `<img>` (and on `<source>` in modern browsers). Next.js `<Image>` requires `width`+`height` or `fill`; static import fills dimensions. Framework notes: [4_nextjs.md](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).

**Thoughts:** Reserve space **before** late content arrives; do not shove new boxes into the current viewport unless the user just asked for them.

**Sources:** [web.dev — CLS](https://web.dev/articles/cls) · [web.dev — Optimize CLS](https://web.dev/articles/optimize-cls) · [web.dev — Vitals](https://web.dev/articles/vitals) · [Next.js fonts](https://nextjs.org/docs/app/getting-started/fonts) · [Next.js Image](https://nextjs.org/docs/app/api-reference/components/image)

</details>

<details>
  <summary>Flash of Unstyled Content (FOUC)</summary>

### Flash of Unstyled Content (FOUC)

**Definition:**

FOUC occurs when HTML renders before CSS is applied. Keep critical CSS in `<head>` — render-blocking CSS is intentional.

| Strategy             | Effect                          |
| -------------------- | ------------------------------- |
| CSS in `<head>`      | Styles apply before first paint |
| Inline critical CSS  | Fast above-fold paint           |
| `font-display: swap` | Control font FOUC (CSS)         |

**How it works:**

- HTML may paint before CSS applied → unstyled flash.
- Keep CSS in `<head>`; critical CSS can be inlined.
- `font-display` in CSS controls font FOUC.

| Strategy            | Effect                    |
| ------------------- | ------------------------- |
| CSS in `<head>`     | Styles before first paint |
| Inline critical CSS | Fast above-fold           |

**When to use:**

Slow CSS CDN · web fonts · CSS loaded via JS (anti-pattern).

#### Common pitfalls

- `@import` chains delaying CSS.
- Hiding body with `opacity:0` until load (bad for LCP).
- Loading styles at end of body.

**Examples:**

```html
<head>
  <link rel="stylesheet" href="/app.css" />
  <link rel="preload" href="/app.css" as="style" />
</head>
```

**Thoughts:** `font-display` and critical CSS → [2_css.md](./2_css.md).

**Sources:** [MDN — FOUC](https://developer.mozilla.org/en-US/docs/Glossary/FOUC)

</details>

<details>
  <summary>Caching (historical note: ApplicationCache)</summary>

### Caching (historical note: ApplicationCache)

**Definition:**

ApplicationCache (appcache) was an HTML5 offline API — deprecated and removed. Use Service Workers and HTTP cache headers instead.

| Approach             | Status                      |
| -------------------- | --------------------------- |
| ApplicationCache     | Deprecated                  |
| Service Workers      | Modern programmatic caching |
| HTTP `Cache-Control` | Standard asset caching      |

**How it works:**

- ApplicationCache (appcache) — removed; do not use.
- Modern: Service Workers + HTTP `Cache-Control`.
- `<link rel="manifest">` for PWA install metadata.

| Approach         | Status     |
| ---------------- | ---------- |
| ApplicationCache | Deprecated |
| Service Workers  | Modern     |
| HTTP cache       | Standard   |

**When to use:**

Static asset caching (HTTP headers) · offline SPA (Service Worker).

#### Common pitfalls

- Copying appcache examples from old tutorials.
- No cache busting on deployed HTML.
- Confusing browser cache with CDN edge cache.

**Examples:**

```html
<!-- Modern: link manifest for PWA, HTTP headers for caching -->
<link rel="manifest" href="/manifest.webmanifest" />
```

**Sources:** [MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching)

</details>

---

<a name="accessibility"></a>
