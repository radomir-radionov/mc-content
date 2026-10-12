# CSS — Modern

2024+ CSS features and advanced phase-2 topics.

[← Back to hub](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Sections

- [Modern CSS (2024+)](#modern-css-2024)
- [CSS Phase 2 (advanced)](#css-phase-2-advanced)

---

## Modern CSS (2024+)

Features worth knowing for interviews and new projects.

<details>
  <summary>@starting-style</summary>

### @starting-style

**Definition:**

`@starting-style` defines entry styles for elements when they first appear (dialogs, popovers).

Entry animations for elements when they first appear (popover, dialog):

**How it works:**

- `@starting-style` defines the "before" state for elements entering the document or toggling `display` / `[open]` / popover states.
- Pairs with `transition` — browser interpolates from starting-style to normal rule on entry.
- Replaces JS `requestAnimationFrame` hacks for first-paint animations on `<dialog>`, `[popover]`, accordions.

**When to use:** Dialog/modal fade-in; popover scale entrance; list item expand. Combine with `@media (prefers-reduced-motion: reduce)`.

**Pluses and minuses:**

| + | − |
|------|------|
| Pure CSS enter animations | Requires `transition-behavior: allow-discrete` for `display` in some cases |
| Works with popover/dialog APIs | Browser support still rolling out — check caniuse |
| No flash of unstyled final state | Exit animations still need separate rules or View Transitions |

**Examples:**

```css
.dialog {
  transition: opacity 0.3s, transform 0.3s;
  opacity: 1;
  transform: scale(1);
}

@starting-style {
  .dialog[open] {
    opacity: 0;
    transform: scale(0.95);
  }
}
```

**Sources:** [MDN — @starting-style](https://developer.mozilla.org/en-US/docs/Web/CSS/@starting-style)

</details>

<details>
  <summary>CSS anchor positioning</summary>

### CSS anchor positioning

**Definition:**

Anchor positioning places popovers relative to an anchor element without JavaScript measurements.

Position popovers/tooltips relative to an anchor element without JS measurements:

**How it works:**

- Anchor element declares `anchor-name: --trigger`; positioned element references it via `position-anchor: --trigger`.
- `anchor()` function places edges relative to anchor: `top: anchor(bottom)`, `left: anchor(center)`.
- `position-area` / `inset-area` shorthand maps to logical sides (block-start, inline-end, etc.).
- `anchor-size()` can size popover to anchor width.

**When to use:** Tooltips, dropdown menus, popovers anchored to buttons. Provide JS fallback or `@supports (anchor-name: --x)` progressive enhancement.

**Pluses and minuses:**

| + | − |
|------|------|
| No JS `getBoundingClientRect` for tooltips | Chromium-first — verify Firefox/Safari support |
| Scroll/resize aware by default | Fallback positioning logic still needed for viewport edges |
| Works with `position: fixed` popovers | Naming collisions if `--anchor` names reused |

**Examples:**

```css
.anchor {
  anchor-name: --trigger;
}

.tooltip {
  position: absolute;
  position-anchor: --trigger;
  inset-area: block-end;
}
```

**Sources:** [MDN — Anchor positioning](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_anchor_positioning)

</details>

<details>
  <summary>Scroll-driven animations</summary>

### Scroll-driven animations

**Definition:**

Scroll-driven animations tie keyframe progress to scroll position instead of time.

Animate tied to scroll progress (no JS scroll listeners):

**How it works:**

- `animation-timeline` binds keyframe progress to scroll or view progress instead of time.
- `scroll()` — tied to scroll container; `view()` — tied to element visibility in scrollport.
- `animation-range` maps scroll/view intervals to keyframe `0%`–`100%` (`entry`, `exit`, `cover`, `contain`).
- Runs on compositor — no `scroll` event listeners for parallax/reveal effects.

**When to use:** Scroll-reveal sections; progress bars tied to scroll; header shrink on scroll. Fallback: `IntersectionObserver` + class toggle.

**Pluses and minuses:**

| + | − |
|------|------|
| No scroll JS — better performance | Limited browser support — progressive enhancement required |
| Smooth parallax and reveal | Harder to debug than time-based animations |
| Respects `prefers-reduced-motion` if gated | Complex ranges need experimentation |

**Examples:**

```css
@keyframes reveal {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.reveal {
  animation: reveal linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 100%;
}
```

**Sources:** [MDN — scroll-driven animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scroll-driven_animations)

</details>

<details>
  <summary>@scope</summary>

### @scope

**Definition:**

`@scope` limits selector reach to a subtree for encapsulation without Shadow DOM.

Limit selector reach to a subtree (native encapsulation without Shadow DOM):

**How it works:**

- `@scope (root)` limits selector matching to descendants of `root` — like a lightweight Shadow DOM boundary.
- `:scope` refers to the scoping root element itself.
- `@scope (.a) to (.b)` sets an optional upper boundary — selectors don't escape past `.b`.
- Specificity unchanged — scope only affects **reach**, not weight.

**When to use:** Card component styles without BEM prefixes; design-system widgets in CMS content. Prefer CSS Modules/Shadow DOM when hard isolation is required.

**Pluses and minuses:**

| + | − |
|------|------|
| Component isolation without build tools | No style encapsulation against global `!important` |
| Flat selectors inside scope (`img` not `.card img`) | Browser support still maturing |
| Pairs with cascade layers | Overlapping scopes need careful root choice |

**Examples:**

```css
@scope (.card) {
  :scope { padding: 1rem; }
  img { border-radius: 8px; } /* only images inside .card */
}
```

**Sources:** [MDN — @scope](https://developer.mozilla.org/en-US/docs/Web/CSS/@scope)

</details>

---

<a name="css-phase-2"></a>

## CSS Phase 2 (advanced)

Stub sections — expand as needed. MDN links for deep dives.

<details>
  <summary>Multi-column layout</summary>

### Multi-column layout

**Definition:**

CSS multi-column layout flows text across columns like magazine layouts.

CSS columns for magazine-style text flow:

**How it works:** [Using multi-column layouts](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_multicol_layout)

**Examples:**

```css
.article {
  column-count: 3;
  column-gap: 2rem;
  column-rule: 1px solid #e5e7eb;
}
.break { break-inside: avoid; }
```

**Sources:** [MDN — Multicol](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_multicol_layout)

</details>

<details>
  <summary>View Transitions API</summary>

### View Transitions API

**Definition:**

View transitions animate shared elements and page state changes in SPAs.

Animate DOM state changes (SPA route changes, shared element transitions):

**How it works:**

- JS triggers `document.startViewTransition(() => { /* DOM update */ })` — browser snapshots old/new states.
- `view-transition-name` on elements pairs old/new snapshots for shared-element morphing.
- `::view-transition-old()` / `::view-transition-new()` pseudo-elements style crossfade layers.
- Default crossfade applies; custom keyframes override per named transition.

[View Transitions](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)

**When to use:** SPA page transitions; card-to-detail hero morph; theme toggle crossfade. Respect `prefers-reduced-motion` — skip or shorten transitions.

**Pluses and minuses:**

| + | − |
|------|------|
| SPA route transitions without FLIP JS | Requires JS API call — not pure CSS alone |
| Shared-element hero animations | Single transition at a time — no parallel VTs |
| Works with MPA via `@view-transition` rule (emerging) | Large DOM snapshots can be slow on low-end devices |

**Examples:**

```css
.card { view-transition-name: hero; }
::view-transition-old(hero) { animation: fade-out 0.2s; }
::view-transition-new(hero) { animation: fade-in 0.2s; }
```

**Sources:** [MDN — View Transitions](https://developer.mozilla.org/en-US/docs/Web/API/View_Transitions_API)

</details>

<details>
  <summary>@property registered custom properties</summary>

### @property registered custom properties

**Definition:**

`@property` registers typed custom properties that can be animated.

Make CSS variables animatable and typed:

**How it works:**

- `@property --name` registers a custom property with `syntax`, `inherits`, and `initial-value`.
- Browser validates assignments — invalid values fall back to `initial-value` instead of invalidating the whole property.
- Registered properties with typed `syntax` can be **animated** in `@keyframes` (e.g. `--angle` from `0deg` to `360deg`).
- Unregistered `--vars` are treated as strings — no interpolation.

[@property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

**When to use:** Animated conic gradients (`--angle`); theme tokens with validation; color transitions on custom properties. Pair with `@supports` or progressive enhancement.

**Pluses and minuses:**

| + | − |
|------|------|
| Animatable gradients, colors, lengths via `--vars` | Verbose registration per property |
| Type safety for design tokens | `syntax` types must match assignments exactly |
| Enables Houdini-style extensibility | Fallback needed for browsers without `@property` |

**Examples:**

```css
@property --angle {
  syntax: '<angle>';
  inherits: false;
  initial-value: 0deg;
}
```

**Sources:** [MDN — @property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

</details>

<details>
  <summary>Relative color syntax</summary>

### Relative color syntax

**Definition:**

Relative color syntax derives new colors from existing values (`oklch(from var(--brand) ...)`).

Derive colors from existing values:

**How it works:** [Relative colors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_colors/Relative_colors)

**Examples:**

```css
.theme {
  --brand: oklch(58% 0.22 264);
  --brand-muted: oklch(from var(--brand) l c h / 50%);
}
```

**Sources:** [MDN — Relative colors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_colors/Relative_colors)

</details>

<details>
  <summary>Table and list styling</summary>

### Table and list styling

**Definition:**

Tables and lists have dedicated properties for borders, spacing, and markers.

`border-collapse`, `table-layout: fixed`, `list-style-type`, `list-style-position`. Use semantic `<table>` for tabular data; style with `thead`/`tbody` for zebra rows.

**Sources:** [MDN — table-layout](https://developer.mozilla.org/en-US/docs/Web/CSS/table-layout)

</details>

<details>
  <summary>Writing-mode and RTL</summary>

### Writing-mode and RTL

**Definition:**

`writing-mode` and `direction` control text flow; pair with logical properties for RTL support.

`writing-mode: vertical-rl`, `direction: rtl` — pair with logical properties (`margin-inline`, `padding-block`). See **Logical properties** in Box Model section.

**Sources:** [MDN — writing-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode)

</details>

---

<a name="architecture-tooling"></a>
