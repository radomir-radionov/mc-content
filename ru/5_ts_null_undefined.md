# TypeScript — null & undefined

Типы отсутствия значения и `strictNullChecks`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [null and undefined](#null-and-undefined)

---

## null and undefined

<details>
  <summary>null and undefined</summary>

### `null` and `undefined`

**Definition:**

Типы отсутствия значения в JavaScript: `null` (намеренно пусто) и `undefined` (не установлено).

| Когда использовать | Заметки |
|-------------|-------|
| С `strictNullChecks`, они не могут быть назначены другим типам, если они не объявлены. Узкий с `=== null`, `??`или необязательная цепочка. | Поведение полностью зависит от `strictNullChecks`. **Выключенный:** `null`/`undefined` бесшумно перемещаться повсюду — как Java без нулевой безопасности. **Вкл.** необходимо сузить поле перед использованием возможного нулевого значения. ` |

**Examples:**

```typescript
function doSomething(x: string | null) {
  if (x === null) {
    return;
  }
  console.log(x.toUpperCase()); // OK — x narrowed to string
}

function liveDangerously(x?: number | null) {
  console.log(x!.toFixed()); // ! asserts x is not null/undefined — no runtime check
}
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>

