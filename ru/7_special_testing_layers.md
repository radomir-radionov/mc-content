# Testing — Layers

Integration, system, E2E, visual, performance, and security testing.

[← Back to hub](./7_special_testing.md)

### Related docs

- [Fundamentals](./7_special_testing_fundamentals.md) · [Layers](./7_special_testing_layers.md) · [Practices](./7_special_testing_practices.md) · [Tooling](./7_special_testing_tooling.md) · [React — Testing](./4_react_ecosystem.md#testing) · [Next.js — Vitest + RTL](./4_nextjs_platform.md#testing-nextjs-vitest--rtl) · [CI/CD](./10_build_ci.md#continuous-integration-ci) · [Interview leftovers](./7_special_interview.md)

### Sections

- [Test layers](#test-layers)
- [Integration testing](#integration-testing)
- [System testing](#system-testing)
- [E2E testing](#e2e-testing)
- [Screenshot testing](#screenshot-testing)
- [Performance testing](#performance-testing)
- [Security testing](#security-testing)

---

<a name="test-layers"></a>

## Test layers

<details>
  <summary>Integration testing</summary>

### Integration testing

**Definition:** Tests that several modules work **together** (API + controller + DB, form + store + mocked HTTP) without booting the whole product as a user would.

**How it works:**

Unit tests isolate one unit (often with doubles). Integration tests keep real seams between a **group** of units. You still stub far edges (payment vendor, email, another team's service) so the suite stays deterministic.

| Approach        | How                                                     | When                                                    |
| --------------- | ------------------------------------------------------- | ------------------------------------------------------- |
| **Big bang**    | Wire all modules, then run scenarios                    | Small systems; fast to write, slow to localize failures |
| **Incremental** | Integrate closest modules first (bottom-up or top-down) | Default — a failing test points at a smaller group      |

a **stub** is called _by_ the group (fake neighbor on the “output” side). A **driver** _calls_ the group (test harness, HTTP client, RTL `userEvent` on the “input” side). Same idea as Jest [stubs/mocks](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies), but named for assembling clusters, not a single function.

Prioritize **critical features** (checkout, auth, money, permissions). Fixtures: only fields the scenario needs. Automate in CI next to units; keep the real browser for [E2E](#e2e-testing).

[Testing Library](https://testing-library.com/) — user-visible queries; unit _and_ integration; no shallow render. **Enzyme** is historical (shallow render, last aimed at React 16) — do not start new suites on it. Runners: Jest / Vitest. Network: MSW. Backend: test DB + HTTP against the app.

**When to use:**

Feature slices units cannot see — registration hitting routes + DB; a React page with MSW; OpenAPI contract vs a real handler.

#### Common pitfalls

Calling a jsdom RTL test “E2E”. Hitting production APIs. Huge JSON fixtures.

**Pluses and minuses:**

| +                                   | −                                      |
| ----------------------------------- | -------------------------------------- |
| Catches wiring bugs units miss      | Slower than units; needs fixtures      |
| Still cheaper/stabler than full E2E | Too many real deps → flaky “mini-E2E”  |
| Good CI default for critical paths  | Big-bang suites hide the broken module |

**Examples:**

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { LoginForm } from "./LoginForm";

test("submit shows welcome after a successful sign-in", async () => {
  const user = userEvent.setup();
  render(<LoginForm />);
  await user.type(screen.getByLabelText(/email/i), "ada@example.com");
  await user.click(screen.getByRole("button", { name: /sign in/i }));
  expect(await screen.findByText(/welcome/i)).toBeInTheDocument();
});
```

RTL does not shallow-render: mock I/O at the boundary (MSW/`fetch`), not child components, so the test still resembles the user ([Testing Library FAQ](https://testing-library.com/docs/react-testing-library/faq)).

**Thoughts:** Units prove pieces; integration proves the **seams** — still not the full product.

**Sources:**

[Testing Library guiding principles](https://testing-library.com/docs/guiding-principles) · [Doka — integration & system tests](https://doka.guide/tools/integration-and-system-testing/) · [React Testing](./4_react_ecosystem.md#testing)

---

</details>

<details>
  <summary>System testing</summary>

### System testing

**Definition:** A **class** of tests that exercise the **whole program** (or a production-like build) — not a subset of modules.

**How it works:**

After units + integration, remaining risk is “does the assembled system behave for a user / under load / under attack?” Split by **purpose**:

| Kind                                           | Question                                             |
| ---------------------------------------------- | ---------------------------------------------------- |
| **[E2E](#e2e-testing)**                        | Can a user complete a flow in a real browser?        |
| **[Screenshot / visual](#screenshot-testing)** | Did the UI pixels regress?                           |
| **[Performance](#performance-testing)**        | Did we get slower (Web Vitals / lab budget)?         |
| **[Security tests](#security-testing)**        | Can we inject, steal cookies, or call APIs unsafely? |

Automate and run in CI — usually a **separate job** (slow, browsers, secrets). See [CI](./10_build_ci.md#continuous-integration-ci).

**When to use:** Release confidence; critical journeys; UI/perf budgets; compliance.

**Pluses and minuses:**

| +                                     | −                                                                               |
| ------------------------------------- | ------------------------------------------------------------------------------- |
| Highest confidence the product works  | Slow, flaky, expensive to maintain                                              |
| Finds env/config bugs units never see | Overuse inverts the [pyramid](./7_special_testing_fundamentals.md#test-pyramid) |

**Thoughts:** System tests answer “the app as a whole” — pick the subtype that matches the risk (flow, look, speed, attack).

**Sources:**

[Doka — integration & system testing](https://doka.guide/tools/integration-and-system-testing/)

---

</details>

<details>
  <summary>E2E testing</summary>

### E2E testing

**Definition:** End-to-end tests drive the app **as a user** (open page → click → see text) through a real browser, so the stack is exercised together.

**How it works:**

Script user actions and assert observable outcomes (URL, heading, toast). Playwright gives each test an isolated **browser context** (fresh profile) and auto-waits for actionability — avoid `sleep()`.

Playwright, Cypress, Puppeteer, Selenium, WebdriverIO, Nightwatch. Playwright/Cypress are the usual frontend default. Selenium/WebDriver is the older protocol; WebdriverIO is a runner that can sit on WebDriver _or_ Playwright. RTL in jsdom is **not** E2E.

**When to use:**

Auth, checkout, navigation + Server Actions, anything that must work when units _and_ integration already passed but the assembled app still failed.

#### Common pitfalls

Testing every unit path in E2E (ice-cream cone). Shared mutable test users. Asserting implementation instead of roles/text.

**Pluses and minuses:**

| +                             | −                                            |
| ----------------------------- | -------------------------------------------- |
| Closest to real users         | Slow; flake from timing, data, third parties |
| Catches CSS/JS/backend wiring | Few tests — only critical paths              |

**Examples:**

```ts
import { test, expect } from "@playwright/test";

test("get started link", async ({ page }) => {
  await page.goto("https://playwright.dev/");
  await page.getByRole("link", { name: "Get started" }).click();
  await expect(
    page.getByRole("heading", { name: "Installation" }),
  ).toBeVisible();
});
```

**Thoughts:** Few, slow, valuable — describe **user** steps, not internals.

**Sources:**

[Playwright — writing tests](https://playwright.dev/docs/writing-tests) · [Next.js testing](./4_nextjs_platform.md#testing-nextjs-vitest--rtl)

---

</details>

<details>
  <summary>Screenshot testing</summary>

### Screenshot testing

**Definition:** Visual regression — capture a **baseline** screenshot and fail CI when later pixels differ beyond a tolerance.

**How it works:**

First run stores the gold image. Later runs compare. Playwright waits until two consecutive screenshots match (animations settle), then diffs against the baseline. Mask dynamic regions (ads, dates, avatars).

Playwright `expect(page).toHaveScreenshot()`, Puppeteer + a diff lib, Chromatic/Percy. Jest/RTL **HTML snapshots** (`toMatchSnapshot`) are markup, not pixels.

**When to use:** Design-system pages, marketing layouts, “did this CSS change the dashboard?” — not business rules (assert those in E2E with text/roles).

**Pluses and minuses:**

| +                            | −                                        |
| ---------------------------- | ---------------------------------------- |
| Catches CSS/layout surprises | Fonts, OS, DPI make baselines brittle    |
| Cheap once stable            | Noise from animations, clocks, A/B flags |

**Examples:**

```ts
import { test, expect } from "@playwright/test";

test("docs home visual", async ({ page }) => {
  await page.goto("https://playwright.dev");
  await expect(page).toHaveScreenshot({ maxDiffPixels: 100 });
});
```

Run baselines in the **same environment** as CI (often Docker), or diffs are meaningless.

**Thoughts:** Pixel budget on UI — update the baseline only when the change is **intended**.

**Sources:**

[Playwright — visual comparisons](https://playwright.dev/docs/test-snapshots)

---

</details>

<details>
  <summary>Performance testing</summary>

### Performance testing

**Definition:** Check that the system did **not get slower** after a change — compare metrics to a baseline (lab budget and/or field Web Vitals).

**How it works:**

Frontend usually tracks **Core Web Vitals**: LCP (load), INP (responsiveness), CLS (stability). **Lighthouse** is lab (synthetic): it scores FCP, LCP, TBT, CLS, Speed Index. Default Lighthouse has **no user**, so it reports **TBT** as a proxy — not field **INP**. Ship gates on a budget; confirm with RUM / `web-vitals`. Micro-benchmarks use `performance.now()` / User Timing — **jsPerf.com is gone**.

Lighthouse (DevTools / CLI / [LHCI](https://github.com/GoogleChrome/lighthouse-ci)), Chrome Performance panel, `performance` API, PageSpeed Insights, WebPageTest. Theory: [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals) · panel: [11_vsc_browser.md](./11_vsc_browser.md#devtools--core-web-vitals).

**When to use:** Perf SLO (“LCP p75 < 2.5s”), CI Lighthouse CI, catching a huge bundle or layout shift before release.

**Pluses and minuses:**

| +                            | −                                             |
| ---------------------------- | --------------------------------------------- |
| Numeric regression signal    | Lab ≠ field (CrUX / Search Console)           |
| Lighthouse CI is automatable | One-off `performance.now()` in tests is noisy |

**Examples:**

```js
performance.mark("search-start");
await search(query);
performance.mark("search-end");
performance.measure("search", "search-start", "search-end");
const { duration } = performance.getEntriesByName("search")[0];
expect(duration).toBeLessThan(200); // lab-only; flake-prone — prefer Lighthouse budgets
```

**Thoughts:** Baseline → change → compare Vitals/Lighthouse — do not trust a green lab score against red field data.

**Sources:**

[Lighthouse](https://github.com/GoogleChrome/lighthouse) · [web.dev / CWV](https://web.dev/articles/vitals)

---

</details>

<details>
  <summary>Security testing</summary>

### Security testing

**Definition:** Checks that the **assembled** app does not leak or execute attacker-controlled data — mostly backend, but frontend still owns output encoding, cookies, and API calls.

**How it works:** Threats live in [web attacks](./7_special_security.md#14-common-web-attacks). Frontend-focused tests often assert: user input is escaped (no `innerHTML` XSS), session cookies are `HttpOnly`/`Secure`/`SameSite`, privileged APIs reject unauthenticated calls, CSP is present.

**When to use:** Auth cookies, rich-text/markdown renderers, `postMessage`, file uploads, CORS on the API you own.

**Pluses and minuses:**

| +                                       | −                                   |
| --------------------------------------- | ----------------------------------- |
| Catches XSS/CSRF-class bugs before prod | Scanners miss logic bugs (IDOR)     |
| Cookie/header checks are cheap in E2E   | Not a substitute for review + OWASP |

**Examples:**

```ts
const cookies = await context.cookies();
const session = cookies.find((c) => c.name === "session");
expect(session?.httpOnly).toBe(true);
expect(session?.secure).toBe(true);
expect(session?.sameSite).toMatch(/Lax|Strict/);
```

**Thoughts:** Frontend security tests = escape input, lock cookies, never trust the client for authz.

**See also:** [XSS](./7_special_security.md#xss-cross-site-scripting) · [CSRF](./7_special_security.md#csrf-cross-site-request-forgery)

**Sources:**

[OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/) · [Doka — integration & system testing](https://doka.guide/tools/integration-and-system-testing/)

---

</details>
