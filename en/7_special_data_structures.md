# Data Structures — hub

Arrays, trees, graphs, heaps, and how to pick the right structure.

[← Back to hub](./7_special.md)

### Related docs

- [Algorithms](./7_special_algorithms.md) · [Computer science](./7_special_cs.md) · [JavaScript — Document](./3_js_browser.md)

### Sections

- [Choosing a data structure](#choosing-a-data-structure)
- [Linear](./7_special_data_structures_linear.md)
- [Trees & graphs](./7_special_data_structures_trees.md)
- [Hash & heap](./7_special_data_structures_advanced.md)

### Interview

| Topic | Covers |
| --- | --- |
| [Arrays — Interview](./7_ds_arrays_interview.md) | Arrays interview questions, answers not filled in. |
| [Hash Tables — Interview](./7_ds_hash_tables_interview.md) | Hash Tables interview questions, answers not filled in. |
| [Stacks — Interview](./7_ds_stacks_interview.md) | Stacks interview questions, answers not filled in. |
| [Heaps and Maps — Interview](./7_ds_heaps_maps_interview.md) | Heaps and Maps interview questions, answers not filled in. |
| [Binary Tree — Interview](./7_ds_binary_tree_interview.md) | Binary Tree interview questions, answers not filled in. |
| [Linked Lists — Interview](./7_ds_linked_lists_interview.md) | Linked Lists interview questions, answers not filled in. |
| [Strings — Interview](./7_ds_strings_interview.md) | Strings interview questions, answers not filled in. |
| [Tree Data Structure — Interview](./7_ds_trees_interview.md) | Tree Data Structure interview questions, answers not filled in. |
| [Queues — Interview](./7_ds_queues_interview.md) | Queues interview questions, answers not filled in. |
| [Trie — Interview](./7_ds_tries_interview.md) | Trie interview questions, answers not filled in. |
| [Data Structures — Interview](./7_ds_overview_interview.md) | Data Structures interview questions, answers not filled in. |

---

## Overview

<details>
  <summary>Choosing a data structure</summary>

### Choosing a data structure

**Definition:**

Lookup table matching common access patterns to the right abstract data structure and expected complexity.

| Need                 | Structure        | Why                     |
| -------------------- | ---------------- | ----------------------- |
| Ordered index access | Array            | O(1) index              |
| Fast lookup by key   | Map / Hash table | O(1) average            |
| Unique values        | Set              | O(1) has                |
| LIFO undo/history    | Stack            | O(1) push/pop           |
| FIFO job queue       | Queue            | O(1) enqueue/dequeue\*  |
| Hierarchy            | Tree             | Parent-child            |
| Relationships        | Graph            | Arbitrary edges         |
| Min/max repeatedly   | Heap             | O(log n) insert/extract |
| Prefix search        | Trie             | O(m) per char           |

\*Use proper queue — `array.shift()` is O(n) in JS.

---

</details>
