# Node.js — Modules

Package managers, CommonJS/ESM, and built-in modules (`fs`, `path`, `http`, and related APIs).

[← Back to hub](./8_nodejs.md)

### Related docs

- [Node.js — Core](./8_nodejs_core.md) · [Node.js — Modules](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Additional](./8_nodejs_process.md) · [Node.js — Interview](./8_nodejs_interview.md)

### Main concepts

| Area               | Topics                                           |
| ------------------ | ------------------------------------------------ |
| **Packages**       | npm, yarn, pnpm, package.json, project structure |
| **Module systems** | CommonJS `require`, ESM `import`/`export`        |
| **Built-ins**      | libuv, fs, path, os, http, URL, cluster, winston |

### Sections

- [Modules & package management](#modules-package-management)
- [Built-in modules](#built-in-modules)

---

## Modules & package management

<details>
  <summary>npm, npx & nvm</summary>

### npm, npx & nvm

**Definition:**

Tooling for installing packages, running binaries, and managing Node versions.

| Tool  | Purpose                                     |
| ----- | ------------------------------------------- |
| `npm` | package manager — install, publish, scripts |
| `npx` | run package binaries without global install |
| `nvm` | switch between Node.js versions per project |

**How it works:** `MAJOR.MINOR.PATCH` — breaking / feature / fix.

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

Package managers for JavaScript. All use the npm registry; differ in lockfile format, install speed, and dependency layout.

|                   | npm                | Yarn               | pnpm                                  |
| ----------------- | ------------------ | ------------------ | ------------------------------------- |
| Lock file         | package-lock.json  | yarn.lock          | pnpm-lock.yaml                        |
| Install speed     | good               | fast (parallel)    | very fast (content-addressable store) |
| Disk usage        | per-project copies | per-project copies | shared store, hard links              |
| Default with Node | yes                | no                 | no                                    |

**When to use:** stick with team/project convention; pnpm for monorepos and disk efficiency.

**Sources:** [pnpm.io](https://pnpm.io/) · [yarnpkg.com](https://yarnpkg.com/)

</details>

<details>
  <summary>Dependencies & package.json</summary>

### Dependencies & package.json

**Definition:**

`package.json` describes the project; `node_modules` holds installed packages; `package-lock.json` pins exact versions for reproducible installs.

| Field / file        | Role                    |
| ------------------- | ----------------------- |
| `dependencies`      | required at runtime     |
| `devDependencies`   | build, test, lint only  |
| `node_modules`      | installed package tree  |
| `package-lock.json` | locked dependency graph |

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

A package is a distributable unit of code (library, module, or app). `require()` (CommonJS) or `import` (ESM) loads modules; metadata lives in `package.json`.

| System   | Syntax                     | Notes                              |
| -------- | -------------------------- | ---------------------------------- |
| CommonJS | `const fs = require('fs')` | synchronous load, `module.exports` |
| ESM      | `import fs from 'fs'`      | static analysis, `export`          |

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

**Sources:** [nodejs.org modules](https://nodejs.org/api/modules.html) · [nodejs.org esm](https://nodejs.org/api/esm.html)

</details>

<details>
  <summary>ESM vs CommonJS (deep dive)</summary>

### ESM vs CommonJS (deep dive)

**Definition:** Node supports two module systems. **CommonJS (CJS)** uses `require`/`module.exports` (synchronous load). **ESM** uses `import`/`export` (static analysis, async load).

**How it works:**

| Aspect                     | CommonJS                      | ESM                                              |
| -------------------------- | ----------------------------- | ------------------------------------------------ |
| Syntax                     | `require()`, `module.exports` | `import`, `export`                               |
| Loading                    | Synchronous, runtime          | Asynchronous, parsed before run                  |
| `this` at top level        | `module.exports`              | `undefined` (strict)                             |
| `__dirname` / `__filename` | Available                     | Use `import.meta.url`                            |
| JSON import                | `require('./x.json')`         | `import x from './x.json' with { type: 'json' }` |
| Dynamic import             | `require(dynamic)`            | `await import(dynamic)`                          |
| Tree-shaking               | Limited                       | Better (static imports)                          |
| Interop                    | ESM can `import` CJS default  | CJS `require()` of ESM only if ESM is async      |

```json
{ "type": "module" }
```

or `.mjs` extension. `.cjs` forces CommonJS in `"type": "module"` projects.

`import fs from 'fs'` — CJS module's `module.exports` becomes default export. Named exports from CJS may need `import pkg from 'cjs'; const { x } = pkg`.

**When to use:** New projects → ESM (`"type": "module"`). Legacy libs → CJS. Dual packages publish both `main` and `exports`.

**Pluses and minuses:**

| + (ESM)                              | − (ESM)                       |
| ------------------------------------ | ----------------------------- |
| Standard in browsers and modern Node | Ecosystem still migrating     |
| Top-level `await`                    | `__dirname` workaround needed |
| Static analysis, tree-shaking        | Some tools expect CJS         |

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

**Thoughts:** `require` is not defined in native ESM. Use `createRequire(import.meta.url)` from `node:module` for legacy `require` in ESM files.

**Sources:** [nodejs.org ESM](https://nodejs.org/api/esm.html) · [nodejs.org modules](https://nodejs.org/api/modules.html)

</details>

<details>
  <summary>CLI commands & flags</summary>

### CLI commands & flags

**Definition:**

Common shell and npm commands for Node.js development.

| Command              | Purpose                                |
| -------------------- | -------------------------------------- |
| `npm i`              | install dependencies from package.json |
| `npm install -S pkg` | add runtime dependency                 |
| `npm install -D pkg` | add dev dependency                     |
| `npm publish`        | publish package to registry            |
| `ls` / `ls -la`      | list files (include hidden with `-a`)  |
| `cat file`           | print file contents                    |
| `touch file`         | create empty file                      |
| `rm`                 | remove files/directories               |
| `pwd`                | print working directory                |
| `chmod +x file`      | make file executable                   |
| `history`            | list previous shell commands           |

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

**Definition:** Organizing Node.js projects for maintainability — separate routes, controllers, services, and config.

**How it works:**

| Pattern            | Structure                                                                                                                   |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| MVC                | `routes/` → `controllers/` → `models/`                                                                                      |
| Feature-based      | `features/users/`, `features/auth/`                                                                                         |
| Clean architecture | `domain/`, `use-cases/`, `infrastructure/` — [three-layer notes](./7_special_architecture.md#three-layer-architecture-doka) |

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

**Sources:** [nodejs.org best practices](https://github.com/goldbergyoni/nodebestpractices)

</details>

---

<a name="built-in-modules"></a>

## Built-in modules

<details>
  <summary>libuv</summary>

### libuv

**Definition:** Cross-platform C I/O library under Node — not a `node:` module you import.

Canonical write-up: [libuv](./8_nodejs_core.md#libuv) in **Node.js — Core** (event loop, thread pool, `UV_THREADPOOL_SIZE`, network vs file I/O).

**Sources:** [libuv design](https://docs.libuv.org/en/v1.x/design.html) · [Node.js — Core: libuv](./8_nodejs_core.md#libuv)

</details>

<details>
  <summary>Files & file systems</summary>

### Files & file systems

**Definition:** A **file** is a named byte sequence stored on a physical medium, plus **attributes** that tell the OS how to handle it. A **file system** is the OS abstraction that allocates that storage, hides hardware differences, and resolves name collisions.

**How it works:**

- RAM holds working data as bytes. Persistence is a named file on disk/SSD (random access) — historically also tape (sequential records) or punch cards (fixed-length records).
- Session: **open** → read/write/attributes → **close**. The kernel tracks each open file with a numeric **file descriptor** (POSIX) or equivalent (Windows). Node maps both to an `fd` / `FileHandle`.
- **Buffering:** writes often land in an OS buffer first, then flush in the background — the UI can look “saved” while the file on disk is still growing.
- Typical OS APIs: `open`, `close`, `read`/`write` (relative to a cursor), `seek`, `delete`, `rename`, read/write attributes.
- **Unix:** devices, directories, and pipes are special files; programs use the same file APIs. Mount attaches another volume into one tree (`mount` / `umount`).

| Model            | Idea                                          | Where                             |
| ---------------- | --------------------------------------------- | --------------------------------- |
| Flat (one level) | Unique names in a single namespace            | Tiny devices (appliance firmware) |
| Fixed depth      | A few hardcoded levels                        | Routers, TVs                      |
| Hierarchical     | Tree: **root dir**, dirs, **files as leaves** | Desktops, servers, phones         |

Collision strategy: forbid duplicate names **in the same directory**. Same basename in different dirs is unique via the **full path**. Separators: `/` (Unix) vs `\` (Windows). Relative names concatenate onto the current (or home) directory.

Unix FS strengths: simple permission model, symbolic links, dirs and devices as files, one mountable namespace.

- **Absolute** = from the root (`/` or `C:\…`). **Relative** = from the working directory. Relatives stay portable inside a project folder; the OS still converts them to absolute on access.
- **`.`** = current dir, **`..`** = parent, **`~`** = home (usually `/home/user`).
- Unix names are **case-sensitive** (`File1` ≠ `file1`). Windows NTFS usually **preserves case but matches case-insensitively**. Unix forbids `/` and `\0` in names.
- Windows reserved in names: `\ / : * ? " < > |`. Spaces need quotes. Legacy **MAX_PATH ≈ 260**; Windows 10+ can lift it, older clients may still fail.
- **Extension** (`.png`, `.exe`) is a hint for users and default apps. The real type lives in attributes / magic bytes. Unix binaries often have **no** extension; `.sh` / `.conf` are convention.

| Dir              | Role                               |
| ---------------- | ---------------------------------- |
| `/bin`, `/sbin`  | Essential user / system binaries   |
| `/boot`          | Bootloader + kernel images         |
| `/dev`           | Device files                       |
| `/etc`           | Host-specific config               |
| `/home`, `/root` | User homes; root’s home            |
| `/lib`           | Shared libs, kernel modules        |
| `/mnt`, `/opt`   | Temp mounts; extra packages        |
| `/proc`, `/sys`  | Kernel / sysfs virtual files       |
| `/tmp`           | Temporary                          |
| `/usr`           | Second-level hierarchy (shareable) |
| `/var`           | Variable data (logs, spool)        |

**When to use:**

Node `fs` / `path`; Docker [bind mounts](./10_build_docker.md#bind-mounts); SPA address bar vs real files ([URI](./6_web_api_core.md#uri-url--urn), History API); browser user files ([File API](./3_js_browser_extra.md#formdata-file-api-filereader)).

**Example** (Node — open / stat / close; prefer `FileHandle` so the fd is not leaked):

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

Mixing `/` and `\` by hand instead of `path.join`; assuming case-insensitive names on Linux CI; leaving `fs.open` fds open; treating `.jpg` as a guarantee of JPEG; Windows illegal chars in generated filenames.

**Pluses and minuses:**

| +                                        | −                                     |
| ---------------------------------------- | ------------------------------------- |
| Hierarchy + path = unique names          | Path separators and case differ by OS |
| Unix “everything is a file” unifies APIs | Must close fds or leak until crash    |
| Relative project trees move easily       | Buffering can delay durable write     |

**Thoughts:** A file is a named byte stream + metadata; the file system is the tree (and APIs) that make that stream portable across disks.

**Sources:** [Doka — files & file systems](https://doka.guide/tools/file/) · [nodejs.org fs — file descriptors](https://nodejs.org/api/fs.html#file-descriptors) · [FHS](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)

</details>

<details>
  <summary>fs (file system)</summary>

### fs (file system)

**Definition:**

Built-in module for reading, writing, and managing files and directories. OS model (paths, attributes, buffering, fds): [Files & file systems](#files--file-systems).

| API style | Methods                                       |
| --------- | --------------------------------------------- |
| Callback  | `fs.readFile`, `fs.writeFile`, `fs.mkdir`     |
| Promise   | `fs.promises.readFile`                        |
| Sync      | `fs.readFileSync` (blocks event loop)         |
| Stream    | `fs.createReadStream`, `fs.createWriteStream` |

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

Operating system utility module — platform info, memory, CPUs, network interfaces.

| Method                           | Returns                          |
| -------------------------------- | -------------------------------- |
| `os.platform()`                  | `'darwin'`, `'linux'`, `'win32'` |
| `os.arch()`                      | `'x64'`, `'arm64'`               |
| `os.totalmem()` / `os.freemem()` | memory in bytes                  |
| `os.cpus()`                      | CPU core info                    |
| `os.networkInterfaces()`         | IP/MAC addresses                 |

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

Cross-platform utilities for file and directory path manipulation. Absolute vs relative, `/` vs `\`: [Files & file systems](#files--file-systems).

| Method              | Purpose          |
| ------------------- | ---------------- |
| `path.join(...)`    | join segments    |
| `path.resolve(...)` | absolute path    |
| `path.dirname(p)`   | parent directory |
| `path.basename(p)`  | filename         |
| `path.extname(p)`   | extension        |

**Examples:**

```js
import path from "node:path";

const file = path.join(__dirname, "data", "users.json");
const ext = path.extname(file); // '.json'
```

**Sources:** [nodejs.org path](https://nodejs.org/api/path.html)

</details>

<details>
  <summary>http</summary>

### http

**Definition:**

Core module for creating HTTP servers and clients without a framework.

| Capability | API                                     |
| ---------- | --------------------------------------- |
| Server     | `http.createServer((req, res) => {})`   |
| Request    | `http.request(options, callback)`       |
| Response   | `res.writeHead`, `res.write`, `res.end` |

**Examples:**

```js
import { createServer } from "node:http";

createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "application/json" });
  res.end(JSON.stringify({ ok: true }));
}).listen(8080);
```

**Sources:** [nodejs.org http](https://nodejs.org/api/http.html) · Concept: [Web server](./6_web_api_http.md#web-server)

</details>

<details>
  <summary>URL & URI</summary>

### URL & URI

**Definition:**

`URL` class (WHATWG) parses and builds URLs. Legacy `url` module still available; prefer `new URL()` for modern code. Address parts (scheme, authority, query, hash): [URI, URL & URN](./6_web_api_core.md#uri-url--urn). Browser helpers: [URL / URLSearchParams](./3_js_browser_extra.md#url--urlsearchparams).

| Method / API                    | Purpose                   |
| ------------------------------- | ------------------------- |
| `new URL(str, base)`            | parse URL into components |
| `url.pathname`, `.searchParams` | access parts              |
| `url.href`                      | full URL string           |

**Examples:**

```js
const url = new URL("https://api.example.com/users?page=1");
console.log(url.hostname); // api.example.com
console.log(url.searchParams.get("page")); // '1'
```

**Sources:** [nodejs.org url](https://nodejs.org/api/url.html)

</details>

<details>
  <summary>winston (logging)</summary>

### winston (logging)

**Definition:**

Popular logging library with multiple transports, levels, and custom formats.

| Feature    | Description                       |
| ---------- | --------------------------------- |
| Levels     | error, warn, info, verbose, debug |
| Transports | console, file, database           |
| Formats    | timestamps, JSON, custom          |

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

Spawns multiple Node worker processes sharing the same server port — utilizes all CPU cores.

| Role   | Description                            |
| ------ | -------------------------------------- |
| Master | forks workers, distributes connections |
| Worker | handles requests on shared port        |

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

**Sources:** [nodejs.org cluster](https://nodejs.org/api/cluster.html)

</details>
