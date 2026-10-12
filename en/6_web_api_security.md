# Web APIs — Security

Browser security and third-party integrations.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Performance](./6_web_api_performance.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [13. Security](#13-security)
- [15. Integrations](#15-integrations)

---

## 13. Security

<details>
  <summary>CORS</summary>

### CORS

**Definition:** CORS (Cross-Origin Resource Sharing) is a **browser** mechanism that lets a server opt in so page JavaScript can **read** a cross-origin HTTP response. Same-Origin Policy is the default deny; CORS is the explicit allow list.

**How it works:**

- **Origin** = scheme + host + port. `https://doka.guide` and `http://doka.guide` are different origins (protocol). Port differences also count.
- Same-origin `fetch` needs no CORS headers. Cross-origin (`app.com` → `api.example.com`) is a **cross-origin request**.
- The browser always sends `Origin`. You do not set CORS request headers in JS — `Origin`, `Access-Control-Request-Method`, and `Access-Control-Request-Headers` are added by the browser.
- **Simple requests** (safelisted method + headers, typical form `Content-Type`) go out immediately. If the response lacks a matching `Access-Control-Allow-Origin`, the **request still hit the server**; JS cannot read the body. CORS errors have no useful message in JS — use DevTools.
- **Preflight:** non-simple method (`PUT`, `DELETE`, …) or non-safelisted headers (`Authorization`, `Content-Type: application/json`, custom headers) → browser sends `OPTIONS` first. Cached via `Access-Control-Max-Age` (browser also has an internal cap; MDN default if omitted is 5s).
- `Access-Control-Allow-Origin` may be **one** origin or `*` (no credentials). Multiple sites: reflect the request `Origin` if it is on an allowlist, and send `Vary: Origin`.
- Preflight must **not** include credentials. Credentialed actual requests need `Access-Control-Allow-Credentials: true` and a **concrete** origin — never `*` (also no `*` on Allow-Methods / Allow-Headers / Expose-Headers with credentials).

| Response header                    | Role                                                                       |
| ---------------------------------- | -------------------------------------------------------------------------- |
| `Access-Control-Allow-Origin`      | Which origin may read the response (`*` or one origin)                     |
| `Access-Control-Allow-Methods`     | Methods allowed on the **actual** request (preflight)                      |
| `Access-Control-Allow-Headers`     | Request headers the client may send (preflight) — **not** response headers |
| `Access-Control-Allow-Credentials` | `true` if cookies / HTTP auth may be used                                  |
| `Access-Control-Max-Age`           | Seconds to cache the preflight result                                      |
| `Access-Control-Expose-Headers`    | Response headers JS may read (`Response.headers`)                          |

| Request header (browser-set)     | Role                                         |
| -------------------------------- | -------------------------------------------- |
| `Origin`                         | Page origin                                  |
| `Access-Control-Request-Method`  | Method of the planned actual request         |
| `Access-Control-Request-Headers` | Non-safelisted headers of the actual request |

**When to use:**

SPA on `https://app.example.com` calling `https://api.example.com`; public unauthenticated API with `*`; cookie/session API with an origin allowlist + credentials.

#### Common pitfalls

Treating CORS as a firewall (the simple request already ran). `Allow-Headers` vs `Expose-Headers`. `*` + cookies. Assuming `*` “causes CSRF” — form CSRF does not need CORS; CORS does not stop cookie-based CSRF. See [CSRF](./7_special_security.md#csrf-cross-site-request-forgery).

**Pluses and minuses:**

| +                                                                | −                                                                 |
| ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| Lets SPAs call a separate API origin with server consent         | Easy to misconfigure (`*`, missing preflight, wrong header names) |
| Preflight can reject unsafe methods/headers before the real call | Browser-only — curl/Postman/server-to-server ignore CORS          |
| Standard `Access-Control-*` headers                              | Does **not** replace CSRF protection                              |

**Examples:**

```http
OPTIONS / HTTP/1.1
Host: api.example.com
Origin: https://doka.guide
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: content-type, authorization
```

```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://doka.guide
Access-Control-Allow-Methods: GET, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
Vary: Origin
```

```js
await fetch("https://api.example.com/data", {
  credentials: "include", // needs Allow-Credentials: true and a concrete Allow-Origin
});
```

**Thoughts:** The server still receives many cross-origin requests; CORS only decides whether **page JS** may read the response.

**Sources:**

[javascript.info/fetch-crossorigin](https://javascript.info/fetch-crossorigin) · [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) · [Doka — CORS](https://doka.guide/tools/cors/)

---

</details>

<details>
  <summary>Content Security Policy (CSP)</summary>

### Content Security Policy (CSP)

**Definition:** CSP is an HTTP header that lists trusted sources for scripts, styles, images, and other content — a backup defense if XSS encoding fails.

**How it works:**

Browser blocks loads and inline script that are not in the policy. Strict CSP uses **nonces or hashes** for allowed scripts and disables `eval` / inline handlers. `default-src` fills in missing fetch directives.

| Directive         | Controls                                                                    |
| ----------------- | --------------------------------------------------------------------------- |
| `script-src`      | JavaScript sources                                                          |
| `style-src`       | CSS sources                                                                 |
| `img-src`         | Images (also used to block tracking pixels)                                 |
| `connect-src`     | `fetch` / XHR / WebSocket                                                   |
| `default-src`     | Fallback for unspecified fetch types                                        |
| `frame-ancestors` | Who may embed this page (clickjacking) — **HTTP header only**, not `<meta>` |

**Examples:**

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://cdn.example.com; frame-ancestors 'self'
```

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'" />
```

**Thoughts:** `'unsafe-inline'` largely defeats XSS protection. `<meta>` CSP cannot set `frame-ancestors` or some other document directives. See [HTML — CSP](./1_html.md) · [Special — CSP](./7_special_security.md#content-security-policy-csp) · [Special — XSS](./7_special_security.md#xss-cross-site-scripting).

**Sources:**

[MDN — CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)

---

</details>

<details>
  <summary>Cross-Site Scripting (XSS)</summary>

### Cross-Site Scripting (XSS)

**Definition:** XSS injects script that runs in **your origin** in another user's browser — session theft, actions as the user, or keylogging.

**How it works:**

Untrusted data is parsed as HTML/JS. **Reflected** — query/body echoed in that response. **Stored** — saved and replayed for later viewers (including XSS keyloggers). **DOM** — client JS writes untrusted strings into the DOM.

| Type      | Vector                                  |
| --------- | --------------------------------------- |
| Stored    | Malicious data saved in DB              |
| Reflected | URL/query echoed in response            |
| DOM-based | Client JS writes untrusted input to DOM |

**Examples:**

```js
element.innerHTML = userInput; // parses tags
element.textContent = userInput; // text only
```

Encode for the **output context** (HTML vs URL). `encodeURIComponent` is for URLs, not HTML. CSP + HttpOnly cookies are extra layers. Full catalog: [Special — XSS](./7_special_security.md#xss-cross-site-scripting).

**Sources:**

[OWASP — XSS](https://owasp.org/www-community/attacks/xss/)

---

</details>

<details>
  <summary>SSRF (Server-Side Request Forgery)</summary>

### SSRF (Server-Side Request Forgery)

**Definition:** **SSRF** tricks **your server** into sending HTTP (or other protocol) requests to a URL the attacker chose — often internal services (`localhost`, `169.254.169.254`, Redis, admin panels) that the browser cannot reach directly.

**How it works:**

1. App accepts a URL or host from user input — webhook callback, PDF fetch, image proxy, import-from-URL, GraphQL `@deprecated` resolvers.
2. Attacker passes `http://169.254.169.254/latest/meta-data/` (cloud metadata), `http://127.0.0.1:6379/`, or an internal admin API.
3. Server performs the request **with its network privileges** — may leak secrets, pivot inside the VPC, or hit unauthenticated internal APIs.

| Vector            | Example                                                             |
| ----------------- | ------------------------------------------------------------------- |
| User-supplied URL | `POST /api/preview { "url": "http://internal:8080/admin" }`         |
| Redirect follow   | Public URL 302 → internal target if redirects are unlimited         |
| DNS rebinding     | Host resolves to public IP at check time, internal IP at fetch time |

**When to use defenses:** Any server-side `fetch`, `axios`, or HTTP client where the target is not 100% fixed at deploy time — especially BFFs, webhooks, crawlers, and file converters.

#### Common pitfalls

- Blocklisting only `127.0.0.1` — attackers use `0.0.0.0`, `[::1]`, decimal IPs, or DNS names that resolve internally.
- Trusting “we only call our API” — open redirects and SSRF chains still happen.
- Assuming SSRF is a browser problem — **no CORS** on server-to-server calls.

**Pluses and minuses:**

| + (allowlist + no redirects)             | −                                                     |
| ---------------------------------------- | ----------------------------------------------------- |
| Stops metadata and internal scans        | Strict allowlists need maintenance when backends move |
| Network segmentation limits blast radius | Over-blocking breaks legitimate webhook integrations  |

**Examples:**

```ts
const ALLOWED_HOSTS = new Set(["api.example.com", "cdn.example.com"]);

function assertSafeUrl(raw: string) {
  const url = new URL(raw);
  if (url.protocol !== "https:") throw new Error("HTTPS only");
  if (!ALLOWED_HOSTS.has(url.hostname)) throw new Error("Host not allowed");
  if (isPrivateIp(url.hostname)) throw new Error("Private network blocked");
  return url;
}

// fetch only after validation; disable redirect following or re-validate each hop
await fetch(assertSafeUrl(userUrl), { redirect: "manual" });
```

**Thoughts:** _Default deny — fixed backend base URLs, not arbitrary user URLs. SSRF is why BFFs must validate before fan-out._ See [Secure BFF patterns](#secure-bff-patterns) · [BFF architecture](./7_special_architecture.md#bff-backend-for-frontend).

**Sources:**

[OWASP — SSRF](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery) · [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)

---

</details>

<details>
  <summary>CSRF (Cross-Site Request Forgery)</summary>

### CSRF (Cross-Site Request Forgery)

**Definition:** CSRF tricks a logged-in user's browser into sending an authenticated request to a site where they already have a session — the server trusts the cookie and performs the action.

**How it works:**

1. User is logged into `bank.com` (session cookie set).
2. User visits `evil.com` which contains `<form action="https://bank.com/transfer" method="POST">` or `<img src="https://bank.com/transfer?to=attacker&amount=1000">`.
3. Browser automatically attaches `bank.com` cookies → server treats request as legitimate.

| Defense                       | How                                                                                      |
| ----------------------------- | ---------------------------------------------------------------------------------------- |
| **CSRF token**                | Server embeds unpredictable token in form/meta; validate on POST                         |
| **SameSite cookies**          | `SameSite=Lax` or `Strict` blocks cross-site cookie on POST                              |
| **Custom headers**            | Require `X-Requested-With` or `Content-Type: application/json` — simple forms cannot set |
| **Origin/Referer check**      | Reject if header missing or wrong                                                        |
| **Re-auth for sensitive ops** | Password/2FA for transfers                                                               |

**When to use:**

- Cookie-based session APIs — CSRF token or `SameSite=Strict` for mutations.
- JWT in `Authorization` header only — **not** vulnerable to classic CSRF (browser does not auto-send custom headers).
- `GET` must never change state — CSRF often exploits state-changing GET.

**Pluses and minuses:**

| + (SameSite + token)            | −                                                           |
| ------------------------------- | ----------------------------------------------------------- |
| Blocks most browser-driven CSRF | SameSite=None needed for some OAuth flows (requires Secure) |
| Token pattern well-understood   | SPA + cookie auth needs careful CORS/credentials setup      |

**Examples:**

```html
<meta name="csrf-token" content="random-token-from-server" />
```

```js
fetch("/api/transfer", {
  method: "POST",
  credentials: "include",
  headers: {
    "Content-Type": "application/json",
    "X-CSRF-Token": document.querySelector("meta[name=csrf-token]").content,
  },
  body: JSON.stringify({ to: "alice", amount: 50 }),
});
```

**Thoughts:** CORS does **not** protect against CSRF — CSRF is a cookie auto-send problem. `SameSite=Lax` is a strong default for session cookies. Authentication (“session is valid”) is not the same as authorizing **this** mutation — use a CSRF token. Cookie `HttpOnly` does not stop CSRF. See [Special — CSRF](./7_special_security.md#csrf-cross-site-request-forgery).

**Sources:**

[OWASP — CSRF](https://owasp.org/www-community/attacks/csrf) · [MDN — SameSite cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#samesitesamesite-value)

---

</details>

<details>
  <summary>Clickjacking</summary>

### Clickjacking

**Definition:** Attacker overlays a transparent iframe of your site so the user clicks **your** UI while looking at a decoy.

**How it works:**

Authenticated cookies apply inside the iframe. No XSS required.

| Defense                                  | Notes                                                        |
| ---------------------------------------- | ------------------------------------------------------------ |
| CSP `frame-ancestors 'none'` or `'self'` | Preferred; list extra hosts if you must embed                |
| `X-Frame-Options: DENY` or `SAMEORIGIN`  | Still widely sent; `ALLOW-FROM` is obsolete (header ignored) |

```http
X-Frame-Options: SAMEORIGIN
Content-Security-Policy: frame-ancestors 'self'
```

**Thoughts:** Header-only for XFO (not `<meta>`). SameSite does not replace framing policy. See [Special — Clickjacking](./7_special_security.md#clickjacking).

**Sources:**

[MDN — X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)

---

</details>

<details>
  <summary>JSONP</summary>

### JSONP

**Definition:**

JSONP bypasses SOP by loading cross-origin data via a `<script>` tag that calls a callback function — legacy technique, largely replaced by CORS.

| JSONP                      | CORS                         |
| -------------------------- | ---------------------------- |
| GET only                   | All methods (with preflight) |
| No standard error handling | Standard HTTP semantics      |
| Trust-based                | Header-based permission      |

**Examples:**

```html
<script src="https://api.example.com/data?callback=handleData"></script>
<script>
  function handleData(json) {
    console.log(json);
  }
</script>
```

**Sources:**

[MDN — CORS (prefer over JSONP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

---

</details>

<details>
  <summary>Secure BFF patterns</summary>

### Secure BFF patterns

**Definition:** A **secure BFF** (Backend for Frontend) keeps **secrets, refresh tokens, and upstream credentials on the server** while exposing a narrow, UI-shaped HTTP API to the browser — defense in depth for SPAs and Next.js Route Handlers.

**How it works:**

| Concern                   | Secure pattern                                                                                   |
| ------------------------- | ------------------------------------------------------------------------------------------------ |
| API keys / client secrets | Only in BFF env vars — never `NEXT_PUBLIC_*` or bundled JS                                       |
| Access token              | Short TTL; browser gets session cookie or memory-held access token                               |
| Refresh token             | **HttpOnly cookie** or server-side store; refresh endpoint on BFF only                           |
| Upstream calls            | BFF attaches `Authorization` server-side after session check                                     |
| CSRF                      | `SameSite` session cookie + CSRF token / double-submit for cookie-auth mutations                 |
| SSRF                      | Validate outbound URLs; allowlist upstream hosts — see [SSRF](#ssrf-server-side-request-forgery) |
| AuthZ                     | BFF checks session **and** object-level permission before fan-out                                |

Typical Next.js shape: browser → `POST /api/bff/checkout` (cookie) → Server Action / Route Handler validates session → parallel `fetch` to domain services with service token → return DTO. The browser never sees microservice URLs or service JWTs.

**When to use:** SPA or App Router app talking to multiple backends; OAuth with refresh; any time the alternative is storing tokens in `localStorage`. Architecture overview: [BFF](./7_special_architecture.md#bff-backend-for-frontend). Refresh flow: [Access token refresh](./6_web_api_auth.md#access-token-refresh).

#### Common pitfalls

- Passing refresh tokens to Client Components via props or Flight payload.
- BFF as sole auth gate with no validation in domain services (defense in depth).
- Fan-out to `fetch(req.body.url)` — classic SSRF + credential leak.
- Skipping CSRF because “we use JSON” while session rides on cookies.

**Pluses and minuses:**

| +                                         | −                                                 |
| ----------------------------------------- | ------------------------------------------------- |
| Secrets never in the browser bundle       | Extra hop and BFF SPOF                            |
| Central place for rate limits and logging | Team must own BFF security reviews                |
| Hides microservice topology from clients  | Can become a god-service if domain logic leaks in |

**Examples:**

```ts
// app/api/session/refresh/route.ts — refresh stays server-side
import { cookies } from "next/headers";

export async function POST() {
  const refresh = (await cookies()).get("refresh_token")?.value;
  if (!refresh)
    return Response.json({ error: "unauthorized" }, { status: 401 });

  const tokens = await exchangeRefreshToken(refresh); // server-only client_secret
  await setSessionCookies(tokens); // new HttpOnly access + rotated refresh
  return Response.json({ ok: true });
}
```

**Thoughts:** _BFF security = least privilege to the browser, full privilege only on the server, validate every inbound mutation._

**See also:** [BFF (Backend for Frontend)](./7_special_architecture.md#bff-backend-for-frontend) · [Next.js — Auth on platform](./4_nextjs_platform.md) · [Access token refresh](./6_web_api_auth.md#access-token-refresh) · [CSRF](#csrf-cross-site-request-forgery)

**Sources:**

[OAuth.net — Refresh Token](https://oauth.net/2/grant-types/refresh-token/) · [Azure — Backends for Frontends](https://learn.microsoft.com/azure/architecture/patterns/backends-for-frontends) · [OWASP — Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

---

</details>

<a name="15-integrations"></a>

## 15. Integrations

<details>
  <summary>Payments, analytics & third-party SDKs</summary>

### Payments, analytics & third-party SDKs

**Definition:** External providers you integrate over HTTPS + a client SDK: **payments** (Stripe/PayPal), **analytics** (GA4, Meta Pixel, product analytics), **maps/auth/CMS**. Your app owns the UX; the vendor owns PCI, tracking pixels, or identity.

**How it works:**

- **Payments:** never store raw card numbers. Stripe Checkout / Payment Element tokenize on their origin; your server confirms with a secret key (`checkout.session.completed` webhook). Idempotency keys on create-payment.
- **Analytics:** load the vendor script `async`; send events after consent (GDPR). Prefer server-side events for revenue so ad-blockers do not drop conversions.
- **SDKs:** wrap in an adapter (`lib/payments.ts`) so swapping vendors does not leak into UI. Lazy-load heavy SDKs (`next/dynamic`) so they do not hurt INP/LCP.

**When to use:**

SaaS checkout, marketing funnels, “Sign in with Google.” Skip loading analytics on the first paint of a landing hero if LCP is the goal.

#### Common pitfalls

Putting secret keys in the browser. Trusting client-reported `amount`. Blocking render on `gtag.js`. Skipping webhook signature checks.

**Pluses and minuses:**

| +                                             | −                                           |
| --------------------------------------------- | ------------------------------------------- |
| PCI / tracking compliance is the vendor's job | Script size, privacy, and CSP `script-src`  |
| Webhooks keep your DB in sync                 | Webhook signature verification is mandatory |
| Faster than building billing yourself         | Vendor lock-in and sandbox vs live keys     |

**Examples:**

```ts
// Server — confirm after webhook, never trust the client amount alone
export async function POST(req: Request) {
  const sig = req.headers.get("stripe-signature");
  const event = stripe.webhooks.constructEvent(
    await req.text(),
    sig!,
    process.env.STRIPE_WEBHOOK_SECRET!,
  );
  if (event.type === "checkout.session.completed") {
    await markPaid(event.data.object.id);
  }
  return new Response(null, { status: 200 });
}
```

**Thoughts:** Client SDK for UX; server + signed webhooks for truth.

**Sources:**

[Stripe webhooks](https://docs.stripe.com/webhooks) · [web.dev third-party](https://web.dev/articles/optimizing-content-efficiency-loading-third-party-javascript)

---

</details>

<a name="interview-practice"></a>
