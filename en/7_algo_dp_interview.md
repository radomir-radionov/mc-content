# Dynamic Programming — Interview

Interview answers in this file are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Dynamic Programming — Interview](./7_algo_dp_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Dynamic Programming](https://devinterview.io/questions/data-structures-and-algorithms/dynamic-programming-interview-questions/).

### Basic Dynamic Programming Concepts

<details>
<summary>What is Dynamic Programming and how does it differ from recursion?</summary>

### What is Dynamic Programming and how does it differ from recursion?

**Definition:**

Dynamic programming solves a recurrence by keeping each overlapping subproblem’s answer. Plain recursion recomputes that answer every time the call tree asks for it.

**How it works:**

- Recursion shrinks the input until a base case. Nothing is stored, so the same call can run exponentially often.
- DP uses the same recurrence, but each distinct state is evaluated a constant number of times and then reused.
- Top-down DP is recursion plus a memo table: compute a state when it is first needed.
- Bottom-up DP fills a table from the smallest states toward the answer, usually with a loop.
- Fibonacci shows the split. The naive tree calls `fib(k)` once per path that reaches it. The memoized or tabulated version calls it once.

**Examples:**

`fib(5)` naively expands into two calls of `fib(4)` and `fib(3)`, and those repeat `fib(2)`. A table `t[i] = t[i-1] + t[i-2]` writes each index one time.
</details>

<details>
<summary>Can you explain the concept of overlapping subproblems?</summary>

### Can you explain the concept of overlapping subproblems?

**Definition:**

Overlapping subproblems means the same smaller problem shows up on many branches of the recursion, so a stored answer can be reused.

**How it works:**

- The repeat is what makes a cache pay. If every subproblem is unique, as with the two halves of merge sort, a table does not remove work.
- Fibonacci is the textbook overlap: `fib(n)` and `fib(n-1)` both need `fib(n-2)`.
- Edit distance has the same shape. The distance between a prefix of “Saturday” and a prefix of “Sunday” is reached by more than one sequence of insert, delete, and replace.
- Optimal substructure is the partner property: an optimal answer is built from optimal answers to those repeated pieces. Overlap alone is not enough if the pieces do not compose that way.

**Examples:**

`fib(6)` calls `fib(4)` from both `fib(6)` and `fib(5)`. One stored `fib(4)` replaces a whole subtree.
</details>

<details>
<summary>Define memoization and how it improves performance in Dynamic Programming.</summary>

### Define memoization and how it improves performance in Dynamic Programming.

**Definition:**

Memoization stores the result of a call under its arguments and returns that result when the same arguments appear again. In DP it is the top-down way to fill the state table.

**How it works:**

- The first call does the work and writes the cache. Later calls are a lookup.
- The code stays close to the recurrence, without an explicit loop over every state.
- You only compute states the original call actually reaches, which can beat a bottom-up table that fills a larger rectangle.
- The costs are the cache itself, the recursion depth, and any policy for dropping stale entries if the function is not a pure function of its arguments.
- For Fibonacci the cache turns an exponential tree into one chain of new states, Θ(n) time and Θ(n) entries.

**Examples:**

```text
fib(n):
  if n in cache: return cache[n]
  if n < 2: return n
  cache[n] = fib(n-1) + fib(n-2)
  return cache[n]
```
</details>

<details>
<summary>What is the difference between top-down and bottom-up approaches in Dynamic Programming?</summary>

### What is the difference between top-down and bottom-up approaches in Dynamic Programming?

**Definition:**

Top-down starts at the requested answer and recurses, caching each state. Bottom-up starts at the base cases and loops until the requested answer is filled.

**How it works:**

- Top-down is memoized recursion. The call `fib(n)` asks for `fib(n-1)` and `fib(n-2)`, and the map stops the repeat.
- Bottom-up allocates `t[0] = 0`, `t[1] = 1`, then sets `t[i] = t[i-1] + t[i-2]` for `i` from 2 to n.
- Both use the same recurrence and the same asymptotic time when every state is needed. Top-down can skip states that nothing asks for. Bottom-up has no call stack and a predictable loop.
- Bottom-up makes the dependency order obvious: a cell is written only after the cells it reads. Top-down discovers that order through the recursion.

**Examples:**

`fib(4)` top-down calls 4, then 3, then 2, then stores 2, 3, and 4 on the way back. Bottom-up writes indices 2, 3, and 4 in that order and never builds a stack of calls.
</details>

<details>
<summary>Explain the importance of state definition in Dynamic Programming solutions.</summary>

### Explain the importance of state definition in Dynamic Programming solutions.

**Definition:**

The state is the smallest tuple of parameters that identifies a subproblem. A wrong tuple either cannot be recursed, or it stores more than the recurrence needs.

**How it works:**

- A clear state is how you name the overlapping pieces and write the transition between them.
- Keeping only the fields the transition reads is what bounds the table. Extra fields multiply the memory.
- The same discipline produces shorter code: one function or one loop per state, not a pile of special cases.
- Optimization states usually store the best value so far, such as max value for a knapsack capacity.
- Counting states store how many ways reach that tuple.
- Decision states store a yes or no, such as “can this prefix form the amount?”.

**Examples:**

0/1 knapsack: state `(i, w)` is the best value using the first `i` items with capacity `w`. Dropping `i` makes the transition ambiguous, because you no longer know which item you are deciding.
</details>

### Fundamental Dynamic Programming Problems

<details>
<summary>Compute the Fibonacci sequence using Dynamic Programming.</summary>

### Compute the Fibonacci sequence using Dynamic Programming.

**Definition:**

The nth Fibonacci number is the classic one-dimensional DP: `F(i) = F(i-1) + F(i-2)` with `F(0) = 0` and `F(1) = 1`.

**How it works:**

- Tabulation allocates an array and writes the base cases first.
- For `i` from 2 through n, set `fib[i]` to the sum of the two previous cells.
- Time is Θ(n), against the exponential tree of the naive recursion.
- Space is Θ(n) for the array. Two rolling variables cut that to Θ(1) if you only need `F(n)`, not the whole sequence.
- The loop order is the proof the dependency is satisfied: a cell is written only after both parents exist.

**Examples:**

```text
fib = [0, 1]
for i in 2..n: fib[i] = fib[i-1] + fib[i-2]
return fib[n]
```

`n = 6` writes `1, 2, 3, 5, 8`.
</details>

<details>
<summary>Implement a solution to the Coin Change Problem using DP.</summary>

### Implement a solution to the Coin Change Problem using DP.

**Definition:**

Coin change (fewest coins) asks for the minimum number of coins from a denomination list that sum to `amount`.

**How it works:**

- Let `dp[x]` be the fewest coins that sum to `x`. Set `dp[0] = 0` and every other cell to a sentinel larger than `amount`, such as `amount + 1`.
- For each amount `x` from 1 to `amount`, and each coin `c`, if `c <= x` then `dp[x] = min(dp[x], 1 + dp[x - c])`.
- The `1` is the coin you just took. `dp[x - c]` is the best way to make the rest.
- If `dp[amount]` is still the sentinel, no combination works.
- Time is Θ(amount × number of coins). Space is Θ(amount).
- This order allows unlimited reuse of a coin. The 0/1 variant would iterate coins on the outside and amounts downward.

**Examples:**

Coins `[1, 2, 5]`, amount `11`. One optimal pick is `5 + 5 + 1`, so the answer is 3. `dp[11]` ends at 3.
</details>

<details>
<summary>Use Dynamic Programming approach to solve the 0/1 Knapsack problem.</summary>

### Use Dynamic Programming approach to solve the 0/1 Knapsack problem.

**Definition:**

The 0/1 knapsack takes each item at most once. Choose a subset whose weight is at most `W` and whose value is maximum.

**How it works:**

- State `dp[i][w]` is the best value using the first `i` items and capacity `w`.
- The table has `(n + 1)` rows and `(W + 1)` columns. Row 0 and column 0 stay 0.
- If item `i` weighs more than `w`, copy `dp[i-1][w]`. Otherwise take the max of skipping it and of `value[i] + dp[i-1][w - weight[i]]`.
- The answer is `dp[n][W]`. Walk back from that cell, and whenever the value changed from the row above, that item was taken.
- Time and the full table are Θ(n · W). One rolling row of length `W + 1`, filled from the right, keeps the same time and Θ(W) memory.

**Examples:**

Items `(weight 2, value 3)` and `(weight 3, value 4)`, capacity 5. Taking both exceeds 5. The table’s best cell is 4, the second item alone, unless a third item fits beside the first.
</details>

<details>
<summary>Implement an algorithm for computing the longest common subsequence of two sequences.</summary>

### Implement an algorithm for computing the longest common subsequence of two sequences.

**Definition:**

A longest common subsequence is the longest sequence that appears in both inputs in the same order, not necessarily as a contiguous slice.

**How it works:**

- Let the strings have lengths `m` and `n`. `L[i][j]` is the LCS length of the first `i` characters of `X` and the first `j` of `Y`.
- Start from a zero border. If `X[i-1] == Y[j-1]`, then `L[i][j] = L[i-1][j-1] + 1`. Otherwise `L[i][j] = max(L[i-1][j], L[i][j-1])`.
- The length is `L[m][n]`. To recover one subsequence, walk from that cell: on a match, emit the character and step diagonally; otherwise step toward the larger neighbor.
- Time and space are Θ(m n) for the full matrix. Two rolling rows keep the length in Θ(min(m, n)) memory; reconstructing the string still needs a way to remember the choices.

**Examples:**

`X = ABCD`, `Y = ACBD`. One LCS is `ABD`, length 3. `ACD` is another of the same length.
</details>

<details>
<summary>Solve matrix chain multiplication problem with Dynamic Programming.</summary>

### Solve matrix chain multiplication problem with Dynamic Programming.

**Definition:**

Matrix chain multiplication chooses where to parenthesize a product `A1 A2 … An` so the number of scalar multiplications is minimized. The matrices’ dimensions are compatible; only the association changes the cost.

**How it works:**

- State `(i, j)` is the cheapest way to multiply the chain from matrix `i` through matrix `j`.
- A single matrix costs 0. A split after matrix `k` costs `dp[i][k] + dp[k+1][j]` plus `rows(i) * cols(k) * cols(j)`.
- Fill by increasing chain length. For each length, try every start `i` and every split `k`.
- The optimal association of a chain uses an optimal association on each side of that split. The table’s argmin records the `k` if you need the parenthesization, not only the cost.
- Time is Θ(n³) from the three nested loops. The table is Θ(n²).

**Examples:**

Dimensions `10×30`, `30×5`, `5×60`. Splitting after the first matrix costs `10*30*5 + 10*5*60 = 4500`. Splitting after the second costs `30*5*60 + 10*30*60 = 27000`. The DP picks 4500.
</details>

### Optimizing Dynamic Programming Solutions

<details>
<summary>What are the trade-offs between memoization and tabulation in Dynamic Programming?</summary>

### What are the trade-offs between memoization and tabulation in Dynamic Programming?

**Definition:**

Memoization is the cached top-down recurrence. Tabulation is the bottom-up loop that writes every dependent state before the caller needs it.

**How it works:**

- Memoization starts at the original problem and computes a subproblem only when a call reaches it. That skips unreachable states.
- Its drawbacks are the recursion overhead and a call stack that can be as deep as the longest chain of states.
- Tabulation has no recursive stack. The loop order has to match the dependencies, which is harder when those dependencies are not a simple index order.
- Tabulation often fills states nobody needed, whenever the table is larger than the set of reachable states.
- Time is the same order when both end up touching the same states. Pick memoization when the reachable set is sparse or the recurrence is clearer as a function. Pick tabulation when you want a bounded stack and a tight rolling array.

**Examples:**

A knapsack whose capacities jump by large weights may never visit most `w` values from the top. The bottom-up row still writes every capacity.
</details>

<details>
<summary>How do you determine the time complexity of a Dynamic Programming algorithm?</summary>

### How do you determine the time complexity of a Dynamic Programming algorithm?

**Definition:**

DP time is the number of states times the work to compute one state from the states it already depends on. Space is the number of states you keep.

**How it works:**

- Count the distinct tuples. A 1D index `i ≤ n` is Θ(n) states. A pair `(i, j)` over lengths `m` and `n` is Θ(m n).
- Multiply by the transitions tried per state. Coin change tries every coin, so the bound is Θ(amount × coins), not Θ(amount).
- Memoized recursion has the same bound once each state is cached, even if the uncached tree was exponential. The stack is extra space on top of the cache.
- A 1D bottom-up loop is often Θ(n × work per cell). A 2D table is Θ(m n × work per cell) time and Θ(m n) memory unless you roll a row away.
- Shrinking the state changes both bounds: two rolling Fibonacci variables, a sliding window of the last row, or iterating a knapsack row backward so one array is enough.

**Examples:**

Matrix chain: Θ(n²) subchains, and each tries Θ(n) splits, so Θ(n³) time. Fibonacci: Θ(n) states and Θ(1) work each, so Θ(n).
</details>

<details>
<summary>Describe techniques to optimize space complexity in Dynamic Programming problems.</summary>

### Describe techniques to optimize space complexity in Dynamic Programming problems.

**Definition:**

Space drops when a state only reads a thin slice of earlier states, so the rest of the table never has to exist at once.

**How it works:**

- Rolling variables: Fibonacci needs only the last two numbers, so the Θ(n) array becomes two words.
- Rolling rows: LCS and edit distance read only the previous row (and the current cell’s left neighbor). Keep two rows, or even one row plus a diagonal variable.
- 0/1 knapsack reads `dp[i-1][w - weight]` while filling row `i`. Iterate `w` downward in a single array so the values you still need have not been overwritten.
- Interleaving is the same idea on a grid: compute one diagonal or one row, then discard it.
- Do not memoize a huge state if the decision depends only on the current position. A maze cell does not need a copy of the whole path.
- Bitsets pack a boolean row, which is how some string and knapsack DP cuts the constant, not the asymptotic shape, when the state is a set of reachable flags.

**Examples:**

Edit distance between strings of length `n` can keep two arrays of length `n + 1` instead of an `(n+1) × (n+1)` matrix. The distance is the last cell; the full alignment still needs the choices or the full table.
</details>

<details>
<summary>What is state transition, and how do you derive state transition relations?</summary>

### What is state transition, and how do you derive state transition relations?

**Definition:**

A state transition is the rule that builds one state’s answer from a few earlier states. Deriving it means naming the state, the legal moves, the start, and the stop.

**How it works:**

- Write down the fields that must be known to continue. That tuple is the state.
- List the choices at that tuple and the state each choice lands in. The relation is the min, max, or sum over those landings.
- The initial state is the empty prefix, zero items, or the base Fibonacci pair. It has to match the real start of the problem.
- A terminal state is where no choice remains: the capacity is exhausted, the string is consumed, or the index has reached `n`.
- The table is only a memory for those relations. Without a correct relation, filling order does not matter.
- Knapsack’s relation is the pattern: from `(i, w)` you either stay at `(i-1, w)` or, when the item fits, add its value to `(i-1, w - weight)`.

**Examples:**

```text
dp[i][w] = dp[i-1][w]                         if weight[i] > w
dp[i][w] = max(dp[i-1][w],
               value[i] + dp[i-1][w-weight])  otherwise
```
</details>

### String-related Dynamic Programming Questions

<details>
<summary>Find the longest palindromic substring in a given string.</summary>

### Find the longest palindromic substring in a given string.

**Definition:**

The longest palindromic substring is the longest contiguous slice that reads the same forward and backward. DP records whether each slice is a palindrome, then keeps the longest one that is.

**How it works:**

- `dp[i][j]` is true when `s[i..j]` is a palindrome.
- Every length-1 slice is true. A length-2 slice is true when the two characters match.
- For a longer slice, `dp[i][j]` is true when `s[i] == s[j]` and `dp[i+1][j-1]` is true. Fill by increasing length so the inner slice already exists.
- Track the `(i, j)` with the greatest `j - i` among the true cells.
- Time and the boolean table are Θ(n²). Expanding around each center finds the same answer in Θ(n²) time and Θ(1) extra memory; the DP table is the version that makes the dependency explicit.

**Examples:**

`s = "babad"`. Both `"bab"` and `"aba"` are palindromes of length 3, and nothing longer is. Either one is a correct answer.
</details>

<details>
<summary>Provide a Dynamic Programming solution for the edit distance problem.</summary>

### Provide a Dynamic Programming solution for the edit distance problem.

**Definition:**

Edit distance here is the Levenshtein distance: the minimum number of single-symbol insertions, deletions, and substitutions that turn one string into the other. Each edit has unit cost. Wikipedia gives the Wagner–Fischer recurrence on prefixes.

**How it works:**

- `D(i, 0) = i` and `D(0, j) = j`, because a prefix becomes empty only by deletions or insertions.
- `D(i, j)` is the minimum of deleting `a[i]` (`D(i-1, j) + 1`), inserting `b[j]` (`D(i, j-1) + 1`), and matching or substituting (`D(i-1, j-1)` plus 0 or 1).
- The full matrix fills every pair of prefixes, so the time is Θ(|a| · |b|). Wikipedia states that the distance alone needs only the previous row, which reduces working memory to O(min(|a|, |b|)).
- `kitten` to `sitting` is 3. The distance is a metric under unit costs.

**Examples:**

```text
def edit_distance(a, b):
    prev = list(range(len(b) + 1))
    for i, ca in enumerate(a, 1):
        cur = [i]
        for j, cb in enumerate(b, 1):
            cost = 0 if ca == cb else 1
            cur.append(min(prev[j] + 1, cur[-1] + 1, prev[j - 1] + cost))
        prev = cur
    return prev[-1]
```

`edit_distance("kitten", "sitting")` is 3.

**Sources:**

- [Wikipedia — Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)
</details>

<details>
<summary>Explain Dynamic Programming on strings with the example of word break problem.</summary>

### Explain Dynamic Programming on strings with the example of word break problem.

**Definition:**

Word break asks whether a string is a concatenation of dictionary words, and a word may be reused. GeeksforGeeks times the bottom-up DP at O(n · m · L), where n is the string length, m the dictionary size, and L the word length, with O(n) extra memory. Trying every cut with no memory is O(2^n).

**How it works:**

- `dp[i]` is true when `s[0:i]` can be segmented. `dp[0]` is true.
- For each end `i` and each word, if the word matches the slice that ends at `i` and the prefix before it is breakable, set `dp[i]`.
- A trie plus DP, the page’s expected approach, is O(n · L + m · L) time.
- Overlapping prefixes are why the boolean array beats raw recursion: `"ilike"` reuses the decision at the cut after `"i"`.

**Examples:**

```text
def word_break(s, words):
    words = set(words)
    dp = [False] * (len(s) + 1)
    dp[0] = True
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in words:
                dp[i] = True
                break
    return dp[-1]
```

`word_break("ilike", ["i", "like", "gfg"])` is true. `word_break("ilikemangoes", ["i", "like", "man", "india", "gfg"])` is false.

**Sources:**

- [GeeksforGeeks — Word Break](https://www.geeksforgeeks.org/dsa/word-break-problem-dp-32/)
</details>

### Sequence and Subsequence related Problems

<details>
<summary>Solve the Longest Increasing Subsequence problem using Dynamic Programming.</summary>

### Solve the Longest Increasing Subsequence problem using Dynamic Programming.

**Definition:**

The longest increasing subsequence is the longest subsequence that is strictly increasing. GeeksforGeeks times the length DP at O(n²) time and O(n) space. A binary-search form on tails is O(n log n) time and O(n) space.

**How it works:**

- `L[i]` is the length of the longest increasing subsequence that ends at `i`. It is 1 plus the best `L[prev]` with `prev < i` and `arr[prev] < arr[i]`, or 1 if no such `prev` exists.
- The answer is the maximum `L[i]`, not `L[n - 1]`.
- `[3, 10, 2, 1, 20]` has length 3, from `3, 10, 20`. Equal values do not extend a strictly increasing chain: `[2, 2, 2]` has length 1.
- The O(n log n) method keeps the smallest possible tail of every length and binary-searches where the next value fits. It computes the length; reconstructing the sequence needs extra parent links.

**Examples:**

```text
def lis(arr):
    n = len(arr)
    L = [1] * n
    for i in range(n):
        for prev in range(i):
            if arr[prev] < arr[i]:
                L[i] = max(L[i], L[prev] + 1)
    return max(L, default=0)
```

`lis([3, 10, 2, 1, 20])` is 3. `lis([3, 4, 5, 1, 2, 3, 4])` is 4.

**Sources:**

- [GeeksforGeeks — Longest increasing subsequence](https://www.geeksforgeeks.org/dsa/longest-increasing-subsequence-dp-3/)
</details>

<details>
<summary>Solve the Maximum Sum Increasing Subsequence problem with DP.</summary>

### Solve the Maximum Sum Increasing Subsequence problem with DP.

**Definition:**

This is the increasing-subsequence DP with sums instead of lengths. GeeksforGeeks times the bottom-up table at O(n²) time and O(n) space, and an optimized form at O(n log n) time and O(n) space. Recursion without memory is O(2^n).

**How it works:**

- `ms[i]` is the best sum of a strictly increasing subsequence that ends at `i`. It starts as `arr[i]`.
- Extend it from any earlier `prev` with `arr[prev] < arr[i]`: `ms[i] = max(ms[i], ms[prev] + arr[i])`.
- The answer is the maximum `ms[i]`. `[1, 101, 2, 3, 100]` yields 106 from `[1, 2, 3, 100]`, not 102 from `[1, 101]`.
- The page assumes positive integers, so a longer chain of positives cannot lose to a shorter one only by length; the objective is still the sum.

**Examples:**

```text
def msis(arr):
    ms = arr[:]
    for i in range(len(arr)):
        for prev in range(i):
            if arr[prev] < arr[i]:
                ms[i] = max(ms[i], ms[prev] + arr[i])
    return max(ms, default=0)
```

`msis([1, 101, 2, 3, 100])` is 106. `msis([4, 1, 2, 3])` is 6.

**Sources:**

- [GeeksforGeeks — Maximum sum increasing subsequence](https://www.geeksforgeeks.org/dsa/maximum-sum-increasing-subsequence-dp-14/)
</details>

<details>
<summary>Implement an algorithm for finding the number of subsequences with a given sum in an array.</summary>

### Implement an algorithm for finding the number of subsequences with a given sum in an array.

**Definition:**

Count index-sets whose values sum to `target`. It is the subset-sum DP with counters instead of booleans. GeeksforGeeks fills the boolean table in O(sum · n) time; the count table has the same shape and the same bound. The boolean form’s expected space is O(sum); the count form needs the same one-dimensional array of integers.

**How it works:**

- `dp[s]` is the number of subsets of the prefix seen so far that sum to `s`. `dp[0]` starts at 1, the empty subset.
- Each value `x` is used at most once: walk the sums downward from `target` to `x` and add `dp[s - x]` into `dp[s]`.
- Walking downward stops one element from being used twice in the same subset.
- This counts subsets, which are subsequences that keep array order. It does not count permutations of the same index set again.
- Recursion over include-or-skip is O(2^n), the page’s naive bound.

**Examples:**

```text
def count_subsequences(arr, target):
    dp = [0] * (target + 1)
    dp[0] = 1
    for x in arr:
        for s in range(target, x - 1, -1):
            dp[s] += dp[s - x]
    return dp[target]
```

`count_subsequences([1, 2, 3], 3)` is 2: `[3]` and `[1, 2]`.

**Sources:**

- [GeeksforGeeks — Subset sum](https://www.geeksforgeeks.org/dsa/subset-sum-problem-dp-25/)
</details>

### Partitioning Problems

<details>
<summary>Solve the partition problem using Dynamic Programming.</summary>

### Solve the partition problem using Dynamic Programming.

**Definition:**

The array can be split into two subsets of equal sum exactly when some subset sums to half the total. GeeksforGeeks times that subset-sum DP at O(sum · n) time. The space-optimized form uses O(sum) extra memory. An odd total is impossible.

**How it works:**

- If `total` is odd, return false. Otherwise ask subset sum for `total // 2`.
- The boolean row marks every sum the prefix can make. A true cell at `total // 2` is a yes.
- `[1, 5, 11, 5]` totals 22, and `{11}` hits 11, so the other subset is `{1, 5, 5}`. `[1, 5, 3]` totals 9 and cannot split.
- Recursion over the two choices per element is O(2^n).

**Examples:**

```text
def can_partition(arr):
    total = sum(arr)
    if total % 2:
        return False
    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True
    for x in arr:
        for s in range(target, x - 1, -1):
            dp[s] = dp[s] or dp[s - x]
    return dp[target]
```

`can_partition([1, 5, 11, 5])` is true. `can_partition([1, 5, 3])` is false.

**Sources:**

- [GeeksforGeeks — Partition into equal sum](https://www.geeksforgeeks.org/dsa/partition-problem-dp-18/)
</details>

<details>
<summary>Provide a Dynamic Programming solution for the subset sum problem.</summary>

### Provide a Dynamic Programming solution for the subset sum problem.

**Definition:**

Subset sum asks whether any subset of non-negative integers adds up to `target`. GeeksforGeeks times tabulation at O(sum · n) time and O(sum · n) space, and the one-row form at O(sum · n) time and O(sum) space.

**How it works:**

- `dp[s]` means some subset of the elements considered so far sums to `s`.
- For each value, update sums from high to low so the value is consumed once.
- `[3, 34, 4, 12, 5, 2]` can make 9 as `4 + 5`, and cannot make 30.
- Include-or-skip recursion is O(2^n) time and O(n) stack space.

**Examples:**

```text
def subset_sum(arr, target):
    dp = [False] * (target + 1)
    dp[0] = True
    for x in arr:
        for s in range(target, x - 1, -1):
            dp[s] = dp[s] or dp[s - x]
    return dp[target]
```

`subset_sum([3, 34, 4, 12, 5, 2], 9)` is true. `subset_sum([3, 34, 4, 12, 5, 2], 30)` is false.

**Sources:**

- [GeeksforGeeks — Subset sum](https://www.geeksforgeeks.org/dsa/subset-sum-problem-dp-25/)
</details>

<details>
<summary>Solve the Balanced Partition problem dynamically.</summary>

### Solve the Balanced Partition problem dynamically.

**Definition:**

A balanced split puts every element in one side or the other and makes the two sums as close as possible. That is the equal-partition DP aimed at the reachable sum nearest `total // 2`. GeeksforGeeks times the reachable-sum table at O(sum · n) time and O(sum) extra memory in the one-row form.

**How it works:**

- An equal split exists only when `total // 2` is reachable and `total` is even. Otherwise the best reachable sum `s <= total // 2` leaves the difference `total - 2s`.
- `[1, 6, 11, 5]` totals 23. The subset `{11}` sums to 11, the other side sums to 12, and the difference is 1.
- The decision version on the partition page is the special case where that difference is 0.
- The naive include-or-skip search is O(2^n).

**Examples:**

```text
def min_partition_diff(arr):
    total = sum(arr)
    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True
    for x in arr:
        for s in range(target, x - 1, -1):
            dp[s] = dp[s] or dp[s - x]
    for s in range(target, -1, -1):
        if dp[s]:
            return total - 2 * s
    return total
```

`min_partition_diff([1, 6, 11, 5])` is 1. `min_partition_diff([1, 5, 11, 5])` is 0.

**Sources:**

- [GeeksforGeeks — Partition into equal sum](https://www.geeksforgeeks.org/dsa/partition-problem-dp-18/)
</details>

### Path-finding Dynamic Programming Questions

<details>
<summary>Solve the unique paths problem in a grid with obstacles using DP.</summary>

### Solve the unique paths problem in a grid with obstacles using DP.

**Definition:**

Count paths from the top-left cell to the bottom-right cell that move only right or down and never step on an obstacle. Filling one value per cell is the same accounting GeeksforGeeks uses for a grid DP: the min-cost table is O(m · n) time because every cell is written once.

**How it works:**

- `dp[0][0]` is 1 when the start is open and 0 when it is blocked.
- A blocked cell stores 0. An open cell stores the sum of the open path counts from the top and from the left.
- There is no third diagonal move in this problem. The min-cost page allows right, down, and diagonal; the path count here uses only the first two.
- The table has one entry per cell, so the fill is Θ(m · n) time and the two rolling rows are O(n) extra memory, the same space class as that page’s expected O(n) grid DP.

**Examples:**

```text
def unique_paths(grid):
    if not grid or grid[0][0] == 1:
        return 0
    m, n = len(grid), len(grid[0])
    dp = [0] * n
    dp[0] = 1
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                dp[j] = 0
            elif j:
                dp[j] += dp[j - 1]
    return dp[-1]
```

On a 3×3 grid whose center cell is blocked, two paths remain along the border.

**Sources:**

- [GeeksforGeeks — Min cost path](https://www.geeksforgeeks.org/dsa/min-cost-path-dp-6/)
</details>

<details>
<summary>Use Dynamic Programming to solve the minimum cost path problem in a grid.</summary>

### Use Dynamic Programming to solve the minimum cost path problem in a grid.

**Definition:**

Each cell has a traversal cost, and the path cost is the sum of the cells visited, including both ends. GeeksforGeeks allows right, down, and diagonal moves, and times the space-optimized DP at O(m · n) time and O(n) extra memory. Memoized and tabulated forms are O(m · n) time and O(m · n) space.

**How it works:**

- `dp[i][j] = cost[i][j] + min(from the left, from above, from the diagonal)`, skipping a direction that would leave the grid.
- The first cell is just `cost[0][0]`.
- On `[[1, 2, 3], [4, 8, 2], [1, 5, 3]]` the minimum is 8, along `(0,0) → (0,1) → (1,2) → (2,2)`.
- Recursion that retries every branch is O(3^(m·n)) on that page. Dijkstra on the cell graph is O((m·n) log(m·n)).

**Examples:**

```text
def min_cost(cost):
    m, n = len(cost), len(cost[0])
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = cost[0][0]
    for j in range(1, n):
        dp[0][j] = dp[0][j - 1] + cost[0][j]
    for i in range(1, m):
        dp[i][0] = dp[i - 1][0] + cost[i][0]
        for j in range(1, n):
            dp[i][j] = cost[i][j] + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[-1][-1]
```

`min_cost([[1, 2, 3], [4, 8, 2], [1, 5, 3]])` is 8.

**Sources:**

- [GeeksforGeeks — Min cost path](https://www.geeksforgeeks.org/dsa/min-cost-path-dp-6/)
</details>

<details>
<summary>Find the longest path in a matrix with given constraints.</summary>

### Find the longest path in a matrix with given constraints.

**Definition:**

The usual constraint is a path of cells that moves to a 4-neighbor and strictly increases. Memoizing the best path length from each cell writes every cell once and inspects four neighbors, the same “one state per cell” accounting GeeksforGeeks uses when a grid DP is O(m · n).

**How it works:**

- `dp[i][j]` is the longest increasing path that starts at `(i, j)`. It is at least 1.
- It is 1 plus the best memoized neighbor whose value is greater than `matrix[i][j]`.
- Without the table the recursion repeats the same cell from many parents. With the table each cell is solved once.
- The answer is the maximum over all starts, because the path need not begin in a corner.
- A cell with no greater neighbor is a path of length 1.

**Examples:**

```text
def longest_increasing_path(matrix):
    if not matrix:
        return 0
    m, n = len(matrix), len(matrix[0])
    dp = [[0] * n for _ in range(m)]

    def best(i, j):
        if dp[i][j]:
            return dp[i][j]
        longest = 1
        for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            ni, nj = i + di, j + dj
            if 0 <= ni < m and 0 <= nj < n and matrix[ni][nj] > matrix[i][j]:
                longest = max(longest, 1 + best(ni, nj))
        dp[i][j] = longest
        return longest

    return max(best(i, j) for i in range(m) for j in range(n))
```

On `[[9, 9, 4], [6, 6, 8], [2, 1, 1]]` one increasing path is `1 → 2 → 6 → 9`, length 4.

**Sources:**

- [GeeksforGeeks — Min cost path](https://www.geeksforgeeks.org/dsa/min-cost-path-dp-6/)
</details>

### Advanced Dynamic Programming Problems

<details>
<summary>Solving the word wrap problem using DP.</summary>

### Solving the word wrap problem using DP.

**Definition:**

Words stay in order. A line holds at most `k` characters, and the cost of a line is the square of its extra spaces. The last line costs nothing. GeeksforGeeks times the tabulated DP at O(n²) time and O(n) space. Greedy placement is not optimal: the page’s example `[3, 2, 2, 5]` with width 6 has cost 10, while a greedy-looking split costs 16.

**How it works:**

- `dp[i]` is the minimum cost of wrapping words `i…n-1`.
- Try every line that starts at `i` and still fits. The cost is `(k - used)²` plus `dp` of the next word, except a line that consumes the rest of the text costs 0.
- There are O(n) starts and O(n) candidate ends, which is the stated O(n²). Recursion without the table is exponential; the page writes that naive bound as O(2^n).
- Words are assumed shorter than the line, so a single word always fits.

**Examples:**

```text
def word_wrap(lengths, k):
    n = len(lengths)
    dp = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        dp[i] = 10**18
        used = -1
        for j in range(i, n):
            used += lengths[j] + 1
            if used > k:
                break
            extra = 0 if j == n - 1 else (k - used) ** 2
            dp[i] = min(dp[i], extra + dp[j + 1])
    return dp[0]
```

`word_wrap([3, 2, 2, 5], 6)` is 10. `word_wrap([3, 2, 2], 4)` is 5.

**Sources:**

- [GeeksforGeeks — Word wrap](https://www.geeksforgeeks.org/dsa/word-wrap-problem-dp-19/)
</details>

<details>
<summary>Implement an algorithm for the maximal square problem in a 2D binary matrix.</summary>

### Implement an algorithm for the maximal square problem in a 2D binary matrix.

**Definition:**

Find the largest square of 1s in a binary matrix and report its area. Each cell is one state: if it is 1, the largest square whose bottom-right corner is that cell is one plus the minimum of the squares ending on the left, above, and on the diagonal. Filling every cell once is the O(m · n) grid DP GeeksforGeeks states for a table of that shape.

**How it works:**

- A 0 cannot be the corner of a square of 1s, so its state is 0.
- A 1 on the first row or first column is a square of side 1.
- The answer is the square of the largest side stored.
- The recurrence only looks at three already-computed neighbors, so one left-to-right, top-to-bottom pass is enough.

**Examples:**

```text
def maximal_square(matrix):
    if not matrix:
        return 0
    m, n = len(matrix), len(matrix[0])
    side = 0
    dp = [[0] * n for _ in range(m)]
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == "1" or matrix[i][j] == 1:
                if i == 0 or j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
                side = max(side, dp[i][j])
    return side * side
```

On `[["1","0","1"],["1","1","1"],["1","1","1"]]` the largest side is 2, so the area is 4.

**Sources:**

- [GeeksforGeeks — Min cost path](https://www.geeksforgeeks.org/dsa/min-cost-path-dp-6/)
</details>

<details>
<summary>Apply Dynamic Programming to solve the maximum profit in job scheduling.</summary>

### Apply Dynamic Programming to solve the maximum profit in job scheduling.

**Definition:**

Each job has a start, an end, and a profit. Chosen jobs must not overlap; a job may start at the exact end time of another. GeeksforGeeks times the tabulated DP, after sorting by finish time and binary-searching the latest compatible job, at O(n log n) time and O(n) space. Recursion without memory is O(2^n).

**How it works:**

- Sort jobs by end time. `dp[i]` is the best profit using the first `i` jobs in that order.
- Skip job `i`, keeping `dp[i - 1]`, or take it and add its profit to `dp` at the rightmost earlier job that finishes no later than this start.
- Binary search finds that predecessor in O(log n), so each of the n jobs costs a logarithm.
- The page’s example `[[1, 2, 50], [3, 5, 20], [6, 19, 100], [2, 100, 200]]` is worth 250 from the first and fourth jobs.

**Examples:**

```text
import bisect

def job_profit(jobs):
    jobs = sorted(jobs, key=lambda job: job[1])
    ends = [job[1] for job in jobs]
    dp = [0] * (len(jobs) + 1)
    for i, (start, end, profit) in enumerate(jobs, 1):
        prev = bisect.bisect_right(ends, start, 0, i - 1)
        dp[i] = max(dp[i - 1], dp[prev] + profit)
    return dp[-1]
```

`job_profit([[1, 2, 50], [3, 5, 20], [6, 19, 100], [2, 100, 200]])` is 250. `job_profit([[1, 3, 60], [2, 5, 50], [4, 6, 70], [5, 7, 30]])` is 130.

**Sources:**

- [GeeksforGeeks — Weighted job scheduling](https://www.geeksforgeeks.org/dsa/weighted-job-scheduling/)
</details>

### Dynamic Programming in Graphs

<details>
<summary>How Dynamic Programming can be used for finding shortest paths in graphs (e.g., Floyd-Warshall algorithm)?</summary>

### How Dynamic Programming can be used for finding shortest paths in graphs (e.g., Floyd-Warshall algorithm)?

**Definition:**

Floyd–Warshall is all-pairs shortest paths by dynamic programming on the set of allowed intermediate vertices. GeeksforGeeks times the triple loop at O(V³), where V is the number of vertices, and O(1) auxiliary space when the distance matrix is updated in place. It allows negative edges and rejects negative cycles. On a dense graph that cubic bound is the reason to prefer it; on a sparse graph the page points at Johnson’s algorithm instead.

**How it works:**

- `dist[i][j]` starts as the direct edge weight, 0 on the diagonal, and a sentinel when no edge exists.
- For each intermediate `k`, and every pair `(i, j)`, replace `dist[i][j]` with `dist[i][k] + dist[k][j]` when that sum is smaller and neither piece is the sentinel.
- When `k` is considered, shortest paths that use only intermediates below `k` are already known. That is the optimal substructure the page relies on.
- After all `k`, `dist[i][j]` is the shortest path that may use any vertex. A negative cycle shows up as a negative diagonal if you run one more check.

**Examples:**

```text
def floyd(dist, inf):
    v = len(dist)
    for k in range(v):
        for i in range(v):
            for j in range(v):
                if dist[i][k] != inf and dist[k][j] != inf:
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist
```

Three nested loops, each of length V, are the O(V³) bound.

**Sources:**

- [GeeksforGeeks — Floyd–Warshall](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
</details>

<details>
<summary>How to use Dynamic Programming to find the number of ways to reach a given vertex in a directed acyclic graph?</summary>

### How to use Dynamic Programming to find the number of ways to reach a given vertex in a directed acyclic graph?

**Definition:**

In a DAG every path respects a topological order, so the number of ways to reach `v` is the sum of the ways to reach each predecessor. Each edge contributes one addition. Paths of one exact length `k` are a different DP: CP-Algorithms raises the adjacency matrix to the `k`-th power in O(n³ log k).

**How it works:**

- Set `ways[source] = 1` and every other vertex to 0.
- Visit vertices so that every incoming edge comes from an earlier vertex. Then `ways[v] += ways[u]` for each edge `u → v`.
- The acyclic order is what makes one pass enough. A cycle would make the same recurrence refer to itself.
- The pass touches each vertex and each edge a constant number of times, so the work follows `|V| + |E|` after a topological sort of the same order.
- If the question is paths of length exactly `k`, the matrix power counts them for every pair at once, at the cubic-log cost above.

**Examples:**

```text
def ways_to(n, edges, source, target):
    incoming = [[] for _ in range(n)]
    indeg = [0] * n
    for u, v in edges:
        incoming[v].append(u)
        indeg[v] += 1
    from collections import deque
    q = deque(i for i in range(n) if indeg[i] == 0)
    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in range(n):
            if u in incoming[v]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    q.append(v)
    ways = [0] * n
    ways[source] = 1
    for v in order:
        for u in incoming[v]:
            ways[v] += ways[u]
    return ways[target]
```

Edges `(0,1), (0,2), (1,2)` give `ways_to(3, those, 0, 2) = 2`.

**Sources:**

- [CP-Algorithms — Binary exponentiation (paths of length k)](https://cp-algorithms.com/algebra/binary-exp.html)
</details>

### Game Theory and Dynamic Programming

<details>
<summary>Implement an algorithm to solve the Nim Game using Dynamic Programming.</summary>

### Implement an algorithm to solve the Nim Game using Dynamic Programming.

**Definition:**

Normal-play Nim is solved by the nim-sum: the bitwise XOR of the heap sizes, which Wikipedia also calls addition without carry. A position is winning when that sum is not zero, because the player to move can always leave a sum of zero, and every move from a zero sum leaves a non-zero sum. The XOR replaces a DP whose state would be the whole tuple of heap sizes.

**How it works:**

- One heap of size `n` has nim-sum `n`. The player takes the whole heap and wins, unless the heap is already 0.
- Heaps 3, 4, and 5 have nim-sum `3 ⊕ 4 ⊕ 5 = 2`, so the first player wins. The winning move Wikipedia gives is to reduce the heap of 3 to 1.
- A literal DP still exists: the losing positions are those with nim-sum 0, and a state is winning when some legal move reaches a losing state. Memoizing that game tree is exponential in the number of heaps; the XOR evaluates the same answer with one pass over the heaps.
- Misère Nim, where the last object loses, matches this strategy until only one heap of size at least 2 remains. The page states the adjustment for that ending.

**Examples:**

```text
def nim_winning(heaps):
    total = 0
    for h in heaps:
        total ^= h
    return total != 0
```

`nim_winning([3, 4, 5])` is true. `nim_winning([1, 1])` is false in normal play: the nim-sum is 0.

**Sources:**

- [Wikipedia — Nim](https://en.wikipedia.org/wiki/Nim)
</details>

<details>
<summary>Apply Dynamic Programming to solve the minimax problem in game theory.</summary>

### Apply Dynamic Programming to solve the minimax problem in game theory.

**Definition:**

Minimax assigns each position the best outcome the player to move can force. Wikipedia’s account of Nim is the impartial special case: a position is a win when some move leaves the opponent a loss, and the nim-sum collapses that recurrence. With a memoized table, each distinct position is evaluated once.

**How it works:**

- A terminal position has a fixed score. In normal play, the player who cannot move has lost.
- The player to move takes the maximum over children. If the opponent also plays optimally, that child value is the opponent’s minimum, which is the same recursion with the roles swapped.
- Alpha-beta pruning skips branches that cannot change the chosen score. It does not change the value; it only avoids work.
- The state space is the limit. Nim heaps would be a tuple, exponential to store, which is why the nim-sum is the closed form of that DP.
- Drawing the tree for a tiny game and filling it from the leaves upward is the same algorithm written bottom-up.

**Examples:**

```text
def minimax(state, is_max, moves, score, terminal):
    if terminal(state):
        return score(state)
    values = [minimax(nxt, not is_max, moves, score, terminal) for nxt in moves(state)]
    return max(values) if is_max else min(values)
```

On a single Nim heap the recursion says a heap of 0 is a loss for the player about to move, and any larger heap is a win.

**Sources:**

- [Wikipedia — Nim](https://en.wikipedia.org/wiki/Nim)
</details>

### Stochastic Dynamic Programming Questions

<details>
<summary>Can Dynamic Programming be used to solve problems with probabilistic states? Provide an example.</summary>

### Can Dynamic Programming be used to solve problems with probabilistic states? Provide an example.

**Definition:**

Yes. The state value becomes an expectation or a probability, and the recurrence weights each next state by its probability. The table is still filled once per state, the same reason a deterministic grid DP such as the min-cost path is O(number of cells): each state has a constant number of successors.

**How it works:**

- Let `E[s]` be the expected remaining cost from state `s`. Then `E[s] = cost(s) + Σ p(s, t) · E[t]`.
- A probability of eventual success uses the same shape with no added cost: `P[s] = Σ p(s, t) · P[t]`, and absorbing states are 0 or 1.
- The order still has to respect dependencies. An acyclic process fills in topological order. A process with cycles is a linear system, not a single forward sweep.
- A coin flipped until heads is the tiny example: from “still flipping”, one trial costs 1, and with probability 1/2 you stop and with probability 1/2 you return. The equation `E = 1 + 0.5 · 0 + 0.5 · E` gives `E = 2`.

**Examples:**

```text
def expected_flips():
    # E = 1 + 0.5 * E  =>  E = 2
    return 2

def ruin(p, i, n):
    # probability of hitting n before 0, starting from i
    if i == 0:
        return 0.0
    if i == n:
        return 1.0
    dp = [0.0] * (n + 1)
    dp[n] = 1.0
    for x in range(1, n):
        # solved in order only when the walk is written as a recurrence;
        # fair coin, closed form i/n
        dp[x] = x / n if p == 0.5 else None
    return dp[i]
```

A fair gambler who stops at 0 or 3 and starts at 1 reaches 3 with probability `1/3`.

**Sources:**

- [GeeksforGeeks — Min cost path](https://www.geeksforgeeks.org/dsa/min-cost-path-dp-6/)
</details>

<details>
<summary>How does Stochastic Dynamic Programming differ from deterministic Dynamic Programming, and what are its applications?</summary>

### How does Stochastic Dynamic Programming differ from deterministic Dynamic Programming, and what are its applications?

**Definition:**

Deterministic DP has one next state for each decision. Stochastic DP has a distribution of next states, so the Bellman update takes an expectation. The min-cost grid is the deterministic pattern: from a cell the successor is chosen, not sampled. The coin-until-heads equation is the stochastic pattern: the same state recurs with probability 1/2.

**How it works:**

- The deterministic recurrence is `V(s) = min over actions of cost(s, a) + V(next(s, a))`.
- The stochastic recurrence inserts the distribution: `V(s) = min over actions of cost(s, a) + Σ_t p(t | s, a) · V(t)`.
- Optimal substructure still holds for the expected value, which is why a table works. You store one number per state, not one number per random outcome.
- Filling an acyclic state graph is still one write per state. Cycles, as in an infinite-horizon process, need value iteration or a linear solve rather than one sweep.
- Applications are the ones where the next demand, failure, or opponent move is random: inventory replenishment, equipment replacement, and Markov decision processes. A game of pure skill such as Nim stays deterministic; only chance nodes make the update an average.

**Examples:**

```text
def deterministic(cost):
    # one successor: V = cost + V[next]
    return cost

def stochastic(cost, outcomes):
    # outcomes is a list of (probability, value)
    return cost + sum(p * v for p, v in outcomes)
```

`stochastic(1, [(0.5, 0), (0.5, 2)])` is 2, the expected flips of a fair coin until heads, once the return value has been solved as 2.

**Sources:**

- [GeeksforGeeks — Min cost path](https://www.geeksforgeeks.org/dsa/min-cost-path-dp-6/)
- [Wikipedia — Nim](https://en.wikipedia.org/wiki/Nim)
</details>
