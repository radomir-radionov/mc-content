# React — Build tooling

Vite and Webpack for React SPAs: config, code splitting, and tree shaking.

[← Back to hub](./4_react.md)

### Related docs

[React — hub](./4_react.md) · [Performance — code splitting](./4_react_perf.md#code-splitting) · [Build — Bundlers (canonical)](./10_build_bundlers.md) · [TypeScript — tsconfig](./5_ts_basics.md#strict-compiler-options)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Dev vs production pipeline for React |
| [Vite for React](#vite-for-react) | `vite.config.ts`, aliases, env |
| [Webpack for React](#webpack-for-react) | When still used; Module Federation |
| [Code splitting in React](#code-splitting-in-react) | Routes, `lazy`, manual chunks |
| [Tree shaking](#tree-shaking) | ESM, side effects, barrel files |

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

**Definition:** A **React build pipeline** type-checks (TypeScript), bundles modules, splits code, shakes unused exports, and emits static assets for deploy or SSR frameworks.

**How it works:**

| Phase | Tool (typical Vite SPA) | Output |
| ----- | ------------------------ | ------ |
| Type-check | `tsc --noEmit` | Errors only |
| Dev server | Vite + `@vitejs/plugin-react` | ESM over HTTP, HMR |
| Production | Rollup (inside Vite) | Hashed JS/CSS in `dist/` |

**When to use:** **Vite** for new React SPAs; **Webpack** for legacy apps, custom loaders, or **Module Federation** ([Architecture — MFE](./4_react_architecture.md#micro-frontends-mfe)). Next.js uses its own bundler — [Next.js platform](./4_nextjs_platform.md).

**Thoughts:** Deep bundler theory → [Build — Bundlers](./10_build_bundlers.md). React route splitting → [Code splitting in React](#code-splitting-in-react).

**Sources:** [vite.dev](https://vite.dev/) · [Webpack concepts](https://webpack.js.org/concepts/)

</details>

---

## Vite for React

<details>
<summary>Vite for React</summary>

**Definition:** **Vite** is the default toolchain for modern React SPAs — fast dev via native ESM, production build via Rollup.

**How it works:**

| Config area | Purpose |
| ----------- | ------- |
| `plugins: [react()]` | Fast Refresh, JSX |
| `resolve.alias` | `@/` → `src/` (mirror in `tsconfig` `paths`) |
| `server.host` / `port` | Dev URL; use `0.0.0.0` in Docker |
| `build.rollupOptions.output.manualChunks` | Vendor vs app split |
| `define` | Inject `import.meta.env` flags |

**When to use:** Greenfield CRA replacement, internal dashboards, libraries with Vite library mode.

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
| Fast HMR | Some CJS deps need `optimizeDeps.include` |
| Simple defaults | Not the Next.js production bundler |

**Sources:** [Vite — config](https://vite.dev/config/) · [Build — Vite](./10_build_bundlers.md#vite)

</details>

---

## Webpack for React

<details>
<summary>Webpack for React</summary>

**Definition:** **Webpack** resolves a dependency graph and emits optimized chunks — still common in **legacy** React apps and **Module Federation** hosts.

**How it works:**

| Piece | Role |
| ----- | ---- |
| `entry` | App bootstrap (`src/main.tsx`) |
| `module.rules` | `babel-loader` / `ts-loader` for TS/JSX |
| `plugins` | `HtmlWebpackPlugin`, `DefinePlugin` |
| `optimization.splitChunks` | Vendor/common splits |
| `ModuleFederationPlugin` | MFE remotes/exposes |

**When to use:** Maintaining existing Webpack repos; Federation; teams with heavy custom loader chains.

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

**Thoughts:** New SPAs → prefer [Vite for React](#vite-for-react). Compare → [Webpack vs Vite](./10_build_bundlers.md#webpack-vs-vite).

**Sources:** [Webpack — Getting started](https://webpack.js.org/guides/getting-started/) · [Build — Webpack](./10_build_bundlers.md#webpack)

</details>

---

## Code splitting in React

<details>
<summary>Code splitting in React</summary>

**Definition:** **Code splitting** ships only the JS needed for the current route or feature, loading other chunks on demand — critical for React SPAs with large admin or chart modules.

**How it works:**

| Strategy | Mechanism |
| -------- | --------- |
| **Route-level** | `React.lazy(() => import('./Page'))` + `<Suspense>` |
| **Dynamic import** | `import()` in event handlers for rare dialogs |
| **Bundler chunks** | Vite/Webpack auto-chunk on dynamic `import()` |
| **Manual vendor** | `manualChunks` / `splitChunks` for `react`, `lodash`, etc. |

**When to use:** Any multi-route app; before optimizing `memo` — measure bundle first ([Performance](./4_react_perf.md)).

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

**Thoughts:** Avoid waterfall: split route **and** prefetch data with router loaders ([Ecosystem — Client routing](./4_react_ecosystem.md#client-routing)). Analyze bundles with `rollup-plugin-visualizer`.

**Sources:** [Build — Code splitting](./10_build_bundlers.md#code-splitting) · [react.dev — lazy](https://react.dev/reference/react/lazy)

</details>

---

## Tree shaking

<details>
<summary>Tree shaking</summary>

**Definition:** **Tree shaking** removes unused **exports** from the production bundle using static ESM analysis.

**How it works:**

| Helps shaking | Hurts shaking |
| ------------- | ------------- |
| `import { fn } from 'lib'` | `import * as lib` then use one fn |
| `"sideEffects": false` in dependency `package.json` | Side-effectful module top-level code |
| Direct imports | Huge barrel `index.ts` re-exporting everything |
| ESM packages | Legacy CJS-only packages |

**When to use:** Always in production; audit when bundle includes full libraries (e.g. all of `lodash` vs `lodash-es` + per-function imports).

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

**Thoughts:** TypeScript does not shake — the **bundler** does. Pair with [Vite build](./10_build_bundlers.md#vite) `build.minify`.

**Sources:** [Build — Tree shaking](./10_build_bundlers.md#tree-shaking) · [Rollup — tree-shaking](https://rollupjs.org/tutorial-tree-shaking)

</details>
