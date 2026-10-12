# Message broker — Core

Основы брокеров, модели доставки и выбор между Kafka, RabbitMQ и NATS.

[← Back to hub](./12_message_broker.md)

### Related docs

- [Message broker — Core](./12_message_broker_core.md) · [Message broker — Kafka](./12_message_broker_kafka.md) · [Message broker — RabbitMQ](./12_message_broker_rabbitmq.md) · [Message broker — NATS](./12_message_broker_nats.md)

### Sections

- [Core foundations](#core-foundations)
- [Full-stack cases](#full-stack-cases)

---

## Core foundations

<details>
  <summary>Message broker</summary>

### Message broker

**Definition:** Программный посредник, который маршрутизирует сообщения между producers и consumers — асинхронная развязанная связь между сервисами.

**Why we need it:**

Без брокера каждый сервис вызывает следующий **синхронно** (HTTP, gRPC). Для маленьких приложений это нормально, на масштабе ломается:

| Problem without a broker          | What happens                                                                                 |
| --------------------------------- | -------------------------------------------------------------------------------------------- |
| **Tight coupling**                | Order API знает URL, схемы и таймауты email, inventory и analytics                           |
| **Availability chain**            | Email-сервис упал — checkout падает или зависает, хотя заказ уже сохранён                    |
| **Traffic spikes**                | Black Friday бьёт в inventory 10× — нет буфера; каждый сервис должен выдержать пик мгновенно |
| **Slow work in the request path** | Отправка email, resize картинок или вызов third party блокируют ответ пользователю           |
| **Hard to add consumers**         | Новый сервис «loyalty points» требует правок order service и redeploy                        |
| **No shared history**             | Downstream не может перечитать прошлые события после багфикса или новой фичи                 |

Брокер стоит **между** producer и consumer: producer кладёт сообщение и идёт дальше; consumer обрабатывает, когда готов. Брокер нужен, когда сервисы общаются **асинхронно**, переживают частичные сбои, гасят всплески нагрузки или когда на одно событие должны реагировать **несколько** систем.

**How it works:**

1. Producer публикует сообщение в брокер (topic, queue или subject).
2. Брокер сохраняет или буферизует сообщение по своей модели доставки.
3. Один или несколько consumers подписываются и обрабатывают сообщения в своём темпе.
4. Acknowledgments, retries и dead-letter обрабатывают сбои.

**When to use:** Связь микросервисов, фоновые задачи, event-driven системы, fan-out уведомления, буферизация всплесков трафика.

**Advantages:**

| Advantage                        | What you gain                                                                                            |
| -------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Temporal decoupling**          | Producer и consumer не обязаны работать одновременно — deploy, restart и scale независимо                |
| **Load leveling**                | Queue или log буферизует всплески; workers обрабатывают в безопасном темпе, не убивая БД и API           |
| **Failure isolation**            | Медленный или упавший consumer не роняет producer; сообщения ждут retry                                  |
| **Fan-out without code changes** | Одно событие `OrderPlaced` доходит до inventory, email, analytics и search — подписчиков добавляют позже |
| **Faster user-facing APIs**      | Сразу `202 Accepted`; тяжёлая или нестабильная работа — в фоне                                           |
| **Horizontal scale**             | Больше consumer-инстансов; брокер распределяет работу (queue groups, partitions)                         |
| **Reliable delivery**            | Persistence, ack/nack, dead-letter — сообщения переживают restart и poison pills                         |
| **Event history (log brokers)**  | Kafka / JetStream дают новым сервисам replay прошлых событий — без backfill из production DB             |

**Pluses and minuses:**

| +                                             | −                                             |
| --------------------------------------------- | --------------------------------------------- |
| Развязывает сервисы во времени и пространстве | Дополнительная сложность инфраструктуры       |
| Буферизует всплески трафика                   | Гарантии порядка сообщений зависят от брокера |
| Включает event-driven архитектуру             | Сложнее отлаживать async-потоки               |
| Надёжная доставка с persistence               | Операционные накладные расходы                |

**Thoughts:** Брокер — когда нужна гарантированная доставка, буферизация или несколько consumers. Прямой HTTP — для синхронного request/response. Node job queues (BullMQ) — в [Build — Infra](./10_build_infra.md#kafka-vs-rabbitmq-vs-bullmq).

**Sources:** [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials)

---

</details>

<details>
  <summary>Queue vs event log</summary>

### Queue vs event log

**Definition:** Две модели async messaging — **queue** (consume and delete) vs **append-only log** (consume and remember offset).


| | Queue (RabbitMQ, NATS Core)  | Log (Kafka, NATS JetStream)            |
| | -----------------  |  ----------------------------  |
| | Message lifecycle | Удаляется после ack          | Хранится по retention policy           |
| | Replay            | Плохо — сообщение исчезло    | First-class — rewind offset            |
| | Fan-out           | Exchanges / несколько queues | Consumer groups + partitions           |
| | Typical use       | Task queues, RPC-style work  | Event sourcing, analytics, audit trail |

**When to use:** Задача выполняется один раз → queue. Несколько команд читают одну историю в разном темпе → log.

**Thoughts:** Queue = распределение работы. Log = общая история. Не используйте Kafka как простую email-очередь без нужды в replay или stream processing.

**Sources:** [Kafka intro](https://kafka.apache.org/intro)

---

</details>

<details>
  <summary>Kafka vs RabbitMQ vs NATS</summary>

### Kafka vs RabbitMQ vs NATS

**Definition:** Три популярных брокера — выбор по **модели доставки**, пропускной способности и операционной нагрузке.


| | RabbitMQ                                 | Kafka                                      | NATS                                                              |
| | ----------  |  ----------------------------------------  |  ------------------------------------------  |
| | Model      | Queue + exchange (AMQP)                  | Append-only distributed log                | Лёгкий pub/sub; JetStream добавляет persistence                   |
| | Routing    | Богатый (direct, fanout, topic, headers) | Topic + partition key                      | Subject wildcards (`orders.*`)                                    |
| | Replay     | Плохо (после ack сообщение удалено)      | First-class (consumer offset)              | JetStream streams; Core NATS — fire-and-forget                    |
| | Throughput | Тысячи–десятки k msg/s                   | Сотни k+ msg/s                             | Миллионы msg/s (Core); JetStream жертвует скоростью за durability |
| | Ops        | Erlang cluster, management UI            | Cluster, disk, partition tuning            | Один маленький binary; кластеризация проще                        |
| | Best fit   | Task queues, сложная маршрутизация       | Event streaming, analytics, event sourcing | Microservices mesh, IoT, low-latency fan-out                      |

**When to use:**

| Broker             | Typical use cases                                                                                                               |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| **RabbitMQ**       | Email/worker queues, priority jobs, сложная маршрутизация (topic/direct exchanges), NestJS microservices, RPC с correlation IDs |
| **Kafka**          | Event sourcing, CDC, clickstream analytics, audit logs, несколько команд на одном stream, Flink/Kafka Streams                   |
| **NATS Core**      | Low-latency pub/sub, request-reply RPC, IoT telemetry, service mesh data plane, edge leaf nodes                                 |
| **NATS JetStream** | Лёгкие persistent queues и replay, когда Kafka ops — overkill                                                                   |

**Pluses and minuses:**

| Broker       | Pluses                                                                                    | Minuses                                                                      |
| ------------ | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **RabbitMQ** | Зрелый AMQP routing; ack/nack/DLX; management UI; отлично для task queues                 | Нет replay; нишевые Erlang ops; не для multi-TB log retention                |
| **Kafka**    | Replay, retention, consumer groups, огромный throughput, экосистема Connect/Streams       | Тяжёлый cluster ops; overkill для простых jobs; порядок только per partition |
| **NATS**     | Маленький binary, sub-ms latency, простые subjects, built-in request-reply, edge-friendly | Core без persistence; JetStream добавляет сложность; меньше connectors       |

**Thoughts:** Queue = consume and forget. Log = consume and remember. NATS Core = самый быстрый fan-out; JetStream — когда сообщения должны пережить restart. AWS managed: SQS ≈ queue, MSK/Kinesis ≈ log.

**Sources:** [Kafka intro](https://kafka.apache.org/intro) · [RabbitMQ docs](https://www.rabbitmq.com/docs) · [NATS — What is NATS?](https://docs.nats.io/nats-concepts/what-is-nats)

---

</details>

<details>
  <summary>Full-stack cases</summary>

### Full-stack cases

**Definition:** Сквозные примеры — кто за что отвечает: **frontend**, **backend API**, **message broker** и **workers** — в реальных продуктах.

**Cases:**

#### 1. E-commerce checkout — simple example

Пользователь покупает футболку. После «Оплатить» нужно сохранить заказ, отправить email и уменьшить stock. Email может идти долго — пользователь не должен ждать.

**What each part does:**

| Part                          | Job in one sentence                                             |
| ----------------------------- | --------------------------------------------------------------- |
| **Frontend**                  | Шлёт заказ в API; показывает «Спасибо!», когда API ответил      |
| **Backend API**               | Сохраняет заказ в БД, кладёт задачу в RabbitMQ, быстро отвечает |
| **Message broker (RabbitMQ)** | Держит **список задач** (queue), пока worker свободен           |
| **Email worker**              | Отдельная программа: читает queue и отправляет email            |

Frontend **никогда** не подключается к RabbitMQ. Только API и workers.

**Simple flow:**

```
1. User clicks Pay
2. Frontend → POST /api/orders → Backend API
3. API saves order in database
4. API adds message to RabbitMQ queue: { orderId: 123, email: "user@mail.com" }
5. API responds 201 → Frontend shows "Order placed!"
   ─── user is done ───
6. Email worker (later) reads message from queue → sends email → deletes message from queue
```

**What the broker does in step 4–6:** API **не** шлёт email. Он только кладёт записку в RabbitMQ: «отправь письмо про заказ 123». RabbitMQ **держит** записку. Когда email worker готов — **берёт** её и отправляет письмо. Если worker занят — сообщения ждут в queue; API пользователю уже ответил.

**Without broker vs with broker:**

| Without broker                                    | With RabbitMQ                         |
| ------------------------------------------------- | ------------------------------------- |
| API шлёт email в том же запросе → user ждёт 3–5 с | API отвечает за ~200 ms               |
| Email-сервис упал → checkout с ошибкой            | Заказ сохранён; email попробует позже |

```
Frontend          Backend API              RabbitMQ           Email worker
    │                  │                      │                    │
    │  POST /orders    │                      │                    │
    │─────────────────►│  save to DB          │                    │
    │                  │  put task in queue   │                    │
    │                  │─────────────────────►│  [order 123]       │
    │  201 OK          │                      │                    │
    │◄─────────────────│                      │                    │
    │  "Thank you!"    │                      │  take task         │
    │                  │                      │───────────────────►│ send email
```

**API code (simplified):**

```js
// 1. Save order
const order = await db.orders.create({ ... });

// 2. Add task to broker — NOT send email here
channel.sendToQueue("send-email", Buffer.from(JSON.stringify({
  orderId: order.id,
  email: order.email,
})));

// 3. Answer user immediately
res.json({ ok: true, orderId: order.id });
```

**Email worker (separate process):**

```js
channel.consume("send-email", async (msg) => {
  const { orderId, email } = JSON.parse(msg.content.toString());
  await sendEmail(email, `Your order #${orderId} is confirmed`);
  channel.ack(msg); // tell RabbitMQ: job done
});
```

**Why broker here:** Разделить **«ответить пользователю сейчас»** и **«медленную работу потом»**. RabbitMQ — просто очередь между API и email worker. Больше queues (например stock) — та же идея; подробнее [RabbitMQ](./12_message_broker_rabbitmq.md).

---

#### 2. User signup — Kafka event log + несколько команд

| Layer              | Role                                                                                                                                         | Technology (example)                                |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **Frontend**       | Форма регистрации, success по ответу API — прямого контакта с брокером нет                                                                   | React SPA или mobile app                            |
| **Backend API**    | Создаёт user в **Postgres**, пишет `user.registered` в Kafka (часто **outbox table** + relay)                                                | Node / Java API                                     |
| **Message broker** | Durable log; у каждой команды свой **consumer group** и свой темп чтения                                                                     | [Kafka](./12_message_broker_kafka.md) topic `users` |
| **Consumers**      | **Search** — индекс в Elasticsearch; **CRM** — sync в Salesforce; **Data** — stream в Snowflake; **Anti-fraud** — scoring в реальном времени | Независимо деплоимые сервисы                        |

```
Frontend → POST /api/register → API → Postgres (+ outbox)
                              ↓
                           Kafka topic "users"
                    ┌─────────┼─────────┬──────────┐
                    ↓         ↓         ↓          ↓
                 Search    CRM sync   Warehouse   Fraud
```

**Why broker here:** Одно событие signup — много подписчиков; новая команда (loyalty) добавляет consumer group без правок API.

---

#### 3. Live in-app notifications — NATS pub/sub

| Layer                    | Role                                                                            | Technology (example)                     |
| ------------------------ | ------------------------------------------------------------------------------- | ---------------------------------------- |
| **Frontend**             | WebSocket к **notification gateway**; toast при приходе сообщения               | React, WebSocket client                  |
| **Backend API**          | На «friend request accepted» публикует в NATS subject `notifications.user.{id}` | Node API, `nats` client                  |
| **Message broker**       | Fire-and-forget fan-out на все gateway-инстансы, подписанные на subject         | [NATS](./12_message_broker_nats.md) Core |
| **Notification gateway** | Подписка на `notifications.user.*`, форвард в WebSocket нужному user            | Node/Go сервис, horizontal scale         |

```
API → NATS publish notifications.user.42
           ↓
    Gateway instances (queue group)
           ↓
    WebSocket → Frontend (user 42 online)
```

**Why broker here:** Sub-ms latency; много API и gateway без sticky sessions и ручного Redis pub/sub.

---

#### 4. File upload + image processing — RabbitMQ work queue

| Layer              | Role                                                                                               | Technology (example)                    |
| ------------------ | -------------------------------------------------------------------------------------------------- | --------------------------------------- |
| **Frontend**       | Upload widget, progress; опрашивает `GET /api/files/:id` за `status: ready`                        | React, presigned S3 или multipart в API |
| **Backend API**    | Метаданные в DB, raw в **S3**, в queue `image.resize` кладёт `{ fileId, s3Key }`, возвращает `202` | Node API                                |
| **Message broker** | Одна **work queue** — сообщение обрабатывает ровно один worker                                     | RabbitMQ durable queue                  |
| **Worker**         | Скачивает из S3, resize thumbnails, заливает variants, обновляет DB `status: ready`                | Node worker + Sharp, ack при успехе     |

**Why broker here:** Resize CPU-heavy и медленный; queue гасит всплески upload и retry упавших jobs.

---

#### 5. Admin dashboard + audit trail — Kafka CDC

| Layer                 | Role                                                                                  | Technology (example)              |
| --------------------- | ------------------------------------------------------------------------------------- | --------------------------------- |
| **Frontend**          | Admin UI читает текущие orders через REST/GraphQL — консистентный OLTP view           | Next.js admin                     |
| **Backend API**       | CRUD только в **Postgres** — без analytics в request path                             | NestJS + Postgres                 |
| **Message broker**    | **Debezium** (Kafka Connect) стримит изменения строк в topic `db.orders`              | Kafka                             |
| **Analytics service** | Читает change stream, строит агрегаты, отдаёт `/api/reports` или пишет в read replica | Flink, ksqlDB или custom consumer |

```
Admin UI → API → Postgres
                    ↓ (CDC)
                 Kafka "db.orders"
                    ↓
              Analytics / reports API
```

**Why broker here:** OLTP остаётся простым; отчёты и replay после багов — из immutable change log.

---

**Thoughts:** Frontend почти никогда не ходит в брокер напрямую — только в API (или WebSocket gateway). Выбор брокера по кейсу: **RabbitMQ** — jobs и routing, **Kafka** — общая история событий, **NATS** — low-latency fan-out. Маленькие Node-only стеки могут взять **BullMQ** (Redis) вместо RabbitMQ для кейсов 1 и 4 — [Build — Infra](./10_build_infra.md#kafka-vs-rabbitmq-vs-bullmq).

**Sources:** [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) · [Kafka intro](https://kafka.apache.org/intro) · [NATS — What is NATS?](https://docs.nats.io/nats-concepts/what-is-nats)

---

</details>
