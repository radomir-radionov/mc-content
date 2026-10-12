# HTML — Forms

Form controls, validation, dialog, and accessible form patterns.

Landmarks and heading meaning live in [HTML — Semantics](./1_html_semantics.md).

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [Forms & validation](#forms-validation)

---

## Forms & validation

<details>
  <summary>Form structure</summary>

### Form structure

**Definition:**

A form wraps controls that collect user input and submit data to a server. Every submitting control needs a `name` attribute.

| Element | Role |
|---------|------|
| `<form>` | Container with `action`, `method` |
| `<fieldset>` / `<legend>` | Group related controls |
| `<label for>` | Associates label with control |
| `<button type="submit">` | Submits form |

**How it works:**

- `<form action method enctype>` defines submission target.
- Controls need `name` to appear in submitted data.
- `<fieldset>`/`<legend>` group related fields.
- `<label for>` associates text with control id.

| Element | Role |
|---------|------|
| `<form>` | Submission container |
| `<label for>` | Accessible name |
| `<button type="submit">` | Submit |

**When to use:**

Login · multi-step wizard · search (`method="get"`).

#### Common pitfalls

- Inputs without `name` (not submitted).
- Placeholder-only labels.
- Nested forms (invalid).

**Examples:**

```html
<form action="/api/login" method="post" novalidate>
  <fieldset>
    <legend>Account</legend>
    <label for="email">Email</label>
    <input id="email" name="email" type="email" required autocomplete="email" />
    <label for="password">Password</label>
    <input id="password" name="password" type="password" required minlength="8" />
    <button type="submit">Sign in</button>
  </fieldset>
</form>
```

**Thoughts:** FormData and JS submission → [3_js_browser_extra.md](./3_js_browser_extra.md#formdata-file-api-filereader).

**Sources:** [MDN — Forms](https://developer.mozilla.org/en-US/docs/Learn/Forms)

</details>

<details>
  <summary>Input types</summary>

### Input types

**Definition:** The `type` attribute on `<input>` selects the control behavior, validation rules, and mobile keyboard.

**How it works:**

- `type` selects control UI, validation rules, and mobile keyboard.
- Browser provides built-in validation for `email`, `url`, `number`, etc.
- `required`, `pattern`, `min`/`max` add constraints.

| `type` | Use |
|--------|-----|
| `text`, `email`, `password` | Text variants |
| `number`, `range` | Numeric input |
| `checkbox`, `radio` | Boolean / single choice |
| `date`, `file` | Date picker / file upload |

**When to use:**

Email signup · quantity stepper · file upload with `accept`.

#### Common pitfalls

- `type="number"` for credit cards (use `text` + `inputmode`).
- Missing `name` on inputs.
- `placeholder` as only label.

**Pluses and minuses:**

| + | − |
|------|------|
| Native validation and mobile keyboards | Limited styling vs custom widgets |
| Accessible by default | `type="number"` quirks for some formats |

**Examples:**

```html
<input type="email" name="email" required />
<input type="number" name="qty" min="1" max="99" />
<input type="file" name="avatar" accept="image/png,image/jpeg" />
```

**Sources:** [MDN — input types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)

</details>

<details>
  <summary>select, textarea, and button types</summary>

### select, textarea, and button types

**Definition:** Beyond `<input>`, forms use `<select>` for option lists, `<textarea>` for multi-line text, and `<button>` with explicit `type` for actions.

**How it works:**

- `<select name>` submits selected `<option value>`; `multiple` allows multi-select (submits as array in JS FormData).
- `<textarea name>` value is **text content** between tags, not a `value` attribute.
- `<button type="submit">` (default inside form), `type="reset"`, or `type="button"` for non-submit actions.

| Control | Key attrs |
|---------|----------|
| `<select>` | `name`, `multiple`, `required` |
| `<textarea>` | `name`, `rows`, `maxlength` |
| `<button>` | `type`, `formaction` |

**When to use:**

Country picker · comment field · cancel button (`type="button"`) alongside submit.

#### Common pitfalls

- `<button>` without `type` accidentally submits parent form.
- Putting textarea content in `value` attribute (ignored).
- Empty placeholder option without accessible label on required `<select>`.

**Pluses and minuses:**

| + | − |
|------|------|
| Native keyboard/accessibility | Limited styling vs custom dropdowns |
| No JS required for basic submit | `<button>` defaults to submit in forms |

**Examples:**

```html
<label for="country">Country</label>
<select id="country" name="country" required>
  <option value="">Choose…</option>
  <option value="de">Germany</option>
</select>

<label for="bio">Bio</label>
<textarea id="bio" name="bio" rows="4" maxlength="500"></textarea>

<button type="submit">Save</button>
<button type="button">Cancel</button>
```

**Sources:** [MDN — select](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/select) · [textarea](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea) · [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)

</details>

<details>
  <summary>Constraint Validation API</summary>

### Constraint Validation API

**Definition:**

HTML5 provides built-in client-side validation via attributes and the Constraint Validation API in JavaScript.

| Attribute / API | Behavior |
|-----------------|----------|
| `required` | Must have a value |
| `pattern` | Regex match |
| `checkValidity()` | Returns boolean |
| `setCustomValidity()` | Custom error message |

**How it works:**

- HTML attributes (`required`, `pattern`, `min`, `max`) set constraints.
- JS: `checkValidity()`, `reportValidity()`, `setCustomValidity(msg)`.
- `:valid`/`:invalid` pseudo-classes reflect state.

| API / attr | Behavior |
|------------|----------|
| `required` | Non-empty |
| `setCustomValidity` | Custom message |
| `reportValidity` | Show native UI |

**When to use:**

Client-side format checks · custom async username check · `novalidate` + JS-only validation.

#### Common pitfalls

- Client validation only (must re-validate server-side).
- `setCustomValidity('')` to clear — empty string means valid.
- `novalidate` forgotten when relying on custom UI.

**Examples:**

```html
<input type="text" name="code" pattern="[A-Z]{2}-\d{4}" title="Format: AB-1234" required />
```

**Thoughts:** Async validation with JS → [3_js_browser.md](./3_js_browser.md).

**Sources:** [MDN — Constraint validation](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation)

</details>

<details>
  <summary>Form submission attributes</summary>

### Form submission attributes

**Definition:**

Submit buttons can override form defaults with `formaction`, `formmethod`, `formenctype`, and `formnovalidate`.

| Attribute | Overrides |
|-----------|-----------|
| `formaction` | `action` URL |
| `formmethod` | `method` (get/post) |
| `formenctype` | `enctype` |
| `formnovalidate` | Skip validation for this submit |

**How it works:**

- Submit buttons can override form defaults per-button.
- `formaction`/`formmethod`/`formenctype`/`formnovalidate` on submit buttons.
- `form` attribute associates control with distant `<form id>`.

| Attribute | Overrides |
|-----------|----------|
| `formaction` | `action` URL |
| `formnovalidate` | Skip validation |

**When to use:**

Save draft vs publish · delete with `formmethod="post"` · button outside form via `form="id"`.

#### Common pitfalls

- Multiple submit buttons without distinct `formaction`.
- `GET` forms leaking sensitive data in URL.
- Missing `enctype="multipart/form-data"` for file uploads.

**Examples:**

```html
<form action="/publish" method="post">
  <button type="submit">Publish</button>
  <button type="submit" formaction="/save-draft" formnovalidate>Save draft</button>
</form>
```

**Sources:** [MDN — button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)

</details>

<details>
  <summary>dialog element (native modal)</summary>

### dialog element (native modal)

**Definition:**

The `<dialog>` element provides a native modal with backdrop, focus trap, and top-layer rendering — no z-index fights.

| Method | Behavior |
|--------|----------|
| `showModal()` | Modal with backdrop |
| `show()` | Non-modal |
| `close(returnValue)` | Closes dialog |

**How it works:**

- `<dialog>` renders in top layer when `showModal()` called.
- Backdrop click and Escape close (configurable).
- Focus trap and inert background handled by browser.

| Method | Behavior |
|--------|----------|
| `showModal()` | Modal + backdrop |
| `close()` | Returns `returnValue` |

**When to use:**

Confirm delete · login modal · non-modal `show()` panels.

#### Common pitfalls

- Opening without focus management for custom content.
- Stacking multiple modals without UX plan.
- Replacing with `<div role="dialog">` when native works.

**Examples:**

```html
<dialog id="confirm">
  <p>Delete this item?</p>
  <button type="button" onclick="confirm.close()">Cancel</button>
  <button type="button" onclick="confirm.close('ok')">Delete</button>
</dialog>
<button type="button" onclick="confirm.showModal()">Open</button>
```

**Sources:** [MDN — dialog](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)

</details>

<details>
  <summary>Accessible form patterns</summary>

### Accessible form patterns

**Definition:**

Accessible forms associate every control with a label, group related fields, and announce errors clearly.

| Pattern | Implementation |
|---------|----------------|
| Label association | `<label for="id">` |
| Grouping | `<fieldset>` + `<legend>` |
| Error announcement | `aria-invalid` + `aria-describedby` |
| Mobile keyboard | `inputmode`, `enterkeyhint` |

**How it works:**

- Every control has visible `<label for>` or wrapping label.
- Errors: `aria-invalid="true"` + `aria-describedby` pointing to error text.
- Group radios/checkboxes in `<fieldset>` with `<legend>`.

| Pattern | Implementation |
|---------|----------------|
| Label | `<label for="id">` |
| Errors | `aria-describedby` + `role="alert"` |

**When to use:**

Registration form · inline validation · multi-field address group.

#### Common pitfalls

- `aria-label` hiding visible label redundancy incorrectly.
- Error only by color (no text).
- `autocomplete` off on login fields (hurts password managers).

**Examples:**

```html
<label for="phone">Phone</label>
<input id="phone" name="phone" type="tel" inputmode="tel" autocomplete="tel" aria-describedby="phone-hint" />
<span id="phone-hint">Include country code.</span>
```

**Sources:** [MDN — Accessible forms](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)

</details>

<details>
  <summary>autocomplete attribute reference</summary>

### autocomplete attribute reference

**Definition:** The `autocomplete` attribute provides tokens that tell the browser how to autofill form fields — improving UX and password manager compatibility.

**How it works:**

- Set on `<input>`, `<select>`, `<textarea>` (and `name` on form for section tokens).
- Values are standardized tokens (`email`, `name`, `street-address`, `cc-number`, etc.).
- `autocomplete="off"` on one field may be ignored by browsers for login fields.

| Token | Field |
|-------|-------|
| `email` | Email address |
| `username` / `current-password` | Login |
| `street-address` | Address line |
| `cc-number` | Payment card |

**When to use:**

Login forms · checkout/shipping · one-time codes (`autocomplete="one-time-code"`).

#### Common pitfalls

- `autocomplete="email"` on a name field.
- Disabling autocomplete on login (frustrates users).
- Custom `name` attributes without matching autocomplete tokens.

**Pluses and minuses:**

| + | − |
|------|------|
| Faster form completion | Wrong token mis-fills fields |
| Password manager integration | `off` often ignored for passwords |

**Examples:**

```html
<input name="email" type="email" autocomplete="email" />
<input name="password" type="password" autocomplete="current-password" />
<input name="otp" autocomplete="one-time-code" />
```

**Sources:** [MDN — autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete)

</details>

---

<a name="media--graphics"></a>
