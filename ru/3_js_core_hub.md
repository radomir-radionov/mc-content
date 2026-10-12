# JS — Core

Основы языка: от синтаксиса до объектов и классов. Каждый раздел ведёт на отдельную страницу темы.

[← К JS hub](./3_js.md)

### Related docs

- [Browser hub](./3_js_browser_hub.md) · [JS Async](./3_js_async.md) · [Modules hub](./3_js_modules_hub.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Topic               | Covers                                                      |
| ------------------- | ----------------------------------------------------------- |
| **Language**        | Runtime, ECMAScript, strict mode, polyfills                 |
| **Variables**       | `let` / `const` / `var`, scope, hoisting, destructuring     |
| **Data Types**      | Primitives, coercion, stack vs heap                         |
| **Data Structures** | Objects, Map/Set, trees, Date, JSON                         |
| **Operators**       | Expressions, comparisons, optional chaining, `??`           |
| **Loops**           | Control flow, `switch`, errors, iterators, generators       |
| **Arrays**          | Indexed collections, iteration, `map` / `filter` / `reduce` |
| **Functions**       | Declarations, closures, `this`, HOF, recursion, timers      |
| **Objects**         | Prototypes, descriptors, Proxy/Reflect, GC                  |
| **Classes**         | Class syntax, inheritance, static and private members       |

### Sections

- [Language](#language)
- [Variables](#variables)
- [Data Types](#data-types)
- [Data Structures](#data-structures)
- [Operators](#operators)
- [Loops](#loops)
- [Arrays](#arrays)
- [Functions](#functions)
- [Objects](#objects)
- [Classes](#classes)

---

<a name="language"></a>

## Language

<details>
  <summary>Language</summary>

### Language

**Definition:** Как JavaScript выполняется в движках, связь с ECMAScript и базовые правила языка.

**How it works:** Контексты выполнения, strict mode и полифиллы — на странице темы Language.

**Full topic:** [JS — Language](./3_js_core.md)

**Sources:** [javascript.info/intro](https://javascript.info/intro) · [MDN — JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

</details>

<a name="variables"></a>

## Variables

<details>
  <summary>Variables</summary>

### Variables

**Definition:** Привязка имён к значениям с лексической областью видимости и hoisting.

**How it works:** `let`, `const`, `var`, temporal dead zone, `globalThis`, деструктуризация.

**Full topic:** [JS — Variables](./3_js_variables.md)

**Sources:** [javascript.info/variables](https://javascript.info/variables) · [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

</details>

<a name="data-types"></a>

## Data Types

<details>
  <summary>Data Types</summary>

### Data Types

**Definition:** Примитивные и ссылочные типы, приведение типов и хранение значений.

**How it works:** `typeof`, truthiness, явное и неявное преобразование, stack vs heap.

**Full topic:** [JS — Data Types](./3_js_data_types.md)

**Sources:** [javascript.info/types](https://javascript.info/types) · [MDN — Data types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

</details>

<a name="data-structures"></a>

## Data Structures

<details>
  <summary>Data Structures</summary>

### Data Structures

**Definition:** Встроенные и типовые структуры для группировки и обхода данных в JS.

**How it works:** Объекты, `Map` / `Set`, weak-коллекции, деревья, `Date`, `JSON`.

**Full topic:** [JS — Data Structures](./3_js_data_structures.md)

**Sources:** [javascript.info/map-set](https://javascript.info/map-set) · [MDN — Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

</details>

<a name="operators"></a>

## Operators

<details>
  <summary>Operators</summary>

### Operators

**Definition:** Операторы и выражения, задающие значения и порядок вычисления.

**How it works:** Арифметика, логика, сравнение, optional chaining (`?.`), nullish coalescing (`??`).

**Full topic:** [JS — Operators](./3_js_operators.md)

**Sources:** [javascript.info/operators](https://javascript.info/operators) · [MDN — Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)

</details>

<a name="loops"></a>

## Loops

<details>
  <summary>Loops</summary>

### Loops

**Definition:** Управление потоком — ветвления, циклы, ошибки и протоколы итерации.

**How it works:** `if` / `switch`, `for` / `while`, `try` / `catch`, итераторы и генераторы.

**Full topic:** [JS — Loops](./3_js_loops.md)

**Sources:** [javascript.info/ifelse](https://javascript.info/ifelse) · [javascript.info/try-catch](https://javascript.info/try-catch)

</details>

<a name="arrays"></a>

## Arrays

<details>
  <summary>Arrays</summary>

### Arrays

**Definition:** Упорядоченные индексированные коллекции с богатым API прототипа.

**How it works:** Создание, изменение, поиск, сортировка, `map`, `filter`, `reduce`.

**Full topic:** [JS — Arrays](./3_js_array.md)

**Sources:** [javascript.info/array](https://javascript.info/array) · [MDN — Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

</details>

<a name="functions"></a>

## Functions

<details>
  <summary>Functions</summary>

### Functions

**Definition:** Вызываемые единицы — объявления, выражения, стрелки и HOF.

**How it works:** Область видимости, замыкания, `this`, рекурсия, таймеры.

**Full topic:** [JS — Functions](./3_js_functions.md)

**Sources:** [javascript.info/function-basics](https://javascript.info/function-basics) · [MDN — Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)

</details>

<a name="objects"></a>

## Objects

<details>
  <summary>Objects</summary>

### Objects

**Definition:** Наборы свойств на прототипах и внутренних методах.

**How it works:** Цепочка прототипов, дескрипторы, `Proxy` / `Reflect`, основы GC.

**Full topic:** [JS — Objects](./3_js_objects.md)

**Sources:** [javascript.info/object](https://javascript.info/object) · [MDN — Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

</details>

<a name="classes"></a>

## Classes

<details>
  <summary>Classes</summary>

### Classes

**Definition:** Синтаксис `class` над прототипами — конструкторы, наследование, инкапсуляция.

**How it works:** `extends`, `super`, static, private fields, mixins.

**Full topic:** [JS — Classes](./3_js_classes.md)

**Sources:** [javascript.info/class](https://javascript.info/class) · [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

</details>
