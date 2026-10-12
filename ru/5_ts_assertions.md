# TypeScript — Assertions & satisfies

Утверждения типов (`as`) и оператор `satisfies`.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Assertions and satisfies](#assertions-and-satisfies)

---

## Assertions and satisfies

<details>
  <summary>Assertions and satisfies</summary>

### Type Assertions

**Definition:**

Приведение во время компиляции, сообщающее TS обрабатывать значение как определенный тип: `value as Type`.

| Когда использовать | Заметки |
|-------------|-------|
| Используйте, когда вы знаете больше, чем компилятор (элементы DOM, сужаются после проверки). Предпочитайте сужение слепому `as`. Нет проверки времени выполнения. | Утверждения работают только тогда, когда типы достаточно перекрываются (`string as number` отклонено). Для диких приведения пройдите `unknown` or `any` первый: `expr as any as TargetType`. А также: **нет проверки во время выполнения** — if |

**Examples:**

```typescript
const canvas = document.getElementById("main_canvas") as HTMLCanvasElement;

// Angle-bracket syntax (not in .tsx files):
const canvas2 = <HTMLCanvasElement>document.getElementById("main_canvas");

// Rejected — types don't overlap:
// const x = "hello" as number;

// Double assertion when you're really sure:
const wild = someValue as unknown as MyType;
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### The `satisfies` Operator (TS 4.9+)

**Definition:**

Оператор, проверяющий соответствие значения типу **без расширения** выведенных литералов (TS 4.9+).

| Когда использовать | Заметки |
|-------------|-------|
| Писать `const x = { ... } satisfies SomeType`. Проверяет форму и сохраняет узкие типы для автозаполнения — безопаснее, чем `as` для объектов конфигурации. | `as Type` отбрасывает умозаключение; `satisfies Type` держит это. Используйте, когда объекты конфигурации должны соответствовать схеме, но вам все равно нужны литеральные типы для автозаполнения. |

**Examples:**

```typescript
type Colors = Record<string, string | number[]>;

// satisfies — validates AND keeps narrow types
const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
} satisfies Colors;
palette.red; // [number, number, number] — tuple preserved

// as — widens red to string | number[]
const palette2 = {
  red: [255, 0, 0],
  green: "#00ff00",
} as Colors;
palette2.red; // string | number[]

// Explicit annotation — same widening problem as `as`
const palette3: Colors = {
  red: [255, 0, 0],
  green: "#00ff00",
};
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>


