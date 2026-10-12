# TypeScript — Utility Types

Hub for built-in utility types from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/utility-types.html). Open a section topic for full definitions and examples.

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [Types hub](./5_ts_types.md) · [Object utilities](./5_ts_utility_object.md) · [Unions & functions](./5_ts_utility_union_function.md)

### Main concepts

| Section                    | Topic page                                             |
| -------------------------- | ------------------------------------------------------ |
| Reference                  | Utility catalog (this page)                            |
| Object utilities           | [Object utilities](./5_ts_utility_object.md)           |
| Unions, functions, strings | [Unions & functions](./5_ts_utility_union_function.md) |

### Sections

- [Utility catalog](#utility-catalog)

---

## Utility catalog

<details>
  <summary>Utility catalog</summary>

**Definition:**

Built-in **utility types** transform other types (objects, unions, functions, promises, string literals). They ship with TypeScript — no imports.

**How it works:**

Each utility is a generic type alias in `lib.es5.d.ts` / `lib.es2015.*.d.ts`. They compose with `keyof`, conditional types, and mapped types from [Type Manipulation](./5_ts_type_manipulation.md).

#### Quick list

| #   | Utility                                                   | One line                                   |
| --- | --------------------------------------------------------- | ------------------------------------------ |
| 1   | `Awaited<T>`                                              | Unwrap `Promise` (recursive)               |
| 2   | `Partial<T>`                                              | All properties optional                    |
| 3   | `Required<T>`                                             | All properties required                    |
| 4   | `Readonly<T>`                                             | All properties readonly                    |
| 5   | `Record<K, V>`                                            | Object with keys `K` and values `V`        |
| 6   | `Pick<T, K>`                                              | Subset of properties                       |
| 7   | `Omit<T, K>`                                              | All properties except `K`                  |
| 8   | `Exclude<U, E>`                                           | Remove union members assignable to `E`     |
| 9   | `Extract<T, U>`                                           | Keep union members assignable to `U`       |
| 10  | `NonNullable<T>`                                          | Remove `null` and `undefined`              |
| 11  | `Parameters<F>`                                           | Function parameter tuple                   |
| 12  | `ConstructorParameters<C>`                                | Constructor parameter tuple                |
| 13  | `ReturnType<F>`                                           | Function return type                       |
| 14  | `InstanceType<C>`                                         | Instance type of a constructor             |
| 15  | `NoInfer<T>`                                              | Block inference at this position (TS 5.4+) |
| 16  | `ThisParameterType<F>`                                    | Explicit `this` parameter type             |
| 17  | `OmitThisParameter<F>`                                    | Function type without `this`               |
| 18  | `ThisType<T>`                                             | Contextual `this` in object literals       |
| 19  | `Uppercase` / `Lowercase` / `Capitalize` / `Uncapitalize` | String literal transforms                  |

#### Where to read more

| Group                      | Utilities                                                                                         | Page                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| Object utilities           | `Partial`, `Required`, `Readonly`, `Record`, `Pick`, `Omit`                                       | [Object utilities](./5_ts_utility_object.md)           |
| Unions, functions, strings | `Awaited`, `Exclude`, `Extract`, `NonNullable`, function utilities, `ThisType`, string intrinsics | [Unions & functions](./5_ts_utility_union_function.md) |

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

</details>
