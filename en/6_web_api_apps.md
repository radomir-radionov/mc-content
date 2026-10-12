# Web APIs — Apps

Protocols, real-time, and types of web apps.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [10. Protocols](#10-protocols)
- [11. Real-time & rendering](#11-real-time-rendering)
- [12. Types of web apps](#12-types-of-web-apps)

---

## 10. Protocols

<details>
  <summary>OSI & TCP/IP models</summary>

### OSI & TCP/IP models

**Definition:** A **network model** is a layered spec of how hosts exchange digital data. **OSI** (Open Systems Interconnection, ISO/IEC 7498-1 / ITU-T X.200) is a 7-layer _reference_ model for teaching, standards talk, and troubleshooting. The Internet runs the **TCP/IP** (DoD / Internet protocol suite) stack described in RFC 1122 / RFC 1123 — four broad layers, not seven.

**How it works:**

Each OSI layer talks to the layer below and offers a service to the layer above. On send, data is **encapsulated** (header/footer added); on receive, headers are stripped. Layers 1–3 are **media** (bits → frames → packets across links and routers); 4–7 are **host** layers.

| OSI            | PDU                            | Job                                                  | Typical examples                                                         |
| -------------- | ------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------------------ |
| 7 Application  | data                           | App protocols the user/program sees                  | HTTP, SMTP, FTP, DNS, SIP, [SSH](./15_devops.md#ssh)                     |
| 6 Presentation | data                           | Syntax: encoding, compression, encryption (textbook) | TLS/MIME/ASN.1; JPEG/TIFF as _formats_, not a wire protocol              |
| 5 Session      | data                           | Setup / checkpoint / teardown of a dialog            | RPC-style sessions, PPTP tunnels; sockets in TCP/IP                      |
| 4 Transport    | segment (TCP) / datagram (UDP) | End-to-end delivery, ports, optional reliability     | TCP, UDP, QUIC                                                           |
| 3 Network      | packet                         | Addressing and **routing** between networks          | IP, ICMP, OSPF. IP does **not** guarantee order, delivery, or uniqueness |
| 2 Data link    | frame                          | Neighbor delivery on one link; MAC; error detect     | Ethernet, Wi-Fi (IEEE 802.11), PPP. IEEE 802 splits **MAC** + **LLC**    |
| 1 Physical     | bit / symbol                   | Signals on a medium                                  | Copper, fiber, radio; 802.15 Bluetooth PHY, DSL, GSM radio               |

interviews and ops still say **L4 load balancer** (IP+port) vs **L7** (HTTP path/headers). The names are OSI even when the stack is TCP/IP. It also maps “where did this break?” (cable vs switch vs routing vs TCP vs TLS vs HTTP).

| TCP/IP                | Maps to OSI                                              | Examples                  |
| --------------------- | -------------------------------------------------------- | ------------------------- |
| Application           | 5 + 6 + 7                                                | HTTP, DNS, TLS, WebSocket |
| Transport             | 4 (plus TCP’s graceful close, which OSI puts in session) | TCP, UDP                  |
| Internet              | 3                                                        | IP, routing               |
| Link / Network Access | 1 + 2                                                    | Ethernet, Wi-Fi           |

RFC 3439 (“Layering considered harmful”) is why real stacks are leaky: TLS sits between HTTP and TCP; HTTP/3 uses **QUIC over UDP**. Do not force every protocol into one OSI box.

- **DoD / TCP/IP** — the one that won; designed for a distributed packet network that is hard to kill (each packet routed independently).
- **IPX/SPX** (Novell NetWare) — IPX routing + SPX transport; common on DOS LANs in the 1990s, weak on global WANs, almost gone.
- **AppleTalk** — another vendor suite; same idea (layers), different protocols.

- **Wireshark** — GUI: packet list, decoded tree, hex/bytes. **Capture filters** (libpcap, at sniff time) vs **display filters** (after capture). Start/stop capture on an interface.
- **tcpdump** — CLI; often needs root/admin. Stop with **SIGINT** (Ctrl+C). Write/read pcap:

```bash
tcpdump -n 'tcp port 80 or udp port 80'
tcpdump -w out.pcap
tcpdump -r out.pcap
tcpdump -n 'port 8080'
```

**When to use:**

Interview: “HTTP is application, TCP transport, IP internet.” Debugging: Wireshark/tcpdump. Design: L4 vs L7 LB ([load balancing](./0_system_design_data.md#load-balancing)).

#### Common pitfalls

Treating OSI as how browsers are implemented. Calling GraphQL/gRPC “session layer” — they are **application** protocols (RPC is only a _textbook_ session example). Assuming IP is reliable. Equating UDP with “no integrity” — UDP still has a checksum; it just does not retransmit or reorder.

**Pluses and minuses:**

| +                                    | −                                                                  |
| ------------------------------------ | ------------------------------------------------------------------ |
| Shared vocabulary (L2/L3/L4/L7)      | OSI _protocols_ never replaced TCP/IP                              |
| Clean mental model for encapsulation | Strict 7-layer mapping of TLS/QUIC/RPC is forced                   |
| Separates “medium” from “app”        | Extra session/presentation layers are often just the app in TCP/IP |

**Examples:**

```
Browser GET https://example.com
  L7  HTTP request
  L4  TCP segments (ports; order + retransmission)  — or QUIC/UDP for HTTP/3
  L3  IP packets (src/dst addresses; routers pick a path per packet)
  L2  Ethernet/Wi-Fi frames (MAC; FCS/CRC on the frame)
  L1  bits on wire / radio
```

Ethernet payload **MTU** is typically **1500** bytes, not a fixed “1446-byte frame.” TCP MSS on IPv4 Ethernet is often **1460** (1500 − 20 IP − 20 TCP); PPPoE and other headers shrink it further.

**Thoughts:** _OSI is the 7-layer dictionary; TCP/IP is the 4-layer stack the web runs on — HTTP over TCP (or QUIC) over IP over a link._

**Sources:**

[Doka — OSI](https://doka.guide/tools/osi/) · [ISO/IEC 7498-1](https://www.iso.org/standard/20269.html) · [RFC 1122](https://www.rfc-editor.org/rfc/rfc1122) · [MDN — How the Internet works](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work) · [tcpdump(1)](https://www.tcpdump.org/manpages/tcpdump.1.html) · [Wireshark capture filters](https://www.wireshark.org/docs/wsug_html_chunked/ChCapCaptureFilterSection.html) · TCP/UDP: [TCP vs UDP](#tcp-vs-udp) · HTTP stack: [HTTP & TCP/IP](./6_web_api_http.md#http--tcpip) · Sockets: [Ports, sockets & network services](#ports-sockets--network-services)

---

</details>

<details>
  <summary>IPv4 & IPv6</summary>

### IPv4 & IPv6

**Definition:** IPv6 (Internet Protocol version 6) is the 128-bit successor to IPv4: it routes packets between hosts and aims at end-to-end connectivity (fewer NAT “grey” vs public addresses).

**How it works:**

- Address is **128 bits**, written as **eight hextets** (four hex digits). Example: `2001:0db8:4ac7:fddd:8da3:981f:fbda:1e43`.
- Typical split of a `/64`: first 64 bits = **network prefix** (ISP / site); last 64 bits = **interface identifier** (device on that subnet — think UUID-like, not a guarantee of a stable MAC).
- **Compression (RFC 5952):** omit leading zeros in a hextet; replace **one** longest run of zero hextets with `::`. `2001:0db8:0000:0000:0000:0000:0000:0000` → `2001:db8::`. You may keep extra zeros for teaching (`2001:db8:0:0:0:0:0:0`); canonical form is compressed + lowercase.
- **URL literals (RFC 3986):** colons clash with `host:port`, so wrap the address in `[]`. Tools like `telnet host port` take the bare address.
- **Prefixes** use CIDR (`address/length`), not dotted IPv4 masks. `/64` is the usual LAN subnet (SLAAC). Larger prefixes from an ISP are collections of `/64`s.
- **DNS:** `A` → IPv4, `AAAA` → IPv6. Dual-stack **Happy Eyeballs** (RFC 8305) queries both and races connects (IPv6 first, IPv4 fallback) so a broken AAAA does not stall the site.
- **Privacy:** OSes use **temporary / privacy addresses** (RFC 8981). Do not treat a full 128-bit client address as a stable person ID; correlate at **`/64` or wider** if you must group a network.

| Prefix | Size           | Typical use                                             |
| ------ | -------------- | ------------------------------------------------------- |
| `/64`  | 1 subnet       | Home/LAN subnet (rough IPv4 `/24` analog for “one LAN”) |
| `/60`  | 16 × `/64`     | Often ISP to a customer                                 |
| `/56`  | 256 × `/64`    | Often ISP to a customer                                 |
| `/48`  | 65 536 × `/64` | Site / organization                                     |
| `/32`  | 65 536 × `/48` | ISP / LIR allocation                                    |

| Prefix                       | Purpose                                                      |
| ---------------------------- | ------------------------------------------------------------ |
| `::1/128`                    | Loopback (`127.0.0.1`)                                       |
| `2001:db8::/32`, `3fff::/20` | Documentation only (RFC 3849, RFC 9637) — not for production |
| `fe80::/10`                  | Link-local (one link; not globally routed)                   |
| `::ffff:0:0/96`              | IPv4-mapped IPv6 (often in DBs / dual-stack sockets)         |
| `2000::/3`                   | Currently allocated **global unicast** (public Internet)     |
| `fc00::/7`                   | Unique local (ULA, “private-ish”)                            |

**When to use:**

Dual-stack public sites (mobile / some regions are IPv6-heavy). WebRTC / VoIP: real public addresses reduce NAT/TURN. Local debug without ngrok when you have a global IPv6. Listen on `[::]` plus IPv4; publish a **static** AAAA.

#### Common pitfalls

Publishing an AAAA that does not answer (blackhole) — dual-stack users may still work via Happy Eyeballs; **IPv6-only** clients fail. Using `2001:db8::` or `3fff::` on real hosts. Treating `fe80::` as a public address. Putting IPv6 in a URL without `[]`. Identifying users by the full address instead of a `/64`. Assuming “nobody uses IPv6” — mobile share is large ([Google](https://www.google.com/intl/en/ipv6/statistics.html), [Cloudflare](https://www.cloudflare.com/ipv6/)).

**Pluses and minuses:**

| +                                                     | −                                                 |
| ----------------------------------------------------- | ------------------------------------------------- |
| Huge space; one address per host is normal            | URLs and logs look noisy; `:` vs port             |
| Less CGNAT: P2P, WebRTC, inbound to a device          | Broken AAAA without Happy Eyeballs = timeouts     |
| Unique host vs shared IPv4 hosting/CGNAT blocks       | Privacy: don’t log 128 bits as a durable user key |
| IPv6-only networks (some mobile / developing regions) | Need a global unicast address + working DNS       |

**Examples:**

```
# URL (brackets required)
https://[2001:db8:4ac7:fddd:8da3:981f:fbda:1e43]:8080/

# CLI (no brackets)
telnet 2001:db8:4ac7:fddd:8da3:981f:fbda:1e43 8080

# Short forms
::1          # loopback
2001:db8::   # documentation prefix, compressed
```

Ship IPv6 on a site: (1) server address in **`2000::/3`**, (2) web server listens on IPv6, (3) stable (static) address, (4) **AAAA** in DNS. Nginx: `listen [::]:443 ssl;` plus an IPv4 `listen`.

**Thoughts:** IPv6 is 128-bit end-to-end addressing; for the web you need listen + static GUA + `AAAA`, and clients already prefer it via Happy Eyeballs.

**Sources:**

[Doka — IPv6 for webdevs](https://doka.guide/tools/ipv6-for-webdevs/) · [MDN — IPv6](https://developer.mozilla.org/en-US/docs/Glossary/IPv6) · [IANA special-purpose IPv6](https://www.iana.org/assignments/iana-ipv6-special-registry) · [RFC 3986](https://www.rfc-editor.org/rfc/rfc3986.html) · [RFC 5952](https://www.rfc-editor.org/rfc/rfc5952.html) · [RFC 8305](https://www.rfc-editor.org/rfc/rfc8305.html) · [RFC 9637](https://www.rfc-editor.org/rfc/rfc9637.html)

---

</details>

<details>
  <summary>SSL/TLS</summary>

### SSL/TLS

**Definition:**

TLS (successor to SSL) encrypts network traffic and authenticates peers using certificates and negotiated cipher suites.

| Feature        | Role                               |
| -------------- | ---------------------------------- |
| Encryption     | Confidentiality on the wire        |
| Authentication | Certificate proves server identity |
| Integrity      | Detect tampering                   |

**Examples:**

```
Browser → supports TLS 1.3
Server  → presents cert signed by Let's Encrypt
Both    → agree cipher suite, exchange keys
```

**Thoughts:** Certificate format, CAs, DV/OV/EV, Let’s Encrypt, OpenSSL → [TLS certificates](./6_web_api_http.md#tls-certificates-x509--pki). Handshake → [SSL/TLS handshake](./6_web_api_http.md#ssltls--https-handshake).

**Sources:**

[MDN — TLS glossary](https://developer.mozilla.org/en-US/docs/Glossary/TLS)

---

</details>

<details>
  <summary>TCP vs UDP</summary>

### TCP vs UDP

**Definition:** TCP and UDP are **transport-layer** protocols (OSI layer 4 / TCP/IP transport). They move application bytes between a client (browser) and a server as packets; they differ in reliability, ordering, and connection setup.

**How it works:**

- Data on the wire is packets of bounded size. **TCP** builds a **byte stream** after a connection; **UDP** sends independent **datagrams** with no session.
- **TCP (Transmission Control Protocol)** — connection-oriented. Retransmits lost segments, drops duplicates, delivers in order, ACKs the sender, and runs **congestion control** (start small, grow with available bandwidth).
- **UDP (User Datagram Protocol)** — connectionless. No handshake, no delivery/order/duplicate guarantees. Header is **8 bytes** (source port, dest port, length, checksum). Checksum is optional in IPv4, required in IPv6. Apps that need reliability implement it themselves (or use TCP/QUIC).

SYN means **synchronize**. Each side picks a random **initial sequence number** (ISN).

1. Client → `SYN` with seq `x`.
2. Server → `SYN-ACK` with seq `y`, ack `x + 1`.
3. Client → `ACK` with ack `y + 1` (its seq is `x + 1`). Connection is **ESTABLISHED**.

Client may send data with/after the final ACK; the server waits until that ACK before treating the connection as open. Close is a **four-way** FIN/ACK exchange (each side closes independently). Handshake sits **after DNS** and **before TLS** on HTTPS.

Many origins on a page = many handshakes = extra RTTs. Mitigations: reuse connections (HTTP keep-alive, HTTP/2 multiplex), `<link rel="preconnect">` (DNS + TCP + TLS; not `dns-prefetch`, which is DNS only), fewer hosts.

- **TFO (RFC 7413)** — after a prior visit, a **Fast Open cookie** lets the client put **application data in the SYN**. Server data in SYN-ACK is optional. SYN payload is limited to about **one MSS** (not 10 segments). Needs OS + stack support on both ends; middleboxes sometimes break it. HTTP/3/QUIC is the web’s usual 0-RTT path today.
- **Initial congestion window (`initcwnd`, RFC 6928)** — after the handshake, sender may burst up to `min(10 * MSS, max(2 * MSS, 14600))` bytes in the first RTT (~10 × 1460 on Ethernet). That is **not** the TFO SYN limit.

Critical rendering path is a **general** browser pipeline (parse HTML/CSS, first paint before all assets), not a Chromium-only feature.

DNS (often), games, live A/V, **WebRTC**, **QUIC / HTTP/3**. Dropping a late frame is better than stalling for a retransmission. NAT (one public IP, many private hosts) plus firewalls make inbound UDP hard: **STUN** learns the public IP:port; **TURN** relays when symmetric NAT blocks peers; **ICE** tries STUN then TURN ([MDN WebRTC protocols](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols)). UDP datagrams can be dropped, duplicated, or reordered across networks; apps must handle that. Shared buffers / NAT bindings can interact badly with TCP on the same path — test real NAT, don’t assume LAN.

current kernel; `initcwnd` ≈ 10; window scaling on; avoid slow-start after idle when it hurts keep-alive; consider TFO only if you control both ends; compress and cut bytes; closer POPs (lower RTT); reuse connections; see [HPBN — TCP](https://hpbn.co/building-blocks-of-tcp/).

test behind NAT; cap send rate (often ≤ competing TCP); app-level congestion control; handle loss/dup/reorder/jitter; don’t blindly retransmit like TCP; keep UDP checksums; use ICE/STUN/TURN for peers.

**When to use:**

TCP — HTML/CSS/JS, files, HTTP/1.1 and HTTP/2, email, anything that must be complete and in order. UDP — DNS, streaming, WebRTC, HTTP/3 (QUIC adds reliability **on UDP**).

|                    | TCP                            | UDP                      |
| ------------------ | ------------------------------ | ------------------------ |
| Connection         | Handshake, then stream         | None                     |
| Delivery           | Reliable, ordered, no dupes    | Best-effort              |
| Congestion control | Yes                            | No (app or QUIC)         |
| Overhead           | Higher (state, ACKs)           | Low (8-byte header)      |
| Web                | HTTP/1.1, HTTP/2, TLS over TCP | DNS, WebRTC, HTTP/3/QUIC |

#### Common pitfalls

- Using `dns-prefetch` when you meant `preconnect`.
- Calling TFO “10 × 1460 bytes” — that is `initcwnd`, not SYN data.
- Assuming UDP “has no checksum” — enable it; required on IPv6.
- Treating HTTP/3 as “unreliable HTTP” — QUIC restores reliability on UDP.

**Pluses and minuses:**

| + (TCP)             | − (TCP)                                                     |
| ------------------- | ----------------------------------------------------------- |
| Integrity and order | Handshake + ACKs add RTT                                    |
| Congestion control  | Head-of-line blocking per connection (HTTP/2 still TCP HOL) |

| + (UDP)                   | − (UDP)                    |
| ------------------------- | -------------------------- |
| Low latency, small header | Loss, reorder, duplicates  |
| Good for real-time / QUIC | NAT/firewall pain; no cwnd |

**Examples:**

```html
<!-- Warm DNS+TCP+TLS to a CDN you will actually use (limit count) -->
<link rel="preconnect" href="https://cdn.example.com" crossorigin />
<link rel="dns-prefetch" href="https://analytics.example.com" />
```

```
HTTP/1.1 & HTTP/2  → TCP (+ TLS)
HTTP/3             → QUIC over UDP (crypto + 0-RTT in the transport)
WebRTC media       → UDP + ICE/STUN/TURN
```

**Thoughts:**

_TCP trades RTT and ACKs for a reliable ordered stream; UDP is a thin datagram — use it when latency beats completeness, or when QUIC implements reliability on top._

Layers → [OSI & TCP/IP models](#osi--tcpip-models). TLS after TCP → [SSL/TLS handshake](./6_web_api_http.md#ssltls--https-handshake). Hints → [preload, prefetch, preconnect](./6_web_api_security.md#preload-prefetch-preconnect--prerender) · [HTML resource hints](./1_html_structure.md#link-rel-types--resource-hints). Handshake-only recap → [TCP three-way handshake](./6_web_api_security.md#tcp-three-way-handshake). Sockets / `ss` → [Ports, sockets & network services](#ports-sockets--network-services). Path → [How the Internet works](./6_web_api_core.md#internet).

**Sources:**

[MDN — TCP](https://developer.mozilla.org/en-US/docs/Glossary/TCP) · [MDN — UDP](https://developer.mozilla.org/en-US/docs/Glossary/UDP) · [MDN — TCP handshake](https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake) · [MDN — preconnect](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/rel/preconnect) · [RFC 7413 TFO](https://datatracker.ietf.org/doc/html/rfc7413) · [RFC 6928 initcwnd](https://datatracker.ietf.org/doc/html/rfc6928) · [Doka — TCP и UDP](https://doka.guide/tools/tcp-udp/) (handshake/TFO numbers above supersede the 2022 article)

---

</details>

<details>
  <summary>Ports, sockets & network services</summary>

### Ports, sockets & network services

**Definition:** A **port** is a 16-bit number (1–65535) on a host that multiplexes connections so the kernel can hand packets to the right process. A **socket** is the OS endpoint (`IP + port + protocol`). The **network stack** (kernel) creates sockets; a **network service** is the application (nginx, `sshd`, Node) bound to one.

**How it works:**

- Browser default HTTPS is port **443** (`https://example.com` ≡ `https://example.com:443`). HTTP is **80**. IANA assigns well-known ports; local apps often use **8080** / **8000**.
- Other app protocols: mail **IMAP / POP3 / SMTP**; chat historically XMPP. The app protocol only defines the **message format**; TCP/UDP still carry the bytes.
- A 4-tuple (src IP, src port, dst IP, dst port) plus protocol identifies a connection. **Static routes** are configured; **dynamic** routes come from routing protocols. Hosts usually have a default gateway, not a full Internet map.
- **Inspect:** `ping` (ICMP echo — reachability, not “HTTP is up”). Addresses: `ip addr` (Linux) / `ifconfig` (BSD/macOS) / `ipconfig /all` (Windows). Connections: prefer **`ss`** on Linux (`net-tools` `netstat` is legacy). Packet bytes: [tcpdump / Wireshark](#osi--tcpip-models). Filter: [firewall](./15_devops.md#firewall--packet-filter).

**When to use:**

“Why is my Node on 3000 not opening in the browser?” (wrong bind / firewall). “Who is listening on 443?” Security review of listening sockets.

#### Common pitfalls

`netstat -L` / `-p tcp` from a macOS man page on Linux. `https://host:443` vs omitting the port. Treating `ping` success as “the website works.”

**Pluses and minuses:**

| +                                            | −                                                |
| -------------------------------------------- | ------------------------------------------------ |
| One port → one listening service             | Ephemeral client ports clutter `ss` output       |
| IANA defaults hide `:80` / `:443` in the URL | Flag names differ (`netstat -p` PID vs protocol) |

**Examples:**

```bash
ping 8.8.8.8                 # ICMP; Google DNS — not HTTP
ip addr                      # Linux addresses (prefer over ifconfig)
ifconfig                     # macOS / older Unix
ipconfig /all                # Windows

ss -tulpn                    # Linux: TCP/UDP, listening, numeric, processes
ss -s                        # protocol stats
netstat -an                  # numeric sockets (most OS)
netstat -p tcp               # macOS/BSD: filter TCP — NOT Linux (-p = PID/program)
```

**Thoughts:** _IP finds the host; the port finds the process; `ss`/`netstat` show the sockets the stack currently has._

**Sources:**

[IANA port registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) · [ss(8)](https://man7.org/linux/man-pages/man8/ss.8.html) · [ip(8)](https://man7.org/linux/man-pages/man8/ip.8.html) · [Doka — Working with the network](https://doka.guide/tools/network/) · [How the Internet works](./6_web_api_core.md#internet)

---

</details>

<details>
  <summary>GraphQL</summary>

### GraphQL

**Definition:**

GraphQL is a query language and runtime where clients request exactly the fields they need from a typed schema.

| REST                 | GraphQL                    |
| -------------------- | -------------------------- |
| Multiple endpoints   | Single `/graphql` endpoint |
| Fixed response shape | Client selects fields      |
| Over-fetching common | Precise data fetching      |

**How it works:** Client sends a query document against a schema (`type Query { book(id: ID!): Book }`). The server resolves each field. Mutations change data; subscriptions push updates (often over WebSocket). Use persisted queries and complexity limits in production.

**When to use:** Many clients needing different shapes of the same graph (mobile vs web). Informal **middle-end**: trim REST over-fetch without cloning endpoints — same idea as a [BFF](./7_special_architecture.md#bff-backend-for-frontend). Avoid as a cache-unfriendly replacement for a simple CRUD REST API.

**Pluses and minuses:**

| +                                                   | −                                               |
| --------------------------------------------------- | ----------------------------------------------- |
| Client picks fields; one round-trip for nested data | Caching and HTTP semantics are harder than REST |
| Strong schema / codegen (orval-like for GraphQL)    | N+1 resolvers without DataLoader                |
| Introspection and playground DX                     | Auth at field level is extra work               |

**Examples:**

```graphql
query GetBook($id: ID!) {
  book(id: $id) {
    title
    author {
      name
    }
  }
}
```

**Sources:**

[GraphQL — Introduction](https://graphql.org/learn/)

---

</details>

<a name="11-real-time--rendering"></a>

## 11. Real-time & rendering

<details>
  <summary>WebSocket</summary>

### WebSocket

**Definition:**

WebSocket upgrades an HTTP connection to a persistent full-duplex channel for real-time bidirectional messaging.

| HTTP polling      | WebSocket                      |
| ----------------- | ------------------------------ |
| Repeated requests | Single long-lived connection   |
| Higher latency    | Low latency push               |
| Simple            | Requires connection management |

**How it works:** Browser sends `Upgrade: websocket` (+ `Sec-WebSocket-*`). Server responds `101 Switching Protocols`. After that, frames go both ways (text or binary). Prefer `wss://` (TLS; proxies less likely to break the tunnel). Heartbeats detect dead peers. Scale with sticky sessions or a pub/sub bus (Redis) behind multiple Node processes.

- Events: `open`, `message`, `error`, `close`. Send: `socket.send(data)` (string / Blob / ArrayBuffer).
- `binaryType`: `'blob'` (default) or `'arraybuffer'`.
- Backpressure: check `socket.bufferedAmount` before flooding `send`.
- Close: `socket.close(code, reason)` — `1000` normal; `1006` means unclean (no close frame). `readyState`: 0 CONNECTING → 1 OPEN → 2 CLOSING → 3 CLOSED.
- No built-in reconnect/auth — libraries or manual retry.

**When to use:** Chat, live dashboards, collaborative cursors. Prefer SSE when the server only needs to **push** (notifications). Prefer HTTP for request/response CRUD.

**Pluses and minuses:**

| +                          | −                                                                         |
| -------------------------- | ------------------------------------------------------------------------- |
| Low-latency bidirectional  | Proxies, load balancers, and serverless timeouts fight long-lived sockets |
| Less overhead than polling | Must handle reconnect, backpressure, auth on the socket                   |

**Examples:**

```js
const ws = new WebSocket("wss://chat.example.com");
ws.binaryType = "arraybuffer";
ws.onopen = () => ws.send(JSON.stringify({ type: "join", room: "general" }));
ws.onmessage = (e) => console.log(e.data);
ws.onclose = (e) => console.log(e.code, e.wasClean);
```

**Thoughts:** Upgrade HTTP once, then speak frames — not a replacement for REST CRUD.

**Sources:**

[javascript.info/websocket](https://javascript.info/websocket) · [MDN — WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

---

</details>

<details>
  <summary>Polling, long polling, SSE & WebSocket</summary>

### Polling, long polling, SSE & WebSocket

**Definition:**

These patterns deliver server updates to clients with different trade-offs between simplicity, latency, and directionality.

| Technique         | Direction       | Connection           |
| ----------------- | --------------- | -------------------- |
| Short polling     | Client pulls    | Repeated HTTP        |
| Long polling      | Client pulls    | Held HTTP until data |
| SSE (EventSource) | Server → client | Persistent HTTP      |
| WebSocket         | Bidirectional   | Upgraded TCP         |

|           | WebSocket          | EventSource (SSE)    |
| --------- | ------------------ | -------------------- |
| Direction | Both ways          | Server → client only |
| Data      | Binary + text      | Text only            |
| Protocol  | `ws://` / `wss://` | HTTP                 |
| Reconnect | Manual             | Built-in             |

**How it works:**

- **Short polling:** client asks on an interval — simple, delayed, noisy when idle.
- **Long polling:** server holds the request until a message (or timeout/`502`); client reconnects immediately. Server must handle many pending connections.
- **SSE:** `Content-Type: text/event-stream`. Fields: `data:`, optional `id:` (→ `Last-Event-ID` on reconnect), `retry:` (ms), `event:` (custom name → `addEventListener`). Auto-reconnect unless `close()` or fatal status. Cross-origin needs CORS; credentials via `{ withCredentials: true }`.
- **WebSocket:** see [WebSocket](#websocket) above.

**When to use:** Rare pushes → long polling or SSE. High-frequency bidirectional → WebSocket. CRUD still HTTP.

**Examples:**

```js
// Server-Sent Events
const es = new EventSource("/api/events");
es.onmessage = (e) => console.log(e.data);
es.addEventListener("bye", (e) => console.log(e.data));
// es.close();
```

```js
// Long polling sketch
async function subscribe() {
  const res = await fetch("/subscribe");
  if (res.status === 502) return subscribe(); // timeout — reconnect
  if (!res.ok) {
    await new Promise((r) => setTimeout(r, 1000));
    return subscribe();
  }
  console.log(await res.text());
  return subscribe();
}
```

**Sources:**

[javascript.info/long-polling](https://javascript.info/long-polling) · [javascript.info/server-sent-events](https://javascript.info/server-sent-events) · [javascript.info/websocket](https://javascript.info/websocket) · [MDN — Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)

---

</details>

<a name="12-types-of-web-apps"></a>

## 12. Types of web apps

Same stack (HTML, CSS, JS). Organization differs: **MPA** vs **SPA**, then where HTML is built. History of payloads (full page → AJAX HTML snippets → JSON): [Evolution of web apps](#evolution-of-web-apps). Next.js/React details: [4_nextjs.md](./4_nextjs.md) · [4_react.md](./4_react.md).

<details>
  <summary>Types of web apps (MPA vs SPA)</summary>

### Types of web apps (MPA vs SPA)

**Definition:** Two main ways to organize a web app: **multi-page** (full reload on each link) and **single-page** (one document; JS owns screens and data). Each has subtypes.

**How it works:**

| Family  | Idea                                              | Pain                                        |
| ------- | ------------------------------------------------- | ------------------------------------------- |
| **MPA** | Many HTML pages linked together                   | Full refresh; no “app-like” scroll/nav      |
| **SPA** | One page; JS creates content, routes, fetches API | Bundle size, where to render, SEO, needs JS |

**When to use:** Landing / docs → MPA static or SSG. Forum / shop with server templates → MPA dynamic HTML. Dashboard with instant screens → SPA (often + SSR/SSG).

**Thoughts:** Pick by UX goal, not by fashion. Instant in-app nav wants SPA; simplest cacheable pages want static MPA.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/)

---

</details>

<details>
  <summary>MPA — static files</summary>

### MPA — static files

**Definition:** A set of ready HTML files (plus CSS/JS/images) served as-is by Nginx/Apache. Links cause a **full page reload**. Server config: [Nginx](./15_devops.md#nginx-configuration) · [Apache](./15_devops.md#apache-http-server).

**How it works:** Several `.html` files on disk. Optional: Pug (or similar) + Gulp/Rollup/Webpack to assemble, then upload static output. Browser, CDN, or Service Worker cache easily.

**When to use:** Company landing, product brochure, few mostly-static pages, little interaction.

**Pluses and minuses:**

| +                               | −                                                    |
| ------------------------------- | ---------------------------------------------------- |
| Simplest to build               | Hundreds/thousands of pages → unmaintainable by hand |
| Excellent cache/CDN performance | Changing data needs a rebuild or another approach    |
|                                 | Full reload on every click                           |

**Examples:**

```
/index.html
/about.html
/css/style.css
nginx → GET /about.html → file from disk
```

**Thoughts:** Ship HTML that already exists. No server script per request.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/)

---

</details>

<details>
  <summary>MPA — dynamic HTML</summary>

### MPA — dynamic HTML

**Definition:** On each request the server runs a script (PHP, Python, Ruby, …), reads a DB, fills a template, and returns HTML.

**How it works:** User opens “friends”: (1) request hits server (2) query DB (3) assemble HTML from parts (4) send HTML. Next visit with new friends → different HTML. Static fragments can still be cached. Client JS stays small.

**When to use:** Classic forums and shops. Large products historically used this (server-generated pages) before SPA shells.

**Pluses and minuses:**

| +                                         | −                                                    |
| ----------------------------------------- | ---------------------------------------------------- |
| Logic stays on the server; smaller client | Full reload — no instant “app” UX                    |
| Fresh data from DB                        | Scroll position lost; uncached pages wait on network |

**Thoughts:** HTML is assembled **per request** on the server — not a pre-made file.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/)

---

</details>

<details>
  <summary>SPA (Single Page Application)</summary>

### SPA (Single Page Application)

**Definition:** One HTML document. JavaScript creates content, switches screens, and loads data (usually via API). Navigation does not reload the whole page.

**How it works:** Instant screen changes; can show loaders while `fetch` runs. Rich ecosystem (UI libs, patterns). Split by **where the first paint is built**: browser (**CSR**) or server (**SSR**). Often needs frameworks.

**When to use:** Complex client logic, app-like UX. Cached SPA JS can still change screens on a bad network; MPA waits for the next HTML.

**Pluses and minuses:**

| +                                | −                                                                          |
| -------------------------------- | -------------------------------------------------------------------------- |
| Instant in-app transitions       | Harder: tools, bundles, performance                                        |
| Visual feedback while data loads | Needs JS — disabled JS = blank                                             |
|                                  | Crawlers prefer static HTML (they _can_ run JS now, but static still wins) |

**Thoughts:** The page stays; JS paints the next screen.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/) · [MDN — SPA](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

---

</details>

<details>
  <summary>CSR (Client-Side Rendering)</summary>

### CSR (Client-Side Rendering)

**Definition:** The **browser** builds every view, including the first. User waits for JS; until then the page is empty or a static loader.

**How it works:** Download JS → execute → render. Simplest SPA. Fine when the bundle is small. Weak devices / slow nets may bounce. Search engines execute JS today, but static HTML still indexes more reliably.

**When to use:** Small SPAs, auth dashboards with low SEO need.

**Pluses and minuses:**

| +                             | −                                |
| ----------------------------- | -------------------------------- |
| Simple SPA setup              | First paint blocked on JS        |
| Fast start if the app is tiny | No JS → nothing                  |
|                               | Large apps: slow TTI, weaker SEO |

**Thoughts:** First HTML is a shell; UI appears after JS.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/) · [Next.js rendering](./4_nextjs_rendering.md#1-rendering-strategies)

---

</details>

<details>
  <summary>SSR (Server-Side Rendering)</summary>

### SSR (Server-Side Rendering)

**Definition:** The **server** sends already-rendered HTML for the first view so the user can read while the SPA JS downloads and hydrates.

**How it works:** Node (typical) runs the same UI on the server. Next.js (React) / Nuxt (Vue) are the usual starters — see [4_react_server.md §8](./4_react_server.md#8-server-rendering--rsc), [4_vue_ssr.md §8](./4_vue_ssr.md#8-ssr--hydration), [4_nextjs_rendering.md §1](./4_nextjs_rendering.md#1-rendering-strategies), [4_nuxt_core.md §4](./4_nuxt_core.md#4-rendering-strategies). Custom SSR is hard (data loading, correct render, infra). Extra server = extra ops.

**When to use:** SEO + first paint that must not wait on the bundle. Personalized pages (session) — see Next.js `cookies()` / `force-dynamic`.

**Pluses and minuses:**

| +                                      | −                      |
| -------------------------------------- | ---------------------- |
| HTML immediately; better UX and search | Server + infra         |
| Same app can hydrate into SPA          | DIY SSR is error-prone |

**Examples:**

```ts
export const dynamic = "force-dynamic";
```

**Thoughts:** First bytes are real content, not an empty shell.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/) · [Next.js — rendering](https://nextjs.org/docs/app/getting-started/caching)

---

</details>

<details>
  <summary>SSG (Static Site Generation)</summary>

### SSG (Static Site Generation)

**Definition:** Build step turns data + templates into **static HTML files** (an MPA on disk). SSG tools often wrap an SPA framework so you get static files _and_ client interactivity.

**How it works:** Blog posts → HTML pages at build. Doka itself is SSG. **Jamstack** = JavaScript + APIs + Markup (Doka: Markdown as the typical content source). React/etc. “wake up” the page; APIs fetch extra data.

**When to use:** Blogs, docs, marketing. Next.js: `generateStaticParams` prerenders dynamic routes at build.

**Pluses and minuses:**

| +                    | −                                |
| -------------------- | -------------------------------- |
| Static + SPA UX      | Frozen until next build (or ISR) |
| CDN cache, great SEO | Not for per-user dashboards      |

**Examples:**

```ts
export async function generateStaticParams() {
  const posts = await fetch("https://example.com/posts").then((r) => r.json());
  return posts.map((post: { slug: string }) => ({ slug: post.slug }));
}
```

**Thoughts:** HTML exists **before** the request (build time), unlike classic PHP-per-request MPA.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/) · [Next.js — generateStaticParams](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

---

</details>

<details>
  <summary>PWA (Progressive Web App)</summary>

### PWA (Progressive Web App)

**Definition (three angles):**

| Kind          | Meaning                                                                                                                                                     |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Everyday**  | A site you can pin as an icon; launched in **standalone** (no browser chrome), can work offline, get push, etc. — looks like a native app.                  |
| **Official**  | A web app that uses **progressive enhancement**: feature-detect Web APIs and, if present, use them for native-class UX. That is why they are _progressive_. |
| **Technical** | A site that meets **browser-specific install criteria** (they change). Not one global spec checklist.                                                       |

**How it works — Chromium install (Chrome/Edge/…):** HTTPS or `localhost` + a **web app manifest**. Chromium **used to** also require a Service Worker; that is **not** required for the install prompt anymore. SW is still how you get offline, push, background sync, and intercepting `fetch`. Safari/Firefox typically **do not** show Chromium’s “Install app” UI. Apple calls these **Home Screen Web Apps**, not “PWA.”

Chrome `beforeinstallprompt` ([web.dev](https://web.dev/articles/install-criteria)): not already installed; HTTPS; user engagement (~30s + a tap); manifest with `name` or `short_name`, `start_url`, `icons` (192 + 512), `display` in `fullscreen` / `standalone` / `minimal-ui` / `window-controls-overlay`; `prefer_related_applications` absent or `false`.

**History (short):** Term coined **2015** by Alex Russell and Frances Berriman (_Progressive Web Apps: Escaping Tabs Without Losing Our Soul_). Idea older: Palm webOS, Firefox OS, ChromeOS; Jobs (2007) described web-delivered apps before the App Store. Community PWA logo: Diego Gonzalez (2017 contest).

**When to use:** Layer on any MPA/SPA. Gradual: add manifest first, then SW for offline. Optional: wrap in a thin native shell for Play/App Store.

**Notes (Doka):** Install and offline are **not** required to “count as PWA.” A tab-only site with push/sensors is still progressive. Video-call apps need a network. PWA is an umbrella for practices + optional install rules — not a strict product type.

| Pros                                                                                | Cons                                                                   |
| ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| One codebase; any form factor with a browser                                        | Same “decentralized” web: **no big PWA store** for ratings/screenshots |
| Native-ish: home-screen icon, OS handlers, offline, push, sensors (when APIs exist) | **Uneven Web API support** across browsers, versions, and OS           |
| Cache = offline + faster repeats; Background Sync when back online                  | iOS: all browsers use **WebKit**; APIs lag or missing until OS update  |
| Visit the URL — no store; cache can update in the background                        | “True cross-platform” is limited by the weakest browser                |
| Smaller than native: call **browser** APIs, load only what you need                 |                                                                        |
| Searchable URLs, share links, copy/translate/zoom/print/extensions                  |                                                                        |
| Browser sandbox + HTTPS; **permission prompts** (push, NFC, …)                      |                                                                        |
| HTML/CSS/JS — no proprietary SDK; debug in DevTools                                 |                                                                        |
| Web stays backward compatible                                                       |                                                                        |

**Examples:**

```html
<link rel="manifest" href="/manifest.json" />
```

```json
{
  "name": "My App",
  "short_name": "App",
  "start_url": "/",
  "display": "standalone",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("/sw.js");
}
```

**Common pitfalls:** Treating SW as required for _install_ (outdated). Expecting Chrome’s install banner in Safari. Caching forever without a versioned SW activate step.

**Thoughts:** PWA = website + optional native capabilities. Install ≠ offline.

**Sources:** [Doka — PWA](https://doka.guide/tools/pwa/) · [web.dev — install criteria](https://web.dev/articles/install-criteria) · [MDN — Progressive Web Apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)

---

</details>

<details>
  <summary>Compare: MPA, SPA, CSR, SSR, SSG, PWA</summary>

### Compare: MPA, SPA, CSR, SSR, SSG, PWA

**Definition:**

Choose by **reload vs JS routing** (MPA/SPA) and **when HTML is built** (CSR/SSR/SSG). PWA is an enhancement, not a fifth architecture.

|                 | First view           | Navigation             | SEO       | Offline        |
| --------------- | -------------------- | ---------------------- | --------- | -------------- |
| **MPA static**  | Files on disk        | Full reload            | Excellent | CDN / SW       |
| **MPA dynamic** | Server template + DB | Full reload            | Good      | Harder         |
| **SPA + CSR**   | After JS             | Instant                | Weaker    | Limited        |
| **SPA + SSR**   | HTML then hydrate    | Instant after boot     | Strong    | Limited        |
| **SSG**         | Build-time HTML      | Reload or hydrated SPA | Excellent | Static cache   |
| **PWA**         | Depends on base      | Depends on base        | Varies    | Service worker |

**When to use:** Bad network: SPA can still change screens from cached JS; MPA waits for the next HTML. SPA can still be slow if the bundle is huge.

**Thoughts:** MPA vs SPA is navigation. CSR/SSR/SSG is _when_ HTML exists.

**Sources:**

[Doka — Types of web apps](https://doka.guide/tools/web-app-types/)

---

</details>
