# TypeScript — JSX

JSX in `.tsx`, intrinsic elements, and component typing.

[← Back to hub](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [JSX](#jsx)

---


## JSX

<details>
  <summary>JSX</summary>

### JSX

**Definition:**

XML-like syntax in `.tsx` files for UI frameworks (React, etc.).

| When to use | Notes |
|-------------|-------|
| Set `"jsx": "react-jsx"` in `tsconfig`. TS types props via `JSX.IntrinsicElements` and component types. | Angle-bracket type assertions (`<Foo>bar`) are **disallowed** in `.tsx` — use `bar as Foo` instead (parser ambiguity with JSX tags). |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Setup & jsx modes

**Definition:**

1. Use `.tsx` extension

2. Set `"jsx"` in `tsconfig.json`

1. Use `.tsx` extension
2. Set `"jsx"` in `tsconfig.json`

| Mode | Output |
|------|--------|
| `preserve` | Keep JSX → `.jsx` |
| `react` | `React.createElement(...)` |
| `react-jsx` | Automatic runtime `_jsx(...)` |
| `react-native` | Keep JSX → `.js` |

**How it works:**

* Lowercase (`<div>`) → intrinsic DOM/host element → `JSX.IntrinsicElements`
* Uppercase (`<MyComponent>`) → your component → resolved from scope

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Type checking highlights

**Definition:**

// FC must return JSX.Element (or customize JSX.ElementType in TS 5.1+)

interface Props { name: string }
function Greet(props: Props) {
  return <span>{props.name}</span>;
}

| Aspect | Detail |
|--------|--------|
| Summary | // FC must return JSX.Element (or customize JSX.ElementType in TS 5.1+)
interface Props { name: string }
function Greet( |

Configure via `jsxFactory`, `jsxFragmentFactory`, `jsxImportSource`.
---

**Examples:**

```typescript
// Intrinsic elements
declare namespace JSX {
  interface IntrinsicElements {
    foo: { bar?: boolean };
  }
}

// FC must return JSX.Element (or customize JSX.ElementType in TS 5.1+)
interface Props { name: string }
function Greet(props: Props) {
  return <span>{props.name}</span>;
}

// Children — JSX.ElementChildrenAttribute (default: children)
// Attributes — JSX.ElementAttributesProperty (default: props)
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
