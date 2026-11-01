# React — Architecture

Как **структурировать** React-код и UI: слои design system, методологии папок и границы между командами.

[← Back to hub](./4_react.md)

### Related docs

[React — hub](./4_react.md) · [Patterns — composition](./4_react_patterns.md) · [Ecosystem — routing & libraries](./4_react_ecosystem.md) · [React 19 & SSR](./4_react_server.md) · [Architecture patterns (canonical)](./7_special_architecture.md)

**Not here:** **Routing** (React Router, URL loaders) → [Ecosystem — Client routing](./4_react_ecosystem.md#client-routing). **Rendering** (SPA vs SSR vs RSC) → [React 19 — SPA vs SSR vs RSC](./4_react_server.md#spa-vs-ssr-vs-rsc).

### Main concepts

| Topic                                                                   | Covers                                            |
| ----------------------------------------------------------------------- | ------------------------------------------------- |
| [Architecture overview](#architecture-overview)                         | Выбор структурной модели                          |
| [Atomic Design](#atomic-design)                                         | Atoms → molecules → organisms → templates → pages |
| [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)               | Слои папок и правила импортов                     |
| [Container / Presentational](#container--presentational)                | Smart vs dumb components                          |
| [Modular monolith (feature folders)](#modular-monolith-feature-folders) | Лёгкая раскладка `features/`                      |
| [Vertical slice architecture](#vertical-slice-architecture)             | Вертикальный срез на use case                     |
| [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)               | Слои папок и правила импортов                     |
| [Bulletproof React](#bulletproof-react)                                 | Строго заданное дерево `src/` (features-first)    |
| [Screaming Architecture](#screaming-architecture)                       | Папки по домену, а не по типу файла               |
| [Micro-frontends (MFE)](#micro-frontends-mfe)                           | Границы команд, Module Federation                 |
| [Monorepo packages](#monorepo-packages)                                 | Apps + shared packages в одном репозитории        |

### Sections

- [Architecture overview](#architecture-overview)
- [Atomic Design](#atomic-design)
- [Container / Presentational](#container--presentational)
- [Modular monolith (feature folders)](#modular-monolith-feature-folders)
- [Vertical slice architecture](#vertical-slice-architecture)
- [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)
- [Bulletproof React](#bulletproof-react)
- [Screaming Architecture](#screaming-architecture)
- [Micro-frontends (MFE)](#micro-frontends-mfe)
- [Monorepo packages](#monorepo-packages)

---

## Architecture overview

<details>
<summary>Architecture overview</summary>

**Definition:** **Frontend architecture** (в этой теме) — это то, как вы **организуете компоненты и папки** и **проводите границы между командами** — а не какой router или режим SSR вы выбираете.

**How it works:**

| Model                                  | Unit of organization                                  | Best when                                |
| -------------------------------------- | ----------------------------------------------------- | ---------------------------------------- |
| **Atomic Design**                      | Гранулярность UI (atom → page)                        | Design systems, Storybook                |
| **Container / Presentational**         | Data/effects vs чистый UI в компонентах               | Переиспользование UI, тесты presentation |
| **Modular monolith / feature folders** | `features/<name>/`                                    | Одна команда, один deploy                |
| **Vertical slice**                     | Одна папка = UI + hooks + API для flow                | Меньше прыжков между папками на feature  |
| **Feature-Sliced Design (FSD)**        | Строгие слои + public API slice                       | Крупный SPA, жёсткие правила импортов    |
| **Bulletproof React**                  | Документированная раскладка `src/` (features, shared) | Greenfield SPA, шаблон команды           |
| **Screaming Architecture**             | Верхний уровень папок = business domains              | Домен виден из дерева                    |
| **Micro-frontends (MFE)**              | Несколько деплоящихся frontends                       | Много команд, независимый release        |
| **Monorepo packages**                  | `apps/web` + `packages/ui`                            | Общий kit между приложениями             |

**When to use:** Сочетайте **Atomic** (UI kit) с **feature folders** или **FSD** (код приложения). **Container/Presentational** — внутри features. **Bulletproof React** и **Screaming Architecture** — соглашения об именах/раскладке; совместимы с FSD. **MFE** — только когда deploy boundaries этого требуют.

**See also:** Паттерны на уровне системы → [Architecture patterns — Frontend structure](./7_special_architecture.md#frontend-structure).

**Pluses and minuses:**

|             | Pluses                                                 | Minuses                                       |
| ----------- | ------------------------------------------------------ | --------------------------------------------- |
| Atomic + DS | Переиспользуемые примитивы, понятные Storybook stories | Не заменяет правила папок для business logic  |
| FSD         | Задаёт направление зависимостей                        | Церемония для крошечных приложений            |
| MFE         | Автономные команды                                     | Интеграция, общая версия React, drift дизайна |

**Thoughts:** В вакансиях «MFA» часто имеют в виду **MFE**. Routing → [Client routing](./4_react_ecosystem.md#client-routing); rendering → [SPA vs SSR vs RSC](./4_react_server.md#spa-vs-ssr-vs-rsc).

**Sources:** [Feature-Sliced Design](https://feature-sliced.design/) · [Atomic Design (Brad Frost)](https://atomicdesign.bradfrost.com/)

</details>

---

## Atomic Design

<details>
<summary>Atomic Design</summary>

**Definition:** **Atomic Design** — методология построения UI на **пяти уровнях** — от мельчайших частей до целых экранов — популярна для **design systems** и библиотек компонентов.

**How it works:**

| Level         | React role                               | Examples                                                            |
| ------------- | ---------------------------------------- | ------------------------------------------------------------------- |
| **Atoms**     | Мельчайшие UI-единицы без business logic | `Button`, `Input`, `Label`, обёртка иконки                          |
| **Molecules** | Составные atoms с одной задачей          | `SearchField` (input + button), `FormField` (label + input + error) |
| **Organisms** | Заметные секции интерфейса               | `Header`, `ProductCard`, `LoginForm`                                |
| **Templates** | Layout страницы без реальных данных      | `DashboardTemplate` со slots/props                                  |
| **Pages**     | Templates + реальный контент/state       | `DashboardPage`, подключённая к данным                              |

**When to use:** Документируйте компоненты в **Storybook**; обеспечивайте единообразие через shared kit (часто под `shared/ui` в FSD или `components/ui` в простых репозиториях).

**Pluses and minuses:**

| +                                  | −                                                                  |
| ---------------------------------- | ------------------------------------------------------------------ |
| Общий словарь с дизайном           | Названия уровней субъективны — зафиксируйте правила команды        |
| Поощряет мелкие, тестируемые части | Atoms могут стать «god components», если в них запихнуть все props |

**Examples:**

```tsx
// Atom
export function Button({ children, ...props }: ButtonProps) {
  return (
    <button type="button" className="btn" {...props}>
      {children}
    </button>
  );
}

// Molecule
export function SearchField({ onSubmit }: { onSubmit: (q: string) => void }) {
  const [q, setQ] = useState("");
  return (
    <form
      onSubmit={(e) => {
        e.preventDefault();
        onSubmit(q);
      }}
    >
      <Input
        value={q}
        onChange={(e) => setQ(e.target.value)}
        aria-label="Search"
      />
      <Button type="submit">Search</Button>
    </form>
  );
}
```

**Thoughts:** **Molecules** — уровень, где обычно живут accessibility и UX валидации (label + control + hint). Сочетайте с [Patterns — composition](./4_react_patterns.md); размещение в папках — с [FSD `shared/ui`](#feature-sliced-design-fsd).

**Sources:** [Atomic Design](https://atomicdesign.bradfrost.com/chapter-2/) · [Storybook — Component hierarchy](https://storybook.js.org/docs)

</details>

---

## Container / Presentational

<details>
<summary>Container / Presentational</summary>

**Definition:** Делите компоненты на **containers** (данные, effects, wiring) и **presentational** (разметка, стили, callbacks через props) — также **smart vs dumb** components.

**How it works:**

| Role               | Responsibility                         | Typical hooks                               |
| ------------------ | -------------------------------------- | ------------------------------------------- |
| **Container**      | Fetch/subscribe, маппинг state в props | `useQuery`, `useSelector`, `useEffect`      |
| **Presentational** | Рендер UI из props; без global store   | Только локальный UI state при необходимости |

**When to use:** Один и тот же UI с разными источниками данных; snapshot-тесты presentational-деревьев.

**Examples:**

```tsx
export function UserList({ users, onSelect }: UserListProps) {
  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>
          <button type="button" onClick={() => onSelect(u.id)}>
            {u.name}
          </button>
        </li>
      ))}
    </ul>
  );
}

export function UserListContainer() {
  const { data } = useGetUsersQuery();
  const navigate = useNavigate();
  return (
    <UserList users={data ?? []} onSelect={(id) => navigate(`/users/${id}`)} />
  );
}
```

**Thoughts:** **Custom hooks** часто заменяют container-компоненты — та же идея, меньше вложенности. См. [Patterns — composition](./4_react_patterns.md).

**Sources:** [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a04c7f) · [react.dev — Custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)

</details>

---

## Modular monolith (feature folders)

<details>
<summary>Modular monolith (feature folders)</summary>

**Definition:** **Modular monolith** сохраняет **одно приложение и один deploy**, но группирует код по **feature**, а не только по техническому типу (`components/`, `hooks/`, `utils/` разбросаны).

**How it works:**

Типичная форма:

```text
src/
  app/          # providers, router mount
  features/
    auth/
    cart/
    catalog/
  shared/       # ui kit, api, lib
  pages/        # optional route entry components
```

| Practice                                            | Why                                          |
| --------------------------------------------------- | -------------------------------------------- |
| Colocate hooks + components + api для одной feature | Изменения остаются локальными                |
| `shared/ui` для примитивов уровня Atomic            | Не дублируйте Button в каждой feature        |
| Тонкий `pages/`                                     | Только compose features — без business logic |

**When to use:** Одна команда или общий релиз; ступень перед полными правилами **FSD**.

**Pluses and minuses:**

| +                                  | −                                                                    |
| ---------------------------------- | -------------------------------------------------------------------- |
| Просто внедрить                    | Без правил импортов features могут импортировать друг друга по кругу |
| Меньше boilerplate, чем FSD slices | Плоский `features/` может перерасти — миграция на слои FSD           |

**Thoughts:** Server-state и auth boundaries тоже **архитектурны** — зафиксируйте, какая feature владеет session, а какая только потребляет `useUser()`. Data libraries → [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching).

**See also:** [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd) · [Micro-frontends (MFE)](#micro-frontends-mfe)

**Sources:** [React — Patterns](./4_react_patterns.md)

</details>

---

## Vertical slice architecture

<details>
<summary>Vertical slice architecture</summary>

**Definition:** **Vertical slice** — один **use case** владеет **вертикальным** срезом стека: UI, state и API client для flow живут вместе — вместо горизонтальных слоёв вроде `components/` + `services/` + `stores/`, общих для всех features.

**How it works:**

```text
features/checkout/
  CheckoutPage.tsx
  useCheckout.ts
  checkoutApi.ts
  checkout.types.ts
```

| Horizontal (layer-first)          | Vertical (slice-first)                       |
| --------------------------------- | -------------------------------------------- |
| Все hooks в `hooks/`              | Hook рядом с feature, которая его использует |
| Общий god-модуль `api/`           | Colocated `checkoutApi.ts`                   |
| Изменение затрагивает много папок | Изменение остаётся в одной slice-папке       |

**When to use:** Те же плюсы, что у [modular monolith](#modular-monolith-feature-folders); акцент на **сквозном ownership** на user story (checkout, onboarding, admin export).

**Thoughts:** **FSD** — формализованная vertical-slice система с именами слоёв; **Bulletproof React** — ещё одна slice-friendly раскладка. Backend «vertical slice» (MediatR и т.д.) — отдельная идея — [Architecture patterns](./7_special_architecture.md).

**See also:** [Modular monolith (feature folders)](#modular-monolith-feature-folders) · [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)

**Sources:** [Jimmy Bogard — Vertical Slice Architecture](https://www.jimmybogard.com/vertical-slice-architecture/)

</details>

---

## Feature-Sliced Design (FSD)

<details>
<summary>Feature-Sliced Design (FSD)</summary>

**Definition:** Методология папок frontend: **layers** (app → pages → widgets → features → entities → shared) плюс **slices** с публичным `index.ts` — масштабирует команды без runtime-сложности micro-frontends.

**How it works:**

| Layer      | React examples                                              |
| ---------- | ----------------------------------------------------------- |
| `app`      | Providers, global styles, store setup                       |
| `pages`    | Композиции маршрутов — связывают widgets/features           |
| `widgets`  | Header, sidebar, блоки dashboard                            |
| `features` | Пользовательские сценарии: «add to cart», «edit profile»    |
| `entities` | Business-сущности: User, Product                            |
| `shared`   | UI kit ([Atomic Design](#atomic-design)), `api` client, lib |

Import rule: **нижние слои не импортируют из верхних**. Слайс делится на **сегменты** (`ui`, `model`, `api`, `lib`). Другие слайсы импортируют только **public API** слайса (`index.ts`), а не файл внутри слайса. Настройка router → [Ecosystem — Client routing](./4_react_ecosystem.md#client-routing).

**When to use:** Средние/крупные SPA; альтернатива [Micro-frontends (MFE)](#micro-frontends-mfe) при одном deploy.

**Sources:** [feature-sliced.design](https://feature-sliced.design/) · [FSD layers](https://feature-sliced.design/docs/reference/layers) · [Frontend structure (canonical)](./7_special_architecture.md#frontend-structure)

</details>

---

## Bulletproof React

<details>
<summary>Bulletproof React</summary>

**Definition:** **Bulletproof React** — community **reference architecture** (Alan Aydogmus) для масштабируемых React-приложений: feature-first папки, shared kernel и явные места для routes, assets и tests.

**How it works:**

Типичная раскладка `src/`:

```text
src/
  app/           # providers, router, global styles
  features/      # domain features (auth, discussions, …)
  components/    # shared UI (часто уровень design system)
  hooks/         # shared hooks
  lib/           # axios client, react-query config, utils
  stores/        # global client state (если нужен)
  types/         # shared TS types
  assets/
  config/
```

| Folder        | Role                                                                     |
| ------------- | ------------------------------------------------------------------------ |
| `features/*`  | Business capabilities; внутри могут быть `components/`, `api/`, `hooks/` |
| `components/` | App-wide UI, не привязанный к одной feature                              |
| `lib/`        | Cross-cutting infra (API client, env)                                    |

**When to use:** Greenfield SPA, когда нужен **copy-paste шаблон**; совпадает с [modular monolith](#modular-monolith-feature-folders) и может эволюционировать в [FSD](#feature-sliced-design-fsd) через переименование слоёв.

**Pluses and minuses:**

| +                                              | −                                                               |
| ---------------------------------------------- | --------------------------------------------------------------- |
| Хорошо задокументирован, часто в интервью      | Не официальный стандарт команды React                           |
| Features-first совпадает с продуктовой моделью | `components/` vs `features/*/components` требует правил команды |

**Thoughts:** Сочетайте с [Atomic Design](#atomic-design): shared `components/` как atoms/molecules. Data layer → [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching).

**Sources:** [bulletproof-react](https://github.com/alan2207/bulletproof-react)

</details>

---

## Screaming Architecture

<details>
<summary>Screaming Architecture</summary>

**Definition:** **Screaming Architecture** (Uncle Bob) — **верхний уровень структуры кричит домен** — `catalog/`, `billing/`, `shipping/` — а не `controllers/`, `views/`, `models/` или плоский `components/`.

**How it works:**

| Screaming (domain-first)       | Technical (layer-first)                 |
| ------------------------------ | --------------------------------------- |
| `src/catalog/ProductList.tsx`  | `src/components/ProductList.tsx`        |
| `src/billing/InvoiceTable.tsx` | `src/hooks/useInvoices.ts` далеко от UI |

React mapping: каждая **domain-папка** содержит свои components, hooks и API helpers (как [vertical slice](#vertical-slice-architecture)); **shared/** — cross-domain UI и lib.

**When to use:** Онбординг и рефакторинг — новички ищут код «checkout» в `features/checkout` или `checkout/`, а не размазанный по типам файлов.

**Pluses and minuses:**

| +                                   | −                                                         |
| ----------------------------------- | --------------------------------------------------------- |
| Код совпадает с языком продукта     | Домены пересекаются → нужны правила (помогают FSD slices) |
| Сочетается с modular monolith / FSD | Неверные границы доменов тяжело переименовать             |

**Thoughts:** FSD **slices** (`features/add-to-cart`) — screaming architecture с принудительным направлением слоёв.

**Sources:** [Robert C. Martin — Screaming Architecture](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html)

</details>

---

## Micro-frontends (MFE)

<details>
<summary>Micro-frontends (MFE)</summary>

**Definition:** **Micro-frontends** делят UI одного продукта на **независимо деплоящиеся** frontends (по команде или домену), собранные в браузере — несколько React roots или remote bundles на одном сайте.

**How it works:**

| Pattern                     | Integration                                  | Tradeoff                                                                                 |
| --------------------------- | -------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Build-time composition**  | Monorepo packages импортируются в один shell | Общий release; проще всего                                                               |
| **Module Federation**       | Host `remotes` + remote `exposes` в runtime  | Независимые deploys; version skew                                                        |
| **Iframe / Web Components** | Жёсткая изоляция                             | UX и styling cost                                                                        |
| **Route-owned remotes**     | Shell загружает remote на URL prefix         | Ясное ownership (настройка routing → [Ecosystem](./4_react_ecosystem.md#client-routing)) |

Shared `react` / `react-dom` должны быть **singletons** в Federation, чтобы не было двух React.

**When to use:** Крупные организации с отдельным cadence релизов; **не** default — предпочитайте [FSD](#feature-sliced-design-fsd) или [modular monolith](#modular-monolith-feature-folders), пока deploy boundaries не мешают.

**Pluses and minuses:**

| +                           | −                                   |
| --------------------------- | ----------------------------------- |
| Автономия команд            | Design system и общее state сложнее |
| Меньший blast radius deploy | Runtime integration bugs            |

**Thoughts:** Code splitting внутри **одного** app — [Code splitting](./4_react_perf.md#code-splitting), не MFE.

**See also:** [Architecture overview](#architecture-overview) · [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)

**Sources:** [Webpack — Module Federation](https://webpack.js.org/concepts/module-federation/) · [Rspack — Module Federation](https://rspack.dev/guide/features/module-federation)

</details>

---

## Monorepo packages

<details>
<summary>Monorepo packages</summary>

**Definition:** **Monorepo** — несколько **packages** и **apps** в одном git-репозитории — например `apps/web` (React SPA) + `packages/ui` (design system) + `packages/api-types` (общие контракты).

**How it works:**

```text
apps/
  web/              # main React app (Vite/Next)
  admin/            # optional second app
packages/
  ui/               # shared Atomic components
  config-eslint/    # shared tooling
  api-client/       # typed client generated from OpenAPI
```

| Tooling                                  | Role                        |
| ---------------------------------------- | --------------------------- |
| **pnpm workspaces** / **npm workspaces** | Линковка локальных packages |
| **Turborepo** / **Nx**                   | Кэш сборок, task graph      |

**When to use:** Один design system для web + admin; общие types с backend; **не** то же самое, что [MFE](#micro-frontends-mfe) — обычно один deploy на app, shared code на этапе сборки.

**Pluses and minuses:**

| +                           | −                                                                                                   |
| --------------------------- | --------------------------------------------------------------------------------------------------- |
| Один PR обновляет app + kit | Размер репо и сложность CI                                                                          |
| Границы packages явные      | Нужны правила [Module / package boundaries](./7_special_architecture.md#module--package-boundaries) |

**Thoughts:** Kit [Atomic Design](#atomic-design) часто живёт в `packages/ui`. TypeScript API types → [TypeScript — API contracts](./5_ts_api_contracts.md).

**See also:** [Micro-frontends (MFE)](#micro-frontends-mfe) · [Architecture patterns — Module boundaries](./7_special_architecture.md#module--package-boundaries)

**Sources:** [Turborepo docs](https://turbo.build/repo/docs) · [pnpm workspaces](https://pnpm.io/workspaces)

</details>
