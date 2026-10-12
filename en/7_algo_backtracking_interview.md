# Backtracking — Interview

Interview answers in this file are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Backtracking — Interview](./7_algo_backtracking_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Backtracking](https://devinterview.io/questions/data-structures-and-algorithms/backtracking-algorithms-interview-questions/).

### Fundamental Concepts and Techniques

<details>
<summary>What is Backtracking?</summary>

### What is Backtracking?

**Definition:**

Backtracking builds a candidate one choice at a time and undoes the latest choice as soon as it cannot lead to a valid solution.

**How it works:**

- The search is a depth-first walk over partial solutions. At each step you pick a legal extension, recurse, then restore the state.
- A constraint check on the partial candidate stops a whole subtree. That early stop is the difference from listing every complete object.
- The call stack stores the path from the root to the current choice. Returning from the call is the undo.
- It fits puzzles with constraints: N-queens, subsets that sum to a target, Sudoku, and generating permutations.

**Examples:**

```text
choose 1, then 2 -> sum 3, too small, continue
choose 1, 2, 4 -> sum 7, target hit, record it
undo 4, undo 2, try 1 then 3
```
</details>

<details>
<summary>How does backtracking differ from brute force methods?</summary>

### How does backtracking differ from brute force methods?

**Definition:**

Brute force generates complete candidates and tests them at the end. Backtracking rejects a partial candidate and never finishes the branches under it.

**How it works:**

- Both can be exponential. Backtracking's saving is the pruned volume, which depends on how early a constraint fails.
- Brute force over permutations of 8 queens tests 8!. Backtracking places queen by queen and drops a row as soon as two queens share a column or a diagonal.
- If the constraint can only be checked on a finished candidate, backtracking degenerates into the same tree as brute force.
- The code shape is also different: brute force often builds the object in a loop or by recursion without an undo of a failed prefix.
</details>

<details>
<summary>Explain the concept of a decision tree in backtracking algorithms.</summary>

### Explain the concept of a decision tree in backtracking algorithms.

**Definition:**

The decision tree is the tree of choices: each level is one variable, and each edge is one value you might assign.

**How it works:**

- The root is the empty assignment. A leaf is either a complete valid solution or a dead end.
- Left-to-right order among siblings is the order you try values. Depth-first search walks one path to a leaf before the next sibling.
- Pruning deletes a node and everything below it when the partial assignment already violates a constraint.
- Drawing the tree is how you explain the algorithm even when the implementation is only a recursive function.

**Examples:**

Subset sum on `{1, 2}` with target 2: the first level is 'take 1 or not', the second is 'take 2 or not'. The path take-1, skip-2 sums to 1 and dies. The path skip-1, take-2 sums to 2 and is a leaf solution.
</details>

<details>
<summary>Discuss common optimizations in backtracking to improve efficiency.</summary>

### Discuss common optimizations in backtracking to improve efficiency.

**Definition:**

Optimizations shrink the tree by failing earlier, trying promising values first, or remembering a state you have already solved.

**How it works:**

- Forward checking removes values that the latest assignment has made impossible, before the recursive call.
- Variable ordering, such as the smallest remaining domain, fails high in the tree instead of after a long prefix.
- Bit sets and incremental scores update a constraint in O(1) or O(width) instead of rescanning the whole partial solution.
- Memoization applies only when the same sub-state can be reached by different paths. A pure permutation search often has no repeated state.
</details>

<details>
<summary>How does backtracking relate to other algorithmic paradigms like divide and conquer?</summary>

### How does backtracking relate to other algorithmic paradigms like divide and conquer?

**Definition:**

Both are recursive, but they split the work differently. Divide and conquer solves independent subproblems and combines them. Backtracking searches one path, then undoes it and tries another.

**How it works:**

- Divide and conquer's subproblems do not share a mutable board. Mergesort's halves can be sorted separately.
- Backtracking's recursive call inherits the board and must restore it, because the sibling call needs the parent state.
- A problem can use both: a divide step that then backtracks inside one region, or a backtracking search whose bounding function comes from a closed-form estimate.
- Dynamic programming is the next relative when subproblems repeat and you store answers instead of re-walking them.
</details>

<details>
<summary>Describe the role of state space tree in understanding backtracking algorithms.</summary>

### Describe the role of state space tree in understanding backtracking algorithms.

**Definition:**

The state-space tree is the decision tree drawn for one run: nodes are partial states, and the algorithm visits only some of them.

**How it works:**

- State means the variables assigned so far, plus whatever incremental facts you keep (used columns, current sum).
- The full tree is conceptual. The running algorithm stores only the current path, which is why the extra memory is proportional to the depth.
- A promising node still has a legal extension. A non-promising node fails the constraint and is not expanded.
- Counting visited nodes, not the size of the full tree, is the real cost of a particular ordering and pruning rule.
</details>

<details>
<summary>Explain the concept of constraint satisfaction in backtracking.</summary>

### Explain the concept of constraint satisfaction in backtracking.

**Definition:**

A constraint-satisfaction problem is a set of variables, a domain for each, and constraints that combinations must obey. Backtracking assigns variables until every constraint is satisfied.

**How it works:**

- Unary constraints limit one variable. Binary constraints link a pair, such as two queens not sharing a diagonal.
- A partial assignment is checked against the constraints that mention only assigned variables. Waiting for a full assignment wastes the prefix.
- Constraint propagation tightens domains after each assignment. Arc consistency removes a value that has no support in a neighbor's domain.
- Sudoku is the standard picture: 81 variables, domains 1–9, and all-different constraints on rows, columns, and boxes.
</details>

### Implementing Backtracking

<details>
<summary>Outline a method to implement backtracking iteratively.</summary>

### Outline a method to implement backtracking iteratively.

**Definition:**

An explicit stack stores frames of `(variable index, next value to try, snapshot)`, and a loop pushes and pops those frames instead of using the call stack.

**How it works:**

- Push the first variable with its first candidate. To go deeper, push the next variable. To backtrack, pop and advance the value on the new top.
- Undo is whatever the frame recorded: clear a cell, restore a sum, or pop a chosen item.
- The loop ends when the stack is empty. A frame that has tried every value is popped.
- This matches recursion's depth-first order and avoids a call-stack overflow on a deep assignment. The logic is easier to get wrong because the undo is manual.

**Examples:**

```text
stack: [(col=0, next_row=0)]
place queen, push (col=1, next_row=0)
dead end -> pop, next_row += 1, try again
```
</details>

<details>
<summary>What are the considerations for choosing candidates at each step in a backtracking algorithm?</summary>

### What are the considerations for choosing candidates at each step in a backtracking algorithm?

**Definition:**

A candidate must be legal given the assignments so far, and the order you try them changes how fast you find the first solution.

**How it works:**

- Generate from the domain, then reject with the constraint, or generate only values that already pass a cheap local check.
- For the first solution, try values that are more likely to succeed first. For every solution, the order does not change the set, only the visit order.
- Least-constraining value tries the choice that rules out the fewest options for neighbors.
- A candidate generator that itself scans the whole board can dominate the runtime even when pruning is good.
</details>

<details>
<summary>Describe the role of pruning in backtracking algorithms.</summary>

### Describe the role of pruning in backtracking algorithms.

**Definition:**

Pruning refuses to expand a node whose partial assignment is already doomed, so those descendants are never built.

**How it works:**

- Feasibility pruning uses a hard constraint: a queen under attack, a sum already past the target, a Sudoku digit repeated.
- Bound pruning uses an estimate: if the best possible completion is worse than a solution you already have, drop the branch. That is the branch-and-bound variant.
- Pruning is safe only when the test has no false failures. A test that rejects a prefix of a real solution loses answers.
- The same algorithm with a weaker test visits many more nodes. Correctness can stay, and the runtime explodes.
</details>

<details>
<summary>How can memoization be integrated with backtracking?</summary>

### How can memoization be integrated with backtracking?

**Definition:**

Memoization stores the answer for a sub-state the first time you solve it, and later paths that reach the same state reuse that answer.

**How it works:**

- The key must include everything that still affects the result: the index you start from, the remaining target, the used mask. Forgetting a field returns a wrong cached answer.
- It helps subset-sum style searches, where many paths share a suffix. It does not help much when every path has a unique board.
- The memory is the number of distinct states, which can be larger than the depth. That is the usual DP trade.
- Store a boolean, a count, or one reconstructed solution. Store the whole list of solutions only when you truly need all of them.

**Examples:**

Knapsack remaining capacity 7 at item 3 is one state. The first path computes it. A later path that also arrives at item 3 with capacity 7 reads the cached best value.
</details>

<details>
<summary>Explain the importance of backtracking in recursive algorithm design.</summary>

### Explain the importance of backtracking in recursive algorithm design.

**Definition:**

Backtracking is the standard recursive shape for 'try a choice, explore, undo'. It keeps the parent state intact for the next choice.

**How it works:**

- The recursive function's contract is: the board is valid on entry, and it is unchanged on exit.
- Forgetting the undo is the bug that makes later siblings see a choice they did not make.
- The same shape generates combinations, permutations, and partitions. Only the candidate rule and the constraint change.
- It is also the mental model for parsers and solvers that commit to a token or a move and retreat when the rest fails.
</details>

<details>
<summary>Explain the impact of variable ordering on the performance of backtracking algorithms.</summary>

### Explain the impact of variable ordering on the performance of backtracking algorithms.

**Definition:**

The order of variables changes the shape of the tree. Failing on a tight variable first cuts more branches than failing on it after a long prefix.

**How it works:**

- Minimum remaining values picks the variable with the smallest domain next. A domain of size 1 is a forced move and should happen immediately.
- A static order fixed before search is simpler and can still be terrible if the first variables are the loose ones.
- Degree heuristic breaks ties by choosing the variable that participates in the most constraints.
- Ordering does not change whether a solution exists. It changes how many nodes you touch before you know.

**Examples:**

In Sudoku, filling a cell that has two legal digits before a cell that has seven fails or finishes that branch sooner than the other way around.
</details>

### Complexity Analysis

<details>
<summary>Explain the time and space complexity of a typical backtracking algorithm.</summary>

### Explain the time and space complexity of a typical backtracking algorithm.

**Definition:**

Time follows the number of nodes the search actually visits. Space follows the depth of one path, plus the output if you store every solution.

**How it works:**

- A complete tree with branching factor `b` and depth `d` is O(bᵈ) nodes. Pruning makes the typical case smaller and leaves the worst case unchanged.
- Work per node is the cost of generating a candidate and testing the constraint. A naive scan can turn O(bᵈ) nodes into something worse.
- Recursion depth is O(d). An explicit board of size d is the other O(d) memory.
- Output-sensitive cost matters: listing all permutations is Θ(n · n!) because there are n! leaves and each takes O(n) to copy.
</details>

<details>
<summary>How do worst-case scenarios in backtracking compare to other algorithms?</summary>

### How do worst-case scenarios in backtracking compare to other algorithms?

**Definition:**

In the worst case backtracking still walks a full exponential tree, the same order as brute force, when every partial candidate looks legal until the end.

**How it works:**

- A problem with a tight constraint, such as N-queens, prunes heavily on typical boards and still has an exponential worst case.
- Dynamic programming wins when the state space is polynomial and repeats. Backtracking without memoization recomputes those overlaps.
- A greedy algorithm or a closed form is better when the problem has the structure they require. Backtracking is what you use when they do not.
- Branch and bound improves the worst practical search for optimization by using a bound, and it still cannot promise polynomial time.
</details>

### Classic Backtracking Problems and Challenges

<details>
<summary>Provide a backtracking solution for the N-Queens problem.</summary>

### Provide a backtracking solution for the N-Queens problem.

**Definition:**

Place n queens on an n×n board so that no two share a row, column, or diagonal. GeeksforGeeks times plain backtracking at O(n!) time and O(n²) auxiliary space for the board and the recursion, and the hashed form at O(n!) time and O(n) space. For n = 4 the column lists are `[2, 4, 1, 3]` and `[3, 1, 4, 2]`. For n = 3 there is no solution.

**How it works:**

- One queen per row. The column chosen in row `r` must be free, and so must both diagonals `r - c` and `r + c`.
- Place the queen, recurse to the next row, then remove it. That undo is the backtrack.
- The factorial bound is the permutation of columns before diagonal pruning. Hash sets make the safety test O(1); a bit mask is the same idea.
- The recursion depth is n, one frame per row.

**Examples:**

```text
def solve_nqueens(n):
    cols, d1, d2 = set(), set(), set()
    board, res = [], []
    def dfs(r):
        if r == n:
            res.append(board[:])
            return
        for c in range(n):
            if c in cols or (r - c) in d1 or (r + c) in d2:
                continue
            cols.add(c); d1.add(r - c); d2.add(r + c); board.append(c + 1)
            dfs(r + 1)
            cols.remove(c); d1.remove(r - c); d2.remove(r + c); board.pop()
    dfs(0)
    return res
```

`solve_nqueens(4)` is `[[2, 4, 1, 3], [3, 1, 4, 2]]`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Solve a Sudoku puzzle with backtracking.</summary>

### Solve a Sudoku puzzle with backtracking.

**Definition:**

Fill a 9×9 grid so each row, column, and 3×3 box contains the digits 1 through 9 once. Backtracking tries a digit in the next empty cell and undoes it when the partial grid cannot be finished. The same place-and-undo shape is what GeeksforGeeks uses for N-queens, timed there at O(n!) before pruning; here each empty cell has at most 9 candidates.

**How it works:**

- Scan for the next empty cell. If none remains, the grid is solved.
- A digit is legal when it is absent from that row, column, and box.
- Write the digit, recurse, and on failure write the empty mark back.
- The box index is `(row // 3) * 3 + (col // 3)`. Pruning happens at the first conflicting digit, so the 9^k tree of k empty cells is not fully walked.

**Examples:**

```text
def solve_sudoku(board):
    def ok(r, c, d):
        if d in board[r]:
            return False
        if any(board[i][c] == d for i in range(9)):
            return False
        br, bc = 3 * (r // 3), 3 * (c // 3)
        return all(board[i][j] != d for i in range(br, br + 3) for j in range(bc, bc + 3))
    def dfs():
        for r in range(9):
            for c in range(9):
                if board[r][c] != ".":
                    continue
                for d in "123456789":
                    if not ok(r, c, d):
                        continue
                    board[r][c] = d
                    if dfs():
                        return True
                    board[r][c] = "."
                return False
        return True
    return dfs()
```

An empty cell that already sees 1–8 in its row accepts only 9, then the search continues.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Solve the rat in a maze problem with backtracking.</summary>

### Solve the rat in a maze problem with backtracking.

**Definition:**

A rat starts at the top-left of a 0/1 maze and may move to an open neighbor. The search marks the current cell, recurses, and unmarks it so another route can use it. Dead ends undo. The branching is the same place-and-undo pattern GeeksforGeeks uses for N-queens.

**How it works:**

- Only cells with value 1 are open. Moves are typically right and down, or all four directions when the statement allows them.
- A path that reaches the bottom-right is an answer. Record the cells, then undo so later paths are still found.
- Without the unmark, the search would treat a cell on the current path as a wall forever.
- In the worst case every open cell is tried from every prefix, so the tree grows with the number of choices per step. A maze of only right and down moves on an open n×n grid has a binomial number of paths, and the search lists them.

**Examples:**

```text
def maze_paths(maze):
    n = len(maze)
    path, res = [], []
    def dfs(r, c):
        if not (0 <= r < n and 0 <= c < n) or maze[r][c] != 1:
            return
        path.append((r, c))
        maze[r][c] = -1
        if (r, c) == (n - 1, n - 1):
            res.append(path[:])
        else:
            dfs(r, c + 1)
            dfs(r + 1, c)
        maze[r][c] = 1
        path.pop()
    if maze and maze[0][0] == 1:
        dfs(0, 0)
    return res
```

On `[[1, 0], [1, 1]]` the only path is `(0, 0) → (1, 0) → (1, 1)`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Write a backtracking algorithm to generate all permutations of a string.</summary>

### Write a backtracking algorithm to generate all permutations of a string.

**Definition:**

A permutation is an ordering of the characters. Backtracking swaps the next position with each later candidate and swaps back. Listing them writes n! strings, and copying a string of length n makes the output cost Θ(n · n!), the same factorial family GeeksforGeeks states as O(n!) for the N-queens search tree.

**How it works:**

- At index `i`, every index `j >= i` is a choice for position `i`.
- Swap, recurse to `i + 1`, swap back.
- Skip a duplicate character at the same depth if the input repeats, so identical permutations are not emitted twice.
- The recursion depth is n. The leaves are the permutations.

**Examples:**

```text
def permutations(s):
    chars, out = list(s), []
    def dfs(i):
        if i == len(chars):
            out.append("".join(chars))
            return
        seen = set()
        for j in range(i, len(chars)):
            if chars[j] in seen:
                continue
            seen.add(chars[j])
            chars[i], chars[j] = chars[j], chars[i]
            dfs(i + 1)
            chars[i], chars[j] = chars[j], chars[i]
    dfs(0)
    return out
```

`permutations("ab")` is `["ab", "ba"]`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Implement a backtracking solution for the subset sum problem.</summary>

### Implement a backtracking solution for the subset sum problem.

**Definition:**

Decide for each element whether it joins the subset, then undo that choice. GeeksforGeeks times this include-or-skip recursion at O(2^n) time and O(n) space. The DP table on the same page is O(sum · n); backtracking is the tree the table compresses.

**How it works:**

- The remaining target and the next index are the state.
- A remaining target of 0 is a hit. A negative target or a finished array is a miss.
- Explore include, then exclude. Appending and popping the chosen value builds one witness subset.
- Sorting and stopping when the next value exceeds the remainder prunes, and does not change the O(2^n) worst case of the page’s recursion.

**Examples:**

```text
def subset_sums(arr, target):
    arr = sorted(arr)
    res, cur = [], []
    def dfs(i, left):
        if left == 0:
            res.append(cur[:])
            return
        if i == len(arr) or left < 0:
            return
        cur.append(arr[i])
        dfs(i + 1, left - arr[i])
        cur.pop()
        dfs(i + 1, left)
    dfs(0, target)
    return res
```

`subset_sums([3, 34, 4, 12, 5, 2], 9)` includes `[4, 5]`.

**Sources:**

- [GeeksforGeeks — Subset sum](https://www.geeksforgeeks.org/dsa/subset-sum-problem-dp-25/)
</details>

<details>
<summary>Develop a backtracking solution to the Crossword Puzzle problem.</summary>

### Develop a backtracking solution to the Crossword Puzzle problem.

**Definition:**

A crossword places each dictionary word into a slot of the right length, then undoes the placement when a later word no longer fits. It is the N-queens pattern of choose, recurse, and undo, whose unconstrained search GeeksforGeeks bounds by O(n!).

**How it works:**

- Slots are runs of empty cells across or down. A word fits a slot only when the length matches and every filled cell agrees.
- Write the word, mark the slot used, recurse to the next slot, then erase letters that this word wrote.
- Shared cells are the constraints. A letter written by an across word prunes the down word that crosses it.
- The branching factor is the number of unused words of that length.

**Examples:**

```text
def fits(grid, slot, word):
    (r, c, dr, dc) = slot
    for k, ch in enumerate(word):
        cell = grid[r + dr * k][c + dc * k]
        if cell not in ("-", ch):
            return False
    return True
```

A slot of length 3 whose middle cell is already `A` accepts `CAT` and rejects `DOG`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Create a backtracking solution for the Word Search puzzle.</summary>

### Create a backtracking solution for the Word Search puzzle.

**Definition:**

Find whether a word sits in a grid by a path of adjacent cells, without reusing a cell. Mark the cell, try four neighbors for the next letter, then unmark. That undo is the same backtracking step as N-queens.

**How it works:**

- Every cell that matches the first letter is a start.
- From a matched prefix of length `k`, a neighbor must equal the next character and must not be on the current path.
- The mark prevents reuse. Removing it lets another path use the cell later.
- Each step has at most 4 neighbors and the path is at most the word length, so one start explores a tree of branching 4 and depth equal to the word.

**Examples:**

```text
def exist(board, word):
    m, n = len(board), len(board[0])
    def dfs(r, c, k):
        if k == len(word):
            return True
        if not (0 <= r < m and 0 <= c < n) or board[r][c] != word[k]:
            return False
        saved, board[r][c] = board[r][c], "#"
        ok = any(dfs(r + dr, c + dc, k + 1) for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)))
        board[r][c] = saved
        return ok
    return any(dfs(r, c, 0) for r in range(m) for c in range(n))
```

`exist([["A","B"],["C","D"]], "ABD")` is true along `A → B → D`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

### Advanced Problem Solving with Backtracking

<details>
<summary>Solve the graph coloring problem backtracking.</summary>

### Solve the graph coloring problem backtracking.

**Definition:**

Assign each vertex a color from 1..m so that no edge joins two vertices of the same color. Try a color, recurse to the next vertex, and undo. An unconstrained assignment of m colors to n vertices is an m^n tree; N-queens is the special case GeeksforGeeks bounds by O(n!) when the choices are a permutation.

**How it works:**

- Vertices are colored in order. A color is legal when no already-colored neighbor holds it.
- If no color fits, the search returns and the previous vertex tries its next color.
- m = 1 succeeds only on a graph with no edges. A bipartite graph is 2-colorable; the search discovers that by succeeding or exhausting the tree.
- The recursion depth is the number of vertices.

**Examples:**

```text
def colorable(n, edges, m):
    adj = [[] for _ in range(n)]
    for u, v in edges:
        adj[u].append(v); adj[v].append(u)
    color = [0] * n
    def dfs(v):
        if v == n:
            return True
        for c in range(1, m + 1):
            if any(color[u] == c for u in adj[v]):
                continue
            color[v] = c
            if dfs(v + 1):
                return True
            color[v] = 0
        return False
    return dfs(0)
```

A triangle is not 2-colorable and is 3-colorable.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Implement a backtracking solution for the Knight's Tour problem.</summary>

### Implement a backtracking solution for the Knight's Tour problem.

**Definition:**

A knight’s tour visits every square of a chessboard exactly once by knight moves. Place the knight, recurse, and remove it when a branch dies. The board mark is the same undo GeeksforGeeks uses when a queen placement fails.

**How it works:**

- From a square, a knight has at most eight L-shaped moves. Off-board and already visited squares are illegal.
- A path of n² visits is a tour. The search records the move index on the board and clears it on the way back.
- The raw tree is huge. Warnsdorff’s heuristic, choosing the square with the fewest onward moves, is an ordering of the same search, not a different problem.
- Depth is the number of squares.

**Examples:**

```text
def knight_tour(n=5):
    board = [[-1] * n for _ in range(n)]
    moves = [(1, 2), (1, -2), (-1, 2), (-1, -2), (2, 1), (2, -1), (-2, 1), (-2, -1)]
    def dfs(r, c, step):
        board[r][c] = step
        if step == n * n - 1:
            return True
        for dr, dc in moves:
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n and board[nr][nc] == -1 and dfs(nr, nc, step + 1):
                return True
        board[r][c] = -1
        return False
    return board if dfs(0, 0, 0) else None
```

On a 5×5 board a tour from the corner exists; the first cell stores 0 and the last stores 24.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Use backtracking approach to solve cryptarithmetic puzzles.</summary>

### Use backtracking approach to solve cryptarithmetic puzzles.

**Definition:**

A cryptarithm such as `SEND + MORE = MONEY` assigns a distinct digit to each letter so the equation holds, and leading digits are not zero. Try a digit, recurse to the next letter, and undo. Distinct digits are a permutation of a subset of 0–9, the same factorial search family as N-queens.

**How it works:**

- Map each new letter to an unused digit. The first letter of a word cannot be 0.
- Once every letter is assigned, evaluate the sum. A match is a solution.
- Column-by-column checking with a carry prunes before the last letter: a column that already disagrees cannot be saved by later letters.
- Ten digits and k distinct letters give P(10, k) leaves before that pruning.

**Examples:**

```text
def solve_send_more():
    letters = "SENDMORY"
    used = [False] * 10
    assign = {}
    def value(word):
        n = 0
        for ch in word:
            n = n * 10 + assign[ch]
        return n
    def dfs(i):
        if i == len(letters):
            return value("SEND") + value("MORE") == value("MONEY")
        for d in range(10):
            if used[d] or (d == 0 and letters[i] in "SM"):
                continue
            used[d] = True
            assign[letters[i]] = d
            if dfs(i + 1):
                return True
            used[d] = False
        return False
    dfs(0)
    return assign
```

One well-known solution is `9567 + 1085 = 10652`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Solve the Hamiltonian path problem using backtracking.</summary>

### Solve the Hamiltonian path problem using backtracking.

**Definition:**

A Hamiltonian path visits each vertex once. Build it by appending an unused neighbor and popping that vertex when the branch fails. The search is a permutation of the vertices filtered by edges, so it sits in the O(n!) family GeeksforGeeks states for N-queens.

**How it works:**

- Start at every vertex if the path may begin anywhere, or at a given source.
- The path is Hamiltonian when it contains every vertex.
- Only graph neighbors are candidates, which prunes non-edges before the leaf.
- A cycle is the same search with an extra edge from the last vertex back to the first.

**Examples:**

```text
def ham_path(n, edges):
    adj = [[] for _ in range(n)]
    for u, v in edges:
        adj[u].append(v); adj[v].append(u)
    path = []
    def dfs(u, seen):
        path.append(u)
        if len(path) == n:
            return True
        for v in adj[u]:
            if v in seen:
                continue
            seen.add(v)
            if dfs(v, seen):
                return True
            seen.remove(v)
        path.pop()
        return False
    for s in range(n):
        if dfs(s, {s}):
            return path
    return None
```

The path graph `0—1—2` yields a Hamiltonian path such as `[0, 1, 2]`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Solve Word Break problem using backtracking.</summary>

### Solve Word Break problem using backtracking.

**Definition:**

Split the string at every dictionary word that matches a prefix, then undo that choice and try the next word. GeeksforGeeks times this recursion, with no memoization, at O(2^n). The DP on the same page reuses the overlapping prefixes and is O(n · m · L).

**How it works:**

- From index `i`, try every dictionary word equal to `s[i:i+len]`.
- A match appends the word and recurses at the new index. Failure pops the word.
- Reaching `i == len(s)` records one segmentation.
- `"ilike"` with `{i, like, gfg}` yields `["i", "like"]`. The exponential bound is the page’s naive approach, before the boolean DP.

**Examples:**

```text
def word_break_bt(s, words):
    words, res, cur = set(words), [], []
    def dfs(i):
        if i == len(s):
            res.append(cur[:])
            return
        for w in words:
            if s.startswith(w, i):
                cur.append(w)
                dfs(i + len(w))
                cur.pop()
    dfs(0)
    return res
```

`word_break_bt("ilike", ["i", "like", "gfg"])` is `[["i", "like"]]`.

**Sources:**

- [GeeksforGeeks — Word Break](https://www.geeksforgeeks.org/dsa/word-break-problem-dp-32/)
</details>

<details>
<summary>Write a backtracking algorithm for palindrome partitioning of a string.</summary>

### Write a backtracking algorithm for palindrome partitioning of a string.

**Definition:**

Cut the string into contiguous pieces that are all palindromes. Try every end of the next piece, keep it only when the slice reads the same both ways, and pop it on the way back. The include-every-cut tree is the O(2^n) recursion GeeksforGeeks states for the unmemoized word-break search; here the dictionary test is the palindrome check.

**How it works:**

- From index `i`, every `j > i` is a candidate cut.
- `s[i:j]` is kept only when it equals its reverse.
- A partition is complete at `i == len(s)`.
- `"aab"` yields `["a", "a", "b"]` and `["aa", "b"]`.

**Examples:**

```text
def pal_parts(s):
    res, cur = [], []
    def dfs(i):
        if i == len(s):
            res.append(cur[:])
            return
        for j in range(i + 1, len(s) + 1):
            piece = s[i:j]
            if piece == piece[::-1]:
                cur.append(piece)
                dfs(j)
                cur.pop()
    dfs(0)
    return res
```

`pal_parts("aab")` is `[["a", "a", "b"], ["aa", "b"]]`.

**Sources:**

- [GeeksforGeeks — Word Break](https://www.geeksforgeeks.org/dsa/word-break-problem-dp-32/)
</details>

<details>
<summary>Solve the combination sum problem using a backtracking approach.</summary>

### Solve the combination sum problem using a backtracking approach.

**Definition:**

Pick numbers that add up to a target. If reuse is allowed, the same index may be chosen again; if not, the search moves to the next index. Either way the choice is popped. GeeksforGeeks times the include-or-skip subset recursion at O(2^n); reuse makes a different tree, still pruned when the partial sum exceeds the target.

**How it works:**

- Sort the candidates so a value larger than the remainder can stop the loop.
- Append a candidate, recurse with the reduced remainder, pop it.
- Starting the next pick at `i` allows reuse. Starting at `i + 1` uses each element once, which is subset sum.
- A remainder of 0 records the current combination.

**Examples:**

```text
def combination_sum(candidates, target):
    candidates = sorted(candidates)
    res, cur = [], []
    def dfs(start, left):
        if left == 0:
            res.append(cur[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > left:
                break
            cur.append(candidates[i])
            dfs(i, left - candidates[i])
            cur.pop()
    dfs(0, target)
    return res
```

`combination_sum([2, 3, 6, 7], 7)` includes `[7]` and `[2, 2, 3]`.

**Sources:**

- [GeeksforGeeks — Subset sum](https://www.geeksforgeeks.org/dsa/subset-sum-problem-dp-25/)
</details>

<details>
<summary>Implement a backtracking algorithm to find all valid IP addresses from a string of numbers.</summary>

### Implement a backtracking algorithm to find all valid IP addresses from a string of numbers.

**Definition:**

An IPv4 address is four integers from 0 to 255, written in decimal without a leading zero. Backtracking chooses the next segment length, 1 to 3 digits, and undoes it. There are only four segments, so the tree is tiny compared with the O(n!) N-queens search: at most 3^4 leaves before the numeric checks.

**How it works:**

- A segment is illegal when it has a leading zero and length greater than 1, or when its value exceeds 255.
- After four segments the input must be consumed exactly.
- `"25525511135"` yields `255.255.111.35` and `255.255.11.135`.
- Popping the segment is what lets the next length be tried.

**Examples:**

```text
def restore_ip(s):
    res, cur = [], []
    def dfs(i):
        if len(cur) == 4:
            if i == len(s):
                res.append(".".join(cur))
            return
        for length in (1, 2, 3):
            if i + length > len(s):
                break
            piece = s[i:i + length]
            if (piece[0] == "0" and length > 1) or int(piece) > 255:
                continue
            cur.append(piece)
            dfs(i + length)
            cur.pop()
    dfs(0)
    return res
```

`restore_ip("25525511135")` is `["255.255.11.135", "255.255.111.35"]`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Develop a backtracking solution to the "All Paths From Source to Target" problem in a directed graph.</summary>

### Develop a backtracking solution to the "All Paths From Source to Target" problem in a directed graph.

**Definition:**

List every directed path from the source to the target. Append the current vertex, recurse along outgoing edges, and pop the vertex so the next edge is tried from the same prefix. On a DAG this cannot loop. The shape is the maze search: each choice is undone.

**How it works:**

- The path starts as `[source]`.
- When the last vertex is the target, copy the path into the answers.
- Otherwise try each neighbor. Popping guarantees the neighbor is not left on a path that did not choose it.
- A DAG’s paths are finite. A directed cycle needs a seen-set, or the recursion does not terminate.

**Examples:**

```text
def all_paths(graph, target):
    res = []
    def dfs(u, path):
        if u == target:
            res.append(path[:])
            return
        for v in graph[u]:
            path.append(v)
            dfs(v, path)
            path.pop()
    dfs(0, [0])
    return res
```

`all_paths([[1, 2], [3], [3], []], 3)` is `[[0, 1, 3], [0, 2, 3]]`.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

### Real-World Applications of Backtracking

<details>
<summary>How is backtracking used in artificial intelligence for games?</summary>

### How is backtracking used in artificial intelligence for games?

**Definition:**

A game tree is explored by making a move, searching the reply, and undoing the move. That is backtracking. N-queens is the same control structure on a board; a two-player game adds an opponent who chooses the move that is worst for you.

**How it works:**

- The recursion depth is the ply. A leaf is a terminal position or a depth cutoff with a static score.
- Minimax takes the max at your turn and the min at the opponent’s turn. Alpha-beta prunes a branch whose score cannot beat the best already found, the same idea as rejecting an attacked queen square before going deeper.
- Undo restores the board so the next sibling move is tried from the parent position.
- The full tree of a game such as chess is not enumerated. The bound is the branching factor raised to the searched depth, cut down by pruning.

**Examples:**

```text
def negamax(board, depth, generate, apply, undo, evaluate):
    if depth == 0:
        return evaluate(board)
    best = -10**9
    for move in generate(board):
        apply(board, move)
        best = max(best, -negamax(board, depth - 1, generate, apply, undo, evaluate))
        undo(board, move)
    return best
```

Each `apply` / `undo` pair is one edge of the game tree.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Explain the role of backtracking in network routing optimization.</summary>

### Explain the role of backtracking in network routing optimization.

**Definition:**

A route is built hop by hop. If the partial path exceeds a cost bound, repeats a node, or violates a capacity, the last hop is removed and another neighbor is tried. That is the maze backtracking already used for grid paths, with an extra numeric bound.

**How it works:**

- The state is the current node, the path, and the cost so far.
- A neighbor is skipped when it is already on the path or when `cost + edge` exceeds the best complete route found.
- Reaching the destination records the path and tightens the bound, so later branches die earlier.
- Without the bound the search lists simple paths, which on a dense graph sits in the factorial family of N-queens. The bound is a prune, not a promise of polynomial time.

**Examples:**

```text
def cheapest(n, edges, src, dst):
    adj = [[] for _ in range(n)]
    for u, v, w in edges:
        adj[u].append((v, w))
    best = [10**18]
    def dfs(u, cost, seen):
        if cost >= best[0]:
            return
        if u == dst:
            best[0] = cost
            return
        for v, w in adj[u]:
            if v in seen:
                continue
            seen.add(v)
            dfs(v, cost + w, seen)
            seen.remove(v)
    dfs(src, 0, {src})
    return best[0]
```

The first feasible path sets a ceiling; a partial path above that ceiling is undone immediately.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>How can backtracking be used in optimizing database query solutions?</summary>

### How can backtracking be used in optimizing database query solutions?

**Definition:**

A join order is a permutation of the tables. Backtracking builds that order, estimates the cost of the partial join, and abandons an order whose partial cost already loses to the best complete plan. The search tree is the permutation tree GeeksforGeeks bounds by O(n!) for N-queens, pruned by the cost bound.

**How it works:**

- At step `k`, append a table that is not yet joined and whose predicates connect to the current set when a bushy or left-deep rule requires it.
- Add the estimated cardinality or I/O of that join. If it exceeds the best finished plan, pop the table.
- A finished order of all tables updates the best plan.
- Dynamic programming over subsets of tables replaces this tree when the number of tables is small enough to store `2^n` states. Backtracking is what remains when that table does not fit.

**Examples:**

```text
def best_join(tables, cost):
    best = [10**18]
    def dfs(order, so_far):
        if so_far >= best[0]:
            return
        if len(order) == len(tables):
            best[0] = so_far
            return
        for t in tables:
            if t in order:
                continue
            order.append(t)
            dfs(order, so_far + cost(order))
            order.pop()
    dfs([], 0)
    return best[0]
```

`cost` is the optimizer’s estimate of the join built so far. A losing prefix is popped.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>

<details>
<summary>Discuss the use of backtracking in scheduling and planning problems.</summary>

### Discuss the use of backtracking in scheduling and planning problems.

**Definition:**

Scheduling assigns each task a resource and a time and undoes the assignment when it collides with a task already placed. Graph coloring is the special case of assigning time slots so that conflicting tasks differ, and N-queens is the board-shaped version of the same choose-and-undo loop.

**How it works:**

- Order the tasks. For the next task, try each feasible slot that does not overlap a conflicting task on the same resource.
- Place it, recurse, and clear the slot on failure.
- A bound on the makespan prunes a partial schedule that already runs longer than the best finished one.
- The unconstrained assignment is exponential in the number of tasks, the same reason the N-queens page gives a factorial worst case before diagonal pruning.

**Examples:**

```text
def schedule(tasks, slots, conflicts):
    assign = {}
    def ok(task, slot):
        return all(assign.get(other) != slot for other in conflicts(task))
    def dfs(i):
        if i == len(tasks):
            return True
        for slot in slots:
            if not ok(tasks[i], slot):
                continue
            assign[tasks[i]] = slot
            if dfs(i + 1):
                return True
            del assign[tasks[i]]
        return False
    return assign if dfs(0) else None
```

Two tasks that conflict cannot receive the same slot; the failed slot is deleted before the next one is tried.

**Sources:**

- [GeeksforGeeks — N Queen Problem](https://www.geeksforgeeks.org/dsa/n-queen-problem-backtracking-3/)
</details>
