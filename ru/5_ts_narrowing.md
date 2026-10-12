# TypeScript — Narrowing

Анализ потока управления и защита типов.

[ ← Вернуться в хаб](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Main concepts

| Площадь | Темы |
|------|--------|
| Охранники | `typeof`, правдивость, равенство, `in`, `instanceof` |
| Пользовательский | Предикаты типов, функции утверждения |
| Поток | Сужение назначения, анализ потока управления |
| Союзы | Дискриминированные профсоюзы, `never` и полнота |

### Sections

- [Type guards](#type-guards)
- [Custom guards](#custom-guards)
- [Control flow](#control-flow)
- [Unions](#unions)

---

## Type guards

<details>
<summary>Type narrowing</summary>

**Definition:** TypeScript уточняет объявленное объединение до более конкретного типа, анализируя проверки во время выполнения и поток управления.

**How it works:**
- Компилятор накладывает анализ типов на конструкции JavaScript (`if`/`else`, тройки, петли, истинность, равенство).
- *Защита типа* — это проверка, которую понимает TypeScript (`typeof`, `in`, `instanceof`, предикаты).
- После охраны вы можете смело использовать членов, которые существуют только в этом варианте.

**When to use:** Любое значение, введенное как объединение перед доступом к членам, которые не являются общими для всех вариантов. Пропустите, если тип уже является одним конкретным типом.

**Examples:**

```typescript
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

**Thoughts:** Структура интервью: сужение — это анализ потока управления, а не анализ во время выполнения. `as` ничего не проверяет.

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>`typeof` type guards</summary>

**Definition:** Проверка `typeof value === "..."` является защитой типа, поскольку TypeScript кодирует JavaScript `typeof` результаты.

**How it works:**
- Распознанные строки: `"string"`, `"number"`, `"bigint"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, `"function"`.
- `typeof null` is `"object"` — a `typeof x === "object"` ветка все еще включает в себя `null`.
- Массивы — это объекты; `typeof arr === "object"` не доказывает `T[]` само собой.

**When to use:** Примитивные союзы (`string | number`, `string | undefined`). Не используйте `typeof` фильтровать `null` - использовать `=== null`.

**Examples:**

```typescript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}

function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    // still `string[] | null` — arrays and null are both objects
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>Truthiness narrowing</summary>

**Definition:** Условия, которые принуждают к логическому значению (`if (value)`, `!value`, `&&` / `||`) сузить ложные варианты.

**How it works:**
- Ложь: `false`, `0`, `0n`, `""`, `null`, `undefined`, `NaN`. Все остальное правда.
- `if (x)` капли `null`/`undefined` (и другие ложные примитивы) из истинной ветки.
- `Boolean(x)` делает вывод `boolean`; `!!x` делает вывод буквально `true` когда `x` всегда правдив.

**When to use:** Защита от `null`/`undefined` на объектах и ​​массивах. Пропустить, когда `0` or `""` являются действительными значениями — они также являются ложными.

**Examples:**

```typescript
function printLength(str: string | null | undefined) {
  if (str) {
    console.log(`Length: ${str.length}`);
  } else {
    // null, undefined, or ""
    console.log("No valid string provided");
  }
}

function checkCount(count: number | undefined) {
  if (count) {
    // skipped when count is 0
    console.log(`Count is ${count}`);
  }
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>Equality narrowing</summary>

**Definition:** `===`, `!==`, `==`, `!=`, и `switch` сужается, когда TypeScript может доказать, какие члены союза остаются.

**How it works:**
- Строгое равенство (`===` / `!==`) сравнивает тип и значение.
- Свободное равенство: `== null` / `!= null` это обычный ярлык для обоих `null` и `undefined`.
- При сравнении двух объединений в истинной ветви сохраняется только перекрывающийся тип.

**When to use:** Литеральные теги, `null`/`undefined` проверки и сопоставление двух переменных, имеющих общий тип. Предпочитать `===` за исключением `== null` идиома.

**Examples:**

```typescript
function processInput(x: string | number, y: string | boolean) {
  if (x === y) {
    // only shared type: string
    console.log(x.toUpperCase());
    console.log(y.toUpperCase());
  }
}

function printValue(value: string | null | undefined) {
  if (value != null) {
    console.log(value.trim());
  }
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>`in` operator narrowing</summary>

**Definition:** `"key" in obj` сужает объединение объектов до членов, которые объявляют это свойство.

**How it works:**
- Истинная ветвь: типы, имеющие ключ (обязательный или необязательный).
- Ложная ветвь: типы, у которых **нет** ключ в качестве обязательного свойства.
- Если ключ не является обязательным (`?`) в одном варианте этот вариант может остаться в `else` ветвь.

**When to use:** Конструктивно разные объекты (`Fish | Bird`), когда вы не используете общее дискриминантное поле. Предпочитаю буквальный `kind` поле, когда вы управляете обоими типами.

**Examples:**

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) animal.swim();
  else animal.fly();
}

type Car = { drive: () => void; sunroof?: boolean };
type Truck = { drive: () => void };

function serviceVehicle(vehicle: Car | Truck) {
  if ("sunroof" in vehicle) {
    // Car
  } else {
    // Truck | Car (Car may omit sunroof)
  }
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>`instanceof` narrowing</summary>

**Definition:** `value instanceof Ctor` сужается до типа экземпляра этого конструктора, используя цепочку прототипов во время выполнения.

**How it works:**
- Работает с классами и функциями-конструкторами (`Date`, `Error`, пользовательские классы).
- Интерфейсы и псевдонимы типов удалены — `instanceof User` является ошибкой компиляции, если `User` это всего лишь тип.
- `Symbol.hasInstance` с предикатом типа можно настроить то, что `instanceof` сужается до.

**When to use:** Ценности, которые вы создали с помощью `new`или встроенные конструкторы. Использовать `in` /предикаты/дискриминанты для простых объектов.

**Examples:**

```typescript
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
  } else {
    console.log(x.toUpperCase());
  }
}

interface User { name: string }
// Error: 'User' only refers to a type, but is being used as a value.
// if (obj instanceof User) { ... }
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

---

## Custom guards

<details>
<summary>Type predicates</summary>

**Definition:** Пользовательский тип защиты — это функция, тип возвращаемого значения которой `arg is SomeType`.

**How it works:**
- On `true`, аргумент сужается до `SomeType` на месте вызова.
- На `false`, этот тип исключается из оставшегося объединения.
- Тело *не* проверяется по предикату — ошибка. `return true` будет лгать средству проверки типов.

**When to use:** Когда встроенной защиты недостаточно(`Array.isArray` помощники по стилю, проверка полезных данных API). Следите за честностью проверки времени выполнения.

**Examples:**

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) pet.swim();
  else pet.fly();
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>Assertion functions</summary>

**Definition:** Функция с возвращаемым типом `asserts condition` or `asserts val is Type` сужается путем броска, если проверка не удалась.

**How it works:**
- `asserts condition` — если функция возвращает значение, `condition` is `true` для остальной части объема.
- `asserts val is Type` - после возвращения, `val` имеет этот тип (защита типа выдачи ошибок).
- Работает с дженериками, например. `asserts val is NonNullable<T>`.

**When to use:** Предварительные условия при входе в функцию, когда сбой должен быть прерван. Предпочитать `if` + предикаты, когда обе ветви должны продолжаться.

**Examples:**

```typescript
function assert(condition: unknown, msg?: string): asserts condition {
  if (!condition) {
    throw new Error(msg ?? "Assertion Failed");
  }
}

function assertIsNumber(val: unknown): asserts val is number {
  if (typeof val !== "number") {
    throw new Error("Not a number!");
  }
}

function assertNonNull<T>(val: T): asserts val is NonNullable<T> {
  if (val == null) {
    throw new Error("Value cannot be null or undefined");
  }
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

---

## Control flow

<details>
<summary>Assignment narrowing</summary>

**Definition:** Присвоение более конкретного значения сужает *наблюдаемый* тип переменной до следующего присвоения.

**How it works:**
- *Объявленный тип* — это аннотация (то, что вы можете назначить позже).
- *Наблюдаемый тип* — это суженный тип после последнего назначения.
- Вы по-прежнему не можете присвоить значение вне объявленного типа.

**When to use:** `let` переменные, которые начинаются как объединение и получают конкретное значение. `const` уже назначенный литерал/тип.

**Examples:**

```typescript
let data: string | number = Math.random() < 0.5 ? 42 : "hello";

data = 100;
console.log(data.toFixed(2));

data = "goodbye";
console.log(data.toUpperCase());

// Error: boolean is not assignable to string | number
// data = true;
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>Control flow analysis</summary>

**Definition:** TypeScript отслеживает, как `if`, `return`, `throw`, `break`, и циклы меняют типы на каждом пути, а затем повторно объединяют их при соединении путей.

**How it works:**
- Ранний `return`/`throw` удаляет этот вариант из остальной части функции — нет `else` необходимый.
- Присвоения внутри веток разделяют тип; после `if`/`else`, остальные возможности объединяются.
- Доступность имеет значение: код после невозвратного вызова недоступен.

**When to use:** Защитные предложения и многоветвевые функции. Не предполагайте, что тип остается суженным после обратного вызова, который может быть назначен позже (замыкания могут сбросить анализ).

**Examples:**

```typescript
function process(input: string | number) {
  if (typeof input === "number") {
    return input * 2;
  }
  return input.toUpperCase();
}

function getStatus() {
  let result: string | number | boolean = true;

  if (Math.random() < 0.5) {
    result = "Success";
  } else {
    result = 500;
  }

  return result; // string | number — boolean is gone
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

---

## Unions

<details>
<summary>Discriminated unions</summary>

**Definition:** Объединение типов объектов, которые имеют общее дискриминантное поле (`kind`, `type`), поэтому проверки этого поля сужают весь объект.

**How it works:**
- Каждый вариант имеет один и тот же ключ с разными строками/числами/логическими литералами.
- `if (shape.kind === "circle")` делает другие поля (`radius`) доступный.
- Необязательные поля в одном общем интерфейсе **не** работают одинаково — TypeScript не подтвердится. `radius` существует из `kind`.

**When to use:** Закрытые наборы принадлежащих вам вариантов (события, узлы AST, результаты). Предпочитайте это мешку дополнительных свойств.

**Examples:**

```typescript
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
  }
}
```

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

<details>
<summary>`never` and exhaustiveness</summary>

**Definition:** `never` — это тип значений, которые никогда не встречаются — оставшиеся члены объединения после обработки каждого варианта или функция, которая всегда выдает/зацикливается.

**How it works:**
- Только `never` присваивается `never`.
- В `switch` `default`, присвойте значение `const _exhaustive: never = shape`.
- Добавление нового члена союза приводит к сбою этого задания, пока вы не добавите `case`.

**When to use:** Дискриминированные профсоюзы, как вы ожидаете, останутся исчерпывающими. Сопряжение с `default` in `switch` поэтому новые варианты являются ошибкой компиляции.

**Examples:**

```typescript
interface Circle { kind: "circle"; radius: number }
interface Square { kind: "square"; sideLength: number }

type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default: {
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
    }
  }
}
```

**Thoughts:** Та же идея, что и исчерпывающий `switch` через объединение TypeScript в коде приложения: необработанные варианты должны давать сбой во время компиляции, а не автоматически во время выполнения.

**Sources:** [Справочник по TypeScript — сужение](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>
