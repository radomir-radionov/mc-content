# Web APIs — Auth & storage

AuthN/AuthZ, sessions, JWT, OAuth, cookies, and storage.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Core](./6_web_api_core.md) · [Web APIs — HTTP](./6_web_api_http.md) · [Web APIs — API styles](./6_web_api_rest.md) · [Web APIs — Auth & storage](./6_web_api_auth.md) · [Web APIs — Apps](./6_web_api_apps.md) · [Web APIs — Security](./6_web_api_security.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [8. Auth](#8-auth)
- [9. Storage](#9-storage)

---

## 8. Auth

<details>
  <summary>Identification, authentication, authorization & HTTP Basic</summary>

### Identification, authentication, authorization & HTTP Basic

**Definition:** **Identification** assigns a name (username, `sub`). **Authentication** proves that claim. **Authorization** grants rights after a successful proof.

**How it works:**

- Ticket analogy: name on the ticket = identification; passport check = authentication; boarding pass / cabin class = authorization.
- HTTP **Basic**: server may send `401` + `WWW-Authenticate: Basic realm="…"`; client retries with `Authorization: Basic <base64(user:pass)>`. Base64 is **not** encryption — only use over HTTPS. Invalid creds → `401`; valid but not allowed → `403`.
- App auth: credentials → session cookie or token in later `Authorization` headers. “Sign in with Google” authenticates at the IdP; your server gets a token, then issues its own session ([OAuth2 & OIDC](#oauth2--oidc-overview), [Sessions & JWT](#sessions--jwt)).

**When to use:**

Basic for staging dashboards behind TLS. Sessions/JWT/OAuth for real products.

#### Common pitfalls

Calling Basic “encrypted.” Mixing Basic with session tokens as if they were the same scheme.

**Pluses and minuses:**

| +                                    | −                                                             |
| ------------------------------------ | ------------------------------------------------------------- |
| Basic is simple and widely supported | Credentials on every request; CSRF-prone vs cookie `SameSite` |
| Clear 401 vs 403                     | Password in Base64 is reversible                              |

**Examples:**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Basic realm="staging"

GET /admin HTTP/1.1
Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
```

(`aladdin:opensesame` in Base64 — MDN’s example, not a secret.)

**Thoughts:** _Identify → prove (authn) → grant (authz). Basic is `user:pass` in a header, not a session._

**Sources:**

[MDN — HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication) · Node: [identification vs auth](./8_nodejs_http.md#authentication-identification--authorization)

---

</details>

<details>
  <summary>OAuth2 & OIDC overview</summary>

### OAuth2 & OIDC overview

**Definition:** **OAuth 2.0** is an authorization framework — delegated access without sharing passwords. **OpenID Connect (OIDC)** is an identity layer on top of OAuth2 that adds authenticated user info (`id_token`).

**How it works:**

1. App redirects user to **Authorization Server** (`/authorize?client_id=…&redirect_uri=…&scope=openid profile&code_challenge=…`).
2. User authenticates and consents.
3. Server redirects back with **authorization code** (short-lived, one-time).
4. App exchanges code at **token endpoint** (`POST /token`) with `client_secret` or **PKCE** `code_verifier` — never expose secret in browser-only apps.
5. Response: **access_token** (call APIs), **refresh_token** (optional), **id_token** (OIDC — JWT with `sub`, `email`, `iss`).

| Token           | Purpose                                       |
| --------------- | --------------------------------------------- |
| Access token    | Bearer credential for resource server APIs    |
| Refresh token   | Obtain new access token without re-login      |
| ID token (OIDC) | Proof of authentication; JWT signed by issuer |

| Concept      | OAuth2                            | OIDC                             |
| ------------ | --------------------------------- | -------------------------------- |
| Primary goal | Authorization (scopes)            | Authentication (who is the user) |
| User info    | Via protected userinfo endpoint   | `id_token` claims + userinfo     |
| Typical use  | "App can read my Google Calendar" | "Sign in with Google"            |

**When to use:**

- **Sign in with Google/GitHub** — OIDC Authorization Code + PKCE.
- **Machine-to-machine** — Client Credentials flow (no user).
- **Mobile/native** — PKCE mandatory; use system browser, not embedded WebView.

**Pluses and minuses:**

| +                                           | −                                            |
| ------------------------------------------- | -------------------------------------------- |
| No password sharing with third parties      | Complex; easy to misconfigure                |
| Standard flows, libraries (Passport, Auth0) | Token theft if stored in localStorage        |
| Fine-grained scopes                         | Refresh token rotation required for security |

**Examples:**

```json
{
  "access_token": "eyJhbG…",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "def502…",
  "id_token": "eyJhbG…"
}
```

**Thoughts:** OAuth2 is **not** authentication by itself — use OIDC for login. Store tokens in **HttpOnly cookies** or secure storage; validate `iss`, `aud`, `exp` on JWTs. Never put access tokens in URLs.

**Sources:**

[OAuth 2.0 RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) · [OpenID Connect](https://openid.net/connect/) · [OAuth 2.0 for Browser-Based Apps (PKCE)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-browser-based-apps)

---

</details>

<details>
  <summary>Sessions & JWT</summary>

### Sessions & JWT

**Definition:** **Sessions** store server-side state; client holds only a session ID cookie. **JWT** encodes claims in a signed token — verification is stateless if you trust the signature and expiry.

**How it works:**

- **Session** — login → server creates session record (Redis/DB) → `Set-Cookie: sid=…; HttpOnly; Secure; SameSite=Lax` → each request sends cookie → server looks up session.
- **JWT** — login → server signs payload → client sends `Authorization: Bearer <token>` → server verifies HMAC/RSA signature + `exp` without DB lookup (unless blocklist needed).

**When to use:** Session for traditional web apps with server-rendered pages. JWT for microservices and mobile APIs. Hybrid: session for web + JWT for API.

**Pluses and minuses:**

| Approach       | +                               | −                                           |
| -------------- | ------------------------------- | ------------------------------------------- |
| Session cookie | Easy revoke (delete session)    | Server state, sticky sessions in cluster    |
| JWT            | Stateless, scales horizontally  | Hard to revoke; token size in every request |
| JWT + refresh  | Short access, revocable refresh | More moving parts                           |

**Examples:**

```http
Set-Cookie: sid=abc123; HttpOnly; Secure; SameSite=Lax; Path=/
Authorization: Bearer eyJhbGciOiJIUzI1NiIs…
```

**Thoughts:**

JWT in `localStorage` is XSS-vulnerable — prefer HttpOnly cookie or memory + refresh. Always use `Secure`, `HttpOnly`, `SameSite` on session cookies.

Sessions are easy to revoke; JWTs scale without a lookup — pick revoke vs scale, not “JWT is more secure.”

**Sources:**

[jwt.io](https://jwt.io/) · [MDN — HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

---

</details>

<details>
  <summary>Access token refresh</summary>

### Access token refresh

**Definition:** **Access token refresh** renews a short-lived **access token** using a long-lived **refresh token** so the user stays signed in without re-entering credentials — typically via `POST /token` with `grant_type=refresh_token`.

**How it works:**

1. **Login** — Authorization Code + PKCE (SPA) or password/session establish trust.
2. **Issue** — IdP returns `access_token` (minutes–hours) + `refresh_token` (days, optional `id_token`).
3. **API calls** — client sends `Authorization: Bearer <access_token>` until `401` or `exp` nears.
4. **Refresh** — client calls token endpoint with `refresh_token`; server returns new pair.
5. **Rotation** — issue a **new** refresh token each use and invalidate the old one — detects theft (reuse of stolen refresh → revoke family).
6. **Retry** — replay the failed API request once with the new access token.

| Storage           | Refresh location                                           | Notes                                  |
| ----------------- | ---------------------------------------------------------- | -------------------------------------- |
| SPA + BFF         | HttpOnly cookie on BFF; refresh Route Handler              | Preferred — refresh secret never in JS |
| SPA + API gateway | Secure httpOnly cookie or backend-for-frontend             | Same as BFF                            |
| Mobile            | OS secure storage (Keychain / Keystore)                    | PKCE on login; rotate refresh          |
| Pure SPA (no BFF) | Memory for access; refresh in httpOnly cookie if same site | Avoid `localStorage` for either token  |

**Silent refresh:** on `401`, queue in-flight requests, call refresh once, update access token, retry — use a single-flight lock so ten parallel 401s trigger one refresh. Libraries (Auth0 SDK, NextAuth, custom middleware) wrap this.

**When to use:** Any JWT access token with TTL &lt; session length. Mandatory with OAuth2/OIDC SPAs. Pair with [Secure BFF patterns](./6_web_api_security.md#secure-bff-patterns) when the browser must not hold refresh tokens.

#### Common pitfalls

- Long-lived access tokens “to avoid refresh” — stolen bearer works until expiry.
- Refresh token in `localStorage` — XSS steals long-lived credential.
- No rotation — stolen refresh works until manual revoke.
- Infinite refresh retry loop on invalid refresh — clear session and redirect to login.

**Pluses and minuses:**

| +                                    | −                                                     |
| ------------------------------------ | ----------------------------------------------------- |
| Short access TTL limits blast radius | More moving parts (rotation store, 401 handling)      |
| Silent UX without re-login           | Refresh endpoint is high-value — rate-limit and audit |
| Rotation detects token replay        | Logout must revoke refresh server-side                |

**Examples:**

```http
POST /oauth/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&refresh_token=def502…
&client_id=spa-client
```

```ts
let refreshPromise: Promise<string> | null = null;

async function fetchWithRefresh(input: RequestInfo, init?: RequestInit) {
  let res = await fetch(input, {
    ...init,
    headers: { ...init?.headers, Authorization: `Bearer ${getAccessToken()}` },
  });
  if (res.status !== 401) return res;

  refreshPromise ??= refreshAccessToken().finally(() => {
    refreshPromise = null;
  });
  await refreshPromise;

  return fetch(input, {
    ...init,
    headers: { ...init?.headers, Authorization: `Bearer ${getAccessToken()}` },
  });
}
```

**Thoughts:** _Refresh is a second credential — protect it more than access. BFF + HttpOnly cookie is the default secure shape for web apps._

**See also:** [OAuth2 & OIDC overview](#oauth2--oidc-overview) · [Sessions & JWT](#sessions--jwt) · [Secure BFF patterns](./6_web_api_security.md#secure-bff-patterns)

**Sources:**

[OAuth.net — Refresh Token](https://oauth.net/2/grant-types/refresh-token/) · [OAuth.net — Refresh tokens](https://oauth.net/2/refresh-tokens/) · [OAuth 2.0 for Browser-Based Apps (PKCE)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-browser-based-apps)

---

</details>

<a name="9-storage"></a>

## 9. Storage

<details>
  <summary>Cookies, sessionStorage & localStorage</summary>

### Cookies, sessionStorage & localStorage

**Definition:** Same three client stores as in [JS Browser — storage](./3_js_browser_extra.md#browser-storage) — cookies are also HTTP state; Web Storage is JS-only key/value.

**How it works:** Cookies are sent on matching requests (~4 KB). `sessionStorage` / `localStorage` are ~5 MiB each, sync strings, never auto-sent. HttpOnly cookies are still sent to the server.

**When to use:** Auth the server must see → cookie. Small client prefs → Web Storage. HTTP flags: [Secure & HttpOnly cookies](#secure--httponly-cookies).

**Thoughts:** Do not use cookies as a general client database.

**Sources:**

[MDN — Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)

---

</details>

<details>
  <summary>IndexedDB</summary>

### IndexedDB

**Definition:** Async same-origin object database for large structured data (indexes, transactions, blobs) — full pick-the-right-store notes in [JS Browser — storage](./3_js_browser_extra.md#browser-storage).

**How it works:** `indexedDB.open` → `onupgradeneeded` to create object stores → `transaction` + `objectStore`. Quotas are disk-based (not 5 MiB). Safari ITP can evict script-writable IDB after 7 days without interaction.

**When to use:** Offline app data, files, anything too big or too frequent for sync Web Storage.

**Examples:**

```js
const request = indexedDB.open("myApp", 1);
request.onupgradeneeded = (e) => {
  e.target.result.createObjectStore("books", { keyPath: "id" });
};
```

**Sources:**

[MDN — IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)

---

</details>

<details>
  <summary>Web Storage API</summary>

### Web Storage API

**Definition:** `window.localStorage` / `sessionStorage` — origin-scoped string KV; local persists across tabs/restarts, session is per tab. Theory + pitfalls: [JS Browser — storage](./3_js_browser_extra.md#browser-storage).

**How it works:** Sync API; wrap writes in `try/catch`. Objects need `JSON.stringify`.

**When to use:** Theme, locale, non-secret UI state.

**Examples:**

```js
try {
  localStorage.setItem("user", JSON.stringify({ id: 1 }));
  const user = JSON.parse(localStorage.getItem("user") ?? "null");
  sessionStorage.removeItem("tempFilter");
} catch {
  /* quota / disabled storage */
}
```

**Sources:**

[MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

---

</details>

<details>
  <summary>Secure & HttpOnly cookies</summary>

### Secure & HttpOnly cookies

**Definition:**

`Secure` limits cookie transmission to HTTPS; `HttpOnly` blocks JavaScript access — both harden session security.

| Flag     | Protects against                                 |
| -------- | ------------------------------------------------ |
| Secure   | Network sniffing on HTTP                         |
| HttpOnly | XSS reading session cookies                      |
| SameSite | CSRF (partial); some clickjacking/leak reduction |

**Examples:**

```http
Set-Cookie: sid=abc; Secure; HttpOnly; SameSite=Lax; Max-Age=86400
```

**Sources:**

[MDN — Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)

---

</details>

<a name="10-protocols"></a>
