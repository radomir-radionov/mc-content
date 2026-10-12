# TypeScript — Primitives

`string`, `number`, `boolean`, `bigint` и `symbol`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Primitives](#primitives)

---

## Primitives

<details>
  <summary>Primitives</summary>

### Primitives: `string`, `number`, `boolean`

**Definition:**

Основные типы значений в JavaScript — текст, числа и true/false. TypeScript использует те же строчные имена, что и `typeof`.

| Когда использовать | Заметки |
|-------------|-------|
| Аннотировать переменные (`let age: number = 42`) или позвольте выводу заполнить их (`let city = "Berlin"` → `string`). Используйте для параметров функции, типов возвращаемых значений и полей объекта. | В JavaScript нет `int` or `float` — все числовое просто `number`. Также, `String`, `Number`, и `Boolean` (с заглавной буквы) существуют как редкие типы оберток. Почти всегда используйте строчные буквы `string`, `онемевший |

**Examples:**

```typescript
let name: string = "Alice";
let age: number = 42;        // not int, not float — just number
let active: boolean = true;

// Number literals — all typed as number
const decimal: number = 6;
const hex: number = 0xf00d;
const binary: number = 0b1010;
const octal: number = 0o744;

// null and undefined are their own types
const u: undefined = undefined;
const n: null = null;

// Inference works too — often you don't need the annotation
let city = "Berlin";         // inferred as string
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### Less Common Primitives: `bigint` and `symbol`

**Definition:**

`bigint` для целых чисел за пределами `Number.MAX_SAFE_INTEGER`; `symbol` для уникальных ключей свойств.

| Когда использовать | Заметки |
|-------------|-------|
| Писать `100n` or `BigInt(100)` для биинта. `Symbol("id")` создает уникальный ключ — каждый вызов возвращает новый символ. | Каждый `Symbol("name")` вызов создает **уникальное** значение — два символа с одинаковым описанием никогда не равны (`===` всегда ложно). В этом вся суть: гарантированные уникальные ключи, особенно для o |

**Examples:**

```typescript
const big: bigint = 100n;
const alsoBig: bigint = BigInt(100);

const id = Symbol("id");
const alsoId = Symbol("id");

if (id === alsoId) {
  // Never runs — always false, even with same description
}

const user = {
  [id]: "secret-metadata",
  name: "Alice",
};
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

<a name="type-narrowing"></a>

</details>


