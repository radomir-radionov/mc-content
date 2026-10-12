# TypeScript — Iterators and Generators

Итераторы, генераторы и `for..of`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Iterators and Generators](#iterators-and-generators)

---


## Iterators and Generators

<details>
  <summary>Iterators and Generators</summary>

### Iterators and Generators

**Definition:**

`Symbol.iterator` позволяет `for..of`; `function*` дает ленивые последовательности.

| Когда использовать | Заметки |
|-------------|-------|
| Введите возврат генератора как `Generator<Yield, Return, Next>`. Итерируемые типы работают с `for..of` и распространиться. | `for..in` перебирает **ключи** (часто индексы массива в виде строк); `for..of` перебирает **значения** через протокол итератора. На `Set`, `for..in` видит только свойства, не являющиеся итераторами, такие как `"species"`, не элемент |

**Examples:**

```typescript
let list = [4, 5, 6];
for (let i in list)  console.log(i); // "0", "1", "2" — keys
for (let v of list)  console.log(v); // 4, 5, 6 — values

function toArray<X>(xs: Iterable<X>): X[] {
  return [...xs];
}

// Generators — function* yields values
function* gen() {
  yield 1;
  yield 2;
}
for (const n of gen()) console.log(n);
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
