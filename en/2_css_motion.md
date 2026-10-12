# CSS — Motion

Transforms, transitions, animations, and visual effects.

[← Back to hub](./2_css.md)

### Related docs

* [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Sections

- [Transforms, Transitions & Animations](#transforms-transitions-animations)
- [Visual Effects](#visual-effects)

---

## Transforms, Transitions & Animations

<details>
  <summary>transform</summary>

### transform

**Definition:** `transform` applies 2D/3D transforms without triggering layout — ideal for GPU-composited animation.

**How it works:**

- `transform` applies a 2D/3D matrix to the element without changing layout flow (element still occupies original space unless combined with positioning).
- Functions compose right-to-left: `translateX(10px) rotate(45deg)` — rotate then translate in transformed space.
- `transform-origin` sets pivot point; `perspective` on parent enables 3D depth.
- Promotes element to compositor layer — repaints skip layout.

`transform` and `opacity` promote to compositor layer — smooth 60fps animations.

`translate()` does not trigger layout; `top`/`left` changes do. Always animate `transform`/`opacity` when possible.

**When to use:** Hover lift (`translateY`), spinners (`rotate`), scale on press, parallax (sparingly). Animate `transform` + `opacity` — never `top`/`left`/`width` for motion (those cause reflow and can raise [CLS](./1_html_performance.md#cls-cumulative-layout-shift)).

**Pluses and minuses:**

| + | − |
|------|------|
| GPU-composited — smooth 60fps | Does not affect document flow (ghost space remains) |
| No layout/reflow on change | 3D transforms can cause blurry text on subpixel layers |
| Composable functions | `transform` on parent creates containing block for `fixed` children |

**Examples:**

```css
.card:hover {
  transform: translateY(-4px) scale(1.02);
}

.spinner {
  transform: rotate(0deg);
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

**Sources:** [MDN — transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

</details>

<details>
  <summary>transition</summary>

### transition

**Definition:** `transition` animates property changes over time with duration, easing, and delay.

**How it works:**

- Browser interpolates from computed start value to end value when a property changes (class toggle, `:hover`, JS `style` change).
- Shorthand: `transition: property duration timing-function delay` — comma-separate for multiple properties.
- Only **animatable** properties transition; `display` generally does not (use `@starting-style` or `allow-discrete` where supported).
- `transition-behavior: allow-discrete` enables discrete transitions for `display`, `visibility`, etc. (modern browsers).

`transition: all` — lists every property; can animate expensive properties accidentally.

**When to use:** Hover color/shadow changes; drawer `transform`; opacity fades. List explicit properties — `transition: opacity 0.2s, transform 0.2s`.

**Pluses and minuses:**

| + | − |
|------|------|
| Simple micro-interactions without JS | `transition: all` accidentally animates layout props |
| Composable easing per property | No control over mid-animation without JS |
| Respects `prefers-reduced-motion` if you gate it | Long durations on layout props cause jank |

**Examples:**

```css
.button {
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.button:hover {
  background-color: #1d4ed8;
  transform: translateY(-1px);
}
```

Shorthand: `transition: property duration timing-function delay`

**Sources:** [MDN — transition](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)

</details>

<details>
  <summary>@keyframes animation</summary>

### @keyframes animation

**Definition:** `@keyframes` defines multi-step animations applied via the `animation` shorthand.

**How it works:**

- `@keyframes` defines intermediate steps (`from`/`to` or `0%`/`100%`); `animation` shorthand applies them.
- `animation-fill-mode: forwards` keeps final keyframe state after end; `both` applies before and after.
- `animation-iteration-count: infinite` for loaders; `animation-play-state: paused` for hover control.
- Unlike `transition`, runs without a trigger property change — starts on load or when `animation-name` is set.

`animation-name` · `duration` · `timing-function` · `delay` · `iteration-count` · `direction` · `fill-mode` · `play-state`

**When to use:** Loaders (`infinite rotate`), entrance effects (`fade-in forwards`), attention pulses. Prefer `transform`/`opacity` keyframes; gate with `@media (prefers-reduced-motion: reduce)`.

**Pluses and minuses:**

| + | − |
|------|------|
| Multi-step, looping, bidirectional | Harder to reverse mid-flight than transitions |
| Independent of property triggers | Animating layout props (`width`, `top`) causes reflow |
| `animation-timeline` for scroll-driven | Many simultaneous animations → GPU memory pressure |

**Examples:**

```css
@keyframes fade-in {
  from { opacity: 0; transform: translateY(8px); }
  to   { opacity: 1; transform: translateY(0); }
}

.modal {
  animation: fade-in 0.3s ease forwards;
}
```

**Sources:** [MDN — @keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes)

</details>

<details>
  <summary>JS animation events</summary>

### JS animation events

**Definition:** `transitionend` and `animationend` fire when CSS animations complete — useful for DOM cleanup in JS.

**Examples:**

```js
el.addEventListener('transitionend', (e) => {
  if (e.propertyName === 'opacity') { /* done */ }
});

el.addEventListener('animationend', () => { /* keyframes done */ });
```

Check `e.target === el` to avoid bubbling from children.

---

**Sources:** [MDN — transitionend](https://developer.mozilla.org/en-US/docs/Web/API/Element/transitionend_event)

</details>

<a name="visual-effects"></a>

## Visual Effects

<details>
  <summary>Filters</summary>

### Filters

**Definition:** CSS `filter` applies graphical effects (blur, brightness, grayscale) to an element's rendered output.

**How it works:**

- `filter` applies graphical effects to the **entire element** (including children) as a single bitmap.
- Functions chain left-to-right: `filter: contrast(1.2) brightness(0.9)`.
- `filter: opacity()` differs from `opacity` property — filter opacity does not create stacking context the same way.
- `drop-shadow()` follows alpha shape — better for icons than `box-shadow` on transparent PNGs.

**When to use:** Image hover effects (`brightness`, `saturate`); disabled state (`grayscale`); icon shadows (`drop-shadow`). Avoid animating `filter: blur()` on full-screen overlays on mobile.

**Pluses and minuses:**

| + | − |
|------|------|
| GPU-composited in many cases | Expensive on large elements (full repaint) |
| `drop-shadow` follows transparency | Chained filters multiply cost |
| No layout impact | `filter` on parent affects all descendants |

**Examples:**

```css
.blur-bg   { filter: blur(4px); }
.dim       { filter: brightness(0.8); }
.mono      { filter: grayscale(100%); }
.combined  { filter: contrast(1.2) saturate(1.1); }
```

Functions: `blur()` · `brightness()` · `contrast()` · `grayscale()` · `hue-rotate()` · `invert()` · `opacity()` · `saturate()` · `sepia()` · `drop-shadow()`

**Sources:** [MDN — filter](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)

</details>

<details>
  <summary>clip-path & shape</summary>

### clip-path & shape

**Definition:** `clip-path` masks element shape; `shape-outside` lets floated content wrap around non-rectangular boxes.

**How it works:**

- `clip-path` masks visible area — content outside path is clipped, not scrolled.
- Basic shapes: `circle()`, `ellipse()`, `inset()`, `polygon()`, `path()`; `clip-path: url(#svg)` for complex SVG paths.
- `shape-outside` affects float wrap geometry only — element must be floated.
- `clip-path` triggers paint; animating complex paths can be costly.

**When to use:** Circular avatars (`circle(50%)`); diagonal section dividers (`polygon`); floated images with text wrap (`shape-outside`). Prefer `border-radius` when a circle suffices.

**Pluses and minuses:**

| + | − |
|------|------|
| Non-rectangular avatars and heroes | Complex `polygon()` hard to maintain — use SVG |
| `shape-outside` for magazine layouts | `clip-path` does not affect hit-testing uniformly in all browsers |
| Animate with `@keyframes` on simple shapes | Float + `shape-outside` is legacy pattern |

**Examples:**

```css
.avatar {
  clip-path: circle(50%);
}

.blob {
  clip-path: polygon(0 0, 100% 0, 100% 85%, 50% 100%, 0 85%);
}

.float-text {
  float: left;
  shape-outside: circle(50%);
  width: 150px;
  height: 150px;
  border-radius: 50%;
}
```

**Sources:** [MDN — clip-path](https://developer.mozilla.org/en-US/docs/Web/CSS/clip-path)

</details>

<details>
  <summary>backdrop-filter</summary>

### backdrop-filter

**Definition:** `backdrop-filter` blurs or filters content behind semi-transparent elements (glass morphism).

**Examples:**

```css
.glass {
  background: rgb(255 255 255 / 20%);
  backdrop-filter: blur(12px);
}
```

**Sources:** [MDN — backdrop-filter](https://developer.mozilla.org/en-US/docs/Web/CSS/backdrop-filter)

</details>

<details>
  <summary>SVG styling</summary>

### SVG styling

**Definition:**

SVG elements use `fill` and `stroke` instead of `background` and `border`.

SVG uses `fill`, `stroke`, `stroke-width` instead of `background`/`border`:

**Examples:**

```css
.icon {
  fill: currentColor;
  stroke: none;
}
```

**Sources:** [MDN — SVG styling](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Fills_and_Strokes)

</details>

<details>
  <summary>scroll-snap</summary>

### scroll-snap

**Definition:** Scroll snap aligns scroll positions to defined points for carousels and paginated scroll areas.

**Examples:**

```css
.carousel {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  gap: 1rem;
}

.slide {
  scroll-snap-align: start;
  flex: 0 0 100%;
}
```

**Sources:** [MDN — scroll-snap](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_scroll_snap)

</details>

<details>
  <summary>pointer-events & outline</summary>

### pointer-events & outline

**Definition:** `pointer-events` controls hit testing; `:focus-visible` provides keyboard-only focus rings.

**Examples:**

```css
.overlay {
  pointer-events: none; /* clicks pass through */
}

.overlay .button {
  pointer-events: auto;
}

:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 2px;
}
```

Use `:focus-visible` instead of `:focus` for keyboard-only focus rings.

---

**Sources:** [MDN — :focus-visible](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible)

</details>

<a name="modern-css"></a>
