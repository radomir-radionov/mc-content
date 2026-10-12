# TypeScript — Object utilities

Утилиты для свойств объектов — [handbook](https://www.typescriptlang.org/docs/handbook/utility-types.html).

[← Utility Types hub](./5_ts_utility_types.md)

### Main concepts

| Concept | Format |
| ------- | ------ |
| Partial | accordion |
| Required | accordion |
| Readonly | accordion |
| Record | accordion |
| Pick | accordion |
| Omit | accordion |

### Related docs

- [Utility Types hub](./5_ts_utility_types.md)
- [Unions & functions](./5_ts_utility_union_function.md)

### Sections

- [Partial](#partial)
- [Required](#required)
- [Readonly](#readonly)
- [Record](#record)
- [Pick](#pick)
- [Omit](#omit)

---


<details>
  <summary>Partial</summary>

**Definition:**

Делает каждое свойство `Type` необязательный.

| Когда использовать                                                                            | Заметки                                                                                                                                                                                                                                  |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Partial<User>` для полезных данных PATCH/обновления, где отправляются только некоторые поля. | Возвращает тип, представляющий **все подмножества** `Type`, а не «той же формы, но необязательно» только в поверхностном смысле — вложенные объекты остаются такими, как они были объявлены, если вы не сопоставляете их самостоятельно. |

**Examples:**

```typescript
interface Todo {
  title: string;
  description: string;
}

function updateTodo(todo: Todo, fields: Partial<Todo>) {
  return { ...todo, ...fields };
}

updateTodo(
  { title: "organize desk", description: "clear clutter" },
  { description: "throw out trash" },
);
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Required</summary>

**Definition:**

Делает все необходимые свойства — противоположными `Partial`.

| Когда использовать                                                                                             | Заметки                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `Required<Partial<User>>` или убедитесь, что объекты конфигурации имеют все ключи после необязательного ввода. | Влияет только на необязательные модификаторы верхнего уровня; он не заполняет волшебным образом недостающие вложенные необязательные цепочки. |

**Examples:**

```typescript
interface Props {
  a?: number;
  b?: string;
}

const ok: Props = { a: 5 };
// const bad: Required<Props> = { a: 5 }; // Error: 'b' is missing
const good: Required<Props> = { a: 5, b: "hi" };
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Readonly</summary>

**Definition:**

Делает каждое свойство верхнего уровня доступным только для чтения — переназначение невозможно.

| Когда использовать                                                                                                   | Заметки                                                                                                                                                           |
| -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Readonly<User>` для замороженной конфигурации. Только поверхностное — вложенные объекты все равно могут мутировать. | Только поверхностное — свойства вложенных объектов по-прежнему можно изменять, если вы не пометите их только для чтения или не используете `Readonly` рекурсивно. |

**Examples:**

```typescript
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = { title: "Delete inactive users" };
// todo.title = "Hello"; // Error: read-only property

// Pairs with Object.freeze at runtime
function freeze<T>(obj: T): Readonly<T> {
  return Object.freeze(obj) as Readonly<T>;
}
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Record</summary>

**Definition:**

Тип объекта с ключами из `Keys` и единый тип значения `Type`.

| Когда использовать                                                                          | Заметки                                                                                                                               |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `Record<"a" \| "b", number>` or `Record<string, boolean>` для словарей и справочных таблиц. | `Keys` должно быть `string \| number \| symbol` — часто союз строковых букв. Удобно для словарей/карт с фиксированным набором ключей. |

**Examples:**

```typescript
type CatName = "miffy" | "boris" | "mordred";

interface CatInfo {
  age: number;
  breed: string;
}

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Pick</summary>

**Definition:**

Новый тип, содержащий только выбранные ключи из `Type`.

| Когда использовать                                                                              | Заметки                                                                              |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `Pick<User, "id" \| "name">` для DTO, предварительных версий или общедоступных подмножеств API. | `Keys` должен быть назначен на `keyof Type` — нельзя подобрать несуществующие ключи. |

**Examples:**

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = { title: "Clean room", completed: false };
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Omit</summary>

**Definition:**

Новый тип с удаленными из списка ключами. `Type`.

| Когда использовать                                                                                                      | Заметки                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `Omit<User, "password">` перед отправкой клиенту. Исправить конфликты пересечений: `Omit<T, "key"> & { key: NewType }`. | Добавлено в ТС 3.5 — до этого люди пользовались `Pick` + `Exclude<keyof T, K>` вручную. |

**Examples:**

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}

type TodoPreview = Omit<Todo, "description" | "createdAt">;
// { title: string; completed: boolean; }
```

**Sources:** [Справочник по TypeScript — типы утилит] (https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>
