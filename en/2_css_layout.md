# CSS — Layout

Flexbox, grid, positioning, responsive design, and mobile viewport.

[← Back to hub](./2_css.md)

### Related docs

- [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Main concepts

| Section             | Topics                                                             |
| ------------------- | ------------------------------------------------------------------ |
| **Positioning**     | `position`, stacking contexts, legacy float, centering patterns    |
| **Flexbox**         | 1D main/cross axis, container & item properties, overflow pitfalls |
| **CSS Grid**        | 2D tracks, `fr` / `minmax`, areas, placement, `subgrid`            |
| **Responsive**      | Mobile-first breakpoints, container queries, cross-browser layout  |
| **Mobile viewport** | Units, safe areas, scroll, touch/pointer                           |

### Sections

- [Positioning & stacking](#positioning-stacking)
- [Flexbox](#flexbox)
- [CSS Grid](#css-grid)
- [Responsive Design & Container Queries](#responsive-design-container-queries)
- [Scroll, Touch & Mobile Viewport](#scroll-touch-mobile-viewport)

---

<a name="positioning-stacking"></a>

## Positioning & stacking

<details>
  <summary>Positioning</summary>

### Positioning

**Definition:**

`position` removes elements from normal flow (`absolute`, `fixed`) or offsets them (`relative`, `sticky`).

| Value      | Containing block                   | Scroll behavior             |
| ---------- | ---------------------------------- | --------------------------- |
| `static`   | Normal flow                        | Normal                      |
| `relative` | Self (offset from normal position) | Normal                      |
| `absolute` | Nearest positioned ancestor        | Normal                      |
| `fixed`    | Viewport                           | Fixed to viewport           |
| `sticky`   | Nearest scroll ancestor            | Sticks within parent bounds |

**How it works:**

ancestor must not have `overflow: hidden`; `top`/`bottom`/`left`/`right` must be set; parent must be tall enough to scroll.

`fixed` is always viewport-relative; `sticky` toggles between relative and fixed within its container's scroll range.

**Sources:** [MDN — position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

</details>

<details>
  <summary>Stacking context & z-index</summary>

### Stacking context & z-index

**Definition:**

A stacking context layers painted elements; `z-index` only compares within the same context.

`z-index` only works on **positioned** elements (`relative`, `absolute`, `fixed`, `sticky`) or flex/grid children.

**How it works:**

`background-color` → `background-image` → `border` → descendants → `outline`. Full stacking list below. Render pipeline → [1_html.md](./1_html.md).

```
1. Stacking contexts with negative z-index
2. In-flow, non-positioned blocks
3. Floated elements
4. In-flow inline elements
5. Positioned elements (z-index: auto / 0)
6. Positioned elements with positive z-index
7. Stacking contexts with z-index: auto that created a context (opacity, transform, etc.)
```

- `position` + `z-index` (not `auto`) on positioned/flex/grid item
- `opacity < 1`
- `transform`, `filter`, `perspective`, `clip-path`
- `will-change` for transform/opacity/filter
- `isolation: isolate`
- `mix-blend-mode` other than `normal`

move overlay/tooltip to a top-level portal, or ensure parent does not create an unintended stacking context.

**`isolation: isolate`** — force a new stacking context without side effects (useful for blend modes / z-index scoping).

```css
.overlay {
  mix-blend-mode: multiply;
}
.hero {
  background-blend-mode: overlay;
}
```

**When to use:**

#### Pitfall

```
┌─ .modal-backdrop (z-index: 100, creates context) ─┐
│  ┌─ .tooltip (z-index: 9999) ─┐  ← still BELOW     │
│  │  still inside parent layer  │     sibling modal  │
│  └─────────────────────────────┘                     │
└──────────────────────────────────────────────────────┘
```

**Examples:**

```css
.card {
  isolation: isolate;
}
.tooltip {
  z-index: 1;
} /* competes only within .card */
```

**Sources:** [MDN — Stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context)

</details>

<details>
  <summary>Float (legacy)</summary>

### Float (legacy)

**Definition:**

Floats pull elements out of flow for text wrapping; legacy for layout — prefer flex or grid.

Floats (`float: left/right`) pull elements out of flow; text wraps around them. Historically used for multi-column layouts.

**How it works:**

- Floated element is taken out of normal block flow; inline content wraps around it.
- Parent often collapses to zero height unless it creates a BFC (`flow-root`, `overflow`, flex/grid).
- Clearing (`clear`, clearfix, `display: flow-root` on parent) stops following content from wrapping.

collapsed parent height, clearing needed (`clearfix`, `overflow: hidden`, `display: flow-root`).

use Flexbox or Grid. Floats remain valid for text-wrapping around images.

**When to use:** Image with text wrap; `shape-outside` for non-rectangular wrap. **Not** for page columns, nav bars, or card grids — use flex/grid.

**Pluses and minuses:**

| +                                   | −                                          |
| ----------------------------------- | ------------------------------------------ |
| Natural text wrap around images     | Collapsed parent height without BFC        |
| Still valid for magazine-style wrap | Clearing hacks (`clearfix`) add complexity |
| Works without flex/grid support     | Multi-column layouts fragile vs Grid       |

**Examples:**

```css
.figure {
  float: left;
  margin: 0 1rem 1rem 0;
  shape-outside: margin-box;
}

.article::after {
  content: "";
  display: table;
  clear: both; /* clearfix */
}
```

**Sources:** [MDN — float](https://developer.mozilla.org/en-US/docs/Web/CSS/float)

</details>

<details>
  <summary>Centering cookbook</summary>

### Centering cookbook

**Definition:** Common patterns for horizontal, vertical, and full centering using margin, flex, grid, or absolute positioning.

**How it works:**

percentage height requires explicit height on every ancestor up to the containing block. Prefer `min-height: 100dvh` on `html, body` for full-viewport layouts.

```css
html,
body {
  min-height: 100%;
}
/* or */
html {
  height: 100%;
}
body {
  min-height: 100%;
}
```

---

**Examples:**

```css
/* Horizontal — block element */
.center-h {
  width: fit-content;
  margin-inline: auto;
}

/* Vertical + horizontal — Flexbox */
.center-flex {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100dvh;
}

/* Vertical + horizontal — Grid */
.center-grid {
  display: grid;
  place-items: center;
  min-height: 100dvh;
}

/* Absolute center (known size) */
.center-abs {
  position: absolute;
  inset: 50%;
  transform: translate(-50%, -50%);
}
```

**Sources:** [CSS for JS](https://learn.javascript.ru/css-for-js)

</details>

<a name="flexbox"></a>

## Flexbox

<details>
  <summary>Flexbox</summary>

### Flexbox

**Definition:** **Flexbox** is a **one-dimensional** layout: distribute free space and align items along a **main axis** (`flex-direction`) and **cross axis** (perpendicular).

**How it works:**

- Container: `display: flex` or `inline-flex` creates a **flex formatting context**.
- **Main axis** follows `flex-direction` (`row` default in LTR); **cross axis** is perpendicular.
- Items default to `flex: 0 1 auto` — can shrink but not grow; `flex: 1` often means `1 1 0%` (equal columns).
- **`justify-content`** aligns on the **main** axis; **`align-items`** on the **cross** axis (single line).
- **`flex-wrap: wrap`** → multiple lines; then **`align-content`** distributes extra cross-axis space between lines.
- **`gap`** sets gutters between items (preferred over margin hacks).
- **`justify-self`** does **not** apply to flex items — use **`align-self`**, **`margin: auto`**, or wrapper.

**When to use:** Nav bars, toolbars, card rows, media objects, sidebar + main, 1D centering ([Centering cookbook](#centering-cookbook)).

**Thoughts:** Full property lists → [Flex container properties](#flex-container-properties) · [Flex item sizing](#flex-item-sizing-and-overflow).

**Sources:** [MDN — Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout)

</details>

<details>
  <summary>Flex container properties</summary>

### Flex container properties

**Definition:** Properties on the **flex container** control direction, wrapping, distribution, and default item alignment.

**How it works:**

| Property                         | Values (common)                               | Effect                      |
| -------------------------------- | --------------------------------------------- | --------------------------- |
| `flex-direction`                 | `row` · `column` · `*-reverse`                | Main axis direction         |
| `flex-wrap`                      | `nowrap` · `wrap`                             | Single vs multi-line        |
| `flex-flow`                      | shorthand of direction + wrap                 |                             |
| `justify-content`                | `flex-start` · `center` · `space-between` · … | Main-axis distribution      |
| `align-items`                    | `stretch` · `center` · `baseline` · …         | Cross-axis per line         |
| `align-content`                  | same keywords as justify                      | Cross-axis when **wrapped** |
| `gap` / `row-gap` / `column-gap` | lengths                                       | Space between items         |

- In **`flex-direction: column`**, **`justify-content` moves vertically** and **`align-items` horizontally** — common interview trap.
- **`align-items: baseline`** lines up text baselines; mixed font sizes need consistent line-height.
- **`align-content`** has **no effect** on a single line (`flex-wrap: nowrap`).

**When to use:** Interview “how do you space items in a row?” — name axis first, then pick justify vs align.

**Examples:**

```css
.toolbar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
```

**Sources:** [MDN — justify-content](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content)

</details>

<details>
  <summary>Flex item sizing and overflow</summary>

### Flex item sizing and overflow

**Definition:** **Flex items** size with **`flex-grow`**, **`flex-shrink`**, **`flex-basis`**, and browser defaults that often cause **overflow**.

**How it works:**

| Shorthand         | Computed       | Typical use                       |
| ----------------- | -------------- | --------------------------------- |
| `flex: 1`         | `1 1 0%`       | Equal-width columns sharing space |
| `flex: auto`      | `1 1 auto`     | Grow/shrink from content width    |
| `flex: none`      | `0 0 auto`     | Fixed-size chip / icon            |
| `flex: 0 0 240px` | explicit basis | Sidebar fixed width               |

- Default **`min-width: auto`** (min-content) prevents shrinking below longest word — text overflows. Fix: **`min-width: 0`** on flex child (column layouts: **`min-height: 0`**).
- **`flex-basis`** sets starting size before free space is distributed; `% basis needs definite container width.
- **`align-self`** overrides container `align-items` for one item.
- **`order`** changes **visual** order only — **tab order** follows DOM; bad for accessibility if misused.

**When to use:** Truncation + ellipsis in flex rows; holy-grail sidebar; pushing one item with `margin-inline-start: auto`.

**Examples:**

```css
.sidebar {
  flex: 0 0 240px;
}
.main {
  flex: 1 1 auto;
  min-width: 0;
}

.nav {
  display: flex;
}
.nav .logo {
  margin-inline-end: auto;
}

.row {
  display: flex;
  gap: 1rem;
}
.row > .truncate {
  flex: 1 1 0;
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

#### Common pitfalls

Nested flex without `min-width: 0`. Using `order` for “importance” instead of DOM order. Expecting `justify-self` on flex items.

**Sources:** [MDN — flex](https://developer.mozilla.org/en-US/docs/Web/CSS/flex)

</details>

<a name="css-grid"></a>

## CSS Grid

<details>
  <summary>CSS Grid</summary>

### CSS Grid

**Definition:** **CSS Grid** is **two-dimensional**: explicit **rows and columns** on one container, with optional **named areas** and **line-based** placement.

**How it works:**

- `display: grid` / `inline-grid` on the container.
- **Explicit grid:** `grid-template-columns`, `grid-template-rows`, `grid-template-areas`.
- **Implicit grid:** extra items flow into auto tracks (`grid-auto-rows`, `grid-auto-columns`, `grid-auto-flow`).
- **`fr`** distributes **leftover** space; pair with **`minmax()`** so tracks can shrink (`minmax(0, 1fr)`).
- **Gap** separates tracks; **`place-items`** shorthand for cell alignment inside tracks.
- **Grid item** properties: `grid-column`, `grid-row`, `grid-area`, `justify-self`, `align-self`.

**When to use:** Page shells, dashboards, responsive card matrices, aligning form labels + fields in two columns.

**Thoughts:** Tracks & `fr` → [Grid tracks and minmax](#grid-tracks-and-minmax) · placement → [Grid placement and template areas](#grid-placement-and-template-areas).

**Sources:** [MDN — Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout)

</details>

<details>
  <summary>Grid tracks and minmax</summary>

### Grid tracks and minmax

**Definition:** **Grid tracks** are row/column sizing lines; **`repeat()`**, **`fr`**, and **`minmax()`** build responsive layouts without extra media queries.

**How it works:**

```css
grid-template-columns: repeat(3, 1fr);
grid-template-columns: 240px 1fr;
grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
```

| Pattern                   | Behavior                                                               |
| ------------------------- | ---------------------------------------------------------------------- |
| `repeat(3, 1fr)`          | Three equal columns                                                    |
| `minmax(200px, 1fr)`      | At least 200px, share extra space                                      |
| `minmax(0, 1fr)`          | Allow track to shrink below content (fix overflow)                     |
| `auto-fill` vs `auto-fit` | **fill** keeps empty tracks; **fit** collapses them so columns stretch |

- **`grid-auto-rows: minmax(120px, auto)`** sizes implicit rows.
- **`grid-auto-flow: dense`** fills holes when items span different sizes (mind reading order vs visual order).
- **`subgrid`** (where supported): child grid inherits parent tracks — align card sections across a row.

**When to use:** Responsive product grids; fixed sidebar + fluid main (`240px 1fr`).

**Examples:**

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}
```

**Sources:** [MDN — repeat()](https://developer.mozilla.org/en-US/docs/Web/CSS/repeat) · [MDN — minmax()](https://developer.mozilla.org/en-US/docs/Web/CSS/minmax)

</details>

<details>
  <summary>Grid placement and template areas</summary>

### Grid placement and template areas

**Definition:** Items are placed by **line numbers**, **`span`**, or **named areas** on a rectangular template.

**How it works:**

```css
.dashboard {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 240px 1fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100dvh;
}
.header {
  grid-area: header;
}
```

- **Areas** must form **rectangles** — no L-shapes; use `.` for empty cells.
- **Line-based:** `grid-column: 1 / 3` or `span 2` for ad-hoc spans.
- **`justify-items` / `align-items`** align content **inside** each cell; **`justify-content` / `align-content`** move the **whole grid** when smaller than container.
- **`place-content: center`** centers the grid in the viewport (with fixed track sizes).

**When to use:** App chrome (header/sidebar/main/footer); overlapping layers via same cell + `z-index` (sparingly).

**Examples:**

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 3;
}
```

#### Common pitfalls

L-shaped areas (invalid). `1fr` columns that refuse to shrink. Dense auto-flow vs DOM order for screen readers.

**Sources:** [MDN — grid-template-areas](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-template-areas)

</details>

<details>
  <summary>Flex vs Grid — when to use which</summary>

### Flex vs Grid — when to use which

**Definition:**

Flexbox excels at 1D component rows; Grid excels at 2D page shells and card matrices.

|                | Flexbox                  | Grid                       |
| -------------- | ------------------------ | -------------------------- |
| Dimensions     | 1D                       | 2D                         |
| Content-driven | Yes — items dictate flow | Yes — or template-driven   |
| Gaps           | `gap`                    | `gap`                      |
| Best for       | Nav, rows, alignment     | Page layout, card matrices |

Use both together: Grid for page shell, Flexbox inside cells.

**Sources:** [web.dev — Flexbox vs Grid](https://web.dev/learn/css/grid/#grid-or-flexbox)

</details>

<a name="responsive-design"></a>

## Responsive Design & Container Queries

<details>
  <summary>Responsive vs adaptive</summary>

### Responsive vs adaptive

**Definition:**

Responsive layouts scale fluidly; adaptive layouts switch at discrete breakpoints.

| Approach       | Behavior                                                                 |
| -------------- | ------------------------------------------------------------------------ |
| **Responsive** | Fluid layouts — one design scales smoothly (%, `fr`, `clamp`, flex/grid) |
| **Adaptive**   | Discrete breakpoints — different layouts at fixed widths                 |

Modern practice: **mobile-first responsive** with breakpoints as enhancement.

**Sources:** [web.dev — Responsive](https://web.dev/learn/design/responsive)

</details>

<details>
  <summary>Cross-browser responsive layout</summary>

### Cross-browser responsive layout

**Definition:** **Adaptive/responsive** layouts must render consistently across engines — via standards-first CSS, **feature queries**, and build-time **prefixing**, not user-agent sniffing.

**How it works:**

- **Mobile-first:** base styles for narrow viewports; **`min-width`** media queries add columns and spacing ([Media queries](#media-queries)).
- **Flex/Grid:** prefer layout modes with broad support; use **`@supports (display: grid)`** fallback when needed.
- **Viewport:** `<meta name="viewport" content="width=device-width, initial-scale=1">` — required for mobile scaling; pair with **`100dvh` / safe-area** → [Safe areas & viewport units](#safe-areas-viewport-units).
- **Cross-browser CSS:** **normalize/reset** → [CSS Core](./2_css_core.md); **Autoprefixer + Browserslist** → [CSS — Architecture](./2_css_architecture.md) (PostCSS & Autoprefixer); check gaps on [caniuse.com](https://caniuse.com/) (linked from [CSS hub](./2_css.md)).
- **Feature detection:** `@supports` for `container-type`, `subgrid`, `backdrop-filter` — degrade gracefully.

**When to use:** JD “кросс-браузерная вёрстка”; fixing Safari vs Chrome flex/grid quirks; production build pipeline questions.

**See also:** [Container queries](#container-queries) · [Touch & pointer](#touch-pointer)

**Sources:** [MDN — Using media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries)

</details>

<details>
  <summary>Media queries</summary>

### Media queries

**Definition:** Media queries apply CSS based on viewport features — width, motion preference, color scheme, pointer type.

**How it works:**

640px · 768px · 1024px · 1280px — tune to your content.

```css
@supports (display: grid) {
  .layout {
    display: grid;
  }
}

@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
  }
}

@media (prefers-color-scheme: dark) {
  :root {
    color-scheme: dark;
  }
}
```

| Feature                        | Values            | Use case                     |
| ------------------------------ | ----------------- | ---------------------------- |
| `prefers-reduced-motion`       | `reduce`          | Disable/limit animations     |
| `prefers-color-scheme`         | `light` \| `dark` | Theme switching              |
| `prefers-contrast`             | `more` \| `less`  | High-contrast UI             |
| `prefers-reduced-transparency` | `reduce`          | Solid backgrounds over glass |

| Feature             | Values               | Use case                                  |
| ------------------- | -------------------- | ----------------------------------------- |
| `(hover: hover)`    | hover-capable device | Show hover-only UI on desktop             |
| `(hover: none)`     | touch-primary        | Larger tap targets, no hover-dependent UX |
| `(pointer: fine)`   | mouse/stylus         | Dense UI                                  |
| `(pointer: coarse)` | finger               | Bigger buttons, more spacing              |

```css
@media (hover: hover) and (pointer: fine) {
  .card:hover {
    transform: translateY(-2px);
  }
}

@media (hover: none) {
  .card-actions {
    opacity: 1;
  } /* always visible on touch */
}
```

**`@supports`** — feature detection for progressive enhancement (not browser sniffing):

```css
@supports (backdrop-filter: blur(1px)) {
  .glass {
    backdrop-filter: blur(12px);
  }
}
```

**Examples:**

```css
/* Mobile-first: base styles, then enhance */
.card-grid {
  display: grid;
  gap: 1rem;
  grid-template-columns: 1fr;
}

@media (min-width: 640px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

**Sources:** [MDN — Media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries)

</details>

<details>
  <summary>Container queries</summary>

### Container queries

**Definition:**

Container queries style elements based on their parent container's size, not the viewport.

Style based on **parent container size**, not viewport — essential for reusable components in sidebars, split views, etc.

**How it works:**

| Value         | Queries on               |
| ------------- | ------------------------ |
| `inline-size` | Width only (most common) |
| `size`        | Width and height         |
| `normal`      | Not a query container    |

**Container query units** (relative to the query container, not viewport):

| Unit              | Meaning                       |
| ----------------- | ----------------------------- |
| `cqw`             | 1% of container width         |
| `cqh`             | 1% of container height        |
| `cqi` / `cqb`     | Inline/block axis %           |
| `cqmin` / `cqmax` | Smaller/larger of cqi and cqb |

```css
.card-container {
  container-type: inline-size;
}

.card-title {
  font-size: clamp(1rem, 4cqi + 0.5rem, 1.5rem);
}
```

use `@container` when a **component** must adapt to its parent (sidebar widget, dashboard panel); use `@media` when layout depends on **viewport** (global nav, page grid).

**When to use:** component adapts to its slot; media queries when layout depends on viewport (nav, page grid).

**Examples:**

```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 120px 1fr;
  }
}

/* Shorthand */
@container (min-width: 30rem) {
  .widget {
    padding: 2rem;
  }
}
```

**Sources:** [MDN — Container queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)

</details>

<details>
  <summary>Units</summary>

### Units

**Definition:**

CSS length units (`px`, `rem`, `%`, `vw`, `fr`, `dvh`) resolve relative to different reference boxes.

| Unit        | Meaning                       | Pitfalls                                                |
| ----------- | ----------------------------- | ------------------------------------------------------- |
| `px`        | Absolute pixels               | Fine for borders; avoid for root font sizing            |
| `%`         | Relative to parent            | Height % needs parent height                            |
| `em`        | Relative to element font-size | Compounds when nested                                   |
| `rem`       | Relative to root font-size    | Preferred for spacing/type scale                        |
| `ch`        | Character width of "0"        | Max line length (`max-width: 65ch`)                     |
| `vw` / `vh` | % of viewport                 | Mobile `100vh` includes URL bar — use `dvh`/`svh`/`lvh` |
| `vmin` / `vmax` | Smaller / larger of `vw` and `vh` | MDN treats `vmin` as `lvmin` and `vmax` as `lvmax` (large viewport). `svmin` / `dvmin` follow the small and dynamic viewports |
| `fr`        | Grid free space fraction      | Grid only                                               |
| `dvh`       | Dynamic viewport height       | Adapts when mobile browser chrome shows/hides           |

**How it works:**

prefer `rem` for consistency; `em` when sizing relative to parent component (buttons inside a card).

```css
.sidebar {
  width: calc(100% - 240px);
  padding: max(1rem, 2vw);
  font-size: clamp(0.875rem, 0.8rem + 0.25vw, 1.125rem);
}
```

`calc()` — mix units (`%`, `px`, `rem`, `vw`). `min()` / `max()` — pick smallest/largest. `clamp(min, preferred, max)` — fluid values without media queries.

---

**Examples:**

```css
h1 {
  font-size: clamp(1.5rem, 2vw + 1rem, 3rem);
}

.section {
  padding: clamp(1rem, 5vw, 4rem);
}
```

**Sources:** [MDN — CSS values](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units)

</details>

<details>
  <summary>CSS Custom Properties</summary>

### CSS Custom Properties

**Definition:** Custom properties (`--name`) are live CSS variables that cascade and inherit, enabling runtime theming.

**How it works:**

Variables defined with `--name`, used with `var()`. Cascade and inherit like normal properties.

`var(--missing, fallback-value)`

set on `:root` for global; on a component class for local theming.

custom properties are live in the browser — runtime theming, media queries, and JS updates (`element.style.setProperty('--x', val)`).

**Examples:**

```css
:root {
  --color-primary: #2563eb;
  --space-md: 1rem;
  --radius: 8px;
}

.button {
  background: var(--color-primary);
  padding: var(--space-md);
  border-radius: var(--radius);
}

.button--danger {
  --color-primary: #dc2626; /* override in scope */
}

.theme-dark {
  --color-primary: #60a5fa;
}
```

**Sources:** [MDN — Custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)

</details>

<details>
  <summary>Native CSS nesting</summary>

### Native CSS nesting

**Definition:**

Native nesting lets you write descendant and pseudo selectors inside a rule block without a preprocessor.

Supported in modern browsers (2023+). No preprocessor required:

**Examples:**

```css
.card {
  padding: 1rem;

  & .title {
    font-weight: 700;
  }

  &:hover {
    box-shadow: 0 4px 12px rgb(0 0 0 / 10%);
  }
}
```

---

**Sources:** [MDN — Nesting](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)

</details>

<a name="scroll-touch-mobile"></a>

## Scroll, Touch & Mobile Viewport

<details>
  <summary>Scroll behavior</summary>

### Scroll behavior

**Definition:** Scroll-related properties control smooth scrolling, scroll chaining, and anchor offset for fixed headers.

**How it works:** `-webkit-overflow-scrolling: touch` (momentum scroll on old iOS) — no longer needed on modern Safari.

**Examples:**

```css
html {
  scroll-behavior: smooth; /* anchor links */
}

.modal-body {
  overflow-y: auto;
  overscroll-behavior: contain; /* no scroll chaining to body */
}
```

| Property              | Values                        | Use case                                  |
| --------------------- | ----------------------------- | ----------------------------------------- |
| `scroll-behavior`     | `auto` \| `smooth`            | Smooth anchor navigation                  |
| `overscroll-behavior` | `auto` \| `contain` \| `none` | Prevent pull-to-refresh / bounce chaining |
| `scroll-padding-top`  | length                        | Offset for fixed headers on anchor scroll |

**Sources:** [MDN — overscroll-behavior](https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior)

</details>

<details>
  <summary>Touch & pointer</summary>

### Touch & pointer

**Definition:** `touch-action` and pointer media queries adapt interaction for touch vs mouse devices.

**Examples:**

```css
.carousel {
  touch-action: pan-x; /* horizontal swipe only */
}

.button {
  touch-action: manipulation; /* disable double-tap zoom delay */
}
```

| `touch-action`    | Effect                                 |
| ----------------- | -------------------------------------- |
| `auto`            | Browser handles all gestures           |
| `none`            | Disable browser gestures (custom drag) |
| `pan-x` / `pan-y` | Allow scroll on one axis               |
| `manipulation`    | Pan + pinch-zoom; no double-tap delay  |

Pair with media queries: `@media (pointer: coarse)` for larger hit areas on touch devices.

**Sources:** [MDN — touch-action](https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action)

</details>

<details>
  <summary>Safe areas & viewport units</summary>

### Safe areas & viewport units

**Definition:**

Safe-area insets and dynamic viewport units (`dvh`) handle notched devices and mobile browser chrome.

Notched devices and mobile browser chrome need extra padding:

**Examples:**

```css
.header {
  padding-top: env(safe-area-inset-top);
  padding-inline: env(safe-area-inset-left) env(safe-area-inset-right);
}

.fullscreen {
  min-height: 100dvh; /* dynamic — adapts when URL bar hides */
  padding-bottom: env(safe-area-inset-bottom);
}
```

| Viewport unit | Behavior                                         |
| ------------- | ------------------------------------------------ |
| `vh` / `vw`   | Static viewport — `100vh` can overflow on mobile |
| `dvh` / `dvw` | Dynamic — changes as browser UI shows/hides      |
| `svh`         | Small viewport (UI visible)                      |
| `lvh`         | Large viewport (UI hidden)                       |

Enable safe-area: `<meta name="viewport" content="width=device-width, viewport-fit=cover">`

**Sources:** [MDN — viewport units](https://developer.mozilla.org/en-US/docs/Web/CSS/length#viewport-percentage_lengths)

</details>

---

<a name="typography-colors"></a>
