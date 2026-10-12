# Web APIs — API styles

REST, XML, SOAP, RPC, tRPC, and how they differ.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [2. REST, Principles & Contract](#rest-principles-and-contract)
- [XML](#xml)
- [3. SOAP](#3-soap)
- [4. RPC](#4-rpc)
- [5. REST vs SOAP vs RPC](#5-rest-vs-soap-vs-rpc)
- [6. tRPC](#6-trpc)

---

## 2. REST, Principles & Contract

See also: [SOAP](#3-soap) · [RPC](#4-rpc) · [REST vs SOAP vs RPC](#5-rest-vs-soap-vs-rpc) · [tRPC](#6-trpc)

<details>
  <summary>REST, Principles & Contract</summary>

### REST, Principles & Contract

**Definition:** REST (Representational State Transfer) is a **style of communication** between components: all needed data is specified in the request (path, HTTP method, query, or body). It is one of the most common API styles on the internet. Fielding’s REST is six architectural constraints for distributed hypermedia — not “JSON over HTTP” by itself. A **REST contract** is the agreed specification between client and server — URLs, HTTP methods, request/response shapes, status codes, and error formats.

**How it works:**

The URL stays a **noun**; CRUD lives in the HTTP method. Same address, different verb.

| CRUD       | HTTP                                               |
| ---------- | -------------------------------------------------- |
| Create (C) | `POST`                                             |
| Read (R)   | `GET`                                              |
| Update (U) | `PUT` (whole object) / `PATCH` (named fields only) |
| Delete (D) | `DELETE`                                           |

Fielding’s six constraints:

| Principle                     | Meaning                                                                                                                                                                                                                                                  |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Client–server**             | **Presentation and data live on separate tiers.** The client does not assume server internals; the server does not depend on a specific UI. Each side can evolve independently (e.g. swap a SPA for a mobile app).                                       |
| **Stateless**                 | **The server stores no session state between requests.** Every request must include all context the server needs (auth token, resource id, filters). Any server instance can handle any request — this is what makes horizontal scaling straightforward. |
| **Uniform interface**         | **Resources use URIs, HTTP methods, and negotiated representations.** JSON, XML, and HTML are selected via `Accept` / `Content-Type`. Same patterns everywhere — no custom wire protocol per endpoint.                                                   |
| **Cacheable**                 | **Responses declare whether they can be cached and reused.** `Cache-Control`, `ETag`, and `Last-Modified` let clients, proxies, and CDNs serve stored copies to cut latency and origin load.                                                             |
| **Layered system**            | **The client cannot tell origin from intermediary.** Load balancers, CDNs, and API gateways add TLS termination, routing, and caching without breaking the uniform interface.                                                                            |
| **Code on demand (optional)** | **The server may send executable code the client runs.** Common on the Web (`<script>` tags); rare in JSON APIs. Fielding marks this constraint as optional.                                                                                             |

**REST contract** (how teams agree on the API):

The contract is the **shared source of truth** between frontend, backend, mobile, and external consumers. Without it, each team guesses URLs, field names, and error shapes — integration breaks at runtime instead of compile time.

**What the contract specifies:**

| Area                     | Agree on                                                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Resources**            | Path templates (`/users/{id}`), allowed HTTP methods per path                                                            |
| **Payloads**             | Request/response JSON schemas (field names, types, required vs optional)                                                 |
| **Status codes**         | Which codes each endpoint returns (`201` + `Location` on create, `404` when missing)                                     |
| **Errors**               | One error shape everywhere — e.g. `{ "code", "message", "details?" }`, not ad-hoc strings                                |
| **Auth**                 | Header name (`Authorization: Bearer …`), which routes need auth                                                          |
| **Pagination / filters** | Query params (`?limit=`, `?cursor=`) and response meta — see [REST versioning & pagination](#rest-versioning-pagination) |

**How teams agree (typical workflow):**

1. **Design first** — backend + frontend sketch endpoints and DTOs in a PR (OpenAPI YAML or Swagger UI mock) **before** coding handlers.
2. **Review the spec** — both sides sign off on paths, methods, and schemas; mobile/external teams join if they consume the API.
3. **Generate types** — `openapi-typescript`, **orval**, or similar produce TS types/hooks from the spec; clients import generated types, not hand-written duplicates.
4. **Validate at boundaries** — server validates input (Zod, class-validator, JSON Schema); optional contract tests assert responses match the spec.
5. **Version breaking changes** — ship `/v2/` or header versioning; keep `v1` until clients migrate — never silently rename fields.

| Layer                  | Tool                                                                   | Role                                                                    |
| ---------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Documentation**      | [OpenAPI / Swagger](./10_build_infra.md#openapi--swagger) spec in repo | Single machine-readable contract + Swagger UI for exploration           |
| **Shared types**       | orval, openapi-typescript, openapi-generator                           | Frontend/mobile import the same shapes the spec describes               |
| **Runtime validation** | Zod, Ajv, framework validators                                         | Reject bad requests before business logic; return consistent 400 bodies |
| **Contract tests**     | Dredd, Schemathesis, or snapshot tests against OpenAPI                 | Catch drift when implementation diverges from the spec                  |
| **Versioning**         | `/v2/` path or `Accept: application/vnd…+json;version=2`               | Breaking changes get a new version; old clients keep working            |

**Breaking vs non-breaking changes:**

| Non-breaking (usually OK in same version)         | Breaking (needs new version or deprecation window) |
| ------------------------------------------------- | -------------------------------------------------- |
| Add optional field to response                    | Remove or rename a field                           |
| Add new endpoint                                  | Change field type (`string` → `number`)            |
| Add new optional query param                      | Change status code semantics                       |
| Add new enum value (if clients tolerate unknowns) | Rename path or change HTTP method meaning          |

**Example (minimal OpenAPI fragment):**

```yaml
paths:
  /users/{id}:
    get:
      responses:
        "200":
          content:
            application/json:
              schema: { $ref: "#/components/schemas/User" }
        "404":
          content:
            application/json:
              schema: { $ref: "#/components/schemas/ApiError" }
components:
  schemas:
    User:
      type: object
      required: [id, name]
      properties:
        id: { type: integer }
        name: { type: string }
    ApiError:
      type: object
      required: [code, message]
      properties:
        code: { type: string }
        message: { type: string }
```

**When to use:** Public web/CRUD APIs. Fix over/under-fetch with a **BFF**. Deeper mapping: [CRUD & REST mapping](#crud--rest-mapping). **Always** define a contract when frontend and backend are separate teams, external integrators exist, or more than one client (web + mobile) shares the API. Interview: name the six constraints.

**Pluses and minuses:**

| +                            | −                                 |
| ---------------------------- | --------------------------------- |
| Most common style            | Bad design over- or under-fetches |
| Built on HTTP                | Strict REST (HATEOAS) is rare     |
| Easy to read (verbs + paths) | Many endpoints for nested data    |

**Examples:**

```http
GET /api/users/42 HTTP/1.1
Accept: application/json

→ 200 OK
{ "id": 42, "name": "Ada" }
```

**Thoughts:** Same address, different method — details in verb + params/body. _Contract = API is a product: spec first, review together, generate types, test drift, version breaks._

**Sources:**

[ByteByteGo — REST cheatsheet](https://blog.bytebytego.com/p/ep94-rest-api-cheatsheet) · [Doka — What is an API](https://doka.guide/tools/api/) · [Roy Fielding — REST dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) · [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)

---

</details>

<details>
  <summary>REST alternatives</summary>

### REST alternatives

**Definition:**

Other styles trade REST’s resource + HTTP-verb model for queries, streams, or remote procedures. Full notes: [SOAP](#3-soap) · [RPC](#4-rpc) · [REST vs SOAP vs RPC](#5-rest-vs-soap-vs-rpc) · [tRPC](#6-trpc).

| Style     | Strength                        | Typical use             |
| --------- | ------------------------------- | ----------------------- |
| GraphQL   | Client-shaped queries           | Mobile, complex UIs     |
| gRPC      | Binary, HTTP/2, streaming       | Microservices           |
| WebSocket | Full-duplex real-time           | Chat, games             |
| JSON-RPC  | Simple remote procedures        | Lightweight RPC         |
| tRPC      | TS end-to-end types, no codegen | TS fullstack / monorepo |
| SOAP      | XML envelopes, WS-\*            | Legacy enterprise       |

**Examples:**

```graphql
query {
  user(id: 42) {
    name
    posts {
      title
    }
  }
}
```

**Sources:**

[GraphQL](https://graphql.org/) · [gRPC](https://grpc.io/)

---

</details>

<details>
  <summary>CRUD & REST mapping</summary>

### CRUD & REST mapping

**Definition:** CRUD maps Create, Read, Update, and Delete operations to HTTP methods against resource URLs. In REST-style APIs the path stays a noun; the verb lives in the method.

**How it works:**

Same collection URL, different methods. `PUT` replaces the whole resource; `PATCH` applies a partial update and may not be idempotent (e.g. increment). Successful updates/deletes often return **204 No Content**.

| Operation        | HTTP method | Example                                             |
| ---------------- | ----------- | --------------------------------------------------- |
| Create           | POST        | `POST /api/orders/` → often **201** + new id (`42`) |
| Read (list)      | GET         | `GET /api/orders`                                   |
| Read (one)       | GET         | `GET /api/orders/42` → JSON (typical)               |
| Update (full)    | PUT         | `PUT /api/orders/42`                                |
| Update (partial) | PATCH       | `PATCH /api/orders/42`                              |
| Delete           | DELETE      | `DELETE /api/orders/42` → often **204**             |

**When to use:** Shop/order APIs and any CRUD HTTP surface. Prefer REST when caching and HTTP semantics matter.

**Examples:**

```http
POST   /api/orders/      → create; response often new id (42)
GET    /api/orders/42    → JSON (typical; XML/HTML possible)
PUT    /api/orders/42    → full replace; often 204 or id
PATCH  /api/orders/42    → only sent fields
DELETE /api/orders/42    → often 204 or deleted id
```

**Thoughts:** Address names the resource; HTTP method names the CRUD action.

**Sources:**

[MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods) · [Doka — What is an API](https://doka.guide/tools/api/)

---

</details>

<details>
  <summary>REST vs RESTful</summary>

### REST vs RESTful

**Definition:**

- **REST API** — a web service that follows **some** REST ideas (HTTP, resources, JSON/XML). Flexible — endpoints and conventions can be adapted. Not every “REST API” meets all constraints.
- **RESTful API** — a web service that **fully adheres** to REST: all six constraints ([REST, Principles & Contract](#rest-principles-and-contract)), proper HTTP methods, **resource-based URLs** (nouns, not verbs in the path), statelessness, caching, and HATEOAS where applicable.

**One-line difference:** Every **RESTful API** is a **REST API**, but not every **REST API** is **RESTful**.

**How it works — comparison:**

| Feature           | REST API                                        | RESTful API                                                                 |
| ----------------- | ----------------------------------------------- | --------------------------------------------------------------------------- |
| **Strictness**    | Based on REST, **not all** constraints required | **All six** constraints implemented                                         |
| **HTTP methods**  | Uses GET/POST/etc., not always correctly        | **Strict** mapping — GET read, POST create, PUT/PATCH update, DELETE remove |
| **URL structure** | Flexible, can vary                              | **Uniform** — resources as nouns (`/users/1/orders`)                        |
| **Statelessness** | Often stateless, **not guaranteed**             | **Fully stateless** — each request carries all needed context               |
| **Caching**       | Optional                                        | **Explicit** cache headers for performance                                  |
| **HATEOAS**       | Often omitted                                   | **Supported** — links in responses for next actions                         |
| **Data format**   | JSON, XML, HTML — mixed                         | **Consistent** representation (usually JSON or XML)                         |

**What RESTful means in practice:**

| Rule                     | Do                                    | Avoid                               |
| ------------------------ | ------------------------------------- | ----------------------------------- |
| **HTTP method = action** | `GET /orders/42`, `DELETE /orders/42` | `POST /orders/42/delete`            |
| **URL = noun**           | `/users`, `/users/1/orders`           | `/getUser`, `/createOrder`          |
| **Status codes**         | `201` on create, `404` when missing   | Always `200` with `{ error: true }` |

**When to use REST API:**

- **Simple projects** — prototypes, small apps, straightforward data access; quick to ship.
- **Flexibility** — varying data shapes, no need to enforce every REST rule; pragmatic endpoint design.

**When to use RESTful API:**

- **Large, complex systems** — e-commerce, enterprise, many consumers and teams.
- **Consistency** — separate frontend/backend teams, **external or internal integrators** who need predictable, readable URLs without reading your code.
- **Scale** — high traffic, caching, and uniform structure matter; strict rules pay off long-term.

**Trade-off:** RESTful URLs are not always ideal for **niche actions** (bulk ops, search, workflows). A documented exception is fine if the rest of the API stays consistent.

**Thoughts:** In interviews: REST API = loose label; RESTful = **strict compliance**. Judge methods, nouns, statelessness — not just “JSON over HTTP”.

**Sources:**

[Roy Fielding — REST dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) · [MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods) · [MDN — REST](https://developer.mozilla.org/en-US/docs/Glossary/REST) · [GeeksforGeeks — REST API vs RESTful API](https://www.geeksforgeeks.org/advance-java/know-the-difference-between-rest-api-and-restful-api/)

---

</details>

<details>
  <summary>REST versioning & pagination</summary>

### REST versioning & pagination

**Definition:** **Versioning** lets you evolve APIs without breaking existing clients. **Pagination** splits large collections into pages so responses stay bounded.

**How it works:**

- **Versioning** — clients pin a version via URL path (`/v2/users`), query (`?version=2`), or header (`Accept: application/vnd.api+json;version=2`). Server routes to the matching handler/schema.
- **Offset pagination** — `?page=2&limit=20` → `OFFSET 20 LIMIT 20`. Simple but slow at high offsets (DB scans skipped rows).
- **Cursor/keyset pagination** — `?cursor=eyJpZCI6MTIzfQ&limit=20` encodes last-seen sort key (`id` or `created_at`). Stable under inserts; no deep-offset penalty.
- **Link headers** — `Link: <...?page=3>; rel="next"` (RFC 8288) or JSON `{ data, meta: { nextCursor, hasMore } }`.

**When to use:**

- Ship **v2** when removing fields or changing semantics; keep **v1** for a deprecation window.
- Use **cursor** for infinite scroll and feeds; **offset** for admin tables with page numbers.
- Always return **total count** only when cheap — counting millions of rows is expensive.

**Pluses and minuses:**

| Strategy                  | +                        | −                                                     |
| ------------------------- | ------------------------ | ----------------------------------------------------- |
| URL path version (`/v1/`) | Obvious in logs and docs | URL churn; multiple code paths                        |
| Header version            | Clean URLs               | Harder to test in browser                             |
| Offset pagination         | Easy to implement        | Slow `OFFSET` at scale; duplicates/skips on live data |
| Cursor pagination         | Efficient, stable sort   | Cannot jump to arbitrary page number                  |

**Examples:**

```http
GET /api/v2/books?limit=20&cursor=eyJpZCI6MTAwfQ HTTP/1.1
Accept: application/json

→ 200 OK
{
  "data": [ { "id": 101, "title": "Dune" } ],
  "meta": { "limit": 20, "nextCursor": "eyJpZCI6MTIwfQ", "hasMore": true }
}
```

**Thoughts:** Version in URL is the most common interview answer. Never break clients silently — announce sunset dates and return `410 Gone` or deprecation headers (`Sunset`, `Deprecation`).

**Sources:**

[MDN — HTTP pagination patterns](https://developer.mozilla.org/en-US/docs/Web/HTTP) · [RFC 8288 — Web Linking](https://datatracker.ietf.org/doc/html/rfc8288)

---

</details>

<a name="xml"></a>

## XML

See also: [SOAP](#3-soap) · [REST](#rest-principles-and-contract) · [HTML vs XML](./1_html_structure.md#html-vs-xml) · [canvas vs SVG](./1_html_media.md#canvas-vs-svg)

<details>
  <summary>XML</summary>

### XML

**Definition:** XML (Extensible Markup Language) is a markup language like HTML, but **you define the tags**. It is a standardized _syntax_ for storing and exchanging data (enterprise APIs, SOAP, RSS, SVG, config) — not a UI language.

**How it works:**

- Markup vs data: tags/attributes are structure; text between tags is payload.
- **One root** element wraps the rest. Extra roots → not well-formed.
- **XML declaration** (`<?xml version="1.0" encoding="UTF-8"?>`) is optional meta, **not a tag**. If present, it must be the first thing in the file (before comments).
- **Well-formed:** every start tag has an end tag; empty nodes are `<cat></cat>` or `<cat />`; attributes are quoted (`"` or `'`); names are case-sensitive; unescaped `<` `&` in text are illegal (`&lt;` `&amp;`).
- **Valid** is stricter: well-formed _and_ matching a DTD/XSD. Browsers often only check well-formedness.
- Built-in entities: `&lt;` `&gt;` `&amp;` `&quot;` `&apos;`. More via DTD.
- **Attributes** = metadata (ids, categories). **Child elements** = the data you would put in JSON fields.
- **Namespaces (`xmlns`):** same local name (`table`) in two vocabularies. Prefix: `xmlns:classic="URI"` then `classic:table`. Default: `xmlns="URI"` on an element and descendants. The URI is an **identifier**, not a document the parser must fetch. Declaring a namespace does **not** by itself restrict tags to a schema — that is validation. `xmlns=""` undeclares the default namespace (Namespaces in XML 1.0). You can hoist prefix declarations onto the root.
- Standalone **SVG as XML** needs `xmlns="http://www.w3.org/2000/svg"` on the outer `<svg>` or the UA will not treat it as SVG. Inline SVG in HTML serialization does **not** require `xmlns` (namespace is implied).

|         | XML                      | HTML                   | JSON                                      |
| ------- | ------------------------ | ---------------------- | ----------------------------------------- |
| Tags    | You invent them          | Fixed vocabulary       | No tags — objects/arrays                  |
| Purpose | Data / documents         | Display in a browser   | Data interchange                          |
| Errors  | Strict well-formedness   | Browsers recover       | `JSON.parse` throws                       |
| Types   | Text (+ optional schema) | N/A                    | Number, bool, null, string, object, array |
| In JS   | Parse to DOM / tree      | Browser already parsed | Native after `JSON.parse`                 |

**When to use:**

SOAP / XML-RPC; RSS/Atom; SVG/MathML files; legacy enterprise interchange; when a schema (XSD) is the contract. Prefer JSON for new public HTTP APIs.

#### Common pitfalls

- Unclosed tags — the whole document is rejected.
- Treating `xmlns` URIs as files you must host, or assuming `xmlns` validates elements.
- Missing SVG `xmlns` on a `.svg` file opened as XML.
- Assuming JSON “needs no parsing” in JS — the wire is still a string.
- Confusing XML (syntax) with SOAP (envelope protocol on top of XML).

**Pluses and minuses:**

| +                                                          | −                                                                                  |
| ---------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Same syntax everywhere; easy to round-trip between systems | Verbose (open + close tags)                                                        |
| Human-readable tree; attributes + nested elements          | All values are text unless a schema adds types                                     |
| Namespaces disambiguate mixed vocabularies                 | Namespaces are easy to get wrong                                                   |
| Separates data from HTML (store XML, render HTML)          | Must parse (`DOMParser`, backend XML libs) — unlike a JS object after `JSON.parse` |

**Examples:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<petstore>
  <pet category="cat">
    <name>Boris</name>
    <year>2021</year>
    <price>485.00</price>
  </pet>
  <pet category='dog "corgi"' />
</petstore>
```

Namespace clash (`table` as HTML vs `table` as “cat furniture”) — prefixes on the root:

```xml
<root
  xmlns:html="http://www.w3.org/1999/xhtml"
  xmlns:cat="https://example.com/cat-table">
  <html:table>
    <html:tr><html:th>Col</html:th></html:tr>
  </html:table>
  <cat:table>
    <cat:name>Boris</cat:name>
  </cat:table>
</root>
```

Film record (typical payload):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<film>
  <name>Shrek</name>
  <description>animation, fantasy, comedy</description>
  <creationYear>2001</creationYear>
  <budget>60000000</budget>
</film>
```

**Thoughts:** XML is a strict, self-describing tree for data; HTML is for pages; JSON is the usual API wire format today.

**Sources:**

[MDN — XML introduction](https://developer.mozilla.org/en-US/docs/Web/XML/Guides/XML_introduction) · [MDN — SVG namespaces](https://developer.mozilla.org/en-US/docs/Web/SVG/Guides/Namespaces_crash_course) · [Doka — XML](https://doka.guide/tools/xml/)

---

</details>

<a name="3-soap"></a>

## 3. SOAP

See also: [XML](#xml) · [REST](#rest-principles-and-contract) · [RPC](#4-rpc) · [REST vs SOAP vs RPC](#5-rest-vs-soap-vs-rpc) · [tRPC](#6-trpc)

<details>
  <summary>SOAP</summary>

### SOAP

**Definition:** SOAP (Simple Object Access Protocol) is a **data-exchange format**: every message follows a fixed structure, almost always XML (Envelope / Header / Body / Fault). REST is an architectural _style_; SOAP is a _format_ — they are not the same kind of thing. XML syntax itself: [XML](#xml).

**How it works:**

Unlike REST, **which HTTP method you use does not matter** for the SOAP contract. The packet is nested XML:

```text
┌─ SOAP-ENV:Envelope ─────────────────────────────────┐
│  root element — this XML *is* the message           │
│  ┌─ SOAP-ENV:Header ──────────────────────────────┐ │
│  │ attributes of the message, e.g. security       │ │
│  └────────────────────────────────────────────────┘ │
│  ┌─ SOAP-ENV:Body ────────────────────────────────┐ │
│  │ payload the applications exchange              │ │
│  └────────────────────────────────────────────────┘ │
│                              (Fault — optional)  ○  │
└─────────────────────────────────────────────────────┘
```

| Part         | RU (Doka)                                                       | Definition                                |
| ------------ | --------------------------------------------------------------- | ----------------------------------------- |
| **Envelope** | корневой элемент, который определяет само сообщение             | Root element — the SOAP message itself    |
| **Header**   | содержит атрибуты сообщения, например информацию о безопасности | Message attributes (security, routing, …) |
| **Body**     | содержит сообщение, которым обмениваются приложения             | Payload exchanged between apps            |
| **Fault**    | необязательный элемент с ошибками обработки, если они были      | Optional processing errors (if any)       |

**When to use:** Legacy enterprise SOAP. New public APIs almost always REST/JSON or RPC/gRPC.

**Pluses and minuses:**

| +                              | −                                         |
| ------------------------------ | ----------------------------------------- |
| Independent of transfer method | Verbose                                   |
| Strict message structure       | Harder on the client than REST            |
| WSDL / WS-\* (enterprise)      | XML loses to JSON; SOAP-over-JSON is rare |

**Examples:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope
  xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <getOrderDetails xmlns="https://example-store.com/orders">
      <orderID>42</orderID>
    </getOrderDetails>
  </soap:Body>
</soap:Envelope>
```

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <getOrderDetailsResponse xmlns="https://example-store.com/orders">
      <getOrderDetailsResult>
        <orderID>42</orderID>
        <userID>43</userID>
        <dateTime>2020-10-10T12:00:00</dateTime>
        <products>
          <productID>1</productID>
          <productID>23</productID>
          <productID>45</productID>
        </products>
      </getOrderDetailsResult>
    </getOrderDetailsResponse>
  </soap:Body>
</soap:Envelope>
```

**Thoughts:** SOAP wraps an operation in XML; REST maps CRUD to HTTP on a resource URL.

**Sources:**

[MDN — SOAP glossary](https://developer.mozilla.org/en-US/docs/Glossary/SOAP) · [Doka — What is an API](https://doka.guide/tools/api/)

---

</details>

<a name="4-rpc"></a>

## 4. RPC

See also: [REST](#rest-principles-and-contract) · [SOAP](#3-soap) · [XML](#xml) · [REST vs SOAP vs RPC](#5-rest-vs-soap-vs-rpc) · [tRPC](#6-trpc)

<details>
  <summary>RPC</summary>

### RPC

**Definition:** RPC (Remote Procedure Call) is a style where the request message stores **both the action to execute and the data that action needs**. On the web that is roughly “call server functionality from the browser.” XML-RPC and JSON-RPC are common; JSON-RPC is easier to read. gRPC is typed RPC over Protobuf + HTTP/2. **tRPC** is TypeScript RPC: you call procedures as functions; types come from the server router, not from OpenAPI/GraphQL codegen.

**How it works:**

| Field    | Request                                       | Response                                 |
| -------- | --------------------------------------------- | ---------------------------------------- |
| `method` | Name of the procedure                         | —                                        |
| `params` | Array of arguments                            | —                                        |
| `id`     | Correlate request ↔ response (parallel calls) | Same `id`                                |
| `result` | —                                             | Return value, or `null` on error         |
| `error`  | —                                             | Error code/message, or `null` on success |

`"jsonrpc": "2.0"`; `params` optional; omit `id` for a notification. Success has **only** `result`. Error has **only** `error` (`code`, `message`). Never both.

|            | JSON-RPC                            | gRPC                      |
| ---------- | ----------------------------------- | ------------------------- |
| Format     | JSON                                | Protocol Buffers          |
| Transport  | HTTP, WebSocket, etc.               | HTTP/2                    |
| Codegen    | Manual                              | Auto from `.proto`        |
| Streaming  | Limited                             | Bidirectional             |
| HTTP cache | Usually unused (POST + method name) | Not REST-cacheable either |

**When to use:**

Internal “call this procedure” APIs, batching. Prefer REST when intermediaries should cache GET. Prefer gRPC for typed internal services. Prefer tRPC when client and server share TypeScript.

#### Common pitfalls

Doka’s sample `"jsonrpc": "2.0"` with both `result` and `"error": null` is **1.0-shaped**. Spec 2.0 uses _either_ `result` _or_ `error`.

**Pluses and minuses:**

| +                                     | −                             |
| ------------------------------------- | ----------------------------- |
| Structured messages                   | Too much logic on the client  |
| JSON is easy to read/write            | HTTP caching is unavailable   |
| Fast when you need **batch** requests | URLs don’t describe resources |

**Examples:**

```json
{
  "jsonrpc": "2.0",
  "method": "orders.get",
  "params": [42],
  "id": 1
}
```

**Example (2.0 success — no** `error` **key):**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "orderId": 42,
    "userId": 43,
    "dateTime": "2020-10-10T12:00:00",
    "products": [{ "productID": 1 }, { "productID": 23 }, { "productID": 45 }]
  },
  "id": 1
}
```

**Example (2.0 error — no** `result` **key):**

```json
{
  "jsonrpc": "2.0",
  "error": { "code": -32000, "message": "Order not found" },
  "id": 1
}
```

**Thoughts:** RPC names the procedure; REST names the resource.

**Sources:**

[JSON-RPC spec](https://www.jsonrpc.org/specification) · [gRPC](https://grpc.io/docs/what-is-grpc/introduction/) · [Doka — What is an API](https://doka.guide/tools/api/)

---

</details>

<a name="5-rest-vs-soap-vs-rpc"></a>

## 5. REST vs SOAP vs RPC

See also: [REST](#rest-principles-and-contract) · [SOAP](#3-soap) · [XML](#xml) · [RPC](#4-rpc) · [tRPC](#6-trpc)

<details>
  <summary>REST vs SOAP vs RPC</summary>

### REST vs SOAP vs RPC

**Definition:** In web work, “API” usually means HTTP to a server. REST, SOAP, and RPC are three different _ways_ to shape that conversation — not three equal protocols. REST is a style; SOAP is a message format; RPC is “call this function remotely.” tRPC is one TypeScript implementation of RPC.

**How it works:**

|                             | REST                                              | SOAP                                      | RPC (JSON-RPC)                              | tRPC                                           |
| --------------------------- | ------------------------------------------------- | ----------------------------------------- | ------------------------------------------- | ---------------------------------------------- |
| **Kind**                    | Architectural style                               | Data-exchange format                      | Remote procedure style                      | TS RPC library                                 |
| **Definition**              | Needed data is in the request (URL + verb + body) | Structured XML envelope                   | Request holds action _and_ data             | Call a typed server procedure as a function    |
| **How you name the action** | HTTP method + resource path                       | Operation XML in Body (`getOrderDetails`) | `method` string (`orders.get`)              | Router path (`orders.get`)                     |
| **Typical wire**            | JSON                                              | XML                                       | JSON (or Protobuf for gRPC)                 | JSON over HTTP (JSON-RPC-shaped envelope)      |
| **Do HTTP verbs matter?**   | Yes (CRUD)                                        | No — transport is not the contract        | Usually one POST                            | Mapped: query → GET, mutation → POST           |
| **HTTP cache**              | GET can be cached                                 | Weak                                      | Weak / unavailable                          | Query GET _can_ be cached; often batched POST  |
| **Types**                   | Hand-written / OpenAPI                            | WSDL                                      | Manual or `.proto` (gRPC)                   | Infer from `AppRouter` — no codegen            |
| **Pros**                    | Common, HTTP-native, readable                     | Transport-agnostic, structured            | Structure, JSON, good batches               | End-to-end TS, autocomplete, batching          |
| **Cons**                    | Over/under-fetch (BFF helps)                      | Verbose, awkward on the client            | Logic on the client; no HTTP cache          | TS-to-TS; not a public resource API by default |
| **When to use**             | Public CRUD, web/mobile                           | Legacy enterprise / WSDL                  | Internal procedures, batches, gRPC services | TS fullstack / monorepo                        |

```http
GET /api/orders/42
```

```xml
<soap:Body><getOrderDetails><orderID>42</orderID></getOrderDetails></soap:Body>
```

```json
{ "jsonrpc": "2.0", "method": "orders.get", "params": [42], "id": 1 }
```

```http
GET /api/trpc/orders.get?input=%7B%22id%22%3A42%7D
```

```ts
const order = await client.orders.get.query({ id: 42 });
```

**When to use:** Interview: “REST is not SOAP’s opposite.” Pick REST for public HTTP; SOAP when the partner already speaks SOAP; RPC/gRPC for internal typed calls; tRPC when both sides are TypeScript.

**Thoughts:** REST = resource + verb. SOAP = envelope. RPC = method + params. tRPC = typed RPC functions over HTTP.

**Sources:**

[Doka — What is an API](https://doka.guide/tools/api/) · [tRPC — concepts](https://trpc.io/docs/concepts) · [tRPC HTTP RPC spec](https://trpc.io/docs/rpc)

---

</details>

<a name="6-trpc"></a>

## 6. tRPC

See also: [RPC](#4-rpc) · [REST vs SOAP vs RPC](#5-rest-vs-soap-vs-rpc)

<details>
  <summary>tRPC</summary>

### tRPC

**Definition:** tRPC (TypeScript Remote Procedure Call) is RPC for TypeScript: you define server procedures and call them from the client with inferred input/output types — no OpenAPI or GraphQL schema and no codegen. Docs mindset: “it’s just functions,” not `GET /users/:id`.

**How it works:**

- Server: `initTRPC` → `router` → procedures: `.query()`, `.mutation()`, `.subscription()`. Export `type AppRouter = typeof appRouter`.
- Client: `createTRPCClient<AppRouter>` (often `httpBatchLink`). TypeScript infers the tree; a renamed procedure breaks the client at compile time.
- One HTTP endpoint (e.g. `/api/trpc`); tRPC routes internally. Nested procedures use dots: `post.byId` → `/api/trpc/post.byId`.
- HTTP mapping (default): **query → GET** with JSON input in `?input=`; **mutation → POST** with JSON body. Subscriptions: SSE (`httpSubscriptionLink`) or WebSocket (`wsLink`).
- Envelopes follow **JSON-RPC 2.0** where possible (`result.data` vs `error` with JSON-RPC-style codes).
- **Batching** (`httpBatchLink`): same-method parallel calls become one request — path `postById,relatedPosts`, `?batch=1&input={...}`; mixed success/error can return `207 Multi-Status`.
- Can sit next to REST: REST keeps resource URLs; tRPC owns one RPC endpoint.

**When to use:**

Next.js / Node fullstack in a TS monorepo. Prefer REST/OpenAPI when unknown clients, CDNs keyed on resource URLs, or non-TS consumers matter. Prefer gRPC when you need binary + HTTP/2 streaming across languages.

#### Common pitfalls

Calling `fetch('/api/trpc/users/123')` like REST — use `client.user.byId.query({ id: '123' })` (wire: `GET /api/trpc/user.byId?input=...`). Sharing only runtime code without the `AppRouter` type. Treating GET/POST as REST CRUD — verbs are transport; intent is in the procedure name (`getUser` vs `GET /users/:id`). Huge query inputs on GET (use `methodOverride: 'POST'` if the server allows it).

**Pluses and minuses:**

| +                                                        | −                                                                   |
| -------------------------------------------------------- | ------------------------------------------------------------------- |
| End-to-end types, no codegen                             | Client and server must share TypeScript (`AppRouter`)               |
| Autocomplete; breaking API changes show up in the client | Poor default for public polyglot APIs (need REST/OpenAPI)           |
| Query GET + mutation POST; optional batching             | Query input in the URL — size limits; cache is not REST-by-resource |
| Adapters: fetch, Next.js `/api/trpc/*`, standalone       | Inspecting the wire still looks like RPC, not REST resources        |

**Examples:**

```ts
import { initTRPC } from "@trpc/server";
import { z } from "zod";

const t = initTRPC.create();

const appRouter = t.router({
  post: t.router({
    byId: t.procedure.input(z.string()).query((opts) => {
      return { id: opts.input, title: "tRPC is the best!" };
    }),
    create: t.procedure
      .input(z.object({ title: z.string() }))
      .mutation((opts) => ({ id: "1", ...opts.input })),
  }),
});

export type AppRouter = typeof appRouter;
```

```ts
import { createTRPCClient, httpBatchLink } from "@trpc/client";
import type { AppRouter } from "./server";

const client = createTRPCClient<AppRouter>({
  links: [httpBatchLink({ url: "http://localhost:3000/api/trpc" })],
});

const post = await client.post.byId.query("1");
await client.post.create.mutate({ title: "Hello" });
```

**Thoughts:** tRPC is typed TypeScript RPC over HTTP, not REST with extra types.

**Sources:**

[tRPC introduction](https://trpc.io/docs) · [tRPC — concepts](https://trpc.io/docs/concepts) · [tRPC HTTP RPC spec](https://trpc.io/docs/rpc)

---

</details>

<a name="7-http"></a>
