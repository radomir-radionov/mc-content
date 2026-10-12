# TypeScript — Object utilities

Object property utilities from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/utility-types.html).

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
- [Type Manipulation](./5_ts_type_manipulation.md)

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

Makes every property of `Type` optional.

| When to use                                                                | Notes                                                                                                                                                                        |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Partial<User>` for PATCH/update payloads where only some fields are sent. | Returns a type representing **all subsets** of `Type`, not “same shape but optional” in a shallow sense only — nested objects stay as declared unless you map them yourself. |

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

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Required</summary>

**Definition:**

Makes every property required — opposite of `Partial`.

| When to use                                                                            | Notes                                                                                                    |
| -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `Required<Partial<User>>` or ensure config objects have all keys after optional input. | Only affects top-level optional modifiers; it does not magically fill in missing nested optional chains. |

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

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Readonly</summary>

**Definition:**

Makes every top-level property readonly — cannot reassign.

| When to use                                                                         | Notes                                                                                                                          |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `Readonly<User>` for frozen config. Shallow only — nested objects can still mutate. | Shallow only — nested object properties can still be mutated unless you also mark them readonly or use `Readonly` recursively. |

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

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Record</summary>

**Definition:**

An object type with keys from `Keys` and uniform value type `Type`.

| When to use                                                                                   | Notes                                                                                                                         |
| --------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `Record<"a" \| "b", number>` or `Record<string, boolean>` for dictionaries and lookup tables. | `Keys` must be `string \| number \| symbol` — often a string-literal union. Handy for dictionaries/maps with a fixed key set. |

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

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Pick</summary>

**Definition:**

A new type with only the selected keys from `Type`.

| When to use                                                             | Notes                                                                             |
| ----------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `Pick<User, "id" \| "name">` for DTOs, previews, or public API subsets. | `Keys` must be assignable to `keyof Type` — you can't pick keys that don't exist. |

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

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Omit</summary>

**Definition:**

A new type with listed keys removed from `Type`.

| When to use                                                                                                         | Notes                                                                              |
| ------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `Omit<User, "password">` before sending to client. Fix intersection conflicts: `Omit<T, "key"> & { key: NewType }`. | Added in TS 3.5 — before that people used `Pick` + `Exclude<keyof T, K>` manually. |

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

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>
