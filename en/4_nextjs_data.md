# Next.js — Data

Fetching, caching, revalidation, Server Actions, and Route Handlers.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [7. Data fetching](#7-data-fetching)
- [8. Caching & revalidation](#8-caching-revalidation)
- [9. Server Actions vs Route Handlers](#9-server-actions-vs-route-handlers)

---

## 7. Data fetching

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Server Component fetching</summary>

### Server Component fetching

**Definition:** Server Components fetch data directly — `async` functions, `fetch()`, or database clients. No `useEffect` waterfalls on the client.

**How it works:**

- Mark `page.tsx` / `layout.tsx` as `async` and `await` data **before** returning JSX. The server runs this code once per request (or at build time for static routes).
- **React request memoization** deduplicates identical `fetch` calls in the same render pass (same URL + options).
- Results are serialized into the RSC payload; only the **minimum** props cross into Client Components (`"use client"` children).
- Errors during fetch typically surface as the segment `error.tsx` boundary unless you handle them in the component.
- Reads belong here by default; **mutations** belong in Server Actions or Route Handlers, not in render.

**When to use:**

- Default for all **read** operations in App Router (lists, detail pages, layout shells).
- Any data needed for **SEO**, first paint, or auth gates before showing UI.
- Whenever secrets (API keys, DB URLs) must never reach the browser.

**Pluses and minuses:**

| +                                                           | −                                            |
| ----------------------------------------------------------- | -------------------------------------------- |
| Data available on first paint                               | Tight coupling to server runtime             |
| Secrets stay on server                                      | Errors surface at render time                |
| Automatic deduplication of identical `fetch` in one request | ORM in serverless — watch connection pooling |

**Examples:**

```tsx
async function getProducts() {
  const res = await fetch("https://api.example.com/products", {
    next: { tags: ["products"] },
  });
  if (!res.ok) throw new Error("Failed to fetch products");
  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();
  return (
    <ul>
      {products.map((p: { id: string; name: string }) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** Pick the **source** first (remote SaaS, DB in the same app, or another service you run), then the **layer** (Server Component vs Client Component vs Route Handler). Walkthrough: [Choosing a data fetch path](#choosing-a-data-fetch-path) · [Fetching from a remote API](#fetching-from-a-remote-api) · [Colocated backend in Next.js](#colocated-backend-in-nextjs) · [Fetching from a separate backend](#fetching-from-a-separate-backend).

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>Choosing a data fetch path</summary>

### Choosing a data fetch path

**Definition:** A decision guide for **where data lives** and **which Next.js surface** should load it — before you pick SWR, Prisma, or a Route Handler.

**How it works:**

| Question                                                     | If yes →          | Typical pattern                                                                        |
| ------------------------------------------------------------ | ----------------- | -------------------------------------------------------------------------------------- |
| Is the data owned by a **vendor** (CMS, Stripe, maps)?       | Remote HTTP API   | `fetch` or SDK in **Server Component** with env secrets                                |
| Is the data in **your DB** in the same repo/deploy?          | Colocated backend | `lib/` + ORM in **Server Component** — no self-`fetch` to `/api`                       |
| Is the data behind **another team’s service** you host?      | Separate backend  | Server `fetch(API_URL)`; browser uses **Route Handler** proxy                          |
| Must the UI **update every few seconds** without navigation? | Client read       | `fetch('/api/...')`, SWR, or TanStack Query in **Client Component**                    |
| Is it a **form submit / mutation**?                          | Server write      | [Handling mutations in Next.js](#handling-mutations-in-nextjs) — not `useEffect` fetch |

**Layer rules (App Router):**

- **Server Component** → read at render from source (DB, remote URL, your API URL). Fastest path; best for HTML and SEO.
- **Route Handler** → HTTP contract for browser, mobile, webhooks, third parties. Optional BFF that hides service tokens.
- **Server Action** → mutations from the UI; do not use as a general **read** API (queued execution).
- **Client fetch** → after hydration; polling, personalization, browser-only APIs.

**When to use:** Greenfield page design, refactors from Pages Router `getServerSideProps`, or splitting a monolith into Next + API.

**Thoughts:** Wrong pattern: Server Component `fetch('http://localhost:3000/api/users')` during static build — no server is listening. Wrong pattern: putting `DATABASE_URL` in a Client Component. Right pattern: one obvious owner per dataset (see the three source-specific topics below).

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend) · [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>Fetching from a remote API</summary>

### Fetching from a remote API

**Definition:** Read data from a **third-party or vendor HTTP API** (headless CMS, payments, analytics, public REST/GraphQL) inside Server Components, Server Actions, or Route Handlers — using `fetch` or an SDK with a server-only API key.

**How it works:**

- Call happens on the **server** during render (or in a handler/action), so secrets stay in env vars (`process.env.CMS_TOKEN`), not in the browser bundle.
- Use an **absolute URL** (`https://api.vendor.com/...`). Next.js `fetch` caching (`cache`, `next.revalidate`, `next.tags`) applies to these GETs when you use `fetch`.
- At **build time** (static generation), the remote API must be reachable; otherwise the build fails or you must mark the route dynamic.
- Do **not** route Server Component reads through your own `app/api/...` Route Handler unless the client needs that endpoint — extra HTTP hop and build-time pitfalls ([Backend for Frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)).

**When to use:**

- Headless **CMS** (Sanity, Contentful, Strapi) for marketing and docs.
- **Payments** catalog, tax, shipping quotes (Stripe, etc.) — server-only keys.
- **Maps, email validation, ML** APIs where you do not store the canonical data.
- Public **REST/GraphQL** feeds (GitHub, open data) with no custom backend.

**Pluses and minuses:**

| +                                    | −                                       |
| ------------------------------------ | --------------------------------------- |
| No duplicate backend to maintain     | Latency and rate limits from vendor     |
| Next cache tags can wrap vendor GETs | Vendor downtime breaks your page render |
| Clear security boundary for API keys | Schema/version drift on external APIs   |

**Common pitfalls:**

- Calling the vendor from the **browser** and leaking API keys — always proxy on the server.
- Wrapping vendor GETs in your own Route Handler **only** for Server Components — extra latency; call vendor directly from RSC instead.
- Static page at build time when the CMS is down — use `dynamic = 'force-dynamic'`, `cache: 'no-store'`, or ISR with sane fallbacks.
- Forgetting `if (!res.ok)` — Next does not throw on 404/500 from `fetch`.

**Examples:**

```tsx
async function getPosts() {
  const res = await fetch("https://cms.example.com/api/posts", {
    headers: { Authorization: `Bearer ${process.env.CMS_TOKEN}` },
    next: { tags: ["cms-posts"], revalidate: 60 },
  });
  if (!res.ok) throw new Error("CMS unavailable");
  return res.json();
}
```

**Thoughts:** Pair `next.tags` with CMS webhooks that call `revalidateTag('cms-posts')` so publishes appear without full redeploy. GraphQL SDKs follow the same rule: run on the server in RSC, not in client bundles.

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data) · [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)

</details>

<details>
  <summary>Colocated backend in Next.js</summary>

### Colocated backend in Next.js

**Definition:** Data lives in the **same Next.js app** — Postgres via Prisma/Drizzle, Redis, file system, or internal `lib/` modules. Server Components call the database or domain functions **directly**, without `fetch("http://localhost:3000/api/...")`.

**How it works:**

- Put queries in `lib/data.ts`, repositories, or Server Actions; import them from `async` Server Components.
- **Route Handlers** (`app/api/.../route.ts`) are for **HTTP clients** (browser, mobile app, webhook, partner) — not for Server Components to read their own app over HTTP.
- **Server Actions** — primary path for **mutations** from forms; avoid using them as a read/query API (queued, sequential).
- Connection pooling matters on serverless — use a pooler (e.g. Vercel Postgres, PgBouncer) or serverless-friendly driver.

**When to use:**

- **Full-stack Next** on Vercel or Node — one git repo, one deploy.
- **Postgres/SQLite/MySQL** with Prisma, Drizzle, or Kysely.
- **Auth + session** read in layout (e.g. `getServerSession`) before rendering children.
- Internal admin tools where every screen is served by the same app.

**Pluses and minuses:**

| +                                                                                                       | −                                                       |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Fastest reads — no self-HTTP                                                                            | Ties data layer to Next runtime                         |
| Transactions and ORM in one process                                                                     | Must not import server-only code into Client Components |
| Same cache/revalidate story for `fetch` to external URLs only; DB uses `use cache` / `unstable_noStore` | Large teams may split API to a separate service later   |

**Common pitfalls:**

- Importing `lib/db` into a Client Component — bundles secrets or breaks the build; keep DB behind Server Components / Actions.
- Opening a **new DB connection per request** without pooling on serverless — exhausts connections; use a pooler.
- Using Route Handlers as the **only** data layer and fetching them from RSC — slower and breaks static generation.
- Caching ORM results incorrectly — use [`use cache`](#use-cache-directive), `unstable_noStore()`, or explicit revalidation, not `fetch` cache.

**Examples:**

```tsx
// File: lib/products.ts — server-only module (never import from "use client")
import { db } from "@/lib/db";

export async function listProducts() {
  return db.product.findMany({ orderBy: { createdAt: "desc" } });
}

// File: app/products/page.tsx  →  URL: /products (Server Component read — no /api hop)
import { listProducts } from "@/lib/products";

export default async function ProductsPage() {
  const products = await listProducts();
  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** Structure as **domain modules** (`lib/orders.ts`, `lib/users.ts`) so a future split to a separate API only changes the implementation, not every page. Client widgets that poll after hydration can call **your** Route Handlers (`fetch("/api/metrics")`) — see [Client-side fetching](#client-side-fetching) and [Route Handlers](#route-handlers).

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend) · [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)

</details>

<details>
  <summary>Fetching from a separate backend</summary>

### Fetching from a separate backend

**Definition:** UI in Next.js, but **business data** comes from **another service you operate** — Express, NestJS, Go API, legacy monolith, or microservice — via HTTP (REST/GraphQL/gRPC gateway).

**How it works:**

- Server Components (or Route Handlers) `fetch` the service **base URL** from env (`API_URL`), with server-side auth (service token, mTLS, signed JWT).
- Same absolute-URL rules as remote APIs; treat your API as an external dependency for caching, timeouts, and retries.
- **Client Components** should usually call a **Next Route Handler** or **public** backend route — not embed service tokens in the browser. The handler forwards cookies or exchanges a session for a service token.
- Split deploys: Next on Vercel, API on another host — CORS matters only for **browser** calls; server-to-server `fetch` is not CORS-limited.

**When to use:**

- **Legacy REST API** already in production; Next is a new frontend.
- **Microservices** (orders service, catalog service) with separate deploy cadence.
- **Mobile + web** share the same backend; Next is one consumer among many.
- **Polyglot** stack (Go/Rust/Java API + React UI on Next).

**Pluses and minuses:**

| +                                     | −                                      |
| ------------------------------------- | -------------------------------------- |
| Independent scaling and deploys       | Extra network hop + failure modes      |
| Clear service boundary                | Duplicated auth/session bridging       |
| Next stays a presentation + BFF layer | Versioning and contract tests required |

**Common pitfalls:**

- Exposing **service tokens** in Client Components or `NEXT_PUBLIC_*` env vars.
- Calling the internal API from the browser without a **BFF** — CORS pain and credential leakage.
- No **timeouts/retries** on server `fetch` — hung renders block the whole page; use `AbortSignal` or a small wrapper.
- Assuming session cookies from Next automatically authenticate the backend — you usually need token exchange or shared auth (JWT, OAuth introspection).

**Examples:**

```tsx
// File: lib/orders.ts — server-only; called from Server Components or Route Handlers
async function getOrders(userId: string) {
  const res = await fetch(`${process.env.API_URL}/v1/users/${userId}/orders`, {
    headers: { Authorization: `Bearer ${process.env.API_SERVICE_TOKEN}` },
    cache: "no-store",
  });
  if (!res.ok) throw new Error("Orders API error");
  return res.json();
}
```

```tsx
// File: app/api/orders/route.ts  →  URL: GET /api/orders
// Client: fetch("/api/orders") — no API_SERVICE_TOKEN in browser
import { auth } from "@/lib/auth";

export async function GET() {
  const session = await auth();
  if (!session) return new Response("Unauthorized", { status: 401 });
  const orders = await getOrders(session.user.id);
  return Response.json(orders);
}
```

**Thoughts:** Next is often the **BFF**: validate the user session, attach service auth, shape DTOs for the UI, and cache short-lived responses in Route Handlers. Compare with [Colocated backend in Next.js](#colocated-backend-in-nextjs) when deciding to merge vs keep a separate API. For mutations, prefer Server Actions that call the backend once, or `POST` from actions — avoid `client → Next API → backend → Next API` chains unless you need a documented public HTTP contract for third parties.

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend) · [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>fetch cache options</summary>

### fetch cache options

**Definition:**

Next.js extends `fetch` with caching semantics that map to SSG/SSR/ISR.

| Option                           | Behavior                         | Pages Router equivalent          |
| -------------------------------- | -------------------------------- | -------------------------------- |
| `cache: 'force-cache'` (default) | Cache until invalidated          | `getStaticProps`                 |
| `cache: 'no-store'`              | Fetch every request              | `getServerSideProps`             |
| `next: { revalidate: N }`        | ISR — revalidate after N seconds | `revalidate` in `getStaticProps` |
| `next: { tags: ['tag'] }`        | Tag for on-demand invalidation   | N/A                              |

**Examples:**

```tsx
const staticData = await fetch(url, { cache: "force-cache" });
const dynamicData = await fetch(url, { cache: "no-store" });
const revalidatedData = await fetch(url, { next: { revalidate: 10 } });
```

**Thoughts:** `fetch` caching applies to **GET** requests extended by Next.js. Direct DB calls use `use cache` or `unstable_noStore()` for cache control.

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching)

</details>

<details>
  <summary>Avoiding waterfalls</summary>

### Avoiding waterfalls

**Definition:** Sequential `await` in one component creates a waterfall. Parallelize independent fetches.

**Examples:**

```tsx
export default async function Page() {
  const [user, posts] = await Promise.all([getUser(), getPosts()]);
  return <Dashboard user={user} posts={posts} />;
}
```

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>Client-side fetching</summary>

### Client-side fetching

**Definition:** Fetch in Client Components via SWR or TanStack Query when data must update without navigation.

Canonical SWR patterns (`mutate`, revalidation options) → [SWR (stale-while-revalidate)](./4_react_ecosystem.md#swr-stale-while-revalidate). Stack map → [Revalidation and invalidation across the stack](#revalidation-and-invalidation-across-the-stack).

For TanStack Query SSR prefetch, `HydrationBoundary`, and persistent cache patterns, see [React — TanStack](./4_react_tanstack.md#ssr-prefetch--hydration).

**When to use:** Infinite scroll, real-time widgets, post-hydration personalization.

**Pluses and minuses:**

| +                                    | −                              |
| ------------------------------------ | ------------------------------ |
| Live updates, polling, optimistic UI | Client waterfall if overused   |
| Works with any REST/GraphQL client   | Worse SEO for critical content |

**Examples:**

```tsx
"use client";

import useSWR from "swr";

export function LiveMetrics() {
  const { data, isLoading } = useSWR(
    "/api/metrics",
    (url) => fetch(url).then((r) => r.json()),
    {
      refreshInterval: 5000,
    },
  );
  if (isLoading) return <p>Loading…</p>;
  return <div>{data.value}</div>;
}
```

**Sources:** [nextjs.org/docs/app/getting-started/fetching-data](https://nextjs.org/docs/app/getting-started/fetching-data)

</details>

<details>
  <summary>draftMode</summary>

<a name="draftmode"></a>

### draftMode

**Definition:** `draftMode()` from `next/headers` enables **Preview / Draft** rendering — bypasses static generation and fetches uncached draft content from a CMS. Async in Next.js 15+.

**When to use:**

Contentful, Sanity, Strapi, or custom CMS preview URLs for editors.

Regular authenticated app previews — use session auth + dynamic rendering instead.

**Pluses and minuses:**

| +                                     | −                                             |
| ------------------------------------- | --------------------------------------------- |
| Editors preview unpublished content   | Must protect enable endpoint with secret      |
| Works with any headless CMS           | Disables static optimization while active     |
| Cookie-based — no special client code | Easy to forget to disable in production tests |

**Examples:**

```tsx
// app/api/draft/route.ts
import { draftMode } from "next/headers";
import { redirect } from "next/navigation";

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const secret = searchParams.get("secret");
  const slug = searchParams.get("slug");

  if (secret !== process.env.DRAFT_SECRET) {
    return new Response("Invalid token", { status: 401 });
  }

  const draft = await draftMode();
  draft.enable();
  redirect(slug ?? "/");
}
```

```tsx
// app/blog/[slug]/page.tsx
import { draftMode } from "next/headers";

export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const { isEnabled } = await draftMode();

  const post = isEnabled
    ? await fetchDraftPost(slug)
    : await fetchPublishedPost(slug);

  return <article>{post.body}</article>;
}
```

**Thoughts:**

- `draftMode().enable()` sets a `__prerender_bypass` cookie; `disable()` clears it.
- Route becomes dynamic while draft mode is active — expected for preview.
- Protect `/api/draft` with `DRAFT_SECRET` — never expose enable URL publicly.

**Sources:** [nextjs.org/docs/app/api-reference/functions/draft-mode](https://nextjs.org/docs/app/api-reference/functions/draft-mode)

</details>

<a name="8-caching--revalidation"></a>

## 8. Caching & revalidation

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Revalidation and invalidation across the stack</summary>

### Revalidation and invalidation across the stack

**Definition:** Production “caching experience” usually spans **several layers**. Each layer caches different artifacts and exposes different **revalidation** or **invalidation** APIs. Bugs often come from fixing the wrong layer.

**How it works:**

| Layer              | What is cached                            | Revalidate / invalidate                                                                                         |
| ------------------ | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **HTTP / CDN**     | Response bodies, static assets            | `Cache-Control`, `ETag` / `304`, CDN purge — [HTTP caching](./6_web_api_http.md#http-caching)                   |
| **Next.js server** | Data Cache, Full Route Cache, RSC payload | `revalidateTag`, `revalidatePath`, `updateTag`, `router.refresh()` — below                                      |
| **SWR (client)**   | In-memory JSON per `key`                  | `mutate`, focus/reconnect, `refreshInterval` — [SWR](./4_react_ecosystem.md#swr-stale-while-revalidate)         |
| **TanStack Query** | Query cache per `queryKey`                | `invalidateQueries`, mutation `onSuccess` — [TanStack Query](./4_react_tanstack.md#working-with-tanstack-query) |

**When to use:**

- Interview answer: “Data looked stale after save” — state **which cache** served the old UI and **which API** you used to refresh it.
- Greenfield Next.js: prefer server caches + tags for shared reads; add SWR/TanStack only for client-only widgets.

**Pluses and minuses:**

| +                                     | −                                                 |
| ------------------------------------- | ------------------------------------------------- |
| Clear layering improves debuggability | Easy to confuse HTTP SWR directive vs SWR library |

**Thoughts:** Route Handlers can set [Vercel CDN cache headers](#vercel-cdn-cache-headers) for the HTTP layer while App Router caches handle RSC/HTML — they stack. See [Common pitfall: stale UI after mutation](#common-pitfall-stale-ui-after-mutation).

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching) · [MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

</details>

<details>
  <summary>Caching model overview</summary>

### Caching model overview

**Definition:**

Next.js maintains **four cache layers** between your data source and the browser. Understanding which layer serves a response — and which invalidation API clears it — is the #1 source of production bugs.

| Cache                   | What it stores                                       | Lifetime                               | Invalidation                                          |
| ----------------------- | ---------------------------------------------------- | -------------------------------------- | ----------------------------------------------------- |
| **Request memoization** | Deduped `fetch` / function results within one render | Single request                         | Automatic — new request                               |
| **Data Cache**          | Persistent `fetch` and `use cache` results           | Until revalidate TTL or on-demand      | `revalidateTag`, `revalidatePath`, `updateTag`        |
| **Full Route Cache**    | Rendered HTML + RSC Flight payload per route         | Build or until revalidated             | `revalidatePath`, `revalidateTag`, `router.refresh()` |
| **Router Cache**        | RSC payload on **client-side** navigations           | Session (soft) / ~30s default (static) | `router.refresh()`, full page reload                  |

**When to use:**

- Default static `fetch` — marketing pages, public catalogs
- `cache: 'no-store'` or `connection()` — dashboards, per-user data
- `next: { revalidate: N }` — ISR content
- `use cache` + `cacheTag` — explicit cache boundaries in Next.js 16

**When not to use:**

- Assuming `revalidatePath` clears client Router Cache instantly — pair with `router.refresh()` in Client Components when needed
- In-process `Map` caches on serverless — use `'use cache: remote'` or external store

**Pluses and minuses:**

| +                                              | −                                                      |
| ---------------------------------------------- | ------------------------------------------------------ |
| Massive perf wins for read-heavy apps          | Stale UI after mutations if you forget invalidation    |
| Request memoization prevents duplicate DB hits | Router Cache can show old RSC payload after navigation |
| CDN-friendly static shells with PPR            | Mental model is harder than "always fresh SSR"         |

**Examples:**

```
Browser request
  → Full Route Cache hit? → return cached HTML/RSC
  → miss → render Server Components
      → Request memoization (same fetch twice = one call)
      → Data Cache hit? → return cached data
      → miss → run fetch / use cache → store in Data Cache
  → store in Full Route Cache (if static)
Client navigation (Link)
  → Router Cache hit? → instant RSC payload
  → miss → server fetch → update Router Cache
```

```tsx
"use server";

import { revalidateTag, revalidatePath } from "next/cache";

export async function updateProduct(id: string, data: FormData) {
  await db.product.update({
    where: { id },
    data: {
      /* ... */
    },
  });
  revalidateTag(`product-${id}`, "max"); // Data Cache
  revalidatePath("/products"); // Full Route Cache
}
```

**Thoughts:**

- **Static by default** in App Router — former Pages Router SSR pages may surprise you with cached output.
- `cookies()` / `headers()` / `searchParams` opt into dynamic rendering — bypasses static Full Route Cache unless PPR shell is used.
- `unstable_noStore()` / `connection()` — escape hatches for fully dynamic data reads.
- See [Common pitfall: stale UI after mutation](#common-pitfall-stale-ui-after-mutation) and [Vercel CDN cache headers](#vercel-cdn-cache-headers).

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching)

</details>

<details>
  <summary>use cache directive</summary>

### use cache directive

**Definition:**

Next.js 16 directive marking a function or component body as cacheable. Enables Cache Components and PPR.

| Variant                | Scope                                              |
| ---------------------- | -------------------------------------------------- |
| `'use cache'`          | Default component/function cache                   |
| `'use cache: private'` | Per-user cache (uses cookies/headers)              |
| `'use cache: remote'`  | Runtime cache — shared across serverless instances |

**Examples:**

```tsx
import { cacheLife, cacheTag } from "next/cache";

async function getProducts() {
  "use cache: remote";
  cacheTag("products-list");
  cacheLife({ expire: 3600 });

  const res = await fetch("https://api.example.com/products");
  return res.json();
}
```

```ts
// next.config.ts
const nextConfig = { cacheComponents: true };
export default nextConfig;
```

**Sources:** [nextjs.org/docs/app/api-reference/directives/use-cache](https://nextjs.org/docs/app/api-reference/directives/use-cache)

</details>

<details>
  <summary>revalidateTag</summary>

### revalidateTag

**Definition:** On-demand invalidation by tag. Next.js 16 requires a **profile** as second argument.

**Examples:**

```tsx
"use server";

import { revalidateTag } from "next/cache";

export async function updateArticle(articleId: string) {
  await db.article.update({ where: { id: articleId }, data: {} });
  revalidateTag(`article-${articleId}`, "max");
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag)

</details>

<details>
  <summary>revalidatePath</summary>

### revalidatePath

**Definition:** Purges the Full Route Cache for a specific path.

**Examples:**

```tsx
"use server";

import { revalidatePath } from "next/cache";

export async function createPost(formData: FormData) {
  await db.post.create({ data: { title: formData.get("title") as string } });
  revalidatePath("/blog");
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/revalidatePath](https://nextjs.org/docs/app/api-reference/functions/revalidatePath)

</details>

<details>
  <summary>updateTag</summary>

### updateTag

**Definition:** Immediately expires a cache tag — read-your-writes semantics.

**Examples:**

```tsx
"use server";

import { updateTag } from "next/cache";

export async function updateProfile(userId: string, data: ProfileData) {
  await db.user.update({ where: { id: userId }, data });
  updateTag(`user-${userId}`);
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/updateTag](https://nextjs.org/docs/app/api-reference/functions/updateTag)

</details>

<details>
  <summary>router.refresh()</summary>

### router.refresh()

**Definition:** Client-side API that re-fetches Server Components for the current route without full page reload.

**Pluses and minuses:**

| +                      | −                                                   |
| ---------------------- | --------------------------------------------------- |
| Preserves client state | Does **not** invalidate server Data Cache by itself |

**Examples:**

```tsx
"use client";

import { useRouter } from "next/navigation";

export function RefreshButton() {
  const router = useRouter();
  return <button onClick={() => router.refresh()}>Refresh</button>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/use-router](https://nextjs.org/docs/app/api-reference/functions/use-router)

</details>

<details>
  <summary>unstable_noStore</summary>

### unstable_noStore

**Definition:** Opts a Server Component or function out of static caching and prerendering — forces dynamic execution every request.

**Pluses and minuses:**

| +                                             | −                                                 |
| --------------------------------------------- | ------------------------------------------------- |
| Simple escape hatch for dynamic data          | Disables static optimization for that tree        |
| Works with direct DB calls (not just `fetch`) | Prefer explicit `cache: 'no-store'` when possible |

**Examples:**

```tsx
import { unstable_noStore as noStore } from "next/cache";

export default async function LiveClock() {
  noStore();
  const now = await db.query("SELECT NOW() as time");
  return <time>{now}</time>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/unstable_noStore](https://nextjs.org/docs/app/api-reference/functions/unstable_noStore)

</details>

<details>
  <summary>Common pitfall: stale UI after mutation</summary>

### Common pitfall: stale UI after mutation

**Definition:**

Calling only `router.refresh()` without `revalidatePath` / `revalidateTag` / `updateTag` often leaves cached data stale.

| Symptom                          | Fix                                                                            |
| -------------------------------- | ------------------------------------------------------------------------------ |
| List doesn't update after create | `revalidatePath('/items')` or `revalidateTag('items', 'max')` in Server Action |
| User sees old profile after edit | `updateTag('user-${id}')`                                                      |
| CDN serves old HTML              | Check `Cache-Control` + `revalidatePath`                                       |

**Thoughts:** [Caching discussion #54075](https://github.com/vercel/next.js/discussions/54075). [Pre-rendering & hydration](https://www.freecodecamp.org/news/what-are-pre-rendering-and-hydration-in-web-dev/).

**Sources:** [nextjs.org/docs/app/guides/caching](https://nextjs.org/docs/app/guides/caching)

</details>

<details>
  <summary>Vercel CDN cache headers</summary>

### Vercel CDN cache headers

**Definition:** Per-response cache headers controlling browser, CDN, and Vercel edge cache layers independently.

**Examples:**

```ts
export async function GET() {
  return new Response("Cached response", {
    headers: {
      "Cache-Control": "max-age=10",
      "CDN-Cache-Control": "max-age=60",
      "Vercel-CDN-Cache-Control": "max-age=3600",
    },
  });
}
```

**Sources:** [vercel.com/docs/headers/cache-control-headers](https://vercel.com/docs/headers/cache-control-headers)

</details>

<details>
  <summary>Deprecated patterns (Next.js 16)</summary>

### Deprecated patterns (Next.js 16)

**Definition:**

APIs removed or changed in Next.js 16 and their recommended replacements.

| Deprecated                      | Replacement                              |
| ------------------------------- | ---------------------------------------- |
| `unstable_cache()`              | `'use cache'` + `cacheTag` / `cacheLife` |
| `experimental.ppr`              | `cacheComponents: true`                  |
| `revalidateTag(tag)` single arg | `revalidateTag(tag, 'max')`              |
| `cacheHandler` (singular)       | `cacheHandlers` (plural)                 |

---

**Sources:** [nextjs.org/docs/app/guides/upgrading/version-16](https://nextjs.org/docs/app/guides/upgrading/version-16)

</details>

<a name="9-server-actions-vs-route-handlers"></a>

## 9. Server Actions vs Route Handlers

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Handling mutations in Next.js</summary>

<a name="handling-mutations-in-nextjs"></a>

### Handling mutations in Next.js

**Definition:** A repeatable way to **create, update, or delete** data in the App Router: pick the right surface ([Server Actions](#server-actions) vs [Route Handlers](#route-handlers)), run the same server steps on every write, then **refresh caches** so lists and detail pages show new data.

**How it works:**

| Step                  | What to do                                                                                                                             |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Choose surface** | Next.js UI form/button → **Server Action** (default). Webhook, mobile app, or public REST URL → **Route Handler**.                     |
| **2. Auth**           | Inside the action/handler: session + permission check (not layout-only).                                                               |
| **3. Validate**       | Parse `FormData` or JSON; return field errors (`useActionState`) or `4xx` (handlers).                                                  |
| **4. Write**          | One call: `lib/db`, ORM, or single `fetch(API_URL)` — avoid long chains.                                                               |
| **5. Revalidate**     | `revalidatePath` / `revalidateTag` / `updateTag` for pages the UI reads ([stale UI pitfall](#common-pitfall-stale-ui-after-mutation)). |
| **6. Respond**        | Return serializable state, `redirect()`, or `Response.json` — details in **Examples**.                                                 |

| Who calls the write?                        | Use                                                             |
| ------------------------------------------- | --------------------------------------------------------------- |
| Your `<form>` / button in this Next app     | [Server Actions](#server-actions)                               |
| Stripe, GitHub, partner `POST` to a URL     | [Webhooks in Route Handlers](#webhooks-in-route-handlers)       |
| Browser `fetch` after hydration (dashboard) | [BFF Route Handler](#bff-route-handler) or action               |
| Server Component on first paint             | **No mutation** — read in render; write only via action/handler |

**When to use:** Any feature that saves settings, creates a row, deletes a comment, or syncs cart state.

**When not to use:** Loading data for SSR ([Server Component fetching](#server-component-fetching)); fire-and-forget jobs (use a queue).

**Pluses and minuses:**

| +                                      | −                                             |
| -------------------------------------- | --------------------------------------------- |
| One checklist for every feature team   | Easy to skip step 5 and ship stale UI         |
| Actions + revalidate fit forms and RSC | Handlers need your own CSRF for browser POSTs |

**Examples:**

```tsx
// File: app/actions/todos.ts — typical Server Action workflow (steps 2–6)
"use server";

import { revalidatePath } from "next/cache";
import { auth } from "@/lib/auth";
import { z } from "zod";

const schema = z.object({ title: z.string().min(1).max(200) });

export async function createTodo(
  _prev: { error?: string },
  formData: FormData,
) {
  const session = await auth();
  if (!session?.user) throw new Error("Unauthorized");

  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Invalid title" };

  await db.todo.create({
    data: { title: parsed.data.title, userId: session.user.id },
  });

  revalidatePath("/todos");
  return { error: undefined };
}
```

```tsx
// File: app/todos/TodoForm.tsx — wire action to UI ("use client" + useActionState)
"use client";

import { useActionState } from "react";
import { createTodo } from "@/app/actions/todos";

export function TodoForm() {
  const [state, action, pending] = useActionState(createTodo, {
    error: undefined,
  });

  return (
    <form action={action}>
      <input name="title" required />
      {state.error && <p role="alert">{state.error}</p>}
      <button disabled={pending}>Add</button>
    </form>
  );
}
```

```tsx
// File: app/api/todos/route.ts  →  URL: POST /api/todos
// Use when a non-Next client needs HTTP JSON (mobile, partner) — same steps 2–4, return Response
import { NextRequest } from "next/server";
import { auth } from "@/lib/auth";

export async function POST(request: NextRequest) {
  const session = await auth();
  if (!session)
    return Response.json({ error: "Unauthorized" }, { status: 401 });

  const body = await request.json();
  const title = String(body.title ?? "").trim();
  if (!title) return Response.json({ error: "Invalid title" }, { status: 400 });

  await db.todo.create({ data: { title, userId: session.user.id } });
  return Response.json({ ok: true }, { status: 201 });
}
```

```tsx
// File: app/actions/orders.ts — separate backend: one server fetch inside the action
"use server";

import { revalidatePath } from "next/cache";
import { auth } from "@/lib/auth";

export async function cancelOrder(formData: FormData) {
  const session = await auth();
  if (!session) throw new Error("Unauthorized");

  const orderId = String(formData.get("orderId"));
  const res = await fetch(`${process.env.API_URL}/orders/${orderId}/cancel`, {
    method: "POST",
    headers: { Authorization: `Bearer ${process.env.API_SERVICE_TOKEN}` },
  });
  if (!res.ok) throw new Error("Cancel failed");

  revalidatePath("/orders");
  revalidatePath(`/orders/${orderId}`);
}
```

**Thoughts:** Default path for product UI: **action + `revalidatePath`**. Compare surfaces: [Comparison table](#comparison-table). Reads stay in RSC — [Choosing a data fetch path](#choosing-a-data-fetch-path).

**Sources:**

- [nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)
- [nextjs.org/docs/app/getting-started/mutating-data](https://nextjs.org/docs/app/getting-started/mutating-data)

</details>

<details>
  <summary>Server Actions</summary>

<a name="server-actions"></a>

### Server Actions

**Definition:** An `async` function marked with `"use server"` that **mutates data on the server** (create, update, delete). It can use the DB, secrets, and `cookies()` — that code never ships to the browser. Wiring, transport, and file patterns are in **Examples** (comments).

**Use Server Actions for:** saving forms, deleting a row, toggling state — **writes** from your Next.js UI. **Do not use them for:** loading a product list or article body ([Server Component fetching](#server-component-fetching)); public APIs for mobile apps or webhooks ([Route Handlers](#route-handlers)).

**Security (important):** Anyone who can load your site can **try** to call an exported action (like calling a POST API). Check “is this user logged in?” and “may they do this?” **inside every action**, not only in a layout or middleware. Details: [CSRF with Server Actions](#csrf-with-server-actions) · [Server Functions](./4_react_rsc.md#server-functions).

**How it works:**

| Placement  | Pattern                                                 |
| ---------- | ------------------------------------------------------- |
| **Module** | `'use server'` file — see first **Example**             |
| **Inline** | Action inside Server Component — see second **Example** |

| Wire protocol | Behavior                                                                                                                                                                           |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Method**    | `POST` to the page route that registered the action (or the current segment during navigation)                                                                                     |
| **Headers**   | `Next-Action` carries the opaque action ID; browser sends cookies for same-origin session auth                                                                                     |
| **Body**      | `multipart/form-data` for `<form>` submits; serialized args for programmatic calls — default cap **1 MB** ([`serverActions.bodySizeLimit`](./4_nextjs_platform.md#next-config-ts)) |

1. Client calls `action` on `<form>`, `formAction` on a button, or imports the reference and runs it inside `startTransition` / `useActionState` — see [formAction and programmatic invocation](#formaction-and-programmatic-invocation).
2. Next.js POSTs with the action ID + **serializable** arguments — rules in [Serialization and return values](#serialization-and-return-values).
3. The function runs **only on the server** (DB, secrets, `cookies()` / `headers()` — **await** them in Next.js 15+) — [Cookies and headers in Server Actions](#cookies-and-headers-in-server-actions).
4. After a successful write, invalidate caches the UI depends on (`revalidatePath`, `revalidateTag`, `updateTag`) — see [Common pitfall: stale UI after mutation](#common-pitfall-stale-ui-after-mutation).
5. Optional `redirect()` navigates after mutate; it **throws** — call it **outside** `try/catch` and typically **after** revalidation.
6. Extra IDs from the UI (e.g. `userId`) — prefer `action.bind(null, id)` over hidden inputs when possible — [Server Action arguments (bind)](#server-action-arguments-bind).

React-level model (directives, serialization): [Server Functions](./4_react_rsc.md#server-functions) · ['use server'](./4_react_rsc.md#use-server).

**When to use:**

- Form submits and in-app **mutations** (create/update/delete) from Server or Client Components.
- Progressive enhancement: `<form action={action}>` works **without** client JS.
- Colocated writes when the only consumer is your Next app — not mobile apps or partner webhooks ([Route Handlers](#route-handlers)).

**When not to use:**

- **Reads** / list/detail data — use `async` Server Components or `fetch` in RSC ([Server Component fetching](#server-component-fetching)).
- Public HTTP APIs, webhooks, arbitrary verbs — Route Handlers.
- Long-running background jobs — queue + worker; actions should stay request-scoped.

**Pluses and minuses:**

| +                                              | −                                                                                                              |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Colocated with UI; works from `<form action>`  | Not a documented REST contract for third parties                                                               |
| Progressive enhancement (no JS required)       | POST-only transport; not cacheable like GET                                                                    |
| Type-safe imports from Client Components       | Every argument must be validated server-side (never trust `FormData`)                                          |
| Built-in same-origin CSRF posture              | Cross-origin calls need `serverActions.allowedOrigins` — [CSRF with Server Actions](#csrf-with-server-actions) |
| Pairs with `useActionState` for errors/pending | Easy to forget `revalidatePath` / `revalidateTag` after write                                                  |

#### Common pitfalls

- Treating actions as a **query** API — they are queued and sequential; use RSC for reads.
- `as string` on `formData.get` without length/format checks — use Zod or similar in the action.
- Only `router.refresh()` after mutate — often insufficient; revalidate server caches explicitly.
- `redirect()` inside `catch` or before revalidation — navigation aborts the flow; revalidate first.
- Mutations in Server Component **render** — only in actions or Route Handlers.

**Examples:**

```tsx
// File: app/actions.ts — module pattern ('use server' at top; all exports are actions)
// Called from: import { createTodo } → <form action={createTodo}> (Server or Client Component)
// Transport: POST + opaque action id (not a REST path like /api/todos)
// Arg: FormData — <input name="title"> → formData.get("title")
"use server";

import { revalidatePath } from "next/cache";
import { redirect } from "next/navigation";
import { auth } from "@/lib/auth";

export async function createTodo(formData: FormData) {
  const session = await auth();
  if (!session?.user) throw new Error("Unauthorized");

  const title = String(formData.get("title") ?? "").trim();
  if (!title) return { error: "Title required" };

  const todo = await db.todo.create({
    data: { title, userId: session.user.id },
  });

  revalidatePath("/todos");
  redirect(`/todos/${todo.id}`);
}
```

```tsx
// File: app/todos/new/page.tsx — inline pattern (Server Component; no "use client")
// Scoped to this page only; <form action={create}> works without JavaScript
export default function NewTodoPage() {
  async function create(formData: FormData) {
    "use server";
    const title = String(formData.get("title"));
    await db.todo.create({ data: { title } });
    revalidatePath("/todos");
  }

  return (
    <form action={create}>
      <input name="title" required />
      <button type="submit">Add</button>
    </form>
  );
}
```

```tsx
// File: app/actions.ts — useActionState action signature: (prevState, formData) => nextState
export async function createTodoWithState(
  _prev: { error?: string },
  formData: FormData,
) {
  const title = String(formData.get("title") ?? "").trim();
  if (!title) return { error: "Title required" };
  await db.todo.create({ data: { title } });
  revalidatePath("/todos");
  return { error: undefined };
}
```

```tsx
// File: app/todos/TodoForm.tsx — "use client"; pending + field errors from returned state
"use client";

import { useActionState } from "react";
import { createTodoWithState } from "@/app/actions";

const initialState = { error: undefined as string | undefined };

export function TodoForm() {
  const [state, formAction, pending] = useActionState(
    createTodoWithState,
    initialState,
  );

  return (
    <form action={formAction}>
      <input name="title" required />
      {state?.error && <p role="alert">{state.error}</p>}
      <button type="submit" disabled={pending}>
        Save
      </button>
    </form>
  );
}
```

**Thoughts:** Keep actions thin — auth, validate, call `lib/db` or `fetch(API_URL)` once ([Colocated backend](#colocated-backend-in-nextjs) · [Separate backend](#fetching-from-a-separate-backend)). Return field errors as serializable state for `useActionState`; throw or `redirect` for hard failures. Compare invocation models: [Comparison table](#comparison-table). Pending UI without prop drilling: [useFormStatus - 19](./4_react_hooks.md#useformstatus-19).

**Sources:**

- [nextjs.org/docs/app/getting-started/mutating-data](https://nextjs.org/docs/app/getting-started/mutating-data)
- [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)
- [nextjs.org/docs/app/guides/server-actions](https://nextjs.org/docs/app/guides/server-actions)

</details>

<details>
  <summary>Server Action arguments (bind)</summary>

<a name="server-action-arguments-bind"></a>

### Server Action arguments (bind)

**Definition:** Pass **extra arguments** to a Server Action beyond `FormData` by creating a bound server reference with `Function.prototype.bind`.

**How it works:**

- Signature stays `(id, formData)` or `(id, prevState, formData)` when paired with `useActionState`.
- `updateUser.bind(null, userId)` fixes the first argument; the form still supplies fields via `FormData`.
- Works from Client or Server Components; `<form action={boundAction}>` keeps progressive enhancement.
- **Validate** bound IDs on the server (format, ownership) — treat them like any untrusted input.

**When to use:**

- One action module, many rows (`deleteTodo.bind(null, todo.id)` per list item).
- Passing enum-like constants that are not user-editable fields.

**When not to use:**

- Large or sensitive blobs — bound args travel with the action reference; prefer server-side lookup by ID from session.
- Secrets — never bind API keys; read from env on the server.

**Pluses and minuses:**

| +                                                  | −                                                      |
| -------------------------------------------------- | ------------------------------------------------------ |
| Avoids tamper-prone hidden `<input>` for IDs       | Bound values must stay serializable                    |
| Same pattern for `formAction` and `useActionState` | Easy to forget server-side authZ check on the bound ID |

**Examples:**

```tsx
// File: app/actions.ts — bind fills 1st arg (userId); form submit fills FormData as 2nd
"use server";

export async function updateUser(userId: string, formData: FormData) {
  const session = await getSession();
  if (!session || session.userId !== userId) throw new Error("Forbidden");

  const name = String(formData.get("name") ?? "");
  await db.user.update({ where: { id: userId }, data: { name } });
  revalidatePath(`/users/${userId}`);
}
```

```tsx
// File: app/users/[id]/UserProfile.tsx — "use client"
// updateUser.bind(null, userId) — still validate userId against session in the action
"use client";

import { updateUser } from "@/app/actions";

export function UserProfile({ userId }: { userId: string }) {
  const updateUserWithId = updateUser.bind(null, userId);

  return (
    <form action={updateUserWithId}>
      <input name="name" />
      <button type="submit">Save</button>
    </form>
  );
}
```

**Thoughts:** Hidden fields are fine for CSRF-safe same-origin forms, but **authorization** must not depend on a client-supplied `userId` without matching the session.

**Sources:** [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)

</details>

<details>
  <summary>Closures and encryption</summary>

<a name="closures-and-encryption"></a>

### Closures and encryption

**Definition:** **Inline** Server Actions defined inside a Server Component **close over** variables from the render pass (e.g. a version string, feature flag, or draft ID snapshot).

**How it works:**

- Captured variables are sent to the client as part of the server reference payload and posted back when the action runs.
- Next.js **encrypts** closed-over values so casual inspection of the client bundle does not expose raw server data.
- Encryption keys are **per-build** — actions from an old deployment cannot be replayed against a new build after redeploy.
- Do **not** close over secrets (DB URLs, private keys); read those from `process.env` inside the action body instead.

**When to use:**

- Optimistic concurrency: snapshot `publishVersion` at render, re-check in `publish()` before writing.
- Feature gating snapshot when the action must match what the user saw when the form rendered.

**Pluses and minuses:**

| +                                       | −                                                     |
| --------------------------------------- | ----------------------------------------------------- |
| No extra round trip to re-fetch context | Payload size grows with captured data                 |
| Encrypted on the wire                   | Still not a substitute for authZ — validate on invoke |

**Examples:**

```tsx
// File: app/editor/page.tsx — Server Component
// publishVersion is closed over at render; Next.js encrypts it in the action payload
export default async function EditorPage() {
  const publishVersion = await getLatestVersion();

  async function publish() {
    "use server";
    if (publishVersion !== (await getLatestVersion())) {
      throw new Error("Version changed — refresh and try again");
    }
    await saveDraft();
    revalidatePath("/editor");
  }

  return (
    <form>
      <button formAction={publish}>Publish</button>
    </form>
  );
}
```

**Thoughts:** Prefer loading authoritative IDs from DB inside the action when tampering matters; closures are for **snapshots**, not trust boundaries.

**Sources:** [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)

</details>

<details>
  <summary>Serialization and return values</summary>

<a name="serialization-and-return-values"></a>

### Serialization and return values

**Definition:** Server Actions accept and return only **React-serializable** values across the server boundary.

**How it works:**

| Direction  | Allowed (typical)                                                                 | Avoid                                                                          |
| ---------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Args**   | `FormData`, strings, numbers, booleans, plain objects/arrays, `null`, `undefined` | Functions, symbols, most class instances, `Date` (use ISO string), `Map`/`Set` |
| **Return** | Same as args; use for field-level errors with `useActionState`                    | Throwing for **expected** validation errors — return `{ error: "…" }` instead  |

- **Expected errors** (validation, business rules) → return serializable state; display via `useActionState` `state`.
- **Unexpected errors** → `throw` or let the nearest `error.tsx` handle; use `unauthorized()` / `forbidden()` for auth failures when `authInterrupts` is enabled.
- Programmatic calls from event handlers should wrap in `startTransition` to avoid blocking the UI.

**When to use:**

- `useActionState` when the form needs inline error text and pending state without full-page navigation.
- `redirect()` / `throw` when the outcome is a different page or hard failure.

**Pluses and minuses:**

| +                                          | −                                                 |
| ------------------------------------------ | ------------------------------------------------- |
| Typed return shapes for form UX            | Easy to accidentally return non-serializable data |
| Clear split: expected vs unexpected errors | `redirect()` throws — do not catch as validation  |

**Examples:**

```tsx
// File: app/actions.ts — return { message } for validation; unauthorized() for auth failure
"use server";

import { unauthorized } from "next/navigation";

export async function updateProfile(
  _prev: { message?: string },
  formData: FormData,
) {
  const session = await verifySession();
  if (!session) unauthorized();

  const title = String(formData.get("title") ?? "").trim();
  if (!title) return { message: "Title is required" };

  await db.post.create({ data: { title, userId: session.userId } });
  revalidatePath("/posts");
  return { message: "" };
}
```

**Thoughts:** Model mutations like API handlers: validate → authorize → write → revalidate → return or redirect.

**Sources:**

- [nextjs.org/docs/app/getting-started/10-error-handling](https://nextjs.org/docs/app/getting-started/error-handling)
- [nextjs.org/docs/app/api-reference/functions/unauthorized](https://nextjs.org/docs/app/api-reference/functions/unauthorized)

</details>

<details>
  <summary>Cookies and headers in Server Actions</summary>

<a name="cookies-and-headers-in-server-actions"></a>

### Cookies and headers in Server Actions

**Definition:** Server Actions run in a **request context** and may read or mutate **cookies** and read **headers** via `next/headers`.

**How it works:**

- `const cookieStore = await cookies()` in Next.js 15+ (async).
- `cookieStore.set` / `delete` triggers a **server re-render** of the current route tree so UI reflects cookie changes (e.g. locale, cart id).
- `headers()` exposes incoming request headers (user-agent, custom proxy headers) — read-only for inspection; prefer `cookies()` for session tokens your app sets.
- Setting cookies in an action does **not** replace auth checks — still verify session inside the action.

**When to use:**

- Login/logout mutations that set session cookies.
- Preference toggles persisted in cookies without a separate Route Handler.

**Pluses and minuses:**

| +                                   | −                                                          |
| ----------------------------------- | ---------------------------------------------------------- |
| One mutation path for form + cookie | Cookie mutations force RSC refresh — plan UX               |
| Same APIs as Route Handlers         | `headers()` is dynamic — opts route into dynamic rendering |

**Examples:**

```tsx
// File: app/actions.ts — <select name="locale"> in <form action={setLocale}>
// cookies().set → server re-renders layout so UI picks up new locale
"use server";

import { cookies } from "next/headers";
import { revalidatePath } from "next/cache";

export async function setLocale(formData: FormData) {
  const locale = String(formData.get("locale") ?? "en");
  const cookieStore = await cookies();
  cookieStore.set("locale", locale, { path: "/", maxAge: 60 * 60 * 24 * 365 });
  revalidatePath("/", "layout");
}
```

**Thoughts:** For auth libraries (Better Auth, NextAuth adapters), use their server helpers inside actions — see [auth](./4_nextjs_platform.md#rsc-security).

**Sources:** [nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)

</details>

<details>
  <summary>formAction and programmatic invocation</summary>

<a name="formaction-and-programmatic-invocation"></a>

### formAction and programmatic invocation

**Definition:** Invoke Server Actions from **forms** (`action`, `formAction`) or **directly** from client event handlers.

**How it works:**

| Pattern             | API                                                                 | Progressive enhancement |
| ------------------- | ------------------------------------------------------------------- | ----------------------- |
| Whole form submit   | `<form action={action}>`                                            | Yes — works without JS  |
| Per-button submit   | `<button formAction={saveDraft}>Draft</button>` inside one `<form>` | Yes                     |
| Client-only button  | `<button formAction={createPost}>` in `"use client"` tree           | Yes if rendered in form |
| Click / custom flow | `startTransition(() => action(arg))`                                | No — requires JS        |

- Multiple actions in one form: different buttons with different `formAction` values (publish vs save draft).
- Import actions only into Client Components from a `"use server"` module — do not define secrets in the client file.

**When to use:**

- `formAction` when one form has multiple mutation types.
- Programmatic calls for delete buttons, toggles, or wizard steps that are not native `<form>` submits.

**Pluses and minuses:**

| +                                     | −                                                                    |
| ------------------------------------- | -------------------------------------------------------------------- |
| Native HTML multi-submit              | Programmatic path skips form validation attributes unless duplicated |
| `startTransition` keeps UI responsive | Must handle pending/error state manually without `useActionState`    |

**Examples:**

```tsx
// File: components/DeleteButton.tsx — "use client"
// Programmatic call (no <form>); requires JS; import deleteTodo from app/actions.ts
"use client";

import { startTransition } from "react";
import { deleteTodo } from "@/app/actions";

export function DeleteButton({ id }: { id: string }) {
  return (
    <button
      type="button"
      onClick={() => {
        startTransition(() => {
          void deleteTodo(id);
        });
      }}
    >
      Delete
    </button>
  );
}
```

```tsx
// File: app/posts/edit/page.tsx — Server Component (publish + saveDraft are Server Actions)
// Default submit → publish; second button → saveDraft via formAction
<form action={publish}>
  <textarea name="body" required />
  <button type="submit">Publish</button>
  <button type="submit" formAction={saveDraft}>
    Save draft
  </button>
</form>
```

**Thoughts:** Prefer `<form>` + `useActionState` for accessible error text; use programmatic calls for small non-form interactions.

**Sources:**

- [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)
- [nextjs.org/docs/app/getting-started/updating-data](https://nextjs.org/docs/app/getting-started/updating-data)

</details>

<details>
  <summary>Optimistic UI with Server Actions</summary>

<a name="optimistic-ui-with-server-actions"></a>

### Optimistic UI with Server Actions

**Definition:** Show **immediate** UI feedback while a Server Action runs, then reconcile when the server responds.

**How it works:**

- `useOptimistic(baseState, reducer)` holds temporary state on the client.
- In a client `form action` wrapper: call `addOptimistic(...)` then `await serverAction(...)`.
- On failure, revert by re-fetching (`router.refresh()`), resetting optimistic state, or showing an error — optimistic state does not auto-roll back.
- Pair with `revalidatePath` inside the action so the next RSC payload matches the optimistic view.

**When to use:**

- Chat messages, likes, checklist toggles where latency is noticeable.
- Not a substitute for `useActionState` field errors — combine when needed.

**Pluses and minuses:**

| +                       | −                                            |
| ----------------------- | -------------------------------------------- |
| Snappy UX               | Must handle action failure explicitly        |
| Works with form actions | Duplicate logic if optimistic ≠ server shape |

**Examples:**

```tsx
// File: app/chat/Thread.tsx — "use client"
// Client form action: addOptimistic first, then await sendMessage() from ./actions.ts
"use client";

import { useOptimistic } from "react";
import { sendMessage } from "./actions";

type Message = { id: string; text: string };

export function Thread({ messages }: { messages: Message[] }) {
  const [optimistic, addOptimistic] = useOptimistic(
    messages,
    (state, text: string) => [...state, { id: `tmp-${state.length}`, text }],
  );

  async function onSubmit(formData: FormData) {
    const text = String(formData.get("text") ?? "");
    addOptimistic(text);
    await sendMessage(text);
  }

  return (
    <div>
      {optimistic.map((m) => (
        <p key={m.id}>{m.text}</p>
      ))}
      <form action={onSubmit}>
        <input name="text" />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

**Thoughts:** Keep optimistic items distinguishable (temp IDs) until the action returns persisted IDs via revalidation.

**Sources:** [nextjs.org/docs/app/guides/forms](https://nextjs.org/docs/app/guides/forms)

</details>

<details>
  <summary>Route Handlers</summary>

<a name="route-handlers"></a>

### Route Handlers

**Definition:** A **Route Handler** is a server file `route.ts` (or `route.js`) in the `app/` folder that answers **real HTTP requests** at a URL path — like a small API endpoint inside your Next.js app. You export functions named after HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`. Each function receives a standard **`Request`** (or `NextRequest`) and returns a **`Response`** (often `Response.json({ ... })`).

**Use Route Handlers when** something **outside** your React UI needs HTTP: Stripe/GitHub **webhooks**, a **mobile app**, a **browser on another domain** (with CORS), or a **Client Component** that polls `fetch("/api/metrics")` after load. **Do not use them** as the main way for **Server Components** to read your own database — call `lib/db` directly ([Colocated backend](#colocated-backend-in-nextjs)). For form submits from your own Next.js pages, prefer [Server Actions](#server-actions) unless you need a stable public URL.

**Security:** A Route Handler is a **public URL**. Verify API keys, webhook signatures, and user sessions **inside the handler**. Same-origin `fetch("/api/...")` from your app still needs auth checks — cookies can be sent, but anyone can call the URL with curl.

**How it works:**

| Piece       | Rule                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **File**    | `route.ts` only under `app/` (not `pages/api` — that is Pages Router)                                                                                  |
| **URL**     | Folder path under `app/` = URL path — see **Examples** comments                                                                                        |
| **Methods** | Export only the verbs you support; unknown methods → **405**                                                                                           |
| **Body**    | `await request.json()`, `request.text()`, or `request.formData()` — no Pages Router `bodyParser` config                                                |
| **Cookies** | `request.cookies` on `NextRequest`; set on the `Response` you return                                                                                   |
| **Config**  | Same segment options as pages: `dynamic`, `revalidate`, `runtime` (`nodejs` vs `edge`) — see [Caching GET Route Handlers](#caching-get-route-handlers) |

**When to use:**

- Webhooks and third-party callbacks ([Webhooks](#webhooks-in-route-handlers)).
- BFF endpoints for the browser (`/api/...`) that hide service tokens ([BFF Route Handler](#bff-route-handler)).
- Public REST/JSON for non-Next clients.

**When not to use:**

- Default **read** path for RSC pages — use Server Components + `fetch` / DB.
- In-app **form mutations** that only your UI needs — [Server Actions](#server-actions).

**Pluses and minuses:**

| +                                 | −                                          |
| --------------------------------- | ------------------------------------------ |
| Real HTTP — any client, any verb  | No `<form action>` progressive enhancement |
| Stable URL you can document       | You own CORS, CSRF, rate limits, auth      |
| Works with webhooks and mobile    | Extra hop if RSC fetches your own `/api`   |
| `GET` can be cached (with config) | Cannot share folder with `page.tsx`        |

**Examples:**

```tsx
// File: app/api/users/route.ts
// URL:  GET/POST … https://your-site.com/api/users
// Rule: this folder must NOT also contain page.tsx — segment is page OR route.ts, not both
export async function GET() {
  const users = await db.user.findMany();
  return Response.json(users);
}
```

```tsx
// File: app/api/health/route.ts  →  URL: /api/health
export async function GET() {
  return Response.json({ ok: true });
}
```

```tsx
// File: app/api/posts/[id]/route.ts
// URL:  GET https://your-site.com/api/posts/123  →  params.id === "123"
// Next.js 15+: params is a Promise — await it before use
export async function GET(
  _request: Request,
  { params }: { params: Promise<{ id: string }> },
) {
  const { id } = await params;
  const post = await db.post.findUnique({ where: { id } });
  if (!post) return Response.json({ error: "Not found" }, { status: 404 });
  return Response.json(post);
}
```

```tsx
// File: app/api/webhook/route.ts  →  URL: POST /api/webhook
export async function POST(request: Request) {
  const raw = await request.text();
  // verify signature on raw body, then process
  return new Response("ok", { status: 200 });
}
```

**Thoughts:** Cross-origin browser calls → [Handling CORS in Next.js](#handling-cors-in-nextjs). Same-origin `fetch("/api/...")` needs no CORS. Compare with [Server Actions](#server-actions) in the [Comparison table](#comparison-table).

**Sources:**

- [nextjs.org/docs/app/getting-started/route-handlers](https://nextjs.org/docs/app/getting-started/route-handlers)
- [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>BFF Route Handler</summary>

<a name="bff-route-handler"></a>

### BFF Route Handler

**Definition:** A Route Handler that sits **between the browser and your backend** — the UI calls **your** `/api/...` URL; the handler checks the user session, adds a **service token**, and forwards to another API or database.

**How it works:** Browser calls your `/api/...` URL; handler forwards to the real backend with server-only credentials — see **Examples** comments. Server Components usually skip this hop ([Fetching from a separate backend](#fetching-from-a-separate-backend)).

**When to use:**

- Dashboards that poll or mutate after hydration ([Client-side fetching](#client-side-fetching)).
- Hiding tokens from the client while still using browser `fetch`.

**Pluses and minuses:**

| +                          | −                                    |
| -------------------------- | ------------------------------------ |
| Clear HTTP contract for UI | Latency vs direct server `fetch`     |
| Central place for authZ    | Easy to duplicate logic with actions |

**Examples:**

```tsx
// File: app/api/orders/route.ts  →  URL: GET /api/orders
// Client: fetch("/api/orders") — same origin, session cookie in browser
// Server: fetch(API_URL/orders) with service token — token never in NEXT_PUBLIC_*
import { NextRequest } from "next/server";
import { getSession } from "@/lib/auth";

export async function GET(_request: NextRequest) {
  const session = await getSession();
  if (!session)
    return Response.json({ error: "Unauthorized" }, { status: 401 });

  const res = await fetch(`${process.env.API_URL}/orders`, {
    headers: { Authorization: `Bearer ${session.accessToken}` },
  });
  const data = await res.json();
  return Response.json(data, { status: res.status });
}
```

**Thoughts:** Same-origin BFF **avoids browser CORS** — prefer this when the only client is your Next app. If a partner SPA must call your API on another host, see [Handling CORS in Next.js](#handling-cors-in-nextjs). Cache short-lived GETs — [Vercel CDN cache headers](#vercel-cdn-cache-headers).

**Sources:** [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)

</details>

<details>
  <summary>Handling CORS in Next.js</summary>

<a name="handling-cors-in-nextjs"></a>

### Handling CORS in Next.js

**Definition:** **CORS** is a **browser** rule: JavaScript on `https://app.example.com` may only read responses from `https://api.example.com` if your Next.js API returns `Access-Control-*` headers (and answers **`OPTIONS`** preflights when needed). Server Components, Server Actions, and server-side `fetch` **do not** use CORS — only cross-origin **browser** `fetch` / XHR does.

**How it works:**

| Step                  | What to do                                                                                                                                             |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1. Need CORS?**     | If the caller is your Next app on the **same origin** (`fetch("/api/...")`) → **no CORS**. If another site or port calls your Route Handler → **yes**. |
| **2. Prefer no CORS** | [BFF Route Handler](#bff-route-handler): browser hits your domain; server calls the real API with secrets.                                             |
| **3. Pick layer**     | One public API → headers on that `route.ts`. Many `/api/*` routes → `next.config` `headers()` or `proxy.ts` allowlist.                                 |
| **4. Preflight**      | Non-simple cross-origin requests send **`OPTIONS` first** — export `OPTIONS` or handle in `proxy.ts`.                                                  |
| **5. Credentials**    | With cookies/`credentials: 'include'`: echo a **specific** allowed `Origin` (not `*`) and set `Access-Control-Allow-Credentials: true`.                |
| **6. Auth**           | CORS is **not** security — still validate tokens/session in the handler.                                                                               |

| Approach                      | When                                          |
| ----------------------------- | --------------------------------------------- |
| **Route Handler headers**     | One partner endpoint, custom policy per route |
| **`next.config` `headers()`** | Fixed allowlist for `/api/:path*`             |
| **`proxy.ts`**                | Dynamic origin check + shared preflight       |
| **BFF (same origin)**         | Your UI only — skip CORS                      |

**When to use:** Partner SPA, `localhost:3001` → `localhost:3000` dev, extension calling your API.

**When not to use:** Replacing auth; `Allow-Origin: *` with credentialed requests; CORS on routes your own pages already call same-origin.

**Pluses and minuses:**

| +                                        | −                                 |
| ---------------------------------------- | --------------------------------- |
| Route Handler — full control per URL     | Easy to forget `OPTIONS`          |
| `headers()` / `proxy` — DRY for `/api/*` | Misconfigured matcher breaks APIs |
| BFF — simplest for your own frontend     | Extra server hop                  |

**Examples:**

```ts
// File: app/api/public/health/route.ts  →  URL: GET /api/public/health
// Caller: https://partner.example (cross-origin) — attach same headers on GET and OPTIONS
const cors = {
  "Access-Control-Allow-Origin": "https://partner.example",
  "Access-Control-Allow-Methods": "GET, OPTIONS",
  "Access-Control-Allow-Headers": "Content-Type, Authorization",
};

export async function OPTIONS() {
  return new Response(null, { status: 204, headers: cors });
}

export async function GET() {
  return Response.json({ ok: true }, { headers: cors });
}
```

```ts
// File: next.config.ts — same CORS policy for all /api/* (static allowlist)
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  async headers() {
    return [
      {
        source: "/api/:path*",
        headers: [
          {
            key: "Access-Control-Allow-Origin",
            value: "https://partner.example",
          },
          {
            key: "Access-Control-Allow-Methods",
            value: "GET, POST, PUT, DELETE, OPTIONS",
          },
          {
            key: "Access-Control-Allow-Headers",
            value: "Content-Type, Authorization",
          },
        ],
      },
    ];
  },
};

export default nextConfig;
```

```ts
// File: proxy.ts — matcher: /api/:path* — dynamic Origin allowlist + OPTIONS preflight
import { NextRequest, NextResponse } from "next/server";

const allowedOrigins = ["https://acme.com", "http://localhost:3001"];

export function proxy(request: NextRequest) {
  const origin = request.headers.get("origin") ?? "";
  const allowed = allowedOrigins.includes(origin);

  if (request.method === "OPTIONS") {
    const headers: Record<string, string> = {
      "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE, OPTIONS",
      "Access-Control-Allow-Headers": "Content-Type, Authorization",
    };
    if (allowed) headers["Access-Control-Allow-Origin"] = origin;
    return NextResponse.json({}, { headers });
  }

  const response = NextResponse.next();
  if (allowed) response.headers.set("Access-Control-Allow-Origin", origin);
  return response;
}

export const config = { matcher: "/api/:path*" };
```

**Thoughts:** Deep dive (preflight, `Vary: Origin`, CDN): [CORS in Next.js](./4_nextjs_platform.md#cors-in-nextjs). Theory: [Web API — CORS](./6_web_api_security.md#cors). [Server Actions](#server-actions) use framework CSRF checks, not CORS, for same-origin forms.

**Sources:**

- [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)
- [nextjs.org/docs/app/api-reference/config/next-config-js/headers](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers)
- [nextjs.org/docs/app/guides/backend-for-frontend](https://nextjs.org/docs/app/guides/backend-for-frontend)

</details>

<details>
  <summary>Webhooks in Route Handlers</summary>

<a name="webhooks-in-route-handlers"></a>

### Webhooks in Route Handlers

**Definition:** An HTTP **POST** (sometimes GET) endpoint that **external services** call when an event happens — payment succeeded, PR merged, email delivered.

**How it works:** Stable public POST URL, raw body for signature verification, return 2xx quickly, dedupe event ids — see **Examples** comments.

**When to use:**

- Any vendor that documents “send webhook to your URL” — not [Server Actions](#server-actions) (they are not arbitrary public POST URLs for third parties).

**Pluses and minuses:**

| +                    | −                               |
| -------------------- | ------------------------------- |
| Standard integration | Must verify signatures yourself |
| Works on Vercel/host | Replay / duplicate events       |

**Examples:**

```tsx
// File: app/api/stripe/webhook/route.ts  →  URL: POST /api/stripe/webhook
// Caller: Stripe servers (not your React app) — configure this URL in Stripe dashboard
// Use request.text() before JSON.parse when verifying Stripe-Signature on raw bytes
export async function POST(request: Request) {
  try {
    const payload = await request.text();
    const sig = request.headers.get("stripe-signature");
    // stripe.webhooks.constructEvent(payload, sig, webhookSecret)
    return new Response("ok", { status: 200 });
  } catch (err) {
    return new Response("Webhook error", { status: 400 });
  }
}
```

**Thoughts:** Disable body size limits only if the platform allows; validate content-type. For browser-facing POSTs from your site, use CSRF protection — webhooks use **shared secrets**, not cookies.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>Caching GET Route Handlers</summary>

<a name="caching-get-route-handlers"></a>

### Caching GET Route Handlers

**Definition:** How **`GET`** Route Handlers interact with Next.js caching and CDN headers — separate from RSC `fetch` cache.

**How it works:**

- Recent Next.js versions treat **`GET` Route Handlers as dynamic by default** (not cached like old behavior) unless you opt in.
- Export segment config on the route file, e.g. `export const dynamic = 'force-static'` when the response is safe to cache at build time.
- Use `revalidate` export or `Cache-Control` on the `Response` for HTTP/CDN caching ([Vercel CDN cache headers](#vercel-cdn-cache-headers)).
- `POST`/`PUT` handlers are not statically cached — design mutations explicitly.

**When to use:**

- Public read-only JSON feeds (`GET /api/public/stats`) with known TTL.
- Not for personalized `GET` without checking auth and setting `private, no-store`.

**Pluses and minuses:**

| +                    | −                                                        |
| -------------------- | -------------------------------------------------------- |
| Edge/CDN friendly    | Stale data if TTL too long                               |
| Same config as pages | Easy to leak cached private data without `Cache-Control` |

**Examples:**

```tsx
// File: app/api/public/config/route.ts  →  URL: GET /api/public/config
// dynamic = 'force-static' — opt in to cached GET (dynamic by default in recent Next.js)
export const dynamic = "force-static";

export async function GET() {
  const data = await getPublicConfig();
  return Response.json(data);
}
```

**Thoughts:** Personalized APIs → `Cache-Control: private, no-store` and session checks on every request.

**Sources:**

- [nextjs.org/docs/app/guides/upgrading/version-15](https://nextjs.org/docs/app/guides/upgrading/version-15)
- [nextjs.org/docs/app/api-reference/file-conventions/route](https://nextjs.org/docs/app/api-reference/file-conventions/route)

</details>

<details>
  <summary>Comparison table</summary>

### Comparison table

**Definition:**

Decision matrix for choosing Server Actions vs Route Handlers based on invocation model and consumers.

|                | Server Actions        | Route Handlers                |
| -------------- | --------------------- | ----------------------------- |
| **Invocation** | RPC / `<form action>` | HTTP to URL                   |
| **Protocol**   | POST (wrapped)        | GET, POST, PUT, PATCH, DELETE |
| **Consumers**  | Next.js app only      | Any HTTP client               |
| **Use case**   | Mutations, forms      | Webhooks, public APIs         |

**Thoughts:** Route Handlers are **only** in `app/`. External systems use Route Handlers, not Server Actions.

**Sources:** [nextjs.org/docs/app/getting-started/route-handlers](https://nextjs.org/docs/app/getting-started/route-handlers)

</details>

<details>
  <summary>CSRF with Server Actions</summary>

<a name="csrf-with-server-actions"></a>

### CSRF with Server Actions

**Definition:**

Server Actions are invoked via **POST** with a server-generated action ID. Next.js includes **built-in CSRF protections** — Origin header validation and encrypted action IDs — so you do not manually add CSRF tokens for same-origin form posts.

| Protection       | How it works                                                    |
| ---------------- | --------------------------------------------------------------- |
| **Origin check** | Request `Origin` must match `Host` (or allowed deployment host) |
| **Action ID**    | Opaque encrypted ID binds POST to the correct server action     |
| **POST-only**    | Actions are not callable via GET — no img-tag CSRF              |

**How it works:**

- Public Route Handlers (`route.ts`) — implement CSRF tokens or signed requests
- Embedding your app in third-party iframes — review `allowedOrigins` in `next.config`
- Proxy/middleware-only auth — still validate in Server Action body (see [RSC security](./4_nextjs_platform.md#rsc-security))

**When to use:** Default for all in-app mutations via Server Actions and `<form action>`.

**Pluses and minuses:**

| +                                       | −                                                               |
| --------------------------------------- | --------------------------------------------------------------- |
| Zero boilerplate for same-origin forms  | Cross-origin Server Action calls need explicit `allowedOrigins` |
| Progressive enhancement safe by default | Route Handlers still need your own CSRF strategy                |
| Works with `<form action={...}>`        | Custom `fetch` to actions must include correct headers/body     |

**Examples:**

```ts
// File: next.config.ts — preview host may POST Server Actions from another origin
const nextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ["localhost:3000", "preview.example.com"],
    },
  },
};
```

```tsx
// File: app/actions.ts — CSRF handled by framework for same-origin forms; authZ is still yours
"use server";

import { revalidatePath } from "next/cache";
import { z } from "zod";

const schema = z.object({ title: z.string().min(1).max(200) });

export async function createTodo(formData: FormData) {
  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Invalid title" };

  const session = await getSession();
  if (!session) throw new Error("Unauthorized");

  await db.todo.create({
    data: { title: parsed.data.title, userId: session.userId },
  });
  revalidatePath("/todos");
}
```

**Thoughts:**

- Built-in CSRF does **not** replace **authorization** — always check user permissions inside the action.
- For Route Handlers accepting browser POSTs, use double-submit cookie, SameSite cookies, or framework CSRF middleware.
- See [Web API — CSRF](./6_web_api_security.md#csrf-cross-site-request-forgery) and [Special — CSRF vs AuthZ](./7_special_security.md#csrf-cross-site-request-forgery).

**Sources:** [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security)

</details>

<a name="10-file-based-routing"></a>
