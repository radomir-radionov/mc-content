# CSS — Visual

Typography, color, decoration, form styling, and print.

[← Back to hub](./2_css.md)

### Related docs

- [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md) · [UX & UI principles](./7_special_ux.md#ux-and-ui-principles)

### Sections

- [Typography & Colors](#typography-colors)
- [Decoration — Background, Border, Shadow](#decoration-background-border-shadow)
- [Form & Native Control Styling](#form-native-control-styling)
- [Print & Paged Media](#print-paged-media)

---

## Typography & Colors

<details>
  <summary>Font loading</summary>

### Font loading

**Definition:** `@font-face` and `font-display` control how web fonts load and swap to avoid invisible text.

**How it works:**

single file, `font-weight: 100 900` range, `font-variation-settings`.

`font-family: "Inter", system-ui, sans-serif;` — always end with generic family.

Match fallback metrics with `size-adjust`, `ascent-override`, `descent-override`, `line-gap-override` so the swap does not shift layout. CLS (FOIT/FOUT, preload, WOFF2, `unicode-range`) → [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift).

**Examples:**

```css
@font-face {
  font-family: "Inter";
  src: url("/fonts/inter.woff2") format("woff2");
  font-weight: 100 900;
  font-display: swap; /* show fallback immediately, swap when loaded */
}
```

| `font-display` | Behavior                                               |
| -------------- | ------------------------------------------------------ |
| `swap`         | Text visible immediately; may FOUT                     |
| `optional`     | Short block period; may keep fallback on slow networks |
| `block`        | Invisible text during load (FOIT) — avoid              |

**Sources:** [MDN — @font-face](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face)

</details>

<details>
  <summary>Typography properties</summary>

### Typography properties

**Definition:** Typography properties control font size, line height, truncation, wrapping, and text rendering quality.

**How it works:**

```css
/* Single-line ellipsis */
.ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Multi-line clamp — see .truncate above */
```

| Property                    | Effect                                                 |
| --------------------------- | ------------------------------------------------------ |
| `overflow-wrap: break-word` | Break long words at arbitrary points if needed         |
| `word-break: break-all`     | Break anywhere (URLs, codes)                           |
| `hyphens: auto`             | Language-aware hyphenation (`lang` attribute required) |

**When to use:**

#### `white-space` pitfalls

| Value      | Behavior                                      |
| ---------- | --------------------------------------------- |
| `normal`   | Collapse whitespace; wrap                     |
| `nowrap`   | No wrap — needs `overflow: hidden` + ellipsis |
| `pre-wrap` | Preserve whitespace; wrap                     |

**Examples:**

```css
body {
  font-size: 1rem;
  line-height: 1.5;
  font-weight: 400;
  text-wrap: pretty; /* reduce orphans */
}

.heading {
  font-size: clamp(1.25rem, 2vw + 1rem, 2rem);
  letter-spacing: -0.02em;
}

.truncate {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

- `line-height` — unitless preferred (1.5 scales with font-size)
- `line-clamp` — truncate multi-line text (use with `overflow: hidden`)
- `text-rendering: optimizeLegibility` — better kerning/ligatures; minor perf cost
- `text-decoration-skip-ink: auto` — underline skips glyph descenders

**Sources:** [MDN — Typography](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_text)

</details>

<details>
  <summary>text-wrap: balance / pretty</summary>

### text-wrap: balance / pretty

**Definition:** `text-wrap` controls how the browser breaks lines — `balance` evens line lengths in headings; `pretty` reduces orphans in body text.

**How it works:**

- `text-wrap: balance` — browser redistributes words across lines for more even line lengths (best for short headings).
- `text-wrap: pretty` — prefers breaks that avoid single-word last lines (orphans) in paragraphs.
- `text-wrap: wrap` — default wrapping behavior.

**When to use:** `balance` on `h1`–`h3` and card titles; `pretty` on article body copy; keep `nowrap` + ellipsis for table cells and badges.

**Pluses and minuses:**

| +                                                | −                                                           |
| ------------------------------------------------ | ----------------------------------------------------------- |
| Better typography without JS                     | `balance` can be costly on long text — use on headings only |
| Progressive enhancement                          | Check caniuse — growing support in Chromium/Safari/Firefox  |
| Pairs with `text-wrap: nowrap` + ellipsis for UI | Not a substitute for manual `<wbr>` in URLs                 |

**Examples:**

```css
h1,
h2,
h3 {
  text-wrap: balance;
}

article p {
  text-wrap: pretty;
}
```

**Sources:** [MDN — text-wrap](https://developer.mozilla.org/en-US/docs/Web/CSS/text-wrap)

</details>

<details>
  <summary>Color formats</summary>

### Color formats

**Definition:** Modern color formats (`rgb()`, `hsl()`, `oklch()`, `color-mix()`) support perceptually uniform theming.

**Examples:**

```css
.keyword {
  color: navy;
}
.hex {
  color: #2563eb;
}
.rgb {
  color: rgb(37 99 235);
}
.rgba {
  color: rgb(37 99 235 / 80%);
}
.hsl {
  color: hsl(220 90% 56%);
}
.oklch {
  color: oklch(58% 0.22 264);
} /* perceptually uniform */
.mix {
  color: color-mix(in oklch, blue 70%, white);
}
.current {
  border-color: currentColor;
}
```

**Prefer `oklch` / `hsl` for theming** — easier to adjust lightness/saturation than hex.

**Sources:** [MDN — color](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value)

</details>

<details>
  <summary>Gradients</summary>

### Gradients

**Definition:** CSS gradients (`linear-gradient`, `radial-gradient`) create smooth color transitions without image assets.

**Examples:**

```css
.hero {
  background: linear-gradient(135deg, #667eea, #764ba2);
}

.badge {
  background: radial-gradient(circle at top left, #fff, #f0f0f0);
}

.shimmer {
  background: linear-gradient(90deg, #eee 25%, #f5f5f5 50%, #eee 75%);
  background-size: 200% 100%;
}
```

**Sources:** [MDN — gradient](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient)

</details>

<details>
  <summary>Dark mode</summary>

### Dark mode

**Definition:** Dark mode uses `prefers-color-scheme` and custom properties to swap color tokens at runtime.

**Examples:**

```css
:root {
  color-scheme: light dark;
  --bg: #fff;
  --text: #111;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #111;
    --text: #eee;
  }
}

body {
  background: var(--bg);
  color: var(--text);
}
```

**Sources:** [MDN — prefers-color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)

</details>

<details>
  <summary>color-scheme & light-dark()</summary>

### color-scheme & light-dark()

**Definition:** `color-scheme` tells the browser which color schemes the page supports (affects scrollbars, form controls, `canvas`); `light-dark()` picks a value based on the used color scheme.

**How it works:**

- `color-scheme: light dark` on `:root` — page supports both; browser picks from `prefers-color-scheme` or author override.
- `color-scheme: dark` — hint dark-native UI chrome even if tokens are custom.
- `light-dark(light-value, dark-value)` — one declaration for both themes without duplicating rules.

**When to use:** Set `color-scheme: light dark` on `:root`; use `light-dark()` for borders and surfaces; combine with `prefers-color-scheme` for complex token swaps.

**Pluses and minuses:**

| +                                 | −                                                            |
| --------------------------------- | ------------------------------------------------------------ |
| Native form control theming       | `light-dark()` needs `color-scheme` set on root              |
| Less `@media` duplication         | Not a full design-token system — pair with custom properties |
| Scrollbar/UI chrome matches theme | Test contrast in both schemes                                |

**Examples:**

```css
:root {
  color-scheme: light dark;
  --surface: light-dark(#fff, #1a1a1a);
  --text: light-dark(#111, #eee);
  --border: light-dark(#e5e7eb, #333);
}

body {
  background: var(--surface);
  color: var(--text);
  border: 1px solid var(--border);
}
```

**Sources:** [MDN — color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme) · [light-dark()](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark)

</details>

<a name="decoration"></a>

## Decoration — Background, Border, Shadow

<details>
  <summary>Background</summary>

### Background

**Definition:**

Background longhands control color, image, size, position, repeat, clip, and origin independently.

Prefer longhands when overriding a single sub-property — `background:` shorthand resets all sub-properties.

**Examples:**

```css
.hero {
  background-color: #1e293b;
  background-image: url("/bg.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed; /* parallax-like; avoid on mobile */
  background-clip: border-box;
  background-origin: padding-box;
}
```

| Property              | Purpose                                          |
| --------------------- | ------------------------------------------------ |
| `background-size`     | `cover` \| `contain` \| explicit size            |
| `background-position` | Anchor point (`center top`, `50% 50%`)           |
| `background-repeat`   | `repeat` \| `no-repeat` \| `space` \| `round`    |
| `background-clip`     | Paint area (`border-box`, `padding-box`, `text`) |
| `background-origin`   | Positioning box for `background-position`        |

Multiple backgrounds stack first-declared on top:

```css
.badge {
  background:
    linear-gradient(rgb(0 0 0 / 40%), rgb(0 0 0 / 40%)),
    url("/photo.jpg") center / cover no-repeat;
}
```

**Sources:** [MDN — background](https://developer.mozilla.org/en-US/docs/Web/CSS/background)

</details>

<details>
  <summary>Border, outline & box-shadow</summary>

### Border, outline & box-shadow

**Definition:** Borders affect layout; outlines do not. `box-shadow` paints multiple layered shadows on the box.

**How it works:**

|             | `box-shadow`                  | `filter: drop-shadow()`        |
| ----------- | ----------------------------- | ------------------------------ |
| Follows     | Box (including border-radius) | Alpha shape of content         |
| SVG/images  | Rectangular box only          | Follows non-transparent pixels |
| Performance | Paint                         | Composite                      |

**Examples:**

```css
.card {
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  outline: 2px solid transparent; /* room for focus ring */
  outline-offset: 2px;
  box-shadow:
    0 1px 2px rgb(0 0 0 / 5%),
    0 8px 24px rgb(0 0 0 / 10%);
}
```

| Property        | Notes                                                     |
| --------------- | --------------------------------------------------------- |
| `border`        | Shorthand — sets width, style, color on all sides         |
| `border-radius` | Per-corner: `border-radius: 8px 8px 0 0`                  |
| `outline`       | Does not affect layout (unlike border); use for focus     |
| `box-shadow`    | Multiple layers comma-separated; `inset` for inner shadow |

**Sources:** [MDN — box-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow)

</details>

---

<a name="form-styling"></a>

## Form & Native Control Styling

<details>
  <summary>Customizing form controls</summary>

### Customizing form controls

**Definition:** Native form controls can be restyled with `appearance: none`, `accent-color`, and pseudo-element selectors.

**How it works:** fully custom `<select>` dropdowns still inconsistent — use native `<select>` with `appearance: none` + custom arrow, or build a custom component with ARIA for full control.

**Examples:**

```css
.input {
  appearance: none;
  width: 100%;
  padding: 0.5rem 0.75rem;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  accent-color: #2563eb;
}

.input::placeholder {
  color: #9ca3af;
}

.input:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

input[type="file"]::file-selector-button {
  padding: 0.25rem 0.75rem;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  background: #f9fafb;
}
```

| Technique          | Applies to                          |
| ------------------ | ----------------------------------- |
| `appearance: none` | Remove OS chrome — rebuild with CSS |
| `accent-color`     | Checkbox, radio, range, progress    |
| `::placeholder`    | Placeholder text color              |
| `:focus-visible`   | Custom focus ring                   |

**Sources:** [MDN — appearance](https://developer.mozilla.org/en-US/docs/Web/CSS/appearance)

</details>

<details>
  <summary>Pure CSS toggle (no JavaScript)</summary>

### Pure CSS toggle (no JavaScript)

**Definition:**

A hidden checkbox plus sibling selectors (`:checked +`) can drive a toggle UI without JavaScript.

iOS-style switch using hidden checkbox + sibling selector:

```html
<label class="switch">
  <input type="checkbox" class="switch__input" />
  <span class="switch__track" aria-hidden="true"></span>
</label>
```

**Examples:**

```css
.switch__input {
  position: absolute;
  width: 1px;
  height: 1px;
  opacity: 0;
}

.switch__track {
  display: inline-block;
  width: 48px;
  height: 28px;
  background: #d1d5db;
  border-radius: 999px;
  position: relative;
  transition: background 0.2s ease;
  cursor: pointer;
}

.switch__track::after {
  content: "";
  position: absolute;
  top: 2px;
  left: 2px;
  width: 24px;
  height: 24px;
  background: #fff;
  border-radius: 50%;
  transition: transform 0.2s ease;
}

.switch__input:checked + .switch__track {
  background: #22c55e;
}

.switch__input:checked + .switch__track::after {
  transform: translateX(20px);
}

.switch__input:focus-visible + .switch__track {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}
```

Uses `:checked` + adjacent sibling `+`. Keep native `<input type="checkbox">` for accessibility and form submission.

**Sources:** [CSS for JS — forms](https://learn.javascript.ru/css-for-js)

</details>

---

<a name="print"></a>

## Print & Paged Media

<details>
  <summary>Print styles</summary>

### Print styles

**Definition:** `@media print` hides chrome, adjusts typography, and controls page breaks for printable output.

**How it works:**

```css
@page {
  margin: 2cm;
  size: A4 portrait;
}
```

Avoid dark backgrounds in print — wastes ink; expand URLs only when useful.

**Examples:**

```css
@media print {
  nav,
  .sidebar,
  .ads,
  .no-print {
    display: none !important;
  }
  body {
    font-size: 12pt;
    color: #000;
    background: #fff;
  }
  a[href^="http"]::after {
    content: " (" attr(href) ")";
    font-size: 0.85em;
  }
  .page-break {
    break-before: page;
  }
  h1,
  h2,
  h3 {
    break-after: avoid;
  }
  img {
    max-width: 100% !important;
  }
}
```

| Property                       | Purpose                             |
| ------------------------------ | ----------------------------------- |
| `break-before` / `break-after` | Force or avoid page breaks          |
| `break-inside: avoid`          | Keep block on one page              |
| `print-color-adjust: exact`    | Print background colors as designed |

**Sources:** [MDN — Print](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries#printing)

</details>

---

<a name="transforms-animations"></a>
