# JS Core — Data Structures

Objects, keyed collections, classic trees/graphs, dates, and JSON. Primitives and type rules live in [Data Types](./3_js_data_types.md); bindings in [Variables](./3_js_variables.md). Complexity and interview depth → [Data Structures (CS)](./7_special_data_structures.md).

[← Back to JS hub](./3_js.md)

### Related docs

- [Data Types](./3_js_data_types.md) · [Variables](./3_js_variables.md) · [Closures](./3_js_functions.md#closures) · [JS Arrays](./3_js_array.md) · [JS Language](./3_js_core.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [YAML vs JSON](./10_build_ci.md#yaml) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area                   | Topics                                             |
| ---------------------- | -------------------------------------------------- |
| **Objects**            | Plain objects, `Object.*` utilities                |
| **Collections**        | `Map`, `Set`, `WeakMap`, `WeakSet`                 |
| **Classic structures** | Trees, graphs, linked lists, stack & queue (in JS) |
| **Interop**            | `Date`, `JSON.parse` / `stringify`                 |

### Sections

- [Objects](#objects)
- [Collections](#collections)
- [Classic structures](#classic-structures)
- [Interop](#interop)
- [Objects and Object utilities](#objects-and-object-utilities)
- [Collections in short](#collections-in-short) · [Map](#map) · [Set](#set) · [WeakMap](#weakmap) · [WeakSet](#weakset)
- [Classic structures in short](#classic-structures-in-short) · [Trees](#trees) · [Graphs](#graphs) · [Linked lists](#linked-lists) · [Stack and queue](#stack-and-queue)
- [Date](#date) · [JSON](#json)
- [Interview](./3_js_interview.md)

Sibling Core topics: [Language](./3_js_core.md) · [Data Types](./3_js_data_types.md) · [Variables](./3_js_variables.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [Array](./3_js_array.md).

<a name="data-structures"></a>

## Objects

### Objects

<a name="objects-and-object-utilities"></a>

<details>
  <summary>Objects and Object utilities</summary>

### Plain objects & Object utilities

**Definition:** A **plain object** is `{}` or `new Object()` — a collection of key–value properties. `Object.*` static methods operate on objects without needing an instance.

**How it works:**

- Keys are strings or `Symbol`; other types stringify (`{[obj]: 1}` → `"[object Object]"`).
- Property access: dot `obj.key` or bracket `obj['key']` / `obj[expr]`.
- Shorthand: `{ name, age }` equals `{ name: name, age: age }`; computed keys `{ [dynamic]: 1 }`.
- `in` checks the prototype chain; `Object.hasOwn(obj, key)` checks own properties only.
- Spread / `Object.assign` shallow-merge objects.

| Method                          | Returns                    |
| ------------------------------- | -------------------------- |
| `Object.keys(obj)`              | Own enumerable string keys |
| `Object.values(obj)`            | Own enumerable values      |
| `Object.entries(obj)`           | `[key, value]` pairs       |
| `Object.assign(target, ...src)` | Target after shallow merge |
| `Object.hasOwn(obj, prop)`      | Boolean (own prop)         |
| `Object.freeze(obj)`            | Frozen object (shallow)    |

|                    | `key in obj`                             | `Object.hasOwn(obj, key)`                   |
| ------------------ | ---------------------------------------- | ------------------------------------------- |
| Own property       | yes                                      | yes                                         |
| Inherited property | yes                                      | no                                          |
| Use when           | Need to know if property exists anywhere | Need own keys only (safe `for...in` filter) |

**When to use:** Config objects, API payloads, maps with string keys. Use `Map` for non-string keys; see [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) for prototypes, descriptors, and deep object patterns.

**Pluses and minuses:**

| +                                               | −                                                                                 |
| ----------------------------------------------- | --------------------------------------------------------------------------------- |
| JSON-friendly DTOs                              | Keys always stringify to string/Symbol                                            |
| Literal syntax is concise                       | Shared references on shallow copy                                                 |
| `Object.entries` + `fromEntries` for transforms | No guaranteed iteration order before ES2015 (now insertion order for string keys) |

**Examples:**

```js
const user = { name: "Ann", age: 30 };
const { name, ...rest } = user;

"age" in user; // true
Object.hasOwn(user, "age"); // true
Object.hasOwn(user, "toString"); // false — inherited

const merged = { ...defaults, ...user };
const pairs = Object.fromEntries(
  Object.entries(user).map(([k, v]) => [k, String(v)]),
);
```

**Sources:** [javascript.info/object](https://javascript.info/object) · [javascript.info/keys-values-entries](https://javascript.info/keys-values-entries)

</details>

## Collections

### Collections

<a name="map-set-weakmap-weakset"></a>
<a name="collections-in-short"></a>

<details>
  <summary>Collections in short</summary>

### Collections in short

**Definition:** Four built-in collection types beyond plain objects and arrays. `Map` is a key→value store; `Set` is a bag of unique values. `WeakMap` and `WeakSet` are like their strong counterparts but hold **weak** references to object keys/values so garbage collection can reclaim them when nothing else references those objects.

|                       | `Map`                          | `Set`               | `WeakMap`               | `WeakSet`               |
| --------------------- | ------------------------------ | ------------------- | ----------------------- | ----------------------- |
| Stores                | Key → value pairs              | Unique values       | Key (object) → value    | Unique objects          |
| Key / value types     | Any type                       | Any type            | **Objects only** (keys) | **Objects only**        |
| Duplicate keys/values | Keys unique; `set` overwrites  | Values unique       | Keys unique             | Values unique           |
| Iterable              | Yes (`for...of`)               | Yes                 | No                      | No                      |
| `.size`               | Yes                            | Yes                 | No                      | No                      |
| GC                    | Strong refs — keeps keys alive | Strong refs         | Weak refs on **keys**   | Weak refs on **values** |
| Serialization         | No `JSON.stringify`            | No `JSON.stringify` | No                      | No                      |

**Thoughts:**

Use `Map`/`Set` when you need collection semantics (any key type, uniqueness, `.size`, iteration). Use `WeakMap`/`WeakSet` for **side metadata** attached to objects without preventing those objects from being collected.

**Decision guide:**

```text
Need primitives as keys/values?     → Map / Set
Need to loop or know .size?          → Map / Set
Attach hidden data to objects?       → WeakMap
Tag "already seen" object refs?      → WeakSet
Prevent GC of keys/values?           → Map / Set (strong refs)
Let GC reclaim when object unused?   → WeakMap / WeakSet
```

Deep dives → [Map](#map) · [Set](#set) · [WeakMap](#weakmap) · [WeakSet](#weakset).

#### Common pitfalls

```js
// Pitfall 1: object keys in Map — must keep same reference
const key = { id: 1 };
const map = new Map([[key, "a"]]);
map.get({ id: 1 }); // undefined — different object
map.get(key); // 'a'

// Pitfall 2: WeakMap key collected → entry gone (by design)
let obj = {};
const wm = new WeakMap();
wm.set(obj, "data");
obj = null; // eventually GC removes the entry

// Pitfall 3: Set dedupe uses SameValueZero, not deep equality
new Set([{ a: 1 }, { a: 1 }]).size; // 2
```

**Sources:** [javascript.info/map-set](https://javascript.info/map-set) · [javascript.info/weakmap-weakset](https://javascript.info/weakmap-weakset) · [MDN Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

</details>

<a name="map"></a>

<details>
  <summary>Map</summary>

### Map

**Definition:** Ordered collection of `[key, value]` entries. Keys can be any type — primitives, objects, functions, symbols — not just strings/symbols like plain objects.

**How it works:**

- Created with `new Map()` or `new Map(iterable)` where iterable yields `[key, value]` pairs.
- Key lookup uses **SameValueZero** equality (`NaN` equals `NaN`; `+0` equals `-0`).
- Insertion order is preserved; `for...of` iterates entries in that order.
- API: `set`, `get`, `has`, `delete`, `clear`, `size`, `keys`, `values`, `entries`, `forEach`.
- Object keys are stored **by reference**, not by stringified form — two different `{id:1}` objects are two different keys.

**When to use:**

| Use `Map`                                                             | Use plain `Object`                             |
| --------------------------------------------------------------------- | ---------------------------------------------- |
| Keys are objects, DOM nodes, or non-strings                           | JSON serialization / REST DTO                  |
| Frequent add/delete of entries                                        | Fixed shape config literals                    |
| Need `.size` or insertion-order iteration                             | Need `Object.keys` / spread ergonomics         |
| Keys should not collide via `toString()` (`{}` → `"[object Object]"`) | Prototype methods or `hasOwnProperty` patterns |

**Pluses and minuses:**

| +                                                      | −                                                                |
| ------------------------------------------------------ | ---------------------------------------------------------------- |
| Any key type (objects, functions)                      | No JSON serialization                                            |
| Predictable iteration order                            | Slightly more memory than plain object for string-key-only cases |
| `.size` in O(1)                                        | No prototype chain / no inherited keys                           |
| Frequent `set`/`delete` without key-stringify overhead | Less familiar to readers expecting `{}`                          |

**Examples:**

```js
const user = { id: 1 };
const meta = new Map([
  ["name", "Ann"],
  [user, { lastLogin: Date.now() }], // object as key
]);

meta.set(42, "answer");
meta.get(user); // { lastLogin: ... }
meta.has("name"); // true
meta.size; // 3

for (const [key, value] of meta) {
  console.log(key, value);
}

// SameValueZero
const m = new Map();
m.set(NaN, "not a number");
m.get(NaN); // 'not a number'
```

</details>

<a name="set"></a>

<details>
  <summary>Set</summary>

### Set

**Definition:** Ordered collection of **unique** values. Adding a duplicate is a no-op (same value per SameValueZero).

**How it works:**

- Created with `new Set()` or `new Set(iterable)`.
- `add`, `has`, `delete`, `clear`, `size`, iteration via `for...of` / `values()` (same as entries for a set).
- Uniqueness is by value (SameValueZero), not by reference identity for primitives.
- Two different object references are always distinct entries.

**When to use:**

| Use `Set`                                  | Use `Array`                                  |
| ------------------------------------------ | -------------------------------------------- |
| Uniqueness (`[...new Set(arr)]`)           | Order + duplicates matter                    |
| Fast membership checks in a loop           | Need `map`/`filter`/`reduce` on indexed data |
| Tag lists, visited nodes, permission flags | JSON-serializable list                       |

**Pluses and minuses:**

| +                                     | −                                  |
| ------------------------------------- | ---------------------------------- |
| O(1) average `has` / `add` / `delete` | No index access (`set[0]` invalid) |
| Deduplication in one line             | No key→value mapping               |
| Iterable, spread-friendly             | Objects compared by reference only |

**Examples:**

```js
const tags = new Set(["js", "react", "js"]); // {'js', 'react'}
tags.add("ts");
tags.has("js"); // true
tags.delete("react");

// Deduplicate array
const unique = [...new Set([1, 2, 2, 3])]; // [1, 2, 3]

// Set operations
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);
const intersection = [...a].filter((x) => b.has(x)); // [2, 3]

// Objects: reference identity
const s = new Set();
s.add({ id: 1 });
s.add({ id: 1 });
s.size; // 2 — two different object references
```

</details>

<a name="weakmap"></a>

<details>
  <summary>WeakMap</summary>

### WeakMap

**Definition:** Map-like structure whose **keys must be objects** (or registered symbols in some engines). Keys are held **weakly**: if the key object has no other strong references, it and its entry can be garbage-collected. Values are held strongly.

**How it works:**

- Only methods: `get`, `set`, `has`, `delete` — no `size`, `clear`, or iteration.
- You cannot list keys — the API is intentionally minimal so GC can run without exposing dead keys.
- Typical pattern: private/side data on objects you do not own (DOM nodes, class instances).
- If the key is collected, `get(key)` would not be callable anyway; the entry disappears silently.

**When to use:**

| Use `WeakMap`                                           | Use `Map` instead                |
| ------------------------------------------------------- | -------------------------------- |
| Cache or metadata keyed by object identity              | Need to iterate entries          |
| DOM node → handler state (browser)                      | Keys include primitives          |
| Private fields pattern before `#` or `Symbol`           | Need `.size` or `clear()`        |
| Instance → computed cache that should die with instance | Long-lived registry of all items |

**Pluses and minuses:**

| +                                                           | −                                            |
| ----------------------------------------------------------- | -------------------------------------------- |
| No memory leaks from cached metadata on short-lived objects | Cannot iterate or get `.size`                |
| Good for libraries attaching data to foreign objects        | Keys must be objects                         |
| Hides data from `Object.keys` / serialization               | Cannot inspect contents for debugging easily |

**Examples:**

```js
const privateData = new WeakMap();

class User {
  constructor(name) {
    privateData.set(this, { name, visits: 0 });
  }
  getName() {
    return privateData.get(this).name;
  }
  visit() {
    privateData.get(this).visits++;
  }
}

const u = new User("Ann");
u.visit();
// When `u` is no longer referenced, the WeakMap entry is GC-eligible.

// DOM metadata (browser)
const listeners = new WeakMap();
function track(el, handler) {
  listeners.set(el, handler);
  // When `el` is removed from DOM and unreferenced, entry can be collected.
}
```

</details>

<a name="weakset"></a>

<details>
  <summary>WeakSet</summary>

### WeakSet

**Definition:** Set-like structure that only accepts **objects**. Membership is a weak reference — if an object has no other strong refs, it drops out of the set automatically.

**How it works:**

- Methods: `add`, `has`, `delete` — no iteration, no `size`, no `clear`.
- `add` returns the set (like `Set`).
- Useful as a **tag set**: "have we already processed this object?"

**When to use:**

| Use `WeakSet`                              | Use `Set` instead                  |
| ------------------------------------------ | ---------------------------------- |
| Mark objects as visited/processed          | Store primitives                   |
| Opt-out / blocklist of object instances    | Need to iterate or count members   |
| Prevent double-registration of same object | Persist membership across sessions |

**Pluses and minuses:**

| +                                              | −                            |
| ---------------------------------------------- | ---------------------------- |
| Automatic cleanup when objects go away         | No way to list members       |
| Cheap "seen this object?" checks               | Objects only                 |
| Avoids leaking memory in long-lived registries | Cannot serialize or snapshot |

**Examples:**

```js
const processed = new WeakSet();

function handleOnce(obj) {
  if (processed.has(obj)) return;
  processed.add(obj);
  // ... expensive work
}

const node = document.createElement("div");
handleOnce(node);
handleOnce(node); // skipped

// Circular structures — WeakSet detects revisiting same object
function deepWalk(obj, seen = new WeakSet()) {
  if (obj && typeof obj === "object") {
    if (seen.has(obj)) return;
    seen.add(obj);
    for (const v of Object.values(obj)) deepWalk(v, seen);
  }
}
```

</details>

## Classic structures

### Classic structures

<a name="classic-structures-in-short"></a>

<details>
  <summary>Classic structures in short</summary>

### Classic structures in short

**Definition:** Beyond built-in `Object` / `Map` / `Set`, most **abstract** structures (tree, graph, list, stack, queue) are **patterns you build** from objects, arrays, and references. JS has no `Tree` or `Graph` class in the language — you model nodes and edges yourself.

**How it works:**

| Structure       | Idea                      | Typical JS shape                                | In the wild                           |
| --------------- | ------------------------- | ----------------------------------------------- | ------------------------------------- |
| **Tree**        | Hierarchy, no cycles      | `{ val, children[] }` or `{ val, left, right }` | DOM, JSON config, React element trees |
| **Graph**       | Nodes + arbitrary edges   | `Map`/`Record` adjacency list                   | Module graph, deps, social graph      |
| **Linked list** | Linear chain via pointers | `{ val, next }` / doubly `{ prev, next }`       | LRU internals, interview patterns     |
| **Stack**       | LIFO                      | `array.push` / `pop`                            | undo, DFS (explicit stack)            |
| **Queue**       | FIFO                      | array + head index, or linked nodes             | BFS, job queues                       |

Arrays already cover **indexed lists** → [JS Arrays](./3_js_array.md). Full complexity tables and interview drills → [Linear structures (CS)](./7_special_data_structures_linear.md) · [Trees & graphs (CS)](./7_special_data_structures_trees.md).

Deep dives in this file → [Trees](#trees) · [Graphs](#graphs) · [Linked lists](#linked-lists) · [Stack and queue](#stack-and-queue).

**Sources:** [Choosing a data structure](./7_special_data_structures.md#choosing-a-data-structure) · [javascript.info/array](https://javascript.info/array)

</details>

<a name="trees"></a>

<details>
  <summary>Trees</summary>

### Trees

**Definition:** A **tree** is a hierarchical structure: nodes with parent/child links and **no cycles**. A **binary tree** has at most two children per node (`left` / `right`).

**How it works:**

- **N-ary tree:** `{ value, children: [...] }` — file trees, nested menus, AST-like shapes.
- **Binary tree:** `{ val, left, right }` with `null` for missing child — BST, heap as array (see CS track).
- **Traversal:** depth-first (pre/in/post-order) uses recursion or an explicit **stack**; breadth-first uses a **queue**.
- **Browser:** the DOM is a live tree (`parentNode`, `childNodes`) → [DOM tree](./3_js_browser.md#dom-tree).
- Deep trees: prefer iterative DFS or guard recursion — [call stack limits](./3_js_data_types.md#memory-stack-heap).

**When to use:** Nested UI, folder pickers, expression trees, decision trees. Balanced BST / trie theory → [Tree (CS)](./7_special_data_structures_trees.md#tree).

**Examples:**

```js
const binary = {
  val: 2,
  left: { val: 1, left: null, right: null },
  right: { val: 3, left: null, right: null },
};

function walk(node, visit) {
  if (!node) return;
  visit(node.val);
  walk(node.left, visit);
  walk(node.right, visit);
}
```

**Sources:** [Tree (CS)](./7_special_data_structures_trees.md#tree) · [Tree traversal (CS)](./7_special_data_structures_trees.md#tree-traversal-recursive-vs-iterative)

</details>

<a name="graphs"></a>

<details>
  <summary>Graphs</summary>

### Graphs

**Definition:** A **graph** is vertices (nodes) plus **edges** between them. Edges can be directed or undirected; graphs may contain cycles (unlike trees).

**How it works:**

- **Adjacency list (usual in JS):** `Map<node, neighbor[]>` or `Record<string, string[]>`.
- **Adjacency matrix:** `boolean[][]` or weight matrix — rare in everyday JS unless dense and small.
- **Traversal:** BFS (queue) for shortest unweighted paths; DFS (stack/recursion) for connectivity, cycle detection, topological sort.
- **Module bundlers** build a dependency **graph** from `import` edges.

**When to use:** Routing, recommendations, state machines, package/deps graphs. Representations and pitfalls → [Graph (CS)](./7_special_data_structures_trees.md#graph).

**Examples:**

```js
const graph = new Map([
  ["A", ["B", "C"]],
  ["B", ["D"]],
  ["C", []],
  ["D", []],
]);

function bfs(start, visit) {
  const q = [start];
  const seen = new Set([start]);
  while (q.length) {
    const v = q.shift(); // for hot paths use index queue — see Stack and queue
    visit(v);
    for (const n of graph.get(v) ?? []) {
      if (!seen.has(n)) {
        seen.add(n);
        q.push(n);
      }
    }
  }
}
```

**Sources:** [Graph (CS)](./7_special_data_structures_trees.md#graph) · [Graph Theory — Interview](./7_algo_graphs_interview.md)

</details>

<a name="linked-lists"></a>

<details>
  <summary>Linked lists</summary>

### Linked lists

**Definition:** A **linked list** chains nodes where each node holds a value and a reference to the **next** node (doubly linked: `prev` + `next`). No O(1) random access by index — walk from `head`.

**How it works:**

- **Singly linked:** `{ val, next }`; keep `head` (and sometimes `tail` for append).
- **Doubly linked:** `{ val, prev, next }` — O(1) removal given a node reference.
- **vs Array:** insert/delete at a known node is O(1) for lists; arrays win on indexing and cache locality.
- JS **arrays** are the default list; linked lists appear in interviews and some library internals (LRU).

**When to use:** Interview patterns, custom caches, when you need stable node handles with O(1) splice. Full ops table → [Linked list (CS)](./7_special_data_structures_linear.md#linked-list).

**Examples:**

```js
function append(head, val) {
  const node = { val, next: null };
  if (!head) return node;
  let cur = head;
  while (cur.next) cur = cur.next;
  cur.next = node;
  return head;
}
```

**Sources:** [Linked list (CS)](./7_special_data_structures_linear.md#linked-list) · [Linked Lists — Interview](./7_ds_linked_lists_interview.md)

</details>

<a name="stack-and-queue"></a>

<details>
  <summary>Stack and queue</summary>

### Stack and queue

**Definition:** A **stack** is LIFO (last in, first out); a **queue** is FIFO (first in, first out). In JS both are usually implemented with **arrays** or **linked nodes**.

**How it works:**

| ADT   | Array pattern           | Gotcha in JS                                           |
| ----- | ----------------------- | ------------------------------------------------------ |
| Stack | `push` / `pop` at end   | O(1) amortized at array end                            |
| Queue | `push` tail + read head | `shift()` is **O(n)** — use head index or linked queue |

- **Deque / double-ended:** `push`/`pop` both ends — still array-friendly.
- **Priority queue:** not built-in — heap in [Hash & heap (CS)](./7_special_data_structures_advanced.md).
- Stacks underpin DFS and undo; queues underpin BFS and async job ordering.

**When to use:** Parsing, undo stacks, BFS/DFS templates. Details → [Stack](./7_special_data_structures_linear.md#stack) · [Queue](./7_special_data_structures_linear.md#queue).

**Examples:**

```js
// Stack
const stack = [];
stack.push(1);
stack.pop();

// Queue without shift(): keep head index
const queue = [];
let head = 0;
queue.push("a");
queue.push("b");
const item = queue[head++]; // 'a'
```

**Sources:** [Stack (CS)](./7_special_data_structures_linear.md#stack) · [Queue (CS)](./7_special_data_structures_linear.md#queue)

</details>

## Interop

### Interop

<details>
  <summary>Date</summary>

<a name="date"></a>

### Date

**Definition:** `Date` stores a single timestamp — milliseconds since **1 Jan 1970 UTC** (Unix epoch). Local getters/setters use the runtime timezone; `getUTC*` / `setUTC*` use UTC.

**How it works:**

- Create: `new Date()` (now), `new Date(ms)`, `new Date(dateString)` (same algorithm as `Date.parse`), `new Date(y, monthIndex, …)` — **month is 0–11**.
- Read: `getFullYear`, `getMonth` (0–11), `getDate` (day of month 1–31), `getDay` (weekday 0=Sun…6=Sat), `getHours`…, `getTime()`, `getTimezoneOffset()`.
- Write: `setFullYear` / `setMonth` / `setDate` / `setHours`… / `setTime(ms)` — out-of-range values **autocorrect** (e.g. Jan 32 → Feb 1).
- Diff: `date1 - date2` or `getTime()` difference → milliseconds. `Date.now()` is current timestamp without allocating a `Date`.
- Prefer `getFullYear()` — never use deprecated `getYear()`.

**When to use:** Timestamps, elapsed time, simple calendar math via autocorrection. Display → `Intl.DateTimeFormat` ([Intl](./3_js_extra.md#intl)). Complex calendars / time zones → consider Temporal (when available) or a dedicated library.

#### Common pitfalls

```js
new Date(2011, 0, 1); // 1 Jan 2011 local — month 0 = January
new Date("2017-01-26"); // date-only → UTC midnight, then shown in local TZ
date.getDate(); // day of month — not getDay (weekday)
date.setDate(0); // last day of previous month
```

**Pluses and minuses:**

| +                          | −                                              |
| -------------------------- | ---------------------------------------------- |
| Built-in, ms precision     | Month 0-based; date-string parsing quirks      |
| Autocorrection for offsets | Mutable; mutating shared dates is a bug source |
| Easy elapsed-time diffs    | No first-class TZ arithmetic                   |

**Examples:**

```js
const start = Date.now();
// …work…
const elapsedMs = Date.now() - start;

const d = new Date(2016, 1, 28);
d.setDate(d.getDate() + 2); // 1 Mar 2016 (leap year)

Date.parse("2017-01-26"); // ms or NaN
```

**Thoughts:** _Epoch ms is the portable value; `Date` is a view with local/UTC accessors — month index and mutable APIs are the interview traps._

**Sources:** [javascript.info/date](https://javascript.info/date) · [MDN Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) · [MDN Date.parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)

</details>

<details>
  <summary>JSON</summary>

### JSON

**Definition:** **JSON** (JavaScript Object Notation) is the most common text format for exchanging data between apps. It looks like a JS object, but it is a language-independent spec ([ECMA-404](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)); how JS uses it is in ECMAScript. Files use the `.json` extension (`package.json` is the best-known example).

**How it works:**

JSON is key–value pairs separated by `,`. A key is always a **double-quoted string**; `:` separates key and value. Values may be:

| Type    | Example              |
| ------- | -------------------- |
| String  | `"I love JSON!"`     |
| Number  | `21`                 |
| Boolean | `true` / `false`     |
| Array   | `[18, true, "lost"]` |
| Object  | `{"isValid": true}`  |
| `null`  | `null`               |

Typical network flow: request → wait → receive JSON text → `JSON.parse` → use JS values. `fetch` helpers: `await res.json()`.

**`JSON.stringify(value[, replacer, space])`** — JS value → JSON **string**. Result type is `'string'` (or `undefined` for a lone function/`undefined`/`Symbol`).

Omitted (object properties): `undefined`, functions/methods, `Symbol` values, **Symbol keys** (even with `replacer`). Only **enumerable own** properties are visited (`Object.keys` order).

Become `null`: `Infinity`, `NaN`; `undefined` **inside arrays**. `Map` / `Set` stringify to `{}` (no enumerable own data keys).

Throws **`TypeError`:** circular references, `BigInt` (unless you add `toJSON`). `Date` / `Temporal` use `toJSON()` (ISO string); parse does **not** revive them as dates.

`replacer` — array of keys (only those properties) or `(key, value) =>` transform; return `undefined` to skip. `space` — number of spaces (clamped to 10) or a string indent for readability (`JSON.stringify(obj, null, 2)`). Custom `toJSON()` on an object overrides serialization.

**`JSON.parse(text[, reviver])`** — JSON string → JS value. Invalid JSON → **`SyntaxError`**. Wrap untrusted input in `try/catch`. `reviver(key, value)` runs depth-first; return `undefined` to delete a property. Use it to revive `Date` from ISO strings.

**When to use:**

REST/API payloads, `localStorage` strings, npm `package.json` and other config, app state sent to a backend. Alternatives: **XML**, **YAML** (comments), Protobuf/MessagePack (size). Deep-clone JSON-safe data with a round-trip; use `structuredClone` for cycles/`Date`/`Map`. JSON5 exists as a looser dialect — not the language standard.

#### Common pitfalls

- Keys/strings must use **double quotes**; trailing commas and JS comments are invalid.
- `undefined` in an object is dropped; in an array it becomes `null`.
- `JSON.parse(JSON.stringify(obj))` loses functions, `undefined`, symbols, turns `Date` into a string, throws on cycles.
- `JSON.parse` does not run JS — no `new Date(...)` inside the text.
- Network APIs (e.g. OpenWeather) return JSON; still parse/validate at the boundary.

**Pluses and minuses:**

| +                              | −                                                     |
| ------------------------------ | ----------------------------------------------------- |
| Universal API / config format  | Not a full clone (types, functions, `undefined` lost) |
| Built-in, fast, human-readable | Circular refs / `BigInt` throw                        |
| Pretty-print with `space`      | No comments in standard JSON (`//` is invalid)        |
| Most languages have libraries  | No schema validation built-in                         |

**Examples:**

```js
const hero = {
  nickname: "BestHealerEver",
  level: 7,
  things: ["sword", "helmet"],
  money: { gold: 6387, diamonds: 2 },
};

typeof hero; // 'object'
typeof JSON.stringify(hero); // 'string'
JSON.stringify(hero, ["nickname", "level"], 2);
// pretty JSON with only those keys

const json = '{"name":"Luke Skywalker","gender":"male","birth_year":"19BBY"}';
const jedi = JSON.parse(json);
jedi.name; // 'Luke Skywalker'

JSON.stringify({ fn() {}, n: NaN, u: undefined }); // '{"n":null}'
JSON.stringify([undefined]); // '[null]'
JSON.parse("{ invalid"); // SyntaxError
```

**Thoughts:** _JSON is a strict data subset of JS object syntax — stringify to send, parse to use, and never treat it as a full object clone._

**Sources:** [Doka — JSON](https://doka.guide/js/json/) · [javascript.info/json](https://javascript.info/json) · [MDN stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) · [MDN parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

</details>
