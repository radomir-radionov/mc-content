# TypeScript — Unions & functions

Утилиты для union, функций, Promise и строковых литералов.

[← Utility Types hub](./5_ts_utility_types.md)

### Main concepts

| Concept | Format |
| ------- | ------ |
| Awaited | accordion |
| Exclude | accordion |
| Extract | accordion |
| NonNullable | accordion |
| Parameters | accordion |
| ConstructorParameters | accordion |
| ReturnType | accordion |
| InstanceType | accordion |
| NoInfer | accordion |
| ThisParameterType | accordion |
| OmitThisParameter | accordion |
| ThisType | accordion |
| String manipulation types | accordion |

### Related docs

- [Utility Types hub](./5_ts_utility_types.md)
- [Object utilities](./5_ts_utility_object.md)

### Sections

- [Awaited](#awaited)
- [Exclude](#exclude)
- [Extract](#extract)
- [NonNullable](#nonnullable)
- [Parameters](#parameters)
- [ConstructorParameters](#constructorparameters)
- [ReturnType](#returntype)
- [InstanceType](#instancetype)
- [NoInfer](#noinfer)
- [ThisParameterType](#thisparametertype)
- [OmitThisParameter](#omitthisparameter)
- [ThisType](#thistype)
- [String manipulation types](#string-manipulation-types)

---


<details>
  <summary>Awaited</summary>

**Definition:**

Разворачивает `Promise` печатает рекурсивно — моделирует то, что `await` возвращается.

| Когда использовать                                                                                                      | Заметки                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `type R = Awaited<Promise<Promise<string>>>` → `string`. Работает и в союзах с членами, не являющимися членами Promise. | Работает и с профсоюзами — `Awaited<boolean \| Promise<number>>` становится `number \| boolean`, не просто `number`. |

**Examples:**

```typescript
type A = Awaited<Promise<string>>; // string
type B = Awaited<Promise<Promise<number>>>; // number
type C = Awaited<boolean | Promise<number>>; // number | boolean
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Exclude</summary>

**Definition:**

Удаляет членов союза, которым можно назначить `ExcludedMembers`.

| Когда использовать                                                                                       | Заметки                                                                                                                                                                        |
| -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Exclude<"a" \| "b" \| "c", "c">` → `"a" \| "b"`. Работает с **объединениями**, а не с ключами объектов. | Использует присваиваемость, а не равенство — `Exclude<string \| number, Function>` урожайность `string \| number` потому что ни один из них не может быть назначен `Function`. |

**Examples:**

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"

type Shape = { kind: "circle"; radius: number } | { kind: "square"; x: number };

type NoCircle = Exclude<Shape, { kind: "circle" }>;
// { kind: "square"; x: number }
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Extract</summary>

**Definition:**

Сохраняет только членов профсоюза, которым можно назначить `Union`.

| Когда использовать                                            | Заметки                                                                                    |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `Extract<Primitive, boolean>` → `boolean`. Зеркало `Exclude`. | Зеркало `Exclude` — `Extract<T, U>` ≡ `T extends U ? T : never` (дистрибутив над союзами). |

**Examples:**

```typescript
type T0 = Extract<"a" | "b" | "c", "a" | "f">; // "a"
type T1 = Extract<string | number | (() => void), Function>; // () => void

type Circle = Extract<Shape, { kind: "circle" }>;
// { kind: "circle"; radius: number }
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>NonNullable</summary>

**Definition:**

Удаляет `null` и `undefined` от `Type`.

| Когда использовать                                                                                                                 | Заметки                                                           |
| ---------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `NonNullable<string \| null>` → `string`. После проверки на нулевое значение или для ввода гарантированно присутствующих значений. | Удаляет только этих двоих — `void` и другие ложные типы остаются. |

**Examples:**

```typescript
type T0 = NonNullable<string | number | undefined>; // string | number
type T1 = NonNullable<string[] | null | undefined>; // string[]
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Parameters</summary>

**Definition:**

Извлекает типы параметров функции в виде кортежа.

| Когда использовать                                                                  | Заметки                                                                                                                                    |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `Parameters<typeof fn>` → `[string, number]` для оберток и функций высшего порядка. | Для перегруженных функций используются параметры **последней** перегрузки. `Parameters<any>` → `unknown[]`; `Parameters<never>` → `never`. |

**Examples:**

```typescript
declare function f1(arg: { a: number; b: string }): void;

type T0 = Parameters<() => string>; // []
type T1 = Parameters<(s: string) => void>; // [s: string]
type T2 = Parameters<typeof f1>; // [arg: { a: number; b: string }]
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ConstructorParameters</summary>

**Definition:**

Извлекает типы параметров конструктора в виде кортежа.

| Когда использовать                                                                                | Заметки                                                                                                                                 |
| ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `ConstructorParameters<typeof Date>` для упаковки собственных или пользовательских конструкторов. | `ConstructorParameters<Function>` is `never` — `Function` слишком свободен, чтобы соответствовать `abstract new (...args: any) => any`. |

**Examples:**

```typescript
type T0 = ConstructorParameters<ErrorConstructor>; // [message?: string]

class C {
  constructor(a: number, b: string) {}
}
type T1 = ConstructorParameters<typeof C>; // [a: number, b: string]
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ReturnType</summary>

**Definition:**

Извлекает тип возвращаемого значения типа функции.

| Когда использовать                                                               | Заметки                                                                                                                          |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `ReturnType<typeof fetchUser>` — определить тип результата API без дублирования. | Перегрузки → побеждает последняя подпись. `ReturnType<string>` ошибки (не функция); `ReturnType<Function>` деградирует до `any`. |

**Examples:**

```typescript
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>; // string
type T1 = ReturnType<typeof f1>; // { a: number; b: string }
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>InstanceType</summary>

**Definition:**

Получает тип экземпляра из типа конструктора.

| Когда использовать                                                                   | Заметки                                                                                              |
| ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| `InstanceType<typeof MyClass>` → `MyClass`. Для фабричных шаблонов и контейнеров DI. | `InstanceType<typeof MyClass>` дает тебе `MyClass` экземпляры — полезны для фабричных шаблонов и DI. |

**Examples:**

```typescript
class C {
  x = 0;
  y = 0;
}

type T0 = InstanceType<typeof C>; // C
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>NoInfer</summary>

**Definition:**

Блокирует TS вывод аргументов типа из этой позиции (TS 5.4+).

| Когда использовать                                                                                                     | Заметки                                                                                                                                                                                                        |
| ---------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Используйте параметры по умолчанию/общие параметры, которые не должны приводить к выводу: `defaultColor?: NoInfer<C>`. | Добавлено в TS 5.4 — исправлены случаи, когда параметр по умолчанию/обобщенный параметр выводится из аргумента, который вы не хотели использовать для вывода (например, `defaultColor` который расширяет `C`). |

**Examples:**

```typescript
function createStreetLight<C extends string>(
  colors: C[],
  defaultColor?: NoInfer<C>,
) {
  // ...
}

createStreetLight(["red", "yellow", "green"], "red"); // OK
// createStreetLight(["red", "yellow", "green"], "blue"); // Error
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ThisParameterType</summary>

**Definition:**

Извлекает тип явного `this` параметр из типа функции.

| Когда использовать                                                            | Заметки                                                                                                        |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Расширенный — для набора текста `bind` обертки и консервация `this` контекст. | Применяется только тогда, когда функция объявляет `this:` явно — обычные методы используют контекстный `this`. |

**Examples:**

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>OmitThisParameter</summary>

**Definition:**

Удаляет `this` из типа функции — результат `.bind()`.

| Когда использовать                                | Заметки                                                                         |
| ------------------------------------------------- | ------------------------------------------------------------------------------- |
| Введите вывод привязки, когда `this` фиксировано. | Дженерики удаляются; в типе вывода сохраняется только **последняя** перегрузка. |

**Examples:**

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);
console.log(fiveToHex()); // "5"
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ThisType</summary>

**Definition:**

Маркер для контекста `this` внутри объектных литералов с методами, а не утилитой преобразования.

| Когда использовать                                                                                      | Заметки                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Использование в дескрипторах объектов, где необходимо ввести методы. `this` без явного `this` параметр. | Пустой интерфейс в `lib.d.ts`; имеет смысл только тогда, когда компилятор применяет контекстную типизацию к `methods` в дескрипторе объекта. |

**Examples:**

```typescript
type ObjectDescriptor<D, M> = {
  data?: D;
  methods?: M & ThisType<D & M>;
};

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
  return { ...(desc.data ?? {}), ...(desc.methods ?? {}) } as D & M;
}

const obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      this.x += dx; // 'this' is { x: number; y: number } & methods
      this.y += dy;
    },
  },
});
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>

## Intrinsic string manipulation types

<details>
  <summary>String manipulation types</summary>

**Definition:**

Встроенные операторы типов для строковых литералов: `Uppercase`, `Lowercase`, `Capitalize`, `Uncapitalize`.

| Когда использовать                                                                                               | Заметки                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Применить к объединениям строковых литералов в отображаемых/шаблонных типах — `Uppercase<"hello">` → `"ПРИВЕТ"`. | Only work on string literal types (or unions of them) — `Прописные буквы<string>` stays `строка`, а не конкретный результат. |

**Examples:**

```typescript
type EventName<T extends string> = `${T}Changed`;
type T0 = Uppercase<"hello">; // "HELLO"
type T1 = Capitalize<"hello">; // "Hello"
type T2 = Uncapitalize<"Hello">; // "hello"

type GetterName<T extends string> = `get${Capitalize<T>}`;
type SetterName<T extends string> = `set${Capitalize<T>}`;
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>
