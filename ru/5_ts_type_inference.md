# TypeScript — Type Inference

Best common type, contextual typing, and inference without annotations.

[← Назад к хабу](./5_ts.md)

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

Как TS выводит типы, когда вы опускаете аннотации.

| Когда использовать | Заметки |
|-------------|-------|
| Пусть сначала сработает умозаключение; аннотировать на границах API. Контекстная типизация заполняет типы параметров обратного вызова. | Вывод выбирает **наилучший общий тип** из кандидатов — если ни одного супертипа не существует, вы получаете **объединение** (`Rhino \| Elephant \| Snake`) вместо `Animal[]` если вы не комментируете или не предоставляете контекст. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Best common type

**Definition:**

Лучший распространенный тип — язык TypeScript или функция компилятора.

| Аспект | Деталь |
|--------|--------|
| Резюме | Лучший распространенный тип — язык TypeScript или функция компилятора. |

**Examples:**

```typescript
let x = 3;                    // number
let arr = [0, 1, null];       // (number | null)[]

let zoo = [new Rhino(), new Elephant(), new Snake()];
// (Rhino | Elephant | Snake)[] — not Animal[]

let zoo2: Animal[] = [new Rhino(), new Elephant(), new Snake()]; // OK
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Contextual typing

**Definition:**

// Без контекста параметр `any` (если только noImplicitAny)

константный обработчик = функция (uiEvent) {
  console.log(uiEvent.button); // ОК — нет ошибок без контекста
};

| Аспект | Деталь |
|--------|--------|
| Резюме | // Без контекста параметр `any` (если только noImplicitAny) |
константный обработчик = функция (uiEvent) {
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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
