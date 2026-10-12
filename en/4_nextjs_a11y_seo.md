# Next.js — Accessibility, SEO & metadata

Metadata API, crawlability, structured data, and accessible App Router pages.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md)

### Sections

- [1. Metadata API](#1-metadata-api)
- [2. SEO & discovery](#2-seo-discovery)
- [3. Accessibility](#3-accessibility)

---

<a name="1-metadata-api"></a>

## 1. Metadata API

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Static metadata</summary>

### Static metadata

**Definition:** Export a `metadata` object (typed as `Metadata` from `next`) from `layout.tsx` or `page.tsx`. Next.js turns it into `<title>`, `<meta>`, and `<link>` tags in the document `<head>`. Replaces Pages Router `next/head`.

**How it works:**

- **Static export** — evaluated at build time for static routes; for dynamic routes, combined with `generateMetadata` on the same segment.
- **Segment tree** — each layout and page can export metadata; child segments **merge** with parents (see [Metadata merging](#metadata-merging)).
- **Common fields** — `title`, `description`, `keywords`, `authors`, `creator`, `publisher`, `formatDetection`, `metadataBase`, `alternates`, `openGraph`, `twitter`, `robots`, `verification`, `icons`, `manifest`, `category`, `classification`, `other` (escape hatch for custom tags).

**When to use:** Fixed marketing copy, docs section defaults, and root layout branding shared by many routes.

**Pluses and minuses:**

| +                                              | −                                                                              |
| ---------------------------------------------- | ------------------------------------------------------------------------------ |
| Type-safe `Metadata` object                    | Nested objects like `openGraph` can be overwritten wholesale in child segments |
| Colocated with the route that owns the content | Does not replace visible on-page headings for a11y/SEO                         |
| Works in Server Components only (no hook)      | Preview/staging need correct `metadataBase` per deploy                         |

**Examples:**

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "My App",
  description: "Full-stack React on Next.js",
  applicationName: "My App",
  referrer: "origin-when-cross-origin",
  openGraph: {
    title: "My App",
    description: "Full-stack React on Next.js",
    url: "https://myapp.com",
    siteName: "My App",
    images: [{ url: "https://myapp.com/og.png", width: 1200, height: 630 }],
    locale: "en_US",
    type: "website",
  },
  twitter: {
    card: "summary_large_image",
    title: "My App",
    description: "Full-stack React on Next.js",
    images: ["https://myapp.com/og.png"],
  },
};
```

**Thoughts:** Social previews need consistent `openGraph` and `twitter` (many crawlers read OG first). Visible `<h1>` and body copy still matter for users and search quality — metadata alone is not the page.

**Sources:** [nextjs.org/docs/app/getting-started/metadata-and-og-images](https://nextjs.org/docs/app/getting-started/metadata-and-og-images) · [nextjs.org/docs/app/api-reference/functions/generate-metadata](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)

</details>

<details>
  <summary>Title template</summary>

### Title template

**Definition:** Nested `title` object with `default`, `template`, and `absolute` controls how segment titles compose across the layout tree.

**How it works:**

- **`title.template`** on a parent layout — child string titles are inserted where `%s` appears (e.g. `"%s | My Site"`).
- **`title.default`** — fallback when a child does not set a title.
- **`title.absolute`** — skips parent templates; use for one-off pages (legal, error) that must not inherit branding suffix.

**When to use:** Multi-section apps (docs, dashboard + marketing) where every inner page should share a suffix or prefix.

**Examples:**

```tsx
// app/layout.tsx
export const metadata: Metadata = {
  title: {
    default: "My Site",
    template: "%s | My Site",
  },
};

// app/blog/page.tsx
export const metadata: Metadata = {
  title: "Blog", // → <title>Blog | My Site</title>
};

// app/legal/privacy/page.tsx
export const metadata: Metadata = {
  title: { absolute: "Privacy Policy" }, // no template applied
};
```

**Thoughts:** Route announcer and screen readers often use `document.title` after navigation — keep templates readable, not keyword-stuffed.

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-metadata#title](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#title)

</details>

<details>
  <summary>metadataBase</summary>

### metadataBase

**Definition:** `metadataBase: new URL("https://example.com")` on the **root** layout. Relative URLs in metadata fields resolve against this origin.

**How it works:** Applies to `openGraph.images`, `alternates.canonical`, `alternates.languages`, and similar path fields. Normalizes duplicate slashes; does not fix wrong host on preview deployments — set base URL from env per environment.

**When to use:** Any production app using relative paths in metadata. Required for correct absolute OG URLs when you pass `/og.png` instead of a full URL.

**Pluses and minuses:**

| +                                  | −                                                |
| ---------------------------------- | ------------------------------------------------ |
| One place to fix origin per deploy | Wrong `metadataBase` on Vercel preview breaks OG |
| Keeps segment metadata DRY         | Still need real HTTPS and valid image dimensions |

**Examples:**

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  metadataBase: new URL(
    process.env.VERCEL_URL
      ? `https://${process.env.VERCEL_URL}`
      : "http://localhost:3000",
  ),
  openGraph: {
    images: "/og-image.png",
  },
};
```

**Thoughts:** Locale sites pair this with [alternates](#alternates-canonical-and-hreflang). Rendering strategy (static HTML vs client fetch) affects what crawlers index — [Next.js — Rendering](./4_nextjs_rendering.md#1-rendering-strategies).

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-metadata#metadatabase](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#metadatabase)

</details>

<details>
  <summary>alternates (canonical and hreflang)</summary>

### alternates (canonical and hreflang)

**Definition:** `metadata.alternates` emits `<link rel="canonical">`, `hreflang` alternates, optional media-specific URLs, and alternate MIME types (RSS, etc.).

**How it works:** Paths are resolved with `metadataBase` when relative. `languages` map keys become `hreflang` values (`en-US`, `x-default`, etc.).

**When to use:** Duplicate URL prevention, multilingual sites, mobile alternate URLs, feed discovery.

**Examples:**

```tsx
export const metadata: Metadata = {
  metadataBase: new URL("https://acme.com"),
  alternates: {
    canonical: "/products",
    languages: {
      "en-US": "/en-US/products",
      "de-DE": "/de-DE/products",
      "x-default": "/products",
    },
  },
};
```

**Thoughts:** Canonical should match the URL you want indexed. For App Router locale segments (`/[locale]/…`), generate alternates from the same slug list you use in `generateStaticParams`.

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-metadata#alternates](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#alternates)

</details>

<details>
  <summary>generateMetadata</summary>

### generateMetadata

**Definition:** `export async function generateMetadata(props, parent): Promise<Metadata>` on a `page.tsx` or `layout.tsx` for data-driven head tags.

**How it works:**

- Receives **`params`** and **`searchParams`** as Promises (Next.js 15+).
- Second argument **`parent: ResolvingMetadata`** — `await parent` to read merged metadata from parent layouts before overriding.
- On dynamic pages, metadata can be **streamed** so the UI is not blocked waiting for slow CMS calls (still runs on the server).
- Use the same `fetch`/DB access patterns as the page; align cache tags with [Next.js — Data](./4_nextjs_data.md) to avoid double fetches or stale titles.

**When to use:** CMS posts, e-commerce SKUs, user-generated public profiles — any route where title, description, or OG image depend on the URL.

**Pluses and minuses:**

| +                                                   | −                                    |
| --------------------------------------------------- | ------------------------------------ |
| Per-page SEO from CMS or DB                         | Runs on server — watch fetch caching |
| Can spread `parent` openGraph/twitter intentionally | Unhandled errors can break head tags |

**Examples:**

```tsx
import type { Metadata, ResolvingMetadata } from "next";

type Props = { params: Promise<{ slug: string }> };

export async function generateMetadata(
  { params }: Props,
  parent: ResolvingMetadata,
): Promise<Metadata> {
  const { slug } = await params;
  const previous = await parent;
  const post = await getPost(slug);

  return {
    title: post.title,
    description: post.excerpt,
    openGraph: {
      ...previous.openGraph,
      title: post.title,
      description: post.excerpt,
      images: [post.coverImage],
    },
  };
}
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-metadata](https://nextjs.org/docs/app/api-reference/functions/generate-metadata)

</details>

<details>
  <summary>Metadata merging</summary>

### Metadata merging

**Definition:** Metadata from root → nested layouts → page is combined when the app renders a URL.

**How it works:** Top-level keys (e.g. `title`) follow child override rules. **Nested objects are shallow-merged in spirit but redefined keys replace the whole nested object** — if a child sets `openGraph: { title: "Blog" }` without spreading parent `openGraph`, `description` from the parent may be dropped.

**When to use:** Always — plan which layout owns `openGraph` defaults and which pages only patch fields.

**Examples:**

```tsx
// app/layout.tsx
export const metadata: Metadata = {
  openGraph: { siteName: "Acme", type: "website" },
};

// app/blog/layout.tsx — BAD: wipes parent openGraph fields
export const metadata: Metadata = {
  openGraph: { title: "Blog" },
};

// app/blog/layout.tsx — BETTER
export const metadata: Metadata = {
  openGraph: { title: "Blog" }, // still prefer generateMetadata + await parent for dynamic routes
};
```

**Thoughts:** In `generateMetadata`, `const previous = await parent` then spread `previous.openGraph` when overriding one field.

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-metadata#merging](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#merging)

</details>

<details>
  <summary>viewport and generateViewport</summary>

### viewport and generateViewport

**Definition:** Viewport and theme appearance are **not** part of `metadata` anymore. Export `viewport` or `generateViewport` from `layout.tsx` / `page.tsx` (type `Viewport` from `next`).

**How it works:** Emits `<meta name="viewport" …>`, `theme-color`, and `color-scheme` tags. Next.js sets sensible viewport defaults; override only when you have a specific reason (e.g. marketing lock-zoom — weigh against accessibility).

**When to use:** PWA theme colors, dark/light browser chrome, rare layout constraints. Prefer not setting `userScalable: false` unless product explicitly requires it — it harms low-vision users.

**Examples:**

```tsx
import type { Viewport } from "next";

export const viewport: Viewport = {
  width: "device-width",
  initialScale: 1,
  themeColor: [
    { media: "(prefers-color-scheme: light)", color: "#ffffff" },
    { media: "(prefers-color-scheme: dark)", color: "#0a0a0a" },
  ],
  colorScheme: "light dark",
};
```

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-viewport](https://nextjs.org/docs/app/api-reference/functions/generate-viewport)

</details>

<details>
  <summary>metadata.robots</summary>

### metadata.robots

**Definition:** Per-route indexing directives via `metadata.robots` and `metadata.robots.googleBot` — separate from `app/robots.txt` / `robots.ts`.

**How it works:** Renders `<meta name="robots">` and Google-specific bot tags on **that page**. Use for `noindex` on thin pages, thank-you pages, or authenticated views that still render HTML.

**When to use:** Page-level index control. Use `robots.ts` for site-wide crawl rules and sitemap pointer.

**Examples:**

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  robots: {
    index: false,
    follow: true,
    googleBot: {
      index: false,
      follow: true,
      "max-image-preview": "large",
    },
  },
};
```

**Thoughts:** `noindex` in metadata does not replace auth — sensitive routes still need server-side protection.

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-metadata#robots](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#robots)

</details>

<details>
  <summary>File-based metadata</summary>

### File-based metadata

**Definition:** Special files under `app/` that Next.js discovers and wires into routes automatically — static or code-generated.

**How it works:** File name sets the role; optional `size` exports for icons; `.tsx` OG/Twitter files can use `ImageResponse` from `next/og`. Colocated per segment (e.g. `app/blog/opengraph-image.tsx` applies under `/blog`).

| File / pattern                  | Purpose                        |
| ------------------------------- | ------------------------------ |
| `favicon.ico`                   | Favicon                        |
| `icon.png` / `icon.tsx`         | App icon(s)                    |
| `apple-icon.png`                | Apple touch icon               |
| `opengraph-image.*`             | OG image (static or generated) |
| `twitter-image.*`               | Twitter card image             |
| `robots.txt` / `robots.ts`      | Crawler rules                  |
| `sitemap.xml` / `sitemap.ts`    | XML sitemap                    |
| `manifest.json` / `manifest.ts` | Web app manifest               |

**When to use:** Default icons/OG for a subtree without duplicating metadata objects; dynamic OG when share text depends on route params inside the image route.

**Examples:**

```tsx
// app/opengraph-image.tsx
import { ImageResponse } from "next/og";

export const size = { width: 1200, height: 630 };
export const contentType = "image/png";

export default function Image() {
  return new ImageResponse(
    <div
      style={{
        fontSize: 64,
        background: "#000",
        color: "#fff",
        width: "100%",
        height: "100%",
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
      }}
    >
      My App
    </div>,
    { ...size },
  );
}
```

**Thoughts:** Generated images run on the server at request/build time — keep JSX simple for performance. Image optimization for page content: [Next.js — Optimizations](./4_nextjs_optimizations.md).

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/metadata](https://nextjs.org/docs/app/api-reference/file-conventions/metadata)

</details>

<details>
  <summary>manifest.ts</summary>

### manifest.ts

**Definition:** `app/manifest.ts` exports a default function returning `MetadataRoute.Manifest` for installable PWA metadata (`name`, `icons`, `display`, `theme_color`, `start_url`).

**How it works:** Served as the web app manifest; pairs with [viewport](#viewport-and-generateviewport) `themeColor` and icons from file-based metadata.

**When to use:** Add-to-home-screen, standalone display mode, branded install prompts.

**Examples:**

```ts
import type { MetadataRoute } from "next";

export default function manifest(): MetadataRoute.Manifest {
  return {
    name: "My App",
    short_name: "App",
    start_url: "/",
    display: "standalone",
    background_color: "#ffffff",
    theme_color: "#000000",
    icons: [
      { src: "/icon-192.png", sizes: "192x192", type: "image/png" },
      { src: "/icon-512.png", sizes: "512x512", type: "image/png" },
    ],
  };
}
```

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/metadata/manifest](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/manifest)

</details>

<a name="2-seo-discovery"></a>

## 2. SEO & discovery

<details>
  <summary>sitemap.ts</summary>

### sitemap.ts

**Definition:** `app/sitemap.ts` default export returns `MetadataRoute.Sitemap` — an array of `{ url, lastModified?, changeFrequency?, priority?, alternates? }`. Served at `/sitemap.xml`.

**How it works:**

- **Caching** — static by default; dynamic when using request-time data or `export const dynamic = 'force-dynamic'`.
- **Scale** — for large sites, split with sitemap index files or multiple sitemaps (see Next.js docs on `generateSitemaps` / id-based sitemaps when you outgrow a single file).
- **Locales** — include each language URL you want crawled; align with `alternates.languages`.

**When to use:** Public sites where you control the list of indexable URLs (marketing, blog, docs). Combine with `generateStaticParams` for dynamic segments.

**Examples:**

```ts
import type { MetadataRoute } from "next";

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const posts = await getAllPosts();
  const staticRoutes: MetadataRoute.Sitemap = [
    { url: "https://acme.com", lastModified: new Date(), priority: 1 },
    {
      url: "https://acme.com/about",
      changeFrequency: "monthly",
      priority: 0.8,
    },
  ];
  const postRoutes = posts.map((post) => ({
    url: `https://acme.com/blog/${post.slug}`,
    lastModified: post.updatedAt,
    changeFrequency: "weekly" as const,
    priority: 0.6,
  }));
  return [...staticRoutes, ...postRoutes];
}
```

**Thoughts:** `priority` and `changeFrequency` are hints, not guarantees. Staging environments should `noindex` or disallow in `robots.ts`, not publish a sitemap to production crawlers.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap)

</details>

<details>
  <summary>robots.ts</summary>

### robots.ts

**Definition:** `app/robots.ts` returns `MetadataRoute.Robots` with `rules` (single or array per `userAgent`), optional `sitemap`, optional `host`.

**How it works:** Generates `/robots.txt`. Different rules per bot when you pass an array of `{ userAgent, allow, disallow }`.

**When to use:** Global crawl policy — block `/api`, `/admin`, preview deployments, or point all bots to your sitemap URL.

**Examples:**

```ts
import type { MetadataRoute } from "next";

export default function robots(): MetadataRoute.Robots {
  const isProd = process.env.VERCEL_ENV === "production";
  if (!isProd) {
    return { rules: { userAgent: "*", disallow: "/" } };
  }
  return {
    rules: [
      { userAgent: "*", allow: "/", disallow: ["/private/", "/api/"] },
      { userAgent: "Googlebot", allow: "/", disallow: "/private/" },
    ],
    sitemap: "https://acme.com/sitemap.xml",
  };
}
```

**Thoughts:** Complements [metadata.robots](#metadatarobots) on individual pages. Handbook apps often implement both `app/sitemap.ts` and `app/robots.ts` with per-locale URLs — same idea at product scale.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/metadata/robots](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots)

</details>

<details>
  <summary>JSON-LD</summary>

### JSON-LD

**Definition:** [schema.org](https://schema.org) graphs embedded as `<script type="application/ld+json">` in server-rendered HTML for rich results (products, articles, FAQs, breadcrumbs, etc.).

**How it works:** Render in Server Components so the JSON is in the first response. Build the object in TS, then `JSON.stringify` into `dangerouslySetInnerHTML` on the script tag. Keep JSON valid; escape user content to avoid breaking the script or XSS (sanitize strings from CMS).

**When to use:** When Google Search features (rich snippets) matter and on-page content already matches the schema you declare.

**Pluses and minuses:**

| +                                         | −                                        |
| ----------------------------------------- | ---------------------------------------- |
| Clear separation from visible UI          | Invalid schema → no rich result          |
| Works with SSR/SSG defaults in App Router | Must not contradict visible page content |

**Examples:**

```tsx
export default function ProductPage({ product }: { product: Product }) {
  const jsonLd = {
    "@context": "https://schema.org",
    "@type": "Product",
    name: product.name,
    image: product.imageUrl,
    offers: {
      "@type": "Offer",
      price: product.price,
      priceCurrency: "USD",
      availability: "https://schema.org/InStock",
    },
  };

  return (
    <article>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
      />
      <h1>{product.name}</h1>
      <p>{product.description}</p>
    </article>
  );
}
```

**Thoughts:** General SEO patterns (canonical URLs, meta descriptions, heading hierarchy) — [HTML — SEO](./1_html_seo.md). If critical text loads only on the client, crawlers may miss it — [Next.js — Data](./4_nextjs_data.md#client-side-fetching).

**Sources:** [nextjs.org/docs/app/guides/json-ld](https://nextjs.org/docs/app/guides/json-ld)

</details>

<a name="3-accessibility"></a>

## 3. Accessibility

<details>
  <summary>Root layout and document language</summary>

### Root layout and document language

**Definition:** The root `app/layout.tsx` must include `<html>` and `<body>`. Accessibility and SEO both start with a correct document shell: `lang`, title via metadata, logical heading order in `children`.

**How it works:**

- **`lang`** on `<html>` — required for screen reader pronunciation and hyphenation; set per locale in `app/[locale]/layout.tsx` when routing i18n.
- **`dir="rtl"`** — for Arabic, Hebrew, etc., on the same element.
- **Metadata** — export `metadata` / `generateMetadata` for title; do not rely on a lone visual logo with no text title.
- **Landmarks** — use semantic elements (`main`, `nav`, `header`, `footer`) in layouts; details in [HTML — Accessibility](./1_html_a11y.md).

**When to use:** Every App Router app. Locale-aware handbooks should switch `lang` when the user switches `/en` ↔ `/ru`.

**Examples:**

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: { default: "My App", template: "%s | My App" },
  description: "Accessible React on Next.js",
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <a href="#main" className="skip-link">
          Skip to content
        </a>
        {children}
      </body>
    </html>
  );
}
```

**Thoughts:** Skip links are not built into Next.js — add them in layout CSS (visually hidden until focus). `next/font` can help readable typography — [Next.js — Optimizations](./4_nextjs_optimizations.md).

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/layout](https://nextjs.org/docs/app/api-reference/file-conventions/layout) · [nextjs.org/docs/architecture/accessibility](https://nextjs.org/docs/architecture/accessibility)

</details>

<details>
  <summary>Route announcer</summary>

### Route announcer

**Definition:** Internal mechanism that updates assistive tech after **client-side** navigations via `next/link` or the App Router client router.

**How it works:** After transition, announcement priority is roughly: **`document.title`** → **first `<h1>`** → **pathname**. Full document loads (`<a href>` without client routing) use the browser’s normal navigation announcement.

**When to use:** Default — no config. Improve it with unique titles, one clear `<h1>` per view, and titles that make sense when read aloud.

**Pluses and minuses:**

| +                                        | −                                                                                      |
| ---------------------------------------- | -------------------------------------------------------------------------------------- |
| No setup for soft nav                    | Missing or duplicate `<h1>` weakens context                                            |
| Works with streaming and RSC navigations | **Focus is not moved** to the top on route change — users may not know content swapped |

**Examples:**

```tsx
// Good: title + h1 align with metadata
export const metadata = { title: "Billing settings" };

export default function BillingPage() {
  return (
    <main id="main">
      <h1>Billing settings</h1>
      {/* ... */}
    </main>
  );
}
```

**Thoughts:** After navigation, consider moving focus to `h1` or a named region in client layouts for complex apps (dialogs, wizards). App Router source intentionally does not auto-focus on nav.

**Sources:** [nextjs.org/docs/architecture/accessibility#route-announcements](https://nextjs.org/docs/architecture/accessibility#route-announcements)

</details>

<details>
  <summary>Focus and client navigation</summary>

### Focus and client navigation

**Definition:** Patterns for keyboard and screen reader users when routes change without a full page load.

**How it works:** Layout router scrolls to hash or top on navigation but **does not reset focus**. Modals, drawers, and multi-step flows need explicit `focus()` on open/close. Use native `<dialog>` or libraries that trap focus; restore focus to the triggering control on close.

**When to use:** Dashboards, settings wizards, checkout, any flow mixing `next/link` with overlays.

**Examples:**

```tsx
"use client";

import { useEffect, useRef } from "react";
import { usePathname } from "next/navigation";

export function FocusMainOnNavigate() {
  const pathname = usePathname();
  const mainRef = useRef<HTMLElement | null>(null);

  useEffect(() => {
    const main = document.getElementById("main");
    main?.focus({ preventScroll: false });
  }, [pathname]);

  return null;
}
```

**Thoughts:** Prefer visible focus styles on interactive elements ([HTML — Accessibility](./1_html_a11y.md)). Do not steal focus on every nav unless your UX research supports it — sometimes preserving focus in a sidebar is correct.

**Sources:** [nextjs.org/docs/architecture/accessibility](https://nextjs.org/docs/architecture/accessibility)

</details>

<details>
  <summary>ESLint and jsx-a11y</summary>

### ESLint and jsx-a11y

**Definition:** Next.js recommended ESLint config includes **`eslint-plugin-jsx-a11y`** — static checks for common JSX accessibility mistakes.

**How it works:** Rules cover ARIA attribute validity, required ARIA props for roles, forbidden roles on elements, `alt` text on images, and more. From **Next.js 16**, use the **ESLint CLI** (or Biome) in `package.json` scripts — `next lint` is removed and `next build` does not run lint.

**When to use:** Local dev and CI on every PR. Pair with Lighthouse accessibility audits and manual keyboard testing.

**Pluses and minuses:**

| +                           | −                                       |
| --------------------------- | --------------------------------------- |
| Catches easy mistakes early | Cannot prove real AT usability          |
| Same rules across team      | False positives on intentional patterns |

**Examples:**

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

**Thoughts:** `next/image` still needs meaningful `alt` — lint helps, but decorative vs informative alt is a content decision. Automated tests: prefer queries by role/label (Testing Library, Playwright) — [Next.js — Platform](./4_nextjs_platform.md#testing-nextjs-vitest--rtl).

**Sources:** [nextjs.org/docs/architecture/accessibility#linting](https://nextjs.org/docs/architecture/accessibility#linting) · [nextjs.org/docs/app/guides/upgrading/version-16](https://nextjs.org/docs/app/guides/upgrading/version-16)

</details>
