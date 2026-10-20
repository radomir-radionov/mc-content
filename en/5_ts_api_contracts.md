# TypeScript — API contracts

Typing HTTP and RPC boundaries: specs, codegen, runtime validation, and typed clients.

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [The Basics — strict & tsconfig](./5_ts_basics.md#strict-compiler-options) · [Web APIs — REST contract](./6_web_api_rest.md#rest-principles-and-contract) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                   | Covers                                        |
| ------------------------------------------------------- | --------------------------------------------- |
| [Overview](#overview)                                   | Where types meet network boundaries           |
| [Compile-time vs runtime](#compile-time-vs-runtime)     | Why TS alone is not enough for JSON           |
| [OpenAPI and codegen](#openapi-and-codegen)             | Single spec → shared TS types                 |
| [Zod and inferred types](#zod-and-inferred-types)       | One schema for validate + `z.infer`           |
| [`satisfies` for configs](#satisfies-for-configs)       | Check shapes without widening literals        |
| [Typed HTTP clients](#typed-http-clients)               | Generics, discriminated results, path params  |
| [Monorepo end-to-end types](#monorepo-end-to-end-types) | tRPC and shared packages vs OpenAPI           |
| [Contract drift](#contract-drift)                       | Tests and CI that catch spec vs code mismatch |

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

**Definition:** An **API contract** is the agreed request/response shapes, status codes, and error format between client and server. In TypeScript, you express that contract as **types** (compile-time) and often **schemas** (runtime at boundaries).

**How it works:**

| Layer        | Tooling                                         | Role                                               |
| ------------ | ----------------------------------------------- | -------------------------------------------------- |
| Design       | OpenAPI YAML/JSON, PR review                    | Human-readable source of truth                     |
| Compile-time | `openapi-typescript`, orval, hand-written types | Autocomplete and breaking-change errors in the IDE |
| Runtime      | Zod, Ajv, class-validator                       | Reject malformed JSON before business logic        |
| Transport    | `fetch`, axios, generated clients               | Wire calls; types do not travel over the network   |

**When to use:** Any REST/JSON API consumed by a TS frontend, BFF, or mobile app. Prefer a **machine-readable spec** when multiple teams or languages consume the same API.

**Thoughts:** TypeScript types are **erased at runtime** — never assume a successful `fetch` returned the shape you typed unless you validate or trust the producer completely. Pair compile-time types with boundary validation for external data.

**Sources:** [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Compile-time vs runtime

<details>
<summary>Compile-time vs runtime</summary>

**Definition:** Static types catch mistakes **before deploy**; runtime validators catch **wrong or hostile JSON** that still parses.

**How it works:**

- `response.json()` is typed as `any` or `unknown` until you narrow or validate.
- Hand-written interfaces duplicate the server — they drift unless generated from the same spec.
- **Parse, don't assert:** treat wire data as `unknown`, then validate → typed value.

**When to use:** Always validate **untrusted** input (public APIs, webhooks, third-party SDKs). Internal monorepo calls may rely on shared types only when the same build ships both sides.

**Pluses and minuses:**

| Approach        | Pluses                                | Minuses                                       |
| --------------- | ------------------------------------- | --------------------------------------------- |
| Types only      | Fast to write; zero runtime cost      | No protection if server sends wrong JSON      |
| Schema + infer  | Single definition; runtime + TS types | Extra dependency; parse errors to handle      |
| OpenAPI codegen | Stays aligned with documented API     | Regenerate step; spec must stay authoritative |

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

**Definition:** **OpenAPI** describes paths, methods, parameters, and response schemas. Generators emit TypeScript **types** or **clients** so consumers import the contract instead of copying interfaces.

**How it works:**

1. Maintain `openapi.yaml` (or export from Nest/Fastify/Spring).
2. Run codegen in CI or postinstall — e.g. **openapi-typescript** (types only), **orval** (types + React Query hooks + mutators).
3. Import `paths`, `components["schemas"]["User"]`, or generated `getUser()` wrappers.
4. On breaking changes, TypeScript errors in clients until they migrate.

**When to use:** REST/JSON with **multiple consumers** (web, mobile, partners) or when you want Swagger UI + typed clients from one file.

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

**Thoughts:** Codegen types describe **intent**; they do not validate responses unless you add a runtime layer. See [Contract drift](#contract-drift).

**Sources:** [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [openapi-typescript](https://github.com/drwpow/openapi-typescript)

</details>

---

## Zod and inferred types

<details>
<summary>Zod and inferred types</summary>

**Definition:** **Zod** (and similar libraries) define a **runtime schema**; `z.infer<typeof schema>` produces the matching TypeScript type — one source of truth.

**How it works:**

- Server: validate `req.body` with the same schema used to infer handler types (or share schema via package).
- Client: parse JSON after `fetch` before passing data to React state.
- Optional: `zod-to-openapi` or tRPC input validators to keep REST docs and TS aligned.

**When to use:** BFF routes, form payloads, env vars, and any JSON from outside your process.

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

**Definition:** The **`satisfies`** operator checks that a value matches a type **without widening** literal types — useful for mock fixtures, route maps, and static API config that must stay exhaustively typed.

**How it works:**

- `const routes = { ... } satisfies Record<string, RouteHandler>` — keys stay literal; each handler is checked.
- Unlike `: Record<...>`, `satisfies` preserves `"GET" | "POST"` unions on keys when you use `as const` patterns.

**When to use:** Client-side route tables, feature flags keyed by API version, test fixtures that must match `User` but keep literal ids for readability.

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

**Definition:** Thin wrappers around `fetch` or axios that **parameterize** response (and sometimes error) types — often built with generics and discriminated unions from [Type Manipulation](./5_ts_type_manipulation.md).

**How it works:**

| Pattern                | Purpose                                                        |
| ---------------------- | -------------------------------------------------------------- |
| `fetchJson<T>(url)`    | Caller supplies `T`; trust or validate separately              |
| `ApiResponse<T>` union | `{ ok: true; data: T } \| { ok: false; error: ApiError }`      |
| Path params            | Template literal types or codegen from OpenAPI `{id}` segments |
| Status codes           | Overloads or mapped types per route (usually from codegen)     |

**When to use:** Shared client module in the frontend; combine with Zod when responses are not fully trusted.

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

**Thoughts:** Prefer codegen or `z.infer` for `T` on public APIs; use bare generics for internal services with integration tests.

**Sources:** [TypeScript handbook — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

</details>

---

## Monorepo end-to-end types

<details>
<summary>Monorepo end-to-end types</summary>

**Definition:** When client and server share a **TypeScript monorepo**, you can skip OpenAPI for some surfaces and export **router types** (tRPC) or **shared DTO packages** instead.

**How it works:**

| Style                       | Types from                            | Best for                            |
| --------------------------- | ------------------------------------- | ----------------------------------- |
| OpenAPI + codegen           | Spec file                             | Public REST, polyglot clients       |
| Shared `packages/api-types` | Hand-maintained or Zod exports        | Internal web + mobile TS            |
| **tRPC**                    | Server `AppRouter` inferred on client | Fullstack TS only; no separate spec |

**When to use:** tRPC when every consumer is TS in the same repo; OpenAPI when you need documentation, non-TS clients, or HTTP caching keyed on resource URLs.

**Pluses and minuses:**

|                             | OpenAPI           | tRPC                           |
| --------------------------- | ----------------- | ------------------------------ |
| Public API                  | Strong            | Weak default                   |
| Refactor safety in monorepo | Good with codegen | Excellent with inferred router |
| CDN / cache-friendly GET    | Natural           | Not the primary model          |

**Sources:** [Web APIs — tRPC](./6_web_api_rest.md#6-trpc) · [Web APIs — REST contract workflow](./6_web_api_rest.md#rest-principles-and-contract)

</details>

---

## Contract drift

<details>
<summary>Contract drift</summary>

**Definition:** **Contract drift** is when implementation, spec, and client types disagree — bugs appear only at runtime or in production.

**How it works:**

- **Contract tests:** Dredd, Schemathesis, or Pact replay requests against OpenAPI.
- **CI codegen diff:** Fail build if generated types change without committed spec update.
- **Versioning:** `/v2/` or header versioning; never rename fields silently in `v1`.

**When to use:** Teams larger than one, external API consumers, or regulated release processes.

**Thoughts:** TypeScript errors from codegen are the first line of defense; contract tests are the second for response **shapes** and status codes the type system cannot see.

**Sources:** [Web APIs — REST contract workflow](./6_web_api_rest.md#rest-principles-and-contract)

</details>
