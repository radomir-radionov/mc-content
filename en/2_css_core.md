# CSS — Core

How CSS is added, cascade, selectors, and the box model.

[← Back to hub](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Sections

- [Core](#core)
- [Selectors & Cascade](#selectors-cascade)
- [Box Model & Display](#box-model-display)

---

<a name="fundamentals"></a>

## Core

<details>
  <summary>Ways to add CSS</summary>

### Ways to add CSS

**Definition:**

CSS can be linked externally, embedded in `<style>`, or applied inline on elements; external stylesheets are the production default.

| Method | Syntax | Use case |
|--------|--------|----------|
| **External** | `<link rel="stylesheet" href="styles.css">` | Production default — cacheable, reusable |
| **Internal** | `<style>` in `<head>` | Single-page prototypes, critical CSS |
| **Inline** | `style="..."` on element | Avoid except dynamic one-offs; specificity 1000 |

**How it works:** prefer `<link rel="stylesheet">` over CSS `@import` — `@import` blocks parallel downloads and adds a serial request chain. Preload critical CSS: `<link rel="preload" href="critical.css" as="style">`.

**Sources:** [MDN — Using CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/How_CSS_is_structured)

</details>

<details>
  <summary>@import anti-patterns</summary>

### @import anti-patterns

**Definition:** `@import` loads another stylesheet from within CSS — convenient but harmful to performance and maintainability compared to `<link>` tags or bundler imports.

**How it works:**

- Browser must parse the parent CSS before discovering `@import` URLs — serializes downloads.
- `@import` inside `<style>` or late-loaded CSS delays everything it pulls in.
- Nested `@import` chains multiply latency; order depends on source position.

**When to use:** Avoid in production — use `<link rel="stylesheet">`, bundler entry (`import './base.css'`), or `@layer` for ordering. Legacy exception: print-only `@import url("print.css") print;` when isolated.

**Pluses and minuses:**

| + | − |
|------|------|
| Split files without HTML changes | Blocks parallel fetch vs `<link>` |
| Works in plain CSS projects | Hard to trace in DevTools cascade |
| | Bundlers may duplicate if mixed with JS imports |

**Examples:**

```css
/* Anti-pattern — blocks rendering */
@import url("reset.css");
@import url("components.css"); /* waits for reset to parse */

/* Prefer in HTML */
/* <link rel="stylesheet" href="reset.css"> */
/* <link rel="stylesheet" href="components.css"> */
```

**Sources:** [MDN — @import](https://developer.mozilla.org/en-US/docs/Web/CSS/@import)

</details>

<details>
  <summary>CSS rule syntax</summary>

### CSS rule syntax

**Definition:** A CSS rule pairs a selector with a declaration block of `property: value` pairs that style matching elements.

**Examples:**

```css
selector {
  property: value;
  another-property: value;
}
```

Multiple selectors (grouping): `h1, .price { color: navy; }`  
Chaining (AND): `.card.featured { }` — element must match all parts.

**Sources:** [MDN — CSS syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/Syntax)

</details>

<details>
  <summary>Global keywords</summary>

### Global keywords

**Definition:**

CSS global keywords (`inherit`, `initial`, `unset`, `revert`) control how property values reset or cascade.

| Keyword | Behavior |
|---------|----------|
| `inherit` | Take computed value from parent |
| `initial` | Reset to CSS spec default |
| `unset` | `inherit` if inheritable, else `initial` |
| `revert` | Roll back to user-agent stylesheet |
| `revert-layer` | Roll back within the current `@layer` |

**How it works:**

- `inherit` — child takes parent's **computed** value for that property.
- `initial` — CSS-wide initial value (not always the UA default for that element).
- `unset` — inheritable properties → `inherit`; others → `initial`.
- `revert` — UA stylesheet value for that element/property.

**When to use:** `all: unset` on buttons before custom styling; `color: revert` to restore UA link colors; `display: revert` to undo author overrides on semantic elements.

**Pluses and minuses:**

| + | − |
|------|------|
| Reset without duplicating rules | `initial` ≠ element default (e.g. `display` on `<div>`) |
| `all: unset` strips author styles fast | `revert` support still maturing in older browsers |

**Examples:**

```css
button {
  all: unset;
  cursor: pointer;
}

a { color: revert; }
```

**Sources:** [MDN — CSS values](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Values_and_Units)

</details>

<details>
  <summary>Code style — property order</summary>

### Code style — property order

**Definition:**

Grouping CSS properties in a consistent order (position → box → typography → decoration → animation) improves readability and review speed.

Group properties for readability (positioning → box model → typography → decoration → animation → misc):

**Examples:**

```css
.card {
  /* Positioning */
  position: relative;
  z-index: 1;

  /* Box model */
  display: flex;
  width: 100%;
  max-width: 320px;
  margin: 0 auto;
  padding: 1rem;

  /* Typography */
  font: 400 1rem/1.5 system-ui, sans-serif;
  color: #333;

  /* Decoration */
  background: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 8px;

  /* Animation */
  transition: box-shadow 0.2s ease;

  /* Misc */
  will-change: auto;
}
```

**Sources:**

[Netology CSS codestyle](https://netology-university.bitbucket.io/codestyle/css/) · [CSS for JS](https://learn.javascript.ru/css-for-js)

> `float` in older style guides is legacy — prefer Flexbox/Grid for layout.

[Netology CSS codestyle](https://netology-university.bitbucket.io/codestyle/css/)

</details>

<details>
  <summary>Reset.css vs Normalize.css</summary>

### Reset.css vs Normalize.css

**Definition:**

Reset strips browser defaults to a blank slate; Normalize preserves useful defaults while fixing cross-browser inconsistencies.

* **Reset** — strips most default browser styles to a blank slate
* **Normalize** — preserves useful defaults, fixes cross-browser inconsistencies

Modern projects often use a minimal reset or framework base (Tailwind Preflight, etc.).

---

**Sources:** [Normalize.css](https://necolas.github.io/normalize.css/)

</details>

<a name="selectors-cascade"></a>

### Cascade & specificity

**Definition:**

The cascade resolves conflicting rules by origin, importance, specificity, and source order; inheritance passes some properties from parent to child.

| Concept | Role |
|---------|------|
| **Cascade** | Picks the winning declaration when rules conflict |
| **Specificity** | Tuple score per selector — more specific wins |
| **Inheritance** | `color`, `font-*`, `line-height` flow down; layout props do not |
| **`@layer`** | Explicit layer order beats specificity among author styles |

**When to use:**

Every stylesheet — understanding cascade prevents specificity wars and `!important` hacks.

Inline `style=""` and ID selectors for styling — they raise specificity and hurt maintainability.

**Examples:**

```css
@layer reset, components, utilities;

@layer components {
  .button { background: #2563eb; }
}

.card .button { background: #1d4ed8; } /* wins: unlayered beats layered */
```

**Thoughts:** Render pipeline — CSS builds the CSSOM before layout → [1_html.md](./1_html.md).

**Sources:** [MDN — Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade) · [Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

### Box model & layout

**Definition:**

Every element is a box (content → padding → border → margin); layout systems position and distribute those boxes in 1D (flex) or 2D (grid).

| System | Best for |
|--------|----------|
| **Flexbox** | Toolbars, nav rows, centering, 1D alignment |
| **Grid** | Page shells, dashboards, responsive card matrices |
| **`border-box`** | Predictable sizing — `width` includes padding and border |
| **BFC** | Contains floats, prevents margin collapse |

**When to use:** Flex for components; Grid for page structure; both together in production layouts.

**Examples:**

```css
*, *::before, *::after { box-sizing: border-box; }

.app {
  display: grid;
  min-height: 100dvh;
  grid-template-columns: 240px 1fr;
  grid-template-areas: "sidebar main";
}

.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
```

**Thoughts:** Layout changes trigger reflow in the render pipeline → [1_html.md](./1_html.md).

**Sources:** [MDN — Box model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model) · [Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout) · [Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout)

### Responsive & modern CSS

**Definition:**

Responsive CSS adapts layouts to viewport and container size using fluid units, media/container queries, and modern features like custom properties and nesting.

| Feature | Purpose |
|---------|---------|
| **Media queries** | Viewport breakpoints, `prefers-reduced-motion`, dark mode |
| **Container queries** | Component adapts to parent width, not viewport |
| **`clamp()`** | Fluid typography and spacing without breakpoints |
| **Custom properties** | Runtime theming; JS-updatable tokens |
| **Modern CSS** | `@scope`, anchor positioning, scroll-driven animations |

**When to use:** Mobile-first `min-width` queries; `@container` for reusable components in sidebars and dashboards.

**Examples:**

```css
:root {
  --space-md: 1rem;
  --text: clamp(0.875rem, 0.8rem + 0.25vw, 1.125rem);
}

.card-wrap { container-type: inline-size; }

@container (min-width: 400px) {
  .card { display: grid; grid-template-columns: 120px 1fr; }
}

@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
}
```

**Sources:** [web.dev — Learn CSS](https://web.dev/learn/css/) · [MDN — Container queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)

## Selectors & Cascade

<details>
  <summary>Selector types</summary>

### Selector types

**Definition:**

CSS selectors target elements by type, class, ID, attribute, or pseudo-class/element; each type contributes to specificity.

| Type | Example | Specificity |
|------|---------|-------------|
| Universal | `*` | 0 |
| Type | `p`, `h1` | 0,0,1 |
| Class | `.btn` | 0,1,0 |
| ID | `#header` | 1,0,0 |
| Attribute | `[type="email"]`, `[href^="https"]` | 0,1,0 |
| Pseudo-class | `:hover`, `:nth-child(2)` | 0,1,0 |
| Pseudo-element | `::before`, `::selection` | 0,0,1 (acts like type) |

**Sources:** [MDN — Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors)

</details>

<details>
  <summary>Combinators</summary>

### Combinators

**Definition:**

Combinators define relationships between selectors — descendant, child, adjacent sibling, and general sibling.

| Combinator | Meaning | Example |
|------------|---------|---------|
| Descendant (space) | Any nested descendant | `.nav li` |
| Child `>` | Direct child only | `.nav > li` |
| Adjacent `+` | Immediate next sibling | `h2 + p` |
| General `~` | Any following sibling | `h2 ~ p` |

**Sources:** [MDN — Combinators](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Combinators)

</details>

<details>
  <summary>Class vs ID</summary>

### Class vs ID

**Definition:**

Classes are reusable styling hooks; IDs are unique per page and carry higher specificity — prefer classes for styling.

* **ID** — unique per page; high specificity; prefer for JS hooks or fragment targets, not styling
* **Class** — reusable; kebab-case (`card-title`); multiple per element (space-separated)

**How it works:**

- ID selector (`#header`) — specificity `(1,0,0)`; document-unique.
- Class selector (`.nav`) — specificity `(0,1,0)`; reusable; multiple per element.
- IDs valid for fragment targets (`#section`), `aria-labelledby`, `label for=`; classes for styling hooks.

**When to use:** Style with classes; reserve IDs for anchors and ARIA references. Avoid `#id .class` chains to win specificity — flatten selectors or use `@layer` instead.

**Pluses and minuses:**

| + (class) | − (ID for styling) |
|--------------|----------------------|
| Low specificity, easy overrides | IDs create specificity wars |
| Composable modifiers (`btn--primary`) | One per page — brittle for components |
| Team-friendly naming conventions | Hard to refactor at scale |

**Examples:**

```html
<header id="main-header" class="site-header">...</header>
<nav class="nav nav--compact">...</nav>
```

**Sources:** [MDN — ID selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/ID_selectors)

</details>

<details>
  <summary>Modern functional selectors</summary>

### Modern functional selectors

**Definition:** `:is()`, `:where()`, `:not()`, and `:has()` group or filter selectors with different specificity rules.

**Examples:**

```css
/* :is() — groups selectors; specificity = highest argument */
:is(h1, h2, h3) { margin-block: 0; }

/* :where() — same grouping, ZERO specificity */
:where(h1, h2, h3) { margin-block: 0; }

/* :not() — exclusion */
li:not(:last-child) { border-bottom: 1px solid #eee; }

/* :has() — parent selector ("has selector") */
.card:has(img) { padding-top: 0; }
.form:has(:invalid) .submit { opacity: 0.5; }
```

**Sources:** [MDN — :has()](https://developer.mozilla.org/en-US/docs/Web/CSS/:has)

</details>

<details>
  <summary>Specificity calculation</summary>

### Specificity calculation

**Definition:**

Specificity is a tuple score (inline, IDs, classes/attributes/pseudo-classes, types/pseudo-elements) that decides which rule wins in the cascade.

Count (inline, IDs, classes/attributes/pseudo-classes, types/pseudo-elements):

**How it works:**

later rule in source order wins.

> The old "1-10-100" mental model is a simplification. Use real specificity tuples or DevTools "Computed" panel.

| Selector | Specificity note |
|----------|------------------|
| `:not(.a)` | `:not()` itself is 0; argument `.a` adds `(0,1,0)` |
| `:is(.a, #x)` | Takes highest argument → `#x` = `(1,0,0)` |
| `:where(.a, #x)` | Always `(0,0,0)` |
| `::before` | Counts as a type pseudo-element `(0,0,1)` |

within the same origin, unlayered styles beat layered styles regardless of specificity. Among layered styles, layer order wins first, then specificity. `!important` inversions apply per origin — author `!important` beats author normal, but user `!important` can beat author `!important`.

```
Cascade resolution (simplified):
  1. Origin + importance (user !important > author !important > author normal > user normal > UA)
  2. @layer order (later layer wins; unlayered beats all layers)
  3. Specificity tuple
  4. Source order
```

**Examples:**

```
#nav .item a:hover     →  1,1,2
.btn.btn--primary     →  0,2,0
p                     →  0,0,1
style="" attribute    →  1000 (inline)
!important             →  beats normal declarations in same origin
```

**Sources:** [MDN — Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

</details>

<details>
  <summary>Cascade origins</summary>

### Cascade origins

**Definition:**

The cascade resolves conflicts across user-agent, user, and author stylesheets before specificity and source order.

| Origin | Source | Typical examples |
|--------|--------|------------------|
| **User-agent (UA)** | Browser default stylesheet | `display: block` on `div`, bold `h1` |
| **User** | User settings / extensions | High-contrast mode, custom fonts |
| **Author** | Your CSS (linked, embedded, inline) | App stylesheets |

**Inheritance** is separate from cascade — inherited values flow down the DOM tree but still participate in cascade when overridden on a child.

**`revert`** rolls back to the UA stylesheet for that property; `revert-layer` rolls back within the current `@layer`.

**Sources:** [MDN — Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)

</details>

<details>
  <summary>Inheritance</summary>

### Inheritance

**Definition:** Some CSS properties pass their computed values from parent to child; others do not. Inheritance is separate from the cascade — a child can inherit a value that still loses to a more specific author rule.

**How it works:**

- Inheritable: `color`, `font-*`, `line-height`, `text-align`, `visibility`, custom properties (`--*`), etc.
- Non-inheritable: `margin`, `padding`, `border`, `width`, `display`, `background`, etc.
- `inherit` keyword forces inheritance even on non-inheritable properties.
- `initial` / `unset` / `revert` interact with inheritance per property rules.

**When to use:** Set `font-family` / `color` on `body` or `:root`; use `color: inherit` on buttons/links; don't expect `width` or `margin` to flow down — set explicitly or use layout containers.

**Pluses and minuses:**

| + | − |
|------|------|
| Typography/color theming from `:root` | Easy to forget what inherits vs what doesn't |
| Custom properties cascade naturally | Inherited `visibility: hidden` hides all descendants |
| Less duplication for text styles | Layout props must be set per element |

**Examples:**

```css
:root {
  --brand: #2563eb;
  font-family: system-ui, sans-serif;
  color: #111;
}

.card {
  color: inherit;           /* explicit inherit on non-default */
  border: 1px solid #e5e7eb; /* does NOT inherit from parent */
}
```

**Sources:** [MDN — Inheritance](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Inheritance)

</details>

<details>
  <summary>@layer (cascade layers)</summary>

### @layer (cascade layers)

**Definition:**

`@layer` declares explicit cascade layers so resets, components, and utilities stack in a predictable order.

Control cascade order explicitly — useful for design systems, resets, and utilities:

**Examples:**

```css
@layer reset, base, components, utilities;

@layer reset {
  * { margin: 0; }
}

@layer utilities {
  .hidden { display: none !important; }
}
```

Unlayered styles beat layered styles. Later layers beat earlier layers within the same origin.

**Sources:** [MDN — @layer](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer)

</details>

<details>
  <summary>Pseudo-classes</summary>

### Pseudo-classes

**Definition:**

Pseudo-classes select elements in a particular state or structural position (`:hover`, `:nth-child`, `:focus-visible`).

| Category | Selectors | Use case |
|----------|-----------|----------|
| **Interaction** | `:hover`, `:active` | Pointer feedback |
| **Focus** | `:focus`, `:focus-visible`, `:focus-within` | Keyboard vs mouse focus; parent when child focused |
| **Structure** | `:first-child`, `:last-child`, `:nth-child(n)`, `:only-child` | Position among all siblings |
| **Type structure** | `:first-of-type`, `:nth-of-type(n)` | Position among same tag siblings only |
| **Negation** | `:not(.disabled)` | Exclude matches |
| **Form state** | `:checked`, `:disabled`, `:invalid`, `:valid`, `:placeholder-shown`, `:required` | Native validation styling |
| **Navigation** | `:target` | URL fragment highlight |
| **Parent** | `:has(.error)` | Style parent based on descendant |

**How it works:** `:focus` fires on any focus (including mouse click); `:focus-visible` only when the UA determines focus should be visible (keyboard). Use `:focus-visible` for outline rings; never remove `:focus` without a replacement.

**Examples:**

```html
<div><p>A</p><span>B</span><p>C</p></div>
```

* `p:nth-child(2)` — no match (`span` is 2nd child)
* `p:nth-of-type(1)` — matches first `<p>` (A)
* `p:nth-of-type(2)` — matches second `<p>` (C)

**Sources:** [MDN — Pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)

</details>

<details>
  <summary>Pseudo-elements</summary>

### Pseudo-elements

**Definition:**

Pseudo-elements style generated sub-parts of an element (`::before`, `::after`, `::selection`).

Target sub-parts of an element (use `::`):

* `::before`, `::after` — generated content (`content` required)
* `::first-letter`, `::first-line`
* `::selection` — user-selected text
* `::placeholder` — input placeholder text

**Examples:**

```css
.tooltip::after {
  content: attr(data-tip);
  position: absolute;
}
```

**Sources:** [MDN — Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)

</details>

<details>
  <summary>attr() function</summary>

### attr() function

**Definition:**

The `attr()` function reads an HTML attribute value for use in CSS, commonly in `content` on pseudo-elements.

Reads an HTML attribute value for use in CSS (common with `content`):

**How it works:**

- `attr(name)` reads the attribute value as a string for `content`, and in modern browsers for other properties (`attr(data-label type(<string>))`).
- Typed `attr()` (CSS Values Level 5) allows `type(<color>)`, `type(<length>)`, etc. — check browser support.
- Empty/missing attributes yield empty string in `content`.

**When to use:** External link hints in print; decorative labels from `data-*`; counter badges. Always pair decorative `attr()` content with accessible names.

**Pluses and minuses:**

| + | − |
|------|------|
| DRY tooltips/labels from HTML data | Limited property support without typed `attr()` |
| No JS for decorative pseudo-content | Screen readers may not read `::after` content — add `aria-label` |
| Good for print link expansion | Security: never inject unescaped user HTML via attributes |

**Examples:**

```css
a[href^="http"]::after {
  content: " (" attr(href) ")";
}
```

**Sources:** [MDN — attr()](https://developer.mozilla.org/en-US/docs/Web/CSS/attr)

</details>

<a name="box-model-display"></a>

## Box Model & Display

<details>
  <summary>CSS box model</summary>

### CSS box model

**Definition:**

Every element is a rectangular box: content, padding, border, and margin — the foundation of layout sizing.

Every element is a rectangular box: **content → padding → border → margin**.

By default, `width` / `height` apply to the **content box** only; padding and border add to total size.

**How it works:** [Introduction to the CSS box model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model)

**Examples:**

```
┌──────────── margin ────────────┐
│  ┌──────── border ────────┐    │
│  │  ┌──── padding ────┐   │    │
│  │  │    content      │   │    │
│  │  └─────────────────┘   │    │
│  └────────────────────────┘    │
└────────────────────────────────┘
```

**Sources:** [MDN — Box model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model)

</details>

<details>
  <summary>box-sizing</summary>

### box-sizing

**Definition:** `box-sizing` controls whether `width`/`height` include padding and border (`border-box`) or content only (`content-box`).

**Examples:**

```css
*, *::before, *::after {
  box-sizing: border-box;
}
```

| Value | `width` includes |
|-------|------------------|
| `content-box` | content only (default) |
| `border-box` | content + padding + border |

Use `border-box` globally to make sizing predictable. `padding-box` exists but is rarely used.

**Sources:** [MDN — box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)

</details>

<details>
  <summary>Margin & padding</summary>

### Margin & padding

**Definition:**

Padding is space inside the border; margin is space outside. Vertical margins between block siblings can collapse.

* **Padding** — space inside border; affects background; does not collapse
* **Margin** — space outside border; can collapse

**How it works:**

vertical margins of adjacent block siblings collapse to the larger value. Does not apply to flex/grid items, floated elements, or horizontal margins.

* **Parent + first/last child** — top margin of first child can collapse with parent's top margin; same for bottom
* **Empty block** — top and bottom margins of an empty block collapse into one
* **Prevention** — padding/border on parent, `overflow: auto`, `display: flow-root`, or flex/grid on parent stops collapse

pull elements visually; can break layout — use sparingly.

**Sources:** [MDN — margin](https://developer.mozilla.org/en-US/docs/Web/CSS/margin)

</details>

<details>
  <summary>Containing block</summary>

### Containing block

**Definition:**

The containing block is the reference box used to compute an element's position and percentage-based sizes.

The **containing block** is the box used to calculate an element's size and position (`top`, `left`, `width`, `height`, percentages).

| `position` | Containing block is |
|------------|---------------------|
| `static` / `relative` | Nearest block ancestor's **content box** |
| `absolute` | Nearest **positioned** ancestor (`relative`, `absolute`, `fixed`, `sticky`) padding edge; if none, initial containing block (viewport) |
| `fixed` | Viewport (or transform/filter ancestor in some browsers) |
| `sticky` | Nearest scroll ancestor's content box |

**How it works:**

`position: relative` on parent does not change flow, but it **does** become the containing block for `position: absolute` children.

[Containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block)

**Examples:**

```css
.parent { position: relative; }
.child  { position: absolute; inset: 0; } /* fills .parent, not viewport */
```

Percentage `width`/`height` resolve against the containing block's width/height — if the containing block has no explicit height, `height: 100%` fails.

**Sources:** [MDN — Containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block)

</details>

<details>
  <summary>Intrinsic & extrinsic sizing</summary>

### Intrinsic & extrinsic sizing

**Definition:**

Intrinsic sizing derives size from content (`min-content`, `max-content`); extrinsic sizing is imposed from outside (`width`, `flex`).

| Keyword | Meaning |
|---------|---------|
| `min-content` | Smallest size without overflow (longest word / widest unbreakable content) |
| `max-content` | Size needed to fit all content on one line |
| `fit-content` | `min(max-content, max(min-content, stretch))` — shrink but not below min-content |
| `auto` | Browser-dependent; in flex/grid often means `minmax(auto, max-content)` |

**How it works:** default `min-width: auto` on flex items prevents shrinking below content — use `min-width: 0` or `minmax(0, 1fr)` in grid to allow truncation.

**Examples:**

```css
.sidebar { width: min(240px, 25vw); }
.card    { width: fit-content; }
.grid-col { grid-template-columns: minmax(0, 1fr); } /* allow shrink below content */
```

**Extrinsic sizing** — size imposed from outside (`width: 300px`, `flex: 1`). **Intrinsic sizing** — size derived from content (`width: max-content`, `width: auto` on inline-block).

**Sources:** [MDN — min-content](https://developer.mozilla.org/en-US/docs/Web/CSS/min-content)

</details>

<details>
  <summary>Block formatting context (BFC)</summary>

### Block formatting context (BFC)

**Definition:** A BFC is an isolated layout region where block boxes stack vertically and floats are contained.

**How it works:**

* `overflow` other than `visible` (`auto`, `hidden`, `scroll`)
* `display: flow-root` (preferred — no side effects)
* `display: flex` / `grid` on container
* `position: absolute` / `fixed`
* `float` other than `none`

* Contains floated children (prevents parent height collapse)
* Prevents margin collapsing with outside elements
* Isolates layout for independent z-index stacking

**Examples:**

```css
.clear-float-context {
  display: flow-root; /* modern clearfix replacement */
}
```

**Thoughts:** An isolated layout region where block boxes stack vertically and floats are contained.

**Sources:** [MDN — BFC](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)

</details>

<details>
  <summary>display</summary>

### display

**Definition:**

The `display` property defines how an element participates in layout — block, inline, flex, grid, or none.

| Value | Behavior |
|-------|----------|
| `block` | New line, full width available |
| `inline` | Flows with text, no width/height |
| `inline-block` | Inline flow, box model applies |
| `flex` | Flex container |
| `grid` | Grid container |
| `none` | Removed from layout and accessibility tree |
| `contents` | Box disappears; children remain |
| `flow-root` | Creates BFC; contains floats |

**Sources:** [MDN — display](https://developer.mozilla.org/en-US/docs/Web/CSS/display)

</details>

<details>
  <summary>display: contents</summary>

### display: contents

**Definition:** `display: contents` makes the element's box disappear — its children participate in the parent's layout as if the wrapper were not there.

**How it works:**

- The element is not rendered as a box; children are hoisted to the parent formatting context.
- Useful for semantic wrappers (`<li>` around flex children, table fixups) without extra layout boxes.
- Pseudo-elements (`::before`/`::after`) on the `contents` element may still generate boxes.

**When to use:** List item wrappers in flex/grid layouts; table accessibility fixes. **Avoid** on interactive or landmark elements (`<button>`, `<nav>`, `<main>`) — use a real layout value or add explicit ARIA roles on children.

**Pluses and minuses:**

| + | − |
|------|------|
| Semantic HTML without layout penalty | **Accessibility:** element removed from a11y tree in many browsers |
| Fixes table/flex participation issues | No box = no `background`, `border`, `padding` on wrapper |
| Cleaner grid/flex item lists | Unpredictable with positioned descendants |

**Examples:**

```css
/* Semantic <li> wrappers in a flex toolbar */
.toolbar > li {
  display: contents;
}

/* a11y warning — screen readers may skip the <li> box */
/* Prefer restructuring markup or use role="listitem" on children if needed */
```

**Sources:** [MDN — display: contents](https://developer.mozilla.org/en-US/docs/Web/CSS/display#contents)

</details>

<details>
  <summary>visibility vs display</summary>

### visibility vs display

**Definition:**

`display: none` removes an element from layout; `visibility: hidden` keeps its space but hides painting.

| Property | In layout? | Painted? | Events? |
|----------|------------|----------|---------|
| `display: none` | No | No | No |
| `visibility: hidden` | Yes | No | No |
| `opacity: 0` | Yes | No* | Yes (unless `pointer-events: none`) |

**Sources:** [MDN — visibility](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility)

</details>

<details>
  <summary>overflow, aspect-ratio, object-fit</summary>

### overflow, aspect-ratio, object-fit

**Definition:** `overflow` clips or scrolls overflowing content; `aspect-ratio` and `object-fit` control media box proportions.

**Examples:**

```css
.scroll-panel {
  overflow: auto;           /* scroll when needed */
  aspect-ratio: 16 / 9;
}

.cover-image {
  width: 100%;
  height: 200px;
  object-fit: cover;      /* fill box, crop */
  object-position: center;
}
```

**Sources:** [MDN — object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit)

</details>

<details>
  <summary>Logical properties</summary>

### Logical properties

**Definition:**

Logical properties (`margin-inline`, `padding-block`) map to physical sides based on writing direction for i18n-friendly CSS.

Write direction-aware styles (RTL, vertical writing):

**Examples:**

```css
.card {
  margin-inline: auto;      /* left+right in LTR */
  padding-block: 1rem;      /* top+bottom */
  border-inline-start: 2px solid blue;
}
```

Prefer logical properties in new code over physical `left`/`right` when supporting i18n.

**Sources:** [MDN — Logical properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_logical_properties_and_values)

</details>

<details>
  <summary>scrollbar-gutter</summary>

### scrollbar-gutter

**Definition:**

`scrollbar-gutter: stable` reserves scrollbar space to prevent layout shift when scrollbars appear.

Reserves space for scrollbar to prevent layout shift:

**Examples:**

```css
html {
  scrollbar-gutter: stable;
}
```

Values: `auto` | `stable` | `stable both-edges`

---

**Sources:** [MDN — scrollbar-gutter](https://developer.mozilla.org/en-US/docs/Web/CSS/scrollbar-gutter)

</details>

<a name="layout-systems"></a>
