# Databases — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [Databases — Interview](./0_sd_databases_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Databases](https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/).

### Database Fundamentals

<details>
<summary>What is a database management system (DBMS), and can you name some examples?</summary>

### What is a database management system (DBMS), and can you name some examples?

**Definition:**

A Database Management System (DBMS) serves as an intermediary between users and the database. It facilitates data maintenance, retrieval, and ongoing management, using a structured mechanism to ensure data integrity, security, and efficiency.

**How it works:**

#### Key Features

- **Data Modeling**: Organizes data into logically structured tables and relationships.

- **Data Manipulation**: Allows for CRUD operations (Create, Read, Update, Delete), usually via a query language.

- **Data Integrity**: Enforces referential integrity and ensures consistent data.

- **Security & Access Control**: Regulates user permissions to the data.

- **Data Concurrency**: Manages simultaneous data access by multiple users to avoid conflicts.

- **Backup & Recovery**: Provides mechanisms for data restoration in case of loss or corruption.

- **Data Analysis & Reporting**: Often includes tools for query optimization and report generation.

#### Types of DBMS

- **Relational DBMS (RDBMS)**: Organizes data into tables with predefined structures defined by a schema. SQL (Structured Query Language) is typically used for data operations. Common examples include MySQL, PostgreSQL, and Microsoft SQL Server.

- **NoSQL DBMS**: Evolved as a response to the limitations of RDBMS, designed for unstructured or semi-structured data and horizontal scalability. Examples include MongoDB for document-oriented storage and Redis for key-value stores.

- **Cloud-Based DBMS**: Hosted on cloud platforms, these systems provide flexibility and scalability, requiring minimal maintenance from users. Notable examples are Amazon RDS, Google Cloud Bigtable, and Azure Cosmos DB.

- **NewSQL DBMS**: Combines the benefits of traditional RDBMS with modern needs like scalability. These systems often offer improved performance, designed for big data scenarios. Examples include Google Cloud Spanner and NuoDB.

- **Object-Oriented DBMS (OODBMS)**: Designed for managing complex, object-based data models. They provide persistence for objects, disentangling the object structure from a relational schema. ODBMS are less popular in current times, but examples include db4o and ObjectStore.

- **In-memory DBMS**: Maintains data in the system’s memory, enabling rapid access and processing. Examples include Oracle TimesTen and Redis.

- **Multimodel DBMS**: Can handle multiple kinds of databases, such as key-value stores, document stores, and graph databases. This offers a variety of data models in a single system. ArangoDB is an example of such a system.

- **Graph DBMS**: Specialized for dealing with interconnected data elements. They are optimized for operations like traversals and pathfinding. Neo4j is a well-known example of this type of DBMS.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Explain the ACID properties in the context of databases.</summary>

### Explain the ACID properties in the context of databases.

**Definition:**

In database management, ACID ensures that transactions are processed in a reliable, standardized manner.

**How it works:**

#### ACID Properties

#### Atomicity

Atomicity ensures that all tasks in a transaction are completed or none are. Databases use transaction logs to manage atomicity. If a task fails, the transaction log allows the system to recognize the incomplete state and restore to the last known consistent state.

- **Consider a banking transfer**: if the debit is successful but the credit fails, atomicity ensures that the debit is also rolled back.

#### Consistency

Consistency requires that a transaction takes the database from one consistent state to another consistent state. It ensures that data integrity rules are not violated. For example, after a transfer, the sum of account balances should remain the same.

#### Isolation

Isolation ensures that the operations within concurrent transactions are invisible to each other until they are completed, to protect against potential conflicts and inconsistencies.

Different isolation levels, like read uncommitted, read committed, repeatable read, and serialize, define the extent to which transactions are isolated from one another.

#### Durability

Durability guarantees that once a transaction is committed, its changes persist, even in the event of a system failure. This is typically achieved through mechanisms such as write-ahead logging and buffer management, which ensure changes are written to disk.

**When to use:**

#### ACID in Practical Scenarios

- **Banking Applications**: ACID ensures that monetary transactions are secure and reliable.

- **Inventory Management**: When goods are purchased, inventory levels are updated consistently, without corruption or errors.

- **Scheduling Systems**: Activities, such as booking appointments, either take place entirely or not at all, avoiding messy partial bookings.

- **E-commerce Order Processing**: The entire cycle, from a customer placing an order to its fulfilment, is a cohesive and consistent unit of work.

- **Messaging Services**: For example, ensuring that a message is sent only after it is completely composed, not mid-way.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What are the differences between SQL and NoSQL databases?</summary>

### What are the differences between SQL and NoSQL databases?

**Definition:**

SQL and NoSQL databases vary in several key aspects. Here, I will elaborate on five of them.

**How it works:**

#### Key Distinctions

- **Data Structure**: SQL databases are table-based, whereas NoSQL databases can be document, key-value, wide-column, or graph-oriented.

- **Schema**: SQL databases are schema-based. They necessitate database schema, and any deviation requires schema modifications. NoSQL databases are either ad-hoc, making each document consistent with one another, or schema-on-read.

- **Querying**: SQL databases employ Structured Query Language to execute queries. NoSQL databases use methods specific to their data model.

- **Scalability**: SQL databases historically have employed vertical scaling or "scaling up" by increasing CPU power, memory, and storage on a single node. However, recent trends show support for horizontal scaling or "scaling out" across multiple nodes. NoSQL databases, by nature, support horizontal scaling more readily, making them "scale-out" architectures.

- **ACID**: Traditional SQL databases often guarantee ACID (Atomicity, Consistency, Isolation, and Durability) compliance. NoSQL databases, especially in eventual consistency models, might trade off immediate consistency for performance and availability.

#### Evolution and Adaptation

- **Consistency Models**: SQL often indicates immediate or strict consistency. NoSQL provides a range of consistency models, from eventual consistency to strong consistency.

- **Data Relationships and Transactions**: SQL databases typically enforce data relationships using methods such as foreign keys and support more complex transactional behavior. NoSQL databases might sacrifice some of these features for greater performance.

- **Use Cases and Popularity**: SQL databases are time-tested, especially in cases that need strict consistency or have clear data relationships. NoSQL databases are popular for their flexibility, particularly in rapidly changing or expansive data needs, and within the realm of big data and other modern computing paradigms.

#### Domain Adherence

- **Relational Integrity**: SQL databases pride themselves on ensuring referential integrity in relational data.

- **Efficiency in Certain Query Workloads**: SQL databases are frequently favored in scenarios that involve complex querying or multi-table joins because of their optimizer capabilities. NoSQL excels in certain types of queries, like key-value lookups, due to their data models.

- **Ease of Horizontal Scaling**: NoSQL is often the preferred choice in setups requiring high availability and distributed data.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Describe a relational database schema.</summary>

### Describe a relational database schema.

**Definition:**

A Relational Database Schema is a set of rules that define the structure, constraints, and relationships of data tables in a Relational Database Management System (RDBMS).

**How it works:**

#### Core Components

- **Tables**: Central data containers often referred to as "relations."

Defined by column names and data types.

- **Columns**: Attributes or fields, defined by their name, type, and optional constraints.

Each column should have a unique name within the table.
Common data types include integers, strings, dates, and more.

- **Rows**: Individual data records, consisting of data based on the table's defined columns.

Each row should have a unique identifier, often referred to as a "primary key".

#### Key Types

- **Primary Key (PK)**: A unique identifier for each record within a table. It's often an auto-incrementing integer. Each table should have precisely one primary key. This is Essential designated as “E” in databases.

- **Foreign Key (FK)**: A field or a group of fields in a table that uniquely identifies a row in another table. Commonly a primary key from another table, these help establish relationships between tables. This term is derived as “F” from Candidate Key in databases

#### Relationship Types

- **One-to-One**: Each record in the first table is related to one and only one record in the second table, and vice versa.

- **One-to-Many**: A record in the first table can be related to one or more records in the second table, but a record in the second table is related to only one record in the first table.

- **Many-to-Many**: Records in both tables can be related to multiple records in the other table. This relationship requires a linking table.

#### Common Constraints

- **NOT NULL**: Specifying that a column must have a value and cannot be left empty. This is Essential designated as “E” in databases.

- **UNIQUE**: Requires all entries in a column or set of columns to be distinct.

- **CHECK**: Allows for conditional checks to be applied to column data.

- **DEFAULT**: Automatically provides a predetermined value when a new column is created.

- **INDEX**: Optimizes data retrieval by creating an index on the column(s), speeding up relevant queries.

**Examples:**

#### Code Example: Schema for University Database

Here is the SQL code:

```sql
-- Table for students
CREATE TABLE Students (
  StudentID INTEGER PRIMARY KEY,
  Name TEXT NOT NULL,
  Age INTEGER CHECK (Age >= 18),  
  MajorID INTEGER,
  FOREIGN KEY (MajorID) REFERENCES Majors(MajorID)
);

-- Table for courses
CREATE TABLE Courses (
  CourseID INTEGER PRIMARY KEY,
  Title TEXT NOT NULL,
  Credits INTEGER CHECK (Credits >= 0)
);

-- The table that maps the many-to-many relationship between Students and Courses
CREATE TABLE Enrollments (
  StudentID INTEGER,
  CourseID INTEGER,
  EnrollmentDate DATE DEFAULT CURRENT_DATE,
  PRIMARY KEY (StudentID, CourseID),
  FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
  FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

-- The table that holds the list of possible majors
CREATE TABLE Majors (
  MajorID INTEGER PRIMARY KEY,
  Name TEXT NOT NULL UNIQUE
);
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What is a primary key, and why is it important?</summary>

### What is a primary key, and why is it important?

**Definition:**

A primary key is a unique identifier for a table. It ensures each data record in a table is distinct and can be efficiently referenced.

**How it works:**

#### Core Features

- **Uniqueness**: Every record in a table must have a unique primary key.

- **Non-Null Value**: A primary key field cannot contain null or undefined values.

- **Stability**: Changes to primary key values should be rare, ensuring its reliability as an identifier.

#### Benefits

- **Data Integrity**: Ensures accuracy and consistency within the dataset.

- **Data Retrieval**: Helps in efficient data retrieval and can easily reference related data tables through Foreign Keys.

- **Data Normalization**: Aids in organizing data across multiple tables, reducing redundancy and improving data integrity.

- **Table Relationships**: Serves as a basis for establishing multiple types of relationships with other tables (one-to-one, one-to-many, many-to-many).

#### RDBMS Requirements

#### MySQL

- **Character Limit**: 1000 characters in total across all the columns defined for a primary key.

- **Restrictions**: Text and blob columns are not permitted in primary keys.

- **Supported Types**: Can use most column types, including integers, strings, and dates.

#### PostgreSQL

- **Character Limit**: 32 characters for the name of the primary key constraint, which defaults to table_name_pkey.

- **Supported Types**: Most column types can be defined as primary keys, including UUID.

#### SQL Server

- **Character Limit**: 900 bytes.

- **Restrictions**: Text and image columns are forbidden in primary keys.

- **Supported Types**: The uniqueidentifier data type is commonly used as a primary key.

#### SQLite

- **Character Limit**: No fixed-length requirement.

- **Supported Types**: Most column types, including integers, can be used for primary keys.

#### Oracle Database

- **Character Limit**: The PRIMARY KEY constraint name, combined with the schema name, cannot exceed 30 characters.

- **Supported Types**: Most column data types can be used, including LOB columns.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Can you explain what a foreign key is and its role in the database?</summary>

### Can you explain what a foreign key is and its role in the database?

**Definition:**

A foreign key (FK) establishes a relationship between two tables in a relational database.

**How it works:**

#### Key Attributes

- **Column Type**: The FK column in the child table must have the same data type as the primary key column in the parent table.

- **Referential Integrity**: The FK ensures data consistency by either rejecting the change, setting it to NULL, or cascading the changes.

- **Joining Tables**: Utilizing the foreign key in SQL queries helps combine related data from multiple tables.

JOIN statement is the SQL command central to integrating tables through foreign keys:

#### Join Types

- **Inner Join**: Matches only the records having related entries in both tables.

- **Outer Join**: Includes records from one table even if there are no related entries in the other table.

#### Verifying Key Relationships

- **SYS.TYPE_CATALOG uses system views for all three types of keys**: primary, foreign, and unique. It displays tables with their corresponding keys.

INFORMATION_SCHEMA.KEY_COLUMN_USAGE furnishes comprehensive key information, such as the table containing the keys, as well as the cardinality.

**Examples:**

#### Examples and Code Explanation

Here is the SQL code:

```sql
-- 1. Parent Table Creation
CREATE TABLE authors (
    author_id INT PRIMARY KEY,
    author_name VARCHAR(100)
);

-- 2. Child Table Creation with Foreign Key
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    book_name VARCHAR(255),
    author_id INT,
    FOREIGN KEY (author_id) REFERENCES authors(author_id)
);

-- 3. Data Insertion
INSERT INTO authors (author_id, author_name) VALUES (1, 'J.K. Rowling');

INSERT INTO books (book_id, book_name, author_id) VALUES (1, 'Harry Potter', 1);

-- 4. Select Query for Data Retrieval
SELECT book_name, author_name FROM books 
JOIN authors ON books.author_id = authors.author_id;
```

In this code, author_id in the books table is a foreign key pointing to the author_id in the authors table. The SELECT statement employs a JOIN to unify the related book and author data.

#### For instance, using SQL SERVER:

```sql
SELECT
  TABLE_NAME, 
  COLUMN_NAME, 
  CONSTRAINT_NAME
FROM 
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE 
WHERE 
  TABLE_NAME = 'books';
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What is database normalization, and why do we use it?</summary>

### What is database normalization, and why do we use it?

**Definition:**

Database normalization is a set of practices that ensure data integrity by minimizing redundancy and dependency within a relational database.

**How it works:**

#### Advantages of Normalization

- **Data Consistency**: Reduction in redundancy decreases the risk of inconsistent data.

- **Improved Maintainability**: With a more structured database, maintenance becomes more straightforward.

- **Easier Updates**: Normalization usually means fewer records to update.

#### Normalization Levels

There are generally six levels of normalization, from 0 to 5 or "BCNF".

#### First Normal Form (1NF)

- **Unique Primary Keys**: A table should have a unique primary key for each record.

- **Atomic Values**: Each cell in a table should hold a single value, eliminating multi-valued attributes.

#### Second Normal Form (2NF)

All requirements of the previous form, as well as:

- **Removal of Partial Dependencies**: Non-primary key columns should depend on the entire primary key.

#### Third Normal Form (3NF)

All requirements of 2NF, as well as:

- **Elimination of Transitive Dependencies**: Non-primary key columns should not be dependent on other non-primary key columns.

#### Boyce-Codd Normal Form (BCNF)

A stricter version of 3NF that ensures each determinant is a candidate key.

#### Fourth Normal Form (4NF)

Deals with multi-valued dependencies.

#### Fifth Normal Form (5NF)

Also called "Projection-Join Normal Form" and deals with join dependencies.

**When to use:**

#### Normal Forms and Use-Cases

Most relational databases aim for 3NF, as it typically strikes a good balance between performance and data integrity.

However, it's essential to understand the specific requirements of your database and decide on an optimal normalization level accordingly. For instance, reporting databases might not be heavily normalized to improve query performance.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What is denormalization and when would you consider it?</summary>

### What is denormalization and when would you consider it?

**Definition:**

Denormalization is the process of reducing normalization (typically for performance reasons) by introducing redundant data into one or more tables. While normalization ensures data integrity, denormalization mainly focuses on improving query performance by eliminating complex JOIN operations.

**How it works:**

#### Benefits and Drawbacks

- **CTEs**: With multiple Common Table Expressions, maintaining benefits of a normalized schema can be achieved in a denormalized setup, potentially eliminating the drawbacks.

- **Maintainability**: Denormalization can make data harder to manage and keep consistent.

- **Query Performance**: Queries may become more efficient, as data required from different normalized tables is now available in a single denormalized table.

- **Storage Efficiency**: Queries that access multiple smaller tables require less disk I/O and can fit into the memory more easily than queries accessing a few larger tables.

- **Data Integrity**: Redundant data can lead to inconsistencies if not managed properly.

**When to use:**

#### Case-by-Case Considerations

- **Lookup Tables**: Tables with relatively static data (like Status types or Location information) that are frequently joined with other tables in your system, are strong candidates for denormalization. This can make frequent lookups faster.

- **Reporting and Analytics**: Systems that are skewed towards read operations over write operations benefit from denormalization. When designing systems for reporting or analytics, it's common practice to have dedicated read replicas that are denormalized for improved read performance.

- **Partitioning**: In large systems, denormalization can be used to partition data across different tables or clusters to distribute load. For example, customer data can be segregated into one cluster, and order data into another, linked via denormalization.

**Examples:**

#### Code Example: Denormalization and Normalization

Here is the SQL code:

```sql
-- Example of Normalized Tables
Customer Table: 
| ID | Name  | StatusID |
| 1  | John  | 2        |
| 2  | Jane  | 1        |

Status Table:
| StatusID | Description |
| 1        | Pending     |
| 2        | Active      |

-- Querying the Status using JOIN
SELECT c.Name, s.Description AS Status FROM Customer c
JOIN Status s ON c.StatusID = s.StatusID;

-- Denormalized Table
Customer Table (Denormalized):
| ID | Name  | Status   |
| 1  | John  | Active   |
| 2  | Jane  | Pending  |

-- Eliminating JOINs in the Query
SELECT Name, Status FROM Customer;
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Compare and contrast the DROP, DELETE, and TRUNCATE commands.</summary>

### Compare and contrast the DROP, DELETE, and TRUNCATE commands.

**Definition:**

Let's look at the three commands - DROP, DELETE and TRUNCATE in terms of their function, operation, speed and recovery.

The DROP TABLE statement deletes an entire table and its structure from the database.

**How it works:**

#### Transaction Control

- **Implicit Commit**: The action is immediately committed, and its effects are irreversible.

#### Considerations

- **Irreversible**: Drops the table as a whole; it doesn't delete records row by row.

- **Data Loss**: Any data in the table is permanently removed.

The DELETE statement operates on individual rows in the table and is reversible.

#### Transaction Control

- **Needs a Transaction**: Works within a transaction and needs to be explicitly committed to become permanent.

#### Considerations

- **Precision**: Removes specific rows based on the provided condition.

- **Recoverability**: Data can be undeleted or restored during the transaction if not committed.

The TRUNCATE TABLE statement quickly removes all rows from a table, providing both speed and simplicity. It doesn't, however, release allocated storage like DELETE does.

#### Transaction Control

- **Requires Commit**: Like DELETE, this statement operates within a transaction and requires explicit commitment to finalize its actions.

#### Considerations

- **Efficiency**: Processes significantly faster compared to DELETE. Ideal for removing all records and resetting table states in certain applications.

- **No Selective Deletion**: Unlike DELETE, it clears all records in the table without regard for specific conditions.

- **Transaction and Recovery**: Upon execution, the data removal can usually be undone or rolled back until a final commit or termination of the transaction.

**Examples:**

```sql
DROP Table
```

#### SQL Syntax

```sql
DROP TABLE table_name;
```

```sql
DELETE from Table
```

#### SQL Syntax

```sql
DELETE FROM table_name WHERE condition;
```

```sql
TRUNCATE Table
```

#### SQL Syntax

```sql
TRUNCATE TABLE table_name;
```

#### Code Example: Table Cleanup Operations

Here is the PostgreSQL code:

```sql
-- Create a sample table
CREATE TABLE employees (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50),
  age INT
);

-- Insert sample data
INSERT INTO employees (name, age) VALUES ('Alice', 25), ('Bob', 30), ('Charlie', 28);

-- Display initial content
SELECT * FROM employees;

-- Use DELETE to remove specific rows
DELETE FROM employees WHERE age > 29;
-- Changes not yet applied
SELECT * FROM employees;

-- Use TRUNCATE to remove all rows
TRUNCATE TABLE employees;
-- Changes not yet applied
SELECT * FROM employees;

-- Final commitment to make changes permanent
COMMIT;
-- Now, the table is empty
SELECT * FROM employees;
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What is the difference between a full join and an inner join?</summary>

### What is the difference between a full join and an inner join?

**Definition:**

Full joins and inner joins serve different purposes in database management systems and exhibit distinct behavior patterns.

**How it works:**

#### Inner Join

An inner join finds and combines matching records from both the parent and child tables, based on a specified join condition. Records that don't have a match in either table are excluded from the result.

#### Full Join

A full join, also known as a full outer join, returns all records from both tables and fills in NULL values where no match is found in the corresponding table. This type of join is less frequently used compared to the others, as it can potentially generate very large result sets.

#### Visual Representation

#### SQL Joins

**Examples:**

#### SQL Syntax

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```

#### SQL Syntax

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2 ON table1.column_name = table2.column_name;
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

### SQL and Query Optimization

<details>
<summary>How would you write an SQL query to fetch duplicate records from a table?</summary>

### How would you write an SQL query to fetch duplicate records from a table?

**Definition:**

Let's look at two common methods for identifying and handling duplicate records in SQL.

**How it works:**

#### 2. Using ROW_NUMBER()

This method can be particularly useful in cases where you also want to keep track of the "duplicate" IDs.

**Examples:**

#### 1. Standard SQL

```sql
SELECT column1, column2, ..., columnN, COUNT(*)
FROM mytable
GROUP BY column1, column2, ..., columnN
HAVING COUNT(*) > 1;
```

```sql
WITH duplicates AS (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY column1, column2, ..., columnN ORDER BY ID) AS rnum
  FROM mytable
)
SELECT * 
FROM duplicates 
WHERE rnum > 1;
```

In both of these methods, column1, column2, ..., columnN refer to the column or set of columns you're using to identify duplicates.

#### Code Example: Standard SQL

```sql
SELECT name, age, COUNT(*)
FROM mytable
GROUP BY name, age
HAVING COUNT(*) > 1;
```

In this example, we're looking for duplicate records based on the "name" and "age" columns from the "mytable".

#### Code Example: ROW_NUMBER()

```sql
WITH duplicates AS (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY name, age ORDER BY ID) AS rnum
  FROM mytable
)
SELECT * 
FROM duplicates 
WHERE rnum > 1;
```

In this example, we're using ROW_NUMBER() to identify records with the same "name" and "age" and keeping track of the unique row number for each.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What is a prepared statement, and why would you use one?</summary>

### What is a prepared statement, and why would you use one?

**Definition:**

Prepared Statements reduce the risk of SQL injection by separating SQL data and commands. They also optimize query execution by allowing repetitive parameter bindings.

**How it works:**

#### Key Advantages

- **Security**: They defend against SQL injection by distinguishing between SQL code and input values.

- **Performance**: Prepared statements can be faster when used repeatedly, as they are parsed and executed in discrete steps.

- **Readability and Maintainability**: Separating the SQL code from the parameters makes it more readable. It can make the code easier to understand, review, and maintain.

**When to use:**

#### When to Use Prepared Statements

- **Input from Untrusted Sources**: When SQL queries are constructed with data from untrusted sources, prepared statements ensure the data is treated as literal values, preventing SQL injection.

- **Repeated Executions**: For queries executed multiple times, using a prepared statement can be more efficient than constructing and executing a new query each time. This is especially relevant in loops or high-volume operations.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What is the N+1 query problem and how can you solve it?</summary>

### What is the N+1 query problem and how can you solve it?

**Definition:**

N+1 Query Problem arises when an object graph is retrieved using a query that results in unnecessary database hits.

For instance, consider a one-to-many relationship where each "Order" has many "LineItems". If you fetch all "Orders" and then individually fetch their "LineItems," it leads to multiple query rounds. These excessive queries are inefficient, especially when the number of related items is high.

Let's say, in a more specific case, you fetch:

All Orders.
Then for each Order, fetch its LineItems.

**How it works:**

#### This corresponds to:

- **Primary Query**: SELECT * FROM Orders.

- **Secondary Query**: SELECT * FROM LineItems WHERE order_id = :order_id (Potentially executed several times based on order count).

Here you have an N+1 scenario because the second query is executed "N" times, once for each Order, hence N+1.

#### Solutions

#### Use a JOIN Query

By employing a JOIN, you can retrieve related entities in a single query.

#### Leverage Lazy Loading

Some ORMs are configured to execute extra queries only when a related object is accessed for the first time. This is known as lazy loading. It reduces the initial query size, improving efficiency but might lead to an N+1 problem if multiple related entities are accessed.

#### Use Explicit Eager Loading

Modern ORMs often provide mechanisms for explicitly defining which related entities to fetch eagerly. This way, you can still benefit from lazy loading while strategically opting for immediate access when needed. In Entity Framework (EF) and Java Persistence API (JPA), for example, you can use annotations like @ManyToOne and @OneToMany to control lazy/eager loading behavior.

#### Use Data Projection

Instead of loading entire entities, you can select specific fields needed for immediate operations. This can be beneficial when you aren't interested in all the entity's properties.

#### Implement Paging

When dealing with a large dataset, it's often more reasonable to employ paging rather than fetching everything at once. A SELECT query can be modified with OFFSET and LIMIT (in SQL Server and PostgreSQL) or the ROW_NUMBER() function (in SQL Server and Oracle) to achieve this neatly.

**Examples:**

#### SQL

```sql
SELECT o.*, li.*
FROM Orders o
JOIN LineItems li ON o.id = li.order_id;
```

ORM ORM tools, like Hibernate, can handle this for you. You might not even know under the hood whether the ORM uses JOIN.

#### SQL

```sql
SELECT id, name FROM Orders;
```

ORM With JPA, you can use constructor expressions in JPQL to achieve this:

```java
TypedQuery<OrderSummary> query = em.createQuery(
    "SELECT NEW OrderSummary(o.id, o.name) FROM Order o", OrderSummary.class
);
```

And in Entity Framework, you can utilize LINQ projections:

```csharp
var orderSummaries = from order in context.Orders
                    select new OrderSummary { Id = order.Id, Name = order.Name };
```

#### SQL Server and PostgreSQL

```sql
SELECT * FROM Orders
ORDER BY id
OFFSET 0 ROWS
FETCH NEXT 5 ROWS ONLY;
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Explain the function of GROUP BY and HAVING clauses in SQL.</summary>

### Explain the function of GROUP BY and HAVING clauses in SQL.

**Definition:**

GROUP BY and HAVING transform, filter, and work together with aggregate functions to enable more complex and nuanced result sets.

**How it works:**

#### Functions of GROUP BY

- **Aggregation**: Identifies groups and computes aggregate functions (e.g., COUNT, SUM) for each group.

- **Redundancy Reduction**: Collapses redundant data based on the grouped columns to provide a leaner dataset.

- **Column Constraint**: Allows queries to select only aggregated columns, and from the source columns, all non-aggregated ones need to be in the GROUP BY, ensuring consistent data representation.

#### Functions of HAVING

- **Group-Wise Filtering**: Applies conditional checks to grouped data.

- **Post-Group Filtering**: Allows filtering based on the results of aggregate functions.

- **Aggregation Assertion**: Validates aggregated values against specific conditions.

- **Comparison with WHERE**: Enables more advanced, aggregate-aware filtering compared to the global, pre-aggregation filtering provided by WHERE.

#### Consider the following relational schema:

#### EMPLOYEE (ID, Name, DeptID, Salary)

#### DEPARTMENT (ID, Name)

To retrieve department IDs where the average salary is over a certain threshold:

**Examples:**

#### Code Example: GROUP BY & HAVING

#### SQL Query

```sql
SELECT DeptID
FROM EMPLOYEE
GROUP BY DeptID
HAVING AVG(Salary) > 50000;
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What are indexes and how do they work in databases?</summary>

### What are indexes and how do they work in databases?

**Definition:**

An index serves as a data structure in a database system. It enhances the efficiency and speed of data lookup operations by logically ordering the indexed data.

**How it works:**

#### Index Types

- **B-Tree (Balanced Tree)**: Suited for ranges and equality operations, such as using WHERE clauses in SQL.

- **Hash**: Ideal for exact-match lookups, like primary keys.

- **Full-text**: Specifically designed for text searches, often seen in search engines.

- **Bitmap**: Efficient for low-cardinality columns, where there are few distinct values, like gender.

#### Data Lookup Using Indexes

- **Ordered Scans**: Possible through B-Tree indexes where data is sorted, aiding range queries.

- **Exact-Match Sequencing**: For Hash and tree-based indexes, this ensures swift exact-value matches.

- **Range Searches**: Supported by B-Trees, they enable operations like finding numbers within a range.

#### Best Practices

- **Consider Index Maintenance Overhead**: While indexes speed up reads, they might slow down data modifications like INSERT, UPDATE, and DELETE.

- **Index Tailing**: Place more selective columns first, and follow them with less-discriminatory columns for best results.

- **Index Coverage**: Aim to cover frequently queried columns, but don't go overboard, leading to index bloat.

**Examples:**

#### Code Example: Index Types in PostgreSQL

Here is the SQL code:

```sql
-- B-Tree Index
CREATE INDEX idx_btree ON table_name (column_name);

-- Hash Index
CREATE INDEX idx_hash ON table_name USING HASH (column_name);
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>What impact do JOIN operations have on database performance?</summary>

### What impact do JOIN operations have on database performance?

**Definition:**

PostgreSQL plans a join of two inputs at a time, and the number of possible join orders grows exponentially with the number of tables.

**How it works:**

The explicit-joins chapter says that beyond a handful of tables the search becomes expensive, and `geqo_threshold` switches the planner to a genetic search. `FULL JOIN` fully constrains the order the planner may use. `LEFT JOIN` and `RIGHT JOIN` constrain it partly. An explicit `INNER JOIN` or `CROSS JOIN` does not constrain the order unless `join_collapse_limit` is 1. A join with no restrictive condition is the Cartesian product of the two inputs, which is why the plan cost, not a single Big-O label, is what `EXPLAIN` reports.

**Sources:**

- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
</details>

<details>
<summary>Define what a subquery is and provide a use case for it.</summary>

### Define what a subquery is and provide a use case for it.

**Definition:**

A scalar subquery is a parenthesized `SELECT` that must return one row and one column. Zero rows become null. More than one row or column is an error.

**How it works:**

The surrounding expression uses that single value. A subquery in `FROM` is planned on its own unless the planner collapses it. Collapse happens only when the parent `FROM` list would still have no more than `from_collapse_limit` items (default eight). Collapsing can produce a better plan and a larger join-search problem at the same time.

**Examples:**

```sql
SELECT *
FROM x, y,
    (SELECT * FROM a, b, c WHERE something) AS ss
WHERE somethingelse;
```

**Sources:**

- [Value Expressions — Scalar Subqueries](https://www.postgresql.org/docs/18/sql-expressions.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Other Planner Options](https://www.postgresql.org/docs/18/runtime-config-query.html)
</details>

<details>
<summary>What is a correlated subquery?</summary>

### What is a correlated subquery?

**Definition:**

A correlated subquery is a subquery that refers to variables from the surrounding query. During any one evaluation those outer values act as constants.

**How it works:**

The expressions page states that rule for scalar subqueries. The subquery is re-evaluated with the current outer row’s values. It still must return at most one row and one column; extra rows or columns are an error, and no row yields null.

**Sources:**

- [Value Expressions — Scalar Subqueries](https://www.postgresql.org/docs/18/sql-expressions.html)
</details>

<details>
<summary>Describe how you would optimize a slow SQL query.</summary>

### Describe how you would optimize a slow SQL query.

**Definition:**

Start from the plan. `EXPLAIN` prints a tree whose costs are in units of `seq_page_cost` (default 1.0), and a parent’s cost includes its children.

**How it works:**

For a join-heavy query, the explicit-joins chapter is the lever: `join_collapse_limit` = 1 makes the written `JOIN` order the planned order, and `from_collapse_limit` stops a subquery from being flattened into a larger join search. `EXPLAIN` shows whether the planner chose a sequential scan or an index scan; the manual does not print a Big-O class for that choice. `EXPLAIN ANALYZE` runs the statement and adds actual times.

**Sources:**

- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Other Planner Options](https://www.postgresql.org/docs/18/runtime-config-query.html)
</details>

<details>
<summary>Explain the EXPLAIN statement and how you use it in query optimization.</summary>

### Explain the EXPLAIN statement and how you use it in query optimization.

**Definition:**

`EXPLAIN` prints the planner’s plan tree without necessarily running the statement. Each node has `cost=startup..total`, a `rows` estimate of rows it emits, and a `width`.

**How it works:**

Costs are arbitrary units based on `seq_page_cost`, not milliseconds. The parent total includes the children. `EXPLAIN ANALYZE` executes the query and reports actual row counts and times beside the estimates. Output formats are text, XML, JSON, and YAML. A large gap between estimated and actual rows is the signal to revisit statistics or the join shape, not a number the command itself rewrites.

**Sources:**

- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
</details>

### Data Modeling and Design

<details>
<summary>What is an Entity-Relationship (ER) model, and why is it useful?</summary>

### What is an Entity-Relationship (ER) model, and why is it useful?

**Definition:**

PostgreSQL’s manual does not define an ER diagram notation. The documented shape of a relationship is a table with a primary key and another table that `REFERENCES` it.

**How it works:**

In the foreign-key tutorial, `cities.name` is the primary key and `weather.city` references it. The database then rejects a weather row whose city is not in `cities`. That is the integrity an ER “entity plus relationship” is mapped onto in this system.

**Examples:**

```sql
CREATE TABLE cities (
    name varchar(80) primary key,
    location point
);
CREATE TABLE weather (
    city varchar(80) references cities(name),
    temp_lo int,
    temp_hi int,
    prcp real,
    date date
);
```

**Sources:**

- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
</details>

<details>
<summary>Describe the process of converting an ER model into a relational database schema.</summary>

### Describe the process of converting an ER model into a relational database schema.

**Definition:**

Each entity becomes a table. Its identity becomes a `PRIMARY KEY`, which is `NOT NULL` and has a unique B-tree index. A relationship becomes a `REFERENCES` column or a foreign-key constraint.

**How it works:**

The constraints chapter shows `product_no integer PRIMARY KEY`. The tutorial shows `weather.city` referencing `cities(name)`. `ALTER TABLE ... ADD FOREIGN KEY` can add the link later, and existing rows must already satisfy it. A `CHECK` constraint, such as `name <> ''`, encodes a rule that is not a key.

**Sources:**

- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
</details>

<details>
<summary>How do you design a scalable database schema for a high-traffic application?</summary>

### How do you design a scalable database schema for a high-traffic application?

**Definition:**

PostgreSQL’s documented split of a very large table is partitioning: one logical table, many physical partitions, range, list, or hash.

**How it works:**

The partitioning chapter says the benefit usually appears only when the table is larger than the server’s physical memory. Queries that touch one partition can scan that partition instead of the whole table, and dropping a partition with `DROP TABLE` or `ALTER TABLE DETACH PARTITION` avoids a bulk `DELETE` and its `VACUUM`. The partitioned parent stores nothing; rows route by the partition key. A primary key and foreign keys still enforce integrity on the tables you create.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
</details>

<details>
<summary>What is database sharding, and what are its benefits and drawbacks?</summary>

### What is database sharding, and what are its benefits and drawbacks?

**Definition:**

The partitioning chapter describes splitting one logical table into smaller physical pieces. It does not describe a separate shard router. Partitions may be foreign tables, and then the user must keep those rows inside the partition bounds.

**How it works:**

Documented benefits: a hot partition’s indexes are more likely to fit in memory; a query that hits most of one partition can use a sequential scan of that partition; bulk load and delete become attach and detach; cold data can sit on slower storage. The same page says this pays off only for a very large table, with a rule of thumb that the table exceeds the database server’s memory. A regular table cannot be turned into a partitioned table in place.

**When to use:**

Use declarative partitioning when one table is huge and queries or retention line up with a key such as a date range. A foreign-table partition is a split across servers only if you accept that PostgreSQL will not check the foreign rows for you.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
</details>

<details>
<summary>Explain the term “data integrity” and how it’s enforced in databases.</summary>

### Explain the term “data integrity” and how it’s enforced in databases.

**Definition:**

In PostgreSQL, integrity constraints reject rows that break a declared rule. A primary key forces `NOT NULL` and uniqueness. `UNIQUE`, `CHECK`, `NOT NULL`, and `FOREIGN KEY` are the other documented tools.

**How it works:**

`ALTER TABLE products ADD CHECK (name <> '')` fails if any existing row violates the check. A foreign key rejects a child row with no parent, as in `weather.city` referencing `cities(name)`. Constraints are checked when they are added and when rows change; they are not a separate cleanup job.

**Sources:**

- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
</details>

### Transactions and Concurrency Control

<details>
<summary>Can you explain the concept of transaction isolation levels?</summary>

### Can you explain the concept of transaction isolation levels?

**Definition:**

SQL names four isolation levels. PostgreSQL implements three behaviors: `READ UNCOMMITTED` is treated as `READ COMMITTED`. The default is `READ COMMITTED`.

**How it works:**

The isolation chapter says PostgreSQL’s `REPEATABLE READ` also prevents phantom reads, which is stricter than the SQL minimum for that level. `SERIALIZABLE` prevents the serialization anomaly; a failure raises `could not serialize access due to read/write dependencies among transactions` and that transaction rolls back. You choose the level with `SET TRANSACTION`. The chapter also says sequences are not transactional.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
</details>

<details>
<summary>What is a deadlock in databases, and how can it be resolved?</summary>

### What is a deadlock in databases, and how can it be resolved?

**Definition:**

A deadlock is a cycle in which each transaction holds a lock the other needs. PostgreSQL detects the cycle and aborts one transaction. Which one is aborted is not something to rely on.

**How it works:**

The locking chapter’s row example is two updates of accounts 11111 and 22222 in opposite orders. Each waits for the other’s row lock. The documented defense is to lock multiple objects in a consistent order, and to take the strongest mode you will need on the first lock of an object. If that is not possible, retry the transaction that was aborted. With no deadlock, a lock waiter waits indefinitely, so `lock_timeout` (default 0, milliseconds) can abort a statement that waits too long. Do not hold a transaction open while waiting for user input.

**Sources:**

- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Client Connection Defaults](https://www.postgresql.org/docs/18/runtime-config-client.html)
</details>

<details>
<summary>Describe optimistic vs. pessimistic locking.</summary>

### Describe optimistic vs. pessimistic locking.

**Definition:**

The locking chapter’s row locks are held until the transaction ends or a savepoint rolls them back. Another transaction that wants a conflicting lock waits. That is lock-first concurrency.

**How it works:**

The isolation chapter’s serializable mode is the other documented path: both transactions run, and one can fail with `could not serialize access due to read/write dependencies among transactions` instead of waiting on a cycle of row locks. The manual does not use the labels optimistic and pessimistic. `SELECT ... FOR UPDATE` is the explicit row lock; plain reads are not blocked by row locks, which block writers and lockers.

**When to use:**

Use `FOR UPDATE` when the next write must see a row that nobody else is updating. Use `SERIALIZABLE` when you would rather let the database abort one transaction than hold locks across a longer read-write cycle.

**Sources:**

- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
- [SELECT](https://www.postgresql.org/docs/18/sql-select.html)
</details>

<details>
<summary>How does a database ensure consistency during concurrent transactions?</summary>

### How does a database ensure consistency during concurrent transactions?

**Definition:**

PostgreSQL combines isolation levels with locks. The default isolation is `READ COMMITTED`. Row locks stop two transactions from holding conflicting locks on the same row.

**How it works:**

A transaction may hold conflicting locks on the same row only across its own subtransactions. Everyone else waits, or, in a deadlock, one transaction is aborted. Under `SERIALIZABLE`, a read/write dependency cycle ends in a serialization error and a rollback. Locks release at transaction end or on savepoint rollback. Sequences are not covered by that transactional consistency.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
</details>

<details>
<summary>What is a savepoint in a database transaction?</summary>

### What is a savepoint in a database transaction?

**Definition:**

A savepoint is a named point inside a transaction. `ROLLBACK TO` undoes work done after that point and releases locks taken after it.

**How it works:**

The SELECT page shows `BEGIN`, `SELECT ... FOR UPDATE`, `SAVEPOINT s`, an `UPDATE` of the same row, then `ROLLBACK TO s`. The row lock from the `SELECT` was taken before the savepoint, so it remains. Locks acquired by the rolled-back `UPDATE` are released, which is the rule in the locking chapter: row locks end at transaction end or during savepoint rollback.

**Examples:**

```sql
BEGIN;
SELECT * FROM mytable WHERE key = 1 FOR UPDATE;
SAVEPOINT s;
UPDATE mytable SET extra = 1 WHERE key = 1;
ROLLBACK TO s;
```

**Sources:**

- [SELECT](https://www.postgresql.org/docs/18/sql-select.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
</details>

### Database Security

<details>
<summary>What is SQL injection, and how do you prevent it?</summary>

### What is SQL injection, and how do you prevent it?

**Definition:**

SQL injection is building a statement by pasting input into the SQL text. `PREPARE` keeps the statement structure fixed and sends values separately as `$1`, `$2`, and so on.

**How it works:**

The PREPARE page shows `PREPARE fooplan (int, text, bool, numeric) AS INSERT INTO foo VALUES($1, $2, $3, $4)` and then `EXECUTE fooplan(...)`. The types are declared on the prepared statement. The value never becomes part of the SQL syntax, so a quote or keyword in the text stays data.

**Examples:**

```sql
PREPARE fooplan (int, text, bool, numeric) AS
    INSERT INTO foo VALUES($1, $2, $3, $4);
EXECUTE fooplan(1, 'Hunter Valley', 't', 200.00);
```

**Sources:**

- [PREPARE](https://www.postgresql.org/docs/18/sql-prepare.html)
</details>

<details>
<summary>Explain the role of access control in database security.</summary>

### Explain the role of access control in database security.

**Definition:**

The encryption chapter documents who may open a connection, not a table-grant tutorial. `pg_hba.conf` lines of type `host` allow non-encrypted connections, `hostssl` requires SSL, and `hostgssenc` requires GSSAPI encryption.

**How it works:**

Role passwords are stored as hashes selected by `password_encryption`. With SCRAM or MD5 authentication the client encrypts the password before it is sent, so the plaintext is not present on the server. SCRAM is preferred. MD5 authentication is deprecated and will be removed. SSL certificates let client and server check each other’s identity and block a machine that only pretends to be the server long enough to read a password.

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
</details>

<details>
<summary>What are the best practices for storing passwords in a database?</summary>

### What are the best practices for storing passwords in a database?

**Definition:**

Database role passwords are stored as hashes. `password_encryption` chooses the hash. SCRAM is preferred because it is an Internet standard and is stronger than PostgreSQL’s MD5 authentication, which is deprecated.

**How it works:**

For a column that stores an application password, pgcrypto documents `crypt(password, gen_salt(...))`. The manual’s example is `gen_salt('md5')`. That column hash is separate from role authentication. Decrypted or plaintext secrets should not be the stored form; the encryption chapter’s point is that the administrator cannot read the role password back from the hash.

**Examples:**

```sql
UPDATE users SET pswhash = crypt('new password', gen_salt('md5'));
```

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
- [pgcrypto](https://www.postgresql.org/docs/18/pgcrypto.html)
</details>

<details>
<summary>How do you secure data transmission to and from a database?</summary>

### How do you secure data transmission to and from a database?

**Definition:**

An SSL connection encrypts the password, the queries, and the returned data. Require it with `hostssl` in `pg_hba.conf`, and the client can refuse non-SSL servers.

**How it works:**

GSSAPI encryption also covers queries and results, and it sends no password. `hostgssenc` requires it; the client uses `gssencmode=require`. Stunnel or SSH are the other documented tunnels. Certificates on both sides block a man-in-the-middle that impersonates the server.

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
</details>

<details>
<summary>Describe the use of encryption within databases for data at rest.</summary>

### Describe the use of encryption within databases for data at rest.

**Definition:**

PostgreSQL documents three layers for data at rest: column encryption with pgcrypto, file-system or disk encryption, and client-side encryption.

**How it works:**

pgcrypto encrypts selected columns. The client supplies the key; the server decrypts and then sends the data, so the plaintext and the key exist on the server briefly. Disk encryption (dm-crypt + LUKS, eCryptfs, EncFS, FreeBSD geli/gbde/PEFS) stops a stolen drive from being read, and does not protect a mounted file system. If the server administrator is not trusted, the client encrypts before sending, and decrypts results itself, so plaintext never appears on the server.

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
- [pgcrypto](https://www.postgresql.org/docs/18/pgcrypto.html)
</details>

### Backup and Recovery

<details>
<summary>What is a database snapshot, and when would you use one?</summary>

### What is a database snapshot, and when would you use one?

**Definition:**

`pg_basebackup` takes a base backup of a running cluster without blocking other clients. The server is put into and out of backup mode automatically. That copy is the starting point for point-in-time recovery and for a log-shipping or streaming standby.

**How it works:**

The backup is the whole cluster, not one database. A full backup is an exact copy of the cluster files. An incremental backup stores changed blocks and cannot be used until `pg_combinebackup` combines it with the backups it depends on. A logical `pg_dump` is a different artifact: SQL text, not a file-level snapshot, and it cannot feed WAL replay.

**When to use:**

Take a base backup when you need PITR or a new standby. Use `pg_dump` when you need to move one database to a newer PostgreSQL version or another CPU architecture.

**Sources:**

- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>

<details>
<summary>Explain the difference between logical and physical backups.</summary>

### Explain the difference between logical and physical backups.

**Definition:**

A logical backup from `pg_dump` is SQL that recreates the database. A physical backup from `pg_basebackup` is a copy of the cluster’s files.

**How it works:**

`pg_dump` can be reloaded into a newer PostgreSQL version and onto a different machine architecture. File-level backups and continuous archiving are specific to the server version. `pg_dump` and `pg_dumpall` do not contain what WAL replay needs. `pg_dumpall` adds cluster-wide objects such as roles and tablespaces; each database dump is consistent by itself, and the databases are not synchronized with each other.

**Sources:**

- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>

<details>
<summary>How would you restore a database from a backup file?</summary>

### How would you restore a database from a backup file?

**Definition:**

A `pg_dump` file is SQL. Restoring it means reloading that SQL into a server, which may be a newer version or a different architecture. A physical restore starts from a `pg_basebackup` and follows the point-in-time recovery steps in the continuous-archiving chapter.

**How it works:**

An incremental base backup is not restorable alone. `pg_combinebackup` must combine it with the full backup and the incrementals it depends on. `pg_dump` cannot be the base of WAL replay. Selective restore of one database is the logical dump; `pg_basebackup` always copies the whole cluster.

**Examples:**

```shell
pg_dump dbname > dumpfile
```

**Sources:**

- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>

<details>
<summary>What are the common strategies for database disaster recovery?</summary>

### What are the common strategies for database disaster recovery?

**Definition:**

PostgreSQL documents two strategies. `pg_dump` produces reloadable SQL. Continuous archiving plus a base backup replays WAL to a chosen point in time.

**How it works:**

Use the logical dump to move to a newer major version or another architecture. Use `pg_basebackup` plus archived WAL when the replacement server is the same version and you need the cluster as it was at a past moment, or as the start of a standby. `pg_dumpall` is the logical copy that also includes roles and tablespaces.

**Sources:**

- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
</details>

<details>
<summary>How does point-in-time recovery work in databases?</summary>

### How does point-in-time recovery work in databases?

**Definition:**

Point-in-time recovery restores a file-level base backup and replays archived WAL up to a chosen point. `pg_basebackup` is the documented way to take that base backup from a running cluster.

**How it works:**

The continuous-archiving chapter states that `pg_dump` and `pg_dumpall` are logical and cannot be used for WAL replay. The base backup is version-specific, unlike a SQL dump. An incremental backup must be combined with `pg_combinebackup` before it can be recovered.

**Sources:**

- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
</details>

### Performance Tuning and Scaling

<details>
<summary>How would you handle a scenario where your database’s read load significantly increases?</summary>

### How would you handle a scenario where your database’s read load significantly increases?

**Definition:**

The partitioning chapter says read performance improves when the rows a query needs sit in one partition or a few partitions, because that replaces the upper levels of an index and can keep the hot index pages in memory.

**How it works:**

If a query reads most of one partition, a sequential scan of that partition can beat an index scan scattered across the whole table. `EXPLAIN` shows which plan node you actually got. The pages fetched here do not document a read-replica count or a pool size.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
</details>

<details>
<summary>What strategies exist for scaling writes in a high-volume database system?</summary>

### What strategies exist for scaling writes in a high-volume database system?

**Definition:**

The partitioning chapter’s write strategy for a large table is to add or remove partitions instead of a bulk `DELETE`, and to let inserts route by the partition key.

**How it works:**

`ALTER TABLE ATTACH PARTITION` can add an existing table as a partition. Updating the partition key moves the row if it no longer fits. Separately, the locking chapter says write throughput collapses if transactions lock objects in inconsistent orders or stay open while waiting: lock waiters wait indefinitely unless `lock_timeout` is set. Those are the documented write-side controls, not a shard-count formula.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Client Connection Defaults](https://www.postgresql.org/docs/18/runtime-config-client.html)
</details>

<details>
<summary>Describe how connection pooling benefits database performance.</summary>

### Describe how connection pooling benefits database performance.

**Definition:**

The PostgreSQL 18 pages used here do not document a built-in connection pooler or a benefit formula for one.

**How it works:**

What they do document is the cost of a session that holds work open. A transaction waiting for a lock waits indefinitely when `lock_timeout` is 0, the default. Holding a transaction open while waiting for user input is called out as a bad idea because other sessions queue behind its locks. `lock_timeout` aborts only the lock wait, and it is applied per lock attempt, in milliseconds.

**Sources:**

- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Client Connection Defaults](https://www.postgresql.org/docs/18/runtime-config-client.html)
</details>

<details>
<summary>How do you monitor and identify database performance bottlenecks?</summary>

### How do you monitor and identify database performance bottlenecks?

**Definition:**

`EXPLAIN` and `EXPLAIN ANALYZE` are the documented tools. Compare the planner’s `rows` and `cost` with the actual rows and time from `ANALYZE`.

**How it works:**

Costs are in `seq_page_cost` units. A parent that is expensive because of a child points at that child. A sequential scan of a huge table, or a join search that has grown past `geqo_threshold`, is visible as the plan shape. `debug_deadlocks` dumps lock information when a deadlock timeout occurs, and only if the server was built with `LOCK_DEBUG`.

**Sources:**

- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
</details>

<details>
<summary>What is horizontal and vertical scaling in databases?</summary>

### What is horizontal and vertical scaling in databases?

**Definition:**

The partitioning chapter does not use the words horizontal and vertical. It does say partitioning pays off when the table exceeds the physical memory of the database server, and that partitions can be foreign tables on other servers.

**How it works:**

A larger server memory is the vertical fact in that rule of thumb: the table is compared with one machine’s RAM. Splitting the logical table into partitions, including foreign partitions, is the documented way to put pieces in more than one place. Foreign partitions do not get automatic checking that their rows match the bounds. Range, list, and hash are the built-in methods.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
</details>

### NoSQL Databases

<details>
<summary>Explain what a document store is and give an example of where it’s appropriate to use.</summary>

### Explain what a document store is and give an example of where it’s appropriate to use.

**Definition:**

PostgreSQL is not described as a document-store product. It does store JSON documents in `jsonb` columns inside relational tables.

**How it works:**

The JSON chapter’s `jsonb` containment operator `@>` tests whether the left document contains the right one. Array order and duplicates do not matter; nesting depth does. A query such as a product document containing `{"version": 9.4}` is the documented use. Constraints and joins still apply to the table around that column.

**Examples:**

```sql
SELECT '{"product": "PostgreSQL", "version": 9.4, "jsonb": true}'::jsonb
    @> '{"version": 9.4}'::jsonb;
```

**Sources:**

- [JSON Types](https://www.postgresql.org/docs/18/datatype-json.html)
</details>

<details>
<summary>What is a graph database, and what are its typical use cases?</summary>

### What is a graph database, and what are its typical use cases?

**Definition:**

The PostgreSQL 18 pages used here do not define a graph database, nodes, or edge traversal.

**How it works:**

The documented way to represent a relationship is a foreign key: `weather.city references cities(name)`. Walking that relationship is a SQL join, whose plan cost is what the explicit-joins and `EXPLAIN` chapters describe. A dedicated graph store is outside those pages.

**Sources:**

- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
</details>

<details>
<summary>Describe the key-value store model and its typical applications.</summary>

### Describe the key-value store model and its typical applications.

**Definition:**

The fetched PostgreSQL chapters do not define a key-value store. The closest documented structure is a table whose identity is a primary key.

**How it works:**

`PRIMARY KEY` forces `NOT NULL` and builds a unique B-tree index. A row is then addressed by that key, and other columns hold the payload, including a `jsonb` value if the payload is a document. That is still a relational table with constraints, not a separate key-value engine.

**Sources:**

- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
- [JSON Types](https://www.postgresql.org/docs/18/datatype-json.html)
</details>

<details>
<summary>How do you choose between consistency and availability in a NoSQL database, referencing the CAP theorem?</summary>

### How do you choose between consistency and availability in a NoSQL database, referencing the CAP theorem?

**Definition:**

The PostgreSQL transaction chapter does not state the CAP theorem or a consistency-versus-availability switch for a NoSQL system.

**How it works:**

Inside one PostgreSQL database the documented consistency controls are the isolation level and locks. The default is `READ COMMITTED`. `REPEATABLE READ` also blocks phantoms. `SERIALIZABLE` aborts a transaction that would create a serialization anomaly. Availability of a second node is not a setting on that page.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
</details>

<details>
<summary>What is eventual consistency, and in what scenarios is it used?</summary>

### What is eventual consistency, and in what scenarios is it used?

**Definition:**

Eventual consistency is not an isolation level in PostgreSQL. The manual’s levels are the SQL names, with `READ UNCOMMITTED` behaving as `READ COMMITTED`.

**How it works:**

A committed transaction’s effects are governed by the isolation level of the reader, not by a promise that replicas will catch up later. The continuous-archiving chapter describes a standby that replays WAL from a base backup; that is shipping a log to another server, and the page does not call the result eventual consistency. Sequences are the documented exception that is not transactional.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>
