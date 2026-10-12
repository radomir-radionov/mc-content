# Greedy Algorithms — Interview

Answers are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Greedy Algorithms — Interview](./7_algo_greedy_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Greedy Algorithms](https://devinterview.io/questions/data-structures-and-algorithms/greedy-algorithms-interview-questions/).

### Understanding Greedy Algorithms

<details>
<summary>What is a Greedy Algorithm?</summary>

### What is a Greedy Algorithm?

**Definition:**

A greedy algorithm builds an answer by taking, at each step, the option that looks best right now, and it does not go back to revise that choice.

**How it works:**

- The greedy-choice property is the hope that a locally best step can be extended to a globally best solution.
- Choices are irreversible. There is no backtracking over the options already taken.
- When the property holds, the algorithm is usually a sort plus a linear scan, which is why it is fast compared with a search of every subset.
- Fractional knapsack is the textbook win: take items in order of value per weight, and split only the last one that does not fit.
- Dijkstra’s algorithm is the graph example: always settle the unsettled vertex with the smallest tentative distance, and never reopen it when every weight is nonnegative.
- If a later step can regret an early choice, greedy is the wrong design. The 0/1 knapsack is the usual counterexample.

**Examples:**

Coins `1, 5, 10` to make 28: take two 10s, one 5, and three 1s. Taking the largest coin that still fits is optimal for this set.
</details>

<details>
<summary>What are Greedy Algorithms used for?</summary>

### What are Greedy Algorithms used for?

**Definition:**

Greedy methods are used on optimization problems where a sorted order makes the next safe choice obvious: shortest paths, spanning trees, codes, and schedules.

**How it works:**

- Dijkstra grows a shortest-path tree on nonnegative weights by settling the closest unsettled vertex.
- Kruskal sorts edges by weight and adds an edge when it connects two components. Prim grows one component by the cheapest edge that leaves it. Both build a minimum spanning tree.
- Huffman coding repeatedly merges the two least frequent symbols so frequent symbols end up with short codes.
- Interval scheduling sorts by finish time and takes the next interval that starts after the last chosen one, maximizing how many fit on one resource.
- The greedy set-cover heuristic repeatedly takes the set that covers the most still-uncovered elements. That is an approximation, not an exact optimum.
- Fractional knapsack, by value density, is exact. The 0/1 version is not a greedy problem.
- The same pattern shows up in filling a line of text and in cache eviction rules that always drop a locally “least useful” entry.

**Examples:**

Activities ending at times 3, 5, and 9, starting at 1, 4, and 6. Sort by finish and take the first and the third. The second overlaps the first.
</details>

<details>
<summary>List some characteristics that are often present in problems that are solvable by greedy algorithms.</summary>

### List some characteristics that are often present in problems that are solvable by greedy algorithms.

**Definition:**

A problem is a greedy candidate when a locally best step is safe to keep, and the rest of the problem is the same kind of problem without that step.

**How it works:**

- Optimal substructure: an optimal solution contains optimal solutions of the pieces left after the choice.
- Greedy-choice property: some locally best option is part of some optimal solution, so you do not have to try the other options.
- The choice is not undone. That is safe only because of the property above, not because irreversibility is a virtue by itself.
- Each input item is considered a constant number of times, often once after a sort, so the memory beyond the input stays small.
- The problem splits into a choice plus one smaller independent remainder, not into a table of overlapping subproblems.
- Classic fits: Dijkstra on nonnegative edges, Huffman coding, activity selection, and coin change for canonical denominations.
- A problem can have optimal substructure and still not be greedy. Edit distance has the substructure and needs the table, because no single local edit is always safe.

**Examples:**

Activity selection: the interval that finishes first is always safe to keep. After it is taken, the problem is the same problem on the intervals that start later.
</details>

<details>
<summary>What is the difference between Greedy and Heuristic algorithms?</summary>

### What is the difference between Greedy and Heuristic algorithms?

**Definition:**

A heuristic is any practical rule that aims at a good answer without a proof that the answer is best. A greedy algorithm is a specific rule: take the locally best feasible step and do not revisit it. Some greedy algorithms are exact. A heuristic does not promise that.

**How it works:**

- Greedy decides from the current state alone, usually after the input has been sorted by a fixed score.
- A heuristic can be a greedy score, a random sample, a cutoff in search, or a rule of thumb learned from data. The word is wider.
- Greedy does not backtrack. Some heuristics do, or they restart.
- When the greedy-choice property is proved, the result is optimal, not merely plausible. Fractional knapsack is that case.
- When it is not proved, the same style of step is only a heuristic. Nearest-neighbor for the traveling salesman is greedy and not optimal.
- Use “heuristic” for “we accept a good answer”. Use “greedy” for the one-step local rule, and say separately whether that rule is exact.

**Examples:**

Huffman coding is greedy and optimal for symbol-by-symbol prefix codes. A chess program that always captures the most valuable piece is a greedy heuristic and loses games a search would win.
</details>

<details>
<summary>Compare Greedy vs Divide & Conquer vs Dynamic Programming algorithms.</summary>

### Compare Greedy vs Divide & Conquer vs Dynamic Programming algorithms.

**Definition:**

Greedy commits to one option per step. Divide and conquer solves independent pieces and combines them. Dynamic programming solves overlapping pieces and stores the answers.

**How it works:**

- Optimality: a proved greedy choice is optimal and cheap. Divide and conquer is optimal when the combine step preserves the answers of the pieces, as in merge sort. DP is optimal when the recurrence enumerates the safe choices and keeps the best.
- Time: greedy is often a sort, Θ(n log n), plus a scan. Divide and conquer follows a recurrence such as `T(n) = 2T(n/2) + Θ(n)`. DP is states times work per state, and it can be heavier because it keeps the losers as well as the winner.
- Memory: greedy usually stores the answer being built. Divide and conquer stores the recursion stack and sometimes a buffer. DP stores a table.
- Greedy examples: Dijkstra, Huffman, interval scheduling.
- Divide and conquer examples: merge sort, binary search, and Karatsuba or Strassen when the pieces are independent.
- DP examples: edit distance, 0/1 knapsack, Floyd-Warshall, and minimax over an explicit game tree when states repeat.
- A problem with overlapping subproblems is a poor fit for plain divide and conquer, because the pieces are recomputed. A problem with no safe local choice is a poor fit for greedy.

**Examples:**

Coin change with denominations `1, 5, 10` is greedy. The same problem with denominations `1, 3, 4` needs DP, because taking the largest coin is not always best.
</details>

<details>
<summary>Is Dijkstra’s algorithm a Greedy or Dynamic Programming algorithm?</summary>

### Is Dijkstra’s algorithm a Greedy or Dynamic Programming algorithm?

**Definition:**

Dijkstra’s algorithm is greedy. It always settles the unsettled vertex with the smallest tentative distance, and with nonnegative weights that choice is never revised.

**How it works:**

- The local rule is the priority queue: extract-min, then relax the outgoing edges.
- Optimal substructure is why the choice is safe. A shortest path to `u` plus a nonnegative edge is a shortest path to the neighbor once `u` is the closest unsettled vertex. Prefixes of a shortest path are themselves shortest.
- That substructure is something Dijkstra shares with dynamic programming. The algorithm does not fill a table of every subproblem the way Floyd-Warshall does, and it does not try alternative previous vertices after a vertex is settled.
- Bellman-Ford is the relaxation algorithm that does allow later improvements, which is why it can handle negative edges. Calling Dijkstra “dynamic programming” because of the substructure blurs that difference.
- The usual bound with a binary heap is Θ((V + E) log V).

**Examples:**

From `S`, edges `S→A` weight 1 and `S→B` weight 4 and `A→B` weight 1. Dijkstra settles `A` first, then relaxes `A→B` to 2, and never reopens `A`. The final distance to `B` is 2.
</details>

<details>
<summary>Is there a way to Mathematically Prove that a Greedy Algorithm will yield the Optimal Solution?</summary>

### Is there a way to Mathematically Prove that a Greedy Algorithm will yield the Optimal Solution?

**Definition:**

Yes, when you can prove a greedy-choice property and optimal substructure. For some problems the same proof is the statement that the feasible sets form a matroid.

**How it works:**

- Greedy choice: show that if an optimal solution disagrees with the first greedy pick, you can swap that pick in and stay optimal.
- Optimal substructure: after the pick, the remainder of that optimal solution is optimal for the smaller instance.
- Induction then says the whole sequence of picks is optimal.
- A matroid packages those two facts. The family of allowed sets is hereditary (a subset of an allowed set is allowed) and has the exchange property (a smaller allowed set can take some element from a larger one and stay allowed). The greedy algorithm that adds the best eligible element is then optimal. Graphic matroids are why Kruskal works.
- Fractional knapsack has a direct exchange proof and is not usually presented as a matroid. 0/1 knapsack does not have the exchange property: you cannot always swap the density leader in without breaking feasibility or optimality.

**Examples:**

If an optimal activity set skips the interval that finishes first, replace its first activity with that interval. The replacement still fits, and the count does not fall. That swap is the greedy-choice proof.
</details>

<details>
<summary>Explain the Greedy Choice Property and its significance in greedy algorithms.</summary>

### Explain the Greedy Choice Property and its significance in greedy algorithms.

**Definition:**

The greedy-choice property says that a locally best option is part of some globally optimal solution. It is what lets the algorithm commit without searching the other branches.

**How it works:**

- The step uses only information available now: the cheapest edge that does not close a cycle, the interval that ends soonest, the symbol pair with the smallest frequencies.
- It does not look at how later steps will score. The proof, done separately, shows that some optimum contains this step.
- Without the property, the same local rule can miss the optimum. That is a limitation of the problem, not a reason to add backtracking and still call it greedy.
- Problems that have the property include minimum spanning trees, Dijkstra on nonnegative weights, Huffman coding, fractional knapsack, and activity selection.
- Cache policies such as “evict the least recently used” are greedy rules aimed at a good hit rate. They do not have this property for every request sequence.

**Examples:**

Children buying candy with a shared goal of “most pieces” should not each spend their coins on the single nicest piece. Activity selection does have the property: the earliest finish leaves the most room for everyone else.
</details>

### Greedy Algorithm Design

<details>
<summary>Design a greedy algorithm for making change using the fewest coins possible.</summary>

### Design a greedy algorithm for making change using the fewest coins possible.

**Definition:**

For canonical coin systems, repeatedly take the largest denomination that does not exceed the remaining amount. US coins and powers of a base work. Arbitrary denominations do not.

**How it works:**

- Sort the denominations descending once.
- While the remainder is positive, take the largest coin that fits, subtract it, and count it.
- With a canonical system this matches the optimum. The proof is an exchange: an optimum that uses enough smaller coins to replace a larger one can swap in the larger coin and not use more coins.
- Time is Θ(number of denominations + number of coins paid) after the sort, which is fine when the smallest coin is 1. It is not Θ(amount) unless you subtract one unit at a time.
- If the denominations are `1, 3, 4` and the amount is 6, two 3s beat a 4 plus two 1s. The greedy pick of 4 is wrong. Use the DP coin-change table for a non-canonical set.
- If no coin equals 1 and some remainder cannot be made, the loop must stop and report failure instead of spinning.

**Examples:**

```text
for coin in denominations_desc:
  take = remainder // coin
  remainder -= take * coin
  count += take
```

Amount 30 with `25, 10, 1` yields one quarter and five cents, six coins, which is optimal for those denominations.
</details>

<details>
<summary>Describe a greedy strategy for scheduling events that use the fewest number of resources.</summary>

### Describe a greedy strategy for scheduling events that use the fewest number of resources.

**Definition:**

The fewest resources equal the deepest overlap: the maximum number of events that are running at the same instant. A sweep finds that depth.

**How it works:**

- Sort the events by start time.
- Walk a min-heap of end times of the events already given a resource. Before placing the next event, pop every end time that is no later than this start. Those resources are free.
- If the heap is empty, this event needs a new resource. Otherwise reuse the resource whose event ended first, and push this event’s end time.
- The maximum size the heap reaches is the number of resources. Each event is assigned the resource you just reused or created.
- Sorting is Θ(n log n), and each event is pushed and popped once, so the total is Θ(n log n).
- Sorting by finish time and taking a non-overlapping subset solves a different problem: the most events one resource can host. It does not compute how many resources a set of overlapping events needs.

**Examples:**

Meetings `[0, 30), [5, 10), [15, 20)`. The second starts during the first, so the depth is 2. The third can reuse the resource freed at time 10. Two rooms are necessary and enough.
</details>

<details>
<summary>Explain how to apply a greedy approach to minimize the total waiting time for a given set of queries.</summary>

### Explain how to apply a greedy approach to minimize the total waiting time for a given set of queries.

**Definition:**

Schedule the shortest query first. Each query’s waiting time is the sum of the durations placed ahead of it, so a long job early in line makes everyone behind it pay.

**How it works:**

- Sort the durations ascending.
- The waiting time of the job in position `i` (0-based) is the sum of the durations before it. The total waiting time is the sum of those prefix sums.
- Equivalently, a job of length `d` that has `k` jobs after it contributes `d` to each of those `k` waits, so it is counted `k` times. Putting large `d` late makes `k` small.
- Sorting is Θ(n log n). The prefix sum is Θ(n).
- This minimizes the sum of waiting times, or the sum of completion times, for a single processor with no release dates and no deadlines. Deadlines or priorities change the rule.
- Shortest-job-first is not fair. A long job can be postponed forever if short jobs keep arriving. The optimality claim is about a fixed batch.

**Examples:**

Durations `5, 1, 3`. Order `1, 3, 5`. Waits are `0`, `1`, and `1+3 = 4`, total 5. Order `5, 3, 1` waits `0 + 5 + 8 = 13`.
</details>

<details>
<summary>Develop a greedy algorithm for the activity selection problem to maximize the number of activities.</summary>

### Develop a greedy algorithm for the activity selection problem to maximize the number of activities.

**Definition:**

Sort activities by finish time and repeatedly take the next one that starts at or after the previous finish. That set is a largest set of non-overlapping activities.

**How it works:**

- An activity is a half-open interval `[start, finish)`.
- Sort by `finish` ascending. The earliest finish is always a safe first pick: any optimal set can swap its first activity for this one.
- Scan the rest. Take an activity when its start is at least the finish of the last taken activity. Skip it otherwise.
- Sorting is Θ(n log n). The scan is Θ(n). Extra memory is the output list.
- Sorting by start time, or by duration, does not have the same proof. A long activity that starts first can block several short ones.
- This maximizes the count on one resource. It does not maximize total duration, and it does not assign many resources.

**Examples:**

```text
sort by finish
last = -infinity
for activity in activities:
  if activity.start >= last:
    take(activity)
    last = activity.finish
```

Intervals finishing at 3, 5, 9 and starting at 1, 2, 6 yield the first and the third.
</details>

### Correctness and Optimization

<details>
<summary>How can you establish the correctness of a greedy algorithm?</summary>

### How can you establish the correctness of a greedy algorithm?

**Definition:**

Show that the first local choice can replace whatever an optimal solution did instead, and that the leftover problem is solved by the same argument.

**How it works:**

- Exchange: take an optimal solution that differs from the greedy pick. Swap the greedy element in. Show feasibility and that the objective does not get worse. Activity selection swaps the earliest-finishing interval into the first slot.
- For a minimum spanning tree the swapped object is a safe edge: the lightest edge across some cut is in some MST. Kruskal and Prim only add safe edges.
- After the exchange, delete that choice and repeat on the smaller instance. Induction covers the whole sequence.
- A matroid is a packaged version of the same two lemmas. If you cannot find the exchange, do not claim optimality. Measure the algorithm as a heuristic instead.
- “It worked on the sample” is not the argument. One counterexample, such as coins `1, 3, 4` for amount 6, kills a general claim.

**Examples:**

Prim grows a tree `T`. The lightest edge with one end in `T` is safe. If an MST uses another edge across that cut, swap. The weight does not rise, and `T` stays acyclic.
</details>

<details>
<summary>What is the role of sorting in many greedy algorithms, and why is it often necessary?</summary>

### What is the role of sorting in many greedy algorithms, and why is it often necessary?

**Definition:**

Sorting turns “the best remaining item” into “the next item in this order”, so the scan can commit in one pass.

**How it works:**

- Activity selection needs finish order. Kruskal needs weight order. Fractional knapsack needs value-per-weight order. Shortest-job-first needs duration order.
- Without that order, finding the best eligible item at every step can cost a linear scan each time, or it can pick an item that is not safe.
- The sort is usually done once, up front. Some algorithms sort only a frontier, which is what a heap does inside Dijkstra and Prim.
- If the input arrives already ordered, the sort is free and the scan is the whole algorithm.
- Sorting does not create the greedy-choice property. It only exposes an order in which a property you have already proved can be applied.
- The Θ(n log n) sort is often the dominant term. The later scan is linear.

**Examples:**

Fractional knapsack items `(value, weight)` `(60, 10)`, `(100, 20)`, `(120, 30)`. Densities are `6`, `5`, and `4`. Take them in that order. Capacity 50 takes all of the first two and `20/30` of the third.
</details>

<details>
<summary>Can greedy algorithms be used to solve optimization problems? Give an example.</summary>

### Can greedy algorithms be used to solve optimization problems? Give an example.

**Definition:**

Yes, when a locally best step is proved to belong to an optimal solution. Activity selection is the clean example: maximize the number of non-overlapping intervals.

**How it works:**

- Sort by finishing time. Take the first interval. Then take the next interval that starts after that finish, and repeat.
- The objective is the count, not the total length. The constraint is that chosen intervals do not overlap.
- The exchange proof says an optimal schedule can be rewritten to start with the greedy interval without lowering the count.
- Time is Θ(n log n) from the sort, then Θ(n) to scan. The output list is Θ(n) in the worst case, when nothing overlaps.
- Greedy is not a general optimizer. The same intervals, if the goal is maximum total length, can prefer one long interval over two short ones that the earliest-finish rule would take. State the objective before you claim the algorithm solves “the” optimization problem.

**Examples:**

Intervals `[1, 4), [3, 5), [0, 6), [5, 7), [8, 9)`. Earliest finish takes `[1, 4)`, then `[5, 7)`, then `[8, 9)`. Three intervals. The long `[0, 6)` is correctly skipped.
</details>

### Greedy Algorithm Challenges

<details>
<summary>Solve the fractional knapsack problem using a greedy method.</summary>

### Solve the fractional knapsack problem using a greedy method.

**Definition:**

Fractional knapsack fills a weight limit by taking items in order of value per unit weight, and it may split the one item that does not fit.

**How it works:**

- GeeksforGeeks sorts by value/weight descending, then takes a whole item while it fits and a fraction of the next. The stated cost is O(n log n) time and O(n) space.
- Taking the lightest items first, or the highest value first, fails the examples on that page. The ratio is the order that works.
- The last item contributes `(value / weight) * remaining capacity`. After that the pack is full.
- Wikipedia contrasts this with 0/1 knapsack: fractions make a polynomial greedy algorithm possible. Forbidding fractions makes the problem NP-hard, and the same order is no longer exact.

**Examples:**

```text
items.sort(key=lambda it: it.value / it.weight, reverse=True)
for item in items:
  if item.weight <= capacity:
    take all of item
  else:
    take item.value * capacity / item.weight
    break
```

Values `[60, 100, 120]`, weights `[10, 20, 30]`, capacity 50. Take both of the first two and `2/3` of the third: `60 + 100 + 80 = 240`.

**Sources:**

- [Fractional Knapsack](https://www.geeksforgeeks.org/dsa/fractional-knapsack-problem/)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Implement a greedy algorithm to arrange a group of intervals by their start times to minimize the number of rooms needed.</summary>

### Implement a greedy algorithm to arrange a group of intervals by their start times to minimize the number of rooms needed.

**Definition:**

The fewest rooms is the deepest overlap. Sorting arrivals and departures, then sweeping them in time order, counts how many intervals are open at once.

**How it works:**

- GeeksforGeeks sorts the arrival array and the departure array, then merges them like merge-sort. An arrival increases the open count. A departure decreases it. The answer is the maximum count.
- The page states this is O(n log n) time and O(1) extra space beyond the two arrays.
- If an arrival time equals a departure time, the page’s comparison `arr[i] <= dep[j]` counts the arrival first, so those two intervals do not share a room.
- The nested count of who overlaps each start is O(n²) time and O(1) space on the same page.
- Sorting only by start time, without a structure of end times, does not by itself free a room. The sweep needs the departures too.

**Examples:**

```text
sort(starts); sort(ends)
while i < n and j < n:
  if starts[i] <= ends[j]: open += 1; i += 1
  else: open -= 1; j += 1
  best = max(best, open)
```

Intervals `[0, 30), [5, 10), [15, 20)` reach depth 2.

**Sources:**

- [Minimum Platforms Required](https://www.geeksforgeeks.org/dsa/minimum-number-platforms-required-railwaybus-station/)
</details>

<details>
<summary>Design a method using greedy algorithms for minimizing the total number of platforms needed at a railway station.</summary>

### Design a method using greedy algorithms for minimizing the total number of platforms needed at a railway station.

**Definition:**

The minimum number of railway platforms is the maximum number of trains present at the same time. A sorted sweep of arrivals and departures finds that maximum.

**How it works:**

- Sort `arr` and `dep`. Walk both like a merge. `arr[i] <= dep[j]` means another train has arrived before the next departure, so the platform count rises. Otherwise a train has left and the count falls.
- GeeksforGeeks states this is O(n log n) time and O(1) extra space. The naive double loop is O(n²) time and O(1) space.
- A same-time departure and arrival do not share a platform. The `<=` test counts the arrival while the other train is still there.
- The sweep-line array on that page is a different bound: O(n + k) time and O(k) space, where k is the maximum time value. It is attractive only when k is small.

**Examples:**

```text
sort(arr); sort(dep)
# arr 900, 940, 950, 1100, 1500, 1800
# dep 910, 1200, 1120, 1130, 1900, 2000
```

The page’s walk peaks at 3 platforms.

**Sources:**

- [Minimum Platforms Required](https://www.geeksforgeeks.org/dsa/minimum-number-platforms-required-railwaybus-station/)
</details>

### Comparison and Analysis

<details>
<summary>Discuss situations where a greedy algorithm might not produce the optimal solution.</summary>

### Discuss situations where a greedy algorithm might not produce the optimal solution.

**Definition:**

A greedy step fails when the locally best choice cannot be swapped into every optimal solution. The algorithm then commits to a dead end it will not undo.

**How it works:**

- Wikipedia’s hill picture climbs the steepest slope and stops at a local peak, missing the global one. The path picture takes 12 instead of 3 and never reaches 99.
- Fractional knapsack has an exact greedy algorithm. The 0/1 version is NP-hard, and Wikipedia says using the fractional rule there only guarantees at least half the optimal value.
- Some coin systems have a greedy solution. Wikipedia says that for some instances of the coin problem the greedy choice is not optimal.
- A greedy coloring proves `χ(G) ≤ Δ(G) + 1`. That is an upper bound from the algorithm, not a promise that the number of colors is the minimum.
- If you cannot finish the exchange argument, report an approximation or switch to search or dynamic programming.

**Examples:**

Coins `1, 3, 4` for amount 6. The largest coin that fits is 4, then two 1s: three coins. Two 3s use two coins.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Compare the efficiency of a greedy algorithm to a brute force approach for the traveling salesman problem.</summary>

### Compare the efficiency of a greedy algorithm to a brute force approach for the traveling salesman problem.

**Definition:**

A greedy tour repeatedly adds the cheapest edge that can still sit inside some complete tour. Wikipedia treats that as an approximation of the NP-complete traveling salesman problem, not as a search of every tour.

**How it works:**

- Start from an empty edge set. Add the next cheapest edge that keeps the chosen edges a subgraph of a Hamiltonian cycle: degree at most 2, and no cycle until the end.
- Wikipedia states this tour is at most `Θ(log n)` times longer than an optimal tour.
- The same page says a greedy algorithm often does not produce an exact solution and can still approximate one in reasonable time. It does not give a cost for listing every tour.
- Exact dynamic programming over subsets is a different algorithm. The greedy pass never revisits an edge it skipped.
- The ratio is why the method is used as a heuristic. It is not a proof that the tour is shortest.

**Examples:**

```text
sort edges by weight
for edge in edges:
  if degrees stay <= 2 and the edge does not close a cycle early:
    take edge
```

A cheap edge that traps the path in one region can force a long jump later. The bound says that jump is within `Θ(log n)` of optimal, not that it is optimal.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>What is the Huffman Coding algorithm, and is it considered a greedy algorithm?</summary>

### What is the Huffman Coding algorithm, and is it considered a greedy algorithm?

**Definition:**

Huffman coding builds a prefix code by repeatedly merging the two least frequent symbols. Frequent symbols end up closer to the root and get shorter codes. It is a greedy algorithm.

**How it works:**

- Put each symbol in a min-heap keyed by frequency. Pop two nodes, make them children of a new node whose frequency is their sum, and push that node.
- Stop when one node remains. A left edge writes 0 and a right edge writes 1. The bits on the path from the root are the code.
- GeeksforGeeks states time O(n log n) and space O(n), where n is the number of unique characters.
- The codes are prefix-free: no code is a prefix of another, so a bit stream has one decoding.
- If the frequencies are already sorted, the two-queue version on the follow-up page is O(n) time and O(n) space. Unsorted input still pays a sort, and that page puts the total back at O(n log n).

**Examples:**

```text
while heap.size > 1:
  a, b = pop two lightest
  push node(a.freq + b.freq, left=a, right=b)
```

Frequencies `a 5, b 9, c 12, d 13, e 16, f 45` give `f: 0`, `c: 100`, `d: 101`, `a: 1100`, `b: 1101`, `e: 111`.

**Sources:**

- [Huffman Coding Algorithm](https://www.geeksforgeeks.org/dsa/huffman-coding-greedy-algo-3/)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

### Real-World Applications

<details>
<summary>How do greedy algorithms apply to network routing protocols?</summary>

### How do greedy algorithms apply to network routing protocols?

**Definition:**

Greedy routing forwards a packet to the neighbor that is closest to the destination under the chosen distance. It does not recompute a global path after that hop.

**How it works:**

- Wikipedia describes the neighbor as the one closest to the destination. Closeness may be physical, as in geographic routing on an ad hoc network, or an artificial coordinate, as in small-world routing and a distributed hash table.
- Dijkstra’s algorithm is the exact greedy shortest-path method on nonnegative weights. Wikipedia lists it with the greedy graph algorithms that are optimal.
- A local “closest neighbor” rule can stop in a dead end when every neighbor is farther than the current node. That is the local-maximum failure on the same page, applied to a network.
- The hop itself is a constant-time choice among the neighbors of one node. Wikipedia does not give a bound for a whole routing session.

**Examples:**

```text
next = neighbor minimizing distance(neighbor, destination)
forward packet to next
```

A node at a local pocket, with the destination on the other side of a void, has no closer neighbor. Geographic routing needs a recovery mode the pure greedy step does not include.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Explain how greedy algorithms can be used in load balancing and task scheduling in distributed systems.</summary>

### Explain how greedy algorithms can be used in load balancing and task scheduling in distributed systems.

**Definition:**

Greedy load balancing assigns the next job to the machine that currently has the least load. Wikipedia lists load balancing among the problems whose approximations are built this way.

**How it works:**

- Keep the load of each machine. The next job increases the smallest load. That choice is local and is not revised when later jobs arrive.
- Wikipedia groups load balancing with set cover, Steiner tree, and independent set as problems where a greedy algorithm is used as an approximation. It does not print a ratio for load balancing on that page.
- The same page does print ratios for two relatives: the greedy metric traveling-salesman tour is at most `Θ(log n)` times optimal, and the fractional-knapsack order on a 0/1 instance is at least half optimal.
- A heap of machine loads makes each assignment a heap update. The page does not state that cost. The decision rule is “smallest current load.”

**Examples:**

```text
for job in jobs:
  machine = argmin(load)
  load[machine] += job
```

Jobs `5, 5, 5` on two machines: the second 5 goes to the other machine. Putting all three on the first machine is the choice this rule refuses.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Describe an application of greedy algorithms in financial portfolio optimization.</summary>

### Describe an application of greedy algorithms in financial portfolio optimization.

**Definition:**

A greedy portfolio spends a limited budget on the positions with the best value per unit of the constraint, in the same order fractional knapsack uses.

**How it works:**

- Rank positions by the ratio the budget cares about, such as expected excess return per unit of weight. Take them in that order.
- If a position may be taken in part, this is fractional knapsack. GeeksforGeeks states that algorithm is O(n log n) time and O(n) space, and it is exact for the fractional objective.
- If a position must be all or nothing, Wikipedia says the same order is only an approximation of 0/1 knapsack and yields at least half the optimal value.
- The ratio has to match the constraint. Ranking by return alone, ignoring the weight, is the “largest value first” order that the knapsack page shows can lose.

**Examples:**

```text
sort by return / weight descending
buy whole positions while budget remains
buy a fraction of the next position with what is left
```

Three assets with ratios 6, 5, and 4 and a budget that covers the first two plus part of the third: the fraction is taken from the third, not from a lower ratio.

**Sources:**

- [Fractional Knapsack](https://www.geeksforgeeks.org/dsa/fractional-knapsack-problem/)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

### Greedy Algorithm Techniques and Methods

<details>
<summary>Discuss the primality of cuts and how it pertains to greedy algorithms.</summary>

### Discuss the primality of cuts and how it pertains to greedy algorithms.

**Definition:**

Prim grows one component of a minimum spanning tree by always adding the cheapest edge that leaves the vertices already chosen. That edge is the lightest edge across the cut between the grown set and the rest of the graph.

**How it works:**

- Start from any vertex. Repeat: among edges with one end inside the grown set and one end outside, add the lightest, and move its outside end inside.
- Wikipedia says Prim’s algorithm always finds an optimal minimum spanning tree. The tree may not be the only one.
- With an adjacency list and a heap, GeeksforGeeks states time O((V + E) log V). With an adjacency matrix the same page states O(V²).
- Auxiliary space for the priority queue and the visited set is O(V + E) on that page. The matrix form uses O(V²) for the matrix.
- The question’s “primality of cuts” is this rule. The cut changes after every added edge. Kruskal uses a different cut: any lightest edge between two components, not only the one grown from a single seed.

**Examples:**

```text
grown = {start}
while grown is not all vertices:
  add the lightest edge with one end in grown
```

**Sources:**

- [Time and Space Complexity Analysis of Prim's Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-prims-algorithm/)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Describe the ‘local ratio’ technique and its application to greedy algorithms.</summary>

### Describe the ‘local ratio’ technique and its application to greedy algorithms.

**Definition:**

The local-ratio technique proves an approximation by splitting the weight into a local piece and a remainder, and charging the algorithm’s choice against the local piece.

**How it works:**

- The ACM survey describes the method as inventing a weight function on which every reasonable solution is good. It is related to the primal-dual schema and is not based on linear programming.
- Bar-Yehuda’s covering slogan, quoted in the “one for the price of two” note, is: pay at most two dollars to reduce the optimum by at least one dollar, so the total payment is at most twice the optimum.
- The same line of work gives a 2-approximation for weighted vertex cover. An uncovered edge pays both endpoints, and an optimum must pay for at least one.
- The survey lists covering, packing, and scheduling as the families the technique fits. It does not replace an exact greedy proof. It bounds a greedy-style choice when the exact optimum is hard.

**Examples:**

```text
while the instance is not covered:
  pick an uncovered constraint
  subtract the same weight from every element that covers it
  when an element's weight hits 0, take it
```

Each payment is charged at a fixed ratio to the drop in the optimum. For vertex cover that ratio is 2.

**Sources:**

- [Local ratio: A unified framework for approximation algorithms](https://dl.acm.org/doi/10.1145/1041680.1041683)
- [Local Ratio: A Unified Framework for Approximation Algorithms](https://csaws.cs.technion.ac.il/~reuven/PDF/survay04.pdf)
</details>

<details>
<summary>What is the ‘exchange argument’ in the context of analyzing greedy algorithms?</summary>

### What is the ‘exchange argument’ in the context of analyzing greedy algorithms?

**Definition:**

An exchange argument shows that swapping the greedy choice into any competing solution does not make that solution worse. Wikipedia uses it as the standard optimality proof for a greedy algorithm.

**How it works:**

- Assume an optimal solution that is not the greedy one.
- Look at the first place they differ.
- Swap the optimal pick for the greedy pick and show the objective does not get worse and the result stays feasible.
- Induction says the whole greedy sequence is optimal, because each swap removes one difference.
- Wikipedia’s activity-selection example sorts by end time and is O(n log n). The exchange is why the earliest finish can replace whatever an optimum scheduled first.
- If the swap raises the cost or breaks a constraint, the argument stops. That is a failure, not a proof.

**Examples:**

An optimum schedules a later-finishing activity first. Replace it with the earliest-finishing feasible activity. The count does not fall, and every later activity that still fit still fits.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

### Limitations and Failures

<details>
<summary>Provide an example where a greedy algorithm fails to achieve the global optimum.</summary>

### Provide an example where a greedy algorithm fails to achieve the global optimum.

**Definition:**

A greedy failure is a locally best step that no optimal solution is forced to keep. The rest of the algorithm never repairs it.

**How it works:**

- Wikipedia’s number path takes 12 at the second step and misses the branch that contains 99.
- The steepest-ascent walk stops at the local peak m and never sees the global peak M.
- Coins are the discrete version. Wikipedia says some coin instances do not have an optimal greedy solution.
- 0/1 knapsack is the weighted version. The fractional order is exact only when splitting is allowed. Without splits, Wikipedia’s guarantee for that order is at least half the optimum, which is a miss whenever the optimum is larger than that.

**Examples:**

Denominations `1, 3, 4`, amount 6. Greedy takes `4 + 1 + 1`. The optimum is `3 + 3`.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Explain why greedy algorithms can be ineffective for the coin change problem with non-standard denominations.</summary>

### Explain why greedy algorithms can be ineffective for the coin change problem with non-standard denominations.

**Definition:**

Greedy coin change takes the largest denomination that does not exceed the remainder. That is optimal only for some denomination sets.

**How it works:**

- Sort denominations descending and take as many of the current coin as fit. US-style canonical sets and powers of a base have the property. Wikipedia says some instances of the coin problem do not.
- The counterexample is denominations `1, 3, 4` and amount 6. Greedy emits one 4 and two 1s. Two 3s use fewer coins.
- After that counterexample, the exact algorithm is the dynamic-programming table, not another greedy order.
- If no denomination is 1, a remainder can become impossible. The loop must stop and report failure instead of repeating a coin that does not fit.

**Examples:**

```text
for coin in denominations_desc:
  take = remainder // coin
  remainder -= take * coin
```

Amount 30 with `25, 10, 1` is one quarter and five cents, and that set does not hit the `1, 3, 4` failure.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Discuss the limitations of greedy algorithms when applied to graph coloring problems.</summary>

### Discuss the limitations of greedy algorithms when applied to graph coloring problems.

**Definition:**

Greedy coloring gives an uncolored vertex any color not used by an already colored neighbor. It always finishes with at most `Δ(G) + 1` colors, and that number can be larger than the chromatic number.

**How it works:**

- Wikipedia’s argument: a vertex has at most `Δ(G)` neighbors, so at most that many colors are forbidden, and one color in `1 … Δ(G) + 1` is free.
- The bound is `χ(G) ≤ Δ(G) + 1`. It is an upper bound produced by this scan. It is not a claim that the scan uses `χ(G)` colors.
- The number of colors depends on the order of vertices. A bad order can use more colors than a good order on the same graph.
- Wikipedia lists graph coloring among the places greedy algorithms supply bounds rather than exact optima. Finding `χ(G)` in general is not this scan.

**Examples:**

```text
for v in order:
  used = {color[u] for u adjacent to v if u already colored}
  color[v] = smallest positive integer not in used
```

A bipartite graph has `χ = 2`. An unlucky order can still make the scan open a third color before it notices the two sides.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

### Optimization Problems

<details>
<summary>Solve the problem of finding the largest number possible from a list of numbers using a greedy approach.</summary>

### Solve the problem of finding the largest number possible from a list of numbers using a greedy approach.

**Definition:**

The largest number you can form from a list is the concatenation of the numbers in an order decided by comparing concatenations, not by numeric size.

**How it works:**

- Turn each number into a string. For two strings `X` and `Y`, `X` comes first when `X + Y` is greater than `Y + X`.
- GeeksforGeeks states this custom sort is O(n log n) time and O(n) space.
- Sorting by numeric value fails: `[2, 3, 10]` in numeric order concatenates to `1032`, while `3210` is larger.
- Sorting the strings lexicographically also fails: `"3", "20", "2"` makes `3202`, while `"2"` before `"20"` makes `3220` because `"220" > "202"`.
- If the first string after the sort is `"0"`, every number was zero. Return `"0"` instead of a string of zeros.

**Examples:**

```text
numbers = [str(x) for x in arr]
numbers.sort(key=cmp(lambda a, b: -1 if a + b > b + a else 1))
return "0" if numbers[0] == "0" else "".join(numbers)
```

`[3, 30, 34, 5, 9]` becomes `"9534330"`.

**Sources:**

- [Create Largest Concatenated Number](https://www.geeksforgeeks.org/dsa/given-an-array-of-numbers-arrange-the-numbers-to-form-the-biggest-number/)
</details>

<details>
<summary>Implement a greedy solution for the job sequencing problem with deadlines to maximize profit.</summary>

### Implement a greedy solution for the job sequencing problem with deadlines to maximize profit.

**Definition:**

Job sequencing schedules unit-time jobs into slots before their deadlines so the total profit is as large as possible. The greedy form considers higher profit first and keeps the latest free slot that still meets the deadline.

**How it works:**

- GeeksforGeeks’s sort-by-profit version is O(n²) time and O(n) space. After sorting profit descending, each job scans backward from `min(n, deadline) - 1` for an empty slot.
- The heap version sorts by deadline ascending and keeps selected profits in a min-heap. A job joins when the heap is smaller than its deadline. Otherwise it replaces the smallest profit if it pays more. The page states O(n log n) time and O(n) space.
- The disjoint-set version is O(n log d) time and O(d) space, where d is the maximum deadline.
- A job that misses every slot before its deadline is dropped. One slot holds one job.

**Examples:**

```text
sort jobs by profit descending
for job in jobs:
  place it in the latest free slot at or before its deadline
```

Deadlines `[2, 1, 2, 1, 1]` and profits `[100, 19, 27, 25, 15]` schedule 2 jobs for profit 127.

**Sources:**

- [Job Sequencing Problem](https://www.geeksforgeeks.org/dsa/job-sequencing-problem/)
</details>

<details>
<summary>Use greedy algorithm to solve the ‘Minimum Number of Arrows to Burst Balloons’ problem.</summary>

### Use greedy algorithm to solve the ‘Minimum Number of Arrows to Burst Balloons’ problem.

**Definition:**

One arrow bursts every balloon whose interval contains the shot. The minimum number of arrows equals the number of balloons left after repeatedly shooting at the end of the earliest-ending remaining balloon.

**How it works:**

- Sort balloons by end coordinate. Shoot at the first end. Every later balloon that starts at or before that end is already burst.
- The next balloon that starts after that end needs a new arrow, shot at its end.
- GeeksforGeeks states time O(N log N) and auxiliary space O(1).
- The example `[[10, 16], [2, 8], [1, 6], [7, 12]]` needs 2 arrows, one through `[1, 6]` and `[2, 8]`, and one through the other pair.
- Sorting by start instead of end loses the “earliest end covers the overlap” exchange. The page sorts by the end.

**Examples:**

```text
points.sort(key=lambda p: p[1])
end, arrows = points[0][1], 1
for start, stop in points[1:]:
  if start > end:
    arrows += 1
    end = stop
```

**Sources:**

- [Find Minimum Number Of Arrows Needed To Burst All Balloons](https://www.geeksforgeeks.org/dsa/find-minimum-number-of-arrows-needed-to-burst-all-balloons/)
</details>

### Complex Greedy Strategy Implementations

<details>
<summary>How would you optimize a network’s bandwidth allocation using a greedy strategy?</summary>

### How would you optimize a network’s bandwidth allocation using a greedy strategy?

**Definition:**

When bandwidth can be split, give it first to the demand with the highest value per unit, the same rule as fractional knapsack. When a demand must be accepted whole, that rule is only an approximation.

**How it works:**

- Sort demands by value per unit of bandwidth. Fill the pipe in that order. A demand that does not fit entirely takes the remainder.
- GeeksforGeeks states the fractional algorithm is O(n log n) time and O(n) space, and it maximizes value for a splittable capacity.
- Wikipedia says the same order on an unsplittable 0/1 instance yields at least half the optimal value. It does not claim the exact optimum.
- Ranking by raw demand size, or by raw value, is the order the knapsack page shows can lose. The ratio has to be value per unit of bandwidth.

**Examples:**

```text
sort by value / bandwidth descending
grant whole demands while capacity remains
grant a fraction of the next demand
```

**Sources:**

- [Fractional Knapsack](https://www.geeksforgeeks.org/dsa/fractional-knapsack-problem/)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Write a greedy solution for compressing data using a variable-length encoding scheme.</summary>

### Write a greedy solution for compressing data using a variable-length encoding scheme.

**Definition:**

A variable-length code gives frequent symbols fewer bits. Huffman’s greedy merge produces a prefix code, so the bit stream decodes in only one way.

**How it works:**

- Build a min-heap of symbol frequencies. Merge the two lightest nodes until one tree remains. Left is 0, right is 1.
- GeeksforGeeks states time O(n log n) and space O(n) for n distinct symbols.
- A code that is a prefix of another is ambiguous. The page’s counterexample `00, 01, 0, 1` can read `0001` as more than one string. Huffman’s tree forbids that.
- Sorted frequencies can be merged with two queues in O(n) time and O(n) space. The follow-up page says unsorted input still costs a sort, so the total returns to O(n log n).
- JPEG, PNG, and MP3 are listed on the page as users of these prefix codes.

**Examples:**

```text
while heap has at least two nodes:
  merge the two lightest
emit the root-to-leaf bits
```

`f` with frequency 45 gets the one-bit code `0`. `a` with frequency 5 gets `1100`.

**Sources:**

- [Huffman Coding Algorithm](https://www.geeksforgeeks.org/dsa/huffman-coding-greedy-algo-3/)
- [Efficient Huffman Coding for Sorted Input](https://www.geeksforgeeks.org/dsa/efficient-huffman-coding-for-sorted-input-greedy-algo-4/)
</details>

<details>
<summary>Implement a greedy strategy to construct the minimum spanning tree of a graph.</summary>

### Implement a greedy strategy to construct the minimum spanning tree of a graph.

**Definition:**

A minimum spanning tree connects every vertex with no cycle and with minimum total edge weight. Kruskal and Prim both build one by a greedy rule, and both are optimal.

**How it works:**

- Kruskal sorts edges by weight and adds an edge when its ends lie in different components. A disjoint-set structure rejects the edge that would close a cycle. Stop at `V - 1` edges.
- GeeksforGeeks states Kruskal’s time as O(E log E) or O(E log V). Sorting is O(E log E). Each find or union is at most O(log V). The page notes O(log E) and O(log V) match because E is at most O(V²).
- Prim grows one component by the cheapest edge that leaves it. With a heap and adjacency lists the complexity page states O((V + E) log V). With a matrix it states O(V²).
- Wikipedia says both algorithms always find an optimal tree, and the tree need not be unique.

**Examples:**

```text
sort edges by weight
for u, v, w in edges:
  if find(u) != find(v):
    unite(u, v)
    take the edge
```

**Sources:**

- [Kruskal’s Minimum Spanning Tree (MST) Algorithm](https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)
- [Time and Space Complexity Analysis of Prim's Algorithm](https://www.geeksforgeeks.org/dsa/time-and-space-complexity-analysis-of-prims-algorithm/)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

### Advanced Theoretical Concepts

<details>
<summary>What is a matroid, and how do greedy algorithms relate to matroid theory?</summary>

### What is a matroid, and how do greedy algorithms relate to matroid theory?

**Definition:**

A matroid is a family of independent sets closed under taking subsets, with an exchange property that makes the greedy algorithm exact. Wikipedia says a maximum-weight independent set in a weighted matroid is found by a greedy algorithm.

**How it works:**

- Consider elements from heaviest to lightest. Add an element when the set stays independent. Reject it when it does not.
- Wikipedia says this fact characterizes matroids. If a family is closed under subsets and, for every weighting, greedy finds a maximum-weight set, then the family is the independent sets of a matroid.
- Jack Edmonds showed that this class of linear combinatorial problems is solved by greedy. Korte and Lovász later used greedoids to prove Prim’s algorithm, which Wikipedia records as a broader class than matroids.
- The forests of a graph are a matroid. Kruskal’s “add the lightest edge that does not close a cycle” is this scan. The page says the weighted problem is solved by greedy. It does not print a separate big-O in that paragraph.
- Matroid intersection, sets independent in two matroids at once, is a different problem. The same paragraph says the largest such set can be found in polynomial time. It is not the single-matroid greedy scan.

**Examples:**

```text
sort elements by weight descending
chosen = empty
for element in elements:
  if chosen plus element is independent:
    add element
```

**Sources:**

- [Matroid](https://en.wikipedia.org/wiki/Matroid)
- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Explain the relationship between greedy algorithms and the concept of safe moves.</summary>

### Explain the relationship between greedy algorithms and the concept of safe moves.

**Definition:**

A safe move is a choice that some optimal solution contains. The exchange argument is how you show the greedy choice is safe: swap it into an optimal solution and the objective does not get worse.

**How it works:**

- Wikipedia’s proof looks at the first index where an optimal solution and the greedy solution differ, swaps in the greedy choice, and checks feasibility and the objective.
- If every greedy step is safe, induction says the finished solution is optimal.
- Prim’s next edge is safe in this sense: Wikipedia says the algorithm always produces an optimal spanning tree. The edge it adds is the lightest edge leaving the grown set.
- A move that fails the swap is not safe. Coins `1, 3, 4` for amount 6: taking 4 cannot be swapped into the two-coin optimum without increasing the number of coins.
- “Safe” is a property of the choice relative to the objective. It is not a claim that every locally largest object is safe.

**Examples:**

Activity selection: the feasible activity that finishes first is safe. Any optimum can start with it, and the count does not fall.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
</details>

<details>
<summary>Discuss how approximation algorithms use greedy strategies to find near-optimal solutions.</summary>

### Discuss how approximation algorithms use greedy strategies to find near-optimal solutions.

**Definition:**

A greedy approximation commits to a local choice and proves a bound against the optimum instead of proving equality. Wikipedia collects several of those bounds.

**How it works:**

- The greedy metric traveling-salesman tour, built by adding the next cheapest edge that can still sit in a tour, is at most `Θ(log n)` times the optimal length.
- Using the fractional-knapsack order on a 0/1 instance yields at least half the optimal value.
- Greedy set cover, load balancing, Steiner tree, and independent set are listed as further problems solved approximately this way. The page’s references include Feige’s paper titled as a `ln n` threshold for approximating set cover.
- A greedy coloring proves `χ(G) ≤ Δ(G) + 1`. That bounds the number of colors the scan uses. It does not compute the chromatic number.
- The local-ratio method is the charging proof behind several of these covering approximations. Pay a constant factor to reduce the optimum by one, and the total stays within that factor.

**Examples:**

Set cover repeatedly takes the set that covers the most still-uncovered elements. Wikipedia places that style of algorithm in the approximation list, not in the list of exact greedy algorithms such as Kruskal and Huffman.

**Sources:**

- [Greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)
- [Local ratio: A unified framework for approximation algorithms](https://dl.acm.org/doi/10.1145/1041680.1041683)
</details>

### Greedy Algorithms in Competitive Programming

<details>
<summary>Design a greedy algorithm to solve the ‘Gas Station’ problem commonly seen in competitive programming contests.</summary>

### Design a greedy algorithm to solve the ‘Gas Station’ problem commonly seen in competitive programming contests.

**Definition:**

The gas-station circuit asks for a start index such that the tank never goes negative on a loop. If the total gas is at least the total cost, the greedy start is the station just after the last failure.

**How it works:**

- Walk once, adding `gas[i] - cost[i]` to the current tank. If the tank goes negative at `i`, no start in the failed segment works, because each of them would arrive at `i` with even less. The next candidate is `i + 1`, and the tank resets.
- GeeksforGeeks states this is O(n) time and O(1) space. Trying every start is O(n²) time and O(1) space.
- After the walk, the unique candidate is valid when total gas is at least total cost. Otherwise return −1. The one-pass form checks that difference instead of simulating the loop again.
- The tank starts empty. A station that cannot reach the next one cannot be a start unless a later failure moves the start past it.

**Examples:**

```text
tank = 0
start = 0
total = 0
for i in range(n):
  gain = gas[i] - cost[i]
  tank += gain
  total += gain
  if tank < 0:
    start = i + 1
    tank = 0
return start if total >= 0 else -1
```

**Sources:**

- [Gas Station](https://www.geeksforgeeks.org/dsa/find-a-tour-that-visits-all-stations/)
</details>

<details>
<summary>Solve the ‘Majority Element’ problem under the constraints of using a single pass algorithm and constant additional space.</summary>

### Solve the ‘Majority Element’ problem under the constraints of using a single pass algorithm and constant additional space.

**Definition:**

A majority element appears more than `⌊n/2⌋` times. Moore’s voting finds a candidate in one pass and constant memory. The page then counts that candidate in a second pass, because a candidate is not always a majority.

**How it works:**

- Keep a candidate and a count. A match increments the count. A mismatch decrements it. A count of 0 replaces the candidate with the current value and sets the count to 1.
- GeeksforGeeks states the algorithm is O(n) time and O(1) space. The nested count is O(n²) time and O(1) space. Sorting is O(n log n). A hash map is linear time and O(n) space, which breaks the memory constraint.
- The first loop is the single pass that produces the only possible majority. If a majority exists, it must be that candidate, because every other value is cancelled more often than it appears.
- The second loop counts the candidate. Return it when the count exceeds `n/2`. Otherwise return −1. The page requires this check when absence is allowed.
- `[1, 1, 2, 1, 3, 5, 1]` returns 1. `[2, 13]` returns −1.

**Examples:**

```text
candidate, count = None, 0
for x in arr:
  if count == 0: candidate, count = x, 1
  elif x == candidate: count += 1
  else: count -= 1
# second pass: return candidate only if it occurs more than n/2 times
```

**Sources:**

- [Majority Element](https://www.geeksforgeeks.org/dsa/majority-element/)
</details>
