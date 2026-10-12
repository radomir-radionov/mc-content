# Data Structures — Hash & heap

Hash tables, heaps, and how the DOM maps to classic structures.

[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Choosing a data structure](./7_special_data_structures.md#choosing-a-data-structure) · [Linear](./7_special_data_structures_linear.md) · [Trees & graphs](./7_special_data_structures_trees.md) · [Hash & heap](./7_special_data_structures_advanced.md) · [Algorithms — Big O](./7_special_algorithms_core.md) · [JavaScript — Document](./3_js_browser.md)

### Sections

- [Hash and heap](#hash-and-heap)
- [Hash table](#hash-table)
- [Heap (priority queue)](#heap-priority-queue)
- [DOM and data structures](#dom-and-data-structures)

---

## Hash and heap

<details>
  <summary>Hash table</summary>

### Hash table

**Definition:** Key-value store that maps keys to **buckets** via a **hash function**; collisions resolved by chaining (linked lists per bucket) or open addressing (probe for next slot).

**How it works:**

1. `hash(key) % bucketCount` → bucket index.
2. **Insert** — store at bucket; on collision, chain or probe.
3. **Lookup** — hash key, search bucket — O(1) average when load factor is low.
4. **Resize** — rehash when load factor exceeds threshold (typically 0.7) to keep O(1) average.
5. Worst case O(n) if all keys collide (adversarial or poor hash).

**When to use:** Caches, deduplication (`Set`), word counts, session stores, database indexes, `Map` for dynamic keys, object property access (V8 hidden classes optimize fixed shapes).

**Pluses and minuses:**

| +                                           | −                                                          |
| ------------------------------------------- | ---------------------------------------------------------- |
| O(1) average lookup, insert, delete         | O(n) worst case with bad hash or attacks                   |
| Ideal for frequency counts, caches, indexes | No inherent ordering (use `Map` for insertion order in JS) |
| JS `Map`/`Set` are hash-based               | Memory overhead for buckets + pointers                     |

**Examples:**

```js
// Frequency count — same pattern as hash table use
const freq = new Map();
for (const word of words) {
  freq.set(word, (freq.get(word) ?? 0) + 1);
}

// Custom hash map concept
class HashMap {
  constructor(size = 16) {
    this.buckets = Array.from({ length: size }, () => []);
  }
  _hash(key) {
    let h = 0;
    for (const c of String(key)) h = (h * 31 + c.charCodeAt(0)) | 0;
    return Math.abs(h) % this.buckets.length;
  }
  set(key, val) {
    const b = this.buckets[this._hash(key)];
    const i = b.findIndex(([k]) => k === key);
    if (i >= 0) b[i][1] = val;
    else b.push([key, val]);
  }
  get(key) {
    return this.buckets[this._hash(key)].find(([k]) => k === key)?.[1];
  }
}
```

**Thoughts:** JS `Map` and `Set` are hash-based. Plain objects use hidden classes for stable shapes; many add/delete keys → dictionary mode (slower). For interview: mention load factor and collision strategies. Table hashes are **not** cryptographic SHA — see [Encryption](./7_special_security.md#encryption).

**Sources:**

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

---

</details>

<details>
  <summary>Heap (priority queue)</summary>

### Heap (priority queue)

**Definition:** Complete binary tree stored in an array where **min-heap** has parent ≤ children (root = minimum) or **max-heap** has parent ≥ children (root = maximum).

**How it works:**

1. **Array representation** — parent at index `i`, children at `2i+1` and `2i+2`.
2. **Insert** — append to end, **bubble up** (swap with parent until heap property holds) — O(log n).
3. **Extract-min** — swap root with last element, remove last, **bubble down** — O(log n).
4. **Peek** — root is min/max — O(1).
5. **Heapify** — build heap from array in O(n) bottom-up.

**When to use:** Dijkstra's algorithm (priority queue), task/job schedulers, merge K sorted lists, find median from stream, event-driven simulation, A\* search.

**Pluses and minuses:**

| +                                   | −                                                 |
| ----------------------------------- | ------------------------------------------------- |
| O(log n) insert and extract-min/max | No O(1) lookup of arbitrary element               |
| O(1) peek at priority element       | Not built into JS — implement or use library      |
| Efficient for streaming top-K       | Array-based heap less intuitive than tree diagram |

**Examples:**

```js
class MinHeap {
  constructor() {
    this.a = [];
  }
  peek() {
    return this.a[0];
  }
  push(val) {
    this.a.push(val);
    let i = this.a.length - 1;
    while (i > 0) {
      const p = (i - 1) >> 1;
      if (this.a[p] <= this.a[i]) break;
      [this.a[p], this.a[i]] = [this.a[i], this.a[p]];
      i = p;
    }
  }
  pop() {
    if (this.a.length === 1) return this.a.pop();
    const min = this.a[0];
    this.a[0] = this.a.pop();
    let i = 0;
    while (true) {
      let s = i,
        l = 2 * i + 1,
        r = 2 * i + 2;
      if (l < this.a.length && this.a[l] < this.a[s]) s = l;
      if (r < this.a.length && this.a[r] < this.a[s]) s = r;
      if (s === i) break;
      [this.a[i], this.a[s]] = [this.a[s], this.a[i]];
      i = s;
    }
    return min;
  }
}
```

**Thoughts:** For top-K largest, use min-heap of size K. `Array.prototype.sort` is O(n log n) for one-shot; heap wins for incremental inserts.

**Sources:**

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

---

</details>

<details>
  <summary>DOM and data structures</summary>

### DOM and data structures

**Definition:**

Mapping between browser DOM concepts and classic data structures — explains traversal APIs, tree shape, and event-loop queues.

| DOM concept        | Data structure       |
| ------------------ | -------------------- |
| Document tree      | Tree                 |
| Parent/child links | Linked structure     |
| Attributes         | Key-value (map-like) |
| childNodes         | Array-like list      |
| classList          | Set-like             |
| Event queue        | Queue (event loop)   |

**Thoughts:** Understanding DOM as a tree explains `querySelector`, traversal APIs, and reflow cost.

**Sources:**

[3_js_browser.md](./3_js_browser.md)

---

</details>
