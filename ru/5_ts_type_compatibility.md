# TypeScript — Type Compatibility

Structural subtyping, assignability, and function parameter variance.

[← Назад к хабу](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Type Compatibility](#type-compatibility)

---

## Type Compatibility

<details>
  <summary>Type Compatibility</summary>

### Type Compatibility

**Definition:**

Правила присвоения типа А типу Б — структурные, а не номинальные.

| Когда использовать | Заметки |
|-------------|-------|
| Дополнительные свойства разрешены для переменных, передаваемых в функции; лишние проверки новых литералов. См. таблицу назначаемости. | TS намеренно **неправильно** местами — дополнительные свойства переменных допустимы при передаче в функцию, но **нет** в объектных литералах, назначенных типизированной переменной. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Core rules

**Definition:**

// let bad: Pet = { name: "Лесси", владелец: "Радд" }; // Ошибка — проверка лишнего свойства

|

**How it works:**

параметры по умолчанию проверяются **двувариантно** (необоснованно) — вы можете назначить `(a: number) => void` to `(b: number, s: string) => void`. Давать возможность `strictFunctionTypes` исправить сигнатуры, не являющиеся методами.

исходный возврат должен быть **подтипом** целевого возврата.

сравниваются только элементы **экземпляра**; статический/конструктор игнорируется. Для совместимости **частные/защищенные** члены должны происходить из **того же** класса.

`Empty<number>` и `Empty<string>` совместимо, если `T` неиспользованный в конструкции; `NotEmpty<number>` vs `NotEmpty<string>` - ошибка.

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // let bad: Pet = { name: "Lassie", owner: "Rudd" }; // Error — excess property check |
```

```typescript
interface Pet { name: string; }
let dog = { name: "Lassie", owner: "Rudd" };
let pet: Pet = dog; // OK — extra 'owner' is fine

// let bad: Pet = { name: "Lassie", owner: "Rudd" }; // Error — excess property check
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Assignability table (summary)

**Definition:**

Таблица назначаемости (сводка) — язык TypeScript или функция компилятора.

|        | любой | неизвестный | объект | пустота | неопределенный | ноль | никогда |
| -------- | ----- | --------- | -------- | ------ | ----------- | ------ |
| → любой | ✓ | ✓ | ✓ | ✓ | ✓ | ✓* | ✕ |
| → неизвестно | ✓ | ✕ | ✕ | ✕ | ✕ | ✕ | ✕ |
|никогда → | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

*`strictNullChecks` выключить изменения `null`/`undefined` поведение значительно.

**неизвестно** ≈ противоположность **никогда**: все, что можно назначить `unknown`; `unknown` присваивается только `any` (и себя).
---

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
