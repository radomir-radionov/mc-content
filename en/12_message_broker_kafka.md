# Message broker — Kafka

Apache Kafka: event streaming, topics, partitions, and consumer groups.

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

**Definition:** Apache Kafka is a distributed **event streaming platform** — publish/subscribe, durable storage, and stream processing in one system. Events are not deleted after consumption; consumers track their position (offset) in the log.

**How it works:**

| Concept            | Role                                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------------------- |
| **Broker**         | Kafka server that stores topic partitions                                                             |
| **Topic**          | Named stream of events (like a folder of ordered files)                                               |
| **Partition**      | Shard of a topic spread across brokers for parallelism                                                |
| **Producer**       | Client that appends events to a topic partition                                                       |
| **Consumer group** | Set of consumers that cooperatively read a topic; each partition is read by one consumer in the group |
| **Offset**         | Position of a consumer in a partition — enables replay                                                |
| **Replication**    | Copies of partitions on multiple brokers for fault tolerance                                          |

Events with the same **key** (e.g. `userId`) land in the same partition — Kafka preserves order **within** a partition, not across the whole topic.

**When to use:**

| Use case                       | Why Kafka fits                                                                  |
| ------------------------------ | ------------------------------------------------------------------------------- |
| **Event sourcing / audit log** | Append-only log retains history; consumers rewind to any offset                 |
| **CDC (Change Data Capture)**  | Debezium + Kafka Connect streams DB changes to many downstream systems          |
| **Real-time analytics**        | Flink / Kafka Streams aggregate high-volume clickstream, metrics, fraud signals |
| **Multi-team data platform**   | Each team runs its own consumer group on the same topic at its own pace         |
| **Microservices integration**  | `OrderPlaced`, `PaymentCaptured` events decouple services without shared DB     |
| **Log / metrics aggregation**  | Durable buffer between producers (apps, agents) and consumers (ELK, warehouses) |

Skip Kafka when you need a simple task queue (one worker, message deleted after ack) — use [RabbitMQ](./12_message_broker_rabbitmq.md) or BullMQ instead.

**Advantages:**

| Advantage                          | What you gain                                                                                        |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **Durable event log**              | Events stay for days or years — new services and bug fixes can reprocess history                     |
| **Independent consumer speed**     | Analytics team reads last month slowly; fraud service reads real-time — same topic, different groups |
| **Massive throughput**             | Partitioned writes scale horizontally; one cluster handles clickstream and IoT at GB/s               |
| **Stream processing in one stack** | Kafka Streams / Flink consume and aggregate without a separate batch ETL nightly job                 |
| **CDC hub**                        | One Debezium connector fans DB row changes to search, cache, warehouse, and microservices            |
| **Operational visibility**         | Consumer lag, retention, and replay are first-class — easy to see who is behind                      |
| **Schema evolution**               | Schema Registry enforces compatible JSON/Avro changes across dozens of producers                     |

**Pluses and minuses:**

| +                                                                         | −                                                                              |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Very high throughput — hundreds of thousands+ msg/s per cluster           | Operationally heavy: broker cluster, disk I/O, partition count, rebalancing    |
| Durable retention — replay any time window for new consumers or bug fixes | Not a drop-in task queue — messages stay until retention expires               |
| Consumer groups scale read parallelism per topic                          | Strict ordering only **within** a partition — bad partition keys hurt fairness |
| Replication + ISR for fault tolerance across datacenters                  | Exactly-once needs idempotent producers + transactional consumers              |
| Rich ecosystem: Connect, Streams, Schema Registry, ksqlDB                 | Larger memory/disk footprint than RabbitMQ or NATS Core                        |
| Industry standard for event-driven and data-mesh architectures            | Learning curve: topics, offsets, compaction, consumer lag monitoring           |

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
// Consumer in a group — partitions split across group members
const consumer = kafka.consumer({ groupId: "inventory-service" });
await consumer.subscribe({ topic: "orders", fromBeginning: false });
await consumer.run({
  eachMessage: async ({ topic, partition, message }) => {
    console.log({ offset: message.offset, value: message.value?.toString() });
  },
});
```

**Thoughts:** Size partitions for target throughput and consumer parallelism. Use a meaningful partition key when order matters per entity. For publish-after-DB-write reliability, pair with the **outbox pattern**. Compare with [RabbitMQ](./12_message_broker_rabbitmq.md) and [NATS](./12_message_broker_nats.md) in [Kafka vs RabbitMQ vs NATS](./12_message_broker_core.md#kafka-vs-rabbitmq-vs-nats).

**Sources:** [Kafka — Introduction](https://kafka.apache.org/intro) · [Kafka — Design](https://kafka.apache.org/documentation/#design)

---

</details>
