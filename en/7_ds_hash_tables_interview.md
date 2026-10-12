# Hash Tables — Interview

Answers cover the published question list.
[← Back to hub](./7_special_data_structures.md)

### Related docs

- [Hash Tables — Interview](./7_ds_hash_tables_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Hash Tables](https://devinterview.io/questions/data-structures-and-algorithms/hash-table-data-structure-interview-questions/).

### Basic Hash Table Concepts

<details>
<summary>What is a Hash Table?</summary>

### What is a Hash Table?

**Definition:**

A hash table stores key-value pairs in an array and uses a hash function to turn a key into a bucket index.

**How it works:**

- Lookup, insert, and delete are expected O(1) when keys spread across buckets. One overloaded bucket makes those operations O(n).
- Two keys can hash to the same index. The table needs a collision policy: a chain, or another slot in the array.
- The load factor is the number of keys divided by the number of buckets. Growing the array and rehashing keeps that ratio in a chosen range.
- Keys need a stable hash and an equality test. Mutable keys that change hash after insertion are lost.

**Examples:**

Buckets of size 8. Key `"tea"` hashes to 3 and is stored in bucket 3. A later lookup hashes `"tea"` again and finds that slot.
</details>

<details>
<summary>What is Hashing?</summary>

### What is Hashing?

**Definition:**

Hashing maps a key of any size to a fixed-size integer, then usually reduces that integer to a bucket index.

**How it works:**

- A hash of a password or a file is a fingerprint. A hash inside a table is an index function. The same word covers both, and the requirements differ.
- For a table, equal keys must hash equal, and unequal keys should spread out. A cryptographic hash is slower than a table needs.
- The index is often `hash(key) modulo capacity`, or a mask when the capacity is a power of two.
- A hash is not encryption. It is not meant to be reversed, and a table hash is not meant to resist an attacker unless you asked for that.
</details>

<details>
<summary>Explain the difference between Hashing and Hash Tables.</summary>

### Explain the difference between Hashing and Hash Tables.

**Definition:**

Hashing is the function from key to integer. A hash table is the data structure that stores pairs and resolves collisions with that function.

**How it works:**

- You can hash without a table: checksums, content addresses, hash sets of signatures.
- A table adds buckets, a collision strategy, a load factor, and rehashing.
- A bad function ruins a good table. A perfect function still needs a place to put the value and a plan for growth.
- Sets and maps in standard libraries are hash tables (or trees) that hide the function.
</details>

<details>
<summary>Provide a simple example of a Hash Function.</summary>

### Provide a simple example of a Hash Function.

**Definition:**

A teaching hash folds the bytes of the key into one integer, then reduces it to the bucket count.

**How it works:**

- A polynomial hash treats characters as digits: `h = h × 31 + code` for each character, in a fixed integer width.
- The multiplier spreads nearby strings. A multiplier of 1 just sums codes and collides anagrams.
- The bucket is `h modulo capacity`. If `h` can be negative, take a non-negative remainder.
- This is not a cryptographic hash. It is fast and good enough when keys are not chosen by an attacker.

**Examples:**

```text
"ab" with multiplier 31:
h = 0
h = 31 + 'a'
h = h * 31 + 'b'
bucket = h % 8
```
</details>

<details>
<summary>How do hash tables work under the hood, specifically in languages like Java, Python, and C++?</summary>

### How do hash tables work under the hood, specifically in languages like Java, Python, and C++?

**Definition:**

The standard maps are buckets plus a published hash and equality on the key, with growth when the load crosses a threshold.

**How it works:**

- Java `HashMap` chains entries. In current JDKs a long chain becomes a tree so a collision attack is O(log n) instead of O(n). The index uses a mix of the key's `hashCode`. Null keys are allowed once.
- Python's `dict` is open addressing in a sparse index array that points at a dense entry table. Insertion order is remembered. The hash is salted per process.
- C++ `std::unordered_map` is typically a chain of buckets. `hash` and `equal_to` are template parameters. Worst-case complexity is allowed to be linear.
- All three rehash into a larger table. Iterators and references can be invalidated by that growth, with language-specific rules.
</details>

### Hash Functions and Collisions

<details>
<summary>What are Hash Collisions?</summary>

### What are Hash Collisions?

**Definition:**

A collision is two different keys whose hashes reduce to the same bucket.

**How it works:**

- Pigeonhole says collisions are unavoidable once there are more possible keys than buckets.
- The table must still return the right value, so it compares keys with equality after the hash match.
- A few collisions are normal. A hash that dumps every key into one bucket turns the table into a list.
- An attacker who can pick keys to force one bucket can make lookups linear unless the implementation randomizes or trees the chain.

**Examples:**

Capacity 4. `"a"` and `"e"` both land in bucket 1. Equality then tells them apart inside that bucket.
</details>

<details>
<summary>Name some Collision Handling Techniques.</summary>

### Name some Collision Handling Techniques.

**Definition:**

The two families are chaining, which stores several keys in one bucket, and open addressing, which stores one key per slot and probes for another slot.

**How it works:**

- Separate chaining: each bucket is a list or a small tree.
- Open addressing: linear probing, quadratic probing, or double hashing walk the array until an empty slot appears.
- Robin Hood and cuckoo hashing are open-addressing variants that bound probe length differently.
- Perfect hashing builds a collision-free function for a frozen key set. It does not fit a map that keeps growing.
</details>

<details>
<summary>Describe the characteristics of a good hash function.</summary>

### Describe the characteristics of a good hash function.

**Definition:**

A good table hash is fast, deterministic, and spreads keys so buckets stay short.

**How it works:**

- Equal keys produce the same hash. If equality says two keys match, their hashes must match.
- A one-bit change in the key should move the hash a lot, so similar strings do not share a bucket.
- The function should be cheap next to the rest of the lookup. A cryptographic hash is the wrong default for an in-memory map.
- For untrusted keys, a secret seed stops an attacker from precomputing one bucket. The seed must stay fixed for the life of the table.
</details>

<details>
<summary>What is Separate Chaining, and how does it handle collisions?</summary>

### What is Separate Chaining, and how does it handle collisions?

**Definition:**

Separate chaining gives every bucket its own small collection. Keys that hash there are appended to that collection.

**How it works:**

- Insert hashes to a bucket and walks the chain for an equal key. A match updates the value. A miss appends a node.
- Lookup and delete walk the same chain and compare keys, not only hashes.
- A linked list is the simple chain. A long chain can be converted to a tree, which is what Java does past a threshold.
- Chains make deletion easy and tolerate load factors above 1. They pay an extra pointer per entry and an extra allocation.

**Examples:**

Bucket 2 holds `("cat", 1) -> ("car", 4)`. Looking up `"car"` hashes to 2, skips `"cat"`, and returns 4.
</details>

<details>
<summary>Explain Open Addressing and its different probing techniques.</summary>

### Explain Open Addressing and its different probing techniques.

**Definition:**

Open addressing stores every entry in the bucket array. A collision looks at other indexes according to a probe sequence until it finds the key or an empty slot.

**How it works:**

- Linear probing tries `index + 1`, `index + 2`, and so on, modulo capacity. It is cache-friendly and forms clusters.
- Quadratic probing tries `index + i²`. It spreads clusters and can miss empty slots if the capacity and the step are not chosen so the probe hits every index.
- Double hashing uses a second hash as the step, so different keys that share a start index walk different sequences.
- Deletion cannot leave a truly empty slot in the middle of a probe chain, or later lookups stop too early. A tombstone marks 'continue, but this slot may be reused'.

**Examples:**

Linear probe, capacity 5, both keys want slot 1. The second key is stored at slot 2. Lookup of the second key checks 1, sees a different key, then checks 2.
</details>

### Complexity and Performance

<details>
<summary>Explain the Time and Space Complexity of a Hash Table.</summary>

### Explain the Time and Space Complexity of a Hash Table.

**Definition:**

Average insert, lookup, and delete are O(1). The worst case is O(n) when every key collides. Space is proportional to the number of buckets plus the number of entries.

**How it works:**

- Expected O(1) assumes a hash that spreads keys and a load factor held under a constant by rehashing.
- One rehash copies every entry and is O(n). Across a series of inserts it is amortized O(1) per insert.
- Chaining uses extra pointers. Open addressing uses empty slots, so the array is larger than the entry count.
- Iteration is O(capacity + n) for many layouts, not O(n), because empty buckets are visited.
</details>

<details>
<summary>What is a Load Factor in the context of Hash Tables?</summary>

### What is a Load Factor in the context of Hash Tables?

**Definition:**

The load factor is `n / capacity`: how many entries sit per bucket on average.

**How it works:**

- A low factor means short chains or short probes and more empty memory. A high factor means more collisions.
- Java's `HashMap` defaults to 0.75. Open addressing usually stays lower, because probes get long as the array fills.
- Crossing the threshold allocates a larger array, often about twice as big, and reinserts every key.
- The factor is an average. A bad hash can overload one bucket while the average still looks fine.
</details>

<details>
<summary>How does the load factor affect performance in a hash table?</summary>

### How does the load factor affect performance in a hash table?

**Definition:**

As the load factor grows, the expected chain length or probe length grows, so each operation does more key comparisons.

**How it works:**

- With chaining and a decent hash, expected chain length is about the load factor. Lookup stays O(1) while that factor is a constant, with a larger constant when the factor is 2 than when it is 0.5.
- With open addressing, probe length climbs sharply as the factor approaches 1. A full table cannot insert.
- Rehashing trades a rare O(n) copy for a return to short operations. Doing it too often wastes time. Doing it too late wastes comparisons.
- Cache behavior also changes: a huge sparse table misses more often than a tighter one, even when the probe math looks better.
</details>

<details>
<summary>Discuss different ways to resize a hash table. What is the complexity involved?</summary>

### Discuss different ways to resize a hash table. What is the complexity involved?

**Definition:**

Resizing allocates a new bucket array and inserts every existing key into it, because indexes depend on the capacity.

**How it works:**

- Doubling is the usual growth. The new index is `hash modulo new_capacity`, not a shift of the old index, unless the hash and the power-of-two mask were designed for that.
- One resize is O(n). If you double every time the factor crosses a constant, the copies across n inserts sum to O(n), so each insert pays amortized O(1).
- Growing by a constant number of slots, such as +10, makes the total copy cost quadratic.
- Incremental rehashing moves a few old buckets on each operation so one insert does not pause to copy everything. The steady-state cost is still amortized O(1).
</details>

<details>
<summary>How can the choice of a hash function impact the efficiency of a hash table?</summary>

### How can the choice of a hash function impact the efficiency of a hash table?

**Definition:**

The hash decides the bucket. A function that clumps keys makes every operation walk a long chain no matter how large the table is.

**How it works:**

- A function that only uses the first character puts every word starting with the same letter in one bucket.
- A function that is slow, such as a cryptographic digest on a short in-memory key, dominates the lookup even when buckets are short.
- A function that ignores part of a composite key collides objects that differ only in the ignored field.
- Switching the function requires a full rehash. Mixing in a seed is the usual fix when keys are adversarial.

**Examples:**

Keys `"a1"`, `"a2"`, `"a3"` with a hash of only the first character all land in the bucket of `'a'`. Lookup of any of them scans the whole chain.
</details>

### Cryptographic Hashing

<details>
<summary>What is the difference between a Hash Function and a Cryptographic Hash Function?</summary>

### What is the difference between a Hash Function and a Cryptographic Hash Function?

**Definition:**

A hash function for a table maps a key to a bucket. A cryptographic hash, such as the SHA-2 functions hashlib always provides, is a one-way digest meant to resist finding another input with the same output.

**How it works:**

- A table hash only has to spread keys. It can be fast and reversible. An attacker is not part of the design.
- hashlib documents SHA-224 through SHA-512 from FIPS 180-4, SHA-3 from FIPS 202, and legacy MD5 (RFC 1321) and SHA-1. The digest is fixed length. It becomes a bucket index only after you reduce it.
- The same page warns that MD5 and SHA-1 have known collision weaknesses. `usedforsecurity=False` marks a call that is not a security use.
- Folding a SHA-256 digest into a finite bucket array still collides. A finite table cannot give every key its own slot.

**When to use:**

Use a fast non-cryptographic mix inside a hash table. Use a cryptographic digest for integrity. Passwords need a slow password hash, not a bare SHA-256.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.python.org/3/library/hashlib.html
</details>

<details>
<summary>What is the role of a Salt in Cryptographic Hashing?</summary>

### What is the role of a Salt in Cryptographic Hashing?

**Definition:**

A salt is a non-secret value mixed into a password hash so the same password does not always produce the same digest.

**How it works:**

- Without a salt, equal passwords share one digest. One precomputed table of common passwords then matches every user.
- A unique salt per password is stored next to the digest. It is not a secret key. The attacker must recompute guesses for that salt.
- hashlib's SHA-256 stays fast even with a salt. The module is a message-digest API. Password storage wants a slow function that also takes a salt.
- A salt does not repair the collision attacks on MD5 and SHA-1. It only stops identical inputs from sharing a digest.

**When to use:**

Salt every stored password. Do not salt a content hash of a file: identical bytes should keep an identical digest.

**Sources:**

https://docs.python.org/3/library/hashlib.html
</details>

<details>
<summary>Explain Types of Resistance in Cryptographic Hash Functions.</summary>

### Explain Types of Resistance in Cryptographic Hash Functions.

**Definition:**

A cryptographic hash is judged by how hard it is to invert, to find a second input with a known input's digest, or to find any colliding pair. hashlib calls out collision attacks on MD5 and SHA-1 and keeps SHA-2 and SHA-3 available.

**How it works:**

- Preimage resistance: given only the digest, finding an input should be infeasible.
- Second-preimage resistance: given one input, finding a different input with the same digest should be infeasible.
- Collision resistance: finding any two different inputs with the same digest should be infeasible, even if the attacker picks both. MD5 and SHA-1 fail this one.
- A hash-table mix function does not claim these properties. It only spreads keys across buckets.

**When to use:**

Demand collision resistance when both parties can choose the bytes, as in a signature. A checksum of random corruption does not need it.

**Sources:**

https://docs.python.org/3/library/hashlib.html
</details>

<details>
<summary>What is MD5?</summary>

### What is MD5?

**Definition:**

MD5 is the 128-bit digest from RFC 1321. hashlib exposes `md5()` and says it has known collision weaknesses. A FIPS-only build may omit or block it.

**How it works:**

- The interface matches the other constructors: `update` the bytes, then `digest()` or `hexdigest()`.
- `usedforsecurity=False` allows MD5 in a restricted build when the call is not a security function.
- The digest is 128 bits. Collision attacks are practical, which is why the docs group MD5 with legacy SHA-1.
- It still appears as a non-security file checksum. It is not a password hash and not a signature hash.

**Examples:**

```text
hashlib.md5(b"abc", usedforsecurity=False).hexdigest()
```

**Sources:**

https://docs.python.org/3/library/hashlib.html
</details>

<details>
<summary>What vulnerabilities exist in Hashing Algorithms like MD5 and SHA1?</summary>

### What vulnerabilities exist in Hashing Algorithms like MD5 and SHA1?

**Definition:**

hashlib states that MD5 and SHA-1 have known collision weaknesses: two different inputs can be built with the same digest. Signatures and certificates that still trust those digests can be forged.

**How it works:**

- The broken property is collision resistance. Chosen-prefix collisions let an attacker attach different headers to one hash.
- SHA-1 is documented as legacy. SHA-224 and longer digests from FIPS 180-4, and SHA-3 from FIPS 202, are the constructors listed without that warning.
- This is not a hash-table collision. Here the attacker chooses the message. A table collision is two keys sharing a bucket.
- `md5()` and `sha1()` take `usedforsecurity`. The default `True` can block them in a FIPS build. `False` says the call is not for security.

**When to use:**

Do not sign, fingerprint a certificate, or store a password with MD5 or SHA-1.

**Sources:**

https://docs.python.org/3/library/hashlib.html
</details>

### Hash Table Usage and Applications

<details>
<summary>Detect if a List is Cyclic using a Hash Table.</summary>

### Detect if a List is Cyclic using a Hash Table.

**Definition:**

Walk the nodes and store each node you have seen. If the next pointer is already in the set, the list has a cycle.

**How it works:**

- Store identity, not the value. Two nodes can hold the same number and still be different objects.
- The set answers whether a cycle exists. Remembering the step index as well tells you where the repeat started.
- Expected time is O(n) lookups. Extra memory is O(n). Two pointers find a cycle in constant memory when the list is huge.
- A set of values, not nodes, false-flags a list that repeats a number without a pointer cycle.

**When to use:**

Use the set when you already keep seen nodes. Use two pointers when memory must stay constant.

**Examples:**

```text
seen = set()
while node:
    if id(node) in seen:
        return True
    seen.add(id(node))
    node = node.next
return False
```

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Remove Duplicates from an Unsorted Linked List using a hash table.</summary>

### Remove Duplicates from an Unsorted Linked List using a hash table.

**Definition:**

Keep a set of values already kept. On one walk, unlink a node whose value is already in the set. Otherwise record the value.

**How it works:**

- The list is unsorted, so comparing only with the previous node is not enough. The set makes the scan one pass.
- Expected time is O(n). Sorting then compressing duplicates is O(n log n) and reorders the list.
- Equality is by value. Identity would keep two nodes that store the same number.
- The first occurrence stays, so the order of the unique values does not change.

**Examples:**

```text
seen = set()
prev = None
node = head
while node:
    if node.val in seen:
        prev.next = node.next
    else:
        seen.add(node.val)
        prev = node
    node = node.next
```

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Find Common Elements in two given Linked Lists and return them as a new Linked List using a hash table.</summary>

### Find Common Elements in two given Linked Lists and return them as a new Linked List using a hash table.

**Definition:**

Insert every value from the first list into a set. Walk the second list and append a new node whenever the value is in the set.

**How it works:**

- The result follows the order of the second list. Building the set is one pass.
- A set emits each value once. A counter map emits the smaller count when duplicates matter.
- Expected time is O(n + m). Pairwise comparison is O(n·m).
- Hash the values. Hashing node objects would ask for shared identity, not a shared number.

**When to use:**

Use a set for distinct values. Use counters when multiplicity matters.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Pair socks from a pile using a Hash Table.</summary>

### Pair socks from a pile using a Hash Table.

**Definition:**

A sock finds its pair when that color is already waiting. A map from color to one unmatched sock does the lookup in expected constant time.

**How it works:**

- One pass: if the color is present, emit the pair and drop the entry. Otherwise store the sock.
- Leftovers stay in the map, at most one per color when only identical colors pair.
- This is a frequency map. Chaining or open addressing both supply get and put.
- Sorting the pile and scanning neighbors avoids the extra memory.

**Examples:**

```text
waiting = {}
for sock in pile:
    if sock.color in waiting:
        emit(waiting.pop(sock.color), sock)
    else:
        waiting[sock.color] = sock
```

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>How can hash tables be used in caching implementations? Describe a typical use case.</summary>

### How can hash tables be used in caching implementations? Describe a typical use case.

**Definition:**

A cache maps a request key to a stored value so the next lookup does not recompute it. A hash table is the usual index because get and put are expected constant time.

**How it works:**

- The key identifies the request. The value is the response or a handle to it.
- The table does not choose the eviction victim. An LRU cache keeps a recency order beside the map. Java documents `LinkedHashMap` as the ordered subclass of `HashMap`.
- `HashMap` uses a default load factor of 0.75 and rehashes to about twice as many buckets when the entry count exceeds load factor times capacity.
- One process uses an ordinary map. Spreading keys across a changing set of cache servers is consistent hashing, which moves about keys/slots on a membership change.

**When to use:**

Use one in-process map for an LRU cache. Use consistent hashing when the cache is a cluster whose servers appear and disappear.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
https://en.wikipedia.org/wiki/Consistent_hashing
</details>

<details>
<summary>Write an algorithm using a hash table to check if two strings are anagrams of each other.</summary>

### Write an algorithm using a hash table to check if two strings are anagrams of each other.

**Definition:**

Two strings are anagrams when every character has the same count. Count the first string in a map and subtract the second. Every count must finish at zero.

**How it works:**

- Lengths must match, or a leftover character is missed when you only look at keys that appeared.
- Expected time is O(n). Sorting both strings and comparing them is O(n log n) and needs no table.
- Case and spaces stay as they are unless the problem says to fold them.
- A fixed array replaces the map when the alphabet is small and known. The map is the version for a large character set.

**Examples:**

```text
from collections import Counter
def anagrams(a, b):
    return Counter(a) == Counter(b)
```

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Implement Autocomplete functionality using Hash Tables.</summary>

### Implement Autocomplete functionality using Hash Tables.

**Definition:**

Map each prefix to the words that start with it. A completion query is one lookup. A trie answers the same query by walking shared prefix nodes, which is what the trie article describes for completion lists.

**How it works:**

- On insert, add the word under every prefix. Lookup is then the list stored at that prefix.
- The word is repeated once per prefix unless the bucket stores an id into one word array. A trie shares the prefix nodes instead.
- A hash set of whole words cannot list completions without scanning every key.
- Sort or trim the bucket by frequency when the menu should stay short.

**When to use:**

Use the prefix map for a small vocabulary and one hash lookup. Use a trie when many words share prefixes and should share nodes.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://en.wikipedia.org/wiki/Trie
</details>

<details>
<summary>Describe how a hash table can be used in the implementation of a CDN.</summary>

### Describe how a hash table can be used in the implementation of a CDN.

**Definition:**

A CDN assigns each content key to a cache server. Consistent hashing maps both keys and servers onto a circle and gives the key to the next server clockwise, so adding or removing a server remaps about keys/slots items instead of almost every key.

**How it works:**

- A plain `hash(key) % N` changes N when a server joins or dies, and nearly every object moves.
- On the circle, a failed server's keys move only to the next server clockwise. A new server takes only the keys that now fall before it.
- Virtual nodes (several labels per server) keep one departure from dumping its whole share onto a single neighbor.
- The consistent-hashing article names this use for content delivery, including Akamai's cluster balancing.

**When to use:**

Use consistent hashing for the server ring. Use an ordinary hash map inside one cache server to store the objects that ring assigned to it.

**Sources:**

https://en.wikipedia.org/wiki/Consistent_hashing
</details>

### Hash Table Structures and Variants

<details>
<summary>What is the difference between HashMaps and HashTables in context of Java’s data structures?</summary>

### What is the difference between HashMaps and HashTables in context of Java’s data structures?

**Definition:**

Java's `HashMap` is a hash table that implements `Map`. The class documentation says it is roughly equivalent to `Hashtable`, except that it is unsynchronized and permits nulls.

**How it works:**

- `HashMap` allows one null key and null values. `Hashtable` does not.
- `HashMap` does not synchronize its methods. Callers share it only with external locking, or they use a concurrent map. `Hashtable` synchronizes operations.
- Both are bucket tables. `HashMap` documents expected constant-time `get` and `put` when the hash spreads keys, and iteration costs capacity plus size.
- The default load factor is 0.75. Crossing load factor times capacity rehashes the table to about twice the buckets. `LinkedHashMap` is the ordered subclass.

**When to use:**

Use `HashMap` for a single-threaded or externally guarded map, including a null key. Use a concurrent map when many threads mutate. Treat `Hashtable` as the older synchronized class.

**Sources:**

https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>

<details>
<summary>Explain Cuckoo Hashing and when it is advantageous to use.</summary>

### Explain Cuckoo Hashing and when it is advantageous to use.

**Definition:**

Cuckoo hashing gives each key two possible locations, from two hash functions. Lookup inspects those two cells and is worst-case constant time. Insert may kick the resident key to its other location.

**How it works:**

- A common layout is two tables of equal size, one hash into each. Lookup is true when either cell holds the key.
- Insert writes into an empty cell or swaps the occupant out and continues. A cycle, or a loop past a fixed threshold, rebuilds both tables with new hash functions.
- Insert stays expected constant time while the load factor stays below one half. Above one half the cuckoo graph tends to grow a component that will not place every key.
- The article notes lookup is often 20–30% slower than linear probing because it touches two places and can miss cache twice. The payoff is the worst-case bound, which matters for real-time lookup. Three hashes or a small stash raise the usable load.

**When to use:**

Use it when lookup must be worst-case constant and a load near one half is acceptable. Prefer linear probing when one cache miss and higher load matter more than the worst-case bound.

**Sources:**

https://en.wikipedia.org/wiki/Cuckoo_hashing
</details>

<details>
<summary>What are Skip Lists and how do they compare to hash tables?</summary>

### What are Skip Lists and how do they compare to hash tables?

**Definition:**

A skip list is an ordered linked list plus sparser express layers. Each node is promoted with a fixed probability, and search, insert, and delete are O(log n) on average. A hash table is unordered and aims at expected O(1) lookup.

**How it works:**

- Search starts at the top layer and drops down when the next node would pass the target. With p = 1/2 the expected cost is logarithmic.
- The Wikipedia table lists worst-case search, insert, and delete as O(n). A badly unbalanced run of coin flips is possible, though unlikely. A balanced tree promises O(log n) in the worst case.
- A hash table does not walk keys in order and does not answer range or successor queries. A skip list does, because the bottom layer is sorted.
- Redis ordered sets and Java's `ConcurrentSkipListMap` are named uses. Parallel inserts in different regions do not need a global rebalance.

**When to use:**

Use a skip list or a tree when you need order, ranges, or a successor. Use a hash table when you only test membership or fetch one key and can accept expected, not worst-case, constant time.

**Sources:**

https://en.wikipedia.org/wiki/Skip_list
https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Discuss the concept of a Dynamic Perfect Hash Table.</summary>

### Discuss the concept of a Dynamic Perfect Hash Table.

**Definition:**

Dynamic perfect hashing keeps worst-case constant-time lookup while insertions and deletions stay expected amortized constant time. It is the dynamic form of the two-level FKS scheme.

**How it works:**

- Level one hashes a key into a bucket. Level two is a small table whose hash is chosen to be injective on the keys in that bucket, so lookup does not probe.
- In the static FKS scheme the second-level tables are quadratic in the bucket size, and the first hash is chosen so total space stays linear in expectation.
- Dietzfelbinger and coauthors rebuild a bucket's second table when an insert collides, and rebuild the whole table after enough updates. Lookup stays worst-case constant. Insert and delete are expected amortized constant, and space stays linear.
- The article notes the structure uses more memory than an ordinary hash table. That is the price of the worst-case lookup.

**When to use:**

Use it when queries must be worst-case O(1) on a set that still changes. An ordinary table is simpler when expected O(1) is enough.

**Sources:**

https://en.wikipedia.org/wiki/Dynamic_perfect_hashing
</details>

<details>
<summary>In what scenarios would you use a Bloom Filter, and how does it relate to hash tables?</summary>

### In what scenarios would you use a Bloom Filter, and how does it relate to hash tables?

**Definition:**

A Bloom filter is a bit array plus several hash functions. Insert sets k bits. Lookup says "definitely absent" or "maybe present". It can false-positive and does not false-negative.

**How it works:**

- It does not store the key. That is the difference from a hash table, which stores the key (and usually a value) and can answer exactly.
- k independent hashes spread one key across k bits. A later lookup reads those bits. Any zero bit means the key was never inserted.
- A classic Bloom filter does not delete: clearing a bit can erase a different key that shared it. Counting filters and cuckoo filters change that trade.
- Use it as a front door in front of a real table or a disk lookup: most absent keys die on the bits, and only a maybe pays for the exact structure.

**When to use:**

Use a Bloom filter for a huge set when some false "present" answers are acceptable and you must not say "absent" by mistake. Use a hash table when the answer must be exact or you must recover the value.

**Sources:**

https://en.wikipedia.org/wiki/Bloom_filter
https://en.wikipedia.org/wiki/Hash_table
</details>

### Advanced Hash Table Questions

<details>
<summary>What is the Pigeonhole Principle and how does it relate to hashing?</summary>

### What is the Pigeonhole Principle and how does it relate to hashing?

**Definition:**

The pigeonhole principle says that if more items are placed into fewer holes, at least one hole holds more than one item. A hash table has fewer buckets than possible keys, so some distinct keys must share a bucket.

**How it works:**

- Collision handling is mandatory, not a bug in one hash function. Separate chaining stores the sharers in a list. Open addressing walks a probe sequence for another slot.
- A cryptographic digest does not escape this once you reduce it modulo the bucket count. Two different digests can still land on one index.
- Perfect hashing avoids collisions only for a chosen finite set, by picking the hash after the set is known and spending extra space. It does not hash every possible string into its own slot.
- Load factor n/m is how full those holes are. The hash-table article says a lower load factor generally makes operations faster.

**Sources:**

https://en.wikipedia.org/wiki/Pigeonhole_principle
https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>What are the advantages of Tries over Hash Tables?</summary>

### What are the advantages of Tries over Hash Tables?

**Definition:**

A trie stores a key along a path of characters, so words that share a prefix share nodes. Search is O(length) in the worst case. A hash table hashes the whole key and does not group prefixes.

**How it works:**

- Autocomplete and longest-common-prefix fall out of one subtree. A hash set of whole words must scan every key to list completions.
- The trie article lists search, insert, and delete as O(n) in the key length, average and worst case. There is no collision chain whose length depends on how many other keys hashed the same way.
- Failed lookups stop at the first missing edge.
- The cost is memory. An uncompressed trie can spend a node per character and a wide fan of empty child slots. A hash table is usually tighter for exact lookup of unrelated keys.

**When to use:**

Prefer a trie for prefixes, completions, and a bound that depends on key length. Prefer a hash table for exact membership of keys that do not share structure.

**Sources:**

https://en.wikipedia.org/wiki/Trie
https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>Explain Robin Hood Hashing and its impact on performance and collision resolution.</summary>

### Explain Robin Hood Hashing and its impact on performance and collision resolution.

**Definition:**

Robin Hood hashing is open addressing that, on a collision, keeps the key with the longer probe distance in the slot and moves the other one onward.

**How it works:**

- Each key has a home bucket. The probe sequence length is how far insertion walked from that home. The displaced key is the one closer to home.
- The hash-table article describes this as favouring the element farthest from its home location. The name is the idea of taking from the key that has traveled less.
- Variance of probe lengths shrinks. Lookups that would have walked a very long chain get shorter, at the cost of moving keys that were previously close to home.
- It is still open addressing: the key lives in the table array, not in a side list. Deletion still has to leave a marker or re-probe, or later keys become unreachable.

**When to use:**

Use it to cap the long tail of linear-probing searches when the table is fairly full. It does not remove collisions. It redistributes how far each key sits from home.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
</details>

<details>
<summary>What is Linear Hashing and how can it be used to handle growing datasets?</summary>

### What is Linear Hashing and how can it be used to handle growing datasets?

**Definition:**

Linear hashing grows or shrinks a hash table one bucket at a time. The hash-table article defines it that way, as opposed to doubling the whole directory in one rehash.

**How it works:**

- A split pointer walks the buckets. When the load says the table should grow, only the bucket under the pointer is split into two, and the pointer advances.
- Keys in the unsplit part keep their old bucket. Only the split bucket's keys are rehashed. Growth cost is spread across inserts.
- The consistent-hashing article contrasts this with its own scheme: linear hashing handles adding and removing buckets in sequence, while consistent hashing lets servers join and leave in any order.
- Inside one process this keeps memory growth smooth. Across machines, sequential split is the wrong tool if any server may disappear.

**When to use:**

Use linear hashing for a single growing table that should not stop to rehash every bucket at once. Use consistent hashing when the buckets are servers that appear and disappear arbitrarily.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://en.wikipedia.org/wiki/Consistent_hashing
</details>

<details>
<summary>How are hash tables applied in distributed systems, for example, in consistent hashing algorithms?</summary>

### How are hash tables applied in distributed systems, for example, in consistent hashing algorithms?

**Definition:**

Consistent hashing is the distributed form of "which bucket holds this key" when the buckets are machines and the set of machines changes. Keys and servers are hashed onto a circle. The key lives on the next server clockwise.

**How it works:**

- Adding or removing one server remaps about keys/slots keys on average, not almost all of them. The article states that contrast with a modular hash.
- Lookup of the successor is O(log N) with a binary search over server positions. That is slower than one array index, and it is the cost of a stable assignment.
- Virtual nodes spread one server around the circle so load and failover do not pile onto one neighbor.
- The article lists this as the basis of distributed hash tables and names Dynamo, Cassandra, and CDN clusters among users. The table inside one server is still an ordinary hash map.

**When to use:**

Use the ring to choose the owner. Use a local hash table on that owner to store the record.

**Sources:**

https://en.wikipedia.org/wiki/Consistent_hashing
</details>

<details>
<summary>Describe a situation where you would use a hash table over a binary search tree, and why.</summary>

### Describe a situation where you would use a hash table over a binary search tree, and why.

**Definition:**

Use a hash table when the only operations are insert, delete, and fetch by exact key, and key order does not matter. A well-spread table does those in expected time independent of how many keys are stored.

**How it works:**

- The hash-table article says average lookup is independent of the number of elements in a well-dimensioned table. A binary search tree is O(log n) per operation even when balanced.
- A tree answers successor, predecessor, and range. A hash table does not. If the product never asks those, the tree's ordering work is wasted.
- `HashMap` documents constant-time `get` and `put` when the hash disperses keys, with a 0.75 load factor before rehash. Worst-case chaining can still degrade if every key hits one bucket.
- Java's `HashMap` also allows a null key. A search tree needs a comparison order. Keys that are not ordered still fit a hash table if they have a hash and an equality test.

**When to use:**

Pick the hash table for exact-key dictionaries, sets, joins, and caches. Pick a tree or a skip list when the caller walks keys in order or asks for a range.

**Sources:**

https://en.wikipedia.org/wiki/Hash_table
https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html
</details>
