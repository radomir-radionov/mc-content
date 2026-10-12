# HTML — Advanced

Web components, shadow DOM, and newer HTML APIs.

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Advanced topics](#advanced-topics)

---

## Advanced topics

<details>
  <summary>Web Components (all three specs)</summary>

### Web Components (all three specs)

**Definition:**

Web Components combine Custom Elements, Shadow DOM, and HTML Templates for reusable, encapsulated UI.

| Technology | Purpose |
|------------|---------|
| Custom Elements | New HTML tags with lifecycle |
| Shadow DOM | Encapsulated DOM + styles |
| `<template>` + `<slot>` | Inert markup cloned into DOM |

**How it works:**

- Custom Elements — new tag names + lifecycle.
- Shadow DOM — encapsulated subtree + styles.
- `<template>` + `<slot>` — declarative composition.

| Spec | Purpose |
|------|--------|
| Custom Elements | New tags |
| Shadow DOM | Encapsulation |

**When to use:**

Design system primitives · embeddable widgets · framework-agnostic components.

#### Common pitfalls

- Shadow DOM breaking global form styles unexpectedly.
- Not projecting content with slots.
- Closed shadow roots blocking legitimate testing.

**Examples:**

```html
<template id="card-tpl">
  <div class="card"><slot name="title"></slot><slot></slot></div>
</template>
<user-card>
  <template shadowrootmode="open">
    <style>:host { display: block; }</style>
    <slot></slot>
  </template>
  <span>Projected content</span>
</user-card>
```

**Thoughts:** Custom Elements JS API → [3_js_browser_extra.md](./3_js_browser_extra.md).

**Sources:** [MDN — Web Components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components)

</details>

<details>
  <summary>Popover API</summary>

### Popover API

**Definition:**

The Popover API provides native top-layer overlays with light-dismiss and focus management via the `popover` attribute.

| Value | Behavior |
|-------|----------|
| `popover="auto"` | Closes on outside click |
| `popover="manual"` | Requires JS to close |

**How it works:**

- `popover` attribute puts element in top layer.
- `popovertarget` on button toggles target by id.
- `popover="auto"` light-dismiss; `manual` needs JS close.

| Value | Behavior |
|-------|----------|
| `auto` | Outside click closes |
| `manual` | JS-controlled |

**When to use:**

Menus · tooltips · teaching UI without z-index wars.

#### Common pitfalls

- Nesting popovers without focus plan.
- Replacing accessible menu patterns incorrectly.
- Browser support check before production.

**Examples:**

```html
<button popovertarget="menu" popovertargetaction="toggle">Open</button>
<div id="menu" popover>
  <p>Popover content</p>
</div>
```

**Sources:** [MDN — Popover API](https://developer.mozilla.org/en-US/docs/Web/API/Popover_API)

</details>

<details>
  <summary>inert, hidden, contenteditable</summary>

### inert, hidden, contenteditable

**Definition:**

These global attributes control visibility, interactivity, and inline editing behavior.

| Attribute | Behavior |
|-----------|----------|
| `hidden` | Not rendered (`display: none`) |
| `inert` | Excluded from interaction and focus |
| `contenteditable="true"` | User can edit content inline |

**How it works:**

- `hidden` — `display:none`, not rendered, not focusable.
- `inert` — visible but removed from interaction/focus (modal backgrounds).
- `contenteditable` — user can edit inner HTML inline.

| Attribute | Behavior |
|-----------|----------|
| `hidden` | Not displayed |
| `inert` | No interaction |

**When to use:**

Modal backdrop (`inert` on main) · toggle sections (`hidden`) · rich text editor.

#### Common pitfalls

- `aria-hidden` instead of `inert` for modal siblings (focus leak).
- `contenteditable` without sanitization on paste.
- `hidden` elements still in tab order if focusable children exist.

**Examples:**

```html
<div hidden>Not visible</div>
<main inert>Background while modal open</main>
<p contenteditable="true">Editable paragraph</p>
```

**Sources:** [MDN — inert](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inert)

</details>

<details>
  <summary>Progressive enhancement vs graceful degradation</summary>

### Progressive enhancement vs graceful degradation

**Definition:**

Progressive enhancement starts with working baseline HTML and layers CSS/JS. Graceful degradation builds full experience with fallbacks.

| Approach | Strategy |
|----------|----------|
| Progressive enhancement | HTML works first; JS enhances |
| Graceful degradation | Full experience with fallbacks |

**How it works:**

- **PE:** baseline HTML works; JS/CSS enhance.
- **GD:** build full experience; provide fallbacks.
- PE is the modern default for resilient web apps.

| Approach | Strategy |
|----------|----------|
| PE | HTML first |
| GD | Full + fallback |

**When to use:**

Form works with POST · search works without JS autocomplete · `<noscript>` notice.

#### Common pitfalls

- Empty `<body>` until JS loads (no PE).
- Essential actions only via JS click handlers.
- Assuming fast JS on all devices.

**Examples:**

```html
<form action="/search" method="get">
  <label for="q">Search</label>
  <input id="q" name="q" type="search" />
  <button type="submit">Search</button>
</form>
<!-- JS can enhance with autocomplete, no-JS still works -->
```

**Sources:** [MDN — Progressive enhancement](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)

</details>

<details>
  <summary>Feature detection vs user-agent sniffing</summary>

### Feature detection vs user-agent sniffing

**Definition:**

Feature detection checks whether a capability exists. User-agent sniffing parses browser strings — brittle and discouraged.

| Technique | Reliability |
|-----------|-------------|
| Feature detection | Preferred |
| User-agent sniffing | Brittle — avoid |

**How it works:**

- Feature detection: `if ('showModal' in HTMLDialogElement.prototype)`.
- UA sniffing: parsing `navigator.userAgent` — brittle.
- `@supports` in CSS for style features.

| Technique | Reliability |
|-----------|-------------|
| Feature detection | Good |
| UA sniffing | Poor |

**When to use:**

Polyfill dialog · progressive `popover` · CSS grid fallback.

#### Common pitfalls

- Blocking browsers by version string.
- Sniffing mobile UA for responsive layout (use CSS).
- Detecting Safari by UA for polyfills (false positives).

**Examples:**

```html
<!-- Prefer @supports in CSS and feature checks in JS -->
<!-- Never gate on browser name/version strings -->
```

**Sources:** [MDN — Feature detection](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Feature_detection)

</details>

<details>
  <summary>HTML validation</summary>

### HTML validation

**Definition:**

Validation checks HTML against the spec. Invalid HTML may render but causes accessibility, SEO, and maintenance issues.

| Type | Checks |
|------|--------|
| Syntax | Well-formed markup |
| Logical | Heading order, unique IDs, alt text |

**How it works:**

- W3C validator checks against HTML spec.
- Invalid markup may still render (error recovery) but hurts a11y/SEO.
- Logical validation: one `h1`, unique ids, alt on images.

| Type | Checks |
|------|--------|
| Syntax | Well-formed tags |
| Logic | Heading order, unique IDs |

**When to use:**

CI HTML lint · pre-release audit · email HTML (stricter clients).

#### Common pitfalls

- Duplicate `id` values (invalid, breaks labels).
- Unclosed tags relying on browser fixes.
- Custom elements without hyphen (invalid CE name).

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head><title>Valid page</title></head>
  <body><main><h1>Title</h1></main></body>
</html>
```

**Sources:** [W3C Validator](https://validator.w3.org/)

</details>

<details>
  <summary>Cross-browser compatibility</summary>

### Cross-browser compatibility

**Definition:**

HTML features vary by browser support. Check caniuse.com before using new APIs; use polyfills or progressive enhancement.

| Tool | Purpose |
|------|---------|
| caniuse.com | Feature support matrix |
| Progressive enhancement | Baseline HTML always works |

**How it works:**

- Check caniuse.com for new HTML APIs.
- Progressive enhancement: baseline HTML always works.
- Polyfills for `dialog`, `popover` where needed.

| Tool | Purpose |
|------|--------|
| caniuse.com | Support matrix |
| PE | Baseline without new APIs |

**When to use:**

Native `<dialog>` with fallback · `input type="date"` varying UI.

#### Common pitfalls

- Shipping features without support matrix.
- Polyfill without feature detect gate.
- Testing only Chrome.

**Examples:**

```html
<dialog id="modal">
  <p>Native modal — check support, provide fallback.</p>
</dialog>
```

**Sources:** [caniuse.com](https://caniuse.com/)

</details>

<details>
  <summary>HTML5 as an open web platform</summary>

### HTML5 as an open web platform

**Definition:**

HTML5 is part of a broader platform: HTML + CSS + JavaScript + DOM APIs. Markup provides structure; behavior and presentation come from JS and CSS.

| Layer | Role |
|-------|------|
| HTML | Structure and semantics |
| CSS | Presentation → [2_css.md](./2_css.md) |
| JS | Behavior → [3_js_browser.md](./3_js_browser.md) |

**How it works:**

- HTML5 = HTML + related APIs (CSS, JS, DOM).
- Markup provides structure; behavior from JS; presentation from CSS.
- Living Standard at WHATWG — continuous evolution.

| Layer | Role |
|-------|------|
| HTML | Structure |
| CSS | Presentation |
| JS | Behavior |

**When to use:**

PWA (HTML+manifest+SW) · media pages · web components.

#### Common pitfalls

- Treating HTML as obsolete in SPAs.
- Inline styles/scripts instead of separation.
- Ignoring semantic HTML in component frameworks.

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head><link rel="stylesheet" href="/app.css" /></head>
  <body><main id="app"></main><script src="/app.js" defer></script></body>
</html>
```

**Sources:** [WHATWG HTML Living Standard](https://html.spec.whatwg.org/)

</details>

<details>
  <summary>ES module import maps</summary>

### ES module import maps

**Definition:**

Import maps let browsers resolve bare module specifiers to URLs without a bundler.

| Key | Maps to |
|-----|---------|
| `"lodash"` | `/vendor/lodash.esm.js` |
| `"components/"` | `/js/components/` |

**How it works:**

- `<script type="importmap">` maps bare specifiers to URLs.
- Enables `import 'lodash'` without bundler in modern browsers.
- JSON object: `imports` and optional `scopes`.

| Key | Maps |
|-----|------|
| `imports` | Bare specifier → URL |
| `scopes` | Path-specific overrides |

**When to use:**

No-bundler prototypes · CDN bare imports · scoped mappings.

#### Common pitfalls

- Import map after module scripts (must be first).
- Missing CORS on mapped CDN URLs.
- Production without fallback for unsupported browsers.

**Examples:**

```html
<script type="importmap">
{
  "imports": {
    "lodash": "/vendor/lodash.esm.js",
    "components/": "/js/components/"
  }
}
</script>
<script type="module" src="/app.mjs"></script>
```

**Sources:** [MDN — import maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap)

</details>

<details>
  <summary>Web App Manifest (PWA)</summary>

### Web App Manifest (PWA)

**Definition:**

The Web App Manifest defines installable PWA metadata — name, icons, `start_url`, `display`, and theme colors.

| Field | Purpose |
|-------|---------|
| `name` | App display name |
| `icons` | Home screen icons |
| `display: standalone` | App-like chrome |

**How it works:**

- `<link rel="manifest" href="/manifest.webmanifest">` — JSON for install metadata.
- Fields: `name`, `icons`, `start_url`, `display`, `theme_color`.
- Works with Service Worker for offline; manifest alone = installable metadata only.

| Field | Purpose |
|-------|--------|
| `icons` | Install icons |
| `display` | `standalone` vs `browser` |

**When to use:**

Add to home screen · standalone display · themed splash.

#### Common pitfalls

- Manifest not served with correct MIME type.
- Missing 192/512 icons.
- `start_url` not cacheable.

**Examples:**

```html
<link rel="manifest" href="/manifest.webmanifest" />
<meta name="theme-color" content="#1a1a2e" />
```

**Sources:** [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)

</details>

<details>
  <summary>Obsolete elements — do not use</summary>

### Obsolete elements — do not use

**Definition:**

Obsolete elements still parse in browsers but should not be used — replace with CSS or modern alternatives.

| Obsolete | Replacement |
|----------|-------------|
| `<font>`, `<center>` | CSS typography/alignment |
| `<marquee>` | CSS animations |
| `<frame>`, `<frameset>` | Never use |
| `<acronym>` | `<abbr>` |

**How it works:**

- Legacy presentational tags removed or deprecated: `font`, `center`, `marquee`, `frame`/`frameset`.
- Use CSS for presentation; semantic elements for structure.
- Browsers may still render but invalid and bad for a11y.

| Obsolete | Replacement |
|----------|-------------|
| `<font>` | CSS `color`/`font-family` |
| `<center>` | CSS `text-align` |

**When to use:**

Migrating legacy sites · sanitizing old CMS output.

#### Common pitfalls

- Copying W3Schools-era examples.
- `u` for non-proper names (prefer CSS).
- Table layout tags (`width` on table).

**Examples:**

```html
<p class="centered">Use CSS instead of obsolete tags.</p>
```

**Thoughts:** CSS replacements → [2_css.md](./2_css.md).

**Sources:** [MDN — Obsolete features](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#obsolete_and_deprecated_elements)

</details>

<details>
  <summary>HTML security basics (XSS)</summary>

### HTML security basics (XSS)

**Definition:**

HTML is the first line of defense against XSS. Escape user input, avoid inline handlers, and use CSP and SRI.

| Risk | Safe practice |
|------|---------------|
| User input in HTML | Escape; use `textContent` not `innerHTML` |
| Inline handlers | Avoid `onclick`; use `addEventListener` |
| Third-party scripts | SRI `integrity` attribute |
| `target="_blank"` | Add `rel="noopener noreferrer"` |

**How it works:**

- Never insert untrusted data into HTML without escaping.
- Avoid inline event handlers (`onclick`); use CSP.
- SRI `integrity` on third-party scripts; `rel="noopener"` on `_blank`.

| Risk | Mitigation |
|------|------------|
| `innerHTML` injection | `textContent` / sanitize |
| Inline handlers | CSP + addEventListener |

**When to use:**

User-generated comments · CMS content · third-party widgets.

#### Common pitfalls

- `innerHTML` with user input.
- `target="_blank"` without `noopener`.
- CDN scripts without SRI.

**Examples:**

```html
<a href="https://external.com" target="_blank" rel="noopener noreferrer">External</a>
<script src="https://cdn.example.com/lib.js" integrity="sha384-..." crossorigin="anonymous"></script>
```

**Thoughts:** DOM XSS patterns → [3_js_browser.md](./3_js_browser.md). Stored vs reflected, CSP, clickjacking → [7_special.md](./7_special_security.md#14-common-web-attacks) · [6_web_api.md](./6_web_api_security.md#security).

**Sources:** [MDN — XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)

</details>

---

<a name="attributes-reference"></a>
