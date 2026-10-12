# JS Promises — States, chaining, and static methods

Promise basics, chaining, static methods, promisification, error handling, unhandled rejections, and `Promise.withResolvers`.

### Related docs

- [JS Event Loop](./3_js_async_event_loop.md) · [JS Callbacks](./3_js_async_callbacks.md) · [JS Async/Await](./3_js_async_await.md) · [JS Async APIs](./3_js_async_apis.md)
- [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area       | Topics                                            |
| ---------- | ------------------------------------------------- |
| **Core**   | States, chaining, static methods, promisification |
| **Errors** | `try/catch` with promises, unhandled rejections   |
| **Modern** | `Promise.withResolvers`                           |

### Sections

- [Promises](#promises)
- [Unhandled promise rejections](#unhandled-promise-rejections)
- [Promise.withResolvers](#promisewithresolvers)
- [Interview](./3_js_interview.md)

Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---

## Promises

<details>
  <summary>Promise basics</summary>

### Promise

**Definition:** Обещание — это объект, представляющий конечный результат асинхронной операции. Он всегда находится в одном из трех состояний: `pending`, `fulfilled`, or `rejected`. Однажды заселенное государство не может измениться.

**How it works:**

- Создано с `new Promise((resolve, reject) => { ... })` или статические помощники (`Promise.resolve`, и т. д.).
- Исполнитель работает **синхронно**; `resolve`/`reject` выполнить обещание один раз (игнорируется при повторном вызове).
- `.then(onFulfilled, onRejected)` планирует обработчики как **микрозадачи**, когда обещание выполняется.
- `.catch(fn)` is `.then(undefined, fn)` — ловит отказы и вбрасывает предварительные `.then`.
- `.finally(fn)` работает при расчете независимо от результата; не получает значения; возвращает новое обещание.
- `await` и `.then` возвращать связанные обещания.

| Метод                            | Цель                                       |
| -------------------------------- | ------------------------------------------ |
| `.then(onFulfilled, onRejected)` | Обрабатывать успех или неудачу             |
| `.catch(onRejected)`             | Обработка отказа                           |
| `.finally(onFinally)`            | Запустить очистку независимо от результата |

**When to use:**

| Используйте обещания              | Использовать `async/await` вместо этого |
| --------------------------------- | --------------------------------------- |
| Обертывание API обратного вызова  | Последовательная бизнес-логика          |
| Публичные API библиотеки          | `try/catch` блоки ошибок                |
| Параллельно `Promise.all` партии  | Читабельный поток управления            |
| Когда вам нужно `.finally` уборка | Циклы с условными операторами           |

#### Common pitfalls

- Не возвращая обещание от `.then` - следующий `.then` получает `undefined`.
- Использование второго аргумента `.then` вместо `.catch` — пропускает броски в обработчике выполнения.
- `new Promise` антишаблон, когда значение уже существует — используйте `Promise.resolve(x)`.

**Pluses and minuses:**

| +                                         | −                                                                                   |
| ----------------------------------------- | ----------------------------------------------------------------------------------- |
| Сборные цепочки                           | Подробный против `async/await`                                                      |
| Ясно устоявшаяся государственная машина   | Ошибки исполнителя требуют внешнего `try/catch` or `.catch` о возвращенном обещании |
| `Promise.all` для параллелизма            | Забывчивость `return` in `.then` разрывает цепь                                     |
| Универсальное взаимодействие (`thenable`) | `.then` второй аргумент используется редко — предпочитаю `.catch`                   |

**Examples:**

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("I'm a Promise!"), 500);
});

promise
  .then((value) => console.log(value))
  .catch((err) => console.error(err))
  .finally(() => console.log("done"));
```

**Sources:** [Обещание MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) · [javascript.info/promise-basics](https://javascript.info/promise-basics)

</details>

<details>
  <summary>Promise states & chaining</summary>

### Promise states & chaining

**Definition:** Обещание подчиняется строгим правилам: оно должно раскрывать `.then` метод; оно начинается `pending` и однажды соглашается `fulfilled` or `rejected`; установленное значение является неизменным.

**How it works:**

- **ожидание** → операция выполняется; обработчики стоят в очереди до урегулирования.
- **выполнено** → `resolve(value)` называется; `.then` обработчики получают `value`.
- **отклонено** → `reject(reason)` или добавьте исполнителя; `.catch` обработчики получают `reason`.
- Каждый `.then`/`.catch`/`.finally` возвращает **новое** обещание — включает цепочку.
- Возвращаемое значение обработчика становится выполнением следующего обещания; выброшенная ошибка становится отклонением.
- Возврат промиса из обработчика **сглаживает** — его ждет следующий промис.

| Государство | Значение                        |
| ----------- | ------------------------------- |
| `pending`   | Исходный; операция продолжается |
| `fulfilled` | Завершено успешно               |
| `rejected`  | Ошибка по причине               |

**When to use:**

| Ситуация                       | Узор                                                                |
| ------------------------------ | ------------------------------------------------------------------- |
| Многошаговые зависимые запросы | `.then` цепной или последовательный `await`                         |
| Преобразовать результат за шаг | `return` сопоставленное значение из каждого `.then`                 |
| Восстановление после ошибки    | `.catch(() => fallback)` затем продолжить цепочку                   |
| Остановить цепочку при ошибке  | Пусть отторжение распространяется – никакого выздоровления `.catch` |

#### Common pitfalls

- Вбрасывание `.then` без `.catch` — необработанный отказ.
- `async` обработчик в `.then` возвращает Promise — должен возвращать его явно, если он вложен.
- Предполагая `.then` запускается немедленно — всегда микрозадача после текущего кода синхронизации.

**Pluses and minuses:**

| +                                                                  | −                                                                  |
| ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| Неизменная установленная стоимость — ее можно безопасно передавать | Невозможно отменить встроенный Promise (используйте `AbortSignal`) |
| Цепочка четко кодирует конвейер                                    | Длинные цепочки сложно отладить без меток                          |
| Ошибки распространяются на ближайшие `.catch`                      | Проглотил ошибки, если `.catch` отсутствует в конце                |

**Examples:**

```js
Promise.resolve(1)
  .then((x) => x + 1)
  .then((x) => {
    throw new Error("fail");
  })
  .catch(() => 42)
  .then(console.log);
// 42
```

**Sources:** [javascript.info/promise-chaining](https://javascript.info/promise-chaining)

</details>

<details>
  <summary>Promise static methods</summary>

### Promise static methods

**Definition:** Методы класса для выполнения нескольких обещаний с различной семантикой завершения.

**How it works:**

- `Promise.all(iterable)` — ждет всех; отклоняет при первом отказе; результаты в порядке.
- `Promise.allSettled(iterable)` — ждет всех; никогда не отвергает; `{ status, value|reason }[]`.
- `Promise.race(iterable)` — рассчитывается, когда **первое** обещание выполнено (выполнено или отклонено).
- `Promise.any(iterable)` — выполняется при первом успехе; отвергает `AggregateError` если все потерпит неудачу.
- Неперспективные элементы, обернутые `Promise.resolve`.
- Пустая итерация: `all` → `[]`, `race` → ожидание навсегда, `any` → `AggregateError`.

| Метод                | Решается, когда    | Отклоняет, когда  |
| -------------------- | ------------------ | ----------------- |
| `Promise.all`        | все выполнить      | сначала отклонить |
| `Promise.race`       | сначала поселиться | сначала отклонить |
| `Promise.allSettled` | все улажено        | никогда           |
| `Promise.any`        | сначала выполнить  | все отвергают     |

**When to use:**

| Метод                | Используйте, когда                                                    |
| -------------------- | --------------------------------------------------------------------- |
| `Promise.all`        | Требуются все результаты (например, пользователь + сообщения)         |
| `Promise.allSettled` | Пакет, в котором некоторые могут дать сбой (статус массовой загрузки) |
| `Promise.race`       | Обертка тайм-аута, побеждает самый быстрый CDN                        |
| `Promise.any`        | Первое успешное зеркало; резервный пул                                |

#### Common pitfalls

- `Promise.all` с последовательным `await` внутри цикла — используйте `map` + `all` вместо.
- `race` по таймауту — проигрышный запрос все еще выполняется; отменить с помощью `AbortController`.
- `allSettled` Порядок результатов соответствует порядку ввода, а не порядку завершения.

**Pluses and minuses:**

| +                                                | −                                                           |
| ------------------------------------------------ | ----------------------------------------------------------- |
| `all` — быстрый провал, упорядоченные результаты | Одна ошибка приводит к потере всех результатов              |
| `allSettled` — проверять каждый результат        | Более подробный анализ результатов                          |
| `race` — тайм-ауты, первый ответчик              | Победа первого отказа — не всегда желательно                |
| `any` — резервные зеркала/CDN                    | Все отказоустойчивые потребности `AggregateError` обработка |

**Examples:**

```js
var p1 = new Promise((resolve) => setTimeout(() => resolve("one"), 500));
var p2 = new Promise((resolve) => setTimeout(() => resolve("two"), 100));

Promise.race([p1, p2]).then(console.log); // "two"

// Timeout pattern
const timeout = (ms) =>
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("timeout")), ms),
  );
await Promise.race([fetch("/api"), timeout(5000)]);
```

**Sources:** [javascript.info/promise-api](https://javascript.info/promise-api)

</details>

<details>
  <summary>Promisification</summary>

### Promisification

**Definition:** Обертывание API в стиле обратного вызова (`fn(arg, (err, result) => ...)`) в функцию, возвращающую обещание.

**How it works:**

- Обернуть оригинальную функцию; возвращаться `new Promise((resolve, reject) => ...)`.
- Вызов оригинала с аргументами + обратный вызов; при обратном вызове с ошибкой: `err ? reject(err) : resolve(result)`.
- Узел обеспечивает `util.promisify(fn)` для стандартных функций, ориентированных на ошибку.
- Теряет семантику множественного обратного вызова (например. `progress` обратные вызовы), если не расширены.
- Результат можно скомпоновать с `await` и `Promise.all`.

**When to use:**

| Ситуация                                          | Подход                                          |
| ------------------------------------------------- | ----------------------------------------------- |
| Устаревший узел `fs.*`                            | `util.promisify` or `fs.promises`               |
| Старый API браузера                               | Ручная упаковка обещаний                        |
| Библиотека с обратным вызовом и версиями обещаний | Используйте обещанную версию, если она доступна |
| Завершение пользовательского события              | `new Promise` + `once` слушатель                |

#### Common pitfalls

- Многообещающая функция, которая вызывает обратный вызов несколько раз — подсчитывается только первый раз.
- Неправильная подпись обратного вызова (только успех или сначала ошибка).
- Не является обязательным `this` для методов — используйте `promisify(fs.readFile.bind(fs))` or `fs.promises`.

**Pluses and minuses:**

| +                                                     | −                                                          |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| Единый асинхронный стиль                              | Теряет семантику множественного обратного вызова           |
| Компонуется с `await`                                 | Необходимо правильно обрабатывать соглашение об ошибках    |
| Одноразовая упаковка, повторное использование повсюду | Некоторые API используют обратные вызовы только для успеха |

**Examples:**

```js
function promisify(fn) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (err, result) => {
        if (err) reject(err);
        else resolve(result);
      });
    });
  };
}

// Node: util.promisify(fs.readFile)
const readFile = promisify(fs.readFile);
const data = await readFile("file.txt", "utf8");
```

**Sources:** [javascript.info/promisify](https://javascript.info/promisify)

</details>

<details>
  <summary>try...catch with promises</summary>

### try...catch with promises

**Definition:** `try...catch` выполняет синхронные броски; с `async/await` он также перехватывает отклоненные обещания. С сырыми обещаниями используйте `.catch()` или конечный `.catch` на цепи.

**How it works:**

- `try/catch` вокруг `await` уловы обещают отказы, как если бы это были броски.
- Синхронизация внутри `async` функция → отклоненное обещание возвращается вызывающему абоненту.
- `.catch` по цепочке обрабатывает отказы от любых предыдущих `.then`.
- `.finally` запускается после успеха или неудачи — хорошо для очистки (закрытия соединений).
- Необработанный отказ: отклоненное обещание без `.catch` и нет `await` в попытке/пойме.

| Подход                | Лучшее для                                       |
| --------------------- | ------------------------------------------------ |
| `.catch()` на цепочке | Обещание трубопроводов                           |
| `try/catch` + `await` | `async` функции                                  |
| `.finally()`          | Очистка как на пути успеха, так и на пути неудач |

**When to use:**

| Ситуация                               | Узор                                  |
| -------------------------------------- | ------------------------------------- |
| Одна асинхронная функция               | `try { await ... } catch (e) { ... }` |
| Обещание трубопровода                  | тянущийся `.catch(console.error)`     |
| Всегда закрывать ресурс                | `try { ... } finally { cleanup() }`   |
| Повторный бросок после входа в систему | `catch (e) { log(e); throw e; }`      |

#### Common pitfalls

- `try/catch` без `await` — `fetch()` возвраты в ожидании обещания; ошибки не пойманы.
- Пустой `catch` проглатывание ошибок — скрывает ошибки.
- `return` in `catch` без повторного броска — вызывающий абонент видит успех с `undefined`.

**Pluses and minuses:**

| +                                          | −                                                       |
| ------------------------------------------ | ------------------------------------------------------- |
| `try/catch` читается как код синхронизации | `try/catch` за `await` многословен                      |
| `.catch` в конце цепочки — один обработчик | Легко забыть обработчик по принципу «выстрелил и забыл» |
| `.finally` для общей уборки                | `.finally` не подавляет неприятие                       |

**Examples:**

```js
const processData = async () => {
  try {
    const data = await fetchData();
    console.log("Processed:", data);
  } catch (error) {
    console.error("Error:", error);
  }
};

// Promise chain equivalent
fetchData()
  .then((data) => console.log("Processed:", data))
  .catch((error) => console.error("Error:", error));
```

**Sources:** [javascript.info/try-catch](https://javascript.info/try-catch)

</details>

## Unhandled promise rejections

<details>
  <summary>Unhandled promise rejections</summary>

### Unhandled promise rejections

**Definition:** Отклоненное обещание без `.catch` обработчик и нет `await` внутри `try/catch` **необработанный**. Среды выполнения создают события и могут завершить процесс (узел) или войти в консоль (браузер).

**How it works:**

- Отклонение отслеживается, когда обещание отклонено, а обработчик еще не прикреплен.
- Если обработчик прикреплен позже (тот же тик или поворот микрозадачи в некоторых движках), все равно может считаться обработанным.
- Браузер: `window` событие `unhandledrejection`; `event.promise`, `event.reason`.
- `event.preventDefault()` — отключить ведение журнала по умолчанию (браузер).
- Узел: `process.on('unhandledRejection', (reason, promise) => ...)`.
- Узел 15+: необработанный отказ по умолчанию может привести к сбою процесса.
- `rejectionhandled` / `handled` события, когда обработчик добавлен поздно.

| Окружающая среда          | Поведение по умолчанию                         |
| ------------------------- | ---------------------------------------------- |
| Браузер                   | `console.error` + `unhandledrejection` событие |
| Узел (современный)        | Предупреждение или выход из процесса           |
| `async` главный без улова | Необработанный отказ по возвращенному обещанию |

**When to use:**

| Ситуация                       | Исправить                                                               |
| ------------------------------ | ----------------------------------------------------------------------- |
| `fetch()` без улова            | Добавлять `.catch` or `try/catch` с `await`                             |
| `async` IIFE                   | `.catch` по обещанию: `(async () => {})().catch(...)`                   |
| Экспресс-обработчики маршрутов | Обертывание в промежуточное программное обеспечение try/catch или error |
| Глобальный мониторинг          | `unhandledrejection` слушатель → сообщить Sentry                        |

#### Common pitfalls

- `async` Возвращаемое значение функции игнорируется — отказ не обрабатывается на месте вызова.
- `.then(fn)` без `.catch` — отказ в `fn` распространяется, но внешнему обещанию нужен обработчик.
- Проглатывание глобального обработчика без устранения основной причины.

**Pluses and minuses:**

| +                                                     | −                                                       |
| ----------------------------------------------------- | ------------------------------------------------------- |
| Поверхности забыли пути ошибок                        | Может привести к сбою производства, если его не поймать |
| Глобальные обработчики для журналирования/мониторинга | Легко пропустить обещания «выстрелил и забыл»           |
| Вызывает явную стратегию ошибок                       | Трассировки асинхронного стека могут быть нечеткими     |

**Examples:**

```js
// Unhandled — logs error / may crash Node
Promise.reject(new Error("oops"));

// Handled
Promise.reject(new Error("oops")).catch(console.error);

// async without catch
async function main() {
  throw new Error("fail");
}
main(); // unhandled
main().catch(console.error); // handled

// Browser global handler
window.addEventListener("unhandledrejection", (event) => {
  console.error("Unhandled:", event.reason);
  event.preventDefault();
});
```

**Sources:** [MDN unhandledrejection](https://developer.mozilla.org/en-US/docs/Web/API/Window/unhandledrejection_event) · [javascript.info/promise-error-handling](https://javascript.info/promise-error-handling) · [javascript.info/microtask-queue](https://javascript.info/microtask-queue) · [Узел unhandledRejection](https://nodejs.org/api/process.html#event-unhandledrejection)

</details>

## Promise.withResolvers

<details>
  <summary>Promise.withResolvers (brief)</summary>

### Promise.withResolvers

**Definition:** Возврат статического метода ES2024 `{ promise, resolve, reject }` — отделяет создание обещания от исполнителя, что полезно, когда решение/отклонение необходимо вызывать извне конструктора.

**How it works:**

- `const { promise, resolve, reject } = Promise.withResolvers();`
- Эквивалентно ручному `let resolve, reject; const promise = new Promise((res, rej) => { resolve = res; reject = rej; });`
- `resolve`/`reject` вызывается несколько раз — учитывается только первый раз (стандартные правила обещаний).
- Очистка отложенных шаблонов, мостов от событий к обещаниям, тестов.

**When to use:**

| Использование                                             | Пример                               |
| --------------------------------------------------------- | ------------------------------------ |
| Отложенный — предоставить разрешение вызывающему абоненту | Ручной контроль обещаний             |
| Событие → пообещай один раз                               | `once(emitter, 'data')` помощник     |
| Испытательные приспособления                              | Решите обещание извне тестового тела |
| Очередь с противодавлением                                | Решите, когда слот будет доступен    |

#### Common pitfalls

- Забыл позвонить `resolve` or `reject` — обещание висит навсегда.
- Вызов обоих — побеждает первый; второй проигнорирован.
- Нет встроенной поддержки в более старых версиях Node — используйте полифилл или шаблон вручную.

**Pluses and minuses:**

| +                                                        | −                                                 |
| -------------------------------------------------------- | ------------------------------------------------- |
| Избегает неуклюжего шаблона исполнителя                  | ES2024 — проверьте поддержку целевой среды        |
| Более понятные API отложенных событий/отложенных событий | Все еще нужно `.catch` для необработанных отказов |
| Та же семантика, что и `new Promise`                     | Polyfill необходим для старых сред выполнения     |

**Examples:**

```js
const { promise, resolve, reject } = Promise.withResolvers();

setTimeout(() => resolve("done"), 1000);

const result = await promise; // 'done'

// Event bridge
function once(emitter, event) {
  const { promise, resolve } = Promise.withResolvers();
  emitter.once(event, resolve);
  return promise;
}
```

**Sources:** [MDN Promise.withResolvers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
