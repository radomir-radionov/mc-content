# Testing — Practices

TDD, BDD, test doubles, and making tests easy to write.

[← Back to hub](./7_special_testing.md)

### Related docs

- [Fundamentals](./7_special_testing_fundamentals.md) · [Layers](./7_special_testing_layers.md) · [Practices](./7_special_testing_practices.md) · [Tooling](./7_special_testing_tooling.md) · [React — Testing](./4_react_ecosystem.md#testing) · [Next.js — Vitest + RTL](./4_nextjs_platform.md#testing-nextjs-vitest--rtl) · [CI/CD](./10_build_ci.md#continuous-integration-ci) · [Interview leftovers](./7_special_interview.md)

### Sections

- [Practices](#practices)
- [TDD (Test-Driven Development)](#tdd-test-driven-development)
- [BDD (Behavior-Driven Development)](#bdd-behavior-driven-development)
- [Test doubles (stubs, mocks, spies)](#test-doubles-stubs-mocks-spies)
- [Making tests easy](#making-tests-easy)

---

<a name="practices"></a>

## Practices

<details>
  <summary>TDD (Test-Driven Development)</summary>

### TDD (Test-Driven Development)

**Definition:** A development practice where you write a **failing test first**, then the **minimum code** to pass it, then **refactor** — repeat in small cycles (~10–15 min).

**How it works:**

| Phase        | Goal                                   | Rule                                                                                         |
| ------------ | -------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Red**      | Test fails for the **expected reason** | If it fails for another reason (syntax, import), fix the test before writing production code |
| **Green**    | Make the test pass                     | Simplest implementation — even "obvious" one-liners; don't over-engineer yet                 |
| **Refactor** | Clean up test + production code        | Safe because tests catch regressions immediately                                             |

TDD only covers the happy path. The cycle applies to **any** case — errors, edge cases, and invalid input are just the next Red test.

**When to use:**

Core business logic, utilities, parsers, reducers — anywhere inputs/outputs and edge cases are specifiable. Less suited for UI spikes, throwaway prototypes, or exploratory research.

#### Common pitfalls

- **Fake test** — `expect(expected).toEqual(expected)` always passes; always assert the **actual result**.
- **Wrong failure in Red** — test crashes on import/type error, not on missing behavior → fix test setup first.
- **`toFixed()` returns a string** — `Number((a / b).toFixed(n))` if you need a number back.
- **Optional args** — use `settings = {}` or a shared `defaultSettings` object, not bare `settings.precision`.
- **Over-complex tests** — test code is still code; keep tests readable, no nested logic.

**Pluses and minuses:**

| +                                                             | − / costs                                              |
| ------------------------------------------------------------- | ------------------------------------------------------ |
| Tests woven into the workflow — not "extra work after"        | Slower at first; ~6 months to feel natural             |
| Forces you to design the **public API** before implementation | Requires thinking about API upfront                    |
| Living documentation with runnable examples                   | "Make tests fail first" feels awkward but is essential |
| Safer refactoring — regressions show up instantly             | Not every task benefits (exploratory spikes)           |

**Examples:**

```js
const defaultSettings = { precision: 1 };

function divide(a, b, settings = defaultSettings) {
  if (b === 0) {
    throw new Error("I'm sorry, Dave. I'm afraid I can't do that.");
  }
  const { precision } = settings;
  return Number((a / b).toFixed(precision));
}

// 1. Red — basic division
test("returns a / b", () => {
  expect(divide(10, 5)).toBe(2);
});

// 2. Red — decimals (default 1 digit)
test("rounds to 1 decimal by default", () => {
  expect(divide(10, 3)).toBe(3.3);
});

// 3. Red — custom precision
test("respects precision option", () => {
  expect(divide(10, 3, { precision: 2 })).toBe(3.33);
});

// 4. Red — divide by zero (wrap call so Jest catches the throw)
test("throws on divide by zero", () => {
  expect(() => divide(10, 0)).toThrow(
    "I'm sorry, Dave. I'm afraid I can't do that.",
  );
});
```

**Refactor tests** with table-driven cases — same assertion, many inputs:

```js
it.each([
  { a: 10, b: 5, expected: 2 },
  { a: 15, b: 5, expected: 3 },
])("divide($a, $b) → $expected", ({ a, b, expected }) => {
  expect(divide(a, b)).toBe(expected);
});
```

**Thoughts:** Small red → green → refactor loops turn big tasks into verified steps; tests are the safety net that makes refactoring cheap. TDD also keeps test doubles small: if the test needs a park of mocks, redesign the API first ([Test doubles](#test-doubles-stubs-mocks-spies)).

**Sources:**

[Jest — toThrow](https://jestjs.io/docs/expect#tothrowerror) · [Eric Elliott on TDD](https://medium.com/javascript-scene)

---

</details>

<details>
  <summary>BDD (Behavior-Driven Development)</summary>

### BDD (Behavior-Driven Development)

**Definition:** Tests describe behavior in business language (often Gherkin: Given/When/Then); extends TDD with shared vocabulary between devs and stakeholders.

**When to use:** Acceptance criteria, cross-team alignment, feature files as contract.

**Pluses and minuses:**

| +                           | −                           |
| --------------------------- | --------------------------- |
| Readable specs for non-devs | Gherkin ceremony can bloat  |
| Focus on user outcomes      | Tooling overhead (Cucumber) |

**Examples:**

```gherkin
Scenario: Successful login
  Given a registered user "ada@example.com"
  When they log in with valid credentials
  Then they see the dashboard
```

|          | TDD              | BDD                    |
| -------- | ---------------- | ---------------------- |
| Focus    | Code correctness | Behavior / user value  |
| Language | Code tests       | Often natural language |
| Audience | Developers       | Devs + PM + QA         |

**Mocha-style `describe` / `it` (tutorial / classic BDD runners):**

- Spec = tests + docs + examples: `describe("pow", …)` groups; `it("raises to n-th power", …)` is one case.
- Prefer **one assertion per `it`** — if the first `assert` fails, later ones never run.
- Nested `describe` groups related cases; `before` / `after` / `beforeEach` / `afterEach` for setup/teardown.
- Chai-style checks: `assert.equal` (`==`), `assert.strictEqual` (`===`), `assert.isNaN`, …
- Flow: write failing spec → implement → green → add cases. Same spirit as [TDD](#tdd-test-driven-development); modern stack often uses Jest/Vitest with the same `describe`/`it` shape — see [Tests and AAA](./7_special_testing_fundamentals.md#tests-and-aaa).

```js
describe("pow", function () {
  it("2^3 is 8", function () {
    assert.equal(pow(2, 3), 8);
  });
  it("for negative n returns NaN", function () {
    assert.isNaN(pow(2, -1));
  });
});
```

**Sources:** [javascript.info/testing-mocha](https://javascript.info/testing-mocha)

---

</details>

<details>
  <summary>Test doubles (stubs, mocks, spies)</summary>

### Test doubles (stubs, mocks, spies)

**Definition:** Test doubles are stand-ins for real dependencies. They share the same interface but are simpler, so tests stay fast, isolated, and repeatable when the unit under test has side effects (I/O, globals, other modules).

**How it works:**

- Pure functions need no doubles: call with arguments, assert the return value.
- Impure functions (`Math.random()`, `fetch`, DOM, `void` helpers that mutate the world) are hard to assert without replacing the dependency.
- Two practical groups (Doka): **stubs** replace a dependency and do **not** assert usage; **mocks** replace it **and** let you assert calls, args, and resulting state. **Spies** sit with mocks: they wrap a real method, record calls, and optionally stub the return value.
- Classic catalog (Meszaros / Fowler): **dummy** (unused filler arg), **stub** (canned answers), **spy** (records calls, often still runs real code), **mock** (pre-programmed expectations you verify), **fake** (working but simplified, e.g. in-memory repo).
- Jest: `jest.fn()` is a mock function (`undefined` if no impl). `.mock` stores `calls`, `results`, `instances`, `contexts`, `lastCall`. Matchers: `toHaveBeenCalled`, `toHaveBeenCalledWith` (same algorithm as `toEqual`). `jest.spyOn(obj, method)` tracks `obj[method]` and **still calls the original** unless you `.mockImplementation` / `.mockReturnValue`. `mockRestore()` restores the original — it only works for `spyOn`, not for a bare `jest.fn()` you assigned yourself. Prefer `afterEach(() => jest.restoreAllMocks())` over swapping `global.Math`.
- Inject dependencies (`getUser(id, repo)`) instead of mocking globals. Shared fixtures: one valid `fakeUser`, then named variants (`fakeUserInvalidEmail`). Put project-wide stubs/mocks in a test helpers module and override per suite.

**When to use:**

Stub `random` / clock / repo when you only need a known return. Mock / spy when the unit returns `undefined` and the contract is “call `toggleClassName('dark-theme')`”. Fake in-memory DB for integration-style unit tests. Skip doubles when the function is already pure.

#### Common pitfalls

- **Mock when a stub would do** — extra assertions on call counts couple tests to internals.
- **If you can avoid mocking, do** — fewer doubles to maintain; inject deps; shrink the impure shell ([DIP](./7_special_engineering.md#dependency-inversion-dip), [TDD](#tdd-test-driven-development)).
- **Assigning `global.Math = stub`** without restore leaks into other files; use `spyOn` + restore.
- **`spyOn` still runs the real method** until you mock the implementation — accidental I/O.
- **Shared fixture mutation** — copy fixtures per test (`{ ...fakeUser }`) so one suite does not poison another.

**Pluses and minuses:**

| +                                                      | −                                               |
| ------------------------------------------------------ | ----------------------------------------------- |
| Fast, deterministic tests without DB/DOM/network       | Over-mocking tests the mock, not the product    |
| Assert collaboration (calls + args) for effectful APIs | Doubles drift when the real interface changes   |
| Shared fixtures keep suites short                      | Global mocks leak between tests if not restored |

**Examples:**

```js
function addRandom(a) {
  return a + Math.random();
}

function toggleTheme(app) {
  app.toggleClassName("dark-theme");
  app.userChangedTheme = true;
}

const fakeUser = { name: "Alex", email: "alex@site.com", role: "user" };
const fakeUserInvalidEmail = { ...fakeUser, email: "not-an-email" };

afterEach(() => {
  jest.restoreAllMocks();
});

test("stub: canned return, no call assertions required", () => {
  jest.spyOn(Math, "random").mockReturnValue(0.42);
  expect(addRandom(2)).toBe(2.42);
});

test("mock: assert how the dependency was used", () => {
  const fakeApp = { toggleClassName: jest.fn(), userChangedTheme: false };
  toggleTheme(fakeApp);
  expect(fakeApp.toggleClassName).toHaveBeenCalledWith("dark-theme");
  expect(fakeApp.userChangedTheme).toBe(true);
});

test("spy: wrap a real method; restore after", () => {
  const video = {
    play() {
      return true;
    },
  };
  const spy = jest.spyOn(video, "play");
  expect(video.play()).toBe(true);
  expect(spy).toHaveBeenCalled();
});
```

**Thoughts:** Stub to _have_ a dependency; mock/spy to _check how it was used_ — and prefer no double at all if the code can stay pure or injected.

**See also:** Shared fixture store + auto-restore as infrastructure — [Making tests easy](#making-tests-easy). Group-level stubs/drivers — [Integration testing](./7_special_testing_layers.md#integration-testing).

**Sources:**

[Jest mock functions](https://jestjs.io/docs/mock-functions) · [jest.spyOn](https://jestjs.io/docs/jest-object#jestspyonobject-methodname) · [toHaveBeenCalledWith](https://jestjs.io/docs/expect#tohavebeencalledwitharg1-arg2-) · [Doka — mocks & stubs](https://doka.guide/tools/mocks-n-stubs/)

---

</details>

<details>
  <summary>Making tests easy</summary>

### Making tests easy

**Definition:** Test laziness is usually **friction**: if a test needs a forest of mocks, shared state, or a novel data shape, people skip it. Lower friction in the **product code** and in the **suite**, and writing tests stops feeling like extra work. Why tests pay off: [Why write tests (AAA)](./7_special_testing_fundamentals.md#tests-and-aaa).

**How it works:**

1. **Pure core** — same args → same return, no I/O. Tests need only inputs and expected output. Put business rules here ([Pure functions](./3_js_functions.md#pure-functions--side-effects)).
2. **Functional architecture (impureim sandwich)** — impure may call pure, never the reverse. Thin I/O shell: fetch/DB/UI → **pure transform** → persist/render. The “bread” sets up the call and observes the result ([Functional core, imperative shell](./7_special_architecture.md#functional-core-imperative-shell)).
3. **Low coupling** (зацепление / сцепление) ≠ **high cohesion** (связность / SRP). Tight coupling → mock archaeology. **DIP:** details depend on abstractions; tests swap fakes at the contract ([Cohesion vs coupling](./7_special_engineering.md#cohesion-vs-coupling), [DIP](./7_special_engineering.md#dependency-inversion-dip)).
4. **Independent tests** — any order, any machine, parallel workers. Shared DB/files/time leak into other cases and hide the real failure. Use `--randomize` to catch order bugs.
5. **Clear setup and expectations (ПВП / AAA)** — enough detail to see _what_ and _under which preconditions_; not a novel. One naming style per repo. Anatomy: [Why write tests (AAA)](./7_special_testing_fundamentals.md#tests-and-aaa).
6. **Unit-test your code only** — do not unit-test libraries you cannot change. Cover **adapters**. Dependency upgrades: [Test layers](./7_special_testing_layers.md#test-layers).
7. **Comfortable runner (Jest)** — `test.only` / `describe.only` (do not commit), `test.skip`, `test.todo` (name only — a callback is an error; use `skip` for implemented-but-disabled). Watch: `--watch` ≈ `-o` (related to **uncommitted** files); `--watchAll` reruns everything. Git: `jest -o` / `--onlyChanged`, `--lastCommit`, `--changedSince=<branch|sha>`. Filter: `jest path/pattern`, `-t` / `--testNamePattern`. `--debug` prints **config**, not a debugger — use `node --inspect-brk` + `--runInBand` to step through tests.
8. **Shared fakes** — fixtures/factories by entity; auto-reset (`--restoreMocks` / `afterEach(jest.restoreAllMocks)`). Treat test infra like product code.

**When to use:**

New module you dread testing; flaky suite that only fails in CI order; TDD skipped under deadline because setup hurts.

#### Common pitfalls

- **`--debug` ≠ debugger** — it dumps Jest config.
- **`test.todo(fn)` throws** — todo is a reminder, not a skipped implementation.
- **Leaving `test.only` in git** — CI silently drops the rest of the file.
- **Unit-testing `lodash` / React internals** — time sink; test _your_ wrapper.
- **Shared mutable fixture** — clone per test (`{ ...fakeUser }`).

**Pluses and minuses:**

| +                                             | −                                                   |
| --------------------------------------------- | --------------------------------------------------- |
| Less mock theater; failures point at one unit | Upfront design (DIP, sandwich, fixture module)      |
| Parallel CI; cheaper onboarding               | Over-abstracted ports if every function is injected |

**Examples:**

```js
// fixtures/user.js — shared fake
export const fakeUser = { id: "1", name: "Ada", email: "ada@ex.com" };

// pure core
export function greeting(user) {
  return `Hello, ${user.name}`;
}

// impure shell (not unit-tested here — adapter + E2E)
export async function showGreeting(id, repo, render) {
  const user = await repo.findById(id); // impure in
  render(greeting(user)); // pure middle, impure out
}

test("greeting uses the user name", () => {
  // Arrange
  const user = { ...fakeUser, name: "Ada" };
  // Act
  const text = greeting(user);
  // Assert
  expect(text).toBe("Hello, Ada");
});
```

```bash
npx jest -o                          # tests related to uncommitted files
npx jest --changedSince=main         # since a branch/commit
npx jest --watch                     # watch + related tests
npx jest -t greeting                 # name filter
npx jest --restoreMocks --randomize  # isolation + order bugs
```

**Thoughts:** Comfortable tests come from a pure, loosely coupled core plus independent AAA cases, your-code-only units, and shared fixtures — not from more willpower.

**Sources:**

[Doka — how to make testing easier](https://doka.guide/tools/how-to-make-testing-easier/) (Bespoiasov, 2023-01-25) · [Jest CLI](https://jestjs.io/docs/cli) · [Jest API `.only` / `.skip` / `.todo`](https://jestjs.io/docs/api)

---

</details>
