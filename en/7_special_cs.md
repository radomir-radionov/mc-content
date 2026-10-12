# Computer science

The study of what can be computed, plus how we *write* programs (paradigms). Logic programming lives here. Algorithms, data structures, and web security stay on their own topics.

### Related docs

* [Computer science](./7_special_cs.md) · [JS Functions](./3_js_functions.md) · [OOP & SOLID](./7_special_engineering.md) · [Architecture patterns](./7_special_architecture.md) · [Algorithms](./7_special_algorithms.md) · [Data structures](./7_special_data_structures.md) · [Web security](./7_special_security.md)

### Sections

- [1. Computer science](#1-computer-science)
- [Programming](#programming)
- [Programming paradigm](#programming-paradigm)
- [Imperative vs declarative](#imperative-vs-declarative)
- [Procedural programming](#procedural-programming)
- [Logic programming](#logic-programming)
- [OOP vs FP](#oop-vs-fp)
- [Choosing a paradigm in practice](#choosing-a-paradigm-in-practice)

---

## 1. Computer science

<details>
  <summary>Computer science</summary>

### Computer science

**Definition:** Computer science is the systematic study of algorithmic processes that describe and transform information — their theory, analysis, design, efficiency, implementation, and application. The underlying question is what can be (efficiently) automated.

**How it works:**

- **Programming** is writing programs. **Computer science** asks what those programs *can* do, how costly they are, and which models of computation apply.
- Core maps in this handbook (do not copy the essays here): [Algorithms & Big O](./7_special_algorithms_core.md) · [Data structures](./7_special_data_structures.md) · [Programming paradigm](#programming-paradigm) (how we *write*) · [Web security](./7_special_security.md) (applied threats, not theory of computation).
- Mathematical logic shows up as a *tool* for specification and inference — see [Logic programming](#logic-programming).

**When to use:** Interview “what is CS vs coding?”; choosing whether a question is complexity, a language style, or an attack surface.

**Thoughts:** CS is not “more syntax.” Syntax is the medium; algorithms, structures, and models of computation are the substance.

**Sources:** [ACM — Computing as a discipline](https://dl.acm.org/doi/10.1145/63238.63239)

---

</details>

<details>
  <summary>Programming</summary>

### Programming

**Definition:** Programming is composing sequences of instructions (programs) that a computer executes. A program is an algorithm expressed in a programming language; code is the text of that program.

**Thoughts:**

Think in concepts, not syntax. Language is the medium; algorithms, data structures, and architecture are the substance.

---

</details>

<details>
  <summary>Programming paradigm</summary>

### Programming paradigm

**Definition:**

A programming paradigm is a coherent style and set of ideas that shape how programs are structured — e.g. imperative, declarative, object-oriented, functional.

| Paradigm | Focus | Typical constructs |
|----------|-------|-------------------|
| **Imperative** | How to do it — step-by-step state changes | variables, assignment, loops |
| **Declarative** | What to do — describe desired result | SQL, HTML, React JSX, `filter`/`map` |
| **Procedural** | Imperative + named subroutines sharing memory | functions, modules, globals |
| **OOP** | Entities as objects (class instances) | classes, inheritance, encapsulation |
| **Logic** | Solve via facts, rules, and inference — [Logic programming](./7_special_cs.md#logic-programming) | Prolog-style models |
| **Functional** | Math-like transforms; pure functions | map/filter/reduce, immutability |
| **Structural** | Control flow via blocks (if/while), no goto spaghetti | structured programming |

**How it works:**

Same destination, different instructions — turn-by-turn directions vs an address. Declarative code is usually easier to *read* as intent; both styles can take similar time to *write*. Hide implementation behind a lower layer when you are at business-logic height.

Один адрес — два объяснения: повороты vs «дом 38». JS — мультипарадигма; линзы, не религия.

**When to use:**

- Imperative: concrete algorithms, hardware/bit-level steps, performance-critical loops
- Declarative: UI (React), queries (SQL), config, “filter this list”
- OOP: domain models with clear entity boundaries
- FP: data transforms, concurrency-friendly pipelines
- Logic: [Logic programming](./7_special_cs.md#logic-programming) — constraint / knowledge modeling (uncommon in JS apps)

**Pluses and minuses:**

| + (multi-paradigm) | − |
|-------------------------|------|
| Pick the right tool per subproblem | Mixing paradigms inconsistently confuses teams |
| Modern languages support several (JS, TS, Python) | "Paradigm wars" distract from delivery |

**Examples:**

```js
// Imperative — how
let sum = 0;
for (const n of nums) sum += n;

// Declarative / functional — what
const sum = nums.reduce((a, b) => a + b, 0);
```

**Thoughts:** JavaScript is multi-paradigm. React blends declarative UI with functional state updates. Interview answer: paradigms are lenses, not religions.

**Sources:**

[Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/) · [cs.lmu.edu paradigms](https://cs.lmu.edu/~ray/notes/paradigms/)

---

</details>

<details>
  <summary>Imperative vs declarative</summary>

### Imperative vs declarative

**Definition:** **Imperative** code is a sequence of commands (*assign, loop, branch, mutate*). **Declarative** code states the result and lets another layer perform the steps (`array.filter`, SQL, JSX).

**How it works:**

- Imperative `onlyOdd`: `result = []` → `for…of` → `% 2` → `push` → `return` — you own every step.
- Declarative `onlyOdd`: “keep elements where the predicate is true” — `filter` hides allocation and iteration ([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter): new array of truthy callback results).
- Difference is **who owns implementation details**, not whether a CPU still loops underneath.

Императив — сам цикл и `push`. Декларатив — `filter`: важно *что* оставить. `filter` всё равно цикл, ты его не пишешь.

**When to use:**

Spell out an algorithm → imperative. Business logic → declarative API; put the loop in a named helper.

#### Common pitfalls

Calling `filter` “magic” — it is still a loop; you just do not reimplement it. Declarative style still needs a correct predicate.

**Pluses and minuses:**

| + | − |
|------|------|
| Declarative: intent-first, smaller working set in your head | Poor abstractions leak the how anyway |
| Imperative: full control for algorithms and side-effect order | Mixes *what* and *how*; harder to scan at scale |

**Examples:**

```js
function onlyOddImperative(array) {
  const result = [];
  for (const element of array) {
    if (element % 2 !== 0) result.push(element);
  }
  return result;
}

function onlyOddDeclarative(array) {
  return array.filter((element) => element % 2 !== 0);
}
```

**Thoughts:** *How* vs *what* — hide the how one layer below the decision you are making.

**Sources:**

[Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/) · [MDN filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

---

</details>

<details>
  <summary>Procedural programming</summary>

### Procedural programming

**Definition:** Imperative style grouped into **subroutines** (procedures/functions) that communicate mainly through **shared memory** (globals, buffers, device state) rather than only return values.

**How it works:**

Sequential commands live in named procedures. Modules grew out of this grouping, but reuse is weak if everything depends on one mutable store. Analog in JS: functions that read/write a shared array instead of taking args and returning new data.

Процедурное = функции + общая память (глобали). Просто для железа; плохо переиспользовать и тестировать.

**When to use:** Drivers, firmware-style bit flags, scripts that step a single workspace. Prefer explicit arguments once the program is larger than a toy.

**Pluses and minuses:**

| + | − |
|------|------|
| Simple mapping to hardware / one memory image | Shared mutable state is hard to reuse and test |
| Clear call order | Race conditions if two procedures mutate the same cells |

**Examples:**

```js
let memory = [0, 0, 0, 0, 0, 0, 0, 0];

function invertSmallestBit() {
  memory[7] = Number(!memory[7]);
  return memory;
}

function invertBiggestBit() {
  memory[0] = Number(!memory[0]);
  return memory;
}

invertSmallestBit(); // [0, 0, 0, 0, 0, 0, 0, 1]
invertBiggestBit();  // [1, 0, 0, 0, 0, 0, 0, 1]
invertSmallestBit(); // [1, 0, 0, 0, 0, 0, 0, 0]
```

**Thoughts:**

Scopes matter — procedures “talk” like globals. See [JS Core — scope](./3_js_core.md) for lexical vs global memory.

Procedures + shared RAM — the ancestor of modules, not a good default for app domain logic.

**Sources:**

[Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/)

---

</details>

<details>
  <summary>Logic programming</summary>

### Logic programming

**Definition:** A declarative paradigm: encode facts and rules; an inference engine (unification, resolution) searches for solutions — closer to applied logic than to sequential “do this then that.”

**How it works:**

- Translate the business problem into predicates. The runtime proves queries (classic: Prolog).
- Used for process modeling, constraints, expert-style rules — not typical day-to-day JS.
- Doka’s framing: closer to mathematics than to an instruction list; you must be able to recast the task as facts, rules, and queries.

Логическое: факты + правила, движок ищет ответ (Prolog). Во фронте почти не встречается.

**When to use:** Knowledge bases, constraint solvers, some AI/planning layers. Skip inventing a math model in an interview unless asked. How this sits among other *styles of writing*: [Programming paradigm](#programming-paradigm).

**Pluses and minuses:**

| + | − |
|------|------|
| Compact expression of rules and relations | Steep encoding cost; rare in frontend stacks |
| Engine searches combinations for you | Debugging search trees is unlike stepping a loop |

**Examples:** (sketch, not JS) `parent(X, Y)` + `ancestor` rules; query `ancestor(alice, Who)`.

**Thoughts:** You state *truths*; the machine derives *answers*.

**Sources:**

[Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/) · [Wikipedia — Logic programming](https://en.wikipedia.org/wiki/Logic_programming)

---

</details>

<details>
  <summary>OOP vs FP</summary>

### OOP vs FP

**Definition:**

Comparison of object-oriented and functional composition styles — objects encapsulate state vs functions transform immutable data.

| | OOP | FP |
|---|-----|-----|
| Unit of composition | Object | Function |
| State | Encapsulated in objects | Avoided or isolated |
| Extension | Inheritance / interfaces | Composition + HOFs |
| Best for | Entity-rich domains | Data pipelines, transforms |

**How it works:**

ООП — сущности со состоянием. ФП — преобразования данных. Во фронте обычно смесь (компоненты + чистые селекторы).

---

**When to use:** Hybrid is standard in modern frontend — OOP for components/classes, FP for state and selectors.

**Pluses and minuses:**

| + (combining) | − |
|------------------|------|
| React: components (OOP-like) + hooks (FP) | Inconsistent style across codebase |
| Domain objects + pure utilities | Team must agree on boundaries |

**Thoughts:** Interview: "Neither replaces the other. Use objects for nouns, functions for verbs."

</details>

<details>
  <summary>Choosing a paradigm in practice</summary>

### Choosing a paradigm in practice

**Definition:** Pure FP and pure OOP are both rare in production. Pick the style that makes **this** task cheap — team conventions, language, and habit matter; **fit to the problem** matters most.

**How it works:**

One function is enough → do not spawn a class zoo. Need I/O → do not refuse FP’s testable core; put effects at the edge. JS is **multi-paradigm** (OOP via prototypes/`class` with caveats). Same module can be a stateful `Calculator` or `add`/`subtract` of arguments.

Чистый ФП и чистый ООП редко нужны. Одна функция — не зоопарк классов. JS можно мешать. Удобство задачи важнее «чистоты» парадигмы.

**When to use:** Default to the smallest abstraction. Mix procedural glue, OOP entities, and FP transforms in one JS app.

**Examples:**

```js
class Calculator {
  constructor(initial = 0) {
    this.value = initial;
  }
  add(x) {
    this.value += x;
  }
  subtract(x) {
    this.value -= x;
  }
  valueOf() {
    return this.value;
  }
}

const calculator = new Calculator();
calculator.add(3);
calculator.add(4);
calculator.subtract(1);
calculator.valueOf(); // 6

function add(a, b) {
  return a + b;
}
function subtract(a, b) {
  return a - b;
}
subtract(add(add(0, 3), 4), 1); // 6
```

**Thoughts:** Convenience for the task beats paradigm purity — JS lets you mix.

**Sources:**

[Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/)

---

</details>

## Interview Practice

<details>
  <summary>Is computer science the same as programming?</summary>

**Answer:** No. Programming composes instructions a machine executes. Computer science studies algorithmic processes — what can be automated, at what cost, under which model. You can program without naming Big O; you cannot call a complexity or computability answer “just syntax.”

**See also:** [Computer science](#computer-science) · [Programming](#programming)

</details>

<details>
  <summary>What is logic programming?</summary>

**Answer:** Facts + rules; an engine (Prolog-style unification/resolution) searches for answers. It is a declarative CS technique, uncommon in everyday JS. Do not confuse it with `&&` / `||` in JavaScript.

**See also:** [Logic programming](#logic-programming)

</details>
