# TypeScript — hub

Handbook, common types, language reference, compiler, and interview practice. Structure follows the [official TypeScript docs](https://www.typescriptlang.org/docs/handbook/intro.html).

### Related docs

[JS — Language (JS vs ECMAScript)](./3_js_core.md#javascript-and-ecmascript) · [Interview](./5_ts_interview.md)

### Navigation — Handbook

Matches the [official TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html).

| Topic                                                    | Covers                                                                                                                                                                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [The Basics](./5_ts_basics.md)                           | tsconfig, declaration files, and intro for JavaScript programmers.                                                                                                                                      |
| [Compiler fundamentals](./5_ts_compiler_fundamentals.md) | Static type-checking, `tsc`, emit behavior, explicit types, and downleveling.                                                                                                                           |
| [Types](./5_ts_types.md)                                 | Hub + catalog; sections: [Values](./5_ts_types_values.md), [Shapes](./5_ts_types_shapes.md), [Combining](./5_ts_types_combining.md), [Safety](./5_ts_types_safety.md), [In code](./5_ts_types_code.md). |
| [Narrowing types](./5_ts_narrowing.md)                   | Control-flow analysis and type guards.                                                                                                                                                                  |
| [Functions](./5_ts_functions.md)                         | Function types, generics, parameters, overloads, and `this`.                                                                                                                                            |
| [Object Types](./5_ts_objects.md)                        | Object shapes, optional properties, tuples.                                                                                                                                                             |
| [Type Manipulation](./5_ts_type_manipulation.md)         | Generics, keyof, typeof, mapped and conditional types.                                                                                                                                                  |
| [Classes](./5_ts_classes.md)                             | Fields, visibility, inheritance, `implements`, abstract classes.                                                                                                                                        |
| [Modules](./5_ts_modules.md)                             | ES modules, CommonJS, resolution, and emit targets.                                                                                                                                                     |
| [Utility Types](./5_ts_utility_types.md)                 | Hub + [Object](./5_ts_utility_object.md) and [Unions & functions](./5_ts_utility_union_function.md) utilities.                                                                                          |
| [Interview](./5_ts_interview.md)                         | TypeScript interview questions, answers not filled in.                                                                                                                                                  |
| [Tasks](/frontend/typescript/ts-interview-tasks)         | Type-level coding challenges.                                                                                                                                                                           |

### Navigation — Old (Common)

Moved to [Types](./5_ts_types.md). Legacy filenames: `5_ts_primitives.md`, `5_ts_arrays.md`, `5_ts_top_types.md`, `5_ts_type_annotations.md`, `5_ts_unions_literals.md`, `5_ts_aliases_interfaces.md`, `5_ts_assertions.md`, `5_ts_null_undefined.md`.

### Navigation — Reference

Matches the [official Reference section](https://www.typescriptlang.org/docs/handbook/utility-types.html).

| Topic                                                  | Covers                                            |
| ------------------------------------------------------ | ------------------------------------------------- |
| [Cheat Sheets](./5_ts_cheat_sheets.md)                 | Quick-reference cards and memorization checklist. |
| [Decorators](./5_ts_decorators.md)                     | Stage 2 and stage 3 decorators.                   |
| [Declaration Merging](./5_ts_declaration_merging.md)   | Interface, namespace, class merging.              |
| [Enums](./5_ts_enums.md)                               | Numeric/string enums and `as const` alternatives. |
| [Iterators and Generators](./5_ts_iterators.md)        | `Symbol.iterator`, generators, `for..of`.         |
| [JSX](./5_ts_jsx.md)                                   | JSX in `.tsx` and component typing.               |
| [Mixins](./5_ts_mixins.md)                             | Compose classes from building blocks.             |
| [Namespaces](./5_ts_namespaces.md)                     | Legacy `namespace` organization.                  |
| [Namespaces and Modules](./5_ts_namespaces_modules.md) | ES modules vs legacy namespaces.                  |
| [Symbols](./5_ts_symbols.md)                           | Unique keys and `unique symbol`.                  |
| [Triple-Slash Directives](./5_ts_triple_slash.md)      | `///` compiler directives and `declare`.          |
| [Type Compatibility](./5_ts_type_compatibility.md)     | Structural subtyping and assignability.           |
| [Type Inference](./5_ts_type_inference.md)             | Best common type and contextual typing.           |
| [Variable Declaration](./5_ts_variable_declaration.md) | `var`, `let`, `const`, and destructuring.         |

---

## Resources & links

<details>
  <summary>External references</summary>

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
- [TypeScript cheatsheets](https://www.typescriptlang.org/cheatsheets)
- [Type challenges (practice)](https://github.com/type-challenges/type-challenges)
- [TS 5.0 release notes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html) (`const` type params, decorators)
- [Project references](https://www.typescriptlang.org/docs/handbook/project-references.html)

</details>

---

<details>
  <summary>TypeScript vs JavaScript</summary>
