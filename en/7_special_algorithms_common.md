# Algorithms — Common

Sorting, search, graph traversal, and classic algorithm choices.

[← Back to hub](./7_special_algorithms.md)

### Related docs

* [Algorithms — Big O](./7_special_algorithms_core.md) · [Algorithms — Common](./7_special_algorithms_common.md) · [Algorithms — Patterns](./7_special_algorithms_patterns.md) · [Data structures](./7_special_data_structures.md)

### Sections

- [Common algorithms](#common-algorithms)

---

## 6. Common algorithms

<details>
  <summary>Algorithm reference table</summary>

### Algorithm reference table

**Definition:**

Quick-reference complexity and use-case guide for common algorithms — use when picking or reviewing an approach.

| Algorithm | Time (typical) | Space | When to use |
|-----------|----------------|-------|-------------|
| **Linear search** | O(n) | O(1) | Unsorted small lists |
| **Binary search** | O(log n) | O(1) | Sorted array lookup |
| **Bubble sort** | O(n²) | O(1) | Never in production — teaching only |
| **Merge sort** | O(n log n) | O(n) | Stable sort, linked lists |
| **Quick sort** | O(n log n) avg | O(log n) stack | General in-place sort |
| **BFS** | O(V + E) | O(V) | Shortest path unweighted, level-order |
| **DFS** | O(V + E) | O(V) | Cycle detect, topological sort, maze |
| **Dijkstra** | O((V+E) log V) | O(V) | Shortest path weighted, non-negative edges |
| **Dynamic programming** | Problem-specific | Table size | Overlapping subproblems, optimal substructure |
| **Greedy** | Often O(n log n) | O(1)–O(n) | Local optimal → global optimal (when provable) |

---

</details>

<details>
  <summary>Binary search</summary>

### Binary search

**Definition:** Repeatedly halve a **sorted** array to find a target.

**When to use:** Sorted arrays, search space reduction, "find first/last position" variants.

**Pluses and minuses:**

| + | − |
|------|------|
| O(log n) lookup | Requires sorted data |
| Simple to implement | Off-by-one errors common |

**Examples:**

```js
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = (lo + hi) >> 1;
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}
```

**Thoughts:**

Template works for "minimize/maximize answer" binary search on answer space too.

---

</details>

<details>
  <summary>Merge sort</summary>

### Merge sort

**Definition:** Divide array in half, sort recursively, merge sorted halves.

**When to use:** Need stable sort; external sorting; linked lists.

**Pluses and minuses:**

| + | − |
|------|------|
| Stable O(n log n) guaranteed | O(n) extra space |
| Predictable | Slower constants than quick sort |

**Examples:**

```js
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  const mid = arr.length >> 1;
  return merge(mergeSort(arr.slice(0, mid)), mergeSort(arr.slice(mid)));
}
function merge(a, b) {
  const out = [];
  let i = 0, j = 0;
  while (i < a.length && j < b.length)
    out.push(a[i] <= b[j] ? a[i++] : b[j++]);
  return out.concat(a.slice(i), b.slice(j));
}
```

---

</details>

<details>
  <summary>Quick sort</summary>

### Quick sort

**Definition:** Divide-and-conquer sort — pick a **pivot**, partition array into elements ≤ pivot and > pivot, recurse on both partitions.

**How it works:**

1. Choose pivot (first, last, random, or median-of-three).
2. **Partition** — scan with two pointers; swap so smaller elements are left of pivot.
3. Recurse on left and right subarrays until length ≤ 1.
4. Average depth O(log n); each level does O(n) work → **O(n log n)** average.

**When to use:** General in-memory sort when stability not required; interview staple. `Array.prototype.sort` is implementation-defined (often Timsort or quicksort hybrid). Use merge sort when stability matters.

**Pluses and minuses:**

| + | − |
|------|------|
| In-place — O(log n) stack space | O(n²) worst with bad pivot (sorted input + first pivot) |
| Fast average; cache-friendly | Unstable — equal elements may reorder |
| Standard library choice in many langs | Recursion depth on huge arrays |

**Examples:**

```js
function quickSort(arr, lo = 0, hi = arr.length - 1) {
  if (lo >= hi) return arr;
  const pivot = arr[hi];
  let i = lo;
  for (let j = lo; j < hi; j++) {
    if (arr[j] <= pivot) [arr[i], arr[j]] = [arr[j], arr[i++]];
  }
  [arr[i], arr[hi]] = [arr[hi], arr[i]];
  quickSort(arr, lo, i - 1);
  quickSort(arr, i + 1, hi);
  return arr;
}
```

**Thoughts:** Random pivot or introsort (switch to heap sort at depth limit) avoids worst case. Lomuto vs Hoare partition — know one well for interviews.

**Sources:**

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

---

</details>

<details>
  <summary>BFS & DFS</summary>

### BFS & DFS

**Definition:**

- **BFS** — explore neighbors level by level (queue)
- **DFS** — explore depth-first (stack or recursion)

| | BFS | DFS |
|---|-----|-----|
| Structure | Queue | Stack / recursion |
| Shortest path (unweighted) | Yes | No |
| Memory | Can be high on wide graphs | O(depth) recursion |

**When to use:**

- BFS: shortest path, level-order tree traversal
- DFS: cycle detection, topological sort, path existence

**Examples:**

```js
function bfs(graph, start) {
  const visited = new Set([start]);
  const queue = [start];
  while (queue.length) {
    const node = queue.shift();
    for (const neighbor of graph[node] ?? [])
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
  }
  return visited;
}
```

**Thoughts:**

`queue.shift()` is O(n) in JS arrays — use index pointer or deque for production graph code.

---

</details>

<details>
  <summary>Dynamic programming</summary>

### Dynamic programming

**Definition:** Solve by breaking into overlapping subproblems, storing results to avoid recomputation. Requires **optimal substructure** and **overlapping subproblems**.

**When to use:** Knapsack, LCS, edit distance, coin change, grid paths.

**Pluses and minuses:**

| + | − |
|------|------|
| Polynomial time for hard problems | State design is non-trivial |
| Memoization / tabulation patterns | Space for DP table |

**Examples:**

```js
function fib(n, memo = {}) {
  if (n <= 1) return n;
  if (memo[n] != null) return memo[n];
  memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
  return memo[n];
}
```

**Thoughts:**

Top-down (memo) vs bottom-up (tabulation). Identify state: `dp[i][j]` meaning.

---

</details>

<details>
  <summary>Greedy algorithms</summary>

### Greedy algorithms

**Definition:** At each step, pick the **locally best** option without revisiting past choices, aiming for a global optimum.

**How it works:**

1. Sort or prioritize candidates by a greedy criterion (earliest end, smallest weight, etc.).
2. Iterate — accept choice if it doesn't violate constraints.
3. Never backtrack — decision is final once made.
4. Works only when **greedy choice property** holds: local optimum leads to global optimum (prove via exchange argument or induction).

**When to use:** Activity selection, Huffman coding, interval scheduling, fractional knapsack, Dijkstra (non-negative edges), minimum spanning tree (Kruskal/Prim). **Not** for 0/1 knapsack or coin change with arbitrary denominations — use DP.

**Pluses and minuses:**

| + | − |
|------|------|
| Often O(n log n) or O(n) — simple and fast | Not always optimal — wrong criterion fails silently |
| Low memory — no DP table | Requires proof; easy to pick wrong heuristic |
| Readable code | Some problems need DP for all optimal solutions |

**Examples:**

```js
function maxActivities(intervals) {
  intervals.sort((a, b) => a.end - b.end);
  let count = 0, lastEnd = -Infinity;
  for (const { start, end } of intervals) {
    if (start >= lastEnd) { count++; lastEnd = end; }
  }
  return count;
}
```

**Thoughts:** If greedy fails, try DP. Interview: always state **why** greedy works (exchange argument: swapping greedy choice for any other doesn't worsen result).

**Sources:**

[javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)

---

</details>
