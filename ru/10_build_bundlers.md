# Build — Bundlers

Транспиляторы, концепции бандлинга, бандлеры и качество кода.

[ ← Вернуться в хаб](./10_build.md)

### Related docs

- [Сборка — CI](./10_build_ci.md) · [Сборка — Bundlers](./10_build_bundlers.md) · [Сборка — Docker](./10_build_docker.md) · [Сборка — Infra](./10_build_infra.md) · [Сборка — Интервью](./10_build_interview.md)

### Sections

- [Transpilers](#transpilers)
- [Bundling concepts](#bundling-concepts)
- [Bundlers](#bundlers)
- [Code quality](#code-quality)

---

## Transpilers

<details>
  <summary>Compiler vs bundler</summary>

### Compiler vs bundler

**Definition:**

| Тип инструмента             | Ввод          | Выход                             | Роль                                            |
| --------------------------- | ------------- | --------------------------------- | ----------------------------------------------- |
| **Компилятор/транспилятор** | Исходный код  | Преобразованный исходный код      | Понижение уровня синтаксиса, удаление типов     |
| **Бандлер**                 | Многие модули | Несколько оптимизированных файлов | Граф зависимостей, разделение кода, минификация |

**Thoughts:** Современные стеки сочетают в себе оба — TypeScript (проверка типа) + esbuild (эмитирование) + Rollup (связка). Не путайте «Babel» с «Webpack».

**Sources:** [Webpack — Концепции](https://webpack.js.org/concepts/)

</details>

<details>
  <summary>Polyfills vs transpilers</summary>

### Polyfills vs transpilers

**Definition:** **Транспилер** переписывает **синтаксис**, который старый движок не может проанализировать. **Полифилл** — это скрипт, который **добавляет недостающие API** (функции/методы) во время выполнения. Большинству современных приложений нужны обе идеи, даже если Vite/esbuild берут на себя тяжелую работу. Язык и рантайм → [JS Core — Polyfills and transpilers](./3_js_core.md#polyfills-and-transpilers).

**How it works:**

- Транспилировать: `height ?? 100` → `(height !== undefined && height !== null) ? height : 100`. Инструменты: Babel, TypeScript Emit, esbuild/swc.
- Полифил: если `!Math.trunc`, назначьте реализацию. Библиотеки: `core-js`, часто через `@babel/preset-env` `useBuiltIns`.
- Синтаксис нельзя полифиллить — нужно транспилировать. Отсутствующие методы могут быть заполнены без переписывания сайтов вызовов.
- Проверьте поддержку: [compat-table](https://compat-table.github.io/compat-table/es6/) (язык), [caniuse.com](https://caniuse.com/) (веб-API).

**When to use:** Отправка современного JS на старые цели; избегайте доставки неиспользованным `core-js` - предпочитать `useBuiltIns: 'usage'` или явный импорт.

**Examples:**

```js
// Polyfill pattern
if (!Math.trunc) {
  Math.trunc = function (number) {
    return number < 0 ? Math.ceil(number) : Math.floor(number);
  };
}
```

**Sources:** [javascript.info/polyfills](https://javascript.info/polyfills) · [core-js](https://github.com/zloirock/core-js)

</details>

<details>
  <summary>Babel</summary>

### Babel

**Definition:** Компилятор JavaScript, преобразующий современный синтаксис (ES2024) в обратно совместимый код для старых браузеров.

**When to use:** Поддержка устаревших браузеров; пользовательские преобразования AST; библиотеки публикуют как ESM, так и CJS.

**Pluses and minuses:**

| +                                                    | −                                                       |
| ---------------------------------------------------- | ------------------------------------------------------- |
| Массивная экосистема плагинов                        | Медленнее, чем esbuild/swc                              |
| `@babel/preset-env` нацелен на список браузеров      | Сложность конфигурации                                  |
| JSX-преобразование для настроек, отличных от ESBuild | Путаница с полифилами (используйте `core-js` намеренно) |

**Examples:**

```js
// babel.config.js
module.exports = {
  presets: [
    ["@babel/preset-env", { targets: "defaults" }],
    ["@babel/preset-react", { runtime: "automatic" }],
  ],
};
```

**Thoughts:** Vite по умолчанию использует esbuild для транспиляции — Babel нужен только для экзотических плагинов. `@babel/preset-env` с `useBuiltIns: 'usage'` автоматически добавляет полифилы; предпочитаю явный `core-js` импорт, чтобы избежать раздувания пакета.

**Sources:** [babeljs.io](https://babeljs.io/)

</details>

<details>
  <summary>TypeScript compiler (tsc)</summary>

### TypeScript compiler (tsc)

**Definition:** Типовые проверки `.ts` файлы и, при необходимости, генерирует JavaScript. `tsc` — это проверка типов; Emit часто делегируется esbuild/swc для повышения скорости.

**When to use:** Любой нетривиальный ТС проект. Бегать `tsc --noEmit` в CI, даже когда дескрипторы Vite/esbuild выдают.

**Pluses and minuses:**

| +                                        | −                                                           |
| ---------------------------------------- | ----------------------------------------------------------- |
| Статическая безопасность типа в масштабе | Выдача медленнее, чем esbuild                               |
| `.d.ts` поколение для библиотек          | Конфигурация (`tsconfig.json`) имеет много вариантов        |
| Ссылки на проекты для монорепозиториев   | `tsc --noEmit` все еще необходим, даже когда Vite испускает |

**Examples:**

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "noEmit": true
  }
}
```

```json
// package.json — separate type-check from build
{ "scripts": { "typecheck": "tsc -b", "build": "vite build" } }
```

**Thoughts:** `moduleResolution: "bundler"` (TS 5+) соответствует поведению Vite/Webpack. Библиотеки, публикующие информацию в npm, нужны `declaration: true` и отдельный `tsconfig.build.json`.

**Sources:**

[typescriptlang.org](https://www.typescriptlang.org/)

---

</details>

<a name="bundling"></a>

## Bundling concepts

<details>
  <summary>Bundling</summary>

### Bundling

**Definition:** Процесс разрешения графа зависимостей модуля и объединения (или разделения) файлов в оптимизированный вывод для браузера или Node.

**When to use:** Все нетривиальные веб-приложения. Даже при использовании HTTP/2 объединение позволяет выполнять преобразования и оптимизацию.

**Pluses and minuses:**

| +                                                           | −                                                       |
| ----------------------------------------------------------- | ------------------------------------------------------- |
| Меньше HTTP-запросов                                        | Первоначальный пакет может вырасти до больших размеров  |
| Включает минимизацию и встряхивание дерева                  | Этап сборки усложняет                                   |
| Преобразует ресурсы, не относящиеся к JS (CSS, изображения) | Аннулирование кэша при любом изменении (без разделения) |

**Thoughts:** Встроенный ESM в браузерах снижает потребность в пакетировании для разработки (Vite), но рабочая среда по-прежнему объединяется для оптимизации.

**Sources:** [Vite — Возможности](https://vite.dev/guide/features.html)

</details>

<details>
  <summary>Single-page application (SPA)</summary>

### Single-page application (SPA)

**Definition:** Веб-приложение, которое загружает одну оболочку HTML и обрабатывает маршрутизацию на стороне клиента без полной перезагрузки страницы.

**When to use:** Панели мониторинга, панели администратора, приложения за авторизацией. Предпочитайте SSR/SSG (Next.js) для общедоступных SEO-критичных страниц. Типы: [6_web_api.md — Типы веб-приложений](./6_web_api_apps.md#types-of-web-apps). PWA (установка или оффлайн): [6_web_api.md — PWA](./6_web_api_apps.md#pwa-progressive-web-app).

**Pluses and minuses:**

| +                                            | −                                             |
| -------------------------------------------- | --------------------------------------------- |
| Быстрая навигация после начальной загрузки   | Большая начальная полезная нагрузка JS        |
| Богатая интерактивность клиентов             | SEO требует дополнительной работы (SSR/SSG)   |
| Упрощенное развертывание (статические файлы) | Время взаимодействия на медленных устройствах |

**Sources:** [MDN — SPA](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

</details>

<details>
  <summary>Code splitting</summary>

### Code splitting

**Definition:**

Метод разделения пакета на более мелкие фрагменты, загружаемые по требованию, — уменьшение начальной полезной нагрузки.

HTTP **кэш** уже пропускает сеть для повторных изображений (и часто JS/CSS). Скрипты по-прежнему **анализируют и выполняют** даже из кэша; большой JS стоит дорого. Разделение кода отправляет только код для текущего маршрута, а затем загружает остальной код при необходимости.

**When to use:** Любое приложение с маршрутами или тяжелыми дополнительными функциями (диаграммы, редакторы, разделы администратора).

**Pluses and minuses:**

| +                                       | −                                           |
| --------------------------------------- | ------------------------------------------- |
| Более быстрая начальная загрузка        | Больше HTTP-запросов (уменьшается HTTP/2)   |
| Отложенная загрузка на основе маршрутов | Состояния загрузки / Требуется приостановка |
| Параллельная загрузка фрагментов        | Сложность стратегии кэширования             |

**Examples:**

```tsx
import { lazy, Suspense } from "react";

const AdminPanel = lazy(() => import("./AdminPanel"));

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <AdminPanel />
    </Suspense>
  );
}
```

```js
// Webpack / Vite — dynamic import creates a separate chunk automatically
const module = await import("./heavy-lib");
```

**Thoughts:** Сначала разделите границы маршрутов, а затем тяжелые библиотеки. Анализируйте с `rollup-plugin-visualizer` или анализатор пакетов Webpack. Избегайте разделения крошечных модулей — накладные расходы превышают экономию. Меньший первоначальный JS также улучшает **INP** — [6_web_api.md](./6_web_api_security.md#inp-interaction-to-next-paint).

**Sources:** [MDN — Разделение кода](https://developer.mozilla.org/en-US/docs/Glossary/Code_splitting) · почему полезная нагрузка выросла: [Doka — Как работают веб-приложения](https://doka.guide/tools/web-app-works/)

</details>

<details>
  <summary>Tree shaking</summary>

### Tree shaking

**Definition:** Устранение мертвого кода — удаление неиспользуемых экспортов из окончательного пакета. Требуется ЕСМ `import`/`export` статический анализ.

**When to use:** Всегда в производственных сборках. Аудит с помощью пакетного анализатора.

**Pluses and minuses:**

| +                                                  | −                                                      |
| -------------------------------------------------- | ------------------------------------------------------ |
| Меньшие производственные пакеты                    | Разрушено модулями с побочными эффектами               |
| Никаких затрат во время выполнения                 | Модули CommonJS плохо трясутся                         |
| Работает автоматически с современными упаковщиками | Файлы ствола (`index.ts` реэкспорт) может победить его |

**Examples:**

```json
// package.json — mark side-effect-free for better shaking
{ "sideEffects": false }

// or list files with side effects
{ "sideEffects": ["*.css", "./src/polyfills.ts"] }
```

```js
// Bad — imports entire library
import _ from "lodash";
_.debounce(fn, 300);

// Good — imports only debounce
import debounce from "lodash/debounce";
```

**Thoughts:** `sideEffects: false` сообщает сборщикам пакетов, что они могут полностью удалить неиспользуемые модули. Импорт CSS — это побочные эффекты — перечислите их явно. Бочка `export *` файлы могут предотвратить встряхивание и раздуть пакет клиента (наносит вред **INP**) — [6_web_api.md — INP](./6_web_api_security.md#inp-interaction-to-next-paint).

</details>

<details>
  <summary>Source maps</summary>

### Source maps

**Definition:** Файлы, отображающие минифицированный/транспилированный код обратно в исходный исходный код для отладки.

**When to use:**

| Окружающая среда               | Рекомендуется                                    |
| ------------------------------ | ------------------------------------------------ |
| Развитие                       | `eval-source-map` или Vite по умолчанию (быстро) |
| Производство (с мониторингом)  | `hidden-source-map` (не указан в комплекте)      |
| Производство (без мониторинга) | `false`                                          |

**Pluses and minuses:**

| +                                                           | −                                                     |
| ----------------------------------------------------------- | ----------------------------------------------------- |
| Отладка производственных проблем с реальными номерами строк | Увеличьте время сборки и размер артефакта             |
| Мониторинг ошибок (Sentry) использует их                    | Раскрывать исходный код, если он опубликован публично |

**Examples:**

```ts
// vite.config.ts
export default defineConfig({
  build: {
    sourcemap: "hidden", // generate but don't expose in JS
  },
});
```

**Thoughts:** Загрузите исходные карты в Sentry/Datadog в CI, а затем удалите их из общедоступной CDN. Никогда не развертывайте `source-map` (встроенная ссылка) на производство — раскрывает бизнес-логику.

</details>

<details>
  <summary>Build-time vs runtime environment variables</summary>

### Build-time vs runtime environment variables

**Definition:**

| Тип                  | Когда решено                   | Видно в браузере?                               | Пример                              |
| -------------------- | ------------------------------ | ----------------------------------------------- | ----------------------------------- |
| **Время сборки**     | В `vite build` / `webpack`     | Да, если есть префикс (`VITE_`, `NEXT_PUBLIC_`) | `VITE_API_URL`                      |
| **Время выполнения** | При запуске сервера/контейнера | Внедряется сервером, а не в пакете JS           | Докер `ENV`, Карта конфигурации K8s |

**Thoughts:**

Никогда не храните секреты `VITE_*` or `NEXT_PUBLIC_*` — они встроены в клиентский пакет. Используйте серверную среду для ключей API.

---

**Sources:** [Vite — переменные Env](https://vite.dev/guide/env-and-mode.html)

</details>

<a name="bundlers"></a>

## Bundlers

<details>
  <summary>Webpack</summary>

### Webpack

**Definition:** Зрелый, легко настраиваемый сборщик модулей. Использует загрузчики для преобразования файлов и плагины для оптимизации.

**When to use:** Устаревшие приложения, микро-интерфейсы (Module Federation), сложные пользовательские цепочки загрузчиков.

**Pluses and minuses:**

| +                                          | −                                       |
| ------------------------------------------ | --------------------------------------- |
| Крупнейшая экосистема плагинов/загрузчиков | Медленный сервер разработки против Vite |
| Модуль Федерация для микро-интерфейсов     | Сложная конфигурация                    |
| Обрабатывает любые типы активов            | Крутая кривая обучения                  |

**Examples:**

```js
// webpack.config.js (minimal)
module.exports = {
  entry: "./src/index.tsx",
  output: { filename: "bundle.[contenthash].js", clean: true },
  module: {
    rules: [
      { test: /\.tsx?$/, use: "ts-loader", exclude: /node_modules/ },
      { test: /\.css$/, use: ["style-loader", "css-loader"] },
    ],
  },
  plugins: [new HtmlWebpackPlugin()],
  optimization: { splitChunks: { chunks: "all" } },
};
```

**Thoughts:**

| Концепция            | Роль                                                                                            |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| **Погрузчики**       | Преобразование файлов (TS → JS, SCSS → CSS)                                                     |
| **Плагины**          | Задачи уровня пакета (генерация HTML, минификация)                                              |
| **Модуль Федерации** | Совместное использование модулей во время выполнения между независимо развернутыми приложениями |

CRA использовал Webpack для внутренних целей (устарело). Предпочитайте Vite для новых приложений React.

**Sources:** [webpack.js.org](https://webpack.js.org/)

</details>

<details>
  <summary>Rollup</summary>

### Rollup

**Definition:** Первый сборщик ESM, оптимизированный для библиотек. Производит чистый, трясущийся результат.

**When to use:** Публикация npm-библиотек, систем дизайна, пакетов утилит.

**Pluses and minuses:**

| +                                                  | −                                                              |
| -------------------------------------------------- | -------------------------------------------------------------- |
| Лучший в своем классе результат библиотеки         | Менее подходит для сложных серверов разработки приложений      |
| Встроенная поддержка ESM                           | Экосистема плагинов меньше, чем Webpack                        |
| Используется внутри компании Vite для производства | Нет HMR (используется как этап сборки, а не сервер разработки) |

**Examples:**

```js
// rollup.config.js
import typescript from "@rollup/plugin-typescript";

export default {
  input: "src/index.ts",
  output: [
    { file: "dist/index.esm.js", format: "esm" },
    { file: "dist/index.cjs.js", format: "cjs" },
  ],
  plugins: [typescript()],
  external: ["react", "react-dom"],
};
```

**Thoughts:** Отметка `react`, `react-dom` as `external` в библиотеках — не объединяйте их. Поставьте как ESM, так и CJS для максимальной совместимости или только ESM для современных потребителей.

**Sources:** [rollupjs.org](https://rollupjs.org/)

</details>

<details>
  <summary>esbuild</summary>

### esbuild

**Definition:** Чрезвычайно быстрый сборщик и минификатор JavaScript/CSS, написанный на Go. Используется в качестве слоя транспиляции внутри Vite и других инструментов.

**When to use:** Транспиляция разработки (через Vite), быстрая минификация производства, пользовательские сценарии сборки.

**Pluses and minuses:**

| +                                       | −                                        |
| --------------------------------------- | ---------------------------------------- |
| В 10–100 раз быстрее, чем Webpack/Babel | Ограниченный API плагинов                |
| Встроенное преобразование TS/JSX        | Без проверки типа (в паре с `tsc`)       |
| Встроенная минификация                  | Некоторые крайние случаи несовместимости |

**Examples:**

```js
// build.mjs
import * as esbuild from "esbuild";

await esbuild.build({
  entryPoints: ["src/index.ts"],
  bundle: true,
  outfile: "dist/bundle.js",
  minify: true,
  sourcemap: true,
  target: "es2020",
});
```

**Thoughts:** esbuild удаляет типы, но не проверяет их. Всегда запускайте `tsc --noEmit` отдельно. Для библиотек отдавайте предпочтение Rollup для более точного управления выводом.

**Sources:** [esbuild.github.io](https://esbuild.github.io/)

</details>

<details>
  <summary>Vite</summary>

### Vite

**Definition:** Сервер разработки с использованием встроенной предварительной сборки ESM + esbuild; Накопительный пакет для производственных сборок. Набор инструментов по умолчанию для современных приложений React/Vue/Svelte.

**When to use:** Новые SPA, заменяющие CRA, сайты с библиотечной документацией и любые новые интерфейсы.

**Pluses and minuses:**

| +                                      | −                                                                       |
| -------------------------------------- | ----------------------------------------------------------------------- |
| Мгновенный холодный старт, быстрый HMR | Экосистема плагинов меньше, чем Webpack, для экзотических загрузчиков   |
| Разумные значения по умолчанию         | Возможно, потребуется устаревший CJS-тяжелый депс `optimizeDeps` тюнинг |
| Первоклассная поддержка TypeScript     | Другая ментальная модель от Webpack                                     |

**Examples:**

```ts
// vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig({
  plugins: [react()],
  server: {
    host: "0.0.0.0", // required inside Docker
    port: 3000,
  },
  resolve: {
    alias: { "@": path.resolve(__dirname, "src") },
  },
  build: {
    sourcemap: "hidden",
    rollupOptions: {
      output: { manualChunks: { vendor: ["react", "react-dom"] } },
    },
  },
});
```

**Thoughts:**

- В Докере: `host: '0.0.0.0'` - обязательность `localhost` слушает только внутри контейнера.
- Псевдонимы путей: настройте в обоих `vite.config.ts` (`resolve.alias`) и `tsconfig.json` (`paths`).
- Миграция CRA: удалить `react-scripts`, добавлять `vite`, `@vitejs/plugin-react`, двигаться `index.html` чтобы укоренить.

**Sources:** [vite.dev](https://vite.dev/)

</details>

<details>
  <summary>Turbopack</summary>

### Turbopack

**Definition:** Инкрементный сборщик на основе Rust, встроенный в Next.js (преемник Webpack в экосистеме Next).

**When to use:** Next.js 15+ проектов с `next dev --turbopack`. Не является заменой Vite для приложений, отличных от Next.

**Pluses and minuses:**

| +                                  | −                                                            |
| ---------------------------------- | ------------------------------------------------------------ |
| Очень быстрая разработка в Next.js | Только Next.js (не отдельный сборщик приложений)             |
| Инкрементные вычисления            | Все еще развивается по сравнению с паритетом функций Webpack |
| Интегрировано с сервером Next dev  | Менее актуально за пределами Next.js                         |

**Examples:**

```bash
next dev --turbopack
```

**Thoughts:** Турбопак ≠ Турборепо. Turborepo — это оркестратор задач монорепо; Турбопак – упаковщик. Используйте Turbopack для повышения скорости разработки; использовать Turborepo для кэширования `build`/`test` по пакетам.

**Sources:** [nextjs.org — Турбопак](https://nextjs.org/docs/architecture/turbopack)

</details>

<details>
  <summary>Webpack vs Vite</summary>

### Webpack vs Vite

**Definition:**

Параллельное сравнение двух наиболее распространенных на сегодняшний день сборщиков интерфейсов.

| Аспект            | Веб-пакет                       | Вите                             |
| ----------------- | ------------------------------- | -------------------------------- |
| Сервер разработки | Объединяет все приложение       | Собственный ESM + по требованию  |
| Скорость HMR      | Медленнее в больших приложениях | Почти мгновенно                  |
| Производство      | Веб-пакет                       | Накопительный пакет (через Vite) |
| Конфигурация      | Высокий                         | Низкий                           |
| Модуль Федерации  | Да                              | Ограниченная                     |

**How it works:** Устаревшие приложения, Федерация модулей. **Vite:** Новые SPA, миграция CRA.

**Examples:**

```js
// Webpack
module.exports = {
  entry: "./src/index.tsx",
  module: { rules: [{ test: /\.tsx?$/, use: "ts-loader" }] },
};

// Vite
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
export default defineConfig({ plugins: [react()] });
```

**Sources:** [Vite — Почему Vite](https://vite.dev/guide/why.html) · [Концепции Webpack](https://webpack.js.org/concepts/)

</details>

<details>
  <summary>Bundler comparison</summary>

### Bundler comparison

| | Веб-пакет | Свернуть | эсбилд | Вите | Турбопак |
| | --- | --------- | -------- | --------- | ------ |
| | **Лучший вариант** | Устаревшие приложения, МФ | Библиотеки | Скрипты, скорость | Современные СПА | Разработчик Next.js |
| | **Дев ХМР** | Медленно | Нет | Нет | Быстро | Быстро |
| | **Конфигурация** | Высокий | Средний | Низкий | Низкий | Нет (Следующий) |
| | **Экосистема** | Крупнейший | Средний | Выращивание | Большой | Только следующие |

---

**Sources:** [Vite — Почему Vite](https://vite.dev/guide/why.html)

</details>

<details>
  <summary>Turborepo</summary>

### Turborepo

**Definition:** Система сборки для монорепозиториев JavaScript — кэширует выходные данные и запускает задачи. `build`/`test`/`lint` параллельно между пакетами.

**When to use:** Монорепозитории с несколькими приложениями/пакетами. Сопряжение с рабочими пространствами pnpm.

**Pluses and minuses:**

| +                                    | −                                         |
| ------------------------------------ | ----------------------------------------- |
| Скорость удаленного кэширования CI   | Специально для монорепо                   |
| Простой `turbo.json` трубопровод     | Требуется настройка рабочего пространства |
| Работает с Vite/Next в каждом пакете | Не упаковщик                              |

**Examples:**

```json
{
  "tasks": {
    "build": { "dependsOn": ["^build"], "outputs": ["dist/**"] },
    "test": { "dependsOn": ["build"] }
  }
}
```

**Thoughts:** Турборепо ≠ Турбопак. Турборепо организует задачи; Turbopack — это сборщик Next.js.

**Sources:** [turbo.build](https://turbo.build/repo/docs)

</details>

<a name="code-quality"></a>

## Code quality

Зачем писать тесты и AAA: [7_special.md — Зачем писать тесты](./7_special_testing_fundamentals.md#tests-and-aaa). Шутка (`jest.config.js`, `npm test`, Node vs jsdom): [7_special.md — Jest](./7_special_testing_tooling.md#jest--first-tests-node-vs-jsdom). Заглушки/издевательства/шпионы: [Тестовые дубли](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies). Проволока `npm test` в CI: [Действия GitHub](./10_build_ci.md#github-actions).

<details>
  <summary>Linter vs formatter</summary>

### Linter vs formatter

**Definition:**

| Инструмент   | Цель                                                | Настраиваемый?                | Примеры          |
| ------------ | --------------------------------------------------- | ----------------------------- | ---------------- |
| **Форматёр** | Визуальная верстка (отступ, кавычки, ширина строки) | Упрямый, минимальный          | Красивее, дпринт |
| **Линтер**   | Логические ошибки, плохие шаблоны, лучшие практики  | Широкие возможности настройки | ESLint, Биом     |

**When to use:** Каждый проект. Сначала запустите форматтер, затем линтер. Отключите правила форматирования ESLint, которые конфликтуют с Prettier, через `eslint-config-prettier`.

**Pluses and minuses:**

| + (используя оба)                             | −                                       |
| --------------------------------------------- | --------------------------------------- |
| Единый стиль + меньше ошибок                  | Перекрытие правил приводит к конфликтам |
| Автоматизация в CI и предварительная фиксация | Время первоначальной настройки          |
| Адаптация — никаких дебатов о стиле           | Ложные срабатывания без настройки       |

**Examples:**

```bash
# Typical pre-commit order
prettier --write .
eslint . --fix
```

**Thoughts:** Не используйте ESLint для форматирования — это работа Prettier. Biome сочетает в себе и то, и другое, но экосистема моложе, чем ESLint + Prettier.

</details>

<details>
  <summary>ESLint</summary>

### ESLint

**Definition:** Подключаемый инструмент статического анализа для JavaScript/TypeScript. Обеспечивает соблюдение правил качества кода и выявляет ошибки еще до выполнения.

**When to use:** Все проекты JS/TS. Расширить пресеты фреймворка (`eslint-config-next`, `@typescript-eslint`).

**Pluses and minuses:**

| +                                                   | −                                        |
| --------------------------------------------------- | ---------------------------------------- |
| Огромная экосистема плагинов (React, import, a11y)  | Конфигурация может быть сложной          |
| Автоисправление многих правил                       | Ложные срабатывания без настройки правил |
| Плоская конфигурация (ESLint 9+) упрощает настройку | Миграция из `.eslintrc` продолжающийся   |

**Examples:**

```js
// eslint.config.js (flat config, ESLint 9+)
import js from "@eslint/js";
import tseslint from "typescript-eslint";
import reactHooks from "eslint-plugin-react-hooks";
import prettier from "eslint-config-prettier";

export default tseslint.config(
  js.configs.recommended,
  ...tseslint.configs.strict,
  reactHooks.configs["recommended-latest"],
  prettier,
  { rules: { "no-console": "warn" } },
);
```

**Thoughts:**

- Использовать `typescript-eslint` с `strict` or `recommended-type-checked` за максимальную стоимость.
- `eslint-plugin-import` перехватывает неправильные пути импорта; `eslint-plugin-jsx-a11y` для доступности.
- Запуск в CI с `--max-warnings 0` как только кодовая база станет чистой.

**Sources:** [eslint.org](https://eslint.org/)

</details>

<details>
  <summary>Prettier</summary>

### Prettier

**Definition:** Упрямый форматировщик кода. Обеспечивает единый стиль с нулевой конфигурацией.

**When to use:** Каждый проект. Сопряжение с `eslint-config-prettier` чтобы отключить конфликтующие правила стиля ESLint.

**Pluses and minuses:**

| +                                                  | −                                                                   |
| -------------------------------------------------- | ------------------------------------------------------------------- |
| Нулевая настройка, согласованность во всей команде | Невозможно многое настроить (по замыслу)                            |
| Поддерживает JS, TS, CSS, HTML, JSON, MD           | Может форматировать созданные файлы (используйте `.prettierignore`) |
| Интеграция редактора                               | Длинные строки в JSX могут быть некрасивыми                         |

**Examples:**

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

```json
// package.json
{
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

**Thoughts:** Добавлять `format:check` в CI (не пишите в CI). Использовать `.prettierignore` для `dist/`, `coverage/`, сгенерированные типы GraphQL.

**Sources:**

[prettier.io](https://prettier.io/)

---

</details>

<a name="docker"></a>
