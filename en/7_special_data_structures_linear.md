# Data Structures — Linear

Arrays, objects, linked lists, stacks, queues, sets, and maps.

[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Choosing a data structure](./7_special_data_structures.md#choosing-a-data-structure) · [Linear](./7_special_data_structures_linear.md) · [Trees & graphs](./7_special_data_structures_trees.md) · [Hash & heap](./7_special_data_structures_advanced.md) · [Algorithms — Big O](./7_special_algorithms_core.md) · [JavaScript — Document](./3_js_browser.md)

### Sections

- [Linear structures](#linear-structures)
- [Array](#array)
- [Object (plain object)](#object-plain-object)
- [Linked list](#linked-list)
- [Stack](#stack)
- [Queue](#queue)
- [Set](#set)
- [Map](#map)

---

## Linear structures

<details>
  <summary>Array</summary>

### Array

**Definition:** Ordered collection with O(1) random access by index.

**When to use:** Lists, buffers, stack/queue (if you know shift cost).

**Pluses and minuses:**

| +                          | −                          |
| -------------------------- | -------------------------- |
| Cache-friendly, fast index | Insert/delete middle O(n)  |
| Versatile in JS            | `shift`/`unshift` are O(n) |

**Examples:**

`const arr = [1, 2, 3]; arr.push(4);`

| Operation       | Time |
| --------------- | ---- |
| Access by index | O(1) |
| Push/pop end    | O(1) |
| Shift/unshift   | O(n) |
| Search unsorted | O(n) |

---

</details>

<details>
  <summary>Object (plain object)</summary>

### Object (plain object)

**Definition:** Unordered key-value map (string/symbol keys). V8 may optimize as hidden class or dictionary mode.

**When to use:** Fixed-shape records, DTOs, namespaces. Prefer `Map` for dynamic keys.

**Pluses and minuses:**

| +                            | −                                       |
| ---------------------------- | --------------------------------------- |
| Literal syntax, JSON-like    | Key order quirks (mostly insertion now) |
| Prototype chain for defaults | Not ideal for frequent add/delete keys  |

**Examples:**

```js
const user = { name: "Ada", role: "admin" };
```

---

</details>

<details>
  <summary>Linked list</summary>

### Linked list

**Definition:** Nodes with `value` + `next` pointer. Doubly linked adds `prev`.

**When to use:** LRU cache internals, undo chains, interview problems. Rare in app JS — arrays usually win.

**Pluses and minuses:**

| +                                | −                    |
| -------------------------------- | -------------------- |
| O(1) insert/delete at known node | O(n) access by index |
| No reindexing on insert          | Extra pointer memory |
|                                  | Poor cache locality  |

**Examples:**

```js
class Node {
  constructor(value, next = null) {
    this.value = value;
    this.next = next;
  }
}
```

---

</details>

<details>
  <summary>Stack</summary>

### Stack

**Definition:** LIFO — last in, first out.

**When to use:** Call stack, bracket matching, DFS iterative, browser history (back).

**Pluses and minuses:**

| +                         | −                |
| ------------------------- | ---------------- |
| O(1) push/pop (array end) | No random access |
| Simple undo, parsing      |                  |

**Examples:**

```js
const stack = [];
stack.push(1);
stack.push(2);
stack.pop(); // 2
```

---

</details>

<details>
  <summary>Queue</summary>

### Queue

**Definition:** FIFO — first in, first out.

**When to use:** BFS, job queues, event loops (macro/micro task queues).

**Pluses and minuses:**

| +                    | −                                       |
| -------------------- | --------------------------------------- |
| Fair ordering        | JS array `shift` is O(n)                |
| BFS, task scheduling | Use ring buffer or linked list for O(1) |

**Examples:**

```js
const queue = [];
queue.push("a");
queue.push("b");
queue.shift(); // 'a' — O(n); use index for hot paths
```

---

</details>

<details>
  <summary>Set</summary>

### Set

**Definition:** Collection of **unique** values.

**When to use:** Unique tags, visited nodes in graph, permission sets.

**Pluses and minuses:**

| +                           | −                           |
| --------------------------- | --------------------------- |
| O(1) has/add/delete average | No index access             |
| Dedup, membership           | Iteration order = insertion |

**Examples:**

```js
const seen = new Set([1, 2, 2, 3]); // Set(3) {1,2,3}
seen.has(2); // true
```

---

</details>

<details>
  <summary>Map</summary>

### Map

**Definition:** Key-value store; keys can be any type (objects, primitives).

**When to use:** Dynamic keys, object keys, counting frequencies, caches.

**Pluses and minuses:**

| +                         | −                                     |
| ------------------------- | ------------------------------------- |
| O(1) average get/set      | More memory than object for tiny maps |
| Maintains insertion order |                                       |
| No prototype pollution    |                                       |

**Examples:**

```js
const counts = new Map();
for (const w of words) counts.set(w, (counts.get(w) ?? 0) + 1);
```

---

</details>
