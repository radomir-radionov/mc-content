# Message broker — RabbitMQ

RabbitMQ: AMQP exchanges, queues, bindings, and reliable task processing.

[← Back to hub](./12_message_broker.md)

### Related docs

- [Message broker — Core](./12_message_broker_core.md) · [Message broker — Kafka](./12_message_broker_kafka.md) · [Message broker — RabbitMQ](./12_message_broker_rabbitmq.md) · [Message broker — NATS](./12_message_broker_nats.md)

### Sections

- [RabbitMQ](#rabbitmq)

---

## RabbitMQ

<details>
  <summary>RabbitMQ</summary>

### RabbitMQ

**Definition:** Open-source message broker implementing **AMQP**. Routes messages from producers to consumers via exchanges and queues — a classic **queue** model where messages are removed after acknowledgment.

**How it works:**

| Concept        | Role                                                                |
| -------------- | ------------------------------------------------------------------- |
| **Producer**   | Sends messages to an exchange                                       |
| **Exchange**   | Routes messages to queues by rules (direct, fanout, topic, headers) |
| **Queue**      | Buffer holding messages until consumed                              |
| **Binding**    | Link between exchange and queue with routing key                    |
| **Consumer**   | Subscribes to a queue and processes messages                        |
| **Ack / nack** | Consumer confirms success or rejects for retry / dead-letter        |

**Exchange types:**

| Type        | Routes when                              |
| ----------- | ---------------------------------------- |
| **direct**  | Routing key matches binding key exactly  |
| **fanout**  | Broadcast to all bound queues            |
| **topic**   | Routing key matches pattern (`orders.*`) |
| **headers** | Match on message header attributes       |

**When to use:**

| Use case                     | Why RabbitMQ fits                                                                  |
| ---------------------------- | ---------------------------------------------------------------------------------- |
| **Background job queue**     | Durable queues + ack/nack — email, PDF generation, image resize                    |
| **Work distribution**        | Multiple workers compete on one queue; broker delivers round-robin                 |
| **Priority tasks**           | Priority queues let urgent jobs jump ahead of bulk work                            |
| **Delayed / scheduled jobs** | Delayed-message plugin or TTL + dead-letter for “run in 30 min”                    |
| **Fan-out notifications**    | `fanout` exchange pushes one event to email, SMS, and push queues                  |
| **Topic-based routing**      | `topic` exchange routes `orders.created` vs `orders.shipped` to different services |
| **RPC over messaging**       | Request–reply pattern with correlation IDs (tutorial #6)                           |
| **NestJS / microservices**   | First-class transport in `@nestjs/microservices`                                   |

Skip RabbitMQ when you need event replay, very high sustained throughput, or a retained event lake — use [Kafka](./12_message_broker_kafka.md). For sub-ms fire-and-forget fan-out without AMQP routing, consider [NATS](./12_message_broker_nats.md).

**Advantages:**

| Advantage                          | What you gain                                                                                              |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Guaranteed task delivery**       | Durable queues + persistent messages + ack — work is not lost if a worker crashes mid-job                  |
| **Flexible routing in the broker** | Exchanges route by key, pattern, or headers — producer sends to one exchange, routing rules live in config |
| **Work stealing**                  | Multiple workers on one queue — broker delivers each message to exactly one idle consumer                  |
| **Poison-message safety**          | Dead-letter exchange quarantines bad messages so the queue keeps moving                                    |
| **Priority and delay**             | Urgent jobs and scheduled “run later” tasks without a separate scheduler service                           |
| **Low integration cost**           | Mature clients for every major language; NestJS, Spring, and Celery integrations out of the box            |
| **Observable queues**              | Management UI shows queue depth, rates, and stuck consumers — ops-friendly for job backlogs                |

**Pluses and minuses:**

| +                                                                   | −                                                                      |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Mature AMQP semantics — ack, nack, reject, prefetch                 | Erlang runtime — smaller pool of ops expertise than Java/Go stacks     |
| Four exchange types cover most routing patterns without custom code | Lower peak throughput than Kafka for multi-GB/s log ingestion          |
| Dead-letter exchanges (DLX) isolate poison messages                 | Message deleted after ack — no built-in replay                         |
| Management UI, metrics, tracing plugins                             | Mirrored / quorum queues add clustering complexity                     |
| Per-message TTL, persistence, and priority                          | Heavier than Redis pub/sub or NATS Core for trivial broadcast          |
| Wide client library support (Node, Java, Python, .NET)              | Competing consumers on one queue — only one consumer gets each message |
| Battle-tested in enterprise task-queue workloads                    | Not ideal as long-term analytics store or event-sourcing backbone      |

**Examples:**

```js
import amqp from "amqplib";

const conn = await amqp.connect(process.env.RABBITMQ_URL);
const channel = await conn.createChannel();
await channel.assertQueue("tasks", { durable: true });

// Producer
channel.sendToQueue(
  "tasks",
  Buffer.from(JSON.stringify({ job: "send-email" })),
  {
    persistent: true,
  },
);

// Consumer
channel.consume("tasks", (msg) => {
  const data = JSON.parse(msg.content.toString());
  console.log("Processing:", data);
  channel.ack(msg);
});
```

**Thoughts:** Use `durable: true` queues and `persistent: true` messages for reliability. **Dead-letter exchanges (DLX)** handle poison messages. NestJS microservices transport: `@nestjs/microservices` with RabbitMQ. Compare with [Kafka](./12_message_broker_kafka.md) when you need replay or very high throughput.

**Sources:** [RabbitMQ documentation](https://www.rabbitmq.com/docs) · [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials)

---

</details>
