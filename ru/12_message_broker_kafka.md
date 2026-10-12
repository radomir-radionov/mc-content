# Message broker — Kafka

Apache Kafka: event streaming, topics, partitions и consumer groups.

[← Back to hub](./12_message_broker.md)

### Related docs

- [Message broker — Core](./12_message_broker_core.md) · [Message broker — Kafka](./12_message_broker_kafka.md) · [Message broker — RabbitMQ](./12_message_broker_rabbitmq.md) · [Message broker — NATS](./12_message_broker_nats.md)

### Sections

- [Kafka](#kafka)

---

## Kafka

<details>
  <summary>Kafka</summary>

### Kafka

**Definition:** Apache Kafka — распределённая **event streaming platform**: publish/subscribe, durable storage и stream processing в одной системе. События не удаляются после consumption; consumers отслеживают позицию (offset) в log.

**How it works:**

| Concept            | Role                                                                                      |
| ------------------ | ----------------------------------------------------------------------------------------- |
| **Broker**         | Kafka server, хранящий partitions топика                                                  |
| **Topic**          | Именованный поток событий                                                                 |
| **Partition**      | Шард топика на нескольких brokers для параллелизма                                        |
| **Producer**       | Клиент, добавляющий события в partition                                                   |
| **Consumer group** | Набор consumers, совместно читающих топик; каждый partition читает один consumer в группе |
| **Offset**         | Позиция consumer в partition — основа replay                                              |
| **Replication**    | Копии partitions на нескольких brokers для fault tolerance                                |

События с одним **key** (например `userId`) попадают в одну partition — порядок гарантируется **внутри** partition, не по всему топику.

**When to use:**

| Use case                       | Why Kafka fits                                                                    |
| ------------------------------ | --------------------------------------------------------------------------------- |
| **Event sourcing / audit log** | Append-only log хранит историю; consumers перематывают на любой offset            |
| **CDC (Change Data Capture)**  | Debezium + Kafka Connect стримит изменения БД во множество downstream-систем      |
| **Real-time analytics**        | Flink / Kafka Streams агрегируют clickstream, метрики, fraud signals              |
| **Multi-team data platform**   | Каждая команда — свой consumer group на одном topic в своём темпе                 |
| **Microservices integration**  | События `OrderPlaced`, `PaymentCaptured` развязывают сервисы без общей БД         |
| **Log / metrics aggregation**  | Durable buffer между producers (приложения, агенты) и consumers (ELK, warehouses) |

Не берите Kafka для простой task queue (один worker, сообщение удаляется после ack) — используйте [RabbitMQ](./12_message_broker_rabbitmq.md) или BullMQ.

**Advantages:**

| Advantage                          | What you gain                                                                                 |
| ---------------------------------- | --------------------------------------------------------------------------------------------- |
| **Durable event log**              | События хранятся днями или годами — новые сервисы и багфиксы переобрабатывают историю         |
| **Independent consumer speed**     | Analytics читает месяц медленно; fraud — в реальном времени; один topic, разные groups        |
| **Massive throughput**             | Partitioned writes масштабируются горизонтально; один cluster тянет clickstream и IoT на GB/s |
| **Stream processing in one stack** | Kafka Streams / Flink агрегируют без отдельного nightly batch ETL                             |
| **CDC hub**                        | Один Debezium connector раздаёт изменения строк БД в search, cache, warehouse и microservices |
| **Operational visibility**         | Consumer lag, retention и replay — first-class; видно, кто отстаёт                            |
| **Schema evolution**               | Schema Registry задаёт совместимые изменения JSON/Avro у десятков producers                   |

**Pluses and minuses:**

| +                                                                         | −                                                                                   |
| ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Очень высокая пропускная способность — сотни тысяч+ msg/s на cluster      | Тяжёлая эксплуатация: cluster brokers, disk I/O, число partitions, rebalancing      |
| Durable retention — replay для новых consumers или после исправления бага | Не drop-in task queue — сообщения живут до истечения retention                      |
| Consumer groups масштабируют параллельное чтение per topic                | Строгий порядок только **внутри** partition — плохие partition keys ломают fairness |
| Replication + ISR для fault tolerance между дата-центрами                 | Exactly-once требует idempotent producers + transactional consumers                 |
| Экосистема: Connect, Streams, Schema Registry, ksqlDB                     | Больший memory/disk footprint, чем у RabbitMQ или NATS Core                         |
| Индустриальный стандарт для event-driven и data-mesh                      | Кривая обучения: topics, offsets, compaction, мониторинг consumer lag               |

**Examples:**

```js
// kafkajs — producer
import { Kafka } from "kafkajs";

const kafka = new Kafka({ clientId: "orders", brokers: ["localhost:9092"] });
const producer = kafka.producer();
await producer.connect();
await producer.send({
  topic: "orders",
  messages: [{ key: "user-42", value: JSON.stringify({ action: "placed" }) }],
});
```

```js
// Consumer в группе — partitions распределяются между участниками
const consumer = kafka.consumer({ groupId: "inventory-service" });
await consumer.subscribe({ topic: "orders", fromBeginning: false });
await consumer.run({
  eachMessage: async ({ topic, partition, message }) => {
    console.log({ offset: message.offset, value: message.value?.toString() });
  },
});
```

**Thoughts:** Размер partitions — под целевую пропускную способность и параллелизм consumers. Осмысленный partition key, когда порядок важен per entity. Для publish-after-DB-write — **outbox pattern**. Сравнение: [RabbitMQ](./12_message_broker_rabbitmq.md), [NATS](./12_message_broker_nats.md), [Kafka vs RabbitMQ vs NATS](./12_message_broker_core.md#kafka-vs-rabbitmq-vs-nats).

**Sources:** [Kafka — Introduction](https://kafka.apache.org/intro) · [Kafka — Design](https://kafka.apache.org/documentation/#design)

---

</details>
