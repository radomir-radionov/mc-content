# Build — Infra

Архитектура доставки, Nginx, брокеры и инструменты DX.

[ ← Вернуться в хаб](./10_build.md)

### Related docs

- [Сборка — CI](./10_build_ci.md) · [Сборка — Bundlers](./10_build_bundlers.md) · [Сборка — Docker](./10_build_docker.md) · [Сборка — Infra](./10_build_infra.md) · [Сборка — Интервью](./10_build_interview.md)

### Sections

- [Architecture](#architecture)
- [Infrastructure](#infrastructure)
- [Developer experience](#developer-experience)

---

## Architecture

<details>
  <summary>Monolith vs microservices</summary>

### Monolith vs microservices

**Definition:**


| | Монолит                         | Микросервисы                          |
| | -------------------  |  -------------------------------  |
| | **Структура**       | Одиночный развертываемый блок   | Независимые услуги для каждого домена |
| | **Развертывание**   | Все или ничего                  | Развертывание для каждой службы       |
| | **Масштабирование** | Масштабировать все приложение   | Масштаб индивидуальных услуг          |
| | **Данные**          | Общая база данных (обычно)      | База данных на сервис                 |
| | **Команда**         | Одна команда, одна кодовая база | Команды на сервис                     |

**When to use:**

| Выбирайте монолит                         | Выберите микросервисы                                     |
| ----------------------------------------- | --------------------------------------------------------- |
| Стартап/MVP                               | Большая организация, много команд                         |
| Небольшая команда (< 10 разработчиков)    | Очистить границы домена                                   |
| Неясные границы домена                    | Различные потребности в масштабировании для каждой услуги |
| Скорость итерации имеет решающее значение | Существует зрелая команда DevOps/платформы                |

**Pluses and minuses:**

| +                                                   | −                                                           |
| --------------------------------------------------- | ----------------------------------------------------------- |
| Простота разработки и первоначального развертывания | Вырастает сложным; плотная связь                            |
| Простая локальная отладка                           | Единая точка отказа                                         |
| Нет задержки в сети между модулями                  | Масштабирование осуществляется по принципу «все или ничего» |
| Транзакции между модулями просты                    | Риск развертывания увеличивается с размером                 |

| +                                           | −                                                |
|  -------------------------------------------  |  ------------------------------------------------  |
| Независимое развертывание и масштабирование | Сложность распределенной системы |
| Разнообразие технологий в каждой услуге | Задержка в сети, частичные сбои |
| Автономия команды                           | Проблемы согласованности данных (саги, события)  |
| Изоляция неисправностей                     | Операционные издержки (наблюдаемость, CI/CD × N) |

**Thoughts:**

Запускайте монолит, извлекайте сервисы, когда границы ясны и боль реальна, а не упреждающе. «Микросервисы» без наблюдаемости (трассировки, журналирования, метрик) — это кошмар распределенного монолита.

---

**Sources:** [ByteByteGo — Микросервисы](https://blog.bytebytego.com/p/a-beginners-guide-to-microservices)

</details>

<a name="infrastructure"></a>

## Infrastructure

<details>
  <summary>Nginx</summary>

### Nginx

**Definition:** Высокопроизводительный веб-сервер, обратный прокси-сервер, балансировщик нагрузки и HTTP-кеш.

**When to use:** Обслуживание статических сборок SPA, обратный прокси-сервер для API Node/Go, завершение TLS, балансировка нагрузки между экземплярами приложений.

**Pluses and minuses:**

| +                                                   | −                                                       |
| --------------------------------------------------- | ------------------------------------------------------- |
| Чрезвычайно быстрое обслуживание статических файлов | Синтаксис конфигурации уникален                         |
| Обратный прокси-сервер для вышестоящих приложений   | Конфигурация завершения SSL может быть сложной          |
| Балансировка нагрузки, ограничение скорости         | Не является заменой аутентификации на уровне приложения |
| Низкий объем памяти                                 |                                                         |

**Examples:**

```nginx
server {
  listen 80;
  server_name example.com;

  # Serve SPA static files
  location / {
    root /usr/share/nginx/html;
    try_files $uri $uri/ /index.html;
  }

  # Proxy API requests
  location /api/ {
    proxy_pass http://api:4000/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
  }
}
```

**Thoughts:** `try_files ... /index.html` требуется для SPA маршрутизации на стороне клиента. В рабочей среде отключите TLS в Nginx или облачном балансировщике нагрузки, а не в Node. Что такое веб-сервер (HTTP, модули, gzip) → [6_web_api.md — Веб-сервер](./6_web_api_http.md#web-server). Полная конфигурация / `reload` vs `restart` /FastCGI → [15_devops.md — конфигурация Nginx](./15_devops.md#nginx-configuration). Апач (`.htaccess`, `Require`) → [15_devops.md — Apache](./15_devops.md#apache-http-server).

**Sources:** [nginx.org](https://nginx.org/)

</details>

<details>
  <summary>Redis</summary>

### Redis

**Definition:** Хранилище данных в памяти, используемое в качестве базы данных, кэша, хранилища сеансов, брокера сообщений и механизма потоковой передачи.

**When to use:** Хранение сеансов, кэширование ответов API, ограничение скорости, публикация/подписка, таблицы лидеров, функции реального времени.

**Pluses and minuses:**

| +                                                   | −                                               |
| --------------------------------------------------- | ----------------------------------------------- |
| Задержка менее миллисекунды                         | Риск потери данных без конфигурации постоянства |
| Универсальность (кэш, публикация/подписка, очереди) | Ограничение памяти — дорого в масштабе          |
| Простой API-интерфейс «ключ-значение»               | Не является заменой основной базы данных        |
| Срок действия на основе TTL                         | Режим кластера усложняет                        |

**Examples:**

```js
import { createClient } from "redis";

const redis = createClient({ url: process.env.REDIS_URL });
await redis.connect();

// Cache with TTL
await redis.set("user:123", JSON.stringify(user), { EX: 300 });
const cached = await redis.get("user:123");
```

**Thoughts:** Всегда устанавливайте TTL для ключей кэша. Используйте Redis для эфемерных данных; Postgres/MySQL как источник истины. Включите сохранение AOF/RDB, если данные должны пережить перезапуск.

**Sources:** [redis.io](https://redis.io/)

</details>

<details>
  <summary>Message broker</summary>

### Message broker

**Definition:** Программный посредник для асинхронной связи producer–consumer.

**When to use:** Микросервисы, фоновые задачи, event-driven системы — когда HTTP-связность недостаточна.

**Thoughts:** Полное покрытие — [Message broker — Core](./12_message_broker_core.md), [Kafka](./12_message_broker_kafka.md), [RabbitMQ](./12_message_broker_rabbitmq.md), [NATS](./12_message_broker_nats.md) (трек Backend).

**Sources:** [Message broker hub](./12_message_broker.md)

</details>

<details>
  <summary>Kafka vs RabbitMQ vs BullMQ</summary>

### Kafka vs RabbitMQ vs BullMQ

**Definition:** Три способа развязать producer и consumer в Node/build-пайплайнах. Выбирайте по **модели доставки**, а не по hype.


| | RabbitMQ                                        | Kafka                               | BullMQ                                 |
| | ----------  |  -----------------------------------------------  |  -----------------------------------  |
| | Model      | Queue + exchange (сообщения исчезают после ack) | Append-only log (offset у consumer) | Redis lists/streams как Node job queue |
| | Fan-out    | Exchanges (fanout/topic)                        | Consumer groups + partitions        | Repeatable jobs / queues по имени      |
| | Replay     | Плохо (сообщение удалено)                       | First-class (rewind offset)         | Ограниченно                            |
| | Throughput | Тысячи–десятки k msg/s                          | Сотни k+                            | Достаточно для типичных Node jobs      |
| | Ops        | Erlang broker                                   | Cluster, disk, partition key        | Redis, который уже есть                |

**When to use:** Email/worker jobs → **BullMQ** (этот файл). Сложная маршрутизация + ack → **RabbitMQ**. Event log, который команды читают в своём темпе → **Kafka**. Подробнее: [Message broker](./12_message_broker.md) (Backend). AWS managed: SQS ≈ queue, Kinesis/MSK ≈ log.

**Pluses and minuses:**

| +                                                             | −                                           |
| ------------------------------------------------------------- | ------------------------------------------- |
| Rabbit: гибкая маршрутизация, ack/nack                        | Не replay log                               |
| Kafka: replay, высокая пропускная способность, event sourcing | Тяжёлая эксплуатация; не простая task queue |
| BullMQ: минимальные ops при наличии Redis                     | Redis — SPOF без кластера                   |

**Thoughts:** Queue = consume and forget. Log = consume and remember (offset). Сравнение с NATS → [Kafka vs RabbitMQ vs NATS](./12_message_broker_core.md#kafka-vs-rabbitmq-vs-nats).

**Sources:** [Kafka intro](https://kafka.apache.org/intro) · [BullMQ](https://docs.bullmq.io/) · [Message broker — Core](./12_message_broker_core.md)

</details>

<a name="developer-experience"></a>

## Developer experience

<details>
  <summary>Storybook</summary>

### Storybook

**Definition:** Инструмент с открытым исходным кодом для разработки, документирования и тестирования компонентов пользовательского интерфейса изолированно — вне основного приложения.

**When to use:** Системы проектирования, библиотеки компонентов, команды дизайнеров, проверяющие состояния пользовательского интерфейса.

**Pluses and minuses:**

| +                                                              | −                                                               |
| -------------------------------------------------------------- | --------------------------------------------------------------- |
| Разрабатывайте компоненты без запуска полной версии приложения | Накладные расходы на обслуживание (истории отклоняются от кода) |
| Визуальная документация для системы проектирования             | Отдельная конфигурация сборки                                   |
| Тестирование взаимодействия с `@storybook/test`                | Может замедлить CI, если не распараллелить                      |
| Хроматическая интеграция для зрительной регрессии              |                                                                 |

**Examples:**

```tsx
// Button.stories.tsx
import type { Meta, StoryObj } from "@storybook/react";
import { Button } from "./Button";

const meta: Meta<typeof Button> = {
  component: Button,
  tags: ["autodocs"],
};
export default meta;

type Story = StoryObj<typeof Button>;

export const Primary: Story = {
  args: { variant: "primary", children: "Click me" },
};
export const Disabled: Story = {
  args: { variant: "primary", children: "Disabled", disabled: true },
};
```

**Thoughts:** Интегрируйте Storybook в CI, чтобы собирать неработающие истории. Используйте хроматический или потерянный пиксель для визуальной регрессии. Сопоставьте истории с компонентами (`Button.stories.tsx` рядом с `Button.tsx`).

**Sources:** [storybook.js.org](https://storybook.js.org/)

</details>

<details>
  <summary>OpenAPI / Swagger</summary>

### OpenAPI / Swagger

**Definition:** Спецификация OpenAPI (OAS) — это машиночитаемый формат описания конечных точек REST API. Swagger — это экосистема инструментов (пользовательский интерфейс, редактор, Codegen), построенная на основе OAS.

**When to use:** API REST с несколькими потребителями, разделение команд фронтенда и бэкенда, общедоступные API.

**Pluses and minuses:**

| +                                                                        | −                                                                     |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| Единый источник достоверной информации о контракте API                   | Спецификация может отклоняться от реализации                          |
| Автоматически создаваемые документы (пользовательский интерфейс Swagger) | Вывод кода требует очистки                                            |
| Генерация клиентского SDK                                                | Не идеально подходит для протоколов, отличных от REST (GraphQL, gRPC) |
| Контрактное тестирование между интерфейсом и серверной частью            |                                                                       |

**Examples:**

```yaml
# openapi.yaml
openapi: 3.1.0
info:
  title: User API
  version: 1.0.0
paths:
  /users/{id}:
    get:
      summary: Get user by ID
      parameters:
        - name: id
          in: path
          required: true
          schema: { type: string }
      responses:
        "200":
          description: User found
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/User"
components:
  schemas:
    User:
      type: object
      properties:
        id: { type: string }
        name: { type: string }
```

```bash
# Generate TypeScript client
npx openapi-typescript openapi.yaml -o src/api/schema.d.ts
```

**Thoughts:** Создавайте типы из спецификации, а не записывайте типы API вручную. Проверьте ответы сервера на соответствие спецификации в интеграционных тестах. Пользовательский интерфейс Swagger предназначен для исследования; Спецификация OpenAPI — это контракт.

**Sources:**

[swagger.io/спецификация](https://swagger.io/спецификация/)

---

</details>

<a name="interview-practice"></a>

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Вопросы от [0_interview.md](0_interview.md). Попробуйте ответить, прежде чем открывать решения.

### Question index

| Тема                                      | Ответ            |
| ----------------------------------------- | ---------------- |
| Что такое CI/CD                           | Вопросы и ответы |
| CI против CD                              | Вопросы и ответы |
| YAML против JSON                          | Вопросы и ответы |
| Вебпак против Vite                        | Вопросы и ответы |
| Дерево трясется                           | Вопросы и ответы |
| Слои Docker                               | Вопросы и ответы |
| Хранилище Docker (тома, привязка и tmpfs) | Вопросы и ответы |
| CMD против ENTRYPOINT                     | Вопросы и ответы |
| Жизненный цикл Docker-контейнера          | Вопросы и ответы |
| Докер Создание                            | Вопросы и ответы |
| Файлы блокировки                          | Вопросы и ответы |

---
