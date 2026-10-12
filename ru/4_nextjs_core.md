# Next.js — Core

Модель платформы, маршрутизатор приложений и принципы проектирования.

[ ← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [1. Framework overview](#1-framework-overview)
- [2. App vs Pages Router](#2-app-vs-pages-router)
- [3. Project structure](#3-project-structure)
- [4. Design principles](#4-design-principles)

---

## 1. Framework overview

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](#topic-template))._

<details>
  <summary>Next.js overview</summary>

### Next.js overview

**Definition:** Next.js — это **фреймворк React** для создания полноценных веб-приложений. UI строится компонентами React; Next.js добавляет маршрутизацию, рендеринг, работу с данными и production-инструменты — сборщики и компиляторы уже настроены, чтобы вы сосредоточились на продукте, а не на стеке.

**When to use:**

- Маркетинговые сайты, блоги и документация с требованиями SEO
- E-commerce, дашборды и SaaS с аутентифицированными и публичными страницами
- BFF (Backend for Frontend) — API рядом с UI
- Команды на Vercel или со standalone Docker output
- Приложения с RSC (меньшие клиентские бандлы, доступ к серверным данным)

**When not to use:**

- Чисто статические сайты с минимальным JS — рассмотрите **Astro**
- Мобильные приложения — **Expo / React Native**
- Тяжёлый кастомный Node-сервер (долгоживущий WebSockets hub) — отдельный backend + Vite SPA могут подойти лучше
- Контентные сайты, где React — лишняя нагрузка

**Pluses and minuses:**

| +                                                                                | −                                                                                                    |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Батарейки в комплекте: маршрутизация, SSR, API, оптимизация изображений/шрифтов  | Мнение: аварийные люки существуют, но усложняют ситуацию                                             |
| Серверные компоненты React + потоковая передача «из коробки» (App Router)        | Ментальная модель крутого кэширования; легко отправлять устаревшие или чрезмерно динамичные страницы |
| Отличная интеграция с Vercel (предварительные развертывания, CDN, аналитика)     | Привязка к поставщикам при использовании функций Vercel                                              |
| SEO-оптимизирован по умолчанию (SSR/SSG/ISR/PPR)                                 | Холодный запуск на бессерверной версии; не идеален для всех рабочих нагрузок                         |
| TypeScript — прежде всего, большая экосистема, сильный сигнал о приеме на работу | Два маршрутизатора (приложение + страницы) — устаревшие кодовые базы несут долг по миграции          |
| Автоматическое разделение кода на маршрут                                        | Время сборки увеличивается с размером приложения                                                     |

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

**Thoughts:** Next.js **не** является заменой знаний React — это верхний слой. Оформление интервью: React = библиотека (просмотр); Next.js = фреймворк (маршрутизация, рендеринг, данные, развертывание). В новых проектах по умолчанию должен использоваться **App Router** (Next.js 13+). Pages Router — это режим обслуживания существующих приложений.

**Sources:** [nextjs.org/docs](https://nextjs.org/docs)

</details>

<details>
  <summary>React vs Next.js</summary>

### React vs Next.js

**Definition:**

React отображает пользовательский интерфейс; Next.js определяет, как и где создается, кэшируется и доставляется этот пользовательский интерфейс.

| | Реагировать | Next.js |
| | --------------------- | --------------------------------------- |
| | **Роль** | библиотека пользовательского интерфейса | Полнофункциональный фреймворк |
| | **Маршрутизация** | Принесите свой собственный | Файловый `app/` каталог |
| | **Извлечение данных** | Клиентские крючки / ручные SSR | Серверные компоненты, `fetch` кэш, Действия сервера |
| | **Рендеринг** | КСО по умолчанию (Vite) | SSR, SSG, ISR, PPR настраиваются для каждого маршрута |
| | **API** | Отдельный бэкэнд | Обработчики маршрутов + Действия сервера |
| | **Развертывание** | Статический CDN или собственный | Оптимизирован для Верселя; `standalone` для Докера |

**Thoughts:** Общие правила RSC, гидратации и перехвата см. в [4_react.md §8 — Серверный рендеринг и RSC](./4_react_server.md#8-server-rendering--rsc). Стратегии рендеринга (SSR, SSG, ISR, PPR): [Next.js — Rendering](./4_nextjs_rendering.md). Семантика кэширования: [Next.js — Data](./4_nextjs_data.md).

**Sources:** [react.dev](https://react.dev/) · [nextjs.org/docs](https://nextjs.org/docs)

</details>

<details>
  <summary>Next.js vs alternatives</summary>

### Next.js vs alternatives

**Definition:**

Сравнение Next.js с другими полнофункциональными и метафреймворками для React и контентных сайтов.

| Рамочная                | Сила                                                     | Компромисс                                                 |
| ----------------------- | -------------------------------------------------------- | ---------------------------------------------------------- |
| **Vite + React Router** | Максимальная гибкость, быстрая разработка, приоритет SPA | Ручная настройка SSR/RSC                                   |
| **Ремикс**              | Веб-стандарты, загрузчики/действия, формы                | Меньшая экосистема, чем Следующая                          |
| **Астро**               | Контент-сайты, архитектура островов                      | Менее подходит для интенсивного взаимодействия с клиентами |
| **SvelteKit**           | Производительность, простота                             | Другая парадигма пользовательского интерфейса              |

**How it works:** SEO, производительность при первой загрузке, логика совместного размещения серверов, командная стандартизация на Vercel.

**Sources:** [nextjs.org/docs/app](https://nextjs.org/docs/app)

</details>

<details>
  <summary>History (compressed)</summary>

### History (compressed)

**Definition:**

Хронология основных выпусков Next.js и основные этапы маршрутизации/рендеринга.

| Версия   | Веха                                                                         |
| -------- | ---------------------------------------------------------------------------- |
| **2016** | Next.js 1 — Маршрутизатор страниц, SSR                                       |
| **v9**   | API-маршруты (`pages/api`)                                                   |
| **v12**  | Промежуточное ПО, поддержка React 18                                         |
| **v13**  | Маршрутизатор приложений (бета-версия), RSC, `app/` каталог                  |
| **v14**  | App Router стабильный, частичный предварительный рендеринг экспериментальный |
| **v15**  | API асинхронных запросов (`cookies`, `headers`, `params`)                    |
| **v16**  | Компоненты кэша, `proxy.ts`, `use cache`, требуются асинхронные API          |

---

**Sources:** [nextjs.org/блог](https://nextjs.org/блог)

</details>

<a name="2-app-vs-pages-router"></a>

### Topic template

Каждая важная тема в `<details>` следующие разделы:

**Определение** → **Как это работает** → **Когда использовать** → **Плюсы и минусы** → **Примеры** → **Мысли** → **Источники**

---

<a name="1-framework-overview"></a>

### Primary references

**Definition:**

Официальная документация и руководства по платформам для Next.js и Vercel.

- [nextjs.org/docs](https://nextjs.org/docs) — официальная документация
- [nextjs.org/docs/llms.txt](https://nextjs.org/docs/llms.txt) — полный указатель документации для LLM.
- [Руководство по платформе Vercel Next.js](https://vercel.com/docs/frameworks/full-stack/nextjs)
- [Частичный предварительный рендеринг на Vercel](https://vercel.com/docs/partial-prerendering)\* [Кэш времени выполнения Vercel](https://vercel.com/docs/caching/runtime-cache)
- [Промежуточное программное обеспечение Vercel Routing] (https://vercel.com/docs/routing-middleware)

**Sources:** [nextjs.org/docs](https://nextjs.org/docs)

</details>

<details>
  <summary>Deep dives</summary>

## 2. App vs Pages Router

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](#topic-template))._

<details>
  <summary>App Router</summary>

### App Router

**Definition:** Современный маршрутизатор в `app/`. Папки задают сегменты URL; маршрут становится **публичным** только когда в сегменте есть `page.tsx` (UI) или `route.ts` (HTTP-обработчик). Построен на серверных компонентах React, вложенных макетах, потоковой передаче через Suspense и модели кэширования Next.js. По умолчанию для всех новых проектов с Next.js 13+.

**How it works:**

- **Маршрутизация по файловой системе** — `app/blog/[slug]/page.tsx` → `/blog/:slug`. Динамические сегменты: `[slug]` (один параметр), `[...slug]` (catch-all), `[[...slug]]` (опциональный catch-all). В Next.js 15+ `params` и `searchParams` — **Promise**; в Server Components их нужно `await`.
- **Специальные файлы в каждом сегменте:**

| Файл            | Роль                                                                                  |
| --------------- | ------------------------------------------------------------------------------------- |
| `layout.tsx`    | Общая оболочка для дочерних сегментов; состояние сохраняется при клиентской навигации |
| `page.tsx`      | Уникальный UI маршрута — делает URL публично доступным                                |
| `loading.tsx`   | Мгновенный fallback, пока сегмент стримится (граница React Suspense)                  |
| `error.tsx`     | Error boundary сегмента (нужен `"use client"`)                                        |
| `not-found.tsx` | UI 404 для сегмента (вызывается через `notFound()`)                                   |
| `route.ts`      | Route Handlers — HTTP-эндпоинты (`GET`, `POST`, …) рядом с UI                         |
| `template.tsx`  | Как `layout`, но **перемонтируется** при навигации (свежее состояние/effects)         |

- **Порядок рендера внутри сегмента:** `layout` → `template` → `error` → `loading` (Suspense) → `not-found` → `page`. Родительские layout оборачивают детей рекурсивно (`app/layout.tsx` → `app/dashboard/layout.tsx` → `page`).
- **Server Components по умолчанию** — `layout` и `page` выполняются на сервере, пока файл не начинается с `"use client"`. Данные загружают прямо в `async`-компонентах; Client Components добавляют только для хуков, событий и browser API.
- **Статика по умолчанию** — маршруты пререндерятся на сборке, пока не читают данные запроса (`cookies()`, `headers()`, `searchParams` или `fetch` с `cache: 'no-store'`). Переопределение на сегмент: `export const dynamic = 'force-static' | 'force-dynamic'`.
- **Колокация без влияния на URL** — route groups `(marketing)` не попадают в путь; приватные папки `_lib` / `_components` игнорируются роутером. Нерутируемые файлы можно держать рядом с `page.tsx`.

**When to use:** Каждый новый проект. Greenfield-приложения, e-commerce, дашборды, контентные сайты.

**Pluses and minuses:**

| +                                                     | −                                                     |
| ----------------------------------------------------- | ----------------------------------------------------- |
| RSC, streaming, вложенные layouts                     | Кривая обучения кэшированию и границам Server/Client  |
| Колокация `loading.tsx`, `error.tsx`, `not-found.tsx` | Нельзя смешать `pages/` и `app/` на одном URL         |
| Server Actions, Metadata API, Route Handlers          | Часть сторонних библиотек всё ещё без RSC             |
| Тонкое кэширование через `use cache` / `revalidate`   | Миграция с Pages требует переписывания                |
| Prefetch и client transitions через `next/link`       | `template.tsx` vs `layout.tsx` часто путают на старте |

**Examples:**

```tsx
// app/layout.tsx — корневой layout (обязателен); нужны <html> и <body>
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
// app/blog/layout.tsx — вложенный layout для /blog/*
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
// app/blog/[slug]/loading.tsx — показывается мгновенно, пока page.tsx стримится
export default function Loading() {
  return <p>Loading post…</p>;
}
```

```ts
// app/api/posts/route.ts — Route Handler (замена pages/api)
import { NextResponse } from "next/server";

export async function GET() {
  const posts = await getPosts();
  return NextResponse.json(posts);
}
```

**Thoughts:** Динамические, parallel и intercepting routes — в [Next.js — Routing](./4_nextjs_routing.md). Кэширование, опции `fetch` и Server Actions — в [Next.js — Data](./4_nextjs_data.md). На интервью: **RSC + static by default**; `"use client"` — opt-in для интерактивности; при конфликте URL побеждает App Router над `pages/`.

**Sources:** [Project structure](https://nextjs.org/docs/app/getting-started/project-structure) · [Layouts and pages](https://nextjs.org/docs/app/getting-started/layouts-and-pages) · [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)

</details>

<details>
  <summary>Pages Router</summary>

### Pages Router

**Definition:** Устаревший маршрутизатор в `pages/`. Использование `getServerSideProps`, `getStaticProps`, `getInitialProps`, и `pages/api` для конечных точек серверной части.

**When to use:** Поддержание только существующих приложений Pages Router. Не начинайте новые проекты на Pages Router.

**Pluses and minuses:**

| +                                                         | −                                                             |
| --------------------------------------------------------- | ------------------------------------------------------------- |
| Зрелые, хорошо документированные шаблоны                  | Нет RSC, нет вложенных макетов                                |
| Упрощенная ментальная модель (данные на основе реквизита) | `getServerSideProps` работает на странице — сложнее составить |
| Поддержка большой существующей кодовой базы               | Не там, где появляются новые возможности Next.js              |

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

Сопоставление «один к одному» API-интерфейсов Pages Router с эквивалентами App Router для поэтапной миграции.

| Маршрутизатор страниц | Эквивалент App Router                                     |
| --------------------- | --------------------------------------------------------- |
| `getStaticProps`      | `fetch` с `cache: 'force-cache'` + `generateStaticParams` |
| `getServerSideProps`  | `fetch` с `cache: 'no-store'` или динамический маршрут    |
| `getInitialProps`     | Асинхронная выборка данных серверного компонента          |
| `pages/api/*.ts`      | `app/**/route.ts` (Обработчики маршрутов)                 |
| `_app.tsx`            | `app/layout.tsx`                                          |
| `_document.tsx`       | `app/layout.tsx` + API метаданных                         |
| `next/head`           | `export const metadata` / `generateMetadata()`            |
| `next/router`         | `next/navigation` (`useRouter`, `usePathname`)            |
| `next/link`           | `next/link` (тот же пакет, с поддержкой App Router)       |

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

Оба роутера могут сосуществовать в одном проекте (`pages/` + `app/`), но **не для того же URL**. Маршрутизатор приложений имеет приоритет. Планируйте поэтапную миграцию по каждому маршруту.

---

**Sources:** [nextjs.org/docs/app/guides/migrating](https://nextjs.org/docs/app/guides/migrating)

</details>

<a name="3-project-structure"></a>

## 3. Project structure

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](#topic-template))._

<details>
  <summary>Directory layout</summary>

### Directory layout

**Definition:** Next.js использует соглашения о файловой системе. `app/` каталог (или `src/app/`) определяет маршруты; специальные имена файлов соответствуют функциям платформы.

**When to use:** Следуйте настройкам по умолчанию, если только не требуется monorepo или FSD. `src/app/` + функциональные папки.

**Pluses and minuses:**

| +                                                | −                                                                                    |
| ------------------------------------------------ | ------------------------------------------------------------------------------------ |
| Предсказуемая структура — быстрая адаптация      | Глубокая вложенность может слишком буквально отражать URL                            |
| Размещение интерфейса маршрута, загрузка, ошибки | Большой `app/` деревьям нужны группы маршрутов или дисциплина FSD                    |
| `src/` опция сохраняет корень чистым             | При перемещении файлов URL-адреса изменяются, если не используются группы/перезаписи |

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

**Thoughts:** Предпочитать `src/app/` в более крупных базах кода для отделения кода приложения от конфигурации в корне. Личные папки: префикс с `_` (e.g. `_components`) — не маршрутизируется.

**Sources:** [nextjs.org/docs/app/getting-started/project-structure](https://nextjs.org/docs/app/getting-started/project-structure)

</details>

<details>
  <summary>Special files</summary>

### Special files

**Definition:**

Имена файлов соглашения в `app/` которые соответствуют функциям платформы — макетам, состояниям загрузки, ошибкам и маршрутам API.

| Файл            | Цель                                                                            |
| --------------- | ------------------------------------------------------------------------------- |
| `page.tsx`      | Уникальный интерфейс для сегмента маршрута; делает маршрут общедоступным        |
| `layout.tsx`    | Оболочка общего пользовательского интерфейса; сохраняет состояние при навигации |
| `template.tsx`  | Похож на макет, но **перемонтируется** при навигации (новое состояние/эффекты)  |
| `loading.tsx`   | Мгновенная загрузка пользовательского интерфейса через границу ожидания         |
| `error.tsx`     | Граница ошибки клиента для сегмента                                             |
| `not-found.tsx` | Пользовательский интерфейс, когда `notFound()` называется                       |
| `route.ts`      | Обработчик маршрута (конечная точка API)                                        |
| `default.tsx`   | Резервный вариант для слотов параллельных маршрутов                             |

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

**Thoughts:** `layout.tsx` состояние выдерживает навигацию клиента; `template.tsx` нет. Использовать `template` для ввода анимации или клавиш аналитики для каждой навигации.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions](https://nextjs.org/docs/app/api-reference/file-conventions)

</details>

<details>
  <summary>Route segments</summary>

### Route segments

**Definition:**

Сегменты URL-пути, созданные по именам папок в `app/` — статические, динамические, универсальные и группы маршрутов.

| Узор                         | URL-адрес               | Пример                                 |
| ---------------------------- | ----------------------- | -------------------------------------- |
| Статический                  | `/blog`                 | `app/blog/page.tsx`                    |
| Динамический                 | `/blog/hello`           | `app/blog/[slug]/page.tsx`             |
| Всеобъемлющий                | `/docs/a/b/c`           | `app/docs/[...slug]/page.tsx`          |
| Дополнительный всеобъемлющий | `/shop` or `/shop/a/b`  | `app/shop/[[...slug]]/page.tsx`        |
| Группа маршрутов             | Никакого влияния на URL | `app/(auth)/login/page.tsx` → `/login` |

**Sources:**

[nextjs.org/docs/app/api-reference/file-conventions](https://nextjs.org/docs/app/api-reference/file-conventions)

---

</details>

## 4. Design principles

<details>
  <summary>SOLID in Next.js</summary>

### SOLID in Next.js

**Definition:** Пять принципов **SOLID** для **модулей Next.js, Server/Client Components, Route Handlers и хелперов в `lib/`** — не только для `class`.

| Principle                     | Meaning                            | Next.js example                                                                              |
| ----------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------- |
| **S** — Single Responsibility | Одна причина для изменения модуля  | `page.tsx` композирует · `queries.ts` читает · `actions.ts` мутирует                         |
| **O** — Open/Closed           | Расширять новым кодом, не правками | Добавить `applePay` — не править `checkout()` под каждый тип оплаты                          |
| **L** — Liskov Substitution   | Подтипы соблюдают контракт         | Общие layout/page props работают для каждого варианта — без client-only полей в server-типах |
| **I** — Interface Segregation | Маленькие сфокусированные API      | Передавать `{ readUser }` — не весь DB-клиент, когда нужны только чтения                     |
| **D** — Dependency Inversion  | Зависеть от абстракций             | `placeOrder(input, store)` — инжектить repository; в тестах — фейковый `store`               |

**How it works:**

Каждая буква соответствует **Examples** ниже — ищите `// good` и `// bad`:

1. **S** — разделить `page.tsx`, `queries.ts`, `actions.ts` — не один файл с fetch, validate, email и render.
2. **O** — новый payment provider = новая функция в map — не новый `if` в `checkout()`.
3. **L** — любой layout wrapper принимает тот же контракт `children` — без варианта, ломающего streaming/RSC.
4. **I** — `loadCheckout()` принимает `{ readCart }` — не весь модуль `db`.
5. **D** — домен в `lib/` получает `store` / `fetch` — Route Handler подключает конкретные адаптеры.

**When to use:** Рефакторинг жирных `page.tsx`; plugin-style провайдеры (auth, payments, CMS); code review для новых фич App Router.

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

**Thoughts:** SOLID — **ориентир**, не закон: для одноразовой маркетинговой страницы пять интерфейсов не нужны. Применять, когда больно менять или тестировать. React: [SOLID in React](./4_react_core.md#solid-in-react). Express: [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Полный справочник: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

</details>
