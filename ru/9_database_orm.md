# Database — ORM

Prisma, Sequelize, Knex, Drizzle и построители запросов.

[ ← Вернуться в хаб](./9_database.md)

### Related docs

* [БД — Основы](./9_database_core.md) · [БД — SQL](./9_database_sql.md) · [БД — NoSQL](./9_database_nosql.md) · [БД — ORM](./9_database_orm.md) · [БД — Производство](./9_database_production.md) · [БД — Интервью](./9_database_interview.md)

### Sections

- [ORM & application layer](#orm-application-layer)
- [Query builder](#query-builder)

---

## ORM & application layer

<details>
  <summary>ORM (concept)</summary>

### ORM (concept)

**Definition:** **Объектно-реляционное сопоставление** — сопоставляет таблицы базы данных с объектами приложения, генерируя SQL на основе вызовов методов.

**How it works:** Тяжелые отчеты, массовые ETL, критически важная для производительности аналитика — используйте необработанный SQL или построитель запросов.

**When to use:** Стандартные API-интерфейсы CRUD, быстрая разработка, команды, которые получают выгоду от схемы как кода.

**Pluses and minuses:**

| + | − |
| -------------------------------- | ----------------------------------------------- |
| Меньше шаблонного SQL | Утечки абстракций — в любом случае нужно знать SQL |
| Типобезопасные модели (с TS) | N+1 запросов, неэффективный генерируемый SQL |
|Интегрированные миграции | Сложно оптимизировать сложные отчеты |
| Переносимость баз данных (теоретически) | Переносимость нарушает особенности диалекта |

**Examples:**

```ts
// TypeORM / Sequelize / Prisma — same idea
const user = await prisma.user.create({
  data: { email: "a@b.com", posts: { create: [{ title: "Hello" }] } },
  include: { posts: true },
});
```

**Thoughts:** ORM — это **инструмент повышения производительности**, а не гарантия производительности. Профильные запросы в постановке с реальным объемом данных.

**Sources:**

[Prisma — Модель данных](https://www.prisma.io/docs/orm/prisma-schema/data-model)

---

</details>

<details>
  <summary>Prisma</summary>

### Prisma

**Definition:** ORM следующего поколения для Node.js/TypeScript — типобезопасный клиент с приоритетом схемы, миграция и Prisma Studio.

**When to use:** Новые серверные части TypeScript, команды хотят, чтобы схема была единственным источником истины.

**Pluses and minuses:**

| + | − |
| ---------------------------------------- | -------------------------------------------- |
| Отличный вывод TypeScript | В крайних случаях необходимы аварийные выходы из необработанного SQL |
| Декларативная схема, чистая миграция | Холодный старт/размер пакета в бессерверном режиме |
| Запросы отношений с `include`/`select` | Не идеально подходит для очень динамичных схем |
| Активная экосистема и документы | Связь с поставщиками (уменьшается экспортом SQL) |

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

- Использовать `prisma.$transaction([...])` для многоступенчатой ​​атомарности.
- `$queryRaw` с тегированным шаблоном для безопасного параметризованного необработанного SQL.
- Пул соединений: используйте PgBouncer в режиме транзакций или Prisma Accelerate для бессерверного режима.

**Sources:**

[Документация Prisma](https://www.prisma.io/docs)

---

</details>

<details>
  <summary>Sequelize</summary>

### Sequelize

**Definition:** ORM на основе обещаний для Node.js с поддержкой Postgres, MySQL, SQLite, MSSQL — модели, миграции, ассоциации.

**When to use:** Проекты Legacy Node, команды, уже работающие над Sequelize, требования к переносимости нескольких баз данных.

**Pluses and minuses:**

| + | − |
| ------------------------- | ------------------------------------- |
| Развитая, широкая поддержка БД | Подробный API против Prisma |
| CLI для миграций/начальных чисел | Поддержка TypeScript слабее, чем у Prisma |
| Гибкие крючки и прицелы | Легко писать запросы N+1 |

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

**Thoughts:** Предпочитать `include` с `attributes` предел. Использовать `sequelize.transaction()` для обновлений нескольких таблиц.

**Sources:** [Секвелизировать документы](https://sequelize.org/docs/v6/)

---

</details>

<details>
  <summary>Drizzle ORM</summary>

### Drizzle ORM

**Definition:**

Легкая ORM TypeScript с SQL-подобной схемой и запросами, минимальным временем выполнения и отличным выводом типов.


| Призма | Морось |
|  -----------  |  -------------  |
| Схема | `.prisma` ДСЛ | Таблицы TypeScript |
| SQL-контроль | Создано | Явный SQL-подобный |
| Размер пакета | Больше | Меньший |

**When to use:** Серверным модулям TypeScript требуется безопасность типов и прозрачность SQL; бессерверная версия с жесткими ограничениями на пакеты.

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

**Sources:** [Документация Drizzle ORM](https://orm.drizzle.team/docs/overview)

</details>

<details>
  <summary>Raw SQL vs ORM vs Query Builder</summary>

### Raw SQL vs ORM vs Query Builder

**Definition:**

Три способа общения с базой данных из кода приложения.

| Подход | Лучшее для | Следите за |
| --------------------------- | ---------------------------------------- | ------------------------------------------- |
|**Необработанный SQL** | Отчеты, массовые операции, функции, специфичные для БД | Строки SQL, внедрение, если они не параметризованы |
| **ОРМ** (Призма, продолжение) | CRUD, отношения, быстрая доставка | N+1, непрозрачный сгенерированный SQL |
| **Конструктор запросов** (Knex) | Динамические фильтры, миграции, SQL-контроль | Все еще строите струны, если неосторожно |
|Плюсы (ОРМ) | Плюсы (чистый SQL) |
| ----------------------- | ------------------------------------ |
| Типовая безопасность, миграция | Полный контроль оптимизатора |
| Меньше шаблонов SQL | Лучшая производительность для сложных запросов |

**When to use:** ORM для 80% кода приложения; необработанный SQL для 20%, которые профилируют медленно; Knex, когда вам нужна программная композиция запросов без полной ORM.

**Thoughts:** Гибрид нормальный — Prisma для CRUD+ `$queryRaw` для отчетов. Никогда не выбирайте в качестве идеологии «только ORM» или «только SQL».

**Sources:**

[Prisma — необработанные запросы](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql)

---

</details>

<details>
  <summary>N+1 problem</summary>

### N+1 problem

**Definition:** Один запрос загружает N родительских записей, затем N дополнительных запросов загружают каждую дочернюю запись — 1 + N циклов туда и обратно.

**When to use:** Всегда исправляйте это при перечислении сущностей со связями (пользователи + сообщения, заказы + элементы).

**Pluses and minuses:**

| + (исправить это) | − |
| ------------------------- | --------------------------------- |
| Значительное сокращение задержки | Нетерпеливая загрузка может привести к переизбытку данных |
| Меньше используется соединений с БД | Запросы JOIN могут быть сложными |

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

**Thoughts:** Включить ведение журнала запросов в dev (`prisma.$on('query')`, Сиквелизация `logging: console.log`). Один `include` с глубокой вложенностью может получить слишком много — используйте `select` ограничить столбцы.

**Sources:**

[Prisma — Реляционные запросы](https://www.prisma.io/docs/orm/prisma-client/queries/relation-queries)

---

</details>

<details>
  <summary>Connection pooling</summary>

### Connection pooling

**Definition:** Повторно используйте открытые соединения с базой данных вместо открытия нового подтверждения TCP + аутентификации для каждого запроса.

**When to use:** Каждое производственное приложение Node. Постгрес по умолчанию `max_connections` (~100) легко истощается без объединения.

**Pluses and minuses:**

| + | − |
| -------------------- | -------------------------------------------- |
| Меньшая задержка, меньше подключений к базе данных | Неправильная конфигурация пула = таймауты |
| Защищает БД от штормов соединения | Бессерверной системе требуется внешний пул (PgBouncer) |

**Examples:**

```ts
// node-postgres Pool
import { Pool } from "pg";
const pool = new Pool({ max: 20, connectionString: process.env.DATABASE_URL });

// Prisma — set connection_limit in URL or use PgBouncer
// DATABASE_URL="postgresql://...?connection_limit=10"
```

**Thoughts:**

- **PgBouncer** в режиме объединения транзакций для бессерверного подсчета/высокого количества экземпляров.
- Размер бассейна ≈ `(CPU cores * 2) + spindle count` Эмпирическое правило — измеряйте, а не гадайте.
- Всегда закрывайте пул при корректном завершении работы.

**Sources:**

[node-postgres — Пул](https://node-postgres.com/features/pooling)

---

</details>

<a name="query-builder"></a>

## Query builder

<details>
  <summary>Knex.js</summary>

### Knex.js

**Definition:** Построитель SQL-запросов для Node.js — свободный API, миграция и поддержка начальных значений без полного сопоставления ORM.

**When to use:** Панели администратора с динамическими фильтрами, проекты, требующие прозрачности SQL, устаревшие базы кода между необработанным SQL и ORM.

**Pluses and minuses:**

| + | − |
| ---------------------------------- | ------------------------------------- |
| Контроль над сгенерированным SQL | Нет автоматического сопоставления отношений |
| Запросы, не зависящие от базы данных (в основном) | Миграции менее эргономичны, чем Prisma |
| Отлично подходит для динамических фильтров | Вы сами управляете моделями/типами |

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

**Thoughts:** Knex по умолчанию использует параметризованные привязки — защищенные от внедрения. Сопряжение с `objection.js` если вы хотите, чтобы ORM поверх Knex.

**Sources:**

[Документация Knex.js](https://knexjs.org/)

---

</details>

<a name="production"></a>
