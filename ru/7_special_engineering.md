# OOP & SOLID

OOP pillars and the five SOLID principles for modules and APIs.

[← Back to hub](./0_system_design.md)

### Related docs

- [System design — Core](./0_system_design_core.md) · [System design — Architecture](./0_system_design_architecture.md) · [Architecture patterns](./7_special_architecture.md) · [System design — Data](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Design patterns](./7_special_patterns.md) · [System design — Interview](./0_system_design_interview.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md)

### Sections

- [9. OOP & SOLID](#9-oop-and-solid)

## 9. OOP & SOLID

<details>
  <summary>Object-Oriented Programming (OOP)</summary>

### Object-Oriented Programming (OOP)

**Definition:** ООП — парадигма программирования, которая моделирует программы как **objects** — живые экземпляры, построенные из **class** (чертёж). Каждый object объединяет **state** (поля) и **behavior** (методы). ООП базируется на четырех ключевых концепциях — инкапсуляция, наследование, полиморфизм, абстракция. В JavaScript `class` — синтаксический сахар над прототипами — см. [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**How it works:**

| Step        | Idea                                                              |
| ----------- | ----------------------------------------------------------------- |
| **Class**   | Чертёж — поля и методы задаются один раз                          |
| **Object**  | Экземпляр — `new Animal("Шарик")` создаёт собственную копию state |
| **Message** | Вызов метода — `dog.speak()`; object сам решает, как ответить     |

Четыре **pillars** объясняют, как objects сотрудничают:

| Pillar            | What it means                                                                                        |
| ----------------- | ---------------------------------------------------------------------------------------------------- |
| **Encapsulation** | Сокрытие внутренней логики и данных объекта от прямого доступа извне                                 |
| **Inheritance**   | Дочерний class на основе родительского — переиспользование кода без дублирования                     |
| **Polymorphism**  | Объекты с разной структурой используют одноимённые методы; поведение зависит от контекста            |
| **Abstraction**   | Выделение главного, сокрытие реализации — например, `video.play()` без знания о декодировании потока |

Развёрнутые блоки: [Encapsulation](#encapsulation) · [Inheritance](#inheritance) · [Polymorphism](#polymorphism) · [Abstraction](#abstraction).

Код на JS (`#`, `extends`/`super()`, полиморфизм `speak()`, `video.play()`): [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**Composition vs inheritance:** умный чайник с таймером — это **has-a** (`this.kettle`, `this.timer`), а не **is-a** чайник _и_ таймер. В JavaScript только один `extends` — когда «оба родителя» нужны, собирайте objects через composition.

**When to use:** Богатые domain models (users, orders, ledgers), деревья UI-компонентов, SDK с общими базовыми типами. Для одноразовых скриптов и в основном data pipelines — functions/modules.

**Pluses and minuses:**

| +                                                   | −                                                                                  |
| --------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Сущности и связи ложатся на язык предметной области | Общий mutable state и много писателей → гонки (нужны locks, CRDT или immutability) |
| UML, ER, DFD выросли вокруг object-мышления         | Плоские «is-a» деревья редко описывают «has both»                                  |
| Изученные паттерны и типичные ошибки                | Глубокие иерархии трудно менять (fragile base class)                               |

**Thoughts:** Эта страница — **что** такое OOP и **когда** применять; синтаксис JS и примеры столпов — в [OOP in JavaScript](./3_js_objects.md#oop-in-javascript). Encapsulation не лечит concurrent edits (два пользователя, один doc) — нужны OT/CRDT или locks. Хороший код — **карта территории**; OOP помогает, когда entities и messages совпадают с этой картой.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [Doka — programming paradigms](https://doka.guide/tools/programming-paradigms/) · [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript) · [Classes (TypeScript)](./5_ts_classes.md)

---

</details>

<details>
  <summary>Encapsulation</summary>

### Encapsulation

**Definition:** Сокрытие внутренней логики и данных объекта от прямого доступа извне. Снаружи — небольшой API; state меняется только через методы с правилами.

**How it works:**

- **На уровне object:** данные и единственные допустимые способы их менять живут внутри object — вызывают `deposit()`, а не `account.balance = -100`.
- **На уровне module:** class `History` прячет хранение записей; снаружи `addRecord()`, а не `history.records.push(...)`. Смена storage без переписывания каждого caller.
- **В JS:** `#` создаёт полностью приватные поля; замыкания и ES-модули тоже скрывают детали. См. [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** Domain objects с инвариантами (balance, permissions, cart totals). Любой type с несколькими экземплярами — методы вместо публичных arrays/maps.

**Pluses and minuses:**

| +                                     | −                                          |
| ------------------------------------- | ------------------------------------------ |
| Одно место для правил и storage       | Слишком private API мешает тестам          |
| Callers стабильны при смене internals | Getters/setters могут просачивать модель   |
| Защита от случайной мутации           | Легко обернуть каждое поле и назвать «OOP» |

**Examples:**

```js
// bad: публичное поле — caller обходит validation
class BankAccount {
  balance = 0;
}
const account = new BankAccount();
account.balance = -100; // без правил, invariant сломан

// good: private state; меняют только методы
class BankAccount {
  #balance = 0;

  constructor(owner) {
    this.owner = owner;
  }

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount("Иван");
account.deposit(100);
console.log(account.getBalance()); // 100
// account.#balance → SyntaxError
```

**Thoughts:** `public` поля в TS/JS — соглашение, пока не `#` или замыкания. Chrome DevTools читает `#` в консоли для отладки — это **не** доступ из app-кода.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Inheritance</summary>

### Inheritance

**Definition:** Подкласс получает свойства и методы родителя (суперкласса) — переиспользование кода и специализация. Позволяет создать дочерний class на основе родительского без дублирования общего кода.

**How it works:**

- Наследование — это **is-a**: каждый `Manager` _есть_ `Employee`.
- Если «и чайник, и таймер» — лучше **has-a** (композиция), а не два родителя.
- В JavaScript: `extends` связывает прототипы; `super()` вызывает конструктор или метод родителя. См. [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** Настоящие связи **is-a** со стабильным базовым поведением. Предпочитайте плоские иерархии (≤2 уровня).

**Pluses and minuses:**

| +                                 | −                                                      |
| --------------------------------- | ------------------------------------------------------ |
| DRY для общего поведения          | Жёсткая связь с реализацией родителя                   |
| Естественное моделирование «is-a» | Нарушение LSP, когда подкласс ломает контракт родителя |

**Examples:**

```js
// bad: дублируем name — меняем constructor Employee → правим Manager вручную
class Employee {
  constructor(name) {
    this.name = name;
  }
  getRole() {
    return "employee";
  }
}
class Manager {
  constructor(name) {
    this.name = name;
  } // копия
  getRole() {
    return "manager";
  }
}

// good: общий код в parent; child вызывает super() и добавляет только отличия
class Employee {
  constructor(name) {
    this.name = name;
  }
  getRole() {
    return "employee";
  }
}
class Manager extends Employee {
  constructor(name, department) {
    super(name);
    this.department = department;
  }
  getRole() {
    return "manager";
  }
}

const bob = new Manager("Bob", "Engineering");
console.log(bob.name); // наследуется от Employee
console.log(bob.department); // поле только у Manager
```

Полиморфизм (`staff.map(e => e.getRole())`) — см. [Polymorphism](#polymorphism). JS `Dog extends Animal` с `super()`: [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**Thoughts:** Предпочитайте композицию, когда поведение зависит от комбинации, а не от таксономии. «Prefer composition over inheritance» — Gang of Four.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Polymorphism</summary>

### Polymorphism

**Definition:** Возможность объектов с разной структурой использовать одноимённые методы, которые работают по-разному в зависимости от контекста. Caller зависит от общей формы (имя метода), а не от конкретного class.

| Type                      | Meaning                                                            |
| ------------------------- | ------------------------------------------------------------------ |
| **Subtype (inheritance)** | Subclass можно использовать везде, где ожидается parent            |
| **Parametric (generics)** | Один код для многих types (`T[]`)                                  |
| **Ad-hoc (overloading)**  | Одно имя, разные сигнатуры (в JS ограничено)                       |
| **Structural (JS/TS)**    | Есть нужные методы — подходит; duck typing / structural types в TS |

**How it works:**

- Один вызов — разное поведение у каждого object: `animals.forEach(a => a.speak())`.
- Код против **общего контракта** (`speak()`, `send()`, `render()`), а не `if (type === "dog")`.
- **В JS:** нет `interface` в runtime — duck typing, JSDoc или TypeScript. Полные примеры: [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** Strategy-подобные замены; списки plugin/notifier; разные component types из одного array; когда `switch` по type постоянно растёт.

**Pluses and minuses:**

| +                                                     | −                                                     |
| ----------------------------------------------------- | ----------------------------------------------------- |
| Меньше `if/else` по конкретным types                  | Нужен реальный контракт, а не случайная строка `type` |
| Слабее связь — меняем одну реализацию, не все callers | JS не проверяет контракт в runtime                    |
| Списки plugin, renderer, strategy                     | «Полиморфизм» через флаги превращается в god `switch` |

**Examples:**

```js
// bad: switch растёт с каждым type — правим каждый caller
function animalSound(type, name) {
  if (type === "dog") console.log(`${name} лает: Гав!`);
  else if (type === "cat") console.log(`${name} мяукает: Мяу!`);
  // Bird → ещё ветка здесь и в каждой копии
}

// good: один вызов метода — новый class = новое поведение, callers не трогаем
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} издаёт звук.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  speak() {
    console.log(`${this.name} (порода: ${this.breed}) лает: Гав!`);
  }
}

class Cat extends Animal {
  speak() {
    console.log(`${this.name} мяукает: Мяу!`);
  }
}

const animals = [new Dog("Рекс", "Овчарка"), new Cat("Пушок")];
animals.forEach((animal) => animal.speak());
// Рекс (порода: Овчарка) лает: Гав!
// Пушок мяукает: Мяу!
```

**Thoughts:** В TS предпочитайте реальный `interface`, когда варианты сильно расходятся в поведении. Subtype polymorphism требует осмысленного parent — см. [Inheritance](#inheritance). Composition + контракт часто безопаснее глубоких деревьев.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Abstraction</summary>

### Abstraction

**Definition:** Выделение главного и сокрытие деталей реализации. Мы создаём интерфейс взаимодействия, не заставляя пользователя разбираться в сложных внутренних механизмах.

**How it works:**

- Caller использует **небольшой стабильный API** — спрашивает _что_ (`play()`, `charge()`, `getUser()`), а не _как_ (декодирование потока, HTTP к Stripe, SQL-запросы).
- Внутренности можно менять (PayPal вместо Stripe, IndexedDB вместо `localStorage`), пока публичный API стабилен.
- Слишком **мало** слоёв — complexity утекает наружу; слишком **много** — теряется поток выполнения.
- **В JS:** нет встроенного `interface` в runtime — детали прячут class, module или function. См. [OOP in JavaScript](./3_js_objects.md#oop-in-javascript).

**When to use:** APIs, repositories, storage adapters, payment providers — любая граница, где caller не должен зависеть от реализации.

**Pluses and minuses:**

| +                       | −                                      |
| ----------------------- | -------------------------------------- |
| Упрощает mental model   | Неверная abstraction дорого откатывать |
| Можно менять реализации | Слишком много слоёв скрывает flow      |

**Examples:**

```js
// bad: caller знает URL Stripe, auth header и форму payload
async function checkout(cart) {
  const res = await fetch("https://api.stripe.com/v1/charges", {
    method: "POST",
    headers: { Authorization: "Bearer sk_..." },
    body: JSON.stringify({ amount: cart.total * 100, currency: "usd" }),
  });
  return res.json();
}

// good: checkout спрашивает _что_ (списать total); gateway прячет _как_
class PaymentGateway {
  async charge(amount) {
    return fetch("/api/charge", {
      method: "POST",
      body: JSON.stringify({ amount }),
    });
  }
}
async function checkout(cart, gateway) {
  return gateway.charge(cart.total); // Stripe → PayPal без правок checkout
}

// good: video.play() — без деталей decode/buffer
const video = document.querySelector("video");
video.play();
```

**Thoughts:** `interface` в TS — abstraction на этапе компиляции. В JS контракты через JSDoc или тесты. Не путайте abstraction с encapsulation — encapsulation прячет _state_, abstraction прячет _как выполняется работа_.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript)

---

</details>

<details>
  <summary>Cohesion vs coupling</summary>

### Cohesion vs coupling

**Definition:** **Cohesion** (связность) is how closely a module's internals belong together. **Coupling** (сцепление) is how much one module knows about another. Want high cohesion, low coupling.

**How it works:**

- Encapsulation raises cohesion: spent-today lives on History, per-day on Budget — `DailyAmount` only subtracts.
- Polymorphism lowers coupling: callers depend on a contract, not on `Spending` vs `Income`.
- Duplicate "spent today" logic in three files is low cohesion spread thin — a rule change misses a copy.

| | High (good / bad) | Low |
| | ------------ | ----------------------------------------- |
| | **Cohesion** | Good — related code in one module | Bad — a class does budget _and_ HTTP _and_ UI |
| | **Coupling** | Bad — many modules break when one changes | Good — swap History storage without touching DailyAmount |

Связность — близкие задачи в одном модуле. Сцепление / зацепление — сколько модули знают друг о друге. Цель: высокая связность, низкое сцепление. High coupling → many mocks just to boot a unit test; see [Making tests easy](./7_special_testing_practices.md#making-tests-easy).

**When to use:** Refactor when you copy the same formula; when a UI tweak forces a domain-class edit.

**Thoughts:** Hide internals (encapsulation) + talk through contracts (polymorphism) = cohesive modules that barely know each other.

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/)

---

</details>

<details>
  <summary>Composition vs inheritance</summary>

### Composition vs inheritance

**Definition:** **Inheritance** builds "is-a" hierarchies. **Composition** builds "has-a" by embedding objects and delegating behavior.

**How it works:**

Inheritance designs **from general to specific** (Animal → Dog). Composition designs **from parts to whole** (Human = skeleton + immune + cardio). Feature sets group into contracts; a class picks the set it needs — no single taxonomy. Biology has several overlapping trees; software usually does too.

Наследование — «является» (иерархия). Композиция — «имеет» (набор фич). Иерархий может быть несколько сразу — композиция не ломается.

**When to use:** Default to composition. Inheritance only for stable, shallow "is-a" (e.g. `Error` subclasses).

**Pluses and minuses:**

| + (composition)                           | −                           |
| ----------------------------------------- | --------------------------- |
| Loose coupling; swap behaviors at runtime | More boilerplate wiring     |
| Avoids fragile base class problem         | Many small objects to track |
| Aligns with LSP and OCP                   |                             |

| + (inheritance)                   | −                                |
| --------------------------------- | -------------------------------- |
| Fast to prototype shared behavior | Changes in base break subclasses |
| Clear taxonomy                    | Deep trees resist refactoring    |

**Examples:**

```js
// Composition — mix feature objects (JS has no `implements`)
const cardio = {
  pumpBlood() {},
};
const immune = {
  fightInfection() {},
};

const human = { ...cardio, ...immune };
const worm = { ...cardio, ...immune }; // same features, no shared parent class
```

**Thoughts:** React favors composition (`children`, render props, hooks). SOLID: OCP via composition; LSP applies to inheritance. Mixins/spread work but stay untyped without TS interfaces. See [Mixins](./3_js_classes.md#mixins).

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/) · [refactoring.guru — composition over inheritance](https://refactoring.guru/design-patterns/composite)

---

</details>

<details>
  <summary>SOLID (overview)</summary>

### SOLID (overview)

**Definition:** **SOLID** — пять принципов дизайна Роберта Мартина («Дядя Боб») для кода, который остаётся **удобным для изменений, тестов и расширения**. Сформулированы в ООП, но относятся к **классам**, **модулям** и **функциям**. **Ориентир, когда больно менять код** — не требование писать пять абстракций в каждом коротком скрипте.

**How it works:**

| Letter | Principle             | Простыми словами                                       |
| ------ | --------------------- | ------------------------------------------------------ |
| **S**  | Single Responsibility | Один модуль / класс / функция = одна задача            |
| **O**  | Open/Closed           | Расширять **новым** кодом — не править стабильный      |
| **L**  | Liskov Substitution   | Любой подтип должен работать там же, где родитель      |
| **I**  | Interface Segregation | Не навязывать методы, которые вызывающий не использует |
| **D**  | Dependency Inversion  | Передавать зависимости снаружи — не хардкодить внутри  |

Каждая буква закрывает свой вид связности. Эссе по каждому принципу на этой странице: [SRP](#single-responsibility-srp) · [OCP](#open-closed-ocp) · [LSP](#liskov-substitution-lsp) · [ISP](#interface-segregation-isp) · [DIP](#dependency-inversion-dip).

**When to use:** God-классы и god-модули; plugin-style провайдеры; code review; интервью. На одноразовом скрипте — без тяжёлой абстракции.

**Pluses and minuses:**

| +                                              | −                                  |
| ---------------------------------------------- | ---------------------------------- |
| Проще тесты, рефакторинг, зоны ответственности | Переусложнение — лишние абстракции |
| Общий словарь на code review                   | Споры про SOLID вместо доставки    |
| Масштабируется с ростом кодовой базы           | Границы субъективны (особенно SRP) |

**Examples:**

```js
// S — good: engine logic separate from dashboard UI
class CarEngine {
  start() {
    return "engine on";
  }
}

class CarDashboard {
  render({ speed }) {
    return `${speed} km/h`;
  }
}

// S — bad: one class mixes engine + UI (two reasons to change)
// class Car {
//   start() { return "engine on"; }
//   renderDashboard() { return `${this.speed} km/h`; }
// }

// O — good: new engine = new class, Car unchanged
class GasEngine {
  start() {
    return "gas started";
  }
}

class Car {
  constructor(engine) {
    this.engine = engine;
  }
  start() {
    return this.engine.start();
  }
}

new Car(new GasEngine());

// O — bad: edit Car.start() for every new engine type
// class Car {
//   start() {
//     if (this.type === "gas") return "gas started";
//     if (this.type === "electric") return "motor started";
//   }
// }

// L — good: goToWork(car) works for any car that returns a result
function goToWork(car) {
  return car.drive(10);
}

class RoadCar {
  drive(km) {
    return { km, status: "ok" };
  }
}

goToWork(new RoadCar());

// L — bad: subclass throws — goToWork crashes without try/catch
class OutOfFuelCar extends RoadCar {
  drive(km) {
    throw new Error("No fuel");
  }
}
// goToWork(new OutOfFuelCar());

// I — good: showroom lists; workshop repairs — separate APIs
class CarLot {
  listAvailable() {
    return ["sedan", "suv"];
  }
}

class CarWorkshop {
  repair(carId) {
    return `repaired ${carId}`;
  }
}

// I — bad: lot forced to stub repair() it never uses
// class Garage {
//   listAvailable() { return ["sedan", "suv"]; }
//   repair() { throw new Error("We don't repair here"); }
// }
// class CarLot extends Garage {}

// D — good: inject logger — business logic does not own console
function processOrder(order, logger) {
  logger.log(`Processing ${order.id}`);
}

// D — bad: hardcoded console inside domain code
// function processOrder(order) {
//   console.log(`Processing ${order.id}`);
// }
```

**Thoughts:** Разбор по стеку с `// good` / `// bad` для всех пяти букв: [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Дополняет [DRY, KISS, YAGNI](./0_system_design_principles.md) — не абстрагируйте, пока не появится второй use case.

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/) · [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Single Responsibility (SRP)</summary>

### Single Responsibility (SRP)

**Definition:** A module (class, function, file) should have **one reason to change** — one cohesive job or axis of change.

**How it works:**

- Identify **who** would request changes: UI team, API team, billing rules, etc.
- If two unrelated stakeholders trigger edits in the same module, split it.
- SRP is about **cohesion**, not "one method per class" — a `UserValidator` with three validation methods still has one job.
- In React: separate data fetching, formatting, and presentation.

**When to use:** Split god components; `UserRepository` (DB) vs `UserPresenter` (UI); separate email template rendering from send logic; extract validation from route handlers.

**Pluses and minuses:**

| +                                       | −                                      |
| --------------------------------------- | -------------------------------------- |
| Easier test, refactor, and reason about | Over-splitting creates file sprawl     |
| Clear ownership per module              | "One reason" is subjective             |
| Smaller diffs, fewer merge conflicts    | Can feel bureaucratic for tiny scripts |

**Examples:**

```js
// ❌ Two reasons to change: DB schema AND API response shape
class UserService {
  async getUser(id) {
    const row = await db.query('SELECT * FROM users WHERE id = ?', [id]);
    return { fullName: `${row.first} ${row.last}`, avatar: row.img_url };
  }
}

// ✅ Split responsibilities
class UserRepository { findById(id) { return db.query(...); } }
class UserMapper { toDto(row) { return { fullName: `${row.first} ${row.last}`, ... }; } }
```

**Thoughts:**

Interview: SRP ≠ "one function" — it's one **axis of change**. A change in tax law shouldn't force editing UI code.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Open/Closed (OCP)</summary>

### Open/Closed (OCP)

**Definition:** Software entities should be **open for extension** but **closed for modification** — add behavior via new code, not by editing stable modules.

**How it works:**

- Define **extension points**: strategy interfaces, middleware hooks, plugin registries.
- New variants implement the interface or register with the registry.
- Core logic calls the abstraction — never a growing `if/else` or `switch` on types.
- In JS: object maps, higher-order functions, and middleware pipelines achieve OCP without inheritance.

**When to use:** Payment providers, export formats (CSV/JSON/PDF), validation rules, Express/Koa middleware, ESLint rules, React render props / compound components.

**Pluses and minuses:**

| +                                         | −                                        |
| ----------------------------------------- | ---------------------------------------- |
| Safer additions — existing code untouched | Abstraction upfront cost                 |
| Plugin and middleware architectures       | Wrong extension points are hard to fix   |
| Fewer regression bugs on extend           | Can over-engineer for one implementation |

**Examples:**

```js
// bad: правим checkout при каждом новом payment provider
function checkout(provider, amount) {
  if (provider === "stripe") return chargeStripe(amount);
  if (provider === "paypal") return chargePayPal(amount);
  // Apple Pay → снова меняем эту функцию (open for modification)
}

// good: checkout закрыт; новые providers регистрируются в map
const paymentStrategies = {
  stripe: (amount) => chargeStripe(amount),
  paypal: (amount) => chargePayPal(amount),
};

function checkout(provider, amount) {
  const pay = paymentStrategies[provider];
  if (!pay) throw new Error("Unknown provider");
  return pay(amount);
}

// расширяем снаружи — checkout не трогаем
paymentStrategies.applePay = (amount) => chargeApplePay(amount);
```

**Thoughts:** OCP сочетается с Strategy и Factory. `switch` по закрытому enum допустим, если набор фиксирован и принадлежит одной команде.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Liskov Substitution (LSP)</summary>

### Liskov Substitution (LSP)

**Definition:** Subtypes must be **substitutable** for their base types — callers using the parent contract must not break when given a subclass.

**How it works:**

- Subclass must honor **preconditions** (not strengthen), **postconditions** (not weaken), and **invariants** of the parent.
- If code expects `Rectangle.setWidth(w)` to leave height unchanged, `Square extends Rectangle` violates LSP when `setWidth` also changes height.
- Prefer composition when "is-a" taxonomy doesn't hold behaviorally.
- In JS/TS: duck typing doesn't save you — behavioral contracts still matter.

**When to use:** Review any `extends` — does subclass throw on parent methods? Narrow input types? Return incompatible shapes? Plugin systems where plugins must not break host assumptions.

**Pluses and minuses:**

| +                                 | −                                                 |
| --------------------------------- | ------------------------------------------------- |
| Reliable polymorphism             | Violations are subtle and discovered late         |
| Safe refactoring with inheritance | Pushes toward composition (good, but more wiring) |
| Clear contracts in hierarchies    | Hard to formalize in untyped JS                   |

**Examples:**

```js
// bad: Penguin is-a Bird, но ломает callers, ожидающих рабочий fly()
class Bird {
  fly() {
    return "flying";
  }
}
class Penguin extends Bird {
  fly() {
    throw new Error("Cannot fly");
  }
}

function makeBirdFly(bird) {
  return bird.fly(); // для Bird ок, для Penguin — throw; нарушение LSP
}
makeBirdFly(new Penguin());

// good: отдельная способность — caller зависит только от нужного метода
class Penguin {
  swim() {
    return "swimming";
  }
}

function makeSwimmer(animal) {
  return animal.swim();
}
makeSwimmer(new Penguin());
```

**Thoughts:** Классическая ловушка на собеседовании: `Square extends Rectangle` — `setWidth` у квадрата меняет и высоту. Решение: общий `Shape` с `area()`, а не ложная иерархия **is-a**.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Interface Segregation (ISP)</summary>

### Interface Segregation (ISP)

**Definition:** Clients should not be forced to depend on methods they don't use — prefer **small, focused interfaces** over fat ones.

**How it works:**

- Split large interfaces into role-specific ones (`Readable`, `Writable`, `Deletable`).
- Callers depend only on the methods they need — easier to mock and implement.
- In TS: `interface` segregation; in JS: pass minimal object shapes or use JSDoc `@typedef` for contracts.
- Fat React component props that force unused callbacks violate ISP spirit.

**When to use:** Split `File` god interface into `read()`, `write()`, `watch()`; API clients that only need `get` shouldn't require `delete`; segregate admin vs read-only service interfaces.

**Pluses and minuses:**

| +                                     | −                                        |
| ------------------------------------- | ---------------------------------------- |
| Smaller, focused contracts            | More types/interfaces to maintain        |
| Easier mocks in tests                 | Can fragment APIs if overdone            |
| Implementers only stub what they need | Discovery — "which interface do I need?" |

**Examples:**

```ts
// bad: жирный DbClient — read-only код вынужден зависеть от write/delete
class DbClient {
  read(id: string) {
    /* ... */
  }
  write(data: Data) {
    /* ... */
  }
  delete(id: string) {
    /* ... */
  }
}

function generateReport(db: DbClient) {
  return db.read("summary"); // нужен только read
}

// good: узкий контракт — caller берёт только то, что использует
type Readable = { read(id: string): Promise<Data> };
type Writable = { write(data: Data): Promise<void> };

function generateReport(source: Readable) {
  return source.read("summary");
}

const readOnlyDb: Readable = { read: (id) => fetchReport(id) };
generateReport(readOnlyDb);
```

**Thoughts:** ISP снижает связность между модулями. На фронте жирные props с неиспользуемыми callbacks — запах ISP; передавайте только нужные поля.

**Sources:** [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>

<details>
  <summary>Dependency Inversion (DIP)</summary>

### Dependency Inversion (DIP)

**Definition:** High-level modules depend on **abstractions**; low-level modules **implement** those abstractions — both depend on interfaces, not on each other directly.

**How it works:**

1. High-level policy (`OrderService`) defines what it needs (`PaymentGateway`, `OrderRepo`).
2. Low-level details (`StripeGateway`, `PostgresOrderRepo`) implement those abstractions.
3. **Inject** dependencies via constructor, factory, or DI container — don't `import` concrete implementations inside business logic.
4. Tests swap in fakes/mocks at the abstraction boundary — kinds: [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies).

**When to use:** Inject `HttpClient`, `Storage`, `Logger`; repository pattern over raw ORM in services; React context/providers for testable dependencies; avoid `fetch` calls inside pure domain functions.

**Pluses and minuses:**

| +                                                | −                                   |
| ------------------------------------------------ | ----------------------------------- |
| Testable via mocks/stubs                         | Indirection and wiring overhead     |
| Swappable implementations (DB, payment, storage) | DI containers can hide dependencies |
| Business logic isolated from frameworks          | Learning curve for junior devs      |

**Examples:**

```ts
// bad: high-level код жёстко привязан к Postgres — сложно тестировать и менять storage
import { PostgresUserRepo } from "./postgres";

function getUserProfile(id: string) {
  const repo = new PostgresUserRepo();
  return repo.findById(id);
}

// good: зависимость от абстракции; реализацию внедряем снаружи
type UserRepo = { findById(id: string): Promise<User> };

function getUserProfile(id: string, repo: UserRepo) {
  return repo.findById(id);
}

// production: getUserProfile(id, new PostgresUserRepo())
// test:       getUserProfile(id, { findById: async () => mockUser })
```

**Thoughts:** DIP ≠ десять параметров у каждой функции. В JS нормальны defaults на уровне модуля с подменой при тестах. Сочетается с Factory и Repository. Constructor injection работает и в plain JS (`new DailyAmount(budget, history)`); в TS помогают типизированные абстракции. См. [Entities, relations & design](./0_system_design_grasp.md#entities-relations--design).

**See also:** Swappable fakes at the interface = less mock archaeology — [Making tests easy](./7_special_testing_practices.md#making-tests-easy).

**Sources:**

[Doka — OOP](https://doka.guide/tools/oop/) · [SOLID (overview)](#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

---

</details>
