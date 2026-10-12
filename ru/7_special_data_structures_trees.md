# Data Structures — Trees & graphs

Trees, traversal, graphs, and tries.

[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Choosing a data structure](./7_special_data_structures.md#choosing-a-data-structure) · [Linear](./7_special_data_structures_linear.md) · [Trees & graphs](./7_special_data_structures_trees.md) · [Hash & heap](./7_special_data_structures_advanced.md) · [Algorithms — Big O](./7_special_algorithms_core.md) · [JavaScript — Document](./3_js_browser.md)

### Sections

- [Tree](#tree)
- [Tree traversal (recursive vs iterative)](#tree-traversal-recursive-vs-iterative)
- [Graph](#graph)
- [Trie](#trie)

---

## Trees and graphs

<details>
  <summary>Tree</summary>

### Tree

**Definition:** Hierarchical nodes; no cycles. Binary tree: ≤2 children per node.

**When to use:** DOM, file systems, decision trees, BST maps.

**Pluses and minuses:**

| +                            | −                                |
| ---------------------------- | -------------------------------- |
| Models hierarchy (DOM, FS)   | Balancing needed for performance |
| O(log n) ops if balanced BST | Unbalanced → O(n)                |

**Examples:**

```js
const node = {
  value: "root",
  children: [
    { value: "left", children: [] },
    { value: "right", children: [] },
  ],
};
```

**Thoughts:**

DOM is a tree. React fiber tree is a linked tree structure. Traversal → next block.

---

</details>

<details>
  <summary>Tree traversal (recursive vs iterative)</summary>

### Tree traversal (recursive vs iterative)

**Definition:** Visit every node of a tree (or forest). Depth-first uses a **stack** (call stack or explicit); breadth-first uses a **queue**.

**How it works:**

- **Recursive DFS:** visit node, then walk each child. Skip `null` / missing children so you do not push junk values.
- **Iterative DFS:** explicit `stack`. Push children in reverse if you want left-to-right order.
- **Call stack:** each recursive frame costs stack space. Deep trees (10k+ levels) can throw `RangeError: Maximum call stack size exceeded`. Production code often uses an iterative stack or a depth limit.
- **React:** nested menus, comment threads, file trees — recursion is natural for _shallow_ UI trees. Prefer iteration or flattening if depth is unbounded (user-generated nesting).

**When to use:**

Interview “print all values”; React recursive `<Folder children={...} />`; DOM walk.

#### Common pitfalls

Forgetting `null` checks and collecting `undefined` in the result. Recursing on production data with unknown depth.

**Pluses and minuses:**

| +                                      | −                            |
| -------------------------------------- | ---------------------------- |
| Recursion matches the data shape       | Stack overflow on deep trees |
| Iterative stack has heap-limited depth | More boilerplate             |

**Examples:**

```js
function walk(node, visit) {
  if (node == null) return;
  visit(node.value);
  for (const child of node.children ?? []) walk(child, visit);
}

function walkIterative(root, visit) {
  const stack = [root];
  while (stack.length) {
    const node = stack.pop();
    if (node == null) continue;
    visit(node.value);
    const children = node.children ?? [];
    for (let i = children.length - 1; i >= 0; i--) stack.push(children[i]);
  }
}
```

**Thoughts:** _Recursion is the clean DFS; an explicit stack is the same algorithm without blowing the call stack._

**Sources:**

[MDN — recursion](https://developer.mozilla.org/en-US/docs/Glossary/Recursion)

---

</details>

<details>
  <summary>Graph</summary>

### Graph

**Definition:** Vertices + edges. Directed or undirected, weighted or not.

**When to use:** Routing, dependency graphs, recommendation, state machines.

**Pluses and minuses:**

| +                             | −                             |
| ----------------------------- | ----------------------------- |
| Models networks, deps, social | More complex than trees       |
| BFS/DFS foundation            | Representation choice matters |

| Type             | +                                     | −                |
| ---------------- | ------------------------------------- | ---------------- |
| Adjacency list   | Sparse graphs, easy iterate neighbors | Slow edge lookup |
| Adjacency matrix | O(1) edge check                       | O(V²) space      |

**Examples:**

```js
const graph = {
  A: ["B", "C"],
  B: ["A", "D"],
  C: ["A"],
  D: ["B"],
};
```

---

</details>

<details>
  <summary>Trie</summary>

### Trie

**Definition:** Tree where each edge is a character; paths spell strings. Good for prefix operations.

**When to use:** Autocomplete, IP routing tables, word games.

**Pluses and minuses:**

| +                           | −                                |
| --------------------------- | -------------------------------- |
| O(m) search prefix length m | Space-heavy for sparse alphabets |
| Autocomplete, spell check   |                                  |

**Examples:**

```js
class TrieNode {
  constructor() {
    this.children = {};
    this.isEnd = false;
  }
}
```

---

</details>
