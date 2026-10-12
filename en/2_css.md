# CSS — hub

Cascade, box model, layout, responsive design, motion, architecture, and performance. Theory lives in the topic files below.

### Related docs

[CSS — Core](./2_css_core.md) · [CSS — Layout](./2_css_layout.md) · [CSS — Visual](./2_css_visual.md) · [CSS — Motion](./2_css_motion.md) · [CSS — Modern](./2_css_modern.md) · [CSS — Architecture](./2_css_architecture.md) · [CSS — Interview](./2_css_interview.md)

### Navigation

| Topic                                         | Covers                                                              |
| --------------------------------------------- | ------------------------------------------------------------------- |
| [CSS — Core](./2_css_core.md)                 | How CSS is added, cascade, selectors, and the box model.            |
| [CSS — Layout](./2_css_layout.md)             | Flexbox, grid, positioning, responsive design, and mobile viewport. |
| [CSS — Visual](./2_css_visual.md)             | Typography, color, decoration, form styling, and print.             |
| [CSS — Motion](./2_css_motion.md)             | Transforms, transitions, animations, and visual effects.            |
| [CSS — Modern](./2_css_modern.md)             | 2024+ CSS features and advanced phase-2 topics.                     |
| [CSS — Architecture](./2_css_architecture.md) | Methodologies, tooling, and rendering performance.                  |
| [CSS — Interview](./2_css_interview.md)       | CSS interview questions, answers not filled in.                     |

### Interview practice

Q&A lives in the Interview topic for this subject.

---

## Resources & links

<details>
  <summary>External references</summary>

- [MDN CSS reference](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [web.dev Learn CSS](https://web.dev/learn/css/)
- [CSS-Tricks](https://css-tricks.com/)
- [Flexbox Froggy](https://flexboxfroggy.com/)
- [Grid Garden](https://cssgridgarden.com/)
- [CSS for JS developers](https://learn.javascript.ru/css-for-js)
- [Modern CSS 2025 (Frontend Masters)](https://frontendmasters.com/blog/what-you-need-to-know-about-modern-css-2025-edition/)
- [CodemyUI patterns](https://codemyui.com/)
- [CSS interview questions](https://www.interviewbit.com/css-interview-questions/)
- [Google Fonts](https://fonts.google.com/)
- [caniuse.com](https://caniuse.com/)

**Videos**

- [5 CSS Tips for Responsive Design](https://www.youtube.com/watch?v=2IV08sP9m3U)
- [10 CSS Pro Tips](https://www.youtube.com/watch?v=44FTAS-qT8Q)
- [CSS Animations](https://www.youtube.com/watch?v=UxFnW2cPRi8)

</details>

### Sections

- [Core](./2_css_core.md#core)
- [Selectors & Cascade](./2_css_core.md#selectors-cascade)
- [Box Model & Display](./2_css_core.md#box-model-display)
- [Layout Systems](./2_css_layout.md#layout-systems)
- [Responsive Design](./2_css_layout.md#responsive-design)
- [Scroll, Touch & Mobile](./2_css_layout.md#scroll-touch-mobile)
- [Typography & Colors](./2_css_visual.md#typography-colors)
- [Decoration](./2_css_visual.md#decoration)
- [Form Styling](./2_css_visual.md#form-styling)
- [Print & Paged Media](./2_css_visual.md#print)
- [Transforms & Animations](./2_css_visual.md#transforms-animations)
- [Visual Effects](./2_css_motion.md#visual-effects)
- [Modern CSS](./2_css_motion.md#modern-css)
- [CSS Phase 2](./2_css_modern.md#css-phase-2)
- [Architecture & Tooling](./2_css_architecture.md#architecture-tooling)
- [Performance](./2_css_architecture.md#performance)
- [Interview Practice](#interview-practice)
- [Interview questions](./2_css_interview.md#interview-practice)

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

### Why CSS styles don't apply

1. Stylesheet not linked or wrong path
2. Selector doesn't match the element
3. Overridden by higher specificity or later rule
4. Property invalid or unsupported in browser
5. `display: none` / `visibility: hidden` on parent
6. Typo in property or value
