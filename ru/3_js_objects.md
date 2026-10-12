# JS Core — Объекты

Объекты, прототипы, `this`, дескрипторы, Proxy/Reflect и сборка мусора.

### Связанные материалы

- [Язык JS](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массивы](./3_js_array.md) · [Функции](./3_js_functions.md) · [Классы](./3_js_classes.md) · [Документ](./3_js_browser.md) · [События](./3_js_events.md) · [Browser APIs](./3_js_browser_extra.md) · [Асинхронность](./3_js_async.md) · [Интервью JS](./3_js_interview.md)
- [OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop) · [Классы (TypeScript)](./5_ts_classes.md) · [GRASP](./0_system_design_grasp.md)

[← К оглавлению JS](./3_js.md)

### Основные темы

| Область         | Темы                                                   |
| --------------- | ------------------------------------------------------ |
| **Объекты**     | Литералы, ссылки, копирование, ключи/значения          |
| **Свойства**    | Конфигурация, флаги, дескрипторы, геттеры/сеттеры      |
| **Прототипы**   | Наследование, `F.prototype`, нативные, `Object.create` |
| **Продвинутое** | Proxy/Reflect, factory/module patterns                 |
| **Память**      | GC, mark-and-sweep, утечки, `WeakRef`                  |

### Sections

> Теория в блоках `<details>`: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** обязателен.

- [Objects](#objects)
- [`this`](#this)
- [Prototypes](#prototypes)
- [Object property configuration](#object-property-configuration)
- [Advanced topics](#advanced-topics)
- **Objects:** [Class vs prototype inheritance](#class-vs-prototype-inheritance) · [OOP in JavaScript](#oop-in-javascript) · [Object types](#objects) · [Ways to create objects](#ways-to-create-objects) · [Objects to primitives](#objects-to-primitives) · [Objects & references](#objects-and-references)
- **`this`:** [`this` binding](#this-binding-four-rules) · [`this` and call context](#this-and-call-context) · [call, apply, bind](#call-apply-bind)
- **Prototypes:** [Prototypal inheritance](#prototypal-inheritance) · [F.prototype](#f-prototype) · [Native prototypes](#native-prototypes) · [Prototype methods, objects without `__proto__`](#prototype-methods-objects-without-proto) · [`instanceof`](#instanceof) · [Extending built-in classes](#extending-built-in-classes)
- **Object property configuration:** [Object properties configuration](#object-properties-configuration) · [Property flags and descriptors](#property-flags-and-descriptors) · [Property getters and setters](#property-getters-and-setters)
- **Advanced topics:** [Shallow vs deep copy](#shallow-vs-deep-copy) · [`in` / `for...in`](#in-operator-and-for-in) · [Garbage collection](#garbage-collection) · [WeakRef / FinalizationRegistry](#weakref-and-finalizationregistry) · [Proxy and Reflect](#proxy-and-reflect) · [Factory and module patterns](#factory-and-module-patterns)
- [Интервью](./3_js_interview.md)

Соседние темы Core: [Язык JS](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Массивы](./3_js_array.md) · [Функции](./3_js_functions.md) · [Классы](./3_js_classes.md).

<a name="objects-references"></a>

## Objects

### Objects

<details>
  <summary>Class vs prototype inheritance</summary>

### Class vs prototype inheritance

**Definition:**

**Class inheritance** uses `class extends` syntax; **prototype inheritance** links objects via `[[Prototype]]` — `Object.create`, `Object.setPrototypeOf`.

| | Class syntax | Prototype chain |
| | ------------------- | -------------------------------- |
| | Readability | Familiar to OOP devs | Lower-level, flexible |
| | Under the hood (JS) | Desugars to prototypes | Native mechanism |
| | Constructors | `constructor()` required pattern | Factory functions + `Object.create` |

**When to use:** `class` for most app code; prototypes for libraries, mixins, or understanding JS internals.

**Pluses and minuses:**

| + (prototypes)                   | −                                          |
| -------------------------------- | ------------------------------------------ |
| Dynamic — add methods at runtime | Harder for newcomers                       |
| Memory-efficient shared methods  | `setPrototypeOf` is slow — avoid hot paths |

**Examples:**

```js
const animal = {
  speak() {
    return "sound";
  },
};
const dog = Object.create(animal);
dog.speak = () => "bark";
```

**Thoughts:** `class` methods live on `prototype`, not instance. `instanceof` walks the prototype chain. Полная таблица сравнения: [OOP in JavaScript](#oop-in-javascript).

**Sources:**

[OOP in JavaScript](#oop-in-javascript) · [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)

---

</details>

<details>
  <summary>OOP in JavaScript</summary>

### OOP in JavaScript

**Definition:** JavaScript **прототипный** — объекты делегируют через `[[Prototype]]`. ООП опирается на четыре концепции: инкапсуляция, наследование, полиморфизм, абстракция. `class` (ES6+) — **синтаксический сахар** над функциями-конструкторами и цепочкой прототипов; **`extends`** и **`super()`** связывают наследование; приватные поля — `#`.

**How it works:**

#### 1. Современный подход: классы (ES6+)

**Класс** — чертёж (шаблон) для создания объектов. `constructor` инициализирует свойства; `new` создаёт экземпляр.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} издаёт звук.`);
  }
}

const dog = new Animal("Шарик");
dog.speak(); // Шарик издаёт звук.
```

#### 2. Под капотом: прототипы

До классов использовали **функции-конструкторы** и `.prototype`. Этот механизм работает и сегодня. При вызове метода движок ищет его в объекте, затем идёт по **цепочке прототипов**.

Код ниже делает то же, что `class Animal` выше:

```js
function AnimalOld(name) {
  this.name = name;
}

AnimalOld.prototype.speak = function () {
  console.log(`${this.name} издаёт звук.`);
};

const cat = new AnimalOld("Мурзик");
cat.speak(); // Мурзик издаёт звук.
```

Методы лежат в прототипе, чтобы **не дублироваться** в каждом экземпляре.

#### 3. Четыре столпа ООП в JavaScript

ООП базируется на четырех ключевых концепциях. Вот как они реализуются в JS:

**Инкапсуляция**

Это сокрытие внутренней логики и данных объекта от прямого доступа извне. В современном JS для создания полностью приватных свойств и методов используется префикс `#`.

```js
class BankAccount {
  #balance = 0; // Приватное свойство, недоступно снаружи класса

  constructor(owner) {
    this.owner = owner;
  }

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return this.#balance; // Доступ к данным только через контролируемый метод
  }
}

const account = new BankAccount("Иван");
account.deposit(100);
console.log(account.getBalance()); // 100
// console.log(account.#balance); // Ошибка! Свойство приватное.
```

**Наследование**

Позволяет создать дочерний класс на основе родительского, чтобы не дублировать код. Реализуется с помощью ключевого слова `extends` и вызова `super()`.

```js
class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Вызов конструктора родительского класса Animal
    this.breed = breed;
  }

  // Переопределение метода родителя
  speak() {
    console.log(`${this.name} (порода: ${this.breed}) лает: Гав!`);
  }
}

const lab = new Dog("Босс", "Лабрадор");
lab.speak(); // Вывод: Босс (порода: Лабрадор) лает: Гав!
```

**Полиморфизм**

Возможность объектов с разной структурой использовать одноименные методы, которые будут работать по-разному в зависимости от контекста.

```js
class Cat extends Animal {
  speak() {
    console.log(`${this.name} мяукает: Мяу!`);
  }
}

// Массив разных животных
const animals = [new Dog("Рекс", "Овчарка"), new Cat("Пушок")];

// Один и тот же метод speak() ведет себя по-разному для каждого объекта
animals.forEach((animal) => animal.speak());
// Вывод:
// Рекс (порода: Овчарка) лает: Гав!
// Пушок мяукает: Мяу!
```

**Абстракция**

Выделение главного и сокрытие деталей реализации. Мы создаём интерфейс взаимодействия, не заставляя пользователя разбираться в сложных внутренних механизмах.

```js
const video = document.querySelector("video");
video.play(); // без деталей decode/buffer — просто используем API
```

#### 4. Классы vs прототипы

| Критерий    | Классы (`class`)                              | Прототипы (`prototype`)                                  |
| ----------- | --------------------------------------------- | -------------------------------------------------------- |
| Синтаксис   | Чистый, декларативный                         | Раздельный: конструктор отдельно, методы на `.prototype` |
| Приватность | `#` private fields                            | Замыкания или соглашение `_field`                        |
| Поднятие    | Не поднимаются — нельзя вызвать до объявления | Функции-конструкторы поднимаются (в отличие от `class`)  |
| Под капотом | Та же цепочка прототипов                      | Прямая работа с объектами-прототипами                    |

Полное сравнение: [Class vs prototype inheritance](#class-vs-prototype-inheritance).

#### 5. `extends`, `super()` и цепочка прототипов

| Feature              | Role                                                                                                         |
| -------------------- | ------------------------------------------------------------------------------------------------------------ |
| **`extends`**        | Дочерний class наследует parent; **только один родитель**. Связывает `Child.prototype` → `Parent.prototype`. |
| **`super()`**        | В `constructor` потомка — вызывает конструктор parent; **обязателен до использования `this`**.               |
| **`super.method()`** | В переопределённом методе — вызывает реализацию parent.                                                      |
| **`static`**         | Методы на самом class (не на экземплярах); подклассы наследуют static-члены.                                 |
| **`instanceof`**     | Идёт по `[[Prototype]]` — `dog instanceof Dog` === `true`, если `Dog.prototype` в цепочке.                   |

- `class` раскрывается в конструктор + методы на `.prototype` — наследование это **цепочка прототипов**, а не копирование полей.
- Только **одиночное наследование** — предпочитайте **композицию** (`Object.assign`, миксины) глубоким деревьям `extends`.
- Инкапсуляция без `#`: замыкания (module pattern), ES-модули или соглашение `_field`.

Развёрнутые блоки по столпам (when to use, composition vs inheritance, SOLID): [OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop) · [Encapsulation](./7_special_engineering.md#encapsulation) · [Inheritance](./7_special_engineering.md#inheritance) · [Polymorphism](./7_special_engineering.md#polymorphism) · [Abstraction](./7_special_engineering.md#abstraction).

**When to use:** Доменные модели с поведением; библиотеки с экземплярами. Для DTO и пайплайнов — простые объекты + функции.

**Pluses and minuses:**

| +                                                | −                                                             |
| ------------------------------------------------ | ------------------------------------------------------------- |
| `class` / `extends` / `super()` читаются знакомо | `super()` обязателен до `this` в constructor потомка          |
| Прототипы гибки для библиотек и делегирования    | Глубокие иерархии наследования хрупки                         |
| Замыкания + модули дают инкапсуляцию без классов | Только один `extends` — для нескольких ролей нужна композиция |

**Thoughts:** Хорошее ООП в JS — соответствие сущностей домену, а не глубокие деревья `extends`. Эта страница — **канонический JS-гайд** (синтаксис + примеры столпов); контекст system design — в [OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop).

**Sources:** [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements) · [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain) · [javascript.info — Prototypal inheritance](https://javascript.info/prototype-inheritance) · [Doka — OOP](https://doka.guide/tools/oop/) · [OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop) · [Classes (TypeScript)](./5_ts_classes.md)

---

</details>

<a name="objects"></a>

<details>
  <summary>Object types</summary>

### Object types

**Definition:** An object stores keyed collections (`key: value` pairs). Arrays are a specialized object for ordered, indexed collections.

**How it works:**

- Object literals create plain objects with `[[Prototype]]` = `Object.prototype` (unless `Object.create(null)`).
- Property access: own keys first, then prototype chain lookup for reads; writes always create/change own properties.
- Keys are strings or symbols; array indices are string keys `"0"`, `"1"`, …

**When to use:**

Config/state objects; method bags on prototypes; `Object.create(null)` for pure dictionaries (no `toString` pollution).

#### Common pitfalls

Shallow copy still shares nested objects; `delete` only removes own properties; comparing `{a:1} === {a:1}` is always false.

| Objects                      | Arrays                                 |
| ---------------------------- | -------------------------------------- |
| Named keys (strings/symbols) | Numeric indices + `length`             |
| General-purpose records      | Ordered collections with array methods |
| Not iterable by default      | Iterable (`for...of`)                  |

**Pluses and minuses:**

| +                             | −                                             |
| ----------------------------- | --------------------------------------------- |
| Flexible key-value records    | Reference semantics — aliases share mutations |
| Prototype sharing for methods | No built-in deep equality                     |
| `Object.*` static helpers     | `for...in` includes inherited enumerable keys |

**Examples:**

```js
const user = { name: "John", age: 30 };

// Object literal
const admin = user;
admin.name = "Jane";
console.log(user.name); // 'Jane' — same reference

delete user.age; // remove property
```

**Thoughts:** Objects are stored and copied **by reference**; primitives are copied **by value**.

**Sources:**

[javascript.info/object](https://javascript.info/object)

#### Object types in JavaScript

**Definition:**

Objects fall into several categories depending on their origin.

| Type                  | Examples                                               |
| --------------------- | ------------------------------------------------------ |
| **Native / built-in** | `Object`, `Array`, `Date`, `Error`                     |
| **Host**              | `window`, `document` (browser); `fs`, `http` (Node.js) |
| **Custom**            | Created via literals, constructors, or classes         |
| **Prototype**         | Template objects shared via the prototype chain        |

**When to use:** Avoid extending native prototypes (`Array.prototype.myMethod = ...`) except for polyfills — risks naming collisions.

**Sources:**

[javascript.info/object](https://javascript.info/object)

#### Property existence: `in` vs `hasOwnProperty`

**Definition:**

The `in` operator checks the full prototype chain; `hasOwnProperty` / `Object.hasOwn()` check only own properties.

| Method                      | Checks prototype chain? |
| --------------------------- | ----------------------- |
| `'key' in obj`              | Yes                     |
| `obj.hasOwnProperty('key')` | No (own only)           |
| `Object.hasOwn(obj, 'key')` | No (own only, modern)   |

**Examples:**

```js
const obj = { a: 1 };

console.log("a" in obj); // true (own)
console.log("toString" in obj); // true (inherited)
console.log(obj.hasOwnProperty("a")); // true
console.log(obj.hasOwnProperty("toString")); // false
```

**Sources:**

[javascript.info/keys-values-entries](https://javascript.info/keys-values-entries)

#### Object static methods

**Definition:**

`Object.keys`, `Object.values`, `Object.entries`, and `Object.is` operate on own enumerable properties. `Object.fromEntries` rebuilds an object from pairs (inverse of `entries`).

| Method                         | Returns                                  |
| ------------------------------ | ---------------------------------------- |
| `Object.keys(obj)`             | Array of own keys                        |
| `Object.values(obj)`           | Array of own values                      |
| `Object.entries(obj)`          | Array of `[key, value]` pairs            |
| `Object.fromEntries(iterable)` | Object from pairs                        |
| `Object.is(a, b)`              | Strict equality; `NaN === NaN` is `true` |

**Examples:**

```js
const user = { name: "Ann", age: 25 };

for (const [key, value] of Object.entries(user)) {
  console.log(key, value);
}

const doubled = Object.fromEntries(
  Object.entries({ a: 1, b: 2 }).map(([k, v]) => [k, v * 2]),
); // { a: 2, b: 4 }

console.log(Object.is(NaN, NaN)); // true
```

**Thoughts:** Unlike `for...in`, these methods skip inherited properties. Syntax is `Object.keys(obj)`, not `obj.keys()` (unlike `Map`).

**Sources:**

[javascript.info/keys-values-entries](https://javascript.info/keys-values-entries)

#### Object comparison

**Definition:**

`==` and `===` compare objects by reference — two objects are equal only if they are the same object in memory.

| Approach          | Use case                                   |
| ----------------- | ------------------------------------------ |
| `===` / `==`      | Same reference check                       |
| `Object.is(a, b)` | Strict compare with correct `NaN` handling |
| Custom function   | Compare specific fields (id, name, etc.)   |

**Examples:**

```js
const a = { x: 1 };
const b = { x: 1 };
const c = a;

console.log(a === b); // false — different objects
console.log(a === c); // true — same reference
```

**Sources:**

[javascript.info/object-copy](https://javascript.info/object-copy)

#### Global object

**Definition:** The global object holds variables and built-ins available everywhere. Use `globalThis` for cross-environment access (`window` in browsers, `global` in Node.js).

**When to use:** Rarely — prefer modules. In non-module scripts, `var` declarations become global properties.

**Examples:**

```js
console.log(globalThis === window); // true in browser
```

**Sources:** [javascript.info/global-object](https://javascript.info/global-object)

</details>

<details>
  <summary>Ways to create objects</summary>

### Ways to create objects

**Definition:** JavaScript offers multiple patterns for object creation, from literals to classes.

**How it works:**

- Literals `{}` and `new Object()` produce equivalent plain objects.
- `Object.create(proto)` sets `[[Prototype]]` without running a constructor.
- `new Fn()` runs constructor, links `[[Prototype]]` to `Fn.prototype`, returns instance.
- `class` desugars to constructor + prototype methods.

**When to use:**

Literals for DTOs; `Object.create` for inheritance without constructors; classes for domain models; factory functions for conditional creation.

#### Common pitfalls

Forgetting `new` on constructor pollutes global or returns `undefined`; `Object.assign` mutates target — use `{}` as first arg for copy.

| #   | Pattern                             | Notes                           |
| --- | ----------------------------------- | ------------------------------- |
| 1   | Object literal `{ }`                | Most common                     |
| 2   | `new Object()`                      | Equivalent to `{}`              |
| 3   | `Object.create(proto)`              | Explicit prototype              |
| 4   | Constructor function + `new`        | Pre-ES6 OOP                     |
| 5   | Constructor + `.prototype`          | Shared methods                  |
| 6   | ES6 `class`                         | Syntactic sugar over prototypes |
| 7   | Singleton IIFE                      | One instance only               |
| 8   | `Object.assign(target, ...sources)` | Copy properties to target       |

**Pluses and minuses:**

| +                                   | −                                                    |
| ----------------------------------- | ---------------------------------------------------- |
| Many patterns for different needs   | Inconsistent style across codebase if mixed          |
| `Object.create` for pure delegation | Constructors easy to forget `new` (classes fix this) |
| Factory functions avoid `new`       | Singleton IIFE harder to test                        |

**Examples:**

```js
// Literal
const literal = { name: "John" };

// Object.create
const proto = {
  greet() {
    return this.name;
  },
};
const created = Object.create(proto);
created.name = "Ann";

// Constructor
function Person(name) {
  this.name = name;
}
const p = new Person("Bob");

// Class
class Employee {
  constructor(role) {
    this.role = role;
  }
}
const e = new Employee("dev");

// Shallow copy
const copy = Object.assign({}, literal);
```

**Sources:** [javascript.info/object](https://javascript.info/object) · [javascript.info/object-copy](https://javascript.info/object-copy)

</details>

<details>
  <summary>Objects to primitives</summary>

### Objects to primitives

**Definition:** When an object is used in a primitive context (`+`, `==`, `alert`), JavaScript calls `Symbol.toPrimitive`, or falls back to `valueOf` / `toString` depending on the hint.

**How it works:**

- `Symbol.toPrimitive(hint)` runs first if defined on the object.
- Otherwise engine tries `valueOf` then `toString` (order swaps for `hint === 'string'`).
- In `if (obj)` any object is truthy regardless of content.

**When to use:**

Value objects (Money, Date-like); debugging with meaningful `toString`; avoid custom coercion unless necessary.

#### Common pitfalls

`{} + 1` → `"[object Object]1"`; relying on `valueOf` for string concat; `==` between object and primitive uses `default` hint.

| Hint      | Conversion order       |
| --------- | ---------------------- |
| `number`  | `valueOf` → `toString` |
| `string`  | `toString` → `valueOf` |
| `default` | `valueOf` → `toString` |

**Pluses and minuses:**

| +                                      | −                                             |
| -------------------------------------- | --------------------------------------------- |
| Custom coercion for value objects      | Easy to create surprising `+` / `==` behavior |
| `Symbol.toPrimitive` centralizes logic | Default `toString` often `[object Object]`    |

**Examples:**

```js
const obj = {
  value: 42,
  [Symbol.toPrimitive](hint) {
    if (hint === "number") return this.value;
    return `value: ${this.value}`;
  },
};

console.log(+obj); // 42
console.log(`${obj}`); // 'value: 42'
console.log(obj + ""); // 'value: 42' (hint: default)
```

**Thoughts:**

In boolean context, any object is `true`. `alert(obj)` coerces to string.

`obj1 + obj2` converts both with `hint: 'default'`. `==` uses `hint: 'default'` for objects.

**Sources:** [javascript.info/object-toprimitive](https://javascript.info/object-toprimitive)

</details>

<details>
  <summary>Objects & references</summary>

### Objects & references

**Definition:** Primitives are copied by value; objects are copied by reference — the variable holds a pointer to shared memory, not the object itself.

**When to use:**

Understand before `Object.assign`, spread, or passing config objects.

When you need an independent copy — use spread/`Object.assign` (shallow) or `structuredClone` (deep).

**Pluses and minuses:**

| + (reference semantics)               | −                                |
| ------------------------------------- | -------------------------------- |
| Efficient passing of large structures | Mutations visible across aliases |
| Shared state when intended            | Accidental mutation bugs         |

**Examples:**

```js
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x); // 2 — same object

const c = { ...a };
c.x = 3;
console.log(a.x); // 2 — shallow copy, top-level independent
```

**Sources:** [javascript.info/object-copy](https://javascript.info/object-copy)

</details>

---

<a name="this-binding"></a>

## `this`

### `this`

<details>
  <summary>`this` binding — four rules</summary>

### `this` binding — four rules

**Definition:** `this` is determined by **how** a function is called, not where it is defined (except arrow functions, which inherit `this` lexically).

**How it works:**

- Engine evaluates call site: `new` → bound object; `call`/`apply`/`bind` → explicit arg; `obj.method()` → `obj`; bare call → default binding.
- Arrow functions ignore these rules — `this` is captured from enclosing lexical scope at creation time.
- `bind` creates a new function with permanently fixed `this` (and optional partial args).

**When to use:**

Class methods as event handlers → arrow class fields or `bind`; borrowing methods → `fn.call(ctx)`; constructors → `new`.

Debug class methods passed as callbacks — bind or use arrow class fields.

#### Common pitfalls

`const g = obj.method; g()` loses receiver; nested object `obj.inner.method()` binds to `inner`, not `outer`; `setTimeout(obj.method)` without bind.

| Priority | Rule                               | `this` value                         |
| -------- | ---------------------------------- | ------------------------------------ |
| 1        | `new` call                         | newly created object                 |
| 2        | Explicit `call` / `apply` / `bind` | first argument                       |
| 3        | Method call `obj.method()`         | `obj`                                |
| 4        | Plain call / lost context          | `undefined` (strict) or `globalThis` |

**Pluses and minuses:**

| +                           | −                                                  |
| --------------------------- | -------------------------------------------------- |
| Explicit rules once learned | Easy to lose context in callbacks                  |
| `bind`/`call` fix context   | Arrow `this` surprises in class methods if misused |

**Examples:**

```js
const user = {
  name: "Alice",
  greet() {
    return this.name;
  },
};

const g = user.greet;
g(); // undefined (strict)

user.greet(); // 'Alice'
g.call(user); // 'Alice'
```

**Thoughts:** `this` follows the call site, except arrows which close over the outer `this`. Classes need `this` so `addRecord` updates _this_ instance when several exist — see [Constructor injection](#constructor-injection).

**Sources:** [javascript.info/bind](https://javascript.info/bind)

</details>

<details>
  <summary>`this` and call context</summary>

### `this` and call context

**Definition:** The call context (`this`) is the object a function was called on. It changes with each invocation based on call site, not definition site.

**How it works:**

- Each function invocation gets its own `this` binding determined at call time.
- Method syntax `obj.fn()` sets `this` to `obj`; extracted reference loses that link.
- Arrow functions close over outer `this` from lexical environment — no own binding.

**When to use:**

DOM handlers (often `this` = element); class instance methods; module-level arrows for stable `this` in React class components (legacy).

Understand before debugging lost context in callbacks, event handlers, and class methods.

#### Common pitfalls

`map(user.getName)` loses `user`; `this` in nested function inside method is `undefined` (strict) unless arrow or bind.

| Context              | `this` refers to                    |
| -------------------- | ----------------------------------- |
| Object method        | The object                          |
| Nested object method | The immediate object (not outer)    |
| Arrow function       | Lexical `this` from enclosing scope |
| Global (non-strict)  | `globalThis`                        |
| Global (strict)      | `undefined`                         |
| Constructor (`new`)  | Newly created instance              |

**Pluses and minuses:**

| +                                      | −                                               |
| -------------------------------------- | ----------------------------------------------- |
| Flexible receiver for shared functions | Context loss is a top JS bug source             |
| Explicit `call`/`bind` when needed     | Arrow in object literal does not bind to object |

**Examples:**

```js
const obj = {
  value: 42,
  getValue() {
    return this.value;
  },
  getValueArrow: () => this?.value, // lexical — not obj
};

const fn = obj.getValue;
fn(); // undefined (strict) — context lost

obj.getValue(); // 42
```

#### Lost context scenarios

| Scenario                          | Risk                          |
| --------------------------------- | ----------------------------- |
| Detached method `const f = obj.m` | `this` is not `obj`           |
| Callbacks (`setTimeout`, `map`)   | Context changes               |
| Event handlers                    | `this` may be the DOM element |
| Class methods as callbacks        | Need bind or arrow fields     |

**Sources:** [javascript.info/object-methods](https://javascript.info/object-methods) · [javascript.info/bind](https://javascript.info/bind)

</details>

<details>
  <summary>call, apply, bind</summary>

### call, apply, bind

**Definition:** `call`, `apply`, and `bind` explicitly set `this` for a function. `call` and `apply` invoke immediately; `bind` returns a new function.

**How it works:**

- All three use `Function.prototype`; first arg sets `this`, rest are call arguments.
- `apply` takes args as array-like; `call` spreads them.
- `bind` returns new function; bound `this` cannot be overridden by further `bind`/`call`.

**When to use:**

`Array.prototype.slice.call(arguments)` (legacy); method borrowing; fixing handler context; `Math.max.apply(null, arr)`.

`bind` for callbacks needing stable context; `call`/`apply` for immediate invocation with a specific `this`.

#### Common pitfalls

Forgetting bound functions cannot be used as constructors; assuming `bind` copies method — still shared on prototype.

| Method                  | Invokes now?          | Arguments                     |
| ----------------------- | --------------------- | ----------------------------- |
| `fn.call(ctx, a, b)`    | Yes                   | Comma-separated               |
| `fn.apply(ctx, [a, b])` | Yes                   | Array-like                    |
| `fn.bind(ctx, a)`       | No — returns bound fn | Partial application supported |

**Pluses and minuses:**

| +                                      | −                                                  |
| -------------------------------------- | -------------------------------------------------- |
| Borrow methods across objects          | `bind` creates new function each time (memory)     |
| Partial application via `bind`         | Easy to overuse instead of arrows/closures         |
| `apply` for spread-before-`...` legacy | `null`/`undefined` first arg → global (non-strict) |

**Examples:**

```js
function greet(greeting, punct) {
  return `${greeting}, ${this.name}${punct}`;
}

const user = { name: "Alice" };

greet.call(user, "Hi", "!"); // 'Hi, Alice!'
greet.apply(user, ["Hello", "."]); // 'Hello, Alice.'

const bound = greet.bind(user, "Hey");
bound("?"); // 'Hey, Alice?'
```

**Thoughts:** Passing `null`/`undefined` to `call`/`apply`/`bind` falls back to default binding. A function bound with `bind()` cannot be re-bound.

**Sources:** [javascript.info/bind](https://javascript.info/bind)

</details>

---

## Prototypes

### Prototypes

<details>
  <summary>Prototypal inheritance</summary>

### Prototypal inheritance

**Definition:** JavaScript shares behavior through **prototype-based inheritance**: each object has an internal `[[Prototype]]` link (`null` at the end). On **read**, the engine walks that chain until the key is found or the chain ends; **writes and deletes** touch only the object you started on. `class` / `extends` sit on top of the same model — see [Class vs prototype inheritance](#class-vs-prototype-inheritance) and [Classes](./3_js_classes.md).

**How it works:**

- **Inheritance** = link prototypes, not copy methods onto every instance.
- Link objects with `Object.setPrototypeOf`, `Object.create`, or (at creation) `new Constructor()` ([F.prototype](#f-prototype)); builtins use the same idea ([Native prototypes](#native-prototypes)).
- Shadowing: an own property hides an inherited one with the same key.
- Setting `Child.prototype = Parent.prototype` shares one prototype object — breaks typical `instanceof` / `super` patterns; use `Object.create(Parent.prototype)` instead.

**When to use:** Animal/user-style delegation; mixins via `Object.assign` on the prototype; behavior shared across many instances.

#### Common pitfalls

Modifying `__proto__` / `setPrototypeOf` in hot paths (deoptimization); assuming assignment updates the prototype parent.

| Rule              | Detail                                   |
| ----------------- | ---------------------------------------- |
| No circular links | Prototype loop throws                    |
| Type restriction  | `[[Prototype]]` must be object or `null` |
| Single parent     | Only one `[[Prototype]]` per object      |
| Read-only chain   | Writes never go up the prototype chain   |

**Pluses and minuses:**

| +                                 | −                                               |
| --------------------------------- | ----------------------------------------------- |
| Shared methods — memory efficient | Deeper chains = slower lookup                   |
| `Object.create` for delegation    | Prefer `getPrototypeOf` over legacy `__proto__` |

**Examples:**

```js
const animal = { eats: true };
const rabbit = { jumps: true };

Object.setPrototypeOf(rabbit, animal);

console.log(rabbit.eats); // true — found on animal
console.log(rabbit.jumps); // true — own property
```

**Thoughts:** Prototypes are for **reading** only. Write/delete operations apply to the object itself.

| Term             | Meaning                                   |
| ---------------- | ----------------------------------------- |
| `[[Prototype]]`  | Internal slot — link to parent object     |
| `__proto__`      | Legacy accessor on `Object.prototype`     |
| `.prototype`     | Property on **functions** — used by `new` |
| Prototype object | The object referenced by `[[Prototype]]`  |

**Sources:** [javascript.info/prototypes](https://javascript.info/prototypes) · [javascript.info/prototype-inheritance](https://javascript.info/prototype-inheritance) · [MDN — Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)

</details>

<details>
  <summary>F.prototype</summary>

### F.prototype

**Definition:** Every function (except arrows) may have an own `prototype` property. When the function is called with **`new`**, the created object's `[[Prototype]]` becomes `F.prototype` (if it is an object).

**How it works:**

1. Create a new object.
2. Set `[[Prototype]]` to `Constructor.prototype`.
3. Call the constructor with `this` bound to that object.
4. Return the object (unless the constructor returns a different object).

- Instance methods live on `Constructor.prototype`; per-instance data is assigned in the constructor (`this.name = …`).
- `F.prototype` is unrelated to `F.[[Prototype]]` (the function object's own inheritance).

**When to use:** Constructor functions and pre-ES6 patterns; understanding how `class` desugars to `prototype` + `new`.

**Examples:**

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {
  return `Hi, ${this.name}`;
};

const p = new Person("Ann");
console.log(p.sayHi()); // 'Hi, Ann'
console.log(Object.getPrototypeOf(p) === Person.prototype); // true
```

**Sources:** [javascript.info/function-prototype](https://javascript.info/function-prototype) · [Classes](./3_js_classes.md)

</details>

<details>
  <summary>Native prototypes</summary>

### Native prototypes

**Definition:** Built-in constructors (`Object`, `Array`, `Function`, `Date`, …) store shared methods on their `.prototype`. Primitives temporarily use wrapper objects that delegate to `String.prototype`, `Number.prototype`, etc.

**How it works:**

- `{}` / `new Object()` inherit from `Object.prototype` (`toString`, `hasOwnProperty`, …).
- `[1, 2, 3]` delegates array methods to `Array.prototype`.
- Only **polyfills** should mutate native `.prototype` in shared code.

| Object stores                  | Prototype stores                   |
| ------------------------------ | ---------------------------------- |
| Data (array items, properties) | Methods (`push`, `toString`, etc.) |

**When to use:** Understanding where `map` / `toString` come from; safe polyfills; avoiding pollution of `Object.prototype`.

**Examples:**

```js
const arr = [1, 2, 3];
console.log(arr.push === Array.prototype.push); // true

const obj = {};
console.log(obj.toString()); // '[object Object]'
```

**Thoughts:** `{ }` is shorthand for `new Object()` — inheriting from `Object.prototype`.

**Sources:** [javascript.info/native-prototypes](https://javascript.info/native-prototypes)

</details>

<details>
  <summary>Prototype methods, objects without __proto__</summary>

### Prototype methods, objects without **proto**

**Definition:** Standard APIs to create objects with a given prototype and to read or write `[[Prototype]]` without relying on the legacy `__proto__` accessor. `Object.create(null)` builds **objects without a prototype** — plain dictionaries safe for arbitrary keys.

**How it works:**

- `Object.create(proto, descriptors?)` — new object with `[[Prototype]]` = `proto`; optional second argument defines own properties.
- `Object.getPrototypeOf(obj)` / `Object.setPrototypeOf(obj, proto)` — read/write the link (`setPrototypeOf` is slow — avoid in hot loops).
- Prefer these over Annex B `__proto__` getter/setter; literal `{ __proto__: proto }` is still valid at object creation time.
- `Object.create(null)` — no inherited `toString` / `hasOwnProperty` / `__proto__` — mitigates `__proto__` key pollution from user input.
- **Borrowing:** assign `Array.prototype.join` (or call with `.call`) so array-like objects can use native methods.

**When to use:** `Object.create(null)` for map-like dicts; prototype-based inheritance without constructors; test doubles with a custom chain.

#### Common pitfalls

Using `setPrototypeOf` on every instance in a constructor; confusing `Object.create(Fn.prototype)` with `new Fn()`.

| Method                               | Purpose                                           |
| ------------------------------------ | ------------------------------------------------- |
| `Object.create(proto, descriptors?)` | New object with `[[Prototype]]` = `proto`         |
| `Object.getPrototypeOf(obj)`         | Read `[[Prototype]]`                              |
| `Object.setPrototypeOf(obj, proto)`  | Write `[[Prototype]]` (slow — avoid in hot paths) |

**Pluses and minuses:**

| +                                              | −                                              |
| ---------------------------------------------- | ---------------------------------------------- |
| Clean delegation without constructors          | `setPrototypeOf` hurts performance             |
| `Object.create(null)` — no inherited pollution | Descriptors in second arg verbose              |
| Explicit prototype in tests/mocks              | Changing proto breaks `instanceof` assumptions |

**Examples:**

```js
const animal = {
  eats: true,
  walk() {
    console.log("walking");
  },
};

const rabbit = Object.create(animal, {
  jumps: { value: true, enumerable: true },
});

rabbit.walk(); // 'walking' — inherited

const pseudo = { 0: "Hello", 1: "world!", length: 2 };
pseudo.join = Array.prototype.join;
console.log(pseudo.join(",")); // 'Hello,world!'

const dict = Object.create(null);
dict["__proto__"] = "not a problem";
```

**Sources:** [javascript.info/prototype-methods](https://javascript.info/prototype-methods)

</details>

<details>
  <summary>`instanceof`</summary>

### `instanceof`

**Definition:** `obj instanceof Constructor` walks `obj.__proto__` and checks if any link equals `Constructor.prototype`.

**How it works:**

- Walk `obj.[[Prototype]]` chain; return `true` if any link `=== Constructor.prototype`.
- `Symbol.hasInstance` on constructor can customize behavior.
- `null` prototype objects only `instanceof Object` if chain reaches `Object.prototype`.

**When to use:**

Type checks in polymorphic code; verifying API response shapes; custom `@@hasInstance` for branded types.

#### Common pitfalls

`instanceof` with arrow functions (no `.prototype`); checking plain object literals — always `instanceof Object`; cross-frame `Array` checks fail.

**Pluses and minuses:**

| +                        | −                                    |
| ------------------------ | ------------------------------------ |
| Works across inheritance | Breaks if `prototype` is changed     |
| Checks prototype chain   | Not reliable across realms (iframes) |

| +                          | −                                                  |
| -------------------------- | -------------------------------------------------- |
| Works with subclass chains | Fails across iframes/realms (different prototypes) |
| Duck typing alternative    | Breaks if `prototype` replaced                     |
| Familiar OOP check         | Not for primitive values                           |

**Examples:**

```js
class Animal {}
class Rabbit extends Animal {}

const r = new Rabbit();
console.log(r instanceof Rabbit); // true
console.log(r instanceof Animal); // true — chain includes Animal.prototype
console.log(r instanceof Object); // true
```

**Sources:** [javascript.info/instanceof](https://javascript.info/instanceof) · [Classes — Class checking: `instanceof`](./3_js_classes.md#class-checking-instanceof)

</details>

<details>
  <summary>Extending built-in classes</summary>

### Extending built-in classes

**Definition:** ES6 allows `class MyArray extends Array` to inherit built-in behavior while adding methods.

**How it works:**

- `class MyArr extends Array` creates subclass whose instances delegate to `Array` internal slots when constructed properly.
- Built-in methods (`map`, `filter`) use `this.constructor[Symbol.species]` (or `constructor`) to build the **result** object — so `filteredArr instanceof PowerArray` stays true unless you override species.
- `static get [Symbol.species]() { return Array; }` makes `filter`/`map` return plain `Array`.
- Built-ins do **not** inherit each other’s **static** methods (`Array.keys` ≠ `Object.keys` via `extends` chain the way user classes do).
- `super` in derived class must be called before `this` use in constructor.
- Some builtins (e.g. certain DOM types) cannot be subclassed reliably.

**When to use:**

`class ValidationError extends Error`; `class TypedArray extends Array` for typed collections; framework element subclasses.

Custom `Error` subclasses, extended `Array`/`Map` with domain methods.

Modifying `Array.prototype` globally (polyfill only).

#### Common pitfalls

Forgetting `super()` in derived constructor; assuming `JSON.stringify` handles custom classes; mutating built-in `.prototype` globally.

**Pluses and minuses:**

| +                     | −                                           |
| --------------------- | ------------------------------------------- |
| Reuse native behavior | Easy to break internal slots if misused     |
| Custom error types    | Some builtins cannot be subclassed reliably |

| +                                | −                                             |
| -------------------------------- | --------------------------------------------- |
| Domain-specific array/map types  | Wrong subclassing breaks `length` / iterators |
| Custom `Error` with stack/status | Engine-specific edge cases                    |
| Native methods still work        | `Array.from` on subclass needs care           |

**Examples:**

```js
class HttpError extends Error {
  constructor(status, message) {
    super(message);
    this.name = "HttpError";
    this.status = status;
  }
}

class PowerArray extends Array {
  isEmpty() {
    return this.length === 0;
  }
  // built-ins like filter return Array, not PowerArray
  static get [Symbol.species]() {
    return Array;
  }
}

const arr = new PowerArray(1, 2, 10);
arr.filter((n) => n >= 10); // Array — no .isEmpty()
```

**Sources:** [javascript.info/extend-natives](https://javascript.info/extend-natives) · [Classes — Extending built-in classes](./3_js_classes.md#extending-built-in-classes)

</details>

---

<a name="object-properties-configuration"></a>

## Object property configuration

### Object property configuration

<details>
  <summary>Object properties configuration</summary>

### Object properties configuration

**Definition:** Beyond individual property descriptors, `Object` provides **object-level** controls: whether new keys can be added and whether the whole shape or values become immutable.

**How it works:**

- `Object.preventExtensions(obj)` — cannot add new properties; existing props still writable/deletable.
- `Object.seal(obj)` — `preventExtensions` + all existing properties `configurable: false` (cannot delete or reconfigure); values still writable unless already read-only.
- `Object.freeze(obj)` — `seal` + all data properties `writable: false`; strict mode assignment throws.
- Check state: `Object.isExtensible`, `Object.isSealed`, `Object.isFrozen`.
- All three are **shallow** — nested objects remain mutable unless recursively frozen.

| Method                | Add | Delete | Write values | Reconfigure descriptors |
| --------------------- | --- | ------ | ------------ | ----------------------- |
| `preventExtensions()` | No  | Yes    | Yes          | Yes                     |
| `seal()`              | No  | No     | Yes          | No                      |
| `freeze()`            | No  | No     | No           | No                      |

**When to use:** `freeze` for exported config constants; `seal` when shape must not change; `preventExtensions` before passing objects to untrusted code.

**Pluses and minuses:**

| +                                 | −                                    |
| --------------------------------- | ------------------------------------ |
| Simple runtime immutability       | Shallow only                         |
| `freeze` for config constants     | Cannot unfreeze                      |
| No library needed for basic cases | Deep freeze needs recursion or Immer |

**Examples:**

```js
const sealed = Object.seal({ a: 1, nested: { b: 2 } });
sealed.a = 2; // OK
sealed.c = 3; // fails silently / strict throw
delete sealed.a; // fails
sealed.nested.b = 99; // nested still mutable!

const frozen = Object.freeze({ x: 1 });
frozen.x = 2; // fails
console.log(Object.isFrozen(frozen)); // true
```

**Sources:** [javascript.info/property-descriptors](https://javascript.info/property-descriptors) · [MDN Object.freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

</details>

<details>
  <summary>Property flags and descriptors</summary>

### Property flags and descriptors

**Definition:** Every own property has a **descriptor** — metadata with **flags** `writable`, `enumerable`, and `configurable`, plus either stored `value` (data descriptor) or accessor functions (see [Property getters and setters](#property-getters-and-setters)).

**How it works:**

- Assignment `obj.key = v` uses default flags (`writable: true`, `enumerable: true`, `configurable: true` for own props).
- `Object.defineProperty` / `defineProperties` set flags explicitly; `Object.getOwnPropertyDescriptor` reads them.
- `enumerable: false` hides a key from `for...in` and `Object.keys` (still visible via `Object.getOwnPropertyNames` / `Reflect.ownKeys`).
- `configurable: false` prevents delete and redefinition; `writable: false` blocks value changes on data properties.

| Flag / field   | Type            | Controls                             |
| -------------- | --------------- | ------------------------------------ |
| `value`        | Data            | Stored value                         |
| `writable`     | Data            | Can value be changed?                |
| `enumerable`   | Data / Accessor | Shows in `for...in` / `Object.keys`? |
| `configurable` | Data / Accessor | Can delete or redefine?              |

**When to use:** Read-only `id`; non-enumerable internal slots; library polyfills that must not be deleted.

**Pluses and minuses:**

| +                                         | −                                                  |
| ----------------------------------------- | -------------------------------------------------- |
| Fine-grained control (immutable id)       | Verbose API                                        |
| Hide internal props (`enumerable: false`) | Easy to forget `configurable: false` for true lock |

**Examples:**

```js
const obj = {};

Object.defineProperty(obj, "id", {
  value: 1,
  writable: false,
  enumerable: true,
  configurable: false,
});

obj.id = 2;
console.log(obj.id); // 1 — not writable
```

**Sources:** [javascript.info/property-descriptors](https://javascript.info/property-descriptors) · [MDN defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

</details>

<details>
  <summary>Property getters and setters</summary>

### Property getters and setters

**Definition:** **Accessor descriptors** use `get` and/or `set` functions instead of a stored `value`. They can validate, compute, or hide backing fields while keeping a property-like API.

**How it works:**

- Object literal: `get name() {}` / `set name(v) {}` (ES5 shorthand).
- `Object.defineProperty(obj, "prop", { get() {}, set(v) {}, enumerable: true, configurable: true })`.
- A property is either a **data** descriptor or an **accessor** descriptor — not both on the same key.
- Class syntax: [Getters and setters](./3_js_classes.md#getters-and-setters).

**When to use:** Computed read-only fields; validation on write; exposing a view without leaking internal storage.

**Pluses and minuses:**

| +                              | −                                         |
| ------------------------------ | ----------------------------------------- |
| Encapsulation without closures | `get` without `set` → read-only from API  |
| Lazy / derived values          | `get` calling itself → infinite recursion |

**Examples:**

```js
const user = {
  first: "Ann",
  last: "Lee",
  get fullName() {
    return `${this.first} ${this.last}`;
  },
};

let _balance = 0;
const account = {};
Object.defineProperty(account, "balance", {
  get() {
    return _balance;
  },
  set(value) {
    if (value < 0) throw new RangeError("negative balance");
    _balance = value;
  },
});
```

**Sources:** [javascript.info/property-accessors](https://javascript.info/property-accessors) · [Classes — Getters and setters](./3_js_classes.md#getters-and-setters)

</details>

---

## Advanced topics

### Advanced topics

<details>

<details>
  <summary>Shallow vs deep copy</summary>

### Shallow vs deep copy

**Definition:** A shallow copy duplicates top-level properties; nested objects remain shared. A deep copy recursively clones all levels.

**How it works:**

- Shallow: new object, but nested object references copied as pointers — shared nested state.
- Deep: recursively clone reachable own properties; `structuredClone` uses structured clone algorithm.
- `JSON` round-trip drops non-JSON types and breaks on cycles.

| Type                            | `JSON.stringify` behavior |
| ------------------------------- | ------------------------- |
| `undefined`, functions, symbols | Removed                   |
| `BigInt`                        | Throws                    |
| `NaN`, `Infinity`               | `null`                    |
| Circular references             | Throws                    |
| `Date`                          | ISO string                |
| `Map`, `Set`                    | `{}`                      |

**When to use:**

Immutable updates (shallow + replace nested); API payload clone; state snapshot before mutation.

#### Common pitfalls

`{...obj}` then mutating `copy.nested` affects original; `structuredClone` on DOM nodes throws; JSON clone turns `Date` to string.

| Strategy                       | Nested objects   | Methods / symbols                              |
| ------------------------------ | ---------------- | ---------------------------------------------- |
| Spread / `Object.assign`       | Shared (shallow) | Copied (enumerable own)                        |
| `structuredClone()`            | Cloned (deep)    | Most types; no functions                       |
| `JSON.parse(JSON.stringify())` | Cloned           | Loses functions, `undefined`, `Date` precision |

**Pluses and minuses:**

| +                                    | −                                        |
| ------------------------------------ | ---------------------------------------- |
| Spread/`assign` fast for flat clones | Shallow copies hide nested sharing bugs  |
| `structuredClone` native deep clone  | No functions, symbols in clone           |
| JSON clone simple for plain data     | JSON loses `Date`, `undefined`, `BigInt` |

**Examples:**

```js
const original = { a: 1, nested: { b: 2 } };

const shallow = { ...original };
shallow.nested.b = 99;
console.log(original.nested.b); // 99 — shared

const deep = structuredClone(original);
deep.nested.b = 0;
console.log(original.nested.b); // 99 — independent
```

**Sources:** [javascript.info/object-copy](https://javascript.info/object-copy) · [MDN structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)

</details>

<details>
  <summary>`in` operator and `for...in`</summary>

### `in` operator and `for...in`

**Definition:** `for...in` iterates all **enumerable** properties, including inherited ones. Use `Object.keys` or `hasOwnProperty` for own properties only.

**How it works:**

- `key in obj` checks entire prototype chain for property existence (own or inherited). Prefer `in` when a property may exist with value `undefined` — `obj.x === undefined` cannot tell “missing” from “set to undefined”.
- `for...in` enumerates **enumerable** keys on object and prototypes.
- **Integer-like keys** (`"49"`, `"1"`) are sorted ascending in enumeration; other keys keep creation order. Prefix with `"+"` (or similar) if you need insertion order for numeric-looking codes.
- `Object.keys` / `hasOwn` / `Object.hasOwn` restrict to own properties.

It is inherited from `Object.prototype` with `{ enumerable: false }`.

**When to use:**

`'length' in arr`; prototype polling; own-key iteration → `Object.keys` or `for...of` on entries.

#### Common pitfalls

`for...in` on arrays iterates indices + maybe inherited; object without `hasOwn` check lists prototype keys; `obj.hasOwnProperty` fails if null prototype without `Object.hasOwn`.

**Pluses and minuses:**

| +                                | −                                                 |
| -------------------------------- | ------------------------------------------------- |
| `in` detects inherited methods   | `for...in` pulls inherited enumerable props       |
| `for...in` simple iteration      | Must filter with `hasOwn`; integer keys reordered |
| `Object.hasOwn` modern own check | `hasOwnProperty` can be shadowed                  |

**Examples:**

```js
const obj = { a: 1, b: 2, test: undefined };
"test" in obj; // true — property exists
obj.test === undefined; // true — but also true if missing

const codes = { 49: "DE", 41: "CH", 1: "US" };
for (const code in codes) console.log(code); // 1, 41, 49 — integer sort

for (const key in obj) {
  if (Object.hasOwn(obj, key)) {
    console.log(key, obj[key]);
  }
}
```

**Sources:** [javascript.info/object](https://javascript.info/object) · [javascript.info/keys-values-entries](https://javascript.info/keys-values-entries)

</details>

<details>
  <summary>Garbage collection</summary>

### Garbage collection

**Definition:** JavaScript automatically frees memory for values that are no longer **reachable** from roots (global variables, current call stack, etc.).

**How it works:**

- Mark-and-sweep: start from roots (global, stack, registers); mark reachable; sweep unmarked.
- Generational GC assumes most objects die young — nursery collected more often.
- Closures keep outer variables alive while inner function is reachable.

**When to use:**

Drop references (`obj = null`) when done; avoid global caches without bounds; `WeakMap` for metadata on DOM nodes.

#### Common pitfalls

Closures holding large outer scope; detached DOM nodes still referenced by JS; `setInterval` holding callback forever; forgetting event listener removal.

| Phase     | Action                                      |
| --------- | ------------------------------------------- |
| **Mark**  | Traverse from roots; mark reachable objects |
| **Sweep** | Delete unmarked (unreachable) objects       |

**Pluses and minuses:**

| +                           | −                                    |
| --------------------------- | ------------------------------------ |
| Automatic memory management | Pauses (mitigated by incremental GC) |
| No manual free()            | Memory leaks still possible          |
| WeakRef/WeakMap for caches  | Unpredictable collection timing      |

**Examples:**

```js
let user = { name: "John" };
// user is reachable — object stays in memory

user = null;
// { name: 'John' } is unreachable — GC will reclaim it
```

#### GC optimizations

| Technique               | Purpose                            |
| ----------------------- | ---------------------------------- |
| Generational collection | Young objects collected more often |
| Incremental collection  | Spread GC across small steps       |
| Idle-time collection    | Run GC when CPU is idle            |

**Thoughts:** Outgoing references do not keep an object alive — only **incoming** references from reachable objects matter. An “island” of objects that only reference each other is collected once no root reaches it.

**Sources:** [javascript.info/garbage-collection](https://javascript.info/garbage-collection)

</details>

<details>
  <summary>WeakRef and FinalizationRegistry</summary>

### WeakRef and FinalizationRegistry

**Definition:** `WeakRef` holds a weak reference without preventing GC; `FinalizationRegistry` runs a callback after an object is collected.

**How it works:**

- `WeakRef` wraps object weakly — `deref()` returns object or `undefined` if collected.
- `FinalizationRegistry` registers cleanup callback when target is GC'd — timing not guaranteed.
- Do not rely on finalizers for critical cleanup — use explicit `dispose()`.

**When to use:**

Image/asset cache; native handle wrappers; memory profiling.

Large-object caches, native addon wrappers, advanced memory management.

Normal app logic — use `WeakMap` for metadata keyed by objects.

#### Common pitfalls

Using WeakRef as only lifecycle management; assuming finalizer runs promptly; strong references in finalizer callback preventing GC.

**Pluses and minuses:**

| +                             | −                                |
| ----------------------------- | -------------------------------- |
| Cache without memory leaks    | Non-deterministic cleanup timing |
| External metadata for objects | Overkill for most apps           |

| +                                   | −                                      |
| ----------------------------------- | -------------------------------------- |
| Cache large objects without leaking | `deref()` may return undefined anytime |
| Observe GC for diagnostics          | Finalization order non-deterministic   |
| Complements `WeakMap`               | Overkill for typical apps              |

**Examples:**

```js
const registry = new FinalizationRegistry((heldValue) => {
  console.log(`Object with id ${heldValue} was garbage-collected`);
});

let obj = { data: "large" };
const ref = new WeakRef(obj);
registry.register(obj, "session-1");
obj = null; // eventually GC + callback
```

**Sources:** [javascript.info/weakref-finalizationregistry](https://javascript.info/weakref-finalizationregistry)

</details>

---

<details>
  <summary>Proxy and Reflect</summary>

### Proxy and Reflect

**Definition:** `Proxy` wraps a target object and intercepts operations via handler traps; `Reflect` provides methods that mirror those operations with sensible return values for use inside traps.

**How it works:**

- `new Proxy(target, handler)` — reads/writes/deletes route through traps (`get`, `set`, `has`, `deleteProperty`, `apply`, `construct`, …).
- Trap must return expected type (e.g. `set` → boolean) or throw; otherwise runtime errors.
- `Reflect.get(target, prop, receiver)` forwards to target with correct `this` for getters.
- Default: no trap → operation forwarded to target unchanged.

**When to use:**

Input validation; observable state; negative array index wrapper; mocking in tests; `Proxy.revocable` for capability security.

#### Common pitfalls

Forgetting `return true` from `set` trap (strict mode error); proxying arrays — some methods use internal slots; deep reactivity needs nested proxies; `JSON.stringify` on proxy serializes target.

**Pluses and minuses:**

| +                                       | −                                       |
| --------------------------------------- | --------------------------------------- |
| Validation, logging, reactivity (Vue 3) | Slight overhead; mental overhead        |
| Virtual properties without storage      | Hard to debug stack traces              |
| Revocable proxies (`Proxy.revocable`)   | Cannot proxy all internal slots equally |

**Examples:**

```js
const user = new Proxy(
  {},
  {
    set(target, prop, value) {
      if (prop === "age" && (typeof value !== "number" || value < 0)) {
        throw new RangeError("age must be non-negative number");
      }
      return Reflect.set(target, prop, value);
    },
    get(target, prop, receiver) {
      if (prop === "fullName") {
        return `${target.first ?? ""} ${target.last ?? ""}`.trim();
      }
      return Reflect.get(target, prop, receiver);
    },
  },
);

user.first = "Ann";
user.last = "Lee";
user.age = 30;
console.log(user.fullName); // 'Ann Lee'
```

**Sources:** [javascript.info/proxy](https://javascript.info/proxy) · [MDN Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

</details>

<details>
  <summary>Factory and module patterns</summary>

### Factory and module patterns

**Definition:** A **factory** returns new objects (or functions) without requiring `new`; a **module pattern** uses closures/IIFE or ES modules to hide private state and expose a public API.

**How it works:**

- Factory function: `function createSpending(amount) { return { amount, dateTime: Date.now() }; }` — no `prototype` required; one place for defaults so you don't copy-paste literals.
- Module pattern (IIFE): `(function () { let secret = 0; return { inc() { secret++; } }; })()` — private `secret`.
- ES modules: `export` public API; top-level scope is module-private by default.
- Grow from factory → `class` when you need `this` (several instances) or methods that must stay on the entity (`addRecord`).

**When to use:**

Factory when creation logic branches; module for singleton config, counters, private caches; ES modules for all new app code.

#### Common pitfalls

Factory returning new function each call breaks `===` identity checks; IIFE module state is singleton — test pollution; confusing factory with constructor (`new` not needed).

**Pluses and minuses:**

| +                                          | −                                        |
| ------------------------------------------ | ---------------------------------------- |
| Factory flexible — plain objects, no `new` | No `instanceof` without shared prototype |
| Module hides implementation                | IIFE modules harder to tree-shake        |
| ES modules standard, static analysis       | Over-factorying simple literals          |

**Examples:**

```js
// Factory — same-shaped objects without `new`
function createSpending(amount) {
  return { amount, dateTime: Date.now(), kind: "spending" };
}

function createCounter(start = 0) {
  let n = start;
  return {
    next() {
      return ++n;
    },
    reset() {
      n = start;
    },
  };
}

// Module pattern (legacy IIFE)
const authModule = (() => {
  let token = null;
  return {
    login(t) {
      token = t;
    },
    logout() {
      token = null;
    },
    isLoggedIn() {
      return token !== null;
    },
  };
})();

// ES module — auth.js
// let token = null;
// export function login(t) { token = t; }
```

**Sources:** [javascript.info/modules-intro](https://javascript.info/modules-intro) · [MDN modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) · [Doka — OOP](https://doka.guide/tools/oop/)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
