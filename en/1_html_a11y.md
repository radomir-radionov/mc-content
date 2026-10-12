# HTML — Accessibility

POUR, names, landmarks, forms, and images.

[← Back to hub](./1_html.md)

### Related docs

- [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md) · [UX & UI principles](./7_special_ux.md#ux-and-ui-principles)

### Sections

- [Accessibility](#accessibility)

---

## Accessibility

Theory: [overview](#accessibility-basics) · [tree](#accessibility-tree) · [name/description](#accessible-name-and-description) · [HTML basics](#accessible-html-basics) · [`lang`](#language-of-the-page-and-fragments) · [forms](#accessible-forms) · [hiding](#hiding-content-from-at-vs-visually) · [images](#image-text-alternatives) · [ARIA](#aria) · [screen readers](#screen-readers)

<details>
  <summary>Web accessibility overview</summary>

### Web accessibility overview

**Definition:**

Web accessibility (a11y) means people with disabilities can perceive, understand, navigate, and interact with the web. Foundation: WCAG (POUR).

| Principle      | Meaning                       |
| -------------- | ----------------------------- |
| Perceivable    | Content available to senses   |
| Operable       | Keyboard and input accessible |
| Understandable | Clear language and behavior   |
| Robust         | Works with assistive tech     |

**How it works:**

- WCAG organized as POUR: Perceivable, Operable, Understandable, Robust.
- HTML is the first layer — semantics, labels, alt text.
- Test with keyboard + screen reader + automated tools.

| Principle   | Meaning            |
| ----------- | ------------------ |
| Perceivable | Alt text, contrast |
| Operable    | Keyboard access    |

**When to use:**

Public sites (legal requirements) · internal tools · component libraries.

#### Common pitfalls

- ARIA-only fixes without native HTML.
- Color-only error states.
- Removing focus outlines without replacement.

**Examples:**

```html
<html lang="en">
  <body>
    <a href="#main">Skip to content</a>
    <main id="main"><h1>Accessible page</h1></main>
  </body>
</html>
```

**Sources:** [MDN — Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility)

</details>

<details>
  <summary>Semantic HTML first</summary>

### Semantic HTML first

**Definition:**

Prefer native HTML elements over ARIA when possible. The first rule of ARIA: don't use ARIA if a native element exists.

| Need          | Use                  | Avoid                       |
| ------------- | -------------------- | --------------------------- |
| Button action | `<button>`           | `<div onclick>`             |
| Navigation    | `<nav>` + `<a href>` | `<div>` with click handlers |
| Modal         | `<dialog>`           | `<div role="dialog">`       |

**How it works:**

- First rule of ARIA: don't use ARIA if native element exists.
- `<button>`, `<nav>`, `<dialog>`, `<details>` before `role=` equivalents.
- Add ARIA only for gaps native HTML cannot fill.

| Need   | Use             | Avoid           |
| ------ | --------------- | --------------- |
| Action | `<button>`      | `<div onclick>` |
| Nav    | `<nav>` + `<a>` | div soup        |

**When to use:**

Custom tabs (ARIA) vs native `<details>` · modal (`<dialog>`).

#### Common pitfalls

- `role="button"` on divs.
- Redundant `role="navigation"` on `<nav>`.
- ARIA overriding native semantics incorrectly.

**Examples:**

```html
<nav aria-label="Main"><a href="/">Home</a></nav>
<button type="button">Open settings</button>
<details>
  <summary>More info</summary>
  <p>Details here.</p>
</details>
```

</details>

**Sources:**

[MDN — ARIA — don't overuse](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Guides)

</details>

<details>
  <summary>Key accessibility checklist</summary>

### Key accessibility checklist

**Definition:**

A practical checklist for HTML accessibility covering semantics, contrast, labels, headings, and keyboard support.

| Check             | Implementation                            |
| ----------------- | ----------------------------------------- |
| Semantic elements | `button` vs `a`, landmarks                |
| Image alt text    | Meaningful `alt`; `alt=""` for decorative |
| Form labels       | `<label for>` on every control            |
| Heading hierarchy | Logical `h1`–`h6` outline                 |
| Keyboard          | All interactive elements focusable        |

**How it works:**

- Landmarks, headings, labels, alt text, keyboard, focus, contrast.
- Forms: label every control; announce errors.
- Dynamic updates: `aria-live` regions.

| Check  | Implementation   |
| ------ | ---------------- |
| Labels | `<label for>`    |
| Images | Meaningful `alt` |

**When to use:**

Pre-release audit · PR review checklist · VPAT documentation.

#### Common pitfalls

- Automated tools only (miss ~30–50% of issues).
- Skip links missing on content-heavy pages.
- Icon buttons without accessible name.

**Examples:**

```html
<label for="search">Search</label>
<input id="search" type="search" name="q" />
<img src="decorative.svg" alt="" />
```

**Sources:** [WCAG Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/)

</details>

<details>
  <summary>ARIA</summary>

### ARIA

**Definition:**

ARIA (Accessible Rich Internet Applications) adds semantics when HTML alone is insufficient — tabs, live regions, expanded states.

| Pattern           | ARIA                          |
| ----------------- | ----------------------------- |
| Live updates      | `aria-live="polite"`          |
| Expanded state    | `aria-expanded="true/false"`  |
| Described by      | `aria-describedby="error-id"` |
| Hidden decorative | `aria-hidden="true"`          |

**How it works:**

- ARIA adds roles, states, properties when HTML insufficient.
- `aria-expanded`, `aria-selected`, `aria-live` for dynamic UI.
- Never change native role (no `role="button"` on `<a href>`).

| Pattern          | ARIA                 |
| ---------------- | -------------------- |
| Live updates     | `aria-live="polite"` |
| Described errors | `aria-describedby`   |

**When to use:**

Tab panels · live search results · custom sliders.

#### Common pitfalls

- `aria-label` duplicating visible text incorrectly.
- Missing `aria-controls` on tabs.
- `aria-hidden="true"` on focusable elements.

**Examples:**

```html
<div role="tablist">
  <button role="tab" aria-selected="true" aria-controls="panel-1">Tab 1</button>
</div>
<div id="panel-1" role="tabpanel">Panel content</div>
<div aria-live="polite" id="status"></div>
```

**Sources:** [MDN — ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)

</details>

<details>
  <summary>Screen readers</summary>

### Screen readers

**Definition:**

A screen reader converts on-screen content to speech or braille. Test with VoiceOver, NVDA, JAWS, or TalkBack.

| Test area | What to verify             |
| --------- | -------------------------- |
| Headings  | Logical outline navigation |
| Landmarks | `main`, `nav`, `header`    |
| Forms     | Labels and error messages  |
| Images    | Meaningful alt text        |

**How it works:**

- Convert UI to speech/braille (VoiceOver, NVDA, JAWS).
- Navigate by headings, landmarks, links, forms mode.
- Test real flows, not just page load.

| Test     | Verify           |
| -------- | ---------------- |
| Headings | Logical outline  |
| Forms    | Labels announced |

**When to use:**

Form submission flow · modal open/close · dynamic list updates.

#### Common pitfalls

- `display:none` content still in DOM but not always read (varies).
- Unlabeled icon buttons.
- Reading order ≠ visual order (CSS reorder).

**Examples:**

```html
<main>
  <h1>Article title</h1>
  <p>Content read aloud by assistive technology.</p>
</main>
```

**Sources:** [MDN — Screen readers](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Guides/screen_reader_support)

</details>

<details>
  <summary>Accessibility basics</summary>

### Accessibility basics

**Definition:** Accessibility is designing environments, products, and services so people with disabilities can use them. **Web accessibility** is that idea applied to sites, apps, and related tools. The numeronym **a11y** is _accessibility_ (_a_ + 11 letters + _y_).

**How it works:**

- **Social model:** disability appears when people meet **barriers** (physical, digital, informational) — not only a medical diagnosis.
- **Curb-cut effect:** ramps, captions, and autocomplete help people with disabilities first, then everyone (broken arm, noisy café, slow network). A useful side effect, not the main goal.
- Related but not identical: **universal design** (one solution for as many people as possible), **inclusive design** (diversity beyond disability), **usability** (efficiency/satisfaction — often tested without disabled users).
- **Digital accessibility** is broader (native apps, PDFs, OS). Web a11y is the web slice.
- W3C: the web is for everyone, regardless of device, language, location, or ability.
- Layers: user agents (browsers), assistive tech (screen readers, magnifiers), content (HTML/CSS/JS), authoring tools, people.
- **POUR** (WCAG): Perceivable, Operable, Understandable, Robust.
- Users: motor, neurological, cognitive, visual, auditory, speech — often combined; also temporary/situational limits.
- Specs: HTML/CSS plus **WAI-ARIA**, **WCAG**, **ATAG**, **UAAG**. Prefer native HTML; ARIA fills gaps (tabs, live regions).
- Why teams invest: law, ethics, reach, SEO, reputation, innovation.
- Overlays that “fix” a11y on top of broken HTML usually fail and can add barriers. WCAG conformance is not a substitute for testing with people.

**When to use:**

Public sites, checkout, design systems. Not: treating a11y as a last-sprint overlay.

#### Common pitfalls

Equating a11y with “screen readers only”; separate “blind versions” of a site (old GOST idea — WCAG 2.1+ prefers one flexible site); postponing a11y to QA.

**Pluses and minuses:**

| +                                                                 | −                                                               |
| ----------------------------------------------------------------- | --------------------------------------------------------------- |
| Legal and ethical baseline; helps more users than “disabled only” | Needs time, training, and ongoing checks as the product changes |
| Semantic HTML covers most cases                                   | Hard to measure AT users via analytics (OS Accessibility APIs)  |

**Thoughts:** _Barriers are in the product; POUR + native HTML is the default, ARIA is the exception._

**Sources:** [Doka — What is accessibility](https://doka.guide/a11y/a11y-overview/) · [W3C WAI](https://www.w3.org/WAI/) · [MDN — Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

</details>

<details>
  <summary>Accessibility tree</summary>

### Accessibility tree

**Definition:** The **accessibility tree** is a browser-built tree of **accessible objects** derived from the DOM (plus CSS and ARIA). Assistive tech does not read the DOM directly — it uses the OS **Accessibility API**, which is fed from this tree.

**How it works:**

- Built alongside the DOM and **updated when the DOM/styles change** (not a one-shot after first paint).
- Each object may have **role**, **name**, **description**, **states/properties**.
- Semantic HTML maps to implicit roles (`<a href>` → `link`, `<button>` → `button`, `<div>`/`<span>` → `generic`).
- Usually **omitted** from the tree: `display: none`, HTML `hidden`, `aria-hidden="true"`, and many empty generic wrappers. `visibility: hidden` is also treated as not shown to AT. Do **not** add redundant `aria-hidden` on already-unrendered nodes.
- **In the tree but visually hidden:** `.sr-only` / clip techniques (see hiding content).
- `::before` / `::after` `content` can contribute to names in some engines.
- Trees **differ by browser** (role names, name computation). DevTools show a **preview**, not what NVDA/VoiceOver actually speak — and they lag on live updates.
- **AOM** (Accessibility Object Model) is experimental; there is no stable JS API to read/write the tree in production.

**When to use:**

Debugging “why isn’t this announced?”; custom widgets; interviews.

#### Common pitfalls

Trusting DevTools only; `role="link"` on a `<span>` without keyboard/activation; assuming decorative `<div>`s always appear.

**Pluses and minuses:**

| +                  | −                                      |
| ------------------ | -------------------------------------- |
| Explains AT output | DevTools ≠ real SR; engine differences |

**Examples:**

```html
<main>
  <h1>Products</h1>
  <label for="addr">Address</label>
  <input id="addr" type="text" aria-describedby="hint" />
  <span id="hint">Street and house number</span>
  <button type="submit">Buy</button>
</main>
```

Chrome Accessibility pane (typical): `main`; `heading` level 1 named “Products”; `textbox` named “Address”, described by the hint; `button` “Buy”, focusable.

**Thoughts:** _AT sees a pruned semantic tree, not your CSS layout._

**Sources:** [MDN — Accessibility tree](https://developer.mozilla.org/en-US/docs/Glossary/Accessibility_tree) · [Doka — Accessibility tree](https://doka.guide/a11y/a11y-tree/) · [Chrome a11y DevTools](https://developer.chrome.com/docs/devtools/accessibility/reference)

</details>

<details>
  <summary>Accessible name and description</summary>

### Accessible name and description

**Definition:** The **accessible name** is the short label AT uses for an object (what it is / does). The **accessible description** is optional extra help, usually announced after name, role, and state.

**How it works:**

- Names are **required** for many interactive roles (see WAI-ARIA “name required”). Not required for `<p>` or `<time>`.
- **Visible names** come from content (`<a>`, `<button>`, headings). **Invisible** names include `alt` and `aria-label`.
- Native name sources: element contents; `<label>`; `<legend>` / `<caption>` / `<figcaption>`; `alt` on `<img>` / `<area>` / `<input type="image">`; submit/reset `value`.
- ARIA: `aria-labelledby` (highest name precedence — **overrides** visible text and `<label>`), then `aria-label`. Description: `aria-describedby` (can list several ids), then `aria-description` (ARIA 1.3 — **prefer `aria-describedby`**).
- Fallback names: `title` and `placeholder` — unreliable; never the only name.
- AccName algorithm (high → low): `aria-labelledby` → `aria-label` → native (`label` / `alt` / `value` / caption / figcaption / contents) → `title` / `placeholder`.
- Description algorithm: `aria-describedby` → `aria-description` → native caption-like text → `title`.
- Naming rules: short; visible when possible; don’t repeat the role (“Submit button”); put the verb/destination first; keep names stable across pages; update names when UI changes; translate names and ARIA strings.

**When to use:**

Icon buttons, inputs, tables, figures, custom widgets.

#### Common pitfalls

`placeholder` as the label; `aria-label` that disagrees with visible text (WCAG 2.5.3 Label in Name); empty `<button>`; claiming a table without `<caption>` is “decorative” — it is still a `table`; it just lacks a strong accessible name.

**Pluses and minuses:**

| +                                               | −                                                |
| ----------------------------------------------- | ------------------------------------------------ |
| Native HTML names stay translatable and visible | `aria-labelledby` silently replaces visible text |

**Examples:**

```html
<label for="email">Email</label>
<input
  id="email"
  type="email"
  autocomplete="email"
  aria-describedby="email-hint"
/>
<span id="email-hint">Use your work address.</span>

<button aria-label="Close">×</button>
```

**Thoughts:** _Name = purpose; description = extra; ARIA name wins over HTML._

**Sources:** [AccName](https://www.w3.org/TR/accname-1.2/) · [MDN — aria-labelledby](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) · [Doka — Accessible name](https://doka.guide/a11y/accessible-name-and-description/)

</details>

<details>
  <summary>Accessible HTML basics</summary>

### Accessible HTML basics

**Definition:** Most a11y comes from **correct semantics and content**, not from ARIA. Native controls already expose role, keyboard behavior, and form participation.

**How it works:**

- `<button>` is focusable, activatable with Enter/Space, and has role `button`. A `<div>` needs `tabindex`, role, key handlers, and focus styles — easy to get wrong.
- Screen readers jump by headings, landmarks, lists, tables, forms — those need real tags, not styled `<div>`s.
- First rule of ARIA: **don’t use ARIA** if a native element exists. Wrong `role` **replaces** native semantics (`<a role="menuitem">` is no longer a link).
- Not every native control is equally supported (`<details>`/`<summary>` still buggy in some SR + browser pairs; nested content inside `<label>` can be skipped in Safari VoiceOver). Always test.
- `div`/`span` have role `generic` (usually not announced). `role` on `<ul>` can drop list semantics.
- Link text must make sense **out of context** (no “click here”). Expand abbreviations in text; `<abbr title>` is not keyboard/touch friendly.
- Document setup: `<html lang>`, unique `<title>`, allow zoom (`user-scalable` must not be `no`/`0`; avoid `maximum-scale` &lt; 2 — WCAG 1.4.4 resize to 200%).
- One `<h1>` and no skipped levels is a strong authoring practice (MDN); don’t pick heading level for visual size.
- Debug: Chrome/Firefox accessibility inspector, axe, Lighthouse, [W3C HTML checker](https://validator.w3.org/). Keyboard must be manual.

**When to use:**

Every page. Custom tabs/dialogs only when HTML has no equivalent — follow [ARIA APG](https://www.w3.org/WAI/ARIA/apg/).

#### Common pitfalls

`placeholder` instead of `<label>` (majority of sites historically); icon-only controls without a name; heading soup.

**Thoughts:** _Semantics + honest text first; ARIA last; tools don’t replace keyboard + SR._

**Sources:**

[Doka — Accessible HTML](https://doka.guide/a11y/html/) · [ARIA APG](https://www.w3.org/WAI/ARIA/apg/) · [MDN — HTML: A good basis for accessibility](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Accessibility/HTML)

</details>

<details>
  <summary>Language of the page and fragments</summary>

### Language of the page and fragments

**Definition:** `lang` tells user agents and screen readers which **BCP 47** language to use for speech, fonts, hyphenation, quotes, and translation. WCAG 3.1.1 requires the page language; 3.1.2 covers parts in another language.

**How it works:**

- Set the document language on `<html lang="ru">` (or `en`, `en-GB`, …). Prefer **short** tags; add region only when it matters (spelling, TTS).
- Mark foreign phrases: `<span lang="sv">Lagom</span>`. CSS: `:lang(sv)`.
- If `lang` is missing, SRs often use the **OS / user** voice — unintelligible for another language.
- `lang="zxx"` = no linguistic content (code, Lorem ipsum).
- RTL scripts: `dir="rtl"` (`ltr` is default; `auto` lets the browser guess).
- Multilingual sites: translate visible text **and** `title`, `alt`, `placeholder`, `label`, `aria-label`, `aria-placeholder`, `aria-roledescription`, `aria-valuetext`, and CSS `content` strings.
- Alternate language URLs: `<link rel="alternate" hreflang="fr">` (SEO) and/or `<a rel="alternate" hreflang="fr">` so AT knows the target language.

**When to use:**

Mixed-language articles, i18n apps, Hebrew/Arabic UI. Full i18n/SEO: [Internationalization](./1_html_seo.md#internationalization-i18n).

#### Common pitfalls

Styling language with CSS classes only; leaving `aria-label` untranslated; `hreflang` without matching `lang` on the target page.

**Examples:**

```html
<html lang="en">
  <p>The Swedish word <span lang="sv">lagom</span> means “just enough.”</p>
  <p lang="he" dir="rtl">לחיים</p>
</html>
```

**Thoughts:** _Wrong `lang` = wrong voice; mark the page and every foreign snippet._

**Sources:** [MDN — lang](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang) · [Doka — Page language](https://doka.guide/a11y/language/) · [Language subtag lookup](https://r12a.github.io/app-subtags/)

</details>

<details>
  <summary>Accessible forms</summary>

### Accessible forms

**Definition:** Accessible forms use native controls, a visible `<label>` per field, predictable focus, format hints, and errors that are not color-only.

**How it works:**

- **Label:** `<label for="id">` (explicit) is more reliable than wrapping the control (implicit). Clicking the label focuses the control. Don’t use `title`/`placeholder` as the only name.
- Native `<input>`/`<select>`/`<textarea>`: keyboard, `name` in submit, `inputmode`/`type` for mobile keyboards. A `contenteditable` `<div>` is not a form control.
- Focus: visible `:focus-visible`; DOM order = tab order (avoid CSS `order`). Don’t auto-advance focus between fields. Split long forms into steps.
- Hints: visible text + `aria-describedby`. Instructions before the form; don’t validate on first keystroke — prefer submit (or blur) then errors.
- Errors: nearby text; `aria-invalid="true"`; link errors with `aria-describedby` (`aria-errormessage` support is weak). Announce with `role="alert"` or `aria-live="assertive"`; move focus to the first invalid field. Success message after submit.
- Types: `email` + `autocomplete="email"`; `password` + `current-password` / `new-password`; radios/checkboxes in `<fieldset>` + `<legend>` (skip fieldset for a single checkbox). Don’t nest fieldsets. Don’t rely on `<optgroup>` as the only grouping cue.
- Buttons: `<button type="submit">` with text (or `value` on `<input type="submit">`). Avoid `type="reset"`. A submit control inside `<form>` enables Enter-to-submit.
- Useful attrs: `required`, `disabled`, `readonly`, `maxlength`, `pattern`, `autocomplete` tokens. Don’t set `autocomplete="off"` without a strong reason.
- One visible `<label>` per control; extra names via `aria-labelledby` / `aria-label` (they **override** `<label>`). `aria-describedby` **adds** hint text — keep the label.
- Search boxes: visually hidden label (`.visually-hidden`) **or** `aria-label` — not both plus `title`.

**When to use:**

Login, checkout, filters. Not: reinventing checkbox with `<div>`.

#### Common pitfalls

`<div for>` (invalid); color-only required markers; sound-only errors; tiny hit targets; clearing field data immediately (except sensitive financial flows).

**Examples:**

```html
<form>
  <label for="email">Email</label>
  <input
    id="email"
    name="email"
    type="email"
    autocomplete="email"
    required
    aria-describedby="email-hint email-err"
    aria-invalid="true"
  />
  <p id="email-hint">Work address.</p>
  <p id="email-err" role="alert">Enter a valid email.</p>
  <button type="submit">Save</button>
</form>
```

**Thoughts:** _Native control + visible label + described errors; ARIA only to wire hints._

**Sources:** [MDN — How to structure a web form](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/How_to_structure_a_web_form) · [Doka — Accessible forms](https://doka.guide/a11y/forms/) · [MDN — aria-invalid](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-invalid)

</details>

<details>
  <summary>Hiding content from AT vs visually</summary>

### Hiding content from AT vs visually

**Definition:** “Hide” means three different things: hide from **everyone**, hide from **AT only**, or hide **visually** but keep in the accessibility tree.

**How it works:**

| Goal                           | Technique                             | Visual                       | AT                                                          |
| ------------------------------ | ------------------------------------- | ---------------------------- | ----------------------------------------------------------- |
| Hide from all                  | `hidden`, `display: none`             | Gone, no box                 | Out of tree                                                 |
| Hide from all, keep layout     | `visibility: hidden`                  | Invisible, still takes space | Treated as hidden                                           |
| Hide from AT, stay on screen   | `aria-hidden="true"`                  | Visible                      | Out of tree (descendants too)                               |
| Hide visually, keep for AT     | `.sr-only` / `.visually-hidden`       | Off-screen 1×1 clip          | Announced                                                   |
| Drop native role, keep content | `role="none"` / `role="presentation"` | Unchanged                    | Semantics removed; **focusable controls stay in tab order** |

```css
.sr-only {
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}
```

- `clip` is deprecated; keep it only as a fallback next to `clip-path`.
- `width: 0` / `height: 0` often **removes** content from AT — don’t use that for SR-only text.
- Decorative SVG inside a named button: `aria-hidden="true"` on the SVG so the path data is not read.
- Don’t put `aria-hidden="true"` or `role="presentation"` on **visible, focusable** controls.

**When to use:**

Icon+text buttons; skip-link off-screen until focus; closed dialog (`hidden` / `inert`).

#### Common pitfalls

`aria-hidden` on a parent of a focused button; combining hidden `<label>` + `aria-label` + `title` (triple announcement).

**Examples:**

```html
<button type="submit">
  <span class="sr-only">Search the page</span>
  <svg aria-hidden="true" width="24" height="24">…</svg>
</button>
<p hidden>Not shown to anyone.</p>
```

**Thoughts:** _`display:none`/`hidden` = nobody; `aria-hidden` = sighted only; `.sr-only` = AT only._

**Sources:** [MDN — aria-hidden](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden) · [MDN — hidden](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/hidden) · [Doka — Hiding content](https://doka.guide/a11y/hiding-content/)

</details>

<details>
  <summary>Image text alternatives</summary>

### Image text alternatives

**Definition:** Meaningful images need a **short alternative** (`alt` on `<img>`, `<title>` on inline SVG). Optional **caption** (`<figcaption>`) and **long description** (`<p>` + `aria-describedby` / `aria-details`) add what `alt` cannot hold.

**How it works:**

- Ask: does the image change meaning if removed? Yes → informative. No → decorative (`alt=""`, never omit `alt` — SRs may read the file path; don’t use a space).
- Don’t start `alt` with “image/photo” — the role is already `img`. Context decides the same photo’s `alt`. Color can matter; describe it when it carries meaning.
- **Informative:** describe the content. **Text-in-image:** put the words in `alt`, or better as real text. **Complex** (charts, screenshots): short `alt` + visible long text; wire with `aria-describedby`. `aria-details` can point at a longer page — **weak SR support**.
- **Groups:** `<figure>` + shared `<figcaption>`; each `<img>` still has its own `alt`. Caption and `alt` must **not** copy each other.
- **Functional** (logo in a link, icon button): `alt` / accessible name = **destination or action** (“Home”, “Doka”), not “logo” or a picture of the icon. If the link already has visible text, use `alt=""`.
- Inline **SVG:** `role="img"` + `<title>` (and `aria-labelledby` on `<title>`/`<desc>` for reliability). Decorative SVG: no `<title>`, `aria-hidden="true"`. CSS `background-image` is invisible to AT.
- Emoji: OS names vary; wrap with `role="img"` + `aria-label` only if the name must be exact.
- AI image description in SRs is a fallback — authors still own `alt`.

**When to use:**

Content photos, charts, logos, icon buttons, decorative flourishes.

#### Common pitfalls

Missing `alt`; empty `alt` on informative images; repeating `figcaption` in `alt`; putting URLs/emoji in `alt`; describing a logo instead of the link target.

**Examples:**

```html
<img
  src="chart.png"
  alt="Q3 revenue rose 12%. Details after the figure."
  aria-describedby="chart-desc"
/>
<p id="chart-desc">…</p>

<a href="/"><img src="logo.svg" alt="Home" /></a>
<a href="https://example.com"><img src="logo.svg" alt="" /> Example</a>
<img src="ornament.png" alt="" />

<svg role="img" viewBox="0 0 100 100" aria-labelledby="dog-title">
  <title id="dog-title">Green dog mascot wearing an a11y collar</title>
</svg>
```

**Thoughts:** _Decorative = `alt=""`; content = describe meaning; controls = name the action._

**Sources:** [MDN — alt](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/alt) · [WAI — Images tutorial](https://www.w3.org/WAI/tutorials/images/) · [Doka — Describing images](https://doka.guide/a11y/images/) · [MDN — SVG in HTML](https://developer.mozilla.org/en-US/docs/Web/SVG/Guides/SVG_in_HTML)

</details>

---

<a name="seo--structured-data"></a>
