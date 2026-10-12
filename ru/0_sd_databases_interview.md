# Databases — собеседование

Ответы заполнены для каждого вопроса в этой заметке.

[← К обзору](./0_system_design.md)

### Related docs

- [Databases — собеседование](./0_sd_databases_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Databases](https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/).

### Database Fundamentals

<details>
<summary>Что такое dataBASE management system (DBMS), and can you name some examples?</summary>

### Что такое dataBASE management system (DBMS), and can you name some examples?

**Definition:**

Система управления базами данных (DBMS) служит посредником между пользователями и базой. Она помогает сопровождать данные, получать их и постоянно ими управлять, с помощью устроенного механизма, который обеспечивает целостность, безопасность и эффективность данных.

**How it works:**

#### Ключевые возможности

- **Data Modeling**: Упорядочивает данные в логически устроенные таблицы и связи.

- **Data Manipulation**: Позволяет операции CRUD (Create, Read, Update, Delete), обычно через язык запросов.

- **Data Integrity**: Поддерживает ссылочную целостность и согласованность данных.

- **Security & Access Control**: Регулирует права пользователей на данные.

- **Data Concurrency**: Управляет одновременным доступом нескольких пользователей к данным, чтобы не было конфликтов.

- **Backup & Recovery**: Даёт механизмы восстановления данных при потере или порче.

- **Data Analysis & Reporting**: Часто включает инструменты оптимизации запросов и построения отчётов.

#### Виды DBMS

- **Relational DBMS (RDBMS)**: Упорядочивает данные в таблицы с заранее заданной структурой, которую определяет схема. Для операций с данными обычно используют SQL (Structured Query Language). Обычные примеры — MySQL, PostgreSQL и Microsoft SQL Server.

- **NoSQL DBMS**: Появились как ответ на ограничения RDBMS, рассчитаны на неструктурированные или полуструктурированные данные и горизонтальную масштабируемость. Примеры — MongoDB для документного хранения и Redis для хранилищ ключ-значение.

- **Cloud-Based DBMS**: Размещены на облачных платформах, дают гибкость и масштабируемость и требуют от пользователей мало сопровождения. Заметные примеры — Amazon RDS, Google Cloud Bigtable и Azure Cosmos DB.

- **NewSQL DBMS**: Соединяет выгоды традиционных RDBMS с современными нуждами вроде масштабируемости. Такие системы часто дают лучшую производительность и рассчитаны на сценарии больших данных. Примеры — Google Cloud Spanner и NuoDB.

- **Object-Oriented DBMS (OODBMS)**: Рассчитаны на сложные объектные модели данных. Они дают сохранность объектов и отделяют структуру объекта от реляционной схемы. ODBMS сейчас менее популярны, но примеры — db4o и ObjectStore.

- **In-memory DBMS**: Держит данные в памяти системы, что даёт быстрый доступ и обработку. Примеры — Oracle TimesTen и Redis.

- **Multimodel DBMS**: Умеет несколько видов баз, например хранилища ключ-значение, документные и графовые. Это даёт разные модели данных в одной системе. ArangoDB — пример такой системы.

- **Graph DBMS**: Специализированы на связанных элементах данных. Они оптимизированы для операций вроде обходов и поиска пути. Neo4j — известный пример такого DBMS.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Объясните the ACID properties in the context of dataBASEs.</summary>

### Объясните the ACID properties in the context of dataBASEs.

**Definition:**

В управлении базами ACID обеспечивает, что транзакции обрабатываются надёжно и по стандарту.

**How it works:**

#### Свойства ACID

#### Atomicity

Атомарность обеспечивает, что все задачи транзакции выполнены или не выполнена ни одна. Базы ведут журналы транзакций, чтобы управлять атомарностью. Если задача срывается, журнал позволяет системе узнать незавершённое состояние и вернуться к последнему известному согласованному состоянию.

- **Consider a banking transfer**: если списание прошло, а зачисление нет, атомарность обеспечивает, что списание тоже откатывается.

#### Consistency

Согласованность требует, чтобы транзакция переводила базу из одного согласованного состояния в другое согласованное. Она следит, чтобы правила целостности данных не нарушались. Например, после перевода сумма остатков на счетах должна остаться той же.

#### Isolation

Изоляция обеспечивает, что операции параллельных транзакций невидимы друг другу, пока они не завершены, чтобы защититься от возможных конфликтов и несогласованности.

Разные уровни изоляции, такие как read uncommitted, read committed, repeatable read и serialize, задают, насколько транзакции изолированы друг от друга.

#### Durability

Долговечность гарантирует, что после фиксации транзакции её изменения сохраняются даже при сбое системы. Обычно этого добиваются механизмами вроде упреждающей журнализации и управления буферами, которые обеспечивают запись изменений на диск.

**When to use:**

#### ACID в практических сценариях

- **Banking Applications**: ACID обеспечивает, что денежные транзакции безопасны и надёжны.

- **Inventory Management**: Когда товары покупают, уровни запасов обновляются согласованно, без порчи и ошибок.

- **Scheduling Systems**: Действия вроде записи на приём либо происходят целиком, либо не происходят вовсе, и не остаётся путаных частичных записей.

- **E-commerce Order Processing**: Весь цикл, от заказа покупателя до его fulfilment, — цельная и согласованная единица работы.

- **Messaging Services**: Например, сообщение отправляется только после того, как оно полностью составлено, а не на середине.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Что такое the differences between SQL and NoSQL dataBASEs?</summary>

### Что такое the differences between SQL and NoSQL dataBASEs?

**Definition:**

Базы SQL и NoSQL различаются в нескольких ключевых аспектах. Здесь я разберу пять из них.

**How it works:**

#### Ключевые отличия

- **Data Structure**: Базы SQL устроены как таблицы, а базы NoSQL могут быть документными, ключ-значение, широкими столбцами или графовыми.

- **Schema**: Базы SQL основаны на схеме. Им нужна схема базы, и любое отклонение требует изменений схемы. Базы NoSQL либо ad-hoc, так что каждый документ согласован с другими, либо schema-on-read.

- **Querying**: Базы SQL выполняют запросы на Structured Query Language. Базы NoSQL используют методы, специфичные для своей модели данных.

- **Scalability**: Исторически базы SQL масштабировались вертикально, «scaling up», наращивая CPU, память и хранилище на одном узле. Однако последние тенденции показывают поддержку горизонтального масштабирования, «scaling out», на несколько узлов. Базы NoSQL по природе легче масштабируются горизонтально, это архитектуры «scale-out».

- **ACID**: Традиционные базы SQL часто гарантируют соответствие ACID (Atomicity, Consistency, Isolation и Durability). Базы NoSQL, особенно в моделях eventual consistency, могут жертвовать немедленной согласованностью ради производительности и доступности.

#### Эволюция и адаптация

- **Consistency Models**: SQL часто означает немедленную или строгую согласованность. NoSQL даёт ряд моделей согласованности, от eventual consistency до сильной.

- **Data Relationships and Transactions**: Базы SQL обычно обеспечивают связи данных методами вроде внешних ключей и поддерживают более сложное транзакционное поведение. Базы NoSQL могут жертвовать частью этих возможностей ради большей производительности.

- **Use Cases and Popularity**: Базы SQL проверены временем, особенно там, где нужна строгая согласованность или ясные связи данных. Базы NoSQL популярны гибкостью, особенно при быстро меняющихся или растущих потребностях в данных, и в области больших данных и других современных вычислительных парадигм.

#### Следование предметной области

- **Relational Integrity**: Базы SQL гордятся тем, что обеспечивают ссылочную целостность реляционных данных.

- **Efficiency in Certain Query Workloads**: Базы SQL часто предпочитают там, где сложные запросы или соединения нескольких таблиц, из-за возможностей оптимизатора. NoSQL силён в отдельных видах запросов, например поиск по ключу, благодаря своим моделям данных.

- **Ease of Horizontal Scaling**: NoSQL часто выбирают там, где нужны высокая доступность и распределённые данные.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Опишите a relational dataBASE schema.</summary>

### Опишите a relational dataBASE schema.

**Definition:**

Реляционная схема базы — это набор правил, которые задают структуру, ограничения и связи таблиц данных в Relational Database Management System (RDBMS).

**How it works:**

#### Основные компоненты

- **Tables**: Главные контейнеры данных, которые часто называют «relations».

Задаются именами столбцов и типами данных.

- **Columns**: Атрибуты или поля, заданные именем, типом и необязательными ограничениями.

У каждого столбца должно быть уникальное имя внутри таблицы.
Обычные типы данных — целые, строки, даты и другие.

- **Rows**: Отдельные записи данных, составленные по столбцам, которые заданы у таблицы.

У каждой строки должен быть уникальный идентификатор, который часто называют «primary key».

#### Виды ключей

- **Primary Key (PK)**: Уникальный идентификатор каждой записи в таблице. Часто это целое с автоинкрементом. У каждой таблицы должен быть ровно один первичный ключ. Это Essential, обозначенное как «E» в базах данных.

- **Foreign Key (FK)**: Поле или группа полей в таблице, которые однозначно указывают строку в другой таблице. Обычно это первичный ключ другой таблицы, и они помогают устанавливать связи между таблицами. Этот термин выведен как «F» из Candidate Key в базах данных

#### Виды связей

- **One-to-One**: Каждая запись первой таблицы связана ровно с одной записью второй, и наоборот.

- **One-to-Many**: Запись первой таблицы может быть связана с одной или несколькими записями второй, но запись второй связана только с одной записью первой.

- **Many-to-Many**: Записи обеих таблиц могут быть связаны с несколькими записями другой. Для такой связи нужна связующая таблица.

#### Обычные ограничения

- **NOT NULL**: Указывает, что у столбца должно быть значение и его нельзя оставить пустым. Это Essential, обозначенное как «E» в базах данных.

- **UNIQUE**: Требует, чтобы все значения столбца или набора столбцов были различны.

- **CHECK**: Позволяет применять к данным столбца условные проверки.

- **DEFAULT**: Автоматически подставляет заранее заданное значение, когда создаётся новый столбец.

- **INDEX**: Ускоряет получение данных, создавая индекс по столбцу или столбцам и ускоряя соответствующие запросы.

**Examples:**

#### Code Example: Schema for University Database

Вот код на SQL:

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
<summary>Что такое Primary key, and why is it important?</summary>

### Что такое Primary key, and why is it important?

**Definition:**

Первичный ключ — уникальный идентификатор таблицы. Он обеспечивает, что каждая запись в таблице отличается от других и на неё можно эффективно ссылаться.

**How it works:**

#### Основные свойства

- **Uniqueness**: У каждой записи в таблице должен быть уникальный первичный ключ.

- **Non-Null Value**: Поле первичного ключа не может содержать null или неопределённые значения.

- **Stability**: Менять значения первичного ключа следует редко, чтобы он оставался надёжным идентификатором.

#### Выгоды

- **Data Integrity**: Обеспечивает точность и согласованность набора данных.

- **Data Retrieval**: Помогает эффективно получать данные и легко ссылаться на связанные таблицы через Foreign Keys.

- **Data Normalization**: Помогает раскладывать данные по нескольким таблицам, уменьшать избыточность и улучшать целостность.

- **Table Relationships**: Служит основой для разных видов связей с другими таблицами (one-to-one, one-to-many, many-to-many).

#### Требования RDBMS

#### MySQL

- **Character Limit**: 1000 символов суммарно по всем столбцам, заданным для первичного ключа.

- **Restrictions**: Столбцы text и blob в первичных ключах не разрешены.

- **Supported Types**: Можно использовать большинство типов столбцов, включая целые, строки и даты.

#### PostgreSQL

- **Character Limit**: 32 символа для имени ограничения первичного ключа, которое по умолчанию table_name_pkey.

- **Supported Types**: Большинство типов столбцов можно задать как первичные ключи, включая UUID.

#### SQL Server

- **Character Limit**: 900 байт.

- **Restrictions**: Столбцы text и image запрещены в первичных ключах.

- **Supported Types**: Тип uniqueidentifier часто используют как первичный ключ.

#### SQLite

- **Character Limit**: Фиксированной длины не требуется.

- **Supported Types**: Большинство типов столбцов, включая целые, можно использовать для первичных ключей.

#### Oracle Database

- **Character Limit**: Имя ограничения PRIMARY KEY вместе с именем схемы не может превышать 30 символов.

- **Supported Types**: Можно использовать большинство типов данных столбцов, включая столбцы LOB.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Можете ли вы explain what a foreign key is and its role in the dataBASE?</summary>

### Можете ли вы explain what a foreign key is and its role in the dataBASE?

**Definition:**

Внешний ключ (FK) устанавливает связь между двумя таблицами в реляционной базе.

**How it works:**

#### Ключевые свойства

- **Column Type**: Столбец FK в дочерней таблице должен иметь тот же тип данных, что и столбец первичного ключа в родительской.

- **Referential Integrity**: FK обеспечивает согласованность данных: отклоняет изменение, ставит NULL или каскадирует изменения.

- **Joining Tables**: Внешний ключ в запросах SQL помогает соединять связанные данные из нескольких таблиц.

Оператор JOIN — это команда SQL, центральная для соединения таблиц через внешние ключи:

#### Виды соединений

- **Inner Join**: Сопоставляет только записи, у которых есть связанные строки в обеих таблицах.

- **Outer Join**: Включает записи из одной таблицы, даже если во второй нет связанных строк.

#### Проверка связей ключей

- **SYS.TYPE_CATALOG использует системные представления для всех трёх видов ключей**: primary, foreign и unique. Он показывает таблицы и соответствующие им ключи.

INFORMATION_SCHEMA.KEY_COLUMN_USAGE даёт подробные сведения о ключах, например таблицу, где лежат ключи, а также кардинальность.

**Examples:**

#### Примеры и разбор кода

Вот код на SQL:

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

В этом коде author_id в таблице books — внешний ключ, который указывает на author_id в таблице authors. Оператор SELECT использует JOIN, чтобы объединить связанные данные книги и автора.

#### Например, в SQL SERVER:

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
<summary>Что такое dataBASE normalization, and why do we use it?</summary>

### Что такое dataBASE normalization, and why do we use it?

**Definition:**

Нормализация базы — это набор практик, которые обеспечивают целостность данных, уменьшая избыточность и зависимость внутри реляционной базы.

**How it works:**

#### Плюсы нормализации

- **Data Consistency**: Меньше избыточности — меньше риск несогласованных данных.

- **Improved Maintainability**: При более устроенной базе сопровождение становится проще.

- **Easier Updates**: Нормализация обычно означает, что обновлять нужно меньше записей.

#### Уровни нормализации

Обычно выделяют шесть уровней нормализации, от 0 до 5, или «BCNF».

#### Первая нормальная форма (1NF)

- **Unique Primary Keys**: У таблицы должен быть уникальный первичный ключ для каждой записи.

- **Atomic Values**: Каждая ячейка таблицы должна хранить одно значение, без многозначных атрибутов.

#### Вторая нормальная форма (2NF)

Все требования предыдущей формы, а также:

- **Removal of Partial Dependencies**: Столбцы, не входящие в первичный ключ, должны зависеть от всего первичного ключа.

#### Третья нормальная форма (3NF)

Все требования 2NF, а также:

- **Elimination of Transitive Dependencies**: Столбцы вне первичного ключа не должны зависеть от других столбцов вне первичного ключа.

#### Нормальная форма Бойса-Кодда (BCNF)

Более строгий вариант 3NF, который обеспечивает, что каждый детерминант является потенциальным ключом.

#### Четвёртая нормальная форма (4NF)

Имеет дело с многозначными зависимостями.

#### Пятая нормальная форма (5NF)

Её также называют «Projection-Join Normal Form», и она имеет дело с зависимостями соединения.

**When to use:**

#### Нормальные формы и случаи применения

Большинство реляционных баз стремятся к 3NF: обычно это хороший баланс производительности и целостности данных.

Однако важно понять конкретные требования своей базы и соответственно выбрать подходящий уровень нормализации. Например, базы для отчётов могут быть нормализованы не сильно, чтобы ускорить запросы.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Что такое denormalization and when would you consider it?</summary>

### Что такое denormalization and when would you consider it?

**Definition:**

Денормализация — это снижение нормализации (обычно ради производительности) за счёт избыточных данных в одной или нескольких таблицах. Нормализация обеспечивает целостность данных, а денормализация в основном улучшает скорость запросов, убирая сложные операции JOIN.

**How it works:**

#### Выгоды и недостатки

- **CTEs**: При нескольких Common Table Expressions выгоды нормализованной схемы можно сохранить и в денормализованной схеме и, возможно, убрать недостатки.

- **Maintainability**: Денормализация может сделать данные труднее сопровождать и держать согласованными.

- **Query Performance**: Запросы могут стать эффективнее, потому что данные, которые раньше брали из разных нормализованных таблиц, теперь лежат в одной денормализованной.

- **Storage Efficiency**: Запросы к нескольким меньшим таблицам требуют меньше дискового ввода-вывода и легче помещаются в память, чем запросы к нескольким более крупным таблицам.

- **Data Integrity**: Избыточные данные могут привести к несогласованности, если ими не управлять как следует.

**When to use:**

#### Соображения по случаям

- **Lookup Tables**: Таблицы со сравнительно статичными данными (вроде типов Status или сведений о Location), которые часто соединяют с другими таблицами системы, — сильные кандидаты на денормализацию. Частые поиски могут стать быстрее.

- **Reporting and Analytics**: Системы, смещённые к чтению, а не к записи, выигрывают от денормализации. При проектировании систем отчётов или аналитики обычная практика — отдельные реплики для чтения, денормализованные ради более быстрого чтения.

- **Partitioning**: В больших системах денормализацию можно использовать, чтобы разнести данные по разным таблицам или кластерам и распределить нагрузку. Например, данные клиентов можно выделить в один кластер, а данные заказов — в другой, связав их денормализацией.

**Examples:**

#### Code Example: Denormalization and Normalization

Вот код на SQL:

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
<summary>Сравните and contrast the DROP, DELETE, and TRUNCATE commands.</summary>

### Сравните and contrast the DROP, DELETE, and TRUNCATE commands.

**Definition:**

Посмотрим на три команды — DROP, DELETE и TRUNCATE — с точки зрения функции, работы, скорости и восстановления.

Оператор DROP TABLE удаляет из базы всю таблицу и её структуру.

**How it works:**

#### Управление транзакцией

- **Implicit Commit**: Действие сразу фиксируется, и его последствия необратимы.

#### Соображения

- **Irreversible**: Удаляет таблицу целиком; записи по строкам не удаляет.

- **Data Loss**: Любые данные в таблице удаляются навсегда.

Оператор DELETE работает с отдельными строками таблицы и обратим.

#### Управление транзакцией

- **Needs a Transaction**: Работает внутри транзакции и должен быть явно зафиксирован, чтобы стать постоянным.

#### Соображения

- **Precision**: Удаляет конкретные строки по заданному условию.

- **Recoverability**: Данные можно вернуть или восстановить во время транзакции, если она ещё не зафиксирована.

Оператор TRUNCATE TABLE быстро удаляет все строки таблицы и даёт и скорость, и простоту. Однако он не освобождает выделенное хранилище так, как это делает DELETE.

#### Управление транзакцией

- **Requires Commit**: Как и DELETE, этот оператор работает внутри транзакции и требует явной фиксации, чтобы завершить действия.

#### Соображения

- **Efficiency**: Работает значительно быстрее, чем DELETE. Подходит, чтобы удалить все записи и в отдельных приложениях сбросить состояние таблицы.

- **No Selective Deletion**: В отличие от DELETE, очищает все записи таблицы без оглядки на конкретные условия.

- **Transaction and Recovery**: После выполнения удаление данных обычно можно отменить или откатить до окончательной фиксации или завершения транзакции.

**Examples:**

```sql
DROP Table
```

#### Синтаксис SQL

```sql
DROP TABLE table_name;
```

```sql
DELETE from Table
```

#### Синтаксис SQL

```sql
DELETE FROM table_name WHERE condition;
```

```sql
TRUNCATE Table
```

#### Синтаксис SQL

```sql
TRUNCATE TABLE table_name;
```

#### Code Example: Table Cleanup Operations

Вот код на PostgreSQL:

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
<summary>Что такое the difference between a full join and an inner join?</summary>

### Что такое the difference between a full join and an inner join?

**Definition:**

Полные и внутренние соединения служат разным целям в системах управления базами и ведут себя по-разному.

**How it works:**

#### Внутреннее соединение

Внутреннее соединение находит и соединяет совпадающие записи родительской и дочерней таблиц по заданному условию соединения. Записи, у которых нет пары ни в одной из таблиц, из результата исключаются.

#### Полное соединение

Полное соединение, также известное как полное внешнее, возвращает все записи обеих таблиц и подставляет NULL там, где во второй таблице нет совпадения. Такой вид соединения используют реже остальных, потому что он может породить очень большие результаты.

#### Наглядное представление

#### Соединения SQL

**Examples:**

#### Синтаксис SQL

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```

#### Синтаксис SQL

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
<summary>Как бы вы write an SQL query to fetch duplicate records from a table?</summary>

### Как бы вы write an SQL query to fetch duplicate records from a table?

**Definition:**

Посмотрим на два обычных способа найти и обработать дубликаты записей в SQL.

**How it works:**

#### 2. Using ROW_NUMBER()

Этот способ особенно полезен, когда нужно ещё и отслеживать идентификаторы «дубликатов».

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

В обоих способах column1, column2, ..., columnN — это столбец или набор столбцов, по которым вы ищете дубликаты.

#### Code Example: Standard SQL

```sql
SELECT name, age, COUNT(*)
FROM mytable
GROUP BY name, age
HAVING COUNT(*) > 1;
```

В этом примере мы ищем дубликаты по столбцам «name» и «age» из «mytable».

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

В этом примере мы используем ROW_NUMBER(), чтобы найти записи с одинаковыми «name» и «age» и сохранить уникальный номер строки для каждой.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Что такое prepared statement, and why would you use one?</summary>

### Что такое prepared statement, and why would you use one?

**Definition:**

Prepared Statements снижают риск SQL-инъекции, разделяя данные SQL и команды. Они также ускоряют выполнение запроса, позволяя многократно подставлять параметры.

**How it works:**

#### Главные преимущества

- **Security**: Они защищают от SQL-инъекции, отличая код SQL от входных значений.

- **Performance**: Prepared statements могут быть быстрее при повторном использовании: их разбирают и выполняют отдельными шагами.

- **Readability and Maintainability**: Если отделить код SQL от параметров, его легче читать. Код проще понимать, рецензировать и сопровождать.

**When to use:**

#### Когда использовать prepared statements

- **Input from Untrusted Sources**: Когда запросы SQL строят из данных недоверенных источников, prepared statements обеспечивают, что данные трактуются как литералы, и это предотвращает SQL-инъекцию.

- **Repeated Executions**: Для запросов, которые выполняют много раз, prepared statement может быть эффективнее, чем каждый раз заново строить и выполнять запрос. Это особенно уместно в циклах или при большом объёме операций.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/databases-interview-questions/
</details>

<details>
<summary>Что такое the N+1 query problem and how can you solve it?</summary>

### Что такое the N+1 query problem and how can you solve it?

**Definition:**

Проблема N+1 Query возникает, когда граф объектов получают запросом, который даёт лишние обращения к базе.

Например, связь один-ко-многим, где у каждого «Order» много «LineItems». Если получить все «Orders», а затем по отдельности их «LineItems», получается несколько кругов запросов. Эти лишние запросы неэффективны, особенно когда связанных элементов много.

Скажем, в более конкретном случае вы получаете:

Все Orders.
Затем для каждого Order — его LineItems.

**How it works:**

#### Это соответствует:

- **Primary Query**: SELECT * FROM Orders.

- **Secondary Query**: SELECT * FROM LineItems WHERE order_id = :order_id (потенциально выполняется несколько раз в зависимости от числа заказов).

Здесь сценарий N+1, потому что второй запрос выполняется «N» раз, по разу на каждый Order, отсюда N+1.

#### Решения

#### Использовать запрос с JOIN

С помощью JOIN можно получить связанные сущности одним запросом.

#### Опереться на ленивую загрузку

Некоторые ORM настроены выполнять дополнительные запросы только когда связанный объект впервые читают. Это называют ленивой загрузкой. Она уменьшает первый запрос и повышает эффективность, но может привести к проблеме N+1, если читают много связанных сущностей.

#### Явная жадная загрузка

Современные ORM часто дают способы явно указать, какие связанные сущности грузить сразу. Так можно сохранить выгоду ленивой загрузки и при этом стратегически брать данные сразу, когда это нужно. В Entity Framework (EF) и Java Persistence API (JPA), например, аннотациями вроде @ManyToOne и @OneToMany управляют ленивой и жадной загрузкой.

#### Проекция данных

Вместо загрузки целых сущностей можно выбрать конкретные поля, нужные для ближайших операций. Это полезно, когда вас не интересуют все свойства сущности.

#### Ввести постраничную выдачу

На большом наборе данных часто разумнее брать страницы, а не всё сразу. Запрос SELECT можно изменить через OFFSET и LIMIT (в SQL Server и PostgreSQL) или функцию ROW_NUMBER() (в SQL Server и Oracle), чтобы сделать это аккуратно.

**Examples:**

#### SQL

```sql
SELECT o.*, li.*
FROM Orders o
JOIN LineItems li ON o.id = li.order_id;
```

Инструменты ORM ORM, такие как Hibernate, могут сделать это за вас. Вы можете даже не знать, использует ли ORM под капотом JOIN.

#### SQL

```sql
SELECT id, name FROM Orders;
```

В JPA можно использовать выражения конструктора в JPQL, чтобы добиться этого:

```java
TypedQuery<OrderSummary> query = em.createQuery(
    "SELECT NEW OrderSummary(o.id, o.name) FROM Order o", OrderSummary.class
);
```

А в Entity Framework можно использовать проекции LINQ:

```csharp
var orderSummaries = from order in context.Orders
                    select new OrderSummary { Id = order.Id, Name = order.Name };
```

#### SQL Server и PostgreSQL

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
<summary>Объясните the function of GROUP BY and HAVING clauses in SQL.</summary>

### Объясните the function of GROUP BY and HAVING clauses in SQL.

**Definition:**

GROUP BY и HAVING преобразуют, фильтруют и работают вместе с агрегатными функциями, чтобы получать более сложные и тонкие наборы результатов.

**How it works:**

#### Функции GROUP BY

- **Aggregation**: Выделяет группы и считает агрегатные функции (например, COUNT, SUM) для каждой группы.

- **Redundancy Reduction**: Схлопывает избыточные данные по столбцам группировки и даёт более компактный набор.

- **Column Constraint**: Позволяет запросам выбирать только агрегированные столбцы, а из исходных столбцов все неагрегированные должны быть в GROUP BY, чтобы представление данных было согласованным.

#### Функции HAVING

- **Group-Wise Filtering**: Применяет условные проверки к сгруппированным данным.

- **Post-Group Filtering**: Позволяет фильтровать по результатам агрегатных функций.

- **Aggregation Assertion**: Проверяет агрегированные значения на конкретные условия.

- **Comparison with WHERE**: Даёт более продвинутую фильтрацию с учётом агрегатов по сравнению с глобальной фильтрацией WHERE до агрегации.

#### Рассмотрим такую реляционную схему:

#### EMPLOYEE (ID, Name, DeptID, Salary)

#### DEPARTMENT (ID, Name)

Чтобы получить идентификаторы отделов, где средняя зарплата выше некоторого порога:

**Examples:**

#### Code Example: GROUP BY & HAVING

#### SQL-запрос

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
<summary>Что такое indexes and how do they work in dataBASEs?</summary>

### Что такое indexes and how do they work in dataBASEs?

**Definition:**

Индекс служит структурой данных в системе базы. Он повышает эффективность и скорость поиска данных, логически упорядочивая индексированные данные.

**How it works:**

#### Виды индексов

- **B-Tree (Balanced Tree)**: Подходит для диапазонов и проверок равенства, например условий WHERE в SQL.

- **Hash**: Идеален для поиска точного совпадения, как у первичных ключей.

- **Full-text**: Специально для поиска по тексту, часто встречается в поисковых системах.

- **Bitmap**: Эффективен для столбцов с низкой кардинальностью, где различных значений мало, например пол.

#### Поиск данных через индексы

- **Ordered Scans**: Возможны через индексы B-Tree, где данные отсортированы, и это помогает запросам по диапазону.

- **Exact-Match Sequencing**: Для хеша и индексов на деревьях это даёт быстрые совпадения точного значения.

- **Range Searches**: Их поддерживают B-Tree, и они позволяют, например, найти числа внутри диапазона.

#### Практические приёмы

- **Consider Index Maintenance Overhead**: Индексы ускоряют чтение, но могут замедлять изменения данных вроде INSERT, UPDATE и DELETE.

- **Index Tailing**: Сначала ставьте более избирательные столбцы, а за ними менее различающие, чтобы результат был лучше.

- **Index Coverage**: Старайтесь покрыть часто запрашиваемые столбцы, но не переусердствуйте, иначе индекс раздуется.

**Examples:**

#### Code Example: Index Types in PostgreSQL

Вот код на SQL:

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
<summary>Какое влияние JOIN operations оказывают на database performance?</summary>

### Какое влияние JOIN operations оказывают на database performance?

**Definition:**

PostgreSQL строит соединение двух входов за раз, а число возможных порядков растёт экспоненциально с числом таблиц.

**How it works:**

Глава про явные JOIN пишет, что при большем числе таблиц поиск дорожает, и `geqo_threshold` переключает планировщик на генетический поиск. `FULL JOIN` полностью фиксирует допустимый порядок. `LEFT JOIN` и `RIGHT JOIN` фиксируют его частично. Явный `INNER JOIN` или `CROSS JOIN` порядок не фиксирует, пока `join_collapse_limit` не равен 1. Соединение без ограничивающего условия — декартово произведение двух входов, поэтому смотреть надо стоимость плана в `EXPLAIN`, а не одну метку Big-O.

**Sources:**

- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
</details>

<details>
<summary>Определите what a subquery is and provide a use case for it.</summary>

### Определите what a subquery is and provide a use case for it.

**Definition:**

Скалярный подзапрос — это `SELECT` в скобках, который обязан вернуть одну строку и один столбец. Ноль строк дают null. Больше одной строки или столбца — ошибка.

**How it works:**

Внешнее выражение использует это единственное значение. Подзапрос в `FROM` планируется отдельно, пока планировщик его не схлопнет. Схлопывание происходит, только если в родительском `FROM` останется не больше `from_collapse_limit` элементов (по умолчанию восемь). Схлопывание может дать лучший план и одновременно более тяжёлый поиск порядка соединений.

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
<summary>Что такое correlated subquery?</summary>

### Что такое correlated subquery?

**Definition:**

Коррелированный подзапрос ссылается на переменные внешнего запроса. На время одного вычисления эти внешние значения ведут себя как константы.

**How it works:**

Страница выражений формулирует это для скалярных подзапросов. Подзапрос вычисляется заново со значениями текущей внешней строки. Он по-прежнему обязан вернуть не больше одной строки и одного столбца; лишние строки или столбцы — ошибка, а отсутствие строки даёт null.

**Sources:**

- [Value Expressions — Scalar Subqueries](https://www.postgresql.org/docs/18/sql-expressions.html)
</details>

<details>
<summary>Опишите how you would optimize a SLOw SQL query.</summary>

### Опишите how you would optimize a SLOw SQL query.

**Definition:**

Начинают с плана. `EXPLAIN` печатает дерево, стоимости которого выражены в единицах `seq_page_cost` (по умолчанию 1.0), а стоимость родителя включает детей.

**How it works:**

Для запроса с большим числом соединений рычаг описан в главе про явные JOIN: `join_collapse_limit` = 1 делает записанный порядок `JOIN` порядком плана, а `from_collapse_limit` не даёт схлопнуть подзапрос в более широкий поиск. `EXPLAIN` показывает, выбрал ли планировщик последовательное или индексное сканирование; класс Big-O руководство для этого выбора не печатает. `EXPLAIN ANALYZE` выполняет оператор и добавляет фактическое время.

**Sources:**

- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Other Planner Options](https://www.postgresql.org/docs/18/runtime-config-query.html)
</details>

<details>
<summary>Объясните the EXPLAIN statement and how you use it in query optimization.</summary>

### Объясните the EXPLAIN statement and how you use it in query optimization.

**Definition:**

`EXPLAIN` печатает дерево плана и не обязан выполнять оператор. У узла есть `cost=startup..total`, оценка `rows` — сколько строк он выдаёт — и `width`.

**How it works:**

Стоимости — условные единицы от `seq_page_cost`, не миллисекунды. Итог родителя включает детей. `EXPLAIN ANALYZE` выполняет запрос и рядом с оценками показывает фактические строки и время. Форматы вывода: text, XML, JSON и YAML. Большой разрыв оценки и факта — повод смотреть статистику или форму соединений, а не число, которое команда сама переписывает.

**Sources:**

- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
</details>

### Data Modeling and Design

<details>
<summary>Что такое Entity-Relationship (ER) model, and why is it useful?</summary>

### Что такое Entity-Relationship (ER) model, and why is it useful?

**Definition:**

Руководство PostgreSQL не задаёт нотацию ER-диаграмм. Документированная форма связи — таблица с первичным ключом и другая таблица, которая на него ссылается через `REFERENCES`.

**How it works:**

В учебнике внешних ключей `cities.name` — первичный ключ, а `weather.city` на него ссылается. База отвергает строку погоды с городом, которого нет в `cities`. Именно на эту целостность в этой системе отображается «сущность и связь» из ER.

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
<summary>Опишите the process of converting an ER model into a relational dataBASE schema.</summary>

### Опишите the process of converting an ER model into a relational dataBASE schema.

**Definition:**

Каждая сущность становится таблицей. Её идентификатор — `PRIMARY KEY`: столбец `NOT NULL` с уникальным индексом B-tree. Связь становится столбцом `REFERENCES` или ограничением внешнего ключа.

**How it works:**

Глава ограничений показывает `product_no integer PRIMARY KEY`. Учебник показывает `weather.city`, который ссылается на `cities(name)`. `ALTER TABLE ... ADD FOREIGN KEY` добавляет связь позже, и уже лежащие строки обязаны ей удовлетворять. `CHECK`, например `name <> ''`, кодирует правило, которое не является ключом.

**Sources:**

- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
</details>

<details>
<summary>Как you design a Scalable dataBASE schema for a high-traffic application?</summary>

### Как you design a Scalable dataBASE schema for a high-traffic application?

**Definition:**

Документированное разбиение очень большой таблицы в PostgreSQL — секционирование: одна логическая таблица и много физических секций, range, list или hash.

**How it works:**

Глава о секционировании говорит, что выигрыш обычно есть лишь когда таблица больше физической памяти сервера. Запросы к одной секции могут сканировать её, а не всю таблицу, а удаление секции через `DROP TABLE` или `ALTER TABLE DETACH PARTITION` обходит массовый `DELETE` и его `VACUUM`. У родителя нет своего хранилища; строки уходят по ключу секции. Первичный ключ и внешние ключи по-прежнему держат целостность созданных таблиц.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
</details>

<details>
<summary>Что такое dataBASE sharding, and what are its benefits and drawbacks?</summary>

### Что такое dataBASE sharding, and what are its benefits and drawbacks?

**Definition:**

Глава о секционировании описывает разбиение одной логической таблицы на меньшие физические части. Отдельный маршрутизатор шардов она не описывает. Секции могут быть сторонними таблицами, и тогда пользователь сам обязан держать строки внутри границ секции.

**How it works:**

Документированные плюсы: индексы горячей секции чаще помещаются в память; запрос, который задевает большую часть одной секции, может идти последовательным сканированием этой секции; массовая загрузка и удаление становятся attach и detach; холодные данные можно держать на медленном носителе. Та же страница говорит, что это окупается только для очень большой таблицы, ориентир — таблица больше памяти сервера. Обычную таблицу нельзя на месте превратить в секционированную.

**When to use:**

Декларативное секционирование уместно, когда одна таблица огромна, а запросы или срок хранения совпадают с ключом, например диапазоном дат. Секция-сторонняя таблица — это разнесение по серверам только если вы принимаете, что PostgreSQL сам чужие строки не проверит.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
</details>

<details>
<summary>Объясните the term “data integrity” and how it’s enforced in dataBASEs.</summary>

### Объясните the term “data integrity” and how it’s enforced in dataBASEs.

**Definition:**

В PostgreSQL ограничения целостности отвергают строки, которые нарушают объявленное правило. Первичный ключ требует `NOT NULL` и уникальность. `UNIQUE`, `CHECK`, `NOT NULL` и `FOREIGN KEY` — остальные документированные средства.

**How it works:**

`ALTER TABLE products ADD CHECK (name <> '')` не выполнится, если хоть одна существующая строка нарушает проверку. Внешний ключ отвергает дочернюю строку без родителя, как `weather.city` со ссылкой на `cities(name)`. Ограничения проверяются при добавлении и при изменении строк; это не отдельная уборка.

**Sources:**

- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
</details>

### Transactions and Concurrency Control

<details>
<summary>Можете ли вы explain the concept of transaction isolation levels?</summary>

### Можете ли вы explain the concept of transaction isolation levels?

**Definition:**

В SQL названы четыре уровня изоляции. PostgreSQL реализует три поведения: `READ UNCOMMITTED` выполняется как `READ COMMITTED`. По умолчанию стоит `READ COMMITTED`.

**How it works:**

Глава изоляции говорит, что `REPEATABLE READ` в PostgreSQL также не допускает фантомное чтение — это строже минимума SQL для этого уровня. `SERIALIZABLE` не допускает аномалию сериализации; сбой даёт `could not serialize access due to read/write dependencies among transactions`, и эта транзакция откатывается. Уровень выбирают через `SET TRANSACTION`. Там же сказано, что последовательности не транзакционны.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
</details>

<details>
<summary>Что такое deadlock in dataBASEs, and how can it be resolved?</summary>

### Что такое deadlock in dataBASEs, and how can it be resolved?

**Definition:**

Взаимная блокировка — цикл, в котором каждая транзакция держит замок, нужный другой. PostgreSQL замечает цикл и прерывает одну транзакцию. На то, какую именно, полагаться нельзя.

**How it works:**

Пример главы блокировок — два `UPDATE` счетов 11111 и 22222 в разном порядке. Каждый ждёт строковый замок другого. Документированная защита — захватывать несколько объектов в одном и том же порядке и сразу брать самый строгий режим, который понадобится. Если так нельзя, повторяют прерванную транзакцию. Пока цикла нет, ожидающий замок ждёт бесконечно, поэтому `lock_timeout` (по умолчанию 0, миллисекунды) может прервать оператор, который ждёт слишком долго. Не держите транзакцию открытой, пока ждёте ввод пользователя.

**Sources:**

- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Client Connection Defaults](https://www.postgresql.org/docs/18/runtime-config-client.html)
</details>

<details>
<summary>Опишите optimistic vs. pessimistic locking.</summary>

### Опишите optimistic vs. pessimistic locking.

**Definition:**

Строковые замки из главы блокировок держатся до конца транзакции или до отката к точке сохранения. Другая транзакция, которой нужен конфликтующий замок, ждёт. Это конкуренция «сначала замок».

**How it works:**

Сериализуемый режим из главы изоляции — другой документированный путь: обе транзакции работают, и одна может упасть с `could not serialize access due to read/write dependencies among transactions`, а не ждать в цикле строковых замков. Слова optimistic и pessimistic руководство не использует. `SELECT ... FOR UPDATE` — явный строковый замок; обычное чтение строковые замки не блокируют, они блокируют писателей и тех, кто берёт замок.

**When to use:**

`FOR UPDATE` уместен, когда следующая запись должна увидеть строку, которую больше никто не меняет. `SERIALIZABLE` уместен, когда лучше дать базе прервать одну транзакцию, чем держать замки на длинном цикле чтения и записи.

**Sources:**

- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
- [SELECT](https://www.postgresql.org/docs/18/sql-select.html)
</details>

<details>
<summary>Как a dataBASE ensure consistency during concurrent transactions?</summary>

### Как a dataBASE ensure consistency during concurrent transactions?

**Definition:**

PostgreSQL сочетает уровни изоляции и замки. Изоляция по умолчанию — `READ COMMITTED`. Строковые замки не дают двум транзакциям держать конфликтующие замки одной строки.

**How it works:**

Конфликтующие замки одной строки транзакция может держать только в своих подтранзакциях. Остальные ждут или, при взаимной блокировке, одна транзакция прерывается. В `SERIALIZABLE` цикл зависимостей чтения и записи заканчивается ошибкой сериализации и откатом. Замки снимаются в конце транзакции или при откате к точке сохранения. Последовательности этой транзакционной согласованностью не покрыты.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
</details>

<details>
<summary>Что такое savepoint in a dataBASE transaction?</summary>

### Что такое savepoint in a dataBASE transaction?

**Definition:**

Точка сохранения — именованная отметка внутри транзакции. `ROLLBACK TO` отменяет работу после неё и снимает замки, взятые после неё.

**How it works:**

Страница SELECT показывает `BEGIN`, `SELECT ... FOR UPDATE`, `SAVEPOINT s`, `UPDATE` той же строки и `ROLLBACK TO s`. Строковый замок от `SELECT` взят до точки сохранения, поэтому он остаётся. Замки отменённого `UPDATE` снимаются — это правило главы блокировок: строковые замки кончаются в конце транзакции или при откате к точке сохранения.

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
<summary>Что такое SQL injection, and how do you prevent it?</summary>

### Что такое SQL injection, and how do you prevent it?

**Definition:**

SQL-инъекция — это сборка оператора вставкой ввода в текст SQL. `PREPARE` фиксирует структуру оператора и передаёт значения отдельно как `$1`, `$2` и далее.

**How it works:**

Страница PREPARE показывает `PREPARE fooplan (int, text, bool, numeric) AS INSERT INTO foo VALUES($1, $2, $3, $4)` и затем `EXECUTE fooplan(...)`. Типы объявлены у подготовленного оператора. Значение не становится частью синтаксиса SQL, поэтому кавычка или ключевое слово в тексте остаются данными.

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
<summary>Объясните the role of access control in dataBASE security.</summary>

### Объясните the role of access control in dataBASE security.

**Definition:**

Глава о шифровании документирует, кому можно открыть соединение, а не учебник прав на таблицы. Строки `pg_hba.conf` типа `host` разрешают незашифрованное соединение, `hostssl` требует SSL, `hostgssenc` требует шифрование GSSAPI.

**How it works:**

Пароли ролей хранятся как хеши, которые выбирает `password_encryption`. При аутентификации SCRAM или MD5 клиент шифрует пароль до отправки, поэтому открытый текст на сервере не появляется. Предпочтителен SCRAM. Аутентификация MD5 устарела и будет удалена. Сертификаты SSL дают клиенту и серверу проверить друг друга и мешают машине притвориться сервером ровно на время чтения пароля.

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
</details>

<details>
<summary>Что такое the best practices for storing passwords in a dataBASE?</summary>

### Что такое the best practices for storing passwords in a dataBASE?

**Definition:**

Пароли ролей базы хранятся как хеши. Алгоритм выбирает `password_encryption`. Предпочтителен SCRAM: это интернет-стандарт, и он сильнее аутентификации MD5 в PostgreSQL, которая устарела.

**How it works:**

Для столбца с паролем приложения pgcrypto документирует `crypt(password, gen_salt(...))`. Пример руководства — `gen_salt('md5')`. Этот хеш столбца отдельно от аутентификации роли. Расшифрованный или открытый секрет не должен быть хранимой формой; смысл главы о шифровании в том, что администратор не может прочитать пароль роли обратно из хеша.

**Examples:**

```sql
UPDATE users SET pswhash = crypt('new password', gen_salt('md5'));
```

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
- [pgcrypto](https://www.postgresql.org/docs/18/pgcrypto.html)
</details>

<details>
<summary>Как you secure data transmission to and from a dataBASE?</summary>

### Как you secure data transmission to and from a dataBASE?

**Definition:**

SSL-соединение шифрует пароль, запросы и возвращённые данные. Его требуют строкой `hostssl` в `pg_hba.conf`, а клиент может отказаться от сервера без SSL.

**How it works:**

Шифрование GSSAPI тоже закрывает запросы и результаты и не передаёт пароль. `hostgssenc` его требует; клиент ставит `gssencmode=require`. Stunnel или SSH — другие документированные туннели. Сертификаты с обеих сторон мешают посреднику, который выдаёт себя за сервер.

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
</details>

<details>
<summary>Опишите the use of encryption within dataBASEs for data at REST.</summary>

### Опишите the use of encryption within dataBASEs for data at REST.

**Definition:**

PostgreSQL документирует три слоя для данных в покое: шифрование столбцов через pgcrypto, шифрование файловой системы или диска и шифрование на клиенте.

**How it works:**

pgcrypto шифрует выбранные столбцы. Ключ даёт клиент; сервер расшифровывает и затем отправляет данные, поэтому открытый текст и ключ ненадолго есть на сервере. Шифрование диска (dm-crypt + LUKS, eCryptfs, EncFS, FreeBSD geli/gbde/PEFS) не даёт прочитать украденный диск и не защищает смонтированную файловую систему. Если администратору сервера нельзя доверять, клиент шифрует до отправки и сам расшифровывает ответы, так что открытый текст на сервере не появляется.

**Sources:**

- [Encryption Options](https://www.postgresql.org/docs/18/encryption-options.html)
- [pgcrypto](https://www.postgresql.org/docs/18/pgcrypto.html)
</details>

### Backup and Recovery

<details>
<summary>Что такое dataBASE snapshot, and when would you use one?</summary>

### Что такое dataBASE snapshot, and when would you use one?

**Definition:**

`pg_basebackup` снимает базовую копию работающего кластера, не блокируя других клиентов. Сервер сам входит в режим копирования и выходит из него. Эта копия — старт для восстановления на момент времени и для резервного сервера с доставкой журнала или потоковой репликацией.

**How it works:**

Копия — это весь кластер, не одна база. Полная копия — точная копия файлов кластера. Инкрементная хранит изменённые блоки и непригодна, пока `pg_combinebackup` не соберёт её с копиями, от которых она зависит. Логический `pg_dump` — другой артефакт: текст SQL, не файловый снимок, и он не может питать проигрывание WAL.

**When to use:**

Базовую копию снимают, когда нужны PITR или новый резервный сервер. `pg_dump` используют, когда одну базу надо перенести на более новую версию PostgreSQL или другую архитектуру процессора.

**Sources:**

- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>

<details>
<summary>Объясните the difference between logical and physical backups.</summary>

### Объясните the difference between logical and physical backups.

**Definition:**

Логическая копия `pg_dump` — это SQL, который заново создаёт базу. Физическая копия `pg_basebackup` — копия файлов кластера.

**How it works:**

`pg_dump` можно загрузить в более новую версию PostgreSQL и на другую архитектуру машины. Файловые копии и непрерывное архивирование привязаны к версии сервера. `pg_dump` и `pg_dumpall` не содержат того, что нужно проигрыванию WAL. `pg_dumpall` добавляет общекластерные объекты, такие как роли и табличные пространства; дамп каждой базы согласован сам по себе, а базы между собой не синхронизированы.

**Sources:**

- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>

<details>
<summary>Как бы вы RESTore a dataBASE from a backup file?</summary>

### Как бы вы RESTore a dataBASE from a backup file?

**Definition:**

Файл `pg_dump` — это SQL. Восстановление — повторная загрузка этого SQL на сервер, который может быть новее или другой архитектуры. Физическое восстановление начинается с `pg_basebackup` и идёт по шагам восстановления на момент времени из главы непрерывного архивирования.

**How it works:**

Инкрементная базовая копия одна не восстанавливается. `pg_combinebackup` должен собрать её с полной копией и инкрементами, от которых она зависит. `pg_dump` не может быть основой проигрывания WAL. Выборочное восстановление одной базы — это логический дамп; `pg_basebackup` всегда копирует весь кластер.

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
<summary>Что такое the common strategies for dataBASE disaster recovery?</summary>

### Что такое the common strategies for dataBASE disaster recovery?

**Definition:**

PostgreSQL документирует две стратегии. `pg_dump` даёт SQL, который можно загрузить заново. Непрерывное архивирование плюс базовая копия проигрывают WAL до выбранного момента.

**How it works:**

Логический дамп используют, чтобы перейти на более новую основную версию или другую архитектуру. `pg_basebackup` плюс архивный WAL — когда запасной сервер той же версии и нужен кластер на прошлый момент или как старт резервного сервера. `pg_dumpall` — логическая копия, которая включает ещё роли и табличные пространства.

**Sources:**

- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
</details>

<details>
<summary>Как point-in-time recovery work in dataBASEs?</summary>

### Как point-in-time recovery work in dataBASEs?

**Definition:**

Восстановление на момент времени поднимает файловую базовую копию и проигрывает архивный WAL до выбранной точки. `pg_basebackup` — документированный способ снять эту базовую копию с работающего кластера.

**How it works:**

Глава непрерывного архивирования прямо говорит, что `pg_dump` и `pg_dumpall` логические и не годятся для проигрывания WAL. Базовая копия привязана к версии, в отличие от SQL-дампа. Инкрементную копию перед восстановлением надо собрать через `pg_combinebackup`.

**Sources:**

- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
- [pg_basebackup](https://www.postgresql.org/docs/18/app-pgbasebackup.html)
- [SQL Dump](https://www.postgresql.org/docs/18/backup-dump.html)
</details>

### Performance Tuning and Scaling

<details>
<summary>Как бы вы handle a scenario where your dataBASE’s read load significantly increases?</summary>

### Как бы вы handle a scenario where your dataBASE’s read load significantly increases?

**Definition:**

Глава о секционировании говорит, что чтение ускоряется, когда нужные запросу строки лежат в одной секции или в немногих: это заменяет верхние уровни индекса и помогает удержать горячие страницы индекса в памяти.

**How it works:**

Если запрос читает большую часть одной секции, последовательное сканирование этой секции может обойти индексное сканирование, разбросанное по всей таблице. `EXPLAIN` показывает, какой узел плана получился на самом деле. Загруженные здесь страницы не документируют число реплик для чтения или размер пула.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
</details>

<details>
<summary>Какие стратегии exist for scaling writes in a high-volume dataBASE system?</summary>

### Какие стратегии exist for scaling writes in a high-volume dataBASE system?

**Definition:**

Стратегия записи для большой таблицы в главе о секционировании — добавлять и убирать секции вместо массового `DELETE` и направлять вставки по ключу секции.

**How it works:**

`ALTER TABLE ATTACH PARTITION` может добавить существующую таблицу как секцию. Изменение ключа секции переносит строку, если она больше не подходит. Отдельно глава блокировок говорит, что пропускная способность записи падает, если транзакции берут объекты в разном порядке или остаются открытыми в ожидании: ждущие замок ждут бесконечно, пока не задан `lock_timeout`. Это документированные рычаги записи, а не формула числа шардов.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Client Connection Defaults](https://www.postgresql.org/docs/18/runtime-config-client.html)
</details>

<details>
<summary>Опишите how connection pooling benefits dataBASE performance.</summary>

### Опишите how connection pooling benefits dataBASE performance.

**Definition:**

Страницы PostgreSQL 18, на которые опирается эта заметка, не документируют встроенный пул соединений и не дают формулу его выигрыша.

**How it works:**

Они документируют цену сеанса, который держит работу открытой. Транзакция, ждущая замок, ждёт бесконечно, когда `lock_timeout` равен 0 — это значение по умолчанию. Держать транзакцию открытой в ожидании ввода пользователя названо плохой идеей, потому что другие сеансы стоят в очереди за её замками. `lock_timeout` прерывает только ожидание замка и применяется к каждой попытке захвата, в миллисекундах.

**Sources:**

- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
- [Client Connection Defaults](https://www.postgresql.org/docs/18/runtime-config-client.html)
</details>

<details>
<summary>Как you monitor and identify dataBASE performance boTTLenecks?</summary>

### Как you monitor and identify dataBASE performance boTTLenecks?

**Definition:**

`EXPLAIN` и `EXPLAIN ANALYZE` — документированные инструменты. Сравнивают `rows` и `cost` планировщика с фактическими строками и временем из `ANALYZE`.

**How it works:**

Стоимости в единицах `seq_page_cost`. Дорогой родитель из-за ребёнка указывает на этого ребёнка. Последовательное сканирование огромной таблицы или поиск соединений, который перешёл `geqo_threshold`, видно по форме плана. `debug_deadlocks` сбрасывает сведения о замках, когда срабатывает таймаут взаимной блокировки, и только если сервер собран с `LOCK_DEBUG`.

**Sources:**

- [Using EXPLAIN](https://www.postgresql.org/docs/18/using-explain.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
- [Explicit Locking](https://www.postgresql.org/docs/18/explicit-locking.html)
</details>

<details>
<summary>Что такое horizontal and vertical scaling in dataBASEs?</summary>

### Что такое horizontal and vertical scaling in dataBASEs?

**Definition:**

Глава о секционировании не использует слова «горизонтальное» и «вертикальное». Она говорит, что секционирование окупается, когда таблица больше физической памяти сервера, и что секции могут быть сторонними таблицами на других серверах.

**How it works:**

Больший объём памяти сервера — это вертикальный факт в том ориентире: таблицу сравнивают с RAM одной машины. Разбиение логической таблицы на секции, включая сторонние, — документированный способ разложить части больше чем в одном месте. У сторонних секций нет автоматической проверки, что строки совпадают с границами. Встроенные методы — range, list и hash.

**Sources:**

- [Table Partitioning](https://www.postgresql.org/docs/18/ddl-partitioning.html)
</details>

### NoSQL Databases

<details>
<summary>Объясните what a document store is and give an example of where it’s appropriate to use.</summary>

### Объясните what a document store is and give an example of where it’s appropriate to use.

**Definition:**

PostgreSQL не описан как продукт «хранилище документов». Он хранит JSON-документы в столбцах `jsonb` внутри реляционных таблиц.

**How it works:**

Оператор вхождения `jsonb` `@>` из главы JSON проверяет, содержит ли левый документ правый. Порядок элементов массива и дубликаты не важны; глубина вложенности важна. Запрос вида «документ продукта содержит `{"version": 9.4}`» — документированное применение. Ограничения и соединения по-прежнему действуют на таблицу вокруг этого столбца.

**Examples:**

```sql
SELECT '{"product": "PostgreSQL", "version": 9.4, "jsonb": true}'::jsonb
    @> '{"version": 9.4}'::jsonb;
```

**Sources:**

- [JSON Types](https://www.postgresql.org/docs/18/datatype-json.html)
</details>

<details>
<summary>Что такое graph database, and what are its typical use cases?</summary>

### Что такое graph database, and what are its typical use cases?

**Definition:**

Страницы PostgreSQL 18, на которые опирается эта заметка, не определяют графовую базу, узлы и обход рёбер.

**How it works:**

Документированный способ задать связь — внешний ключ: `weather.city references cities(name)`. Проход по этой связи — соединение SQL, а его стоимость плана описывают главы про явные JOIN и `EXPLAIN`. Отдельное графовое хранилище за пределами этих страниц.

**Sources:**

- [Foreign Keys](https://www.postgresql.org/docs/18/tutorial-fk.html)
- [Controlling the Planner with Explicit JOIN Clauses](https://www.postgresql.org/docs/18/explicit-joins.html)
</details>

<details>
<summary>Опишите the key-value store model and its typical applications.</summary>

### Опишите the key-value store model and its typical applications.

**Definition:**

Загруженные главы PostgreSQL не определяют модель «ключ–значение». Ближайшая документированная структура — таблица, у которой идентификатор является первичным ключом.

**How it works:**

`PRIMARY KEY` требует `NOT NULL` и строит уникальный индекс B-tree. Строка тогда адресуется этим ключом, а остальные столбцы держат данные, в том числе значение `jsonb`, если данные — документ. Это по-прежнему реляционная таблица с ограничениями, а не отдельный движок «ключ–значение».

**Sources:**

- [Constraints](https://www.postgresql.org/docs/18/ddl-constraints.html)
- [JSON Types](https://www.postgresql.org/docs/18/datatype-json.html)
</details>

<details>
<summary>Как you choose between consistency and availability in a NoSQL dataBASE, referencing the CAP theorem?</summary>

### Как you choose between consistency and availability in a NoSQL dataBASE, referencing the CAP theorem?

**Definition:**

Глава транзакций PostgreSQL не формулирует теорему CAP и не даёт переключателя «согласованность или доступность» для NoSQL.

**How it works:**

Внутри одной базы PostgreSQL документированные рычаги согласованности — уровень изоляции и замки. По умолчанию `READ COMMITTED`. `REPEATABLE READ` также не пускает фантомы. `SERIALIZABLE` прерывает транзакцию, которая создала бы аномалию сериализации. Доступность второго узла на той странице не является параметром.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
</details>

<details>
<summary>Что такое eventual consistency, and in what scenarios is it used?</summary>

### Что такое eventual consistency, and in what scenarios is it used?

**Definition:**

Итоговая согласованность не является уровнем изоляции в PostgreSQL. Уровни руководства — имена из SQL, причём `READ UNCOMMITTED` ведёт себя как `READ COMMITTED`.

**How it works:**

Эффекты зафиксированной транзакции определяет уровень изоляции читателя, а не обещание, что реплики догонят позже. Глава непрерывного архивирования описывает резервный сервер, который проигрывает WAL с базовой копии; это доставка журнала на другой сервер, и страница не называет результат итоговой согласованностью. Последовательности — документированное исключение, которое не транзакционно.

**Sources:**

- [Transaction Isolation](https://www.postgresql.org/docs/18/transaction-iso.html)
- [Continuous Archiving and Point-in-Time Recovery](https://www.postgresql.org/docs/18/continuous-archiving.html)
</details>
