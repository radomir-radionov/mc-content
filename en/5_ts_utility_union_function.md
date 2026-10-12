# TypeScript — Unions & functions

Union, function, promise, and string literal utilities.

[← Utility Types hub](./5_ts_utility_types.md)

### Main concepts

| Concept | Format |
| ------- | ------ |
| Awaited | accordion |
| Exclude | accordion |
| Extract | accordion |
| NonNullable | accordion |
| Parameters | accordion |
| ConstructorParameters | accordion |
| ReturnType | accordion |
| InstanceType | accordion |
| NoInfer | accordion |
| ThisParameterType | accordion |
| OmitThisParameter | accordion |
| ThisType | accordion |
| String manipulation types | accordion |

### Related docs

- [Utility Types hub](./5_ts_utility_types.md)
- [Object utilities](./5_ts_utility_object.md)
- [Functions](./5_ts_functions.md)

### Sections

- [Awaited](#awaited)
- [Exclude](#exclude)
- [Extract](#extract)
- [NonNullable](#nonnullable)
- [Parameters](#parameters)
- [ConstructorParameters](#constructorparameters)
- [ReturnType](#returntype)
- [InstanceType](#instancetype)
- [NoInfer](#noinfer)
- [ThisParameterType](#thisparametertype)
- [OmitThisParameter](#omitthisparameter)
- [ThisType](#thistype)
- [String manipulation types](#string-manipulation-types)

---


<details>
  <summary>Awaited</summary>

**Definition:**

Unwraps `Promise` types recursively — models what `await` returns.

| When to use                                                                                            | Notes                                                                                                       |
| ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `type R = Awaited<Promise<Promise<string>>>` → `string`. Works on unions with non-Promise members too. | Works on unions too — `Awaited<boolean \| Promise<number>>` becomes `number \| boolean`, not just `number`. |

**Examples:**

```typescript
type A = Awaited<Promise<string>>; // string
type B = Awaited<Promise<Promise<number>>>; // number
type C = Awaited<boolean | Promise<number>>; // number | boolean
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Exclude</summary>

**Definition:**

Removes union members assignable to `ExcludedMembers`.

| When to use                                                                                | Notes                                                                                                                                           |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `Exclude<"a" \| "b" \| "c", "c">` → `"a" \| "b"`. Operates on **unions**, not object keys. | Uses assignability, not equality — `Exclude<string \| number, Function>` yields `string \| number` because neither is assignable to `Function`. |

**Examples:**

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"

type Shape = { kind: "circle"; radius: number } | { kind: "square"; x: number };

type NoCircle = Exclude<Shape, { kind: "circle" }>;
// { kind: "square"; x: number }
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Extract</summary>

**Definition:**

Keeps only union members assignable to `Union`.

| When to use                                                     | Notes                                                                                         |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| `Extract<Primitive, boolean>` → `boolean`. Mirror of `Exclude`. | Mirror of `Exclude` — `Extract<T, U>` ≡ `T extends U ? T : never` (distributive over unions). |

**Examples:**

```typescript
type T0 = Extract<"a" | "b" | "c", "a" | "f">; // "a"
type T1 = Extract<string | number | (() => void), Function>; // () => void

type Circle = Extract<Shape, { kind: "circle" }>;
// { kind: "circle"; radius: number }
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>NonNullable</summary>

**Definition:**

Removes `null` and `undefined` from `Type`.

| When to use                                                                                         | Notes                                                       |
| --------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `NonNullable<string \| null>` → `string`. After a null check, or to type guaranteed-present values. | Only removes those two — `void` and other falsy types stay. |

**Examples:**

```typescript
type T0 = NonNullable<string | number | undefined>; // string | number
type T1 = NonNullable<string[] | null | undefined>; // string[]
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>Parameters</summary>

**Definition:**

Extracts function parameter types as a tuple.

| When to use                                                                           | Notes                                                                                                                              |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `Parameters<typeof fn>` → `[string, number]` for wrappers and higher-order functions. | For overloaded functions, uses the **last** overload's parameters. `Parameters<any>` → `unknown[]`; `Parameters<never>` → `never`. |

**Examples:**

```typescript
declare function f1(arg: { a: number; b: string }): void;

type T0 = Parameters<() => string>; // []
type T1 = Parameters<(s: string) => void>; // [s: string]
type T2 = Parameters<typeof f1>; // [arg: { a: number; b: string }]
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ConstructorParameters</summary>

**Definition:**

Extracts constructor parameter types as a tuple.

| When to use                                                                      | Notes                                                                                                                 |
| -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `ConstructorParameters<typeof Date>` for wrapping native or custom constructors. | `ConstructorParameters<Function>` is `never` — `Function` is too loose to match `abstract new (...args: any) => any`. |

**Examples:**

```typescript
type T0 = ConstructorParameters<ErrorConstructor>; // [message?: string]

class C {
  constructor(a: number, b: string) {}
}
type T1 = ConstructorParameters<typeof C>; // [a: number, b: string]
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ReturnType</summary>

**Definition:**

Extracts the return type of a function type.

| When to use                                                                 | Notes                                                                                                                    |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `ReturnType<typeof fetchUser>` — infer API result type without duplicating. | Overloads → last signature wins. `ReturnType<string>` errors (not a function); `ReturnType<Function>` degrades to `any`. |

**Examples:**

```typescript
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>; // string
type T1 = ReturnType<typeof f1>; // { a: number; b: string }
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>InstanceType</summary>

**Definition:**

Gets the instance type from a constructor type.

| When to use                                                                         | Notes                                                                                              |
| ----------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `InstanceType<typeof MyClass>` → `MyClass`. For factory patterns and DI containers. | `InstanceType<typeof MyClass>` gives you `MyClass` instances — useful for factory patterns and DI. |

**Examples:**

```typescript
class C {
  x = 0;
  y = 0;
}

type T0 = InstanceType<typeof C>; // C
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>NoInfer</summary>

**Definition:**

Blocks TS from inferring type arguments from this position (TS 5.4+).

| When to use                                                                                 | Notes                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Use on default/generic params that should not drive inference: `defaultColor?: NoInfer<C>`. | Added in TS 5.4 — fixes cases where a default/generic parameter gets inferred from an argument you didn't want to drive inference (e.g. a `defaultColor` that widens `C`). |

**Examples:**

```typescript
function createStreetLight<C extends string>(
  colors: C[],
  defaultColor?: NoInfer<C>,
) {
  // ...
}

createStreetLight(["red", "yellow", "green"], "red"); // OK
// createStreetLight(["red", "yellow", "green"], "blue"); // Error
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ThisParameterType</summary>

**Definition:**

Extracts the type of an explicit `this` parameter from a function type.

| When to use                                                          | Notes                                                                                                |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Advanced — for typing `bind` wrappers and preserving `this` context. | Only applies when the function declares `this:` explicitly — ordinary methods use contextual `this`. |

**Examples:**

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>OmitThisParameter</summary>

**Definition:**

Removes `this` from a function type — result of `.bind()`.

| When to use                                      | Notes                                                                       |
| ------------------------------------------------ | --------------------------------------------------------------------------- |
| Type the output of binding when `this` is fixed. | Generics are erased; only the **last** overload is kept in the output type. |

**Examples:**

```typescript
function toHex(this: Number) {
  return this.toString(16);
}

const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);
console.log(fiveToHex()); // "5"
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>


<details>
  <summary>ThisType</summary>

**Definition:**

Marker for contextual `this` inside object literals with methods — not a transforming utility.

| When to use                                                                              | Notes                                                                                                                            |
| ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Use in object descriptors where methods need typed `this` without explicit `this` param. | Empty interface in `lib.d.ts`; only meaningful when the compiler applies contextual typing to `methods` in an object descriptor. |

**Examples:**

```typescript
type ObjectDescriptor<D, M> = {
  data?: D;
  methods?: M & ThisType<D & M>;
};

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
  return { ...(desc.data ?? {}), ...(desc.methods ?? {}) } as D & M;
}

const obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      this.x += dx; // 'this' is { x: number; y: number } & methods
      this.y += dy;
    },
  },
});
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>

## Intrinsic string manipulation types

<details>
  <summary>String manipulation types</summary>

**Definition:**

Built-in type operators for string literals: `Uppercase`, `Lowercase`, `Capitalize`, `Uncapitalize`.

| When to use                                                                                 | Notes                                                                                                              |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Apply to string literal unions in mapped/template types — `Uppercase<"hello">` → `"HELLO"`. | Only work on string literal types (or unions of them) — `Uppercase<string>` stays `string`, not a specific result. |

**Examples:**

```typescript
type EventName<T extends string> = `${T}Changed`;
type T0 = Uppercase<"hello">; // "HELLO"
type T1 = Capitalize<"hello">; // "Hello"
type T2 = Uncapitalize<"Hello">; // "hello"

type GetterName<T extends string> = `get${Capitalize<T>}`;
type SetterName<T extends string> = `set${Capitalize<T>}`;
```

**Sources:** [TypeScript handbook — utility-types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
</details>
