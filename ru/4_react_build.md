# React — сборка (build tooling)

Vite и Webpack для React SPA: конфигурация, code splitting и tree shaking.

[← Назад к хабу](./4_react.md)

### Related docs

[React — хаб](./4_react.md) · [Performance — code splitting](./4_react_perf.md#code-splitting) · [Build — Bundlers (канон)](./10_build_bundlers.md) · [TypeScript — tsconfig](./5_ts_basics.md#strict-compiler-options)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Dev- и production-пайплайн для React |
| [Vite for React](#vite-for-react) | `vite.config.ts`, алиасы, env |
| [Webpack for React](#webpack-for-react) | Когда ещё используют; Module Federation |
| [Code splitting in React](#code-splitting-in-react) | Маршруты, `lazy`, manual chunks |
| [Tree shaking](#tree-shaking) | ESM, side effects, barrel-файлы |

### Sections

- [Overview](#overview)
- [Vite for React](#vite-for-react)
- [Webpack for React](#webpack-for-react)
- [Code splitting in React](#code-splitting-in-react)
- [Tree shaking](#tree-shaking)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **React build pipeline** проверяет типы (TypeScript), собирает модули, делит код на чанки, удаляет неиспользуемые экспорты и отдаёт статические ассеты для деплоя или SSR-фреймворков.

**How it works:**

| Phase | Tool (typical Vite SPA) | Output |
| ----- | ------------------------ | ------ |
| Type-check | `tsc --noEmit` | Только ошибки |
| Dev server | Vite + `@vitejs/plugin-react` | ESM по HTTP, HMR |
| Production | Rollup (внутри Vite) | JS/CSS с хешами в `dist/` |

**When to use:** **Vite** — для новых React SPA; **Webpack** — для legacy-приложений, кастомных loaders или **Module Federation** ([Architecture — MFE](./4_react_architecture.md#micro-frontends-mfe)). У Next.js свой бандлер — [Next.js platform](./4_nextjs_platform.md).

**Thoughts:** Теория бандлеров → [Build — Bundlers](./10_build_bundlers.md). Сплит маршрутов в React → [Code splitting in React](#code-splitting-in-react).

**Sources:** [vite.dev](https://vite.dev/) · [Webpack concepts](https://webpack.js.org/concepts/)

</details>

---

## Vite for React

<details>
<summary>Vite for React</summary>

**Definition:** **Vite** — стандартный toolchain для современных React SPA: быстрый dev через native ESM, production-сборка через Rollup.

**How it works:**

| Config area | Purpose |
| ----------- | ------- |
| `plugins: [react()]` | Fast Refresh, JSX |
| `resolve.alias` | `@/` → `src/` (дублируйте в `tsconfig` `paths`) |
| `server.host` / `port` | URL dev-сервера; в Docker — `0.0.0.0` |
| `build.rollupOptions.output.manualChunks` | Разделение vendor и app |
| `define` | Подстановка флагов `import.meta.env` |

**When to use:** Замена CRA с нуля, внутренние дашборды, библиотеки в Vite library mode.

**Examples:**

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path";

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: { "@": path.resolve(__dirname, "src") },
  },
  server: { port: 3000, host: true },
  build: {
    sourcemap: true,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ["react", "react-dom"],
          router: ["react-router-dom"],
        },
      },
    },
  },
});
```

**Pluses and minuses:**

| + | − |
| - | - |
| Быстрый HMR | Часть CJS-зависимостей требует `optimizeDeps.include` |
| Простые дефолты | Не production-бандлер Next.js |

**Sources:** [Vite — config](https://vite.dev/config/) · [Build — Vite](./10_build_bundlers.md#vite)

</details>

---

## Webpack for React

<details>
<summary>Webpack for React</summary>

**Definition:** **Webpack** строит граф зависимостей и отдаёт оптимизированные чанки — по-прежнему часто в **legacy** React-приложениях и хостах **Module Federation**.

**How it works:**

| Piece | Role |
| ----- | ---- |
| `entry` | Точка входа (`src/main.tsx`) |
| `module.rules` | `babel-loader` / `ts-loader` для TS/JSX |
| `plugins` | `HtmlWebpackPlugin`, `DefinePlugin` |
| `optimization.splitChunks` | Разделение vendor/common |
| `ModuleFederationPlugin` | MFE remotes/exposes |

**When to use:** Поддержка существующих Webpack-репозиториев; Federation; команды с длинными цепочками кастомных loaders.

**Examples:**

```js
// webpack.config.js (minimal React)
module.exports = {
  entry: "./src/main.tsx",
  resolve: { extensions: [".tsx", ".ts", ".js"] },
  module: {
    rules: [{ test: /\.tsx?$/, use: "ts-loader", exclude: /node_modules/ }],
  },
  optimization: {
    splitChunks: { chunks: "all", cacheGroups: { vendor: { test: /node_modules/ } } },
  },
};
```

**Thoughts:** Новые SPA → предпочитайте [Vite for React](#vite-for-react). Сравнение → [Webpack vs Vite](./10_build_bundlers.md#webpack-vs-vite).

**Sources:** [Webpack — Getting started](https://webpack.js.org/guides/getting-started/) · [Build — Webpack](./10_build_bundlers.md#webpack)

</details>

---

## Code splitting in React

<details>
<summary>Code splitting in React</summary>

**Definition:** **Code splitting** отдаёт только JS, нужный для текущего маршрута или фичи, остальные чанки подгружает по запросу — критично для React SPA с тяжёлыми admin- или chart-модулями.

**How it works:**

| Strategy | Mechanism |
| -------- | --------- |
| **Route-level** | `React.lazy(() => import('./Page'))` + `<Suspense>` |
| **Dynamic import** | `import()` в обработчиках для редких диалогов |
| **Bundler chunks** | Vite/Webpack автоматически чанкуют динамический `import()` |
| **Manual vendor** | `manualChunks` / `splitChunks` для `react`, `lodash` и т.д. |

**When to use:** Любое multi-route приложение; до оптимизации `memo` — сначала измерьте бандл ([Performance](./4_react_perf.md)).

**Examples:**

```tsx
import { lazy, Suspense } from "react";
import { createBrowserRouter, RouterProvider } from "react-router-dom";

const Dashboard = lazy(() => import("@/pages/Dashboard"));
const Settings = lazy(() => import("@/pages/Settings"));

const router = createBrowserRouter([
  {
    path: "/",
    element: (
      <Suspense fallback={<p>Loading…</p>}>
        <Dashboard />
      </Suspense>
    ),
  },
  {
    path: "/settings",
    element: (
      <Suspense fallback={<p>Loading…</p>}>
        <Settings />
      </Suspense>
    ),
  },
]);

export function App() {
  return <RouterProvider router={router} />;
}
```

**Thoughts:** Избегайте waterfall: сплит маршрута **и** prefetch данных через loaders роутера ([Ecosystem — Client routing](./4_react_ecosystem.md#client-routing)). Анализ бандла — `rollup-plugin-visualizer`.

**Sources:** [Build — Code splitting](./10_build_bundlers.md#code-splitting) · [react.dev — lazy](https://react.dev/reference/react/lazy)

</details>

---

## Tree shaking

<details>
<summary>Tree shaking</summary>

**Definition:** **Tree shaking** удаляет неиспользуемые **экспорты** из production-бандла статическим анализом ESM.

**How it works:**

| Helps shaking | Hurts shaking |
| ------------- | ------------- |
| `import { fn } from 'lib'` | `import * as lib` и одна функция |
| `"sideEffects": false` в `package.json` зависимости | Top-level код с side effects |
| Прямые импорты | Огромный barrel `index.ts` с re-export всего |
| ESM-пакеты | Legacy только CJS |

**When to use:** Всегда в production; аудит, если в бандл попадает целая библиотека (весь `lodash` vs `lodash-es` + импорт по функциям).

**Examples:**

```json
{
  "name": "my-ui-kit",
  "sideEffects": ["**/*.css"]
}
```

```ts
// Prefer
import debounce from "lodash-es/debounce";
// Avoid if it pulls the whole package
import { debounce } from "lodash";
```

**Thoughts:** TypeScript сам не «трясёт» — это делает **бандлер**. В паре с [Vite build](./10_build_bundlers.md#vite) `build.minify`.

**Sources:** [Build — Tree shaking](./10_build_bundlers.md#tree-shaking) · [Rollup — tree-shaking](https://rollupjs.org/tutorial-tree-shaking)

</details>
