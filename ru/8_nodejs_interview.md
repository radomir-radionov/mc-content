# Node.js — Interview

Ответы заполнены для всего списка вопросов.

[ ← Вернуться в хаб](./8_nodejs.md)

### Related docs

- [Node.js — Core](./8_nodejs_core.md) · [Node.js — Модули](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Дополнительно](./8_nodejs_process.md) · [Node.js — Интервью](./8_nodejs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы с [Devinterview.io — Node.js](https://devinterview.io/questions/web-and-mobile-development/node-interview-questions/).

### Node.js Basics

<details>
<summary>Что такое Node.js and why is it used?</summary>

### Что такое Node.js and why is it used?

**Definition:**

Node.js — открытая кроссплатформенная среда выполнения JavaScript. Она запускает JavaScript вне браузера на V8, том же движке, что и Chrome, а libuv даёт кроссплатформенный слой ввода-вывода. Модель событийная и неблокирующая, поэтому один процесс держит много соединений, пока ждёт сеть.

**How it works:**

- У JavaScript на главном потоке один стек вызовов. Ввод-вывод запускается и завершается позже через цикл событий, поэтому ожидающий сокет не замораживает процесс.
- libuv выполняет файловый ввод-вывод, часть DNS-запросов и `uv_queue_work` в небольшом пуле потоков (по умолчанию четыре, `UV_THREADPOOL_SIZE`). Сетевой ввод-вывод остаётся на потоке цикла.
- npm — менеджер пакетов и публичный реестр, которые идут вместе с Node. Yarn и pnpm — другие клиенты того же реестра.
- Один язык может работать в браузере и на сервере, поэтому команды делят типы, проверку данных и небольшие утилиты. Браузерные API от этого на сервере не появляются.
- Чат или живая лента хорошо ложатся на эту модель: сокеты остаются открытыми, а цикл продолжает принимать другую работу. Сервера WebSocket в ядре нет: берут HTTP-сервер и библиотеку либо глобальный клиент `WebSocket`, если он есть в этой версии.
- Длинные циклы на CPU по-прежнему блокируют главный поток. Сжатие, картинки и другая тяжёлая работа уходят в `worker_threads`, дочерний процесс или отдельный сервис.

**When to use:**

- HTTP и JSON API, прокси и бэкенды для одностраничных приложений.
- Потоковая отдача файлов, логов или медиа кусками, без буфера на весь объём.
- Продукты реального времени (чат, уведомления, совместные курсоры) и небольшие сервисы, которые масштабируют добавлением процессов.
- Скрипты и утилиты на устройствах, где хватает небольшой среды, включая платы, на которых запускается Node.

**Pluses and minuses:**

- Один язык с двух сторон укорачивает стек и позволяет переносить библиотеки между клиентом и сервером, если они не трогают DOM или `fs`.
- Реестр и небольшой фреймворк вроде Express быстро поднимают первый API.
- Модуль `cluster` и менеджер процессов масштабируют сервис одним процессом на ядро. Это параллелизм процессов, а не потоки внутри одного изолята.
- Открытый код и большой набор пакетов экономят время и требуют фиксировать версии и следить за цепочкой поставок.
- Для плотных числовых циклов на главном потоке это слабый выбор по умолчанию.

**Examples:**

```js
import http from "node:http";

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("ok");
  })
  .listen(3000);
```

**Sources:**

- [Node.js — About](https://nodejs.org/en/about)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Как Node.js handle child threads?</summary>

### Как Node.js handle child threads?

**Definition:**

JavaScript, который вы пишете, выполняется на одном главном потоке с одним стеком вызовов. Другие потоки у Node всё же есть: пул libuv для части блокирующих системных вызовов и `worker_threads`, когда нужен второй изолят V8 для работы на CPU. Это разные механизмы.

**How it works:**

- Цикл событий остаётся на главном потоке и продолжает принимать ввод-вывод. Если вызов файловой системы, `dns.lookup` или часть crypto заблокировали бы поток, libuv ставит их в пул (`UV_THREADPOOL_SIZE`, по умолчанию 4). Сетевые сокеты в этот пул не отдают.
- Когда пул заканчивает, libuv возвращает результат, и цикл вызывает ваш callback. Главный поток внутри системного вызова не сидел.
- `worker_threads` запускает другой изолят со своим циклом событий и своей памятью. Общаются через `postMessage` / `parentPort`. Обычные объекты клонируют или передают владение, ими не делятся. `SharedArrayBuffer` — явный путь общей памяти.
- `isMainThread` показывает, на какой стороне разреза вы находитесь. Файл воркера может быть тем же модулем: ветка родителя делает `new Worker`, ветка потомка слушает `parentPort`.
- `child_process` — третий инструмент: целый процесс ОС для команды оболочки или жёсткой изоляции. `cluster` порождает процессы, чтобы занять больше ядер. Ни то ни другое не поток внутри родительского изолята.
- Вынос плотного цикла освобождает главный цикл для сокетов. Воркер на каждый запрос обычно дороже пользы: изоляты тяжёлые.

**When to use:**

- Пул потоков уже работает за асинхронным `fs`, `dns.lookup` и частью API `crypto`. Этими потоками редко управляют вручную.
- Worker threads: картинки, сжатие, разбор большой нагрузки или любой цикл на CPU, который остановил бы сервер.
- Дочерний процесс: запустить CLI, изолировать недоверенный код или упасть, не утянув родителя.

**Pluses and minuses:**

- Главный цикл остаётся отзывчивым, пока идут работа пула и воркеры.
- Параллелизм появляется без замков вокруг каждого объекта JavaScript.
- Передача сообщений копирует данные. Большие объёмы требуют списка transfer или общей памяти, иначе копирование съедает выигрыш.
- Пул маленький и общий. Всплеск вызовов `fs` и `pbkdf2` выстраивается в очередь друг за другом.

**Examples:**

```js
import { Worker, isMainThread, parentPort } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.on("message", (message) => console.log("Received:", message));
  worker.postMessage("Hello from the main thread");
} else {
  parentPort.on("message", (message) => {
    console.log("Received in the worker:", message);
    parentPort.postMessage("Hello from the worker thread");
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop)
</details>

<details>
<summary>Опишите the event-driven programming in Node.js.</summary>

### Опишите the event-driven programming in Node.js.

**Definition:**

Событийный код реагирует, когда что-то происходит — запрос, кусок файла, таймер, закрытый сокет — вместо того чтобы сидеть в цикле, пока это закончится. В Node обычная форма: эмиттер, слушатель и цикл событий, который возвращается к слушателю после другой работы.

**How it works:**

- `EventEmitter` — реестр. `on(name, listener)` сохраняет callback. `emit(name, ...args)` вызывает слушателей этого имени.
- `emit` запускает слушателей на текущем стеке, в порядке регистрации. Он не откладывает их на потом и не создаёт поток. Если слушатель сам начинает ввод-вывод, тот завершится позже на цикле.
- Стек вызовов выполняет JavaScript, который уже идёт. Когда он пустеет, цикл берёт следующий таймер, callback ввода-вывода, `setImmediate` или callback закрытия.
- Многие объекты ядра — эмиттеры: HTTP-сервер (`request`), читаемый поток (`data`, `end`, `error`), сокет (`close`). Подписываются вместо блокировки до появления результата.
- Без слушателя `error` процесс может упасть. Подпишитесь на `error` или берите API, которые возвращают промис и делают reject.
- Промисы и `async`/`await` сидят на том же цикле. Исполненный промис ставит микрозадачу. Это не второй поток.

**When to use:**

- HTTP-сервер, который ждёт соединения.
- Файловые и сетевые потоки, которые отдают куски по мере поступления.
- Свой код с несколькими слушателями: шина задач, сигнал процесса, свой `EventEmitter`.

**Pluses and minuses:**

- Много соединений остаются дешёвыми: каждое — подписка, а не заблокированный поток.
- Поток управления труднее читать, когда каждый шаг — callback. Промисы и `async`/`await` оставляют тот же цикл и выпрямляют эту форму.
- Слушатель, который бросает исключение или не обрабатывает `error`, роняет больше, чем одно событие.

**Examples:**

```js
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();

emitter.on("event-name", (eventArgs) => {
  console.log(`Event-name was emitted with arguments: ${eventArgs}`);
});

emitter.emit("event-name", "Some Payload");
```

**Sources:**

- [Node.js — Events](https://nodejs.org/api/events.html)
- [Node.js — The event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
</details>

<details>
<summary>Что такое the event loop in Node.js?</summary>

### Что такое the event loop in Node.js?

**Definition:**

Цикл событий — это круг libuv, который выполняет таймеры, callback ввода-вывода и закрытия, когда текущий стек JavaScript простаивает. Поэтому один поток чередует много операций. Node поднимает его при старте и крутит, пока не останется работы и открытых хендлов.

**How it works:**

- Каждый оборот (Node.js 20+ / libuv 1.45) идёт так: отложенные callback, idle/prepare (внутренние), poll, check (`setImmediate`), callback закрытия, затем таймеры. Просроченные таймеры могут выполниться ещё один раз до первой итерации. Полная картина: [Event loop](./8_nodejs_core.md#event-loop).
- Poll — фаза, которая блокируется, когда больше ничего не готово, до следующего таймера. Это не одна «очередь callback», общая для всех API.
- `process.nextTick` — не фаза. Его очередь осушается после текущей операции, до продолжения цикла. Callback nextTick, который ставит ещё один nextTick, может заморить ввод-вывод.
- Реакции промисов и `queueMicrotask` используют очередь микрозадач. Node осушает её сразу после очереди nextTick. В CommonJS сначала nextTick. При вычислении ESM тело модуля уже микрозадача, поэтому `queueMicrotask` идёт раньше `nextTick`.
- `setTimeout(fn, 0)` — таймер. `setImmediate(fn)` — фаза check. Их порядок друг относительно друга не фиксирован, если оба запланированы из главного модуля: таймер может успеть или не успеть до первого poll.
- Долгий синхронный цикл блокирует все фазы, включая новые соединения. Цикл живёт на поток: у воркера свой цикл, и он не делит очереди родителя.
- Браузерное слово «macrotask» не называет фазы Node. Таймеры, callback poll и `setImmediate` — отдельные очереди libuv.

**When to use:**

- Объяснить, почему `console.log` после `readFile` печатается первым.
- Объяснить, почему цикл `for` на две секунды замораживает каждый запрос этого процесса.
- Берите `nextTick`, только когда код обязан выполниться до продолжения ввода-вывода. Для обычной отсрочки лучше промис или `queueMicrotask`, а `setImmediate` — чтобы вернуть управление циклу.

**Examples:**

```js
console.log("Start");

setTimeout(() => {
  console.log("timer");
  Promise.resolve().then(() => console.log("promise inside timer"));
}, 0);

setImmediate(() => console.log("setImmediate"));

process.nextTick(() => {
  console.log("nextTick");
  process.nextTick(() => console.log("nextTick nested"));
});

Promise.resolve().then(() => console.log("promise"));

console.log("End");
```

**Sources:**

- [Node.js — The event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>Что такое the difference between Node.js and traditional web server technologies?</summary>

### Что такое the difference between Node.js and traditional web server technologies?

**Definition:**

Сервер «поток на запрос» тратит поток ОС на каждое соединение, в том числе пока этот поток ждёт ввод-вывод. Node держит один поток JavaScript на процесс и продолжает другую работу, пока ввод-вывод не завершился. «Классический» здесь — эта модель (старый Apache, многие серверы Java и .NET), а не любой сервер до Node. nginx уже событийный.

**How it works:**

- Поток на запрос: у клиента может быть свой поток. Поток занимает память, даже когда заблокирован на базе или файле. Потолок примерно равен числу потоков, которые машина готова планировать.
- Node: один стек вызовов. Асинхронный ввод-вывод регистрирует callback и возвращается. Цикл выполняет чужой запрос, пока первое ожидание продолжается. Простаивающие соединения дёшевы, пока работа не упирается в CPU.
- Блокирующий ввод-вывод держит вызывающего, пока диск или сокет не вернут результат. Неблокирующий возвращается сразу. Callback, промисы и `async`/`await` — способ забрать результат позже.
- Серверы на Java, C# или PHP часто ставят другой язык на сервере и JavaScript в браузере. Node использует JavaScript с обеих сторон, поэтому проверку данных и небольшие утилиты можно делить. DOM браузера на сервере по-прежнему нет.
- Скрипт правят и запускают снова. V8 компилирует горячий код, пока процесс работает. Отдельный бинарник приложения, как у `javac`, не производят. Исключение — нативные дополнения: их нужно пересобирать.
- Зависимости приходят из npm (или Yarn, или pnpm) и `package.json`. У других стеков свои репозитории, например Maven или NuGet.
- Развёртывание Node — процесс и порт, часто за обратным прокси, с менеджером процессов и TLS. Операционную работу это не отменяет. Модуль `cluster` запускает один процесс на ядро, когда одного цикла мало.
- Модель подходит для множества одновременных соединений и realtime-раздачи. Тяжёлые вычисления, пакетные задачи и часть корпоративных стеков лучше ложатся на многопоточную среду или на воркер рядом с Node.

**When to use:**

- Node: API, чат, потоковая передача, прокси и сервисы, которые больше ждут ввод-вывод, чем считают.
- Потоковый сервер: тяжёлая по CPU работа в запросе, которую не хотите изолировать сами, или платформа (JVM, .NET), которую команда уже эксплуатирует.

**Sources:**

- [Node.js — About](https://nodejs.org/en/about)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Объясните what "non-blocking" means in Node.js.</summary>

### Объясните what "non-blocking" means in Node.js.

**Definition:**

Неблокирующий вызов начинает работу, которая может занять время, и возвращается до её конца. Процесс продолжает другой JavaScript. Результат приходит позже на цикле событий: через callback, промис или `async`/`await`.

**How it works:**

- При блокирующем вводе-выводе вызывающий сидит внутри вызова, пока ресурс не готов. На этом потоке больше ничего не выполняется.
- При неблокирующем вызов просит систему продолжить и возвращается. Пока идёт чтение файла, сервер может принять другой запрос.
- `fs.readFile` отдаёт чтение пулу потоков libuv и вызывает callback, когда байты готовы. `fs.readFileSync` блокирует. Цикл не примет новую работу, пока вызов не вернётся.
- Сетевое чтение неблокирующее на самом потоке цикла. Ядро сообщает о готовности, затем JavaScript запускает обработчик. Пока идёт ожидание, поток пула не занят.
- Это конкурентность на одном потоке JavaScript, а не поток на запрос. Callback всё равно идут по одному. Параллельная работа на CPU требует воркеров или процессов.
- Функция может выглядеть асинхронной и всё же блокировать, если callback делает длинное синхронное вычисление. Неблокирующий ввод-вывод не делает неблокирующим JavaScript внутри callback.
- Потоки данных — та же идея для больших объёмов: обрабатываете кусок и возвращаетесь, вместо ожидания всего файла.

**When to use:**

- Обработчики запросов, раздача файлов, клиенты баз и всё остальное в серверном процессе.
- Синхронные методы уместны в коротком CLI, который делает одно дело и выходит.

**Examples:**

```js
import { readFile } from "node:fs";

readFile("notes.txt", "utf8", (err, text) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(text);
});

console.log("This message is displayed immediately.");
```

**Sources:**

- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>Как you update Node.js to the latest version?</summary>

### Как you update Node.js to the latest version?

**Definition:**

Заменяют бинарник `node`. Это делает менеджер версий или установщик с nodejs.org. `npm install` обновляет пакеты проекта. Среду выполнения он не обновляет, и ставить Node через npm — не поддерживаемый путь обновления.

**How it works:**

- Менеджеры версий ставят выпуски рядом и направляют `PATH` на один из них. `nvm` и `fnm` — обычные инструменты Unix. `nvs` работает на разных платформах. Пакет `n` тоже переключает версии, но чтобы его поставить, Node уже должен работать, поэтому для первой установки он плох.
- Официальный установщик и пакеты ОС заменяют системный бинарник. На macOS формула Homebrew `node` следит за выпуском. На Windows прямой путь — установщик с nodejs.org, winget или менеджер версий.
- На сервере фиксируют текущий LTS. «Latest» включает линию Current, которая не является долгосрочной.
- Результат проверяют через `node -v` и `npm -v`. `npm install -g npm` может обновить CLI npm, который пришёл с Node. На другую мажорную версию Node это всё равно не переводит.
- Нативные дополнения собираются под ABI Node. После мажорного обновления зависимости ставят заново, чтобы дополнения пересобрались.
- `npm cache clean` — не шаг обновления.

**When to use:**

- Локальные машины: менеджер версий, чтобы каждый репозиторий мог зафиксировать свой мажор.
- CI и production: тот же мажор, что в `.nvmrc` или `engines`, поставленный образом или установщиком, а не плавающим `npm install -g`.

**Examples:**

```bash
nvm install --lts
nvm use --lts
node -v
npm -v
```

**Sources:**

- [Node.js — Download](https://nodejs.org/en/download)
- [Node.js releases](https://nodejs.org/en/about/previous-releases)
</details>

<details>
<summary>Что такое npm and what is it used for?</summary>

### Что такое npm and what is it used for?

**Definition:**

npm — менеджер пакетов, который идёт с Node, и имя публичного реестра, с которым он говорит. CLI ставит библиотеки, записывает диапазоны версий, запускает скрипты проекта и может опубликовать ваш пакет.

**How it works:**

- Клиент — команда `npm` на машине. Реестр — база опубликованных пакетов на registry.npmjs.org. Сайт npmjs.com — браузерный вид этого реестра, а не второй установщик.
- `npm install` читает `package.json`, скачивает дерево в `node_modules` и пишет `package-lock.json`, чтобы следующая установка повторила те же версии.
- `npm init` создаёт `package.json`. `npm publish` выкладывает пакет. `npm install <pkg>` добавляет зависимость. `npm uninstall <pkg>` удаляет её.
- `scripts` в `package.json` — именованные команды. `npm start` и `npm test` особые, им не нужен `run`. Остальное — `npm run <name>`. Скрипты видят `node_modules/.bin` в `PATH`.
- Диапазоны версий и lock-файл делают установки предсказуемыми. CI должен ставить из lock-файла (`npm ci`), а не заново разрешать каждый диапазон.
- Yarn и pnpm — другие клиенты. У них свои lock-файлы (`yarn.lock`, `pnpm-lock.yaml`), и реестр может быть тот же. И npm, и Yarn детерминированы, если lock-файл закоммичен. Старый разрыв был в поведении установки, а не в том, что lock-файл есть только у Yarn.

**When to use:**

- Каждый проект Node: объявить зависимости, поставить их и запускать сборку, тесты и старт из `scripts`.
- Публикация библиотеки: `npm publish`, когда у пакета есть имя, версия и лицензия.

**Examples:**

```json
{
  "scripts": {
    "start": "node server.js"
  }
}
```

```bash
npm install express
npm install --save-dev vitest
npm start
npm run test
```

**Sources:**

- [npm Docs — About npm](https://docs.npmjs.com/about-npm)
- [npm Docs — npm-run-script](https://docs.npmjs.com/cli/v10/commands/npm-run-script)
</details>

<details>
<summary>Как you manage packages in a Node.js project?</summary>

### Как you manage packages in a Node.js project?

**Definition:**

Проект объявляет зависимости и скрипты в `package.json` и фиксирует установленное дерево в lock-файле. npm, Yarn или pnpm ставят, обновляют и удаляют модули по этому описанию. В репозитории выбирают один клиент и коммитят его lock-файл.

**How it works:**

- `npm init` создаёт манифест. `npm install <pkg>` добавляет запись в `dependencies` и обновляет `package-lock.json`. `--save-dev` (`-D`) записывает `devDependency` (TypeScript, тест-раннер). У Yarn добавление — `yarn add <pkg>`, а для dev — `yarn add -D <pkg>`.
- `npm update <pkg>` двигает установленные пакеты внутри диапазонов, уже разрешённых в `package.json`. Он не игнорирует диапазон и не прыгает на любой тег latest. Мажорный скачок — это осознанная правка версии, затем установка.
- `npm uninstall <pkg>` убирает пакет из `node_modules` и из манифеста. У Yarn это `yarn remove`.
- Оба клиента пишут lock-файл: `package-lock.json`, `yarn.lock` или `pnpm-lock.yaml`. Этот файл делает установки повторяемыми на разных машинах. Его коммитят. CI должен вызывать `npm ci` (или замороженную установку Yarn/pnpm), которая не даёт отойти от lock-файла.
- `node_modules` — результат установки. Его не коммитят. Production-установка может опустить devDependencies (`npm ci --omit=dev`).
- Скрипты в `package.json` — повторяемые команды проекта: `npm run <name>` из каталога проекта.

**When to use:**

- Репозитории приложений: один lock-файл, `npm ci` в CI, диапазоны не шире того, что готовы принять на следующем обновлении.
- Библиотеки: диапазоны в `dependencies`, чтобы приложения могли схлопывать версии; собственные инструменты разработки библиотеки всё равно фиксируют lock-файлом.

**Examples:**

```bash
npm install lodash
npm install express --save-dev
npm update lodash
npm uninstall lodash
npm ci
```

**Sources:**

- [npm Docs — package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [npm Docs — npm-ci](https://docs.npmjs.com/cli/v10/commands/npm-ci)
</details>

<details>
<summary>Что такое package.JSON file?</summary>

### Что такое package.JSON file?

**Definition:**

`package.json` — манифест проекта Node или публикуемого пакета. Он называет проект, перечисляет нужные пакеты и задаёт команды, которые может запустить npm. Его читают установщик и, для опубликованных пакетов, загрузчик модулей Node.

**How it works:**

- `name` и `version` идентифицируют пакет. Они обязательны, если вы публикуете. `private: true` помечает приложение, которое нельзя публиковать.
- `description`, `author`, `license` и `repository` — метаданные. Они важны, когда пакет публичный.
- `dependencies` нужны в рантайме. `devDependencies` нужны для сборки или тестов. `optionalDependencies` могут не установиться, не роняя всё дерево. `peerDependencies` — пакеты, которые должно поставить приложение-хост (например, ожидаемый React у плагина); внутрь плагина они по умолчанию не ставятся.
- Диапазоны версий следуют semver. `^1.2.3` разрешает совместимые minor и patch. `~1.2.3` разрешает patch. Lock-файл записывает версию, которая была установлена на самом деле.
- `scripts` сопоставляет имя команде оболочки. `npm start` и `npm test` — обычные короткие команды. `npm run build` запускает то, что лежит в `build`.
- `main`, `exports` и `type` говорят Node, как загружать пакет. `engines` говорит, какие версии Node вы ожидаете. Файл создают через `npm init` или `yarn init`. Ручная смена версии без обновления lock-файла разъедет следующую установку.

**When to use:**

- Каждый проект, включая один скрипт, как только появляется зависимость или команда, которую хотите повторять.
- CI читает тот же файл: установка из lock-файла, затем `npm test` или `npm run build`. Манифест — контракт; системе CI не нужна своя копия команды, если скрипт определён здесь.

**Examples:**

```json
{
  "name": "notes",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "start": "node server.js",
    "build": "tsc",
    "test": "node --test"
  },
  "dependencies": { "express": "^5.1.0" }
}
```

**Sources:**

- [npm Docs — package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [Node.js — package.json](https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions)
</details>

### Node.js Core Modules

<details>
<summary>Опишите some of the core modules of Node.js.</summary>

### Опишите some of the core modules of Node.js.

**Definition:**

Встроенные модули лежат внутри бинарника `node`. Их импортируют с префиксом `node:` и не ставят из реестра. Они закрывают операционную систему, файлы, сеть и типы данных, которыми среда пользуется для ввода-вывода.

**How it works:**

- Система: `os` сообщает память, CPU и платформу (`os.freemem()`, `os.totalmem()`). `process` — текущий процесс (env, exit, `nextTick`). `util` держит мелкие помощники вроде `inspect` и `promisify`.
- Файлы: `fs` читает, пишет и следит за файлами. `path` собирает и разбирает строки путей, не трогая диск. `url` разбирает и форматирует URL.
- Сеть: `http` и `https` реализуют HTTP-сервер и клиент (`http.createServer`). `net` — TCP. `dgram` — UDP.
- Данные: `stream` — ввод-вывод кусками. `buffer` — сырые байты. `zlib` сжимает. `crypto` считает хеши и HMAC. `events` — это `EventEmitter`.
- Процессы: `child_process` порождает процессы ОС. `worker_threads` запускает дополнительные изоляты JavaScript. `cluster` форкает воркеры, которые делят порт сервера.
- Предпочитайте `node:fs` голому имени `fs`. Локальный файл `fs.js` не затенит встроенный модуль, если указана схема.
- Эти модули есть в каждом процессе Node. В браузерной сборке их нет, пока бандлер не подставит полифил, и большинство не стоит тащить в клиентский код.

**When to use:**

- Берите встроенный модуль раньше зависимости, если задача — файлы, HTTP, пути, crypto или процессы. Пакет добавляют, когда нужны маршруты, драйвер базы или протокол, которого в Node нет.

**Examples:**

```js
import os from "node:os";
import { createServer } from "node:http";

console.log(os.freemem(), os.totalmem());
createServer((_req, res) => res.end("ok")).listen(0);
```

**Sources:**

- [Node.js — Core modules](https://nodejs.org/api/modules.html#built-in-modules)
</details>

<details>
<summary>Как you create a simple server in Node.js using the HTTP module?</summary>

### Как you create a simple server in Node.js using the HTTP module?

**Definition:**

`http.createServer` строит сервер. Его слушатель запроса выполняется один раз на запрос и получает `IncomingMessage` (`req`) и `ServerResponse` (`res`). `listen` занимает TCP-порт и начинает принимать соединения.

**How it works:**

- Импортируйте `node:http`. Передайте слушатель в `createServer` или вызовите `server.on("request", listener)`.
- `req` — входящее сообщение: `req.method`, `req.url` (путь плюс query string) и `req.headers`. Тело — не строка. Это читаемый поток, который потребляют, если клиент его прислал.
- `res` — исходящее сообщение. `writeHead(status, headers)` задаёт статус и заголовки. `write` отправляет кусок. `end` отправляет необязательные последние байты и завершает ответ. Пока нет `end`, клиент остаётся открытым.
- `server.listen(port)` начинает приём. Callback `listening` выполняется один раз, когда порт занят. Порт `0` просит у ОС свободный порт.
- Маршрутизация — ваш `if` по `req.url` и `req.method`. Это сервер, который оборачивает Express. Фреймворк добавляет роутер, разбор тела и обработку ошибок, которые иначе пишут здесь.
- Один слушатель обслуживает все соединения на цикле событий. Медленное синхронное тело в этой функции блокирует остальные.

**When to use:**

- Крошечный сервис, проверка здоровья или разбор сырого жизненного цикла запроса.
- Для настоящего API фреймворк обычно короче. Модуль HTTP всё равно то, что этот фреймворк вызывает.

**Examples:**

```js
import http from "node:http";

const requestListener = (req, res) => {
  if (req.url === "/profile") {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("Welcome to your profile");
    return;
  }
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("Hello, World!");
};

const server = http.createServer(requestListener);
server.listen(8080);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

<details>
<summary>Объясните the puRPOse of the File System (fs) module.</summary>

### Объясните the puRPOse of the File System (fs) module.

**Definition:**

`node:fs` — встроенный модуль для файлов и каталогов на локальной машине: чтение, запись, переименование, список, права и ссылки. Ставить его не нужно. Страница в браузере его вызвать не может.

**How it works:**

- У большинства операций есть форма с callback и форма с промисом (`node:fs/promises`). Обе сразу возвращают управление и заканчивают работу на цикле событий. Файловая работа идёт через пул потоков libuv.
- Синхронные пары заканчиваются на `Sync` (`readFileSync`, `renameSync`). Они блокируют цикл событий, пока ядро не вернётся. Это приемлемо в одноразовом скрипте и плохо внутри HTTP-обработчика, который делит процесс с другими соединениями.
- Ввод-вывод: `readFile` / `writeFile` для целого файла или `createReadStream` / `createWriteStream`, когда файл большой.
- Метаданные: `stat` даёт размер, режим и временные метки. `access` проверяет права.
- Каталоги: `mkdir`, `readdir`, `rm`. `rename` и `unlink` перемещают и удаляют. `link` и `symlink` создают жёсткие и символические ссылки.
- Пути — пути ОС. Собирайте их через `node:path` или берите `file:` URL относительно `import.meta.url`. Не стройте пути склейкой строк.
- Каждый асинхронный вызов может упасть. Обрабатывайте аргумент `err` или `await` внутри `try`/`catch`. Голый `throw` внутри callback — необработанное исключение.

**When to use:**

- Серверный код, CLI и инструменты сборки, которые трогают диск.
- На сервере предпочитайте промисы или callback. Для больших файлов — потоки. `*Sync` оставьте коду старта и коротким скриптам.

**Examples:**

```js
import { readFile } from "node:fs/promises";
import { readFileSync } from "node:fs";

const text = await readFile(new URL("./input.txt", import.meta.url), "utf8");
console.log("Asynchronous read:", text);

const syncText = readFileSync(new URL("./input.txt", import.meta.url), "utf8");
console.log("Synchronous read:", syncText);
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>Что такое the Buffer class in Node.js?</summary>

### Что такое the Buffer class in Node.js?

**Definition:**

`Buffer` — последовательность сырых байтов фиксированного размера. Файлы, сокеты и crypto работают с октетами, а не со строками JavaScript, и буфер — объект, который эти API передают. Он глобальный и также живёт в `node:buffer`.

**How it works:**

- Это подкласс `Uint8Array`. Длина фиксируется при выделении. Чтобы «изменить размер», выделяют другой буфер и копируют.
- `Buffer.from("Hey!")` кодирует строку (по умолчанию utf8). `buf.toString()` декодирует её обратно. `Buffer.alloc(5, "a")` создаёт обнулённый буфер и заполняет его байтом `a`.
- `buf.write(string)` копирует закодированные байты в существующий буфер и останавливается на `buf.length`. Буфер не растёт.
- `Buffer.alloc` обнуляет память. `Buffer.allocUnsafe` это пропускает и может показать старые байты, если не перезаписать каждый индекс.
- `subarray(start, end)` — вид на ту же память. Запись через вид меняет оригинал. Копируйте, когда нужен независимый буфер.
- Куски HTTP и файлов часто приходят буферами. Декодируйте явной кодировкой, когда нагрузка — текст. Большое тело передавайте потоком, а не склейкой каждого куска в один буфер.
- `Buffer` — не массив символов. `Buffer.from("é").length` — длина в байтах, в utf8 она больше 1.

**When to use:**

- Бинарные протоколы, хеши, куски файлов и всё, что нельзя исказить кодировкой строки.
- Текст в коде приложения остаётся строкой, пока не пересекает границу байтов.

**Examples:**

```js
const fromText = Buffer.from("Hey!");
console.log(fromText.toString());

const allocated = Buffer.alloc(5, "a");
console.log(allocated.toString());

allocated.write("Hello");
console.log(allocated.toString());

const head = allocated.subarray(0, 3);
console.log(head.toString());
```

**Sources:**

- [Node.js — Buffer](https://nodejs.org/api/buffer.html)
</details>

<details>
<summary>Что такое streams in Node.js and what types are available?</summary>

### Что такое streams in Node.js and what types are available?

**Definition:**

Поток двигает данные кусками, а не держит всю нагрузку в памяти. В Node четыре вида: readable, writable, duplex и transform. Стандартный ввод, вывод и ошибка — тоже потоки.

**How it works:**

- Readable: из него читают. `fs.createReadStream` и тело HTTP-запроса — readable. Их потребляют через `for await` или событие `data`, затем `end`.
- Writable: в него пишут. `fs.createWriteStream` и HTTP-ответ — writable. `write` отправляет кусок. `end` завершает. `process.stdout` и `process.stderr` — writable. `process.stdin` — readable.
- Duplex: оба направления на одном объекте, как у TCP-сокета (`net.Socket`). Сторона чтения и сторона записи независимы.
- Transform: duplex, который меняет куски по пути, например `zlib.createGzip()` или поток хеша.
- `pipe` соединяет readable с writable. `pipeline` из `node:stream/promises` соединяет всю цепочку, передаёт ошибки и уничтожает потоки, если один упал. Его предпочитают ручному `pipe`, когда шагов больше одного.
- Обычная цепочка читает файл, сжимает байты gzip и пишет результат. Память остаётся около размера куска, а не файла.
- Важно обратное давление. `write` возвращает `false`, когда приёмник отстаёт. Ждут `drain`, прежде чем производить ещё. `pipeline` делает это за вас.
- HTTP — потоки в обе стороны: клиент пишет запрос и читает ответ; сервер читает запрос и пишет ответ.

**When to use:**

- Большие файлы, загрузки, сжатие и любой ответ, который можно отправлять по мере готовности.
- Небольшие конфиги проще через `readFile`. Поток — лишняя механика, когда нагрузку специально держат в памяти.

**Examples:**

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";
import { pipeline } from "node:stream/promises";

await pipeline(
  createReadStream("notes.txt"),
  createGzip(),
  createWriteStream("notes.txt.gz"),
);
```

**Sources:**

- [Node.js — Stream](https://nodejs.org/api/stream.html)
</details>

<details>
<summary>Как you read and write files in Node.js?</summary>

### Как you read and write files in Node.js?

**Definition:**

Берите `readFile` и `writeFile` из `node:fs/promises`, когда файл целиком помещается в память. Передавайте путь или `file:` URL и кодировку вроде `"utf8"`, если содержимое — текст. Большой файл читайте и пишите потоками `createReadStream` и `createWriteStream`, а не целиком в буфер.

**How it works:**

- Форма на промисах: `import { readFile, writeFile } from "node:fs/promises"`. `await readFile(path, "utf8")` резолвится строкой. Без кодировки промис резолвится `Buffer`.
- `writeFile(path, data, "utf8")` создаёт файл или заменяет его. `data` может быть строкой, `Buffer` или `TypedArray`. Вызов возвращает промис.
- Форма с колбэком: `readFile` из `node:fs` принимает `(err, data)`. Документация указывает на колбэк, когда нужна минимальная накладная стоимость.
- Обе асинхронные формы выполняют работу с файловой системой в пуле потоков libuv и завершаются в цикле событий. Одновременные записи в один файл не синхронизированы: процесс может повредить этот файл.
- `readFile` буферизует весь файл. Документация советует `createReadStream`, когда файл большой.
- `file:` URL из `new URL("./note.txt", import.meta.url)` — допустимый путь. Он привязан к модулю, в отличие от `process.cwd()`.

**When to use:**

- Конфиг, шаблоны и небольшие документы: `readFile` / `writeFile`.
- Загрузки, логи и крупные артефакты: потоки, обычно через `pipeline` из `node:stream/promises`.

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const fileUrl = new URL("./note.txt", import.meta.url);

await writeFile(fileUrl, "hello\n", "utf8");
const text = await readFile(fileUrl, "utf8");
console.log(text);
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>Как you use the EventEmitter in Node.js?</summary>

### Как you use the EventEmitter in Node.js?

**Definition:**

`EventEmitter` из `node:events` — реестр именованных слушателей. Его наследуют или используют напрямую: регистрируют функции через `on` или `once` и вызывают `emit`, когда что-то произошло. Многие объекты ядра (`http.Server`, потоки, `process`) — это эмиттеры.

**How it works:**

- `on(name, listener)` добавляет слушателя в конец. `once(name, listener)` снимает его после первого вызова. `emit(name, ...args)` вызывает слушателей этого имени в порядке регистрации.
- `removeListener(name, listener)` удаляет одну регистрацию. Функция должна быть той же ссылкой, которую регистрировали.
- Событие `'error'` без слушателя бросает ошибку, печатает стек и завершает процесс. На каждый свой эмиттер вешайте слушателя `'error'`.
- `listenerCount(name)` и `eventNames()` показывают, что ещё подписано. Слушатель, которого не сняли, удерживает эмиттер в памяти.
- `prependListener` вставляет слушателя в начало. По умолчанию вызовы идут в порядке добавления.
- `events.once(emitter, name)` возвращает промис на следующее подходящее событие — удобно с `await`.

**When to use:**

- Свой pub/sub внутри одного процесса: прогресс, остановка, завершение задачи.
- Объекты ввода-вывода ядра уже эмитят события. Подписывайтесь на них, а не заводите вторую шину поверх.

**Examples:**

```js
import { EventEmitter } from "node:events";

class JobEmitter extends EventEmitter {}

const jobs = new JobEmitter();

jobs.on("error", (err) => {
  console.error(err.message);
});

jobs.once("done", (id) => {
  console.log("finished", id);
});

jobs.emit("done", 7);
jobs.emit("error", new Error("disk full"));
```

**Sources:**

- [Node.js — Events](https://nodejs.org/api/events.html)
</details>

<details>
<summary>Что такое the QueryString module?</summary>

### Что такое the QueryString module?

**Definition:**

`node:querystring` разбирает и собирает query-строки URL. `parse` превращает `foo=bar&abc=xyz` в объект. `stringify` собирает объект обратно в query-строку. Это API Node: на длинных строках он быстрее `URLSearchParams` и при этом не является стандартом WHATWG.

**How it works:**

- `querystring.parse(str)` делит строку по `&` и `=`, декодирует percent-encoding и собирает повторяющиеся ключи в массив.
- `querystring.stringify(obj)` кодирует ключи и значения. Значение-массив повторяет ключ (`baz=qux&baz=quux`).
- Кодировка по умолчанию — utf8. Опции `sep` и `eq` меняют разделители.
- `URL` и `URLSearchParams` — глобалы и следуют правилам браузера. Документация рекомендует их, когда нужно то же поведение, что у браузерного кода.
- `querystring` остаётся для кода, который уже зависит от его правил разбора. Эти правила расходятся с `URLSearchParams` на краевых случаях (например, как трактуется `+`).
- У запроса `node:http` query лежит в `req.url`. Отрежьте часть после `?` или передайте URL в `new URL(req.url, "http://localhost")` и читайте `searchParams`.

**When to use:**

- `URLSearchParams` в новом коде, который должен совпадать с веб-платформой.
- `node:querystring`, когда измеренный горячий путь требует именно этот парсер, или когда нужно сохранить стабильный результат старого разбора.

**Examples:**

```js
import { parse, stringify } from "node:querystring";

const parsed = parse("foo=bar&abc=xyz&abc=123");
console.log(parsed.foo, parsed.abc);

const built = stringify({ foo: "bar", baz: ["qux", "quux"] });
console.log(built);

const params = new URLSearchParams("foo=bar&abc=xyz");
console.log(params.get("foo"));
```

**Sources:**

- [Node.js — Query string](https://nodejs.org/api/querystring.html)
- [Node.js — URL](https://nodejs.org/api/url.html)
</details>

<details>
<summary>Как you manage path operations in Node.js?</summary>

### Как you manage path operations in Node.js?

**Definition:**

`node:path` соединяет, раскрывает и разбирает пути файловой системы. Берите `join` и `resolve`, а не склейку строк: у Windows и POSIX разные разделители. Методы `path` следуют операционной системе, на которой запущен процесс. `path.posix` и `path.win32` дают фиксированный стиль на любой машине.

**How it works:**

- `path.join(...segments)` склеивает сегменты разделителем платформы и нормализует `.` и `..`. Пустые сегменты игнорируются. Сегмент не-строка бросает `TypeError`.
- `path.resolve(...segments)` строит абсолютный путь справа налево. Если сегменты относительные, старт — `process.cwd()`.
- `path.basename(p, suffix)` — последний сегмент. Суффикс снимается только при точном совпадении, сравнение чувствительно к регистру.
- `path.dirname` — каталог. `path.extname` — расширение вместе с точкой.
- `path.sep` — `\` в Windows и `/` в POSIX. `path.basename('C:\\temp\\myfile.html')` на POSIX не режет строку по обратным слэшам. Для Windows-пути на любой ОС берите `path.win32`.
- Модуль должен опираться на `new URL("./data", import.meta.url)` или `path.join` от известного каталога. `process.cwd()` меняется, если процесс запустили из другой папки.

**When to use:**

- Любой пользовательский путь, имя загрузки или расположение конфига, которое вы соединяете с базовым каталогом.
- Нормализуйте путь до проверки, что результат остаётся внутри разрешённого корня (`path.resolve` и проверка префикса).

**Examples:**

```js
import path from "node:path";

const joined = path.join("/foo", "bar", "baz/asdf", "quux", "..");
console.log(joined);

const base = path.basename("/foo/bar/baz/asdf/quux.html", ".html");
console.log(base);

const fileUrl = new URL("./note.txt", import.meta.url);
console.log(fileUrl.pathname);
```

**Sources:**

- [Node.js — Path](https://nodejs.org/api/path.html)
</details>

### Asynchronous Programming

<details>
<summary>Что такое callbacks in Node.js?</summary>

### Что такое callbacks in Node.js?

**Definition:**

Колбэк — функция, которую вы передаёте, чтобы она выполнилась позже, когда операция закончится. В I/O API ядра Node обычная форма — error-first: первый аргумент `err` (`null` при успехе), дальше результат.

**How it works:**

- `readFile(path, (err, data) => { ... })` из `node:fs` возвращает управление сразу. Колбэк выполняется на следующем обороте цикла событий, после того как пул потоков закончит чтение.
- Проверяйте `err` до использования `data`. Брошенная внутри колбэка ошибка — необработанное исключение, если её больше никто не перехватит.
- Одна и та же функция может вызваться синхронно или на следующем тике. API, который иногда вызывает колбэк до возврата, трудно рассуждать. Документация `process.nextTick` показывает, как отложить синхронную ветку, чтобы колбэк всегда был асинхронным.
- Колбэки складываются вложенностью или именованной следующей функцией. Они не возвращают значение вызывающему `readFile`.
- `util.promisify` оборачивает error-first функцию в промис. У многих модулей уже есть promise API (`node:fs/promises`).
- Таймеры (`setTimeout`, `setImmediate`) тоже колбэки. Они ставятся в цикл событий, а не в пул потоков.

**When to use:**

- Вызываете callback-стиль ядра и вам нужна меньшая стоимость аллокаций, о которой пишет документация fs.
- Новый код приложения обычно ждёт promise API. Колбэк оставляйте на границе, которая его всё ещё требует.

**Examples:**

```js
import { readFile } from "node:fs";

readFile(new URL("./note.txt", import.meta.url), "utf8", (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>Что такое callback hell and how can it be avoided?</summary>

### Что такое callback hell and how can it be avoided?

**Definition:**

Callback hell — глубоко вложенные error-first колбэки, где каждый следующий шаг стартует только внутри предыдущего. Поток трудно читать, и на каждом уровне нужна своя проверка `err`. Выравнивайте это именованными функциями, промисами или `async`/`await`.

**How it works:**

- Вложенность растёт, когда шаг B зависит от результата A, а шаг C — от B: прочитать путь из одного файла, затем прочитать этот путь, затем записать результат.
- Каждый колбэк замыкает внешние переменные. Ранний `return` после `err` нужен на каждом уровне, иначе следующий шаг пойдёт по ошибке.
- Именованная функция на шаг (`readConfig`, `readBody`, `writeOut`) держит вложенность на одном уровне. Следующую функцию передают аргументом.
- `node:fs/promises` превращает те же шаги в `await` внутри одного `try`/`catch`. Ошибка попадает в `catch`, а не в аргумент `err`.
- `Promise.all` запускает независимые шаги вместе. Вложенные колбэки прячут эту независимость и гоняют шаги по очереди.
- `pipeline` из `node:stream/promises` заменяет цепочку колбэков потоков и пробрасывает первую ошибку.

**When to use:**

- Любая последовательность I/O в коде приложения: предпочтителен `async`/`await`.
- Колбэк оставляйте там, где API только callback-стиль и обёртка в промис не окупается.

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const dir = new URL("./", import.meta.url);

try {
  const name = (await readFile(new URL("name.txt", dir), "utf8")).trim();
  const body = await readFile(new URL(name, dir), "utf8");
  await writeFile(new URL("out.txt", dir), body.toUpperCase(), "utf8");
} catch (err) {
  console.error(err);
}
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Объясните promises in Node.js.</summary>

### Объясните promises in Node.js.

**Definition:**

Промис — объект, который обозначает значение, ещё не готовое. Он завершается один раз: fulfilled со значением или rejected с причиной. Promise API Node (`node:fs/promises`, `node:stream/promises`, `node:timers/promises`) возвращают промисы, чтобы складывать шаги без вложенных колбэков.

**How it works:**

- `then` выполняется при успехе. `catch` — при отказе. `finally` — в обоих случаях. Промис, возвращённый из `then`, продолжает цепочку ожидания.
- `async`-функция всегда возвращает промис. `await` приостанавливает эту функцию, пока промис не завершится, и продолжает её в микрозадаче.
- Отклонённый промис без `catch` или `await` в `try` порождает `unhandledRejection` на `process`. Обрабатывайте его на месте вызова.
- `Promise.all` ждёт все промисы и отклоняется на первой ошибке. `Promise.allSettled` ждёт каждый исход. `Promise.race` завершается первым результатом.
- `util.promisify(fn)` ждёт error-first колбэк. У fs promise API уже есть. Предпочтительнее этот импорт, чем оборачивать `node:fs` самому.
- Реакции промисов идут через очередь микрозадач. В CommonJS они выполняются после того, как осушится текущая очередь `process.nextTick`. В ES-модуле порядок относительно `nextTick` другой, потому что сам модуль выполняется из очереди микрозадач.

**When to use:**

- I/O приложения, последовательности шагов и всё, что должен покрывать `try`/`catch`.
- Callback-стиль остаётся на `node:fs`, когда применима заметка документации о производительности.

**Examples:**

```js
import { readFile } from "node:fs/promises";

readFile(new URL("./note.txt", import.meta.url), "utf8")
  .then((text) => {
    console.log(text);
  })
  .catch((err) => {
    console.error(err);
  });
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>Как async/await functions work in Node.js?</summary>

### Как async/await functions work in Node.js?

**Definition:**

`async` помечает функцию, которая возвращает промис. `await` приостанавливает эту функцию, пока промис не выполнится, и продолжает её со значением. Отказ бросает исключение на `await`, поэтому его ловит `try`/`catch`. Вызывающий `async`-функцию сразу получает промис.

**How it works:**

- `await` ставит на паузу только async-функцию, не процесс. Пока I/O в полёте, цикл событий делает другую работу.
- Код после `await` выполняется как продолжение микрозадачи. Он не блокирует поток так, как `readFileSync`.
- `await` на не-промисе оборачивает значение в разрешённый промис и продолжает на следующей микрозадаче.
- Параллельная независимая работа — `await Promise.all([readA(), readB()])`. Последовательные `await` складывают задержки.
- Top-level `await` разрешён в ES-модулях. В CommonJS тело оборачивают в `async`-функцию и вызывают её.
- `async`-функция, которая бросает или ждёт отказ, возвращает отклонённый промис. Если его никто не ждёт, `process` эмитит `unhandledRejection`.

**When to use:**

- Обработчики запросов, скрипты и любая последовательность promise API.
- На колбэках оставайтесь только на границе, у которой нет формы с промисом.

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const fileUrl = new URL("./note.txt", import.meta.url);

try {
  const text = await readFile(fileUrl, "utf8");
  await writeFile(fileUrl, text.trim() + "\n", "utf8");
} catch (err) {
  console.error(err);
}
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Что такое the difference between synchronous and asynchronous methods in the fs module?</summary>

### Что такое the difference between synchronous and asynchronous methods in the fs module?

**Definition:**

Асинхронные методы `node:fs` (`readFile` и те же имена в `node:fs/promises`) возвращают управление сразу и отдают результат позже. Синхронные методы заканчиваются на `Sync` (`readFileSync`) и возвращают результат только после ответа ядра, поэтому на главном потоке больше ничего не выполняется.

**How it works:**

- `readFile` / `fsPromises.readFile` ставят работу в пул потоков libuv. Цикл событий продолжает другие колбэки. Завершение приходит колбэком или выполненным промисом.
- `readFileSync` выполняется на главном потоке. Таймеры, сокеты и другие запросы ждут за ним.
- И promise API, и callback API асинхронные. Документация отмечает, что callback API меньше аллоцирует, когда нужна максимальная пропускная способность.
- Операции на промисах не потокобезопасны друг относительно друга. Две пересекающиеся записи в один файл могут его повредить. Вызовы `Sync` не пересекаются с JavaScript на том же потоке, потому что блокируют его.
- `readFile` в любом стиле буферизует весь файл. Большому файлу нужен `createReadStream` — он асинхронный.
- Ошибки: `Sync` бросает. Колбэк получает `err`. Промис отклоняется, поэтому его `await` стоит внутри `try`/`catch`.

**When to use:**

- `*Sync` в коде старта и коротких скриптах, до `listen` сервера.
- Колбэки или промисы внутри сервера, который делит процесс с другими соединениями.

**Examples:**

```js
import { readFile } from "node:fs/promises";
import { readFileSync } from "node:fs";

const fileUrl = new URL("./note.txt", import.meta.url);

const asyncText = await readFile(fileUrl, "utf8");
console.log("async", asyncText);

const syncText = readFileSync(fileUrl, "utf8");
console.log("sync", syncText);
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

### Networking in Node.js

<details>
<summary>Как Node.js handle HTTP requests and responses?</summary>

### Как Node.js handle HTTP requests and responses?

**Definition:**

`http.createServer` слушает TCP-соединения и на событие `'request'` отдаёт `IncomingMessage` (`req`) и `ServerResponse` (`res`). `req` — читаемый поток тела. `res` — записываемый поток, который завершают через `end`. На каждый запрос слушатель выполняется в цикле событий.

**How it works:**

- `req.method` и `req.url` определяют запрос. `req.headers` — карта с именами в нижнем регистре. Тело приходит чанками; его читают через `for await (const chunk of req)` или события `'data'` / `'end'`.
- `res.writeHead(statusCode, headers)` отправляет статус и заголовки. Вызывайте один раз, до `end`. Заголовки из `writeHead` перекрывают более ранние `setHeader`.
- `res.end(body)` завершает сообщение. `Content-Length` — длина в байтах. Для строки берите `Buffer.byteLength(body)`.
- `server.listen(port)` занимает порт. Событие `'error'` на сервере сообщает о неудачном bind, например `EADDRINUSE`.
- Событие `'upgrade'` — сырой хук HTTP-upgrade (в документации показан ответ 101). Это не WebSocket-сервер.
- `http.request` / `http.get` — клиент. Объект ответа тоже `IncomingMessage`.

**When to use:**

- Небольшой сервис, прокси или любой сервер, который вы хотите оставить на API ядра.
- Фреймворк вроде Express сидит на том же событии `request` и добавляет маршрутизацию.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  const body = JSON.stringify({ method: req.method, url: req.url });
  res.writeHead(200, {
    "Content-Type": "application/json; charset=utf-8",
    "Content-Length": Buffer.byteLength(body),
  });
  res.end(body);
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

<details>
<summary>Что такое Express.js and why is it important for Node.js?</summary>

### Что такое Express.js and why is it important for Node.js?

**Definition:**

Express — пакет npm, не модуль ядра. Он оборачивает `node:http` роутером, цепочкой middleware `(req, res, next)` и помощниками для кодов статуса и тела ответа. Так команды обычно описывают HTTP API на Node.

**How it works:**

- `express()` возвращает приложение. `app.listen(port)` создаёт нижележащий HTTP-сервер и биндит его.
- `app.get(path, handler)` и остальные методы сопоставляют `req.method` и шаблон пути. Параметры маршрута попадают в `req.params`.
- `app.use(fn)` регистрирует middleware, которая выполняется до обработчика маршрута. `next()` продолжает цепочку. `next(err)` переходит к error-handling middleware с четырьмя аргументами `(err, req, res, next)`.
- `express.json()` читает JSON-тело и записывает `req.body`. Без неё тело остаётся сырым потоком `IncomingMessage` из `node:http`.
- Express не заменяет цикл событий. Долгий цикл на CPU в обработчике по-прежнему блокирует остальные запросы этого процесса.
- То же приложение можно смонтировать под префиксом или отдать в `http.createServer(app)`, если нужен объект сервера ядра.

**When to use:**

- JSON API и страницы, где вся задача — маршруты и middleware.
- Оставайтесь на `node:http`, когда зависимость не нужна и хватает одного слушателя.

**Examples:**

```js
import express from "express";

const app = express();

app.use(express.json());

app.get("/health", (_req, res) => {
  res.status(200).json({ ok: true });
});

app.listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Express — Routing](https://expressjs.com/en/guide/routing.html)
</details>

<details>
<summary>Как you create a RESTful API with Node.js?</summary>

### Как you create a RESTful API with Node.js?

**Definition:**

REST API сопоставляет ресурсы URL, а действия — методам HTTP: `GET` читает, `POST` создаёт, `PUT` или `PATCH` обновляет, `DELETE` удаляет. На `node:http` вы ветвитесь по `req.method` и pathname, затем отвечаете кодом статуса и JSON-телом.

**How it works:**

- Разберите `req.url` через `new URL(req.url, "http://localhost")`. `url.pathname` — маршрут, `url.searchParams` — query. Хост в этом конструкторе только задаёт базу; заголовок `Host` клиента — отдельно.
- JSON-тело читают, собирая поток запроса, затем `JSON.parse`. Отклоняйте тело больше лимита до разбора.
- Отдавайте `201` после создания, `204` для пустого успеха, `404` если ресурса нет, `400` для плохого тела. Ставьте `Content-Type: application/json; charset=utf-8`.
- Обработчики не должны делить изменяемое состояние так, чтобы один запрос портил другой. Хранение — в драйвере базы, его нет в `node:http`.
- Идемпотентные методы (`GET`, `PUT`, `DELETE`) безопасно повторять. `POST` — метод, который при каждом вызове создаёт новый ресурс.
- Роутер фреймворка — та же идея с меньшим ручным сопоставлением. Коды и методы остаются HTTP, они не принадлежат Express.

**When to use:**

- API в форме ресурсов для браузера, мобильного приложения или другого сервиса.
- Потоки и server-push — другая форма. REST — это запрос, затем ответ.

**Examples:**

```js
import http from "node:http";

const notes = new Map();

const server = http.createServer((req, res) => {
  const url = new URL(req.url ?? "/", "http://localhost");
  if (req.method === "GET" && url.pathname === "/notes") {
    const body = JSON.stringify([...notes.values()]);
    res.writeHead(200, {
      "Content-Type": "application/json; charset=utf-8",
      "Content-Length": Buffer.byteLength(body),
    });
    res.end(body);
    return;
  }
  res.writeHead(404, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("not found");
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [MDN — HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
</details>

<details>
<summary>Что такое middleware in the context of Node.js?</summary>

### Что такое middleware in the context of Node.js?

**Definition:**

Middleware — функция на пути от сырого запроса к итоговому ответу. Она может читать или менять `req` и `res`, завершить ответ или передать управление дальше. У `node:http` нет типа middleware. Это цепочка, которую вы пишете сами, а Express закрепил сигнатуру `(req, res, next)`.

**How it works:**

- Каждая функция делает одно дело: лог, проверка заголовка, разбор JSON, заголовок безопасности. `next()` запускает следующую функцию. Если не вызвать `next()` и не вызвать `res.end()`, запрос зависнет.
- `next(err)` переходит к обработчику ошибок. В Express у него четыре параметра, `(err, req, res, next)`, чтобы роутер отличил его от обычной middleware.
- Порядок и есть поведение. Парсер тела должен идти до обработчика, который читает `req.body`. Проверка авторизации — до маршрута, который она защищает.
- Эквивалент в ядре: один слушатель `createServer` вызывает маленькие функции по очереди, или вы оборачиваете слушатель. Потоки никуда не деваются. Middleware, которая буферизует всё тело, должна иметь лимит.
- Middleware делит процесс. Всё, что она кладёт в глобал, протекает между запросами. Данные запроса держите в `res.locals` или в `WeakMap` с ключом `req`.
- Error middleware всё равно должна завершить ответ. Необработанный throw в async-обработчике Express требует явного `next(err)` или обёртки: отклонённый промис сам в `next` не попадает.

**When to use:**

- Сквозные шаги многих маршрутов: логи, авторизация, разбор, заголовки.
- Одному обработчику с двумя строками подготовки цепочка фреймворка не нужна.

**Examples:**

```js
import http from "node:http";

function withJsonHeader(req, res, next) {
  res.setHeader("Content-Type", "application/json; charset=utf-8");
  next();
}

const server = http.createServer((req, res) => {
  withJsonHeader(req, res, () => {
    const body = JSON.stringify({ ok: true });
    res.setHeader("Content-Length", Buffer.byteLength(body));
    res.writeHead(200);
    res.end(body);
  });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Express — Writing middleware](https://expressjs.com/en/guide/writing-middleware.html)
</details>

<details>
<summary>Как you ensure security in HTTP headers with Node.js?</summary>

### Как you ensure security in HTTP headers with Node.js?

**Definition:**

Ставьте заголовки ответа, которые говорят браузеру, как обращаться со страницами и ответами. API ядра — `res.setHeader(name, value)` на `ServerResponse`. Модуля заголовков безопасности в Node нет. Пакет вроде `helmet` только собирает те же вызовы `setHeader`.

**How it works:**

- `Content-Security-Policy` ограничивает, какие скрипты, стили и фреймы загрузит браузер. Это важно, когда вы отдаёте HTML.
- `X-Content-Type-Options: nosniff` запрещает браузеру угадывать тип ответа иначе, чем указано в `Content-Type`.
- `Strict-Transport-Security` просит браузер и дальше ходить на ваш хост по HTTPS. Отправляйте его только на настоящем HTTPS-ответе.
- `Referrer-Policy` и жёсткий `Content-Security-Policy` уменьшают, что браузер утечёт и выполнит. `Cache-Control` на авторизованном JSON не даёт общим кэшам его хранить.
- В значения заголовков нельзя класть сырой ввод пользователя. `writeHead` бросает `TypeError` на недопустимые символы — это как раз response splitting.
- `setHeader` сливается в заголовки, которые отправит `writeHead`. Более поздний заголовок в `writeHead` перекрывает то же имя.

**When to use:**

- Каждый ответ сервера, к которому ходят браузеры, включая API, которые могут оказаться в контексте страницы.
- Вызовы держите в одной middleware, чтобы новый маршрут их не забыл.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((_req, res) => {
  res.setHeader("X-Content-Type-Options", "nosniff");
  res.setHeader("Referrer-Policy", "no-referrer");
  res.setHeader("Content-Security-Policy", "default-src 'none'");
  res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
  res.end(JSON.stringify({ ok: true }));
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Security best practices](https://nodejs.org/en/learn/getting-started/security-best-practices)
- [MDN — X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Content-Type-Options)
</details>

### Error Handling & Debugging

<details>
<summary>Как you handle errors in Node.js?</summary>

### Как you handle errors in Node.js?

**Definition:**

Обрабатывайте каждый сбой у API, который его породил. Синхронный throw и `await` ловит `try`/`catch`. Колбэки проверяют аргумент `err`. Эмиттерам нужен слушатель `'error'`. Отказ или throw, который вышел из запроса, становится `unhandledRejection` или `uncaughtException` на `process`.

**How it works:**

- У `Error` есть `message` и `stack`. У многих ошибок Node также задан `code` (`ENOENT`, `EADDRINUSE`). Ветвитесь по `code`, когда нужно конкретное восстановление.
- `'error'` на `EventEmitter` без слушателей бросает исключение и завершает процесс. Потоки и сокеты эмитят `'error'`. Слушайте его или берите `pipeline`: он пробрасывает ошибку и уничтожает потоки.
- `process` эмитит `unhandledRejection` для отклонённого промиса, который никто не обработал, и `uncaughtException` для throw в цикле событий. Документация process считает `uncaughtException` последним путём «записать лог и остановиться»: процесс может быть в плохом состоянии, поэтому перестаньте принимать работу и выйдите.
- `uncaughtExceptionMonitor` даёт записать лог, не становясь официальным обработчиком. Установка `uncaughtException` меняет выход по умолчанию, поэтому после лога всё равно выходите сами.
- Async-обработчик HTTP должен поймать ошибку и завершить ответ. Throw после отправки заголовков уже не меняет строку статуса.
- Операционные ошибки (нет файла, база недоступна) восстанавливаются в рамках запроса. Ошибка программиста (чтение свойства у `undefined`) значит, что логика этого запроса сломана. Запишите лог и провалите запрос.

**When to use:**

- `try`/`catch` вокруг `await` на каждом пути запроса, который трогает I/O.
- Обработчик на уровне процесса — только чтобы записать лог и остановиться, а не продолжать обслуживать.

**Examples:**

```js
import { readFile } from "node:fs/promises";
import process from "node:process";

process.on("unhandledRejection", (reason) => {
  console.error(reason);
  process.exitCode = 1;
});

try {
  const text = await readFile(new URL("./note.txt", import.meta.url), "utf8");
  console.log(text);
} catch (err) {
  console.error(err.code ?? err.message);
}
```

**Sources:**

- [Node.js — Errors](https://nodejs.org/api/errors.html)
- [Node.js — Events](https://nodejs.org/api/events.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

<details>
<summary>Опишите some error first callback patterns in Node.js.</summary>

### Опишите some error first callback patterns in Node.js.

**Definition:**

Error-first колбэк получает `err` первым аргументом, а результат — после него. При успехе `err` равен `null` или `undefined`. При сбое `err` — это `Error`, а результата нет. Таков контракт callback API в `node:fs` и большей части старого ядра.

**How it works:**

- Сразу возвращайтесь после обработки `err`. Код ниже проверки должен считать, что результат есть.
- Наружу ошибку передавайте через `callback(err)`, а не throw внутри колбэка. Throw из колбэка на позднем тике — необработанное исключение.
- Если своя функция иногда уже знает ответ, отложите колбэк через `process.nextTick`, чтобы вызывающий успел довесить свою логику. Документация process показывает эту форму `definitelyAsync`.
- `util.promisify` переводит этот контракт в промис: ложный `err` резолвит результатом, истинный `err` отклоняет.
- Один колбэк должен вызваться один раз. Второй вызов отдаёт вызывающему два завершения вместо одного.
- Node-style колбэки не возвращают результат функции, которая начала I/O. Результат существует только внутри колбэка.

**When to use:**

- Вы реализуете или вызываете callback API ядра (`readFile`, callback-форма `randomBytes` из `node:crypto`).
- Promise API — тот же контракт уже разобранный: rejection и есть аргумент `err`.

**Examples:**

```js
import { readFile } from "node:fs";
import { nextTick } from "node:process";

function loadNote(callback) {
  readFile(new URL("./note.txt", import.meta.url), "utf8", (err, data) => {
    if (err) {
      callback(err);
      return;
    }
    callback(null, data);
  });
}

function alwaysAsync(value, callback) {
  nextTick(callback, null, value);
}

loadNote((err, text) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(text);
});

alwaysAsync("ok", (_err, value) => {
  console.log(value);
});
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>Что такое some common debugging techniques for Node.js applications?</summary>

### Что такое some common debugging techniques for Node.js applications?

**Definition:**

Запускайте процесс с `node --inspect`, чтобы Chrome DevTools или отладчик VS Code подключились к инспектору V8. Ставьте `debugger` там, где нужна пауза, и пишите структурные детали через `console`. Для зависания или утечки снимите CPU-профиль или снимок кучи в той же сессии инспектора.

**How it works:**

- `node --inspect index.js` печатает URL `ws://127.0.0.1:9229/...`. Инспектор биндится на localhost. `--inspect-brk` ставит паузу до любого кода приложения.
- Откройте `chrome://inspect` или подключитесь из редактора. Точки останова, стек и область видимости работают в том изоляте V8, где выполняется файл.
- `debugger;` — точка останова, когда инспектор подключён. Без инспектора Node проходит мимо.
- `console.error` пишет в `process.stderr`. Включайте `err.stack` и `err.code`. `util.debuglog` / `NODE_DEBUG` включают узкие debug-логи подсистем ядра (`NODE_DEBUG=http`).
- `--enable-source-maps` возвращает кадры стека к строкам TypeScript или транспилированного кода, если рядом лежит source map.
- Медленному процессу нужен CPU-профиль из инспектора. Растущему `process.memoryUsage().heapUsed` — снимок кучи. Оба берутся из той же сессии `--inspect`.

**When to use:**

- Баг, который воспроизводится локально: `--inspect-brk` и точка останова.
- Инцидент в проде: сначала логи и метрики. Инспектор на машине открывайте только через доверенный туннель: порт может выполнять код в процессе.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  debugger;
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(req.url ?? "/");
});

server.listen(8000);
```

**Sources:**

- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

<details>
<summary>Объясните process.nextTick().</summary>

### Объясните process.nextTick().

**Definition:**

`process.nextTick(callback, ...args)` ставит `callback` в очередь next tick. Она осушается после текущего стека JavaScript и до того, как продолжится ввод-вывод. Для обычной отсрочки документация process предпочитает `queueMicrotask()`: та же очередь микрозадач, что у реакций промисов, и она есть не только в Node.

**How it works:**

- `nextTick` — не фаза цикла событий. Колбэк, который ставит ещё один `nextTick`, может заморить таймеры и I/O: очередь осушается, пока не опустеет.
- В CommonJS колбэки `nextTick` идут раньше реакций промисов и `queueMicrotask`. После очереди next tick Node осушает очередь микрозадач.
- ES-модуль и так выполняется из очереди микрозадач, поэтому `queueMicrotask` и реакции промисов идут раньше `nextTick`.
- `nextTick(fn, a, b)` передаёт `a` и `b` в `fn`. У `queueMicrotask` нет лишних аргументов: их замыкают или биндят.
- `nextTick` нужен, когда API обязан быть асинхронным в каждой ветке, включая ту, где ответ уже есть. Пример `definitelyAsync` в документации откладывает эту ветку, чтобы вызывающий всегда видел поздний колбэк.
- `setImmediate` возвращает управление циклу (фаза check). Берите его, когда цель — дать I/O выполниться, а не успеть до I/O.

**When to use:**

- Граница библиотеки, которая должна оставаться асинхронной на любом пути.
- Коду приложения, которому нужно просто «скоро», достаточно `await` промиса или `queueMicrotask`.

**Examples:**

```js
import { nextTick } from "node:process";

function definitelyAsync(value, callback) {
  nextTick(callback, null, value);
}

console.log("start");
definitelyAsync("ok", (_err, value) => {
  console.log(value);
});
console.log("scheduled");
```

**Sources:**

- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
- [Node.js — Event loop, timers, and process.nextTick](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
</details>

<details>
<summary>Что такое the global object in Node.js?</summary>

### Что такое the global object in Node.js?

**Definition:**

Глобальный объект — место, где живут по-настоящему глобальные привязки. В Node его историческое имя — `global`. Документация globals помечает `global` как legacy и просит использовать `globalThis`: это тот же объект и имя, которое используют другие хосты JavaScript.

**How it works:**

- `var`, `const` и `function` на верхнем уровне модуля Node остаются локальными для модуля. Свойствами `global` они не становятся. Это верно и для CommonJS, и для ES-модулей.
- Имена только CommonJS выглядят глобальными и не являются ими: `__dirname`, `__filename`, `exports`, `module`, `require`. Они есть в обёртке модуля. В ES-модулях вместо них — `import.meta.url`.
- Настоящие глобалы включают `console`, `Buffer`, `process`, `URL`, `URLSearchParams`, `fetch`, `TextEncoder`, `queueMicrotask` и `setTimeout`. `WebSocket` — глобальный клиент, стабильный с Node 22.4, и это не сервер.
- Присваивание `globalThis.myApp = ...` публикует значение каждому модулю изолята. Общий слот — обычный способ протечь состоянием между запросами.
- `globalThis` в браузере — это `window`. Код, который трогает только стандартные глобалы, может работать в обоих местах. `process` и `Buffer` в браузере не появятся.
- У worker thread свой `globalThis`. Объект главного потока он не делит. Сообщения копируются или передаются.

**When to use:**

- Стандартные глобалы (`URL`, `fetch`, `queueMicrotask`) читайте напрямую.
- Состояние приложения держите в области модуля или в явном аргументе. `globalThis` — для привязки, которая правда должна быть видна всему изоляту.

**Examples:**

```js
console.log(globalThis === global);
console.log(typeof globalThis.fetch);
console.log(typeof globalThis.WebSocket);

const params = new URLSearchParams({ q: "node" });
console.log(params.get("q"));
```

**Sources:**

- [Node.js — Global objects](https://nodejs.org/api/globals.html)
</details>

### Testing in Node.js

<details>
<summary>Какие frameworks доступны для testing Node.js applications?</summary>

### Какие frameworks доступны для testing Node.js applications?

**Definition:**

В Node есть раннер тестов `node:test`. Импортируете `describe` и `it`, проверяете через `node:assert/strict` и запускаете `node --test`. Jest, Mocha и Vitest — пакеты npm со своими раннерами. `node:test` загружается только с префиксом `node:`.

**How it works:**

- `describe` группирует тесты. `it` (то же, что экспорт `test`) — один случай. Брошенное утверждение проваливает тест. Async-тест проваливается, если возвращённый промис отклонён.
- `node --test` находит файлы вроде `*.test.js`. Можно передать путь к файлу. Код выхода процесса ненулевой, если тест упал.
- `node:assert/strict` сравнивает через `Object.is` и бросает `AssertionError`. Он предпочтительнее свободного равенства из `node:assert`.
- Раннер собирает покрытие через `node --test --experimental-test-coverage`. Репортёры лежат в `node:test/reporters` и тоже требуют префикс `node:`.
- Jest и Vitest добавляют поддельный DOM, snapshot-файлы и свои моки. Это отдельные установки. Mocha — раннер, к которому подключают библиотеку утверждений.
- Тесты должны ходить на `127.0.0.1` и порт, который вы сами заняли. Тест, которому нужна сеть или база, должен это объявить и убраться в `after`.

**When to use:**

- Новые сервисы, которые могут остаться на ядре: `node:test`.
- React-интерфейс в том же репозитории часто уже на Vitest или Jest. Один раннер на пакет делает скрипты очевидными.

**Examples:**

```js
import { describe, it } from "node:test";
import assert from "node:assert/strict";

describe("sum", () => {
  it("adds two numbers", () => {
    assert.equal(1 + 2, 3);
  });
});
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — Assert](https://nodejs.org/api/assert.html)
</details>

<details>
<summary>Объясните the concept of mocking in Node.js.</summary>

### Объясните the concept of mocking in Node.js.

**Definition:**

Мок подменяет соседа, чтобы тест проверил ваш код без настоящего диска, сети или часов. `node:test` даёт `mock.fn` и `mock.method` в контексте теста. `mock.module` существует и требует `node --experimental-test-module-mocks`. Это не способ по умолчанию подменять встроенный модуль.

**How it works:**

- `t.mock.fn(impl)` возвращает функцию, которая пишет вызовы в `fn.mock.calls` и всё равно выполняет `impl`.
- `t.mock.method(object, name)` оборачивает один метод и восстанавливает его в конце теста. Шпион сохраняет исходный `this`.
- `mock.module(specifier, { exports })` подменяет модуль для поздних импортов. Документация говорит, что уже случившиеся импорты держат исходный модуль, а процесс надо запустить с `--experimental-test-module-mocks`.
- Мок должен стоять на границе, которой вы не владеете (HTTP, часы, источник случайности). Мок функции под тестом прячет баг.
- Восстановление — часть контракта. Контекст теста сам возвращает методы. Ручное присваивание `object.method` должно вернуть оригинал в `finally`.
- Когда можете, передавайте зависимость аргументом (`function load(readFile)`). Мок закрытого модуля ядра — более трудный путь.

**When to use:**

- Юнит-тест ветвлений, которые иначе пошли бы в сеть или на диск.
- HTTP-тест может поднять настоящий сервер `node:http` на свободном порту и обойтись без мока.

**Examples:**

```js
import { test } from "node:test";
import assert from "node:assert/strict";

test("records a call", (t) => {
  const sum = t.mock.fn((a, b) => a + b);
  assert.equal(sum(1, 2), 3);
  assert.equal(sum.mock.calls.length, 1);
});
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html#mocking)
</details>

<details>
<summary>Почему is benchmarking important in Node.js?</summary>

### Почему is benchmarking important in Node.js?

**Definition:**

Бенчмарк измеряет, сколько времени занимает кусок работы и сколько памяти он берёт, на фиксированном входе, до и после изменения. Цикл событий Node делает догадку «кажется медленно» ненадёжной: один заблокированный тик тормозит все соединения, а микрооптимизация может проиграть аллокациям.

**How it works:**

- `performance.now()` из `node:perf_hooks` возвращает миллисекунды монотонных часов. Вычтите два замера вокруг работы. `process.hrtime.bigint()` даёт наносекунды.
- Итераций должно быть столько, чтобы шум старта был малой долей. Сообщайте распределение, не один замер. Паузы сборки мусора попадают внутрь измерения.
- `process.memoryUsage()` сообщает `heapUsed`, `rss` и `external` в байтах. Функция быстрее, которая удерживает растущий буфер, всё равно проблема.
- `node --prof` пишет tick-лог V8, его разбирают через `--prof-process`. CPU-профиль инспектора отвечает на тот же вопрос интерактивно.
- Бенчмаркьте реальную форму данных. Короткая строка прячет цену большого `JSON.parse` или `readFile`, который буферизует весь файл.
- Сравнивайте на той же версии Node и той же машине. Результат с другой мажорной версии не переносится.

**When to use:**

- Перед тем как переписать горячий путь, добавить кэш или сменить синхронный вызов на асинхронный.
- Пропустите это для кода, который выполняется один раз на старте и очевидно дёшев.

**Examples:**

```js
import { performance } from "node:perf_hooks";

const start = performance.now();
let n = 0;
for (let i = 0; i < 1_000_000; i += 1) n += i;
const elapsedMs = performance.now() - start;
console.log({ n, elapsedMs });
```

**Sources:**

- [Node.js — Performance hooks](https://nodejs.org/api/perf_hooks.html)
- [Node.js — Process](https://nodejs.org/api/process.html#processmemoryusage)
</details>

<details>
<summary>Как you test an HTTP server in Node.js?</summary>

### Как you test an HTTP server in Node.js?

**Definition:**

Поднимите сервер через `listen(0)`, чтобы ОС выбрала свободный порт, запросите его через `http.get` на `127.0.0.1`, проверьте статус и тело, затем вызовите `server.close()`. `node:test` запускает это как async `it`. Отдельный HTTP-фреймворк не нужен.

**How it works:**

- `listen(0, "127.0.0.1")` биндит loopback. `server.address().port` — выбранный порт. Жёсткий `3000` сталкивается с dev-сервером и с параллельными тестами.
- Гоняйте сервер через HTTP. Вызов функции-обработчика напрямую пропускает разбор заголовков и поток ответа.
- Тело соберите из потока ответа (`'data'`, затем `'end'`). Статус сравнивайте с `res.statusCode`.
- `server.close()` перестаёт принимать соединения и вызывает колбэк, когда текущие закончились. Закрывайте и когда утверждение бросило (`try`/`finally`).
- Обработчик, который читает тело, тестируйте через `http.request` и записанный payload, включая слишком большое тело, если есть лимит.
- Процесс теста держите однозадачным. `node --test` может запускать файлы параллельно. Бинд только на `127.0.0.1` и порт `0` убирает взаимные помехи.

**When to use:**

- Каждый маршрут, который вы отдаёте: хотя бы один тест статуса и тела.
- Полный сценарий в браузере — отдельный end-to-end инструмент. Этот тест покрывает сервер.

**Examples:**

```js
import { it } from "node:test";
import assert from "node:assert/strict";
import http from "node:http";

it("GET / returns ok", async () => {
  const server = http.createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  });

  await new Promise((resolve) => {
    server.listen(0, "127.0.0.1", resolve);
  });

  try {
    const { port } = server.address();
    const { statusCode, body } = await new Promise((resolve, reject) => {
      http
        .get({ hostname: "127.0.0.1", port, path: "/" }, (res) => {
          const chunks = [];
          res.on("data", (chunk) => chunks.push(chunk));
          res.on("end", () => {
            resolve({
              statusCode: res.statusCode,
              body: Buffer.concat(chunks).toString("utf8"),
            });
          });
        })
        .on("error", reject);
    });
    assert.equal(statusCode, 200);
    assert.equal(body, "ok");
  } finally {
    await new Promise((resolve, reject) => {
      server.close((err) => (err ? reject(err) : resolve()));
    });
  }
});
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

### Node.js with Databases

<details>
<summary>Как you connect a MySQL dataBASE with Node.js?</summary>

### Как you connect a MySQL dataBASE with Node.js?

**Definition:**

В ядре Node нет клиента MySQL. Ставят драйвер, например `mysql2`, и открывают пул с хостом, пользователем, базой и паролем. Запросы идут через promise API пула, SQL остаётся в коде, значения передаются плейсхолдерами.

**How it works:**

- `mysql2/promise` экспортирует `createPool`. Пул переиспользует TCP-соединения. `createConnection` — одно соединение, оно плохо подходит серверу с параллельными запросами.
- `pool.execute(sql, values)` отправляет prepared statement. Плейсхолдеры (`?`) не дают пользовательскому вводу попасть в строку SQL. Промис резолвится в `[rows, fields]`.
- Настройки соединения берутся из `process.env` (или `process.loadEnvFile`). Пароль в репозитории — утёкший секрет.
- Пул живёт всё время процесса. На остановке вызывайте `pool.end()`, чтобы процесс мог завершиться.
- Медленный запрос занимает соединение из пула, а не поток JavaScript, пока вы ждёте промис и не пользуетесь синхронными помощниками драйвера.
- Транзакция: `getConnection()`, затем `beginTransaction()`, `commit()` и `release()` в `finally`. Невозвращённое соединение уменьшает пул.

**When to use:**

- Реляционные данные со связями, ограничениями и транзакциями.
- ORM или query builder могут сидеть на том же драйвере. Соединение всё равно этот пул.

**Examples:**

```js
import mysql from "mysql2/promise";

const pool = mysql.createPool({
  host: process.env.MYSQL_HOST,
  user: process.env.MYSQL_USER,
  password: process.env.MYSQL_PASSWORD,
  database: process.env.MYSQL_DATABASE,
});

const [rows] = await pool.execute(
  "SELECT id, title FROM notes WHERE id = ?",
  [1],
);
console.log(rows);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [mysql2 — Promise wrappers](https://sidorares.github.io/node-mysql2/docs/documentation/promise-wrapper)
</details>

<details>
<summary>Объясните how NoSQL dataBASEs like MongoDB can be used with Node.js.</summary>

### Объясните how NoSQL dataBASEs like MongoDB can be used with Node.js.

**Definition:**

К MongoDB ходят через официальный драйвер `mongodb` — это пакет npm. `MongoClient` открывает соединение, `db()` выбирает базу, `collection()` — коллекцию документов. Документы — объекты JavaScript. Node не встраивает MongoDB в себя.

**How it works:**

- `new MongoClient(uri)` и `await client.connect()` открывают пул внутри драйвера. URI лежит в `process.env`.
- `insertOne`, `find` и `updateOne` принимают объекты документов. `find` возвращает курсор. `toArray()` загружает совпадения в память, поэтому большой результат читают через `for await`.
- Жёсткой таблицы нет. Приложению всё равно нужно соглашение о полях или валидатор коллекции. Драйвер сохранит тот объект, который вы передали.
- Одного `MongoClient` на процесс достаточно. Клиент на каждый запрос исчерпает соединения.
- `client.close()` на остановке отпускает сокеты. Сервер без этого может зависнуть на выходе: пул держит цикл событий.
- Связи — это агрегация (`$lookup`), не SQL. Документы моделируют вокруг тех чтений, которые вы реально делаете.

**When to use:**

- Данные в форме документов с разным набором полей или команда, которая уже ведёт MongoDB.
- Жёсткая реляционная целостность по-прежнему хочет SQL-базу и её ограничения.

**Examples:**

```js
import { MongoClient } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI);
await client.connect();

const notes = client.db("app").collection("notes");
await notes.insertOne({ title: "hello" });

const found = await notes.find({ title: "hello" }).toArray();
console.log(found);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [MongoDB Node.js driver — Connect](https://www.mongodb.com/docs/drivers/node/current/connect/)
</details>

<details>
<summary>Какова роль ORM в Node.js?</summary>

### Какова роль ORM в Node.js?

**Definition:**

ORM (object-relational mapper) — библиотека npm, которая сопоставляет объекты JavaScript строкам SQL и генерирует запросы. В ядре Node ORM нет. Prisma, Drizzle, Sequelize и TypeORM — пакеты, которые ставят поверх драйвера базы.

**How it works:**

- Таблицы описывают моделями или схемой. Библиотека превращает вызовы методов в SQL и строки результата обратно в объекты.
- Миграции — версионированные изменения этой схемы. Они лежат в репозитории и запускаются шагом до старта нового процесса.
- Query builder (Knex или SQL-сторона Drizzle) ближе к SQL. Более тяжёлый ORM ещё держит идентичность и связи в памяти.
- Сгенерированный SQL может спрятать N+1: один запрос за родителем и по запросу на каждого ребёнка. Смотрите SQL, когда страница замедляется.
- Транзакции по-прежнему нужны. ORM их показывает, чтобы несколько записей закоммитились или откатились вместе.
- Под соединением всё ещё пул драйвера (`mysql2`, `pg` и похожие). Своего отдельного сокета ORM не открывает.

**When to use:**

- Код приложения, который в основном создаёт, читает и обновляет строки и хочет одно место для схемы.
- Отчёты и возможности конкретной базы яснее писать SQL, через драйвер или тонкий query builder.

**Examples:**

```js
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();
const notes = await prisma.note.findMany({ take: 20 });
console.log(notes);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html)
- [Prisma — CRUD](https://www.prisma.io/docs/orm/prisma-client/queries/crud)
</details>

### Node.js Performance

<details>
<summary>Как you monitor the performance of a Node.js app?</summary>

### Как you monitor the performance of a Node.js app?

**Definition:**

Следите за задержкой цикла событий, временем CPU и памятью процесса, плюс кодами статуса и длительностью HTTP-запросов. `node:perf_hooks` и `process.memoryUsage()` / `process.cpuUsage()` — измерения ядра. В проде эти числа уходят в логи или в систему метрик.

**How it works:**

- `monitorEventLoopDelay()` строит гистограмму, насколько цикл опаздывает. `enable()` её запускает. Значения в наносекундах. Растущее среднее значит, что JavaScript занят или цикл голодает.
- `process.memoryUsage()` возвращает `rss`, `heapTotal`, `heapUsed`, `external` и `arrayBuffers` в байтах. Снимайте по интервалу и смотрите тренд.
- `process.cpuUsage(previous)` возвращает пользовательские и системные микросекунды с момента `previous`. Сопоставьте с стенным временем, чтобы увидеть, правда ли процесс на CPU.
- Время запроса: `performance.now()` в начале слушателя `request` и снова в `res.on("finish")`. Логируйте метод, путь, статус и миллисекунды.
- `node --inspect` и CPU-профиль объясняют всплеск, который можно воспроизвести. `--prof` делает то же из командной строки.
- Метрики — на процесс. У primary в `cluster` своя строка на каждого worker. Агрегируйте снаружи процесса.

**When to use:**

- В проде всегда: задержка, доля ошибок, задержка цикла, память.
- Разовому скрипту хватает `performance.now()` вокруг работы.

**Examples:**

```js
import { monitorEventLoopDelay, performance } from "node:perf_hooks";
import process from "node:process";

const delay = monitorEventLoopDelay({ resolution: 20 });
delay.enable();

const startedAt = performance.now();
const cpuStart = process.cpuUsage();

setInterval(() => {
  console.log({
    elapsedMs: performance.now() - startedAt,
    cpu: process.cpuUsage(cpuStart),
    heapUsed: process.memoryUsage().heapUsed,
    loopDelayMs: delay.mean / 1e6,
  });
}, 10_000).unref();
```

**Sources:**

- [Node.js — Performance hooks](https://nodejs.org/api/perf_hooks.html#monitoreventloopdelayoptions)
- [Node.js — Process](https://nodejs.org/api/process.html#processmemoryusage)
</details>

<details>
<summary>Что такое clustering in Node.js and how does it work?</summary>

### Что такое clustering in Node.js and how does it work?

**Definition:**

Модуль `node:cluster` запускает несколько процессов Node, которые делят один порт сервера. Primary вызывает `cluster.fork()`. Каждый worker — полный процесс со своим изолятом V8 и своим циклом событий. Операционная система раздаёт принятые соединения по worker-ам.

**How it works:**

- `cluster.isPrimary` истинен в процессе, который вы запустили. `cluster.isWorker` истинен в форкнутом worker. `cluster.worker.id` есть только у worker.
- `cluster.fork()` порождает worker. Вызывайте по разу на ядро, которое хотите занять (`availableParallelism()` из `node:os`). Форкать может только primary.
- Worker-ы снова выполняют тот же файл. `listen` HTTP ставьте в ветке worker. Порт общий. Не нужно слушать один раз в primary и проксировать руками.
- Память и состояние JavaScript не общие. `Map` в одном worker не виден остальным. Сессии лежат в хранилище вне процесса.
- Если worker вышел, primary может снова `fork()` на событии `'exit'`. Сам primary лучше держать маленьким.
- `cluster` — параллелизм процессов. Он не переносит тяжёлую функцию на другой поток внутри одного изолята. Для этого есть `worker_threads`.

**When to use:**

- HTTP-сервер на одной машине, которому нужны несколько ядер, когда вы хотите модуль ядра, а не cluster-режим менеджера процессов.
- Несколько машин — это балансировщик перед несколькими хостами, это уже вне `node:cluster`.

**Examples:**

```js
import cluster from "node:cluster";
import http from "node:http";
import { availableParallelism } from "node:os";

if (cluster.isPrimary) {
  const n = availableParallelism();
  for (let i = 0; i < n; i += 1) cluster.fork();
} else {
  http
    .createServer((_req, res) => {
      res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
      res.end("ok");
    })
    .listen(8000);
}
```

**Sources:**

- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
- [Node.js — OS](https://nodejs.org/api/os.html#osavailableparallelism)
</details>

<details>
<summary>Как you prevent memory leaks in a Node.js application?</summary>

### Как you prevent memory leaks in a Node.js application?

**Definition:**

Утечка — память, которую процесс держит после того, как работа, которой она была нужна, закончилась. Сбрасывайте ссылки, когда заканчиваются запрос, таймер или слушатель, и смотрите `process.memoryUsage().heapUsed`. Ровный рост, который не падает после сборки мусора, и есть сигнал.

**How it works:**

- `Map` или массив без верхней границы в роли кэша растёт вместе с трафиком. Ограничьте размер или задайте записям срок жизни и удаляйте их.
- `emitter.on` без парного `removeListener` удерживает слушателя и всё, что он замыкает. Для разового события берите `once`.
- `setInterval`, который вы не остановили через `clearInterval`, держит свой колбэк. `unref()` у таймера позволяет процессу выйти. Он не освобождает замыкание, которое всё ещё держит большой буфер.
- Поток запроса, который никто не читает, или сокет, который никто не уничтожил, остаётся в таблице хендлов. Обычные точки уборки — `pipeline` и `res.on("close")`.
- Замыкание, которое захватило большой `Buffer`, держит этот буфер, пока само достижимо. Скопируйте маленький кусок, который вам нужен.
- Подтверждайте снимком кучи под `node --inspect`. Сравните два снимка после одной и той же нагрузки. `heapUsed` из `memoryUsage` показывает тренд до того, как вы откроете снимок.

**When to use:**

- Любой долго живущий сервер. Скрипты, которые завершаются, отдают всё при выходе процесса.
- Чиньте тот объект, который снимок называет удерживающим. Дополнительная RAM только отодвигает следующий рестарт.

**Examples:**

```js
import { EventEmitter } from "node:events";
import process from "node:process";

const bus = new EventEmitter();

function onPing(id) {
  console.log(id);
}

bus.on("ping", onPing);
bus.emit("ping", 1);
bus.removeListener("ping", onPing);

console.log(process.memoryUsage().heapUsed);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processmemoryusage)
- [Node.js — Events](https://nodejs.org/api/events.html)
- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
</details>

<details>
<summary>Объясните the use of the --inspect flag in Node.js.</summary>

### Объясните the use of the --inspect flag in Node.js.

**Definition:**

`node --inspect file.js` включает инспектор V8 и слушает `127.0.0.1:9229`. DevTools подключается к напечатанному `ws://` URL и может ставить точки останова, шагать и снимать профили CPU и кучи. `--inspect-brk` ставит паузу до выполнения скрипта.

**How it works:**

- Хост по умолчанию — loopback, порт по умолчанию — `9229`. `--inspect=127.0.0.1:9230` меняет порт. Публичный адрес открывает отладчик, который может выполнить код в процессе.
- Оператор `debugger` останавливает выполнение только пока клиент подключён.
- Та же сессия пишет CPU-профиль и снимок кучи. Так разбирают высокую задержку цикла или растущий `heapUsed`.
- `--inspect` не меняет ваш HTTP-сервер. Он добавляет второй слушатель протокола инспектора.
- Менеджер процессов должен пробросить флаг в бинарник Node. Worker кластера — отдельный процесс, и ему нужен свой порт инспектора.
- `--enable-source-maps` совмещает кадр паузы со строкой TypeScript, если source map есть.

**When to use:**

- Локальная отладка и воспроизведённый прод-баг на машине, которой вы доверяете.
- На публичном сервере флаг выключен. Если он там нужен, заходите SSH-туннелем на `127.0.0.1`.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((_req, res) => {
  debugger;
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

server.listen(8000);
```

```console
node --inspect-brk server.js
```

**Sources:**

- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
</details>

### Concurrency in Node.js

<details>
<summary>Как Node.js handle concurrency?</summary>

### Как Node.js handle concurrency?

**Definition:**

JavaScript в одном процессе выполняется на одном стеке вызовов. Параллельность дают цикл событий, который чередует колбэки ввода-вывода, пул потоков libuv для части блокирующих системных вызовов и дополнительные процессы или worker threads, когда вы их запрашиваете.

**How it works:**

- Сетевые сокеты остаются на потоке цикла. Ожидающий запрос не занимает поток JavaScript. Слушатель `'request'` выполняется, когда ядро сообщает, что соединение можно читать или писать.
- Файловый I/O, `dns.lookup` и часть crypto идут в пул libuv (`UV_THREADPOOL_SIZE`, по умолчанию 4). Это не один поток на запрос.
- `worker_threads` запускает другой изолят V8 в том же процессе для работы на CPU. С главным потоком он говорит через `postMessage`.
- `node:cluster` и `child_process.fork` запускают ещё процессы. У каждого своя память и свой цикл. Порт они делят только через `cluster` или внешний балансировщик.
- Плотный цикл `for` на главном потоке останавливает цикл, пока не вернётся. Остальные соединения ждут.
- Параллельность async-функций — это «много операций в полёте», а не параллельный JavaScript. `Promise.all` перекрывает ожидания. Колбэки всё равно идут по одному.

**When to use:**

- Серверы с тяжёлым I/O остаются на цикле событий.
- Работа на CPU уходит в `worker_threads` или другой процесс. Больше ядер для HTTP — это `cluster` или несколько процессов за прокси.

**Examples:**

```js
import { readFile } from "node:fs/promises";

const dir = new URL("./", import.meta.url);
const [a, b] = await Promise.all([
  readFile(new URL("a.txt", dir), "utf8"),
  readFile(new URL("b.txt", dir), "utf8"),
]);
console.log(a.length, b.length);
```

**Sources:**

- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
</details>

<details>
<summary>Что такое the difference between process and child_process modules?</summary>

### Что такое the difference between process and child_process modules?

**Definition:**

`node:process` — это текущий процесс Node: окружение, pid, память, сигналы и `nextTick`. `node:child_process` запускает другие программы. `spawn` — базовая функция. `exec` и `execFile` буферизуют вывод. `fork` запускает другой модуль Node с каналом IPC.

**How it works:**

- `process.env`, `process.pid`, `process.exitCode` и `process.memoryUsage()` описывают этот процесс. `process` — `EventEmitter` (`'exit'`, `'uncaughtException'`, сигналы).
- `spawn(command, args)` создаёт подпроцесс и даёт потоки stdin, stdout и stderr. Если полный пайп никто не читает, дочерний процесс может заблокироваться на этом пайпе.
- `exec` запускает оболочку и буферизует stdout и stderr. В документации есть `maxBuffer`, потому что весь вывод сидит в памяти. `execFile` запускает файл без оболочки.
- `fork(modulePath)` — особый `spawn` бинарника Node. Родитель и ребёнок могут `process.send` и слушать `'message'`.
- Ребёнок не делит вашу кучу JavaScript. Данные переходят байтами или сериализованными сообщениями.
- Родитель должен обработать `'error'` и `'exit'` и убить или `unref` детей, которые больше не нужны, иначе родитель может остаться живым на их пайпах.

**When to use:**

- `process` — для окружения, остановки и сигналов этого процесса.
- `spawn` — для CLI, чей вывод вы стримите. `fork` — когда другая сторона это код Node, который должен вам писать. `exec` — для короткой команды с небольшим выводом.

**Examples:**

```js
import { spawn } from "node:child_process";
import process from "node:process";

console.log(process.pid);

const child = spawn(process.execPath, ["-e", "console.log('child')"]);
child.stdout.on("data", (chunk) => {
  console.log(chunk.toString());
});
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html)
- [Node.js — Child process](https://nodejs.org/api/child_process.html)
</details>

<details>
<summary>Как worker threads work in Node.js?</summary>

### Как worker threads work in Node.js?

**Definition:**

`node:worker_threads` выполняет JavaScript на другом потоке со своим изолятом V8. Главный поток создаёт `Worker` из file URL. Worker читает `parentPort`, обе стороны вызывают `postMessage`. Так работу на CPU снимают с цикла событий, не запуская второй процесс.

**How it works:**

- `isMainThread` истинен в исходном потоке процесса. Один и тот же файл может ветвиться: главный поток делает `new Worker(new URL(import.meta.url))`, а worker выполняет ветку `else`.
- Сообщения по умолчанию копируются (structured clone). `transferList` переносит `ArrayBuffer`, чтобы обе стороны его не держали. `SharedArrayBuffer` — общая память, ей нужна своя синхронизация.
- `parentPort` на главном потоке равен `null`. В worker это канал. `'message'` доставляет то, что отправила другая сторона.
- У worker свой цикл событий и свой `globalThis`. Он может пользоваться `node:fs` и `node:http`. Эти вызовы стоят столько же, сколько стоят. Выигрыш — параллельный JavaScript.
- На `Worker` слушайте `'error'` и `'exit'`. Необработанная ошибка в worker эмитит `'error'` на объекте `Worker` у родителя.
- Worker тяжелее колбэка. Заведите маленький пул и переиспользуйте его. Один worker на запрос съедает время, которое вы хотели сэкономить.

**When to use:**

- Сжатие, картинки, разбор огромного тела или любой плотный цикл, который затормозит HTTP.
- Параллельность I/O остаётся на главном цикле. Сам по себе worker не ускоряет ожидание базы.

**Examples:**

```js
import { Worker, isMainThread, parentPort } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.once("message", (message) => {
    console.log(message);
  });
  worker.postMessage("Hello, world!");
} else {
  parentPort.once("message", (message) => {
    parentPort.postMessage(message);
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
</details>

### Node.js and Microservices

<details>
<summary>Как Node.js used in microservices architecture?</summary>

### Как Node.js used in microservices architecture?

**Definition:**

Микросервис на Node — это один процесс (или небольшой cluster процессов), который владеет одной задачей и отдаёт её по сети, обычно через HTTP или очередь. Node подходит, потому что сервис большую часть времени ждёт другие сервисы, а цикл событий держит много таких ожиданий в полёте.

**How it works:**

- У каждого сервиса свой `package.json`, свой деплой и своё хранилище. Другой сервис доходит до него через HTTP (`node:http` или `fetch`) или публикует сообщение. Файлы другого сервиса он не `require`-ит.
- Для края небольшого сервиса хватает `http.createServer`. Фреймворк только добавляет маршруты.
- Состояние живёт вне процесса. `Map` на сервере исчезает, когда вы запускаете вторую копию, и worker-ы `cluster` её тоже не делят.
- У медленной или упавшей зависимости должны быть таймаут и путь отказа. `AbortSignal.timeout` на `fetch` обрывает зависший вызов. Один застрявший запрос не должен остановить цикл.
- Тяжёлая работа на CPU внутри сервиса уходит в `worker_threads` или в отдельный worker-сервис. HTTP-процесс остаётся тонким.
- Наблюдаемость — на сервис: логируйте полученный id запроса и передавайте его дальше. `process.memoryUsage` и задержка цикла — локальные сигналы здоровья.

**When to use:**

- Граница, которую можно назвать, задеплоить и масштабировать отдельно (auth, биллинг, ресайз картинок).
- Одно деплоимое приложение на Node проще на старте, когда команда и данные ещё одни.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  if (req.method === "GET" && req.url === "/health") {
    res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ ok: true }));
    return;
  }
  res.writeHead(404).end();
});

server.listen(process.env.PORT ?? 8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
</details>

<details>
<summary>Объясните inter-process communication in a Node.js microservice architecture.</summary>

### Объясните inter-process communication in a Node.js microservice architecture.

**Definition:**

Сервисы на разных машинах говорят по сети: HTTP (`fetch` или `node:http`) или брокер сообщений. Процессы на одной машине могут ещё использовать `child_process.fork`: у обеих сторон есть `process.send` и событие `'message'`. Это разные каналы. Деплой микросервисов обычно выбирает сеть, потому что другой процесс может быть не на этом хосте.

**How it works:**

- `fork` открывает канал IPC. `subprocess.send(message)` у родителя и `process.send(message)` у ребёнка доставляют structured clone. Событие — `'message'`. Тем же механизмом `node:cluster` связывает primary и worker.
- HTTP — это запрос и ответ. Отправьте JSON, задайте таймаут через `AbortSignal` и считайте статус не-2xx ошибкой, которую обрабатывает вызывающий.
- Очередь отвязывает отправителя от получателя. Отправитель может завершиться. Получатель работает в своём темпе. Сообщение хранит брокер, это не модуль ядра.
- Сообщения должны быть версионированы и небольшими. Общая строка в базе — не шина сообщений. Два сервиса, которые пишут в одни таблицы, связывают свои деплои.
- Хендлы IPC держат родителя живым. На остановке закройте канал или `kill` ребёнка.
- Не кладите секреты в строку лога, которая записывает сообщение. `process.env` одного сервиса другому не виден.

**When to use:**

- `fork` для процесса-помощника, который живёт и умирает вместе с этим процессом Node.
- HTTP или очередь — между деплоимыми сервисами.

**Examples:**

```js
import { fork } from "node:child_process";

const child = fork(new URL("./worker.js", import.meta.url));

child.on("message", (message) => {
  console.log(message);
});

child.send({ type: "ping" });
```

**Sources:**

- [Node.js — Child process](https://nodejs.org/api/child_process.html#child_processforkmodulepath-args-options)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

### Security in Node.js

<details>
<summary>Что такое some common security best practices for Node.js applications?</summary>

### Что такое some common security best practices for Node.js applications?

**Definition:**

Держите рантайм и зависимости свежими, не выполняйте недоверенные строки как код, ограничивайте тела запросов и храните секреты в окружении. Модель прав Node (`node --permission`) может запретить доступ к файлам и сети, о котором процесс не просил. Публичный сервер стоит за TLS, обычно его обрывает обратный прокси.

**How it works:**

- `eval` и `new Function(userInput)` выполняют любую переданную строку. Данные запроса остаются данными. JSON разбирайте через `JSON.parse`.
- `NODE_TLS_REJECT_UNAUTHORIZED` ставьте только если вы согласны выключить проверку сертификатов. Документация TLS предупреждает, что `'0'` отключает проверку.
- Читайте тело с лимитом байт и останавливайтесь. Неограниченный `for await` по `req` — это DoS по памяти.
- `node --permission --allow-fs-read=./data --allow-net` заставляет лишние обращения к файлам и сети падать. Это предел рантайма, а не проверка вашей логики.
- Запускайте процесс не от root. Инспектор (`--inspect`) остаётся на `127.0.0.1`.
- Фиксируйте версии локфайлом и смотрите новые пакеты. Зависимость работает с теми же правами, что и ваш процесс.
- Клиенту отдавайте статус и короткое сообщение. Стек остаётся в логе.

**When to use:**

- Каждый сетевой процесс, включая внутренние. Форма практик одна и та же при любом размере.
- Флаги прав добавляйте, когда уже ясна поверхность файлов и сети приложения.

**Examples:**

```js
import http from "node:http";

const limit = 1_000_000;

const server = http.createServer(async (req, res) => {
  const chunks = [];
  let size = 0;
  for await (const chunk of req) {
    size += chunk.length;
    if (size > limit) {
      res.writeHead(413).end();
      return;
    }
    chunks.push(chunk);
  }
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(String(size));
});

server.listen(8000);
```

**Sources:**

- [Node.js — Security best practices](https://nodejs.org/en/learn/getting-started/security-best-practices)
- [Node.js — Permissions](https://nodejs.org/api/permissions.html)
- [Node.js — TLS](https://nodejs.org/api/tls.html)
</details>

<details>
<summary>Как бы вы protect your Node.js application from XSS attacks?</summary>

### Как бы вы protect your Node.js application from XSS attacks?

**Definition:**

Межсайтовый скриптинг случается, когда браузер выполняет строку, пришедшую от пользователя. Для сервера на Node это значит: не вклеивайте сырой ввод в HTML, а JSON API отдавайте как JSON, чтобы браузер не принял его за страницу. Заголовок `Content-Security-Policy` ограничивает, какие скрипты могут выполниться.

**How it works:**

- Кодируйте текст до того, как он попадёт в HTML. JSON API с `Content-Type: application/json; charset=utf-8` — не HTML-документ. Добавьте `X-Content-Type-Options: nosniff`, чтобы браузер сохранил этот тип.
- Шаблон `res.end("<p>" + req.url + "</p>")` кладёт URL в HTML. Кодируйте `&`, `<`, `>`, кавычки или берите шаблонизатор, который экранирует по умолчанию.
- `Content-Security-Policy` с жёстким `script-src` останавливает внедрённый inline-скрипт, даже если ошибка проскочила. Ставьте его через `res.setHeader`.
- Cookie браузерной сессии хотят `HttpOnly`, чтобы скрипт страницы их не прочитал, и `Secure` на HTTPS. Это ограничивает ущерб XSS. Сам баг это не убирает.
- Богатый текст, который вы намеренно храните как HTML, нуждается в пакете-санитайзере. Санитайзера HTML в ядре Node нет.
- Отражение `req.headers` в страницу — та же форма, что и отражение тела. И то и другое контролирует пользователь.

**When to use:**

- Любой ответ, который браузер может разобрать как HTML или загрузить в страницу.
- Чистому JSON API всё равно нужен тип JSON и `nosniff`, чтобы браузер не угадал в нём HTML.

**Examples:**

```js
import http from "node:http";

function escapeHtml(value) {
  return value
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;");
}

const server = http.createServer((req, res) => {
  const safe = escapeHtml(req.url ?? "/");
  const body = `<!doctype html><p>${safe}</p>`;
  res.writeHead(200, {
    "Content-Type": "text/html; charset=utf-8",
    "Content-Security-Policy": "default-src 'self'",
    "X-Content-Type-Options": "nosniff",
  });
  res.end(body);
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Security best practices](https://nodejs.org/en/learn/getting-started/security-best-practices)
- [MDN — Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy)
</details>

<details>
<summary>Что такое environment variables and how could you use them in Node.js?</summary>

### Что такое environment variables and how could you use them in Node.js?

**Definition:**

Переменные окружения — строки, которые операционная система передаёт в процесс. Node показывает их как `process.env`. Оттуда читают порты, хосты и секреты, чтобы одна и та же сборка работала в разных местах. `process.loadEnvFile()` и `node --env-file` на старте загружают файл `.env` в `process.env`.

**How it works:**

- Каждое значение в `process.env` — строка. `process.env.PORT ?? "8000"` всё равно нужно прогнать через `Number()`, если нужно число. Отсутствующий ключ — это `undefined`.
- Присваивание `process.env.FOO = "bar"` задаёт значение этому процессу и детям, которые наследуют окружение. Оболочку для следующего запуска оно не правит.
- `loadEnvFile()` из `node:process` читает файл `.env` (по умолчанию `.env` в текущем рабочем каталоге) и не перезаписывает переменные, которые уже заданы. `NODE_OPTIONS` внутри этого файла ни на что не влияет.
- `node --env-file=.env server.js` загружает файл до запуска скрипта. Это встроенный путь. Пакет `dotenv` — инструмент npm, который так же заполняет `process.env`.
- Положите `.env` в `.gitignore`. В репозиторий кладите пример с пустыми значениями. Настоящий файл остаётся на машине или в хранилище секретов хоста.
- Лог `process.env` печатает секреты. Логируйте имена нужных ключей, не сам объект.

**When to use:**

- Конфигурация, которая меняется между ноутбуком, CI и продом.
- Туда же флаги возможностей и размеры пулов. Ветки кода — нет.

**Examples:**

```js
import { loadEnvFile } from "node:process";
import http from "node:http";

loadEnvFile();

const port = Number(process.env.PORT ?? 8000);

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  })
  .listen(port);
```

**Sources:**

- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
- [Node.js — process.loadEnvFile](https://nodejs.org/api/process.html#processloadenvfilepath)
</details>

### Node.js and WebSockets

<details>
<summary>Что такое WebSockets and how do they work with Node.js?</summary>

### Что такое WebSockets and how do they work with Node.js?

**Definition:**

WebSocket — долгий двусторонний канал поверх одного TCP-соединения. Он начинается как HTTP-запрос с `Upgrade: websocket` и после ответа `101` обе стороны могут слать кадры в любой момент. Глобальный класс `WebSocket` в Node — это клиент той же формы, что API браузера, стабильный с Node 22.4. WebSocket-сервера в ядре нет.

**How it works:**

- Клиент открывает `new WebSocket(url)`. `'open'` значит, что рукопожатие закончилось. `send` пишет кадр. `'message'` доставляет один кадр. `close` закрывает сокет.
- HTTP-сервер видит рукопожатие как событие `'upgrade'` на `node:http`: запрос, сырой сокет и первые буферизованные байты. Это хук. Кадры WebSocket он за вас не разбирает и не собирает.
- Серверная библиотека (обычный пакет npm — `ws`) принимает upgrade, говорит на RFC 6455 и даёт сокет с `message` и `send`.
- Соединение остаётся открытым, поэтому процесс держит сокет на клиента. Между кадрами цикл событий делает другую работу. Плотный цикл в обработчике `'message'` блокирует все соединения этого процесса.
- Кадры — это сообщения, не тело HTTP. Свой формат вы задаёте сами, часто это небольшой JSON-текст.
- Если клиенту нужны только обновления от сервера, Server-Sent Events (`text/event-stream` на обычном ответе) — односторонний вариант, который остаётся на `node:http`.

**When to use:**

- Чат, живые курсоры и другие потоки, где обе стороны шлют данные без нового HTTP-запроса.
- Обычные чтения и записи остаются запросом и ответом.

**Examples:**

```js
const socket = new WebSocket("wss://echo.websocket.org");

socket.addEventListener("open", () => {
  socket.send("hello");
});

socket.addEventListener("message", (event) => {
  console.log(event.data);
  socket.close();
});
```

**Sources:**

- [Node.js — Global WebSocket](https://nodejs.org/api/globals.html#class-websocket)
- [Node.js — HTTP upgrade](https://nodejs.org/api/http.html#event-upgrade)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>Как you set up a WebSocket server in Node.js?</summary>

### Как you set up a WebSocket server in Node.js?

**Definition:**

Ставят библиотеку WebSocket-сервера. Встроенного WebSocket-сервера в Node нет. Пакет `ws` садится на сервер `node:http`, завершает рукопожатие upgrade и эмитит соединение. Глобальный класс `WebSocket` только открывает клиентское соединение.

**How it works:**

- Создайте HTTP-сервер через `http.createServer`, затем `new WebSocketServer({ server })` из `ws`. Библиотека слушает `'upgrade'` и отвечает `101` с заголовками протокола.
- `'connection'` даёт сокет на каждого клиента. `socket.on("message", cb)` читает кадр. `socket.send(data)` пишет кадр. `socket.close()` завершает этого клиента.
- HTTP-сервер по-прежнему отвечает на обычные запросы. Клиенты WebSocket идут в путь upgrade. Все остальные попадают в слушатель `request`.
- Рассылка — цикл по `wss.clients`. Каждый `send` — на свой сокет. Обратное давление — это объём буфера сокета. Медленного клиента пропускают или отключают.
- Горизонтальное масштабирование не делит сокеты между процессами. Второму процессу Node нужен брокер или другой fan-out, если каждый клиент должен видеть каждое сообщение.
- В проде сервер стоит за TLS (`wss:`). Браузер откажется от небезопасного WebSocket со страницы HTTPS.

**When to use:**

- Вы сервер, и браузерам (или другим клиентам) нужен двусторонний канал.
- Вызов чужого адреса — это глобальный клиент `WebSocket`, а не этот сервер.

**Examples:**

```js
import http from "node:http";
import { WebSocketServer } from "ws";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

const wss = new WebSocketServer({ server });

wss.on("connection", (socket) => {
  socket.on("message", (data) => {
    socket.send(data);
  });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Global WebSocket](https://nodejs.org/api/globals.html#class-websocket)
- [ws — README](https://github.com/websockets/ws#simple-server)
</details>

### Node.js Deployment

<details>
<summary>Как you deploy a Node.js application in production?</summary>

### Как you deploy a Node.js application in production?

**Definition:**

Отдавайте тот же исходник, который тестировали, ставьте прод-зависимости из локфайла, конфигурацию кладите в окружение и запускайте `node` под менеджером процессов за обратным прокси. Процесс слушает `process.env.PORT`. Прокси обрывает TLS и пересылает на этот порт.

**How it works:**

- `npm ci --omit=dev` ставит локфайл и пропускает `devDependencies`. Версия Node на хосте должна совпадать с той, на которой тестировали (поле `engines` это записывает).
- Приложение читает `PORT`, адреса баз и секреты из `process.env` или `--env-file`. Секрет не запекают в образ и не кладут в git.
- Менеджер процессов перезапускает процесс, когда тот вышел, и может держать по процессу на ядро. То же умеет `node:cluster` внутри одного запуска. Выбирают что-то одно, а не складывают оба вслепую.
- Прокси (nginx, Caddy, облачный балансировщик) — публичный адрес. Node биндится на `127.0.0.1` или приватный интерфейс. `--inspect` выключен.
- Логи идут в stdout и stderr. Их собирает платформа. Файл лога внутри процесса требует ротации, о которой иначе забудут.
- Мягкая остановка: на `SIGTERM` перестаньте принимать (`server.close()`), доделайте текущие запросы и выйдите. Оркестраторы шлют этот сигнал до того, как убьют процесс.

**When to use:**

- Любой сервис, который вызывают другие. `node server.js` на ноутбуке — цикл разработки.
- Контейнеры и serverless — другая упаковка того же процесса. Форма «слушай порт и читай окружение» остаётся.

**Examples:**

```js
import http from "node:http";
import process from "node:process";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

const port = Number(process.env.PORT ?? 8000);
server.listen(port);

process.on("SIGTERM", () => {
  server.close(() => process.exit(0));
});
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
</details>

<details>
<summary>Что такое PM2 and how is it used in Node.js?</summary>

### Что такое PM2 and how is it used in Node.js?

**Definition:**

PM2 — менеджер процессов из npm, не модуль ядра. Он запускает вашу программу `node`, перезапускает её после падения и может форкнуть по процессу на CPU в режиме cluster. Его ставят рядом с приложением и указывают скрипт, который вы и так запускаете через `node`.

**How it works:**

- `pm2 start server.js` демонизирует этот скрипт. `pm2 logs` читает stdout и stderr. `pm2 restart` и `pm2 stop` бьют в процесс по имени или id.
- `pm2 start server.js -i max` включает cluster: несколько worker-ов на одном порту, та же идея, что у `node:cluster`. Worker-ы — отдельные процессы.
- Падение завершает worker. PM2 поднимает новый. Код всё равно должен перестать течь по памяти. Цикл рестартов прячет баг, пока машина занята рестартами.
- `pm2 startup` вместе с `pm2 save` записывает список процессов, чтобы после перезагрузки приложения вернулись. Это настройка хоста, не то, что лежит внутри `server.js`.
- Окружение приходит из оболочки или из ecosystem-файла, который читает PM2. `process.env` внутри приложения от этого не меняет смысл.
- PM2 — один вариант. systemd, оркестратор контейнеров или `node:cluster` под простым супервизором закрывают те же задачи. Супервизор один.

**When to use:**

- Виртуальная машина или голый хост, где нужны рестарты и cluster без своего цикла fork.
- Kubernetes или другой оркестратор уже перезапускает контейнеры. PM2 внутри контейнера это дублирует.

**Examples:**

```js
import http from "node:http";

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  })
  .listen(process.env.PORT ?? 8000);
```

```console
pm2 start server.js -i max
```

**Sources:**

- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
- [PM2 — Quick start](https://pm2.keymetrics.io/docs/usage/quick-start/)
</details>

<details>
<summary>Объясните how you would use Docker with a Node.js application.</summary>

### Объясните how you would use Docker with a Node.js application.

**Definition:**

Dockerfile упаковывает бинарник Node, установку по локфайлу и ваш исходник в образ. Официальный гайд Node начинает с `FROM node`, копирует `package.json` и локфайл, запускает `npm ci`, копирует остальное и ставит `CMD` на `node` и файл сервера. Образ слушает порт, который вы `EXPOSE`. Конфигурацию передают в момент запуска.

**How it works:**

- Манифест копируют раньше остального исходника, чтобы изменение только кода могло переиспользовать слой установки.
- `npm ci --omit=dev` в образе совпадает с продом. Инструменты разработки остаются вне runtime-образа. Multi-stage сборка может собрать TypeScript на одной стадии и скопировать `dist` в меньшую стадию.
- Не копируйте `node_modules` с ноутбука. `.dockerignore` перечисляет их, плюс `.env` и `.git`. Образ сам запускает `npm ci`.
- `CMD ["node", "server.js"]` — это процесс. PID 1 должен быть Node (или крошечный init). Приложение обрабатывает `SIGTERM` и закрывает сервер.
- `docker run -e PORT=8000 -p 8000:8000` публикует порт и задаёт `process.env.PORT`. Секреты — окружение на запуске, не строки `ENV`, запечённые в образ.
- Тот же образ едет из CI в прод. На сервере его не пересобирают с другим исходником.

**When to use:**

- Нужны одна и та же файловая система и одна версия Node на каждой машине.
- Платформа, которая уже запускает бинарник Node (некоторые PaaS-сборки), не требует, чтобы вы сами вели Dockerfile.

**Examples:**

```dockerfile
FROM node:22-bookworm-slim
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev
COPY . .
EXPOSE 8000
USER node
CMD ["node", "server.js"]
```

**Sources:**

- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
</details>

### Node.js and Version Control

<details>
<summary>Как you manage versioning of a Node.js API?</summary>

### Как you manage versioning of a Node.js API?

**Definition:**

Версию API кладут в путь URL (`/v1/notes`) и держат этот обработчик стабильным. Ломающее изменение выходит как `/v2`, пока `/v1` всё ещё отвечает. Клиент передаёт версию самим путём. Сервер ветвится по `url.pathname` в `node:http` или по префиксу роутера.

**How it works:**

- Ломающее изменение — то, из-за которого существующий клиент становится неправ: убранное поле, новое обязательное поле, другой смысл. Необязательное JSON-поле может остаться на `/v1`.
- Обе версии могут жить в одном процессе. Общее, что не менялось, делят. Там, где контракт разный, у каждой версии свой обработчик.
- О закрытии сообщают заголовком вроде `Deprecation` и датой, затем старый путь убирают в позднем деплое. `/v1` на месте не переписывают.
- Версии в query и в своих заголовках тоже бывают. Путь — тот вариант, который виден в логах и в команде `curl`.
- Сам сервер версионируют semver в `package.json`. Это версия релиза кода. Версия в URL — контракт. Часы у них разные.
- Контрактные тесты бьют в `/v1` и `/v2` отдельными случаями, чтобы правка одной молча не изменила другую.

**When to use:**

- Публичный или межкомандный HTTP API, который переживёт первого клиента.
- Внутренний обработчик с одним вызывающим может меняться вместе с ним и обойтись без публичного префикса версии.

**Examples:**

```js
import http from "node:http";

function send(res, status, body) {
  const payload = JSON.stringify(body);
  res.writeHead(status, {
    "Content-Type": "application/json; charset=utf-8",
    "Content-Length": Buffer.byteLength(payload),
  });
  res.end(payload);
}

const server = http.createServer((req, res) => {
  const url = new URL(req.url ?? "/", "http://localhost");
  if (req.method === "GET" && url.pathname === "/v1/notes") {
    send(res, 200, [{ id: 1, title: "hello" }]);
    return;
  }
  if (req.method === "GET" && url.pathname === "/v2/notes") {
    send(res, 200, [{ id: 1, title: "hello", body: "" }]);
    return;
  }
  send(res, 404, { error: "not found" });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — package.json](https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions)
</details>

<details>
<summary>Что такое semantic versioning (semver) and its importance in Node.js development?</summary>

### Что такое semantic versioning (semver) and its importance in Node.js development?

**Definition:**

Семантическое версионирование нумерует релиз как `MAJOR.MINOR.PATCH`. PATCH растёт на исправление бага, MINOR — на совместимую возможность, MAJOR — когда вы ломаете вызывающих. npm и `package.json` живут на этом контракте: диапазон вроде `^1.4.0` может поставить поздние релизы 1.x и откажется от 2.0.0.

**How it works:**

- `"version"` в `package.json` — собственный semver пакета. Зависимости перечисляют диапазоны. Локфайл фиксирует точное дерево, которое поставили.
- `^1.4.0` разрешает `1.x` не ниже `1.4.0`. `~1.4.0` разрешает `1.4.x`. Точная версия разрешает только её.
- У `0.y.z` ведущий ноль значит, что API ещё нестабилен. Минорный шаг под `0.` может сломать вызывающих. npm трактует `^0.2.3` жёстче, чем `^1.2.3`.
- Ломающее изменение, опубликованное как patch, подхватят все потребители `^` на следующей установке. Так маленький релиз роняет сборки.
- `npm ci` ставит локфайл. `npm update` двигается внутри диапазонов и переписывает лок. Коммитьте локфайл, чтобы CI и прод разрешали одни и те же версии.
- Контракт HTTP `/v1` и версия npm связаны только вашей дисциплиной. MAJOR-шаг npm — правильный спутник ломающего программного API.

**When to use:**

- Каждый пакет, который вы публикуете, и каждый диапазон зависимости, который вы пишете.
- Приложениям, которые не публикуют, всё равно полезны локфайл и чтение диапазонов перед обновлением.

**Examples:**

```json
{
  "name": "notes",
  "version": "1.4.2",
  "dependencies": {
    "express": "^4.21.0"
  }
}
```

**Sources:**

- [Node.js — package.json](https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions)
- [npm — About semantic versioning](https://docs.npmjs.com/about-semantic-versioning)
- [Semantic Versioning](https://semver.org/)
</details>

### Node.js Advanced Topics

<details>
<summary>Что такое the difference between exports and module.exports in Node.js?</summary>

### Что такое the difference between exports and module.exports in Node.js?

**Definition:**

В модуле CommonJS `exports` в начале — ярлык на `module.exports`. Добавление свойства (`exports.area = fn`) меняет объект, который получат вызывающие. Присваивание `exports = something` перепривязывает локальное имя и оставляет `module.exports` как был. Заменить экспорт значит присвоить `module.exports`.

**How it works:**

- Node оборачивает файл в функцию и передаёт `module` и `exports`, где `exports` указывает на `module.exports` (в начале это пустой объект).
- `exports.hello = true` — та же запись, что `module.exports.hello = true`. `require` возвращает `module.exports`.
- `exports = { hello: false }` направляет локальную привязку на новый объект. Модуль по-прежнему экспортирует исходный объект. Документация показывает это комментарием, что такое присваивание не экспортируется.
- `module.exports = class Square {}` заменяет экспортируемое значение. Вызывающие получают класс, а не набор свойств.
- Обычный приём, когда вы заменяете объект и всё ещё хотите ярлык: `module.exports = exports = function Constructor() {}`.
- В ES-модулях нет `exports` и `module.exports`. Там `export` и `import`. `module.exports` из CommonJS виден как default import, когда ES-модуль грузит этот файл.

**When to use:**

- `exports.fn = ...`, когда модуль экспортирует несколько функций.
- `module.exports = ...`, когда модуль экспортирует одну функцию, класс или объект.

**Examples:**

```js
module.exports.hello = true;

exports.area = (r) => Math.PI * r ** 2;

module.exports = class Square {
  constructor(width) {
    this.width = width;
  }
};
```

**Sources:**

- [Node.js — exports shortcut](https://nodejs.org/api/modules.html#exports-shortcut)
- [Node.js — ECMAScript modules](https://nodejs.org/api/esm.html)
</details>

<details>
<summary>Как you create a simple TCP server in Node.js?</summary>

### Как you create a simple TCP server in Node.js?

**Definition:**

`net.createServer` из `node:net` слушает TCP-соединения. Колбэк получает `net.Socket` — дуплексный поток. Клиенту пишут байты через `write` и завершают сокет через `end`, когда обмен закончен. `listen(port)` занимает порт.

**How it works:**

- Сокет эмитит `'data'` на входящие байты, `'end'`, когда клиент полузакрыл соединение, и `'error'` при сбое. Слушайте `'error'`, чтобы сброс не уронил процесс.
- `socket.write(chunk)` возвращает `false`, когда буфер ядра полон. До следующей записи ждите `'drain'`. `socket.pipe(socket)` эхом возвращает данные и сам учитывает это обратное давление.
- У TCP нет заголовков HTTP и нет `req.url`. Рамки сообщений задаёте вы (перевод строки, префикс длины). `node:http` как раз разбирает HTTP поверх такого сокета.
- `server.listen(port, host)` биндит адрес. `'error'` на сервере ловит `EADDRINUSE`.
- Каждое соединение — один сокет на цикле событий. Долгий цикл на CPU в обработчике `'data'` тормозит остальные сокеты.
- Закрытие — `server.close()`. Уже открытые сокеты живут, пока не закончатся, если вы их не уничтожите.

**When to use:**

- Свой бинарный или построчный протокол, или небольшой внутренний сервис не на HTTP.
- Браузерам и JSON API нужен `node:http`.

**Examples:**

```js
import net from "node:net";

const server = net.createServer((socket) => {
  socket.write("hello\n");
  socket.pipe(socket);
});

server.on("error", (err) => {
  console.error(err);
});

server.listen(8124, "127.0.0.1");
```

**Sources:**

- [Node.js — Net](https://nodejs.org/api/net.html)
</details>

<details>
<summary>Что такое REPL in Node.js?</summary>

### Что такое REPL in Node.js?

**Definition:**

REPL значит read, eval, print, loop. `node` без скрипта открывает такой цикл в терминале: вы вводите выражение, Node его вычисляет, печатает результат и ждёт следующую строку. `node:repl` может запустить тот же цикл из кода, в том числе поверх сокета.

**How it works:**

- Сессия по умолчанию использует `process.stdin` и `process.stdout`. Приглашение — `>`. Результат каждого выражения лежит в `_`.
- Tab дополняет имена. `.help` показывает команды. `.exit` или Ctrl+D завершает сессию. `.editor` собирает многострочный блок.
- `repl.start({ prompt, input, output })` строит сессию. Документация показывает сессию на `net.Socket`, чтобы удалённый клиент получил приглашение. Такой сокет может выполнить любой JavaScript, который может выполнить процесс.
- `useGlobal: true` заставляет сессию делить настоящий объект `global`. REPL по умолчанию держит свой контекст.
- REPL — инструмент разработки. `repl.start` на сетевом порту без аутентификации отдаёт процесс.
- Набранные команды не заменяют файл теста. Они исчезают вместе с сессией.

**When to use:**

- Пощупать функцию или значение, пока вы разбираете API.
- Точка останова в `--inspect` лучше, когда код уже живёт в файле.

**Examples:**

```js
import repl from "node:repl";

repl.start({ prompt: "> " });
```

```console
node
```

**Sources:**

- [Node.js — REPL](https://nodejs.org/api/repl.html)
</details>

<details>
<summary>Объясните the role of a reverse proxy with Node.js applications.</summary>

### Объясните the role of a reverse proxy with Node.js applications.

**Definition:**

Обратный прокси — процесс, к которому подключаются клиенты. Он обрывает TLS, может отдавать статику и пересылает динамические запросы одному или нескольким процессам Node. Node слушает приватный порт. Прокси — не модуль ядра. На этом месте обычно nginx, Caddy или облачный балансировщик.

**How it works:**

- Браузер говорит с прокси по HTTPS. Прокси говорит по HTTP с `127.0.0.1:PORT`, где слушает `node`. `server.listen` не обязан владеть файлами сертификатов.
- Несколько процессов Node (или хостов) стоят за одним публичным именем. Прокси раскладывает соединения. Так задействуют больше одной машины. `node:cluster` покрывает только ядра одной машины.
- Прокси может сжимать gzip, ограничивать частоту запросов и отсекать медленных клиентов до того, как они займут процесс Node.
- `X-Forwarded-For` и `X-Forwarded-Proto` говорят Node, что увидел прокси. Этим заголовкам верьте только когда ближайший сосед — ваш прокси. Клиент, который достучался до Node напрямую, может их подделать.
- Upgrade для WebSocket тоже нужно пробрасывать (`Upgrade` и `Connection`). Прокси, который их срезает, ломает сервер `ws` за собой.
- Проверки здоровья бьют в дешёвый путь вроде `/health`, чтобы прокси перестал слать трафик мёртвому процессу.

**When to use:**

- Каждый прод-деплой HTTP, который смотрит в публичный интернет.
- Локальный `node server.js` во время разработки может обойтись без него.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  const forwarded = req.headers["x-forwarded-proto"];
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(typeof forwarded === "string" ? forwarded : "direct");
});

server.listen(8000, "127.0.0.1");
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
</details>

<details>
<summary>Как Node.js streams enhance performance?</summary>

### Как Node.js streams enhance performance?

**Definition:**

Потоки передают данные чанками, поэтому память остаётся около high-water mark, а не полного размера. `pipeline` из `node:stream/promises` соединяет читаемый поток с записываемым, учитывает обратное давление и уничтожает цепочку, если одна сторона ошиблась. Так большой файл или загрузка не становятся одним гигантским `Buffer`.

**How it works:**

- `readFile` выделяет память на весь файл, и только потом вы его обрабатываете. `createReadStream` эмитит чанки по мере ответа диска. Первый байт можно обработать до того, как прочитан последний.
- `writable.write` возвращает `false`, когда приёмник отстаёт. Читаемый поток должен встать на паузу до `'drain'`. `pipe` и `pipeline` делают эту паузу за вас. Если игнорировать `false`, чанки копятся в памяти.
- HTTP-ответ уже записываемый поток. Прокидывание файлового потока в `res` шлёт тело по мере чтения. Клиент начинает получать данные до конца файла.
- `pipeline` пробрасывает первую ошибку и убирает за собой. Ручной `pipe` оставляет приёмник открытым, когда источник эмитит `'error'`. Документация потоков прямо называет это утечкой.
- Время до первого байта падает, потому что вы не ждёте полного чтения. Суммарный CPU может быть похожим. Выигрыш — память и перекрытие чтения с записью.
- Маленькие файлы по-прежнему удобнее через `readFile`. У потока свои объекты на чанк. Берите его, когда данные большие или без заранее известного конца.

**When to use:**

- Скачивание и загрузка файлов, сжатие (`zlib.createGzip` посередине `pipeline`) и проксирование тела ответа.
- Несколько килобайт JSON проще одним буфером.

**Examples:**

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";
import { pipeline } from "node:stream/promises";

await pipeline(
  createReadStream("notes.txt"),
  createGzip(),
  createWriteStream("notes.txt.gz"),
);
```

**Sources:**

- [Node.js — Stream](https://nodejs.org/api/stream.html)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

### Frameworks and Libraries in Node.js

<details>
<summary>Опишите some popular frameworks and libraries in the Node.js ecosystem.</summary>

### Опишите some popular frameworks and libraries in the Node.js ecosystem.

**Definition:**

Фреймворки и библиотеки здесь — пакеты npm поверх ядра. Express, Fastify и Koa оборачивают `node:http` маршрутизацией. NestJS добавляет структуру модулей и TypeScript. Ни один из них не входит в бинарник `node`. Ядро остаётся `node:http`, `node:fs`, `node:stream` и раннером тестов.

**How it works:**

- Express — обычный HTTP-слой: роутер, middleware `(req, res, next)` и большой набор соседних пакетов.
- Fastify — HTTP-фреймворк вокруг JSON Schema для маршрутов и с упором на малые накладные расходы. Это отдельный пакет со своим `listen` и моделью плагинов.
- Koa — меньшее ядро от тех же авторов, что и Express. Middleware — `async (ctx, next)`. Маршруты и разбор тела — отдельные пакеты.
- NestJS раскладывает код на модули и провайдеры, с декораторами, и может сидеть на Express или Fastify как на настоящем HTTP-сервере.
- Библиотеки данных (`mysql2`, `mongodb`, Prisma) и realtime (`ws`, Socket.IO) — зависимость того же рода: их ставят и импортируют. Это не модули `node:`.
- Выберите стек и закоммитьте его локфайл. Два HTTP-фреймворка в одном процессе — это понятия двух серверов на один порт.

**When to use:**

- Express или Fastify для HTTP API. NestJS, когда команде нужна фиксированная раскладка. Ядро `node:http`, когда маршрутов немного.
- Библиотеку добавляйте, когда ядро эту работу не делает. Для `fs`, `path` или `http` она не нужна.

**Examples:**

```js
import http from "node:http";

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ ok: true }));
  })
  .listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Express](https://expressjs.com/)
- [Fastify](https://fastify.dev/)
</details>

<details>
<summary>Как Koa different from Express.js?</summary>

### Как Koa different from Express.js?

**Definition:**

Оба — HTTP-фреймворки из npm от тех же исходных авторов, поверх HTTP-сервера Node. Express даёт `req`, `res` и `next`, и его экосистема выросла вокруг callback-middleware. Koa даёт один `ctx` и ждёт async-middleware, которая делает `await next()`. В самом пакете Koa роутера нет.

**How it works:**

- Обработчик Express — `(req, res) => { res.status(200).json(body) }`. Middleware вызывает `next()` или завершает ответ. Маршруты — `app.get` на приложении Express.
- Обработчик Koa — `async (ctx, next) => { await next(); ctx.body = body }`. `ctx.request` и `ctx.response` оборачивают запрос и ответ Node. Нижележащая middleware выполняется на `await next()`, затем функция продолжается.
- Разбор тела, маршруты и статика — отдельные пакеты Koa (обычный роутер — `@koa/router`). Приложения Express тоже берут многое из пакетов middleware, но сам роутер встроен.
- Ошибки в Koa отклоняют промис middleware, и верхний `try/catch` вокруг `await next()` их видит. Обработчики ошибок Express — функции с четырьмя аргументами `(err, req, res, next)`.
- Ни один фреймворк не снимает работу с цикла событий. Блокирующий цикл блокирует процесс в обоих.
- Любой из них можно посадить на `http.createServer`. Разница — в объектах, которых касается обработчик.

**When to use:**

- Express, когда нужен более широкий набор готовой middleware и `app.get` в том же пакете.
- Koa, когда нужно маленькое ядро и async-middleware как форма по умолчанию.

**Examples:**

```js
import Koa from "koa";

const app = new Koa();

app.use(async (ctx) => {
  ctx.body = { ok: true };
});

app.listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Koa](https://koajs.com/)
- [Express — Writing middleware](https://expressjs.com/en/guide/writing-middleware.html)
</details>

<details>
<summary>Что такое NestJS and when would you choose it for your Node.js project?</summary>

### Что такое NestJS and when would you choose it for your Node.js project?

**Definition:**

NestJS — фреймворк из npm, который раскладывает сервер Node на модули, контроллеры и внедряемые провайдеры, обычно на TypeScript. HTTP обрабатывает адаптер. Адаптер по умолчанию — Express. Его можно сменить на Fastify. Nest не входит в ядро Node.

**How it works:**

- Модуль объявляет контроллеры и провайдеры. Метод контроллера — маршрут (`@Get()`, `@Post()`). Провайдер — класс, который инжектор создаёт и передаёт в конструкторы.
- Декораторы называют HTTP-контракт: статус, параметры, тело. Фреймворк вызывает ваш метод. Вы возвращаете значение или бросаете HTTP-исключение. Nest превращает это в ответ.
- Pipe проверяют и преобразуют ввод. Guard решают, выполнится ли маршрут. Interceptor оборачивает вызов. Это имена Nest для шагов вроде middleware с фиксированным порядком.
- Нижележащий сервер — всё ещё Node. Блокирующий цикл в провайдере блокирует цикл событий этого процесса.
- CLI (`nest new`, `nest generate`) создаёт папки. В этом и смысл раскладки: новый человек находит маршрут, находя контроллер.
- За это платят декораторами, шагом сборки и понятиями, которые не нужны десяти строкам `http.createServer`.

**When to use:**

- Долгоживущий API с несколькими доменами и команда, которой нужна одна структура.
- Небольшой сервис с парой маршрутов яснее на Express, Fastify или `node:http`.

**Examples:**

```js
import { Controller, Get } from "@nestjs/common";

@Controller("health")
export class HealthController {
  @Get()
  getHealth() {
    return { ok: true };
  }
}
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [NestJS — Controllers](https://docs.nestjs.com/controllers)
</details>

<details>
<summary>Что такое the benefits of using TypeScript with Node.js?</summary>

### Что такое the benefits of using TypeScript with Node.js?

**Definition:**

TypeScript проверяет типы до запуска и затем стирает их, поэтому Node всё равно выполняет JavaScript. Несовпавшие аргументы, пропущенные поля и плохую работу с `null` ловят редактор и `tsc`. Свежие выпуски Node умеют запускать файлы `.ts`, стирая типы. Стирание не проверяет типы. Это по-прежнему делает `tsc`.

**How it works:**

- Файл `.ts` описывает значения аннотациями и выведенными типами. `tsc` сообщает об ошибках и может выдать `.js` и source maps. Node запускает выданный JavaScript, либо Node с поддержкой стирания типов запускает сам файл `.ts`.
- `@types/node` описывает модули ядра (`process`, `fs`, `http`), чтобы редактор знал `req.method` и `res.end`.
- Типы исчезают в рантайме. Проверка, которая нужна пока сервер работает (`typeof`, библиотека схем), всё равно пишется как JavaScript, который остаётся после стирания.
- Общие типы между API на Node и клиентом на TypeScript держат JSON-контракт в одном месте. Формат на проводе всё равно JSON.
- `tsconfig.json` задаёт режим модулей (`nodenext` совпадает с разрешением самого Node) и строгость проверок. Слабый конфиг прячет те ошибки, ради которых типы и нужны.
- Собирайте это в CI. Зелёный `tsc --noEmit` значит, что типы согласованы. `node --test` он не заменяет.

**When to use:**

- Сервисы и библиотеки, которые правит больше одного человека, и любая публичная функция, чьи аргументы легко перепутать.
- Короткий скрипт может остаться на обычном JavaScript. Поведение рантайма в обоих случаях одно.

**Examples:**

```ts
import http from "node:http";

const server = http.createServer((req, res) => {
  const method: string = req.method ?? "GET";
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(method);
});

server.listen(8000);
```

**Sources:**

- [Node.js — TypeScript](https://nodejs.org/api/typescript.html)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [TypeScript — Node.js](https://www.typescriptlang.org/docs/handbook/typescript-tooling-in-5-minutes.html)
</details>

### Integrations & Third-Party Node.js Modules

<details>
<summary>Как бы вы integrate a Node.js app with a third-party API?</summary>

### Как бы вы integrate a Node.js app with a third-party API?

**Definition:**

Чужой сервис вызывают глобальным `fetch`, нужные заголовки кладут в запрос, а ожидание ограничивают через `AbortSignal.timeout`. `response.ok` проверяют до разбора тела. Базовый URL и токен держат в `process.env`. Отдельного клиента под чужой API Node не включает.

**How it works:**

- `fetch(url, { method, headers, body, signal })` возвращает промис `Response`. `response.json()` разбирает JSON-тело. `response.text()` возвращает сырой текст.
- Статус не-2xx всё равно резолвит промис. В этом случае `response.ok` ложен. Обработайте его до того, как доверять телу.
- `AbortSignal.timeout(ms)` обрывает зависший вызов. Поймайте abort и решите, повторять ли. Повтор `POST`, который уже успел, может создать дубликат. Сначала повторяйте идемпотентные чтения.
- Их JSON отображайте в свой тип на границе. Остальное приложение не должно зависеть от имён полей, которые вы не контролируете.
- В лог идут таймауты, коды статуса и id запроса. Токен — нет. Читайте его из `process.env` и отправляйте заголовком.
- Исходящие вызовы делят цикл событий. Много параллельных `fetch` — нормально. Разбор огромного JSON за один раз — та часть, которая может затормозить цикл.

**When to use:**

- Любой HTTP API, которым вы не владеете: платежи, почта, карты, сервис другой команды.
- Очередь перед вызовом, когда их простой не должен тут же ронять запрос вашего пользователя.

**Examples:**

```js
const response = await fetch(process.env.NOTES_API_URL, {
  headers: {
    Accept: "application/json",
    Authorization: `Bearer ${process.env.NOTES_API_TOKEN}`,
  },
  signal: AbortSignal.timeout(5000),
});

if (!response.ok) {
  throw new Error(`notes api status ${response.status}`);
}

const notes = await response.json();
console.log(notes);
```

**Sources:**

- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
</details>

<details>
<summary>Что такое Socket.IO and how does it work with Node.js?</summary>

### Что такое Socket.IO and how does it work with Node.js?

**Definition:**

Socket.IO — библиотека npm для двусторонних сообщений между браузером и процессом Node. Это не модуль ядра и не сам протокол WebSocket. Клиент Socket.IO говорит с сервером Socket.IO. Сервер может сесть на уже существующий сервер `node:http`. Глобальный класс `WebSocket` на этом протоколе не говорит.

**How it works:**

- `new Server(httpServer)` из `socket.io` цепляется к вашему HTTP-серверу. Клиент подключается к тому же origin. Первый запрос — HTTP. Дальше библиотека берёт WebSocket, если путь позволяет, и long polling, если нет.
- `'connection'` даёт сокет. `socket.emit(name, payload)` шлёт этому клиенту. `socket.on(name, cb)` принимает. `io.emit` шлёт каждому подключённому сокету в пространстве имён по умолчанию.
- Комнаты (`socket.join`) группируют сокеты, чтобы слать подмножеству. Комнаты живут в памяти этого сервера, пока вы не добавите Redis-адаптер для нескольких процессов Node.
- Обрамление принадлежит Socket.IO, поэтому браузерный `new WebSocket(url)` на этот сервер сессию Socket.IO не завершит. Берите клиент `socket.io`.
- Каждое соединение — состояние процесса. Масштабирование наружу требует адаптер, чтобы emit на одном процессе дошёл до сокетов на другом.
- Обычный запрос-ответ HTTP он не заменяет. REST оставляйте на `node:http` или Express, а Socket.IO — для событий.

**When to use:**

- Браузерный интерфейс, которому нужны пуши от сервера и мелкие события, и вы хотите, чтобы переподключение делала библиотека.
- Обычная пара клиента и сервера WebSocket — это `ws` плюс глобальный `WebSocket`, без протокола Socket.IO.

**Examples:**

```js
import http from "node:http";
import { Server } from "socket.io";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

const io = new Server(server);

io.on("connection", (socket) => {
  socket.emit("hello", { ok: true });
  socket.on("ping", () => {
    socket.emit("pong");
  });
});

server.listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Socket.IO — Server API](https://socket.io/docs/v4/server-api/)
</details>

<details>
<summary>Объясните how GraphQL can be used with Node.js.</summary>

### Объясните how GraphQL can be used with Node.js.

**Definition:**

GraphQL — язык запросов к API. В Node его реализует пакет `graphql` (и обычно HTTP-обёртка вокруг него). Клиент шлёт один запрос на одну точку. Сервер проверяет его по схеме и вызывает резолверы, которые вы написали. Это не модуль ядра, и транспорт всё равно `node:http` или `fetch`.

**How it works:**

- Схема объявляет типы и корневой `Query` (и `Mutation`, когда клиенты пишут). `buildSchema` или JS-построители схемы создают эту схему.
- `graphql({ schema, source, rootValue })` выполняет строку запроса и возвращает `{ data, errors }`. Резолверы на `rootValue` или на полях схемы дают значения.
- По HTTP обычная форма — `POST` с JSON-телом `{ "query": "{ hello }" }` на один путь. Это тело вы разбираете сами на `node:http` или ставите пакет, который делает это.
- Клиент называет нужные поля. Обработчик REST вместо этого возвращает фиксированное представление одного ресурса. GraphQL всё равно может загрузить лишнее на сервере, если резолвер берёт слишком много.
- N+1 появляется, когда резолвер списка ходит в базу по разу на каждого ребёнка. Пакетная загрузка в духе DataLoader — паттерн приложения поверх резолверов. В `node:http` её нет.
- Авторизация по-прежнему ваша. Проверяйте вызывающего до резолверов, как на REST-маршруте.

**When to use:**

- Несколько клиентов, которым нужны разные срезы одного графа, и команда, готовая владеть схемой.
- Небольшой API ресурсов проще оставить REST на `node:http`.

**Examples:**

```js
import { buildSchema, graphql } from "graphql";

const schema = buildSchema("type Query { hello: String }");

const result = await graphql({
  schema,
  source: "{ hello }",
  rootValue: { hello: () => "world" },
});

console.log(result.data);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [GraphQL.js](https://graphql.org/graphql-js/)
</details>

### Node.js with Frontend Technologies

<details>
<summary>Как Node.js interact with frontend frameworks like Angular or React?</summary>

### Как Node.js interact with frontend frameworks like Angular or React?

**Definition:**

Браузер выполняет React или Angular. Node выполняет API и может ещё отдать HTML и JavaScript, которые эти приложения грузят. Обычный контракт — JSON по HTTP: страница вызывает `fetch`, Node отвечает через `node:http`. Язык общий. Процесс и DOM — нет.

**How it works:**

- Отдельный dev-сервер (Vite, Angular CLI) отдаёт UI в разработке и проксирует `/api` на Node. В проде собранные файлы отдаёт либо Node, либо статический хост, а API остаётся на Node.
- `createReadStream` собранного `index.html`, направленный в `res`, — способ ядра отдать файл. `Content-Type` ставят по расширению. Часто эту работу делает статический хост или обратный прокси.
- Страницам с другого origin нужны заголовки CORS (`Access-Control-Allow-Origin`) на ответе API. Тому же origin они не нужны.
- Серверный рендеринг — это Node, который вызывает серверный рендерер фреймворка и шлёт HTML. Клиентский бандл затем гидратирует этот HTML в браузере.
- Аутентификация — cookie или заголовок, который хранит браузер и проверяет API. Состояние React Node не видит.
- Типы можно делить общим пакетом. Значения в рантайме всё равно пересекают сеть как JSON.

**When to use:**

- Node как API, когда UI — отдельное приложение. Node как раздача файлов, когда нужен один деплой.
- Dev-сервера фреймворка достаточно, пока вы правите только компоненты.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  if (req.url === "/api/notes") {
    const body = JSON.stringify([{ id: 1, title: "hello" }]);
    res.writeHead(200, {
      "Content-Type": "application/json; charset=utf-8",
      "Content-Length": Buffer.byteLength(body),
    });
    res.end(body);
    return;
  }
  res.writeHead(404).end();
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>Что такое server-side rendering and how can it be achieved with Node.js?</summary>

### Что такое server-side rendering and how can it be achieved with Node.js?

**Definition:**

Серверный рендеринг собирает HTML для URL на сервере и отправляет этот HTML ответом. Node — процесс, который запускает рендерер и пишет результат в `res`. Браузер рисует этот HTML, затем грузит клиентский бандл и вешает обработчики. Приложение только на клиенте вместо этого шлёт почти пустую HTML-оболочку и заполняет её в браузере.

**How it works:**

- Запрос попадает в `http.createServer`. Для маршрута страницы вызывают серверный рендер фреймворка (серверный рендерер React, platform-server Angular или мета-фреймворк вроде Next.js) и делают `res.end` со строкой HTML либо пайпят поток HTML.
- Потоковый рендер пишет документ по мере готовности. Ответ при этом — записываемый поток, и байты доходят до клиента раньше, чем если буферизовать всю страницу.
- Данные для страницы грузят на сервере до или во время этого рендера. Клиент не должен заново забирать всё только ради первой отрисовки.
- HTML должен совпасть с тем, что отрисует клиент, иначе гидрация предупредит и перепишет DOM. Любой пользовательский текст в разметке экранируйте.
- У Node по-прежнему один цикл событий. Медленный рендер блокирует другие запросы этого процесса. Кэшируйте страницы, которые не зависят от пользователя, или уберите рендер с горячего пути.
- Статическая генерация собирает HTML заранее, и Node (или файловый хост) только отдаёт файл. Это другое расписание, не рендер на каждый запрос.

**When to use:**

- Страницы, у которых первый HTML должен уже содержать содержимое (краулеры, медленные устройства, осмысленная первая отрисовка).
- Кабинеты за логином могут остаться клиентским рендером. API при этом всё равно Node.

**Examples:**

```js
import http from "node:http";

function renderPage(title) {
  return `<!doctype html><html><body><h1>${title}</h1></body></html>`;
}

http
  .createServer((_req, res) => {
    const html = renderPage("Notes");
    res.writeHead(200, {
      "Content-Type": "text/html; charset=utf-8",
      "Content-Length": Buffer.byteLength(html),
    });
    res.end(html);
  })
  .listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Stream](https://nodejs.org/api/stream.html)
</details>

### Node.js Best Practices

<details>
<summary>Что такое some coding conventions and best practices in Node.js?</summary>

### Что такое some coding conventions and best practices in Node.js?

**Definition:**

Пишите небольшие async-функции, импортируйте модули ядра с префиксом `node:` и держите конфигурацию в `process.env`. Ошибки обрабатывайте на месте вызова. Не блокируйте цикл событий методами `*Sync` или долгими циклами на CPU внутри сервера. Форматирование и линт гоняйте в CI, чтобы репозиторий оставался ровным.

**How it works:**

- `import { readFile } from "node:fs/promises"` называет встроенный модуль и не цепляет пакет, у которого случайно то же голое имя. Префикс `node:` — способ ядра сказать, что модуль встроен.
- `await` внутри `try/catch` на каждой границе I/O. Error-first колбэк возвращается после `if (err)`. Слушатель `'error'` висит на каждом эмиттере, которым вы владеете.
- Бросайте или отклоняйте через `Error`, чтобы был стек. Строка в роли ошибки выбрасывает трассировку.
- Обработчики запросов не держат общие изменяемые map, если это не сознательный кэш с лимитом. Worker-ы cluster эту map не видят.
- Файлы и функции называют по работе, которую они делают. Один модуль экспортирует одну обязанность. Скрипты `package.json` (`start`, `test`, `lint`) — команды, которые запускают все.
- ESLint (инструмент npm) кодирует правила. `node --test` кодирует поведение. Оба запускаются до слияния.

**When to use:**

- С первого файла сервиса. Соглашения дёшевы в начале и дороги, когда их вводят потом.
- Разовый скрипт может взять `readFileSync` и не следовать серверным правилам. Напишите это сверху.

**Examples:**

```js
import { readFile } from "node:fs/promises";

const fileUrl = new URL("./note.txt", import.meta.url);

try {
  const text = await readFile(fileUrl, "utf8");
  console.log(text);
} catch (err) {
  console.error(err);
  process.exitCode = 1;
}
```

**Sources:**

- [Node.js — Modules](https://nodejs.org/api/modules.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- [Node.js — Errors](https://nodejs.org/api/errors.html)
</details>

<details>
<summary>Как you ensure your Node.js application adheres to the twelve-factor app principles?</summary>

### Как you ensure your Node.js application adheres to the twelve-factor app principles?

**Definition:**

Модель двенадцати факторов описывает приложение как одну кодовую базу, явные зависимости, конфиг в окружении и процессы без своего состояния, которые биндят порт и пишут логи в stdout. Сервис на Node совпадает с этим через `package.json` и локфайл, `process.env`, `server.listen(port)` и `console` в stdout и stderr.

**How it works:**

- Один репозиторий (или один репозиторий на сервис) и `npm ci` из локфайла. Зависимости объявлены, а не лежат россыпью скопированных файлов.
- Конфиг — это `process.env` или `--env-file`. Закоммиченный `config/production.json` с секретами ломает фактор «конфиг в окружении».
- Внешние сервисы (база, очередь) подключаются URL. Смена URL меняет сервис. Хост в код не вшивают.
- Сборка, релиз и запуск разделены. CI собирает артефакт или образ. Релиз добавляет окружение. `node server.js` — стадия запуска.
- Процессы без своего состояния. Сессии и загруженные байты живут в хранилище, не в `Map` на уровне модуля, которая умирает с процессом и не видна следующему.
- Параллельность — это больше процессов (`cluster`, PM2 или больше контейнеров). Одноразовость — быстрый старт и обработчик `SIGTERM`, который делает `server.close()`. Логи — поток событий в stdout, их собирают снаружи приложения.

**When to use:**

- Любой сервис, который вы деплоите больше одного раза или переносите в другое окружение.
- Локальный скрипт может не биндить порт и не думать об одноразовости. Это не такой тип приложения.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

server.listen(Number(process.env.PORT ?? 8000));
console.log("listening");
```

**Sources:**

- [The Twelve-Factor App](https://12factor.net/)
- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

<details>
<summary>Что такое code linting and how is it applied in Node.js?</summary>

### Что такое code linting and how is it applied in Node.js?

**Definition:**

Линт читает исходник и сообщает о шаблонах, которые вы решили считать ошибками: неиспользуемые привязки, `==` там, где нужен `===`, забытый `await`. Обычный инструмент npm — ESLint. Линтера в ядре Node нет. Его запускают скриптом `package.json` и из CI.

**How it works:**

- Файл конфигурации (`eslint.config.js` для flat config) перечисляет плагины и правила. Парсер понимает тот JavaScript или TypeScript, который вы реально запускаете.
- `npx eslint .` завершается ненулевым кодом, когда правило падает. CI считает это упавшей сборкой, так же как упавший `node --test`.
- `--fix` переписывает механические случаи. Правила про корректность остаются отчётами, которые вы правите руками.
- Линт не выполняет программу и не доказывает типы. Это закрывают `tsc --noEmit` и тесты. Линт закрывает стиль и набор ловушек (пустой `catch`, `eval`).
- Интеграция с редактором показывает те же диагностики, пока вы печатаете, по тому же конфигу, что и задача CI.
- Правило, которое команда везде выключает, — шум. Набор держите таким, чтобы красная строка значила «это надо изменить».

**When to use:**

- Каждый пакет в репозитории, включая скрипты.
- Сгенерированные файлы и `node_modules` остаются в ignore, чтобы линтер видел только ваш код.

**Examples:**

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

**Sources:**

- [Node.js — package.json](https://nodejs.org/api/packages.html)
- [ESLint](https://eslint.org/docs/latest/use/getting-started)
</details>

### Scaling Node.js Applications

<details>
<summary>Что такое some strategies for scaling Node.js applications?</summary>

### Что такое some strategies for scaling Node.js applications?

**Definition:**

HTTP-сервис на Node масштабируют большим числом процессов и машин и тем, что цикл событий каждого процесса остаётся свободным. `node:cluster` занимает ядра одного хоста. Балансировщик занимает несколько хостов. Кэш, меньшие тела и перенос работы на CPU в `worker_threads` поднимают то, сколько успевает один процесс, прежде чем вы добавите следующий.

**How it works:**

- Вертикальный шаг: `cluster.fork()` или cluster-режим менеджера процессов, по worker на ядро. Worker-ы делят порт и не делят память.
- Горизонтальный шаг: обратный прокси или облачный балансировщик раскладывает трафик по хостам. Приложение остаётся без своего состояния, чтобы любой процесс обслужил любой запрос.
- Узкое место ищут по задержке цикла, CPU и задержке запроса. Новые процессы не чинят один медленный запрос к базе, которого ждёт каждый запрос.
- Ответы, которые в основном читают, кэшируют вне процесса (HTTP-кэш, Redis). `Map` внутри процесса другим worker-ам не помогает.
- Большие тела стримьте. `readFile` огромного файла на каждый запрос тратит память, под которую потом нужны ещё машины.
- Функции с тяжёлым CPU уходят в `worker_threads` или в отдельный сервис. HTTP-worker-ы остаются отзывчивыми.

**When to use:**

- После того как измерили предел: насыщение CPU, задержка цикла или сеть одного хоста.
- Ранний cluster-режим усложняет логи и деплои до того, как один процесс реально занят.

**Examples:**

```js
import cluster from "node:cluster";
import http from "node:http";
import { availableParallelism } from "node:os";

if (cluster.isPrimary) {
  for (let i = 0; i < availableParallelism(); i += 1) cluster.fork();
} else {
  http
    .createServer((_req, res) => {
      res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
      res.end("ok");
    })
    .listen(8000);
}
```

**Sources:**

- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — OS](https://nodejs.org/api/os.html#osavailableparallelism)
</details>

<details>
<summary>Как you handle session management in a scaled Node.js application?</summary>

### Как you handle session management in a scaled Node.js application?

**Definition:**

Данные сессии хранят там, куда может прочитать каждый процесс, либо держат сессию в подписанной cookie, которую браузер присылает обратно. `Map` в одном процессе Node не видна другим worker-ам и пропадает при рестарте. Механизм ядра — заголовок cookie. Общее хранилище — база или кэш, к которым ходят по сети.

**How it works:**

- `Set-Cookie` в ответе и заголовок `Cookie` в запросе — обычный HTTP. Помечайте cookie `HttpOnly`, `Secure` на HTTPS и `SameSite`.
- Id сессии в cookie указывает на строку или ключ Redis. Любой процесс, который достаёт до этого хранилища, загрузит сессию. Прилипание к одному процессу — более слабая замена, и оно ломается, когда этот процесс умирает.
- Подпишите или зашифруйте cookie, если значение и есть сессия. Неподписанную cookie клиент может изменить.
- Задайте срок жизни и удаляйте серверную запись при выходе. Cookie без срока — сессионная cookie браузера. Серверная запись всё равно должна истекать.
- Не кладите в cookie пароли и большие объекты. Кладите id, а тело держите в хранилище.
- `express-session` и похожие пакеты реализуют этот шаблон. Это библиотеки npm. Заголовки под ними всё те же: `Set-Cookie` и `Cookie`.

**When to use:**

- Браузерный логин, который должен пережить второй процесс Node или рестарт.
- Скрипту на одного пользователя сессия не нужна. Токен в `Authorization` — другой обычный выбор для клиентов не из браузера.

**Examples:**

```js
import http from "node:http";
import { randomBytes } from "node:crypto";

const sessions = new Map();

const server = http.createServer((req, res) => {
  const id = randomBytes(16).toString("hex");
  sessions.set(id, { createdAt: Date.now() });
  res.writeHead(200, {
    "Content-Type": "text/plain; charset=utf-8",
    "Set-Cookie": `sid=${id}; HttpOnly; SameSite=Lax`,
  });
  res.end("ok");
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Crypto](https://nodejs.org/api/crypto.html)
- [MDN — Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
</details>

<details>
<summary>Как the use of microservices affect the Scalability of a Node.js application?</summary>

### Как the use of microservices affect the Scalability of a Node.js application?

**Definition:**

Разделение приложения Node на сервисы позволяет запускать больше копий занятой части, не копируя тихую. Каждый сервис масштабируется своим числом процессов и своими данными. Платите сетевыми вызовами, частичным сбоем и операционной нагрузкой, которой не было у одного процесса.

**How it works:**

- Сервис с тяжёлым CPU может иметь много реплик. Редко вызываемый сервис может иметь одну. `cluster` и балансировщик применяются к сервису, а не к одному процессу монолита, который делает всю работу.
- Вызовы между сервисами — это I/O. Цикл событий с этим справляется хорошо, а задержка складывается по цепочке. Таймаут (`AbortSignal.timeout`) не даёт одному медленному звену держать вызывающего.
- Владение данными решает, сможете ли вы масштабироваться. Два сервиса, которые пишут в одни строки, упрутся в эту базу, сколько бы процессов Node вы ни запустили.
- Сбой теперь частичный. Вызывающему нужен ответ на «другой сервис лежит». Один общий процесс раньше ронял все маршруты вместе.
- Деплой и наблюдение — на каждый сервис. Id запроса в логах обеих сторон показывает одно действие пользователя через процессы.
- Начинайте с одного процесса Node, пока у измеренного узкого места нет границы. Схема микросервисов сама по себе мощность не создаёт.

**When to use:**

- Часть системы со своей нагрузкой, своим циклом релизов или своими данными.
- Небольшой API остаётся одним деплоем. Несколько копий этого одного сервиса вы всё равно можете запустить.

**Examples:**

```js
const response = await fetch(process.env.BILLING_URL, {
  signal: AbortSignal.timeout(2000),
});

if (!response.ok) {
  throw new Error(`billing status ${response.status}`);
}
```

**Sources:**

- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
</details>

### Node.js and Message Queues

<details>
<summary>Что такое message queues and how are they used in Node.js?</summary>

### Что такое message queues and how are they used in Node.js?

**Definition:**

Очередь сообщений позволяет одному процессу отдать работу и идти дальше. Продюсер публикует сообщение. Брокер его хранит. Консьюмер обрабатывает позже. Брокера сообщений в ядре Node нет. К Redis, RabbitMQ, NATS или облачной очереди ходят через пакет, а `postMessage` в worker из `worker_threads` используют, когда работа должна остаться внутри процесса.

**How it works:**

- HTTP-обработчик публикует и отвечает. Медленная работа (письмо, картинка, отчёт) идёт в другом цикле, поэтому запрос её не ждёт.
- Консьюмер часто — другой процесс Node на той же кодовой базе. Он делает `ack` после успеха. Падение до ack заставляет брокера доставить сообщение снова, поэтому работу должно быть безопасно выполнить дважды, либо она должна записать, что уже выполнилась.
- Очереди сглаживают всплески. Продюсеры какое-то время могут обгонять консьюмеров. Брокер держит хвост. Консьюмеры масштабируются большим числом процессов.
- `worker_threads` и `child_process.fork` — передача внутри процесса и на той же машине. Они исчезают, если родитель вышел. Брокер это переживает.
- Тело — байты или JSON. Держите его маленьким и кладите версию и id. Остальное консьюмер загружает из базы.
- Ядовитым сообщениям нужен предел. После нескольких сбоев уберите сообщение в сторону и поднимите тревогу, иначе консьюмер будет повторять его вечно.

**When to use:**

- Работа, которая может закончиться после HTTP-ответа, и работа, которая должна пережить рестарт процесса.
- Прямой `await` яснее, когда пользователь ждёт результат и вызов быстрый.

**Examples:**

```js
import { parentPort, Worker, isMainThread } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.postMessage({ id: 1, title: "resize" });
} else {
  parentPort.on("message", (job) => {
    parentPort.postMessage({ id: job.id, done: true });
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Child process](https://nodejs.org/api/child_process.html)
</details>

<details>
<summary>Как you implement RabbitMQ with Node.js?</summary>

### Как you implement RabbitMQ with Node.js?

**Definition:**

RabbitMQ — отдельный брокер. Node говорит с ним через npm-клиент, например `amqplib`. Подключаются AMQP URL, открывают канал, объявляют очередь и либо `sendToQueue`, либо `consume`. В `node:` нет реализации AMQP.

**How it works:**

- `amqp.connect(url)` открывает TCP-соединение. `createChannel()` — сессия, в которой публикуют и читают. URL лежит в `process.env`.
- `assertQueue(name)` создаёт очередь, если её нет. Публикация в очередь, которую никто не объявил, может упасть или потерять сообщение — это зависит от настройки брокера.
- `sendToQueue(name, Buffer.from(json))` публикует байты. `consume` вызывает вашу функцию на каждую доставку. `channel.ack(msg)` говорит брокеру, что работа сделана. `nack` просит доставить снова.
- Консьюмер, который бросил исключение до `ack`, увидит сообщение ещё раз. Обработчик должен быть безопасен при двойном запуске, либо вы храните id сообщения, которое уже закончили.
- Prefetch (`channel.prefetch(n)`) ограничивает, сколько неподтверждённых сообщений держит этот консьюмер. Так один медленный процесс не заберёт всю очередь в память.
- На `SIGTERM` закройте канал и соединение. Процесс, который вышел без закрытия, может оставить неподтверждённые сообщения до таймаута брокера.

**When to use:**

- Работа, которая должна пережить HTTP-процесс и разойтись по другим сервисам, которые тоже говорят на AMQP.
- Передачи в `worker_threads` хватает, когда другого консьюмера нет и рестарт может потерять задачу.

**Examples:**

```js
import amqp from "amqplib";

const conn = await amqp.connect(process.env.RABBIT_URL);
const channel = await conn.createChannel();
await channel.assertQueue("notes");

channel.sendToQueue("notes", Buffer.from(JSON.stringify({ id: 1 })));

channel.consume("notes", (msg) => {
  if (!msg) return;
  console.log(msg.content.toString());
  channel.ack(msg);
});
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [amqplib](https://amqp-node.github.io/amqplib/channel_api.html)
</details>

<details>
<summary>Что такое the significance of ZeroMQ in Node.js applications?</summary>

### Что такое the significance of ZeroMQ in Node.js applications?

**Definition:**

ZeroMQ — библиотека обмена сообщениями, а не брокер, который вы запускаете как сервер. Процессы соединяют сокеты (publish/subscribe, request/reply, push/pull), и ZeroMQ перевозит сообщения. Из Node используют npm-привязку, например `zeromq`. Это не модуль ядра. Сокеты — инструмент связи процессов рядом с `child_process` и HTTP.

**How it works:**

- Центральный процесс очереди деплоить не нужно. Издатель делает `bind` на TCP-адрес. Подписчики делают `connect`. Если никто не слушает, собственные очереди библиотеки решают, что отбросить и что оставить, по типу сокета и его high-water mark.
- API — это шаблоны. PUB/SUB рассылает. PUSH/PULL раздаёт работу по worker-ам. REQ/REP — один запрос, затем один ответ. Вы выбираете шаблон, а не строите топологию брокера.
- Сообщения — кадры байтов. Шлите `Buffer` или строку. Раскладку договариваете сами. Кода статуса HTTP нет.
- Привязка для Node сидит на цикле событий. Приём сообщения — колбэк или async-итератор. Медленный обработчик всё равно блокирует JavaScript этого процесса.
- На диск сообщения за вас никто не пишет. Мёртвый подписчик пропускает то, что опубликовали, пока он лежал, если вы сами не добавите хранение.
- Используйте её в сети, которой доверяете. Работа библиотеки — транспорт и шаблоны, не учётные записи.

**When to use:**

- Обмен между процессами с низкой задержкой в сети, которую вы контролируете, без эксплуатации RabbitMQ или похожего.
- Работе, которая должна пережить рестарты и подтверждаться, нужен брокер с хранением.

**Examples:**

```js
import { Publisher } from "zeromq";

const publisher = new Publisher();
await publisher.bind("tcp://127.0.0.1:3000");
await publisher.send(["notes", "hello"]);
```

**Sources:**

- [Node.js — Net](https://nodejs.org/api/net.html)
- [zeromq.js](https://github.com/zeromq/zeromq.js)
</details>

### Node.js and Cloud Services

<details>
<summary>Как cloud platforms like AWS, Azure, or GCP facilitate Node.js application deployment?</summary>

### Как cloud platforms like AWS, Azure, or GCP facilitate Node.js application deployment?

**Definition:**

Эти платформы запускают ваш процесс Node, выдают ему `PORT`, обрывают TLS и перезапускают его, когда он умирает. Вы по-прежнему отдаёте программу, которая делает `listen` и читает `process.env`. Платформа добавляет машину, балансировщик, логи и способ выкатить новую сборку вперёд или назад. Сам Node не знает, какой вендор его хостит.

**How it works:**

- Сервис контейнеров (ECS, Cloud Run, Azure Container Apps) запускает образ из гайда Node про Docker. PaaS-сборщик или App Service запускают `npm ci` и `node server.js` из репозитория. Сервис функций запускает одну экспортированную функцию на запрос.
- Платформа задаёт `PORT` и подкладывает секреты как переменные окружения. Код берёт `process.env.PORT` и не зашивает `80`.
- Проверки здоровья бьют в путь, который вы открыли. Проваленная проверка вынимает экземпляр из балансировщика. `server.close()` на `SIGTERM` даёт платформе осушить соединения.
- Логи в stdout собирают. Обычный путь — не заходить по SSH и не делать tail файла.
- Больше экземпляров — это настройка или правило автомасштаба. Каждый экземпляр — обычный процесс Node со своей памятью. Сессии по-прежнему лежат в общем хранилище.
- API вендора (объектное хранилище, очереди) — это HTTP или их SDK, вызов как у любого стороннего API. Это не модули ядра.

**When to use:**

- Когда хост, TLS и рестарты должен вести кто-то другой.
- Одна виртуальная машина с systemd или PM2 — меньшая версия той же идеи.

**Examples:**

```js
import http from "node:http";

const port = Number(process.env.PORT ?? 8000);

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  })
  .listen(port);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
</details>

<details>
<summary>Что такое serverless architecture, and how does it relate to Node.js?</summary>

### Что такое serverless architecture, and how does it relate to Node.js?

**Definition:**

Serverless здесь значит, что платформа запускает вашу функцию, когда приходит запрос или событие, и затем замораживает или останавливает её. Процесс с `server.listen` вы не держите. Node — обычный рантайм таких функций: платформа грузит файл и вызывает экспортированную функцию с событием. Вы всё равно пишете JavaScript. Машину вы не эксплуатируете.

**How it works:**

- Холодный старт грузит рантайм и ваш модуль. Тёплый старт переиспользует уже живой экземпляр. Глобалы и соединения могут пережить вызовы на тёплом экземпляре, и считать их долговечной сессией нельзя.
- Обработчик возвращает статус и тело или бросает ошибку. Платформа превращает это в HTTP-ответ. Своего долгого `IncomingMessage`, который вы сами принимаете, нет, если только продукт не контейнер, который умеет схлопываться в ноль.
- Лимиты времени короткие. Работа на минуты принадлежит консьюмеру очереди или обычному серверу, не функции запроса.
- Соединения с базой открывайте аккуратно. Новое соединение на каждый вызов исчерпает базу. Клиент в области модуля помогает только пока экземпляр тёплый.
- Запись на файловую систему эфемерна. Долговечные байты кладите в объектное хранилище. `node:fs` на многих платформах всё ещё работает с `/tmp`, и этот диск исчезает вместе с экземпляром.
- Локально функцию гоняют эмулятором вендора или прямым вызовом обработчика. Прод добавляет источник событий, предел параллельности и логи.

**When to use:**

- Рваный HTTP или событийная работа, за которую иначе пришлось бы платить простаивающему процессу.
- Ровный API с высокой нагрузкой часто дешевле и проще как долго живущий сервер Node.

**Examples:**

```js
export async function handler(event) {
  const name = event?.queryStringParameters?.name ?? "world";
  return {
    statusCode: 200,
    headers: { "Content-Type": "application/json; charset=utf-8" },
    body: JSON.stringify({ hello: name }),
  };
}
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

### Environment Management

<details>
<summary>Как you manage multiple Node.js versions on the same machine?</summary>

### Как you manage multiple Node.js versions on the same machine?

**Definition:**

Версии ставят рядом через менеджер версий и направляют `PATH` на одну из них. `nvm` и `fnm` — обычные инструменты на macOS и Linux. `nvs` работает на разных платформах. Каждый проект может записать нужную версию. Бинарник `node`, который вы получаете, — тот, который менеджер поставил первым в `PATH`.

**How it works:**

- Менеджер скачивает релиз с nodejs.org и распаковывает его в свой каталог. `nvm use 22` или `fnm use 22` переписывает `PATH` текущей оболочки. Другие проекты держат свой выбор.
- Файл `.nvmrc` или `.node-version` в репозитории называет версию. Менеджер читает его при входе в каталог, если этот хук включён.
- `"engines": { "node": ">=22" }` в `package.json` документирует диапазон. Сам бинарник он не переключает. `npm` может предупредить, если запущенная версия вне диапазона.
- Установка Node инсталлятором с nodejs.org или системным пакетом кладёт на машину один бинарник. Вторая установка его перезаписывает. Так две версии не удержать.
- CI должен использовать тот же мажор, что и разработка. Задайте его в workflow, а не берите умолчание образа.
- `process.version` печатает то, что реально запущено. Проверяйте его, когда баг есть только на одной машине.

**When to use:**

- Любая машина, на которой больше одного репозитория, и каждый репозиторий, который должен оставаться на известном мажоре.
- Одного глобального Node достаточно для одного приложения в серверном образе, который вы пересобираете при обновлении.

**Examples:**

```js
import process from "node:process";

console.log(process.version);
```

```console
node --version
```

**Sources:**

- [Node.js — Installing Node.js](https://nodejs.org/en/download)
- [Node.js — process.version](https://nodejs.org/api/process.html#processversion)
- [Node.js — package.json engines](https://nodejs.org/api/packages.html#engines)
</details>

<details>
<summary>Что такое .env files and how do they work in a Node.js application?</summary>

### Что такое .env files and how do they work in a Node.js application?

**Definition:**

Файл `.env` — список строк `KEY=VALUE`, которые на один запуск становятся переменными окружения. Node грузит его через `node --env-file=.env` или `process.loadEnvFile()`. Файл — локальная замена окружения, которое платформа подставит в проде. Сам по себе `node server.js` этот файл не читает.

**How it works:**

- Каждая строка не-комментарий задаёт одну переменную. Значения — строки. `loadEnvFile()` не перезаписывает переменную, которая уже есть в настоящем окружении. `NODE_OPTIONS` внутри файла ни на что не влияет.
- `--env-file=.env` грузит файл до старта скрипта, поэтому код верхнего уровня видит значения. `loadEnvFile()` грузит в момент вызова.
- Пакет `dotenv` — более старый инструмент npm, который так же заполняет `process.env` из файла такого вида. Встроенный загрузчик закрывает обычный случай.
- Положите `.env` в `.gitignore`. Коммитьте `.env.example` с ключами и пустыми значениями, чтобы остальные знали, что задать.
- В проде переменные лучше подставлять в конфиге сервиса. Копировать файл `.env` на сервер легко утечь и легко забыть.
- Не логируйте файл и весь объект `process.env`. Ключи вроде `DATABASE_URL` содержат секреты.

**When to use:**

- Локальная разработка и задачи CI, которым нужен повторяемый набор переменных.
- Хост, у которого уже есть хранилище секретов, должен использовать его. Файл `.env` — удобство разработчика.

**Examples:**

```js
import { loadEnvFile } from "node:process";

loadEnvFile(new URL("./.env", import.meta.url));

console.log(Boolean(process.env.DATABASE_URL));
```

**Sources:**

- [Node.js — process.loadEnvFile](https://nodejs.org/api/process.html#processloadenvfilepath)
- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
</details>

<details>
<summary>Опишите the usage of the config module in Node.js.</summary>

### Опишите the usage of the config module in Node.js.

**Definition:**

Модуля ядра `node:config` нет. Пакет npm `config` (node-config) читает JSON или JS из каталога `config/` и выбирает файл под `NODE_ENV`. `config.get("port")` возвращает слитое значение. Секреты по-прежнему живут в окружении. Этот пакет — один способ сложить несекретные значения по умолчанию.

**How it works:**

- `config/default.json` держит базу. `config/production.json` перекрывает ключи, когда `NODE_ENV=production`. Локальные перекрытия могут лежать в `config/local.json`, который не коммитят.
- `config.get(name)` бросает, если ключа нет, поэтому опечатка падает на старте, а не возвращает `undefined`.
- Переменные окружения могут перекрыть значения из файла, если вы описали соответствие в файле окружения, который понимает пакет. Соответствие явное. Случайный ключ `process.env` сам ключом конфига не становится.
- Слитый объект читают один раз. Правка JSON не меняет уже запущенный процесс. Чтобы подхватить новый файл, процесс перезапускают.
- Не кладите пароли базы в эти JSON, если файлы коммитятся. Пароль читайте из `process.env`, а в файле держите имя хоста и размер пула.
- Это зависимость, поэтому фиксируйте её как любой другой пакет. Ядро уже даёт `process.env` и `loadEnvFile`, если хватает файлов окружения.

**When to use:**

- Приложение с несколькими несекретными настройками, которые отличаются по окружениям и неудобны как десятки переменных.
- Небольшой сервис может остаться только на `process.env` и обойтись без пакета.

**Examples:**

```js
import config from "config";

const port = config.get("port");
console.log(port);
```

**Sources:**

- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
- [node-config](https://github.com/node-config/node-config)
</details>

### Node.js and Continuous Integration/Continuous Deployment (CI/CD)

<details>
<summary>Что такое continuous integration/deployment and how is it implemented for Node.js apps?</summary>

### Что такое continuous integration/deployment and how is it implemented for Node.js apps?

**Definition:**

Непрерывная интеграция на каждое изменение гоняет установку, линт и `node --test`, чтобы `main` оставался запускаемым. Непрерывная доставка собирает артефакт, который можно выпустить. Непрерывный деплой выпускает этот артефакт, когда проверки прошли. Для Node команды — `npm ci`, раннер тестов и затем деплой того же локфайла, который тестировали.

**How it works:**

- Push или pull request поднимает чистую машину. `npm ci` ставит локфайл. Он падает, если `package.json` и лок расходятся. В этом смысл.
- Дальше идут `node --test` и линтер. Ненулевой код выхода блокирует слияние. Версия Node в CI совпадает с `engines` и с тем, что запускают разработчики.
- Workflow кэширует каталог npm между прогонами. `node_modules` с ноутбука он не кэширует.
- Доставка собирает образ Docker или упакованный архив с того же коммита. Тег образа — id коммита, не один только `latest`.
- Деплой — поздняя задача, которая обновляет хост или облачный сервис. Секреты прода подставляют там. В лог тестов их не печатают.
- Упавший деплой должен остановиться и оставить предыдущую версию. Пайплайн не заходит по SSH и не правит файлы руками.

**When to use:**

- Любой репозиторий, куда пушит больше одного человека, и любой репозиторий, который деплоится.
- Частный эксперимент может гонять `node --test` локально, пока у него нет удалённого репозитория.

**Examples:**

```yaml
# .github/workflows/ci.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npm test
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [npm — npm ci](https://docs.npmjs.com/cli/commands/npm-ci)
</details>

<details>
<summary>Как you set up a CI/CD pipeline for a Node.js project?</summary>

### Как you set up a CI/CD pipeline for a Node.js project?

**Definition:**

Пайплайн — это задачи по порядку: checkout, установка через `npm ci`, линт, `node --test`, сборка образа, затем деплой этого образа. Каждая задача идёт на чистой машине. Деплой запускается только после тестов и только с ветки, которую вы выпускаете.

**How it works:**

- Версию Node фиксируют в workflow (`node-version: 22` или `.nvmrc`, который читает шаг установки). `cache: npm` позволяет поздним прогонам переиспользовать скачанные пакеты.
- `"test": "node --test"` в `package.json` — команда, которую вызывает CI. Ту же команду запускаете локально.
- Задачи, которые могут идти вместе, разделяйте: линт и тесты. Деплой зависит от обеих и от ветки (`main`).
- Контейнер собирают в CI из Dockerfile. Его пушат в реестр с тегом, равным SHA коммита. Шаг деплоя говорит хосту запустить этот тег.
- Учётные данные реестра и прод-окружение лежат в хранилище секретов CI. Файл workflow называет секреты. Их значений в нём нет.
- Пайплайн остаётся красным, когда падают `npm ci` или тесты. Зелёный деплой непроверенного кода отменяет смысл пайплайна.

**When to use:**

- Как только у приложения есть удалённый репозиторий и место, где оно работает.
- Начните с CI только из тестов, если хоста ещё нет. Задачу деплоя добавьте, когда хост появится.

**Examples:**

```json
{
  "scripts": {
    "test": "node --test",
    "lint": "eslint ."
  },
  "engines": {
    "node": ">=22"
  }
}
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [npm — npm ci](https://docs.npmjs.com/cli/commands/npm-ci)
- [Node.js — package.json](https://nodejs.org/api/packages.html)
</details>

### Node.js Interview Problem Solving and Scenarios

<details>
<summary>Как бы вы troubleshoot a SLOw running Node.js application?</summary>

### Как бы вы troubleshoot a SLOw running Node.js application?

**Definition:**

Сначала измерьте задержку цикла событий, CPU и время каждого запроса, и только потом меняйте код. Медленный процесс Node часто — заблокированный цикл (`readFileSync`, плотный цикл, огромный `JSON.parse`) или медленная зависимость, которой ждёт обработчик. `monitorEventLoopDelay` и `node --inspect` отличают одно от другого.

**How it works:**

- Логируйте метод, путь, статус и миллисекунды из `performance.now()` вокруг обработчика. Профилировать нужно медленный маршрут. Среднее по процессу его прячет.
- Если задержка цикла растёт вместе с CPU, снимите CPU-профиль (`node --inspect` или `node --prof`). Горячую функцию переносят в worker или переписывают.
- Если задержка цикла низкая, а запрос всё ещё медленный, время сидит в базе или в исходящем `fetch`. Замерьте этот вызов отдельно. Worker-ы `cluster` один запрос быстрее не сделают.
- `readFileSync`, синхронный crypto и `JSON.parse` гигантского тела видны как время главного потока. Стримьте или уберите эту работу с цикла.
- Смотрите `process.memoryUsage().heapUsed`. Утечка делает поздние запросы медленнее, потому что сборка мусора работает тяжелее. Сравните снимки кучи.
- Воспроизводите на фиксированном входе. Бенчмарк, который не похож на прод-трафик, отправит вас за не той функцией.

**When to use:**

- Тревоги по задержке или CPU и любой отчёт «после деплоя стало медленно».
- Догадки и новые серверы — после профиля, когда профиль говорит, что CPU правда кончился.

**Examples:**

```js
import { monitorEventLoopDelay, performance } from "node:perf_hooks";

const delay = monitorEventLoopDelay({ resolution: 20 });
delay.enable();

const started = performance.now();
setInterval(() => {
  console.log({
    elapsedMs: performance.now() - started,
    loopDelayMs: delay.mean / 1e6,
  });
}, 5000).unref();
```

**Sources:**

- [Node.js — Performance hooks](https://nodejs.org/api/perf_hooks.html#monitoreventloopdelayoptions)
- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Опишите how to handle file uploads in a Node.js application.</summary>

### Опишите how to handle file uploads in a Node.js application.

**Definition:**

Тело запроса — это поток. Задайте предел в байтах, затем `pipeline` байтов в `createWriteStream` или в объектное хранилище. `node:http` даёт сырое тело. `multipart/form-data` оно не разбирает. Пакет вроде `busboy` делит части. Каждую файловую часть всё равно стримьте, а не буферизуйте.

**How it works:**

- `req` — читаемый поток. Если собрать его в массив чанков, вся загрузка окажется в памяти. Запись на диск держит память около размера чанка.
- Остановитесь, когда счётчик байт перешёл предел. Ответьте `413` и уничтожьте запрос, чтобы остаток тела не сидел в процессе.
- У multipart есть границы и поля. `busboy` эмитит поток `file` на каждую часть. Этот поток направьте в `fs.createWriteStream` в каталог, который вы контролируете. Путь собирайте через `node:path` и отвергайте имена, которые выходят из каталога.
- Хранимое имя генерируйте сами (`randomBytes`). Имя файла от клиента — подпись, не путь.
- Проверьте тип и размер до того, как пометите загрузку готовой. `pipeline` закроет поток записи, чтобы сорванная загрузка не оставила полуфайл, который вы потом отдадите.
- Готовый путь или ключ объекта запишите в базу в том же шаге, который говорит, что загрузка удалась.

**When to use:**

- Любой маршрут, который принимает файл от клиента.
- Маленькое JSON-тело можно буферизовать с небольшим лимитом. Видео или серию картинок — нет.

**Examples:**

```js
import { createWriteStream } from "node:fs";
import { pipeline } from "node:stream/promises";
import { randomBytes } from "node:crypto";
import path from "node:path";
import http from "node:http";
import { fileURLToPath } from "node:url";

const dir = new URL("./uploads/", import.meta.url);

const server = http.createServer(async (req, res) => {
  if (req.method !== "POST" || req.url !== "/upload") {
    res.writeHead(404).end();
    return;
  }
  const name = `${randomBytes(16).toString("hex")}.bin`;
  const filePath = path.join(fileURLToPath(dir), name);
  await pipeline(req, createWriteStream(filePath));
  res.writeHead(201, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(name);
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Stream](https://nodejs.org/api/stream.html)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>Как бы вы handle heavy computation tasks in a Node.js application?</summary>

### Как бы вы handle heavy computation tasks in a Node.js application?

**Definition:**

Уберите работу на CPU с главного цикла событий. `worker_threads` выполняет её в другом изоляте V8 внутри того же процесса. Дочерний процесс или другой сервис делают то же, когда нужен отдельный жизненный цикл. HTTP-обработчик остаётся асинхронным и только ждёт сообщение.

**How it works:**

- Плотный цикл, кодирование картинки или сильное сжатие на главном потоке останавливают каждый сокет, пока функция не вернётся. `await` не помогает, если сама функция — синхронный JavaScript.
- `new Worker(url)` грузит файл. `postMessage` шлёт вход. Worker отправляет результат обратно. Обработчик `'message'` на главном потоке завершает HTTP-ответ.
- Переиспользуйте маленький пул worker-ов. Worker на каждый запрос тратит время на старт изолята.
- Данные копируются, если вы не передаёте `ArrayBuffer`. Копия огромного буфера может стоить столько же, сколько сама работа. Передавайте его, когда главному потоку эти байты больше не нужны.
- `node:cluster` добавляет процессы для большей параллельности HTTP. Один тяжёлый вызов он быстрее не делает. Вызов всё равно блокирует worker, который его принял, пока тот worker не отдаст работу потоку.
- Если работа может закончиться после ответа, опубликуйте её в очередь и дайте процессу-консьюмеру выполнить CPU.

**When to use:**

- Любой путь запроса, чьё время CPU уже двигает задержку цикла.
- Короткие вычисления остаются на главном потоке. У worker есть накладные расходы.

**Examples:**

```js
import { Worker, isMainThread, parentPort } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.once("message", (total) => {
    console.log(total);
  });
  worker.postMessage(1_000_000);
} else {
  parentPort.once("message", (n) => {
    let total = 0;
    for (let i = 0; i < n; i += 1) total += i;
    parentPort.postMessage(total);
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

### Node.js and DevOps

<details>
<summary>Что такое the role of a Node.js application in DevOps?</summary>

### Что такое the role of a Node.js application in DevOps?

**Definition:**

Приложение Node — это нагрузка, которую запускает DevOps, и иногда скрипт, который автоматизирует шаг. Оно слушает порт, пишет логи в stdout, чисто выходит по `SIGTERM` и открывает путь проверки здоровья. Пайплайн ставит его через `npm ci`, тестирует через `node --test` и отправляет тот же артефакт. Node — не оркестратор.

**How it works:**

- Маршрут здоровья возвращает 200, когда процесс может обслуживать трафик. Платформа по нему добавляет или убирает экземпляр. Маршрут, который всегда отвечает 200, пока база лежит, прячет сбой.
- Логи — строки в `process.stdout` и `process.stderr`. Сборщик снаружи процесса. Файл лога внутри контейнера требует ротации, которую потом надо эксплуатировать.
- `SIGTERM` значит «перестань принимать и закончи текущее». Это делает `server.close()`. Процесс, который игнорирует сигнал, убьют посреди запроса.
- Скрипты `package.json` — контракт, который вызывает CI: `test`, `lint`, `start`. Пайплайн не должен выдумывать команду, отличную от той, что запускают разработчики.
- Конфиг и секреты приходят как `process.env` в момент релиза. Образ один и тот же во всех окружениях.
- Небольшие скрипты Node в репозитории (миграции, проверка релиза) уместны. Они на том же рантайме. Вторым прод-сервером они становятся только если вы их так деплоите.

**When to use:**

- Каждый сервис под менеджером процессов, контейнерной платформой или облачным хостом.
- Принципы те же, даже когда «команда DevOps» — те же люди, которые пишут маршрут.

**Examples:**

```js
import http from "node:http";
import process from "node:process";

const server = http.createServer((req, res) => {
  if (req.url === "/health") {
    res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ ok: true }));
    return;
  }
  res.writeHead(404).end();
});

server.listen(Number(process.env.PORT ?? 8000));

process.on("SIGTERM", () => {
  server.close(() => process.exit(0));
});
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
</details>

<details>
<summary>Опишите containerization and its benefits for Node.js applications.</summary>

### Опишите containerization and its benefits for Node.js applications.

**Definition:**

Образ контейнера упаковывает версию Node, установку по локфайлу и исходник, чтобы на каждой машине была одна и та же файловая система. Обычная отправная точка — официальный гайд Node про Docker: `FROM node`, `npm ci`, `CMD ["node", "server.js"]`. Польза — повторяемый запуск, меньший радиус ущерба и деплой вида «запусти этот образ», а не «настрой этот сервер».

**How it works:**

- Образ фиксирует мажор Node. У коллеги с другой версией на ноутбуке в проде больше не окажется другой `process.version`.
- `npm ci` внутри образа ставит Linux-бинарники нативных аддонов под этот образ, а не под ваш macOS `node_modules`.
- Процесс изолирован: своя файловая система, своё окружение, опубликованный порт. Другое приложение на хосте не делит этот `node_modules`.
- Неизменяемые образы катятся вперёд и назад по тегу. Работающий контейнер не патчат. Запускают новый.
- Оркестраторы запускают много копий одного образа. Приложение должно остаться без своего состояния, слушать `PORT` и обрабатывать `SIGTERM`, иначе копии будут драться за локальные файлы и игнорировать остановку.
- Образы больше скрипта, и им нужны реестр и цикл патчей, когда в базовом образе закрывают уязвимость Node.

**When to use:**

- Любой сервис, который должен одинаково работать в CI и в проде или делить хост с другими сервисами.
- Одна виртуальная машина, где уже стоит нужный бинарник Node, может запустить `node server.js` без образа. Настройку этой машины тогда ведёте вы.

**Examples:**

```dockerfile
FROM node:22-bookworm-slim
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev
COPY . .
ENV NODE_ENV=production
EXPOSE 8000
USER node
CMD ["node", "server.js"]
```

**Sources:**

- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

### Node.js and IoT

<details>
<summary>Как Node.js used in IoT (Internet of Things)?</summary>

### Как Node.js used in IoT (Internet of Things)?

**Definition:**

Node хорошо сидит на шлюзе и на небольшом сервере, с которым говорит много устройств: много соединений, маленькие сообщения и работа, которая в основном ждёт. Цикл событий держит эти сокеты открытыми. Библиотеки npm говорят на протоколах устройств (клиенты MQTT, последовательные порты, GPIO на платах, где Node запускается, например Raspberry Pi). Крошечный микроконтроллер обычно Node не запускает. Он шлёт сообщения процессу Node, который может.

**How it works:**

- Устройства публикуют показания по MQTT или HTTP. Процесс Node подписывается или принимает запрос, проверяет тело и пишет его в хранилище или очередь. Этот процесс — обычный сервис Node.
- На плате с полноценной ОС программа Node может открыть последовательный порт или GPIO через npm-привязку. Такие привязки — нативные аддоны, не модули ядра. `node:net` и `node:http` — то, что встроено.
- Сообщения остаются маленькими. Показание датчика — JSON-объект или несколько байт. Потоки и обратное давление всё равно важны, когда устройство шлёт быстрее, чем вы сохраняете.
- Устройство и облако — разные зоны отказа. Шлюз копит показания, пока сети нет, и отправляет их, когда она вернулась.
- Безопасность — на устройство: учётные данные, TLS до брокера и способ отвергнуть устройство, которым вы больше не владеете.
- Johnny-Five и похожие проекты npm — способ крутить любительское железо из JavaScript. Это пакеты. В бинарник `node` они не входят.

**When to use:**

- Шлюз, API приёма или плата, где уже есть Linux и достаточно RAM для Node.
- Датчику на батарейке нужен меньший рантайм и радио. Node стоит на другом конце этого радио.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  if (req.method === "POST" && req.url === "/readings") {
    res.writeHead(202, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ accepted: true }));
    return;
  }
  res.writeHead(404).end();
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Net](https://nodejs.org/api/net.html)
</details>

<details>
<summary>Что бы вы you consider when developing a Node.js application for IoT devices?</summary>

### Что бы вы you consider when developing a Node.js application for IoT devices?

**Definition:**

Планируйте маленькие сообщения, рвущуюся сеть и устройства, которые нельзя патчить как сервер. Держите процесс Node на шлюзе или на пути приёма в облаке, если только у платы нет RAM и полноценной ОС. Ограничивайте каждое тело, ставьте работу в очередь, когда канала нет, и давайте каждому устройству свои учётные данные.

**How it works:**

- Память на плате класса Pi ограничена. Утечка или `Map` показаний без предела убьёт процесс. Ограничивайте буферы и стримьте на диск или в брокер.
- Сеть пропадает. Храните показания локально и отправляйте позже. HTTP-вызов с `AbortSignal.timeout` должен быстро упасть и оставить показание в очереди.
- Часы расходятся, и доставки повторяются. Дайте каждому показанию id и время устройства, чтобы повтор не вставил вторую строку.
- TLS до брокера или API и способ отозвать одно устройство, не меняя секрет у всех остальных. Не зашивайте один общий пароль в образ прошивки.
- Доступ к GPIO и последовательному порту — нативные аддоны. Блокирующий вызов в таком аддоне тормозит цикл событий и задерживает остальные датчики этого процесса.
- Обновление должно быть безопасно прервать. Наполовину записанное приложение на устройстве в поле само не поднимется. Предпочтительнее шлюз, который вы можете задеплоить заново, чем перепрошивка микроконтроллера.

**When to use:**

- Когда процесс Node стоит на стороне устройства в IoT-системе или принимает трафик устройств.
- Обычный веб-API, который только хранит показания, может следовать обычным правилам HTTP и всё равно применять лимит тела и таймаут.

**Examples:**

```js
const response = await fetch(process.env.INGEST_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ deviceId: "pi-1", tempC: 21.5 }),
  signal: AbortSignal.timeout(3000),
});

if (!response.ok) {
  throw new Error(`ingest status ${response.status}`);
}
```

**Sources:**

- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

### Node.js and Machine Learning

<details>
<summary>Можете ли вы use Node.js for machine learning? If so, how?</summary>

### Можете ли вы use Node.js for machine learning? If so, how?

**Definition:**

Да, в основном чтобы выполнить модель или вызвать сервис, который её выполняет. Node — сильное место для API, очереди и подготовки JSON и файлов. Обучение больших моделей обычно делают на Python. Модуля машинного обучения в ядре Node нет. Инференс идёт через пакет npm вроде TensorFlow.js или ONNX Runtime, либо через HTTP к серверу модели.

**How it works:**

- Запрос попадает в `node:http` или `fetch`. Вы проверяете вход, запускаете модель или вызываете сервис модели и возвращаете JSON. Цикл событий должен оставаться свободным, поэтому тяжёлый числовой цикл уходит в worker thread или в тот другой сервис.
- TensorFlow.js (`@tensorflow/tfjs-node`) выполняет графы внутри процесса, с нативным бэкендом. ONNX Runtime для Node грузит файл `.onnx`, который вы экспортировали из стека обучения.
- Вызов Python-сервиса по HTTP оставляет стек обучения там, где он есть, а Node остаётся продуктовым API. Таймауты и очередь работают так же, как у любого стороннего вызова.
- Модели — большие файлы. Грузите их один раз на старте, не на каждый запрос. Файл кладут рядом с приложением или скачивают из объектного хранилища.
- Входам и выходам нужна явная схема. Модель, которая ждёт массив чисел, не примет сырую загрузку, пока вы её не преобразуете.
- Сборки под GPU и нативные бинарники зависят от платформы. Образ Docker должен совпасть с бинарником, который вы поставили. `npm ci` на ноутбуке не даёт Linux-аддон.

**When to use:**

- Инференс рядом с API на Node или оркестрация вокруг модели, которая живёт в другом месте.
- Исследовательское обучение, ноутбуки и большие задачи на GPU остаются в тех инструментах Python, которые модель создали.

**Examples:**

```js
const response = await fetch(process.env.MODEL_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ features: [0.2, 1.4, 3.1] }),
  signal: AbortSignal.timeout(5000),
});

if (!response.ok) {
  throw new Error(`model status ${response.status}`);
}

const prediction = await response.json();
console.log(prediction);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
</details>

<details>
<summary>Что такое some machine learning libraries or tools available for Node.js?</summary>

### Что такое some machine learning libraries or tools available for Node.js?

**Definition:**

Это пакеты npm, не модули ядра. TensorFlow.js (`@tensorflow/tfjs-node`) выполняет графы TensorFlow в процессе Node. ONNX Runtime (`onnxruntime-node`) грузит модели `.onnx`, экспортированные из других инструментов обучения. Для более тяжёлого Node вызывает сервер модели по HTTP. Модуля `node:ml` нет.

**How it works:**

- `@tensorflow/tfjs-node` использует нативный бэкенд, поэтому тензорная математика — не чистый JavaScript. Его ставят под ту платформу, где вы запускаетесь. Браузерная сборка (`@tensorflow/tfjs`) — другой пакет и этой нативной привязки не использует.
- `onnxruntime-node` создаёт сессию инференса из пути к файлу и выполняет именованные входы. Файл модели — артефакт, который версионируют рядом с кодом или скачивают на старте.
- Оба пакета могут блокировать цикл событий на большом `run`. Вызывайте их из worker `worker_threads`, когда API должен оставаться отзывчивым.
- Hugging Face и похожие хосты отдают инференс по HTTP. Хватает `fetch`. Веса в процесс Node не встраивают.
- API обучения есть в части этих библиотек, и на обработчике запроса это плохой выбор по умолчанию. Обучайте офлайн, экспортируйте модель и грузите замороженный граф в Node.
- Фиксируйте версию пакета. Нативный аддон, собранный под один ABI Node, падает на следующем мажоре, пока вы его не пересоберёте.

**When to use:**

- Инференс в процессе, когда модель достаточно маленькая и нужен один деплой.
- Отдельный сервис модели, когда модели нужен GPU, которого нет на узлах API.

**Examples:**

```js
import { InferenceSession } from "onnxruntime-node";

const session = await InferenceSession.create("./model.onnx");
console.log(session.inputNames);
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [ONNX Runtime — Node.js](https://onnxruntime.ai/docs/get-started/with-javascript/node.html)
- [TensorFlow.js — Node](https://www.tensorflow.org/js/guide/nodejs)
</details>

### APIs and Node.js

<details>
<summary>Что такое best practices for designing RESTful APIs in Node.js?</summary>

### Что такое best practices for designing RESTful APIs in Node.js?

**Definition:**

URL моделируют как ресурсы, методы HTTP — как действия, а коды статуса — как исход. JSON-тела держите небольшими, списки листайте страницами и проверяйте ввод до обращения к базе. Маршруты реализуют на `node:http` или на роутере поверх него. Практики — это HTTP. Фреймворк их не приносит.

**How it works:**

- В пути существительные (`/notes`, `/notes/1`). `GET` читает, `POST` создаёт, `PUT` заменяет, `PATCH` меняет часть, `DELETE` удаляет. `GET`, `PUT` и `DELETE` должно быть безопасно повторить.
- `201` с `Location` после создания, `204` когда тела нет, `400` для плохого запроса, `404` когда id нет, `409` для конфликта. Тела ошибок — JSON стабильной формы, не стек.
- Списки принимают `limit` и курсор или смещение и не возвращают всю таблицу. `URLSearchParams` читает query из `req.url`.
- Ломающее изменение версионируйте под `/v1`. Необязательные поля добавляйте без новой версии.
- Ставьте `Content-Type: application/json; charset=utf-8` и `Content-Length` из `Buffer.byteLength`. Ограничьте размер тела до `JSON.parse`.
- Аутентифицируйте до того, как обработчик начнёт работу. Логируйте метод, путь и статус. Заголовок авторизации не логируйте.

**When to use:**

- API ресурсов для браузеров, мобильных приложений и других сервисов.
- Двусторонний поток — другая схема. Не впихивайте его в REST-ресурс.

**Examples:**

```js
import http from "node:http";

function send(res, status, body) {
  const payload = JSON.stringify(body);
  res.writeHead(status, {
    "Content-Type": "application/json; charset=utf-8",
    "Content-Length": Buffer.byteLength(payload),
  });
  res.end(payload);
}

const server = http.createServer((req, res) => {
  const url = new URL(req.url ?? "/", "http://localhost");
  if (req.method === "GET" && url.pathname === "/notes") {
    send(res, 200, {
      items: [],
      limit: Number(url.searchParams.get("limit") ?? 20),
    });
    return;
  }
  send(res, 404, { error: "not found" });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — URL](https://nodejs.org/api/url.html)
- [MDN — HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)
</details>
