# Testing — Fundamentals

AAA, the test pyramid, testing approaches, and common test purposes.

[← Back to hub](./7_special_testing.md)

### Related docs

- [Fundamentals](./7_special_testing_fundamentals.md) · [Layers](./7_special_testing_layers.md) · [Practices](./7_special_testing_practices.md) · [Tooling](./7_special_testing_tooling.md) · [React — Testing](./4_react_testing.md) · [Next.js — Vitest + RTL](./4_nextjs_platform.md#testing-nextjs-vitest--rtl) · [CI/CD](./10_build_ci.md#continuous-integration-ci) · [Interview leftovers](./7_special_interview.md)

### Sections

- [Fundamentals](#fundamentals)
- [Tests and AAA](#tests-and-aaa)
- [Test pyramid](#test-pyramid)
- [Testing approaches (black-box, white-box, gray-box)](#testing-approaches-black-box-white-box-gray-box)
- [Tests by purpose (smoke, sanity, regression)](#tests-by-purpose-smoke-sanity-regression)

---

<a name="fundamentals"></a>

## Fundamentals

<details>
  <summary>Tests and AAA</summary>

### Tests and AAA

**Definition:** A test is code that checks assumptions about other code. Tests make new code cleaner (you design the public API and edge cases first) and make old code safer to change (failures catch regressions before production).

**How it works:**

- Primitive check: call the unit, compare with an expected value (`console.assert`). That lacks a description, readable matchers, and auto-rerun on change — runners exist for that.
- Frontend runners: **Jest** (common default), **Vitest** (Jest-like API, Vite-native). Pick the runner for the layer: unit → Jest/Vitest; components / integration → RTL; E2E → Playwright / Cypress / WebdriverIO. Vue: [Vue Test Utils](./4_vue_ecosystem.md#testing) · Nuxt: [@nuxt/test-utils](./4_nuxt_platform.md#nuxt-test-utils). Setup: [Jest — first tests](./7_special_testing_tooling.md#jest--first-tests-node-vs-jsdom). Layers: [Integration](./7_special_testing_layers.md#integration-testing) · [E2E](./7_special_testing_layers.md#e2e-testing).
- Anatomy is **AAA** (Arrange / Act / Assert) — Russian mnemonic **ПВП**: подготовка, выполнение, проверка.
  - **Arrange** — inputs, expected value, fakes/doubles.
  - **Act** — run the scenario (one call or user flow).
  - **Assert** — compare actual vs expected. Prefer **one assertion / one assumption** per test; extra cases → extra tests.
- Ideal test: all three stages (often three lines), independent of other tests (same result anywhere), readable as an English sentence via `describe` + `it`.
- Jest: `describe(name, fn)` groups a suite; `test` / `it(name, fn)` is one case; `expect(received).toEqual(expected)` is deep equality (`Object.is` for primitives). `toBe` is reference / exact primitive identity — two equal objects fail `toBe`.

**When to use:**

Default for production code. Skip or defer for throwaway prototypes and projects that will not live long enough for tests to pay back. Treat tests as planned work, not leftover time. Make the code cheap to test: [Making tests easy](./7_special_testing_practices.md#making-tests-easy).

#### Common pitfalls

- Coupled tests — shared mutable state so order changes the result.
- Many assertions in one `it` — a failure hides which assumption broke.
- Skipping tests because “extra work” — cost is front-loaded; the suite pays back on every refactor and upgrade.
- Testing only the happy path — the point of writing tests is to hunt “what can go wrong”.

**Pluses and minuses:**

| +                                                         | −                                                                                                                         |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Forces edge cases, not only the happy path                | More time on a young project                                                                                              |
| Fewer regressions when adding features or sharing helpers | Need structure for fixtures and doubles ([Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies)) |
| Confidence to refactor (including `==` vs `=` bugs)       | CI must run the suite and block bad merges                                                                                |
| Catch breaking dependency upgrades                        | Designing fakes is an architecture skill                                                                                  |
| Living docs that cannot silently rot (unlike comments)    |                                                                                                                           |

**Examples:**

```js
function add(a, b) {
  return a + b;
}

// Primitive — no name, no watch, manual throw
function testAdd() {
  const result = add(10, 5);
  const expected = 15;
  console.assert(result === expected, `got ${result}, expected ${expected}`);
}

// AAA — Arrange args/expected, Act call, Assert matcher
describe("When given 2 numbers", () => {
  it("returns the sum of those 2 numbers", () => {
    const expected = 15;
    const result = add(10, 5);
    expect(result).toEqual(expected);
  });
});
```

Jest install: `npm install --save-dev jest`, `"test": "jest"` in `package.json`, then `npm test`. Official first test uses `toBe` for primitives (`expect(sum(1, 2)).toBe(3)`).

**Thoughts:** Tests are a development tool and a contract that stays true; write them from day one unless the code is a short-lived prototype.

**See also:** When writing tests still feels painful — [Making tests easy](./7_special_testing_practices.md#making-tests-easy). Green units but a dead app — [Integration](./7_special_testing_layers.md#integration-testing) · [System](./7_special_testing_layers.md#system-testing).

**Sources:**

[Doka — how and why to write tests](https://doka.guide/tools/how-to-test-and-why/) · [Jest Getting Started](https://jestjs.io/docs/getting-started) · [Jest `describe` / `test`](https://jestjs.io/docs/api) · [`.toEqual`](https://jestjs.io/docs/expect#toequalvalue)

---

</details>

<details>
  <summary>Test pyramid</summary>

### Test pyramid

**Definition:**

Ideal test distribution — many fast unit tests at the base, fewer integration tests, fewest slow E2E/UI tests at the top. Why write any of them: [Why write tests (AAA)](#tests-and-aaa).

```mermaid
flowchart TB
  e2e[E2E / UI — few, slow, expensive]
  integration[Integration — moderate]
  unit[Unit — many, fast, cheap]
  unit --> integration --> e2e
```

| Layer            | Scope                                                               | Speed               | Cost   |
| ---------------- | ------------------------------------------------------------------- | ------------------- | ------ |
| **Unit**         | One function/module; other deps replaced with doubles               | Fast, parallel-safe | Low    |
| **Integration**  | Two+ parts of one job — **result** of collaboration, not call order | Medium              | Medium |
| **E2E** (system) | App as a user: click, fill, assert                                  | Slow                | High   |
| **Acceptance**   | Spec / TZ scenarios before ship; often **manual**                   | Slowest             | High   |

**How it works:**

- **Unit** — most of the suite. Isolation: if the module imports others, use fakes. TDD sits here: [TDD](./7_special_testing_practices.md#tdd-test-driven-development).
- **Integration** — slightly richer harness so several units run together. Typical target: middleware — unit tests check the middleware’s job in isolation; integration checks how it talks to real neighbors.
- **E2E** — a robot that uses the product (e.g. full signup). Tools: Playwright (preferred in this conspect), Cypress, WebdriverIO.
- **Acceptance** — rarer on the web (short delivery cycle → automate instead). Common before desktop/mobile releases with a long store cycle.

**When to use:** Default strategy. Invert pyramid (ice cream cone) = pain.

**Pluses and minuses:**

| + (pyramid shape)         | −                                              |
| ------------------------- | ---------------------------------------------- |
| Fast CI feedback          | E2E-only gives slow, flaky suite               |
| Cheap maintenance at base | Under-testing integration misses contract bugs |

**Thoughts:**

Contract tests between services sit between integration and E2E. Snapshot tests are unit-ish but can be brittle. Unit tests belong on **your** code; libraries get coverage via adapters + this pyramid’s integration/E2E layers — [Making tests easy](./7_special_testing_practices.md#making-tests-easy). Depth: [Integration](./7_special_testing_layers.md#integration-testing) · [System](./7_special_testing_layers.md#system-testing) · [E2E](./7_special_testing_layers.md#e2e-testing).

---

</details>

<a name="testing-approaches-black-box-white-box-gray-box"></a>

<details>
  <summary>Testing approaches (black-box, white-box, gray-box)</summary>

### Testing approaches (black-box, white-box, gray-box)

**Definition:** **Approaches** (sometimes called _methods_ in QA courses — «методы тестирования») describe **how much you know about the implementation** when you choose inputs and oracles. They are orthogonal to [test layers](./7_special_testing_fundamentals.md#test-pyramid) (unit vs E2E) and to [automation](./7_special_testing_tooling.md).

**How it works:**

| Approach       | Tester knows                         | Typical focus                                      | Common in dev work                                      |
| -------------- | ------------------------------------ | -------------------------------------------------- | ------------------------------------------------------- |
| **Black-box**  | Specs / API / UI only                | Inputs → observable outputs; requirements coverage | RTL by role/label; API contract tests; manual UAT       |
| **White-box**  | Code structure, branches, data flow  | Paths, conditions, error handling                  | Unit tests with mocks; coverage tools; branch-aware cases |
| **Gray-box**   | Partial internals (schema, logs, DB) | Integration seams with limited visibility          | MSW + real reducer; test DB with known seed; API + OpenAPI |

**Static vs dynamic** (another QA axis):

- **Static** — analyze artifacts **without running** the program: lint, typecheck, code review, SAST, dependency audit. Cheap in CI; catches many classes of bugs early.
- **Dynamic** — **execute** the software and observe behavior: unit/integration/E2E, profilers, DAST. Required to prove runtime behavior.

**Manual vs automated:** Manual exploration finds surprises; automated suites give repeatable [regression](#tests-by-purpose-smoke-sanity-regression) signal. Both stay in mature teams — automation does not remove [exploratory](./7_special_testing_interview.md) sessions.

**When to use:**

- **Black-box** — acceptance criteria, user stories, public APIs, “does it match the TZ?”
- **White-box** — algorithms, parsers, security-sensitive branches, refactors where coverage guides missing cases.
- **Gray-box** — full-stack features where you know the DB shape or wire format but still test through the UI/API.

#### Common pitfalls

- Calling every Jest test “white-box” while ignoring requirements — combine with black-box cases from specs.
- 100% line coverage as a goal — coverage is a white-box **hint**, not proof of correct behavior.
- Skipping static checks and relying only on E2E — slow feedback and shallow defect detection.

**Pluses and minuses:**

| +                                      | −                                           |
| -------------------------------------- | ------------------------------------------- |
| Shared vocabulary with QA / university | Easy to confuse with pyramid “levels”     |
| Picks the right technique per risk     | Gray-box needs discipline (no over-mocking) |

**Thoughts:** In practice you mix approaches: white-box unit tests under a black-box E2E journey. Design black-box cases systematically — [Black-box test design techniques](./7_special_testing_practices.md#black-box-test-design-techniques).

**Sources:**

[Doka — integration & system testing](https://doka.guide/tools/integration-and-system-testing/) · [ISTQB Glossary](https://glossary.istqb.org/)

---

</details>

<a name="tests-by-purpose-smoke-sanity-regression"></a>

<details>
  <summary>Tests by purpose (smoke, sanity, regression)</summary>

### Tests by purpose (smoke, sanity, regression)

**Definition:** **Purpose** labels describe **why** you run a set of tests now — not which layer (unit/E2E) they use. The same Playwright spec can be smoke on every commit and part of a full regression before release.

**How it works:**

| Purpose        | Question answered                                      | Scope / timing                                      |
| -------------- | ------------------------------------------------------ | --------------------------------------------------- |
| **Smoke**      | “Is the build testable at all?”                        | Tiny critical path after deploy/build; fail fast     |
| **Sanity**     | “Did this change break the area we touched?”           | Narrow checks around a fix or feature slice         |
| **Regression** | “Did we break something that used to work?”           | Re-run known-good suite (often all automated tests) |
| **Acceptance** | “Does the product meet agreed requirements?”           | Often manual UAT or sign-off scenarios; see [pyramid](./7_special_testing_fundamentals.md#test-pyramid) |

Typical CI shape: **smoke** (or tagged `@smoke`) on every PR → full **regression** on `main` / nightly → optional manual **acceptance** before release. **Sanity** is common after hotfixes (“login + checkout still work”).

**When to use:**

- Add **smoke** when E2E is slow — gate the long suite behind a 2–3 minute subset ([CI](./10_build_ci.md#continuous-integration-ci)).
- Run **regression** on every merge that touches shared code paths.
- Use **sanity** when full regression is too heavy for a small patch.

#### Common pitfalls

- Using “smoke” for the entire E2E folder — smoke must stay minimal or it stops being a gate.
- No regression after refactors — the pyramid only helps if CI actually re-runs the suite.
- Confusing **sanity** with **smoke** — sanity is **targeted**; smoke is **breadth-minimal**.

**Pluses and minuses:**

| +                              | −                                      |
| ------------------------------ | -------------------------------------- |
| Clear release and CI vocabulary | Teams define smoke differently — document yours |
| Faster feedback loops          | Over-tagging tests blurs categories    |

**Thoughts:** Tag tests (`@smoke`, `@regression`) in Playwright/Jest so one codebase serves multiple purposes.

**Sources:**

[Doka — how and why to write tests](https://doka.guide/tools/how-to-test-and-why/) · [Playwright — test projects & grep](https://playwright.dev/docs/test-projects)

---

</details>
