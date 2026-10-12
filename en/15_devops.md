# DevOps & Infrastructure — overview

Deployment, containers, CI/CD integration, and production operations — interview-level depth for full-stack frontend roles.

### Related docs

- [Build & Tooling](./10_build.md) · [What is CI/CD](./10_build_interview.md#what-is-cicd) · [GitHub Actions](./10_build_ci.md#github-actions) · [SDLC](./7_special_sdlc.md) · [Node.js](./8_nodejs.md) · [Files & file systems](./8_nodejs_modules.md#files--file-systems) · [Node — SIGTERM](./15_devops.md#unix-process-management) · [System Design](./0_system_design.md) · [OS processes & signals](./0_system_design_core.md#processes-multitasking) · [Web server](./6_web_api_http.md#web-server) · [How the Internet works](./6_web_api_core.md#internet) · [Ports & sockets](./6_web_api_apps.md#ports-sockets--network-services) · [OSI & TCP/IP](./6_web_api_apps.md#osi--tcpip-models) · [Encryption](./7_special_security.md#encryption) · [Git — SSH](./0_git.md#ssh-setup) · [DevOps — Interview](./15_devops_interview.md) · [Interview hub](./0_interview.md)

### Main concepts

| Area              | Topics                                                                                                                                       |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **CI/CD**         | Pipelines, GitHub Actions, GitLab CI, CircleCI, deploy strategies                                                                            |
| **Containers**    | Docker (see [10_build.md](./10_build.md)), Compose                                                                                           |
| **Orchestration** | Kubernetes Pod / Service / Deployment (basics)                                                                                               |
| **Runtime**       | Nginx, Apache httpd, env vars, secrets                                                                                                       |
| **Ops**           | Health checks, logs, rollbacks, zero-downtime, [SSH](#ssh), [Unix processes](#unix-process-management), [firewall](#firewall--packet-filter) |
| **Observability** | Structured logs, OpenTelemetry traces, Prometheus/Grafana                                                                                    |
| **Cloud basics**  | Preview deploys, CDN, SSL/TLS                                                                                                                |
| **AWS**           | Lambda, API Gateway, S3, SQS, CloudWatch                                                                                                     |

### Sections

- [Docker](#docker)
- [CI systems](#ci-systems)
- [Kubernetes (interview basics)](#kubernetes-interview-basics)
- [Observability](#observability)
- [Nginx & deployment](#nginx--deployment)
- [AWS & serverless](#aws--serverless)
- [SSH](#ssh)
- [Unix process management](#unix-process-management)

---

## Docker

Full Docker / Compose / multi-stage notes live in [10_build.md — Docker](./10_build.md). This file keeps a one-screen reminder plus ops topics Docker does not cover.

<details>
  <summary>Docker fundamentals</summary>

### Docker fundamentals

**Definition:** An **image** is an immutable layered template; a **container** is a running instance of that image; a **volume** stores data outside the container lifecycle.

**How it works:** Engine = `dockerd` + API + `docker` CLI. Build layers (`FROM`, `COPY`, `RUN`). Multi-stage: compile in a builder, copy artifacts into a slim runtime. Engine, Hub, networks, CLI, lifecycle: [10_build.md — Docker](./10_build_docker.md#docker) · [Dockerfile](./10_build_docker.md#dockerfile) · [container lifecycle](./10_build_docker.md#container-lifecycle) · [Compose](./10_build.md).

**When to use:**

#### Pitfalls

running as root, baking secrets into image, no `.dockerignore`, huge context.

**Examples:**

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
ENV NODE_ENV=production
CMD ["node", "dist/index.js"]
```

**Thoughts:** Image = recipe. Container = process. Persist with [volumes](./10_build_docker.md#volumes), not the writable layer. Lifecycle: [10_build.md — Container lifecycle](./10_build_docker.md#container-lifecycle). What an OS process is: [Processes, multitasking & interrupts](./0_system_design_core.md#processes-multitasking).

</details>

<details>
  <summary>Docker Compose</summary>

### Docker Compose

Multi-container local stack — app + Postgres + Redis:

```yaml
services:
  app:
    build: .
    ports: ["3000:3000"]
    environment:
      DATABASE_URL: postgres://db:5432/app
    depends_on: [db]
  db:
    image: postgres:16
    volumes: [pgdata:/var/lib/postgresql/data]
volumes:
  pgdata:
```

**See also:** [Docker Compose](./10_build_interview.md#docker-compose) · [storage mounts](./10_build_docker.md#docker-storage-mounts) · [container lifecycle](./10_build_docker.md#container-lifecycle).

</details>

---

## CI systems

<details>
  <summary>GitHub Actions vs GitLab CI vs CircleCI</summary>

### GitHub Actions vs GitLab CI vs CircleCI

**Definition:** Hosted CI that runs jobs on git events (push, PR). YAML describes stages: install → test → build → deploy. Concept and product loop: [10_build.md — What is CI/CD](./10_build_interview.md#what-is-cicd). YAML syntax: [10_build.md — YAML](./10_build_ci.md#yaml). Full GitHub Actions syntax (triggers, cron, `needs`, matrix, contexts): [10_build.md — GitHub Actions](./10_build_ci.md#github-actions).

**How it works:**

A runner checks out the repo, executes steps, uploads artifacts. Secrets come from the platform vault, not the repo. Matrix builds test Node versions in parallel.

|             | GitHub Actions            | GitLab CI               | CircleCI                |
| ----------- | ------------------------- | ----------------------- | ----------------------- |
| Config      | `.github/workflows/*.yml` | `.gitlab-ci.yml`        | `.circleci/config.yml`  |
| Native git  | GitHub PRs, environments  | GitLab MR pipelines     | Orbs + GitHub/Bitbucket |
| Typical use | Most JS OSS and startups  | GitLab-hosted companies | Older enterprise setups |

**When to use:** PR checks (`npm ci`, `tsc --noEmit`, tests). Production deploy on `main` after tests. Prefer `npm ci` over `npm install` in CI.

**Pluses and minuses:**

| +                                             | −                                  |
| --------------------------------------------- | ---------------------------------- |
| Same mental model: jobs + artifacts + secrets | Vendor lock-in on YAML helpers     |
| Preview deploys from PR (Vercel/Netlify)      | Self-hosted runners need hardening |

**Examples:**

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: "20", cache: "npm" }
      - run: npm ci
      - run: npm test
```

**Thoughts:** CI is a repeatable machine — `npm ci` + secrets + fail-closed deploys.

**Sources:** [GitHub Actions](https://docs.github.com/en/actions) · [GitLab CI](https://docs.gitlab.com/ee/ci/) · [CircleCI](https://circleci.com/docs/)

</details>

---

<a name="kubernetes-interview-basics"></a>

## Kubernetes (interview basics)

<details>
  <summary>Pod, Service, Deployment</summary>

### Pod, Service, Deployment

**Definition:** Kubernetes schedules **containers** onto a cluster. You rarely create Pods by hand — a **Deployment** keeps N replicas; a **Service** gives them a stable DNS name.

**How it works:**

- **Pod** — smallest unit: one or more containers sharing network/namespace. Ephemeral IPs.
- **ReplicaSet** — keeps a count of Pods. You usually do not manage it directly.
- **Deployment** — owns ReplicaSets; rolling updates and rollbacks (`kubectl rollout undo`).
- **Service** — virtual IP + kube-proxy / ClusterIP / LoadBalancer so clients do not chase Pod IPs.

**When to use:**

Multi-replica Node APIs, cron via CronJob. Skip K8s for a hobby Next.js on Vercel.

#### Common pitfalls

Putting a database in a Deployment without a Volume. Hitting Pod IP from another service instead of the Service DNS.

**Pluses and minuses:**

| +                                  | −                                               |
| ---------------------------------- | ----------------------------------------------- |
| Self-heal and roll out without SSH | YAML and networking (CNI, Ingress) are a career |
| Same API on GKE/EKS/AKS            | Overkill for a single VPS + Docker Compose      |

**Examples:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: { name: api }
spec:
  replicas: 3
  selector: { matchLabels: { app: api } }
  template:
    metadata: { labels: { app: api } }
    spec:
      containers:
        - name: api
          image: ghcr.io/acme/api:1.2.3
          ports: [{ containerPort: 3000 }]
---
apiVersion: v1
kind: Service
metadata: { name: api }
spec:
  selector: { app: api }
  ports: [{ port: 80, targetPort: 3000 }]
```

**Thoughts:** Deployment = desired replicas + rollouts. Service = stable address. Pods die.

**Sources:** [kubernetes.io — Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) · [Services](https://kubernetes.io/docs/concepts/services-networking/service/)

</details>

---

## Observability

<details>
  <summary>Logs, traces, metrics (OpenTelemetry)</summary>

### Logs, traces, metrics (OpenTelemetry)

**Definition:** Three signals: **logs** (what happened), **metrics** (how much, how fast), **traces** (the path of one request across services). **OpenTelemetry (OTel)** is the vendor-neutral API/SDK + Collector that exports all three.

**How it works:**

App SDK creates spans (`trace_id` + `span_id`). Collector receives OTLP (gRPC 4317 / HTTP 4318), batches, exports to Jaeger/Tempo (traces), Prometheus (metrics), Loki/ELK (logs). Grafana dashboards query those backends. Structured logs are JSON with the same `trace_id` so you can jump from a span to log lines.

| Signal  | Question                                | Typical backend       |
| ------- | --------------------------------------- | --------------------- |
| Logs    | What did this instance say?             | Loki, CloudWatch, ELK |
| Metrics | Is p99 latency up?                      | Prometheus + Grafana  |
| Traces  | Which service was slow on this request? | Jaeger, Tempo, X-Ray  |

**When to use:**

Microservices, serverless (X-Ray / OTel), SLOs. Start with structured JSON logs + one RED dashboard (Rate, Errors, Duration) before full tracing.

#### Common pitfalls

`console.log` strings without request id. High-cardinality labels (`user_id` on every metric). Tracing 100% of traffic in prod without sampling.

**Pluses and minuses:**

| +                                    | −                                        |
| ------------------------------------ | ---------------------------------------- |
| One instrumentation, swap vendors    | Collector is another moving part         |
| Trace-id ties logs to a user request | Cardinality explosions in metrics labels |

**Examples:**

```js
import { trace } from "@opentelemetry/api";

const tracer = trace.getTracer("checkout");
await tracer.startActiveSpan("charge", async (span) => {
  try {
    await stripe.charges.create({ amount });
  } finally {
    span.end();
  }
});
```

**Thoughts:** Logs tell stories; metrics tell trends; traces tell the path. Correlate with `trace_id`.

**Sources:** [opentelemetry.io](https://opentelemetry.io/docs/) · [Prometheus](https://prometheus.io/docs/introduction/overview/)

</details>

---

<a name="nginx--deployment"></a>

## Nginx & deployment

<details>
  <summary>Reverse proxy</summary>

### Reverse proxy

**Definition:** Nginx sits in front of the app: it matches the request URI to a **`location`**, then either **serves a file** from disk or **proxies** to an upstream (Node, Go, etc.). It often terminates TLS.

**How it works:**

1. Request arrives (`Host`, path, method). `server` block matches `server_name` + `listen`.
2. **`location`** chooses a handler: prefix (`/api/`), exact (`=`), or regex (`~`).
3. **Static:** `root` + URI → filesystem path (`root /www/data;` + `/images/a.png` → `/www/data/images/a.png`). Paths/FHS: [Files & file systems](./8_nodejs_modules.md#files--file-systems).
4. **`try_files`** checks files in order; last argument is a fallback URI, named location, or `=404`. SPAs use `try_files $uri /index.html` so client routes do not 404.
5. **`proxy_pass`** forwards to an upstream. Set `Host` and `X-Real-IP` so the app sees the original client.

**When to use:** React/Vite `dist/` behind Nginx; `/api/` to Node; CORS headers on the proxy if the API is another origin.

**Pluses and minuses:**

| +                                      | −                                               |
| -------------------------------------- | ----------------------------------------------- |
| Fast static files + one entry for TLS  | Config syntax is easy to get wrong              |
| SPA fallback without Node serving HTML | `proxy_pass` trailing slash changes URI rewrite |

**Examples:**

```nginx
server {
  listen 443 ssl;
  server_name example.com;

  location / {
    root /var/www/html;
    try_files $uri $uri/ /index.html;
  }

  location /api/ {
    proxy_pass http://127.0.0.1:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
  }
}
```

**Thoughts:**

_Nginx maps URL → `location` → file (`root` + `try_files`) or upstream (`proxy_pass`) — that is how the server “knows which resource to send.”_

Same pattern in [10_build.md — Nginx](./10_build.md). Config syntax, FastCGI, systemd → [Nginx configuration](#nginx-configuration). Apache equivalent → [Apache HTTP Server](#apache-http-server). HTTP semantics / compression / Basic auth → [6_web_api.md — Web server](./6_web_api_http.md#web-server). CORS is browser-only → [3_js_browser.md](./3_js_browser_extra.md#cors-same-origin).

**Sources:** [nginx.org — try_files](https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files) · [Serve static content](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/)

</details>

<details>
  <summary>Nginx configuration</summary>

### Nginx configuration

**Definition:** Directives in `nginx.conf` (simple `name args;` or block `{ }`) turn modules on and describe how the master process + workers handle requests.

**How it works:**

- Contexts nest: `main` → `events` / `http` → `server` → `location`. Directives inside `{ }` inherit that context.
- `listen` + `server_name` pick the virtual host. `location` then maps URI → static file (`root` + URI) or upstream (`proxy_pass` / `fastcgi_pass`).
- Prefix `location /images/` wins on **longest prefix**; `location ~ \.png$` is a regex (`~`). Nginx checks prefixes first, then regex.
- `nginx -t` checks syntax without applying. `nginx -s reload` (HUP): master validates, starts new workers, old workers finish in-flight requests. `nginx -s stop` is fast shutdown; `quit` is graceful.
- systemd: `systemctl start|stop|reload|restart|enable|status nginx`. **`reload` = nginx config**, not `daemon-reload` (that one reloads the **unit file**).
- Prefer `reload` after config edits (keep live connections). `restart` drops them (stop then start).

**When to use:**

Node/Next on `localhost:3000` behind a domain; static HTML; PHP via FastCGI; images from disk, rest to an app.

#### Common pitfalls

Treating `systemctl reload nginx` as a systemd-unit reload. Putting `root` on `location /images/` vs `alias` (path join differs). HTTPS in prod is `:443` + certs (Mozilla SSL Configuration Generator), not only `:80`.

**Pluses and minuses:**

| +                                | −                                           |
| -------------------------------- | ------------------------------------------- |
| Event-driven, cheap static + TLS | Easy to misread `proxy_pass` trailing `/`   |
| Graceful reload without downtime | Regex `location` order is easy to get wrong |

**Examples:**

```nginx
# Static + reverse proxy (Node / Next / Express on :3000)
server {
  listen 80;
  listen [::]:80;
  server_name yourdomain.com www.yourdomain.com;

  location / {
    proxy_redirect off;
    proxy_pass http://localhost:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
  }
}

# Prefix vs regex vs FastCGI (official beginner pattern)
server {
  listen 8080;
  root /data/up1;

  location / {
    proxy_pass http://localhost:8080;  # or fastcgi_pass localhost:9000;
  }

  location /images/ {
    root /data;  # URI /images/a.png → /data/images/a.png
  }

  location ~ \.(gif|jpg|png)$ {
    root /data/images;
  }
}

# FastCGI (PHP): ngx_http_fastcgi_module, not HTTP proxy
server {
  location / {
    fastcgi_pass  localhost:9000;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param QUERY_STRING    $query_string;
  }
}

# Extensionless HTML: /about → about.html, /posts → /posts/index.html
# Prefer try_files over a rewrite that leaves $1 unused.
server {
  root /path-to-your-static-files;
  index index.html;
  server_name mydomain.com;

  location / {
    try_files $uri $uri.html $uri/ =404;
  }
}
```

```bash
nginx -t
nginx -s reload          # or: sudo systemctl reload nginx
sudo systemctl status nginx
```

**Thoughts:** _Simple directives end with `;`; blocks are contexts. URI → longest `location` → file or upstream; apply with `-t` then `reload`, not a hard restart._

**See also:** [Reverse proxy](#nginx--deployment) · [Apache HTTP Server](#apache-http-server) · [10_build.md — Nginx](./10_build.md)

**Sources:** [nginx.org — Beginner’s Guide](https://nginx.org/en/docs/beginners_guide.html) · [Controlling nginx](https://nginx.org/en/docs/control.html)

</details>

<details>
  <summary>Apache HTTP Server</summary>

### Apache HTTP Server

**Definition:** Modular HTTP server (`httpd`, since 1995, Apache Software Foundation) configured with text directives in `httpd.conf` (and `Include`d files), optionally plus per-directory `.htaccess`.

**How it works:**

- One directive per line; `\` continues a line. `#` starts a **whole-line** comment (not trailing on the same line as a directive). Quoted args if they contain spaces.
- **Directive names are case-insensitive**; arguments are often case-sensitive. `${VAR}` expands `Define` (and process env from **before** start). `SetEnv` is too late for `${VAR}` in the config file.
- Containers use HTML-like tags: `<Directory path>`, `<DirectoryMatch regex>`, `<Files>`, `<FilesMatch>`, `<Location>`, `<LocationMatch>`, `<VirtualHost addr[:port]>`.
- `.htaccess` (name via `AccessFileName`) is read **on every request** for that directory tree. `AllowOverride None` ignores it — prefer main config when you control the server.
- Access in **2.4** is `Require` (`mod_authz_core`), not 2.2 `Order`/`Allow`/`Deny`. Combine with `<RequireAll>` / `<RequireAny>` / `<RequireNone>`.
- Control: `httpd -t` or `apachectl configtest`. `httpd -S` dumps vhosts. `apachectl -k stop|graceful-stop|restart|graceful`. systemd unit is often `httpd` (RHEL) or `apache2` (Debian). Windows: `httpd.exe -k shutdown|restart` when run as a service/console.
- Stacks: **AMP / XAMPP** = Apache + MySQL/MariaDB + Perl/PHP/Python for local installs.

**When to use:**

Classic LAMP; per-folder rules on shared hosting; `mod_alias` / `mod_rewrite` URL mapping; `mod_status` at `/status`; PHP via `proxy:fcgi` (php-fpm).

#### Common pitfalls

Copying `Order Deny,Allow` from 2.2 into 2.4. Assuming directive names are case-sensitive. Using `SetEnv` and expecting `${VAR}` in `httpd.conf`. Leaving `Require all granted` on `/server-status` on the public internet. `reload` vs `restart`: graceful finishes current requests; restart/`stop` does not.

**Pluses and minuses:**

| +                                                | −                                                                    |
| ------------------------------------------------ | -------------------------------------------------------------------- |
| `.htaccess` without root access (shared hosting) | `.htaccess` on every request is slower                               |
| Huge module ecosystem, virtual hosts             | Prefork MPM: one thread/process per connection (slowloris-sensitive) |
| Familiar on legacy PHP hosts                     | Mix of 2.2 vs 2.4 auth syntax in old tutorials                       |

**Examples:**

```apache
Include /etc/apache2/virtual-hosts/*.conf
Define SITE_ROOT /www/docs/host.example.com

<Directory "/usr/local/httpd/htdocs">
  AllowOverride None
</Directory>

<VirtualHost *:80>
  ServerName host.example.com
  DocumentRoot "${SITE_ROOT}"
  ErrorLog "logs/host.example.com-error_log"
  CustomLog "logs/host.example.com-access_log" combined
  ErrorDocument 404 /404.html

  Alias /common /web/common
  Redirect permanent /old http://example.com/new

  <Location "/status">
    SetHandler server-status
    Require ip 10 172.20 192.168.2
  </Location>

  # PHP-FPM (2.4) — not the old AddHandler/cgi-bin snippet
  <FilesMatch "\.php$">
    SetHandler "proxy:unix:/run/php/php-fpm.sock|fcgi://localhost/"
  </FilesMatch>
</VirtualHost>

# 2.4 allow only loopback (do not use Order/Deny/Allow)
<Directory "/www/docs">
  Require ip 127.0.0.1
</Directory>

RewriteEngine On
RewriteCond %{HTTP_HOST} ^www\.example\.com$ [NC]
RewriteRule ^(.*)$ http://example.com$1 [R=301,L]
RewriteRule ^/news/([0-9]+)$ /news.cfm?id=$1 [PT,L]
```

```bash
httpd -t                 # or: apachectl configtest
httpd -S                 # virtual host dump
sudo systemctl reload httpd   # graceful; Debian: apache2
sudo apachectl -k graceful
```

**Thoughts:** _Containers (`<Directory>` / `<VirtualHost>`) scope directives; `.htaccess` is optional per-dir override; 2.4 auth is `Require`; test with `-t`, apply with graceful reload._

**See also:** [Nginx configuration](#nginx-configuration) · reverse proxy in [0_system_design.md](./0_system_design.md) · [10_build.md](./10_build.md)

**Sources:** [httpd.apache.org — Configuration Files](https://httpd.apache.org/docs/current/configuring.html) · [Stopping and Restarting](https://httpd.apache.org/docs/current/stopping.html) · [Access Control](https://httpd.apache.org/docs/2.4/howto/access.html)

</details>

---

## AWS & serverless

<details>
  <summary>AWS Lambda</summary>

### AWS Lambda

**Definition:** Serverless compute — you upload a function; AWS runs it in response to events and bills per invocation and duration (GB-seconds).

**How it works:**

1. **Trigger** fires (API Gateway HTTP, S3 upload, SQS message, EventBridge schedule, DynamoDB stream).
2. Lambda **creates or reuses** an execution environment (container).
3. **Init phase** — download code, start runtime, run code outside handler (`INIT_START` in logs).
4. **Invoke phase** — run handler with event payload.
5. **Freeze or destroy** environment after idle timeout.

| Concept         | Detail                                                                   |
| --------------- | ------------------------------------------------------------------------ |
| **Cold start**  | New environment = extra latency (`initDurationMs` in platform report)    |
| **Warm start**  | Reused environment — faster                                              |
| **Timeout**     | Max 15 minutes per invocation                                            |
| **Memory**      | 128 MB – 10 GB (more memory = more CPU)                                  |
| **Concurrency** | Account/region limits; reserved concurrency per function                 |
| **State**       | **Stateless** — persist to DB/S3/DynamoDB, not `/tmp` across invocations |
| **Env vars**    | Config + secrets (use Secrets Manager for sensitive values)              |

| Trigger     | Use case                 |
| ----------- | ------------------------ |
| API Gateway | REST/HTTP API backend    |
| S3          | Image resize on upload   |
| SQS         | Async job processing     |
| EventBridge | Cron (`rate(5 minutes)`) |
| SNS         | Fan-out notifications    |

```
Client → API Gateway → Lambda → DynamoDB / RDS / S3
                             → SQS (async downstream)
```

Provisioned concurrency, smaller bundles, Node 20+ runtime, avoid huge dependencies, keep handler outside heavy init when possible.

CloudWatch Logs (automatic), X-Ray tracing, structured JSON logs via Lambda Powertools.

_"Used Lambda behind API Gateway for event-driven endpoints — pay per request, no server management. Watch cold starts, keep functions stateless, env vars for config, IAM roles for least-privilege access to S3/SQS."_

**Examples:**

```js
export const handler = async (event) => {
  const body = JSON.parse(event.body ?? "{}");
  // business logic
  return {
    statusCode: 200,
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ ok: true, input: body }),
  };
};
```

**Sources:** [AWS Lambda — Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) · [Lambda runtime environment](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtime-environment.html)

</details>

<details>
  <summary>AWS services map (interview)</summary>

### AWS services map (interview)

**Definition:**

Quick map of AWS services commonly asked on fullstack/backend interviews.

| Service             | Role                                  |
| ------------------- | ------------------------------------- |
| **Lambda**          | Serverless functions                  |
| **API Gateway**     | HTTP/WebSocket API front door         |
| **S3**              | Object storage (files, static assets) |
| **RDS / Aurora**    | Managed PostgreSQL/MySQL              |
| **DynamoDB**        | Managed NoSQL key-value/document      |
| **SQS**             | Message queue                         |
| **SNS**             | Pub/sub notifications                 |
| **EventBridge**     | Event bus + scheduling                |
| **CloudWatch**      | Logs, metrics, alarms                 |
| **IAM**             | Roles, policies — who can call what   |
| **Secrets Manager** | Rotate and inject secrets             |
| **ECS/EKS**         | Containers when Lambda is not enough  |
| **CloudFront**      | CDN                                   |

**How it works:** IAM role attached to Lambda grants `s3:GetObject`, `sqs:SendMessage`, etc. — never embed long-lived keys in code.

**When to use:** Serverless API (API GW + Lambda + DynamoDB); file pipeline (S3 trigger → Lambda resize → S3); async workers (SQS → Lambda).

**Sources:** [AWS documentation](https://docs.aws.amazon.com/)

</details>

---

<a name="ssh"></a>

## SSH

<details>
  <summary>SSH (remote login, keys, tunnels)</summary>

### SSH

**Definition:** **SSH** (Secure Shell) is an **application-layer** protocol for encrypted remote login, remote commands, file transfer (`scp` / SFTP), and TCP tunnels. OpenSSH is the usual client/server. SSH-2 (RFC 4251–4254, Internet Standard since 2006) replaced **Telnet** (cleartext) and obsolete SSH-1.

**How it works:**

- Client talks to `sshd` (default TCP **22**). After key exchange, the session is encrypted. SSHv2 is built to resist passive sniffing and to authenticate the **host** so a naïve **MITM** cannot silently swap endpoints (you still must verify the host key on first connect).
- **Public-key auth (preferred):** you keep a **private** key; the server stores the matching **public** key in `~/.ssh/authorized_keys` (one key per line). The client proves it holds the private key; the server never sees that private key. Algorithms: **Ed25519** (default for new keys), ECDSA, RSA. Asymmetric crypto: [Encryption](./7_special_security.md#encryption).
- **Password / keyboard-interactive:** weaker (guessable, phishing, brute-force). Typical for the **first** login before a key is installed; then turn it off.
- **Host-based / “by IP”:** OpenSSH `HostbasedAuthentication` (rhosts-style) is weak. An **IP allowlist** is not identity — NAT and shared IPv4 mean many users can sit behind one address. Use keys, then optionally restrict _who_ may connect (`AllowUsers`, firewall) as defense in depth.
- **Do not copy the private key** between machines. Copy only `*.pub`. Protect the private key with a passphrase; use `ssh-agent` so you type it once per session.

```
# /etc/ssh/sshd_config — then: sudo systemctl restart sshd   (or ssh.service)
PubkeyAuthentication yes
PasswordAuthentication no          # disables password login for everyone
PermitRootLogin prohibit-password  # OpenSSH default: root by key only, not password
# PermitRootLogin no               # blocks ALL root SSH (keys too) — use a sudo user instead
```

`PermitRootLogin no` is **not** “turn off password for root.” It forbids root over SSH entirely. To keep root+keys but drop passwords, use `prohibit-password` plus `PasswordAuthentication no`.

generate a dedicated Ed25519 key, paste `id_ed25519.pub` under **Settings → SSH and GPG keys → New SSH key**, or `gh ssh-key add ~/.ssh/id_ed25519.pub`. Git-side steps: [Git — SSH setup](./0_git.md#ssh-setup).

**When to use:**

VPS / hosting shells; Git remotes (`git@github.com:…`); `scp`/`sftp`; local (`-L`) / remote (`-R`) / SOCKS (`-D`) forwarding. In Kubernetes you apply YAML — you do not SSH into Pods as the normal deploy path ([Pod vs Deployment](#pod-vs-deployment-vs-service)).

#### Common pitfalls

Treating `id_rsa` as the modern default. Moving `/tmp/*.pub` **onto** `authorized_keys` (wipes other keys). World-writable `~/.ssh`. Sharing the **private** key. Setting `PermitRootLogin no` before a sudo user + key exists (lockout). Skipping host-key verification.

**Pluses and minuses:**

| +                                 | −                                                                                     |
| --------------------------------- | ------------------------------------------------------------------------------------- |
| Encrypted shell, files, tunnels   | Host-key TOFU: a first-connect MITM can still trick you if you skip fingerprint check |
| Key auth: no password on the wire | Loose `~/.ssh` modes → sshd **ignores** `authorized_keys` (`StrictModes`)             |
| Ed25519 keys are short and strong | Root + password on the internet is an instant brute-force target                      |

**Examples:**

```bash
# Prefer Ed25519 (GitHub and current OpenSSH). Default files: ~/.ssh/id_ed25519 + .pub
ssh-keygen -t ed25519 -C "you@example.com"

# macOS: copy public key
pbcopy < ~/.ssh/id_ed25519.pub

# Install pubkey (asks for the current password once)
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@192.0.2.50
ssh-copy-id -i ~/.ssh/id_ed25519.pub root@2001:db8::5

# Manual fallback: scp then append on the server (do not overwrite existing keys)
scp ~/.ssh/id_ed25519.pub root@192.0.2.50:/tmp/
scp ~/.ssh/id_ed25519.pub 'root@[2001:db8::5]:/tmp/'   # IPv6: brackets for scp

ssh root@192.0.2.50
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
# chown user:user -R ~/.ssh   # that user, not always root

ssh root@192.0.2.50   # should not ask the account password (passphrase for the key still may)
```

**Thoughts:** _SSH encrypts the session; identity is a private key the server never stores — put only the public half in `authorized_keys`, then disable passwords._

**Sources:** [OpenSSH ssh(1)](https://github.com/openssh/openssh-portable/blob/master/ssh.1) · [sshd(8)](https://github.com/openssh/openssh-portable/blob/master/sshd.8) · [RFC 4251](https://www.rfc-editor.org/rfc/rfc4251) · [GitHub — generating an SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) · [GitHub — adding a key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

</details>

---

<a name="unix-process-management"></a>

## Unix process management

<details>
  <summary>PID, jobs, nice, top, ps, kill</summary>

### PID, jobs, nice, top, ps, kill

**Definition:** On Unix-like systems a **process** is a running program identified by a **PID** (process ID). The OS scheduler, job-control shell, and tools like `ps`/`top`/`kill` use that ID to inspect, pause, resume, re-prioritize, or signal the process. Scheduler, context, interrupts, POSIX signal table: [Processes, multitasking & interrupts](./0_system_design_core.md#processes-multitasking).

**How it works:**

- **Windows:** Task Manager (GUI). **Unix:** CLI plus job control — the shell tracks _jobs_ of the current terminal separately from system-wide PIDs.
- **Foreground vs background** is _job control_ (does the terminal wait for the command?), not CPU niceness. Foreground: the shell waits; you cannot type a new command until it exits (or you suspend it). Background: append `&`; the shell prints `[job_number] PID` and returns the prompt.
- **Job vs PID:** `[1]` is the job index in _this_ shell session. PID is global. `jobs` lists jobs (`+` = current, `-` = previous). `fg` / `bg` take a job spec (`%1`), not a PID.
- **Suspend / continue:** Ctrl+Z sends **SIGTSTP** (catchable stop from the terminal). Status becomes `suspended` / `Stopped`. `bg` continues it in the background; `fg` (no args) brings the current job (`+`) to the foreground. **SIGSTOP** also stops a process but cannot be caught (unlike SIGTSTP).
- **Niceness (CPU priority):** Linux `nice` values run from **−20** (most CPU time) to **19** (least). Lower number = higher priority. Unprivileged users can only _raise_ niceness (make themselves nicer / slower), not grab more CPU than others.
- **Inspect:** `top` — live CPU, RAM, swap, all processes (`q` quit, `?` help). Linux often uses `htop` (ncurses, cross-platform including macOS). `ps` = process status: `ps -A` (same as `-e`) all processes; `ps -Af` full-format listing. PID is in the PID column.
- **Signals:** `kill PID` sends **SIGTERM** (15) by default — catchable, used for graceful exit. `kill -9 PID` / `kill -KILL PID` sends **SIGKILL**, which **cannot be caught, blocked, or ignored**. `sudo` is needed only if you do not own the process — not because `-9` is special.

**When to use:**

Long `ping` / logs in the background while you keep using the shell. Archive a large tree with low niceness so a DB keeps CPU. Find a runaway Node PID with `ps`/`top` and `kill` it. Deploy path: SIGTERM first (K8s / `docker stop`), SIGKILL only after the grace period — [Node SIGTERM](./15_devops.md#unix-process-management).

#### Common pitfalls

Calling foreground/background “process priority” (that is `nice`). Assuming default `kill` is SIGKILL — it is SIGTERM. Believing the program must “handle SIGKILL” to die — SIGKILL is forced by the kernel. Using `kill -9` first and leaking connections. Mixing up job id `[1]` with PID. `renice -n` on util-linux is **absolute** niceness by default, not a delta (POSIX relative mode needs `POSIXLY_CORRECT` or `--relative`).

**Pluses and minuses:**

| +                                    | −                                                       |
| ------------------------------------ | ------------------------------------------------------- |
| PID is the handle for every ops tool | PID reuse: a late `kill` can hit a new process          |
| SIGTERM lets the app drain work      | SIGKILL skips cleanup (locks, temp files, connections)  |
| Job control is per-terminal and fast | `jobs`/`fg`/`bg` do not see processes started elsewhere |

**Examples:**

```bash
# Background (job number + PID). -i = ping interval in seconds
ping 8.8.8.8 -i 15 &
# [1] 22490
ping 8.8.4.4 -i 10 &
# [2] 22491

jobs
# [1]  - running    ping 8.8.8.8 -i 15
# [2]  + running    ping 8.8.4.4 -i 10   # + current → fg with no args

# Foreground, then suspend (Ctrl+Z → SIGTSTP), then continue in background
ping 8.8.8.8 -i 15
# ^Z
# [1]  + 22495 suspended  ping 8.8.8.8 -i 15
bg          # continue job + in background
fg %1       # take job 1 back to the foreground

# Low CPU priority for a heavy archive (do not starve the DB)
nice -n 19 tar -czvf /path/to_save_archive.tar.gz /path/to_big_folder
renice -n 19 -p 22495   # Linux: -n is absolute niceness unless POSIXLY_CORRECT

ps -A          # all processes
ps -Af         # full format
top            # live; q to quit
kill 22495     # SIGTERM — prefer this
kill -9 22495  # SIGKILL — last resort; sudo only if not your process
```

**Thoughts:** _Identify with PID; job-control (`&` / `fg` / `bg` / Ctrl+Z) is only this shell; `nice` is CPU share; default `kill` is SIGTERM — SIGKILL (`-9`) cannot be handled._

**Sources:** [kill(1)](https://man7.org/linux/man-pages/man1/kill.1.html) · [signal(7)](https://man7.org/linux/man-pages/man7/signal.7.html) · [nice(1)](https://man7.org/linux/man-pages/man1/nice.1.html) · [renice(1)](https://man7.org/linux/man-pages/man1/renice.1.html) · [ps(1)](https://man7.org/linux/man-pages/man1/ps.1.html) · [htop](https://htop.dev/)

</details>

<details>
  <summary>Firewall / packet filter</summary>

### Firewall / packet filter

**Definition:** A **firewall** (packet filter) drops, accepts, or rejects traffic by IP, port, and protocol. On Linux the filter runs in the kernel; `iptables` / `nftables` only install rules.

**How it works:**

- Chains: **INPUT** (to this host), **OUTPUT** (from this host), **FORWARD** (transit, e.g. a router). Rules match in order; first hit wins unless you append (`-A`) and later rules never run for that packet.
- Typical web host: allow 22/80/443, drop the rest. ICMP echo can be rejected so `ping` fails while HTTP still works.
- Host sockets and ports: [6_web_api.md — Ports](./6_web_api_apps.md#ports-sockets--network-services). Path of a request: [How the Internet works](./6_web_api_core.md#internet).

**When to use:**

Hide a Node port from the Internet (bind `127.0.0.1` **and** firewall). Block scanners. Do not use a random `iptables` one-liner on a remote SSH box without a allow-22 rule — you can lock yourself out.

#### Common pitfalls

`- j REJECT` (invalid). Filtering ICMP and thinking the host is “offline” for HTTP. Forgetting IPv6 (`ip6tables` / `nft` inet).

**Pluses and minuses:**

| +                        | −                                            |
| ------------------------ | -------------------------------------------- |
| Cheap host-level control | Easy to lock out SSH                         |
| Kernel-speed             | `iptables` vs `nftables` vs cloud SG overlap |

**Examples:**

```bash
# Reject inbound ping (ICMP echo-request). No space in -j.
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j REJECT
sudo iptables -L

# Modern Linux prefers nftables; cloud VMs also use security groups — both must allow 443.
```

**Thoughts:** _The kernel filters packets; the CLI only writes the rules — keep SSH allowed before you experiment._

**Sources:** [iptables(8)](https://man7.org/linux/man-pages/man8/iptables.8.html) · [nftables](https://wiki.nftables.org/) · [Doka — Working with the network](https://doka.guide/tools/network/)

</details>

---

<a name="interview-practice"></a>

## Interview Practice

### Question index

| Topic                                    | Answer                                                  |
| ---------------------------------------- | ------------------------------------------------------- |
| Nginx reverse proxy / which file to send | Q&A                                                     |
| Nginx vs Apache config                   | Q&A                                                     |
| Docker vs VM                             | Q&A                                                     |
| Container lifecycle                      | [10_build.md](./10_build_docker.md#container-lifecycle) |
| CI/CD pipeline stages                    | Q&A                                                     |
| Blue-green / rolling deploy              | Q&A                                                     |
| Environment variables vs secrets         | Q&A                                                     |
| Health checks                            | Q&A                                                     |
| AWS Lambda                               | Q&A                                                     |
| AWS services map                         | Q&A                                                     |
| Observability (logs / traces / metrics)  | Q&A                                                     |
| Pod vs Deployment vs Service             | Q&A                                                     |
| SSH keys vs password                     | Q&A                                                     |
| PID / kill vs kill -9                    | Q&A                                                     |
| Firewall / iptables                      | Q&A                                                     |

---

## Questions & Answers

<details>
  <summary>How does the server know which resource to send?</summary>

### How does the server know which resource to send?

**Definition:** The HTTP server maps **method + Host + path** to a handler: a static file, a reverse-proxy upstream, or an application route.

**How it works:**

1. TCP/TLS accepts the connection; HTTP parses the request line (`GET /app/settings HTTP/1.1`) and `Host`.
2. **Virtual host** (`server_name`) picks the site config.
3. **Nginx `location`** matches the URI. Longest prefix / regex rules decide among blocks.
4. **Static:** `root` or `alias` + URI → disk path; `try_files $uri $uri/ /index.html` serves the file if it exists, else the SPA shell.
5. **API:** `location /api/` + `proxy_pass` forwards to Node; Node’s router (Express/Fastify) then matches `/users/:id`.
6. Response headers include `Content-Type` (from extension or the app). CORS headers may be added here — they do not apply to Postman.

**When to use:** Production SPA + API on one domain; `index.html` fallback for client-side routing.

**Thoughts:** _The browser asks for a URL; Nginx `location` + `try_files`/`proxy_pass` (or the app router) pick the bytes to return._

**See also:** [Reverse proxy](#nginx--deployment) · [Apache HTTP Server](#apache-http-server)

**Sources:** [nginx.org — location](https://nginx.org/en/docs/http/ngx_http_core_module.html#location)

</details>

<details>
  <summary>Nginx vs Apache — how do you configure them?</summary>

### Nginx vs Apache — how do you configure them?

**Definition:** Both are HTTP servers/reverse proxies. Nginx uses `server`/`location` in `nginx.conf`; Apache uses `httpd.conf` containers (`<VirtualHost>`, `<Directory>`) and optional `.htaccess`.

**How it works:** Nginx: event-driven workers, `nginx -t` then `reload` (HUP, keep connections). Apache: MPM (event/worker/prefork), `httpd -t`, `apachectl -k graceful`. Apache 2.4 access is `Require`, not 2.2 `Order`/`Allow`. Nginx has no `.htaccess`; you edit the main config (or included snippets).

**When to use:** Nginx in front of Node/SPA; Apache on LAMP / shared hosting that only allows `.htaccess`.

**Thoughts:** _Nginx = contexts + `location`; Apache = tagged containers + optional `.htaccess`; always syntax-check, then graceful reload._

**See also:** [Nginx configuration](#nginx-configuration) · [Apache HTTP Server](#apache-http-server)

</details>

<details>
  <summary>Docker vs virtual machine?</summary>

### Docker vs virtual machine?

VMs virtualize hardware + full OS. Containers share host kernel, isolate processes/filesystem — lighter, faster start. Use containers for app packaging; VMs for strong isolation or different OS. Dockerfile: [10_build.md](./10_build_docker.md#dockerfile).

</details>

<details>
  <summary>Docker container lifecycle?</summary>

### Docker container lifecycle?

Create → start → (pause) → stop (`SIGTERM` then `SIGKILL`) → remove. Full answer: [10_build.md — Container lifecycle](./10_build_docker.md#container-lifecycle).

</details>

<details>
  <summary>Blue-green vs rolling deployment?</summary>

### Blue-green vs rolling deployment?

**Blue-green** — two full environments; switch traffic instantly; easy rollback. **Rolling** — replace instances gradually; less infra duplication; slower rollback.

</details>

<details>
  <summary>Secrets management?</summary>

### Secrets management?

Never commit secrets. Use CI secret store, vault, or platform env vars (Vercel/GitHub Secrets). Inject at runtime; rotate keys; separate dev/staging/prod.

</details>

<details>
  <summary>Health check endpoint?</summary>

### Health check endpoint?

`GET /health` returns 200 when app + dependencies OK. Load balancer removes unhealthy instances. Check DB connectivity lightly — don't overload DB on every probe.

</details>

<details>
  <summary>What is AWS Lambda and how did you use it?</summary>

### What is AWS Lambda and how did you use it?

**Definition:** Serverless function runtime — event-triggered, auto-scaling, pay per invocation.

**How it works:** Trigger → execution environment (cold/warm) → handler runs → response or async side effect. Stateless; config via env vars; permissions via IAM role.

**Typical uses:** API Gateway endpoints, S3 event processing, SQS consumers, scheduled cron jobs (EventBridge).

**What to mention in interview:**

- Cold start awareness and bundle size optimization.
- IAM least privilege (function role, not access keys in code).
- CloudWatch for logs and alarms.
- Pair with SQS for retries and DLQ.
- When **not** to use Lambda — long-running processes, persistent WebSockets at huge scale, very low-latency always-warm APIs (consider ECS).

**Sources:** [AWS Lambda DG](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

</details>

<details>
  <summary>Common AWS services for a backend?</summary>

### Common AWS services for a backend?

API Gateway + Lambda + DynamoDB/RDS for APIs; S3 for files; SQS/SNS for async; CloudWatch for ops; IAM for security. CloudFront for CDN in front of static/API assets.

See [AWS services map](#aws-services-map-interview).

</details>

<details>
  <summary>Logs vs traces vs metrics?</summary>

### Logs vs traces vs metrics?

**Definition:** Logs = events. Metrics = aggregates over time. Traces = one request across services (`trace_id`). OpenTelemetry exports all three; Prometheus/Grafana for metrics; Jaeger/Tempo for traces.

**Thoughts:** Correlate with `trace_id` — do not grep three tools separately.

**See also:** [Logs, traces, metrics](#observability)

</details>

<details>
  <summary>Pod vs Deployment vs Service?</summary>

### Pod vs Deployment vs Service?

**Definition:** **Pod** is the running container(s). **Deployment** keeps N replicas and rolls out new images. **Service** is a stable DNS/IP in front of changing Pods.

**Thoughts:** You apply a Deployment + Service; you do not SSH into Pods.

**See also:** [Pod, Service, Deployment](#kubernetes-interview-basics)

</details>

<details>
  <summary>SSH keys vs password?</summary>

### SSH keys vs password?

**Definition:** SSH can authenticate with a **key pair** (client proves it holds the private key; server has the public key in `authorized_keys`) or with a **password**.

**How it works:** Ed25519 (or RSA) via `ssh-keygen`; install `.pub` with `ssh-copy-id`. Then `PasswordAuthentication no`. `PermitRootLogin prohibit-password` allows root **keys** only; `PermitRootLogin no` blocks root SSH completely.

**Thoughts:** Keys first, then disable passwords — full notes: [SSH](#ssh).

</details>

<details>
  <summary>What is a PID? kill vs kill -9?</summary>

### What is a PID? kill vs kill -9?

**Definition:** **PID** is the process identifier. `kill PID` sends **SIGTERM** (graceful, catchable). `kill -9 PID` sends **SIGKILL**, which the kernel applies and the process cannot handle.

**Thoughts:** Prefer SIGTERM; SIGKILL is last resort. Full notes: [Unix process management](#unix-process-management).

</details>

<details>
  <summary>Firewall / iptables?</summary>

### Firewall / iptables?

**Definition:** Kernel packet filter: match IP/port/protocol on INPUT/OUTPUT/FORWARD and ACCEPT/DROP/REJECT. `iptables`/`nftables` write the rules.

**How it works:** Example — reject ping without touching HTTP: `iptables -A INPUT -p icmp --icmp-type echo-request -j REJECT`. Keep SSH allowed. Full notes: [Firewall / packet filter](#firewall--packet-filter).

**Thoughts:** Filter in the kernel; CLI is just the config.

</details>

---

### Tasks

### Task 1 — Explain failed deploy rollback

New version crashes on start. Steps to restore service?

<details><summary>Answer</summary>

Revert to previous image/tag in orchestrator, or redeploy last known-good artifact; use feature flags to disable bad feature without full rollback if applicable.

</details>

### Task 2 — Why multi-stage Docker build?

<details><summary>Answer</summary>

Smaller production image (no devDependencies, no source), fewer attack surfaces, faster deploys.

</details>

---

**usefull links**:

- https://selectel.ru/blog/courses/network-protocols/
