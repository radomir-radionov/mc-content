# Fibonacci Sequence — Interview

Interview answers in this file are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Fibonacci Sequence — Interview](./7_algo_fibonacci_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Fibonacci Sequence](https://devinterview.io/questions/data-structures-and-algorithms/fibonacci-sequence-interview-questions/).

### Fibonacci Sequence Basics

<details>
<summary>What is the Fibonacci sequence?</summary>

### What is the Fibonacci sequence?

**Definition:**

The Fibonacci sequence starts `0, 1` and each later term is the sum of the two before it: `F(n) = F(n-1) + F(n-2)`, with `F(0) = 0` and `F(1) = 1`.

**How it works:**

- The first terms are `0, 1, 1, 2, 3, 5, 8, 13`. Some texts start at `1, 1`; the recurrence is the same and the index shifts by one.
- The ratio of consecutive terms approaches the golden ratio `φ = (1 + √5) / 2 ≈ 1.618`.
- The same recurrence shows up when counting tilings, binary strings without two consecutive ones, and the leaves of a naive recursion tree.
- A direct recursive function matches the definition and repeats a huge amount of work. A loop or a table writes each term once.

**Examples:**

```text
F(0)=0, F(1)=1
F(6)=8, because 0,1,1,2,3,5,8
```
</details>

<details>
<summary>Write a function to calculate the nth Fibonacci number using a recursive approach.</summary>

### Write a function to calculate the nth Fibonacci number using a recursive approach.

**Definition:**

The recursive function returns the base cases for 0 and 1 and otherwise returns the sum of the two smaller calls.

**How it works:**

- If `n` is 0, return 0. If `n` is 1, return 1.
- Otherwise return `fib(n-1) + fib(n-2)`.
- Time is Θ(φⁿ), which people often write as O(2ⁿ). Every call branches in two, and the recursion tree has exponentially many leaves.
- Stack space is Θ(n), because the deepest chain is the `n-1` spine, not the whole tree.
- Memoization or a loop removes the repeated calls. The recurrence itself stays the same.

**Examples:**

```text
fib(n):
  if n < 2: return n
  return fib(n-1) + fib(n-2)
```

`fib(5)` returns 5 after a tree that recomputes `fib(2)` and `fib(3)` several times.
</details>

<details>
<summary>Provide a non-recursive implementation for generating the Fibonacci sequence to the nth number.</summary>

### Provide a non-recursive implementation for generating the Fibonacci sequence to the nth number.

**Definition:**

A non-recursive Fibonacci either loops from the two seeds or evaluates Binet’s closed form. The loop is the one you ship.

**How it works:**

- Binet: `F(n) = (φⁿ − ψⁿ) / √5`, where `φ = (1+√5)/2` and `ψ = (1−√5)/2`. Rounding `φⁿ / √5` to the nearest integer also works while the floating-point value is exact.
- That formula is O(1) arithmetic and O(1) memory, and it drifts once `φⁿ` no longer fits in a precise float. Large `n` needs an exact method.
- The iterative method starts with `prev = 0`, `curr = 1`. For each step up to `n`, replace the pair with `(curr, prev + curr)`.
- The loop is Θ(n) time and Θ(1) extra memory if you only keep the last two terms. Keeping the whole list is Θ(n) memory, which is the output size.

**Examples:**

```text
prev, curr = 0, 1
for _ in 2..n:
  prev, curr = curr, prev + curr
return curr if n > 0 else 0
```

After four updates the pair walks `1,1` then `1,2` then `2,3` then `3,5`.
</details>

<details>
<summary>What is the time complexity of the recursive Fibonacci solution, and how can this be improved?</summary>

### What is the time complexity of the recursive Fibonacci solution, and how can this be improved?

**Definition:**

The naive recursion is exponential because each call makes two more calls. A cache or a loop computes each index a constant number of times.

**How it works:**

- Base cases: return 0 at `n = 0` and 1 at `n = 1`.
- Each larger call adds the two recursive results. The call tree is Θ(φⁿ), often bounded as O(2ⁿ). The stack depth is Θ(n).
- Memoization stores `fib(n)` the first time it is computed. Later calls return the stored value. Time drops to Θ(n), and the cache is Θ(n) memory.
- The iterative pair `(a, b) = (0, 1)`, updated `n` times, is also Θ(n) time and only Θ(1) memory.
- Matrix exponentiation and fast doubling go further, to Θ(log n) arithmetic operations, if multiplication of the growing integers is treated carefully.

**Examples:**

`fib(10)` without a cache does hundreds of calls. With a cache it fills indexes 2 through 10 once each.
</details>

<details>
<summary>Describe the memoization technique as applied to the Fibonacci sequence calculation.</summary>

### Describe the memoization technique as applied to the Fibonacci sequence calculation.

**Definition:**

Memoized Fibonacci keeps a map from `n` to `F(n)` and fills an entry only on the first call with that argument.

**How it works:**

- On entry, if `n` is already in the map, return it.
- If `n` is 0 or 1, store that base and return it.
- Otherwise compute `fib(n-1) + fib(n-2)`, store the sum under `n`, and return it.
- Each index from 0 through n is written once, so the time is Θ(n) instead of exponential.
- The map holds Θ(n) entries, and the recursion still uses Θ(n) stack until the base case returns. An explicit loop avoids that stack.

**Examples:**

```text
fib(n, memo):
  if n in memo: return memo[n]
  memo[n] = n if n < 2 else fib(n-1, memo) + fib(n-2, memo)
  return memo[n]
```

The second call to `fib(40)` after the first one is a single lookup.
</details>

### Efficient Fibonacci Algorithms

<details>
<summary>Implement an iterative solution to generate the Fibonacci sequence, discussing its time and space complexity.</summary>

### Implement an iterative solution to generate the Fibonacci sequence, discussing its time and space complexity.

**Definition:**

The iterative solution keeps the last two terms and advances them `n` times. It never builds the recursion tree.

**How it works:**

- Start with `a = 0` and `b = 1`, which are `F(0)` and `F(1)`.
- Repeat `n` times if you want `F(n)`: set `a, b = b, a + b`. After `k` steps, `a` is `F(k)`.
- One step is a constant amount of arithmetic on the current integers. With fixed-width words that is Θ(n) time. With growing big integers the later additions cost more than O(1).
- Extra memory is Θ(1) besides the output, against Θ(n) stack for the naive recursion and Θ(φⁿ) time.
- If the caller wants the whole sequence, append each `a` to a list. That list is Θ(n) and is the result, not overhead.

**Examples:**

```text
a, b = 0, 1
repeat n times:
  a, b = b, a + b
# a is F(n)
```

The first four steps of `a` are `1, 1, 2, 3`.
</details>

<details>
<summary>Explain the concept of dynamic programming as it relates to the Fibonacci sequence.</summary>

### Explain the concept of dynamic programming as it relates to the Fibonacci sequence.

**Definition:**

Fibonacci is the smallest DP example: the recurrence overlaps, and storing each `F(k)` once turns an exponential tree into a linear chain.

**How it works:**

- Optimal substructure is the recurrence itself. `F(n)` is exactly `F(n-1) + F(n-2)`, so the answer is built from smaller answers.
- Overlap is the waste in the naive tree: `F(n-2)` is reached from both branches.
- Top-down DP memoizes the recursive function. Bottom-up DP writes an array from index 2 up to n.
- Both are Θ(n) time. The array is the explicit table; the memo map is the same table filled in the order the recursion discovers.
- Two rolling variables are the space-optimized bottom-up form. They are still DP: each step reuses the two previous states and throws the older ones away.

**Examples:**

`t[0] = 0`, `t[1] = 1`, then `t[i] = t[i-1] + t[i-2]`. The cell `t[n]` is the answer, and no cell was computed twice.
</details>

<details>
<summary>Solve the nth Fibonacci number problem using matrix exponentiation and analyze its efficiency.</summary>

### Solve the nth Fibonacci number problem using matrix exponentiation and analyze its efficiency.

**Definition:**

The step from `(F(n), F(n+1))` to the next pair is multiplication by a fixed 2×2 matrix. Raising that matrix to the nth power jumps straight to `F(n)`.

**How it works:**

- `[[1, 1], [1, 0]]ⁿ = [[F(n+1), F(n)], [F(n), F(n-1)]]`. The top-right entry is `F(n)`.
- Exponentiation by squaring computes the power with Θ(log n) matrix multiplications: square to get `M²`, `M⁴`, and so on, and multiply the factors whose bits are set in `n`.
- Each multiplication is a constant amount of arithmetic if the integers are fixed width. With big integers the bit length of `F(n)` is Θ(n), so each multiplication costs more as `n` grows.
- Extra memory is the Θ(log n) stack of squaring, or Θ(1) if the squaring is written as a loop.
- This is the same recurrence as the loop. It skips the intermediate Fibonacci numbers you do not need, except the ones the squaring happens to form.

**Examples:**

```text
M = [[1, 1], [1, 0]]
# top-right of M^n is F(n)
# M^1 gives F(1)=1, M^5 gives F(5)=5
```
</details>

<details>
<summary>Can the nth Fibonacci number be found using the golden ratio, and if so, how would you implement this method?</summary>

### Can the nth Fibonacci number be found using the golden ratio, and if so, how would you implement this method?

**Definition:**

Yes. The nearest integer to `φⁿ / √5` is `F(n)`, for `φ = (1 + √5) / 2`. It is Binet’s formula with the smaller `ψⁿ` term rounded away.

**How it works:**

- `φ` and `√5` are irrational, so the implementation uses floating point or an exact algebraic type.
- Double precision stays exact only while `F(n)` is small enough that the rounding error is under `0.5`. Past roughly `n = 70` a 64-bit float is no longer a safe source of the integer.
- The arithmetic is O(1) for a fixed-width float and does not build the previous terms.
- For a large exact `F(n)`, use the integer loop, fast doubling, or matrix power. Use the golden-ratio form for a closed formula, a limit argument, or a small `n` where you have checked the rounding.

**Examples:**

```text
phi = (1 + sqrt(5)) / 2
F(n) = round(phi**n / sqrt(5))
```

`n = 10` rounds to 55. Do not use this for `n = 100` in a 64-bit float.
</details>

<details>
<summary>Present an approach to precomputing Fibonacci numbers to answer multiple nth Fibonacci queries efficiently.</summary>

### Present an approach to precomputing Fibonacci numbers to answer multiple nth Fibonacci queries efficiently.

**Definition:**

If many queries share an upper bound, fill a table once up to that bound and answer each query by index.

**How it works:**

- Let `N` be the largest `n` you will be asked, or grow the table when a query exceeds it.
- Write `t[0] = 0`, `t[1] = 1`, and `t[i] = t[i-1] + t[i-2]` through `N`. That pass is Θ(N) arithmetic.
- A query for `n ≤ N` is Θ(1): return `t[n]`.
- `q` queries with maximum `N` cost Θ(N + q) instead of Θ(N) per query.
- The table is Θ(N) memory, or more once the integers themselves become large. If only one query arrives, the rolling loop is enough and the table is wasted.

**Examples:**

```text
build through max_n once
answer(n) = table[n]
```

Queries `7, 3, 7` after a build through 7 are three array reads: `13, 2, 13`.
</details>

### Fibonacci Variants and Applications

<details>
<summary>How might the Fibonacci sequence be altered to start with two arbitrary initial values? Provide an algorithm for such a sequence.</summary>

### How might the Fibonacci sequence be altered to start with two arbitrary initial values? Provide an algorithm for such a sequence.

**Definition:**

Keep the same additive step and replace the seeds. A sequence with `G(0) = a` and `G(1) = b`, then `G(n) = G(n-1) + G(n-2)`, is a Fibonacci-like sequence. Lucas numbers are the case `a = 2`, `b = 1`.

**How it works:**

- If `n` is 0, return `a`. If `n` is 1, return `b`. The two seeds may be equal; nothing in the recurrence forbids that.
- Otherwise iterate `n - 1` steps from the pair `(a, b)`, replacing it with `(b, a + b)` each time. The first component becomes `G(n)`.
- Time is Θ(n) and extra memory is Θ(1), the same as ordinary Fibonacci.
- Any such sequence is a linear combination of the standard Fibonacci sequence, so the closed form and the matrix power still apply after you change the initial vector.

**Examples:**

```text
nth(a, b, n):
  if n == 0: return a
  x, y = a, b
  repeat n-1 times:
    x, y = y, x + y
  return x
```

`nth(2, 1, 5)` is the Lucas number 11: `2, 1, 3, 4, 7, 11`.
</details>

<details>
<summary>Explain an algorithm to compute the sum of the first n Fibonacci numbers without generating the entire sequence.</summary>

### Explain an algorithm to compute the sum of the first n Fibonacci numbers without generating the entire sequence.

**Definition:**

The sum of `F(1) + … + F(n)` equals `F(n+2) − 1`. With `F(0) = 0` included, the sum through `F(n)` is the same value. You compute one later Fibonacci number, then subtract 1.

**How it works:**

- The identity is `F(1) + … + F(n) = F(n+2) − 1`. Check: `1+1+2+3+5 = 12` and `F(7) − 1 = 13 − 1`.
- Compute `F(n+2)` with the rolling loop, fast doubling, or the matrix power, and subtract 1. You never store `F(1)` through `F(n)`.
- The matrix form is the same 2×2 Fibonacci matrix. Its nth power yields `F(n+2)` in the corner you read, in Θ(log n) multiplications.
- Changing the seeds changes the closed sum. For the standard seeds the subtraction of 1 is the whole shortcut.
- Time matches whatever Fibonacci routine you call. The rolling loop is Θ(n); the matrix power is Θ(log n) arithmetic operations.

**Examples:**

Sum of the first 6 positive Fibonacci numbers: `1+1+2+3+5+8 = 20`, and `F(8) − 1 = 21 − 1 = 20`.
</details>

<details>
<summary>Define the Lucas sequence and detail how a program can generate it.</summary>

### Define the Lucas sequence and detail how a program can generate it.

**Definition:**

The Lucas sequence uses the Fibonacci recurrence with seeds `L(0) = 2` and `L(1) = 1`. The terms begin `2, 1, 3, 4, 7, 11, 18`.

**How it works:**

- `L(n) = L(n-1) + L(n-2)`. The step is the same addition as Fibonacci; only the start pair changes. It is not a cheaper recurrence.
- A loop from `(2, 1)` produces `L(n)` in Θ(n) time and Θ(1) extra memory.
- The closed form is `L(n) = φⁿ + ψⁿ`, which is an integer even though `φ` and `ψ` are irrational. Fibonacci’s Binet form divides by `√5` and then rounds.
- Lucas and Fibonacci are linked: `L(n) = F(n-1) + F(n+1)`. If you already have a Fibonacci table, the Lucas term is one addition.

**Examples:**

```text
x, y = 2, 1
repeat n times:
  x, y = y, x + y
# x is L(n)
```

`L(5) = 11`.
</details>

<details>
<summary>How can the Fibonacci sequence be used to solve the tiling problem, where you need to cover a 2xn rectangle with 2x1 tiles?</summary>

### How can the Fibonacci sequence be used to solve the tiling problem, where you need to cover a 2xn rectangle with 2x1 tiles?

**Definition:**

The number of ways to tile a `2×n` board with `2×1` dominoes is a Fibonacci number, because the leftmost column forces a split into a smaller board of the same kind.

**How it works:**

- Let `T(n)` be the number of tilings of `2×n`. `T(0) = 1` (one way to tile nothing) and `T(1) = 1` (one vertical domino). A horizontal domino covers two columns and must come with a partner, so it is one piece of width 2.
- A tiling either starts with a vertical domino and a tiling of `2×(n-1)`, or with two horizontal dominoes and a tiling of `2×(n-2)`. Those cases do not overlap.
- So `T(n) = T(n-1) + T(n-2)`. With these seeds, `T(n) = F(n+1)` when `F(1) = 1`, `F(2) = 1`, `F(3) = 2`.
- Compute it with the same rolling loop. Time Θ(n), extra memory Θ(1).

**Examples:**

A `2×3` board has 3 tilings: three verticals; two horizontal on the left plus one vertical; one vertical plus two horizontal on the right. `T(3) = 3 = F(4)`.
</details>

### Large Fibonacci Numbers and Modular Arithmetic

<details>
<summary>How to calculate large Fibonacci numbers without encountering integer overflow issues?</summary>

### How to calculate large Fibonacci numbers without encountering integer overflow issues?

**Definition:**

`F(n)` has Θ(n) bits, so a fixed-width integer overflows. Use a type that grows, or stop storing the full integer if you only need a residue or a suffix.

**How it works:**

- A wider primitive only postpones the overflow. `F(93)` already exceeds a signed 64-bit integer. `long` in C++ is not a plan for `F(1000)`.
- Arbitrary-precision integers, such as Python `int` or Java `BigInteger`, allocate more limbs as the number grows. The loop is still the right algorithm; each addition costs more as the bit length grows.
- If you only need `F(n) mod m`, reduce at every step. The values stay bounded by `m`, and overflow of the full `F(n)` never happens. The Pisano period says the sequence of residues repeats.
- The last `k` decimal digits are the special case `m = 10^k`.
- A hand-rolled digit array or a base-10⁹ limb array is the same idea as a big-integer library when the language does not provide one.

**Examples:**

```text
a, b = 0, 1          # language big integers
repeat n times:
  a, b = b, a + b
```

In a language with fixed words, store `a` and `b` as digit lists and add them with a carry.
</details>

<details>
<summary>Implement a function that computes the nth Fibonacci number modulo m.</summary>

### Implement a function that computes the nth Fibonacci number modulo m.

**Definition:**

Compute `F(n) mod m` without building `F(n)`. Fast doubling, from the matrix form on CP-Algorithms, uses `F(2k) = F(k) · (2 F(k+1) − F(k))` and `F(2k+1) = F(k)² + F(k+1)²`. Every product is reduced modulo m, so a factor never exceeds `(m − 1)²` before the remainder. The recurrence halves the index, so the cost is O(log n) arithmetic steps, the same bound the page gives for matrix powering.

**How it works:**

- The pair returned for `k` is `(F(k) mod m, F(k+1) mod m)`.
- `(2b − a) % m` is taken before the multiply, and both squares are reduced, then added modulo m. That is the modular multiplication: the product of two residues is reduced before it is used again.
- A linear loop that adds modulo m is O(n) and also correct. Doubling is the logarithmic form.
- `F(10) = 55`, so `fib_mod(10, 100)` is 55 and `fib_mod(11, 10)` is 9.

**Examples:**

```text
def mul(x, y, m):
    return (x % m) * (y % m) % m

def fib_mod(n, m):
    def rec(k):
        if k == 0:
            return 0, 1 % m
        a, b = rec(k >> 1)
        c = mul(a, (2 * b - a) % m, m)
        d = (mul(a, a, m) + mul(b, b, m)) % m
        if k & 1:
            return d, (c + d) % m
        return c, d
    return rec(n)[0]
```

`fib_mod(10, 100)` is 55. `fib_mod(11, 10)` is 9.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
- [CP-Algorithms — Binary exponentiation](https://cp-algorithms.com/algebra/binary-exp.html)
</details>

<details>
<summary>Implement an algorithm to find the last k digits of the nth Fibonacci number.</summary>

### Implement an algorithm to find the last k digits of the nth Fibonacci number.

**Definition:**

The last k decimal digits are `F(n) mod 10^k`. Use the same modular fast doubling, with modulus `10^k`, so the product of two residues is reduced before it can overflow that modulus. CP-Algorithms places the doubling identities at O(log n).

**How it works:**

- `10^k` replaces `m`. The last k digits are the residue, including leading zeros if you print a fixed width.
- A linear loop of k-digit additions is O(n) big-digit steps. Doubling does O(log n) multiplications of k-digit numbers.
- `F(10) = 55`, so the last digit is 5 and the last two digits are 55.
- This is the Pisano sequence for modulus `10^k`, but you do not need the period to compute one term.

**Examples:**

```text
def mul(x, y, m):
    return (x % m) * (y % m) % m

def fib_mod(n, m):
    def rec(k):
        if k == 0:
            return 0, 1 % m
        a, b = rec(k >> 1)
        c = mul(a, (2 * b - a) % m, m)
        d = (mul(a, a, m) + mul(b, b, m)) % m
        if k & 1:
            return d, (c + d) % m
        return c, d
    return rec(n)[0]
```

`fib_mod(10, 10)` is 5. `fib_mod(10, 100)` is 55.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Check if a given number is a Fibonacci number.</summary>

### Check if a given number is a Fibonacci number.

**Definition:**

Walk the sequence with the linear recurrence until the term meets or passes the candidate. CP-Algorithms defines `F(0) = 0`, `F(1) = 1`, and `F(n) = F(n−1) + F(n−2)`, and Binet’s formula shows `F(n)` grows like `φ^n / √5`, so the index you reach is proportional to the logarithm of the candidate.

**How it works:**

- Keep two consecutive terms. Stop when the latest is equal to `x` (yes) or greater than `x` (no).
- `0` and `1` are Fibonacci. `4` is not: the walk hits 3, then 5.
- The number of additions is the index `m` with `F(m) ≥ x`. Exponential growth makes `m` logarithmic in `x`, not linear in the value.
- Rounding `φ^n / √5` to the nearest integer is the closed form on the page, and the page warns that fractional precision makes it impractical for large n. The integer walk does not have that error.

**Examples:**

```text
def is_fib(x):
    if x < 0:
        return False
    a, b = 0, 1
    while a < x:
        a, b = b, a + b
    return a == x
```

`is_fib(21)` is true. `is_fib(4)` is false.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Solve for the nth Fibonacci number using fast doubling.</summary>

### Solve for the nth Fibonacci number using fast doubling.

**Definition:**

Fast doubling evaluates the index by halves. CP-Algorithms derives `F(2k+1) = F(k+1)² + F(k)²` and `F(2k) = F(k) · (2 F(k+1) − F(k))` by squaring the matrix form, and the recursion depth is O(log n).

**How it works:**

- The call on `n >> 1` returns `F(floor n/2)` and the next term.
- Even `n = 2k` returns the first identity. Odd `n` returns the second, and the following term is their sum.
- Each level does a constant number of multiplications, and there are O(log n) levels.
- The integer products are the full Fibonacci numbers. For a residue, reduce every product modulo m, as in the modular version.

**Examples:**

```text
def fib_pair(n):
    if n == 0:
        return 0, 1
    a, b = fib_pair(n >> 1)
    c = a * (2 * b - a)
    d = a * a + b * b
    if n & 1:
        return d, c + d
    return c, d
```

`fib_pair(10)[0]` is 55. `fib_pair(11)[0]` is 89.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

### Fibonacci Sequence in Data Structures

<details>
<summary>How can a Fibonacci heap data structure benefit from the properties of the Fibonacci sequence?</summary>

### How can a Fibonacci heap data structure benefit from the properties of the Fibonacci sequence?

**Definition:**

A Fibonacci heap keeps trees whose minimum size grows with the Fibonacci numbers. CP-Algorithms records that `F(n)` grows like `φ^n / √5`. A tree whose smallest possible size at rank `d` is a Fibonacci number therefore has rank logarithmic in the number of nodes, because the exponential outgrows any larger rank.

**How it works:**

- The linking rule that produces that size bound is what the heap is named for. The sequence is not stored in the heap; it is the lower bound on tree size.
- A logarithmic rank means a root list and the cascading cuts stay proportional to a logarithm of the heap, which is why decrease-key can be cheap in the amortized accounting.
- A binary heap pays a logarithm for decrease-key in the worst case of one operation. The Fibonacci bound is the reason a graph algorithm with many decrease-key steps, such as Dijkstra with a decrease on every edge relaxation, considers this heap.
- The sequence page does not define the heap operations. The benefit that comes from the sequence itself is the exponential size lower bound, hence the logarithmic rank.

**Examples:**

```text
# F(d+2) grows like phi^(d+2)/sqrt(5).
# A rank-d tree with at least F(d+2) nodes has d = O(log n).
```

`F(10) = 55` already forces a rank-8 style tree to hold dozens of nodes, and the closed form on the page keeps making the next ranks larger by about `φ`.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Implement Fibonacci sequence generation using an array as storage.</summary>

### Implement Fibonacci sequence generation using an array as storage.

**Definition:**

Store every term up to `n`. CP-Algorithms’ linear method computes `F(n)` in O(n) from the two previous terms; keeping the array uses O(n) extra memory instead of the O(1) pair.

**How it works:**

- `a[0] = 0`, `a[1] = 1`, and `a[i] = a[i-1] + a[i-2]`.
- Each index is written once, so the loop is Θ(n) additions.
- The array answers later “what is `F(i)`?” in O(1) after that build. The two-variable loop forgets the prefix.
- The values are the full integers. A fixed-width word overflows; the page’s growth formula is why.

**Examples:**

```text
def fib_array(n):
    if n == 0:
        return [0]
    a = [0] * (n + 1)
    a[1] = 1
    for i in range(2, n + 1):
        a[i] = a[i - 1] + a[i - 2]
    return a
```

`fib_array(6)` is `[0, 1, 1, 2, 3, 5, 8]`.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Provide an example of when and why a Fibonacci queue might be preferred over a traditional queue structure.</summary>

### Provide an example of when and why a Fibonacci queue might be preferred over a traditional queue structure.

**Definition:**

A traditional queue is FIFO: enqueue and dequeue are the only operations, and it does not reorder by priority. A priority queue whose trees obey the Fibonacci size bound is preferred when the algorithm decreases keys many times, because that bound, using the exponential growth CP-Algorithms gives for `F(n)`, keeps the rank logarithmic in the number of nodes.

**How it works:**

- Breadth-first search wants a FIFO queue. Nothing about Fibonacci numbers helps a pure FIFO buffer.
- Dijkstra relaxes edges and decreases the key of a vertex already in the queue. A binary heap pays a logarithm per decrease. The Fibonacci rank bound is what makes a heap with that name a candidate there.
- The sequence is the size lower bound, not the order of service. Items are not dequeued in Fibonacci order.
- If the workload is only push and pop at the ends, the plain queue is the right structure. The extra trees buy decrease-key, which a FIFO queue does not offer.

**Examples:**

```text
# FIFO queue: append and pop(0). No decrease-key.
# Fibonacci heap: rank-d trees hold at least a Fibonacci number of nodes,
# so rank stays logarithmic as F(n) grows like phi^n / sqrt(5).
```

Use the FIFO queue for BFS. Consider the Fibonacci-ranked heap when a shortest-path search decreases keys.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

### Advanced Fibonacci Concepts

<details>
<summary>Explain the Pisano Period and how it pertains to the calculation of Fibonacci numbers modulo m.</summary>

### Explain the Pisano Period and how it pertains to the calculation of Fibonacci numbers modulo m.

**Definition:**

The sequence of Fibonacci numbers modulo m is periodic. CP-Algorithms proves it for a prime modulus `p`: there are only `p²` possible consecutive pairs, so among the first `p² + 1` pairs two match, and the sequence of pairs therefore repeats. The proof also shows the repetition starts at `(F(0), F(1))`, so there is no pre-period. The Pisano period `π(m)` is the length of that cycle. Then `F(n) mod m = F(n mod π(m)) mod m`.

**How it works:**

- For modulus 2 the residues are `0, 1, 1` and then they repeat, so `π(2) = 3`.
- Finding the period means simulating the pair recurrence until `(0, 1)` returns. The pair argument caps a prime modulus at `p²` steps.
- After the period is known, a huge index is reduced before the linear or doubling computation.
- Doubling modulo m does not need the period. The period is a reduction of the index, useful when many queries share one m.

**Examples:**

```text
def pisano(m):
    a, b, length = 0, 1, 0
    while True:
        a, b = b, (a + b) % m
        length += 1
        if a == 0 and b == 1:
            return length
```

`pisano(2)` is 3. `F(8) = 21`, and `8 mod 3 = 2`, so `F(8) mod 2 = F(2) mod 2 = 1`.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>What is the relationship between Fibonacci numbers and the Euclidean algorithm for finding the greatest common divisor?</summary>

### What is the relationship between Fibonacci numbers and the Euclidean algorithm for finding the greatest common divisor?

**Definition:**

CP-Algorithms states `gcd(F(m), F(n)) = F(gcd(m, n))`, and that Fibonacci numbers are the worst possible inputs for the Euclidean algorithm (Lamé’s theorem).

**How it works:**

- The gcd identity is about the indices, not an extra factor. `gcd(F(6), F(9)) = gcd(8, 34) = 2`, and `F(gcd(6, 9)) = F(3) = 2`.
- Euclid replaces `(a, b)` with `(b, a mod b)`. On consecutive Fibonacci numbers the remainder is the previous Fibonacci number, so each step peels exactly one index.
- That is the slowest the subtraction of indices can go, which is why those inputs maximize the number of divisions for a given size.
- The closed form is not required for the identity. The page notes it can be proved from the addition formula.

**Examples:**

```text
def egcd_steps(a, b):
    steps = 0
    while b:
        a, b = b, a % b
        steps += 1
    return steps
```

`egcd_steps(F(12), F(11))` takes more division steps than a random pair of the same magnitude, because each remainder is the previous Fibonacci number.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Can you prove that the sum of n Fibonacci numbers is equivalent to the (n+2)nd Fibonacci number minus 1?</summary>

### Can you prove that the sum of n Fibonacci numbers is equivalent to the (n+2)nd Fibonacci number minus 1?

**Definition:**

Yes, for the indexing on CP-Algorithms, `F(0) = 0`, `F(1) = 1`, `F(2) = 1`, and `F(n) = F(n−1) + F(n−2)`. The sum of the first n positive terms is `F(1) + … + F(n) = F(n+2) − 1`.

**How it works:**

- Base `n = 1`: `F(1) = 1` and `F(3) − 1 = 2 − 1 = 1`.
- Assume the sum through `n` is `F(n+2) − 1`. Add `F(n+1)`: the total is `F(n+2) + F(n+1) − 1 = F(n+3) − 1`, which is the claim for `n + 1`.
- The inductive step is exactly the defining recurrence.
- Check: `1 + 1 + 2 + 3 + 5 = 12` and `F(7) − 1 = 13 − 1 = 12`.

**Examples:**

```text
def sum_first(n):
    a, b, total = 0, 1, 0
    for _ in range(n):
        a, b = b, a + b
        total += a
    return total
```

`sum_first(5)` is 12, and `F(7) − 1` is 12.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

### Fibonacci Numbers in Sorting and Searching Algorithms

<details>
<summary>Describe a scenario where Fibonacci numbers can be applied to optimize a search algorithm.</summary>

### Describe a scenario where Fibonacci numbers can be applied to optimize a search algorithm.

**Definition:**

Fibonacci search splits a sorted array at a Fibonacci index instead of at the midpoint. CP-Algorithms shows `F(n)` grows like `φ^n / √5`, so each step throws away a constant fraction of the remaining interval and the number of probes is logarithmic in the length, the same class as binary search.

**How it works:**

- Build the smallest Fibonacci number that is at least the array length. The split index is `offset + F(k−2)`.
- If the key is larger, the eliminated prefix has size `F(k−2)` and the new window is the following Fibonacci size. If it is smaller, the window shrinks to `F(k−2)`.
- The index updates are additions and subtractions of earlier Fibonacci numbers. That avoids a division at the split, which is the practical reason to prefer it on a machine where division is the expensive step.
- It does not change the sorted-array assumption. An unsorted array still needs a different search.

**Examples:**

```text
def fib_search(arr, x):
    n = len(arr)
    if n == 0:
        return -1
    fib2, fib1 = 0, 1
    fib = fib2 + fib1
    while fib < n:
        fib2, fib1 = fib1, fib
        fib = fib1 + fib2
    offset = -1
    while fib > 1:
        i = min(offset + fib2, n - 1)
        if arr[i] < x:
            fib, fib1, fib2 = fib1, fib2, fib1 - fib2
            offset = i
        elif arr[i] > x:
            fib, fib1, fib2 = fib2, fib1 - fib2, 2 * fib2 - fib1
        else:
            return i
    if fib1 and offset + 1 < n and arr[offset + 1] == x:
        return offset + 1
    return -1
```

On a sorted array the probe count follows the Fibonacci index of the length, and Binet’s growth makes that index `O(log n)`.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Implement a Fibonacci Search algorithm and compare its efficiency with binary search.</summary>

### Implement a Fibonacci Search algorithm and compare its efficiency with binary search.

**Definition:**

Both searches find a key in a sorted array with a logarithmic number of comparisons. Binary search halves the interval, one division per step. Fibonacci search moves the window by Fibonacci sizes, using the exponential growth CP-Algorithms gives for `F(n)`, so the depth is also `O(log n)`. The difference is the split arithmetic, not the asymptotic class.

**How it works:**

- Binary search computes `mid = (lo + hi) // 2` and discards one half.
- Fibonacci search discards either `F(k−2)` or the complementary Fibonacci block. Because `F(k) / F(k−1)` approaches `φ`, the larger remaining piece is about `1/φ` of the window, a constant factor.
- Neither is faster than the other by more than a constant on comparisons. Fibonacci search wins only when the division in the midpoint is the cost you are avoiding.
- Both return the index or a miss. Both require a sorted array.

**Examples:**

```text
def fib_search(arr, x):
    n = len(arr)
    if n == 0:
        return -1
    fib2, fib1 = 0, 1
    fib = fib2 + fib1
    while fib < n:
        fib2, fib1 = fib1, fib
        fib = fib1 + fib2
    offset = -1
    while fib > 1:
        i = min(offset + fib2, n - 1)
        if arr[i] < x:
            fib, fib1, fib2 = fib1, fib2, fib1 - fib2
            offset = i
        elif arr[i] > x:
            fib, fib1, fib2 = fib2, fib1 - fib2, 2 * fib2 - fib1
        else:
            return i
    if fib1 and offset + 1 < n and arr[offset + 1] == x:
        return offset + 1
    return -1
```

`fib_search([10, 22, 35, 40, 45, 50, 80, 82, 85, 90, 100], 85)` finds index 8. Binary search on the same array also takes a handful of comparisons, `O(log n)` for both.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Can the Fibonacci sequence optimize the worst-case running time of an algorithm? Provide an example.</summary>

### Can the Fibonacci sequence optimize the worst-case running time of an algorithm? Provide an example.

**Definition:**

It can tighten a bound, and it can be the input that forces the worst case. CP-Algorithms gives both. Matrix powering computes `F(n)` in `O(log n)` instead of the linear loop’s `O(n)`. Lamé’s theorem says consecutive Fibonacci numbers are the worst inputs to Euclid’s algorithm: each remainder is the previous Fibonacci number, so the division count tracks the index.

**How it works:**

- The linear loop writes one term per index. Binary exponentiation of the 2×2 matrix squares the index, which is `O(log n)` multiplications. That is an optimization of the running time, and the page states it.
- Euclid on `(F(n+1), F(n))` peels one index per division. No pair of that magnitude forces more steps. The sequence here describes the worst case rather than speeding the algorithm up.
- Fast doubling is the same logarithmic optimization written without an explicit matrix.
- Replacing a linear Fibonacci loop with doubling is the clean example of the sequence improving a running time.

**Examples:**

```text
def fib_linear(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

`fib_linear` is Θ(n). Fast doubling on the same page is O(log n).

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
- [CP-Algorithms — Binary exponentiation](https://cp-algorithms.com/algebra/binary-exp.html)
</details>

### Fibonacci Sequence Mathematical Questions

<details>
<summary>Prove by induction that the sum of the first n even-indexed Fibonacci numbers is the (n+1)st Fibonacci number minus 1.</summary>

### Prove by induction that the sum of the first n even-indexed Fibonacci numbers is the (n+1)st Fibonacci number minus 1.

**Definition:**

With the CP-Algorithms indexing `F(0) = 0`, `F(1) = 1`, `F(2) = 1`, the first n even-indexed terms are `F(2) + F(4) + … + F(2n)`. The identity the recurrence proves is `F(2) + … + F(2n) = F(2n+1) − 1`. That is the next odd-indexed term after `F(2n)`, minus 1, not `F(n+1) − 1`. For `n = 2` the sum is `1 + 3 = 4` and `F(5) − 1 = 4`, while `F(3) − 1 = 1`.

**How it works:**

- Base `n = 1`: `F(2) = 1` and `F(3) − 1 = 1`.
- Assume the sum through `F(2n)` is `F(2n+1) − 1`. Add `F(2n+2)`.
- `F(2n+1) − 1 + F(2n+2) = F(2n+3) − 1`, by the defining recurrence `F(2n+3) = F(2n+2) + F(2n+1)`.
- The inductive claim for `n + 1` is exactly that. The “(n+1)st” wording matches only if the index in view is `2n+1`, the odd place just after the last even term.

**Examples:**

```text
# n = 3: F(2)+F(4)+F(6) = 1+3+8 = 12
# F(7)-1 = 13-1 = 12
# F(n+1)-1 = F(4)-1 = 2, which is not the sum
```

The induction above is the statement that holds.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>How can Fibonacci numbers be related to the Binet’s formula, and is it practical for computing Fibonacci numbers?</summary>

### How can Fibonacci numbers be related to the Binet’s formula, and is it practical for computing Fibonacci numbers?

**Definition:**

CP-Algorithms gives Binet’s formula, already known to de Moivre: `F(n) = (φ^n − ψ^n) / √5`, where `φ = (1+√5)/2` and `ψ = (1−√5)/2`. Because `|ψ^n| < 1` and shrinks exponentially, `F(n)` is the integer nearest to `φ^n / √5`. The page says both formulas are of little use in practical calculations, because they need very high fractional accuracy.

**How it works:**

- The closed form is exact in real arithmetic. In floating point, `φ^n` for large n is not exact, and rounding to the nearest integer then picks the wrong Fibonacci number.
- The integer recurrence, the linear loop, and fast doubling use only integer additions and multiplications.
- Doubling is `O(log n)` integer steps. Binet looks like `O(1)` and is not reliable once `n` outgrows the mantissa.
- The formula is still the right tool for asymptotics: it is why the index of a value is logarithmic and why Fibonacci search’s split ratio tends to `φ`.

**Examples:**

```text
import math
def binet(n):
    phi = (1 + math.sqrt(5)) / 2
    return round(phi ** n / math.sqrt(5))
```

`binet(10)` is 55. For a large n the same rounding is the method the page advises against; use `fib_pair` instead.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Do Fibonacci numbers have a connection with Pascal’s Triangle?</summary>

### Do Fibonacci numbers have a connection with Pascal’s Triangle?

**Definition:**

Yes. The rising diagonals of Pascal’s triangle sum to Fibonacci numbers. With `F(0) = 0`, `F(1) = 1` and the recurrence from CP-Algorithms, `F(n+1) = Σ_k C(n−k, k)`, where the binomial coefficient is 0 when the bottom index is out of range.

**How it works:**

- For `n = 5`: `C(5,0) + C(4,1) + C(3,2) = 1 + 4 + 3 = 8 = F(6)`.
- For `n = 4`: `C(4,0) + C(3,1) + C(2,2) = 1 + 3 + 1 = 5 = F(5)`.
- The binomial recurrence `C(n, k) = C(n−1, k) + C(n−1, k−1)` is why the diagonal sums obey the Fibonacci recurrence. Each new diagonal adds the two previous diagonal sums.
- This is an identity between two integer tables. It is not a faster way to compute `F(n)` than doubling, which the same page places at `O(log n)`.

**Examples:**

```text
def diag(n):
    total, k = 0, 0
    while n - k >= k:
        term = 1
        for i in range(k):
            term = term * (n - k - i) // (i + 1)
        total += term
        k += 1
    return total
```

`diag(5)` is 8, which is `F(6)`.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

<details>
<summary>Generate the Fibonacci sequence using a two-variable iterative process.</summary>

### Generate the Fibonacci sequence using a two-variable iterative process.

**Definition:**

Keep only the last two terms. CP-Algorithms writes this as the linear solution: O(n) time and O(1) additional memory, from `F(0) = 0` and `F(1) = 1`.

**How it works:**

- Replace the pair `(a, b)` with `(b, a + b)`, `n` times. The first component is `F(n)`.
- Nothing earlier is stored. If you need the whole list, use the array form, which is O(n) memory.
- The additions are on the full integers. For a residue, add modulo m.
- This is the algorithm doubling improves from O(n) to O(log n).

**Examples:**

```text
def fib(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

`fib(0)` is 0, `fib(1)` is 1, `fib(7)` is 13.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>

### Fibonacci Numbers in Theoretical Computer Science

<details>
<summary>How is the Fibonacci sequence used in the analysis of algorithms, particularly in the derivation of time complexities?</summary>

### How is the Fibonacci sequence used in the analysis of algorithms, particularly in the derivation of time complexities?

**Definition:**

Two uses are on the CP-Algorithms page. The closed form `φ^n / √5` turns a recurrence `T(n) = T(n−1) + T(n−2)` into an exponential, the same growth as the naive Fibonacci recursion. The matrix form turns the constructive recurrence into `O(log n)` multiplications. Lamé’s theorem uses the sequence as the worst-case input of Euclid, so the division count is the Fibonacci index of the inputs.

**How it works:**

- A recurrence that adds the two previous subproblems solves to Θ(φ^n). That is why the naive recursive Fibonacci function is exponential, not linear.
- The same sequence, computed by squaring a 2×2 matrix, is O(log n). The analysis changed because the algorithm changed.
- Euclid’s step count on integers near `F(n)` is proportional to `n`, and `n` is logarithmic in the value because of Binet. The worst-case time of Euclid is therefore logarithmic in the input value, and the Fibonacci inputs are what prove the constant.
- Fibonacci search’s comparison count is the same logarithm: the window shrinks by `φ` each step.

**Examples:**

```text
# Naive recursion: T(n) = T(n-1) + T(n-2) + O(1) = Theta(phi^n)
# Matrix power or fast doubling: O(log n)
# Euclid on F(n+1), F(n): Theta(n) divisions, and n = Theta(log F(n))
```

The sequence is both the solution of the bad recurrence and the witness for Euclid’s worst case.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
- [CP-Algorithms — Binary exponentiation](https://cp-algorithms.com/algebra/binary-exp.html)
</details>

<details>
<summary>Discuss the significance of the Fibonacci sequence in computational complexity theory and provide an example.</summary>

### Discuss the significance of the Fibonacci sequence in computational complexity theory and provide an example.

**Definition:**

The sequence separates two complexities of one problem. CP-Algorithms computes `F(n)` in O(n) by the linear loop and in O(log n) by raising the matrix `[[1,1],[1,0]]` to the n-th power. The naive recursion that branches into `n−1` and `n−2` is exponential, because the closed form grows like `φ^n`. The same numbers are the worst-case inputs of the Euclidean algorithm.

**How it works:**

- Exponential, linear, and logarithmic are three algorithms for one sequence. The complexity is a property of the algorithm, and the closed form tells you which recurrence you solved.
- Binary exponentiation is why the matrix method is logarithmic: there are O(log n) squarings. The page on exponentiation states that bound, and the Fibonacci page points at it.
- Lamé’s theorem is a lower-bound style example inside an upper bound: no input makes Euclid slower than the Fibonacci pair of that size, and that pair takes a logarithmic number of divisions in the value.
- A Fibonacci heap’s rank bound is the same growth applied to a data structure: exponential minimum size implies logarithmic degree.

**Examples:**

```text
# F(n) by recursion: Theta(phi^n)
# F(n) by the two-variable loop: Theta(n)
# F(n) by matrix power: O(log n)
```

`φ = (1+√5)/2` is the base the page uses for the exponential.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
- [CP-Algorithms — Binary exponentiation](https://cp-algorithms.com/algebra/binary-exp.html)
</details>

### Fibonacci Sequence Interview Challenges

<details>
<summary>Implement a script that generates a spiral matrix filled in with Fibonacci numbers.</summary>

### Implement a script that generates a spiral matrix filled in with Fibonacci numbers.

**Definition:**

Walk a matrix in spiral order and write `F(0), F(1), F(2), …` from the two-variable loop. That loop is the O(n) method on CP-Algorithms; here n is the number of cells, so an `s×s` matrix takes Θ(s²) writes and Θ(1) extra memory beyond the matrix.

**How it works:**

- Empty cells start as `None`, because `F(0) = 0` and 0 cannot mark “not yet written”.
- The direction order is right, down, left, up. A step that would leave the matrix or land on a filled cell turns to the next direction.
- Each cell is written once, so the fill is Θ(s²).
- A 2×2 spiral is `[[0, 1], [2, 1]]`: `0` at the start, then right to `1`, down to `1`, left to `2`.

**Examples:**

```text
def fib_spiral(size):
    grid = [[None] * size for _ in range(size)]
    dirs = ((0, 1), (1, 0), (0, -1), (-1, 0))
    r = c = turn = 0
    a, b = 0, 1
    for _ in range(size * size):
        grid[r][c] = a
        a, b = b, a + b
        nr, nc = r + dirs[turn][0], c + dirs[turn][1]
        if not (0 <= nr < size and 0 <= nc < size and grid[nr][nc] is None):
            turn = (turn + 1) % 4
            nr, nc = r + dirs[turn][0], c + dirs[turn][1]
        r, c = nr, nc
    return grid
```

`fib_spiral(2)` is `[[0, 1], [2, 1]]`.

**Sources:**

- [CP-Algorithms — Fibonacci numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)
</details>
