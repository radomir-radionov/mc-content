# HTML — Rendering

URL to pixels: parse, CSSOM, render tree, layout, paint, composite.

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Browser & rendering](#browser-rendering)

---

## Browser & rendering

<details>
  <summary>From URL to pixels</summary>

### From URL to pixels
**Definition:**

Entering a URL triggers a chain from DNS resolution through HTTP to HTML parsing and the full browser rendering pipeline.

| Step | Action |
|------|--------|
| Parse URL | Scheme, host, path, query, fragment |
| Resolve IP | HTTP cache of HTML, then DNS caches → IP |
| TCP | Three-way handshake (SYN → SYN-ACK → ACK) |
| TLS | Extra handshake + encryption if `https:` |
| HTTP | Request HTML; stream response |
| Parse | HTML → DOM; CSS → CSSOM |
| Render | Render tree → layout → paint → **GPU compositing** |

**How it works:**

1. **Parse the URL** — scheme (`http`/`https`), host, port, path, query, hash. Hash is not sent to the server.
2. **Reuse or fetch** — browser HTTP cache may return a stored HTML document (`Cache-Control` / `ETag`). If not, resolve the host to an IP.
3. **Find the IP** — hosts file → browser DNS cache → OS DNS cache → recursive resolver (often ISP) → root → TLD → authoritative. First hit wins.
4. **TCP handshake** — SYN, SYN-ACK, ACK so both sides agree on a connection.
5. **HTTPS** — TLS handshake (certificate + session keys) then all HTTP bytes are encrypted. HTTP skips this step.
6. **HTTP(S) request** — typically `GET /path` with `Host`, cookies, `Accept`. Server (often Nginx) maps the URI to a file or upstream.
7. **HTML stream** — parser builds the **DOM**; linked CSS builds the **CSSOM**. Parser-blocking JS without `defer`/`async`/`type="module"` pauses HTML parse.
8. **Render tree** — merge DOM + CSSOM; skip `display: none`.
9. **Layout** — geometry (where / how big). **Paint** — pixels (color, text, shadows). **Compositing** — GPU stacks layers into the frame (`transform` / `opacity` stay on the compositor — this is what makes cheap animations).

**When to use:**

Full page load · SPA client navigation (History API skips full reload) · prefetch/prerender hints.

#### Common pitfalls

- Assuming DNS is instant (TTL, cache misses).
- Ignoring TLS handshake cost on first visit.
- Measuring only TTFB, not LCP/CLS.
- Animating `top`/`left`/`width` (layout) instead of `transform`/`opacity` (compositor / GPU).

**Pluses and minuses:**

| + | − |
|------|------|
| Predictable critical path | Many network hops on cold start |
| Cacheable at each layer | Hard to debug without DevTools |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head><title>Products</title></head>
  <body><h1>Catalog</h1></body>
</html>
```

**Thoughts:**

*URL → caches/DNS → TCP → TLS if HTTPS → HTTP HTML → DOM+CSSOM → render tree → layout → paint → GPU composite.*

CSSOM and paint stages → [2_css.md](./2_css.md). DOM APIs after parse → [3_js_browser.md](./3_js_browser.md). HTTP/TLS/cache → [6_web_api.md](./6_web_api.md).

**Sources:** [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)

</details>

<details>
  <summary>DNS</summary>

### DNS
**Definition:**

DNS (Domain Name System) maps human-readable domain names to IP addresses so the browser can open a network connection.

| Lookup order | Cache layer |
|--------------|-------------|
| 0 | `/etc/hosts` (or Windows `hosts`) — local overrides |
| 1 | Browser DNS cache |
| 2 | OS DNS cache |
| 3 | Recursive resolver (often ISP) |
| 4 | Root → TLD → authoritative nameserver |

**How it works:**

- Check **hosts file**, then **browser cache**, then **OS cache**, then a **recursive resolver**. Misses walk root → TLD → authoritative.
- Returns A/AAAA records (IP); CNAME chains add latency.
- `dns-prefetch` / `preconnect` warm DNS and TCP/TLS early.

**When to use:**

First visit · CDN CNAME · split-horizon internal DNS.

#### Common pitfalls

- Hard-coding IPs instead of hostnames.
- Forgetting `preconnect` for third-party origins.
- Long CNAME chains without monitoring.

**Pluses and minuses:**

| + | − |
|------|------|
| Human-readable hostnames | Extra lookup latency |
| Distributed, cacheable | Misconfigured TTL causes stale IPs |

**Examples:**

```html
<!-- Browser resolves example.com → 93.184.216.34 before fetching -->
```

**Sources:** [MDN — DNS](https://developer.mozilla.org/en-US/docs/Glossary/DNS)

</details>

<details>
  <summary>Fetching HTML</summary>

### Fetching HTML
**Definition:**

After DNS resolves, the browser opens a TCP/TLS connection, sends an HTTP request, and receives an HTML document stream to parse.

| Phase | What happens |
|-------|--------------|
| Connect | TCP + TLS handshake (HTTPS) |
| Request | `GET /` with `Host` header |
| Response | `200 OK`, `Content-Type: text/html` |
| Parse | Tokenize HTML stream into DOM |

**How it works:**

- After connect, browser sends `GET` with `Host`, `Accept`, cookies.
- Server responds with status, headers, body stream.
- Parser consumes HTML incrementally (streaming).
- Preload scanner discovers linked resources in parallel.
- Linked assets are extra requests: `<link rel="stylesheet">`, `<img src>`, `<script src>`, fonts, etc. Images are typically non-blocking; classic scripts are parser-blocking.

**When to use:**

Same-origin HTML · CDN edge cache hit · `304 Not Modified` revalidation.

#### Common pitfalls

- Giant inline JSON in HTML bloating TTFB.
- Missing `Content-Type: text/html; charset=utf-8`.
- No compression (gzip/Brotli) on HTML.

**Pluses and minuses:**

| + | − |
|------|------|
| Streaming parse starts early | Large HTML still blocks first paint |
| HTTP/2 multiplexing | Head-of-line blocking on HTTP/1.1 |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <link href="/style.css" rel="stylesheet" />
    <title>Document</title>
  </head>
  <body>
    <img src="/hello.jpg" alt="Hello" width="400" height="300" />
    <script src="/index.js" defer></script>
  </body>
</html>
```

The HTML response also triggers fetches for `style.css`, `hello.jpg`, and `index.js`.

**Sources:** [MDN — HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) · [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/how_browsers_work)

</details>

<details>
  <summary>DOM</summary>

### DOM
**Definition:**

The DOM (Document Object Model) is the browser's live in-memory tree representation of the HTML document. HTML is the source; parsing builds the DOM.

| Concept | Description |
|---------|-------------|
| HTML | Static markup in the response |
| DOM | Runtime object graph |
| Mutation | JS changes DOM → browser re-renders |

**How it works:**

- Parser tokenizes HTML into nodes: `document` → `html` → `head`/`body` → elements/text.
- DOM is **live** — JS mutations trigger style/layout updates.
- HTML source ≠ DOM after JS changes.
- Classic `<script src>` mid-document: parser has only built nodes **above** the tag. `getElementById` for a later `<img>` is `null` unless you use `defer`/`async`/`module` or move the script after the markup.

**When to use:**

Static pages · client-rendered SPAs · `innerHTML` updates.

#### Common pitfalls

- Confusing HTML file with runtime DOM.
- `innerHTML` with untrusted input (XSS).
- Assuming whitespace text nodes don't exist.

**Pluses and minuses:**

| + | − |
|------|------|
| Standard API across browsers | Verbose vs frameworks |
| Live updates | Frequent writes cause layout thrashing |

**Examples:**

```html
<!-- image is null: parser stopped at the script -->
<script src="script.js"></script>
<img id="image" src="/hello.jpg" alt="Hello" />

<!-- image is found -->
<img id="image" src="/hello.jpg" alt="Hello" />
<script src="script.js"></script>

<!-- also found: defer runs after HTML parse -->
<script src="script.js" defer></script>
<img id="image" src="/hello.jpg" alt="Hello" />
```

**Thoughts:** DOM manipulation → [3_js_browser.md](./3_js_browser.md). Events → [3_js_events.md](./3_js_events.md).

**Sources:** [MDN — DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) · [MDN — script](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)

</details>

<details>
  <summary>HTML parsing</summary>

### HTML parsing
**Definition:**

The browser parses HTML top-to-bottom: tokenization converts bytes to tokens, then tree construction creates DOM nodes. Classic scripts without `async`/`defer`/`type="module"` block parsing.

| Step | Action |
|------|--------|
| Tokenization | Bytes → start/end tags, text, comments |
| Tree construction | Tokens → DOM nodes |
| Script handling | Blocking scripts pause parser |
| Preload scanner | Discovers resources early |

**How it works:**

- Tokenizer reads bytes → tags, attributes, text, comments.
- Tree builder inserts nodes; fixes optional tags per spec.
- **Parser-blocking** `<script>` without `async`/`defer`/`module` pauses tree build.
- Preload scanner fetches CSS/images/scripts early.

**When to use:**

Document with deferred scripts · inline script mid-body · malformed HTML (error recovery).

#### Common pitfalls

- `<script>` in `<head>` without `defer` blocks entire parse.
- `document.write` during parse (legacy anti-pattern).
- Relying on implicit optional tags for layout.

**Pluses and minuses:**

| + | − |
|------|------|
| Forgiving error recovery | Invalid HTML may differ across engines |
| Incremental build | Blocking scripts delay DOMContentLoaded |

**Examples:**

```html
<html><body><h1>Hello</h1><p>Text</p></body></html>
```

**Thoughts:** `async`/`defer` script behavior → [3_js_browser.md](./3_js_browser.md).

**Sources:** [WHATWG — Parsing](https://html.spec.whatwg.org/multipage/parsing.html)

</details>

<details>
  <summary>CSSOM</summary>

### CSSOM
**Definition:**

The CSSOM (CSS Object Model) is the browser's object representation of parsed CSS rules. The DOM holds structure; the CSSOM holds computed style information.

| Model | Holds |
|-------|-------|
| DOM | Document structure |
| CSSOM | Style rules per selector |
| Render tree | DOM + CSSOM merged |

**How it works:**

- Each stylesheet parsed into rule objects (selector → declarations).
- Cascade computes winning styles per element.
- CSSOM is separate from DOM until render tree merge.
- Render-blocking: CSS in `<head>` delays first paint (avoids FOUC). Prefer **critical CSS** first (shared + page-specific above-the-fold).
- Waiting for CSS need not stop HTML parse (preload scanner), but **it blocks JS**: scripts often query computed styles / selectors.

**When to use:**

External stylesheet · inline `<style>` · `@import` chains (slower).

#### Common pitfalls

- `@import` inside CSS (serial loading).
- Missing `<link rel="stylesheet">` in `<head>`.
- Assuming DOM order alone determines paint order.

**Pluses and minuses:**

| + | − |
|------|------|
| Separates structure from presentation | `@import` blocks parallel fetch |
| Cacheable CSS files | Large CSS delays first paint |

**Examples:**

```html
<head><link rel="stylesheet" href="/styles.css" /></head>
<body><h1>Hello</h1></body>
```

**Thoughts:** CSS cascade and specificity → [2_css.md](./2_css.md).

**Sources:** [MDN — CSSOM](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model) · [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/how_browsers_work)

</details>

<details>
  <summary>Render tree</summary>

### Render tree
**Definition:**

The render tree combines the DOM and CSSOM into a tree of boxes ready for layout and paint. Elements with `display: none` are excluded. WebKit/Blink call it the render tree; Gecko uses **frame tree**.

| Input | Output |
|-------|--------|
| DOM | Structure nodes |
| CSSOM | Style for each node |
| Render tree | Visible boxes with computed styles |

**How it works:**

- Walk DOM; attach computed styles from CSSOM.
- Exclude `display: none` subtrees entirely.
- `visibility: hidden` stays in tree (invisible, still occupies space).
- Output: boxes ready for layout.

**When to use:**

Hidden modals (`display:none`) · off-screen menus · `content-visibility: auto`.

#### Common pitfalls

- `visibility:hidden` still affects layout/CLS.
- `aria-hidden` does not remove from render tree.
- `display:none` children still in DOM (queryable).

**Pluses and minuses:**

| + | − |
|------|------|
| Only visible work is laid out | Easy to confuse `hidden` vs `display:none` |
| Merges DOM + styles | Pseudo-elements add extra boxes |

**Examples:**

```html
<body>
  <h1>Hello</h1>
  <p hidden>Not in render tree</p>
</body>
```

**Thoughts:** `display: none` vs `visibility: hidden` → [2_css.md](./2_css.md).

**Sources:** [web.dev — Render tree](https://web.dev/articles/critical-rendering-path/constructing-the-object-model)

</details>

<details>
  <summary>Layout (Reflow)</summary>

### Layout (Reflow)

**Definition:**

Layout (reflow) computes the geometry of every visible element — width, height, position, and margins — on the page.

| Property change | Triggers reflow? |
|-----------------|-----------------|
| `width`, `height`, `padding` | Yes |
| DOM insert/remove | Yes |
| `transform`, `opacity` | No (compositor) |

**How it works:**

- Computes x/y/width/height for every render-tree box, recursively from the root (viewport size).
- **Normal flow:** boxes that do not affect siblings can be sized in one pass — stay in flow so the engine does less work.
- **Global layout** — whole tree (window resize). **Incremental layout** — only **dirty** nodes: the mutated element and its descendants (children may depend on the parent).
- Triggers: size changes, font load, DOM insert/remove, reading layout after write.
- Flexbox/Grid/table algorithms run here.

**When to use:**

Initial page layout · responsive resize · dynamic content injection.

#### Common pitfalls

- Animating `width`/`height`/`top`/`left` (triggers layout every frame).
- Reading `offsetHeight` then writing style in a loop.
- Images without dimensions causing reflow on load.

**Pluses and minuses:**

| + | − |
|------|------|
| Accurate geometry | Most expensive rendering stage |
| Required for readable text flow | Layout thrashing if read/write interleaved |

**Examples:**

```html
<div>One</div>
<div>Two</div>
```

**Thoughts:** Box model and layout modes → [2_css.md](./2_css.md).

**Sources:** [MDN — Reflow](https://developer.mozilla.org/en-US/docs/Glossary/Reflow)

</details>

<details>
  <summary>Paint</summary>

### Paint

**Definition:**

Paint rasterizes the visual appearance of each box — text, backgrounds, borders, and shadows — into bitmap layers.

| Stage | Answers |
|-------|---------|
| Layout | Where and how big? |
| Paint | What does it look like? |
| Composite | How are layers stacked? |

**How it works:**

- Rasterizes visual parts: text, backgrounds, borders, shadows.
- Split into paint records per layer; viewport tiled into rectangles — only dirty tiles repaint.
- Paint is typically the most expensive CPU stage among parse → layout → paint.
- Simplified order (back → front, within a stacking context): `background-color` → `background-image` → `border` → descendants → `outline`. Full order → [2_css.md — stacking context](./2_css.md).
- `overflow:hidden` and stacking contexts create clip boundaries.

**When to use:**

Text rendering · box shadows · `border-radius` · large `box-shadow` repaints.

#### Common pitfalls

- Huge `box-shadow` on scrolling containers.
- Frequent `background` changes on large elements.
- Assuming `opacity` always avoids paint (depends on layer).

**Pluses and minuses:**

| + | − |
|------|------|
| Necessary for visual fidelity | Large paint areas are costly |
| Layer splitting helps | `will-change` overuse wastes memory |

**Examples:**

```html
<p class="hero">Styled text</p>
```

**Thoughts:** Paint cost and `will-change` → [2_css.md](./2_css.md).

**Sources:** [MDN — Repaint](https://developer.mozilla.org/en-US/docs/Glossary/Repaint)

</details>

<details>
  <summary>Compositing</summary>

### Compositing

**Definition:**

Compositing merges painted layers into the final frame on the GPU. Elements with `transform`, `opacity`, or `will-change` may be promoted to their own layer.

| Change type | Typical cost |
|-------------|--------------|
| Layout | Highest |
| Paint | Medium |
| Composite only | Lowest |

**How it works:**

- Layout and paint run on the **CPU**; compositor-friendly animations use the **GPU**.
- GPU merges painted layers into final framebuffer. Layers are independent — a change in one need not repaint another.
- Promoted layers: `transform`, `opacity`, `will-change`, `<video>`, `<canvas>`.
- Compositor-only changes skip layout and often skip paint.
- Check which CSS properties trigger layout/paint/composite: [CSS Triggers](https://csstriggers.com/).

**When to use:**

`transform` animations · fixed headers · video overlays.

#### Common pitfalls

- Animating `margin` instead of `transform`.
- `will-change: transform` on everything.
- Ignoring mobile GPU memory limits.

**Pluses and minuses:**

| + | − |
|------|------|
| Smooth 60fps animations | Extra layers use GPU memory |
| Skips layout for transforms | Over-promotion causes layer explosion |

**Examples:**

```html
<div class="animated-card">Compositor-friendly</div>
```

**Thoughts:** GPU-friendly animations → [2_css.md](./2_css.md).

**Sources:** [MDN — Composite](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Composite)

</details>

<details>
  <summary>Layout vs Paint vs Compositing</summary>

### Layout vs Paint vs Compositing

**Definition:**

These three rendering stages answer different questions. Understanding which CSS properties trigger which stage helps optimize animations and avoid jank.

| Stage | Answers | Typical triggers |
|-------|---------|------------------|
| **Layout** | Where? How big? | `width`, `height`, `padding`, DOM insert |
| **Paint** | What does it look like? | `color`, `background`, `box-shadow` |
| **Composite** | How are layers stacked? | `transform`, `opacity` |

**How it works:**

- **Layout** — geometry. **Paint** — pixels. **Composite** — layer stack.
- DevTools Performance panel shows which stage each change triggered.
- Prefer composite-friendly properties for animations.

| Stage | Cost | Typical properties |
|-------|------|--------------------|
| Layout | Highest | `width`, `height`, `padding` |
| Paint | Medium | `color`, `background`, `box-shadow` |
| Composite | Lowest | `transform`, `opacity` |

**When to use:**

Hover color change (paint) · accordion height (layout) · slide-in panel (`transform`, composite).

#### Common pitfalls

- Using `top`/`left` for animations instead of `transform`.
- One property change triggering all three stages.
- Not profiling before micro-optimizing.

**Examples:**

```html
<div id="box">Content</div>
```

**Thoughts:** Full CSS performance guide → [2_css.md](./2_css.md).

**Sources:** [web.dev — Rendering performance](https://web.dev/articles/rendering-performance)

</details>

<details>
  <summary>Global vs incremental layout (dirty nodes)</summary>

### Global vs incremental layout (dirty nodes)

**Definition:** Layout can recalculate the entire render tree (**global**) or only the **dirty** subtree (**incremental**) — the changed element and its descendants.

**How it works:**

- **Global** — e.g. viewport resize: every box must fit the new width. Expensive.
- **Incremental** — a style/DOM change dirties that node; children are dirtied because their size/position may depend on the parent.
- Stay in **normal flow** when possible so siblings do not force extra passes.

**When to use:**

Window resize (global) · toggling a sidebar width (dirty subtree) · injecting a list item.

#### Common pitfalls

Reading `offsetHeight` after a write forces a synchronous (often global-feeling) reflow.

**Pluses and minuses:**

| + | − |
|---|---|
| Incremental avoids full-tree work | Dirty parent still walks all children |
| Flow layout is one-pass for independent boxes | Out-of-flow / percentage sizes can cascade dirtiness |

**Examples:** Changing one card's `padding` relayouts that card and its nested content, not necessarily the whole page.

**Thoughts:**

*Dirty = changed node + descendants; window resize is global.*

CSS property cost table → [2_css.md — Reflow, repaint, composite](./2_css.md).

**Sources:** [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/how_browsers_work) · [Doka — How the browser renders pages](https://doka.guide/js/how-the-browser-renders-pages/)

</details>

<details>
  <summary>Reflow, repaint, and animation frames</summary>

### Reflow, repaint, and animation frames

**Definition:** The render pipeline is cyclic. DOM/style changes rebuild (parts of) the render tree, then **reflow** (layout) and/or **repaint**, once per **animation frame**.

**How it works:**

- New node, text change, or geometry CSS → new/updated render tree → layout → paint.
- Color/`background` often **repaint** only; `width`/`margin`/`top` → **reflow** then paint.
- Target ~60 fps (~16 ms/frame). `requestAnimationFrame(cb)` runs `cb` **before the next paint**, aligned with the display.
- `setInterval(fn, 1000/60)` is not synced to the pipeline: frames can be uneven, and a hidden tab may **catch up** by firing many callbacks at once.

**When to use:**

Canvas/JS animation loops · measuring layout then mutating in rAF · avoiding layout thrashing.

| + rAF | − `setInterval` for visuals |
|--------|-----------------------------|
| Before paint, pauses in background | Unrelated to paint phase |
| Close to refresh rate | Catch-up bursts after inactive tab |

#### Common pitfalls

Animating `left`/`top`/`margin`/`background` every frame (layout/paint). Prefer `transform` and `opacity`.

**Examples:**

```js
function animate(ts) {
  box.style.transform = `translateX(${Math.min(ts / 10, 400)}px)`;
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

**Thoughts:**

*One cycle = one animation frame; tell the browser with `requestAnimationFrame`, not a 16 ms interval.*

API details → [3_js_browser.md — requestAnimationFrame](./3_js_browser_extra.md#request-animation-frame).

**Sources:** [MDN — requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) · [MDN — CSS vs JS animation performance](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/CSS_JavaScript_animation_performance)

</details>

<details>
  <summary>Progressive rendering</summary>

### Progressive rendering

**Definition:** Progressive rendering shows useful pixels early instead of waiting for the full HTML/CSS/JS bundle — paint incrementally as resources arrive.

**How it works:**

- HTML is a **stream**: the parser builds DOM as bytes land; first paint still waits on **render-blocking CSS** for the CSSOM.
- Ship **critical CSS** first; defer the rest. `defer`/`async`/`type="module"` keep scripts off the parser-critical path.
- Below-fold: `loading="lazy"`, `content-visibility: auto`, skeletons while data loads.

**When to use:**

Long articles · marketing LCP hero · SSR HTML that hydrates later.

#### Common pitfalls

One giant CSS file in `<head>`; parser-blocking bundle before content.

**Pluses and minuses:**

| + | − |
|---|---|
| Faster FCP/LCP perception | Extra split CSS/JS complexity |
| Streaming HTML starts parse early | Incomplete CSS still delays first styled paint |

**Examples:** Inline above-the-fold CSS in `<head>`; `<script defer>` for the app; `loading="lazy"` on below-fold images.

**Thoughts:**

*Paint something useful ASAP — critical CSS + non-blocking JS + lazy below-fold.*

CSS techniques → [2_css.md — progressive rendering](./2_css.md). Script loading → [3_js_browser.md](./3_js_browser.md).

**Sources:** [MDN — Critical rendering path](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path) · [web.dev — Critical rendering path](https://web.dev/articles/critical-rendering-path)

</details>

<details>
  <summary>First render — full pipeline</summary>

### First render — full pipeline

**Definition:**

The critical rendering path is the sequence from receiving HTML/CSS/JS to the first painted frame. Parser-blocking resources delay time-to-first-paint.

| # | Step |
|---|------|
| 1 | DNS lookup |
| 2 | TCP/TLS connection |
| 3 | HTTP request/response |
| 4 | HTML → DOM |
| 5 | CSS → CSSOM |
| 6 | Render tree |
| 7 | Layout → Paint → Composite |

**How it works:**

- Critical rendering path: HTML → DOM + CSS → CSSOM → render tree → layout → paint → composite.
- First Contentful Paint (FCP) / LCP mark user-visible milestones. Full FCP: [6_web_api.md](./6_web_api_security.md#first-contentful-paint-fcp).
- Parser-blocking CSS in `<head>` delays first paint (browser waits for CSSOM). Parser-blocking JS without `defer` pauses HTML parse.

**When to use:**

Marketing landing (optimize LCP hero) · dashboard (defer non-critical JS) · SSR + hydration.

#### Common pitfalls

- Render-blocking scripts in `<head>` without `defer`.
- Hero image without `width`/`height` or `fetchpriority`.
- CSS loaded at bottom of `<body>` causing FOUC.

**Pluses and minuses:**

| + | − |
|------|------|
| Clear optimization targets (CRP) | Many moving parts across HTML/CSS/JS |
| Measurable via Web Vitals | Over-inlining bloats HTML |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="/critical.css" />
    <script src="/app.js" defer></script>
  </head>
  <body><main><h1>First paint</h1></main></body>
</html>
```

**Thoughts:** Script loading → [3_js_browser.md](./3_js_browser.md). CSS blocking → [2_css.md](./2_css.md).

**Sources:** [web.dev — Critical rendering path](https://web.dev/articles/critical-rendering-path)

</details>

---

<a name="document-structure--metadata"></a>
