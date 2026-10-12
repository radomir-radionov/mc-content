# TypeScript — Arrays

Типизированные массивы: `number[]` и `Array<T>`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Arrays](#arrays)

---

## Arrays

<details>
  <summary>Arrays</summary>

### Arrays

**Definition:**

Упорядоченный список, в котором каждый элемент имеет один и тот же тип — `number[]` or `Array<number>`.

| Когда использовать | Заметки |
|-------------|-------|
| Объявить `const ids: number[] = [1, 2, 3]`. Используйте с `map`, `filter`, `push`и т. д. Для смешанных фиксированных позиций вместо этого используйте **кортеж**. | `[number]` **не** массив чисел — это **кортеж** (массив фиксированной длины с определенными позициями). `number[]` это то, что вам нужно для обычного массива. |

**Examples:**

```typescript
const scores: number[] = [95, 87, 72];
const tags: Array<string> = ["typescript", "javascript"];

// Both are equivalent:
type A = string[];
type B = Array<string>;
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>


