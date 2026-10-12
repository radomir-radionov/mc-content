# Database — MongoDB

Document database, aggregation, and Mongoose in Node.js.

[← Back to NoSQL hub](./9_database_nosql.md)

### Related docs

[NoSQL — Core](./9_database_nosql_core.md) · [NoSQL — MongoDB](./9_database_mongodb.md) · [NoSQL — Redis](./9_database_redis.md) · [NoSQL — ClickHouse](./9_database_clickhouse.md)

### Sections

- [MongoDB](#mongodb)

---

## MongoDB

<details>
  <summary>MongoDB</summary>

### MongoDB

**Definition:** Document-oriented database — stores BSON (binary JSON) **documents** in **collections**. One document can embed nested objects and arrays.

**Why we need it:** Relational tables need many columns or join tables when every product has different attributes. MongoDB stores each product as one document `{ name, specs: { ... } }` without migrations for every new field.

**How it works:**

| Concept         | Role                                                |
| --------------- | --------------------------------------------------- |
| **Database**    | Namespace (like a Postgres database)                |
| **Collection**  | Group of documents (like a table, no fixed columns) |
| **Document**    | One JSON-like record, max 16 MB                     |
| **Replica set** | Primary + secondaries, automatic failover           |
| **Shard**       | Horizontal split by shard key                       |

**When to use:**

| Use case                | Why MongoDB                                        |
| ----------------------- | -------------------------------------------------- |
| **Product catalog**     | Different attributes per category (phone vs shirt) |
| **Content / CMS**       | Nested blocks, drafts, versions in one doc         |
| **User profiles**       | Embedded preferences, addresses                    |
| **Mobile offline sync** | Document maps cleanly to app state                 |

**Advantages:**

| Advantage                   | What you gain                                        |
| --------------------------- | ---------------------------------------------------- |
| **Flexible schema**         | Add fields without `ALTER TABLE`                     |
| **Nested data in one read** | `orders` with `items[]` — no join                    |
| **Aggregation pipeline**    | Server-side transforms like SQL `GROUP BY` + filters |
| **Horizontal sharding**     | Scale writes across nodes                            |

**Pluses and minuses:**

| +                                | −                                                    |
| -------------------------------- | ---------------------------------------------------- |
| Fast iteration on document shape | 16 MB document limit                                 |
| Replica sets for HA              | Multi-doc transactions only since 4.0 (with caveats) |
| Rich aggregation                 | Large aggregations can use lots of RAM               |

**Examples:**

```js
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $group: { _id: "$userId", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } },
  { $limit: 10 },
]);
```

**Thoughts:** Embed data read together; **reference** by `userId` when arrays grow unbounded. Index fields used in `find()` and `sort()`.

**Sources:** [MongoDB manual](https://www.mongodb.com/docs/manual/)

---

</details>

<details>
  <summary>Mongoose</summary>

### Mongoose

**Definition:** ODM for MongoDB in Node.js — schemas, validation, middleware, `populate()`.

**When to use:** Node apps that want typed models and validation on top of flexible documents.

**Pluses and minuses:**

| +                                 | −                           |
| --------------------------------- | --------------------------- |
| Schema enforcement on flexible DB | Know MongoDB queries anyway |
| Hooks (`pre('save')`)             | `populate()` can cause N+1  |
| TypeScript generics               | Heavy for trivial CRUD      |

**Examples:**

```ts
const userSchema = new Schema(
  { email: { type: String, required: true, unique: true }, name: String },
  { timestamps: true },
);
const User = model("User", userSchema);
await User.findOne({ email }).lean();
```

**Sources:** [Mongoose docs](https://mongoosejs.com/docs/guide.html)

---

</details>
