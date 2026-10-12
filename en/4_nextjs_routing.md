# Next.js — Routing

App Router, file-based routes, and navigation.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [1. App vs Pages Router](#1-app-vs-pages-router)
- [2. Project structure](#2-project-structure)
- [3. File-based routing](#3-file-based-routing)
- [4. Special files](#4-special-files)
- [5. Navigation](#5-navigation)

---

## 1. App vs Pages Router

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>App Router</summary>

### App Router

**Definition:** The modern router living in `app/`. Folders define URL segments; a route becomes **public** only when a segment has `page.tsx` (UI) or `route.ts` (HTTP handler). Built on React Server Components, nested layouts, streaming Suspense boundaries, and the Next.js caching model. Default for all new projects since Next.js 13.

**How it works:**

- **File-system routing** — `app/blog/[slug]/page.tsx` maps to `/blog/:slug`. Dynamic segments: `[slug]` (one param), `[...slug]` (catch-all), `[[...slug]]` (optional catch-all). In Next.js 15+, `params` and `searchParams` are **Promises** — `await` them in Server Components.
- **Special files per segment:**

| File            | Role                                                                           |
| --------------- | ------------------------------------------------------------------------------ |
| `layout.tsx`    | Shared shell wrapping child segments; state persists across client navigations |
| `page.tsx`      | Unique UI for the route — makes the URL publicly accessible                    |
| `loading.tsx`   | Instant fallback while the segment streams (React Suspense boundary)           |
| `error.tsx`     | Segment error boundary (`"use client"` required)                               |
| `not-found.tsx` | 404 UI for this segment (`notFound()` triggers it)                             |
| `route.ts`      | Route Handlers — HTTP endpoints (`GET`, `POST`, …) colocated with UI           |
| `template.tsx`  | Like `layout`, but **remounts** on navigation (fresh state/effects)            |

- **Render order inside a segment:** `layout` → `template` → `error` → `loading` (Suspense) → `not-found` → `page`. Parent layouts wrap children recursively (`app/layout.tsx` → `app/dashboard/layout.tsx` → `page`).
- **Server Components by default** — `layout` and `page` run on the server unless the file starts with `"use client"`. Fetch data directly in `async` components; add Client Components only for hooks, events, or browser APIs.
- **Static by default** — routes prerender at build time unless they read request-time data (`cookies()`, `headers()`, `searchParams`, or `fetch` with `cache: 'no-store'`). Override per segment with `export const dynamic = 'force-static' | 'force-dynamic'`.
- **Colocation without routing** — route groups `(marketing)` omit the folder name from the URL; private folders `_lib` / `_components` are ignored by the router. Non-routable files can also sit beside `page.tsx` in the same folder.

**When to use:** Every new project. Greenfield apps, e-commerce, dashboards, content sites.

**Pluses and minuses:**

| +                                                     | −                                                       |
| ----------------------------------------------------- | ------------------------------------------------------- |
| RSC, streaming, nested layouts                        | Learning curve for caching and Server/Client boundaries |
| Colocated `loading.tsx`, `error.tsx`, `not-found.tsx` | Cannot mix `pages/` and `app/` for the same URL         |
| Server Actions, Metadata API, Route Handlers          | Some third-party libs still lack RSC support            |
| Fine-grained caching with `use cache` / `revalidate`  | Migration from Pages requires rewrites                  |
| Prefetch + client transitions via `next/link`         | `template.tsx` vs `layout.tsx` trips up teams early     |

**Examples:**

```tsx
// app/layout.tsx — root layout (required); must include <html> and <body>
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

```tsx
// app/blog/layout.tsx — nested layout shared by /blog/*
export default function BlogLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <section>
      <nav>Blog</nav>
      {children}
    </section>
  );
}
```

```tsx
// app/blog/[slug]/page.tsx — async Server Component (default)
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
// app/blog/[slug]/loading.tsx — shown instantly while page.tsx streams
export default function Loading() {
  return <p>Loading post…</p>;
}
```

```ts
// app/api/posts/route.ts — Route Handler (replaces pages/api)
import { NextResponse } from "next/server";

export async function GET() {
  const posts = await getPosts();
  return NextResponse.json(posts);
}
```

**Thoughts:** Deep dives on dynamic, parallel, and intercepting routes live in [Next.js — Routing](./4_nextjs_routing.md). Rendering strategies and streaming: [Next.js — Rendering](./4_nextjs_rendering.md). Caching, `fetch` options, and Server Actions: [Next.js — Data](./4_nextjs_data.md). Interview framing: **RSC + static by default**; `"use client"` is the opt-in for interactivity; App Router wins URL conflicts over `pages/`.

**Sources:** [Project structure](https://nextjs.org/docs/app/getting-started/project-structure) · [Layouts and pages](https://nextjs.org/docs/app/getting-started/layouts-and-pages) · [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Pages Router</summary>

### Pages Router

**Definition:** Legacy router in `pages/`. Uses `getServerSideProps`, `getStaticProps`, `getInitialProps`, and `pages/api` for backend endpoints.

**When to use:** Maintaining existing Pages Router apps only. Do not start new projects on Pages Router.

**Pluses and minuses:**

| +                                       | −                                                      |
| --------------------------------------- | ------------------------------------------------------ |
| Mature, well-documented patterns        | No RSC, no nested layouts                              |
| Simpler mental model (props-based data) | `getServerSideProps` runs per page — harder to compose |
| Large existing codebase support         | Not where new Next.js features land                    |

**Examples:**

```tsx
// pages/blog/[slug].tsx — legacy pattern
export async function getStaticProps({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug);
  return { props: { post } };
}

export default function BlogPost({ post }: { post: Post }) {
  return <article>{post.title}</article>;
}
```

**Sources:** [nextjs.org/docs/pages](https://nextjs.org/docs/pages)

</details>

<details>
  <summary>Migration mapping</summary>

### Migration mapping

**Definition:**

One-to-one mapping from Pages Router APIs to App Router equivalents for incremental migration.

| Pages Router         | App Router equivalent                                        |
| -------------------- | ------------------------------------------------------------ |
| `getStaticProps`     | `fetch` with `cache: 'force-cache'` + `generateStaticParams` |
| `getServerSideProps` | `fetch` with `cache: 'no-store'` or dynamic route            |
| `getInitialProps`    | Server Component async data fetch                            |
| `pages/api/*.ts`     | `app/**/route.ts` (Route Handlers)                           |
| `_app.tsx`           | `app/layout.tsx`                                             |
| `_document.tsx`      | `app/layout.tsx` + Metadata API                              |
| `next/head`          | `export const metadata` / `generateMetadata()`               |
| `next/router`        | `next/navigation` (`useRouter`, `usePathname`)               |
| `next/link`          | `next/link` (same package, App Router aware)                 |

**Examples:**

```tsx
// Pages Router (legacy)
export async function getServerSideProps() {
  const res = await fetch("https://api.example.com/data");
  return { props: { data: await res.json() } };
}

// App Router (modern)
async function getData() {
  const res = await fetch("https://api.example.com/data", {
    cache: "no-store",
  });
  return res.json();
}

export default async function Page() {
  const data = await getData();
  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

**Thoughts:**

Both routers can coexist in one project (`pages/` + `app/`), but **not for the same URL**. App Router takes precedence. Plan incremental migration route-by-route.

---

**Sources:** [nextjs.org/docs/app/guides/migrating](https://nextjs.org/docs/app/guides/migrating)

</details>

<a name="3-project-structure"></a>

## 2. Project structure

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Directory layout</summary>

### Directory layout

**Definition:** Next.js uses file-system conventions. The `app/` directory (or `src/app/`) defines routes; special filenames map to framework features.

**When to use:** Follow defaults unless monorepo or FSD requires `src/app/` + feature folders.

**Pluses and minuses:**

| +                                          | −                                                      |
| ------------------------------------------ | ------------------------------------------------------ |
| Predictable structure — onboarding is fast | Deep nesting can mirror URL too literally              |
| Colocation of route UI, loading, errors    | Large `app/` trees need route groups or FSD discipline |
| `src/` option keeps root clean             | Moving files changes URLs unless using groups/rewrites |

**Examples:**

```
my-app/
├── app/                    # App Router (or src/app/)
│   ├── layout.tsx          # Root layout (required)
│   ├── page.tsx            # Home route (/)
│   ├── loading.tsx         # Loading UI for segment
│   ├── error.tsx           # Error boundary for segment
│   ├── not-found.tsx       # 404 for segment
│   ├── (marketing)/        # Route group (no URL segment)
│   │   └── about/page.tsx  # /about
│   ├── blog/
│   │   ├── page.tsx        # /blog
│   │   └── [slug]/page.tsx # /blog/:slug
│   └── api/
│       └── webhook/route.ts
├── public/                 # Static assets (served as-is)
├── next.config.ts
├── package.json
└── tsconfig.json
```

**Thoughts:** Prefer `src/app/` in larger codebases to separate app code from config at root. Private folders: prefix with `_` (e.g. `_components`) — not routed.

**Sources:** [nextjs.org/docs/app/getting-started/project-structure](https://nextjs.org/docs/app/getting-started/project-structure)

</details>

<details>
  <summary>Special files</summary>

### Special files

**Definition:**

Convention filenames in `app/` that map to framework features — layouts, loading states, errors, and API routes.

| File            | Purpose                                                           |
| --------------- | ----------------------------------------------------------------- |
| `page.tsx`      | Unique UI for a route segment; makes route publicly accessible    |
| `layout.tsx`    | Shared UI wrapper; persists state across navigations              |
| `template.tsx`  | Like layout but **re-mounts** on navigation (fresh state/effects) |
| `loading.tsx`   | Instant loading UI via Suspense boundary                          |
| `error.tsx`     | Client error boundary for segment                                 |
| `not-found.tsx` | UI when `notFound()` is called                                    |
| `route.ts`      | Route Handler (API endpoint)                                      |
| `default.tsx`   | Fallback for parallel route slots                                 |

**Examples:**

```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div className="flex">
      <aside>Sidebar</aside>
      <main>{children}</main>
    </div>
  );
}
```

**Thoughts:** `layout.tsx` state survives client navigations; `template.tsx` does not. Use `template` for enter animations or per-navigation analytics keys.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions](https://nextjs.org/docs/app/api-reference/file-conventions)

</details>

<details>
  <summary>Route segments</summary>

### Route segments

**Definition:**

URL path segments created by folder names in `app/` — static, dynamic, catch-all, and route groups.

| Pattern            | URL                    | Example                                |
| ------------------ | ---------------------- | -------------------------------------- |
| Static             | `/blog`                | `app/blog/page.tsx`                    |
| Dynamic            | `/blog/hello`          | `app/blog/[slug]/page.tsx`             |
| Catch-all          | `/docs/a/b/c`          | `app/docs/[...slug]/page.tsx`          |
| Optional catch-all | `/shop` or `/shop/a/b` | `app/shop/[[...slug]]/page.tsx`        |
| Route group        | No URL impact          | `app/(auth)/login/page.tsx` → `/login` |

**Sources:**

[nextjs.org/docs/app/api-reference/file-conventions](https://nextjs.org/docs/app/api-reference/file-conventions)

---

</details>

## 3. File-based routing

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Dynamic routes</summary>

### Dynamic routes

**Definition:**

Bracket syntax in folder names creates dynamic URL segments. Access values via `params` (async in Next.js 16).

| Pattern       | URL                  | `params`                                   |
| ------------- | -------------------- | ------------------------------------------ |
| `[id]`        | `/products/42`       | `{ id: '42' }`                             |
| `[...slug]`   | `/docs/a/b`          | `{ slug: ['a', 'b'] }`                     |
| `[[...slug]]` | `/shop` or `/shop/a` | `{ slug: undefined }` or `{ slug: ['a'] }` |

**Examples:**

```tsx
export default async function ProductPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  const product = await getProduct(id);
  return <h1>{product.name}</h1>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes](https://nextjs.org/docs/app/api-reference/file-conventions/dynamic-routes)

</details>

<details>
  <summary>generateStaticParams</summary>

### generateStaticParams

**Definition:** Pre-renders dynamic routes at build time. Returns list of param objects.

**Pluses and minuses:**

| +                                 | −                                             |
| --------------------------------- | --------------------------------------------- |
| SSG for dynamic paths             | Build time scales with param count            |
| Great for blogs, product catalogs | Must handle new slugs (fallback or on-demand) |

**Examples:**

```tsx
export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return posts.map((post: { slug: string }) => ({ slug: post.slug }));
}
```

**Thoughts:** Combine with `dynamicParams = false` to 404 unknown slugs, or `true` (default) to generate on first visit.

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-static-params](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

</details>

<details>
  <summary>Parallel routes</summary>

### Parallel routes

**Definition:** Named slots via `@folder` syntax render multiple pages in the same layout simultaneously.

**Pluses and minuses:**

| +                                  | −                                          |
| ---------------------------------- | ------------------------------------------ |
| Independent loading/error per slot | Complex mental model                       |
| Dashboard layouts with side panels | Requires `default.tsx` for unmatched slots |

**Examples:**

```
app/
├── layout.tsx
├── @team/page.tsx
├── @analytics/page.tsx
└── page.tsx
```

```tsx
// app/layout.tsx
export default function Layout({
  children,
  team,
  analytics,
}: {
  children: React.ReactNode;
  team: React.ReactNode;
  analytics: React.ReactNode;
}) {
  return (
    <div>
      {children}
      <div className="grid grid-cols-2">
        {team}
        {analytics}
      </div>
    </div>
  );
}
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/parallel-routes](https://nextjs.org/docs/app/api-reference/file-conventions/parallel-routes)

</details>

<details>
  <summary>Intercepting routes</summary>

### Intercepting routes

**Definition:**

`(.)` convention intercepts a route to render it inside the current layout — modal pattern.

| Convention | Intercepts    |
| ---------- | ------------- |
| `(.)`      | Same level    |
| `(..)`     | One level up  |
| `(..)(..)` | Two levels up |
| `(...)`    | From root     |

**When to use:** Photo lightboxes, quick-edit modals, share dialogs.

**Examples:**

```
app/
├── @modal/(.)photo/[id]/page.tsx  # Modal overlay
└── photo/[id]/page.tsx            # Full page on direct visit
```

**Thoughts:** Close modal with `router.back()` — not `router.push('/')` — to preserve history stack.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/intercepting-routes](https://nextjs.org/docs/app/api-reference/file-conventions/intercepting-routes)

</details>

<details>
  <summary>URL state — array query parameters</summary>

### URL state — array query parameters

**Definition:** Filter UIs often serialize multi-select state as repeated query keys (e.g. `category_ids[]=1&category_ids[]=2`).

**When to use:** Project filters, faceted search, tag selection.

**Pluses and minuses:**

| +                                   | −                                       |
| ----------------------------------- | --------------------------------------- |
| Shareable/bookmarkable filter state | Encoding complexity (`%5B%5D` for `[]`) |
| Works without client state libs     | Requires `useSearchParams` + Suspense   |

**Examples:**

```tsx
"use client";

import { useRouter } from "next/navigation";
import { useEffect, useState, type ReactNode } from "react";

export function CategoryFilter({ filterKey }: { filterKey: string }) {
  const router = useRouter();
  const [selected, setSelected] = useState<string[]>([]);

  const onCategoryChange = (value: number) => {
    setSelected((prev) =>
      prev.includes(`${value}`)
        ? prev.filter((id) => id !== `${value}`)
        : [...prev, `${value}`],
    );
  };

  useEffect(() => {
    const query = selected.map((id) => `${filterKey}%5B%5D=${id}`).join("&");
    router.push(`/projects?${query}`);
  }, [selected, router, filterKey]);

  return (
    /* render checkboxes calling onCategoryChange */
    <div>{/* … */}</div>
  );
}
```

**Thoughts:** Prefer `URLSearchParams` for encoding instead of manual `%5B%5D`. For complex URL state consider `nuqs` library.

**Sources:** [nextjs.org/docs/app/api-reference/functions/use-search-params](https://nextjs.org/docs/app/api-reference/functions/use-search-params)

</details>

<details>
  <summary>Route groups</summary>

### Route groups

**Definition:** Folders wrapped in `(name)` organize routes without affecting the URL.

**When to use:** Separate marketing vs app layouts, auth vs dashboard route trees.

**Examples:**

```
app/
├── (marketing)/
│   ├── layout.tsx    # Marketing layout
│   └── about/page.tsx
└── (app)/
    ├── layout.tsx    # App shell with sidebar
    └── dashboard/page.tsx
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/route-groups](https://nextjs.org/docs/app/api-reference/file-conventions/route-groups)

</details>

<details>
  <summary>Linking and prefetch</summary>

### Linking and prefetch

**Definition:** `<Link>` from `next/link` enables client-side navigation with automatic prefetching of static routes.

**Examples:**

```tsx
import Link from "next/link";

<Link href="/blog/my-post" prefetch={true}>
  Read post
</Link>;
```

**Thoughts:**

At build time, ensure trailing slash consistency via `trailingSlash` in `next.config.ts` — mismatched slashes cause 404s or duplicate routes.

---

**Sources:** [nextjs.org/docs/app/getting-started/linking-and-navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating)

</details>

<a name="4-special-files"></a>

## 4. Special files

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>error.tsx</summary>

### error.tsx

**Definition:** Client Component error boundary for a route segment. Catches errors in `page.tsx` and children during rendering.

**Pluses and minuses:**

| +                             | −                                                  |
| ----------------------------- | -------------------------------------------------- |
| Granular error UI per section | Must be `'use client'`                             |
| `reset()` re-attempts render  | Does not catch errors in same-segment `layout.tsx` |

**Examples:**

```tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}
```

**Sources:** [nextjs.org/docs/app/getting-started/error-handling](https://nextjs.org/docs/app/getting-started/error-handling)

</details>

<details>
  <summary>not-found.tsx</summary>

### not-found.tsx

**Definition:** UI shown when `notFound()` is called from a Server Component or Route Handler.

**Examples:**

```tsx
import { notFound } from "next/navigation";

export default async function ProductPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  const product = await getProduct(id);
  if (!product) notFound();
  return <div>{product.name}</div>;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/not-found](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)

</details>

<details>
  <summary>forbidden.tsx / unauthorized.tsx</summary>

### forbidden.tsx / unauthorized.tsx

**Definition:** Auth interrupt pages triggered by `forbidden()` and `unauthorized()` helpers (requires `authInterrupts: true` in config).

**When to use:** Role-based access — 403 vs 401 semantics.

**Examples:**

```tsx
import { unauthorized } from "next/navigation";

export default async function AdminPage() {
  const session = await getSession();
  if (!session) unauthorized();
  return <AdminDashboard />;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/forbidden](https://nextjs.org/docs/app/api-reference/functions/forbidden)

</details>

<details>
  <summary>global-error.tsx</summary>

### global-error.tsx

**Definition:** Root-level error boundary wrapping the entire `html`/`body`. Last resort for root layout errors.

**Examples:**

```tsx
"use client";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <html lang="en">
      <body>
        <h2>Something went wrong</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  );
}
```

**Thoughts:** Must include own `<html>` and `<body>` tags.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/error](https://nextjs.org/docs/app/api-reference/file-conventions/error)

</details>

<details>
  <summary>Redirect helpers</summary>

### Redirect helpers

**Definition:**

Navigation helpers that throw special errors to trigger redirects or 404 responses during Server Component render.

| Function                 | HTTP          | Use case               |
| ------------------------ | ------------- | ---------------------- |
| `redirect(url)`          | 307 Temporary | Post-mutation redirect |
| `permanentRedirect(url)` | 308 Permanent | URL migration          |
| `notFound()`             | 404           | Missing resource       |

**Examples:**

```tsx
import { redirect } from "next/navigation";

export default async function Page() {
  const session = await getSession();
  if (!session) redirect("/login");
  return <Dashboard />;
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/redirect](https://nextjs.org/docs/app/api-reference/functions/redirect)

</details>

<details>
  <summary>unstable_rethrow</summary>

### unstable_rethrow

**Definition:** Re-throws Next.js internal errors (`redirect`, `notFound`) from catch blocks — otherwise they are swallowed.

**Examples:**

```tsx
import { unstable_rethrow } from "next/navigation";

export default async function Page() {
  try {
    const data = await fetchData();
    return <div>{data}</div>;
  } catch (error) {
    unstable_rethrow(error);
    return <div>Failed to load</div>;
  }
}
```

---

**Sources:** [nextjs.org/docs/app/api-reference/functions/unstable_rethrow](https://nextjs.org/docs/app/api-reference/functions/unstable_rethrow)

</details>

<a name="5-navigation"></a>

## 5. Navigation

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>next/link</summary>

### next/link

**Definition:** Client-side navigation component with automatic prefetch for visible links in production.

**Pluses and minuses:**

| +                        | −                               |
| ------------------------ | ------------------------------- |
| Fast SPA-like navigation | Prefetch can increase bandwidth |
| Scroll restoration       | External URLs need plain `<a>`  |

**Examples:**

```tsx
import Link from 'next/link';

<Link href="/about">About</Link>
<Link href="/blog/post-1" replace>Replace history</Link>
<Link href="/docs" scroll={false}>No scroll to top</Link>
```

**Sources:** [nextjs.org/docs/app/api-reference/components/link](https://nextjs.org/docs/app/api-reference/components/link)

</details>

<details>
  <summary>Navigation hooks</summary>

### Navigation hooks

**Definition:**

Client-side hooks from `next/navigation` (App Router). Do not use `next/router` — that is Pages Router only.

| Hook                | Returns                                               |
| ------------------- | ----------------------------------------------------- |
| `useRouter()`       | `{ push, replace, refresh, back, forward, prefetch }` |
| `usePathname()`     | Current pathname string                               |
| `useSearchParams()` | `URLSearchParams` — requires Suspense                 |
| `useParams()`       | Dynamic route params object                           |

**Examples:**

```tsx
"use client";

import { useRouter, usePathname, useSearchParams } from "next/navigation";

export function Nav() {
  const router = useRouter();
  const pathname = usePathname();
  const searchParams = useSearchParams();

  return (
    <nav>
      <span>
        Current: {pathname}?{searchParams.toString()}
      </span>
      <button onClick={() => router.push("/dashboard")}>Dashboard</button>
      <button onClick={() => router.refresh()}>Refresh server data</button>
    </nav>
  );
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/use-router](https://nextjs.org/docs/app/api-reference/functions/use-router)

</details>

<details>
  <summary>Programmatic navigation</summary>

### Programmatic navigation

**Definition:** Imperative client-side navigation via `useRouter()` methods — push, replace, back, and refresh.

**Examples:**

```tsx
router.push("/projects?page=2"); // Navigate with history entry
router.replace("/login"); // Replace current entry
router.back(); // History back
router.refresh(); // Re-fetch RSC for current route
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/use-router](https://nextjs.org/docs/app/api-reference/functions/use-router)

</details>

<details>
  <summary>trailingSlash</summary>

### trailingSlash

**Definition:** `next.config.ts` option controlling whether URLs require trailing slashes.

**Examples:**

```ts
const nextConfig = { trailingSlash: true }; // /about/ instead of /about
```

**Thoughts:**

Be consistent — mixed links (`/about` vs `/about/`) cause duplicate content and broken relative paths. When `trailingSlash: true`, always include `/` in `href` and `router.push` paths.

---

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js/trailingSlash](https://nextjs.org/docs/app/api-reference/config/next-config-js/trailingSlash)

</details>

**See also:** Metadata API, sitemaps, robots, JSON-LD, and accessibility → [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md).

<a name="14-built-in-optimizations"></a>
