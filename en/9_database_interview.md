# Database — Interview

Answers are filled for every question in this file.

[← Back to hub](./9_database.md)

### Related docs

- [DB — Core](./9_database_core.md) · [DB — SQL](./9_database_sql.md) · [DB — NoSQL](./9_database_nosql.md) · [DB — ORM](./9_database_orm.md) · [DB — Production](./9_database_production.md) · [DB — Interview](./9_database_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — SQL](https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/).

### SQL Fundamentals

<details>
<summary>What is SQL and what is it used for?</summary>

### What is SQL and what is it used for?

**Definition:**

SQL (Structured Query Language). SQL is the standardized domain-specific, declarative language for managing and querying Relational Database Management Systems (RDBMS). Beyond legacy relational models, modern SQL dialects (e.g., PostgreSQL 18, DuckDB 1.2) now incorporate support for semi-structured data (JSONB) and vector embeddings for RAG (Retrieval-Augmented Generation) workflows.

**How it works:**

Core Components: DDL (`CREATE`, `ALTER`, `DROP`). DML (`INSERT`, `UPDATE`, `DELETE`, `MERGE`). DCL (`GRANT`, `REVOKE`). TCL (`COMMIT`, `ROLLBACK`, `SAVEPOINT`). DQL (`SELECT`).

Essential commands: `SELECT` with CTEs and window functions (`OVER`, `PARTITION BY`); `MERGE` upsert; `LATERAL JOIN`; `MATERIALIZED VIEW`.

**When to use:**

Modern tasks: data retrieval with aggregation and window functions; ACID; vector search with pgvector and k-NN; normalization and B-Tree, LSM-Tree, BRIN indexes for O(log n) point lookups; sharding and replication.

**Examples:**

```sql
-- Schema Definition
CREATE TABLE Department (
    dept_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL
);
CREATE TABLE Employee (
    emp_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    emp_name VARCHAR(255) NOT NULL,
    dept_id INT REFERENCES Department(dept_id)
);
WITH DepartmentStats AS (
    SELECT dept_id, COUNT(*) as emp_count
    FROM Employee
    GROUP BY dept_id
)
SELECT e.emp_name, d.dept_name, ds.emp_count
FROM Employee e
JOIN Department d ON e.dept_id = d.dept_id
JOIN DepartmentStats ds ON d.dept_id = ds.dept_id
WHERE ds.emp_count > 0;
```

**Thoughts:**

2026 performance: Index Scan vs Seq Scan; `EXPLAIN ANALYZE`; MVCC so readers do not block writers.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Describe the difference between SQL and NoSQL databases.</summary>

### Describe the difference between SQL and NoSQL databases.

**Definition:**

SQL uses the relational model (Codd, 1970), with ACID and rigid schemas. NoSQL is built around horizontal scaling and the CAP theorem, and is often AP with eventual consistency.

**How it works:**

Taxonomy: document, key-value, wide-column, graph.

Comparison: schema-on-write vs schema-on-read; constraints vs app validation; joins vs denormalization; scaling; declarative SQL vs a proprietary API. The same split shows up as normalization vs denormalization, and as an ACID note.

Transaction models: 2PC / Paxos / Raft vs vector clocks / LWW.

2026: PostgreSQL JSONB; MongoDB multi-document ACID; distributed SQL (CockroachDB, TiDB, YugabyteDB).

**When to use:**

SQL for integrity. NoSQL for high-velocity, sparse, or evolving schema. Distributed SQL when you want SQL plus elastic scale.

**Pluses and minuses:**

- SQL gives constraints, joins, and ACID. The schema is rigid, and scaling out is the harder axis.
- NoSQL gives horizontal scale and a looser schema. It often trades that for eventual consistency and a proprietary API.
- Distributed SQL keeps declarative SQL and adds elastic scale. Coordination (2PC, Paxos, Raft) is the cost.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What are the different types of SQL commands?</summary>

### What are the different types of SQL commands?

**Definition:**

SQL commands are grouped as DQL, DDL, DML, TCL, and DCL.

**How it works:**

- DQL: `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, window functions, `JOIN`, CTE.
- DDL: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`.
- DML: `INSERT`, `UPDATE`, `DELETE`, `MERGE`.
- TCL: `COMMIT`, `ROLLBACK`, `SAVEPOINT`.
- DCL: `GRANT`, `REVOKE`, `DENY`.

**Thoughts:**

DQL is a subset of DML in SQL:2023/2025, but it is kept separate for teaching.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Explain the purpose of the SELECT statement.</summary>

### Explain the purpose of the SELECT statement.

**Definition:**

`SELECT` retrieves, projects, and transforms.

**How it works:**

Logical order: `FROM` / `JOIN`, `WHERE`, `GROUP BY`, `HAVING`, `SELECT`, `DISTINCT`, `ORDER BY`, `LIMIT` / `OFFSET`.

The same statement covers JSONB, vectors, window functions, and CTEs.

**Examples:**

```sql
WITH OrderSummary AS (
    SELECT
        o.order_id,
        c.customer_name,
        e.employee_name,
        SUM(od.unit_price * od.quantity) AS TotalValue
    FROM Orders o
    JOIN Customers c ON o.customer_id = c.customer_id
    JOIN Employees e ON o.employee_id = e.employee_id
    JOIN OrderDetails od ON o.order_id = od.order_id
    GROUP BY o.order_id, c.customer_name, e.employee_name
)
SELECT order_id, customer_name, employee_name, TotalValue
FROM OrderSummary
WHERE TotalValue > 1000
LIMIT 100;
```

**Thoughts:**

`EXPLAIN ANALYZE` shows whether the plan is a nested loop or a hash join.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What is the difference between WHERE and HAVING clauses?</summary>

### What is the difference between WHERE and HAVING clauses?

**Definition:**

`WHERE` is the pre-aggregation row filter. `HAVING` is the post-aggregation group filter.

**How it works:**

`WHERE` is SARGable and cannot use aggregates. `HAVING` can use `COUNT` and `SUM`.

Pipeline: `FROM` / `JOIN`, `WHERE`, `GROUP BY`, `HAVING`, `SELECT`, `ORDER BY`.

**Thoughts:**

Optimizers may rewrite an aggregate-free `HAVING` into `WHERE`.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Define what a JOIN is in SQL and list its types.</summary>

### Define what a JOIN is in SQL and list its types.

**Definition:**

`JOIN` combines rows on a predicate.

**How it works:**

Nested loop is O(n\*m). Hash and sort-merge are O(n+m) on average.

Types: `INNER`, `LEFT`, `RIGHT`, `FULL`, `CROSS`, `SELF`.

**Examples:**

```sql
SELECT e.emp_name, d.dept_name
FROM Employee e
INNER JOIN Department d ON e.dept_id = d.dept_id;
```

```sql
SELECT COALESCE(e.dept_id, d.dept_id) AS dept_id, e.emp_name, d.dept_name
FROM Employee e
FULL JOIN Department d ON e.dept_id = d.dept_id;
```

```sql
WITH RECURSIVE Hierarchy AS (
    SELECT emp_id, emp_name, manager_id, 1 AS level
    FROM Employee
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.emp_name, e.manager_id, h.level + 1
    FROM Employee e
    JOIN Hierarchy h ON e.manager_id = h.emp_id
)
SELECT emp_id, emp_name, manager_id, level
FROM Hierarchy;
```

```sql
SELECT E.Name AS Employee, M.Name AS Manager FROM Employee E LEFT JOIN Employee M ON E.ManagerID = M.EmpID;
```

**Thoughts:**

Avoid `SELECT *`. Hash distribution hints.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What is a primary key in a database?</summary>

### What is a primary key in a database?

**Definition:**

A primary key is unique and `NOT NULL`, and it should be immutable.

**How it works:**

It enforces entity integrity, it is the target of a foreign key, and it is often the clustered index. Prefer surrogate keys: `BIGINT` identity or UUIDv7, and keep the key narrow.

**Examples:**

```sql
CREATE TABLE Students (
    student_id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    student_uuid UUID NOT NULL,
    student_name VARCHAR(255) NOT NULL
);

CREATE UNIQUE INDEX students_uuid_uidx ON Students (student_uuid);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Explain what a foreign key is and how it is used.</summary>

### Explain what a foreign key is and how it is used.

**Definition:**

A foreign key enforces referential integrity to a primary key or a unique key.

**How it works:**

`NULL` is allowed unless the column is `NOT NULL`. `ON DELETE` and `ON UPDATE` say what happens when the parent row changes. Foreign-key columns are not automatically indexed; add an index. The child foreign key need not be unique: that is the one-to-many case.

**Examples:**

```sql
CREATE TABLE departments (
    dept_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    emp_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    emp_name VARCHAR(255) NOT NULL,
    dept_id INT,
    CONSTRAINT fk_department
        FOREIGN KEY (dept_id)
        REFERENCES departments (dept_id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE
);

CREATE INDEX employees_dept_id_idx ON employees (dept_id);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>How can you prevent SQL injections?</summary>

### How can you prevent SQL injections?

**Definition:**

Parameterized queries come first.

**How it works:**

Bind values instead of concatenating them into the statement. Stored procedures help only if they do not build SQL strings. Allow-list validation checks identifiers and sort columns before they are interpolated. Least privilege limits what a successful injection can do.

**Pluses and minuses:**

Manual regex escaping is an anti-pattern. Use ORMs and bound parameters.

**Examples:**

```python
from sqlalchemy import text

stmt = text("SELECT id, email FROM users WHERE email = :email")
rows = connection.execute(stmt, {"email": email})
```

```python
from typing import Literal

from pydantic import BaseModel

class UserQuery(BaseModel):
    order_by: Literal["id", "email", "created_at"]
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What is normalization? Explain with examples.</summary>

### What is normalization? Explain with examples.

**Definition:**

Normalization removes repeating and dependent facts so each fact is stored once: 1NF is atomic, 2NF has no partial dependency, 3NF has no transitive dependency, then BCNF and 4NF.

**How it works:**

An unnormalized invoice table repeats the customer and packs several products into one row. Split it into `Customers`, `Invoices`, `Invoice_Items`, and `Products`.

**When to use:**

OLTP stays in 3NF. OLAP uses a star schema.

**Examples:**

```sql
CREATE TABLE Products (
    product_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    product_name VARCHAR(255) NOT NULL,
    unit_price NUMERIC(10, 2) NOT NULL
);

CREATE TABLE Customers (
    customer_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    customer_name VARCHAR(255) NOT NULL
);

CREATE TABLE Invoices (
    invoice_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    customer_id INT NOT NULL REFERENCES Customers (customer_id),
    invoice_date DATE NOT NULL
);

CREATE TABLE Invoice_Items (
    invoice_id INT NOT NULL REFERENCES Invoices (invoice_id),
    product_id INT NOT NULL REFERENCES Products (product_id),
    quantity INT NOT NULL,
    PRIMARY KEY (invoice_id, product_id)
);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Describe the concept of denormalization and when you would use it.</summary>

### Describe the concept of denormalization and when you would use it.

**Definition:**

Denormalization is intentional redundancy for read latency.

**How it works:**

The usual shapes are flattening, materialized views, redundant attributes, and a star schema. OLTP stays in 3NF. OLAP denormalizes. Sharding uses the copy for locality.

**When to use:**

Prefer materialized views or read replicas. Use physical denormalization only when joins dominate.

**Pluses and minuses:**

Reads get cheaper. Consistency then depends on CDC, writes amplify, and schema evolution gets harder because the same fact lives in more than one place.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What are indexes and how can they improve query performance?</summary>

### What are indexes and how can they improve query performance?

**Definition:**

Indexes are auxiliary structures that turn an O(n) scan into an O(log n) or O(1) lookup.

**How it works:**

Common forms: B+Tree, LSM, hash, BRIN, GIN / GiST.

Practices: `EXPLAIN ANALYZE`, partial indexes, left-prefix composite indexes, `INCLUDE` covering indexes, and fresh statistics. Primary and unique keys create indexes. Foreign keys often do not.

**Pluses and minuses:**

Lookups and some sorts get cheaper. The costs are write amplification, extra storage, and little help when selectivity is low.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Explain the purpose of the GROUP BY clause.</summary>

### Explain the purpose of the GROUP BY clause.

**Definition:**

`GROUP BY` partitions rows for aggregates.

**How it works:**

The engine can hash-aggregate or stream-aggregate. `ONLY_FULL_GROUP_BY` rejects a selected column that is neither grouped nor aggregated. Partition-wise aggregation can run the group per partition.

**Examples:**

```sql
SELECT Region, SUM(Amount) AS total_amount
FROM Sales
GROUP BY Region;
```

```sql
SELECT Region, COUNT(*) AS sale_count
FROM Sales
WHERE Amount > 100
GROUP BY Region
HAVING COUNT(*) > 50;
```

```sql
SELECT
    Region,
    Amount,
    Amount / SUM(Amount) OVER (PARTITION BY Region) AS relative_contribution
FROM Sales;
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What is a subquery, and when would you use one?</summary>

### What is a subquery, and when would you use one?

**Definition:**

A subquery is an inner `SELECT`.

**How it works:**

A scalar subquery returns one value. A table subquery returns a relation. A non-correlated subquery can run once. A correlated subquery refers to the outer row.

**When to use:**

Prefer CTEs, window functions, and `LATERAL` when the nested form is only there to compute a value per group or per outer row.

**Examples:**

```sql
WITH AvgValue AS (
    SELECT AVG(amount) AS avg_amount
    FROM Orders
)
SELECT o.order_id, o.amount
FROM Orders o
JOIN AvgValue a ON o.amount > a.avg_amount;
```

```sql
SELECT c.customer_id, lo.order_id, lo.ordered_at
FROM Customers c
CROSS JOIN LATERAL (
    SELECT o.order_id, o.ordered_at
    FROM Orders o
    WHERE o.customer_id = c.customer_id
    ORDER BY o.ordered_at DESC
    LIMIT 1
) lo;
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Describe the functions of the ORDER BY clause.</summary>

### Describe the functions of the ORDER BY clause.

**Definition:**

`ORDER BY` sorts an otherwise unordered result.

**How it works:**

Each key is `ASC` or `DESC`, with `NULLS FIRST` or `NULLS LAST`. The sort is O(N log N). An index that already matches the order can make it O(N).

**Examples:**

```sql
SELECT product_id, units_sold
FROM sales
ORDER BY units_sold DESC NULLS LAST;
```

```sql
SELECT product_id, units_sold
FROM (
    SELECT
        product_id,
        units_sold,
        RANK() OVER (ORDER BY units_sold DESC) AS rnk
    FROM sales
) ranked
WHERE rnk <= 10;
```

**Thoughts:**

Discourage `ORDER BY 1`. `ORDER BY RANDOM()` is expensive.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>What are aggregate functions in SQL?</summary>

### What are aggregate functions in SQL?

**Definition:**

An aggregate function computes one value from a set of input rows. General-purpose aggregates include `count`, `sum`, `avg`, `min`, `max`, `bool_and`, `bool_or`, `array_agg`, `string_agg`, `json_agg`, and `jsonb_agg`.

**How it works:**

Most aggregates skip null inputs. `count(*)` counts input rows. `count(expression)` counts rows where the expression is not null. `avg` of an integer argument returns `numeric`. Aggregates are normally used with `GROUP BY`; without it, the query is one group. `FILTER (WHERE condition)` restricts which rows enter that aggregate. `DISTINCT` inside the aggregate, as in `count(DISTINCT city)`, collapses duplicate inputs first.

**Examples:**

```sql
SELECT dept_id,
       count(*) AS headcount,
       avg(salary) AS avg_salary,
       count(*) FILTER (WHERE active) AS active_count
FROM employee
GROUP BY dept_id;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-aggregate.html
- https://www.postgresql.org/docs/current/queries-table-expressions.html
</details>

<details>
<summary>Explain the differences between INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.</summary>

### Explain the differences between INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.

**Definition:**

These joins combine rows from two tables using a match condition. `INNER` keeps only matches. `LEFT`, `RIGHT`, and `FULL` also keep unmatched rows from one side or both, filling the other side with nulls.

**How it works:**

`INNER JOIN` returns a row only when the `ON` condition is true. `LEFT JOIN` (the same as `LEFT OUTER JOIN`) returns every left row; unmatched right columns are null. `RIGHT JOIN` returns every right row. `FULL JOIN` returns every row from both sides. `USING (col)` merges the shared column into one output column. `OUTER` is optional.

**When to use:**

Use `INNER` when both sides must match. Use `LEFT` when the driving table must stay complete, including rows with no match. Prefer swapping the tables and writing `LEFT` instead of `RIGHT` when that reads more clearly. Use `FULL` when unmatched rows on both sides matter.

**Examples:**

```sql
SELECT t1.num, t1.name, t2.value
FROM t1
LEFT JOIN t2 ON t1.num = t2.num;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-table-expressions.html
- https://www.postgresql.org/docs/current/tutorial-join.html
</details>

<details>
<summary>How do you insert a new row into a database table?</summary>

### How do you insert a new row into a database table?

**Definition:**

`INSERT` adds rows to a table. You name the target columns and supply values with `VALUES`, a query, or `DEFAULT`.

**How it works:**

Omitted columns take their default, or null if they have none and are nullable. `RETURNING` gives back expressions computed from each inserted row. `ON CONFLICT` turns a unique-violation into `DO NOTHING` or `DO UPDATE`. You need `INSERT` privilege on the table.

**Examples:**

```sql
INSERT INTO films (code, title, did)
VALUES ('UA502', 'Bananas', DEFAULT)
RETURNING code, title;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-insert.html
</details>

<details>
<summary>Explain how to update records in a database table.</summary>

### Explain how to update records in a database table.

**Definition:**

`UPDATE` changes columns in rows that satisfy a `WHERE` condition. Columns not listed in `SET` keep their previous values.

**How it works:**

The new value can be an expression, `DEFAULT`, or a one-row sub-`SELECT`. A `FROM` clause can join other tables into the assignment, but each target row should match at most one joined row or the chosen source row is not predictable. `RETURNING` can expose the new values, and `OLD.column` the previous ones. Without `WHERE`, every row is updated.

**Examples:**

```sql
UPDATE films
SET kind = 'Dramatic'
WHERE kind = 'Drama'
RETURNING code, kind, OLD.kind AS old_kind;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-update.html
</details>

<details>
<summary>What is a SQL View and what are its advantages?</summary>

### What is a SQL View and what are its advantages?

**Definition:**

A view is a named query. `CREATE VIEW` does not store the result; the query runs again each time the view is referenced.

**How it works:**

The view name must be unique among relations in the schema. A simple view is automatically updatable when it has one base relation and no `DISTINCT`, `GROUP BY`, `HAVING`, `LIMIT`, `OFFSET`, set operation, aggregate, window function, or set-returning function at the top level. `WITH CHECK OPTION` rejects `INSERT`, `UPDATE`, and `MERGE` rows that would not be visible through the view. By default, access to base tables is checked as the view owner. `security_invoker = true` checks them as the invoking user.

**When to use:**

Use a view to reuse a query, present a stable column list, or grant access to a subset of a table. Use a materialized view when you need a stored result instead of a query that runs every time.

**Examples:**

```sql
CREATE VIEW comedies AS
    SELECT *
    FROM films
    WHERE kind = 'Comedy';
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createview.html
</details>

### SQL Data Types and Operators

<details>
<summary>List the different data types available in SQL.</summary>

### List the different data types available in SQL.

**Definition:**

PostgreSQL types cover booleans, numbers, character strings, binary data, dates and times, network addresses, JSON, arrays, ranges, enumerated types, and geometric values, plus domains and composite types you define.

**How it works:**

Numeric types include `smallint`, `integer`, `bigint`, `numeric`, `real`, and `double precision`. Character types are `character varying(n)`, `character(n)`, and `text`. Date and time types include `date`, `time`, `timestamp`, `timestamptz`, and `interval`. `boolean` is `true` or `false`. `bytea` holds raw bytes. `json` and `jsonb` hold JSON. `uuid`, `inet`, `cidr`, and arrays such as `integer[]` are built in. `numeric` is exact; `real` and `double precision` are approximate.

**When to use:**

Pick `numeric` for exact quantities such as money. Pick `text` or `varchar` for strings. Pick `timestamptz` when the value is an instant that must survive time-zone changes. Pick `jsonb` when the payload is JSON you will query.

**Examples:**

```sql
CREATE TABLE ledger (
    entry_id bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    amount numeric(12, 2) NOT NULL,
    note text,
    posted_at timestamptz NOT NULL DEFAULT now(),
    tags text[]
);
```

**Sources:**

- https://www.postgresql.org/docs/current/datatype.html
- https://www.postgresql.org/docs/current/datatype-numeric.html
- https://www.postgresql.org/docs/current/datatype-datetime.html
</details>

<details>
<summary>What are the differences between CHAR, VARCHAR, and TEXT data types?</summary>

### What are the differences between CHAR, VARCHAR, and TEXT data types?

**Definition:**

`character(n)` (`char(n)`) is fixed-length and blank-padded. `character varying(n)` (`varchar(n)`) is variable-length with a character limit. `text` is variable-length with no declared limit.

**How it works:**

The length `n` counts characters, not bytes, and must be from 1 to 10485760. Storing a longer string raises an error unless the extra characters are spaces, which are truncated. An explicit cast to `varchar(n)` or `char(n)` truncates without an error. Short `character(n)` values are stored padded with spaces; trailing spaces are ignored in comparisons of `character` values and stripped when converting to `varchar` or `text`. Trailing spaces in `varchar` and `text` are significant, including for `LIKE`. `varchar` without `n` accepts any length. There is no performance advantage to `character(n)` in PostgreSQL; the blank padding costs space, and the length check costs a little CPU. The longest stored string is about 1 GB.

**When to use:**

Prefer `text` or `character varying`. Use `character(n)` only when a blank-padded fixed width is part of the data contract.

**Examples:**

```sql
CREATE TABLE test1 (a character(4));
INSERT INTO test1 VALUES ('ok');
SELECT a, char_length(a) FROM test1;
```

**Sources:**

- https://www.postgresql.org/docs/current/datatype-character.html
</details>

<details>
<summary>How do you use the BETWEEN operator in SQL?</summary>

### How do you use the BETWEEN operator in SQL?

**Definition:**

`BETWEEN` tests whether a value lies inside a range, including both ends.

**How it works:**

`a BETWEEN x AND y` is equivalent to `a >= x AND a <= y`. If `x` is greater than `y`, the predicate is never true. `BETWEEN SYMMETRIC` treats the two bounds as unordered, so the lesser and greater values are used as the ends. `NOT BETWEEN` negates the test. A null operand yields null, not true.

**Examples:**

```sql
SELECT city, temp_lo
FROM weather
WHERE temp_lo BETWEEN 40 AND 50;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-comparison.html
</details>

<details>
<summary>Describe the use of the IN operator.</summary>

### Describe the use of the IN operator.

**Definition:**

`IN` tests membership in a list of values or in the result of a subquery.

**How it works:**

`expression IN (value1, value2, ...)` is true when the expression equals any listed value. `expression IN (subquery)` is true when it equals any row the subquery returns. `NOT IN` is the negation. If the expression is null, the result is null. If any list or subquery value is null and no equal value is found, `NOT IN` yields null rather than true, so the row is filtered out of a `WHERE` clause.

**Examples:**

```sql
SELECT title
FROM films
WHERE kind IN ('Comedy', 'Drama');
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-comparison.html
</details>

<details>
<summary>Explain the use of wildcard characters in SQL.</summary>

### Explain the use of wildcard characters in SQL.

**Definition:**

In `LIKE` and `ILIKE` patterns, `%` matches any sequence of characters, including the empty sequence, and `_` matches exactly one character.

**How it works:**

The pattern is not a regular expression. To match a literal `%` or `_`, declare an escape character: `LIKE '100\%' ESCAPE '\'`. `SIMILAR TO` uses SQL regular-expression wildcards, and `~` uses POSIX regular expressions. A B-tree index can support `LIKE` only when the pattern is a constant anchored at the start, such as `'foo%'`, and often needs a pattern-ops operator class unless the collation is `C`.

**Examples:**

```sql
SELECT name
FROM cities
WHERE name LIKE 'San%';
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-matching.html
- https://www.postgresql.org/docs/current/indexes-types.html
</details>

<details>
<summary>What is the purpose of the LIKE operator?</summary>

### What is the purpose of the LIKE operator?

**Definition:**

`LIKE` matches a string against a pattern. It is case-sensitive. `ILIKE` is the case-insensitive PostgreSQL form. `NOT LIKE` and `NOT ILIKE` negate the test.

**How it works:**

`string LIKE pattern` returns true, false, or null. If either argument is null, the result is null. The default escape character is a backslash. `~~` is the operator equivalent of `LIKE`, and `~~*` of `ILIKE`. Trailing spaces are significant for `text` and `varchar`.

**When to use:**

Use `LIKE` for simple prefix, suffix, or single-character patterns. Use POSIX operators (`~`, `~*`) when the pattern needs regular-expression features that `%` and `_` do not provide.

**Examples:**

```sql
SELECT name
FROM customers
WHERE name ILIKE 'ann%';
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-matching.html
</details>

<details>
<summary>How do you handle NULL values in SQL?</summary>

### How do you handle NULL values in SQL?

**Definition:**

Null means the value is unknown or absent. It is not equal to empty string, zero, or another null.

**How it works:**

Comparisons with `=` or `<>` involving null produce null, so they do not succeed in `WHERE`. Test null with `IS NULL` and `IS NOT NULL`. `IS DISTINCT FROM` treats null as an ordinary value and returns a boolean. Most aggregates skip nulls. `count(*)` still counts the row. A `UNIQUE` constraint treats nulls as distinct unless you declare `NULLS NOT DISTINCT`, so more than one null is allowed by default. `COALESCE` and `NULLIF` substitute or manufacture nulls. `ORDER BY ... NULLS FIRST` or `NULLS LAST` places them explicitly.

**Examples:**

```sql
SELECT id, name
FROM customers
WHERE email IS NULL
   OR phone IS DISTINCT FROM email;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-comparison.html
- https://www.postgresql.org/docs/current/ddl-constraints.html
</details>

<details>
<summary>What does the COALESCE function do?</summary>

### What does the COALESCE function do?

**Definition:**

`COALESCE(value [, ...])` returns the first argument that is not null. It returns null only when every argument is null.

**How it works:**

Arguments must be convertible to one result type. Arguments after the first non-null argument are not evaluated. It is the SQL-standard equivalent of `NVL` and `IFNULL` in other systems. `NULLIF(value1, value2)` is the inverse pattern: it returns null when the two values are equal, otherwise `value1`.

**Examples:**

```sql
SELECT COALESCE(description, short_description, '(none)')
FROM products;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-conditional.html
</details>

<details>
<summary>What is the difference between UNION and UNION ALL?</summary>

### What is the difference between UNION and UNION ALL?

**Definition:**

`UNION` combines the results of two queries and removes duplicate rows. `UNION ALL` combines them and keeps every row, including duplicates.

**How it works:**

The queries must return the same number of columns, and matching columns must have compatible types. The column names come from the first query. `UNION` implies a duplicate-elimination step. `INTERSECT` keeps rows present in both results, and `EXCEPT` keeps rows from the first that are not in the second. `ALL` variants of those operators keep duplicates as well. An `ORDER BY` applies to the combined result and is written once at the end.

**When to use:**

Use `UNION ALL` when duplicates are impossible or you want to keep them, because it avoids the distinct sort or hash. Use `UNION` when the caller must see each distinct row once.

**Examples:**

```sql
SELECT city FROM weather
UNION
SELECT name FROM cities
ORDER BY 1;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-union.html
</details>

<details>
<summary>Describe the use of arithmetic operators in SQL queries.</summary>

### Describe the use of arithmetic operators in SQL queries.

**Definition:**

PostgreSQL numeric operators include `+`, `-`, `*`, `/`, and `%` (remainder). Unary `+` and `-` are also valid.

**How it works:**

The argument types decide the result type. Dividing two integers truncates toward zero; `5 / 2` is `2`. Cast at least one side when you need a fractional result: `5::numeric / 2`. `%` is the remainder. `div(y, x)` is integer division. A divisor of zero raises an error. Null in an arithmetic expression yields null.

**Examples:**

```sql
SELECT amount,
       amount * 0.06 AS tax,
       amount % 10 AS remainder
FROM invoices;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-math.html
</details>

### SQL Advanced Queries

<details>
<summary>Explain how to use the CASE statement in SQL.</summary>

### Explain how to use the CASE statement in SQL.

**Definition:**

`CASE` is a conditional expression. It can appear anywhere an expression is allowed. It is not a statement that stands alone.

**How it works:**

The searched form is `CASE WHEN condition THEN result ... ELSE result END`. The first true condition wins, and later branches are not evaluated. If no condition is true and there is no `ELSE`, the result is null. The simple form is `CASE expression WHEN value THEN result ... END`, which compares the expression to each value with `=`. All result arms must share a common type.

**Examples:**

```sql
SELECT a,
       CASE WHEN a = 1 THEN 'one'
            WHEN a = 2 THEN 'two'
            ELSE 'other'
       END
FROM test;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-conditional.html
</details>

<details>
<summary>How would you perform a self JOIN?</summary>

### How would you perform a self JOIN?

**Definition:**

A self join uses the same table twice in one query. Each occurrence needs its own alias, because the table name alone would be ambiguous.

**How it works:**

The join condition relates columns of the two aliases, the same way it would for two different tables. This is the usual way to pair a row with a related row in the same table, such as an employee and that employee's manager.

**Examples:**

```sql
SELECT e.emp_name AS employee, m.emp_name AS manager
FROM employee AS e
LEFT JOIN employee AS m ON e.manager_id = m.emp_id;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-table-expressions.html
</details>

<details>
<summary>What is a cross JOIN and when would you use it?</summary>

### What is a cross JOIN and when would you use it?

**Definition:**

`CROSS JOIN` produces the Cartesian product: every row of the first table paired with every row of the second. It has no join condition.

**How it works:**

If the inputs have `N` and `M` rows, the result has `N * M` rows. A comma in `FROM a, b` is also a cross join. `CROSS JOIN LATERAL` is different: the right side can refer to the current left row.

**When to use:**

Use it for a deliberate combination of two small sets, such as every date with every product. Avoid it on large tables when you meant an equijoin and forgot `ON`.

**Examples:**

```sql
SELECT *
FROM t1
CROSS JOIN t2;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-table-expressions.html
</details>

<details>
<summary>How to implement pagination in SQL queries?</summary>

### How to implement pagination in SQL queries?

**Definition:**

Pagination returns one slice of an ordered result. PostgreSQL supports `LIMIT` / `OFFSET` and the standard `OFFSET ... FETCH` clause.

**How it works:**

`LIMIT count` stops after that many rows. `OFFSET start` skips that many rows first. `FETCH FIRST count ROWS ONLY` is the standard spelling of `LIMIT`. The planner still has to produce the skipped rows, so a large offset gets slower as the page number grows. A keyset (seek) page uses the last seen sort key in `WHERE` and then `LIMIT`, which stays cheap when that key is indexed.

**When to use:**

`LIMIT` and `OFFSET` are enough for small page numbers. Use a keyset condition when users can jump deep into a large ordered set and the sort key is unique and indexed.

**Examples:**

```sql
SELECT id, title
FROM films
ORDER BY id
LIMIT 20 OFFSET 40;
```

```sql
SELECT id, title
FROM films
WHERE id > 40
ORDER BY id
LIMIT 20;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-limit.html
</details>

<details>
<summary>Explain the concept of Common Table Expressions (CTEs) and recursive CTEs.</summary>

### Explain the concept of Common Table Expressions (CTEs) and recursive CTEs.

**Definition:**

A `WITH` clause names auxiliary queries, called common table expressions, for one primary statement. `WITH RECURSIVE` lets a CTE refer to its own output.

**How it works:**

Each CTE can be a `SELECT`, `INSERT`, `UPDATE`, `DELETE`, or `MERGE`. The primary statement can be one of those as well. A non-recursive CTE can be inlined by the planner unless you write `AS MATERIALIZED`. A recursive CTE has a non-recursive term, then `UNION` or `UNION ALL`, then a recursive term that is the only place allowed to reference the CTE name. Evaluation is iterative: the working table starts as the non-recursive result and is replaced by each new recursive step until that step returns no rows. `UNION` discards duplicates; `UNION ALL` does not.

**When to use:**

Use a CTE to name a subquery that the rest of the statement reads more than once, or to make a long query readable. Use `RECURSIVE` for trees and graphs stored as adjacency lists. Add a depth or path stop so a cycle cannot run forever.

**Examples:**

```sql
WITH RECURSIVE t(n) AS (
    VALUES (1)
  UNION ALL
    SELECT n + 1 FROM t WHERE n < 100
)
SELECT sum(n) FROM t;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-with.html
</details>

<details>
<summary>What are window functions and how are they used?</summary>

### What are window functions and how are they used?

**Definition:**

A window function calculates a value across a set of rows related to the current row, without collapsing those rows the way `GROUP BY` does.

**How it works:**

The call is followed by `OVER (PARTITION BY ... ORDER BY ... frame)`. `PARTITION BY` splits the input into independent windows. `ORDER BY` orders rows inside the partition. The frame defaults depend on whether `ORDER BY` is present. `row_number`, `rank`, `dense_rank`, `lag`, `lead`, and aggregates such as `sum` can all be window functions. A window function is not allowed in `WHERE`; filter its result in an outer query.

**Examples:**

```sql
SELECT depname, empno, salary,
       row_number() OVER (
           PARTITION BY depname
           ORDER BY salary DESC, empno
       ) AS pos
FROM empsalary;
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-window.html
- https://www.postgresql.org/docs/current/functions-window.html
</details>

<details>
<summary>How can you concatenate column values in SQL?</summary>

### How can you concatenate column values in SQL?

**Definition:**

The `||` operator concatenates two strings. `concat` concatenates any number of values. `concat_ws` inserts a separator. `string_agg` concatenates values across rows.

**How it works:**

`'Post' || 'greSQL'` is `PostgreSQL`. If either side of `||` is null, the result is null. `concat` ignores null arguments: `concat('abcde', 2, NULL, 22)` is `abcde222`. `concat_ws(',', 'a', NULL, 'b')` is `a,b`. At least one `||` input must be a string unless you cast. `string_agg(expression, delimiter ORDER BY ...)` is the aggregate form.

**Examples:**

```sql
SELECT concat_ws(' ', first_name, last_name) AS full_name
FROM customers;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-string.html
- https://www.postgresql.org/docs/current/functions-aggregate.html
</details>

<details>
<summary>What is the PIVOT operation and how would you apply it?</summary>

### What is the PIVOT operation and how would you apply it?

**Definition:**

A pivot turns row values into columns. PostgreSQL has no `PIVOT` keyword. The same result is built with conditional aggregates, or with the `crosstab` function from the `tablefunc` extension.

**How it works:**

`count(*) FILTER (WHERE status = 'open')` counts only matching rows inside the group. `sum(CASE WHEN status = 'open' THEN amount END)` is the same idea for a searched expression. `crosstab(source_sql)` expects a query that returns row name, category, and value, ordered by row name and category, and you declare the output columns in the `AS` alias.

**When to use:**

Prefer `FILTER` or `CASE` when the category list is known in the query and you want to stay in core SQL. Use `crosstab` when you already depend on `tablefunc` and want its pivot helper.

**Examples:**

```sql
SELECT dept_id,
       count(*) FILTER (WHERE status = 'open') AS open_count,
       count(*) FILTER (WHERE status = 'closed') AS closed_count
FROM tickets
GROUP BY dept_id;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-aggregate.html
- https://www.postgresql.org/docs/current/tablefunc.html
</details>

<details>
<summary>Explain the process of combining a query that uses a GROUP BY with one that uses ORDER BY.</summary>

### Explain the process of combining a query that uses a GROUP BY with one that uses ORDER BY.

**Definition:**

`GROUP BY` builds groups and computes aggregates. `ORDER BY` then sorts the grouped result. Grouping does not promise any output order.

**How it works:**

`WHERE` filters input rows before grouping. `HAVING` filters groups after aggregates are computed. `ORDER BY` can use grouped columns, aggregate expressions, or output aliases. `DISTINCT` and `ORDER BY` processing follows the grouped projection. An index on the grouping columns can supply rows already ordered, but you still write `ORDER BY` if the client depends on order.

**Examples:**

```sql
SELECT region, sum(amount) AS total_amount
FROM sales
WHERE amount > 0
GROUP BY region
HAVING sum(amount) > 1000
ORDER BY total_amount DESC;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-table-expressions.html
- https://www.postgresql.org/docs/current/queries-order.html
</details>

<details>
<summary>How would you find duplicate records in a table?</summary>

### How would you find duplicate records in a table?

**Definition:**

Duplicate detection groups the columns that should be unique and keeps groups whose row count is greater than one.

**How it works:**

`GROUP BY` those columns and `HAVING count(*) > 1` returns the duplicated keys and how often they appear. To list every duplicated row, join that set back to the table, or use `count(*) OVER (PARTITION BY ...)` and keep rows where the window count is greater than one.

**Examples:**

```sql
SELECT email, count(*) AS copies
FROM users
GROUP BY email
HAVING count(*) > 1;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-table-expressions.html
- https://www.postgresql.org/docs/current/tutorial-window.html
</details>

### Database Design & Architecture

<details>
<summary>What is the Entity-Relationship Model?</summary>

### What is the Entity-Relationship Model?

**Definition:**

The entity-relationship model describes data as entities, attributes of those entities, and relationships between them. In a relational database those become tables, columns, and foreign keys.

**How it works:**

An entity is a table. An attribute is a column. A relationship is a foreign key, sometimes through a junction table when many rows on each side relate to many on the other. Cardinality is enforced with `PRIMARY KEY`, `UNIQUE`, `NOT NULL`, and `REFERENCES`. The model is a design picture; PostgreSQL stores the resulting tables, not the diagram.

**Examples:**

```sql
CREATE TABLE department (
    dept_id integer PRIMARY KEY,
    dept_name text NOT NULL
);

CREATE TABLE employee (
    emp_id integer PRIMARY KEY,
    emp_name text NOT NULL,
    dept_id integer REFERENCES department (dept_id)
);
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl.html
- https://www.postgresql.org/docs/current/tutorial-fk.html
</details>

<details>
<summary>Explain the different types of database schema.</summary>

### Explain the different types of database schema.

**Definition:**

In design vocabulary, a conceptual schema names entities and relationships, a logical schema names tables and keys, and a physical schema names indexes, partitions, and storage. In PostgreSQL, a schema is also a namespace inside one database.

**How it works:**

`CREATE SCHEMA` creates a namespace. Unqualified names are resolved through `search_path`. Objects in different schemas can share a name. Privileges are granted separately: `USAGE` on the schema and the relevant privilege on the object. `public` is the default schema in a new database. Temporary objects live in a session-local schema.

**When to use:**

Use separate PostgreSQL schemas to group objects, isolate applications, or give each tenant a namespace when that isolation level is enough. The conceptual, logical, and physical layers are design stages, not three `CREATE` commands.

**Examples:**

```sql
CREATE SCHEMA sales;
CREATE TABLE sales.orders (
    order_id bigint PRIMARY KEY,
    amount numeric(12, 2) NOT NULL
);
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-schemas.html
</details>

<details>
<summary>What are Stored Procedures and how are they beneficial?</summary>

### What are Stored Procedures and how are they beneficial?

**Definition:**

A procedure is a routine invoked with `CALL`. `CREATE PROCEDURE` stores it in the database. Unlike a function, a procedure does not return a table result as its primary result, and it may execute transaction-control commands when the language and the procedure settings allow it.

**How it works:**

Arguments are `IN`, `OUT`, `INOUT`, or `VARIADIC`. The default mode is `IN`. The body can be `LANGUAGE sql` or `LANGUAGE plpgsql`. `SECURITY INVOKER` (the default) runs as the caller. `SECURITY DEFINER` runs as the owner, but a security-definer procedure cannot run `COMMIT` or `ROLLBACK`. `CREATE OR REPLACE PROCEDURE` keeps ownership and permissions.

**When to use:**

Use a procedure for a multi-statement operation you want to call by name, especially when it must commit intermediate work. Use a function when the routine must appear inside a `SELECT` list or `WHERE` clause.

**Examples:**

```sql
CREATE PROCEDURE insert_data(a integer, b integer)
LANGUAGE SQL
BEGIN ATOMIC
  INSERT INTO tbl VALUES (a);
  INSERT INTO tbl VALUES (b);
END;

CALL insert_data(1, 2);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createprocedure.html
- https://www.postgresql.org/docs/current/sql-call.html
</details>

<details>
<summary>What is a trigger in SQL and when should it be used?</summary>

### What is a trigger in SQL and when should it be used?

**Definition:**

A trigger runs a function automatically when `INSERT`, `UPDATE`, `DELETE`, or `TRUNCATE` happens on a table or view.

**How it works:**

Timing is `BEFORE`, `AFTER`, or `INSTEAD OF`. Granularity is `FOR EACH ROW` or `FOR EACH STATEMENT`. A row-level trigger function can read `NEW` and `OLD`. The command is `EXECUTE { FUNCTION | PROCEDURE } function_name()`. A `WHEN` condition can skip the call. `BEFORE` triggers can change `NEW` or return null to skip the row operation.

**When to use:**

Use a trigger for a rule that must hold no matter which client writes the row, such as an audit row or a derived column that a `CHECK` constraint cannot compute. Prefer a declared constraint when it expresses the rule directly. Triggers are easy to miss when reading application code.

**Examples:**

```sql
CREATE TRIGGER emp_stamp
BEFORE UPDATE ON emp
FOR EACH ROW
EXECUTE FUNCTION emp_stamp();
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createtrigger.html
- https://www.postgresql.org/docs/current/trigger-definition.html
</details>

<details>
<summary>Describe the concept of ACID in databases.</summary>

### Describe the concept of ACID in databases.

**Definition:**

ACID names four properties of a transaction: atomicity, consistency, isolation, and durability.

**How it works:**

Atomicity means `COMMIT` applies every change in the transaction and `ROLLBACK` applies none. Consistency means declared constraints are satisfied at commit. Isolation means concurrent transactions do not see one another's unfinished work; PostgreSQL implements this with multiversion concurrency control and snapshot isolation. Durability means a committed transaction survives a crash, which PostgreSQL provides by writing the change to the write-ahead log before reporting commit. Sequence changes are an exception: `nextval` is not rolled back.

**Examples:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00 WHERE acctnum = 7534;
UPDATE accounts SET balance = balance + 100.00 WHERE acctnum = 12345;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-transactions.html
- https://www.postgresql.org/docs/current/mvcc.html
- https://www.postgresql.org/docs/current/wal-intro.html
</details>

<details>
<summary>What is database sharding?</summary>

### What is database sharding?

**Definition:**

Sharding splits one logical data set across multiple databases so each shard holds a subset of the rows. PostgreSQL does not include an automatic sharding coordinator.

**How it works:**

Declarative partitioning splits a table inside one database by `RANGE`, `LIST`, or `HASH`, and the planner can prune partitions. A foreign partition, or `postgres_fdw`, can place part of the data on another PostgreSQL server. Logical replication copies changes between servers. Cross-shard transactions, global unique keys, and scatter queries are application or extension concerns; core SQL does not hide them.

**When to use:**

Partition inside one cluster when the goal is manageability or pruning. Move to multiple servers when one machine's storage or write rate is the limit, and accept that the application must choose the shard key and route queries.

**Examples:**

```sql
CREATE TABLE measurement (
    city_id int NOT NULL,
    logdate date NOT NULL,
    unitsales int
) PARTITION BY RANGE (logdate);
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-partitioning.html
- https://www.postgresql.org/docs/current/postgres-fdw.html
- https://www.postgresql.org/docs/current/logical-replication.html
</details>

<details>
<summary>How do database indexes work and what types are there?</summary>

### How do database indexes work and what types are there?

**Definition:**

An index is a separate structure the planner can use to find rows without reading the whole table. `CREATE INDEX` builds a B-tree unless you write `USING`.

**How it works:**

Built-in types are B-tree, hash, GiST, SP-GiST, GIN, and BRIN. The `bloom` type comes from an extension. A B-tree supports `<`, `<=`, `=`, `>=`, `>`, `BETWEEN`, `IN`, `IS NULL`, `IS NOT NULL`, and some prefix `LIKE` patterns, and it can return rows in sorted order. Hash supports `=` only. GiST and SP-GiST support operator classes such as geometric search and nearest-neighbor `ORDER BY`. GIN is an inverted index for composite values such as arrays and `jsonb`. BRIN stores summaries of physical block ranges and helps when column values correlate with table order.

**Examples:**

```sql
CREATE INDEX films_kind_idx ON films (kind);
CREATE INDEX name ON places USING HASH (code);
```

**Sources:**

- https://www.postgresql.org/docs/current/indexes-types.html
- https://www.postgresql.org/docs/current/sql-createindex.html
</details>

<details>
<summary>Describe the process of data warehousing.</summary>

### Describe the process of data warehousing.

**Definition:**

A data warehouse stores integrated historical data for analysis. The load path is extract, transform, and load. PostgreSQL can hold that store; it is not a separate warehouse product.

**How it works:**

Extract with `COPY` or a foreign table. Transform with SQL: type casts, `COALESCE`, joins, and aggregates. Load into fact and dimension tables, often partitioned by time. A materialized view can store a summary so repeated reports do not rescan the detail. `REFRESH MATERIALIZED VIEW` rebuilds it. Analysis queries then aggregate the loaded tables.

**Examples:**

```sql
CREATE MATERIALIZED VIEW sales_by_region AS
SELECT region, sum(amount) AS total_amount
FROM sales
GROUP BY region;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-copy.html
- https://www.postgresql.org/docs/current/rules-materializedviews.html
- https://www.postgresql.org/docs/current/ddl-partitioning.html
</details>

<details>
<summary>Explain the difference between OLTP and OLAP systems.</summary>

### Explain the difference between OLTP and OLAP systems.

**Definition:**

OLTP systems handle many short transactions that read and write a few rows. OLAP systems scan and aggregate large historical sets for reports.

**How it works:**

PostgreSQL serves both shapes in one engine. OLTP relies on indexes, primary keys, short transactions, and MVCC so readers do not block writers. OLAP relies on sequential scans, hash or group aggregates, partitioning so the planner can skip old ranges, and materialized views for repeated summaries. A wide `SELECT *` with no predicate and a point lookup want different plans.

**When to use:**

Keep the transactional workload on normalized tables with indexes that match the predicates. Build reporting summaries, partitions, or a separate database fed by logical replication when analytical scans would compete with those short transactions.

**Examples:**

```sql
SELECT region, sum(amount)
FROM sales
GROUP BY region;
```

**Sources:**

- https://www.postgresql.org/docs/current/mvcc.html
- https://www.postgresql.org/docs/current/using-explain.html
- https://www.postgresql.org/docs/current/ddl-partitioning.html
</details>

<details>
<summary>What are materialized views and how do they differ from standard views?</summary>

### What are materialized views and how do they differ from standard views?

**Definition:**

A standard view stores a query and runs it on each reference. A materialized view stores the query's result on disk.

**How it works:**

`CREATE MATERIALIZED VIEW name AS query` fills the storage at creation. Later changes to the base tables do not appear until `REFRESH MATERIALIZED VIEW`. `REFRESH ... CONCURRENTLY` does not lock out readers for the whole refresh, and it requires a unique index on the materialized view. You can index a materialized view like a table. `WITH NO DATA` creates it empty.

**When to use:**

Use a normal view when the result must be current and the query is cheap. Use a materialized view when the summary is expensive and readers can accept data as of the last refresh.

**Examples:**

```sql
CREATE MATERIALIZED VIEW wrd AS SELECT * FROM words;
CREATE UNIQUE INDEX wrd_word ON wrd (word);
REFRESH MATERIALIZED VIEW CONCURRENTLY wrd;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-creatematerializedview.html
- https://www.postgresql.org/docs/current/rules-materializedviews.html
</details>

### SQL Optimization and Performance

<details>
<summary>How do you identify and optimize slow-running queries?</summary>

### How do you identify and optimize slow-running queries?

**Definition:**

A slow query is one whose runtime or I/O stands out. PostgreSQL records that in the log and, with an extension, in `pg_stat_statements`.

**How it works:**

`log_min_duration_statement` logs statements that run longer than a threshold. `pg_stat_statements` aggregates total time, calls, and rows by query text. `EXPLAIN (ANALYZE, BUFFERS)` runs the statement and shows estimated versus actual rows and buffer hits. Typical fixes are a missing index for a selective predicate, fresh statistics from `ANALYZE`, a rewrite that makes the predicate sargable, or a smaller working set.

**Examples:**

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT * FROM films WHERE kind = 'Comedy';
```

**Sources:**

- https://www.postgresql.org/docs/current/using-explain.html
- https://www.postgresql.org/docs/current/pgstatstatements.html
- https://www.postgresql.org/docs/current/runtime-config-logging.html
</details>

<details>
<summary>What is query execution plan in SQL?</summary>

### What is query execution plan in SQL?

**Definition:**

The execution plan is the tree of steps the planner chooses to run a statement: scans, joins, sorts, aggregates, and how they pass rows.

**How it works:**

The planner costs alternatives using table statistics. A node such as `Seq Scan` reads the heap. `Index Scan` or `Index Only Scan` uses an index. Join nodes include nested loop, hash join, and merge join. The first cost number pair is startup cost and total cost in arbitrary units. `EXPLAIN` prints the plan without executing it. Row estimates that diverge from reality usually mean missing or stale statistics, or a predicate the planner cannot estimate.

**Examples:**

```sql
EXPLAIN SELECT * FROM films WHERE kind = 'Comedy';
```

**Sources:**

- https://www.postgresql.org/docs/current/using-explain.html
- https://www.postgresql.org/docs/current/sql-explain.html
</details>

<details>
<summary>Explain how to use EXPLAIN or EXPLAIN ANALYZE.</summary>

### Explain how to use EXPLAIN or EXPLAIN ANALYZE.

**Definition:**

`EXPLAIN` shows the plan the planner would use. `EXPLAIN ANALYZE` executes the statement and adds actual row counts and time.

**How it works:**

Useful options are `ANALYZE`, `BUFFERS`, `VERBOSE`, `SETTINGS`, and `FORMAT` (`TEXT`, `XML`, `JSON`, or `YAML`). `ANALYZE` really runs the statement, so `EXPLAIN ANALYZE INSERT`, `UPDATE`, `DELETE`, or `MERGE` modifies data. Wrap it in `BEGIN` and `ROLLBACK` when you need the measurements without keeping the change. `BUFFERS` shows shared blocks hit and read.

**When to use:**

Use plain `EXPLAIN` to see the shape of a plan without paying the execution, especially for a statement you must not run. Use `EXPLAIN (ANALYZE, BUFFERS)` when estimated rows and actual rows need to be compared.

**Examples:**

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT * FROM tenk1 WHERE unique1 < 100;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-explain.html
- https://www.postgresql.org/docs/current/using-explain.html
</details>

<details>
<summary>How can indexing affect performance both positively and negatively?</summary>

### How can indexing affect performance both positively and negatively?

**Definition:**

An index can turn a selective lookup or an ordered scan into fewer page reads. It also costs storage and extra work on every write that changes an indexed column.

**How it works:**

The planner uses an index when its cost estimate beats a sequential scan. A B-tree helps equality, range, and some ordering. A low-selectivity predicate, such as a boolean that is true for most rows, often stays a sequential scan. Each extra index must be updated on `INSERT`, `UPDATE`, and `DELETE`. `VACUUM` and bloat apply to indexes as well as tables. A partial index indexes only rows that match a `WHERE` clause, so it stays smaller when queries always use that predicate.

**When to use:**

Add an index that matches a frequent, selective predicate or a required order. Drop indexes that no query uses. Check the plan with `EXPLAIN` rather than assuming every foreign key already has one.

**Examples:**

```sql
CREATE INDEX films_unreleased_idx
    ON films (title)
    WHERE did IS NULL;
```

**Sources:**

- https://www.postgresql.org/docs/current/indexes.html
- https://www.postgresql.org/docs/current/indexes-partial.html
- https://www.postgresql.org/docs/current/indexes-types.html
</details>

<details>
<summary>Describe how to measure the performance of SQL queries.</summary>

### Describe how to measure the performance of SQL queries.

**Definition:**

Measure a query by its actual runtime, rows processed, and buffer I/O, not by the planner's cost number alone.

**How it works:**

`EXPLAIN (ANALYZE, BUFFERS)` reports planning time, execution time, actual rows, and shared buffer hits versus reads. `track_io_timing` must be on before those timings include block read time. `pg_stat_statements` sums time across many executions of the same statement. The log line written by `log_min_duration_statement` records duration for one execution. Run `ANALYZE` so the planner's estimates match the table before you compare plans.

**Examples:**

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT sum(amount) FROM sales WHERE region = 'EMEA';
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-explain.html
- https://www.postgresql.org/docs/current/pgstatstatements.html
- https://www.postgresql.org/docs/current/runtime-config-statistics.html
</details>

<details>
<summary>How would you rewrite a query to improve its performance?</summary>

### How would you rewrite a query to improve its performance?

**Definition:**

A faster query usually gives the planner a sargable predicate, less work per row, and a join or aggregate shape that matches the indexes and statistics.

**How it works:**

Compare plans before and after with `EXPLAIN (ANALYZE, BUFFERS)`. Putting a column inside a function, as in `WHERE lower(name) = 'ann'`, blocks a plain B-tree on `name`; an expression index on `lower(name)` or a different predicate fixes that. Replace `NOT IN (subquery)` that may contain nulls. Select only needed columns so an index-only scan can apply. A large `OFFSET` can become a keyset predicate. A correlated subquery that repeats per outer row can become a join or a window.

**Examples:**

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT id, title
FROM films
WHERE kind = 'Comedy';
```

**Sources:**

- https://www.postgresql.org/docs/current/using-explain.html
- https://www.postgresql.org/docs/current/indexes-expressional.html
</details>

<details>
<summary>What are partitioned tables and how can they optimize performance?</summary>

### What are partitioned tables and how can they optimize performance?

**Definition:**

A partitioned table is a parent whose rows are stored in child partitions. PostgreSQL declarative partitioning supports `RANGE`, `LIST`, and `HASH`.

**How it works:**

The parent defines `PARTITION BY`. Each partition is `CREATE TABLE ... PARTITION OF parent FOR VALUES ...`. If the planner can prove a partition cannot match the query, partition pruning skips it. Inserts are routed to the matching partition. Updating a partition key can move the row, which is implemented as delete plus insert. Indexes are created on partitions; a unique constraint that should apply to the whole table must include the partition key.

**When to use:**

Partition large tables on a column that queries restrict, often a date, so scans and maintenance such as dropping an old partition stay local. Do not partition a small table into many tiny children; planning cost can exceed the scan you save.

**Examples:**

```sql
CREATE TABLE measurement (
    city_id int NOT NULL,
    logdate date NOT NULL,
    unitsales int
) PARTITION BY RANGE (logdate);

CREATE TABLE measurement_y2026m01 PARTITION OF measurement
    FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-partitioning.html
</details>

### SQL Security

<details>
<summary>How do you implement database encryption in SQL?</summary>

### How do you implement database encryption in SQL?

**Definition:**

PostgreSQL can encrypt client connections with SSL, store passwords with SCRAM, and encrypt individual values with the `pgcrypto` extension. Core SQL has no transparent full-disk encryption command.

**How it works:**

Connection encryption is server configuration (`ssl = on`) plus host rules in `pg_hba.conf`, not a column clause. `pgp_sym_encrypt(data, password)` returns `bytea`. `pgp_sym_decrypt` reverses it when the password matches. The password should not sit in the query text that lands in logs. Disk and backup encryption are operating-system or backup-tool features; `pg_dump` output is plaintext unless you encrypt the file yourself.

**When to use:**

Use SSL for data on the wire. Use `pgcrypto` when specific column values must be unreadable without an application-held key. Use filesystem or volume encryption when the threat is stolen disks.

**Examples:**

```sql
CREATE EXTENSION pgcrypto;

SELECT pgp_sym_encrypt('sensitive', 'key');
```

**Sources:**

- https://www.postgresql.org/docs/current/pgcrypto.html
- https://www.postgresql.org/docs/current/ssl-tcp.html
- https://www.postgresql.org/docs/current/auth-password.html
</details>

<details>
<summary>What are roles and how do they manage database access?</summary>

### What are roles and how do they manage database access?

**Definition:**

A role is a database user or a group of users. `CREATE ROLE` defines it. `GRANT` and `REVOKE` give it privileges. There is no separate user object: a role with `LOGIN` can connect.

**How it works:**

Privileges include `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `REFERENCES`, `TRIGGER`, and `USAGE` on schemas, plus column-level grants. `GRANT role_a TO role_b` lets `role_b` inherit `role_a`'s privileges when `INHERIT` is set. The table owner bypasses ordinary privilege checks. `PUBLIC` is the implicit group of all roles.

**Examples:**

```sql
CREATE ROLE analyst LOGIN PASSWORD NULL;
GRANT USAGE ON SCHEMA sales TO analyst;
GRANT SELECT ON sales.orders TO analyst;
```

**Sources:**

- https://www.postgresql.org/docs/current/user-manag.html
- https://www.postgresql.org/docs/current/sql-createrole.html
- https://www.postgresql.org/docs/current/sql-grant.html
</details>

<details>
<summary>Explain the concept of row-level security.</summary>

### Explain the concept of row-level security.

**Definition:**

Row-level security adds policies that filter which rows a role can see or write. It applies only after table privileges allow the command.

**How it works:**

`ALTER TABLE ... ENABLE ROW LEVEL SECURITY` turns it on. `CREATE POLICY name ON table FOR command TO role USING (expression) WITH CHECK (expression)` defines the filter. `USING` restricts existing rows for `SELECT`, `UPDATE`, and `DELETE`. `WITH CHECK` restricts rows produced by `INSERT` and `UPDATE`. The table owner bypasses row security unless you `FORCE ROW LEVEL SECURITY`. Superusers and roles with `BYPASSRLS` also bypass it. If no policy applies, the default is to deny.

**Examples:**

```sql
ALTER TABLE passwd ENABLE ROW LEVEL SECURITY;

CREATE POLICY user_mod ON passwd
FOR UPDATE
USING (current_user = user_name)
WITH CHECK (current_user = user_name);
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-rowsecurity.html
- https://www.postgresql.org/docs/current/sql-createpolicy.html
</details>

<details>
<summary>Describe how to create and use user-defined functions (UDFs).</summary>

### Describe how to create and use user-defined functions (UDFs).

**Definition:**

A user-defined function is created with `CREATE FUNCTION` and can be called from SQL expressions. The language can be `sql`, `plpgsql`, or another installed language.

**How it works:**

You declare argument types and a `RETURNS` type. `LANGUAGE SQL` functions can use a string body or `BEGIN ATOMIC ... END`. The function is invoked as `name(arguments)` in a select list, `WHERE`, or other expression. `SECURITY INVOKER` is the default. `SECURITY DEFINER` runs with the owner's privileges; set a safe `search_path` on that function so an unqualified name cannot be captured. Callers need `EXECUTE` privilege.

**Examples:**

```sql
CREATE FUNCTION add(integer, integer) RETURNS integer
AS $$ SELECT $1 + $2; $$
LANGUAGE SQL
IMMUTABLE;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createfunction.html
- https://www.postgresql.org/docs/current/xfunc-sql.html
</details>

### SQL Functions and Expressions

<details>
<summary>Describe scalar-valued and table-valued functions.</summary>

### Describe scalar-valued and table-valued functions.

**Definition:**

A scalar function returns one value per call. A set-returning function returns a set of rows. PostgreSQL spells the second form `RETURNS SETOF` or `RETURNS TABLE`.

**How it works:**

`RETURNS integer` is a scalar. `RETURNS TABLE (f1 int, f2 text)` is a set of composite rows and can be used in `FROM`. `RETURNS SETOF some_type` is the same idea for an existing type. A scalar is called in an expression. A set-returning function in the select list is allowed but is clearer in `FROM`, including `LATERAL` when it depends on the current outer row.

**When to use:**

Return a scalar when the caller needs one value, such as a computed tax. Return a table when the caller needs a relation it can join.

**Examples:**

```sql
CREATE FUNCTION getfoo() RETURNS TABLE (fooid int, fooname text)
LANGUAGE SQL
AS $$
    SELECT fooid, fooname FROM foo;
$$;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createfunction.html
- https://www.postgresql.org/docs/current/queries-table-expressions.html
</details>

<details>
<summary>How would you define a stored procedure with input and output parameters?</summary>

### How would you define a stored procedure with input and output parameters?

**Definition:**

Procedure arguments take a mode: `IN`, `OUT`, `INOUT`, or `VARIADIC`. Omitted mode means `IN`. Output arguments are assigned inside the procedure and returned to the `CALL`.

**How it works:**

`IN` passes a value in. `OUT` is written by the procedure. `INOUT` does both. `CALL` supplies a placeholder for each `OUT` argument. Procedures are not used as expressions in `SELECT`. A `LANGUAGE plpgsql` body assigns the output name like a variable.

**Examples:**

```sql
CREATE PROCEDURE sales_tax(IN subtotal numeric, OUT tax numeric)
LANGUAGE plpgsql
AS $$
BEGIN
    tax := subtotal * 0.06;
END;
$$;

CALL sales_tax(100.00, NULL);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createprocedure.html
- https://www.postgresql.org/docs/current/sql-call.html
</details>

<details>
<summary>What is the difference between a function and a stored procedure?</summary>

### What is the difference between a function and a stored procedure?

**Definition:**

A function returns a value and is called inside an expression. A procedure is invoked with `CALL` and does not return a result set as its return type.

**How it works:**

Functions can appear in `SELECT`, `WHERE`, and defaults. Procedures cannot. A procedure may `COMMIT` or `ROLLBACK` inside its body when it is not `SECURITY DEFINER`, has no `SET` clause that would forbid transaction control, and `CALL` itself is not already inside a transaction block. A function cannot commit the surrounding transaction. Both support `IN`, `OUT`, and `INOUT` arguments and `SECURITY INVOKER` or `SECURITY DEFINER`.

**When to use:**

Choose a function for a computation used in queries. Choose a procedure for a command that performs several data changes and may need its own transaction control.

**Examples:**

```sql
SELECT add(2, 3);

CALL insert_data(1, 2);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createfunction.html
- https://www.postgresql.org/docs/current/sql-createprocedure.html
</details>

<details>
<summary>How do you use the CAST and CONVERT functions?</summary>

### How do you use the CAST and CONVERT functions?

**Definition:**

`CAST(expression AS type)` converts a value to another data type. The PostgreSQL spelling `expression::type` does the same conversion. `convert` does not cast types: it converts `bytea` from one character encoding to another.

**How it works:**

`CAST('100' AS integer)` and `'100'::integer` both yield the integer 100. An invalid representation raises an error. Assignment casts and explicit casts follow different rules; an over-long string errors on assignment to `varchar(n)` but is truncated by an explicit cast. `convert(bytes, src_encoding, dest_encoding)` returns `bytea`. `convert_from` and `convert_to` convert between `bytea` and `text` in a named encoding.

**Examples:**

```sql
SELECT CAST('100' AS integer), '100'::integer;

SELECT convert_from('\x4869'::bytea, 'UTF8');
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-expressions.html
- https://www.postgresql.org/docs/current/functions-string.html
- https://www.postgresql.org/docs/current/typeconv.html
</details>

### Transaction Control and Locking

<details>
<summary>What is a database transaction?</summary>

### What is a database transaction?

**Definition:**

A transaction is a group of SQL commands that commit or roll back as one unit. PostgreSQL runs each statement in its own transaction unless you open one explicitly.

**How it works:**

`BEGIN` or `START TRANSACTION` opens a transaction block. `COMMIT` makes its changes durable and visible. `ROLLBACK` discards them. A statement error aborts the current transaction; further commands fail until `ROLLBACK` or `ROLLBACK TO` a savepoint. Closing the session without commit rolls the transaction back.

**Examples:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00 WHERE acctnum = 7534;
UPDATE accounts SET balance = balance + 100.00 WHERE acctnum = 12345;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-transactions.html
- https://www.postgresql.org/docs/current/sql-begin.html
</details>

<details>
<summary>Explain the concept of locking and its types in SQL databases.</summary>

### Explain the concept of locking and its types in SQL databases.

**Definition:**

Locks coordinate concurrent access. PostgreSQL takes them automatically and also exposes explicit row and table locks.

**How it works:**

MVCC lets a plain `SELECT` read a snapshot without blocking writers. `SELECT ... FOR UPDATE` takes a row lock that conflicts with updates and deletes. `FOR NO KEY UPDATE` is weaker when the update does not touch key columns. `FOR SHARE` and `FOR KEY SHARE` are shared locks. Table-level modes run from `ACCESS SHARE`, taken by `SELECT`, up to `ACCESS EXCLUSIVE`, taken by `DROP TABLE`, `TRUNCATE`, and some forms of `ALTER TABLE` and `VACUUM FULL`. `LOCK TABLE` requests a table lock explicitly. Locks are held until the transaction ends.

**Examples:**

```sql
BEGIN;
SELECT balance
FROM accounts
WHERE acctnum = 7534
FOR UPDATE;
UPDATE accounts SET balance = balance - 100.00 WHERE acctnum = 7534;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/explicit-locking.html
- https://www.postgresql.org/docs/current/mvcc.html
</details>

<details>
<summary>What are the properties of transactions?</summary>

### What are the properties of transactions?

**Definition:**

The properties are atomicity, consistency, isolation, and durability.

**How it works:**

Atomicity is `COMMIT` versus `ROLLBACK`, including savepoints. Consistency is the constraint set checked for the transaction: `NOT NULL`, `CHECK`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, and exclusion constraints. Isolation is the snapshot and lock behavior of the chosen isolation level; the default is Read Committed. Durability is the write-ahead log flush at commit. `nextval` on a sequence is not undone by rollback.

**Examples:**

```sql
BEGIN;
INSERT INTO orders (order_id, amount) VALUES (1, 20.00);
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-transactions.html
- https://www.postgresql.org/docs/current/ddl-constraints.html
- https://www.postgresql.org/docs/current/wal-intro.html
</details>

<details>
<summary>How do you manage transaction isolation levels?</summary>

### How do you manage transaction isolation levels?

**Definition:**

The isolation level controls which concurrent changes a transaction can observe. PostgreSQL implements Read Committed, Repeatable Read, and Serializable. Requesting Read Uncommitted behaves as Read Committed.

**How it works:**

Read Committed is the default. Each statement sees rows committed before that statement started. Repeatable Read keeps one snapshot for the whole transaction, and PostgreSQL also prevents phantom reads at that level. Serializable adds detection of serialization anomalies and may abort a transaction with SQLSTATE `40001`. Set the level with `SET TRANSACTION ISOLATION LEVEL` after `BEGIN`, or `SET SESSION CHARACTERISTICS` for later transactions. `SERIALIZABLE` applications must be ready to retry.

**When to use:**

Stay on Read Committed for short statements that each touch known rows. Use Repeatable Read when every statement in the transaction must see the same snapshot. Use Serializable when the result must match some one-at-a-time order of the concurrent transactions.

**Examples:**

```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SELECT sum(balance) FROM accounts;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/transaction-iso.html
- https://www.postgresql.org/docs/current/sql-set-transaction.html
</details>

<details>
<summary>What does it mean to commit or roll back a transaction?</summary>

### What does it mean to commit or roll back a transaction?

**Definition:**

`COMMIT` ends the transaction and makes its changes durable and visible to other transactions. `ROLLBACK` ends it and discards those changes.

**How it works:**

After `COMMIT`, the server has flushed the commit record to the write-ahead log according to `synchronous_commit`. After `ROLLBACK`, it is as if the changes were never made, except for sequence advances. `SAVEPOINT name` marks a point inside the transaction. `ROLLBACK TO SAVEPOINT name` undoes only the work after that point, and the transaction stays open. `RELEASE SAVEPOINT` drops the marker without undoing work.

**Examples:**

```sql
BEGIN;
SAVEPOINT sp1;
INSERT INTO wines VALUES ('Chateau Lafite 2003', 24);
ROLLBACK TO SAVEPOINT sp1;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-transactions.html
- https://www.postgresql.org/docs/current/sql-commit.html
- https://www.postgresql.org/docs/current/sql-rollback.html
</details>

### SQL and Modern Data Ecosystems

<details>
<summary>How can SQL be integrated with big data technologies?</summary>

### How can SQL be integrated with big data technologies?

**Definition:**

PostgreSQL integrates with outside data through foreign data wrappers and file import, while SQL stays the query language on the PostgreSQL side.

**How it works:**

`postgres_fdw` queries another PostgreSQL server as a foreign table. `file_fdw` reads a server-side file, such as CSV, as a foreign table. `COPY` loads or unloads a file in one command. The wrapper pushes some filters to the remote side when it can. A foreign scan is still not a distributed query engine: joins of huge remote sets may pull rows to the local server.

**When to use:**

Use a foreign table when the remote data should look like a table in SQL. Use `COPY` when you are loading a batch once. Keep heavy repeated analytics in local partitioned tables or a materialized view after the load.

**Examples:**

```sql
CREATE EXTENSION file_fdw;
CREATE SERVER local_file FOREIGN DATA WRAPPER file_fdw;
CREATE FOREIGN TABLE words (word text NOT NULL)
    SERVER local_file
    OPTIONS (filename '/usr/share/dict/words');
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-foreign-data.html
- https://www.postgresql.org/docs/current/postgres-fdw.html
- https://www.postgresql.org/docs/current/file-fdw.html
</details>

<details>
<summary>Discuss the interoperability of SQL with cloud-based data stores.</summary>

### Discuss the interoperability of SQL with cloud-based data stores.

**Definition:**

PostgreSQL talks to another PostgreSQL instance, including one hosted by a cloud provider, with foreign tables and logical replication. It does not embed each vendor's SQL dialect.

**How it works:**

`postgres_fdw` maps a remote table into the local database. The local role needs a user mapping for the remote connection. Logical replication publishes changes from one server and subscribes on another, which is a common path onto a cloud replica or off one. `COPY` moves files through client or server streams. Queries you send still use PostgreSQL syntax.

**When to use:**

Use `postgres_fdw` for occasional SQL against a remote PostgreSQL database. Use logical replication when the remote side should keep its own copy. Do not assume a foreign table gives the same latency as a local table.

**Examples:**

```sql
CREATE EXTENSION postgres_fdw;
CREATE SERVER cloud_pg FOREIGN DATA WRAPPER postgres_fdw
    OPTIONS (host 'db.example.com', dbname 'app', port '5432');
```

**Sources:**

- https://www.postgresql.org/docs/current/postgres-fdw.html
- https://www.postgresql.org/docs/current/logical-replication.html
</details>

<details>
<summary>What is Data Lake and how can SQL interact with it?</summary>

### What is Data Lake and how can SQL interact with it?

**Definition:**

A data lake is a store of files, often CSV, JSON, or Parquet, kept outside a table's heap. PostgreSQL reads those files with `COPY` or a foreign-data wrapper; it does not manage the lake.

**How it works:**

`COPY table FROM 'filename' WITH (FORMAT csv, HEADER true)` loads a server-side file into a table. The PostgreSQL operating-system user must be able to read that path, and the database role needs the privileges `COPY` requires. `file_fdw` leaves the file in place and scans it as a foreign table. After the load, ordinary SQL runs on the local table.

**When to use:**

Query a foreign file when you need a one-off scan. Load into a real table, with types and indexes, when the same data will be queried often.

**Examples:**

```sql
COPY orders (order_id, amount, region)
FROM '/var/import/orders.csv'
WITH (FORMAT csv, HEADER true);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-copy.html
- https://www.postgresql.org/docs/current/file-fdw.html
</details>

<details>
<summary>Explain the interaction between SQL and NoSQL within the same application.</summary>

### Explain the interaction between SQL and NoSQL within the same application.

**Definition:**

PostgreSQL stores relational rows and document-shaped JSON in the same database. `jsonb` is the binary JSON type meant for querying.

**How it works:**

A `jsonb` column holds an object or array. `->` returns a JSON value, `->>` returns text, and `@>` tests containment. Expression indexes and GIN indexes support `jsonb` operators. Constraints and foreign keys still apply to the relational columns beside the document. JSON does not replace a foreign key: a nested id is not checked unless you model it as a column and a constraint.

**When to use:**

Use columns and keys for data you filter, join, and constrain. Use `jsonb` for a payload whose shape varies and is not the join key.

**Examples:**

```sql
SELECT doc->>'sku' AS sku
FROM products
WHERE doc @> '{"active": true}';
```

**Sources:**

- https://www.postgresql.org/docs/current/datatype-json.html
- https://www.postgresql.org/docs/current/functions-json.html
</details>

<details>
<summary>How does SQL work within a microservices architecture?</summary>

### How does SQL work within a microservices architecture?

**Definition:**

Each service usually owns its data. PostgreSQL can enforce that with separate databases, schemas, and roles. SQL then runs inside one service's boundary.

**How it works:**

A role granted only on its schema cannot read another service's tables. Cross-service reads are not foreign keys across services; if you do query another database, `postgres_fdw` makes that dependency explicit. Logical replication can feed a reporting database without letting services write one another's tables. Transactions do not span services: a PostgreSQL transaction covers one database connection.

**When to use:**

Give the service its own role and schema or database, and keep joins inside that boundary. Add a foreign table only when a synchronous remote read is an accepted dependency.

**Examples:**

```sql
CREATE SCHEMA billing;
CREATE ROLE billing_svc LOGIN;
GRANT USAGE ON SCHEMA billing TO billing_svc;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA billing TO billing_svc;
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-schemas.html
- https://www.postgresql.org/docs/current/sql-grant.html
- https://www.postgresql.org/docs/current/postgres-fdw.html
</details>

### SQL Best Practices and Standards

<details>
<summary>What are some common SQL coding practices you follow?</summary>

### What are some common SQL coding practices you follow?

**Definition:**

Practical PostgreSQL style is explicit columns, constraints in the database, parameters instead of concatenated literals, and an `ORDER BY` whenever order matters.

**How it works:**

Name every target column in `INSERT` and every output column in `SELECT`. Declare `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, and `CHECK` so invalid rows fail in the server. Pass values as parameters; a literal built by string pasting is how SQL injection happens. Write `ORDER BY` because a query without it has no guaranteed order. Prefer `text` or `varchar` over blank-padded `char(n)`. Run `EXPLAIN` on a query before adding an index for it.

**Examples:**

```sql
INSERT INTO films (code, title)
VALUES ('UA502', 'Bananas');
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-insert.html
- https://www.postgresql.org/docs/current/ddl-constraints.html
- https://www.postgresql.org/docs/current/datatype-character.html
</details>

<details>
<summary>How can you ensure the portability of SQL scripts across different database systems?</summary>

### How can you ensure the portability of SQL scripts across different database systems?

**Definition:**

Portable SQL stays inside the constructs those systems share. PostgreSQL extensions then need a deliberate replacement on the other system.

**How it works:**

`CAST(x AS type)` is standard; `x::type` is PostgreSQL. `OFFSET ... FETCH FIRST n ROWS ONLY` is standard; `LIMIT` is widely implemented but not the original standard spelling. `RETURNING`, `DISTINCT ON`, `ILIKE`, and `FROM` on `UPDATE` are PostgreSQL extensions. `convert` in PostgreSQL is an encoding function, not a type cast. Identity columns use `GENERATED AS IDENTITY`. Quoting identifiers with double quotes and strings with single quotes matches the standard.

**When to use:**

Write standard syntax when the script must run on more than one product. Use a PostgreSQL extension when you are staying on PostgreSQL and the standard form cannot express the operation.

**Examples:**

```sql
SELECT title
FROM films
ORDER BY title
OFFSET 0 ROWS
FETCH FIRST 10 ROWS ONLY;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-expressions.html
- https://www.postgresql.org/docs/current/queries-limit.html
- https://www.postgresql.org/docs/current/features.html
</details>

<details>
<summary>What methods do you use for version controlling SQL scripts?</summary>

### What methods do you use for version controlling SQL scripts?

**Definition:**

Schema changes live as ordered SQL scripts in the application repository. The server does not provide a source-control history of those scripts.

**How it works:**

Each migration is a file of `CREATE`, `ALTER`, and `COMMENT` statements applied in order. `pg_dump --schema-only` writes the current schema as SQL, which is a snapshot you can compare, not a substitute for the migration history. `pg_dump` of data is a backup. Review the script the same way as application code, and apply it in a transaction when every statement in it can run inside one.

**Examples:**

```sql
ALTER TABLE films
    ADD COLUMN kind varchar(50);
```

**Sources:**

- https://www.postgresql.org/docs/current/app-pgdump.html
- https://www.postgresql.org/docs/current/sql-altertable.html
</details>

<details>
<summary>What are the benefits of using stored procedures instead of embedding SQL queries in code?</summary>

### What are the benefits of using stored procedures instead of embedding SQL queries in code?

**Definition:**

A procedure or function keeps a multi-statement operation in the database and exposes it as `CALL` or a function call. The application then needs `EXECUTE` on that routine rather than broad table rights.

**How it works:**

`GRANT EXECUTE ON PROCEDURE name(argument_types) TO role` allows the call. `SECURITY DEFINER` runs as the owner, so the caller need not have table privileges; that also means the owner must lock down `search_path`. The routine is one definition for every client. A procedure may control transactions. The cost is that the SQL is no longer visible next to the application code and is tied to PostgreSQL syntax.

**When to use:**

Use a routine when several clients must share one audited write path or when the caller should not have direct table privileges. Keep ordinary single-statement queries in the application, still as parameterized SQL.

**Examples:**

```sql
GRANT EXECUTE ON PROCEDURE insert_data(integer, integer) TO app_role;
CALL insert_data(1, 2);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-createprocedure.html
- https://www.postgresql.org/docs/current/sql-grant.html
</details>

<details>
<summary>How do you document SQL code effectively?</summary>

### How do you document SQL code effectively?

**Definition:**

`COMMENT ON` stores a comment on a database object. PostgreSQL keeps that text in the system catalogs and shows it in `\d+` descriptions.

**How it works:**

You can comment a table, column, function, procedure, view, or other object. `COMMENT ON COLUMN table.column IS 'text'` sets it. `COMMENT ON ... IS NULL` removes it. Only the owner, or a superuser, can set a comment. Comments describe intent that the type and the constraint do not already state. Migration files should contain the `COMMENT` statements so a restored database keeps them.

**Examples:**

```sql
COMMENT ON TABLE films IS 'Catalog of films available for rental.';
COMMENT ON COLUMN films.kind IS 'Genre label such as Comedy or Drama.';
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-comment.html
</details>

### Analytical SQL Questions

<details>
<summary>How would you find the Nth highest salary from a table?</summary>

### How would you find the Nth highest salary from a table?

**Definition:**

The Nth highest distinct salary is the row whose dense rank is N when salaries are ordered descending.

**How it works:**

`dense_rank() OVER (ORDER BY salary DESC)` assigns the same rank to ties and does not skip numbers. Filter `rk = N` in an outer query because window functions are not allowed in `WHERE`. `ORDER BY salary DESC OFFSET N - 1 LIMIT 1` returns a row, but ties make "Nth row" different from "Nth distinct salary".

**Examples:**

```sql
SELECT salary
FROM (
    SELECT salary,
           dense_rank() OVER (ORDER BY salary DESC) AS rk
    FROM employee
) ranked
WHERE rk = 3;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-window.html
- https://www.postgresql.org/docs/current/tutorial-window.html
</details>

<details>
<summary>How do you count the number of occurrences of a specific value in a column?</summary>

### How do you count the number of occurrences of a specific value in a column?

**Definition:**

`count(*) FILTER (WHERE column = value)` counts rows where the comparison is true. `count(*)` with a `WHERE` clause does the same for a single total.

**How it works:**

A plain `count(column)` counts non-null values and does not by itself restrict which value. The `FILTER` clause applies only to that aggregate, so one query can count several values. `sum(CASE WHEN column = value THEN 1 ELSE 0 END)` is the equivalent conditional aggregate.

**Examples:**

```sql
SELECT count(*) FILTER (WHERE status = 'open') AS open_count
FROM tickets;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-aggregate.html
</details>

<details>
<summary>How can you calculate running totals in SQL?</summary>

### How can you calculate running totals in SQL?

**Definition:**

A running total is a windowed `sum` ordered by the sequence column. Each output row keeps its own values and adds the sum so far.

**How it works:**

`sum(amount) OVER (ORDER BY ordered_at)` uses a frame from the start of the partition through the current row and its peers. Add `PARTITION BY account_id` to restart the total per account. Specify `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` when peer ties should not share one total. The window does not reduce the row count.

**Examples:**

```sql
SELECT ordered_at,
       amount,
       sum(amount) OVER (ORDER BY ordered_at) AS running_total
FROM payments;
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-window.html
- https://www.postgresql.org/docs/current/functions-window.html
</details>

<details>
<summary>Explain how to reverse the contents of a column without using a reverse function.</summary>

### Explain how to reverse the contents of a column without using a reverse function.

**Definition:**

PostgreSQL provides `reverse(text)`, which returns the characters in reverse order. The same result can be built with a recursive CTE and `substring` if the task forbids that function.

**How it works:**

The anchor selects the original string and an empty accumulator. Each step moves the first character onto the front of the accumulator and keeps the remainder with `substring(rest FROM 2)`. The step stops when the remainder is empty. The finished row is the one whose remainder is empty.

**Examples:**

```sql
WITH RECURSIVE rev(id, rest, acc) AS (
    SELECT id, code, ''::text
    FROM labels
  UNION ALL
    SELECT id,
           substring(rest FROM 2),
           substring(rest FROM 1 FOR 1) || acc
    FROM rev
    WHERE char_length(rest) > 0
)
SELECT id, acc AS reversed
FROM rev
WHERE rest = '';
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-with.html
- https://www.postgresql.org/docs/current/functions-string.html
</details>

<details>
<summary>What approach do you use for creating a calendar table, and what are its uses?</summary>

### What approach do you use for creating a calendar table, and what are its uses?

**Definition:**

A calendar table is one row per day, generated with `generate_series` and stored so reports can outer-join it and keep days that have no facts.

**How it works:**

`generate_series` can return a set of integers. Adding each integer to a start `date` uses the `date + integer` operator and yields one day. A timestamp overload of `generate_series` takes an `interval` step instead. Extra columns such as month or weekend flag are expressions on that date. Once stored, `LEFT JOIN` from the calendar to facts on the date shows gaps as null measures, which `COALESCE` can turn into zero.

**When to use:**

Store the table when many reports need the same date spine. Call `generate_series` inline when the range is small and used once.

**Examples:**

```sql
CREATE TABLE calendar AS
SELECT DATE '2026-01-01' + s.n AS day
FROM generate_series(0, 364) AS s(n);
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-srf.html
</details>

### Data Manipulation and ETL

<details>
<summary>What is the process of Extract, Transform, Load (ETL)?</summary>

### What is the process of Extract, Transform, Load (ETL)?

**Definition:**

ETL moves data from a source into a destination table. Extract reads the source, transform reshapes and cleans it, and load writes the result.

**How it works:**

In PostgreSQL, extract is `COPY ... FROM`, a foreign table, or `SELECT` from another table. Transform is SQL in between: casts, `trim`, `COALESCE`, joins, and aggregates, often landing in a staging table. Load is `INSERT INTO target SELECT ... FROM stage` or `COPY` into the destination. Constraints on the target reject rows the transform missed.

**Examples:**

```sql
INSERT INTO sales_fact (region, total_amount)
SELECT region, sum(amount)
FROM sales_stage
GROUP BY region;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-copy.html
- https://www.postgresql.org/docs/current/sql-insert.html
</details>

<details>
<summary>How do you import/export data from/to a flat file using SQL?</summary>

### How do you import/export data from/to a flat file using SQL?

**Definition:**

`COPY` moves rows between a table and a file or between a query and a file. Formats include `text`, `csv`, and `binary`.

**How it works:**

`COPY table FROM 'filename'` reads a file on the database server. `COPY table TO 'filename'` writes one. `COPY (SELECT ...) TO` exports a query. Options include `FORMAT csv`, `HEADER`, `DELIMITER`, `NULL`, and `ENCODING`. Server-side file `COPY` is restricted to superusers and roles that are granted permission to run it, because it reads and writes as the server operating-system user. psql's `\copy` streams through the client and uses the client's filesystem instead.

**Examples:**

```sql
COPY orders (order_id, amount)
FROM STDIN WITH (FORMAT csv, HEADER true);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-copy.html
</details>

<details>
<summary>Explain the steps for a basic ETL process in a data warehousing environment.</summary>

### Explain the steps for a basic ETL process in a data warehousing environment.

**Definition:**

A basic warehouse load stages the extract, checks and reshapes it, then inserts aggregates or facts into the warehouse tables.

**How it works:**

Create a staging table without the warehouse's final constraints if you need to inspect bad rows. `COPY` the file into staging. Transform with `INSERT ... SELECT` using casts, `NULLIF` for sentinels, and joins to dimension keys. Load the fact table inside a transaction. Refresh a materialized view that reports read. `TRUNCATE` the stage when the load has committed, or keep it for audit.

**Examples:**

```sql
BEGIN;
INSERT INTO sales_fact (day, region, total_amount)
SELECT day, region, sum(amount)
FROM sales_stage
GROUP BY day, region;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-copy.html
- https://www.postgresql.org/docs/current/sql-insert.html
- https://www.postgresql.org/docs/current/rules-materializedviews.html
</details>

<details>
<summary>How do you cleanse and format data using SQL queries?</summary>

### How do you cleanse and format data using SQL queries?

**Definition:**

Cleansing rewrites source values into the types and domains the destination accepts. Formatting functions change case, whitespace, and null markers.

**How it works:**

`trim` and `btrim` remove leading and trailing characters. `lower` and `upper` fold case. `NULLIF(code, '')` turns an empty string into null so it does not masquerade as a value. `COALESCE` supplies a default. `CAST` applies the destination type and fails the row when the text is not valid. A `CHECK` constraint on the destination keeps the cleaned rule in force after the load. `DISTINCT` or `GROUP BY` collapses exact duplicate extracts.

**Examples:**

```sql
SELECT NULLIF(btrim(email), '') AS email,
       lower(btrim(status)) AS status
FROM import_customers;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-string.html
- https://www.postgresql.org/docs/current/functions-conditional.html
</details>

<details>
<summary>What tools do you use for automating data import/export routines?</summary>

### What tools do you use for automating data import/export routines?

**Definition:**

PostgreSQL's own tools for repeatable import and export are `COPY`, `pg_dump`, and `pg_restore`. Scheduling them is outside the SQL language.

**How it works:**

A script runs `psql -f load.sql` or `pg_dump` on a timer provided by the operating system. `pg_dump` writes a consistent snapshot of one database to a script or custom archive. `pg_restore` loads a custom archive. `COPY` is the row-level flat-file path inside that script. `pg_dump` does not dump the server configuration, and it dumps each database separately.

**Examples:**

```sql
COPY orders TO STDOUT WITH (FORMAT csv, HEADER true);
```

**Sources:**

- https://www.postgresql.org/docs/current/backup-dump.html
- https://www.postgresql.org/docs/current/app-pgdump.html
- https://www.postgresql.org/docs/current/app-pgrestore.html
</details>

### Domain-Specific SQL Scenarios

<details>
<summary>How would you model a many-to-many relationship in SQL?</summary>

### How would you model a many-to-many relationship in SQL?

**Definition:**

A many-to-many relationship is a junction table whose rows pair a key from each side. Each pairing is one row. The two foreign keys together are the primary key, unless the pairing itself needs its own identifier.

**How it works:**

Each foreign key references one parent. `ON DELETE CASCADE` removes pairings when a parent goes away; omit it when the pairing must block the delete. Query both directions with two joins through the junction table.

**Examples:**

```sql
CREATE TABLE enrollment (
    student_id integer REFERENCES student (student_id),
    course_id integer REFERENCES course (course_id),
    PRIMARY KEY (student_id, course_id)
);
```

**Sources:**

- https://www.postgresql.org/docs/current/tutorial-fk.html
- https://www.postgresql.org/docs/current/ddl-constraints.html
</details>

<details>
<summary>Describe how to manage hierarchical data in SQL.</summary>

### Describe how to manage hierarchical data in SQL.

**Definition:**

An adjacency list stores each row's parent id. A recursive CTE walks from a starting node to descendants or ancestors.

**How it works:**

The anchor selects the root. The recursive term joins the table to the CTE on `parent_id`. `UNION ALL` keeps every step. A depth column or a path array stops cycles: stop when the id is already in the path. The `ltree` extension is an alternative type for label paths; the recursive query does not require it.

**Examples:**

```sql
WITH RECURSIVE subtrees AS (
    SELECT id, parent_id, name, 1 AS depth
    FROM nodes
    WHERE parent_id IS NULL
  UNION ALL
    SELECT n.id, n.parent_id, n.name, s.depth + 1
    FROM nodes AS n
    JOIN subtrees AS s ON n.parent_id = s.id
)
SELECT id, name, depth
FROM subtrees;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-with.html
- https://www.postgresql.org/docs/current/ltree.html
</details>

<details>
<summary>How would you approach writing SQL queries for a reporting application?</summary>

### How would you approach writing SQL queries for a reporting application?

**Definition:**

A reporting query aggregates stable grain, keeps missing groups visible, and has an explicit order. It is often backed by a materialized view when the same summary is read often.

**How it works:**

Start from the grain of the report, such as day and region. `GROUP BY` those columns. `LEFT JOIN` from a calendar or dimension so empty groups remain, and `COALESCE` the measure to zero. Window functions add shares of a total without a second collapse. `ORDER BY` matches the report layout. If `EXPLAIN ANALYZE` shows a repeated expensive aggregate, store it as a materialized view and refresh it on a schedule.

**Examples:**

```sql
SELECT c.day,
       COALESCE(sum(s.amount), 0) AS total_amount
FROM calendar AS c
LEFT JOIN sales AS s ON s.day = c.day
GROUP BY c.day
ORDER BY c.day;
```

**Sources:**

- https://www.postgresql.org/docs/current/queries-table-expressions.html
- https://www.postgresql.org/docs/current/tutorial-window.html
- https://www.postgresql.org/docs/current/rules-materializedviews.html
</details>

<details>
<summary>Explain how to handle temporal data and time zones in SQL.</summary>

### Explain how to handle temporal data and time zones in SQL.

**Definition:**

`timestamp with time zone` (`timestamptz`) stores an instant. `timestamp without time zone` stores a clock reading with no zone. PostgreSQL does not store the original zone name inside a `timestamptz` value.

**How it works:**

Input that includes a zone is converted to UTC for storage. Display uses the session `TimeZone` parameter. `AT TIME ZONE` converts between `timestamptz` and `timestamp`: `timestamptz AT TIME ZONE zone` yields `timestamp without time zone` in that zone. `date_trunc('day', ts)` truncates in the session zone for `timestamptz`. `now()` and `CURRENT_TIMESTAMP` return `timestamptz`.

**When to use:**

Store instants, such as when a payment was committed, as `timestamptz`. Store a civil date or a wall time that must not move, such as a store's local opening date, as `date` or `timestamp without time zone`.

**Examples:**

```sql
SELECT ordered_at AT TIME ZONE 'Europe/Warsaw' AS warsaw_local
FROM orders;
```

**Sources:**

- https://www.postgresql.org/docs/current/datatype-datetime.html
</details>

<details>
<summary>How do you use SQL in financial applications for risk and portfolio analysis?</summary>

### How do you use SQL in financial applications for risk and portfolio analysis?

**Definition:**

Portfolio math in PostgreSQL uses `numeric` for quantities, transactions for updates that must stay consistent, and aggregates or windows for exposure.

**How it works:**

`numeric` is exact; `real` and `double precision` are not appropriate for money. A transfer is two updates in one transaction. Position value is `sum(quantity * price)`. A window can compute a running net or a share of the book: `sum(market_value) OVER (PARTITION BY portfolio_id)`. Constraints keep quantity domains explicit. SQL can aggregate exposures; fitting a statistical model is not a built-in SQL command.

**Examples:**

```sql
SELECT portfolio_id,
       sum(quantity * price) AS market_value
FROM positions
GROUP BY portfolio_id;
```

**Sources:**

- https://www.postgresql.org/docs/current/datatype-numeric.html
- https://www.postgresql.org/docs/current/tutorial-transactions.html
- https://www.postgresql.org/docs/current/functions-aggregate.html
</details>

### Troubleshooting and Debugging

<details>
<summary>What steps do you take to troubleshoot a failed SQL query?</summary>

### What steps do you take to troubleshoot a failed SQL query?

**Definition:**

A failed statement returns an error with a SQLSTATE code and a primary message. The code class identifies the kind of failure.

**How it works:**

Read the message and the `ERROR` line before changing the query. Syntax errors point at a position. Constraint failures name the constraint (`23505` unique violation, `23503` foreign key, `23502` not null, `23514` check). Permission failures are `42501`. A statement error inside a transaction aborts that transaction until `ROLLBACK`. If the statement is valid but slow, that is a plan problem for `EXPLAIN`, not a failure. Server detail beyond the client message is in the PostgreSQL log.

**Examples:**

```sql
INSERT INTO employee (emp_id, emp_name, dept_id)
VALUES (1, 'Ada', 999);
```

**Sources:**

- https://www.postgresql.org/docs/current/errcodes-appendix.html
- https://www.postgresql.org/docs/current/tutorial-transactions.html
</details>

<details>
<summary>How can you recover data from a corrupt SQL database?</summary>

### How can you recover data from a corrupt SQL database?

**Definition:**

Recovery uses a backup and the write-ahead log. A corrupt heap or index is not repaired by a normal SQL statement.

**How it works:**

`pg_dump` takes a logical backup of a database that can still be read. Continuous archiving plus a base backup allows point-in-time recovery: restore the base backup and replay WAL up to a chosen time. If the server cannot read a page, stop writing and restore from that backup. Reindexing can rebuild a damaged index when the heap is intact: `REINDEX INDEX name`. Guessing at removed files or resetting WAL is a last resort documented for experts and can lose committed data.

**Examples:**

```sql
REINDEX INDEX films_kind_idx;
```

**Sources:**

- https://www.postgresql.org/docs/current/backup.html
- https://www.postgresql.org/docs/current/continuous-archiving.html
- https://www.postgresql.org/docs/current/sql-reindex.html
</details>

<details>
<summary>What methods do you employ to ensure data integrity?</summary>

### What methods do you employ to ensure data integrity?

**Definition:**

Integrity is enforced with constraints, transactions, and privileges so invalid data cannot be committed.

**How it works:**

`PRIMARY KEY` and `UNIQUE` stop duplicate keys. `FOREIGN KEY` requires the referenced row. `NOT NULL` and `CHECK` limit a column. An exclusion constraint stops overlapping ranges. `DEFERRABLE` constraints can wait until commit. A transaction keeps multi-row changes atomic. Row-level security and grants limit who can write. Constraints are checked by the server for every client.

**Examples:**

```sql
CREATE TABLE orders (
    order_id integer PRIMARY KEY,
    amount numeric(12, 2) NOT NULL CHECK (amount >= 0),
    customer_id integer NOT NULL REFERENCES customers (customer_id)
);
```

**Sources:**

- https://www.postgresql.org/docs/current/ddl-constraints.html
- https://www.postgresql.org/docs/current/tutorial-transactions.html
</details>

<details>
<summary>How do you decipher and resolve deadlocks in SQL?</summary>

### How do you decipher and resolve deadlocks in SQL?

**Definition:**

A deadlock is a cycle of transactions waiting on one another's locks. PostgreSQL detects the cycle, aborts one transaction, and returns SQLSTATE `40P01`.

**How it works:**

The error detail lists the processes and the locks. `log_lock_waits` and `deadlock_timeout` control related wait logging. The aborted transaction must be retried by the application. The usual prevention is to lock rows in a consistent order in every transaction, and to keep transactions short. `SELECT ... FOR UPDATE` makes that order explicit. `pg_locks` joined to `pg_stat_activity` shows waiters while they are still blocked.

**Examples:**

```sql
BEGIN;
SELECT emp_id FROM employee WHERE emp_id = 1 FOR UPDATE;
SELECT emp_id FROM employee WHERE emp_id = 2 FOR UPDATE;
COMMIT;
```

**Sources:**

- https://www.postgresql.org/docs/current/explicit-locking.html
- https://www.postgresql.org/docs/current/runtime-config-locks.html
</details>

### Advanced Data Analysis in SQL

<details>
<summary>Explain how to use SQL for predictive analysis and machine learning purposes.</summary>

### Explain how to use SQL for predictive analysis and machine learning purposes.

**Definition:**

Core PostgreSQL does not train or store a machine-learning model. SQL prepares features and labels and can apply a simple numeric formula you write yourself.

**How it works:**

Aggregates and windows build the training table: counts, sums, lags, and ratios per entity. `COPY` exports that result for a tool that fits a model. A scored column can be loaded back and joined. A hand-written formula, such as a weighted sum in a `SELECT`, is ordinary arithmetic, not a fitted model. There is no `CREATE MODEL` statement in core SQL.

**When to use:**

Use SQL to define a reproducible feature query and to serve scores that have already been written into a table. Do the fit in a system that is built to train models, then bring the scores or the parameters back if the application needs them in the database.

**Examples:**

```sql
COPY (
    SELECT customer_id,
           count(*) AS order_count,
           sum(amount) AS total_amount
    FROM orders
    GROUP BY customer_id
) TO STDOUT WITH (FORMAT csv, HEADER true);
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-aggregate.html
- https://www.postgresql.org/docs/current/tutorial-window.html
- https://www.postgresql.org/docs/current/sql-copy.html
</details>
