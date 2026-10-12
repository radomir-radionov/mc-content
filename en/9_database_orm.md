# Database — ORM

Prisma, Sequelize, Knex, Drizzle, and query builders.

[← Back to hub](./9_database.md)

### Related docs

- [DB — Core](./9_database_core.md) · [DB — SQL](./9_database_sql.md) · [DB — NoSQL](./9_database_nosql.md) · [DB — ORM](./9_database_orm.md) · [DB — Production](./9_database_production.md) · [DB — Interview](./9_database_interview.md)

### Sections

- [ORM & application layer](#orm-application-layer)
- [Query builder](#query-builder)

---

## ORM & application layer

<details>
  <summary>ORM (concept)</summary>

### ORM (concept)

**Definition:** **Object-Relational Mapping** — maps database tables to application objects, generating SQL from method calls.

**How it works:** Heavy reporting, bulk ETL, performance-critical analytics — use raw SQL or query builder.

**When to use:** Standard CRUD APIs, rapid development, teams that benefit from schema-as-code.

**Pluses and minuses:**

| +                                | −                                               |
| -------------------------------- | ----------------------------------------------- |
| Less boilerplate SQL             | Abstraction leaks — must know SQL anyway        |
| Type-safe models (with TS)       | N+1 queries, inefficient generated SQL          |
| Migrations integrated            | Hard to optimize complex reports                |
| Database portability (in theory) | Portability breaks on dialect-specific features |

**Examples:**

```ts
// TypeORM / Sequelize / Prisma — same idea
const user = await prisma.user.create({
  data: { email: "a@b.com", posts: { create: [{ title: "Hello" }] } },
  include: { posts: true },
});
```

**Thoughts:** ORM is a **productivity tool**, not a performance guarantee. Profile queries in staging with real data volume.

**Sources:**

[Prisma — Data model](https://www.prisma.io/docs/orm/prisma-schema/data-model)

---

</details>

<details>
  <summary>Prisma</summary>

### Prisma

**Definition:** Next-gen ORM for Node.js/TypeScript — schema-first, type-safe client, migrations, and Prisma Studio.

**When to use:** New TypeScript backends, teams wanting schema-as-single-source-of-truth.

**Pluses and minuses:**

| +                                        | −                                            |
| ---------------------------------------- | -------------------------------------------- |
| Excellent TypeScript inference           | Raw SQL escape hatches needed for edge cases |
| Declarative schema, clean migrations     | Cold start / bundle size in serverless       |
| Relation queries with `include`/`select` | Not ideal for very dynamic schemas           |
| Active ecosystem and docs                | Vendor coupling (mitigated by SQL export)    |

**Examples:**

```prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@index([email])
}

model Post {
  id       String @id @default(cuid())
  title    String
  author   User   @relation(fields: [authorId], references: [id])
  authorId String
}
```

```ts
const users = await prisma.user.findMany({
  where: { email: { contains: "@acme.com" } },
  include: { posts: { take: 5, orderBy: { createdAt: "desc" } } },
});
```

**Thoughts:**

- Use `prisma.$transaction([...])` for multi-step atomicity.
- `$queryRaw` with tagged template for safe parameterized raw SQL.
- Connection pooling: use PgBouncer in transaction mode or Prisma Accelerate for serverless.

**Sources:**

[Prisma documentation](https://www.prisma.io/docs)

---

</details>

<details>
  <summary>Sequelize</summary>

### Sequelize

**Definition:** Promise-based ORM for Node.js supporting Postgres, MySQL, SQLite, MSSQL — models, migrations, associations.

**When to use:** Legacy Node projects, teams already on Sequelize, multi-DB portability requirements.

**Pluses and minuses:**

| +                         | −                                     |
| ------------------------- | ------------------------------------- |
| Mature, wide DB support   | Verbose API vs Prisma                 |
| CLI for migrations/seeds  | TypeScript support weaker than Prisma |
| Flexible hooks and scopes | Easy to write N+1 queries             |

**Examples:**

```js
const User = sequelize.define("User", { email: DataTypes.STRING });
const Post = sequelize.define("Post", { title: DataTypes.STRING });
User.hasMany(Post);
Post.belongsTo(User);

const users = await User.findAll({ include: Post });
```

#### Sequelize CLI commands

### Sequelize CLI commands

```bash
npx sequelize-cli model:generate --name User --attributes email:string
npx sequelize-cli db:migrate
npx sequelize-cli db:migrate:undo
npx sequelize-cli migration:generate --name add-index-to-users
npx sequelize-cli db:migrate:status
```

**Thoughts:** Prefer `include` with `attributes` limit. Use `sequelize.transaction()` for multi-table updates.

**Sources:** [Sequelize docs](https://sequelize.org/docs/v6/)

---

</details>

<details>
  <summary>Drizzle ORM</summary>

### Drizzle ORM

**Definition:**

Lightweight TypeScript ORM with SQL-like schema and queries, minimal runtime, and excellent type inference.

|             | Prisma        | Drizzle            |
| ----------- | ------------- | ------------------ |
| Schema      | `.prisma` DSL | TypeScript tables  |
| SQL control | Generated     | Explicit, SQL-like |
| Bundle size | Larger        | Smaller            |

**When to use:** TypeScript backends needing type safety with SQL transparency; serverless with tight bundle limits.

**Examples:**

```ts
import { drizzle } from "drizzle-orm/node-postgres";
import { pgTable, text } from "drizzle-orm/pg-core";

export const users = pgTable("users", {
  id: text("id").primaryKey(),
  email: text("email").notNullable().unique(),
});

const rows = await db.select().from(users).where(eq(users.email, "a@b.com"));
```

**Sources:** [Drizzle ORM docs](https://orm.drizzle.team/docs/overview)

</details>

<details>
  <summary>Raw SQL vs ORM vs Query Builder</summary>

### Raw SQL vs ORM vs Query Builder

**Definition:**

Three ways to talk to the database from application code.

| Approach                    | Best for                                 | Watch out for                               |
| --------------------------- | ---------------------------------------- | ------------------------------------------- |
| **Raw SQL**                 | Reports, bulk ops, DB-specific features  | SQL strings, injection if not parameterized |
| **ORM** (Prisma, Sequelize) | CRUD, relations, rapid delivery          | N+1, opaque generated SQL                   |
| **Query builder** (Knex)    | Dynamic filters, migrations, SQL control | Still string-building if careless           |

| Pros (ORM)              | Pros (raw SQL)                       |
| ----------------------- | ------------------------------------ |
| Type safety, migrations | Full optimizer control               |
| Less SQL boilerplate    | Best performance for complex queries |

**When to use:** ORM for 80% of app code; raw SQL for the 20% that profiles slow; Knex when you need programmatic query composition without full ORM.

**Thoughts:** Hybrid is normal — Prisma for CRUD + `$queryRaw` for reports. Never choose "ORM only" or "SQL only" as ideology.

**Sources:**

[Prisma — Raw queries](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql)

---

</details>

<details>
  <summary>N+1 problem</summary>

### N+1 problem

**Definition:** One query loads N parent records, then N additional queries load each child — 1 + N round trips.

**When to use:** Always fix when listing entities with relations (users + posts, orders + items).

**Pluses and minuses:**

| + (of fixing it)          | −                                 |
| ------------------------- | --------------------------------- |
| Massive latency reduction | Eager loading can over-fetch data |
| Fewer DB connections used | JOIN queries can be complex       |

**Examples:**

```ts
// BAD — N+1
const users = await prisma.user.findMany();
for (const u of users) {
  u.posts = await prisma.post.findMany({ where: { authorId: u.id } });
}

// GOOD — single query with JOIN
const users = await prisma.user.findMany({ include: { posts: true } });

// GOOD — raw SQL
// SELECT u.*, p.* FROM users u LEFT JOIN posts p ON p.author_id = u.id
```

**Thoughts:** Enable query logging in dev (`prisma.$on('query')`, Sequelize `logging: console.log`). One `include` with deep nesting can fetch too much — use `select` to limit columns.

**Sources:**

[Prisma — Relation queries](https://www.prisma.io/docs/orm/prisma-client/queries/relation-queries)

---

</details>

<details>
  <summary>Connection pooling</summary>

### Connection pooling

**Definition:** Reuse open database connections instead of opening a new TCP + auth handshake per request.

**When to use:** Every production Node app. Postgres default `max_connections` (~100) is easily exhausted without pooling.

**Pluses and minuses:**

| +                                   | −                                            |
| ----------------------------------- | -------------------------------------------- |
| Lower latency, fewer DB connections | Pool misconfiguration = timeouts             |
| Protects DB from connection storms  | Serverless needs external pooler (PgBouncer) |

**Examples:**

```ts
// node-postgres Pool
import { Pool } from "pg";
const pool = new Pool({ max: 20, connectionString: process.env.DATABASE_URL });

// Prisma — set connection_limit in URL or use PgBouncer
// DATABASE_URL="postgresql://...?connection_limit=10"
```

**Thoughts:**

- **PgBouncer** in transaction pooling mode for serverless/high-instance count.
- Pool size ≈ `(CPU cores * 2) + spindle count` rule of thumb — measure, don't guess.
- Always close pool on graceful shutdown.

**Sources:**

[node-postgres — Pooling](https://node-postgres.com/features/pooling)

---

</details>

<a name="query-builder"></a>

## Query builder

<details>
  <summary>Knex.js</summary>

### Knex.js

**Definition:** SQL query builder for Node.js — fluent API, migrations, and seed support without full ORM mapping.

**When to use:** Admin dashboards with dynamic filters, projects needing SQL transparency, legacy codebases between raw SQL and ORM.

**Pluses and minuses:**

| +                                  | −                                     |
| ---------------------------------- | ------------------------------------- |
| Control over generated SQL         | No automatic relation mapping         |
| Database-agnostic queries (mostly) | Migrations less ergonomic than Prisma |
| Great for dynamic filters          | You manage models/types yourself      |

**Examples:**

```js
const rows = await knex("orders")
  .join("users", "orders.user_id", "users.id")
  .where("orders.status", "pending")
  .where("users.email", "like", "%@acme.com")
  .select("orders.id", "orders.total", "users.email")
  .orderBy("orders.created_at", "desc")
  .limit(20);
```

```js
// Migration
exports.up = (knex) =>
  knex.schema.createTable("users", (t) => {
    t.increments("id").primary();
    t.string("email").notNullable().unique();
    t.timestamps(true, true);
  });
```

**Thoughts:** Knex uses parameterized bindings by default — safe from injection. Pair with `objection.js` if you want ORM on top of Knex.

**Sources:**

[Knex.js documentation](https://knexjs.org/)

---

</details>

<a name="production"></a>
