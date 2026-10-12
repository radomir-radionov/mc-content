# Binary Tree — Interview

Answers are filled for every question in this file.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Binary Tree — Interview](./7_ds_binary_tree_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Binary Tree](https://devinterview.io/questions/data-structures-and-algorithms/binary-tree-data-structure-interview-questions/).

### Fundamental Tree Concepts

<details>
<summary>What is a Tree Data Structure?</summary>

### What is a Tree Data Structure?

**Definition:**

A tree is a connected acyclic graph with a hierarchy: one parent path from each node up to a root, and children below.

**How it works:**

- A node holds a value and links to its children. The root has no parent. A leaf has no children.
- There is exactly one path between any two nodes. A cycle would make it a general graph.
- Depth is the number of edges from the root. Height is the number of edges on the longest downward path.
- Typical uses are file systems, DOM documents, decision trees, and search trees.
</details>

<details>
<summary>What is a Binary Tree?</summary>

### What is a Binary Tree?

**Definition:**

A binary tree gives every node at most two children, usually called left and right.

**How it works:**

- A node may have only a left child, only a right child, both, or neither. Missing children are not the same as empty placeholder nodes unless you are drawing a heap.
- A full binary tree has 0 or 2 children at every node. A complete one fills levels left to right. A perfect one is full and every leaf is at the same depth.
- Traversal is inorder (left, node, right), preorder (node, left, right), or postorder (left, right, node). Level order uses a queue.
- A chain of n nodes is still a binary tree, and its height is n−1, so algorithms that look logarithmic on a balanced tree become linear.
</details>

### Specific Tree Types and Data Structures

<details>
<summary>What is Binary Heap?</summary>

### What is Binary Heap?

**Definition:**

A binary heap is a complete binary tree with a parent-child order, stored in an array. It is a priority queue, not a search tree.

**How it works:**

- Complete means every level except possibly the last is full, and the last level is packed from the left. Children of index `i` are `2i+1` and `2i+2`.
- A min-heap puts the smallest key at the root. A max-heap puts the largest there. Siblings are not ordered.
- Insert sifts up and delete-extreme sifts down, both O(log n). Search for an arbitrary key is O(n).
- Do not confuse it with a binary search tree, where the left subtree is smaller than the node and the right is larger.
</details>

<details>
<summary>What is a Binary Search Tree?</summary>

### What is a Binary Search Tree?

**Definition:**

A binary search tree orders keys: every key in the left subtree is smaller than the node, and every key in the right subtree is larger.

**How it works:**

- Search, insert, and delete follow one path from the root and cost O(height). On a balanced tree that is O(log n). On a sorted insertion order it is O(n).
- Inorder traversal visits keys in sorted order.
- Delete of a node with two children replaces it with its inorder successor or predecessor, then deletes that stand-in from its old place.
- Equal keys need a fixed policy: reject them, or send them consistently left or right. The comparisons must stay transitive.

**Examples:**

Insert 5, then 2, then 8, then 1. Search for 1 goes left of 5, left of 2, and finds 1.
</details>

<details>
<summary>What is AVL Tree? How to Balance it?</summary>

### What is AVL Tree? How to Balance it?

**Definition:**

An AVL tree is a binary search tree where the heights of the two children of every node differ by at most one.

**How it works:**

- Each node stores a balance factor: height(left) − height(right), in {−1, 0, +1}.
- Insert and delete update heights on the way back up. The first node whose factor becomes ±2 is rebalanced with a rotation.
- A left-left or right-right imbalance is one rotation. A left-right or right-left imbalance is a double rotation: rotate the child, then the node.
- Height stays about 1.44 log₂ n, so search, insert, and delete are O(log n). AVL rotates more often than a red-black tree and looks up a bit faster.
</details>

<details>
<summary>What is a Red-Black Tree?</summary>

### What is a Red-Black Tree?

**Definition:**

A red-black tree is a binary search tree with a color on each node and rules that keep the longest path at most twice the shortest.

**How it works:**

- Every node is red or black. The root is black. A red node has only black children. Every path from a node to a leaf sentinel has the same number of black nodes.
- Insert colors the new node red and repairs with recoloring and at most a couple of rotations. Delete has more cases and still a constant number of rotations.
- The height is O(log n), so the map operations are O(log n). The bound is looser than AVL, and updates rotate less.
- C++ `std::map` and Java `TreeMap` are typical red-black trees.
</details>

<details>
<summary>How is an AVL Tree different from a B-Tree?</summary>

### How is an AVL Tree different from a B-Tree?

**Definition:**

An AVL tree is a binary search tree balanced by height, aimed at in-memory lookup. A B-tree is a wide search tree aimed at disks and pages.

**How it works:**

- AVL nodes have one key and at most two children. B-tree nodes have many keys and many children, sized to a page.
- AVL rotates to fix a height gap of 2. A B-tree splits a full node and merges or borrows on delete to keep a minimum occupancy.
- An AVL lookup touches O(log n) nodes, and those nodes are small. A B-tree lookup touches O(log_b n) pages, which is the cost that matters on disk.
- Databases and file systems use B-trees or B+ trees. Language library maps use AVL or red-black trees.
</details>

<details>
<summary>How can a Fenwick Tree (Binary Indexed Tree) be beneficial in algorithm design?</summary>

### How can a Fenwick Tree (Binary Indexed Tree) be beneficial in algorithm design?

**Definition:**

A Fenwick tree stores prefix aggregates in an array so a point update and a prefix query both walk O(log n) indexes.

**How it works:**

- Index `i` is responsible for a range whose length is the lowest set bit of `i`. Adding that bit moves to the next prefix. Clearing it moves to the parent.
- It uses n integers and no pointers. It is simpler and tighter than a segment tree when you only need prefix sums or point updates.
- Range sums are `prefix(r) − prefix(l−1)`. Range updates need a second tree or a difference-array trick.
- It does not store the original array's arbitrary ranges as flexibly as a segment tree, and it wants 1-based indexes.

**Examples:**

Frequencies `[0, 3, 2, −1]`. `add(2, 5)` and `prefix(3)` each touch a couple of slots, not the whole prefix.
</details>

<details>
<summary>What is a Segment Tree, and how does it differ from a traditional binary tree in usage and efficiency?</summary>

### What is a Segment Tree, and how does it differ from a traditional binary tree in usage and efficiency?

**Definition:**

A segment tree is a full binary tree over an array, where each node stores the aggregate of a half-open range.

**How it works:**

- The root is `[0, n)`. Each internal node splits its range in half. Leaves are single indexes.
- A point update recomputes O(log n) ancestors. A range query visits O(log n) nodes whose ranges cover the query without a linear scan.
- It is built for 'sum, min, or max on a range that changes', not for ordered-key search. A BST does the opposite.
- Memory is about 4n nodes in the array layout. Build is O(n).

**Examples:**

Array `[2, 1, 5, 3]`. The node for `[0, 2)` stores `2+1`. A query `[1, 4)` combines the leaf `1` with the node for `[2, 4)`.
</details>

<details>
<summary>What is a Splay Tree, and how does its splay operation work?</summary>

### What is a Splay Tree, and how does its splay operation work?

**Definition:**

A splay tree is a binary search tree that rotates the accessed node up to the root, so recently used keys stay near the top.

**How it works:**

- Splay uses zig, zig-zig, and zig-zag rotations along the path, not a single rotate-to-root that can leave the tree skinny.
- Search, insert, and delete splay the touched node. A missing search splays the last node visited.
- One operation can be O(n). Over a sequence, the amortized cost is O(log n) per operation.
- It needs no balance bits. It is a good fit for locality: the same key looked up again is now at the root.
</details>

<details>
<summary>Explain the concept and structure of a Ternary Tree.</summary>

### Explain the concept and structure of a Ternary Tree.

**Definition:**

A ternary tree gives each node up to three children. In a ternary search tree those children mean less, equal, and greater for one character.

**How it works:**

- A plain ternary tree is just a branching factor of three, used for 3-ary heaps or expression nodes with three operands.
- A ternary search tree stores a character in the node. The left child is a smaller character, the right a larger one, and the middle child is the next character of the same word.
- Lookup of a word of length L is O(L log σ) in the character alphabet when the side trees are balanced, and it can share prefixes the way a trie does, with less empty space than a 26-way trie node.
- It is a string index, not a balanced BST of whole keys.
</details>

<details>
<summary>Describe a Lazy Segment Tree and when it is used over a regular Segment Tree.</summary>

### Describe a Lazy Segment Tree and when it is used over a regular Segment Tree.

**Definition:**

A lazy segment tree postpones range updates by storing a pending tag on a node, and pushes that tag to the children only when a later query needs them.

**How it works:**

- A range add on `[L, R)` marks O(log n) canonical nodes and updates their aggregates immediately. Their children stay stale until a push.
- A query pushes tags on the way down, so it sees the updates that cover its path. Both update and query stay O(log n).
- Without laziness, adding a value to a range of length n would edit n leaves. That is the case the lazy tree avoids.
- The tag must compose: two pending adds become one add. Assignment tags overwrite adds. Getting that composition wrong corrupts the tree.
</details>

<details>
<summary>What is a Treap, and how does it combine the properties of a binary search tree and a heap?</summary>

### What is a Treap, and how does it combine the properties of a binary search tree and a heap?

**Definition:**

A treap stores a key and a random priority in each node. Keys obey the BST order. Priorities obey the heap order.

**How it works:**

- Insert searches by key, then rotates the new node up until its priority is in heap order with its parent.
- Delete rotates the node down until it is a leaf, preferring the child with the better priority, then unlinks it.
- Random priorities make the expected height O(log n). There is no separate balance factor.
- Split and merge by key are the operations that make implicit treaps useful for sequences.
</details>

### Tree Properties and Operations

<details>
<summary>What is a Balanced Tree?</summary>

### What is a Balanced Tree?

**Definition:**

A balanced tree keeps its height logarithmic in the number of nodes, so a root-to-leaf path stays short after updates.

**How it works:**

- AVL, red-black trees, B-trees, and treaps with random priorities are balanced in their own senses.
- A plain BST inserted in sorted order is a chain and is not balanced.
- The guarantee is what makes map operations O(log n) rather than O(n).
- Balance has a cost: extra fields, rotations, or wider nodes. A write-once sorted array needs none of that.
</details>

<details>
<summary>What are advantages and disadvantages of BST?</summary>

### What are advantages and disadvantages of BST?

**Definition:**

A BST gives ordered search, successor, and range queries. Without balance, those operations follow the height and can be linear.

**How it works:**

- Inorder is the sorted sequence. Range queries walk only the relevant slice.
- A balanced BST does insert, delete, and search in O(log n) and keeps keys ordered, which a hash table does not.
- An unbalanced BST degrades to a list. You either randomize insertion, use a treap, or use AVL or red-black rules.
- Pointer nodes are heavier and less cache-friendly than a sorted array. Prefer the array when the data is static.
</details>

<details>
<summary>How does Inserting or Deleting nodes affect a Red-Black Tree?</summary>

### How does Inserting or Deleting nodes affect a Red-Black Tree?

**Definition:**
Insert and delete can break the color rules or the equal black-height rule. The tree repairs the path with recoloring and rotations so the height stays O(log n) and search, insert, and delete remain O(log n).

**How it works:**
- The introduction states a height limit of O(log n) and worst-case search, insertion, and deletion in O(log n). A plain BST can take O(n) when it degenerates.
- Insert places the key by the BST rule and colors the new node red, then repairs red-red pairs. The insert article states time O(log N) and space O(N) for the tree of N nodes.
- Delete removes the BST node, then restores the black-height. The introduction includes deletion in the same O(log n) worst case. Rotations change a constant number of child pointers; the AVL delete page, for the same kind of rotation, says a rotation is constant time.
- **Time:** O(log n) for insert and delete. **Space:** O(N) to store the tree, from the insert article.

**Examples:**
```text
insert 10, 20, 30 into an empty red-black tree
30 would make a right spine; a rotation makes 20 the black root
left 10, right 30
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
- https://www.geeksforgeeks.org/dsa/red-black-tree-set-2-insert/
</details>

<details>
<summary>What is the time complexity for Insert into Red-Black Tree?</summary>

### What is the time complexity for Insert into Red-Black Tree?

**Definition:**
Insert into a red-black tree is O(log N) time. N is the number of nodes. The walk down is the BST search, and the repair on the way up does not change that bound.

**How it works:**
- The insert article states time complexity O(log N) and space complexity O(N), where N is the number of nodes in the tree.
- The introduction states the same worst-case bound for insertion: O(log n), because the height is O(log n). A skewed BST insert is O(n).
- The new node is colored red. Recoloring and at most a short sequence of rotations fix a red parent. Those rotations are pointer rewrites on the path.
- **Time:** O(log N). **Space:** O(N) for the stored nodes. The recursive insert, like the AVL recursive insert, also uses a call stack; the AVL article states that stack as O(log n) on a balanced tree.

**Examples:**
```text
def insert(root, key):  # then fix red-red on the path
    if root is None:
        return Node(key, RED)
    if key < root.key:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)
    return repair(root)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/red-black-tree-set-2-insert/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
</details>

<details>
<summary>How does tree balancing improve search operation performance in a tree?</summary>

### How does tree balancing improve search operation performance in a tree?

**Definition:**
Search in a BST follows one root-to-leaf path, so it costs O(h). Balance keeps h at O(log n). Without it, sorted inserts make a chain and search costs O(n).

**How it works:**
- The BST article states search as O(h) time. The recursive form uses O(h) auxiliary space. The iterative form is O(h) time and O(1) auxiliary space.
- The AVL article says BST operations are O(h) and become O(n) on a skewed tree. Keeping the height O(log n) after every insert and delete makes the upper bound O(log n). An AVL tree's height is always O(log n).
- A red-black tree states the same improvement as a height limit of O(log n) and search in O(log n), against O(n) for a plain BST.
- **Time:** O(log n) on an AVL or red-black tree, O(h) on a plain BST, O(n) when the BST is a chain. **Space:** O(1) extra for an iterative search.

**Examples:**
```text
# sorted inserts 1,2,3,4: search 4 walks 4 nodes
# after AVL rotations the same search walks about log2(4) nodes
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
</details>

<details>
<summary>Discuss the concept of immutable binary trees and its implications in functional programming.</summary>

### Discuss the concept of immutable binary trees and its implications in functional programming.

**Definition:**
An immutable binary tree never changes a node in place. An update builds a new root. Subtrees that the update did not touch are shared with the old root, so the previous version stays valid.

**How it works:**
- The persistent-structures article keeps every version. One method copies the nodes that must change and reconnects them. Unchanged nodes stay as they are, which is what lets two versions share a subtree.
- The fat-node method on that page, from Sleator, Tarjan, and their team, stores a modification history in the node. The article states amortized O(1) time and space for access and updates.
- A search still follows the BST rule on whichever root you hold. The BST article's search is O(h) time. Balance still matters: a shared spine of length n is an O(n) search on that version.
- Functional code returns the new root from insert. Callers that kept the old root still see the old tree.
- **Time:** BST search O(h) on the version you hold. Fat-node access and update are amortized O(1) on that page. **Space:** unchanged nodes are shared; the fat-node method is amortized O(1) extra.

**Examples:**
```text
def insert(node, key):
    if node is None:
        return Node(key)
    if key < node.key:
        return Node(node.key, insert(node.left, key), node.right)
    return Node(node.key, node.left, insert(node.right, key))
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/persistent-data-structures/
- https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/
</details>

### Comparisons Between Tree Varieties

<details>
<summary>Explain the difference between Binary Tree and Binary Search Tree.</summary>

### Explain the difference between Binary Tree and Binary Search Tree.

**Definition:**
A binary tree gives each node at most two children and no key order. A BST is a binary tree whose left subtree holds smaller keys and whose right subtree holds larger keys, with the same rule in every subtree.

**How it works:**
- The comparison article states average insert, delete, and search as O(n) on a binary tree, because a lookup may walk the whole tree. On a BST those three are O(h), where h is the height.
- Both use O(n) space. The space depends on the number of nodes, not on the order.
- BST insertion follows the key. Binary-tree insertion has no key rule. The BST article's search is O(h) time, O(h) stack when recursive, O(1) extra when iterative.
- Inorder on a BST yields sorted keys. Inorder on a plain binary tree is only left-node-right.
- **Time:** O(n) average for binary-tree search; O(h) for BST search. **Space:** O(n) to store either tree.

**Examples:**
```text
# binary tree:  5 with left 9 and right 1  — legal
# BST:          5 with left 1 and right 9  — required
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/difference-between-binary-tree-and-binary-search-tree/
- https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/
</details>

<details>
<summary>What is the difference between Heap and Red-Black Tree?</summary>

### What is the difference between Heap and Red-Black Tree?

**Definition:**
A binary heap is a complete binary tree with the heap order: the parent is greater than or equal to its children in a max-heap, or less than or equal in a min-heap. A red-black tree is a BST with color rules. The heap finds the extreme key quickly. The red-black tree finds an arbitrary key quickly.

**How it works:**
- The heap introduction states heapify, insertion, and deletion of the root as O(log n). getMax or getMin is O(1) because the extreme sits at the root.
- The red-black introduction states search, insertion, and deletion as O(log n) in the worst case, with height O(log n).
- A heap does not keep left-smaller and right-larger. Finding an arbitrary key is not one of the O(log n) heap operations above. A red-black search uses that BST order.
- Heaps are stored in arrays. Red-black nodes store a color and two child pointers. The heap is the usual priority queue. The red-black tree is the usual ordered map.
- **Time:** heap insert and extract O(log n), peek O(1). Red-black search, insert, and delete O(log n).

**Examples:**
```text
# max-heap array [10, 7, 9]: root 10, children need not be sorted
# red-black: search 7 goes left or right by key, O(log n)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/introduction-to-heap-data-structure/
- https://www.geeksforgeeks.org/dsa/binary-heap/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
</details>

<details>
<summary>Compare Trie vs. Binary Search Trie.</summary>

### Compare Trie vs. Binary Search Trie.

**Definition:**
A trie stores one character per edge, so a word of length n is a path of n steps. Insert and search are O(n) in that length. A BST stores a whole key in a node and searches in O(h), which becomes O(n) when the tree is skewed.

**How it works:**
- The trie article states insert as O(n) time and O(n) auxiliary space, and search as O(n) time and O(1) auxiliary space, where n is the length of the word. It says this is faster than a BST.
- Prefix search is also O(n) time and O(1) auxiliary space. The walk stops at the last character of the prefix. Every word under that node shares the prefix.
- BST search is O(h) time. A balanced tree makes h O(log N) for N keys, from the AVL and red-black articles, but the comparison is on whole keys, not on characters, and it does not share prefixes.
- A ternary search tree is the character BST: less, equal, and greater children. The plain trie is the fixed-alphabet node. The bounds above are for the trie article's insert and search.
- **Time:** trie insert and search O(n) in the word length. BST search O(h). **Space:** trie insert O(n) auxiliary for a new word; trie search O(1) auxiliary.

**Examples:**
```text
insert "cat", then "car"
trie: c -> a -> t
             -> r
BST of whole keys compares "cat" and "car" at one node
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/trie-insert-and-search/
- https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
</details>

<details>
<summary>Compare Red-Black Trees and AVL Trees.</summary>

### Compare Red-Black Trees and AVL Trees.

**Definition:**
Both are BSTs whose height stays O(log n), so search, insert, and delete are O(log n). AVL keeps the heights of the two children within 1 and rotates as soon as the balance factor leaves {−1, 0, +1}. A red-black tree allows a looser height and repairs with colors.

**How it works:**
- The AVL insert article states height O(log n), insert time O(log n), and auxiliary space O(log n) for the recursive call stack. Delete is also O(log n) time and O(log n) auxiliary space. Rotations themselves are constant time.
- The red-black introduction states height O(log n) and worst-case search, insert, and delete O(log n). The insert article states insert time O(log N) and stored space O(N).
- AVL is the stricter balance, so a lookup path is a bit shorter and updates rotate more often. Red-black updates recolor first and rotate less. Both beat the O(n) skewed BST.
- **Time:** O(log n) operations on both. **Space:** O(log n) recursion stack for the AVL recursive insert and delete; O(N) to store either tree.

**Examples:**
```text
# insert sorted 1..n
# AVL: balance factor stays in -1..1, height O(log n)
# red-black: black-height stays equal, height O(log n)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-2-deletion/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
- https://www.geeksforgeeks.org/dsa/red-black-tree-set-2-insert/
</details>

<details>
<summary>When standard BSTs might be preferred over AVL Trees?</summary>

### When standard BSTs might be preferred over AVL Trees?

**Definition:**
A plain BST is enough when the height is already short, so the extra rotations of an AVL tree do not pay. Search, insert, and delete are O(h) either way. AVL spends the rotation work to force h to O(log n).

**How it works:**
- The BST article states search as O(h) time. Iterative search is O(1) auxiliary space. There is no balance factor and no rotation.
- The AVL article says the O(n) cost appears when the tree is skewed, and that AVL insert is O(log n) including the rotations, with O(log n) stack. If inserts are not sorted, h often stays near log n without those rotations.
- A tree that is built once and then only searched never needs delete rebalancing. The AVL delete article still costs O(log n) when you do delete on an AVL tree.
- Prefer AVL or red-black when the insert order can be sorted. The red-black and AVL articles both exist to replace the O(n) chain.
- **Time:** BST O(h), which is O(n) if skewed and O(log n) if the height is already logarithmic. AVL insert and delete O(log n).

**Examples:**
```text
# random keys: a plain BST search is O(h) with h near log n
# keys 1,2,3,...,n: the plain BST is a chain, O(n); use AVL
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-2-deletion/
</details>

<details>
<summary>Compare the performance of a Splay Tree versus a Red-Black Tree under various operations.</summary>

### Compare the performance of a Splay Tree versus a Red-Black Tree under various operations.

**Definition:**
A splay tree moves the accessed node to the root. One operation can take O(n) time. Across a sequence, the splay article states O(log n) average time per operation. A red-black tree bounds every search, insert, and delete by O(log n) in the worst case.

**How it works:**
- The splay article says a single BST operation is O(n) when the tree is skewed, and AVL or red-black make the worst case O(log n). It then states that any single splay operation can take Θ(n) time in the worst case, while all splay operations run in O(log n) time on average. A recently used key is then at the root, so the next lookup of that key is O(1).
- M operations are described as O(M log N) total, even though one of them may cost O(N).
- Red-black search, insert, and delete are O(log n) worst case, height O(log n). There is no extra O(1) for a repeated lookup unless the key is already the root.
- Splay stores no color and no balance factor. Red-black stores a color and repairs on the way up.
- **Time:** splay amortized / average O(log n), worst single operation Θ(n). Red-black worst case O(log n).

**Examples:**
```text
# lookup 5 twice in a splay tree:
# first walk can be long, then 5 is the root
# second lookup starts at 5
# red-black: both lookups are O(log n)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/splay-tree-set-1-insert/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
</details>

<details>
<summary>How does a B+ Tree differ from a B-Tree and in what situations might it be preferred?</summary>

### How does a B+ Tree differ from a B-Tree and in what situations might it be preferred?

**Definition:**
A B-tree stores keys and data in internal nodes and in leaves. A B+ tree stores data only in the leaves. Internal nodes hold keys for navigation, and the leaves form a linked list. Range scans prefer the B+ tree. A point update that can stop inside an internal node is the B-tree case.

**How it works:**
- The B-tree article states search, insert, and delete as O(log n), and a full traverse as O(n).
- The B+ article stores every key's record in a leaf, links the leaves with Pnext, and keeps internal nodes as routing keys only. A search starts at the root, walks internal nodes, and finishes at a leaf. All leaves sit on one level.
- Insert splits a full leaf and pushes a copy of the middle key up. Delete borrows from a sibling or merges. The article says a B+ tree of the same height can hold more entries in internal nodes than a B-tree, because those nodes carry no records, and that this shortens the search.
- Prefer B+ for database and file indexes with range queries: the leaf list is sequential. The article also notes the cost: an exact match still walks to a leaf, and the leaf links take extra pointers.
- **Time:** B-tree search, insert, and delete O(log n). **Space:** B+ uses extra leaf pointers; internal nodes hold keys only.

**Examples:**
```text
# B-tree: internal node may hold the record for key 10
# B+ tree: 10 in an internal node is only a router
# the record lives in the leaf, whose next leaf holds 11, 12, ...
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/introduction-of-b-tree/
- https://www.geeksforgeeks.org/dsa/b-tree-set-1-introduction-2/
</details>

### Tree Implementations and Traversals

<details>
<summary>Name some ways to implement Priority Queue.</summary>

### Name some ways to implement Priority Queue.

**Definition:**
A priority queue extracts the smallest or the largest key next. A binary heap does insert and extract in O(log n) and peek in O(1). A balanced BST such as a red-black tree does insert, find-min, and delete-min in O(log n) by the ordered-key bounds.

**How it works:**
- The priority-queue article states O(log n) for the operations except getMax, which is O(1), and space O(n). That matches the heap introduction: insert, delete-root, and heapify are O(log n), getMax and getMin are O(1).
- A red-black tree supports the same ordered operations in O(log n) worst case. The minimum is the leftmost node. Delete of that node is the red-black delete, also O(log n).
- The heap uses an array and no BST pointers. The tree keeps keys sorted, so it can also search for an arbitrary key in O(log n), which the heap's stated operations do not include.
- **Time:** heap insert and extract O(log n), peek O(1). Red-black insert and delete-min O(log n). **Space:** O(n) for the heap priority queue.

**Examples:**
```text
# heap: push 4, 1, 7; peek() == 1 for a min-heap; pop() == 1
# red-black: insert the same keys; delete the leftmost node
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/priority-queue-set-1-introduction/
- https://www.geeksforgeeks.org/dsa/introduction-to-heap-data-structure/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
</details>

<details>
<summary>Classify Tree Traversal Algorithms.</summary>

### Classify Tree Traversal Algorithms.

**Definition:**
Traversals are depth-first or breadth-first. Depth-first is preorder (node, left, right), inorder (left, node, right), and postorder (left, right, node). Breadth-first is level order, one level at a time through a queue.

**How it works:**
- Iterative preorder with a stack is O(n) time and O(n) space. Morris preorder is O(n) time and O(1) space.
- Morris inorder walks every edge a constant number of times: O(n) time and O(1) auxiliary space.
- Level order by a queue is O(n) time and O(n) space. The recursive level-order on that page is the same O(n) time and O(n) space.
- Preorder, inorder, and postorder are one DFS family. Only the moment when the node is recorded changes. Level order is BFS.
- **Time:** O(n) for each of these full walks. **Space:** O(n) for a stack or a queue, O(1) extra for Morris.

**Examples:**
```text
# tree: 1 with left 2 and right 3; 2 has left 4
# preorder  1 2 4 3
# inorder   4 2 1 3
# postorder 4 2 3 1
# level     1 2 3 4
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
- https://www.geeksforgeeks.org/dsa/inorder-tree-traversal-without-recursion-and-without-stack/
- https://www.geeksforgeeks.org/dsa/level-order-tree-traversal/
</details>

<details>
<summary>Implement Pre-order Traversal of Binary Tree using Recursion.</summary>

### Implement Pre-order Traversal of Binary Tree using Recursion.

**Definition:**
Preorder visits the node, then the left subtree, then the right subtree. The recursive function records the node and makes those two calls. A null child returns immediately.

**How it works:**
- Every node is visited once, which is the O(n) time the iterative preorder article states for its stack walks and for Morris.
- The stack form on that page is O(n) space. Morris preorder is O(n) time and O(1) space because it does not keep a stack. The recursive form uses the call stack in place of that explicit stack.
- The order is fixed: emit, left, right. Swapping the two calls produces a mirror preorder, not the standard one.
- **Time:** O(n). **Space:** O(n) for the explicit-stack preorder on that page; Morris is O(1) extra.

**Examples:**
```text
def preorder(node):
    if node is None:
        return
    print(node.data)
    preorder(node.left)
    preorder(node.right)
# 1, left 2, right 3 -> 1 2 3
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
</details>

<details>
<summary>Implement Iterative Pre-order Traversal of a Binary Tree without Recursion.</summary>

### Implement Iterative Pre-order Traversal of a Binary Tree without Recursion.

**Definition:**
Push the root on a stack. Pop a node, record it, then push its right child and its left child. The left child is popped next. Morris preorder removes the stack by temporarily threading each node to its predecessor.

**How it works:**
- The article's simple stack preorder is O(n) time and O(n) space. The variant that keeps a current pointer and a stack is the same O(n) time and O(n) space.
- Morris preorder is O(n) time and O(1) space. Every node is still visited once, and the temporary threads are removed before the walk finishes.
- Push right before left. A stack pops the latest push, so the left subtree is processed before the right, matching recursive preorder.
- **Time:** O(n). **Space:** O(n) with a stack, O(1) extra with Morris.

**Examples:**
```text
st = [root]
while st:
    node = st.pop()
    if node is None:
        continue
    print(node.data)
    st.append(node.right)
    st.append(node.left)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
</details>

<details>
<summary>Convert a Binary Tree into a Doubly Linked List.</summary>

### Convert a Binary Tree into a Doubly Linked List.

**Definition:**
Inorder visits left, node, right. Point each node's left at the previous inorder node and the previous node's right at the current node. The first inorder node is the list head.

**How it works:**
- Searching a predecessor at every node is O(n) time and O(h) space. One inorder pass that keeps the previous pointer is O(n) time and O(h) space.
- Morris conversion is O(n) time and O(1) extra space. The threads find the predecessor without a stack, then become the list links.
- The list is inorder order. On a BST that is sorted order. On a plain binary tree it is still left-node-right.
- **Time:** O(n). **Space:** O(h) for the recursive pass, O(1) extra for Morris.

**Examples:**
```text
prev = None
def join(node):
    global prev
    if node is None:
        return
    join(node.left)
    node.left = prev
    if prev:
        prev.right = node
    prev = node
    join(node.right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convert-binary-tree-to-doubly-linked-list-using-inorder-traversal/
</details>

<details>
<summary>Build a Binary Expression Tree for the given expression.</summary>

### Build a Binary Expression Tree for the given expression.

**Definition:**
An expression tree stores an operator in each internal node and an operand in each leaf. Inorder with parentheses prints the expression. The article builds the tree from a postfix scan: operands become leaves, and an operator pops two trees and becomes their parent.

**How it works:**
- The article states time O(n) and auxiliary space O(n). One pass over the expression pushes nodes. The stack holds subtrees that do not yet have a parent.
- For an operator, the right child is popped first, then the left child, because the stack stored the left operand earlier.
- Inorder of `A + B * C / D` is the example the article prints. The operator nodes sit above their operands, so precedence is the shape of the tree, not a later scan.
- **Time:** O(n). **Space:** O(n) auxiliary.

**Examples:**
```text
# postfix A B C * +
# push A, push B, push C
# '*' pops C and B -> node(*, B, C)
# '+' pops that node and A -> node(+, A, node(*, B, C))
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/expression-tree/
</details>

<details>
<summary>What is Morris Traversal for a Tree? How to implement one?</summary>

### What is Morris Traversal for a Tree? How to implement one?

**Definition:**
Morris traversal walks a binary tree with no stack and no recursion. A null right pointer of a node's inorder predecessor is temporarily pointed at the node. The second time that thread is seen, it is removed and the node is processed. The threaded-tree idea is the same link, stored for good instead of temporarily.

**How it works:**
- The Morris inorder article states O(n) time, because every edge is traversed at most three times, and O(1) auxiliary space.
- Morris preorder on the iterative-preorder page is also O(n) time and O(1) space.
- If the left child is null, visit the node and go right. Otherwise find the predecessor, thread it if it is unthreaded, and go left. If it is already threaded, remove the thread, visit the node, and go right.
- A permanent threaded tree uses those null links as predecessor or successor pointers so a later walk can follow them. Morris creates the thread only for the duration of the visit.
- **Time:** O(n). **Space:** O(1) auxiliary.

**Examples:**
```text
# Morris inorder step when node has a left child:
# pred = rightmost of node.left
# if pred.right is None: pred.right = node; go left
# else: pred.right = None; visit node; go right
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/inorder-tree-traversal-without-recursion-and-without-stack/
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
- https://www.geeksforgeeks.org/dsa/threaded-binary-tree/
</details>

<details>
<summary>Explain how to perform level-order traversal in a binary tree and its applications.</summary>

### Explain how to perform level-order traversal in a binary tree and its applications.

**Definition:**
Level order visits the root, then both of its children, then their children. A queue holds the next nodes. The recursive form on the same page groups nodes by depth. Either way every node is visited once.

**How it works:**
- The queue method is O(n) time and O(n) space. Push the root. While the queue is not empty, pop a node, record it, and push its non-null children.
- The recursive method is also O(n) time and O(n) space. It starts at level 0 and appends the node to that level's list.
- The width of the queue is the widest level, which the article bounds by O(n) space for the whole walk.
- Uses that need levels: the leftmost or rightmost node of each level, the depth of the tree, and a breadth-first shortest path in an unweighted tree.
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
from collections import deque
q = deque([root])
while q:
    node = q.popleft()
    print(node.data)
    if node.left: q.append(node.left)
    if node.right: q.append(node.right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/level-order-tree-traversal/
</details>

<details>
<summary>Describe the use of BFS and DFS in binary trees and how they relate to tree traversals.</summary>

### Describe the use of BFS and DFS in binary trees and how they relate to tree traversals.

**Definition:**
Breadth-first search on a binary tree is level order: a queue expands both children after the node. Depth-first search is preorder, inorder, or postorder: the call stack, or an explicit stack, finishes one subtree before the other.

**How it works:**
- Level order is O(n) time and O(n) space with a queue.
- Iterative preorder, the DFS that records the node before its children, is O(n) time and O(n) space with a stack. Morris DFS is O(n) time and O(1) space.
- BFS finds the shallowest node that matches a test, because level k is finished before level k+1. DFS uses less auxiliary memory on a balanced tree when Morris applies, O(1), and it is the walk that expression trees and BST inorder need.
- A tree has no cross edge to a visited cousin if you only follow child pointers, so the visited set that graph DFS needs is optional. The stack or the queue is the whole difference.
- **Time:** O(n) for a full BFS or DFS. **Space:** O(n) for the queue or the explicit stack, O(1) extra for Morris.

**Examples:**
```text
# BFS: queue, level order 1 2 3 4
# DFS preorder: stack, 1 2 4 3
# same tree, different container
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/level-order-tree-traversal/
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
- https://www.geeksforgeeks.org/dsa/inorder-tree-traversal-without-recursion-and-without-stack/
</details>

### Advanced Tree Concepts and Applications

<details>
<summary>Implement a Map data structure using Binary Search Tree.</summary>

### Implement a Map data structure using Binary Search Tree.

**Definition:**
A map from this BST stores a key and a value in each node. Lookup, insert, and update follow one path: left when the key is smaller, right when it is larger. The BST article states that search as O(h) time.

**How it works:**
- Recursive search is O(h) time and O(h) auxiliary space. Iterative search is O(h) time and O(1) auxiliary space.
- On an AVL or red-black tree the height is O(log n), so those operations are O(log n). A sorted insert order without balance is O(n).
- Insert creates a leaf when the walk falls off the tree. An equal key replaces the value and does not add a second node. The comparison article says a BST has no duplicate nodes under the strict order.
- Inorder walks the map in key order. A hash map does not.
- **Time:** O(h), which is O(log n) when the tree is an AVL or red-black tree. **Space:** O(1) extra for iterative search, O(n) to store the nodes.

**Examples:**
```text
def get(node, key):
    while node:
        if key == node.key:
            return node.value
        node = node.left if key < node.key else node.right
    return None
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/binary-search-tree-set-1-search-and-insertion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
- https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
- https://www.geeksforgeeks.org/dsa/difference-between-binary-tree-and-binary-search-tree/
</details>

<details>
<summary>Explain the process of Tree Rotation and its use cases.</summary>

### Explain the process of Tree Rotation and its use cases.

**Definition:**
A rotation rewrites three child pointers so one node moves up and the BST inorder stays the same. AVL and red-black trees use it after insert or delete when the shape would be too tall. The AVL delete article says a left or right rotation is constant time because only a few pointers change.

**How it works:**
- A right rotation of node y, whose left child is x, makes x the parent. y becomes x's right child. x's old right subtree becomes y's left subtree.
- A left rotation is the mirror. AVL uses one rotation for a straight imbalance and two for a zigzag. The insert article keeps the height O(log n), so the whole insert, rotations included, is O(log n) time with O(log n) stack.
- Red-black insert is O(log N) time. Recoloring handles some cases. A rotation handles the rest, still on a path of length O(log n).
- Splay rotations move the accessed node all the way to the root. One splay can be Θ(n). The average is O(log n).
- **Time:** one rotation is constant time. AVL insert that uses rotations is O(log n).

**Examples:**
```text
# right_rotate(y):
# x = y.left
# y.left = x.right
# x.right = y
# return x
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/avl-tree-set-2-deletion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
- https://www.geeksforgeeks.org/dsa/red-black-tree-set-2-insert/
- https://www.geeksforgeeks.org/dsa/splay-tree-set-1-insert/
</details>

<details>
<summary>Discuss the concept of Threaded Binary Trees.</summary>

### Discuss the concept of Threaded Binary Trees.

**Definition:**
A threaded binary tree stores the inorder predecessor or successor in a child pointer that would otherwise be null. Morris traversal uses the same kind of thread only while the walk is running, then removes it. The permanent thread lets the next inorder step follow a pointer instead of a stack.

**How it works:**
- The Morris inorder article states O(n) time and O(1) auxiliary space. Every edge is traversed at most three times. That is the bound for a walk that follows temporary threads.
- A right thread points at the inorder successor. A left thread points at the inorder predecessor. A bit on the node tells a thread from a real child, otherwise a later insert would treat the successor as a subtree.
- Morris preorder is the other O(n) time, O(1) space walk on the iterative-preorder page.
- Building the threads is a linear inorder pass. After that, the successor of a node with a right thread is that pointer. The successor of a node with a real right child is the leftmost node of that child.
- **Time:** O(n) for a Morris walk. **Space:** O(1) auxiliary.

**Examples:**
```text
# node with no right child:
# node.right = inorder successor
# node.right_is_thread = True
# Morris sets that link, visits, then clears it
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/threaded-binary-tree/
- https://www.geeksforgeeks.org/dsa/inorder-tree-traversal-without-recursion-and-without-stack/
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
</details>

<details>
<summary>In what scenarios would a Recursive Tree Traversal be impractical?</summary>

### In what scenarios would a Recursive Tree Traversal be impractical?

**Definition:**
Recursive traversal is impractical when the depth can reach n, because each call keeps a frame and the interview notes bound that auxiliary space by O(n) in the worst case. A chain-shaped tree is that case. The call stack overflows before the walk finishes.

**How it works:**
- A balanced AVL or red-black tree has height O(log n), so the recursive stack is the O(log n) the AVL insert article states. A skewed BST is O(n) tall, and the same recursion is O(n) frames.
- Morris inorder and Morris preorder are O(n) time and O(1) auxiliary space. They are the walks to use when the height is unknown and a stack of n frames is not acceptable.
- An explicit stack, as in iterative preorder, is O(n) space too, but it is heap memory rather than the smaller native call stack. The recursion interview notes recommend that replacement when depth can overflow.
- **Time:** the walk is still O(n). **Space:** recursive O(height), up to O(n); Morris O(1) extra.

**Examples:**
```text
# skewed right spine of 100000 nodes:
# recursive inorder: 100000 frames
# Morris inorder: a few pointers, O(1) extra
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
- https://www.geeksforgeeks.org/dsa/inorder-tree-traversal-without-recursion-and-without-stack/
- https://www.geeksforgeeks.org/dsa/iterative-preorder-traversal/
</details>

<details>
<summary>Discuss the Space Complexity for storing a binary tree.</summary>

### Discuss the Space Complexity for storing a binary tree.

**Definition:**
Storing a binary tree takes space proportional to the number of nodes. The comparison article states O(n) space for a binary tree and O(n) space for a BST. Each node holds a value and two child links. Balance bits or a color are a constant factor on top of that.

**How it works:**
- n nodes mean n values and up to 2n child pointers. Leaves store null children. The O(n) bound does not change if many children are null.
- A perfect tree of height h has 2^(h+1) − 1 nodes, from the perfect-tree article's total-node formula. The array layout stores a node at index i with children at 2i+1 and 2i+2. That array is still Θ(n) for a perfect tree.
- Traversal space is separate. A queue level order is O(n) extra. Morris traversal is O(1) extra and does not change the O(n) storage of the tree itself.
- **Space:** O(n) to store the tree.

**Examples:**
```text
# n = 3: nodes [5, 1, 9], six child slots, four of them null
# still O(n) nodes allocated
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/difference-between-binary-tree-and-binary-search-tree/
- https://www.geeksforgeeks.org/dsa/perfect-binary-tree/
</details>

<details>
<summary>How to determine if a given binary tree is a full binary tree?</summary>

### How to determine if a given binary tree is a full binary tree?

**Definition:**
A full binary tree gives every node either zero children or two. One child makes it not full. The check walks every node and rejects the first node that has exactly one child.

**How it works:**
- The recursive article states O(n) time and O(n) space. A null node is accepted. A leaf is accepted. A node with both children continues into both. A node with one child returns false.
- Level order on the same page is also O(n) time and O(n) space. The queue visits every node and applies the same test.
- This is not the perfect-tree test. A full tree may skip a whole level on one side as long as no node has a single child. A perfect tree also requires every level to be filled.
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
def is_full(node):
    if node is None:
        return True
    if node.left is None and node.right is None:
        return True
    if node.left is None or node.right is None:
        return False
    return is_full(node.left) and is_full(node.right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/check-whether-binary-tree-full-binary-tree-not/
- https://www.geeksforgeeks.org/dsa/perfect-binary-tree/
</details>

<details>
<summary>What are the characteristics of a perfect binary tree and how do you identify one?</summary>

### What are the characteristics of a perfect binary tree and how do you identify one?

**Definition:**
A perfect binary tree is filled on every level. Every internal node has two children, and every leaf is on the same level. The article states the leaf count as 2^h, the internal-node count as 2^h − 1, the total as 2^(h+1) − 1, and the height as log(n + 1) − 1.

**How it works:**
- The two-pass check finds the depth, then verifies that every leaf is at that depth and that no node has one child. It is O(n) time and O(h) space.
- The single DFS is also O(n) time and O(h) space. It records the level of the first leaf and requires every other leaf to match, and every non-leaf to have two children.
- An empty tree is treated as perfect by those functions. A root with two leaf children is perfect. A root whose one side is deeper is not.
- The array form stores the left child of index i at 2i+1 and the right child at 2i+2.
- **Time:** O(n). **Space:** O(h).

**Examples:**
```text
def is_perfect(node, level, leaf_level):
    if node is None:
        return True
    if node.left is None and node.right is None:
        return level == leaf_level[0]
    if node.left is None or node.right is None:
        return False
    return is_perfect(node.left, level + 1, leaf_level) and is_perfect(node.right, level + 1, leaf_level)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/perfect-binary-tree/
</details>

<details>
<summary>How can you serialize and deserialize a binary tree?</summary>

### How can you serialize and deserialize a binary tree?

**Definition:**
Serialization writes nodes in a fixed walk and writes −1 for a null child. Deserialization reads that sequence and rebuilds the same links. Preorder does it with recursion. Level order does it with a queue. Both are O(n) time and O(n) space.

**How it works:**
- Recursive preorder appends the value, then the left subtree, then the right subtree. On the way back, −1 means a missing child. The article states O(n) time and O(n) space.
- Level order pushes the root, writes a value or −1, and enqueues children, including nulls. Rebuild creates the root from the first value and attaches left and right as the later values arrive. Same O(n) time and O(n) space.
- The marker has to be a value the nodes do not use, or a separate bit. The article uses −1.
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
def serialize(node, out):
    if node is None:
        out.append(-1)
        return
    out.append(node.data)
    serialize(node.left, out)
    serialize(node.right, out)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/serialize-deserialize-binary-tree/
</details>

<details>
<summary>How to check if a binary tree is height-balanced?</summary>

### How to check if a binary tree is height-balanced?

**Definition:**
A tree is height-balanced when, at every node, the heights of the left and right subtrees differ by at most one, and both subtrees are balanced. The single-walk check returns the height and the balance flag together.

**How it works:**
- Computing the height from scratch at every node is O(n²) time and O(h) space. Each height walk repeats work the parent already did.
- The expected single traversal is O(n) time and O(h) space. A null child has height 0. A node is unbalanced when the absolute difference of the child heights is greater than 1, or when either child already returned unbalanced. Otherwise the node's height is 1 plus the larger child height.
- AVL trees maintain this property on every update and keep height O(log n). This check only reports whether a given tree already has it.
- **Time:** O(n) for the single walk, O(n²) if each height is recomputed. **Space:** O(h).

**Examples:**
```text
def check(node):
    if node is None:
        return 0, True
    lh, lok = check(node.left)
    rh, rok = check(node.right)
    ok = lok and rok and abs(lh - rh) <= 1
    return 1 + max(lh, rh), ok
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/how-to-determine-if-a-binary-tree-is-balanced/
- https://www.geeksforgeeks.org/dsa/avl-tree-set-1-insertion/
</details>

<details>
<summary>Describe the application of Binary Trees in networking (e.g., routing, addressing).</summary>

### Describe the application of Binary Trees in networking (e.g., routing, addressing).

**Definition:**
A routing lookup walks a binary trie of address bits. Left is 0, right is 1, and the longest prefix that is marked as a route is the match. The trie article states a walk of a key of length n as O(n) time.

**How it works:**
- Insert of a prefix of length n is O(n) time and O(n) auxiliary space for the new nodes. Search is O(n) time and O(1) auxiliary space. For an IPv4 address, n is at most 32. For IPv6 it is at most 128. The bound follows the bit length, not the number of routes, which is why the trie article says this is faster than a BST of whole keys.
- Prefix search stops at the last bit of the query that still has a node. Autocomplete on the same structure lists every word under a node; longest-prefix routing keeps the last marked route along the walk.
- A BST of full addresses searches in O(h). It does not share prefixes, and a skewed tree is O(n) in the number of routes.
- **Time:** O(n) in the bit length for a trie lookup. **Space:** O(n) auxiliary to insert one new prefix.

**Examples:**
```text
# insert prefixes 0, 01, 011
# lookup 0110 walks 0 -> 1 -> 1 and keeps the route marked at 011
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/trie-insert-and-search/
- https://www.geeksforgeeks.org/dsa/auto-complete-feature-using-trie/
</details>

<details>
<summary>Explain the concept of Path Sum in a binary tree and how it is calculated.</summary>

### Explain the concept of Path Sum in a binary tree and how it is calculated.

**Definition:**
A root-to-leaf path sum adds the values from the root down to a leaf. Subtract the current value and recurse on each child. A leaf matches when the remaining target equals the leaf value. The single-walk shape is the balance check: O(n) time and O(h) space.

**How it works:**
- The height-balance article's expected single traversal is O(n) time and O(h) space. Path sum visits each node once on the same recursion, carrying the remaining sum instead of a height.
- At a null node, return false. At a leaf, return whether the remaining target equals the leaf. Otherwise ask the left child with target − value, then the right child.
- Any path, not only root-to-leaf, is a different problem. This definition is the root-to-leaf target.
- A perfect tree has height log(n + 1) − 1, so the recursion depth on that shape is that height. A skewed tree makes the O(h) stack into O(n) frames.
- **Time:** O(n). **Space:** O(h).

**Examples:**
```text
def has_path(node, target):
    if node is None:
        return False
    if node.left is None and node.right is None:
        return node.data == target
    rest = target - node.data
    return has_path(node.left, rest) or has_path(node.right, rest)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/how-to-determine-if-a-binary-tree-is-balanced/
- https://www.geeksforgeeks.org/dsa/perfect-binary-tree/
</details>

<details>
<summary>Find the lowest common ancestor (LCA) of two nodes in a binary tree.</summary>

### Find the lowest common ancestor (LCA) of two nodes in a binary tree.

**Definition:**
The LCA is the deepest node that has both targets as descendants, counting a node as a descendant of itself. A general binary tree has no BST order, so the search looks at both sides. The expected single traversal returns the node as soon as the two sides split.

**How it works:**
- Checking every node as a candidate is O(n²) time and O(h) space.
- Storing both root-to-node paths and comparing them is O(n) time and O(n) space.
- The expected single traversal is O(n) time and O(h) space. A null node returns null. A node that is either target returns itself. Otherwise the left and right results are combined: both non-null means this node is the LCA; one non-null means the ancestor is that side.
- The BST shortcut, which walks one path in O(h), does not apply unless the tree is a BST.
- **Time:** O(n) expected. **Space:** O(h).

**Examples:**
```text
def lca(node, a, b):
    if node is None or node is a or node is b:
        return node
    left = lca(node.left, a, b)
    right = lca(node.right, a, b)
    if left and right:
        return node
    return left or right
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/lowest-common-ancestor-binary-tree-set-1/
</details>

<details>
<summary>Can a binary tree be reconstructed from its inorder and preorder (or postorder) traversals?</summary>

### Can a binary tree be reconstructed from its inorder and preorder (or postorder) traversals?

**Definition:**
Yes, when the keys are unique. Preorder's first value is the root. That key's index in the inorder array splits the left subtree from the right. The same rule rebuilds each side. Postorder uses the last value as the root instead of the first.

**How it works:**
- The naive search for the root inside the inorder array is O(n²) time and O(h) space. Each level may scan the remaining inorder slice.
- The expected method stores inorder indexes in a hash map, so each root is found in O(1). The article states O(n) time and O(n) space.
- The preorder cursor advances by one for the root, then builds the left subtree from the next values, then the right. The inorder bounds shrink to the left slice and the right slice.
- Duplicate keys make the inorder index ambiguous. The article's split assumes one index per root value.
- **Time:** O(n) with the map, O(n²) if each root is scanned. **Space:** O(n) for the map, O(h) recursion for the naive form.

**Examples:**
```text
# preorder 1,2,3 and inorder 2,1,3
# root 1, left inorder [2], right inorder [3]
def build(pre, inorder, idx):
    ...
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/construct-tree-from-given-inorder-and-preorder-traversal/
</details>

<details>
<summary>Implement a function to check if a binary tree is symmetric (a mirror image of itself).</summary>

### Implement a function to check if a binary tree is symmetric (a mirror image of itself).

**Definition:**
A tree is symmetric when its left subtree is the mirror of its right. Two nodes mirror each other when their values are equal, the left child of one mirrors the right child of the other, and the right child of one mirrors the left child of the other. The perfect-tree article's single DFS, a recursive check over every node, is O(n) time and O(h) space.

**How it works:**
- An empty tree is symmetric. A single node is symmetric. Compare the root's left and right with the swapped-child rule.
- The walk examines each node once, the same full recursive shape the perfect-tree checker uses: O(n) time and O(h) space.
- Equality of inorder alone is not enough. The mirror pairing is left-with-right, not left-with-left.
- **Time:** O(n). **Space:** O(h).

**Examples:**
```text
def mirror(a, b):
    if a is None or b is None:
        return a is b
    return a.data == b.data and mirror(a.left, b.right) and mirror(a.right, b.left)

def symmetric(root):
    return root is None or mirror(root.left, root.right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/perfect-binary-tree/
</details>

<details>
<summary>Describe how binary trees can be used in decision-making processes (e.g., decision trees in AI).</summary>

### Describe how binary trees can be used in decision-making processes (e.g., decision trees in AI).

**Definition:**
A decision tree is a supervised model for classification or regression. Internal nodes are attribute tests, branches are the answers, and leaves are the prediction. The article builds it by recursive splits: pick the attribute with the highest information gain, then build each child on the subset that followed that branch.

**How it works:**
- Information gain is entropy of the parent minus the weighted entropy of the children. A pure child has entropy 0. The Gini index is 1 − Σ pᵢ². The article says a lower Gini is preferred, and that sklearn's default criterion is gini.
- Building stops when the subset is pure, when no attributes remain, or when no instances remain. The leaf is then the majority label, or the parent's majority if the subset is empty.
- A prediction starts at the root and follows one branch per test until a leaf. On a perfect binary tree the height is log(n + 1) − 1, so that walk is that many tests.
- The article does not state a training-time formula. The prediction walk is one path, not a scan of every node.
- **Space:** one node per test plus the leaves. **Time:** one path from root to leaf for a prediction.

**Examples:**
```text
# income > 50000?
#   no  -> leaf "No Purchase"
#   yes -> age > 30?
#            no  -> "No Purchase"
#            yes -> "Purchase" 
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/decision-tree-introduction-example/
- https://www.geeksforgeeks.org/dsa/perfect-binary-tree/
</details>

<details>
<summary>Discuss the role of binary trees in garbage collection algorithms like the mark-and-sweep algorithm.</summary>

### Discuss the role of binary trees in garbage collection algorithms like the mark-and-sweep algorithm.

**Definition:**
Mark-and-sweep frees heap objects the program can no longer reach. The mark phase is a depth-first search from the roots through object references, and it sets a mark bit on every reachable object. The sweep phase scans the heap and releases every object whose mark bit is still false. The object graph is not required to be a binary tree; a binary tree is the special case where each object has two child references.

**How it works:**
- The article's mark starts with the bit false. Mark(root) sets it true and calls Mark on every referenced object. Cycles are safe: a marked object is not walked again, so the search does not loop.
- Iterative DFS on a graph is O(V + E) time and O(V) space. Here V is the reachable objects and E is the references followed from them.
- Sweep walks every heap object. A true bit is cleared for the next collection. A false bit releases the object. The article states two costs and no separate Big-O: the program stops while the collector runs, and free space fragments, so a later allocation can fail even when the free bytes would suffice if they were contiguous.
- A binary tree collector marks the node, then the left child, then the right child. That is the same preorder DFS with two references.
- **Time:** mark O(V + E) by the DFS bound. **Space:** O(V) for the DFS stack and visited state.

**Examples:**
```text
def mark(obj):
    if obj is None or obj.marked:
        return
    obj.marked = True
    mark(obj.left)
    mark(obj.right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/mark-and-sweep-garbage-collection-algorithm/
- https://www.geeksforgeeks.org/dsa/iterative-depth-first-traversal/
</details>

<details>
<summary>Explain how trie data structures are used in implementing autocomplete features.</summary>

### Explain how trie data structures are used in implementing autocomplete features.

**Definition:**
Autocomplete inserts every word into a trie, one character per step. A query walks the characters typed so far. Every word stored under that node is a completion. The autocomplete article states the time as O(N · L), where N is the number of words and L is the length of the longest word.

**How it works:**
- Trie insert of one word of length n is O(n) time and O(n) auxiliary space. Search and prefix search are O(n) time and O(1) auxiliary space. The autocomplete bound O(N · L) covers building the trie and collecting completions across the dictionary.
- The walk of the typed prefix fails immediately when a character has no edge. There is nothing to suggest.
- From the node that remains, a DFS or BFS lists the end-of-word marks below it. Sharing a prefix means "hell" is stored once for "hello", "help", and "helping".
- A BST of whole strings compares entire keys and does not share those prefixes. The trie article says the character walk is faster than a BST.
- **Time:** O(N · L) for the autocomplete article's dictionary pass. Prefix search of one string is O(n).

**Examples:**
```text
# insert hello, help, helping
# query "hel" lands on the node after l
# completions: hello, help, helping
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/auto-complete-feature-using-trie/
- https://www.geeksforgeeks.org/dsa/trie-insert-and-search/
</details>

<details>
<summary>What is Huffman Coding, and how is a binary tree used in this compression technique?</summary>

### What is Huffman Coding, and how is a binary tree used in this compression technique?

**Definition:**
Huffman coding builds a binary tree of character frequencies. Each step joins the two least frequent nodes under a new parent whose frequency is their sum. Left edges are bit 0 and right edges are bit 1. The path from the root to a leaf is that character's code. The article states time O(n log n), where n is the number of distinct characters, and space O(n).

**How it works:**
- Leaves are the characters. Internal nodes exist only to add frequencies. A min-heap extracts the two smallest nodes in the O(log n) heap operations, n times, which is the O(n log n) bound.
- No character's code is a prefix of another, because a character is only a leaf. Decoding reads bits from the root until it hits a leaf, emits that character, and returns to the root.
- The tree is not a BST. Child order is the bit, not a key order. Frequent characters end up closer to the root because they are joined later.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
# frequencies a:5, b:2, c:1
# join b and c into 3; join that with a
# a -> 0, b -> 10, c -> 11
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/huffman-coding-greedy-algo-3/
- https://www.geeksforgeeks.org/dsa/introduction-to-heap-data-structure/
</details>
