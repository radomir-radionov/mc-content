# HTML — Media

Images, picture, audio, video, canvas, and SVG.

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Media & graphics](#media-graphics)

---

## Media & graphics

<details>
  <summary>img element and responsive images</summary>

### img element and responsive images

**Definition:**

The `<img>` element embeds images. Use `alt`, explicit dimensions, `srcset`/`sizes`, and `loading` for performance and accessibility.

| Attribute | Purpose |
|-----------|---------|
| `alt` | Alternative text (required for meaningful images) |
| `width` / `height` | Reserve space → prevent CLS |
| `srcset` + `sizes` | Responsive selection |
| `loading="lazy"` | Defer off-screen images |

**How it works:**

- `src` fallback; `srcset` + `sizes` pick resolution by viewport.
- `alt` required for meaningful images; decorative → `alt=""`.
- `width`/`height` reserve space to prevent CLS.

| Attribute | Purpose |
|-----------|--------|
| `alt` | Text alternative |
| `srcset`/`sizes` | Responsive selection |
| `loading="lazy"` | Defer off-screen |

**When to use:**

Hero LCP image · lazy below-fold · retina `srcset`.

#### Common pitfalls

- Missing `alt` on content images.
- `loading="lazy"` on LCP hero.
- No dimensions → layout shift on load.

**Examples:**

```html
<img
  src="hero-800.jpg"
  srcset="hero-400.jpg 400w, hero-800.jpg 800w, hero-1200.jpg 1200w"
  sizes="(max-width: 600px) 100vw, 50vw"
  alt="Team collaborating in an office"
  width="800"
  height="450"
  loading="lazy"
  fetchpriority="high"
/>
```

**Sources:** [MDN — Responsive images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images)

</details>

<details>
  <summary>picture element</summary>

### picture element

**Definition:**

The `<picture>` element provides art direction and modern format fallbacks (AVIF → WebP → JPEG).

| Use case | Approach |
|----------|----------|
| Format fallback | Multiple `<source type>` |
| Art direction | Different crops per breakpoint |

**How it works:**

- `<picture>` wraps multiple `<source>` + fallback `<img>`.
- `type` on source for format negotiation (AVIF/WebP).
- `media` on source for art direction breakpoints.

| Use | Approach |
|-----|----------|
| Format fallback | `<source type="image/avif">` |
| Art direction | `<source media="(max-width: 600px)">` |

**When to use:**

AVIF with JPEG fallback · mobile crop vs desktop crop.

#### Common pitfalls

- Forgetting required `<img>` fallback.
- Same `srcset` on all sources (no benefit).
- `picture` for simple resize (use `img srcset` only).

**Examples:**

```html
<picture>
  <source srcset="hero.avif" type="image/avif" />
  <source srcset="hero.webp" type="image/webp" />
  <img src="hero.jpg" alt="Hero banner" />
</picture>
```

**Sources:** [MDN — picture](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture)

</details>

<details>
  <summary>video and audio</summary>

### video and audio

**Definition:**

The `<video>` and `<audio>` elements embed media with native controls. Provide multiple formats and captions for accessibility.

| Attribute | Notes |
|-----------|-------|
| `controls` | Native playback UI |
| `preload` | `none`, `metadata`, `auto` |
| `poster` | Thumbnail before play |
| `<track>` | Captions/subtitles |

**How it works:**

- `<video>`/`<audio>` embed media with native controls.
- Multiple `<source>` for codec fallback.
- `<track kind="captions">` for accessibility.

| Attribute | Notes |
|-----------|-------|
| `controls` | Native UI |
| `poster` | Video thumbnail |
| `<track>` | Captions/subtitles |

**When to use:**

Product demo video · podcast player · background video (prefer CSS, muted, `playsinline`).

#### Common pitfalls

- Autoplay with sound (blocked by browsers).
- No captions on speech content.
- Single format without fallback source.

**Examples:**

```html
<video controls preload="metadata" poster="thumb.jpg">
  <source src="clip.webm" type="video/webm" />
  <source src="clip.mp4" type="video/mp4" />
  <track kind="captions" src="en.vtt" srclang="en" label="English" default />
</video>
```

**Sources:** [MDN — video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)

</details>

<details>
  <summary>canvas vs SVG</summary>

### canvas vs SVG

**Definition:**

`<canvas>` is a bitmap surface drawn via JavaScript. `<svg>` is vector markup in the DOM — scalable and accessible.

| | `<canvas>` | `<svg>` |
|---|-----------|---------|
| Model | Bitmap (pixels) | Vector (DOM nodes) |
| Scaling | Loses quality | Resolution-independent |
| Accessibility | Needs ARIA fallback | Native DOM |
| Best for | Games, heavy pixel animation | Icons, diagrams |

**How it works:**

- `<canvas>` — bitmap surface drawn via JS API (`getContext('2d')`).
- `<svg>` — vector DOM; scales cleanly; style with CSS.
- Canvas for pixels/games; SVG for icons/diagrams.

| | `<canvas>` | `<svg>` |
|---|-----------|--------|
| Model | Bitmap | Vector DOM |
| A11y | Needs fallback text | DOM accessible |

**When to use:**

Chart.js on canvas · inline icon SVG · game sprites on canvas.

#### Common pitfalls

- Icon fonts instead of SVG (a11y, scaling).
- Canvas charts without text alternative.
- Huge SVGs not optimized (SVGO).

**Examples:**

```html
<canvas id="chart" width="400" height="200"></canvas>
<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M10 20v-6h4v6z"/></svg>
```

**Thoughts:** Canvas drawing API → [3_js_browser.md](./3_js_browser.md). Standalone `.svg` files are XML: set `xmlns="http://www.w3.org/2000/svg"` on the outer `<svg>` or the browser will not treat the file as SVG. Inline SVG in HTML does not need `xmlns`. Namespaces: [6_web_api.md — XML](./6_web_api_rest.md#xml).

**Sources:** [MDN — canvas](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas)

</details>

---

<a name="performance--resource-loading"></a>
