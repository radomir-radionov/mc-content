# JS Core — Variables

`let` / `const` / `var`, область видимости, hoisting, `globalThis` и деструктуризация (rest/spread).

[← К оглавлению JS](./3_js.md)

### Related docs

- [Переменные](./3_js_variables.md) · [Замыкания](./3_js_functions.md#closures) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Функции JS](./3_js_functions.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Язык JS](./3_js_core.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Область      | Темы                                |
| ------------ | ----------------------------------- |
| **Привязки** | `let` / `const` / `var`, именование |
| **Область**  | Global, function, block             |
| **Hoisting** | TDZ, объявления функций             |
| **Паттерны** | Rest, spread, деструктуризация      |

### Sections

- [Bindings & scope](#bindings-and-scope)
- [Rest, spread & destructuring](#rest-spread-and-destructuring)
- **Bindings & scope:** [globalThis](#globalthis) · [Variables](#variables) · [Scope](#scope) · [Hoisting](#hoisting)
- **Rest, spread & destructuring:** [Rest](#rest) · [Spread](#spread) · [Destructuring](#destructuring)
- [Интервью](./3_js_interview.md)

Соседние темы Core: [Язык](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массив](./3_js_array.md).

<a name="scope-closures"></a>

## Bindings & scope

### Bindings & scope

<details>
  <summary>globalThis</summary>

### globalThis

**Definition:** Стандартный способ доступа к глобальному объекту в любой среде (браузер `window`, Узел `global`, рабочие `self`).

**How it works:**

- `globalThis` — это один и тот же объект в разных областях данной среды JS.
- В браузерах глобальное также `window`; в узле `global`. Предпочитать `globalThis` для переносимого кода.
- `var` объявления глобалов и функций в классических скриптах становятся свойствами глобального объекта; `let`/`const` не\*\*.
- В строгом режиме/модули голые `this` на верхнем уровне есть `undefined` - использовать `globalThis` когда вам действительно нужен глобальный.
- Предпочитайте область действия модуля или явный экспорт, а не загрязняющий глобальный контент; прикрепляйте только настоящие полифилы (`if (!globalThis.Promise) ...`).

**When to use:** Комплекты библиотеки UMD, тестовая установка, редкие полифилы. Не прикрепляйте состояние приложения к `globalThis`.

**Pluses and minuses:**

| +                        | −                                    |
| ------------------------ | ------------------------------------ |
| Межсредовый API          | Анти-паттерн глобального загрязнения |
| Совместимость с Polyfill |                                      |

**Examples:**

```js
globalThis.setTimeout(fn, 0); // works in browser and Node

// classic script only — modules don't put let on global:
// var gVar = 5; → window.gVar === 5
// let gLet = 5; → window.gLet === undefined
```

**Sources:** [MDN globalThis](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) · [javascript.info/global-object](https://javascript.info/global-object)

</details>

<a name="variables"></a>

<details>
  <summary>Variables</summary>

### Variables

**Definition:** **Variables** — именованные привязки (`let`, `const`, `var`) в лексической среде. `const` запрещает перепривязку переменной; содержимое объектов и массивов остаётся изменяемым.

**How it works:**

|                         | `var`       | `let` | `const`      |
| ----------------------- | ----------- | ----- | ------------ |
| Scope                   | function    | block | block        |
| Hoisting                | `undefined` | TDZ   | TDZ          |
| Redeclare in same scope | yes         | no    | no           |
| Reassign                | yes         | yes   | no (binding) |

Область видимости → [Scope](#scope). Hoisting и TDZ → [Hoisting](#hoisting).

**When to use:**

| Использование                                        | Избегайте                                   |
| ---------------------------------------------------- | ------------------------------------------- |
| `const` по умолчанию                                 | `var` в новом коде                          |
| `let` при перепривязке (`for` счётчик, аккумуляторы) | `const` для значений, которые переназначите |
| Блочная область в `if`/`for` для временных привязок  | Опора на hoisting для логики                |

**Naming:**

- Предпочитайте camelCase (`userName`, `shoppingCart`); буквы, цифры, `$`, `_`; имя не с цифры.
- Объявляйте привязку **один раз** — повторный `let` / `const` с тем же именем в одной области — `SyntaxError`.
- **Верхний регистр `CONST_NAME`:** только для констант, известных до запуска (`COLOR_RED = "#F00"`). Вычисляемые во время выполнения — camelCase (`const age = someCode(birthday)`).

```js
const COLOR_ORANGE = "#FF7F00"; // hard-coded → UPPER_SNAKE
const pageLoadTime = performance.now(); // computed once → camelCase
```

**Pluses and minuses:**

| + (`const`/`let`)                     | − (`var`)                            |
| ------------------------------------- | ------------------------------------ |
| Блочная область ограничивает утечки   | Область `var` игнорирует `{}`        |
| TDZ ловит использование до объявления | Поднятый `undefined` скрывает ошибки |
| Нет случайного повторного объявления  | Замыкания в цикле с `var` + async    |

**Definition (`var` loop pitfall vs `let`):** `var` в `for` создаёт одну привязку на весь цикл; `let` — новую на каждую итерацию.

**How it works:**

- `var i` — одна привязка на функцию; все замыкания видят одно и то же `i` после цикла.
- `let i` — блочная привязка на итерацию; каждый async callback захватывает свой `i`.

**When to use:** Всегда `let` в `for` с замыканиями; `for...of`, если индекс не нужен.

**Examples:**

```js
// var — one shared binding
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); // 3, 3, 3
}

// let — new binding per iteration
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0); // 0, 1, 2
}
```

**Sources:** [javascript.info/variables](https://javascript.info/variables) · [javascript.info/let-const](https://javascript.info/let-const)

</details>

<a name="scope"></a>

<details>
  <summary>Scope</summary>

### Scope

**Definition:** **Scope** определяет, где видна привязка. Три уровня: global, function и block.

**How it works:**

- У каждой области (global, function, block) — Environment Record с привязками.
- Внутренние области ссылаются на внешние через `[[OuterEnv]]` — цепочка для [замыканий](./3_js_functions.md#closures).
- **Global** — верхний уровень модуля/скрипта; в браузере `var` также на `window`.
- **Function** — параметры и `var`/`let`/`const` в теле функции.
- **Block** — `let`/`const` в `{}`, `if`, `for`, `while`.

**When to use:** Блочная область для временных привязок; function scope только через `var` (legacy).

| Scope    | Created by                        | Example                         |
| -------- | --------------------------------- | ------------------------------- |
| Global   | Script/module top level           | `const API_URL = '...'`         |
| Function | `function` declaration/expression | `function f() { const x = 1; }` |
| Block    | `{ }` with `let`/`const`          | `if (ok) { const tmp = 1; }`    |

**Examples:**

```js
const global = 1;

function outer() {
  const local = 2;
  if (true) {
    const block = 3;
    // block visible here only
  }
  // console.log(block); // ReferenceError
}
```

**Pluses and minuses:**

| + (block scope)                     | −                                       |
| ----------------------------------- | --------------------------------------- |
| Ограничивает время жизни переменной | Нужно выбрать правильное ключевое слово |
| Меньше утечек из `if`/`for`         |                                         |

**Sources:** [javascript.info/closure](https://javascript.info/closure)

</details>

<a name="hoisting"></a>

<details>
  <summary>Hoisting</summary>

### Hoisting

**Definition:** **Hoisting** регистрирует объявления до выполнения строки. `var` поднимается как `undefined`; `let`/`const` в **TDZ** до инициализации. **Объявления функций** поднимаются целиком; **выражения** — нет.

**How it works:**

- Фаза разбора записывает привязки; фаза выполнения присваивает значения.
- `var` с начала области — `undefined`.
- Доступ к `let`/`const` до объявления → `ReferenceError` (TDZ).
- `typeof undeclaredVar` безопасен; `typeof letBeforeDeclare` в TDZ бросает.

Каждый вызов функции получает контекст (среду, `this`, кадр стека). **Лексическая среда** задаёт внешние привязки для замыканий — по **месту записи кода**, не вызова.

**When to use:** Не используйте переменные до объявления; для поднятых хелперов — function declaration.

| Declaration            | Hoisted as          |
| ---------------------- | ------------------- |
| `var`                  | `undefined`         |
| `function` declaration | Full function       |
| `let`/`const`          | TDZ until line runs |

**Examples:**

```js
sayHi(); // works — declaration hoisted
function sayHi() {
  console.log("Hi");
}

console.log(a); // undefined — var hoisted
var a = 1;

// console.log(b); // ReferenceError — TDZ
let b = 2;
```

**Pluses and minuses:**

| + (TDZ)                   | −                                    |
| ------------------------- | ------------------------------------ |
| Раннее обнаружение ошибок | Удивляет разработчиков других языков |

**Sources:** [javascript.info/variables](https://javascript.info/variables) · [javascript.info/let-const](https://javascript.info/let-const)

</details>

## Rest, spread & destructuring

### Rest, spread & destructuring

<a name="rest"></a>

<details>
  <summary>Rest</summary>

### Rest

**Definition:** **Rest** (`...name`) собирает оставшиеся элементы массива или свойства объекта в новый массив/объект. В функциях **rest-параметры** собирают лишние аргументы в массив.

**How it works:**

- В деструктуризации rest должен быть **последним** в паттерне.
- **Поверхностная** копия «остатка» — вложенные объекты остаются общими.
- Rest-параметры заменяют устаревший `arguments`.
- Тот же токен `...`, что и у spread; смысл задаёт позиция: собрать vs развернуть.

**When to use:**

| Особенность     | Используйте, когда                                    |
| --------------- | ----------------------------------------------------- |
| Rest в массивах | `const [head, ...tail] = items`                       |
| Rest в объектах | Исключить поля (`const { password, ...safe } = user`) |
| Rest-параметры  | Вариадические функции (`sum(...nums)`)                |

**Examples:**

```js
const [first, second, ...rest] = [1, 2, 3, 4];

function sum(...nums) {
  return nums.reduce((acc, n) => acc + n, 0);
}

const { id, ...metadata } = { id: 1, a: 1, b: 2 };
```

**Pluses and minuses:**

| +                     | −                           |
| --------------------- | --------------------------- |
| Заменяет `arguments`  | Только последним в паттерне |
| Понятные variadic API | Shallow-копия остатка       |

**Sources:** [javascript.info/rest-parameters-spread](https://javascript.info/rest-parameters-spread)

</details>

<a name="spread"></a>

<details>
  <summary>Spread</summary>

### Spread

**Definition:** **Spread** (`...iterable`) разворачивает итерируемое в элементы литерала массива или копирует собственные перечисляемые свойства в литерал объекта.

**How it works:**

- В `[...arr, x]` и `{ ...obj, key: val }` spread **расширяет** новый литерал.
- **Поверхностное** копирование — вложенные ссылки общие.
- В object spread порядок важен: поздние ключи перекрывают ранние.

**When to use:**

| Особенность    | Используйте, когда                      |
| -------------- | --------------------------------------- |
| Spread массива | Копия, concat, prepend/append           |
| Spread объекта | Immutable merge / переопределение полей |

#### Common pitfalls

```js
const inner = { x: 1 };
const nested = { data: inner };
const clone = { ...nested };
clone.data.x = 99;
inner.x; // 99
```

**Examples:**

```js
const copy = [...arr, newItem];
const merged = { ...defaults, ...userOverrides };
```

**Pluses and minuses:**

| +                            | −                      |
| ---------------------------- | ---------------------- |
| Краткие immutable-обновления | Только shallow         |
| Читаемые merge               | Легко ждать deep clone |

**Sources:** [javascript.info/rest-parameters-spread](https://javascript.info/rest-parameters-spread)

</details>

<a name="destructuring"></a>

<details>
  <summary>Destructuring</summary>

### Destructuring

**Definition:** **Деструктуризация** распаковывает значения массива или объекта в отдельные переменные через паттерн.

**How it works:**

- Массив — по **позиции**; объект — по **имени** (rename, значения по умолчанию).
- Default срабатывает при `undefined` у источника.
- **Умные параметры:** `function show({ title = "Menu", width = 100 } = {})` — `= {}` для вызова без аргументов.
- Присваивание без `let`/`const`: `({ title, width } = options)` в скобках.
- Rest в паттерне → [Rest](#rest).

**When to use:**

| Особенность              | Используйте, когда         |
| ------------------------ | -------------------------- |
| Деструктуризация массива | Обмен, tuple-like возвраты |
| Деструктуризация объекта | Props, API, options        |
| Объект опций             | Много optional с defaults  |

#### Common pitfalls

```js
const {
  address: { city },
} = {}; // TypeError

let title, width;
({ title, width } = { title: "Menu", width: 200 });
```

**Examples:**

```js
[a, b] = [b, a];

const { years: age, isAdmin = false } = { name: "John", years: 30 };

function showMenu({ title = "Untitled", width = 200, height = 100 } = {}) {
  return `${title} ${width}x${height}`;
}
```

**Pluses and minuses:**

| +                  | −                                |
| ------------------ | -------------------------------- |
| Краткая распаковка | Затенение внешних имён           |
| Читаемые параметры | Вложенные паттерны и `undefined` |

**Sources:** [javascript.info/destructuring-assignment](https://javascript.info/destructuring-assignment)

</details>
