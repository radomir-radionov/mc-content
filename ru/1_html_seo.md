# HTML — SEO

Возможность сканирования, метаданные и структурированные данные.

[ ← Вернуться в центр](./1_html.md)

### Related docs

* [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1_html_ Performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [SEO & structured data](#seo-structured-data)

---

## SEO & structured data

<details>
  <summary>SEO fundamentals for developers</summary>

### SEO fundamentals for developers

**Definition:**

SEO улучшает видимость в органическом поиске. Разработчики влияют на возможность сканирования, индексирования, структуру, производительность и удобство для мобильных устройств.

| Фактор | HTML-реализация |
|--------|---------------------|
| Сканируемость | Валидный HTML, нет неработающих ссылок |
| Индексирование | `<title>`, `meta description`, `canonical` |
| Структура | Семантические заголовки, `<main>` |
| Производительность | Основные веб-показатели |

**How it works:**

- Сканируемый HTML, действительная структура, уникальная `<title>`, метаописание.
- смысловые заголовки, `<main>`, внутренние ссылки.
- Производительность (Core Web Vitals) является сигналом ранжирования.

| Фактор | HTML |
|--------|------|
| Название | Уникально на странице |
| Канонический | Дублирующийся контент |

**When to use:**

Маркетинговые страницы · сайты документации · SPA с SSR/prerender для индексируемого HTML.

#### Common pitfalls

- Пусто или дубликат `<title>` на СПА-маршрутах.
- Блокировка контента за JS без SSR.
- `noindex` ушел на производство.

**Examples:**

```html
<head>
  <title>How to Structure HTML — Guide</title>
  <meta name="description" content="Learn semantic HTML for SEO and accessibility." />
  <link rel="canonical" href="https://example.com/guide" />
</head>
```

**Sources:** [Центр поиска Google](https://developers.google.com/search/docs)

</details>

<details>
  <summary>Open Graph & Twitter cards</summary>

### Open Graph & Twitter cards

**Definition:**

Метатеги Open Graph и Twitter Card управляют тем, как ссылки отображаются при публикации на социальных платформах.

| Тег | Цель |
|-----|---------|
| `og:title` | Название акции |
| `og:description` | Поделиться сводкой |
| `og:image` | Предварительный просмотр изображения |
| `twitter:card` | Тип карты Twitter |

**How it works:**

- `og:*` мета-элементы управления предварительным просмотром Facebook/LinkedIn/Slack.
- `twitter:card` для макета карты Twitter/X.
- Требуются абсолютные URL-адреса для изображений.

| Тег | Цель |
|-----|--------|
| `og:title` | Название акции |
| `og:image` | Предварительный просмотр изображения |

**When to use:**

Карточки блога · Страницы продуктов · `summary_large_image`.

#### Common pitfalls

- Родственник `og:image` URL-адреса.
- Изображение размером менее 1200×630 для больших карточек.
- Отсутствующий `og:url` канонический.

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

JSON-LD внедряет машиночитаемые структурированные данные для получения расширенных результатов поиска — статей, продуктов, часто задаваемых вопросов, хлебных крошек.

| Формат | Заметки |
|--------|-------|
| JSON-LD | Предпочтительный формат Google |
| Микроданные | Атрибуты HTML-элементов |
| РДФа | Сегодня менее распространены |

**How it works:**

- `<script type="application/ld+json">` встраивает данные Schema.org.
- Google предпочитает JSON-LD микроданным.
- Типы: статья, продукт, часто задаваемые вопросы, хлебные крошки.

| Формат | Заметки |
|--------|-------|
| JSON-LD | Предпочитается Google |
| Микроданные | Атрибуты HTML |

**When to use:**

Расширенные описания · Цены на товары · Часто задаваемые вопросы по аккордеонам в поисковой выдаче.

#### Common pitfalls

- Разметка не соответствует видимому содержимому (штраф за спам).
- Неверный синтаксис JSON.
- Неправильный `@type` для контента.

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

**Sources:** [schema.org](https://schema.org/) · [структурированные данные Google](https://developers.google.com/search/docs/appearance/structured-data)

</details>

<details>
  <summary>Internationalization (i18n)</summary>

### Internationalization (i18n)

**Definition:**

i18n делает контент доступным для разных языков и направлений написания. Набор `lang` on `<html>` и использовать `hreflang` для региональных вариантов.

| Техника | Реализация |
|-----------|----------------|
| Язык страницы | `<html lang="en">` |
| РТЛ | `dir="rtl"` on `<html>` |
| Региональные URL-адреса | `<link rel="alternate" hreflang>` |

**How it works:**

- `<html lang="en">` устанавливает язык страницы для SR и расстановки переносов.
- `dir="rtl"` для арабского/иврита; используйте логические свойства CSS.
- `hreflang` ссылки для региональных альтернатив.

| Техника | Реализация |
|-----------|----------------|
| Язык страницы | `<html lang>` |
| РТЛ | `dir="rtl"` |

**When to use:**

Многоязычный сайт · Верстка с RTL · `lang` на встроенных иностранных фразах.

#### Common pitfalls

- Неправильный `lang` (плохое произношение).
- `hreflang` без взаимных связей.
- Жестко заданные поля LTR в CSS.

**Examples:**

```html
<html lang="en">
<link rel="alternate" hreflang="de" href="https://example.com/de/" />
<link rel="alternate" hreflang="x-default" href="https://example.com/" />
```

**Thoughts:** Логические свойства CSS для RTL → [2_css.md](./2_css.md).

**Sources:** [MDN — язык](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang)

</details>

---

<a name="advanced-topics"></a>
