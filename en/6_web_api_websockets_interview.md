# WebSockets — Interview

Answers are filled for the full question list.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Apps](./6_web_api_apps.md) · [WebSockets — Interview](./6_web_api_websockets_interview.md) · [PWA — Interview](./6_web_api_pwa_interview.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/).

### WebSockets Fundamentals

<details>
<summary>What is WebSocket protocol and how does it differ from HTTP?</summary>

### What is WebSocket protocol and how does it differ from HTTP?

**Definition:**

### WebSocket vs. HTTP: 2026 Technical Audit

**WebSocket** (RFC 6455) and **HTTP/3** (RFC 9114) represent distinct paradigms in transport and application-layer communication. While HTTP/3 over QUIC has narrowed the latency gap for request-response cycles, WebSocket remains the standard for stateful, persistent, full-duplex streams.

**How it works:**

### Core Differences

* **Communication Model**: HTTP is inherently **request-response**. Even with HTTP/2 and HTTP/3 multiplexing, the client dictates data exchange. WebSockets implement **Full-Duplex** communication over a single TCP/TLS connection, allowing either party to push data asynchronously.
* **Connection Lifecycle**: HTTP is primarily **stateless**. While HTTP/3 maintains connection persistence (0-RTT), it remains transactional. WebSockets undergo an **HTTP 101 Switching Protocols** handshake, upgrading the connection to a persistent binary stream that remains open until explicitly terminated.
* **Protocol Overhead**: HTTP/1.1 requires repetitive header transmission. While HTTP/2+ uses **HPACK/QPACK** header compression to mitigate this, WebSockets outperform in high-frequency, low-payload scenarios because frames after the handshake incur only a **2-to-14 byte overhead**.
* **Data Framing**: WebSockets utilize a distinct framing protocol that allows interleaved control frames (ping/pong) and data frames (text/binary) without re-initiating a request cycle.

### Operation Mechanism

* **HTTP**: Operates on a transactional basis. The client sends a request; the server processes the state and returns a response. Modern HTTP/3 utilizes **QUIC** (UDP-based) to solve head-of-line blocking at the transport layer, significantly improving performance for traditional web content.
* **WebSockets**: Leverages a long-lived TCP connection. By maintaining the pipe, the server avoids the overhead of repeated TLS handshakes for every message. This reduces the latency of message delivery to `O(1)` post-handshake, making it deterministic compared to the jitter inherent in sequential HTTP requests.

### Protocol Stack Integration

* **HTTP**: Sits at the **Application Layer** (Layer 7). HTTP/3 leverages QUIC, which integrates transport layer features into the protocol stack.
* **WebSockets**: Initiates via an **HTTP/1.1 upgrade header**, then transitions to a custom protocol implementation. In 2026, **WebTransport** (built on HTTP/3) is increasingly used for use cases requiring unreliable or unordered data streams, though WebSocket remains the ubiquitous choice for reliable ordered communication.

### Best-Candidate Use Cases

* **HTTP/3**: Optimal for document fetching, RESTful APIs, and idempotent data retrieval where the overhead of maintaining state is unnecessary.
* **WebSockets**: Mandatory for low-latency, state-dependent environments:
 * **Real-time Collaborative Engines**: Multi-user editing (e.g., CRDT-based synchronization).
 * **Financial Market Data**: High-frequency ticker updates where every millisecond of latency reduction is critical.
 * **Interactive Gaming**: Managing synchronized game-state updates across high-concurrency client sessions.

### 2026 Modernization Note: WebSocket vs. WebTransport
While WebSockets remain standard for reliable streams, **WebTransport API** is now preferred for low-latency media streaming and advanced gaming, as it allows developers to choose between reliable and unreliable data delivery over HTTP/3, mitigating the "head-of-line blocking" issues inherent in the TCP-based WebSocket protocol.

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>Explain how the WebSocket handshake works.</summary>

### Explain how the WebSocket handshake works.

**Definition:**

### WebSocket Handshake Architecture (2026 Update)

The **WebSocket handshake** transitions a standard HTTP/1.1 request into a persistent, **full-duplex** binary framing protocol (RFC 6455). This mechanism facilitates low-latency communication by bypassing the overhead of repetitive HTTP request-response cycles.

**How it works:**

#### 1. Client Initiation
The client initiates the upgrade via an HTTP GET request. In 2026, clients must include the `Sec-WebSocket-Version: 13` header, which remains the definitive version for global interoperability.

#### 2. Server Response and Protocol Switching
The server inspects the request. Upon validation, it returns an **HTTP 101 Switching Protocols** status. The connection remains open, shifting the underlying socket from textual HTTP parsing to the **WebSocket Frame Protocol**.

#### 3. Cryptographic Handshake Integrity
To prevent caching proxies from incorrectly identifying the connection and to confirm that the server supports the protocol, the server processes the `Sec-WebSocket-Key` using the **GUID** `258EAFA5-E914-47DA-95CA-C5AB0DC85B11`.

**Modernized Node.js (v22+) Implementation:**

*Note:* While SHA-1 is cryptographically weak for digital signatures, it is utilized here strictly for **handshake obfuscation** and protocol validation; it remains the standard per RFC 6455.

#### 4. Post-Handshake Full-Duplex Framing
Once the handshake is complete, data is exchanged via **frames**. These frames introduce a minimal header (2–14 bytes), significantly reducing overhead compared to standard HTTP/2 or HTTP/3 headers.

* **Complexity:** The frame parsing complexity is `O(1)` relative to payload size, as the header structure is fixed-length once the payload length is determined.
* **Persistent State:** The TCP connection remains active until a `Close` control frame (opcode `0x8`) is received or a network timeout occurs.
* **Security (2026 Best Practice):** Always mandate **WSS (WebSocket Secure)** using **TLS 1.3** to ensure data integrity and confidentiality. Do not use unencrypted `ws://` in production environments.

**Examples:**

```http
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: <base64-encoded-nonce>
Sec-WebSocket-Version: 13
Origin: https://example.com
```

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: <computed-hash>
```

```javascript
import { createHash } from 'node:crypto';

const MAGIC_STRING = '258EAFA5-E914-47DA-95CA-C5AB0DC85B11';

/**
 * Computes the Sec-WebSocket-Accept header value.
 * @param {string} clientKey - Provided via Sec-WebSocket-Key header
 */
function computeAcceptHeader(clientKey) {
    return createHash('sha1')
        .update(clientKey + MAGIC_STRING)
        .digest('base64');
}
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>What are some common use cases for WebSockets?</summary>

### What are some common use cases for WebSockets?

**Definition:**

### WebSockets (RFC 6455) Modernized Audit

**WebSockets** provide a persistent, full-duplex communication channel over a single TCP connection. In 2026, they remain the standard for low-latency, stateful communication, though often abstracted by higher-level protocols like **WebTransport** or **gRPC-Web** in specific enterprise architectures.

**How it works:**

### High-Frequency Use Cases

* **Collaborative Productivity (CRDT-based)**: Real-time conflict resolution using **Conflict-free Replicated Data Types (CRDTs)** for multi-user editing environments (e.g., decentralized text editors, Figma-like canvas engines).
* **Edge-Compute IoT Telemetry**: Bi-directional streaming for low-latency command-and-control loops in robotics and industrial IoT, typically leveraging **MessagePack** or **Protobuf** serialization to minimize payload overhead.
* **High-Frequency Trading (HFT) Interfaces**: Real-time order book synchronization. WebSockets remain preferred over **Server-Sent Events (SSE)** when bi-directional signaling (e.g., order cancellation) is required.
* **Live Multi-User Gaming**: State synchronization for game servers. While **WebRTC** is preferred for peer-to-peer data channels, WebSockets handle server-authoritative state broadcasting for massive concurrent sessions.
* **AI-Agent Orchestration**: Streaming token-by-token responses from LLM backends to frontend interfaces, minimizing Time to First Token (TTFT).

### Technical Limitations & 2026 Alternatives
* **WebTransport (HTTP/3)**: For scenarios requiring **unreliable** delivery (datagrams), WebTransport is now the preferred successor to WebSockets, as it avoids Head-of-Line (HOL) blocking inherent in TCP.
* **Scalability**: WebSocket connections are stateful and consume memory per socket (`O(n)` where `n = connected clients`). Modern implementations utilize **Redis Pub/Sub** or **NATS** as a message broker to decouple WebSocket gateway servers from application logic.

### 2026 Implementation Standard (Modernized)

Modern implementations utilize **TypeScript** for type safety across the wire, often leveraging **Zod** for schema validation of incoming JSON payloads to prevent injection attacks or malformed data state errors.

### Performance Metrics
* **Latency**: Typical round-trip time (RTT) for WebSockets is significantly lower than polling (`O(RTT)` vs `O(RTT + polling interval)`).
* **Memory Footprint**: Each open socket consumes approximately 10KB–50KB of RAM on the server side, necessitating efficient **load balancing** (e.g., NGINX, HAProxy) using sticky sessions if state is maintained at the application layer.

**Examples:**

```typescript
// Modernized WebSocket implementation with Type Safety
interface MarketUpdate {
  symbol: string;
  price: number;
  timestamp: number;
}

const socket = new WebSocket('wss://api.exchange.com/v2/stream');

socket.addEventListener('open', () => {
  const subscribeMsg = JSON.stringify({ action: 'subscribe', pair: 'BTC-USD' });
  socket.send(subscribeMsg);
});

socket.addEventListener('message', (event: MessageEvent<string>) => {
  try {
    const data: MarketUpdate = JSON.parse(event.data);
    // UI thread processing using requestAnimationFrame for 60/120fps smoothness
    requestAnimationFrame(() => updateDashboard(data));
  } catch (err) {
    console.error('Schema validation failed:', err);
  }
});

function updateDashboard(data: MarketUpdate): void {
  const el = document.getElementById(`price-${data.symbol}`);
  if (el) el.textContent = data.price.toFixed(2);
}
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>What are the limitations of WebSockets?</summary>

### What are the limitations of WebSockets?

**Definition:**

### Limitations of WebSockets in 2026

**WebSockets (RFC 6455)** remain the industry standard for full-duplex communication. However, the shift toward **HTTP/3 (QUIC)** and serverless paradigms necessitates a re-evaluation of their role in modern architectures.

**How it works:**

#### Connection Lifecycle & Resource Management
* **Persistent Resource Consumption**: Unlike the request-response model of HTTP/1.1 or the multiplexed streams of HTTP/2 and HTTP/3, WebSockets maintain stateful TCP connections. Each connection consumes a socket file descriptor and memory on the server. At scale, this leads to `O(N)` memory complexity where `N` is the number of concurrent users, complicating horizontal scaling without an external message broker (e.g., Redis Pub/Sub, NATS).
* **State Persistence & Serverless**: WebSocket persistence is fundamentally incompatible with the ephemeral nature of **Serverless Functions (FaaS)**. Solutions like *AWS API Gateway WebSocket APIs* or *Azure Web PubSub* effectively abstract this, but they introduce vendor lock-in and additional latency overhead for the handshake/upgrade process.

#### Infrastructure & Network Constraints
* **Middlebox Interference**: While HTTP/3 over QUIC bypasses TCP head-of-line blocking, WebSockets are bound to TCP. Older firewalls, Deep Packet Inspection (DPI) proxies, and restrictive corporate gateways often terminate or drop long-lived WebSocket connections, necessitating aggressive heartbeat/keep-alive strategies.
* **Load Balancing Complexity**: Conventional Layer 7 load balancers require "sticky sessions" or WebSocket-aware routing to maintain the stateful pipe. In contrast, modern **Service Meshes (Istio/Linkerd)** handle this, but misconfiguration often leads to silent connection drops during deployment rolling updates.

#### Protocol Efficiency vs. Alternatives
* **HTTP/3 and WebTransport**: **WebTransport (RFC 9218)** is the 2026 successor to WebSockets for high-throughput, low-latency requirements. It provides unreliable (datagram-based) and reliable stream-based communication over HTTP/3, mitigating head-of-line blocking issues present in WebSocket’s TCP foundation.
* **Overkill for Simple Updates**: For unidirectional "server-to-client" streams, **Server-Sent Events (SSE)** remain superior. SSE is lighter, operates over standard HTTP/2 streams, and features automatic reconnection natively, reducing the boilerplate code required for WebSockets.

#### Security & Compliance
* **Authentication Fragility**: WebSockets do not support standard HTTP headers (like `Authorization: Bearer `) during the `101 Switching Protocols` handshake in browser environments. Developers must often pass tokens as query parameters, which are susceptible to leakage in server logs and browser history. **Subprotocol negotiation** or cookie-based auth is preferred, but requires careful CSRF mitigation.
* **Privacy Regulation (GDPR/CCPA)**: Persistent connections make "Right to be Forgotten" requests more complex. Real-time data streams must be actively tracked in memory to ensure immediate termination upon user request or session expiration to maintain compliance.

#### Mobile & Connectivity
* **Battery and Radio State**: Maintaining a persistent TCP connection keeps the mobile radio in a high-power state longer. In regions with unstable 5G/6G signals, frequent reconnection cycles (the "thundering herd" problem) can significantly degrade mobile device battery life compared to HTTP-based polling or Push Notifications (APNs/FCM).

#### Summary Table: 2026 Protocol Comparison

| Feature | WebSockets | HTTP/3 (WebTransport) | SSE |
| :--- | :--- | :--- | :--- |
| **Transport** | TCP | QUIC | HTTP/2+ |
| **Full-Duplex** | Yes | Yes | No |
| **Complexity** | Moderate | High | Low |
| **Use Case** | Real-time gaming/Chat | High-perf streaming | Real-time dashboards |

#### Recommendation
For new greenfield projects in 2026:
1. Use **SSE** for uni-directional server-to-client updates.
2. Use **WebTransport** for high-performance, low-latency bidirectional requirements.
3. Use **WebSockets** only when legacy browser compatibility (pre-2023 environments) or broad ecosystem library support is a critical business constraint.

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>Can you describe the WebSocket API provided by HTML5?</summary>

### Can you describe the WebSocket API provided by HTML5?

**Definition:**

### Technical Audit: WebSocket API (2026 Standards)

**WebSocket** is a stateful, **full-duplex** communication protocol standardized in **RFC 6455**. It operates over a single, persistent TCP connection, bypassing the overhead of HTTP/1.1 request-response cycles. While originally introduced as part of the HTML5 suite, it is now an independent IETF standard.

**How it works:**

### Key Aspects of WebSockets

- **Protocol Upgrade**: Initiated via an **HTTP/1.1 GET request** featuring specific `Upgrade: websocket` and `Sec-WebSocket-Key` headers. Post-handshake, the connection upgrades to a binary framing layer.
- **Bi-directional Framing**: Data is partitioned into **frames** (text, binary, ping, pong, close). Framing allows for fragmented message delivery, maintaining `O(1)` overhead per frame header compared to HTTP overhead.
- **Browser Native Stack**: Fully integrated into the **Web API** (WHATWG Living Standard). Modern architectures prefer `wss://` (WebSocket Secure) to ensure TLS encryption, mandatory for production deployment.

### WebSocket Lifecycle

1. **The Handshake**: Client sends an HTTP GET request with `Upgrade` headers. Server validates via `Sec-WebSocket-Accept` (SHA-1 hashing of the client's key).
2. **Persistent State**: Unlike HTTP/3 (QUIC-based), which optimizes streams, WebSocket provides a raw, long-lived pipe. For load balancing, **sticky sessions** or **Redis-based pub/sub** backplanes are required for horizontal scaling.
3. **Data Transmission**: Messages are sent as continuous frames. Binary data (e.g., `ArrayBuffer`, `Blob`) is natively supported, making it superior to HTTP for high-frequency binary data (e.g., WebRTC signaling, binary serialization like Protobuf).
4. **Termination**: Controlled shutdown via `CloseFrame` (opcode `0x8`). Abrupt network failure triggers an `onerror` event followed by `onclose`.

### 2026 Modern Considerations

- **WebSocket vs. WebTransport**: For 2026-era high-performance needs, **WebTransport** (built on HTTP/3) is preferred over WebSockets for scenarios requiring unreliable delivery or multi-streaming without head-of-line blocking.
- **Resource Management**: WebSocket connections consume server memory per-socket. Audit implementations for **backpressure** support to prevent buffer overflows during high-throughput bursts.

### Revised Code Example (ES2026+)

### Key Use-Cases

- **Real-Time Data Streaming**: Financial tickers, live analytics.
- **Collaborative CRDTs**: Maintaining shared state (e.g., Yjs/Automerge) in multi-user document editors.
- **Signaling Layer**: Providing the "handshake" mechanism for **WebRTC** Peer-to-Peer connections.
- **IoT Telemetry**: Low-latency sensor data ingestion with minimal framing latency.

**Examples:**

```javascript
// Using modern Class-based architecture and Optional Chaining
class SocketClient {
  constructor(url) {
    this.socket = new WebSocket(url);
    this.init();
  }

  init() {
    this.socket.addEventListener('open', () => console.log('Connected'));
    this.socket.addEventListener('message', ({ data }) => this.handle(data));
    this.socket.addEventListener('error', (err) => console.error(err));
  }

  handle(data) {
    // Handling Blob/ArrayBuffer natively
    if (data instanceof Blob) {
      data.text().then(text => console.log('Received:', text));
    }
  }

  send(payload) {
    if (this.socket.readyState === WebSocket.OPEN) {
      this.socket.send(payload);
    }
  }
}

const client = new SocketClient('wss://api.example.com/v2/stream');
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>Explain the WebSocket frame format.</summary>

### Explain the WebSocket frame format.

**Definition:**

### WebSocket Frame Architecture (RFC 6455 Refined)

The **WebSocket frame** protocol enables full-duplex communication over a single TCP connection. Modern implementations (2026+) prioritize high-throughput data framing and strict security hygiene, particularly regarding masking and frame fragmentation.

**How it works:**

#### Initial Header and Control Bits

Every frame begins with a fixed 2-byte sequence. The first byte identifies the fragment state and opcode, while the second byte dictates the **Mask** bit and initial payload length.

* **FIN (1 bit):** Indicates the final fragment of a message.
* **RSV1-3 (3 bits):** Reserved for protocol extensions (e.g., per-message `deflate` compression). Must be `0` unless an extension is negotiated.
* **OPCODE (4 bits):**
 * `0x0`: Continuation
 * `0x1`: Text (UTF-8)
 * `0x2`: Binary
 * `0x8`: Connection Close
 * `0x9`: Ping
 * `0xA`: Pong
* **MASK (1 bit):** If set to `1`, a `32`-bit masking key follows. Per RFC 6455, clients **must** mask all frames sent to servers. Servers **must not** mask frames sent to clients.

#### Extended Header Logic

The payload length field (7 bits) determines the necessity of extended length bytes:
* **0–125:** The value is the payload length.
* **126:** The following `16` bits (`2` bytes) represent the payload length (unsigned big-endian).
* **127:** The following `64` bits (`8` bytes) represent the payload length.

#### Payload and Masking

The payload data consists of the **Extension Data** (if any) followed by the **Application Data**. 

**Masking Mechanics:**
If the mask bit is set, the payload is XORed with a `32`-bit masking key (`masking_key`). The transformation for the `i`-th byte of data (`transformed_octet_i`) is:

`transformed_octet_i = original_octet_i XOR masking_key[i mod 4]`

This mechanism provides no cryptographic security; it exists exclusively to prevent cross-protocol attacks on intermediary proxies that might otherwise interpret malicious binary data as legitimate HTTP requests.

#### Example Interpretation (2026 Context)

Consider a binary frame: **FIN=1, Opcode=0x2 (Binary), Mask=0, Payload=75 bytes of 'A' (0x41).**

1. **Byte 0:** `10000010` (`0x82`). (FIN set, Binary opcode).
2. **Byte 1:** `01001011` (`0x4B`). (Mask bit 0, Length 75).
3. **Payload:** `75` bytes of `0x41`.

**Modern Implementation Note:** In 2026 production environments (e.g., using Node.js `ws` or Python `websockets` libraries), developers rarely interact with raw frames. High-level abstractions handle fragmentation and masking internally. Audits should focus on the **RSV1 bit** usage, as modern performance-critical applications heavily utilize the `permessage-deflate` extension (`RSV1=1`) to reduce bandwidth by `O(n)` relative to compression ratios.

**Examples:**

```plaintext
 0                   1
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
+-+-+-+-+-------+---------------+
|F|R|R|R| opcode|M| Payload len |
|I|S|S|S|  (4)  |A|     (7)     |
|N|V|V|V|       |S|             |
+-+-+-+-+-------+---------------+
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>How do WebSockets handle communication through proxies and firewalls?</summary>

### How do WebSockets handle communication through proxies and firewalls?

**Definition:**

### WebSockets and Network Intermediaries (2026 Standards)

**WebSockets** (RFC 6455) provide persistent, full-duplex communication channels over a single TCP connection. In 2026, architectural designs must account for the prevalence of **HTTP/3 (QUIC)** and sophisticated **Layer 7 (L7) firewalls**.

**How it works:**

### Proxies and Intermediaries

* **HTTP/1.1 Upgrade Mechanism**: WebSockets initiate via an HTTP `GET` request containing the `Upgrade: websocket` and `Connection: Upgrade` headers. Older, non-WebSocket-aware proxies often fail to propagate these headers, leading to a `400 Bad Request` or `502 Bad Gateway`. 
* **Protocol Negotiation**: Modern proxies act as **WebSocket-aware gateways**. They participate in the HTTP handshake to maintain statefulness. If an intermediary does not explicitly support the `Upgrade` mechanism, it will drop the connection.
* **Connection Multiplexing and HTTP/2+**: While HTTP/1.1 proxies struggle with long-lived WebSocket streams (causing timeout-induced closures), **HTTP/2 and HTTP/3 (QUIC)** proxying handles multiplexing more efficiently. However, WebSocket-over-HTTP/2 (RFC 8441) is the required standard for modern infrastructure to prevent head-of-line blocking at the proxy level.

### Firewalls and Content Inspection

* **Port Utilization**: Industry standards mandate WebSocket traffic over **WSS (WebSocket Secure)** using port **443**. Firewalls configured for HTTPS traffic generally permit WSS traffic as the handshake and subsequent framing are opaque to stateful inspection engines once the TLS tunnel is established.
* **Deep Packet Inspection (DPI)**: Sophisticated L7 firewalls perform **TLS Termination**. By decrypting traffic, these firewalls can inspect individual WebSocket frames. If the WebSocket payload violates security policies (e.g., unauthorized protocol tunneling), the firewall will send a `TCP RST` to terminate the connection.
* **Idle Timeout Policies**: Firewalls often terminate TCP connections that appear idle. Modern WebSocket implementations must utilize **Ping/Pong control frames** (every 30–60 seconds) to maintain state and prevent silent session teardown by middleboxes.

### Addressing Challenges: 2026 Best Practices

* **Encapsulation via TLS**: TLS 1.3 is the mandatory transport layer. It provides **Forward Secrecy**, rendering content inspection significantly more difficult for firewalls unless they perform man-in-the-middle (MITM) proxying with enterprise-trusted root certificates.
* **Fallback Strategies**: In high-restriction environments, **WebSockets-over-HTTPS** acts as the baseline. If connectivity fails due to protocol-specific blocking, applications should fall back to **WebTransport** (via HTTP/3), which provides low-latency communication that is natively compatible with modern QUIC-based network infrastructure.

### Modern Implementation (Python 3.14+)

In 2026, `websockets` (asyncio-native) is the preferred library. It handles frame-level management and heartbeat (ping/pong) mechanisms automatically, crucial for maintaining proxy transparency.

### Technical Audit Summary
* **Deprecation Warning**: Avoid manual `websocket-client` (threading-based) in high-concurrency environments; use `asyncio` for performance efficiency.
* **Standard Compliance**: Ensure all production endpoints support **RFC 8441** for seamless traversal through HTTP/2-enabled proxies.
* **Complexity**: WebSocket connection establishment involves an `O(1)` handshake transition, but maintaining state in NAT environments requires `O(n)` heartbeats over the connection lifetime.

**Examples:**

```python
import asyncio
import websockets
import ssl

# Define SSL context for secure TLS 1.3 tunneling
ssl_context = ssl.create_default_context()

async def connect_proxy():
    uri = "wss://api.example.com/v1/socket"
    
    # Standard 2026 approach: Use context manager for robust lifecycle
    async with websockets.connect(
        uri, 
        ssl=ssl_context,
        ping_interval=30,  # Prevent firewall idle timeouts
        ping_timeout=10
    ) as websocket:
        
        await websocket.send("Client-Hello")
        response = await websocket.recv()
        print(f"Received: {response}")

# Execute within asyncio event loop
if __name__ == "__main__":
    asyncio.run(connect_proxy())
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>What are the security considerations when using WebSockets?</summary>

### What are the security considerations when using WebSockets?

**Definition:**

### Security Considerations for WebSocket Architecture (2026)

Persistent connections shift the security boundary from request-based validation to session-state validation. Modern implementations must account for long-lived transport layers and the mitigation of asymmetric resource consumption.

**How it works:**

#### Key Security Concerns

* **Cross-Site WebSocket Hijacking (CSWSH)**: Unlike standard CORS, the WebSocket handshake does not automatically enforce same-origin policies. The `Origin` header must be strictly validated against a whitelist of trusted schemes and hostnames. Failure to validate the `Origin` allows cross-site scripts to initiate unauthorized persistent connections.
* **Input Sanitization (Protocol-Level)**: WebSockets transport raw binary or text frames. Since the persistent nature allows for high-frequency streaming, perform **schema validation** (e.g., using Protocol Buffers or JSON Schema) at the application layer to prevent injection attacks (SQLi, Command Injection) that bypass traditional HTTP middleware.
* **Resource Exhaustion & Rate Limiting**: Persistent connections bypass standard per-request firewall rules. Implement **backpressure management** and concurrency limits per IP address to mitigate `O(n)` connection-exhaustion DDoS attacks. Enforce idle timeouts to prevent "zombie" connections from consuming file descriptors.
* **Transport Layer Security (TLS)**: Always utilize `wss://` (WebSocket Secure). Standard `ws://` connections are susceptible to Man-in-the-Middle (MitM) attacks and packet sniffing. TLS 1.3 is the mandated baseline in 2026.
* **Authentication & Session Management**: Authentication should occur during the initial HTTP handshake (e.g., via Secure/HttpOnly Cookies or short-lived Bearer tokens). Once established, the server must map the persistent connection to an authenticated identity, ensuring that subsequent messages within the stream are strictly verified against the session owner's permissions.
* **Message Size & Framing Attacks**: Enforce strict frame-size limits. A malicious client could stream massive payloads to induce heap exhaustion. Implement a `maxPayloadLength` threshold at the server’s WebSocket handler level.

#### Modernized Implementation (Node.js/ws)

In 2026, origin verification and frame-size limiting are critical defense-in-depth measures.

#### Audit Summary
1. **Transport**: Moved from `ws://` (unencrypted) to mandatory `wss://`.
2. **Origin Validation**: Leveraged `verifyClient` hook to terminate unauthenticated handshakes before resource allocation.
3. **Stability**: Introduced `maxPayload` constraints to prevent buffer overflow/DoS vectors.
4. **Security Posture**: Emphasized that session context must be validated per-frame, as state persists beyond the initial handshake.

**Examples:**

```javascript
// Server (Node.js 22+ with 'ws' 8.x+)
const { WebSocketServer } = require('ws');
const wss = new WebSocketServer({ 
  port: 8080,
  maxPayload: 1024 * 16, // 16KB limit to prevent memory exhaustion
  verifyClient: (info, cb) => {
    const origin = info.origin;
    if (isAllowedOrigin(origin)) {
      cb(true); // Accept connection
    } else {
      cb(false, 403, 'Forbidden');
    }
  }
});

function isAllowedOrigin(origin) {
  const allowed = ['https://trusted.app'];
  return allowed.includes(origin);
}

wss.on('connection', (ws, req) => {
  // Session established. 
  // Perform secondary validation if using JWT/Tokens extracted from headers/cookies
  ws.on('message', (data) => {
    // Validate data schema here (e.g., Joi or Zod)
  });
});
```

```javascript
// Client (Browser Standard API)
// Always use 'wss' for encrypted transmission.
const ws = new WebSocket('wss://api.example.com/v1/socket');

ws.onmessage = (event) => {
  // Never interpret message data as executable code (e.g., avoid eval())
  const payload = JSON.parse(event.data);
  processUpdate(payload);
};
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>How would you detect and handle WebSocket connection loss?</summary>

### How would you detect and handle WebSocket connection loss?

**Definition:**

### Connection Monitoring

1. **Native Heartbeats**: Utilize the browser-native `WebSocket.ping()` and `pong` events (if supported by the specific protocol implementation) or implement application-layer heartbeat frames. Standardize on an interval `I` (typically 30s) and a timeout `T < I`.
2. **Server-Side Monitoring**: Deploy **Prometheus** exporters to track `active_connections` via socket gauges. In 2026, utilize **OpenTelemetry** trace spans to correlate socket lifecycle events with downstream microservice latency.
3. **Client-Side Tracking**: Utilize the `onclose` and `onerror` event listeners. For React 19+ environments, wrap socket logic in `useSyncExternalStore` or custom hooks to ensure state consistency across the component tree.

**How it works:**

### Identifying Disconnection Causes

1. **Graceful Closure**: Indicated by Close Code `1000` (Normal Closure).
2. **Abnormal Disconnection**: Indicated by Close Code `1006` (No close frame received). This usually signifies a TCP-level timeout, NAT table expiration, or physical network failure.
3. **Protocol Errors**: Indicated by codes `1002` (Protocol error) or `1009` (Message too large). These require client-side logic updates rather than reconnection.

### Reconnection Strategies

1. **Jittered Exponential Backoff**: Prevent "Thundering Herd" syndrome by adding a random factor to the backoff. 
 The delay `D` is calculated as: `D = min(base × 2^n, max_delay) + jitter`, where `n` is the attempt count and `jitter in [0, base]`.
2. **State Synchronization**: Upon reconnection, the client must perform a state-sync handshake to retrieve missed events during the offline window (e.g., requesting a delta-payload based on the last processed sequence ID).
3. **Visibility API Integration**: Utilize `document.visibilityState`. Pause or reduce heartbeat frequency when the page is hidden to save energy and reduce server overhead.

### Modernized Code Example (TypeScript/React 19)

### 2026 Auditor Notes
* **Performance**: Avoid frequent polling; rely on TCP Keep-Alive settings configured at the load balancer (e.g., NGINX `proxy_read_timeout` / AWS NLB idle timeouts) to drop dead connections.
* **Security**: Ensure all WebSocket upgrades originate from `wss://` (TLS 1.3).
* **Complexity**: Reconnection logic maintains `O(1)` space complexity, but server-side connection state management during widespread reconnect events is `O(N)` where `N` is the number of concurrent clients. Ensure server-side rate limiting on the `/ws` handshake endpoint.

**Examples:**

```typescript
const RECONNECT_BASE = 2000;
const MAX_DELAY = 30000;

function useWebSocket(url: string) {
  const [ws, setWs] = useState<WebSocket | null>(null);
  const attempt = useRef(0);

  const connect = useCallback(() => {
    const socket = new WebSocket(url);

    socket.onopen = () => {
      attempt.current = 0;
      console.log('Connected');
    };

    socket.onclose = (event) => {
      if (event.code === 1000) return;

      const delay = Math.min(RECONNECT_BASE * 2 ** attempt.current, MAX_DELAY) + Math.random() * 1000;
      setTimeout(connect, delay);
      attempt.current++;
    };

    setWs(socket);
  }, [url]);

  useEffect(() => {
    connect();
    return () => ws?.close();
  }, [connect]);
}
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>Explain the role of ping/pong frames in WebSockets.</summary>

### Explain the role of ping/pong frames in WebSockets.

**Definition:**

### Role of Ping/Pong Frames in WebSockets (RFC 6455)

**Ping** (Opcode `0x9`) and **Pong** (Opcode `0xA`) frames are **Control Frames** within the WebSocket protocol designed for **Liveness Monitoring** and **Path Maintenance**. They operate independently of the data stream, ensuring the underlying TCP connection remains viable in the presence of middleboxes (NATs, load balancers, and firewalls).

**How it works:**

#### Purpose and Operational Necessity

1. **Connection Keep-Alive (Liveness):** Stateful firewalls often drop idle TCP connections after a predetermined TTL (Time-To-Live). Periodic Ping frames reset these timers, preventing abrupt connection termination.
2. **Dead Peer Detection:** If a network path fails or a client silently disconnects (e.g., mobile radio sleep), the Pong response absence allows the server to identify "zombie" connections. The server can then initiate `Close` frames to reclaim file descriptors and memory.
3. **Latency Probing:** By including a timestamp in the application data payload, endpoints can measure Round-Trip Time (RTT) to assess network health and adjust congestion control parameters.

#### Frame Architecture (RFC 6455 Specification)

Control frames must have a payload length `≤ 125` bytes and cannot be fragmented. 

| Frame Type | Opcode (Hex) | Description |
| :--- | :--- | :--- |
| **Ping** | `0x9` | Request for peer verification. |
| **Pong** | `0xA` | Unsolicited or solicited response to a Ping. |

**Hexadecimal Structure:**
A Ping frame with no payload is represented as:
`89 00` (FIN bit set, Opcode 9, Mask bit 0, Payload Length 0).

*Note:* Per RFC 6455 Section 5.5.3, a Pong frame **must** be sent in response to a Ping frame as soon as possible, mirroring the "Application Data" payload of the received Ping.

#### Modern Security Considerations

* **Resource Exhaustion:** Rapid, high-frequency Pings are a vector for **DDoS (Distributed Denial of Service)**. Implement rate-limiting at the application layer or via an ingress controller (e.g., NGINX/Envoy) to drop connections exceeding a predefined ping-rate threshold.
* **Payload Mirroring:** Since Pong frames echo the Ping payload, they are not suitable for authentication tokens or sensitive data. Do not treat Pong responses as proof of identity; they only verify path connectivity.

#### Modern Implementation (Python 3.14+ / `websockets` 15.0+)

In current asynchronous paradigms, the `websockets` library handles Ping/Pong heartbeats automatically via the `ping_interval` and `ping_timeout` configuration parameters. Manual invocation is typically unnecessary.

#### Complexity Analysis
* **Memory Overhead:** `O(1)` per connection, as ping frames are transient and non-buffered.
* **Network Overhead:** `O(k)` where `k` is the frequency of pings. The protocol ensures this is minimal, as control frames share the existing TCP socket rather than establishing new handshakes.

**Examples:**

```python
import asyncio
import websockets

async def connection_manager():
    # Automatically manages keep-alive pings every 20 seconds
    # Closes connection if Pong is not received within 10 seconds
    async with websockets.connect(
        "wss://api.example.com",
        ping_interval=20,
        ping_timeout=10
    ) as ws:
        async for message in ws:
            print(f"Data received: {message}")

if __name__ == "__main__":
    asyncio.run(connection_manager())
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>How does WebSocket ensure ordered delivery of messages?</summary>

### How does WebSocket ensure ordered delivery of messages?

**Definition:**

### WebSocket Ordered Delivery Mechanisms

**WebSocket** ensures ordered delivery by leveraging the inherent stream-based guarantees of **TCP** (Transmission Control Protocol) and the **WebSocket Framing Protocol** (RFC 6455).

**How it works:**

#### The Role of TCP Layering
WebSockets operate over a single, long-lived TCP connection. **TCP** provides the foundational guarantee of ordered delivery through the following mechanisms:

* **Sequence Numbers**: Every byte transmitted is assigned a unique sequence number.
* **Acknowledgment (ACK)**: The receiver acknowledges receipt of segments; if a gap in sequence numbers is detected, the receiver buffers out-of-order packets and requests retransmission of missing segments.
* **In-Order Delivery**: The TCP stack ensures that segments are delivered to the application layer (the WebSocket implementation) only after all preceding segments have been successfully received and reassembled.

#### Full-Duplex Communication
Unlike the HTTP/1.1 request-response cycle, the WebSocket protocol maintains a persistent **full-duplex** channel. This prevents "head-of-line blocking" at the HTTP application level by allowing asynchronous traffic flow, while the underlying TCP connection maintains strict serial integrity for that specific bidirectional stream.

#### WebSocket Framing and Message Integrity
When a WebSocket message exceeds the frame buffer size or is fragmented, the **WebSocket Framing Protocol** ensures reconstruction via the **FIN (Final)** bit:

1. **Fragmentation**: If a message is too large or requires streaming, the sender splits the payload into multiple frames.
2. **Opcode Tracking**: The first frame uses a specific **opcode** (e.g., `0x1` for text, `0x2` for binary), while subsequent fragments use the **continuation opcode** (`0x0`).
3. **FIN Bit**: The `FIN` bit is set to `0` for all non-final fragments and `1` for the last fragment. 
4. **Reconstruction**: The endpoint buffers the payloads of frames with `FIN=0` until the frame with `FIN=1` is received, at which point the message is processed as a discrete unit.

#### Modern Implementation (2026 Standards)
Modern environments leverage **Streams API** and **Web Workers** for efficient frame reconstruction, avoiding the memory-intensive string concatenation seen in older implementations.

### Complexity Analysis
The reconstruction of WebSocket frames is effectively `O(n)` relative to the total payload size, where `n` is the sum of bytes in all fragments. Because TCP guarantees the arrival of segments in order (`i, i+1, dots, n`), the application-level reconstruction is memory-bound rather than compute-bound. 

`T(n) = sum_i=1^k copy(frame_i) approx O(n)`

*Note: The usage of `ArrayBuffer` and `TypedArrays` in 2026 environments ensures that the space complexity remains `O(n)`, minimizing garbage collection pressure compared to legacy string concatenation.*

**Examples:**

```javascript
// 2026 Modern Implementation using ReadableStream
const socket = new WebSocket('wss://api.example.com/stream');

// Using a TransformStream to handle fragmentation natively
const messageStream = new TransformStream({
  transform(chunk, controller) {
    // Logic for handling FIN bits and buffer assembly
    controller.enqueue(chunk);
  }
});

socket.onmessage = async (event) => {
  // Utilizing the Blob/ArrayBuffer interface for memory efficiency
  const data = event.data;
  
  // In 2026, logic handles binary fragments via Uint8Array concatenation
  // to avoid the O(n^2) cost of string concatenation in older JS engines
  const buffer = await new Response(data).arrayBuffer();
  processBuffer(buffer);
};
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>Can WebSockets be used for broadcasting messages to multiple clients? If so, how?</summary>

### Can WebSockets be used for broadcasting messages to multiple clients? If so, how?

**Definition:**

### WebSocket Broadcasting: 2026 Architectural Audit

**WebSockets** support true **bidirectional full-duplex communication**. While the protocol defines point-to-point connections between a client and a server, broadcasting is achieved by maintaining an application-level registry of active connections and iterating over them.

**How it works:**

### Modern Broadcasting Mechanism
- **Connection Registry**: The server manages a set of active socket descriptors.
- **Complexity**: Broadcasting a message to `N` connected clients incurs an `O(N)` overhead per message.
- **Horizontal Scaling**: In 2026 production environments, simple memory-resident sets (as shown in the previous code) fail under load-balanced clusters. **Pub/Sub brokers** (e.g., Redis, NATS, or RabbitMQ) are now the standard for synchronizing state across multiple server nodes.

### Optimized Code Example (Python 3.14+)
Using modern `asyncio` patterns and typed connection management.

### Architectural Best Practices (2026)

* **Broadcast Reliability**: The naive loop `O(N)` is susceptible to slow-client head-of-line blocking. Utilize `asyncio.gather` with `return_exceptions=True` to prevent a single disconnected client from crashing the broadcast loop.
* **Pub/Sub Decoupling**: For distributed systems, replace the local `set()` with an external event bus. When a server node receives an event, it publishes to a Redis channel; all server instances subscribe to this channel and push to their locally connected clients.
* **Connection Lifecycle**: Always implement **Heartbeat/Ping-Pong** mechanisms (handled natively by the `websockets` library) to prune stale connections (`O(1)` cleanup) and prevent memory leaks.
* **Protocol Considerations**: For scenarios requiring unidirectional broadcasting (server-to-client only), evaluate **Server-Sent Events (SSE)** over HTTP/2, which provides automatic reconnection and lower header overhead compared to the WebSocket handshake.

**Examples:**

```python
import asyncio
from typing import Set
from websockets.server import serve, WebSocketServerProtocol

# Type-hinted connection tracking
active_clients: Set[WebSocketServerProtocol] = set()

async def handler(websocket: WebSocketServerProtocol):
    active_clients.add(websocket)
    try:
        async for message in websocket:
            # Broadcast pattern: Concurrently propagate to all clients
            if active_clients:
                tasks = [client.send(message) for client in active_clients]
                await asyncio.gather(*tasks, return_exceptions=True)
    finally:
        active_clients.remove(websocket)

async def main():
    async with serve(handler, "localhost", 8765):
        await asyncio.get_running_loop().create_future()  # Run forever

if __name__ == "__main__":
    asyncio.run(main())
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>What is the difference between WebSockets and Server-Sent Events (SSE)?</summary>

### What is the difference between WebSockets and Server-Sent Events (SSE)?

**Definition:**

### Comparative Audit: WebSockets vs. Server-Sent Events (SSE)

**WebSockets** and **Server-Sent Events** (SSE) represent distinct architectural patterns for streaming data. As of 2026, SSE is universally supported across all modern browsers (including full implementation in WebKit/Safari), rendering the previous "niche" classification obsolete.

**How it works:**

### Key Distinctions

#### WebSockets
- **Characteristics**: **Full-duplex**, bidirectional communication over a single, long-lived TCP connection. Enables low-latency, stateful interactions where both the client and server act as peers.
- **Protocol**: Operates on a custom framing protocol (RFC 6455) initiated via an HTTP 1.1/2/3 upgrade request.
- **Complexity**: Requires **state management** on both ends. Developers must manually implement heartbeat mechanisms (ping/pong) to detect silent connection drops.
- **Use Case**: High-frequency, interactive applications requiring sub-millisecond latency, such as **collaborative editing (CRDTs)**, multiplayer gaming, and high-frequency trading dashboards.

#### Server-Sent Events (SSE)
- **Characteristics**: **Unidirectional** (Server-to-Client). Operates over standard HTTP. Once the connection is established, the server pushes text-based event streams to the client.
- **Protocol**: Native **HTTP**. It leverages standard transport layers, meaning it benefits from HTTP/2 multiplexing and HTTP/3 (QUIC) stream-level features without requiring a custom protocol handshake.
- **Features**: Built-in support for **automatic reconnection**, event ID tracking for message recovery, and lightweight consumption via the `EventSource` API. 
- **Use Case**: Low-overhead streaming of data updates, such as **LLM response generation** (streaming inference tokens), live notification feeds, and telemetry monitoring.

### Technical Comparison Summary

| Feature | WebSockets | Server-Sent Events (SSE) |
| :--- | :--- | :--- |
| **Communication** | Full-Duplex (Bidirectional) | Unidirectional (Server -> Client) |
| **Transport** | Custom TCP/HTTP Upgrade | Native HTTP (1.1, 2, 3) |
| **Connection Overhead** | High (Stateful) | Low (Stateless HTTP) |
| **Data Format** | Binary or Text (Custom) | UTF-8 Text (EventStream) |
| **Browser Support** | Universal | Universal (including Safari/iOS) |
| **Complexity** | `O(n)` state sync logic | `O(1)` stream consumption |

#### 2026 Architectural Guidance
* **Prefer SSE** for read-heavy or update-heavy applications (e.g., real-time monitoring, AI token streaming) due to the reduced resource overhead of standard HTTP and native automatic reconnection. 
* **Prefer WebSockets** only when the application requires true peer-to-peer messaging (e.g., VoIP, gaming). Avoid WebSockets for simple "server-to-client" updates to prevent unnecessary TCP handshake overhead and state management complexity.

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>Explain how a WebSocket connection is closed.</summary>

### Explain how a WebSocket connection is closed.

**Definition:**

### WebSocket Connection Termination (2026 Standard)

A **WebSocket connection** (RFC 6455) is a persistent, full-duplex communication channel. Termination follows a defined **Close Handshake** procedure to ensure data integrity.

**How it works:**

### Closure Scenarios

#### The Close Control Frame
Termination is governed by the exchange of **Close frames** (Opcode `0x8`). When one endpoint initiates closure, it sends a Close frame containing an optional status code and reason. The peer must acknowledge this with a corresponding Close frame before the underlying **TCP connection** is severed.

#### The CloseEvent Interface
Upon termination, the browser triggers a `CloseEvent` on the `WebSocket` object:

* **`code`**: An `unsigned short` indicating the closure status (e.g., `1000` for Normal Closure).
* **`reason`**: A `USVString` providing a human-readable explanation.
* **`wasClean`**: A `boolean` (True if the connection closed gracefully via the handshake).

### Closing via Client API

1. **Graceful Closure**: The client invokes `socket.close(code, reason)`. The browser initiates the handshake and waits for the server to acknowledge.
2. **Abrupt Termination**: Triggered by network interface loss or `AbortController` signal integration, resulting in `wasClean = false`.

### Closing via Server-Side

1. **Graceful Closure**: The server sends a Close frame. The client, upon receipt, must respond with its own Close frame.
2. **Protocol Error**: If the server detects a violation of the WebSocket protocol (e.g., malformed frames), it terminates the connection immediately with status `1002`.

### 2026 Lifecycle Management

Modern applications leverage the **Streams API** or **WebTransport** for high-performance needs, but for standard WebSockets, connection stability is managed via `keepalive` mechanisms at the transport layer to prevent silent TCP half-open states.

### Implementation: Modern Closure Handling

### Technical Note on Status Codes
As of 2026, status codes follow IANA registry standards:
* **1000**: Normal Closure (Success).
* **1001**: Going Away (Browser navigation or server shutdown).
* **1006**: Abnormal Closure (Signal for reconnection logic).
* **1011**: Internal Error (Server-side exception).

**Examples:**

```javascript
// React 19 / Modern Browser Context
const socket = new WebSocket('wss://api.example.com/v1/stream');

socket.onclose = (event) => {
  const { code, reason, wasClean } = event;
  
  if (wasClean) {
    console.info(`Connection closed cleanly [`code]:`{reason}`);
  } else {
    // Implement exponential backoff reconnection logic
    console.error(`Connection dropped unexpectedly [${code}]`);
    attemptReconnection();
  }
};

// Standardized closure invocation
const handleShutdown = (reason) => {
  // 1000: Normal Closure
  socket.close(1000, reason || 'Client-side termination');
};

// React 19: Clean up during component unmount
import { useEffect } from 'react';

function useWebSocket(url) {
  useEffect(() => {
    const ws = new WebSocket(url);
    return () => ws.close(1000, 'Component unmounted');
  }, [url]);
}
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>What fallback mechanisms can be used if WebSockets are not supported by a browser or server?</summary>

### What fallback mechanisms can be used if WebSockets are not supported by a browser or server?

**Definition:**

### Modernization Audit: Real-Time Communication Fallbacks

In 2026, the architectural standard for real-time web communication relies on the **WebSocket API (RFC 6455)**, **WebTransport**, and **Server-Sent Events (SSE)**. Legacy polling methods are discouraged due to high overhead on `O(n)` network traffic and server resource exhaustion. 

Modern applications favor **graceful degradation** via libraries like Socket.io or SignalR, which abstract these fallback chains.

---

**How it works:**

### Polling & Streaming Evolution

#### Short Polling (Deprecated/Legacy)
* **Mechanism**: Periodic HTTP requests.
* **Assessment**: High latency and significant server overhead (`O(n)` requests over time). Use only if extreme backward compatibility is required.

#### Long Polling (Deprecated)
* **Mechanism**: Server holds request until data is available. 
* **Assessment**: Mitigates latency but suffers from **Head-of-Line (HoL) blocking** and connection overhead. Avoid in 2026 deployments.

#### Server-Sent Events (SSE)
* **Mechanism**: Unidirectional, text-based stream over HTTP.
* **2026 Status**: Re-emerged as the preferred standard for AI streaming (e.g., LLM tokens) and unidirectional updates. Native browser support is excellent.
* **Pros**: Efficient, built-in reconnection logic, lower overhead than WebSockets.

#### HTTP/3 and WebTransport
* **Mechanism**: A modern successor to WebSockets, operating over QUIC.
* **2026 Status**: **The industry standard for low-latency, multiplexed communication.** It solves HoL blocking and provides both reliable and unreliable data streams.

---

### Implementation Patterns (2026 Standard)

#### Modern Fetch/SSE Implementation

#### WebTransport (The 2026 WebSocket Alternative)

---

### Architectural Recommendations

| Mechanism | 2026 Usage | Rationale |
| :--- | :--- | :--- |
| **WebTransport** | **Primary** | Multiplexed, QUIC-based, solves HoL blocking. |
| **SSE** | **Secondary** | Best for unidirectional data (AI streaming/dashboards). |
| **WebSockets** | **Legacy/Compat** | Maintain for existing infrastructure; migration is recommended. |
| **Polling** | **Prohibited** | Use only for edge cases where persistent streams are blocked. |

### Security & Reliability
* **TLS/QUIC**: All real-time streams must operate over `WSS` or `HTTP/3` (TLS 1.3). Encryption is mandatory.
* **Backpressure**: Implement `ReadableStream` backpressure to ensure that slow clients do not cause memory spikes on the server.
* **Connection Resilience**: Use `Exponential Backoff` for reconnection logic (`T = min(cap, base × 2^attempt)`) to prevent "thundering herd" server crashes.

**Examples:**

```javascript
// Using ReadableStream for efficient processing (2026 Standard)
async function consumeStream(url) {
  const response = await fetch(url, { headers: { 'Accept': 'text/event-stream' } });
  const reader = response.body.pipeThrough(new TextDecoderStream()).getReader();

  while (true) {
    const { value, done } = await reader.read();
    if (done) break;
    processData(value);
  }
}
```

```javascript
// WebTransport provides high-performance, low-latency streams
const transport = new WebTransport('https://api.example.com:443/ws');
await transport.ready;

const stream = await transport.createBidirectionalStream();
const writer = stream.writable.getWriter();
await writer.write(new TextEncoder().encode('Hello, Server!'));
```

**Sources:**

- [Devinterview.io — WebSockets](https://devinterview.io/questions/web-and-mobile-development/websocket-interview-questions/)
</details>

<details>
<summary>How would you implement a chat application using WebSockets?</summary>

### How would you implement a chat application using WebSockets?

**Definition:**

A chat app keeps one WebSocket per browser. The server remembers those sockets and writes each chat message out to the other open sockets. The protocol itself has no rooms; the app decides who receives a frame.

**How it works:**

- **Connection**: The page calls `new WebSocket("wss://example.com/chat")`. The browser sends the HTTP/1.1 upgrade (`Upgrade: websocket`, `Sec-WebSocket-Key`). The server answers `101 Switching Protocols` and then both sides send frames.
- **Who is in the room**: After the handshake, the server stores the socket. MDN’s server guide says to keep a table of clients so you do not handshake the same socket twice. A chat room is that table, filtered by a room id the client sent.
- **A message**: Text frames are UTF-8. A small JSON object (`{ "room", "text" }`) is enough. `send()` queues it; `bufferedAmount` grows until the bytes leave the browser.
- **Fan-out**: On the `ws` server, `wss.clients` is the set of sockets. Send only to sockets whose `readyState` is `OPEN`, and skip the sender if the UI should not echo.
- **Leave**: `close()` starts the closing handshake. Drop the socket from the table on `close` so later messages do not write to a dead connection.

**When to use:**

- Use a WebSocket when each side must push without a new HTTP request: a room where people type at the same time.
- Keep history, login, and “load older messages” on ordinary HTTP. The socket carries the live tail, not the whole archive.

**Examples:**

```js
import { WebSocketServer } from "ws";

const wss = new WebSocketServer({ port: 8080 });

wss.on("connection", (socket) => {
  socket.on("message", (data, isBinary) => {
    for (const client of wss.clients) {
      if (client !== socket && client.readyState === WebSocket.OPEN) {
        client.send(data, { binary: isBinary });
      }
    }
  });
});
```

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [ws — server broadcast](https://github.com/websockets/ws)
</details>

<details>
<summary>Explain how pub/sub messaging can be achieved with WebSockets.</summary>

### Explain how pub/sub messaging can be achieved with WebSockets.

**Definition:**

Publish/subscribe is an application pattern on top of WebSocket frames. Publishers send a message; the server copies it to the sockets that subscribed to that topic. RFC 6455 does not define topics.

**How it works:**

- **Subscribe**: After `open`, the client sends a text frame such as `{"op":"sub","topic":"orders"}`. The server records that socket under the topic.
- **Publish**: A publisher frame names the same topic. The server walks the topic’s sockets and calls `send` only when `readyState` is `OPEN`.
- **One socket, many topics**: The browser still has one connection. Topics are names in your JSON, not separate TCP connections.
- **Several processes**: A socket lives on one process. To publish across machines, that process forwards the payload through a shared bus, then each process writes to its own sockets.

**When to use:**

- Use a topic map when many clients should hear the same event and you control both ends.
- Use Socket.IO rooms when you want that map, plus its own packet format and a long-polling fallback. A raw WebSocket client cannot speak that format.

**Examples:**

```js
const topics = new Map();

function publish(topic, payload) {
  for (const socket of topics.get(topic) ?? []) {
    if (socket.readyState === WebSocket.OPEN) socket.send(payload);
  }
}
```

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — server broadcast](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>

<details>
<summary>What kind of data can be sent over WebSocket connections?</summary>

### What kind of data can be sent over WebSocket connections?

**Definition:**

A WebSocket message is either text or binary. The browser `send()` method accepts a string, or binary as `Blob`, `ArrayBuffer`, or a typed-array view.

**How it works:**

- **Text**: A string is queued as UTF-8. On the wire that is a text frame (opcode `0x1`).
- **Binary**: `ArrayBuffer`, `Blob`, `TypedArray`, and `DataView` are queued as a binary frame (opcode `0x2`). A `Blob`’s MIME type is not sent.
- **Not a frame type**: JSON, protobuf, and images are your encoding inside one of those two frame types. Control frames (close, ping, pong) are not application data.

**Examples:**

```js
socket.send("hello");
socket.send(new TextEncoder().encode(JSON.stringify({ id: 1 })));
```

**Sources:**

- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>How do you handle binary data in WebSockets?</summary>

### How do you handle binary data in WebSockets?

**Definition:**

Binary arrives as a `Blob` unless you set `binaryType` to `"arraybuffer"`. You then read the bytes with a `DataView` or decode them with your own schema.

**How it works:**

- **Default**: MDN’s default for `binaryType` is `"blob"`. Text messages still arrive as strings.
- **ArrayBuffer**: Set `socket.binaryType = "arraybuffer"` before you rely on the type. In `message`, `event.data instanceof ArrayBuffer` means a binary frame.
- **Sending**: `send()` accepts `ArrayBuffer`, a typed array, `DataView`, or `Blob`. The payload is one binary message, possibly split into continuation frames.

**Examples:**

```js
socket.binaryType = "arraybuffer";
socket.addEventListener("message", (event) => {
  if (event.data instanceof ArrayBuffer) {
    const view = new DataView(event.data);
    console.log(view.getInt32(0));
  }
});
```

**Sources:**

- [MDN — WebSocket.binaryType](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/binaryType)
- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
</details>

<details>
<summary>Can you describe how WebSockets can be used for real-time gaming?</summary>

### Can you describe how WebSockets can be used for real-time gaming?

**Definition:**

A game uses the open socket so the server can push state without waiting for the browser to poll. Each gameplay message is still an ordered, reliable frame on one TCP connection.

**How it works:**

- **Path**: MDN notes that the request URI has no protocol meaning, so `/game` can be a different app on the same host as `/chat`.
- **Messages**: Small binary frames suit positions and inputs. Text JSON is easier to inspect. Either way, opcode `0x1` or `0x2` carries the payload.
- **Order**: TCP delivers bytes in order. A late position update waits behind earlier bytes on that connection. That is head-of-line blocking, which the WebSocket framing does not remove.
- **Liveness**: Ping and pong (opcodes `0x9` and `0xA`) tell you the peer is still there between gameplay frames.

**When to use:**

- Use WebSocket for turn state, lobby chat, and any update that must arrive intact and in order.
- For a stream that may drop stale positions, look at a transport that can send unreliable datagrams. WebSocket itself stays reliable and ordered.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>What are the common frameworks or libraries for working with WebSockets?</summary>

### What are the common frameworks or libraries for working with WebSockets?

**Definition:**

The browser already has `WebSocket`. On the server you add a library that speaks RFC 6455, or a higher-level library that uses WebSocket as one transport.

**How it works:**

- **Browser**: `new WebSocket(url, protocols)` is the platform API. Node’s `ws` client is not for the browser; the `ws` readme says browser code must use the native object.
- **Node**: `ws` is a WebSocket client and server for Node. It passes the Autobahn suite. `WebSocketServer` can listen on a port or attach to an existing HTTP server.
- **Socket.IO**: Socket.IO is not a WebSocket implementation. It can use WebSocket, long-polling, or WebTransport, and it adds its own packet framing.
- **.NET**: ASP.NET Core exposes raw WebSockets with `UseWebSockets` and `AcceptWebSocketAsync`. SignalR sits above that and prefers WebSockets, then falls back.

**When to use:**

- Pick `WebSocket` plus `ws` when both sides can speak frames you define.
- Pick Socket.IO or SignalR when you want reconnection, rooms, and a fallback transport and you can use their client.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [ws](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>

<details>
<summary>How is flow control managed in WebSocket communication?</summary>

### How is flow control managed in WebSocket communication?

**Definition:**

WebSocket does not add its own receive window. TCP already slows a sender when the receiver’s buffer fills. In the browser, `bufferedAmount` is how you see bytes that `send()` queued but has not transmitted.

**How it works:**

- **bufferedAmount**: The property is an unsigned long. It returns to zero after the queued data is sent. Closing the socket does not zero it, and further `send()` calls keep increasing it.
- **Send too early**: `send()` throws `InvalidStateError` while `readyState` is `CONNECTING`. In `CLOSING` or `CLOSED` the data is discarded.
- **Backpressure in the page**: Before `send()`, check `bufferedAmount`. Skip or coalesce updates while the number stays high, so a slow peer does not grow the queue without bound.

**Examples:**

```js
if (socket.bufferedAmount < 64 * 1024) {
  socket.send(JSON.stringify(nextPosition));
}
```

**Sources:**

- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/bufferedAmount)
- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
</details>

<details>
<summary>Describe the process of scaling WebSocket applications.</summary>

### Describe the process of scaling WebSocket applications.

**Definition:**

Each open WebSocket is state on one process: the TCP connection and your client table. Scaling means more processes, and a way for a message that arrives on one process to reach sockets on the others.

**How it works:**

- **One machine**: MDN says servers are often separate from the HTTP app, with a reverse proxy detecting the handshake and forwarding it. Ports other than 80 and 443 have more trouble with firewalls.
- **Many machines**: A load balancer must send the upgrade to a process and then keep that TCP connection on that process. Another process does not have the socket.
- **Fan-out**: Publish the chat or topic payload on a shared bus. Every WebSocket process subscribes and writes to the local sockets that match.
- **Limits**: MDN says the server may refuse clients that connect too often, to limit denial of service. Track sockets so a closed one leaves the table.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — server broadcast](https://github.com/websockets/ws)
</details>

<details>
<summary>How can you send custom headers when initializing a WebSocket connection?</summary>

### How can you send custom headers when initializing a WebSocket connection?

**Definition:**

The browser constructor is `new WebSocket(url, protocols)`. It does not accept an arbitrary header map. The browser sets the handshake headers, including `Origin`.

**How it works:**

- **What you can pass**: `protocols` is one subprotocol string or a list, sent as `Sec-WebSocket-Protocol`. A repeated or illegal token throws `SyntaxError`.
- **Cookies and auth**: The handshake is an HTTP request, so the browser can attach cookies for that host. MDN says headers such as `Cookie` may be present and the server may use them. You cannot set `Authorization` from the constructor.
- **Outside the browser**: A Node client such as `ws` can set headers because it builds the HTTP request itself. That API is not the browser `WebSocket`.
- **A token in the page**: Put a short-lived value in the query string or in the first message after `open`, and check it while completing the upgrade or before you trust frames.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>Can WebSockets be used together with other web technologies like WebRTC?</summary>

### Can WebSockets be used together with other web technologies like WebRTC?

**Definition:**

Yes. WebRTC needs a signaling channel so two peers can exchange session descriptions and ICE candidates. MDN’s signaling guide says the channel can be a WebSocket or any two-way path.

**How it works:**

- **Different jobs**: The WebSocket carries small JSON signaling messages through your server. Media and data channels then go peer to peer, or through a TURN relay, not through that WebSocket.
- **Who must be connected**: Signaling is required only while negotiating. After the RTCPeerConnection is up, the peers can keep media flowing if the socket drops, until they need to renegotiate.
- **Same server**: A page can use `fetch` for accounts, `WebSocket` for signaling, and `RTCPeerConnection` for the call. They do not share one framing protocol.

**When to use:**

- Use the socket for “who is calling whom” and the SDP/ICE text.
- Do not send the audio samples themselves over the WebSocket if WebRTC is available. That would pin the media to your server and to TCP.

**Sources:**

- [MDN — Signaling and video calling](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Signaling_and_video_calling)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>


### WebSockets Protocol Details

<details>
<summary>What defines a WebSocket message?</summary>

### What defines a WebSocket message?

**Definition:**

A message is a sequence of frames that belong together. It starts with a text or binary opcode and ends at the frame whose FIN bit is 1. A single-frame message has FIN 1 on that first frame.

**How it works:**

- **Opcodes**: `0x1` starts text (UTF-8). `0x2` starts binary. `0x0` continues the same message. Control opcodes are not part of that data message.
- **FIN**: FIN 0 means more frames follow. FIN 1 means the payload so far is the complete message. The browser `message` event fires for the assembled message, not for each fragment.
- **Interleaving**: Control frames (ping, pong, close) may arrive between fragments. They do not end the data message.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — message event](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/message_event)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>How are WebSocket extensions used?</summary>

### How are WebSocket extensions used?

**Definition:**

An extension changes how frames or payloads are carried. It does not define the meaning of the application message. Compression such as `permessage-deflate` is the usual example.

**How it works:**

- **Negotiate**: The client lists extensions in `Sec-WebSocket-Extensions`. The server echoes the ones it accepts on the 101 response. RSV bits in the frame are reserved for extensions.
- **Not a subprotocol**: MDN: extensions are optional and general (compression). Subprotocols are the structure of the payload (chat versus a game) and do not change the frame.
- **Cost**: The `ws` server leaves permessage-deflate off by default because zlib can cost memory and CPU under concurrency. Turn it on only when payloads are large enough to pay for that.

**When to use:**

- Enable `permessage-deflate` for chatty JSON that repeats keys.
- Leave it off for small binary game packets and for servers that are already CPU-bound.

**Examples:**

```js
import { WebSocketServer } from "ws";

const wss = new WebSocketServer({
  port: 8080,
  perMessageDeflate: {
    threshold: 1024,
    concurrencyLimit: 10,
  },
});
```

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — Sec-WebSocket-Extensions](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Sec-WebSocket-Extensions)
- [ws — compression](https://github.com/websockets/ws)
</details>

<details>
<summary>What are WebSocket subprotocols and how are they negotiated?</summary>

### What are WebSocket subprotocols and how are they negotiated?

**Definition:**

A subprotocol is a name for the payload structure both sides agree to use. The client offers names; the server picks at most one.

**How it works:**

- **Offer**: The constructor’s second argument becomes one or more `Sec-WebSocket-Protocol` header values, in preference order.
- **Choose**: The server sends back a single `Sec-WebSocket-Protocol` with one chosen name. MDN says more than one response header is illegal, and an empty header is illegal. Omit the header to select none.
- **After that**: The name does not change opcodes. Your code must parse the payload the name implies, for example JSON for a `json` subprotocol. `socket.protocol` is the selected name.

**Examples:**

```js
const socket = new WebSocket("wss://example.com/chat", ["chat.example.com", "json"]);
```

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — Protocol upgrade](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Protocol_upgrade_mechanism)
</details>

<details>
<summary>Explain WebSocket's support for text and binary frames.</summary>

### Explain WebSocket's support for text and binary frames.

**Definition:**

Data frames use opcode `0x1` for UTF-8 text and `0x2` for opaque binary. Both can be split across continuation frames. The opcode of the first frame sets the type of the whole message.

**How it works:**

- **Text**: The payload must be valid UTF-8. MDN’s server notes say to decode a text message as UTF-8 after unmasking. Invalid UTF-8 fails the connection (close code 1007).
- **Binary**: Binary has no character set. The application decides the layout. The browser exposes it as `Blob` or `ArrayBuffer` according to `binaryType`.
- **Reserved opcodes**: Opcodes `0x3`–`0x7` and `0xB`–`0xF` have no meaning in this version. Control opcodes are `0x8` close, `0x9` ping, `0xA` pong.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — binaryType](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/binaryType)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>How is message fragmentation handled in WebSockets?</summary>

### How is message fragmentation handled in WebSockets?

**Definition:**

Fragmentation splits one message into several data frames. The first frame carries opcode `0x1` or `0x2` and FIN 0. Later frames use opcode `0x0`. The last frame sets FIN to 1.

**How it works:**

- **Where it applies**: MDN says fragmentation is only for opcodes `0x0` through `0x2`. Control frames are not fragmented this way, and their payload is at most 125 bytes.
- **Reassembly**: The receiver concatenates payloads until FIN is 1, then treats the result as one text or binary message. The browser does this before the `message` event.
- **Example from MDN**: One frame `FIN=1, opcode=0x1` is a whole message. A later message can be `FIN=0, opcode=0x1`, then `FIN=0, opcode=0x0`, then `FIN=1, opcode=0x0`.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>What are the standard status codes for indicating connection closure in WebSockets and what do they mean?</summary>

### What are the standard status codes for indicating connection closure in WebSockets and what do they mean?

**Definition:**

A Close frame may carry a status code and a short reason. RFC 6455 defines 1000–1015 and reserves ranges for later specs, libraries, and private use. The browser `close()` method may only set `1000` or a code from 3000 to 4999.

**How it works:**

- **On the wire**: 1000 normal closure. 1001 going away. 1002 protocol error. 1003 unsupported data. 1007 bad UTF-8. 1008 policy. 1009 message too big. 1010 client required an extension the server did not select. 1011 unexpected server condition.
- **Not sent in a frame**: 1005 means no status was present. 1006 means the connection closed without a Close frame. 1015 means the TLS handshake failed. Endpoints must not put these in a Close frame. 1004 is reserved.
- **Ranges**: 0–999 are unused. 1000–2999 are for the protocol and public extensions. 3000–3999 are for libraries registered with IANA. 4000–4999 are private agreements.
- **Browser close()**: The reason string must be at most 123 UTF-8 bytes. A code outside `1000` and `3000`–`4999` throws `InvalidAccessError`.

**Sources:**

- [RFC 6455 — Status Codes](https://www.rfc-editor.org/rfc/rfc6455#section-7.4)
- [MDN — WebSocket.close()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close)
</details>

<details>
<summary>How does WebSocket mask data from the client to the server?</summary>

### How does WebSocket mask data from the client to the server?

**Definition:**

Every client-to-server frame sets the MASK bit and includes a 4-byte masking key. Each payload byte is XORed with `key[i % 4]`. Server-to-client frames are not masked.

**How it works:**

- **Required**: MDN says the server must disconnect a client that sends an unmasked frame (RFC 6455 section 5.1). Masking is required even on a secure socket.
- **Key**: The client picks a new masking key per frame. MDN describes it as a measure so intermediaries cannot predict the bytes on the wire.
- **Unmask**: Read the 4 octets after the length, then XOR. The browser does this for you. A server you write, or a library such as `ws`, does it before your `message` handler sees the payload.

**Examples:**

```js
function unmask(encoded, mask) {
  return encoded.map((byte, i) => byte ^ mask[i % 4]);
}
```

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>


### WebSockets and JavaScript

<details>
<summary>How do you create a WebSocket connection in JavaScript?</summary>

### How do you create a WebSocket connection in JavaScript?

**Definition:**

Call `new WebSocket(url)` or `new WebSocket(url, protocols)`. The constructor starts the handshake immediately and returns the object while `readyState` is still `CONNECTING` (0).

**How it works:**

- **URL**: The scheme must be `ws`, `wss`, `http`, or `https`. A fragment throws `SyntaxError`. A relative URL resolves against the script’s base URL.
- **Protocols**: The optional second argument is a subprotocol name or an array of names, in preference order.
- **When it is open**: Wait for the `open` event before `send()`. `OPEN` is 1, `CLOSING` is 2, `CLOSED` is 3.

**Examples:**

```js
const socket = new WebSocket("wss://example.com/chat", ["json"]);
socket.addEventListener("open", () => {
  socket.send(JSON.stringify({ type: "join", room: "general" }));
});
```

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>Explain the different event handlers available on the WebSocket object in JavaScript.</summary>

### Explain the different event handlers available on the WebSocket object in JavaScript.

**Definition:**

The browser fires `open`, `message`, `error`, and `close` on the `WebSocket`. You can use `addEventListener` or the `onopen`, `onmessage`, `onerror`, and `onclose` properties.

**How it works:**

- **open**: The handshake finished and `readyState` is `OPEN`. This is the first safe time to `send()`.
- **message**: A complete data message arrived. `event.data` is a string, a `Blob`, or an `ArrayBuffer`.
- **error**: A connection failure fired `error`. The event does not include a useful message body. The socket then proceeds to `close`.
- **close**: The connection is `CLOSED`. `event.code` and `event.reason` describe the close. `1006` means it died without a Close frame. `wasClean` is true when the closing handshake completed.

**Sources:**

- [MDN — message event](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/message_event)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [MDN — close()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close)
</details>

<details>
<summary>How do you send and receive messages in JavaScript using WebSockets?</summary>

### How do you send and receive messages in JavaScript using WebSockets?

**Definition:**

After `open`, `send(data)` queues a text or binary message. Incoming data arrives as a `message` event whose `data` is the whole message.

**How it works:**

- **Send**: A string is UTF-8 text. `ArrayBuffer`, `Blob`, `TypedArray`, and `DataView` are binary. `send` returns `undefined`. It throws `InvalidStateError` if the socket is still `CONNECTING`.
- **Receive**: `addEventListener("message", ...)` or `onmessage`. `event.data` is a string for text. Binary follows `binaryType`.
- **Queue**: `bufferedAmount` counts bytes queued by `send` and not yet transmitted.

**Examples:**

```js
socket.addEventListener("open", () => socket.send("ping"));
socket.addEventListener("message", (event) => {
  console.log(event.data);
});
```

**Sources:**

- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [MDN — message event](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/message_event)
</details>

<details>
<summary>Can you demonstrate how to convert a binary WebSocket message to a JavaScript object?</summary>

### Can you demonstrate how to convert a binary WebSocket message to a JavaScript object?

**Definition:**

Set `binaryType` to `"arraybuffer"`, then decode the bytes. JSON text that you sent as UTF-8 bytes becomes an object through `TextDecoder` and `JSON.parse`. A packed layout uses `DataView` instead.

**How it works:**

- **Know the schema**: The binary frame has no type tag. Both sides must agree, often via the selected subprotocol.
- **JSON bytes**: Decode UTF-8, then `JSON.parse`. This is still a binary frame on the wire if you passed a `Uint8Array` to `send`.
- **Numbers**: MDN’s `binaryType` example reads `view.getInt32(0)` from a `DataView` over the `ArrayBuffer`.

**Examples:**

```js
socket.binaryType = "arraybuffer";
socket.addEventListener("message", (event) => {
  if (!(event.data instanceof ArrayBuffer)) return;
  const text = new TextDecoder().decode(event.data);
  const value = JSON.parse(text);
  console.log(value.id);
});
```

**Sources:**

- [MDN — binaryType](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/binaryType)
- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
</details>

<details>
<summary>Describe error handling with WebSocket connections in JavaScript.</summary>

### Describe error handling with WebSocket connections in JavaScript.

**Definition:**

Treat `error` as “the connection failed” and read the reason from the following `close` event. `send()` and `close()` throw `DOMException` for illegal calls.

**How it works:**

- **error event**: The handler receives an `Event`. It does not carry a status code. Wait for `close` for `code`, `reason`, and `wasClean`.
- **send**: `InvalidStateError` if you send while `CONNECTING`. Data passed while `CLOSING` or `CLOSED` is discarded and does not throw.
- **close**: `InvalidAccessError` for a code other than `1000` or `3000`–`4999`. `SyntaxError` if the reason is longer than 123 UTF-8 bytes.
- **Parse**: A `message` handler that `JSON.parse`s should catch `SyntaxError` and close with a private code in `4000`–`4999` if the peer violated your schema.

**Examples:**

```js
socket.addEventListener("error", () => {
  console.error("socket failed");
});
socket.addEventListener("close", (event) => {
  if (!event.wasClean) console.error(event.code, event.reason);
});
```

**Sources:**

- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [MDN — WebSocket.close()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>


### WebSockets Server-Side Implementation

<details>
<summary>How would you implement a WebSocket server in Node.js?</summary>

### How would you implement a WebSocket server in Node.js?

**Definition:**

Node does not parse WebSocket frames for you. The `ws` package provides `WebSocketServer`, which completes the HTTP upgrade and emits `connection` with a socket that speaks `message`, `send`, `ping`, and `close`.

**How it works:**

- **Listen**: `new WebSocketServer({ port: 8080 })` opens its own HTTP server. Pass `{ server }` to share an existing HTTP or HTTPS server.
- **Messages**: On `connection`, listen for `message`. The callback gets `data` and an `isBinary` flag. Echo with `ws.send`.
- **Several paths**: Use `{ noServer: true }` and, on the HTTP `upgrade` event, call `handleUpgrade` only for the paths you accept. Destroy the socket otherwise.

**Examples:**

```js
import { WebSocketServer } from "ws";

const wss = new WebSocketServer({ port: 8080 });
wss.on("connection", (socket) => {
  socket.on("message", (data) => socket.send(data));
});
```

**Sources:**

- [ws — simple server](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>What is the role of WebSocket server libraries, such as ws or Socket.IO?</summary>

### What is the role of WebSocket server libraries, such as ws or Socket.IO?

**Definition:**

`ws` implements the WebSocket protocol for Node: the upgrade, masking, fragmentation, ping/pong, and optional permessage-deflate. Socket.IO is a different protocol that may use a WebSocket as its transport.

**How it works:**

- **ws**: A browser `WebSocket` can connect to a `ws` server. The readme says `ws` passes the Autobahn test suite and does not run in the browser.
- **Socket.IO**: The Socket.IO docs say it is not a WebSocket implementation. A raw client cannot connect to a Socket.IO server, because each packet adds Engine.IO and Socket.IO framing.
- **What you skip writing**: You do not XOR masks or assemble FIN bits. You do still define authentication, rooms, and what a text payload means.

**When to use:**

- Use `ws` when the other side is a browser `WebSocket` or another RFC 6455 stack.
- Use Socket.IO when every client will use the Socket.IO client and you want its fallback, acknowledgements, and rooms.

**Sources:**

- [ws](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>

<details>
<summary>How do servers handle WebSocket connection upgrades?</summary>

### How do servers handle WebSocket connection upgrades?

**Definition:**

The client sends `GET` with `Upgrade: websocket`, `Connection: Upgrade`, `Sec-WebSocket-Key`, and `Sec-WebSocket-Version: 13`. The server replies `101 Switching Protocols` and `Sec-WebSocket-Accept`.

**How it works:**

- **Accept value**: Concatenate the key with `258EAFA5-E914-47DA-95CA-C5AB0DC85B11`, take SHA-1, and base64-encode it. For the sample key `dGhlIHNhbXBsZSBub25jZQ==`, the accept value is `s3pPLMBiTxaQ9kYGzzhZRbK+xOo=`.
- **Reject**: An unknown header or a bad value is `400` and the socket closes. An unsupported version sends back `Sec-WebSocket-Version`. HTTP status codes apply only before a successful handshake.
- **Libraries**: `ws` does this inside `WebSocketServer`, or when you call `handleUpgrade` from an HTTP `upgrade` listener.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — handleUpgrade](https://github.com/websockets/ws)
</details>

<details>
<summary>How would you authenticate WebSocket connections on the server side?</summary>

### How would you authenticate WebSocket connections on the server side?

**Definition:**

Authenticate during the HTTP upgrade, before you treat the socket as a chat member. The `ws` readme shows an `upgrade` listener that calls your `authenticate` function and answers `401` if it fails.

**How it works:**

- **What the request has**: MDN says the handshake may include `Cookie` and other HTTP headers. Browsers also send `Origin`. The browser constructor cannot set an `Authorization` header.
- **Fail closed**: In the `ws` example, a failed `authenticate` writes `HTTP/1.1 401 Unauthorized` and destroys the socket. Only then does `handleUpgrade` run.
- **After open**: If the token cannot ride on a cookie, require the first application message to carry it, and ignore other frames until that check passes. Close with a private code if it does not.

**Examples:**

```js
server.on("upgrade", (request, socket, head) => {
  authenticate(request, (err, client) => {
    if (err || !client) {
      socket.write("HTTP/1.1 401 Unauthorized\r\n\r\n");
      socket.destroy();
      return;
    }
    wss.handleUpgrade(request, socket, head, (ws) => {
      wss.emit("connection", ws, request, client);
    });
  });
});
```

**Sources:**

- [ws — client authentication](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>Explain how you can integrate WebSockets with existing web application frameworks.</summary>

### Explain how you can integrate WebSockets with existing web application frameworks.

**Definition:**

Share the HTTP server the framework already listens on. WebSocket upgrades are `upgrade` events on that server, not ordinary route handlers that write a body.

**How it works:**

- **Attach**: The `ws` readme constructs `WebSocketServer` with `{ server }` where `server` is an `https` or `http` server. The same pattern works when Express or another framework created that server.
- **Paths**: With `{ noServer: true }`, branch on `request.url` inside `upgrade` and call `handleUpgrade` for `/chat` while leaving other paths to the framework.
- **Proxy**: MDN says a reverse proxy often detects the handshake and forwards it to a dedicated WebSocket process, so the app server does not also parse cookies for every frame.

**Sources:**

- [ws — external HTTP server](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>


### WebSockets and Security

<details>
<summary>What is WebSocket Secure (WSS), and when should you use it?</summary>

### What is WebSocket Secure (WSS), and when should you use it?

**Definition:**

`wss` is the WebSocket handshake over TLS. `ws` is the same framing without TLS. MDN says browsers generally require a secure connection, with a possible exception for local devices.

**How it works:**

- **URL**: The constructor accepts `wss` and `https` as schemes. A page loaded over HTTPS cannot open `ws://` to another host; the browser treats that as mixed content.
- **Masking still applies**: MDN says client frames are masked even on a secure socket. TLS does not replace the MASK bit.
- **Server**: The `ws` example for HTTPS passes `cert` and `key` to `https.createServer`, then attaches `WebSocketServer` to that server.

**When to use:**

- Use `wss` for any connection that leaves the machine.
- `ws://localhost` is the usual exception while developing.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — HTTPS server](https://github.com/websockets/ws)
</details>

<details>
<summary>How do you handle session management in WebSocket connections?</summary>

### How do you handle session management in WebSocket connections?

**Definition:**

The handshake can carry the same cookie as the rest of the site. After `101`, you keep your own record of that user next to the socket, because later frames do not repeat the cookie.

**How it works:**

- **At upgrade**: Read the session from the HTTP request, as the `ws` authentication example does, and pass the user into the `connection` handler.
- **While open**: MDN says to keep a table of usernames or ids beside the socket. That table is the session for the life of the TCP connection.
- **When it dies**: Delete the row on `close`. A reconnect is a new handshake and must be checked again. Do not trust a user id that arrives only inside a later text frame.

**Sources:**

- [ws — client authentication](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>What measures can be taken to secure WebSocket transmissions?</summary>

### What measures can be taken to secure WebSocket transmissions?

**Definition:**

Use `wss`, check `Origin`, authenticate the upgrade, and limit how many sockets one client may hold. Frame masking is required from client to server, and it is not a substitute for TLS.

**How it works:**

- **TLS**: Prefer port 443 and `wss`. MDN warns that other ports have more trouble with firewalls and that browsers generally want a secure connection.
- **Origin**: Browsers send `Origin`. MDN says you can reject a mismatch with `403`. Non-browser clients can forge that header, so also require a session or token.
- **Limits**: Refuse clients that open too many connections. Cap message size and close with `1009` when a message is too big to process.
- **Payload**: Treat text as data, not as HTML to insert into the page. Validate UTF-8; invalid text is close code `1007`.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>How can Cross-Site WebSocket Hijacking (CSWSH) be prevented?</summary>

### How can Cross-Site WebSocket Hijacking (CSWSH) be prevented?

**Definition:**

CSWSH is a page on another origin opening a WebSocket to your server and riding the user’s cookies. MDN says checking the `Origin` header and answering `403` is effective against that for browsers.

**How it works:**

- **Origin**: Browsers send `Origin` on the handshake. Allow only your site’s origins. Missing `Origin` is a reason to reject a browser client.
- **Forge**: MDN warns that a non-browser agent can send a fake `Origin`. Pair the check with a cookie `SameSite` policy and a session that is not enough by itself for a dangerous action.
- **Don’t trust the first frame alone**: A malicious page can send frames after a cookie-authenticated upgrade. Reject the upgrade when `Origin` is wrong so those frames never start.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>Can you use OAuth or tokens with WebSocket connections?</summary>

### Can you use OAuth or tokens with WebSocket connections?

**Definition:**

Yes, but the browser `WebSocket` constructor cannot set `Authorization`. The token has to travel as a cookie on the handshake, or as a value you check yourself before you trust the socket.

**How it works:**

- **Cookie session**: If the OAuth flow already set an `HttpOnly` session cookie, the handshake sends it. Validate that session in the `upgrade` listener, as in the `ws` `401` example.
- **Bearer token**: A non-browser client can set the header. A page cannot. Common page patterns are a short-lived ticket in the query string, or the token in the first message after `open`.
- **Lifetime**: A long-lived socket does not repeat the OAuth handshake. When the token expires, close the socket and let the client reconnect with a new one.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [ws — client authentication](https://github.com/websockets/ws)
</details>


### WebSockets Testing and Debugging

<details>
<summary>What tools are available for testing WebSocket connections?</summary>

### What tools are available for testing WebSocket connections?

**Definition:**

Use a browser `WebSocket` in the page, the Node `ws` client, and the browser’s network panel. Protocol compatibility is checked with the Autobahn suite, which the `ws` readme says its client and server pass.

**How it works:**

- **DevTools**: The handshake is an HTTP request that switches to frames. You can see the `101` response and later text frames there.
- **A small client**: The `ws` client example connects with `new WebSocket(url)` from Node, sends on `open`, and logs `message`. That is a scripted check, not a browser.
- **Autobahn**: `ws` publishes Autobahn reports for its server and client. That suite exercises framing, fragmentation, and UTF-8, not your chat schema.

**Sources:**

- [ws](https://github.com/websockets/ws)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>How do you debug a WebSocket application?</summary>

### How do you debug a WebSocket application?

**Definition:**

Separate handshake failures from frame bugs. A failed upgrade is an HTTP status. A failed conversation is a `close` code, a stuck `bufferedAmount`, or a payload that does not match the schema.

**How it works:**

- **Handshake**: Expect `101`, `Upgrade`, `Connection`, and `Sec-WebSocket-Accept`. `400` or a missing accept value means you never left HTTP.
- **Close code**: `1006` means no Close frame (the TCP connection died). `1002` is a protocol error. `1007` is bad UTF-8. `1008` is policy. `1009` is too big.
- **Liveness**: The `ws` FAQ uses ping every 30 seconds and `terminate()` when a pong does not clear the `isAlive` flag. That finds half-open connections.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — broken connections](https://github.com/websockets/ws)
- [RFC 6455 — status codes](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>What are the challenges when writing automated tests for WebSocket applications?</summary>

### What are the challenges when writing automated tests for WebSocket applications?

**Definition:**

The handshake is asynchronous, messages can split across frames, and a test must not pass just because `send` returned. You assert on the peer’s `message` or on a `close` code.

**How it works:**

- **Wait for open**: `send` throws while `CONNECTING`. The test has to wait for `open` or the `ws` `open` callback.
- **Ordering**: Frames on one connection are ordered, but your test’s timers are not. Drive the server with one client and assert the next `message`, instead of sleeping a fixed time.
- **Cleanup**: Call `close` and wait for `close`, or the process keeps the port. Half-open sockets need the ping timeout from the `ws` FAQ or they look alive.

**Sources:**

- [MDN — WebSocket.send()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/send)
- [ws](https://github.com/websockets/ws)
</details>


### Advanced WebSockets Topics

<details>
<summary>How can WebSockets be used in distributed systems?</summary>

### How can WebSockets be used in distributed systems?

**Definition:**

A WebSocket stays on the process that accepted the TCP connection. Other services learn about it only if that process publishes the payload somewhere they can read.

**How it works:**

- **Edge of the system**: Browsers connect to a gateway that speaks RFC 6455. Behind it, services keep using HTTP or a queue. MDN already describes a proxy in front of a dedicated WebSocket server.
- **Fan-out**: The gateway publishes each inbound message to a bus. Every gateway instance consumes messages for users whose sockets it holds.
- **No shared socket**: You cannot move an open TCP connection to another process by copying an id. A reconnect creates a new socket on whichever instance accepts it.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — broadcast](https://github.com/websockets/ws)
</details>

<details>
<summary>Explain the role of WebSockets in IoT (Internet of Things).</summary>

### Explain the role of WebSockets in IoT (Internet of Things).

**Definition:**

A device that can open TCP can keep a WebSocket and receive commands without polling. The framing is the same RFC 6455 handshake and masked client frames.

**How it works:**

- **Direction**: Either side can send after the handshake. A server push is a server frame with MASK 0. The device’s frames are masked.
- **Small payloads**: Binary opcode `0x2` carries sensor bytes. Ping/pong shows the device is still reachable when it has nothing to report.
- **Not every device**: MDN’s server guide assumes a TCP stack and an HTTP handshake. A device that cannot speak that handshake needs a gateway that does, then a simpler link on the far side.

**When to use:**

- Use WebSocket when the device or a nearby gateway already has HTTP and you need server-pushed commands.
- Use a messaging protocol aimed at constrained devices when RAM and radio duty cycle matter more than browser compatibility.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>How does WebSocket protocol handle backpressure or throttling?</summary>

### How does WebSocket protocol handle backpressure or throttling?

**Definition:**

The protocol relies on TCP for transport backpressure. The browser exposes the result as `bufferedAmount`: bytes accepted by `send()` that have not been written to the network.

**How it works:**

- **No app window**: RFC 6455 does not define a WebSocket receive window. If the peer reads slowly, TCP shrinks the sender’s window and `bufferedAmount` climbs.
- **What does not reset it**: MDN: the value goes to zero when the queue has been sent. Closing does not zero it. Further `send()` calls still add to it.
- **Application throttle**: Stop calling `send` while `bufferedAmount` is above a limit you choose, or close with `1009` when one message is larger than you will process.

**Sources:**

- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/bufferedAmount)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>What strategies can be employed for WebSocket versioning and backward compatibility?</summary>

### What strategies can be employed for WebSocket versioning and backward compatibility?

**Definition:**

The protocol version on the handshake is `Sec-WebSocket-Version: 13`. Your application version belongs in a subprotocol name or in the payload, not in a new opcode.

**How it works:**

- **Protocol version**: MDN: if the server does not understand the client’s version, it responds with `Sec-WebSocket-Version` listing versions it does understand, and does not switch protocols.
- **`ws` drafts**: The `ws` readme supports HyBi drafts 13–17 by default (`protocolVersion: 13`) and older drafts 07–12 with `protocolVersion: 8`. Browsers speak the RFC version.
- **Your schema**: Offer `chat.example.com.v2` and `chat.example.com.v1` in the constructor. The server picks one `Sec-WebSocket-Protocol`. Old clients keep working because they never offer the new name.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — protocol support](https://github.com/websockets/ws)
</details>

<details>
<summary>How do you handle reconnection and state sync after a WebSocket reconnects?</summary>

### How do you handle reconnection and state sync after a WebSocket reconnects?

**Definition:**

A dropped socket does not resume. The client opens a new `WebSocket`, authenticates again, and asks for anything it missed. `1006` means the old connection died without a Close frame.

**How it works:**

- **Detect**: Listen for `close`. If `code` is `1000`, the peer finished on purpose. Otherwise schedule a new constructor call.
- **Backoff**: Wait longer after each failure so a dead server is not hit by every client at once. Socket.IO documents this as an exponential back-off that it does for you; with raw WebSocket you write the timer.
- **Sync**: Give each application message an id. On `open`, send the last id you applied. The server replies with the gap, or with a snapshot if the gap is too large. HTTP can serve the snapshot.

**Examples:**

```js
socket.addEventListener("close", (event) => {
  if (event.code === 1000) return;
  setTimeout(connect, 1000);
});
```

**Sources:**

- [MDN — WebSocket.close()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close)
- [Socket.IO — reconnection](https://socket.io/docs/v4/)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>Discuss the impact of network latency on WebSocket applications and how to mitigate it.</summary>

### Discuss the impact of network latency on WebSocket applications and how to mitigate it.

**Definition:**

After the handshake, a small frame still waits on the network’s round trip and on every earlier byte of that TCP connection. The handshake itself is at least the HTTP upgrade plus TLS when the URL is `wss`.

**How it works:**

- **One connection**: You pay the handshake once, then many messages share it. That is the point of the upgrade MDN describes.
- **Head of line**: A lost TCP segment stalls later WebSocket frames, including a newer position update. Coalesce updates in the client while `bufferedAmount` is high so you send the latest state, not every intermediate one.
- **Distance**: Put the process that holds the sockets near the users. A chat message still crosses the path between the two browsers via your server.

**Sources:**

- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/bufferedAmount)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>What are some patterns for integrating WebSocket communications in microservices architectures?</summary>

### What are some patterns for integrating WebSocket communications in microservices architectures?

**Definition:**

Keep the sockets in a gateway service. Other services publish events; the gateway writes frames only to sockets it accepted.

**How it works:**

- **Gateway**: MDN describes a reverse proxy in front of a dedicated WebSocket server. That server is the only place that stores the client table.
- **Events in, frames out**: A chat service emits “message created”. Each gateway instance receives it and `send`s to local sockets in that room.
- **Commands**: An inbound frame becomes a normal request to another service. The socket process should not own the business database by itself.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — broadcast](https://github.com/websockets/ws)
</details>

<details>
<summary>Can you explain the concept of WebSocket tunneling and when it might be used?</summary>

### Can you explain the concept of WebSocket tunneling and when it might be used?

**Definition:**

Tunneling here means carrying the WebSocket handshake and frames through a proxy that would otherwise allow only ordinary HTTP. The `ws` FAQ connects via an HTTP or SOCKS proxy using a custom `http.Agent`.

**How it works:**

- **Why**: Some networks allow TLS to port 443 and block other ports. `wss` on 443 looks like HTTPS until the upgrade, which is why MDN prefers 80 and 443.
- **Client side**: The `ws` readme points at `https-proxy-agent` and `socks-proxy-agent` as the agent you pass when the Node client must dial through a proxy.
- **Not a second protocol**: After the proxy connects, the bytes are still RFC 6455 frames. The application schema does not change.

**When to use:**

- Use it when a corporate proxy or a locked-down network is why the raw socket cannot dial.
- Do not add a tunnel when the browser can open `wss` directly.

**Sources:**

- [ws — connect via a proxy](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>How do WebSockets fit into the concept of edge computing and serverless architectures?</summary>

### How do WebSockets fit into the concept of edge computing and serverless architectures?

**Definition:**

A WebSocket needs a process that stays alive for the life of the TCP connection. A function that starts, handles one HTTP request, and exits does not hold that socket.

**How it works:**

- **Long-lived process**: MDN’s server keeps a client table after the handshake. That table exists only while the process exists.
- **Edge**: Running that process near the user shortens the path. The connection is still stateful on that one instance.
- **Split**: Put request/response work in short functions. Put the sockets in a small always-on gateway that calls those functions when a frame arrives.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>What considerations are there for using WebSockets in mobile applications?</summary>

### What considerations are there for using WebSockets in mobile applications?

**Definition:**

A phone radio and the OS will suspend a background app. The socket then dies, often with close code `1006`, and the app must reconnect when it is in the foreground again.

**How it works:**

- **Foreground**: Keep the socket while the user is in the screen that needs live data. Close it with `1000` when they leave, if you do not need updates.
- **Background**: Socket.IO’s docs say an open TCP connection is a poor background channel and point to a push service for that. The same limit applies to a raw WebSocket.
- **Retry**: On `close`, reconnect with backoff when the app is active, then send the last applied message id so the server can fill the gap.

**Sources:**

- [Socket.IO](https://socket.io/docs/v4/)
- [MDN — WebSocket.close()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close)
</details>


### WebSockets Libraries and Frameworks

<details>
<summary>Comparatively, what features do WebSocket libraries like ws, WebSocket-Node, and Socket.IO offer?</summary>

### Comparatively, what features do WebSocket libraries like ws, WebSocket-Node, and Socket.IO offer?

**Definition:**

`ws` and WebSocket-Node implement RFC 6455 for Node. Socket.IO adds its own framing, a long-polling fallback, and rooms, and it is not a raw WebSocket peer.

**How it works:**

- **ws**: Client and server, Autobahn reports, `WebSocketServer`, `handleUpgrade`, broadcast via `wss.clients`, ping helpers, and optional permessage-deflate. It does not run in the browser.
- **WebSocket-Node**: A pure JavaScript client and server for Node that implements RFC 6455. Use it when you want that stack; browser pages still use the native `WebSocket`.
- **Socket.IO**: Transports include HTTP long-polling, WebSocket, and WebTransport. Features include reconnection, buffering while disconnected, acknowledgements, namespaces, and rooms. A browser `WebSocket` cannot connect to it.

**Sources:**

- [ws](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>

<details>
<summary>How does Socket.IO enhance or differ from the standard WebSocket API?</summary>

### How does Socket.IO enhance or differ from the standard WebSocket API?

**Definition:**

Socket.IO keeps a connection and names events. Under that, it may use a WebSocket, but every packet has extra Engine.IO and Socket.IO bytes, so it is not the browser `WebSocket` protocol by itself.

**How it works:**

- **Not interchangeable**: The docs show that `io("ws://echo.websocket.org")` will not connect, and a raw client will not connect to a Socket.IO server. They point at `ws` for a plain server.
- **Extra behavior**: Long-polling fallback, automatic reconnect with backoff, a buffer while disconnected, `emit` acknowledgements, `io.to(room).emit`, and namespaces on one connection.
- **Overhead**: The docs say `socket.emit("hello", "world")` goes out as `42["hello","world"]`: Engine.IO type `4`, Socket.IO type `2`, then JSON.

**Sources:**

- [Socket.IO](https://socket.io/docs/v4/)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>Discuss the advantages of using a framework like SignalR for real-time communication.</summary>

### Discuss the advantages of using a framework like SignalR for real-time communication.

**Definition:**

ASP.NET Core SignalR is a library for server-to-client calls. It prefers WebSockets, then Server-Sent Events, then long polling, and it targets clients or groups without you managing each frame.

**How it works:**

- **Microsoft’s advice**: The WebSockets article says to prefer SignalR for most apps: transport fallback, an RPC model, and no significant performance disadvantage versus raw WebSockets in most cases.
- **Hubs**: A hub lets the client and server call methods on each other. Protocols are JSON by default and MessagePack as a smaller binary option.
- **Scale**: The introduction lists groups, a Redis backplane, and Azure SignalR Service for more traffic. Raw sockets still need that fan-out if you run more than one process.

**When to use:**

- Use SignalR for a .NET app that needs chat, dashboards, or notifications and can use a SignalR client.
- Use `UseWebSockets` when the peer is a browser `WebSocket` that you do not control.

**Sources:**

- [ASP.NET Core SignalR](https://learn.microsoft.com/en-us/aspnet/core/signalr/introduction)
- [ASP.NET Core WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets)
</details>

<details>
<summary>What are some client-side libraries for working with WebSockets?</summary>

### What are some client-side libraries for working with WebSockets?

**Definition:**

In the browser the client is the built-in `WebSocket`. Libraries on top either wrap that object or speak a different protocol such as Socket.IO.

**How it works:**

- **Native**: `new WebSocket(url, protocols)` is what MDN documents. It runs in pages and in workers.
- **Same code on Node**: The `ws` readme says the package does not work in the browser and points at a wrapper such as `isomorphic-ws` when one module must construct a socket in both places.
- **Socket.IO client**: The JavaScript client speaks the Socket.IO protocol, including the long-polling fallback. It is not a drop-in replacement for `WebSocket`.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [ws](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>

<details>
<summary>How do you choose a WebSocket library or framework for a project?</summary>

### How do you choose a WebSocket library or framework for a project?

**Definition:**

Choose from the peer you must talk to. A browser `WebSocket` needs an RFC 6455 server. A Socket.IO or SignalR client needs that same stack on the server.

**How it works:**

- **Raw frames**: `ws` on Node, the Python `websockets` server, Java `Endpoint`, ASP.NET `AcceptWebSocketAsync`, or `coder/websocket` in Go all speak the protocol the browser speaks.
- **App features**: If you would otherwise build reconnect, rooms, and a fallback, Socket.IO or SignalR already document those. You then cannot mix in a raw peer.
- **Tests**: Prefer a stack that passes Autobahn, as `ws` and `coder/websocket` state they do, so framing bugs are not yours.

**Sources:**

- [ws](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
- [ASP.NET Core SignalR](https://learn.microsoft.com/en-us/aspnet/core/signalr/introduction)
</details>


### WebSockets Performance and Scaling

<details>
<summary>Describe the challenges of scaling WebSocket applications horizontally.</summary>

### Describe the challenges of scaling WebSocket applications horizontally.

**Definition:**

Horizontal scale adds processes, but each TCP connection stays on one of them. A message accepted on process A is invisible to sockets on process B unless you forward it.

**How it works:**

- **Affinity**: The load balancer must keep the connection on the process that completed the upgrade. A later packet to another process has no socket.
- **Broadcast**: `wss.clients` is local to one `WebSocketServer`. A shared bus has to carry the payload to every instance, which then writes to its own clients.
- **Deploys**: Restarting a process closes its sockets. Clients see `close` and must reconnect, landing on whichever instance is up.

**Sources:**

- [ws — broadcast](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>How can you measure WebSocket application performance?</summary>

### How can you measure WebSocket application performance?

**Definition:**

Measure the handshake separately from steady-state frames. Useful numbers are time to `open`, round trip of one payload, `bufferedAmount`, and how many open sockets a process holds.

**How it works:**

- **Round trip**: The `ws` readme sends `Date.now()` and subtracts it from the echoed value. That includes your server, not just the network.
- **Queue**: A rising `bufferedAmount` means the browser is accepting sends faster than the network drains. That is the backpressure signal MDN exposes.
- **Compression**: The `ws` docs warn that permessage-deflate can hurt memory under concurrency. Compare a run with it off before you treat CPU as a WebSocket cost.

**Sources:**

- [ws — round-trip time](https://github.com/websockets/ws)
- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>What are the implications of using sticky sessions with WebSockets?</summary>

### What are the implications of using sticky sessions with WebSockets?

**Definition:**

A sticky rule pins the TCP connection to the instance that accepted the upgrade. You need that pin because no other instance has the socket. You still need a bus if one user’s message must reach a socket on another instance.

**How it works:**

- **What stickiness fixes**: The handshake and every later frame hit the same process. Without it, a balancer can send frame bytes to a process that never saw `101`.
- **What it does not fix**: `wss.clients` on instance A does not include sockets on instance B. Chat between those users needs a publish step.
- **Failure**: If the sticky instance dies, the connection dies. The client reconnects and may stick to a different instance. Sync state on `open`, not from process memory alone.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws](https://github.com/websockets/ws)
</details>

<details>
<summary>How does load balancing work with WebSocket traffic?</summary>

### How does load balancing work with WebSocket traffic?

**Definition:**

The balancer must forward the HTTP upgrade and then the raw TCP bytes of that connection to the same upstream. It is not a series of independent requests.

**How it works:**

- **Upgrade**: MDN’s handshake is `GET` plus `Upgrade: websocket` and a `101` response. The proxy has to allow that switch and keep the tunnel open.
- **Idle**: A quiet socket can look idle to a proxy. Ping and pong, or ASP.NET’s `KeepAliveInterval`, exist so the path does not drop a live connection.
- **Choice of upstream**: Balance new handshakes. Do not balance each frame. Connection limits on one upstream matter more than request-per-second limits.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ASP.NET Core WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets)
</details>

<details>
<summary>What are connection limits and resource usage considerations for WebSocket servers?</summary>

### What are connection limits and resource usage considerations for WebSocket servers?

**Definition:**

Each client is a socket plus your session row, held until `close`. File descriptors, memory, and the size of the client table bound how many you can accept.

**How it works:**

- **Table**: MDN says to track clients and to deny those that connect too often, as a denial-of-service limit.
- **Messages**: Close with `1009` when a message is too big. Fragmented messages still have to be buffered until FIN.
- **Compression**: `ws` disables permessage-deflate on the server by default because zlib under concurrency can fragment memory.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — compression](https://github.com/websockets/ws)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455#section-7.4)
</details>


### WebSockets Interoperability and Compatibility

<details>
<summary>How do you ensure compatibility between different WebSocket implementations?</summary>

### How do you ensure compatibility between different WebSocket implementations?

**Definition:**

Speak RFC 6455 version 13, including masked client frames and the `Sec-WebSocket-Accept` hash. Use a stack that passes Autobahn rather than a partial frame parser.

**How it works:**

- **Version header**: Clients send `Sec-WebSocket-Version: 13`. A server that does not understand it must say which versions it does.
- **Suites**: `ws` and `github.com/coder/websocket` both state that they pass Autobahn. That covers framing more than your JSON schema.
- **Subprotocol**: Agree on a `Sec-WebSocket-Protocol` name so a binary layout from one library is not read as text by another.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws](https://github.com/websockets/ws)
- [coder/websocket](https://pkg.go.dev/github.com/coder/websocket)
</details>

<details>
<summary>Can WebSockets communicate with services using other protocols?</summary>

### Can WebSockets communicate with services using other protocols?

**Definition:**

Not on the same byte stream. A gateway reads WebSocket messages and calls the other service with HTTP, a queue, or that service’s own client.

**How it works:**

- **Translate**: An inbound text frame becomes a request body. The response becomes `send()` on the socket that asked.
- **Do not mix frames**: After `101`, the connection is not HTTP. MDN says HTTP status codes apply only before the handshake succeeds.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>How do WebSockets integrate with REST APIs or GraphQL?</summary>

### How do WebSockets integrate with REST APIs or GraphQL?

**Definition:**

REST or GraphQL answers a request. The WebSocket pushes the next change. Use both: load the resource over HTTP, then subscribe on the socket for later updates.

**How it works:**

- **Snapshot plus tail**:  `fetch` or a GraphQL query returns the current document. The socket sends patches or ids of what changed.
- **Same auth**: The handshake can send the session cookie the REST API already uses. Check it before `101`, as in the `ws` upgrade example.
- **GraphQL over frames**: Some servers accept GraphQL operations inside text frames. That is an application subprotocol. It is still not the HTTP GraphQL endpoint.

**When to use:**

- Keep create, update, and “load this page” on the API you already have.
- Add the socket where the server must speak first.

**Sources:**

- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [ws — client authentication](https://github.com/websockets/ws)
</details>

<details>
<summary>What are the options for using WebSockets in hybrid applications with web views?</summary>

### What are the options for using WebSockets in hybrid applications with web views?

**Definition:**

A web view can use the same `new WebSocket` as a browser page, if the view’s origin is allowed to open `wss` and the server accepts that `Origin`.

**How it works:**

- **Page inside the app**: MDN’s constructor and events work in the view. Cookies and `Origin` follow the view’s URL, not the native shell.
- **Native stack**: The shell can open its own RFC 6455 client and pass messages into the view. Then you have two connections unless you pick one.
- **Origin check**: A custom scheme or `file` origin will fail a strict allow-list. Decide the origin the view will send and allow that value.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>Describe any challenges with using WebSockets in cross-platform environments.</summary>

### Describe any challenges with using WebSockets in cross-platform environments.

**Definition:**

The wire format is shared. The APIs are not. A browser `WebSocket`, Node `ws`, Java `Session`, and Go `websocket.Conn` do not share method names, and a Socket.IO peer is a different protocol.

**How it works:**

- **Headers**: Only non-browser clients can set arbitrary handshake headers. Pages have `url` and `protocols`.
- **Binary**: Browsers need `binaryType`. Other stacks give you bytes directly. Agree on the layout in a subprotocol name.
- **Tests**: Run Autobahn, or at least a browser client against the server, before assuming a library matches RFC 6455.

**Sources:**

- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
- [ws](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>


### WebSockets in Different Languages and Environments

<details>
<summary>How do you establish a WebSocket connection in Java?</summary>

### How do you establish a WebSocket connection in Java?

**Definition:**

Java EE’s WebSocket API (JSR 356) lets you extend `Endpoint` and register it at a path. The container performs the handshake; you handle messages on the `Session`.

**How it works:**

- **Programmatic endpoint**: The tutorial extends `Endpoint` and implements `onOpen(Session, EndpointConfig)`. `onClose` and `onError` are the other lifecycle methods.
- **Messages**: `session.addMessageHandler` receives a whole text message. `session.getBasicRemote().sendText` sends one back.
- **Deploy**: `ServerEndpointConfig.Builder.create(EchoEndpoint.class, "/echo").build()` publishes it. The tutorial’s URL shape is `ws://localhost:8080/echoapp/echo`.

**Examples:**

```java
public class EchoEndpoint extends Endpoint {
  @Override
  public void onOpen(Session session, EndpointConfig config) {
    session.addMessageHandler(new MessageHandler.Whole<String>() {
      @Override
      public void onMessage(String msg) {
        session.getBasicRemote().sendText(msg);
      }
    });
  }
}
```

**Sources:**

- [Java EE tutorial — programmatic endpoints](https://docs.oracle.com/javaee/7/tutorial/websocket003.htm)
</details>

<details>
<summary>What support is available for WebSockets in the .NET environment?</summary>

### What support is available for WebSockets in the .NET environment?

**Definition:**

ASP.NET Core accepts raw WebSockets with `app.UseWebSockets()` and `AcceptWebSocketAsync()`. SignalR is the higher-level library Microsoft recommends for most real-time apps.

**How it works:**

- **Middleware**: `UseWebSockets` can set `KeepAliveInterval` (default two minutes) and `AllowedOrigins`. `KeepAliveTimeout` aborts the socket if a pong does not arrive.
- **Accept**: Check `context.WebSockets.IsWebSocketRequest`, then `AcceptWebSocketAsync()`. The sample uses the path `/ws`.
- **HTTP/2**: Kestrel supports WebSockets over HTTP/2 (RFC 8441). Those use `CONNECT` rather than `GET`.

**Examples:**

```csharp
app.UseWebSockets();
app.Use(async (context, next) => {
  if (context.Request.Path == "/ws" && context.WebSockets.IsWebSocketRequest) {
    using var webSocket = await context.WebSockets.AcceptWebSocketAsync();
    await Echo(webSocket);
  } else {
    await next(context);
  }
});
```

**Sources:**

- [ASP.NET Core WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets)
- [ASP.NET Core SignalR](https://learn.microsoft.com/en-us/aspnet/core/signalr/introduction)
</details>

<details>
<summary>Discuss how to implement WebSocket communication in Python.</summary>

### Discuss how to implement WebSocket communication in Python.

**Definition:**

The `websockets` library serves RFC 6455 on asyncio. You `await websocket.recv()` and `await websocket.send()` inside a handler passed to `serve`.

**How it works:**

- **Serve**: The tutorial imports `serve` from `websockets.asyncio.server`. `serve(handler, "", 8001)` listens on port 8001, then `serve_forever()` keeps the process up.
- **Handler**: The handler is `async def handler(websocket)`. A loop around `recv()` reads the next message. The browser uses the native `WebSocket` against this server.
- **Runtime**: Current docs require Python 3.11 or newer and `pip install websockets`.

**Examples:**

```python
import asyncio
from websockets.asyncio.server import serve

async def handler(websocket):
    while True:
        message = await websocket.recv()
        await websocket.send(message)

async def main():
    server = await serve(handler, "", 8001)
    await server.serve_forever()

asyncio.run(main())
```

**Sources:**

- [websockets tutorial](https://websockets.readthedocs.io/en/stable/intro/tutorial1.html)
</details>

<details>
<summary>How can you use WebSockets in a PHP-based application?</summary>

### How can you use WebSockets in a PHP-based application?

**Definition:**

PHP does not include a WebSocket server in the language. MDN says any language with Berkeley sockets can implement the handshake and frames, and it names PHP as one of those languages.

**How it works:**

- **Not a request**: A typical PHP page starts and ends with one HTTP response. A WebSocket stays open after `101`, so it needs a long-running process, not a script that exits.
- **Same handshake**: That process must still answer with `Sec-WebSocket-Accept` derived from the GUID `258EAFA5-E914-47DA-95CA-C5AB0DC85B11` and then unmask client frames.
- **Split**: Keep PHP for the HTTP app. Run the socket process beside it, and publish between them. The browser still uses `new WebSocket`.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>Explain how to create a WebSocket server using Go.</summary>

### Explain how to create a WebSocket server using Go.

**Definition:**

`github.com/coder/websocket` accepts a handshake from an `http.Handler` with `websocket.Accept`. `golang.org/x/net/websocket` is deprecated.

**How it works:**

- **Accept**: The readme calls `websocket.Accept(w, r, nil)` inside an `http.HandlerFunc`, then `defer c.CloseNow()`.
- **JSON**: `wsjson.Read` and `wsjson.Write` use a `context`. The sample sets a timeout with `context.WithTimeout` instead of using `r.Context()`.
- **Close**: The sample finishes with `c.Close(websocket.StatusNormalClosure, "")`, which is the normal closure status.

**Examples:**

```go
c, err := websocket.Accept(w, r, nil)
if err != nil {
  return
}
defer c.CloseNow()
err = wsjson.Write(ctx, c, "hi")
c.Close(websocket.StatusNormalClosure, "")
```

**Sources:**

- [coder/websocket](https://pkg.go.dev/github.com/coder/websocket)
</details>


### WebSockets and Cross-Origin Communication

<details>
<summary>What is the Same-Origin Policy and how does it apply to WebSockets?</summary>

### What is the Same-Origin Policy and how does it apply to WebSockets?

**Definition:**

The browser sends the page’s `Origin` on the WebSocket handshake. It does not apply a CORS preflight. Your server decides whether that origin may use the user’s cookies on the socket.

**How it works:**

- **What is automatic**: The page may call `new WebSocket` to another host. The constructor does not take an origin; the browser sets `Origin`.
- **What you enforce**: MDN says to check `Origin` and return `403` when it is not yours. That blocks another site from using the user’s cookies (CSWSH).
- **Non-browsers**: They can forge `Origin`. Authenticate the upgrade as well.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
</details>

<details>
<summary>How can you safely enable cross-origin WebSocket connections?</summary>

### How can you safely enable cross-origin WebSocket connections?

**Definition:**

Allow a list of origins on the handshake, require a real session, and use `wss`. An open allow-list plus cookies is how another site hijacks the socket.

**How it works:**

- **Allow list**: Compare the `Origin` header to the sites you own. ASP.NET’s `WebSocketOptions.AllowedOrigins` is that list; the default is to allow all origins, so set it when cookies matter.
- **Reject**: MDN: `403` and close the TCP connection before `101` when the origin is wrong.
- **Credentials**: A cookie session is enough only together with the origin check. A public socket that does not use cookies can allow more origins.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ASP.NET Core WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets)
</details>

<details>
<summary>What is CORS (Cross-Origin Resource Sharing) and how does it relate to WebSockets?</summary>

### What is CORS (Cross-Origin Resource Sharing) and how does it relate to WebSockets?

**Definition:**

CORS is how browsers relax the same-origin policy for HTTP APIs using response headers. The WebSocket handshake does not use that CORS header dance. It sends `Origin`, and the server accepts or rejects the upgrade.

**How it works:**

- **No preflight**: You do not configure `Access-Control-Allow-Origin` to make `new WebSocket` succeed. A wrong CORS setup on your REST API does not open or close the socket.
- **The check you do have**: Read `Origin` during the upgrade. That is the control MDN documents for browser clients.
- **HTTP beside the socket**: The page’s `fetch` calls still need CORS if they are cross-origin. That is a separate response from the `101`.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>How to handle cross-origin WebSocket connections in Node.js?</summary>

### How to handle cross-origin WebSocket connections in Node.js?

**Definition:**

In the `upgrade` listener, read `request.headers.origin` before `handleUpgrade`. Destroy the socket when the origin is not allowed.

**How it works:**

- **Header**: Node gives you the raw HTTP request. `origin` is a normal header on that request, the same one MDN tells you to check.
- **Reject**: Follow the `ws` authentication example: write an HTTP error and `socket.destroy()` instead of calling `handleUpgrade`.
- **Then authenticate**: An allowed origin is not a user. Still validate the session or token on that same request.

**Examples:**

```js
const allowed = new Set(["https://app.example"]);
server.on("upgrade", (request, socket, head) => {
  if (!allowed.has(request.headers.origin)) {
    socket.write("HTTP/1.1 403 Forbidden\r\n\r\n");
    socket.destroy();
    return;
  }
  wss.handleUpgrade(request, socket, head, (ws) => {
    wss.emit("connection", ws, request);
  });
});
```

**Sources:**

- [ws — handleUpgrade](https://github.com/websockets/ws)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>


### WebSockets and Full-Duplex Communication

<details>
<summary>What does full-duplex communication mean in the context of WebSockets?</summary>

### What does full-duplex communication mean in the context of WebSockets?

**Definition:**

Full duplex means either side may send a frame at any time on the same connection, without waiting for the other side to ask. MDN describes that as the reason to leave HTTP after the handshake.

**How it works:**

- **After 101**: The client does not send a new HTTP request for each message. Server frames have the MASK bit clear. Client frames are masked.
- **Independent**: A server ping (opcode `0x9`) can be written while the client is still sending a text message. Control frames may sit between fragments.
- **Still one TCP pipe**: Bytes remain ordered. Full duplex does not mean a slow send stops blocking later bytes on that connection.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>

<details>
<summary>Can you provide an example where full-duplex WebSocket communication is essential?</summary>

### Can you provide an example where full-duplex WebSocket communication is essential?

**Definition:**

A voice-call signaling session and a multiplayer position stream both need the server to push while the client is also sending. Request-response would make the server wait for the next poll.

**How it works:**

- **Signaling**: MDN’s WebRTC guide uses a two-way channel so either peer can send an ICE candidate when it learns one. A WebSocket is one such channel.
- **Chat**: The other person types while you do. The server writes a frame without a fresh `GET` from you.
- **Not required**: A dashboard that only receives ticks can be server-sent events. Full duplex matters when the client also sends often on the same pipe.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>How does full-duplex communication affect the design of client and server logic?</summary>

### How does full-duplex communication affect the design of client and server logic?

**Definition:**

Each side reads and writes at the same time, so you cannot structure the code as one request then one response. You need a read loop and a separate way to send.

**How it works:**

- **Client**: Register `message` once. Call `send` from UI events. Do not assume the next `message` is the answer to the last `send` unless you put an id in the payload.
- **Server**: The Python tutorial loops on `recv`. Outbound chat is `send` from another task. Those two must be safe together.
- **Backpressure**: A read loop that never pauses, plus a `send` that ignores `bufferedAmount`, will queue without bound on a slow peer.

**Sources:**

- [websockets tutorial](https://websockets.readthedocs.io/en/stable/intro/tutorial1.html)
- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>


### WebSockets and State Management

<details>
<summary>How should the state be managed over WebSocket connections?</summary>

### How should the state be managed over WebSocket connections?

**Definition:**

Keep durable state in your store or database. Keep only the connection’s identity on the socket: user id, rooms, and the last message id you have sent.

**How it works:**

- **On connect**: Bind the user during the upgrade. MDN’s client table is this binding.
- **In messages**: Send facts (“message 42 was created”), not “replace the whole world” on every keystroke, unless the document is small.
- **On disconnect**: Drop the socket row. Do not delete the user’s documents because the TCP connection closed.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>

<details>
<summary>Discuss the implications of stateful vs stateless WebSocket servers.</summary>

### Discuss the implications of stateful vs stateless WebSocket servers.

**Definition:**

The TCP connection is always state. A “stateless” design means the process does not keep business data that exists only in memory: it can rebuild from a store after reconnect.

**How it works:**

- **Must be in memory**: The socket itself, and which rooms it joined, live on that process. Another process cannot write to it.
- **Should be outside**: Messages, presence that must survive a restart, and auth sessions belong in a store. Otherwise a deploy wipes the chat.
- **Reconnect**: The new socket is a new connection. It loads state by id. It does not inherit RAM from the old process.

**When to use:**

- Keep the socket map in the process.
- Keep anything a user would miss after a restart in a store both instances can read.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws](https://github.com/websockets/ws)
</details>

<details>
<summary>How can you synchronize the state between the client and server over WebSockets?</summary>

### How can you synchronize the state between the client and server over WebSockets?

**Definition:**

Give every change an id or a version. The client sends the last version it applied; the server sends the missing changes or a full snapshot.

**How it works:**

- **Open**: On `open`, send `{ "last": 17 }`. Do this on every reconnect, not only the first visit.
- **Gap**: If the server still has events 18 onward, it sends them in order. If not, it sends a snapshot from HTTP or in one binary or text message.
- **Live tail**: After the gap, ordinary frames continue. The client ignores a version it already applied so a replay is safe.

**Sources:**

- [MDN — message event](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
</details>


### WebSockets and UI/UX Concerns

<details>
<summary>How can WebSockets improve user experience in web applications?</summary>

### How can WebSockets improve user experience in web applications?

**Definition:**

The server can update the page as soon as it knows, without the user refreshing or the page polling. The handshake is paid once.

**How it works:**

- **Push**: Chat lines, scores, and ticket status arrive as `message` events and the UI renders `event.data`.
- **Less waiting**: A poll that runs every few seconds is late by up to that interval. A frame arrives when it is sent, plus the network delay.
- **Show the connection**: Use `open`, `close`, and `error` so the screen says when it is live or retrying, instead of looking frozen.

**Sources:**

- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [MDN — message event](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>What are some potential UI/UX pitfalls when using WebSockets for real-time updates?</summary>

### What are some potential UI/UX pitfalls when using WebSockets for real-time updates?

**Definition:**

A live socket can flood the screen, hide failures, or apply updates out of the user’s current edit. The protocol will not order your product decisions.

**How it works:**

- **Silence**: `error` carries little detail. If you ignore `close`, the UI still shows old data and looks connected. Surface `code` `1006` as disconnected.
- **Flood**: Appending every tick without a limit makes a list no one can read. Coalesce when `bufferedAmount` is high and when the eye cannot follow.
- **Clobber**: A snapshot pushed while someone is typing in a field will wipe the field if you replace the whole view. Patch the parts they are not editing.

**Sources:**

- [MDN — WebSocket.close()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/close)
- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>How do you manage user notifications with WebSockets?</summary>

### How do you manage user notifications with WebSockets?

**Definition:**

While the page is open, a notification is a frame on the user’s socket. When the page is closed, that socket is gone and you need another channel.

**How it works:**

- **Address**: Keep user id → sockets. One user may have several tabs, so send to each `OPEN` socket, using the same loop as the `ws` broadcast example.
- **Payload**: Send a small JSON object the page can show. Do not send HTML.
- **Background**: Socket.IO’s docs say a held TCP connection is the wrong tool for mobile background alerts and point to a push service. Use the socket for the open app.

**Sources:**

- [ws — broadcast](https://github.com/websockets/ws)
- [Socket.IO](https://socket.io/docs/v4/)
</details>


### WebSockets Monitoring and Maintenance

<details>
<summary>What are some effective strategies for monitoring WebSocket traffic?</summary>

### What are some effective strategies for monitoring WebSocket traffic?

**Definition:**

Count openings, closes by status code, and messages. Watch `bufferedAmount` and failed pongs so a half-open connection is visible.

**How it works:**

- **Handshake**: Log `101` versus `400`, `401`, and `403`. Those are still HTTP, which MDN says is the only place for those statuses.
- **Close codes**: Chart `1000`, `1001`, `1006`, `1008`, and `1009`. A spike of `1006` is dropped TCP, not an application error you parsed.
- **Ping**: The `ws` FAQ marks a client dead when `pong` does not arrive before the next ping, then `terminate()`s it. Alert on how often that happens.

**Sources:**

- [ws — broken connections](https://github.com/websockets/ws)
- [RFC 6455 — status codes](https://www.rfc-editor.org/rfc/rfc6455#section-7.4)
- [MDN — bufferedAmount](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>How do you maintain and update WebSocket-based systems with minimal downtime?</summary>

### How do you maintain and update WebSocket-based systems with minimal downtime?

**Definition:**

A process restart closes its sockets. Minimize downtime by draining, letting clients reconnect to another instance, and keeping the data they resync from outside that process.

**How it works:**

- **Drain**: Stop accepting new upgrades. Send close code `1001` (going away) so clients know the server is leaving, which is the meaning RFC 6455 gives that code.
- **Overlap**: Bring up the new process before the old one exits. Sticky connections stay on the old process until they reconnect.
- **Resync**: On `open`, the client sends the last version. The new process answers from the store, not from the old process’s RAM.

**Sources:**

- [RFC 6455 — status codes](https://www.rfc-editor.org/rfc/rfc6455#section-7.4)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
</details>


### WebSockets Advanced Security Topics

<details>
<summary>Discuss the use of client certificates with WebSockets for enhanced security.</summary>

### Discuss the use of client certificates with WebSockets for enhanced security.

**Definition:**

Client certificates are a TLS feature of `wss`, checked during the handshake before WebSocket frames exist. They identify the TLS peer; they do not replace application messages.

**How it works:**

- **When**: The browser or device presents a certificate while negotiating TLS for `wss`. If that fails, the connection never reaches `101`. Close code `1015` is reserved for “TLS handshake failed” and must not be sent in a Close frame.
- **Server**: The `ws` HTTPS example loads `cert` and `key` for the server certificate. Client-certificate checks are part of that TLS listener, then your `upgrade` handler still checks the user.
- **Limit**: A certificate is not a per-message signature. After the handshake, use the bound identity and ordinary frames.

**Sources:**

- [RFC 6455 — 1015](https://www.rfc-editor.org/rfc/rfc6455#section-7.4)
- [ws — HTTPS](https://github.com/websockets/ws)
</details>

<details>
<summary>Explain how to implement rate limiting for WebSocket connections to prevent abuse.</summary>

### Explain how to implement rate limiting for WebSocket connections to prevent abuse.

**Definition:**

Limit new handshakes per client and messages per open socket. MDN says a server may deny clients that connect too often. Oversized messages use close code `1009`.

**How it works:**

- **Connections**: Count upgrades per IP or per user. Past the limit, answer with an HTTP error and destroy the socket before `handleUpgrade`.
- **Messages**: Count frames per socket per interval. When the count trips, `close` with a private code in `4000`–`4999` or with `1008` if it violated policy.
- **Size**: RFC 6455 defines `1009` for a message too big to process. Check the length before you buffer an unbounded fragment sequence.

**Sources:**

- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [RFC 6455 — status codes](https://www.rfc-editor.org/rfc/rfc6455#section-7.4)
- [ws](https://github.com/websockets/ws)
</details>

<details>
<summary>What is the role of security tokens in WebSocket communication, and how do you implement token-based authentication?</summary>

### What is the role of security tokens in WebSocket communication, and how do you implement token-based authentication?

**Definition:**

A token proves who opened the socket. Check it on the HTTP upgrade or on the first message, then remember the user on that connection. Frames after that do not carry the token unless you design them to.

**How it works:**

- **Browser limit**: `new WebSocket` cannot set `Authorization`. Use a session cookie, a ticket query parameter, or the first text frame.
- **Upgrade**: The `ws` example calls `authenticate(request)` and writes `401` on failure before `handleUpgrade`.
- **Expiry**: When the token expires, close the socket. The client gets a new token over HTTP and connects again.

**Sources:**

- [ws — client authentication](https://github.com/websockets/ws)
- [MDN — WebSocket()](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/WebSocket)
</details>


### WebSockets Compliance and Standards

<details>
<summary>What are the RFCs (Request for Comments) that define the WebSocket protocol?</summary>

### What are the RFCs (Request for Comments) that define the WebSocket protocol?

**Definition:**

RFC 6455 is the WebSocket protocol: the handshake, framing, masking, and close codes. Later RFCs add extensions and other transports. The handshake GUID in RFC 6455 is `258EAFA5-E914-47DA-95CA-C5AB0DC85B11`.

**How it works:**

- **RFC 6455**: MDN tells server authors to read it, especially sections 1 and 4–7, and section 10 for security. Version 13 is the `Sec-WebSocket-Version` browsers send.
- **Compression**: The `ws` readme negotiates the permessage-deflate extension from RFC 7692. It is optional and separate from the base framing.
- **HTTP/2**: ASP.NET documents WebSockets over HTTP/2 as RFC 8441. Those handshakes use `CONNECT` rather than the original `GET` upgrade.

**Sources:**

- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws — permessage-deflate](https://github.com/websockets/ws)
- [ASP.NET Core WebSockets](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/websockets)
</details>

<details>
<summary>How does compliance with WebSocket standards affect interoperability and future-proofing your application?</summary>

### How does compliance with WebSocket standards affect interoperability and future-proofing your application?

**Definition:**

If you implement the handshake, masking, and close codes from RFC 6455, a browser `WebSocket` and another compliant server can talk. A private framing scheme cannot.

**How it works:**

- **Interoperability**: Autobahn, which `ws` passes, catches illegal UTF-8, bad lengths, and fragmentation mistakes that a single browser might forgive.
- **Extensions**: Negotiate `permessage-deflate` with the header. Do not invent RSV-bit meanings the peer does not know. Unknown opcodes `0x3`–`0x7` and `0xB`–`0xF` have no meaning in this version.
- **Your version**: Put application versions in a subprotocol name. Leave `Sec-WebSocket-Version` at 13 so future browsers still complete the handshake.

**Sources:**

- [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455)
- [MDN — Writing WebSocket servers](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)
- [ws](https://github.com/websockets/ws)
</details>
