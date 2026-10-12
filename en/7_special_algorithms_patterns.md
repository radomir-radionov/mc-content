# Algorithms — Patterns

Two pointers, sliding window, and quick greedy vs DP decisions.

[← Back to hub](./7_special_algorithms.md)

### Related docs

* [Algorithms — Big O](./7_special_algorithms_core.md) · [Algorithms — Common](./7_special_algorithms_common.md) · [Algorithms — Patterns](./7_special_algorithms_patterns.md) · [Data structures](./7_special_data_structures.md)

### Sections

- [Interview patterns](#interview-patterns)

---

## 7. Interview patterns

<details>
  <summary>Two pointers</summary>

### Two pointers

**Definition:** Two indices moving through array/string toward each other or in same direction.

**When to use:** Sorted pair sum, palindrome check, remove duplicates, merge sorted arrays.

**Pluses and minuses:**

| + | − |
|------|------|
| O(n) instead of O(n²) | Requires sorted or structured input |
| In-place | Pattern recognition needed |

**Examples:**

```js
function twoSumSorted(nums, target) {
  let lo = 0, hi = nums.length - 1;
  while (lo < hi) {
    const sum = nums[lo] + nums[hi];
    if (sum === target) return [lo, hi];
    if (sum < target) lo++;
    else hi--;
  }
  return null;
}
```

**Thoughts:**

This variant needs a **sorted** array. Unsorted Two Sum → Map of complements (next block).

---

</details>

<details>
  <summary>Two Sum with Map</summary>

### Two Sum with Map

**Definition:** Find two indices `i`, `j` such that `nums[i] + nums[j] === target` in **one pass** using a hash map of value → index.

**How it works:**

- For each `nums[i]`, look up `need = target - nums[i]` in a `Map`. If present, return `[map.get(need), i]`. Otherwise `map.set(nums[i], i)`.
- Time **O(n)**, extra space **O(n)**. Nested loops are O(n²).
- **`Map` vs object:** object keys are strings (`{1: true}` and `{"1": true}` collide). `Map` keeps number keys as numbers (`SameValueZero`). Safer for integer arrays.

**When to use:**

Classic live-coding Two Sum. Sorted-only variant → two pointers (previous block).

#### Common pitfalls

Using `{}` with numeric keys. Forgetting that the map stores *previous* indices so the same element is not paired with itself.

**Pluses and minuses:**

| + | − |
|------|------|
| One pass, O(1) amortized lookup | O(n) memory |
| Works on unsorted input | Does not find all pairs unless you keep going |

**Examples:**

```js
function twoSum(nums, target) {
  const seen = new Map(); // value → index
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (seen.has(need)) return [seen.get(need), i];
    seen.set(nums[i], i);
  }
  return null;
}
```

**Thoughts:** *Store what you have seen; look up the complement — `Map` keeps numeric keys, objects do not.*

**Sources:**

[MDN Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

---

</details>

<details>
  <summary>Sliding window</summary>

### Sliding window

**Definition:** Maintain a window `[left, right]` over array/string; expand right, shrink left when constraint violated.

**When to use:** Max sum subarray of size k, longest substring without repeat, minimum window substring.

**Pluses and minuses:**

| + | − |
|------|------|
| O(n) for substring/subarray problems | Window invariant must be clear |
| Avoids re-scanning | |

**Examples:**

```js
function maxSumSubarray(nums, k) {
  let sum = 0, max = 0;
  for (let i = 0; i < nums.length; i++) {
    sum += nums[i];
    if (i >= k) sum -= nums[i - k];
    if (i >= k - 1) max = Math.max(max, sum);
  }
  return max;
}
```

**Thoughts:**

Fixed-size vs variable-size window. Map/set for character counts in string windows.

---

</details>

<details>
  <summary>Greedy vs DP — quick decision</summary>

### Greedy vs DP — quick decision

**Definition:**

Heuristic checklist for choosing greedy vs dynamic programming in interview and design problems.

| Question | Lean toward |
|----------|-------------|
| Optimal substructure + overlapping subproblems? | DP |
| Local choice provably safe? | Greedy |
| Count all ways / min/max with constraints? | DP |
| Scheduling / intervals with sort? | Often greedy |

---

</details>
