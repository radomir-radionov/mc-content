# JS Async / Await — Sequential and parallel async flows

`async`/`await` basics, parallel vs sequential flows, async iterators, top-level await, and the concurrency model (tasks, workers).

### Related docs

- [JS Event Loop](./3_js_async_event_loop.md) · [JS Promises](./3_js_async_promises.md) · [JS Async APIs](./3_js_async_apis.md)
- [Web Workers](./6_web_api_performance.md#web-workers) · [Node worker_threads](./8_nodejs_core.md#worker_threads) · [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area        | Topics                                                   |
| ----------- | -------------------------------------------------------- |
| **Syntax**  | `async`/`await`, parallel vs sequential, async iterators |
| **Modules** | Top-level await                                          |
| **Model**   | Microtasks, macrotasks, workers vs main thread           |

### Sections

- [Async / await](#async-await)
- [Top-level await](#top-level-await)
- [Async/await, tasks, and concurrency model](#asyncawait-tasks-and-concurrency-model)
- [Interview](./3_js_interview.md)

Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---

## Async / await

<details>
  <summary>async/await basics</summary>

### async/await

**Definition:** `async` отмечает функцию, которая всегда возвращает обещание. `await` приостанавливает выполнение внутри этой функции до тех пор, пока не будет выполнено обещание, а затем возобновляется со значением.

**How it works:**

- `async function f() { ... }` — всегда возвращается `Promise`; `return x` → `Promise.resolve(x)`; `throw` → отказ.
- `await promise` — приостанавливает **только** эту асинхронную функцию; другой код в потоке продолжается.
- После завершения ожидания выполнение возобновляется как **микрозадача**.
- `await` работает с thenables, а не только с собственными обещаниями.
- Высший уровень `await` разрешено в модулях ES — см. [Await верхнего уровня](#top-level-await).

**When to use:**

| Используйте async/await            | Используйте необработанные обещания     |
| ---------------------------------- | --------------------------------------- |
| По умолчанию для логики приложения | Библиотека возвращает имущество         |
| Последовательные шаги с ветвями    | `Promise.race` остроты                  |
| `try/catch` обработка ошибок       | Взаимодействие в стиле обратного вызова |

#### Common pitfalls

- Отсутствующий `await` — `const x = fetch()` это обещание, а не данные.
- `async` IIFE забыли — await верхнего уровня только в модулях.
- `forEach` с `async` обратный вызов — не ожидает итераций.

**Pluses and minuses:**

| +                                       | −                                                                          |
| --------------------------------------- | -------------------------------------------------------------------------- |
| Читаемый последовательный код           | Забывчивость `await` молча ломает логику                                   |
| Родной `try/catch`                      | Последовательные циклы выполняются медленно без пакетной обработки         |
| Знакомый поток управления (`if`, `for`) | Отладка асинхронных стеков может оказаться сложнее                         |
| Взаимодействие с обещаниями             | Невозможно использовать `await` в не-`async` функции (кроме TLA в модулях) |

**Examples:**

```js
async function load() {
  const res = await fetch("/api/data");
  if (!res.ok) throw new Error(res.statusText);
  return res.json();
}

load().then((data) => console.log(data));
```

**Sources:** [javascript.info/async-await](https://javascript.info/async-await)

</details>

<details>
  <summary>Parallel vs sequential</summary>

### Parallel vs sequential

**Definition:** Последовательный `await` запускает задачи одну за другой; параллельное выполнение сначала запускает все задачи, а затем ожидает их вместе.

**How it works:**

- `const a = await fetchA(); const b = await fetchB();` — Б начинается только после завершения А.
- `const [a, b] = await Promise.all([fetchA(), fetchB()]);` — оба запускаются сразу; подожди обоих.
- Ключ: вызов функций **before** `await` приступить к работе — `const p = fetchA(); await p` является параллельным.
- `for await` и последовательный `await` в цикле — по одному за раз.
- Использовать `Promise.all` для независимого ввода/вывода; последовательный, когда каждый шаг требует предыдущего результата.

**When to use:**

| Узор                     | Когда                                            |
| ------------------------ | ------------------------------------------------ |
| Последовательный `await` | Шагу Б нужен результат шага А                    |
| `Promise.all`            | Независимые выборки, все необходимое             |
| `Promise.allSettled`     | Независимо, хорошо, если что-то потерпит неудачу |
| `map` + `all`            | N параллельных товаров с сохранением порядка     |

#### Common pitfalls

- `await` внутри `for` цикл — N × задержка; партия с `Promise.all`.
- Запуск обещаний, но ожидание одно за другим — по-прежнему последовательно, если второе ожидание ожидает завершения первого только после того, как первое началось поздно.
- `Promise.all` когда один сбой не должен отменять другие — используйте `allSettled`.

**Pluses and minuses:**

| +                                                            | −                                                                      |
| ------------------------------------------------------------ | ---------------------------------------------------------------------- |
| Последовательный: простые зависимости                        | Последовательный: сумма задержек                                       |
| Параллельно: минимальная задержка для самостоятельной работы | Параллельно: все потерпит неудачу, если `Promise.all` и один отвергает |
| `allSettled` для частичного успеха                           | Больше памяти — все запросы в работе                                   |

**Examples:**

```js
// Sequential — B waits for A
const a = await fetchA();
const b = await fetchB();

// Parallel — start both, await together
const [a, b] = await Promise.all([fetchA(), fetchB()]);

// Parallel in loop
const results = await Promise.all(ids.map((id) => fetchItem(id)));
```

**Sources:** [javascript.info/promise-api](https://javascript.info/promise-api)

</details>

<details>
  <summary>Async iterators and for await...of</summary>

### Async iterators and for await...of

**Definition:** Объекты с `[Symbol.asyncIterator]` давать обещания; `for await...of` потребляет их последовательно.

**How it works:**

- Асинхронные итеративные разоблачения `asyncIterator()` возвращение `{ next() → Promise<{ value, done }> }`.
- `for await (const x of asyncIterable)` — ждет каждого `next()` прежде чем продолжить.
- Асинхронные генераторы: `async function* gen() { yield await fetch(...); }`.
- Работает с потоками (читаемыми узлами), API с разбивкой на страницы, веб-потоками. `ReadableStream`.
- Должно быть внутри `async` функция или модуль верхнего уровня (с TLA).

**When to use:**

| Использовать `for await...of`                             | Использовать `Promise.all`                     |
| --------------------------------------------------------- | ---------------------------------------------- |
| Файловые/сетевые потоки                                   | Независимые параллельные запросы               |
| API с разбивкой на страницы (страница зависит от курсора) | Исправлен список известных URL-адресов         |
| Поток сообщений WebSocket                                 | Пакетная обработка с ограничением параллелизма |

#### Common pitfalls

- С использованием `for await` на неасинхронной итерации — ошибка времени выполнения.
- Не прерывание цикла при ошибке — весь поток завершается сбоем, если не обрабатываться за итерацию.
- Параллельно с `for await` — неправильный инструмент; используйте одновременных рабочих + очередь.

**Pluses and minuses:**

| +                                                | −                                                       |
| ------------------------------------------------ | ------------------------------------------------------- |
| Потоковая передача больших асинхронных данных    | Последовательный — медленнее параллельного              |
| Чистый синтаксис против руководства `.then` цепи | Требуется контекст асинхронной функции                  |
| Противодавление с потоками (Узел)                | Необходимость обработки ошибок `try/catch` вокруг петли |
| Пагинация с эффективным использованием памяти    | Не для несвязанных параллельных выборок                 |

**Examples:**

```js
async function* generateIds() {
  for (let i = 0; i < 3; i++) {
    await new Promise((r) => setTimeout(r, 100));
    yield i;
  }
}

(async () => {
  for await (const id of generateIds()) {
    console.log(id);
  }
})();
```

**Sources:** [javascript.info/async-iterators-generators](https://javascript.info/async-iterators-generators)

</details>

## Top-level await

<details>
  <summary>Top-level await (brief)</summary>

### Top-level await

**Definition:** `await` на верхнем уровне модуля (не внутри `async` функция) — приостанавливает **вычисление модуля** до тех пор, пока не будет выполнено обещание. Функция ES2022 только для модулей ES.

**How it works:**

- Только в `.mjs` файлы или `<script type="module">` — не CommonJS, не классические скрипты.
- Граф модуля ожидает — импортеры этого модуля блокируются до завершения TLA.
- Используется для: загрузки конфигурации, инициализации WASM, условного импорта.
- Недостаток: сериализует цепочку загрузки модуля — может замедлить запуск приложения.

**When to use:**

| Хорошее применение                                    | Избегайте                                                  |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| `const config = await loadConfig()` во входном модуле | Тяжелый ввод-вывод в широко импортируемом служебном модуле |
| Динамический `await import()` на основе окружения     | Замена всей отложенной загрузки на TLA                     |
| ВАСМ `await init()`                                   | ЗАО `require` контексты                                    |

#### Common pitfalls

- TLA в дереве глубоких зависимостей — непредвиденная задержка запуска.
- Использование в общем модуле, импортированном многими — узкое место.
- Классический скрипт или CJS — `SyntaxError`; используйте асинхронный IIFE или динамический импорт.

**Pluses and minuses:**

| +                              | −                                                     |
| ------------------------------ | ----------------------------------------------------- |
| Нет асинхронной оболочки IIFE  | Блокирует импорт зависимых модулей                    |
| Очистить конфигурацию/загрузку | Не в CJS без динамики `import()`                      |
| Естественно для модуля init    | Чрезмерное использование вредит параллельной загрузке |

**Examples:**

```js
// config.mjs
const res = await fetch("/api/config");
export const config = await res.json();

// app.mjs — waits for config.mjs to finish
import { config } from "./config.mjs";
console.log(config.apiUrl);
```

**Sources:** [Ожидание верхнего уровня MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await) · [javascript.info/modules](https://javascript.info/modules-intro)

</details>

<a name="asyncawait-tasks-and-concurrency-model"></a>

<details>
  <summary>Async/await, tasks, and concurrency model</summary>

### Async/await, tasks, and concurrency model

**Definition:** В JavaScript **`async/await`** — синтаксический сахар над Promise на **одном main thread**. **Tasks** — работа в очередях: **microtasks** (promises, продолжения `async/await`) и **macrotasks** (таймеры, I/O, UI events). **Многопоточность** для JS-кода — **Web Workers** (браузер) или **`worker_threads` / `cluster`** (Node), а не параллельный `await` на main thread.

**How it works:**

| Концепт              | JS / браузер                                                          | Node.js                                          |
| -------------------- | --------------------------------------------------------------------- | ------------------------------------------------ |
| **async/await**      | `async` возвращает Promise; `await` ставит функцию на паузу, не поток | То же; продолжения — microtasks                  |
| **Task queues**      | Sync → microtasks → одна macrotask → (paint)                          | Sync → `nextTick` → microtasks → фаза event loop |
| **Параллельный I/O** | Запускать promises до `await`; `Promise.all`                          | То же + libuv thread pool для fs/crypto          |
| **Параллельный CPU** | `new Worker('worker.js')` — без DOM                                   | `worker_threads` или `cluster` для multi-core    |

Маппинг JD **«async/await, Task, многопоточность»**:

- **async/await** → async с видом sync; ошибки через `try/catch`; параллель — `Promise.all`, не цикл с `await`.
- **Task** → **microtask/macrotask**, не C# `Task` (та же идея: единица async-работы). На .NET — `Task`/`Task.Run`; в JS — очереди event loop.
- **Многопоточность** → main thread JS один; CPU — workers; I/O — async + thread pool под капотом.

**When to use:** JD про async, event-loop задачи, «почему Node однопоточный, но держит много соединений».

**Pluses and minuses:**

| Модель                   | Плюс                          | Минус                                    |
| ------------------------ | ----------------------------- | ---------------------------------------- |
| async/await              | Читаемый поток                | Легко написать случайно sequential код   |
| Microtasks               | Упорядоченные promise-реакции | Длинные цепочки блокируют timers и paint |
| Workers / worker_threads | Настоящий parallel CPU        | Нет общего DOM; overhead message passing |

**Thoughts:** Профилируйте до workers — чаще узкое место I/O или main thread, не «мало потоков». CPU loop в Node → `worker_threads`; parse/sort в браузере → Web Worker. Процессы/потоки ОС → [Processes & multitasking](./0_system_design_core.md#processes-multitasking).

**Sources:** [javascript.info/event-loop](https://javascript.info/event-loop) · [MDN async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) · [MDN Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) · [Node worker_threads](https://nodejs.org/api/worker_threads.html)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
