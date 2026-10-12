# DRY, KISS, YAGNI

Three simplicity heuristics: one representation of knowledge, the simplest solution that works, and no speculative features. SOLID, GRASP, modeling, and TDD live in sibling topics.

[← Back to hub](./0_system_design.md)

### Related docs

* [System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [Tests](./7_special_testing.md) · [System design — Interview](./0_system_design_interview.md)

### Main concepts

| Area | Topics |
|------|--------|
| Knowledge | DRY |
| Simplicity | KISS, YAGNI |

### Sections

- [Design principles](#design-principles)

---

## Design principles

<details>
  <summary>DRY (Don't Repeat Yourself)</summary>

### DRY (Don't Repeat Yourself)

**Definition:** Every piece of knowledge should have a single, unambiguous representation in the system — not “fewer characters.”

**When to use:** Shared validation, constants, API clients, business rules. Skip coincidental similarity with different reasons to change.

**Pluses and minuses:**

| + | − |
|------|------|
| Fix once, fixed everywhere | Premature abstraction duplicates the *wrong* knowledge |
| Consistent behavior | “Copy-paste vs wrong abstraction” — duplicate once, extract on the third use (WET) |

**Examples:** Shared `formatCurrency()` is DRY. Two UIs that look similar but evolve separately should not share one component yet.

**Thoughts:** Two similar loops may be fine. SOLID letters: [SOLID](./7_special_engineering.md#9-oop-and-solid).

**Sources:** [Wikipedia — Don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

</details>

<details>
  <summary>KISS (Keep It Simple, Stupid)</summary>

### KISS (Keep It Simple, Stupid)

**Definition:** Prefer the simplest solution that meets current requirements. Complexity must be justified by measured need.

**When to use:** Default choice. Add layers only when scale or a real constraint shows up.

**Pluses and minuses:**

| + | − |
|------|------|
| Readable, cheaper to debug | May revisit when scale demands |
| Faster delivery | “Simple” is subjective |

**Thoughts:** KISS fights resume-driven architecture.

**Sources:** [Wikipedia — KISS principle](https://en.wikipedia.org/wiki/KISS_principle)

</details>

<details>
  <summary>YAGNI (You Aren't Gonna Need It)</summary>

### YAGNI (You Aren't Gonna Need It)

**Definition:** Don't implement functionality until it is actually needed.

**When to use:** Default stance. Build today's requirements with clean seams (OCP), not speculative features or unused frameworks.

**Pluses and minuses:**

| + | − |
|------|------|
| Less dead code, focus on current value | May refactor when the requirement arrives |
| Faster delivery | Under-engineering if the next need is already known |

**Thoughts:** YAGNI vs extensibility — design seams (interfaces) without building unused features. SOLID: [Open/Closed](./7_special_engineering.md#openclosed-ocp).

**Sources:** [Wikipedia — You aren't gonna need it](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it)

</details>
