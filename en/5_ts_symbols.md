# TypeScript — Symbols

Unique primitive keys and `unique symbol` types.

[← Back to hub](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Symbols](#symbols)

---


## Symbols

<details>
  <summary>Symbols</summary>

### Symbols

**Definition:**

ES6 primitive for unique keys — `Symbol("desc")` always creates a new value.

| When to use | Notes |
|-------------|-------|
| Use for hidden object keys, `Symbol.iterator` for iterables, `unique symbol` for branded types. | `sym2 === sym3` is always `false` even with the same description. TypeScript's `unique symbol` type ties identity to a **specific declaration** — only on `const` or `readonly static`.  --- |

**Examples:**

```typescript
const sym = Symbol();
let obj = { [sym]: "value" };
console.log(obj[sym]); // "value"

// unique symbol — must be const
declare const sym1: unique symbol;
let sym3: typeof sym1 = sym1; // OK — same declaration

const sym2 = Symbol();
const sym3b = Symbol();
// if (sym2 === sym3b) {} // Error: types have no overlap

class C {
  static readonly StaticSymbol: unique symbol = Symbol();
}
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
