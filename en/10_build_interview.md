# Build — Interview

Interview Q&A for build and CI.

[← Back to hub](./10_build.md)

### Related docs

* [Build — CI](./10_build_ci.md) · [Build — Bundlers](./10_build_bundlers.md) · [Build — Docker](./10_build_docker.md) · [Build — Infra](./10_build_infra.md) · [Build — Interview](./10_build_interview.md)

### Sections

- [Questions & Answers](#questions-answers)

---

## Questions & Answers

<details>
  <summary>What is CI/CD?</summary>

### What is CI/CD?

**Definition:** **Continuous Integration / Continuous Delivery** — automate build, test, and publish so each change can be integrated and shipped with a short feedback loop. **Continuous Deployment** auto-ships to production after CI. Full notes: [What is CI/CD](#what-is-cicd).

**How it works:** Git event → pipeline (lint/test/build) → artifact → staging → gated or automatic prod. Wider loop also includes operate and plan. Clarify “CD” as delivery vs deployment.

**Sources:** [GitHub Actions — Understanding GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)

</details>

<details>
  <summary>CI vs CD vs Continuous Delivery?</summary>

### CI vs CD vs Continuous Delivery?

**Definition:**

**CI** — automated build + test on each change. **Continuous Delivery** — deployable artifact, manual prod gate. **Continuous Deployment** — auto-deploy to prod.

| Model | Deployment |
|-------|------------|
| CI | None |
| Continuous Delivery | Manual / approval |
| Continuous Deployment | Automatic |

**Sources:** [ByteByteGo — CI/CD](https://blog.bytebytego.com/p/a-crash-course-in-cicd)

</details>

<details>
  <summary>YAML vs JSON?</summary>

### YAML vs JSON?

**Definition:** JSON is the wire/API format; YAML is the human-edited config format (CI, Compose, K8s). Full notes: [YAML](./10_build_ci.md#yaml).

**How it works:** YAML adds comments, optional quotes, indentation instead of `{}`/`[]`, and anchors. Indent with spaces (never tabs). `<<: *anchor` merge is YAML 1.1 — still common in CI.

**Sources:** [YAML 1.2.2 spec](https://yaml.org/spec/1.2.2/)

</details>

<details>
  <summary>Webpack vs Vite?</summary>

### Webpack vs Vite?

**Definition:** Webpack bundles the entire app — flexible but slow HMR. Vite serves native ESM in dev and Rollup for prod — faster DX.

**Sources:** [vite.dev](https://vite.dev/) · [webpack.js.org](https://webpack.js.org/)

</details>

<details>
  <summary>dev vs prod build?</summary>

### dev vs prod build?

**Definition:**

Dev optimizes feedback speed; prod optimizes size, caching, and runtime.

| Dev | Prod |
|-----|------|
| HMR, fast rebuild | Minify, tree-shake |
| Inline source maps | Hashed filenames |
| No minification | Code splitting |

**Sources:** [Vite — Building for production](https://vite.dev/guide/build.html)

</details>

<details>
  <summary>What is tree shaking?</summary>

### What is tree shaking?

**Definition:** Dead-code elimination — bundler removes unused ES module exports.

**Sources:** [Webpack — Tree shaking](https://webpack.js.org/guides/tree-shaking/)

</details>

<details>
  <summary>Docker image layers?</summary>

### Docker image layers?

**Definition:** Filesystem layers come from `FROM`, `RUN`, `COPY`, and `ADD`. Copy `package*.json` before source for cache hits. Multi-stage builds produce slim runtime images. Full notes: [Dockerfile](./10_build_docker.md#dockerfile).

**Sources:** [Dockerfile reference](https://docs.docker.com/reference/dockerfile/) · [Best practices](https://docs.docker.com/build/building/best-practices/)

</details>

<details>
  <summary>CMD vs ENTRYPOINT?</summary>

### CMD vs ENTRYPOINT?

**Definition:** `ENTRYPOINT` is the image’s main executable; `CMD` is the default args (or the default command if there is no `ENTRYPOINT`). Full notes: [Dockerfile](./10_build_docker.md#dockerfile).

**How it works:** Exec form JSON arrays. `docker run image extra` **replaces** `CMD` and **appends** to exec-form `ENTRYPOINT`. Shell-form `ENTRYPOINT` ignores extra args and often is not PID 1.

**Thoughts:** CLI-tool image → `ENTRYPOINT` + overridable `CMD`. App with an easy override → `CMD` only.

</details>

<details>
  <summary>Docker container lifecycle?</summary>

### Docker container lifecycle?

**Definition:** Create → start (running) → optional pause → stop (`SIGTERM` then `SIGKILL`) → remove. `docker run` = create + start. Full notes: [Container lifecycle](./10_build_docker.md#container-lifecycle).

**Thoughts:** Stopped containers still occupy a name until `docker rm` / `--rm`.

</details>

<details>
  <summary>Volumes vs bind mounts vs tmpfs?</summary>

### Volumes vs bind mounts vs tmpfs?

**Definition:** Three ways to keep data **off** the writable layer. **Volume** = Docker-managed, portable, best default. **Bind mount** = host path, for local source/config. **tmpfs** = RAM, Linux, dies with the container.

**How it works:** Empty named volume ← copy image files in; bind/tmpfs ← **hide** files at that path. Full notes: [Docker storage mounts](./10_build_docker.md#docker-storage-mounts).

**Thoughts:** Prod data → volumes. Laptop source → bind. Secrets/scratch → tmpfs.

**Sources:** [Docker — Storage](https://docs.docker.com/engine/storage/)

</details>

<details>
  <summary>Docker Compose?</summary>

### Docker Compose?

**Definition:** YAML (`compose.yaml`) + `docker compose up` / `down` for a multi-container app. One service ≈ one container; default project network; named volumes persist. Full notes: [Docker Compose](#docker-compose).

**How it works:** `services` required; `version` obsolete. Service names are DNS. `depends_on` ≠ ready. Local scale: `--scale`, not Swarm `deploy.replicas`.

**Thoughts:** One file, one project — not Kubernetes.

</details>

<details>
  <summary>package-lock.json purpose?</summary>

### package-lock.json purpose?

**Definition:** Lockfile with exact dependency tree for reproducible installs. Use `npm ci` in CI, `npm install` in local dev.

**Sources:** [npm — package-lock.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-lock-json)

</details>

---

### Tasks

### Task 1 — Minimal GitHub Actions CI

<details><summary>Solution</summary>

```yaml
name: CI
on:
  push:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: npm }
      - run: npm ci && npm test && npm run build
```

</details>

### Task 2 — Multi-stage Dockerfile for Node app

<details><summary>Solution</summary>

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /app/dist ./dist
CMD ["node", "dist/index.js"]
```

</details>

---

<details>
  <summary>Junior — Front-end (build)</summary>

### Junior — Front-end (build)

51. What is webpack?

52. How does a dev build differ from a production build?

133. What is `package-lock.json` for?

134. What is the difference between `npm install` and `npm ci`?

135. What are bundlers used for?

</details>

<details>
  <summary>Middle — Front-end (build)</summary>

### Middle — Front-end (build)

136. Describe modular script loading and bundler loaders.

See also (git, not build): [0_git.md — Interview Practice](0_git_interview.md#interview-practice) — merge vs rebase, staging area, code review.

</details>

</details>


