# JS Core — Language

Охватывает основы среды выполнения ECMAScript: движки и рантайм. ES-модули — в [Modules](./3_js_modules.md). Привязки, типы, операторы и структуры — в соседних темах Core ([Переменные](./3_js_variables.md), [Типы данных](./3_js_data_types.md), [Структуры данных](./3_js_data_structures.md), [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массив](./3_js_array.md) · [Функции](./3_js_functions.md)).

[ ← Назад в JS-хаб](./3_js.md)

### Related docs

- [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Стек и куча](./3_js_data_types.md#memory-stack-heap) · [globalThis](./3_js_variables.md#globalthis) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Ошибка обработка](./3_js_loops.md#error-handling) · [Итераторы](./3_js_loops.md#iterators) · [JS-массивы](./3_js_array.md) · [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [JS Дополнительно](./3_js_extra.md) · [JS-функции](./3_js_functions.md) · [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md); [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md) · [Стек вызовов](./3_js_async_event_loop.md#call-stack) · [TypeScript](./5_ts.md) · [Веб-API](./6_web_api.md) · [Build — polyfills & transpilers](./10_build_bundlers.md#polyfills-vs-transpilers) · [YAML](./10_build_ci.md#yaml) · [Node.js](./8_nodejs.md) · [Процесс и потоки](./8_nodejs_process.md#process--threads) · [React](./4_react.md) · [Vue](./4_vue.md) · [ООП и SOLID](./7_special_engineering.md#объектно-ориентированное-программирование-oop) · [Информатика](./7_special_cs.md) · [SDLC](./7_special_sdlc.md#software-design-methodology)

### Main concepts

| Площадь              | Темы                                                                                   |
| -------------------- | -------------------------------------------------------------------------------------- |
| **Время выполнения** | JS против ECMAScript, JIT, polyfills и transpilers, строгий режим, консоль, стиль кода |

### Sections

> Теория `<details>` блоки следуют: **Определение** · **Как это работает** · **Когда использовать** · **Плюсы и минусы** · **Примеры** · **Мысли** · **Источники**. **Определение** обязательно; другие разделы являются необязательными.

- [Core](#core)
- [Design principles](#design-principles)
- [Интервью](./3_js_interview.md)

Стек и куча → [Типы данных](./3_js_data_types.md#memory-stack-heap). Обработка ошибок → [Циклы](./3_js_loops.md#error-handling). Итераторы и генераторы → [Циклы](./3_js_loops.md#iterators).

Родственные темы Core: [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массив](./3_js_array.md) · [Функции](./3_js_functions.md) · [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md).

Также обсуждались темы: **JIT**, **polyfills / transpilers**, **строгий режим**. **ESM** → [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md). Типы/stack-heap → [3_js_data_types.md](./3_js_data_types.md); объекты/JSON → [3_js_data_structures.md](./3_js_data_structures.md); переменные/область/`globalThis`/замыкания → [3_js_variables.md](./3_js_variables.md); операторы → [3_js_operators.md](./3_js_operators.md); циклы/ошибки/итераторы → [3_js_loops.md](./3_js_loops.md); массивы → [3_js_array.md](./3_js_array.md); Intl/RegExp/Сигналы → [3_js_extra.md](./3_js_extra.md); цикл событий → [JS Async](./3_js_async_await.md); GC/Прокси/глубокое копирование → [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md); парадигмы → [7_special_cs.md](./7_special_cs.md).

<a name="core"></a>

## Core

<details>
  <summary>JavaScript and ECMAScript</summary>

### JavaScript & ECMAScript

**Definition:** JavaScript — это язык сценариев высокого уровня, созданный для того, чтобы сделать веб-страницы интерактивными. Программы представляют собой текстовые **скрипты** (часто встроенные в HTML), которые запускаются без отдельного этапа компиляции. **ECMAScript** (ECMA-262) — языковой стандарт; «JavaScript» — это повседневное название этого языка плюс **API хоста** (DOM в браузерах, `fs` /сеть в Node и т. д.). Он не имеет никакого технического отношения к Java — название пришло из маркетинга, когда его кратко называли LiveScript.

| черта       | Деталь                                                                                                               |
| ----------- | -------------------------------------------------------------------------------------------------------------------- |
| Парадигмы   | Событийный, функциональный, ООП, императивный                                                                        |
| Ввод текста | Динамический — типы проверяются во время выполнения                                                                  |
| Исполнение  | Движок анализирует → компилирует → запускает (JIT-оптимизация); см. [Движок и среда выполнения](#engine-and-runtime) |
| Модули      | `.mjs` or `type="module"` для ЕСМ                                                                                    |
| Хозяева     | Браузер, Node/Deno/Bun, встроенные движки на других устройствах                                                      |

**How it works:**

- **Движок (ВМ):** каждый хост имеет встроенный движок. Распространенные кодовые имена: **V8** (Chrome, Opera, Edge, Node), **SpiderMonkey** (Firefox), **JavaScriptCore** / Nitro / SquirrelFish (Safari). Наследие: **Чакра** (IE/ранняя версия Edge). Поддержка функций часто обсуждается по названию движка («поддерживается в V8»).
- **Конвейер (основы):** анализ сценария → компиляция в машинный код → запуск; двигатели продолжают оптимизировать горячие пути.Более глубокое различие JIT/AOT → [Engine и среда выполнения](#engine-and-runtime).
- **Возможности браузера:** манипулирование HTML/CSS; реагировать на ввод пользователя; сетевые запросы (XHR/`fetch`, исторически AJAX/COMET); файлы cookie/диалоги; клиентское хранилище (`localStorage`, и т. д.). То, что _может_ делать язык, зависит от **хоста**, а не только от ECMAScript.
- **Ограничения в браузере (безопасность):** нет произвольного доступа к диску/ОС; файл/камера/микрофон требуют явных жестов или разрешения пользователя; Доступ к кросс-таблице/окну DOM заблокирован из разных источников (**Политика одного и того же происхождения** — схема + хост + порт); Для чтения из сети между источниками требуется согласие удаленной стороны (заголовки CORS).За пределами браузера (узел, расширения с предоставленными разрешениями) эти правила песочницы не применяются таким же образом. Подробности SOP/CORS → [CORS и политика одного и того же происхождения](./3_js_browser_extra.md#cors-same-origin).
- **Почему JS победил браузер:** полная интеграция HTML/CSS, простые задачи остаются простыми, доставка включена по умолчанию во всех основных браузерах — уникальное сочетание браузерных технологий.Также используется для серверов, мобильных (RN/Hermes), настольных компьютеров (Electron) и т. д.
- **Языки «поверх» JS:** многие языки **транспилируются** в JavaScript (TypeScript, CoffeeScript, Flow, Dart→JS, Brython, Kotlin/JS, …). Вам все еще нужен JS для отладки того, что на самом деле запускает браузер/узел. Слой набора текста сегодня → [TypeScript](./5_ts.md).

**When to use:** Пользовательский интерфейс браузера и любой хост, в который встроен движок JS. Предпочитайте TypeScript (или аналогичный) для больших типизированных баз кода, но сначала изучите JS. Не предполагайте, что API-интерфейсы браузера существуют на сервере (и наоборот).

**Pluses and minuses:**

| +                                                                      | −                                                                                                |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Единственный вездесущий язык браузера со встроенным HTML/CSS.          | Название сбивает людей с Java; динамическая типизация может скрыть ошибки до момента выполнения. |
| Один и тот же язык на клиенте и на многих серверах (Node, Deno, Bun).  | Набор возможностей зависит от хоста — изолированные программные среды безопасности различаются.  |
| Огромная экосистема; движки сходятся на ежегодных выпусках ECMAScript. | Стек Transpile-to-JS добавляет инструменты;надо еще рассуждать о выдаваемом JS.                  |

**Examples:**

```html
<!-- Script in the page — runs as the document loads -->
<script>
  document.body.textContent = "Hello from a script";
</script>
```

```js
// ECMAScript module — strict by default; host APIs come from the environment
export function greet(name) {
  return `Hello, ${name}`;
}
```

**Thoughts:** Структура интервью: ECMAScript = спецификация языка; JavaScript = этот язык + API хоста. «Что может JS?» всегда означает «в какой среде?» Песочница браузера (SOP, без произвольной FS) — это функция, а не ошибка. Первым паровозом Брендана Эйха был SpiderMonkey (1995); современные двигатели реализуют тот же ECMA-262 с небольшими отличиями в крайнем случае.

**Sources:** [javascript.info — Введение](https://javascript.info/intro) · [MDN — Что такое JavaScript?](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript) · [MDN — технологии JavaScript обзор](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/JavaScript_technologies_overview) · [ECMA-262](https://tc39.es/ecma262/)

</details>

<details>
  <summary>Strict mode</summary>

### Strict mode

**Definition:**

`"use strict"` выбирает более строгий анализ и поведение во время выполнения: никаких случайных глобальных переменных, дублирование параметров запрещено, `delete` на ненастраиваемые свойства выкидывает.

| Ограничено                     | Эффект                                    |
| ------------------------------ | ----------------------------------------- |
| Необъявленное присвоение       | `ReferenceError` вместо глобальной утечки |
| Повторяющиеся имена параметров | Синтаксическая ошибка                     |
| Восьмеричные литералы          | Синтаксическая ошибка                     |
| `this` в простых вызовах       | `undefined` (нет `globalThis`)            |

**How it works:**

- Место `"use strict";` (or `'use strict';`) в **верхней части** скрипта или в качестве первого оператора функции. Над ним могут появляться только комментарии — в противном случае он игнорируется.
- Нет **нет** `"no use strict"` — после включения вы не сможете отменить его для этой области.
- **Модули** (`import`/`export`, `type="module"`) и **class** тела автоматически включают строгий режим — директива не требуется.
- **консоль** браузера по умолчанию не является строгой; обернуть фрагменты в IIFE с помощью `'use strict'` при тестировании строгого поведения.

**When to use:** Полностью новый код. Предпочитайте модули/классы, чтобы строгий режим был неявным; в противном случае поместите директиву вверху классических сценариев.

**Examples:**

```js
"use strict";
function demo() {
  // this === undefined in strict mode
}

// Modules / classes are already strict:
export function f() {
  /* strict */
}
class C {
  method() {
    /* strict */
  }
}
```

**Sources:** [javascript.info/strict-mode](https://javascript.info/strict-mode) · [Строгий режим MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

</details>

<details>
  <summary>Engine and runtime</summary>

### JavaScript engine pipeline

**Definition:** Механизм JS анализирует исходный код в AST, интерпретирует его в байт-код, профилирует горячие пути и JIT-компилирует оптимизированный машинный код. Вот почему «интерпретируется JavaScript?» не имеет ответа да/нет.

**How it works:**

- **AOT-компиляция** (C, Go): перевести всю программу в машинный код _перед_ запуском. Выявляет множество ошибок на ранней стадии; медленнее начинать итерацию.
- **Интерпретировано** (классические сценарии): переводите и запускайте построчно. Быстрый старт; опечатка может появиться только тогда, когда эта строка выполняется.
- **JS на практике:** синтаксический анализ + компиляция происходят _во время выполнения_ (по этой причине MDN до сих пор называет JS «интерпретируемым»).Синтаксис сначала проверяется для **всего** сценария — `SyntaxError` в строке 2 предотвращает запуск линии 1. А `ReferenceError` из-за неправильно написанного идентификатора возникает **ошибка времени выполнения**: предыдущие строки уже выполнялись.
- **объявления** функции подняты, поэтому вызов может появиться над `function` в исходнике, а не построчная интерпретация.
- **JIT:** интерпретатор холодного кода; если функция часто выполняется со стабильными типами, оптимизирующий компилятор генерирует машинный код (и может деоптимизировать, если типы изменяются). Babel/TypeScript генерирует старый JS _до_ движка — это не то же самое, что JIT.

| Этап                      | Роль                                 |
| ------------------------- | ------------------------------------ |
| Парсер                    | Токенизирует и строит AST            |
| Переводчик                | Быстро запускает байт-код            |
| Профайлер                 | Находит горячие функции              |
| Оптимизирующий компилятор | Испускает более быстрый машинный код |

**When to use:**

Интервью «интерпретированное vs составленное»; объяснить, почему синтаксические ошибки vs `ReferenceError` отличаться; почему горячие петли становятся быстрее после прогрева.

#### Common pitfalls

Вызов JS «только что интерпретированный» или «только что скомпилированный»; смесительный двигатель JIT с транспилем Babel.

**Pluses and minuses:**

| +                                                        | −                                                          |
| -------------------------------------------------------- | ---------------------------------------------------------- |
| Быстрый запуск + более поздняя пиковая скорость          | Типовые спекуляции могут деоптимизировать                  |
| Синтаксические ошибки не позволяют выполнить весь анализ | Ярлыки «интерпретировано»/«скомпилировано» сбивают с толку |

**Examples:**

```text
Source → Parser → AST → Interpreter (bytecode) → Profiler → Compiler (optimized code)
```

```js
console.log("this never runs");
console.log("oops" "); // SyntaxError — parse fails first

const a = 0;
console.log("ran");
console.log(a2); // ReferenceError after "ran"

praise("Pushkin"); // works — function declaration is hoisted
function praise(name) {
  console.log(`Ай да, ${name}!`);
}
```

**Thoughts:**

JIT сочетает в себе быстрый запуск (интерпретатор) с пиковой производительностью (компилятор). Transpilers и polyfills работают **до** или **рядом** с движком — см. [Polyfills and transpilers](#polyfills-and-transpilers); это не JIT движка.

| Двигатель      | Хозяин          |
| -------------- | --------------- |
| В8             | Хром, Нод, Дено |
| ПаукОбезьяна   | Firefox         |
| JavaScriptCore | Сафари          |

**Sources:** [javascript.info/intro](https://javascript.info/intro) · [MDN — интерпретируемый или скомпилированный](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/What_is_JavaScript) · [MDN JIT](https://developer.mozilla.org/en-US/docs/Glossary/Just_in_time_compilation) · [блог V8](https://v8.dev/blog)

</details>

<details>
  <summary>Polyfills and transpilers</summary>

### Polyfills and transpilers

**Definition:** **Transpiler** (компилятор «исходник → исходник») переписывает **синтаксис ECMAScript**, который целевой движок не умеет разбирать, в более старый синтаксис. **Polyfill** — это JavaScript во время выполнения, который **реализует отсутствующий API** (методы, конструкторы, глобалы), чтобы вызовы в коде не менялись. Ни то ни другое не заменяет **движок** (V8, SpiderMonkey, …) и не является **JIT** — см. [Engine and runtime](#engine-and-runtime). Детали сборки и Babel → [Polyfills vs transpilers](./10_build_bundlers.md#polyfills-vs-transpilers).

**How it works:**

- **Разрыв синтаксиса:** `class`, `async`/`await`, optional chaining (`?.`), nullish coalescing (`??`) — старый парсер бросает `SyntaxError` до выполнения строк. Лечится **транспиляцией** (Babel, emit TypeScript, esbuild, SWC), а не polyfill.
- **Разрыв API:** `Promise`, `Array.prototype.at`, `Object.hasOwn` — файл парсится, но метода нет. Лечится **polyfill** (`core-js`, или `if (!globalThis.X) { … }`) либо отказом от API на этой цели.
- **Типичный pipeline:** пишете современный код → транспилируете до версии ECMAScript под Browserslist (или поле `engines` в Node) → подключаете polyfills только там, где в рантайме нет API. Vite/esbuild транспилируют по умолчанию; polyfills — осознанный выбор (`core-js`, `useBuiltIns` в `@babel/preset-env`).
- **Бандлеры** (webpack, Rollup, Vite) собирают модули; могут вызывать transpiler, но сами transpiler не являются.

**When to use:** Вы поддерживаете браузеры или Node без нужной языковой фичи или встроенного API. Проверяйте разрыв (таблицы совместимости MDN, [caniuse.com](https://caniuse.com/), [compat-table](https://compat-table.github.io/compat-table/es6/)). Предпочитайте нативную реализацию — MDN отмечает, что polyfill медленнее и может не повторить всё поведение нативного кода.

**Pluses and minuses:**

| +                                             | −                                                     |
| --------------------------------------------- | ----------------------------------------------------- |
| Один код для разных сред выполнения           | `core-js` и неаккуратные пресеты раздувают бандл      |
| Polyfills сохраняют читаемые вызовы           | Синтаксис не «заполняется» polyfill — только API      |
| Transpilers дают новый синтаксис на старых VM | В стеке часто виден сгенерированный JS, а не исходник |

**Examples:**

```js
// Разрыв API — polyfill (сначала feature detect)
if (!Math.trunc) {
  Math.trunc = (n) => (n < 0 ? Math.ceil(n) : Math.floor(n));
}

// Разрыв синтаксиса — транспиляция до запуска движком
// const size = config?.width ?? 100;
// → старый JS с явными проверками null/undefined
```

**Thoughts:** На интервью: «Babel vs polyfill?» — transpiler меняет **грамматику**; polyfill добавляет **объекты/функции в рантайме**. «Polyfill для `async`?» — не одной строкой; транспиляция в generators/Promises (или отказ от движка). TypeScript → JavaScript — транспиляция; типы стираются и не polyfill’ятся. jQuery v1 — ранний пример **API**-polyfill (один интерфейс поверх разных браузеров), см. глоссарий MDN.

**Sources:** [MDN — Polyfill](https://developer.mozilla.org/en-US/docs/Glossary/Polyfill) · [javascript.info — Polyfills](https://javascript.info/polyfills) · [core-js](https://github.com/zloirock/core-js)

</details>

<details>
  <summary>Console</summary>

### Console API

**Definition:** API отладки браузера/узла для ведения журнала, синхронизации и проверки объектов. Подключайтесь к отладчику Sources, когда вам нужно сделать паузу — см. [Отладка JS (Источники)](./11_vsc_browser.md#browser-devtools).

**How it works:**

- `console.log` / `table` / `dir` / `time` / `timeEnd` для вывода без остановки выполнения.
- Предпочитаете **точку останова** или `debugger` когда вам нужны живые локальные пользователи и стек вызовов.

**Examples:**

```js
console.log("info", { a: 1 });
console.table([{ id: 1 }, { id: 2 }]);
console.time("fetch");
// await fetch(...)
console.timeEnd("fetch");
console.dir(document.body, { depth: 1 });
```

**Sources:** [Консоль MDN](https://developer.mozilla.org/en-US/docs/Web/API/console) · [javascript.info/debugging-chrome](https://javascript.info/debugging-chrome)

</details>

<details>
  <summary>Code style and comments</summary>

### Code style & comments

**Definition:** Правила стиля существуют для того, чтобы сделать код читабельным и избежать ошибок (ASI, отсутствие фигурных скобок). Комментарии должны объяснять **почему** и архитектуру, а не рассказывать **что** делает следующая строка.

**How it works:**

- Отдавайте предпочтение египетским брекетам (`if (cond) {`), пробелы вокруг операторов и длину строки команды (часто 80–120). Всегда ставьте `;` операторы after — см. [ASI](./3_js_operators.md).
- Уменьшить гнездование с ранним `return` / `continue` вместо глубокого `else` деревья.
- Размещение функций: часто **сначала вызывайте сайт**, помощники ниже — читатели узнают, _что происходит_, прежде чем _как_.
- **Хорошие комментарии**: обзор архитектуры, неочевидные решения, JSDoc для публичных API. **Плохие комментарии:** повторение `i++` или называя то, что уже говорит хорошо названная функция — вместо этого учитывайте функцию.
- Антишаблоны («код ниндзя»): названия однобуквенные, расплывчатые. `data`/`obj`, противоречивые синонимы (`show` vs `display` для того же самого), тихие побочные эффекты в `is*`/`check*` функции, повторное использование переменных для несвязанных значений в середине функции.

**When to use:** Согласуйте руководство по стилю (Airbnb, Standard, Google или Prettier + ESLint) и примените линтер — см. [ESLint](./10_build_bundlers.md).

**Examples:**

```js
// Prefer early return over nested else
function pow(x, n) {
  if (n < 0) return NaN;
  let result = 1;
  for (let i = 0; i < n; i++) result *= x;
  return result;
}

/**
 * Returns x raised to the n-th power.
 * @param {number} x
 * @param {number} n Natural number
 * @return {number}
 */
function documentedPow(x, n) {
  /* ... */
}
```

**Thoughts:** Читабельные имена превосходят остроумные остроты. Если вам нужен комментарий, объясняющий _что_, перепишите код.

**Sources:** [javascript.info/coding-style](https://javascript.info/coding-style) · [javascript.info/comments](https://javascript.info/comments) · [javascript.info/ninja-code](https://javascript.info/ninja-code)

</details>

<details>
  <summary>Glossary</summary>

### Glossary

**Definition:**

Общие термины, используемые в основной документации JS.

| Срок          | Значение                                                                   |
| ------------- | -------------------------------------------------------------------------- |
| Выделить      | Зарезервировать память для значения                                        |
| Разложение    | Разбейте проблему на более мелкие части                                    |
| Мемоизация    | Кэшируйте результаты функции, чтобы избежать повторных вычислений          |
| Идентификатор | Имя переменной, функции или метки                                          |
| Линтер        | Инструмент, который обеспечивает соблюдение стиля кода и выявляет проблемы |
| Нормализатор  | Функция, стандартизирующая форму данных                                    |
| Минимизация   | Удалите пробелы/комментарии, чтобы уменьшить размер пакета                 |

**Sources:** [Глоссарий MDN](https://developer.mozilla.org/en-US/docs/Glossary)

</details>

<a name="design-principles"></a>

## Design principles

<details>
  <summary>SOLID in Classes</summary>

### SOLID in Classes

**Definition:** **SOLID** — пять архитектурных принципов дизайна Роберта Мартина («Дядя Боб») для кода, который остаётся **удобным для изменений, тестов и расширения**. В JavaScript — особенно с приходом ES6-**`class`** — они помогают избежать **«спагетти-кода»**. На **иерархиях `class`** каждая буква связана с прототипами, `extends` и инжектом через constructor. В JS `class` — **синтаксический сахар над прототипами**: методы лежат на prototype, `extends` связывает цепочку — поэтому **Liskov** и **композиция** важны при каждом наследовании. В чистом JS нет встроенных `interface` (как в TypeScript или Java) — часть принципов адаптируется через **композицию функций** или **утиную типизацию** (JSDoc `@typedef`, TS `implements`). Маппинг на модули и функции: [SOLID in Functions](#solid-in-functions).

| Letter | Principle             | Какую проблему закрывает (классы)                         |
| ------ | --------------------- | --------------------------------------------------------- |
| **S**  | Single Responsibility | God-классы с несвязанными обязанностями                   |
| **O**  | Open/Closed           | Бесконечные правки `if/else` внутри `applyDiscount()`     |
| **L**  | Liskov Substitution   | Подклассы, ломающие код, рассчитанный на родительский тип |
| **I**  | Interface Segregation | Жирные class API с неиспользуемыми методами               |
| **D**  | Dependency Inversion  | Классы с `new EmailService()` или `fetch` внутри          |

| Principle                     | Meaning                                                                                                                                                                                                                                  | JS `class` example                                                                             |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **S** — Single Responsibility | Класс или функция должны иметь **одну причину для изменения** — одну задачу. Валидация — в `UserValidator`; сохранение в БД — в `UserRepository`. Смешать validate + DB + HTML в одном `User` — правка UI может сломать логику хранения. | `UserValidator` валидирует; `UserRepository` сохраняет — не один `User` на всё                 |
| **O** — Open/Closed           | Открыт для **расширения**, закрыт для **модификации**. Новый тип скидки — новый подкласс (`PercentDiscount`), а не очередная ветка `if/else` в `applyDiscount()`. Существующие вызывающие не переписываются.                             | Добавить `FixedDiscount` без правки `applyDiscount()` — новый подкласс `Discount`              |
| **L** — Liskov Substitution   | Наследники должны **легко заменять родителей**, не ломая логику. Если `Ostrich` бросает в `fly()`, вызывающим нужен `try/catch` — подставить безопасно нельзя.                                                                           | `releaseBird(new Ostrich())` падает, а `releaseBird(new Sparrow())` работает — LSP нарушен     |
| **I** — Interface Segregation | Клиенты не должны зависеть от методов, которые **не используют**. Read-only листер не обязан реализовывать `delete()` или `update()` только потому, что у «большого» `UserRepository` они есть.                                          | `UserReader` только читает — не требовать `delete()` у read-only класса                        |
| **D** — Dependency Inversion  | Модули верхнего уровня (`OrderService`) не зависят от нижнего уровня — оба зависят от **абстракций**. Mailer передаётся в `constructor` — prod, тесты и будущие каналы меняются без переписывания заказов.                               | `OrderService` принимает `mailer` в `constructor` — подставить `EmailService` или `SmsService` |

**How it works:**

Каждая буква соответствует **Examples** ниже — ищите `// good` и `// bad`:

1. **S** — один класс, одна задача (`UserValidator` vs `UserRepository`).
2. **O** — новая скидка = новый подкласс (`PercentDiscount`), а не новый `if` внутри `applyDiscount()`.
3. **L** — `releaseBird(bird)` должен работать с любой птицей; нелетающим нужен другой контракт, а не подкласс с throw в `fly()`.
4. **I** — `UserReader` только читает; `UserWriter` меняет — не наследуйте лишние методы.
5. **D** — передавайте `mailer` в `constructor` (`new OrderService(mailer)`); не делайте `new EmailService()` внутри `OrderService`.

**When to use:** Рефакторинг god-классов; plugin-style провайдеры; ответы на интервью; code review, когда растут цепочки `extends`.

**Examples:**

```js
// S — good: one job per class
class UserValidator {
  validate(data) {
    return Boolean(data.email && data.name);
  }
}

class UserRepository {
  save(user) {
    return db.insert("users", user);
  }
}

// S — bad: god class mixes validate, DB, and HTML
// class User {
//   validate(data) { return Boolean(data.email); }
//   save(user) { return db.insert("users", user); }
//   toHtml(user) { return `<p>${user.name}</p>`; }
// }

// O — good: new discount type = new subclass (no edit to applyDiscount)
class Discount {
  apply(total) {
    throw new Error("Subclass must implement apply()");
  }
}

class PercentDiscount extends Discount {
  constructor(rate) {
    super();
    this.rate = rate;
  }
  apply(total) {
    return total * (1 - this.rate);
  }
}

class FixedDiscount extends Discount {
  constructor(amount) {
    super();
    this.amount = amount;
  }
  apply(total) {
    return Math.max(0, total - this.amount);
  }
}

function applyDiscount(total, discount) {
  return discount.apply(total);
}

// O — bad: edit applyDiscount for every new discount type
// function applyDiscount(total, type, value) {
//   if (type === "percent") return total * (1 - value);
//   if (type === "fixed") return Math.max(0, total - value);
// }

// I — good: reader lists; writer mutates — separate APIs
class UserReader {
  findAll() {
    return db.query("SELECT * FROM users");
  }
}

class UserWriter {
  update(id, data) {
    return db.query("UPDATE users SET ? WHERE id = ?", [data, id]);
  }
}

// I — bad: read-only reader forced to stub methods it never uses
// class UserRepository {
//   findAll() { return db.query("SELECT * FROM users"); }
//   update() { throw new Error("Read-only"); }
// }
// class UserReader extends UserRepository {}

// L — one caller; every "bird" must work here
function releaseBird(bird) {
  return bird.fly(); // expects a string — not a crash
}

// L — good: fly() works for flying birds
class Sparrow {
  fly() {
    return "flying";
  }
}
releaseBird(new Sparrow()); // "flying"

// L — bad: still a Bird, but fly() throws — releaseBird breaks
class Bird {
  fly() {
    return "flying";
  }
}
class Ostrich extends Bird {
  fly() {
    throw new Error("Cannot fly");
  }
}
// releaseBird(new Ostrich()); // crash — caller needs try/catch

// L — good fix: separate contract for ground birds (composition, not broken extends)
class Ostrich {
  run() {
    return "running";
  }
}

// D — good: inject mailer — swap EmailService or SmsService
class EmailService {
  send(userId, message) {
    return `email to ${userId}: ${message}`;
  }
}

class SmsService {
  send(userId, message) {
    return `sms to ${userId}: ${message}`;
  }
}

class OrderService {
  #mailer;
  constructor(mailer) {
    this.#mailer = mailer;
  }
  place(order) {
    this.#mailer.send(order.userId, `Order ${order.id} placed`);
    return { ok: true, orderId: order.id };
  }
}

new OrderService(new EmailService());
new OrderService(new SmsService());

// D — bad: hardcoded concrete mailer inside order logic
// class OrderService {
//   place(order) {
//     const mailer = new EmailService();
//     mailer.send(order.userId, `Order ${order.id} placed`);
//   }
// }
```

**Thoughts:** SOLID — **ориентир**, не закон: для 10-строчного скрипта пять классов не нужны. Применять, когда больно менять код. Функции и модули: [SOLID in Functions](#solid-in-functions). Синтаксис классов: [Классы](./3_js_classes.md#classes-full). Фреймворки: [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Полный OOP-справочник: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [SOLID in Functions](#solid-in-functions) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

</details>

<details>
  <summary>SOLID in Functions</summary>

### SOLID in Functions

**Definition:** Те же пять принципов **SOLID** относятся к **обычным функциям**, **замыканиям** и **модулям ESM** — тому стилю, в котором большинство JavaScript пишут каждый день. Они помогают избежать **«спагетти-кода»** в function-first коде. Граница модуля или экспортируемая функция может быть единицей с одной задачей, расширяемой новыми функциями и тестируемой, когда зависимости передают снаружи. В чистом JS нет встроенных `interface` — **I** и **D** адаптируются через **композицию** (object spread, мелкие mixin-функции) и **утиную типизацию**. Маппинг на классы: [SOLID in Classes](#solid-in-classes).

| Letter | Principle             | Какую проблему закрывает (функции / модули)                                |
| ------ | --------------------- | -------------------------------------------------------------------------- |
| **S**  | Single Responsibility | God-модули, смешивающие parse, format и I/O                                |
| **O**  | Open/Closed           | Рост одной функции очередной веткой `if (paymentType)`                     |
| **L**  | Liskov Substitution   | Колбэки, которые бросают или возвращают разную форму                       |
| **I**  | Interface Segregation | Жирные объекты конфигурации и barrel-файлы с лишними экспортами            |
| **D**  | Dependency Inversion  | Доменные функции с прямым `fetch`, `localStorage` или `new EmailService()` |

| Principle                     | Meaning                                                                                                                                                                                                            | JS function example                                                           |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| **S** — Single Responsibility | Одна функция — **одна причина для изменения**. Парсинг JSON — в `parseUser()`; форматирование для UI — в `formatUserLabel()` — не одна `handleUser()` на всё.                                                      | `parseUser()` vs `formatUserLabel()` — не один `handleUser()` на всё          |
| **O** — Open/Closed           | Открыт для **расширения**, закрыт для **модификации**. Новый способ оплаты — новая strategy-функция, а не правка `processOrder()` очередной веткой.                                                                | `processOrder(order, payWithCard)` — добавить `payWithWallet` без правки ядра |
| **L** — Liskov Substitution   | Любой колбэк с ожидаемым контрактом должен **вести себя как остальные**. Если `normalizeUser()` обычно возвращает `{ id, name }`, ветка со throw или строкой ломает всех вызывающих — тот же LSP без `extends`.    | Сломанный `normalize` ломает вызывающих `mapUsers()`                          |
| **I** — Interface Segregation | Делите **крупные объекты конфигураций** на роль-специфичные формы. Предпочитайте **композицию** (mixin-функции) наследованию неиспользуемых методов. Экспортируйте **узкие поверхности** — не один barrel со всем. | `canRead(store)` для list-only — не fat repo с заглушками `write()`           |
| **D** — Dependency Inversion  | Высокоуровневые функции зависят от **инжектируемых коллабораторов** (HTTP, mailer, storage), а не от глобалов. Передавайте `http` или `notify` аргументом — тесты подставят фейки без переписывания бизнес-логики. | `placeOrder(order, notify)` — подставить `emailService` или `smsService`      |

**How it works:**

Каждая буква соответствует **Examples** ниже — ищите `// good` и `// bad`:

1. **S** — `parseUser()` и `formatUserLabel()` отдельно; не один `handleUser()`.
2. **O** — новая оплата = новая функция (`payWithWallet`), а не новый `if` в `processOrder()`.
3. **L** — колбэки в `mapUsers()` всегда возвращают одну форму; не строку вместо объекта.
4. **I** — собирайте `canRead(store)` для list-only; делите fat configs; не один `userUtils.js` со всем.
5. **D** — передавайте `notify` в `placeOrder()` и `http` в `loadProfile()`; не хардкодьте `EmailService` или `fetch` внутри.

**When to use:** Utility-модули, Node-сервисы, data-слои, рефакторинг god-файлов; большинство повседневного JS — functions-first.

**Examples:**

```js
// S — good: parse and format are separate functions
function parseUser(raw) {
  return JSON.parse(raw);
}

function formatUserLabel(user) {
  return `${user.name} <${user.email}>`;
}

// S — bad: one function parses and formats (two reasons to change)
// function handleUser(raw) {
//   const user = JSON.parse(raw);
//   return `${user.name} <${user.email}>`;
// }

// O + D — good: new payment = new function, processOrder stays closed
function processOrder(order, pay) {
  return pay(order.total);
}

function payWithCard(amount) {
  return { ok: true, method: "card", amount };
}

function payWithWallet(amount) {
  return { ok: true, method: "wallet", amount };
}

// O + D — bad: edit processOrder for every new payment type
// function processOrder(order) {
//   if (order.payment === "card") return { ok: true, method: "card" };
//   if (order.payment === "wallet") return { ok: true, method: "wallet" };
// }

// L — good: normalize always returns the same shape
function mapUsers(users, normalize = (u) => ({ id: u.id, name: u.name })) {
  return users.map(normalize);
}

// L — bad: callback sometimes returns a string — breaks mapUsers callers
// mapUsers(users, (u) => (u.active ? { id: u.id, name: u.name } : "skipped"));

// I — good: compose behavior from small mixin functions (no fat inheritance)
const canRead = (base) => ({
  ...base,
  read: (id) => base.store.get(id),
});
const canWrite = (base) => ({
  ...base,
  write: (id, data) => base.store.set(id, data),
});

const store = {
  data: new Map(),
  get: (id) => store.data.get(id),
  set: (id, v) => store.data.set(id, v),
};
const reader = canRead({ store }); // list-only consumer — no write/delete stubs

// I — bad: fat config forces unused keys on every consumer
// function initApp({ readUser, writeUser, deleteUser, auditUser, emailUser }) { ... }
// listPageOnlyNeeds({ readUser }); // still depends on write/delete/email shape

// D — good: inject notify — swap email or SMS in tests
function placeOrder(order, notify) {
  notify.send(order.userId, `Order ${order.id} placed`);
  return { ok: true, orderId: order.id };
}

const emailService = { send: (userId, msg) => `email: ${msg}` };
const smsService = { send: (userId, msg) => `sms: ${msg}` };
placeOrder({ id: "42", userId: "u1" }, emailService);
placeOrder({ id: "43", userId: "u2" }, smsService);

// D — bad: hardcoded EmailService inside domain function
// function placeOrder(order) {
//   const mailer = { send: (id, msg) => console.log(`email: ${msg}`) };
//   mailer.send(order.userId, `Order ${order.id} placed`);
// }

// D — good: inject http — swap fake in tests
async function loadProfile(userId, http) {
  return http.get(`/api/users/${userId}`);
}
// loadProfile("1", { get: async () => ({ id: "1", name: "Ada" }) });

// D — bad: hardcoded fetch — hard to test, tied to browser/Node global
// async function loadProfile(userId) {
//   const res = await fetch(`/api/users/${userId}`);
//   return res.json();
// }
```

**Thoughts:** Большинство JS — функции и модули, не классы; для повседневного кода начинайте здесь. `class` / `extends`: [SOLID in Classes](#solid-in-classes). Фреймворки: [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript). Полный справочник: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [MDN — Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) · [freecodecamp — SOLID](https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [SOLID in Classes](#solid-in-classes) · [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express) · [SOLID in TypeScript](./5_ts_basics.md#solid-in-typescript)

</details>
