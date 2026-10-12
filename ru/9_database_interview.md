# Database — Interview

Ответы заполнены для всех вопросов в этом файле.

[ ← Вернуться в хаб](./9_database.md)

### Related docs

- [БД — Основы](./9_database_core.md) · [БД — SQL](./9_database_sql.md) · [БД — NoSQL](./9_database_nosql.md) · [БД — ORM](./9_database_orm.md) · [БД — Производство](./9_database_production.md) · [БД — Интервью](./9_database_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы с [Devinterview.io — SQL](https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/).

### SQL Fundamentals

<details>
<summary>Что такое SQL and what is it used for?</summary>

### Что такое SQL and what is it used for?

**Definition:**

SQL (Structured Query Language). SQL — стандартизированный предметно-ориентированный декларативный язык для управления реляционными СУБД (RDBMS) и запросов к ним. Помимо унаследованных реляционных моделей, современные диалекты SQL (например, PostgreSQL 18, DuckDB 1.2) поддерживают полуструктурированные данные (JSONB) и векторные эмбеддинги для сценариев RAG (Retrieval-Augmented Generation).

**How it works:**

Базовые компоненты: DDL (`CREATE`, `ALTER`, `DROP`). DML (`INSERT`, `UPDATE`, `DELETE`, `MERGE`). DCL (`GRANT`, `REVOKE`). TCL (`COMMIT`, `ROLLBACK`, `SAVEPOINT`). DQL (`SELECT`).

Основные команды: `SELECT` с CTE и оконными функциями (`OVER`, `PARTITION BY`); upsert через `MERGE`; `LATERAL JOIN`; `MATERIALIZED VIEW`.

**When to use:**

Современные задачи: выборка с агрегацией и оконными функциями; ACID; векторный поиск через pgvector и k-NN; нормализация и индексы B-Tree, LSM-Tree, BRIN для точечных поисков за O(log n); шардирование и репликация.

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

Производительность 2026: Index Scan против Seq Scan; `EXPLAIN ANALYZE`; MVCC, поэтому читатели не блокируют писателей.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Опишите the difference between SQL and NoSQL dataBASEs.</summary>

### Опишите the difference between SQL and NoSQL dataBASEs.

**Definition:**

SQL использует реляционную модель (Codd, 1970), ACID и жёсткие схемы. NoSQL строится вокруг горизонтального масштабирования и теоремы CAP и часто работает как AP с согласованностью в конечном счёте.

**How it works:**

Таксономия: документ, ключ-значение, широкие столбцы, граф.

Сравнение: schema-on-write против schema-on-read; ограничения против проверки в приложении; соединения против денормализации; масштабирование; декларативный SQL против проприетарного API. Тот же разрез — нормализация против денормализации и замечание про ACID.

Модели транзакций: 2PC / Paxos / Raft против векторных часов / LWW.

2026: JSONB в PostgreSQL; многодокументный ACID в MongoDB; распределённый SQL (CockroachDB, TiDB, YugabyteDB).

**When to use:**

SQL — для целостности. NoSQL — для высокой скорости поступления данных, разреженных данных или меняющейся схемы. Распределённый SQL — когда нужен SQL и эластичный масштаб.

**Pluses and minuses:**

- SQL даёт ограничения, соединения и ACID. Схема жёсткая, а масштабирование вширь — более трудная ось.
- NoSQL даёт горизонтальный масштаб и более свободную схему. Часто это меняют на согласованность в конечном счёте и проприетарный API.
- Распределённый SQL сохраняет декларативный SQL и добавляет эластичный масштаб. Координация (2PC, Paxos, Raft) — цена этого выбора.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Что такое the different types of SQL commands?</summary>

### Что такое the different types of SQL commands?

**Definition:**

Команды SQL делят на DQL, DDL, DML, TCL и DCL.

**How it works:**

- DQL: `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, оконные функции, `JOIN`, CTE.
- DDL: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT`.
- DML: `INSERT`, `UPDATE`, `DELETE`, `MERGE`.
- TCL: `COMMIT`, `ROLLBACK`, `SAVEPOINT`.
- DCL: `GRANT`, `REVOKE`, `DENY`.

**Thoughts:**

В SQL:2023/2025 DQL — подмножество DML, но в обучении его держат отдельно.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Объясните the puRPOse of the SELECT statement.</summary>

### Объясните the puRPOse of the SELECT statement.

**Definition:**

`SELECT` извлекает, проецирует и преобразует.

**How it works:**

Логический порядок: `FROM` / `JOIN`, `WHERE`, `GROUP BY`, `HAVING`, `SELECT`, `DISTINCT`, `ORDER BY`, `LIMIT` / `OFFSET`.

Тот же оператор покрывает JSONB, векторы, оконные функции и CTE.

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

`EXPLAIN ANALYZE` показывает, nested loop это или hash join.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Что такое the difference between WHERE and HAVING clauses?</summary>

### Что такое the difference between WHERE and HAVING clauses?

**Definition:**

`WHERE` — фильтр строк до агрегации. `HAVING` — фильтр групп после агрегации.

**How it works:**

`WHERE` подходит для SARGable-условий и не принимает агрегаты. `HAVING` может использовать `COUNT` и `SUM`.

Конвейер: `FROM` / `JOIN`, `WHERE`, `GROUP BY`, `HAVING`, `SELECT`, `ORDER BY`.

**Thoughts:**

Оптимизаторы могут переписать `HAVING` без агрегатов в `WHERE`.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Определите what a JOIN is in SQL and list its types.</summary>

### Определите what a JOIN is in SQL and list its types.

**Definition:**

`JOIN` соединяет строки по предикату.

**How it works:**

Nested loop — O(n*m). Hash и sort-merge — в среднем O(n+m).

Типы: `INNER`, `LEFT`, `RIGHT`, `FULL`, `CROSS`, `SELF`.

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

Избегайте `SELECT *`. Подсказки hash distribution.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Что такое Primary key in a dataBASE?</summary>

### Что такое Primary key in a dataBASE?

**Definition:**

Первичный ключ уникален, имеет `NOT NULL`, и его не следует менять.

**How it works:**

Он обеспечивает целостность сущности, служит целью внешнего ключа и часто является кластерным индексом. Предпочтительны суррогатные ключи: identity на `BIGINT` или UUIDv7, и сам ключ лучше держать узким.

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
<summary>Объясните what a foreign key is and how it is used.</summary>

### Объясните what a foreign key is and how it is used.

**Definition:**

Внешний ключ обеспечивает ссылочную целостность на первичный ключ или уникальный ключ.

**How it works:**

`NULL` допустим, пока столбец не объявлен `NOT NULL`. `ON DELETE` и `ON UPDATE` задают, что делать при изменении родительской строки. Столбцы внешнего ключа сами по себе не индексируются; индекс добавляют отдельно. Внешний ключ у потомка не обязан быть уникальным: это связь один ко многим.

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
<summary>Как you prevent SQL injections?</summary>

### Как you prevent SQL injections?

**Definition:**

Сначала — параметризованные запросы.

**How it works:**

Значения связывают, а не склеивают в текст оператора. Хранимые процедуры помогают, только если сами не собирают SQL-строки. Проверка по allow-list смотрит идентификаторы и столбцы сортировки до подстановки. Минимальные привилегии ограничивают ущерб, если инъекция всё же пройдёт.

**Pluses and minuses:**

Ручное экранирование регулярными выражениями — антипаттерн. Используйте ORM и связанные параметры.

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
<summary>Что такое normalization? Объясните с примерами.</summary>

### Что такое normalization? Объясните с примерами.

**Definition:**

Нормализация убирает повторяющиеся и зависимые факты, чтобы каждый факт хранился один раз: 1NF — атомарные значения, 2NF — нет частичной зависимости, 3NF — нет транзитивной зависимости, далее BCNF и 4NF.

**How it works:**

Ненормализованная таблица счетов повторяет покупателя и складывает несколько товаров в одну строку. Её делят на `Customers`, `Invoices`, `Invoice_Items` и `Products`.

**When to use:**

OLTP остаётся в 3NF. OLAP использует схему «звезда».

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
<summary>Опишите the concept of denormalization and when you would use it.</summary>

### Опишите the concept of denormalization and when you would use it.

**Definition:**

Денормализация — намеренная избыточность ради задержки чтения.

**How it works:**

Обычные формы: сплющивание, материализованные представления, избыточные атрибуты и схема «звезда». OLTP остаётся в 3NF. OLAP денормализует. При шардировании копия даёт локальность данных.

**When to use:**

Предпочтительны материализованные представления или реплики для чтения. Физическую денормализацию берут только когда соединения доминируют в стоимости запроса.

**Pluses and minuses:**

Чтение дешевеет. Согласованность тогда держится на CDC, запись усиливается, а эволюция схемы усложняется: один и тот же факт живёт в нескольких местах.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Что такое indexes and how can they improve query performance?</summary>

### Что такое indexes and how can they improve query performance?

**Definition:**

Индексы — вспомогательные структуры. Они превращают просмотр O(n) в поиск O(log n) или O(1).

**How it works:**

Обычные виды: B+Tree, LSM, hash, BRIN, GIN / GiST.

Практика: `EXPLAIN ANALYZE`, частичные индексы, составные индексы по левому префиксу, покрывающие индексы с `INCLUDE` и свежая статистика. Первичные и уникальные ключи создают индексы. Внешние ключи часто не создают.

**Pluses and minuses:**

Поиск и часть сортировок дешевеют. Цена — усиление записи, дополнительное место на диске и слабая польза при низкой селективности.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Объясните the puRPOse of the GROUP BY clause.</summary>

### Объясните the puRPOse of the GROUP BY clause.

**Definition:**

`GROUP BY` разбивает строки для агрегатов.

**How it works:**

Движок может агрегировать через хеш или через поток. `ONLY_FULL_GROUP_BY` отвергает выбранный столбец, который не входит в группировку и не обёрнут в агрегат. Агрегация по секциям может считать группу внутри каждой секции.

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
<summary>Что такое subquery, and when would you use one?</summary>

### Что такое subquery, and when would you use one?

**Definition:**

Подзапрос — это внутренний `SELECT`.

**How it works:**

Скалярный подзапрос возвращает одно значение. Табличный подзапрос возвращает отношение. Некоррелированный подзапрос можно выполнить один раз. Коррелированный ссылается на внешнюю строку.

**When to use:**

CTE, оконные функции и `LATERAL` предпочтительны, когда вложенность нужна только чтобы посчитать значение на группу или на внешнюю строку.

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
<summary>Опишите the functions of the ORDER BY clause.</summary>

### Опишите the functions of the ORDER BY clause.

**Definition:**

`ORDER BY` сортирует результат, который иначе не упорядочен.

**How it works:**

Каждый ключ идёт как `ASC` или `DESC`, с `NULLS FIRST` или `NULLS LAST`. Сортировка стоит O(N log N). Индекс, который уже совпадает с этим порядком, может сделать её O(N).

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

`ORDER BY 1` лучше не писать. `ORDER BY RANDOM()` дорогой.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/sql-interview-questions/
</details>

<details>
<summary>Что такое aggregate functions in SQL?</summary>

### Что такое aggregate functions in SQL?

**Definition:**

Агрегатная функция считает одно значение по набору входных строк. К общим агрегатам относятся `count`, `sum`, `avg`, `min`, `max`, `bool_and`, `bool_or`, `array_agg`, `string_agg`, `json_agg` и `jsonb_agg`.

**How it works:**

Большинство агрегатов пропускает null. `count(*)` считает входные строки. `count(expression)` считает строки, где выражение не null. `avg` от целого аргумента возвращает `numeric`. Агрегаты обычно идут вместе с `GROUP BY`; без него запрос — это одна группа. `FILTER (WHERE condition)` ограничивает, какие строки попадают в этот агрегат. `DISTINCT` внутри агрегата, как в `count(DISTINCT city)`, сначала схлопывает повторяющиеся входы.

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
<summary>Объясните the differences between INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.</summary>

### Объясните the differences between INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL JOIN.

**Definition:**

Эти соединения собирают строки двух таблиц по условию совпадения. `INNER` оставляет только совпадения. `LEFT`, `RIGHT` и `FULL` дополнительно сохраняют несовпавшие строки одной стороны или обеих и заполняют другую сторону null.

**How it works:**

`INNER JOIN` возвращает строку только когда условие `ON` истинно. `LEFT JOIN` (то же, что `LEFT OUTER JOIN`) возвращает каждую левую строку; несовпавшие правые столбцы равны null. `RIGHT JOIN` возвращает каждую правую строку. `FULL JOIN` возвращает каждую строку обеих сторон. `USING (col)` сливает общий столбец в один выходной столбец. `OUTER` необязателен.

**When to use:**

Берите `INNER`, когда совпасть должны обе стороны. Берите `LEFT`, когда ведущая таблица должна остаться полной, включая строки без пары. Если так читается яснее, поменяйте таблицы местами и напишите `LEFT` вместо `RIGHT`. Берите `FULL`, когда важны несовпавшие строки с обеих сторон.

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
<summary>Как you insert a new row into a dataBASE table?</summary>

### Как you insert a new row into a dataBASE table?

**Definition:**

`INSERT` добавляет строки в таблицу. Вы называете целевые столбцы и передаёте значения через `VALUES`, запрос или `DEFAULT`.

**How it works:**

Пропущенные столбцы получают значение по умолчанию либо null, если умолчания нет и столбец допускает null. `RETURNING` отдаёт выражения, посчитанные по каждой вставленной строке. `ON CONFLICT` превращает нарушение уникальности в `DO NOTHING` или `DO UPDATE`. На таблицу нужно право `INSERT`.

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
<summary>Объясните how to update records in a dataBASE table.</summary>

### Объясните how to update records in a dataBASE table.

**Definition:**

`UPDATE` меняет столбцы в строках, которые удовлетворяют условию `WHERE`. Столбцы, не перечисленные в `SET`, сохраняют прежние значения.

**How it works:**

Новое значение может быть выражением, `DEFAULT` или подзапросом `SELECT` из одной строки. Предложение `FROM` может подключить другие таблицы к присваиванию, но каждая целевая строка должна совпасть не больше чем с одной присоединённой строкой, иначе выбранная исходная строка непредсказуема. `RETURNING` может показать новые значения, а `OLD.column` — прежние. Без `WHERE` обновляется каждая строка.

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
<summary>Что такое SQL View and what are its advantages?</summary>

### Что такое SQL View and what are its advantages?

**Definition:**

Представление — это именованный запрос. `CREATE VIEW` не хранит результат; запрос выполняется заново при каждом обращении к представлению.

**How it works:**

Имя представления должно быть уникальным среди отношений схемы. Простое представление автоматически обновляемо, если в нём одно базовое отношение и на верхнем уровне нет `DISTINCT`, `GROUP BY`, `HAVING`, `LIMIT`, `OFFSET`, операции над множествами, агрегата, оконной функции или функции, возвращающей набор. `WITH CHECK OPTION` отклоняет строки `INSERT`, `UPDATE` и `MERGE`, которые не были бы видны через представление. По умолчанию доступ к базовым таблицам проверяется от имени владельца представления. `security_invoker = true` проверяет их от имени вызывающего пользователя.

**When to use:**

Представление нужно, чтобы переиспользовать запрос, показать стабильный список столбцов или выдать доступ к подмножеству таблицы. Материализованное представление нужно, когда требуется сохранённый результат, а не запрос, который выполняется каждый раз.

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
<summary>Перечислите the different data types available in SQL.</summary>

### Перечислите the different data types available in SQL.

**Definition:**

Типы PostgreSQL покрывают логические значения, числа, символьные строки, двоичные данные, даты и время, сетевые адреса, JSON, массивы, диапазоны, перечисления и геометрические значения, а также домены и составные типы, которые задаёте вы.

**How it works:**

К числовым типам относятся `smallint`, `integer`, `bigint`, `numeric`, `real` и `double precision`. Символьные типы — `character varying(n)`, `character(n)` и `text`. К типам даты и времени относятся `date`, `time`, `timestamp`, `timestamptz` и `interval`. `boolean` — это `true` или `false`. `bytea` хранит сырые байты. `json` и `jsonb` хранят JSON. `uuid`, `inet`, `cidr` и массивы вроде `integer[]` встроены. `numeric` точный; `real` и `double precision` приближённые.

**When to use:**

Берите `numeric` для точных величин, например денег. Берите `text` или `varchar` для строк. Берите `timestamptz`, когда значение — это момент времени, который должен пережить смену часового пояса. Берите `jsonb`, когда полезная нагрузка — это JSON, по которому вы будете делать запросы.

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
<summary>Что такое the differences between CHAR, VARCHAR, and TEXT data types?</summary>

### Что такое the differences between CHAR, VARCHAR, and TEXT data types?

**Definition:**

`character(n)` (`char(n)`) — фиксированной длины и дополняется пробелами. `character varying(n)` (`varchar(n)`) — переменной длины с ограничением по символам. `text` — переменной длины без объявленного предела.

**How it works:**

Длина `n` считает символы, а не байты, и должна быть от 1 до 10485760. Запись более длинной строки вызывает ошибку, если лишние символы — не пробелы; пробелы усекаются. Явное приведение к `varchar(n)` или `char(n)` усекает без ошибки. Короткие значения `character(n)` хранятся с дополнением пробелами; концевые пробелы игнорируются при сравнении значений `character` и снимаются при преобразовании в `varchar` или `text`. Концевые пробелы в `varchar` и `text` значимы, в том числе для `LIKE`. `varchar` без `n` принимает любую длину. У `character(n)` в PostgreSQL нет преимущества по скорости; дополнение пробелами стоит места, а проверка длины — немного CPU. Самая длинная хранимая строка — около 1 ГБ.

**When to use:**

Предпочитайте `text` или `character varying`. `character(n)` используйте только когда дополненная пробелами фиксированная ширина входит в контракт данных.

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
<summary>Как you use the BETWEEN operator in SQL?</summary>

### Как you use the BETWEEN operator in SQL?

**Definition:**

`BETWEEN` проверяет, лежит ли значение внутри диапазона, включая оба конца.

**How it works:**

`a BETWEEN x AND y` эквивалентно `a >= x AND a <= y`. Если `x` больше `y`, предикат никогда не истинен. `BETWEEN SYMMETRIC` считает границы неупорядоченными, поэтому концами становятся меньшее и большее значения. `NOT BETWEEN` отрицает проверку. Операнд null даёт null, а не истину.

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
<summary>Опишите the use of the IN operator.</summary>

### Опишите the use of the IN operator.

**Definition:**

`IN` проверяет вхождение в список значений или в результат подзапроса.

**How it works:**

`expression IN (value1, value2, ...)` истинно, когда выражение равно любому перечисленному значению. `expression IN (subquery)` истинно, когда оно равно любой строке, которую вернул подзапрос. `NOT IN` — это отрицание. Если выражение равно null, результат — null. Если какое-либо значение списка или подзапроса равно null и равного значения не найдено, `NOT IN` даёт null, а не истину, поэтому строка отфильтровывается из предложения `WHERE`.

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
<summary>Объясните the use of wildcard characters in SQL.</summary>

### Объясните the use of wildcard characters in SQL.

**Definition:**

В шаблонах `LIKE` и `ILIKE` знак `%` совпадает с любой последовательностью символов, включая пустую, а `_` — ровно с одним символом.

**How it works:**

Шаблон — это не регулярное выражение. Чтобы совпасть с буквальными `%` или `_`, объявите экранирующий символ: `LIKE '100\%' ESCAPE '\'`. `SIMILAR TO` использует подстановочные знаки регулярных выражений SQL, а `~` — регулярные выражения POSIX. Индекс B-tree может поддержать `LIKE`, только когда шаблон — константа, привязанная к началу, например `'foo%'`, и часто нужен класс операторов для шаблонов, если правило сортировки не `C`.

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
<summary>Что такое the puRPOse of the LIKE operator?</summary>

### Что такое the puRPOse of the LIKE operator?

**Definition:**

`LIKE` сопоставляет строку с шаблоном. Он чувствителен к регистру. `ILIKE` — нечувствительная к регистру форма PostgreSQL. `NOT LIKE` и `NOT ILIKE` отрицают проверку.

**How it works:**

`string LIKE pattern` возвращает true, false или null. Если любой аргумент равен null, результат — null. Экранирующий символ по умолчанию — обратная косая черта. `~~` — операторный эквивалент `LIKE`, а `~~*` — эквивалент `ILIKE`. Концевые пробелы значимы для `text` и `varchar`.

**When to use:**

`LIKE` подходит для простых шаблонов префикса, суффикса или одного символа. Операторы POSIX (`~`, `~*`) нужны, когда шаблону требуются возможности регулярных выражений, которых нет у `%` и `_`.

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
<summary>Как you handle NULL values in SQL?</summary>

### Как you handle NULL values in SQL?

**Definition:**

Null означает, что значение неизвестно или отсутствует. Он не равен пустой строке, нулю или другому null.

**How it works:**

Сравнения через `=` или `<>` с участием null дают null, поэтому в `WHERE` они не проходят. Проверяйте null через `IS NULL` и `IS NOT NULL`. `IS DISTINCT FROM` считает null обычным значением и возвращает boolean. Большинство агрегатов пропускает null. `count(*)` всё равно считает строку. Ограничение `UNIQUE` считает null различными, если не объявить `NULLS NOT DISTINCT`, поэтому по умолчанию допускается больше одного null. `COALESCE` и `NULLIF` подставляют значение или создают null. `ORDER BY ... NULLS FIRST` или `NULLS LAST` явно задаёт их место.

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
<summary>Что означает the COALESCE function do?</summary>

### Что означает the COALESCE function do?

**Definition:**

`COALESCE(value [, ...])` возвращает первый аргумент, который не null. Null возвращается только когда каждый аргумент равен null.

**How it works:**

Аргументы должны приводиться к одному типу результата. Аргументы после первого ненулевого не вычисляются. Это стандартный SQL-эквивалент `NVL` и `IFNULL` в других системах. `NULLIF(value1, value2)` — обратный приём: возвращает null, когда два значения равны, иначе `value1`.

**Examples:**

```sql
SELECT COALESCE(description, short_description, '(none)')
FROM products;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-conditional.html
</details>

<details>
<summary>Что такое the difference between UNION and UNION ALL?</summary>

### Что такое the difference between UNION and UNION ALL?

**Definition:**

`UNION` объединяет результаты двух запросов и удаляет повторяющиеся строки. `UNION ALL` объединяет их и сохраняет каждую строку, включая повторы.

**How it works:**

Запросы должны вернуть одинаковое число столбцов, а соответствующие столбцы — совместимые типы. Имена столбцов берутся из первого запроса. `UNION` подразумевает шаг устранения дубликатов. `INTERSECT` оставляет строки, присутствующие в обоих результатах, а `EXCEPT` — строки первого, которых нет во втором. Варианты этих операторов с `ALL` тоже сохраняют дубликаты. `ORDER BY` применяется к объединённому результату и пишется один раз в конце.

**When to use:**

Берите `UNION ALL`, когда дубликаты невозможны или их нужно сохранить: так не нужна сортировка или хеш для distinct. Берите `UNION`, когда вызывающий должен увидеть каждую различную строку один раз.

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
<summary>Опишите the use of arithmetic operators in SQL queries.</summary>

### Опишите the use of arithmetic operators in SQL queries.

**Definition:**

К числовым операторам PostgreSQL относятся `+`, `-`, `*`, `/` и `%` (остаток). Унарные `+` и `-` тоже допустимы.

**How it works:**

Типы аргументов определяют тип результата. Деление двух целых усекает к нулю; `5 / 2` равно `2`. Приведите хотя бы одну сторону, когда нужна дробная часть: `5::numeric / 2`. `%` — это остаток. `div(y, x)` — целочисленное деление. Делитель ноль вызывает ошибку. Null в арифметическом выражении даёт null.


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
<summary>Объясните how to use the CASE statement in SQL.</summary>

### Объясните how to use the CASE statement in SQL.

**Definition:**

`CASE` — это условное выражение. Оно может стоять везде, где допустимо выражение. Это не оператор, который выполняется сам по себе.

**How it works:**

Искомая форма — `CASE WHEN condition THEN result ... ELSE result END`. Побеждает первое истинное условие, последующие ветки не вычисляются. Если ни одно условие не истинно и нет `ELSE`, результат — null. Простая форма — `CASE expression WHEN value THEN result ... END`: она сравнивает выражение с каждым значением через `=`. Все ветки результата должны иметь общий тип.

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
<summary>Как бы вы perform a self JOIN?</summary>

### Как бы вы perform a self JOIN?

**Definition:**

Самосоединение использует одну и ту же таблицу дважды в одном запросе. Каждому вхождению нужен свой псевдоним, потому что одного имени таблицы недостаточно и оно было бы неоднозначным.

**How it works:**

Условие соединения связывает столбцы двух псевдонимов так же, как для двух разных таблиц. Так обычно сопоставляют строку со связанной строкой той же таблицы, например сотрудника и его руководителя.

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
<summary>Что такое cross JOIN and when would you use it?</summary>

### Что такое cross JOIN and when would you use it?

**Definition:**

`CROSS JOIN` строит декартово произведение: каждая строка первой таблицы в паре с каждой строкой второй. Условия соединения нет.

**How it works:**

Если на входах `N` и `M` строк, в результате `N * M` строк. Запятая в `FROM a, b` — тоже перекрёстное соединение. `CROSS JOIN LATERAL` устроен иначе: правая сторона может ссылаться на текущую левую строку.

**When to use:**

Он нужен для намеренного сочетания двух небольших множеств, например каждой даты с каждым товаром. Не используйте его на больших таблицах, если имелось в виду соединение по равенству и забыли `ON`.

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
<summary>Как implement pagination in SQL queries?</summary>

### Как implement pagination in SQL queries?

**Definition:**

Постраничная выдача возвращает один срез упорядоченного результата. PostgreSQL поддерживает `LIMIT` / `OFFSET` и стандартное предложение `OFFSET ... FETCH`.

**How it works:**

`LIMIT count` останавливается после стольких строк. `OFFSET start` сначала пропускает столько строк. `FETCH FIRST count ROWS ONLY` — стандартная запись `LIMIT`. Планировщик всё равно должен получить пропущенные строки, поэтому большой offset замедляется с ростом номера страницы. Страница по последнему ключу берёт последний увиденный ключ сортировки в `WHERE` и затем `LIMIT`; это остаётся дешёвым, когда ключ проиндексирован.

**When to use:**

`LIMIT` и `OFFSET` достаточны для небольших номеров страниц. Условие по ключу нужно, когда пользователи могут уйти глубоко в большое упорядоченное множество, а ключ сортировки уникален и проиндексирован.

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
<summary>Объясните the concept of Common Table Expressions (CTEs) and Recursive CTEs.</summary>

### Объясните the concept of Common Table Expressions (CTEs) and Recursive CTEs.

**Definition:**

Предложение `WITH` именует вспомогательные запросы, их называют общими табличными выражениями, для одного основного оператора. `WITH RECURSIVE` позволяет CTE ссылаться на собственный выход.

**How it works:**

Каждое CTE может быть `SELECT`, `INSERT`, `UPDATE`, `DELETE` или `MERGE`. Основной оператор тоже может быть одним из них. Нерекурсивное CTE планировщик может встроить, если не написать `AS MATERIALIZED`. У рекурсивного CTE есть нерекурсивная часть, затем `UNION` или `UNION ALL`, затем рекурсивная часть — единственное место, где можно сослаться на имя CTE. Вычисление итеративное: рабочая таблица начинается с нерекурсивного результата и заменяется каждым новым рекурсивным шагом, пока шаг не вернёт ни одной строки. `UNION` отбрасывает дубликаты; `UNION ALL` — нет.

**When to use:**

CTE нужно, чтобы назвать подзапрос, который остальная часть оператора читает больше одного раза, или чтобы длинный запрос читался проще. `RECURSIVE` нужно для деревьев и графов, хранящихся как списки смежности. Добавьте остановку по глубине или пути, чтобы цикл не шёл бесконечно.

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
<summary>Что такое window functions and how are they used?</summary>

### Что такое window functions and how are they used?

**Definition:**

Оконная функция считает значение по набору строк, связанных с текущей строкой, и не схлопывает эти строки так, как это делает `GROUP BY`.

**How it works:**

После вызова идёт `OVER (PARTITION BY ... ORDER BY ... frame)`. `PARTITION BY` делит вход на независимые окна. `ORDER BY` упорядочивает строки внутри раздела. Значения рамки по умолчанию зависят от того, есть ли `ORDER BY`. `row_number`, `rank`, `dense_rank`, `lag`, `lead` и агрегаты вроде `sum` могут быть оконными функциями. Оконная функция недопустима в `WHERE`; фильтруйте её результат во внешнем запросе.

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
<summary>Как you concatenate column values in SQL?</summary>

### Как you concatenate column values in SQL?

**Definition:**

Оператор `||` сцепляет две строки. `concat` сцепляет любое число значений. `concat_ws` вставляет разделитель. `string_agg` сцепляет значения по строкам.

**How it works:**

`'Post' || 'greSQL'` даёт `PostgreSQL`. Если любая сторона `||` равна null, результат — null. `concat` игнорирует аргументы null: `concat('abcde', 2, NULL, 22)` даёт `abcde222`. `concat_ws(',', 'a', NULL, 'b')` даёт `a,b`. Хотя бы один вход `||` должен быть строкой, если вы не приводите тип. `string_agg(expression, delimiter ORDER BY ...)` — агрегатная форма.

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
<summary>Что такое the PIVOT operation and how would you apply it?</summary>

### Что такое the PIVOT operation and how would you apply it?

**Definition:**

Свод поворачивает значения строк в столбцы. В PostgreSQL нет ключевого слова `PIVOT`. Тот же результат строится условными агрегатами или функцией `crosstab` из расширения `tablefunc`.

**How it works:**

`count(*) FILTER (WHERE status = 'open')` считает внутри группы только подходящие строки. `sum(CASE WHEN status = 'open' THEN amount END)` — та же идея для искомого выражения. `crosstab(source_sql)` ждёт запрос, который возвращает имя строки, категорию и значение, упорядоченные по имени строки и категории, а выходные столбцы вы объявляете в псевдониме `AS`.

**When to use:**

Предпочитайте `FILTER` или `CASE`, когда список категорий известен в запросе и нужно остаться в базовом SQL. `crosstab` берите, когда вы уже зависите от `tablefunc` и хотите его помощник для свода.

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
<summary>Объясните the process of combining a query that uses a GROUP BY with one that uses ORDER BY.</summary>

### Объясните the process of combining a query that uses a GROUP BY with one that uses ORDER BY.

**Definition:**

`GROUP BY` строит группы и считает агрегаты. Затем `ORDER BY` сортирует сгруппированный результат. Группировка не обещает никакого порядка выхода.

**How it works:**

`WHERE` фильтрует входные строки до группировки. `HAVING` фильтрует группы после вычисления агрегатов. `ORDER BY` может использовать сгруппированные столбцы, агрегатные выражения или выходные псевдонимы. Обработка `DISTINCT` и `ORDER BY` идёт по сгруппированной проекции. Индекс по столбцам группировки может подать уже упорядоченные строки, но `ORDER BY` всё равно нужно писать, если клиент зависит от порядка.

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
<summary>Как бы вы find duplicate records in a table?</summary>

### Как бы вы find duplicate records in a table?

**Definition:**

Поиск дубликатов группирует столбцы, которые должны быть уникальны, и оставляет группы, где число строк больше одной.

**How it works:**

`GROUP BY` по этим столбцам и `HAVING count(*) > 1` возвращает повторяющиеся ключи и то, как часто они встречаются. Чтобы перечислить каждую повторяющуюся строку, присоедините это множество обратно к таблице или используйте `count(*) OVER (PARTITION BY ...)` и оставьте строки, где оконный счётчик больше одного.

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
<summary>Что такое the Entity-Relationship Model?</summary>

### Что такое the Entity-Relationship Model?

**Definition:**

Модель «сущность–связь» описывает данные как сущности, их атрибуты и связи между ними. В реляционной базе это становится таблицами, столбцами и внешними ключами.

**How it works:**

Сущность — это таблица. Атрибут — это столбец. Связь — это внешний ключ, иногда через таблицу соединения, когда много строк с каждой стороны связаны со многими на другой. Мощность задают `PRIMARY KEY`, `UNIQUE`, `NOT NULL` и `REFERENCES`. Модель — это чертёж; PostgreSQL хранит получившиеся таблицы, а не диаграмму.

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
<summary>Объясните the different types of dataBASE schema.</summary>

### Объясните the different types of dataBASE schema.

**Definition:**

В языке проектирования концептуальная схема называет сущности и связи, логическая — таблицы и ключи, физическая — индексы, секции и хранение. В PostgreSQL схема — это ещё и пространство имён внутри одной базы.

**How it works:**

`CREATE SCHEMA` создаёт пространство имён. Неквалифицированные имена разрешаются через `search_path`. Объекты в разных схемах могут носить одно имя. Права выдаются отдельно: `USAGE` на схему и нужное право на объект. `public` — схема по умолчанию в новой базе. Временные объекты живут в схеме сеанса.

**When to use:**

Отдельные схемы PostgreSQL нужны, чтобы сгруппировать объекты, изолировать приложения или дать каждому арендатору пространство имён, когда такого уровня изоляции достаточно. Концептуальный, логический и физический слои — это этапы проектирования, а не три команды `CREATE`.

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
<summary>Что такое Stored Procedures and how are they beneficial?</summary>

### Что такое Stored Procedures and how are they beneficial?

**Definition:**

Процедура — это подпрограмма, которую вызывают через `CALL`. `CREATE PROCEDURE` сохраняет её в базе. В отличие от функции, процедура не возвращает табличный результат как свой основной результат и может выполнять команды управления транзакцией, когда язык и настройки процедуры это позволяют.

**How it works:**

Аргументы бывают `IN`, `OUT`, `INOUT` или `VARIADIC`. Режим по умолчанию — `IN`. Тело может быть `LANGUAGE sql` или `LANGUAGE plpgsql`. `SECURITY INVOKER` (значение по умолчанию) выполняется от имени вызывающего. `SECURITY DEFINER` выполняется от имени владельца, но такая процедура не может выполнить `COMMIT` или `ROLLBACK`. `CREATE OR REPLACE PROCEDURE` сохраняет владельца и права.

**When to use:**

Процедура нужна для многооператорного действия, которое вы хотите вызывать по имени, особенно если оно должно фиксировать промежуточную работу. Функция нужна, когда подпрограмма должна появиться в списке `SELECT` или в предложении `WHERE`.

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
<summary>Что такое trigger in SQL and when should it be used?</summary>

### Что такое trigger in SQL and when should it be used?

**Definition:**

Триггер автоматически запускает функцию, когда на таблице или представлении происходит `INSERT`, `UPDATE`, `DELETE` или `TRUNCATE`.

**How it works:**

Момент — `BEFORE`, `AFTER` или `INSTEAD OF`. Гранулярность — `FOR EACH ROW` или `FOR EACH STATEMENT`. Функция триггера уровня строки может читать `NEW` и `OLD`. Команда — `EXECUTE { FUNCTION | PROCEDURE } function_name()`. Условие `WHEN` может пропустить вызов. Триггеры `BEFORE` могут изменить `NEW` или вернуть null, чтобы пропустить операцию над строкой.

**When to use:**

Триггер нужен для правила, которое должно держаться независимо от того, какой клиент пишет строку, например строки аудита или вычисляемого столбца, который ограничение `CHECK` посчитать не может. Объявленное ограничение предпочтительнее, когда оно прямо выражает правило. Триггеры легко не заметить, читая код приложения.

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
<summary>Опишите the concept of ACID in dataBASEs.</summary>

### Опишите the concept of ACID in dataBASEs.

**Definition:**

ACID называет четыре свойства транзакции: атомарность, согласованность, изолированность и долговечность.

**How it works:**

Атомарность означает, что `COMMIT` применяет каждое изменение транзакции, а `ROLLBACK` — ни одного. Согласованность означает, что объявленные ограничения выполнены в момент фиксации. Изолированность означает, что параллельные транзакции не видят незавершённую работу друг друга; PostgreSQL реализует это управлением параллельным доступом через многоверсионность и изоляцией снимка. Долговечность означает, что зафиксированная транзакция переживает сбой: PostgreSQL записывает изменение в журнал предзаписи до сообщения о фиксации. Изменения последовательности — исключение: `nextval` не откатывается.

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
<summary>Что такое dataBASE sharding?</summary>

### Что такое dataBASE sharding?

**Definition:**

Шардирование делит один логический набор данных между несколькими базами, так что каждый шард хранит подмножество строк. В PostgreSQL нет автоматического координатора шардов.

**How it works:**

Декларативное секционирование делит таблицу внутри одной базы по `RANGE`, `LIST` или `HASH`, и планировщик может отсекать секции. Внешняя секция или `postgres_fdw` может поместить часть данных на другой сервер PostgreSQL. Логическая репликация копирует изменения между серверами. Транзакции через шарды, глобальные уникальные ключи и рассеянные запросы — забота приложения или расширения; базовый SQL их не скрывает.

**When to use:**

Секционируйте внутри одного кластера, когда цель — управляемость или отсечение. Переходите на несколько серверов, когда предел — хранилище или скорость записи одной машины, и примите, что приложение само выбирает ключ шарда и маршрутизирует запросы.

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
<summary>Как dataBASE indexes work and what types are there?</summary>

### Как dataBASE indexes work and what types are there?

**Definition:**

Индекс — отдельная структура, которой планировщик может пользоваться, чтобы найти строки, не читая всю таблицу. `CREATE INDEX` строит B-tree, если вы не написали `USING`.

**How it works:**

Встроенные типы — B-tree, hash, GiST, SP-GiST, GIN и BRIN. Тип `bloom` приходит из расширения. B-tree поддерживает `<`, `<=`, `=`, `>=`, `>`, `BETWEEN`, `IN`, `IS NULL`, `IS NOT NULL` и некоторые префиксные шаблоны `LIKE`, а также может возвращать строки в порядке сортировки. Hash поддерживает только `=`. GiST и SP-GiST поддерживают классы операторов, например геометрический поиск и поиск ближайших соседей в `ORDER BY`. GIN — инвертированный индекс для составных значений, таких как массивы и `jsonb`. BRIN хранит сводки физических диапазонов блоков и помогает, когда значения столбца коррелируют с порядком таблицы.

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
<summary>Опишите the process of data warehousing.</summary>

### Опишите the process of data warehousing.

**Definition:**

Хранилище данных хранит интегрированные исторические данные для анализа. Путь загрузки — извлечение, преобразование и загрузка. PostgreSQL может держать это хранилище; это не отдельный продукт-склад.

**How it works:**

Извлекайте через `COPY` или внешнюю таблицу. Преобразуйте на SQL: приведения типов, `COALESCE`, соединения и агрегаты. Загружайте в таблицы фактов и измерений, часто секционированные по времени. Материализованное представление может хранить сводку, чтобы повторные отчёты не сканировали детали заново. `REFRESH MATERIALIZED VIEW` перестраивает его. Аналитические запросы затем агрегируют загруженные таблицы.

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
<summary>Объясните the difference between OLTP and OLAP systems.</summary>

### Объясните the difference between OLTP and OLAP systems.

**Definition:**

Системы OLTP обрабатывают много коротких транзакций, которые читают и пишут несколько строк. Системы OLAP сканируют и агрегируют большие исторические наборы для отчётов.

**How it works:**

PostgreSQL обслуживает обе формы в одном движке. OLTP опирается на индексы, первичные ключи, короткие транзакции и MVCC, чтобы читатели не блокировали писателей. OLAP опирается на последовательные сканирования, хеш-агрегаты или агрегаты по группам, секционирование, чтобы планировщик пропускал старые диапазоны, и материализованные представления для повторных сводок. Широкий `SELECT *` без предиката и точечный поиск хотят разные планы.

**When to use:**

Транзакционную нагрузку держите на нормализованных таблицах с индексами, которые совпадают с предикатами. Стройте сводки для отчётов, секции или отдельную базу, которую питает логическая репликация, когда аналитические сканирования стали бы конкурировать с этими короткими транзакциями.

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
<summary>Что такое materialized views and how do they differ from standard views?</summary>

### Что такое materialized views and how do they differ from standard views?

**Definition:**

Обычное представление хранит запрос и выполняет его при каждом обращении. Материализованное представление хранит результат запроса на диске.

**How it works:**

`CREATE MATERIALIZED VIEW name AS query` заполняет хранилище при создании. Позднейшие изменения базовых таблиц не появляются, пока не будет `REFRESH MATERIALIZED VIEW`. `REFRESH ... CONCURRENTLY` не блокирует читателей на всё обновление и требует уникальный индекс на материализованном представлении. Материализованное представление можно индексировать как таблицу. `WITH NO DATA` создаёт его пустым.

**When to use:**

Обычное представление нужно, когда результат должен быть текущим, а запрос дешёвый. Материализованное представление нужно, когда сводка дорогая и читатели могут принять данные на момент последнего обновления.


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
<summary>Как you identify and optimize SLOw-running queries?</summary>

### Как you identify and optimize SLOw-running queries?

**Definition:**

Медленный запрос — тот, у которого выделяются время выполнения или ввод-вывод. PostgreSQL записывает это в журнал и, с расширением, в `pg_stat_statements`.

**How it works:**

`log_min_duration_statement` пишет в журнал операторы, которые выполняются дольше порога. `pg_stat_statements` суммирует общее время, вызовы и строки по тексту запроса. `EXPLAIN (ANALYZE, BUFFERS)` выполняет оператор и показывает оценку против фактических строк и попаданий в буфер. Обычные исправления — недостающий индекс для избирательного предиката, свежая статистика от `ANALYZE`, переписывание, которое делает предикат пригодным для индекса, или меньший рабочий набор.

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
<summary>Что такое query execution plan in SQL?</summary>

### Что такое query execution plan in SQL?

**Definition:**

План выполнения — это дерево шагов, которые планировщик выбирает для оператора: сканирования, соединения, сортировки, агрегаты и то, как они передают строки.

**How it works:**

Планировщик оценивает варианты по статистике таблиц. Узел вроде `Seq Scan` читает кучу. `Index Scan` или `Index Only Scan` использует индекс. Узлы соединения включают вложенный цикл, хеш-соединение и соединение слиянием. Первая пара чисел стоимости — стартовая стоимость и полная стоимость в условных единицах. `EXPLAIN` печатает план, не выполняя его. Оценки строк, которые расходятся с реальностью, обычно означают отсутствующую или устаревшую статистику либо предикат, который планировщик не может оценить.

**Examples:**

```sql
EXPLAIN SELECT * FROM films WHERE kind = 'Comedy';
```

**Sources:**

- https://www.postgresql.org/docs/current/using-explain.html
- https://www.postgresql.org/docs/current/sql-explain.html
</details>

<details>
<summary>Объясните how to use EXPLAIN or EXPLAIN ANALYZE.</summary>

### Объясните how to use EXPLAIN or EXPLAIN ANALYZE.

**Definition:**

`EXPLAIN` показывает план, которым воспользовался бы планировщик. `EXPLAIN ANALYZE` выполняет оператор и добавляет фактические числа строк и время.

**How it works:**

Полезные параметры — `ANALYZE`, `BUFFERS`, `VERBOSE`, `SETTINGS` и `FORMAT` (`TEXT`, `XML`, `JSON` или `YAML`). `ANALYZE` действительно выполняет оператор, поэтому `EXPLAIN ANALYZE INSERT`, `UPDATE`, `DELETE` или `MERGE` изменяет данные. Оберните это в `BEGIN` и `ROLLBACK`, когда нужны измерения без сохранения изменения. `BUFFERS` показывает попадания и чтения общих блоков.

**When to use:**

Обычный `EXPLAIN` нужен, чтобы увидеть форму плана, не платя за выполнение, особенно для оператора, который нельзя запускать. `EXPLAIN (ANALYZE, BUFFERS)` нужен, когда надо сравнить оценённые и фактические строки.

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
<summary>Как indexing affect performance both positively and neGAtively?</summary>

### Как indexing affect performance both positively and neGAtively?

**Definition:**

Индекс может превратить избирательный поиск или упорядоченное сканирование в меньшее число чтений страниц. Он также стоит места и дополнительной работы на каждую запись, которая меняет индексированный столбец.

**How it works:**

Планировщик использует индекс, когда его оценка стоимости лучше последовательного сканирования. B-tree помогает равенству, диапазону и некоторой сортировке. Предикат с низкой избирательностью, например boolean, истинный для большинства строк, часто остаётся последовательным сканированием. Каждый лишний индекс нужно обновлять при `INSERT`, `UPDATE` и `DELETE`. `VACUUM` и раздувание относятся и к индексам, и к таблицам. Частичный индекс индексирует только строки, подходящие под предложение `WHERE`, поэтому он остаётся меньше, когда запросы всегда используют этот предикат.

**When to use:**

Добавляйте индекс, который совпадает с частым избирательным предикатом или требуемым порядком. Удаляйте индексы, которыми не пользуется ни один запрос. Проверяйте план через `EXPLAIN`, а не предполагайте, что у каждого внешнего ключа он уже есть.

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
<summary>Опишите how to measure the performance of SQL queries.</summary>

### Опишите how to measure the performance of SQL queries.

**Definition:**

Измеряйте запрос по фактическому времени, обработанным строкам и буферному вводу-выводу, а не по одному числу стоимости планировщика.

**How it works:**

`EXPLAIN (ANALYZE, BUFFERS)` сообщает время планирования, время выполнения, фактические строки и попадания в общий буфер против чтений. `track_io_timing` должен быть включён, прежде чем в эти времена войдёт время чтения блока. `pg_stat_statements` суммирует время по многим выполнениям одного оператора. Строка журнала от `log_min_duration_statement` записывает длительность одного выполнения. Запустите `ANALYZE`, чтобы оценки планировщика совпали с таблицей, прежде чем сравнивать планы.

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
<summary>Как бы вы rewrite a query to improve its performance?</summary>

### Как бы вы rewrite a query to improve its performance?

**Definition:**

Более быстрый запрос обычно даёт планировщику предикат, пригодный для индекса, меньше работы на строку и форму соединения или агрегата, которая совпадает с индексами и статистикой.

**How it works:**

Сравнивайте планы до и после через `EXPLAIN (ANALYZE, BUFFERS)`. Столбец внутри функции, как в `WHERE lower(name) = 'ann'`, блокирует обычный B-tree по `name`; индекс по выражению `lower(name)` или другой предикат это исправляет. Замените `NOT IN (subquery)`, который может содержать null. Выбирайте только нужные столбцы, чтобы могло примениться сканирование только по индексу. Большой `OFFSET` можно заменить предикатом по ключу. Коррелированный подзапрос, который повторяется на каждую внешнюю строку, можно заменить соединением или окном.

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
<summary>Что такое partitioned tables and how can they optimize performance?</summary>

### Что такое partitioned tables and how can they optimize performance?

**Definition:**

Секционированная таблица — это родитель, чьи строки хранятся в дочерних секциях. Декларативное секционирование PostgreSQL поддерживает `RANGE`, `LIST` и `HASH`.

**How it works:**

Родитель задаёт `PARTITION BY`. Каждая секция — это `CREATE TABLE ... PARTITION OF parent FOR VALUES ...`. Если планировщик может доказать, что секция не подходит запросу, отсечение секций её пропускает. Вставки направляются в подходящую секцию. Обновление ключа секции может переместить строку; это реализовано как удаление плюс вставка. Индексы создаются на секциях; уникальное ограничение, которое должно действовать на всю таблицу, обязано включать ключ секционирования.

**When to use:**

Секционируйте большие таблицы по столбцу, которым ограничивают запросы, часто по дате, чтобы сканирования и обслуживание вроде удаления старой секции оставались локальными. Не делите маленькую таблицу на множество крошечных детей: стоимость планирования может превысить сканирование, которое вы экономите.

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
<summary>Как you implement dataBASE encryption in SQL?</summary>

### Как you implement dataBASE encryption in SQL?

**Definition:**

PostgreSQL может шифровать клиентские соединения через SSL, хранить пароли через SCRAM и шифровать отдельные значения расширением `pgcrypto`. В базовом SQL нет команды прозрачного шифрования всего диска.

**How it works:**

Шифрование соединения — это настройка сервера (`ssl = on`) плюс правила узлов в `pg_hba.conf`, а не предложение столбца. `pgp_sym_encrypt(data, password)` возвращает `bytea`. `pgp_sym_decrypt` обращает это, когда пароль совпадает. Пароль не должен лежать в тексте запроса, который попадает в журналы. Шифрование диска и резервных копий — возможности операционной системы или инструмента копирования; вывод `pg_dump` — открытый текст, пока вы сами не зашифруете файл.

**When to use:**

SSL нужен для данных на проводе. `pgcrypto` нужен, когда конкретные значения столбца должны быть нечитаемы без ключа, который держит приложение. Шифрование файловой системы или тома нужно, когда угроза — украденные диски.

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
<summary>Что такое roles and how do they manage dataBASE access?</summary>

### Что такое roles and how do they manage dataBASE access?

**Definition:**

Роль — это пользователь базы или группа пользователей. `CREATE ROLE` её определяет. `GRANT` и `REVOKE` дают ей права. Отдельного объекта «пользователь» нет: роль с `LOGIN` может подключаться.

**How it works:**

К правам относятся `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `REFERENCES`, `TRIGGER` и `USAGE` на схемы, плюс права на уровне столбца. `GRANT role_a TO role_b` позволяет `role_b` наследовать права `role_a`, когда установлен `INHERIT`. Владелец таблицы обходит обычные проверки прав. `PUBLIC` — неявная группа всех ролей.

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
<summary>Объясните the concept of row-level security.</summary>

### Объясните the concept of row-level security.

**Definition:**

Безопасность на уровне строк добавляет политики, которые фильтруют, какие строки роль может видеть или писать. Она применяется только после того, как права на таблицу разрешили команду.

**How it works:**

`ALTER TABLE ... ENABLE ROW LEVEL SECURITY` включает её. `CREATE POLICY name ON table FOR command TO role USING (expression) WITH CHECK (expression)` задаёт фильтр. `USING` ограничивает существующие строки для `SELECT`, `UPDATE` и `DELETE`. `WITH CHECK` ограничивает строки, которые производят `INSERT` и `UPDATE`. Владелец таблицы обходит безопасность строк, если не сделать `FORCE ROW LEVEL SECURITY`. Суперпользователи и роли с `BYPASSRLS` тоже её обходят. Если не подходит ни одна политика, по умолчанию доступ запрещён.

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
<summary>Опишите how to create and use user-defined functions (UDFS).</summary>

### Опишите how to create and use user-defined functions (UDFS).

**Definition:**

Пользовательская функция создаётся через `CREATE FUNCTION` и может вызываться из выражений SQL. Язык может быть `sql`, `plpgsql` или другим установленным языком.

**How it works:**

Вы объявляете типы аргументов и тип `RETURNS`. Функции `LANGUAGE SQL` могут использовать строковое тело или `BEGIN ATOMIC ... END`. Функция вызывается как `name(arguments)` в списке выборки, в `WHERE` или в другом выражении. `SECURITY INVOKER` — значение по умолчанию. `SECURITY DEFINER` выполняется с правами владельца; задайте этой функции безопасный `search_path`, чтобы неквалифицированное имя нельзя было подменить. Вызывающим нужно право `EXECUTE`.

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
<summary>Опишите scalar-valued and table-valued functions.</summary>

### Опишите scalar-valued and table-valued functions.

**Definition:**

Скалярная функция возвращает одно значение на вызов. Функция, возвращающая набор, возвращает множество строк. В PostgreSQL вторая форма записывается как `RETURNS SETOF` или `RETURNS TABLE`.

**How it works:**

`RETURNS integer` — скаляр. `RETURNS TABLE (f1 int, f2 text)` — набор составных строк, его можно использовать в `FROM`. `RETURNS SETOF some_type` — та же идея для существующего типа. Скаляр вызывают в выражении. Функцию, возвращающую набор, в списке выборки писать можно, но яснее в `FROM`, включая `LATERAL`, когда она зависит от текущей внешней строки.

**When to use:**

Возвращайте скаляр, когда вызывающему нужно одно значение, например посчитанный налог. Возвращайте таблицу, когда вызывающему нужно отношение, которое можно соединить.

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
<summary>Как бы вы define a stored procedure with input and output parameters?</summary>

### Как бы вы define a stored procedure with input and output parameters?

**Definition:**

Аргументы процедуры имеют режим: `IN`, `OUT`, `INOUT` или `VARIADIC`. Пропущенный режим означает `IN`. Выходные аргументы присваиваются внутри процедуры и возвращаются в `CALL`.

**How it works:**

`IN` передаёт значение внутрь. `OUT` записывает процедура. `INOUT` делает и то и другое. `CALL` передаёт заполнитель для каждого аргумента `OUT`. Процедуры не используются как выражения в `SELECT`. Тело `LANGUAGE plpgsql` присваивает выходному имени как переменной.

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
<summary>Что такое the difference between a function and a stored procedure?</summary>

### Что такое the difference between a function and a stored procedure?

**Definition:**

Функция возвращает значение и вызывается внутри выражения. Процедура вызывается через `CALL` и не возвращает набор результатов как свой тип возврата.

**How it works:**

Функции могут появляться в `SELECT`, `WHERE` и значениях по умолчанию. Процедуры — нет. Процедура может выполнить `COMMIT` или `ROLLBACK` внутри тела, когда она не `SECURITY DEFINER`, у неё нет предложения `SET`, которое запрещает управление транзакцией, и сам `CALL` ещё не находится внутри блока транзакции. Функция не может зафиксировать окружающую транзакцию. Обе поддерживают аргументы `IN`, `OUT` и `INOUT`, а также `SECURITY INVOKER` или `SECURITY DEFINER`.

**When to use:**

Выбирайте функцию для вычисления, которое используется в запросах. Выбирайте процедуру для команды, которая делает несколько изменений данных и может нуждаться в собственном управлении транзакцией.

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
<summary>Как you use the CAST and CONVERT functions?</summary>

### Как you use the CAST and CONVERT functions?

**Definition:**

`CAST(expression AS type)` преобразует значение в другой тип данных. Запись PostgreSQL `expression::type` делает то же преобразование. `convert` типы не приводит: он преобразует `bytea` из одной кодировки символов в другую.

**How it works:**

`CAST('100' AS integer)` и `'100'::integer` оба дают целое 100. Неверное представление вызывает ошибку. Приведения при присваивании и явные приведения следуют разным правилам; слишком длинная строка даёт ошибку при присваивании в `varchar(n)`, но усекается явным приведением. `convert(bytes, src_encoding, dest_encoding)` возвращает `bytea`. `convert_from` и `convert_to` преобразуют между `bytea` и `text` в названной кодировке.

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
<summary>Что такое dataBASE transaction?</summary>

### Что такое dataBASE transaction?

**Definition:**

Транзакция — это группа команд SQL, которые фиксируются или откатываются как одно целое. PostgreSQL выполняет каждый оператор в собственной транзакции, если вы не открыли её явно.

**How it works:**

`BEGIN` или `START TRANSACTION` открывает блок транзакции. `COMMIT` делает её изменения долговечными и видимыми. `ROLLBACK` их отбрасывает. Ошибка оператора прерывает текущую транзакцию; дальнейшие команды падают, пока не будет `ROLLBACK` или `ROLLBACK TO` к точке сохранения. Закрытие сеанса без фиксации откатывает транзакцию.

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
<summary>Объясните the concept of locking and its types in SQL dataBASEs.</summary>

### Объясните the concept of locking and its types in SQL dataBASEs.

**Definition:**

Блокировки согласовывают параллельный доступ. PostgreSQL берёт их автоматически и также даёт явные блокировки строк и таблиц.

**How it works:**

MVCC позволяет обычному `SELECT` читать снимок, не блокируя писателей. `SELECT ... FOR UPDATE` берёт блокировку строки, которая конфликтует с обновлениями и удалениями. `FOR NO KEY UPDATE` слабее, когда обновление не трогает ключевые столбцы. `FOR SHARE` и `FOR KEY SHARE` — разделяемые блокировки. Режимы уровня таблицы идут от `ACCESS SHARE`, который берёт `SELECT`, до `ACCESS EXCLUSIVE`, который берут `DROP TABLE`, `TRUNCATE` и некоторые формы `ALTER TABLE` и `VACUUM FULL`. `LOCK TABLE` явно запрашивает блокировку таблицы. Блокировки держатся до конца транзакции.

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
<summary>Что такое the properties of transactions?</summary>

### Что такое the properties of transactions?

**Definition:**

Свойства — это атомарность, согласованность, изолированность и долговечность.

**How it works:**

Атомарность — это `COMMIT` против `ROLLBACK`, включая точки сохранения. Согласованность — набор ограничений, проверяемых для транзакции: `NOT NULL`, `CHECK`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY` и ограничения-исключения. Изолированность — поведение снимка и блокировок выбранного уровня изоляции; по умолчанию это Read Committed. Долговечность — сброс журнала предзаписи при фиксации. `nextval` последовательности откат не отменяет.

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
<summary>Как you manage transaction isolation levels?</summary>

### Как you manage transaction isolation levels?

**Definition:**

Уровень изоляции управляет тем, какие параллельные изменения может наблюдать транзакция. PostgreSQL реализует Read Committed, Repeatable Read и Serializable. Запрос Read Uncommitted ведёт себя как Read Committed.

**How it works:**

Read Committed — уровень по умолчанию. Каждый оператор видит строки, зафиксированные до его начала. Repeatable Read держит один снимок на всю транзакцию, и на этом уровне PostgreSQL также не допускает фантомного чтения. Serializable добавляет обнаружение аномалий сериализации и может прервать транзакцию с SQLSTATE `40001`. Уровень задаётся через `SET TRANSACTION ISOLATION LEVEL` после `BEGIN` или через `SET SESSION CHARACTERISTICS` для последующих транзакций. Приложения на `SERIALIZABLE` должны быть готовы повторить попытку.

**When to use:**

Оставайтесь на Read Committed для коротких операторов, каждый из которых трогает известные строки. Repeatable Read нужен, когда каждый оператор транзакции должен видеть один и тот же снимок. Serializable нужен, когда результат должен совпасть с каким-то поочерёдным порядком параллельных транзакций.

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
<summary>Что означает it mean to commit or roll back a transaction?</summary>

### Что означает it mean to commit or roll back a transaction?

**Definition:**

`COMMIT` заканчивает транзакцию и делает её изменения долговечными и видимыми другим транзакциям. `ROLLBACK` заканчивает её и отбрасывает эти изменения.

**How it works:**

После `COMMIT` сервер сбросил запись о фиксации в журнал предзаписи согласно `synchronous_commit`. После `ROLLBACK` всё выглядит так, будто изменений не было, кроме продвижения последовательностей. `SAVEPOINT name` отмечает точку внутри транзакции. `ROLLBACK TO SAVEPOINT name` отменяет только работу после этой точки, и транзакция остаётся открытой. `RELEASE SAVEPOINT` снимает метку, не отменяя работу.


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
<summary>Как SQL be integrated with big data technologies?</summary>

### Как SQL be integrated with big data technologies?

**Definition:**

PostgreSQL стыкуется с внешними данными через обёртки сторонних данных и импорт файлов, а SQL остаётся языком запросов на стороне PostgreSQL.

**How it works:**

`postgres_fdw` запрашивает другой сервер PostgreSQL как внешнюю таблицу. `file_fdw` читает файл на стороне сервера, например CSV, как внешнюю таблицу. `COPY` загружает или выгружает файл одной командой. Обёртка проталкивает часть фильтров на удалённую сторону, когда может. Внешнее сканирование всё же не распределённый движок запросов: соединения огромных удалённых множеств могут вытянуть строки на локальный сервер.

**When to use:**

Внешняя таблица нужна, когда удалённые данные должны выглядеть как таблица в SQL. `COPY` нужен, когда партия загружается один раз. Тяжёлую повторную аналитику держите в локальных секционированных таблицах или в материализованном представлении после загрузки.

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
<summary>Обсудите the interoperability of SQL with cloud-BASEd data stores.</summary>

### Обсудите the interoperability of SQL with cloud-BASEd data stores.

**Definition:**

PostgreSQL говорит с другим экземпляром PostgreSQL, в том числе размещённым у облачного провайдера, через внешние таблицы и логическую репликацию. Он не встраивает SQL-диалект каждого поставщика.

**How it works:**

`postgres_fdw` отображает удалённую таблицу в локальную базу. Локальной роли нужно сопоставление пользователя для удалённого соединения. Логическая репликация публикует изменения с одного сервера и подписывается на другом; это обычный путь на облачную реплику или с неё. `COPY` перемещает файлы через потоки клиента или сервера. Запросы, которые вы посылаете, по-прежнему используют синтаксис PostgreSQL.

**When to use:**

`postgres_fdw` нужен для редкого SQL к удалённой базе PostgreSQL. Логическая репликация нужна, когда удалённая сторона должна хранить собственную копию. Не считайте, что внешняя таблица даёт ту же задержку, что локальная.

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
<summary>Что такое Data Lake and how can SQL interact with it?</summary>

### Что такое Data Lake and how can SQL interact with it?

**Definition:**

Озеро данных — это хранилище файлов, часто CSV, JSON или Parquet, которое лежит вне кучи таблицы. PostgreSQL читает эти файлы через `COPY` или обёртку сторонних данных; озером он не управляет.

**How it works:**

`COPY table FROM 'filename' WITH (FORMAT csv, HEADER true)` загружает файл на стороне сервера в таблицу. Пользователь операционной системы PostgreSQL должен уметь читать этот путь, а роли базы нужны права, которых требует `COPY`. `file_fdw` оставляет файл на месте и сканирует его как внешнюю таблицу. После загрузки обычный SQL работает по локальной таблице.

**When to use:**

Запрашивайте внешний файл, когда нужно разовое сканирование. Загружайте в настоящую таблицу, с типами и индексами, когда одни и те же данные будут запрашивать часто.

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
<summary>Объясните the interaction between SQL and NoSQL within the same application.</summary>

### Объясните the interaction between SQL and NoSQL within the same application.

**Definition:**

PostgreSQL хранит реляционные строки и JSON в форме документа в одной базе. `jsonb` — двоичный тип JSON, предназначенный для запросов.

**How it works:**

Столбец `jsonb` хранит объект или массив. `->` возвращает значение JSON, `->>` возвращает текст, а `@>` проверяет вложение. Индексы по выражению и индексы GIN поддерживают операторы `jsonb`. Ограничения и внешние ключи по-прежнему применяются к реляционным столбцам рядом с документом. JSON не заменяет внешний ключ: вложенный идентификатор не проверяется, пока вы не смоделируете его столбцом и ограничением.

**When to use:**

Столбцы и ключи нужны для данных, которые вы фильтруете, соединяете и ограничиваете. `jsonb` нужен для полезной нагрузки, форма которой меняется и которая не является ключом соединения.

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
<summary>Как SQL work within a microservices architecture?</summary>

### Как SQL work within a microservices architecture?

**Definition:**

Обычно каждый сервис владеет своими данными. PostgreSQL может это обеспечить отдельными базами, схемами и ролями. Тогда SQL выполняется внутри границы одного сервиса.

**How it works:**

Роль, которой выданы права только на свою схему, не может читать таблицы другого сервиса. Чтения между сервисами — это не внешние ключи между сервисами; если вы всё же запрашиваете другую базу, `postgres_fdw` делает эту зависимость явной. Логическая репликация может питать отчётную базу, не позволяя сервисам писать в таблицы друг друга. Транзакции не пересекают сервисы: транзакция PostgreSQL покрывает одно соединение с базой.

**When to use:**

Дайте сервису собственную роль и схему или базу и держите соединения внутри этой границы. Внешнюю таблицу добавляйте только когда синхронное удалённое чтение — принятая зависимость.

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
<summary>Что такое some common SQL coding practices you follow?</summary>

### Что такое some common SQL coding practices you follow?

**Definition:**

Практичный стиль PostgreSQL — явные столбцы, ограничения в базе, параметры вместо склеенных литералов и `ORDER BY` всякий раз, когда порядок важен.

**How it works:**

Называйте каждый целевой столбец в `INSERT` и каждый выходной столбец в `SELECT`. Объявляйте `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL` и `CHECK`, чтобы недопустимые строки отвергала сама база. Передавайте значения параметрами; литерал, собранный склейкой строк, — это путь к SQL-инъекции. Пишите `ORDER BY`, потому что у запроса без него нет гарантированного порядка. Предпочитайте `text` или `varchar` дополненному пробелами `char(n)`. Запускайте `EXPLAIN` по запросу, прежде чем добавлять под него индекс.

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
<summary>Как you ensure the portability of SQL scripts across different dataBASE systems?</summary>

### Как you ensure the portability of SQL scripts across different dataBASE systems?

**Definition:**

Переносимый SQL остаётся внутри конструкций, которые эти системы разделяют. Расширениям PostgreSQL тогда нужна осознанная замена на другой системе.

**How it works:**

`CAST(x AS type)` стандартен; `x::type` — это PostgreSQL. `OFFSET ... FETCH FIRST n ROWS ONLY` стандартен; `LIMIT` широко реализован, но это не исходная стандартная запись. `RETURNING`, `DISTINCT ON`, `ILIKE` и `FROM` у `UPDATE` — расширения PostgreSQL. `convert` в PostgreSQL — функция кодировки, а не приведение типа. Столбцы-идентификаторы используют `GENERATED AS IDENTITY`. Кавычки идентификаторов двойные, а строк — одинарные, как в стандарте.

**When to use:**

Пишите стандартный синтаксис, когда скрипт должен выполняться больше чем на одном продукте. Расширение PostgreSQL берите, когда вы остаётесь на PostgreSQL и стандартная форма не может выразить операцию.

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
<summary>Какие методы do you use for version controlling SQL scripts?</summary>

### Какие методы do you use for version controlling SQL scripts?

**Definition:**

Изменения схемы живут как упорядоченные SQL-скрипты в репозитории приложения. Сервер не ведёт историю контроля версий этих скриптов.

**How it works:**

Каждая миграция — файл операторов `CREATE`, `ALTER` и `COMMENT`, применяемых по порядку. `pg_dump --schema-only` пишет текущую схему как SQL: это снимок, который можно сравнить, а не замена истории миграций. `pg_dump` данных — это резервная копия. Просматривайте скрипт так же, как код приложения, и применяйте его в транзакции, когда каждый оператор в нём может выполняться внутри одной.

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
<summary>Что такое the benefits of using stored procedures instead of embedding SQL queries in code?</summary>

### Что такое the benefits of using stored procedures instead of embedding SQL queries in code?

**Definition:**

Процедура или функция держит многооператорное действие в базе и открывает его как `CALL` или вызов функции. Приложению тогда нужно `EXECUTE` на эту подпрограмму, а не широкие права на таблицы.

**How it works:**

`GRANT EXECUTE ON PROCEDURE name(argument_types) TO role` разрешает вызов. `SECURITY DEFINER` выполняется от имени владельца, поэтому вызывающему не нужны права на таблицы; это также значит, что владелец должен зафиксировать `search_path`. Подпрограмма — одно определение для каждого клиента. Процедура может управлять транзакциями. Цена в том, что SQL больше не виден рядом с кодом приложения и привязан к синтаксису PostgreSQL.

**When to use:**

Подпрограмму берите, когда несколько клиентов должны делить один проверяемый путь записи или когда у вызывающего не должно быть прямых прав на таблицы. Обычные однооператорные запросы оставляйте в приложении, по-прежнему как параметризованный SQL.

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
<summary>Как you document SQL code effectively?</summary>

### Как you document SQL code effectively?

**Definition:**

`COMMENT ON` хранит комментарий к объекту базы. PostgreSQL держит этот текст в системных каталогах и показывает его в описаниях `\d+`.

**How it works:**

Комментировать можно таблицу, столбец, функцию, процедуру, представление или другой объект. `COMMENT ON COLUMN table.column IS 'text'` задаёт комментарий. `COMMENT ON ... IS NULL` его снимает. Комментарий может задать только владелец или суперпользователь. Комментарии описывают намерение, которого тип и ограничение ещё не выражают. В файлах миграций должны быть операторы `COMMENT`, чтобы восстановленная база их сохранила.

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
<summary>Как бы вы find the Nth highest salary from a table?</summary>

### Как бы вы find the Nth highest salary from a table?

**Definition:**

N-я наибольшая различная зарплата — это строка, чей плотный ранг равен N, когда зарплаты упорядочены по убыванию.

**How it works:**

`dense_rank() OVER (ORDER BY salary DESC)` назначает один ранг совпадениям и не пропускает номера. Фильтруйте `rk = N` во внешнем запросе, потому что оконные функции в `WHERE` недопустимы. `ORDER BY salary DESC OFFSET N - 1 LIMIT 1` возвращает строку, но при совпадениях «N-я строка» — не то же самое, что «N-я различная зарплата».

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
<summary>Как you count the number of occurrences of a specific value in a column?</summary>

### Как you count the number of occurrences of a specific value in a column?

**Definition:**

`count(*) FILTER (WHERE column = value)` считает строки, где сравнение истинно. `count(*)` с предложением `WHERE` делает то же для одного итога.

**How it works:**

Обычный `count(column)` считает ненулевые значения и сам по себе не ограничивает, какое это значение. Предложение `FILTER` применяется только к этому агрегату, поэтому один запрос может посчитать несколько значений. `sum(CASE WHEN column = value THEN 1 ELSE 0 END)` — эквивалентный условный агрегат.

**Examples:**

```sql
SELECT count(*) FILTER (WHERE status = 'open') AS open_count
FROM tickets;
```

**Sources:**

- https://www.postgresql.org/docs/current/functions-aggregate.html
</details>

<details>
<summary>Как you calculate running totals in SQL?</summary>

### Как you calculate running totals in SQL?

**Definition:**

Нарастающий итог — это оконный `sum`, упорядоченный по столбцу последовательности. Каждая выходная строка сохраняет свои значения и добавляет сумму на данный момент.

**How it works:**

`sum(amount) OVER (ORDER BY ordered_at)` использует рамку от начала раздела через текущую строку и её совпадения. Добавьте `PARTITION BY account_id`, чтобы итог начинался заново для каждого счёта. Укажите `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`, когда совпадения не должны делить один итог. Окно не уменьшает число строк.

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
<summary>Объясните how to reverse the contents of a column without using a reverse function.</summary>

### Объясните how to reverse the contents of a column without using a reverse function.

**Definition:**

PostgreSQL даёт `reverse(text)`, которая возвращает символы в обратном порядке. Тот же результат можно собрать рекурсивным CTE и `substring`, если задание запрещает эту функцию.

**How it works:**

Якорь выбирает исходную строку и пустой накопитель. Каждый шаг переносит первый символ в начало накопителя и оставляет остаток через `substring(rest FROM 2)`. Шаг останавливается, когда остаток пуст. Готовая строка — та, у которой остаток пуст.

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
<summary>Какой approach вы используете для создания calendar table и для чего она нужна?</summary>

### Какой approach вы используете для создания calendar table и для чего она нужна?

**Definition:**

Таблица календаря — одна строка на день. Её порождают через `generate_series` и сохраняют, чтобы отчёты могли сделать внешнее соединение и сохранить дни, в которых нет фактов.

**How it works:**

`generate_series` может вернуть набор целых. Прибавление каждого целого к начальной `date` использует оператор `date + integer` и даёт один день. Перегрузка `generate_series` для timestamp вместо этого принимает шаг `interval`. Дополнительные столбцы, например месяц или признак выходного, — выражения от этой даты. После сохранения `LEFT JOIN` от календаря к фактам по дате показывает пробелы как пустые меры, которые `COALESCE` может превратить в ноль.

**When to use:**

Храните таблицу, когда многим отчётам нужен один и тот же каркас дат. Вызывайте `generate_series` прямо в запросе, когда диапазон мал и используется один раз.

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
<summary>Что такое the process of Extract, Transform, Load (ETL)?</summary>

### Что такое the process of Extract, Transform, Load (ETL)?

**Definition:**

ETL переносит данные из источника в целевую таблицу. Извлечение читает источник, преобразование меняет форму и очищает данные, загрузка записывает результат.

**How it works:**

В PostgreSQL извлечение — это `COPY ... FROM`, внешняя таблица или `SELECT` из другой таблицы. Преобразование — SQL между ними: приведения, `trim`, `COALESCE`, соединения и агрегаты, часто с посадкой в промежуточную таблицу. Загрузка — `INSERT INTO target SELECT ... FROM stage` или `COPY` в назначение. Ограничения цели отвергают строки, которые преобразование пропустило.

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
<summary>Как you import/export data from/to a flat file using SQL?</summary>

### Как you import/export data from/to a flat file using SQL?

**Definition:**

`COPY` перемещает строки между таблицей и файлом или между запросом и файлом. Форматы включают `text`, `csv` и `binary`.

**How it works:**

`COPY table FROM 'filename'` читает файл на сервере базы. `COPY table TO 'filename'` его пишет. `COPY (SELECT ...) TO` выгружает запрос. Среди параметров — `FORMAT csv`, `HEADER`, `DELIMITER`, `NULL` и `ENCODING`. Файловый `COPY` на стороне сервера ограничен суперпользователями и ролями, которым выдано право его выполнять, потому что он читает и пишет от имени пользователя операционной системы сервера. `\copy` в psql идёт потоком через клиент и использует файловую систему клиента.

**Examples:**

```sql
COPY orders (order_id, amount)
FROM STDIN WITH (FORMAT csv, HEADER true);
```

**Sources:**

- https://www.postgresql.org/docs/current/sql-copy.html
</details>

<details>
<summary>Объясните the steps for a basic ETL process in a data warehousing environment.</summary>

### Объясните the steps for a basic ETL process in a data warehousing environment.

**Definition:**

Базовая загрузка хранилища кладёт извлечение на промежуточный слой, проверяет и меняет форму, затем вставляет агрегаты или факты в таблицы хранилища.

**How it works:**

Создайте промежуточную таблицу без окончательных ограничений хранилища, если нужно осмотреть плохие строки. `COPY` загружает файл в промежуточную таблицу. Преобразуйте через `INSERT ... SELECT` с приведениями, `NULLIF` для сторожевых значений и соединениями к ключам измерений. Загрузите таблицу фактов внутри транзакции. Обновите материализованное представление, которое читают отчёты. `TRUNCATE` промежуточной таблицы делайте после фиксации загрузки либо оставьте её для аудита.

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
<summary>Как you cleanse and format data using SQL queries?</summary>

### Как you cleanse and format data using SQL queries?

**Definition:**

Очистка переписывает исходные значения в типы и домены, которые принимает назначение. Функции форматирования меняют регистр, пробелы и маркеры null.

**How it works:**

`trim` и `btrim` снимают начальные и концевые символы. `lower` и `upper` меняют регистр. `NULLIF(code, '')` превращает пустую строку в null, чтобы она не притворялась значением. `COALESCE` подставляет умолчание. `CAST` применяет тип назначения и роняет строку, когда текст неверен. Ограничение `CHECK` на назначении удерживает очищенное правило после загрузки. `DISTINCT` или `GROUP BY` схлопывает точные дубликаты извлечения.

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
<summary>Какие инструменты do you use for automating data import/export routines?</summary>

### Какие инструменты do you use for automating data import/export routines?

**Definition:**

Собственные инструменты PostgreSQL для повторяемого импорта и экспорта — `COPY`, `pg_dump` и `pg_restore`. Расписание для них лежит вне языка SQL.

**How it works:**

Скрипт запускает `psql -f load.sql` или `pg_dump` по таймеру операционной системы. `pg_dump` пишет согласованный снимок одной базы в скрипт или нестандартный архив. `pg_restore` загружает нестандартный архив. `COPY` — путь плоского файла на уровне строк внутри этого скрипта. `pg_dump` не выгружает конфигурацию сервера и выгружает каждую базу отдельно.

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
<summary>Как бы вы model a many-to-many relationship in SQL?</summary>

### Как бы вы model a many-to-many relationship in SQL?

**Definition:**

Связь «многие ко многим» — это таблица соединения, строки которой спаривают ключ с каждой стороны. Каждая пара — одна строка. Два внешних ключа вместе составляют первичный ключ, если самой паре не нужен собственный идентификатор.

**How it works:**

Каждый внешний ключ ссылается на одного родителя. `ON DELETE CASCADE` удаляет пары, когда родитель исчезает; не указывайте его, когда пара должна блокировать удаление. Запрашивайте оба направления двумя соединениями через таблицу соединения.

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
<summary>Опишите how to manage hierarchical data in SQL.</summary>

### Опишите how to manage hierarchical data in SQL.

**Definition:**

Список смежности хранит идентификатор родителя каждой строки. Рекурсивное CTE идёт от начального узла к потомкам или предкам.

**How it works:**

Якорь выбирает корень. Рекурсивная часть соединяет таблицу с CTE по `parent_id`. `UNION ALL` сохраняет каждый шаг. Столбец глубины или массив пути останавливает циклы: остановитесь, когда идентификатор уже есть в пути. Расширение `ltree` — альтернативный тип для путей меток; рекурсивному запросу оно не требуется.

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
<summary>Как бы вы approach writing SQL queries for a reporting application?</summary>

### Как бы вы approach writing SQL queries for a reporting application?

**Definition:**

Запрос для отчёта агрегирует устойчивое зерно, оставляет отсутствующие группы видимыми и имеет явный порядок. Его часто поддерживает материализованное представление, когда одну и ту же сводку читают часто.

**How it works:**

Начните с зерна отчёта, например дня и региона. `GROUP BY` по этим столбцам. `LEFT JOIN` от календаря или измерения, чтобы пустые группы остались, и `COALESCE` меры в ноль. Оконные функции добавляют доли итога без второго схлопывания. `ORDER BY` совпадает с раскладкой отчёта. Если `EXPLAIN ANALYZE` показывает повторный дорогой агрегат, сохраните его как материализованное представление и обновляйте по расписанию.

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
<summary>Объясните how to handle temporal data and time zones in SQL.</summary>

### Объясните how to handle temporal data and time zones in SQL.

**Definition:**

`timestamp with time zone` (`timestamptz`) хранит момент времени. `timestamp without time zone` хранит показание часов без пояса. PostgreSQL не хранит исходное имя пояса внутри значения `timestamptz`.

**How it works:**

Ввод, который включает пояс, преобразуется в UTC для хранения. Показ использует параметр сеанса `TimeZone`. `AT TIME ZONE` преобразует между `timestamptz` и `timestamp`: `timestamptz AT TIME ZONE zone` даёт `timestamp without time zone` в этом поясе. `date_trunc('day', ts)` для `timestamptz` усекает в поясе сеанса. `now()` и `CURRENT_TIMESTAMP` возвращают `timestamptz`.

**When to use:**

Моменты, например когда платёж был зафиксирован, храните как `timestamptz`. Гражданскую дату или настенное время, которое не должно сдвигаться, например местную дату открытия магазина, храните как `date` или `timestamp without time zone`.

**Examples:**

```sql
SELECT ordered_at AT TIME ZONE 'Europe/Warsaw' AS warsaw_local
FROM orders;
```

**Sources:**

- https://www.postgresql.org/docs/current/datatype-datetime.html
</details>

<details>
<summary>Как you use SQL in financial applications for risk and portfolio analysis?</summary>

### Как you use SQL in financial applications for risk and portfolio analysis?

**Definition:**

Портфельная арифметика в PostgreSQL использует `numeric` для величин, транзакции для обновлений, которые должны остаться согласованными, и агрегаты или окна для экспозиции.

**How it works:**

`numeric` точный; `real` и `double precision` для денег не подходят. Перевод — это два обновления в одной транзакции. Стоимость позиции — `sum(quantity * price)`. Окно может посчитать нарастающий нетто или долю книги: `sum(market_value) OVER (PARTITION BY portfolio_id)`. Ограничения держат домены количеств явными. SQL может агрегировать экспозиции; подгонка статистической модели не является встроенной командой SQL.

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
<summary>Какие шаги вы предпринимаете для troubleshooting failed SQL query?</summary>

### Какие шаги вы предпринимаете для troubleshooting failed SQL query?

**Definition:**

Неудавшийся оператор возвращает ошибку с кодом SQLSTATE и основным сообщением. Класс кода определяет вид сбоя.

**How it works:**

Прочитайте сообщение и строку `ERROR`, прежде чем менять запрос. Синтаксические ошибки указывают на позицию. Сбои ограничений называют ограничение (`23505` — нарушение уникальности, `23503` — внешний ключ, `23502` — not null, `23514` — check). Сбои прав — `42501`. Ошибка оператора внутри транзакции прерывает эту транзакцию до `ROLLBACK`. Если оператор верен, но медленный, это задача плана для `EXPLAIN`, а не сбой. Подробности сервера сверх сообщения клиента лежат в журнале PostgreSQL.

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
<summary>Как you recover data from a corrupt SQL dataBASE?</summary>

### Как you recover data from a corrupt SQL dataBASE?

**Definition:**

Восстановление использует резервную копию и журнал предзаписи. Повреждённая куча или индекс обычным оператором SQL не чинится.

**How it works:**

`pg_dump` снимает логическую резервную копию базы, которую ещё можно прочитать. Непрерывное архивирование плюс базовая копия дают восстановление на момент времени: восстановите базовую копию и воспроизведите WAL до выбранного времени. Если сервер не может прочитать страницу, прекратите запись и восстановитесь из этой копии. Переиндексация может пересобрать повреждённый индекс, когда куча цела: `REINDEX INDEX name`. Догадки об удалённых файлах или сброс WAL — крайняя мера, описанная для специалистов, и она может потерять зафиксированные данные.

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
<summary>Какие методы do you employ to ensure data integrity?</summary>

### Какие методы do you employ to ensure data integrity?

**Definition:**

Целостность обеспечивается ограничениями, транзакциями и правами, чтобы недопустимые данные нельзя было зафиксировать.

**How it works:**

`PRIMARY KEY` и `UNIQUE` останавливают повторяющиеся ключи. `FOREIGN KEY` требует строку, на которую ссылаются. `NOT NULL` и `CHECK` ограничивают столбец. Ограничение-исключение останавливает перекрывающиеся диапазоны. Ограничения `DEFERRABLE` могут подождать до фиксации. Транзакция держит изменения нескольких строк атомарными. Безопасность строк и выданные права ограничивают, кто может писать. Ограничения проверяет сервер для каждого клиента.

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
<summary>Как you decipher and resolve deadlocks in SQL?</summary>

### Как you decipher and resolve deadlocks in SQL?

**Definition:**

Взаимная блокировка — это цикл транзакций, которые ждут блокировок друг друга. PostgreSQL обнаруживает цикл, прерывает одну транзакцию и возвращает SQLSTATE `40P01`.

**How it works:**

Подробности ошибки перечисляют процессы и блокировки. `log_lock_waits` и `deadlock_timeout` управляют связанной записью ожиданий в журнал. Прерванную транзакцию должно повторить приложение. Обычная профилактика — блокировать строки в одном и том же порядке в каждой транзакции и держать транзакции короткими. `SELECT ... FOR UPDATE` делает этот порядок явным. `pg_locks` в соединении с `pg_stat_activity` показывает ожидающих, пока они ещё заблокированы.

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
<summary>Объясните how to use SQL for predictive analysis and machine learning puRPOses.</summary>

### Объясните how to use SQL for predictive analysis and machine learning puRPOses.

**Definition:**

Базовый PostgreSQL не обучает и не хранит модель машинного обучения. SQL готовит признаки и метки и может применить простую числовую формулу, которую вы напишете сами.

**How it works:**

Агрегаты и окна строят обучающую таблицу: количества, суммы, лаги и отношения на сущность. `COPY` выгружает этот результат в инструмент, который подгоняет модель. Столбец с оценкой можно загрузить обратно и присоединить. Формула, написанная вручную, например взвешенная сумма в `SELECT`, — обычная арифметика, а не подогнанная модель. В базовом SQL нет оператора `CREATE MODEL`.

**When to use:**

SQL нужен, чтобы задать воспроизводимый запрос признаков и отдавать оценки, которые уже записаны в таблицу. Подгонку делайте в системе, которая создана обучать модели, а затем верните оценки или параметры, если приложению они нужны в базе.


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
