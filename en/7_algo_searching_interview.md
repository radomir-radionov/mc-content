# Searching Algorithms — Interview

Answers for this topic are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Searching Algorithms — Interview](./7_algo_searching_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Searching Algorithms](https://devinterview.io/questions/data-structures-and-algorithms/searching-algorithms-interview-questions/).

### Basics of Searching

<details>
<summary>What is Linear Search (Sequential Search)?</summary>

### What is Linear Search (Sequential Search)?

**Definition:**

Linear search walks the collection from one end and compares each element with the target until it finds a match or runs out of elements.

**How it works:**

- Start at the first index. If it equals the target, return that index. Otherwise step forward.
- Worst case and the missing-target case look at all `n` elements, so time is Θ(n). Extra memory is Θ(1).
- The data need not be sorted. That is the reason to use it on a short list, a one-off scan, or a collection that changes so often that keeping it sorted would cost more than the scans.
- It needs no extra index. A database query with no usable index falls back to the same full scan.
- On a tiny array the constant factors can beat binary search, because binary search still has to be allowed to assume sorted order.

**Examples:**

```text
for i in 0..n-1:
  if a[i] == target: return i
return -1
```
</details>

<details>
<summary>Explain what is Binary Search.</summary>

### Explain what is Binary Search.

**Definition:**

Binary search finds a key in a sorted random-access array by comparing it with the middle element and discarding the half that cannot contain it.

**How it works:**

- Set `lo` and `hi` to the ends of the range. While the range is non-empty, set `mid` to the middle.
- If `a[mid]` is the target, return `mid`. If the target is larger, continue on the right. If it is smaller, continue on the left.
- Each step halves the range, so the worst case is Θ(log n) comparisons. The iterative form uses Θ(1) extra memory.
- The recursive form is the same logic with Θ(log n) stack frames.
- It requires sorted order and O(1) access to the middle. A linked list has the halving idea and loses it, because reaching the middle is already linear.
- Typical uses are a sorted index, `git bisect` over a history, and a search for a boundary in a monotone predicate.

**Examples:**

```text
while lo <= hi:
  mid = lo + (hi - lo) // 2
  if a[mid] == target: return mid
  if a[mid] < target: lo = mid + 1
  else: hi = mid - 1
```
</details>

<details>
<summary>Compare Binary Search vs. Linear Search.</summary>

### Compare Binary Search vs. Linear Search.

**Definition:**

Linear search checks elements in order and works on unsorted data. Binary search discards half the range each time and requires a sorted random-access array.

**How it works:**

- Linear search accepts sorted or unsorted input and any sequence you can walk. Binary search needs sorted order and O(1) indexing.
- Linear search compares the target with each element. Binary search compares it with the middle and keeps one side.
- Time: linear search is Θ(n). Binary search is Θ(log n) comparisons.
- Extra memory: both iterative forms are Θ(1). Recursive binary search is Θ(log n) stack.
- Prefer the scan when the collection is small, unsorted, or updated so often that sorting is wasted. Prefer binary search when the data is large, sorted, and queried many times.
- If the structure is a tree or a hash table, neither array algorithm is the one you want. Use the structure’s own lookup.

**Examples:**

Finding one name in an unsorted list of 50 is a scan. Finding it in a sorted array of a million is binary search, about 20 probes.
</details>

<details>
<summary>What characteristics of the data determine the choice of a searching algorithm?</summary>

### What characteristics of the data determine the choice of a searching algorithm?

**Definition:**

The search you pick follows the size, the order, how often the data changes, and whether a middle element is cheap to read.

**How it works:**

- Small or unsorted: linear search. Large and sorted, with random access: binary search.
- Duplicate keys change which binary-search variant you want. A plain search may return any match. Finding the first or last match continues after a hit.
- The physical layout matters. An array supports the middle probe. A linked list does not. A tree or a graph needs its own traversal.
- If the data is updated between queries, maintaining a sorted order or an index has a cost. A single query on unsorted data should not pay for a full sort.
- Many queries on a stable collection justify sorting once, building a hash table, or building an index.
- Exact match, “first index at least x”, and approximate match are different problems. Fuzzy match is not binary search.
- A latency ceiling favors a bound you can predict, such as Θ(log n), over an average that has a long tail. Tight memory favors a scan or an iterative binary search over a large index.

**Examples:**

A log file you scan once stays linear. A dictionary queried on every keystroke is sorted or hashed first.
</details>

### Linear Search Variants and Complexity

<details>
<summary>Name some Optimization Techniques for Linear Search.</summary>

### Name some Optimization Techniques for Linear Search.

**Definition:**

A linear scan is still Θ(n) in the worst case. The optimizations cut the average cost when searches repeat, the array is partly ordered, or several cores are free.

**How it works:**

- Return as soon as the target is found. The worst case is unchanged; the hit in the middle no longer scans the tail.
- Two pointers walking inward check two ends per iteration. That is still Θ(n) work and can finish sooner when the target is near either end.
- On a sorted array, jump search inspects every √n-th element, then scans one block. That is no longer plain linear search; it is the sorted-data shortcut.
- Move-to-front or transposition slides a found item toward the start so the next search for it is shorter. Move-to-end does the opposite for rare items. These pay off only when the same keys repeat.
- An index maps a key to its position and turns later lookups into O(1) or O(log n), at the cost of building and maintaining the index.
- Split the array across cores and scan the chunks in parallel. The worst-case work is still Θ(n) in total; the wall time drops with the number of cores until memory bandwidth runs out.

**Examples:**

After finding `"error"` at index 900, move-to-front swaps it toward index 0. The next search for `"error"` stops at the first slot.
</details>

<details>
<summary>What is Sentinel Search?</summary>

### What is Sentinel Search?

**Definition:**

Sentinel search is linear search with the target planted at the end of the range, so the loop can stop on a match and does not also test the index against the length.

**How it works:**

- Write the target into `a[n]` (the array must have a spare slot, or you append and later restore).
- Scan from the start with a single test: `while a[i] != target: i += 1`.
- If `i` landed on the spare slot, the target was absent. Otherwise it was found at `i`.
- Time stays Θ(n). Extra memory is one slot, Θ(1).
- A second, different idea often bundled with the name is move-to-front: after a real hit, shift that element one step toward the front so repeated searches get cheaper. That part changes the array’s order. The sentinel itself only removes the bounds check.
- The loop is branch-lighter. Modern compilers often hoist or eliminate the bounds check anyway, so the win can be small.

**Examples:**

```text
a[n] = target
i = 0
while a[i] != target: i += 1
return -1 if i == n else i
```
</details>

<details>
<summary>What are the Drawbacks of Sentinel Search?</summary>

### What are the Drawbacks of Sentinel Search?

**Definition:**

The sentinel removes a bounds check by writing into the collection. That write is the drawback: the algorithm is no longer a pure read.

**How it works:**

- A shared or read-only buffer cannot accept the planted value. The write races with another reader or faults on a constant array.
- Callers who need the original tail preserved must save and restore `a[n]`. Forgetting the restore corrupts the data.
- A fixed array with no spare slot cannot grow. You need a dynamic array, a list, or an extra cell allocated up front.
- Compilers already delete many redundant bounds checks, so the simpler loop with an index test can be just as fast and easier to read.
- If the planted slot is a real element you keep overwriting, you did not reserve a sentinel; you destroyed data.
- The complexity of the extra write is rarely worth it next to an ordinary scan, unless you are in a tight inner loop where the measurement says so.

**Examples:**

Searching a `const` buffer of sensor samples cannot store the target past the last sample. Use the index test. The sentinel version would be undefined on that buffer.
</details>

<details>
<summary>How does the presence of duplicates affect the performance of Linear Search?</summary>

### How does the presence of duplicates affect the performance of Linear Search?

**Definition:**

Duplicates do not change the worst-case cost of a scan that stops at the first match. They change how much of the array you must read if you need every match, or the earliest one in a sense other than “first from the left”.

**How it works:**

- Stop-at-first-hit is still Θ(1) best case and Θ(n) worst case. A duplicate near the start returns immediately. A missing key, or the only copy at the end, still scans everything.
- Collecting every index that matches is always Θ(n), because a later duplicate is invisible until you reach it.
- Extra memory stays Θ(1) for a single index, and Θ(k) if you store k matches.
- The scan does not get a shortcut from “there are many copies” unless you also reorder the array, as move-to-front does.
- If the array is sorted, duplicates are a reason to switch to a lower-bound binary search, which finds the first match in Θ(log n) and can then walk the equal run.

**Examples:**

`[1, 4, 4, 4, 9]`, target `4`. A left-to-right scan returns index 1 after two comparisons. Listing all three 4s still reads the whole middle run, and proving there is no fourth 4 reads to the end or to the first non-4.
</details>

### Advanced Linear Search Techniques

<details>
<summary>Implement an Order-Agnostic Linear Search that works on sorted and unsorted arrays.</summary>

### Implement an Order-Agnostic Linear Search that works on sorted and unsorted arrays.

**Definition:**

Order-agnostic here means the scan does not assume ascending or descending order. It compares every element with the target, so a sorted array and a shuffled array use the same loop.

**How it works:**

- Walk from index 0 through `n - 1`.
- On equality, return the index.
- If the loop ends, return a miss, conventionally `-1`.
- Do not return early because a key became larger than the target. That shortcut is only valid for a sorted ascending array, and this routine is required to work when the array is not sorted.
- Time is Θ(n) whether or not the array is ordered. Extra memory is Θ(1).
- If you know the array is sorted in one of the two directions, binary search is the better algorithm. This routine is the one you call when you do not know.

**Examples:**

```text
def find(a, target):
  for i, value in enumerate(a):
    if value == target: return i
  return -1
```

`find([3, 1, 2], 2)` and `find([1, 2, 3], 2)` both return an index. Neither loop inspects order.
</details>

<details>
<summary>Modify Linear Search to perform on a multi-dimensional array.</summary>

### Modify Linear Search to perform on a multi-dimensional array.

**Definition:**

Treat the multi-dimensional array as a row-major sequence: scan each row from left to right, then the next row. The comparisons are the same as a flat scan of every cell.

**How it works:**

- For a matrix, the outer loop is the row and the inner loop is the column. Return the pair `(r, c)` on a match.
- Time is Θ(N) where N is the number of cells, not the length of one side. A `r` by `c` matrix costs Θ(r · c).
- Extra memory is Θ(1) for the two indexes.
- The same nesting works for higher rank: one loop per dimension, and the innermost loop compares the cell.
- If each row is itself sorted, you can binary-search a row or start from a staircase (top-right of a matrix sorted by rows and columns). That is no longer linear search. The plain nested loops do not need those sorted rows.

**Examples:**

```text
for r in rows:
  for c in cols:
    if m[r][c] == target: return (r, c)
return None
```

In `[[2, 5, 8], [3, 6, 9]]` the target `6` is found at row 1, column 1, after the first row has been scanned.
</details>

### Binary Search In-depth

<details>
<summary>Explain why complexity of Binary Search is O(log n).</summary>

### Explain why complexity of Binary Search is O(log n).

**Definition:**

Each comparison throws away half of the remaining elements. You can do that only about log₂ n times before a single element is left.

**How it works:**

- Start with n candidates. After one probe, at most ⌊n/2⌋ remain. After k probes, at most n / 2^k remain.
- The search stops when the range is empty or one element is checked, so n / 2^k is about 1, and k is about log₂ n.
- The recurrence is `T(n) = T(n/2) + Θ(1)`, which solves to Θ(log n). The O(log n) upper bound and the Ω(log n) unsuccessful search match, so the worst case is Θ(log n).
- The base of the log changes the constant, not the class. log₁₀ n and log₂ n differ by a constant factor.
- This counting assumes the middle element is obtained in constant time. If reaching it costs linear time, the total is no longer logarithmic.

**Examples:**

1,024 sorted keys: 2^10 = 1024, so an unsuccessful search does at most about 10 probes, not 1,024.
</details>

<details>
<summary>Compare Recursive vs. Iterative Binary Search.</summary>

### Compare Recursive vs. Iterative Binary Search.

**Definition:**

Both forms halve the same range and do Θ(log n) comparisons. They differ in the stack and in how obvious the loop is.

**How it works:**

- Iterative search keeps `lo` and `hi` in local variables and repeats the middle test in a loop. Extra memory is Θ(1).
- Recursive search passes the new `lo` and `hi` to a call. Extra memory is Θ(log n) frames, unless a compiler turns the tail call into a jump. That optimization is not portable.
- The recursive form is short and matches the “search the half” sentence. The loop is what most libraries ship, because the stack bound is obvious and there is no call overhead.
- A bug in either form is usually the same: an infinite range from rounding `mid` the wrong way, or a `lo`/`hi` update that does not exclude `mid`.
- Pick the loop unless the surrounding algorithm is already recursive and the depth is known to be small.

**Examples:**

On a million keys both do about 20 comparisons. The recursive version also has about 20 frames live at the deepest call. The loop has one frame.
</details>

<details>
<summary>In Binary Search, why Round Down the midpoint instead of Rounding Up?</summary>

### In Binary Search, why Round Down the midpoint instead of Rounding Up?

**Definition:**

The midpoint is computed with integer division, which rounds down. What keeps the search finite is excluding `mid` from the next range, not the direction of the rounding by itself.

**How it works:**

- `mid = lo + (hi - lo) // 2` is the lower of the two central indexes when the width is even. For an odd count there is a single center.
- The next range must be `lo .. mid-1` or `mid+1 .. hi`. If a branch sets `hi = mid` or `lo = mid` when `a[mid]` is not the target, the range never shrinks and the loop never ends.
- Rounding down is the standard because the formula is a single division and, paired with excluding `mid`, every iteration reduces the number of candidates.
- Rounding up (`mid = lo + (hi - lo + 1) // 2`) is also correct if the updates still exclude that `mid`. It is the form some lower-bound loops use so the window shrinks from the other side.
- Adding `lo + hi` before the division can overflow a fixed-width integer when both ends are large. The `(hi - lo) // 2` form stays inside the element type.

**Examples:**

`lo = 0`, `hi = 1`, target greater than `a[0]`. Floor midpoint is 0. The next `lo` must become 1. If the code set `lo = mid` without the `+ 1`, it would test index 0 forever.
</details>

<details>
<summary>Write a Binary Search algorithm that finds the first occurrence of a given value.</summary>

### Write a Binary Search algorithm that finds the first occurrence of a given value.

**Definition:**

Finding the first match means continuing into the left half after a hit, and remembering the last index where the key was equal to the target.

**How it works:**

- Keep the usual `lo` and `hi`, plus `ans = -1`.
- If `a[mid]` is less than the target, the first match is to the right: `lo = mid + 1`.
- If `a[mid]` is greater or equal, a match at `mid` might still have an earlier twin: record `mid` when equal, and set `hi = mid - 1`.
- When the range empties, `ans` is the leftmost match, or `-1` if there was none.
- Time stays Θ(log n). Extra memory is Θ(1). You do not stop at the first hit you see, because that hit may be in the middle of the equal run.
- The same loop with the inequality flipped finds the last match.

**Examples:**

```text
ans = -1
while lo <= hi:
  mid = lo + (hi - lo) // 2
  if a[mid] >= target:
    if a[mid] == target: ans = mid
    hi = mid - 1
  else:
    lo = mid + 1
```

In `[1, 2, 2, 2, 3]` the first `2` is index 1, even if an early probe lands on index 2 or 3.
</details>

### Binary Search Applications and Variants

<details>
<summary>How would you apply Binary Search to an array of objects sorted by a specific key?</summary>

### How would you apply Binary Search to an array of objects sorted by a specific key?

**Definition:**

Compare the target with the key of the middle object, not with the object as a whole. The array must be ordered by that same key.

**How it works:**

- `lo` and `hi` still bound indexes. `mid` is the middle index. Read `a[mid].key`.
- Equal keys: return `mid`, or keep searching left if you need the first object with that key.
- A smaller middle key means the target is to the right. A larger middle key means it is to the left.
- The loop ends when the range is empty. Return a miss sentinel.
- Time is Θ(log n) key comparisons, assuming a key comparison is O(1). A key that is a long string makes each comparison cost the string length; say so instead of hiding it inside O(1).
- Extra memory is Θ(1) for the iterative form.
- If the array is sorted by a different field than the one you query, binary search on that field is wrong. Scan, or keep a second index.

**Examples:**

```text
# users sorted by id
while lo <= hi:
  mid = lo + (hi - lo) // 2
  if users[mid].id == id: return users[mid]
  if users[mid].id < id: lo = mid + 1
  else: hi = mid - 1
```
</details>

<details>
<summary>Perform a Binary Search in a rotated sorted array.</summary>

### Perform a Binary Search in a rotated sorted array.

**Definition:**

A rotated sorted array is a sorted sequence cut at one pivot and swapped. With distinct values, one binary-search step still discards half the range, because one side of the midpoint is always sorted.

**How it works:**

- Compare `a[mid]` with the target. If it matches, return `mid`.
- If `a[lo] <= a[mid]`, the left half is sorted. If the target sits in `[a[lo], a[mid])`, set `hi = mid - 1`. Otherwise set `lo = mid + 1`.
- If the left half is not sorted, the right half is. Keep the right half only when the target sits in `(a[mid], a[hi]]`.
- Distinct keys: time Θ(log n), extra memory Θ(1). Duplicates can force a linear scan, because equal ends no longer tell you which half is sorted.
- Finding the pivot first, then binary-searching one sorted side, is the same bound.
**Examples:**

```text
while lo <= hi:
  mid = lo + (hi - lo) // 2
  if a[mid] == target: return mid
  if a[lo] <= a[mid]:
    if a[lo] <= target < a[mid]: hi = mid - 1
    else: lo = mid + 1
  else:
    if a[mid] < target <= a[hi]: lo = mid + 1
    else: hi = mid - 1
```

In `[4, 5, 6, 1, 2, 3]`, a probe at `6` sees a sorted left half. Target `2` is not in it, so the search continues on `[1, 2, 3]`.

**Sources:**

- [Search in Rotated Sorted Array — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/search-an-element-in-a-sorted-and-pivoted-array/)
</details>

<details>
<summary>Provide an algorithm for finding an element in a virtually infinite sorted array.</summary>

### Provide an algorithm for finding an element in a virtually infinite sorted array.

**Definition:**

Without a known length you cannot start binary search, because there is no `hi`. Double a window until it passes the target, then binary-search inside that window.

**How it works:**

- Set `lo = 0` and `hi = 1`. While `a[hi] < target`, set `lo = hi` and `hi = 2 * hi`.
- The window doubles, so the bound lands after O(log p) probes, where `p` is the index of the key.
- Binary-search `[lo, hi]`. That is another O(log p). Together the time is O(log p).
- The iterative binary search uses Θ(1) extra memory. The recursive form uses O(log p) stack frames.
- On a real finite array, clamp `hi` to the last index before the probe. An infinite array has no such end.
**Examples:**

```text
lo, hi = 0, 1
while a[hi] < target:
  lo = hi
  hi = 2 * hi
binary_search(a, lo, hi, target)
```

**Sources:**

- [Find position of an element in a sorted array of infinite numbers — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/find-position-element-sorted-array-infinite-numbers/)
</details>

### Interpolation, Jump, and Exponential Searches

<details>
<summary>What is Interpolation Search?</summary>

### What is Interpolation Search?

**Definition:**

Interpolation search probes a sorted array at the index where a uniform distribution would place the key, instead of always probing the middle.

**How it works:**

- `pos = lo + (target - a[lo]) * (hi - lo) / (a[hi] - a[lo])`, using a real division so the index is not truncated too early.
- Equal means found. A smaller `a[pos]` continues on the right. A larger one continues on the left. Stop when the range is empty or the target falls outside `a[lo]..a[hi]`.
- Average time on uniformly distributed sorted data is O(log log n). Worst case is O(n), when values clump and the probe barely moves.
- Extra memory is O(1) for the iterative form.
- It needs sorted numeric keys and a non-zero gap `a[hi] - a[lo]`.
**Examples:**

```text
while lo <= hi and a[lo] <= target <= a[hi]:
  if a[hi] == a[lo]:
    return lo if a[lo] == target else -1
  pos = lo + (target - a[lo]) * (hi - lo) / (a[hi] - a[lo])
  if a[pos] == target: return pos
  if a[pos] < target: lo = pos + 1
  else: hi = pos - 1
```

**Sources:**

- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
</details>

<details>
<summary>Provide an example where Interpolation Search is less efficient than Binary Search.</summary>

### Provide an example where Interpolation Search is less efficient than Binary Search.

**Definition:**

Interpolation search is slower than binary search when the sorted values are far from uniform. The probe then jumps to an end and the range shrinks by one element, which is a linear scan.

**How it works:**

- Worst-case time is O(n). Binary search stays Θ(log n) on any sorted array with O(1) access.
- A classic bad input is exponential growth: `[1, 2, 4, 8, …, 2^(n-1)]` while looking for a small key. The formula keeps estimating a position near `lo`.
- The same happens on a long run of duplicates with one different key at the far end: `a[hi] - a[lo]` is tiny or the probe lands on the plateau.
- Average O(log log n) is only the uniform case. If you cannot defend that assumption, binary search is the safer bound.
**Examples:**

On `[1, 2, 4, 8, 16, 32, 64, 128]` a search for `2` estimates a position next to the left end, then repeats. Binary search discards half the indexes every probe.

**Sources:**

- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
</details>

<details>
<summary>What is Jump Search (Block Search) technique?</summary>

### What is Jump Search (Block Search) technique?

**Definition:**

Jump search scans a sorted array in fixed strides, then finishes with a linear search inside the one block that can hold the key.

**How it works:**

- Step size `m` is `floor(sqrt(n))`. Check indexes `m-1`, `2m-1`, … until the value is at least the target or the array ends.
- Step back one block and scan forward at most `m - 1` elements.
- Worst-case comparisons are about `n/m + m - 1`, which is O(√n) at that step size. Extra memory is O(1).
- The array must be sorted. Jump search is better than a full linear scan and worse than binary search on random-access memory.
- It looks backward only once. That matters when moving backward is much more expensive than moving forward.
**Examples:**

```text
m = floor(sqrt(n)); prev = 0; step = m
while a[min(step, n) - 1] < target:
  prev = step
  step += m
  if prev >= n: return -1
for i in prev .. min(step, n) - 1:
  if a[i] == target: return i
```

**Sources:**

- [Jump Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/jump-search/)
</details>

<details>
<summary>Explain the rationale behind choosing the Optimal Block Size for Jump Search.</summary>

### Explain the rationale behind choosing the Optimal Block Size for Jump Search.

**Definition:**

The block size minimizes the worst-case number of comparisons, which is the jumps plus the linear scan inside the last block.

**How it works:**

- Worst case does about `n/m` jumps and then `m - 1` linear comparisons: `f(m) = n/m + m - 1`.
- The derivative `f'(m) = -n/m^2 + 1` is zero at `m = √n`. The second derivative is positive there, so it is a minimum.
- Smaller blocks mean too many jumps. Larger blocks mean a long linear scan. Both ends are worse than √n.
- At `m = √n` the time is O(√n) and extra memory stays O(1).
- `m` has to be an integer, so use `floor(√n)` and clamp the last jump at `n`.
**Examples:**

For `n = 10_000`, `m = 100` gives about `100 + 99` comparisons. `m = 10` gives about `1000 + 9`. `m = 1000` gives about `10 + 999`.

**Sources:**

- [Jump Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/jump-search/)
</details>

<details>
<summary>When Jump Search is preferable over Binary Search?</summary>

### When Jump Search is preferable over Binary Search?

**Definition:**

Jump search is the better fit when the sorted data is cheap to scan forward and expensive to access at an arbitrary index, or when you want a single backward move instead of log n random probes.

**How it works:**

- On a random-access array, binary search uses fewer comparisons: Θ(log n) against O(√n).
- Jump search walks forward and steps back once. Binary search may jump both directions on every probe. If backward movement is costly, that single step-back is the reason to prefer jumps.
- Both require sorted order. Jump search uses O(1) extra memory, the same as iterative binary search.
- For a short sorted array the √n and log n curves are close, and the simpler forward scan can win on constants.
- It is not a replacement for binary search in ordinary RAM.
**Examples:**

A tape or a cursor that seeks forward quickly and rewinds slowly matches the jump pattern: long forward strides, one rewind, then a short forward scan.

**Sources:**

- [Jump Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/jump-search/)
</details>

<details>
<summary>What is Exponential Search (Doubling/Galloping)?</summary>

### What is Exponential Search (Doubling/Galloping)?

**Definition:**

Exponential search finds a range that must contain the key by doubling an index, then runs binary search on that range.

**How it works:**

- If `a[0]` is the target, return 0. Otherwise set `i = 1` and double while `i < n` and `a[i] < target`.
- The key, if present, lies in `[i/2, min(i, n - 1)]`. Binary-search that slice.
- Doubling and the following binary search are each O(log p) for a hit at index `p`, so the whole search is O(log n) on a finite array and O(log p) when the hit is near the front.
- Iterative binary search after the doubling uses O(1) extra memory. Recursive binary search uses O(log n) stack.
- It is the same idea as unbounded search. It is faster than a binary search that always starts on the full array when the key sits near the beginning.
**Examples:**

```text
if a[0] == target: return 0
i = 1
while i < n and a[i] < target:
  i *= 2
binary_search(a, i // 2, min(i, n - 1), target)
```

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
</details>

<details>
<summary>Compare Exponential Search to Interpolation Search regarding time complexity and use cases.</summary>

### Compare Exponential Search to Interpolation Search regarding time complexity and use cases.

**Definition:**

Exponential search keeps a logarithmic worst case by doubling and then binary-searching. Interpolation search is faster only on uniform numeric data and can fall back to linear time.

**How it works:**

- Exponential search: O(log n) time on a finite sorted array, O(log p) when the key is at index `p`. Extra memory O(1) iterative, O(log n) if the binary search is recursive.
- Interpolation search: average O(log log n) when sorted values are uniform, worst case O(n). Extra memory O(1).
- Use exponential search for an unbounded or front-loaded sorted array, and whenever you need the binary-search worst case.
- Use interpolation search for a large sorted array of uniformly spread numbers, such as a dictionary-style numeric index, and only if a rare O(n) probe is acceptable.
- Exponential search does not assume how the values are spaced. Interpolation search does.
**Examples:**

A key at index 3 in a million-element array: exponential search checks indexes 1, 2, 4 and then binary-searches a tiny range. Interpolation search may land even closer if the values are uniform, and may walk almost the whole array if they grow exponentially.

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
</details>

### Ternary Search and Other Divide-and-Conquer Methods

<details>
<summary>What is Ternary Search?</summary>

### What is Ternary Search?

**Definition:**

Ternary search splits the current range into three parts with two probes and keeps the third that can still hold the answer.

**How it works:**

- `mid1 = lo + (hi - lo) / 3` and `mid2 = hi - (hi - lo) / 3`.
- On a sorted array, a target below `a[mid1]` continues on the left third. A target above `a[mid2]` continues on the right third. Otherwise the search stays between the probes.
- The index range shrinks to about one third, so the number of iterations is O(log3 n). Each iteration does two comparisons, and `2 log3 n` is larger than `log2 n`, so binary search usually wins on a sorted array.
- Extra memory is O(1) iterative.
- The shape it is actually for is a unimodal function: one valley or one peak. Compare the two probes and discard the side that cannot contain the extremum.
**Examples:**

```text
mid1 = lo + (hi - lo) // 3
mid2 = hi - (hi - lo) // 3
if target < a[mid1]: hi = mid1 - 1
elif target > a[mid2]: lo = mid2 + 1
else: lo, hi = mid1 + 1, mid2 - 1
```

**Sources:**

- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
</details>

<details>
<summary>What are the advantages of Binary Search over Ternary Search?</summary>

### What are the advantages of Binary Search over Ternary Search?

**Definition:**

On a sorted array, binary search does less work per discarded element than ternary search, even though ternary search shrinks the index range faster.

**How it works:**

- Binary search uses one midpoint and one comparison per step. Ternary search uses two midpoints, `lo + (hi-lo)/3` and `hi - (hi-lo)/3`, and two comparisons.
- Iterations are O(log2 n) versus O(log3 n). Two comparisons per ternary step make `2 log3 n` larger than `log2 n`, so binary search is usually faster in practice.
- Both are O(1) extra memory in the iterative form. Both need sorted order for an exact key lookup.
- Ternary search earns its extra probe on a unimodal function, where you are locating a peak or a valley rather than an equal key.

**Examples:**

Searching a sorted million-element array: binary search takes about 20 probes. Ternary search takes fewer rounds but about two key comparisons each, and the total is higher.

**Sources:**

- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
</details>

<details>
<summary>Implement Ternary Search for a finding a local maximum in a unimodal array.</summary>

### Implement Ternary Search for a finding a local maximum in a unimodal array.

**Definition:**

A unimodal array rises to one peak and then falls, or the other way around. Two probes tell you which side of the peak you are on, so you can discard a third of the range.

**How it works:**

- Set `mid1` and `mid2` one third and two thirds of the way through `[lo, hi]`.
- If `a[mid1] < a[mid2]`, the peak is not left of `mid1`: set `lo = mid1`. Otherwise the peak is not right of `mid2`: set `hi = mid2`.
- Stop when the range is a few elements and take the maximum among them.
- Time is O(log3 n) iterations, two reads each. Extra memory is O(1).
- This is not an equality search. A flat plateau or two peaks breaks the discard rule.

**Examples:**

```text
while hi - lo > 2:
  mid1 = lo + (hi - lo) // 3
  mid2 = hi - (hi - lo) // 3
  if a[mid1] < a[mid2]: lo = mid1
  else: hi = mid2
return index of max(a[lo..hi])
```

On `[1, 3, 8, 12, 9, 4, 2]` the peak is `12`.

**Sources:**

- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
</details>

<details>
<summary>Compare the efficiency of divide-and-conquer searching algorithms on unimodal and bitonic sequences.</summary>

### Compare the efficiency of divide-and-conquer searching algorithms on unimodal and bitonic sequences.

**Definition:**

A unimodal or bitonic sequence has one turn (up then down, or down then up). Divide-and-conquer uses two probes to discard a side that cannot hold the extremum. A fully sorted array is a special case and is cheaper to search with one probe.

**How it works:**

- Sorted equality search: binary search, Θ(log2 n) comparisons, one midpoint.
- One peak or one valley: ternary search, O(log3 n) rounds and two comparisons per round. The page lists bitonic point and unimodal optimization as the cases this split is for.
- `2 log3 n` comparisons is more than `log2 n`, so do not use the two-probe split to hunt an equal key in a sorted array.
- Extra memory is O(1) if the loop is iterative.
- Two turns, a plateau, or an unsorted sequence remove the discard proof. Fall back to a linear scan, O(n).

**Examples:**

Bitonic `[1, 4, 9, 7, 3, 0]`: `mid1` and `mid2` compared once drop a third of the indexes. Sorted `[0, 1, 3, 4, 7, 9]` looking for `7` only needs the single binary midpoint.

**Sources:**

- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
</details>

### Search in Data Structures

<details>
<summary>Explain the rationale behind using Binary Search on a Doubly Linked List.</summary>

### Explain the rationale behind using Binary Search on a Doubly Linked List.

**Definition:**

The halving idea still applies to a sorted list, but finding the middle node is itself a walk. On a linked list that walk costs linear time, so the whole search is O(n). A linear scan is the recommended search.

**How it works:**

- Each binary step finds the middle by advancing two pointers (slow and fast) from the start of the current sublist. That is Θ(length) pointer hops.
- Halving log n times still touches Θ(n) nodes in total, because `n + n/2 + n/4 + …` is Θ(n). GeeksforGeeks states the linked-list binary search as O(n) time and O(1) extra memory.
- A doubly linked list can walk backward, which makes the sublist bounds easier to express. It does not make the middle O(1). Random access is what binary search on an array is paying for.
- Prefer a linear scan, O(n) with a smaller constant. For sublinear search, use a skip list or an array index.
- The algorithm is worth showing so you can explain why the array version does not transfer.

**Examples:**

```text
mid = slow/fast walk from start to last
if mid.data == key: return true
if mid.data < key: start = mid.next
else: last = mid
```

**Sources:**

- [Binary Search on Singly Linked List — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/binary-search-on-singly-linked-list/)
</details>

<details>
<summary>Explain strategies for performing search operations in hash tables.</summary>

### Explain strategies for performing search operations in hash tables.

**Definition:**

A hash table maps a key through a hash function to a bucket and searches only that bucket. With a decent hash and a controlled load, search, insert, and delete are O(1) on average.

**How it works:**

- Compute `h(key)` and look in that slot. A hit returns the value. An empty slot, under open addressing with the same probe sequence, is a miss.
- Collisions are stored either by separate chaining (a list or tree in the bucket) or by open addressing (another slot in the table).
- Average time for search, insert, and delete is O(1). A bucket that collected every key is a list, and the search walks that bucket.
- Extra memory is Θ(n) for the table plus chains. A high load factor makes chains longer; rehash into a larger table to bring the average back down.
- Hash lookup is equality on the key. It does not answer “nearest key” or a range. Those need sorted order.

**Examples:**

```text
bucket = h(key) mod m
for item in table[bucket]:
  if item.key == key: return item
return miss
```

**Sources:**

- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

### Graph Searching Algorithms

<details>
<summary>Explain the Breadth-First Search (BFS) traversing method.</summary>

### Explain the Breadth-First Search (BFS) traversing method.

**Definition:**

BFS walks a graph level by level from a source. Every node one edge away is visited before any node two edges away.

**How it works:**

- Put the source in a queue and mark it visited. Repeat: pop a node, then enqueue each unvisited neighbor and mark it when you enqueue it, so it is not queued twice.
- A disconnected graph needs an outer loop that starts BFS from every unvisited vertex.
- Time is O(V + E): each vertex and each edge is processed once. The queue and the visited array take O(V) extra memory.
- On an unweighted graph the first time you reach a node is a shortest path in number of edges.
- The same traversal detects cycles, connected components, and is the skeleton of several routing and scheduling searches.

**Examples:**

```text
q = [src]; visited[src] = true
while q:
  u = q.pop_front()
  for v in adj[u]:
    if not visited[v]:
      visited[v] = true
      q.push_back(v)
```

**Sources:**

- [Breadth First Search or BFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Explain the Depth-First Search algorithm.</summary>

### Explain the Depth-First Search algorithm.

**Definition:**

DFS follows one neighbor as far as it can, then backtracks and tries the next neighbor. The order matches a preorder walk, with a visited set so a cycle does not loop forever.

**How it works:**

- Mark the current vertex visited, record it, then recurse on each unvisited neighbor.
- For a disconnected graph, start a new DFS at every vertex that is still unvisited.
- Time is O(V + E). Extra memory is O(V) for the visited array plus the recursion stack; GeeksforGeeks also counts the adjacency storage in the auxiliary bound O(V + E) for the recursive form.
- Neighbor order changes the traversal. There is more than one valid DFS of the same graph.
- An explicit stack does the same walk without recursion, which matters when the graph is deeper than the call stack.

**Examples:**

```text
def dfs(u):
  visited[u] = true
  for v in adj[u]:
    if not visited[v]:
      dfs(v)
```

**Sources:**

- [Depth First Search or DFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Compare the use cases of BFS and DFS in graph search.</summary>

### Compare the use cases of BFS and DFS in graph search.

**Definition:**

Both visit each vertex and edge a constant number of times, so both are O(V + E). They differ in order, memory shape, and which question the order answers.

**How it works:**

- BFS uses a queue and visits in increasing distance from the source. The first visit is a shortest path on an unweighted graph. Applications listed with BFS include shortest paths, cycle detection, connected components, and network routing.
- DFS uses recursion or a stack and finishes a whole branch before the next sibling. It fits topological walks, maze backtracking, and detecting a back edge to an ancestor.
- BFS queue memory is O(V) and can hold a wide level. DFS stack memory follows the depth. A long path makes the recursive DFS deep; an explicit stack avoids overflowing the call stack.
- A disconnected graph needs an outer scan of unvisited vertices for either algorithm.
- Neither handles positive edge weights as shortest paths. BFS counts edges, not weights.

**Examples:**

Fewest hops from a router to a host: BFS. Exploring one corridor of a maze until it dead-ends, then stepping back: DFS.

**Sources:**

- [Breadth First Search or BFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Depth First Search or DFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Implement a solution to detect a cycle in an undirected graph using either BFS or DFS.</summary>

### Implement a solution to detect a cycle in an undirected graph using either BFS or DFS.

**Definition:**

In an undirected graph a cycle shows up as a visited neighbor that is not the parent you just came from. BFS checks that while it walks each component.

**How it works:**

- Store `(node, parent)` in the queue. The source starts with parent `-1`.
- For each neighbor: if it is new, mark it and enqueue it with the current node as parent. If it is already visited and it is not the parent, return true.
- Repeat from every unvisited vertex so a cycle in another component is not missed.
- Time is O(V + E). Extra memory is O(V) for the queue and the visited array. The adjacency lists are the input, not extra scratch space.
- The parent exception matters. The edge back to the parent is the same undirected edge, not a cycle.

**Examples:**

```text
q = [(start, -1)]; visited[start] = true
while q:
  node, parent = q.pop_front()
  for v in adj[node]:
    if not visited[v]:
      visited[v] = true
      q.push_back((v, node))
    elif v != parent:
      return true
```

Edges `0-1, 0-2, 1-2` return true. A straight path returns false.

**Sources:**

- [Detect cycle in an undirected graph using BFS — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/detect-cycle-in-an-undirected-graph-using-bfs/)
</details>

### Search Optimization and the Decision-Making Process

<details>
<summary>Choose The Fastest Algorithm for the given scenario.</summary>

### Choose The Fastest Algorithm for the given scenario.

**Definition:**

Pick the search from the shape of the data and from how many times you will ask. There is no single fastest algorithm until those are fixed.

**How it works:**

- One lookup in an unsorted collection: linear search, Θ(n) worst case, O(1) extra memory. Building an index first is wasted work.
- Many equality lookups: hash the keys. Search, insert, and delete are O(1) on average.
- A sorted random-access array and a worst-case promise: binary or exponential search, O(log n). Exponential search is the better of the two when the hit is near the front or the length is unknown: O(log p).
- Sorted and uniform numbers, and a rare linear worst case is acceptable: interpolation search, average O(log log n), worst O(n).
- Unweighted shortest path: BFS, O(V + E), not a binary search over vertices.

**Examples:**

Sorted array of a million ids, one query anywhere: binary search. The same array queried only near index 0: exponential search. Unsorted logs, one scan: linear search.

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Breadth First Search or BFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Discuss how you would balance a search tree to ensure that search operations remain efficient.</summary>

### Discuss how you would balance a search tree to ensure that search operations remain efficient.

**Definition:**

A plain binary search tree searches in O(height). Sorted inserts make the height Θ(n) and the search linear. A red-black tree recolors and rotates after each update so the height stays O(log n).

**How it works:**

- Insert as in a BST, then repair. A new red node under a black parent is already legal. A red parent breaks the “no two reds in a row” rule and is fixed by recoloring or a rotation.
- The black-height rule (every path to a leaf has the same number of black nodes) plus no adjacent reds implies height `h <= 2 log(n+1)`.
- Search, insert, and delete are then O(log n) in the worst case. An unbalanced BST is O(n).
- The same bound is why maps and sets use red-black trees: each lookup stays logarithmic after a stream of inserts.
- Rotations are O(1) pointer rewrites. The cost of staying balanced is a few of those per update, not a full rebuild.

**Examples:**

Inserting `1, 2, 3` into an empty BST makes a chain and the next search walks three nodes. The same inserts in a red-black tree rotate `2` to the root so the height stays logarithmic.

**Sources:**

- [Introduction to Red-Black Tree — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/)
</details>

<details>
<summary>How can heuristics be used to optimize search queries in a non-sortable dataset?</summary>

### How can heuristics be used to optimize search queries in a non-sortable dataset?

**Definition:**

If the keys have no order you can trust, binary search is invalid. A hash is the direct equality index. A heuristic helps when the query is fuzzy or the state space is a tree of choices, not a sorted row.

**How it works:**

- Equality on an unsorted set: hash the key. Average search is O(1). Collisions are chains or open addressing; a bad hash walks a long bucket.
- A unimodal numeric score, even if the raw records are not sorted by id, can be optimized with ternary search on that score: two probes, O(log3 n) rounds.
- In a game tree or other huge decision tree, Monte Carlo tree search biases expansion toward promising moves (selection, expansion, random playout, backpropagation) instead of enumerating every branch.
- Heuristics do not change a proof of sorted order. They change which region you bother to look at.
- If you will ask many exact questions, pay for the hash table once. A heuristic scan on every query stays expensive.

**Examples:**

User ids with no order: `bucket = hash(id)`. A game position with no sort key: expand the child with the best win rate so far, and still sample an under-visited child.

**Sources:**

- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
- [Monte Carlo tree search — Wikipedia](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search)
</details>

### Searching in Infinite or Unknown Sized Data

<details>
<summary>Explain how you should perform a search in a stream of infinite or unknown size data.</summary>

### Explain how you should perform a search in a stream of infinite or unknown size data.

**Definition:**

You cannot binary-search a stream until you have a finite sorted window. Grow a window by doubling until it passes the target, then binary-search inside it.

**How it works:**

- Start at `lo = 0`, `hi = 1`. While the value at `hi` is still below the target, set `lo = hi` and double `hi`.
- The bound is found in O(log p) probes, where `p` is the index of the key. Binary search on `[lo, hi]` is another O(log p). Total time is O(log p).
- Iterative binary search uses O(1) extra memory. The recursive form uses O(log p) stack.
- This requires random access and sorted order. A one-pass stream that you cannot reread has no `a[hi]` to jump to; you either buffer a sorted window or you scan.
- On a finite array that only pretends to be infinite, clamp `hi` before it runs off the end.

**Examples:**

```text
lo, hi = 0, 1
while a[hi] < target:
  lo = hi
  hi = 2 * hi
binary_search(a, lo, hi, target)
```

**Sources:**

- [Find position of an element in a sorted array of infinite numbers — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/find-position-element-sorted-array-infinite-numbers/)
</details>

<details>
<summary>Implement a technique for backtracking through data when searching for a condition that requires context.</summary>

### Implement a technique for backtracking through data when searching for a condition that requires context.

**Definition:**

DFS is the backtracking search: you walk one branch to the end, then return to the previous vertex and try the next neighbor. The call stack is the context.

**How it works:**

- Mark the vertex on the way in. Recurse into each unvisited neighbor. The path from the root of the recursion to the current call is the context that led here.
- When the recursive call returns, the next neighbor is tried. That return is the backtrack.
- Time is O(V + E) for a plain traversal that does not revisit. Extra memory is the visited array plus the recursion stack.
- A condition that depends on the path (a partial route, an open parenthesis count, a used column) is checked before the recursive call. If it fails, you skip that neighbor. The page’s graph DFS is the same control flow with “unvisited” as the condition.
- An explicit stack stores the same context when the depth would overflow the call stack.

**Examples:**

```text
def dfs(u, context):
  if fails(context): return
  if goal(u, context): return u
  visited[u] = true
  for v in adj[u]:
    if not visited[v]:
      dfs(v, context.with(v))
```

**Sources:**

- [Depth First Search or DFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

### Probabilistic and Approximate Searches

<details>
<summary>Describe Monte Carlo Tree Search and its applications in game AI.</summary>

### Describe Monte Carlo Tree Search and its applications in game AI.

**Definition:**

Monte Carlo tree search estimates good moves by playing many random games and growing the tree toward moves that win more often, instead of scoring every leaf with a hand-written evaluation.

**How it works:**

- Each round is four steps. Selection walks from the root to a leaf, biased toward promising children. Expansion adds a child for a legal move. Simulation plays random moves to the end. Backpropagation writes the result back up the path.
- UCT balances exploitation (high win rate) and exploration (few visits). After the time budget, the usual choice is the child with the most simulations.
- There is no closed O(log n) bound: the tree grows with the number of playouts, and a single playout can be as long as the game. The point is to spend a fixed time budget on the promising part of an exponential game tree.
- Pure Monte Carlo (the same number of playouts on every legal move) is the simpler version. Tree search concentrates later playouts where earlier ones won.
- Used in Go, chess, shogi, and other board games. AlphaGo and AlphaZero keep the tree and replace the random playout with a neural network value.

**Examples:**

From a root with three moves, 21 playouts might show 11 wins for the side to move. The next selection prefers the child with a high win ratio and still tries a child that has barely been sampled.

**Sources:**

- [Monte Carlo tree search — Wikipedia](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search)
</details>

<details>
<summary>Explain what is Approximate String Matching, and provide an example of where it’s used.</summary>

### Explain what is Approximate String Matching, and provide an example of where it’s used.

**Definition:**

Approximate string matching reports how close two strings are when they are not required to be equal. The usual distance is Levenshtein: the minimum insertions, deletions, and substitutions that turn one string into the other.

**How it works:**

- Dynamic programming fills a grid. `dp[i][j]` is the distance between the first `i` characters of `s` and the first `j` characters of `t`.
- A match copies the diagonal. A mismatch takes one plus the best of insert, delete, or substitute.
- Time is O(m n). A full table uses O(m n) memory. Keeping only the previous row brings extra memory to O(n) and leaves the time at O(m n).
- Spell check, search boxes that tolerate a typo, and DNA comparison all ask for a small distance rather than an exact hit.
- Exact search structures (hash, binary search) miss `kitten` versus `sitting`. The distance is 3.

**Examples:**

```text
for i in 1..m:
  for j in 1..n:
    cost = 0 if s[i-1] == t[j-1] else 1
    dp[i][j] = min(
      dp[i-1][j] + 1,
      dp[i][j-1] + 1,
      dp[i-1][j-1] + cost)
```

**Sources:**

- [Introduction to Levenshtein Distance — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-levenshtein-distance/)
</details>

### Complex Search Scenarios

<details>
<summary>Implement an algorithm to find the k closest points to a given origin in a multidimensional space.</summary>

### Implement an algorithm to find the k closest points to a given origin in a multidimensional space.

**Definition:**

Keep a max-heap of the k closest points seen so far, ordered by squared distance from the origin. Squared distance preserves order and skips the square root.

**How it works:**

- For each point, compute `x^2 + y^2` (and further coordinates the same way).
- If the heap holds fewer than k points, push. If it is full and this point is closer than the farthest stored point, pop that farthest and push the new one.
- Time is O(n log k). Extra memory is O(k) for the heap.
- Sorting every point is O(n log n). The heap is the expected approach when k is much smaller than n.
- The heap does not return points in distance order. Popping it at the end sorts those k.

**Examples:**

```text
heap = max-heap by squared distance
for p in points:
  d = p.x*p.x + p.y*p.y
  if heap.size < k: heap.push(d, p)
  elif d < heap.max: heap.pop(); heap.push(d, p)
```

**Sources:**

- [K Closest Points to the Origin — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/find-k-closest-points-to-the-origin/)
</details>

<details>
<summary>How does Search Engine Indexing work, and explain the role of search algorithms in this process.</summary>

### How does Search Engine Indexing work, and explain the role of search algorithms in this process.

**Definition:**

A search engine does not scan the web at query time. It stores each term in a structure that can answer “which documents contain this key” quickly, then combines those hits.

**How it works:**

- Hashing maps a term to a bucket in average O(1), which is how a dictionary of terms supports search, insert, and delete. The bucket holds the posting list of document ids.
- If those ids are kept sorted, binary or exponential search finds a document id in O(log n), and exponential search is the better fit when the id is near the front of a long list.
- Several terms are combined by walking those ordered lists. Jump search is one way to stride through a sorted posting list in O(√n) when random jumps are costly.
- Ranking and crawling are outside the lookup. The search algorithms answer existence and position inside the index that the crawl already built.
- A typo is not an equality hit. Levenshtein distance, O(m n) time, measures a near miss between the query and a dictionary term.

**Examples:**

Query `red tree`: hash `red` and hash `tree`, then walk the two sorted id lists. A mistyped `tress` is a distance check against dictionary terms, not a binary search for the exact string.

**Sources:**

- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Introduction to Levenshtein Distance — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-levenshtein-distance/)
</details>

### Constraints and Specific Criteria

<details>
<summary>How do you find an item with multiple search criteria or constraints?</summary>

### How do you find an item with multiple search criteria or constraints?

**Definition:**

Split the constraints into equality, which a hash answers, and order, which a sorted structure answers. Apply the most selective test first.

**How it works:**

- Each equality field can be a hash key. Average lookup is O(1). A composite key such as `(user, day)` is one hash if you always query both.
- A range (“price at most 20”, “nearest value”) needs sorted order. Binary search for the closest value is O(log n) time and O(1) extra memory.
- If one constraint is a graph distance with no weights, BFS from the source is O(V + E) and the other filters are checked on the nodes it reaches.
- Doing the O(n) scan first and the O(1) hash second throws away the index. Intersect the small hash hit-list with the next constraint.
- A constraint that is not equality and not ordered (a fuzzy string) is a distance, O(m n), and should run on the already reduced set.

**Examples:**

Find orders for `user = 7` and `total` nearest 100: hash `user` to a short list, then binary-search that list if it is sorted by `total`.

**Sources:**

- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Closest in Sorted array — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/find-closest-number-array/)
</details>

<details>
<summary>Discuss how to adapt searching algorithms in databases that enforce unique constraints.</summary>

### Discuss how to adapt searching algorithms in databases that enforce unique constraints.

**Definition:**

A unique constraint is a set of distinct keys. The structure that implements a set of keys with average O(1) search, insert, and delete is a hash table. A balanced search tree is the alternative when you also need order.

**How it works:**

- On insert, search the key first. If the search hits, reject the row. Hashing does that probe in O(1) on average.
- A collision chain or an open-addressed probe must be walked to be sure the key is absent. Uniqueness is only as strong as a full probe of the bucket.
- If the unique column is also used for range queries, a red-black tree keeps search, insert, and delete at O(log n) worst case and stays sorted. A hash does not answer “next key”.
- Do not linear-scan the table to enforce uniqueness. That is O(n) per insert and races with the constraint.
- The index is the constraint’s lookup path. Dropping it leaves a scan.

**Examples:**

```text
if hash_search(table, key) != miss: reject
hash_insert(table, key)
```

**Sources:**

- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Introduction to Red-Black Tree — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/)
</details>

### Recursion and Iteration in Searches

<details>
<summary>Identify potential issues in this Recursive Binary Search implementation.</summary>

### Identify potential issues in this Recursive Binary Search implementation.

**Definition:**

A correct recursive binary search computes the middle as `lo + (hi - lo) / 2`, searches only `lo..mid-1` or `mid+1..hi`, and stops when `hi < lo`. Several small mistakes turn that into an infinite recursion or a wrong miss.

**How it works:**

- `mid = (lo + hi) / 2` can overflow a fixed-width integer when both ends are large. The form `lo + (hi - lo) / 2` stays in range. GeeksforGeeks uses that form.
- Setting `hi = mid` or `lo = mid` when `a[mid]` is not the target never shrinks the range. The recursive call must exclude `mid`.
- The base case is `hi < lo`, returning the miss sentinel. Forgetting it recurses forever.
- Time is O(log n) comparisons. Extra memory is O(log n) stack frames, one per halving. That stack is the cost the iterative loop does not pay.
- The array must stay sorted for the lifetime of the call. A concurrent writer that reorders elements makes either half a lie.

**Examples:**

```text
def binary_search(a, lo, hi, x):
  if hi < lo: return -1
  mid = lo + (hi - lo) // 2
  if a[mid] == x: return mid
  if a[mid] > x: return binary_search(a, lo, mid - 1, x)
  return binary_search(a, mid + 1, hi, x)
```

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
</details>

<details>
<summary>Provide a side-by-side comparison of recursive versus iterative approaches in search algorithm implementations.</summary>

### Provide a side-by-side comparison of recursive versus iterative approaches in search algorithm implementations.

**Definition:**

The comparisons are the same. The difference is where the bounds live: in stack frames, or in two local variables.

**How it works:**

- Both binary searches do O(log n) comparisons on a sorted array. Exponential search is the same algorithm after a doubling loop, and GeeksforGeeks states the recursive form as O(log n) time and O(log n) space, the iterative form as O(log n) time and O(1) space.
- Recursive DFS is O(V + E) time with a call stack. The visited array is O(V). Deep graphs overflow that stack; an explicit stack keeps the same walk.
- BFS is naturally iterative: the queue is the frontier. Recursing “level by level” does not simplify it.
- Recursive code matches the recurrence (half the array, or one neighbor). Iterative code makes the memory bound obvious.
- Tail calls are not a given. Python and Java do not remove the binary-search frame, so the O(log n) stack is real there.

**Examples:**

```text
# iterative
while lo <= hi:
  mid = lo + (hi - lo) // 2
  ...
# recursive
binary_search(a, lo, mid - 1, x)
```

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Depth First Search or DFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

<details>
<summary>Discuss scenarios where an iterative approach to searching algorithm might be beneficial over recursive.</summary>

### Discuss scenarios where an iterative approach to searching algorithm might be beneficial over recursive.

**Definition:**

Use the loop when the search is a binary search or a graph walk deep enough to blow the call stack, and when you want O(1) extra memory besides the input.

**How it works:**

- Iterative exponential search is O(log n) time and O(1) extra memory. The recursive binary search inside the same algorithm is O(log n) stack.
- A skewed tree or a long path makes recursive DFS O(n) frames. An explicit stack still searches in O(V + E) and fails by returning, not by crashing the process.
- Languages without tail-call elimination keep every binary-search frame. The loop does not.
- The loop is also easier to stop from another thread: one flag checked at the top of `while`, instead of unwinding frames.
- Recursion stays the better explanation for a short divide-and-conquer proof. It is the worse deployment when depth is data-dependent.

**Examples:**

Binary search on a billion sorted keys is about 30 frames if recursive and two integers if iterative. A linked-list-shaped DFS of a million nodes is a million frames.

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Depth First Search or DFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/depth-first-search-or-dfs-for-a-graph/)
</details>

### Edge Cases and Search Algorithm Selection

<details>
<summary>Illustrate how to choose the appropriate search algorithm when dealing with sparse datasets.</summary>

### Illustrate how to choose the appropriate search algorithm when dealing with sparse datasets.

**Definition:**

Sparse means most possible keys are absent. Do not allocate a dense array just to binary-search empty slots. Store the keys that exist and search that smaller structure.

**How it works:**

- A hash of the present keys answers equality in O(1) on average and uses memory proportional to the keys you stored, not to the size of the key space.
- If the present keys are sorted in an array, binary or exponential search is O(log n) in the number of present keys. Empty ranges are simply not in the array.
- A graph with few edges is sparse. BFS and DFS are O(V + E), which is much smaller than O(V^2) only because you do not scan a dense matrix.
- Interpolation search assumes the stored values are uniform. A sparse sorted array of the survivors can still be uniform; the missing keys are not holes inside the array.
- Scanning every integer from the minimum key to the maximum key is O(range), which is the mistake sparsity is warning you about.

**Examples:**

User ids in `1..10^12` with 50_000 accounts: hash the 50_000, or sort them and binary-search. Do not allocate `10^12` slots.

**Sources:**

- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Breadth First Search or BFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
</details>

<details>
<summary>Discuss the impact of sorted vs. unsorted data on selecting the appropriate search algorithm.</summary>

### Discuss the impact of sorted vs. unsorted data on selecting the appropriate search algorithm.

**Definition:**

Sorted order is the permission slip for binary, exponential, jump, and interpolation search. Without it those algorithms return the wrong index. Unsorted data is a scan or a hash.

**How it works:**

- Unsorted, one query: linear search, Θ(n) worst case.
- Unsorted, many equality queries: build a hash, average O(1) per search after the build.
- Sorted random access: binary or exponential search, O(log n). Exponential search is preferable when the key is near the start.
- Sorted and uniform: interpolation, average O(log log n), worst O(n). Jump search is O(√n) and only steps back once.
- Sorting an unsorted array just to binary-search it once costs more than one linear scan. Sort when you will search many times or you already need the order.

**Examples:**

`[4, 1, 7]` binary-searched for `4` looks at the middle `1`, decides the target is to the right, and misses `4` on the left. Sorted `[1, 4, 7]` finds `4` at index 1.

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Jump Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/jump-search/)
- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

<details>
<summary>How do you handle searches in datasets with periodic, predictable patterns or anomalies?</summary>

### How do you handle searches in datasets with periodic, predictable patterns or anomalies?

**Definition:**

Binary search and interpolation search both assume a monotone layout. A periodic pattern repeats, so the same value appears in many places and a single midpoint does not tell you which period holds the hit you want.

**How it works:**

- If the series is sorted inside each period and you know the period length, reduce the query to one period and binary-search that slice in O(log p), where `p` is the period.
- A unimodal hump inside each period is a ternary search on that hump, O(log3 p) rounds, not a binary search across periods.
- An anomaly that breaks sorted order (one swapped pair, one spike) invalidates every discard step. Detect it or search the unbroken prefix and suffix separately.
- Equality of a repeating key is a hash, average O(1), which does not care that the key is periodic.
- Do not run interpolation across a sine-like sorted-looking sample. The uniform-line formula is exactly what a periodic curve violates, and the worst case is O(n).

**Examples:**

Daily temperatures repeating every 24 samples: binary-search inside one day for a threshold, or hash the timestamp. Do not binary-search the whole month for “the” peak.

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
</details>

### Search Algorithm Modifications and Customization

<details>
<summary>Describe how to customize a binary search algorithm for finding the closest value to a target in a sorted array.</summary>

### Describe how to customize a binary search algorithm for finding the closest value to a target in a sorted array.

**Definition:**

Keep the binary search, and remember the closest value seen at each midpoint. When the range empties, that remembered value is the answer. Exact equality can return immediately.

**How it works:**

- `res` starts as `a[0]`. While `lo <= hi`, set `mid = lo + (hi - lo) / 2`.
- If `abs(a[mid] - k)` is smaller than `abs(res - k)`, replace `res`. On a tie, keep the larger value.
- If `a[mid] == k`, return it. If `a[mid] < k`, set `lo = mid + 1`. Otherwise set `hi = mid - 1`.
- Time is O(log n). Extra memory is O(1). A linear scan is O(n) and only worth it for a tiny array.
- The array must be sorted. Duplicates are fine: the tie rule picks the greater value when two numbers are equally close.

**Examples:**

```text
res = a[0]
while lo <= hi:
  mid = lo + (hi - lo) // 2
  if abs(a[mid] - k) < abs(res - k): res = a[mid]
  elif abs(a[mid] - k) == abs(res - k): res = max(res, a[mid])
  if a[mid] == k: return a[mid]
  if a[mid] < k: lo = mid + 1
  else: hi = mid - 1
```

In `[1, 2, 4, 5, 6, 6, 8, 8, 9]` the closest value to `11` is `9`.

**Sources:**

- [Closest in Sorted array — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/find-closest-number-array/)
</details>

<details>
<summary>How can you modify standard search algorithms to support concurrent searches in a multi-threaded environment?</summary>

### How can you modify standard search algorithms to support concurrent searches in a multi-threaded environment?

**Definition:**

A search is safe to run in parallel when every thread owns its indices and the structure it reads does not change. Sharing `lo` and `hi` between threads corrupts the range.

**How it works:**

- Iterative binary or exponential search keeps `lo`, `hi`, and `mid` in locals. Two threads can run that loop on the same sorted array. Time per search stays O(log n). Extra memory is O(1) per thread.
- BFS and DFS each need a private visited array and a private queue or stack. Sharing visited marks drops vertices another search still needs. Each search is still O(V + E) time and O(V) extra memory.
- A hash search is an average O(1) read of a bucket. A concurrent insert that rehashes moves buckets while a reader walks them. Publish a finished table, or protect the update, before readers run.
- A writer that swaps two elements removes the sorted-order assumption. Concurrent readers of a binary search then discard the wrong half. Rebuild or replace the array, then let new searches see the new pointer.
- The recursive form adds a private stack per thread. It does not need a lock on the array if the array is immutable for the duration.

**Examples:**

```text
# each thread
lo, hi = 0, n - 1
while lo <= hi:
  mid = lo + (hi - lo) // 2
  ...
```

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Breadth First Search or BFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

### Search in Real-world Applications

<details>
<summary>Explain how search algorithms are used in machine learning model optimization.</summary>

### Explain how search algorithms are used in machine learning model optimization.

**Definition:**

Training searches a space of parameters for a better loss. When that space is a sorted or unimodal line, the same divide-and-conquer searches used on arrays apply. When it is a combinatorial tree, branch-and-bound prunes it.

**How it works:**

- A unimodal loss of one variable (one valley) is a ternary search: two probes per round, O(log3 n) rounds on a discrete grid. Binary search fits a monotone predicate such as “is this learning rate already too high?”.
- Hyperparameters that are equality keys in a cache of finished runs are a hash: average O(1) to see if that configuration was already trained.
- A discrete choice such as a 0/1 assignment is a state-space tree. Branch and bound computes a bound at each node and prunes a node whose bound cannot beat the best solution known so far. Worst case the tree is still large in the size of the problem.
- These searches assume the shape you claim. A loss with many valleys is not ternary-searchable; pruning needs a real bound, not a guess.
- The model’s own gradient step is a different iteration. It is not binary search unless the objective is the monotone or unimodal case above.

**Examples:**

Pick the smallest regularization that keeps validation error under a limit: binary-search the sorted grid of penalties. Fit a unimodal width of a kernel: ternary-search that width.

**Sources:**

- [Ternary Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/ternary-search/)
- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
- [Introduction to Branch and Bound — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-branch-and-bound-data-structures-and-algorithms-tutorial/)
</details>

<details>
<summary>Discuss how search algorithms are used in network routing and optimization.</summary>

### Discuss how search algorithms are used in network routing and optimization.

**Definition:**

On an unweighted network, fewest hops is breadth-first search. Routing tables that must stay ordered use a balanced search tree. A combinatorial route choice that needs a proven optimum is branch and bound.

**How it works:**

- BFS from the source visits nodes in increasing hop count. Time is O(V + E). The queue is O(V). The first time a host is dequeued, the path that reached it is a shortest hop path. GeeksforGeeks lists network routing among BFS applications.
- A routing table keyed by address, with inserts and longest-match style ordered lookup, fits a red-black tree: search, insert, and delete are O(log n) worst case, and the height is O(log n).
- Choosing a route through a set of discrete options (assignment, a tour) builds a state-space tree. Branch and bound prunes a partial route whose bound cannot beat the best complete route. The worst-case tree can still be very large.
- DFS explores one path to the end before others. That is a poor fewest-hop search, because the first hit is not the shortest.
- Weights on edges are not hop counts. BFS does not minimize a sum of positive weights.

**Examples:**

Five routers in a line, unit links: BFS from router 0 reaches router 4 in four hops and does not need to score any other walk.

**Sources:**

- [Breadth First Search or BFS for a Graph — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/breadth-first-search-or-bfs-for-a-graph/)
- [Introduction to Red-Black Tree — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/)
- [Introduction to Branch and Bound — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-branch-and-bound-data-structures-and-algorithms-tutorial/)
</details>

### Optimization Techniques in Searches

<details>
<summary>Explain how branch and bound can be used to improve the efficiency of search algorithms.</summary>

### Explain how branch and bound can be used to improve the efficiency of search algorithms.

**Definition:**

Branch and bound searches a state-space tree of partial solutions and refuses to expand a node whose bound cannot beat the best complete solution already found.

**How it works:**

- Branching splits a problem into smaller subproblems, the children of the node.
- Bounding computes an upper or lower bound on the best solution inside that child. If the bound is worse than the incumbent, the node is pruned.
- FIFO order is a queue (breadth-first). LIFO order is a stack. Least-cost order expands the live node with the best bound first, which is the version that skips the most hopeless branches.
- The page does not give a single closed time bound. It does say you do not explore every node, and that in the worst case the number of nodes can still be very large in the size of the problem. Pruning improves typical runs; it is not a promise of O(log n).
- It fits discrete and combinatorial optimization: 0/1 knapsack, assignment, the traveling salesman problem. It is the wrong tool for a sorted-array lookup.

**Examples:**

A partial tour whose lower bound is already 40, while a finished tour costs 25, is not expanded. Every city sequence under that partial tour is skipped.

**Sources:**

- [Introduction to Branch and Bound — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/introduction-to-branch-and-bound-data-structures-and-algorithms-tutorial/)
</details>

<details>
<summary>Describe an implementation of an adaptive search algorithm that optimizes based on past accessibility of data.</summary>

### Describe an implementation of an adaptive search algorithm that optimizes based on past accessibility of data.

**Definition:**

Adaptive here means the next probe depends on where earlier probes landed, not on a fixed middle. Exponential search does that by doubling until it passes the key, so keys near the front cost O(log p) instead of a full-array binary search.

**How it works:**

- Remember the last high index that was still below the target. The next window starts there and doubles. Past probes are the bounds, not a discarded history.
- After the window is found, binary search inside it. Total time is O(log p) for a hit at index `p`. Iterative form uses O(1) extra memory.
- Interpolation search adapts the probe to the values, not only to the indexes: `pos` slides toward `hi` when the key is near `a[hi]`. Average O(log log n) on uniform data, worst O(n).
- A hash remembers the bucket of a key you inserted before. The next equality search of that key is an average O(1) bucket read.
- A fixed binary search ignores where the last hit was. Use exponential search when the working set sits at the front of a sorted array.

**Examples:**

```text
i = 1
while i < n and a[i] < target:
  i *= 2
binary_search(a, i // 2, min(i, n - 1), target)
```

**Sources:**

- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
- [Hashing in Data Structure — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)
</details>

### Combining Search Algorithms

<details>
<summary>How can you combine Linear search with other search algorithms for an improved search strategy in some cases?</summary>

### How can you combine Linear search with other search algorithms for an improved search strategy in some cases?

**Definition:**

Jump search and exponential search both refuse to scan every element, then finish with a short linear or binary scan of one block.

**How it works:**

- Jump search strides by `m = floor(√n)` and linear-scans only the last block, at most `m - 1` elements. Worst case O(√n), extra memory O(1). The linear tail is the part that confirms the key.
- Exponential search doubles an index, which is a sparse linear probe, then binary-searches `[i/2, min(i, n-1)]`. Time O(log n), and O(log p) when the key is at index `p`.
- The combination wins when a full linear scan is Θ(n) and a pure binary search’s random jumps are expensive. Jump search steps backward only once.
- Interpolation can replace the final binary search only if the block is uniform. Its worst case inside a bad block is linear anyway.
- Do not linear-scan the whole array after you already know a O(log n) window. The linear work has to be the small block.

**Examples:**

```text
# jump, then linear
while a[min(step, n) - 1] < target: step += m
scan a[prev .. min(step, n))
```

**Sources:**

- [Jump Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/jump-search/)
- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
</details>

<details>
<summary>Discuss strategies to hybridize binary and linear searches when dealing with non-uniformly distributed datasets.</summary>

### Discuss strategies to hybridize binary and linear searches when dealing with non-uniformly distributed datasets.

**Definition:**

Non-uniform sorted data is where interpolation search falls from O(log log n) average to O(n) worst. Binary search does not care about the spacing and stays O(log n). A hybrid uses a value-based probe only while it shrinks the range, then switches.

**How it works:**

- Run an interpolation probe. If the new range is only one smaller, or `a[hi] - a[lo]` is tiny, stop trusting the formula and binary-search the remaining slice. Binary search is O(log n) and O(1) extra memory.
- Jump search is the other hybrid: fixed strides, then a linear scan of one block, O(√n). It never assumes uniform values, only sorted order.
- Exponential search plus binary search is the hybrid for a key near the front, still O(log p), with no uniformity assumption.
- A pure linear scan is the right last step only for a short block, as in jump search’s `m - 1` comparisons. Using it on the whole non-uniform array is Θ(n).
- The switch has to be explicit. Interpolation alone on exponential data is the slow case.

**Examples:**

On `[1, 2, 4, 8, …]`, the interpolation index sticks near the left end. After one probe that barely moves `hi`, hand `[lo, hi]` to binary search.

**Sources:**

- [Interpolation Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/interpolation-search/)
- [Jump Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/jump-search/)
- [Exponential Search — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/exponential-search/)
</details>
