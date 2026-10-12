# Message broker — RabbitMQ

RabbitMQ: AMQP exchanges, queues, bindings и надёжная обработка задач.

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

**Definition:** Open-source message broker с **AMQP**. Маршрутизирует сообщения от producers к consumers через exchanges и queues — классическая **queue**-модель: сообщение удаляется после acknowledgment.

**How it works:**

| Concept        | Role                                                                 |
| -------------- | -------------------------------------------------------------------- |
| **Producer**   | Отправляет сообщения в exchange                                      |
| **Exchange**   | Маршрутизирует в queues по правилам (direct, fanout, topic, headers) |
| **Queue**      | Буфер до consumption                                                 |
| **Binding**    | Связь exchange ↔ queue с routing key                                 |
| **Consumer**   | Подписывается на queue и обрабатывает сообщения                      |
| **Ack / nack** | Подтверждение успеха или отказ для retry / dead-letter               |

**Exchange types:**

| Type        | Routes when                                    |
| ----------- | ---------------------------------------------- |
| **direct**  | Routing key совпадает с binding key            |
| **fanout**  | Broadcast во все bound queues                  |
| **topic**   | Routing key совпадает с паттерном (`orders.*`) |
| **headers** | Match по заголовкам сообщения                  |

**When to use:**

| Use case                     | Why RabbitMQ fits                                                                     |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| **Background job queue**     | Durable queues + ack/nack — email, PDF, resize изображений                            |
| **Work distribution**        | Несколько workers на одной queue; broker раздаёт round-robin                          |
| **Priority tasks**           | Priority queues — срочные задачи впереди bulk-работы                                  |
| **Delayed / scheduled jobs** | Delayed-message plugin или TTL + dead-letter для «запустить через 30 мин»             |
| **Fan-out notifications**    | `fanout` exchange шлёт одно событие в email, SMS и push queues                        |
| **Topic-based routing**      | `topic` exchange маршрутизирует `orders.created` vs `orders.shipped` в разные сервисы |
| **RPC over messaging**       | Request–reply с correlation IDs (tutorial #6)                                         |
| **NestJS / microservices**   | First-class transport в `@nestjs/microservices`                                       |

Не берите RabbitMQ, если нужен event replay, очень высокий sustained throughput или retained event lake — [Kafka](./12_message_broker_kafka.md). Для sub-ms fire-and-forget без AMQP routing — [NATS](./12_message_broker_nats.md).

**Advantages:**

| Advantage                          | What you gain                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Guaranteed task delivery**       | Durable queues + persistent messages + ack — работа не теряется при падении worker mid-job             |
| **Flexible routing in the broker** | Exchanges маршрутизируют по key, pattern или headers — producer шлёт в один exchange, правила в config |
| **Work stealing**                  | Несколько workers на одной queue — брокер отдаёт каждое сообщение ровно одному свободному consumer     |
| **Poison-message safety**          | Dead-letter exchange изолирует плохие сообщения — очередь не встаёт                                    |
| **Priority and delay**             | Срочные задачи и отложенный «запустить позже» без отдельного scheduler-сервиса                         |
| **Low integration cost**           | Зрелые клиенты для всех языков; NestJS, Spring и Celery из коробки                                     |
| **Observable queues**              | Management UI показывает глубину очереди, rates и зависших consumers — удобно для job backlog          |

**Pluses and minuses:**

| +                                                            | −                                                                            |
| ------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| Зрелая AMQP-семантика — ack, nack, reject, prefetch          | Erlang runtime — меньше ops-экспертизы, чем Java/Go стеки                    |
| Четыре типа exchange покрывают большинство routing-паттернов | Ниже peak throughput, чем у Kafka для multi-GB/s log ingestion               |
| Dead-letter exchanges (DLX) изолируют poison messages        | Сообщение удаляется после ack — нет built-in replay                          |
| Management UI, metrics, tracing plugins                      | Mirrored / quorum queues усложняют кластеризацию                             |
| Per-message TTL, persistence и priority                      | Тяжелее Redis pub/sub или NATS Core для trivial broadcast                    |
| Широкая поддержка клиентов (Node, Java, Python, .NET)        | Competing consumers на одной queue — каждое сообщение только одному consumer |
| Проверен в enterprise task-queue workloads                   | Не подходит как long-term analytics store или event-sourcing backbone        |

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

**Thoughts:** `durable: true` queues и `persistent: true` messages — для надёжности. **Dead-letter exchanges (DLX)** — poison messages. NestJS: `@nestjs/microservices` с RabbitMQ. Сравнение с [Kafka](./12_message_broker_kafka.md) — когда нужен replay или очень высокий throughput.

**Sources:** [RabbitMQ documentation](https://www.rabbitmq.com/docs) · [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials)

---

</details>
