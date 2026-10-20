# React — тестирование

Jest, React Testing Library, пирамида тестов, интеграционные тесты и E2E (Playwright / Cypress).

[← Назад к хабу](./4_react.md)

### Related docs

[Testing hub](./7_special_testing.md) · [Fundamentals — pyramid](./7_special_testing_fundamentals.md#test-pyramid) · [Tooling — Jest & RTL](./7_special_testing_tooling.md) · [Layers — E2E](./7_special_testing_layers.md#e2e-testing) · [MSW](./7_special_testing_tooling.md#msw-mock-service-worker) · [Build — CI](./10_build_ci.md)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Testing pyramid (React)](#testing-pyramid-react) | Баланс unit → integration → E2E |
| [Jest setup](#jest-setup) | jsdom, моки, matchers |
| [React Testing Library](#react-testing-library) | Компонентные тесты «как пользователь» |
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

**Definition:** **Test pyramid** рекомендует много **быстрых узких** тестов в основании и меньше **медленных широких** наверху — для React: unit/логика → интеграция компонентов (RTL) → E2E (реальный браузер).

**How it works:**

| Layer | Tooling | What you prove |
| ----- | ------- | -------------- |
| **Unit** | Jest / Vitest | Чистые функции, хуки (с `renderHook` из `@testing-library/react`), reducers |
| **Integration** | RTL + jsdom + MSW | Страница/фича ведёт себя для пользователя; API замокан на границе сети |
| **E2E** | Playwright / Cypress | Полное приложение + реальный backend или staging; только критичные пути |

**When to use:** В вакансиях часто требуют **Jest + RTL** на интеграционном слое; **Playwright** — для checkout/login/smoke, а не тысячи E2E-спеков.

**Pluses and minuses:**

| + | − |
| - | - |
| RTL ловит реальные регрессии UI | E2E нестабильны при перегрузе |
| Пирамида держит CI быстрым | Неверный слой → медленные или хрупкие сьюты |

**Thoughts:** Каноническая статья о пирамиде → [Test pyramid](./7_special_testing_fundamentals.md#test-pyramid). Vitest совместим с Jest — [Vitest vs Jest](./7_special_testing_tooling.md#vitest-vs-jest).

**Sources:** [Testing Library — guiding principles](https://testing-library.com/docs/guiding-principles)

</details>

---

## Jest setup

<details>
<summary>Jest setup</summary>

**Definition:** **Jest** — test runner (assertions, mocks, coverage), часто с **jsdom** для имитации браузера в React-тестах.

**How it works:**

| Piece | Role |
| ----- | ---- |
| `testEnvironment: 'jsdom'` | DOM для RTL |
| `@testing-library/jest-dom` | Matchers вроде `toBeInTheDocument()` |
| `moduleNameMapper` | Заглушки CSS/картинок, алиас `@/` |
| `setupFilesAfterEnv` | Глобальный cleanup RTL / import jest-dom |
| `transform` | `ts-jest` или `@swc/jest` для TS/TSX |

**When to use:** Стандарт команды или legacy CRA; greenfield на Vite часто берут **Vitest** с тем же API.

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

**Definition:** **React Testing Library (RTL)** рендерит компоненты и ищет в DOM **как пользователь** — roles, labels, text — а не детали реализации.

**How it works:**

| Practice | Why |
| -------- | --- |
| `getByRole('button', { name: /save/i })` | Доступные запросы переживают рефакторинг |
| `userEvent.click()` | Реалистичное взаимодействие (`@testing-library/user-event`) |
| Не тестировать state/props напрямую | Тест поведения, не внутренностей React |
| `waitFor` / `findBy*` | Асинхронный UI после fetch или transition |

**When to use:** **Обязательный** навык для большинства frontend-ролей — дефолт для component и feature тестов.

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

**Thoughts:** Не shallow-render детей — мокайте **сеть** через [MSW](#integration-tests), а не каждый дочерний компонент.

**Sources:** [RTL — intro](https://testing-library.com/docs/react-testing-library/intro/) · [Testing — RTL](./7_special_testing_tooling.md#react-testing-library)

</details>

---

## Integration tests

<details>
<summary>Integration tests</summary>

**Definition:** **Integration tests** в React объединяют **RTL**, **routing**, **providers** (QueryClient, Redux) и **HTTP mocks**, чтобы прогнать срез фичи без реального backend.

**How it works:**

1. Обёртка дерева в `MemoryRouter`, `QueryClientProvider` и т.д.
2. **MSW** перехватывает `fetch`/axios — отдаёт JSON-фикстуры
3. Проверка видимых исходов (строки таблицы, alert об ошибке, empty state)
4. Опционально: `renderHook` для data hooks с wrapper

**When to use:** Завершённые фичи (login, list + filter, валидация формы) — основной ROI после unit-тестов.

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

**Definition:** **End-to-end** тесты управляют **реальным браузером**, ходят в задеплоенное или preview-приложение и проверяют пользовательские исходы через весь стек.

**How it works:**

| Tool | Strengths |
| ---- | --------- |
| **Playwright** | Multi-browser, auto-wait, trace viewer, параллель в CI |
| **Cypress** | Удобный runner, time-travel debug |
| Both | Не замена RTL — вершина [pyramid](#testing-pyramid-react) |

**When to use:** Smoke на PR preview (login, create item, paywall); 5–20 критичных путей, не каждый edge case.

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
| Ловит wiring, env, CORS | Медленнее; нужны стабильные тестовые данные |
| Playwright traces для flakes | Перегруз E2E переворачивает пирамиду |

**Thoughts:** E2E против **staging** или ephemeral preview в CI → [Build — CI](./10_build_ci.md).

**Sources:** [Playwright — writing tests](https://playwright.dev/docs/writing-tests) · [E2E testing](./7_special_testing_layers.md#e2e-testing)

</details>
