# TypeScript — Type Compatibility

Structural subtyping, assignability, and function parameter variance.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Type Compatibility](#type-compatibility)

---

## Type Compatibility

<details>
  <summary>Type Compatibility</summary>

### Type Compatibility

**Definition:**

Rules for when type A is assignable to type B — structural, not nominal.

| When to use | Notes |
|-------------|-------|
| Extra properties OK on variables passed to functions; excess checks on fresh literals. See assignability table. | TS is intentionally **unsound** in places — extra properties on variables are OK when passing to a function, but **not** in object literals assigned to a typed variable. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Core rules

**Definition:**

// let bad: Pet = { name: "Lassie", owner: "Rudd" }; // Error — excess property check

|

**How it works:**

parameters are checked **bivariantly** by default (unsound) — you can assign `(a: number) => void` to `(b: number, s: string) => void`. Enable `strictFunctionTypes` to fix for non-method signatures.

source return must be a **subtype** of target return.

only **instance** members compared; static/constructor ignored. **Private/protected** members must originate from the **same** class for compatibility.

`Empty<number>` and `Empty<string>` compatible if `T` unused in structure; `NotEmpty<number>` vs `NotEmpty<string>` — error.

**Examples:**

```

| Aspect | Detail |
|--------|--------|
| Summary | // let bad: Pet = { name: "Lassie", owner: "Rudd" }; // Error — excess property check
```

```typescript
interface Pet { name: string; }
let dog = { name: "Lassie", owner: "Rudd" };
let pet: Pet = dog; // OK — extra 'owner' is fine

// let bad: Pet = { name: "Lassie", owner: "Rudd" }; // Error — excess property check
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Assignability table (summary)

**Definition:**

Assignability table (summary) — TypeScript language or compiler feature.

|        | any | unknown | object | void | undefined | null | never |
|--------|-----|---------|--------|------|-----------|------|-------|
| → any  | ✓   | ✓       | ✓      | ✓    | ✓         | ✓*   | ✕     |
| → unknown | ✓ | ✕       | ✕      | ✕    | ✕         | ✕    | ✕     |
| never → | ✓  | ✓       | ✓      | ✓    | ✓         | ✓    | ✓     |

*`strictNullChecks` off changes `null`/`undefined` behavior significantly.

**unknown** ≈ inverse of **never**: everything assignable to `unknown`; `unknown` assignable only to `any` (and itself).
---

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
