# Message broker — Core

Broker fundamentals, delivery models, and how to pick Kafka, RabbitMQ, or NATS.

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

**Definition:** Software intermediary that routes messages between producers and consumers — enabling asynchronous, decoupled communication across services.

**Why we need it:**

Without a broker, every service calls the next one **synchronously** (HTTP, gRPC). That works for small apps but breaks down at scale:

| Problem without a broker          | What happens                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------ |
| **Tight coupling**                | Order API must know email, inventory, and analytics URLs, schemas, and timeouts            |
| **Availability chain**            | If the email service is down, checkout fails or hangs even though the order was saved      |
| **Traffic spikes**                | Black Friday hits inventory 10× — no buffer; every service must absorb the spike instantly |
| **Slow work in the request path** | Sending email, resizing images, or calling a third party blocks the user response          |
| **Hard to add consumers**         | New “loyalty points” service requires changing the order service and redeploying           |
| **No shared history**             | Downstream cannot re-read past events after a bug fix or new feature                       |

A message broker sits **between** producers and consumers: the producer drops a message and moves on; consumers process when ready. You need one when services must communicate **asynchronously**, survive partial outages, absorb load spikes, or when **more than one** system should react to the same event.

**How it works:**

1. Producer publishes a message to the broker (topic, queue, or subject).
2. Broker stores or buffers the message according to its delivery model.
3. One or more consumers subscribe and process messages at their own pace.
4. Acknowledgments, retries, and dead-letter paths handle failures.

**When to use:** Microservices communication, background job processing, event-driven systems, fan-out notifications, traffic spike buffering.

**Advantages:**

| Advantage                        | What you gain                                                                                     |
| -------------------------------- | ------------------------------------------------------------------------------------------------- |
| **Temporal decoupling**          | Producer and consumer need not run at the same time — deploy, restart, or scale independently     |
| **Load leveling**                | Queue or log buffers bursts; workers drain at a safe rate instead of overwhelming DB or APIs      |
| **Failure isolation**            | A slow or crashed consumer does not take down the producer; messages wait for retry               |
| **Fan-out without code changes** | One `OrderPlaced` event can reach inventory, email, analytics, and search — add subscribers later |
| **Faster user-facing APIs**      | Return `202 Accepted` immediately; heavy or flaky work runs in the background                     |
| **Horizontal scale**             | Add more consumer instances; broker distributes work (queue groups, partitions)                   |
| **Reliable delivery**            | Persistence, ack/nack, dead-letter queues — messages survive restarts and poison-pill handling    |
| **Event history (log brokers)**  | Kafka / JetStream let new services replay past events — no backfill scripts from production DB    |

**Pluses and minuses:**

| +                                    | −                                          |
| ------------------------------------ | ------------------------------------------ |
| Decouples services in time and space | Added infrastructure complexity            |
| Handles traffic spikes (buffering)   | Message ordering guarantees vary by broker |
| Enables event-driven architecture    | Debugging async flows is harder            |
| Reliable delivery with persistence   | Operational overhead                       |

**Thoughts:** Prefer a message broker when you need guaranteed delivery, buffering, or multiple consumers. Use direct HTTP for synchronous request/response with simple coupling. Node job queues (BullMQ) live in [Build — Infra](./10_build_infra.md#kafka-vs-rabbitmq-vs-bullmq).

**Sources:** [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials)

---

</details>

<details>
  <summary>Queue vs event log</summary>

### Queue vs event log

**Definition:** Two mental models for async messaging — **queue** (consume and delete) vs **append-only log** (consume and remember offset).

|                   | Queue (RabbitMQ, NATS Core) | Log (Kafka, NATS JetStream)            |
| ----------------- | --------------------------- | -------------------------------------- |
| Message lifecycle | Removed after ack           | Retained by retention policy           |
| Replay            | Poor — message is gone      | First-class — rewind offset            |
| Fan-out           | Exchanges / multiple queues | Consumer groups + partitions           |
| Typical use       | Task queues, RPC-style work | Event sourcing, analytics, audit trail |

**When to use:** Task that runs once → queue. Many teams consume the same history at different speeds → log.

**Thoughts:** Queue = work distribution. Log = shared history. Do not use Kafka as a simple email queue unless you need replay or stream processing.

**Sources:** [Kafka intro](https://kafka.apache.org/intro)

---

</details>

<details>
  <summary>Kafka vs RabbitMQ vs NATS</summary>

### Kafka vs RabbitMQ vs NATS

**Definition:** Three popular brokers — pick by **delivery model**, throughput needs, and ops footprint.

|            | RabbitMQ                              | Kafka                                      | NATS                                                         |
| ---------- | ------------------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| Model      | Queue + exchange (AMQP)               | Append-only distributed log                | Lightweight pub/sub; JetStream adds persistence              |
| Routing    | Rich (direct, fanout, topic, headers) | Topic + partition key                      | Subject wildcards (`orders.*`)                               |
| Replay     | Poor (message gone after ack)         | First-class (consumer offset)              | JetStream streams; Core NATS is fire-and-forget              |
| Throughput | Thousands–tens of k msg/s             | Hundreds of k+ msg/s                       | Millions msg/s (Core); JetStream trades speed for durability |
| Ops        | Erlang cluster, management UI         | Cluster, disk, partition tuning            | Single small binary; clustering is simpler                   |
| Best fit   | Task queues, complex routing          | Event streaming, analytics, event sourcing | Microservices mesh, IoT, low-latency fan-out                 |

**When to use:**

| Broker             | Typical use cases                                                                                                            |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| **RabbitMQ**       | Email/worker queues, priority jobs, complex routing (topic/direct exchanges), NestJS microservices, RPC with correlation IDs |
| **Kafka**          | Event sourcing, CDC, clickstream analytics, audit logs, multiple teams reading the same stream, Flink/Kafka Streams          |
| **NATS Core**      | Low-latency pub/sub, request-reply RPC, IoT telemetry, service mesh data plane, edge leaf nodes                              |
| **NATS JetStream** | Lightweight persistent queues and replay when Kafka ops are overkill                                                         |

**Pluses and minuses:**

| Broker       | Pluses                                                                              | Minuses                                                                         |
| ------------ | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **RabbitMQ** | Mature AMQP routing; ack/nack/DLX; management UI; great for task queues             | No replay; Erlang ops niche; not for multi-TB log retention                     |
| **Kafka**    | Replay, retention, consumer groups, huge throughput, Connect/Streams ecosystem      | Heavy cluster ops; overkill for simple jobs; ordering per partition only        |
| **NATS**     | Tiny binary, sub-ms latency, simple subjects, built-in request-reply, edge-friendly | Core has no persistence; JetStream adds complexity; smaller connector ecosystem |

**Thoughts:** Queue = consume and forget. Log = consume and remember. NATS Core = fastest fan-out; add JetStream when messages must survive restarts. AWS managed: SQS ≈ queue, MSK/Kinesis ≈ log.

**Sources:** [Kafka intro](https://kafka.apache.org/intro) · [RabbitMQ docs](https://www.rabbitmq.com/docs) · [NATS — What is NATS?](https://docs.nats.io/nats-concepts/what-is-nats)

---

</details>

<details>
  <summary>Full-stack cases</summary>

### Full-stack cases

**Definition:** End-to-end examples of who does what — **frontend**, **backend API**, **message broker**, and **workers** — in real products.

**Cases:**

#### 1. E-commerce checkout — simple example

User buys a T-shirt. After “Pay” the shop must save the order, send email, and reduce stock. Email can be slow — the user should not wait for it.

**What each part does:**

| Part                          | Job in one sentence                                              |
| ----------------------------- | ---------------------------------------------------------------- |
| **Frontend**                  | Sends the order to the API; shows “Thank you!” when API answers  |
| **Backend API**               | Saves order to DB, puts a small task into RabbitMQ, returns fast |
| **Message broker (RabbitMQ)** | Keeps a **to-do list** (queue) until a worker is free            |
| **Email worker**              | Separate small program that reads the queue and sends the email  |

The frontend **never** connects to RabbitMQ. Only the API and workers do.

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

**What the broker does in step 4–6:** The API does **not** send email. It only drops a note into RabbitMQ: “please email user about order 123”. RabbitMQ **holds** that note. When the email worker is ready, it **takes** the note and sends the mail. If the worker is busy, messages wait in the queue — the API already responded to the user.

**Without broker vs with broker:**

| Without broker                                             | With RabbitMQ                        |
| ---------------------------------------------------------- | ------------------------------------ |
| API sends email inside the same request → user waits 3–5 s | API returns in ~200 ms               |
| Email service down → checkout shows error                  | Order saved; email tries again later |

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

**Why broker here:** Split **“answer the user now”** from **“do slow work later”**. RabbitMQ is just a waiting line between API and email worker. More queues (e.g. stock) work the same way — see [RabbitMQ](./12_message_broker_rabbitmq.md).

---

#### 2. User signup — Kafka event log + multiple teams

| Layer              | Role                                                                                                                                           | Technology (example)                                |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **Frontend**       | Registration form, shows success when API returns — no direct broker contact                                                                   | React SPA or mobile app                             |
| **Backend API**    | Creates user in **Postgres**, produces `user.registered` event to Kafka (often via **outbox table** + relay)                                   | Node / Java API                                     |
| **Message broker** | Durable log; each downstream team has its own **consumer group** reading at its own speed                                                      | [Kafka](./12_message_broker_kafka.md) topic `users` |
| **Consumers**      | **Search** — index profile in Elasticsearch; **CRM** — sync to Salesforce; **Data** — stream to Snowflake; **Anti-fraud** — score in real time | Independent deployable services                     |

```
Frontend → POST /api/register → API → Postgres (+ outbox)
                              ↓
                           Kafka topic "users"
                    ┌─────────┼─────────┬──────────┐
                    ↓         ↓         ↓          ↓
                 Search    CRM sync   Warehouse   Fraud
```

**Why broker here:** One signup event, many subscribers; new team (e.g. loyalty) adds a consumer group without changing the API.

---

#### 3. Live in-app notifications — NATS pub/sub

| Layer                    | Role                                                                                   | Technology (example)                     |
| ------------------------ | -------------------------------------------------------------------------------------- | ---------------------------------------- |
| **Frontend**             | Open WebSocket to **notification gateway**; renders toast when message arrives         | React, WebSocket client                  |
| **Backend API**          | On “friend request accepted”, publishes to NATS subject `notifications.user.{id}`      | Node API, `nats` client                  |
| **Message broker**       | Fire-and-forget fan-out to all gateway instances subscribed to that subject            | [NATS](./12_message_broker_nats.md) Core |
| **Notification gateway** | Subscribes to `notifications.user.*`, forwards matching events to connected WebSockets | Small Node/Go service, horizontal scale  |

```
API → NATS publish notifications.user.42
           ↓
    Gateway instances (queue group)
           ↓
    WebSocket → Frontend (user 42 online)
```

**Why broker here:** Sub-ms latency, many API instances and gateway instances without sticky sessions or Redis pub/sub wiring.

---

#### 4. File upload + image processing — RabbitMQ work queue

| Layer              | Role                                                                                                                        | Technology (example)                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **Frontend**       | Upload widget (drag-drop), progress bar, polls `GET /api/files/:id` for `status: ready`                                     | React, presigned S3 upload or multipart to API |
| **Backend API**    | Stores file metadata in DB, uploads raw file to **S3**, enqueues `{ fileId, s3Key }` to `image.resize` queue, returns `202` | Node API                                       |
| **Message broker** | Single **work queue** — one message processed by exactly one worker                                                         | RabbitMQ durable queue                         |
| **Worker**         | Downloads from S3, resizes thumbnails, uploads variants, updates DB `status: ready`                                         | Node worker with Sharp, ack on success         |

**Why broker here:** Resize is CPU-heavy and slow; queue absorbs upload spikes and retries failed jobs.

---

#### 5. Admin dashboard + audit trail — Kafka CDC

| Layer                 | Role                                                                                       | Technology (example)              |
| --------------------- | ------------------------------------------------------------------------------------------ | --------------------------------- |
| **Frontend**          | Admin UI reads current orders via REST/GraphQL — always consistent OLTP view               | Next.js admin                     |
| **Backend API**       | CRUD on **Postgres** only — no analytics logic in request path                             | NestJS + Postgres                 |
| **Message broker**    | **Debezium** (Kafka Connect) streams row changes to Kafka topic `db.orders`                | Kafka                             |
| **Analytics service** | Consumes change stream, builds aggregates, serves `/api/reports` or writes to read replica | Flink, ksqlDB, or custom consumer |

```
Admin UI → API → Postgres
                    ↓ (CDC)
                 Kafka "db.orders"
                    ↓
              Analytics / reports API
```

**Why broker here:** OLTP stays simple; reporting and replay after bugs use the immutable change log.

---

**Thoughts:** Frontend almost never talks to the broker directly — it calls your API (or WebSocket gateway). Pick the broker by case: **RabbitMQ** for jobs and routing, **Kafka** for shared event history, **NATS** for low-latency fan-out. Small Node-only stacks can use **BullMQ** (Redis) instead of RabbitMQ for case 1 and 4 — see [Build — Infra](./10_build_infra.md#kafka-vs-rabbitmq-vs-bullmq).

**Sources:** [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) · [Kafka intro](https://kafka.apache.org/intro) · [NATS — What is NATS?](https://docs.nats.io/nats-concepts/what-is-nats)

---

</details>
