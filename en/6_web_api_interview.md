# Web APIs — Interview

Interview Q&A for HTTP and Web APIs.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Performance](./6_web_api_performance.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [Interview Practice](#interview-practice)
- [Questions & Answers](#questions-answers)

---

## Interview Practice

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Questions from [0_interview.md](0_interview.md). Try answering before opening solutions.

### Question index

| Topic                                       | Answer |
| ------------------------------------------- | ------ |
| OSI vs TCP/IP layers                        | Q&A    |
| LAN vs Internet / packets / how to inspect  | Q&A    |
| TCP vs UDP                                  | Q&A    |
| IPv4 vs IPv6 / AAAA / Happy Eyeballs        | Q&A    |
| URL anatomy (scheme, query, hash)           | Q&A    |
| What is an API? REST vs SOAP vs RPC (tRPC)  | Q&A    |
| XML vs HTML vs JSON                         | Q&A    |
| HTTP methods + idempotency                  | Q&A    |
| Status codes                                | Q&A    |
| REST versioning & pagination                | Q&A    |
| GET vs POST                                 | Q&A    |
| What happens on `fetch`                     | Q&A    |
| Caching headers                             | Q&A    |
| CORS                                        | Q&A    |
| OAuth2 vs OIDC                              | Q&A    |
| JWT vs session                              | Q&A    |
| CSRF mitigation                             | Q&A    |
| Clickjacking / CSP                          | Q&A    |
| Rate limiting (429)                         | Q&A    |
| Fetch AbortController                       | Q&A    |
| WebSocket vs polling                        | Q&A    |
| Client–server / why a backend               | Q&A    |
| Types of web apps (MPA/SPA/CSR/SSR/SSG/PWA) | Q&A    |
| What is a PWA?                              | Q&A    |
| What is FCP?                                | Q&A    |
| What is LCP?                                | Q&A    |
| Payments / webhooks                         | Q&A    |
| INP vs FID                                  | Q&A    |
| TTFB                                        | Q&A    |
| Core Web Vitals (lab vs field)              | Q&A    |
| API & client performance optimization       | Q&A    |

---

</details>

## Questions & Answers

<details>
  <summary>OSI vs TCP/IP layers?</summary>

### OSI vs TCP/IP layers?

**Definition:** Both models describe how data moves across networks. **OSI** has 7 layers; the practical **TCP/IP** model has 4. Full notes: [OSI & TCP/IP models](./6_web_api_apps.md#osi--tcpip-models).

**How it works:**

| OSI (7)                            | TCP/IP (4)      | Examples                  |
| ---------------------------------- | --------------- | ------------------------- |
| Application, Presentation, Session | **Application** | HTTP, DNS, WebSocket, TLS |
| Transport                          | **Transport**   | TCP, UDP                  |
| Network                            | **Internet**    | IP, routing               |
| Data Link, Physical                | **Link**        | Ethernet, Wi-Fi           |

HTTP sits at the **application layer** — it assumes TCP (reliable delivery) and IP (routing) below it.

**When to use:** Interview answer: "HTTP is application layer on top of TCP." TLS encrypts at presentation/session in OSI terms but is usually drawn between HTTP and TCP.

**Sources:** [OSI & TCP/IP models](./6_web_api_apps.md#osi--tcpip-models) · [MDN — How the Internet works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work)

---

</details>

<details>
  <summary>LAN vs Internet — how do computers exchange data?</summary>

### LAN vs Internet — how do computers exchange data?

**Definition:** Same TCP/IP packets on a home LAN and on the public Internet. The LAN uses private addresses and a local gateway; the Internet is globally routed public IPs. DNS maps a name to an IP; TCP/TLS then HTTP talk to that IP.

**How it works:** File or page → split into packets → each packet is forwarded toward the dest IP → the dest kernel demuxes by **port** to a service → bytes are reassembled. Inspect with `ping`, `ip addr`/`ifconfig`, `ss`/`netstat`; capture with tcpdump/Wireshark. Full notes: [How the Internet works](./6_web_api_core.md#internet) · [Ports, sockets & network services](./6_web_api_apps.md#ports-sockets--network-services).

**When to use:** Interview: “What happens when I type a URL?” Answer DNS → TCP → TLS → HTTP, not “packets go to DNS and DNS rewrites them.”

**Thoughts:** _Addressing and NAT differ; the packet model does not._

**Sources:** [How the Internet works](./6_web_api_core.md#internet)

---

</details>

<details>
  <summary>TCP vs UDP?</summary>

### TCP vs UDP?

**Definition:** Transport-layer choice: TCP = reliable ordered stream after a 3-way handshake; UDP = connectionless datagrams.

**Interview answer:** HTTP/1.1 and HTTP/2 use TCP (integrity of HTML/CSS/JS). Streaming, DNS, WebRTC often use UDP (latency). HTTP/3 is QUIC **on UDP** but still reliable. Handshake cost → `preconnect` / reuse connections. Full block: [TCP vs UDP](#tcp-vs-udp).

---

</details>

<details>
  <summary>IPv4 vs IPv6 / AAAA / Happy Eyeballs?</summary>

### IPv4 vs IPv6 / AAAA / Happy Eyeballs?

**Definition:** IPv4 is 32-bit (`192.0.2.1`); IPv6 is 128-bit hextets (`2001:db8::1`). DNS `A` vs `AAAA`. Dual-stack browsers use Happy Eyeballs so IPv6 is tried without hanging if it is broken.

**How it works:** Compress zeros with `::` once. In URLs wrap literals: `https://[2001:db8::1]:443/`. Prefix `/64` is one LAN. Loopback `::1`. Docs: `2001:db8::/32`, `3fff::/20`. Public GUA is in `2000::/3`. Enable a site: listen on IPv6 + static address + AAAA.

**When to use:** “Why WebRTC needs TURN on IPv4” (NAT); “why mobile users hit IPv6”; “why a bad AAAA hurts IPv6-only networks.”

**Thoughts:** Dual-stack = A + AAAA + a server that actually answers on both.

**Sources:** [IPv4 & IPv6](./6_web_api_apps.md#ipv4--ipv6)

---

</details>

<details>
  <summary>URL anatomy (scheme, query, hash)?</summary>

### URL anatomy (scheme, query, hash)?

**Definition:** A URL locates a resource and names the access method. Shape: `scheme://[userinfo@]host[:port]/path?query#fragment`.

**How it works:** Scheme selects the protocol (`https`, `mailto`). Host is looked up by DNS. Default ports (80 / 443) are omitted. `?` starts query pairs (`&`-separated). `#` is a client fragment — not on the HTTP request. Relative URLs inherit the current origin.

**When to use:** Address bar; `<a href>`; REST paths vs `?page=2`; in-page `#id`.

**Thoughts:** Query is extra input to the server; hash never leaves the browser.

**Sources:** [URI, URL & URN](./6_web_api_core.md#uri-url--urn)

---

</details>

<details>
  <summary>What is an API? REST vs SOAP vs RPC?</summary>

### What is an API? REST vs SOAP vs RPC?

**Definition:** An API is the set of features one program exposes to others. In web backends the usual styles are REST, SOAP, and RPC. tRPC is TypeScript RPC.

| Style | Definition                                                                  | Pros                           | Cons                                   |
| ----- | --------------------------------------------------------------------------- | ------------------------------ | -------------------------------------- |
| REST  | Communication style: needed data is in the request (URL + HTTP verb + body) | Common, HTTP-native, readable  | Over/under-fetch (BFF helps)           |
| SOAP  | Structured data-exchange format (XML envelope)                              | Transport-agnostic, structured | Verbose, worse on the client than REST |
| RPC   | Request holds the action _and_ its data (`method` + `params` + `id`)        | Structure, JSON, good batches  | Logic on client; no HTTP cache         |
| tRPC  | TS procedures inferred from `AppRouter`; query GET / mutation POST          | End-to-end types, no codegen   | TS-to-TS; not a public resource API    |

**When to use:** REST for public CRUD and caching. SOAP for legacy enterprise. JSON-RPC when you want procedure calls / batches. tRPC when client and server share TypeScript.

**Thoughts:** Full compare: [REST vs SOAP vs RPC](./6_web_api_rest.md#5-rest-vs-soap-vs-rpc) · [tRPC](./6_web_api_rest.md#6-trpc) · [XML](./6_web_api_rest.md#xml). Don’t treat SOAP as “the opposite of REST.” JSON-RPC 2.0 success/error are mutually exclusive. tRPC is RPC, not REST — procedure names carry intent.

**Sources:** [Doka — What is an API](https://doka.guide/tools/api/) · [JSON-RPC spec](https://www.jsonrpc.org/specification) · [tRPC — concepts](https://trpc.io/docs/concepts)

---

</details>

<details>
  <summary>XML vs HTML vs JSON?</summary>

### XML vs HTML vs JSON?

**Definition:** XML is a markup _syntax_ you invent tags for; HTML is a fixed vocabulary for pages; JSON is a typed data format. SOAP _uses_ XML; REST usually uses JSON.

**How it works:** XML needs one root, closed tags, quoted attributes, optional `<?xml …?>` at the top. Namespaces (`xmlns`) split the same local name into different vocabularies. HTML parsers forgive errors; XML and JSON do not.

**When to use:** XML for SOAP, RSS, SVG files, legacy enterprise. JSON for new APIs. HTML for documents in the browser.

**Thoughts:** Same tree-shaped markup as HTML, but for data — not for rendering.

**Sources:** [XML](./6_web_api_rest.md#xml)

---

</details>

<details>
  <summary>HTTP methods and idempotency?</summary>

### HTTP methods and idempotency?

**Definition:** HTTP methods express intent. **Safe** methods do not change server state; **idempotent** methods produce the same effect when repeated.

| Method  | Idempotent | Safe | Typical use                  |
| ------- | ---------- | ---- | ---------------------------- |
| GET     | yes        | yes  | Read resource                |
| HEAD    | yes        | yes  | Headers only                 |
| OPTIONS | yes        | yes  | CORS preflight, capabilities |
| PUT     | yes        | no   | Full replace                 |
| DELETE  | yes        | no   | Remove (2nd call may 404)    |
| POST    | no         | no   | Create, actions              |
| PATCH   | no\*       | no   | Partial update               |

PATCH idempotency depends on server implementation. `TRACE` is safe and idempotent (loop-back); `CONNECT` is neither (tunnel). Full list: [HTTP protocol](./6_web_api_http.md#http-protocol).

**How it works:** Repeating `PUT /users/1` with the same body leaves the same state. Repeating `POST /users` may create multiple users. Retries on network failure should use idempotent methods or **idempotency keys** on POST.

**When to use:** Use PUT for upsert-by-ID; POST for create; DELETE safe to retry if server treats missing resource as success (204/404).

**Sources:** [MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) · [MDN — Idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)

---

</details>

<details>
  <summary>What is a web server?</summary>

### What is a web server?

**Definition:** A program that accepts HTTP requests, handles them (file, cache, proxy, or app), and returns HTTP responses.

**How it works:** HTTP/1.1 messages = start-line + headers + blank line + optional body. Production servers add modules: TLS, gzip/brotli, reverse proxy, cache. Nginx/Apache/Caddy often sit in front of Node.

**When to use:** `npx serve` / `http-server` for local static files; Nginx for TLS + static + `/api/` proxy.

**Thoughts:** Full block: [Web server](./6_web_api_http.md#web-server) · Compression: [Content-Encoding](./6_web_api_http.md#http-compression-content-encoding) · TLS: [HTTPS handshake](./6_web_api_http.md#ssltls--https-handshake).

**Sources:** [MDN — HTTP messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Messages) · [Doka — Web server](https://doka.guide/tools/web-server/)

---

</details>

<details>
  <summary>Common HTTP status codes?</summary>

### Common HTTP status codes?

**Definition:** Three-digit codes tell the client outcome class and how to proceed.

| Code                                        | Meaning                       | When                                  |
| ------------------------------------------- | ----------------------------- | ------------------------------------- |
| **200** OK                                  | Success with body             | GET, PUT, PATCH success               |
| **201** Created                             | Resource created              | POST success + `Location` header      |
| **204** No Content                          | Success, empty body           | DELETE success                        |
| **301** Moved Permanently                   | URL changed forever           | SEO redirects                         |
| **302/307** Temporary redirect              | Temporary move                | Login redirect, POST→GET              |
| **304** Not Modified                        | Cached copy still valid       | `If-None-Match` / `If-Modified-Since` |
| **400** Bad Request                         | Malformed client input        | Invalid JSON                          |
| **401** Unauthorized                        | Auth required or failed       | Missing/invalid token                 |
| **403** Forbidden                           | Authenticated but not allowed | Role check failed                     |
| **404** Not Found                           | Resource does not exist       | Wrong ID                              |
| **409** Conflict                            | State conflict                | Duplicate email                       |
| **422** Unprocessable                       | Validation failed             | Schema errors                         |
| **429** Too Many Requests                   | Rate limited                  | See rate limiting section             |
| **500** Internal Server Error               | Server bug                    | Unhandled exception                   |
| **502/503/504** Gateway/Unavailable/Timeout | Upstream/down/overloaded      | Load balancer, deploy                 |

**When to use:** Return **401** when credentials missing/invalid; **403** when user is known but lacks permission. Use **201** + `Location` on create.

**Sources:** [MDN — HTTP status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

---

</details>

<details>
  <summary>REST versioning & pagination?</summary>

### REST versioning & pagination?

See **REST → REST versioning & pagination** for full notes.

**Short answer:** Version via URL path (`/v2/`) or `Accept` header. Paginate with **cursor** for feeds (efficient) or **offset** for page numbers (simple). Return `nextCursor` / `Link` headers and bound `limit` (max 100).

**Sources:** [RFC 8288 — Web Linking](https://datatracker.ietf.org/doc/html/rfc8288)

---

</details>

<details>
  <summary>Cache-Control headers?</summary>

### Cache-Control headers?

**Definition:** `Cache-Control` and validators (`ETag`, `Last-Modified`) control browser and CDN caching.

**How it works:**

| Directive      | Effect                                 |
| -------------- | -------------------------------------- |
| `max-age=3600` | Fresh for 3600 seconds                 |
| `no-cache`     | Must revalidate with server before use |
| `no-store`     | Do not store at all (sensitive data)   |
| `private`      | Browser only, not shared CDN           |
| `public`       | CDN may cache                          |
| `immutable`    | Never revalidate (hashed filenames)    |

Conditional request: client sends `If-None-Match: "etag"` → server returns **304** with empty body if unchanged.

**When to use:** Static assets with hash in filename → `max-age=31536000, immutable`. API user data → `private, no-store` or short `max-age`. App-level cache (RTK Query) is separate — see [HTTP caching](./6_web_api_rest.md#7-http).

**Thoughts:** `max-age` _is freshness;_ `no-cache` _means revalidate;_ `no-store` _means do not write a cache._

**Sources:** [MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

---

</details>

<details>
  <summary>CORS — how does it work?</summary>

### CORS — how does it work?

**Definition:** Browsers enforce the **Same-Origin Policy** — JS cannot read cross-origin responses unless the server opts in via CORS headers.

**How it works:**

1. Same origin → no CORS check. Cross-origin → browser sends `Origin`.
2. Simple request goes to the server; JS reads the body only if `Access-Control-Allow-Origin` matches.
3. Non-simple → `OPTIONS` preflight (`Access-Control-Request-Method` / `Headers`); then the real request.
4. Credentials need `Allow-Credentials: true` and a **concrete** origin — not `*`.

**When to use:** SPA + API on another origin; curl/Postman have **no** CORS. Full headers and pitfalls → [CORS](./6_web_api_security.md#cors).

**Thoughts:** _The server often still receives the request; the browser hides the response from page JS without matching_ `Access-Control-*` _headers._

| Pros                                             | Cons                                    |
| ------------------------------------------------ | --------------------------------------- |
| Protects users from malicious sites reading data | Misconfiguration blocks legitimate SPAs |
| Explicit server consent                          | Easy to over-allow `*` in dev           |

**Sources:** [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

---

</details>

<details>
  <summary>OAuth2 vs OIDC?</summary>

### OAuth2 vs OIDC?

**Definition:** OAuth2 delegates **authorization** (access to resources). OIDC adds **authentication** (who logged in) via `id_token`.

**How it works:** Authorization Code + PKCE for SPAs → exchange code for `access_token` (API) + `id_token` (identity JWT with `sub`, `email`).

**When to use:** "Sign in with Google" = OIDC. "App reads my calendar" = OAuth2 scopes. Never use Implicit flow for new apps.

**Sources:** [OpenID Connect](https://openid.net/connect/)

---

</details>

<details>
  <summary>JWT vs session cookie?</summary>

### JWT vs session cookie?

**Definition:** Sessions keep state on the server; JWTs are self-contained signed tokens verified without a lookup.

|            | Session                    | JWT                              |
| ---------- | -------------------------- | -------------------------------- |
| Storage    | Server (Redis/DB)          | Client (cookie/header)           |
| Revocation | Delete session — instant   | Blocklist or short TTL + refresh |
| Scale      | Needs shared session store | Stateless verification           |
| XSS risk   | HttpOnly cookie safer      | localStorage is dangerous        |

**How it works:** Session — cookie holds opaque ID; server loads user. JWT — server verifies signature and `exp` on each request.

**When to use:** Session for monolith web apps. JWT for microservices and mobile. Prefer **HttpOnly cookie** for either when possible.

**Sources:** [jwt.io](https://jwt.io/) · [OWASP — Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

---

</details>

<details>
  <summary>CSRF — how to mitigate?</summary>

### CSRF — how to mitigate?

**Definition:** CSRF forges authenticated requests using the victim's cookies. AuthN (valid session) is not the same as authorizing this mutation.

**Mitigations:** CSRF token (synchronizer token), `SameSite=Lax/Strict` cookies, require custom headers for API mutations, never change state on GET. `HttpOnly` does not stop CSRF.

**When to use:** Cookie session + form POST → token required. JWT in `Authorization` header only → not CSRF-vulnerable.

**Sources:** [OWASP — CSRF](https://owasp.org/www-community/attacks/csrf)

---

</details>

<details>
  <summary>Clickjacking — how to mitigate?</summary>

### Clickjacking — how to mitigate?

**Definition:** User clicks a decoy while a framed copy of your site receives the click.

**Mitigations:** CSP `frame-ancestors 'none'` or `'self'`; `X-Frame-Options: DENY` or `SAMEORIGIN`. Do not use `ALLOW-FROM` (obsolete). XFO is HTTP-header only.

**Sources:** [MDN — X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)

---

</details>

<details>
  <summary>Rate limiting — what is 429?</summary>

### Rate limiting — what is 429?

**Definition:** **429 Too Many Requests** when client exceeds allowed rate. Server should send `**Retry-After`\*\*.

**How it works:** Counter per IP/API key in Redis or edge proxy. Client backs off exponentially with jitter.

**Sources:** [MDN — 429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)

---

</details>

<details>
  <summary>AbortController with fetch?</summary>

### AbortController with fetch?

**Definition:** `AbortController` cancels in-flight `fetch` — useful for timeouts, debounced search, and React cleanup.

**Examples:**

```js
const ac = new AbortController();
fetch("/api/search?q=ab", { signal: ac.signal });
ac.abort(); // throws AbortError in catch
```

**Sources:** [MDN — AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

---

</details>

<details>
  <summary>GET vs POST?</summary>

### GET vs POST?

**Definition:** HTTP methods express intent — GET reads resources; POST creates or triggers actions.

|                | GET                            | POST                              |
| -------------- | ------------------------------ | --------------------------------- |
| **Purpose**    | Retrieve data                  | Create resource / submit action   |
| **Safe**       | Yes (no server state change)   | No                                |
| **Idempotent** | Yes                            | No (repeat may create duplicates) |
| **Body**       | Typically none (params in URL) | Often JSON/form body              |
| **Cacheable**  | Yes (by default)               | No                                |
| **Example**    | `GET /api/users/1`             | `POST /api/users`                 |

**Thoughts:** _GET = read (bookmarkable, cacheable); POST = write/action (not idempotent by default)._

**See also:** [CRUD & REST mapping](./6_web_api_rest.md#rest-principles-and-contract) · [PUT vs POST](./6_web_api_http.md#put-vs-post)

**Sources:** [MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

---

</details>

<details>
  <summary>What happens when the frontend calls `fetch`?</summary>

### What happens when the frontend calls `fetch`?

**Definition:** `fetch(url, options)` initiates an HTTP request from the browser and returns a `Promise<Response>`.

**How it works:**

```
1. JS calls fetch('/api/users')
2. Browser resolves URL → DNS → TCP/TLS connection
3. HTTP request sent (method, headers, optional body)
4. Server processes → HTTP response (status, headers, body stream)
5. Promise resolves with Response (4xx/5xx do NOT reject — check res.ok)
6. res.json() parses body
7. Browser checks CORS — blocks JS reading cross-origin response without Access-Control-Allow-Origin
```

**Frontend ↔ API contract:** Client expects JSON shape; server must match status codes and `Content-Type`.

**Thoughts:** _Network errors reject; HTTP errors don't — always check_ `res.ok` _and validate JSON shape._

**See also:** [Fetch API](#fetch-api) · [CORS](./6_web_api_security.md#cors)

**Sources:** [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

---

</details>

<details>
  <summary>WebSocket vs polling?</summary>

### WebSocket vs polling?

**Definition:** Different patterns for server→client or bidirectional updates.

| Pattern       | Pros                     | Cons                           |
| ------------- | ------------------------ | ------------------------------ |
| Short polling | Simple                   | Wasteful, high latency         |
| Long polling  | Near real-time           | Connection overhead per wait   |
| SSE           | Server push over HTTP    | One-way only                   |
| WebSocket     | Full duplex, low latency | Connection management, proxies |

**When to use:** Chat/games → WebSocket. Live notifications read-only → SSE. Rare updates → polling is fine.

**Sources:** [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [MDN — SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)

---

</details>

<details>
  <summary>Client–server — why a backend?</summary>

### Client–server — why a backend?

**Definition:** Client (browser) requests; server applies business logic and uses a DB. The UI is HTML/CSS/JS; shared durable data needs a backend.

**How it works:** Classic MPA reloaded the whole page. AJAX first returned HTML fragments. Modern APIs return JSON; the client paints. HTTP verbs + resource URLs = REST. Payload bloat → BFF/GraphQL.

**See:** [Client–server architecture](./6_web_api_core.md#client-server-architecture) · [Evolution of web apps](#evolution-of-web-apps) · [0_system_design.md](./0_system_design_core.md#clientserver-architecture-why-a-backend)

**Sources:** [Doka — How web apps work](https://doka.guide/tools/web-app-works/)

---

</details>

<details>
  <summary>Types of web apps (MPA vs SPA vs CSR/SSR/SSG)?</summary>

### Types of web apps (MPA vs SPA vs CSR/SSR/SSG)?

**Definition:** MPA = many HTML pages, full reload on links. SPA = one page, JS owns screens. CSR/SSR/SSG = **when** the first HTML is built. PWA adds install/offline (manifest + service worker).

|             | When HTML exists            | Navigation             |
| ----------- | --------------------------- | ---------------------- |
| MPA static  | Files on disk               | Full reload            |
| MPA dynamic | Server template per request | Full reload            |
| CSR         | After JS in the browser     | Instant                |
| SSR         | Server HTML, then hydrate   | Instant after boot     |
| SSG         | Build time                  | Reload or hydrated SPA |

**Thoughts:** Do not treat PWA as a separate architecture. Install vs offline: [PWA](./6_web_api_apps.md#pwa-progressive-web-app). Taxonomy: [Types of web apps](./6_web_api_apps.md#12-types-of-web-apps).

**Sources:** [Doka — Types of web apps](https://doka.guide/tools/web-app-types/)

---

</details>

<details>
  <summary>What is a PWA?</summary>

### What is a PWA?

**Definition:** Three answers: (1) **Everyday** — installable site that runs standalone like a native app. (2) **Official** — progressive enhancement of Web APIs for native-class UX. (3) **Technical** — meets that browser’s install rules (they differ).

**How it works:** Chromium install prompt: HTTPS/`localhost` + web app manifest (`name`/`short_name`, icons, `start_url`, `display`). **Service Worker is not required to install** (it used to be). SW is still required for offline/push/background sync. Safari/Firefox usually skip the Chromium install UI; Apple says **Home Screen Web Apps**.

**Pros (interview):** one codebase; home-screen + offline + push; no store required; small (browser APIs); SEO/URLs; sandbox + HTTPS + permission prompts; HTML/CSS/JS.

**Cons:** no big PWA catalog; fragmented APIs; iOS WebKit in every browser.

**Thoughts:** Install/offline are not mandatory for the _official_ definition. See [PWA](./6_web_api_apps.md#pwa-progressive-web-app).

**Sources:** [Doka — PWA](https://doka.guide/tools/pwa/) · [web.dev — install criteria](https://web.dev/articles/install-criteria)

---

</details>

<details>
  <summary>What is FCP?</summary>

### What is FCP?

**Definition:** First Contentful Paint — time until the first text, image, SVG, or non-empty canvas appears. Blank screen ended, not “ready.”

**How it works:** Supporting Web Vital (not Core, not ranking). Good ≤ 1.8 s p75; poor > 3 s. Slow FCP usually implies slow LCP (shared TTFB + blocking CSS). INP can still be late.

**Thoughts:** Fix first paint with TTFB + render-blocking resources; still measure LCP and INP.

**See also:** [First Contentful Paint (FCP)](./1_html_performance.md#first-contentful-paint-fcp)

---

</details>

<details>
  <summary>What is LCP?</summary>

### What is LCP?

**Definition:** Largest Contentful Paint — time until the largest viewport content (usually hero image or big text) is painted. Browser chooses the element.

**How it works:** Core Web Vital. Good ≤ 2.5 s p75; poor > 4 s. Four slices: TTFB, resource load delay, resource load duration, element render delay. Do not lazy-load the hero; use `fetchpriority="high"` on 1–2 resources.

**Thoughts:** Discover the LCP resource in HTML early; then cut TTFB, bytes, and render-blocking CSS/JS.

**See also:** [LCP (Largest Contentful Paint)](./1_html_performance.md#lcp-largest-contentful-paint)

---

</details>

<details>
  <summary>INP vs FID?</summary>

### INP vs FID?

**Definition:** **INP** is the Core Web Vital for responsiveness (since **12 Mar 2024**). **FID** only measured the **first** interaction’s wait until the handler _started_ (not handler work or next paint).

**How it works:** INP = click/tap/key → next paint for (almost) the worst interaction on the visit (one outlier dropped per 50). Phases: input delay + processing + presentation. Good ≤ 200 ms p75; poor > 500 ms. Lighthouse without a user reports **TBT**, not INP.

**Thoughts:** Cut main-thread JS; show UI first, then yield (`scheduler.yield()` + `setTimeout` fallback).

**See also:** [INP (Interaction to Next Paint)](./1_html_performance.md#inp-interaction-to-next-paint)

---

</details>

<details>
  <summary>How do you take a payment without storing card numbers?</summary>

### How do you take a payment without storing card numbers?

**Definition:** The vendor (Stripe) tokenizes the card in their iframe. Your server never sees PAN. Truth is the **signed webhook**, not the browser callback.

**How it works:** Checkout Session → customer pays → `checkout.session.completed` webhook with `Stripe-Signature` → verify → mark order paid. Use idempotency keys on create.

**Thoughts:** Client SDK for UX; webhook for the ledger.

**See also:** [Payments, analytics & third-party SDKs](./6_web_api_security.md#15-integrations)

---

</details>

<details>
  <summary>Lab Lighthouse is green, Search Console is red — why?</summary>

### Lab Lighthouse is green, Search Console is red — why?

**Definition:** Lighthouse is **lab** (one synthetic run). Search Console Core Web Vitals is **field** CrUX at **p75** over **28 days**. Ranking uses field, not Lighthouse.

**How it works:** Default Lighthouse has no user → **INP** is missing; **TBT** is only a proxy. Field **CLS** includes scroll and late shifts. Field **LCP** uses real viewports and warm caches. After a fix, wait for the rolling window. Debug in DevTools; ship based on RUM/`web-vitals`.

**Thoughts:** Lab = reproduce. Field p75 = users and ranking.

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals)

---

</details>

<details>
  <summary>What is TTFB and why does it matter?</summary>

### What is TTFB and why does it matter?

**Definition:** Time from navigation start to the first response byte. Diagnostic, not a Core Web Vital. Good ≤ 0.8 s; poor > 1.8 s.

**How it works:** Redirects + SW start + DNS + TLS + origin until first byte. No first byte → nothing to parse → FCP/LCP wait. Low TTFB still does not guarantee a good LCP.

**Thoughts:** Fix network (CDN, fewer redirects) and origin (stream/SSG/ISR, `Server-Timing`). Then optimize paint.

**See also:** [TTFB](./1_html_performance.md#ttfb-time-to-first-byte)

---

</details>

<details>
  <summary>How do you optimize API and client performance?</summary>

### How do you optimize API and client performance?

**Answer:** Measure first (RUM, APM/traces, Lighthouse, `Server-Timing`), then fix the slowest layer on the user path. **API:** pagination, indexes, kill N+1, cache (HTTP/Redis/CDN), compression, async for heavy work, fewer round trips (BFF/batch). **Client:** smaller bundles (split/lazy), less main-thread work (memo, virtualization, defer third-party), Core Web Vitals (LCP images, INP long tasks, CLS layout). Re-check p95/p99 and field vitals after each change.

**See also:** [API and client performance optimization](./6_web_api_performance.md#api-and-client-performance-optimization) · [§10 Performance](./4_react_ecosystem.md#10-performance) · [TTFB](./1_html_performance.md#ttfb-time-to-first-byte)

---

</details>

---

## Coding tasks

<details>
  <summary>Task 1 — Design REST API for books</summary>

### Task 1 — Design REST API for books

List endpoints for CRUD on `/books` with methods and status codes.

Outline

`GET /books` 200, `GET /books/:id` 200/404, `POST /books` 201, `PUT /books/:id` 200/404, `DELETE /books/:id` 204/404

---

</details>

<details>
  <summary>Task 2 — Read Cache-Control scenario</summary>

### Task 2 — Read Cache-Control scenario

Response has `Cache-Control: max-age=3600, public`. When will browser re-fetch?

Answer

May serve from cache without revalidation for 3600 seconds; shared caches (CDN) allowed.

---

</details>

<details>
  <summary>Coding tasks (legacy)</summary>

### Coding tasks (legacy)

// 12 What is REST? What request methods do we have? What are status codes?

---

</details>

<details>
  <summary>Junior — General (HTTP / REST)</summary>

### Junior — General (HTTP / REST)

1. Describe the testing pyramid.
2. What types of automated tests have you written? Which libraries and tools do you prefer?
3. What are unit tests? Where do they sit in the testing pyramid?
4. What is code coverage? Is 100% coverage required?
5. How do you prevent the browser from serving cached HTTP responses?
6. What is XSS (Cross-Site Scripting)?

---

</details>

<details>
  <summary>Middle — General</summary>

### Middle — General

1. Describe the testing pyramid.
2. What types of automated tests have you written? Which libraries and tools do you prefer?
3. What are unit tests? Where do they sit in the testing pyramid?
4. What is code coverage? Is 100% coverage required?
5. How do you prevent the browser from serving cached HTTP responses?
6. What is XSS (Cross-Site Scripting)?
7. Explain Observer vs Pub/Sub — differences and browser/framework examples.
8. What is the purpose of `self.addEventListener('fetch', …)` in a service worker?
9. What is the event loop? Explain microtasks vs macrotasks.

---

</details>

<details>
  <summary>General</summary>

### General

1. Explain functional programming concepts.
2. What is TDD / BDD?
3. Explain how HTTPS works in detail. What is in a TLS certificate, and how does the CA signature prove the server’s public key?
4. What tech stack would you choose to clone a well-known project and why?
5. How do you maintain a legacy codebase while shipping changes?
6. If you know several frameworks — which would you pick for the next project and why?
7. What is the V8 engine?

---

</details>
 frameworks — which would you pick for the next project and why?
7. What is the V8 engine?

---

</details>
