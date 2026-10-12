# Recursion — Interview

Answers are filled for every question in this file.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Recursion — Interview](./7_algo_recursion_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Recursion](https://devinterview.io/questions/data-structures-and-algorithms/recursion-algorithm-interview-questions/).

### Understanding Recursion

<details>
<summary>How Dynamic Programming is different from Recursion and Memoization?</summary>

### How Dynamic Programming is different from Recursion and Memoization?

**Definition:**

Recursion is a call that solves a smaller copy of the same problem. Memoization is recursion plus a cache. Dynamic programming is the decision to define states and transitions so each state is solved once; the bottom-up table is DP without the recursive calls.

**How it works:**

- Plain recursion can recompute the same arguments on every branch, which is why naive Fibonacci is exponential.
- Memoization keeps the top-down shape and stores a result the first time those arguments appear. Later calls are lookups. It only fills states the original call reaches.
- Bottom-up DP fills every state the dependency order requires, usually with loops, and does not keep a call stack.
- All three can express the same recurrence. What changes is whether repeated work is stored, and whether you start from the answer or from the base cases.
- The cache and the table both cost memory proportional to the number of states you keep. The recursive forms also cost stack depth.

**Examples:**

`fib(n)` that calls itself twice is recursion. The same function that returns a stored `fib(n)` is memoization. A loop that writes `t[i] = t[i-1] + t[i-2]` is bottom-up DP.
</details>

<details>
<summary>What are some Common Examples of Recursion in computer science?</summary>

### What are some Common Examples of Recursion in computer science?

**Definition:**

Recursion fits when a value is defined by the same kind of value on a smaller input: a tree, a range, a string, or a pile of disks.

**How it works:**

- Tree walks: in-order visits the left subtree, the node, then the right subtree. Each call is the same procedure on a child.
- A palindrome check compares the ends and recurses on the slice between them.
- Factorial is `n * fact(n-1)` with `fact(0) = 1`.
- A directory listing calls itself on each subdirectory.
- Towers of Hanoi moves `n-1` disks out of the way, moves the largest disk, then moves the `n-1` back on top.
- Merge sort sorts the two halves, then merges. That is divide and conquer, which is recursion plus a combine step.
- Catalan numbers count binary trees, balanced parentheses, and polygon triangulations with a recurrence that sums products of smaller Catalan numbers.

**Examples:**

`fact(4)` is `4 * fact(3)`, which unwinds to `4*3*2*1`. The directory walk is the same shape with a list of children instead of `n-1`.
</details>

<details>
<summary>What is the difference between Backtracking and Recursion?</summary>

### What is the difference between Backtracking and Recursion?

**Definition:**

Backtracking is a way of searching a space of choices. It is usually written with recursion, but recursion by itself does not undo a choice.

**How it works:**

- Recursion splits a problem into smaller copies and combines their results. Merge sort, tree walks, and factorial never say “that choice was wrong, restore the previous state”.
- Backtracking builds a candidate one step at a time. When the partial candidate cannot be extended to a solution, it undoes the last step and tries a different one.
- The undo is the name: pop the queen, clear the cell, remove the edge from the current path.
- Recursion without search: traversals, divide and conquer, and a DP recurrence.
- Backtracking as search: Sudoku, permutations, and the subsets of a knapsack when you explore include-or-skip and abandon a branch that already exceeds the capacity.
- You can write backtracking with an explicit stack. The recursive form is the usual one because the call stack stores the choice to undo.

**Examples:**

Permutations: append a number, recurse, then pop it. The pop is the backtrack. Factorial has a recursive call and nothing to undo.
</details>

<details>
<summary>Define Base Case in the context of recursive functions.</summary>

### Define Base Case in the context of recursive functions.

**Definition:**

The base case is the input the function answers without calling itself. It is what makes the recursion stop.

**How it works:**

- Identify the smallest input that is already an answer: `fact(0) = 1`, an empty range in binary search, a missing child in a tree walk.
- Put that test before the recursive call. If the call comes first, the base case is never reached.
- Every recursive call must move toward a base case. `fact(n)` calls `fact(n-1)`, not `fact(n)`.
- Missing the base case, or stepping the wrong way, grows the stack until the runtime aborts it.
- A function can have more than one base case. Fibonacci stops at both 0 and 1.

**Examples:**

```text
fact(n):
  if n == 0: return 1      # base
  return n * fact(n - 1)   # step toward 0
```
</details>

<details>
<summary>Explain the concept of Recursion Depth and its implications on algorithm complexity.</summary>

### Explain the concept of Recursion Depth and its implications on algorithm complexity.

**Definition:**

Recursion depth is how many calls are open at once, the height of the call stack. It bounds stack memory, and it is not the same number as the total number of calls.

**How it works:**

- A single chain, such as `fact(n)` calling `fact(n-1)`, has depth `n` and also Θ(n) calls. Time and stack match.
- A binary recursion tree can have exponentially many calls and only depth `n`, as in naive Fibonacci. Time follows the number of calls. Stack follows the depth.
- Space for the frames is Θ(depth) plus whatever each frame stores. A frame that holds an array of size n makes the space worse than the depth alone.
- Tail-call elimination reuses one frame when the recursive call is the last action. Not every language does this, so a tail-recursive function can still overflow.
- If the depth would exceed the stack, rewrite the algorithm as a loop or as an explicit heap-allocated stack, or cache results so the depth stays the length of one chain of new states.

**Examples:**

`fact(100000)` is one thin chain of 100000 frames and often dies with a stack overflow even though the math is trivial. A loop uses one frame.
</details>

<details>
<summary>How does the Call Stack operate in recursive function calls?</summary>

### How does the Call Stack operate in recursive function calls?

**Definition:**

Each call pushes a frame that holds the arguments, the return address, and the local variables. The frame stays until that call returns. Recursion is a stack of those frames, one per open call.

**How it works:**

- Calling `fact(3)` pushes a frame with `n = 3`. It cannot multiply yet, so it calls `fact(2)`, which pushes another frame.
- The base case `fact(0)` or `fact(1)` pushes a frame that returns immediately. That pop is the first return.
- Each waiting frame then multiplies by its own `n` and pops. The stack unwinds from the deepest call back to the original caller.
- The maximum number of frames is the depth, not the total number of calls over the whole run. After a frame pops, its memory can be reused by a later sibling call.
- A debugger’s stack trace is this chain. The top frame is the call that has not returned.

**Examples:**

```text
fact(3) waits
  fact(2) waits
    fact(1) returns 1
  fact(2) returns 2
fact(3) returns 6
```
</details>

<details>
<summary>Are there any safety considerations when determining the Recursion Depth? If yes, provide an example.</summary>

### Are there any safety considerations when determining the Recursion Depth? If yes, provide an example.

**Definition:**

Yes. Depth is a resource limit. A deep or cyclic recursion exhausts the call stack, and an attacker who controls the depth can turn that into a denial of service.

**How it works:**

- The runtime gives each thread a fixed stack. Past that limit the process raises a stack overflow instead of returning an answer.
- A base case that is never hit, or a graph walk that does not mark visited vertices, is an infinite recursion. The overflow is the symptom.
- Untrusted input that sets `n`, or a nested document an attacker can make arbitrarily deep, should not be followed with one frame per level. Cap the depth, or use a loop and a heap stack you can limit.
- Even a correct recursion of depth tens of thousands can overflow a default stack that was sized for hundreds of frames.
- Tail-call optimization removes the growth only when the compiler actually rewrites the call. Do not rely on it for safety unless the language guarantees it.

**Examples:**

A recursive JSON walker on a 100,000-deep array of single-element arrays will overflow long before it finishes the document. An explicit stack with a max depth returns a controlled error.
</details>

### Recursive Algorithms

<details>
<summary>Explain the Depth-First Search algorithm.</summary>

### Explain the Depth-First Search algorithm.

**Definition:**

Depth-first search follows one outgoing edge as far as it can, then backtracks to the next untried edge. The frontier is a stack.

**How it works:**

- Mark the start visited so a cycle does not revisit it.
- Recurse on each unvisited neighbor, or push neighbors and pop them in a loop.
- When a call returns, the next neighbor of the caller is tried. That return is the backtrack.
- An empty stack means every vertex reachable from the start was seen. Another component needs a new start.
- Time is Θ(V + E) with an adjacency list. Stack space is Θ(V) on a long path.
- Use it for cycles, topological order, and connected components. It does not produce shortest paths.

**Examples:**

```text
def dfs(u):
  seen.add(u)
  for v in neighbors(u):
    if v not in seen:
      dfs(v)
```
</details>

<details>
<summary>Implement a Recursive Algorithm to perform a Binary Search.</summary>

### Implement a Recursive Algorithm to perform a Binary Search.

**Definition:**

Recursive binary search looks at the middle of the current range and calls itself on the half that can still hold the target.

**How it works:**

- If the range is empty (`lo > hi`), the target is absent.
- Let `mid = lo + (hi - lo) // 2`. The subtraction form avoids overflowing a fixed-width `lo + hi`.
- If `a[mid]` is the target, return `mid`. If the target is greater, search `mid + 1 .. hi`. If it is smaller, search `lo .. mid - 1`.
- The array must already be sorted, and `mid` must be readable in O(1).
- Time is Θ(log n) comparisons. The recursive form uses Θ(log n) stack frames. The iterative form uses Θ(1) extra memory and the same number of comparisons.

**Examples:**

```text
def bsearch(a, target, lo, hi):
  if lo > hi: return -1
  mid = lo + (hi - lo) // 2
  if a[mid] == target: return mid
  if a[mid] < target: return bsearch(a, target, mid + 1, hi)
  return bsearch(a, target, lo, mid - 1)
```
</details>

<details>
<summary>Solve the Tower of Hanoi problem recursively.</summary>

### Solve the Tower of Hanoi problem recursively.

**Definition:**

Tower of Hanoi moves `n` disks from a source peg to a destination peg, using a spare peg. Only one disk moves at a time, and a larger disk never sits on a smaller one.

**How it works:**

- To move `n` disks from `src` to `dst` using `spare`: move `n-1` from `src` to `spare` (using `dst`), move the largest disk from `src` to `dst`, then move `n-1` from `spare` to `dst` (using `src`).
- The base case is `n == 0`: do nothing. `n == 1` is a single move.
- The number of moves is `2^n − 1`. Each level doubles the work, so the time is Θ(2ⁿ).
- Stack depth is Θ(n), one frame per disk still waiting for the second half of its move.
- The recurrence is forced by the rules. There is no shorter sequence that obeys them.

**Examples:**

```text
hanoi(n, src, dst, spare):
  if n == 0: return
  hanoi(n-1, src, spare, dst)
  move disk n from src to dst
  hanoi(n-1, spare, dst, src)
```

Three disks take 7 moves.
</details>

<details>
<summary>Recursively Check for Palindromes in a string.</summary>

### Recursively Check for Palindromes in a string.

**Definition:**

A string is a palindrome when the ends match and the slice between them is a palindrome. The empty string and a single character are palindromes.

**How it works:**

- If the range has length 0 or 1, return true.
- If the first and last characters differ, return false.
- Otherwise recurse on the open interval between them.
- Skip this if you need to ignore case or spaces; normalize first, or pass indexes and a predicate.
- There are Θ(n) comparisons and Θ(n) stack frames if each call slices a new string. Passing indexes avoids the copies and keeps the extra heap memory at Θ(1), with the stack still Θ(n).
- Two pointers in a loop do the same comparisons in Θ(1) extra memory.

**Examples:**

```text
pal(s, i, j):
  if i >= j: return true
  if s[i] != s[j]: return false
  return pal(s, i+1, j-1)
```

`"radar"` matches at both ends twice and finishes on `"d"`. `"hello"` fails on `h` versus `o`.
</details>

<details>
<summary>Explain the process of performing a recursive In-order Tree Traversal.</summary>

### Explain the process of performing a recursive In-order Tree Traversal.

**Definition:**

In-order traversal of a binary tree visits the left subtree, then the node, then the right subtree. On a binary search tree that order is the sorted order of the keys.

**How it works:**

- If the node is missing, return. That is the base case.
- Recurse on the left child, emit the node’s key, recurse on the right child.
- The call stack holds the ancestors waiting to emit themselves after their left subtree finishes.
- Time is Θ(n) for n nodes. Stack depth is the height, Θ(log n) when the tree is balanced and Θ(n) when it is a spine.
- Pre-order emits the node before the children. Post-order emits it after both children. Only the in-order sandwich matches BST sort order.

**Examples:**

```text
inorder(node):
  if node is null: return
  inorder(node.left)
  emit(node.key)
  inorder(node.right)
```

A BST with root 2, left 1, right 3 emits `1, 2, 3`.
</details>

### Tail Recursion

<details>
<summary>Calculate N-th Fibonacci Number using Tail Recursion.</summary>

### Calculate N-th Fibonacci Number using Tail Recursion.

**Definition:**

Tail-recursive Fibonacci carries the two previous values as arguments, so the recursive call is the entire remaining work. Nothing is left to do after it returns.

**How it works:**

- `go(n, a, b)` returns `a` when `n` is 0. Otherwise it calls `go(n-1, b, a+b)`.
- Start with `go(n, 0, 1)`. After k steps, `a` is `F(k)`.
- Time is Θ(n). A compiler that does tail-call elimination reuses one stack frame, so the extra memory is Θ(1). Without that elimination the depth is still Θ(n).
- This is the iterative loop written so the last action is the call. It does not change the number of additions.

**Examples:**

```text
def fib(n, a=0, b=1):
  if n == 0: return a
  return fib(n-1, b, a+b)
```

`fib(6)` returns 8.
</details>

<details>
<summary>Discuss how Tail Recursion can be optimized by compilers and its benefits.</summary>

### Discuss how Tail Recursion can be optimized by compilers and its benefits.

**Definition:**

Tail-call optimization rewrites a call that is the last action of a function into a jump, reusing the current frame. A tail-recursive loop then uses constant stack space.

**How it works:**

- A call is in tail position when the caller returns its result unchanged. `return f(...)` can be a jump. `return n * f(...)` cannot, because the multiply is still waiting.
- The compiler replaces the push of a new frame with an update of the current arguments and a branch back to the start.
- Languages in the Scheme family require this. Many functional compilers do it. C, Java, and CPython do not guarantee it, so the same source can still grow the stack.
- The benefit is that a recursive definition of a loop stays a loop at runtime: large `n` does not overflow, and you do not pay a frame per step.
- It does not make an exponential recursion linear. Naive Fibonacci is not a tail call, because both recursive results are still needed.

**Examples:**

`return fact(n-1, acc*n)` is a tail call. `return n * fact(n-1)` is not: the frame must stay alive to multiply after the call returns.
</details>

<details>
<summary>What is the difference between Head Recursion and Tail Recursion?</summary>

### What is the difference between Head Recursion and Tail Recursion?

**Definition:**

Head recursion does the recursive call first and uses the result on the way back. Tail recursion does the work first and makes the recursive call as its last action.

**How it works:**

- Head recursion, as in printing a list from the bottom or computing `n * fact(n-1)`, must keep the frame alive. The stack holds every frame until the deepest call returns.
- Tail recursion passes the updated accumulator into the call and returns that call’s result directly. After the call there is nothing to multiply or print.
- Only the tail form is eligible for tail-call elimination. Head recursion always needs the stack proportional to the depth, because the pending work is the point.
- The same function can be rewritten from head to tail by adding an accumulator argument. Factorial’s pending multiply becomes `acc * n` passed downward.
- “Head” here means the call happens before the local work, not a special position in the source file.

**Examples:**

```text
head(n):                 tail(n, acc):
  if n == 0: return 1     if n == 0: return acc
  return n * head(n-1)    return tail(n-1, acc*n)
```
</details>

<details>
<summary>Use Tail Recursive function to calculate the Factorial of a number.</summary>

### Use Tail Recursive function to calculate the Factorial of a number.

**Definition:**
n! is n × (n − 1) × … × 1, and 0! is 1. A call is tail recursive when that call is the last thing the function does. Ordinary factorial multiplies after the recursive call returns, so it is not tail recursive.

**How it works:**
- Pass the running product in a second argument. When n drops to 1 or below, return that product.
- The recursive call is the returned value, so nothing in the frame is needed after it.
- A compiler can then avoid saving the frame and jump back to the start, as a loop or a goto.
- The iterative product is O(n) time and O(1) space. The plain recursive form is O(n) time and O(n) stack.
- **Time:** O(n) multiplications. **Space:** O(1) extra if the tail call reuses the frame; O(n) if each call keeps a frame.

**Examples:**
```text
fact(n):
    return fact_tr(n, 1)

fact_tr(n, acc):
    if n <= 1: return acc
    return fact_tr(n - 1, n * acc)

fact(5) → 120
fact(0) → 1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/tail-recursion/
- https://www.geeksforgeeks.org/dsa/program-for-factorial-of-a-number/

</details>

<details>
<summary>Can Tail Recursion be more efficient than non-tail recursion? Explain with an example.</summary>

### Can Tail Recursion be more efficient than non-tail recursion? Explain with an example.

**Definition:**
Yes, when the compiler reuses the current frame. A tail call is the last action, so the frame’s locals are dead. A non-tail call still has work after it returns, so that frame has to stay.

**How it works:**
- `return n * fact(n - 1)` must keep n until the recursive call returns. Depth n uses O(n) stack. The factorial article states that recursive form as O(n) time and O(n) space.
- `return fact(n - 1, n * acc)` does nothing after the call. The tail-recursion article says the compiler can send control back to the start with a loop or a goto and skip saving the frame.
- The number of multiplications stays O(n) either way. The iterative loop is O(n) time and O(1) space.
- **Time:** O(n). **Space:** O(n) for the non-tail form; the optimized tail form need not keep a frame per call.

**Examples:**
```text
non-tail: fact(3) waits to multiply by 3 after fact(2)
tail:     fact(3, 1) → fact(2, 3) → fact(1, 6) → 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/tail-recursion/
- https://www.geeksforgeeks.org/dsa/program-for-factorial-of-a-number/

</details>

### Converting Recursive to Iterative

<details>
<summary>Is it always possible to write a Non-Recursive form for every Recursive function?</summary>

### Is it always possible to write a Non-Recursive form for every Recursive function?

**Definition:**
Yes. The recursion article states that every recursive program can be written iteratively, and the other way around as well. The iterative form keeps the same pending work in a loop, using an explicit stack when the call is not the last action.

**How it works:**
- The interview notes convert a recursive solution by finding the recursive calls, replacing them with a loop, and storing parameters in an explicit stack until that stack is empty.
- The same notes bound auxiliary space by the maximum depth: O(n) in the worst case, because each frame sits on the call stack.
- Tail-call elimination is the special case. One frame is O(1); N frames are O(N). Reusing the current frame drops that to O(1).
- **Time:** the recurrence does not change. **Space:** the explicit stack replaces the call stack and stays proportional to the depth, O(n) in the worst case from the interview notes.

**Examples:**
```text
sum(n):
    acc = 0
    while n > 0:
        acc += n
        n -= 1
    return acc
# non-tail: push the frame, finish the work after pop
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/recursion/
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/tail-call-elimination/
</details>

<details>
<summary>Convert the given Recursive Function into an Iterative Function.</summary>

### Convert the given Recursive Function into an Iterative Function.

**Definition:**
Replace each recursive call with a loop. If nothing runs after the call, update the arguments in place. If work remains after the call, push that state on an explicit stack and finish it when the state is popped.

**How it works:**
- The interview notes list four steps: identify the calls, switch to a loop, store parameters or local state in a stack (or a queue when the order is level by level), and process until the stack is empty.
- A tail-recursive factorial becomes a loop. Tail-call elimination states that N frames cost O(N) and the eliminated form costs O(1).
- A non-tail call such as `n + sum(n - 1)` still needs the saved `n`. The explicit stack holds those values. Worst-case auxiliary space stays O(n), the bound the interview notes give for recursion depth.
- **Time:** O(n) for a single chain of n calls that does O(1) work each, the recurrence T(n) = T(n−1) + O(1) from the interview notes. **Space:** O(1) extra after a tail call is turned into a loop; O(n) when the explicit stack stores every frame.

**Examples:**
```text
def sum_iter(n):
    acc = 0
    while n > 0:
        acc += n
        n -= 1
    return acc
# sum_iter(3) == 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/recursion/
- https://www.geeksforgeeks.org/dsa/tail-call-elimination/
</details>

<details>
<summary>Describe how to simulate the recursive QuickSort algorithm iteratively.</summary>

### Describe how to simulate the recursive QuickSort algorithm iteratively.

**Definition:**
Recursive QuickSort partitions around a pivot, then sorts the two sides. The iterative form pushes those index ranges on an explicit stack and partitions in a loop until the stack is empty.

**How it works:**
- Lomuto partition, used in the QuickSort article, is O(n) and places the pivot in its final index. The two recursive calls are `quickSort(low, pi - 1)` and `quickSort(pi + 1, high)`.
- The iterative article stores those pairs `(low, high)` on a stack. Pop a range, partition it, and push each side that still has more than one element.
- Recursive bounds from the QuickSort article: best time Ω(n log n) with O(log n) stack, average θ(n log n), worst time O(n²) with O(n) stack when the pivot is always an extreme element.
- The iterative article states time O(n log n) and auxiliary space O(n) for its explicit stack of ranges.
- **Time:** Ω(n log n) best, θ(n log n) average, O(n²) worst, as the recursive article states. **Space:** O(log n) best-case stack, O(n) worst-case stack.

**Examples:**
```text
stack = [(0, n - 1)]
while stack:
    low, high = stack.pop()
    if low >= high:
        continue
    pi = partition(a, low, high)  # Lomuto, pivot a[high]
    stack.append((low, pi - 1))
    stack.append((pi + 1, high))
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/quick-sort/
- https://www.geeksforgeeks.org/dsa/iterative-quick-sort/
</details>

<details>
<summary>Convert a recursive Depth-First Search to an iterative approach using a Stack.</summary>

### Convert a recursive Depth-First Search to an iterative approach using a Stack.

**Definition:**
Recursive DFS marks the current vertex and calls itself on each unvisited neighbor. The iterative form pushes the start vertex on a stack, pops a vertex, marks it when it is first popped, and pushes its unvisited neighbors.

**How it works:**
- The iterative article starts by pushing node 0. While the stack is not empty it pops a node, skips it if it was already visited, otherwise marks it and appends it to the result.
- Neighbors are pushed from the end of the adjacency list toward the start so the first neighbor is popped first, matching a left-to-right recursive walk.
- A disconnected graph repeats that loop from every unvisited vertex.
- **Time:** O(V + E). **Space:** O(V). Both bounds are stated for the connected and the disconnected iterative DFS.

**Examples:**
```text
st = [0]
visited = [False] * n
while st:
    node = st.pop()
    if visited[node]:
        continue
    visited[node] = True
    for v in reversed(adj[node]):
        if not visited[v]:
            st.append(v)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/iterative-depth-first-traversal/
</details>

### Recursive Problems in Data Structures

<details>
<summary>Convert a Binary Tree into a Doubly Linked List.</summary>

### Convert a Binary Tree into a Doubly Linked List.

**Definition:**
An inorder walk visits a binary tree in sorted order when the tree is a BST, and in left-node-right order for any binary tree. Link each visited node to the previous one with `left` as `prev` and `right` as `next`, and the nodes become one doubly linked list.

**How it works:**
- The article gives three inorder conversions. Searching the predecessor or successor at every node is O(n) time and O(h) space. A single inorder pass that remembers the previous node is also O(n) time and O(h) space.
- Morris inorder threads the tree temporarily and converts it in O(n) time and O(1) extra space.
- The head of the list is the first inorder node. The tail is the last. Each tree edge used as a child pointer is rewritten as a list link.
- **Time:** O(n). **Space:** O(h) for the recursive inorder pass, O(1) extra for Morris.

**Examples:**
```text
def to_dll(node, prev):
    if node is None:
        return prev
    prev = to_dll(node.left, prev)
    node.left = prev
    if prev:
        prev.right = node
    return to_dll(node.right, node)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/convert-binary-tree-to-doubly-linked-list-using-inorder-traversal/
</details>

<details>
<summary>Reverse a Linked List recursively.</summary>

### Reverse a Linked List recursively.

**Definition:**
Recurse to the tail. That tail is the new head. On the way back, point the next node's `next` at the current node and clear the current node's `next`.

**How it works:**
- The base case is an empty list or a single node: return that node.
- After `rev = reverse(head.next)`, the node `head.next` is the tail of the already reversed suffix, so `head.next.next = head` appends the current node.
- Setting `head.next = None` cuts the old forward link. Every node is visited once.
- **Time:** O(n). **Space:** O(n) for the recursion stack. Both figures are stated on the recursive reversal article.

**Examples:**
```text
def reverse_list(head):
    if head is None or head.next is None:
        return head
    rev = reverse_list(head.next)
    head.next.next = head
    head.next = None
    return rev
# 1 -> 2 -> 3  becomes  3 -> 2 -> 1
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/recursively-reversing-a-linked-list-a-simple-implementation/
</details>

<details>
<summary>Sort a Stack using Recursion.</summary>

### Sort a Stack using Recursion.

**Definition:**
Pop the stack until it is empty, then insert each popped value back into the sorted stack so that it sinks to the first position where the value under it is smaller. No second stack is required.

**How it works:**
- The recursive article sorts with two recursive routines: one empties the stack, the other pulls out larger values until the new value can sit on a smaller one, then pushes the pulled values back.
- The same article also shows an auxiliary-stack sort. Both are stated as O(n²) time and O(n) auxiliary space.
- Each inserted value can travel past every value already sorted, which is where the quadratic bound comes from. The recursion depth follows the number of items.
- **Time:** O(n²). **Space:** O(n) auxiliary.

**Examples:**
```text
def insert(st, x):
    if not st or st[-1] <= x:
        st.append(x)
        return
    y = st.pop()
    insert(st, x)
    st.append(y)

def sort_stack(st):
    if not st:
        return
    x = st.pop()
    sort_stack(st)
    insert(st, x)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sort-a-stack-using-recursion/
</details>

<details>
<summary>Implement a recursive algorithm to Find the Lowest Common Ancestor in a BST.</summary>

### Implement a recursive algorithm to Find the Lowest Common Ancestor in a BST.

**Definition:**
The lowest common ancestor of two nodes is the deepest node that has both as descendants, counting a node as a descendant of itself. In a BST, walk left when both keys are smaller than the current node, right when both are larger, and stop when the keys split.

**How it works:**
- The recursive BST method is stated as O(h) time and O(h) space. One comparison pair decides the next child, so the walk is a single path of length h.
- If both keys are smaller, the ancestor is in the left subtree. If both are larger, it is in the right subtree. Otherwise the current node is the ancestor, including the case where the current node is one of the two keys.
- The iterative form of the same test is O(h) time and O(1) space. A general binary tree, without the BST order, falls back to a full search: the article's naive bound is O(n) time and O(n) space.
- **Time:** O(h). **Space:** O(h) recursive, O(1) iterative.

**Examples:**
```text
def find_lca(root, n1, n2):
    if root.data > n1 and root.data > n2:
        return find_lca(root.left, n1, n2)
    if root.data < n1 and root.data < n2:
        return find_lca(root.right, n1, n2)
    return root
# keys 10 and 14 under 12: 12 is the split
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/lowest-common-ancestor-in-a-binary-search-tree/
</details>

<details>
<summary>Solve the problem of recursively Flattening a Multilevel Doubly Linked List.</summary>

### Solve the problem of recursively Flattening a Multilevel Doubly Linked List.

**Definition:**
Each node has a `next` sibling and a `child` list. Flattening splices every child list into the `next` chain and clears `child`, so one level remains. The level-order article appends each child list at the current tail.

**How it works:**
- Keep `curr` on the node being scanned and `tail` on the last node of the growing list. When `curr.child` exists, set `tail.next = curr.child`, walk that child list to its end, move `tail` there, and set `curr.child = None`.
- That article states O(n) time, where n is the number of nodes, and O(1) auxiliary space. The links are rewritten in place.
- A depth-first flatten would recurse into `child` before `next`. The level-order procedure above is the one whose O(n) / O(1) bounds were fetched.
- On a doubly linked node, also set `child.prev = tail` when the child is spliced, and set `tail.next = None` only at the true end.
- **Time:** O(n). **Space:** O(1) auxiliary.

**Examples:**
```text
tail = head
while tail.next:
    tail = tail.next
curr = head
while curr:
    if curr.child:
        tail.next = curr.child
        curr.child.prev = tail
        while tail.next:
            tail = tail.next
        curr.child = None
    curr = curr.next
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/flatten-a-linked-list-with-next-and-child-pointers/
</details>

<details>
<summary>How would you recursively Serialize and Deserialize a Binary Tree?</summary>

### How would you recursively Serialize and Deserialize a Binary Tree?

**Definition:**
Serialization writes the tree into an array. Recursive preorder writes the node value, then the left subtree, then the right, and writes −1 for a null child. Deserialization reads that array in the same order and rebuilds the node when the value is not −1.

**How it works:**
- The preorder article states O(n) time and O(n) space. Every node and every null child becomes one entry, and the recursion follows the height.
- Deserialize consumes the next value. A −1 returns null. Any other value creates a node and recursively reads its left child, then its right child.
- The same article's level-order form is also O(n) time and O(n) space. It stores −1 for nulls and rebuilds with a queue. The recursive form is the preorder one.
- **Time:** O(n). **Space:** O(n).

**Examples:**
```text
def serialize(root, out):
    if root is None:
        out.append(-1)
        return
    out.append(root.data)
    serialize(root.left, out)
    serialize(root.right, out)

def deserialize(it):
    val = next(it)
    if val == -1:
        return None
    node = Node(val)
    node.left = deserialize(it)
    node.right = deserialize(it)
    return node
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/serialize-deserialize-binary-tree/
</details>

### Fundamental Recursive Problems

<details>
<summary>Calculate the N-th value of the Fibonacci Sequence recursively.</summary>

### Calculate the N-th value of the Fibonacci Sequence recursively.

**Definition:**
The article defines fib(0) = 0, fib(1) = 1, and fib(n) = fib(n−1) + fib(n−2) otherwise. The plain recursive form calls both predecessors. Memoization stores each result so it is computed once.

**How it works:**
- The plain recursion is stated as O(2ⁿ) time, because each state calls both previous states, and O(n) auxiliary space for the recursion stack.
- Memoization calculates each Fibonacci number once: O(n) time and O(n) auxiliary space for the table.
- The space-optimized bottom-up loop on the same page is O(n) time and O(1) space. Matrix exponentiation is stated as O(log n) time and O(log n) space.
- **Time:** O(2ⁿ) plain, O(n) with memoization. **Space:** O(n) for the plain stack and for the memo table.

**Examples:**
```text
def fib(n):
    if n == 0:
        return 0
    if n == 1 or n == 2:
        return 1
    return fib(n - 1) + fib(n - 2)
# fib(5) = 5; the call tree repeats fib(2) and fib(3)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/program-for-nth-fibonacci-number/
</details>

<details>
<summary>Write a recursive function to calculate Sum of Digits in a number.</summary>

### Write a recursive function to calculate Sum of Digits in a number.

**Definition:**
The sum of digits is the last digit plus the sum of digits of the number with that digit removed. The base case is a one-digit number, which is already the sum.

**How it works:**
- `n % 10` is the last digit. `n // 10` is the remaining number. The article stops when the value is less than 10, so a further call is unnecessary.
- Each call removes one decimal digit. The article states time O(log₁₀ n), because that is how many digits there are, and auxiliary space O(log₁₀ n) for the recursion.
- Negative inputs are handled by summing the digits of the absolute value, then the sign is not part of a digit sum. The fetched bound is for the digit walk.
- **Time:** O(log₁₀ n). **Space:** O(log₁₀ n).

**Examples:**
```text
def sum_digits(n):
    n = abs(n)
    if n < 10:
        return n
    return (n % 10) + sum_digits(n // 10)
# sum_digits(123) = 6
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sum-digit-number-using-recursion/
</details>

<details>
<summary>Implement Merge Sort recursively.</summary>

### Implement Merge Sort recursively.

**Definition:**
Merge sort divides the array into two halves, sorts each half by the same function, and merges the two sorted halves. One element is already sorted, which is the base case.

**How it works:**
- The article's recurrence is T(n) = Θ(1) when n = 1, and T(n) = 2T(n/2) + Θ(n) when n > 1. The Θ(n) term is the merge.
- Best, average, and worst time are all O(n log n). The merge uses a temporary array, so auxiliary space is O(n).
- The merge copies a left half and a right half, then writes the smaller head back. Equal keys take the left half first, which is why the sort is stable.
- **Time:** O(n log n). **Space:** O(n) auxiliary.

**Examples:**
```text
def merge_sort(a, left, right):
    if left >= right:
        return
    mid = (left + right) // 2
    merge_sort(a, left, mid)
    merge_sort(a, mid + 1, right)
    merge(a, left, mid, right)
# [38, 27, 43, 10] -> [10, 27, 38, 43]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/merge-sort/
</details>

<details>
<summary>Implement a recursive function to Print all Permutations of a given string.</summary>

### Implement a recursive function to Print all Permutations of a given string.

**Definition:**
Fix one index at a time. Swap every later character into that index, recurse on the next index, then swap back. When the index reaches the end, the current arrangement is one permutation.

**How it works:**
- The article generates permutations by recursion and swapping and states the time as O(n! · n). There are n! arrangements, and writing or scanning one of them takes Θ(n).
- The swap is undone after the recursive call, so the same buffer is reused. The depth of the calls is the length of the string, one fixed index per frame.
- Duplicate characters repeat identical strings unless identical swaps are skipped. The fetched bound counts the swap recursion, including repeated outputs such as the all-equal case the article lists.
- **Time:** O(n! · n).

**Examples:**
```text
def permute(s, i):
    if i == len(s):
        print("".join(s))
        return
    for j in range(i, len(s)):
        s[i], s[j] = s[j], s[i]
        permute(s, i + 1)
        s[i], s[j] = s[j], s[i]
# "AB" -> AB, BA
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/write-a-c-program-to-print-all-permutations-of-a-given-string/
</details>

<details>
<summary>Generate all Balanced Parentheses using a recursive approach.</summary>

### Generate all Balanced Parentheses using a recursive approach.

**Definition:**
A sequence of n pairs is balanced when it never has more closing parentheses than opening ones, and it ends with n of each. Backtracking places an opening parenthesis while any remain, and a closing one only when it would not outnumber the openings.

**How it works:**
- The naive generator builds every string of n pairs and then checks balance. The article states that as O(2ⁿ · n) time and O(n) space.
- The backtracking generator only builds valid sequences. Its time is O(Cₙ · n), where Cₙ is the n-th Catalan number (2n)! / ((n+1)! n!). Auxiliary space is O(n) for the recursion depth and the current string.
- The counts are the two arguments: openings still available, and closings still available. A closing parenthesis is legal only when fewer closings than openings have been placed.
- **Time:** O(Cₙ · n) for the backtracking form. **Space:** O(n) auxiliary.

**Examples:**
```text
def gen(n, open_left, close_left, cur):
    if open_left == 0 and close_left == 0:
        print(cur)
        return
    if open_left:
        gen(n, open_left - 1, close_left, cur + "(")
    if close_left > open_left:
        gen(n, open_left, close_left - 1, cur + ")")
# n = 2 -> (()), ()()
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/print-all-combinations-of-balanced-parentheses/
</details>

### Advanced Recursive Problems

<details>
<summary>Use recursive approach to solve the Skyline Problem.</summary>

### Use recursive approach to solve the Skyline Problem.

**Definition:**
The skyline is the outline of buildings given as left, right, and height. The fetched article's expected solution sweeps the corner x-coordinates and keeps active heights in a max-heap. A recursive divide splits the buildings and merges two outlines the way merge sort merges runs.

**How it works:**
- The naive sweep on that page is O(n²) time and O(n) space: every corner asks which building is tallest there.
- The expected sweep with a priority queue is O(n log n) time and O(n) space. The heap answers the current maximum height, and a key is emitted only when that height changes.
- Divide-and-conquer merge sort, on its own page, is the recursive pattern: T(n) = 2T(n/2) + Θ(n), which is O(n log n) time. Merging two sorted skylines is the same linear merge. The skyline page's stated expected bound is the one to quote for the problem: O(n log n) time and O(n) space.
- **Time:** O(n log n) expected, O(n²) naive. **Space:** O(n).

**Examples:**
```text
# buildings: (left, right, height)
# sweep events: (x, start, height) and (x, end, height)
# max-heap keeps active heights; emit (x, height) when the max changes
# recursive merge: split buildings, merge two sorted outlines
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/the-skyline-problem-using-divide-and-conquer-algorithm/
- https://www.geeksforgeeks.org/dsa/merge-sort/
</details>

<details>
<summary>Implement a recursive backtracking solution for the Sudoku Solver problem.</summary>

### Implement a recursive backtracking solution for the Sudoku Solver problem.

**Definition:**
Scan for an empty cell. Try digits 1 through 9 that do not repeat in that row, column, or 3×3 box. Recurse. If the recursive call fails, clear the digit and try the next one. A board with no empty cell is solved.

**How it works:**
- The backtracking article describes the same choose, explore, and undo loop. Sudoku is one of its standard problems.
- The Sudoku article states the plain backtracking time as O(9^(n·n)): every empty cell has 9 options. It states auxiliary space O(1) for that version, because the board is updated in place.
- The bitmask version on the same page is O(9^(n·n)) time and O(n) space. A bit test answers in O(1) whether a digit is free in the row, column, or box.
- For the usual 9×9 board, n = 9, so the exponential is 9^81 in the worst case before pruning. Pruning still sits inside that bound.
- **Time:** O(9^(n·n)). **Space:** O(1) auxiliary for the in-place search.

**Examples:**
```text
def solve(board, n=9):
    cell = find_empty(board)
    if cell is None:
        return True
    r, c = cell
    for d in range(1, 10):
        if safe(board, r, c, d):
            board[r][c] = d
            if solve(board, n):
                return True
            board[r][c] = 0
    return False
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/sudoku-backtracking-7/
- https://www.geeksforgeeks.org/dsa/introduction-to-backtracking-data-structure-and-algorithm-tutorials/
</details>

<details>
<summary>Find the K-th Order Statistic in an unsorted list using a recursive strategy.</summary>

### Find the K-th Order Statistic in an unsorted list using a recursive strategy.

**Definition:**
The k-th smallest element is the element that would sit at index k−1 after sorting. Quickselect partitions like QuickSort and recurses into only the side that contains that index.

**How it works:**
- Sorting the array and reading index k−1 is the article's naive method: O(n log n) time and O(1) extra space.
- A max-heap of size k is O(n log k) time and O(k) space.
- The recursive partition method on the same page is O(n²) time in the worst case and O(n log n) on average, which the article says beats the heap method on average. Auxiliary space is O(n) for the recursion stack in the worst case and O(log n) on average.
- After partition, if the pivot index equals k−1, return it. If the pivot index is larger, recurse left. If it is smaller, recurse right.
- **Time:** O(n²) worst, O(n log n) average for the recursive partition. **Space:** O(n) worst-case stack, O(log n) average.

**Examples:**
```text
def kth(a, k, low, high):
    pi = partition(a, low, high)  # Lomuto
    if pi == k - 1:
        return a[pi]
    if pi > k - 1:
        return kth(a, k, low, pi - 1)
    return kth(a, k, pi + 1, high)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/kth-smallest-largest-element-in-unsorted-array/
- https://www.geeksforgeeks.org/dsa/quick-sort/
</details>

<details>
<summary>Use a recursive algorithm to detect a Cycle in an Undirected Graph.</summary>

### Use a recursive algorithm to detect a Cycle in an Undirected Graph.

**Definition:**
An undirected graph has a cycle when a depth-first or breadth-first walk reaches a visited vertex that is not the parent of the current edge. Recursive DFS passes the parent so the edge just used is not reported as a cycle.

**How it works:**
- The undirected-cycle article detects a cycle in O(V + E) time. It visits each vertex once and each edge once on an adjacency list. Space is O(V) for the visited array; the BFS form also uses a queue of that order.
- Recursive DFS uses the same visited array. The call stack follows the path. A neighbor that is visited and is not the parent closes a cycle.
- A disconnected graph starts a new search from every unvisited vertex. One component can be a tree while another holds the cycle.
- **Time:** O(V + E). **Space:** O(V) for the visited set in the fetched undirected algorithm.

**Examples:**
```text
def dfs(u, parent):
    visited[u] = True
    for v in adj[u]:
        if not visited[v]:
            if dfs(v, u):
                return True
        elif v != parent:
            return True
    return False
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/detect-cycle-in-an-undirected-graph-using-bfs/
</details>

<details>
<summary>Find All Possible Full Binary Trees with N nodes recursively.</summary>

### Find All Possible Full Binary Trees with N nodes recursively.

**Definition:**
A full binary tree gives every node 0 children or 2. The parentheses article's Catalan number Cₙ = (2n)! / ((n+1)! n!) counts the balanced n-pair sequences, and the same split recurrence builds the trees: the root plus a full left tree and a full right tree whose node counts sum to n−1.

**How it works:**
- n must be odd. A full tree always has one more leaf than internal node, so an even n yields an empty list.
- For a root, every odd left size `i` from 1 to n−2 leaves n−1−i nodes, also odd, for the right. Recurse on both sizes and pair the results.
- Generating the n-pair parenthesis strings costs O(Cₙ · n) time and O(n) auxiliary space on that article. The tree builder follows the same Catalan recurrence; each tree written has n nodes.
- **Time:** O(Cₖ · n) with k = (n−1)/2, the parentheses bound on a Catalan family of size Cₖ. **Space:** O(n) recursion depth, plus the space of the trees themselves.

**Examples:**
```text
def full_trees(n):
    if n % 2 == 0:
        return []
    if n == 1:
        return [Node(0)]
    out = []
    for i in range(1, n, 2):
        for left in full_trees(i):
            for right in full_trees(n - 1 - i):
                root = Node(0)
                root.left, root.right = left, right
                out.append(root)
    return out
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/print-all-combinations-of-balanced-parentheses/
</details>

### Recursion in Algorithms Design

<details>
<summary>Explain how Divide and Conquer is a form of recursion and give examples.</summary>

### Explain how Divide and Conquer is a form of recursion and give examples.

**Definition:**
Divide and conquer splits a problem into smaller copies, solves the copies by the same procedure, and combines the results. The recursive call is the conquer step. The base case is the copy that is small enough to solve directly.

**How it works:**
- The recursion article lists divide-and-conquer, and names binary search, mergesort, and quicksort as recursive applications. The divide-and-conquer article assumes the subproblems have the same size.
- Mergesort's recurrence is T(n) = 2T(n/2) + Θ(n), which solves to O(n log n) time with O(n) auxiliary space.
- The divide-and-conquer page states closest pair of points in O(n log n), the straightforward pair scan in O(n²), a simple matrix product in O(n³), Strassen in O(n^2.8974), and the Cooley–Tukey FFT in O(n log n).
- QuickSort's recursive form is Ω(n log n) best, θ(n log n) average, O(n²) worst.
- **Time:** O(n log n) for mergesort in every case. **Space:** O(n) auxiliary for the merge.

**Examples:**
```text
def merge_sort(a, left, right):
    if left >= right:
        return
    mid = (left + right) // 2
    merge_sort(a, left, mid)
    merge_sort(a, mid + 1, right)
    merge(a, left, mid, right)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/recursion/
- https://www.geeksforgeeks.org/dsa/introduction-to-divide-and-conquer-algorithm/
- https://www.geeksforgeeks.org/dsa/merge-sort/
- https://www.geeksforgeeks.org/dsa/quick-sort/
</details>

<details>
<summary>Use recursion to solve the Longest Common Subsequence problem.</summary>

### Use recursion to solve the Longest Common Subsequence problem.

**Definition:**
A common subsequence appears in both strings in order, not necessarily contiguously. If the last characters match, the answer is 1 plus the LCS of the prefixes. If they differ, the answer is the longer of the two results that drop one string's last character.

**How it works:**
- The naive recursion is O(2^min(m, n)) time and O(min(m, n)) space. Each mismatch branches into two calls, and the stack follows the shorter string.
- Memoization, top-down DP, is O(m · n) time and O(m · n) space. Each pair of prefixes is solved once.
- Bottom-up tabulation is the same O(m · n) time and O(m · n) space. The one-array optimization on that page is O(m · n) time and O(n) space.
- **Time:** O(2^min(m, n)) plain, O(m · n) memoized. **Space:** O(min(m, n)) plain, O(m · n) for the memo table.

**Examples:**
```text
def lcs(a, b, i, j):
    if i == 0 or j == 0:
        return 0
    if a[i - 1] == b[j - 1]:
        return 1 + lcs(a, b, i - 1, j - 1)
    return max(lcs(a, b, i - 1, j), lcs(a, b, i, j - 1))
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/longest-common-subsequence-dp-4/
</details>

<details>
<summary>Define the role of recursion in the design of Greedy Algorithms.</summary>

### Define the role of recursion in the design of Greedy Algorithms.

**Definition:**
A greedy algorithm commits to a locally best choice and then solves the remaining instance. Recursion is one way to write that remainder: the call receives the items the choice did not consume. The choice itself is not revisited.

**How it works:**
- The recursion article lists backtracking and divide-and-conquer as recursive families. Greedy is the relative that does not branch: one choice, then one smaller problem.
- Huffman coding is the concrete greedy tree on the fetched page. Each step joins the two least frequent nodes. The article states O(n log n) time, where n is the number of distinct characters, and O(n) space.
- The recursive shape is `build(forest) = leaf` when one tree remains, otherwise extract two minimums, make them children of a new node, and recurse. An iterative heap does the same joins.
- Backtracking would try the other join. Greedy does not.
- **Time:** O(n log n) for Huffman. **Space:** O(n).

**Examples:**
```text
def huffman(forest):
    if len(forest) == 1:
        return forest[0]
    a, b = extract_two_min(forest)
    return huffman(forest + [Node(a.freq + b.freq, a, b)])
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/greedy-algorithms/
- https://www.geeksforgeeks.org/dsa/huffman-coding-greedy-algo-3/
- https://www.geeksforgeeks.org/dsa/recursion/
</details>

<details>
<summary>How is recursion utilized in the Backtracking paradigm for algorithm design?</summary>

### How is recursion utilized in the Backtracking paradigm for algorithm design?

**Definition:**
Backtracking builds a candidate one choice at a time. Recursion explores that choice. If the partial candidate is illegal, the choice is undone and the next option is tried. The call stack is the list of choices still in force.

**How it works:**
- The backtracking article names the loop choose, explore, check, and undo. Recursion is the explore step. Tree walks and mergesort are cited there as recursion that does not undo a choice; N-Queens, Sudoku, and permutations are the backtracking examples.
- Sudoku's in-place search is O(9^(n·n)) time and O(1) auxiliary space. The exponential is the branching. The undo step is what makes the next branch see a clean cell.
- Permutations by swap-and-undo are O(n! · n) time. Balanced parentheses by the same undo are O(Cₙ · n) time and O(n) auxiliary space.
- **Time:** problem-dependent; Sudoku O(9^(n·n)), permutations O(n! · n). **Space:** the recursion depth equals the number of choices still open, O(1) extra beyond the board for in-place Sudoku.

**Examples:**
```text
def bt(state):
    if rejected(state):
        return
    if complete(state):
        emit(state)
        return
    for choice in options(state):
        apply(state, choice)
        bt(state)
        undo(state, choice)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/introduction-to-backtracking-data-structure-and-algorithm-tutorials/
- https://www.geeksforgeeks.org/dsa/sudoku-backtracking-7/
- https://www.geeksforgeeks.org/dsa/write-a-c-program-to-print-all-permutations-of-a-given-string/
- https://www.geeksforgeeks.org/dsa/print-all-combinations-of-balanced-parentheses/
</details>

<details>
<summary>Illustrate the use of recursion in Dynamic Programming with an example problem.</summary>

### Illustrate the use of recursion in Dynamic Programming with an example problem.

**Definition:**
Dynamic programming is recursion whose repeated subproblems are stored. For the longest common subsequence, the recursive cases are the matching last characters and the two mismatches. Memoization fills a cell the first time that pair of prefixes appears.

**How it works:**
- Plain LCS recursion is O(2^min(m, n)) time and O(min(m, n)) space. The same page's memoized, top-down form is O(m · n) time and O(m · n) space.
- Fibonacci is the smaller picture. Plain recursion is O(2ⁿ) time and O(n) stack. Memoization is O(n) time and O(n) for the table, because each index is computed once.
- Rod cutting on the memoization-versus-tabulation page is the other worked example: top-down memoization is O(n²) time and O(n) space, and bottom-up tabulation is O(n²) time and O(n) space.
- The recursive call does not change. The cache is what removes the second visit.
- **Time:** LCS drops from O(2^min(m, n)) to O(m · n). **Space:** O(m · n) for the memo table.

**Examples:**
```text
def lcs(i, j, memo):
    if i == 0 or j == 0:
        return 0
    if (i, j) in memo:
        return memo[(i, j)]
    if a[i - 1] == b[j - 1]:
        memo[(i, j)] = 1 + lcs(i - 1, j - 1, memo)
    else:
        memo[(i, j)] = max(lcs(i - 1, j, memo), lcs(i, j - 1, memo))
    return memo[(i, j)]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/longest-common-subsequence-dp-4/
- https://www.geeksforgeeks.org/dsa/program-for-nth-fibonacci-number/
- https://www.geeksforgeeks.org/dsa/tabulation-vs-memoization/
</details>

### Optimization in Recursive Functions

<details>
<summary>Define Memoization and explain how it optimizes recursive solutions.</summary>

### Define Memoization and explain how it optimizes recursive solutions.

**Definition:**
Memoization stores the result of a recursive call under its arguments and returns the stored result on the next call with those arguments. The recursion stays top-down. The repeated branches disappear.

**How it works:**
- The interview notes define it as storing solved subproblems, usually in an array or a map, so they are not computed again.
- Fibonacci shows the change in numbers: plain recursion O(2ⁿ) time and O(n) stack, memoization O(n) time and O(n) for the table.
- LCS goes from O(2^min(m, n)) time and O(min(m, n)) space to O(m · n) time and O(m · n) space.
- Rod cutting's top-down form is O(n²) time and O(n) space. The table must cover every argument tuple that the recursion actually reaches.
- **Time:** one computation per state, then O(1) lookups. **Space:** the table, O(n) for Fibonacci and rod cutting, O(m · n) for LCS.

**Examples:**
```text
memo = {}
def fib(n):
    if n in memo:
        return memo[n]
    if n < 2:
        memo[n] = n
    else:
        memo[n] = fib(n - 1) + fib(n - 2)
    return memo[n]
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/program-for-nth-fibonacci-number/
- https://www.geeksforgeeks.org/dsa/longest-common-subsequence-dp-4/
- https://www.geeksforgeeks.org/dsa/tabulation-vs-memoization/
</details>

<details>
<summary>What’s the impact of Stack Overflow in recursive algorithms and how is it avoided?</summary>

### What’s the impact of Stack Overflow in recursive algorithms and how is it avoided?

**Definition:**
Each non-tail call keeps a frame until the deeper calls finish. Factorial of n keeps n frames. When that chain is longer than the call stack, the program runs out of stack space.

**How it works:**
- The recursive factorial is O(n) time and O(n) space. The iterative product is the same O(n) time with O(1) extra space, so it does not grow a frame per multiply.
- A tail call can be rewritten by the compiler as a loop or a goto, because nothing remains to do in the current frame. That removes the chain of frames for the factorial accumulator.
- **Space:** O(n) frames for `return n * fact(n - 1)`. O(1) extra for the loop.

**Examples:**
```text
iterative, O(1) extra:
    acc = 1
    for i in 2..n: acc = acc * i
    return acc
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/program-for-factorial-of-a-number/
- https://www.geeksforgeeks.org/dsa/tail-recursion/

</details>

<details>
<summary>Discuss Space Complexity and how it relates to iterative vs. recursive solutions.</summary>

### Discuss Space Complexity and how it relates to iterative vs. recursive solutions.

**Definition:**
The recursive factorial stores one frame per level. The iterative factorial stores the running product and the loop index.

**How it works:**
- Both do Θ(n) multiplications, so both are O(n) time.
- Recursive: O(n) space, one frame for each call down to the base case.
- Iterative: O(1) space. The answer starts at 1 and is multiplied by 2, 3, …, n.
- A tail-recursive factorial describes the same product. A compiler that turns the tail call into a loop or goto does not have to keep the O(n) frames.
- **Time:** O(n). **Space:** O(n) recursive, O(1) iterative.

**Examples:**
```text
n = 4
loop: 1 → 2 → 6 → 24, two variables
recursion: frames for 4, 3, 2, 1, then the multiplies unwind
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/program-for-factorial-of-a-number/
- https://www.geeksforgeeks.org/dsa/tail-recursion/

</details>

<details>
<summary>Explain Tail Call Optimization (TCO) and its support in various programming languages.</summary>

### Explain Tail Call Optimization (TCO) and its support in various programming languages.

**Definition:**
Tail-call elimination reuses the current stack frame when the recursive call is the last action. The tail-call article says one frame is O(1) memory, N calls are O(N), and eliminating the call drops the recursion's space from O(N) to O(1). No new frame is created; the function continues in constant memory.

**How it works:**
- The call must be in tail position. `return n * fact(n - 1)` is not: the multiply happens after the call, so the frame that holds n has to stay. `return fact(n - 1, n * acc)` is: nothing uses the frame after the call.
- The interview notes say some compilers turn that tail call into a loop, and that this is what reduces stack use. They also say the optimization applies only when the implementation supports it.
- The elimination article does not tie the O(N) to O(1) drop to one language. Where the compiler does not reuse the frame, a tail-recursive function still uses one frame per call, and the iterative loop is the form that keeps O(1) extra memory.
- **Time:** the N steps remain. **Space:** O(N) frames before elimination, O(1) after it.

**Examples:**
```text
def fact(n, acc=1):
    if n == 0:
        return acc
    return fact(n - 1, n * acc)  # tail position

def fact_loop(n):
    acc = 1
    while n > 0:
        acc *= n
        n -= 1
    return acc
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/tail-call-elimination/
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
</details>

<details>
<summary>Implement a recursive algorithm with an explicit Manual Stack to avoid Stack Overflow.</summary>

### Implement a recursive algorithm with an explicit Manual Stack to avoid Stack Overflow.

**Definition:**
A manual stack stores the frames the call stack would have stored. The loop pops a frame, does the work, and pushes the next frames. The native call depth stays constant, so a long chain does not overflow the call stack.

**How it works:**
- The interview notes give this as the conversion method and list the benefit: it removes the stack-overflow risk of deep recursion. Worst-case auxiliary space of the recursive form is O(n), the depth.
- Iterative DFS is the graph instance. It is O(V + E) time and O(V) space. The explicit stack holds vertices, not native frames.
- Tail-call elimination is the other escape, and only for a tail call: space goes from O(N) to O(1). A manual stack is what remains when the call is not in tail position or the language will not reuse the frame.
- **Time:** O(V + E) for the DFS instance. **Space:** O(V) for its explicit stack and visited array.

**Examples:**
```text
st = [root]
while st:
    node = st.pop()
    if node is None:
        continue
    visit(node)
    st.append(node.right)
    st.append(node.left)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/iterative-depth-first-traversal/
- https://www.geeksforgeeks.org/dsa/tail-call-elimination/
</details>

### Tricky Recursive Cases and Handling

<details>
<summary>How to handle negative input cases in a recursive Absolute Value function?</summary>

### How to handle negative input cases in a recursive Absolute Value function?

**Definition:**
The absolute value is the non-negative magnitude. A negative argument is one recursive call on the negated value. The base case returns a non-negative argument unchanged. The digit-sum article uses the same sign normalization before a recursive digit walk.

**How it works:**
- `abs(n)` returns n when n ≥ 0. When n < 0 it returns `abs(-n)`. That is one extra frame, then the base case. The interview notes bound auxiliary space by the depth; here the depth is 1 past the base case.
- Do not recurse by subtracting one until zero for a negative start. That walks |n| frames. The sign flip reaches the base case in a single call.
- The most negative fixed-width integer cannot be negated in the same width. Handle that value before the recursive call, or compute the magnitude in a wider type.
- **Time:** O(1) for the sign flip plus the base case. **Space:** O(1) frames beyond the caller.

**Examples:**
```text
def abs_rec(n):
    if n >= 0:
        return n
    return abs_rec(-n)
# abs_rec(-4) -> abs_rec(4) -> 4
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/recursion/
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/sum-digit-number-using-recursion/
</details>

<details>
<summary>Solve the problem of computing all possible combinations of well-formed m-n pairs recursively.</summary>

### Solve the problem of computing all possible combinations of well-formed m-n pairs recursively.

**Definition:**
Well-formed pairs are the balanced-parentheses sequences. With a budget of openings and a budget of closings, recursion places an opening while openings remain, and a closing only when more openings than closings have already been placed. For n pairs the two budgets both start at n.

**How it works:**
- The backtracking article states time O(Cₙ · n), where Cₙ = (2n)! / ((n+1)! n!), and auxiliary space O(n) for the depth and the current string.
- The naive filter that builds every string first is O(2ⁿ · n) time and O(n) space. The counters avoid those illegal prefixes.
- Two pair types, round and square, are the same recursion with two opening budgets. A closer is legal only for a type that is still open. The fetched Catalan bound is for one type and n pairs.
- **Time:** O(Cₙ · n). **Space:** O(n) auxiliary.

**Examples:**
```text
def pairs(open_left, close_left, cur):
    if open_left == 0 and close_left == 0:
        print(cur)
        return
    if open_left:
        pairs(open_left - 1, close_left, cur + "(")
    if close_left > open_left:
        pairs(open_left, close_left - 1, cur + ")")
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/print-all-combinations-of-balanced-parentheses/
</details>

<details>
<summary>Discuss strategies to visually debug recursive functions.</summary>

### Discuss strategies to visually debug recursive functions.

**Definition:**
Print the arguments when a call starts and the result when it returns, indented by the depth. The recursion article's `printFun(3)` shows the stack picture: output `3 2 1 1 2 3`, the calls going down and the work after the call coming back up.

**How it works:**
- Indent with the depth argument. Pass `depth + 1` on the recursive call. The first print is the pre-order visit. The print after the recursive call is the post-order visit. That split is exactly what `printFun` demonstrates.
- Log the base case on its own line. A missing base case is what the recursion article says leads to stack overflow: the base never hits, and each call allocates another frame.
- The interview notes bound that stack by O(n) in the worst case, n being the maximum depth. A printed trace that grows without a base-case line is that overflow in slow motion.
- Draw one node per call, with an edge to each recursive call. The Fibonacci article's plain recursion is the trace where the same argument appears many times; that picture is the reason to memoize.
- **Space:** the trace itself is one line per call. The algorithm's own stack is still O(depth), O(n) worst case in the interview notes.

**Examples:**
```text
def fib(n, depth=0):
    print("  " * depth + f"fib({n})")
    if n < 2:
        return n
    return fib(n - 1, depth + 1) + fib(n - 2, depth + 1)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/recursion/
- https://www.geeksforgeeks.org/dsa/commonly-asked-data-structure-interview-questions-on-recursion/
- https://www.geeksforgeeks.org/dsa/program-for-nth-fibonacci-number/
</details>

<details>
<summary>Recursively determine if a Graph is Connected.</summary>

### Recursively determine if a Graph is Connected.

**Definition:**
An undirected graph is connected when a traversal from any vertex reaches every vertex. Recursive DFS marks the start component. The graph is connected when the marked count equals V. The connected-components article finds every such piece.

**How it works:**
- Start DFS at vertex 0, or at any vertex. Mark it, then recurse on each unmarked neighbor.
- The article's DFS over components is O(V + E) time: every vertex is visited at most once and every edge is traversed at most once. Auxiliary space is O(V + E), covering the stored graph and the recursive stack.
- BFS on the same page is O(V + E) time and O(V) space, one search per component. For a single connectivity test, one search is enough: if a vertex stays unmarked, the graph is disconnected.
- A directed graph needs weak connectivity on the underlying undirected edges, or strong connectivity with a second search on the reversed edges. The fetched bounds are for the undirected component walk.
- **Time:** O(V + E). **Space:** O(V + E) auxiliary for the recursive DFS in that article.

**Examples:**
```text
def dfs(u):
    visited[u] = True
    for v in adj[u]:
        if not visited[v]:
            dfs(v)

def connected():
    dfs(0)
    return all(visited)
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/connected-components-in-an-undirected-graph/
</details>

<details>
<summary>Recursively implement a Regular Expression Matcher.</summary>

### Recursively implement a Regular Expression Matcher.

**Definition:**
A recursive matcher walks a text and a pattern together. The fetched wildcard article is the concrete form: `?` matches one character, `*` matches any sequence, and any other pattern character must equal the text character. If the match fails, that branch returns false.

**How it works:**
- The naive recursion is O(2^(n+m)) time and O(n) space. The star is the branch: consume one text character and keep the star, or drop the star. n is the text length and m is the pattern length.
- Memoization on that page is O(n · m) time and O(n · m) space. Bottom-up DP is the same bound. The space-optimized DP is O(n · m) time and O(m) space.
- A `.` in a regular expression is the same one-character match as `?`. A `*` that repeats the preceding atom is the same choice the wildcard star makes: take one more character or stop. The numbers above are the wildcard article's numbers.
- The base cases are an empty pattern, which matches only an empty text, and a pattern of only stars, which matches any remaining text.
- **Time:** O(2^(n+m)) plain, O(n · m) memoized. **Space:** O(n) plain, O(n · m) memoized.

**Examples:**
```text
def match(text, pat, i, j):
    if j == len(pat):
        return i == len(text)
    if pat[j] == "*":
        return match(text, pat, i, j + 1) or (
            i < len(text) and match(text, pat, i + 1, j)
        )
    if i < len(text) and (pat[j] == "?" or pat[j] == text[i]):
        return match(text, pat, i + 1, j + 1)
    return False
```

**Sources:**
- https://www.geeksforgeeks.org/dsa/wildcard-pattern-matching/
</details>

### Recursion Theory Questions

<details>
<summary>Explain the relationship between Recursion and Lambda Calculus.</summary>

### Explain the relationship between Recursion and Lambda Calculus.

**Definition:**
Pure lambda calculus has no named functions. Recursion is a fixed point: a function F such that F(x) = x. The Y combinator produces that x from F, so the recursive call is an application, not a name that refers to itself.

**How it works:**
- The fixed-point page defines recursive definitions through fixed-point combinators and works the factorial as the example. Y(F) is a value that F returns unchanged, which is the recursive function.
- Unfolding once gives the next call: the factorial combinator receives n, and the recursive application is the call on n−1. Nothing in the calculus needs a stack instruction; the call stack is an implementation of that unfolding.
- The lambda-calculus page notes that a good measure of space complexity for reduction is not currently known. Do not attach an O-bound to Y itself from that remark.
- A programming language with named recursion is sugar over the same fixed point. Tail-call elimination, O(N) frames down to O(1), is an implementation choice about how those applications are stored, not a change to the combinator.
- **Space:** the fixed-point page does not state a reduction bound. Frame costs belong to the implementation: O(N) before tail-call elimination, O(1) after it.

**Examples:**
```text
# Y(F) = F(Y(F))
# fact = Y(lambda f: lambda n: 1 if n == 0 else n * f(n - 1))
# fact(3) unfolds to 3 * (2 * (1 * 1))
```

**Sources:**
- https://en.wikipedia.org/wiki/Fixed-point_combinator
- https://en.wikipedia.org/wiki/Lambda_calculus
- https://www.geeksforgeeks.org/dsa/tail-call-elimination/
</details>
