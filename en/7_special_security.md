# Web security

Security model and common web attacks.

[← Back to hub](./7_special.md)

### Related docs

- [Computer science](./7_special_cs.md) · [Architecture patterns](./7_special_architecture.md) · [SOLID](./7_special_engineering.md) · [Design patterns](./7_special_patterns.md) · [SDLC](./7_special_sdlc.md) · [Testing](./7_special_testing.md) · [Web security](./7_special_security.md) · [Interview leftovers](./7_special_interview.md)

### Sections

- [13. Web security](#13-web-security)
- [14. Common web attacks](#14-common-web-attacks)

---

## 13. Web security

<details>
  <summary>Web application security (principles)</summary>

### Web application security (principles)

**Definition:**

Practices to protect confidentiality, integrity, and availability of web apps and user data.

| Area                   | Key practices                                                                       |
| ---------------------- | ----------------------------------------------------------------------------------- |
| **Authentication**     | Strong passwords, MFA, OAuth; hash+salt passwords (bcrypt/argon2); rate-limit login |
| **Authorization**      | RBAC/ABAC; least privilege; server-side checks on every request                     |
| **Input validation**   | Whitelist input; encode output; parameterized queries                               |
| **Session management** | HttpOnly, Secure, SameSite cookies; short TTL; rotate on privilege change           |
| **Transport**          | HTTPS everywhere; HSTS                                                              |
| **Headers**            | CSP, X-Frame-Options, X-Content-Type-Options                                        |
| **Dependencies**       | Lock files, audit (`npm audit`), minimal deps                                       |
| **Logging**            | Audit auth events; never log secrets                                                |

**When to use:** Always in production. Threat model before launch.

**Pluses and minuses:**

| + (defense in depth) | −                               |
| -------------------- | ------------------------------- |
| Layered mitigations  | Complexity and perf overhead    |
| Compliance readiness | False sense if only client-side |

**Thoughts:**

Security is ongoing — not a checklist once. Transport crypto: [Encryption](#encryption) · [HTTPS](./6_web_api_http.md#https). HTTP-specific headers: [6_web_api.md](./6_web_api.md).

---

</details>

<details>
  <summary>Encryption (symmetric, asymmetric, hashing)</summary>

### Encryption

**Definition:** Encryption transforms data with a **cipher** and a **key** so unauthorized parties cannot read it; only someone with the matching key (or pair) can reverse the transform. **Cryptography** studies ciphers, keys, and whole cryptosystems (storage + transit). Hashing is related but is **not** encryption: it is a one-way digest.

**How it works:**

- The internet (IP/TCP) does **not** encrypt by default. Confidentiality is added by users and protocols: **TLS/HTTPS**, mail over TLS, signed APIs, tokens. IP itself never grew a built-in “privacy layer.”
- Judge an algorithm by **cryptographic strength** (resistance to cryptanalysis) and **speed** (work vs input size, ops/byte, hardware). Strength without usable speed (or vice versa) is the wrong pick.
- **Symmetric** — same secret for encrypt and decrypt (block ciphers such as AES; stream ciphers). Caesar/Atbash/Vigenère are teaching toys, not production. Fast, short keys for a given strength; key **distribution** and **N² pairwise secrets** are the pain.
- **Asymmetric (public-key)** — key **pair**. Public key encrypts (or verifies); private key decrypts (or signs). RSA, ECC. No pre-shared secret; slower, longer keys. Typical web use: handshake / wrap a **session** key, not bulk HTTP bytes. **SSH** uses the same idea for login: private key stays on the client; public key is in `authorized_keys` — [DevOps — SSH](./15_devops.md#ssh).
- **Hybrid (real TLS):** parties authenticate (certs) and **agree** a fresh **symmetric** session key (modern: **ECDHE**, not “RSA-encrypt the session token”). AES-GCM (or similar) then encrypts the session. Keys die with the session or when revoked.
- **Digital signatures** invert the “who can read” goal: **private** key signs; **public** key verifies. Anyone can check authorship/integrity; only the private-key holder could have signed. Signatures usually hash the document first, then sign the digest — they do not encrypt the whole file. **Diffie–Hellman** is **key agreement**, not a signature scheme. **ECDSA** is a signature algorithm (used in TLS and in some blockchains).
- **Cryptographic hashes** (MDN: one-way, tamper-evident, collision-resistant, fixed-length). Tiny input changes → unrelated digest. Collisions are **hard**, not impossible — do not claim “unique forever.” **MD5 / SHA-1** are broken for security. **SHA-256/512** for integrity; **Argon2id / scrypt / bcrypt / PBKDF2** for **passwords** (slow + unique **salt**). Fast SHA-256 alone is the wrong password store ([OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)).
- **Web surface:** TLS, X.509 certs, signatures, hashes. Details of certs/handshake stay in [HTTPS](./6_web_api_http.md#https) and [TLS certificates](./6_web_api_http.md#tls-certificates-x509--pki).

**When to use:**

HTTPS sessions; encrypting a file at rest with AES; verifying a download with SHA-256; storing login secrets with Argon2id; Git content-addressing (see [Git internals](./0_git_core.md#git-internals--dag) — Git still uses SHA-1 for objects; that is integrity addressing, not password hashing).

| Keys                 | Role                                                    | Typical algorithms (status)                                                                  |
| -------------------- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **None (digest)**    | Integrity, Git objects, **not** reversible              | SHA-2/3; avoid MD5/SHA-1 for security. Passwords: Argon2id / bcrypt, not “hash = encryption” |
| **One (symmetric)**  | Fast bulk data                                          | AES (+ mode GCM/CTR); historical: DES (broken), RC4 (do not use)                             |
| **Two (asymmetric)** | Key agreement, encryption of small payloads, signatures | RSA, ECC/ECDH, ECDSA, DSA. DH = agreement                                                    |

| Symmetric vs asymmetric | Symmetric                   | Asymmetric                          |
| ----------------------- | --------------------------- | ----------------------------------- |
| Speed / bulk data       | Fast                        | Slow; message size tied to key size |
| Key handling            | Must share the secret first | Public key can travel in the open   |
| Key count at scale      | Many pairwise secrets       | One pair per party                  |

#### Common pitfalls

Calling hashes “irreversible encryption.” Using MD5/SHA-1 for passwords or certs. Encrypting passwords so they can be decrypted. RSA-encrypting whole payloads. Teaching Caesar as “how HTTPS works.” Mixing **hash tables** (non-crypto bucket index — [Hash table](./7_special_data_structures_advanced.md#hash-table)) with SHA. Assuming DH is a signature.

**Examples:**

SHA-256 digest in a secure context (`SubtleCrypto.digest` — [MDN](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/digest)). Use this for integrity, not for storing passwords.

```js
async function sha256Hex(message) {
  const bytes = new TextEncoder().encode(message);
  const buf = await crypto.subtle.digest("SHA-256", bytes);
  return Array.from(new Uint8Array(buf))
    .map((b) => b.toString(16).padStart(2, "0"))
    .join("");
}
```

**Thoughts:** _TLS = public-key handshake + fast symmetric session; hashes prove integrity and (with a slow KDF) protect passwords — they do not decrypt._

**Sources:**

[MDN — Symmetric-key cryptography](https://developer.mozilla.org/en-US/docs/Glossary/Symmetric-key_cryptography) · [MDN — Public-key cryptography](https://developer.mozilla.org/en-US/docs/Glossary/Public-key_cryptography) · [MDN — Hash function](https://developer.mozilla.org/en-US/docs/Glossary/Hash_function) · [MDN — SubtleCrypto.digest](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto/digest) · [OWASP Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) · [Doka — Encryption](https://doka.guide/tools/encryption/) (concept map; prefer MDN/OWASP for algorithm status)

---

</details>

<a name="14-common-web-attacks"></a>

## 14. Common web attacks

Using these techniques against systems you do not own is **illegal**. Study them only to defend apps you maintain.

Any place users can type, upload, or pass data is an attack surface. Harm = leaked data, actions as the victim, or taking the service down.

Catalog (Doka): [clickjacking](#clickjacking) · [XSS](#xss-cross-site-scripting) · [SQLi](#sql-injection) · [stored XSS / keylogging](#stored-xss--xss-driven-keylogging) · [CSP](#content-security-policy-csp) · [CSRF](#csrf-cross-site-request-forgery) · [SSRF](./6_web_api_security.md#ssrf-server-side-request-forgery) · [DoS](#denial-of-service-dos--ddos) · [Slowloris](#slowloris-slow-http-dos) · [zip bombs](#decompression-bombs-data-bombs) · [MITM](#man-in-the-middle-mitm)

HTTP headers, cookies, CORS: [Web API — security](./6_web_api.md). SQL bind: [Database — SQL injection](./9_database_sql.md#sql-injection). Guided labs (not exploits): [application.security](https://application.security).

<details>
  <summary>XSS (Cross-Site Scripting)</summary>

### XSS (Cross-Site Scripting)

**Definition:** Attacker injects script that runs in the victim's browser **in your origin** — so it can act as the user (cookies, DOM, requests).

**How it works:**

Untrusted string is treated as HTML/JS. Classic **reflected** XSS: search `q=` is echoed into the page; a payload in the URL runs on that response. **Stored** XSS: payload is saved (DB, file, cache) and runs later for every viewer. **DOM XSS:** client JS writes untrusted data into the DOM (`innerHTML`, `document.write`).

| Type          | When it fires                                                |
| ------------- | ------------------------------------------------------------ |
| **Reflected** | Immediate, in the HTTP response (often a crafted URL)        |
| **Stored**    | Later, for many users — harder to clean (must purge storage) |
| **DOM-based** | No server echo required — buggy client rendering             |

| Mitigation                                                 | Why                                                                           |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Encode output for the **context** (HTML / attr / URL / JS) | `<script>` becomes harmless text (`&lt;script&gt;…`)                          |
| `textContent` / framework text nodes                       | React/Svelte text is escaped; `innerHTML` / `dangerouslySetInnerHTML` are not |
| Sanitize if you must allow HTML                            | DOMPurify (or equivalent) — never “escape with `encodeURI`” for HTML          |
| CSP (`script-src` nonce/hash)                              | Backup if encoding fails — unknown scripts do not run                         |
| HttpOnly session cookies                                   | XSS cannot read the cookie (can still act **as** the user via the page)       |

**When to use:** Search pages, comments, profiles, error messages that echo input, rich-text editors. Reflected XSS often looks like a search query echoed into “nothing found for …”.

**Examples:**

```js
// Unsafe — parses markup
el.innerHTML = userInput;

// Safer for plain text (not for a <script> element's textContent — that still runs)
el.textContent = userInput;
```

HTML encoding turns markup into text the parser will not execute:

```
<input>  <script>…</script>
output   &lt;script&gt;…&lt;/script&gt;
```

**Thoughts:**

SQL injection is **not** XSS — different channel (SQL vs HTML/JS). `encodeURI` / `encodeURIComponent` encode **URLs**, not HTML. Encode **on output** in the right context; also validate on the server. Labs: DOM XSS / reflected XSS on [application.security](https://application.security). Automated checks: [Security testing](./7_special_testing_layers.md#security-testing).

_Treat every user string as data — encode for the output context; CSP is the backup net._

**Sources:**

[OWASP XSS](https://owasp.org/www-community/attacks/xss/) · [MDN — XSS](https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/XSS) · [Doka — web security](https://doka.guide/security/web-security/)

---

</details>

<details>
  <summary>Stored XSS & XSS-driven keylogging</summary>

### Stored XSS & XSS-driven keylogging

**Definition:** Stored XSS persists in the app. A common payload class is a **keylogger**: a script that records keystrokes (and other events) and sends them to an attacker.

**How it works:**

1. Attacker posts content that the app stores unsanitized (comment, “tweet”, profile).
2. Every subscriber/viewer loads that HTML; the script runs in the site origin.
3. Listeners on `keydown` / form fields collect credentials; exfiltration often uses a request the page is allowed to make (e.g. `img`/`fetch` to an attacker host unless CSP `connect-src`/`img-src` blocks it).
4. Cleanup requires deleting or sanitizing stored records — not just a code fix.

| Pros (for attacker)                   | Mitigation                                                     |
| ------------------------------------- | -------------------------------------------------------------- |
| Hits many users over time             | Encode on **store and render**; treat old DB rows as untrusted |
| Can self-spread via other bugs        | CSP + no `unsafe-inline`; lock down `connect-src`              |
| Steals passwords typed on your origin | HttpOnly cookies; MFA; treat XSS as full account compromise    |

**When to use:** Any UGC that is rendered as HTML later. Keylogging is a **consequence** of XSS, not a separate protocol — the script runs as the site and can listen for keys, then send them out unless CSP `connect-src` / `img-src` blocks it.

**Thoughts:**

Cleanup is a **data** problem (purge or re-encode stored rows), not only a code fix. A worm can re-post itself via other bugs. Lab: stored XSS on [application.security](https://application.security).

_Stored XSS is delayed and multi-victim; encoding + CSP + purging stored HTML are all required._

**Sources:**

[OWASP XSS](https://owasp.org/www-community/attacks/xss/) · [MDN — CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)

---

</details>

<details>
  <summary>Content-Security-Policy (CSP)</summary>

### Content-Security-Policy (CSP)

**Definition:** HTTP header listing **trusted sources** for scripts, styles, images, connections — a second line of defense if XSS encoding fails.

**How it works:** The browser blocks loads and inline script outside the policy. `default-src` fills unspecified fetch directives. `script-src` with **nonces/hashes** (not `'unsafe-inline'`) is what actually stops injected `<script>`. `frame-ancestors` is clickjacking (who may **embed you**), not `frame-src` (what **you** may iframe).

**When to use:** Every HTML response. Pair with output encoding — CSP alone is not enough if you allow `'unsafe-inline'`.

**Pluses and minuses:**

| +                                                                 | −                                                 |
| ----------------------------------------------------------------- | ------------------------------------------------- |
| Limits stolen-keystroke beacons and injected CDNs                 | Breaks inline scripts/styles until you use nonces |
| `frame-ancestors` replaces obsolete `X-Frame-Options: ALLOW-FROM` | `<meta>` CSP cannot set `frame-ancestors`         |

**Examples:**

```http
Content-Security-Policy: default-src 'self' https://*.trusted.com; script-src 'self'; frame-ancestors 'self'
```

Full directive table: [Web API — CSP](./6_web_api_security.md#content-security-policy-csp). Header catalog: [securityheaders.com](https://securityheaders.com/).

**Thoughts:** _Allow-list origins; never treat `'unsafe-inline'` as XSS protection._

**Sources:**

[MDN — CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) · [MDN — frame-ancestors](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy/frame-ancestors)

---

</details>

<details>
  <summary>CSRF (Cross-Site Request Forgery)</summary>

### CSRF (Cross-Site Request Forgery)

**Definition:** Attacker tricks an **already authenticated** browser into sending a state-changing request to your site. Session cookies are sent automatically; the server thinks the user intended the action.

**How it works:**

1. User has a session cookie on `app.com`.
2. User visits `evil.com`, which triggers a POST/GET to `app.com` (form, image, `fetch` with cookies if CORS allows).
3. Server authenticates the cookie but never checks that **this request was authorized by the user** (CSRF token / SameSite / Origin).

- **Authentication** — who is this? (session is valid).
- **Authorization** — is this request allowed **and intended**? CSRF is missing the second check for cookie-authenticated mutations.

| Defense                          | How                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------------------- |
| CSRF token in form / header      | Secret the attacker cannot read (synchronizer or double-submit)                        |
| `SameSite=Lax` or `Strict`       | Cookie not sent on most cross-site POSTs                                               |
| `HttpOnly`                       | Stops JS from **reading** the cookie — does **not** stop CSRF (browser still sends it) |
| `Origin` / `Referer` check       | Reject missing or foreign origin                                                       |
| CORS + no credentialed wildcards | CORS is **not** CSRF protection by itself                                              |
| Re-auth / step-up                | Transfers, password change                                                             |

**When to use:** Cookie sessions + HTML forms (e.g. “send money”); cookie SPA APIs. JWT in `Authorization` is not auto-attached by the browser.

**Examples:**

```html
<form action="/send_money/" method="POST">
  <input type="hidden" name="csrf" value="token-generated-by-server" />
  <!-- rest of form -->
</form>
```

```http
Set-Cookie: sid=abc; HttpOnly; Secure; SameSite=Strict
```

AJAX: send the same secret as `X-CSRF-Token`. Full patterns: [Web API — CSRF](./6_web_api_security.md#csrf-cross-site-request-forgery). Lab: [application.security](https://application.security) CSRF.

**Thoughts:** Never change state on GET. `SameSite` is defense in depth, not a complete substitute for tokens on sensitive POSTs. CORS does not stop cookie CSRF. `HttpOnly` hides the cookie from JS — the browser still **sends** it.

**Sources:**

[OWASP CSRF](https://owasp.org/www-community/attacks/csrf) · [MDN — SameSite](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite)

---

</details>

<details>
  <summary>SQL injection</summary>

### SQL injection

**Definition:** Untrusted input is concatenated into SQL so the attacker changes **query structure** (read, write, or destroy data). This is **injection into SQL**, not XSS.

**How it works:**

Login or search builds `SELECT … WHERE email = '${input}'`. Input like `' OR '1'='1` makes the predicate always true. `UNION SELECT` can append extra result sets if the app returns them. Destructive statements are possible if the DB role allows them.

| Mitigation                                                                   |
| ---------------------------------------------------------------------------- |
| Parameterized queries / prepared statements (bind values, never concatenate) |
| ORM APIs that bind parameters (avoid raw string SQL)                         |
| Least-privilege DB user (app cannot `DROP DATABASE`)                         |
| Never trust client-side “escaping” as the SQLi fix                           |

**When to use:** Any user value in a query — login, search, IDs. Same class exists for NoSQL (operator injection in JSON).

**Examples:** Full dump of impact + five defenses (parameters, ORM, validation, least privilege, procedures): [Database — SQL injection](./9_database_sql.md#sql-injection). Lab: [application.security](https://application.security) SQL injection.

**Thoughts:**

Escaping on the **frontend** does not protect the database. Validate types/length as extra defense; parameterization is the control.

_Bind values; never concatenate user input into SQL. SQLi ≠ XSS._

**Sources:**

[OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)

---

</details>

<a name="clickjacking"></a>

<details>
  <summary>Clickjacking</summary>

### Clickjacking

**Definition:** UI redress — a decoy page overlays a **transparent iframe** of your site so the user's click hits **your** authenticated UI (like, pay, follow).

**How it works:**

1. Evil page shows a tempting button.
2. Transparent `<iframe src="https://yoursite.com/…">` sits on top (victim's cookies apply in the iframe).
3. Click hits the real control in the iframe.

| Header                                                 | Effect                                                     |
| ------------------------------------------------------ | ---------------------------------------------------------- |
| `X-Frame-Options: DENY`                                | Never embed this document in a frame                       |
| `X-Frame-Options: SAMEORIGIN`                          | Embed only if all ancestors share this origin              |
| `X-Frame-Options: ALLOW-FROM`                          | **Obsolete** — modern browsers **ignore the whole header** |
| CSP `frame-ancestors 'none'` / `'self'` / listed hosts | Preferred replacement; more flexible than XFO              |

**When to use:** Payments, deletes, OAuth consent, social like/follow. SameSite cookies help when auth is cookie-based but do **not** replace framing controls (public unauthenticated UI can still be framed).

**Examples:**

```http
X-Frame-Options: SAMEORIGIN
Content-Security-Policy: frame-ancestors 'self'
```

`X-Frame-Options` must be an **HTTP header**, not `<meta>`. `SAMEORIGIN` is the usual choice when the product iframes itself (historically used by Twitter/X).

**Thoughts:**

CSP `frame-ancestors` cannot be set via `<meta http-equiv>`. Frame-busting JS is weak alone. Lab: [application.security](https://application.security) clickjacking.

_Do not allow foreign pages to frame authenticated UI — `frame-ancestors` first, XFO as fallback._

**Sources:**

[javascript.info/clickjacking](https://javascript.info/clickjacking) · [OWASP Clickjacking](https://owasp.org/www-community/attacks/Clickjacking) · [MDN — frame-ancestors](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)

---

</details>

<details>
  <summary>DoS and DDoS</summary>

### Denial of Service (DoS / DDoS)

**Definition:** Overwhelm the service so legitimate users cannot use it (availability). **DDoS** is the same idea from many sources at once.

**How it works:**

Flood HTTP (or other) so CPU, RAM, sockets, or bandwidth are exhausted. One machine can still hurt a small origin; many bots (botnets) overwhelm IP-based limits.

| Mitigation                    | Limit                                                       |
| ----------------------------- | ----------------------------------------------------------- |
| Rate limiting (user, IP, key) | Helps abuse and brute force; **weak vs distributed floods** |
| Capacity + CDN / Anycast      | Absorb or drop junk at the edge                             |
| Autoscaling / failover        | Replace saturated instances; change IPs if needed           |
| WAF / scrubbing               | Filter known flood patterns                                 |

**When to use:** Public APIs, login, expensive endpoints (search, reports). See [Web API — 429](./6_web_api_http.md#rate-limiting-429) and [System design — rate limiting](./0_system_design_data.md#rate-limiting).

**Thoughts:**

There is no 100% DoS shield. “Reflect DDoS back at the attacker” is not a practical or safe control — drop/filter at the edge instead.

_Availability: rate-limit + capacity at the edge; IP limits fail against a botnet._

**Sources:**

[OWASP Denial of Service](https://owasp.org/www-community/attacks/Denial_of_Service)

---

</details>

<details>
  <summary>Slowloris</summary>

### Slowloris (slow HTTP DoS)

**Definition:** DoS by opening many connections and sending request data **extremely slowly**, so workers stay busy on incomplete requests.

**How it works:**

Server cannot finish parsing headers/body; connection stays open. When one drops, the attacker opens another. Thread/process servers (classic Apache prefork) are especially sensitive; one client can hold many slots.

| Mitigation                                 | Why                                                           |
| ------------------------------------------ | ------------------------------------------------------------- |
| Short read timeouts / min bytes per second | Close idle incomplete requests (e.g. Apache `mod_reqtimeout`) |
| Cap connections per IP                     | Limits one-source Slowloris                                   |
| Event-driven servers / reverse proxy       | Fewer threads blocked per slow client                         |
| Rate limits                                | Extra signal for suspicious traffic                           |

**When to use:** Any origin that waits a long time for incomplete HTTP. Frontend cannot fix this — it is origin/proxy config.

**Thoughts:** _Timeouts and connection limits, not “more XSS escaping.”_

**Sources:**

[Qualys — slow HTTP attacks](https://blog.qualys.com/vulnerabilities-threat-research/2011/11/02/how-to-protect-against-slow-http-attacks)

---

</details>

<details>
  <summary>Decompression / zip bombs</summary>

### Decompression bombs (data bombs)

**Definition:** A tiny upload (archive, gzip, some images) that **explodes in size** when the server decompresses it — disk, CPU, or RAM exhaustion (DoS).

**How it works:**

Nested zip layers (the well-known **42.zip** is ~42 KB and expands to a huge tree if fully unpacked). Similar pattern: gzip/deflate bombs, XML “billion laughs.” The HTTP body looks small; processing is huge.

| Layer    | Control                                                                 |
| -------- | ----------------------------------------------------------------------- |
| Frontend | Max file size + MIME allow-list (UX only — not sufficient)              |
| Server   | Cap decompressed size; stream with a hard limit; refuse nested archives |
| Images   | Decode with max width/height/pixels; do not unbounded inflate           |

**When to use:** Avatar upload, unzip-on-server, “import zip”, image pipelines.

**Thoughts:**

Frontend MIME/size checks are bypassed by a crafted request. Limits belong on the server. Frontend can still cap size/MIME for UX.

_Bound decompressed size on the server — never unpack untrusted archives unbounded._

**Sources:**

[OWASP — zip bombs / decompression](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload) · [Doka — web security](https://doka.guide/security/web-security/)

---

</details>

<details>
  <summary>MITM (Man in the Middle)</summary>

### Man in the Middle (MITM)

**Definition:** Attacker sits on the path (rogue AP, compromised router) and **reads or alters** traffic between browser and server.

**How it works:**

On open Wi-Fi, plaintext **HTTP** shows cookies and passwords. A rogue AP can impersonate the router. Homegrown “here is my public key” over HTTP can be swapped for the attacker’s key (classic RSA-over-cleartext story). **HTTPS** uses TLS with certificates from a **trusted CA**; the client already has CA roots, so a café laptop cannot silently substitute the server identity. Tampering breaks the handshake or record integrity.

| Mitigation                                     | Why                                             |
| ---------------------------------------------- | ----------------------------------------------- |
| HTTPS everywhere                               | Confidentiality + integrity on the wire         |
| HSTS                                           | Stops downgrade to HTTP after first HTTPS visit |
| Do not ignore cert warnings                    | User bypass re-enables MITM                     |
| Certificate Transparency / pinning (rare, ops) | Extra trust checks if a CA is compromised       |

**When to use:** Cafés, hotels, ISP interception. See [Web API — HTTPS](./6_web_api_http.md#https) and [TLS certificates](./6_web_api_http.md#tls-certificates-x509--pki).

**Thoughts:**

Trust still depends on CAs (root store + chain). Compromised or rogue CAs are an industry-level failure, not an app bug. RSA is one algorithm family; modern TLS uses negotiated cipher suites and ephemeral key agreement — not “encrypt the first HTTP hello with RSA.”

_HTTPS/TLS + HSTS; never click through certificate warnings._

**Sources:**

[MDN — HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS) · [MDN — TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS)

---

</details>

<details>
  <summary>IDOR (Insecure Direct Object Reference)</summary>

### IDOR (Insecure Direct Object Reference)

**Definition:** Authorization failure where changing an **object ID** in URL, body, or API call grants access to another user's resource because the server checks authentication but **not ownership**.

**How it works:**

1. User A is authenticated (`GET /api/orders/123` — their order).
2. User A changes ID to `124` — server returns User B's order.
3. Root cause: endpoint verifies "is logged in?" but not "does this resource belong to this user?"
4. UUIDs obscure IDs but **do not fix** IDOR — attacker can still enumerate or leak IDs.

| Pros (for attacker)                       | Mitigation                                               |
| ----------------------------------------- | -------------------------------------------------------- |
| Trivial to exploit — change one parameter | **Authorize every resource access** server-side          |
| Very common in API audits                 | Check `resource.ownerId === session.userId` (or RBAC)    |
| Works with sequential and UUID IDs        | Use policy layer (ABAC) — IDs are not secrets            |
|                                           | Integration tests for cross-user access (negative cases) |
|                                           | Audit logs for suspicious ID probing                     |

**When to use:** Any API returning user-specific data by ID — orders, invoices, messages, files, medical records. Especially dangerous in REST where URL structure exposes resource type and ID.

**Examples:**

```js
// ❌ IDOR — only checks auth, not ownership
app.get("/api/orders/:id", async (req, res) => {
  const order = await db.orders.findById(req.params.id);
  res.json(order); // any logged-in user can read any order
});

// ✅ Authorize ownership
app.get("/api/orders/:id", async (req, res) => {
  const order = await db.orders.findById(req.params.id);
  if (!order || order.userId !== req.user.id) return res.status(404).end();
  res.json(order);
});
```

**Thoughts:** Return **404** (not 403) for missing/unauthorized resources to avoid leaking existence. Most common API bug in pentests. OWASP Top 10 category: Broken Access Control.

**Sources:**

[OWASP IDOR](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References)

---

</details>

<details>
  <summary>Supply chain attacks</summary>

### Supply chain attacks

**Definition:**

Compromise via malicious or hijacked dependencies (npm packages, typosquatting).

| Mitigation                                  |
| ------------------------------------------- |
| Lock files, `npm audit`, Dependabot         |
| Pin versions; review new deps               |
| Subresource Integrity (SRI) for CDN scripts |
| Minimal dependency tree                     |

**Thoughts:** `postinstall` scripts and abandoned packages are attack surface. Use `npm ci` in CI.

**Sources:**

[dev.to — frontend security attacks](https://dev.to/tinymce/7-common-front-end-security-attacks-372p)

---

</details>
