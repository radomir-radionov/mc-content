# Design patterns

Каталог GoF: творческие, структурные и поведенческие модели. SOLID, DRY/KISS/YAGNI и GRASP живут в родственных темах Принципов. TDD/BDD: [Тесты](./7_special_testing.md). Внедрение конструктора: [Сущности, отношения и дизайн](./0_system_design_grasp.md#entities-relations--design).

[ ← Вернуться в хаб](./0_system_design.md)

### Related docs

- [Проектирование системы — Ядро](./0_system_design_core.md) · [Проектирование системы — Архитектура](./0_system_design_architecture.md) · [Шаблоны архитектуры](./7_special_architecture.md) · [Проектирование системы — Данные](./0_system_design_data.md) · [SOLID](./7_special_engineering.md) · [DRY, KISS, YAGNI](./0_system_design_principles.md) · [GRASP](./0_system_design_grasp.md) · [Шаблоны проектирования](./7_special_patterns.md) · [Проектирование системы —Интервью](./0_system_design_interview.md)

### Main concepts

| Площадь   | Темы                                                 |
| --------- | ---------------------------------------------------- |
| Каталог   | 23 GoF patterns (creational, structural, behavioral) |
| Шпаргалки | Группировка, ориентированная на интерфейс            |
| Экстра    | Репозиторий (не GoF)                                 |

### Sections

- [10. Design patterns](#10-design-patterns)
- [GOF design patterns — catalog](#gof-design-patterns--catalog)
- [Creational patterns — cheat sheet](#creational-patterns--cheat-sheet)
- [Structural patterns — cheat sheet](#structural-patterns--cheat-sheet)
- [Behavioral patterns — cheat sheet](#behavioral-patterns--cheat-sheet)

---

<a name="10-design-patterns"></a>

## 10. Design patterns

<details>
  <summary>Design pattern (general)</summary>

### Design pattern (general)

**Definition:**

Многоразовое решение повторяющейся проблемы дизайна — не копирование кода, а структура и шаблон сотрудничества.

| Категория         | Примеры                                                                        |
| ----------------- | ------------------------------------------------------------------------------ |
| **Творческий**    | Фабрика, Фабричный метод, Абстрактная фабрика, Построитель, Синглтон, Прототип |
| **Структурный**   | Адаптер, Декоратор, Фасад, Прокси                                              |
| **Поведенческий** | Цепочка ответственности, Наблюдатель, Стратегия, Команда, Итератор             |

**When to use:** Когда проблема соответствует каталогу; шаблоны названий в документах и ​​обзорах.

**Pluses and minuses:**

| +                            | −                                            |
| ---------------------------- | -------------------------------------------- |
| Общий словарь                | Чрезмерное применение («типичная лихорадка») |
| Испытанные в бою конструкции | Может скрыть простой код                     |

**Thoughts:** Полный каталог и группировку внешнего интерфейса см. в [Каталог GOF](#gof-design-patterns--catalog). Распределение ответственности (Эксперт, Создатель, Контролер): [GRASP](./0_system_design_grasp.md#grasp-principles). Творческий список: [шпаргалка](#creational-patterns--шпаргалка). Структурный список: [шпаргалка](#structural-patterns--шпаргалка). Поведенческий список: [шпаргалка](#behavioral-patterns--шпаргалка).Шаблоны не являются целью пиара.

**Sources:**

[refactoring.guru](https://refactoring.guru/design-patterns/catalog) · [Doka — Архитектура и шаблоны проектирования](https://doka.guide/tools/architecture-and-design-patterns/)

---

</details>

<details>
  <summary>GOF design patterns — catalog</summary>

<a name="gof-design-patterns--catalog"></a>

### GOF design patterns — catalog

**Definition:**

Банда четырех (Гамма, Хелм, Джонсон, Влиссидес) — 23 многократно используемых шаблона ООП-проектирования из _Шаблоны проектирования: элементы многоразового объектно-ориентированного программного обеспечения_ (1994).

| Творческий          | Структурный | Поведенческий           |
| ------------------- | ----------- | ----------------------- |
| Фабричный метод     | Адаптер     | Цепочка ответственности |
| Абстрактная фабрика | Мост        | Команда                 |
| Строитель           | Композитный | Переводчик              |
| Прототип            | Декоратор   | Итератор                |
| Синглтон            | Фасад       | Медиатор                |

| Наилегчайший вес | Сувенир |

| Прокси | наблюдатель |
|
| Государство |
|
| Стратегия |
|
| Шаблонный метод |
|
| Посетитель |

**Thoughts:**

Шаблоны — это словарный запас, а не цели. «Нам нужна фабрика» — неправильная отправная точка: сначала определите проблему, а затем назовите закономерность. Примечания Deep JS/Doka: [Шпаргалка по созданию](#creational-patterns--шпаргалка).

Список фронтенда Doka (не все 23):

| Вид               | Работа                                                           | Имена, которые нужно знать                                                      |
| ----------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Творческий**    | Создание объектов/семейств без дублирования                      | Простая фабрика (неформальная), Фабричный метод, Абстрактная фабрика, Строитель |
| **Структурный**   | Объединить сущности («как собрать компонент»)                    | Адаптер, Декоратор, Фасад, Прокси                                               |
| **Поведенческий** | Кто что делает, как разговаривает («как организовать поведение») | Цепочка ответственности, Стратегия, Командование, Наблюдатель                   | Гуру рефакторинга: Creational = механизмы создания; структурный = собрать объекты/классы; поведенческий = алгоритмы и ответственность. **Простая фабрика** — это общее название функции, которая `switch`es по типу — полезно, но не является отдельным шаблоном GoF (фабричный метод есть). Короткие списки: [структурные](#structural-patterns--шпаргалка) · [поведенческие](#behavioral-patterns--шпаргалка). |

**Sources:** [Гуру рефакторинга — Каталог](https://refactoring.guru/design-patterns/catalog) · [Doka — Архитектура и шаблоны проектирования](https://doka.guide/tools/architecture-and-design-patterns/) · [ByteByteGo — ООП-дизайн Шаблоны] (https://blog.bytebytego.com/i/162154465/oop-design-patterns-you-must-know)

</details>

<details>
  <summary>Creational patterns — cheat sheet</summary>

<a name="creational-patterns--cheat-sheet"></a>

### Creational patterns — cheat sheet

**Definition:**

Шаблоны создания скрывают **как рождаются объекты** — поля, значения по умолчанию, дополнительные данные, экземпляр какого класса создавать — поэтому вызывающие объекты не повторяют конструкцию и не зависят от конкретных типов.

Четыре повседневных набора Doka (не полный список GoF): **Factory** (простой), **Абстрактная фабрика**, **Builder**, **Singleton**. В GoF также есть **Фабричный метод** и **Прототип**.«Фабрика» Doka — это неформальная **простая фабричная** функция, а не фабричный метод.

| Узор                    | Одна линия                                                                          | Ментальная модель                                        |
| ----------------------- | ----------------------------------------------------------------------------------- | -------------------------------------------------------- |
| **Завод** (простой)     | Функция владеет рецептом; вызывающий абонент передает намерение (`createGuitar(6)`) | Гитарный магазин против фрезерования грифа               |
| **Заводской метод**     | Подклассы создателя переопределяют `createX()` — какой _класс_ рождается            | Department.hire → Программист vs Бухгалтер               |
| **Абстрактная фабрика** | Интерфейс, создающий **семейство** сопутствующих продуктов                          | Резервирование оркестра: инструмент + музыкант, один API |
| **Строитель**           | Дополнительные шаги, затем `build()`; цепочка возвращается `this`                   | Кофе: эспрессо + молоко + сахар                          |
| **Синглтон**            | Не более одного экземпляра; позже `new` возвращает первый                           | One Sun — вместо этого обычно делают редизайн            |

**How it works:**

- **Фабрика против Абстрактной фабрики** — Фабрика возвращает **один** продукт. Абстрактная Фабрика — это фабрика фабрик: одни и те же _операции_, разные _семьи_ (Скрипка+Скрипач против Виолончели+Виолончелист). Цель снижается `instanceof` деревья.
- **Factory vs Builder** — Фабрика: «дайте мне полный объект, может быть, с парой аргументов». Построитель: много **необязательных шагов**, порядок имеет значение, избегайте конструктора с десятью логическими значениями.
- **Простой фабричный метод против фабричного метода** — `createX(type)` + `switch` это идиома. Фабричный метод — это когда _класс_ откладывается `new` на подклассы (Refactoring Guru).
- **Шаблон Singleton против DI «singleton»** — Шаблон = ограничить количество. Срок службы DI = повторное использование одного и того же внедренного экземпляра. Разные работы (Дока).

| Использование                                                                           | Узор                                      |
| --------------------------------------------------------------------------------------- | ----------------------------------------- |
| Построение > 1–2 строк, или требуется расчет/дополнительные данные                      | Фабрика                                   |
| Сопутствующие продукты должны оставаться совместимыми; клиент не должен называть классы | Абстрактная фабрика                       |
| Множество дополнительных функций;телескопические конструкторы                           | Строитель                                 |
| Домен действительно допускает только один (редкий)                                      | Синглтон — предпочитаю ESM `export const` |

```js
// Factory — recipe + a calculation. Call sites stay createGuitar(6) if fields change.
function createGuitar(stringsCount = 6, maxWeight = 5) {
  const fretBoardMaterial = maxWeight <= 5 ? "fir" : "cedar";
  return { stringsCount, fretsCount: 24, fretBoardMaterial };
}
createGuitar(6);
createGuitar(7, 8);
```

```js
// Abstract Factory — same two methods; violin vs cello family. reserve() never instanceofs.
const violinKit = {
  reserveInstrument: () => ({ play: (n) => `violin ${n}` }),
  notifyPlayer: () => ({ play: (piece) => piece.map((n) => `violin ${n}`) }),
};
const celloKit = {
  reserveInstrument: () => ({ play: (n) => `cello ${n}` }),
  notifyPlayer: () => ({ play: (piece) => piece.map((n) => `cello ${n}`) }),
};
function reserve(kit) {
  kit.notifyPlayer();
  return kit.reserveInstrument();
}
reserve(violinKit).play("A"); // 'violin A'
reserve(celloKit).play("A"); // 'cello A'
```

```js
// Builder — optional steps, then build(). addX returns this so you can chain.
class DrinkBuilder {
  settings = { base: "espresso" };
  addMilk() {
    this.settings.milk = true;
    return this;
  }
  addSugar() {
    this.settings.sugar = true;
    return this;
  }
  build() {
    return { ...this.settings };
  }
}
new DrinkBuilder().addMilk().build(); // { base: 'espresso', milk: true }
new DrinkBuilder().addSugar().addMilk().build();
```

```js
// Singleton — second new returns the first object. Prefer export const in real JS.
class Sun {
  static #instance = null;
  constructor() {
    if (Sun.#instance) return Sun.#instance;
    Sun.#instance = this;
  }
}
new Sun() === new Sun(); // true
// Idiomatic: export const sun = Object.freeze({ mass: 1.989e30 });
```

**See also:** [Factory](#factory--factory-method) · [Абстрактная фабрика](#abstract-factory) · [Builder](#builder) · [Singleton](#singleton-caution) · [Каталог GOF](#gof-design-patterns--catalog) · [фабрика JS](./3_js_objects.md#factory-and-module-patterns)

**Sources:**

[Doka — Шаблоны создания](https://doka.guide/tools/design-patterns-creational/) (Саша Беспоясов, обновление 29 февраля 2024 г.) · [Refactoring Guru — Каталог](https://refactoring.guru/design-patterns/catalog) · [Сравнение фабрик](https://refactoring.guru/design-patterns/factory-comparison)

---

</details>

<details>
  <summary>Structural patterns — cheat sheet</summary>

<a name="structural-patterns--cheat-sheet"></a>

### Structural patterns — cheat sheet

**Definition:**

Структурные шаблоны отвечают на вопрос: «Как нам собрать компонент, чтобы он мог сочетаться с другими?» — они управляют **композицией объектов**, а не созданием (креационное) или тем, кто с кем разговаривает (поведенческое).

| Узор          | Одна линия                                                                 | Ментальная модель                             |
| ------------- | -------------------------------------------------------------------------- | --------------------------------------------- |
| **Адаптер**   | Перевести несовместимую форму API/данных в то, что уже ожидает клиент      | Дорожная вилка                                |
| **Фасад**     | Один простой метод для многих шагов подсистемы                             | «Нагреть воду» против 8 вызовов атомных машин |
| **Декоратор** | Перенос, чтобы **добавить** поведение; сохранить тот же интерфейс          | Телефон + чехол; `debounce(fn)`               |
| **Прокси**    | Тот же интерфейс; **gate** доступ (ленивый, аутентификация, кеш, перехват) | Администратор; JS `Proxy`                     |

**Чем они отличаются** (Гуру рефакторинга):

- **Адаптер против Декоратора** — Адаптер предоставляет _другой_ интерфейс. Декоратор сохраняет (или расширяет) оригинал и поддерживает укладку оберток.
- **Адаптер против фасада** — адаптер оборачивает _один_ объект в соответствии с целевым интерфейсом. Фасад предоставляет _новый, более простой_ интерфейс над _подсистемой_ (много объектов/много вызовов).- **Декоратор против прокси** — оба оборачивают и обычно сохраняют один и тот же интерфейс. Декоратор _добавляет_ обязанности (регистрация, повтор, шифрование). Прокси _управляет_ доступом (отложенная инициализация, защита, удаленная заглушка, кеш) и не предназначен для обогащения общедоступного API.
- **ДжС `Proxy` против шаблона** — `new Proxy(target, handler)` — это языковой инструмент, который перехватывает операции (`get`, `set`, …). С его помощью вы можете реализовать GoF Proxy _или_ Decorator. Использование реактивности Vue 3 `Proxy`.

**How it works:**

| Использование                                                                                             | Узор      |
| --------------------------------------------------------------------------------------------------------- | --------- |
| Сервер `snake_case` / вложенная полезная нагрузка ≠ приложение `camelCase` DTO                            | Адаптер   |
| Шестиугольный / порты: Prisma, Stripe, Express работают во внешнем слое **адаптера**                      | Адаптер   |
| Один и тот же многошаговый танец SDK, скопированный в нескольких местах                                   | Фасад     |
| Сквозные дополнительные возможности: журнал, метрики, кеш, `throttle` / `debounce`                        | Декоратор |
| Проверка/задержка/отказ перед реальным объектом (аутентификация, отложенная проверка, защита вставки DOM) | Прокси    |

```js
// Adapter — foreign shape → our shape. Callers never see snake_case.
function toUser(dto) {
  return { id: dto.ID, name: dto.user_name };
}
toUser({ ID: "1", user_name: "Ada" }); // { id: '1', name: 'Ada' }
```

```js
// Facade — one function runs several subsystem steps. UI calls brew(), not 4 SDK methods.
function brew(machine) {
  machine.grind();
  machine.heat();
  machine.pour();
  return machine.cup();
}
```

```js
// Decorator — same function, extra behavior before/after. update itself stays clean.
const withLog =
  (fn) =>
  (...args) => {
    console.log("call", args);
    return fn(...args);
  };
const save = (user) => {
  /* write */
};
const saveLogged = withLog(save);
saveLogged({ id: 1 }); // logs, then save
```

```js
// Proxy — same property names; trap decides what happens. original.secret stays readable.
const user = { name: "Ada", secret: "x" };
const gate = new Proxy(user, {
  get(t, key, r) {
    if (key === "secret") return undefined;
    return Reflect.get(t, key, r);
  },
});
gate.name; // 'Ada'
gate.secret; // undefined
```

**Thoughts:** Адаптер **переводит**. Фасад **упрощается**. Декоратор **добавляет**. Прокси **ворота**.

**See also:** [Адаптер](#адаптер) · [Фасад](#фасад) · [Декоратор](#декоратор) · [Прокси](#прокси) · [JS `Proxy`](./3_js_objects.md#proxy-and-reflect) · [Шестиугольные адаптеры](./7_special_architecture.md#clean-architecture--hexagonal)

**Sources:**

[Гуру рефакторинга — отношения адаптера](https://refactoring.guru/design-patterns/adapter) · [Декоратор](https://refactoring.guru/design-patterns/decorator) · [Фасад](https://refactoring.guru/design-patterns/facade) · [Прокси](https://refactoring.guru/design-patterns/proxy) · [MDN `Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

---

</details>

<details>
  <summary>Behavioral patterns — cheat sheet</summary>

<a name="behavioral-patterns--cheat-sheet"></a>

### Behavioral patterns — cheat sheet

**Definition:**

Поведенческие модели отвечают на вопрос: «Кто что делает и как разговаривает?» — они разделяют **ответственность** и **общение**, а не создание или сборку объектов.

| Узор                        | Одна линия                                                                                        | Поменяйтесь местами в своей голове              |
| --------------------------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| **Стратегия**               | Та же работа, другой алгоритм; выбрать (и поменять местами) во время выполнения                   | Навигатор: прогулка/велосипед/поезд             |
| **Цепочка ответственности** | Запрос обходит обработчики до тех пор, пока **один** не сможет его обработать                     | Начисление доллара США против евро против рубля |
| **Команда**                 | Действие + данные становятся объектом, который можно ставить в очередь, регистрировать и отменять | Редукторное действие `{ type, payload }`        |
| **Наблюдатель**             | Субъект уведомляет всех, кто подписался; список может измениться                                  | Вакансия → подбор кандидатов                    |

**How it works:**

- **Стратегия против CoR** — Стратегия: вызывающий абонент _выбирает_ алгоритм. CoR: звонивший _не знает_, кто будет отвечать; порядок обработчиков решает.
- **CoR против `switch`** — `switch` работает, когда все случаи известны и принадлежат здесь (редюсеры Redux). CoR выигрывает, когда обработчики, порядок или варианты неизвестны или подключаются позже — диспетчер не импортирует каждый обработчик.
- **CoR против промежуточного программного обеспечения** — Doka: промежуточное программное обеспечение позволяет запускать **каждую** ссылку (аутентификация → журнал → обработчик). Canonical CoR: **один** обработчик обрабатывает запрос и останавливается. Refactoring Guru по-прежнему рассматривает промежуточное программное обеспечение как CoR с перевернутым принципом «всегда вызывать». `next()` если ты не остановишься».
- **Null Object** — тот же интерфейс, ничего не делает. Использовать как `switch` `default` или терминатор цепочки, чтобы звонящие пропускали вызов `if (handler)`.
- **Command vs Observer** — Команда представляет собой _однократный_ инкапсулированный запрос (вызыватель → `execute`). У Observer _действующая_ подписка (`attach` / `detach` / `notify`).

| Использование                                                            | Узор        |
| ------------------------------------------------------------------------ | ----------- |
| Алгоритм может измениться во время работы приложения                     | Стратегия   |
| Неизвестные/обработчики плагинов; не связывайте диспетчера со всеми ними | КОР         |
| Отмена, очередь, история, дополнительная полезная нагрузка для действия  | Команда     |
| Неизвестно, сколько слушателей; подписка временная                       | наблюдатель |

**Thoughts:** Назовите _проблему_ (поменяйте алгоритм / продолжайте до тех пор, пока не будет обработано / повторите действие / уведомите многих). Шаблон — это словарный запас, а не количество классов.

**Sources:** [Doka — Поведенческие шаблоны](https://doka.guide/tools/design-patterns-behaviorial/) · [Гуру рефакторинга — Каталог](https://refactoring.guru/design-patterns/catalog)

</details>

<details>
  <summary>Observer</summary>

<a name="observer"></a>

### Observer

**Definition:** Один объект изменился → все **подписчики** узнают об этом. Также **Listener** или **Event-Subscriber** (поведенческий шаблон GoF).

**Simple idea:** Канал на YouTube выложил видео → все подписчики получают уведомление. Не нужно заходить каждый час и проверять.

**How it works:**

| Часть                    | Задача                                      |
| ------------------------ | ------------------------------------------- |
| **Subject** (издатель)   | Данные + список слушателей                  |
| **Observer** (подписчик) | Callback, который срабатывает при изменении |

1. **Подписка** — слушатель попадает в список.
2. **Изменение** — субъект обновляет данные.
3. **Уведомление** — субъект вызывает каждого слушателя.
4. **Отписка** — слушатель уходит (важно — иначе утечка памяти).

**Push vs pull:** **Push** = субъект шлёт новые данные (`notify(25)`). **Pull** = субъект говорит «я изменился»; слушатель читает `getState()`.

| Это не то же самое | Разница                                |
| ------------------ | -------------------------------------- |
| **Pub/Sub**        | Через брокер/топик, часто async        |
| **Mediator**       | Центральный роутер, не прямой fan-out  |
| **Command**        | Одно действие — не постоянная подписка |

См. также: [Mediator](#mediator) · [Command](#command).

**When to use:** Клики по кнопке, Redux `subscribe`, `EventEmitter` — когда **много частей** должны отреагировать на **одно** изменение.

**Pluses and minuses:**

| +                                           | −                                    |
| ------------------------------------------- | ------------------------------------ |
| Новые слушатели без правок субъекта         | Легко забыть отписаться              |
| Подписка/отписка в runtime                  | Сложнее понять, кто кого вызвал      |
| Субъект не знает конкретные типы слушателей | Слушатели могут зациклить друг друга |

#### Common pitfalls

- Нет cleanup → утечка (`removeEventListener`, return в `useEffect`, отписка от store).
- Слушатель меняет субъект → бесконечный цикл notify.
- Один callback? Просто передайте функцию — Observer для **многих** слушателей.

**Examples:**

```js
// 1) Минимальный store — подписка, изменение, уведомление
const listeners = new Set();
let count = 0;

function subscribe(fn) {
  listeners.add(fn);
  return () => listeners.delete(fn); // отписка
}

function setCount(next) {
  count = next;
  listeners.forEach((fn) => fn(count)); // сообщить всем
}

const off = subscribe((n) => console.log("count is", n));
setCount(1); // logs: count is 1
off(); // больше не слушаем
```

```js
// 2) Вы уже используете Observer — события DOM
const btn = document.querySelector("button");
const onClick = () => console.log("clicked");

btn.addEventListener("click", onClick); // подписка
btn.removeEventListener("click", onClick); // отписка
```

```js
// 3) Метеостанция — один источник, много экранов
class WeatherStation {
  #listeners = [];
  #temp = 20;

  subscribe(fn) {
    this.#listeners.push(fn);
  }

  setTemp(c) {
    this.#temp = c;
    this.#listeners.forEach((fn) => fn(this.#temp));
  }
}

const station = new WeatherStation();
station.subscribe((t) => console.log("Phone:", t + "°C"));
station.subscribe((t) => console.log("TV:", t + "°C"));
station.setTemp(25);
// Phone: 25°C
// TV: 25°C
```

**Thoughts:** Спросите: «Кому нужно знать, когда это изменится?» Cleanup в React `useEffect` = отписка. Redux `store.subscribe` = Observer.

**Sources:** [Refactoring Guru — Observer](https://refactoring.guru/design-patterns/observer) · [MDN — EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget) · [GeeksforGeeks — Observer](https://www.geeksforgeeks.org/system-design/observer-pattern-set-1-introduction/) · [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)

---

</details>

<details>
  <summary>Factory / Factory Method</summary>

<a name="factory--factory-method"></a>

### Factory / Factory Method

**Definition:** Инкапсулируйте создание объекта, чтобы вызывающий объект не знал имен полей, значений по умолчанию, дополнительных поисков или того, является ли результат простым объектом или `new Class()`.

**How it works:**

**Простая фабрика** — это функция, которая возвращает объекты одинаковой формы (`createGuitar(6)`, `createSpending(amount)`). **Фабричный метод** (GoF) откладывается `new` в подкласс. Оба хранят правила построения в одном месте — если меняется схема продукта, меняется только фабрика.

Doka: используйте фабрику, когда создание занимает более 1–2 строк, особенно если вы сначала что-то **вычисляете** (дерево грифа из максимального веса). Сайты звонков остаются `createGuitar(6)` даже если ты позже вернешься `new Guitar(...)`.

**When to use:**

Логика построения/умолчания/проверка; много похожих объектов; `createParser('json')`; тестовые двойки. Пропустить `{ id }` литералы.

#### Common pitfalls

Фабричная упаковка каждого предмета («шаблональная лихорадка»). Путаница простой фабрики с фабричным методом/абстрактной фабрикой.

**Pluses and minuses:**

| +                                                                      | −                                                         |
| ---------------------------------------------------------------------- | --------------------------------------------------------- |
| Скрывает сложность конструкции                                         | Дополнительная косвенность                                |
| Централизует настройки по умолчанию (например. `dateTime: Date.now()`) | Чрезмерная фабрикация простых DTO                         |
| Меньше опечаток, чем при дублировании литералов объектов               | Нет `instanceof` если вы не поделитесь прототипом/`class` |

**Examples:**

```js
// 1) Construction logic — Doka: compute a field, hide the rest of the recipe
function createGuitar(stringsCount = 6, maxWeight = 5) {
  const fretBoardMaterial = maxWeight <= 5 ? "fir" : "cedar";
  return {
    stringsCount,
    fretsCount: 24,
    fretBoardMaterial,
    boardMaterial: "maple",
  };
}
createGuitar(6); // light guitar → fir
createGuitar(7, 9); // heavy → cedar
```

```js
// 2) Many similar objects — defaults (timestamp, kind) in one place
function createSpending(amount) {
  return { amount, dateTime: Date.now(), kind: "spending" };
}
const spending1 = createSpending(250);
const spending2 = createSpending(100);
```

```js
// 3) Pick implementation by type — simple factory (not Factory Method)
function createParser(kind) {
  if (kind === "json") return (text) => JSON.parse(text);
  if (kind === "csv") return (text) => text.split(",").map((s) => s.trim());
  throw new Error(`Unknown parser: ${kind}`);
}
createParser("json")('{"a":1}'); // { a: 1 }
createParser("csv")("a, b"); // ['a', 'b']
```

```js
// 4) Test double — same factory, fake in tests
function createLogger(env) {
  if (env === "test") return { info: () => {} };
  return { info: (msg) => console.log(msg) };
}
createLogger("prod").info("up");
createLogger("test").info("silent");
```

```js
// 5) Factory Method (GoF) — subclass decides which class is born
class Department {
  hire(id) {
    const employee = this.createEmployee(id);
    employee.start();
    return employee;
  }
  createEmployee(id) {
    throw new Error("override me");
  }
}
class ITDepartment extends Department {
  createEmployee(id) {
    return { id, role: "programmer", start() {} };
  }
}
class AccountingDepartment extends Department {
  createEmployee(id) {
    return { id, role: "accountant", start() {} };
  }
}
new ITDepartment().hire(1).role; // 'programmer'
new AccountingDepartment().hire(2).role; // 'accountant'
```

**Thoughts:**

Заводов достаточно для небольших объектов. Вырасти в `class` когда тебе нужно `this` или методы, которые должны оставаться на объекте. См. [Factory patterns](./3_js_objects.md#factory-and-module-patterns).

Знание того, _как_ строить жизнь в одной функции.

**Sources:**

[Doka — Шаблоны создания](https://doka.guide/tools/design-patterns-creational/) · [Doka — ООП](https://doka.guide/tools/oop/) · [Гуру рефакторинга — Сравнение фабрик](https://refactoring.guru/design-patterns/factory-comparison)

---

</details>

<details>
  <summary>Abstract Factory</summary>

<a name="abstract-factory"></a>

### Abstract Factory

**Definition:** Интерфейс для создания **семейства** связанных продуктов без указания конкретных классов — «фабрика фабрик». Он возвращает абстракции (`Instrument`, `Musician`), ни одного жестко запрограммированного типа.

**How it works:**

Долевой договор (`ReservationFactory { reserveInstrument(); notifyPlayer(); }`). Каждая семья (скрипка, виолончель) выполняет этот контракт. Клиент `reserve(factory)` никогда `instanceof`с. Добавить альт → новый класс, а не новую ветку в `reserve()`.

Гуру рефакторинга: одна фабрика производит соответствующие продукты (кнопка Windows + флажок Windows). Сложнее добавить _новый тип продукта_ (каждая фабрика приобретает свой метод).

**When to use:**

Связанные, но неидентичные объекты с одинаковыми этапами создания (зарезервировать + уведомить; тематические наборы пользовательского интерфейса).

#### Common pitfalls

Типы союзов + `instanceof` вместо общего заводского интерфейса — хрупкого, когда оркестр растет.

**Pluses and minuses:**

| +                                      | −                                                     |
| -------------------------------------- | ----------------------------------------------------- |
| Совместимые семейства продуктов        | Множество интерфейсов/классов                         |
| Клиентский код остается на абстракциях | Добавление нового вида продукции затронет все фабрики |
| Нет `instanceof` деревья               | Перебор для одного типа продукта                      |

**Examples:**

```js
// 1) Orchestra family — same API, different products. Client has no instanceof.
class Violin {
  playNote(note) {
    return `violin ${note}`;
  }
}
class Cello {
  playNote(note) {
    return `cello ${note}`;
  }
}
class Violinist {
  instrument = new Violin();
  play(piece) {
    return piece.map((n) => this.instrument.playNote(n));
  }
}
class Cellist {
  instrument = new Cello();
  play(piece) {
    return piece.map((n) => this.instrument.playNote(n));
  }
}

class ViolinReservation {
  reserveInstrument() {
    return new Violin();
  }
  notifyPlayer() {
    return new Violinist();
  }
}
class CelloReservation {
  reserveInstrument() {
    return new Cello();
  }
  notifyPlayer() {
    return new Cellist();
  }
}

function reserve(factory) {
  const player = factory.notifyPlayer();
  factory.reserveInstrument();
  return player;
}
reserve(new ViolinReservation()).play(["A", "C"]); // ['violin A', 'violin C']
reserve(new CelloReservation()).play(["A", "C"]); // ['cello A', 'cello C']
```

```js
// 2) Themed UI kit — button + input stay in the same family (dark vs light)
const lightUi = {
  button: (label) => ({ kind: "button", theme: "light", label }),
  input: (name) => ({ kind: "input", theme: "light", name }),
};
const darkUi = {
  button: (label) => ({ kind: "button", theme: "dark", label }),
  input: (name) => ({ kind: "input", theme: "dark", name }),
};

function loginForm(ui) {
  return { submit: ui.button("Log in"), email: ui.input("email") };
}
loginForm(lightUi).submit.theme; // 'light'
loginForm(darkUi).email.theme; // 'dark'
```

**Thoughts:** Вы создаете _типы продуктов_, а не `new Violin()` в клиенте.

**Sources:**

[Doka — Шаблоны создания](https://doka.guide/tools/design-patterns-creational/) · [Гуру рефакторинга — Абстрактная фабрика](https://refactoring.guru/design-patterns/abstract-factory)

---

</details>

<details>
  <summary>Builder</summary>

<a name="builder"></a>

### Builder

**Definition:** Создайте сложный объект **шаг за**, используя дополнительные части, а затем `build()`. Один и тот же процесс построения может привести к разным представлениям.

**How it works:**

Строитель хранит настройки. Каждый `addX()` / `setX()` мутирует и ** возвращается `this`** (свободная цепь). `build()` производит продукт. Гуру рефакторинга: убивает **телескопические конструкторы** (`Pizza(size)`, `Pizza(size, cheese)`, `Pizza(size, cheese, pepperoni)`…). Директор (необязательно) выполняет фиксированную последовательность шагов.

В отличие от других творческих шаблонов, Builder может создавать продукты, которые **не имеют общего** одного интерфейса (автомобиль и руководство по одним и тем же этапам).

**When to use:**

Множество дополнительных ингредиентов/полей (кофейный напиток, HTTP-запрос, запрос, тестовые данные). Процесс разбивается на именованные шаги.

#### Common pitfalls

Строитель, когда достаточно фабрики с двумя аргументами. Забывчивость `return this` разрывает цепочку.

**Pluses and minuses:**

| +                                            | −                                                                |
| -------------------------------------------- | ---------------------------------------------------------------- |
| Читаемая дополнительная конфигурация         | Подробное описание для объектов с двумя полями                   |
| Избегает конструкторов с логическими флагами | Изменяемый конструктор при повторном использовании без `reset()` |
| Те же шаги, разные продукты                  |                                                                  |

**Examples:**

```js
// 1) Coffee drink — optional ingredients, fluent chain (Doka)
class DrinkBuilder {
  settings = { base: "espresso" };
  addMilk() {
    this.settings.milk = true;
    return this;
  }
  addSugar() {
    this.settings.sugar = true;
    return this;
  }
  addCream() {
    this.settings.cream = true;
    return this;
  }
  build() {
    return { ...this.settings };
  }
}
const latte = new DrinkBuilder().addMilk().build();
const extra = new DrinkBuilder().addSugar().addCream().build();
```

```js
// 2) HTTP request — skip unused options instead of fetch(url, { method, headers, body, … })
class RequestBuilder {
  #url = "";
  #init = { method: "GET", headers: {} };
  url(value) {
    this.#url = value;
    return this;
  }
  method(value) {
    this.#init.method = value;
    return this;
  }
  header(key, value) {
    this.#init.headers[key] = value;
    return this;
  }
  json(body) {
    this.#init.body = JSON.stringify(body);
    return this.header("Content-Type", "application/json").method("POST");
  }
  build() {
    return { url: this.#url, ...this.#init };
  }
}
new RequestBuilder().url("/api/users").json({ name: "Ada" }).build();
// { url: '/api/users', method: 'POST', headers: { 'Content-Type': 'application/json' }, body: '{"name":"Ada"}' }
```

```js
// 3) Query / test data — named steps, no telescoping constructor
class UserBuilder {
  #user = { name: "anon", role: "user", active: true };
  name(value) {
    this.#user.name = value;
    return this;
  }
  admin() {
    this.#user.role = "admin";
    return this;
  }
  inactive() {
    this.#user.active = false;
    return this;
  }
  build() {
    return { ...this.#user };
  }
}
new UserBuilder().name("Ada").admin().build();
// { name: 'Ada', role: 'admin', active: true }
```

**Thoughts:** Сначала шаги, потом объект — `build()` единственный `new`.

**Sources:**

[Doka — Шаблоны создания](https://doka.guide/tools/design-patterns-creational/) · [Гуру рефакторинга — Builder](https://refactoring.guru/design-patterns/builder)

---

</details>

<details>
  <summary>Strategy</summary>

<a name="strategy"></a>

### Strategy

**Definition:** Меняйте **как** что-то делается — без гигантского `if/else`. Каждый вариант — отдельная стратегия; вызывающий код выбирает одну.

**Simple idea:** Оплата картой или PayPal — одна кнопка «Оплатить», разная логика внутри.

**How it works:**

1. Общая форма (например, `calculate(price)`).
2. Каждый вариант — своя функция/класс.
3. Контекст вызывает `strategy.calculate(price)` — без `switch` внутри checkout.

| vs [State](#state) | Strategy                  | State                  |
| ------------------ | ------------------------- | ---------------------- |
| Кто выбирает       | Вы, снаружи               | Объект переключает сам |
| Пример             | скидка `vip` vs `regular` | заказ: draft → sent    |

**When to use:** Способы оплаты, скидки, сортировка, валидаторы — везде, где пишете `if (type === 'a') … else if …`.

**Pluses and minuses:**

| +                                     | −                                   |
| ------------------------------------- | ----------------------------------- |
| Новый вариант без правки старого кода | Больше мелких функций/классов       |
| Подмена в runtime                     | Избыточно для 2 фиксированных веток |

**Examples:**

```js
// 1) Object map — выбор по ключу (самый частый вариант в JS)
const discounts = {
  regular: (price) => price,
  vip: (price) => price * 0.8,
  sale: (price) => price * 0.5,
};

function checkout(price, customerType) {
  return discounts[customerType](price);
}

checkout(100, "vip"); // 80
checkout(100, "sale"); // 50
```

```js
// 2) То же с платёжными провайдерами
const pay = {
  stripe: (amount) => chargeStripe(amount),
  paypal: (amount) => chargePayPal(amount),
};

pay[provider](total); // без if-цепочки в вызывающем коде
```

**Thoughts:** Объект **сам меняет режим** → [State](#state). **Вы** выбираете алгоритм → Strategy.

**Sources:** [Refactoring Guru — Strategy](https://refactoring.guru/design-patterns/strategy) · [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)

---

</details>

<details>
  <summary>Singleton (caution)</summary>

<a name="singleton-caution"></a>

### Singleton (caution)

**Definition:** Шаблон создания, обеспечивающий существование **только одного экземпляра**; более поздняя попытка создания возвращает существующий объект. Предоставляет глобальную точку доступа.

**How it works:**

- Типичная форма класса: статический частный `#instance`; конструктор возвращает существующий экземпляр (Doka `Sun`) или частный актер + `getInstance()`.
- В JS **область модуля** (`export const db = createPool()`), естественно, дает один экземпляр для каждого процесса — идиоматический синглтон без церемоний.
- Риск: скрытое глобальное изменяемое состояние, общее для всех тестов и запросов. Refactoring Guru: те же недостатки, что и у глобальных переменных — менее модульные, модульные тесты требуют наличия синглтона.

Doka: может нарушать **открытие/закрытие**, увеличивать связанность — иногда рассматривается как антишаблон. Отдайте предпочтение дизайну, который в этом не нуждается.Шаблон Singleton (ограниченное количество) и DI **время существования синглтона** (один и тот же экземпляр внедряется в зависимые объекты). Различные механизмы (Дока).

**When to use:** Редко во внешнем интерфейсе — предпочитают область видимости модуля или контейнер DI. Сервер: пул БД, конфигурация, логгер. **Избегайте** одиночных элементов бизнес-логики с изменяемым состоянием сеанса. Домен «только одно Солнце» — это учебник, да, но все же подумайте дважды.

**Pluses and minuses:**

| +                                       | −                                                   |
| --------------------------------------- | --------------------------------------------------- |
| Контролируемая глобальная точка доступа | Скрытое глобальное состояние — трудно проверить     |
| Ленивая инициализация экономит ресурсы  | Плотная привязка к конкретному экземпляру           |
| Один пул соединений/объект конфигурации | Проблемы параллелизма в многопоточных средах (Java) |

| Нарушает DIP, если вызывающие абоненты не могут ввести альтернативы |

**Examples:**

```js
// 1) Domain uniqueness — Doka Sun: later new returns the same object
class Sun {
  static #instance = null;
  constructor() {
    if (Sun.#instance) return Sun.#instance;
    Sun.#instance = this;
  }
}
const a = new Sun();
const b = new Sun();
a === b; // true
```

```js
// 2) Idiomatic JS — one module export per process (config / logger / pool)
// config.js
export const config = Object.freeze({ apiUrl: process.env.API_URL ?? "/api" });
// importers share the same binding — no class Singleton needed
```

```js
// 3) Logger registry — one shared object; still prefer injecting it in tests
class Logger {
  static #instance = null;
  #lines = [];
  constructor() {
    if (Logger.#instance) return Logger.#instance;
    Logger.#instance = this;
  }
  info(msg) {
    this.#lines.push(msg);
  }
  dump() {
    return this.#lines.slice();
  }
}
new Logger().info("boot");
new Logger().dump(); // ['boot'] — same instance
```

```js
// 4) Not the same as DI singleton — container reuses one instance when injecting
function createApp({ logger = new Logger() } = {}) {
  return { log: (msg) => logger.info(msg) };
}
const logger = new Logger();
const appA = createApp({ logger });
const appB = createApp({ logger });
// both apps share logger because we passed it — not because of a global getInstance()
```

**Thoughts:**

Класс GoF Singleton часто является **анти-шаблоном** в тестах — вместо этого внедряйте зависимости. Интервью: главное возражение — проверяемость.

Один экземпляр — это последнее средство, а не значение по умолчанию.

**Sources:**

[Doka — Шаблоны создания](https://doka.guide/tools/design-patterns-creational/) · [Гуру рефакторинга — Singleton](https://refactoring.guru/design-patterns/singleton)

---

</details>

<details>
  <summary>Facade</summary>

<a name="facade"></a>

### Facade

**Definition:** Структурный шаблон, обеспечивающий **упрощенный интерфейс** для сложной подсистемы — одна точка входа скрывает множество внутренних классов/API.

**How it works:**

- Звонки клиентов `facade.doThing()` вместо того, чтобы организовывать `auth()`, `retry()`, `parse()`, `cache()` сам.
- Фасад **делегирует** компонентам подсистемы — он не дублирует их логику.
- Можно объединить устаревшие API, сторонние SDK или вызовы микросервисов в один стабильный контракт.
- Риск: фасад превращается в объект-бог, если он накапливает бизнес-логику вместо делегирования.

Звонящие пишут `getUser('42')` or `heatWater(machine)` вместо повторения обновления токена + выборки + анализа или `turnOn` / `getWater` / ступеньки обогревателя. Фасад **владеет последовательностью**; кофемашина/стек HTTP остаются немыми.

**When to use:** `apiClient.getUser()` обновление аутентификации + выборка + повтор + анализ JSON; `paymentService.charge()` над Страйпом; кофемашина `heatWater()` над `turnOn` / `getWater` / `turnOnHeater`; Хуки React, объединяющие несколько операций чтения контекста.

**Pluses and minuses:**

| +                                                                     | −                                                |
| --------------------------------------------------------------------- | ------------------------------------------------ |
| Упрощенный API для потребителей                                       | Может стать фасадом бога, если закрадется логика |
| Скрывает устаревшие или сложные внутренние компоненты                 | Дополнительный слой для поддержания              |
| Стабильная общедоступная поверхность во время рефакторинга подсистемы | Может скрыть необходимый низкоуровневый контроль |

**Examples:**

```js
// Facade over fetch + auth + error handling
export async function getUser(id) {
  const token = await auth.refreshIfNeeded();
  const res = await fetch(`/api/users/${id}`, {
    headers: { Authorization: `Bearer ${token}` },
  });
  if (!res.ok) throw new ApiError(res.status);
  return res.json();
}

const user = await getUser("42");

// Same idea: many atomic SDK calls → one use case
function heatWater(machine) {
  machine.turnOn();
  while (machine.getWaterLevel() < 1000) machine.getWater();
  machine.turnOnHeater();
  if (machine.getTemperature() >= 90) machine.turnOffHeater();
}
```

**Thoughts:** Фасад ≠ Адаптер (изменяет интерфейс в соответствии с клиентом) или Прокси (управляет доступом). Фасад упрощает **многие** вызовы до **одного**. Это может сделать неудобный API удобным для использования, но это побочный эффект — работа заключается в инкапсуляции повторяющегося рабочего процесса. Не позволяйте этому стать объектом Бога.

**Sources:**

[refactoring.guru — Фасад](https://refactoring.guru/design-patterns/facade)

---

</details>

<details>
  <summary>Adapter</summary>

<a name="adapter"></a>

### Adapter

**Definition:** Структурный шаблон, преобразующий один интерфейс в другой, **ожидаемый клиентом**, — оборачивает несовместимый API, поэтому существующий код работает без изменений.

**How it works:**

- **Target** — интерфейс, который ожидает клиент.
- **Adaptee** — устаревший или сторонний API другой формы.
- **Адаптер** — переводит звонки: `target.method()` → `adaptee.oldMethod()` с сопоставлением аргументов/ответов.
- Адаптер объекта (композиция) против адаптера класса (наследование) — композиция предпочтительна в JS.

`fakeAPI()` является адаптируемым (вложенным `entries`, `user_name`, `ID`). `responseToWantedAdapter` это единственное место, которое знает это отображение. Остальная часть приложения получает `{ userName, email, id }[]` — если сервер переименовывает поле, вы меняете адаптер, а не каждый экран.

**When to use:** Сервер JSON использует `user_name` / `ID` пока пользовательский интерфейс хочет `camelCase`; обернуть старый REST при переходе на GraphQL; адаптировать узел `fs` обратные вызовы к обещаниям; нормализовать платежные системы, чтобы `PaymentResult`; `axios` адаптер для `fetch`-образный код.

**Pluses and minuses:**

| +                                                                | −                                                       |
| ---------------------------------------------------------------- | ------------------------------------------------------- |
| Повторное использование существующего кода с новыми интерфейсами | Тонкие адаптеры могут накапливаться                     |
| Чистая интеграция сторонних SDK                                  | Картографический слой может отставать от обновлений API |
| Изолировать наследие от современных модулей                      | Дополнительная косвенность                              |

**Examples:**

```js
function fakeAPI() {
  return {
    entries: [{ user_name: "Ada", email_address: "a@x.com", ID: "1" }],
  };
}

function responseToWantedAdapter(response) {
  return response.entries.map((entry) => ({
    userName: entry.user_name,
    email: entry.email_address,
    id: entry.ID,
  }));
}

const compatible = responseToWantedAdapter(fakeAPI());
// [{ userName: 'Ada', email: 'a@x.com', id: '1' }]
```

**Thoughts:** Адаптер меняет **интерфейс**; Декоратор добавляет **поведение** в тот же интерфейс. В чистой/шестиугольной архитектуре адаптеры занимают внешний уровень (DB, HTTP, UI), поэтому домен никогда не зависит от сторонних форм.

**Sources:**

[refactoring.guru — Адаптер](https://refactoring.guru/design-patterns/adapter)

---

</details>

<details>
  <summary>Decorator</summary>

<a name="decorator"></a>

### Decorator

**Definition:** Структурный шаблон, который **обертывает** объект для динамического добавления поведения без создания подклассов — тот же интерфейс, расширенная функциональность.

**How it works:**

- Декоратор реализует тот же интерфейс, что и обернутый объект.
- Делегирует внутренний объект, добавляя логику до/после (ведение журнала, кеширование, аутентификация).
- Сложите несколько декораторов: `withAuth(withCache(withRetry(api)))`.
- В React: **Компоненты высшего порядка (HOC)** и компоненты-обертки похожи на декораторы.

`loggingDecorator` это HOF: это занимает `update` и возвращается `wrapped`. `wrapped` logs, затем вызывает оригинал с теми же аргументами. Вы **не** редактировали `update`. Вы можете сложить больше оберток (`debounce(loggingDecorator(update))`) таким же образом.

**When to use:** Промежуточное программное обеспечение для ведения журналов/метрик; `memoize()`; Реагировать HOC `withAuth(Component)`; Промежуточное программное обеспечение Express/Redux; кэширование в репозиториях; **`throttle` / `debounce`** как оболочки функций ([JS Functions](./3_js_functions.md)).

**Pluses and minuses:**

| +                                                   | −                                                 |
| --------------------------------------------------- | ------------------------------------------------- |
| Открыто/закрыто — расширить без изменения исходного | Множество оберток → трудно отследить стек вызовов |
| Создание поведения во время выполнения              | Порядок декораторов имеет значение                |
| Альтернатива глубоким деревьям наследования         | Может скрыть то, что на самом деле работает       |

**Examples:**

```js
function loggingDecorator(fn) {
  return function wrapped(...args) {
    console.log(`log: ${args.join(", ")}`);
    return fn(...args);
  };
}

const user = { name: "Ada", email: "a@x.com" };
function update(name, email) {
  user.name = name;
  user.email = email;
}

const updateWithLogging = loggingDecorator(update);
updateWithLogging("Maria", "m@x.com");
// console: "log: Maria, m@x.com" — then user becomes { name: 'Maria', email: 'm@x.com' }
```

**Thoughts:** Машинопись `experimentalDecorators` Декораторы stage-3 формализуют это для классов — _шаблон_ более старый и работает как HOF в JS. Декоратор против прокси: декоратор — это явная оболочка, которая _добавляет_ поведение; Прокси GoF _закрывает_ оригинал. Укладка (`EncryptionDecorator(CompressionDecorator(file))`) — изображение GoF.

**Sources:**

[refactoring.guru — Декоратор](https://refactoring.guru/design-patterns/decorator)

---

</details>

<details>
  <summary>Proxy</summary>

<a name="proxy"></a>

### Proxy

**Definition:** Структурный шаблон, который предоставляет **суррогат** с **тот же интерфейс**, что и реальный объект, поэтому клиент взаимодействует с прокси-сервером, а прокси-сервер решает, когда (и нужно ли) достичь оригинала.

**How it works:**

- Клиент зависит от целевого интерфейса; прокси-сервер пересылает, задерживает, кэширует или отклоняет.
- Распространенные виды: **виртуальный** (отложенное создание), **защита** (аутентификация), **удаленный** (заглушка RPC), **кэширование**.
- В JS, `new Proxy(target, handler)` перехватывает операции через ловушки. `handler.get(target, property, receiver)` бежит дальше `proxy.foo`. Использовать `Reflect.get(target, property, receiver)` по умолчанию `[[Get]]` поведение (MDN).
- Исходный объект может остаться нетронутым; только прокси «странный».

Чтения проходят `get`. Ловушка блокируется `ssn` на **только прокси**. `Reflect.get` это значение по умолчанию `[[Get]]` (MDN), поэтому другие поля по-прежнему поступают из `original`. Это **защитный прокси**: те же ключи, закрытый доступ — не новый DTO (адаптер) и не дополнительное ведение журнала, обернутое функцией (декоратором).

**When to use:** Скрывать `ssn` на пользовательском объекте; ленивая инициализация тяжелого модуля; кэш `getUser`; проверьте условия перед вставкой узла DOM; Vue реактивность.

**Pluses and minuses:**

| +                                                                                       | −                                                                            |
| --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Ленивая загрузка, контроль доступа, кеш, журналирование без изменения реального объекта | Дополнительный хмель; отладка через ловушки сложнее                          |
| Прозрачно для вызывающих абонентов, которые уже используют целевой API                  | `Proxy` ловушки имеют инварианты (например, незаписываемые реквизиты данных) |
| Родной `Proxy` как читает треки Vue 3                                                   | Накладные расходы на горячие пути                                            |

**Examples:**

```js
const original = { name: "Maria", email: "hi@site.com", ssn: "000" };

const proxied = new Proxy(original, {
  get(target, prop, receiver) {
    if (prop === "ssn") throw new Error("Access denied");
    console.log(`get ${String(prop)}`);
    return Reflect.get(target, prop, receiver);
  },
});

proxied.name; // logs "get name", then 'Maria'
proxied.ssn; // throws 'Access denied'
original.ssn; // still '000' — the real object is not mutated
```

**Thoughts:** Прокси-сервер GoF не существует для _изменения публичного поведения продукта_, как это делает Декоратор — он _опосредует_. Демо-версия в стиле статьи, которая возвращает `'YOU HAVE BEEN PWND!'` ибо каждое поле — это перехват, а не типичный производственный прокси. Язык `Proxy` ≠ всегда шаблон проектирования.

**Sources:**

[refactoring.guru — Прокси](https://refactoring.guru/design-patterns/proxy) · [MDN `Proxy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) · [MDN `handler.get`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/get)

---

</details>

<details>
  <summary>Mediator</summary>

### Mediator

**Definition:** Один **посредник** маршрутизирует сообщения между частями. Компоненты говорят с посредником — не друг с другом.

**Simple idea:** Чат-комната — вы шлёте сообщение в комнату, комната доставляет остальным. Пользователям не нужны ссылки на каждого.

**How it works:**

1. Компоненты регистрируются у посредника.
2. Компонент A вызывает `mediator.notify("message", data)`.
3. Посредник решает, кому передать, и пересылает.

| vs [Observer](#observer) | Mediator              | Observer                         |
| ------------------------ | --------------------- | -------------------------------- |
| Форма                    | Хаб между участниками | Один источник → много слушателей |
| Пример                   | Чат-комната           | `store.subscribe`                |

**When to use:** Чат, сложные формы со связанными полями, Redux store (actions → reducers), NestJS event bus.

**Pluses and minuses:**

| +                                   | −                                |
| ----------------------------------- | -------------------------------- |
| Нет спагетти A→B, A→C, B→C          | Посредник может стать god-object |
| Одно место для правил маршрутизации | Дополнительный слой              |

**Examples:**

```js
// Чат-комната — посредник доставляет сообщения
const chatRoom = {
  users: [],

  join(user) {
    this.users.push(user);
  },

  send(from, text) {
    for (const user of this.users) {
      if (user !== from) user.onMessage(text);
    }
  },
};

const alice = {
  name: "Alice",
  onMessage(t) {
    console.log("Alice got:", t);
  },
};
const bob = {
  name: "Bob",
  onMessage(t) {
    console.log("Bob got:", t);
  },
};

chatRoom.join(alice);
chatRoom.join(bob);
chatRoom.send(alice, "Hi!"); // Bob got: Hi!
```

**Thoughts:** Много ко многим → Mediator. Одно изменилось, много слушают → [Observer](#observer).

**Sources:** [Refactoring Guru — Mediator](https://refactoring.guru/design-patterns/mediator)

</details>

<details>
  <summary>Chain of Responsibility</summary>

<a name="chain-of-responsibility"></a>

### Chain of Responsibility

**Definition:** Запрос идёт по цепочке обработчиков. Каждый говорит **«не моё, дальше»** или **«я обработал, стоп»**.

**Simple idea:** Тикет в поддержку: сначала бот → не помог → junior → не помог → senior.

**How it works:**

1. Запрос попадает к обработчику A.
2. A обрабатывает **или** передаёт B.
3. Останавливается, когда кто-то обработал (или цепочка кончилась).

**Классический CoR** = первое совпадение. **Middleware** (Express `next()`) = каждый слой выполняется — похожая идея, другое правило остановки.

**When to use:** Набор обработчиков неизвестен заранее или их нужно подключать без переписывания диспетчера. Если всегда одни и те же 3 случая в одном файле — хватит `switch`.

**Pluses and minuses:**

| +                                        | −                                      |
| ---------------------------------------- | -------------------------------------- |
| Новые обработчики без правок вызывающего | Запрос может дойти до конца без ответа |
| Каждый обработчик — одна задача          | Порядок важен — легко ошибиться        |

**Examples:**

```js
// Обработчики как функции — передать next, если не можешь обработать
function usdHandler(currency, amount, next) {
  if (currency === "USD") return console.log(`Charged $${amount}`);
  return next?.(currency, amount);
}

function eurHandler(currency, amount, next) {
  if (currency === "EUR") return console.log(`Charged €${amount}`);
  return next?.(currency, amount);
}

function run(currency, amount) {
  usdHandler(currency, amount, (c, a) =>
    eurHandler(c, a, () => console.log("No handler for", c)),
  );
}

run("USD", 50); // Charged $50
run("GBP", 50); // No handler for GBP
```

**Thoughts:** Middleware запускает **все** слои (`next()`). Классический CoR останавливается на **первом** совпадении. См. [Behavioral cheat sheet](#behavioral-patterns--cheat-sheet).

**Sources:** [Doka — Behavioral](https://doka.guide/tools/design-patterns-behaviorial/) · [Refactoring Guru — Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)

</details>

<details>
  <summary>Command</summary>

<a name="command"></a>

### Command

**Definition:** Оберните действие в **объект**, чтобы выполнить позже, поставить в очередь, залогировать или **отменить**.

**Simple idea:** Ctrl+Z в редакторе — каждое действие сохранено как команда; undo откатывает последнее.

**How it works:**

| Часть        | Задача                                          |
| ------------ | ----------------------------------------------- |
| **Command**  | `{ do(), undo() }` — действие и его откат       |
| **Invoker**  | Кнопка, меню, очередь — вызывает `do()`         |
| **Receiver** | То, что реально меняется (редактор, API, store) |

Не [Observer](#observer) (постоянная подписка). Не [Strategy](#strategy) (выбор алгоритма — Command это одно конкретное действие).

**When to use:** Undo/redo, очереди задач, audit log, Redux actions.

**Pluses and minuses:**

| +                                      | −                                 |
| -------------------------------------- | --------------------------------- |
| Undo и история «бесплатно»             | Лишние объекты для простых кликов |
| Можно отложить или поставить в очередь | Легко переусложнить               |

**Examples:**

```js
// 1) Стек undo — суть в простом JS
const history = [];
const editor = { text: "" };

const addHello = {
  do() {
    editor.text += "hello";
  },
  undo() {
    editor.text = editor.text.slice(0, -5);
  },
};

function run(cmd) {
  cmd.do();
  history.push(cmd);
}

function undo() {
  history.pop()?.undo();
}

run(addHello); // editor.text = "hello"
undo(); // editor.text = ""
```

```js
// 2) Redux — action это данные команды; reducer — receiver
const action = { type: "SET_NAME", payload: "Alex" };

function reducer(state, action) {
  if (action.type === "SET_NAME") return { ...state, name: action.payload };
  return state;
}

reducer({ name: "" }, action); // { name: "Alex" }
```

**Thoughts:** Redux `dispatch(action)` = отправить команду. Reducer делает работу. Контраст: [Observer](#observer) (много слушателей, постоянно) и [Chain of Responsibility](#chain-of-responsibility) (передача до обработки).

**Sources:** [Doka — Behavioral](https://doka.guide/tools/design-patterns-behaviorial/) · [Refactoring Guru — Command](https://refactoring.guru/design-patterns/command)

</details>

<details>
  <summary>State</summary>

<a name="state"></a>

### State

**Definition:** Объект **ведёт себя по-разному** в зависимости от текущего режима. Вместо `if (status === 'draft') … else if …` — отдельный объект на каждый режим.

**Simple idea:** Заказ в `draft` → можно отправить. Уже `sent` → отправить снова нельзя. Одна кнопка, разные правила по режиму.

**How it works:**

1. Объект хранит `state` (текущий режим).
2. Методы вызывают `this.state.submit()` — не большой switch.
3. Объект-состояние решает, что произойдёт **и** какой режим дальше.

| vs [Strategy](#strategy) | State                  | Strategy              |
| ------------------------ | ---------------------- | --------------------- |
| Кто выбирает             | Объект переключает сам | Вы выбираете снаружи  |
| Пример                   | draft → sent → done    | скидка vip vs regular |

**When to use:** Жизненный цикл заказа, медиаплеер (play/pause/stop), шаги wizard, статус соединения.

**Pluses and minuses:**

| +                                    | −                               |
| ------------------------------------ | ------------------------------- |
| Нет гигантского `if/else` по статусу | Больше объектов на каждый режим |
| Недопустимые действия очевидны       | Избыточно для 2 простых флагов  |

**Examples:**

```js
// 1) Заказ — каждое состояние знает, что можно
const sent = {
  submit() {
    throw new Error("Already sent");
  },
};

const draft = {
  submit(order) {
    order.state = sent;
    console.log("Sent to warehouse");
  },
};

const order = { state: draft };

order.state.submit(order); // ok → sent
order.state.submit(order); // throws: Already sent
```

```js
// 2) Светофор — поведение зависит от цвета
const green = { next: () => yellow };
const yellow = { next: () => red };
const red = { next: () => green };

let light = green;
light = light.next(); // yellow
light = light.next(); // red
```

**Thoughts:** Объект **сам меняет режим** → State. **Вы** выбираете алгоритм → [Strategy](#strategy).

**Sources:** [Refactoring Guru — State](https://refactoring.guru/design-patterns/state) · [Doka — Behavioral patterns](https://doka.guide/tools/design-patterns-behaviorial/)

</details>

<details>
  <summary>Repository</summary>

### Repository

**Definition:** Шаблон, который **посредствует между логикой предметной области и сопоставлением данных** — интерфейс, похожий на коллекцию, для объектов предметной области, скрывающий детали персистентности (SQL, ORM, API).

**How it works:**

- Вызовы кодов домена/услуги `userRepo.findById(id)` — а не сырой SQL или Prisma напрямую.
- Репозиторий реализует запросы CRUD + домена (`findByEmail`, `save`).
- Заменяемые серверные части: репозиторий Postgres, репозиторий в памяти для тестов, репозиторий ложного API.
- Часто по одному репозиторию на совокупный корень (Пользователь, Заказ).

**When to use:** Сервисный уровень в бэкэнде; модули доступа к внешним данным (`userApi` с последовательным интерфейсом); тестирование бизнес-правил без базы данных; Обертки репозитория Prisma/TypeORM.

**Pluses and minuses:**

| +                                                 | −                                                     |
| ------------------------------------------------- | ----------------------------------------------------- |
| Тестируемая доменная логика с подделками в памяти | Дополнительная абстракция над ORM                     |
| Централизованная логика запросов                  | Универсальные репозитории становятся объектами-богами |
| Технология стойкости может измениться             | Сопоставление объекта ↔ Заголовок строки БД           |

**Examples:**

```ts
type User = { id: string; email: string };
type UserRepository = {
  findById(id: string): Promise<User | null>;
  save(user: User): Promise<void>;
};

class InMemoryUserRepo implements UserRepository {
  #store = new Map<string, User>();
  async findById(id) {
    return this.#store.get(id) ?? null;
  }
  async save(user) {
    this.#store.set(user.id, user);
  }
}

async function inviteUser(email: string, repo: UserRepository) {
  const existing = await repo.findById(email); // domain doesn't know storage
  if (existing) throw new Error("Already exists");
  await repo.save({ id: crypto.randomUUID(), email });
}
```

**Thoughts:** Сочетается с ДИП. Не оборачивайте каждый вызов ORM вслепую — репозиторий хорош, когда логика предметной области богата, а постоянство варьируется (тесты, уровень кэширования, несколько источников).

**Sources:**

[refactoring.guru — Репозиторий](https://refactoring.guru/design-patterns/repository)

---

</details>
