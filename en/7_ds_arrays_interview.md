# Arrays — Interview

Answers are filled.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Arrays — Interview](./7_ds_arrays_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Arrays](https://devinterview.io/questions/data-structures-and-algorithms/array-data-structure-interview-questions/).

### Fundamental Array Concepts

<details>
<summary>What is an Array?</summary>

### What is an Array?

**Definition:**

An array stores elements of one width in one contiguous block, so an index turns into an address with arithmetic.

**How it works:**

- The address of index `i` is `base + i × element size`. That is why random access is O(1).
- A search in an unsorted array is O(n). Insert or delete in the middle shifts neighbors, so those are O(n).
- The capacity is fixed unless the language wraps the block in a dynamic array. Indexes are usually zero-based.
- Contiguous layout makes a sequential scan friendly to the CPU cache.

**Examples:**

```text
base = 1000, element size = 4
index 3 -> address 1012
values [10, 20, 30, 40], a[2] = 30
```
</details>

<details>
<summary>What are Dynamic Arrays?</summary>

### What are Dynamic Arrays?

**Definition:**

A dynamic array is a contiguous buffer plus a length and a capacity. When an append does not fit, it allocates a larger buffer and copies.

**How it works:**

- Growth is usually by a constant factor, often 2. A series of appends is amortized O(1) even though one resize is O(n).
- Indexed access stays O(1). Insert or delete in the middle is still O(n) because elements shift, and a resize can add another O(n).
- The unused capacity is the space overhead. Shrinking on delete is optional and is usually delayed so the array does not thrash.
- `ArrayList`, `std::vector`, and Python's `list` are this structure.

**Examples:**

```text
capacity 2, length 2, append 7
allocate 4, copy the two old items, write 7, length = 3
```
</details>

<details>
<summary>What is an Associative Array (Dictionary)?</summary>

### What is an Associative Array (Dictionary)?

**Definition:**

An associative array stores values under keys and looks them up by key, not by a dense index.

**How it works:**

- A hash table does this in expected O(1) and can fall to O(n) when every key lands in one bucket.
- A balanced search tree does it in O(log n) and keeps keys ordered.
- A list of pairs is O(n) and is only reasonable for a handful of keys.
- Writing an existing key replaces the value. Missing keys are an absence, not a hole in the middle of an index range.

**Examples:**

`prices["tea"] = 2` then `prices["tea"]` returns 2. The key is the string, not a position.
</details>

<details>
<summary>What defines the Dimensionality of an array?</summary>

### What defines the Dimensionality of an array?

**Definition:**

Dimensionality is how many indexes you need to name one element.

**How it works:**

- One index is a sequence. Two indexes are a grid. Three indexes are a stack of grids.
- The element count is the product of the axis lengths.
- Memory is still one block in row-major or column-major order, or it is an array of arrays with a separate allocation per row.
- Row-major offset for `[r, c]` is `r × columns + c`. Column-major swaps those roles.

**Examples:**

A 2×3 grid `[ [1, 2, 3], [4, 5, 6] ]` in row-major order is the flat buffer `[1, 2, 3, 4, 5, 6]`. Element `[1, 2]` sits at offset `1×3+2 = 5`.
</details>

### Array Types and Properties

<details>
<summary>Name some Advantages and Disadvantages of arrays.</summary>

### Name some Advantages and Disadvantages of arrays.

**Definition:**

Arrays win on index access and on scans. They lose when the middle of the collection changes all the time or the size is unknown and huge.

**How it works:**

- O(1) access by index, and a sequential scan that uses the cache because neighbors sit next to each other.
- Fixed capacity unless you pay for a dynamic array's copies. Insert and delete in the middle are O(n).
- A hole in the middle is either a shift or a wasted slot. Sparse data pays for every empty cell.
- Prefer an array when the size is known or index access dominates. Prefer a list or a hash table when the middle changes constantly or lookup is by an arbitrary key.
</details>

<details>
<summary>Explain Sparse and Dense arrays.</summary>

### Explain Sparse and Dense arrays.

**Definition:**

A dense array allocates every slot, including defaults. A sparse array stores only the entries that differ from the default, together with their indexes.

**How it works:**

- Dense lookup is arithmetic and O(1). Sparse lookup searches a map, a coordinate list, or a compressed index.
- CSR and CSC store non-zeros of a matrix plus row or column pointers. A coordinate list stores `(row, column, value)` triples.
- Sparse wins when almost every cell is empty, as in term-document counts or many linear systems. Dense wins when a large fraction of cells are filled, because the index would cost more than the holes.
- Arithmetic on sparse formats has to skip zeros explicitly. A library that densifies by accident can blow memory.
</details>

<details>
<summary>What are advantages and disadvantages of Sorted Arrays?</summary>

### What are advantages and disadvantages of Sorted Arrays?

**Definition:**

A sorted array keeps elements in order so search and range queries are logarithmic, and every insert pays a shift to restore that order.

**How it works:**

- Binary search, successor, predecessor, and range queries are O(log n), and the scan of a range is cache-friendly.
- Insert and delete are O(n) because elements move to keep the order.
- The order is total, which a heap does not give you. You can binary-search any key, not only read the extreme.
- It fits data that is searched often and changed rarely.
</details>

<details>
<summary>What are the advantages of Heaps over Sorted Arrays?</summary>

### What are the advantages of Heaps over Sorted Arrays?

**Definition:**

A binary heap keeps only a parent-child order, so the extreme element is at the root in O(1) and an update of that extreme is O(log n).

**How it works:**

- A sorted array also exposes the minimum or maximum in O(1), but every insert or delete shifts elements and is O(n).
- The heap does not support fast search for an arbitrary key. Finding a middle value is O(n).
- Use a heap when the set changes often and you only need the current extreme, as in a priority queue or Dijkstra.
- Use a sorted array when you need binary search or a range, not only the end of the order.
</details>

### Array Operations and Management

<details>
<summary>How does Indexing work in arrays?</summary>

### How does Indexing work in arrays?

**Definition:**

An index names a position, usually from 0 through n−1, and the address is computed instead of scanned.

**How it works:**

- Equal-sized elements make `address(i) = base + i × width`.
- A bounds check compares `i` with the length. Skipping it is a memory-safety bug.
- A multi-dimensional index flattens. In row-major order the offset is `row × row_length + column`.
- Negative indexes in some languages count from the end and are sugar over `length + i`, not a second array.

**Examples:**

`a = [10, 20, 30]`, `a[0]` is 10, `a[2]` is 30. In Python `a[-1]` is the same slot as `a[2]`.
</details>

<details>
<summary>Merge two Sorted Arrays into one Sorted Array.</summary>

### Merge two Sorted Arrays into one Sorted Array.

**Definition:**

Walk both sorted inputs with one index each and always append the smaller current value.

**How it works:**

- If the heads are equal, append one and then the other; stability keeps the left input's copy first if you care.
- When one input ends, copy the rest of the other. Those leftovers are already sorted.
- Time is O(n + m). The output needs O(n + m) extra space. This is the combine step of mergesort.
- Merging in place into a buffer that already has empty slots at the end is the same comparison order run backwards.

**Examples:**

```text
[1, 4, 6] and [2, 3, 5]
-> 1, then 2, then 3, then 4, then 5, then 6
```
</details>

<details>
<summary>Implement three Stacks with one Array.</summary>

### Implement three Stacks with one Array.

**Definition:**

Either give each stack a fixed slice of the array, or store all three as linked lists of slots plus a free list so any stack can use any free slot.

**How it works:**

- Fixed thirds: each stack has its own top inside `[0, n/3)`, `[n/3, 2n/3)`, and `[2n/3, n)`. Push and pop are O(1), and one stack can be full while another slice is empty.
- Flexible layout: each slot stores a value and the index of the previous slot of the same stack. A free list threads unused indexes.
- Push pops a free index, writes the value, and points it at the old top. Pop returns that index to the free list.
- Both versions are O(1) per operation. The flexible one uses an extra integer per slot and fills the whole array.

**Examples:**

```text
array slots: [a, b, _, c]
tops: stack0=0, stack1=1, stack2=3
free: 2
push stack0 -> take slot 2, prev=0
```
</details>

<details>
<summary>How do you perform Array Rotation and what are its applications?</summary>

### How do you perform Array Rotation and what are its applications?

**Definition:**

A rotation moves a prefix to the end, or a suffix to the front, without changing the relative order inside those pieces.

**How it works:**

- Left rotate by `k` is three reversals: reverse the first `k`, reverse the rest, reverse the whole array. Right rotate reverses the whole array first.
- `k` is taken modulo n so a huge count does not loop. Time is O(n) and extra memory is O(1).
- A juggling cycle or a temporary buffer of `k` also works. Reversal is the in-place form that needs no extra buffer.
- The same idea appears in ring buffers, rotating an image row, and testing whether one string is a rotation of another by searching `s + s`.

**Examples:**

```text
[1, 2, 3, 4, 5] left by 2
reverse [1, 2] -> [2, 1, 3, 4, 5]
reverse [3, 4, 5] -> [2, 1, 5, 4, 3]
reverse all -> [3, 4, 5, 1, 2]
```
</details>

<details>
<summary>Reverse an Array in place.</summary>

### Reverse an Array in place.

**Definition:**

Swap the element at the left index with the element at the right index, then move both indexes toward the center until they meet.

**How it works:**

- About `n/2` swaps run in O(n) time with O(1) extra memory. The original array is overwritten.
- Copy first if the old order must survive.
- An odd length leaves the middle element unswapped, which is correct.
- The same two-pointer pattern reverses a string buffer or a slice of an array.

**Examples:**

```text
[1, 2, 3, 4]
swap 1 and 4 -> [4, 2, 3, 1]
swap 2 and 3 -> [4, 3, 2, 1]
```
</details>

<details>
<summary>Remove Duplicates from a sorted array without using extra space.</summary>

### Remove Duplicates from a sorted array without using extra space.

**Definition:**

Because the array is sorted, duplicates sit together. A write index keeps the last unique value, and a read index scans forward.

**How it works:**

- Start `write` at 0. For each later `read`, if `a[read]` differs from `a[write]`, advance `write` and copy `a[read]` there.
- The unique prefix has length `write + 1`. Slots past that length are leftovers and must be ignored.
- Time is O(n) and extra memory is O(1). The algorithm relies on sorted order; an unsorted array needs a set or a sort first.
- A variant that allows each value twice compares against `a[write - 1]` instead of `a[write]`.

**Examples:**

```text
[1, 1, 2, 2, 3]
write stays on the first 1, then copies 2, then copies 3
unique prefix [1, 2, 3], length 3
```
</details>

<details>
<summary>Implement a Queue using an array.</summary>

### Implement a Queue using an array.

**Definition:**

A ring buffer stores the queue in a fixed array with a head index, a tail index, and a size, and wraps with modulo.

**How it works:**

- Enqueue writes at `tail` and sets `tail = (tail + 1) % capacity`. Dequeue reads `head` and advances it the same way.
- Both are O(1). The queue is empty when `size` is 0 and full when `size` equals capacity.
- Without wrap-around, dequeued slots at the front are wasted and the queue seems full while the back is the only part in use.
- Resizing copies the live elements in order from `head`, then resets `head` to 0. Space stays proportional to capacity.

**Examples:**

```text
capacity 4, enqueue 1, 2, 3 -> slots [1, 2, 3, _]
dequeue -> 1, head wraps
enqueue 4, 5 -> [5, 2, 3, 4]
```
</details>

### Array-based Algorithms

<details>
<summary>Check the String for Balanced Parentheses, using linear time and constant space.</summary>

### Check the String for Balanced Parentheses, using linear time and constant space.

**Definition:**

A Dyck word is a balanced bracket string. For one bracket pair, a counter checks it in one pass with a fixed amount of storage.

**How it works:**

- Walk the string once. `(` increments the counter. `)` decrements it. If the counter would go negative, a close has no open. If it is not zero at the end, an open has no close.
- The counter is a constant number of integers, and the walk visits each character once. That is the linear-time, constant-space check the question asks for.
- Two or more bracket kinds are not settled by one counter. `([)]` and `([])` can share the same counts and only one is balanced. Those need a stack of the still-open kinds, and that stack grows with the unmatched prefix.
- The stack version is still one pass. Its extra memory follows the depth, so it is not the constant-space solution.

**Examples:**

```js
function balanced(s) {
  let n = 0;
  for (const ch of s) {
    if (ch === "(") n++;
    else if (ch === ")") {
      if (n === 0) return false;
      n--;
    }
  }
  return n === 0;
}
```

**Sources:**

- [Wikipedia — Dyck word](https://en.wikipedia.org/wiki/Dyck_word)
</details>

<details>
<summary>Explain an algorithm to find the Longest Consecutive Sequence in an unsorted array.</summary>

### Explain an algorithm to find the Longest Consecutive Sequence in an unsorted array.

**Definition:**

The hashing approach finds the longest run of values that differ by one, even when they are not adjacent in the array.

**How it works:**

- GeeksforGeeks states hashing as O(n) time and O(n) space. Sorting is the other listed approach, O(n log n) time and O(1) extra space, then a scan of the sorted array for the longest subarray of consecutive elements.
- Put the values in a set. A value starts a run only when `value - 1` is absent. From there walk `value`, `value + 1`, … while the set contains them, and keep the longest length.
- Each value starts at most one walk, because a value with a predecessor is skipped as a start. The sort-based scan instead counts a streak while neighbors in the sorted array differ by one, and resets the streak when they do not.
- Duplicates do not extend a consecutive sequence. The set collapses them; the sorted scan must not count a repeated value as a new step.

**Examples:**

```text
[100, 4, 200, 1, 3, 2]
1 starts a run (0 is absent): 1, 2, 3, 4 -> length 4
```

**Sources:**

- [GeeksforGeeks — Longest consecutive subsequence](https://www.geeksforgeeks.org/dsa/longest-consecutive-subsequence/)
</details>

<details>
<summary>Find the “Kth” Largest Element in an Array.</summary>

### Find the “Kth” Largest Element in an Array.

**Definition:**

Quickselect finds the kth order statistic by partitioning the array around a pivot and recurring into only one side.

**How it works:**

- It uses the same partition as quicksort, but does not recur into both sides. Wikipedia says the average complexity is linear and the worst case, from consistently bad pivots, is quadratic.
- The kth largest is the element that would sit at index `n - k` after an ascending sort. Partition until the pivot lands on that index.
- Median of medians chooses the pivot so the worst case is also linear. Wikipedia notes the overhead is high and this pivot is generally not used in practice; it can be a fallback when the basic pivot is bad.
- A full heapsort also yields the element, and heapsort is O(n log n) in the best, average, and worst case. That sorts the whole array, which is more work than stopping at one rank.

**Examples:**

```text
[3, 2, 1, 5, 6, 4], 2nd largest
partition until index n - 2 holds 5
```

**Sources:**

- [Wikipedia — Quickselect](https://en.wikipedia.org/wiki/Quickselect)
- [Wikipedia — Heapsort](https://en.wikipedia.org/wiki/Heapsort)
</details>

<details>
<summary>Explain how to do Binary Search in a Sorted Array.</summary>

### Explain how to do Binary Search in a Sorted Array.

**Definition:**

Binary search finds a target in a sorted array by comparing it with the middle element and discarding the half that cannot contain it.

**How it works:**

- If the middle equals the target, stop. If the target is smaller, continue on the left half. If it is larger, continue on the right half. The array must already be sorted, or the discarded half is not known to be empty of the target.
- Wikipedia states the worst case is floor(log2(n)) + 1 iterations, because that is the depth of the comparison tree, and no comparison-only search has a better worst-case number of iterations.
- The loop maintains an inclusive low and an exclusive or inclusive high. The middle must be computed without overflowing the index type: `low + (high - low) / 2`, not `(low + high) / 2` when the sum can overflow.
- If the search ends with an empty range, the target is absent. The same loop finds the first position where the target could be inserted, by remembering the boundary instead of returning early.

**Examples:**

```text
[1, 3, 4, 7, 9], target 7
mid 4 -> 7 is to the right
mid 7 -> found
```

**Sources:**

- [Wikipedia — Binary search algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
</details>

<details>
<summary>Implement Heap Sort using an Array.</summary>

### Implement Heap Sort using an Array.

**Definition:**

Heapsort sorts an array by turning it into a binary heap and then repeatedly moving the root to the end.

**How it works:**

- The array is the heap. The children of index `i` are `2i + 1` and `2i + 2`. Building the heap from the bottom is O(n). A parent is greater than or equal to its children in a max-heap, or less than or equal in a min-heap.
- Heapsort's best, average, and worst time are O(n log n). Each sift is bounded by the height, and the height of a binary heap is logarithmic.
- After the heap exists, swap the root with the last unsorted slot, shrink the heap by one, and sift the new root down. Repeat until one element remains.
- The sort is in the same array. Delete-min on a binary heap is O(log n) worst case; heapsort performs that extraction once per element after the O(n) build.

**Examples:**

```text
[3, 1, 2] as a max-heap -> [3, 1, 2]
swap 3 to the end, sift [2, 1] -> [2, 1, 3]
swap 2 -> [1, 2, 3]
```

**Sources:**

- [Wikipedia — Heapsort](https://en.wikipedia.org/wiki/Heapsort)
- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
</details>

### Array Comparison

<details>
<summary>What are some pros and cons of Linked List compared to Arrays?</summary>

### What are some pros and cons of Linked List compared to Arrays?

**Definition:**

A linked list's order is the chain of pointers, not the physical placement of the nodes. An array's order is the order of contiguous slots.

**How it works:**

- Wikipedia: access time in a linked list is linear in the number of nodes, because reaching a node walks the links. A dynamic array is a random-access list.
- Insertion or removal at a node you already hold is the list's efficient case. Inserting into the middle of an array shifts the tail.
- A list node pays for the link as well as the value, and the nodes need not sit together. Sequential locality is linear access of a one-dimensional array, which is the array's layout, not the list's.
- A dynamic array appends in amortized constant time by doubling. A list append is a pointer write only if you already hold the tail.

**When to use:**

Use the list when you insert and delete at known nodes and rarely index by position. Use the array when you need a slot by index or a scan that should touch neighboring memory.

**Sources:**

- [Wikipedia — Linked list](https://en.wikipedia.org/wiki/Linked_list)
- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
</details>

<details>
<summary>Compare Array-based vs Linked List stack implementations.</summary>

### Compare Array-based vs Linked List stack implementations.

**Definition:**

Both can put the next pop at one end. They differ in how that end grows and how a push finds its slot.

**How it works:**

- An array stack pushes at the end. A dynamic array does that in amortized constant time: inserting n elements is O(n) overall, while one reallocation is Θ(n) and doubles the capacity.
- A linked stack pushes by pointing a new node at the old head. That is the list's efficient insertion at a position you already hold, and it does not move the other nodes.
- The array stack reaches the top by index. The list stack reaches it by the head pointer. Walking to an older element is linear in the list and is index arithmetic in the array.
- The array wastes the reserved capacity Wikipedia shows as grey cells. The list spends a pointer per value and scatters the nodes.

**When to use:**

Use the array stack when the size is bounded or doubling is acceptable and you want contiguous slots. Use the linked stack when each push must allocate only one node and you never index into the middle.

**Sources:**

- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
- [Wikipedia — Linked list](https://en.wikipedia.org/wiki/Linked_list)
</details>

<details>
<summary>Compare Dynamic Arrays with Linked Lists.</summary>

### Compare Dynamic Arrays with Linked Lists.

**Definition:**

A dynamic array is a random-access list that grows. A linked list is a chain of nodes whose order is the links.

**How it works:**

- The array overcomes a static array's fixed capacity. It doubles and uses the spare slots. Adding n elements is O(n) overall, so each append is amortized constant time, provided one allocation is O(1). A single growth is still Θ(n).
- List access is linear in the number of nodes. Insertion or removal at a node already in hand does not shift a tail.
- The array's slots are contiguous, which is sequential locality. The list's next node can be anywhere.
- Indexing, slicing, and binary search need the array. A frequently edited sequence of known nodes needs the list.

**When to use:**

Choose the dynamic array for index and scan. Choose the list for splice at a known node when you do not need the i-th element quickly.

**Sources:**

- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
- [Wikipedia — Linked list](https://en.wikipedia.org/wiki/Linked_list)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
</details>

### Advanced Array Topics and Techniques

<details>
<summary>Shuffle an array, ensuring that each element has an equal probability of being in any position.</summary>

### Shuffle an array, ensuring that each element has an equal probability of being in any position.

**Definition:**

The modern Fisher–Yates shuffle produces an unbiased permutation: every permutation is equally likely. It runs in time proportional to the number of items and shuffles them in place.

**How it works:**

- Durstenfeld's version walks the array from the end. At step i it swaps i with a uniformly chosen index in `0..i`. The swapped-out item is the one that has been 'struck', so it is not chosen again.
- A naive method that picks any index and retries on duplicates spends extra time. Wikipedia contrasts that with Durstenfeld, whose swap form avoids the quadratic counting of the pencil-and-paper method. The stated comparison is against O(n²).
- The random index must be uniform on the remaining range. A biased remainder, such as `rand() % (i + 1)` when `rand()` does not divide the range evenly, makes some permutations more likely.
- Sattolo's variant swaps with an index strictly below i and generates a random cycle, not every permutation. Do not use it when every permutation must be possible.

**Examples:**

```js
function shuffle(a) {
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}
```

**Sources:**

- [Wikipedia — Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
</details>

<details>
<summary>Solve the Rain Water Trapping problem using arrays.</summary>

### Solve the Rain Water Trapping problem using arrays.

**Definition:**

Water stands at an index only when a taller bar exists on both sides. The depth is the lower of those two sides, minus the bar's own height.

**How it works:**

- GeeksforGeeks lists prefix and suffix maxima as O(n) time and O(n) space: `left[i]` is the tallest bar at or left of i, `right[i]` is the tallest at or right of i, and the water at i is `min(left[i], right[i]) - height[i]` when that is positive.
- The two-pointer form is O(n) time and O(1) extra space. Move the side whose max is smaller. The water on that side is bounded by that max, because the other side is at least as tall.
- A stack form is also listed, O(n) time and O(n) space. It keeps a decreasing run of bars and settles a valley when a taller bar arrives.
- Bars of equal height trap nothing between them. Negative water is not stored; the formula contributes zero when the bar is itself the limiting side.

**Examples:**

```text
height [0, 1, 0, 2, 1, 0, 1, 3]
water between the 1 and the 3 sums to 5
```

**Sources:**

- [GeeksforGeeks — Trapping Rain Water](https://www.geeksforgeeks.org/dsa/trapping-rain-water/)
</details>

<details>
<summary>Explain the two-pointer technique and give examples of its use in array problems.</summary>

### Explain the two-pointer technique and give examples of its use in array problems.

**Definition:**

Two pointers scan one array from different indexes and move one of them when a predicate says that side is settled.

**How it works:**

- The trapping-rain two-pointer scan is O(n) time and O(1) extra space. Each step advances the pointer on the side with the smaller running maximum, and that index is never revisited.
- The same shape finds a pair with a given sum in a sorted array: start at the ends, advance the left when the sum is too small, and retreat the right when it is too large. Each index moves toward the other at most once.
- Dutch-flag partitioning is three pointers, the same idea with a middle cursor. Wikipedia's bound is Θ(n) moves and examinations.
- The technique needs a reason that the discarded side cannot contain a better answer. Without that reason, moving a pointer loses candidates.

**When to use:**

Use two pointers on a sorted array or on a problem whose answer at an index depends only on a running extreme. Use a hash set when the array is unsorted and you need complements in expected constant lookups.

**Examples:**

```text
sorted [1, 2, 4, 7], sum 6
1+7 too big -> right
1+4 too small -> left
2+4 = 6
```

**Sources:**

- [GeeksforGeeks — Trapping Rain Water](https://www.geeksforgeeks.org/dsa/trapping-rain-water/)
- [Wikipedia — Dutch national flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem)
</details>

<details>
<summary>What are the typical use cases for using an array in a Sliding Window Algorithm?</summary>

### What are the typical use cases for using an array in a Sliding Window Algorithm?

**Definition:**

A sliding window is a contiguous segment of an array whose ends move forward. The array supplies O(1) reads of those ends by index.

**How it works:**

- Wikipedia's monotonic queue finds the minimum or maximum of every window in linear time. The naive scan of each window is O(n·k). A search per window is O(n log n) time and O(n) space.
- The window is a pair of indexes into the array, not a copy of the elements. Adding the new right end and removing the old left end updates a running sum or a count.
- Kadane's scan is the variable window for the maximum sum: extend while the running sum helps, and start over at the current element when it does not. Wikipedia states that this O(n)-time algorithm is as fast as possible, because any algorithm must read the array.
- The array has to be random-access or at least sequential. A structure that cannot drop the leftmost element in step with the right edge is not this window.

**When to use:**

Use a fixed window when the length k is given. Use a variable window when the segment grows and shrinks to satisfy a sum, a count, or Kadane's restart rule.

**Sources:**

- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
- [Wikipedia — Maximum subarray problem](https://en.wikipedia.org/wiki/Maximum_subarray_problem)
</details>

<details>
<summary>Find all pairs in an Array with a given sum.</summary>

### Find all pairs in an Array with a given sum.

**Definition:**

A pair sums to the target when the two values add to it. On a sorted array the two ends find every such pair without a second copy of the values.

**How it works:**

- Sort, unless the array is already sorted. Heapsort does that in O(n log n) best, average, and worst time, in the same array.
- Then set left at 0 and right at n − 1. If the sum is too small, advance left. If it is too large, retreat right. If it matches, record the pair and move both, so the same indexes are not reported twice.
- Each index moves at most n times after the sort. The rain-water page is the same two-pointer movement, there bounded by O(n) for its scan.
- An unsorted scan that, for each value, searches the complement from scratch repeats work. Sorting once, then the inward scan, avoids that.

**Examples:**

```text
[1, 2, 4, 5], target 6
1+5 and 2+4
```

**Sources:**

- [Wikipedia — Heapsort](https://en.wikipedia.org/wiki/Heapsort)
- [GeeksforGeeks — Trapping Rain Water](https://www.geeksforgeeks.org/dsa/trapping-rain-water/)
</details>

### Array Optimization and Use Cases

<details>
<summary>Explain the difference between Column-major Order and Row-major Order in multidimensional arrays.</summary>

### Explain the difference between Column-major Order and Row-major Order in multidimensional arrays.

**Definition:**

Both orders store a multidimensional array in linear memory. They differ in which neighbors are contiguous.

**How it works:**

- In row-major order, consecutive elements of a row sit next to each other. In column-major order, consecutive elements of a column do.
- A scan that walks a row hits sequential addresses in row-major layout and strides by the column count in column-major layout. Sequential locality is exactly that linear walk of a one-dimensional array.
- Transposition swaps which index changes fastest. Wikipedia has a section on transposition because the two orders are transposes of each other in the linear layout.
- The address of `(i, j)` in a row-major `rows × cols` array is `(i * cols + j) * elementSize`, plus the base. Column-major swaps the roles of the two indexes.

**When to use:**

Match the loop nest to the layout. Walk the contiguous index in the inner loop so each step is the next address.

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
</details>

<details>
<summary>How is a Circular Array useful, and how would you implement one?</summary>

### How is a Circular Array useful, and how would you implement one?

**Definition:**

A circular array is a fixed buffer used as a ring. It implements a queue without sliding the live elements forward on every removal.

**How it works:**

- Write at the end index and read at the start index. Advance either with `(index + 1) % capacity`. Wikipedia states that fixed-capacity queue operations are then constant time.
- Two indexes cannot tell empty from full if every slot may be used. Keep a count, or leave one slot unused.
- A full write either fails or overwrites the oldest element. Growth copies the live range, starting at the read index, and that copy is the costly part.
- The same ring is a deque: the start index can move backward modulo the capacity.

**Examples:**

```text
capacity 4, count
enqueue 1, 2 -> [1, 2, _, _]
dequeue -> [1 consumed, 2, _, _]
enqueue 3, 4, 5 -> [5, 2, 3, 4]
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
</details>

<details>
<summary>How to optimize operations in a Very Large Array that has many zero elements (i.e., a Sparse Array).</summary>

### How to optimize operations in a Very Large Array that has many zero elements (i.e., a Sparse Array).

**Definition:**

A sparse array is one in which most elements are zero. Storing every zero wastes the memory the nonzero values do not need.

**How it works:**

- Wikipedia's common criterion is that the number of nonzeros is roughly the number of rows or columns. A matrix with most entries nonzero is dense.
- Store the nonzeros and their coordinates, not a slot per index. Reads of a missing coordinate are zero. Writes of a new nonzero insert a coordinate; writes of zero delete it.
- Dense algorithms on a huge sparse matrix are sometimes infeasible because they allocate and touch the zeros. Special storage is what makes the matrix fit.
- A scan that needs every index, including zeros, gains nothing from the sparse form, because it must invent the missing zeros anyway.

**When to use:**

Use sparse storage when the nonzero count is near the row or column count and the hot operations touch nonzeros. Keep the dense array when most entries are nonzero or you stream every index.

**Sources:**

- [Wikipedia — Sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix)
</details>

<details>
<summary>How would you create and work with a Multi-Dimensional Array?</summary>

### How would you create and work with a Multi-Dimensional Array?

**Definition:**

A multidimensional array is stored in linear memory. Row-major and column-major order decide which index is contiguous.

**How it works:**

- One allocation of `rows * cols` elements, plus the address formula, is the usual layout. In row-major order the address of `(i, j)` is `i * cols + j`. Column-major swaps the indexes.
- An array of row pointers is a different object: each row can move, and a row walk is contiguous only inside that row. The single-block layout keeps the whole matrix one sequence.
- Nested loops should run the contiguous index inside. That is sequential locality: linear access of a one-dimensional array.
- Jagged rows are not a rectangular address formula. Store a length per row, or the column index in the formula is wrong.

**Examples:**

```js
const cols = 3;
const at = (i, j) => i * cols + j;
// values [1, 2, 3, 4, 5, 6] -> row 1 is indexes 3, 4, 5
```

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
</details>

<details>
<summary>Explain the methodology for Resizing a Dynamic Array and its impact on time complexity.</summary>

### Explain the methodology for Resizing a Dynamic Array and its impact on time complexity.

**Definition:**

A dynamic array grows by a large factor, typically doubling, instead of one slot at a time. Most appends are constant time. The append that reallocates is Θ(n).

**How it works:**

- When `size == capacity`, set capacity to twice itself and copy the elements into the new storage. Wikipedia shows that reserved space as the grey cells.
- Inserting n elements takes O(n) time overall, so each insertion is amortized constant time, as long as a given allocation takes O(1).
- Growing by one slot each time copies an ever longer prefix and does not get that amortized bound. The large growth is what avoids resizing on every insert.
- Some arrays also shrink when the size falls below a threshold such as 30% of capacity. A shrink is another Θ(n) copy, so it should not run on every removal.

**When to use:**

Double when appends dominate and occasional Θ(n) copies are acceptable. Do not double without a bound if the next allocation cannot be satisfied; then the insert has to fail or use another structure.

**Sources:**

- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
</details>

### Array Techniques and Optimization

<details>
<summary>Implement Binary Search in a circularly sorted array.</summary>

### Implement Binary Search in a circularly sorted array.

**Definition:**

A circularly sorted array is a sorted array that has been rotated. One side of the middle is still sorted, so binary search can still discard a half.

**How it works:**

- Compare the target with the middle. Also compare the ends of the current range to see which half is ordered: if `a[low] <= a[mid]`, the left half is ordered, otherwise the right half is.
- If the target lies inside the ordered half, search there. Otherwise search the other half. Each step discards one half, which is the same elimination binary search uses. Wikipedia's worst case for that elimination is floor(log2(n)) + 1 iterations.
- Duplicates at the ends can make both halves look unordered. Then the test `a[low] <= a[mid]` does not prove the left half is a clean sorted range, and you cannot discard it.
- An empty range means the target is absent. The rotation point itself is the index where `a[i] > a[i + 1]`, found by the same half-discard.

**Examples:**

```text
[4, 5, 6, 1, 2, 3], target 2
mid is 6, right half 1,2,3 is ordered and contains 2
```

**Sources:**

- [Wikipedia — Binary search algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
</details>

<details>
<summary>Describe an algorithm for Dutch National Flag sorting problem.</summary>

### Describe an algorithm for Dutch National Flag sorting problem.

**Definition:**

The Dutch national flag problem partitions an array into three bands in one pass. Wikipedia's bound is Θ(n) moves and examinations.

**How it works:**

- Keep three indexes: bottom, middle, and top. The middle walks forward. A bottom-group value swaps with the element just above the bottom band, and the bottom grows. A top-group value swaps with the element just below the top band, and the top shrinks. A middle-group value stays and the middle advances.
- The middle does not advance after a swap with the top, because the value that arrived has not been examined.
- At the end the bottom band, the middle band, and the top band are contiguous. For the colors that is red, then white, then blue. For sorting keys it is `< pivot`, then `= pivot`, then `> pivot`.
- The equal band is why this partition helps quicksort on many duplicates: those equals are finished and are not partitioned again.

**Examples:**

```text
[2, 0, 1, 2, 0] with bands 0, 1, 2
-> [0, 0, 1, 2, 2]
```

**Sources:**

- [Wikipedia — Dutch national flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem)
- [Wikipedia — Quickselect](https://en.wikipedia.org/wiki/Quickselect)
</details>

<details>
<summary>How would you multiply two large numbers represented as integer arrays?</summary>

### How would you multiply two large numbers represented as integer arrays?

**Definition:**

Each digit is one array slot, least significant digit at index 0. The product of an a-digit number and a b-digit number has at most a + b digits.

**How it works:**

- Allocate a result array of length `a.length + b.length`, filled with zeros. For each digit `a[i]` and `b[j]`, add `a[i] * b[j]` into `result[i + j]`.
- Then walk the result once and carry: `result[k + 1] += floor(result[k] / 10)` and `result[k] %= 10`. The nested loops perform one multiplication for every pair of digits.
- A leading zero in the result is not a digit of the number. Strip it when you present the product, unless the product is zero.
- The same carry fixes an addition of two such arrays. Do not add the digits in a language integer that cannot hold `9 * 9 * max(a.length, b.length)` plus the carry, or the slot overflows before the carry step.

**Examples:**

```text
99 * 99 as [9, 9] * [9, 9]
slots before carry hold 81, 162, 81
after carry -> [1, 8, 0, 9] which is 9801
```

**Sources:**

- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
</details>

<details>
<summary>Explain how to implement an Efficient Range Sum Query using arrays.</summary>

### Explain how to implement an Efficient Range Sum Query using arrays.

**Definition:**

A prefix-sum array answers a range sum by one subtraction. A segment tree answers it after updates without rewriting the whole prefix.

**How it works:**

- After `S[0] = 0` and `S[k] = a[0] + … + a[k - 1]`, the sum of `a[l..r)` is `S[r] - S[l]`. cp-algorithms states that precomputed prefix sums answer a sum query in O(1), and that changing one array element then costs O(n) prefix updates.
- A segment tree stores the sum of a segment in each node. The standard tree uses 4n vertices for an array of n. Construction is O(n) when the merge is constant time. A sum of `a[l..r)` and an assignment `a[i] = x` are both O(log n).
- A Fenwick tree, described on the prefix-sum page, looks up one prefix and modifies one array value in logarithmic time per operation. A range sum is the difference of two prefixes.
- The plain prefix array is the one to build when the array is fixed. The tree is the one to build when assignments are mixed with queries.

**When to use:**

Use the prefix array for a static array. Use the segment tree or the Fenwick tree when elements change between queries.

**Sources:**

- [cp-algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
- [Wikipedia — Prefix sum](https://en.wikipedia.org/wiki/Prefix_sum)
</details>

<details>
<summary>Describe a Space-Efficient Data Structure for sparse arrays.</summary>

### Describe a Space-Efficient Data Structure for sparse arrays.

**Definition:**

Store the nonzero entries and where they sit. The zeros are implied.

**How it works:**

- A sparse array is one in which most elements are zero. A common criterion is that the nonzero count is roughly the number of rows or columns. A dense matrix is the opposite: most entries are nonzero.
- A coordinate list is an array of triples `(index, value)`, or `(row, column, value)` for a matrix. Its length follows the nonzero count, not the product of the dimensions.
- A read of a missing coordinate returns zero. A write of zero deletes the coordinate. A write of a new nonzero appends one, which on a dynamic array is amortized constant time until you also keep the coordinates sorted for binary search.
- Dense algorithms that allocate every zero are what Wikipedia calls sometimes infeasible for very large sparse matrices.

**When to use:**

Use the coordinate list when nonzeros are few and you look them up. Keep a dense block when you scan every index, because the missing zeros would be rebuilt anyway.

**Sources:**

- [Wikipedia — Sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix)
- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
</details>

### Searching and Sorting Challenges

<details>
<summary>Explain the Quickselect algorithm and its relation to arrays.</summary>

### Explain the Quickselect algorithm and its relation to arrays.

**Definition:**

Quickselect selects one rank from an array in place. It partitions like quicksort and then looks at only the side that contains that rank.

**How it works:**

- Wikipedia: the average case is linear, because each step is linear and the searched set shrinks by a constant fraction on a typical pivot. Consistently bad pivots, such as shrinking by one element, make the worst case quadratic.
- The kth smallest ends up at index k − 1. The kth largest ends up at index n − k. The rest of the array is not fully sorted.
- Median of medians forces a linear worst case. The overhead of that pivot is high, so it is generally not used alone; it can be a fallback.
- The partition is the same in-place array partition quicksort uses. After it, the pivot index tells you which side to discard.

**Examples:**

```text
[7, 2, 5, 1], 2nd smallest
partition until index 1 is the 2nd smallest
```

**Sources:**

- [Wikipedia — Quickselect](https://en.wikipedia.org/wiki/Quickselect)
</details>

<details>
<summary>How can Quicksort be optimized for arrays with many duplicate elements?</summary>

### How can Quicksort be optimized for arrays with many duplicate elements?

**Definition:**

A two-way partition sends values equal to the pivot down both recursive calls. A three-way partition finishes the equals in the middle band.

**How it works:**

- The Dutch national flag partition groups `< pivot`, `= pivot`, and `> pivot` in Θ(n) moves and examinations for that pass.
- Recurse only into the strictly less band and the strictly greater band. The equal band is already in its final place.
- Quickselect's worst case comes from pivots that barely shrink the problem. A long run of duplicates is that bad pivot if equals are not peeled off: each call removes one copy and leaves the rest.
- The three-way split does not change the need for a decent pivot on the unequal parts. It only stops the equal run from being partitioned again.

**When to use:**

Use three-way partition when repeats are common. A two-way partition is enough when keys are almost all distinct, because the equal band would be a single pivot anyway.

**Sources:**

- [Wikipedia — Dutch national flag problem](https://en.wikipedia.org/wiki/Dutch_national_flag_problem)
- [Wikipedia — Quickselect](https://en.wikipedia.org/wiki/Quickselect)
</details>

<details>
<summary>Describe an algorithm to perform Inversion Count in an array.</summary>

### Describe an algorithm to perform Inversion Count in an array.

**Definition:**

An inversion is a pair `i < j` with `a[i] > a[j]`. Merge the array as in mergesort and count those pairs when a right-half value is taken before the left half is exhausted.

**How it works:**

- Split the array, count inversions in each half, then merge the two sorted halves. When the next right value is smaller than the next left value, it forms an inversion with every still-unmerged element of the left half. Add that remaining left count.
- When the next left value is smaller or equal, it forms no new inversion with the remaining right half, because those right values are later indexes and are not smaller.
- Each element of a merge is moved once into the output. The split is the same halving binary search uses, so the merge depth follows that halving. The count is the sum of the cross-half additions.
- A double loop that checks every pair also counts inversions. The merge counts the same pairs while the halves are already sorted, instead of revisiting every pair from scratch.

**Examples:**

```text
[2, 4] and [1, 3]
take 1 -> inversions with 2 and 4
take 2 -> no inversion
take 3 -> inversion with 4
take 4
total 3
```

**Sources:**

- [Wikipedia — Binary search algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
</details>

<details>
<summary>Explain the application of Binary Indexed Trees or Fenwick Trees in the context of arrays.</summary>

### Explain the application of Binary Indexed Trees or Fenwick Trees in the context of arrays.

**Definition:**

A Fenwick tree stores an array so that one prefix sum can be read, and one element can be changed, in logarithmic time per operation.

**How it works:**

- The prefix-sum page states that bound. A range sum is the difference of two prefix sums, so it is two of those lookups.
- A plain prefix array answers the sum in O(1), and cp-algorithms notes that updating one element then rewrites O(n) prefix entries. The Fenwick tree is the structure that avoids that rewrite.
- A segment tree answers the same pair of operations in O(log n), with 4n vertices and an O(n) build. The Fenwick tree is the tighter array form of the prefix problem, not a general segment merge.
- The tree is stored in an array. The index arithmetic walks the bits of the position; the page's claim is the logarithmic lookup and update, not a second data structure beside the array.

**When to use:**

Use a Fenwick tree for prefix sums and point updates on an array. Use a segment tree when the merge is not a sum, or when the query is not a prefix difference.

**Sources:**

- [Wikipedia — Prefix sum](https://en.wikipedia.org/wiki/Prefix_sum)
- [cp-algorithms — Segment Tree](https://cp-algorithms.com/data_structures/segment_tree.html)
</details>

<details>
<summary>Design an algorithm for the Skyline Problem using arrays.</summary>

### Design an algorithm for the Skyline Problem using arrays.

**Definition:**

The skyline is the outline of buildings given as `[left, right, height]` triples. It changes only where a building starts or ends, and only when the tallest active height changes.

**How it works:**

- Turn each building into two events: a start at `left` with its height, and an end at `right`. Sort the events by x. Heapsort does that in O(n log n).
- Scan the events. Active heights live in a binary heap so the current maximum is find-min on a max-heap, which is O(1), and removing the maximum is O(log n) worst case when you already hold that root.
- Finding an arbitrary ended building in a binary heap, with no handle, is O(n). A start is an insert, O(log n) worst case. Record a skyline point only when the maximum height actually changes.
- Events at the same x must apply all starts and ends at that x before you emit a point, or the outline flickers through intermediate heights.

**Examples:**

```text
[1, 3, 2] and [2, 4, 3]
outline heights: 2 at x=1, 3 at x=2, 0 at x=4
```

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Heapsort](https://en.wikipedia.org/wiki/Heapsort)
</details>

### Multidimensional Arrays and Matrices

<details>
<summary>How would you implement an efficient matrix transpose operation?</summary>

### How would you implement an efficient matrix transpose operation?

**Definition:**

Transposition swaps the roles of the two indexes. In a row-major layout that changes which index is contiguous.

**How it works:**

- A square matrix stored as one array transposes in place: for `i < j`, swap `(i, j)` with `(j, i)`. The address is `i * n + j`. Each pair swaps once, so the diagonal is left alone.
- A non-square `rows × cols` matrix becomes `cols × rows`. The in-place swap no longer maps the block onto itself. Write into a second array of the new shape, or follow the cycles of the permutation if a second buffer is forbidden.
- After the transpose, loops that walked a row are walking what used to be a column. Sequential locality follows the new contiguous index.
- An array of row pointers transposes by allocating the new row count and copying. That is not the one-block swap.

**Examples:**

```text
1 2 3
4 5 6
-> 
1 4
2 5
3 6
```

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
</details>

<details>
<summary>What are some efficient methods for performing matrix multiplication using arrays?</summary>

### What are some efficient methods for performing matrix multiplication using arrays?

**Definition:**

The product `C[i][j]` is the dot of row i of A and column j of B. The array layout decides whether that dot walks contiguous memory.

**How it works:**

- Three nested loops compute one multiply-add per triple `(i, j, k)`. For row-major A, B, and C, the inner walk of `k` is contiguous in A and strided in B.
- Sequential locality is a linear walk of a one-dimensional array. Making the inner index the contiguous one, or accumulating into a small block that fits together, is the layout change the locality page is about. The arithmetic result is the same.
- The operand sizes have to agree: A's column count is B's row count. The result has A's row count and B's column count.
- A sparse matrix, most of whose entries are zero, should not run this dense triple loop over the zeros. Store the nonzeros and multiply those.

**When to use:**

Use the dense triple loop when the matrices are dense and you can order the inner index along contiguous addresses. Use the nonzero list when the matrices are sparse.

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
- [Wikipedia — Sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix)
</details>

<details>
<summary>Solve the Sudoku Solver problem using arrays.</summary>

### Solve the Sudoku Solver problem using arrays.

**Definition:**

A Sudoku board is a 9×9 array. A placement is legal when the digit is absent from that row, that column, and that 3×3 box.

**How it works:**

- Store the board in row-major order, address `r * 9 + c`. An empty cell is the next decision.
- Try digits 1 through 9. Reject a digit that already occurs in the row, the column, or the box whose origin is `(floor(r / 3) * 3, floor(c / 3) * 3)`.
- Write the digit and continue to the next empty cell. If no digit fits, erase the digit and try the next candidate. That undo is what makes the search exhaustive.
- A full board with no empty cell is a solution. If the first cell runs out of digits, the puzzle has none under these rules.

**Examples:**

```text
cell (4, 5) box origin (3, 3)
a digit already in row 4, column 5, or that box is illegal
```

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
</details>

<details>
<summary>Flatten a 2D Matrix into a 1D Array without extra space.</summary>

### Flatten a 2D Matrix into a 1D Array without extra space.

**Definition:**

A row-major matrix that already lives in one array is the flat array. Flattening it does not allocate a second buffer.

**How it works:**

- The element at row i, column j is at `i * cols + j`. Reading the storage from index 0 through `rows * cols - 1` is the row-by-row sequence.
- No extra space means you do not allocate another `rows * cols` block and copy. The indexes are the flattening.
- An array of separately allocated rows is not one block. Flattening that without a new buffer is not the address formula; the rows would have to already be adjacent.
- Column-major storage flattens column by column with `j * rows + i`. Using the row-major formula on it reads the wrong neighbor.

**Examples:**

```text
[[1, 2, 3], [4, 5, 6]] stored as [1, 2, 3, 4, 5, 6]
(1, 0) -> 1 * 3 + 0 = 3, value 4
```

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
</details>

<details>
<summary>Implement an algorithm to search in a Row-wise and Column-wise Sorted 2D Array.</summary>

### Implement an algorithm to search in a Row-wise and Column-wise Sorted 2D Array.

**Definition:**

Each row increases to the right, and each column increases downward. From the top-right corner one comparison discards a row or a column.

**How it works:**

- Start at row 0, column `cols - 1`. If that value equals the target, stop. If it is greater than the target, the rest of the row is larger, so move left. If it is smaller, the rest of the column is smaller, so move down.
- Each move drops one row or one column. The walk stops when the target is found or the indexes leave the matrix.
- The matrix is stored row-major, so moving left is the previous address and moving down jumps by `cols`.
- This is not binary search on the whole matrix. A row is sorted, so a row can be binary-searched, but the staircase above uses the column order as well and does not assume the row starts are globally sorted against each other beyond the column increases.

**Examples:**

```text
1  4  7
2  5  8
3  6  9
target 6 starts at 7, moves left to 4, down to 5, down to 6
```

**Sources:**

- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Binary search algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
</details>

### Specific Data Structures Using Arrays

<details>
<summary>Explain how a Priority Queue can be implemented using an array.</summary>

### Explain how a Priority Queue can be implemented using an array.

**Definition:**

A binary heap is a priority queue stored in an array. The parent of index i is at floor((i − 1) / 2), and the children are at 2i + 1 and 2i + 2.

**How it works:**

- In a min-heap every parent is less than or equal to its children. Find-min reads index 0 and is O(1). Insert sifts up and is O(log n) worst case, O(1) on average. Delete-min sifts the last element down from the root and is O(log n) worst case.
- The shape stays complete: the array is filled from the left, so there are no holes. Building that heap from an unsorted array is O(n).
- Finding an arbitrary element, not the minimum, is O(n). Decrease-key is O(log n) worst case when the index is already known.
- The array length is the heap size. A dynamic array underneath grows by doubling when an insert passes the capacity; that growth is a separate Θ(n) copy.

**When to use:**

Use the array heap when you need the minimum or maximum next and you have random access. It is the wrong structure when you need the middle element by rank without extracting.

**Sources:**

- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
</details>

<details>
<summary>How can we use arrays to implement a Suffix Array and what is its usefulness?</summary>

### How can we use arrays to implement a Suffix Array and what is its usefulness?

**Definition:**

A suffix array is the array of starting indexes of a string, sorted by the suffix that starts at each index.

**How it works:**

- For a string of length n the array holds a permutation of `0..n-1`. Comparing two suffixes is a character scan from those indexes. Sorting those indexes is what builds the array; heapsort on n indexes is O(n log n) comparisons, and each comparison may scan characters.
- Once the array is sorted, a pattern is a binary search over it. Wikipedia's binary search worst case is floor(log2(n)) + 1 comparisons. Here each comparison is a string comparison of the pattern with the suffix at that index.
- A suffix tree is the related structure. Wikipedia states that the usual suffix-tree constructions are linear-time for a constant-size alphabet. The array is the sorted list of the same suffixes without the tree nodes.
- The array is useful when you need the suffixes in sorted order: the leftmost and rightmost indexes whose suffix starts with the pattern are the match range.

**Examples:**

```text
"banana" suffixes, sorted starts
1 a, 3 ana, 5 a, 0 banana, 2 nana, 4 na
a pattern "na" falls between the na/nana entries
```

**Sources:**

- [Wikipedia — Heapsort](https://en.wikipedia.org/wiki/Heapsort)
- [Wikipedia — Binary search algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
- [Wikipedia — Suffix tree](https://en.wikipedia.org/wiki/Suffix_tree)
</details>

<details>
<summary>Design a Double-ended Queue (Deque) using arrays.</summary>

### Design a Double-ended Queue (Deque) using arrays.

**Definition:**

A deque on an array is a circular buffer whose start index can move either way. Both ends are constant-time index updates at a fixed capacity.

**How it works:**

- Push back writes at `end` and advances it modulo the capacity. Push front retreats `start` by one modulo the capacity, then writes. Pop back and pop front undo those moves.
- Wikipedia's circular queue is constant time per operation at fixed capacity, and a deque is that buffer used at both ends.
- Empty and full still cannot be told from the two indexes alone if every slot is usable. Keep a count, or leave one slot empty.
- Growth copies the live elements in order from `start` into a larger array. That copy is the costly expansion.

**Examples:**

```js
function pushFront(q, x) {
  if (q.count === q.cap) throw new Error("full");
  q.start = (q.start - 1 + q.cap) % q.cap;
  q.data[q.start] = x;
  q.count++;
}
```

**Sources:**

- [Wikipedia — Circular buffer](https://en.wikipedia.org/wiki/Circular_buffer)
- [Wikipedia — Double-ended queue](https://en.wikipedia.org/wiki/Double-ended_queue)
</details>

<details>
<summary>Provide the implementation of a Trie data structure using an array-based representation.</summary>

### Provide the implementation of a Trie data structure using an array-based representation.

**Definition:**

A trie stores strings so that a node's place in the tree is the key, and each edge is one character. An array-based node stores one child slot per character in the alphabet.

**How it works:**

- The root is the empty string. The child for character c is `children[c - base]`. A missing child is an empty slot, not a hashed bucket.
- Insert walks the characters and creates a node when the slot is empty. Search fails at the first empty slot. A terminal mark on the node distinguishes a stored word from a proper prefix.
- Wikipedia notes that a basic trie can be memory-intensive. An array of alphabet-sized slots spends that array even when most children are absent. Compression is the optimization the page points at.
- Lookup follows the key length, one slot per character. It does not scan the other stored keys, and it has no hash collision, which is the advantage Wikipedia gives over a hash table.

**Examples:**

```text
alphabet a..z, insert "cat"
root['c'] -> ['a'] -> ['t' + terminal]
search "car" dies at the missing 'r' slot
```

**Sources:**

- [Wikipedia — Trie](https://en.wikipedia.org/wiki/Trie)
</details>

### Advanced Algorithmic Problems

<details>
<summary>Develop an algorithm to solve the Kadane’s Algorithm (Maximum Subarray Problem).</summary>

### Develop an algorithm to solve the Kadane’s Algorithm (Maximum Subarray Problem).

**Definition:**

Kadane's algorithm finds a contiguous subarray of maximum sum in one pass. Wikipedia states that this is an O(n)-time algorithm, and that any algorithm must read the array, so that bound is as fast as possible.

**How it works:**

- For the version that does not allow an empty subarray, keep `best` and `current`. For each x, set `current = max(x, current + x)` and `best = max(best, current)`. The page's example [−2, 1, −3, 4, −1, 2, 1, −5, 4] has maximum sum 6 on [4, −1, 2, 1].
- The version that allows an empty subarray starts `best` at 0 and uses `current = max(0, current + x)`. The answer is then 0 when every element is non-positive.
- The implementation stores those two running values, not a table of every subarray. If the array is empty, the nonempty version has no answer.
- All-nonnegative input has the whole array as a maximum subarray. All-nonpositive input, when the empty subarray is forbidden, has a single element, the largest one.

**Examples:**

```js
function maxSubarray(a) {
  let best = -Infinity;
  let current = 0;
  for (const x of a) {
    current = Math.max(x, current + x);
    best = Math.max(best, current);
  }
  return best;
}
```

**Sources:**

- [Wikipedia — Maximum subarray problem](https://en.wikipedia.org/wiki/Maximum_subarray_problem)
</details>

<details>
<summary>Solve the Minimum Jumps to Reach End problem with arrays.</summary>

### Solve the Minimum Jumps to Reach End problem with arrays.

**Definition:**

Each index i of the array allows a jump of length at most `a[i]`. The greedy scan keeps the farthest index reachable so far.

**How it works:**

- GeeksforGeeks lists recursion as O(n^n) time and O(n) space, bottom-up dynamic programming as O(n^2) time and O(n) space, and the greedy approach as O(n) time and O(1) space.
- The greedy pass tracks the end of the current jump range and the farthest index any position in that range can reach. When the scan hits the end of the range, one jump is spent and the range extends to that farthest index.
- If the farthest index never moves past the current position, the end is unreachable.
- The DP table `jumps[i]` is the fewest jumps that reach i, filled from the jumps that can land on it. That is the O(n^2) fill: each pair of indexes is a candidate jump.

**Examples:**

```text
[2, 3, 1, 1, 4]
from 0 the range covers index 2
from there index 4 is inside the next range
two jumps
```

**Sources:**

- [GeeksforGeeks — Minimum jumps to reach end](https://www.geeksforgeeks.org/dsa/minimum-number-jumps-reach-endset-2on-solution/)
</details>

<details>
<summary>Design a data structure that supports adding new elements, removing specific elements, and finding random elements efficiently.</summary>

### Design a data structure that supports adding new elements, removing specific elements, and finding random elements efficiently.

**Definition:**

Keep the values in a dynamic array so a random element is one index, and remember each value's index so a removal does not scan.

**How it works:**

- Insert appends. Wikipedia: inserting n elements into a dynamic array is O(n) overall, so each append is amortized constant time, while the doubling copy is Θ(n).
- A random element is `data[floor(random() * data.length)]`. That read is the array's random access, not a walk.
- Removal of a known index swaps that slot with the last slot and pops the last. The swapped value's stored index is updated. The array stays dense, so the random index stays uniform over the live elements.
- The side table maps a value to its index. Without it, finding the value to remove walks the array, and Wikipedia's linked-list access is the reminder that a chain would make that walk linear in the number of nodes. The table is what avoids the walk; this note does not assign the table a separate big-O beyond the array bounds above.

**Examples:**

```text
data [a, b, c], index of b is 1
remove b -> swap with c -> [a, c], pop not needed if b was swapped to the end and popped
```

**Sources:**

- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
- [Wikipedia — Linked list](https://en.wikipedia.org/wiki/Linked_list)
</details>

<details>
<summary>Solve the 0/1 Knapsack Problem using a dynamic programming approach based on arrays.</summary>

### Solve the 0/1 Knapsack Problem using a dynamic programming approach based on arrays.

**Definition:**

Each item is taken once or not at all. The array `m[i][w]` is the best value using the first i items with capacity w.

**How it works:**

- Wikipedia states the dynamic-programming solution runs in O(nW). W is the capacity, not a polynomial in the number of items, which is why the bound does not contradict NP-completeness.
- The recurrence uses the previous row: skip item i, or take it when `w` is at least its weight and add its value. Items are not reused, so the previous row is the one that matters.
- If only the final value `m[n][W]` is required, the page says the memory can be reduced to O(W) by keeping the recent two rows.
- Dividing all weights and W by their greatest common divisor shortens W and improves that running time. The answer is the value, or the set of choices if you also store the take-or-skip bit.

**Examples:**

```text
weights [1, 2], values [1, 3], W = 2
best is the second item alone, value 3
```

**Sources:**

- [Wikipedia — Knapsack problem](https://en.wikipedia.org/wiki/Knapsack_problem)
</details>

<details>
<summary>Develop an algorithm to find the Median of Two Sorted Arrays of different sizes.</summary>

### Develop an algorithm to find the Median of Two Sorted Arrays of different sizes.

**Definition:**

The median is the middle of the merged sorted sequence. Binary search finds the cut without merging the arrays.

**How it works:**

- GeeksforGeeks lists sorting everything as O((n + m) log(n + m)) time and O(n + m) space, the merge of mergesort as O(m + n) time and O(1) space, and binary search as O(log min(n, m)) time and O(1) space.
- Search on the smaller array for how many elements belong to the left half of the merged sequence. The cut is correct when the left side's max is less than or equal to the right side's min, on both arrays.
- If the total length is odd, the median is the max of the left half. If it is even, it is the average of the max of the left and the min of the right. The page states both cases.
- The merge walks both arrays like a mergesort combine and stops at the middle. It is the O(m + n) option when the log bound is unnecessary.

**Examples:**

```text
[1, 3] and [2]
merged middle is 2
```

**Sources:**

- [GeeksforGeeks — Median of two sorted arrays](https://www.geeksforgeeks.org/dsa/median-of-two-sorted-arrays-of-different-sizes/)
</details>

### Performance and Memory Considerations

<details>
<summary>Provide strategies for handling Integer Overflow while working with numeric arrays.</summary>

### Provide strategies for handling Integer Overflow while working with numeric arrays.

**Definition:**

An array index or a running total overflows when the sum of two in-range values is outside the type. Test the operation before you perform it, or use a wider accumulator.

**How it works:**

- Binary search's midpoint is the addition that overflows: `low + high` can exceed the index type even though both ends are inside the array. Compute `low + (high - low) / 2` so the addition stays inside the range the algorithm already knows.
- A prefix sum of n values can overflow a slot that fits one value. Keep the prefix in a wider type. Kadane's running sum has the same issue: `current + x` must use a type that can hold the sum, not only one element.
- Digit arrays avoid a fixed integer width. Each slot is one digit and the carry walk writes `floor(slot / 10)` into the next slot, so the value is not required to fit in one machine word.
- A dynamic array's length check is the same pattern for sizes: compare with capacity before the index is used, and treat a failed growth as a failed insert rather than wrapping the length.

**When to use:**

Widen the accumulator when the sum is the result. Use the subtraction form when you only need a midpoint. Use digit arrays when the integer must grow past every fixed width.

**Sources:**

- [Wikipedia — Binary search algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
- [Wikipedia — Maximum subarray problem](https://en.wikipedia.org/wiki/Maximum_subarray_problem)
- [Wikipedia — Dynamic array](https://en.wikipedia.org/wiki/Dynamic_array)
</details>

<details>
<summary>What are the implications of Data Caching and Locality of Reference in array operations?</summary>

### What are the implications of Data Caching and Locality of Reference in array operations?

**Definition:**

Arrays help caches when the next element is the next address. A pointer chase does not.

**How it works:**

- Spatial locality is use of data in nearby locations. Sequential locality, a special case, is linear access such as walking a one-dimensional array. Temporal locality is reuse of the same data within a short time.
- Systems with strong locality are the ones caching and prefetching pay off for. An inner loop that walks a row-major row is that linear access. An inner loop that strides down a column jumps by the row length and leaves most of each fetched line unused.
- A binary heap in an array keeps parent and children a constant index calculation apart, but a sift still jumps toward the root or the leaves. The packed array is still one block, unlike nodes allocated one by one.
- The algorithmic bounds, such as a circular buffer's constant-time index update, do not include a cache miss. Locality changes the cost of the memory access inside those steps.

**When to use:**

Walk the contiguous index in the inner loop. Pack hot data into one array when the alternative is a node per element.

**Sources:**

- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Binary heap](https://en.wikipedia.org/wiki/Binary_heap)
</details>

<details>
<summary>How to minimize Page Faults while accessing large arrays in memory-constrained environments?</summary>

### How to minimize Page Faults while accessing large arrays in memory-constrained environments?

**Definition:**

A page fault is the exception the MMU raises when a process touches a page that is not prepared. The contents may have to be loaded from a backing store such as a disk.

**How it works:**

- A sequential walk of an array faults when it enters a page and then uses the rest of that page. Sequential locality is this linear access. A random index into a huge array can fault on almost every access, because each access may be a different page.
- Match the loop to row-major or column-major order so the inner index is contiguous. Striding by a large column step touches a new page more often for the same number of elements.
- A sparse array avoids allocating, and therefore faulting, the pages that would have held only zeros. That helps only when the algorithm does not need those zeros materialized.
- Touching more of the array than the working set forces the kernel to keep loading pages. Process the array in blocks that stay mapped, and do not keep every row live if the next step only needs one band.

**When to use:**

Scan in storage order. Keep the live band small. Store only nonzeros when the zeros are not part of the computation.

**Sources:**

- [Wikipedia — Page fault](https://en.wikipedia.org/wiki/Page_fault)
- [Wikipedia — Locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference)
- [Wikipedia — Row- and column-major order](https://en.wikipedia.org/wiki/Row-_and_column-major_order)
- [Wikipedia — Sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix)
</details>
