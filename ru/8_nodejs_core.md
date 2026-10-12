# Node.js — Core

V8, цикл событий, потоки, буферы и основы среды выполнения.

[ ← Вернуться в хаб](./8_nodejs.md)

### Related docs

- [Node.js — Core](./8_nodejs_core.md) · [Node.js — Модули](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Дополнительно](./8_nodejs_process.md) · [Node.js — Интервью](./8_nodejs_interview.md)
- [Цикл событий JS](./3_js_async_event_loop.md#event-loop) (стек вызовов, микрозадачи, macrotask в браузере)

### Main concepts

| Priority         | Topics                                                                                                                                                                                                                                                                                                                                       |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Essential**    | [С чего начать](#what-to-learn-first) · [Node.js overview](#nodejs-overview) · [libuv](#libuv) · [Event loop](#event-loop) · [Single-threaded vs multi-threaded](#single-threaded-vs-multi-threaded) · [EventEmitter](#eventemitter) · [Buffer](#buffer) · [Streams](#streams) · [Stream events & backpressure](#stream-events-backpressure) |
| **Skim**         | [Global object](#global-object) · [worker_threads](#worker_threads) · [Core terminology](#core-terminology)                                                                                                                                                                                                                                  |
| **Reference**    | [Encoding & numbering systems](#encoding-numbering-systems)                                                                                                                                                                                                                                                                                  |
| **Outside Core** | [Модули](./8_nodejs_modules.md) (CJS/ESM, `fs`, `path`) · [HTTP](./8_nodejs_http.md) · [JS async](./3_js_async.md) (promises, `async/await`)                                                                                                                                                                                                 |

### Sections

- [Node runtime & core](#node-runtime-core)
- [Streams, buffers & I/O](#streams-buffers-io)

Пакеты, CJS/ESM и встроенные модули → [Node.js — Модули](./8_nodejs_modules.md).

---

## Node runtime & core

<details>
  <summary>What to learn first</summary>

### What to learn first

**Definition:** Приоритет изучения этой страницы — что освоить для собеседований и backend-работы, а что достаточно просмотреть или отложить.

**How it works:**

| Уровень            | Концепции                                                                                                                                                                                                                                                                                              | Цель                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| **Обязательно**    | [Node.js overview](#nodejs-overview), [libuv](#libuv) (кратко), [Event loop](#event-loop), [Single-threaded vs multi-threaded](#single-threaded-vs-multi-threaded), [EventEmitter](#eventemitter), [Buffer](#buffer), [Streams](#streams), [Stream events & backpressure](#stream-events-backpressure) | Объяснить неблокирующий I/O, планирование, бинарные данные и потоки в реальных приложениях |
| **Просмотреть**    | [Global object](#global-object), [worker_threads](#worker_threads), [Core terminology](#core-terminology)                                                                                                                                                                                              | Знать `process` / `console`; когда выносить CPU-работу; глоссарий по модулям и env         |
| **Ниже приоритет** | [Encoding & numbering systems](#encoding-numbering-systems)                                                                                                                                                                                                                                            | Полезно для crypto и сериализации; не столь же «ядро runtime», как цикл событий            |

**libuv (кратко):** C API знать не нужно. Достаточно: libuv под Node, сетевой I/O на потоке цикла, fs / DNS / часть `crypto` — в пуле потоков (`UV_THREADPOOL_SIZE`, по умолчанию 4).

Core — только **основы runtime**. Соседние темы всё равно нужны:

| Тема                                           | Где                                                                           |
| ---------------------------------------------- | ----------------------------------------------------------------------------- |
| CJS vs ESM, `require` / `import`, `fs`, `path` | [Node.js — Модули](./8_nodejs_modules.md)                                     |
| HTTP-сервер, Express, REST                     | [Node.js — HTTP](./8_nodejs_http.md)                                          |
| Promises, `async/await`, callbacks             | [JS — Async](./3_js_async.md) · [Node.js — Интервью](./8_nodejs_interview.md) |

**Thoughts:** Если мало времени — сначала **Event loop** и **libuv**: они закрывают большинство вопросов «как устроен Node?». Затем streams и EventEmitter, прежде чем углубляться в кодировки.

</details>

<details>
  <summary>Node.js overview</summary>

### Node.js overview

**Definition:** Node.js — это бесплатная кроссплатформенная серверная среда выполнения с открытым исходным кодом, которая позволяет разработчикам создавать масштабируемые приложения с помощью JavaScript.

**How it works:**

| Особенность                   | Описание                                                           |
| ----------------------------- | ------------------------------------------------------------------ |
| Файловый ввод-вывод           | чтение/запись файлов на диске                                      |
| Сеть                          | HTTP-серверы, сокеты TCP/UDP                                       |
| БД                            | подключиться через драйверы (см. [9_database.md](./9_database.md)) |
| Совместное использование кода | повторное использование JS между интерфейсом и сервером            |
| Пакеты                        | реестр npm с миллионами модулей                                    |

V8 (JS-движок) + libuv (асинхронный ввод-вывод, пул потоков) + привязки узлов.

| Глобальный     | Цель                                |
| -------------- | ----------------------------------- |
| `__dirname`    | путь к каталогу текущего модуля     |
| `__filename`   | полный путь к текущему файлу модуля |
| `process.argv` | Массив аргументов CLI               |
| `process.env`  | переменные среды                    |

**Examples:**

```js
console.log(__dirname);
console.log(process.argv.slice(2)); // user args after node script.js
```

**Sources:** [nodejs.org](https://nodejs.org/en/about)

</details>

<details>
  <summary>libuv</summary>

### libuv

**Definition:** Кроссплатформенная C-библиотека, на которой держится асинхронный ввод-вывод Node: цикл событий, пул потоков, сокеты TCP/UDP и асинхронная работа с файловой системой.

**How it works:**

Node стоит на трёх слоях: **V8** выполняет JavaScript, **libuv** планирует ввод-вывод, **привязки Node** соединяют JS API с libuv.

1. JavaScript вызывает асинхронный API (например, `fs.readFile`, `http.createServer`).
2. Привязка Node передаёт работу libuv.
3. **Сетевой ввод-вывод** остаётся на потоке цикла — libuv опрашивает сокеты через примитивы ОС (`epoll`, `kqueue`, `IOCP`).
4. **Блокирующая работа** (файловый I/O, `dns.lookup`, часть `crypto`) выполняется в **глобальном пуле потоков** libuv (по умолчанию **4** потока, настраивается через `UV_THREADPOOL_SIZE`, максимум **1024**).
5. По завершении libuv ставит callback в очередь; **цикл событий** выполняет его на главном JS-потоке.

| Компонент       | Роль                                                                    |
| --------------- | ----------------------------------------------------------------------- |
| Цикл событий    | Один на поток. Node 20+: pending → poll → check → close → таймеры       |
| Пул потоков     | Общий для всех циклов; блокирующие системные вызовы вне главного потока |
| `uv_queue_work` | Планирует пользовательскую C/работу вне потока, callback — на цикле     |

**When to use:** Увеличивайте `UV_THREADPOOL_SIZE`, если профилирование показывает очередь на fs/crypto/DNS. Тяжёлый JS на CPU → `worker_threads`, а не только больший пул.

**Pluses and minuses:**

| +                                        | −                                                       |
| ---------------------------------------- | ------------------------------------------------------- |
| Эффективный I/O без потока на соединение | Размер пула ограничивает параллельный fs/crypto/DNS     |
| Одна абстракция на Linux, macOS, Windows | API libuv по умолчанию не потокобезопасны               |
| Главный поток свободен для JS-callback   | CPU-нагрузка на главном потоке всё равно блокирует цикл |

**Examples:**

```bash
UV_THREADPOOL_SIZE=8 node server.js
```

**Thoughts:** libuv — причина, почему Node силён в I/O-bound серверах, но для CPU-bound работы нужны workers или отдельные процессы. [Цикл событий](#event-loop) — это цикл планирования libuv, как его видит JavaScript.

**Sources:** [libuv design](https://docs.libuv.org/en/v1.x/design.html) · [libuv thread pool](https://docs.libuv.org/en/v1.x/threadpool.html) · [Node.js event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)

</details>

<details>
  <summary>Event loop</summary>

### Event loop

**Definition:** Цикл событий — это планировщик [libuv](#libuv) на потоке JavaScript. Так Node делает неблокирующий ввод-вывод с одним стеком вызовов: ядро или пул потоков libuv заканчивает работу в другом месте, а этот цикл запускает callback, когда стек свободен.

**How it works:**

1. Node инициализирует цикл и выполняет входной скрипт до конца. `fs.readFile`, `http.createServer`, таймеры и `process.nextTick` только **регистрируют** работу и сразу возвращаются. Они не ждут.
2. Когда скрипт вернулся — и снова после **каждого следующего JavaScript-callback** — Node осушает две очереди, которые **не являются фазами**: очередь `process.nextTick`, затем очередь микрозадач (`Promise.then` / `catch` / `finally`, `queueMicrotask`, продолжения `await`). Если `nextTick` снова и снова ставит `nextTick`, ввод-вывод не наступает и процесс встаёт.
3. Затем идёт одна итерация libuv. С **Node.js 20** (libuv 1.45) порядок такой, как в таблице. Просроченные таймеры могут выполниться ещё **один раз до первой итерации** — поэтому старые тексты ставят таймеры первыми на каждом обороте.

| Порядок | Фаза              | Что выполняется                                                                                                                                  |
| ------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1       | pending callbacks | Ввод-вывод, отложенный на следующий оборот, например часть ошибок TCP (`ECONNREFUSED`)                                                           |
| 2       | idle, prepare     | Только внутреннее                                                                                                                                |
| 3       | poll              | Новые события ввода-вывода и почти все I/O-callback. Здесь поток **спит**, когда больше ничего не готово                                         |
| 4       | check             | Callback `setImmediate` в порядке постановки                                                                                                     |
| 5       | close callbacks   | Резкое закрытие: например `socket.destroy()` шлёт `'close'` здесь. Обычное закрытие может уйти в очередь через `process.nextTick`                |
| 6       | timers            | `setTimeout` / `setInterval`, у которых выдержка уже прошла. Выдержка — это **минимум**, не дедлайн. `setTimeout(fn, 0)` поднимается до **1 мс** |

У каждой фазы своя FIFO-очередь. Цикл выполняет её, пока она не опустеет или не упрётся в лимит callback libuv, и переходит дальше. Poll подробнее:

- Очередь poll не пуста: выполнять callback, пока она не опустеет или не сработает лимит.
- Очередь poll пуста и ждёт `setImmediate`: не спать, перейти в **check**.
- Очередь poll пуста и immediate нет: спать, пока не будет готов сокет или не подойдёт следующий таймер.
- Долгий callback в poll может выйти за выдержку таймера. Именно poll решает, когда таймерам дать ход.
- `setImmediate`, поставленный **пока check уже идёт**, ждёт следующей итерации.

Где работа выполняется на самом деле:

| Работа                                                                | Где выполняется                                | Callback возвращается |
| --------------------------------------------------------------------- | ---------------------------------------------- | --------------------- |
| Сетевые сокеты                                                        | Этот поток, через `epoll` / `kqueue` / `IOCP`  | poll                  |
| `fs` (кроме watcher и `*Sync`), `dns.lookup`, часть `crypto` и `zlib` | [Пул потоков libuv](#libuv)                    | poll                  |
| Ваш JavaScript                                                        | Этот же поток, один стек вызовов               | —                     |
| Тяжёлый по CPU JS                                                     | `worker_threads` — **отдельный** цикл и изолят | `message` у родителя  |

Очереди, которые не фазы:

| Очередь                                                 | Когда выполняется                                                                                          |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `process.nextTick`                                      | После текущей операции, до продолжения цикла. На схеме фаз её нет                                          |
| Микрозадачи                                             | Сразу после осушения очереди nextTick                                                                      |
| Скрипт CommonJS                                         | Сначала `nextTick`, потом промисы / `queueMicrotask`                                                       |
| Вычисление ESM-модуля                                   | Тело модуля уже микрозадача, поэтому `queueMicrotask` во время этого вычисления идёт **раньше** `nextTick` |
| `setTimeout(fn, 0)` и `setImmediate` из главного модуля | **Не фиксирован** — зависит от того, успел ли таймер до первого poll                                       |
| Та же пара **внутри** I/O-callback (`fs.readFile`)      | `setImmediate` (check) всегда раньше таймера                                                               |

После каждого оборота Node смотрит ref-хендлы (слушающий сервер, активный таймер, незавершённый запрос). Если их не осталось, процесс завершается. `timeout.unref()` / `immediate.unref()` снимает эту удержку: процесс может выйти, пока таймер ещё запланирован.

Браузерная картина — одна macrotask, затем все микрозадачи, затем отрисовка — в [цикле событий JS](./3_js_async_event_loop.md#event-loop). Node не складывает таймеры, ввод-вывод и `setImmediate` в одну общую очередь macrotask.

**When to use:**

- Разбирать порядок логов: синхронный код, затем `nextTick` / микрозадачи, затем callback фазы.
- Держать каждый callback коротким. Синхронный цикл или `fs.readFileSync` на запросе блокирует всех остальных клиентов этого процесса.
- Выполнить код после того, как вызывающий успел повесить обработчики (`'listening'`, emit из конструктора): `process.nextTick`.
- Отдать ход, чтобы прошли poll и таймеры: `setImmediate`. Node советует его вместо `nextTick`, если не нужно выполниться строго до продолжения ввода-вывода.
- Отложить на тот же оборот так, чтобы работало и в браузере: `queueMicrotask` или промис.
- Тяжёлый JavaScript: `worker_threads`, а не более короткий таймер.

**Pluses and minuses:**

| +                                                           | −                                                                                 |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Много соединений делят один поток JavaScript                | Один длинный callback замораживает таймеры, сокеты и новые запросы на этом потоке |
| Поток спит в poll, а не крутится вхолостую                  | Рекурсивный `process.nextTick` или микрозадачи морят ввод-вывод                   |
| У таймеров, ввода-вывода и `setImmediate` отдельные очереди | Порядок `setTimeout(fn, 0)` и `setImmediate` из главного модуля нестабилен        |
| `unref` позволяет фоновым таймерам не держать процесс       | Выдержка таймера — минимум; планировщик ОС и другие callback сдвигают его позже   |

**Examples:**

```js
// CommonJS / обычный скрипт. Верхний уровень ESM меняет nextTick и промис местами — см. таблицу выше.
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
process.nextTick(() => console.log("4"));
console.log("5");
// 1, 5, 4, 3, 2
```

```js
import fs from "node:fs";

fs.readFile(import.meta.filename, () => {
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
});
// внутри I/O-callback: immediate, затем timeout
```

**Thoughts:** Ответ на интервью и короткий список фаз: [What is the event loop in Node.js?](./8_nodejs_interview.md#what-is-the-event-loop-in-nodejs). Если в схеме таймеры стоят вверху каждого оборота, это Node до версии 20.

**Sources:** [Node.js — The event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick) · [libuv design](https://docs.libuv.org/en/v1.x/design.html) · [process.nextTick vs queueMicrotask](https://nodejs.org/api/process.html#when-to-use-queuemicrotask-vs-processnexttick) · [timers](https://nodejs.org/api/timers.html) · [Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop) · [learn.javascript.ru — цикл событий](https://learn.javascript.ru/event-loop)

</details>

<details>
  <summary>Global object</summary>

### Global object

**Definition:**

В Node.js глобальный объект (`global`) — это пространство имен верхнего уровня, аналогичное `window` в браузерах. Встроенные и определяемые пользователем привязки верхнего уровня доступны во всех модулях.

| Недвижимость                 | Цель                                    |
| ---------------------------- | --------------------------------------- |
| `console`                    | протоколирование и отладка              |
| `process`                    | информация о процессе, окружение, выход |
| `Buffer`                     | двоичные данные                         |
| `setTimeout` / `setInterval` | таймеры                                 |
| `__dirname` / `__filename`   | пути к модулям (CommonJS)               |

**Examples:**

```js
console.log(process.pid);
console.log(process.env.NODE_ENV);
```

**Thoughts:** Избегайте загрязнения `global`; предпочитаю явный `require`/`import` и экспорт модулей.

**Sources:** [глобальные переменные nodejs.org](https://nodejs.org/api/globals.html)

</details>

<details>
  <summary>Core terminology</summary>

### Core terminology

**Definition:**

Ключевые понятия для понимания того, как структурированы и выполняются приложения Node.js.

| Срок                         | Определение                                                             |
| ---------------------------- | ----------------------------------------------------------------------- |
| Процесс                      | запуск экземпляра программы с собственной памятью и ресурсами           |
| Тема                         | единица выполнения внутри процесса                                      |
| Модуль                       | автономный блок кода с экспортом                                        |
| Напильник для ствола         | реэкспортирует несколько модулей из одного индексного файла             |
| Кэширование                  | хранение часто используемых данных в памяти                             |
| Зависимости                  | внешние пакеты, необходимые во время выполнения                         |
| Зависимости от разработчиков | пакеты, необходимые только во время разработки                          |
| Внедрение зависимостей       | зависимости, предоставленные извне, а не созданные внутри               |
| Переменные среды             | Конфигурация уровня ОС через `process.env`                              |
| `.env` файл                  | локальные переменные окружения (никогда не передавать секреты)          |
| `nodemon`                    | инструмент разработчика, который перезапускает Node при изменении файла |

**How it works:** `process.env` является глобальным узлом; тот `dotenv` пакетные нагрузки `.env` файлы в него при запуске.

**Sources:** [процесс nodejs.org](https://nodejs.org/api/process.html#processenv)

</details>

<details>
  <summary>Single-threaded vs multi-threaded</summary>

### Single-threaded vs multi-threaded

**Definition:**

Node.js запускает JavaScript в **одном основном потоке**, но libuv использует пул потоков для блокировки ввода-вывода (fs, криптография, DNS). Для параллелизма используйте кластер, дочерние процессы или рабочие потоки.

| Подход                        | Вариант использования                         |
| ----------------------------- | --------------------------------------------- |
| Основной поток + цикл событий | Серверы с привязкой к вводу-выводу            |
| `worker_threads`              | JS с привязкой к процессору в том же процессе |
| `child_process` / `cluster`   | отдельные процессы, изоляция неисправностей   |
| Несколько серверов/Докер      | горизонтальное масштабирование                |

**How it works:** Node является однопоточным для выполнения JS; многопоточный для ввода-вывода через libuv.

**Sources:** [nodejs.org worker_threads](https://nodejs.org/api/worker_threads.html) · [кластер nodejs.org](https://nodejs.org/api/cluster.html)

</details>

<details>
  <summary>worker_threads</summary>

### worker_threads

**Definition:** `worker_threads` запускает JavaScript в параллельных потоках в рамках одного и того же процесса — отдельные изоляты V8 с дополнительными `SharedArrayBuffer` для общей памяти.

**How it works:**

1. Основной поток создает `new Worker('./worker.js', { workerData })`.
2. Worker запускает собственный цикл событий и контекст JS.
3. Общайтесь через `postMessage` / `on('message')` (структурированный клон) или `SharedArrayBuffer`.
4. Worker завершает работу, когда скрипт завершается или `worker.terminate()` называется.

| против кластера/дочернего_процесса | рабочие_потоки                                 | кластер                      |
| ---------------------------------- | ---------------------------------------------- | ---------------------------- |
| Память                             | Общий процесс (дополнительная общая память)    | Отдельные процессы           |
| Стартап                            | Зажигалка                                      | Тяжелее (новый V8)           |
| Изоляция неисправностей            | Авария работника может занять процесс          | Изоляция процесса            |
| Вариант использования              | JS с привязкой к процессору (хэш, изображение) | HTTP-работников на процессор |

**When to use:** Пакетное хеширование паролей, изменение размера изображения, анализ PDF-файлов, тяжелое преобразование JSON — не для простого ввода-вывода (этим занимается цикл событий).

**Pluses and minuses:**

| +                                        | −                                                |
| ---------------------------------------- | ------------------------------------------------ |
| Настоящая параллельная работа ЦП в Node  | Нет ДОМ; накладные расходы на передачу сообщений |
| Легче вилки                              | GIL-подобно — один JS-поток на каждого работника |
| `workerData` проходит исходное состояние | Отладка сложнее                                  |

**Examples:**

```js
import {
  Worker,
  isMainThread,
  parentPort,
  workerData,
} from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url), {
    workerData: { n: 40 },
  });
  worker.on("message", (result) => console.log("fib:", result));
} else {
  const fib = (n) => (n <= 1 ? n : fib(n - 1) + fib(n - 2));
  parentPort.postMessage(fib(workerData.n));
}
```

**Thoughts:** Не порождайте воркеры для каждого запроса — используйте пул. Рабочие файлы могут быть `.js` с `type: module` в package.json.

**Sources:** [nodejs.org worker_threads](https://nodejs.org/api/worker_threads.html)

</details>

---

<a name="streams-io"></a>

## Streams, buffers & I/O

<details>
  <summary>Buffer</summary>

### Buffer

**Definition:**

`Buffer` — это глобальный класс Node.js для двоичных данных фиксированного размера (расширяет `Uint8Array`). Используется для файлов, сетевых пакетов, изображений и любых нетекстовых данных.

| Концепция         | Описание                                                 |
| ----------------- | -------------------------------------------------------- |
| Кодирование       | преобразование строки Buffer ↔ (`utf8`, `hex`, `base64`) |
| Декодирование     | строка → Буфер                                           |
| Почему не струны? | двоичные данные повреждаются в строках JS                |

**Examples:**

```js
const buf = Buffer.from("Hello", "utf8");
console.log(buf); // <Buffer 48 65 6c 6c 6f>
console.log(buf.toString("hex")); // 48656c6c6f

const fromHex = Buffer.from([0x1a, 0x1b, 0x1c]);
```

**Sources:** [буфер nodejs.org](https://nodejs.org/api/buffer.html)

</details>

<details>
  <summary>Encoding & numbering systems</summary>

### Encoding & numbering systems

**Definition:**

Кодирование преобразует данные в передаваемый формат. Юникод сопоставляет символы с числами; UTF-8/UTF-16 — распространенные кодировки.

| Кодирование       | Описание                           |
| ----------------- | ---------------------------------- |
| ASCII             | 7-битные английские символы        |
| UTF-8             | переменная 1–4 байта, веб-стандарт |
| UTF-16            | 2 или 4 байта на символ            |
| База64            | двоичный → текст ASCII             |
| Шестнадцатеричный | представление base-16              |

**How it works:**

| База              | Префикс | Пример        |
| ----------------- | ------- | ------------- |
| Десятичный        | —       | `25`          |
| Двоичный          | `0b`    | `0b1011` → 11 |
| Восьмеричный      | `0o`    | `0o25` → 21   |
| Шестнадцатеричный | `0x`    | `0x2A` → 42   |

**Examples:**

```js
(7).toString(2); // '111'
Buffer.from("Бе", "utf8"); // Cyrillic UTF-8 bytes
```

**Sources:** [javascript.info двоичный-шестнадцатеричный](https://javascript.info/binary)

</details>

<details>
  <summary>EventEmitter</summary>

### EventEmitter

**Definition:**

Встроенный класс для шаблона событий публикации/подписки. Событийная архитектура Core to Node; многие модули расширяют его.

| Метод                | Цель                   |
| -------------------- | ---------------------- |
| `.on(event, fn)`     | прослушать событие     |
| `.emit(event, data)` | пожар                  |
| `.once(event, fn)`   | послушай один раз      |
| `.off(event, fn)`    | удалить прослушиватель |

**How it works:** `http.Server`, `net.Socket`, `fs.ReadStream`, `fs.WriteStream`.

**Examples:**

```js
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();
emitter.on("data", (chunk) => console.log(chunk));
emitter.emit("data", "hello");
```

**Sources:** [события nodejs.org](https://nodejs.org/api/events.html)

</details>

<details>
  <summary>Streams</summary>

### Streams

**Definition:**

Абстрактный интерфейс для обработки данных порциями — эффективен для больших файлов и сетевой передачи. Все потоки есть `EventEmitter` экземпляры.

| Тип                 | Направление                          | Пример                 |
| ------------------- | ------------------------------------ | ---------------------- |
| Читабельно          | только чтение                        | `fs.createReadStream`  |
| Доступно для записи | писать только                        | `fs.createWriteStream` |
| Дуплекс             | читать + писать                      | `net.Socket`           |
| Трансформировать    | изменить при прокладке трубопроводов | `zlib.createGzip`      |

**How it works:**

| Выгода                | Почему                                     |
| --------------------- | ------------------------------------------ |
| Разделение интересов  | небольшие сборные детали                   |
| Гибкость              | трубопровод в несколько пунктов назначения |
| Эффективность памяти  | обрабатывать фрагменты, а не весь файл     |
| Эффективность времени | начать обработку до полной загрузки        |

**Examples:**

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";

createReadStream("input.txt")
  .pipe(createGzip())
  .pipe(createWriteStream("input.txt.gz"));
```

**Sources:** [поток nodejs.org](https://nodejs.org/api/stream.html)

</details>

<details>
  <summary>Stream events & backpressure</summary>

### Stream events & backpressure

**Definition:**

Читаемые потоки излучают `data` куски в потоковом режиме. Записываемые потоки излучают `drain`, `finish`, `error`. **Обратное давление:** приостанавливайте чтение, когда записываемый буфер заполнен.

| Событие  | Поток               | Когда                    |
| -------- | ------------------- | ------------------------ |
| `data`   | Читабельно          | доступен фрагмент        |
| `end`    | Читабельно          | больше нет данных        |
| `drain`  | Доступно для записи | буфер пуст, можно писать |
| `finish` | Доступно для записи | все данные сброшены      |
| `error`  | оба                 | провал                   |

**Examples:**

```js
readStream.on("data", (chunk) => {
  const ok = writeStream.write(chunk);
  if (!ok) readStream.pause();
});
writeStream.on("drain", () => readStream.resume());
```

**Sources:** [противодавление потока nodejs.org](https://nodejs.org/en/docs/guides/backpressuring-in-streams/)

</details>
