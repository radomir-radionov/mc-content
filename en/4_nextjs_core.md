# Next.js — Core

Framework model, App Router, and design principles.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [1. Framework overview](#1-framework-overview)
- [2. App vs Pages Router](#2-app-vs-pages-router)
- [3. Project structure](#3-project-structure)
- [4. Design principles](#4-design-principles)

---

## 1. Framework overview

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](#topic-template))._

<details>
  <summary>Next.js overview</summary>

### Next.js overview

**Definition:** Next.js is a **React framework** for building full-stack web applications. You build the UI with React components; Next.js adds routing, rendering, data fetching, and production tooling — bundlers and compilers configured so you focus on the product, not the stack.

**When to use:**

- Marketing sites, blogs, and documentation with SEO requirements
- E-commerce, dashboards, and SaaS with authenticated and public pages
- BFF (Backend for Frontend) — colocate API with UI
- Teams deploying on Vercel or needing standalone Docker output
- Apps benefiting from RSC (smaller client bundles, server data access)

**When not to use:**

- Pure static sites with minimal JS — consider **Astro**
- Mobile apps — use **Expo / React Native**
- Heavy custom Node server (long-lived WebSockets hub) — dedicated backend + Vite SPA may fit better
- Content-only sites where React is unnecessary overhead

**Pluses and minuses:**

| +                                                               | −                                                                    |
| --------------------------------------------------------------- | -------------------------------------------------------------------- |
| Batteries-included: routing, SSR, API, image/font optimization  | Opinionated — escape hatches exist but add complexity                |
| React Server Components + streaming out of the box (App Router) | Steep caching mental model; easy to ship stale or over-dynamic pages |
| Excellent Vercel integration (preview deploys, CDN, analytics)  | Vendor affinity when using Vercel-specific features                  |
| SEO-friendly by default (SSR/SSG/ISR/PPR)                       | Cold starts on serverless; not ideal for all workloads               |
| TypeScript-first, large ecosystem, strong hiring signal         | Two routers (App + Pages) — legacy codebases carry migration debt    |
| Automatic code splitting per route                              | Build times grow with app size                                       |

**Examples:**

```bash
npx create-next-app@latest my-app --typescript --app --turbopack
cd my-app && npm run dev
```

```tsx
// app/page.tsx — Server Component by default
export default async function HomePage() {
  const res = await fetch("https://api.example.com/posts", {
    next: { revalidate: 60 },
  });
  const posts = await res.json();

  return (
    <main>
      <h1>Posts</h1>
      <ul>
        {posts.map((p: { id: string; title: string }) => (
          <li key={p.id}>{p.title}</li>
        ))}
      </ul>
    </main>
  );
}
```

**Thoughts:** Next.js is **not** a replacement for React knowledge — it is a layer on top. Interview framing: React = library (view); Next.js = framework (routing, rendering, data, deployment). New projects should default to **App Router** (Next.js 13+). Pages Router is maintenance mode for existing apps.

**Sources:** [nextjs.org/docs](https://nextjs.org/docs)

</details>

<details>
  <summary>React vs Next.js</summary>

### React vs Next.js

**Definition:**

React renders UI; Next.js orchestrates how and where that UI is built, cached, and delivered.

|                   | React                     | Next.js                                          |
| ----------------- | ------------------------- | ------------------------------------------------ |
| **Role**          | UI library                | Full-stack framework                             |
| **Routing**       | Bring your own            | File-based `app/` directory                      |
| **Data fetching** | Client hooks / manual SSR | Server Components, `fetch` cache, Server Actions |
| **Rendering**     | CSR by default (Vite)     | SSR, SSG, ISR, PPR configurable per route        |
| **API**           | Separate backend          | Route Handlers + Server Actions                  |
| **Deployment**    | Static CDN or custom      | Optimized for Vercel; `standalone` for Docker    |

**Thoughts:** For generic RSC, hydration, and hook rules see [4_react.md §8 — Server rendering & RSC](./4_react_server.md#8-server-rendering--rsc). Rendering strategies (SSR, SSG, ISR, PPR): [Next.js — Rendering](./4_nextjs_rendering.md). Caching semantics: [Next.js — Data](./4_nextjs_data.md).

**Sources:** [react.dev](https://react.dev/) · [nextjs.org/docs](https://nextjs.org/docs)

</details>

<details>
  <summary>Next.js vs alternatives</summary>

### Next.js vs alternatives

**Definition:**

Comparison of Next.js against other full-stack and meta-framework choices for React and content sites.

| Framework               | Strength                                 | Trade-off                                  |
| ----------------------- | ---------------------------------------- | ------------------------------------------ |
| **Vite + React Router** | Maximum flexibility, fast dev, SPA-first | Manual SSR/RSC setup                       |
| **Remix**               | Web standards, loaders/actions, forms    | Smaller ecosystem than Next                |
| **Astro**               | Content sites, islands architecture      | Less suited for heavy client interactivity |
| **SvelteKit**           | Performance, simplicity                  | Different UI paradigm                      |

**How it works:** SEO, first-load performance, colocated server logic, team standardization on Vercel.

**Sources:** [nextjs.org/docs/app](https://nextjs.org/docs/app)

</details>

<details>
  <summary>History (compressed)</summary>

### History (compressed)

**Definition:**

Timeline of major Next.js releases and routing/rendering milestones.

| Version  | Milestone                                                      |
| -------- | -------------------------------------------------------------- |
| **2016** | Next.js 1 — Pages Router, SSR                                  |
| **v9**   | API Routes (`pages/api`)                                       |
| **v12**  | Middleware, React 18 support                                   |
| **v13**  | App Router (beta), RSC, `app/` directory                       |
| **v14**  | App Router stable, partial prerendering experimental           |
| **v15**  | Async request APIs (`cookies`, `headers`, `params`)            |
| **v16**  | Cache Components, `proxy.ts`, `use cache`, async APIs required |

---

**Sources:** [nextjs.org/blog](https://nextjs.org/blog)

</details>

<a name="2-app-vs-pages-router"></a>

### Topic template

Every major topic in `<details>` sections follows:

**Definition** → **How it works** → **When to use** → **Pluses and minuses** → **Examples** → **Thoughts** → **Sources**

---

<a name="1-framework-overview"></a>

### Primary references

**Definition:**

Official documentation and platform guides for Next.js and Vercel.

- [nextjs.org/docs](https://nextjs.org/docs) — official documentation
- [nextjs.org/docs/llms.txt](https://nextjs.org/docs/llms.txt) — full docs index for LLMs
- [Vercel Next.js framework guide](https://vercel.com/docs/frameworks/full-stack/nextjs)
- [Partial Prerendering on Vercel](https://vercel.com/docs/partial-prerendering)
- [Vercel Runtime Cache](https://vercel.com/docs/caching/runtime-cache)
- [Vercel Routing Middleware](https://vercel.com/docs/routing-middleware)

**Sources:** [nextjs.org/docs](https://nextjs.org/docs)

</details>

<details>
  <summary>Deep dives</summary>

## 2. App vs Pages Router

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

## 3. Project structure

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

## 4. Design principles

<details>
  <summary>SOLID in Next.js</summary>

### SOLID in Next.js

**Definition:** The five **SOLID** principles applied to **Next.js modules, Server/Client components, Route Handlers, and `lib/` helpers** — not only `class`.

| Principle                     | Meaning                         | Next.js example                                                                                |
| ----------------------------- | ------------------------------- | ---------------------------------------------------------------------------------------------- |
| **S** — Single Responsibility | One reason to change per module | `page.tsx` composes · `queries.ts` reads · `actions.ts` mutates                                |
| **O** — Open/Closed           | Extend via new code, not edits  | Add `applePay` provider — don't edit `checkout()` for every payment type                       |
| **L** — Liskov Substitution   | Subtypes honor parent contract  | Shared layout/page props work for every variant — no sneaky client-only fields on server types |
| **I** — Interface Segregation | Small focused APIs              | Pass `{ readUser }` — not the whole DB client when only reads are needed                       |
| **D** — Dependency Inversion  | Depend on abstractions          | `placeOrder(input, store)` — inject repository; test with a fake `store`                       |

**How it works:**

Each letter matches the **Examples** below — look for `// good` and `// bad`:

1. **S** — split `page.tsx`, `queries.ts`, `actions.ts` — not one file that fetches, validates, emails, and renders.
2. **O** — new payment provider = new function in a map — not a new `if` in `checkout()`.
3. **L** — any layout wrapper must accept the same `children` contract — no variant that breaks streaming or RSC rules.
4. **I** — `loadCheckout()` takes `{ readCart }` — not the entire `db` module.
5. **D** — domain in `lib/` receives `store` / `fetch` — Route Handler wires concrete adapters.

**When to use:** Refactoring fat `page.tsx` files; plugin-style providers (auth, payments, CMS); code review for new App Router features.

**Examples:**

```ts
type OrderInput = { sku: string; qty: number };
type Order = OrderInput & { id: string };
type OrderStore = { save(order: Order): Promise<void> };

// S — good: domain validation separate from route wiring
export function validateOrder(input: OrderInput): Order {
  return { ...input, id: crypto.randomUUID() };
}

export async function placeOrder(input: OrderInput, store: OrderStore) {
  await store.save(validateOrder(input));
}

// S — bad: page.tsx fetches, validates, saves, sends email, renders form
// export default async function CheckoutPage() {
//   const cart = await db.cart.find(...);
//   await sendReceiptEmail(...);
//   return <form>...</form>;
// }

// O — good: add provider without editing checkout
const payProviders = {
  stripe: (amount: number) => ({ ok: true, provider: "stripe", amount }),
  paypal: (amount: number) => ({ ok: true, provider: "paypal", amount }),
};

function checkout(provider: keyof typeof payProviders, amount: number) {
  return payProviders[provider](amount);
}

// O — bad: edit checkout() for every new provider
// function checkout(provider: string, amount: number) {
//   if (provider === "stripe") return payStripe(amount);
//   if (provider === "paypal") return payPaypal(amount);
// }

// L — good: layout always renders children the same way
export default function AppLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}

// L — bad: "layout" that sometimes omits children or throws for certain routes
// export default function BrokenLayout({ children, variant }) {
//   if (variant === "marketing") throw new Error("No layout");
//   return <div>{children}</div>;
// }

// I — good: query helper needs only readUser
type UserReader = { readUser(id: string): Promise<{ id: string; name: string }> };

export async function loadProfile(id: string, users: UserReader) {
  return users.readUser(id);
}

// I — bad: pass whole db client into a function that only reads one row
// export async function loadProfile(id: string, db: DbClient) {
//   return db.user.findUnique({ where: { id } });
// }

// D — good: inject store in lib/ — test with fake
export async function placeOrder(input: OrderInput, store: OrderStore) {
  await store.save(validateOrder(input));
}
// placeOrder(input, { save: async () => {} });

// D — bad: lib/ imports Prisma directly — hard to test, locked to one DB
// import { prisma } from "@/lib/prisma";
// export async function placeOrder(input: OrderInput) {
//   await prisma.order.create({ data: validateOrder(input) });
// }
```

```tsx
// S — good: page composes; queries live elsewhere
import { CheckoutForm } from "./checkout-form";
import { loadCheckout } from "./queries";

export default async function CheckoutPage() {
  const checkout = await loadCheckout();
  return <CheckoutForm initial={checkout} />;
}
```

**Thoughts:** SOLID is a **guide**, not a law — a one-off marketing page does not need five interfaces. Apply when change pain or test pain appears. React mapping: [SOLID in React](./4_react_core.md#solid-in-react). Express mapping: [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Full reference: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

</details>
