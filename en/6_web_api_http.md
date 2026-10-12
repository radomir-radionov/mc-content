# Web APIs — HTTP

HTTP, HTTPS, TLS, caching, CORS, and URL anatomy.

[← Back to hub](./6_web_api.md)

### Related docs

* [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [7. HTTP](#7-http)

---

## 7. HTTP

<details>
  <summary>HTTP protocol</summary>

### HTTP protocol

**Definition:** HTTP is an application-layer request/response protocol: a **client** (user-agent, usually a browser) sends messages; a **server** answers. It carries HTML, CSS, JS, JSON APIs, images, media, form data, and more.

**How it works:**

- Three HTTP-layer roles: **client** (sends requests, processes responses), **origin server** (handles them), **proxy** (relay that may cache, filter, load-balance, authenticate, or log). Routers and TCP sit below HTTP.
- The client **always initiates**. The server does not push classic HTTP responses until a request arrives (SSE / WebSocket are extra mechanisms).
- Flow: open or reuse a **TCP** connection (HTTP/3 uses **QUIC/UDP**) → send a request → read the response → keep or close the connection. HTTP/1.1 persistent connections; HTTP/2 multiplexes streams on one connection.
- **Stateless, not sessionless:** each request is independent. Cookies (and other tokens) recreate a **session** so login/cart state can span requests.
- HTTP/1.x messages are text. HTTP/2/3 keep the same **semantics** (methods, status, headers) inside binary frames. Current specs: [RFC 9110](https://httpwg.org/specs/rfc9110.html) (semantics), RFC 9112 (HTTP/1.1), RFC 9113 (HTTP/2), RFC 9114 (HTTP/3) — not the obsolete RFC 2616.

start-line → headers → blank line → optional **body**. A body is signaled by `Content-Length` or `Transfer-Encoding` (e.g. `chunked`). HEAD responses have no body; many GET requests have none.

| Start-line | Format | Example |
| ---------- | ------ | ------- |
| Request-line | `METHOD target HTTP/version` | `GET / HTTP/1.1` |
| Status-line | `HTTP/version code reason` | `HTTP/1.1 200 OK` |

| Method | Intent |
| ------ | ------ |
| `GET` | Fetch a representation (safe; should not have a request body) |
| `HEAD` | Same as GET, **no response body** |
| `POST` | Submit data / trigger processing |
| `PUT` | Replace the resource at the URI |
| `PATCH` | Partial update |
| `DELETE` | Remove the resource |
| `OPTIONS` | Communication options for the target (`*` = whole server); CORS preflight |
| `TRACE` | Loop-back of the request along the path (often disabled) |
| `CONNECT` | Open a TCP tunnel (HTTPS through a proxy) |

| Class | Meaning | Common codes |
| ----- | ------- | ------------ |
| 1xx | Informational, request continues | `100 Continue`, `103 Early Hints` |
| 2xx | Success | `200 OK` |
| 3xx | Redirect / cache revalidate | `301 Moved Permanently`, `307 Temporary Redirect` (method preserved), `304 Not Modified` |
| 4xx | Client error | `400 Bad Request`, `403 Forbidden` (identity known, no access), `404 Not Found` |
| 5xx | Server error | `500 Internal Server Error` |

Headers are metadata (`Name: value`). Informal groups: **general** (either side), **request**, **response**, **representation/entity** (`Content-Type`, `Content-Length`). Custom headers are allowed. Compression: client `Accept-Encoding: gzip, br, deflate`; server `Content-Encoding: gzip` — [HTTP compression](#http-compression-content-encoding).

Typical body media types: `text/html`, `application/json`, `multipart/form-data` (forms/files).

**Proxies** (application-layer hops): cache; filter (antivirus, parental control); load-balance; authenticate; log; sometimes detect abusive traffic. Reverse-proxy / LB detail: [0_system_design.md — Reverse proxy](./0_system_design_data.md#reverse-proxy).

**When to use:**

Every page load and `fetch`; REST APIs; static assets. Not for long bidirectional streams — [WebSocket](./6_web_api_apps.md#websocket). Inspect with DevTools Network or `curl -v`.

#### Common pitfalls

Treating HTTP as “stateful” without cookies. Calling OPTIONS “data transformation.” Using `302` when you need `307` (keep POST). Enabling `TRACE` in production (cross-site tracing). Assuming HTTP/2 has a textual `GET / HTTP/2.0` on the wire.

**Pluses and minuses:**

| + | − |
| - | - |
| Universal, cacheable, human-readable (HTTP/1.1) | Plain HTTP is unencrypted — use [HTTPS](#https) |
| Extensible via headers | No built-in session — cookies/tokens |
| Same verbs/status across HTTP/1.1, 2, 3 | HTTP/2+ framing is binary; DevTools still shows 1.1-style lines |

**Examples:**

```http
GET / HTTP/1.1
Host: example.com
User-Agent: curl/8.0
Accept: */*

HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1256

<!doctype html>…
```

```bash
curl -v http://example.com
# *  TCP / connection
# >  request (client)
# <  response (server)
```

**Thoughts:**

*The browser starts every HTTP exchange; methods and status codes are the contract; cookies add sessions on a stateless protocol.*

Structure: [HTTP request structure](#http-request-structure) · Headers: [HTTP headers](#http-headers) · Methods/status: [HTTP methods & status codes](#http-methods--status-codes) · Cookies: [HTTP cookies](#http-cookies) · JS: [HTTP in JavaScript](#http-in-javascript) · Server process: [Web server](#web-server) · Packets / DNS / LAN: [How the Internet works](./6_web_api_core.md#internet).

**Sources:**

[MDN — HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview) · [MDN — HTTP messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Messages) · [MDN — Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods) · [MDN — Status](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status) · [Doka — HTTP](https://doka.guide/tools/http-protocol/)

---

</details>

<details>
  <summary>Web server</summary>

### Web server

**Definition:** A program that accepts HTTP requests from clients (usually browsers), handles them (file, cache, proxy, or app), and sends HTTP responses.

**How it works:**

- Client–server: one server, many clients. The web server is the HTTP process on that machine (or behind a load balancer). First such programs appeared around 1991.
- Typical path: TCP (or QUIC for HTTP/3) → optional TLS → HTTP message. The server reads the start-line + headers, then the body if any, then writes a response.
- **Static:** map the request path to a file (HTML/CSS/JS/images) and return it with `Content-Type`.
- **Dynamic:** **proxy** the request to an app (Node, PHP-FPM, Python, …). The app builds the body; the web server still owns the HTTP connection to the client.
- Extra work is usually **modules**: compression, TLS certificates, auth, cache, reverse proxy. Nginx, Apache httpd, Caddy, and Node `http` are all web servers; production stacks often put Nginx/Caddy in front of the app.

Message shape (HTTP/1.1 text; HTTP/2+ uses the same semantics in binary frames / pseudo-headers):

| Part | Request | Response |
| ---- | ------- | -------- |
| Start-line | `METHOD target HTTP/1.1` | `HTTP/1.1 code reason` |
| Headers | `Name: value` per line | same |
| Blank line | end of head | end of head |
| Body | optional (POST/PUT/PATCH) | optional (not for 204 / some 1xx) |

**When to use:**

Local preview of a built site; SPA `dist/` + `/api/` proxy; TLS termination; gzip/brotli for HTML/CSS/JS.

#### Common pitfalls

Treating `GET /path HTTP/2.0` as on-the-wire HTTP/2 — HTTP/2 has no textual request-line; DevTools still *shows* method + path. Using public-key crypto for the whole payload — TLS uses asymmetric crypto in the handshake, then **symmetric** session keys ([SSL/TLS handshake](#ssltls--https-handshake)). Compressing JPEGs/ZIP again ([HTTP compression](#http-compression-content-encoding)).

**Pluses and minuses:**

| + | − |
| - | - |
| One place for TLS, compression, static files | Misconfigured proxy hides the real app error |
| Modules cover cache, auth, reverse proxy | Dev `http-server` is not a production server |

**Examples:**

```js
import { createServer } from 'node:http';

createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
  res.end('ok');
}).listen(8080);
```

Local static preview (not for production load): `npx serve` or `npx http-server` in the site folder → `http://127.0.0.1:8080` (loopback). LAN URL uses the machine’s LAN IP (e.g. `http://192.168.1.56:8080`). `browser-sync` can mirror scroll/navigation across devices on the same network.

**Thoughts:**

*The web server speaks HTTP; apps, caches, and TLS are features of that process or of a reverse proxy in front of it.*

Protocol (client, proxy, messages): [HTTP protocol](#http-protocol). Listen port / `8080`: [Ports, sockets & network services](./6_web_api_apps.md#ports-sockets--network-services).

**Sources:**

[MDN — HTTP messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Messages) · [Doka — Web server](https://doka.guide/tools/web-server/) · [Node.js http](https://nodejs.org/api/http.html) · Proxy: [Reverse proxy](./0_system_design_data.md#reverse-proxy) · [Nginx](./10_build_infra.md#nginx) · [Node `http`](./8_nodejs_modules.md#http)

---

</details>

<details>
  <summary>HTTP & TCP/IP</summary>

### HTTP & TCP/IP

**Definition:**

TCP/IP is the network stack; HTTP is an application protocol that runs on top of TCP, using its reliable byte stream for request/response messages.

Layer names and why OSI still shows up in interviews: [OSI & TCP/IP models](./6_web_api_apps.md#osi--tcpip-models).

| Protocol | Layer       | Role                       |
| -------- | ----------- | -------------------------- |
| IP       | Internet    | Addressing and routing ([IPv4 & IPv6](./6_web_api_apps.md#ipv4--ipv6)) |
| TCP      | Transport   | Reliable ordered delivery  |
| TLS      | Above TCP   | Encryption + server auth — not a TCP replacement ([HTTPS](#https)) |
| HTTP     | Application | Request/response semantics |

Ports and listening sockets: [Ports, sockets & network services](./6_web_api_apps.md#ports-sockets--network-services). End-to-end path: [How the Internet works](./6_web_api_core.md#internet).

**Examples:**

```
Client                    Server
  |---- TCP SYN ----------->|
  |<--- SYN-ACK ------------|
  |---- ACK --------------->|
  |---- HTTP GET / -------->|
  |<--- HTTP 200 OK --------|
```

**Thoughts:** Handshake, UDP, TFO vs `initcwnd`, STUN/TURN → [TCP vs UDP](./6_web_api_interview.md#tcp-vs-udp). HTTP/3 skips TCP and uses QUIC on UDP.

**Sources:**

[MDN — HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

---

</details>

<details>
  <summary>HTTPS</summary>

### HTTPS

**Definition:**

HTTPS is HTTP encrypted with TLS, providing confidentiality, integrity, and server authentication via certificates.

| HTTP              | HTTPS                    |
| ----------------- | ------------------------ |
| Port 80 (default) | Port 443 (default)       |
| Plaintext         | Encrypted                |
| No certificate    | TLS certificate required |

**How it works:**

- HTTPS is **HTTP over TLS** on port **443** (HTTP is plaintext on **80**).
- Extra work vs HTTP: **TLS handshake** (authenticate the server with a certificate, agree session keys) then **encrypt** request/response bytes on the wire.
- Confidentiality (eavesdroppers cannot read), integrity (tamper detection), authenticity (certificate chain to a trusted CA). Modern browsers treat many APIs as **secure-context** only (HTTPS).
- After TCP connects, TLS runs **before** any HTTP headers/body. HTTP/2 and HTTP/3 still sit on this encrypted channel (HTTP/3 uses QUIC/UDP, not TCP).

**When to use:** All production websites; cookies with `Secure`; mixed content blocked if page is HTTPS and subresource is HTTP.

**Pluses and minuses:**

| +                                 | −                                               |
| ------------------------------------ | -------------------------------------------------- |
| Stops MiTM reading/changing traffic  | First visit pays TLS handshake latency             |
| Required for modern browser features | Certificate misconfig / expiry takes the site down |

**Examples:**

```
https://example.com
  → TCP handshake
  → TLS handshake (cert + session keys)
  → Encrypted HTTP GET / HTTP response
```

**Thoughts:**

*HTTPS = HTTP + TLS handshake + encryption; the HTTP verbs and headers are the same, the wire is not.*

On public Wi-Fi, HTTP is a **MITM** (read/alter) risk. TLS certificates from a trusted CA stop a café attacker from swapping keys. Users who click through cert warnings undo that. HSTS reduces HTTP downgrade. Symmetric vs public-key vs hashes: [Special — Encryption](./7_special_security.md#encryption). What a cert contains and who signs it: [TLS certificates](#tls-certificates-x509--pki). See [Special — MITM](./7_special_security.md#man-in-the-middle-mitm).

**Sources:**

[howhttps.works](https://howhttps.works/) · [MDN — TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS)

---

</details>

<details>
  <summary>HTTP method characteristics</summary>

### HTTP method characteristics

**Definition:**

HTTP methods have semantic properties — safe (read-only), idempotent (repeatable without side effects), and cacheable.

| Property   | Meaning                   | Methods                                      |
| ---------- | ------------------------- | -------------------------------------------- |
| Safe       | No server state change    | GET, HEAD, OPTIONS, TRACE                    |
| Idempotent | Same effect when repeated | GET, HEAD, OPTIONS, TRACE, PUT, DELETE       |
| Cacheable  | Response may be stored    | GET, HEAD (typically); POST/PATCH if freshness + `Content-Location` |
| Neither    | Tunnel / side effects     | CONNECT (not safe, not idempotent, not cacheable) |

**Examples:**

```http
GET /api/counter HTTP/1.1     ← safe, idempotent
POST /api/counter/increment   ← not safe, not idempotent
PUT /api/settings/theme       ← idempotent replace
```

**Thoughts:** CONNECT/TRACE and status classes: [HTTP protocol](#http-protocol). Full method table: [HTTP methods & status codes](#http-methods--status-codes).

**Sources:**

[MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)

---

</details>

<details>
  <summary>SSL/TLS & HTTPS handshake</summary>

### SSL/TLS & HTTPS handshake

**Definition:**

TLS negotiates encryption keys and authenticates the server before HTTP data flows over a secure channel.

| Key type                       | Purpose                              |
| ------------------------------ | ------------------------------------ |
| Asymmetric (public/private)    | Key exchange during handshake        |
| Symmetric (shared session key) | Fast bulk encryption after handshake |

**How it works:**

1. **TCP first** — SYN / SYN-ACK / ACK (no HTTP yet).
2. **ClientHello** — TLS version, cipher suites, key share (TLS 1.3 often 1-RTT).
3. **ServerHello + certificate** — server authenticates; browser verifies the cert chain against trusted CAs (hostname match, expiry). See [TLS certificates](#tls-certificates-x509--pki).
4. **Finished** — both sides derive **symmetric** session keys (asymmetric crypto only for the handshake). Modern TLS uses **ephemeral key agreement** (e.g. ECDHE), not “encrypt the session key with RSA” (that old RSA key-exchange is deprecated).
5. **Application data** — HTTP request/response encrypted with those keys. Session tickets / 0-RTT can skip work on later visits.

HSTS (`Strict-Transport-Security`) tells the browser to use HTTPS on future visits without an HTTP hop.

1. TCP connection
2. ClientHello / ServerHello + cert
3. Browser validates with CA
4. Symmetric session keys
5. Encrypted HTTP begins

**When to use:** Every `https://` navigation; `preconnect` to warm TCP+TLS; TLS termination at Nginx or a load balancer.

**Pluses and minuses:**

| +                                             | −                                                  |
| ------------------------------------------------ | ----------------------------------------------------- |
| Fast bulk encryption after handshake             | Extra RTT(s) on cold connect                          |
| Certificate proves you talk to the intended host | Wrong clock / missing intermediate CA fails handshake |

**Examples:**

```js
// Node.js — enforce HTTPS in production
if (process.env.NODE_ENV === 'production' && !req.secure) {
  return res.redirect(301, `https://${req.headers.host}${req.url}`);
}
```

**Thoughts:** *TLS authenticates the server and sets keys; HTTP then runs inside that tunnel.*

**Sources:**

[howhttps.works — TLS explained](https://howhttps.works/the-handshake/) · [MDN — TLS handshake](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Transport_Layer_Security)

---

</details>

<details>
  <summary>TLS certificates (X.509 / PKI)</summary>

### TLS certificates (X.509 / PKI)

**Definition:** A TLS (historically “SSL”) certificate is an **X.509** document that binds a **public key** to a **subject** (domain, sometimes org) and is **signed** by a **certificate authority (CA)** the browser already trusts.

**How it works:**

- On the wire the protocol is **TLS** (SSL is the old name). HTTPS = HTTP inside a TLS tunnel. The server keeps the **private key**; the certificate is public.
- Contents (web PKI, RFC 5280 / X.509 v3): public key, subject (SAN hostnames), issuer, validity window, key usage / EKU, CA signature. Browsers ship a **root store**; the leaf must **chain** through intermediates to a trusted root.
- Trust model: site A proves identity to visitor B because both trust CA T. T signs A’s cert with T’s private key. B hashes the cert fields and checks them against T’s signature using T’s **public** key from the root/intermediate. Match → the public key in the cert belongs to that subject (as far as T checked).
- After the handshake, bulk traffic uses a **symmetric** session key. Asymmetric crypto authenticates the server and helps agree that key — it does not encrypt every HTTP byte.
- Getting a cert: (1) public CA (Let’s Encrypt ACME / paid CA) after proving **domain control**; (2) **self-signed** (issuer = subject) for local/dev/intranet — browsers warn unless you install the issuer; (3) throwaway/fake names for lab only.
- Chrome (2014+) pushed HTTPS; **July 2018** Chrome 68 labeled HTTP **Not secure**. Browsers did not generally *block* all HTTP sites. The padlock is a UI hint, not extra crypto.

Organizations that sign certs after checks. Trust is the product: a discredited CA is dropped from root stores. Pick by inclusion in browser/OS stores, ACME automation, jurisdiction, price. **Let’s Encrypt** issues free **DV** certs via ACME (hosting often does this; otherwise Certbot or another ACME client).

| Kind | What the CA actually checked | Typical use |
| ---- | ---------------------------- | ----------- |
| **DV** | Control of the domain (DNS/HTTP challenge) | Almost all public sites, Let’s Encrypt |
| **OV** | Domain + organization identity | Some enterprises |
| **EV** | Stricter org checks (CA/Browser Forum) | Banks historically; **no extra encryption**, browsers dropped the green-bar UI |
| **Wildcard** | `*.example.com` (usually not the apex `example.com` unless also listed) | Many subdomains, one cert |
| **SAN / multi-domain** | Several hostnames on one cert | `www` + apex + extras |

DV/OV/EV differ in **identity vetting**, not cipher strength. Historical “Class 1–5” (VeriSign) and RU “физлицо / бизнес / EV” catalogs are vendor packaging; interview answer is **DV / OV / EV + wildcard/SAN**.

**When to use:**

Production HTTPS (CA-signed); local HTTPS (`mkcert` / self-signed); internal PKI with a company root on managed devices.

#### Common pitfalls

Treating `openssl req -new` output as a cert (it is a **CSR**). RSA key-exchange handshake story (“browser encrypts session key with the site’s public key”) — obsolete; TLS 1.3 uses (EC)DHE. Assuming EV is “stronger HTTPS.” Clicking through cert warnings. Wildcard does not automatically cover the registrable domain unless SAN includes it.

**Pluses and minuses:**

| + | − |
| - | - |
| Stops casual MITM; unlocks secure-context APIs | Expiry / missing intermediate takes the site down |
| Let’s Encrypt + ACME is free and automated | Self-signed is untrusted on the public web |
| Same TLS crypto for cheap DV as for EV | Compromised CA is an industry-level failure |

**Examples:**

```bash
# Private key (add -aes256 to passphrase-protect it)
openssl genrsa -out example.com.key 2048

# CSR for a public CA — not a certificate; do not install this as the server cert
openssl req -new -key example.com.key -out example.com.csr
openssl req -noout -text -in example.com.csr

# Self-signed leaf for localhost / labs (issuer = subject)
openssl req -newkey rsa:2048 -x509 -sha256 -days 365 -nodes \
  -out cert.pem -keyout key.pem
```

**Thoughts:**

*A CA signature on an X.509 cert lets the browser trust a public key for a hostname; TLS then uses that to authenticate the server and set symmetric keys.*

Handshake steps → [SSL/TLS handshake](#ssltls--https-handshake). MITM → [7_special.md](./7_special_security.md#man-in-the-middle-mitm). TLS termination at Nginx → [10_build.md — Nginx](./10_build_infra.md#nginx).

**Sources:**

[MDN — Certificate authority](https://developer.mozilla.org/en-US/docs/Glossary/Certificate_authority) · [MDN — TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS) · [Let’s Encrypt — Getting Started](https://letsencrypt.org/getting-started/) · [OpenSSL HOWTO certificates](https://github.com/openssl/openssl/blob/master/doc/HOWTO/certificates.txt) · [Doka — SSL-сертификаты](https://doka.guide/tools/ssl/) (trust model; handshake/CSR details above supersede the 2022 article)

---

</details>

<details>
  <summary>HTTP request structure</summary>

### HTTP request structure

**Definition:** HTTP/1.x messages are a **start-line**, optional **headers**, a blank line, then an optional **body**. Requests use a request-line; responses use a status-line.

**How it works:**

- Request-line: `<method> <request-target> <protocol>` — e.g. `GET / HTTP/1.1`. Order is fixed. Target is usually origin-form (path + query); `Host` holds the authority. Other forms: absolute URL (proxy GET), `host:port` (`CONNECT`), `*` (`OPTIONS`).
- Status-line: `<protocol> <status-code> <reason-phrase>` — e.g. `HTTP/1.1 200 OK`. Reason phrase is for humans. Classes: [HTTP protocol](#http-protocol).
- Headers: `Name: value` (name case-insensitive). Presence of a body is indicated by `Content-Length` or `Transfer-Encoding` (often `chunked`). `Content-Type` names the media type (`text/html`, `application/json`, `multipart/form-data`).
- HTTP/2+ keeps the same fields as **pseudo-headers** (`:method`, `:path`, `:authority`, `:scheme`, `:status`) inside binary frames; version is not repeated on each message.

**When to use:**

Debug with DevTools / `curl -v`. GET should not send a request body; HEAD has no **response** body; POST/PUT/PATCH usually have a request body.

#### Common pitfalls

Forgetting `Host` on HTTP/1.1. Assuming HTTP/2 looks like `HTTP/2.0` on the wire. Confusing hop-by-hop `Transfer-Encoding` with end-to-end `Content-Encoding`.

**Examples:**

```http
POST /api/login HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer eyJhbG…

{"email":"user@example.com","password":"***"}
```

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /users/123

{"id":123}
```

```bash
curl -v http://example.com
# * connection  > request  < response
```

**Thoughts:** *Learn HTTP/1.1 text; HTTP/2/3 change framing, not methods, status codes, or header meaning.*

**Sources:**

[MDN — HTTP messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Messages)

---

</details>

<details>
  <summary>HTTP headers</summary>

### HTTP headers

**Definition:** HTTP headers are key–value metadata lines that describe the request, response, or representation (body). New features are often just new headers both sides agree on.

**How it works:**

Send **general** headers first, then request- or response-only, then **representation/entity** headers (`Content-Type`, `Content-Length`, `Content-Encoding`). Names are case-insensitive. Custom headers are allowed. Typical jobs: `Host`, `Authorization`, cache (`Cache-Control`, `ETag`), language (`Accept-Language`), last modification.

| Category | Role | Examples |
| -------- | ---- | -------- |
| General | Either side | `Cache-Control`, `Date`, `Connection` |
| Request | Client only | `Authorization`, `Accept`, `Origin`, `Host`, `User-Agent` |
| Response | Server only | `Set-Cookie`, `ETag`, `Location`, `WWW-Authenticate` |
| Representation (entity) | Body metadata | `Content-Type`, `Content-Length`, `Content-Encoding` |

**When to use:** Negotiate encoding (`Accept-Encoding` / `Content-Encoding`) — [HTTP compression](#http-compression-content-encoding). Auth, cookies, CORS all live in headers.

**Examples:**

```http
GET /api/data HTTP/1.1
Accept: application/json
Accept-Language: en-US
Accept-Encoding: gzip, deflate, br
User-Agent: MyApp/1.0
Origin: https://app.example.com
```

**Thoughts:** *Start-line is the verb/status; headers are the control plane.*

**Sources:**

[MDN — HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers)

---

</details>

<details>
  <summary>HTTP compression (Content-Encoding)</summary>

### HTTP compression (Content-Encoding)

**Definition:** The server may compress the **message body**; `Content-Encoding` names the algorithm so the client can decode back to the `Content-Type` resource.

**How it works:**

1. Client advertises `Accept-Encoding: gzip, deflate, br` (and often `zstd`).
2. Server picks one, compresses the body, responds with `Content-Encoding: br` (or `gzip`) and should send `Vary: Accept-Encoding` so caches store separate variants.
3. `Content-Length` refers to the **encoded** bytes.

| Token | Algorithm |
| ----- | --------- |
| `gzip` | LZ77 + CRC — universal |
| `deflate` | zlib wrapper around deflate |
| `br` | Brotli — usually smaller than gzip for text |
| `zstd` | Zstandard — growing support |

**When to use:**

HTML, CSS, JS, JSON, SVG. Skip already-compressed types (JPEG, PNG, WebP, ZIP, most video). Images need format/size/quality work, not gzip-on-pixels.

#### Common pitfalls

Double-compressing (app + proxy both gzip). Treating Zopfli as a separate HTTP encoding — Zopfli emits **gzip-compatible** bytes. Hop-by-hop `Transfer-Encoding` (e.g. `chunked`) is not the same as end-to-end `Content-Encoding`.

**Pluses and minuses:**

| + | − |
| - | - |
| Smaller text payloads, faster load | CPU on server; tiny bodies may grow |
| Brotli often beats gzip on HTML/CSS/JS | Missing `Vary` poisons shared caches |

**Examples:**

```http
GET /app.js HTTP/1.1
Host: example.com
Accept-Encoding: gzip, deflate, br

HTTP/1.1 200 OK
Content-Type: text/javascript; charset=utf-8
Content-Encoding: br
Vary: Accept-Encoding
```

**Thoughts:** *Negotiate with `Accept-Encoding`; label the body with `Content-Encoding`; do not gzip JPEGs.*

**Sources:**

[MDN — Content-Encoding](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding) · [MDN — Compression in HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Compression)

---

</details>

<details>
  <summary>HTTP methods & status codes</summary>

### HTTP methods & status codes

**Definition:**

Methods express intent on a resource; status codes tell the client whether the request succeeded and how to proceed.

| Method | Purpose |
| ------ | ------- |
| GET | Retrieve a representation |
| HEAD | Like GET, no response body |
| POST | Submit data / trigger action |
| PUT | Replace the resource at this URI |
| PATCH | Partial update |
| DELETE | Remove the resource |
| OPTIONS | Communication options / CORS preflight |
| TRACE | Loop-back of the request (often disabled) |
| CONNECT | TCP tunnel (typically HTTPS via proxy) |

| Code range | Meaning | Interview examples |
| ---------- | ------- | ------------------ |
| 1xx | Informational | `100 Continue` |
| 2xx | Success | `200 OK`, `201 Created`, `204 No Content` |
| 3xx | Redirect / revalidate | `301`, `307` (keep method), `304` |
| 4xx | Client error | `400`, `403` vs `401`, `404` |
| 5xx | Server error | `500`, `502`, `503`, `504` |

Safe / idempotent / PUT vs POST: [HTTP method characteristics](#http-method-characteristics) · [PUT vs POST](#put-vs-post) · [PUT vs PATCH](#put-vs-patch). Overview: [HTTP protocol](#http-protocol).

**Examples:**

```http
PUT /users/1 HTTP/1.1       → 200 OK or 404 Not Found
PATCH /users/1 HTTP/1.1     → partial field update
OPTIONS /users HTTP/1.1     → Allow: GET, POST, OPTIONS
```

**Sources:**

[MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods) · [MDN — HTTP status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)

---

</details>

<details>
  <summary>PUT vs POST</summary>

### PUT vs POST

**Definition:**

POST creates a new resource or triggers processing; PUT replaces a resource at a known URI and is idempotent.

|             | POST                  | PUT                   |
| ----------- | --------------------- | --------------------- |
| Idempotent  | No                    | Yes                   |
| Typical use | Create, actions       | Full replace / upsert |
| URI         | Collection (`/books`) | Specific (`/books/3`) |

**Examples:**

```http
POST /books        → creates new book, returns 201 + Location
PUT  /books/3      → replaces book 3 entirely (same result if repeated)
```

**Sources:**

[MDN — PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) · [MDN — POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)

---

</details>

<details>
  <summary>PUT vs PATCH</summary>

### PUT vs PATCH

**Definition:**

PUT replaces the entire resource; PATCH applies a partial modification.

|                | PUT                     | PATCH               |
| -------------- | ----------------------- | ------------------- |
| Payload        | Full representation     | Changed fields only |
| Idempotent     | Yes                     | Depends on server   |
| Missing fields | Often cleared/nullified | Unchanged           |

**Examples:**

```http
PATCH /users/1 HTTP/1.1
Content-Type: application/json

{ "email": "new@example.com" }
```

Only `email` changes; other fields remain.

**Sources:**

[MDN — PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH)

---

</details>

<details>
  <summary>Idempotent requests</summary>

### Idempotent requests

**Definition:**

An idempotent request produces the same server state no matter how many times it is repeated.

| Method                    | Idempotent?     |
| ------------------------- | --------------- |
| GET, HEAD, OPTIONS, TRACE | Yes             |
| PUT, DELETE               | Yes (by design) |
| POST, PATCH               | No (by default) |

**Examples:**

```http
DELETE /files/report.pdf   ← deleting twice: 204 then 404 (state unchanged)
POST   /files              ← each call may create another file
```

**Sources:**

[MDN — Idempotent glossary](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)

---

</details>

<details>
  <summary>HTTP cookies</summary>

### HTTP cookies

**Definition:**

Cookies are small name–value pairs the server sets via `Set-Cookie` and the browser sends back on subsequent requests to the same domain. A session cookie often holds an opaque token so the [web server](#web-server) / app can recognize an authorized user.

| Use case    | Example cookie     |
| ----------- | ------------------ |
| Session ID  | `sessionId=abc123` |
| Preferences | `lang=en`          |
| Analytics   | `_ga=GA1.2.…`      |

**Examples:**

```http
Set-Cookie: sessionId=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
```

```js
document.cookie; // readable unless HttpOnly
```

**Thoughts:** Client-only prefs belong in Web Storage / IndexedDB, not cookies (cookies ride on every request). Compare APIs: [JS Browser — storage](./3_js_browser_extra.md#browser-storage).

**Sources:**

[javascript.info/cookie](https://javascript.info/cookie) · [MDN — HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies)

---

</details>

<details>
  <summary>HttpOnly cookies</summary>

### HttpOnly cookies

**Definition:**

The `HttpOnly` flag prevents client-side JavaScript from reading a cookie, reducing XSS token theft risk.

| Flag     | Effect                               |
| -------- | ------------------------------------ |
| HttpOnly | Not accessible via `document.cookie` |
| Secure   | Sent only over HTTPS                 |
| SameSite | Limits cross-site sending            |

**Examples:**

```http
Set-Cookie: token=xyz; HttpOnly; Secure; SameSite=Strict
```

**Sources:**

[MDN — Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)

---

</details>

<details>
  <summary>HTTP caching</summary>

### HTTP caching

**Definition:** HTTP caching stores responses so repeat requests can be served from cache without contacting the origin.

**How it works:**

1. Server sends `Cache-Control: max-age=3600` and/or `ETag: "abc"`. Origin servers and reverse proxies may also cache identical requests and skip re-running the app (TTL/strategy is server config).
2. Browser (and optionally a CDN) stores the response until it is no longer **fresh**. `Cache-Control: public, max-age=31536000` tells the client a static asset can stay a year (`max-age` is seconds).
3. On expiry, the browser may send a **conditional** request: `If-None-Match: "abc"` (or `If-Modified-Since`).
4. Server returns **304 Not Modified** (no body) if unchanged — saves bandwidth.

| Directive      | Effect                                                 |
| -------------- | ------------------------------------------------------ |
| `max-age=3600` | Fresh for 3600 seconds                                 |
| `no-cache`     | Store, but **revalidate** with the origin before reuse |
| `no-store`     | Do not store (sensitive / user-specific)               |
| `private`      | Browser only, not shared CDN                           |
| `public`       | Shared caches / CDN may store                          |
| `immutable`    | Never revalidate (hashed filenames, long `max-age`)    |

| Layer        | Who                                   | Example                                                                                |
| ------------ | ------------------------------------- | -------------------------------------------------------------------------------------- |
| HTTP / CDN   | Browser disk cache, Nginx, Cloudflare | `Cache-Control`, `ETag`, `304`                                                         |
| App (client) | JS in memory / Redux                  | **RTK Query**, TanStack Query — keyed by endpoint + args, refetch, `keepUnusedDataFor` |

HTTP headers do **not** replace a client cache: two `fetch` calls in React still hit the network unless you use a library (or Cache Storage). RTK Query default: data stays while a hook is subscribed; after unmount it is garbage-collected after **60s** (`keepUnusedDataFor`), independent of `Cache-Control`.

Cached **images** skip the network. Cached **JS/CSS** still cost parse (and JS execution) — that is why code splitting exists ([10_build.md](./10_build_bundlers.md#code-splitting)).

**When to use:** Hashed asset filenames → `public, max-age=31536000, immutable`. User API → `private, no-store`. Repeat UI reads of the same GET → RTK Query / TanStack Query.

**Pluses and minuses:**

| +                                  | −                                 |
| ------------------------------------- | ------------------------------------ |
| Faster repeat loads, less server load | Stale data if TTL too long           |
| 304 saves bandwidth                   | Invalidation is hard for dynamic API |

**Examples:**

```http
Cache-Control: public, max-age=3600
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"

GET /style.css
If-None-Match: "33a64df…"
→ 304 Not Modified
```

**Thoughts:**

`Cache-Control` *is the HTTP/CDN layer; RTK Query is an in-memory client cache of JSON — they stack, they are not the same.*

| Strategy               | Behavior                           |
| ---------------------- | ---------------------------------- |
| Cache only             | Serve static assets from cache     |
| Network only           | Always fetch (analytics)           |
| Cache → network        | Offline-first fallback             |
| Network → cache        | Fresh data, cache on success       |
| Stale-while-revalidate | Serve stale, refresh in background |

**Sources:**

[MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

---

</details>

<details>
  <summary>HTTP in JavaScript</summary>

### HTTP in JavaScript

**Definition:**

Browsers expose several APIs for asynchronous HTTP without full page reloads (AJAX pattern). `fetch` is the modern default; `XMLHttpRequest` remains for upload progress and some legacy paths.

| API              | Status               |
| ---------------- | -------------------- |
| `fetch`          | Modern standard      |
| `XMLHttpRequest` | Legacy + upload progress |
| HTML `<form>`    | Full page navigation |
| Axios / jQuery   | Third-party wrappers |

**How it works:** `fetch(url, init)` returns `Promise<Response>`. Set `method` to an HTTP verb (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`, …). JSON bodies need `JSON.stringify` and usually `Content-Type: application/json`. Network failures reject; HTTP 4xx/5xx do **not** — check `res.ok`. Body once: only one of `res.json()` / `text()` / `blob()` / `arrayBuffer()`.

**Download progress:** read `response.body.getReader()` chunks; sum `value.length` vs `Content-Length` when present. **Upload progress:** not available on `fetch` — use `xhr.upload.onprogress`.

**XHR sketch:** `open` → `send` → `onload` / `onerror` / `onprogress`. `xhr.responseType = 'json'` | `'blob'` | `'arraybuffer'`. Cross-origin cookies: `xhr.withCredentials = true` (same idea as `fetch` `credentials: 'include'`).

**Resumable upload idea:** ask server how many bytes it has (`X-File-Id`), then `file.slice(startByte)` + `X-Start-Byte` via XHR (server must append).

**Examples:**

```js
const res = await fetch('/api/save-text', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(state),
});
if (!res.ok) throw new Error(`HTTP ${res.status}`);
```

```js
// download progress (fetch)
const res = await fetch(url);
const total = +res.headers.get('Content-Length') || 0;
const reader = res.body.getReader();
let received = 0;
const chunks = [];
while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  chunks.push(value);
  received += value.length;
  console.log(received, total);
}
```

**Sources:**

[javascript.info/fetch](https://javascript.info/fetch) · [javascript.info/fetch-progress](https://javascript.info/fetch-progress) · [javascript.info/xmlhttprequest](https://javascript.info/xmlhttprequest) · [javascript.info/resume-upload](https://javascript.info/resume-upload) · [MDN — Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) · [MDN — HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)

---

</details>

<details>
  <summary>Fetch API — AbortController & FormData</summary>

### Fetch API — AbortController & FormData

**Definition:** `fetch` is the modern browser API for HTTP. `AbortController` cancels in-flight requests; `FormData` builds `multipart/form-data` bodies for file uploads and mixed fields.

**How it works:**

- `fetch(url, options)` returns a `Promise<Response>` — check `res.ok` (`status 200–299`); network errors reject, HTTP 4xx/5xx do not.
- `AbortController` — `const ac = new AbortController(); fetch(url, { signal: ac.signal })`; call `ac.abort()` → promise rejects with `AbortError`. One controller can abort many fetches sharing `signal`.
- `FormData` — `fd.append('file', fileInput.files[0])`; pass `body: fd` — browser sets `Content-Type` with boundary automatically (do not set manually). File-like: `append(name, blob, fileName)`.
- Timeouts — combine `AbortController` with `setTimeout(() => ac.abort(), 5000)`.
- Rare options: `keepalive: true` (beacon-style, e.g. `unload`, body ≤ 64 KB total); `credentials` / `mode` / `cache` / `integrity` — see [javascript.info/fetch-api](https://javascript.info/fetch-api). CORS details: [CORS](./6_web_api_security.md#cors).

**When to use:**

- Debounced search — abort previous request when user types again.
- React `useEffect` cleanup — abort fetch on unmount.
- File upload — `FormData` + `POST`; for JSON use `JSON.stringify` + `Content-Type: application/json`.
- Analytics on leave — `keepalive` POST (or `navigator.sendBeacon`).

**Pluses and minuses:**

| Feature           | +                                        | −                                        |
| ----------------- | ------------------------------------------- | ------------------------------------------- |
| `AbortController` | Cancels stale requests (search, navigation) | Must handle `AbortError` in catch           |
| `FormData`        | Native multipart uploads                    | Less control than manual boundary           |
| `fetch`           | Promise-based, streams via `res.body`       | No upload progress without `XMLHttpRequest` |

**Examples:**

```js
const ac = new AbortController();
const timeout = setTimeout(() => ac.abort(), 8000);

try {
  const res = await fetch('/api/upload', {
    method: 'POST',
    signal: ac.signal,
    body: (() => {
      const fd = new FormData();
      fd.append('title', 'Report');
      fd.append('file', file);
      return fd;
    })(),
  });
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return await res.json();
} catch (e) {
  if (e.name === 'AbortError') return; // cancelled — ignore
  throw e;
} finally {
  clearTimeout(timeout);
}
```

**Thoughts:** `credentials: 'include'` sends cookies cross-origin only if CORS allows credentials. Prefer `res.json()` only after checking status.

**Sources:**

[javascript.info/fetch](https://javascript.info/fetch) · [javascript.info/formdata](https://javascript.info/formdata) · [javascript.info/fetch-abort](https://javascript.info/fetch-abort) · [javascript.info/fetch-api](https://javascript.info/fetch-api) · [MDN — AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) · [MDN — FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

---

</details>

<details>
  <summary>Rate limiting (429)</summary>

### Rate limiting (429)

**Definition:** Rate limiting caps how many requests a client can make in a time window. When exceeded, the server responds with **429 Too Many Requests**.

**How it works:**

- Server tracks counters per key — IP, API key, user ID, or combination.
- Algorithms: **fixed window** (100 req/min), **sliding window** (smoother), **token bucket** (burst + sustained rate).
- Response includes `**Retry-After**` header (seconds or HTTP-date) telling client when to retry.
- Edge/CDN (Cloudflare, API Gateway) or app middleware (Redis `INCR` + TTL) enforce limits.

**When to use:**

- Public APIs — tiered limits (free vs paid).
- Login endpoints — strict limits against brute force.
- Client — respect `429` + `Retry-After`; exponential backoff with jitter.

**Pluses and minuses:**

| Approach       | +                | −                  |
| -------------- | ------------------- | --------------------- |
| IP-based       | Simple              | Shared NAT unfairness |
| API key / user | Fair per tenant     | Requires auth         |
| Token bucket   | Allows short bursts | More complex          |
| Redis counter  | Distributed, fast   | Extra infrastructure  |

**Examples:**

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Content-Type: application/json

{ "error": "rate_limit_exceeded", "limit": 100, "window": "1m" }
```

```js
// Client retry with backoff
async function fetchWithRetry(url, opts, retries = 3) {
  const res = await fetch(url, opts);
  if (res.status !== 429 || retries === 0) return res;
  const wait = Number(res.headers.get('Retry-After') ?? 1) * 1000;
  await new Promise((r) => setTimeout(r, wait));
  return fetchWithRetry(url, opts, retries - 1);
}
```

**Thoughts:** Return **429**, not **403** — 403 means forbidden regardless of rate. Document limits in API docs (`X-RateLimit-Limit`, `X-RateLimit-Remaining` are common conventions). IP limits help brute force and Slowloris-from-one-host; they do **not** stop a large DDoS — that needs edge capacity. Incomplete slow requests need **read timeouts** as well as 429. See [Special — DoS / Slowloris](./7_special_security.md#denial-of-service-dos--ddos).

**Sources:**

[MDN — 429 Too Many Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) · [IETF — Retry-After](https://datatracker.ietf.org/doc/html/rfc7231#section-7.1.3)

---

</details>

<a name="8-auth"></a>
