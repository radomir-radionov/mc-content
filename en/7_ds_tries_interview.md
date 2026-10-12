# Trie — Interview

Answers cover the published question list.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Trie — Interview](./7_ds_tries_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Trie Data Structure](https://devinterview.io/questions/data-structures-and-algorithms/trie-data-structure-interview-questions/).

### Understanding Tries

<details>
<summary>What is a Trie?</summary>

### What is a Trie?

**Definition:**

A trie is a tree of characters. Each edge is one character, and a path from the root spells a prefix or a whole key.

**How it works:**

- A node holds a map from character to child, plus a flag that a word ends here. The flag is needed because a word can be a prefix of another.
- Insert, search, and starts-with walk one edge per character, so a key of length L costs O(L) and does not depend on how many other keys share no prefix.
- Shared prefixes share nodes. That is the space win over storing each string separately when the dictionary overlaps.
- The alphabet size decides whether the child map is an array, a hash map, or a sorted list.

**Examples:**

Insert `cat` and `car`. The node `c → a` is shared. `t` and `r` are different children, and both are marked as word ends.
</details>

<details>
<summary>What are the advantages of Tries over Hash Tables?</summary>

### What are the advantages of Tries over Hash Tables?

**Definition:**

A trie finds every key with a given prefix and can list them in sorted order. A hash table finds an exact key and scatters order.

**How it works:**

- Prefix search, autocomplete, and longest-prefix match are a walk to the prefix node plus a subtree scan.
- There is no hash collision and no load factor. The cost is proportional to the key length.
- Sorted iteration is a depth-first walk of the children in character order.
- A hash table is smaller and faster for exact lookup of long, unshared keys. The trie pays a node per character.
</details>

<details>
<summary>What are Advantages and Disadvantages of a Trie?</summary>

### What are Advantages and Disadvantages of a Trie?

**Definition:**

Tries excel at prefixes and shared structure. They waste space when keys barely overlap or the alphabet array is wide and sparse.

**How it works:**

- Lookup is O(L) with a small constant when children are an array. Failed searches stop at the first missing edge.
- An array of 26 or 256 pointers per node is fast and mostly empty. A hash map of children is denser and slower per step.
- Delete must unmark the word and then prune nodes that are not a word end and have no children.
- Unicode and long random ids make the tree deep or wide. A hash table or a radix tree (compressed trie) is often a better fit.
</details>

<details>
<summary>How do you handle the case sensitivity problem in Tries?</summary>

### How do you handle the case sensitivity problem in Tries?

**Definition:**

Decide the alphabet before the first insert. Either fold case when you walk, or store both cases as different edges.

**How it works:**

- Folding to lower case at insert and at search makes `Cat` and `cat` one word. Do it with a Unicode case fold, not only ASCII `toLowerCase`, if the dictionary is not English.
- Keeping case means `C` and `c` are different children. Search is case-sensitive and matches the stored spelling.
- A third option stores the original spelling at the terminal node and walks a case-folded path, so autocomplete can show `Cat` while search ignores case.
- Mixing the policies between insert and search is a bug: one path lowercases and the other does not, and the word is missing.
</details>

### Applications and Use Cases of Tries

<details>
<summary>What are some Practical Applications of a Trie?</summary>

### What are some Practical Applications of a Trie?

**Definition:**

Tries are used wherever the next character narrows a set of strings.

**How it works:**

- Autocomplete and spell check walk the prefix and then explore the subtree.
- Longest-prefix match in a router walks the bits of an address until the deepest marked prefix.
- A dictionary in a word game tests a prefix before extending a path.
- T9 and some tokenizers use the same character tree. Exact-id lookup of random UUIDs usually does not.
</details>

<details>
<summary>Describe how a Trie can be used for implementing autocomplete functionality.</summary>

### Describe how a Trie can be used for implementing autocomplete functionality.

**Definition:**

Autocomplete walks the characters typed so far, then collects word-end flags in that subtree, usually the first k in sorted order.

**How it works:**

- If the walk dies on a missing edge, there are no suggestions.
- A depth-first walk of children in alphabetical order yields lexicographic suggestions. Stop after k hits.
- A frequency stored at the terminal node lets you keep a small heap of the best k instead of the alphabetical k.
- The walk is O(L + output). Precomputing the top suggestions on each node makes the query faster and the update slower.

**Examples:**

Dictionary `car`, `card`, `cat`. The prefix `ca` lands on the shared node. A walk finds `car`, `card`, and `cat`.
</details>

<details>
<summary>Explain the use of a Trie in IP Routing to match IP prefixes.</summary>

### Explain the use of a Trie in IP Routing to match IP prefixes.

**Definition:**

A routing trie walks the bits of the destination address and remembers the deepest prefix that was marked as a route.

**How it works:**

- Each edge is a bit, or a stride of several bits. A marked node stores the next hop for that prefix.
- Search does not stop at the first mark. A longer matching prefix overrides a shorter one. The last mark on the walk is the answer.
- If the walk cannot continue and a mark was seen higher up, that higher prefix still matches.
- A compressed radix tree (a Patricia trie) collapses chains of one child, which is how forwarding tables stay small.

**Examples:**

Routes `10.0.0.0/8` and `10.1.0.0/16`. Address `10.1.2.3` walks past both marks and uses the `/16`. Address `10.2.2.3` stops being able to follow the `/16` and keeps the `/8`.
</details>

<details>
<summary>How is a Trie used in text spell-checking and correction systems?</summary>

### How is a Trie used in text spell-checking and correction systems?

**Definition:**

The dictionary is a trie. A word is correct if the walk ends on a word flag. Suggestions are words within a small edit distance of the path that failed.

**How it works:**

- Exact check is one walk of length L.
- Correction explores deletions, insertions, substitutions, and sometimes transpositions, pruning when the remaining budget of edits cannot reach a word.
- Shared prefixes mean one wrong suffix does not restart the search from scratch for every dictionary word.
- A frequency on the terminal node ranks `teh` → `the` above a rare word at the same distance.
</details>

### Implementation and Operations in Tries

<details>
<summary>Name some Trie Implementation strategies.</summary>

### Name some Trie Implementation strategies.

**Definition:**

The child map is the implementation choice. The walk algorithm stays the same.

**How it works:**

- A fixed array indexed by character is the fastest for a small alphabet and the most wasteful when most slots are empty.
- A hash map per node is compact for sparse Unicode and adds hashing on every step.
- A sorted vector of children is compact and makes ordered walks natural. Lookup inside the node is logarithmic in the number of children.
- A radix tree compresses single-child chains into one edge labeled with a string. A ternary search tree stores one character and three links.
</details>

<details>
<summary>Write the code to insert a word into a Trie.</summary>

### Write the code to insert a word into a Trie.

**Definition:**

Start at the root and for each character create the child if it is missing, then move to that child. Mark the last node as a word.

**How it works:**

- Creating missing edges is what makes the first insert of a prefix allocate the path.
- A second insert of the same word only sets the end flag again.
- Do not mark intermediate nodes unless that shorter string was also inserted.
- Time is O(L) child lookups, times the cost of the map.

**Examples:**

```text
def insert(root, word):
    node = root
    for ch in word:
        node = node.children.setdefault(ch, Node())
    node.is_word = True
```
</details>

<details>
<summary>Implement a Search function to determine if a word is included in a Trie.</summary>

### Implement a Search function to determine if a word is included in a Trie.

**Definition:**

Walk the characters. If any edge is missing, the word is absent. If the walk finishes, the node must be marked as a word, not merely a prefix.

**How it works:**

- `startsWith` returns true at the end of the walk even when the end flag is false.
- Search returns false for a proper prefix of an inserted word.
- An empty word is present only if the root itself is marked, which is a policy you should decide.
- The walk is O(L).

**Examples:**

```text
def search(root, word):
    node = root
    for ch in word:
        node = node.children.get(ch)
        if node is None:
            return False
    return node.is_word
```

`car` is found. `ca` walks successfully and returns false if only `car` was inserted.
</details>

<details>
<summary>Write a script to Delete a word from a Trie.</summary>

### Write a script to Delete a word from a Trie.

**Definition:**

Unmark the terminal node, then delete nodes on the way back while they have no children and are not the end of another word.

**How it works:**

- If the word is only a prefix of a longer word, clearing the flag is enough. The nodes stay because the longer word uses them.
- If a node still has other children, stop pruning there.
- If the word was never inserted, the walk fails and the trie is unchanged.
- A recursive delete returns whether the child was removed, so the parent can drop that map entry.

**Examples:**

```text
def delete(node, word, i=0):
    if i == len(word):
        if not node.is_word:
            return False
        node.is_word = False
        return len(node.children) == 0
    ch = word[i]
    child = node.children.get(ch)
    if child is None or not delete(child, word, i + 1):
        return False
    del node.children[ch]
    return not node.is_word and len(node.children) == 0
```
</details>

### Trie Variants and Comparison with Other Data Structures

<details>
<summary>Compare Trie vs. Binary Search Tree.</summary>

### Compare Trie vs. Binary Search Tree.

**Definition:**

A trie branches on the next character. A BST branches on a comparison of the whole key.

**How it works:**

- Trie search is O(L). BST search is O(L · height) if each comparison scans the string, or O(height) after the keys are interned.
- A trie gives prefix queries for free. A BST of whole strings gives predecessor and successor of a full key, and prefix queries need extra care.
- A balanced BST has a logarithmic height in the number of keys. A trie is as deep as the longest key, which can be more or less than log n.
- The trie spends memory on characters. The BST spends memory on key pointers and balance fields.
</details>

<details>
<summary>How to choose between a Hash Table and a Trie?</summary>

### How to choose between a Hash Table and a Trie?

**Definition:**

Choose a hash table for exact lookup of opaque keys. Choose a trie when the characters themselves are the query, especially prefixes.

**How it works:**

- Hash lookup is expected O(L) to hash the key plus O(1) in the table, with a collision tail. Trie lookup is O(L) with no collision tail and a larger constant if the map is wide.
- Autocomplete, longest prefix, and lexicographic enumeration point at the trie.
- Random ids, cache keys, and records addressed by a primary key point at the hash table.
- A radix tree or a Patricia trie is the compromise when you want prefixes without a node per character.
</details>

<details>
<summary>What is a Suffix Trie, and how does it differ from a standard Trie?</summary>

### What is a Suffix Trie, and how does it differ from a standard Trie?

**Definition:**

A suffix trie inserts every suffix of one text, so any substring is a prefix of some suffix and can be found by a walk.

**How it works:**

- A standard trie stores a dictionary of separate words. A suffix trie stores one string's suffixes, often with an index of where that suffix starts.
- Naively it has O(n²) nodes. A suffix tree compresses unary paths and has O(n) nodes. A suffix array stores the sorted suffixes in O(n) integers.
- Search of a pattern of length m is O(m) in the trie or tree after the build, and reports every occurrence via the leaves below that node.
- Use it for many queries against a fixed text. For one search, KMP is less machinery.

**Examples:**

Text `banana`. The suffix `ana` is inserted, so the pattern `ana` walks that path and can report the start indexes 1 and 3.
</details>

<details>
<summary>How does a Compressed Trie or Radix Tree improve on a standard Trie structure?</summary>

### How does a Compressed Trie or Radix Tree improve on a standard Trie structure?

**Definition:**

A compressed trie, also called a radix tree, stores the same set of strings as a character trie but merges a chain of single-child nodes into one edge labeled with a string.

**How it works:**

- In a plain trie each edge is one character, so a long unique tail is a spine of nodes that never branch.
- Compression replaces that spine with one edge. The label is often a slice `(start, end)` into the original key, not a copied string.
- Lookup still follows the query from the root. On a compressed edge it must match the whole label, not a single character.
- Wikipedia notes the radix tree as the usual space optimization of a trie. A Patricia trie is the same idea on bits.

**When to use:**

Use it when keys share a prefix and then run off into long unique suffixes, so node objects dominate memory.

**Examples:**

Keys `romane` and `romanus` share `roman`. A radix tree then has two edges, `e` and `us`, instead of five extra single-child nodes.

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

### Advanced Operations in Tries

<details>
<summary>Find the longest common prefix among a set of strings using a Trie.</summary>

### Find the longest common prefix among a set of strings using a Trie.

**Definition:**

Insert every string, then walk from the root while the current node has exactly one child and is not itself the end of a shorter key. The characters on that path are the longest common prefix.

**How it works:**

- If one key is a prefix of the others, stop at the node marked as a word. Extending further would drop that shorter key.
- If the root already has two or more children, the common prefix is empty.
- Building the trie is O(total characters). The walk is O(length of the prefix).
- The same walk is what a radix tree stores on its first edge when every key shares that prefix.

**Examples:**

```text
insert ten, tea, ted
root -t-> node -e-> node
the only path is "te"; the next node branches to n/a/d
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Implement an algorithm to count the total number of words stored in a Trie.</summary>

### Implement an algorithm to count the total number of words stored in a Trie.

**Definition:**

A word is a node with the end-of-key flag set. Count those flags with a depth-first walk, or keep a counter that insert and delete update.

**How it works:**

- Do not count every node. Intermediate nodes are prefixes, not stored words, unless they were inserted as words.
- A full scan is O(number of nodes), which is proportional to the total stored characters in an uncompressed trie.
- An incremental counter is O(1) per query and stays correct if delete clears the flag only when that exact word is removed.
- Shared prefixes do not double-count: `tea` and `team` are two flags on one path.

**Examples:**

```text
def count_words(node):
    total = 1 if node.is_word else 0
    for child in node.children.values():
        total += count_words(child)
    return total
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>List all words with a given prefix stored in a Trie.</summary>

### List all words with a given prefix stored in a Trie.

**Definition:**

Walk the prefix. If any edge is missing, the answer is empty. Otherwise collect every end-of-word node in the subtree, appending the characters along the way.

**How it works:**

- The walk to the prefix node is O(length of the prefix) child lookups.
- The collection visits only the subtree, so the cost tracks how many completions exist, not the whole dictionary.
- This is the autocomplete operation Wikipedia describes: the trie is organized by shared prefixes so completion lists fall out of one subtree.
- Sort the children if the caller wants lexicographic order. A hash map of children does not give that order by itself.

**When to use:**

Use a trie here when the product is prefix completion. A hash set of whole words can test membership but cannot list completions without scanning every key.

**Examples:**

```text
def words_with(node, prefix):
    for ch in prefix:
        node = node.children.get(ch)
        if node is None:
            return []
    out = []
    def dfs(n, path):
        if n.is_word:
            out.append(prefix + path)
        for ch, child in n.children.items():
            dfs(child, path + ch)
    dfs(node, "")
    return out
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Write an algorithm for the word search in a matrix grid using a Trie.</summary>

### Write an algorithm for the word search in a matrix grid using a Trie.

**Definition:**

Insert the dictionary into a trie, then start a depth-first search from every cell. At each step follow the edge for that letter and skip branches the trie does not contain.

**How it works:**

- Mark the current cell visited for this path so a letter is not reused, then unmark it on the way back.
- A missing child means this path cannot spell any dictionary word. That prune is the reason to use a trie instead of testing each word against the grid.
- When the node is marked as a word, record it. Keep walking if longer words continue through the same prefix.
- Time is bounded by the grid size times branching, and the trie cuts branches that no word uses.

**Examples:**

```text
board of letters, dict = {oath, pea, eat}
insert into trie
from each cell, dfs: consume letter, move 4-direction, backtrack
emit a word when is_word is set
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

### Optimization and Efficiency

<details>
<summary>How can using a Trie reduce the time complexity of searching for a word in a dataset?</summary>

### How can using a Trie reduce the time complexity of searching for a word in a dataset?

**Definition:**

Search follows one edge per character of the query. Wikipedia lists trie search as O(n) in the key length, in both the average and the worst case, rather than a scan of every stored string.

**How it works:**

- A linear scan of D words of length L costs O(D·L) character comparisons in the worst case.
- A trie pays O(L) child lookups. The dictionary size shows up only in how expensive one child lookup is (array index versus a map).
- Failed searches stop at the first missing edge, so a wrong prefix does not read the rest of the dictionary.
- The trade is memory: an uncompressed trie can use O(total characters) nodes, and Wikipedia lists worst-case space as O(w·n) for alphabet width w.

**When to use:**

Prefer the trie when many queries share prefixes or you need the worst-case bound to depend on the query length. A hash table of whole words is often faster for exact lookup and uses less memory when prefixes are not shared.

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Discuss the space optimizations that can be made in a Trie.</summary>

### Discuss the space optimizations that can be made in a Trie.

**Definition:**

An uncompressed trie spends a node per character and often a wide array of empty child slots. Compression, a smaller child map, and bitwise keys cut that overhead.

**How it works:**

- Replace a fixed alphabet array with a hash map or a sorted list of the children that exist. Lookup inside the node gets slightly more expensive; empty slots disappear.
- Collapse single-child chains into one radix-tree edge. Wikipedia calls this the notable space optimization.
- A bitwise trie uses one bit per level instead of a whole character, which suits integers and addresses.
- At a leaf, store the remaining suffix as one string instead of a node per character.

**When to use:**

Compress when the trie is large and most nodes have one child. Keep a flat array only for a tiny alphabet where the index is worth the empty slots.

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>What is Memory Pooling, and how can it make Trie operations more efficient?</summary>

### What is Memory Pooling, and how can it make Trie operations more efficient?

**Definition:**

Memory pooling allocates many trie nodes from one pre-sized block (an arena) instead of asking the allocator for each node separately.

**How it works:**

- A trie node is small and there are many of them. Per-node allocation scatters them across the heap, so a walk jumps through unrelated cache lines.
- An arena hands out nodes with bump allocation. Freeing the whole trie is one release of the block, which matches a dictionary you build and then discard.
- Individual delete is harder: the pool usually does not return one node to the OS. A free list inside the arena covers that if words are removed often.
- The lookup algorithm does not change. Only the addresses of the child pointers get closer together.

**When to use:**

Pool nodes when the trie is rebuilt in bulk or lives for the process lifetime. Skip the pool for a tiny dictionary where allocator overhead is invisible.

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

### Implementation Challenges

<details>
<summary>How would you serialize and deserialize a trie?</summary>

### How would you serialize and deserialize a trie?

**Definition:**

Write the tree in preorder: for each node store the edge character, the end-of-word flag, the child count, then the children. Reading that stream rebuilds the same links.

**How it works:**

- Preorder matches the structure Wikipedia sketches: a node is a set of child links plus an optional value at a terminal.
- Store children in sorted character order so the bytes are stable across runs.
- A radix-tree edge must also store the label length, or the label itself, or the stream cannot tell where the next node starts.
- JSON of nested maps is a readable form of the same walk. A binary preorder is smaller and faster to load.

**Examples:**

```text
write(node):
    emit is_word, child_count
    for ch, child in sorted(node.children):
        emit ch
        write(child)
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Implement a Trie that supports wildcard characters ‘?’ and ‘*’ for pattern matching.</summary>

### Implement a Trie that supports wildcard characters ‘?’ and ‘*’ for pattern matching.

**Definition:**

A literal character follows one edge. `?` tries every child once. `*` tries both “consume no extra character” and “consume one child and keep the star”, with memoization so the same (node, pattern index) pair is not expanded twice.

**How it works:**

- Without a memo key the star branches exponentially on long patterns.
- `?` must not match the end of the key. It consumes exactly one existing edge.
- A match succeeds only if the pattern is exhausted on a node marked as a word, or a trailing `*` can stay on that node.
- This walks the trie Wikipedia defines (child link per character). It does not require a second index structure.

**When to use:**

Use the wildcard walk for pattern search over a dictionary (`a?c`, `a*c`). For a single pattern against one text, a regular expression is enough and a trie adds nothing.

**Examples:**

```text
def match(node, pat, i, memo):
    key = (id(node), i)
    if key in memo:
        return memo[key]
    if i == len(pat):
        memo[key] = node.is_word
        return node.is_word
    ok = False
    if pat[i] == "*":
        ok = match(node, pat, i + 1, memo) or any(
            match(child, pat, i, memo) for child in node.children.values()
        )
    elif pat[i] == "?":
        ok = any(match(child, pat, i + 1, memo) for child in node.children.values())
    else:
        child = node.children.get(pat[i])
        ok = child is not None and match(child, pat, i + 1, memo)
    memo[key] = ok
    return ok
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Implement a Trie backed Subsequence Indexing system for a rapid text searching.</summary>

### Implement a Trie backed Subsequence Indexing system for a rapid text searching.

**Definition:**

Index each word in a trie, then answer a subsequence query by a walk that may skip characters of the stored word. Memoize the pair (node, position in the query) so the search stays practical.

**How it works:**

- A full subsequence trie, with an edge for every later character, is exponential in word length. Do not materialize it.
- The query `tse` against a stored word matches if those letters appear in order. From a node, either take the edge equal to the next query character, or the search is allowed to have skipped earlier stored letters only if you stored the original word and scan it. On a plain trie the cheaper exact tool is prefix search.
- For “letters of the query appear in order inside some dictionary word”, depth-first search from the root: at each node try the child for the current query character (advance the query) and also try every child without advancing, which means skipping a stored letter. Memoization is required.
- Exact word and prefix queries still use the ordinary O(length) walk. The subsequence walk is the extra operation.

**When to use:**

Use the skip-walk when the product is subsequence lookup (`tse` inside `these`). Use ordinary trie search when the query is a prefix or a whole word.

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Design a Trie that supports insert, search, and startsWith operations efficiently.</summary>

### Design a Trie that supports insert, search, and startsWith operations efficiently.

**Definition:**

One node holds a map of children and an end-of-word flag. `insert` creates missing edges, `search` requires the flag at the end of the walk, and `startsWith` only requires the walk to finish.

**How it works:**

- Each operation does one child lookup per character. With a hash map that is expected O(L). With a fixed alphabet array it is strict O(L).
- `search("app")` is false when only `apple` was inserted, because the node after `app` is not terminal. `startsWith("app")` is true.
- Wikipedia’s search stops on a null link. That is the failure case for both operations.
- Keep the root empty. Do not store the character on the node; the incoming edge is the character.

**Examples:**

```text
def insert(root, word):
    node = root
    for ch in word:
        node = node.children.setdefault(ch, Node())
    node.is_word = True

def search(root, word):
    node = walk(root, word)
    return node is not None and node.is_word

def starts_with(root, prefix):
    return walk(root, prefix) is not None
```

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>How can a Balanced Trie be achieved, and what are the factors affecting the balance?</summary>

### How can a Balanced Trie be achieved, and what are the factors affecting the balance?

**Definition:**

A character trie is not rebalanced by rotations. Its depth is the key length, which Wikipedia’s bounds already treat as n in the O(n) search. “Balance” here means keeping that depth honest and keeping each node’s child directory cheap.

**How it works:**

- Unary chains make the tree tall without adding choices. A radix compression shortens depth to the number of branching points plus label length.
- The shape follows the data. Many keys that share a short prefix and then differ produce a wide level. A few very long keys produce a deep path. You cannot rotate a long key up the way an AVL tree rotates a node.
- Inside one node, a fixed array is O(1) and wide; a tree or sorted vector of children is balanced in the number of children and costs O(log alphabet) per step.
- Worst-case space in the Wikipedia table grows with alphabet width times key length. Shrinking the alphabet (bits instead of characters) changes both width and depth.

**When to use:**

Compress unary paths when depth is wasted on non-branching chains. Switch the child container when one node’s alphabet is large and sparse. Do not import AVL rotations onto the trie itself.

**Sources:**

https://en.wikipedia.org/wiki/Trie
</details>
