# Divide & Conquer — Interview

Answers for this topic are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Divide & Conquer — Interview](./7_algo_divide_conquer_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Divide & Conquer](https://devinterview.io/questions/data-structures-and-algorithms/divide-and-conquer-interview-questions/).

### Basic Concepts of Divide & Conquer Algorithms

<details>
<summary>Define Divide & Conquer algorithms and their main characteristics.</summary>

### Define Divide & Conquer algorithms and their main characteristics.

**Definition:**

Divide and conquer splits a problem into smaller independent subproblems of the same kind, solves them, and combines the results.

**How it works:**

- The split must produce subproblems that do not need each other's intermediate state.
- The base case is a constant-size input solved directly.
- The combine step is where the real work often sits: merging two sorted halves, or adding two partial products.
- The usual cost is a recurrence such as `T(n) = a T(n/b) + f(n)`.

**Examples:**

Binary search looks at the middle, then continues in one half. The other half is discarded, so `a = 1` and `b = 2`.
</details>

<details>
<summary>Explain the difference between Divide & Conquer and Dynamic Programming.</summary>

### Explain the difference between Divide & Conquer and Dynamic Programming.

**Definition:**

Divide and conquer solves each subproblem once as it recurses, and those subproblems are disjoint. Dynamic programming is for overlapping subproblems, and it stores answers so they are not recomputed.

**How it works:**

- Mergesort's halves do not overlap. Fibonacci's naive recursion computes the same `F(k)` many times; the DP table computes it once.
- Divide and conquer is usually top-down. DP is top-down with memoization or bottom-up over a table.
- If you add a cache to a divide-and-conquer recursion and the cache hits, you have reinvented DP.
- DP needs an optimal substructure too: the best answer is built from best answers of smaller states.
</details>

<details>
<summary>What is the role of recursion in Divide & Conquer algorithms?</summary>

### What is the role of recursion in Divide & Conquer algorithms?

**Definition:**

Recursion is the usual way to apply the same algorithm to each smaller piece until the base case.

**How it works:**

- Each call receives a range, often `lo` and `hi`, and calls itself on one or more subranges.
- The call stack holds the unfinished combine steps. Depth is O(log n) when the size shrinks by a constant factor, and O(n) for a badly unbalanced split.
- An explicit stack or a bottom-up loop can replace the calls. Mergesort's bottom-up form merges runs of length 1, then 2, then 4.
- The recursion does not create the speedup by itself. The speedup comes from fewer combinations than a naive scan of all pairs.
</details>

<details>
<summary>What are the three main steps in a typical Divide & Conquer algorithm?</summary>

### What are the three main steps in a typical Divide & Conquer algorithm?

**Definition:**

The three steps are divide, conquer, and combine.

**How it works:**

- Divide splits the input into smaller independent pieces. Equal sizes keep the recurrence balanced.
- Conquer solves each piece. If it is small enough, solve it directly. Otherwise recurse.
- Combine merges the piece results into an answer for the original input.
- Some algorithms have a trivial combine (binary search returns the recursive answer) and some have a trivial divide (quicksort's work is the partition before the recursive calls).

**Examples:**

Mergesort: split the array in half, sort each half, then merge with two pointers.
</details>

### Mathematical Foundations

<details>
<summary>Give an example of a recurrence relation that can describe the time complexity of a Divide & Conquer algorithm.</summary>

### Give an example of a recurrence relation that can describe the time complexity of a Divide & Conquer algorithm.

**Definition:**

A recurrence states the cost of size n in terms of the cost of the smaller calls plus the work outside them.

**How it works:**

- `T(n) = 2 T(n/2) + O(n)` is mergesort: two halves, and a linear merge.
- `T(n) = T(n/2) + O(1)` is binary search: one half, and a constant test.
- `T(n) = 2 T(n/2) + O(n²)` would be a quadratic combine on two halves, which is worse than a straight double loop.
- The base is `T(1) = O(1)`. Floors and ceilings do not change the asymptotic result for these smooth splits.

**Examples:**

Unrolling mergesort: `n + 2·(n/2) + 4·(n/4) + …` is `n` per level for `log₂ n` levels, so `O(n log n)`.
</details>

<details>
<summary>Explain the Master Theorem and its importance in analyzing Divide & Conquer algorithms.</summary>

### Explain the Master Theorem and its importance in analyzing Divide & Conquer algorithms.

**Definition:**

The Master Theorem gives the asymptotic solution of `T(n) = a T(n/b) + O(nᵏ)` by comparing the work at the root with the work in the leaves.

**How it works:**

- Let `c_crit = log_b(a)`. If `k < c_crit`, the leaves dominate and `T(n) = Θ(n^{c_crit})`.
- If `k = c_crit`, every level costs the same and `T(n) = Θ(nᵏ log n)`.
- If `k > c_crit`, the root's combine dominates and `T(n) = Θ(nᵏ)`.
- It does not cover uneven splits, non-polynomial combine costs, or floors that change the shape. Those need a recursion tree or the Akra–Bazzi theorem.

**Examples:**

Mergesort has `a = 2`, `b = 2`, `k = 1`. `log₂ 2 = 1 = k`, so the middle case gives `Θ(n log n)`.
</details>

<details>
<summary>How can the Master Theorem be applied to find the time complexity of a binary search algorithm?</summary>

### How can the Master Theorem be applied to find the time complexity of a binary search algorithm?

**Definition:**

Binary search makes one recursive call on half the array and does a constant amount of work to compare the middle element.

**How it works:**

- So `a = 1`, `b = 2`, and `f(n) = O(1) = O(n⁰)`.
- `log₂ 1 = 0`, which equals the exponent of `f`. The balanced-levels case applies.
- The theorem then says `T(n) = Θ(n⁰ log n) = Θ(log n)`.
- The same bound comes from the depth: each call throws away half the remaining elements, so there are about `log₂ n` calls.
</details>

### Algorithms Design and Implementation

<details>
<summary>Describe how you would use Divide & Conquer to find the maximum and minimum of an array.</summary>

### Describe how you would use Divide & Conquer to find the maximum and minimum of an array.

**Definition:**

Split the array in half, find the min and max of each half, and keep the smaller min and the larger max.

**How it works:**

- A one-element range returns that element as both min and max. A two-element range takes one comparison.
- The combine is two comparisons. The recurrence is `T(n) = 2 T(n/2) + O(1)`, which is `Θ(n)`.
- The comparison count is about `3n/2 − 2` for n a power of two, better than the `2n − 2` of a single left-to-right scan that tracks both.
- The algorithm is still linear. The win is the constant, not the big-O class versus one pass.

**Examples:**

`[4, 1, 7, 3]` splits into `[4, 1]` and `[7, 3]`. Those return `(1, 4)` and `(3, 7)`. The combine returns min 1 and max 7.
</details>

<details>
<summary>Illustrate how the Merge Sort algorithm exemplifies the Divide & Conquer technique.</summary>

### Illustrate how the Merge Sort algorithm exemplifies the Divide & Conquer technique.

**Definition:**

Mergesort sorts each half recursively and merges the two sorted halves with a linear two-pointer scan.

**How it works:**

- Divide is the midpoint. Conquer is the two recursive sorts. Combine is the merge into an auxiliary buffer.
- The merge is stable if equal keys keep the left half's element first.
- Time is `Θ(n log n)` in every case. Extra memory for the buffer is `Θ(n)`.
- The recursion tree has `log n` levels and each level moves `Θ(n)` elements.

**Examples:**

```text
[3, 1, 4, 2]
[3, 1] [4, 2]
[3] [1] [4] [2]
merge -> [1, 3] [2, 4] -> [1, 2, 3, 4]
```
</details>

<details>
<summary>Explain how Quicksort works and how it adopts the Divide & Conquer strategy.</summary>

### Explain how Quicksort works and how it adopts the Divide & Conquer strategy.

**Definition:**

Quicksort partitions the array around a pivot, then sorts the two sides. The combine step does nothing because the partition already put every smaller key on the left.

**How it works:**

- The work is the partition, which is linear in the current range. Recursive calls then sort the left and the right, skipping the pivot.
- Average time on random or well-chosen pivots is `O(n log n)`. A pivot that is always the extreme key is `O(n²)`.
- It is usually in place, with `O(log n)` stack on balanced pivots and `O(n)` stack on degenerate ones. Sorting the smaller side first keeps the stack logarithmic.
- It is not stable in the usual in-place partition.

**Examples:**

`[3, 1, 4, 2]` with pivot 3 becomes `[1, 2, 3, 4]` after partition, and the two sides are already trivial.
</details>

<details>
<summary>How does the Karatsuba algorithm for multiplying large numbers employ Divide & Conquer?</summary>

### How does the Karatsuba algorithm for multiplying large numbers employ Divide & Conquer?

**Definition:**

Karatsuba multiplies two n-digit numbers with three multiplications of n/2-digit numbers instead of the four a schoolbook split would use.

**How it works:**

- Write `x = a · B + b` and `y = c · B + d`, where B is the base at the midpoint.
- The schoolbook product needs `ac`, `ad`, `bc`, and `bd`. Karatsuba also computes `(a + b)(c + d)` and recovers `ad + bc` as that product minus `ac` minus `bd`.
- The recurrence is `T(n) = 3 T(n/2) + O(n)`. Since `log₂ 3 ≈ 1.585`, the time is `O(n^{1.585})`, better than `O(n²)`.
- Below a small digit cutoff the implementation switches to schoolbook multiplication, because the extra additions dominate.
</details>

<details>
<summary>Describe the Strassen’s algorithm for matrix multiplication using Divide & Conquer.</summary>

### Describe the Strassen’s algorithm for matrix multiplication using Divide & Conquer.

**Definition:**

Strassen multiplies two n×n matrices with seven recursive multiplications of n/2 blocks instead of the eight in the block schoolbook method.

**How it works:**

- Each matrix is split into four n/2 blocks. Seven products of sums and differences of those blocks are enough to assemble the four blocks of the product.
- The recurrence is `T(n) = 7 T(n/2) + O(n²)`. `log₂ 7 ≈ 2.807`, so the time is `O(n^{2.807})`, below the classical `O(n³)`.
- The extra additions and the awkward block layout make it slower than a tuned classical kernel until n is large.
- Later algorithms have a lower exponent. Libraries still prefer blocked classical multiplication for the sizes they actually ship.
</details>

### Problem Solving with Divide & Conquer

<details>
<summary>How would you use a Divide & Conquer approach to calculate the power of a number?</summary>

### How would you use a Divide & Conquer approach to calculate the power of a number?

**Definition:**

Exponentiation by squaring halves the exponent each call, so a power takes a logarithmic number of multiplications.

**How it works:**

- `xⁿ = (x²)^{n/2}` when n is even. When n is odd, `xⁿ = x · xⁿ⁻¹`, and n−1 is even.
- The recurrence is `T(n) = T(n/2) + O(1)` multiplications, so `O(log n)` multiplications instead of `O(n)`.
- The same idea with modular multiplies computes `xⁿ mod m` without building the huge integer.
- Iterative binary exponentiation scans the bits of n and squares an accumulator, which is the same algorithm without recursion.

**Examples:**

`3⁸`: square to `9`, square to `81`, square to `6561`. Three squarings, not eight multiplications by 3.
</details>

<details>
<summary>Solve the Tower of Hanoi problem using Divide & Conquer techniques.</summary>

### Solve the Tower of Hanoi problem using Divide & Conquer techniques.

**Definition:**

To move n disks from peg A to peg C using spare peg B, move n−1 to the spare, move the largest disk to the target, then move n−1 from the spare onto the target.

**How it works:**

- The largest disk can move only when the n−1 above it are off it, and it can land only on an empty peg or a larger disk. The recursive moves keep that invariant.
- `T(n) = 2 T(n−1) + O(1)` with `T(1) = 1`, so the move count is `2ⁿ − 1`.
- This split is into two problems of size n−1, not n/2. The Master Theorem's `n/b` form does not apply; the closed form comes from unrolling.
- It is optimal for the classic rules: each disk configuration graph requires at least that many moves.

**Examples:**

Three disks A→C: move 2 from A to B (using C), move disk 3 A→C, move 2 from B to C (using A). The 'move 2' steps themselves split into single-disk moves.
</details>

<details>
<summary>Solve the Closest Pair of Points problem using Divide & Conquer.</summary>

### Solve the Closest Pair of Points problem using Divide & Conquer.

**Definition:**

Sort points by x, split them by a vertical line, solve each half, then check only the strip near the line where a closer pair could still hide.

**How it works:**

- The recursive answers give a distance δ, the best pair entirely on the left or entirely on the right.
- A closer pair must have one point in each half and both inside the vertical strip of width 2δ around the split.
- Points in that strip, sorted by y, need only a constant number of neighbors ahead in that order. Each point compares with the next few, not with the whole strip.
- The recurrence is `T(n) = 2 T(n/2) + O(n)` after the initial sort, so the search is `O(n log n)`.

**Examples:**

Left half's closest distance is 5, right half's is 4, so δ = 4. Only points within 4 of the midline are compared across the cut, and the true closest pair might be one of those cross pairs at distance 3.
</details>

<details>
<summary>Use Divide & Conquer to solve the Majority Element problem in an array.</summary>

### Use Divide & Conquer to solve the Majority Element problem in an array.

**Definition:**
Split the array in half, find a majority candidate in each half, then keep a candidate only if it occurs more than n/2 times in the whole range. This is the recursive split. Boyer–Moore voting is a different algorithm.

**How it works:**
- A majority of the whole range is a majority of at least one half.
- Base case: one element is its own majority.
- Combine by counting the two candidates across the current range. That scan is Θ(n).
- Recurrence T(n) = 2T(n/2) + Θ(n), the same recurrence merge sort solves as O(n log n).
- **Time:** O(n log n). **Space:** O(log n) for the recursion stack, plus O(1) extra counters.
- Moore’s voting finds a candidate in one pass and verifies it: O(n) time and O(1) space. It does not split the array.

**Examples:**
```text
majority(a, lo, hi):
    if lo == hi: return a[lo]
    mid = (lo + hi) // 2
    left = majority(a, lo, mid)
    right = majority(a, mid + 1, hi)
    if left == right: return left
    if count(a, lo, hi, left) > (hi - lo + 1) // 2: return left
    if count(a, lo, hi, right) > (hi - lo + 1) // 2: return right
    return null
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/majority-element/

</details>

<details>
<summary>Implement an algorithm to efficiently find the Median of Two Sorted Arrays.</summary>

### Implement an algorithm to efficiently find the Median of Two Sorted Arrays.

**Definition:**
The median is the middle value of the merged sorted arrays (or the average of the two middle values when the total length is even). Binary search on the smaller array finds the cut that partitions both arrays into equal halves.

**How it works:**
- Choose a cut i in the smaller array. The matching cut in the other array is half − i.
- The cut is valid when the largest value on the left of each array is at most the smallest value on the right of the other.
- If the left of the smaller array is too big, move the cut left; otherwise move it right.
- **Time:** O(log min(n, m)). **Space:** O(1).

**Examples:**
```text
median(a, b):  // a is the shorter array
    half = (len(a) + len(b) + 1) // 2
    lo, hi = 0, len(a)
    while lo <= hi:
        i = (lo + hi) // 2
        j = half - i
        leftA = a[i - 1] if i > 0 else -infinity
        rightA = a[i] if i < len(a) else +infinity
        leftB = b[j - 1] if j > 0 else -infinity
        rightB = b[j] if j < len(b) else +infinity
        if leftA <= rightB and leftB <= rightA:
            if (len(a) + len(b)) is odd: return max(leftA, leftB)
            return (max(leftA, leftB) + min(rightA, rightB)) / 2
        if leftA > rightB: hi = i - 1
        else: lo = i + 1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/median-of-two-sorted-arrays-of-different-sizes/

</details>

### Advanced Divide & Conquer Applications

<details>
<summary>Explain the key differences when Divide & Conquer is applied to External Sorting vs In-memory Sorting.</summary>

### Explain the key differences when Divide & Conquer is applied to External Sorting vs In-memory Sorting.

**Definition:**
Both use the same merge-sort shape: sort independent runs, then merge them. External sorting does that when the data does not fit in RAM. In-memory sorting keeps the whole array in RAM.

**How it works:**
- In memory, merge sort still needs an auxiliary buffer: O(n) extra space, and every case is O(n log n) time.
- Quicksort is the usual in-memory alternative: average θ(n log n), worst O(n²) when the pivot is always extreme, and it is cache-friendly because it partitions in place.
- External sorting is listed as a merge-sort application: produce sorted runs, then merge those runs. Sequential merge fits storage that is slow at random access.
- **Time:** in-memory merge is O(n log n) in every case. **Space:** O(n) auxiliary for the array merge.

**Examples:**
```text
external:
    read a RAM-sized chunk, sort it, write a run
    repeat until the file is consumed
    merge the runs with the same two-way merge used in memory
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/

</details>

<details>
<summary>How is Divide & Conquer utilized in the Cooley-Tukey FFT algorithm?</summary>

### How is Divide & Conquer utilized in the Cooley-Tukey FFT algorithm?

**Definition:**
A radix-2 decimation-in-time FFT rewrites a length-N DFT as two length-N/2 DFTs (even and odd indices) plus a combine stage of N/2 size-2 butterflies. That identity is the Danielson–Lanczos lemma.

**How it works:**
- Base case: a length-1 DFT is the sample itself.
- Even and odd halves are transformed recursively, then combined with twiddle factors.
- The two smaller results are reused for every output, which is why the cost is linearithmic: order N log N.
- Many implementations walk that tree breadth-first and avoid an explicit recursive call.
- **Time:** O(N log N). The direct DFT is a separate, slower algorithm.

**Examples:**
```text
ditfft2(x, N, s):
    if N == 1: return [x[0]]
    even = ditfft2(x, N/2, 2s)
    odd = ditfft2(x[s:], N/2, 2s)
    for k in 0 .. N/2 - 1:
        t = exp(-2πi k / N) * odd[k]
        X[k] = even[k] + t
        X[k + N/2] = even[k] - t
    return X
```

**Sources:**
- https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm

</details>

<details>
<summary>Solve the Skyline Problem using Divide & Conquer.</summary>

### Solve the Skyline Problem using Divide & Conquer.

**Definition:**
The skyline is the sequence of points (x, height) where the visible outline of buildings changes. The published expected solution sweeps those corners from left to right.

**How it works:**
- Collect every left and right corner, sort them, and walk left to right.
- A priority queue tracks which buildings are active at the current x. Emit a point only when the maximum active height changes.
- A naive sweep that rescans buildings at every corner is O(n²) time and O(n) space.
- **Time:** O(n log n) for the sweep with a priority queue. **Space:** O(n).

**Examples:**
```text
buildings = [[1, 5, 11]]
corners at x = 1 (height 11) and x = 5 (height drops)
skyline = [[1, 11], [5, 0]]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/the-skyline-problem-using-divide-and-conquer-algorithm/

</details>

### Optimization and Improvement

<details>
<summary>What strategies can be employed to reduce the overhead in recursive calls of Divide & Conquer algorithms?</summary>

### What strategies can be employed to reduce the overhead in recursive calls of Divide & Conquer algorithms?

**Definition:**
The recursion is the divide tree. Overhead is the call stack and the work done on tiny ranges. Cut that off with a larger base case, turn one recursive call into a loop, or drop recursion entirely when the combine step allows it.

**How it works:**
- Convex hull by divide and conquer stops at about 5 points and finishes them by the O(n³) brute hull. That local cost does not change the overall O(n log n) time; the merge of two hulls is O(n).
- Quicksort is tail recursive: the second partition can be a loop, so tail-call elimination can shrink the stack. Best auxiliary stack is O(log n); the worst, when the pivot is always extreme, is O(n).
- Heapsort’s recursive heapify uses O(log n) stack. The same heapify written as a loop uses O(1) auxiliary space. Time stays O(n log n) in every case.
- **Space:** from O(n) stack down to O(log n) or O(1), depending on which of these the algorithm allows.

**Examples:**
```text
quicksort(a, lo, hi):
    while lo < hi:
        p = partition(a, lo, hi)
        if p - lo < hi - p:
            quicksort(a, lo, p - 1)  // smaller side only
            lo = p + 1                // loop the larger side
        else:
            quicksort(a, p + 1, hi)
            hi = p - 1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

<details>
<summary>Discuss how tail recursion optimization could be beneficial for Divide & Conquer algorithms.</summary>

### Discuss how tail recursion optimization could be beneficial for Divide & Conquer algorithms.

**Definition:**
A call is in tail position when nothing remains to do after it returns. Tail-call elimination reuses the current frame instead of pushing another one.

**How it works:**
- Quicksort’s article describes it as tail recursive, so a compiler that performs tail-call optimization can turn the second call into a jump.
- Merge sort cannot do that for both halves: the second half runs, then a Θ(n) merge still has to run. Only a rewritten form that loops one side (as in the quicksort pattern) shrinks the stack.
- The time bounds do not change. Quicksort is still θ(n log n) on average and O(n²) in the worst case. The stack can drop from O(n) toward O(log n) when the larger side is the loop.
- **Space:** best auxiliary stack for quicksort is O(log n); worst is O(n).

**Examples:**
```text
// after partition, recurse on one side and set lo/hi for the other
// the second call is gone; the frame is reused by the loop
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Explain how memoization or caching can affect the performance of Divide & Conquer algorithms.</summary>

### Explain how memoization or caching can affect the performance of Divide & Conquer algorithms.

**Definition:**
Memoization stores a subproblem result so a later identical call can reuse it. It changes the cost only when the divide tree repeats work.

**How it works:**
- Merge sort’s halves are disjoint ranges. Each element sits in one subarray per level, and the combine step is a fresh Θ(n) merge. A cache of those ranges does not remove that merge, so the bound stays O(n log n) time and O(n) auxiliary space.
- The radix-2 FFT does reuse the two half-length DFTs: each butterfly reads the already computed half-results. That reuse is why the DFT drops to order N log N. Caching a half that is computed once is the algorithm, not an extra table on top of a naive recursion that recomputes it.
- **Time:** unchanged for disjoint splits such as merge sort; order N log N for the FFT because the half DFTs are reused.

**Examples:**
```text
merge sort level: [3, 1, 4, 2] → [3, 1] and [4, 2]
those two ranges never appear again, so a map keyed by range is never hit
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm

</details>

### Complexity Analysis

<details>
<summary>Compare the worst-case time complexities of Merge Sort, Quicksort, and Heapsort.</summary>

### Compare the worst-case time complexities of Merge Sort, Quicksort, and Heapsort.

**Definition:**
All three sort by repeatedly reducing the problem, but the worst-case bounds are not the same.

**How it works:**
- Merge sort: best, average, and worst are O(n log n). The merge is Θ(n) and the split is even. Auxiliary space is O(n).
- Quicksort: worst O(n²) when the pivot is always the extreme element (a sorted array with the first or last element as pivot). Average is θ(n log n). Best is Ω(n log n).
- Heapsort: O(n log n) in every case, in place. Recursive heapify uses O(log n) stack; an iterative heapify uses O(1).
- **Time:** worst case is O(n log n), O(n²), and O(n log n) respectively.

**Examples:**
```text
sorted input, pivot = last element:
    partitions are 0 and n-1, then 0 and n-2, ...
    comparisons sum to about n²/2
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

<details>
<summary>How do you determine the space complexity of a Divide & Conquer algorithm?</summary>

### How do you determine the space complexity of a Divide & Conquer algorithm?

**Definition:**
Add the extra memory of one combine step to the deepest chain of recursive frames. The two halves are not alive together except through that chain.

**How it works:**
- Merge sort’s merge allocates a temporary array of the current range, so the auxiliary space is O(n). The recursion is also O(log n) frames, which does not change the O(n) bound.
- Quicksort partitions in place. The extra memory is the call stack: O(log n) when partitions are balanced, O(n) when the pivot is always extreme.
- Heapsort rearranges the array in place. Recursive heapify uses O(log n) stack; an iterative heapify uses O(1).
- **Space:** report both the buffers and the stack. Time bounds stay O(n log n) for merge and heap, and O(n²) in the worst case for quicksort.

**Examples:**
```text
merge of n items: two temp arrays whose lengths sum to n → O(n)
quicksort stack: one frame per pivot → O(log n) balanced, O(n) skewed
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

<details>
<summary>Why do some Divide & Conquer algorithms have a high space complexity, and how can this be mitigated?</summary>

### Why do some Divide & Conquer algorithms have a high space complexity, and how can this be mitigated?

**Definition:**
The extra space is high when every level keeps a copy of its range, or when a bad split makes the recursion a chain of n frames.

**How it works:**
- Merge sort is not in-place. The merge uses an auxiliary array, so auxiliary space is O(n). That is the cost of the stable Θ(n) merge.
- Quicksort’s naive partition also copies the array and uses O(n) extra space. Lomuto and Hoare partition in place; then only the stack remains, O(log n) in the best case and O(n) in the worst.
- Heapsort stays in the same array. Iterative heapify removes the O(log n) stack and leaves O(1) auxiliary space. Time is still O(n log n).
- **Space:** O(n) for a copying merge; O(1) auxiliary when the work is swaps in the original array and the recursion is removed.

**Examples:**
```text
mitigations that the articles actually use:
    merge sort: accept the O(n) buffer (needed for the stable merge)
    quicksort: Lomuto/Hoare instead of a copied partition
    heapsort: iterative heapify → O(1) auxiliary
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/heap-sort/

</details>

### Parallelization and Distributed Computing

<details>
<summary>Discuss how Divide & Conquer algorithms can be parallelized.</summary>

### Discuss how Divide & Conquer algorithms can be parallelized.

**Definition:**
The two recursive calls do not read each other’s output, so they can run at the same time. The merge waits until both halves are done.

**How it works:**
- Merge sort’s article calls this naturally parallel: sort the subarrays independently, then merge.
- The merge itself is a Θ(n) sequential pass over the two halves. Parallelism does not remove that pass, so the work is still O(n log n). The span (the longest chain) is the depth of the tree, O(log n) splits plus the merges along one path.
- Quicksort’s two sides are also independent after the partition. The partition is still a linear scan of the current range.
- **Time:** O(n log n) work for merge sort in every case. **Space:** O(n) auxiliary for the merges, plus one set of frames per worker.

**Examples:**
```text
parallel_merge_sort(a, lo, hi):
    if hi - lo < cutoff: sort sequentially; return
    mid = (lo + hi) // 2
    in parallel:
        parallel_merge_sort(a, lo, mid)
        parallel_merge_sort(a, mid + 1, hi)
    merge(a, lo, mid, hi)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Provide an example of a Divide & Conquer algorithm that could benefit from a distributed computing environment.</summary>

### Provide an example of a Divide & Conquer algorithm that could benefit from a distributed computing environment.

**Definition:**
External merge sort is the case the merge-sort article names for data that does not fit in memory. Each machine sorts its own run; a later merge combines the runs.

**How it works:**
- The dataset is split into pieces that fit in RAM. Each piece is sorted independently, which is the divide step and needs no communication.
- Sorted runs are then merged. The in-memory merge is O(n log n) time and O(n) auxiliary space for a run of n items.
- The same shape sorts a linked list, which the article prefers merge sort for, because the nodes are not random-access and the two halves can still be sorted separately.
- **Time:** O(n log n) for each in-memory run. **Space:** O(n) auxiliary on the machine that merges a run of n items.

**Examples:**
```text
machines 1..k:
    sort the local chunk with merge sort
coordinator:
    merge the k sorted runs, same two-way merge
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

### Miscellaneous

<details>
<summary>How does the use of randomization in Quicksort improve its performance on average?</summary>

### How does the use of randomization in Quicksort improve its performance on average?

**Definition:**
A random pivot has no fixed input pattern that always lands on the extreme element. The worst case still exists, but it is not the sorted array.

**How it works:**
- Last-element pivot hits the worst case on an already sorted array: every partition peels off one element.
- The quicksort article prefers a random pivot because there is no pattern for which that worst case happens.
- Average time stays θ(n log n). Worst time is still O(n²). Best time is Ω(n log n), when the pivot splits the array in half.
- **Space:** O(log n) stack when the tree is balanced, O(n) when it is a chain.

**Examples:**
```text
partition(a, lo, hi):
    swap(a[lo + random(0, hi - lo)], a[hi])  // hide the random pivot at the end
    then Lomuto-partition around a[hi]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/quick-sort/

</details>

<details>
<summary>Describe an instance where a Divide & Conquer algorithm is not the best choice.</summary>

### Describe an instance where a Divide & Conquer algorithm is not the best choice.

**Definition:**
The maximum-subarray divide-and-conquer is correct and faster than checking every subarray, and Kadane’s scan is still faster.

**How it works:**
- Checking every subarray is O(n²) time and O(1) space.
- The divide-and-conquer version takes the max of the left half, the right half, and the best sum that crosses the middle. That recurrence matches merge sort.
- **Time:** O(n log n) for the split, O(n) for Kadane. **Space:** O(n) for the split, O(1) for Kadane.
- The page treats the split as a good illustration, and states that Kadane is the better algorithm.

**Examples:**
```text
a = [2, 3, -8, 7, -1, 2, 3]
Kadane and the split both return 11, from [7, -1, 2, 3]
Kadane does it in one forward pass
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/maximum-subarray-sum-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Discuss the application of Divide & Conquer in non-computer-science fields.</summary>

### Discuss the application of Divide & Conquer in non-computer-science fields.

**Definition:**
A convex hull is the smallest convex polygon that contains a set of points. The divide-and-conquer construction is used outside programming, wherever a shape or a collision region is the outer boundary of points.

**How it works:**
- Sort is assumed so the set can be split into a left half and a right half. Each half’s hull is built recursively until the set is tiny (about 5 points), where a brute O(n³) hull is used.
- The two hulls are merged by the upper and lower tangents. That merge is O(n).
- The article names collision detection, pulling a shape out of an image, and drawing the outline of a scatter plot.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
robot workspace: points the arm can reach
hull: the outer polygon used as the collision shape
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/

</details>

### Practical Applications and Coding Challenges

<details>
<summary>Develop a Divide & Conquer algorithm to count inversions in an array.</summary>

### Develop a Divide & Conquer algorithm to count inversions in an array.

**Definition:**
An inversion is a pair i < j with arr[i] > arr[j]. A sorted array has 0 inversions. A reversed array has the maximum.

**How it works:**
- Split, count inversions inside each half, then count cross pairs while merging.
- During the merge, when the right value is smaller, it is smaller than every still-unused left value, so add (n1 − i) in one step.
- The merge sorts the range. Copy the array first if the caller needs the original order.
- Naive pair scan: O(n²) time and O(1) space.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
count(a, l, r):
    if l >= r: return 0
    m = (l + r) // 2
    inv = count(a, l, m) + count(a, m + 1, r)
    return inv + merge_count(a, l, m, r)

merge_count: if left[i] <= right[j]: take left
             else: take right and add (n1 - i)
[4, 3, 2, 1] → 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/counting-inversions/

</details>

<details>
<summary>Write an efficient algorithm for the Convex Hull problem using Divide & Conquer.</summary>

### Write an efficient algorithm for the Convex Hull problem using Divide & Conquer.

**Definition:**
Return the smallest convex polygon that contains every point. If the hulls of the left half and the right half are known, the full hull is their upper and lower tangents plus the outer chains.

**How it works:**
- Split by x-coordinate until a part has about 5 points or fewer. Those are solved by the brute hull, which is O(n³) locally.
- Merging the two hulls by tangents is O(n). Halving the points makes the whole algorithm O(n log n).
- A base of 3 points is the points themselves, but merging a 2-point hull with a 3-point hull can loop; the article therefore stops at 5 and uses the brute hull.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
hull(points sorted by x):
    if size <= 5: return brute_hull(points)   // O(n³) locally
    left = hull(first half)
    right = hull(second half)
    return upper_tangent(left, right) + lower_tangent(left, right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Provide an example of a real-world application where Divide & Conquer algorithms significantly improve performance.</summary>

### Provide an example of a real-world application where Divide & Conquer algorithms significantly improve performance.

**Definition:**
A length-N discrete Fourier transform written as two length-N/2 transforms is the radix-2 Cooley–Tukey step. Reusing those halves is what makes spectrum work practical.

**How it works:**
- The direct sum over all pairs is much heavier. The Danielson–Lanczos split reuses each half-length DFT in a butterfly, and the resulting cost is order N log N.
- The same idea shows up in external sorting: a dataset that does not fit in memory is sorted as independent runs and then merged, which merge sort lists as an application. Each run is O(n log n).
- **Time:** O(N log N) for the radix-2 FFT.

**Examples:**
```text
audio frame of N samples:
    even = FFT(samples 0, 2, 4, ...)
    odd = FFT(samples 1, 3, 5, ...)
    combine with N/2 butterflies
```

**Sources:**
- https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Implement a Divide & Conquer algorithm to solve the Maximum Subarray Problem.</summary>

### Implement a Divide & Conquer algorithm to solve the Maximum Subarray Problem.

**Definition:**
The answer is the best of three pieces: the best subarray entirely on the left of the middle, entirely on the right, or the best sum that crosses the middle.

**How it works:**
- One element is its own best sum.
- The crossing sum walks left from the middle (including the middle) and right from the middle + 1, then adds those two best prefixes.
- The recurrence is the same shape as merge sort.
- Checking every subarray is O(n²) time and O(1) space. Kadane’s scan is O(n) time and O(1) space, and the article calls it the better algorithm.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
max_sum(a, l, h):
    if l == h: return a[l]
    m = l + (h - l) // 2
    cross = best_sum(a, m, l, -1) + best_sum(a, m + 1, h, +1)
    return max(max_sum(a, l, m), max_sum(a, m + 1, h), cross)

[2, 3, -8, 7, -1, 2, 3] → 11
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/maximum-subarray-sum-using-divide-and-conquer-algorithm/

</details>

### Coding Challenges on Divide & Conquer Algorithms

<details>
<summary>Implement an algorithm to find the Median of Two Sorted Arrays using Divide & Conquer.</summary>

### Implement an algorithm to find the Median of Two Sorted Arrays using Divide & Conquer.

**Definition:**
Same partition search as the efficient median algorithm: discard half of the shorter array on every comparison of the two cuts, until the left and right sides of the merged sequence are balanced.

**How it works:**
- Each step throws away the half that cannot contain the median cut.
- Odd total length: the median is the max of the two left borders. Even length: average the max of the left borders and the min of the right borders.
- **Time:** O(log min(n, m)). **Space:** O(1).

**Examples:**
```text
// a = [1, 3], b = [2] → half = 2, cut i = 1 in a, j = 1 in b
// left borders 1 and 2, right border 3 → odd length → max(1, 2) = 2
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/median-of-two-sorted-arrays-of-different-sizes/

</details>

<details>
<summary>Code a Divide & Conquer solution to Merge k Sorted Lists into one sorted list.</summary>

### Code a Divide & Conquer solution to Merge k Sorted Lists into one sorted list.

**Definition:**
Split the k lists into two groups, merge each group recursively, then merge the two resulting sorted lists with the ordinary two-pointer merge.

**How it works:**
- Stop when a group has one list: that list is already sorted.
- Pairwise merging is the same merge step as merge sort.
- A linear scan for the smallest head among k lists is slower: O(n · k²) time if each list has O(n) nodes, and O(1) extra space.
- A min-heap of the current heads is O(n · k · log k) time when each list has O(n) nodes, which is also O(N log k) for N total nodes, and O(k) extra space.
- **Time:** O(n · k · log k), where n is the length of the longest list. **Space:** O(log k) for the recursion.

**Examples:**
```text
merge_k(lists, lo, hi):
    if lo == hi: return lists[lo]
    mid = (lo + hi) // 2
    left = merge_k(lists, lo, mid)
    right = merge_k(lists, mid + 1, hi)
    return merge_two(left, right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-k-sorted-linked-lists/

</details>

<details>
<summary>Develop a Divide & Conquer algorithm to solve the Maximum Subarray Problem and find the contiguous subarray with the maximum sum.</summary>

### Develop a Divide & Conquer algorithm to solve the Maximum Subarray Problem and find the contiguous subarray with the maximum sum.

**Definition:**
Same three-way split as the maximum-subarray algorithm. Return the sum; the crossing walk is what finds a subarray that sits on both sides of the cut.

**How it works:**
- Left walk includes index m and moves toward l. Right walk starts at m + 1 and moves toward h. Their sum is the best crossing subarray.
- Take the maximum of left, right, and crossing.
- **Time:** O(n log n). **Space:** O(n).
- Kadane finds the same sum in O(n) time and O(1) space.

**Examples:**
```text
a = [-2, -4] → -2   // the single element -2 beats -4 and beats -6
a = [5, 4, 1, 7, 8] → 25
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/maximum-subarray-sum-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Construct a Binary Tree from given Preorder and Inorder traversal arrays using Divide & Conquer.</summary>

### Construct a Binary Tree from given Preorder and Inorder traversal arrays using Divide & Conquer.

**Definition:**
The next preorder value is the root. Its index in the inorder array splits the remaining values into the left subtree and the right subtree.

**How it works:**
- A hash map from inorder value to index makes that lookup O(1). Without it, each root is found by a linear scan.
- Recurse on the inorder range left of the root, then on the range right of the root. The preorder index moves forward once per node.
- **Time:** O(n) with the map, O(n²) if each root is scanned in the inorder array. **Space:** O(n) with the map; the scan version uses O(h) stack.

**Examples:**
```text
build(inL, inR):
    if inL > inR: return null
    root = Node(pre[preIndex]); preIndex += 1
    i = indexOf[root.val]
    root.left = build(inL, i - 1)
    root.right = build(i + 1, inR)
    return root
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/construct-tree-from-given-inorder-and-preorder-traversal/

</details>

<details>
<summary>Construct a Binary Tree from given Inorder and Postorder traversal arrays employing a Divide & Conquer approach.</summary>

### Construct a Binary Tree from given Inorder and Postorder traversal arrays employing a Divide & Conquer approach.

**Definition:**
The next unused postorder value, read from the end, is the root. Postorder visits left, then right, then root, so the right subtree is built before the left one.

**How it works:**
- Map each inorder value to its index. Finding the root by scanning is the slow version.
- Consume postorder from the back. Build the right inorder range first, then the left range.
- **Time:** O(n) with hashing, O(n²) when every root is searched linearly. **Space:** O(n) with the map; the scan version is listed as O(n) space.

**Examples:**
```text
build(inL, inR):
    if inL > inR: return null
    root = Node(post[postIndex]); postIndex -= 1
    i = indexOf[root.val]
    root.right = build(i + 1, inR)
    root.left = build(inL, i - 1)
    return root
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/construct-a-binary-tree-from-postorder-and-inorder/

</details>

<details>
<summary>Convert a Sorted Array into a height-balanced Binary Search Tree using Divide & Conquer.</summary>

### Convert a Sorted Array into a height-balanced Binary Search Tree using Divide & Conquer.

**Definition:**
The middle element becomes the root. The left half becomes the left subtree and the right half becomes the right subtree, so the two sides differ by at most one node.

**How it works:**
- An empty range is a null child.
- Choosing the middle keeps about half the values on each side, which is why the tree stays height-balanced.
- The same construction can be done with a queue instead of recursion. Both expected approaches on the page are O(n).
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
build(a, lo, hi):
    if lo > hi: return null
    mid = (lo + hi) // 2
    root = Node(a[mid])
    root.left = build(a, lo, mid - 1)
    root.right = build(a, mid + 1, hi)
    return root

[1, 2, 3, 4, 5, 6, 7] → root 4, left 2, right 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sorted-array-to-balanced-bst/

</details>

<details>
<summary>Convert a Sorted Linked List into a height-balanced Binary Search Tree with a Divide & Conquer method.</summary>

### Convert a Sorted Linked List into a height-balanced Binary Search Tree with a Divide & Conquer method.

**Definition:**
The list is already sorted, so an inorder walk of the tree is the list itself. Build the left half, take the next list node as the root, then build the right half.

**How it works:**
- Count the nodes once. A call that owns n nodes builds the left subtree with n/2 nodes, makes the current head the root, advances the head, and builds the right subtree with n − n/2 − 1 nodes.
- Copying the list into an array and then taking middles is also correct: O(n) time and O(n) space. For an even length the array version on the page picks the second middle, (start + end + 1) / 2.
- **Time:** O(n). **Space:** O(log n) for the inorder recursion. The array version is O(n) space.

**Examples:**
```text
1 → 2 → 3 → 4 → 5 → 6 → 7
root 4, left subtree from the first 3 nodes, right from the last 3
preorder: 4 2 1 3 6 5 7
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sorted-linked-list-to-balanced-bst/

</details>

<details>
<summary>Create a Divide & Conquer algorithm to Sort a linked list.</summary>

### Create a Divide & Conquer algorithm to Sort a linked list.

**Definition:**
Merge sort is the sort the merge-sort article prefers for a linked list. Split the node chain in half, sort each half, then merge by relinking nodes.

**How it works:**
- A list has no random access, which is why the article says quicksort and heapsort do poorly on it.
- The split finds the middle with two pointers, sorts both sides, and merges them so that the smaller head is taken first. Equal keys keep the left one, which is what makes the array merge stable (L[i] <= R[j]).
- **Time:** O(n log n) in the best, average, and worst case. **Space:** the array algorithm uses O(n) auxiliary memory for the temporary arrays. The recursion depth of the even split is O(log n).

**Examples:**
```text
sort(head):
    if head is null or head.next is null: return head
    left, right = split_in_half(head)
    return merge(sort(left), sort(right))
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Implement an efficient algorithm to Search a 2D Matrix II with a Divide & Conquer approach.</summary>

### Implement an efficient algorithm to Search a 2D Matrix II with a Divide & Conquer approach.

**Definition:**
Every row and every column is sorted. Compare the middle cell with the key, then search only the quarters that can still hold it.

**How it works:**
- If the middle is smaller than the key, the top-left cannot contain it, so the search continues in the lower band and the upper-right quarter.
- If the middle is larger, the bottom-right cannot contain it, so the search continues in the left band and the upper-right quarter.
- Modeled as three subproblems of size n/2, the recurrence is T(n) = 3T(n/2) + O(1).
- **Time:** O(n^1.58) by the Master theorem, for an n×n matrix. **Space:** O(log n).
- A staircase walk from a corner, on the related row-and-column sorted search, is O(n + m) time and O(1) space. That is a different algorithm.

**Examples:**
```text
search(mat, r1, r2, c1, c2, key):
    if r1 > r2 or c1 > c2: return false
    i = (r1 + r2) // 2
    j = (c1 + c2) // 2
    if mat[i][j] == key: return true
    if search upper-right quarter: return true
    if mat[i][j] < key: return search rows i+1..r2
    return search columns c1..j-1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/search-in-a-row-wise-and-column-wise-sorted-2d-array-using-divide-and-conquer-algorithm/

</details>

<details>
<summary>Code a Divide & Conquer solution for Count of Smaller Numbers After Self in an array.</summary>

### Code a Divide & Conquer solution for Count of Smaller Numbers After Self in an array.

**Definition:**
For each index, count how many later elements are smaller. Those pairs are inversions. The merge that counts inversions can record them per index instead of only as a total.

**How it works:**
- Split, then merge two sorted halves. The right half holds later indexes.
- When the next right value is smaller, it is smaller than every still-unused left value, so the inversion merge adds (n1 − i) at once. Put that increment on each of those left indexes.
- The merge sorts the range. Copy the array if the original order must stay.
- Pair scan: O(n²) time and O(1) extra space.
- **Time:** O(n log n). **Space:** O(n).

**Examples:**
```text
[5, 2, 6, 1]
5 is greater than 2 and 1 → 2
2 is greater than 1 → 1
6 is greater than 1 → 1
1 → 0
result [2, 1, 1, 0]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/counting-inversions/
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Use Divide & Conquer to calculate the Count of Range Sum in an array for given lower and upper bounds.</summary>

### Use Divide & Conquer to calculate the Count of Range Sum in an array for given lower and upper bounds.

**Definition:**
Count contiguous subarrays whose sum lies in [l, r]. Prefix sums turn that into pairs: a later prefix minus an earlier prefix falls inside [l, r].

**How it works:**
- Sort the prefix halves with merge sort. Before the merge, both halves are sorted, so two pointers count, for each left prefix, how many right prefixes sit in [left + l, left + r].
- Add the counts from the two halves and the cross pairs.
- Nested loops are O(n²) time and O(1) space.
- The page’s other expected method slides a window and subtracts “sum < l” from “sum ≤ r”: O(n) time and O(1) space.
- **Time:** O(n log n) for the prefix merge. **Space:** O(n).

**Examples:**
```text
l = 4, r = 13, a = [2, 3, 5, 8]
subarrays in range: [2, 3], [2, 3, 5], [3, 5], [5], [5, 8], [8] → 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/number-of-subarrays-having-sum-in-a-given-range/

</details>

<details>
<summary>Find the Top K Frequent Elements in an array using a Divide & Conquer strategy.</summary>

### Find the Top K Frequent Elements in an array using a Divide & Conquer strategy.

**Definition:**
Count how often each value appears, then select the k highest counts. The selection is Quickselect: partition around a pivot and recurse into only the side that still holds the cut.

**How it works:**
- One pass builds the frequency of each distinct value.
- Quickselect puts values greater than the pivot on the left. If the pivot lands at index k − 1, the first k entries are the top. Otherwise only one side is searched.
- On that page the worst case is O(n²), and the average is O(n log n). Auxiliary space is O(n) in the worst case and O(log n) on average. Here n is the number of distinct values being partitioned.
- The same page’s heap method keeps k extremes in O(n log k) time and O(k) space. That is a different algorithm.
- **Time:** average O(n log n) and worst O(n²) for the Quickselect step, as stated for this partition. **Space:** average O(log n), worst O(n), plus the frequency map.

**Examples:**
```text
a = [1, 1, 1, 2, 2, 3], k = 2
counts: 1 → 3, 2 → 2, 3 → 1
partition by count; the cut at k - 1 keeps 1 and 2
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/kth-smallest-largest-element-in-unsorted-array/

</details>

<details>
<summary>Determine the Kth Largest Element in an unsorted array with a Divide & Conquer technique.</summary>

### Determine the Kth Largest Element in an unsorted array with a Divide & Conquer technique.

**Definition:**
Quickselect partitions the array so that elements greater than the pivot sit on its left. The pivot’s index is then compared with k − 1, and only the side that contains the kth largest is searched.

**How it works:**
- If the pivot lands at index k − 1, it is the answer.
- Sorting the whole array first is O(n log n) time. The page lists that sort as O(1) extra space.
- A max-heap of the k candidates is the expected approach on the same page: O(n log k) time and O(k) space.
- **Time:** O(n²) worst, average O(n log n), and the page says that average beats the heap. **Space:** O(n) for the recursion stack in the worst case, O(log n) on average.

**Examples:**
```text
a = [10, 5, 4, 3, 48, 6, 2, 33, 53, 10], k = 4
partition greater values to the left
stop when the pivot index is 3
→ 5
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/kth-smallest-largest-element-in-unsorted-array/

</details>

<details>
<summary>Implement a Divide & Conquer algorithm to compute the Number of Ships in a Rectangle on a 2D plane, given top-right and bottom-left coordinates of rectangles.</summary>

### Implement a Divide & Conquer algorithm to compute the Number of Ships in a Rectangle on a 2D plane, given top-right and bottom-left coordinates of rectangles.

**Definition:**
`hasShips(topRight, bottomLeft)` says whether a rectangle, boundary included, holds at least one ship. Each integer point holds at most one ship. The rectangle holds at most 10 ships.

**How it works:**
- An empty `hasShips` answer is 0. That quadrant is not split.
- A single cell that contains a ship counts 1.
- Otherwise the rectangle is split into four quadrants, and the four counts are added.
- **Time:** O(C × log max(m, n)). **Space:** O(log max(m, n)). C is the number of ships; m and n are the side lengths.

**Examples:**
```text
ships at (1,1), (2,2), (3,3), (5,5)
rectangle [0,0] .. [4,4]
(5,5) is outside → 3

count(topRight, bottomLeft):
    if the rectangle is inside-out: return 0
    if not hasShips: return 0
    if one cell: return 1
    split at the mid x and mid y
    return the sum of the four quadrants
```

**Sources:**
- https://github.com/doocs/leetcode/blob/main/solution/1200-1299/1274.Number%20of%20Ships%20in%20a%20Rectangle/README_EN.md

</details>

<details>
<summary>Balance a given Binary Search Tree into a balanced BST using Divide & Conquer.</summary>

### Balance a given Binary Search Tree into a balanced BST using Divide & Conquer.

**Definition:**
An inorder walk of a BST is a sorted sequence. The middle value of that sequence becomes the new root, and the two halves become the two subtrees.

**How it works:**
- Collect the nodes in inorder. That pass is linear in the number of nodes.
- The sorted-array construction picks the middle as the root so each side holds about half of the values. Both sides then differ by at most one node, which keeps the tree height-balanced.
- A queue can build the same shape. The page’s recursive and queue versions are both O(n).
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
inorder = [1, 2, 3, 4, 5, 6, 7]
root = 4
left subtree built from [1, 2, 3], right from [5, 6, 7]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sorted-array-to-balanced-bst/

</details>

<details>
<summary>Create a Sorted Array through a series of Instructions using a Divide & Conquer and Binary Indexed Tree.</summary>

### Create a Sorted Array through a series of Instructions using a Divide & Conquer and Binary Indexed Tree.

**Definition:**
Insert `instructions[i]` from left to right. The cost of an insertion is the minimum of how many already inserted values are strictly smaller and how many are strictly greater. Return the sum of costs modulo 10^9 + 7.

**How it works:**
- A Fenwick tree (binary indexed tree) stores how many times each value has been inserted.
- `query(num - 1)` is the count of strictly smaller values. `i - query(num)` is the count of strictly greater values, because i values are already inserted and `query(num)` includes equals.
- Add `min(smaller, greater)`, then `update(num, 1)`.
- Scanning the inserted values each time is O(N²).
- **Time:** O(N log M), because each update and query is O(log M) and M is the maximum value. **Space:** O(M).

**Examples:**
```text
instructions = [1, 5, 6, 2]
insert 1 → cost 0
insert 5 → 1 smaller, 0 greater → 0
insert 6 → 2 smaller, 0 greater → 0
insert 2 → 1 smaller, 2 greater → min = 1
total 1
```

**Sources:**
- https://algomap.io/question-bank/create-sorted-array-through-instructions

</details>

<details>
<summary>Use a Divide & Conquer approach to Sort an Array.</summary>

### Use a Divide & Conquer approach to Sort an Array.

**Definition:**
Merge sort divides the array until every piece has one element, then merges those sorted pieces.

**How it works:**
- T(n) = Θ(1) when n = 1, and T(n) = 2T(n/2) + Θ(n) otherwise. The Θ(n) term is the merge.
- **Time:** O(n log n) in the best, average, and worst case. **Space:** O(n) for the temporary arrays.
- The merge is stable: when the heads are equal, the left one is taken. The two halves can be sorted independently, which is why the article calls it naturally parallel.

**Examples:**
```text
[38, 27, 43, 10]
→ [38, 27] and [43, 10]
→ [27, 38] and [10, 43]
→ [10, 27, 38, 43]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/

</details>

<details>
<summary>Design and implement a Divide & Conquer solution to generate a Beautiful Array.</summary>

### Design and implement a Divide & Conquer solution to generate a Beautiful Array.

**Definition:**
A beautiful array of length n is a permutation of 1..n such that no value sits strictly between two others whose average it is: there is no i < k < j with 2 * nums[k] = nums[i] + nums[j].

**How it works:**
- If A is beautiful, so are the odd image 2·A − 1 and the even image 2·A.
- Build the odds from a beautiful array of size (n + 1) / 2 and the evens from one of size n / 2, then concatenate odds and evens.
- One element, [1], is beautiful.
- Trying every permutation is O(n! × n³) time and O(n) space.
- **Time:** O(n log n). **Space:** O(n log n).

**Examples:**
```text
beautiful(1) = [1]
beautiful(4) = odds from 2, evens from 2 → [1, 3, 2, 4]
beautiful(5) = odds from 3, evens from 2 → [1, 5, 3, 2, 4]

beautiful(n):
    if n == 1: return [1]
    odds = [2*x - 1 for x in beautiful((n + 1) // 2)]
    evens = [2*x for x in beautiful(n // 2)]
    return odds + evens
```

**Sources:**
- https://www.tutorialspoint.com/practice/beautiful-array.htm

</details>

<details>
<summary>Solve the Longest Increasing Subsequence II problem where updates and queries happen in real-time, using a Divide & Conquer strategy combined with advanced data structures.</summary>

### Solve the Longest Increasing Subsequence II problem where updates and queries happen in real-time, using a Divide & Conquer strategy combined with advanced data structures.

**Definition:**
The length of the longest strictly increasing subsequence can be maintained online. Each new value asks for the best length among smaller values, then publishes its own length.

**How it works:**
- Coordinate compression maps the values onto 1..N so the tree stays size N.
- A binary indexed tree stores the best length at each compressed value. `query(index - 1)` is the best length among strictly smaller values. `update` writes length + 1 and walks the tree with `index += index & -index`.
- Process the array from left to right. That is one query and one update per element.
- **Time:** O(N log N). **Space:** O(N).

**Examples:**
```text
[6, 5, 1, 3, 2, 4, 8, 7]
lengths become 1, 1, 1, 2, 2, 3, 4, 4
answer 4   // for example 1, 3, 4, 7
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/longest-increasing-subsequence-using-bit/

</details>
