# TypeScript — Types — Safety

`any` and `unknown` — opting out of checking vs type-safe top types.

[← Types hub](./5_ts_types.md)

### Related docs

[Types hub](./5_ts_types.md) · [Narrowing](./5_ts_narrowing.md) · [Top types](./5_ts_top_types.md) · [Types in code](./5_ts_types_code.md) · [Strictness & tsconfig](./5_ts_basics.md#strictness--tsconfig)

### Main concepts

| Topic     | Role                         |
| --------- | ---------------------------- |
| `any`     | Escape hatch — checking off  |
| `unknown` | Safe top — narrow before use |

### Sections

- [any](#any)
- [unknown](#unknown)

---

## any

<details>
  <summary>any</summary>

**Definition:**

A special type that disables type-checking for a value — TypeScript assumes you know better than the compiler.

**How it works:**

With `any`, you can call anything, assign to/from any type, and access any property (all become `any`). **`any` is contagious** — once a value is `any`, expressions built from it tend to stay `any`, so mistakes do not surface at compile time.

**Explicit vs implicit `any`:**

|                         | Explicit `any`                | Implicit `any`                                       |
| ----------------------- | ----------------------------- | ---------------------------------------------------- |
| How it appears          | You write `: any` or `as any` | You omit a type and the checker has nothing to infer |
| With default `tsconfig` | Allowed (escape hatch)        | Often **allowed** — parameter becomes `any` silently |
| With `noImplicitAny`    | Still allowed if you wrote it | **Error** — you must add a type or fix inference     |

### `noImplicitAny`

**Definition:**

A `tsconfig` compiler option that turns **implicit** `any` into a compile error when TypeScript would otherwise fall back to `any` because it cannot infer a type.

| When to use                                                                                             | Notes                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| On by default via `"strict": true` on new projects; enable `"noImplicitAny": true` on legacy codebases. | Without it, untyped function parameters (and some other positions) silently become `any` — you lose checking **inside** that function even if the rest of the file is strict. Does **not** ban explicit `: any`; it only blocks the silent kind. |

**Examples:**

```typescript
let obj: any = { x: 0 };
obj.foo();
obj = "hello";
const n: number = obj; // allowed — dangerous

// --- implicit any (noImplicitAny) ---

// noImplicitAny: OFF — compiles; `person` is any
function greet(person) {
  person.toUpperCase();
}

// noImplicitAny: ON — error: Parameter 'person' implicitly has an 'any' type
function greet(person: string) {
  person.toUpperCase();
}

let data; // noImplicitAny: ON — error unless you annotate or assign immediately
let data: unknown;
```

**When to use:**

Rare escape hatch for gradual migration or untyped third-party code. Prefer [`unknown`](#unknown) at boundaries. Keep **`noImplicitAny` enabled** so you do not accumulate hidden `any` — see also [Strictness & tsconfig](./5_ts_basics.md#strictness--tsconfig).

**Sources:** [TypeScript handbook — Everyday Types: any](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any) · [TypeScript handbook — noImplicitAny](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#noimplicitany) · [tsconfig — strict](https://www.typescriptlang.org/tsconfig#strict)

</details>

---

## unknown

<details>
  <summary>unknown</summary>

**Definition:**

A **type-safe top type** — like `any`, it can hold any JavaScript value, but TypeScript will not let you _use_ that value until you narrow or assert it.

**How it works:**

Both `any` and `unknown` accept every value. The difference is what the checker allows **after** assignment:

|                                                   | `any`                      | `unknown`                               |
| ------------------------------------------------- | -------------------------- | --------------------------------------- |
| Assign any value to it                            | yes                        | yes                                     |
| Read properties / call methods without checks     | yes (unsafe)               | **no** — compile error                  |
| Assign to `string`, `number`, etc. without checks | yes (unsafe)               | **no** — compile error                  |
| Spread into another value without checks          | yes                        | **no**                                  |
| Typical role                                      | opt out of the type system | external input, `JSON.parse`, migration |

Narrow with `typeof`, `instanceof`, equality checks, user-defined type guards, or validation libraries — see [Narrowing](./5_ts_narrowing.md). `any` is **assignable to** `unknown` (you can always “forget” safety), but assigning `unknown` to a concrete type requires proof.

With **`useUnknownInCatchVariables`** (part of `strict`), `catch (e)` gives `e: unknown` instead of `any`.

**When to use:**

- Function parameters for data you do not trust yet (API responses, `JSON.parse`, message payloads).
- Replacing `any` at module boundaries while you add real types.
- Prefer `unknown` over `any` whenever you are willing to narrow once before use.

**Pluses and minuses:**

| +                                                                                       | −                                            |
| --------------------------------------------------------------------------------------- | -------------------------------------------- |
| Forces handling of bad shapes at compile time; does not “infect” other types like `any` | More branching or validation code than `any` |

**Examples:**

```typescript
let value: unknown = JSON.parse('{"id": 1}');

// value.id;           // Error — 'value' is of type 'unknown'
// const n: number = value; // Error

if (typeof value === "object" && value !== null && "id" in value) {
  const id = (value as { id: number }).id;
}

let loose: any = value; // OK — widening to any is allowed
loose.foo(); // allowed on any — still unsafe at runtime

function parseUser(input: unknown): { name: string } {
  if (
    typeof input === "object" &&
    input !== null &&
    "name" in input &&
    typeof (input as { name: unknown }).name === "string"
  ) {
    return { name: (input as { name: string }).name };
  }
  throw new Error("Invalid user");
}
```

**Sources:** [TypeScript handbook — Other Types: unknown](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown) · [TypeScript handbook — Everyday Types: any](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#any)

</details>
