# JS — Web Components

Custom elements, shadow DOM, templates, slots, styling, and events — browser-native component encapsulation.

[← Back to JS hub](./3_js.md) · [Browser hub](./3_js_browser_hub.md)

### Related docs

- [Document — shadow DOM pointer](./3_js_browser.md#shadow-dom) · [HTML advanced](./1_html_advanced.md)
- [CSS architecture](./2_css_architecture.md) · [JS Modules](./3_js_modules.md)
- [JS Interview](./3_js_interview.md)

### Main concepts

| Area                 | Topics                                           |
| -------------------- | ------------------------------------------------ |
| **Custom elements**  | `customElements.define`, lifecycle callbacks     |
| **Shadow DOM**       | Encapsulated tree, `mode: open` / `closed`       |
| **Composition**      | `<template>`, `<slot>`, light DOM children       |
| **Styling & events** | `::part`, CSS variables, retargeting, `composed` |

### Sections

- [Web Components overview](#web-components-overview)
- [Custom elements](#custom-elements)
- [Shadow DOM](#shadow-dom)
- [Template element](#template-element)
- [Shadow DOM slots and composition](#shadow-dom-slots-and-composition)
- [Shadow DOM styling](#shadow-dom-styling)
- [Shadow DOM and events](#shadow-dom-and-events)

---

<a name="web-components-overview"></a>

## Web Components overview

<details>
  <summary>Web Components overview</summary>

### Web Components overview

**Definition:** **Web Components** are a set of standards for reusable, encapsulated UI: **Custom Elements**, **Shadow DOM**, and **HTML `<template>`**, usually loaded as **ES modules**.

**How it works:**

| Standard        | Role                                        |
| --------------- | ------------------------------------------- |
| Custom Elements | New tag names (`<user-card>`) with JS class |
| Shadow DOM      | Private subtree + scoped CSS                |
| `<template>`    | Inert markup cloned into shadow roots       |
| ES modules      | `import` in `<script type="module">`        |

**When to use:** Design-system primitives, embeddable widgets on third-party pages, framework-agnostic leaf components.

**Pluses and minuses:**

| +                    | −                                         |
| -------------------- | ----------------------------------------- |
| Native encapsulation | SSR/hydration tooling varies by framework |
| Works across stacks  | Closed shadow harder to debug             |

**Sources:** [javascript.info/web-components](https://javascript.info/web-components) · [MDN — Web components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components)

</details>

<a name="custom-elements"></a>

## Custom elements

<details>
  <summary>Custom elements</summary>

### Custom elements

**Definition:** A **custom element** is a class extending `HTMLElement` (or `HTMLElement` subclasses) registered with a **hyphenated** tag name.

**How it works:**

- `customElements.define("my-button", MyButton)` — register once per name.
- **Lifecycle:** `connectedCallback` (inserted), `disconnectedCallback` (removed), `attributeChangedCallback` (observed attrs), `adoptedCallback` (moved documents).
- **Observed attributes:** `static get observedAttributes() { return ["disabled"]; }`
- **Upgrade:** `customElements.whenDefined("my-button")` before querying undefined elements.

**When to use:** Buttons, cards, inputs with stable public tag API.

#### Common pitfalls

Forgotten `customElements.define` guard (throws on double define); not calling `super()` in constructor when required.

**Examples:**

```js
class MyButton extends HTMLElement {
  connectedCallback() {
    if (!this.shadowRoot) {
      this.attachShadow({ mode: "open" });
      this.shadowRoot.innerHTML = `<button><slot></slot></button>`;
    }
  }
}
customElements.define("my-button", MyButton);
```

**Sources:** [javascript.info/custom-elements](https://javascript.info/custom-elements) · [MDN — CustomElementRegistry](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry)

</details>

<a name="shadow-dom"></a>

## Shadow DOM

<details>
  <summary>Shadow DOM</summary>

### Shadow DOM

**Definition:** **Shadow DOM** attaches a private subtree to a **host** element. Light DOM children remain in markup; shadow tree is rendered instead for layout/paint inside the component.

**How it works:**

- `const root = host.attachShadow({ mode: "open" | "closed" })` — `open` exposes `host.shadowRoot`; `closed` hides it from JS (not a security boundary).
- Global `document.querySelector` does **not** see inside shadow trees.
- IDs and selectors are scoped to the shadow root.

**When to use:** Hide internal buttons/labels; prevent global CSS from breaking widget layout.

**See also:** [Document — shadow DOM](./3_js_browser.md#shadow-dom) (short pointer)

**Sources:** [javascript.info/shadow-dom](https://javascript.info/shadow-dom) · [MDN — Using shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM)

</details>

<a name="template-element"></a>

## Template element

<details>
  <summary>Template element</summary>

### Template element

**Definition:** **`<template>`** holds markup that is **inert** in the document — scripts do not run, images do not load until cloned.

**How it works:**

- `const fragment = template.content.cloneNode(true)` — clone into shadow root or light DOM.
- Keeps large component markup out of the constructor string; friendly to SSR snapshots when paired with declarative shadow DOM (advanced).

**When to use:** Repeated shadow subtree; table rows generated from one pattern.

**Examples:**

```html
<template id="row">
  <tr>
    <td></td>
  </tr>
</template>
```

```js
tbody.append(template.content.cloneNode(true));
```

**Sources:** [javascript.info/template-element](https://javascript.info/template-element) · [MDN — template](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/template)

</details>

<a name="shadow-dom-slots-and-composition"></a>

## Shadow DOM slots and composition

<details>
  <summary>Shadow DOM slots and composition</summary>

### Shadow DOM slots and composition

**Definition:** **Slots** project **light DOM** children into designated places in the shadow tree — composition without losing encapsulation.

**How it works:**

- Default slot: `<slot></slot>` — unnamed children map here.
- Named: `<slot name="title"></slot>` ← `<span slot="title">Hello</span>`.
- **`slot` attribute** on light DOM child picks the target.
- **`slotchange` event** on `HTMLSlotElement` when assigned nodes change.
- **`element.assignedSlot`** — which slot claimed a node.

**When to use:** Card title/body/footer regions; icon + label button.

**Examples:**

```html
<user-card>
  <span slot="title">Ada</span>
  <p>Engineer</p>
</user-card>
```

```js
this.shadowRoot.innerHTML = `
  <h2><slot name="title"></slot></h2>
  <div class="body"><slot></slot></div>
`;
```

**Sources:** [javascript.info/slots-composition](https://javascript.info/slots-composition) · [MDN — slot](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/slot)

</details>

<a name="shadow-dom-styling"></a>

## Shadow DOM styling

<details>
  <summary>Shadow DOM styling</summary>

### Shadow DOM styling

**Definition:** Styles inside a shadow root **do not leak out**; global page CSS **does not** style shadow internals (except inherited properties like `color` and `font`).

**How it works:**

- Put component CSS in `<style>` inside shadow root or constructable stylesheets (`adoptedStyleSheets`).
- **Theming from outside:** CSS custom properties inherit into shadow — `host { --accent: hotpink; }` and `button { background: var(--accent); }`.
- **Expose parts:** `part="label"` on internal node → `my-btn::part(label) { … }` from light DOM (requires `exportparts` for nested shadow).
- **`:host`** styles the host element; **`:host(.active)`** when host has class.

**When to use:** Tokens via variables; rare piercing via `::part` instead of `!important` global hacks.

#### Common pitfalls

Expecting Bootstrap classes inside closed shadow to work; forgetting `display` on `:host` for custom tags (often `display: inline-block` or `block`).

**Sources:** [javascript.info/shadow-dom-style](https://javascript.info/shadow-dom-style) · [MDN — ::part](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Selectors/::part)

</details>

<a name="shadow-dom-and-events"></a>

## Shadow DOM and events

<details>
  <summary>Shadow DOM and events</summary>

### Shadow DOM and events

**Definition:** Events that originate inside shadow DOM **retarget** when observed from outside — `event.target` appears as the **host** element, not the inner button.

**How it works:**

- **`event.composedPath()`** — full path including shadow nodes (useful in tests and delegation).
- **`composed: true`** (default for most UI events) — event crosses shadow boundary to document listeners.
- **`composed: false`** — e.g. some `focus` events stay inside shadow; attach listeners on `shadowRoot` when needed.
- Listen on `shadowRoot` with `{ capture: true }` for internal-only handling before retargeting.

**When to use:** Click on `<my-button>` — document sees host; inner `<button>` still receives event inside shadow.

**Examples:**

```js
host.addEventListener("click", (e) => {
  console.log(e.target === host); // often true from outside
  console.log(e.composedPath()); // includes inner button
});
```

**Sources:** [javascript.info/shadow-dom-events](https://javascript.info/shadow-dom-events) · [MDN — Event.composedPath](https://developer.mozilla.org/en-US/docs/Web/API/Event/composedPath)

</details>
