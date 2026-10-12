# Caching — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Caching — Interview](./0_sd_caching_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Caching](https://devinterview.io/questions/software-architecture-and-system-design/caching-interview-questions/).

### Caching Fundamentals

<details>
<summary>Define caching in the context of computer programming.</summary>

### Define caching in the context of computer programming.

**Definition:**

A cache stores a copy of data that was expensive to fetch or compute, so the next request can use the copy.

**How it works:**

- The copy sits closer to the reader: CPU cache, process memory, Redis, or a CDN.
- It cuts repeated work and takes load off the origin, at the cost of serving a value that may be older than the source.
- It pays off for hot data, slow computations, and spikes the origin cannot absorb.
- It is the wrong tool when every read must see the latest write.

**Examples:**

```text
miss:  read database, store under key "product:42", return row
hit:   return the stored row, skip the database
```
</details>

<details>
<summary>What are the main purposes of using a cache in a software application?</summary>

### What are the main purposes of using a cache in a software application?

**Definition:**

A cache exists to make repeated work cheap and to keep the system answering when the origin is slow or briefly down.

**How it works:**

- Latency drops because memory is faster than a query or a remote call.
- The origin sees fewer reads, so a spike can be served from the copy.
- A replicated cache can still answer if one cache node dies. That is resilience of the copy, not of the source of truth.
- Expiration and invalidation are how you limit how stale the copy may be.
- Memoizing a pure function, such as a Fibonacci step already computed, is the same idea inside one process.

**Examples:**

```text
fib(n): if n in cache return it
        else compute, store, return
```
</details>

<details>
<summary>Can you explain the concept of cache hit and cache miss?</summary>

### Can you explain the concept of cache hit and cache miss?

**Definition:**

A hit is a lookup that finds a usable copy. A miss is a lookup that does not, so the caller pays the full cost and usually stores the result.

**How it works:**

- A compulsory miss is the first use: the cache is empty for that key.
- A capacity miss happens because the cache was full and an eviction removed a key you needed again.
- A conflict miss happens when two keys land in the same slot and knock each other out, as in a direct-mapped CPU cache.
- A coherence miss happens when another writer invalidated the line you held.
- The hit rate is hits divided by lookups. Raising it is the point of sizing and of the eviction policy.

**Examples:**

Ten lookups, seven served from memory: hit rate 70%. The other three queried the database and filled the cache.
</details>

<details>
<summary>Describe the impact of cache size on performance.</summary>

### Describe the impact of cache size on performance.

**Definition:**

A larger cache holds more keys, so fewer requests miss, until the cache is so large that it costs more than the misses it prevents.

**How it works:**

- Too small: evictions thrash, and the hit rate stays low.
- Large enough for the working set: most hot keys stay, and the origin sees a fraction of the traffic.
- Bigger than the working set: extra memory sits idle. On a small device that memory was needed elsewhere.
- A huge cache is also a huge stale surface. Invalidation and expiration have more keys to get right.
- The eviction policy decides which extra slot helps. Size without a sane policy still throws away the hot key.

**Examples:**

A catalog’s hot 1,000 products fit in memory and the database QPS collapses. Caching all 20 million cold SKUs mostly stores keys nobody asks for.
</details>

<details>
<summary>How does a cache improve data retrieval times?</summary>

### How does a cache improve data retrieval times?

**Definition:**

A cache improves retrieval by answering from a faster tier and by not repeating the slow work.

**How it works:**

- Memory and a local disk beat a round trip to a database or another region.
- A browser cache skips downloading CSS and images that have not changed.
- An in-memory store such as Redis or Memcached serves the hot keys for many app instances.
- A distributed cache spreads those keys across nodes so one machine’s RAM is not the ceiling.
- Fewer origin reads also mean the origin spends less time queuing, so the misses that remain are faster too.
</details>

<details>
<summary>What is the difference between local caching and distributed caching?</summary>

### What is the difference between local caching and distributed caching?

**Definition:**

A local cache lives inside one process and is invisible to the others. A distributed cache is a shared service that many instances read and write.

**How it works:**

- Local has no network hop and is trivial to keep coherent inside that process. Each instance can hold a different value.
- Distributed pays a network hop and can serve the same key from every app node. Copies across cache nodes are often eventually consistent.
- Use local when the data is private to the process, tiny, or latency inside one box dominates.
- Use distributed when many instances must see one value, when the working set is larger than one process, or when a restarted process should not start cold.

**Examples:**

Each web process caches a user’s permissions in a map: fast, and stale until that process restarts. A shared Redis key `perm:7` is what every process sees after an update, once the write reaches Redis.
</details>

<details>
<summary>Explain the concept of cache eviction and mention common strategies.</summary>

### Explain the concept of cache eviction and mention common strategies.

**Definition:**

Eviction is how a full cache chooses a key to drop so a new key can enter.

**How it works:**

- Least recently used drops the key nobody has touched for the longest time. It needs a recency order.
- Least frequently used drops the key with the smallest hit count. The counter is extra work and can stick on a key that was hot once.
- First in, first out drops the oldest resident regardless of use.
- Random is cheap and surprisingly decent when you cannot afford bookkeeping.
- A time-to-live drop is expiration, not pressure from a full cache, and it is often combined with one of the above.

**Examples:**

A map plus a doubly linked list implements LRU: a hit moves the node to the head, and a full insert evicts the tail.
</details>

<details>
<summary>What is a cache key and how is it used?</summary>

### What is a cache key and how is it used?

**Definition:**

A cache key is the identity of a stored value. The same inputs must produce the same key, and different inputs must not collide.

**How it works:**

- A key is often a name plus the arguments: `product:42` or a hash of the query.
- If you forget a varying argument, two users share a value. If you include a useless one, you never hit.
- Normalize order and case before hashing, or `b=1&a=2` and `a=2&b=1` miss each other.
- Do not put secrets in a key that will be logged.

**Examples:**

```text
key = "catalog:" + region + ":" + sku
GET catalog:eu:lamp
```
Leaving `region` out serves the EU price to a US request.
</details>

<details>
<summary>Explain the importance of cache expiration and how it is managed.</summary>

### Explain the importance of cache expiration and how it is managed.

**Definition:**

Expiration bounds how long a copy may be served after the source has changed, even if nobody remembers to invalidate it.

**How it works:**

- A TTL drops the key after a duration from write or from last access.
- A short TTL keeps data fresher and causes more misses. A long TTL does the opposite.
- Expiration is a safety net. Explicit invalidation on write is how you drop a key the moment it is wrong.
- Serve-stale-while-revalidate can return the old value once and refresh in the background, so a miss storm does not hit the origin.

**Examples:**

`SET price:lamp 40 EX 60` means that after a minute the next reader misses and loads the database, whether or not the price changed.
</details>

<details>
<summary>How does cache invalidation work and why is it necessary?</summary>

### How does cache invalidation work and why is it necessary?

**Definition:**

Invalidation removes or replaces a cached value because the source changed. Without it, readers keep a copy that is no longer true until the TTL happens to expire.

**How it works:**

- On a write, delete the key or overwrite it. The next read fills a fresh copy. That is cache-aside invalidation.
- Write-through updates the cache and the database in the same request, so a later read hits the new value.
- Write-behind updates the cache and flushes to the database later. The cache is ahead of the source, and a crash can lose the write.
- In a distributed cache every node that might hold the key has to drop it, or you accept a window of disagreement.
- The hard parts are knowing every key a write affects, and doing that under concurrent updates.

**Examples:**

Updating the lamp price deletes `price:lamp`. The next product page misses, reads 42 from the database, and stores 42. A page that still held 40 would have been wrong.
</details>

### Cache Implementation and Design

<details>
<summary>Describe the steps involved in implementing a basic cache system.</summary>

### Describe the steps involved in implementing a basic cache system.

**Definition:**

A basic cache is a map from key to value, a way to bound its size, and a rule for what happens on a miss.

**How it works:**

- Look up the key. On a hit, return the value and update whatever the eviction policy tracks.
- On a miss, load from the source, store the value, and return it.
- If the map is full, evict one entry first. LRU is the usual default.
- Give each entry a TTL so a forgotten key cannot live forever.
- On a source write, delete the key. Do not let the cache be the only place the new value exists unless you have chosen write-through.

**Examples:**

```text
get(key):
  if key in map and not expired: return map[key]
  value = load(key)
  if map is full: evict least recently used
  map[key] = value
  return value
```
</details>

<details>
<summary>How would you handle cache synchronization in a distributed environment?</summary>

### How would you handle cache synchronization in a distributed environment?

**Definition:**

Synchronization means a write that changes the source also changes, or drops, every cached copy that could still be served.

**How it works:**

- The simplest approach is a short TTL and no active sync. Copies lag, and they heal by expiring.
- Explicit invalidation publishes “drop this key” on a bus, and every app node deletes it from its local cache.
- A shared cache such as Redis is one copy. App nodes do not each keep their own, so there is nothing to fan out except Redis’s own replicas.
- Write-through updates the shared cache as part of the write. Write-behind risks losing updates if the cache dies before the flush.
- Version the value. A reader that sees an older version refetches. That tolerates a missed invalidation message.

**Examples:**

After a price change, the writer deletes the Redis key and publishes `invalidate price:lamp`. Each server drops its local copy. A server that misses the message still expires the local entry after 30 seconds.
</details>

<details>
<summary>Explain the use of hash maps in cache implementation.</summary>

### Explain the use of hash maps in cache implementation.

**Definition:**

A hash map is the usual structure for a cache because the average lookup, insert, and delete are constant time.

**How it works:**

- The key is hashed to a bucket, and the value sits there with its metadata: expiry, hit count, or a pointer into an LRU list.
- The map answers “is this key present?”. It does not by itself know which key is oldest. That is why LRU pairs the map with a linked list.
- Collisions mean two keys share a bucket. The implementation chains them or probes onward. A bad hash makes that a scan.
- Resizing the table costs a pause or incremental work. Bound the entry count so resize is rare.

**Examples:**

```text
buckets[hash("price:lamp") % n] -> {value: 40, expires: t}
get: hash, find node, return value
```
</details>

<details>
<summary>What are some common caching algorithms, and how do they differ?</summary>

### What are some common caching algorithms, and how do they differ?

**Definition:**

The common algorithms differ in which resident they evict when the cache is full, and in how much bookkeeping that choice costs.

**How it works:**

- LRU evicts the least recently used. It tracks recency and fits workloads where the recent past predicts the next read.
- LFU evicts the least frequently used. It fits a stable set of hot keys and can cling to a key that was popular and then went cold.
- FIFO evicts the oldest insertion and ignores later hits. It is simple and weaker when a key is hot.
- MRU evicts the most recently used. It is a niche fit for a scan that will not reuse what it just touched.
- Random evicts any key. It needs almost no metadata.
- TTL is orthogonal: it removes a key because it is old in time, whether or not the cache is full.
</details>

<details>
<summary>Explain the design considerations for a cache that supports high concurrency.</summary>

### Explain the design considerations for a cache that supports high concurrency.

**Definition:**

A concurrent cache must let many readers and writers proceed without turning the map into one global lock.

**How it works:**

- Shard the map by key hash so two keys rarely share a lock. A single mutex around the whole cache serializes every request.
- Prefer concurrent reads. A read-write lock or an immutable snapshot per shard keeps hits off the write path.
- Make the hit-path update of an LRU list cheap, or a popular key’s “move to head” becomes the bottleneck. Sometimes a sampled LRU is enough.
- Atomic put-if-absent stops two misses from both loading the origin and overwriting each other.
- Decide staleness up front. Write-through is fresher and slower. A short TTL with invalidation is usually enough and avoids a distributed transaction.
- Bound memory per shard so one hot shard cannot grow without limit.

**Examples:**

```text
shard = hash(key) % 32
lock only shard
on miss: single-flight the load so 100 waiters share one origin read
```
</details>

<details>
<summary>How would you prevent cache stampede in a high-load application?</summary>

### How would you prevent cache stampede in a high-load application?

**Definition:**

A stampede is many callers missing the same key and all loading the origin. One caller must do the load. The others wait or serve a stale value.

**How it works:**

- Redis documents a lock as a key with a TTL, so a crashed holder does not keep the lock forever. Mutual exclusion means only one client holds it. The TTL is what makes the lock deadlock-free if the holder dies.
- On a miss, try to take that lock, load the origin once, write the cache, and delete the lock. Everyone else waits briefly and reads the cache again.
- A single Redis instance is a single place this can fail. Replication does not fix it: replication is asynchronous, so a master can die before the lock write reaches a replica, and a second client can also acquire it. Redlock is the documented attempt to get a majority across independent instances. It still needs the safety properties: one holder, eventual release, and a majority up.
- Do not hold the lock without a TTL. That fails the liveness property Redis states: a dead client must not own the key forever.
- A short TTL on the cached value still limits how long a missed invalidation lasts, but the TTL alone does not stop the herd at the moment of expiry. The lock does.

**When to use:**

Single-flight with a TTL lock when one cache serves many app nodes. A process-local lock is enough when only one process can miss.

**Examples:**

```text
miss on price:lamp
SET lock:price:lamp token NX PX 5000
winner: load origin, SET price:lamp, DEL lock if token matches
others: wait, GET price:lamp
```

**Sources:**

- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>What are the trade-offs between read-heavy and write-heavy caching strategies?</summary>

### What are the trade-offs between read-heavy and write-heavy caching strategies?

**Definition:**

A cache pays off when the same stored response is reused. A read-heavy key is reused. A write-heavy key changes before the copy is worth keeping.

**How it works:**

- HTTP caching stores a response and reuses it so the origin does not parse, query, and render again. That win is a hit on a later read.
- Redis treats cache entries as copies of persistently stored data, so they can be evicted and loaded again. `allkeys-lru` is the documented default when a subset of keys is read far more often than the rest. `allkeys-lrm` keeps data that is still being modified and is aimed at read-heavy workloads where you want to tell updates apart from reads.
- A write that changes the origin makes the stored copy stale. HTTP has no way to delete a response that an intermediate cache stored with a long `max-age`. The server loses control of that URL until the age expires.
- `Cache-Control: no-cache` forces validation before reuse, so a frequently updated resource still hits the origin. `no-store` refuses storage, and it also drops benefits such as the back/forward cache. MDN prefers `no-cache` with `private` over liberal `no-store`.
- Evicting on memory pressure is safe only because the copy can be rebuilt. If every write must be durable, Redis says use `noeviction` or a separate instance, and do not treat the cache as the only copy.

**When to use:**

Cache reads that repeat. Validate or skip the cache for data that changes on every write.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
</details>

<details>
<summary>Describe the role of a cache manifest in web applications.</summary>

### Describe the role of a cache manifest in web applications.

**Definition:**

A cache manifest was an earlier offline list. MDN points at the Service Worker Cache API instead: you store Request/Response pairs yourself and delete old named caches on activate.

**How it works:**

- The Service Worker API page contrasts service workers with earlier attempts such as AppCache. A service worker does not assume what you meant to cache. You decide.
- On `install`, the usual step is to open a named cache and put the assets you need offline into it. `Cache.add` fetches a URL and stores the response. `addAll` does that for a list.
- The Cache API does not honor HTTP caching headers. Entries do not expire unless your script deletes them. `Cache.delete` removes one entry. `caches.delete` removes a named cache.
- On update, keep the new cache name (the MDN example moves from `v1` to `v2`) and delete the old names in `activate`, after previous workers are gone.
- `Set-Cookie` is stripped from a response stored this way, so the cache does not set cookies later.

**Examples:**

```js
const cache = await caches.open("v2");
await cache.addAll(["/index.html", "/app.js"]);
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
</details>

<details>
<summary>How do you ensure consistency between cache and primary data storage?</summary>

### How do you ensure consistency between cache and primary data storage?

**Definition:**

The cache holds a copy. Consistency means either the copy is still fresh, or the client checks the origin before reusing it.

**How it works:**

- HTTP freshness is age against `max-age`. A fresh response is reused. A stale one can be revalidated with `If-None-Match` or `If-Modified-Since`. A matching validator returns `304 Not Modified`. RFC 9110, as MDN cites it, prefers both `ETag` and `Last-Modified` on a `200`. If both validators are sent, `If-None-Match` wins.
- `Cache-Control: no-cache` stores the response but refuses reuse without validation. `no-store` skips storing this response and does not delete an older stored one for the same URL.
- Redis keys expire with a TTL, or they are evicted under `maxmemory`. `INFO` reports `expired_keys` and `evicted_keys`. A short TTL bounds how long a stale copy lives. It does not push the new value out by itself.
- While a partition exists, Brewer's later statement is that you choose availability or a single up-to-date copy. Asynchronous copies trade staleness for latency. Redis replication is asynchronous, so a write can be lost if the primary dies before the replica has it.
- Personalized data uses `Cache-Control: private` so a shared cache does not serve one user's copy to another.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Explain the use of cache tagging and its benefits.</summary>

### Explain the use of cache tagging and its benefits.

**Definition:**

HTTP does not define a tag you can purge by. A managed cache can still delete a stored response through its own dashboard or API, which is the practical stand-in for a tag.

**How it works:**

- MDN states that the HTTP caching specification essentially does not define a way to explicitly delete a cache. A long `max-age` means the origin may see no further requests for that URL.
- Managed caches (reverse proxies, CDNs, service workers with the Cache API) can delete a stored response through a dashboard, an API, or a restart. That is the benefit: you do not wait for `max-age`.
- Some CDNs add their own headers, for example `Surrogate-Control`. MDN notes work toward a standard `CDN-Cache-Control` header. Those headers are product controls, not a tag vocabulary in the HTTP spec.
- In the Cache API you delete by request (`Cache.delete`) or by cache name (`caches.delete` on activate). The name is the group.
- Do not put `User-Agent` in `Vary` to imitate tags. MDN says that header has so many values that reuse collapses.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
</details>

### Caching in Distributed Systems

<details>
<summary>Explain distributed caching and its advantages over local caching.</summary>

### Explain distributed caching and its advantages over local caching.

**Definition:**

A private cache belongs to one client. A shared or managed cache sits between many clients and the origin and reuses one stored response for all of them.

**How it works:**

- MDN's private cache is tied to a specific client, usually the browser. It may store a personalized response. A shared cache stores responses that can be shared among users: proxy caches and managed caches.
- The closer that shared cache is to the clients, the faster the reuse, and the origin skips the work. Request collapse forwards one of many identical in-flight requests.
- A process-local map dies with the process and is invisible to the next server. A Redis instance is a cache many application processes can read. Hits and misses show up in `INFO` as `keyspace_hits` and `keyspace_misses`.
- The cost is coherence and failure. Redis replication is asynchronous. One Redis plus a replica is not a quorum. Personalized responses still need `Cache-Control: private` so they never enter the shared cache.
- Local memory is the lowest latency. Use it for data that only that process needs. Use the shared store when many nodes must see the same copy.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>How do you handle cache partitioning in distributed systems?</summary>

### How do you handle cache partitioning in distributed systems?

**Definition:**

Split the key space so each cache node holds a subset, and keep the memory limit and eviction policy local to that node.

**How it works:**

- Redis checks memory on each command that adds data. When usage passes `maxmemory`, it evicts according to `maxmemory-policy` until usage is back under the limit. A big write can overshoot first.
- `allkeys-lru` fits a hot subset. `allkeys-random` fits keys that are all touched about equally, such as a repeating cycle. `volatile-ttl` evicts expiring keys with the shortest TTL left, if your code can mark the disposable ones.
- `volatile-*` policies act like `noeviction` when no key has a TTL. Redis suggests two instances when you mix cache keys and keys that must not be evicted, instead of one policy trying to do both.
- Partitioning is how you add a second memory budget: keys that hash to different nodes never compete for the same `maxmemory`. The application has to send the key to the node that owns it.
- A partition of the network, in the CAP sense, is a different word. During that partition you cannot treat every node as holding the single up-to-date copy.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Describe consistency models in distributed caching (e.g., eventual consistency).</summary>

### Describe consistency models in distributed caching (e.g., eventual consistency).

**Definition:**

A cache is already a copy, so it is stale until refreshed. Eventual consistency means asynchronous copies catch up later. A partition forces a choice between that staleness and refusing updates.

**How it works:**

- Brewer's restatement: during a partition, choose consistency or availability. Consistency here means a single up-to-date copy. Availability means that data accepts updates. Partition means continuing when messages are lost. A practical partition is a timeout.
- Asynchronous remote copies trade staleness for latency. That is the eventual model: readers can see an older copy until the copy arrives.
- Redis replication is asynchronous. The lock documentation says a primary can die before the lock write reaches a replica, and another client can acquire it. Do not describe one primary plus a replica as a majority.
- HTTP freshness is a time-based version of the same idea. Inside `max-age` the stored response is reused even if the origin has a newer byte. `no-cache` plus `ETag` turns the next use into a check. `304` keeps the old body when the validator matches.
- Redlock tries a majority across independent Redis instances. That is the stronger side: the side that cannot get a majority does not pretend it holds the lock.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>What is cache replication and how is it typically implemented?</summary>

### What is cache replication and how is it typically implemented?

**Definition:**

Replication copies cache entries to another node so a reader can use that node. In Redis that copy is asynchronous, so it can lag or be missing after a crash.

**How it works:**

- The distributed-lock page states that replication is asynchronous. A primary can fail before the write is on a replica. A second client can then take a lock the first client still thinks it holds.
- Redlock is the documented attempt to require a majority of independent instances, with a TTL so a dead holder releases the key. One primary and its replica are not that majority.
- HTTP shared caches are not replicas of each other by the spec. Each cache stores responses it has seen. Request collapse shares one origin fetch among concurrent clients of the same cache.
- Spark's replicated storage levels (`MEMORY_ONLY_2` and the other `_2` levels) keep each partition on two nodes so a lost node does not force a recompute before tasks continue. Every storage level can recompute a lost partition. Replication only avoids the wait.
- Eviction buffers for Redis replication are excluded from the `maxmemory` comparison. Count them via `INFO` field `mem_not_counted_for_evict`, and leave RAM for them when you set `maxmemory`.

**Sources:**

- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://redis.io/docs/latest/develop/reference/eviction/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Name some strategies to avoid cache coherence issues in distributed systems.</summary>

### Name some strategies to avoid cache coherence issues in distributed systems.

**Definition:**

Stop a shared cache from storing a private response, key variants separately, and bound how long a copy is reused without asking the origin.

**How it works:**

- `Cache-Control: private` keeps a personalized response in the private cache only. A cookie by itself does not make a response private.
- `Vary` keys the stored response by the listed request headers as well as the URL. MDN's example is `Vary: Accept-Language`. Do not add `User-Agent`. Too many values, almost no reuse.
- `no-cache` forces validation. `ETag` / `If-None-Match` returns `304` when the validator matches. `no-store` avoids storing the new response. It does not erase an older one.
- Give disposable Redis keys a TTL. `volatile-ttl` can then evict the ones closest to expiry. A TTL also makes a lock deadlock-free if the holder dies.
- Do not serve a replica as if it were the primary. Asynchronous replication can be behind. During a partition, pick the up-to-date side or the side that still accepts updates, and recover afterward.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>How do you handle network latency in distributed caching?</summary>

### How do you handle network latency in distributed caching?

**Definition:**

Put a reusable response closer to the client so the request never reaches the origin. A local hit avoids the network. A remote cache only helps when it is closer than the origin.

**How it works:**

- MDN: because the request is not delivered to the origin, the closer the client and the cache, the faster the response. The browser cache is the typical case. A CDN or reverse proxy is a managed cache on that path.
- Request collapse sends one origin request for many identical concurrent ones, so latency to the origin is paid once.
- `max-age` on an immutable hashed URL, plus `immutable`, avoids revalidation on reload. MDN's pattern is `Cache-Control: max-age=31536000, immutable`.
- Redis is still a network hop away from the application. It wins when the origin (a database query, a render) is slower than that hop. `INFO` hit rate tells you whether it is.
- Spark `MEMORY_ONLY` keeps partitions in the JVM so the next action does not recompute them. The guide says later actions are often more than 10 times faster. That is latency inside the cluster, not an HTTP header.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>What are the challenges in maintaining a distributed cache?</summary>

### What are the challenges in maintaining a distributed cache?

**Definition:**

You are keeping copies. The hard parts are staleness, memory limits, lost nodes, and a herd of clients missing the same key.

**How it works:**

- Without `Cache-Control`, HTTP may still store and reuse a response by heuristic caching. MDN says every response should set `Cache-Control` explicitly. A long `max-age` then cannot be deleted from an intermediate cache by the spec.
- Redis will evict or error when `maxmemory` is hit. The wrong policy shows up as a low hit rate and a high `evicted_keys`. Replication buffers are extra RAM and are not counted in that limit.
- Asynchronous replication can drop the latest write. A single node is a single place the cache can fail.
- A stampede is many clients missing one key and all loading the origin. A lock key with a TTL is Redis's deadlock-free mutual exclusion. Redlock wants a majority of independent instances.
- Personalized data in a shared cache leaks. `private`, and `Vary` for negotiation, are the HTTP controls. Request collapse will reuse a response even when `no-cache` is set, so `private` is what keeps a user-specific body out of that collapse.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>Explain the concept of a write-through cache in a distributed environment.</summary>

### Explain the concept of a write-through cache in a distributed environment.

**Definition:**

Write-through means the application updates the durable store and the cache copy as part of the same change, so the next read can hit. The cache is still a copy Redis is allowed to evict.

**How it works:**

- Redis describes a cache as speeding reads of a slower server or database. Entries are copies. Evicting them is safe because they can be loaded again.
- On a write, update the database, then `SET` the cache key to the new value (with a TTL if the key is disposable). A later `GET` hits. If you only delete the key, the next read misses and loads the origin once.
- If the process dies between the database write and the cache write, the copy is stale or missing until TTL, eviction, or the next load. That window is the consistency gap. Asynchronous replication can drop the cache write even after `SET` returns on the primary.
- `noeviction` refuses commands that add data once `maxmemory` is reached. Reads of existing keys still work. Use that only when this Redis is not allowed to forget keys. Otherwise keep durable data in the database and let the cache evict.
- HTTP is not write-through. A `POST` is one of the few spec methods that can disturb a stored response, and MDN notes it is awkward for clients. Prefer `no-cache` when the resource changes often so the origin keeps seeing requests.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>How does a distributed cache handle node failures?</summary>

### How does a distributed cache handle node failures?

**Definition:**

A lost cache node drops the copies it held. Readers miss and load the origin, or they read another copy if you actually replicated. The durable data has to live somewhere else.

**How it works:**

- Redis eviction docs treat cache data as rebuildable. Losing a node is a large eviction: the keys are gone, the next read is a miss, `keyspace_misses` rises, and the origin fills them again.
- A replica is not a guaranteed copy. Replication is asynchronous, so the latest writes may never have arrived. Redlock's rule is a majority of separate instances, plus a TTL so a dead holder does not keep the lock.
- Spark's cache is fault-tolerant in a different way: a lost RDD partition is recomputed from the transformations that created it. Replicated levels (`*_2`) let tasks continue without waiting for that recompute.
- An HTTP managed cache failing open means the request reaches the origin. That is correct for a cache. It is not correct for a lock or a session you stored only in that node.
- Set `maxmemory` below RAM when replication or persistence is on, so the replica buffer has room. Otherwise eviction and the buffer can chase each other.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>What is a shared cache, and how does it differ from a distributed cache?</summary>

### What is a shared cache, and how does it differ from a distributed cache?

**Definition:**

In HTTP, a shared cache sits between clients and the origin and may store only responses that are safe to share. A distributed cache is an application store, such as Redis, that many servers read by key.

**How it works:**

- MDN splits caches into private and shared. Shared caches are proxy caches and managed caches. Managed examples are reverse proxies, CDNs, and service workers with the Cache API.
- A shared HTTP cache is allowed to reuse a response across users. That is why `private` and `Vary` exist. Request collapse is a shared-cache behavior.
- The HTTP spec does not define deleting an entry. A managed cache often can, through its own API. A proxy cache in the path often cannot see HTTPS bodies at all, so it only tunnels them.
- Redis is not that proxy. The application chooses the key, the TTL, and the eviction policy. Any app node that can reach Redis can read the key. That is distribution by an explicit protocol, not by `Cache-Control`.
- Putting a user-specific value in Redis under a key that omits the user id is the same leak as omitting `private` on a shared HTTP cache.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
</details>

### Advanced Caching Concepts

<details>
<summary>Explain the concept of a content delivery network (CDN) and its relation to caching.</summary>

### Explain the concept of a content delivery network (CDN) and its relation to caching.

**Definition:**

A CDN is a managed cache: the service deploys it to offload the origin and to store responses closer to clients.

**How it works:**

- MDN lists CDNs with reverse proxies and service workers as managed caches. You steer them with `Cache-Control` and with the product's own configuration, dashboard, or API.
- Because they are managed, they can delete a stored response even though the HTTP spec does not define explicit deletion. Some send their own header such as `Surrogate-Control`. A `CDN-Cache-Control` header is noted as work in progress.
- `Cache-Control: no-store` can opt out of a private or proxy cache while a managed cache still applies its own strategy. If that cache ignores `no-store` on purpose, MDN says to follow that product's docs rather than piling on kitchen-sink headers.
- A long `max-age` on a URL you might change means the CDN may not come back to the origin. Use a new URL (cache busting) for immutable bytes, or `no-cache` when the bytes change.
- Personalized responses do not belong there unless the cache key includes whatever `Vary` lists, or the response is `private`.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>Explain edge caching and its use cases.</summary>

### Explain edge caching and its use cases.

**Definition:**

Edge caching means the stored response is near the client. MDN's reason is distance: a cache closer than the origin returns faster and spares the origin the work.

**How it works:**

- The browser's own cache is the nearest private edge. A CDN or reverse proxy is a shared managed cache further out, still in front of the origin.
- Use it for responses that many clients can share: hashed static files with a long `max-age` and `immutable`, or a public document whose `ETag` can revalidate.
- Do not use it for a body that depends on a cookie unless you sent `private`, which keeps it out of the shared edge. Content negotiation belongs in `Vary`, not in a single shared body.
- Service workers plus the Cache API are an edge on the device itself. Fill them at `install`. They ignore HTTP cache headers, so your script is the policy.
- Request collapse at a shared edge cuts a burst of identical requests down to one origin fetch. It will reuse that result even for `no-cache`, so do not rely on collapse for a per-user body.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
</details>

<details>
<summary>What is cache warming and when is it used?</summary>

### What is cache warming and when is it used?

**Definition:**

Warming fills the cache before the traffic that would miss. The sourced form is a service worker writing responses during `install`, before it handles fetches.

**How it works:**

- MDN's service worker `install` handler opens a cache and `addAll`s the offline assets. Until that finishes, those URLs would be misses.
- A cold Redis is the same shape. The first readers miss, and `keyspace_misses` climbs, until something `SET`s the keys. If many readers miss together, that is a stampede. One loader should take the TTL lock.
- Spark `persist` or `cache` does not fill memory until the first action computes the RDD. That first action is the warm-up. Later actions reuse the partitions and are often much faster.
- Warm data you know will be read, and that is expensive to rebuild. Do not warm every key. `allkeys-lru` will evict the cold ones anyway once `maxmemory` is hit.
- A warmed HTTP response with a huge `max-age` is then hard to delete. Warm immutable URLs, or be ready to purge through the managed cache's API.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>How does query result caching work in database systems?</summary>

### How does query result caching work in database systems?

**Definition:**

The database stays the durable store. A cache stores the result of a query under a key and serves the next identical read from that copy until it expires or is evicted.

**How it works:**

- Redis is documented for exactly this: speed up reads of a slower database. The entry is a copy, so eviction under `maxmemory` is acceptable.
- The key has to name the query. A different parameter is a different key. Otherwise two queries share one body, which is the same bug as omitting `Vary`.
- Hits and misses are `keyspace_hits` and `keyspace_misses` from `INFO stats`. The hit percentage is hits divided by hits plus misses. A low percentage plus high `evicted_keys` means the policy is dropping keys you still read. A high `expired_keys` means the TTL is shorter than the reuse.
- When the underlying rows change, the copy is stale. Delete or replace that key on the write, or give it a TTL you can tolerate. HTTP's analogue is `ETag` revalidation rather than hoping the copy tracks the table.
- Do not put the only copy of a write in the cache. `noeviction` errors on new writes at the limit. The database is the place the write must land.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>Describe object caching and its advantages in object-oriented programming.</summary>

### Describe object caching and its advantages in object-oriented programming.

**Definition:**

Object caching stores a computed object under a key so the next caller does not build it again. The stored value is a copy, separate from the caller's instance.

**How it works:**

- Same rule as any cache: the expensive constructor or query runs on a miss. A hit returns the stored value. Redis keeps that value as its own copy of persistently stored data.
- If the caller mutates the object it received, that does not update the cache and must not be assumed to update other callers. Write a new value with `SET` when the durable state changes.
- Spark's `MEMORY_ONLY` stores deserialized Java objects in the JVM. That is the CPU-cheap form. `MEMORY_ONLY_SER` stores one serialized byte array per partition, which uses less space and more CPU to read. Python always pickles, so the serialized level does not change the format.
- A cache of objects still needs a bound. Redis evicts at `maxmemory`. Spark drops old partitions LRU and can `unpersist`.
- Do not cache an object that includes another user's private fields and then share the cache. That is the `private` rule in object form.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>Discuss the impact of caching on microservices architecture.</summary>

### Discuss the impact of caching on microservices architecture.

**Definition:**

Each service can hide origin work behind a private or shared copy. The copy becomes another source of truth unless validation and eviction are explicit.

**How it works:**

- A shared managed cache or a Redis instance in front of several services means one stored response serves many callers. That cuts load, which is the HTTP point of caching.
- It also couples them. A long `max-age` means the writing service cannot make intermediate caches drop the old body. Services that must see updates use `no-cache` and validators, or they delete the Redis key on the write.
- Personalized service responses need `private` or a key that includes the caller. A shared Redis key without that id leaks across tenants.
- One Redis for every service is a shared failure point, and its replication is asynchronous. A cache miss must still be able to reach the owning service.
- Local memory inside one service instance is invisible to the others. After a deploy, that memory is empty. The first requests miss. Guard that with a single-flight lock if the origin is expensive.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>Explain how caching interacts with serverless computing models.</summary>

### Explain how caching interacts with serverless computing models.

**Definition:**

A serverless invocation does not keep a process-local cache you can count on. Reuse has to live in HTTP caches, a managed edge, or an external store such as Redis.

**How it works:**

- MDN's browser and CDN caches sit outside the function. A hashed static asset with `max-age` and `immutable` never needs the function. A dynamic response with `no-cache` still invokes something that can revalidate.
- If the platform discards the process, an in-memory map is empty on the next call. Redis survives that, with `maxmemory` and a TTL. The function pays a network hop and must handle a miss by reading the database.
- Cold starts multiply a stampede: many new instances miss together. One of them should take the Redis lock with a TTL and load the origin.
- Do not store the only copy of user state in the function or in a cache with `allkeys-lru`. Eviction and instance death both drop it. `private` still applies if a shared HTTP cache might see the response.
- The Cache API is for the browser service worker, not for the serverless process. It does not honor HTTP cache headers and lives until the script deletes entries.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
</details>

<details>
<summary>Describe cache compression techniques and their trade-offs.</summary>

### Describe cache compression techniques and their trade-offs.

**Definition:**

A smaller stored value uses less memory and less bandwidth. It costs CPU to encode and decode, and it must be a separate cache entry when clients negotiate different encodings.

**How it works:**

- Spark's `MEMORY_ONLY_SER` stores a serialized byte array per partition. The guide says this is more space-efficient than deserialized objects and more CPU-intensive to read. Leave `MEMORY_ONLY` when the data fits. It is the most CPU-efficient. In Python, pickle is always used, so choosing a serialized level does not change that.
- HTTP content negotiation can return a different body for `Accept-Encoding`. If those bodies differ, `Vary` must include that header or a cache will reuse the wrong one. MDN warns that a `Vary` header with too many variants, especially `User-Agent`, destroys reuse.
- Redis stores the bytes you `SET`. A compressed value leaves more room under `maxmemory` and costs CPU on every read and write. Eviction still follows `maxmemory-policy`. Compression does not change hit-rate math.
- Do not compress to hide a missing TTL or a private response in a shared cache. Size and privacy are different problems.
- Disk spill (`MEMORY_AND_DISK`, `DISK_ONLY`) is the other space trade-off in Spark. The guide says not to spill unless recomputing the partition is expensive. Reading disk can be slower than recomputing.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
</details>

<details>
<summary>How does machine learning influence caching strategies?</summary>

### How does machine learning influence caching strategies?

**Definition:**

Redis LFU keeps a small estimate of how often a key is used and decays that estimate, so a key that was hot and then went quiet can be evicted. That is the documented frequency policy, not a separate model service.

**How it works:**

- `allkeys-lfu` and `volatile-lfu` evict by approximated least-frequently-used. The estimate is a Morris counter in a few bits, plus a decay period so old frequency fades. Defaults in the docs: the counter saturates around one million requests, and it decays about once a minute (`lfu-log-factor 10`, `lfu-decay-time 1`).
- Decay is why a pattern shift matters. Without it, a key that was popular last week stays protected. With it, the policy follows what is frequent now.
- `allkeys-lru` is still the recommended default when a small hot set dominates, which the page ties to a Pareto-style pattern. Use LFU when frequency, not recency alone, predicts the next read. Check the choice with `keyspace_hits`, `keyspace_misses`, and `evicted_keys`.
- Sampling is approximate for LRU and LFU. `maxmemory-samples` is how many keys are considered. This is not a training loop you deploy beside the cache.
- HTTP does not have an LFU directive. Freshness is `max-age` and validators. A CDN's own policy is product configuration.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
</details>

<details>
<summary>Discuss the role of caching in IoT (Internet of Things) applications.</summary>

### Discuss the role of caching in IoT (Internet of Things) applications.

**Definition:**

A device is a client with a private cache. It reuses a fresh response while it cannot or should not reach the origin, and it must not share another device's private response.

**How it works:**

- MDN's private cache is tied to one client. A device storing its own responses matches that. `Cache-Control: private` is required if a shared proxy might otherwise keep a personalized body.
- Intermittent connectivity is what the Cache API is for on the web: the service worker stores Request/Response pairs at `install` and serves them until the script deletes them. Those entries ignore HTTP expiry, so the device script has to delete or version them (`v1`, `v2`).
- A gateway in front of many devices is a shared cache. Request collapse helps identical reads. It is wrong for per-device data unless the response is `private` or `Vary` separates them.
- If the gateway uses Redis, set `maxmemory` and a policy. Sensor values that only matter briefly should have a short TTL so `volatile-ttl` or ordinary expiry drops them before they fill memory.
- Replication does not make a reading durable. The durable history belongs in storage that is not evicted as a cache.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
- https://redis.io/docs/latest/develop/reference/eviction/
</details>

### Caching Tools and Technologies

<details>
<summary>What are some popular caching systems and their key features?</summary>

### What are some popular caching systems and their key features?

**Definition:**

The mechanisms with a spec or a vendor page here are the HTTP cache, Redis, Spark's RDD cache, and the browser Cache API.

**How it works:**

- HTTP caches store a response for a request. Private versus shared, freshness via `max-age`, validators via `ETag` and `Last-Modified`, and explicit `Cache-Control`. Managed caches add a dashboard or API delete.
- Redis is a cache in front of a slower store. `maxmemory` plus a policy (`allkeys-lru` as the usual default, also LFU, random, volatile TTL). `INFO` exposes hits, misses, and evictions. Locks use a key plus a TTL. Replication is asynchronous.
- Spark `cache()` is `MEMORY_ONLY`. `persist` selects a storage level, including disk spill and a second replica. Lost partitions are recomputed. LRU drops old partitions. `unpersist` drops one RDD.
- The Cache API stores Request/Response pairs for a service worker or window. It ignores HTTP cache headers. You delete entries yourself.
- A path proxy in front of HTTPS usually cannot cache the body. Do not pick a caching product by assuming it sees plaintext.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>How do you configure cache settings in a web server (e.g., Nginx, Apache)?</summary>

### How do you configure cache settings in a web server (e.g., Nginx, Apache)?

**Definition:**

Configuration here means the origin emits the HTTP caching headers MDN specifies. A managed reverse proxy then follows those headers or its own documented rules.

**How it works:**

- Send `Cache-Control` on purpose. Heuristic caching applies when you do not. `max-age` is the freshness lifetime. Prefer it over `Expires`.
- Immutable hashed files: `Cache-Control: max-age=31536000, immutable`. Changing documents: `no-cache` plus `ETag` and `Last-Modified`. Personalized documents: `private`. Responses that must not be stored: `no-store`, used sparingly because it gives up the back/forward cache.
- Add `Vary` only for headers that actually change the body, such as `Accept-Language`.
- A reverse proxy or CDN is a managed cache. MDN says to read that product's configuration. It may delete entries from a dashboard even though HTTP cannot. Avoid the kitchen-sink list `no-store, no-cache, max-age=0, must-revalidate, proxy-revalidate` as a substitute for reading that config.
- `fetch` cache modes are the browser side of the same headers: `cache: "no-cache"` revalidates, `cache: "reload"` is the force reload.

**Examples:**

```http
Cache-Control: max-age=31536000, immutable
```
```http
Cache-Control: no-cache
ETag: "33a64df5"
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>Explain the role of HTTP headers in web caching.</summary>

### Explain the role of HTTP headers in web caching.

**Definition:**

The headers tell a cache whether it may store a response, how long the response stays fresh, and how to revalidate it. The URL is the default key. `Vary` extends that key.

**How it works:**

- `Cache-Control: max-age=604800` means fresh for that many seconds of age. A shared cache can add `Age` so the client sees the remaining freshness. `Expires` is the old absolute date. If both exist, `max-age` wins.
- `no-cache` stores but must revalidate. `no-store` does not store this response and does not delete an older one. `private` limits storage to the private cache. `immutable` with a long `max-age` skips reload revalidation for content that never changes.
- `ETag` is an opaque validator. A stale cache sends it as `If-None-Match`. The origin returns `304` when it matches and `200` with a new body when it does not. `Last-Modified` pairs with `If-Modified-Since`. If both validators are present, `If-None-Match` takes precedence.
- `Vary: Accept-Language` stores a separate response per language. The same URL with a different `Accept-Language` must not reuse the other body.
- `Clear-Site-Data: cache` can clear the browser cache. MDN says it does not affect intermediate caches. A reload sends `Cache-Control: max-age=0` plus validators. A force reload sends `Cache-Control: no-cache` and `Pragma: no-cache`.

**Examples:**

```http
Cache-Control: max-age=3600
ETag: "33a64df5"
Vary: Accept-Language
```

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>How do caching mechanisms differ across various programming languages?</summary>

### How do caching mechanisms differ across various programming languages?

**Definition:**

The HTTP and Redis rules do not change with the language. What changes is how the runtime stores an in-process object, which Spark's Java versus Python storage levels show.

**How it works:**

- `Cache-Control`, `ETag`, and `Vary` are headers any HTTP client must send and honor the same way. `fetch(..., { cache: "no-cache" })` is the browser API for a revalidating request. Another language's HTTP client still speaks those headers.
- Redis commands (`SET`, `GET`, `INFO`, `CONFIG SET maxmemory`) are the protocol. A client library is a binding. Eviction happens in the server, not in the language runtime.
- Spark is the place the language shows up. Java and Scala can use `MEMORY_ONLY_SER`. Python always serializes with pickle, and the serialized storage levels are not a different format there. `cache()` remains `MEMORY_ONLY` in all of them.
- The Cache API is JavaScript in a window or service worker. Entries do not expire by HTTP headers in any language wrapping it. The script deletes them.
- An in-process object cache is ordinary memory of that runtime. It is a private cache of one process. It is not a shared HTTP cache and it is empty after the process exits.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
</details>

<details>
<summary>Describe the use of caching in mobile application development.</summary>

### Describe the use of caching in mobile application development.

**Definition:**

The phone holds a private cache. It can reuse fresh responses and, with a service worker, keep Request/Response pairs for offline use until the app deletes them.

**How it works:**

- A private cache may store a personalized response because it is not shared. If any shared proxy is on the path, the origin still sends `Cache-Control: private` for that body.
- `no-store` on the origin opts out of storing and also gives up the back/forward cache. MDN prefers `no-cache` plus `private` when you need freshness without that loss.
- For an installed web app, `install` fills a named Cache API cache and `activate` deletes older names. The Cache API ignores HTTP caching headers, so a new app version must change the cache name or call `delete`.
- Large media should be a long-lived immutable URL when the bytes do not change, so the phone does not revalidate on every reload (`max-age` and `immutable`).
- Do not pin session secrets only in that cache. Cache entries are copies. A managed or private cache can drop them, and `Set-Cookie` is stripped from Cache API responses.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers
- https://developer.mozilla.org/en-US/docs/Web/API/Cache
</details>

<details>
<summary>What tools are available for monitoring and analyzing cache performance?</summary>

### What tools are available for monitoring and analyzing cache performance?

**Definition:**

Redis exposes hit, miss, and eviction counters through `INFO`. HTTP freshness is visible as `Age` and as whether the origin still receives requests.

**How it works:**

- `INFO stats` includes `keyspace_hits` and `keyspace_misses`. The hit percentage is hits / (hits + misses) × 100. `EXISTS` reporting a missing key counts as a miss.
- If that percentage is below the hot-set you expected, read `evicted_keys`. A high value means the policy is dropping useful keys. `allkeys-lru` is the documented next try. If `evicted_keys` is low and `expired_keys` is high, the TTL is too short or applied to the wrong keys.
- `INFO memory` includes `used_memory_dataset` and `mem_not_counted_for_evict`. `current_eviction_exceeded_time` is how long the cache has been over `maxmemory`. `commandstats` shows commands rejected at the limit under `noeviction`.
- For HTTP, a response that never reaches the origin is a hit in front of you. `Age` tells the client how long a shared cache has held it. If you needed the origin to see the request, you used too long a `max-age`.
- Spark drops cached partitions LRU. A recomputed partition after a loss is the cache miss analogue. The storage level decides whether that recompute was required.

**Sources:**

- https://redis.io/docs/latest/develop/reference/eviction/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Explain the integration of caching in cloud computing services.</summary>

### Explain the integration of caching in cloud computing services.

**Definition:**

Cloud edges and managed caches are the shared layer in front of an origin. An explicit cache such as Redis is a separate service with its own memory limit. Neither one replaces durable storage.

**How it works:**

- MDN's managed caches are what a cloud CDN or reverse proxy is for: offload the origin, store responses near clients, and delete them from a dashboard or API when the HTTP spec will not.
- Control them with `Cache-Control`, `ETag`, and `Vary`. Product-specific headers such as `Surrogate-Control` apply only to that product. Follow that product's docs if it deliberately ignores `no-store`.
- Redis in the same cloud is the application cache. Set `maxmemory` below the instance RAM when replication is on, pick `allkeys-lru` unless you have a reason not to, and watch `INFO`. Asynchronous replicas can lose the latest write.
- A partition still forces the CAP choice. The cache can serve a stale copy (available, not the single up-to-date value) or refuse the read. Do not claim both.
- Server processes in the cloud are replaceable. Their local maps are not a cache you can page someone about. Put shared reuse in the managed HTTP cache or in Redis.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>How does caching work in content management systems (CMS)?</summary>

### How does caching work in content management systems (CMS)?

**Definition:**

A CMS page is an HTTP response. Cache it with `max-age` while it is stable, and revalidate with `ETag` and `Last-Modified` when editors change it. `Last-Modified` is also how a CMS shows the last edit time.

**How it works:**

- MDN notes that `Last-Modified` is not only a cache validator. CMS systems use it to display the last-modified time, and crawlers use it to adjust crawl frequency. Send it together with `ETag` on a `200`. On revalidation, `If-None-Match` wins if both are present.
- A published page that rarely changes can be fresh for a `max-age`. A page an editor just saved cannot push that fresh copy out of an intermediate cache. Use `no-cache` if the CMS must see every view, or change the URL, or purge the managed cache through its API.
- Personalized CMS output (a logged-in editor bar) needs `Cache-Control: private` or it will be shared. A cookie does not imply `private`.
- `Vary` if the same URL returns different languages. Do not vary on `User-Agent`.
- Heuristic caching can store a CMS response that omitted `Cache-Control`, using a fraction of the age since `Last-Modified`. Set the header so you are not relying on that.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>

<details>
<summary>What are the considerations for caching in a serverless architecture?</summary>

### What are the considerations for caching in a serverless architecture?

**Definition:**

Count on caches that outlive one invocation. Do not count on the function's memory. Misses must be safe, and personalized responses must stay private.

**How it works:**

- Static content should stop at a CDN with a long `max-age` on a hashed URL. The function should not run for that URL.
- Dynamic content that must be current uses `no-cache` and validators so a shared cache revalidates. `no-store` is for responses you truly must not retain, and it costs the back/forward cache in the browser.
- A Redis key with a TTL is the cross-invocation copy. Size it with `maxmemory`. Expect misses after eviction and after a cold function. One caller takes a TTL lock so the rest do not stampede the database.
- The function can die after writing the database and before `SET`. The next read must tolerate a miss or a stale key. Asynchronous replication can lose the `SET` too.
- `Cache-Control: private` if the body depends on the caller. A shared edge will otherwise collapse concurrent requests onto one stored response.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
- https://redis.io/docs/latest/develop/reference/eviction/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>Name some caching strategies in big data processing and analytics.</summary>

### Name some caching strategies in big data processing and analytics.

**Definition:**

Spark persists an RDD so later actions reuse partitions instead of recomputing them. Redis holds a small hot result in front of a slower store. HTTP caching is for the response you send out, not for the shuffle.

**How it works:**

- `persist` or `cache` marks the RDD. The first action computes it and keeps partitions on the nodes. The guide says later actions are often more than 10 times faster. This is aimed at iterative algorithms and interactive use.
- `cache()` means `MEMORY_ONLY`: deserialized Java objects, and partitions that do not fit are recomputed. `MEMORY_AND_DISK` spills those partitions. `MEMORY_ONLY_SER` saves space in Java and Scala. Python always pickles. `*_2` levels replicate each partition on two nodes so serving can continue without waiting to recompute.
- Spark already persists some shuffle data, for example around `reduceByKey`, so a failed node does not recompute the whole input. Still call `persist` if you reuse the result. The cache itself is LRU per node. `unpersist()` drops it. Lost partitions are recomputed from the lineage, so the cache is fault-tolerant without being the durable copy.
- Do not spill to disk unless the computation is expensive or it filters a lot. Recomputing can be as fast as reading disk.
- A small aggregate that many requests read can sit in Redis under `allkeys-lru` with a TTL. The cluster data stays in the job's input. The cache entry is a copy you can evict.

**Examples:**

```scala
val lines = sc.textFile(path).persist(StorageLevel.MEMORY_ONLY)
lines.count()
lines.count() // reuses partitions
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://redis.io/docs/latest/develop/reference/eviction/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
</details>
