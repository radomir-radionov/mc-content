# Next.js — Optimizations

Images, fonts, scripts, bundling, and Core Web Vitals.

[← Back to hub](./4_nextjs.md)

### Related docs

- [Next.js — Core](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Data](./4_nextjs_data.md) · [Next.js — Routing](./4_nextjs_routing.md) · [Next.js — Accessibility, SEO & metadata](./4_nextjs_a11y_seo.md) · [Next.js — Optimizations](./4_nextjs_optimizations.md) · [Next.js — Platform](./4_nextjs_platform.md) · [Next.js — Interview](./4_nextjs_interview.md)

### Sections

- [1. Built-in optimizations](#1-built-in-optimizations)
- [2. Bundling](#2-bundling)
- [3. Performance](#3-performance)

---

<a name="1-built-in-optimizations"></a>

## 1. Built-in optimizations

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>next/image</summary>

### next/image

**Definition:** Image component with automatic optimization — resizing, WebP/AVIF, lazy loading, blur placeholder.

**When to use:** Always prefer over raw `<img>` for photos and hero images.

**Pluses and minuses:**

| +                                           | −                                                      |
| ------------------------------------------- | ------------------------------------------------------ |
| Better LCP, smaller payloads                | Requires config for remote domains                     |
| Prevents layout shift with `width`/`height` | SVG handling needs `unoptimized` or different approach |
| `preload` for LCP images                    | Build-time optimization adds deploy step on Vercel     |

**Examples:**

```tsx
import Image from "next/image";

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={630}
  preload
  sizes="(max-width: 768px) 100vw, 50vw"
/>;
```

```ts
// next.config.ts — remote images
const nextConfig = {
  images: {
    remotePatterns: [{ protocol: "https", hostname: "cdn.example.com" }],
  },
};
```

**Thoughts:** For the LCP image, `preload` inserts a `<link rel="preload">` and disables lazy loading. `priority` is deprecated in Next.js 16 — use `preload`. Breakdown → [6_web_api.md — LCP](./6_web_api_security.md#lcp-largest-contentful-paint).

**Sources:** [nextjs.org/docs/app/api-reference/components/image](https://nextjs.org/docs/app/api-reference/components/image)

</details>

<details>
  <summary>next/font</summary>

### next/font

**Definition:** Self-hosted font loading with zero layout shift. Fonts are downloaded at build time — no Google Fonts network request at runtime.

**Pluses and minuses:**

| +                          | −                                           |
| -------------------------- | ------------------------------------------- |
| Zero CLS from font swap    | Subset selection affects character coverage |
| No external CDN dependency | Multiple font weights increase bundle       |

**Examples:**

```tsx
import { Geist, Geist_Mono } from "next/font/google";

const geistSans = Geist({ subsets: ["latin"], variable: "--font-geist-sans" });

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={geistSans.variable}>
      <body>{children}</body>
    </html>
  );
}
```

**Thoughts:** Do not use `<link href="fonts.googleapis.com">` — use `next/font` for production apps on Vercel.

**Sources:** [nextjs.org/docs/app/api-reference/components/font](https://nextjs.org/docs/app/api-reference/components/font)

</details>

<details>
  <summary>next/script</summary>

### next/script

**Definition:**

Optimized third-party script loading with strategies to avoid blocking main thread.

| Strategy                     | When                                          |
| ---------------------------- | --------------------------------------------- |
| `beforeInteractive`          | Critical scripts — injected into initial HTML |
| `afterInteractive` (default) | Analytics, chat widgets                       |
| `lazyOnload`                 | Low-priority trackers                         |
| `worker`                     | Experimental — run in web worker              |

**Examples:**

```tsx
import Script from "next/script";

<Script
  src="https://analytics.example.com/script.js"
  strategy="afterInteractive"
/>;
```

**Sources:** [nextjs.org/docs/app/api-reference/components/script](https://nextjs.org/docs/app/api-reference/components/script)

</details>

<details>
  <summary>@next/third-parties</summary>

### @next/third-parties

**Definition:** Official wrappers for Google Analytics, Google Tag Manager, YouTube embeds with optimized loading.

**Examples:**

```tsx
import { GoogleAnalytics } from "@next/third-parties/google";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XXXXXXXX" />
    </html>
  );
}
```

---

**Sources:** [nextjs.org/docs/app/guides/third-party-libraries](https://nextjs.org/docs/app/guides/third-party-libraries)

</details>

<a name="2-bundling"></a>

## 2. Bundling

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Bundle analysis</summary>

### Bundle analysis

**Definition:** Identify large client bundles before they ship.

**Examples:**

```bash
# Analyze with @next/bundle-analyzer
ANALYZE=true npm run build
```

```ts
// next.config.ts
import bundleAnalyzer from "@next/bundle-analyzer";

const withBundleAnalyzer = bundleAnalyzer({
  enabled: process.env.ANALYZE === "true",
});
export default withBundleAnalyzer(nextConfig);
```

**Sources:** [nextjs.org/docs/app/guides/package-bundling](https://nextjs.org/docs/app/guides/package-bundling)

</details>

<details>
  <summary>optimizePackageImports</summary>

### optimizePackageImports

**Definition:** Tree-shake heavy icon/UI libraries automatically. Barrel `index` re-exports can otherwise pull unused modules into the client bundle and hurt **INP** — [6_web_api.md](./6_web_api_security.md#inp-interaction-to-next-paint).

**Examples:**

```ts
const nextConfig = {
  experimental: {
    optimizePackageImports: ["lucide-react", "@mui/material", "date-fns"],
  },
};
```

**Thoughts:** The flag is still `experimental`. Turbopack often analyzes barrel imports without it; Webpack needs the config.

**Sources:** [nextjs.org/docs/app/guides/package-bundling](https://nextjs.org/docs/app/guides/package-bundling) · [optimizePackageImports](https://nextjs.org/docs/app/api-reference/config/next-config-js/optimizePackageImports)

</details>

<details>
  <summary>Bundle pitfalls</summary>

### Bundle pitfalls

**Definition:**

Common mistakes that inflate client bundles or leak server code into the browser.

| Mistake                                  | Fix                                     |
| ---------------------------------------- | --------------------------------------- |
| Importing `fs`, `db` in Client Component | Keep data access in Server Components   |
| `"use client"` at page root              | Push to leaf components                 |
| Barrel files importing server code       | Direct imports or separate entry points |
| Heavy libs in layout                     | Dynamic `import()` with loading state   |

**Sources:** [nextjs.org/docs/app/guides/lazy-loading](https://nextjs.org/docs/app/guides/lazy-loading)

</details>

<a name="3-performance"></a>

## 3. Performance

_Topics follow: Definition → Pros / Cons → When to use → Example → Notes → Source ([template](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Core Web Vitals on Vercel</summary>

### Core Web Vitals on Vercel

**Definition:** Google's field metrics for real-user experience. **Core** vitals today: **LCP**, **INP**, **CLS**. **TTFB** is a diagnostic (how fast the first byte arrives) — it influences LCP but is not itself a Core Web Vital.

**How it works:**

Chrome records the metrics; CrUX / Speed Insights aggregate them. Next.js levers:

| Metric                | Good                       | What it measures                            | Next.js levers                                                                                                                                                                                                |
| --------------------- | -------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **LCP**               | ≤ 2.5s                     | Largest contentful paint (hero image/text)  | `next/image` `preload`, PPR static shell, `next/font`                                                                                                                                                         |
| **INP**               | ≤ 200ms                    | Interaction to next paint (replaced FID)    | Smaller client JS, Server Components, fewer hydration handlers                                                                                                                                                |
| **CLS**               | ≤ 0.1                      | Layout shift                                | `next/font` (fallback `size-adjust`, self-hosted files), explicit image `width`/`height`/`sizes` or `fill`. Metric + HTML/CSS causes → [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift) |
| **TTFB** (diagnostic) | ≤ 0.8 s good, > 1.8 s poor | Time to first byte                          | CDN/SSG/ISR, streaming, avoid `force-dynamic` unless needed, regional Node vs distant Edge+DB — [full TTFB](./6_web_api_security.md#ttfb-time-to-first-byte)                                                  |
| **FCP** (diagnostic)  | ≤ 1.8s                     | First contentful paint (blank screen ended) | Same early path as LCP: TTFB, blocking CSS, `next/font`, less JS before first paint — [6_web_api.md](./6_web_api_security.md#first-contentful-paint-fcp)                                                      |

**When to use:**

Interview “how do you make Next fast?” — name the three vitals, one lever each, and that INP replaced FID.

#### Common pitfalls

Calling FID a Core Web Vital (it is not, since 2024). Shipping a huge `'use client'` tree that wrecks INP. Images without dimensions → CLS. Putting `useReportWebVitals` in the root layout and marking the whole layout `'use client'`.

**Pluses and minuses:**

| +                                | −                                                            |
| -------------------------------- | ------------------------------------------------------------ |
| Shared language with SEO/product | Lab Lighthouse ≠ field CrUX                                  |
| Maps cleanly to Next primitives  | Chasing TTFB on Edge while the DB is far away can _hurt_ LCP |

**Examples:**

```tsx
import Image from "next/image";

export function Hero() {
  return (
    <Image
      src="/hero.jpg"
      alt="Product"
      width={1200}
      height={630}
      preload
      sizes="100vw"
    />
  );
}
```

keep `'use client'` on a tiny reporter so `layout` stays a Server Component:

```tsx
"use client";

import { useReportWebVitals } from "next/web-vitals";

export function WebVitals() {
  useReportWebVitals((metric) => {
    navigator.sendBeacon("/analytics", JSON.stringify(metric));
  });
  return null;
}
```

Import `<WebVitals />` from `app/layout.tsx`. Lab vs field, CrUX 28-day window, `web-vitals/attribution` → [6_web_api.md — Core Web Vitals](./6_web_api_security.md#core-web-vitals). LCP slices (`fetchpriority`, `picture`, Speculation Rules) → [6_web_api.md — LCP](./6_web_api_security.md#lcp-largest-contentful-paint).

**Thoughts:** LCP = first meaningful paint, INP = input snappiness, CLS = visual stability. TTFB is a means, not the goal. `priority` on `next/image` is deprecated — use `preload`.

**See also:** [TTFB](./6_web_api_security.md#ttfb-time-to-first-byte) · [INP](./6_web_api_security.md#inp-interaction-to-next-paint)

**Sources:** [web.dev/articles/vitals](https://web.dev/articles/vitals) · [next.js — useReportWebVitals](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals) · [vercel.com/docs/speed-insights](https://vercel.com/docs/speed-insights)

</details>

<details>
  <summary>Performance checklist</summary>

### Performance checklist

**Definition:**

Production performance audit checklist for App Router applications.

1. Default to Server Components — minimize `"use client"` surface
2. Parallelize independent fetches with `Promise.all`
3. Use `loading.tsx` and Suspense for perceived speed
4. Tag and invalidate cache explicitly after mutations
5. Analyze bundle after adding any client library
6. Use `next/image` with correct `sizes`
7. Avoid `force-dynamic` unless truly needed
8. Connection pool for serverless DB (Neon, Prisma Accelerate)

**Thoughts:** Cache invalidation stays in [Caching](./4_nextjs_data.md#caching-model-overview). Prefetch stays in [Linking and prefetch](./4_nextjs_routing.md#linking-and-prefetch). Rendering strategies stay in [Rendering](./4_nextjs_rendering.md#1-rendering-strategies).

**Sources:** [nextjs.org/docs](https://nextjs.org/docs)

</details>
