# JS Browser — Animation

CSS motion, easing curves, and JavaScript timed to the display refresh.

[← Back to JS hub](./3_js.md) · [Browser hub](./3_js_browser_hub.md)

### Related docs

- [CSS motion](./2_css_motion.md) · [CSS interview — timing functions](./2_css_interview.md)
- [Browser APIs — debounce / throttle](./3_js_browser_extra.md#debounce) · [Event loop](./3_js_async_event_loop.md)
- [JS Interview](./3_js_interview.md)

### Main concepts

| Area       | Topics                                           |
| ---------- | ------------------------------------------------ |
| **CSS**    | Transitions, `@keyframes`, `cubic-bezier`        |
| **JS**     | `requestAnimationFrame`, style/attribute updates |
| **Timing** | Easing, frame budget, `transitionend`            |

### Sections

- [Animation](#animation)
- [Bezier curve](#bezier-curve)
- [CSS animations](#css-animations)
- [JavaScript animations](#javascript-animations)
- [requestAnimationFrame](#requestanimationframe)

---

<a name="animation"></a>

## Animation

<details>
  <summary>Animation</summary>

### Animation

**Definition:** **Animation** changes visual state over time — via CSS (GPU-friendly) or JavaScript (full control).

**How it works:**

- **CSS:** declarative `@keyframes` and `transition` — browser interpolates between keyframes ([CSS motion](./2_css_motion.md)).
- **JS:** update `style` / attributes / canvas each frame — usually driven by [requestAnimationFrame](#requestanimationframe).
- **Events:** `transitionend`, `animationend` fire when CSS animations complete — useful for DOM cleanup.

**When to use:** Prefer CSS for simple opacity/transform moves; JS for physics, drag, or canvas games.

**Sources:** [javascript.info/animation](https://javascript.info/animation) · [MDN — CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Animations)

</details>

<a name="bezier-curve"></a>

## Bezier curve

<details>
  <summary>Bezier curve</summary>

### Bezier curve

**Definition:** A **cubic Bézier curve** describes easing: how fast a property moves between two keyframes. CSS exposes it as **`cubic-bezier(x1, y1, x2, y2)`**.

**How it works:**

- Control points `(0,0)` and `(1,1)` are fixed; `(x1,y1)` and `(x2,y2)` shape the curve (`x` values clamped to `[0,1]`).
- Presets: `ease`, `linear` (= `cubic-bezier(0,0,1,1)`), `ease-in`, `ease-out`, `ease-in-out`.
- In JS animation libraries, the same math maps normalized time `t ∈ [0,1]` to eased progress before applying to `left` / `opacity`.

**When to use:** Custom “snappy” or “soft” motion; match design-system easing tokens.

**Examples:**

```css
.panel {
  transition: transform 300ms cubic-bezier(0.34, 1.56, 0.64, 1);
}
```

**Sources:** [MDN — cubic-bezier()](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/easing-function/cubic-bezier) · [javascript.info/css-animations](https://javascript.info/css-animations)

</details>

<a name="css-animations"></a>

## CSS animations

<details>
  <summary>CSS animations</summary>

### CSS animations

**Definition:** **CSS animations** run on the compositor thread when only `transform` and `opacity` change — smooth and decoupled from JS busy work.

**How it works:**

- **`transition`:** animate when a property changes (hover, class toggle).
- **`@keyframes` + `animation`:** multi-step timelines, infinite loops, `animation-direction`.
- **`will-change`** sparingly; prefer `transform` over `top`/`left` for movement.

**When to use:** Loaders, modals, micro-interactions — default choice before writing a JS loop.

**Full topic:** [CSS — Motion](./2_css_motion.md)

**Sources:** [javascript.info/css-animations](https://javascript.info/css-animations) · [MDN — Using CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Animations/Using)

</details>

<a name="javascript-animations"></a>

## JavaScript animations

<details>
  <summary>JavaScript animations</summary>

### JavaScript animations

**Definition:** **JavaScript animations** update layout, attributes, or canvas pixels on a timer — typically **`requestAnimationFrame`** — instead of CSS keyframes.

**How it works:**

1. Record start time and from/to values.
2. Each frame: compute eased progress `t` (see [Bezier curve](#bezier-curve)).
3. Apply `element.style.transform = \`translateX(${x}px)\`` or draw on canvas.
4. Stop when `t >= 1` or cancel rAF on unmount.

**When to use:** Drag follow, spring physics, scrolling parallax tied to input, game loops.

#### Common pitfalls

Animating `width`/`height` every frame (layout thrashing); using `setInterval(16)` instead of rAF; not canceling rAF after navigation.

**Examples:**

```js
function easeOutCubic(t) {
  return 1 - (1 - t) ** 3;
}

function animate({ from, to, duration, onUpdate, onDone }) {
  const start = performance.now();
  function frame(now) {
    const t = Math.min(1, (now - start) / duration);
    onUpdate(from + (to - from) * easeOutCubic(t));
    if (t < 1) requestAnimationFrame(frame);
    else onDone?.();
  }
  requestAnimationFrame(frame);
}
```

**Sources:** [javascript.info/js-animation](https://javascript.info/js-animation) · [MDN — Animation performance](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Animation_performance)

</details>

<a name="requestanimationframe"></a>

## requestAnimationFrame

<details>
  <summary>requestAnimationFrame</summary>

### requestAnimationFrame

**Definition:** **`requestAnimationFrame(callback)`** schedules work **before the next repaint** (~display refresh rate). Paused when the tab is hidden.

**How it works:**

- Callback receives a `DOMHighResTimeStamp` for delta-time animation.
- Chain `requestAnimationFrame` calls to build a loop; `cancelAnimationFrame(id)` on cleanup.
- Unlike `setInterval(fn, 16)`, rAF is tied to paint and does not “catch up” bursts after a hidden tab.

**When to use:** Visual JS animations; read layout then write in the same frame carefully (batch reads/writes).

#### Common pitfalls

Heavy work inside the rAF callback blocks paint; using rAF as a generic delay (use `queueMicrotask` / `setTimeout` instead).

**Examples:**

```js
let id;
function tick(time) {
  ball.style.transform = `translateX(${time % 1000}px)`;
  id = requestAnimationFrame(tick);
}
id = requestAnimationFrame(tick);
// cleanup: cancelAnimationFrame(id);
```

**See also:** [HTML rendering pipeline](./1_html_rendering.md)

**Sources:** [MDN — requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) · [javascript.info/js-animation](https://javascript.info/js-animation)

</details>
