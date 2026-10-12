# Database — SQL

PostgreSQL, SQL Server (T-SQL), joins, indexes, and execution plans.

[← Back to hub](./9_database.md)

### Related docs

- [DB — Core](./9_database_core.md) · [DB — SQL](./9_database_sql.md) · [DB — NoSQL](./9_database_nosql.md) · [DB — ORM](./9_database_orm.md) · [DB — Production](./9_database_production.md) · [DB — Interview](./9_database_interview.md) · [Web security](./7_special_security.md)

### Main concepts

| #     | Topics                                                                                                                            |
| ----- | --------------------------------------------------------------------------------------------------------------------------------- |
| 1–10  | SQL, DDL/DML/DCL, primary key, foreign key, normalization, joins, subqueries, DISTINCT, WHERE vs HAVING, indexes                  |
| 11–20 | ACID, triggers, VIEW, LIKE, aggregates, UNION, procedures, EXPLAIN (Postgres + SQL Server), constraints, SQL injection            |
| 21–30 | Relational DB, INNER vs OUTER, NULL, CASE, COMMIT/ROLLBACK, CHAR vs VARCHAR, temp tables, windows, CTE, DROP TABLE                |
| 31–40 | ALTER TABLE, correlated subquery, DELETE/TRUNCATE/DROP, self-join, backup, many-to-many, GRANT/REVOKE, hashing, how triggers work |
| 41–42 | T-SQL dialect (SQL Server), SQL Server indexes (clustered / nonclustered)                                                         |

### Sections

- [Topics](#topics)

---

## Topics

<details>
  <summary>SQL</summary>

### SQL

**Definition:** **Structured Query Language** — declarative language for defining and querying relational data. You describe _what_ data you want; the optimizer decides _how_.

**How it works:**

SQL is the interface to a relational catalog. Typical jobs:

- Define objects: databases, tables, types, indexes (`CREATE` / `ALTER` / `DROP`).
- Change rows: `INSERT`, `UPDATE`, `DELETE`.
- Read: `SELECT` with `JOIN`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`.
- Control access: `GRANT` / `REVOKE`.
- Group statements: `BEGIN` / `COMMIT` / `ROLLBACK`.

Command families: [DDL, DML, and DCL](#ddl-dml-and-dcl). Dialects share the core; Postgres, MySQL, and SQL Server diverge on types, upsert, and functions.

**When to use:** Structured data with relationships, reporting, transactions, ad-hoc analytics.

**Pluses and minuses:**

| +                                  | −                                       |
| ---------------------------------- | --------------------------------------- |
| Declarative, portable across RDBMS | Dialect differences (Postgres vs MySQL) |
| Mature ecosystem, tooling          | Complex queries become hard to maintain |
| Strong consistency and joins       | Vertical scaling limits on single node  |

**Examples:**

```sql
SELECT u.email, COUNT(o.id) AS order_count
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
WHERE u.created_at >= '2025-01-01'
GROUP BY u.id, u.email
HAVING COUNT(o.id) > 5
ORDER BY order_count DESC
LIMIT 10;
```

**Thoughts:** Apps connect via **drivers** (`pg`, `mysql2`). The query planner uses statistics — stale stats = bad plans. Always parameterize user input.

**Sources:** [PostgreSQL — SQL commands](https://www.postgresql.org/docs/current/sql-commands.html) · [SQL Academy — Interview questions](https://sql-academy.org/ru/interview-questions) · [Habr — SQL interview questions, part 1](https://habr.com/ru/articles/790850/) · [Habr — SQL interview questions, part 2](https://habr.com/ru/articles/791208/) · [Habr — Top-65 SQL interview questions, part I](https://habr.com/ru/companies/otus/articles/461067/)

---

</details>

<details>
  <summary>DDL, DML, and DCL</summary>

### DDL, DML, and DCL

**Definition:** SQL statements group by job: **DDL** changes schema, **DML** changes or reads rows, **DCL** grants and revokes privileges.

**How it works:**

| Group                         | Job                       | Typical commands                                 |
| ----------------------------- | ------------------------- | ------------------------------------------------ |
| **DDL** (Data Definition)     | Create/alter/drop objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT` |
| **DML** (Data Manipulation)   | Work with rows            | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE`  |
| **DCL** (Data Control)        | Privileges                | `GRANT`, `REVOKE`                                |
| **TCL** (Transaction Control) | Commit or abort           | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`       |

Interview lists often put `SELECT` in DML. The SQL standard also calls read-only queries **DQL** — say both if asked.

`TRUNCATE` is a table-level DDL command in PostgreSQL, not a row-by-row `DELETE`.

**When to use:** Name the group when explaining migrations (DDL) vs app queries (DML) vs roles (DCL).

**Thoughts:** Migrations should be DDL in versioned files. App users should not hold DDL rights in production.

**Sources:** [PostgreSQL — SQL commands](https://www.postgresql.org/docs/current/sql-commands.html) · [PostgreSQL — Privileges](https://www.postgresql.org/docs/current/ddl-priv.html)

</details>

<details>
  <summary>Primary key</summary>

### Primary key

**Definition:** Primary key is the unique identifier of a row in a table. It allows no duplicates and no NULL values.

**How it works:**

- One primary key per table.
- May be one column or several (`PRIMARY KEY (a, c)` — composite key). Composite columns are all `NOT NULL`.
- Implies `UNIQUE` + `NOT NULL`. Extra unique columns use `UNIQUE`, not a second PK.
- Postgres creates a unique B-tree index for the primary key.

**When to use:** Every entity table (`students`, `courses`). Surrogate `id` is the usual app PK; a natural key can be `UNIQUE` beside it.

**Examples:**

```sql
CREATE TABLE students (
  student_id INT PRIMARY KEY,
  name VARCHAR(100),
  age INT
);

-- Composite primary key
CREATE TABLE example (
  a INT,
  b INT,
  c INT,
  PRIMARY KEY (a, c)
);
```

**Thoughts:** Interview line: “identifies the row, unique, never NULL, maybe composite.” Default target of `REFERENCES` / [Foreign key](#foreign-key). Other constraints: [Schema constraints](#schema-constraints-fk-check-unique).

**Sources:** [PostgreSQL — Primary keys](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-PRIMARY-KEYS)

</details>

<details>
  <summary>Foreign key</summary>

### Foreign key

**Definition:** Foreign key is a constraint that links a column (or set of columns) in one table to a column (or columns) in another table — usually the [primary key](#primary-key), or another unique key.

It enforces **referential integrity**: every non-NULL foreign-key value must exist in the referenced table.

**How it works:**

#### Restricts invalid inserts

- You cannot insert (or update) a child value that is not in the parent table.

#### Restricts deleting referenced parents

- You cannot delete a parent row while children still point at it, unless you add an action (`ON DELETE`).
- Default is `NO ACTION` (error at check time). `RESTRICT` is stricter (not deferrable). `CASCADE` deletes children. `SET NULL` / `SET DEFAULT` rewrite the FK instead.
- Same family exists for `ON UPDATE`.

`REFERENCES parent` with no column list uses the parent’s primary key. Self-FK (tree): [Self-join](#self-join). Junction tables: [Many-to-many](#many-to-many). Other constraint types: [Schema constraints](#schema-constraints-fk-check-unique).

**When to use:** Every real relationship (`employees.department_id → departments.department_id`). Skip only for denormalized snapshots you accept can dangle.

**Examples:**

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments (department_id)
);
```

**Thoughts:** Postgres does **not** auto-index the referencing FK column — add one for joins and deletes.

**Sources:** [PostgreSQL — Foreign keys](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK)

</details>

<details>
  <summary>Normalization</summary>

### Normalization

**Definition:** Normalization stores each fact once so updates cannot diverge. Full forms (1NF–3NF, BCNF): [Normal forms](./9_database_core.md#normal-forms-1nf3nf-bcnf).

**How it works:**

Main goals:

- **Remove redundancy** — stop storing the same fact in many rows.
- **Raise integrity** — one place to update a fact, so copies cannot drift.
- **Easier maintenance** — a clearer schema that is simpler to change.

Main normal forms (each builds on the previous):

| Form    | Rule                                                                            | Example                                                                                                                              |
| ------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **1NF** | Every column holds atomic (indivisible) values — no repeating groups in a cell  | Not 1NF: `courses = "Math, Music"` in one cell. 1NF: one course per row.                                                             |
| **2NF** | Satisfies 1NF, and every non-key column depends on the **whole** primary key    | PK `(student_id, course_id)` still storing `student_name` — name depends only on `student_id`. Move name to `students`.              |
| **3NF** | Satisfies 2NF, and there are no transitive dependencies between non-key columns | `courses` has `instructor` and `instructor_office` — office depends on instructor, not on `course_id`. Move office to `instructors`. |

Wide row `student + course` ends in `students`, `courses`, and [enrollments](#many-to-many).

**When to use:** OLTP schemas, interview “why so many tables?”. Stop at 3NF for most apps. Denormalize on purpose for read models — [Normalization vs denormalization](./9_database_core.md#normalization-vs-denormalization).

**Examples:**

**1NF — atomic cells**

Before: two courses in one cell.

| StudentID | StudentName | Courses     |
| --------- | ----------- | ----------- |
| 1         | John        | Math, Music |

After: one value per cell (one course per row).

| StudentID | StudentName | CourseID | CourseName |
| --------- | ----------- | -------- | ---------- |
| 1         | John        | 101      | Math       |
| 1         | John        | 102      | Music      |

**2NF — depend on the whole key**

Before: 1NF, but `StudentName` depends only on `StudentID`, not on `(StudentID, CourseID)`. “John” is copied on every course row.

| StudentID | StudentName | CourseID | CourseName |
| --------- | ----------- | -------- | ---------- |
| 1         | John        | 101      | Math       |
| 1         | John        | 102      | Music      |

After: name lives on `Students`. The link table keeps only the pair of keys plus course facts that still belong on the line.

**Students**

| StudentID | StudentName |
| --------- | ----------- |
| 1         | John        |

**Enrollments**

| StudentID | CourseID | CourseName |
| --------- | -------- | ---------- |
| 1         | 101      | Math       |
| 1         | 102      | Music      |

**3NF — no transitive dependency**

`CourseName` depends on `CourseID`, not on the enrollment pair — same idea as instructor office depending on instructor, not on the course row.

Before (office repeats because it follows Ada, not the course):

| CourseID | CourseName | Instructor | Office  |
| -------- | ---------- | ---------- | ------- |
| 101      | Math       | Ada        | Room 12 |
| 102      | CS         | Ada        | Room 12 |

After:

**Instructors**

| Instructor | Office  |
| ---------- | ------- |
| Ada        | Room 12 |

**Courses**

| CourseID | CourseName | Instructor |
| -------- | ---------- | ---------- |
| 101      | Math       | Ada        |
| 102      | CS         | Ada        |

The John / Math / Music case after 3NF is three tables: `Students`, `Courses`, `Enrollments` (ids only) — see [Many-to-many](#many-to-many).

**Students**

| StudentID | StudentName |
| --------- | ----------- |
| 1         | John        |

**Courses**

| CourseID | CourseName |
| -------- | ---------- |
| 101      | Math       |
| 102      | Music      |

**Enrollments**

| StudentID | CourseID |
| --------- | -------- |
| 1         | 101      |
| 1         | 102      |

**Thoughts:** M:N after 1NF is a junction table, not a repeating course list on the student row.

**Sources:** [PostgreSQL — Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html) · [Database normalization](https://en.wikipedia.org/wiki/Database_normalization) · [SQL Academy — Database normalization](https://sql-academy.org/ru/blog/database-normalization)

</details>

<details>
  <summary>Joins</summary>

### Joins

**Definition:** JOIN combines rows from two or more tables using related columns.

**How it works:**

The optimizer picks nested loop, hash join, or merge join from stats and indexes. The match rule lives in `ON` (or `USING`). A `WHERE` filter on the inner side of an outer join can drop unmatched rows and turn it into an inner join.

Kinds of JOIN:

- **INNER JOIN** — only rows that have a match in **both** tables. No match → the row is omitted.
- **LEFT JOIN** (LEFT OUTER JOIN) — every row from the **left** table, plus matching right-table columns. No match → right columns are NULL.
- **RIGHT JOIN** (RIGHT OUTER JOIN) — every row from the **right** table, plus matching left-table columns. No match → left columns are NULL.
- **FULL OUTER JOIN** (FULL JOIN) — every row that matches in either table. Unmatched sides are NULL.
- **CROSS JOIN** — Cartesian product: every row of the first table paired with every row of the second. No `ON` condition.

`LEFT` / `RIGHT` / `FULL` already mean OUTER; the word `OUTER` is optional. INNER vs OUTER in one place: [INNER JOIN vs OUTER JOIN](#inner-join-vs-outer-join). Same table twice: [Self-join](#self-join).

| Join type      | Returns                                           |
| -------------- | ------------------------------------------------- |
| **INNER**      | Rows with matches in both tables                  |
| **LEFT**       | All left rows + matching right (NULL if no match) |
| **RIGHT**      | Mirror of LEFT                                    |
| **FULL OUTER** | All rows from both, NULLs where no match          |
| **CROSS**      | Cartesian product — every left × every right      |

Predicate style: `ON expr`, `USING (col)` (shared column once), `NATURAL` (join all same-named columns — fragile).

**When to use:**

INNER for required relationships; LEFT when optional related data (e.g. user without orders). RIGHT is the same as swapping tables and LEFT — prefer LEFT. `EXISTS` often beats `IN` for large subqueries.

**Pluses and minuses:**

| +                                 | −                                                  |
| --------------------------------- | -------------------------------------------------- |
| Normalized schema, no duplication | Expensive on large tables without indexes          |
| Single query for related data     | Wrong join type = silent data loss (INNER vs LEFT) |

**Examples:**

```sql
-- Users with at least one order (INNER)
SELECT u.id, u.email, o.total
FROM users u
INNER JOIN orders o ON o.user_id = u.id;

-- All users, order total or NULL (LEFT)
SELECT u.id, u.email, o.total
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;

-- RIGHT: all departments, even with no employees
SELECT d.name, e.name AS employee
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;

-- FULL OUTER
SELECT *
FROM employees e
FULL JOIN departments d ON e.department_id = d.id;

-- CROSS: every employee × every department
SELECT e.name, d.name AS department
FROM employees e
CROSS JOIN departments d;

-- EXISTS — often faster than IN for large subqueries
SELECT u.id FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);
```

**Thoughts:** Prefer `EXISTS` over `IN (SELECT ...)` for correlated checks. Avoid `SELECT *` in JOINs — fetch only needed columns. `NATURAL JOIN` breaks when a new same-named column appears. Same table twice: [Self-join](#self-join).

**Sources:**

[PostgreSQL — Joins](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN) · [Use The Index, Luke — Joins](https://use-the-index-luke.com/sql/join)

---

</details>

<details>
  <summary>Subqueries</summary>

### Subqueries

**Definition:** Subquery is a SQL query nested inside another query. You run it so the outer query can use its result.

**How it works:**

The inner `SELECT` is evaluated (once, unless it is [correlated](#correlated-subquery)) and the outer statement uses that result.

Typical places:

- **Filter** — subquery in `WHERE` or `HAVING` (`=`, `IN`, `EXISTS`).
- **Select list** — scalar subquery as a column (must return one row and one column).
- **Derived table** — subquery in `FROM`, in parentheses, with an alias (a virtual table).

Named form of a subquery: [CTE](#cte).

**When to use:** When the outer query needs a computed set or scalar (department id for `'IT'`, a grouped count). Prefer a JOIN when both sides are large and you need many columns.

**Examples:**

```sql
-- WHERE: employees in the IT department
SELECT name
FROM employees
WHERE department_id = (SELECT id FROM departments WHERE name = 'IT');

-- SELECT list: scalar subquery as a column
SELECT
  e.name,
  (SELECT d.name FROM departments d WHERE d.id = e.department_id) AS department
FROM employees e;

-- FROM: derived table (must have an alias)
SELECT sub.department, COUNT(*)
FROM (
  SELECT department_id AS department
  FROM employees
) AS sub
GROUP BY sub.department;

SELECT u.id FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);
```

**Thoughts:** A correlated subquery is not always slow — `EXISTS` can short-circuit. Measure with [EXPLAIN](#query-optimization-explain).

**Sources:** [PostgreSQL — Subquery expressions](https://www.postgresql.org/docs/current/functions-subquery.html) · [PostgreSQL — Table expressions (FROM)](https://www.postgresql.org/docs/current/queries-table-expressions.html) · [SQL Academy — Correlated subqueries](https://sql-academy.org/ru/guide/correlated-subqueries)

</details>

<details>
  <summary>DISTINCT</summary>

### DISTINCT

**Definition:** `SELECT DISTINCT` returns unique rows for the selected column list — duplicates are collapsed after the result is computed.

**How it works:**

- Distinctness is over **all selected columns**, not one column unless that is the only column in the list.
- `DISTINCT ON (expr)` (PostgreSQL) keeps the first row per key after `ORDER BY`.
- Deduping large results is extra work — a unique index or `GROUP BY` may be clearer.

**When to use:** Distinct labels, unique user ids from a join that fans out. Skip when the join should not duplicate rows — fix the join instead.

**Examples:**

```sql
SELECT DISTINCT position FROM employees;

SELECT DISTINCT ON (user_id) user_id, created_at
FROM events
ORDER BY user_id, created_at DESC;
```

**Thoughts:** `DISTINCT` is not a substitute for a unique constraint. Prefer `GROUP BY` when you also aggregate.

**Sources:** [PostgreSQL — DISTINCT](https://www.postgresql.org/docs/current/sql-select.html#SQL-DISTINCT)

</details>

<details>
  <summary>WHERE vs HAVING</summary>

### WHERE vs HAVING

**Definition:** `WHERE` filters individual rows **before** grouping (no aggregates). `HAVING` filters groups **after** `GROUP BY` (aggregates allowed).

**How it works:**

Logical order: `FROM` / `JOIN` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`.

**WHERE**

- Runs **before** `GROUP BY`.
- Cannot use aggregate functions (`SUM()`, `COUNT()`, `AVG()`, …).
- Applies to single table (or join) rows.

**HAVING**

- Runs **after** `GROUP BY`.
- Can use aggregate functions.
- Applies to the grouped result, not to raw rows.
- Allowed without `GROUP BY` — then it filters the single overall aggregate row.

**When to use:** Row predicates (`salary > 50000`) in `WHERE` so indexes can apply. Group predicates (`COUNT(*) > 5`) in `HAVING`.

**Examples:**

```sql
SELECT department_id, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department_id;

SELECT department_id, COUNT(*) AS num_employees
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;
```

**Thoughts:** Filtering in `WHERE` is cheaper than aggregating rows you will throw away in `HAVING`.

**Sources:** [PostgreSQL — SELECT](https://www.postgresql.org/docs/current/sql-select.html)

</details>

<details>
  <summary>Indexes</summary>

### Indexes

**Definition:**

Data structures (usually **B-tree** in Postgres) that speed up lookups at the cost of write overhead and storage.

**How it works:**

The index stores ordered keys plus pointers (TIDs) into the heap. A matching `WHERE`/`JOIN`/`ORDER BY` can do an index scan instead of a sequential scan. Writes (`INSERT`/`UPDATE`/`DELETE`) must update every index on the changed columns.

| Index type           | Use case                                     |
| -------------------- | -------------------------------------------- |
| **B-tree** (default) | `=`, `<`, `>`, `BETWEEN`, `ORDER BY`         |
| **Hash**             | Equality only (rare in Postgres)             |
| **GIN**              | JSONB, arrays, full-text                     |
| **GiST**             | Geospatial, range types                      |
| **Partial**          | Index subset of rows (`WHERE active = true`) |
| **Composite**        | Multi-column (`user_id, created_at`)         |
| **Covering**         | INCLUDE columns to avoid table lookup        |

**When to use:** Columns in `WHERE`, `JOIN`, `ORDER BY` on large tables. Don't index low-cardinality columns alone (e.g. boolean `is_active`). Composite left-prefix: `(user_id, created_at)` helps `WHERE user_id = $1` and the pair, not `created_at` alone.

**Pluses and minuses:**

| +                                   | −                                              |
| ----------------------------------- | ---------------------------------------------- |
| Dramatic read speedups              | Slow INSERT/UPDATE/DELETE on indexed columns   |
| Enables efficient sorting/filtering | Wrong index = wasted space, planner may ignore |
| Unique indexes enforce constraints  | Too many indexes confuse the optimizer         |

**Examples:**

```sql
CREATE INDEX idx_orders_user_created ON orders (user_id, created_at DESC);
CREATE INDEX idx_users_active_email ON users (email) WHERE deleted_at IS NULL;
```

**Thoughts:** Indexes don't help `LIKE '%foo%'` (leading wildcard). Use `EXPLAIN ANALYZE` to verify index usage. Drop unused indexes found in `pg_stat_user_indexes`. Extra disk + slower `INSERT`/`UPDATE`/`DELETE` because each index row is maintained. A **hash index** is an access method — not a password hash. See [Hash functions](#hash-functions). SQL Server **clustered / nonclustered** model: [SQL Server indexes (clustered vs nonclustered)](#sql-server-indexes-clustered-vs-nonclustered).

**Sources:**

[Use The Index, Luke](https://use-the-index-luke.com/) · [PostgreSQL — Indexes](https://www.postgresql.org/docs/current/indexes.html)

---

</details>

<details>
  <summary>DELETE vs TRUNCATE</summary>

### DELETE vs TRUNCATE

**Definition:** `DELETE` removes chosen rows (optional `WHERE`).

`TRUNCATE` removes all rows from a table in one shot, with no `WHERE`.

**How it works:**

#### DELETE (selected rows)

- Removes selected rows from the table.
- You can use `WHERE` to delete specific rows.
- The operation is written to the transaction log **row by row**.
- `ON DELETE` triggers fire.
- Slower than `TRUNCATE`.

#### TRUNCATE (all rows)

- Removes **all** rows from the table. In **most DBMS** you cannot undo it with `ROLLBACK` (MySQL and similar). In **PostgreSQL** it **can** `ROLLBACK` with the surrounding transaction.
- You cannot use `WHERE`.
- Faster, because it is not logged row by row.
- Resets identity / autoincrement (`RESTART IDENTITY` in Postgres; default there is `CONTINUE IDENTITY` unless you ask).
- `ON DELETE` triggers do **not** fire (`ON TRUNCATE` statement triggers can).

| Command    | Rows              | `WHERE` | `DELETE` triggers | Speed  | `ROLLBACK` (Postgres) |
| ---------- | ----------------- | ------- | ----------------- | ------ | --------------------- |
| `DELETE`   | Selected (or all) | Yes     | Yes               | Slower | Yes                   |
| `TRUNCATE` | All               | No      | No                | Faster | Yes                   |

**When to use:** `DELETE` for filtered cleanup (`salary < 30000`) or per-row triggers / `RETURNING`.

`TRUNCATE` to empty a table you keep.

To drop the object: [DROP TABLE](#drop-table). All three: [DELETE, TRUNCATE, and DROP](#delete-truncate-and-drop).

**Examples:**

```sql
DELETE FROM employees WHERE salary < 30000;
TRUNCATE TABLE employees;
TRUNCATE TABLE employees RESTART IDENTITY;
```

**Thoughts:** “TRUNCATE cannot ROLLBACK” is a MySQL/old-lore myth — in PostgreSQL it can. Still prefer `DELETE` when you need `WHERE` or per-row triggers.

**Sources:** [PostgreSQL — DELETE](https://www.postgresql.org/docs/current/sql-delete.html) · [PostgreSQL — TRUNCATE](https://www.postgresql.org/docs/current/sql-truncate.html) · [SQL Academy — TRUNCATE, DELETE, and DROP](https://sql-academy.org/ru/blog/sql-truncate-delete-drop)

</details>

<details>
  <summary>ACID</summary>

### ACID

**Definition:** Transaction is a sequence of operations that must finish as a whole or not at all. **ACID** is four guarantees: Atomicity, Consistency, Isolation, Durability.

**How it works:**

#### Atomicity

- The transaction runs fully, or not at all.
- On failure, every change is undone (`ROLLBACK`).

#### Consistency

- The transaction takes the database from one valid state to another.
- All rules and constraints (PK, FK, CHECK) still hold when it ends.
- This is not CAP “every replica is current.”

#### Isolation

- Other transactions do not see the results until this one completes (`COMMIT`).
- Concurrent transactions do not corrupt each other’s work. Levels: [Transactions & isolation](./9_database_core.md#transactions-isolation-levels).

#### Durability

- After a successful `COMMIT`, the results survive a crash.
- Changes are written to durable storage (WAL).

SQL verbs: [COMMIT and ROLLBACK](#commit-and-rollback). Fuller note: [ACID](./9_database_core.md#acid).

**When to use:** Transfers, inventory, bookings — anywhere a partial write is a bug.

**Examples:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
-- On error: ROLLBACK — neither update stays alone
```

**Thoughts:** SQL `COMMIT`/`ROLLBACK` are the verbs; ACID is the contract. ACID **Consistency** is not CAP consistency.

**Sources:** [PostgreSQL — Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html) · [SQL Academy — ACID](https://sql-academy.org/ru/blog/sql-acid)

</details>

<details>
  <summary>Triggers</summary>

### Triggers

**Definition:** A **trigger** runs a function automatically on table events (`INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`) — `BEFORE`, `AFTER`, or `INSTEAD OF` (views).

**How it works:**

- `FOR EACH ROW` vs `FOR EACH STATEMENT`.
- Row triggers see `NEW` / `OLD` (and `NEW.*` / `OLD.*` in PL/pgSQL).
- `BEFORE` can change `NEW` or skip the op (`RETURN NULL`). `AFTER` is for side effects (audit).
- PostgreSQL: `CREATE FUNCTION … RETURNS trigger`, then `CREATE TRIGGER … EXECUTE FUNCTION …`.
- Table DML triggers: `INSERT` / `UPDATE` / `DELETE` / `TRUNCATE`. DDL (`CREATE`/`ALTER`/`DROP`) uses **event triggers**, not `CREATE TRIGGER` on a table.

**When to use:** Audit tables, derived columns you cannot express as generated columns, enforcing rules constraints cannot. Skip for app-only logic that belongs in a service — triggers hide write paths.

**Pluses and minuses:**

| +                                 | −                                |
| --------------------------------- | -------------------------------- |
| Always runs, even outside the app | Hard to test and easy to recurse |
| Central integrity / audit         | Surprises ORMs and bulk loaders  |

**Examples:**

```sql
CREATE FUNCTION log_salary_change() RETURNS trigger
LANGUAGE plpgsql AS $$
BEGIN
  IF NEW.salary IS DISTINCT FROM OLD.salary THEN
    INSERT INTO salary_history (employee_id, old_salary, new_salary)
    VALUES (NEW.id, OLD.salary, NEW.salary);
  END IF;
  RETURN NEW;
END;
$$;

CREATE TRIGGER trg_salary_update
AFTER UPDATE OF salary ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

**Thoughts:** `TRUNCATE` does not fire `DELETE` row triggers — see [DELETE, TRUNCATE, and DROP](#delete-truncate-and-drop).

**Sources:** [PostgreSQL — CREATE TRIGGER](https://www.postgresql.org/docs/current/sql-createtrigger.html) · [PostgreSQL — Trigger functions](https://www.postgresql.org/docs/current/plpgsql-trigger.html)

</details>

<details>
  <summary>VIEW</summary>

### VIEW

**Definition:** View is a virtual table whose rows come from a stored `SELECT`. It does not keep its own copy of the data; it is a named way to look at one or more tables.

**How it works:**

- `CREATE VIEW name AS SELECT …`. Each query against the view runs that `SELECT` again.
- `CREATE MATERIALIZED VIEW` is different: it stores a snapshot you `REFRESH`.

#### Simplify complex queries

- Save a join (or a long `SELECT`) and query it like a table.

#### Security

- Grant `SELECT` on the view and withhold grants on base tables so a role sees only chosen columns/rows.
- Not a full wall by itself: use grants, RLS, and `security_barrier` when the view is a security filter.

#### Updatability

- Simple views (one table in `FROM`, no `DISTINCT` / `GROUP BY` / `UNION` / aggregates) can take `INSERT` / `UPDATE` / `DELETE`.
- `WITH CHECK OPTION` rejects writes that would disappear from the view.
- A join view (like `employee_details` below) is read-only unless you add `INSTEAD OF` triggers.

#### Combined data

- A view can join several tables so readers always see the same shape — it does not replace PK/FK/`CHECK`.

**When to use:** Hide joins behind a stable name.

Give a role a column-limited window, not the whole table.

Skip when the query is used once.

**Pluses and minuses:**

| +                            | −                                                 |
| ---------------------------- | ------------------------------------------------- |
| One name for a complex join  | Extra catalog object to migrate                   |
| Can hide columns from a role | Not a security boundary without grants / RLS      |
| Simple views may be writable | Join / aggregate views need `INSTEAD OF` to write |

**Examples:**

```sql
CREATE VIEW employee_details AS
SELECT e.id, e.name, d.name AS department, e.salary
FROM employees e
JOIN departments d ON e.department_id = d.id;

SELECT * FROM employee_details WHERE salary > 50000;
```

**Thoughts:** Interview trap: “views store data.” Ordinary views do not; materialized views do. Grant on the view, revoke on base tables.

**Sources:** [PostgreSQL — Views](https://www.postgresql.org/docs/current/tutorial-views.html) · [PostgreSQL — CREATE VIEW](https://www.postgresql.org/docs/current/sql-createview.html) · [PostgreSQL — CREATE MATERIALIZED VIEW](https://www.postgresql.org/docs/current/sql-creatematerializedview.html)

</details>

<details>
  <summary>LIKE</summary>

### LIKE

**Definition:** `LIKE` is used in `WHERE` to find rows whose string matches a pattern.

**How it works:**

#### %

- Any sequence of characters, including empty.

#### \_

- Exactly one character.

- The pattern must match the **whole** string; put `%` at both ends to match a substring.
- PostgreSQL `LIKE` is case-sensitive. Use `ILIKE` for a locale case-insensitive match (not in the SQL standard). Typical MySQL collations treat `LIKE` as case-insensitive.
- Literal `%` or `_`: prefix with the escape character (Postgres default `\`), or `ESCAPE`.

**When to use:** Prefix, suffix, or “nth character is …” filters.

Skip `'%foo%'` at scale on a B-tree — leading `%` usually cannot use the index. Use full-text search or `pg_trgm` instead.

**Examples:**

```sql
-- Starts with A
SELECT * FROM employees WHERE name LIKE 'A%';

-- Ends with e
SELECT * FROM employees WHERE name LIKE '%e';

-- Second character is a
SELECT * FROM employees WHERE name LIKE '_a%';
```

**Thoughts:** `'%foo%'` will not use a normal B-tree. See [Indexes](#indexes).

**Sources:** [PostgreSQL — Pattern matching](https://www.postgresql.org/docs/current/functions-matching.html)

</details>

<details>
  <summary>Aggregate functions</summary>

### Aggregate functions

**Definition:** Aggregate functions compute over a set of values and return one value. They are often used with `GROUP BY`.

**How it works:**

#### COUNT()

- Counts rows. `COUNT(*)` counts every input row. `COUNT(col)` counts non-NULL `col` values.

#### SUM()

- Sum of non-NULL values. No rows → `NULL`, not `0` (`COALESCE` if you need zero).

#### AVG()

- Arithmetic mean of non-NULL values.

#### MAX() / MIN()

- Largest / smallest non-NULL value.

Without `GROUP BY`, the whole result is one group. With `GROUP BY`, you get one row per bucket; non-aggregated `SELECT` columns must appear in `GROUP BY`. `WHERE` filters rows **before** the aggregate (no `COUNT` in `WHERE`). `HAVING` filters groups after: [WHERE vs HAVING](#where-vs-having). One row per row with a running total: [Window functions](#window-functions).

**When to use:** Headcount, totals, averages per department, extrema, a month’s sum.

**Examples:**

```sql
-- Count employees
SELECT COUNT(*) FROM employees;

-- Average salary per department
SELECT department_id, AVG(salary) AS average_salary
FROM employees
GROUP BY department_id;

-- Highest salary in the company
SELECT MAX(salary) FROM employees;

-- Total sales in January
SELECT SUM(amount) FROM sales
WHERE sale_date BETWEEN '2023-01-01' AND '2023-01-31';
```

**Thoughts:** `WHERE temp = MAX(temp)` is invalid — the aggregate belongs in a subquery or in `HAVING`.

**Sources:** [PostgreSQL — Aggregate functions](https://www.postgresql.org/docs/current/tutorial-agg.html) · [PostgreSQL — Aggregate function list](https://www.postgresql.org/docs/current/functions-aggregate.html)

</details>

<details>
  <summary>UNION vs UNION ALL</summary>

### UNION vs UNION ALL

**Definition:** `UNION` combines the results of two or more `SELECT` queries and removes duplicate rows.

`UNION ALL` combines those results and keeps duplicates.

**How it works:**

#### UNION

- Appends `SELECT` results (order of rows is not guaranteed).
- Removes duplicates from the combined result (same idea as `DISTINCT`).
- Extra sort/hash work — slower than `UNION ALL`.

#### UNION ALL

- Appends `SELECT` results the same way.
- Keeps duplicate rows.
- Faster, because it skips the distinct step.

Both arms must return the same number of columns with compatible types. Output column names come from the first `SELECT`. `INTERSECT` / `EXCEPT` are the other set operators (`ALL` keeps duplicates there too).

**When to use:** `UNION` when duplicates must disappear.

`UNION ALL` when sources are disjoint (hot + archive) or duplicates are valid.

**Examples:**

```sql
SELECT column_list FROM table1
UNION
SELECT column_list FROM table2;

SELECT column_list FROM table1
UNION ALL
SELECT column_list FROM table2;

SELECT email FROM customers
UNION ALL
SELECT email FROM leads;
```

**Thoughts:** Recursive CTEs pick `UNION` vs `UNION ALL` on purpose — `UNION` can stop duplicate graph nodes. See [CTE](#cte).

**Sources:** [PostgreSQL — Combining queries](https://www.postgresql.org/docs/current/queries-union.html)

</details>

<details>
  <summary>Procedures vs functions</summary>

### Procedures vs functions

**Definition:** A **function** returns a value and can appear in SQL expressions. A **procedure** is invoked with `CALL` and may perform transaction control.

**How it works:**

|                            | Function                                | Procedure                                  |
| -------------------------- | --------------------------------------- | ------------------------------------------ |
| Create                     | `CREATE FUNCTION`                       | `CREATE PROCEDURE` (PG 11+)                |
| Invoke                     | `SELECT fn(…)`, inside expressions      | `CALL proc(…)`                             |
| Return                     | Required (`RETURNS`)                    | No result set required; `OUT` args allowed |
| `COMMIT`/`ROLLBACK` inside | No (except some procedures/tx patterns) | Allowed in `CALL`                          |

**When to use:** Functions for computed values, generated defaults, trigger bodies. Procedures for admin batches (`CALL clean_emp()`). App CRUD usually stays in the client with parameterized SQL.

**Examples:**

```sql
CREATE FUNCTION employee_salary(emp_id INT) RETURNS NUMERIC
LANGUAGE sql AS $$
  SELECT salary FROM employees WHERE id = emp_id;
$$;

SELECT name, employee_salary(id) FROM employees;

CREATE PROCEDURE clean_emp()
LANGUAGE sql AS $$
  DELETE FROM employees WHERE salary < 0;
$$;

CALL clean_emp();
```

**Thoughts:** MySQL “stored procedure vs function” interviews map here, but PostgreSQL functions are first-class SQL. Do not move all business logic into the database by default.

**Sources:** [PostgreSQL — CREATE FUNCTION](https://www.postgresql.org/docs/current/sql-createfunction.html) · [PostgreSQL — CREATE PROCEDURE](https://www.postgresql.org/docs/current/sql-createprocedure.html) · [PostgreSQL — SQL procedures](https://www.postgresql.org/docs/current/xfunc-sql.html)

</details>

<details>
  <summary>Query optimization & EXPLAIN</summary>

### Query optimization & EXPLAIN

**Definition:** Analyze how the database executes a query — scan type, index usage, row estimates, actual timing. Same goal in PostgreSQL (`EXPLAIN`) and SQL Server (Actual Execution Plan).

**How it works:**

Typical levers (measure with a plan first — see [Indexes](#indexes)):

- Index columns used in `WHERE`, `JOIN`, `ORDER BY`; drop unused indexes.
- Select only needed columns — avoid `SELECT *`.
- Prefer equality join keys; do not wrap an indexed column in a function unless you have a matching expression index.
- Prefer `JOIN` / `EXISTS` over correlated subqueries that run per row.
- `LIMIT` / keyset pagination instead of deep `OFFSET`.
- Keep statistics fresh; stale stats → bad plans.
- Materialized views or app cache for hot aggregates — see production caching notes.

#### PostgreSQL

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM orders WHERE user_id = 42 AND status = 'pending';
```

| Operator (bad → good) | Meaning                                              |
| --------------------- | ---------------------------------------------------- |
| `Seq Scan`            | Full table read — OK on tiny tables only             |
| `Index Scan`          | Index lookup + heap fetch                            |
| `Index Only Scan`     | Covered by index — best when columns match           |
| `Nested Loop`         | Inner probe per outer row — OK when inner is indexed |

Refresh stats: `ANALYZE orders;`. Find unused indexes: `pg_stat_user_indexes`.

#### SQL Server

Enable **Include Actual Execution Plan** in SSMS or Azure Data Studio (or `SET STATISTICS IO ON` for I/O). Green = SELECT; yellow = warnings (missing index, implicit conversion).

| Operator (bad → good)                  | Meaning                                                                   |
| -------------------------------------- | ------------------------------------------------------------------------- |
| **Table Scan**                         | Full table read                                                           |
| **Clustered Index Scan**               | Full clustered index read                                                 |
| **Index Seek**                         | Efficient lookup on nonclustered index                                    |
| **Key Lookup** (RID / bookmark lookup) | Extra hop to fetch non-indexed columns — add `INCLUDE` or narrow `SELECT` |
| **Hash Match** / **Sort**              | Build/probe hash or sort — watch spills to tempdb                         |

Refresh stats: `UPDATE STATISTICS orders;`. Missing-index hints: `sys.dm_db_missing_index_details` (suggestions only — validate with a plan).

**What to read in any plan:** estimated vs actual row counts (large gap = bad stats or stale histogram), scan vs seek on hot predicates, warnings on sorts/hashes, key lookup on wide `SELECT *`.

**When to use:** Slow endpoints, before adding indexes, after large data growth or schema change.

**Pluses and minuses:**

| +                                   | −                                                                |
| ----------------------------------- | ---------------------------------------------------------------- |
| Finds missing indexes and bad plans | Output requires practice to read                                 |
| Cheaper than guessing in production | `ANALYZE` / `UPDATE STATISTICS` adds load — use on staging first |

**Examples:**

```sql
-- PostgreSQL
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT id, total FROM orders WHERE user_id = 42 AND status = 'pending';

-- SQL Server — turn on Actual Execution Plan in the client, then:
SELECT id, total FROM orders WHERE user_id = 42 AND status = 'pending';
```

**Thoughts:**

- **N+1 at SQL level** — app runs 1 query + N queries in a loop. Fix with JOIN or `WHERE id IN (...)`.
- Avoid `SELECT *`; fetch columns you need (reduces key lookups in SQL Server, enables Index Only Scan in Postgres).
- Pagination: `OFFSET` is slow at depth — use keyset (`WHERE id > $cursor ORDER BY id LIMIT 20` / `OFFSET-FETCH` with a seek key).
- Prefer equality join keys; wrapping an indexed column in a function (`WHERE lower(email) = …`) needs a matching expression index.

**Sources:** [PostgreSQL — EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html) · [SQL Server — Display an actual execution plan](https://learn.microsoft.com/en-us/sql/relational-databases/performance/display-an-actual-execution-plan) · [SQL Server — Clustered and nonclustered indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described) · [SQL Academy — Query optimization](https://sql-academy.org/ru/blog/sql-query-optimization)

---

</details>

<details>
  <summary>Schema constraints (FK, CHECK, UNIQUE)</summary>

### Schema constraints (FK, CHECK, UNIQUE)

**Definition:** Constraints enforce data integrity at the database layer — not only in application code.

**How it works:**

| Constraint             | Purpose                                                                                                                                                   |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PRIMARY KEY**        | Unique row identifier                                                                                                                                     |
| **FOREIGN KEY (FK)**   | Referential integrity: child must match parent; insert of a missing parent id fails; parent delete fails unless `ON DELETE` — [Foreign key](#foreign-key) |
| **UNIQUE**             | No duplicate values in column(s)                                                                                                                          |
| **NOT NULL**           | Column required                                                                                                                                           |
| **CHECK**              | Boolean expression per row (`price > 0`, `status IN ('open','closed')`)                                                                                   |
| **DEFAULT**            | Value when insert omits column                                                                                                                            |
| **EXCLUDE** (Postgres) | No two rows satisfy a given operator (typical: overlapping ranges)                                                                                        |

PK is `NOT NULL` + `UNIQUE` plus “this is the row identity”. A table may have several `UNIQUE` constraints but only one `PRIMARY KEY`.

| On delete/update         | Effect                               |
| ------------------------ | ------------------------------------ |
| `RESTRICT` / `NO ACTION` | Block if children exist              |
| `CASCADE`                | Delete/update children automatically |
| `SET NULL`               | Nullify FK in children               |
| `SET DEFAULT`            | Set FK columns to their defaults     |

**When to use:** Always FK `orders.user_id → users.id`. Use `CHECK` for enums and positive amounts. `ON DELETE CASCADE` for join tables; `RESTRICT` for orders referencing users. `DEFAULT` for status/`created_at`.

**Pluses and minuses:**

| +                                 | −                                             |
| --------------------------------- | --------------------------------------------- |
| Integrity even if app has bugs    | Migration order matters (parent before child) |
| Self-documenting relationships    | `CASCADE` deletes can surprise                |
| DB rejects bad data at write time | ORM may fight FK during bulk loads            |

**Examples:**

```sql
CREATE TABLE orders (
  id         SERIAL PRIMARY KEY,
  user_id    INT NOT NULL REFERENCES users(id) ON DELETE RESTRICT,
  total      NUMERIC(10,2) NOT NULL CHECK (total >= 0),
  status     TEXT NOT NULL CHECK (status IN ('pending', 'paid', 'shipped')),
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE UNIQUE INDEX idx_users_email_lower ON users (lower(email));

-- UNIQUE vs PK: extra identity, NULLs allowed unless NOT NULL
CREATE TABLE users (
  id INT PRIMARY KEY,
  email TEXT NOT NULL UNIQUE
);
```

**Thoughts:** Prisma maps `@relation` to FK. Add indexes on FK columns — Postgres does not auto-index FKs. Topics: [Primary key](#primary-key), [Foreign key](#foreign-key).

**Sources:**

[PostgreSQL — Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)

---

</details>

<details>
  <summary>SQL injection</summary>

### SQL injection

**Definition:** SQL injection is an attack that inserts malicious SQL through user input so the database runs unauthorized queries.

**How it works:**

String-concatenated SQL lets the attacker change query structure (`' OR '1'='1`, comment the rest, sometimes a second statement). Parameterized queries bind values separately so that input stays data, not SQL — they do not “escape” characters into the query text.

#### Impact

- Steal data.
- Delete or change data.
- Gain admin-level access if the DB role allows it.

#### 1. Parameterized queries (prepared statements)

- Use placeholders instead of concatenating strings. See [Prepared statements](#prepared-statements-placeholders).

#### 2. ORM

- Query APIs usually bind parameters. Unsafe if you concatenate into `$queryRaw` / `sequelize.query`.

#### 3. Validate input

- Check type and format on the **server**. Client checks are UX, not a SQLi control. Allow-list identifiers (table/column/`ORDER BY`) — they cannot be bound as parameters.

#### 4. Least privilege

- Grant the app the minimum rights. No `DROP` / `GRANT` / catalog admin. See [GRANT and REVOKE](#grant-and-revoke).

#### 5. Stored procedures

- Safe only when arguments are bound, not concatenated into dynamic SQL. Encapsulating logic is not enough. See [Procedures vs functions](#procedures-vs-functions).

Do not return raw DB errors to clients.

**When to use:** Every user-controlled value in a query (login, search, IDs).

**Pluses and minuses:**

| + (of prevention)                        | −                                   |
| ---------------------------------------- | ----------------------------------- |
| Parameterization removes the whole class | One string concat still fails       |
| ORM APIs bind by default                 | Raw SQL in ORMs is still injectable |

**Examples:**

```java
String sql = "SELECT * FROM users WHERE username = ? AND password = ?";
PreparedStatement stmt = connection.prepareStatement(sql);
stmt.setString(1, username);
stmt.setString(2, password);
ResultSet rs = stmt.executeQuery();
```

```js
// VULNERABLE
const q = `SELECT * FROM users WHERE email = '${email}'`;

// SAFE — node-postgres
await pool.query("SELECT * FROM users WHERE email = $1", [email]);

// SAFE — Prisma
await prisma.user.findUnique({ where: { email } });
```

Attack payload: `email = "' OR '1'='1' --"` makes a concatenated `WHERE` always true.

**Thoughts:** SQL injection is not XSS (SQL vs HTML/JS). Frontend escaping does not bind parameters. Storing a raw password in `WHERE` is a separate bug — hash it. See [Special — SQL injection](./7_special_security.md#sql-injection).

**Sources:** [OWASP — SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection) · [OWASP — SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

</details>

<details>
  <summary>Relational database</summary>

### Relational database

**Definition:** Relational database is a database based on the relational model. Data is stored in tables; relationships between data are defined with keys.

**How it works:**

#### Tables (relations)

- Data is organized in named tables of rows and columns. In PostgreSQL a _relation_ is essentially a table.

#### Rows (records / tuples)

- Each row is one record. SQL does **not** guarantee row order unless you `ORDER BY`.

#### Columns (attributes)

- Each column has a name and a type (`integer`, `text`, `numeric`, …). Column order is fixed.

#### Keys

- Identify records and link tables: [Primary key](#primary-key), [Foreign key](#foreign-key), extra [UNIQUE](#schema-constraints-fk-check-unique).

SQL is the language for this model. 1:1, 1:N, M:N via a [junction table](#many-to-many). Broader store types: [Database taxonomy](./9_database_core.md#database-taxonomy).

**When to use:** Structured OLTP, reporting, integrity constraints.

Document/KV stores when the access pattern is not relational — [DB selection matrix](./9_database_nosql_core.md#db-selection-matrix).

**Pluses and minuses:**

| +                                          | −                                        |
| ------------------------------------------ | ---------------------------------------- |
| Add tables and columns as the schema grows | Schema changes need migrations           |
| Constraints keep integrity at write time   | Vertical scale ceiling on one primary    |
| SQL is the standard language for the data  | Wrong join type loses or duplicates rows |

**Examples:**

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  department_id INT REFERENCES departments (id)
);
```

**Thoughts:** “Relational” is Codd’s model, not “Postgres only”. MySQL and SQL Server are relational too.

**Sources:** [PostgreSQL — Concepts](https://www.postgresql.org/docs/current/tutorial-concepts.html) · [PostgreSQL — Table basics](https://www.postgresql.org/docs/current/ddl-basics.html)

</details>

<details>
  <summary>INNER JOIN vs OUTER JOIN</summary>

### INNER JOIN vs OUTER JOIN

**Definition:** **INNER JOIN** returns only rows with matches in both tables. **OUTER JOIN** (`LEFT` / `RIGHT` / `FULL`) also keeps unmatched rows and fills the other side with NULL.

**How it works:**

INNER keeps only matches. OUTER keeps unmatched rows. `LEFT`/`RIGHT`/`FULL` mean OUTER; the `OUTER` keyword is optional.

| Join type      | Returns                                           |
| -------------- | ------------------------------------------------- |
| **INNER**      | Rows with matches in both tables                  |
| **LEFT**       | All left rows + matching right (NULL if no match) |
| **RIGHT**      | Mirror of LEFT                                    |
| **FULL OUTER** | All rows from both, NULLs where no match          |

All join kinds including CROSS: [Joins](#joins).

**When to use:** INNER for required relationships; LEFT when optional related data (e.g. user without orders). RIGHT is the same as swapping tables and LEFT — prefer LEFT.

**Examples:**

```sql
SELECT u.id, u.email, o.total
FROM users u
INNER JOIN orders o ON o.user_id = u.id;

SELECT u.id, u.email, o.total
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;
```

**Thoughts:** Wrong join type = silent data loss (INNER vs LEFT).

**Sources:** [PostgreSQL — Joins](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN)

</details>

<details>
  <summary>NULL</summary>

### NULL

**Definition:** `NULL` is a special SQL marker for missing or unknown data. It is not an empty string and not zero.

**How it works:**

- Most operations with `NULL` return `NULL` (unknown).
- `NULL = NULL` is **not true**. In standard SQL it yields **unknown** (`NULL`), not `FALSE`. `WHERE` treats unknown as “no row”, so it _looks_ like false. Never write `col = NULL`.

#### IS NULL / IS NOT NULL

- Test for missing vs present values. PostgreSQL also allows nonstandard postfix `ISNULL` / `NOTNULL` (same as `IS NULL`).

#### COALESCE

- Returns the first argument that is not `NULL`. SQL standard. MySQL `IFNULL(a, b)` is `COALESCE(a, b)`.

#### ISNULL / IFNULL (other engines)

- MySQL `ISNULL(x)` → `1`/`0`. SQL Server `ISNULL(x, alt)` is a two-arg substitute.
- PostgreSQL has no `IFNULL`. Use `COALESCE`. Postfix `x ISNULL` is **not** MySQL `ISNULL(x)`.

`IS DISTINCT FROM` / `IS NOT DISTINCT FROM` compare with NULLs as real values (`NULL IS NOT DISTINCT FROM NULL` is true). Aggregates skip NULL except `COUNT(*)`.

**When to use:** Optional FKs, unknown dates.

Prefer `NOT NULL` + `DEFAULT` when a value is always required.

**Examples:**

```sql
-- Unknown birth date
SELECT * FROM employees WHERE birth_date IS NULL;

-- Known birth date
SELECT * FROM employees WHERE birth_date IS NOT NULL;

SELECT COALESCE(nickname, name, '(none)') AS display FROM employees;
```

**Thoughts:** `WHERE col != 'x'` drops NULL rows. Keep them with `col IS DISTINCT FROM 'x'`.

**Sources:** [PostgreSQL — Comparison](https://www.postgresql.org/docs/current/functions-comparison.html) · [PostgreSQL — COALESCE](https://www.postgresql.org/docs/current/functions-conditional.html#FUNCTIONS-COALESCE-NVL-IFNULL) · [SQL Academy — COALESCE](https://sql-academy.org/ru/blog/sql-coalesce)

</details>

<details>
  <summary>CASE</summary>

### CASE

**Definition:** `CASE` implements if/else logic in SQL. It returns a value from the first `WHEN` condition that is true, or from `ELSE`.

**How it works:**

#### Searched CASE

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END
```

- First true `WHEN` wins (order matters: `>= 80000` before `>= 50000`).
- Omit `ELSE` and no match → `NULL`.

#### Simple CASE

- `CASE expr WHEN value THEN result … END` — like a C `switch` on equality.

Result types of `THEN`/`ELSE` must share a common type. Valid in `SELECT`, `WHERE`, `ORDER BY`, `UPDATE`. Unused branches are usually not evaluated.

**When to use:** Salary bands, mapping codes to labels, conditional aggregates.

Skip when a lookup table is the real model.

**Examples:**

```sql
SELECT name,
    salary,
    CASE
        WHEN salary >= 80000 THEN 'High'
        WHEN salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

**Thoughts:** `FILTER (WHERE …)` on aggregates is often clearer than `SUM(CASE WHEN … THEN 1 END)`.

**Sources:** [PostgreSQL — CASE](https://www.postgresql.org/docs/current/functions-conditional.html#FUNCTIONS-CASE)

</details>

<details>
  <summary>COMMIT and ROLLBACK</summary>

### COMMIT and ROLLBACK

**Definition:** `COMMIT` ends the current transaction and makes its changes permanent and visible to others. You cannot undo them afterward.

`ROLLBACK` aborts the current transaction. All of its changes are discarded; the database is back to the state before the block started.

**How it works:**

#### Start the transaction

- PostgreSQL: `BEGIN` or `START TRANSACTION` (`BEGIN TRANSACTION` is the same — `TRANSACTION` is optional).
- Without `BEGIN`, each statement autocommits on success.

#### COMMIT

- Fixes the transaction.
- Changes become durable and visible to other sessions.
- After `COMMIT`, `ROLLBACK` cannot undo them.

#### ROLLBACK

- Cancels the transaction.
- All statements in the block are undone.

`SAVEPOINT` / `ROLLBACK TO` undo only part of a still-open block. Isolation: [Transactions & isolation](./9_database_core.md#transactions-isolation-levels). Contract: [ACID](#acid).

**When to use:** Multi-statement writes that must succeed together (money transfer).

Keep the block short.

**Examples:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;

BEGIN;
DELETE FROM orders WHERE order_date < DATE '2022-01-01';
-- changed our mind
ROLLBACK;
```

**Thoughts:** ACID is the guarantee; these are the verbs. Isolation levels: [Core](./9_database_core.md#transactions-isolation-levels).

**Sources:** [PostgreSQL — Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html) · [PostgreSQL — BEGIN](https://www.postgresql.org/docs/current/sql-begin.html) · [PostgreSQL — COMMIT](https://www.postgresql.org/docs/current/sql-commit.html) · [PostgreSQL — ROLLBACK](https://www.postgresql.org/docs/current/sql-rollback.html) · [SQL Academy — ACID](https://sql-academy.org/ru/blog/sql-acid)

</details>

<details>
  <summary>CHAR vs VARCHAR</summary>

### CHAR vs VARCHAR

**Definition:** `CHAR(n)` (`character(n)`) stores a string of fixed length `n`. Shorter input is padded with spaces to `n`.

`VARCHAR(n)` (`character varying(n)`) stores a string of variable length up to `n`. It uses the characters plus a small length overhead.

**How it works:**

#### CHAR(n)

- Always occupies the declared width (padding included).
- Trailing spaces are insignificant on `CHAR` comparison; they are stripped when you cast to `text`/`varchar`.
- Dump use: country codes, postal codes, other truly fixed codes.

#### VARCHAR(n)

- Stores the actual string; longer than `n` → error (unless the extra characters are all spaces).
- Dump use: names and other variable text.
- PostgreSQL also has unlimited `text` (preferred for most columns). `varchar` without `(n)` is unlimited too.

#### Memory and performance

- Dump lore: `CHAR` is fixed and “faster”; `VARCHAR` saves space but “may be a bit slower.”
- PostgreSQL: no speed win for `CHAR`. Padding costs extra storage and CPU; `character(n)` is usually the **slowest**. Prefer `text` or `varchar`.

**When to use:** `CHAR` only for truly fixed-width codes.

Otherwise `varchar(n)` or `text`.

**Examples:**

```sql
CREATE TABLE products (
    code CHAR(10),      -- fixed product code
    name VARCHAR(100)   -- variable name
);

INSERT INTO products (code, name)
VALUES ('A123', 'Lenovo laptop');
-- code is stored as 'A123' plus spaces to length 10
```

**Thoughts:** Padding surprises `length()`, `LIKE`, and concatenation. Default to `text` in Postgres.

**Sources:** [PostgreSQL — Character types](https://www.postgresql.org/docs/current/datatype-character.html)

</details>

<details>
  <summary>Temporary tables</summary>

### Temporary tables

**Definition:** Temporary table is a table that exists only for the current session (or connection) and is dropped automatically when the session ends.

**How it works:**

- PostgreSQL: `CREATE TEMP TABLE` or `CREATE TEMPORARY TABLE`. Each session must create its own; names can match across sessions without clashing.
- Optional `ON COMMIT DROP` / `DELETE ROWS` / `PRESERVE ROWS` — drop or empty at end of transaction, not only at session end.
- Invisible to other sessions. A temp table with the same name as a permanent one wins in `search_path` unless you schema-qualify.
- Indexes on a temp table are temporary too.
- SQL Server `#TempTable` / `##` prefixes are **not** PostgreSQL. MySQL uses `CREATE TEMPORARY TABLE` without `#`.

**When to use:** Hold intermediate results in a complex query.

Batch large data in steps.

Avoid clashes when several users run the same script (session-local names).

Skip when the set must survive reconnect — real table or `UNLOGGED`. Prefer a [CTE](#cte) when the set is small and one-query.

**Examples:**

```sql
CREATE TEMP TABLE temp_table (
    id INT,
    name VARCHAR(100)
);

INSERT INTO temp_table (id, name)
VALUES (1, 'Ivan'), (2, 'Petr');

SELECT * FROM temp_table;
-- Dropped automatically when the session ends
```

**Thoughts:** Connection pools reuse sessions — leftover temp tables can surprise you. `ANALYZE` the temp table if you will join it in a large query (autovacuum cannot see it).

**Sources:** [PostgreSQL — CREATE TABLE](https://www.postgresql.org/docs/current/sql-createtable.html)

</details>

<details>
  <summary>Window functions</summary>

### Window functions

**Definition:** **Window functions** compute across related rows without collapsing the result set.

**How it works:**

| Window                       | Role                                   |
| ---------------------------- | -------------------------------------- |
| `ROW_NUMBER()`               | Unique 1…n in the partition            |
| `RANK()`                     | Ties share a rank; next number skipped |
| `DENSE_RANK()`               | Ties share a rank; no gaps             |
| `LAG` / `LEAD`               | Previous / next row value              |
| `FIRST_VALUE` / `LAST_VALUE` | Edges of the window frame              |

`OVER (PARTITION BY … ORDER BY …)` defines the window. Window results cannot go in `WHERE` — wrap in a subquery.

**When to use:** Rankings, running totals, deduplication (`ROW_NUMBER` + filter), department averages beside each employee row, pagination with stable sort.

**Examples:**

```sql
SELECT
  user_id,
  id,
  total,
  ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY total DESC) AS rank
FROM orders;

SELECT
  created_at,
  amount,
  SUM(amount) OVER (ORDER BY created_at) AS running_total
FROM payments;
```

**Thoughts:** Window functions run **after** WHERE/GROUP BY. `ROW_NUMBER` vs `RANK` vs `DENSE_RANK`: ties skip numbers only with `RANK`.

**Sources:** [PostgreSQL — Window functions](https://www.postgresql.org/docs/current/tutorial-window.html) · [SQL Academy — Window functions](https://sql-academy.org/ru/guide/windows-functions) · [SQL Academy — ROW_NUMBER, RANK, DENSE_RANK](https://sql-academy.org/ru/blog/row-number-rank-dense-rank)

</details>

<details>
  <summary>CTE</summary>

### CTE

**Definition:** A **CTE** (Common Table Expression) names a subquery with `WITH` for readability and reuse.

**How it works:**

| Feature        | Subquery        | CTE (`WITH`)                 |
| -------------- | --------------- | ---------------------------- |
| Readability    | Can nest deeply | Named, linear                |
| Reuse in query | Limited         | Can reference multiple times |
| Recursion      | No              | Yes (`WITH RECURSIVE`)       |

**When to use:** Multi-step reports, org trees, readable pipelines.

**Examples:**

```sql
WITH totals AS (
  SELECT user_id, SUM(amount) AS total
  FROM orders GROUP BY user_id
)
SELECT u.email, t.total
FROM users u JOIN totals t ON t.user_id = u.id
WHERE t.total > 1000;
```

**Thoughts:** Recursive CTEs use `UNION` vs `UNION ALL` deliberately — `UNION` can stop duplicate graph nodes.

**Sources:** [PostgreSQL — WITH queries](https://www.postgresql.org/docs/current/queries-with.html) · [SQL Academy — Operator WITH](https://sql-academy.org/ru/guide/operator-with) · [SQL Academy — CTE](https://sql-academy.org/ru/blog/sql-cte)

</details>

<details>
  <summary>DROP TABLE</summary>

### DROP TABLE

**Definition:** `DROP TABLE` removes the table object together with its data, indexes, and constraints.

**How it works:**

- No `WHERE` — the relation is gone from the catalog.
- Transactional in PostgreSQL (can `ROLLBACK` in the same block).
- Irreversible once committed unless you restore a backup.
- Drops indexes, triggers, and constraints on **this** table. Views or FKs **on other tables** that reference it need `CASCADE` (`CASCADE` drops the dependent view, or only the FK constraint — not the other table). Default is `RESTRICT`.

**When to use:** When the object should not exist. To empty rows and keep the table: [DELETE vs TRUNCATE](#delete-vs-truncate).

**Examples:**

```sql
DROP TABLE employees;
```

**Thoughts:** Compare with DELETE/TRUNCATE in [DELETE, TRUNCATE, and DROP](#delete-truncate-and-drop).

**Sources:** [PostgreSQL — DROP TABLE](https://www.postgresql.org/docs/current/sql-droptable.html) · [SQL Academy — TRUNCATE, DELETE, and DROP](https://sql-academy.org/ru/blog/sql-truncate-delete-drop)

</details>

<details>
  <summary>ALTER TABLE</summary>

### ALTER TABLE

**Definition:** `ALTER TABLE` changes an existing table: columns, constraints, types, defaults — without dropping the table.

**How it works:**

- `ADD COLUMN` appends a column. `NOT NULL` on a populated table needs a `DEFAULT` (or a three-step backfill).
- `DROP COLUMN`, `RENAME COLUMN`, `ALTER COLUMN … TYPE` rewrite or catalog-update depending on the change.
- Adding an FK or `CHECK` scans existing rows. `ALTER TABLE … ADD CONSTRAINT … FOREIGN KEY` — same rules as [Foreign key](#foreign-key).

**When to use:** Migrations. Prefer additive changes in production; large rewrites need a lock plan.

**Examples:**

```sql
ALTER TABLE employees ADD COLUMN email TEXT;
ALTER TABLE employees
  ADD COLUMN date_of_birth DATE NOT NULL DEFAULT DATE '1900-01-01';
```

**Thoughts:** Expand-contract: add nullable column → backfill → set NOT NULL. Avoid long `ACCESS EXCLUSIVE` locks on big tables.

**Sources:** [PostgreSQL — ALTER TABLE](https://www.postgresql.org/docs/current/sql-altertable.html) · [PostgreSQL — Adding a column](https://www.postgresql.org/docs/current/ddl-alter.html#DDL-ALTER-ADDING-A-COLUMN)

</details>

<details>
  <summary>Correlated subquery</summary>

### Correlated subquery

**Definition:** A **correlated subquery** references columns from the outer query and runs in that row’s context (typical `EXISTS` / per-group comparison). Uncorrelated subqueries run once.

**How it works:**

- Correlated form sees outer columns; the planner often rewrites to a join, but a naive nested loop is N inner executions.

**When to use:** When the inner query must see the current outer row — rewrite to JOIN/`GROUP BY` if `EXPLAIN` shows a nested loop disaster.

**Examples:**

```sql
SELECT e.name, e.salary
FROM employees e
WHERE e.salary > (
  SELECT AVG(salary)
  FROM employees
  WHERE department_id = e.department_id
);
```

**Thoughts:** A correlated subquery is not always slow — `EXISTS` can short-circuit. Measure with [EXPLAIN](#query-optimization-explain).

**Sources:** [PostgreSQL — Subquery expressions](https://www.postgresql.org/docs/current/functions-subquery.html) · [SQL Academy — Correlated subqueries](https://sql-academy.org/ru/guide/correlated-subqueries)

</details>

<details>
  <summary>DELETE, TRUNCATE, and DROP</summary>

### DELETE, TRUNCATE, and DROP

**Definition:** Three ways to remove data: **DELETE** removes matching rows, **TRUNCATE** empties a table fast, **DROP TABLE** removes the table object.

**How it works:**

| Command      | What it removes                       | `WHERE` | Triggers                           | Transaction                                      |
| ------------ | ------------------------------------- | ------- | ---------------------------------- | ------------------------------------------------ |
| `DELETE`     | Selected rows                         | Yes     | Row `DELETE` triggers fire         | Fully transactional                              |
| `TRUNCATE`   | All rows; optional `RESTART IDENTITY` | No      | Statement `TRUNCATE` triggers only | **Transactional in PostgreSQL** (can `ROLLBACK`) |
| `DROP TABLE` | Table + indexes + constraints         | —       | Object gone                        | Transactional in PostgreSQL                      |

`TRUNCATE` reclaims space immediately and may `CASCADE` to tables that reference it.

**When to use:** `DELETE` for filtered cleanup. `TRUNCATE` to empty a table you keep. `DROP` only when the object should not exist.

**Examples:**

```sql
DELETE FROM employees WHERE salary < 30000;
TRUNCATE TABLE employees RESTART IDENTITY;
DROP TABLE employees;
```

**Thoughts:** “TRUNCATE cannot ROLLBACK” is a MySQL/old-lore myth — in PostgreSQL it can. Still prefer `DELETE` when you need `WHERE` or per-row triggers.

**Sources:** [PostgreSQL — DELETE](https://www.postgresql.org/docs/current/sql-delete.html) · [PostgreSQL — TRUNCATE](https://www.postgresql.org/docs/current/sql-truncate.html) · [PostgreSQL — DROP TABLE](https://www.postgresql.org/docs/current/sql-droptable.html) · [SQL Academy — TRUNCATE, DELETE, and DROP](https://sql-academy.org/ru/blog/sql-truncate-delete-drop)

</details>

<details>
  <summary>Self-join</summary>

### Self-join

**Definition:** A **self-join** is a join of a table to itself, using aliases so each role is a separate input.

**How it works:**

- Typical pattern: self-referential [foreign key](#foreign-key) (`manager_id → employee_id` on the same table).
- `LEFT JOIN` keeps roots with no parent (`manager_id IS NULL`).
- Hierarchical walks of unknown depth use `WITH RECURSIVE`, not a single self-join.

**When to use:** Org charts, comment threads one level deep, “rows that match other rows”. Skip when a recursive CTE is the real query.

**Examples:**

```sql
SELECT e.name AS employee, m.name AS manager
FROM employee e
LEFT JOIN employee m ON e.manager_id = m.employee_id;
```

**Thoughts:** Always alias both sides. Forcing an inner join drops the CEO/root row.

**Sources:** [PostgreSQL — Joins](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN)

</details>

<details>
  <summary>Backup and restore</summary>

### Backup and restore

**Definition:** Backup and restore keep a copy of the database so you can recover after failure or loss. The tools depend on the DBMS.

**How it works:**

#### Backup (logical dump)

**PostgreSQL** — `pg_dump`:

```bash
pg_dump -U username mydatabase > backup.sql
```

**MySQL** — `mysqldump`:

```bash
mysqldump -u username -p mydatabase > backup.sql
```

`-U` / `-u` is the DB user; `mydatabase` is the database; `backup.sql` is the dump file.

`pg_dump -Fc` is a custom format for parallel `pg_restore`. Physical base backup + WAL enables PITR (any time in the window): [Backup & PITR](./9_database_production.md#backup-pitr-ops-note).

#### Restore

**PostgreSQL:**

```bash
psql -U username mydatabase < backup.sql
```

**MySQL:**

```bash
mysql -u username -p mydatabase < backup.sql
```

#### Practice

- **Rights** — the role must be allowed to dump and restore.
- **Schedule** — automate regular backups.
- **Storage** — keep copies off the primary server.
- **Test** — restore on a staging server on a schedule.

A replica is not a backup — it can copy a bad write. Untested restore is not a backup.

**When to use:** Daily SQL dumps for small DBs; WAL archiving when RPO is minutes. After `DROP TABLE` / bad deploy: restore or PITR.

**Thoughts:** Treat dump files as secrets (passwords, PII). Define RPO/RTO before picking dump vs PITR.

**Sources:** [PostgreSQL — Backup and restore](https://www.postgresql.org/docs/current/backup.html) · [PostgreSQL — pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html)

</details>

<details>
  <summary>Many-to-many</summary>

### Many-to-many

**Definition:** Many-to-many means one row in table A can link to many rows in table B, and one row in B can link to many in A.

In SQL you model that with a **junction table** (link / bridge table) that holds foreign keys to both parents’ primary keys.

**How it works:**

1. Create the two entity tables.
2. Create a junction table with FKs to both PKs.
3. Put a **composite primary key** on the pair so each link is unique, and FKs for referential integrity.

Optional payload on the junction (`enrolled_at`, `role`). Both FKs often use `ON DELETE CASCADE` if the link should die with either side. See [Foreign key](#foreign-key).

**When to use:** Students↔courses, posts↔tags, users↔roles.

Skip embedding arrays when you need constraints, extra columns, or queries from both sides.

**Examples:**

```sql
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE course (
    course_id INT PRIMARY KEY,
    title VARCHAR(100)
);

CREATE TABLE student_course (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student (student_id),
    FOREIGN KEY (course_id) REFERENCES course (course_id)
);
```

- Each `student_course` row is one enrollment pair.
- `PRIMARY KEY (student_id, course_id)` blocks duplicate enrollments.
- FKs reject orphans (student/course that do not exist).

**Thoughts:** Same pattern as Postgres `order_items (product_no, order_id)`. Prisma uses an implicit M:N table unless you model the join explicitly.

**Sources:** [PostgreSQL — Foreign keys](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK)

</details>

<details>
  <summary>GRANT and REVOKE</summary>

### GRANT and REVOKE

**Definition:** `GRANT` and `REVOKE` manage who can do what on database objects. They give or take privileges from users or roles — DCL for access control.

**How it works:**

#### GRANT

- Gives a role privileges on an object (table, view, schema, database, routine, …).

```sql
GRANT privileges ON object TO user [WITH GRANT OPTION];
```

- **privileges** — e.g. `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `ALL PRIVILEGES`.
- **object** — table, view, schema, …
- **user** — role name (Postgres unifies users and groups as roles).
- **WITH GRANT OPTION** — grantee may pass the privilege on.

#### REVOKE

- Removes privileges previously granted.

```sql
REVOKE privileges ON object FROM user;
```

- Dependent grants (passed via `WITH GRANT OPTION`) need `CASCADE` or the revoke fails (`RESTRICT` is the default).

Schema `USAGE` is required to use objects inside a schema — a table `GRANT` alone is not enough. Table grants do not cover sequences used by `SERIAL` / identity — grant those separately.

**When to use:** App role: DML on needed tables only.

Migrator role: DDL. Humans: groups, not superuser.

**Examples:**

```sql
GRANT SELECT ON employees TO user1;
REVOKE SELECT ON employees FROM user1;

GRANT SELECT, INSERT ON employees TO app_user;
REVOKE DELETE ON employees FROM app_user;
```

**Thoughts:** Least privilege is part of injection defense — a stolen query cannot `DROP TABLE` if the role cannot. See [SQL injection](#sql-injection).

**Sources:** [PostgreSQL — GRANT](https://www.postgresql.org/docs/current/sql-grant.html) · [PostgreSQL — REVOKE](https://www.postgresql.org/docs/current/sql-revoke.html) · [PostgreSQL — Privileges](https://www.postgresql.org/docs/current/ddl-priv.html)

</details>

<details>
  <summary>Hash functions</summary>

### Hash functions

**Definition:** Hash functions turn input of any length into a fixed-length digest. Used for security, integrity checks, and sometimes faster comparison / lookup.

Not the same as a **hash index** (access method). See [Indexes](#indexes).

**How it works:**

#### 1. Password storage

- Store a digest, not plaintext. Prefer a **slow, salted** KDF (`crypt` + `gen_salt('bf')` in `pgcrypto`, or Argon2/bcrypt in the app).
- Dump lore often shows raw `SHA-256` / `SHA-512` / SQL Server `HASHBYTES` — that is **not** enough for passwords (fast, often unsalted).

#### 2. Integrity

- Hash the payload; compare later. Change → digest changes. Postgres: `digest(data, 'sha256')`.

#### 3. Indexing / fast compare

- Digests can speed equality checks on large blobs. A **hash index** is a separate B-tree alternative for `=` lookups — not a crypto hash of a password.

#### 4. Content-derived ids

- Hash of content as a stable id (dedupe, cache keys). Collisions are rare but possible — do not treat SHA as a PK guarantee alone.

Hashing is one-way — you cannot recover the original from the digest. Prefer `sha256`/`sha512` for integrity; avoid `md5`/`sha1` for security. Prefer hashing passwords in the **app** so the DB never sees plaintext.

**When to use:** Checksums, HMAC, salted password columns if you must hash in SQL.

Skip plain SHA of the password.

**Examples:**

```sql
CREATE EXTENSION IF NOT EXISTS pgcrypto;

CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    password_hash TEXT NOT NULL
);

-- Integrity (not login)
SELECT encode(digest('payload', 'sha512'), 'hex');

-- Password: salted Blowfish crypt — not HASHBYTES('SHA2_512', …)
INSERT INTO users (user_id, username, password_hash)
VALUES (1, 'user1', crypt('password123', gen_salt('bf')));

SELECT user_id FROM users
WHERE username = 'user1'
  AND password_hash = crypt('password123', password_hash);
```

**Thoughts:** Interview trap: `HASHBYTES` / unsalted SHA. Threat = offline brute force; fix = salt + slow KDF (or app-side Argon2).

**Sources:** [PostgreSQL — pgcrypto](https://www.postgresql.org/docs/current/pgcrypto.html)

</details>

<details>
  <summary>How SQL triggers work</summary>

### How SQL triggers work

**Definition:** SQL trigger is a database object — a special stored routine that runs automatically when an event happens, such as `INSERT`, `UPDATE`, or `DELETE` on a table or view.

**How it works:**

The trigger fires by itself when the event occurs (for example, a new row is inserted).

#### BEFORE

- Runs before the operation.
- Validate or change data before it is stored (`NEW` can be modified; `RETURN NULL` skips the row).

#### AFTER

- Runs after the operation succeeds.
- Audit logs, update related tables.

#### INSTEAD OF

- Replaces the default action.
- Typical on views that are not auto-updatable.

Row triggers see `OLD` / `NEW` (Postgres: records in PL/pgSQL). `INSERT` has no `OLD`; `DELETE` has no `NEW`. Use `IS DISTINCT FROM` for NULL-safe salary compares.

PostgreSQL: `CREATE FUNCTION … RETURNS trigger`, then `CREATE TRIGGER … EXECUTE FUNCTION …`. MySQL can put `BEGIN … END` in `CREATE TRIGGER` itself. Overview + DML vs DDL: [Triggers](#triggers).

**When to use:** Audit / change history.

Complex integrity rules constraints cannot express.

Sync related tables.

Derived / aggregate columns that must stay in sync.

Skip when the app service should own the write path — triggers hide side effects from ORMs.

**Examples:**

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    salary NUMERIC(10, 2)
);

CREATE TABLE salary_history (
    employee_id INT,
    old_salary NUMERIC(10, 2),
    new_salary NUMERIC(10, 2),
    change_date TIMESTAMPTZ DEFAULT now()
);

CREATE FUNCTION log_salary_change() RETURNS trigger
LANGUAGE plpgsql AS $$
BEGIN
  IF NEW.salary IS DISTINCT FROM OLD.salary THEN
    INSERT INTO salary_history (employee_id, old_salary, new_salary)
    VALUES (NEW.employee_id, OLD.salary, NEW.salary);
  END IF;
  RETURN NEW;
END;
$$;

CREATE TRIGGER trg_after_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
EXECUTE FUNCTION log_salary_change();
```

**Thoughts:** `TRUNCATE` does not fire `DELETE` row triggers — see [DELETE, TRUNCATE, and DROP](#delete-truncate-and-drop).

**Sources:** [PostgreSQL — CREATE TRIGGER](https://www.postgresql.org/docs/current/sql-createtrigger.html) · [PostgreSQL — Trigger functions](https://www.postgresql.org/docs/current/plpgsql-trigger.html)

</details>

<details>
  <summary>Upsert (ON CONFLICT)</summary>

### Upsert (ON CONFLICT)

**Definition:** **Upsert** inserts a row or updates it if a unique constraint conflicts — atomic "create or update".

**How it works:**

```sql
INSERT INTO users (email, name, updated_at)
VALUES ($1, $2, now())
ON CONFLICT (email)          -- unique index / constraint
DO UPDATE SET
  name = EXCLUDED.name,
  updated_at = now()
RETURNING *;
```

- `ON CONFLICT (columns)` targets a unique constraint.
- `DO NOTHING` — skip silently (idempotent insert).
- `DO UPDATE` — merge; `EXCLUDED` refers to the row that would have been inserted.

**When to use:** Sync external IDs, login "create if not exists", settings row per user, deduplicated event ingestion.

**Pluses and minuses:**

| +                                              | −                                        |
| ---------------------------------------------- | ---------------------------------------- |
| One round trip, race-safe vs check-then-insert | Must have matching unique index          |
| Idempotent sync/import jobs                    | Easy to overwrite fields unintentionally |

**Examples:**

```ts
await prisma.user.upsert({
  where: { email: "a@b.com" },
  create: { email: "a@b.com", name: "Ada" },
  update: { name: "Ada Lovelace" },
});
```

**Thoughts:** MySQL uses `ON DUPLICATE KEY UPDATE`. SQLite supports `ON CONFLICT` similarly. Without unique constraint, upsert is not available — add one first.

**Sources:**

[PostgreSQL — INSERT ON CONFLICT](https://www.postgresql.org/docs/current/sql-insert.html#SQL-ON-CONFLICT)

---

</details>

<details>
  <summary>Prepared statements & placeholders</summary>

### Prepared statements & placeholders

**Definition:**

A **prepared statement** sends SQL with placeholders (`$1`, `?`) once; values are bound separately — preventing SQL injection.

| Approach                    | Safe? | Notes                              |
| --------------------------- | ----- | ---------------------------------- |
| Parameterized query         | Yes   | `$1`, `?` placeholders             |
| ORM methods                 | Yes   | Prisma `findUnique`, Knex bindings |
| String concatenation        | No    | `'${email}'` is vulnerable         |
| `$queryRaw` tagged template | Yes   | Prisma tagged templates            |

Quotes and `;` in the bound value stay **data**. Identifiers (table/column names) cannot be placeholders — whitelist them.

**When to use:** Every query with user-controlled values.

**Examples:**

```ts
await pool.query("SELECT * FROM users WHERE email = $1", [email]);
await prisma.$queryRaw`SELECT * FROM users WHERE email = ${email}`;
```

**Thoughts:** Placeholders handle values only — never parameterize table or column names.

**Sources:** [OWASP — SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

</details>

<details>
  <summary>SQL Server (T-SQL)</summary>

### SQL Server (T-SQL)

**Definition:** **T-SQL** (Transact-SQL) is Microsoft SQL Server's dialect of SQL — same relational core as PostgreSQL (`SELECT`, `JOIN`, `GROUP BY`, transactions) with different syntax for limits, upserts, dates, and temp objects.

**How it works:**

Core SQL transfers: joins, subqueries, CTEs, window functions, `BEGIN` / `COMMIT` / `ROLLBACK`. Watch dialect traps in interviews and migrations:

| Task              | PostgreSQL                           | SQL Server (T-SQL)                                                |
| ----------------- | ------------------------------------ | ----------------------------------------------------------------- |
| Limit rows        | `LIMIT 10` / `OFFSET 20`             | `TOP (10)` or `ORDER BY … OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY` |
| Auto-increment PK | `SERIAL` / `GENERATED … AS IDENTITY` | `IDENTITY(1,1)` on column                                         |
| Upsert            | `INSERT … ON CONFLICT DO UPDATE`     | `MERGE` (or separate try/update)                                  |
| Current time      | `now()` / `CURRENT_TIMESTAMP`        | `GETDATE()` / `SYSDATETIME()`                                     |
| Null substitute   | `COALESCE(a, b)` (portable)          | `ISNULL(a, b)` (two args only)                                    |
| String concat     | `\|\|` or `concat()`                 | `+` (NULL poisons result — use `CONCAT()`)                        |
| Temp table        | `CREATE TEMP TABLE …`                | `#local` (session) / `##global` (shared)                          |
| Boolean           | native `BOOLEAN`                     | `BIT` (0/1)                                                       |
| Identifier quotes | double quotes `"col"`                | square brackets `[col]` or `"col"` in quoted mode                 |

**When to use:** Enterprise .NET stacks, Azure SQL, legacy on-prem SQL Server. Postgres-first teams still need T-SQL literacy for interviews and brownfield integrations.

**Pluses and minuses:**

| +                                      | −                                            |
| -------------------------------------- | -------------------------------------------- |
| Rich tooling (SSMS, Actual Plan)       | `MERGE` has edge-case bugs — test carefully  |
| Strong window-function / CTE support   | Licensing / cloud lock-in on Azure tiers     |
| Portable SQL core with Microsoft stack | Dialect differences trip ORMs and migrations |

**Examples:**

```sql
-- Top customers by order count (T-SQL)
SELECT TOP (10)
  u.email,
  COUNT(o.id) AS order_count
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
WHERE u.created_at >= '2025-01-01'
GROUP BY u.id, u.email
HAVING COUNT(o.id) > 5
ORDER BY order_count DESC;

-- Upsert with MERGE
MERGE users AS target
USING (SELECT @email AS email, @name AS name) AS source
ON target.email = source.email
WHEN MATCHED THEN UPDATE SET name = source.name, updated_at = GETDATE()
WHEN NOT MATCHED THEN INSERT (email, name, updated_at)
  VALUES (source.email, source.name, GETDATE());
```

**Thoughts:** `#temp` tables participate in statistics and can beat table variables for large sets. Compare plans: [Query optimization & EXPLAIN](#query-optimization-explain). Index model differs: [SQL Server indexes (clustered vs nonclustered)](#sql-server-indexes-clustered-vs-nonclustered). Postgres deep dive: [PostgreSQL](#postgresql).

**Sources:** [T-SQL language reference](https://learn.microsoft.com/en-us/sql/t-sql/language-reference) · [MERGE (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/statements/merge-transact-sql) · [SELECT — TOP clause](https://learn.microsoft.com/en-us/sql/t-sql/queries/top-transact-sql)

---

</details>

<details>
  <summary>SQL Server indexes (clustered vs nonclustered)</summary>

### SQL Server indexes (clustered vs nonclustered)

**Definition:** SQL Server splits indexes into **clustered** (defines table row order; at most one per table) and **nonclustered** (separate structure pointing to rows). PostgreSQL has no clustered index in this sense — see [Indexes](#indexes).

**How it works:**

| Type              | What it is                                        | Typical use                                 |
| ----------------- | ------------------------------------------------- | ------------------------------------------- |
| **Clustered**     | Table data stored in index key order              | Primary key — range scans, `ORDER BY` on PK |
| **Nonclustered**  | B-tree of key + pointer to row (or clustered key) | `WHERE` / `JOIN` on non-PK columns          |
| **Included cols** | Non-key columns stored in leaf (`INCLUDE`)        | Covering index — avoids key lookup          |
| **Columnstore**   | Compressed column batches                         | Analytics / large aggregations              |

Rules that differ from Postgres:

- Only **one** clustered index per table (often the PK).
- Nonclustered index on a heap uses **RID lookup**; on a clustered table uses **key lookup** to fetch missing columns.
- Filtered index: `CREATE INDEX … WHERE deleted_at IS NULL` (like Postgres partial index).
- `INCLUDE (col)` is SQL Server's covering pattern; Postgres uses `INCLUDE` on B-tree similarly.

**When to use:** Clustered on the main access path (usually PK). Nonclustered on FK + filter columns. Columnstore for warehouse/reporting — not default OLTP.

**Pluses and minuses:**

| +                                    | −                                              |
| ------------------------------------ | ---------------------------------------------- |
| `INCLUDE` reduces key lookups        | Wrong clustered key hurts inserts / hot splits |
| Filtered indexes shrink maintenance  | Too many nonclustered indexes slow writes      |
| Columnstore for scan-heavy analytics | Columnstore wrong for point lookups            |

**Examples:**

```sql
-- Clustered PK (default if PRIMARY KEY specified)
CREATE TABLE orders (
  id INT IDENTITY(1,1) PRIMARY KEY CLUSTERED,
  user_id INT NOT NULL,
  created_at DATETIME2 NOT NULL,
  total DECIMAL(10,2) NOT NULL
);

-- Nonclustered + covering INCLUDE
CREATE NONCLUSTERED INDEX idx_orders_user_created
ON orders (user_id, created_at DESC)
INCLUDE (total);

-- Filtered (soft-delete pattern)
CREATE NONCLUSTERED INDEX idx_users_active_email
ON users (email)
WHERE deleted_at IS NULL;
```

**Thoughts:** Interview line: “Clustered = table order, one per table; nonclustered = extra indexes for seeks.” Read plans: [Query optimization & EXPLAIN](#query-optimization-explain). General index tuning principles: [Indexes](#indexes).

**Sources:** [Clustered and nonclustered indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described) · [Create nonclustered indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/create-nonclustered-indexes) · [Columnstore indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-overview)

---

</details>

<details>
  <summary>PostgreSQL</summary>

### PostgreSQL

**Definition:** Open-source RDBMS with strong ACID, extensibility (JSONB, PostGIS, pgvector), and advanced SQL (CTEs, window functions, advisory locks).

**How it works:** Parser → rewriter (views/RLS) → planner (indexes, join type) → executor. Extensions (`pgcrypto`, PostGIS, pgvector) add types and functions in-process.

**When to use:** Default choice for new backend projects — OLTP, mixed relational + JSON, full-text search (with extensions).

**Pluses and minuses:**

| +                                    | −                                                                  |
| ------------------------------------ | ------------------------------------------------------------------ |
| Rich feature set, excellent JSONB    | Single-writer primary — write scaling needs sharding/read replicas |
| Mature extensions ecosystem          | Tuning requires DBA knowledge at scale                             |
| Strong community and hosting options | Connection limits — use pooling (PgBouncer)                        |

**Examples:**

```sql
-- JSONB query
SELECT data->>'name' AS name
FROM products
WHERE data @> '{"category": "electronics"}';

-- Recursive CTE — org tree
WITH RECURSIVE tree AS (
  SELECT id, parent_id, name FROM nodes WHERE parent_id IS NULL
  UNION ALL
  SELECT n.id, n.parent_id, n.name
  FROM nodes n JOIN tree t ON n.parent_id = t.id
)
SELECT * FROM tree;
```

**Thoughts:**

- **Advisory locks** — app-level mutex: `SELECT pg_advisory_lock(123)`.
- **LISTEN/NOTIFY** — lightweight pub/sub inside Postgres (not a replacement for Redis/Kafka).
- **pgvector** — vector similarity search without a separate vector DB for moderate scale.

#### PostgreSQL ops cheatsheet

### PostgreSQL ops cheatsheet

```bash
# macOS (Homebrew)
brew services start postgresql@16
psql -U postgres

# Common psql commands
\l          -- list databases
\dt         -- list tables
\d users    -- describe table
\q          -- quit
```

```sql
CREATE DATABASE myapp;
GRANT ALL PRIVILEGES ON SCHEMA public TO myapp_user;
```

**Sources:** [PostgreSQL documentation](https://www.postgresql.org/docs/current/)

---

</details>
