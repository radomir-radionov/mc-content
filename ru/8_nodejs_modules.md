# Node.js — Modules

Менеджеры пакетов, CommonJS/ESM и встроенные модули (`fs`, `path`, `http`и связанные API).

[ ← Вернуться в хаб](./8_nodejs.md)

### Related docs

- [Node.js — Core](./8_nodejs_core.md) · [Node.js — Модули](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Дополнительно](./8_nodejs_process.md) · [Node.js — Интервью](./8_nodejs_interview.md)

### Main concepts

| Площадь               | Темы                                              |
| --------------------- | ------------------------------------------------- |
| **Пакеты**            | npm, пряжа, pnpm, package.json, структура проекта |
| **Модульные системы** | CommonJS `require`, ЕСМ `import`/`export`         |
| **Встроенные**        | libuv, fs, путь, os, http, URL, кластер, Winston  |

### Sections

- [Modules & package management](#modules-package-management)
- [Built-in modules](#built-in-modules)

---

## Modules & package management

<details>
  <summary>npm, npx & nvm</summary>

### npm, npx & nvm

**Definition:**

Инструменты для установки пакетов, запуска двоичных файлов и управления версиями Node.

| Инструмент | Цель                                                      |
| ---------- | --------------------------------------------------------- |
| `npm`      | менеджер пакетов — установка, публикация, скрипты         |
| `npx`      | запускать двоичные файлы пакетов без глобальной установки |
| `nvm`      | переключаться между версиями Node.js для каждого проекта  |

**How it works:** `MAJOR.MINOR.PATCH` — поломка/особенность/исправление.

**Examples:**

```bash
npm install express
npx create-next-app@latest my-app
nvm install 20 && nvm use 20
```

**Sources:** [docs.npmjs.com](https://docs.npmjs.com/) · [github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

</details>

<details>
  <summary>npm vs yarn vs pnpm</summary>

### npm vs yarn vs pnpm

**Definition:**

Менеджеры пакетов для JavaScript. Все используют реестр npm; различаются форматом файла блокировки, скоростью установки и расположением зависимостей.

| | НПМ | Пряжа | пнпм |
| | --- | ----- | ------ |
| | Заблокировать файл | пакет-lock.json | пряжа.замок | pnpm-lock.yaml |
| | Скорость установки | хорошо | быстрый (параллельный) | очень быстро (хранилище с адресацией по содержимому) |
| | Использование диска | копии для каждого проекта | копии для каждого проекта | общий магазин, жесткие ссылки |
| | По умолчанию с узлом | да | нет | нет |

**When to use:** придерживаться соглашения команды/проекта; pnpm для монорепозиториев и эффективности диска.

**Sources:** [pnpm.io](https://pnpm.io/) · [yarnpkg.com](https://yarnpkg.com/)

</details>

<details>
  <summary>Dependencies & package.json</summary>

### Dependencies & package.json

**Definition:**

`package.json` описывает проект; `node_modules` хранит установленные пакеты; `package-lock.json` точные версии контактов для воспроизводимых установок.

| Поле/файл           | Роль                                  |
| ------------------- | ------------------------------------- |
| `dependencies`      | требуется во время выполнения         |
| `devDependencies`   | сборка, тестирование, только проверка |
| `node_modules`      | установленное дерево пакетов          |
| `package-lock.json` | заблокированный график зависимостей   |

**Examples:**

```bash
npm install lodash          # dependencies (-S default)
npm install -D jest         # devDependencies
```

**Sources:** [docs.npmjs.com package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)

</details>

<details>
  <summary>require & packages</summary>

### require & packages

**Definition:**

Пакет — это распространяемая единица кода (библиотека, модуль или приложение). `require()` (CommonJS) или `import` (ESM) загружает модули; метаданные живут в `package.json`.

| Система  | Синтаксис                  | Заметки                               |
| -------- | -------------------------- | ------------------------------------- |
| CommonJS | `const fs = require('fs')` | синхронная нагрузка, `module.exports` |
| ЕСМ      | `import fs from 'fs'`      | статический анализ, `export`          |

**Examples:**

```js
// CommonJS
const express = require("express");
module.exports = { createApp: () => express() };

// ESM
import express from "express";
export function createApp() {
  return express();
}
```

**Sources:** [модули nodejs.org](https://nodejs.org/api/modules.html) · [nodejs.org esm](https://nodejs.org/api/esm.html)

</details>

<details>
  <summary>ESM vs CommonJS (deep dive)</summary>

### ESM vs CommonJS (deep dive)

**Definition:** Node поддерживает две модульные системы. **CommonJS (CJS)** использует `require`/`module.exports` (синхронная нагрузка). **ESM** использует `import`/`export` (статический анализ, асинхронная загрузка).

**How it works:**

| Аспект                     | CommonJS                            | ЕСМ                                                       |
| -------------------------- | ----------------------------------- | --------------------------------------------------------- |
| Синтаксис                  | `require()`, `module.exports`       | `import`, `export`                                        |
| Загрузка                   | Синхронный, среда выполнения        | Асинхронный, анализируется перед запуском                 |
| `this` на высшем уровне    | `module.exports`                    | `undefined` (строгий)                                     |
| `__dirname` / `__filename` | Доступно                            | Использовать `import.meta.url`                            |
| Импорт JSON                | `require('./x.json')`               | `import x from './x.json' with { type: 'json' }`          |
| Динамический импорт        | `require(dynamic)`                  | `await import(dynamic)`                                   |
| Дерево-тряска              | Ограниченная                        | Лучше (статический импорт)                                |
| Взаимодействие             | ЕСМ может `import` CJS по умолчанию | ЗАО `require()` ESM, только если ESM является асинхронным |

```json
{ "type": "module" }
```

or `.mjs` расширение. `.cjs` заставляет CommonJS `"type": "module"` проекты.

`import fs from 'fs'` — Модуль CJS `module.exports` становится экспортом по умолчанию. Может потребоваться именованный экспорт из CJS `import pkg from 'cjs'; const { x } = pkg`.

**When to use:** Новые проекты → ЕСМ (`"type": "module"`). Устаревшие библиотеки → CJS. Двойные пакеты публикуют оба `main` и `exports`.

**Pluses and minuses:**

| + (ЕСМ)                                   | − (ЭСМ)                             |
| ----------------------------------------- | ----------------------------------- |
| Стандарт в браузерах и современном Node   | Экосистема все еще мигрирует        |
| Верхний уровень `await`                   | `__dirname` необходим обходной путь |
| Статический анализ, встряхивание деревьев | Некоторые инструменты ожидают CJS   |

**Examples:**

```js
// ESM — dirname equivalent
import { fileURLToPath } from "node:url";
import { dirname, join } from "node:path";
const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);

// Dynamic import in CJS
const { createServer } = await import("node:http");
```

**Thoughts:** `require` не определен в собственном ESM. Использовать `createRequire(import.meta.url)` от `node:module` для наследия `require` в файлах ESM.

**Sources:** [nodejs.org ESM](https://nodejs.org/api/esm.html) · [модули nodejs.org](https://nodejs.org/api/modules.html)

</details>

<details>
  <summary>CLI commands & flags</summary>

### CLI commands & flags

**Definition:**

Общие команды оболочки и npm для разработки Node.js.

| Команда              | Цель                                           |
| -------------------- | ---------------------------------------------- |
| `npm i`              | установить зависимости из package.json         |
| `npm install -S pkg` | добавить зависимость времени выполнения        |
| `npm install -D pkg` | добавить зависимость от разработчиков          |
| `npm publish`        | опубликовать пакет в реестре                   |
| `ls` / `ls -la`      | список файлов (включая скрытые с помощью `-a`) |
| `cat file`           | распечатать содержимое файла                   |
| `touch file`         | создать пустой файл                            |
| `rm`                 | удалить файлы/каталоги                         |
| `pwd`                | распечатать рабочий каталог                    |
| `chmod +x file`      | сделать файл исполняемым                       |
| `history`            | список предыдущих команд оболочки              |

**Examples:**

```bash
npm install -D typescript
npm run build
```

**Sources:** [docs.npmjs.com cli](https://docs.npmjs.com/cli/v10/commands)

</details>

<details>
  <summary>Project structure</summary>

### Project structure

**Definition:** Организация проектов Node.js для удобства обслуживания — отдельные маршруты, контроллеры, сервисы и конфигурации.

**How it works:**

| Узор               | Структура                                                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| МВК                | `routes/` → `controllers/` → `models/`                                                                                        |
| На основе функций  | `features/users/`, `features/auth/`                                                                                           |
| Чистая архитектура | `domain/`, `use-cases/`, `infrastructure/` — [трехслойные заметки](./7_special_architecture.md#three-layer-architecture-doka) |

**Examples:**

```
src/
  routes/
  controllers/
  services/
  middleware/
  config/
  index.js
```

**Sources:** [лучшие практики nodejs.org](https://github.com/goldbergyoni/nodebestpractices)

</details>

---

<a name="built-in-modules"></a>

## Built-in modules

<details>
  <summary>libuv</summary>

### libuv

**Definition:** Кроссплатформенная C-библиотека под Node — не модуль `node:`, который можно импортировать.

Полный разбор: [libuv](./8_nodejs_core.md#libuv) в **Node.js — Core** (цикл событий, пул потоков, `UV_THREADPOOL_SIZE`, сеть vs файловый I/O).

**Sources:** [libuv design](https://docs.libuv.org/en/v1.x/design.html) · [Node.js — Core: libuv](./8_nodejs_core.md#libuv)

</details>

<details>
  <summary>Files & file systems</summary>

### Files & file systems

**Definition:** **Файл** — это именованная последовательность байтов, хранящаяся на физическом носителе, плюс **атрибуты**, указывающие ОС, как с ней обращаться. **Файловая система** — это абстракция ОС, которая выделяет это хранилище, скрывает аппаратные различия и разрешает конфликты имен.

**How it works:**

- ОЗУ хранит рабочие данные в байтах. Постоянство — это именованный файл на диске/SSD (произвольный доступ), исторически также на ленте (последовательные записи) или перфокартах (записи фиксированной длины).
- Сеанс: **открыть** → чтение/запись/атрибуты → **закрыть**. Ядро отслеживает каждый открытый файл с помощью числового **дескриптора файла** (POSIX) или его эквивалента (Windows). Узел сопоставляется как с `fd` / `FileHandle`.
- **Буферизация:** записи часто сначала попадают в буфер ОС, а затем сбрасываются в фоновом режиме — пользовательский интерфейс может выглядеть «сохраненным», пока файл на диске все еще растет.
- Типичные API-интерфейсы ОС: `open`, `close`, `read`/`write` (относительно курсора), `seek`, `delete`, `rename`, атрибуты чтения/записи.
- **Unix:** устройства, каталоги и каналы представляют собой специальные файлы; программы используют одни и те же файловые API. Mount присоединяет другой том к одному дереву (`mount` / `umount`).

| Модель                  | Идея                                                         | Где                                               |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------- |
| Квартира (один уровень) | Уникальные имена в одном пространстве имен                   | Миниатюрные устройства (прошивка бытовой техники) |
| Фиксированная глубина   | Несколько жестко запрограммированных уровней                 | Маршрутизаторы, телевизоры                        |
| Иерархический           | Дерево: **корневой каталог**, каталоги, **файлы как листья** | Настольные компьютеры, серверы, телефоны          |

Стратегия коллизий: запретить повторяющиеся имена **в одном и том же каталоге**.Одно и то же базовое имя в разных каталогах уникально благодаря **полному пути**. Сепараторы: `/` (Unix) против `\` (Окна). Относительные имена объединяются в текущий (или домашний) каталог.

Сильные стороны Unix FS: простая модель разрешений, символические ссылки, каталоги и устройства в виде файлов, одно монтируемое пространство имен.

- **Абсолютное** = от корня (`/` or `C:\…`). **Относительный** = из рабочего каталога. Родственники остаются переносимыми внутри папки проекта; ОС по-прежнему преобразует их в абсолютные при доступе.
- **`.`** = текущий каталог, **`..`** = родитель, **`~`** = дом (обычно `/home/user`).
- Имена Unix **чувствительны к регистру** (`File1` ≠ `file1`). Windows NTFS обычно **сохраняет регистр, но сопоставляет без учета регистра**. Unix запрещает `/` и `\0` в именах.
- Окна зарезервированы в именах: `\ / : * ? " < > |`. Пробелы нуждаются в кавычках. Устаревшая версия **MAX_PATH ≈ 260**; Windows 10+ может снять эту проблему, но старые клиенты все равно могут выйти из строя.
- **Расширение** (`.png`, `.exe`) — это подсказка для пользователей и приложений по умолчанию. Настоящий тип живет в атрибутах/магических байтах. Двоичные файлы Unix часто не имеют расширения **no**; `.sh` / `.conf` являются условностью.

| Режиссёр         | Роль                                         |
| ---------------- | -------------------------------------------- |
| `/bin`, `/sbin`  | Основные двоичные файлы пользователя/системы |
| `/boot`          | Загрузчик + образы ядра                      |
| `/dev`           | Файлы устройств                              |
| `/etc`           | Конфигурация для конкретного хоста           |
| `/home`, `/root` | Дома пользователей; дом корня                |
| `/lib`           | Общие библиотеки, модули ядра                |
| `/mnt`, `/opt`   | Временные крепления; дополнительные пакеты   |
| `/proc`, `/sys`  | Виртуальные файлы ядра/sysfs                 |
| `/tmp`           | Временное                                    |
| `/usr`           | Иерархия второго уровня (разделяемая)        |
| `/var`           | Переменные данные (логи, катушка)            |

**When to use:**

Узел `fs` / `path`; Docker [bind mounts](./10_build_docker.md#bind-mounts); Адресная строка SPA по сравнению с реальными файлами ([URI](./6_web_api_core.md#uri-url--urn), History API); файлы пользователя браузера ([File API](./3_js_browser_extra.md#formdata-file-api-filereader)).

**Пример** (Узел — открыть/стат/закрыть; предпочитают `FileHandle` так что fd не утек):

```js
import { open } from "node:fs/promises";
import path from "node:path";

const abs = path.resolve("notes", "todo.txt"); // OS-correct join
let file;
try {
  file = await open(abs, "r");
  const stat = await file.stat(); // size, mtime, mode, …
  console.log(stat.size);
} finally {
  await file?.close();
}
```

#### Common pitfalls

Смешивание `/` и `\` вручную вместо `path.join`; принятие имен без учета регистра в Linux CI; уход `fs.open` ФДС открыт; лечение `.jpg` в качестве гарантии JPEG; Недопустимые символы Windows в генерируемых именах файлов.

**Pluses and minuses:**

| +                                                 | −                                                           |
| ------------------------------------------------- | ----------------------------------------------------------- |
| Иерархия + путь = уникальные имена                | Разделители путей и регистр различаются в зависимости от ОС |
| Unix «все есть файл» объединяет API               | Должен закрыть fds или вызвать утечку до сбоя               |
| Относительные деревья проектов легко перемещаются | Буферизация может задержать длительную запись               |

**Thoughts:** Файл представляет собой именованный поток байтов + метаданные; файловая система — это дерево (и API), которые делают этот поток переносимым между дисками.

**Sources:** [Doka — файлы и файловые системы](https://doka.guide/tools/file/) · [nodejs.org fs — дескрипторы файлов](https://nodejs.org/api/fs.html#file-descriptors) · [FHS](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)

</details>

<details>
  <summary>fs (file system)</summary>

### fs (file system)

**Definition:**

Встроенный модуль для чтения, записи и управления файлами и каталогами. Модель ОС (пути, атрибуты, буферизация, fds): [Файлы и файловые системы](#files--file-systems).

| Стиль API        | Методы                                        |
| ---------------- | --------------------------------------------- |
| Обратный звонок  | `fs.readFile`, `fs.writeFile`, `fs.mkdir`     |
| Обещание         | `fs.promises.readFile`                        |
| Синхронизировать | `fs.readFileSync` (блокирует цикл событий)    |
| Поток            | `fs.createReadStream`, `fs.createWriteStream` |

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const data = await readFile("./config.json", "utf8");
await writeFile("./out.txt", data);
```

**Sources:** [nodejs.org fs](https://nodejs.org/api/fs.html)

</details>

<details>
  <summary>os</summary>

### os

**Definition:**

Служебный модуль операционной системы — информация о платформе, памяти, процессорах, сетевых интерфейсах.

| Метод                            | Возврат                          |
| -------------------------------- | -------------------------------- |
| `os.platform()`                  | `'darwin'`, `'linux'`, `'win32'` |
| `os.arch()`                      | `'x64'`, `'arm64'`               |
| `os.totalmem()` / `os.freemem()` | память в байтах                  |
| `os.cpus()`                      | Информация о ядре процессора     |
| `os.networkInterfaces()`         | IP/MAC-адреса                    |

**Examples:**

```js
import os from "node:os";
console.log(os.hostname(), os.freemem());
```

**Sources:** [nodejs.org os](https://nodejs.org/api/os.html)

</details>

<details>
  <summary>path</summary>

### path

**Definition:**

Кроссплатформенные утилиты для манипулирования путями к файлам и каталогам. Абсолютное против относительного, `/` vs `\`: [Файлы и файловые системы](#files--file-systems).

| Метод               | Цель                 |
| ------------------- | -------------------- |
| `path.join(...)`    | объединить сегменты  |
| `path.resolve(...)` | абсолютный путь      |
| `path.dirname(p)`   | родительский каталог |
| `path.basename(p)`  | имя файла            |
| `path.extname(p)`   | расширение           |

**Examples:**

```js
import path from "node:path";

const file = path.join(__dirname, "data", "users.json");
const ext = path.extname(file); // '.json'
```

**Sources:** [путь к nodejs.org](https://nodejs.org/api/path.html)

</details>

<details>
  <summary>http</summary>

### http

**Definition:**

Основной модуль для создания HTTP-серверов и клиентов без фреймворка.

| Возможность | API                                     |
| ----------- | --------------------------------------- |
| Сервер      | `http.createServer((req, res) => {})`   |
| Запрос      | `http.request(options, callback)`       |
| Ответ       | `res.writeHead`, `res.write`, `res.end` |

**Examples:**

```js
import { createServer } from "node:http";

createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "application/json" });
  res.end(JSON.stringify({ ok: true }));
}).listen(8080);
```

**Sources:** [nodejs.org http](https://nodejs.org/api/http.html) · Концепция: [Веб-сервер](./6_web_api_http.md#web-server)

</details>

<details>
  <summary>URL & URI</summary>

### URL & URI

**Definition:**

`URL` class (WHATWG) анализирует и создает URL-адреса. Наследие `url` модуль еще доступен; предпочитать `new URL()` для современного кода. Части адреса (схема, полномочия, запрос, хеш): [URI, URL и URN](./6_web_api_core.md#uri-url--urn). Помощники браузера: [URL / URLSearchParams](./3_js_browser_extra.md#url--urlsearchparams).

| Метод/API                       | Цель                        |
| ------------------------------- | --------------------------- |
| `new URL(str, base)`            | разобрать URL на компоненты |
| `url.pathname`, `.searchParams` | части доступа               |
| `url.href`                      | полная строка URL           |

**Examples:**

```js
const url = new URL("https://api.example.com/users?page=1");
console.log(url.hostname); // api.example.com
console.log(url.searchParams.get("page")); // '1'
```

**Sources:** [URL-адрес nodejs.org](https://nodejs.org/api/url.html)

</details>

<details>
  <summary>winston (logging)</summary>

### winston (logging)

**Definition:**

Популярная библиотека журналирования с несколькими транспортами, уровнями и пользовательскими форматами.

| Особенность | Описание                                               |
| ----------- | ------------------------------------------------------ |
| Уровни      | ошибка, предупреждение, информация, подробный, отладка |
| Транспорт   | консоль, файл, база данных                             |
| Форматы     | временные метки, JSON, пользовательские                |

**Examples:**

```js
import winston from "winston";

const logger = winston.createLogger({
  level: "info",
  transports: [new winston.transports.Console()],
});
logger.info("Server started");
```

**Sources:** [github.com/winstonjs/winston](https://github.com/winstonjs/winston)

</details>

<details>
  <summary>cluster</summary>

### cluster

**Definition:**

Создает несколько рабочих процессов Node, использующих один и тот же порт сервера — задействует все ядра ЦП.

| Роль    | Описание                                     |
| ------- | -------------------------------------------- |
| Мастер  | разъединяет рабочих, распределяет соединения |
| Рабочий | обрабатывает запросы на общий порт           |

**Examples:**

```js
import cluster from "node:cluster";
import { createServer } from "node:http";

if (cluster.isPrimary) {
  for (let i = 0; i < 4; i++) cluster.fork();
} else {
  createServer((req, res) => res.end("ok")).listen(3000);
}
```

**Sources:** [кластер nodejs.org](https://nodejs.org/api/cluster.html)

</details>
