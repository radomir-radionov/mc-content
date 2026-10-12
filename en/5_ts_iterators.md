# TypeScript — Iterators and Generators

`Symbol.iterator`, iterables, generators, and `for..of` typing.

[← Back to hub](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Iterators and Generators](#iterators-and-generators)

---


## Iterators and Generators

<details>
  <summary>Iterators and Generators</summary>

### Iterators and Generators

**Definition:**

`Symbol.iterator` enables `for..of`; `function*` yields lazy sequences.

| When to use | Notes |
|-------------|-------|
| Type generator return as `Generator<Yield, Return, Next>`. Iterable types work with `for..of` and spread. | `for..in` iterates **keys** (often array indices as strings); `for..of` iterates **values** via the iterator protocol. On `Set`, `for..in` only sees non-iterator properties like `"species"`, not eleme |

**Examples:**

```typescript
let list = [4, 5, 6];
for (let i in list)  console.log(i); // "0", "1", "2" — keys
for (let v of list)  console.log(v); // 4, 5, 6 — values

function toArray<X>(xs: Iterable<X>): X[] {
  return [...xs];
}

// Generators — function* yields values
function* gen() {
  yield 1;
  yield 2;
}
for (const n of gen()) console.log(n);
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
