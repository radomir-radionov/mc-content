# TypeScript — Type Inference

Best common type, contextual typing, and inference without annotations.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Type Inference](#type-inference)

---

## Type Inference

<details>
  <summary>Type Inference</summary>

### Type Inference

**Definition:**

How TS deduces types when you omit annotations.

| When to use | Notes |
|-------------|-------|
| Let inference work first; annotate at API boundaries. Contextual typing fills callback param types. | Inference picks a **best common type** from candidates — if no single supertype exists, you get a **union** (`Rhino \| Elephant \| Snake`) instead of `Animal[]` unless you annotate or provide context. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Best common type

**Definition:**

Best common type — TypeScript language or compiler feature.

| Aspect | Detail |
|--------|--------|
| Summary | Best common type — TypeScript language or compiler feature. |

**Examples:**

```typescript
let x = 3;                    // number
let arr = [0, 1, null];       // (number | null)[]

let zoo = [new Rhino(), new Elephant(), new Snake()];
// (Rhino | Elephant | Snake)[] — not Animal[]

let zoo2: Animal[] = [new Rhino(), new Elephant(), new Snake()]; // OK
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Contextual typing

**Definition:**

// Without context, parameter is `any` (unless noImplicitAny)

const handler = function (uiEvent) {
  console.log(uiEvent.button); // OK — no error without context
};

| Aspect | Detail |
|--------|--------|
| Summary | // Without context, parameter is `any` (unless noImplicitAny)
const handler = function (uiEvent) {
  console.log(uiEvent |

---

**Examples:**

```typescript
window.onmousedown = function (mouseEvent) {
  console.log(mouseEvent.button);    // OK — MouseEvent
  // console.log(mouseEvent.kangaroo); // Error
};

// Without context, parameter is `any` (unless noImplicitAny)
const handler = function (uiEvent) {
  console.log(uiEvent.button); // OK — no error without context
};

// Context applies to: call arguments, assignment RHS, assertions,
// object/array literals, return statements
function createZoo(): Animal[] {
  return [new Rhino(), new Elephant(), new Snake()]; // Animal[] inferred from return type
}
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
