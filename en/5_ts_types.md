# TypeScript — Types

Hub for everyday TypeScript types. The index below is grouped by **section** (Reference, Values, Object shapes, …). Open any row to read it here, or use the section topics in the sidebar for a shorter list.

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Types — Values](./5_ts_types_values.md) · [Types — Object shapes](./5_ts_types_shapes.md) · [Types — Combining](./5_ts_types_combining.md) · [Types — Safety](./5_ts_types_safety.md) · [Types in code](./5_ts_types_code.md) · [Narrowing](./5_ts_narrowing.md) · [Functions](./5_ts_functions.md) · [Object Types](./5_ts_objects.md)

### Main concepts

| Section         | Topic page                                      |
| --------------- | ----------------------------------------------- |
| Reference       | Type catalog (this page)                        |
| Values          | [Types — Values](./5_ts_types_values.md)        |
| Object shapes   | [Types — Object shapes](./5_ts_types_shapes.md) |
| Combining types | [Types — Combining](./5_ts_types_combining.md)  |
| Type safety     | [Types — Safety](./5_ts_types_safety.md)        |
| In your code    | [Types in code](./5_ts_types_code.md)           |

### Sections

- [Type catalog](#type-catalog)

---

## Type catalog

<details>
  <summary>Type catalog</summary>

**Definition:**

A single index of **type forms** in TypeScript — primitives, composites, escape hatches, and type-level constructs — with links to where each is explained in this handbook.

**How it works:**

Types are erased at runtime; the compiler uses them for checking only. Each row is **one type or one type form** — not mixed keywords. Deeper syntax (optional fields, assertions, assignability) is linked under **See also**.

#### Value types

| Type                                              | Where to read                                                                                                                                                                          |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `string`, `number`, `boolean`, `bigint`, `symbol` | [Primitives](./5_ts_types_values.md#primitives) · [handbook — primitives](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#the-primitives-string-number-and-boolean) |
| `null`                                            | [null and undefined](./5_ts_types_values.md#null-and-undefined)                                                                                                                        |
| `undefined`                                       | [null and undefined](./5_ts_types_values.md#null-and-undefined)                                                                                                                        |
| Literal (`"open"`, `42`, `true`)                  | [Literal types](./5_ts_types_combining.md#literal-types) · [handbook — literals](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types)                     |
| Array (`T[]`, `Array<T>`)                         | [Arrays](./5_ts_types_values.md#arrays) · [Object Types — Array](./5_ts_objects.md#the-array-type)                                                                                     |
| Tuple (`[string, number]`, …)                     | [Object Types — Tuple](./5_ts_objects.md#tuple-types)                                                                                                                                  |
| Object shape (`{ x: number }`)                    | [Object types](./5_ts_types_shapes.md#object-types) · [Object Types](./5_ts_objects.md)                                                                                                |
| `any`                                             | [any](./5_ts_types_safety.md#any) · [any, unknown, never](./5_ts_top_types.md)                                                                                                         |
| `unknown`                                         | [unknown](./5_ts_types_safety.md#unknown) · [any, unknown, never](./5_ts_top_types.md)                                                                                                 |
| `never`                                           | [Functions — other types](./5_ts_functions.md#other-types-to-know-about) · [Narrowing](./5_ts_narrowing.md) · [any, unknown, never](./5_ts_top_types.md)                               |
| `void`                                            | [Functions — other types](./5_ts_functions.md#other-types-to-know-about)                                                                                                               |
| `object`                                          | [Functions — other types](./5_ts_functions.md#other-types-to-know-about)                                                                                                               |

#### Named and combined types

| Type                          | Where to read                                                                                                                                                    |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `interface`                   | [Interfaces](./5_ts_types_shapes.md#interfaces) · [handbook — interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces)         |
| `type` (alias)                | [Type aliases](./5_ts_types_shapes.md#type-aliases) · [handbook — type aliases](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases) |
| Union (`A \| B`)              | [Union types](./5_ts_types_combining.md#union-types) · [Narrowing](./5_ts_narrowing.md)                                                                          |
| Intersection (`A & B`)        | [Object Types — Intersection](./5_ts_objects.md#intersection-types)                                                                                              |
| Function type (`(a: T) => R`) | [Functions](./5_ts_types_code.md#functions) · [Functions](./5_ts_functions.md)                                                                                   |
| `class`                       | [Classes](./5_ts_classes.md)                                                                                                                                     |
| `enum`                        | [Enums](./5_ts_types_combining.md#enums) · [Enums (deep dive)](./5_ts_enums.md)                                                                                  |
| Generic (`<T>`, `extends`)    | [Generic functions](./5_ts_functions.md#generic-functions) · [Type Manipulation](./5_ts_type_manipulation.md)                                                    |

#### Type-level forms (build types from types)

| Form                                 | Where to read                                                                               |
| ------------------------------------ | ------------------------------------------------------------------------------------------- |
| `typeof`                             | [Type Manipulation](./5_ts_type_manipulation.md)                                            |
| `keyof`                              | [Type Manipulation](./5_ts_type_manipulation.md)                                            |
| Indexed access (`T["prop"]`)         | [Type Manipulation](./5_ts_type_manipulation.md)                                            |
| Mapped type                          | [Type Manipulation](./5_ts_type_manipulation.md) · [Utility Types](./5_ts_utility_types.md) |
| Conditional type                     | [Type Manipulation](./5_ts_type_manipulation.md)                                            |
| Template literal type                | [Type Manipulation](./5_ts_type_manipulation.md)                                            |
| Utility types (`Partial`, `Pick`, …) | [Utility Types](./5_ts_utility_types.md)                                                    |

**See also (not standalone types):** [optional / readonly fields](./5_ts_objects.md#optional-properties) · [index signatures](./5_ts_objects.md#index-signatures) · [type assertions](./5_ts_types_code.md#type-assertions) · [Type vs Interface](./5_ts_types_shapes.md#type-vs-interface) · [Type Compatibility](./5_ts_type_compatibility.md)

**When to use:**

- Interview recap or “what exists in TS?” before diving into one topic.
- Pick a row, open the linked page, then return here for the next gap.

**Sources:** [TypeScript handbook — Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) · [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>
