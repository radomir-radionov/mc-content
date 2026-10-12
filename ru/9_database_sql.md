# Database — SQL

PostgreSQL, SQL Server (T-SQL), соединения, индексы и планы выполнения.

[ ← Вернуться в хаб](./9_database.md)

### Related docs

- [БД — Основы](./9_database_core.md) · [БД — SQL](./9_database_sql.md) · [БД — NoSQL](./9_database_nosql.md) · [БД — ORM](./9_database_orm.md) · [БД — Производство](./9_database_production.md) · [БД — Интервью](./9_database_interview.md) · [Веб-безопасность](./7_special_security.md)

### Main concepts

| #     | Темы                                                                                                                     |
| ----- | ------------------------------------------------------------------------------------------------------------------------ |
| 1–10  | SQL, DDL/DML/DCL, первичный ключ, внешний ключ, нормализация, соединения, подзапросы, DISTINCT, WHERE vs HAVING, индексы |
| 11–20 | ACID, триггеры, VIEW, LIKE, агрегаты, UNION, процедуры, EXPLAIN (Postgres + SQL Server), ограничения, SQL-инъекция       |
| 21–30 | Реляционная БД, INNER и OUTER, NULL, CASE, COMMIT/ROLLBACK, CHAR и VARCHAR, временные таблицы, окна, CTE, DROP TABLE     |
| 31–40 | ALTER TABLE, коррелированный подзапрос, DELETE/TRUNCATE/DROP, самосоединение, резервное копирование, многие-ко-многим, GRANT/REVOKE, хеширование, как работают триггеры |
| 41–42 | T-SQL (SQL Server), индексы SQL Server (clustered / nonclustered)                                                          |

### Sections

- [Topics](#topics)

---

## Topics

<details>
  <summary>SQL</summary>

### SQL

**Definition:** **Язык структурированных запросов** — декларативный язык для определения и запроса реляционных данных. Вы описываете, какие данные вам нужны; оптимизатор решает _как_.

**How it works:**

SQL — это интерфейс реляционного каталога. Типичные вакансии:

- Определить объекты: базы данных, таблицы, типы, индексы (`CREATE` / `ALTER` / `DROP`).
- Изменить строки: `INSERT`, `UPDATE`, `DELETE`.
- Читать: `SELECT` с `JOIN`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`.
- Контроль доступа: `GRANT` / `REVOKE`.
- Заявления группы: `BEGIN` / `COMMIT` / `ROLLBACK`.

Семейства команд: [DDL, DML и DCL](#ddl-dml-and-dcl). Диалекты имеют общую основу; Postgres, MySQL и SQL Server различаются по типам, обновлениям и функциям.

**When to use:** Структурированные данные со связями, отчетностью, транзакциями, специальной аналитикой.

**Pluses and minuses:**

| +                                       | −                                                       |
| --------------------------------------- | ------------------------------------------------------- |
| Декларативная, переносимая в СУБД       | Различия в диалектах (Postgres и MySQL)                 |
| Зрелая экосистема, инструменты          | Сложные запросы становится трудно поддерживать          |
| Сильная последовательность и соединения | Ограничения вертикального масштабирования на одном узле |

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

**Thoughts:** Приложения подключаются через **драйверы** (`pg`, `mysql2`). Планировщик запросов использует статистику: устаревшая статистика = плохие планы. Всегда параметризуйте ввод пользователя.

**Sources:** [PostgreSQL — SQL-команды](https://www.postgresql.org/docs/current/sql-commands.html) · [SQL Academy — Вопросы для собеседования](https://sql-academy.org/ru/interview-questions) · [Хабр — SQL-вопросы для собеседования, часть 1](https://habr.com/ru/articles/790850/) · [Хабр — SQL-вопросы для собеседования, часть 2](https://habr.com/ru/articles/791208/) · [Хабр — Топ-65 вопросов SQL-собеседования, часть I](https://habr.com/ru/companies/otus/статьи/461067/)

---

</details>

<details>
  <summary>DDL, DML, and DCL</summary>

### DDL, DML, and DCL

**Definition:** Операторы SQL группируются по заданиям: **DDL** изменяет схему, **DML** изменяет или читает строки, **DCL** предоставляет и отзывает привилегии.

**How it works:**

| Группа                        | Работа                             | Типичные команды                                 |
| ----------------------------- | ---------------------------------- | ------------------------------------------------ |
| **DDL** (Определение данных)  | Создавать/изменять/удалять объекты | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `COMMENT` |
| **DML** (манипуляция данными) | Работа со строками                 | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE`  |
| **DCL** (Управление данными)  | Привилегии                         | `GRANT`, `REVOKE`                                |
| **TCL** (контроль транзакций) | Зафиксировать или прервать         | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`       |

Списки интервью часто помещаются `SELECT` в ДМЛ. Стандарт SQL также называет запросы только для чтения **DQL** — если спросят, произнесите и то, и другое.

`TRUNCATE` — это команда DDL на уровне таблицы в PostgreSQL, а не строка за строкой. `DELETE`.

**When to use:** Назовите группу при объяснении миграции (DDL), запросов приложений (DML) и ролей (DCL).

**Thoughts:** Миграции должны осуществляться в формате DDL в файлах с поддержкой версий. Пользователи приложений не должны иметь права DDL в рабочей среде.

**Sources:** [PostgreSQL — команды SQL](https://www.postgresql.org/docs/current/sql-commands.html) · [PostgreSQL — привилегии](https://www.postgresql.org/docs/current/ddl-priv.html)

</details>

<details>
  <summary>Primary key</summary>

### Primary key

**Definition:** Первичный ключ — это уникальный идентификатор строки в таблице. Он не допускает дублирования и значений NULL.

**How it works:**

- Один первичный ключ на таблицу.
- Может быть один столбец или несколько (`PRIMARY KEY (a, c)` — составной ключ). Все составные столбцы `NOT NULL`.
- Подразумевается `UNIQUE` + `NOT NULL`. Использование дополнительных уникальных столбцов `UNIQUE`, а не второй ПК.
- Postgres создает уникальный индекс B-дерева для первичного ключа.

**When to use:** Каждая таблица сущностей (`students`, `courses`). Суррогатное материнство `id` это обычное приложение ПК; естественный ключ может быть `UNIQUE` рядом с ним.

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

**Thoughts:** Строка интервью: «идентифицирует строку, уникальную, никогда не NULL, возможно, составную». Цель по умолчанию: `REFERENCES` /[Внешний ключ](#внешний-ключ). Другие ограничения: [Ограничения схемы](#schema-constraints-fk-check-unique).

**Sources:** [PostgreSQL — Первичные ключи](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-PRIMARY-KEYS)

</details>

<details>
  <summary>Foreign key</summary>

### Foreign key

**Definition:** Внешний ключ — это ограничение, которое связывает столбец (или набор столбцов) в одной таблице со столбцом (или столбцами) в другой таблице — обычно это [первичный ключ](#primary-key) или другой уникальный ключ.

Он обеспечивает **ссылочную целостность**: каждое значение внешнего ключа, отличное от NULL, должно существовать в указанной таблице.

**How it works:**

#### Restricts invalid inserts

- Вы не можете вставить (или обновить) дочернее значение, которого нет в родительской таблице.

#### Restricts deleting referenced parents

- Вы не можете удалить родительскую строку, пока на нее указывают дочерние элементы, если только вы не добавите действие (`ON DELETE`).
- По умолчанию `NO ACTION` (ошибка во время проверки). `RESTRICT` является более строгим (не подлежит отсрочке). `CASCADE` удаляет детей. `SET NULL` / `SET DEFAULT` вместо этого перепишите FK.
- Одна и та же семья существует для `ON UPDATE`.

`REFERENCES parent` без списка столбцов используется первичный ключ родителя. Self-FK (дерево): [Self-join](#self-join). Таблицы соединений: [Многие-ко-многим](#многие-ко-многим). Другие типы ограничений: [Ограничения схемы](#schema-constraints-fk-check-unique).

**When to use:** Все настоящие отношения(`employees.department_id → departments.department_id`). Пропускайте только для денормализованных снимков, которые, по вашему мнению, могут болтаться.

**Examples:**

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments (department_id)
);
```

**Thoughts:** Postgres **не** автоматически индексирует ссылающийся столбец FK — добавьте его для объединений и удалений.

**Sources:** [PostgreSQL — внешние ключи](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK)

</details>

<details>
  <summary>Normalization</summary>

### Normalization

**Definition:** Нормализация сохраняет каждый факт один раз, поэтому обновления не могут расходиться. Полные формы (1NF–3NF, BCNF): [Нормальные формы](./9_database_core.md#normal-forms-1nf3nf-bcnf).

**How it works:**

Основные цели:

- **Удалить избыточность** — перестаньте хранить один и тот же факт во многих строках.
- **Повысить целостность** — одно место для обновления факта, чтобы копии не могли перемещаться.
- **Упрощенное обслуживание** — более понятная схема, которую легче изменить.

Основные нормальные формы (каждая основана на предыдущей):

| Форма | Правило | Пример |
| --- | --- | --- |
|**1НФ** | Каждый столбец содержит атомарные (неделимые) значения — в ячейке нет повторяющихся групп | Не 1НФ: `courses = "Math, Music"` в одной ячейке. 1NF: один ряд в ряд. |
| **2НФ** | Соответствует 1NF, и каждый неключевой столбец зависит от **всего** первичного ключа | ПК `(student_id, course_id)` все еще храню `student_name` — имя зависит только от `student_id`. Переместить имя в `students`. |
| **3НФ** | Удовлетворяет 2NF, и между неключевыми столбцами нет транзитивных зависимостей | `courses` имеет `instructor` и `instructor_office` — должность зависит от инструктора, а не от `course_id`. Переехать в офис `instructors`. |

Широкий ряд `student + course` заканчивается в `students`, `courses`и [регистрации](#многие-ко-многим).

**When to use:** OLTP-схемы, интервью «зачем так много таблиц?». Для большинства приложений остановитесь на 3NF. Специально денормализовать для моделей чтения — [Нормализация и денормализация](./9_database_core.md#normalization-vs-denormalization).

**Examples:**

**1NF — атомные клетки**

Раньше: два курса в одной камере.

| ID студента | Имя студента | Курсы              |
| ----------- | ------------ | ------------------ |
| 1           | Джон         | Математика, Музыка |

После: одно значение на ячейку (один курс на строку).

| ID студента | Имя студента | Идентификатор курса | Название курса |
| ----------- | ------------ | ------------------- | -------------- |
| 1           | Джон         | 101                 | Математика     |
| 1           | Джон         | 102                 | Музыка         |

**2NF — зависит от всего ключа**

До: 1NF, но `StudentName` зависит только от `StudentID`, не на `(StudentID, CourseID)`. «Джон» копируется в каждой строке курса.

| ID студента | Имя студента | Идентификатор курса | Название курса |
| ----------- | ------------ | ------------------- | -------------- |
| 1           | Джон         | 101                 | Математика     |
| 1           | Джон         | 102                 | Музыка         |

После: имя продолжает жить `Students`. Таблица ссылок сохраняет только пару ключей и факты курса, которые все еще принадлежат этой строке.

**Студенты**

| ID студента | Имя студента |
| ----------- | ------------ |
| 1           | Джон         |

**Зачисление**

| ID студента | Идентификатор курса | Название курса |
| ----------- | ------------------- | -------------- |
| 1           | 101                 | Математика     |
| 1           | 102                 | Музыка         |

**3NF — нет транзитивной зависимости**

`CourseName` зависит от `CourseID`, а не в зачисленной паре — та же идея, что и в кабинете инструктора, в зависимости от преподавателя, а не от строки курса.

До (офис повторяется, потому что следует Аде, а не курсу):

| Идентификатор курса | Название курса | Инструктор | Офис       |
| ------------------- | -------------- | ---------- | ---------- |
| 101                 | Математика     | Ада        | Комната 12 |
| 102                 | КС             | Ада        | Комната 12 |

После:

**Инструкторы**

| Инструктор | Офис       |
| ---------- | ---------- |
| Ада        | Комната 12 |

| **Курсы** | Идентификатор курса | Название курса | Инструктор |
| --------- | ------------------- | -------------- | ---------- |
| 101       | Математика          | Ада            |
| 102       | КС                  | Ада            |

Случай Джон/Математика/Музыка после 3НФ представляет собой три таблицы: `Students`, `Courses`, `Enrollments` (только идентификаторы) — см. [Многие-ко-многим](#многие-ко-многим).

**Студенты**

| ID студента | Имя студента |
| ----------- | ------------ |
| 1           | Джон         |

**Курсы**

| Идентификатор курса | Название курса |
| ------------------- | -------------- |
| 101                 | Математика     |
| 102                 | Музыка         |

**Зачисление**

| ID студента | Идентификатор курса |
| ----------- | ------------------- |
| 1           | 101                 |
| 1           | 102                 |

**Thoughts:** M:N после 1NF — это соединительная таблица, а не повторяющийся список курсов в строке студента.

**Sources:** [PostgreSQL — Ограничения](https://www.postgresql.org/docs/current/ddl-constraints.html) · [Нормализация базы данных](https://en.wikipedia.org/wiki/Database_normalization) · [SQL Academy — Нормализация базы данных](https://sql-academy.org/ru/blog/database-normalization)

</details>

<details>
  <summary>Joins</summary>

### Joins

**Definition:** JOIN объединяет строки из двух или более таблиц, используя связанные столбцы.

**How it works:**

Оптимизатор выбирает вложенный цикл, хеш-соединение или объединение слиянием из статистики и индексов. Правило соответствия живет в `ON` (or `USING`). A `WHERE` Фильтр на внутренней стороне внешнего соединения может удалить несовпадающие строки и превратить его во внутреннее соединение.

Виды ПРИСОЕДИНЕНИЯ:

- **INNER JOIN** — только строки, имеющие совпадение в **обеих** таблицах. Нет совпадений → строка опущена.
- **LEFT JOIN** (LEFT OUTER JOIN) — каждая строка из **левой** таблицы плюс соответствующие столбцы правой таблицы. Нет совпадений → правые столбцы имеют значение NULL.- **RIGHT JOIN** (RIGHT OUTER JOIN) — каждая строка из **правой** таблицы плюс соответствующие столбцы левой таблицы. Нет совпадений → левые столбцы имеют значение NULL.
- **FULL OUTER JOIN** (FULL JOIN) — каждая строка, совпадающая в любой таблице. Несовпадающие стороны имеют значение NULL.
- **CROSS JOIN** — Декартово произведение: каждая строка первой таблицы соединена с каждой строкой второй. Нет `ON` состояние.

`LEFT` / `RIGHT` / `FULL` уже означает ВНЕШНИЙ; слово `OUTER` является необязательным. INNER и OUTER в одном месте: [INNER JOIN vs OUTER JOIN](#inner-join-vs-outer-join). Одна и та же таблица дважды: [Self-join](#self-join).

| Тип соединения     | Возврат                                               |
|  ------------------  |  -----------------------------------------------------  |
| **ВНУТРЕННИЙ**     | Строки с совпадениями в обеих таблицах |
| **СЛЕВА** | Все левые строки + совпадение справа (NULL, если совпадений нет) |
| **ПРАВИЛЬНО**      | Зеркало ЛЕВОЕ                                         |
| **ПОЛНЫЙ ВНЕШНИЙ** | Все строки из обоих, NULL, где нет совпадений         |
| **КРЕСТ**          | Декартово произведение — каждое левое × каждое правое |

Стиль предиката: `ON expr`, `USING (col)` (общий столбец один раз), `NATURAL` (объединить все одноименные столбцы — хрупко).

**When to use:**

INNER для необходимых отношений; СЛЕВА, если дополнительные сопутствующие данные (например, пользователь без заказов). RIGHT — то же самое, что замена таблиц, а LEFT — предпочтение LEFT. `EXISTS` часто бьет `IN` для больших подзапросов.

**Pluses and minuses:**

| +                                              | −                                                                        |
| ---------------------------------------------- | ------------------------------------------------------------------------ |
| Нормализованная схема, отсутствие дублирования | Дорого для больших таблиц без индексов                                   |
| Единый запрос связанных данных                 | Неправильный тип соединения = тихая потеря данных (ВНУТРЕННЯЯ или ЛЕВАЯ) |

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

**Thoughts:** Предпочитать `EXISTS` над `IN (SELECT ...)` для коррелирующих проверок. Избегать `SELECT *` в JOINs — извлекайте только необходимые столбцы. `NATURAL JOIN` прерывается, когда появляется новый столбец с тем же именем. Одна и та же таблица дважды: [Self-join](#self-join).

**Sources:**

[PostgreSQL — соединения](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN) · [Использовать индекс, Люк — соединения](https://use-the-index-luke.com/sql/join)

---

</details>

<details>
  <summary>Subqueries</summary>

### Subqueries

**Definition:** Подзапрос — это SQL-запрос, вложенный в другой запрос. Вы запускаете его, чтобы внешний запрос мог использовать его результат.

**How it works:**

Внутренний `SELECT` оценивается (один раз, если только он не является [коррелированным](#correlated-subquery)) и внешний оператор использует этот результат.

Типичные места:

- **Фильтр** — подзапрос в `WHERE` or `HAVING` (`=`, `IN`, `EXISTS`).
- **Выбрать список** — скалярный подзапрос в виде столбца (должен возвращать одну строку и один столбец).
- **Производная таблица** — подзапрос в `FROM`в скобках — псевдоним (виртуальная таблица).

Именованная форма подзапроса: [CTE](#cte).

**When to use:** Когда внешнему запросу требуется вычисленный набор или скаляр (идентификатор отдела для `'IT'`, сгруппированный счет). Предпочитайте JOIN, когда обе стороны большие и вам нужно много столбцов.

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

**Thoughts:** Коррелированный подзапрос не всегда медленный — `EXISTS` может произойти короткое замыкание. Измерьте с помощью [EXPLAIN](#query-optimization-explain).

**Sources:** [PostgreSQL — выражения подзапросов](https://www.postgresql.org/docs/current/functions-subquery.html) · [PostgreSQL — табличные выражения (FROM)](https://www.postgresql.org/docs/current/queries-table-expressions.html) · [SQL Academy — коррелированные подзапросы](https://sql-academy.org/ru/guide/correlated-subqueries)

</details>

<details>
  <summary>DISTINCT</summary>

### DISTINCT

**Definition:** `SELECT DISTINCT` возвращает уникальные строки для выбранного списка столбцов — дубликаты сворачиваются после вычисления результата.

**How it works:**

- Различие распространяется на **все выбранные столбцы**, а не на один столбец, если только он не единственный столбец в списке.
- `DISTINCT ON (expr)` (PostgreSQL) сохраняет первую строку для каждого ключа после `ORDER BY`.
- Дедупликация больших результатов требует дополнительной работы: уникальный индекс или `GROUP BY` может быть яснее.

**When to use:** Отличительные ярлыки, уникальные идентификаторы пользователей из разветвленного объединения. Пропустите, когда соединение не должно дублировать строки — вместо этого исправьте соединение.

**Examples:**

```sql
SELECT DISTINCT position FROM employees;

SELECT DISTINCT ON (user_id) user_id, created_at
FROM events
ORDER BY user_id, created_at DESC;
```

**Thoughts:** `DISTINCT` не является заменой уникального ограничения. Предпочитать `GROUP BY` когда вы также суммируете.

**Sources:** [PostgreSQL — DISTINCT](https://www.postgresql.org/docs/current/sql-select.html#SQL-DISTINCT)

</details>

<details>
  <summary>WHERE vs HAVING</summary>

### WHERE vs HAVING

**Definition:** `WHERE` фильтрует отдельные строки **перед** группировкой (без агрегирования). `HAVING` фильтрует группы **после** `GROUP BY` (допускаются агрегаты).

**How it works:**

Логический порядок: `FROM` / `JOIN` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`.

**ГДЕ**

- Запускается **до** `GROUP BY`.
- Невозможно использовать агрегатные функции (`SUM()`, `COUNT()`, `AVG()`, …).
- Применяется к отдельным строкам таблицы (или объединения).

**ИМЕЮ**

- Запускается **после** `GROUP BY`.
- Может использовать агрегатные функции.
- Применяется к сгруппированному результату, а не к необработанным строкам.
- Разрешено без `GROUP BY` — затем он фильтрует одну общую совокупную строку.

**When to use:** Предикаты строк (`salary > 50000`) in `WHERE` поэтому индексы могут применяться. Групповые предикаты (`COUNT(*) > 5`) in `HAVING`.

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

**Thoughts:** Фильтрация `WHERE` дешевле, чем агрегирование строк, которые вы выбросите `HAVING`.

**Sources:** [PostgreSQL — ВЫБРАТЬ](https://www.postgresql.org/docs/current/sql-select.html)

</details>

<details>
  <summary>Indexes</summary>

### Indexes

**Definition:**

Структуры данных (обычно **B-дерево** в Postgres), которые ускоряют поиск за счет затрат на запись и хранение.

**How it works:**

Индекс хранит в куче упорядоченные ключи и указатели (TID). Соответствие `WHERE`/`JOIN`/`ORDER BY` может выполнять индексное сканирование вместо последовательного сканирования. Пишет(`INSERT`/`UPDATE`/`DELETE`) должен обновить каждый индекс измененных столбцов.

| Тип индекса                 | Вариант использования                                |
| --------------------------- | ---------------------------------------------------- |
| **B-дерево** (по умолчанию) | `=`, `<`, `>`, `BETWEEN`, `ORDER BY`                 |
| **Хеш**                     | Только равенство (редко в Postgres)                  |
| **ДЖИН**                    | JSONB, массивы, полнотекстовый                       |
| **ГиСТ**                    | Геопространственные данные, типы диапазонов          |
| **Частично**                | Индексное подмножество строк (`WHERE active = true`) |
| **Композитный**             | Многоколоночный (`user_id, created_at`)              |
| **Покрытие**                | ВКЛЮЧИТЬ столбцы, чтобы избежать поиска в таблице    |

**When to use:** Столбцы в `WHERE`, `JOIN`, `ORDER BY` на больших столах. Не индексируйте только столбцы с низкой мощностью (например, логические `is_active`). Составной левый префикс: `(user_id, created_at)` помогает `WHERE user_id = $1` и пара, не `created_at` один.

**Pluses and minuses:**

| +                                              | −                                                                        |
|  ----------------------------------------------  |  ------------------------------------------------------------------------  |
| Значительное ускорение чтения                  | Медленная INSERT/UPDATE/DELETE в индексированных столбцах                |
| Обеспечивает эффективную сортировку/фильтрацию | Неправильный индекс = пустая трата места, планировщик может игнорировать |
| Уникальные индексы налагают ограничения | Слишком большое количество индексов сбивает с толку оптимизатор |

**Examples:**

```sql
CREATE INDEX idx_orders_user_created ON orders (user_id, created_at DESC);
CREATE INDEX idx_users_active_email ON users (email) WHERE deleted_at IS NULL;
```

**Thoughts:** Индексы не помогают `LIKE '%foo%'` (ведущий подстановочный знак). Использовать `EXPLAIN ANALYZE` для проверки использования индекса. Удалите неиспользуемые индексы, найденные в `pg_stat_user_indexes`. Дополнительный диск + медленнее `INSERT`/`UPDATE`/`DELETE` потому что каждая строка индекса сохраняется. **хеш-индекс** — это метод доступа, а не хэш пароля. См. [Хеш-функции](#хеш-функции).

**Sources:**

[Используй индекс, Люк](https://use-the-index-luke.com/) · [PostgreSQL — Индексы](https://www.postgresql.org/docs/current/indexes.html)

---

</details>

<details>
  <summary>DELETE vs TRUNCATE</summary>

### DELETE vs TRUNCATE

**Definition:** `DELETE` удаляет выбранные строки (необязательно `WHERE`).

`TRUNCATE` удаляет все строки из таблицы за один раз, без `WHERE`.

**How it works:**

#### DELETE (selected rows)

- Удаляет выбранные строки из таблицы.
- Вы можете использовать `WHERE` для удаления определенных строк.
- Операция записывается в журнал транзакций **строка за строкой**.
- `ON DELETE` вызывает огонь.
- Медленнее, чем `TRUNCATE`.

#### TRUNCATE (all rows)

- Удаляет **все** строки из таблицы. В **большинстве СУБД** вы не можете отменить это с помощью `ROLLBACK` (MySQL и подобные). В **PostgreSQL** это **можно** `ROLLBACK` с окружающей транзакцией.
- Вы не можете использовать `WHERE`.
- Быстрее, потому что не протоколируется построчно.
- Сбрасывает идентичность/автоинкремент (`RESTART IDENTITY` в Постгресе; по умолчанию есть `CONTINUE IDENTITY` если ты не спросишь).
- `ON DELETE` триггеры **не** срабатывают (`ON TRUNCATE` триггеры операторов могут).

| Команда    | Строки            | `WHERE` | `DELETE` триггеры | Скорость  | `ROLLBACK` (Постгрес) |
| ---------- | ----------------- | ------- | ----------------- | --------- | --------------------- |
| `DELETE`   | Выбрано (или все) | Да      | Да                | Медленнее | Да                    |
| `TRUNCATE` | Все               | Нет     | Нет               | Быстрее   | Да                    |

**When to use:** `DELETE` для фильтрованной очистки (`salary < 30000`) или триггеры для каждой строки / `RETURNING`.

`TRUNCATE` чтобы очистить стол, который вы держите.

Чтобы удалить объект: [DROP TABLE](#drop-table). Все три: [DELETE, TRUNCATE и DROP](#delete-truncate-and-drop).

**Examples:**

```sql
DELETE FROM employees WHERE salary < 30000;
TRUNCATE TABLE employees;
TRUNCATE TABLE employees RESTART IDENTITY;
```

**Thoughts:** «TRUNCATE не может ROLLBACK» — это MySQL/старый миф, а в PostgreSQL это возможно. Все еще предпочитаю `DELETE` когда тебе нужно `WHERE` или триггеры для каждой строки.

**Sources:** [PostgreSQL — УДАЛЕНИЕ](https://www.postgresql.org/docs/current/sql-delete.html) · [PostgreSQL — TRUNCATE](https://www.postgresql.org/docs/current/sql-truncate.html) · [SQL Academy — TRUNCATE, DELETE и DROP](https://sql-academy.org/ru/blog/sql-truncate-delete-drop)

</details>

<details>
  <summary>ACID</summary>

### ACID

**Definition:** Транзакция — это последовательность операций, которая должна завершиться полностью или не завершиться вообще. **ACID** — это четыре гарантии: атомарность, согласованность, изоляция, долговечность.

**How it works:**

#### Atomicity

- Транзакция выполняется полностью или не выполняется вообще.
- В случае сбоя все изменения отменяются (`ROLLBACK`).

#### Consistency

- Транзакция переводит базу данных из одного допустимого состояния в другое.
- Все правила и ограничения (PK, FK, CHECK) остаются в силе после его завершения.
- Это не КАП «каждая реплика актуальна».

#### Isolation

- Другие транзакции не видят результатов, пока эта не завершится (`COMMIT`).
- Параллельные транзакции не портят работу друг друга. Уровни: [Транзакции и изоляция](./9_database_core.md#transactions-isolation-levels).

#### Durability

- После успешного `COMMIT`, результаты выдерживают крах.
- Изменения записываются в долговременное хранилище (WAL).

Глаголы SQL: [COMMIT и ROLLBACK](#commit-and-rollback). Более полное примечание: [ACID](./9_database_core.md#acid).

**When to use:** Переносы, инвентарь, бронирование — везде частичная запись — это баг.

**Examples:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
-- On error: ROLLBACK — neither update stays alone
```

**Thoughts:** SQL `COMMIT`/`ROLLBACK` являются глаголами; ACID — это контракт. ACID **Согласованность** не является согласованностью CAP.

**Sources:** [PostgreSQL — Транзакции](https://www.postgresql.org/docs/current/tutorial-transactions.html) · [Академия SQL — ACID](https://sql-academy.org/ru/blog/sql-acid)

</details>

<details>
  <summary>Triggers</summary>

### Triggers

**Definition:** **Триггер** автоматически запускает функцию для событий таблицы (`INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`) — `BEFORE`, `AFTER`, or `INSTEAD OF` (просмотры).

**How it works:**

- `FOR EACH ROW` vs `FOR EACH STATEMENT`.
- Триггеры строк см. `NEW` / `OLD` (и `NEW.*` / `OLD.*` в PL/pgSQL).
- `BEFORE` может измениться `NEW` или пропусти операцию(`RETURN NULL`). `AFTER` предназначен для побочных эффектов (аудит).
- PostgreSQL: `CREATE FUNCTION … RETURNS trigger`, затем `CREATE TRIGGER … EXECUTE FUNCTION …`.
- Триггеры таблицы DML: `INSERT` / `UPDATE` / `DELETE` / `TRUNCATE`. ДДЛ (`CREATE`/`ALTER`/`DROP`) использует **триггеры событий**, а не `CREATE TRIGGER` на столе.

**When to use:** Таблицы аудита, производные столбцы, которые нельзя выразить как сгенерированные столбцы, а также ограничения правил. Пропустить логику, предназначенную только для приложения и принадлежащую службе — триггеры скрывают пути записи.

**Pluses and minuses:**

| +                                    | −                                       |
| ------------------------------------ | --------------------------------------- |
| Всегда работает, даже вне приложения | Трудно протестировать и легко повторить |
| Центральная целостность/аудит        | Сюрпризы ORM и погрузчики               |

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

**Thoughts:** `TRUNCATE` не стреляет `DELETE` триггеры строк — см. [DELETE, TRUNCATE и DROP](#delete-truncate-and-drop).

**Sources:** [PostgreSQL — СОЗДАНИЕ ТРИГГЕРА](https://www.postgresql.org/docs/current/sql-createtrigger.html) · [PostgreSQL — Триггерные функции](https://www.postgresql.org/docs/current/plpgsql-trigger.html)

</details>

<details>
  <summary>VIEW</summary>

### VIEW

**Definition:** Представление — это виртуальная таблица, строки которой берутся из сохраненного `SELECT`. Он не хранит собственную копию данных; это именованный способ просмотра одной или нескольких таблиц.

**How it works:**

- `CREATE VIEW name AS SELECT …`. Каждый запрос к представлению выполняет это `SELECT` снова.
- `CREATE MATERIALIZED VIEW` отличается: он хранит ваш снимок `REFRESH`.

#### Simplify complex queries

- Сохраните соединение (или длинное `SELECT`) и запросить его как таблицу.

#### Security

- Грант `SELECT` в представлении и запретить предоставление прав на базовые таблицы, чтобы роль видела только выбранные столбцы/строки.
- Не полноценная стена сама по себе: используйте гранты, СБН и `security_barrier` когда представление является фильтром безопасности.

#### Updatability

- Простые представления (одна таблица в `FROM`, no `DISTINCT` / `GROUP BY` / `UNION` /агрегаты) может занять `INSERT` / `UPDATE` / `DELETE`.
- `WITH CHECK OPTION` отклоняет записи, которые исчезнут из поля зрения.
- Представление объединения (например, `employee_details` ниже) доступен только для чтения, если вы не добавите `INSTEAD OF` триггеры.

#### Combined data

- Представление может объединять несколько таблиц, поэтому читатели всегда видят одну и ту же форму — оно не заменяет PK/FK/`CHECK`.

**When to use:** Скрыть объединения за стабильным именем.

Назначьте роли окно с ограничением по столбцам, а не всю таблицу.

Пропустить, если запрос используется один раз.

**Pluses and minuses:**

| +                                                    | −                                                              |
| ---------------------------------------------------- | -------------------------------------------------------------- |
| Одно имя для сложного соединения                     | Дополнительный объект каталога для переноса                    |
| Можно скрыть столбцы из роли                         | Без грантов не граница безопасности / РЛС                      |
| Простые представления могут быть доступны для записи | Необходимо объединить/объединить просмотры `INSTEAD OF` писать |

**Examples:**

```sql
CREATE VIEW employee_details AS
SELECT e.id, e.name, d.name AS department, e.salary
FROM employees e
JOIN departments d ON e.department_id = d.id;

SELECT * FROM employee_details WHERE salary > 50000;
```

**Thoughts:** Ловушка на собеседовании: «В представлениях хранятся данные». Обычные взгляды этого не делают; материализованные представления делают. Предоставить представление, отозвать базовые таблицы.

**Sources:** [PostgreSQL — Представления](https://www.postgresql.org/docs/current/tutorial-views.html) · [PostgreSQL — СОЗДАНИЕ ПРЕДСТАВЛЕНИЯ](https://www.postgresql.org/docs/current/sql-createview.html) · [PostgreSQL — СОЗДАНИЕ МАТЕРИАЛИЗИРОВАННОГО ПРОСМОТР](https://www.postgresql.org/docs/current/sql-creatematerializedview.html)

</details>

<details>
  <summary>LIKE</summary>

### LIKE

**Definition:** `LIKE` используется в `WHERE` чтобы найти строки, строка которых соответствует шаблону.

**How it works:**

#### %

- Любая последовательность символов, в том числе пустая.

#### \_

- Ровно один персонаж.

- Шаблон должен соответствовать **целой** строке; помещать `%` на обоих концах, чтобы соответствовать подстроке.
- PostgreSQL `LIKE` чувствителен к регистру. Использовать `ILIKE` для соответствия локали без учета регистра (не в стандарте SQL). Типичная процедура сортировки MySQL `LIKE` как регистронезависимый.
- Буквальный `%` or `_`: префикс с escape-символом (по умолчанию Postgres `\`), or `ESCAPE`.

**When to use:** Фильтры префикса, суффикса или типа «n-й символ…».

Пропускать `'%foo%'` в масштабе по B-дереву — ведущий `%` обычно не может использовать индекс. Воспользуйтесь полнотекстовым поиском или `pg_trgm` вместо.

**Examples:**

```sql
-- Starts with A
SELECT * FROM employees WHERE name LIKE 'A%';

-- Ends with e
SELECT * FROM employees WHERE name LIKE '%e';

-- Second character is a
SELECT * FROM employees WHERE name LIKE '_a%';
```

**Thoughts:** `'%foo%'` не будет использовать обычное B-дерево. См. [Индексы](#indexes).

**Sources:** [PostgreSQL — сопоставление с образцом](https://www.postgresql.org/docs/current/functions-matching.html)

</details>

<details>
  <summary>Aggregate functions</summary>

### Aggregate functions

**Definition:** Агрегатные функции вычисляют набор значений и возвращают одно значение. Они часто используются с `GROUP BY`.

**How it works:**

#### COUNT()

- Считает ряды. `COUNT(*)` считает каждую входную строку. `COUNT(col)` считается отличным от NULL `col` ценности.

#### SUM()

- Сумма значений, отличных от NULL. Нет строк → `NULL`, нет `0` (`COALESCE` если вам нужен ноль).

#### AVG()

- Среднее арифметическое значений, отличных от NULL.

#### MAX() / MIN()

- Наибольшее/наименьшее значение, отличное от NULL.

Без `GROUP BY`, весь результат представляет собой одну группу. С `GROUP BY`, вы получаете одну строку на сегмент; неагрегированный `SELECT` столбцы должны появиться в `GROUP BY`. `WHERE` фильтрует строки **перед** агрегатом (нет `COUNT` in `WHERE`). `HAVING` фильтрует группы после: [WHERE vs HAVING](#where-vs-having). Одна строка в строке с промежуточным итогом: [Оконные функции](#window-functions).

**When to use:** Численность, итоги, средние показатели по отделам, экстремумы, сумма за месяц.

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

**Thoughts:** `WHERE temp = MAX(temp)` недействителен — агрегат принадлежит подзапросу или `HAVING`.

**Sources:** [PostgreSQL — агрегатные функции](https://www.postgresql.org/docs/current/tutorial-agg.html) · [PostgreSQL — список агрегатных функций](https://www.postgresql.org/docs/current/functions-aggregate.html)

</details>

<details>
  <summary>UNION vs UNION ALL</summary>

### UNION vs UNION ALL

**Definition:** `UNION` объединяет результаты двух или более `SELECT` запрашивает и удаляет повторяющиеся строки.

`UNION ALL` объединяет эти результаты и сохраняет дубликаты.

**How it works:**

#### UNION

- Добавляет `SELECT` результаты (порядок строк не гарантируется).
- Удаляет дубликаты из объединенного результата (та же идея, что и `DISTINCT`).
- Дополнительная сортировка/хеширование — медленнее, чем `UNION ALL`.

#### UNION ALL

- Добавляет `SELECT` результаты такие же.
- Сохраняет повторяющиеся строки.
- Быстрее, потому что он пропускает отдельный шаг.

Оба плеча должны возвращать одинаковое количество столбцов совместимых типов. Имена выходных столбцов начинаются с первого `SELECT`. `INTERSECT` / `EXCEPT` другие операторы множества (`ALL` там же хранятся дубликаты).

**When to use:** `UNION` когда дубликаты должны исчезнуть.

`UNION ALL` когда источники не пересекаются (горячий + архив) или действительны дубликаты.

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

**Thoughts:** Выбор рекурсивных CTE `UNION` vs `UNION ALL` специально — `UNION` может остановить дублирование узлов графа. См. [CTE](#cte).

**Sources:** [PostgreSQL — Объединение запросов](https://www.postgresql.org/docs/current/queries-union.html)

</details>

<details>
  <summary>Procedures vs functions</summary>

### Procedures vs functions

**Definition:** **функция** возвращает значение и может использоваться в выражениях SQL. **Процедура** вызывается с помощью `CALL` и может осуществлять контроль транзакций.

**How it works:**


| | Функция                                                   | Процедура                                                 |
| | --------------------------  |  ---------------------------------------------------------  |
| | Создать                    | `CREATE FUNCTION`                                         | `CREATE PROCEDURE` (PG 11+)                               |
| | Вызов                      | `SELECT fn(…)`, внутри выражений                          | `CALL proc(…)`                                            |
| | Вернуться                  | Необходимый (`RETURNS`)                                   | Набор результатов не требуется; `OUT` аргументы разрешены |
| | `COMMIT`/`ROLLBACK` внутри | Нет (за исключением некоторых процедур/шаблонов передачи) | Разрешено в `CALL`                                        |

**When to use:** Функции для вычисленных значений, сгенерированные значения по умолчанию, тела триггеров. Процедуры для административных пакетов (`CALL clean_emp()`). Приложение CRUD обычно остается на клиенте с параметризованным SQL.

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

**Thoughts:** Здесь показаны интервью MySQL «хранимая процедура против функции», но функции PostgreSQL — это первоклассный SQL. Не перемещайте всю бизнес-логику в базу данных по умолчанию.

**Sources:** [PostgreSQL — СОЗДАНИЕ ФУНКЦИИ] (https://www.postgresql.org/docs/current/sql-createfunction.html) · [PostgreSQL — СОЗДАНИЕ ПРОЦЕДУРЫ] (https://www.postgresql.org/docs/current/sql-createprocedure.html) · [PostgreSQL — процедуры SQL] (https://www.postgresql.org/docs/current/xfunc-sql.html)

</details>

<details>
  <summary>Query optimization & EXPLAIN</summary>

### Query optimization & EXPLAIN

**Definition:** Проанализируйте, как база данных выполняет запрос — тип сканирования, использование индекса, оценки строк, фактическое время. Та же цель в PostgreSQL (`EXPLAIN`) и SQL Server (Actual Execution Plan).

**How it works:**

Типичные рычаги (сначала смотрите план — см. [Indexes](#indexes)):

- Индексные столбцы, используемые в `WHERE`, `JOIN`, `ORDER BY`; удалить неиспользуемые индексы.
- Выбирайте только необходимые столбцы — избегайте `SELECT *`.
- Предпочитать ключи равенства; не переносите индексированный столбец в функцию, если у вас нет соответствующего индекса выражения.
- Предпочитать `JOIN` / `EXISTS` более коррелированным подзапросам, которые выполняются для каждой строки.
- `LIMIT` / пагинация набора ключей вместо глубокой `OFFSET`.
- Держите статистику свежей; устаревшая статистика → плохие планы.
- Материализованные представления или кеш приложения для горячих агрегатов — см. примечания к рабочему кэшированию.

#### PostgreSQL

```sql
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT * FROM orders WHERE user_id = 42 AND status = 'pending';
```

| Оператор (плохо → хорошо) | Значение                                           |
| ------------------------- | -------------------------------------------------- |
| `Seq Scan`                | Полное сканирование таблицы — OK только на малых   |
| `Index Scan`              | Поиск по индексу + чтение heap                     |
| `Index Only Scan`         | Покрыт индексом — лучший вариант при совпадении колонок |
| `Nested Loop`             | Внутренний probe на каждую внешнюю строку — OK если внутренний проиндексирован |

Обновить статистику: `ANALYZE orders;`. Неиспользуемые индексы: `pg_stat_user_indexes`.

#### SQL Server

Включите **Include Actual Execution Plan** в SSMS или Azure Data Studio (или `SET STATISTICS IO ON` для I/O). Зелёный = SELECT; жёлтый = предупреждения (missing index, implicit conversion).

| Оператор (плохо → хорошо) | Значение                                              |
| ------------------------- | ----------------------------------------------------- |
| **Table Scan**            | Полное сканирование таблицы                           |
| **Clustered Index Scan**  | Полное сканирование clustered-индекса                 |
| **Index Seek**            | Эффективный поиск по nonclustered-индексу             |
| **Key Lookup**            | Доп. переход за колонками вне индекса — добавьте `INCLUDE` или сузьте `SELECT` |
| **Hash Match** / **Sort** | Хеш или сортировка — следите за spill в tempdb        |

Обновить статистику: `UPDATE STATISTICS orders;`. Подсказки missing index: `sys.dm_db_missing_index_details` (только подсказки — проверяйте планом).

**Что смотреть в любом плане:** estimated vs actual rows (большой разрыв = плохая статистика), scan vs seek на горячих предикатах, предупреждения на sort/hash, key lookup при широком `SELECT *`.

**When to use:** Медленные конечные точки, перед добавлением индексов, после большого роста данных или изменения схемы.

**Pluses and minuses:**

| +                                          | −                                                                      |
| ------------------------------------------ | ---------------------------------------------------------------------- |
| Находит недостающие индексы и плохие планы | Для вывода требуется практика чтения                                   |
| Дешевле гадать на production               | `ANALYZE` / `UPDATE STATISTICS` добавляет нагрузку — сначала на staging |

**Examples:**

```sql
-- PostgreSQL
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT id, total FROM orders WHERE user_id = 42 AND status = 'pending';

-- SQL Server — включите Actual Execution Plan в клиенте, затем:
SELECT id, total FROM orders WHERE user_id = 42 AND status = 'pending';
```

**Thoughts:**

- **N+1 на уровне SQL** — приложение выполняет 1 запрос + N запросов в цикле. Исправьте с помощью JOIN или `WHERE id IN (...)`.
- Избегать `SELECT *`; получить нужные столбцы (меньше key lookup в SQL Server, Index Only Scan в Postgres).
- Пагинация: `OFFSET` работает медленно на глубине — используйте набор ключей (`WHERE id > $cursor ORDER BY id LIMIT 20` / `OFFSET-FETCH` с seek-ключом).
- Предпочитать ключи равенства; перенос индексированного столбца в функцию (`WHERE lower(email) = …`) требует соответствующий индекс выражения.

**Sources:** [PostgreSQL — EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html) · [SQL Server — Display an actual execution plan](https://learn.microsoft.com/en-us/sql/relational-databases/performance/display-an-actual-execution-plan) · [SQL Server — Clustered and nonclustered indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described) · [SQL Academy — Оптимизация запросов](https://sql-academy.org/ru/blog/sql-query-optimization)

---

</details>

<details>
  <summary>Schema constraints (FK, CHECK, UNIQUE)</summary>

### Schema constraints (FK, CHECK, UNIQUE)

**Definition:** Ограничения обеспечивают целостность данных на уровне базы данных, а не только в коде приложения.

**How it works:**

| Ограничение              | Цель                                                                                                                                                                                                                                            |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ПЕРВИЧНЫЙ КЛЮЧ**       | Уникальный идентификатор строки                                                                                                                                                                                                                 |
| **ВНЕШНИЙ КЛЮЧ (FK)**    | Ссылочная целостность: дочерний элемент должен соответствовать родительскому; вставка отсутствующего родительского идентификатора не удалась;родительское удаление завершается неудачей, если только `ON DELETE` — [Внешний ключ](#foreign-key) |
| **УНИКАЛЬНО**            | Нет повторяющихся значений в столбцах                                                                                                                                                                                                           |
| **НЕ НУЛЬ**              | Столбец обязателен                                                                                                                                                                                                                              |
| **ПРОВЕРИТЬ**            | Логическое выражение для каждой строки (`price > 0`, `status IN ('open','closed')`)                                                                                                                                                             |
| **ПО УМОЛЧАНИЮ**         | Значение при вставке без столбца                                                                                                                                                                                                                |
| **ИСКЛЮЧИТЬ** (Postgres) | Никакие две строки не удовлетворяют данному оператору (типично: перекрывающиеся диапазоны)                                                                                                                                                      |

ПК `NOT NULL` + `UNIQUE` плюс «это идентификатор строки». В таблице может быть несколько `UNIQUE` ограничения, но только один `PRIMARY KEY`.

| При удалении/обновлении  | Эффект                                                |
| ------------------------ | ----------------------------------------------------- |
| `RESTRICT` / `NO ACTION` | Блокировать, если есть дети                           |
| `CASCADE`                | Автоматическое удаление/обновление дочерних элементов |
| `SET NULL`               | Обнулить ФК у детей                                   |
| `SET DEFAULT`            | Установите для столбцов FK значения по умолчанию      |

**When to use:** Всегда ФК `orders.user_id → users.id`. Использовать `CHECK` для перечислений и положительных сумм. `ON DELETE CASCADE` для объединения таблиц; `RESTRICT` для заказов со ссылкой на пользователей. `DEFAULT` для статуса/`created_at`.

**Pluses and minuses:**

| +                                               | −                                                         |
| ----------------------------------------------- | --------------------------------------------------------- |
| Целостность, даже если в приложении есть ошибки | Порядок миграции имеет значение (родитель важнее ребенка) |
| Самодокументированные отношения                 | `CASCADE` удаления могут удивить                          |
| БД отклоняет неверные данные во время записи    | ORM может бороться с FK во время массовых грузов          |

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

**Thoughts:** Карты Призма `@relation` в ФК. Добавляйте индексы к столбцам FK. Postgres не индексирует FK автоматически. Темы: [Первичный ключ](#primary-key), [Внешний ключ](#foreign-key).

**Sources:**

[PostgreSQL — ограничения](https://www.postgresql.org/docs/current/ddl-constraints.html)

---

</details>

<details>
  <summary>SQL injection</summary>

### SQL injection

**Definition:** SQL-инъекция — это атака, при которой вредоносный SQL-код вводится через пользовательский ввод, в результате чего база данных выполняет несанкционированные запросы.

**How it works:**

SQL-код, объединенный строками, позволяет злоумышленнику изменить структуру запроса (`' OR '1'='1`, остальное комментируйте, иногда второе высказывание). Параметризованные запросы связывают значения отдельно, так что входные данные остаются данными, а не SQL — они не «экранируют» символы в текст запроса.

#### Impact

- Украсть данные.
- Удалить или изменить данные.
- Получите доступ на уровне администратора, если это позволяет роль БД.

#### 1. Parameterized queries (prepared statements)

- Используйте заполнители вместо объединения строк. См. раздел [Подготовленные операторы](#prepared-statements-placeholders).

#### 2. ORM

- API-интерфейсы запросов обычно связывают параметры. Небезопасно, если вы объединяете в `$queryRaw` / `sequelize.query`.

#### 3. Validate input

- Проверьте тип и формат на **сервере**. Клиентские проверки — это UX, а не элемент управления SQLi. Идентификаторы белого списка (таблица/столбец/`ORDER BY`) — их нельзя привязать как параметры.

#### 4. Least privilege

- Предоставьте приложению минимальные права. Нет `DROP` / `GRANT` / админ каталога. См. [GRANT и REVOKE](#grant-and-revoke).

#### 5. Stored procedures

- Безопасно только тогда, когда аргументы связаны, а не объединены в динамический SQL. Инкапсулировать логику недостаточно. См. раздел [Процедуры и функции](#procedures-vs-functions).

Не возвращайте клиентам необработанные ошибки БД.

**When to use:** Каждое контролируемое пользователем значение в запросе (логин, поиск, идентификаторы).

**Pluses and minuses:**

| + (профилактики)                              | −                                                   |
| --------------------------------------------- | --------------------------------------------------- |
| Параметризация удаляет весь класс             | Объединение одной строки по-прежнему не удается     |
| API-интерфейсы ORM привязываются по умолчанию | Необработанный SQL в ORM по-прежнему можно внедрять |

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

Полезная нагрузка атаки: `email = "' OR '1'='1' --"` делает объединенный `WHERE` всегда правда.

**Thoughts:** SQL-инъекция — это не XSS (SQL против HTML/JS). Экранирование внешнего интерфейса не привязывает параметры. Хранение необработанного пароля в `WHERE` это отдельная ошибка — хэшируйте ее. См. [Специальное — SQL-инъекция](./7_special_security.md#sql-injection).

**Sources:** [OWASP — SQL-инъекция](https://owasp.org/www-community/attacks/SQL_Injection) · [OWASP — Предотвращение SQL-инъекций](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

</details>

<details>
  <summary>Relational database</summary>

### Relational database

**Definition:** Реляционная база данных — это база данных, основанная на реляционной модели. Данные хранятся в таблицах; отношения между данными определяются с помощью ключей.

**How it works:**

#### Tables (relations)

- Данные организованы в именованные таблицы строк и столбцов. В PostgreSQL _отношение_ по сути является таблицей.

#### Rows (records / tuples)

- Каждая строка представляет собой одну запись. SQL **не** гарантирует порядок строк, если только вы не `ORDER BY`.

#### Columns (attributes)

- Каждый столбец имеет имя и тип (`integer`, `text`, `numeric`, …). Порядок столбцов фиксирован.

#### Keys

- Определить записи и связать таблицы: [Первичный ключ](#primary-key), [Внешний ключ](#foreign-key), дополнительный [UNIQUE](#schema-constraints-fk-check-unique).

SQL — это язык этой модели. 1:1, 1:N, M:N через [таблицу соединений](#многие-ко-многим). Более широкие типы магазинов: [Таксономия базы данных](./9_database_core.md#database-taxonomy).

**When to use:** Структурированная OLTP, отчетность, ограничения целостности.

Документ/KV сохраняется, когда шаблон доступа не является реляционным — [матрица выбора БД](./9_database_nosql_core.md#db-selection-matrix).

**Pluses and minuses:**

| +                                                 | −                                                                    |
| ------------------------------------------------- | -------------------------------------------------------------------- |
| Добавляйте таблицы и столбцы по мере роста схемы  | Изменения схемы требуют миграции                                     |
| Ограничения сохраняют целостность во время записи | Потолок вертикального масштаба на одном основном                     |
| SQL — стандартный язык данных                     | Неправильный тип соединения приводит к потере или дублированию строк |

**Examples:**

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  department_id INT REFERENCES departments (id)
);
```

**Thoughts:** «Реляционная» — это модель Кодда, а не «только Postgres». MySQL и SQL Server также являются реляционными.

**Sources:** [PostgreSQL — Основные понятия](https://www.postgresql.org/docs/current/tutorial-concepts.html) · [PostgreSQL — Основы работы с таблицами](https://www.postgresql.org/docs/current/ddl-basics.html)

</details>

<details>
  <summary>INNER JOIN vs OUTER JOIN</summary>

### INNER JOIN vs OUTER JOIN

**Definition:** **INNER JOIN** возвращает только строки с совпадениями в обеих таблицах. **ВНЕШНЕЕ СОЕДИНЕНИЕ** (`LEFT` / `RIGHT` / `FULL`) также сохраняет несовпадающие строки и заполняет другую сторону NULL.

**How it works:**

INNER хранит только совпадения. OUTER сохраняет несовпадающие строки. `LEFT`/`RIGHT`/`FULL` означает ВНЕШНИЙ; тот `OUTER` ключевое слово является необязательным.

| Тип соединения | Возврат                                                          |
|  --------------  |  ----------------------------------------------------------------  |
| **ВНУТРЕННИЙ** | Строки с совпадениями в обеих таблицах                           |
| **СЛЕВА**      | Все левые строки + совпадение справа (NULL, если совпадений нет) |
| **ПРАВИЛЬНО**  | Зеркало ЛЕВОЕ |
| **ПОЛНЫЙ ВНЕШНИЙ** | Все строки из обоих, NULL, где нет совпадений |

Все виды соединений, включая CROSS: [Joins](#joins).

**When to use:** INNER для необходимых отношений; СЛЕВА, если дополнительные сопутствующие данные (например, пользователь без заказов). RIGHT — то же самое, что замена таблиц, а LEFT — предпочтение LEFT.

**Examples:**

```sql
SELECT u.id, u.email, o.total
FROM users u
INNER JOIN orders o ON o.user_id = u.id;

SELECT u.id, u.email, o.total
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;
```

**Thoughts:** Неправильный тип соединения = тихая потеря данных (INNER или LEFT).

**Sources:** [PostgreSQL — объединения](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN)

</details>

<details>
  <summary>NULL</summary>

### NULL

**Definition:** `NULL` — это специальный SQL-маркер для отсутствующих или неизвестных данных. Это не пустая строка и не ноль.

**How it works:**

- Большинство операций с `NULL` возвращаться `NULL` (неизвестный).
- `NULL = NULL` это **неправда**. В стандартном SQL выдается **unknown** (`NULL`), нет `FALSE`. `WHERE` обрабатывает неизвестное как «нет строки», поэтому оно _выглядит_ как ложное. Никогда не пишите `col = NULL`.

#### IS NULL / IS NOT NULL

- Тест на отсутствующие и текущие значения. PostgreSQL также поддерживает нестандартные постфиксы. `ISNULL` / `NOTNULL` (то же самое, что `IS NULL`).

#### COALESCE

- Возвращает первый аргумент, который не является `NULL`. Стандарт SQL. MySQL `IFNULL(a, b)` is `COALESCE(a, b)`.

#### ISNULL / IFNULL (other engines)

- MySQL `ISNULL(x)` → `1`/`0`. SQL-сервер `ISNULL(x, alt)` является заменой с двумя аргументами.
- У PostgreSQL нет `IFNULL`. Использовать `COALESCE`. Постфикс `x ISNULL` это **не** MySQL `ISNULL(x)`.

`IS DISTINCT FROM` / `IS NOT DISTINCT FROM` сравнить с NULL как с реальными значениями (`NULL IS NOT DISTINCT FROM NULL` это правда). Агрегаты пропускают NULL, за исключением `COUNT(*)`.

**When to use:** Дополнительные FK, даты неизвестны.

Предпочитать `NOT NULL` + `DEFAULT` когда значение всегда требуется.

**Examples:**

```sql
-- Unknown birth date
SELECT * FROM employees WHERE birth_date IS NULL;

-- Known birth date
SELECT * FROM employees WHERE birth_date IS NOT NULL;

SELECT COALESCE(nickname, name, '(none)') AS display FROM employees;
```

**Thoughts:** `WHERE col != 'x'` удаляет NULL-строки. Держите их с `col IS DISTINCT FROM 'x'`.

**Sources:** [PostgreSQL — Сравнение](https://www.postgresql.org/docs/current/functions-comparison.html) · [PostgreSQL — COALESCE](https://www.postgresql.org/docs/current/functions-conditional.html#FUNCTIONS-COALESCE-NVL-IFNULL) · [Академия SQL — COALESCE](https://sql-academy.org/ru/blog/sql-coalesce)

</details>

<details>
  <summary>CASE</summary>

### CASE

**Definition:** `CASE` реализует логику if/else в SQL. Он возвращает значение из первого `WHEN` условие, которое истинно, или из `ELSE`.

**How it works:**

#### Searched CASE

```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END
```

- Первая правда `WHEN` побеждает (порядок имеет значение: `>= 80000` до `>= 50000`).
- Пропускать `ELSE` и нет совпадений → `NULL`.

#### Simple CASE

- `CASE expr WHEN value THEN result … END` - как С `switch` о равенстве.

Типы результатов `THEN`/`ELSE` должны иметь общий тип. Действительно в `SELECT`, `WHERE`, `ORDER BY`, `UPDATE`. Неиспользуемые ветки обычно не оцениваются.

**When to use:** Диапазоны зарплат, сопоставление кодов с метками, условные агрегаты.

Пропустить, если таблица поиска является реальной моделью.

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

**Thoughts:** `FILTER (WHERE …)` по агрегатам часто более ясна, чем `SUM(CASE WHEN … THEN 1 END)`.

**Sources:** [PostgreSQL — CASE](https://www.postgresql.org/docs/current/functions-conditional.html#FUNCTIONS-CASE)

</details>

<details>
  <summary>COMMIT and ROLLBACK</summary>

### COMMIT and ROLLBACK

**Definition:** `COMMIT` завершает текущую транзакцию и делает ее изменения постоянными и видимыми для других. Вы не сможете отменить их впоследствии.

`ROLLBACK` прерывает текущую транзакцию. Все его изменения отбрасываются; база данных возвращается в состояние до начала блока.

**How it works:**

#### Start the transaction

- PostgreSQL: `BEGIN` or `START TRANSACTION` (`BEGIN TRANSACTION` это то же самое — `TRANSACTION` является необязательным).
- Без `BEGIN`, каждый оператор автоматически подтверждает успех.

#### COMMIT

- Фиксирует транзакцию.
- Изменения становятся устойчивыми и видимыми для других сеансов.
- После `COMMIT`, `ROLLBACK` не могу их отменить.

#### ROLLBACK

- Отменяет транзакцию.
- Все операторы в блоке отменены.

`SAVEPOINT` / `ROLLBACK TO` отменить только часть все еще открытого блока. Изоляция: [Транзакции и изоляция](./9_database_core.md#transactions-isolation-levels). Контракт: [ACID](#acid).

**When to use:** Многократное заявление пишет, что необходимо выполнить вместе (перевод денег).

Держите блок коротким.

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

**Thoughts:** КИСЛОТА — гарантия; это глаголы. Уровни изоляции: [Основы](./9_database_core.md#transactions-isolation-levels).

**Sources:** [PostgreSQL — Транзакции](https://www.postgresql.org/docs/current/tutorial-transactions.html) · [PostgreSQL — НАЧАТЬ](https://www.postgresql.org/docs/current/sql-begin.html) · [PostgreSQL — COMMIT](https://www.postgresql.org/docs/current/sql-commit.html) · [PostgreSQL — ROLLBACK](https://www.postgresql.org/docs/current/sql-rollback.html) · [Академия SQL — ACID](https://sql-academy.org/ru/blog/sql-acid)

</details>

<details>
  <summary>CHAR vs VARCHAR</summary>

### CHAR vs VARCHAR

**Definition:** `CHAR(n)` (`character(n)`) хранит строку фиксированной длины `n`. Более короткий ввод дополняется пробелами, чтобы `n`.

`VARCHAR(n)` (`character varying(n)`) хранит строку переменной длины до `n`. Он использует символы плюс небольшие накладные расходы на длину.

**How it works:**

#### CHAR(n)

- Всегда занимает заявленную ширину (включая отступы).
- Конечные пробелы незначительны на `CHAR` сравнение; они удаляются, когда вы выполняете кастинг `text`/`varchar`.
- Использование дампа: коды стран, почтовые индексы и другие действительно фиксированные коды.

#### VARCHAR(n)

- Сохраняет фактическую строку; дольше, чем `n` → ошибка (если только дополнительные символы не являются пробелами).
- Использование дампа: имена и другой переменный текст.
- PostgreSQL также имеет неограниченное количество `text` (предпочтительно для большинства столбцов). `varchar` без `(n)` тоже безлимит.

#### Memory and performance

- Дамп знаний: `CHAR` фиксирован и «быстрее»; `VARCHAR` экономит место, но «может быть немного медленнее».
- PostgreSQL: нет выигрыша в скорости `CHAR`. Заполнение требует дополнительного хранилища и процессора; `character(n)` обычно является **самым медленным**. Предпочитать `text` or `varchar`.

**When to use:** `CHAR` только для кодов действительно фиксированной ширины.

В противном случае `varchar(n)` or `text`.

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

**Thoughts:** Набивные сюрпризы `length()`, `LIKE`и конкатенация. По умолчанию `text` в Постгресе.

**Sources:** [PostgreSQL — Типы символов](https://www.postgresql.org/docs/current/datatype-character.html)

</details>

<details>
  <summary>Temporary tables</summary>

### Temporary tables

**Definition:** Временная таблица — это таблица, которая существует только для текущего сеанса (или соединения) и автоматически удаляется при завершении сеанса.

**How it works:**

- PostgreSQL: `CREATE TEMP TABLE` or `CREATE TEMPORARY TABLE`. Каждая сессия должна создавать свою собственную; имена могут совпадать в разных сеансах без конфликтов.
- Необязательный `ON COMMIT DROP` / `DELETE ROWS` / `PRESERVE ROWS` — удалить или очистить в конце транзакции, а не только в конце сеанса.
- Невидим для других сессий. Временная таблица с тем же именем, что и постоянная, выигрывает в `search_path` если вы не квалифицируете схему.
- Индексы во временной таблице также являются временными.
- SQL-сервер `#TempTable` / `##` префиксы **не** PostgreSQL. MySQL использует `CREATE TEMPORARY TABLE` без `#`.

**When to use:** Храните промежуточные результаты в сложном запросе.

Пакетная обработка больших данных поэтапно.

Избегайте конфликтов, когда несколько пользователей запускают один и тот же сценарий (локальные имена сеанса).

Пропустить, когда набор должен пережить повторное подключение — реальная таблица или `UNLOGGED`. Предпочитайте [CTE](#cte), если набор небольшой и состоит из одного запроса.

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

**Thoughts:** Пулы соединений повторно используют сеансы — оставшиеся временные таблицы могут вас удивить. `ANALYZE` временную таблицу, если вы присоединитесь к ней в большом запросе (автоочистка не сможет ее увидеть).

**Sources:** [PostgreSQL — СОЗДАТЬ ТАБЛИЦУ](https://www.postgresql.org/docs/current/sql-createtable.html)

</details>

<details>
  <summary>Window functions</summary>

### Window functions

**Definition:** **Оконные функции** выполняют вычисления по связанным строкам, не сворачивая набор результатов.

**How it works:**

| Окно                         | Роль                                           |
| ---------------------------- | ---------------------------------------------- |
| `ROW_NUMBER()`               | Уникальный 1…n в разделе                       |
| `RANK()`                     | Ничьи разделяют ранг; следующий номер пропущен |
| `DENSE_RANK()`               | Ничьи разделяют ранг; нет пробелов             |
| `LAG` / `LEAD`               | Значение предыдущей/следующей строки           |
| `FIRST_VALUE` / `LAST_VALUE` | Края оконной рамы                              |

`OVER (PARTITION BY … ORDER BY …)` определяет окно. Результаты окна не могут войти `WHERE` — обернуть подзапросом.

**When to use:** Рейтинги, текущие итоги, дедупликация (`ROW_NUMBER` + фильтр), средние значения по отделу рядом с каждой строкой о сотрудниках, нумерация страниц со стабильной сортировкой.

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

**Thoughts:** Оконные функции выполняются **после** WHERE/GROUP BY. `ROW_NUMBER` vs `RANK` vs `DENSE_RANK`: связывает пропуск номеров только с `RANK`.

**Sources:** [PostgreSQL — Оконные функции](https://www.postgresql.org/docs/current/tutorial-window.html) · [SQL Academy — Оконные функции](https://sql-academy.org/ru/guide/windows-functions) · [SQL Academy — ROW_NUMBER, RANK, DENSE_RANK](https://sql-academy.org/ru/blog/row-number-rank-dense-rank)

</details>

<details>
  <summary>CTE</summary>

### CTE

**Definition:** **CTE** (Общее табличное выражение) называет подзапрос с помощью `WITH` для удобства чтения и повторного использования.

**How it works:**

| Особенность                       | Подзапрос                  | КТР (`WITH`)                  |
| --------------------------------- | -------------------------- | ----------------------------- |
| Читабельность                     | Может глубоко вкладываться | Именный, линейный             |
| Повторное использование в запросе | Ограниченная               | Можно ссылаться несколько раз |
| Рекурсия                          | Нет                        | Да (`WITH RECURSIVE`)         |

**When to use:** Многоэтапные отчеты, организационные деревья, читаемые конвейеры.

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

**Thoughts:** Рекурсивные CTE используют `UNION` vs `UNION ALL` умышленно - `UNION` может остановить дублирование узлов графа.

**Sources:** [PostgreSQL — запросы С](https://www.postgresql.org/docs/current/queries-with.html) · [Академия SQL — Оператор С](https://sql-academy.org/ru/guide/operator-with) · [Академия SQL — CTE](https://sql-academy.org/ru/blog/sql-cte)

</details>

<details>
  <summary>DROP TABLE</summary>

### DROP TABLE

**Definition:** `DROP TABLE` удаляет объект таблицы вместе с его данными, индексами и ограничениями.

**How it works:**

- No `WHERE` — отношение пропало из каталога.
- Транзакции в PostgreSQL (может `ROLLBACK` в том же блоке).
- Необратимо после совершения, если вы не восстановите резервную копию.
- Удаляет индексы, триггеры и ограничения в **этой** таблице. Представления или FK **в других таблицах**, которые ссылаются на него, нужны. `CASCADE` (`CASCADE` удаляет зависимое представление или только ограничение FK, а не другую таблицу). По умолчанию `RESTRICT`.

**When to use:** Когда объект не должен существовать. Чтобы очистить строки и сохранить таблицу: [DELETE vs TRUNCATE](#delete-vs-truncate).

**Examples:**

```sql
DROP TABLE employees;
```

**Thoughts:** Сравните с DELETE/TRUNCATE в [DELETE, TRUNCATE и DROP](#delete-truncate-and-drop).

**Sources:** [PostgreSQL — DROP TABLE](https://www.postgresql.org/docs/current/sql-droptable.html) · [SQL Academy — TRUNCATE, DELETE и DROP](https://sql-academy.org/ru/blog/sql-truncate-delete-drop)

</details>

<details>
  <summary>ALTER TABLE</summary>

### ALTER TABLE

**Definition:** `ALTER TABLE` изменяет существующую таблицу: столбцы, ограничения, типы, значения по умолчанию — не удаляя таблицу.

**How it works:**

- `ADD COLUMN` добавляет столбец. `NOT NULL` для заполненной таблицы требуется `DEFAULT` (или трехэтапная засыпка).
- `DROP COLUMN`, `RENAME COLUMN`, `ALTER COLUMN … TYPE` переписать или обновить каталог в зависимости от изменения.
- Добавление ФК или `CHECK` сканирует существующие строки. `ALTER TABLE … ADD CONSTRAINT … FOREIGN KEY` — те же правила, что и для [Внешний ключ](#foreign-key).

**When to use:** Миграции. Предпочитают аддитивные изменения в производстве; большие перезаписи требуют плана блокировки.

**Examples:**

```sql
ALTER TABLE employees ADD COLUMN email TEXT;
ALTER TABLE employees
  ADD COLUMN date_of_birth DATE NOT NULL DEFAULT DATE '1900-01-01';
```

**Thoughts:** Развернуть-контракт: добавить столбец с нулевым значением → заполнение → установить NOT NULL. Избегайте длинных `ACCESS EXCLUSIVE` замки на больших таблицах.

**Sources:** [PostgreSQL — ALTER TABLE](https://www.postgresql.org/docs/current/sql-altertable.html) · [PostgreSQL — Добавление столбца](https://www.postgresql.org/docs/current/ddl-alter.html#DDL-ALTER-ADDING-A-COLUMN)

</details>

<details>
  <summary>Correlated subquery</summary>

### Correlated subquery

**Definition:** **коррелированный подзапрос** ссылается на столбцы из внешнего запроса и выполняется в контексте этой строки (типично `EXISTS` / сравнение по группам). Некоррелированные подзапросы выполняются один раз.

**How it works:**

- Коррелированная форма видит внешние столбцы; планировщик часто перезаписывает соединение, но наивный вложенный цикл — это N внутренних выполнений.

**When to use:** Когда внутренний запрос должен видеть текущую внешнюю строку — перепишите на JOIN/`GROUP BY` if `EXPLAIN` показывает катастрофу вложенного цикла.

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

**Thoughts:** Коррелированный подзапрос не всегда медленный — `EXISTS` может произойти короткое замыкание. Измерьте с помощью [EXPLAIN](#query-optimization-explain).

**Sources:** [PostgreSQL — Выражения подзапросов](https://www.postgresql.org/docs/current/functions-subquery.html) · [Академия SQL — Коррелированные подзапросы](https://sql-academy.org/ru/guide/correlated-subqueries)

</details>

<details>
  <summary>DELETE, TRUNCATE, and DROP</summary>

### DELETE, TRUNCATE, and DROP

**Definition:** Три способа удаления данных: **DELETE** удаляет соответствующие строки, **TRUNCATE** быстро очищает таблицу, **DROP TABLE** удаляет объект таблицы.

**How it works:**

| Команда      | Что он удаляет                                | `WHERE` | Триггеры                             | Транзакция                                         |
| ------------ | --------------------------------------------- | ------- | ------------------------------------ | -------------------------------------------------- |
| `DELETE`     | Выбранные строки                              | Да      | Ряд `DELETE` вызывает пожар          | Полностью транзакционный                           |
| `TRUNCATE`   | Все строки; необязательный `RESTART IDENTITY` | Нет     | Заявление `TRUNCATE` только триггеры | **Транзакционный в PostgreSQL** (может `ROLLBACK`) |
| `DROP TABLE` | Таблица + индексы + ограничения               | —       | Объект пропал                        | Транзакции в PostgreSQL                            |

`TRUNCATE` немедленно освобождает место и может `CASCADE` к таблицам, которые ссылаются на него.

**When to use:** `DELETE` для фильтрованной очистки. `TRUNCATE` чтобы очистить стол, который вы держите. `DROP` только тогда, когда объект не должен существовать.

**Examples:**

```sql
DELETE FROM employees WHERE salary < 30000;
TRUNCATE TABLE employees RESTART IDENTITY;
DROP TABLE employees;
```

**Thoughts:** «TRUNCATE не может ROLLBACK» — это MySQL/старый миф, а в PostgreSQL это возможно. Все еще предпочитаю `DELETE` когда тебе нужно `WHERE` или триггеры для каждой строки.

**Sources:** [PostgreSQL — УДАЛЕНИЕ](https://www.postgresql.org/docs/current/sql-delete.html) · [PostgreSQL — TRUNCATE](https://www.postgresql.org/docs/current/sql-truncate.html) · [PostgreSQL — УДАЛЕНИЕ ТАБЛИЦЫ](https://www.postgresql.org/docs/current/sql-droptable.html) · [Академия SQL — TRUNCATE, DELETE и DROP](https://sql-academy.org/ru/blog/sql-truncate-delete-drop)

</details>

<details>
  <summary>Self-join</summary>

### Self-join

**Definition:** **Самообъединение** — это соединение таблицы с самой собой с использованием псевдонимов, поэтому каждая роль является отдельным входом.

**How it works:**

- Типичный шаблон: самоссылка [внешний ключ](#foreign-key) (`manager_id → employee_id` на том же столе).
- `LEFT JOIN` сохраняет корни без родителя (`manager_id IS NULL`).
- Иерархические маршруты неизвестного использования глубины. `WITH RECURSIVE`, ни одного самосоединения.

**When to use:** Организационные диаграммы, цепочки комментариев глубиной на один уровень, «строки, соответствующие другим строкам». Пропустить, если рекурсивный CTE является реальным запросом.

**Examples:**

```sql
SELECT e.name AS employee, m.name AS manager
FROM employee e
LEFT JOIN employee m ON e.manager_id = m.employee_id;
```

**Thoughts:** Всегда используйте псевдонимы обеих сторон. Принудительное внутреннее соединение удаляет строку CEO/корневую строку.

**Sources:** [PostgreSQL — объединения](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-JOIN)

</details>

<details>
  <summary>Backup and restore</summary>

### Backup and restore

**Definition:** Резервное копирование и восстановление сохраняют копию базы данных, чтобы вы могли восстановить ее после сбоя или потери. Инструменты зависят от СУБД.

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

`-U` / `-u` является пользователем БД; `mydatabase` это база данных; `backup.sql` это файл дампа.

`pg_dump -Fc` это специальный формат для параллельного `pg_restore`. Резервное копирование физической базы + WAL включает PITR (в любое время в окне): [Backup & PITR](./9_database_production.md#backup-pitr-ops-note).

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

- **Права** — роли должно быть разрешено выполнять дамп и восстановление.
- **Расписание** — автоматизируйте регулярное резервное копирование.
- **Хранилище** — храните копии на основном сервере.
- **Тест** — восстановление на промежуточном сервере по расписанию.

Реплика не является резервной копией — она может скопировать неверную запись. Непроверенное восстановление не является резервной копией.

**When to use:** Ежедневные дампы SQL для небольших БД; Архивирование WAL, когда RPO составляет минуты. После `DROP TABLE` /плохое развертывание: восстановление или PITR.

**Thoughts:** Считайте файлы дампа секретами (пароли, личные данные). Определите RPO/RTO, прежде чем выбирать дамп или PITR.

**Sources:** [PostgreSQL — резервное копирование и восстановление](https://www.postgresql.org/docs/current/backup.html) · [PostgreSQL — pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html)

</details>

<details>
  <summary>Many-to-many</summary>

### Many-to-many

**Definition:** «Многие ко многим» означает, что одна строка в таблице A может быть связана со многими строками в таблице B, а одна строка в B может быть связана со многими строками в таблице A.

В SQL вы моделируете это с помощью **соединительной таблицы** (таблицы связей/мостов), которая содержит внешние ключи для первичных ключей обоих родителей.

**How it works:**

1. Создайте две таблицы сущностей.
2. Создайте соединительную таблицу с FK для обоих ПК.
3. Поместите в пару **составной первичный ключ**, чтобы каждая ссылка была уникальной, и FK для обеспечения ссылочной целостности.

Дополнительная полезная нагрузка на перекрестке (`enrolled_at`, `role`). Оба ФК часто используют `ON DELETE CASCADE` если ссылка должна умереть с любой стороны. См. [Внешний ключ](#foreign-key).

**When to use:** Студенты↔курсы, публикации↔теги, пользователи↔роли.

Пропустите внедрение массивов, если вам нужны ограничения, дополнительные столбцы или запросы с обеих сторон.

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

- Каждый `student_course` строка — одна пара регистрации.
- `PRIMARY KEY (student_id, course_id)` блокирует повторные регистрации.
- ФК отвергают сирот (студентов/курсов, которых не существует).

**Thoughts:** Тот же шаблон, что и Postgres `order_items (product_no, order_id)`. Prisma использует неявную таблицу M:N, если вы не моделируете соединение явно.

**Sources:** [PostgreSQL — внешние ключи](https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK)

</details>

<details>
  <summary>GRANT and REVOKE</summary>

### GRANT and REVOKE

**Definition:** `GRANT` и `REVOKE` управлять тем, кто и что может делать с объектами базы данных. Они дают или забирают привилегии у пользователей или ролей — DCL для контроля доступа.

**How it works:**

#### GRANT

- Предоставляет роли привилегии для объекта (таблицы, представления, схемы, базы данных, процедуры и т. д.).

```sql
GRANT privileges ON object TO user [WITH GRANT OPTION];
```

- **привилегии** — например. `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `ALL PRIVILEGES`.
- **object** — таблица, представление, схема,…
- **user** — имя роли (Postgres объединяет пользователей и группы в роли).
- **С ОПЦИЕЙ ГРАНТА** — грантополучатель может передать привилегию.

#### REVOKE

- Удаляет предоставленные ранее привилегии.

```sql
REVOKE privileges ON object FROM user;
```

- Зависимые гранты (передаваемые через `WITH GRANT OPTION`) нуждаться `CASCADE` или отзыв не удался (`RESTRICT` это значение по умолчанию).

Схема `USAGE` требуется для использования объектов внутри схемы — таблицы `GRANT` одного недостаточно. Гранты таблиц не распространяются на последовательности, используемые `SERIAL` /identity — предоставить их отдельно.

**When to use:** Роль приложения: DML только для необходимых таблиц.

Роль переносчика: DDL. Люди: группы, а не суперпользователи.

**Examples:**

```sql
GRANT SELECT ON employees TO user1;
REVOKE SELECT ON employees FROM user1;

GRANT SELECT, INSERT ON employees TO app_user;
REVOKE DELETE ON employees FROM app_user;
```

**Thoughts:** Наименьшие привилегии являются частью защиты от внедрения: украденный запрос не может `DROP TABLE` если роль не может. См. [SQL-инъекция](#sql-инъекция).

**Sources:** [PostgreSQL — GRANT](https://www.postgresql.org/docs/current/sql-grant.html) · [PostgreSQL — REVOKE](https://www.postgresql.org/docs/current/sql-revoke.html) · [PostgreSQL — Привилегии](https://www.postgresql.org/docs/current/ddl-priv.html)

</details>

<details>
  <summary>Hash functions</summary>

### Hash functions

**Definition:** Хэш-функции превращают входные данные любой длины в дайджест фиксированной длины. Используется для безопасности, проверки целостности, а иногда и для более быстрого сравнения/поиска.

Это не то же самое, что **хэш-индекс** (метод доступа). См. [Индексы](#indexes).

**How it works:**

#### 1. Password storage

- Храните дайджест, а не открытый текст. Предпочитаю **медленный, соленый** KDF (`crypt` + `gen_salt('bf')` in `pgcrypto`или Argon2/bcrypt в приложении).
- Информация о дампе часто отображается в сыром виде. `SHA-256` / `SHA-512` / SQL-сервер `HASHBYTES` — этого **недостаточно** для паролей (быстрых, часто несоленых).

#### 2. Integrity

- Хешировать полезную нагрузку; сравните позже. Изменить → дайджест изменений. Постгрес: `digest(data, 'sha256')`.

#### 3. Indexing / fast compare

- Дайджесты могут ускорить проверку равенства больших больших двоичных объектов. **хеш-индекс** — это отдельная альтернатива B-дерева для `=` поиск — это не криптохеш пароля.

#### 4. Content-derived ids

- Хэш контента в виде стабильного идентификатора (дедупликация, ключи кэша). Коллизии редки, но возможны — не рассматривайте SHA только как гарантию PK.

Хеширование является односторонним — вы не можете восстановить оригинал из дайджеста. Предпочитать `sha256`/`sha512` за целостность; избегать `md5`/`sha1` для безопасности. Предпочитайте хеширование паролей в **приложении**, чтобы БД никогда не видела открытый текст.

**When to use:** Контрольные суммы, HMAC, столбцы с солеными паролями, если вам необходимо хешировать в SQL.

Пропустите простой SHA пароля.

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

**Thoughts:** Ловушка на собеседовании: `HASHBYTES` / несоленый ША. Угроза = оффлайн-грубая сила; fix = соль + медленный KDF (или Argon2 на стороне приложения).

**Sources:** [PostgreSQL — pgcrypto](https://www.postgresql.org/docs/current/pgcrypto.html)

</details>

<details>
  <summary>How SQL triggers work</summary>

### How SQL triggers work

**Definition:** Триггер SQL — это объект базы данных — специальная хранимая процедура, которая запускается автоматически при возникновении события, например `INSERT`, `UPDATE`, or `DELETE` на столе или в представлении.

**How it works:**

Триггер срабатывает сам по себе при возникновении события (например, при вставке новой строки).

#### BEFORE

- Бегает перед операцией.
- Проверить или изменить данные перед их сохранением (`NEW` можно изменить; `RETURN NULL` пропускает строку).

#### AFTER

- Запускается после успешного завершения операции.
- Журналы аудита, обновление связанных таблиц.

#### INSTEAD OF

- Заменяет действие по умолчанию.
- Типично для представлений, которые не подлежат автоматическому обновлению.

Триггеры строк см. `OLD` / `NEW` (Postgres: записи в PL/pgSQL). `INSERT` не имеет `OLD`; `DELETE` не имеет `NEW`. Использовать `IS DISTINCT FROM` для NULL-безопасного сравнения зарплат.

PostgreSQL: `CREATE FUNCTION … RETURNS trigger`, затем `CREATE TRIGGER … EXECUTE FUNCTION …`. MySQL может поставить `BEGIN … END` in `CREATE TRIGGER` сам. Обзор + DML против DDL: [Триггеры](#триггеры).

**When to use:** История аудита/изменений.

Ограничения сложных правил целостности не могут быть выражены.

Синхронизируйте связанные таблицы.

Производные/агрегированные столбцы, которые должны синхронизироваться.

Пропустить, когда путь записи должен принадлежать службе приложения — триггеры скрывают побочные эффекты от ORM.

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

**Thoughts:** `TRUNCATE` не стреляет `DELETE` триггеры строк — см. [DELETE, TRUNCATE и DROP](#delete-truncate-and-drop).

**Sources:** [PostgreSQL — СОЗДАНИЕ ТРИГГЕРА](https://www.postgresql.org/docs/current/sql-createtrigger.html) · [PostgreSQL — Триггерные функции](https://www.postgresql.org/docs/current/plpgsql-trigger.html)

</details>

<details>
  <summary>Upsert (ON CONFLICT)</summary>

### Upsert (ON CONFLICT)

**Definition:** **Upsert** вставляет строку или обновляет ее, если уникальное ограничение конфликтует — атомарное «создать или обновить».

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

- `ON CONFLICT (columns)` нацелен на уникальное ограничение.
- `DO NOTHING` — пропустить молча (идемпотентная вставка).
- `DO UPDATE` — слиться; `EXCLUDED` относится к строке, которая должна была быть вставлена.

**When to use:** Синхронизация внешних идентификаторов, вход в систему «создать, если не существует», строка настроек для каждого пользователя, дедуплицированный прием событий.

**Pluses and minuses:**

| +                                                                         | −                                              |
| ------------------------------------------------------------------------- | ---------------------------------------------- |
| Один рейс туда и обратно, безопасно для гонок или «проверь, потом вставь» | Должен иметь соответствующий уникальный индекс |
| Идемпотентные задания синхронизации/импорта                               | Легко перезаписать поля непреднамеренно        |

**Examples:**

```ts
await prisma.user.upsert({
  where: { email: "a@b.com" },
  create: { email: "a@b.com", name: "Ada" },
  update: { name: "Ada Lovelace" },
});
```

**Thoughts:** MySQL использует `ON DUPLICATE KEY UPDATE`. SQLite поддерживает `ON CONFLICT` сходным образом. Без ограничения уникальности upsert недоступен — сначала добавьте его.

**Sources:**

[PostgreSQL — ВСТАВКА ПРИ КОНФЛИКТЕ](https://www.postgresql.org/docs/current/sql-insert.html#SQL-ON-CONFLICT)

---

</details>

<details>
  <summary>Prepared statements & placeholders</summary>

### Prepared statements & placeholders

**Definition:**

**Подготовленный оператор** отправляет SQL с заполнителями (`$1`, `?`) один раз; значения привязываются отдельно, что предотвращает внедрение SQL.

| Подход                      | Безопасный? | Заметки                             |
| --------------------------- | ----------- | ----------------------------------- |
| Параметризованный запрос    | Да          | `$1`, `?` заполнители               |
| методы ОРМ                  | Да          | Призма `findUnique`, крепления Knex |
| Конкатенация строк          | Нет         | `'${email}'` уязвим                 |
| `$queryRaw` шаблон с тегами | Да          | Шаблоны с тегами Prisma             |

Цитаты и `;` в связанном значении остаются **data**. Идентификаторы (имена таблиц/столбцов) не могут быть заполнителями — внесите их в белый список.

**When to use:** Каждый запрос со значениями, управляемыми пользователем.

**Examples:**

```ts
await pool.query("SELECT * FROM users WHERE email = $1", [email]);
await prisma.$queryRaw`SELECT * FROM users WHERE email = ${email}`;
```

**Thoughts:** Заполнители обрабатывают только значения — никогда не параметризуйте имена таблиц или столбцов.

**Sources:** [OWASP — Предотвращение SQL-инъекций](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

</details>

<details>
  <summary>SQL Server (T-SQL)</summary>

### SQL Server (T-SQL)

**Definition:** **T-SQL** (Transact-SQL) — диалект SQL в Microsoft SQL Server. Тот же реляционный core, что в PostgreSQL (`SELECT`, `JOIN`, `GROUP BY`, транзакции), но другой синтаксис лимитов, upsert, дат и временных объектов.

**How it works:**

Базовый SQL переносится: joins, подзапросы, CTE, оконные функции, `BEGIN` / `COMMIT` / `ROLLBACK`. На собеседованиях и в миграциях ловите диалектные ловушки:

| Задача            | PostgreSQL                         | SQL Server (T-SQL)                                      |
| ----------------- | ---------------------------------- | ------------------------------------------------------- |
| Лимит строк       | `LIMIT 10` / `OFFSET 20`           | `TOP (10)` или `ORDER BY … OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY` |
| Auto-increment PK | `SERIAL` / `GENERATED … AS IDENTITY` | `IDENTITY(1,1)` на колонке                           |
| Upsert            | `INSERT … ON CONFLICT DO UPDATE`   | `MERGE` (или отдельный try/update)                      |
| Текущее время     | `now()` / `CURRENT_TIMESTAMP`      | `GETDATE()` / `SYSDATETIME()`                           |
| Замена NULL       | `COALESCE(a, b)` (переносимо)      | `ISNULL(a, b)` (только два аргумента)                   |
| Конкатенация      | `\|\|` или `concat()`              | `+` (NULL «отравляет» результат — используйте `CONCAT()`) |
| Временная таблица | `CREATE TEMP TABLE …`              | `#local` (сессия) / `##global` (общая)                  |
| Boolean           | нативный `BOOLEAN`                 | `BIT` (0/1)                                             |
| Кавычки идентификаторов | двойные `"col"`              | квадратные `[col]` или `"col"` в quoted mode            |

**When to use:** Enterprise .NET, Azure SQL, legacy on-prem SQL Server. Postgres-командам T-SQL всё равно нужен для собеседований и интеграций.

**Pluses and minuses:**

| +                                      | −                                           |
| -------------------------------------- | ------------------------------------------- |
| Богатые инструменты (SSMS, Actual Plan) | `MERGE` имеет edge-case баги — тестируйте    |
| Сильные CTE и window functions         | Лицензирование / vendor lock-in на Azure    |
| Общий SQL core в Microsoft-стеке       | Диалект ломает ORM и миграции               |

**Examples:**

```sql
-- Топ клиентов по числу заказов (T-SQL)
SELECT TOP (10)
  u.email,
  COUNT(o.id) AS order_count
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
WHERE u.created_at >= '2025-01-01'
GROUP BY u.id, u.email
HAVING COUNT(o.id) > 5
ORDER BY order_count DESC;

-- Upsert через MERGE
MERGE users AS target
USING (SELECT @email AS email, @name AS name) AS source
ON target.email = source.email
WHEN MATCHED THEN UPDATE SET name = source.name, updated_at = GETDATE()
WHEN NOT MATCHED THEN INSERT (email, name, updated_at)
  VALUES (source.email, source.name, GETDATE());
```

**Thoughts:** `#temp` участвует в статистике и часто выигрывает у table variables на больших наборах. Планы: [Query optimization & EXPLAIN](#query-optimization-explain). Модель индексов: [SQL Server indexes (clustered vs nonclustered)](#sql-server-indexes-clustered-vs-nonclustered). Postgres: [PostgreSQL](#postgresql).

**Sources:** [T-SQL language reference](https://learn.microsoft.com/en-us/sql/t-sql/language-reference) · [MERGE (Transact-SQL)](https://learn.microsoft.com/en-us/sql/t-sql/statements/merge-transact-sql) · [SELECT — TOP clause](https://learn.microsoft.com/en-us/sql/t-sql/queries/top-transact-sql)

---

</details>

<details>
  <summary>SQL Server indexes (clustered vs nonclustered)</summary>

### SQL Server indexes (clustered vs nonclustered)

**Definition:** В SQL Server индексы делятся на **clustered** (задаёт порядок строк таблицы; не более одного на таблицу) и **nonclustered** (отдельная структура с указателями на строки). В PostgreSQL нет clustered index в этом смысле — см. [Indexes](#indexes).

**How it works:**

| Тип             | Что это                                         | Типичное использование               |
| --------------- | ----------------------------------------------- | -------------------------------------- |
| **Clustered**   | Данные таблицы хранятся в порядке ключа индекса | PK — range scan, `ORDER BY` по PK      |
| **Nonclustered**| B-tree ключа + указатель на строку               | `WHERE` / `JOIN` по не-PK колонкам     |
| **Included cols** | Неключевые колонки в листе (`INCLUDE`)      | Covering index — без key lookup        |
| **Columnstore** | Сжатые column batches                           | Аналитика / большие агрегаты           |

Отличия от Postgres:

- Только **один** clustered index на таблицу (часто PK).
- Nonclustered на heap использует **RID lookup**; на clustered table — **key lookup** за недостающими колонками.
- Filtered index: `CREATE INDEX … WHERE deleted_at IS NULL` (как partial index в Postgres).
- `INCLUDE (col)` — covering-паттерн SQL Server; в Postgres B-tree тоже поддерживает `INCLUDE`.

**When to use:** Clustered на основной путь доступа (обычно PK). Nonclustered на FK и фильтры. Columnstore для DW/отчётов — не default OLTP.

**Pluses and minuses:**

| +                                   | −                                              |
| ----------------------------------- | ---------------------------------------------- |
| `INCLUDE` уменьшает key lookup      | Неверный clustered key бьёт по INSERT/split    |
| Filtered indexes экономят maintenance | Много nonclustered замедляет запись          |
| Columnstore для scan-heavy analytics | Columnstore не для point lookup               |

**Examples:**

```sql
-- Clustered PK (по умолчанию при PRIMARY KEY)
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

-- Filtered (soft-delete)
CREATE NONCLUSTERED INDEX idx_users_active_email
ON users (email)
WHERE deleted_at IS NULL;
```

**Thoughts:** На собеседовании: «Clustered = порядок таблицы, один на таблицу; nonclustered = доп. индексы для seek». Планы: [Query optimization & EXPLAIN](#query-optimization-explain). Общие принципы: [Indexes](#indexes).

**Sources:** [Clustered and nonclustered indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described) · [Create nonclustered indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/create-nonclustered-indexes) · [Columnstore indexes](https://learn.microsoft.com/en-us/sql/relational-databases/indexes/columnstore-indexes-overview)

---

</details>

<details>
  <summary>PostgreSQL</summary>

### PostgreSQL

**Definition:** СУБД с открытым исходным кодом, сильным ACID, расширяемостью (JSONB, PostGIS, pgvector) и расширенным SQL (CTE, оконные функции, консультативные блокировки).

**How it works:** Парсер → переписчик (представления/RLS) → планировщик (индексы, тип соединения) → исполнитель. Расширения (`pgcrypto`, PostGIS, pgvector) добавляют типы и функции в процессе.

**When to use:** Выбор по умолчанию для новых серверных проектов — OLTP, смешанный реляционный + JSON, полнотекстовый поиск (с расширениями).

**Pluses and minuses:**

| +                                         | −                                                                                                                   |
|  -----------------------------------------  |  -------------------------------------------------------------------------------------------------------------------  |
| Богатый набор функций, отличный JSONB     | Первичный с одним записывающим устройством — для масштабирования записи требуется репликация сегментирования/чтения |
| Зрелая экосистема расширений | Настройка требует знаний администратора базы данных в большом масштабе |
| Сильное сообщество и возможности хостинга | Ограничения на соединения — используйте пул (PgBouncer)                                                             |

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

- **Консультационные блокировки** — мьютекс на уровне приложения: `SELECT pg_advisory_lock(123)`.
- **LISTEN/NOTIFY** — облегченная публикация/подписка внутри Postgres (не замена Redis/Kafka).
- **pgvector** — поиск по сходству векторов без отдельной векторной БД для среднего масштаба.

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

**Sources:** [Документация PostgreSQL](https://www.postgresql.org/docs/current/)

---

</details>
