# Next.js — Optimizations

Изображения, шрифты, скрипты, бандлинг и Core Web Vitals.

[ ← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Оптимизации](./4_nextjs_optimizations.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [1. Built-in optimizations](#1-built-in-optimizations)
- [2. Bundling](#2-bundling)
- [3. Performance](#3-performance)

---

<a name="1-built-in-optimizations"></a>

## 1. Built-in optimizations

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>next/image</summary>

### next/image

**Definition:** Компонент изображения с автоматической оптимизацией — изменение размера, WebP/AVIF, отложенная загрузка, размытие заполнителя.

**When to use:** Всегда предпочитаю сырому `<img>` для фотографий и изображений героев.

**Pluses and minuses:**

| +                                             | −                                                                 |
| --------------------------------------------- | ----------------------------------------------------------------- |
| Лучше LCP, меньше полезной нагрузки           | Требуется конфигурация для удаленных доменов                      |
| Предотвращает сдвиг макета с `width`/`height` | Необходимость обработки SVG `unoptimized` или другой подход       |
| `preload` для изображений LCP                 | Оптимизация времени сборки добавляет этап развертывания на Vercel |

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

**Thoughts:** Для изображения LCP `preload` вставляет `<link rel="preload">` и отключает отложенную загрузку. `priority` устарел в Next.js 16 — используйте `preload`. Разбивка → [6_web_api.md — LCP](./6_web_api_security.md#lcp-largest-contentful-paint).

**Sources:** [nextjs.org/docs/app/api-reference/components/image](https://nextjs.org/docs/app/api-reference/components/image)

</details>

<details>
  <summary>next/font</summary>

### next/font

**Definition:** Самостоятельная загрузка шрифтов без смещения макета. Шрифты загружаются во время сборки — во время выполнения запросы к сети Google Fonts не требуются.

**Pluses and minuses:**

| +                                | −                                             |
| -------------------------------- | --------------------------------------------- |
| Нулевой CLS из-за замены шрифтов | Выбор подмножества влияет на охват персонажей |
| Нет внешней зависимости от CDN   | Несколько весов шрифта увеличивают пакет      |

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

**Thoughts:** Не используйте `<link href="fonts.googleapis.com">` - использовать `next/font` для производственных приложений на Vercel.

**Sources:** [nextjs.org/docs/app/api-reference/components/font](https://nextjs.org/docs/app/api-reference/components/font)

</details>

<details>
  <summary>next/script</summary>

### next/script

**Definition:**

Оптимизирована загрузка сторонних скриптов со стратегиями, позволяющими избежать блокировки основного потока.

| Стратегия                         | Когда                                           |
| --------------------------------- | ----------------------------------------------- |
| `beforeInteractive`               | Критические сценарии — внедрены в исходный HTML |
| `afterInteractive` (по умолчанию) | Аналитика, виджеты чатов                        |
| `lazyOnload`                      | Трекеры с низким приоритетом                    |
| `worker`                          | Экспериментально — запуск в веб-воркере         |

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

**Definition:** Официальные оболочки для Google Analytics, Google Tag Manager, встраивания YouTube с оптимизированной загрузкой.

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

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Bundle analysis</summary>

### Bundle analysis

**Definition:** Определите крупные клиентские пакеты перед их отправкой.

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

**Definition:** Автоматическое встряхивание тяжелых значков/библиотек пользовательского интерфейса. Бочка `index` в противном случае повторный экспорт может привести к попаданию неиспользуемых модулей в пакет клиента и повредить **INP** — [6_web_api.md](./6_web_api_security.md#inp-interaction-to-next-paint).

**Examples:**

```ts
const nextConfig = {
  experimental: {
    optimizePackageImports: ["lucide-react", "@mui/material", "date-fns"],
  },
};
```

**Thoughts:** Флаг по-прежнему в `experimental`. Turbopack часто анализирует barrel-импорты сам; для Webpack конфиг нужен.

**Sources:** [nextjs.org/docs/app/guides/package-bundling](https://nextjs.org/docs/app/guides/package-bundling) · [optimizePackageImports](https://nextjs.org/docs/app/api-reference/config/next-config-js/optimizePackageImports)

</details>

<details>
  <summary>Bundle pitfalls</summary>

### Bundle pitfalls

**Definition:**

Распространенные ошибки, которые приводят к раздуванию клиентских пакетов или утечке серверного кода в браузер.

| Ошибка                                    | Исправить                                        |
| ----------------------------------------- | ------------------------------------------------ |
| Импорт `fs`, `db` в клиентском компоненте | Сохраняйте доступ к данным в компонентах сервера |
| `"use client"` в корне страницы           | Нажмите на листовые компоненты                   |
| Файлы ствола, импортирующие серверный код | Прямой импорт или отдельные точки ввоза          |
| Тяжелые библиотеки в макете               | Динамический `import()` с состоянием загрузки    |

**Sources:** [nextjs.org/docs/app/guides/lazy-loading](https://nextjs.org/docs/app/guides/lazy-loading)

</details>

<a name="3-performance"></a>

## 3. Performance

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Core Web Vitals on Vercel</summary>

### Core Web Vitals on Vercel

**Definition:** Полевые показатели Google для реального взаимодействия с пользователем. **Основные** показатели сегодня: **LCP**, **INP**, **CLS**. **TTFB** — это диагностический показатель (насколько быстро поступает первый байт). Он влияет на LCP, но сам по себе не является ключевым параметром сети.

**How it works:**

Chrome записывает метрики; CruX/Speed ​​Insights агрегируют их. Рычаги Next.js:

| Метрическая            | Хорошо                        | Что он измеряет                                                 | Рычаги Next.js                                                                                                                                                                                                    |
| ---------------------- | ----------------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ЛКП**                | ≤ 2,5 с                       | Самая большая содержательная краска (главное изображение/текст) | `next/image` `preload`, статическая оболочка ППР, `next/font`                                                                                                                                                     |
| **ИЯФ**                | ≤ 200 мс                      | Взаимодействие со следующей отрисовкой (заменено FID)           | Меньший клиентский JS, серверные компоненты, меньше обработчиков гидратации                                                                                                                                       |
| **CLS**                | ≤ 0,1                         | Смена макета                                                    | `next/font` (отступать `size-adjust`, локальные файлы), явное изображение `width`/`height`/`sizes` or `fill`. Метрика + причины HTML/CSS → [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift) |
| **TTFB** (диагностика) | ≤ 0,8 с хорошо, > 1,8 с плохо | Время до первого байта                                          | CDN/SSG/ISR, потоковая передача, избегать `force-dynamic` если не требуется, региональный узел или удаленный Edge+DB — [full TTFB](./6_web_api_security.md#ttfb-time-to-first-byte)                               |
| **FCP** (диагностика)  | ≤ 1,8 с                       | Первая содержательная раскраска (кончился пустой экран)         | Тот же ранний путь, что и у LCP: TTFB, блокировка CSS, `next/font`, меньше JS перед первой отрисовкой — [6_web_api.md](./6_web_api_security.md#first-contentful-paint-fcp)                                        |

**When to use:**

Интервью «Как сделать Next быстрым?» — назовите три жизненно важных органа, по одному рычагу каждый, и что INP заменил FID.

#### Common pitfalls

Называя FID жизненно важным элементом сети (с 2024 года это не так). Доставка огромная `'use client'` дерево, разрушающее ИЯФ. Изображения без размеров → CLS. положить `useReportWebVitals` в корневом макете и отмечаем весь макет `'use client'`.

**Pluses and minuses:**

| +                                              | −                                                                        |
| ---------------------------------------------- | ------------------------------------------------------------------------ |
| Общий язык с SEO/продуктом                     | Лаборатория Маяк ≠ поле CrUX                                             |
| Четко сопоставляется со следующими примитивами | Погоня за TTFB на Edge, когда БД находится далеко, может _навредить_ LCP |

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

держать `'use client'` на крошечном репортере так `layout` остается серверным компонентом:

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

Импорт `<WebVitals />` от `app/layout.tsx`. Лаборатория против поля, 28-дневное окно CrUX, `web-vitals/attribution` → [6_web_api.md — основные веб-показатели](./6_web_api_security.md#core-web-vitals). Срезы LCP (`fetchpriority`, `picture`, Правила спекуляций) → [6_web_api.md — LCP](./6_web_api_security.md#lcp-largest-contentful-paint).

**Thoughts:** LCP = первая осмысленная краска, INP = резкость ввода, CLS = визуальная стабильность. TTFB — это средство, а не цель. `priority` у `next/image` устарел — используйте `preload`.

**See also:** [TTFB](./6_web_api_security.md#ttfb-time-to-first-byte) · [INP](./6_web_api_security.md#inp-interaction-to-next-paint)

**Sources:** [web.dev/articles/vitals](https://web.dev/articles/vitals) · [next.js — useReportWebVitals](https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals) · [vercel.com/docs/speed-insights](https://vercel.com/docs/speed-insights)

</details>

<details>
  <summary>Performance checklist</summary>

### Performance checklist

**Definition:**

Контрольный список аудита производственной производительности для приложений App Router.

1. По умолчанию для серверных компонентов — свернуть. `"use client"` поверхность
2. Распараллелить независимые выборки с помощью `Promise.all`
3. Используйте `loading.tsx` и Приостановка восприятия скорости
4. Помечать и аннулировать кеш явно после мутаций.
5. Анализируйте пакет после добавления любой клиентской библиотеки.
6. Используйте `next/image` с правильным `sizes`
7. Избегайте `force-dynamic` если это действительно не нужно
8. Пул соединений для бессерверной БД (Neon, Prisma Accelerate)

**Thoughts:** Инвалидация кэша — в [Caching](./4_nextjs_data.md#caching-model-overview). Prefetch — в [Linking and prefetch](./4_nextjs_routing.md#linking-and-prefetch). Стратегии рендеринга — в [Rendering](./4_nextjs_rendering.md#1-rendering-strategies).

**Sources:** [nextjs.org/docs](https://nextjs.org/docs)

</details>
