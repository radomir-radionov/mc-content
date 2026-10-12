# Next.js — Rendering

Rendering strategies, Server/Client boundaries, and streaming.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [1. Rendering strategies](#1-rendering-strategies)
- [2. Server & Client boundaries](#2-server-client-boundaries)
- [3. Streaming & Suspense](#3-streaming-suspense)

---

<a name="1-rendering-strategies"></a>

## 1. Rendering strategies

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Overview</summary>

### Overview

**Definition:**

Next.js supports multiple rendering modes per route. The App Router defaults to static where possible and opts into dynamic behavior when the route reads request-time data (`cookies`, `headers`, `searchParams`, uncached `fetch`).

| Strategy | When HTML is produced                 | Data freshness         | SEO              |
| -------- | ------------------------------------- | ---------------------- | ---------------- |
| **CSR**  | Browser (Client Components only)      | Live                   | Weak without SSR |
| **SSR**  | Server per request                    | Always fresh           | Strong           |
| **SSG**  | Build time                            | Static until rebuild   | Strong           |
| **ISR**  | Build + periodic revalidation         | Stale-while-revalidate | Strong           |
| **PPR**  | Static shell + streamed dynamic holes | Mixed per segment      | Strong           |

**Examples:**

```tsx
// SSG (default) — HTML at build; fetch uses force-cache unless overridden
// app/about/page.tsx
export default async function About() {
  const data = await fetch("https://api.example.com/about").then((r) =>
    r.json(),
  );
  return <main>{data.title}</main>;
}

// SSR — fresh HTML on every request (any one trigger opts the route in)
// app/dashboard/page.tsx
import { cookies } from "next/headers";

export const dynamic = "force-dynamic";

export default async function Dashboard() {
  const session = (await cookies()).get("session");
  return <h1>Hello, {session?.value ?? "guest"}</h1>;
}

// ISR — SSG + timed refresh (Next.js 16, cacheComponents: true)
// app/products/page.tsx
import { cacheLife, cacheTag } from "next/cache";

async function getProducts() {
  "use cache";
  cacheTag("products");
  cacheLife({ revalidate: 3600 });
  return fetch("https://api.example.com/products").then((r) => r.json());
}

export default async function ProductsPage() {
  const products = await getProducts();
  return (
    <ul>
      {products.map((p: { id: string; title: string }) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}

// CSR — browser-only subtree; no server HTML for this component
// app/counter.tsx
("use client");

import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}

// PPR — static shell + streamed dynamic holes (Next.js 16, cacheComponents: true)
// app/page.tsx
import { Suspense } from "react";
import { cookies } from "next/headers";

async function Greeting() {
  const name = (await cookies()).get("user")?.value;
  return <p>Hi, {name ?? "guest"}</p>;
}

export default function Page() {
  return (
    <section>
      <h1>Welcome</h1>
      <Suspense fallback={<p>Loading…</p>}>
        <Greeting />
      </Suspense>
    </section>
  );
}
```

**Thoughts:** See [4_react.md §8 — Server rendering & RSC](./4_react_server.md#8-server-rendering--rsc) for SPA vs SSR vs RSC. Taxonomy (MPA vs SPA, CSR/SSR/SSG/PWA): [6_web_api.md — Types of web apps](./6_web_api_apps.md#types-of-web-apps). Next.js implements these at the **route segment** level. Deep dives with tables and flows: [SSG](#static-generation-ssg) · [SSR](#server-side-rendering-ssr) · [ISR](#incremental-static-regeneration-isr) · [PPR](#partial-prerendering-ppr-cache-components) · [Combining strategies](#combining-rendering-strategies). Next.js 16 examples assume `cacheComponents: true`.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching)

</details>

<details>
  <summary>Static Generation (SSG)</summary>

### Static Generation (SSG)

**Definition:** Static Site Generation (SSG) pre-renders **HTML and the RSC payload** at **build time** (`next build`), not on each request. The same output is served to every visitor until the next build or an ISR revalidation window expires.

In the App Router, SSG is the **default** for any route segment that:

- Uses Server Components (no `"use client"` at the page root)
- Does **not** read request-time APIs — `cookies()`, `headers()`, `searchParams`, or `connection()`
- Uses `fetch` with the default cache (`force-cache`) or marks data with `"use cache"`

Next.js stores the result in the **Full Route Cache** (prerendered HTML + RSC payload per route). A CDN or static host can serve it with no Node server per page view. Pages Router equivalent: `getStaticProps` (+ `getStaticPaths` for dynamic routes). App Router equivalent: default cached `fetch` + optional `generateStaticParams()` for `[slug]` segments.

**How it works:**

| Stage   | What happens                                                               |
| ------- | -------------------------------------------------------------------------- |
| Build   | Server Components run; data is fetched; HTML + RSC payload are generated   |
| Deploy  | Cached route output ships with the build — CDN-ready                       |
| Request | Browser gets populated HTML immediately; Client Components hydrate on load |
| Refresh | Full rebuild, or switch to [ISR](#incremental-static-regeneration-isr)     |

A route **opts out** of SSG when it reads request-time data, uses `fetch(..., { cache: "no-store" })`, or sets `export const dynamic = "force-dynamic"`.

**When to use:** Marketing pages, blogs, docs, product catalogs — any content shared across users and tolerating build-time or ISR freshness.

**Pluses and minuses:**

| +                                        | −                                                      |
| ---------------------------------------- | ------------------------------------------------------ |
| Fastest CDN delivery, lowest server cost | Build time grows with page count                       |
| Predictable performance                  | Not suitable for per-user content without client fetch |

**Examples:**

Each piece has a different job — they work together, but none of them alone means “SSG”:

| Piece                    | Role at build (`next build`)                                             | Role after deploy                                |
| ------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------ |
| `generateStaticParams()` | **Which URLs** to render (`/blog/a`, `/blog/b`, …)                       | Unchanged slugs still served from cache          |
| `"use cache"`            | Marks `getPost` output as **storable** (HTML + data)                     | Reuses cached result instead of re-fetching      |
| `cacheLife("days")`      | Lifetime **long enough** to include in build output (not a dynamic hole) | Revalidates after ~1 day; `expire` after ~1 week |
| `cacheTag("posts")`      | Tags the cache entry                                                     | `revalidateTag("posts")` refreshes on publish    |

```tsx
// next.config.ts — step 0: enable Cache Components (Next.js 16+)
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  cacheComponents: true,
};

export default nextConfig;
```

```tsx
// app/blog/[slug]/page.tsx
import { cacheLife, cacheTag } from "next/cache";

type Post = { slug: string; title: string; body: string };

// Step 2–4: cacheable data fetch
async function getPost(slug: string): Promise<Post> {
  "use cache"; // output can be stored and reused

  cacheTag("posts"); // name for on-demand invalidation (step 5)
  cacheLife("days"); // revalidate ~1 day, expire ~1 week — long enough for build inclusion
  // cacheLife("seconds") would be TOO short → dynamic hole, fetched per request instead

  const res = await fetch(`https://api.example.com/posts/${slug}`);
  if (!res.ok) throw new Error("Post not found");
  return res.json();
}

// Step 1: tell build which /blog/[slug] pages to generate
export async function generateStaticParams() {
  const posts: Post[] = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return posts.map((post) => ({ slug: post.slug }));
  // → next build runs PostPage for each slug and stores HTML in Full Route Cache
}

export default async function PostPage({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const post = await getPost(slug); // cached at build; reused on later requests

  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </article>
  );
}
```

```tsx
// app/blog/actions.ts — step 5: refresh cache after CMS publish (does NOT cause build)
"use server";

import { revalidateTag } from "next/cache";

export async function publishPost() {
  await saveToCms();
  revalidateTag("posts", "max"); // bust tagged cache; next visit regenerates getPost
}
```

**Build flow:**

```text
next build
  → generateStaticParams() returns [{ slug: "hello" }, { slug: "world" }]
  → for each slug: PostPage runs → getPost(slug) runs once
  → "use cache" + cacheLife("days") → result stored in Full Route Cache
  → deploy: CDN serves pre-built HTML (no server render per visitor)
```

**Thoughts:** `cacheLife` does **not** trigger prerendering — it only declares how long the cache is valid. Prerendering happens because `generateStaticParams` lists the URLs and the route stays static (no `cookies()` / `headers()`). Short profiles like `cacheLife("seconds")` **exclude** content from the build shell. See [ISR](#incremental-static-regeneration-isr) for timed/on-demand refresh details.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache) · [generateStaticParams](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

</details>

<details>
  <summary>Server-Side Rendering (SSR)</summary>

### Server-Side Rendering (SSR)

**Definition:** Server-Side Rendering (SSR) generates **fresh HTML on every request**. The server runs Server Components, fetches data, and sends a new page — nothing is reused from the **Full Route Cache** built at deploy time.

Unlike [SSG](#static-generation-ssg), there is no shared HTML for all visitors. Unlike [PPR](#partial-prerendering-ppr-cache-components), the whole route (or the dynamic subtree) renders at request time.

**When to use:** Authenticated dashboards, carts, admin panels, real-time data — anything that must reflect the current user or moment.

**Pluses and minuses:**

| +                           | −                                |
| --------------------------- | -------------------------------- |
| Always fresh data           | Higher TTFB, server load         |
| Per-request personalization | No CDN cache for HTML by default |

**Examples:**

Any **one** of these opts the route into SSR (they stack — you only need one):

| Trigger                                                                  | What it signals                             |
| ------------------------------------------------------------------------ | ------------------------------------------- |
| `await cookies()` / `await headers()` / reading `searchParams` in `page` | Request-specific data needed                |
| `fetch(..., { cache: "no-store" })`                                      | Data must not be cached                     |
| `export const dynamic = "force-dynamic"`                                 | Explicit “always SSR”                       |
| `await connection()`                                                     | WebSockets, SSE, or other request-only APIs |

```tsx
// app/dashboard/page.tsx — SSR: HTML regenerated every request
import { cookies } from "next/headers";

export const dynamic = "force-dynamic"; // optional — cookies() already opts in

export default async function Dashboard() {
  // Step 1: request API → route is dynamic → Full Route Cache skipped
  const session = (await cookies()).get("session")?.value;

  // Step 2: no-store → Data Cache skipped too
  const stats = await fetch("https://api.example.com/stats", {
    cache: "no-store",
  }).then((r) => r.json());

  return (
    <div>
      <h1>Welcome, {session ?? "guest"}</h1>
      <p>Active users: {stats.active}</p>
    </div>
  );
}
```

**Request flow:**

```text
request arrives
  → Next.js detects cookies() / force-dynamic → skip Full Route Cache
  → Server runs Dashboard + fetch (no-store)
  → fresh HTML + RSC payload sent to browser
  → repeats on every visit (no CDN HTML reuse)
```

**Thoughts:** SSR is the opposite of SSG — no build-time HTML reuse. Reading `cookies()` in `layout.tsx` makes the **entire subtree** dynamic; scope auth checks to the leaf that needs them. For mostly-static pages with a small personalized header, prefer [PPR](#partial-prerendering-ppr-cache-components) instead of full-route SSR.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [connection](https://nextjs.org/docs/app/api-reference/functions/connection) · [route segment config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)

</details>

<details>
  <summary>Incremental Static Regeneration (ISR)</summary>

### Incremental Static Regeneration (ISR)

**Definition:** Incremental Static Regeneration (ISR) is **SSG + timed or on-demand refresh**. Pages are still pre-rendered and CDN-fast, but Next.js **regenerates** them in the background after a TTL expires — or immediately when you call `revalidateTag` / `revalidatePath`.

Stale-while-revalidate: visitors keep seeing the old page while the server rebuilds the new one in the background.

**When to use:** Product listings, news, docs — shared content that changes occasionally but does not need per-request freshness.

**Pluses and minuses:**

| +                             | −                                                       |
| ----------------------------- | ------------------------------------------------------- |
| CDN-fast + eventual freshness | Users may see stale content during revalidation window  |
| Scales better than pure SSR   | Requires clear invalidation strategy for urgent updates |

**Examples:**

ISR = [SSG](#static-generation-ssg) caching **plus** a revalidation window:

| Piece                                          | Role at build / first request     | Role after TTL expires                       |
| ---------------------------------------------- | --------------------------------- | -------------------------------------------- |
| `"use cache"` + `cacheLife({ revalidate: N })` | Stores HTML + data with a TTL     | Serves stale page, regenerates in background |
| `cacheTag("products")`                         | Tags the cache entry              | `revalidateTag` busts cache on CMS publish   |
| `generateStaticParams()`                       | Pre-renders known URLs (optional) | Same slugs, fresher content after revalidate |

```tsx
// app/products/page.tsx — ISR via cacheLife revalidate window
// requires cacheComponents: true in next.config.ts
import { cacheLife, cacheTag } from "next/cache";

type Product = { id: string; name: string };

async function getProducts(): Promise<Product[]> {
  "use cache";
  cacheTag("products");
  cacheLife({ stale: 300, revalidate: 3600, expire: 86400 });
  // revalidate: 3600 → after 1 h, next request triggers background refresh
  // stale: 300   → client can show cached nav for 5 min without re-check

  const res = await fetch("https://api.example.com/products");
  return res.json();
}

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

```tsx
// app/products/actions.ts — on-demand ISR (no full rebuild)
"use server";

import { revalidateTag } from "next/cache";

export async function addProduct() {
  await db.product.create({ data: { name: "New item" } });
  revalidateTag("products", "max"); // bust tagged cache → next visit regenerates
}
```

**Request flow:**

```text
first build / first request
  → getProducts() runs → stored in Full Route Cache + Data Cache
within revalidate window (e.g. 1 h)
  → serve cached HTML instantly (CDN-fast)
after revalidate expires
  → visitor A gets STALE page immediately
  → server regenerates in background
  → visitor B gets fresh version
urgent CMS publish
  → revalidateTag("products") → skip waiting for TTL
```

**Thoughts:** ISR sits between SSG (frozen until rebuild) and SSR (always fresh). `cacheLife("days")` alone is closer to pure SSG; add a `revalidate` value or call `revalidateTag` when content must update sooner. Caching layers: [Next.js — Data § Caching model](./4_nextjs_data.md#caching-model-overview).

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [cacheLife](https://nextjs.org/docs/app/api-reference/functions/cacheLife) · [revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag)

</details>

<details>
  <summary>Partial Prerendering (PPR) / Cache Components</summary>

### Partial Prerendering (PPR) / Cache Components

**Definition:** Partial Prerendering (PPR) — enabled by **Cache Components** (`cacheComponents: true`) — splits one route into two regions on the **same page**:

- **Static shell** — prerendered at build, CDN-fast (`"use cache"` + long `cacheLife`)
- **Dynamic holes** — resolved per request inside `<Suspense>` (`cookies()`, short `cacheLife`, or uncached `fetch`)

The user gets fast first paint from the shell while personalized or live data streams in.

**When to use:** Pages with mostly public content + small personalized regions (header, cart badge, “Welcome, {name}”).

**Pluses and minuses:**

| +                                     | −                               |
| ------------------------------------- | ------------------------------- |
| Best of static + dynamic in one route | Requires `use cache` discipline |
| Fast LCP from static shell            | New API — team training needed  |

**Examples:**

| Piece                                | Build time (`next build`)            | Request time              |
| ------------------------------------ | ------------------------------------ | ------------------------- |
| `cacheComponents: true`              | Enables PPR model                    | —                         |
| `"use cache"` + `cacheLife("hours")` | Baked into **static shell**          | Served from cache         |
| `cookies()` inside `<Suspense>`      | Left as **dynamic hole** placeholder | Streams real content      |
| `<Suspense fallback>`                | Fallback included in shell           | Shown until hole resolves |

```tsx
// next.config.ts — step 0: enable Cache Components (PPR)
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  cacheComponents: true,
};

export default nextConfig;
```

```tsx
// app/dashboard/page.tsx
import { Suspense } from "react";
import { cacheLife, cacheTag } from "next/cache";
import { cookies } from "next/headers";

// Static shell — cached at build, long cacheLife → included in prerender
async function ProductsList() {
  "use cache";
  cacheTag("products-list");
  cacheLife("hours");

  const products = await fetch("https://api.example.com/products").then((r) =>
    r.json(),
  );
  return (
    <ul>
      {products.map((p: { id: string; name: string }) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}

// Dynamic hole — cookies() cannot run inside "use cache"
async function Greeting() {
  const user = (await cookies()).get("user")?.value;
  return <p>Welcome back, {user ?? "guest"}</p>;
}

export default function Page() {
  return (
    <section>
      <h1>Dashboard</h1> {/* static shell */}
      <ProductsList /> {/* static shell */}
      <Suspense fallback={<p>Loading…</p>}>
        <Greeting /> {/* dynamic hole — streams at request time */}
      </Suspense>
    </section>
  );
}
```

**Build + request flow:**

```text
next build
  → <h1> + ProductsList prerendered into static shell
  → Greeting slot saved as dynamic hole (Suspense boundary)
request
  → CDN sends static shell immediately (fast LCP)
  → browser shows <h1>, product list, and "Loading…" fallback
  → Greeting resolves (cookies()) → streams into the hole
  → user sees "Welcome back, alice"
```

**Thoughts:** PPR is not a third rendering mode on its own — it **combines** [SSG](#static-generation-ssg) shell with [SSR](#server-side-rendering-ssr) holes. Without `<Suspense>`, a `cookies()` call in the page tree can force the **entire** route dynamic. Wrap every request-time region. See also [Streaming SSR](#streaming-ssr).

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache) · [vercel.com/docs/partial-prerendering](https://vercel.com/docs/partial-prerendering)

</details>

<details>
  <summary>Combining rendering strategies</summary>

### Combining rendering strategies

**Definition:** Real apps rarely use one rendering mode everywhere. Next.js lets you **mix SSG, ISR, SSR, PPR, and CSR** at three levels:

| Level                    | What varies                                    | Typical pattern                                                     |
| ------------------------ | ---------------------------------------------- | ------------------------------------------------------------------- |
| **App (route tree)**     | Each URL segment picks its own mode            | Marketing = SSG, blog = ISR, dashboard = SSR                        |
| **Page (one URL)**       | Static shell + dynamic holes on the same route | [PPR](#partial-prerendering-ppr-cache-components) with `<Suspense>` |
| **Component (one page)** | Server HTML for shared data + client islands   | Server Component fetch + `"use client"` leaves                      |

The goal: **CDN-fast and SEO-friendly where possible**, fresh or personalized only where needed.

**When to use:** Any production app with public pages, semi-static content, and authenticated or personalized areas.

**Pluses and minuses:**

| +                                         | −                                                            |
| ----------------------------------------- | ------------------------------------------------------------ |
| Optimize cost and performance per section | More boundaries to design and test                           |
| One codebase, mixed strategies            | Easy to leak `cookies()` into a layout and dynamify too much |

**Examples:**

Three patterns you combine in practice — often all three in one app:

| Pattern                 | Where                       | Static part          | Dynamic part                   |
| ----------------------- | --------------------------- | -------------------- | ------------------------------ |
| **1. Route split**      | Different folders in `app/` | `/about`, `/blog`    | `/dashboard`                   |
| **2. Page split (PPR)** | Same `page.tsx`             | `"use cache"` blocks | `<Suspense>` + `cookies()`     |
| **3. Component split**  | Same Server `page.tsx`      | Server fetch + HTML  | Client leaves (`"use client"`) |

```text
app/
├── (marketing)/
│   └── about/page.tsx          → SSG  (use cache + cacheLife("max"))
├── blog/
│   └── [slug]/page.tsx         → ISR  (use cache + revalidate window)
├── dashboard/
│   └── page.tsx                → SSR  (cookies / force-dynamic)
└── page.tsx                    → PPR  (static shell + Suspense hole)
```

**1. Route-level hybrid** — each segment chooses its own mode:

```tsx
// app/(marketing)/about/page.tsx — SSG
import { cacheLife } from "next/cache";

async function getAbout() {
  "use cache";
  cacheLife("max");
  return fetch("https://api.example.com/about").then((r) => r.json());
}

export default async function AboutPage() {
  const data = await getAbout();
  return <main>{data.title}</main>;
}
```

```tsx
// app/dashboard/page.tsx — SSR (only this subtree is per-request)
import { cookies } from "next/headers";

export const dynamic = "force-dynamic";

export default async function DashboardPage() {
  const session = (await cookies()).get("session")?.value;
  return <h1>Welcome, {session ?? "guest"}</h1>;
}
```

**2. Page-level hybrid (PPR)** — static + dynamic on **one URL** (see [PPR](#partial-prerendering-ppr-cache-components)):

```tsx
// app/page.tsx — requires cacheComponents: true
import { Suspense } from "react";
import { cacheLife, cacheTag } from "next/cache";
import { cookies } from "next/headers";

async function PublicFeed() {
  "use cache";
  cacheTag("feed");
  cacheLife("hours");
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return (
    <ul>
      {posts.map((p: { id: string; title: string }) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}

async function UserBadge() {
  const name = (await cookies()).get("user")?.value;
  return <span>{name ?? "Sign in"}</span>;
}

export default function HomePage() {
  return (
    <main>
      <header>
        <Suspense fallback={<span>…</span>}>
          <UserBadge />
        </Suspense>
      </header>
      <PublicFeed />
    </main>
  );
}
```

**3. Component-level hybrid** — server HTML + client islands (no `cookies()` on the page):

```tsx
// app/posts/page.tsx — Server Component: SSG/ISR data
import { cacheLife, cacheTag } from "next/cache";
import { LikeButton } from "./like-button";

async function getPosts() {
  "use cache";
  cacheTag("posts");
  cacheLife({ revalidate: 3600 });
  return fetch("https://api.example.com/posts").then((r) => r.json());
}

export default async function PostsPage() {
  const posts = await getPosts();
  return (
    <ul>
      {posts.map((p: { id: string; title: string }) => (
        <li key={p.id}>
          {p.title}
          <LikeButton postId={p.id} /> {/* CSR island — hooks/events */}
        </li>
      ))}
    </ul>
  );
}
```

```tsx
// app/posts/like-button.tsx — CSR leaf only
"use client";

import { useState } from "react";

export function LikeButton({ postId }: { postId: string }) {
  const [liked, setLiked] = useState(false);
  return (
    <button onClick={() => setLiked((v) => !v)}>
      {liked ? "♥" : "♡"} {postId}
    </button>
  );
}
```

**Decision flow:**

```text
Is content the same for every visitor?
  yes → SSG or ISR (use cache + cacheLife)
  no  → Does only a small region need request data?
          yes → PPR: cache the rest, Suspense + cookies() for the hole
          no  → SSR for the whole route (or CSR fetch in a client leaf)
```

**Thoughts:** Prefer the **smallest dynamic boundary** — a Client leaf or a Suspense hole beats marking the whole app `force-dynamic`. Keep `cookies()` out of root `layout.tsx` unless every page needs it. Route groups `(marketing)` / `(app)` help organize hybrid trees without changing URLs.

**Sources:** [nextjs.org/docs/app/getting-started/caching](https://nextjs.org/docs/app/getting-started/caching) · [use cache](https://nextjs.org/docs/app/api-reference/directives/use-cache) · [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Route segment config</summary>

### Route segment config

**Definition:**

Route-level exports in `layout.tsx`, `page.tsx`, or `route.ts` that override Next.js defaults for caching, rendering, and runtime for that segment and its children (unless a child overrides).

| Export            | Values                                                                                 | Effect                                                      |
| ----------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `dynamic`         | `'auto'`, `'force-static'`, `'force-dynamic'`, `'error'`                               | Static vs dynamic rendering for the segment                 |
| `dynamicParams`   | `true` (default), `false`                                                              | Allow on-demand generation for unseen dynamic params vs 404 |
| `revalidate`      | `false`, `0`, `number` (seconds)                                                       | ISR interval for the segment                                |
| `fetchCache`      | `'auto'`, `'default-no-store'`, `'only-no-store'`, `'force-no-store'`, `'force-cache'` | Override default `fetch` caching in the segment             |
| `runtime`         | `'nodejs'` (default), `'edge'`                                                         | Execution runtime for Server Components and Route Handlers  |
| `preferredRegion` | string or `string[]`                                                                   | Pin serverless execution to regions                         |
| `maxDuration`     | number (seconds)                                                                       | Serverless function timeout cap                             |

**When to use:**

- `export const revalidate = 60` — content that changes occasionally (blog, catalog)
- `export const dynamic = 'force-static'` — legal/marketing pages that must never read request data
- `export const dynamicParams = false` — only slugs from `generateStaticParams` are valid
- `export const runtime = 'edge'` — low-latency Route Handler at the edge (see [Edge vs Node](./4_nextjs_platform.md#edge-vs-node-runtime))

**When not to use:**

- `force-dynamic` on root `layout.tsx` when only one leaf needs cookies — read request APIs in that leaf instead
- `fetchCache = 'force-no-store'` when `use cache` + `revalidateTag` already models invalidation

**Pluses and minuses:**

| +                               | −                                                  |
| ------------------------------- | -------------------------------------------------- |
| Surgical control per route tree | Easy to blanket `force-dynamic` and kill CDN cache |
| Children inherit parent config  | Can fight declarative `use cache` boundaries       |
| No need to wrap every `fetch`   | Some exports deprecated or narrowed in Next.js 16  |

**Examples:**

```tsx
// app/blog/[slug]/page.tsx
export const revalidate = 3600;
export const dynamicParams = true;

export async function generateStaticParams() {
  const slugs = await getSlugs();
  return slugs.map((slug) => ({ slug }));
}

export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const post = await getPost(slug);
  return <article>{post.title}</article>;
}
```

```tsx
// app/dashboard/layout.tsx — authenticated subtree
export const dynamic = "force-dynamic";

export default async function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const session = await getSession();
  if (!session) redirect("/login");
  return <>{children}</>;
}
```

**Thoughts:**

- Reading `cookies()`, `headers()`, or `connection()` in a Server Component opts the route into **dynamic rendering** unless PPR/`use cache` provides a static shell.
- `dynamic = 'error'` — build fails if anything would make the segment dynamic (guardrail for "must be static" routes).
- Next.js 16: prefer `use cache` + `cacheLife` / `cacheTag` for ISR; `export const revalidate` still works but is the legacy segment-level knob.
- Prefer declarative caching over blanket `force-dynamic` on a layout.
- Pages Router mapping: `getStaticProps` + `revalidate` → `cacheLife({ revalidate: N })` or `revalidate` export; `getServerSideProps` → dynamic via request APIs.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/route-segment-config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)

</details>

<a name="2-server-client-boundaries"></a>

## 2. Server & Client boundaries

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Server Components</summary>

### Server Components

**Definition:** Components in `app/` are **Server Components by default** — no directive needed unless you opt into the client. They run on the server, can be `async`, access DB/filesystem, and ship no component JS to the client.

**How it works:**

- File has no `"use client"` — React executes it on the server only
- Can be `async` — `await` database queries, `fetch`, or filesystem reads in the component body
- Output becomes **RSC payload** (and often prerendered HTML); server-only logic is stripped from the client bundle
- May import **Client Components** as leaves — pass serializable props or `children` for interactivity

**Not supported:**

- React hooks (`useState`, `useEffect`, etc.)
- Browser APIs (`window`, `document`, `localStorage`)
- Event handlers (`onClick`, `onChange`)

**When to use:** Data fetching, layouts, static content, passing data to interactive leaves.

**Pluses and minuses:**

| +                                           | −                                               |
| ------------------------------------------- | ----------------------------------------------- |
| Zero client JS for server-only UI           | No `useState`, `useEffect`, event handlers      |
| Direct secrets and DB access                | Props to Client Components must be serializable |
| Automatic code splitting at import boundary | Debugging requires server logs                  |

**Examples:**

```tsx
// app/products/page.tsx — no "use client" directive
export default async function ProductsPage() {
  const products = await db.product.findMany();
  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Client Components</summary>

### Client Components

**Definition:** Files with `"use client"` at the top. Hydrate in the browser; support hooks, state, and events.

**When to use:** Buttons, forms with instant validation, charts, anything with `useState` / `useEffect`.

**Pluses and minuses:**

| +                                     | −                                 |
| ------------------------------------- | --------------------------------- |
| Full interactivity                    | Increases client bundle size      |
| Browser APIs, third-party client libs | Cannot import server-only modules |

**Examples:**

```tsx
"use client";

import { useState } from "react";

export function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}
```

**Thoughts:** Push `"use client"` to **leaves** — not page roots. A single `"use client"` at the top of a large tree pulls all imports into the client bundle.

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Composition pattern</summary>

### Composition pattern

**Definition:** **Composition** is how Server and Client Components are wired together in the App Router. The default shape: a **Server Component parent** fetches data and renders static markup; **Client Component leaves** handle state, events, and browser APIs. Server Components may import Client Components; Client Components **cannot** import Server Components — pass server UI as `children` or other props instead.

**How it works:**

| Pattern                 | Direction                      | Rule                                                                                                                                     |
| ----------------------- | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Leaf import**         | Server → Client                | Server `page`/`layout` imports `"use client"` components; props must be [serializable](#serialization-rules)                             |
| **`children` slot**     | Server → Client                | Client shell (`Modal`, `Tabs`) receives Server Components as `children` — server tree is rendered first, then passed across the boundary |
| **Context provider**    | Client wraps server tree       | Provider is a Client Component; Server `layout` wraps `{children}` inside it — context only works below the provider                     |
| **Third-party wrapper** | Server → thin Client re-export | Library component without `"use client"` gets a one-line Client wrapper, then Server pages import the wrapper                            |

```text
app/posts/page.tsx          Server — fetch + markup
  └── LikeButton.tsx        Client leaf — hooks / onClick
app/layout.tsx              Server
  └── ThemeProvider.tsx     Client — context; children stay Server
      └── {children}
```

**When to use:**

- Page needs both server data (DB, secrets, `fetch`) and interactive islands (forms, toggles, charts)
- Client shell needs server-rendered content inside (`Modal` + `Cart`, `Tabs` + server panels)
- App-wide client context (theme, i18n client store) must wrap part of the tree

**When not to use:**

- Entire page is interactive — a Client page that fetches on the client may be simpler (but loses server data access and SEO defaults)
- Passing huge objects as props — map to DTOs before the Client boundary
- Importing a Server Component inside a Client file — use `children` or lift the Client leaf down

**Pluses and minuses:**

| +                                                     | −                                                           |
| ----------------------------------------------------- | ----------------------------------------------------------- |
| Smallest possible client bundle — only leaves hydrate | Mental model: two module graphs, one RSC boundary           |
| Secrets and DB stay on the server                     | Props and `children` slots must follow serialization rules  |
| Server HTML + client interactivity on the same route  | Easy to accidentally mark a large subtree `"use client"`    |
| `children` pattern keeps Client shells reusable       | Context providers add a client boundary above their subtree |

**Examples:**

**1. Leaf import** — Server page, Client button:

```tsx
// app/posts/page.tsx — Server Component
import { LikeButton } from "./like-button";

export default async function PostsPage() {
  const posts = await getPosts();
  return (
    <ul>
      {posts.map((p) => (
        <li key={p.id}>
          {p.title}
          <LikeButton postId={p.id} initialLikes={p.likes} />
        </li>
      ))}
    </ul>
  );
}
```

```tsx
// app/posts/like-button.tsx — Client leaf
"use client";

import { useState } from "react";

export function LikeButton({
  postId,
  initialLikes,
}: {
  postId: string;
  initialLikes: number;
}) {
  const [likes, setLikes] = useState(initialLikes);
  return (
    <button onClick={() => setLikes((n) => n + 1)}>
      ♥ {likes} ({postId})
    </button>
  );
}
```

**2. `children` slot** — Client shell, server content inside:

```tsx
// app/ui/modal.tsx — Client shell (state, focus trap, portal)
"use client";

import { useState } from "react";

export function Modal({ children }: { children: React.ReactNode }) {
  const [open, setOpen] = useState(false);
  if (!open) {
    return (
      <button type="button" onClick={() => setOpen(true)}>
        Open cart
      </button>
    );
  }
  return (
    <dialog open>
      {children}
      <button type="button" onClick={() => setOpen(false)}>
        Close
      </button>
    </dialog>
  );
}
```

```tsx
// app/cart/page.tsx — Server Component passes server UI as children
import { Modal } from "../ui/modal";
import { Cart } from "../ui/cart"; // Server — fetches cart on the server

export default function CartPage() {
  return (
    <Modal>
      <Cart />
    </Modal>
  );
}
```

**3. Context provider** — Client provider, server layout wraps children:

```tsx
// app/theme-provider.tsx
"use client";

import { createContext, useContext } from "react";

const ThemeContext = createContext<"light" | "dark">("light");
export const useTheme = () => useContext(ThemeContext);

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>;
}
```

```tsx
// app/layout.tsx — Server layout; provider only wraps {children}, not <html>
import { ThemeProvider } from "./theme-provider";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  );
}
```

**Thoughts:**

- `"use client"` applies to the **module graph** — everything the Client file imports becomes client code. Server Components passed as `children` are **not** imported into that graph; they render on the server first.
- Keep providers **as deep as possible** — wrap `{children}`, not the whole `<html>`, so more of the tree stays server-only.
- Same idea at route level: [Combining rendering strategies](#combining-rendering-strategies) — server fetch + CSR leaves without making the whole page dynamic.
- Mark server-only helpers with `import "server-only"` so accidental Client imports fail at build time.

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components) · [Serialization rules](#serialization-rules)

</details>

<details>
  <summary>Next.js 16 async request APIs</summary>

### Next.js 16 async request APIs

**Definition:** In Next.js 15+, all **request-time** inputs are **async**. Route props (`params`, `searchParams`) arrive as `Promise`s; `cookies()`, `headers()`, and `draftMode()` from `next/headers` also return Promises. In Server Components and Server Functions, **`await` them** inside `async` functions. Synchronous access still works temporarily for migration, but is deprecated — treat Next.js 16 as **await-only**.

**How it works:**

| API            | Where                                                | Server Component                          | Client Component                                            |
| -------------- | ---------------------------------------------------- | ----------------------------------------- | ----------------------------------------------------------- |
| `params`       | `page`, `layout`, `generateMetadata`, Route Handlers | `const { slug } = await params`           | `useParams()` or `use(params)` on the page prop             |
| `searchParams` | `page` only                                          | `const { q } = await searchParams`        | `useSearchParams()` or `use(searchParams)` on the page prop |
| `cookies()`    | Server Components, Actions, Route Handlers           | `const store = await cookies()`           | Not available — use Server Actions                          |
| `headers()`    | Server Components, Actions, Route Handlers           | `const list = await headers()`            | Not available                                               |
| `draftMode()`  | Server Components, Actions, Route Handlers           | `const { isEnabled } = await draftMode()` | Not available                                               |

```text
Request arrives
  → Next.js resolves params / searchParams / cookies / headers as Promises
  → Server Component awaits inside async function
  → Values known only at request time → route may become dynamic
```

**When to use:**

- Dynamic routes — `app/blog/[slug]/page.tsx` needs `await params`
- Filters, pagination, sort — `await searchParams` on listing pages
- Auth/session — `await cookies()` before rendering protected UI (details: [cookies() & headers()](#cookies-headers))
- CMS preview — `await draftMode()` to bypass static cache
- Typed routes — `PageProps<'/blog/[slug]'>` after `next dev` / `next build` generates types

**When not to use:**

- Inside `"use cache"` bodies for data that should stay static — request APIs force dynamic rendering
- In Client Components via `next/headers` — build error; keep request reads on the server
- Reading `searchParams` in root `layout` when only one child route needs it — scope to that segment

**Pluses and minuses:**

| +                                                      | −                                                                       |
| ------------------------------------------------------ | ----------------------------------------------------------------------- |
| Clear boundary: request data is always async           | Breaking change from Next.js 14 sync props                              |
| Works uniformly in pages, layouts, metadata, handlers  | Forgetting `await` causes subtle bugs or type errors                    |
| `PageProps<'/route'>` gives autocomplete for `params`  | `searchParams` on a page opts the **whole page** into dynamic rendering |
| Client pages can still read promises via React `use()` | Sync fallback will be removed — migrate early                           |

**Examples:**

**1. Server page** — all four request inputs:

```tsx
import { cookies, headers } from "next/headers";

export default async function Page({
  params,
  searchParams,
}: {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ q?: string; page?: string }>;
}) {
  const { id } = await params;
  const { q = "", page = "1" } = await searchParams;
  const session = (await cookies()).get("session")?.value;
  const country = (await headers()).get("x-vercel-ip-country") ?? "US";

  const item = await getItem(id, { q, page, session, country });
  return <div>{item.title}</div>;
}
```

**2. Layout + metadata** — `params` is async outside `page` too:

```tsx
// app/blog/[slug]/layout.tsx
export default async function BlogLayout({
  children,
  params,
}: {
  children: React.ReactNode;
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  return (
    <section>
      <nav>Series: {slug}</nav>
      {children}
    </section>
  );
}
```

```tsx
// app/blog/[slug]/page.tsx
import type { Metadata } from "next";

export async function generateMetadata({
  params,
}: {
  params: Promise<{ slug: string }>;
}): Promise<Metadata> {
  const { slug } = await params;
  const post = await getPost(slug);
  return { title: post.title };
}
```

**3. Typed `PageProps`** — route literal from generated types:

```tsx
export default async function Page(props: PageProps<"/blog/[slug]">) {
  const { slug } = await props.params;
  const { tag } = await props.searchParams;
  return (
    <h1>
      {slug} — {tag}
    </h1>
  );
}
```

**4. Client page** — cannot be `async`; unwrap with React `use()`:

```tsx
"use client";

import { use } from "react";

export default function Page({
  params,
  searchParams,
}: {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ q?: string }>;
}) {
  const { id } = use(params);
  const { q } = use(searchParams);
  return (
    <p>
      ID: {id}, q: {q}
    </p>
  );
}
```

**5. Client leaf (not page)** — hooks instead of props:

```tsx
"use client";

import { useParams, useSearchParams } from "next/navigation";

export function Filters() {
  const { slug } = useParams<{ slug: string }>();
  const searchParams = useSearchParams();
  const q = searchParams.get("q");
  return (
    <span>
      {slug} — {q}
    </span>
  );
}
```

| Example route                    | URL             | `await params` resolves to           |
| -------------------------------- | --------------- | ------------------------------------ |
| `app/shop/[slug]/page.tsx`       | `/shop/hat`     | `{ slug: "hat" }`                    |
| `app/shop/[...slug]/page.tsx`    | `/shop/a/b`     | `{ slug: ["a", "b"] }`               |
| `app/shop/page.tsx` + `?a=1&b=2` | `/shop?a=1&b=2` | `searchParams`: `{ a: "1", b: "2" }` |

**Thoughts:**

- **Dynamic rendering trigger** — `searchParams`, `cookies()`, `headers()`, and `connection()` opt the route dynamic at request time unless isolated with PPR or a static shell. See [SSR](#server-side-rendering-ssr) and [cookies() & headers()](#cookies-headers).
- **Migrate from sync** — run `npx @next/codemod@latest next-async-request-api .` on Next.js 14 codebases.
- **Do not mix patterns** — in Server Components use `await`; in Client leaves use `useParams` / `useSearchParams`; on Client **pages** use `use(params)` / `use(searchParams)`.
- Preview/draft content: [draftMode](./4_nextjs_data.md#draftmode) in the Data topic.

**Sources:** [page.js — params & searchParams](https://nextjs.org/docs/app/api-reference/file-conventions/page) · [cookies](https://nextjs.org/docs/app/api-reference/functions/cookies) · [headers](https://nextjs.org/docs/app/api-reference/functions/headers) · [draftMode](https://nextjs.org/docs/app/api-reference/functions/draft-mode)

</details>

<details>
  <summary>cookies() & headers() in Server Components</summary>

<a name="cookies-headers"></a>

### cookies() & headers() in Server Components

**Definition:**

`cookies()` and `headers()` from `next/headers` read the incoming HTTP request on the server. In Next.js 15+, both return **Promises** and must be `await`ed. Using them opts the route into **dynamic rendering** (unless isolated with PPR/`use cache: private`).

| API         | Read                              | Write           | Typical use                                          |
| ----------- | --------------------------------- | --------------- | ---------------------------------------------------- |
| `cookies()` | `get`, `getAll`, `has`            | `set`, `delete` | Session tokens, preferences, A/B buckets             |
| `headers()` | `get`, `getAll`, `has`, `entries` | Read-only       | Auth bearer, `user-agent`, geo, custom proxy headers |

**When to use:**

- Session validation in Server Component before rendering protected UI
- Reading `Accept-Language` or `Authorization` for server-side personalization
- Setting auth cookies in Server Actions after login (`cookies().set(...)`)

**When not to use:**

- Reading cookies in root layout when only `/dashboard` needs auth — scope to dashboard layout
- Storing large payloads in cookies (4 KB limit, sent on every request)

**Pluses and minuses:**

| +                                                   | −                                                                                   |
| --------------------------------------------------- | ----------------------------------------------------------------------------------- |
| No client exposure of sensitive tokens              | Forces dynamic rendering when used in page tree                                     |
| Works in Server Components, Actions, Route Handlers | Cannot use in Client Components — use `document.cookie` sparingly or server actions |
| `httpOnly` cookies stay off `window`                | Over-reading cookies in layout affects entire subtree                               |

**Examples:**

```tsx
import { cookies, headers } from "next/headers";
import { redirect } from "next/navigation";

export default async function AccountPage() {
  const cookieStore = await cookies();
  const session = cookieStore.get("session")?.value;

  if (!session) redirect("/login");

  const headersList = await headers();
  const country = headersList.get("x-vercel-ip-country") ?? "US";

  return <p>Welcome — region: {country}</p>;
}
```

```tsx
"use server";

import { cookies } from "next/headers";

export async function login(formData: FormData) {
  const token = await authenticate(formData);
  const cookieStore = await cookies();
  cookieStore.set("session", token, {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "lax",
    path: "/",
    maxAge: 60 * 60 * 24 * 7,
  });
}
```

**Thoughts:**

- Client Components: use `useSearchParams()` / `useParams()` for URL state; never import `next/headers` in `'use client'` files.
- `headers()` is read-only — set response headers in Route Handlers (`NextResponse`) or `next.config` `headers`.
- Pair with [Authentication patterns](./4_nextjs_platform.md#authentication-patterns) — validate session in the data layer, not only in proxy.

**Sources:** [nextjs.org/docs/app/api-reference/functions/cookies](https://nextjs.org/docs/app/api-reference/functions/cookies) · [headers](https://nextjs.org/docs/app/api-reference/functions/headers)

</details>

<details>
  <summary>RSC payload</summary>

### RSC payload

**Definition:**

The **RSC payload** (React Server Component Payload) is a compact, serialized representation of the rendered Server Component tree that Next.js sends from server to browser. It is **not** HTML and not client JavaScript — it is the **Flight wire format** React uses to reconcile the UI on the client.

| Part                             | Purpose                                         |
| -------------------------------- | ----------------------------------------------- |
| Rendered Server Component output | UI structure and text the browser should show   |
| Client Component placeholders    | Where interactive islands mount                 |
| Client chunk references          | Which JS bundles to download                    |
| Serializable props               | Data passed Server → Client across the boundary |

**How it works:**

**On the server:** Next.js renders route segments (layouts + pages). Server Components become an RSC payload; Client Components are referenced (not executed) in that payload. Together with Client Component code, Next.js can also prerender HTML.

**First load (browser):**

1. **HTML** — immediate non-interactive preview (fast FCP)
2. **RSC payload** — reconcile server/client tree on the client
3. **Client JS** — hydrate Client Components (attach event handlers)

**Client-side navigation (`<Link>`):** Next.js prefetches/fetches a fresh RSC payload for the target route. The **Router Cache** stores recent payloads in the session; **Full Route Cache** stores HTML + payload for static routes at build/revalidation time. See [Caching model](./4_nextjs_data.md#caching-model-overview).

**When to use:**

Understanding RSC payload helps when debugging:

- `?_rsc=` requests or `text/x-component` responses in DevTools
- Stale UI after mutations (`router.refresh()` re-fetches payload)
- Large TTFB from oversized serialized props

**Pluses and minuses:**

| +                                           | −                                                |
| ------------------------------------------- | ------------------------------------------------ |
| Server Component logic stays off the client | Opaque format — harder to inspect than JSON APIs |
| Smaller client bundles vs traditional SSR   | Prop mistakes surface as serialization errors    |
| Enables streaming + partial updates         | Payload size directly affects navigation speed   |

**Examples:**

```tsx
// app/posts/[id]/page.tsx — Server Component
import { LikeButton } from "@/components/like-button";

export default async function PostPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  const post = await db.post.findUnique({ where: { id } });

  return (
    <article>
      <h1>{post.title}</h1> {/* → rendered output in RSC payload */}
      <LikeButton likes={post.likes} />{" "}
      {/* → placeholder + chunk ref + props */}
    </article>
  );
}
```

```
Network tab (client navigation):
  GET /posts/42?_rsc=abc123
  Content-Type: text/x-component   ← RSC payload (Flight)
```

**Thoughts:**

- Same route may return **HTML + RSC payload** on first document request; navigations often fetch **payload only**.
- Trim props — every byte in the payload ships to the client. DTO rules: [Serialization rules](#serialization-rules).
- Generic RSC vs hydration theory: [4_react_server.md §8](./4_react_server.md#8-server-rendering--rsc). Security — treat Client-bound props as public: [RSC security](./4_nextjs_platform.md#rsc-security).

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components) · [React Server Components](https://react.dev/reference/rsc/server-components)

</details>

<details>
  <summary>Serialization rules</summary>

### Serialization rules

**Definition:**

Props passed from Server Components to Client Components cross the **RSC Flight boundary** — they must be JSON-serializable. Next.js serializes them into the [RSC payload](#rsc-payload) sent to the browser.

| Allowed                                             | Not allowed                                              |
| --------------------------------------------------- | -------------------------------------------------------- |
| Primitives (`string`, `number`, `boolean`, `null`)  | Functions (except Server Action references)              |
| Plain objects and arrays                            | Class instances (`new Date()` — pass ISO string instead) |
| `undefined` (omitted from payload)                  | `Map`, `Set`, `Symbol`, `BigInt`                         |
| Server Action function references                   | Non-enumerable or circular structures                    |
| React elements from Server Components as `children` | DOM nodes, refs to server-only objects                   |

**When to use:**

- Pass **DTOs** (plain objects) from server fetch to client UI — `{ id, title, publishedAt: post.createdAt.toISOString() }`
- Pass Server Actions as props to Client form components — `action={updateTodo}` without invoking inside cached bodies
- Use `children` composition — pass `<ClientChart data={serialized} />` from Server parent

**When not to use:**

- Passing ORM models with methods/hidden fields — map to plain objects first
- Passing callbacks from Server to Client — use Server Actions or client-side handlers instead

**Pluses and minuses:**

| +                                  | −                                        |
| ---------------------------------- | ---------------------------------------- |
| Small, predictable client payloads | Easy to accidentally pass huge objects   |
| Clear server/client contract       | `Date`/`Map` require manual conversion   |
| Server Actions passable as props   | Debugging serialization errors is opaque |

**Examples:**

```tsx
// app/posts/page.tsx — Server Component
import { LikeButton } from "./like-button";
import { toggleLike } from "./actions";

export default async function PostsPage() {
  const posts = await db.post.findMany();
  const serialized = posts.map((p) => ({
    id: p.id,
    title: p.title,
    createdAt: p.createdAt.toISOString(), // Date → string
  }));

  return (
    <ul>
      {serialized.map((p) => (
        <li key={p.id}>
          {p.title}
          <LikeButton postId={p.id} onLike={toggleLike} />
        </li>
      ))}
    </ul>
  );
}
```

```tsx
// WRONG — class instance won't serialize
<ClientCard user={await db.user.findUnique({ where: { id } })} />

// RIGHT — plain DTO
<ClientCard user={{ id: user.id, name: user.name }} />
```

**Thoughts:**

- Server Actions passed as props are **references** — do not call them inside `'use cache'` cached function bodies; pass the reference through.
- [RSC payload](#rsc-payload) size affects TTFB — trim props to what the client actually renders.
- `children` slot pattern avoids serializing large trees as props — Server renders static shell, Client wraps interactive leaves.

**Sources:** [nextjs.org/docs/app/getting-started/server-and-client-components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<a name="3-streaming-suspense"></a>

## 3. Streaming & Suspense

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Streaming SSR</summary>

### Streaming SSR

**Definition:** Server sends HTML in chunks as async Server Components resolve. Slow sections do not block fast sections.

**When to use:** Pages with independent slow/fast data regions.

**Pluses and minuses:**

| +                                     | −                                      |
| ------------------------------------- | -------------------------------------- |
| Better TTFB and perceived performance | More complex loading state design      |
| Natural fit for nested data           | Debugging streamed responses is harder |

**Examples:**

```tsx
import { Suspense } from "react";

async function SlowData() {
  const data = await fetch("https://api.example.com/slow").then((r) =>
    r.json(),
  );
  return <div>{data.value}</div>;
}

export default function Page() {
  return (
    <div>
      <h1>Fast header</h1>
      <Suspense fallback={<p>Loading…</p>}>
        <SlowData />
      </Suspense>
    </div>
  );
}
```

**Sources:** [nextjs.org/docs/app/getting-started/linking-and-navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating)

</details>

<details>
  <summary>loading.tsx</summary>

### loading.tsx

**Definition:** Convention file that wraps the segment in Suspense with instant fallback during navigation.

**Pluses and minuses:**

| +                                     | −                                                            |
| ------------------------------------- | ------------------------------------------------------------ |
| Zero boilerplate Suspense per route   | Coarse-grained — one loading UI per segment                  |
| Instant feedback on client navigation | Does not replace in-component Suspense for partial streaming |

**Examples:**

```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <div className="skeleton">Loading dashboard…</div>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/loading](https://nextjs.org/docs/app/api-reference/file-conventions/loading)

</details>

<details>
  <summary>CSR bailout</summary>

### CSR bailout

**Definition:** Some hooks (`useSearchParams`, `usePathname` in certain setups) cause the nearest Suspense boundary to bail out to client rendering if not wrapped.

**When to use:** Wrap components using `useSearchParams()` in `<Suspense>`.

**Examples:**

```tsx
import { Suspense } from "react";
import { SearchResults } from "./search-results";

export default function Page() {
  return (
    <Suspense fallback={<p>Loading search…</p>}>
      <SearchResults />
    </Suspense>
  );
}
```

**Sources:** [nextjs.org/docs/messages/missing-suspense-with-csr-bailout](https://nextjs.org/docs/messages/missing-suspense-with-csr-bailout)

</details>

<details>
  <summary>connection()</summary>

### connection()

**Definition:** API to opt a route out of static prerendering — required for WebSockets, SSE, or other request-time-only features.

**Examples:**

```tsx
import { connection } from "next/server";

export async function GET() {
  await connection();
  // WebSocket upgrade or dynamic handler
  return new Response("OK");
}
```

**Thoughts:**

See [React Suspense SSR architecture](https://github.com/reactwg/react-18/discussions/37) for underlying React 18 streaming design.

---

**Sources:** [nextjs.org/docs/app/api-reference/functions/connection](https://nextjs.org/docs/app/api-reference/functions/connection)

</details>
