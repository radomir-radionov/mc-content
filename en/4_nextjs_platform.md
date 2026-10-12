# Next.js — Platform

Proxy, config, Vercel, migration, and debugging.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [1. Proxy & middleware](#1-proxy-and-middleware)
- [2. Configuration](#2-configuration)
- [3. Vercel deployment](#3-vercel-deployment)
- [4. Migration & pitfalls](#4-migration-and-pitfalls)
- [5. Debugging](#5-debugging)

---

<a name="1-proxy-and-middleware"></a>

## 1. Proxy & middleware

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Three "middleware" concepts</summary>

### Three "middleware" concepts

**Definition:**

The Next.js/Vercel ecosystem has **three distinct request interception layers**. Confusing them causes security holes, double redirects, and "works locally, fails in prod" bugs.

| Layer                                | File                           | Runtime            | Runs when                      | Scope                        |
| ------------------------------------ | ------------------------------ | ------------------ | ------------------------------ | ---------------------------- |
| **Next.js 16 Proxy**                 | `proxy.ts` (root or `src/`)    | Node.js only       | Before Next.js routing         | Next.js app network boundary |
| **Vercel Routing Middleware**        | `middleware.ts` (project root) | Edge / Node / Bun  | Before cache, at platform edge | Any framework on Vercel      |
| **Pages Router middleware (legacy)** | `middleware.ts` in Next 16     | Deprecated warning | Same as old middleware         | Migrate to `proxy.ts`        |

**How it works:**

```
Client
  → Vercel Routing Middleware (optional, platform)
  → Next.js proxy.ts (optional, Node)
  → Full Route Cache / CDN
  → Next.js render (RSC, Route Handlers)
  → Response
```

**When to use:**

- **proxy.ts** — Next.js-specific rewrites, admin path guards, bot filtering at app boundary
- **Vercel middleware** — cross-framework edge logic, geo headers, A/B at CDN edge
- **Neither alone for auth** — always validate in Server Components / Server Actions / Route Handlers

**When not to use:**

- Heavy DB lookups or session validation only in proxy — use server data layer
- Sole auth gate in middleware — **CVE-2025-29927** showed `x-middleware-subrequest` header could bypass middleware-only checks on affected versions

**Pluses and minuses:**

| +                                | −                                                 |
| -------------------------------- | ------------------------------------------------- |
| Early redirects save render cost | Tempting to put all auth here                     |
| Geo/locale routing at edge       | Node proxy cannot use Edge-only APIs              |
| Header injection before cache    | Two layers can conflict if both rewrite same path |

**Examples:**

```ts
// proxy.ts — coarse gate only
export function proxy(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith("/admin")) {
    if (!request.cookies.get("session")) {
      return NextResponse.redirect(new URL("/login", request.url));
    }
  }
  return NextResponse.next();
}
```

```tsx
// app/admin/page.tsx — real authorization
export default async function AdminPage() {
  const session = await getSession();
  if (!session || session.role !== "admin") forbidden();
  return <AdminDashboard />;
}
```

**Thoughts:**

- Keep proxy/middleware **fast** — no N+1 DB, no external API calls if avoidable.
- `matcher` config limits which paths run the interceptor — exclude `_next/static`, images, favicon.
- Patch Next.js to fixed versions for known middleware bypass advisories.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy) · [vercel.com/docs/routing-middleware](https://vercel.com/docs/routing-middleware)

</details>

<details>
  <summary>Next.js 16 proxy.ts</summary>

### Next.js 16 proxy.ts

**Definition:** Renamed from `middleware.ts` in Next.js 16. Sits at the network boundary; exported function must be named `proxy`.

**How it works:** `npx @next/codemod@latest middleware-to-proxy`. Edge runtime is **not** supported on `proxy` (Node only). If you need Edge (e.g. some i18n middleware), keep `middleware.ts` until the framework documents an Edge path.

**When to use:** Geo redirects, A/B routing, header injection, bot filtering at edge of Next.js app.

**Pluses and minuses:**

| +                                     | −                                       |
| ------------------------------------- | --------------------------------------- |
| Full Node.js APIs                     | Node.js runtime only — not Edge         |
| Clearer security boundary naming      | Migration required from `middleware.ts` |
| Rewrites, redirects, header injection | Not for heavy business logic            |

**Examples:**

```ts
// proxy.ts (project root or src/proxy.ts)
import { NextRequest, NextResponse } from "next/server";

export function proxy(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith("/admin")) {
    const token = request.cookies.get("session");
    if (!token) {
      return NextResponse.redirect(new URL("/login", request.url));
    }
  }
  return NextResponse.next();
}

export const config = {
  matcher: ["/admin/:path*"],
};
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)

</details>

<details>
  <summary>Vercel Routing Middleware</summary>

### Vercel Routing Middleware

**Definition:** Platform feature running **before the cache** on matching requests. Works with Next.js, SvelteKit, Astro, or any deployed framework.

**When to use:** Cross-framework patterns, geo personalization, edge redirects when not using Next.js proxy.

**Examples:**

```ts
// middleware.ts — Vercel platform middleware
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export default function middleware(request: NextRequest) {
  const country = request.geo?.country ?? "US";
  const response = NextResponse.next();
  response.headers.set("x-user-country", country);
  return response;
}
```

**Sources:** [vercel.com/docs/routing-middleware](https://vercel.com/docs/routing-middleware)

</details>

<details>
  <summary>Matcher config</summary>

### Matcher config

**Definition:** `config.matcher` patterns in `proxy.ts` / middleware limiting which request paths run the interceptor.

**Examples:**

```ts
export const config = {
  matcher: [
    "/dashboard/:path*",
    "/((?!api|_next/static|_next/image|favicon.ico).*)",
  ],
};
```

---

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)

</details>

<a name="2-configuration"></a>

## 2. Configuration

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>next.config.ts</summary>

### next.config.ts

**Definition:**

Central configuration for build, routing, images, caching, and experimental features.

| Option                               | Purpose                                 |
| ------------------------------------ | --------------------------------------- |
| `output: 'standalone'`               | Docker-friendly self-contained build    |
| `output: 'export'`                   | Static HTML export (no server features) |
| `cacheComponents: true`              | Enable Cache Components / PPR           |
| `images.remotePatterns`              | Allow remote image domains              |
| `rewrites` / `redirects` / `headers` | URL and header rules                    |
| `serverActions.bodySizeLimit`        | Max Server Action payload               |
| `typedRoutes: true`                  | Type-safe `href` in `Link`              |
| `reactCompiler: true`                | Enable React Compiler                   |
| `trailingSlash`                      | Enforce trailing slashes in URLs        |
| `authInterrupts: true`               | Enable `forbidden()` / `unauthorized()` |

**Examples:**

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  cacheComponents: true,
  typedRoutes: true,
  images: {
    remotePatterns: [{ protocol: "https", hostname: "**.example.com" }],
  },
  experimental: {
    reactCompiler: true,
  },
};

export default nextConfig;
```

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js](https://nextjs.org/docs/app/api-reference/config/next-config-js)

</details>

<details>
  <summary>output modes</summary>

### output modes

**Definition:**

The `output` option in `next.config.ts` controls the **shape of the production build** — default server bundle, self-contained Docker standalone, or fully static HTML export.

| Mode           | Config                 | Deploy target                     | Server features                                                 |
| -------------- | ---------------------- | --------------------------------- | --------------------------------------------------------------- |
| **Default**    | (omit `output`)        | Vercel, `next start`, Node PaaS   | SSR, ISR, Server Actions, Route Handlers                        |
| **Standalone** | `output: 'standalone'` | Docker, Kubernetes, VPS           | Same as default — minimal `node server.js` bundle               |
| **Export**     | `output: 'export'`     | S3, GitHub Pages, any static host | **None** — no SSR, no Server Actions, no dynamic Route Handlers |

**When to use:**

- **Default** — almost all App Router apps on Vercel or Node hosting
- **`standalone`** — Docker/K8s self-hosting with `output: 'standalone'` + copy `public` and `.next/static`
- **`export`** — marketing microsites with zero server logic, or pre-rendered docs only

**When not to use:**

- `export` for dashboards, auth, Server Actions, or webhooks — use default or standalone
- `standalone` when deploying to Vercel — Vercel uses its own output pipeline

**Pluses and minuses:**

| +                                              | −                                                             |
| ---------------------------------------------- | ------------------------------------------------------------- |
| Standalone trims `node_modules` to traced deps | Standalone still needs Node process — not "static files only" |
| Export is cheapest hosting                     | Export cannot use `cookies()`, ISR, or most dynamic APIs      |
| Default zero-config on Vercel                  | Wrong mode → build passes but features silently break         |

**Examples:**

```ts
// next.config.ts
const nextConfig = { output: "standalone" };
export default nextConfig;
```

```dockerfile
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/static ./.next/static
EXPOSE 3000
CMD ["node", "server.js"]
```

```ts
const nextConfig = { output: "export", images: { unoptimized: true } };
```

**Thoughts:**

- `next export` CLI removed — set `output: 'export'` in config and run `next build`.
- Static export requires `generateStaticParams` for all dynamic routes (or no dynamic routes).
- Multi-instance ISR on self-hosted standalone needs custom [`cacheHandlers`](#cachehandlers-nextjs-16).

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js/output](https://nextjs.org/docs/app/api-reference/config/next-config-js/output)

</details>

<details>
  <summary>Edge vs Node runtime</summary>

<a name="edge-vs-node-runtime"></a>

### Edge vs Node runtime

**Definition:**

Next.js can run a route on **`nodejs`** (default, full Node APIs) or the **Edge runtime** (V8 isolate, Web APIs). For **new apps, do not choose Edge for SSR**. Next.js documents `export const runtime = 'edge'` as **deprecated** — delete it; Node is the default.

|                 | Node.js runtime                              | Edge runtime                                                                              |
| --------------- | -------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Status**      | Default. Recommended for SSR + DB            | Deprecated for new route rendering                                                        |
| **APIs**        | `fs`, `crypto`, full `node:*`                | Web standard (`fetch`, `Request`, `crypto.subtle`)                                        |
| **DB drivers**  | Prisma, `pg`, most ORMs                      | HTTP-based only (Neon serverless, Turso HTTP)                                             |
| **Placement**   | Pin **region next to the database**          | Historically "close to the user" — often slower once you round-trip to a single-region DB |
| **Still valid** | App rendering, Server Actions, Fluid compute | Vercel **Routing Middleware** (geo, auth, A/B) — not standalone Edge Functions            |

**How it works:**

Server Actions, Prisma/Drizzle, file uploads, `sharp`, anything importing Node-only packages, **almost all HTML rendering**.

Static/CDN shells, PPR streaming the shell from the edge while compute runs in the DB region, Cloudflare Workers when data is globally distributed.

**Pluses and minuses:**

| +                                             | −                                                                              |
| --------------------------------------------- | ------------------------------------------------------------------------------ |
| Node near data — fewer ocean hops to Postgres | Historically slower cold starts (Fluid compute / bytecode cache improved this) |
| Full npm + native addons                      |                                                                                |
| Edge middleware — cheap geo/JWT at the PoP    | Edge SSR + distant DB often **worse** TTFB than regional Node                  |

**Examples:**

```tsx
// app/api/hello/route.ts — Node.js default
export async function GET() {
  return Response.json({ message: "Hello" });
}
```

**Thoughts:**

- Vercel: standalone Edge Functions deprecated; use Vercel Functions (Node) + optional Routing Middleware.
- Architecture interview frame: [Edge rendering vs data locality](./0_system_design_architecture.md#edge-rendering-vs-data-locality).
- Next.js 16 `proxy.ts` runs **Node only**. Use Vercel Routing Middleware for edge interception.
- Test leftover Edge routes separately — many npm packages fail at bundle time.

**Sources:** [Edge runtime deprecated](https://nextjs.org/docs/messages/edge-runtime-deprecated) · [Vercel Functions / Edge](https://vercel.com/docs/functions/runtimes/edge)

</details>

<details>
  <summary>cacheHandlers (Next.js 16)</summary>

### cacheHandlers (Next.js 16)

**Definition:** Plural `cacheHandlers` replaces deprecated singular `cacheHandler` for custom cache storage in multi-instance deployments.

**When to use:**

Self-hosted ISR across multiple Node instances — implement shared cache backend (Redis, etc.).

---

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js/cacheHandlers](https://nextjs.org/docs/app/api-reference/config/next-config-js/cacheHandlers)

</details>

<a name="3-vercel-deployment"></a>

## 3. Vercel deployment

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Deployment model</summary>

### Deployment model

**Definition:**

Vercel is the **native platform** for Next.js — Git push triggers build, every PR gets a preview URL, merge to main deploys production. The framework and platform are co-designed for ISR, Edge, and RSC.

| Stage          | What happens                                                   |
| -------------- | -------------------------------------------------------------- |
| **Git push**   | Webhook triggers Vercel build (`next build`)                   |
| **Preview**    | Unique URL per branch/PR — same env vars scoped to Preview     |
| **Production** | Atomic promotion on merge — CDN + serverless functions updated |
| **Rollback**   | Instant revert to previous deployment in dashboard             |

| Feature                        | Benefit                                                                                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Preview deployments**        | QA and design review on real infrastructure before merge                                                                  |
| **Git integration**            | Auto-deploy; status checks on PRs                                                                                         |
| **Edge Network**               | Global CDN for static assets and cached Full Route HTML                                                                   |
| **Fluid Compute**              | Shared serverless pool — reduced cold starts vs classic functions                                                         |
| **Analytics / Speed Insights** | Core Web Vitals without manual instrumentation — [Core Web Vitals](./4_nextjs_optimizations.md#core-web-vitals-on-vercel) |
| **Environment scoping**        | Different `DATABASE_URL` for Preview vs Production                                                                        |

**When to use:**

Default choice for Next.js teams wanting managed infra, preview URLs, and minimal DevOps.

Strict on-prem-only requirements — use [`standalone` Docker](#output-modes) + custom CDN; or regulated environments needing full infra control.

**Pluses and minuses:**

| +                                                      | −                                              |
| ------------------------------------------------------ | ---------------------------------------------- |
| Zero-config Next.js features (ISR, images, middleware) | Vendor affinity for advanced platform features |
| Instant rollbacks and previews                         | Cost scales with traffic and function duration |
| Automatic HTTPS and CDN                                | Self-hosting requires more ops for parity      |

**Examples:**

```bash
git checkout -b feature/checkout
# push → Vercel comments preview URL on PR
# merge → production deploy at your-domain.com
```

**Thoughts:**

- Build command defaults to `next build`; output directory `.next` (Vercel handles internally).
- Server Components and Server Actions run as **serverless functions** unless configured otherwise.
- Pair with [Serverless constraints](#serverless-constraints) for DB pooling and timeout limits.
- Self-hosting parity: see [Self-hosting](#self-hosting) and [`cacheHandlers`](#cachehandlers-nextjs-16).

**Sources:** [vercel.com/docs/frameworks/full-stack/nextjs](https://vercel.com/docs/frameworks/full-stack/nextjs)

</details>

<details>
  <summary>Environment variables</summary>

### Environment variables

**Definition:** `NEXT_PUBLIC_*` exposed to browser; all others server-only.

**Examples:**

```bash
# .env.local
DATABASE_URL=postgresql://...
NEXT_PUBLIC_API_URL=https://api.example.com
```

| Scope                                             | Access               |
| ------------------------------------------------- | -------------------- |
| Server Components, Route Handlers, Server Actions | All env vars         |
| Client Components                                 | Only `NEXT_PUBLIC_*` |

**Thoughts:** Never put secrets in `NEXT_PUBLIC_`. Use Vercel dashboard for production env vars with environment scoping (Production / Preview / Development).

**Sources:** [nextjs.org/docs/app/guides/environment-variables](https://nextjs.org/docs/app/guides/environment-variables)

</details>

<details>
  <summary>Serverless constraints</summary>

### Serverless constraints

**Definition:**

Runtime limits on Vercel/serverless — no persistent memory, timeouts, cold starts, and DB connection pooling.

| Constraint                        | Implication                                               |
| --------------------------------- | --------------------------------------------------------- |
| No persistent process memory      | In-process LRU caches are useless — use Runtime Cache     |
| Function timeout (plan-dependent) | Long jobs → background queue (Inngest, Trigger.dev)       |
| Cold starts                       | Minimize bundle size; use Fluid Compute                   |
| DB connection limits              | Use connection pooler (PgBouncer, Neon serverless driver) |

**How it works:** `'use cache: remote'`, Vercel Runtime Cache, or external Redis.

**Examples:**

```ts
// BAD — lost between invocations on serverless
import { LRUCache } from "lru-cache";
const cache = new LRUCache({ max: 500 });
```

**Sources:** [vercel.com/docs/functions/runtimes](https://vercel.com/docs/functions/runtimes)

</details>

<details>
  <summary>Self-hosting</summary>

### Self-hosting

**Definition:**

Run Next.js outside Vercel with `output: 'standalone'` Docker image or `next start`.

| Works out of the box            | Needs extra setup                           |
| ------------------------------- | ------------------------------------------- |
| SSR, SSG, API routes            | Multi-instance ISR (custom `cacheHandlers`) |
| Image optimization (with sharp) | CDN cache invalidation                      |
| Standalone Docker               | Edge middleware features                    |

**Examples:**

```dockerfile
FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/static ./.next/static
CMD ["node", "server.js"]
```

**Sources:**

[nextjs.org/docs/app/guides/self-hosting](https://nextjs.org/docs/app/guides/self-hosting)

---

</details>

<a name="4-migration-and-pitfalls"></a>

## 4. Migration & pitfalls

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Pages → App Router checklist</summary>

### Pages → App Router checklist

**Definition:**

Step-by-step migration checklist from Pages Router to App Router conventions.

1. Create `app/layout.tsx` with root HTML structure
2. Migrate pages one route at a time to `app/**/page.tsx`
3. Replace `getServerSideProps` / `getStaticProps` with Server Component `fetch`
4. Replace `pages/api` with `app/**/route.ts`
5. Replace `next/head` with Metadata API
6. Replace `next/router` with `next/navigation`
7. Add `"use client"` only where interactivity is needed
8. Audit caching — default `force-cache` may surprise former SSR pages
9. Migrate `middleware.ts` → `proxy.ts` (Next.js 16)
10. Update `revalidateTag` to two-arg form

**Sources:** [nextjs.org/docs/app/guides/migrating](https://nextjs.org/docs/app/guides/migrating)

</details>

<details>
  <summary>Deprecated APIs</summary>

### Deprecated APIs

**Definition:**

Pages Router and legacy Next.js APIs superseded by App Router patterns.

| Removed / deprecated                      | Migration                       |
| ----------------------------------------- | ------------------------------- |
| `next export` CLI                         | `output: 'export'` in config    |
| `getServerSideProps` / `getStaticProps`   | Server Components + `fetch`     |
| `next/head`                               | `metadata` / `generateMetadata` |
| `next/router`                             | `next/navigation`               |
| `unstable_cache`                          | `'use cache'`                   |
| `revalidateTag(tag)`                      | `revalidateTag(tag, 'max')`     |
| `middleware.ts` (Next 16)                 | `proxy.ts`                      |
| Sync `cookies()` / `headers()` / `params` | `await` all request APIs        |

**Sources:** [nextjs.org/docs/app/guides/migrating](https://nextjs.org/docs/app/guides/migrating)

</details>

<details>
  <summary>Hydration errors</summary>

### Hydration errors

**Definition:**

Server HTML does not match client render — React cannot reconcile.

| Cause                                    | Fix                                                                                               |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `Date.now()` / `Math.random()` in render | Generate in `useEffect` or pass from server                                                       |
| `typeof window !== 'undefined'` branches | Use `useEffect` or Server Component only                                                          |
| Invalid HTML nesting (`<p><div>`)        | Fix HTML structure                                                                                |
| Browser extensions modifying DOM         | Test in incognito                                                                                 |
| Third-party libs assuming browser        | Dynamic import with `ssr: false` — [Bundle pitfalls](./4_nextjs_optimizations.md#bundle-pitfalls) |

**Sources:** [nextjs.org/docs/messages/react-hydration-error](https://nextjs.org/docs/messages/react-hydration-error)

</details>

<details>
  <summary>RSC security</summary>

### RSC security

**Definition:**

React Server Components use the **Flight protocol** — a serialized stream of component output and references sent from server to client. Security issues in RSC packages or misconfiguration can expose server data, enable request forgery, or allow middleware bypass.

| Risk                                                  | Mitigation                                                                                                   |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **RSC package vulnerabilities** (Dec 2025 advisories) | Keep `react`, `react-dom`, `next` on **patched** versions — monitor [react.dev/blog](https://react.dev/blog) |
| **Middleware/proxy bypass** (CVE-2025-29927)          | Patch Next.js; never rely on proxy/middleware as sole auth gate                                              |
| **Secrets in RSC props**                              | Never pass API keys, tokens, or PII in props to Client Components                                            |
| **Server Action abuse**                               | Validate auth + input in every action; configure `allowedOrigins`                                            |
| **Data leaks via Flight**                             | Treat all Client-bound props as public — serialize DTOs only                                                 |

**How it works:**

1. Patch `next`, `react`, `react-dom` promptly on security releases
2. Auth in **Server Component / Server Action**, not proxy alone
3. Map DB entities to plain DTOs before passing to Client Components
4. Use `forbidden()` / `unauthorized()` (with `authInterrupts: true`) for auth errors
5. Audit `NEXT_PUBLIC_*` — browser-visible by definition
6. Review third-party components for `"use client"` + accidental secret imports

**When to use:** Default RSC model — validate at every trust boundary (proxy → Server Component → Server Action).

**Pluses and minuses:**

| +                                          | −                                                                                     |
| ------------------------------------------ | ------------------------------------------------------------------------------------- |
| Server secrets stay server-side by default | Flight payload is opaque — easy to over-share in props                                |
| Built-in Server Action CSRF checks         | Ecosystem still maturing — audit third-party RSC libs                                 |
| `httpOnly` cookies never reach client JS   | Misconfigured caching can serve one user's data to another — use `use cache: private` |

**Examples:**

```tsx
// Server Component — secrets stay here
export default async function BillingPage() {
  const session = await getSession();
  if (!session) unauthorized();

  const invoice = await stripe.invoices.retrieve(session.stripeCustomerId);
  // Pass only what UI needs — not full Stripe object
  return (
    <InvoiceSummary
      id={invoice.id}
      amount={invoice.amount_due}
      status={invoice.status}
    />
  );
}
```

**Thoughts:**

- RSC is **not** a security boundary by itself — authorization logic must run on the server on every request/action.
- Enable Dependabot/Renovate for `next` and `react` — RSC advisories patch quickly.
- See [CSRF with Server Actions](./4_nextjs_data.md#csrf-with-server-actions) and [Authentication patterns](#authentication-patterns).

**Sources:** [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security) · [react.dev/blog](https://react.dev/blog)

</details>

<details>
  <summary>Authentication patterns</summary>

### Authentication patterns

**Definition:**

App Router auth combines **session identity** (cookie or token), **route protection** (layout guards), and **authorization** (role/permission checks in data layer). Managed providers and DIY sessions both follow the same defense-in-depth shape.

| Approach                          | Best for                        | Notes                                               |
| --------------------------------- | ------------------------------- | --------------------------------------------------- |
| **Clerk / Auth0 / Descope**       | Fast SaaS launch, OAuth, MFA    | Provider middleware + server SDK; Vercel-friendly   |
| **Better Auth / Auth.js**         | Self-hosted sessions, custom DB | Session in DB + `httpOnly` cookie                   |
| **Session in httpOnly cookie**    | Full control, BFF pattern       | Validate JWT/session in Server Components + Actions |
| **External IdP + Route Handlers** | Enterprise SSO                  | Token exchange in `route.ts`; store session cookie  |

| Layer             | Responsibility                                      |
| ----------------- | --------------------------------------------------- |
| **proxy.ts**      | Coarse redirect if no session cookie (optional)     |
| **Layout / page** | `getSession()` → `redirect()` or `unauthorized()`   |
| **Server Action** | Re-validate session + permissions on every mutation |
| **Route Handler** | Bearer/cookie auth for webhooks and public API      |

**When to use:**

- **Managed provider** — OAuth social login, MFA, orgs/roles out of the box
- **DIY session** — full control, existing user table, BFF with Next.js only
- **`authInterrupts: true`** + `forbidden()` / `unauthorized()` — semantic 403/401 pages (Next.js 15+)

**When not to use:**

- Storing JWT in `localStorage` for SSR apps — prefer `httpOnly` cookie
- Auth check only in `proxy.ts` — bypass risk; always validate in server data layer

**Pluses and minuses:**

| +                                         | −                                                     |
| ----------------------------------------- | ----------------------------------------------------- |
| httpOnly cookies resist XSS token theft   | Cookie sessions need CSRF awareness on Route Handlers |
| Server-side validation on every render    | JWT in cookie — must verify signature server-side     |
| Managed providers reduce auth boilerplate | Vendor lock-in for advanced auth features             |

**Examples:**

```tsx
// app/(dashboard)/layout.tsx
import { redirect } from "next/navigation";
import { getSession } from "@/lib/auth";

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

```tsx
// lib/auth.ts
import { cookies } from "next/headers";
import { cache } from "react";

export const getSession = cache(async () => {
  const token = (await cookies()).get("session")?.value;
  if (!token) return null;
  return verifySessionToken(token); // throws if invalid
});
```

```tsx
// next.config.ts
const nextConfig = { authInterrupts: true };
```

```tsx
// app/admin/page.tsx
import { forbidden, unauthorized } from "next/navigation";

export default async function AdminPage() {
  const session = await getSession();
  if (!session) unauthorized();
  if (session.role !== "admin") forbidden();
  return <AdminPanel />;
}
```

**Thoughts:**

- Use React `cache()` around `getSession()` — dedupe within one request.
- Server Actions must call `getSession()` again — do not trust client-sent user IDs.
- For OAuth callbacks, use Route Handler (`app/api/auth/callback/route.ts`) to set cookie and redirect.
- Cross-link: [cookies() & headers()](./4_nextjs_rendering.md#cookies-headers), [RSC security](#rsc-security).

**Sources:** [nextjs.org/docs/app/guides/authentication](https://nextjs.org/docs/app/guides/authentication)

</details>

<a name="5-debugging"></a>

## 5. Debugging

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Debug build paths</summary>

### Debug build paths

**Definition:** CLI flag to build a single route path for faster iteration during route-specific debugging.

**When to use:** Iterating on a single route's build without full rebuild.

**Examples:**

```bash
next build --debug-build-paths "/blog/[slug]"
```

**Sources:** [nextjs.org/docs/app/api-reference/cli/next](https://nextjs.org/docs/app/api-reference/cli/next)

</details>

<details>
  <summary>Next.js MCP server</summary>

### Next.js MCP server

**Definition:** Next.js 15+ includes an MCP endpoint for AI-assisted debugging in development.

**Thoughts:** Enable via Next.js dev tooling docs — useful with Cursor and other AI agents for route-level inspection.

**Sources:** [nextjs.org/docs/app/guides/mcp](https://nextjs.org/docs/app/guides/mcp)

</details>

<details>
  <summary>Testing Next.js (Vitest + RTL)</summary>

<a name="testing-nextjs-vitest--rtl"></a>

### Testing Next.js (Vitest + RTL)

**Definition:**

Test Next.js apps with **Vitest** (fast ESM-native runner) and **React Testing Library** (RTL) for component behavior. Why tests / AAA: [Testing — AAA](./7_special_testing_fundamentals.md#tests-and-aaa). Server Components and Server Actions need separate strategies — mock `next/navigation` and test actions as plain async functions. Stub vs mock vs spy: [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies). Runner choice: [Vitest vs Jest](./7_special_testing_tooling.md#vitest-vs-jest).

| What to test      | Tool         | Approach                                  |
| ----------------- | ------------ | ----------------------------------------- |
| Client Components | Vitest + RTL | `render()`, `userEvent`, assert DOM       |
| Server Actions    | Vitest       | Import action; mock `db` / `cookies`      |
| Route Handlers    | Vitest       | Call exported `GET`/`POST` with `Request` |
| E2E flows         | Playwright   | Real browser — login, navigation, forms   |

**When to use:**

- Unit/integration: forms, hooks, Client Components, action validation logic
- E2E: auth flows, checkout, anything crossing Server Action + navigation

**Pluses and minuses:**

| +                                       | −                                                      |
| --------------------------------------- | ------------------------------------------------------ |
| Vitest — fast, Jest-compatible API      | RSC cannot render in jsdom — test Client leaves or E2E |
| RTL — tests user-visible behavior       | Heavy mocking of `next/*` modules                      |
| `next/experimental/testmode` (evolving) | Not a replacement for Playwright on critical paths     |

**Examples:**

```ts
// vitest.config.ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";
import path from "node:path";

export default defineConfig({
  plugins: [react()],
  test: { environment: "jsdom", setupFiles: ["./vitest.setup.ts"] },
  resolve: { alias: { "@": path.resolve(__dirname, "./src") } },
});
```

```ts
// vitest.setup.ts
import "@testing-library/jest-dom/vitest";
```

```tsx
// components/counter.test.tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { Counter } from "./counter";

it("increments on click", async () => {
  render(<Counter />);
  await userEvent.click(screen.getByRole("button", { name: /increment/i }));
  expect(screen.getByText("1")).toBeInTheDocument();
});
```

```tsx
// actions.test.ts — mock server deps
import { vi } from "vitest";
vi.mock("next/cache", () => ({ revalidatePath: vi.fn() }));
vi.mock("@/lib/db", () => ({ todo: { create: vi.fn() } }));

import { createTodo } from "./actions";

it("rejects empty title", async () => {
  const fd = new FormData();
  fd.set("title", "");
  const result = await createTodo(fd);
  expect(result).toEqual({ error: "Invalid title" });
});
```

**Thoughts:**

- Mock `next/navigation`: `vi.mock('next/navigation', () => ({ redirect: vi.fn(), useRouter: () => ({ refresh: vi.fn() }) }))`.
- Prefer testing **Client Component leaves** fed with static props over rendering full RSC trees in unit tests.
- Add Playwright for Server Action form submit + `revalidatePath` visibility. Pyramid + tool map: [Testing — E2E](./7_special_testing_layers.md#e2e-testing).

**Sources:** [nextjs.org/docs/app/guides/testing](https://nextjs.org/docs/app/guides/testing) · [vitest.dev](https://vitest.dev/)

</details>

<a name="20-resources"></a>
