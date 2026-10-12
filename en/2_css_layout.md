# CSS — Layout

Flexbox, grid, positioning, responsive design, and mobile viewport.

[← Back to hub](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Sections

- [Layout Systems](#layout-systems)
- [Responsive Design & Container Queries](#responsive-design-container-queries)
- [Scroll, Touch & Mobile Viewport](#scroll-touch-mobile-viewport)

---

## Layout Systems

<details>
  <summary>Positioning</summary>

### Positioning

**Definition:**

`position` removes elements from normal flow (`absolute`, `fixed`) or offsets them (`relative`, `sticky`).

| Value | Containing block | Scroll behavior |
|-------|------------------|-----------------|
| `static` | Normal flow | Normal |
| `relative` | Self (offset from normal position) | Normal |
| `absolute` | Nearest positioned ancestor | Normal |
| `fixed` | Viewport | Fixed to viewport |
| `sticky` | Nearest scroll ancestor | Sticks within parent bounds |

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

* `position` + `z-index` (not `auto`) on positioned/flex/grid item
* `opacity < 1`
* `transform`, `filter`, `perspective`, `clip-path`
* `will-change` for transform/opacity/filter
* `isolation: isolate`
* `mix-blend-mode` other than `normal`

move overlay/tooltip to a top-level portal, or ensure parent does not create an unintended stacking context.

**`isolation: isolate`** — force a new stacking context without side effects (useful for blend modes / z-index scoping).

```css
.overlay { mix-blend-mode: multiply; }
.hero    { background-blend-mode: overlay; }
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
.card { isolation: isolate; }
.tooltip { z-index: 1; } /* competes only within .card */
```

**Sources:** [MDN — Stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context)

</details>

<details>
  <summary>Flexbox</summary>

### Flexbox

**Definition:** One-dimensional layout: distribute space along a **main axis** (`row` or `column`).

**How it works:**

`display: flex` / `inline-flex`. Main axis = `flex-direction`; cross axis is perpendicular. Items size with `flex` (`grow shrink basis`). Default `min-width: auto` (min-content) — set `min-width: 0` (column: `min-height: 0`) so items can shrink. `justify-self` does **not** apply to flex items — use `align-self` or `margin: auto`.

| Container | Role |
|-----------|------|
| `flex-direction` | `row` \| `row-reverse` \| `column` \| `column-reverse` |
| `flex-wrap` | `nowrap` \| `wrap` \| `wrap-reverse` |
| `justify-content` | Main axis: `flex-start` \| `center` \| `flex-end` \| `space-between` \| `space-around` \| `space-evenly` |
| `align-items` | Cross axis, one line: `stretch` \| `flex-start` \| `center` \| `flex-end` \| `baseline` |
| `align-content` | Extra cross-axis space when wrapped; ignored on a single line |
| `gap` | Space between items |

| Item | Role |
|------|------|
| `flex: 1` | `1 1 0%` — share leftover space evenly |
| `flex: auto` | `1 1 auto` — grow/shrink from content size |
| `flex: none` | `0 0 auto` — fixed; no grow/shrink |
| `flex: 0 0 240px` | Fixed sidebar |
| `align-self` | Override `align-items` |
| `order` | Visual reorder only — **not** tab order |

**When to use:**

Toolbars, nav, card rows, centering, sidebar + main.

#### Common pitfalls

Overflow without `min-width: 0`. `order` vs a11y. Column flex: `justify-content` is **vertical**.

**Pluses and minuses:**

| + | − |
| --- | --- |
| Content-driven 1D alignment | Nested 2D layouts fight Flex — use Grid |
| `gap` + `margin: auto` for leftover space | `min-width: auto` overflows text unless reset |

**Examples:**

```css
.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.sidebar { flex: 0 0 240px; }
.main { flex: 1 1 auto; min-width: 0; }

.nav { display: flex; }
.nav .logo { margin-inline-end: auto; } /* leftover space → links right */
```

**Thoughts:** One axis at a time — Flex for rows/columns; Grid when you need both.

**Sources:** [MDN — Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout)

</details>

<details>
  <summary>CSS Grid</summary>

### CSS Grid

**Definition:** Two-dimensional layout: rows **and** columns on one container.

**How it works:**

`display: grid` / `inline-grid`. Explicit tracks = `grid-template-columns` / `rows` / `areas`. Overflowing items create an **implicit** grid (`grid-auto-rows` / `columns` / `flow`). `fr` = share of free space; `minmax(min, max)` bounds a track — use `minmax(0, 1fr)` so tracks can shrink below content. Items: `grid-column` / `grid-row` / `grid-area`, `justify-self` / `align-self`. Align cells with `justify-items` / `align-items`; align the whole grid with `justify-content` / `align-content`. `gap` for gutters.

`repeat(auto-fit|auto-fill, minmax(280px, 1fr))`: **auto-fill** keeps empty tracks; **auto-fit** collapses them so remaining columns stretch. Areas: same cell count per row, rectangles only (no L-shapes), `.` for empty cells.

**When to use:**

Page shells, dashboards, card matrices. Nested `subgrid` inherits parent tracks (align card title/body/footer across a row).

#### Common pitfalls

`1fr` that won’t shrink (use `minmax(0, 1fr)`). L-shaped `grid-template-areas`. Mixing Flex 2D nesting instead of Grid.

**Pluses and minuses:**

| + | − |
| --- | --- |
| 2D template + named areas | Overkill for a single toolbar row |
| `auto-fit` / `fr` without media queries | Implicit tracks surprise if `auto-flow` unset |

**Examples:**

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}

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
.header { grid-area: header; }

.grid {
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(120px, auto);
  grid-auto-flow: dense;
}

.card-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; }
.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 3;
}
```

**Thoughts:** Grid owns the 2D shell; Flex owns 1D alignment inside cells.

**Sources:** [MDN — Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout)

</details>

<details>
  <summary>Flex vs Grid — when to use which</summary>

### Flex vs Grid — when to use which

**Definition:**

Flexbox excels at 1D component rows; Grid excels at 2D page shells and card matrices.

| | Flexbox | Grid |
|---|---------|------|
| Dimensions | 1D | 2D |
| Content-driven | Yes — items dictate flow | Yes — or template-driven |
| Gaps | `gap` | `gap` |
| Best for | Nav, rows, alignment | Page layout, card matrices |

Use both together: Grid for page shell, Flexbox inside cells.

**Sources:** [web.dev — Flexbox vs Grid](https://web.dev/learn/css/grid/#grid-or-flexbox)

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

| + | − |
|------|------|
| Natural text wrap around images | Collapsed parent height without BFC |
| Still valid for magazine-style wrap | Clearing hacks (`clearfix`) add complexity |
| Works without flex/grid support | Multi-column layouts fragile vs Grid |

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
html, body {
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

<a name="responsive-design"></a>

## Responsive Design & Container Queries

<details>
  <summary>Responsive vs adaptive</summary>

### Responsive vs adaptive

**Definition:**

Responsive layouts scale fluidly; adaptive layouts switch at discrete breakpoints.

| Approach | Behavior |
|----------|----------|
| **Responsive** | Fluid layouts — one design scales smoothly (%, `fr`, `clamp`, flex/grid) |
| **Adaptive** | Discrete breakpoints — different layouts at fixed widths |

Modern practice: **mobile-first responsive** with breakpoints as enhancement.

**Sources:** [web.dev — Responsive](https://web.dev/learn/design/responsive)

</details>

<details>
  <summary>Media queries</summary>

### Media queries

**Definition:** Media queries apply CSS based on viewport features — width, motion preference, color scheme, pointer type.

**How it works:**

640px · 768px · 1024px · 1280px — tune to your content.

```css
@supports (display: grid) {
  .layout { display: grid; }
}

@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; }
}

@media (prefers-color-scheme: dark) {
  :root { color-scheme: dark; }
}
```

| Feature | Values | Use case |
|---------|--------|----------|
| `prefers-reduced-motion` | `reduce` | Disable/limit animations |
| `prefers-color-scheme` | `light` \| `dark` | Theme switching |
| `prefers-contrast` | `more` \| `less` | High-contrast UI |
| `prefers-reduced-transparency` | `reduce` | Solid backgrounds over glass |

| Feature | Values | Use case |
|---------|--------|----------|
| `(hover: hover)` | hover-capable device | Show hover-only UI on desktop |
| `(hover: none)` | touch-primary | Larger tap targets, no hover-dependent UX |
| `(pointer: fine)` | mouse/stylus | Dense UI |
| `(pointer: coarse)` | finger | Bigger buttons, more spacing |

```css
@media (hover: hover) and (pointer: fine) {
  .card:hover { transform: translateY(-2px); }
}

@media (hover: none) {
  .card-actions { opacity: 1; } /* always visible on touch */
}
```

**`@supports`** — feature detection for progressive enhancement (not browser sniffing):

```css
@supports (backdrop-filter: blur(1px)) {
  .glass { backdrop-filter: blur(12px); }
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
  .card-grid { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1024px) {
  .card-grid { grid-template-columns: repeat(3, 1fr); }
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

| Value | Queries on |
|-------|------------|
| `inline-size` | Width only (most common) |
| `size` | Width and height |
| `normal` | Not a query container |

**Container query units** (relative to the query container, not viewport):

| Unit | Meaning |
|------|---------|
| `cqw` | 1% of container width |
| `cqh` | 1% of container height |
| `cqi` / `cqb` | Inline/block axis % |
| `cqmin` / `cqmax` | Smaller/larger of cqi and cqb |

```css
.card-container { container-type: inline-size; }

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
  .widget { padding: 2rem; }
}
```

**Sources:** [MDN — Container queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)

</details>

<details>
  <summary>Units</summary>

### Units

**Definition:**

CSS length units (`px`, `rem`, `%`, `vw`, `fr`, `dvh`) resolve relative to different reference boxes.

| Unit | Meaning | Pitfalls |
|------|---------|----------|
| `px` | Absolute pixels | Fine for borders; avoid for root font sizing |
| `%` | Relative to parent | Height % needs parent height |
| `em` | Relative to element font-size | Compounds when nested |
| `rem` | Relative to root font-size | Preferred for spacing/type scale |
| `ch` | Character width of "0" | Max line length (`max-width: 65ch`) |
| `vw` / `vh` | % of viewport | Mobile `100vh` includes URL bar — use `dvh`/`svh`/`lvh` |
| `fr` | Grid free space fraction | Grid only |
| `dvh` | Dynamic viewport height | Adapts when mobile browser chrome shows/hides |

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

| Property | Values | Use case |
|----------|--------|----------|
| `scroll-behavior` | `auto` \| `smooth` | Smooth anchor navigation |
| `overscroll-behavior` | `auto` \| `contain` \| `none` | Prevent pull-to-refresh / bounce chaining |
| `scroll-padding-top` | length | Offset for fixed headers on anchor scroll |

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

| `touch-action` | Effect |
|----------------|--------|
| `auto` | Browser handles all gestures |
| `none` | Disable browser gestures (custom drag) |
| `pan-x` / `pan-y` | Allow scroll on one axis |
| `manipulation` | Pan + pinch-zoom; no double-tap delay |

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

| Viewport unit | Behavior |
|---------------|----------|
| `vh` / `vw` | Static viewport — `100vh` can overflow on mobile |
| `dvh` / `dvw` | Dynamic — changes as browser UI shows/hides |
| `svh` | Small viewport (UI visible) |
| `lvh` | Large viewport (UI hidden) |

Enable safe-area: `<meta name="viewport" content="width=device-width, viewport-fit=cover">`

**Sources:** [MDN — viewport units](https://developer.mozilla.org/en-US/docs/Web/CSS/length#viewport-percentage_lengths)

</details>

---

<a name="typography-colors"></a>
