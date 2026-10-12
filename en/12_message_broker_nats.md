# Message broker — NATS

NATS: subjects, pub/sub, request-reply, and JetStream persistence.

[← Back to hub](./12_message_broker.md)

### Related docs

- [Message broker — Core](./12_message_broker_core.md) · [Message broker — Kafka](./12_message_broker_kafka.md) · [Message broker — RabbitMQ](./12_message_broker_rabbitmq.md) · [Message broker — NATS](./12_message_broker_nats.md)

### Sections

- [NATS](#nats)

---

## NATS

<details>
  <summary>NATS</summary>

### NATS

**Definition:** Open-source messaging system for cloud-native apps, IoT, and microservices. Applications connect to a **NATS server** and exchange messages by **subject** without knowing each other's network addresses — **location transparency**.

**How it works:**

| Concept                    | Role                                                                              |
| -------------------------- | --------------------------------------------------------------------------------- |
| **Server (`nats-server`)** | Single lightweight binary that routes messages                                    |
| **Subject**                | Named channel (`orders.created`, `users.>` wildcards)                             |
| **Publisher**              | Sends a message to a subject                                                      |
| **Subscriber**             | Listens on a subject; queue groups load-balance among subscribers                 |
| **Request-reply**          | Synchronous RPC pattern over subjects with inbox replies                          |
| **JetStream**              | Optional persistence layer — streams, consumers, ack, replay (like a lighter log) |
| **Accounts**               | Multi-tenant isolation within one cluster                                         |

**Core NATS vs JetStream:**

|          | Core NATS                         | JetStream                                              |
| -------- | --------------------------------- | ------------------------------------------------------ |
| Delivery | Fire-and-forget pub/sub           | Persistent streams with ack                            |
| Replay   | No                                | Yes — stream retention                                 |
| Latency  | Sub-millisecond, millions msg/s   | Durability trade-off                                   |
| Use      | Service mesh, telemetry, commands | Work queues needing persistence without full Kafka ops |

**When to use:**

| Use case                          | Core NATS                                   | JetStream                                |
| --------------------------------- | ------------------------------------------- | ---------------------------------------- |
| **Service-to-service pub/sub**    | ✓ — subjects + queue groups load-balance    | Optional ack for at-least-once           |
| **Request-reply RPC**             | ✓ — built-in `request()` / inbox replies    | —                                        |
| **IoT / telemetry ingest**        | ✓ — millions of small messages, low latency | ✓ — buffer when cloud link is down       |
| **Edge + cloud (leaf nodes)**     | ✓ — local hub forwards to central cluster   | ✓ — persist at edge until uplink returns |
| **Real-time dashboards / alerts** | ✓ — fire-and-forget fan-out                 | —                                        |
| **Work queues with retry**        | —                                           | ✓ — streams, consumers, ack, redelivery  |
| **Event replay after deploy**     | —                                           | ✓ — rewind consumer on stream            |
| **Multi-tenant SaaS**             | ✓ — accounts isolate tenants on one cluster | ✓ — per-account streams                  |

Add JetStream when messages must survive broker restarts or consumers need at-least-once delivery. Prefer [Kafka](./12_message_broker_kafka.md) for multi-TB retained logs and heavy stream analytics. Prefer [RabbitMQ](./12_message_broker_rabbitmq.md) for complex AMQP routing (headers exchange, DLX chains).

**Advantages:**

| Advantage                       | What you gain                                                                                 |
| ------------------------------- | --------------------------------------------------------------------------------------------- |
| **Minimal ops footprint**       | Single ~15 MB binary — deploy in seconds on laptop, K8s, or edge device                       |
| **Lowest latency fan-out**      | Core NATS: millions of msg/s, sub-ms — ideal for live dashboards and control signals          |
| **No service-discovery client** | Publish to `payments.charge`; NATS routes — consumers move hosts without producer config      |
| **Built-in request-reply**      | RPC over messaging without hand-rolling correlation IDs and reply queues                      |
| **Edge + cloud topology**       | Leaf nodes run locally; messages forward to central cluster when the link is up               |
| **Queue groups for free**       | Competing workers on the same subject — load balance without a separate queue name per worker |
| **JetStream on demand**         | Start with fire-and-forget Core; add persistence and replay only when requirements grow       |
| **Multi-tenant by design**      | Accounts isolate teams or SaaS customers on one shared cluster                                |

**Pluses and minuses:**

| +                                                                | −                                                                  |
| ---------------------------------------------------------------- | ------------------------------------------------------------------ |
| Single small binary (~15 MB RAM) — fast to deploy and upgrade    | Core NATS: no persistence; offline subscribers miss messages       |
| Sub-millisecond latency at millions of msg/s                     | JetStream is a separate config layer — streams, consumers, storage |
| Subject wildcards (`orders.*`, `>`) without exchange topology    | Smaller third-party connector ecosystem than Kafka Connect         |
| Queue groups give competing consumers without extra infra        | Fewer managed offerings than AWS SQS / MSK / Amazon MQ             |
| Built-in request-reply — no correlation-ID boilerplate           | JetStream throughput lower than Kafka for huge retained logs       |
| Leaf nodes bridge edge sites to central cluster                  | Less familiar in enterprise interviews than Kafka/RabbitMQ         |
| Location transparency — clients need no service-discovery client | Core vs JetStream choice confuses newcomers                        |
| Multi-tenant accounts on one cluster                             | Not a drop-in AMQP replacement for existing RabbitMQ apps          |

**Examples:**

```js
import { connect, StringCodec } from "nats";

const nc = await connect({ servers: "nats://localhost:4222" });
const sc = StringCodec();

// Publish
nc.publish("orders.created", sc.encode(JSON.stringify({ id: 1 })));

// Subscribe
const sub = nc.subscribe("orders.created");
for await (const msg of sub) {
  console.log(sc.decode(msg.data));
}

// Request-reply
const reply = await nc.request("users.get", sc.encode("42"), { timeout: 1000 });
console.log(sc.decode(reply.data));
```

**Thoughts:** Start with Core NATS for fast decoupling; enable JetStream when you need at-least-once delivery and replay without operating a Kafka cluster. For complex AMQP routing rules, prefer [RabbitMQ](./12_message_broker_rabbitmq.md). For huge retained event lakes, prefer [Kafka](./12_message_broker_kafka.md).

**Sources:** [NATS — What is NATS?](https://docs.nats.io/nats-concepts/what-is-nats) · [NATS — Core concepts](https://docs.nats.io/nats-concepts/overview)

---

</details>
