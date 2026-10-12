# Node.js — Additional

child_process, cluster, workers, and real-time servers.

[← Back to hub](./8_nodejs.md)

### Related docs

* [Node.js — Core](./8_nodejs_core.md) · [Node.js — Modules](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Additional](./8_nodejs_process.md) · [Node.js — Interview](./8_nodejs_interview.md)

### Sections

- [Process, cluster & real-time](#process-cluster-real-time)

---

## Process, cluster & real-time

<details>
  <summary>Process & threads</summary>

### Process & threads

**Definition:**

A process is a running program with its own isolated virtual address space (PID). A thread is a unit of execution within a process (typically its own stack). Node's main JS runs on one thread; libuv uses a thread pool for I/O. OS scheduling, context, interrupts, POSIX signals: [Processes, multitasking & interrupts](./0_system_design_core.md#processes-multitasking). Stack vs heap inside that space: [Memory: stack and heap](./3_js_data_types.md#memory-stack-heap).

| Concept | Node.js |
|---------|---------|
| Main process | one V8 instance, one event loop |
| Child process | separate memory, own V8 |
| Worker thread | shared memory option, separate JS context |

**Sources:** [nodejs.org process](https://nodejs.org/api/process.html)

</details>

<details>
  <summary>child_process</summary>

### child_process

**Definition:**

Module to spawn child processes and communicate via stdio or IPC.

| Method | Use case |
|--------|----------|
| `spawn(cmd, args)` | streaming I/O, long-running commands |
| `exec(cmd)` | shell command, buffered output |
| `execFile(file, args)` | run executable directly (no shell) |
| `fork(modulePath)` | spawn Node module with IPC channel |

**How it works:** `fork` is specialized `spawn` for Node with `process.send` IPC — ideal for cluster workers.

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

Library for low-latency, bidirectional, event-based communication between client and server over WebSocket (with fallbacks).

| Layer | Role |
|-------|------|
| Engine.IO | low-level connection, transport negotiation |
| Socket.IO | high-level events, rooms, namespaces |

**How it works:** unsubscribe in `useEffect` return to avoid duplicate listeners on reconnect.

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

**Definition:** `grep` searches text in files/streams. `ripgrep` (`rg`) is a faster alternative.

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

Core OS components relevant to Node deployment.

| Component | Role |
|-----------|------|
| Boot loader | starts the system |
| Kernel | manages CPU, memory, devices |
| Init system | starts daemons |
| Daemons | background services |
| Shell (CLI) | command-line interface |

**How it works:**

portable Unix standard — Node and most server tools target POSIX-like systems.

named byte streams, hierarchical dirs, FHS (`/etc`, `/var`, …): [Files & file systems](./8_nodejs_modules.md#files--file-systems).

`cp`, `ps`, `cat` (Linux) / `type` (Windows equivalent for file display). PID, jobs, `nice`, `top`/`htop`, `kill`: [Unix process management](./15_devops.md#unix-process-management).

**Sources:** [linuxfoundation.org](https://www.linuxfoundation.org/)

</details>

<details>
  <summary>sudo</summary>

### sudo

**Definition:** Runs commands with elevated privileges (superuser) on Unix-like systems. On Windows, use `gsudo` for similar UX.

**Examples:**

```bash
sudo npm install -g pm2   # avoid global npm with sudo when possible
```

**Sources:** [sudo.ws](https://www.sudo.ws/)

</details>

<details>
  <summary>Graceful shutdown & SIGTERM</summary>

### Graceful shutdown & SIGTERM

**Definition:** Graceful shutdown stops accepting new work, finishes in-flight requests, closes connections and resources, then exits — triggered by **SIGTERM** (Docker/K8s) or **SIGINT** (Ctrl+C).

**How it works:**

1. Orchestrator sends **SIGTERM** (Kubernetes gives ~30s grace period).
2. App sets `server.close()` — stops accepting new connections.
3. Drain in-flight HTTP requests and background jobs.
4. Close DB pool, Redis, message consumers.
5. `process.exit(0)` or let process exit naturally.

| Signal | Source | Typical action |
|--------|--------|----------------|
| SIGTERM | `docker stop`, K8s pod delete | Graceful shutdown |
| SIGINT | Ctrl+C in terminal | Same handler in dev |
| SIGKILL | `kill -9` | Cannot catch — hard kill |

**When to use:** K8s rolling deploy — old pod gets SIGTERM; new pod receives traffic. PM2 `kill_timeout` configures grace period.

**Pluses and minuses:**

| + | − |
|------|------|
| No dropped requests during deploy | Must implement; default is abrupt exit |
| Clean resource release | Long requests may need timeout + force exit |

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

**Thoughts:** `server.close()` waits for existing connections — use `closeIdleConnections()` (Node 18+) or track active requests. Health check should fail when shutting down so load balancer stops sending traffic. From the shell: `kill PID` is SIGTERM; `kill -9` is SIGKILL — [Unix process management](./15_devops.md#unix-process-management).

**Sources:** [nodejs.org process signals](https://nodejs.org/api/process.html#signal-events) · [Kubernetes pod termination](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-termination)

</details>

---

<a name="koa"></a>
