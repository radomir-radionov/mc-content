# Database — Core

ACID, CAP, transactions, and normalization.

[← Back to hub](./9_database.md)

### Related docs

- [DB — Core](./9_database_core.md) · [DB — SQL](./9_database_sql.md) · [DB — NoSQL](./9_database_nosql.md) · [DB — ORM](./9_database_orm.md) · [DB — Production](./9_database_production.md) · [DB — Interview](./9_database_interview.md)

### Sections

- [Core foundations](#core-foundations)

---

## Core foundations

<details>
  <summary>Database overview</summary>

### Database overview

**Definition:** A structured system for storing, retrieving, and managing data with defined access patterns, integrity rules, and concurrency control.

**How it works:** Client sends query via driver → connection pool → DB parses SQL → planner chooses index/seq scan → executor returns rows → transaction log ensures durability on commit.

**When to use:**

OLTP (Postgres) for users/orders; Redis for cache; queue for async — polyglot persistence when measured need exists.

Any app that must persist state beyond a single request — users, orders, sessions, analytics.

**Pluses and minuses:**

| +                          | −                                                     |
| -------------------------- | ----------------------------------------------------- |
| Central source of truth    | Operational complexity (backups, migrations, scaling) |
| Queryable, auditable data  | Schema and access patterns must be designed upfront   |
| ACID / consistency options | Wrong store choice is expensive to reverse            |

**Examples:**

```sql
-- Relational: structured, joinable
CREATE TABLE users (
  id         SERIAL PRIMARY KEY,
  email      TEXT NOT NULL UNIQUE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

**Thoughts:** A database is not just storage — it is a concurrency coordinator. Choose based on access patterns, consistency needs, and team expertise, not hype.

**Sources:**

[PostgreSQL — Introduction](https://www.postgresql.org/docs/current/tutorial.html)

---

</details>

<details>
  <summary>Database taxonomy</summary>

### Database taxonomy

**Definition:**

Databases group by data model and consistency guarantees — not by a single "SQL vs NoSQL" axis.

| Type                   | Examples                                                  | Best for                                   |
| ---------------------- | --------------------------------------------------------- | ------------------------------------------ |
| **Relational (RDBMS)** | PostgreSQL, MySQL, SQL Server                             | Structured data, joins, transactions       |
| **Document**           | MongoDB, CouchDB                                          | Flexible schemas, nested JSON documents    |
| **Key-value**          | Redis, DynamoDB                                           | Caching, sessions, simple lookups          |
| **Column-family**      | Cassandra, HBase                                          | Wide rows, high write throughput           |
| **Graph**              | Neo4j                                                     | Relationship-heavy queries (social, fraud) |
| **Time-series**        | TimescaleDB, InfluxDB                                     | Metrics, IoT, monitoring                   |
| **Search**             | [Elasticsearch](./9_database_nosql_core.md#elasticsearch) | Full-text search, log analytics            |

**When to use:** Start with one primary store (usually PostgreSQL). Add Redis/Mongo/queue only when measured pain justifies it.

**Pluses and minuses:**

| + (polyglot persistence)     | −                                 |
| ---------------------------- | --------------------------------- |
| Right tool per workload      | More ops, sync, and failure modes |
| Optimize read vs write paths | Data duplication across stores    |

**Thoughts:** "NoSQL" means **Not Only SQL** — many teams run Postgres + Redis + a queue. Relational model (tables, rows, keys, SQL): [Relational database](./9_database_sql.md#relational-database). See [DB selection matrix](./9_database_nosql_core.md#db-selection-matrix).

**Sources:**

[ByteByteGo — Database types](https://blog.bytebytego.com/p/understanding-database-types)

---

</details>

<details>
  <summary>ACID</summary>

### ACID

**Definition:** Four transactional guarantees — **Atomicity**, **Consistency**, **Isolation**, **Durability**.

**How it works:**

| Letter          | Meaning                                                           |
| --------------- | ----------------------------------------------------------------- |
| **Atomicity**   | The transaction is all-or-nothing                                 |
| **Consistency** | Constraints hold when it ends (not CAP “every replica current”)   |
| **Isolation**   | Concurrent transactions do not see each other’s uncommitted state |
| **Durability**  | A successful `COMMIT` survives process/OS crash (WAL flushed)     |

**When to use:** Financial transfers, inventory, booking systems — anywhere partial updates are unacceptable.

**Pluses and minuses:**

| +                                    | −                                            |
| ------------------------------------ | -------------------------------------------- |
| Predictable state after failures     | Higher write latency vs eventual consistency |
| Strong integrity for money/inventory | Harder to scale writes across regions        |
| Familiar mental model for OLTP       | Long transactions block other work           |

**Examples:**

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
-- ROLLBACK on any error — neither update persists alone
```

**Thoughts:**

- **Consistency** in ACID = constraints hold (FK, CHECK), not CAP consistency.
- NoSQL systems often relax **I** (isolation) or **D** (durability) for scale — see BASE.
- Keep transactions short; hold locks for milliseconds, not seconds.
- SQL verbs: [COMMIT and ROLLBACK](./9_database_sql.md#commit-and-rollback).

**Sources:**

[PostgreSQL — Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)

---

</details>

<details>
  <summary>CAP theorem</summary>

### CAP theorem

**Definition:**

In a distributed data store, you can guarantee at most **two** of three properties under network partition:

1. **Consistency** — every read returns the latest write (or errors)
2. **Availability** — every request gets a non-error response
3. **Partition tolerance** — system works when nodes cannot talk

**How it works:**

| System                   | Under partition | Trade-off                        |
| ------------------------ | --------------- | -------------------------------- |
| PostgreSQL (single node) | N/A             | Full ACID                        |
| MongoDB (default)        | AP              | Eventual consistency on replicas |
| etcd / ZooKeeper         | CP              | Consistency over availability    |
| Cassandra                | AP              | Tunable consistency per query    |

**When to use:** Designing multi-region or multi-node data layers — pick CP (strong consistency, may reject reads) or AP (always respond, may be stale).

**Pluses and minuses:**

| + (explicit trade-off)         | −                                           |
| ------------------------------ | ------------------------------------------- |
| Forces architectural honesty   | Oversimplified for real systems             |
| Guides CP vs AP product choice | Partitions are rare but inevitable at scale |

**Thoughts:** CAP assumes partition happens — the real question is behavior **during** and **after** recovery. See PACELC.

**Sources:**

[CAP theorem (Wikipedia)](https://en.wikipedia.org/wiki/CAP_theorem)

---

</details>

<details>
  <summary>PACELC theorem</summary>

### PACELC theorem

**Definition:** Extension of CAP — **if Partition**, choose **A** or **C**; **Else** (normal operation), choose **L**atency or **C**onsistency.

**When to use:** Explaining why read replicas return stale data even without network splits.

**Pluses and minuses:**

| +                                      | −                                            |
| -------------------------------------- | -------------------------------------------- |
| Models everyday latency vs consistency | Less famous than CAP — explain in interviews |
| Explains async replication behavior    | Not a formal proof like CAP                  |

**Examples:** Postgres primary + async replica — no partition, but replica lag means reads can be **eventually consistent** (lower latency on replica) or you read from primary (higher latency, strong consistency).

**Thoughts:** Most production incidents are "PACELC without P" — replica lag, not split brain.

**Sources:**

[PACELC (Daniel Abadi)](https://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html)

---

</details>

<details>
  <summary>BASE</summary>

### BASE

**Definition:** Alternative to strict ACID for distributed systems — **Basically Available**, **Soft state**, **Eventually consistent**.

**When to use:** Social feeds, analytics counters, shopping carts at scale, cache layers — where brief inconsistency is acceptable.

**Pluses and minuses:**

| +                                      | −                                               |
| -------------------------------------- | ----------------------------------------------- |
| High availability and write throughput | Stale reads, conflict resolution needed         |
| Scales horizontally easier             | Application must tolerate inconsistency windows |
| Fits agile schema evolution            | Debugging "wrong" data is harder                |

**Examples:**

```js
// Eventually consistent counter — two nodes may diverge briefly
await redis.incr("page:views:home");
// Different readers may see different counts for milliseconds
```

**Thoughts:** BASE is a design choice, not a database feature. You can run "BASE-style" reads on top of ACID Postgres via read replicas.

**Sources:**

[ACID vs BASE](https://www.ibm.com/think/topics/acid-vs-base)

---

</details>

<details>
  <summary>Transactions & isolation levels</summary>

### Transactions & isolation levels

**Definition:**

A transaction groups operations into one atomic unit. **Isolation levels** control what concurrent transactions can see from each other.

| Level            | Dirty read | Non-repeatable read | Phantom read      |
| ---------------- | ---------- | ------------------- | ----------------- |
| READ UNCOMMITTED | Possible   | Possible            | Possible          |
| READ COMMITTED   | No         | Possible            | Possible          |
| REPEATABLE READ  | No         | No                  | Possible (PG: no) |
| SERIALIZABLE     | No         | No                  | No                |

**When to use:**

- **READ COMMITTED** — Postgres default; good for most OLTP.
- **SERIALIZABLE** — financial invariants, seat booking when race conditions matter.

**Pluses and minuses:**

| + (higher isolation) | −                                 |
| -------------------- | --------------------------------- |
| Fewer anomalies      | More locking, retries, deadlocks  |
| Easier reasoning     | Lower throughput under contention |

**Examples:**

```sql
-- Postgres: explicit transaction
BEGIN ISOLATION LEVEL REPEATABLE READ;
  SELECT balance FROM accounts WHERE id = 1;
  UPDATE accounts SET balance = balance - 50 WHERE id = 1;
COMMIT;
```

**Thoughts:** Postgres uses **MVCC** (Multi-Version Concurrency Control) — readers don't block writers. Watch for **deadlocks** (`deadlock_detected`) and **serialization failures** (retry with backoff).

**Sources:**

[PostgreSQL — Transaction isolation](https://www.postgresql.org/docs/current/transaction-iso.html)

---

</details>

<details>
  <summary>Normal forms (1NF–3NF, BCNF)</summary>

### Normal forms (1NF–3NF, BCNF)

**Definition:** Normalization organizes relations so each fact is stored once, reducing redundancy and insert/update/delete anomalies. Codd defined 1NF (1970) then 2NF/3NF (1971); Boyce–Codd (BCNF) followed in 1974. Informal target for OLTP: **3NF**.

**How it works:**

A **functional dependency** `X → Y` means the values of `X` determine `Y`. **Prime** attributes belong to some candidate key. Each form is stricter than the previous.

| Form     | Informal rule                                                                | Typical fix                                   |
| -------- | ---------------------------------------------------------------------------- | --------------------------------------------- |
| **1NF**  | Atomic cells — no repeating groups, no nested tables in a column             | One value per cell; extra rows or child table |
| **2NF**  | 1NF + every non-key column depends on the **whole** candidate key            | Split partial deps (composite key → subset)   |
| **3NF**  | 2NF + non-key columns do not depend on other non-key columns (no transitive) | Move `instructor_office` off the course row   |
| **BCNF** | Every non-trivial FD `X → Y` has `X` as a **superkey** (stricter 3NF)        | Split overlapping composite keys              |

Kent (interview mnemonic): _each non-key field is a fact about the key, the whole key, and nothing but the key._

update the same city in many rows and miss one; cannot insert a course until a student enrolls; deleting the last student also deletes the course.

**When to use:**

Schema design and “why so many tables?” interviews. Stop at 3NF for most apps. BCNF when overlapping keys still allow contradictory facts. 4NF+ (independent multi-valued facts) is rare in interviews.

OLTP writes, money/inventory, shared reference data (`users`, `products`).

Read models, checkout snapshots, caches — see [Normalization vs denormalization](#normalization-vs-denormalization). Postgres `ARRAY` / `JSONB` are valid when the value is a document, not a disguised 1:N table.

**Pluses and minuses:**

| +                                 | −                                               |
| --------------------------------- | ----------------------------------------------- |
| One place to update a fact        | More JOINs and FKs at read time                 |
| Clear entities (user, order, sku) | SQL still allows arrays/JSONB that skip 1NF     |
| Integrity via keys, not copies    | Over-normalize (6NF-style) explodes table count |

**Examples:**

```sql
-- UNF: repeating group in one cell
-- orders(id, products)  -- 'Book, Pen'

-- 1NF: atomic, but product_name depends only on product_id (partial of composite key)
-- order_lines(order_id, product_id, product_name, qty)  PK (order_id, product_id)

-- 2NF: product facts in their own table
CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL
);
CREATE TABLE order_items (
  order_id INT REFERENCES orders(id),
  product_id INT REFERENCES products(id),
  qty INT NOT NULL,
  PRIMARY KEY (order_id, product_id)
);

-- 3NF: city depends on customer, not on order_id
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  city TEXT NOT NULL
);
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  customer_id INT REFERENCES customers(id)
);
```

**Thoughts:**

- 2NF is automatic if every table has a **single-column** primary key — partial dependency needs a composite key.
- BCNF ⊂ 3NF: a table can be 3NF (all non-prime attrs depend only on keys) but not BCNF if a **non-superkey** determines part of a key. Decomposition to BCNF is not always dependency-preserving.
- “Normalized” in conversation usually means **3NF**, not 6NF.

**Sources:**

[Database normalization (Wikipedia)](https://en.wikipedia.org/wiki/Database_normalization) · [First normal form](https://en.wikipedia.org/wiki/First_normal_form) · [BCNF](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) · Codd, _A Relational Model of Data for Large Shared Data Banks_ (1970)

---

</details>

<details>
  <summary>Normalization vs denormalization</summary>

### Normalization vs denormalization

**Definition:** **Normalization** splits data into related tables to reduce redundancy (forms: [1NF–3NF, BCNF](#normal-forms-1nf3nf-bcnf)). **Denormalization** duplicates data for faster reads.

**How it works:**

OLTP, frequently updated entities, strong integrity (users, orders, payments).

Read-heavy analytics, embedded documents (order snapshot at checkout), cache tables, CQRS read models.

**Pluses and minuses:**

| + (normalized)            | − (normalized)                 |
| ------------------------- | ------------------------------ |
| No update anomalies       | More JOINs at read time        |
| Smaller storage footprint | Complex queries for dashboards |

| + (denormalized)              | − (denormalized)                       |
| ----------------------------- | -------------------------------------- |
| Faster reads, fewer JOINs     | Update anomalies — must sync copies    |
| Better for read-heavy reports | More application logic for consistency |

**Examples:**

```sql
-- 3NF — name lives once in products
CREATE TABLE products (id SERIAL PRIMARY KEY, name TEXT, price NUMERIC(10,2));
CREATE TABLE order_items (
  id SERIAL PRIMARY KEY,
  order_id INT REFERENCES orders(id),
  product_id INT REFERENCES products(id),
  qty INT
);

-- Denormalized snapshot — freeze name/price at purchase (catalog can change later)
CREATE TABLE order_items (
  id SERIAL PRIMARY KEY,
  order_id INT,
  product_id INT,
  product_name TEXT,
  unit_price NUMERIC(10,2),
  qty INT
);
```

**Thoughts:** Target 3NF for writes. Denormalize **after** measuring slow queries, not upfront. Copies must be updated together or treated as immutable history.

**Sources:**

[Use The Index, Luke — Analyzing](https://use-the-index-luke.com/sql/anatomy) · [Database normalization](https://en.wikipedia.org/wiki/Database_normalization)

---

</details>

<details>
  <summary>CRUD</summary>

### CRUD

**Definition:** **Create, Read, Update, Delete** — the four basic persistence operations every data-backed API implements.

**When to use:** REST resources, admin panels, simple entity management.

**Pluses and minuses:**

| +                                          | −                                                    |
| ------------------------------------------ | ---------------------------------------------------- |
| Universal vocabulary across stacks         | Hides complex domain operations behind generic verbs |
| Maps cleanly to HTTP (POST/GET/PUT/DELETE) | Bulk/batch operations don't fit neatly               |

**Examples:**

```ts
// Repository pattern — one place for CRUD
type UserRepo = {
  create(data: CreateUserDto): Promise<User>;
  findById(id: string): Promise<User | null>;
  update(id: string, data: UpdateUserDto): Promise<User>;
  delete(id: string): Promise<void>;
};
```

**Thoughts:** Domain actions (`placeOrder`, `refundPayment`) often span multiple tables — don't force everything into generic CRUD controllers.

**Sources:**

[MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

---

</details>

<details>
  <summary>Migrations & seeds</summary>

### Migrations & seeds

**Definition:** **Migrations** version schema changes (up/down). **Seeds** insert reference or test data.

**When to use:** Every team project with a shared database. Never alter production schema by hand.

**Pluses and minuses:**

| +                                        | −                                                  |
| ---------------------------------------- | -------------------------------------------------- |
| Schema in version control alongside code | Bad migrations can lock large tables in production |
| Reproducible environments                | Rollback complexity on data migrations             |
| Team sync on schema changes              | Requires discipline in CI/CD                       |

**Examples:**

```prisma
// schema.prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  createdAt DateTime @default(now())
}
```

```bash
npx prisma migrate dev --name init
npx prisma db seed
```

#### Sequelize migration & seed commands

### Sequelize migration & seed commands

```bash
npx sequelize-cli migration:generate --name create-users
npx sequelize-cli db:migrate
npx sequelize-cli db:migrate:undo
npx sequelize-cli seed:generate --name demo-users
npx sequelize-cli db:seed:all
```

Migration file shape:

```js
module.exports = {
  async up(queryInterface, Sequelize) {
    await queryInterface.createTable("Users", {
      /* ... */
    });
  },
  async down(queryInterface) {
    await queryInterface.dropTable("Users");
  },
};
```

**Thoughts:**

- Prefer **backward-compatible** migrations (add column nullable → backfill → add NOT NULL).
- Large table changes: use `CONCURRENTLY` indexes in Postgres, avoid blocking `ALTER` in peak hours.
- Seeds are for dev/staging — never run destructive seeds in production.

**Sources:** [Prisma Migrate](https://www.prisma.io/docs/orm/prisma-migrate)

---

</details>
