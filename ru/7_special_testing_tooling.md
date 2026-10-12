# Testing — Tooling

Jest, Vitest, React Testing Library, MSW, and code coverage.

[← Back to hub](./7_special_testing.md)

### Related docs

- [Fundamentals](./7_special_testing_fundamentals.md) · [Layers](./7_special_testing_layers.md) · [Practices](./7_special_testing_practices.md) · [Tooling](./7_special_testing_tooling.md) · [React — Testing](./4_react_ecosystem.md#testing) · [Next.js — Vitest + RTL](./4_nextjs_platform.md#testing-nextjs-vitest--rtl) · [CI/CD](./10_build_ci.md#continuous-integration-ci) · [Interview leftovers](./7_special_interview.md)

### Sections

- [Tooling](#tooling)
- [Vitest vs Jest](#vitest-vs-jest)
- [React Testing Library](#react-testing-library)
- [MSW (Mock Service Worker)](#msw-mock-service-worker)
- [Code coverage](#code-coverage)
- [Jest — first tests (Node vs jsdom)](#jest-first-tests-node-vs-jsdom)

---

<a name="tooling"></a>

## Tooling

<details>
  <summary>Vitest vs Jest</summary>

### Vitest vs Jest

**Definition:** Two JavaScript test runners with the same mental model (`describe` / `it` / `expect`) but different runtimes. **Jest** is the long-standing default; **Vitest** is Vite-native, ESM-first, and Jest-compatible for most day-to-day APIs.

**How it works:**

| | **Jest** | **Vitest** |
| | ---------------- | --------------------------------------- |
| | Runtime | Own transformer (often Babel) | Reuses Vite pipeline |
| | ESM / TypeScript | Works; config-heavy in some repos | Native in Vite projects |
| | API | `jest.fn`, `jest.mock`, `jest.spyOn` | `vi.fn`, `vi.mock`, `vi.spyOn` |
| | DOM env | `jest-environment-jsdom` (separate pkg) | `environment: 'jsdom'` or `happy-dom` in config |
| | Watch | `--watch`, `-o` (changed files) | Fast HMR-style watch |
| | Coverage | `--coverage`, `coverageThreshold` | `vitest run --coverage` (v8 or istanbul) |

Pick **Vitest** for Vite, Next.js (modern), Nuxt, and greenfield ESM repos. Pick **Jest** when the team already standardized on it or tooling (Create React App legacy, some monorepos) assumes Jest. Both pair with [React Testing Library](#react-testing-library) and [MSW](#msw-mock-service-worker).

**When to use:** New Vite/Next app → Vitest. Existing Jest suite with hundreds of tests → migrate only when the DX win is worth `vi.mock` factory shape changes and config work.

#### Common pitfalls

- **`vi.mock('./path', () => 'hello')`** — Vitest needs a module object: `{ default: 'hello' }`, not a bare string ([Vitest migration guide](https://vitest.dev/guide/migration)).
- **Assuming 1:1 Jest plugins** — some reporters/setup files differ; read the migration page before flipping CI.
- **Two runners in one repo** — pick one per package; do not run Jest and Vitest on the same sources without a reason.

**Examples:**

```ts
// vitest.config.ts
import { defineConfig } from "vitest/config";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  test: {
    environment: "jsdom",
    setupFiles: ["./vitest.setup.ts"],
  },
});
```

```ts
// vitest.setup.ts
import "@testing-library/jest-dom/vitest";
```

Framework-specific wiring (Server Actions, `next/navigation` mocks): [Next.js — Vitest + RTL](./4_nextjs_platform.md#testing-nextjs-vitest--rtl).

**Thoughts:** Same pyramid and AAA rules regardless of runner — swap tooling, not strategy.

**Sources:** [Vitest — features & config](https://vitest.dev/guide/features) · [Vitest — Jest migration](https://vitest.dev/guide/migration) · [Jest docs](https://jestjs.io/docs/getting-started)

---

</details>

<details>
  <summary>React Testing Library</summary>

### React Testing Library

**Definition:** A library for testing React components by **user-visible behavior** — roles, labels, and text — not implementation details (internal state, hook call order, private helpers).

**How it works:**

1. **Render** — `render(<LoginForm />)` mounts into jsdom (or a real browser in E2E).
2. **Query** — prefer `getByRole`, `getByLabelText`, `findBy*` for async UI; avoid `querySelector` and test IDs unless roles are impossible.
3. **Act** — `userEvent` (preferred) or `fireEvent` simulates keyboard/pointer input.
4. **Assert** — visible output (`expect(screen.getByText(...))`), not `useState` snapshots.

RTL sits in the **integration** layer of the [pyramid](./7_special_testing_fundamentals.md#test-pyramid): several components + mocked I/O, still not a full browser product test. Pair with Jest or Vitest as the runner; mock HTTP with [MSW](#msw-mock-service-worker) instead of stubbing child components.

**When to use:** Forms, conditional UI, error states, accessibility-sensitive flows. **Skip:** third-party library internals, private functions, shallow snapshots of entire trees.

**Pluses and minuses:**

| +                                       | −                                     |
| --------------------------------------- | ------------------------------------- |
| Tests resemble how users and AT find UI | jsdom lacks real layout and some APIs |
| Encourages accessible markup            | Heavy `next/*` mocking in App Router  |

**Examples:**

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { LoginForm } from "./LoginForm";

test("shows welcome after sign-in", async () => {
  const user = userEvent.setup();
  render(<LoginForm />);
  await user.type(screen.getByLabelText(/email/i), "ada@example.com");
  await user.click(screen.getByRole("button", { name: /sign in/i }));
  expect(await screen.findByText(/welcome/i)).toBeInTheDocument();
});
```

**Thoughts:** Test like a user — roles and labels, not class names. `userEvent` over `fireEvent` for realistic input.

**See also:** [Integration testing](./7_special_testing_layers.md#integration-testing) · [E2E testing](./7_special_testing_layers.md#e2e-testing) · [React ecosystem pointer](./4_react_ecosystem.md#testing)

**Sources:** [Testing Library — guiding principles](https://testing-library.com/docs/guiding-principles) · [RTL — React intro](https://testing-library.com/docs/react-testing-library/intro/)

---

</details>

<details>
  <summary>MSW (Mock Service Worker)</summary>

### MSW (Mock Service Worker)

**Definition:** **Mock Service Worker** intercepts HTTP (and GraphQL / WebSocket) at the **network** layer so tests and local dev use the same handlers — without rewriting `fetch` in every file.

**How it works:**

- **`setupServer(...handlers)`** (Node / Vitest / Jest) registers interceptors for `fetch`, XHR, and Node `ClientRequest`.
- Handlers use `http.get/post/...` and return `HttpResponse.json(...)` (or errors, delays, empty bodies).
- **`server.listen()`** in `beforeAll`, **`server.close()`** in `afterAll`; per-test overrides with `server.use(...)`.
- Prefer MSW when many tests share API contracts; use `jest.fn` on `global.fetch` only for a one-off unit ([Jest — first tests](#jest--first-tests-node-vs-jsdom)).

**When to use:** Integration tests for pages that `fetch` REST/GraphQL; contract-style checks that the client sends the right URL/body; Storybook / local dev without a live backend.

#### Common pitfalls

- **Forgot `server.listen()`** — requests hit the real network and flake.
- **Handlers out of sync with OpenAPI** — update MSW when the API changes.
- **Replacing all unit doubles with MSW** — pure functions still need no network mock.

**Examples:**

```ts
import { http, HttpResponse } from "msw";
import { setupServer } from "msw/node";

const server = setupServer(
  http.get("/books", () =>
    HttpResponse.json([{ id: "1", title: "The Lord of the Rings" }]),
  ),
);

beforeAll(() => server.listen({ onUnhandledRequest: "warn" }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test("loads books", async () => {
  const res = await fetch("/books");
  expect(await res.json()).toEqual([
    { id: "1", title: "The Lord of the Rings" },
  ]);
});
```

**Thoughts:** MSW keeps the **client code path** real while controlling the wire — closer to production than mocking `fetch` with a bare `jest.fn`.

**See also:** [Integration testing](./7_special_testing_layers.md#integration-testing) · [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies)

**Sources:** [MSW — `setupServer`](https://mswjs.io/docs/api/setup-server) · [MSW — Node integration](https://mswjs.io/docs/integrations/node)

---

</details>

<details>
  <summary>Code coverage</summary>

### Code coverage

**Definition:** A metric of **which lines, branches, functions, and statements** ran during tests — usually reported as percentages and highlighted in HTML reports.

**How it works:**

- **Lines / statements** — did this code execute?
- **Branches** — did both sides of `if` / `switch` run?
- **Functions** — was each function called at least once?
- **100% coverage ≠ bug-free** — you can execute every line without asserting behavior. Coverage is a **gap finder**, not a quality score.
- **Jest:** `--coverage`, `collectCoverageFrom`, `coverageThreshold` (fail CI below a minimum). Collecting coverage slows runs — scope it to `./src` and exclude config/tests.
- **Vitest:** `vitest run --coverage` with the v8 or istanbul provider.

Aim for **meaningful** coverage on business logic and critical paths; do not chase 100% on glue, generated code, or third-party wrappers.

**When to use:** CI gate on core modules (e.g. 80% lines on `src/lib/`); finding untested branches after a refactor; interview question “is 100% required?” → **no**, but zero coverage on payment/auth is unacceptable.

**Pluses and minuses:**

| +                                      | −                                                       |
| -------------------------------------- | ------------------------------------------------------- |
| Shows dead code and missed branches    | Vanity metrics; tests that hit lines but assert nothing |
| `coverageThreshold` blocks regressions | Slower CI when enabled globally                         |

**Examples:**

```js
// jest.config.js
module.exports = {
  collectCoverageFrom: ["src/**/*.{js,ts,tsx}", "!src/**/*.test.*"],
  coverageThreshold: {
    global: { lines: 80, branches: 70, functions: 80, statements: 80 },
  },
};
```

```json
{ "scripts": { "test:coverage": "vitest run --coverage" } }
```

**Thoughts:** Cover **behavior** first; use coverage reports to find holes, not as the definition of “done.”

**Sources:** [Jest — `coverageThreshold`](https://jestjs.io/docs/configuration#coveragethreshold-object) · [Jest — `collectCoverageFrom`](https://jestjs.io/docs/configuration#collectcoveragefrom-array) · [Vitest — coverage](https://vitest.dev/guide/features#coverage)

---

</details>

<details>
  <summary>Jest — first tests (Node vs jsdom)</summary>

### Jest — first tests (Node vs jsdom)

**Definition:** Jest is a JS test runner with assertions, mocks, and two built-in environments: **`node`** (default, no `document`) and **`jsdom`** (browser APIs in Node). Use it to lock behavior of utilities, async I/O, and DOM code so refactors and CI fail loudly.

**How it works:**

- Tests document behavior, catch regressions on CI / pre-commit, and make refactors cheap.
- Pick the runner for the layer you test: unit (Jest/Vitest), component (RTL), E2E (Playwright/Cypress). Pyramid: [Test pyramid](./7_special_testing_fundamentals.md#test-pyramid). Vitest vs Jest: [Vitest vs Jest](#vitest-vs-jest). Focus helpers, `-o` / `--watch`, `--restoreMocks`: [Making tests easy](./7_special_testing_practices.md#making-tests-easy).
- **Config** lives in `jest.config.js` (or `package.json`). Defaults: `testEnvironment: 'node'`. Transform with `babel-jest` so tests can use ESM and JSX. `setupFilesAfterEnv` runs after the framework loads (custom matchers, global setup).
- **Per-file env:** a JSDoc docblock overrides the global env — needed when one suite is Node (build scripts) and another needs `document`.
- Jest 28+: `jsdom` is the extra package `jest-environment-jsdom`, not bundled with `jest`.
- **`jest.fn()`** replaces a function and records calls (`mock.calls`, `toHaveBeenCalledTimes`). **`jest.mock(path, factory)`** replaces a whole module (hoisted above imports). Stub vs mock vs spy: [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies).
- Importing a module **runs it**. If the import graph touches `document` at load time, the test fails before your `it()` — that is an import side effect, not “Jest forgot DOM”.

**When to use:**

- Node env: formatters, bundler/platform scripts, anything without DOM.
- jsdom: querySelector, `classList`, events — emulate a page in Node.
- Mock `fetch` / modules when you want offline, deterministic tests (not a live API).

#### Common pitfalls

- **Need to `export` only for tests** — either the function should already be public, or it should not be unit-tested (test through the public API).
- **Import side effects** — modules that touch DOM/globals on load are hard to test; prefer pure functions, effects at the edge ([Side effects](./3_js_async_callbacks.md#side-effects)).
- **`fetch` polyfill vs mock** — real requests are integration tests; unit tests should not require the network.
- **jsdom ≠ Chrome** — no real layout, incomplete Web APIs; full pages → [E2E](./7_special_testing_layers.md#e2e-testing). Real `fetch` → [integration](./7_special_testing_layers.md#integration-testing), not a unit test.
- **`jest.mock` path** must match the module the SUT imports.

**Pluses and minuses:**

| +                                                         | −                                                                       |
| --------------------------------------------------------- | ----------------------------------------------------------------------- |
| One runner for Node + browser-like tests                  | jsdom is not a real browser (no layout, limited APIs)                   |
| Built-in mocks; `--watch` reruns related tests            | Mocking `fetch` tests the mock, not the network                         |
| Living docs of public APIs                                | Import-time effects make setup painful                                  |
| Cheap guard on tiny functions (signature changes fail CI) | Changing production code _only_ for tests often means the API was wrong |

**Examples:**

```js
// jest.config.js
module.exports = {
  testEnvironment: "node", // or 'jest-environment-node'
  setupFilesAfterEnv: ["<rootDir>/jest.setup.js"],
  transform: {
    "\\.[jt]sx?$": "babel-jest",
  },
};
```

```json
{ "scripts": { "test": "jest" } }
```

```bash
npm test
npm test -- --watch   # rerun tests for changed files
```

```js
// title-formatter.js
function titleFormatter(segments) {
  return segments.filter(Boolean).join(" — ");
}

// __tests__/title-formatter.js
import { titleFormatter } from "../title-formatter";

describe("titleFormatter", () => {
  it("joins truthy segments with an em dash", () => {
    expect(titleFormatter(["test", "test2"])).toEqual("test — test2");
  });
});
```

If someone swaps the signature to `titleFormatter(separator, segments)` without updating callers, the test fails instead of shipping `Cannot read properties of undefined (reading 'filter')`.

`fetch` is missing in Node unless polyfilled. A real network test couples you to the API and needs online. Stub `global.fetch` so it returns `{ json: () => Promise.resolve(data) }` — that is the `Response` shape `search()` uses.

```js
describe("searchClient", () => {
  it("returns JSON and hits the search query param", async () => {
    const expectedResult = {
      title: "Как и зачем писать тесты",
      link: "/tools/how-to-test-and-why/",
      category: "tools",
    };

    const json = jest.fn(() => Promise.resolve(expectedResult));
    global.fetch = jest.fn(() => Promise.resolve({ json }));

    const searchResult = await searchClient.search("test");
    expect(searchResult).toEqual(expectedResult);
    expect(global.fetch).toHaveBeenCalledTimes(1);
    expect(global.fetch.mock.calls[0][0]).toContain("search=test");
  });
});
```

A stub that resolves `42` fails with `.json is not a function`. A stub that always returns the same JSON only proves “given this fetch, search returns that value” — assert the URL/args too. Docs shorthand: `jest.fn().mockResolvedValue({ json: () => expected })`.

If `document` is undefined on import, set jsdom (config or docblock). Mock heavy `header` EventEmitter instead of running 250 lines of real header code.

```js
/**
 * @jest-environment jsdom
 */

import { init } from "../article-aside";

jest.mock("../header", () => {
  const fixed = [];
  return {
    on: (eventName, callback) => {
      if (eventName === "fixed") fixed.push(callback);
    },
    callFixed: () => {
      fixed.forEach((callback) => callback());
    },
  };
});

import { callFixed } from "../header";

describe("articleAside", () => {
  it("adds offset class when header is fixed", () => {
    const el = document.createElement("div");
    el.className = "article__aside";
    document.body.appendChild(el);
    init();
    expect(el.classList.contains("article__aside--offset")).toBe(false);
    callFixed();
    expect(el.classList.contains("article__aside--offset")).toBe(true);
  });
});
```

`expect(init).toBeDefined()` is a smoke test — it does not prove behavior.

**Thoughts:** Start with one test next to the code; Node for logic, jsdom + mocks for DOM/network — tests fail when the contract changes, not when the internet is down.

**Sources:**

[Jest configuration](https://jestjs.io/docs/configuration) · [Test environments](https://jestjs.io/docs/configuration#testenvironment-string) · [Mock functions](https://jestjs.io/docs/mock-functions) · [CLI `--watch`](https://jestjs.io/docs/cli#--watch) · [Doka — first tests](https://doka.guide/tools/how-to-test/)

---

</details>
