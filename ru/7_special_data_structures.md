# Data Structures — hub

Массивы, деревья, графы, кучи и выбор подходящей структуры.

[← Back to hub](./7_special.md)

### Related docs

- [Algorithms](./7_special_algorithms.md) · [Computer science](./7_special_cs.md) · [JavaScript — Document](./3_js_browser.md)

### Sections

- [Choosing a data structure](#choosing-a-data-structure)
- [Linear](./7_special_data_structures_linear.md)
- [Trees & graphs](./7_special_data_structures_trees.md)
- [Hash & heap](./7_special_data_structures_advanced.md)

### Interview

| Тема | О чём |
| --- | --- |
| [Arrays — собеседование](./7_ds_arrays_interview.md) | Вопросы по Arrays, ответы не заполнены. |
| [Hash Tables — собеседование](./7_ds_hash_tables_interview.md) | Вопросы по Hash Tables, ответы не заполнены. |
| [Stacks — собеседование](./7_ds_stacks_interview.md) | Вопросы по Stacks, ответы не заполнены. |
| [Heaps and Maps — собеседование](./7_ds_heaps_maps_interview.md) | Вопросы по Heaps and Maps, ответы не заполнены. |
| [Binary Tree — собеседование](./7_ds_binary_tree_interview.md) | Вопросы по Binary Tree, ответы не заполнены. |
| [Linked Lists — собеседование](./7_ds_linked_lists_interview.md) | Вопросы по Linked Lists, ответы не заполнены. |
| [Strings — собеседование](./7_ds_strings_interview.md) | Вопросы по Strings, ответы не заполнены. |
| [Tree Data Structure — собеседование](./7_ds_trees_interview.md) | Вопросы по Tree Data Structure, ответы не заполнены. |
| [Queues — собеседование](./7_ds_queues_interview.md) | Вопросы по Queues, ответы не заполнены. |
| [Trie — собеседование](./7_ds_tries_interview.md) | Вопросы по Trie, ответы не заполнены. |
| [Data Structures — собеседование](./7_ds_overview_interview.md) | Вопросы по Data Structures, ответы не заполнены. |

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
