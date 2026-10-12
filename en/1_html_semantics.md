# HTML — Semantics

Landmark regions, headings, lists, and phrasing elements that carry meaning.

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Semantic markup](#semantic-markup)

---

## Semantic markup

<details>
  <summary>Semantics</summary>

### Semantics
**Definition:**

Semantic markup uses elements for their intended meaning, not just visual appearance. It improves accessibility, SEO, and maintainability.

| Benefit | How semantics help |
|---------|-------------------|
| Accessibility | Screen readers understand structure |
| SEO | Search engines index meaning |
| Maintainability | Styles change without losing meaning |

**How it works:**

- Elements carry meaning beyond default styling.
- Assistive tech maps roles from native tags.
- Search engines use headings/landmarks for outline.
- CSS can change; semantics should not.

| Benefit | How |
|---------|-----|
| Accessibility | SR announces `<nav>`, `<button>` correctly |
| SEO | Heading hierarchy signals topics |
| Maintainability | Meaning survives CSS refactors |

**When to use:**

Public content sites · forms · design systems with native widgets.

#### Common pitfalls

- `<div class="btn">` instead of `<button>`.
- Div-soup layouts with no landmarks.
- ARIA roles duplicating native elements.

**Examples:**

```html
<nav aria-label="Main">
  <a href="/">Home</a>
</nav>
<main><article><h1>Post</h1></article></main>
```

**Sources:** [MDN — Semantics](https://developer.mozilla.org/en-US/docs/Glossary/Semantics)

</details>

<details>
  <summary>Landmark elements</summary>

### Landmark elements

**Definition:**

Landmark elements define major page regions so assistive technology and search engines can navigate the document outline.

| Element | Purpose |
|---------|---------|
| `<header>` | Introductory content |
| `<nav>` | Navigation links |
| `<main>` | Primary unique content (one per page) |
| `<article>` | Self-contained content |
| `<aside>` | Tangentially related content |
| `<footer>` | Footer for page or section |

**How it works:**

- Landmarks: `header`, `nav`, `main`, `footer`, `aside`, `section`, `article`.
- Screen readers provide landmark navigation shortcuts.
- One `<main>` per page; label multiple `nav` with `aria-label`.

| Element | Purpose |
|---------|--------|
| `<main>` | Primary unique content |
| `<nav>` | Navigation link groups |
| `<aside>` | Tangential content |

**When to use:**

Marketing site · blog (`article`) · app shell with `aside` sidebar.

#### Common pitfalls

- Multiple `<main>` elements.
- `<section>` without heading (anonymous region).
- `<header>`/`<footer>` inside every card (over-nesting).

**Examples:**

```html
<header><p>Site title</p></header>
<nav aria-label="Main"><a href="/">Home</a></nav>
<main><article><h1>Post title</h1><p>Content</p></article></main>
<footer><p>&copy; 2025</p></footer>
```

**Sources:** [MDN — Landmarks](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/landmark_role)

</details>

<details>
  <summary>Heading hierarchy</summary>

### Heading hierarchy

**Definition:**

Headings (`h1`–`h6`) create a logical document outline. Use one `<h1>` per page and do not skip levels.

| Rule | Reason |
|------|--------|
| One `<h1>` per page | Clear page topic |
| No skipped levels | Predictable outline for screen readers |
| Heading per section | Thematic grouping |

**How it works:**

- `h1`–`h6` define document outline levels.
- One logical `h1` per page (topic title).
- Do not skip levels (`h1` → `h3`).
- Section headings should describe the section below.

| Rule | Reason |
|------|--------|
| One `h1` | Clear page topic |
| No skipped levels | Predictable SR outline |

**When to use:**

Article page · settings page with sections · modal with its own `h2`.

#### Common pitfalls

- Choosing heading level for font size (use CSS).
- Empty sections with headings only.
- `h1` in every component on SPA pages.

**Examples:**

```html
<h1>Product catalog</h1>
  <h2>Electronics</h2>
    <h3>Phones</h3>
```

**Sources:** [MDN — Headings](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)

</details>

<details>
  <summary>Figure and navigation patterns</summary>

### Figure and navigation patterns

**Definition:**

The `<figure>` element groups self-contained content with an optional `<figcaption>`. `<nav>` wraps navigation link lists.

| Element | Use |
|---------|-----|
| `<figure>` | Image, diagram, code with caption |
| `<figcaption>` | Caption for figure |
| `<nav>` | Group of navigation links |

**How it works:**

- `<figure>` groups media + optional `<figcaption>`.
- `<nav>` wraps major navigation link lists.
- Use `aria-label` when multiple nav regions exist.

| Element | Use |
|---------|-----|
| `<figure>` | Self-contained media + caption |
| `<nav>` | Link groups for navigation |

**When to use:**

Charts with captions · image galleries · footer vs main nav.

#### Common pitfalls

- `<nav>` around every link group (overuse).
- Figcaption missing for informative images in figure.
- Navigation built from `<div onclick>`.

**Examples:**

```html
<figure>
  <img src="chart.png" alt="Sales grew 40% in Q4" />
  <figcaption>Q4 sales chart</figcaption>
</figure>
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>
```

**Sources:** [MDN — figure](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/figure)

</details>

<details>
  <summary>Text semantics</summary>

### Text semantics

**Definition:**

Phrasing elements convey meaning for text — emphasis, abbreviations, quotations, code, and dates.

| Element | Meaning |
|---------|---------|
| `<strong>` | Strong importance |
| `<em>` | Stress emphasis |
| `<abbr title>` | Abbreviation |
| `<time datetime>` | Machine-readable date |
| `<code>` / `<pre>` | Code snippets |

**How it works:**

- Phrasing elements: `strong`, `em`, `abbr`, `time`, `code`, `pre`, `mark`.
- `strong`/`em` carry meaning; `b`/`i` are stylistic defaults.
- `time datetime` enables machine-readable dates.

| Element | Meaning |
|---------|--------|
| `<strong>` | Importance |
| `<em>` | Stress emphasis |
| `<time datetime>` | Machine-readable date |

**When to use:**

Warnings (`strong`) · emphasized phrase (`em`) · publish dates (`time`).

#### Common pitfalls

- Using headings for bold text.
- `abbr` without `title` (still OK if expanded in text).
- `code` for non-code bold styling.

**Examples:**

```html
<p><strong>Warning:</strong> <em>Do not</em> skip heading levels.</p>
<abbr title="HyperText Markup Language">HTML</abbr>
<time datetime="2025-07-02">July 2, 2025</time>
```

**Sources:** [MDN — strong](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/strong)

</details>

<details>
  <summary>details and summary (native disclosure)</summary>

### details and summary (native disclosure)

**Definition:**

The `<details>` and `<summary>` elements provide native expand/collapse UI without JavaScript.

| Element | Role |
|---------|------|
| `<details>` | Disclosure widget container |
| `<summary>` | Visible label / toggle |

**How it works:**

- `<details>` is a disclosure widget; open state is native.
- `<summary>` is the visible toggle (first child).
- No JS required; keyboard accessible by default.

| Element | Role |
|---------|------|
| `<details>` | Expand/collapse container |
| `<summary>` | Toggle label |

**When to use:**

FAQ accordions · optional form sections · help panels.

#### Common pitfalls

- Nesting interactive elements inside `<summary>`.
- Using for primary navigation (use `<nav>`).
- Hiding critical content collapsed by default without hint.

**Examples:**

```html
<details>
  <summary>Shipping info</summary>
  <p>Free shipping on orders over $50.</p>
</details>
```

**Sources:** [MDN — details](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details)

</details>

<details>
  <summary>Lists</summary>

### Lists

**Definition:**

HTML provides three list types for unordered, ordered, and definition-style content.

| Element | Type |
|---------|------|
| `<ul>` + `<li>` | Unordered list |
| `<ol>` + `<li>` | Ordered list |
| `<dl>` + `<dt>` + `<dd>` | Definition list |

**How it works:**

- `<ul>` unordered, `<ol>` ordered, `<dl>` definition list (`dt`/`dd`).
- Lists must contain `<li>` (or `dt`/`dd` pairs).
- CSS `list-style` controls bullets; semantics stay.

| Element | Type |
|---------|------|
| `<ul>` | Unordered |
| `<ol>` | Ordered |
| `<dl>` | Term/definition |

**When to use:**

Nav menus (`ul` + `li` + `a`) · step instructions (`ol`) · glossaries (`dl`).

#### Common pitfalls

- `<div>` bullets instead of list markup.
- Empty `<li>` for spacing.
- Using tables for non-tabular lists.

**Examples:**

```html
<ul><li>Item one</li><li>Item two</li></ul>
<ol><li>First</li><li>Second</li></ol>
<dl><dt>HTML</dt><dd>Hypertext Markup Language</dd></dl>
```

**Sources:** [MDN — Lists](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul)

</details>

---

### HTML semantics

**Definition:** Semantic HTML uses elements for their intended meaning (`<nav>`, `<main>`, `<h1>`) rather than generic `<div>` wrappers. Visual CSS can change; the meaning should stay in the markup.

**How it works:**

- Semantic elements expose implicit ARIA roles to assistive technology.
- Landmarks (`header`, `nav`, `main`, `aside`, `footer`) and headings form the document outline.
- Prefer native tags over ARIA roles that duplicate the same meaning.

| Pattern | Semantic choice | Avoid |
|---------|-----------------|-------|
| Navigation | `<nav>` + `<a href>` | `<div onclick>` |
| Primary content | `<main>` | `<div class="main">` |
| Page topic | one `<h1>` | heading level chosen for font size |
| User action | `<button>` | `<div class="btn">` |

**When to use:** Marketing sites, articles, app shells — any page that needs a navigable outline. Form *controls* and validation → [HTML — Forms](./1_html_forms.md).

#### Common pitfalls

- `<div class="btn">` instead of `<button>`.
- Div-soup layouts with no landmarks.
- ARIA roles duplicating native elements.

**Pluses and minuses:**

| + | − |
|------|------|
| Free accessibility and SEO outline | Custom designs may tempt non-semantic markup |
| Native roles without extra ARIA | Complex widgets may still need ARIA |

**Examples:**

```html
<header>
  <nav aria-label="Main">
    <a href="/">Home</a>
  </nav>
</header>
<main>
  <article>
    <h1>Post title</h1>
  </article>
</main>
```

**Thoughts:** Semantics is document meaning. Forms collect named input — keep those as a separate study sitting.

**Sources:** [MDN — Semantics](https://developer.mozilla.org/en-US/docs/Glossary/Semantics) · [MDN — Structuring documents](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Structuring_documents)

---

<a name="semantic-markup"></a>
