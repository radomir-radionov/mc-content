# Next.js — Routing

App Router, маршруты на основе файлов и навигация.

[ ← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [1. App vs Pages Router](#1-app-vs-pages-router)
- [2. Project structure](#2-project-structure)
- [3. File-based routing](#3-file-based-routing)
- [4. Special files](#4-special-files)
- [5. Navigation](#5-navigation)

---

## 1. App vs Pages Router

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

## 2. Project structure

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

## 3. File-based routing

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Dynamic routes</summary>

### Dynamic routes

**Definition:**

Синтаксис скобок в именах папок создает динамические сегменты URL-адресов. Доступ к значениям через `params` (асинхронный в Next.js 16).

| Узор          | URL-адрес            | `params`                                   |
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

**Definition:** Предварительно визуализирует динамические маршруты во время сборки. Возвращает список объектов параметров.

**Pluses and minuses:**

| +                                                | −                                                                    |
| ------------------------------------------------ | -------------------------------------------------------------------- |
| SSG для динамических путей                       | Построение шкал времени с учетом количества параметров               |
| Отлично подходит для блогов, каталогов продукции | Должен обрабатывать новые пули (резервный вариант или по требованию) |

**Examples:**

```tsx
export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return posts.map((post: { slug: string }) => ({ slug: post.slug }));
}
```

**Thoughts:** Комбинировать с `dynamicParams = false` до 404 неизвестных пулей, или `true` (по умолчанию) генерируется при первом посещении.

**Sources:** [nextjs.org/docs/app/api-reference/functions/generate-static-params](https://nextjs.org/docs/app/api-reference/functions/generate-static-params)

</details>

<details>
  <summary>Parallel routes</summary>

### Parallel routes

**Definition:** Именованные слоты через `@folder` синтаксис отображает несколько страниц в одном макете одновременно.

**Pluses and minuses:**

| +                                             | −                                                |
| --------------------------------------------- | ------------------------------------------------ |
| Независимая загрузка/ошибка для каждого слота | Сложная ментальная модель                        |
| Макеты приборной панели с боковыми панелями   | Требует `default.tsx` для непревзойденных слотов |

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

`(.)` соглашение перехватывает маршрут для его рендеринга внутри текущего макета — модальный шаблон.

| Конвенция  | Перехваты        |
| ---------- | ---------------- |
| `(.)`      | Тот же уровень   |
| `(..)`     | На уровень выше  |
| `(..)(..)` | Два уровня вверх |
| `(...)`    | Из корня         |

**When to use:** Лайтбоксы для фотографий, модальные окна быстрого редактирования, диалоговые окна обмена.

**Examples:**

```
app/
├── @modal/(.)photo/[id]/page.tsx  # Modal overlay
└── photo/[id]/page.tsx            # Full page on direct visit
```

**Thoughts:** Закрыть модальное окно с помощью `router.back()` - нет `router.push('/')` — для сохранения стека истории.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/intercepting-routes](https://nextjs.org/docs/app/api-reference/file-conventions/intercepting-routes)

</details>

<details>
  <summary>URL state — array query parameters</summary>

### URL state — array query parameters

**Definition:** Пользовательские интерфейсы фильтров часто сериализуют состояние множественного выбора в виде повторяющихся ключей запроса (например, `category_ids[]=1&category_ids[]=2`).

**When to use:** Фильтры проектов, фасетный поиск, выбор тегов.

**Pluses and minuses:**

| +                                                                   | −                                         |
| ------------------------------------------------------------------- | ----------------------------------------- |
| Состояние фильтра, доступное для совместного использования/закладки | Сложность кодирования (`%5B%5D` для `[]`) |
| Работает без библиотек состояния клиента                            | Требует `useSearchParams` + Интрига       |

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

**Thoughts:** Предпочитать `URLSearchParams` для кодирования вместо ручного `%5B%5D`. Для сложного состояния URL рассмотрите `nuqs` библиотека.

**Sources:** [nextjs.org/docs/app/api-reference/functions/use-search-params](https://nextjs.org/docs/app/api-reference/functions/use-search-params)

</details>

<details>
  <summary>Route groups</summary>

### Route groups

**Definition:** Папки, завернутые в `(name)` организовывать маршруты, не затрагивая URL-адрес.

**When to use:** Отдельные макеты маркетинга и приложений, деревья маршрутов аутентификации и информационной панели.

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

**Definition:** `<Link>` от `next/link` обеспечивает навигацию на стороне клиента с автоматической предварительной выборкой статических маршрутов.

**Examples:**

```tsx
import Link from "next/link";

<Link href="/blog/my-post" prefetch={true}>
  Read post
</Link>;
```

**Thoughts:**

Во время сборки обеспечьте согласованность косой черты с помощью `trailingSlash` in `next.config.ts` — несовпадающие косые черты приводят к ошибкам 404 или дублированию маршрутов.

---

**Sources:** [nextjs.org/docs/app/getting-started/linking-and-navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating)

</details>

<a name="4-special-files"></a>

## 4. Special files

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>error.tsx</summary>

### error.tsx

**Definition:** Граница ошибки клиентского компонента для сегмента маршрута. Отлавливает ошибки в `page.tsx` и дети во время рендеринга.

**Pluses and minuses:**

| +                                              | −                                                  |
| ---------------------------------------------- | -------------------------------------------------- |
| Детальный интерфейс ошибок для каждого раздела | Должно быть `'use client'`                         |
| `reset()` повторные попытки оказать            | Не улавливает ошибки в одном сегменте `layout.tsx` |

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

**Definition:** Пользовательский интерфейс отображается, когда `notFound()` вызывается из серверного компонента или обработчика маршрута.

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

**Definition:** Страницы прерываний аутентификации, вызванные `forbidden()` и `unauthorized()` помощники (требуется `authInterrupts: true` в конфигурации).

**When to use:** Доступ на основе ролей — семантика 403 против 401.

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

**Definition:** Граница ошибки корневого уровня, охватывающая всю `html`/`body`. Крайнее средство для ошибок корневого макета.

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

**Thoughts:** Должен включать собственный `<html>` и `<body>` теги.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/error](https://nextjs.org/docs/app/api-reference/file-conventions/error)

</details>

<details>
  <summary>Redirect helpers</summary>

### Redirect helpers

**Definition:**

Помощники навигации, которые выдают специальные ошибки для запуска перенаправления или ответов 404 во время рендеринга серверных компонентов.

| Функция                  | HTTP           | Вариант использования         |
| ------------------------ | -------------- | ----------------------------- |
| `redirect(url)`          | 307 Временно   | Перенаправление после мутации |
| `permanentRedirect(url)` | 308 Постоянный | Миграция URL                  |
| `notFound()`             | 404            | Отсутствует ресурс            |

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

**Definition:** Повторно выдает внутренние ошибки Next.js (`redirect`, `notFound`) из блоков catch — иначе они проглатываются.

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

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>next/link</summary>

### next/link

**Definition:** Компонент навигации на стороне клиента с автоматической предварительной загрузкой видимых ссылок в рабочей среде.

**Pluses and minuses:**

| +                             | −                                                              |
| ----------------------------- | -------------------------------------------------------------- |
| Быстрая навигация в стиле SPA | Предварительная выборка может увеличить пропускную способность |
| Восстановление прокрутки      | Внешние URL-адреса должны быть простыми `<a>`                  |

**Examples:**

```tsx
import Link from 'next/link';

<Link href="/about">About</Link>
<Link href="/blog/post-1" replace>Replace history</Link>
<Link href="/docs" scroll={false}>No scroll to top</Link>
```

**Sources:** [nextjs.org/docs/app/api-reference/comComponents/link](https://nextjs.org/docs/app/api-reference/comComponents/link)

</details>

<details>
  <summary>Navigation hooks</summary>

### Navigation hooks

**Definition:**

Клиентские хуки от `next/navigation` (Маршрутизатор приложений). Не используйте `next/router` — это только Pages Router.

| Крюк                | Возврат                                               |
| ------------------- | ----------------------------------------------------- |
| `useRouter()`       | `{ push, replace, refresh, back, forward, prefetch }` |
| `usePathname()`     | Текущая строка пути                                   |
| `useSearchParams()` | `URLSearchParams` — требуется Саспенс                 |
| `useParams()`       | Объект параметров динамического маршрута              |

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

**Definition:** Обязательная навигация на стороне клиента через `useRouter()` методы — push, replace, back и update.

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

**Definition:** `next.config.ts` параметр, определяющий, требуют ли URL-адреса косой черты в конце.

**Examples:**

```ts
const nextConfig = { trailingSlash: true }; // /about/ instead of /about
```

**Thoughts:**

Будьте последовательны — смешанные ссылки (`/about` vs `/about/`) вызывают дублирование контента и нарушение относительных путей. Когда `trailingSlash: true`, всегда включайте `/` in `href` и `router.push` пути.

---

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js/trailingSlash](https://nextjs.org/docs/app/api-reference/config/next-config-js/trailingSlash)

</details>

**См. также:** Metadata API, sitemap, robots, JSON-LD и доступность → [Next.js — Доступность, SEO & metadata](./4_nextjs_a11y_seo.md).

<a name="14-built-in-optimizations"></a>
