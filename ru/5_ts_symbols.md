# TypeScript — Symbols

Уникальные ключи и `unique symbol`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Symbols](#symbols)

---


## Symbols

<details>
  <summary>Symbols</summary>

### Symbols

**Definition:**

Примитив ES6 для уникальных ключей — `Symbol("desc")` всегда создает новую ценность.

| Когда использовать | Заметки |
|-------------|-------|
| Используйте для поиска ключей, `Symbol.iterator` для итераций, `unique symbol` для брендовых типов. | `sym2 === sym3` всегда `false` даже с таким же описанием. TypeScript `unique symbol` тип связывает идентичность с **конкретным объявлением** — только на `const` or `readonly static`.  --- |

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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
