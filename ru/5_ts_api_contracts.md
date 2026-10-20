# TypeScript — API-контракты

Типизация границ HTTP и RPC: спецификации, codegen, runtime-валидация и типизированные клиенты.

[← Назад к хабу](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [The Basics — strict & tsconfig](./5_ts_basics.md#strict-compiler-options) · [Web APIs — REST contract](./6_web_api_rest.md#rest-principles-and-contract) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Где типы встречаются с сетевыми границами |
| [Compile-time vs runtime](#compile-time-vs-runtime) | Почему одного TS недостаточно для JSON |
| [OpenAPI and codegen](#openapi-and-codegen) | Одна спека → общие TS-типы |
| [Zod and inferred types](#zod-and-inferred-types) | Одна схема для validate + `z.infer` |
| [`satisfies` for configs](#satisfies-for-configs) | Проверка форм без расширения литералов |
| [Typed HTTP clients](#typed-http-clients) | Generics, discriminated results, path params |
| [Monorepo end-to-end types](#monorepo-end-to-end-types) | tRPC и shared packages vs OpenAPI |
| [Contract drift](#contract-drift) | Тесты и CI, ловящие расхождение спеки и кода |

### Sections

- [Overview](#overview)
- [Compile-time vs runtime](#compile-time-vs-runtime)
- [OpenAPI and codegen](#openapi-and-codegen)
- [Zod and inferred types](#zod-and-inferred-types)
- [`satisfies` for configs](#satisfies-for-configs)
- [Typed HTTP clients](#typed-http-clients)
- [Monorepo end-to-end types](#monorepo-end-to-end-types)
- [Contract drift](#contract-drift)

---

## Overview

<details>
<summary>API contract typing overview</summary>

**Definition:** **API contract** — согласованные формы запросов и ответов, коды статуса и формат ошибок между клиентом и сервером. В TypeScript контракт выражают **типами** (compile-time) и часто **схемами** (runtime на границах).

**How it works:**

| Layer | Tooling | Role |
| ----- | ------- | ---- |
| Design | OpenAPI YAML/JSON, PR review | Человекочитаемый source of truth |
| Compile-time | `openapi-typescript`, orval, ручные типы | Autocomplete и ошибки breaking change в IDE |
| Runtime | Zod, Ajv, class-validator | Отклонение некорректного JSON до business logic |
| Transport | `fetch`, axios, сгенерированные клиенты | Вызовы по сети; типы по HTTP не передаются |

**When to use:** Любой REST/JSON API, который потребляет TS-frontend, BFF или мобильное приложение. Предпочитайте **машиночитаемую спеку**, когда один API используют несколько команд или языков.

**Thoughts:** Типы TypeScript **стираются в runtime** — не предполагайте, что успешный `fetch` вернул ту форму, которую вы типизировали, пока не провалидируете данные или полностью не доверяете продюсеру. Сочетайте compile-time типы с валидацией на границе для внешних данных.

**Sources:** [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Compile-time vs runtime

<details>
<summary>Compile-time vs runtime</summary>

**Definition:** Статические типы ловят ошибки **до деплоя**; runtime-валидаторы ловят **неверный или враждебный JSON**, который всё равно парсится.

**How it works:**

- `response.json()` имеет тип `any` или `unknown`, пока вы не сузите тип или не провалидируете.
- Ручные интерфейсы дублируют сервер — они расходятся, если не генерировать из той же спеки.
- **Parse, don't assert:** относитесь к данным с провода как к `unknown`, затем validate → типизированное значение.

**When to use:** Всегда валидируйте **недоверенный** ввод (публичные API, webhooks, сторонние SDK). Внутри monorepo можно опираться только на shared types, когда одна сборка поставляет обе стороны.

**Pluses and minuses:**

| Approach | Pluses | Minuses |
| -------- | ------ | ------- |
| Types only | Быстро писать; нулевая runtime-стоимость | Нет защиты, если сервер прислал неверный JSON |
| Schema + infer | Одно определение; runtime + TS-типы | Доп. зависимость; нужно обрабатывать ошибки parse |
| OpenAPI codegen | Совпадает с задокументированным API | Шаг regenerate; спека должна быть авторитетной |

**Examples:**

```typescript
// ❌ Trusting the network
type User = { id: string; email: string };
const user = (await res.json()) as User; // no runtime check

// ✅ unknown → validate → typed
const raw: unknown = await res.json();
const user = userSchema.parse(raw); // Zod throws or returns User
```

**Sources:** [TypeScript handbook — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

</details>

---

## OpenAPI and codegen

<details>
<summary>OpenAPI and codegen</summary>

**Definition:** **OpenAPI** описывает paths, methods, parameters и response schemas. Генераторы выдают TypeScript **types** или **clients**, чтобы потребители импортировали контракт вместо копирования интерфейсов.

**How it works:**

1. Поддерживайте `openapi.yaml` (или экспорт из Nest/Fastify/Spring).
2. Запускайте codegen в CI или postinstall — например **openapi-typescript** (только типы), **orval** (типы + React Query hooks + mutators).
3. Импортируйте `paths`, `components["schemas"]["User"]` или сгенерированные обёртки `getUser()`.
4. При breaking changes TypeScript ругается в клиентах, пока они не мигрируют.

**When to use:** REST/JSON с **несколькими потребителями** (web, mobile, partners) или когда нужны Swagger UI и типизированные клиенты из одного файла.

**Examples:**

```typescript
// Generated (conceptual) from openapi-typescript
import type { paths } from "./api.generated";

type GetUserResponse =
  paths["/users/{id}"]["get"]["responses"]["200"]["content"]["application/json"];

async function getUser(id: string): Promise<GetUserResponse> {
  const res = await fetch(`/users/${id}`);
  if (!res.ok) throw new Error(String(res.status));
  return res.json();
}
```

**Thoughts:** Типы codegen описывают **намерение**; они не валидируют ответы, пока не добавите runtime-слой. См. [Contract drift](#contract-drift).

**Sources:** [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [openapi-typescript](https://github.com/drwpow/openapi-typescript)

</details>

---

## Zod and inferred types

<details>
<summary>Zod and inferred types</summary>

**Definition:** **Zod** (и похожие библиотеки) задают **runtime schema**; `z.infer<typeof schema>` даёт соответствующий TypeScript-тип — один source of truth.

**How it works:**

- Server: валидируйте `req.body` той же схемой, из которой выводятся типы handler (или шарьте schema через package).
- Client: парсите JSON после `fetch` перед передачей в state React.
- Опционально: `zod-to-openapi` или tRPC input validators, чтобы REST-доки и TS оставались согласованными.

**When to use:** BFF routes, payload форм, env vars и любой JSON извне вашего процесса.

**Examples:**

```typescript
import { z } from "zod";

const userSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  role: z.enum(["admin", "member"]),
});

type User = z.infer<typeof userSchema>;

export function parseUser(raw: unknown): User {
  return userSchema.parse(raw);
}
```

**Sources:** [Zod](https://zod.dev/)

</details>

---

## satisfies for configs

<details>
<summary>satisfies for configs</summary>

**Definition:** Оператор **`satisfies`** проверяет, что значение соответствует типу **без widening** литеральных типов — удобно для mock fixtures, карт маршрутов и статической API-конфигурации с exhaustive typing.

**How it works:**

- `const routes = { ... } satisfies Record<string, RouteHandler>` — ключи остаются литералами; каждый handler проверяется.
- В отличие от `: Record<...>`, `satisfies` сохраняет union `"GET" | "POST"` на ключах при паттернах с `as const`.

**When to use:** Клиентские таблицы маршрутов, feature flags по версии API, test fixtures, которые должны совпадать с `User`, но сохранять литеральные id для читаемости.

**Examples:**

```typescript
type Endpoint = { method: "GET" | "POST"; path: string };

const catalog = {
  listUsers: { method: "GET", path: "/users" },
  createUser: { method: "POST", path: "/users" },
} satisfies Record<string, Endpoint>;

type Key = keyof typeof catalog; // "listUsers" | "createUser"
```

**Sources:** [TypeScript handbook — satisfies](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator)

</details>

---

## Typed HTTP clients

<details>
<summary>Typed HTTP clients</summary>

**Definition:** Тонкие обёртки над `fetch` или axios, которые **параметризуют** тип ответа (и иногда ошибки) — часто через generics и discriminated unions из [Type Manipulation](./5_ts_type_manipulation.md).

**How it works:**

| Pattern | Purpose |
| ------- | ------- |
| `fetchJson<T>(url)` | Вызывающий задаёт `T`; доверие или validate отдельно |
| `ApiResponse<T>` union | `{ ok: true; data: T } \| { ok: false; error: ApiError }` |
| Path params | Template literal types или codegen из OpenAPI-сегментов `{id}` |
| Status codes | Overloads или mapped types по route (обычно из codegen) |

**When to use:** Общий client module во frontend; сочетайте с Zod, если ответам не полностью доверяете.

**Examples:**

```typescript
type ApiError = { code: string; message: string };

type ApiResponse<T> =
  | { success: true; data: T }
  | { success: false; error: ApiError };

async function fetchJson<T>(url: string): Promise<ApiResponse<T>> {
  const res = await fetch(url);
  const body: unknown = await res.json();
  if (!res.ok) {
    return { success: false, error: body as ApiError };
  }
  return { success: true, data: body as T };
}
```

**Thoughts:** Для публичных API предпочитайте codegen или `z.infer` для `T`; голые generics — для внутренних сервисов с integration tests.

**Sources:** [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Monorepo end-to-end types

<details>
<summary>Monorepo end-to-end types</summary>

**Definition:** Когда client и server делят **TypeScript monorepo**, для части поверхностей можно обойтись без OpenAPI и экспортировать **router types** (tRPC) или **shared DTO packages**.

**How it works:**

| Style | Types from | Best for |
| ----- | ---------- | -------- |
| OpenAPI + codegen | Spec file | Публичный REST, polyglot clients |
| Shared `packages/api-types` | Ручная поддержка или экспорт Zod | Внутренний web + mobile TS |
| **tRPC** | `AppRouter` сервера, inferred на клиенте | Fullstack только TS; отдельная спека не нужна |

**When to use:** tRPC, когда все потребители — TS в том же repo; OpenAPI, когда нужна документация, non-TS clients или HTTP caching по resource URLs.

**Pluses and minuses:**

| | OpenAPI | tRPC |
| - | ------- | ---- |
| Public API | Сильная сторона | Слабый default |
| Refactor safety в monorepo | Хорошо с codegen | Отлично с inferred router |
| CDN / cache-friendly GET | Естественно | Не основная модель |

**Sources:** [Web APIs — tRPC](./6_web_api_rest.md#6-trpc) · [Web APIs — REST contract workflow](./6_web_api_rest.md#rest-principles-and-contract)

</details>

---

## Contract drift

<details>
<summary>Contract drift</summary>

**Definition:** **Contract drift** — расхождение implementation, spec и client types; баги проявляются только в runtime или в production.

**How it works:**

- **Contract tests:** Dredd, Schemathesis или Pact — прогон запросов против OpenAPI.
- **CI codegen diff:** Падающий build, если сгенерированные типы изменились без коммита обновлённой спеки.
- **Versioning:** `/v2/` или versioning через header; никогда не переименовывайте поля молча в `v1`.

**When to use:** Команды больше одного человека, внешние потребители API или регламентированные релизы.

**Thoughts:** Ошибки TypeScript от codegen — первая линия обороны; contract tests — вторая для **форм** ответов и status codes, которые type system не видит.

**Sources:** [Web APIs — REST contract workflow](./6_web_api_rest.md#rest-principles-and-contract)

</details>
