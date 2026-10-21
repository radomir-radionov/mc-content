# TypeScript — Манипуляция типами

Создание типов из типов — generics, операторы типов, conditional и mapped types — по [руководству TypeScript](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html).

[← Назад к хабу](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [API contracts](./5_ts_api_contracts.md) · [Object Types](./5_ts_objects.md#generic-object-types) · [Functions](./5_ts_functions.md#generic-functions) · [Utility Types](./5_ts_utility_types.md) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                             | Covers                                                      |
| ----------------------------------------------------------------- | ----------------------------------------------------------- |
| [Overview](#overview)                                             | Что такое Type Manipulation и карта тем handbook            |
| [Creating Types from Types](#creating-types-from-types)           | Композиция типов из существующих типов                      |
| [Generics](#generics)                                             | Параметры типов, ограничения, generic-классы и интерфейсы   |
| [Keyof Type Operator](#keyof-type-operator)                       | Union ключей объекта — `keyof T`                            |
| [`typeof` Type Operator](#typeof-type-operator)                   | Тип значения в type position                                |
| [Indexed Access Types](#indexed-access-types)                     | Тип свойства — `T["key"]`                                   |
| [Conditional Types](#conditional-types)                           | If/else на уровне типов — `T extends U ? X : Y`             |
| [Mapped Types](#mapped-types)                                     | Преобразование каждого свойства — `{ [K in keyof T]: ... }` |
| [Template Literal Types](#template-literal-types)                 | Строковые типы из union и литералов                         |
| [How the tools combine](#how-the-tools-combine)                   | Цепочки операторов                                          |
| [Map Pattern](#map-pattern)                                       | Union → lookup-таблица на объекте                           |
| [Advanced generics](#advanced-generics)                           | Multi-arg inference, `const` type params, currying          |
| [Distributive conditional types](#distributive-conditional-types) | Распределение по union и отключение                         |
| [Recursive type-level helpers](#recursive-type-level-helpers)     | DeepReadonly, лимиты глубины, цепочки в стиле `Awaited`     |
| [Branded / Nominal Types](#branded--nominal-types)                | Паттерн для различимых примитивов на собеседованиях         |

### Sections

- [Overview](#overview)
- [Creating Types from Types](#creating-types-from-types)
- [Generics](#generics)
- [Keyof Type Operator](#keyof-type-operator)
- [`typeof` Type Operator](#typeof-type-operator)
- [Indexed Access Types](#indexed-access-types)
- [Conditional Types](#conditional-types)
- [Mapped Types](#mapped-types)
- [Template Literal Types](#template-literal-types)
- [How the tools combine](#how-the-tools-combine)
- [Map Pattern](#map-pattern)
- [Advanced generics](#advanced-generics)
- [Distributive conditional types](#distributive-conditional-types)
- [Recursive type-level helpers](#recursive-type-level-helpers)
- [Branded / Nominal Types](#branded--nominal-types)

---

## Overview

<details>
<summary>Type Manipulation</summary>

**Definition:** **Type Manipulation** — раздел handbook про построение **новых типов из существующих** — generics, операторы типов и преобразования на уровне типов. Вы описываете связи между типами вместо ручного дублирования форм.

**How it works:**

| Topic                                             | Covers                                          | Example                           |
| ------------------------------------------------- | ----------------------------------------------- | --------------------------------- |
| [Generics](#generics)                             | Параметры типов у функций, интерфейсов, классов | `function id<T>(x: T): T`         |
| [Keyof Type Operator](#keyof-type-operator)       | Union ключей — `keyof T`                        | `type K = keyof { a: 1 }` → `"a"` |
| [`typeof` Type Operator](#typeof-type-operator)   | Тип значения в type position                    | `type S = typeof myString`        |
| [Indexed Access Types](#indexed-access-types)     | Тип свойства по lookup                          | `type Age = Person["age"]`        |
| [Conditional Types](#conditional-types)           | If/else на уровне типов                         | `T extends U ? X : Y`             |
| [Mapped Types](#mapped-types)                     | Преобразование каждого свойства                 | `{ [K in keyof T]: boolean }`     |
| [Template Literal Types](#template-literal-types) | Строковые типы из union                         | `` `${"a" \| "b"}_id` ``          |

**Prerequisites:** Повседневные типы ([Types](./5_ts_types.md)), формы объектов ([Object Types](./5_ts_objects.md)) и основы generic-функций ([Functions — Generic Functions](./5_ts_functions.md#generic-functions)). Встроенные helpers на этих паттернах: [Utility Types](./5_ts_utility_types.md).

**When to use:** Переиспользуемые API (`fetchJson<T>`), безопасный доступ к свойствам (`keyof` + `extends`), вывод типов из значений (`typeof`, indexed access) и собственные utility types (conditionals, mapped types, template literals).

**Thoughts:** Читайте в порядке handbook выше. Начните с **Generics**, затем **keyof** / **typeof** / **indexed access**; добавляйте **conditional** и **mapped** types, когда нужны `infer`, трансформы в стиле `Partial` или remapping ключей. Дополнительные паттерны в конце: [How the tools combine](#how-the-tools-combine), [Map Pattern](#map-pattern), [Branded types](#branded--nominal-types).

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) · [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Creating Types from Types

<details>
<summary>Creating Types from Types</summary>

**Definition:** Приёмы построения **новых типов из существующих** без дублирования форм — ядро продвинутой типизации в TypeScript.

**How it works:**

- **Generics** — параметризация типов и функций (`Box<T>`).
- **Type operators** — `keyof`, `typeof`, indexed access (`T[K]`).
- **Type transforms** — conditional types, mapped types, template literal types.
- Инструменты комбинируются: например, `ReturnType<typeof fn>` использует `typeof` + conditional + `infer`.

**When to use:** Когда runtime-значение или базовый interface уже кодирует нужную форму — выводите типы вместо параллельных определений.

**Examples:**

```typescript
type Box<T> = { value: T };
type Keys = keyof Box<{ id: number }>; // "value"
type Id = Box<{ id: number }>["value"]; // number
```

**Thoughts:** Начните с generics и `keyof`/`typeof`; добавляйте conditionals и mapped types, когда нужны utility types или обёртки API. См. также [Utility Types](./5_ts_utility_types.md) для built-in на этих паттернах.

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>

---

## Generics

<details>
<summary>Generics</summary>

**Definition:** Параметры типов (`<T>`) позволяют одной функции, классу или interface работать с разными типами, сохраняя информацию о типах — в отличие от `any`, который её стирает.

**How it works:**

| Topic                 | Example                                             |
| --------------------- | --------------------------------------------------- |
| Identity function     | `function identity<T>(arg: T): T { return arg; }`   |
| Inference             | `identity("hi")` → `string`                         |
| Generic arrays        | `function first<T>(arr: T[]): T { return arr[0]; }` |
| Generic function type | `let fn: <T>(x: T) => T`                            |
| Generic interface     | `interface Fn<T> { (x: T): T }`                     |
| Generic class         | `class Box<T> { value: T }`                         |
| `extends` constraint  | `<T extends { length: number }>(x: T)`              |
| `keyof` constraint    | `<T, K extends keyof T>(obj: T, key: K)`            |
| Class factory         | `function create<T>(c: new () => T): T`             |
| Default type param    | `<T = HTMLDivElement>(el?: T)`                      |

### Hello World — функция тождества

- `function identity<Type>(arg: Type): Type` захватывает тип аргумента и возвращает тот же тип.
- Вызов с явным аргументом типа: `identity<string>("myString")`.
- Или полагайтесь на **вывод аргумента типа**: `identity("myString")` → `string`.

```typescript
function identity<Type>(arg: Type): Type {
  return arg;
}
const a = identity("myString"); // string
```

### Работа с generic type variables

- `T` — любой тип; нельзя предполагать `.length`, пока нет constraint.
- Используйте `T[]` или `Array<T>`, когда аргумент — массив.

```typescript
function firstElement<T>(arr: T[]): T {
  return arr[0];
}
```

### Generic types (типы функций и interfaces)

- Тип функции: `let fn: <T>(arg: T) => T`.
- Generic interface: `interface GenericIdentityFn<Type> { (arg: Type): Type }`.

```typescript
interface GenericIdentityFn<Type> {
  (arg: Type): Type;
}
let fn: GenericIdentityFn<number> = (n) => n;
```

### Generic classes

- Параметр типа на классе относится к **instance**-членам, не к static.

```typescript
class GenericNumber<NumType> {
  zeroValue!: NumType;
  add!: (x: NumType, y: NumType) => NumType;
}
```

### Generic constraints

- `extends` ограничивает, чем может быть `T`; комбинируйте с `keyof` для безопасного доступа к свойствам.

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}
```

### Using class types in generics

- Передавайте тип конструктора, чтобы фабрика создавала экземпляры.

```typescript
function create<Type>(c: { new (): Type }): Type {
  return new c();
}
```

### Generic parameter defaults

- Пропущенные аргументы типа подставляются из defaults; обязательные параметры — первыми.

```typescript
declare function tag<T extends HTMLElement = HTMLDivElement>(el?: T): T;
```

**When to use:** Переиспользуемые контейнеры, обёртки API, React hooks/components и любой API, где типы входа и выхода должны оставаться связанными. Основы на уровне функций: [Generic Functions](./5_ts_functions.md#generic-functions). На уровне объектов: [Generic Object Types](./5_ts_objects.md#generic-object-types).

**Thoughts:** Паттерн на собеседовании — generic API wrapper: `async function fetchJson<T>(url: string): Promise<ApiResponse<T>>` с `ApiResponse<T> = { success: true; data: T } | { success: false; error: string }`.

**Sources:** [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Keyof Type Operator

<details>
<summary>Keyof Type Operator</summary>

**Definition:** `keyof T` даёт union ключей `T` — `"a" | "b"` для `{ a: 1; b: 2 }`.

**How it works:**

- С index signatures `keyof { [k: string]: boolean }` → `string | number` (числовые ключи stringify).
- Вместе с `extends` — безопасный доступ: `get<T, K extends keyof T>(obj: T, key: K): T[K]`.
- Основа mapped types и многих utility types.

**Examples:**

```typescript
type Point = { x: number; y: number };
type P = keyof Point; // "x" | "y"

type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish; // number

type Mapish = { [k: string]: boolean };
type M = keyof Mapish; // string | number

function pick<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

**Sources:** [TypeScript handbook — Keyof Type Operator](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

</details>

---

## `typeof` Type Operator

<details>
<summary>`typeof` Type Operator</summary>

**Definition:** В **type position** `typeof` обозначает **тип** значения — не runtime-строку `typeof`.

**How it works:**

- Значения и типы в разных namespaces — `typeof myVar` даёт тип из значения.
- `ReturnType<typeof fn>` нужен `typeof`, потому что `fn` — значение, не тип.
- Работает с идентификаторами и их свойствами; не с произвольными call expressions.

**Examples:**

```typescript
let s = "hello";
let n: typeof s; // string

function f() {
  return { x: 10, y: 3 };
}

type P = ReturnType<typeof f>; // { x: number; y: number }

// type P = ReturnType<f>; // Error — f is a value

const STATUS_CONFIG = {
  idle: { label: "Idle", retry: false },
  loading: { label: "Loading…", retry: false },
} as const;

type Status = keyof typeof STATUS_CONFIG;
```

**Sources:** [TypeScript handbook — Typeof Type Operator](https://www.typescriptlang.org/docs/handbook/2/typeof-types.html)

</details>

---

## Indexed Access Types

<details>
<summary>Indexed Access Types</summary>

**Definition:** Тип свойства из другого типа — `Person["age"]` → `number`, или `T[K]` где `K extends keyof T`.

**How it works:**

- Индекс должен быть **типом**, не runtime-переменной — для ключей-значений используйте `typeof key` с `as const`.
- `Person[keyof Person]` — union всех типов свойств.
- `typeof MyArray[number]` — тип элемента массива-значения.

**Examples:**

```typescript
type Person = { age: number; name: string; alive: boolean };

type Age = Person["age"]; // number
type I1 = Person["age" | "name"]; // string | number
type I2 = Person[keyof Person]; // string | number | boolean

const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
];
type PersonFromArray = (typeof MyArray)[number];
type AgeFromArray = (typeof MyArray)[number]["age"]; // number

type key = "age";
type Good = Person[key]; // number
```

**Sources:** [TypeScript handbook — Indexed Access Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)

</details>

---

## Conditional Types

<details>
<summary>Conditional Types</summary>

**Definition:** If/else на уровне типов — `SomeType extends OtherType ? TrueType : FalseType`.

**How it works:**

- **Distributive** по «голым» параметрам типа в union: `ToArray<string | number>` → `string[] | number[]`.
- Обёртка `[T]` отключает distribution: `[Type] extends [any] ? Type[] : never`.
- `infer` извлекает типы в ветках conditional — основа `ReturnType`, `Parameters` и своих unwrap helpers.

**Examples:**

```typescript
type Example1 = Dog extends Animal ? number : string; // number

type NameOrId<T extends number | string> = T extends number
  ? IdLabel
  : NameLabel;

type MessageOf<T> = T extends { message: unknown } ? T["message"] : never;

type Flatten<T> = T extends Array<infer Item> ? Item : T;

type Str = Flatten<string[]>; // string

type GetReturnType<T> = T extends (...args: never[]) => infer Return
  ? Return
  : never;

type Num = GetReturnType<() => number>; // number
```

**When to use:** Разворачивание promises и массивов (`infer`), извлечение return type функций, фильтрация union и свои utility types поверх built-in.

**Pluses and minuses:**

|                                | Benefit                                  | Cost                               |
| ------------------------------ | ---------------------------------------- | ---------------------------------- |
| Distributive conditionals      | Обработка каждого члена union            | Неожиданно, если не знать правило  |
| `infer`                        | Внутренние типы без ручного дублирования | Сложнее читать в глубоких цепочках |
| Non-distributive обёртка `[T]` | Union как целое — предсказуемо           | Дополнительный синтаксис           |

**Thoughts:** Если conditional «режет» union неожиданно — оберните проверяемый тип в tuple: `[T] extends [U] ? X : Y`. См. [Distributive conditional types](#distributive-conditional-types).

**Sources:** [TypeScript handbook — Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)

</details>

---

## Mapped Types

<details>
<summary>Mapped Types</summary>

**Definition:** Преобразование каждого свойства типа перебором ключей — `{ [Property in keyof T]: NewType }`.

**How it works:**

- Модификаторы mapping: `readonly`, `?`, и снятие `-readonly`, `-?`.
- Remapping ключей через `as` (TS 4.1+): `[K in keyof T as NewKey]: T[K]`.
- Фильтр ключей через mapping в `never`.
- Основа `Partial`, `Record`, `Pick` и своих flag-объектов.

**Examples:**

```typescript
type OptionsFlags<T> = {
  [Property in keyof T]: boolean;
};

type Features = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<Features>;
// { darkMode: boolean; newUserProfile: boolean }

type CreateMutable<T> = {
  -readonly [P in keyof T]: T[P];
};

type Concrete<T> = {
  [P in keyof T]-?: T[P];
};

type UpdatedPerson = {
  [P in keyof Person]: P extends "name" ? string : number;
};
```

**Sources:** [TypeScript handbook — Mapped Types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html)

</details>

---

## Template Literal Types

<details>
<summary>Template Literal Types</summary>

**Definition:** Строковые типы из string literals и union — `` `hello ${World}` ``, `` `${A | B}_id` ``.

**How it works:**

- Union в template position **перемножаются**: `` `${"a" | "b"}_${"x" | "y"}` `` → четыре комбинации.
- Вместе с mapped types для имён событий: `` `${K}Changed` ``.
- Built-in intrinsics (не locale-aware): `Uppercase`, `Lowercase`, `Capitalize`, `Uncapitalize`.

**Examples:**

```typescript
type World = "world";
type Greeting = `hello ${World}`; // "hello world"

type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;

type Shouty = Uppercase<"hello">; // "HELLO"
type Cap = Capitalize<"hello world">; // "Hello world"

type PropEventSource<T> = {
  on<K extends string & keyof T>(
    eventName: `${K}Changed`,
    callback: (newValue: T[K]) => void,
  ): void;
};
```

**Sources:** [TypeScript handbook — Template Literal Types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html)

</details>

---

## How the tools combine

<details>
<summary>How the tools combine</summary>

**Definition:** В реальном коде типы **цепочкой** комбинируют операторы — редко по одному.

**Examples:**

```typescript
// keyof + indexed access + mapped + template literal
type EventMap<T> = {
  [K in keyof T as `${string & K}Changed`]: T[K];
};

// typeof + conditional + infer (like ReturnType)
type MyReturn<T> = T extends (...args: never[]) => infer R ? R : never;

// Generic + keyof + extends (typed getProperty)
function pick<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>

---

## Map Pattern

<details>
<summary>Map Pattern</summary>

**Definition:** Большой union несовместимых форм заменяют объектом с ключом-дискриминантом — type-safe lookup без невозможных пересечений.

**When to use:** `type Status = "idle" | "loading" | "error"` хватает, пока каждому статусу не нужны разные metadata. `Record` или объект `as const` масштабируется лучше.

**Examples:**

```typescript
const STATUS_CONFIG = {
  idle: { label: "Idle", retry: false },
  loading: { label: "Loading…", retry: false },
  success: { label: "Done", retry: false },
  error: { label: "Failed", retry: true },
} as const;

type Status = keyof typeof STATUS_CONFIG;

function getConfig(s: Status) {
  return STATUS_CONFIG[s]; // autocomplete for all keys
}
```

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>

---

## Advanced generics

<details>
<summary>Advanced generics</summary>

**Definition:** **Advanced generics** сочетают несколько параметров типа, **constraints**, **defaults** и (с TS 5.0) **`const` type parameters**, чтобы API сохраняли literal и readonly информацию.

**How it works:**

| Technique                 | What it solves                                                            |
| ------------------------- | ------------------------------------------------------------------------- |
| Multi-parameter inference | `function zip<A, B>(a: A[], b: B[]): [A, B][]` — оба типа из аргументов   |
| Constraint + `keyof`      | `<T, K extends keyof T>` привязывает доступ к валидным ключам             |
| Default type params       | `<T = HTMLDivElement>` — удобные call sites; обязательные params первыми  |
| `const` on type param     | `<const T extends readonly string[]>` сохраняет tuple literals в generics |
| Curried generics          | `<T>() => <U>(x: U) => ...` — fluent builders и scoped inference          |

**When to use:** API библиотек (React hooks, data fetchers, ORM helpers), где вызывающий должен получать точные типы без повторения type arguments.

**Examples:**

```typescript
// const type parameter — literals stay literal
function defineRoutes<const T extends readonly RouteDef[]>(routes: T) {
  return routes;
}

const routes = defineRoutes([
  { path: "/users", method: "GET" },
  { path: "/users", method: "POST" },
] as const);

// Multi-arg inference
function merge<A, B>(a: A, b: B): A & B {
  return { ...a, ...b };
}

const m = merge({ id: 1 }, { name: "Ann" }); // { id: number; name: string }

// Factory with constructor constraint
function createInstance<T>(Ctor: new () => T): T {
  return new Ctor();
}
```

**Thoughts:** Если inference выбирает `string` вместо literal union — проверьте `as const` на значении или `const` type parameter на generic. Для API wrappers см. [API contracts — Typed HTTP clients](./5_ts_api_contracts.md#typed-http-clients).

**Sources:** [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) · [TS 5.0 — const type parameters](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#const-type-parameters)

</details>

---

## Distributive conditional types

<details>
<summary>Distributive conditional types</summary>

**Definition:** Когда **проверяемый тип** — **naked type parameter**, который является **union**, conditional type **распределяется** по каждому члену: `ToArray<T | U>` становится `ToArray<T> | ToArray<U>`.

**How it works:**

- **Distributive:** `type ToArray<T> = T extends any ? T[] : never` — каждый член union отдельно.
- **Отключить distribution:** tuple-обёртка — `[T] extends [U] ? X : Y` трактует union как один тип.
- **Фильтр union:** `T extends U ? T : never` оставляет только члены, assignable к `U` (как `Extract`).

**When to use:** Утилиты в стиле `Exclude`/`Extract`, optional transforms по ключам, unwrap helpers с учётом union.

**Examples:**

```typescript
type ToArray<T> = T extends unknown ? T[] : never;

type StrOrNumArray = ToArray<string | number>;
// string[] | number[]  — not (string | number)[]

type NonNullable<T> = T extends null | undefined ? never : T;

type T1 = NonNullable<string | null | undefined>; // string

// Non-distributive — whole union at once
type Boxed<T> = [T] extends [string] ? { s: T } : { other: T };

type B = Boxed<string | number>;
// { other: string | number }
```

**Sources:** [TypeScript handbook — Distributive Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#distributive-conditional-types)

</details>

---

## Recursive type-level helpers

<details>
<summary>Recursive type-level helpers</summary>

**Definition:** **Recursive conditional types** обходят вложенные структуры (массивы, promises, объекты) — как built-in `Awaited`, `DeepReadonly` или JSON-подобные деревья — с опциональным **лимитом глубины**, чтобы избежать ошибок рекурсии компилятора.

**How it works:**

- Снятие слоёв: `T extends Promise<infer U> ? AwaitedInner<U> : T`.
- Обход объекта: map каждого свойства через `keyof` + conditional по типу значения.
- **Depth guard:** tuple `[never, 0, 1, 2, …]` и декремент, чтобы остановить бесконечное развёртывание.

**When to use:** Свои utility types для immutable views, serializable payloads или flattening вложенных цепочек `Promise` в typings библиотек.

**Examples:**

```typescript
type AwaitedDeep<T> = T extends Promise<infer U> ? AwaitedDeep<U> : T;

type DeepReadonly<T> = T extends object
  ? { readonly [K in keyof T]: DeepReadonly<T[K]> }
  : T;

// Depth-limited (conceptual) — stop at D = 0
type Depth = [never, 0, 1, 2, 3, 4, 5];

type DeepPartial<T, D extends number = 5> = [D] extends [never]
  ? T
  : T extends object
    ? { [K in keyof T]?: DeepPartial<T[K], Depth[D]> }
    : T;
```

**Thoughts:** Сначала built-in `Awaited`, `Partial` и codegen для API DTO — до глубокой custom recursion; на очень глубоких типах падает производительность компилятора и качество сообщений об ошибках.

**Sources:** [TypeScript handbook — Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html) · [Utility Types — Awaited](./5_ts_utility_union_function.md)

</details>

---

## Branded / Nominal Types

<details>
<summary>Branded / Nominal Types</summary>

**Definition:** Структурно одинаковые типы делают несовместимыми через phantom brand field — частый паттерн на собеседованиях.

**Examples:**

```typescript
type UserId = string & { readonly __brand: unique symbol };
type OrderId = string & { readonly __brand: unique symbol };

function userId(id: string): UserId {
  return id as UserId;
}

function getUser(id: UserId) {
  /* ... */
}

const uid = userId("u-123");
const oid = userId("o-456") as OrderId;
// getUser(oid); // Error — OrderId is not UserId
getUser(uid); // OK
```

**Sources:** [TypeScript handbook — Creating Types from Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

</details>
