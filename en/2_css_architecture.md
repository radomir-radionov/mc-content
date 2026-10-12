# CSS — Architecture

Methodologies, tooling, and rendering performance.

[← Back to hub](./2_css.md)

### Related docs

- [CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md) · [SDLC — methodologies](./7_special_sdlc.md#development-methodologies)

### Sections

- [Architecture & Tooling](#architecture-tooling)
- [Performance](#performance)

---

## Architecture & Tooling

<details>
  <summary>BEM (Block Element Modifier)</summary>

### BEM (Block Element Modifier)

**Definition:**

BEM is a naming convention: `block__element--modifier` for predictable, low-specificity CSS.

Naming convention for modular CSS:

**Pluses and minuses:**

predictable specificity, reusable blocks, team consistency

verbose class names; not required if using CSS Modules or scoped frameworks

**Examples:**

```
block__element--modifier
```

```html
<button class="button button--primary button--large">
  <span class="button__icon">...</span>
  <span class="button__text">Submit</span>
</button>
```

| Part     | Separator | Example           |
| -------- | --------- | ----------------- |
| Block    | —         | `button`          |
| Element  | `__`      | `button__icon`    |
| Modifier | `--`      | `button--primary` |

**Sources:** [getbem.com](http://getbem.com/)

</details>

<details>
  <summary>CSS preprocessors (Sass/SCSS, Less)</summary>

### CSS preprocessors (Sass/SCSS, Less)

**Definition:**

Preprocessors compile Sass/Less to CSS, adding variables, mixins, and nesting at build time.

Compile to CSS. Historically added variables, nesting, mixins, imports.

**How it works:**

native custom properties and nesting reduce the need for preprocessors. Sass still useful for `@mixin`, `@function`, and large design-token pipelines.

SCSS uses `{}` syntax (CSS-compatible); indented Sass syntax is less common.

**Examples:**

```scss
$primary: #2563eb;

@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  color: $primary;

  &__title {
    font-weight: 700;
  }
}
```

**Sources:** [Sass](https://sass-lang.com/)

</details>

<details>
  <summary>PostCSS & Autoprefixer</summary>

### PostCSS & Autoprefixer

**Definition:**

PostCSS transforms CSS with plugins; Autoprefixer adds vendor prefixes from Browserslist config.

PostCSS transforms CSS with plugins. **Autoprefixer** adds vendor prefixes from [Browserslist](https://browsersl.ist/) config — do not hand-write `-webkit-` prefixes in 2025.

**How it works:**

- PostCSS parses CSS to an AST; plugins transform and stringify back — like Babel for CSS.
- **Autoprefixer** reads Browserslist (`> 1%, last 2 versions`) and adds only needed prefixes.
- Other plugins: `postcss-preset-env` (stage features), `cssnano` (minify), `postcss-nesting`, custom transforms.
- Runs at build time in Vite/Webpack — zero runtime cost.

**When to use:** Autoprefixer for all production builds; `postcss-preset-env` for nesting/`color-mix` polyfills; `cssnano` for production minification. Set Browserslist in `package.json` — single source of truth.

**Pluses and minuses:**

| +                                       | −                                                 |
| --------------------------------------- | ------------------------------------------------- |
| Write modern CSS; ship compatible CSS   | Extra build step and config                       |
| Autoprefixer eliminates manual prefixes | Plugin order matters — can break if misconfigured |
| Ecosystem of transforms                 | Debugging generated CSS harder than source        |

**Examples:**

```json
// package.json
"browserslist": ["> 1%", "last 2 versions", "not dead"]
```

```js
// postcss.config.js
export default {
  plugins: {
    "postcss-preset-env": { stage: 2 },
    autoprefixer: {},
    cssnano: process.env.NODE_ENV === "production" ? {} : false,
  },
};
```

**Sources:** [PostCSS](https://postcss.org/) · [Autoprefixer](https://github.com/postcss/autoprefixer)

</details>

<details>
  <summary>CSS Modules</summary>

### CSS Modules

**Definition:**

CSS Modules scope class names at build time to prevent global style collisions.

Scope class names at build time — `import styles from './Button.module.css'` → unique hashes prevent global collisions.

**How it works:**

- Bundler renames `.button` → `.Button_button_x7f3a` at compile time.
- Import as JS object: `className={styles.button}` — typo-safe with TypeScript plugins.
- `:global(.foo)` escapes scoping for third-party overrides; `:local` is default.
- Composition: `composes: primary from './theme.module.css'`.

**When to use:** React/Vue/Svelte component styles; design-system primitives; replacing BEM in SPAs. Pair with CSS custom properties for theming across modules.

**Pluses and minuses:**

| +                                 | −                                                     |
| --------------------------------- | ----------------------------------------------------- |
| No global namespace collisions    | Class names unreadable in DevTools                    |
| Co-located styles with components | Dynamic class names need `styles[variant]` pattern    |
| Works with any framework          | No true DOM encapsulation — globals can still leak in |

**Examples:**

```css
/* Button.module.css */
.button {
  padding: 0.5rem 1rem;
}
.primary {
  composes: button;
  background: var(--brand);
}
```

```jsx
import styles from "./Button.module.css";
<button className={styles.primary}>Submit</button>;
```

**Sources:** [CSS Modules](https://github.com/css-modules/css-modules)

</details>

<details>
  <summary>Tailwind CSS (utility-first)</summary>

### Tailwind CSS (utility-first)

**Definition:** Tailwind applies atomic utility classes in HTML for rapid, token-consistent styling.

**How it works:** Tailwind utilities have equal specificity — order in generated CSS matters, not HTML class order. Use `tailwind-merge` or arbitrary variants for conflicts.

**Pluses and minuses:**

| +                         | −                                            |
| ------------------------- | -------------------------------------------- |
| Fast prototyping          | HTML verbosity                               |
| Consistent design tokens  | Learning curve                               |
| Purged unused CSS in prod | Class precedence issues with dynamic classes |

**Examples:**

```html
<button class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
  Submit
</button>
```

**Sources:** [Learn Tailwind — responsive card](https://www.freecodecamp.org/news/learn-tailwind-css-by-building-a-responsive-product-card/)

</details>

<details>
  <summary>Bootstrap</summary>

### Bootstrap

**Definition:**

Bootstrap is a component framework with grid, utilities, and optional JS widgets.

Component framework with grid, utilities, and JS widgets. Good for rapid MVPs; learn underlying CSS to avoid fighting the framework.

**Sources:** [Bootstrap](https://getbootstrap.com/docs/)

</details>

<details>
  <summary>CSS methodologies comparison</summary>

### CSS methodologies comparison

**Definition:**

Methodologies (BEM, OOCSS, SMACSS, ITCSS) organize CSS for maintainability and team scale.

| Method     | Idea                                          |
| ---------- | --------------------------------------------- |
| **BEM**    | Block/element/modifier naming                 |
| **OOCSS**  | Separate structure and skin                   |
| **SMACSS** | Base, layout, module, state, theme categories |
| **ITCSS**  | Inverted triangle — specificity layers        |

---

**Sources:** [CSS-Tricks — methodologies](https://css-tricks.com/poll-results-what-css-methodology-do-you-use/)

</details>

<a name="performance"></a>

## Performance

<details>
  <summary>Reflow, repaint, composite</summary>

### Reflow, repaint, composite

**Definition:**

Style changes trigger style recalc, layout (reflow), paint, or composite — with different performance costs.

When CSS changes, the browser pays different costs:

| Step                | Trigger examples                                     | Cost       |
| ------------------- | ---------------------------------------------------- | ---------- |
| **Style recalc**    | class change, `:hover`                               | Low–medium |
| **Layout (reflow)** | `width`, `height`, `margin`, `font-size`, DOM insert | High       |
| **Paint**           | `color`, `background`, `box-shadow`, `visibility`    | Medium     |
| **Composite**       | `transform`, `opacity`, `filter`                     | Low (GPU)  |

**How it works:**

animate `transform` and `opacity`; avoid layout properties in animation loops. Batch DOM reads/writes in JS to avoid layout thrashing.

`width`, `height`, `min-*`, `max-*`, `margin`, `padding`, `border`, `font-size`, `line-height`, `display`, `position`, `top`/`left`, `flex-*`, `grid-*`, DOM insert/remove, reading `offsetHeight`/`getBoundingClientRect()` after a write.

```
Need to animate?
├─ transform / opacity only → Composite (best)
├─ color / box-shadow / background → Paint (ok for short transitions)
└─ width / height / margin / top → Layout (expensive — avoid in loops)
```

**Thoughts:** Full pipeline, dirty/global layout, rAF vs interval → [1_html.md — Browser & rendering](./1_html_rendering.md#browser--rendering). Property → layout/paint/composite → [CSS Triggers](https://csstriggers.com/). Field metrics (LCP / INP / CLS) → [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals). INP phases/fixes → [6_web_api.md — INP](./1_html_performance.md#inp-interaction-to-next-paint). Layout-shift metric → [CLS](./1_html_performance.md#cls-cumulative-layout-shift).

**Sources:** [web.dev — Rendering performance](https://web.dev/articles/critical-rendering-path)

</details>

<details>
  <summary>Critical CSS</summary>

### Critical CSS

**Definition:**

Critical CSS inlines above-the-fold styles to reduce render-blocking and improve first paint.

Inline or preload CSS required for **above-the-fold** content to reduce render-blocking.

Benefits: faster first paint, better LCP, improved mobile performance.

**Sources:** [web.dev — Extract critical CSS](https://web.dev/articles/extract-critical-css)

</details>

<details>
  <summary>Compositor layers & GPU</summary>

### Compositor layers & GPU

**Definition:**

Compositor layers isolate GPU-friendly properties (`transform`, `opacity`) for smooth animation.

Properties that typically composite on GPU (no layout/paint): `transform`, `opacity`, `filter`, `will-change`.

**How it works:**

`transform`, `opacity < 1`, `filter`, `will-change`, `position: fixed`, video/canvas, etc.

| Do                                            | Don't                                             |
| --------------------------------------------- | ------------------------------------------------- |
| Add briefly before a known animation          | Leave on dozens of elements permanently           |
| Use `will-change: transform` for hover lifts  | Use `will-change: auto` everywhere "just in case" |
| Remove after `transitionend` / `animationend` | Expect it to fix layout bugs                      |

**When to use:**

#### Pitfall

too many layers → high memory use → crashes on mobile. Do not sprinkle `will-change` everywhere; apply before animation, remove after.

**Sources:** [web.dev — Layers](https://web.dev/articles/stick-to-compositor-only-properties-and-manage-layer-count)

</details>

<details>
  <summary>contain & content-visibility</summary>

### contain & content-visibility

**Definition:** `contain` isolates layout/paint; `content-visibility: auto` skips rendering off-screen content.

**How it works:**

```css
.section {
  content-visibility: auto;
  contain-intrinsic-size: auto 500px; /* placeholder height prevents scrollbar jump */
}
```

Browser skips layout/paint for off-screen sections; `contain-intrinsic-size` reserves approximate space so scroll position stays stable.

| `contain` value | Isolates                               |
| --------------- | -------------------------------------- |
| `layout`        | Internal layout from affecting outside |
| `style`         | Counters, quotes                       |
| `paint`         | Painting to element box                |
| `size`          | Size as if empty (use carefully)       |
| `strict`        | `layout style paint size`              |

**Examples:**

```css
.card-list-item {
  contain: layout style paint;
}

.below-fold {
  content-visibility: auto;
  contain-intrinsic-size: 0 500px;
}
```

`content-visibility: auto` skips rendering off-screen content — major perf win for long lists.

**Sources:** [MDN — content-visibility](https://developer.mozilla.org/en-US/docs/Web/CSS/content-visibility) · INP / large DOM: [6_web_api.md](./1_html_performance.md#inp-interaction-to-next-paint)

</details>

<details>
  <summary>CSS sprites (historical)</summary>

### CSS sprites (historical)

**Definition:**

Sprites pack multiple icons in one image, positioned via `background-position` — largely replaced by SVG.

One image, multiple icons via `background-position`. Less relevant with HTTP/2, SVG icons, and icon fonts — but still valid for legacy sprite sheets.

**How it works:**

- Single raster image contains all icons at fixed coordinates.
- Each icon element sets `background-image: url(sprite.png)`, fixed `width`/`height`, and negative `background-position` to show the correct tile.
- HTTP/1.1 reduced requests — one image vs many; HTTP/2 reduces this advantage.

**When to use:** Legacy enterprise sites; email templates (limited SVG support). **Today:** inline SVG, SVG sprites (`<symbol>` + `<use>`), or icon fonts — not raster sprites.

**Pluses and minuses:**

| +                                   | −                                                  |
| ----------------------------------- | -------------------------------------------------- |
| Fewer HTTP requests on HTTP/1       | Painful to maintain — pixel math on every icon add |
| Consistent icon sizing in one asset | Retina requires `@2x` sprite sheets                |
| No JS required                      | SVG icons + icon components are the modern default |

**Examples:**

```css
.icon-home {
  width: 24px;
  height: 24px;
  background: url("icons.png") no-repeat 0 0;
}
.icon-search {
  background-position: -24px 0;
}
```

**Sources:** [MDN — background-position](https://developer.mozilla.org/en-US/docs/Web/CSS/background-position)

</details>

<details>
  <summary>Shorthand pitfalls</summary>

### Shorthand pitfalls

**Definition:**

CSS shorthands reset omitted sub-properties to initial values — use longhands when overriding one sub-property.

Shorthand resets unspecified sub-properties to initial values:

**Examples:**

```css
/* Dangerous — resets background-image, position, etc. */
background: #fff;

/* Safer when overriding one sub-property */
background-color: #fff;
```

---

**Sources:** [MDN — Shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties)

</details>

<details>
  <summary>Accessibility in CSS</summary>

### Accessibility in CSS

**Definition:** Accessible CSS respects motion preferences, preserves focus indicators, and maintains sufficient contrast.

**Examples:**

```css
/* Respect motion preferences */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

/* Keyboard focus only */
:focus {
  outline: none;
}
:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

/* Screen-reader only */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Sufficient contrast — aim WCAG AA: 4.5:1 body text, 3:1 large text */
```

Never remove focus indicators without a `:focus-visible` replacement.

---

**Sources:** [MDN — Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/CSS_and_JavaScript)

</details>

---

<a name="interview-practice"></a>

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Questions and hands-on tasks from [0_interview.md](0_interview.md). HTML-only topics → [1_html.md](1_html.md). Render pipeline → [1_html.md](./1_html.md).

Try answering before opening solutions.

### Original questions (from 0_interview.md)

**Junior — CSS / layout (Q53–61)**

53. What is the CSS box model?
54. What centering techniques do you know (horizontal and vertical)?
55. What layout approaches do you know (float, flex, grid, etc.)?
56. How do you make an application responsive?
57. Why do some CSS properties need vendor prefixes (`-webkit-`, `-moz-`, etc.)?
58. How do you simplify cross-browser styling?
59. Practical: review and fix a bad CSS or HTML example.
60. What are CSS preprocessors? Which have you used? What do they add over standard CSS?

> Q57 (semantic layout) → [1_html.md](1_html.md)

**CSS / layout (Q57–73, CSS only)**

57. Explain the difference between `px`, `em`, and `rem`.
58. What are CSS custom properties for? Give usage examples.
59. What happens when you add `* { box-sizing: border-box; }`?
60. How do you adapt a page for print?
61. Describe styling native form controls.
62. What is progressive rendering? What approaches exist?
63. Name ways to lazy-load media on a page.
64. Name popular CSS methodologies and their differences.
65. How does CSS Grid work?
66. Which image formats support animation?
67. How do you track CSS `@keyframes` or `transition` end in JS?
68. Which CSS properties are GPU-composited? What are compositor layers?
69. How do you reuse inline SVG elements on a page?
70. What is faux bold or faux italic font rendering?

> Q64 (templating), Q71–72, Q74–77 (SVG opt, shadow DOM, canvas) → [1_html.md](1_html.md) or general frontend

### Question index

| #     | Topic                                     | Answer below            |
| ----- | ----------------------------------------- | ----------------------- |
| 53    | Box model                                 | Box model               |
| 54    | Centering                                 | Centering               |
| 55    | float / flex / grid                       | Layout approaches       |
| 56    | Responsive app                            | Responsive              |
| 58–59 | Vendor prefixes / cross-browser           | Vendor prefixes         |
| 60    | Review bad CSS                            | Bad CSS review + Task 6 |
| 61    | Preprocessors                             | Preprocessors           |
| 57†   | px / em / rem                             | Units                   |
| 58†   | CSS variables                             | Custom properties       |
| 59†   | `* { box-sizing }`                        | Universal box-sizing    |
| 60†   | Print styles                              | Print styles            |
| 61†   | Form styling                              | Form styling            |
| 62†   | Progressive rendering                     | Progressive rendering   |
| 63†   | Lazy-loading media                        | Lazy-loading            |
| 65†   | CSS methodologies                         | Methodologies           |
| 66†   | CSS Grid                                  | Grid                    |
| 67†   | Animated image formats                    | Animated images         |
| 68†   | Animation events in JS                    | Animation JS + Task 8   |
| 69†   | GPU / compositor layers                   | GPU layers              |
| 70†   | Reuse inline SVG                          | Reuse SVG               |
| 73†   | Faux bold/italic                          | Faux fonts              |
| —     | BFC, reflow, flex overflow, implicit grid | See Q&A                 |
| —     | Containing block                          | Containing block        |
| —     | `min-width: 0` in flex                    | Flex min-width          |
| —     | `auto-fit` vs `auto-fill`                 | Auto-fit vs auto-fill   |
| —     | Intrinsic vs extrinsic sizing             | Intrinsic sizing        |
| —     | `100vh` vs `100dvh`                       | Viewport units          |
| —     | `@supports`                               | Supports queries        |
| —     | Stacking context                          | Stacking context        |
| —     | `box-shadow` vs `drop-shadow`             | Shadow vs drop-shadow   |
| —     | Native `<select>` limits                  | Select styling          |
| —     | Pure CSS toggle                           | CSS toggle              |
| 45    | iPhone toggle without JS                  | CSS toggle + Task 15    |
| Q13   | Cache busting CSS/JS                      | Cache busting           |

† Advanced block · Semantic layout (Q57) → [1_html.md](1_html.md)

---
