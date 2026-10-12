# Database — NoSQL Core

NoSQL fundamentals, store selection, and Elasticsearch for search.

[← Back to NoSQL hub](./9_database_nosql.md)

### Related docs

[NoSQL — Core](./9_database_nosql_core.md) · [NoSQL — MongoDB](./9_database_mongodb.md) · [NoSQL — Redis](./9_database_redis.md) · [NoSQL — ClickHouse](./9_database_clickhouse.md)

### Sections

- [NoSQL foundations](#nosql-foundations)

---

## NoSQL foundations

<details>
  <summary>NoSQL (concept)</summary>

### NoSQL (concept)

**Definition:** Non-relational databases optimized for flexible schemas, horizontal scale, or specialized access patterns — document, key-value, column, graph.

**Why we need it:** Postgres is the default for most apps, but some workloads fit poorly: nested JSON that changes every week, sub-ms cache reads, or billion-row analytics. NoSQL stores trade SQL joins for a model that matches one access pattern really well.

**When to use:** Flexible documents, hot cache layers, analytics on huge tables, search at scale — **not** as a blind replacement for Postgres.

**Advantages:**

| Advantage | What you gain |
| --------- | ------------- |
| **Schema flexibility** | Ship new product fields without `ALTER TABLE` migrations |
| **Horizontal scale** | Sharding built into MongoDB, Redis Cluster, ClickHouse clusters |
| **Specialized speed** | Redis for cache; ClickHouse for aggregates; ES for full-text |
| **Simpler mental model** | One document = one product; one key = one session |

**Pluses and minuses:**

| + | − |
| --- | --- |
| Fast iteration on document shape | Weaker default consistency than SQL |
| Good for nested / hierarchical data | No universal query language |
| Scales out for specific patterns | Joins and cross-store transactions are harder |

**Thoughts:** “Schema-less” means schema lives in **application code** — use Mongoose, Zod, or JSON Schema anyway.

**Sources:** [MongoDB — Data modeling](https://www.mongodb.com/docs/manual/core/data-modeling-introduction/)

---

</details>

<details>
  <summary>MongoDB vs Redis vs ClickHouse</summary>

### MongoDB vs Redis vs ClickHouse

**Definition:** Three common NoSQL picks in Node backends — different jobs, not interchangeable.


| | MongoDB | Redis | ClickHouse |
| | ---  |  ---  |  ---  |
| | **Model** | Document (BSON JSON) | Key-value + structures in RAM | Column-oriented OLAP |
| | **Primary job** | App data with flexible fields | Cache, sessions, fast lookups | Analytics, dashboards, logs |
| | **Durability** | Primary database for documents | Ephemeral or optional persistence | Durable analytics store |
| | **Query** | Rich aggregation pipeline | Simple commands per key | SQL-like, huge scans fast |
| | **Typical stack role** | Main DB (when documents win) | Side cache next to Postgres | Read replica for reports |

**When to use:**

- **MongoDB** — catalog with varying attributes, content, mobile sync with embedded docs.
- **Redis** — cache API responses, sessions, rate limits, BullMQ jobs.
- **ClickHouse** — “count orders per day for 500M rows” in seconds, not minutes.

**Thoughts:** Default Node stack: **Postgres + Redis**. Add Mongo only when the document model wins. Add ClickHouse when OLTP DB struggles with heavy `GROUP BY` on history.

**Sources:** [Redis docs](https://redis.io/docs/) · [ClickHouse docs](https://clickhouse.com/docs)

---

</details>

<details>
  <summary>DB selection matrix</summary>

### DB selection matrix

**Definition:** Quick guide for picking a store per workload.

| Need | Choose | Avoid |
| ---- | ------ | ----- |
| Transactions, joins, reporting | **PostgreSQL** | Mongo for relational data |
| Flexible nested documents | **[MongoDB](./9_database_mongodb.md)** | Postgres JSONB if queries stay simple |
| Cache, sessions, rate limits | **[Redis](./9_database_redis.md)** | Postgres for hot ephemeral data |
| Analytics on billions of rows | **[ClickHouse](./9_database_clickhouse.md)** | Postgres without partitioning |
| Full-text search at scale | **Elasticsearch** (below) | `LIKE '%x%'` on millions of rows |
| Async service communication | **[Message broker](../12_message_broker.md)** | Database as message bus |

**Thoughts:** Most Node apps: **Postgres + Redis + (optional) queue**. Add Mongo, ClickHouse, or ES only when measured need exists.

**Sources:** [ByteByteGo — Database types](https://blog.bytebytego.com/p/understanding-database-types)

---

</details>

<a name="elasticsearch"></a>

<details>
  <summary>Elasticsearch</summary>

### Elasticsearch

**Definition:** Distributed search engine (Lucene + JSON). Index documents, query with relevance and analyzers. Job is **search**, not OLTP.

**When to use:** Product catalog search, log analytics (ELK), autocomplete — when Postgres full-text is not enough.

**Pluses and minuses:**

| + | − |
| --- | --- |
| Relevance ranking, morphology, scale | Eventual consistency vs primary DB |
| Query DSL + official JS client | Ops and mapping changes; not source of truth for money |

**Thoughts:** Keep orders and payments in Postgres/Mongo; sync to ES for search (dual-write or outbox).

**Sources:** [Elasticsearch — text analysis](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis.html)

---

</details>
