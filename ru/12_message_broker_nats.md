# Message broker — NATS

NATS: subjects, pub/sub, request-reply и JetStream persistence.

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

**Definition:** Open-source messaging system для cloud-native приложений, IoT и microservices. Приложения подключаются к **NATS server** и обмениваются сообщениями по **subject** без знания сетевых адресов друг друга — **location transparency**.

**How it works:**

| Concept                    | Role                                                       |
| -------------------------- | ---------------------------------------------------------- |
| **Server (`nats-server`)** | Один лёгкий binary, маршрутизирующий сообщения             |
| **Subject**                | Именованный канал (`orders.created`, wildcards `users.>`)  |
| **Publisher**              | Отправляет сообщение на subject                            |
| **Subscriber**             | Слушает subject; queue groups балансируют нагрузку         |
| **Request-reply**          | Синхронный RPC поверх subjects с inbox replies             |
| **JetStream**              | Опциональный persistence — streams, consumers, ack, replay |
| **Accounts**               | Multi-tenant изоляция в одном cluster                      |

**Core NATS vs JetStream:**


| | Core NATS                         | JetStream                                       |
| | --------  |  ---------------------------------  |
| | Delivery | Fire-and-forget pub/sub           | Persistent streams с ack                        |
| | Replay   | Нет                               | Да — stream retention                           |
| | Latency  | Sub-millisecond, миллионы msg/s   | Durability trade-off                            |
| | Use      | Service mesh, telemetry, commands | Work queues с persistence без полного Kafka ops |

**When to use:**

| Use case                          | Core NATS                                        | JetStream                                    |
| --------------------------------- | ------------------------------------------------ | -------------------------------------------- |
| **Service-to-service pub/sub**    | ✓ — subjects + queue groups балансируют нагрузку | Optional ack для at-least-once               |
| **Request-reply RPC**             | ✓ — встроенный `request()` / inbox replies       | —                                            |
| **IoT / telemetry ingest**        | ✓ — миллионы мелких сообщений, low latency       | ✓ — buffer при обрыве cloud link             |
| **Edge + cloud (leaf nodes)**     | ✓ — локальный hub форвардит в central cluster    | ✓ — persist на edge до восстановления uplink |
| **Real-time dashboards / alerts** | ✓ — fire-and-forget fan-out                      | —                                            |
| **Work queues with retry**        | —                                                | ✓ — streams, consumers, ack, redelivery      |
| **Event replay after deploy**     | —                                                | ✓ — rewind consumer на stream                |
| **Multi-tenant SaaS**             | ✓ — accounts изолируют tenants на одном cluster  | ✓ — per-account streams                      |

Включайте JetStream, когда сообщения должны пережить restart брокера или consumers нужен at-least-once. Для multi-TB retained logs и heavy stream analytics — [Kafka](./12_message_broker_kafka.md). Для сложного AMQP routing (headers exchange, DLX chains) — [RabbitMQ](./12_message_broker_rabbitmq.md).

**Advantages:**

| Advantage                       | What you gain                                                                                 |
| ------------------------------- | --------------------------------------------------------------------------------------------- |
| **Minimal ops footprint**       | Один binary ~15 MB — deploy за секунды на laptop, K8s или edge                                |
| **Lowest latency fan-out**      | Core NATS: миллионы msg/s, sub-ms — live dashboards и control signals                         |
| **No service-discovery client** | Publish на `payments.charge`; NATS маршрутизирует — consumers меняют host без правок producer |
| **Built-in request-reply**      | RPC over messaging без ручных correlation IDs и reply queues                                  |
| **Edge + cloud topology**       | Leaf nodes локально; сообщения в central cluster, когда link поднят                           |
| **Queue groups for free**       | Competing workers на одном subject — балансировка без отдельного имени queue на worker        |
| **JetStream on demand**         | Старт с fire-and-forget Core; persistence и replay — только когда выросли требования          |
| **Multi-tenant by design**      | Accounts изолируют команды или SaaS-клиентов на одном shared cluster                          |

**Pluses and minuses:**

| +                                                                  | −                                                                |
| ------------------------------------------------------------------ | ---------------------------------------------------------------- |
| Один маленький binary (~15 MB RAM) — быстрый deploy и upgrade      | Core NATS: нет persistence; offline subscribers теряют сообщения |
| Sub-millisecond latency при миллионах msg/s                        | JetStream — отдельный config layer: streams, consumers, storage  |
| Subject wildcards (`orders.*`, `>`) без exchange topology          | Меньше third-party connectors, чем у Kafka Connect               |
| Queue groups дают competing consumers без лишней инфраструктуры    | Меньше managed offerings, чем AWS SQS / MSK / Amazon MQ          |
| Встроенный request-reply — без boilerplate correlation-ID          | JetStream throughput ниже Kafka для огромных retained logs       |
| Leaf nodes связывают edge sites с central cluster                  | Менее знаком на собеседованиях, чем Kafka/RabbitMQ               |
| Location transparency — клиентам не нужен service-discovery client | Выбор Core vs JetStream путает новичков                          |
| Multi-tenant accounts на одном cluster                             | Не drop-in AMQP замена для существующих RabbitMQ apps            |

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

**Thoughts:** Начните с Core NATS для быстрой развязки; включайте JetStream для at-least-once и replay без Kafka cluster. Сложные AMQP routing rules — [RabbitMQ](./12_message_broker_rabbitmq.md). Большие retained event lakes — [Kafka](./12_message_broker_kafka.md).

**Sources:** [NATS — What is NATS?](https://docs.nats.io/nats-concepts/what-is-nats) · [NATS — Core concepts](https://docs.nats.io/nats-concepts/overview)

---

</details>
