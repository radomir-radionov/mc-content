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

| Question                                                     | If yes →          | Typical pattern                                                     |
| ------------------------------------------------------------ | ----------------- | ------------------------------------------------------------------- |
| Is the data owned by a **vendor** (CMS, Stripe, maps)?       | Remote HTTP API   | `fetch` or SDK in **Server Component** with env secrets             |
| Is the data in **your DB** in the same repo/deploy?          | Colocated backend | `lib/` + ORM in **Server Component** — no self-`fetch` to `/api`    |
| Is the data behind **another team’s service** you host?      | Separate backend  | Server `fetch(API_URL)`; browser uses **Route Handler** proxy       |
| Must the UI **update every few seconds** without navigation? | Client read       | `fetch('/api/...')`, SWR, or TanStack Query in **Client Component** |
| Is it a **form submit / mutation**?                          | Server write      | **Server Action** or `POST` Route Handler — not `useEffect` fetch   |

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
// lib/products.ts
import { db } from "@/lib/db";

export async function listProducts() {
  return db.product.findMany({ orderBy: { createdAt: "desc" } });
}

// app/products/page.tsx
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
// app/api/orders/route.ts — browser-safe proxy
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
  <summary>Server Actions</summary>

### Server Actions

**Definition:** Async functions marked with `"use server"` callable from Client Components or `<form action>`. Next.js serializes them as **POST** requests — an RPC bridge between browser and server.

**When to use:** Form submissions, mutations, internal app logic.

**Pluses and minuses:**

| +                         | −                                   |
| ------------------------- | ----------------------------------- |
| Colocated with UI         | Not a public HTTP API               |
| Progressive enhancement   | POST-only under the hood            |
| Type-safe with TypeScript | Must validate all input server-side |

**Examples:**

```tsx
// app/actions.ts
"use server";

import { revalidatePath } from "next/cache";

export async function createTodo(formData: FormData) {
  const title = formData.get("title") as string;
  await db.todo.create({ data: { title } });
  revalidatePath("/todos");
}
```

**Sources:** [nextjs.org/docs/app/getting-started/mutating-data](https://nextjs.org/docs/app/getting-started/mutating-data)

</details>

<details>
  <summary>Route Handlers</summary>

### Route Handlers

**Definition:** `route.ts` files exporting HTTP method functions. Standard Web Request/Response API.

**Pluses and minuses:**

| +                                 | −                                             |
| --------------------------------- | --------------------------------------------- |
| Real HTTP — webhooks, public APIs | No progressive enhancement                    |
| Any HTTP method                   | Cannot coexist with `page.tsx` in same folder |

**Examples:**

```tsx
// app/api/webhook/route.ts
import { NextRequest } from "next/server";

export async function POST(request: NextRequest) {
  const body = await request.json();
  return Response.json({ received: true });
}
```

**Sources:** [nextjs.org/docs/app/getting-started/route-handlers](https://nextjs.org/docs/app/getting-started/route-handlers)

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
// next.config.ts — allow preview domain to invoke actions
const nextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ["localhost:3000", "preview.example.com"],
    },
  },
};
```

```tsx
"use server";

import { revalidatePath } from "next/cache";
import { z } from "zod";

const schema = z.object({ title: z.string().min(1).max(200) });

export async function createTodo(formData: FormData) {
  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Invalid title" };

  const session = await getSession(); // always re-validate auth in action
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
