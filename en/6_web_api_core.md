# Web APIs — Core

Internet, DNS, and how the web stack fits together.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [1. Core](#1-core)

---

## 1. Core

<details>
  <summary>Browser page rendering pipeline</summary>

### Browser page rendering pipeline

**Definition:** Rendering is the multi-stage process by which a browser turns downloaded HTML, CSS, and JavaScript into pixels on screen.

**How it works:**

| Stage     | What happens                |
| --------- | --------------------------- |
| Load      | Fetch HTML, CSS, JS, images |
| Parse     | Build DOM and CSSOM         |
| Style     | Compute used styles         |
| Layout    | Calculate geometry (reflow) |
| Paint     | Rasterize pixels            |
| Composite | Layer and display on GPU    |

**When to use:** JS that reads layout (`offsetWidth`) then writes style forces **layout thrashing** — batch reads/writes. `defer` scripts run after HTML parse; `async` runs when downloaded.

**Pluses and minuses:**

| + (understanding pipeline)     | −                                  |
| ------------------------------ | ---------------------------------- |
| Targeted perf fixes (LCP, CLS) | Many engine-specific optimizations |

**Examples:**

```html
<!-- Critical CSS in <head> speeds first paint -->
<link rel="stylesheet" href="critical.css" />
<script src="app.js" defer></script>
```

**Thoughts:** Canonical pipeline (fetch, dirty layout, paint order, rAF, progressive rendering) → [1_html.md — Browser & rendering](./1_html_rendering.md#browser--rendering). CSS triggers → [2_css.md](./2_css.md).

**Sources:**

[web.dev — How browsers work](https://web.dev/articles/howbrowserswork)

---

</details>

<details>
  <summary>Internet, Web & DNS</summary>

### Internet, Web & DNS

**Definition:**

The Internet is a global network of computers; the Web is an HTTP-based system of linked documents accessed via browsers; DNS maps human-readable domain names to IP addresses.

| Term     | Role                                              |
| -------- | ------------------------------------------------- |
| Internet | Global packet-routed network (TCP/IP)             |
| Web      | Hyperlinked documents over HTTP                   |
| DNS      | Domain → IP phonebook (`A` = IPv4, `AAAA` = IPv6) |
| Browser  | User agent that requests and renders pages        |

**How it works:** Dual-stack clients query **AAAA** and **A**. **Happy Eyeballs** (RFC 8305) races connections and prefers working IPv6, falling back to IPv4 if IPv6 is slow or broken. Full addressing: [IPv4 & IPv6](./6_web_api_apps.md#ipv4--ipv6). LAN vs public Internet, packets, and the page-load path: [How the Internet works](#internet). DNS is a **lookup** (cache → resolver → nameservers → IP); the browser then opens TCP/TLS to that IP — packets are not “sent to DNS as the destination.”

**Examples:**

```
User types https://example.com
  → DNS: AAAA + A (e.g. 2001:db8::1 and 93.184.216.34)
  → Happy Eyeballs picks a working address
  → Browser opens TCP, sends HTTP GET
  → Server returns HTML; browser renders
```

**Sources:**

[Cloudflare — What is HTTP?](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) · [MDN — HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) · [RFC 8305](https://www.rfc-editor.org/rfc/rfc8305.html)

---

</details>

<details>
  <summary>Internet</summary>

### Internet

**Definition:** Hosts exchange **packets** under shared **protocols**. The same TCP/IP stack runs on a **LAN** and on the public **Internet**; the difference is addressing, routing scope, and who can reach whom — not a different “kind” of packet.

**How it works:**

- A **protocol** is the rule set every hop agrees on (size, headers, who ACKs). Devices: hosts, switches, routers, gateways, firewalls, APs.
- Each host has an **IP**. Packets are forwarded toward the destination IP (routers pick a next hop). The OS **network stack** (often in the kernel) applies addresses, ports, and local filter rules.
- **LAN vs Internet:** same Ethernet/Wi-Fi + IP + TCP/UDP. A LAN is usually one site with **private** IPv4 (`10/8`, `172.16/12`, `192.168/16`) or IPv6 ULA/link-local; a home/office **NAT gateway** hides many hosts behind one public address. The Internet is many independently routed networks (public unicast, BGP). A LAN peer (`192.168.1.20`) is not reachable from the public Internet unless you publish a public IP, port-forward, or use a tunnel/VPN.
- **Page load (MDN):** type a URL → **DNS** maps the host to IP (local cache, then a resolver) → **TCP** handshake → **TLS** if `https:` → **HTTP** request → server sends files as packets → browser **reassembles** HTML and repeats for CSS/JS/images. DNS does **not** rewrite in-flight packets; the client learns the IP first, then talks to the origin (or CDN).
- **HTTP** is application-layer (OSI 7 / TCP/IP application). **TCP** is reliable ordered streams; **UDP** is datagrams (checksum, no handshake, no retransmission). **TLS** is **not** a third transport like TCP: HTTPS is HTTP **over TLS over TCP** (HTTP/3: HTTP over **QUIC over UDP**). Plain HTTP on a hop you control is readable; TLS encrypts the bytes on the wire. Details: [HTTPS](./6_web_api_http.md#https) · [TCP vs UDP](./6_web_api_interview.md#tcp-vs-udp).
- **Ports** (1–65535) select which **socket**/process gets the packet. OS **network service** binds a socket to an IP:port; a **network service** (daemon) is the app that speaks HTTP, SMTP, etc. Defaults: HTTP **80**, HTTPS **443**. Full table and `ss`/`netstat`: [Ports, sockets & network services](./6_web_api_apps.md#ports-sockets--network-services).

**When to use:**

Explain “what happens when I open a URL”; why `localhost:8080` works on your machine but not from a phone on another network; why café Wi-Fi + HTTP is a MITM risk.

#### Common pitfalls

Treating DNS as the HTTP destination. Calling TLS a replacement for TCP. Assuming LAN IPs are globally routable. Copying Linux `netstat -p` as “protocol” (`-p` is PID on Linux; protocol filter is BSD/macOS).

**Pluses and minuses:**

| +                                        | −                                             |
| ---------------------------------------- | --------------------------------------------- |
| One mental model (packets + IP + port)   | NAT and firewalls hide “just ping the LAN IP” |
| Same tools on LAN and WAN (`ping`, `ss`) | “TLS/IP stack” as a peer of TCP/IP is a myth  |

**Examples:**

```
https://example.com/index.html
  1. DNS A/AAAA → 93.184.216.34 (example)
  2. TCP 3-way handshake to that IP:443
  3. TLS handshake (cert + session keys)
  4. Encrypted HTTP GET /index.html
  5. Packets routed hop-by-hop; server stack demuxes port 443 → web server
  6. HTML packets return; browser parses and fetches /style.css the same way
```

**Thoughts:**

_DNS finds the IP; TCP/TLS opens the pipe; HTTP is the conversation; IP+port is how packets find the right process._

Layers: [OSI & TCP/IP models](./6_web_api_apps.md#osi--tcpip-models). Addresses: [IP address](#ip-address) · [IPv4 & IPv6](./6_web_api_apps.md#ipv4--ipv6). Packet capture: Wireshark/`tcpdump` in the OSI block. Host firewall: [15_devops.md — Firewall](./15_devops.md#firewall--packet-filter).

**Sources:**

[MDN — How the web works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works) · [MDN — How the Internet works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work) · [MDN — Domain names](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) · [Doka — Working with the network](https://doka.guide/tools/network/)

---

</details>

<details>
  <summary>Client–server architecture</summary>

### Client–server architecture

**Definition:** The usual web split: a **client** (browser) requests work; a **server** applies business rules and talks to a **database**. Why a backend exists: the UI is HTML/CSS/JS; durable, shared, authorized data is not.

**How it works:**

- Client → “read / filter / save this.” For the user: present data and collect edits.
- Server → persist, authorize, transform; optional background jobs (index, email).
- DB → storage, replicas, backups — often more than one technology.

**When to use:** Feeds, docs, auth. Full picture: [0_system_design.md — Client–server](./0_system_design_core.md#clientserver-architecture-why-a-backend). Messages, methods, proxies: [HTTP protocol](./6_web_api_http.md#http-protocol). The HTTP process on the server is the [web server](./6_web_api_http.md#web-server).

**Thoughts:** Architecture describes contracts between parts, not every module’s internals.

**Sources:**

[Doka — How web apps work](https://doka.guide/tools/web-app-works/)

---

</details>

<details>
  <summary>Evolution of web apps (reload → AJAX HTML → JSON)</summary>

### Evolution of web apps (reload → AJAX HTML → JSON)

**Definition:** Same client–server idea; the **payload** changed: full HTML pages → HTML fragments without reload → **JSON** (or similar) that the client renders.

**How it works:**

1. **Classic MPA** — every click (even “sort this table”) is a new document. All business logic on the server; the browser only shows HTML.
2. **AJAX (Web 2.0)** — background HTTP (`XMLHttpRequest` / later `fetch`). Early pattern: server still returns a **chunk of HTML**; JS inserts it. Better UX, still couples markup to the API.
3. **Data APIs** — first response is HTML (SSR) or a shell + embedded JSON; later actions send JSON. The client **re-renders** from data (React and similar), not by swapping HTML strings.

**When to use:**

Interview: name the three eras. Prefer JSON when multiple clients share one API; SSR HTML is still valid for first paint.

| Data-only APIs                   | HTML fragments / full pages                |
| -------------------------------- | ------------------------------------------ |
| Server can ignore page structure | Faster to ship a snippet for a tiny widget |
| Smaller payloads                 | Markup and API version together            |
| SSR optional for first HTML      | Harder to reuse on mobile / another UI     |

**Thoughts:** Modern apps exchange **state as data**; the client owns most of the View.

**Sources:**

[Doka — How web apps work](https://doka.guide/tools/web-app-works/) · Types: [MPA vs SPA](./6_web_api_apps.md#12-types-of-web-apps) · AJAX: [JS Async](./3_js_async_await.md)

---

</details>

<details>
  <summary>IP address</summary>

### IP address

**Definition:**

An IP address is a numeric identifier for a device on a network, used to route packets to the correct destination.

| Version | Size                         | Notes              |
| ------- | ---------------------------- | ------------------ |
| IPv4    | 32-bit (~4.3B addresses)     | e.g. `192.168.0.1` |
| IPv6    | 128-bit (vast address space) | e.g. `2001:db8::1` |

**How it works:** Public unicast is globally routed. **Private** IPv4 (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) stays on the LAN unless NAT or a tunnel publishes it. Loopback: `127.0.0.1` / `::1`. Packets + LAN vs Internet: [How the Internet works](#internet). Formats, prefixes, URL literals, DNS `AAAA`, and Happy Eyeballs: [IPv4 & IPv6](./6_web_api_apps.md#ipv4--ipv6).

**Examples:**

```
IPv4: 192.168.1.10
IPv6: 2001:db8:85a3::8a2e:370:7334
```

**Sources:**

[MDN — IP address glossary](https://developer.mozilla.org/en-US/docs/Glossary/IP_Address)

---

</details>

<details>
  <summary>URI, URL & URN</summary>

### URI, URL & URN

**Definition:**

A **URI** identifies a resource. A **URL** is a URI that also says _how_ to reach it (scheme + location). A **URN** names a resource without locating it (`urn:isbn:…`).

| Type | Identifies         | Includes access method |
| ---- | ------------------ | ---------------------- |
| URI  | Resource (general) | Optional               |
| URL  | Resource location  | Yes (`https://…`)      |
| URN  | Resource name      | No (`urn:isbn:…`)      |

**How it works:**

An absolute HTTP(S) URL is `scheme ":" "//" authority path [ "?" query ] [ "#" fragment ]`. Authority is `[ userinfo "@" ] host [ ":" port ]` (RFC 3986). The browser resolves the host via [DNS](#internet-web--dns) to an IP, then opens TCP to the port.

| Part      | Example                 | Role                                                                                                                                                                                              |
| --------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scheme    | `https`                 | Protocol; `:` is the separator. `http` is unencrypted. Also `mailto:`, `ftp:`, `ws:` / `wss:`.                                                                                                    |
| Authority | `user:pass@host:443`    | Starts after `://`. Usually just the host.                                                                                                                                                        |
| Userinfo  | `admin:secret@`         | Login + password before `@`. RFC 3986 **deprecates** `user:password` in URLs (leaks in history, logs, Referer). Browsers may ignore or block it.                                                  |
| Host      | `doka.guide`            | Domain or IP. People type names; DNS is the phonebook to IPv4/`A` and IPv6/`AAAA`.                                                                                                                |
| Port      | `:443`                  | Process on the host. WHATWG defaults: **http/ws → 80**, **https/wss → 443**, **ftp → 21**. Default port is omitted from the URL and from `origin`.                                                |
| Path      | `/tools/url`            | Resource on the origin.                                                                                                                                                                           |
| Query     | `?query=hi&category=js` | Starts at `?`; pairs `key=value` joined by `&`. Same key may repeat. Not the resource identity — filters, search, pagination. REST split: [query vs path params](#query-parameters--path-params). |
| Fragment  | `#kratko`               | Starts at `#`. Client-only; **not sent to the server**. Scrolls to `id` / named anchor.                                                                                                           |

No host — the browser fills scheme/host from the current document (or `<base href>`). Path without `/` is relative to the current path; `/…` is from the origin root; `../` goes up one path segment; `//host/path` is scheme-relative (keep `https`/`http`). The address bar resembles a file path; SSR may hide `index.html`, and client routing can show a virtual path via History API — [Files & file systems](./8_nodejs_modules.md#files--file-systems).

One registered domain can front several services (`mail.google.com` vs `www.google.com`). `www` is just a common third-level name; most sites redirect `www` ↔ apex.

**When to use:**

Paste a full URL in the address bar; `<a href>` / `location`; REST resource IDs; `mailto:` contact links.

#### Common pitfalls

Putting secrets in query or userinfo; treating `#` as a server param; omitting encoding (`привет` must be percent-encoded on the wire); assuming extra ports are a security control (obscurity only — use auth). `www` vs apex are different hosts until DNS/redirects say otherwise.

**Pluses and minuses:**

| +                                      | −                                        |
| -------------------------------------- | ---------------------------------------- |
| Human-readable vs raw IPs              | Userinfo in URLs is a credential leak    |
| Relative links survive domain moves    | Fragment never reaches the API           |
| `URL` / `URLSearchParams` parse safely | Hand-splitting `?` / `&` breaks encoding |

**Examples:**

```
https://doka.guide/tools/url
https://doka.guide/search/?query=привет&category=html#results
mailto:hi@doka.guide
urn:isbn:978-0-393-04002-9
```

```js
const url = new URL("https://doka.guide");
const page = new URL("/tools/url", url);
page.searchParams.set("q", "url");
page.hash = "kratko";
page.href;
// https://doka.guide/tools/url?q=url#kratko
```

**Thoughts:** URL = _how to fetch_ + _where_; DNS turns the host into an IP; query is extra input; hash stays in the browser.

**Sources:**

[MDN — URI](https://developer.mozilla.org/en-US/docs/Glossary/URI) · [MDN — URL](https://developer.mozilla.org/en-US/docs/Web/API/URL) · [WHATWG URL](https://url.spec.whatwg.org/) · [RFC 3986](https://www.rfc-editor.org/rfc/rfc3986) · [Doka — URL](https://doka.guide/tools/url/)

---

</details>

<details>
  <summary>Host vs domain</summary>

### Host vs domain

**Definition:**

A host is a machine (server) on the network with an IP address; a domain is a human-readable name that DNS maps to one or more hosts. In a URL the host sits in the authority; anatomy: [URI, URL & URN](#uri-url--urn).

| Term        | Meaning                                                       |
| ----------- | ------------------------------------------------------------- |
| Host        | Physical/virtual server (`api.example.com` resolves to an IP) |
| Domain      | Registered name (`example.com`)                               |
| Web hosting | Service that stores and serves site files                     |

**Examples:**

```
Domain: google.com
Host:   www.google.com → 142.250.185.78
```

**Sources:**

[MDN — Host glossary](https://developer.mozilla.org/en-US/docs/Glossary/Host)

---

</details>

<details>
  <summary>Query parameters & path params</summary>

### Query parameters & path params

**Definition:**

Path parameters identify a resource in the URL path; query parameters filter, sort, or paginate without changing the resource identity. Full URL parts (scheme, host, fragment, relative links): [URI, URL & URN](#uri-url--urn). JS helpers: [URL / URLSearchParams](./3_js_browser_extra.md#url--urlsearchparams).

| Style       | Example                       | Use case                 |
| ----------- | ----------------------------- | ------------------------ |
| Path param  | `/books/3`                    | Resource identity        |
| Query param | `/books?sort=title&page=2`    | Filtering, pagination    |
| Both        | `/users/1/orders?status=open` | Nested resource + filter |

**Examples:**

```js
const params = new URLSearchParams({ page: "2", limit: "20" });
fetch(`/api/books?${params}`);
```

**Sources:**

[MDN — URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)

---

</details>

<details>
  <summary>API (Application Programming Interface)</summary>

### API (Application Programming Interface)

**Definition:** _API (Application Programming Interface)_ is the set of features one program exposes to others: “talk to me this way — ask here, change things there.” It exists so modules, systems, and languages can communicate (JS client ↔ Python/Java server, exported functions, Gulp, Arduino SDK).

**How it works:**

A JS client cannot pass a native object to a Python/Java server. Both sides agree on a surface (URLs + HTTP methods, exported functions, SDK methods) and usually a wire format (JSON). That surface is the API.

| API type             | Where it runs             | Example                                         |
| -------------------- | ------------------------- | ----------------------------------------------- |
| Browser / Web API    | In the browser (`window`) | `fetch`, `console.log`, `localStorage`          |
| Library / module API | Same process              | Exported `CreditCalculator` functions, Gulp API |
| Device / SDK API     | Native / firmware         | Arduino SDK                                     |
| Web/HTTP API         | Over HTTP                 | REST, GraphQL, RPC endpoints                    |
| Third-party API      | External platform         | X (Twitter), Stripe, Maps                       |

**Ideal API** (Doka: бесшовность, быстродействие, понятность, полнота) — all four are the goal; a perfect API does not exist.

| Quality  | RU             | One-liner                       |
| -------- | -------------- | ------------------------------- |
| Seamless | Бесшовность    | Reuse with glue, not a rewrite  |
| Fast     | Быстродействие | Cheap serialize + round-trip    |
| Clear    | Понятность     | Names that don’t mislead        |
| Complete | Полнота        | Right amount of data per action |

- **Бесшовность (seamless)** — **Definition:** reuse once-written behavior in another project with only a thin adapter, not a rewrite. No universal module exists; the most seamless API is the one where that glue hurts least.
- **Быстродействие (fast)** — **Definition:** time spent translating (usually to JSON), talking to the DB, and returning data. Less time on that path = better API.
- **Понятность (clear)** — **Definition:** names of functions, methods, and URLs that match intent so callers make fewer mistakes.
- **Полнота (complete)** — **Definition:** each action returns the right amount of data — not missing fields, not a dump of everything.

**When to use:**

Browser talking to a backend; one module used by the rest of an app; plugins consuming a host API (Gulp); hardware SDKs.

#### Common pitfalls

Treating “API” as only REST URLs; over/under-fetching from a poorly shaped contract; copying JSON-RPC 1.0 response shapes (`result` + `error: null`) into JSON-RPC 2.0.

**Examples:**

```http
POST /api/v1.0/users   → create user
GET  /api/v1.0/users   → list users
```

```js
localStorage.setItem("key", "value");
console.log("debug");
```

**Thoughts:** API is any published contract between programs — HTTP routes are one kind, not the only kind.

**Sources:**

[Doka — What is an API](https://doka.guide/tools/api/) · [AWS — What is an API?](https://aws.amazon.com/what-is/api/) · [MDN — Web APIs](https://developer.mozilla.org/en-US/docs/Web/API)

---

</details>

<details>
  <summary>CDN (Content Delivery Network)</summary>

### CDN (Content Delivery Network)

**Definition:** A CDN is a geographically distributed network of edge servers that cache and serve static content closer to users.

**Pluses and minuses:**

| +                                | −                             |
| -------------------------------- | ----------------------------- |
| Lower latency, less origin load  | Cache invalidation complexity |
| DDoS mitigation, TLS termination | Extra cost and configuration  |

**Examples:**

```html
<script src="https://cdn.example.com/lib/react.min.js"></script>
```

**Thoughts:** Edge HTML + HTTP/2/3/TLS 1.3 is the usual network fix for high TTFB. Full metric: [TTFB](./6_web_api_security.md#ttfb-time-to-first-byte).

**Sources:**

[Cloudflare — What is a CDN?](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)

---

</details>
