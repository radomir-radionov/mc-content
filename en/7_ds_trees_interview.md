# Tree Data Structure — Interview

Answers are filled.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Tree Data Structure — Interview](./7_ds_trees_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Tree Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/tree-data-structure-interview-questions/).

### Introduction to Trees

<details>
<summary>What is a Tree Data Structure?</summary>

### What is a Tree Data Structure?

**Definition:**

A tree connects nodes with parent-child edges so that there is exactly one path between any pair and no cycle.

**How it works:**

- One node is the root. Every other node has one parent. Children are the nodes directly below.
- The absence of a cycle is what makes 'the' path from the root unique.
- Recursive structure: a tree is a root plus zero or more trees as children.
- File systems, ASTs, and organization charts are trees. A social graph with mutual friends usually is not.
</details>

<details>
<summary>What is a Binary Tree?</summary>

### What is a Binary Tree?

**Definition:**

A binary tree limits every node to at most two ordered children, left and right.

**How it works:**

- Left and right are distinct. A single child on the left is a different shape from a single child on the right.
- The maximum number of nodes at depth d is 2ᵈ if the root is depth 0.
- Inorder, preorder, and postorder are the three classic depth-first walks. Level order is breadth-first.
- Binary does not mean search tree and does not mean balanced. A chain is a binary tree.
</details>

<details>
<summary>Explain Height and Depths in the context of a Tree.</summary>

### Explain Height and Depths in the context of a Tree.

**Definition:**

Depth is how far a node is from the root. Height is how far a node is from its deepest descendant.

**How it works:**

- The root has depth 0. A child's depth is the parent's depth plus one.
- A leaf has height 0. An internal node's height is one plus the larger height of its children.
- The height of the tree is the height of the root, which equals the maximum depth of any node.
- Some authors count nodes instead of edges, so they are off by one. Say which convention you are using.

**Examples:**

Root A, children B and C, and B's child D. Depth of D is 2. Height of B is 1. Height of A is 2.
</details>

<details>
<summary>What is the difference between a Tree and a Graph?</summary>

### What is the difference between a Tree and a Graph?

**Definition:**

A tree is a special graph: connected and acyclic, so it has exactly n−1 edges for n nodes and a unique path between any pair.

**How it works:**

- A graph may have cycles, several components, and more than one path between two nodes.
- Edges in a graph need not be parent-child. A tree picks a root and orients the unique paths away from it.
- Graph search needs a visited set because of cycles. Tree recursion does not, if you only follow child links.
- Every tree is a graph. Most graphs are not trees.
</details>

<details>
<summary>Define Leaf and Internal nodes in a Tree.</summary>

### Define Leaf and Internal nodes in a Tree.

**Definition:**

A leaf has no children. An internal node has at least one child.

**How it works:**

- The root is internal when the tree has more than one node, and it is a leaf when the tree is a single node.
- In a binary tree of n nodes there are at most (n+1)/2 leaves in some shapes and exactly one leaf in a chain.
- Many algorithms do the real work at leaves (a trie word ends, a segment-tree index) and combine at internal nodes.
- A null child pointer is not a node. Do not count it as a leaf.
</details>

<details>
<summary>What is a Rooted Tree, and how does it differ from an Unrooted Tree?</summary>

### What is a Rooted Tree, and how does it differ from an Unrooted Tree?

**Definition:**

A rooted tree designates one node as the root, which creates parent, child, depth, and subtree. An unrooted tree is only the shape of the connections.

**How it works:**

- Picking a different root on the same edges changes depths and which node is an ancestor.
- Phylogenetic trees are often drawn unrooted until a root is chosen. File paths assume a root.
- The edge count is still n−1 either way.
- Algorithms that say 'parent pointer' have already chosen a root.
</details>

<details>
<summary>What is a N-ary Tree, and how does it generalize a binary tree?</summary>

### What is a N-ary Tree, and how does it generalize a binary tree?

**Definition:**

An n-ary tree allows up to n children per node. A binary tree is the case n = 2.

**How it works:**

- Children are usually an ordered list, not two named fields. A node with fewer than n children simply has a shorter list.
- A complete n-ary tree packs into an array: the children of `i` start at `n*i+1`.
- DOM elements and file directories are n-ary with no small fixed n.
- Height and traversal are the same ideas. The branching factor replaces the constant 2 in the node-count formulas.
</details>

<details>
<summary>Discuss the properties of a Full Binary Tree.</summary>

### Discuss the properties of a Full Binary Tree.

**Definition:**

A full binary tree, also called proper or plane-strict, gives every node either 0 children or 2. No node has exactly one child.

**How it works:**

- If there are L leaves, there are L−1 internal nodes, and the total number of nodes is 2L−1, which is odd.
- This is not the same as complete. A complete tree can have a node with one child on the last level. A full tree can be unbalanced.
- Expression trees for binary operators are full: each operator has two operands.
- The relation fails as soon as one unary node appears.

**Examples:**

Nodes: `*` with children `+` and `3`, and `+` with children `1` and `2`. Every node has 0 or 2 children. Leaves: 1, 2, 3. Internal nodes: two.
</details>

<details>
<summary>What is the significance of the degree of a node in a tree?</summary>

### What is the significance of the degree of a node in a tree?

**Definition:**

The degree of a node is how many children it has. The degree of the tree is the largest degree of any node.

**How it works:**

- A leaf has degree 0. A binary-tree node has degree at most 2.
- In some graph texts degree counts every incident edge, including the parent. In tree texts for interviews, degree usually means the number of children. Say which one you mean.
- The sum of degrees (as children) equals n−1, one parent link per non-root.
- A bound on degree is the branching factor that shows up in time bounds for a trie or a B-tree.
</details>

<details>
<summary>Explain the concept of a Path in a tree.</summary>

### Explain the concept of a Path in a tree.

**Definition:**

A path is a sequence of distinct nodes where each step follows an edge. In a tree that path is unique.

**How it works:**

- The length of a path is the number of edges, one less than the number of nodes on it.
- The path from the root to a node is the ancestor chain. Its length is the depth.
- The lowest common ancestor of two nodes is the deepest node that sits on both root-paths.
- Because the path is unique, distance between two nodes is depth(u) + depth(v) − 2·depth(LCA).
</details>

### Types of Binary Trees

<details>
<summary>What is a Binary Search Tree (BST)?</summary>

### What is a Binary Search Tree (BST)?

**Definition:**

A BST is a binary tree with the search order: left subtree keys are smaller than the node, right subtree keys are larger.

**How it works:**

- Search compares and goes left or right. It costs O(height).
- Inorder visits keys from small to large.
- Insert walks the same path and hangs the new key as a leaf.
- Without rebalancing, sorted inserts produce a chain of height n−1.
</details>

<details>
<summary>Explain the difference between a Binary Tree and a Binary Search Tree (BST).</summary>

### Explain the difference between a Binary Tree and a Binary Search Tree (BST).

**Definition:**

Every BST is a binary tree. A binary tree becomes a BST only when the left-smaller and right-larger order holds at every node.

**How it works:**

- A binary tree may store keys in any arrangement. Traversal order is not sorted order.
- The BST order is what makes search O(height) instead of a full scan.
- A heap is a binary tree with a different order (parent versus children) and is not a BST.
- Checking the BST property needs the whole subtree, not only the two children. A node can be larger than its left child and still smaller than some node deeper on the left... actually the violation is a left descendant greater than the node. The check must carry an allowed range.

**Examples:**

Keys 2, 1, 3 with 2 at the root is a BST. Keys 2, 3, 1 with 3 on the left is a binary tree and not a BST.
</details>

<details>
<summary>What is a Complete Binary Tree?</summary>

### What is a Complete Binary Tree?

**Definition:**

A complete binary tree fills every level except possibly the last, and the last level is packed from the left with no gaps.

**How it works:**

- That shape is exactly what lets a heap live in an array without holes.
- All levels above the last are full. The last level may stop early, but only after a prefix.
- A complete tree is not necessarily full (a node on the last level can have one child... actually the leftmost nodes on the last level's parents may have one child only at the boundary). A node with a right child and no left child is not complete.
- Height is floor(log₂ n).
</details>

<details>
<summary>Define a Perfect Binary Tree and its characteristics.</summary>

### Define a Perfect Binary Tree and its characteristics.

**Definition:**

A perfect binary tree is full and complete: every internal node has two children, and every leaf is at the same depth.

**How it works:**

- The node count is 2^(h+1) − 1 if the height is h in edges. Every level d has exactly 2ᵈ nodes.
- It is the shape of a full heap level before the next insert would start a new level.
- A perfect tree is complete and full. The converse needs both properties; either one alone is weaker.
- The number of leaves is one more than the number of internal nodes, and also exactly half of n rounded up.

**Examples:**

Height 2: one root, two children, four leaves. Total 7 nodes.
</details>

<details>
<summary>Explain what a Degenerate (or Pathological) Tree is and its impact on operations.</summary>

### Explain what a Degenerate (or Pathological) Tree is and its impact on operations.

**Definition:**

A degenerate tree is a binary tree that has become a chain: every node has only one child, so the height is n−1.

**How it works:**

- Sorted insertion into a plain BST produces this shape. Each new key hangs on the right of the previous one.
- Search, insert, and delete become O(n), the same as a linked list, and you still pay for two child pointers.
- Balanced trees exist to forbid this shape. A random insertion order only makes it unlikely, not impossible.
- Recursion on a degenerate tree is also a stack-overflow risk, because the call depth is n.

**Examples:**

Insert 1, 2, 3, 4 into an empty BST. The tree is a right spine. Searching for 4 compares all four keys.
</details>

<details>
<summary>Discuss the concept of a Balanced Binary Tree and why it’s important.</summary>

### Discuss the concept of a Balanced Binary Tree and why it’s important.

**Definition:**

A height-balanced binary tree keeps the heights of the two subtrees of every node within one of each other. That bound is what keeps the height logarithmic.

**How it works:**

- Wikipedia defines this balance as the left and right subtrees differing in height by at most 1. The minimum height of a binary tree on n nodes is log2(n + 1) − 1, achieved by a perfect tree.
- A binary search tree's search, insert, and delete are Θ(log n) on average and O(n) in the worst case. The worst case is the chain. Balance is what stops that chain.
- AVL trees were the first self-balancing BSTs. Their search, insert, and delete are O(log n).
- Without the height bound, a sorted insertion sequence produces the chain the BST page compares to a linked list.

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>What is a Balanced Tree?</summary>

### What is a Balanced Tree?

**Definition:**

A balanced tree is one whose height stays within a constant factor of the minimum, so a root-to-leaf path cannot become a list of n nodes.

**How it works:**

- Wikipedia's height balance for a binary tree: the subtrees of every node differ by at most 1. Weight balance instead compares the number of leaves, or the sizes, of the two sides.
- A BST page calls a tree height-balanced when the heights of the left and right subtrees are related by a constant factor. Self-balancing updates keep the height at the binary logarithm.
- Scapegoat trees relax this: they are not weight-balanced at every moment, but they stay loosely height-balanced and rebuild a subtree when an insertion breaks that bound.
- The point of the definition is the path length. A bound on height is a bound on the comparisons a search performs.

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — Scapegoat tree](https://en.wikipedia.org/wiki/Scapegoat_tree)
</details>

### Binary Tree Traversals

<details>
<summary>Classify Tree Traversal Algorithms.</summary>

### Classify Tree Traversal Algorithms.

**Definition:**

Tree walks are depth-first or breadth-first. The three depth-first orders for a binary tree are pre-order, in-order, and post-order.

**How it works:**

- Wikipedia: depth-first search goes as deep as possible before the next sibling, and it is implemented with a stack, including the call stack. Breadth-first search is implemented with a queue.
- Pre-order visits the node, then the left subtree, then the right (NLR). In-order visits left, node, right (LNR). Post-order visits left, right, node (LRN).
- Pre-order or post-order together with in-order describes a binary tree of distinct keys. Pre-order together with post-order does not.
- In a BST, in-order yields the keys in ascending order. Reverse in-order yields descending order.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
</details>

<details>
<summary>How to perform an In-Order Traversal in a Binary Search Tree (BST)?</summary>

### How to perform an In-Order Traversal in a Binary Search Tree (BST)?

**Definition:**

In-order on a binary search tree visits the left subtree, then the node, then the right subtree, and the keys come out sorted ascending.

**How it works:**

- Wikipedia states that result for a BST whose key is greater than every key in the left subtree and less than every key in the right.
- The walk is depth-first. Recursion uses the call stack. An explicit stack does the same deferral of the right child.
- An empty child returns immediately. The first value emitted is the leftmost node. The last is the rightmost.
- Reverse in-order, right then node then left, emits descending order.

**Examples:**

```js
function inorder(n, out) {
  if (!n) return;
  inorder(n.left, out);
  out.push(n.key);
  inorder(n.right, out);
}
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Implement a Pre-Order Traversal in a binary tree.</summary>

### Implement a Pre-Order Traversal in a binary tree.

**Definition:**

Pre-order visits the node before either subtree. The parent is processed before its children, so the sequence is a topological order of the tree edges.

**How it works:**

- The order is NLR: visit, then left, then right. Wikipedia calls this a topologically sorted traversal because the parent is processed before any child is done.
- The first value is the root. A subtree occupies a contiguous segment after its root.
- Together with in-order, pre-order rebuilds a binary tree of distinct keys. Alone, it does not determine the shape.
- The recursive form and the stack form visit the same nodes. The stack form pushes the right child before the left so the left is processed next.

**Examples:**

```js
function preorder(n, out) {
  if (!n) return;
  out.push(n.key);
  preorder(n.left, out);
  preorder(n.right, out);
}
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Implement a Post-Order Traversal in a binary tree.</summary>

### Implement a Post-Order Traversal in a binary tree.

**Definition:**

Post-order visits both subtrees before the node. Children are finished before the parent.

**How it works:**

- The order is LRN. Wikipedia notes it produces the postfix form of a binary expression tree.
- The root is last. Deleting or evaluating a node is safe at the visit, because both children have already been processed.
- Together with in-order it describes a binary tree of distinct keys. Paired only with pre-order, some shapes stay ambiguous.
- An explicit stack can emit a node only after both children have been seen, which is why the iterative form records a previous pointer or pushes a marker.

**Examples:**

```js
function postorder(n, out) {
  if (!n) return;
  postorder(n.left, out);
  postorder(n.right, out);
  out.push(n.key);
}
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Discuss the applications of each type of Tree Traversal algorithm.</summary>

### Discuss the applications of each type of Tree Traversal algorithm.

**Definition:**

The order you visit nodes is the order the algorithm can use them. Each classical walk matches a different dependency.

**How it works:**

- Pre-order emits a parent before its children. Wikipedia calls that a topological order. It is the walk that copies a tree or writes a prefix serialization.
- In-order on a BST emits keys ascending. That is the walk for a sorted listing.
- Post-order emits children first. Wikipedia points at postfix evaluation of an expression tree. It is also the walk that frees a node after its subtrees.
- Breadth-first order, the queue walk, visits a whole level before the next. DFS does not. BFS finds shortest paths in an unweighted tree; DFS does not generally find them.

**When to use:**

Pre-order to copy or prefix a tree. In-order to list a BST. Post-order to evaluate or destroy. Level order when distance from the root matters.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
</details>

<details>
<summary>Compare the iterative and recursive implementations of Tree Traversals.</summary>

### Compare the iterative and recursive implementations of Tree Traversals.

**Definition:**

Recursive and stack-based walks visit the same nodes. The call stack is the stack the recursive form does not name.

**How it works:**

- Wikipedia: deferred nodes sit on a stack for depth-first search, including the call stack, and on a queue for breadth-first search.
- The recursive in-order, pre-order, and post-order procedures are the three NLR/LNR/LRN orders. An explicit stack pushes the not-yet-visited child and loops.
- A threaded-tree page notes the problem with the recursive algorithm: stack space proportional to the height. A balanced tree makes that logarithmic. A chain makes it n frames.
- The iterative form can stop without unwinding a deep call chain, and it can be written as a loop the compiler does not have to keep as frames. The visit order does not change.

**When to use:**

Use recursion when the height is known to be small. Use an explicit stack when the tree may be a chain and a deep call stack is the failure mode.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
</details>

<details>
<summary>How can you traverse a binary tree Without Using Recursion or stack?</summary>

### How can you traverse a binary tree Without Using Recursion or stack?

**Definition:**

A walk with neither recursion nor an explicit stack uses the tree's own null child links as the temporary stack. That is Morris traversal, built on the threaded-tree idea.

**How it works:**

- The threaded-tree page states the recursive problem directly: stack space proportional to the height, logarithmic on a fairly balanced tree and linear when the tree is a chain.
- Morris in-order, facing a node with a left child, walks to that child's rightmost descendant and threads it back to the current node. The next step follows the thread, then removes it, so the tree is restored.
- A node with no left child is visited immediately. Each thread is created and destroyed once, so the extra memory is the pointers already in the nodes, not a stack of n frames.
- The visit order is still in-order. The tree must be writable for the duration of the walk, because the threads occupy child slots.

**Examples:**

```text
node with left child: thread its rightmost to the node
follow left, later follow the thread, then clear it
```

**Sources:**

- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

### BFS and DFS in Trees

<details>
<summary>Explain the Breadth-First Search (BFS) traversing method.</summary>

### Explain the Breadth-First Search (BFS) traversing method.

**Definition:**

Breadth-first search expands the earliest discovered vertex next. On a tree the frontier is a queue, and the visit order is level order.

**How it works:**

- Wikipedia: BFS explores every vertex and every edge in the worst case, in time O(|V| + |E|), and it finds shortest paths. Space is O(|V|).
- The queue holds discovered nodes whose children are not yet enqueued. The root goes in first. Dequeue a node, then enqueue its children left to right.
- On a tree there is one path from the root, so the first time a node is dequeued is the shortest path. A graph needs a seen-set so an edge back to a discovered node is not enqueued again.
- The queue, not a stack, is the whole difference from DFS. DFS does not generally find shortest paths.

**Examples:**

```text
root A, children B and C, B's child D
queue: A, then B C, then C D, then D
order A, B, C, D
```

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
</details>

<details>
<summary>Explain the Depth-First Search (DFS) algorithm.</summary>

### Explain the Depth-First Search (DFS) algorithm.

**Definition:**

Depth-first search follows one child as far as it can before the sibling. On a binary tree that is pre-order, in-order, or post-order depending on when you visit.

**How it works:**

- Wikipedia implements DFS with a stack, including the call stack. Time on an explicit graph traversed without repetition is O(|V| + |E|). It does not generally find shortest paths.
- Space is O(|V|) if the whole graph is marked, and O(longest path) for an implicit graph searched to a limited depth. On a tree the longest path is the height.
- The binary-tree steps are: if the node is empty, return; otherwise do visit, left, and right in the chosen order.
- A chain makes the recursion depth n. The threaded-tree page flags that stack use as proportional to the height.

**Sources:**

- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
</details>

<details>
<summary>What are the key differences between BFS and DFS?</summary>

### What are the key differences between BFS and DFS?

**Definition:**

BFS uses a queue and finds shortest paths. DFS uses a stack and does not generally find them. Both are O(|V| + |E|) on an explicit graph traversed without repetition.

**How it works:**

- BFS space is O(|V|). DFS space is O(|V|) when the whole graph is marked, or proportional to the longest path on an implicit search.
- BFS visits in order of discovery time, which on an unweighted tree is level order. DFS finishes a branch before the sibling.
- Wikipedia says the choice depends less on the shared complexity and more on the vertex order each algorithm produces.
- A tree has |E| = |V| − 1, so both bounds are linear in the number of nodes. The difference that remains is the order and the shape of the frontier.

**When to use:**

BFS for distance from the root. DFS for a finishing order, a path that should complete, or a recursion that matches the tree.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
</details>

<details>
<summary>Why does Breadth-First Search use more memory than Depth-First Search?</summary>

### Why does Breadth-First Search use more memory than Depth-First Search?

**Definition:**

BFS keeps every node of the current level, and the next level can be wider. DFS keeps only the path from the root to the cursor.

**How it works:**

- Wikipedia's BFS space is O(|V|). On a perfect binary tree the widest level has about half the nodes, so the queue holds that many.
- DFS on an implicit tree uses space proportional to the longest path, which is the height. A balanced binary tree has minimum height log2(n + 1) − 1, so the DFS stack follows that height rather than the width.
- The threaded-tree page makes the same contrast for recursion: stack proportional to height, not to n, when the tree is fairly balanced.
- A chain reverses the comparison. DFS then holds n frames, and BFS holds one node at a time.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
</details>

<details>
<summary>Illustrate the difference in Peak Memory Consumption between DFS and BFS.</summary>

### Illustrate the difference in Peak Memory Consumption between DFS and BFS.

**Definition:**

Peak memory is the largest frontier. For BFS that is the widest level. For DFS it is the deepest path.

**How it works:**

- A perfect binary tree of height h has the most nodes on the last level. BFS's queue reaches that level. Wikipedia's BFS space bound is O(|V|), and that last level is a constant fraction of |V|.
- DFS peak is the height. The minimum height is log2(n + 1) − 1. The chain's height is n − 1 edges if the root is an end, and the DFS stack tracks it.
- The recursive DFS pays that peak as call frames. Morris traversal is the in-order walk that does not, by threading null links instead.
- Reporting the peak means saying which shape. A bushy tree peaks on the BFS queue. A chain peaks on the DFS stack.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
</details>

<details>
<summary>Provide some practical examples of using Depth-First Search vs Breadth-First Search.</summary>

### Provide some practical examples of using Depth-First Search vs Breadth-First Search.

**Definition:**

Use BFS when the answer is a shortest path or a whole level. Use DFS when the answer is a finished subtree or a single path.

**How it works:**

- Shortest paths in an unweighted tree are BFS. Wikipedia calls BFS optimal for that and DFS not generally optimal.
- Evaluating an expression tree is post-order DFS, because the operator needs both children first. Copying a tree is pre-order, parent before children.
- Listing a BST in order is in-order DFS. Level-order printing of a directory one generation at a time is the queue.
- A deep narrow tree makes the DFS stack the risk the threaded-tree page describes. A very wide level makes the BFS queue the risk.

**When to use:**

BFS for distance and levels. DFS for postfix, prefix, and sorted BST order.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

### Binary Search Tree Operations

<details>
<summary>Implement an Insert Function for a Binary Search Tree (BST).</summary>

### Implement an Insert Function for a Binary Search Tree (BST).

**Definition:**

BST insert walks from the root by key comparison and hangs the new key on the first missing child.

**How it works:**

- If the new key is less, go left. If it is greater, go right. Replace the null child with the new node. Equal keys are a policy: reject them, or send them consistently left or right. The BST order Wikipedia uses is strict: greater than the left subtree, less than the right.
- The walk length is the depth of the new leaf. Average search, insert, and delete are Θ(log n). Worst case is O(n), the chain.
- The new node starts with two empty children. Nothing above it is rearranged by a plain BST insert. That is why sorted input degenerates.
- A self-balancing insert adds rotations or a rebuild after this same search, to keep the height logarithmic.

**Examples:**

```js
function insert(n, key) {
  if (!n) return { key, left: null, right: null };
  if (key < n.key) n.left = insert(n.left, key);
  else if (key > n.key) n.right = insert(n.right, key);
  return n;
}
```

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>Implement a Delete Function for a Binary Search Tree (BST).</summary>

### Implement a Delete Function for a Binary Search Tree (BST).

**Definition:**

BST delete finds the key, then repairs the hole so the search order still holds.

**How it works:**

- A leaf is removed. A node with one child is replaced by that child. A node with two children is replaced by its in-order successor, the leftmost node of the right subtree, and that successor is deleted from its old place. Wikipedia describes the successor taking the node's position.
- The successor has no left child, so removing it is the one-child case.
- Average delete is Θ(log n) and worst case is O(n), the same table as search and insert. The walk to the node and then to the successor follows the height.
- A plain delete does not rebalance. Repeated deletes can still leave a chain.

**Examples:**

```text
delete 2 from 2 with left 1 and right 3
successor is 3, which replaces 2
```

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>How do you Search for a value in a Binary Search Tree (BST)?</summary>

### How do you Search for a value in a Binary Search Tree (BST)?

**Definition:**

BST search compares the target with the node and discards one subtree.

**How it works:**

- Less goes left, greater goes right, equal is found. An empty node means the key is absent.
- Average time is Θ(log n). Worst time is O(n), when the tree is a chain. Space of the tree is Θ(n).
- The running time grows with the height if insertions were arbitrary. Self-balancing variants bound the worst lookup by the binary logarithm.
- In-order is not the search. Search is one path. In-order visits every node.

**Examples:**

```js
function search(n, key) {
  while (n) {
    if (key === n.key) return n;
    n = key < n.key ? n.left : n.right;
  }
  return null;
}
```

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>What are the time complexities for Insert, Search, and Delete operations in a BST, and why?</summary>

### What are the time complexities for Insert, Search, and Delete operations in a BST, and why?

**Definition:**

Search, insert, and delete on a BST all follow one root-to-node path. Their cost is the height.

**How it works:**

- Wikipedia's table: average Θ(log n) and worst O(n) for each of search, insert, and delete. Space is Θ(n).
- The worst case is the linked-list shape. Sorted insertion builds it. The page says self-balancing variants were introduced to bound the worst lookup by the binary logarithm.
- Insert and search stop at the first null or the matching key. Delete of two children also walks to the in-order successor, still along a path, not across the whole tree.
- A balanced tree replaces the O(n) column. AVL search, insert, and delete are O(log n).

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>What are the advantages and disadvantages of BSTs?</summary>

### What are the advantages and disadvantages of BSTs?

**Definition:**

A BST gives ordered search on one path, and it falls apart when that path becomes n long.

**How it works:**

- Average search, insert, and delete are Θ(log n). In-order lists the keys sorted. The tree uses Θ(n) space.
- Worst case is O(n) for those three operations, the chain. Nothing in a plain insert prevents it.
- Compared with a hash table, the BST keeps order: successor, range, and sorted output are walks, not a full copy into another structure. A trie avoids hash collisions for strings; a BST compares whole keys.
- When the bound matters, an AVL or red-black tree keeps the logarithmic worst case. The plain BST is the smaller implementation when the insertion order is already random or the data set is tiny.

**When to use:**

Use a plain BST for a teaching implementation or for data whose insertion order you already trust. Use a balanced BST when a sorted insert must not become a list.

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
- [Wikipedia — Trie](https://en.wikipedia.org/wiki/Trie)
</details>

### Self-Balancing Trees

<details>
<summary>What is an AVL Tree? How do you balance it?</summary>

### What is an AVL Tree? How do you balance it?

**Definition:**

An AVL tree is a BST whose two subtrees at every node differ in height by at most 1. Search, insert, and delete are O(log n).

**How it works:**

- It was invented in 1962 by Adelson-Velsky and Landis, the first self-balancing BST. The BST page credits it with bounding the worst lookup by the binary logarithm.
- After an insert or delete, walk back toward the root. A node whose balance factor leaves the range −1, 0, 1 is rotated. A single rotation fixes a straight imbalance. A double rotation fixes a zigzag.
- The height stays at the binary-tree minimum order, log2(n + 1) − 1 in the perfect case, and the AVL invariant keeps it logarithmic rather than n.
- Rotations are pointer rewrites on a constant number of nodes. The search path is the only place they run.

**Sources:**

- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
</details>

<details>
<summary>What is a Red-Black Tree?</summary>

### What is a Red-Black Tree?

**Definition:**

A red-black tree is a BST with a color bit on each node and a set of color rules that keep the height logarithmic. Search and insert are O(log n).

**How it works:**

- Wikipedia's table gives search and insert as O(log n). The same logarithmic bound is why it is used where a BST must not degenerate.
- Every node is red or black. The root is black. A red node has black children. Every path from a node to a leaf has the same number of black nodes. Those rules cap the height.
- Insert and delete restore the rules with recoloring and rotations, local pointer changes on the path back to the root.
- Compared with AVL, the color rules allow a slightly taller tree. The BST worst case O(n) is still ruled out.

**Sources:**

- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>How does Inserting or Deleting nodes affect a Red-Black Tree?</summary>

### How does Inserting or Deleting nodes affect a Red-Black Tree?

**Definition:**

Insert and delete on a red-black tree do the BST structural change first, then repair colors so the black-height rules still hold.

**How it works:**

- A new node is inserted as in a BST and colored red. A red parent then violates the red-child rule. Rotations and recolors move that violation up until it disappears or the root is recolored black.
- Delete removes the BST way, successor included. If a black node leaves a path, that path has one fewer black node than the others. The repair pushes that deficit up with rotations and recolors.
- Search does not change colors. Wikipedia's search and insert bounds are O(log n); the repair walks the same path the search did.
- The tree remains a BST throughout. Colors are extra. The in-order sequence is still the sorted keys.

**Sources:**

- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>What is the time complexity for Insert into a Red-Black Tree?</summary>

### What is the time complexity for Insert into a Red-Black Tree?

**Definition:**

Insert into a red-black tree is O(log n) in the worst case, the same bound Wikipedia lists for search.

**How it works:**

- The BST search that finds the insertion point is one path. The color repair walks back along that path. The red-black rules keep the path logarithmic, so neither walk is the O(n) chain of a plain BST.
- A rotation or recolor at a node is a constant amount of pointer and color work. The number of such steps follows the height.
- The average BST insert is Θ(log n) and the worst is O(n). The red-black insert replaces that worst column with O(log n).
- The bound is per operation, not amortized across a sequence. Splay trees are the ones whose published bound is amortized.

**Sources:**

- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — Splay tree](https://en.wikipedia.org/wiki/Splay_tree)
</details>

<details>
<summary>Compare Red-Black Trees and AVL trees.</summary>

### Compare Red-Black Trees and AVL trees.

**Definition:**

Both are BSTs with a logarithmic worst case. AVL keeps a tighter height. Red-black does less work on the way back up.

**How it works:**

- AVL search, insert, and delete are O(log n), and the balance factor of every node stays in −1, 0, 1. That is the height difference of at most 1 from the binary-tree definition.
- Red-black search and insert are O(log n). The color rules allow a greater height than AVL's ±1, still logarithmic, still not the O(n) BST chain.
- AVL rotates whenever the balance factor breaks. Red-black can recolor and rotate less often along the path, which is why updates that do not look at the result immediately are often put in a red-black tree.
- In-order is sorted for both. Pick AVL when lookups dominate. Pick red-black when updates are frequent and a slightly taller tree is acceptable.

**When to use:**

AVL for lookup-heavy trees. Red-black for mixed updates, including the usual library ordered map.

**Sources:**

- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Explain how a Splay Tree works and where it is used.</summary>

### Explain how a Splay Tree works and where it is used.

**Definition:**

A splay tree is a BST that rotates the accessed node to the root. Frequently used nodes end up near the top.

**How it works:**

- Wikipedia: average search, insert, and delete are O(log n). The worst case of each is O(n). The height can be linear.
- After the access, splay rotations (zig, zig-zig, zig-zag) move that node to the root. The next lookup of the same node is at the root.
- A sequence of m operations has amortized cost O(m log n). One operation can be the linear worst case; the accounting pays it back on later operations.
- The dynamic-optimality conjecture, which the page marks unproven, says this is within a constant of the best self-adjusting BST for any access pattern.

**When to use:**

Use a splay tree when the same keys are touched again and again and a single slow operation is acceptable. Do not use it when every operation must be logarithmic in the worst case.

**Sources:**

- [Wikipedia — Splay tree](https://en.wikipedia.org/wiki/Splay_tree)
</details>

<details>
<summary>What are Scapegoat Trees, and how do they ensure balance?</summary>

### What are Scapegoat Trees, and how do they ensure balance?

**Definition:**

A scapegoat tree is a BST that stays loosely height-balanced and, when an insertion is too deep, rebuilds one unbalanced subtree into a complete tree.

**How it works:**

- Search is O(log n) worst case. Insert and delete are O(log n) on average and O(n) worst case, the cost of that rebuild.
- It does not store a balance byte the way AVL does. It computes the weight when it needs a scapegoat: an ancestor that is not α-weight-balanced, for a chosen α between 0.5 and 1.
- A high α rebuilds less often, so inserts are quicker and lookups slower. A low α does the opposite. The page says α can be chosen for the mix of operations.
- The rebuild is rare and expensive, unlike AVL's constant-size rotations on every breaking update.

**When to use:**

Use it when you want logarithmic search, no per-node balance field, and you can accept an occasional linear rebuild.

**Sources:**

- [Wikipedia — Scapegoat tree](https://en.wikipedia.org/wiki/Scapegoat_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>Discuss the concept of a Treap and its expected time complexities.</summary>

### Discuss the concept of a Treap and its expected time complexities.

**Definition:**

A treap is a BST on the keys and a heap on independently random priorities. Search, insert, and delete take logarithmic time.

**How it works:**

- Wikipedia ties that to random BSTs: a random insertion order has logarithmic height with high probability, and the heap priorities reproduce that shape.
- Insert searches as in a BST, then rotates the new node up until the heap order on priorities holds. Delete rotates it down until it is a leaf, then removes it.
- The expected shape is the random BST, not the sorted chain. A plain BST's worst case O(n) is still possible for an unlucky priority assignment, with low probability.
- The page also describes set union, intersection, and difference on this structure. Those are the extra operations a heap-ordered BST gives you beyond a single lookup.

**Sources:**

- [Wikipedia — Treap](https://en.wikipedia.org/wiki/Treap)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

### Specific Tree Data Structures

<details>
<summary>What is the difference between a Heap and a Red-Black Tree?</summary>

### What is the difference between a Heap and a Red-Black Tree?

**Definition:**

A binary heap finds the minimum in O(1) and does not find an arbitrary key quickly. A red-black tree finds any key in O(log n).

**How it works:**

- Heap find-min is O(1). Insert is O(log n) worst case and O(1) on average. Delete-min is O(log n) worst case. Finding an arbitrary element is O(n).
- A red-black tree's search and insert are O(log n). It is a BST, so in-order is sorted and a predecessor is a walk, not a scan.
- The heap is an array with the complete shape. The red-black tree is nodes with a color and two children. Parent and children in the heap are index arithmetic.
- Use the heap for 'next minimum'. Use the red-black tree for 'this key, or the next key above it'.

**When to use:**

Heap for a priority queue. Red-black tree for an ordered map or set.

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
</details>

<details>
<summary>What is a B-Tree?</summary>

### What is a B-Tree?

**Definition:**

A B-tree is a balanced search tree with many keys per node, built so the height stays small when a node is a disk page. Search, insert, and delete are O(log n).

**How it works:**

- Bayer and McCreight introduced it in 1970. Wikipedia's table lists search, insert, and delete as O(log n).
- A node holds several keys and several children. All leaves sit at the same depth. A full node splits, and the middle key moves up, which is what grows the height only at the root.
- The high branching is the point for secondary storage: one node read brings many keys, so the number of page reads follows the small height.
- A binary AVL node holds one key and two children. A B-tree node holds a pageful. Both are balanced. They are not the same shape.

**Sources:**

- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>How is an AVL Tree different from a B-Tree?</summary>

### How is an AVL Tree different from a B-Tree?

**Definition:**

An AVL tree is a binary BST with one key per node and a height difference of at most 1. A B-tree puts many keys in a node and keeps every leaf at the same depth.

**How it works:**

- AVL search, insert, and delete are O(log n) with binary branching. B-tree search, insert, and delete are also O(log n), with the log taken in a larger base because a node has many children.
- AVL rotations fix a binary node. A full B-tree node splits and promotes a key. Both restore balance on the way to the root.
- AVL is the in-memory shape: two pointers and a balance factor. A B-tree node is sized like a page so one read fetches many keys.
- In-order of an AVL tree is the sorted keys. A B-tree's keys are sorted inside the node and across the children the same way.

**Sources:**

- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
</details>

<details>
<summary>What are the differences between B-Trees and B+ Trees?</summary>

### What are the differences between B-Trees and B+ Trees?

**Definition:**

A B+ tree stores the records in the leaves and uses internal nodes only as a routing index. A B-tree can store records in internal nodes too.

**How it works:**

- Wikipedia: data sits in the leaves, and more branching of internal nodes reduces the height and therefore the search time. It works well on secondary storage.
- Leaves of a B+ tree are usually linked, so a range is a walk along that chain after one search to the start. A B-tree range has to traverse internal nodes as well.
- A split can propagate to the root and decrease or increase the height. The page notes that a merge can propagate to the root and decrease the height.
- Balance is unchanged by the structural tweak in the sense the page states: any change in structure does not affect performance because of the balanced-tree properties. Both stay short.

**When to use:**

B+ tree when scans of consecutive records matter and internal pages should hold only keys. B-tree when a record may live in an internal node and you do not need a leaf chain.

**Sources:**

- [Wikipedia — B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)
- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
</details>

<details>
<summary>What is a Segment Tree, and what are its applications?</summary>

### What is a Segment Tree, and what are its applications?

**Definition:**

The array segment tree answers range queries and point updates in O(log n). The geometry segment tree stores intervals and reports which of them cover a point.

**How it works:**

- cp-algorithms: the standard sum tree uses 4n vertices, builds in O(n) when merge is constant time, and answers a sum of a[l..r) and an assignment a[i] = x in O(log n). A prefix array is O(1) for the sum and O(n) for an update.
- Wikipedia's segment tree stores n intervals in O(n log n) space, builds in O(n log n), and inserting one interval costs O(log n). A query lists the stored segments that contain a point.
- The array form is a full binary tree over index ranges. The geometry form stores an interval at O(log n) nodes, at most twice per level.
- Same name, two jobs. Say which one before quoting a bound.

**When to use:**

Array segment tree for sums, minima, and point updates. Geometry segment tree for 'which intervals cover this point'.

**Sources:**

- [cp-algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
- [Wikipedia — Segment tree](https://en.wikipedia.org/wiki/Segment_tree)
</details>

<details>
<summary>Explain Trie (Prefix Trees) and their common use-cases.</summary>

### Explain Trie (Prefix Trees) and their common use-cases.

**Definition:**

A trie stores strings by characters on the edges. The node's place is the key. The root is the empty string.

**How it works:**

- Unlike a BST, a node does not hold the whole key. Children are individual characters. Every child shares a prefix with its parent.
- Wikipedia names autocomplete, spell checking, and IP routing. There is no hash collision, which is the advantage over a hash table.
- A basic trie is memory-intensive. Compression is the optimization the page mentions.
- Search follows one edge per character and stops at a missing child. It does not depend on comparing whole keys the way a BST does.

**Sources:**

- [Wikipedia — Trie](https://en.wikipedia.org/wiki/Trie)
</details>

<details>
<summary>What is an Interval Tree, and how is it different from a Segment Tree?</summary>

### What is an Interval Tree, and how is it different from a Segment Tree?

**Definition:**

An interval tree answers which stored intervals overlap a point or another interval. A geometry segment tree stores each interval in O(log n) canonical nodes and lists those that contain a query point.

**How it works:**

- Wikipedia's centered interval tree splits intervals into left of a center point, right of it, and crossing it. The crossing set is what the node searches.
- The BST-shaped interval tree annotates each node with the maximum upper endpoint in its subtree. Maintaining that annotation updates the ancestors, and the page says this takes time following the height.
- The segment tree uses O(n log n) space for n intervals and O(log n) to insert one. The page says it is less storage-efficient than the interval tree for one-dimensional range queries because of that higher storage, O(n log n).
- Worst case of a naive walk of the interval BST can scan every node. The annotation is what lets a search skip a subtree whose maximum end is left of the query.

**When to use:**

Interval tree for overlap queries with one node per interval. Segment tree when the canonical decomposition of intervals is the operation you want.

**Sources:**

- [Wikipedia — Interval tree](https://en.wikipedia.org/wiki/Interval_tree)
- [Wikipedia — Segment tree](https://en.wikipedia.org/wiki/Segment_tree)
</details>

<details>
<summary>Discuss the properties and uses of a Binary Indexed Tree (Fenwick Tree).</summary>

### Discuss the properties and uses of a Binary Indexed Tree (Fenwick Tree).

**Definition:**

A Fenwick tree, also called a binary indexed tree, is an array that reads one prefix sum and updates one element in logarithmic time.

**How it works:**

- The prefix-sum page states that pair of bounds. A range sum is the difference of two prefixes.
- A plain prefix array is O(1) to read and O(n) to update, which is the contrast cp-algorithms draws before introducing the segment tree. The Fenwick tree sits between them: logarithmic for both, in one array.
- It is not a tree of nodes with child pointers. The index walks by clearing or adding the lowest set bit.
- It does not replace a BST. It does not search keys. It accumulates an array.

**Sources:**

- [Wikipedia — Prefix sum](https://en.wikipedia.org/wiki/Prefix_sum)
- [cp-algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
</details>

<details>
<summary>What is a Suffix Tree, and why is it a powerful data structure in string processing?</summary>

### What is a Suffix Tree, and why is it a powerful data structure in string processing?

**Definition:**

A suffix tree is a compressed trie of every suffix of a string. Constructions are linear-time for a constant-size alphabet.

**How it works:**

- Wikipedia states that linear-time claim and a worst-case running time that grows when the alphabet is not constant. The worst-case space shows up on a string that forces many nodes.
- A pattern is searched by walking characters from the root. A missing edge means the pattern does not occur.
- The suffix array is the sorted list of the same starting positions without the tree. The tree is the one with the linear construction bound on a fixed alphabet.
- The page points at string algorithms and computational biology as the setting. Sequence alignment uses this kind of index so repeated substring questions do not rescan the text from scratch.

**Sources:**

- [Wikipedia — Suffix tree](https://en.wikipedia.org/wiki/Suffix_tree)
</details>

### Balanced Trees in Real-World Systems

<details>
<summary>Why are B-Trees used for Databases and File Systems?</summary>

### Why are B-Trees used for Databases and File Systems?

**Definition:**

B-trees are used for databases and file systems because a node can be a disk page and the tree stays short.

**How it works:**

- Search, insert, and delete are O(log n). The large fanout makes that logarithm a small number of page reads.
- A B+ tree puts the records in the leaves and routes through internal nodes. Wikipedia says the extra branching reduces height and search time and works well on secondary storage.
- A binary AVL tree would turn each comparison into its own potential page. The B-tree packs many keys into the page you already paid to read.
- Leaves at one depth mean every lookup does the same number of descents. A BST chain does not.

**Sources:**

- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)
</details>

<details>
<summary>When might standard BSTs be preferred over AVL trees?</summary>

### When might standard BSTs be preferred over AVL trees?

**Definition:**

A plain BST is enough when the height will not become linear, or when the implementation must stay a few pointer writes with no rotations.

**How it works:**

- Average search, insert, and delete are already Θ(log n). The O(n) column appears when insertion order builds a chain. Random order makes that unlikely. It does not make it impossible.
- AVL guarantees O(log n) and pays a balance factor plus rotations on the way back up. If lookups are few and the data set is small, that machinery is the larger cost.
- If you need a worst-case promise, the plain BST is the wrong choice. The page introduced AVL specifically to bound the worst lookup.
- Teaching code and one-shot trees built from a shuffled array are the cases that stay plain.

**When to use:**

Stay with a BST for a short lifetime or a known-random insert order. Move to AVL or red-black when an adversary can insert in sorted order.

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>Why are Hash Tables not used instead of B-Trees to index data within a database?</summary>

### Why are Hash Tables not used instead of B-Trees to index data within a database?

**Definition:**

A hash table does not keep keys in order and does not map onto disk pages the way a B-tree node does.

**How it works:**

- A B-tree search, insert, and delete are O(log n) with a large fanout, so a lookup is a few page reads. A hash bucket that collides walks a chain; a trie page notes that an imperfect hash has that worst-case chain, while a trie has no collision.
- Range scans and ordered successors are in-order walks of a search tree. A hash table has no in-order.
- A B+ tree leaf chain is the range. Secondary storage is the setting Wikipedia gives the B+ tree: data in the leaves, height reduced by branching.
- Use a hash table for equality lookup in memory. Use the B-tree when the next key, a range, or a page-sized node is the operation.

**When to use:**

Hash for point equality in RAM. B-tree or B+ tree for ordered disk indexes.

**Sources:**

- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)
- [Wikipedia — Trie](https://en.wikipedia.org/wiki/Trie)
</details>

<details>
<summary>Explain the importance of Red-Black Trees in the implementation of associative containers in C++ STL.</summary>

### Explain the importance of Red-Black Trees in the implementation of associative containers in C++ STL.

**Definition:**

Ordered associative containers need a worst-case logarithmic find, insert, and delete that also produces sorted order. A red-black tree is that BST.

**How it works:**

- Search and insert are O(log n). The color rules keep the height logarithmic, so the O(n) chain of a plain BST does not appear.
- In-order is the sorted key order. Iteration of a map is that walk. A hash table has neither the bound's worst-case path nor the order.
- Insert and delete repair colors with rotations on the path. The container can offer insert and erase without rebuilding the whole tree.
- AVL is the tighter alternative. The red-black tree is the one whose update repair is the usual library choice, because recoloring often finishes the violation without a tall rotation chain.

**Sources:**

- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

### Tree Algorithms

<details>
<summary>Write an algorithm to generate a Mirror of a Binary Tree.</summary>

### Write an algorithm to generate a Mirror of a Binary Tree.

**Definition:**

A mirror swaps every left child with the right child. The shape flips. A BST's in-order order reverses.

**How it works:**

- Visit in post-order or pre-order and swap the two children after or before the recursive calls. Every node is swapped once.
- DFS time on a tree is linear in the nodes, since |E| = |V| − 1 and both BFS and DFS are O(|V| + |E|) on an explicit graph. The mirror touches each node once.
- In-order of the mirror is reverse in-order of the original. Wikipedia says reverse in-order of a BST is descending.
- Doing it in place uses the call stack, proportional to the height. An explicit stack does the same.

**Examples:**

```js
function mirror(n) {
  if (!n) return n;
  const left = mirror(n.left);
  n.left = mirror(n.right);
  n.right = left;
  return n;
}
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
</details>

<details>
<summary>How to determine if two binary trees are Isomorphic?</summary>

### How to determine if two binary trees are Isomorphic?

**Definition:**

Two binary trees are isomorphic when one becomes the other by swapping left and right children at some nodes. The keys, if any, still have to match.

**How it works:**

- At each pair of nodes, the children match in order, or they match after a swap. Both choices are tried only when the unswapped pairing fails.
- Empty matches empty. One empty and one present fails. A key mismatch fails before the children are considered.
- The walk is DFS over the two trees together. It does not build the mirror as a third tree.
- Identical shape without swaps is the special case. Isomorphism allows the flips.

**Examples:**

```text
1 with left 2, and 1 with right 2, are isomorphic
1 with left 2, and 1 with left 3, are not
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
</details>

<details>
<summary>Implement an algorithm to check if a binary tree is a Valid BST.</summary>

### Implement an algorithm to check if a binary tree is a Valid BST.

**Definition:**

A binary tree is a valid BST when every node is greater than every key in its left subtree and less than every key in its right subtree, not only greater than its children.

**How it works:**

- Wikipedia's BST definition is that subtree form. Checking only the parent against its two children accepts a tree that breaks further down.
- Carry an open interval (low, high). The left call tightens high to the node. The right call tightens low to the node. A key outside the interval fails.
- In-order must be strictly increasing. That is the same fact as 'in-order emits ascending order'. One pass can track the previous key.
- Equal keys fail a strict BST. Decide the policy before you call the tree valid.

**Examples:**

```js
function valid(n, low, high) {
  if (!n) return true;
  if (n.key <= low || n.key >= high) return false;
  return valid(n.left, low, n.key) && valid(n.right, n.key, high);
}
```

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Solve the Lowest Common Ancestor (LCA) problem in a Binary Tree.</summary>

### Solve the Lowest Common Ancestor (LCA) problem in a Binary Tree.

**Definition:**

The lowest common ancestor of two nodes is the deepest node that has both as descendants, counting a node as a descendant of itself.

**How it works:**

- A plain walk from the root: if both targets are in the left subtree, go left; if both are in the right, go right; otherwise this node is the LCA. That is correct on a binary tree with a parent-free search.
- On a BST the same idea is key order. A node between the two keys, or equal to one of them, is the split.
- Wikipedia: a tree can be preprocessed in linear time so later LCA queries are constant time. On a dynamic forest the heavy-light decomposition supports logarithmic LCA.
- The one-shot walk does not need that preprocess. It is a DFS that returns as soon as the split is known.

**Sources:**

- [Wikipedia — Lowest common ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Write a code to find the Height of a Binary Tree.</summary>

### Write a code to find the Height of a Binary Tree.

**Definition:**

The height of a node is the number of edges on the longest path down to a leaf. A leaf has height 0. The tree's height is the root's.

**How it works:**

- Wikipedia's binary-tree figure uses that edge count: a tree of height 3. A root alone has height 0. The minimum height on n nodes is log2(n + 1) − 1.
- Post-order computes it: the height is 1 plus the larger child height, or 0 when both children are empty.
- Depth is the edges up to the root. Height is the edges down. They meet at the deepest leaf.
- The recursive form uses stack proportional to the height, the cost the threaded-tree page names. An explicit post-order stack does the same.

**Examples:**

```js
function height(n) {
  if (!n) return -1;
  return 1 + Math.max(height(n.left), height(n.right));
}
```

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
</details>

<details>
<summary>What is the Diameter of a Tree?</summary>

### What is the Diameter of a Tree?

**Definition:**

The diameter is the longest path between any two nodes, measured in edges. It does not have to pass through the root.

**How it works:**

- For each node the longest path through it is the height of the left subtree plus the height of the right subtree, plus two edges if both sides exist, using the same edge-height as the binary-tree page.
- The diameter is the maximum of those through-paths and of the diameters of the children. One post-order pass returns both the height and the best path.
- A chain has diameter n − 1. A perfect tree's diameter is about twice the height, and the minimum height is log2(n + 1) − 1.
- Counting nodes instead of edges shifts the answer by one. Say which unit you are using.

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
</details>

<details>
<summary>How would you find the Maximum Path Sum in a Binary Tree?</summary>

### How would you find the Maximum Path Sum in a Binary Tree?

**Definition:**

The maximum path sum is the largest sum of node values on a downward path that may turn once, through a node, from one subtree into the other.

**How it works:**

- At each node the best path that can be extended upward is the node plus the better child contribution, and a negative child contributes nothing.
- The best path that turns at this node is the node plus both child contributions when they help. The answer is the maximum of those turns over the tree.
- The walk is post-order: children first, then the node, the same order Wikipedia uses to finish children before the parent.
- An all-negative tree has its maximum at the single largest node, the same corner the maximum-subarray page describes for an array of nonpositive numbers.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
</details>

### Tree Problems

<details>
<summary>Discuss the problem of Serializing and Deserializing a Binary Tree.</summary>

### Discuss the problem of Serializing and Deserializing a Binary Tree.

**Definition:**

Serializing a binary tree writes it so a later read rebuilds the same shape, including the missing children.

**How it works:**

- Pre-order plus a marker for null does it. Wikipedia says pre-order alone does not determine the tree, and pre-order with in-order does, for distinct keys. The null markers are the extra information that lets pre-order stand alone.
- The write is DFS. The read consumes the next token, builds a node, then builds the left subtree and the right subtree. A null token returns an empty child.
- In-order plus pre-order rebuilds without null markers when keys are distinct, because the in-order split finds the left subtree's size.
- BFS level order with null markers is the other encoding. It uses the queue Wikipedia uses for breadth-first search.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
</details>

<details>
<summary>Solve the Vertical Order Traversal of a Binary Tree.</summary>

### Solve the Vertical Order Traversal of a Binary Tree.

**Definition:**

Vertical order groups nodes that share a column when the left child steps one column left and the right child steps one column right.

**How it works:**

- The root is column 0. A DFS or BFS records (column, node). Nodes in one column come out together.
- BFS visits a column's nodes from top to bottom, because level order dequeues parents before children. DFS would need a later sort by depth.
- Wikipedia's BFS is the queue walk, time O(|V| + |E|). On a tree that is linear in the number of nodes.
- Ties on the same column and the same depth keep the left-before-right order of the enqueue.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Implement Zigzag (Spiral) Level Order Traversal of a Binary Tree.</summary>

### Implement Zigzag (Spiral) Level Order Traversal of a Binary Tree.

**Definition:**

Zigzag level order is BFS that reverses the direction of every other level.

**How it works:**

- The queue still expands level by level. Wikipedia's BFS dequeues the earliest discovered node. Collect one level, then reverse that level's list before appending it, and leave the queue's child order unchanged.
- Even levels left to right, odd levels right to left, is the usual convention. The reverse is on the output of the level, not on the tree.
- Time stays the BFS bound O(|V| + |E|). The extra reverse is another pass over the level.
- A deque can append the next level at either end and avoid the separate reverse. The visit order is the same.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>How to count the number of Nodes with a given value in a binary tree?</summary>

### How to count the number of Nodes with a given value in a binary tree?

**Definition:**

Count the nodes whose key equals a given value by visiting each node once.

**How it works:**

- DFS or BFS both cover every node. On a tree the time is linear in the number of nodes, from the O(|V| + |E|) bound with |E| = |V| − 1.
- A BST can skip a side: if the target is less than the node, the right subtree is greater and need not be counted, and the opposite for a greater target. That uses the BST search order. Equals may sit on both sides if duplicates were allowed.
- A tree that is not a BST has no side to discard. The count is a full walk.
- The counter is one integer. The recursion depth follows the height.

**Sources:**

- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Solve the problem of finding the Kth Smallest Element in a BST.</summary>

### Solve the problem of finding the Kth Smallest Element in a BST.

**Definition:**

The kth smallest key in a BST is the kth node in in-order.

**How it works:**

- Wikipedia: in-order emits the keys ascending. Walk left, node, right, and decrement k on each visit. The node where k hits zero is the answer.
- The walk can stop there. It does not have to visit the rest.
- Worst case the target is the rightmost node, so the walk is the whole tree, O(n) visits. A balanced tree does not change that if k is n.
- An augmented size on each subtree finds the side in O(height) without visiting every smaller key. A plain BST does not store that size. AVL and red-black trees have logarithmic height if you add it.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

### Tree Challenges

<details>
<summary>Discuss the Subtree with Maximum Average problem and how to approach it.</summary>

### Discuss the Subtree with Maximum Average problem and how to approach it.

**Definition:**

The subtree with maximum average is the node whose descendant-average, including itself, is the largest.

**How it works:**

- Post-order returns two numbers from each child: the sum and the count. The node's sum is its value plus both child sums. The count is 1 plus both child counts. The average is the sum divided by the count.
- Track the node with the best average as you return. Children are finished before the parent, which is why the order is post-order.
- Every node is a candidate, including leaves. A leaf's average is its own value.
- The walk visits each node once, linear in the size of the tree.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Convert a Binary Search Tree to a Sorted Doubly Linked List.</summary>

### Convert a Binary Search Tree to a Sorted Doubly Linked List.

**Definition:**

A BST's in-order sequence is sorted. Thread those nodes into a doubly linked list in that order and the list is the sorted sequence.

**How it works:**

- Visit in-order. The previous node gets its right pointer set to the current node, and the current node's left pointer set to the previous. No extra node is allocated.
- The head is the first, leftmost, node. The tail is the last. Wikipedia's in-order is ascending, so the list reads sorted from the head.
- The tree shape is consumed: child pointers become list links. Callers must not use the tree afterward.
- The walk is the full in-order, one visit per node.

**Examples:**

```text
tree 2 with left 1 and right 3
list 1 <-> 2 <-> 3
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Construct a Binary Tree from Inorder and Preorder traversals?</summary>

### Construct a Binary Tree from Inorder and Preorder traversals?

**Definition:**

In-order and pre-order together determine a binary tree of distinct keys. Pre-order says which node is the root. In-order says which keys are in the left subtree.

**How it works:**

- Wikipedia: either pre-order or post-order paired with in-order is enough. Pre-order with post-order leaves ambiguity.
- The first pre-order key is the root. Find it in the in-order sequence. Keys to its left build the left subtree. Keys to its right build the right subtree. The next pre-order keys are consumed in that same split.
- Duplicate keys break the unique split. The page's claim is for distinct elements.
- The same idea with post-order takes the root from the end of the post-order sequence.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Write a function to Check if a Binary Tree is Full or not.</summary>

### Write a function to Check if a Binary Tree is Full or not.

**Definition:**

A full binary tree gives every node zero children or two. A node with exactly one child means it is not full.

**How it works:**

- Check each node. A leaf passes. A node with both children continues. A node with one child fails.
- This is not the height-balance definition. A full tree can still be a long zigzag of pairs, and a complete tree of height 1 with three nodes is full. Balance is a separate property: subtree heights differ by at most 1.
- The walk is DFS and visits every node, because a single-child node can sit anywhere.
- A perfect tree is full and every leaf is at the same depth. Full alone does not say that.

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
</details>

<details>
<summary>Implement a Connect Nodes at the Same Level in a Binary Tree.</summary>

### Implement a Connect Nodes at the Same Level in a Binary Tree.

**Definition:**

Connecting nodes at the same level sets each node's next pointer to the sibling on its right, including across different parents.

**How it works:**

- BFS is the level walk. Wikipedia's queue dequeues a node and then its children. Inside one level, the previously dequeued node points at the current one.
- The last node of a level points nowhere. The first node of the next level does not continue that link.
- If the nodes already have a next field, the connection is in place. Otherwise you are describing the links, not allocating a parallel list.
- A perfect tree can connect a level using only child pointers, left-to-right, without a queue. A sparse tree needs the queue to see the next node on the level.

**Sources:**

- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
</details>

### Tree Data Structure Interview Challenges

<details>
<summary>Solve the problem of finding the number of Islands in a binary matrix.</summary>

### Solve the problem of finding the number of Islands in a binary matrix.

**Definition:**

An island in a binary matrix is a connected group of land cells, usually 1s joined on the four sides. Counting islands is a graph search on the grid.

**How it works:**

- Scan every cell. When you see an unvisited land cell, increment the count and DFS or BFS through its neighbors, marking them visited so they are not a second island.
- Wikipedia's DFS and BFS are O(|V| + |E|) on the graph you explore. Here a vertex is a land cell and an edge is a side-adjacency.
- The matrix is row-major storage. The neighbor of (r, c) is a constant offset, not a pointer already stored in the cell.
- Diagonal neighbors are a different graph. Say four-connected unless the problem says eight.

**Sources:**

- [Wikipedia — Depth-first search](https://en.wikipedia.org/wiki/Depth-first_search)
- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
</details>

<details>
<summary>Flatten a Binary Tree to a Linked List in place.</summary>

### Flatten a Binary Tree to a Linked List in place.

**Definition:**

Flattening a binary tree to a linked list in place rewrites right pointers into a list, usually in pre-order, and clears left pointers.

**How it works:**

- Pre-order is parent then left then right. The list should be that sequence: each node's right child becomes the next node in pre-order, and the left child becomes empty.
- One way: flatten the left subtree, flatten the right, then insert the flattened left between the node and the flattened right.
- No new nodes. The original right child is attached at the end of the flattened left.
- In-order would be a different list, the sorted one only if the tree is a BST. The usual flatten is pre-order.

**Examples:**

```text
1 with left 2 and right 5, 2 with right 3
list 1 -> 2 -> 3 -> 5
```

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Implement a Boundary Traversal of a Binary Tree?</summary>

### Implement a Boundary Traversal of a Binary Tree?

**Definition:**

Boundary traversal lists the left edge, the leaves that are not already on that edge, and the right edge in reverse, without repeating a node.

**How it works:**

- The left edge is the path that always prefers the left child. The right edge prefers the right child. Leaves are the nodes with no children, walked left to right, skipping those already emitted as the left or right edge.
- The root is emitted once. A single-node tree is just the root.
- This is not one of the three DFS orders. It is three walks. Each node is still considered a constant number of times.
- In-order would mix internal nodes into the middle. The boundary keeps internals only on the two edges.

**Sources:**

- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
</details>

<details>
<summary>Implement a function to find if a given binary tree is Symmetric or not.</summary>

### Implement a function to find if a given binary tree is Symmetric or not.

**Definition:**

A binary tree is symmetric when it is a mirror of itself: the left subtree matches the right with children swapped.

**How it works:**

- Compare the left child of one side with the right child of the other, and the right with the left. Both must exist or both be empty, and keys must match.
- The root has no partner. Symmetry is a property of its two subtrees.
- A mirror of the whole tree equals the original when the tree is symmetric. You do not have to build the mirror; the paired walk is enough.
- One different key or one missing child on one side fails.

**Examples:**

```text
2 with children 1 and 1, each of those with outer child 3
is symmetric
2 with children 1 and 3 is not
```

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Write a code that describes Locking and Unlocking of nodes in an n-ary tree structure.</summary>

### Write a code that describes Locking and Unlocking of nodes in an n-ary tree structure.

**Definition:**

Locking a node in an n-ary tree succeeds only when no ancestor and no descendant is locked. Unlock clears that node. The structure has to answer both checks without scanning the whole tree every time.

**How it works:**

- Store a lock bit and a count of locked descendants. Lock fails if the bit is already set, if any ancestor is locked, or if the locked-descendant count is nonzero.
- A successful lock walks the ancestors and increments each locked-descendant count, then sets the bit. Unlock reverses the increments.
- The ancestor walk is the depth. On a balanced tree that depth is logarithmic in the sense of the height bound. On a chain it is n steps. The descendant check is the stored count, not a fresh DFS.
- An n-ary node has a list of children. The count sums all of them. A binary tree is the special case of two.

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

### Advanced Tree Topics

<details>
<summary>Explain the Morris In-Order Tree Traversal algorithm and its advantages.</summary>

### Explain the Morris In-Order Tree Traversal algorithm and its advantages.

**Definition:**

Morris in-order walks a binary tree with no recursion and no explicit stack by temporarily threading each left subtree's rightmost node back to its successor.

**How it works:**

- The threaded-tree page states why: recursive traversal uses stack space proportional to the height, logarithmic when the tree is fairly balanced and linear on a chain.
- If the node has no left child, visit it and go right. If it has a left child, find that child's rightmost node. If its right is empty, point it at the current node and go left. If it already points at the current node, clear it, visit the current node, and go right.
- Every thread is written and later removed, so the tree is unchanged afterward. The visit order is in-order, which on a BST is sorted.
- The tree must be mutable during the walk. A shared or persistent tree cannot lend its null pointers as threads.

**Sources:**

- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>Discuss the Hardy-Ramanujan Theorem as it pertains to tree data structures.</summary>

### Discuss the Hardy-Ramanujan Theorem as it pertains to tree data structures.

**Definition:**

The Hardy–Ramanujan asymptotic is a formula for the integer partition function p(n). It is number theory. It is not a balance rule for search trees.

**How it works:**

- Wikipedia: the asymptotic formula was first obtained by G. H. Hardy and Ramanujan in 1918, and independently by J. V. Uspensky in 1920. p(n) counts partitions of n.
- Nothing in that statement constrains the height of a BST, the colors of a red-black tree, or the fanout of a B-tree.
- Tree balance is a different theorem: AVL keeps subtree heights within 1, and the minimum binary-tree height is log2(n + 1) − 1.
- If an interview names Hardy–Ramanujan next to trees, separate the two. The partition formula does not choose rotations.

**Sources:**

- [Wikipedia — Partition function (number theory)](https://en.wikipedia.org/wiki/Partition_function_(number_theory))
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>How can Persistent Data Structures be implemented using Trees?</summary>

### How can Persistent Data Structures be implemented using Trees?

**Definition:**

A persistent tree keeps the old version after an update. Path copying shares the unchanged nodes and allocates only the path that changed.

**How it works:**

- Wikipedia: in a balanced BST without parent pointers, the worst-case modification cost of path copying is O(log n) plus the cost of the update. A linked list's worst-case modification is O(n) plus the update, because the path can be the whole list.
- The new root points at a mix of new nodes and old nodes. Readers of the old root still see the old tree.
- Fat nodes store each modification in the node instead of copying the path. The page gives O(1) extra space per modification for that method, with a different lookup cost.
- Parent pointers break simple path copying, because a child's new version would need the parent updated, which is the path you are already copying.

**Sources:**

- [Wikipedia — Persistent data structure](https://en.wikipedia.org/wiki/Persistent_data_structure)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Explain Heavy-Light Decomposition and its applications.</summary>

### Explain Heavy-Light Decomposition and its applications.

**Definition:**

Heavy-light decomposition splits a tree into paths so that a walk from any node to the root crosses only a logarithmic number of those paths.

**How it works:**

- Wikipedia: light edges leave a path tree of height at most logarithmic. Each data-structure operation then does only a small number of steps, one per heavy path.
- A heavy child is the child whose subtree is largest. The heavy edges form disjoint paths. A light edge drops the subtree size by at least half, which is why the light edges on a root path are logarithmic.
- Put a segment tree or a Fenwick structure on each heavy path and a path query becomes a logarithmic number of those structures. The segment-tree query itself is O(log n).
- LCA on a dynamic forest can use this decomposition. Wikipedia says that supports logarithmic LCA in the size of the tree.

**Sources:**

- [Wikipedia — Heavy path decomposition](https://en.wikipedia.org/wiki/Heavy_path_decomposition)
- [cp-algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
- [Wikipedia — Lowest common ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor)
</details>

<details>
<summary>Can you describe a Universal Value Tree and its uniqueness in tree structures?</summary>

### Can you describe a Universal Value Tree and its uniqueness in tree structures?

**Definition:**

A universal-value tree, in the interview sense, is a binary tree in which every node holds the same value. It is also called univalued.

**How it works:**

- A leaf qualifies. An internal node qualifies only when its value equals each child's value and each child subtree is itself universal.
- The check is a DFS that returns the value and a bit. A mismatch of the two children, or of a child with the node, fails.
- This is not a balanced-tree invariant and not a BST invariant. A chain of equal keys is universal and is still the O(n) BST shape.
- Counting how many subtrees are universal is the same recursion, adding one when the bit is true.

**Sources:**

- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>What is meant by the invariant of a tree, and how is it maintained in self-balancing trees like AVL trees?</summary>

### What is meant by the invariant of a tree, and how is it maintained in self-balancing trees like AVL trees?

**Definition:**

An invariant is a property every operation must leave true. In an AVL tree it is the balance factor of every node, the height difference of the two subtrees, staying in −1, 0, 1.

**How it works:**

- Search does not change the tree, so it cannot break the invariant. Insert and delete can, and the rotations on the path back to the root are what restore it.
- Wikipedia's AVL bounds, O(log n) search, insert, and delete, are consequences of that invariant plus the binary shape. The minimum height log2(n + 1) − 1 is the perfect end of the same idea.
- A red-black tree's invariant is the color rules, equal black height and no red-red edge, instead of a numeric balance factor.
- If a rotation is skipped, the invariant is false and the next search may pay the O(n) BST worst case.

**Sources:**

- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Binary tree](https://en.wikipedia.org/wiki/Binary_tree)
</details>

### Trees and System Design

<details>
<summary>How do File Systems use trees to manage files and directories?</summary>

### How do File Systems use trees to manage files and directories?

**Definition:**

File systems use trees because a directory contains directories. The B-tree is the on-disk form that keeps that hierarchy, or the file index, in a few page reads.

**How it works:**

- A directory tree is the composite shape: a folder has children, a file does not. Path lookup is a walk from the root.
- The index of a large directory or of file blocks is where the B-tree or B+ tree shows up. Search, insert, and delete are O(log n), and a node is a page. The B+ tree keeps records in the leaves so a sequential read of a file's index is a leaf walk.
- An unbalanced BST of directory entries would turn a sorted insert into a chain, O(n) worst case. The B-tree's equal leaf depth avoids that.
- The namespace tree and the block index are different trees. One is the path users see. The other is the page structure on disk.

**Sources:**

- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)
- [Wikipedia — Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern)
</details>

<details>
<summary>Discuss how trees enable Efficient Undo/Redo Operations in text editors or IDEs.</summary>

### Discuss how trees enable Efficient Undo/Redo Operations in text editors or IDEs.

**Definition:**

Undo keeps the previous version of the document instead of mutating the only copy. A persistent tree does that by path copying.

**How it works:**

- Wikipedia: path copying on a balanced BST without parent pointers costs O(log n) plus the update in the worst case. The unchanged subtrees are shared with the old root.
- Undo is 'use the previous root'. Redo is 'use the newer root'. Neither walks the whole document.
- A full copy of the tree on every keystroke would copy every node. The page calls that inefficient. Path copying copies only the ancestors of the edit.
- A chain, like a linked list of characters with no balancing, makes the modified path O(n). The balanced tree is what keeps undo logarithmic.

**Sources:**

- [Wikipedia — Persistent data structure](https://en.wikipedia.org/wiki/Persistent_data_structure)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Explain the use of Tree Structures in managing hierarchical data in relational databases.</summary>

### Explain the use of Tree Structures in managing hierarchical data in relational databases.

**Definition:**

Hierarchical rows in a relational table are an adjacency list or a nested set. The engine's own indexes on those keys are B-trees.

**How it works:**

- A parent-id column is the edge. Walking descendants is repeated lookup of that id. A nested-set or materialized path stores the tree so a subtree is a range.
- The index that makes the lookup logarithmic on disk is the B-tree or B+ tree: O(log n) search, a page per node, records in the leaves for the B+ form.
- The SQL hierarchy and the index tree are not the same object. One is the foreign key the application sees. The other is the page tree the storage engine uses for any indexed column.
- A plain BST in memory has average Θ(log n) and worst O(n). The on-disk index uses the B-tree so the worst lookup stays a few pages.

**Sources:**

- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Describe a system design that efficiently indexes data using tree structures.</summary>

### Describe a system design that efficiently indexes data using tree structures.

**Definition:**

An index that must answer ordered lookup and range scans on disk is a B+ tree: short height, many keys per page, records in the leaves.

**How it works:**

- Search, insert, and delete on a B-tree are O(log n). The B+ tree stores data in the leaves and uses internal nodes to branch more, which Wikipedia says reduces height and search time on secondary storage.
- Equality and the next key are a descent plus a step along the leaf chain. A hash index answers equality and does not give the next key.
- The page size is the node size. A binary tree would spend a page on one key. The fanout spends it on many.
- In memory, a red-black tree is the same job with O(log n) search and insert and no page. On disk, the B+ tree is the one that matches the page.

**Sources:**

- [Wikipedia — B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)
- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
</details>

### Trees in Data Analysis

<details>
<summary>How are Decision Trees used in Machine Learning for classification and regression tasks?</summary>

### How are Decision Trees used in Machine Learning for classification and regression tasks?

**Definition:**

A decision tree for classification or regression splits the training rows into smaller groups until a leaf predicts a class or a number.

**How it works:**

- Wikipedia: the tree is built by splitting the source set, the root, into subsets that are the children. The split uses rules on the features. The same step repeats on each subset.
- A classification leaf predicts a class. A regression leaf predicts a numeric value, often the mean of the rows that landed there.
- This tree is not a BST and not balanced by the AVL rule. Its shape follows the splits that separate the labels, and a bad split can make it deep.
- Prediction walks one path from the root, one feature test per node, until a leaf.

**Sources:**

- [Wikipedia — Decision tree learning](https://en.wikipedia.org/wiki/Decision_tree_learning)
</details>

<details>
<summary>Discuss the role of Segment Trees in computational geometry and data analysis.</summary>

### Discuss the role of Segment Trees in computational geometry and data analysis.

**Definition:**

In computational geometry the segment tree stores intervals and answers which of them contain a query point. The array segment tree is the other structure that shares the name.

**How it works:**

- Wikipedia: n intervals use O(n log n) space and O(n log n) build time. Inserting one interval costs O(log n). A query reports the stored segments that contain the point.
- An interval is stored at most twice on each level. That is why the space is O(n log n), and why the page says this is less storage-efficient than an interval tree for one-dimensional queries.
- The array form from cp-algorithms is sums and point updates in O(log n) with 4n vertices. Geometry problems that need stabbing queries want the Wikipedia structure, not the sum tree.
- Both are binary trees over a line. They store different things in the nodes.

**Sources:**

- [Wikipedia — Segment tree](https://en.wikipedia.org/wiki/Segment_tree)
- [cp-algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
- [Wikipedia — Interval tree](https://en.wikipedia.org/wiki/Interval_tree)
</details>

<details>
<summary>Explain how Suffix Trees are utilized in sequence alignment algorithms in bioinformatics.</summary>

### Explain how Suffix Trees are utilized in sequence alignment algorithms in bioinformatics.

**Definition:**

Suffix trees index a string so a substring query walks from the root instead of scanning the text again. Bioinformatics uses that for sequences.

**How it works:**

- Wikipedia: the standard constructions are linear-time for a constant-size alphabet. DNA's alphabet is four letters, which is that setting.
- A pattern that occurs as a substring is a path from the root. Alignment and repeat finding ask that question many times against one sequence.
- The page cites string algorithms and computational biology together. The tree is the index. The alignment algorithm is the client.
- Space can be large. The page notes a worst-case space that shows up when the string forces many nodes. A suffix array is the more compact sorted list of the same suffixes when the tree's nodes are too expensive.

**Sources:**

- [Wikipedia — Suffix tree](https://en.wikipedia.org/wiki/Suffix_tree)
</details>

<details>
<summary>Describe the application of trees in Multi-dimensional Searching.</summary>

### Describe the application of trees in Multi-dimensional Searching.

**Definition:**

Multi-dimensional search stores a point by several keys. A single BST orders only one key. A range in two dimensions needs a structure that can discard a region, not only a half-line.

**How it works:**

- A BST search is Θ(log n) on average and O(n) worst on one ordered key. A second coordinate is invisible to that comparison.
- An interval tree and a geometry segment tree are the one-dimensional versions of 'who covers this position'. Wikipedia's segment tree answers a stabbing query over intervals. Higher-dimensional range search generalizes that decomposition.
- A B-tree still orders one key, with O(log n) search and a page-sized node. It does not by itself answer a rectangle query.
- The practical split is: one ordered key, use a BST or B-tree; an interval on a line, use an interval tree or a segment tree; several coordinates, say so and do not pretend a one-key BST discarded a rectangle.

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — Interval tree](https://en.wikipedia.org/wiki/Interval_tree)
- [Wikipedia — Segment tree](https://en.wikipedia.org/wiki/Segment_tree)
- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
</details>

### Performance and Optimization

<details>
<summary>Discuss Amortized Analysis in the context of splay trees and their operations.</summary>

### Discuss Amortized Analysis in the context of splay trees and their operations.

**Definition:**

Splay-tree analysis charges an operation its rotations plus the change in a potential. A single access can cost O(n). A sequence of m accesses costs O(m log n) amortized.

**How it works:**

- Wikipedia: average search, insert, and delete are O(log n), and each worst case is O(n). The height can be linear.
- The potential is high for a poorly balanced tree and low for a well-balanced one. A rotation that improves balance decreases the potential, and that decrease pays for the rotation in the accounting.
- The page sums this to T_amortized(m) = O(m log n). Going from amortized time to actual time adds the drop in potential from the start, which is O(n log n).
- Amortized does not mean each call is logarithmic. It means the expensive call is paid for by the cheap ones around it. AVL's O(log n) is a worst-case promise splay does not make.

**Sources:**

- [Wikipedia — Splay tree](https://en.wikipedia.org/wiki/Splay_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
</details>

<details>
<summary>Explain how to Optimize Space Complexity in tree algorithms.</summary>

### Explain how to Optimize Space Complexity in tree algorithms.

**Definition:**

Space follows the nodes you allocate and the stack the walk uses. Share structure, and do not keep a stack the height does not require.

**How it works:**

- A persistent update that copies the whole tree uses space proportional to n. Path copying on a balanced BST allocates O(log n) new nodes plus the update, and shares the rest.
- Recursive DFS uses stack proportional to the height. Morris in-order uses the null child pointers as threads and restores them, so the extra stack is gone.
- A trie spends an array of child slots per node. Wikipedia calls the basic form memory-intensive and points at compression.
- A segment tree of intervals uses O(n log n) space. An interval tree stores one node per interval and is the tighter choice when that is all the query needs.

**Sources:**

- [Wikipedia — Persistent data structure](https://en.wikipedia.org/wiki/Persistent_data_structure)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
- [Wikipedia — Trie](https://en.wikipedia.org/wiki/Trie)
- [Wikipedia — Segment tree](https://en.wikipedia.org/wiki/Segment_tree)
- [Wikipedia — Interval tree](https://en.wikipedia.org/wiki/Interval_tree)
</details>

<details>
<summary>How can you measure and improve the Cache Performance of tree operations?</summary>

### How can you measure and improve the Cache Performance of tree operations?

**Definition:**

A tree in an array of nodes touches neighboring memory when the walk does. Pointer-linked nodes touch whatever the allocator returned.

**How it works:**

- Sequential locality is linear access of a one-dimensional array. A heap already is that array: parent and children are index arithmetic, so a sift stays inside one block.
- A pointer BST's next node can be anywhere. The search still does Θ(log n) comparisons on average, and each comparison can miss the cache. Packing nodes in an array, with child indexes instead of pointers, makes the block contiguous even though the child index still jumps.
- A B-tree's whole point on disk is the same idea at page size: one read brings many keys, O(log n) searches with a large base.
- Cache behavior does not change the big-O of the comparisons. It changes the time of each comparison.

**Sources:**

- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

<details>
<summary>Provide strategies for Load Balancing in distributed tree structures.</summary>

### Provide strategies for Load Balancing in distributed tree structures.

**Definition:**

Load is balanced across a distributed tree when each machine owns a key range it can search without asking the others, and a split moves part of that range when one machine grows.

**How it works:**

- A B-tree already splits a full node and promotes a key so the height stays even. A distributed index uses the same idea on machines: a partition splits when it passes a size, and the separator key is the promoted key.
- Search is still O(log n) inside a partition that is itself a B-tree, plus one hop to the machine that owns the range. The fanout is what keeps the hop count small.
- A plain BST range that was filled in sorted order is the O(n) chain. Do not hand a machine a sorted run and call it balanced.
- Rebalancing moves keys. Readers need the separator updated before the old machine drops them, or the search follows a range that no longer holds the key.

**Sources:**

- [Wikipedia — B-tree](https://en.wikipedia.org/wiki/B-tree)
- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
</details>

### Design Patterns and Best Practices

<details>
<summary>Discuss the Composite Design Pattern and its relationship with tree data structures.</summary>

### Discuss the Composite Design Pattern and its relationship with tree data structures.

**Definition:**

The composite pattern builds a tree of parts and wholes so a client calls the same operation on a leaf and on a group.

**How it works:**

- Wikipedia: the intent is to compose objects into tree structures for part-whole hierarchies, and to let clients treat individual objects and compositions uniformly.
- A file and a directory, or a widget and a panel, are the usual pair. The group stores children and forwards the operation. The leaf does the operation.
- The walk is the tree walk. Pre-order does the group before the children. Post-order does the children before the group's own combination.
- The pattern does not balance the tree and does not make lookup logarithmic. It only shares the interface.

**Sources:**

- [Wikipedia — Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>

<details>
<summary>When should one consider using the Decorator Pattern with trees?</summary>

### When should one consider using the Decorator Pattern with trees?

**Definition:**

The decorator pattern adds behavior to one object without changing the other instances of its class. On a tree, that is a wrapper on one node or one subtree, not a subclass of every node.

**How it works:**

- Wikipedia: behavior is added dynamically to an individual object, and the other instances of the class are unchanged. It also helps the single-responsibility split, because the extra behavior lives in its own class.
- A decorator has the same interface as the node and holds the node it wraps. Calls go to the wrapper, which calls the node and adds the extra step.
- Use it when one subtree needs logging, a permission check, or a cached answer and the other subtrees must stay plain. A composite is the wrong tool for that: composite adds children, decorator adds behavior.
- Stacking decorators is a chain, not a balanced tree. Do not expect logarithmic lookup from the wrapper list.

**When to use:**

Decorate one node when the extra behavior is optional and per object. Subclass or change the node type when every node must have the behavior.

**Sources:**

- [Wikipedia — Decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern)
- [Wikipedia — Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern)
</details>

<details>
<summary>Explain the importance of Iterator Pattern in tree traversals.</summary>

### Explain the importance of Iterator Pattern in tree traversals.

**Definition:**

The iterator pattern walks a container without the client knowing how the links are stored. A tree has several legal orders, so the iterator has to name which one.

**How it works:**

- Wikipedia: an iterator traverses a container and the pattern decouples algorithms from containers. Some algorithms stay container-specific and cannot be decoupled.
- An in-order iterator hides the stack or the Morris threads. The client only asks for the next key. On a BST that next key is the next in sorted order.
- A level-order iterator hides the queue of BFS. A pre-order iterator hides the stack of DFS. They are different iterators, not one iterator with a flag soup if you can avoid it.
- The iterator must not expose a half-rotated splay or a half-built Morris thread if the client can stop early. Document whether abandoning the iterator restores the tree.

**Sources:**

- [Wikipedia — Iterator pattern](https://en.wikipedia.org/wiki/Iterator_pattern)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
- [Wikipedia — Breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search)
</details>

<details>
<summary>Discuss Best Practices in implementing tree data structures to avoid common pitfalls.</summary>

### Discuss Best Practices in implementing tree data structures to avoid common pitfalls.

**Definition:**

The failures that matter are a BST that becomes a chain, a walk that uses a stack as deep as that chain, and a balance invariant that an update forgets to restore.

**How it works:**

- Plain BST search, insert, and delete are O(n) in the worst case. If sorted input is possible, use AVL or red-black, whose search and insert are O(log n).
- Recursive walks use stack proportional to the height. Morris in-order avoids that stack when the tree is writable. Otherwise use an explicit stack and a loop.
- After insert and delete, AVL rotations or red-black recolors are part of the operation, not a later cleanup. A broken invariant silently returns the O(n) tree.
- Test the empty tree, a single node, a chain, and a full node with two children. Serialization must record null children, because pre-order alone does not determine the shape.

**Sources:**

- [Wikipedia — Binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree)
- [Wikipedia — AVL tree](https://en.wikipedia.org/wiki/AVL_tree)
- [Wikipedia — Red–black tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree)
- [Wikipedia — Threaded binary tree](https://en.wikipedia.org/wiki/Threaded_binary_tree)
- [Wikipedia — Tree traversal](https://en.wikipedia.org/wiki/Tree_traversal)
</details>
