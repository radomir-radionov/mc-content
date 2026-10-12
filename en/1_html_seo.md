# HTML — SEO

Crawlability, metadata, and structured data.

[← Back to hub](./1_html.md)

### Related docs

* [HTML — Core](./1_html_core.md) · [HTML — Structure](./1_html_structure.md) · [HTML — Semantics](./1_html_semantics.md) · [HTML — Forms](./1_html_forms.md) · [HTML — Media](./1_html_media.md) · [HTML — Rendering](./1_html_rendering.md) · [HTML — Performance](./1_html_performance.md) · [HTML — Accessibility](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Advanced](./1_html_advanced.md) · [HTML — Interview](./1_html_interview.md)

### Sections

- [SEO & structured data](#seo-structured-data)

---

## SEO & structured data

<details>
  <summary>SEO fundamentals for developers</summary>

### SEO fundamentals for developers

**Definition:**

SEO improves visibility in organic search. Developers affect crawlability, indexing, structure, performance, and mobile-friendliness.

| Factor | HTML implementation |
|--------|---------------------|
| Crawlability | Valid HTML, no broken links |
| Indexing | `<title>`, `meta description`, `canonical` |
| Structure | Semantic headings, `<main>` |
| Performance | Core Web Vitals |

**How it works:**

- Crawlable HTML, valid structure, unique `<title>`, meta description.
- Semantic headings, `<main>`, internal links.
- Performance (Core Web Vitals) is ranking signal.

| Factor | HTML |
|--------|------|
| Title | Unique per page |
| Canonical | Duplicate content |

**When to use:**

Marketing pages · docs sites · SPA with SSR/prerender for indexable HTML.

#### Common pitfalls

- Empty or duplicate `<title>` on SPA routes.
- Blocking content behind JS with no SSR.
- `noindex` left on production.

**Examples:**

```html
<head>
  <title>How to Structure HTML — Guide</title>
  <meta name="description" content="Learn semantic HTML for SEO and accessibility." />
  <link rel="canonical" href="https://example.com/guide" />
</head>
```

**Sources:** [Google Search Central](https://developers.google.com/search/docs)

</details>

<details>
  <summary>Open Graph & Twitter cards</summary>

### Open Graph & Twitter cards

**Definition:**

Open Graph and Twitter Card meta tags control how links appear when shared on social platforms.

| Tag | Purpose |
|-----|---------|
| `og:title` | Share title |
| `og:description` | Share summary |
| `og:image` | Preview image |
| `twitter:card` | Twitter card type |

**How it works:**

- `og:*` meta controls Facebook/LinkedIn/Slack previews.
- `twitter:card` for Twitter/X card layout.
- Requires absolute URLs for images.

| Tag | Purpose |
|-----|--------|
| `og:title` | Share title |
| `og:image` | Preview image |

**When to use:**

Blog share cards · product pages · `summary_large_image`.

#### Common pitfalls

- Relative `og:image` URLs.
- Image smaller than 1200×630 for large cards.
- Missing `og:url` canonical.

**Examples:**

```html
<meta property="og:title" content="Page title" />
<meta property="og:description" content="Summary for social shares" />
<meta property="og:image" content="https://example.com/og-image.jpg" />
<meta name="twitter:card" content="summary_large_image" />
```

**Sources:** [ogp.me](https://ogp.me/)

</details>

<details>
  <summary>Structured data (JSON-LD)</summary>

### Structured data (JSON-LD)

**Definition:**

JSON-LD embeds machine-readable structured data for search rich results — articles, products, FAQs, breadcrumbs.

| Format | Notes |
|--------|-------|
| JSON-LD | Google's preferred format |
| Microdata | Attributes on HTML elements |
| RDFa | Less common today |

**How it works:**

- `<script type="application/ld+json">` embeds schema.org data.
- Google prefers JSON-LD over microdata.
- Types: Article, Product, FAQ, BreadcrumbList.

| Format | Notes |
|--------|-------|
| JSON-LD | Preferred by Google |
| Microdata | Attributes on HTML |

**When to use:**

Rich snippets · product prices · FAQ accordions in SERP.

#### Common pitfalls

- Markup doesn't match visible content (spam penalty).
- Invalid JSON syntax.
- Wrong `@type` for content.

**Examples:**

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "How to structure HTML",
  "author": { "@type": "Person", "name": "Jane Doe" },
  "datePublished": "2025-07-02"
}
</script>
```

**Sources:** [schema.org](https://schema.org/) · [Google structured data](https://developers.google.com/search/docs/appearance/structured-data)

</details>

<details>
  <summary>Internationalization (i18n)</summary>

### Internationalization (i18n)

**Definition:**

i18n makes content accessible across languages and writing directions. Set `lang` on `<html>` and use `hreflang` for regional variants.

| Technique | Implementation |
|-----------|----------------|
| Page language | `<html lang="en">` |
| RTL | `dir="rtl"` on `<html>` |
| Regional URLs | `<link rel="alternate" hreflang>` |

**How it works:**

- `<html lang="en">` sets page language for SR and hyphenation.
- `dir="rtl"` for Arabic/Hebrew; use CSS logical properties.
- `hreflang` links for regional alternates.

| Technique | Implementation |
|-----------|----------------|
| Page lang | `<html lang>` |
| RTL | `dir="rtl"` |

**When to use:**

Multi-language site · RTL layout · `lang` on inline foreign phrases.

#### Common pitfalls

- Wrong `lang` (bad pronunciation).
- `hreflang` without reciprocal links.
- Hard-coded LTR margins in CSS.

**Examples:**

```html
<html lang="en">
<link rel="alternate" hreflang="de" href="https://example.com/de/" />
<link rel="alternate" hreflang="x-default" href="https://example.com/" />
```

**Thoughts:** CSS logical properties for RTL → [2_css.md](./2_css.md).

**Sources:** [MDN — lang](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang)

</details>

---

<a name="advanced-topics"></a>
