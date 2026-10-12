# HTML — Core

What HTML is, how markup models content, and how it fits with CSS and JavaScript.

[← Back to hub](./1_html.md)

### Related docs

- [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Main concepts

| Area         | Topics                                         |
| ------------ | ---------------------------------------------- |
| **Language** | What HTML is, living standard, HTML vs XML     |
| **Model**    | Elements, tags, attributes, content categories |
| **Platform** | HTML + CSS + JavaScript layers                 |

### Sections

- [Language & standard](#language-standard)
- [Markup model](#markup-model)
- [Web platform layers](#web-platform-layers)

---

<a name="language-standard"></a>

## Language & standard

<details>
  <summary>What is HTML</summary>

### What is HTML

**Definition:**

**HTML** (HyperText Markup Language) is the standard markup for documents on the web. It describes **structure** and **meaning** of content — headings, paragraphs, links, forms, media — not visual styling or application logic.

| Term                | Meaning                                                                         |
| ------------------- | ------------------------------------------------------------------------------- |
| **Markup**          | Tags and attributes annotate text; browser interprets them into a document tree |
| **HyperText**       | Links connect documents into a graph of resources                               |
| **Not programming** | HTML declares content; behavior belongs in scripts                              |

**How it works:**

- Authors write text with **elements** (e.g. `<p>`, `<a href="…">`).
- The browser **parses** markup into a **DOM** tree (nodes for elements, text, comments).
- **CSS** maps presentation to that tree; **JavaScript** can read and update the DOM.

**When to use:**

Every web page or app shell needs HTML (or an equivalent DOM produced by a framework). Prefer semantic elements over generic `<div>` when the meaning is known.

**Pluses and minuses:**

| +                                          | −                                                    |
| ------------------------------------------ | ---------------------------------------------------- |
| Declarative, cacheable, works without JS   | Easy to produce invalid or non-semantic markup       |
| Universal support in browsers and crawlers | Presentation mixed into markup hurts maintainability |

**Examples:**

```html
<h1>Product</h1>
<p>Short description with a <a href="/docs">link</a>.</p>
```

**Sources:** [MDN — Introduction to HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)

</details>

<details>
  <summary>HTML Living Standard</summary>

### HTML Living Standard

**Definition:**

Modern HTML is maintained as a **living standard** by WHATWG (with browser vendors). It evolves continuously; there is no separate “HTML5 finished” version number in day-to-day work.

**How it works:**

- [WHATWG HTML](https://html.spec.whatwg.org/) is the canonical spec for parsing and semantics.
- Browsers implement the spec; **invalid markup** is often **error-corrected** (not like strict XML).
- New elements and attributes (e.g. `<dialog>`, `popover`) land in the living standard and ship in browsers on rolling releases.

**When to use:**

Check MDN or the living standard when unsure if a tag or attribute is supported; use progressive enhancement for newer features.

**Thoughts:** Interview answers often contrast “HTML5” (era of APIs + semantics) with the ongoing living standard.

**Sources:** [WHATWG HTML](https://html.spec.whatwg.org/) · [MDN — HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)

</details>

<details>
  <summary>HTML vs XML</summary>

### HTML vs XML

**Definition:**

HTML is a **fixed** vocabulary for web documents. **XML** is a syntax for **custom** tag names — data and documents, not a single UI language.

**How it works:**

- XML is strict: one root, closed tags, quoted attributes; parsers fail on errors.
- HTML5 parsers **recover** from common mistakes (unclosed tags, optional quotes).
- `DOCTYPE` switches HTML **document modes**; XML may use `<?xml …?>`. SVG/MathML in XML files often need `xmlns`; inline SVG in HTML usually does not.

**When to use:**

Interview “how is XML not HTML?” · config/API payloads (often JSON today) · SVG as standalone XML.

**Thoughts:** HTML is for display in browsers; XML (and JSON) often carry data. Deeper XML syntax: [Web API — XML](./6_web_api_rest.md#xml).

**Sources:** [MDN — XML introduction](https://developer.mozilla.org/en-US/docs/Web/XML/Guides/XML_introduction)

</details>

---

<a name="markup-model"></a>

## Markup model

<details>
  <summary>Elements, tags, and attributes</summary>

### Elements, tags, and attributes

**Definition:**

An **element** is a node in the document tree. In source, it is written with **tags**; **attributes** on the opening tag add names, links, media URLs, or behavior hints.

| Piece       | Example         | Role                                 |
| ----------- | --------------- | ------------------------------------ |
| Opening tag | `<a href="/x">` | Starts element; may carry attributes |
| Content     | `Read more`     | Text or child elements               |
| Closing tag | `</a>`          | Ends element (void elements omit it) |
| Attribute   | `href="/x"`     | `name="value"` or boolean attribute  |

**How it works:**

- **Global attributes** (`id`, `class`, `lang`, `data-*`, …) work on any element — see [Global attributes](./1_html_global_attributes.md).
- **Element-specific** attributes (`href` on `<a>`, `src` on `<img>`, …) — see [Attributes](./1_html_attributes.md) and [Attributes by element](./1_html_attributes_by_element.md).
- **Boolean attributes** (`disabled`, `checked`, …) are true when present; value often omitted.

**When to use:**

Choose the element for **meaning** ([Semantics](./1_html_semantics.md)); use attributes for links, media, form names, and accessibility (`alt`, `aria-*` where needed).

**Examples:**

```html
<img src="/logo.png" alt="Company" width="120" height="40" />
<input name="email" type="email" required />
```

**Sources:** [MDN — Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes) · [MDN — Elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

</details>

<details>
  <summary>Content categories</summary>

### Content categories

**Definition:**

The HTML spec groups elements into **content categories** (e.g. **flow**, **phrasing**, **interactive**) that define where an element may appear and what it may contain.

**How it works:**

- **Flow content** — most body-level elements (`<p>`, `<section>`, `<div>`, …).
- **Phrasing content** — inline semantics (`<span>`, `<em>`, `<a>`, …).
- **Sectioning** — outline landmarks (`<article>`, `<nav>`, `<section>`, …) — details in [Semantics](./1_html_semantics.md).
- Putting a block element inside a phrasing-only context is **invalid** and may be fixed by the parser in surprising ways.

**When to use:**

Debugging “why did the browser break my layout?” · choosing valid nesting for forms and rich text.

**Thoughts:** Full element lists → [Elements reference](./1_html_elements.md) (in progress); skeleton and head/body → [Structure](./1_html_structure.md).

**Sources:** [MDN — Content categories](https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Content_categories)

</details>

<details>
  <summary>Semantic markup principles</summary>

### Semantic markup principles

**Definition:**

**Semantic markup** uses elements for their **meaning**, not only for default styling. Structure should still make sense with CSS disabled and should expose roles to assistive tech.

**How it works:**

- Prefer `<button>` for actions, `<a href>` for navigation, `<h1>`–`<h6>` for headings (one logical outline).
- Avoid “div soup” when a landmark or text-level semantic exists.
- **Presentation** belongs in CSS; **behavior** in JS — not `onclick` on non-interactive elements without roles.

**When to use:**

New components, design handoff, accessibility reviews, SEO (clear headings and landmarks).

**Pluses and minuses:**

| +                                  | −                                                 |
| ---------------------------------- | ------------------------------------------------- |
| Better a11y and SEO with less ARIA | Legacy designs may rely on non-semantic hooks     |
| Easier team reasoning about DOM    | Frameworks sometimes obscure tags in abstractions |

**Examples:**

```html
<!-- Prefer -->
<nav aria-label="Main">
  <ul>
    <li><a href="/">Home</a></li>
  </ul>
</nav>

<!-- Avoid when meaning is navigation -->
<div class="nav" onclick="go()">Home</div>
```

**Sources:** [MDN — Semantics](https://developer.mozilla.org/en-US/docs/Glossary/Semantics) · [HTML — Semantics](./1_html_semantics.md)

</details>

---

<a name="web-platform-layers"></a>

## Web platform layers

<details>
  <summary>HTML, CSS, and JavaScript</summary>

### HTML, CSS, and JavaScript

**Definition:**

The classic **web platform stack** splits responsibilities:

| Layer               | Technology | Responsibility                               |
| ------------------- | ---------- | -------------------------------------------- |
| Structure & meaning | HTML       | Document tree, semantics, forms, media hooks |
| Presentation        | CSS        | Layout, typography, color, motion            |
| Behavior            | JavaScript | Logic, fetching data, updating DOM           |

**How it works:**

- HTML loads first; CSS blocks or defers rendering depending on how it is linked; JS can block parsing unless `defer` / `module` / end-of-body patterns are used — see [Performance](./1_html_performance.md).
- **Unobtrusive** pattern: HTML works without JS; JS enhances.
- Frameworks (React, Vue, …) still emit HTML/DOM; the split of concerns remains at the product level.

**When to use:**

Architecture discussions, interview “PEA” / three-layer answers, deciding what belongs in templates vs styles vs modules.

**Thoughts:** Mnemonic **PEA** — **P**latform (HTML), **E**ngine (JS), **A**ppearance (CSS).

**Sources:** [MDN — How the web works](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works)

</details>

<details>
  <summary>Validation and conformance</summary>

### Validation and conformance

**Definition:**

**Conformance** means your document follows HTML rules (valid nesting, required attributes, unique `id`s). **Validators** report errors and warnings; browsers may still render invalid pages via error recovery.

**How it works:**

- [W3C Markup Validation Service](https://validator.w3.org/) checks uploaded markup or URLs.
- Linters in CI (eslint-plugin-html, etc.) catch issues in templates.
- Accessibility audits (axe, Lighthouse) complement syntax validation.

**When to use:**

Before release, after large template refactors, when copy-pasting from WYSIWYG tools.

**Pluses and minuses:**

| +                                       | −                                         |
| --------------------------------------- | ----------------------------------------- |
| Catches typos and invalid nesting early | Not every warning blocks real users       |
| Teaches spec rules                      | Framework output can be noisy to validate |

**Sources:** [W3C Validator](https://validator.w3.org/) · [MDN — Debugging HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Debugging_HTML)

</details>
