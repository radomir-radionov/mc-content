# React — Testing

Jest, React Testing Library, test pyramid, integration tests, and E2E (Playwright / Cypress).

[← Back to hub](./4_react.md)

### Related docs

[Testing hub](./7_special_testing.md) · [Fundamentals — pyramid](./7_special_testing_fundamentals.md#test-pyramid) · [Tooling — Jest & RTL](./7_special_testing_tooling.md) · [Layers — E2E](./7_special_testing_layers.md#e2e-testing) · [MSW](./7_special_testing_tooling.md#msw-mock-service-worker) · [Build — CI](./10_build_ci.md)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Testing pyramid (React)](#testing-pyramid-react) | Unit → integration → E2E balance |
| [Jest setup](#jest-setup) | jsdom, mocks, matchers |
| [React Testing Library](#react-testing-library) | User-centric component tests |
| [Integration tests](#integration-tests) | RTL + MSW + router |
| [E2E tests](#e2e-tests) | Playwright vs Cypress |

### Sections

- [Testing pyramid (React)](#testing-pyramid-react)
- [Jest setup](#jest-setup)
- [React Testing Library](#react-testing-library)
- [Integration tests](#integration-tests)
- [E2E tests](#e2e-tests)

---

## Testing pyramid (React)

<details>
<summary>Testing pyramid (React)</summary>

**Definition:** The **test pyramid** recommends many **fast, narrow** tests at the base and fewer **slow, wide** tests at the top — for React: unit/logic → component integration (RTL) → E2E (real browser).

**How it works:**

| Layer | Tooling | What you prove |
| ----- | ------- | -------------- |
| **Unit** | Jest / Vitest | Pure functions, hooks (with `@testing-library/react` `renderHook`), reducers |
| **Integration** | RTL + jsdom + MSW | Page/feature behaves for user; API mocked at network boundary |
| **E2E** | Playwright / Cypress | Full app + real backend or staging; critical paths only |

**When to use:** Job requirements often mandate **Jest + RTL** for integration-layer tests; add **Playwright** for checkout/login/smoke flows — not thousands of E2E specs.

**Pluses and minuses:**

| + | − |
| - | - |
| RTL catches real UI regressions | E2E flakiness if overused |
| Pyramid keeps CI fast | Wrong layer → slow or brittle suites |

**Thoughts:** Canonical pyramid essay → [Test pyramid](./7_special_testing_fundamentals.md#test-pyramid). Vitest is Jest-compatible — [Vitest vs Jest](./7_special_testing_tooling.md#vitest-vs-jest).

**Sources:** [Testing Library — guiding principles](https://testing-library.com/docs/guiding-principles)

</details>

---

## Jest setup

<details>
<summary>Jest setup</summary>

**Definition:** **Jest** is a test runner (assertions, mocks, coverage) commonly paired with **jsdom** to simulate a browser for React tests.

**How it works:**

| Piece | Role |
| ----- | ---- |
| `testEnvironment: 'jsdom'` | DOM for RTL |
| `@testing-library/jest-dom` | Matchers like `toBeInTheDocument()` |
| `moduleNameMapper` | CSS/images stubs, `@/` alias |
| `setupFilesAfterEnv` | Global RTL cleanup / jest-dom import |
| `transform` | `ts-jest` or `@swc/jest` for TS/TSX |

**When to use:** Team standard or legacy CRA; greenfield Vite repos often use **Vitest** with the same API.

**Examples:**

```ts
// jest.config.ts
import type { Config } from "jest";

const config: Config = {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: ["<rootDir>/src/test/setup.ts"],
  moduleNameMapper: {
    "^@/(.*)$": "<rootDir>/src/$1",
    "\\.(css|less)$": "identity-obj-proxy",
  },
};

export default config;
```

```ts
// src/test/setup.ts
import "@testing-library/jest-dom/vitest";
import { cleanup } from "@testing-library/react";
import { afterEach } from "@jest/globals";

afterEach(() => cleanup());
```

**Sources:** [Jest — getting started](https://jestjs.io/docs/getting-started) · [Testing — Jest first tests](./7_special_testing_tooling.md#jest--first-tests-node-vs-jsdom)

</details>

---

## React Testing Library

<details>
<summary>React Testing Library</summary>

**Definition:** **React Testing Library (RTL)** renders components and queries the DOM **as users see it** — roles, labels, text — not implementation details.

**How it works:**

| Practice | Why |
| -------- | --- |
| `getByRole('button', { name: /save/i })` | Accessible queries survive refactors |
| `userEvent.click()` | Realistic interaction (use `@testing-library/user-event`) |
| Avoid testing state/props directly | Tests behavior, not React internals |
| `waitFor` / `findBy*` | Async UI after fetch or transition |

**When to use:** **Required** skill for most frontend roles — default for component and feature tests.

**Examples:**

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { LoginForm } from "./LoginForm";

test("submits email and password", async () => {
  const user = userEvent.setup();
  const onSubmit = jest.fn();
  render(<LoginForm onSubmit={onSubmit} />);

  await user.type(screen.getByLabelText(/email/i), "a@test.com");
  await user.type(screen.getByLabelText(/password/i), "secret");
  await user.click(screen.getByRole("button", { name: /sign in/i }));

  expect(onSubmit).toHaveBeenCalledWith({
    email: "a@test.com",
    password: "secret",
  });
});
```

**Thoughts:** Do not shallow-render children — mock **network** with [MSW](#integration-tests), not every child component.

**Sources:** [RTL — intro](https://testing-library.com/docs/react-testing-library/intro/) · [Testing — RTL](./7_special_testing_tooling.md#react-testing-library)

</details>

---

## Integration tests

<details>
<summary>Integration tests</summary>

**Definition:** **Integration tests** in React combine **RTL**, **routing**, **providers** (QueryClient, Redux), and **HTTP mocks** to exercise a feature slice without a real backend.

**How it works:**

1. Wrap tree in `MemoryRouter`, `QueryClientProvider`, etc.
2. **MSW** intercepts `fetch`/axios — return JSON fixtures
3. Assert visible outcomes (table rows, error alert, empty state)
4. Optional: `renderHook` for data hooks with wrapper

**When to use:** Feature-complete flows (login, list + filter, form validation) — main ROI layer after unit tests.

**Examples:**

```tsx
import { render, screen, waitFor } from "@testing-library/react";
import { rest } from "msw";
import { setupServer } from "msw/node";
import { UsersPage } from "./UsersPage";
import { AppProviders } from "@/app/providers";

const server = setupServer(
  rest.get("/api/users", (_req, res, ctx) =>
    res(ctx.json([{ id: "1", name: "Ada" }])),
  ),
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

test("shows users from API", async () => {
  render(
    <AppProviders>
      <UsersPage />
    </AppProviders>,
  );
  await waitFor(() =>
    expect(screen.getByText("Ada")).toBeInTheDocument(),
  );
});
```

**See also:** [Integration testing](./7_special_testing_layers.md#integration-testing) · [REST client errors](./4_react_api_client.md#error-handling)

**Sources:** [MSW — docs](https://mswjs.io/docs/) · [Testing — MSW](./7_special_testing_tooling.md#msw-mock-service-worker)

</details>

---

## E2E tests

<details>
<summary>E2E tests</summary>

**Definition:** **End-to-end** tests drive a **real browser**, hit the deployed or preview app, and assert user-visible outcomes across the full stack.

**How it works:**

| Tool | Strengths |
| ---- | --------- |
| **Playwright** | Multi-browser, auto-wait, trace viewer, parallel CI |
| **Cypress** | Developer-friendly runner, time-travel debug |
| Both | Not a replacement for RTL — top of [pyramid](#testing-pyramid-react) |

**When to use:** Smoke tests on PR preview (login, create item, paywall); 5–20 critical paths, not every edge case.

**Examples:**

```ts
// playwright — tests/checkout.spec.ts
import { test, expect } from "@playwright/test";

test("guest can open catalog", async ({ page }) => {
  await page.goto("/");
  await expect(page.getByRole("heading", { name: /catalog/i })).toBeVisible();
  await page.getByRole("link", { name: /details/i }).first().click();
  await expect(page).toHaveURL(/\/products\//);
});
```

**Pluses and minuses:**

| + | − |
| - | - |
| Catches wiring, env, CORS issues | Slower; needs stable test data |
| Playwright traces debug flakes | Overuse inverts pyramid |

**Thoughts:** Run E2E against **staging** or ephemeral preview URLs in CI → [Build — CI](./10_build_ci.md).

**Sources:** [Playwright — writing tests](https://playwright.dev/docs/writing-tests) · [E2E testing](./7_special_testing_layers.md#e2e-testing)

</details>
