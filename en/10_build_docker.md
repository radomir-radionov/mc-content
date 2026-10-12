# Build — Docker

Images, containers, Compose, and container lifecycle.

[← Back to hub](./10_build.md)

### Related docs

- [Build — CI](./10_build_ci.md) · [Build — Bundlers](./10_build_bundlers.md) · [Build — Docker](./10_build_docker.md) · [Build — Infra](./10_build_infra.md) · [Build — Interview](./10_build_interview.md)

### Sections

- [Docker](#docker)

---

## Docker

<details>
  <summary>Docker / containerization</summary>

### Docker / containerization

**Definition:** Platform that packages an app **with its environment** (OS bits, runtime, deps, config) into a **container** so it runs the same on any Docker host — “if it runs here, it runs anywhere.”

**How it works:**

- **Docker Engine** is a client–server stack: long-running daemon `dockerd` + **Engine API** + CLI `docker`. The machine running the daemon is the **Docker host**. CLI/Dashboard talk to the daemon via the API; the daemon creates images, containers, networks, volumes.
- **Docker Desktop** (Mac/Windows, optional on Linux) bundles Engine, CLI, Compose, Dashboard, Hub login. On Mac/Windows, Linux containers run inside a **Linux VM** — you do not run `dockerd` natively on the host kernel.
- `docker run` typically: pull image (if missing) → create container → attach a writable layer + network → start PID 1. Image ≈ class; container ≈ instance.
- Check install: `docker --version`. First tutorial image from current docs:

```bash
docker pull docker/welcome-to-docker
docker run --detach --name welcome --publish 8080:80 docker/welcome-to-docker
# then http://localhost:8080  —  docker rm --force welcome
```

**When to use:**

Server deploy; CI images; frontend — bundle/lint/test, asset pipelines, try a CMS/framework without installing Node/DB/nginx on the host, reproducible demos, editor + toolchains in a container.

#### Common pitfalls

Binding the app to `127.0.0.1` inside the container (host cannot reach it — listen on `0.0.0.0`). Treating `EXPOSE` as publish (`-p` still required). Letting stopped containers pile up (`docker ps -a`) until names collide.

**Pluses and minuses:**

| +                                             | −                                                                      |
| --------------------------------------------- | ---------------------------------------------------------------------- |
| Same env on laptop, CI, prod                  | Networking, volumes, and Desktop VM have a learning curve              |
| Seconds to start vs a full VM                 | Shared kernel — not a VM; native modules must match **container** arch |
| One process per container; Compose for stacks | Image bloat without multi-stage + `.dockerignore`                      |
| Recipe in git (Dockerfile)                    | Harder to debug than a local process                                   |

**Examples:**

```bash
docker run --name some-wordpress -p 8080:80 -d wordpress
docker run --name some-drupal -p 8080:80 -d drupal
# bind-mount host folder, then run ImageMagick in the container:
docker run --rm -v "$PWD":/imgs dpokidov/imagemagick /imgs/in.png -resize 100x100 /imgs/out.png
```

**Thoughts:** Ship the **environment with the app**. Engine = daemon + API + CLI; image is the template, container is the running (or stopped) instance.

**Sources:** [Docker Engine](https://docs.docker.com/engine/) · [Overview](https://docs.docker.com/get-started/docker-overview/) · [Run an app](https://docs.docker.com/get-started/tutorials/run-an-app/)

</details>

<details>
  <summary>Container vs virtual machine</summary>

### Container vs virtual machine

|                    | Container                     | Virtual Machine      |
| ------------------ | ----------------------------- | -------------------- |
| **Isolation**      | Process-level (shared kernel) | Full OS per VM       |
| **Startup**        | Seconds                       | Minutes              |
| **Resource usage** | Lightweight                   | Heavy (full OS)      |
| **Portability**    | High (with arch match)        | High                 |
| **Use case**       | Microservices, CI             | Legacy apps, diff OS |

**Sources:** [Docker — Containers](https://www.docker.com/resources/what-container/)

</details>

<details>
  <summary>Core concepts</summary>

### Core concepts

| Concept          | Definition                                                                                                                                                       |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Image**        | Read-only layered template (union of diffs). Built from Dockerfile or pulled from a registry. Layers are reused across images.                                   |
| **Container**    | Instance of an image: namespaces + cgroups + a **writable** top layer. Ephemeral by default.                                                                     |
| **Volume**       | Docker-managed persistent directory (not a [bind mount](#bind-mounts)). Survives `docker rm`. Details: [Volumes](#volumes).                                      |
| **Network**      | Virtual LAN between containers and/or the host. Compose creates a default user-defined bridge.                                                                   |
| **Registry**     | Remote image store. Default: **Docker Hub**. Also GHCR, ECR, GCR/Artifact Registry, ACR, Yandex CR, etc. Prefer **official** images (`node`, `python`, `nginx`). |
| **Dockerfile**   | Recipe for an image (`FROM` then layers).                                                                                                                        |
| **Compose file** | YAML for a multi-container app.                                                                                                                                  |

**Network drivers (Linux Engine):**

| Driver      | Role                                                                                                        |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| **bridge**  | Default. Isolated network; containers talk by IP (user-defined bridge: also by **name**). Typical app ↔ DB. |
| **host**    | No network namespace — container shares the host’s network (Linux).                                         |
| **none**    | No networking.                                                                                              |
| **overlay** | Multi-host (Swarm). Also **ipvlan** / **macvlan** for physical-looking NICs.                                |

```mermaid
flowchart LR
  dockerfile[Dockerfile] -->|docker build| image[Image]
  registry[Registry] -->|docker pull| image
  image -->|docker run| container[Container]
  volume[Volume] <-->|mount| container
  net[Network] <--> container
```

**Thoughts:** Image layers stack (union FS); only the container’s top layer is writable. Persist data in volumes, wire services with a bridge network.

**Sources:** [Concepts](https://docs.docker.com/get-started/docker-concepts/) · [Networking](https://docs.docker.com/engine/network/)

</details>

<details>
  <summary>Dockerfile</summary>

### Dockerfile

**Definition:** A text file of sequential instructions that Docker Build uses as a recipe for an **image** — OS, runtime, files, env, and the process that starts when a container runs.

**How it works:**

- Format: `# comment` then `INSTRUCTION arguments` (instructions are case-insensitive; convention is UPPERCASE). Optional first line: `# syntax=docker/dockerfile:1` (BuildKit frontend).
- A Dockerfile must start with `FROM` (only global `ARG` / parser directives may precede it). Each `FROM` starts a **build stage**.
- The image is a **union of layers** (stacked diffs). `FROM`, `RUN`, `COPY`, and `ADD` create filesystem layers; other instructions mainly set metadata. Unchanged layers are reused from cache — put rarely changing steps first (`package*.json` + `npm ci` before `COPY . .`).
- Layers can be shared across images; Docker only pulls layers the host does not already have.
- **Context** is the set of files `COPY`/`ADD` can see (usually `.`). Exclude junk with `.dockerignore` so secrets and `node_modules` never enter the context.
- Build: `docker build [OPTIONS] PATH | URL | -`. Then `docker run` starts a **container** (writable layer + process) from the image.
- Official guidance: one concern per container, no unused packages, pin base tags, sort multi-line `RUN` args, use `.dockerignore`, multi-stage for prod, `--no-cache` / `--pull` when you need a clean rebuild (cache is for reuse, not for every layer forever).

`CMD ["node","app.js"]` (JSON array, double quotes) is PID 1 and gets `SIGTERM`. Shell form `CMD node app.js` runs via `/bin/sh -c`, so the shell is PID 1 and the app may miss stop signals.

Treat the image as a binary → `ENTRYPOINT` (exec) + `CMD` as default args. Treat it as a generic box with a default process → `CMD` only (easy to override). `docker run image sh` replaces `CMD`, not exec-form `ENTRYPOINT`.

```dockerfile
ENTRYPOINT ["yaspeller"]
CMD ["."]
# docker run --rm yaspeller README.md  → yaspeller README.md
```

#### Build, extra Dockerfiles, ignore file

```bash
docker build .                              # context = cwd, file = Dockerfile
docker build -t user/app:2.0 .              # name:tag (repo on Hub is user/app)
docker build -f containers/Dockerfile.dev .
docker build -t myapp --build-arg user=deploy .
docker build -f- . < Dockerfile             # Dockerfile from stdin; context still PATH
docker build - < Dockerfile                 # no context — COPY from context fails
# Git / tar URL as context (gzip, bzip2, xz archives):
docker build -f ctx/Dockerfile http://server/ctx.tar.gz
docker image inspect myapp --format '{{.Size}}'
docker history myapp                        # layers; ARG values may show here
```

`.dockerignore` lives at the **context root** (gitignore-like). Typical Node ignore: `node_modules/`, `dist/`, `.git`, `.env`. With `-f myapp.Dockerfile`, BuildKit also checks `myapp.Dockerfile.dockerignore` next to that file, then falls back to `.dockerignore`.

#### Multi-stage

Builder stage has compilers/`npm`; final stage copies only artifacts. Intermediate stages are not in the shipped image.

```dockerfile
# syntax=docker/dockerfile:1
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**When to use:**

Reproducible Node/nginx images; CI artifacts; CLI tools packaged as `ENTRYPOINT`; multi-stage SPA (build in Node, serve with nginx).

#### Instructions

| Instruction   | Role                                                                                                                                                                                 |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `FROM`        | Base image / new stage. Pin a tag (`node:20-alpine`), not bare `node` (`latest`). `AS name` for multi-stage.                                                                         |
| `RUN`         | Shell command **at build time** — install packages, compile. Chain with `&&` in one `RUN` to cut layers.                                                                             |
| `COPY`        | Copy from **build context** into the image. Prefer this. `COPY --from=stage` copies from another stage/image.                                                                        |
| `ADD`         | Like `COPY`, plus remote URLs and auto-extract of local archives — easy to misuse; avoid unless you need that.                                                                       |
| `WORKDIR`     | Working directory for later `RUN`/`COPY`/`CMD`/`ENTRYPOINT`. Created if missing.                                                                                                     |
| `ENV`         | Env vars in the **image** (available at runtime and later build steps).                                                                                                              |
| `ARG`         | **Build-time** vars (`docker build --build-arg KEY=val`). Not in the running container unless you copy into `ENV`. Visible in `docker history` — not for secrets.                    |
| `USER`        | UID/GID for later `RUN`/`CMD`/`ENTRYPOINT`. Create the user first; official `node` image already has `USER node`.                                                                    |
| `EXPOSE`      | **Documents** the listen port. Does **not** publish to the host. Publish with `docker run -p host:container` or Compose `ports`. `-P` maps all `EXPOSE`d ports to random host ports. |
| `VOLUME`      | Declares a mount point (anonymous volume unless you mount something at run). Host path is **not** set in the Dockerfile — [Volumes](#volumes).                                       |
| `CMD`         | Default command. Overridden by args after `docker run IMAGE …`. Only the last `CMD` counts.                                                                                          |
| `ENTRYPOINT`  | Main executable. `docker run` args are **appended** (exec form). Use with `CMD` for default flags.                                                                                   |
| `HEALTHCHECK` | How the daemon probes liveness (`CMD` inside the container).                                                                                                                         |
| `LABEL`       | Image metadata. `STOPSIGNAL` / `SHELL` — stop signal and default shell.                                                                                                              |

#### Common pitfalls

- `EXPOSE 8080` ≠ `http://localhost:8080` — you still need `-p 8080:8080` (and the app must listen on `0.0.0.0`).
- `sudo` in `RUN` — official bases already run as root; `sudo` often is not installed.
- `USER alice` without creating `alice` → build/run fails.
- Baking passwords into `ENV`/`ARG`; use runtime env, Compose secrets, or BuildKit `--secret`, not `docker history`-visible args.
- `COPY . .` without `.dockerignore` (sends `.git`, `node_modules`, `.env`).
- Shell-form `ENTRYPOINT` ignoring `CMD` and `docker run` args; app not PID 1 → `docker stop` waits then `SIGKILL`.
- Unpinned `FROM node` and unnecessary packages (editors in a runtime image).

**Pluses and minuses:**

| +                                     | −                                              |
| ------------------------------------- | ---------------------------------------------- |
| Same runtime on laptop and server     | Bad layer order → slow rebuilds                |
| Cache + multi-stage keep images small | `latest` tags and huge context bloat CI        |
| Recipe is versioned with the app      | Secrets in `ARG`/`ENV` leak into image history |

**Examples:**

```dockerfile
# syntax=docker/dockerfile:1
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
ENV NODE_ENV=production
USER node
EXPOSE 3000
CMD ["node", "app.js"]
```

**Thoughts:** Dockerfile = layered recipe; keep containers **stateless** (data in volumes), one concern per image, slim final stage.

**Sources:** [Dockerfile reference](https://docs.docker.com/reference/dockerfile/) · [Best practices](https://docs.docker.com/build/building/best-practices/) · [Build context](https://docs.docker.com/build/concepts/context/)

</details>

<details>
  <summary>Container lifecycle</summary>

### Container lifecycle

**Definition:** A container is a **running or stopped instance** of an image: namespaces + cgroups + a writable layer. The image stays read-only; PID 1 is usually `ENTRYPOINT` + `CMD`.

**How it works:**

Interview sequence: create → start (running) → optional pause/unpause → stop or kill → restart or remove.

```mermaid
flowchart LR
  image[Image]
  created[Created]
  running[Running]
  paused[Paused]
  stopped[Stopped / Exited]
  gone[Removed]
  image -->|create| created
  image -->|run = create + start| running
  created -->|start| running
  running -->|pause| paused
  paused -->|unpause| running
  running -->|stop SIGTERM then SIGKILL| stopped
  running -->|kill SIGKILL| stopped
  stopped -->|start| running
  running -->|restart = stop + start| running
  stopped -->|rm| gone
  running -->|rm -f| gone
```

| Command                    | Effect                                                    |
| -------------------------- | --------------------------------------------------------- |
| `docker create`            | Allocate from an image; not running                       |
| `docker start`             | Start a created or exited container                       |
| `docker run`               | `create` + `start` (often `--rm` to delete on exit)       |
| `docker pause` / `unpause` | Freeze / resume (cgroup freezer)                          |
| `docker stop`              | `SIGTERM` (or `STOPSIGNAL`), then `SIGKILL` after timeout |
| `docker kill`              | Immediate signal (`SIGKILL` by default)                   |
| `docker restart`           | Stop then start                                           |
| `docker rm`                | Delete a **stopped** container (`-f` kills then removes)  |

Restart policies (`--restart` / Compose `restart:`): `no` (default), `on-failure[:n]`, `always`, `unless-stopped`. Manual `stop` suppresses `always` until you start again. Exited containers stay in `docker ps -a` — a name conflict means an old stopped container still exists.

**When to use:**

Interview “describe the lifecycle”; K8s `SIGTERM` + Node graceful shutdown — [8_nodejs.md — SIGTERM](./15_devops.md#unix-process-management).

#### Common pitfalls

`rm` on a running container without `-f` fails; `pause` ≠ `stop` (process still exists); `docker compose down` keeps named volumes unless `-v`.

**Thoughts:** Image is immutable; the writable layer and process are ephemeral — persist with volumes.

**Sources:** [Running containers](https://docs.docker.com/get-started/docker-concepts/running-containers/) · [docker container CLI](https://docs.docker.com/reference/cli/docker/container/) · [Restart policies](https://docs.docker.com/engine/containers/start-containers-automatically/)

</details>

<details>
  <summary>Docker storage mounts</summary>

### Docker storage mounts

**Definition:** Files written only on the **writable container layer** disappear when that container is removed. A **mount** still looks like a path inside the container, but the bytes live elsewhere.

**How it works:**

| Type                                | Where data lives                                  | Survives `docker rm`?            | Typical use                                |
| ----------------------------------- | ------------------------------------------------- | -------------------------------- | ------------------------------------------ |
| **[Volume](#volumes)**              | Docker-managed dir (or a volume driver)           | Yes, until `volume rm` / `prune` | Databases, shared app data                 |
| **[Bind mount](#bind-mounts)**      | Any host path you choose                          | Yes (host filesystem)            | Dev source, host config                    |
| **[tmpfs](#tmpfs-and-named-pipes)** | Host RAM (Linux)                                  | No                               | Secrets, scratch, hot cache                |
| **Named pipe**                      | Windows npipe                                     | IPC / session                    | Docker Engine API from a Windows container |
| **Image mount**                     | Another image, read-only (containerd image store) | N/A                              | Extra tools/assets without rebuilding      |

From the app’s view every type is just a directory or file. Overlay rule: mounting onto a **non-empty** container path **hides** the image files there (USB into `/mnt`). Exception: an **empty named volume** is **pre-filled** from the image (unless `volume-nocopy`).

**When to use:**

Persist Postgres; live-reload `./src`; keep credentials off the writable layer.

#### Common pitfalls

Calling a bind mount a “volume”; storing prod data only in the writable layer; expecting macOS/Windows binds to be as fast as Linux volumes (Desktop uses a Linux VM).

**Pluses and minuses:**

| +                                  | −                                                           |
| ---------------------------------- | ----------------------------------------------------------- |
| Same path API inside the container | Different overlay, security, and portability rules per type |
| Volumes do not bloat the image     | Writable layer is slow and hard to extract                  |

**Thoughts:** Persist with **volumes**; edit host files with **bind mounts**; keep ephemeral/sensitive scratch in **tmpfs**.

**Sources:** [Docker — Storage](https://docs.docker.com/engine/storage/)

</details>

<details>
  <summary>Bind mounts</summary>

### Bind mounts

**Definition:** A **bind mount** maps a host file or directory into a container. Host processes and the container see the same files; Docker does **not** manage that path (`docker volume ls` will not list it).

**How it works:**

- Prefer `--mount type=bind,src=…,dst=…`. `-v host:container` is shorter.
- `-v`: missing host path is **created as a directory**. `--mount` **errors** unless `bind-create-src`.
- Bind onto a non-empty container path → image files at that path are **hidden** until you recreate without the mount.
- Bind propagation (Linux Engine, not Desktop): default `rprivate`; also `private` / `shared` / `slave` and recursive `r*`.
- SELinux `z` (shared among containers) and `Z` (private) exist only on `-v`. Never use `Z` on `/usr` or `/home`.
- The path is on the **daemon** host. Desktop proxies native paths into the Linux VM.

**When to use:**

Share source / build artifacts; Docker’s own DNS via host `/etc/resolv.conf`; write generated files onto the host.

#### Common pitfalls

Binding over `/usr` and breaking the image; `Z` on `/home`; Windows vs Unix paths; expecting `--mount` to auto-create the host dir.

**Pluses and minuses:**

| +                              | −                                                              |
| ------------------------------ | -------------------------------------------------------------- |
| Instant host ↔ container edits | Host layout must exist; not portable                           |
| Simple local DX                | Container can mutate host files (`ro` / `readonly` to prevent) |
|                                | Slow / missing FS events on Docker Desktop                     |

**Examples:**

```bash
docker run -d -it --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app \
  node:lts

docker run -d -it --name devtest -v "$(pwd)"/target:/app node:lts

docker run --mount type=bind,src="$(pwd)"/target,dst=/app,readonly node:lts
```

```json
"Mounts": [{
  "Type": "bind",
  "Source": "/tmp/source/target",
  "Destination": "/app",
  "RW": true,
  "Propagation": "rprivate"
}]
```

```yaml
services:
  frontend:
    image: node:lts
    volumes:
      - type: bind
        source: ./static
        target: /opt/app/static
```

**Thoughts:** A bind mount is a **host path glued into the container** — right for local source, risky for prod data.

**Sources:** [Docker — Bind mounts](https://docs.docker.com/engine/storage/bind-mounts/)

</details>

<details>
  <summary>Volumes</summary>

### Volumes

**Definition:** **Volumes** are Docker-managed storage. Data lives under the daemon (Linux: `/var/lib/docker/volumes/<name>/_data`) or on a **volume driver**. Editing that host folder by hand is unsupported.

**How it works:**

- **Named** (`my-vol`) vs **anonymous** (Docker IDs them). Named volumes survive `docker run --rm`; anonymous volumes are removed with `--rm`.
- Independent of containers: `docker rm` does not delete a volume. Use `docker volume rm` / `prune`.
- Prefer `--mount type=volume`. `-v my-vol:/app` works. `:ro` / `readonly` for read-only. Propagation is always `rprivate`.
- **Empty** named volume on a path with image files → files are **copied into the volume** (`volume-nocopy` disables). **Non-empty** volume → **hides** image files.
- Dockerfile `VOLUME` only declares a mount point — you cannot set a host path there. Writes **after** `VOLUME` during build: discarded on the **legacy** builder; **kept with BuildKit**.
- Compose top-level `volumes:` creates/attaches named volumes; `external: true` = already exists.

Plugins or `local` + kernel mount options (NFS, CIFS). Driver options require `--mount`, not `-v`. Quote CSV-like `volume-opt` values.

```bash
docker service create -d --name nfs-service \
  --mount 'type=volume,source=nfsvolume,target=/app,volume-driver=local,volume-opt=type=nfs,volume-opt=device=:/var/docker-nfs,volume-opt=o=addr=10.0.0.10' \
  nginx:latest
```

**Backup / restore** (`--volumes-from` reuses another container’s volumes):

```bash
docker run -v /dbdata --name dbstore ubuntu /bin/bash
docker run --rm --volumes-from dbstore -v "$(pwd)":/backup ubuntu tar cvf /backup/backup.tar /dbdata

docker run -v /dbdata --name dbstore2 ubuntu /bin/bash
docker run --rm --volumes-from dbstore2 -v "$(pwd)":/backup ubuntu \
  bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1"
```

**When to use:**

DB data; share one dataset across containers; backup/migrate; high I/O on Docker Desktop (volume on the Linux VM); native Linux FS semantics for databases.

#### Common pitfalls

`compose down -v` deletes named volumes; anonymous volumes from image `VOLUME`; expecting Finder/Explorer UX; `RUN` after `VOLUME` on the legacy builder.

**Pluses and minuses:**

| +                                             | −                                       |
| --------------------------------------------- | --------------------------------------- |
| CLI/API; Linux + Windows containers           | Awkward to browse from the host         |
| Drivers: NFS, remote, extra processing        | Unused volumes pile up until `prune`    |
| Empty volume can be pre-filled from the image | Not for live-editing a host source tree |

**Examples:**

```bash
docker volume create my-vol
docker run -d --name devtest --mount source=my-vol,target=/app node:lts
# docker run -d --name devtest -v my-vol:/app node:lts

docker volume ls
docker volume inspect my-vol
docker container stop devtest && docker container rm devtest
docker volume rm my-vol

docker run --rm -v /foo -v awesome:/bar busybox top  # anonymous /foo dies; awesome stays
docker volume prune
```

Dockerfile (seed **before** `VOLUME` if you still use the legacy builder):

```dockerfile
FROM node:lts
RUN mkdir /data && echo seed > /data/x
VOLUME /data
```

Compose:

```yaml
services:
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
  # myapp:
  #   external: true
```

**Thoughts:** Volumes are the **default production persistence** — Docker owns the bytes; containers only mount a path.

**Sources:** [Docker — Volumes](https://docs.docker.com/engine/storage/volumes/) · [Dockerfile `VOLUME`](https://docs.docker.com/reference/dockerfile/#volume)

</details>

<details>
  <summary>tmpfs and named pipes</summary>

### tmpfs and named pipes

**Definition:** **tmpfs** is an in-memory filesystem on **Linux**. **Named pipes (npipe)** are a Windows IPC mount (often the Docker Engine API). Neither is durable disk storage.

**How it works:**

- tmpfs is **not** shared between containers. Data is gone when the container stops. The kernel may still **swap** tmpfs to disk.
- Usage counts toward the container **memory** limit. A large `tmpfs-size` does not add RAM.
- `--mount type=tmpfs,destination=/app` with `tmpfs-size` (bytes; default cap ~50% of host RAM) and `tmpfs-mode` (octal, default `1777`).
- `--tmpfs /app` is shorter (`size=64m`, `mode=1777`, `noexec`, …). Swarm services need `--mount`.
- Overlay: tmpfs on a non-empty container path **hides** image files.

**When to use:** Keys/certs in memory; lots of non-persistent writes without the writable layer.

**Pluses and minuses:**

| +                                         | −                                  |
| ----------------------------------------- | ---------------------------------- |
| Fast; nothing left on disk by design      | Linux only; one container; can OOM |
| Better than baking secrets into the image | Can still hit swap                 |

**Examples:**

```bash
docker run -d -it --name tmptest \
  --mount type=tmpfs,destination=/app \
  node:lts

docker run -d -it --name tmptest --tmpfs /app node:lts
```

**Thoughts:** tmpfs = **RAM scratch for one Linux container**, not a backup target.

**Sources:** [Docker — tmpfs](https://docs.docker.com/engine/storage/tmpfs/) · [Storage — named pipes](https://docs.docker.com/engine/storage/#named-pipes)

</details>

<details>
  <summary>Docker Compose</summary>

### Docker Compose

**Definition:** Tool for defining and running a **multi-container application** from one YAML file (`compose.yaml`) and the `docker compose` CLI — one service per container, started together as a project.

**How it works:**

- Docker’s recommended model: **one service = one container** (web, DB, cache, reverse proxy). Easier to debug, update, scale, and monitor than stuffing PHP + Nginx + MySQL into a single image.
- A **project** is one deployment of that file. Compose prefixes resources with the project name (`name:` or the directory name) so two stacks can share a machine.
- Compose creates a **default network** unless you declare `networks`. Service names are DNS hostnames (`db`, `api`).
- Prefer `compose.yaml` / `compose.yml` in the project root. `docker-compose.yaml` / `.yml` still work; if both exist, `compose.yaml` wins.
- Docker Desktop (Mac/Windows) ships Compose. Linux: install Compose v2 with Engine. Use **`docker compose`** (v2, Docker CLI plugin). Hyphenated `docker-compose` is the old v1 Python tool.
- Top-level **`version`** is **obsolete** (warning only). Compose always validates against the current Compose Specification schema.
- Other top-level keys: **`services`** (required), **`networks`**, **`volumes`**, **`configs`** (non-secret runtime files, no rebuild), **`secrets`** (sensitive files mounted into containers), **`name`**, **`include`**.

```yaml
services:
  frontend:
    image: example/webapp
    build: ./webapp # context; Dockerfile in that folder

  backend:
    image: example/database
    build:
      context: backend
      dockerfile: ../backend.Dockerfile
      args:
        GIT_COMMIT: cdc3b19 # Dockerfile ARG
```

```bash
docker compose build --build-arg GIT_COMMIT=cdc3b19
```

**Keep a container alive** (debug / wait): override `command`, not the image build:

```yaml
services:
  debug:
    build: .
    env_file: ./.env
    command: /bin/sh -c "while sleep 1000; do :; done"
```

`mode` (`replicated` / `global` / jobs), `replicas`, `resources.limits` / `reservations`, placement, rolling `update_config`. These describe **platform** deployment (Swarm `docker stack deploy`). Compose on a single engine does **not** spawn six replicas just because `deploy.replicas: 6` is in the file. Scale locally with `docker compose up --scale frontend=6`.

**Resources** are a **hard cap** (`limits`) vs a **guarantee** (`reservations`) — they do **not** add up to a “peak”:

```yaml
deploy:
  resources:
    limits:
      cpus: "0.50" # at most 0.5 CPU
      memory: 50M
    reservations:
      cpus: "0.25" # scheduler should keep 0.25 CPU available
      memory: 20M
```

#### Networks

Isolate tiers: frontend on `front-tier` + `back-tier`; database only on `back-tier`. Empty `networks:` entries create project-scoped bridges. `external: true` attaches an existing Docker network.

```yaml
services:
  frontend:
    image: example/webapp
    networks: [front-tier, back-tier]
  backend:
    image: example/database
    networks: [back-tier]

networks:
  front-tier: {}
  back-tier: {}
```

#### Volumes

Named volumes (top-level `volumes:`) persist across `down` (until `down -v`). Bind mounts (`./frontend:/usr/src/app`) sync host code for dev. Anonymous volume e.g. `/usr/src/app/node_modules` hides the host’s `node_modules` so the image’s Linux modules win. Types and overlay rules: [Docker storage mounts](#docker-storage-mounts).

**MERN-style local stack** (bind mounts + two networks):

```yaml
services:
  frontend:
    build: frontend
    ports: ["3000:3000"]
    stdin_open: true
    volumes:
      - ./frontend:/usr/src/app
      - /usr/src/app/node_modules
    restart: always
    networks: [react-express]
    depends_on: [backend]

  backend:
    build: backend
    restart: always
    volumes:
      - ./backend:/usr/src/app
      - /usr/src/app/node_modules
    depends_on: [mongo]
    networks: [express-mongo, react-express]

  mongo:
    image: mongo:7
    restart: always
    volumes:
      - ./data:/data/db
    networks: [express-mongo]

networks:
  react-express:
  express-mongo:
```

**When to use:**

Local full stack (app + DB + Redis), integration tests, small VPS / staging. Production at scale → Kubernetes (or Swarm `docker stack deploy` for `deploy.*` replica/placement semantics). Samples: [Awesome Compose](https://github.com/docker/awesome-compose).

#### Common pitfalls

- `docker-compose` (v1) vs `docker compose` (v2).
- Putting `version: "3.9"` thinking it selects a schema — it is ignored.
- Assuming `deploy.replicas` scales `docker compose up`.
- `depends_on` without `healthcheck` / `condition: service_healthy` — app starts before MySQL accepts connections.
- Binding DB data as a host folder on Docker Desktop can be slower/permission-awkward vs a named volume.

**Pluses and minuses:**

| +                                          | −                                                                |
| ------------------------------------------ | ---------------------------------------------------------------- |
| One command for the whole stack            | Not a cluster scheduler; no auto-scale like K8s                  |
| Declarative, git-friendly YAML             | `depends_on` waits for **start**, not “ready” (use healthchecks) |
| DNS by service name; named volumes persist | `deploy.replicas` is Swarm/stack; local Compose uses `--scale`   |

**Examples:**

```yaml
# compose.yaml
services:
  db:
    image: mysql:8.0
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      MYSQL_ROOT_PASSWORD: somewordpress

  wordpress:
    image: wordpress:latest
    restart: always
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
```

```bash
docker compose up -d          # pull/build, create nets/volumes, start
# site: http://localhost:8000
docker compose down            # stop + remove containers and networks
docker compose down -v         # also remove named volumes (DB data gone)
```

```yaml
services:
  api:
    build:
      context: ./services/api
      dockerfile: Dockerfile.local
    ports:
      - "${HOST_API_PORT:-4000}:4000"
    environment:
      DATABASE_URL: postgres://postgres:secret@database:5432/app
    env_file:
      - ./services/api/.env
    volumes:
      - ./services/api/src:/app/src
    depends_on:
      - database

  database:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: app
    ports:
      - "${HOST_DB_PORT:-5555}:5432"
    volumes:
      - pg_data:/var/lib/postgresql/data

volumes:
  pg_data:
```

#### Services

Each key under `services` is an atomic unit: `image` and/or `build`, `ports`, `environment` / `env_file`, `volumes`, `networks`, `command` (overrides image `CMD` at **runtime**), `restart`, `depends_on`.

**Thoughts:** Compose is the YAML + CLI for a whole app: services on networks, data in volumes, one `up` / `down`.

**Sources:** [Compose application model](https://docs.docker.com/compose/intro/compose-application-model/) · [Compose file](https://docs.docker.com/reference/compose-file/) · [Deploy spec](https://docs.docker.com/reference/compose-file/deploy/)

</details>

<details>
  <summary>Docker troubleshooting</summary>

### Docker troubleshooting

| Problem                              | Cause                                            | Fix                                                                      |
| ------------------------------------ | ------------------------------------------------ | ------------------------------------------------------------------------ |
| Can't reach app on `localhost:3000`  | Dev server binds `127.0.0.1` inside container    | Vite: `server.host: '0.0.0.0'`                                           |
| File changes not detected (HMR)      | FS events don't propagate through bind mounts    | `CHOKIDAR_USEPOLLING=true` in environment                                |
| nodemon not restarting               | Same FS event issue                              | `"dev": "nodemon -L index.js"`                                           |
| `bcrypt_lib.node: Exec format error` | Native module built for host arch, not container | Rebuild inside container: `npm rebuild bcrypt` or `npm ci` in Dockerfile |
| Port conflict                        | Host port already in use                         | Use `HOST_DB_PORT` / `CONTAINER_DB_PORT` env vars                        |
| CRA not hot-reloading in Docker      | Chokidar issue on Docker Desktop                 | `CHOKIDAR_USEPOLLING=true`                                               |

**Example — Vite in Docker:**

```yaml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      CHOKIDAR_USEPOLLING: "true"
    volumes:
      - ./src:/app/src
```

```ts
// vite.config.ts
export default defineConfig({
  server: { host: "0.0.0.0", port: 3000, watch: { usePolling: true } },
});
```

**Thoughts:** On macOS/Windows Docker Desktop, bind-mount performance is slow — use delegated/consistent flags or sync tools (Mutagen). On Linux, native mounts are fast.

</details>
<details>
  <summary>Docker CLI reference</summary>

### Essential Docker commands

Names must be unique per object type; unnamed objects get a generated name. `ls` / `rm` / `prune` work the same idea across `image`, `container`, `volume`.

**Monitor:**

```bash
docker ps                         # running
docker ps -a                      # running + stopped
docker ps -s                      # size
docker ps -f name=hello           # filter
```

**Run:**

```bash
docker run --name test -it hello                  # stdin + TTY
docker run --rm -p 3000:3000 --name app myimage
docker run -it --mount type=bind,src=/data,dst=/data hello bash
docker stop <id> && docker rm <id>
docker logs -f <id>
docker exec -it <id> sh
docker container start|restart|stop|pause <id>
```

**Images:**

```bash
docker build -t myapp:latest .              # Dockerfile in cwd
docker build -f Dockerfile.dev -t myapp:dev .
docker build --no-cache --pull -t myapp .   # fresh base + no layer cache
docker image ls
docker image pull hello
docker image push user/hello
docker image inspect hello        # image JSON (not the container)
docker image rm hello
docker image prune                # unused
```

**Volumes:** (notes: [Volumes](#volumes))

```bash
docker volume ls
docker volume ls -f name=hello
docker volume create hello
docker volume inspect hello
docker volume rm hello
docker volume prune
```

**Compose:**

```bash
docker compose up -d
docker compose up -d --build
docker compose stop
docker compose down          # stop + remove containers/networks
docker compose down -v       # also remove volumes
docker compose logs -f <service>
docker compose exec <service> sh
```

**Cleanup:**

```bash
docker system prune -a       # remove unused containers, networks, images
```

**Useful patterns:**

```bash
# Rebuild and restart one service
docker compose stop api && docker compose up -d --build api

# Logs for service by name
docker compose logs -f api

# Port mapping
docker run -p HOST:CONTAINER myimage   # e.g. -p 5555:5432
```

**Sources:** [Docker CLI](https://docs.docker.com/reference/cli/docker/)

</details>

<details>
  <summary>Orchestration (brief)</summary>

### Orchestration (brief)

| Tool               | Purpose                                         |
| ------------------ | ----------------------------------------------- |
| **Docker Compose** | Local / small multi-container apps              |
| **Docker Swarm**   | Built-in Docker clustering (declining adoption) |
| **Kubernetes**     | Production container orchestration at scale     |

**Thoughts:** Kubernetes is not part of Docker. Use Compose for dev; K8s (or managed: EKS, GKE, ECS) for production scale, rolling deploys, and self-healing.

---

**Sources:** [Kubernetes docs](https://kubernetes.io/docs/concepts/overview/)

</details>

<a name="architecture"></a>
