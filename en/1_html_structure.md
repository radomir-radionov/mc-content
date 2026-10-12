# HTML — Structure

Document skeleton, metadata, elements, and attributes.

[← Back to hub](./1_html.md)

### Related docs

- [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Document structure & metadata](#document-structure-metadata)
- [Elements reference](#elements-reference)
- [Attributes reference](#attributes-reference)

---

## Document structure & metadata

<details>
  <summary>Basic HTML document structure</summary>

### Basic HTML document structure

**Definition:**

A valid HTML5 document has a DOCTYPE, root `<html>` with `lang`, a `<head>` for metadata, and a `<body>` for visible content.

| Part              | Role                                |
| ----------------- | ----------------------------------- |
| `<!DOCTYPE html>` | Triggers standards mode             |
| `<html lang>`     | Root element; language for a11y/SEO |
| `<head>`          | Metadata, links — not page content  |
| `<body>`          | Visible content                     |

**How it works:**

- `<!DOCTYPE html>` → standards mode.
- `<html lang>` sets document language.
- `<head>`: charset, viewport, title, linked resources.
- `<body>`: visible flow content only.

**When to use:**

Static site · SPA shell (`<div id="app">`) · AMP-like minimal docs.

#### Common pitfalls

- Missing `<meta charset>` (encoding bugs).
- Multiple `<main>` or none.
- Placing visible content in `<head>`.

**Pluses and minuses:**

| +                            | −                                    |
| ---------------------------- | ------------------------------------ |
| Valid skeleton aids a11y/SEO | Boilerplate easy to omit in snippets |
| `lang` helps screen readers  | Wrong `lang` hurts pronunciation     |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Page title</title>
    <link rel="stylesheet" href="/styles.css" />
  </head>
  <body></body>
</html>
```

**Sources:** [MDN — Getting started](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started)

</details>

<details>
  <summary>DOCTYPE</summary>

### DOCTYPE

**Definition:**

The `<!DOCTYPE html>` declaration tells the browser to use HTML5 standards mode. Without it, browsers may enter quirks mode with inconsistent layout behavior.

| Mode      | Trigger                 | Effect                |
| --------- | ----------------------- | --------------------- |
| Standards | `<!DOCTYPE html>`       | Spec-compliant layout |
| Quirks    | Missing/invalid DOCTYPE | Legacy IE emulation   |

**How it works:**

- `<!DOCTYPE html>` is the HTML5 doctype — not an element.
- Triggers **standards mode** (no quirks).
- Must be first line; no attributes.

| Mode      | Trigger                 | Effect                |
| --------- | ----------------------- | --------------------- |
| Standards | `<!DOCTYPE html>`       | Spec-compliant layout |
| Quirks    | Missing/invalid DOCTYPE | Legacy box model bugs |

**When to use:**

New HTML5 pages · legacy pages missing DOCTYPE (quirks).

#### Common pitfalls

- Uppercase `<!DOCTYPE HTML>` still works but inconsistent style.
- XML-style `<!DOCTYPE html />` is wrong.
- XHTML doctypes on HTML5 pages.

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <body></body>
</html>
```

**Sources:** [MDN — DOCTYPE](https://developer.mozilla.org/en-US/docs/Glossary/Doctype)

</details>

<details>
  <summary>HTML vs XML</summary>

### HTML vs XML

**Definition:** HTML is a fixed web vocabulary; XML is a custom-tag syntax for data and documents.

**Key idea:** Full comparison and interview framing live in [HTML — Core](./1_html_core.md#html-vs-xml). XML syntax and namespaces: [Web API — XML](./6_web_api_rest.md#xml).

**Sources:** [HTML — Core — HTML vs XML](./1_html_core.md#html-vs-xml)

</details>

<details>
  <summary>Meta tags</summary>

### Meta tags

**Definition:**

Meta elements live in `<head>` and provide document metadata — charset, viewport, description, and robots directives.

| Meta          | Purpose                   |
| ------------- | ------------------------- |
| `charset`     | Character encoding        |
| `viewport`    | Mobile responsive scaling |
| `description` | Search snippet text       |
| `robots`      | Crawl/index directives    |

**How it works:**

- `<meta>` void elements in `<head>` for document metadata.
- `charset` must be early (first 1024 bytes ideally).
- `viewport` required for mobile responsive layout.
- `description`/`robots` affect search snippets and crawling.

| Meta          | Purpose            |
| ------------- | ------------------ |
| `charset`     | Character encoding |
| `viewport`    | Mobile scaling     |
| `description` | Search snippet     |
| `robots`      | Crawl directives   |

**When to use:**

Public pages (SEO meta) · `noindex` staging · `theme-color` PWA.

#### Common pitfalls

- Duplicate `viewport` tags.
- `keywords` meta (ignored by major engines).
- `refresh` redirect (bad for a11y; use HTTP redirect).

**Examples:**

```html
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="description" content="Short page summary for search results" />
<meta name="robots" content="index, follow" />
```

**Sources:** [Google — Special tags](https://developers.google.com/search/docs/crawling-indexing/special-tags)

</details>

<details>
  <summary>link rel types & resource hints</summary>

### link rel types & resource hints

**Definition:** The `<link>` element connects the document to external resources — stylesheets, icons, canonical URLs, and performance hints.

**How it works:**

- `<link rel="stylesheet">` — render-blocking CSS in critical path.
- `canonical` — preferred URL for duplicate content (SEO).
- `preconnect` — early DNS+TCP+TLS; `preload` — high-priority current-page fetch; `prefetch` — low-priority next navigation.

| `rel` value  | Purpose                                |
| ------------ | -------------------------------------- |
| `stylesheet` | CSS                                    |
| `canonical`  | Preferred URL for SEO                  |
| `preconnect` | Early connection (DNS+TCP+TLS)         |
| `preload`    | High-priority fetch for current page   |
| `prefetch`   | Low-priority fetch for next navigation |

**When to use:**

Critical font preload · CDN preconnect · next-page prefetch.

| `rel`        | When               |
| ------------ | ------------------ |
| `stylesheet` | App CSS            |
| `preconnect` | Third-party origin |
| `preload`    | LCP image/font     |

#### Common pitfalls

- Preloading everything (bandwidth waste).
- `preload` without matching `as` type.
- `preconnect` to origins never used.

**Examples:**

```html
<link rel="stylesheet" href="/app.css" />
<link rel="canonical" href="https://example.com/page" />
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link
  rel="preload"
  href="/hero.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>
<script
  src="https://cdn.example.com/lib.js"
  integrity="sha384-..."
  crossorigin="anonymous"
></script>
```

**Thoughts:** Why `preconnect` exists (TCP+TLS RTT) → [TCP vs UDP](./6_web_api_interview.md#tcp-vs-udp).

**Sources:** [MDN — link types](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel)

</details>

<details>
  <summary>Favicon & app icons (link rel icon)</summary>

### Favicon & app icons (link rel icon)

**Definition:** `<link rel="icon">` and related `rel` values declare favicons and home-screen icons browsers and OSes use for tabs, bookmarks, and PWA install UI.

**How it works:**

- Browsers fetch `rel="icon"` from `<head>` (fallback: `/favicon.ico` at site root).
- Provide PNG/SVG sizes; `apple-touch-icon` for iOS; manifest `icons` for PWA install prompts.
- SVG favicons scale; PNG needs multiple sizes for retina.

| `rel`              | Purpose           |
| ------------------ | ----------------- |
| `icon`             | Standard favicon  |
| `apple-touch-icon` | iOS home screen   |
| `mask-icon`        | Safari pinned tab |

**When to use:**

Browser tab branding · PWA install · dark/light favicon via `media` attribute on `<link>`.

#### Common pitfalls

- 404 on `/favicon.ico` when only PNG linked.
- Single 16×16 icon blurry on retina.
- Manifest `icons` missing 192×512 for Android install.

**Pluses and minuses:**

| +                                     | −                             |
| ------------------------------------- | ----------------------------- |
| Professional polish in tabs/bookmarks | Many sizes to maintain        |
| SVG one file scales                   | Older IE ignored SVG favicons |

**Examples:**

```html
<link rel="icon" href="/favicon.svg" type="image/svg+xml" />
<link rel="icon" href="/favicon-32.png" sizes="32x32" type="image/png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" sizes="180x180" />
```

**Sources:** [MDN — link rel icon](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel#icon)

</details>

<details>
  <summary>modulepreload & dns-prefetch resource hints</summary>

### modulepreload & dns-prefetch resource hints

**Definition:** Resource hints tell the browser to resolve DNS or fetch modules early, reducing latency on critical cross-origin or ESM resources.

**How it works:**

- `dns-prefetch` — DNS lookup only (cheap hint for third-party domains).
- `preconnect` — DNS + TCP + TLS (use for fonts/CDN you will definitely use).
- `modulepreload` — fetch + parse ES module early; use for entry `.mjs` files.

| Hint            | Cost            | Use when                 |
| --------------- | --------------- | ------------------------ |
| `dns-prefetch`  | DNS only        | Many third-party origins |
| `preconnect`    | Full connection | Critical CDN/fonts       |
| `modulepreload` | Module fetch    | ESM entry point          |

**When to use:**

Analytics domain dns-prefetch · Google Fonts preconnect · `modulepreload` for `app.mjs`.

#### Common pitfalls

- `preconnect` to origins never used on page.
- `modulepreload` without matching `crossorigin` on CORS modules.
- Replacing all `preconnect` with `dns-prefetch` when full connection warmup needed.

**Pluses and minuses:**

| +                       | −                                           |
| ----------------------- | ------------------------------------------- |
| Cuts connection latency | Over-hinting wastes bandwidth               |
| Faster ESM startup      | `modulepreload` needs `crossorigin` if CORS |

**Examples:**

```html
<link rel="dns-prefetch" href="https://analytics.example.com" />
<link rel="preconnect" href="https://fonts.googleapis.com" crossorigin />
<link rel="modulepreload" href="/app.mjs" crossorigin />
```

**Sources:** [MDN — dns-prefetch](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/dns-prefetch) · [modulepreload](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/modulepreload)

</details>

<details>
  <summary>base element</summary>

### base element

**Definition:**

The `<base>` element sets the default URL and target for all relative links on the page. Use sparingly — one per document, must be in `<head>`.

| Attribute | Effect                      |
| --------- | --------------------------- |
| `href`    | Base URL for relative links |
| `target`  | Default `target` for links  |

**How it works:**

- Single `<base href>` in `<head>` rewrites relative URLs for `a`, `link`, `img`, `form`, etc.
- Optional `target` sets default link target.
- Affects entire document — use sparingly.

**When to use:**

Email HTML templates · embedded docs in iframe · mirror sites.

#### Common pitfalls

- Multiple `<base>` elements (invalid).
- `<base>` after relative URLs already parsed.
- `target="_blank"` on base affecting all links.

**Pluses and minuses:**

| +                            | −                                     |
| ---------------------------- | ------------------------------------- |
| Portable relative paths      | Breaks fragment-only links if mis-set |
| One place to change base URL | Hard to debug unexpected resolves     |

**Examples:**

```html
<base href="https://example.com/assets/" target="_blank" />
```

**Sources:** [MDN — base](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)

</details>

<details>
  <summary>Absolute vs relative paths</summary>

### Absolute vs relative paths

**Definition:**

URLs in HTML can be relative to the current document or absolute. Relative paths are portable; absolute URLs are required for external resources.

| Type            | Example                          | Use case            |
| --------------- | -------------------------------- | ------------------- |
| Relative        | `/images/logo.png`               | Same site           |
| Relative parent | `../css/app.css`                 | Sibling directories |
| Absolute URL    | `https://cdn.example.com/app.js` | External CDN        |

**How it works:**

- **Relative** — resolved against current URL or `<base>`.
- **Root-relative** (`/assets/x`) — from site root.
- **Absolute** (`https://...`) — full URL, required for cross-origin.

| Type          | Example               | Use          |
| ------------- | --------------------- | ------------ |
| Root-relative | `/img/logo.png`       | Same site    |
| Relative      | `../css/app.css`      | Nested paths |
| Absolute      | `https://cdn.../x.js` | External CDN |

**When to use:**

Same-repo assets (`/css/app.css`) · sibling dir (`../img/x`) · CDN (`https://cdn...`).

#### Common pitfalls

- Broken paths when moving files without updating `../`.
- Protocol-relative `//cdn...` (deprecated pattern).
- Relative paths in emails (unreliable).

**Examples:**

```html
<img src="/images/logo.png" alt="Logo" />
<link rel="stylesheet" href="../css/app.css" />
<script src="https://cdn.example.com/lib.js"></script>
```

**Sources:** [MDN — Links](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks)

</details>

<details>
  <summary>Comments & HTML entities</summary>

### Comments & HTML entities

**Definition:**

HTML comments are not rendered. Entities represent special characters that would otherwise break markup or are not on the keyboard.

| Entity          | Character          |
| --------------- | ------------------ |
| `&amp;`         | `&`                |
| `&lt;` / `&gt;` | `<` / `>`          |
| `&copy;`        | ©                  |
| `&nbsp;`        | non-breaking space |

**How it works:**

- `<!-- comment -->` not rendered; cannot nest.
- Entities encode reserved/special chars: `&lt;`, `&amp;`, `&copy;`.
- Numeric entities `&#169;` / `&#xA9;` for any Unicode code point.

| Entity          | Character          |
| --------------- | ------------------ |
| `&amp;`         | `&`                |
| `&lt;` / `&gt;` | `<` / `>`          |
| `&nbsp;`        | non-breaking space |

**When to use:**

Template notes in source · displaying `<` in code samples · copyright symbols.

#### Common pitfalls

- Unescaped `&` in URLs/attributes.
- `&nbsp;` for layout spacing (use CSS).
- Comments with `--` inside (invalid).

**Examples:**

```html
<!-- This is a comment; not rendered -->
<p>&copy; 2025 &mdash; Price: &euro;10 &lt; &euro;20</p>
```

**Sources:** [MDN — Entities](https://developer.mozilla.org/en-US/docs/Glossary/Entity)

</details>

---

<a name="elements-reference"></a>

### HTML structure

**Definition:** HTML (Hypertext Markup Language) is the standard markup language for web pages. An HTML document is a text file the browser parses into a tree of elements that communicate structure, meaning, and behavior.

**How it works:**

- Source file contains tags, attributes, and text nodes.
- Browser parser builds a live DOM tree from the markup.
- CSS and JS attach to that tree for presentation and behavior.

| Term          | Role                                                              |
| ------------- | ----------------------------------------------------------------- |
| **Tag**       | Markup syntax (`<p>`, `</p>`) in source code                      |
| **Element**   | Live DOM node — opening tag + content + closing tag (or void tag) |
| **Attribute** | Key/value metadata on an element (`href`, `class`, `alt`)         |

**When to use:**

Every web page · email HTML · SSR/SSG output · component templates.

Every web page starts with HTML — it is the structural foundation for CSS and JavaScript.

#### Common pitfalls

- Confusing tags (syntax) with elements (DOM nodes).
- Omitting `lang` on `<html>`.
- Void elements written with closing tags (invalid for some).

**Pluses and minuses:**

| +                              | −                                       |
| ------------------------------ | --------------------------------------- |
| Universal web foundation       | Not Turing-complete alone               |
| Semantic elements aid a11y/SEO | Invalid markup relies on error recovery |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My page</title>
  </head>
  <body>
    <p class="intro">Hello, world.</p>
  </body>
</html>
```

**Thoughts:** Void elements (`<img>`, `<br>`, `<input>`, `<meta>`, `<link>`) have no closing tag. Validate with [validator.w3.org](https://validator.w3.org/).

**Sources:** [MDN — HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) · [WHATWG HTML Living Standard](https://html.spec.whatwg.org/)

<a name="browser-rendering-pipeline"></a>

Browser rendering (parse → layout → paint → composite) lives in [HTML — Rendering](./1_html_rendering.md).

## Elements reference

<details>
  <summary>Block vs inline elements</summary>

### Block vs inline elements

**Definition:**

Block-level elements stack vertically and accept full box model sizing. Inline elements flow with text and typically contain only phrasing content.

| Type   | Behavior           | Examples                       |
| ------ | ------------------ | ------------------------------ |
| Block  | Full width, stacks | `div`, `p`, `section`, `table` |
| Inline | Flows with text    | `span`, `a`, `strong`, `em`    |

**How it works:**

- **Block** — starts new line, accepts width/height/margin block axis.
- **Inline** — flows in text line; width/height ignored (mostly).
- HTML5 content categories refine nesting rules.

| Type   | Behavior          | Examples              |
| ------ | ----------------- | --------------------- |
| Block  | Stacks vertically | `div`, `p`, `section` |
| Inline | Flows with text   | `span`, `a`, `em`     |

**When to use:**

Paragraphs (`p`) · inline emphasis (`span`, `a`) · `display` CSS can change presentation.

#### Common pitfalls

- Block elements inside `p` (auto-closes `p`).
- Interactive block-in-inline invalid nesting.
- Assuming `display` changes HTML category.

**Examples:**

```html
<div><p>Block paragraph</p></div>
<p>Inline <span>wrapper</span> inside text.</p>
```

**Sources:** [MDN — Block-level elements](https://developer.mozilla.org/en-US/docs/Glossary/Block-level_content)

</details>

<details>
  <summary>Links (anchor element)</summary>

### Links (anchor element)

**Definition:**

The `<a>` element creates hyperlinks for navigation. Use `<button>` for in-page actions, not links.

| Attribute  | Purpose                             |
| ---------- | ----------------------------------- |
| `href`     | URL or fragment                     |
| `target`   | `_self`, `_blank`, etc.             |
| `rel`      | `noopener noreferrer` with `_blank` |
| `download` | Suggest file download               |

**How it works:**

- `<a href>` for navigation to URLs or fragments.
- Absolute `https://…` vs relative: no host (browser uses current origin / `<base>`); `/path` from root; `../` up one segment; `mailto:` for email. Full anatomy: [URI, URL & URN](./6_web_api_core.md#uri-url--urn).
- `target="_blank"` needs `rel="noopener noreferrer"`.
- `download` suggests file download for same-origin URLs.

| Attribute  | Purpose                 |
| ---------- | ----------------------- |
| `href`     | Destination             |
| `rel`      | `noopener` for `_blank` |
| `download` | Save as file            |

**When to use:**

Internal routes · external docs · skip links (`href="#main"`).

#### Common pitfalls

- `<a>` without `href` (not keyboard focusable).
- `javascript:void(0)` hrefs.
- Using `<a>` for buttons (use `<button>`).

**Examples:**

```html
<a href="/about" target="_blank" rel="noopener noreferrer">About</a>
<a href="mailto:hi@example.com">Email</a>
```

**Sources:** [MDN — a](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) · [URI, URL & URN](./6_web_api_core.md#uri-url--urn)

</details>

<details>
  <summary>Tables</summary>

### Tables

**Definition:**

Tables present tabular data with header and body row groups. Use `scope` on `<th>` for accessibility.

| Element               | Role        |
| --------------------- | ----------- |
| `<caption>`           | Table title |
| `<thead>` / `<tbody>` | Row groups  |
| `<th scope>`          | Header cell |
| `<td>`                | Data cell   |

**How it works:**

- Tables for **tabular data** only — not layout.
- `<caption>`, `<thead>`, `<tbody>`, `<th scope>` aid structure.
- `scope="col"` / `scope="row"` for screen readers.

| Element      | Role        |
| ------------ | ----------- |
| `<caption>`  | Table title |
| `<th scope>` | Header cell |
| `<td>`       | Data cell   |

**When to use:**

Financial reports · comparison matrices · data grids (consider ARIA for complex).

#### Common pitfalls

- Layout tables (use CSS Grid/Flex).
- Missing headers on data tables.
- `th` without `scope` in simple tables (still better with scope).

**Examples:**

```html
<table>
  <caption>
    Q4 sales by region
  </caption>
  <thead>
    <tr>
      <th scope="col">Region</th>
      <th scope="col">Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">EMEA</th>
      <td>$1.2M</td>
    </tr>
  </tbody>
</table>
```

**Sources:** [MDN — table](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)

</details>

<details>
  <summary>iframe and embed security</summary>

### iframe and embed security

**Definition:**

The `<iframe>` embeds another document. Restrict capabilities with `sandbox`, set `title` for accessibility, and control referrer with `referrerpolicy`.

| Attribute        | Purpose                       |
| ---------------- | ----------------------------- |
| `sandbox`        | Restrict scripts, forms, etc. |
| `referrerpolicy` | Control Referer header        |
| `title`          | Required for screen readers   |

**How it works:**

- `<iframe>` embeds another browsing context.
- `sandbox` attribute restricts scripts, forms, popups.
- Always set `title` for accessibility.

| Attribute        | Purpose                 |
| ---------------- | ----------------------- |
| `sandbox`        | Capability restrictions |
| `referrerpolicy` | Referer control         |
| `title`          | Accessible name         |

**When to use:**

Payment widgets · video embeds · isolated third-party content.

#### Common pitfalls

- Missing `sandbox` on untrusted embeds.
- `allow-scripts allow-same-origin` together negates isolation.
- No `title` (SR users hear "frame" only).

**Examples:**

```html
<iframe
  src="https://example.com/embed"
  sandbox="allow-scripts allow-same-origin"
  referrerpolicy="no-referrer"
  title="Embedded content"
></iframe>
```

**Sources:** [MDN — iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)

</details>

<details>
  <summary>Miscellaneous elements</summary>

### Miscellaneous elements

**Definition:**

HTML includes specialized elements for suggestions, progress, output, and no-JS fallbacks.

| Element      | Purpose                        |
| ------------ | ------------------------------ |
| `<datalist>` | Suggestions for `<input list>` |
| `<meter>`    | Scalar value in known range    |
| `<progress>` | Task completion                |
| `<noscript>` | Fallback when JS disabled      |

**How it works:**

- `<datalist>` — suggestions for `<input list>`.
- `<meter>` — gauge in known range (not progress).
- `<progress>` — task completion.
- `<noscript>` — fallback when JS disabled.

| Element      | Purpose              |
| ------------ | -------------------- |
| `<datalist>` | Input suggestions    |
| `<progress>` | Completion indicator |
| `<noscript>` | No-JS fallback       |

**When to use:**

Autocomplete suggestions · upload progress · critical content fallback.

#### Common pitfalls

- Confusing `<meter>` with `<progress>`.
- Relying on `<noscript>` for essential functionality only.
- `datalist` as sole validation (not enforced).

**Examples:**

```html
<input list="browsers" name="browser" />
<datalist id="browsers">
  <option value="Chrome"></option>
  <option value="Firefox"></option>
</datalist>
<noscript><p>Enable JavaScript for full experience.</p></noscript>
```

**Sources:** [MDN — datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)

</details>

---

<a name="forms--validation"></a>

## Attributes reference

<details>
  <summary>Common attributes</summary>

### Common attributes

**Definition:**

Attributes provide metadata and behavior on HTML elements — global attributes work on any element; others are element-specific.

| Attribute | Element(s)          | Purpose               |
| --------- | ------------------- | --------------------- |
| `src`     | img, script, iframe | Resource URL          |
| `href`    | a, link             | Link target           |
| `class`   | Global              | CSS hook              |
| `id`      | Global              | Unique identifier     |
| `name`    | form controls       | Key in submitted data |

**How it works:**

- Element-specific and global attributes modify behavior.
- `src`/`href` locate resources; `name` keys form data; `class`/`id` hook CSS/JS.

| Attribute | Purpose        |
| --------- | -------------- |
| `src`     | Resource URL   |
| `name`    | Form field key |

**When to use:**

Image `src`+`alt` · link `href` · form control `name`.

#### Common pitfalls

- `id` reused on page.
- `class` for JS-only hooks (prefer `data-*`).
- Missing `alt` when using `src` on img.

**Examples:**

```html
<img src="/logo.png" alt="Logo" class="brand" id="site-logo" />
<a href="/about" class="nav-link">About</a>
<input id="email" name="email" type="email" required />
```

**Sources:** [MDN — Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)

</details>

<details>
  <summary>class vs id</summary>

### class vs id

**Definition:**

`class` can appear on many elements for shared styling. `id` must be unique per page — used for anchors, label association, and unique targets.

|              | `class`         | `id`                 |
| ------------ | --------------- | -------------------- |
| Cardinality  | Many elements   | One per page         |
| CSS selector | `.my-class`     | `#my-id`             |
| Use for      | Shared patterns | Unique anchor/target |

**How it works:**

- `class` — reusable, many per page, CSS `.class`.
- `id` — unique per document, `#id` selector, fragment targets.
- `label for` and ARIA reference `id`.

|       | `class` | `id`         |
| ----- | ------- | ------------ |
| Count | Many    | One per page |

**When to use:**

Shared card style (`class`) · skip link target (`id="main"`).

#### Common pitfalls

- Duplicate ids (invalid).
- Styling with ids (high specificity).
- Auto-generated ids colliding in loops.

**Examples:**

```html
<p class="intro highlight">Shared classes</p>
<main id="main">Unique landmark anchor</main>
<label for="email">Email</label>
```

**Sources:** [MDN — id](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id)

</details>

<details>
  <summary>Global attributes</summary>

### Global attributes

**Definition:**

Global attributes can be used on any HTML element — `class`, `id`, `hidden`, `inert`, `tabindex`, `data-*`, and all `aria-*` attributes.

| Attribute          | Purpose                    |
| ------------------ | -------------------------- |
| `data-*`           | Custom data for JS/CSS     |
| `aria-*`           | Accessibility semantics    |
| `tabindex`         | Focus order (`0` or `-1`)  |
| `hidden` / `inert` | Visibility and interaction |

**How it works:**

- Valid on any element: `class`, `id`, `hidden`, `inert`, `tabindex`, `data-*`, `aria-*`, `lang`, `title`.

| Attribute | Purpose       |
| --------- | ------------- |
| `data-*`  | Custom data   |
| `aria-*`  | Accessibility |

**When to use:**

`data-user-id` for JS · `aria-label` on icon buttons · `inert` during modal.

#### Common pitfalls

- `tabindex="1"` positive values (avoid).
- `title` as primary label (unreliable).
- Invalid `data-*` names (uppercase).

**Examples:**

```html
<div data-user-id="42" aria-label="User profile card" tabindex="0"></div>
```

**Sources:** [MDN — Global attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)

</details>

---

<a name="interview-practice"></a>
