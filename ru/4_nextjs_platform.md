# Next.js — Platform

Прокси, конфиг, Vercel, миграция и отладка.

[ ← Вернуться в центр](./4_nextjs.md)

### Related docs

- [Next.js — Ядро](./4_nextjs_core.md) · [Next.js — Rendering](./4_nextjs_rendering.md) · [Next.js — Данные](./4_nextjs_data.md) · [Next.js — Маршрутизация](./4_nextjs_routing.md) · [Next.js — Оптимизации](./4_nextjs_optimizations.md) · [Next.js — Платформа](./4_nextjs_platform.md) · [Next.js — Интервью](./4_nextjs_interview.md)

### Sections

- [1. Proxy & middleware](#1-proxy-and-middleware)
- [2. Configuration](#2-configuration)
- [3. Vercel deployment](#3-vercel-deployment)
- [4. Migration & pitfalls](#4-migration-and-pitfalls)
- [5. Debugging](#5-debugging)

---

<a name="1-proxy-and-middleware"></a>

## 1. Proxy & middleware

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Three "middleware" concepts</summary>

### Three "middleware" concepts

**Definition:**

Экосистема Next.js/Vercel имеет **три отдельных уровня перехвата запросов**. Их путаница приводит к дырам в безопасности, двойным перенаправлениям и ошибкам «работает локально, не работает в продукте».

| Слой                                           | Файл                             | Время выполнения          | Запускается, когда                   | Область применения              |
| ---------------------------------------------- | -------------------------------- | ------------------------- | ------------------------------------ | ------------------------------- |
| **Прокси Next.js 16**                          | `proxy.ts` (корень или `src/`)   | Только Node.js            | До маршрутизации Next.js             | Граница сети приложения Next.js |
| **Промежуточное ПО Vercel Routing**            | `middleware.ts` (корень проекта) | Край/Узел/Пучок           | Перед кешем, на краю платформы       | Любой фреймворк на Vercel       |
| **Промежуточное ПО Pages Router (устаревшее)** | `middleware.ts` в следующих 16   | Устаревшее предупреждение | То же, что и старое промежуточное ПО | Мигрировать в `proxy.ts`        |

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

- **proxy.ts** — перезапись, специфичная для Next.js, защита пути администратора, фильтрация ботов на границе приложения.
- **Промежуточное программное обеспечение Vercel** — межплатформенная пограничная логика, географические заголовки, A/B на границе CDN.
- **Ни один для аутентификации** — всегда проверяйте в компонентах сервера/Действиях сервера/Обработчиках маршрутов

**When not to use:**

- Тяжёлый поиск в БД или проверка сессии только в proxy — используйте server data layer
- Единственный auth gate в middleware — **CVE-2025-29927**: заголовок `x-middleware-subrequest` мог обходить middleware-only checks

**Pluses and minuses:**

| +                                              | −                                                                        |
| ---------------------------------------------- | ------------------------------------------------------------------------ |
| Ранние редиректы экономят стоимость рендеринга | Заманчиво разместить здесь всю авторизацию                               |
| Гео/локальная маршрутизация на границе         | Прокси-сервер узла не может использовать API-интерфейсы только для Edge  |
| Внедрение заголовка перед кэшем                | Два уровня могут конфликтовать, если оба переписывают один и тот же путь |

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

- Обеспечьте **быструю** работу прокси-сервера и промежуточного программного обеспечения — без N+1 БД и без вызовов внешних API, если их можно избежать.
- `matcher` config ограничивает пути запуска перехватчика — исключить `_next/static`, изображения, значок.
- Исправьте Next.js до фиксированных версий для известных рекомендаций по обходу промежуточного программного обеспечения.

**Sources:** [nextjs.org/docs/app/api-reference/file-conventions/proxy](https://nextjs.org/docs/app/api-reference/file-conventions/proxy) · [vercel.com/docs/routing-middleware](https://vercel.com/docs/routing-middleware)

</details>

<details>
  <summary>Next.js 16 proxy.ts</summary>

### Next.js 16 proxy.ts

**Definition:** Переименовано из `middleware.ts` в Next.js 16. Находится на границе сети; экспортируемая функция должна иметь имя `proxy`.

**How it works:** `npx @next/codemod@latest middleware-to-proxy`. Среда выполнения Edge **не** поддерживается на `proxy` (Только узел). Если вам нужен Edge (например, какое-то промежуточное ПО i18n), сохраните `middleware.ts` до тех пор, пока платформа не задокументирует путь Edge.

**When to use:** Географические перенаправления, маршрутизация A/B, внедрение заголовков, фильтрация ботов на границе приложения Next.js.

**Pluses and minuses:**

| +                                                 | −                                         |
| ------------------------------------------------- | ----------------------------------------- |
| Полные API Node.js                                | Только среда выполнения Node.js — не Edge |
| Более четкое обозначение границ безопасности      | Требуется миграция из `middleware.ts`     |
| Перезапись, перенаправление, внедрение заголовков | Не для тяжелой бизнес-логики              |

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

**Definition:** Функция платформы выполняется **перед кешем** при сопоставлении запросов. Работает с Next.js, SvelteKit, Astro или любой развернутой платформой.

**When to use:** Межплатформенные шаблоны, географическая персонализация, пограничное перенаправление, когда не используется прокси Next.js.

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

**Definition:** `config.matcher` шаблоны в `proxy.ts` / промежуточное программное обеспечение, ограничивающее пути запроса, запускающие перехватчик.

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

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>next.config.ts</summary>

### next.config.ts

**Definition:**

Централизованная конфигурация для сборки, маршрутизации, образов, кэширования и экспериментальных функций.

| Вариант                              | Цель                                                |
| ------------------------------------ | --------------------------------------------------- |
| `output: 'standalone'`               | Автономная сборка, удобная для Docker               |
| `output: 'export'`                   | Статический экспорт HTML (без функций сервера)      |
| `cacheComponents: true`              | Включить компоненты кэша / PPR                      |
| `images.remotePatterns`              | Разрешить удаленные домены изображений              |
| `rewrites` / `redirects` / `headers` | Правила URL и заголовков                            |
| `serverActions.bodySizeLimit`        | Полезная нагрузка Max Server Action                 |
| `typedRoutes: true`                  | Типобезопасный `href` in `Link`                     |
| `reactCompiler: true`                | Включить компилятор React                           |
| `trailingSlash`                      | Применять косые черты в конце URL-адресов           |
| `authInterrupts: true`               | Давать возможность `forbidden()` / `unauthorized()` |

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

- `output` вариант в `next.config.ts` контролирует **форму производственной сборки** — пакет сервера по умолчанию, автономный Docker или полностью статический экспорт HTML.

| Режим            | Конфигурация           | Развернуть цель                             | Возможности сервера                                                              |
| ---------------- | ---------------------- | ------------------------------------------- | -------------------------------------------------------------------------------- |
| **По умолчанию** | (пропускать `output`)  | Версель, `next start`, Node PaaS            | SSR, ISR, действия сервера, обработчики маршрутов                                |
| **Автономный**   | `output: 'standalone'` | Докер, Кубернетес, VPS                      | То же, что и по умолчанию — минимальное `node server.js` расслоение              |
| **Экспорт**      | `output: 'export'`     | S3, страницы GitHub, любой статический хост | **Нет** — нет SSR, нет действий сервера, нет динамических обработчиков маршрутов |

**When to use:**

- **По умолчанию** — почти все приложения App Router на хостинге Vercel или Node.
- **`standalone`** — Самостоятельное размещение Docker/K8s с `output: 'standalone'` + копия `public` и `.next/static`
- **`export`** — маркетинговые микросайты без серверной логики или только с предварительно обработанной документацией

**When not to use:**

- `export` для dashboards, auth, Server Actions или webhooks — используйте default или standalone
- `standalone` при развертывании в Vercel — Vercel использует собственный конвейер вывода.

**Pluses and minuses:**

| +                                                           | −                                                                                  |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Автономные планки `node_modules` для отслеживания отложений | Автономному режиму по-прежнему нужен процесс Node, а не «только статические файлы» |
| Экспорт — самый дешевый хостинг                             | Экспорт не может использовать `cookies()`, ISR или большинство динамических API    |
| Нулевая конфигурация по умолчанию на Vercel                 | Неправильный режим → сборка проходит, но функции молча ломаются                    |

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

- `next export` CLI удален — установлен `output: 'export'` в конфиге и запускай `next build`.
- Для статического экспорта требуется `generateStaticParams` для всех динамических маршрутов (или без динамических маршрутов).
- Для многоэкземплярного ISR на автономном хостинге требуется специальная [`cacheHandlers`](#cachehandlers-nextjs-16).

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js/output](https://nextjs.org/docs/app/api-reference/config/next-config-js/output)

</details>

<details>
  <summary>Edge vs Node runtime</summary>

<a name="edge-vs-node-runtime"></a>

### Edge vs Node runtime

**Definition:**

Next.js может прокладывать маршрут на **`nodejs`** (по умолчанию, полные API-интерфейсы узла) или **среда выполнения Edge** (изолированная версия V8, веб-API). Для **новых приложений не выбирайте Edge для SSR**. Документы Next.js `export const runtime = 'edge'` как **устарело** — удалите; Узел используется по умолчанию.

|                       | среда выполнения Node.js                                  | Среда выполнения Edge                                                                                               |
|  ---------------------  |  ---------------------------------------------------------  |
| **Статус**            | По умолчанию. Рекомендуется для SSR + БД                  | Устарело для нового рендеринга маршрутов                                                                            |
| **API**               | `fs`, `crypto`, полный `node:*`                           | Веб-стандарт (`fetch`, `Request`, `crypto.subtle`)                                                                  |
| **Драйверы БД**       | Призма, `pg`, большинство ORM                             | Только на основе HTTP (бессерверная технология Neon, Turso HTTP)                                                    |
| **Размещение**        | Закрепите **регион рядом с базой данных**                 | Исторически «близко к пользователю» — часто медленнее при обращении к базе данных с одним регионом                  |
| **Все еще действует** | Рендеринг приложений, действия сервера, гибкие вычисления | Vercel **Промежуточное программное обеспечение маршрутизации** (geo, auth, A/B) — не автономные пограничные функции |

**How it works:**

Действия сервера, Prisma/Drizzle, загрузка файлов, `sharp`, все, что импортирует пакеты только для Node, **почти вся HTML-рендеринг**.

Статические оболочки/CDN, PPR, передающая оболочку с периферии, пока вычисления выполняются в регионе БД, Cloudflare Workers, когда данные глобально распределены.

**Pluses and minuses:**

| +                                                                            | −                                                                                          |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Узел рядом с данными — меньше переходов по океану к Postgres                 | Исторически более медленный холодный запуск (жидкие вычисления/кеш байт-кода улучшили это) |
| Полный npm + встроенные дополнения                                           |                                                                                            |
| Промежуточное программное обеспечение Edge — дешевый geo/JWT на точке продаж | Edge SSR + удаленная БД часто **хуже** TTFB, чем региональный Node                         |

**Examples:**

```tsx
// app/api/hello/route.ts — Node.js default
export async function GET() {
  return Response.json({ message: "Hello" });
}
```

**Thoughts:**

- Vercel: автономные Edge Functions устарели; используйте функции Vercel (узел) + дополнительное промежуточное программное обеспечение маршрутизации.
- Кадр интервью по архитектуре: [Рендеринг краев и локальность данных](./0_system_design_architecture.md#edge-rendering-vs-data-locality).
- Next.js 16 `proxy.ts` работает **Только узел**. Используйте промежуточное программное обеспечение Vercel Routing для перехвата пограничных сетей.
- Тестируйте оставшиеся Edge-маршруты отдельно — многие пакеты npm выходят из строя во время сборки.

**Sources:** [Среда выполнения Edge устарела](https://nextjs.org/docs/messages/edge-runtime-deprecated) · [Vercel Functions / Edge](https://vercel.com/docs/functions/runtimes/edge)

</details>

<details>
  <summary>cacheHandlers (Next.js 16)</summary>

### cacheHandlers (Next.js 16)

**Definition:** Множественное число `cacheHandlers` заменяет устаревшее единственное число `cacheHandler` для настраиваемого хранилища кэша в развертываниях с несколькими экземплярами.

**When to use:**

Самостоятельный ISR для нескольких экземпляров Node — внедрите серверную часть общего кэша (Redis и т. д.).

---

**Sources:** [nextjs.org/docs/app/api-reference/config/next-config-js/cacheHandlers](https://nextjs.org/docs/app/api-reference/config/next-config-js/cacheHandlers)

</details>

<a name="3-vercel-deployment"></a>

## 3. Vercel deployment

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Deployment model</summary>

### Deployment model

**Definition:**

Vercel — это **родная платформа** для Next.js: сборка триггеров Git push, каждый PR получает URL-адрес предварительного просмотра, объединение с основным продуктом развертывания. Платформа и платформа разработаны совместно для ISR, Edge и RSC.

| Этап                         | Что происходит                                                                           |
| ---------------------------- | ---------------------------------------------------------------------------------------- |
| **Git push**                 | Webhook запускает сборку Vercel (`next build`)                                           |
| **Предварительный просмотр** | Уникальный URL-адрес для каждой ветки/PR — те же переменные окружения, что и для Preview |
| **Производство**             | Атомное продвижение при слиянии — обновлены бессерверные функции CDN +                   |
| **Откат**                    | Мгновенный возврат к предыдущему развертыванию на информационной панели                  |

| Особенность                       | Выгода                                                                                                                    |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Предварительные развертывания** | Проверка качества и проектирование реальной инфраструктуры перед слиянием                                                 |
| **Интеграция с Git**              | Автоматическое развертывание;проверки статуса PR                                                                          |
| **Периферийная сеть**             | Глобальный CDN для статических ресурсов и кэшированный HTML-код полного маршрута                                          |
| **Гибкие вычисления**             | Общий бессерверный пул — меньше холодных запусков по сравнению с классическими функциями                                  |
| **Аналитика/Статистика скорости** | Core Web Vitals без ручного инструментирования — [Core Web Vitals](./4_nextjs_optimizations.md#core-web-vitals-on-vercel) |
| **Обзор среды**                   | Другой `DATABASE_URL` для предварительного просмотра и производства                                                       |

**When to use:**

Выбор по умолчанию для команд Next.js, которым требуется управляемая инфраструктура, URL-адреса предварительного просмотра и минимальный DevOps.

Строгие требования только к локальному использованию — используйте [`standalone` Docker](#output-modes) + собственный CDN; или регулируемые среды, требующие полного инфраструктурного контроля.

**Pluses and minuses:**

| +                                                                            | −                                                                                     |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Функции Next.js с нулевой конфигурацией (ISR, изображения, промежуточное ПО) | Склонность поставщиков к расширенным функциям платформы                               |
| Мгновенные откаты и предварительный просмотр                                 | Стоимость зависит от трафика и продолжительности функции                              |
| Автоматический HTTPS и CDN                                                   | Самостоятельный хостинг требует большего количества операций для обеспечения четности |

**Examples:**

```bash
git checkout -b feature/checkout
# push → Vercel comments preview URL on PR
# merge → production deploy at your-domain.com
```

**Thoughts:**

- По умолчанию команда сборки имеет значение `next build`; выходной каталог `.next` (Версель обрабатывает внутри страны).
- Серверные компоненты и действия сервера выполняются как **бессерверные функции**, если не указано иное.
- Сопряжение с [Бессерверными ограничениями](#serverless-constraints) для объединения БД в пул и ограничений по времени ожидания.
- Паритет самостоятельного размещения: см. [Самостоятельное размещение](#self-hosting) и [`cacheHandlers`](#cachehandlers-nextjs-16).

**Sources:** [vercel.com/docs/frameworks/full-stack/nextjs](https://vercel.com/docs/frameworks/full-stack/nextjs)

</details>

<details>
  <summary>Environment variables</summary>

### Environment variables

**Definition:** `NEXT_PUBLIC_*` доступен браузеру; все остальные только для сервера.

**Examples:**

```bash
# .env.local
DATABASE_URL=postgresql://...
NEXT_PUBLIC_API_URL=https://api.example.com
```

| Область применения                                            | Доступ                   |
| ------------------------------------------------------------- | ------------------------ |
| Серверные компоненты, обработчики маршрутов, действия сервера | Все переменные окружения |
| Клиентские компоненты                                         | Только `NEXT_PUBLIC_*`   |

**Thoughts:** Никогда не храните секреты `NEXT_PUBLIC_`. Используйте панель управления Vercel для переменных рабочей среды с областью действия среды (Производство/Предварительная версия/Разработка).

**Sources:** [nextjs.org/docs/app/guides/environment-variables](https://nextjs.org/docs/app/guides/environment-variables)

</details>

<details>
  <summary>Serverless constraints</summary>

### Serverless constraints

**Definition:**

Ограничения времени выполнения для Vercel/бессерверной версии — отсутствие постоянной памяти, тайм-аутов, холодного запуска и пула подключений к БД.

| Ограничение                         | Значение                                                         |
|  -----------------------------------  |  ----------------------------------------------------------------  |
| Нет постоянной памяти процесса      | Внутрипроцессные кэши LRU бесполезны — используйте Runtime Cache |
| Тайм-аут функции (зависит от плана) | Длинные задания → фоновая очередь (Inngest, Trigger.dev)         |
| Холодный старт                      | Минимизируйте размер пакета; использовать Fluid Compute |
| Ограничения на подключение к БД | Использовать пул соединений (PgBouncer, бессерверный драйвер Neon) |

**How it works:** `'use cache: remote'`, Vercel Runtime Cache или внешний Redis.

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

Запустите Next.js за пределами Верселя с помощью `output: 'standalone'` Docker-образ или `next start`.

| Работает «из коробки»                 | Требует дополнительной настройки                         |
| ------------------------------------- | -------------------------------------------------------- |
| SSR, SSG, маршруты API                | Многоэкземплярный ISR (настраиваемый `cacheHandlers`)    |
| Оптимизация изображения (с резкостью) | Недействительность кэша CDN                              |
| Автономный Докер                      | Возможности промежуточного программного обеспечения Edge |

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

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Pages → App Router checklist</summary>

### Pages → App Router checklist

**Definition:**

Пошаговый контрольный список перехода от Pages Router к соглашениям App Router.

1. Создать `app/layout.tsx` с корневой структурой HTML
2. Переносите страницы по одному маршруту за раз в `app/**/page.tsx`
3. Заменить `getServerSideProps` / `getStaticProps` с серверным компонентом `fetch`
4. Заменить `pages/api` с `app/**/route.ts`
5. Заменить `next/head` с API метаданных
6. Заменить `next/router` с `next/navigation`
7. Добавить `"use client"` только там, где необходима интерактивность
8. Кэширование аудита — по умолчанию `force-cache` может удивить бывших страниц ССР
9. Миграция `middleware.ts` → `proxy.ts` (Следующий.js 16)
10. Обновление `revalidateTag` к форме с двумя аргументами

**Sources:** [nextjs.org/docs/app/guides/migrating](https://nextjs.org/docs/app/guides/migrating)

</details>

<details>
  <summary>Deprecated APIs</summary>

### Deprecated APIs

**Definition:**

Pages Router и устаревшие API Next.js заменены шаблонами App Router.

| Удалено/устарело                                      | Миграция                          |
| ----------------------------------------------------- | --------------------------------- |
| `next export` интерфейс командной строки              | `output: 'export'` в конфигурации |
| `getServerSideProps` / `getStaticProps`               | Серверные компоненты + `fetch`    |
| `next/head`                                           | `metadata` / `generateMetadata`   |
| `next/router`                                         | `next/navigation`                 |
| `unstable_cache`                                      | `'use cache'`                     |
| `revalidateTag(tag)`                                  | `revalidateTag(tag, 'max')`       |
| `middleware.ts` (Следующие 16)                        | `proxy.ts`                        |
| Синхронизировать `cookies()` / `headers()` / `params` | `await` все API запросов          |

**Sources:** [nextjs.org/docs/app/guides/migrating](https://nextjs.org/docs/app/guides/migrating)

</details>

<details>
  <summary>Hydration errors</summary>

### Hydration errors

**Definition:**

Серверный HTML не соответствует клиентскому рендерингу — React не может согласовать.

| Причина                                      | Исправить                                                                                           |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| `Date.now()` / `Math.random()` в рендеринге  | Генерировать в `useEffect` или перейти с сервера                                                    |
| `typeof window !== 'undefined'` филиалы      | Использовать `useEffect` или только серверный компонент                                             |
| Неверное вложение HTML (`<p><div>`)          | Исправить структуру HTML                                                                            |
| Расширения браузера, изменяющие DOM          | Тест инкогнито                                                                                      |
| Сторонние библиотеки, предполагающие браузер | Динамический импорт с `ssr: false` — [Bundle pitfalls](./4_nextjs_optimizations.md#bundle-pitfalls) |

**Sources:** [nextjs.org/docs/messages/react-гидратация-error](https://nextjs.org/docs/messages/react-гидратация-error)

</details>

<details>
  <summary>RSC security</summary>

### RSC security

**Definition:**

Серверные компоненты React используют **Протокол полета** — сериализованный поток вывода компонентов и ссылок, отправляемый с сервера клиенту. Проблемы безопасности в пакетах RSC или неправильная конфигурация могут привести к раскрытию данных сервера, возможности подделки запросов или обходу промежуточного программного обеспечения.

| Риск                                                        | смягчение последствий                                                                                                        |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Уязвимости пакета RSC** (рекомендации от декабря 2025 г.) | Держать `react`, `react-dom`, `next` в **исправленных** версиях — мониторьте [react.dev/blog](https://react.dev/blog)        |
| **Обход промежуточного ПО/прокси** (CVE-2025-29927)         | Патч Next.js; никогда не полагайтесь на прокси/промежуточное программное обеспечение как на единственный шлюз аутентификации |
| **Секреты реквизита RSC**                                   | Никогда не передавайте ключи API, токены или личные данные в реквизитах клиентских компонентов                               |
| **Злоупотребление действиями сервера**                      | Подтверждать аутентификацию + ввод при каждом действии; настроить `allowedOrigins`                                           |
| **Утечка данных через Flight**                              | Рассматривать все реквизиты, связанные с клиентом, как общедоступные — сериализовать только DTO                              |

**How it works:**

1. Патч `next`, `react`, `react-dom` оперативно о выпусках безопасности
2. Аутентификация в **Компоненте сервера/Действии сервера**, а не только через прокси-сервер.
3. Сопоставьте объекты БД с простыми DTO перед переходом к клиентским компонентам.
4. Используйте `forbidden()` / `unauthorized()` (с `authInterrupts: true`) для ошибок аутентификации
5. Аудит `NEXT_PUBLIC_*` — видимый браузером по определению
6. Проверьте сторонние компоненты на наличие `"use client"` + случайный секретный импорт

**When to use:** Модель RSC по умолчанию — проверка на каждой границе доверия (прокси → Компонент сервера → Действие сервера).

**Pluses and minuses:**

| +                                                        | −                                                                                                                          |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Секреты сервера по умолчанию остаются на стороне сервера | Полезная нагрузка полета непрозрачна — ее легко перераспределить в реквизите                                               |
| Встроенные проверки CSRF действий сервера                | Экосистема все еще развивается — аудит сторонних библиотек RSC                                                             |
| `httpOnly` файлы cookie никогда не достигают клиента JS  | Неправильно настроенное кэширование может передавать данные одного пользователя другому — используйте `use cache: private` |

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

- RSC сам по себе **не** является границей безопасности — логика авторизации должна выполняться на сервере при каждом запросе/действии.
- Включить Dependabot/Renovate для `next` и `react` — Рекомендации RSC быстро исправляются.
- См. [CSRF с действиями сервера](./4_nextjs_data.md#csrf-with-server-actions) и [Шаблоны аутентификации](#authentication-patterns).

**Sources:** [nextjs.org/docs/app/guides/data-security](https://nextjs.org/docs/app/guides/data-security) · [react.dev/blog](https://react.dev/blog)

</details>

<details>
  <summary>Authentication patterns</summary>

### Authentication patterns

**Definition:**

Аутентификация App Router сочетает в себе **идентификатор сеанса** (файл cookie или токен), **защиту маршрута** (защита макета) и **авторизацию** (проверку ролей/разрешений на уровне данных). Управляемые поставщики и сеансы DIY имеют одну и ту же форму глубокоэшелонированной защиты.

| Подход                                  | Лучшее для                                  | Заметки                                                                              |
| --------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Клерк/Auth0/Descope**                 | Быстрый запуск SaaS, OAuth, MFA             | Промежуточное программное обеспечение провайдера + серверный SDK;Версель дружелюбный |
| **Улучшенная аутентификация/Auth.js**   | Самостоятельные сеансы, пользовательская БД | Сессия в БД + `httpOnly` печенье                                                     |
| **Сеанс в файле cookie httpOnly**       | Полный контроль, шаблон BFF                 | Проверка JWT/сеанса в компонентах сервера + действия                                 |
| **Внешний IdP + обработчики маршрутов** | Единый вход для предприятий                 | Обмен токенов в `route.ts`; сохранить сеансовый файл cookie                          |

| Слой                    | Ответственность                                                           |
| ----------------------- | ------------------------------------------------------------------------- |
| **прокси.ц**            | Грубое перенаправление, если нет файлов cookie сеанса (необязательно)     |
| **Макет/страница**      | `getSession()` → `redirect()` or `unauthorized()`                         |
| **Действие сервера**    | Повторная проверка сеанса и разрешений для каждой мутации                 |
| **Обработчик маршрута** | Аутентификация носителя/cookie для веб-перехватчиков и общедоступного API |

**When to use:**

- **Управляемый поставщик** — вход в систему через социальные сети OAuth, MFA, организации/роли прямо из коробки.
- **Сеанс «Сделай сам»** — полный контроль, существующая таблица пользователей, лучший друг только с Next.js.
- **`authInterrupts: true`** + `forbidden()` / `unauthorized()` — семантические 403/401 страницы (Next.js 15+)

**When not to use:**

- Хранение JWT в `localStorage` для SSR apps — предпочитайте `httpOnly` cookie
- Проверка авторизации только внутри `proxy.ts` — риск обхода; всегда проверять на уровне данных сервера

**Pluses and minuses:**

| +                                                         | −                                                                    |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| Файлы cookie httpOnly противостоят краже токенов XSS      | Для сеансов cookie требуется поддержка CSRF в обработчиках маршрутов |
| Проверка на стороне сервера при каждом рендеринге         | JWT в cookie — необходимо проверить подпись на стороне сервера       |
| Управляемые провайдеры сокращают шаблонную аутентификацию | Привязка к поставщику для расширенных функций аутентификации         |

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

- Используйте Реагировать `cache()` вокруг `getSession()` — дедупликация в рамках одного запроса.
- Действия сервера должны вызывать `getSession()` еще раз — не доверяйте идентификаторам пользователей, отправленным клиентом.
- Для обратных вызовов OAuth используйте обработчик маршрута (`app/api/auth/callback/route.ts`), чтобы установить файлы cookie и перенаправить.
- Перекрестная ссылка: [cookies() и заголовки()](./4_nextjs_rendering.md#cookies-headers), [RSC-безопасность](#rsc-security).

**Sources:** [nextjs.org/docs/app/guides/authentication](https://nextjs.org/docs/app/guides/authentication)

</details>

<a name="5-debugging"></a>

## 5. Debugging

_Темы следующие: Определение → Плюсы и минусы → Когда использовать → Пример → Примечания → Источник ([шаблон](./4_nextjs_core.md#topic-template))._

<details>
  <summary>Debug build paths</summary>

### Debug build paths

**Definition:** Флаг CLI для создания единого пути маршрута для ускорения итерации во время отладки конкретного маршрута.

**When to use:** Итерация построения одного маршрута без полной перестройки.

**Examples:**

```bash
next build --debug-build-paths "/blog/[slug]"
```

**Sources:** [nextjs.org/docs/app/api-reference/cli/next](https://nextjs.org/docs/app/api-reference/cli/next)

</details>

<details>
  <summary>Next.js MCP server</summary>

### Next.js MCP server

**Definition:** Next.js 15+ включает конечную точку MCP для отладки с помощью искусственного интеллекта в разработке.

**Thoughts:** Включите через документацию по инструментам разработчика Next.js — полезно с Cursor и другими агентами искусственного интеллекта для проверки на уровне маршрута.

**Sources:** [nextjs.org/docs/app/guides/mcp](https://nextjs.org/docs/app/guides/mcp)

</details>

<details>
  <summary>Testing Next.js (Vitest + RTL)</summary>

<a name="testing-nextjs-vitest--rtl"></a>

### Testing Next.js (Vitest + RTL)

**Definition:**

Тестируйте приложения Next.js с помощью **Vitest** (быстрого средства запуска ESM) и **Библиотеки тестирования React** (RTL) на предмет поведения компонентов. Зачем тесты/ААА: [7_special.md](./7_special_testing_fundamentals.md#tests-and-aaa). Компоненты сервера и действия сервера требуют отдельных стратегий — макет `next/navigation` и тестируйте действия как простые асинхронные функции. Заглушка против макета против шпиона: [Тестовые дубли](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies).

| Что тестировать       | Инструмент   | Подход                                               |
| --------------------- | ------------ | ---------------------------------------------------- |
| Клиентские компоненты | Витест + РТЛ | `render()`, `userEvent`, утверждаем DOM              |
| Действия сервера      | Витест       | Импортировать действие; насмехаться `db` / `cookies` |
| Обработчики маршрутов | Витест       | Вызов экспортирован `GET`/`POST` с `Request`         |
| E2E-потоки            | Драматург    | Реальный браузер — вход, навигация, формы            |

**When to use:**

- Модуль/интеграция: формы, хуки, клиентские компоненты, логика проверки действий.
- E2E: потоки аутентификации, оформление заказа, все, что пересекает действие сервера + навигация.

**Pluses and minuses:**

| +                                              | −                                                                         |
| ---------------------------------------------- | ------------------------------------------------------------------------- |
| Vitest — быстрый Jest-совместимый API          | RSC не может выполнить рендеринг в jsdom — тестовый клиент уходит или E2E |
| RTL — тестирует видимое пользователю поведение | Тяжёлое издевательство над `next/*` модули                                |
| `next/experimental/testmode` (развивается)     | Не замена Драматургу на ответственных путях                               |

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

- Насмехаться `next/navigation`: `vi.mock('next/navigation', () => ({ redirect: vi.fn(), useRouter: () => ({ refresh: vi.fn() }) }))`.
- Предпочитайте тестирование **листов клиентских компонентов**, снабженных статическими реквизитами, а не рендеринг полных деревьев RSC в модульных тестах.
- Добавить драматурга для формы действия на сервере, отправить + `revalidatePath` видимость. Пирамида + карта инструментов: [7_special.md — E2E](./7_special_testing_layers.md#e2e-testing).

**Sources:** [nextjs.org/docs/app/guides/testing](https://nextjs.org/docs/app/guides/testing) · [vitest.dev](https://vitest.dev/)

</details>

<a name="20-resources"></a>
