# Heaps and Maps — Interview

Answers cover the published question list.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Heaps and Maps — Interview](./7_ds_heaps_maps_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Heaps And Maps](https://devinterview.io/questions/data-structures-and-algorithms/heap-and-map-data-structures-interview-questions/).

### Basic Heap Concepts

<details>
<summary>What is a Heap?</summary>

### What is a Heap?

**Definition:**

A binary heap is a complete binary tree stored so that every parent is ordered against its children. The extreme value sits at the root.

**How it works:**

- In a min-heap each parent is less than or equal to its children. In a max-heap each parent is greater than or equal to its children.
- The tree is complete, so it packs into an array: the children of index `i` are `2i + 1` and `2i + 2` when indexes start at 0.
- Insert and delete-extreme are O(log n). Reading the extreme is O(1). Finding an arbitrary value is O(n).
- The heap order is not a full sort. The array is not sorted from left to right.

**Examples:**

Min-heap array `[1, 3, 2]`: 1 is the root, 3 is the left child, 2 is the right child.
</details>

<details>
<summary>What is a Priority Queue?</summary>

### What is a Priority Queue?

**Definition:**

A priority queue retrieves the item with the best key next, not the item that arrived first.

**How it works:**

- The usual operations are insert and extract-min or extract-max. A binary heap implements both in O(log n).
- A sorted array makes extract O(1) and insert O(n). An unsorted array makes insert O(1) and extract O(n).
- Dijkstra, A*, Huffman coding, and schedulers use it to always expand the current best.
- Stable ordering of equal keys is not part of the heap contract. If arrival order matters among equals, store a counter in the key.
</details>

<details>
<summary>How does the Binary Heap property differ between a max heap and a min heap?</summary>

### How does the Binary Heap property differ between a max heap and a min heap?

**Definition:**

The shape is the same complete tree. Only the comparison at each parent flips.

**How it works:**

- Max-heap: `parent ≥ left` and `parent ≥ right`. The largest value is at index 0.
- Min-heap: `parent ≤ left` and `parent ≤ right`. The smallest value is at index 0.
- Children of one parent have no order between them. `[1, 4, 2]` and `[1, 2, 4]` can both be min-heaps.
- A max-heap is a min-heap of negated keys, which is how one implementation serves both.
</details>

<details>
<summary>Can you explain heap property maintenance after an Insert operation and a Delete operation?</summary>

### Can you explain heap property maintenance after an Insert operation and a Delete operation?

**Definition:**

Insert sifts the new item up. Delete-extreme puts the last item at the root and sifts it down.

**How it works:**

- Insert writes the item at the next free index, which is the next leaf of the complete tree. While it beats its parent, swap them. That is O(log n) swaps.
- Delete-min (or delete-max) reads the root, moves the last leaf into the root, shrinks the size, and sifts down: swap with the better child until the order holds.
- Sift-up and sift-down only follow one path. They do not rebuild the whole tree.
- After either operation the tree is still complete and every parent still obeys the heap order.

**Examples:**

Min-heap `[1, 3, 2]`, insert 0. The array becomes `[1, 3, 2, 0]`, then 0 swaps with 3 and then with 1, ending as `[0, 1, 2, 3]`.
</details>

<details>
<summary>Insert an item into the Heap. Explain your actions.</summary>

### Insert an item into the Heap. Explain your actions.

**Definition:**

Append the item as the last leaf, then swap it upward while it violates the order with its parent.

**How it works:**

- The parent of index `i` is `(i - 1) // 2` for zero-based arrays.
- Stop when the item is the root or it no longer beats its parent.
- Each swap restores the order on that edge and may break the edge above, which the next swap fixes.
- The item does not compare with its siblings. Only the ancestor chain matters.

**Examples:**

Max-heap `[9, 5, 6]`, insert 8. Place 8 at index 3. Parent index 1 holds 5, and 8 > 5, so swap to `[9, 8, 6, 5]`. Parent of index 1 is 9, and 8 ≤ 9, so stop.
</details>

### Heaps Implementation and Optimization

<details>
<summary>Compare Heaps-Based vs. Arrays-Based priority queue implementations.</summary>

### Compare Heaps-Based vs. Arrays-Based priority queue implementations.

**Definition:**

A heap keeps insert and extract logarithmic. An array makes one of those two operations linear.

**How it works:**

- Unsorted array: insert at the end is O(1), extract scans for the extreme and shifts, O(n).
- Sorted array: extract at the end is O(1), insert binary-searches and then shifts, O(n).
- Binary heap: both are O(log n), and the extreme is always at index 0.
- Use the unsorted array only for a tiny queue. Use the heap when both operations are frequent, which is the usual graph algorithm.
</details>

<details>
<summary>How can you implement a heap efficiently using Dynamic Arrays?</summary>

### How can you implement a heap efficiently using Dynamic Arrays?

**Definition:**

The heap's complete shape is exactly a prefix of a dynamic array. Grow the array when the next leaf does not fit.

**How it works:**

- Index arithmetic replaces pointers. No per-node allocation, and siblings sit near each other in memory.
- Append is the insert's first step. Doubling keeps a series of inserts amortized O(1) before the O(log n) sift.
- Delete-extreme swaps the root with the last index and then `pop`s the array, which is O(1) at the end, then sifts down.
- Shrinking the capacity after many deletes is optional. Leaving spare capacity avoids a grow-shrink loop.
</details>

<details>
<summary>Name some ways to implement Priority Queue.</summary>

### Name some ways to implement Priority Queue.

**Definition:**

Any structure that can insert an item and later remove the best one is a priority queue. The heap is the default.

**How it works:**

- Binary heap on an array: O(log n) insert and extract.
- Unsorted or sorted array, or a linked list: one of the two operations is O(n).
- A balanced BST orders every key, so extract-min is O(log n) and you also get search. The constants and the pointers are heavier than a heap.
- A Fibonacci heap has a cheaper theoretical decrease-key. In practice a binary heap is what libraries ship.
</details>

<details>
<summary>How does the Lazy deletion technique work in a heap and what is its purpose?</summary>

### How does the Lazy deletion technique work in a heap and what is its purpose?

**Definition:**

Lazy deletion marks an item dead and leaves it in place until it reaches the root, instead of searching the array to remove it now.

**How it works:**

- A direct delete of an arbitrary index is possible in O(log n) if you already know the index. Finding that index is O(n) unless you keep a side map.
- With a deleted-set, extract skips a root that is marked and sifts the replacement, repeating until the root is live.
- Insert of a 'new version' of a key can leave the old version marked. Decrease-key in Dijkstra is often done this way: push the better pair and ignore the stale one when it comes out.
- The heap can hold dead entries, so the size used for complexity is the number of pushes, not the number of live items, until a rebuild drops the dead ones.
</details>

<details>
<summary>Explain Heapify and where it is used in heap operations.</summary>

### Explain Heapify and where it is used in heap operations.

**Definition:**

Heapify restores the heap order at one subtree, assuming the children are already heaps. Building a heap calls it from the bottom up.

**How it works:**

- Sift-down at index `i` compares the node with its children and swaps with the extreme child, then repeats.
- Floyd's build starts at the last parent, index `n // 2 - 1`, and sifts down through the root. The total is O(n), not O(n log n), because most nodes are near the leaves.
- Insert uses sift-up, which is a different direction. Delete-extreme uses sift-down at the root, which is heapify on the whole tree.
- Heapsort's loop is extract plus heapify on the shrinking prefix.
</details>

### Heaps Sorting and Algorithms

<details>
<summary>What is Heap Sort?</summary>

### What is Heap Sort?

**Definition:**

Heapsort builds a max-heap and then repeatedly swaps the root with the last unsorted index and sifts the new root down.

**How it works:**

- After the build, the largest item is at index 0. Swap it to the end, shrink the heap by one, and sift down. The end of the array fills with sorted items.
- Time is O(n log n) in every case. Extra memory is O(1) beyond the input array.
- It is not stable. Equal keys can reorder.
- The inner loop jumps around the array, so it is often slower than a well-tuned quicksort even though the worst case is better.

**Examples:**

`[3, 1, 2]` becomes a max-heap `[3, 1, 2]`, swap 3 to the end, sift `[2, 1]`, swap 2, and the array is `[1, 2, 3]`.
</details>

<details>
<summary>How does Heap Sort compare to Quick Sort in terms of speed and memory usage?</summary>

### How does Heap Sort compare to Quick Sort in terms of speed and memory usage?

**Definition:**

Heapsort guarantees O(n log n) time and O(1) extra memory. Quicksort is usually faster and uses O(log n) stack, with an O(n²) worst case if pivots are bad.

**How it works:**

- Quicksort's partition scans a range sequentially, which matches the cache. Heapsort's sift jumps from parent to child.
- Introsort starts as quicksort and switches to heapsort when the recursion gets too deep, which is how several standard libraries get both speed and a worst-case bound.
- Mergesort is stable and also O(n log n), and it needs O(n) extra buffer. Heapsort does not.
- Pick heapsort when you need a hard time bound and no extra buffer and stability does not matter.
</details>

<details>
<summary>Describe the algorithm for Merging k Sorted Arrays using a heap.</summary>

### Describe the algorithm for Merging k Sorted Arrays using a heap.

**Definition:**

A min-heap of size k holds the current head of each array. Each extract-min is the next output item, and you push the successor from that same array.

**How it works:**

- Initialize the heap with `(value, array_id, index)` for index 0 of every non-empty array.
- Pop the smallest, append it to the output, and if that array has a next element, push it. Repeat until the heap is empty.
- There are N elements in total and each costs O(log k), so the merge is O(N log k). The heap stores k tuples, not all N items.
- A pairwise mergesort of the arrays is another O(N log k). The heap version streams the output and matches the k-way merge inside external sort.

**Examples:**

Arrays `[1, 4]`, `[2, 3]`, `[0]`. The heap starts with 1, 2, and 0. Pop 0, then 1, push 4, pop 2, pop 3, pop 4. Output `[0, 1, 2, 3, 4]`.
</details>

<details>
<summary>Implement a heap that efficiently supports Find-Max and Find-Min operations.</summary>

### Implement a heap that efficiently supports Find-Max and Find-Min operations.

**Definition:**

One binary heap exposes only one extreme in O(1). To read both, keep a min-heap and a max-heap of the same items, or use a min-max heap that alternates the order by level.

**How it works:**

- Twin heaps: every item lives in both, linked by indexes or ids. Insert updates both in O(log n). Delete-min removes from the min-heap and from the max-heap by the stored index.
- A min-max heap stores mins on even levels and maxes on odd levels, in one array. Find-min and find-max are O(1). Insert and delete are still O(log n), with a longer sift.
- A balanced BST also does both in O(1) for the ends and O(log n) for updates, and it uses more memory per node.
- If you only need both ends and not a general priority delete, a double-ended heap is enough. Do not fully sort the array.
</details>

### Heap Applications

<details>
<summary>What are some Practical Applications of Heaps?</summary>

### What are some Practical Applications of Heaps?

**Definition:**

Heaps show up wherever the next step is 'take the current best' and the set of candidates keeps changing.

**How it works:**

- Dijkstra and Prim grow a frontier by the smallest distance or weight.
- Heapsort and the selection of the k smallest items: a bounded heap of size k is O(n log k).
- Huffman coding repeatedly merges the two lightest trees.
- Operating systems schedule the next timer or the next ready task by priority. Streaming medians use two heaps.
</details>

<details>
<summary>How is a heap used in the Huffman Coding algorithm?</summary>

### How is a heap used in the Huffman Coding algorithm?

**Definition:**

Huffman coding builds an optimal prefix code by repeatedly joining the two lowest-frequency symbols. A min-heap is the priority queue that finds those two symbols.

**How it works:**

- Each symbol starts as a leaf whose key is its frequency. The heap's minimum is the next symbol or subtree to merge.
- Pop twice, make a parent whose frequency is the sum, and push the parent back. Repeat until one tree remains. Left and right edges become the bits of the code.
- A binary heap does each pop and push in O(log n), so the whole build is O(n log n). heapq is a min-heap: the smallest key sits at index 0.
- More frequent symbols end up closer to the root and get shorter codes. The Huffman article shows this on the frequencies of a sample sentence.

**Sources:**

https://en.wikipedia.org/wiki/Huffman_coding
https://docs.python.org/3/library/heapq.html
</details>

<details>
<summary>Explain how heaps are used in Real-Time Scheduling systems.</summary>

### Explain how heaps are used in Real-Time Scheduling systems.

**Definition:**

A scheduler keeps runnable tasks in a priority queue and always runs the one with the best key, often the earliest deadline or the smallest scheduled time. A binary heap finds that task at the root.

**How it works:**

- heapq's theory section describes this use: the heap holds incoming events, and the win condition is the smallest scheduled time. New events are scheduled into the future, so they insert without being better than the last event already popped.
- Peek is O(1). Insert and delete-min are O(log n) worst case on a binary heap, so the cost does not jump with a long sorted-array shift.
- The same page says heaps suit schedulers because the speed is almost flat and the worst case is not much worse than the average. That bound is what a real-time loop can budget.
- Equal priorities need a tie-break. heapq's priority-queue notes store (priority, entry count, task) so equal keys stay in arrival order and tasks themselves are never compared.

**When to use:**

Use a heap when the next task is always an extreme key. Use a sorted list of arrival order when the policy is plain FIFO and priorities do not change.

**Sources:**

https://docs.python.org/3/library/heapq.html
https://en.wikipedia.org/wiki/Binary_heap
</details>

<details>
<summary>Discuss how heaps can manage efficiently Data Stream Median Finding.</summary>

### Discuss how heaps can manage efficiently Data Stream Median Finding.

**Definition:**

A running median keeps a max-heap of the lower half and a min-heap of the upper half. The median is the root of the larger heap, or the average of the two roots when the sizes match.

**How it works:**

- heapq documents this split. Its sample pushes into the upper min-heap, then moves the smallest of that half onto a max-heap of the lower half, and rebalances so the two sizes differ by at most one.
- Each arrival is O(log n). A full sort on every arrival is O(n log n) per step.
- Python's heapq is a min-heap. The lower half is a max-heap: since 3.14, heapify_max and heappush_max exist. Before that, store negated keys.
- The heaps hold values seen so far, not a sorted array, so the median moves without rewriting the whole history.

**Examples:**

```text
# lower half: max-heap, upper half: min-heap
# sizes differ by at most 1; median is a root or the average of the roots
```

**Sources:**

https://docs.python.org/3/library/heapq.html
</details>

### Heaps Comparison and Choice

<details>
<summary>What are the advantages of Heaps over Sorted Arrays?</summary>

### What are the advantages of Heaps over Sorted Arrays?

**Definition:**

A binary heap finds the extreme element in O(1) and inserts or removes it in O(log n). A sorted array also finds the extreme in O(1), but inserting in order shifts O(n) elements.

**How it works:**

- The binary-heap article requires a complete tree, so the array layout has no holes, and the heap property only orders a parent against its children. The array is not fully sorted.
- heapify builds that order in linear time. Sorting the same array is O(n log n).
- Merge of two binary heaps is O(n) in that table. A sorted array merges in O(n) too, but only after both sides are fully sorted.
- Read the k largest with heapq.nlargest. The docs say it is aimed at small k; for large k, sorted is the better tool.

**When to use:**

Use a heap when the next step only needs the current extreme and the set keeps changing. Use a sorted array when you need the full order, binary search, or a stable scan.

**Sources:**

https://en.wikipedia.org/wiki/Binary_heap
https://docs.python.org/3/library/heapq.html
</details>

<details>
<summary>What is the difference between Heap and Red-Black Tree?</summary>

### What is the difference between Heap and Red-Black Tree?

**Definition:**

A binary heap is a complete tree that only guarantees the parent is ordered against its children, so the minimum or maximum sits at the root. A red-black tree is a balanced search tree: every node splits smaller keys from larger keys, and Java's TreeMap guarantees log(n) for containsKey, get, put, and remove.

**How it works:**

- Heap search for an arbitrary key is not O(1) or O(log n) by the heap property. You scan, or you keep a side map of indexes. A red-black tree finds a key by comparisons in O(log n) worst case.
- Heap insert and delete-min are O(log n). TreeMap's put and remove are also O(log n), and they keep the whole order, not just the extreme.
- A heap is the right priority queue. A red-black map is the right ordered dictionary: successor, predecessor, and ranges come from the search-tree order.
- heapq stores a heap in a list with no parent pointers. TreeMap stores red-black links and a comparator.

**When to use:**

Pick a heap for next-minimum or next-maximum. Pick a red-black tree when you look up arbitrary keys and walk them in order.

**Sources:**

https://en.wikipedia.org/wiki/Binary_heap
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html
</details>

<details>
<summary>When would you choose a Fibonacci Heap over a Binary Heap?</summary>

### When would you choose a Fibonacci Heap over a Binary Heap?

**Definition:**

A Fibonacci heap makes insert, find-min, decrease-key, and merge amortized constant time. Delete-min stays O(log n) amortized. A binary heap charges O(log n) for insert and decrease-key as well.

**How it works:**

- Fredman and Tarjan's bound: a sequence of a inserts and decrease-keys plus b delete-mins is O(a + b log n) on a Fibonacci heap, and O((a + b) log n) on a binary or binomial heap.
- That gap matters when decrease-key dominates, as in Dijkstra or Prim with a heap. The Fibonacci article states those algorithms improve because of it.
- Merge is amortized O(1), against O(n) for a binary heap in the binary-heap table, and against logarithmic merge for a binomial heap.
- The worst-case delete-min on a Fibonacci heap is O(n). The structure is also heavier to implement. A binary heap's O(log n) worst case and tight array are usually faster in practice.

**When to use:**

Choose a Fibonacci heap for an asymptotic bound on a decrease-key-heavy graph algorithm. Choose a binary heap for a priority queue you actually ship.

**Sources:**

https://en.wikipedia.org/wiki/Fibonacci_heap
https://en.wikipedia.org/wiki/Binary_heap
</details>

<details>
<summary>What situations might call for using a Binomial Heap?</summary>

### What situations might call for using a Binomial Heap?

**Definition:**

A binomial heap is a collection of heap-ordered binomial trees. The Fibonacci-heap article compares it directly: a sequence of inserts, decrease-keys, and delete-mins is O((a + b) log n), and merging two binomial heaps is logarithmic, better than a binary heap's linear merge.

**How it works:**

- Use it when the workload merges priority queues often and you want a simpler structure than a Fibonacci heap.
- Insert and delete-min stay logarithmic, same family of bounds as a binary heap for a mixed sequence. You do not get the amortized O(1) decrease-key.
- A binary heap is a single complete array. A binomial heap is a forest, which is why two of them merge by combining trees instead of copying every element into one array.
- If decrease-key is the hot operation and the asymptotic bound is the point, the Fibonacci article says that structure wins. If the code must stay small, a binary heap still wins.

**When to use:**

Pick a binomial heap for logarithmic merges of priority queues. Pick a binary heap when there is one queue and the array layout matters. Pick a Fibonacci heap when decrease-key must be amortized constant.

**Sources:**

https://en.wikipedia.org/wiki/Fibonacci_heap
https://en.wikipedia.org/wiki/Binary_heap
</details>

<details>
<summary>Compare and contrast the use cases of Min-Heaps vs. Max-Heaps.</summary>

### Compare and contrast the use cases of Min-Heaps vs. Max-Heaps.

**Definition:**

A min-heap keeps the smallest key at the root. A max-heap keeps the largest. The binary-heap article defines both by the parent-child inequality. The shape, a complete tree, is the same.

**How it works:**

- heapq's default is a min-heap: heap[0] is the smallest, and the invariant is heap[k] <= heap[2*k+1] and heap[k] <= heap[2*k+2]. Since 3.14, heappop_max and heappush_max maintain the opposite inequality.
- Dijkstra, Huffman, and earliest-deadline scheduling want a min-heap. A fixed-size top-k of the largest values also wants a min-heap of size k: the root is the smallest of the winners and the first to be evicted.
- Heapsort in the textbook style often uses a max-heap so the extracted root lands at the end of the array. heapq.nlargest is the library form of "k largest" and does not require you to build a max-heap yourself.
- Both compare with < only, in heapq. There is no separate max comparator. Negating numbers is the older way to flip a min-heap.

**When to use:**

Min-heap: next smallest, or the gatekeeper of a top-k largest set. Max-heap: next largest, or the lower half of a running median.

**Sources:**

https://docs.python.org/3/library/heapq.html
https://en.wikipedia.org/wiki/Binary_heap
</details>

### Heap Operations and Challenges

<details>
<summary>Find 100 largest numbers in an Array of 1 billion numbers.</summary>

### Find 100 largest numbers in an Array of 1 billion numbers.

**Definition:**

Keep a min-heap of 100 values. For each later number, if it is larger than the root, replace the root. The heap holds the 100 largest seen so far, and the root is the smallest of them.

**How it works:**

- Memory is O(100), not O(1e9). One pass touches each element once.
- Each of the billion steps is O(log 100). heapq.heapreplace pops the smallest and pushes the new value in one step. heappushpop is the other combined operation; heapreplace is the one that always removes the current root.
- heapq.nlargest(100, data) is the same idea. The docs say nlargest is aimed at small n relative to the data, and that a full sorted() is better when n is large. 100 out of a billion is the small-n case.
- If the array fits in memory and you need a stable full order, sorted is simpler. It is the wrong budget when only 100 winners matter.

**Examples:**

```text
import heapq
h = data[:100]
heapq.heapify(h)
for x in data[100:]:
    if x > h[0]:
        heapq.heapreplace(h, x)
```

**Sources:**

https://docs.python.org/3/library/heapq.html
</details>

<details>
<summary>Implement the operation Decrease-Key in a Binary Heap, and discuss its complexity.</summary>

### Implement the operation Decrease-Key in a Binary Heap, and discuss its complexity.

**Definition:**

Decrease-key lowers a key that is already in the heap and then bubbles it up while it is smaller than its parent. On a binary heap the binary-heap article lists this as O(log n) average and worst case.

**How it works:**

- You need the index of the node. The heap property does not find an arbitrary key quickly. A side map from item to index makes the start of the bubble O(1) expected.
- After the key drops, swap toward the root while the child is smaller than the parent. The height of a complete tree is floor(log n), so the walk is O(log n).
- heapq has no decrease-key call. Its priority-queue notes say changing a priority in place breaks the invariant, so the practical pattern is to mark the old entry removed and push a new entry with the new priority. Pop skips removed entries.
- A Fibonacci heap makes decrease-key amortized O(1), which is why Dijkstra's bound improves there. A binary heap pays the logarithm.

**Sources:**

https://en.wikipedia.org/wiki/Binary_heap
https://docs.python.org/3/library/heapq.html
https://en.wikipedia.org/wiki/Fibonacci_heap
</details>

<details>
<summary>Solve the Kth Smallest Element in a Min Heap without modifying the heap.</summary>

### Solve the Kth Smallest Element in a Min Heap without modifying the heap.

**Definition:**

The root is the smallest. The k-th smallest is found by exploring candidates in an auxiliary min-heap, starting from the root, without popping the original heap.

**How it works:**

- Push the root index into the auxiliary heap. Repeat k times: pop the smallest candidate and push its two children, if they exist.
- Each step pushes at most two children, so the auxiliary heap stays O(k) and the cost is O(k log k). You never write into the original array.
- Child indexes in a zero-based binary heap are 2*i+1 and 2*i+2, the same layout heapq documents.
- Sorting a copy is O(n log n) and also leaves the original alone, but it does not use the heap order you already have.

**Examples:**

```text
aux = [(heap[0], 0)]
for _ in range(k - 1):
    val, i = heapq.heappop(aux)
    for j in (2 * i + 1, 2 * i + 2):
        if j < len(heap):
            heapq.heappush(aux, (heap[j], j))
return aux[0][0]
```

**Sources:**

https://docs.python.org/3/library/heapq.html
https://en.wikipedia.org/wiki/Binary_heap
</details>

<details>
<summary>How could you implement a heap which supports the operation of Deleting any element (not only the root)?</summary>

### How could you implement a heap which supports the operation of Deleting any element (not only the root)?

**Definition:**

Keep a map from the element to its index in the heap array. To delete it, swap it with the last slot, shrink the array, and sift the swapped element up or down. If you cannot sift in place, mark the entry removed and skip it on pop, which is the pattern heapq's priority-queue notes use.

**How it works:**

- Finding the element is the hard part. A binary heap does not search by key in O(log n). The map makes find expected O(1). The sift is O(log n).
- heapq says a direct priority change or removal breaks the invariant if you only edit the tuple. The documented workaround stores entries as [priority, count, task], keeps entry_finder, and sets the task slot to a REMOVED sentinel. pop_task discards sentinels.
- The sentinel version does not reclaim the slot until that entry reaches the root. The index-and-sift version removes it immediately and keeps the array tight.
- Decrease-key is the same map plus a sift up. Delete-any is that plus the possibility of sifting down when the hole is filled by the old last element.

**Sources:**

https://docs.python.org/3/library/heapq.html
https://en.wikipedia.org/wiki/Binary_heap
</details>

### Map Data Structures Basics

<details>
<summary>Define a Map data structure and explain how it differs from a Heap.</summary>

### Define a Map data structure and explain how it differs from a Heap.

**Definition:**

A map stores values under keys and fetches a value by its key. A heap stores keys so that the extreme key is at the root. Java's HashMap is the hash-table map; a binary heap is the priority queue.

**How it works:**

- HashMap documents expected constant-time get and put when the hash spreads keys, and it allows one null key. A heap's root is an extreme, not an arbitrary lookup.
- A map does not promise which key is smallest. A heap does not promise O(1) lookup of an arbitrary key.
- TreeMap is still a map: red-black, sorted, log(n) get and put. It is not a heap. The order is search-tree order, so any key is findable by comparison.
- They compose. The heapq notes use a dictionary, entry_finder, beside the heap so a task can be found and then marked removed.

**When to use:**

Use a map to remember a value by identity. Use a heap to pick the next extreme. Use both when you must find an item and also change its priority.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://en.wikipedia.org/wiki/Binary_heap
https://docs.python.org/3/library/heapq.html
</details>

<details>
<summary>What is a Hash Map, and how does it work internally?</summary>

### What is a Hash Map, and how does it work internally?

**Definition:**

A hash map turns a key into a bucket index with a hash function and stores the key and value in that bucket. Java's HashMap is this structure: expected constant-time get and put when the hash disperses keys.

**How it works:**

- The hash-table article names the two collision strategies: separate chaining, several entries in one slot, and open addressing, which probes for another slot.
- HashMap's capacity is the bucket count. The load factor, default 0.75, is how full the table may get before a rehash about doubles the buckets.
- Iteration costs capacity plus size, so a huge capacity with few entries makes a walk expensive even when lookups stay short. The docs say not to set the initial capacity too high or the load factor too low if iteration matters.
- One null key is allowed. Equality and the hash must agree, or a key you stored cannot be found.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Discuss various collision resolution strategies in a Hash Map.</summary>

### Discuss various collision resolution strategies in a Hash Map.

**Definition:**

Two keys can hash to one bucket. The hash-table article names two families: separate chaining stores several entries in that slot, and open addressing probes for another empty slot.

**How it works:**

- Chaining keeps a list (or a tree of long chains) in the bucket. Lookup walks that list and compares keys. A deletion unlinks one node and does not disturb other buckets.
- Open addressing stores one entry per slot. Linear probing checks the next slots. Robin Hood probing keeps the key that has traveled farther from home. Cuckoo hashing gives each key two homes and kicks the resident to its other home.
- Chaining tolerates a load factor above 1, at the cost of pointer chasing. Open addressing wants empty slots, so the load factor stays below 1, and it is usually kinder to the cache.
- HashMap documents a 0.75 load factor and a rehash that grows the bucket array, which shortens chains or probe runs after the table fills.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

### Map Data Structures Implementation

<details>
<summary>Implement a simple Hash Map with Chaining for collision resolution.</summary>

### Implement a simple Hash Map with Chaining for collision resolution.

**Definition:**

An array of buckets each holds a list of (key, value) pairs. The hash modulo the bucket count picks the list. A colliding key is appended to that list.

**How it works:**

- get walks the list and returns the value whose key compares equal. put replaces the value when the key is already there, otherwise appends.
- This is separate chaining from the hash-table article. Deleting unlinks the pair. Other buckets are untouched.
- When the number of pairs exceeds a load threshold, allocate a larger bucket array and rehash. HashMap uses 0.75 and about doubles capacity.
- A bad hash that sends every key to one list makes get O(n). The expected constant time assumes the hash spreads keys.

**Examples:**

```text
def put(buckets, key, value):
    slot = hash(key) % len(buckets)
    for i, (k, _) in enumerate(buckets[slot]):
        if k == key:
            buckets[slot][i] = (key, value)
            return
    buckets[slot].append((key, value))
```

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

<details>
<summary>How does a Tree Map work, and when would you use it over a hash map?</summary>

### How does a Tree Map work, and when would you use it over a hash map?

**Definition:**

Java's TreeMap is a red-black tree sorted by the keys' natural order or by a comparator. containsKey, get, put, and remove are guaranteed log(n).

**How it works:**

- Lookup walks comparisons, not a hash. The left subtree holds smaller keys, the right holds larger keys, and rotations keep the tree balanced.
- You get successor, predecessor, and sub-maps because the keys are ordered. HashMap does not promise order and does not offer a range.
- HashMap aims at expected constant-time get and put and allows one null key. TreeMap needs a comparison order consistent with equals, and it has no null-key shortcut of that kind.
- Use TreeMap when the caller asks for order or a range. Use HashMap when the only question is the value for one exact key.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

<details>
<summary>Could you implement a map using an Array? If so, what would be the limitations?</summary>

### Could you implement a map using an Array? If so, what would be the limitations?

**Definition:**

Yes, if the key is a dense integer in a known range: the array index is the key and the slot is the value. That is a direct-address table, not a hash.

**How it works:**

- Lookup is a true O(1) index, with no hash and no collision. Missing keys are empty slots.
- The array is as large as the key domain. A map from arbitrary strings, or from integers spread across a huge range, wastes memory on empty slots. A hash map sizes itself to the number of stored keys, not the size of the domain.
- Keys that are not integers need an encoding into indexes first. Two keys that encode to one index collide, and you are back to chaining or probing.
- Java's HashMap exists for the general case: any key with a hash and equality, one null key, and growth by load factor rather than by the size of the universe.

**When to use:**

Use a plain array for dense integer keys such as small ids. Use a hash map when the keys are sparse or not integers.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

### Maps Operations and Complexities

<details>
<summary>What is the average-case and worst-case Time Complexity for operations in a Hash Map?</summary>

### What is the average-case and worst-case Time Complexity for operations in a Hash Map?

**Definition:**

With a hash that spreads keys, HashMap documents constant-time get and put. The hash-table article says average lookup in a well-dimensioned table does not depend on the number of stored elements. The worst case is a walk of every key that landed in one bucket.

**How it works:**

- Chaining worst case is O(n) when every key shares one list. Open addressing can also probe across the whole table when it is full of collisions.
- Average cost stays short while the load factor stays in the range the design expects. HashMap rehashes around load factor 0.75 so chains do not grow with n forever.
- Iteration is not O(1). HashMap says a walk costs capacity plus size, so a huge empty bucket array dominates.
- Cuckoo hashing is the variant whose lookup is worst-case constant, at two cells, while insert may rebuild. An ordinary HashMap does not promise that worst case.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://en.wikipedia.org/wiki/Hash_table
https://en.wikipedia.org/wiki/Cuckoo_hashing
</details>

<details>
<summary>How do Cuckoo Hashing and Linear Probing improve Hash Map performance?</summary>

### How do Cuckoo Hashing and Linear Probing improve Hash Map performance?

**Definition:**

Linear probing and cuckoo hashing are open-addressing layouts that avoid a pointer chase per entry. They improve on a chain of separately allocated nodes when the table sits in cache.

**How it works:**

- Linear probing checks the next array slots after a collision. Those slots are adjacent, so one cache line often covers the probe. The hash-table article credits this scheme to open addressing.
- Cuckoo hashing looks at two homes and stops. Lookup is worst-case constant. The cuckoo article says that is often 20–30% slower than linear probing because of two cache misses, and that the win is the worst-case bound, which matters for real-time lookup.
- Cuckoo insert stays expected constant only below a load factor of one half, then it may rehash. Linear probing degrades as the table fills and clusters form, which is why the load factor stays below 1.
- Neither fixes a hash that piles keys onto one home. Both still need the hash to spread keys.

**When to use:**

Linear probing for a tight in-memory table and short probes. Cuckoo hashing when lookup must be worst-case constant and a load near one half is acceptable.

**Sources:**

https://en.wikipedia.org/wiki/Cuckoo_hashing
https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Explain the impact of the Load Factor on a Hash Map’s performance.</summary>

### Explain the impact of the Load Factor on a Hash Map’s performance.

**Definition:**

The load factor is the number of stored entries divided by the number of buckets. The hash-table article says a lower load factor generally makes operations faster. HashMap's default is 0.75.

**How it works:**

- As the ratio grows, chains get longer and probe sequences get longer, so get and put slow down. Past 1, open addressing has no empty slot left.
- HashMap rehashes when entries exceed load factor times capacity, rebuilding into about twice as many buckets. That restores a short average probe at the cost of one O(n) rebuild.
- A very low load factor wastes memory and makes iteration slower, because a walk costs capacity plus size. The HashMap docs warn against a capacity that is too high or a load factor that is too low when iteration matters.
- Cuckoo hashing has a sharper threshold: below one half, insert is expected constant; above it, placement fails and the table must be rebuilt.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://en.wikipedia.org/wiki/Cuckoo_hashing
</details>

### Maps in Algorithms and Applications

<details>
<summary>Describe a scenario where a Map is used in a Caching System (like LRUCache).</summary>

### Describe a scenario where a Map is used in a Caching System (like LRUCache).

**Definition:**

An LRU cache maps a request key to a value and must also know which key was used least recently. The map finds the entry. An order structure, such as Java's LinkedHashMap, remembers recency.

**How it works:**

- get and put on the map are expected constant time, which is why the index is a hash map and not a scan of the cached values.
- On a hit, the key moves to the most-recent end. On a miss at capacity, the least-recent key is evicted. LinkedHashMap is the HashMap subclass that keeps insertion or access order.
- The map alone does not know recency. A heap of timestamps can evict the oldest, but updating a key's timestamp is a decrease-key, not a plain put. A hash map plus a linked list does both the lookup and the move-to-front in expected O(1).
- A cluster of cache servers adds consistent hashing in front: the ring picks the server, and that server's map stores the entry.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://docs.python.org/3/library/heapq.html
https://en.wikipedia.org/wiki/Consistent_hashing
</details>

<details>
<summary>How are maps used in the implementation of Graph Data Structures like adjacency lists?</summary>

### How are maps used in the implementation of Graph Data Structures like adjacency lists?

**Definition:**

An adjacency list maps each vertex to the collection of its neighbors. A hash map does that when vertex ids are sparse, strings, or objects. An array does it when vertices are dense integers.

**How it works:**

- Edge insert is a put on the source vertex's collection. Neighbors of v are one get. Both are expected constant time on a HashMap, plus the cost of updating the neighbor collection.
- A matrix is an array of arrays and answers "is there an edge" by index, but it spends memory on missing edges. The map spends memory on vertices and edges that exist.
- Weights live in the neighbor record, or in a second map from an (edge) pair to a weight. The heap in Dijkstra then orders those weights. The map is how you find the list to relax.
- TreeMap would keep vertices in sorted key order at log(n) per lookup. Use it when you iterate vertices in order. Use HashMap when you only follow edges.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html
</details>

<details>
<summary>Discuss the role of maps in constructing an efficient Word Dictionary or Inverted Index.</summary>

### Discuss the role of maps in constructing an efficient Word Dictionary or Inverted Index.

**Definition:**

A dictionary maps a word to its definition or to the list of documents that contain it. An inverted index is that map from term to postings. Lookup of one word does not scan the corpus.

**How it works:**

- HashMap gives expected constant-time get for an exact word. Building the index increments a postings list on each occurrence.
- A trie is the better map when the query is a prefix. A hash map of whole words cannot list completions without scanning keys. TreeMap can walk a range of words that share a prefix only if you use successor iteration, at log(n) per step.
- Frequencies are a map from word to a count. The k most common words then come from a heap of size k over those counts.
- The load factor still applies. A large vocabulary rehashes at 0.75 in HashMap so chains stay short as documents are added.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html
https://docs.python.org/3/library/heapq.html
</details>

### Maps and Heaps Coding Challenges

<details>
<summary>Design a data structure that supports the following operations: Insertion, Deletion, Get-Random Element, where all operations have equal probability using both Heaps and Maps.</summary>

### Design a data structure that supports the following operations: Insertion, Deletion, Get-Random Element, where all operations have equal probability using both Heaps and Maps.

**Definition:**

Uniform get-random means every stored element is equally likely. Store the elements in an array, which is also the layout of a binary heap, and keep a map from value to index. Random draws an index. Delete swaps that index with the last slot.

**How it works:**

- Insert appends and records the index in the map. Expected O(1) for the map write, O(1) for the append.
- Delete looks up the index, swaps with the last element, updates the map for the swapped element, and pops the array. If the array must also stay a heap, sift the swapped element up or down, which is O(log n). The map is what makes the sift start at the right index.
- Get-random is array[random(0, n)]. Every index holds one live element, so the draw is uniform. The heap order is irrelevant to fairness. The root is not a uniform sample.
- heapq's own delete-any marks an entry removed and skips it later. That also uses a map, but a random index could land on a tombstone. The swap-with-last array does not have holes, so the random draw stays uniform.

**When to use:**

Keep the heap order only if you also need the extreme element. For insert, delete, and uniform random alone, the array plus the map is enough and the sift is optional.

**Sources:**

https://docs.python.org/3/library/heapq.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

<details>
<summary>Implement a Counter Map to find and list the k most frequent elements in an array.</summary>

### Implement a Counter Map to find and list the k most frequent elements in an array.

**Definition:**

Count with a map from value to frequency, then keep a min-heap of the k best counts. The root of that heap is the smallest frequency still in the top k.

**How it works:**

- One pass builds the map. HashMap-style get and put are expected O(1) per element, so the count is expected O(n).
- A second pass pushes into a heap of size k. heapq is a min-heap, so you evict the root when a count is larger. That is O(n log k).
- heapq.nlargest(k, counts, key=...) is the same selection. The docs recommend it when k is small relative to the number of distinct values.
- Ties need a rule: equal counts can break by value, using a tuple (count, value) so the heap compares a total order.

**Examples:**

```text
from collections import Counter
import heapq
counts = Counter(nums)
top = heapq.nlargest(k, counts, key=counts.get)
```

**Sources:**

https://docs.python.org/3/library/heapq.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

<details>
<summary>Write a solution for the Sliding Window Maximum problem using Heaps.</summary>

### Write a solution for the Sliding Window Maximum problem using Heaps.

**Definition:**

A max-heap stores (value, index) for elements inside the current window of width k. The root is the maximum, after you discard indexes that have slid out of the window.

**How it works:**

- As the right edge advances, push the new pair. Before reading the answer, pop while the root index is <= i - k.
- Each index is pushed once and popped once, so the cost is O(n log n). A deque that drops dominated indexes is O(n) and is the tighter solution. The heap still meets the problem if a heap is required.
- heapq is a min-heap. Push negated values, or use heappop_max where Python 3.14 provides it. Store the index beside the value so an outdated root can be recognized.
- Do not decrease a key in the heap when the window moves. Leaving the stale pair and skipping it on read is the same lazy deletion heapq uses for removed tasks.

**Examples:**

```text
heap = []  # (-value, index), min-heap of negated values
for i, x in enumerate(nums):
    heapq.heappush(heap, (-x, i))
    while heap[0][1] <= i - k:
        heapq.heappop(heap)
    if i >= k - 1:
        emit(-heap[0][0])
```

**Sources:**

https://docs.python.org/3/library/heapq.html
</details>

<details>
<summary>Solve the Top K Frequent Words problem with a combination of Heap and Map data structures.</summary>

### Solve the Top K Frequent Words problem with a combination of Heap and Map data structures.

**Definition:**

A map counts words. A heap of size k then keeps the best (count, word) pairs. The map is the frequency table. The heap is the selection.

**How it works:**

- Count in one pass. Expected O(n) with a hash map.
- Push (count, word) into a heap of size k. heapq.nlargest can take a key. For a manual min-heap, order by count ascending and break ties by the word order the problem asks, so the root is the first pair to evict.
- Cost after the count is O(n log k). Sorting every distinct word is O(m log m) and is fine when m is close to k.
- The heap does not replace the map. Without the counts you would re-scan the text for every candidate.

**Sources:**

https://docs.python.org/3/library/heapq.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

<details>
<summary>Design a Leaderboard system that supports fast player score updates and rank retrieval using both Heaps and Maps.</summary>

### Design a Leaderboard system that supports fast player score updates and rank retrieval using both Heaps and Maps.

**Definition:**

A map from player to score makes an update find the old score in expected constant time. Rank — how many players are strictly ahead — needs ordered scores. A binary heap gives you the extreme, not the rank. Java's TreeMap gives log(n) get, put, and remove on an ordered key, which is the structure that can count how many keys are above a score.

**How it works:**

- Update: read the old score from the map, remove that score from the ordered structure, write the new score, and put it back. A heap can do the remove only with an index map and a sift, and it still cannot count the players above you without a walk.
- A size-k min-heap answers "top k" only. That is the leaderboard strip, not "what is player P's rank". Use the heap for the strip and the ordered map for exact rank.
- If scores collide, the ordered key must include the player id, or a multiset of scores, so two players with the same score both exist.
- heapq's lazy deletion can refresh a top-k heap by marking the old (score, player) removed and pushing the new one. Pop skips tombstones. That keeps the top-k strip current. It does not compute a rank.

**When to use:**

Map plus TreeMap (or a balanced tree of scores) for update and rank. Add a heap only for a top-k view. A heap alone does not retrieve rank in logarithmic time.

**Sources:**

https://docs.python.org/3/library/heapq.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html
</details>
