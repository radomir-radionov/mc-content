# Node.js — Additional

child_process, кластер, рабочие и серверы реального времени.

[ ← Вернуться в хаб](./8_nodejs.md)

### Related docs

* [Node.js — Core](./8_nodejs_core.md) · [Node.js — Модули](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Дополнительно](./8_nodejs_process.md) · [Node.js — Интервью](./8_nodejs_interview.md)

### Sections

- [Process, cluster & real-time](#process-cluster-real-time)

---

## Process, cluster & real-time

<details>
  <summary>Process & threads</summary>

### Process & threads

**Definition:**

Процесс — это работающая программа с собственным изолированным виртуальным адресным пространством (PID). Поток — это единица выполнения внутри процесса (обычно это собственный стек). Основной JS Node работает в одном потоке; libuv использует пул потоков для ввода-вывода. Планирование ОС, контекст, прерывания, сигналы POSIX: [Процессы, многозадачность и прерывания](./0_system_design_core.md#processes-multitasking).Стек против кучи внутри этого пространства: [Память: стек и куча](./3_js_data_types.md#memory-stack-heap).

| Концепция | Node.js |
|---------|---------|
| Основной процесс | один экземпляр V8, один цикл событий |
| Дочерний процесс | отдельная память, собственный V8 |
| Рабочий поток | опция общей памяти, отдельный контекст JS |

**Sources:** [процесс nodejs.org](https://nodejs.org/api/process.html)

</details>

<details>
  <summary>child_process</summary>

### child_process

**Definition:**

Модуль для создания дочерних процессов и связи через stdio или IPC.

| Метод | Вариант использования |
|--------|----------|
| `spawn(cmd, args)` | потоковый ввод-вывод, длительные команды |
| `exec(cmd)` | команда оболочки, буферизованный вывод |
| `execFile(file, args)` | запустить исполняемый файл напрямую (без оболочки) |
| `fork(modulePath)` | Spawn Node-модуль с каналом IPC |

**How it works:** `fork` специализируется `spawn` для узла с `process.send` IPC — идеален для работников кластера.

**Examples:**

```js
import { spawn, fork } from 'node:child_process';

const ls = spawn('ls', ['-la']);
ls.stdout.on('data', (data) => console.log(data.toString()));

const worker = fork('./worker.js');
worker.send({ task: 'compute' });
```

**Sources:** [nodejs.org child_process](https://nodejs.org/api/child_process.html)

</details>

<details>
  <summary>Socket.IO</summary>

### Socket.IO

**Definition:**

Библиотека для двунаправленной связи на основе событий с малой задержкой между клиентом и сервером через WebSocket (с резервными вариантами).

| Слой | Роль |
|-------|------|
| Двигатель.ИО | низкоуровневое соединение, транспортные переговоры |
| Сокет.ИО | мероприятия высокого уровня, помещения, пространства имён |

**How it works:** отписаться в `useEffect` return, чтобы избежать дублирования прослушивателей при повторном подключении.

**Examples:**

```js
// server
import { Server } from 'socket.io';
const io = new Server(httpServer);
io.on('connection', (socket) => {
  socket.on('message', (msg) => io.emit('message', msg));
});

// client
import { io } from 'socket.io-client';
const socket = io();
socket.emit('message', 'hello');
```

**Sources:** [socket.io](https://socket.io/docs/v4/)

</details>

<details>
  <summary>grep & shell tools</summary>

### grep & shell tools

**Definition:** `grep` ищет текст в файлах/потоках. `ripgrep` (`rg`) — более быстрая альтернатива.

**Examples:**

```bash
grep "error" app.log
docker ps | grep redis
rg "createUser" src/
```

**Sources:** [gnu.org grep](https://www.gnu.org/software/grep/)

</details>

<details>
  <summary>Linux basics</summary>

### Linux basics

**Definition:**

Основные компоненты ОС, относящиеся к развертыванию Node.

| Компонент | Роль |
|-----------|------|
| Загрузчик | запускает систему |
| Ядро | управляет процессором, памятью, устройствами |
| Система инициализации | запускает демонов |
| Демоны | справочные услуги |
| Оболочка (CLI) | интерфейс командной строки |

**How it works:**

переносимый стандарт Unix. Node и большинство серверных инструментов предназначены для POSIX-подобных систем.

именованные потоки байтов, иерархические каталоги, FHS (`/etc`, `/var`, …): [Файлы и файловые системы](./8_nodejs_modules.md#files--file-systems).

`cp`, `ps`, `cat` (Линукс) / `type` (Эквивалент Windows для отображения файлов). ПИД, рабочие места, `nice`, `top`/`htop`, `kill`: [Управление процессами Unix](./15_devops.md#unix-process-management).

**Sources:** [linuxfoundation.org](https://www.linuxfoundation.org/)

</details>

<details>
  <summary>sudo</summary>

### sudo

**Definition:** Запускает команды с повышенными привилегиями (суперпользователя) в Unix-подобных системах. В Windows используйте `gsudo` для аналогичного UX.

**Examples:**

```bash
sudo npm install -g pm2   # avoid global npm with sudo when possible
```

**Sources:** [sudo.ws](https://www.sudo.ws/)

</details>

<details>
  <summary>Graceful shutdown & SIGTERM</summary>

### Graceful shutdown & SIGTERM

**Definition:** Грациозное завершение работы прекращает прием новой работы, завершает текущие запросы, закрывает соединения и ресурсы, а затем завершает работу — инициируется **SIGTERM** (Docker/K8s) или **SIGINT** (Ctrl+C).

**How it works:**

1. Оркестратор отправляет **SIGTERM** (Kubernetes предоставляет льготный период ~30 секунд).
2. Наборы приложений `server.close()` — перестает принимать новые подключения.
3. Отключите текущие HTTP-запросы и фоновые задания.
4. Закройте пул БД, Redis, потребителей сообщений.
5. `process.exit(0)` или позвольте процессу завершиться естественным путем.

| Сигнал | Источник | Типичное действие |
|--------|--------|----------------|
| СИГТЕРМ | `docker stop`, удаление модуля K8s | Грациозное завершение работы |
| СИГИНТ | Ctrl+C в терминале | Тот же обработчик в dev |
| СИГКИЛЛ | `kill -9` | Не могу поймать — жестко убить |

**When to use:** Развертывание K8 — старый модуль получает SIGTERM; новый модуль получает трафик. ПМ2 `kill_timeout` настраивает льготный период.

**Pluses and minuses:**

| + | − |
|------|------|
| Никаких отброшенных запросов во время развертывания | Должен реализовать; по умолчанию — резкий выход |
| Чистый выпуск ресурсов | Для длинных запросов может потребоваться таймаут + принудительный выход |

**Examples:**

```js
import { createServer } from 'node:http';

const server = createServer(app);
const dbPool = await createPool();

server.listen(3000);

function shutdown(signal) {
  console.log(`${signal} received — closing server`);
  server.close(async () => {
    await dbPool.end();
    console.log('clean exit');
    process.exit(0);
  });
  // Force exit if drain takes too long
  setTimeout(() => {
    console.error('forced shutdown');
    process.exit(1);
  }, 25_000).unref();
}

process.on('SIGTERM', () => shutdown('SIGTERM'));
process.on('SIGINT', () => shutdown('SIGINT'));
```

**Thoughts:** `server.close()` ждет существующих соединений — используйте `closeIdleConnections()` (Node 18+) или отслеживать активные запросы. Проверка работоспособности должна завершиться неудачей при завершении работы, поэтому балансировщик нагрузки перестанет отправлять трафик. Из оболочки: `kill PID` это СИГТЕРМ; `kill -9` это SIGKILL — [Управление процессами Unix](./15_devops.md#unix-process-management).

**Sources:** [сигналы процесса nodejs.org](https://nodejs.org/api/process.html#signal-events) · [завершение модуля Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-termination)

</details>

---

<a name="koa"></a>
