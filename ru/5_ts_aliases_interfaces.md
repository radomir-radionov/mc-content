# TypeScript — Aliases & Interfaces

Псевдонимы `type`, `interface` и когда что выбирать.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Aliases and interfaces](#aliases-and-interfaces)

---

## Aliases and interfaces

<details>
  <summary>Aliases and interfaces</summary>

### Type Aliases

**Definition:**

Многоразовое имя для любого типа: `type Point = { x: number; y: number }` or `type ID = string | number`.

| Когда использовать | Заметки |
|-------------|-------|
| Определите один раз, используйте везде. Лучше всего подходит для объединений, кортежей, примитивов, отображаемых/условных типов и сложных композиций. | Псевдоним типа — это просто псевдоним. `type ID = number \| string` везде одинаково с письмом `number \| string`. Вы не можете создать две «разные» версии одного и того же базового типа. Также введите |

**Examples:**

```typescript
type Point = {
  x: number;
  y: number;
};

type ID = number | string;

// Alias for a primitive — same as writing string everywhere
type Name = string;
let id: Name;
id = "42";  // OK
// id = 10; // Error

function printCoord(pt: Point) {
  console.log(pt.x, pt.y);
}

// Aliases work for any type, not just objects:
type Result = "success" | "error" | "pending";
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### Interfaces

**Definition:**

Именованный контракт для формы объекта — сигнатуры свойств и методов без реализации.

| Когда использовать | Заметки |
|-------------|-------|
| `interface User { name: string; getPass(): string }`. Расширить с помощью `extends`, объявления слияния или `implements` на занятиях. | Интерфейсы могут быть **расширены** (`interface Bear extends Animal`) и **объединены** (объявить `interface Window` дважды и поля объединяются). Псевдонимы типов тоже не подходят. Эвристика: используйте `interface` для объекта |

**Examples:**

```typescript
interface Point {
  x: number;
  y: number;
}

interface Animal {
  name: string;
}

interface Bear extends Animal {
  honey: boolean;
}

// Declaration merging — both blocks combine into one Window:
interface Window {
  title: string;
}
interface Window {
  ts: TypeScriptAPI;
}

// Same name merges fields — common for augmenting library types
interface User {
  id: number;
  name: string;
}
interface User {
  age: number; // merged into User
}

const u: User = { id: 1, name: "Alice", age: 25 };
```

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### Interface vs type alias — when to use which

**Definition:** Использовать `interface` для фигур объектов вы можете расширить или **объявление-объединить**. Использовать `type` для объединений, кортежей, примитивов, отображаемых/условных типов и конвейеров служебных типов.

**How it works:**

Оба описывают формы объектов. `interface` можно открыть заново(`interface Window { … }` дважды сливается). `type` не могу — ни секунды `type WindowType = …` является повторяющимся идентификатором. `interface` простирается с `extends`; `type` сочиняет с `&`. Союзы (`string | number`) и отображаемые типы существуют только на `type`.


| `interface` | `type` |
| --- | ------------- |
| Продлить | `extends` | пересечение `&` |
| Открыть заново/объединить | да (слияние объявлений) | нет — повторяющееся имя является ошибкой |
| Союзы/примитивы | нет | да (`string \| number`, `type ID = string`) |
| Кортежи | неловкий | естественный (`[string, number]`) |
| Сопоставленные/условные типы | ограниченный | да — основной инструмент |
| Формы объектов | да | да |
| `implements` на занятиях | да | да |

[Справочник — различия](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

**When to use:** Договоры на объекты публичной библиотеки → `interface` (потребители могут объединиться). Дискриминированные профсоюзы / `Result<T>` → `type`. Эмпирическое правило команды — это хорошо; **неправильный** ответ: «Они идентичны».

**Pluses and minuses:**

| + | − |
|------|------|
| `interface` соответствует тому, как расширяются объекты JS (`.d.ts`) | Слияние может вас удивить |
| `type` выражает союзы и отображаемые типы | Большие условные типы ухудшают читаемость |

**Examples:**

```typescript
// type — intersection
type UserProps = { name: string; age: number };
type AdminProps = UserProps & { role: string };

// interface — extends
interface IUserProps { name: string; age: number }
interface IAdminProps extends IUserProps { role: string }
```

```typescript
const project = {
  title: "Project",
  specification: { areaSize: 100, rooms: 3 },
};
type Specification = typeof project["specification"]; // { areaSize: number; rooms: number }
```

**Thoughts:** Слияние и `extends` → интерфейс. Объединения, кортежи, отображаемые типы → тип.

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>


