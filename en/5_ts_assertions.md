# TypeScript — Assertions & satisfies

Type assertions (`as`, `!`) — [Types in code](./5_ts_types_code.md#type-assertions). This page covers **`satisfies`**.

[← Back to hub](./5_ts.md)

### Related docs

[Types](./5_ts_types.md) · [Interview](./5_ts_interview.md)

### Sections

- [satisfies](#satisfies)

---

## satisfies

<details>
  <summary>satisfies</summary>

**Definition:**

An operator that checks a value matches a type **without widening** inferred literals (TS 4.9+).

**How it works:**

`as Type` throws away inference; `satisfies Type` validates shape and keeps narrow types for autocomplete.

**When to use:**

Config objects that must match a schema but should keep literal types.

**Examples:**

```typescript
type Colors = Record<string, string | number[]>;

const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
} satisfies Colors;
palette.red; // [number, number, number]

const palette2 = {
  red: [255, 0, 0],
  green: "#00ff00",
} as Colors;
palette2.red; // string | number[]
```

**Sources:** [TypeScript handbook — Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>
