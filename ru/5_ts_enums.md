# TypeScript — Enums

Числовые и строковые enum, const enum и `as const`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Enums](#enums)

---


## Enums

<details>
  <summary>Enums</summary>

### Enums

**Definition:**

Набор именованных числовых или строковых констант, которые **существуют во время выполнения** как объект JS.

| Когда использовать | Заметки |
|-------------|-------|
| Объявить `enum Status { Active, Inactive }`, использовать `Status.Active`. Предпочитать `as const` объекты или литеральные объединения в современном коде, чтобы избежать дополнительного размера пакета. | Перечисления — одна из немногих функций TS, которые добавляют код времени выполнения. Многие команды предпочитают `as const` вместо этого объекты или литеральные объединения — та же безопасность типов, без дополнительного вывода JS. Знайте, что они существуют; не тянись к ним |

**Examples:**

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

Direction.Up;    // 0
Direction.Down;  // 1

enum Color { Red, Green, Blue }
Color.Green;           // 1 (auto-increment from 0)
Color.Green === 1;     // true — numeric enums are numbers at runtime

// Custom start values — auto-increment continues
enum Dir2 {
  Up = 2,
  Down = 4,
  Left = 6,
  Right,         // 7
}

// Reverse mapping (numeric enums only)
enum Dir3 { Up }
Dir3[0];         // "Up"

// String enums — no reverse mapping, better for serialization
enum Links {
  Youtube = "https://youtube.com/",
  Vk = "https://vk.com/",
}

// Literal union alternative (no runtime enum object):
type Direction2 = "Up" | "Down" | "Left" | "Right";
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### React bundle size — prefer `as const` over enum

**Definition:**

Обычные перечисления создают объект времени выполнения. Для кода внешнего интерфейса `as const` объекты избегают дополнительного JS:

| Аспект | Деталь |
|--------|--------|
| Резюме | Обычные перечисления создают объект времени выполнения. Для кода внешнего интерфейса `as const` объекты избегают дополнительного JS: |

Обычные перечисления выдают среду выполнения |

Обычные перечисления создают объект времени выполнения. Для кода внешнего интерфейса `as const` объекты избегают дополнительного JS:

**Examples:**

```typescript
// enum — emits runtime object + lookup function
enum Status { Pending = "Pending" }

// Preferred in React/modern code — zero runtime overhead
const Status2 = { Pending: "Pending" } as const;
type Status2 = typeof Status2[keyof typeof Status2];

import { useState } from "react";
const [status, setStatus] = useState(Status2.Pending);
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### `const enum` — inlined at compile time

**Definition:**

// Неиспользуемое константное перечисление — скомпилированный вывод может быть почти пустым:

// "используйте строгое";
// (объект перечисления не создается)

| Аспект | Деталь |
|--------|--------|
| Резюме | // Неиспользуемое константное перечисление — скомпилированный вывод может быть почти пустым: |
// "используйте строгое";
// (объект перечисления не создается) |

**Examples:**

```typescript
const enum Links {
  Vk = "https://vk.com/",
}

const arr = [Links.Vk]; // Links.Vk replaced with literal at compile time

// Unused const enum — compiled output may be nearly empty:
// "use strict";
// (no enum object emitted)

// Used in .d.ts with isolatedModules → can break consumers — prefer regular enum or as const
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)


### Enums

**Definition:**

Именованные константы, которые компилируются в реальный объект JavaScript (числовые или строковые члены).

| Когда использовать | Заметки |
|-------------|-------|
| `enum Color { Red, Green }` затем `Color.Red`. Числовые перечисления поддерживают обратный поиск; строковые перечисления этого не делают. См. «Повседневные типы» → «Перечисления для React/».`as const` альтернативы. | Современный код часто предпочитает `as const` объекты вместо перечислений, чтобы оставаться ближе к простому JS. Числовые перечисления получают **обратные сопоставления** (`Enum[0] === "A"`); строковые перечисления этого не делают. |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Numeric enums

**Definition:**

// Без инициализатора: Вверх = 0, Вниз = 1, ...

enum Dir { Вверх, Вниз, Влево, Вправо }
 |

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // Without initializer: Up = 0, Down = 1, ... |
enum Dir { Up, Down, Left, Right }
```

```typescript
enum Direction {
  Up = 1,
  Down,   // 2 — auto-increment
  Left,   // 3
  Right,  // 4
}

// Without initializer: Up = 0, Down = 1, ...
enum Dir { Up, Down, Left, Right }
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### String enums

**Definition:**

Сериализация/отладка лучше, чем непрозрачные числовые значения. Нет автоинкремента.

| Аспект | Деталь |
|--------|--------|
| Резюме | Сериализация/отладка лучше, чем непрозрачные числовые значения. Нет автоинкремента. |

Сериализация/отладка лучше, чем непрозрачные числовые значения. Нет автоинкремента.

**Examples:**

```typescript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Union enums & const enums

**Definition:**

// const enum — встраивается во время компиляции, нет объекта времени выполнения

const enum Direction { Вверх, Вниз, Влево, Вправо }
let dirs = [Направление.Вверх, Направление.Вниз]; // → [0, 1] в JS

| Оператор | Значение |
|----------|----------|
| `A \| B` | Один из A или B |
| `A & B` | Должен удовлетворить оба |

**How it works:**

использовать `keyof typeof MyEnum`, нет `keyof MyEnum`.

---

**Examples:**

```typescript
// Literal enum members → enum members become types too
enum ShapeKind { Circle, Square }
interface Circle { kind: ShapeKind.Circle; radius: number; }

// const enum — inlined at compile time, no runtime object
const enum Direction { Up, Down, Left, Right }
let dirs = [Direction.Up, Direction.Down]; // → [0, 1] in JS

// Pitfall: ambient const enums in .d.ts break isolatedModules consumers
```

```typescript
const ODirection = { Up: 0, Down: 1, Left: 2, Right: 3 } as const;
type Direction = typeof ODirection[keyof typeof ODirection];
```

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
