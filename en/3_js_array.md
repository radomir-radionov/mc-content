# JS Core — Array

Ordered collections, array methods, and interview patterns around `map` / `filter` / `reduce`.

[← Back to JS hub](./3_js.md)

### Related docs

- [JS Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Async](./3_js_async.md) · [JS Document](./3_js_browser.md)
- [JS Interview](./3_js_interview.md) · [TypeScript cart task](./5_ts_interview.md#interview-practice)

### Main concepts

| Area      | Topics                                           |
| --------- | ------------------------------------------------ |
| **Shape** | Indices, `length`, sparse holes, `Array.isArray` |
| **Copy**  | Spread, `slice`, `Array.from`, shallow vs deep   |
| **API**   | Mutating vs non-mutating, search, sort, flat     |
| **HOF**   | `map`, `filter`, `reduce`, `forEach`             |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Array basics](#array-basics)
- [Copy & creation](#copy-and-creation)
- [Mutating methods](#mutating-methods)
- [map, filter, reduce & iteration](#map-filter-reduce-and-iteration)
- [Search, sort & flat](#search-sort-and-flat)
- [Interview](./3_js_interview.md)

<a name="array-basics"></a>

## Array basics

<details>
  <summary>Array basics</summary>

### Array basics

**Definition:** An `Array` is an object for an ordered list of values at nonnegative integer indices, plus a `length` that is one greater than the highest index. It is not an associative map — string keys are ordinary properties, not elements.

**How it works:**

- Zero-based: last index is `length - 1`. `arr[2]` and `arr["2"]` hit the same element; `arr["02"]` is a different property.
- `length` grows when you assign past the end (`arr[5] = x`) and shrinks by **deleting** elements when you lower it.
- Holes (`arr[1] = empty`) are not the same as `undefined`. Older methods (`forEach`, `map`, `filter`) skip holes; newer ones (`keys`, `find`, `includes`) treat holes like `undefined`.
- `typeof [] === "object"` — use `Array.isArray(value)` (or `Object.prototype.toString.call(value) === "[object Array]"`).

**When to use:** Sequences where order and index access matter. Use `Set` for uniqueness-only, `Map` for non-string keys.

**Common pitfalls:**

```js
Array.isArray([]); // true
Array.isArray({ length: 2 }); // false — array-like is not an Array

const sparse = ["a"];
sparse[2] = "c";
sparse.length; // 3
sparse.map((x) => x); // ['a', empty, 'c'] — hole preserved

fruits.length = 10; // extends with holes, not undefined elements
fruits.length = 2; // deletes indexes >= 2
```

**Pluses and minuses:**

| +                              | −                                                    |
| ------------------------------ | ---------------------------------------------------- |
| Literal syntax and rest/spread | Sparse arrays surprise people                        |
| Iterable, JSON-friendly        | Not a real tuple — mixed types and holes are allowed |
| Rich built-in API              | String keys are properties, not elements             |

**Thoughts:** Interviewers want `Array.isArray`, hole vs `undefined`, and that `length` is live — not “arrays are a special primitive.”

**Sources:** [MDN Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) · [javascript.info/array](https://javascript.info/array)

</details>

<a name="copy-and-creation"></a>

## Copy & creation

<details>
  <summary>Copy & creation</summary>

### Copy & creation

**Definition:** Most array copies are **shallow**: a new array whose elements are the same references as the original. Nested objects are not cloned by spread, `slice`, `concat`, or `map`.

**How it works:**

- `[...arr]`, `arr.slice()`, and `arr.concat()` return a new array with the same element references.
- `Array.from(iterable, mapFn?)` builds a real `Array` from array-likes (`{ length, 0: … }`) or any iterable (`Set`, `Map` keys, strings).
- `Array.of(a, b, c)` is like `[a, b, c]` but handles a single numeric argument correctly (`Array.of(7)` → `[7]`, not seven holes).
- Deep clone needs `structuredClone`, a library, or a manual walk — not spread alone.

**When to use:** Copy before mutating in React state or shared data. Use `Array.from` when you have an iterable or DOM `NodeList`, not when you already have an array (spread is enough).

**Examples:**

```js
const arr = ["banana", "apple", "peach"];
const copy = arr.slice(); // shallow copy
const next = [...arr, "mango"]; // new array

Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
Array.from("hi"); // ['h', 'i']
```

**Pluses and minuses:**

| +                                  | −                                                                            |
| ---------------------------------- | ---------------------------------------------------------------------------- |
| Spread/slice are fast and readable | Easy to mistake shallow copy for deep clone                                  |
| `Array.from` maps while building   | `Array(n)` creates holes; `Array.from({ length: n })` fills with `undefined` |

**Sources:** [MDN Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) · [MDN spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

</details>

<a name="mutating-methods"></a>

## Mutating methods

<details>
  <summary>Mutating methods</summary>

### Mutating methods

**Definition:** Some methods change the array in place (`push`, `splice`, `sort`, …). ES2023 added non-mutating twins (`toSpliced`, `toSorted`, `toReversed`) that return a new array and leave the original unchanged.

**How it works:**

| Mutating                             | Non-mutating alternative               |
| ------------------------------------ | -------------------------------------- |
| `push` / `pop` / `shift` / `unshift` | `concat`, `slice`, spread              |
| `splice`                             | `toSpliced`                            |
| `sort`, `reverse`                    | `toSorted`, `toReversed`               |
| `fill`, `copyWithin`                 | build a new array with `map` / `slice` |

- `push` / `pop` — end of array (stack). `shift` / `unshift` — front (queue); `shift` is O(n) because indexes move.
- `splice(start, deleteCount, …items)` removes and/or inserts in place; returns removed elements.
- `delete arr[i]` leaves a **hole** and does not change `length`; prefer `splice` when the list should shrink.

**When to use:** Mutate local scratch buffers; prefer non-mutating methods for props, Redux state, and anything shared. Use `splice` when you need in-place insert/remove and accept mutation.

**Examples:**

```js
const arr = ["banana", "apple", "peach"];
arr.push("kiwi"); // mutates; length 4

const removed = arr.splice(1, 1); // removes 'apple'
const safe = arr.toSpliced(1, 0, "pear"); // new array, arr unchanged
```

**Thoughts:** _Stack = `push`/`pop`. Queue = `push`/`shift` but mind `shift` cost on large arrays._

**Sources:** [MDN Array#mutator_methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#mutator_methods) · [MDN toSpliced](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced)

</details>

<a name="map-filter-reduce-and-iteration"></a>

## map, filter, reduce & iteration

<details>
  <summary>map, filter, reduce & iteration</summary>

### map, filter, reduce & iteration

**Definition:** Iterative methods take `(element, index, array)` and either build a new array or fold to one value. `map` / `filter` / `flatMap` copy; `forEach` returns `undefined`; `reduce` returns whatever you accumulate.

**How it works:**

- `map(fn)` — new array, **same length**; skipped holes stay holes. Do not use `map` only for side effects.
- `filter(fn)` — new array of truthy callback results; holes are skipped (not kept as `undefined`).
- `reduce(fn, init)` — fold. Always pass `init` if the array may be empty (`TypeError` otherwise).
- `forEach(fn)` — visit assigned indexes; cannot `break` (use `for...of` / `some`).
- `flatMap(fn)` — `map` then `flat(1)`; handy when each item expands to zero or more values.
- `["1","2","3"].map(parseInt)` is `[1, NaN, NaN]` — `map` passes `(element, index)` and `parseInt` treats index as radix.

| Method    | Returns                        | Mutates?            |
| --------- | ------------------------------ | ------------------- |
| `map`     | new array, same length         | no                  |
| `filter`  | new array, maybe shorter       | no                  |
| `flatMap` | new array, flattened one level | no                  |
| `reduce`  | accumulator (any type)         | no                  |
| `forEach` | `undefined`                    | no (callback might) |

**When to use:** `map` to transform; `filter` to drop; `reduce` to aggregate or group; `forEach` / `for...of` for effects. Chain `filter` then `map` rather than stuffing both into one `reduce` unless you are building a non-array.

**Pluses and minuses:**

| +                                          | −                                                      |
| ------------------------------------------ | ------------------------------------------------------ |
| Declarative, chainable, interview-standard | `reduce` is easy to overuse into unreadable folds      |
| Copying methods keep originals intact      | Sparse + `map` keeps holes; `map(parseInt)` radix trap |

**Examples:**

```js
[1, 2, 3].map((n) => n * 2); // [2, 4, 6]
[1, 2, 3].filter((n) => n > 1); // [2, 3]
[1, 2, 3].reduce((sum, n) => sum + n, 0); // 6
[1, 2, 3].forEach((n) => console.log(n)); // undefined

["1", "2", "3"].map(Number); // [1, 2, 3]
["1", "2", "3"].map((s) => parseInt(s, 10)); // [1, 2, 3]
```

**Thoughts:** _`map` transforms, `filter` selects, `reduce` aggregates, `forEach` only does work._ If you need one object or number from a list, `reduce`; if you still want a list, stay on `map`/`filter`.

**Sources:** [MDN Array.prototype.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) · [MDN iterative methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#iterative_methods) · [javascript.info/array-methods](https://javascript.info/array-methods)

</details>

<a name="search-sort-and-flat"></a>

## Search, sort & flat

<details>
  <summary>Search, sort & flat</summary>

### Search, sort & flat

**Definition:** Search methods locate elements or indexes; `sort` orders in place (or `toSorted` copies first). `flat` / `flatMap` collapse nested arrays by depth.

**How it works:**

- Search: `find` / `findIndex` / `includes` / `indexOf` / `lastIndexOf`. `includes` uses SameValueZero (`NaN` matches); `indexOf` uses `===` (`NaN` misses).
- `sort(compareFn?)` mutates and returns the **same** array. Default compare converts to strings (UTF-16), so `[10, 2]` → `[10, 2]`. Pass `(a, b) => a - b` for numbers.
- `toSorted(compareFn?)` returns a new sorted array (ES2023).
- `flat(depth = 1)` copies and flattens nested arrays; `flat(Infinity)` for fully nested lists.

| Method               | Returns                               | Mutates?                 |
| -------------------- | ------------------------------------- | ------------------------ |
| `find` / `findIndex` | element / index or `undefined` / `-1` | no                       |
| `sort`               | the **same** array                    | **yes** — use `toSorted` |
| `flat`               | new flattened array                   | no                       |

**When to use:** `find` for first match object; `includes` for primitives and `NaN`. Always pass a compare function for numeric or locale-aware sort. Prefer `flatMap` over `map(...).flat()` when each step may emit multiple items.

**Examples:**

```js
[10, 2, 1].toSorted((a, b) => a - b); // [1, 2, 10]
[1, [2, [3]]].flat(2); // [1, 2, 3]
[NaN].includes(NaN); // true
[NaN].indexOf(NaN); // -1
```

**Pluses and minuses:**

| +                                           | −                                       |
| ------------------------------------------- | --------------------------------------- |
| `toSorted` / `flat` avoid surprise mutation | `sort` without compare is lexicographic |
| `includes` handles `NaN`                    | `shift` + sort on huge lists is costly  |

**Sources:** [MDN Array.prototype.sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) · [MDN Array.prototype.flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) · [MDN Array.prototype.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

</details>
