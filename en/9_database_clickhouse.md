# Database — ClickHouse

Column-oriented OLAP for analytics and fast aggregates.

[← Back to NoSQL hub](./9_database_nosql.md)

### Related docs

[NoSQL — Core](./9_database_nosql_core.md) · [NoSQL — MongoDB](./9_database_mongodb.md) · [NoSQL — Redis](./9_database_redis.md) · [NoSQL — ClickHouse](./9_database_clickhouse.md)

### Sections

- [ClickHouse](#clickhouse)

---

## ClickHouse

<details>
  <summary>ClickHouse</summary>

### ClickHouse

**Definition:** Open-source **column-oriented** database for **OLAP** — optimized for scanning billions of rows and running aggregates (`COUNT`, `SUM`, `GROUP BY`) fast. Speaks SQL.

**Why we need it:** Postgres is row-oriented — great for “get order #123”. Bad for “sum revenue by country for 2 years of clicks” on billions of rows. ClickHouse stores columns separately so analytics queries read only needed columns.

**How it works:**

| Concept               | Role                                                            |
| --------------------- | --------------------------------------------------------------- |
| **Table**             | Columns with types; often append-only event/log data            |
| **MergeTree engine**  | Default — data sorted by key, background merges                 |
| **Partition**         | Split by date (`PARTITION BY toYYYYMM(date)`) — prune old scans |
| **Replica / shard**   | Cluster for HA and scale                                        |
| **Materialized view** | Pre-aggregate on insert for faster dashboards                   |

**When to use:**

| Use case              | Why ClickHouse                                |
| --------------------- | --------------------------------------------- |
| **Product analytics** | Page views, clicks, funnels over huge history |
| **Logs / metrics**    | Store and query application or access logs    |
| **BI dashboards**     | Sub-second `GROUP BY` on billions of events   |
| **Real-time reports** | Insert streams + fast aggregate queries       |

Skip ClickHouse for OLTP — updating single rows by primary key is a Postgres job.

**Advantages:**

| Advantage                | What you gain                                |
| ------------------------ | -------------------------------------------- |
| **Very fast aggregates** | Column storage + vectorized execution        |
| **SQL interface**        | Familiar `SELECT` for analysts               |
| **Compression**          | Less disk than row stores for analytics data |
| **Scales horizontally**  | Sharded clusters for ingest and query        |

**Pluses and minuses:**

| +                                                | −                                            |
| ------------------------------------------------ | -------------------------------------------- |
| Excellent for read-heavy analytics               | Not a replacement for Postgres transactions  |
| Handles huge ingest rates                        | Updates/deletes are limited compared to OLTP |
| Open source + managed options (ClickHouse Cloud) | Another system to operate and sync data into |

**Examples:**

```sql
-- Events table, partitioned by month
CREATE TABLE events (
  event_date Date,
  user_id UInt64,
  event_type String,
  amount Decimal(10, 2)
) ENGINE = MergeTree()
PARTITION BY toYYYYMM(event_date)
ORDER BY (event_date, user_id);

-- Fast dashboard query
SELECT event_date, count(), sum(amount)
FROM events
WHERE event_date >= today() - 30
GROUP BY event_date
ORDER BY event_date;
```

**Thoughts:** Typical pattern: **Postgres** = source of truth for orders; **ETL or streaming** (Kafka, cron) copies events into ClickHouse for reports. Do not run checkout on ClickHouse.

**Sources:** [ClickHouse documentation](https://clickhouse.com/docs)

---

</details>
