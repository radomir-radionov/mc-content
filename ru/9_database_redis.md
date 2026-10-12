# Database — Redis

In-memory cache, sessions, and data structures.

[← Back to NoSQL hub](./9_database_nosql.md)

### Related docs

[NoSQL — Core](./9_database_nosql_core.md) · [NoSQL — MongoDB](./9_database_mongodb.md) · [NoSQL — Redis](./9_database_redis.md) · [NoSQL — ClickHouse](./9_database_clickhouse.md)

### Sections

- [Redis](#redis)

---

## Redis

<details>
  <summary>Redis</summary>

### Redis

**Definition:** In-memory **key-value** store with rich data types (strings, hashes, lists, sets, sorted sets, streams). Optional persistence to disk.

**Why we need it:** Postgres is too slow and too heavy for “read this key 10,000 times per second” (sessions, cache, rate limits). Redis answers in sub-millisecond from RAM.

**How it works:**

| Type | Example use |
| ---- | ----------- |
| **String** | `SET user:42 "{...}" EX 3600` — cached JSON |
| **Hash** | `HSET cart:42 item:1 qty 2` — small objects |
| **List** | Simple queues (prefer BullMQ for jobs) |
| **Sorted set** | Leaderboards — score + rank |
| **TTL** | Auto-expire keys — critical for cache |

**When to use:**

| Use case | Why Redis |
| -------- | --------- |
| **API response cache** | Avoid repeated Postgres queries |
| **Session store** | Fast read/write per logged-in user |
| **Rate limiting** | `INCR` + TTL per IP or API key |
| **Pub/sub** | Simple real-time (not a full message broker) |
| **Job queues** | BullMQ uses Redis under the hood |

**Advantages:**

| Advantage | What you gain |
| --------- | ------------- |
| **Sub-ms latency** | Hot paths stay fast |
| **TTL built-in** | Cache entries expire automatically |
| **Simple API** | `GET` / `SET` / `DEL` — easy from Node |
| **Structures beyond strings** | Leaderboards, counters without SQL |

**Pluses and minuses:**

| + | − |
| --- | --- |
| Extremely fast | Data mainly in RAM — cost at scale |
| TTL, pub/sub, leaderboards | Not primary DB for business truth |
| Replication and cluster mode | Wrong persistence mode = data loss risk |

**Examples:**

```js
async function getUser(id) {
  const cached = await redis.get(`user:${id}`);
  if (cached) return JSON.parse(cached);
  const user = await db.user.findUnique({ where: { id } });
  await redis.setEx(`user:${id}`, 3600, JSON.stringify(user));
  return user;
}
```

**Thoughts:** Always set **TTL** on cache keys. Never `KEYS *` in production — use `SCAN`. Postgres remains source of truth; Redis is a speed layer.

**Sources:** [Redis documentation](https://redis.io/docs/)

---

</details>
