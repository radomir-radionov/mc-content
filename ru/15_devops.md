# DevOps & Infrastructure — overview

Развертывание, контейнеры, интеграция CI/CD и производственные операции — глубина на уровне собеседования для ролей полного стека внешнего интерфейса.

### Related docs

- [Сборка и инструменты](./10_build.md) · [Что такое CI/CD](./10_build_interview.md#what-is-cicd) · [Действия GitHub](./10_build_ci.md#github-actions) · [SDLC](./7_special_sdlc.md) · [Node.js](./8_nodejs.md) · [Файлы и файловые системы](./8_nodejs_modules.md#files--file-systems) · [Node — SIGTERM](./15_devops.md#unix-process-management) · [Проектирование системы](./0_system_design.md) · [Процессы ОС & сигналы](./0_system_design_core.md#processes-multitasking) · [Веб-сервер](./6_web_api_http.md#web-server) · [Как работает Интернет](./6_web_api_core.md#internet) · [Порты и сокеты](./6_web_api_apps.md#ports-sockets--network-services) · [OSI & TCP/IP](./6_web_api_apps.md#osi--tcpip-models) · [Шифрование](./7_special_security.md#encryption) · [Git — SSH](./0_git.md#ssh-setup) · [DevOps — собеседование](./15_devops_interview.md) · [Центр интервью](./0_interview.md)

### Main concepts

| Площадь                     | Темы                                                                                                                                                               |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **CI/CD**                   | Конвейеры, действия GitHub, GitLab CI, CircleCI, стратегии развертывания                                                                                           |
| **Контейнеры**              | Docker (см. [10_build.md](./10_build.md)), Compose                                                                                                                 |
| **Аранжировка**             | Kubernetes Pod/Сервис/Развертывание (основы)                                                                                                                       |
| **Время выполнения**        | Nginx, Apache httpd, переменные окружения, секреты                                                                                                                 |
| **Операции**                | Проверки работоспособности, журналы, откаты, нулевое время простоя, [SSH](#ssh), [Процессы Unix](#unix-process-management), [брандмауэр](#firewall--packet-filter) |
| **Наблюдаемость**           | Структурированные журналы, трассировки OpenTelemetry, Prometheus/Grafana                                                                                           |
| **Основы работы с облаком** | Предварительные развертывания, CDN, SSL/TLS                                                                                                                        |
| **АВС**                     | Lambda, API-шлюз, S3, SQS, CloudWatch                                                                                                                              |

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

Полные заметки Docker/Compose/многоэтапные находятся в [10_build.md — Docker](./10_build.md). В этом файле хранится одноэкранное напоминание, а также темы эксплуатации, которые Docker не охватывает.

<details>
  <summary>Docker fundamentals</summary>

### Docker fundamentals

**Definition:** **Изображение** — это неизменяемый многоуровневый шаблон; **контейнер** — это работающий экземпляр этого образа; **том** хранит данные вне жизненного цикла контейнера.

**How it works:** Двигатель = `dockerd` + API + `docker` Интерфейс командной строки. Построить слои (`FROM`, `COPY`, `RUN`). Многоэтапность: компилируйте в сборщике, копируйте артефакты в тонкую среду выполнения. Engine, Hub, сети, CLI, жизненный цикл: [10_build.md — Docker](./10_build_docker.md#docker) · [Dockerfile](./10_build_docker.md#dockerfile) · [жизненный цикл контейнера](./10_build_docker.md#container-lifecycle) · [Compose](./10_build.md).

**When to use:**

#### Pitfalls

запуск от имени пользователя root, сохранение секретов в образе, нет `.dockerignore`, огромный контекст.

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

**Thoughts:** Изображение = рецепт. Контейнер = процесс. Сохраняйте с помощью [volumes](./10_build_docker.md#volumes), а не записываемого слоя. Жизненный цикл: [10_build.md — Жизненный цикл контейнера](./10_build_docker.md#container-lifecycle). Что такое процесс ОС: [Процессы, многозадачность и прерывания](./0_system_design_core.md#processes-multitasking).

</details>

<details>
  <summary>Docker Compose</summary>

### Docker Compose

Многоконтейнерный локальный стек — приложение + Postgres + Redis:

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

**See also:** [Docker Compose](./10_build_interview.md#docker-compose) · [монтирование хранилища](./10_build_docker.md#docker-storage-mounts) · [жизненный цикл контейнера](./10_build_docker.md#container-lifecycle).

</details>

---

## CI systems

<details>
  <summary>GitHub Actions vs GitLab CI vs CircleCI</summary>

### GitHub Actions vs GitLab CI vs CircleCI

**Definition:** Размещенный CI, который запускает задания по событиям git (push, PR). YAML описывает этапы: установка → тестирование → сборка → развертывание. Концепция и цикл продукта: [10_build.md — Что такое CI/CD](./10_build_interview.md#what-is-cicd). Синтаксис YAML: [10_build.md — YAML](./10_build_ci.md#yaml). Полный синтаксис действий GitHub (триггеры, cron, `needs`, матрица, контексты): [10_build.md — Действия GitHub](./10_build_ci.md#github-actions).

**How it works:**

Бегун проверяет репозиторий, выполняет шаги, загружает артефакты. Секреты исходят из хранилища платформы, а не из репозитория. Matrix параллельно создает тестовые версии Node.

| | Действия GitHub        | GitLab CI                             | КругCI                          |
| | ---------------------- | ------------------------------------- | ------------------------------- | ------------------------------ |
| | Конфигурация           | `.github/workflows/*.yml`             | `.gitlab-ci.yml`                | `.circleci/config.yml`         |
| | Родной git             | PR-сообщения GitHub, окружающая среда | Конвейеры GitLab MR             | Сферы + GitHub/Bitbucket       |
| | Типичное использование | Большинство JS OSS и стартапов        | Компании, размещенные на GitLab | Старые корпоративные настройки |

**When to use:** PR-проверки(`npm ci`, `tsc --noEmit`, тесты). Развертывание производства на `main` после тестов. Предпочитать `npm ci` над `npm install` в КИ.

**Pluses and minuses:**

| +                                                           | −                                       |
| ----------------------------------------------------------- | --------------------------------------- |
| Та же ментальная модель: работа + артефакты + секреты       | Привязка к поставщику в помощниках YAML |
| Предварительная версия развертываний от PR (Vercel/Netlify) | Самостоятельным бегунам нужна закалка   |

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

**Thoughts:** CI — это повторяемая машина. `npm ci` + секреты + аварийно-закрытые развертывания.

**Sources:** [Действия GitHub](https://docs.github.com/en/actions) · [GitLab CI](https://docs.gitlab.com/ee/ci/) · [CircleCI](https://circleci.com/docs/)

</details>

---

<a name="kubernetes-interview-basics"></a>

## Kubernetes (interview basics)

<details>
  <summary>Pod, Service, Deployment</summary>

### Pod, Service, Deployment

**Definition:** Kubernetes планирует **контейнеры** в кластер. Поды редко создаются вручную — **Развертывание** содержит N реплик; **Сервис** предоставляет им стабильное DNS-имя.

**How it works:**

- **Pod** — наименьшая единица: один или несколько контейнеров, совместно использующих сеть/пространство имен. Эфемерные IP-адреса.
- **ReplicaSet** — сохраняет количество подов. Обычно вы не управляете этим напрямую.
- **Развертывание** — владеет наборами реплик; накатывающие обновления и откаты (`kubectl rollout undo`).
- **Сервис** — виртуальный IP + kube-proxy/ClusterIP/LoadBalancer, чтобы клиенты не гонялись за IP-адресами подов.

**When to use:**

API-интерфейсы узлов с несколькими репликами, cron через CronJob. Откажитесь от K8 ради хобби. Next.js на Vercel.

#### Common pitfalls

Размещение базы данных в развертывании без тома. Нажатие IP-адреса модуля из другой службы вместо DNS службы.

**Pluses and minuses:**

| +                                          | −                                                          |
| ------------------------------------------ | ---------------------------------------------------------- |
| Самовосстановление и развертывание без SSH | YAML и сетевое взаимодействие (CNI, Ingress) — это карьера |
| Тот же API для GKE/EKS/AKS                 | Overkill для одного VPS + Docker Compose                   |

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

**Thoughts:** Развертывание = желаемые реплики + развертывание. Сервис = стабильный адрес. Стручки умирают.

**Sources:** [kubernetes.io — Развертывания](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) · [Сервисы](https://kubernetes.io/docs/concepts/services-networking/service/)

</details>

---

## Observability

<details>
  <summary>Logs, traces, metrics (OpenTelemetry)</summary>

### Logs, traces, metrics (OpenTelemetry)

**Definition:** Три сигнала: **логи** (что произошло), **метрики** (сколько и как быстро), **трассировки** (путь одного запроса между сервисами). **OpenTelemetry (OTel)** — это независимый от поставщика API/SDK + Collector, который экспортирует все три.

**How it works:**

App SDK создает промежутки (`trace_id` + `span_id`). Коллектор получает OTLP (gRPC 4317/HTTP 4318), пакеты, экспорт в Jaeger/Tempo (трассировки), Prometheus (метрики), Loki/ELK (логи). Панели мониторинга Grafana запрашивают эти бэкэнды. Структурированные журналы представляют собой JSON с тем же `trace_id` так что вы можете переходить от промежутка к строкам журнала.

| Сигнал  | Вопрос                                                     | Типичный бэкэнд       |
| ------- | ---------------------------------------------------------- | --------------------- |
| Журналы | Что сказал этот экземпляр?                                 | Локи, CloudWatch, ELK |
| Метрики | Задержка p99 увеличилась?                                  | Прометей + Графана    |
| Следы   | Какой сервис работал медленно при обработке этого запроса? | Егерь, Темпо, Рентген |

**When to use:**

Микросервисы, бессерверные (X-Ray/OTel), SLO. Начните со структурированных журналов JSON + одной информационной панели RED (частота, ошибки, продолжительность) перед полной трассировкой.

#### Common pitfalls

`console.log` строки без идентификатора запроса. Метки высокой мощности (`user_id` по каждому показателю). Трассировка 100% трафика в проде без сэмплирования.

**Pluses and minuses:**

| +                                                  | −                                     |
| -------------------------------------------------- | ------------------------------------- |
| Один прибор, замена поставщиков                    | Коллектор — еще одна движущаяся часть |
| Trace-id связывает журналы с запросом пользователя | Взрывы мощности в метках метрик       |

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

**Thoughts:** Журналы рассказывают истории; показатели показывают тенденции; следы подскажут путь. Коррелировать с `trace_id`.

**Sources:** [opentelemetry.io](https://opentelemetry.io/docs/) · [Прометей](https://prometheus.io/docs/introduction/overview/)

</details>

---

<a name="nginx--deployment"></a>

## Nginx & deployment

<details>
  <summary>Reverse proxy</summary>

### Reverse proxy

**Definition:** Nginx находится перед приложением: он сопоставляет URI запроса с **`location`**, затем либо **обслуживает файл** с диска, либо **проксирует** восходящий поток (Node, Go и т. д.). Он часто завершает работу TLS.

**How it works:**

1. Приходит запрос(`Host`путь, метод). `server` блокировать совпадения `server_name` + `listen`.
2. **`location`** выбирает обработчик: префикс (`/api/`), точный (`=`) или регулярное выражение (`~`).
3. **Статика:** `root` + URI → путь к файловой системе (`root /www/data;` + `/images/a.png` → `/www/data/images/a.png`). Пути/FHS: [Файлы и файловые системы](./8_nodejs_modules.md#files--file-systems).
4. **`try_files`** проверяет файлы по порядку; последний аргумент — это резервный URI, именованное местоположение или `=404`. использование СПА `try_files $uri /index.html` поэтому клиентские маршруты не имеют ошибки 404.
5. **`proxy_pass`** пересылает в восходящий поток. Набор `Host` и `X-Real-IP` поэтому приложение видит исходный клиент.

**When to use:** Реагировать/Вите `dist/` за Nginx; `/api/` к узлу; Заголовки CORS на прокси-сервере, если API является другим источником.

**Pluses and minuses:**

| +                                                | −                                                      |
| ------------------------------------------------ | ------------------------------------------------------ |
| Быстрые статические файлы + одна запись для TLS  | В синтаксисе конфигурации легко ошибиться              |
| Резервный вариант SPA без обслуживания Node HTML | `proxy_pass` косая черта в конце меняет перезапись URI |

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

- URL-адрес карт Nginx → `location` → файл (`root` + `try_files`) или вверх по течению (`proxy_pass`) — именно так сервер «знает, какой ресурс отправить».\*

Тот же шаблон в [10_build.md — Nginx](./10_build.md). Синтаксис конфигурации, FastCGI, systemd → [Конфигурация Nginx](#nginx-configuration). Эквивалент Apache → [Apache HTTP-сервер](#apache-http-server). Семантика HTTP/сжатие/Базовая аутентификация → [6_web_api.md — Веб-сервер](./6_web_api_http.md#web-server).CORS доступен только в браузере → [3_js_browser.md](./3_js_browser_extra.md#cors-same-origin).

**Sources:** [nginx.org — try_files](https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files) · [Отправлять статический контент](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/)

</details>

<details>
  <summary>Nginx configuration</summary>

### Nginx configuration

**Definition:** Директивы в `nginx.conf` (простой `name args;` или заблокировать `{ }`) включите модули и опишите, как главный процесс + воркеры обрабатывают запросы.

**How it works:**

- Гнездо контекстов: `main` → `events` / `http` → `server` → `location`. Директивы внутри `{ }` наследовать этот контекст.
- `listen` + `server_name` выберите виртуальный хост. `location` затем сопоставляет URI → статический файл (`root` + URI) или восходящий поток (`proxy_pass` / `fastcgi_pass`).
- Префикс `location /images/` выигрывает по **самому длинному префиксу**; `location ~ \.png$` это регулярное выражение (`~`). Nginx сначала проверяет префиксы, а затем регулярное выражение.
- `nginx -t` проверяет синтаксис без применения. `nginx -s reload` (HUP): мастер проверяет, запускает новые рабочие процессы, старые рабочие завершают текущие запросы. `nginx -s stop` быстрое отключение; `quit` изящный.
- системный: `systemctl start|stop|reload|restart|enable|status nginx`. **`reload` = конфигурация nginx**, не `daemon-reload` (это перезагружает **файл модуля**).
- Предпочитать `reload` после редактирования конфигурации (сохраняйте живые соединения). `restart` бросает их (остановись, затем начни).

**When to use:**

Узел/Далее `localhost:3000` за доменом; статический HTML; PHP через FastCGI; изображения с диска, остальное в приложение.

#### Common pitfalls

Лечение `systemctl reload nginx` как перезагрузка системного модуля. положить `root` on `location /images/` vs `alias` (соединение путей отличается). HTTPS в проде есть `:443` + сертификаты (генератор конфигурации SSL Mozilla), не только `:80`.

**Pluses and minuses:**

| +                                            | −                                                        |
| -------------------------------------------- | -------------------------------------------------------- |
| Управляемая событиями, дешевая статика + TLS | Легко неправильно прочитать `proxy_pass` тянущийся `/`   |
| Грациозная перезагрузка без простоев         | регулярное выражение `location` в заказе легко ошибиться |

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

**Thoughts:** _Простые директивы заканчиваются `;`; блоки — это контексты. URI → самый длинный `location` → файл или восходящий поток; подать заявку с `-t` затем `reload`, а не жесткий перезапуск._

**See also:** [Обратный прокси](#nginx--deployment) · [HTTP-сервер Apache](#apache-http-server) · [10_build.md — Nginx](./10_build.md)

**Sources:** [nginx.org — Руководство для начинающих](https://nginx.org/en/docs/beginners_guide.html) · [Управление nginx](https://nginx.org/en/docs/control.html)

</details>

<details>
  <summary>Apache HTTP Server</summary>

### Apache HTTP Server

**Definition:** Модульный HTTP-сервер (`httpd`, с 1995 года Apache Software Foundation), настроенный с помощью текстовых директив в `httpd.conf` (и `Include`d файлы), опционально плюс для каждого каталога `.htaccess`.

**How it works:**

- Одна директива на строку; `\` продолжает линию. `#` запускает комментарий длиной **целую строку** (не идущий на той же строке, что и директива). Аргументы заключены в кавычки, если они содержат пробелы.
- **Имена директив не чувствительны к регистру**; аргументы часто чувствительны к регистру. `${VAR}` расширяется `Define` (и обрабатывать env с **before** start). `SetEnv` слишком поздно для `${VAR}` в файле конфигурации.
- В контейнерах используются HTML-теги: `<Directory path>`, `<DirectoryMatch regex>`, `<Files>`, `<FilesMatch>`, `<Location>`, `<LocationMatch>`, `<VirtualHost addr[:port]>`.
- `.htaccess` (имя через `AccessFileName`) читается **при каждом запросе** для этого дерева каталогов. `AllowOverride None` игнорирует это — предпочитаете основную конфигурацию, когда вы управляете сервером.
- Доступ в **2.4** есть `Require` (`mod_authz_core`), а не 2,2 `Order`/`Allow`/`Deny`. Комбинировать с `<RequireAll>` / `<RequireAny>` / `<RequireNone>`.
- Контроль: `httpd -t` or `apachectl configtest`. `httpd -S` сбрасывает vhosts. `apachectl -k stop|graceful-stop|restart|graceful`. системный модуль часто `httpd` (RHEL) или `apache2` (Дебиан). Окна: `httpd.exe -k shutdown|restart` при запуске как служба/консоль.
- Стеки: **AMP/XAMPP** = Apache + MySQL/MariaDB + Perl/PHP/Python для локальной установки.

**When to use:**

Классическая ЛАМПА; правила для каждой папки на общем хостинге; `mod_alias` / `mod_rewrite` сопоставление URL-адресов; `mod_status` at `/status`; PHP через `proxy:fcgi` (php-фпм).

#### Common pitfalls

Копирование `Order Deny,Allow` с 2,2 на 2,4. Предполагается, что имена директив чувствительны к регистру. С использованием `SetEnv` и ожидая `${VAR}` in `httpd.conf`. Уход `Require all granted` on `/server-status` в общедоступном Интернете. `reload` vs `restart`: корректно завершает текущие запросы; перезапуск/`stop` нет.

**Pluses and minuses:**

| +                                              | −                                                                  |
| ---------------------------------------------- | ------------------------------------------------------------------ |
| `.htaccess` без root-доступа (общий хостинг)   | `.htaccess` на каждый запрос медленнее                             |
| Огромная экосистема модулей, виртуальные хосты | Prefork MPM: один поток/процесс на соединение (с учетом Slowloris) |
| Знаком с устаревшими хостами PHP               | Смесь синтаксиса аутентификации 2.2 и 2.4 в старых руководствах    |

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

**Thoughts:** _Контейнеры (`<Directory>` / `<VirtualHost>`) директивы области действия; `.htaccess` является необязательным переопределением для каждого каталога; 2.4 авторизация есть `Require`; тест с `-t`, применить с плавной перезагрузкой._

**See also:** [Конфигурация Nginx](#nginx-configuration) · обратный прокси в [0_system_design.md](./0_system_design.md) · [10_build.md](./10_build.md)

**Sources:** [httpd.apache.org — файлы конфигурации](https://httpd.apache.org/docs/current/configuring.html) · [Остановка и перезапуск](https://httpd.apache.org/docs/current/stopping.html) · [Контроль доступа](https://httpd.apache.org/docs/2.4/howto/access.html)

</details>

---

## AWS & serverless

<details>
  <summary>AWS Lambda</summary>

### AWS Lambda

**Definition:** Бессерверные вычисления — вы загружаете функцию; AWS запускает его в ответ на события и счета за вызов и продолжительность (ГБ-секунды).

**How it works:**

1. Срабатывает **триггер** (шлюз API HTTP, загрузка S3, сообщение SQS, расписание EventBridge, поток DynamoDB).
2. Lambda **создает или повторно использует** среду выполнения (контейнер).
3. **Фаза инициализации** — загрузка кода, запуск среды выполнения, запуск кода вне обработчика (`INIT_START` в журналах).
4. **Фаза вызова** — запуск обработчика с полезной нагрузкой события.
5. **Заморозить или уничтожить** среду после тайм-аута простоя.

| Концепция            | Деталь                                                                                  |
| -------------------- | --------------------------------------------------------------------------------------- |
| **Холодный старт**   | Новая среда = дополнительная задержка (`initDurationMs` в отчете платформы)             |
| **Теплый старт**     | Повторно используемая среда — быстрее                                                   |
| **Тайм-аут**         | Макс. 15 минут на вызов                                                                 |
| **Память**           | 128 МБ – 10 ГБ (больше памяти = больше процессора)                                      |
| **Параллелизм**      | Ограничения аккаунта/региона; зарезервированный параллелизм для каждой функции          |
| **Штат**             | **Без сохранения состояния** — сохраняется в DB/S3/DynamoDB, а не в `/tmp` через вызовы |
| **Переменные среды** | Конфигурация + секреты (используйте диспетчер секретов для конфиденциальных значений)   |

| Триггер         | Вариант использования                      |
| --------------- | ------------------------------------------ |
| API-шлюз        | Серверная часть API REST/HTTP              |
| С3              | Изменение размера изображения при загрузке |
| СКС             | Асинхронная обработка заданий              |
| EventBridge     | Крон (`rate(5 minutes)`)                   |
| социальные сети | Уведомления о разветвлении                 |

```
Client → API Gateway → Lambda → DynamoDB / RDS / S3
                             → SQS (async downstream)
```

Обеспеченный параллелизм, меньшие пакеты, среда выполнения Node 20+, избегание огромных зависимостей, по возможности оставляйте обработчик вне тяжелой инициализации.

Журналы CloudWatch (автоматически), рентгеновская трассировка, структурированные журналы JSON с помощью Lambda Powertools.

_"Использовал Lambda за шлюзом API для конечных точек, управляемых событиями — оплата за запрос, без управления сервером.Следите за холодным запуском, сохраняйте функции без сохранения состояния, переменные окружения для конфигурации, роли IAM для доступа с наименьшими привилегиями к S3/SQS."_

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

**Sources:** [AWS Lambda — Руководство для разработчиков](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) · [Среда выполнения Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtime-environment.html)

</details>

<details>
  <summary>AWS services map (interview)</summary>

### AWS services map (interview)

**Definition:**

Краткая карта сервисов AWS, которые часто спрашивают на собеседованиях по фулстеку/бэкенду.

| Сервис                | Роль                                            |
| --------------------- | ----------------------------------------------- |
| **Лямбда**            | Бессерверные функции                            |
| **API-шлюз**          | Входная дверь HTTP/WebSocket API                |
| **С3**                | Хранилище объектов (файлы, статические ресурсы) |
| **РДС / Аврора**      | Управляемый PostgreSQL/MySQL                    |
| **ДинамоБД**          | Управляемый NoSQL ключ-значение/документ        |
| **SQS**               | Очередь сообщений                               |
| **СС**                | Уведомления о публикации/подписке               |
| **EventBridge**       | Шина событий + планирование                     |
| **CloudWatch**        | Журналы, метрики, сигналы тревоги               |
| **ИАМ**               | Роли, политика – кто что может называть         |
| **Менеджер секретов** | Вращайте и вводите секреты                      |
| **ЭКС/ЭКС**           | Контейнеры, когда Lambda недостаточно           |
| **CloudFront**        | CDN                                             |

**How it works:** Роль IAM, связанная с грантами Lambda `s3:GetObject`, `sqs:SendMessage`и т. д. — никогда не встраивайте в код долгоживущие ключи.

**When to use:** Бессерверный API (API GW + Lambda + DynamoDB); файловый конвейер (триггер S3 → изменение размера Lambda → S3); асинхронные рабочие (SQS → Lambda).

**Sources:** [Документация AWS](https://docs.aws.amazon.com/)

</details>

---

<a name="ssh"></a>

## SSH

<details>
  <summary>SSH (remote login, keys, tunnels)</summary>

### SSH

**Definition:** **SSH** (Secure Shell) — это протокол **прикладного уровня** для зашифрованного удаленного входа в систему, удаленных команд и передачи файлов (`scp` /SFTP) и TCP-туннели. OpenSSH — обычный клиент/сервер. SSH-2 (RFC 4251–4254, Интернет-стандарт с 2006 г.) заменил **Telnet** (открытый текст) и устаревший SSH-1.

**How it works:**

- Клиент общается с `sshd` (TCP по умолчанию **22**). После обмена ключами сеанс шифруется. SSHv2 создан для противодействия пассивному прослушиванию и для аутентификации **хоста**, поэтому наивный **MITM** не может незаметно поменять конечные точки (вы все равно должны проверить ключ хоста при первом подключении).
- **Аутентификация с использованием открытого ключа (предпочтительно):** вы сохраняете **закрытый** ключ; сервер хранит соответствующий **открытый** ключ в `~/.ssh/authorized_keys` (один ключ на строку). Клиент доказывает, что у него есть закрытый ключ; сервер никогда не видит этот закрытый ключ. Алгоритмы: **Ed25519** (по умолчанию для новых ключей), ECDSA, RSA. Асимметричное шифрование: [Шифрование](./7_special_security.md#encryption).
- **Интерактивный пароль/клавиатура:** более слабый (угадывание, фишинг, перебор). Типично для **первого** входа в систему до установки ключа; затем выключите его.- **На основе хоста/"по IP":** OpenSSH `HostbasedAuthentication` (стиль rhosts) слаб. **Список разрешенных IP-адресов** не является идентификацией — NAT и общий IPv4 означают, что за одним адресом может находиться множество пользователей. Используйте ключи, а затем при необходимости ограничьте _кто_ может подключаться (`AllowUsers`, брандмауэр) в качестве глубокоэшелонированной защиты.
- **Не копируйте закрытый ключ** между компьютерами. Только копирование `*.pub`. Защитите закрытый ключ с помощью парольной фразы; использовать `ssh-agent` поэтому вы вводите его один раз за сеанс.

```
# /etc/ssh/sshd_config — then: sudo systemctl restart sshd   (or ssh.service)
PubkeyAuthentication yes
PasswordAuthentication no          # disables password login for everyone
PermitRootLogin prohibit-password  # OpenSSH default: root by key only, not password
# PermitRootLogin no               # blocks ALL root SSH (keys too) — use a sudo user instead
```

`PermitRootLogin no` **не** «отключить пароль для root». Он полностью запрещает root-доступ через SSH. Чтобы сохранить root+ключи, но удалить пароли, используйте `prohibit-password` плюс `PasswordAuthentication no`.

сгенерируйте специальный ключ Ed25519, вставьте `id_ed25519.pub` в разделе **Настройки → Ключи SSH и GPG → Новый ключ SSH** или `gh ssh-key add ~/.ssh/id_ed25519.pub`. Действия со стороны Git: [Git — настройка SSH](./0_git.md#ssh-setup).

**When to use:**

оболочки VPS/хостинга; Git-пульты (`git@github.com:…`); `scp`/`sftp`; местный (`-L`) / удаленный (`-R`) / НОСКИ (`-D`) переадресация. В Kubernetes вы применяете YAML — вы не подключаете SSH к модулям, как обычный путь развертывания ([Pod vs Deployment](#pod-vs-deployment-vs-service)).

#### Common pitfalls

Лечение `id_rsa` как современное значение по умолчанию. Движущийся `/tmp/*.pub` **на** `authorized_keys` (стирает другие клавиши). Доступен для записи во всем мире `~/.ssh`. Делимся **закрытым** ключом. Параметр `PermitRootLogin no` до того, как появится пользователь + ключ sudo (блокировка). Пропуск проверки ключа хоста.

**Pluses and minuses:**

| +                                          | −                                                                                                                        |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| Зашифрованная оболочка, файлы, туннели     | Хост-ключ TOFU: MITM при первом подключении все равно может обмануть вас, если вы пропустите проверку отпечатков пальцев |
| Ключ аутентификации: нет пароля на проводе | Свободный `~/.ssh` режимы → sshd **игнорирует** `authorized_keys` (`StrictModes`)                                        |
| Ed25519 клавиши короткие и прочные         | Root + пароль в Интернете — мгновенная цель для перебора                                                                 |

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

**Thoughts:** _SSH шифрует сеанс; личность — это закрытый ключ, который сервер никогда не хранит — помещайте в него только открытую половину `authorized_keys`, затем отключите пароли._

**Sources:** [OpenSSH ssh(1)](https://github.com/openssh/openssh-portable/blob/master/ssh.1) · [sshd(8)](https://github.com/openssh/openssh-portable/blob/master/sshd.8) · [RFC 4251](https://www.rfc-editor.org/rfc/rfc4251) · [GitHub — создание ключа SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) · [GitHub — добавление ключ](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/добавление-нового-ssh-ключа-к-вашему-github-аккаунту)

</details>

---

<a name="unix-process-management"></a>

## Unix process management

<details>
  <summary>PID, jobs, nice, top, ps, kill</summary>

### PID, jobs, nice, top, ps, kill

**Definition:** В Unix-подобных системах **процесс** — это работающая программа, идентифицируемая **PID** (идентификатором процесса). Планировщик ОС, оболочка управления заданиями и такие инструменты, как `ps`/`top`/`kill` используйте этот идентификатор для проверки, приостановки, возобновления, изменения приоритетов или сигнализации процесса. Планировщик, контекст, прерывания, таблица сигналов POSIX: [Процессы, многозадачность и прерывания](./0_system_design_core.md#processes-multitasking).

**How it works:**

- **Windows:** Диспетчер задач (графический интерфейс). **Unix:** CLI плюс управление заданиями — оболочка отслеживает _задания_ текущего терминала отдельно от общесистемных PID.
- **Передний план против фона** — это _управление заданиями_ (ждёт ли терминал команду?), а не удобство процессора. На переднем плане: оболочка ждет; вы не можете ввести новую команду, пока она не завершится (или вы не приостановите ее). Справочная информация: добавить `&`; оболочка печатает `[job_number] PID` и возвращает приглашение.
- **Работа против PID:** `[1]` это индекс задания в _этом_ сеансе оболочки. PID является глобальным. `jobs` список вакансий(`+` = ток, `-` = предыдущий). `fg` / `bg` взять спецификацию на работу(`%1`), а не PID.
- **Приостановить/продолжить:** Ctrl+Z отправляет **SIGTSTP** (перехватываемую остановку с терминала). Статус становится `suspended` / `Stopped`. `bg` продолжает его в фоновом режиме; `fg` (без аргументов) возвращает текущее задание (`+`) на передний план. **SIGSTOP** также останавливает процесс, но не может быть перехвачен (в отличие от SIGTSTP).
- **Красота (приоритет процессора):** Linux `nice` значения варьируются от **-20** (максимальное время процессора) до **19** (минимальное). Меньшее число = более высокий приоритет. Непривилегированные пользователи могут только _повысить_ качество (сделать себя лучше/медленнее), а не захватить больше процессора, чем другие.
- **Осмотр:** `top` — работающий процессор, оперативная память, своп, все процессы (`q` покидать, `?` помощь). Linux часто использует `htop` (ncurses, кроссплатформенность, включая macOS). `ps` = статус процесса: `ps -A` (то же самое, что `-e`) все процессы; `ps -Af` полноформатный листинг. PID указан в столбце PID.
- **Сигналы:** `kill PID` по умолчанию отправляет **SIGTERM** (15) — перехватываемый, используется для корректного выхода. `kill -9 PID` / `kill -KILL PID` отправляет **SIGKILL**, который **нельзя перехватить, заблокировать или проигнорировать**. `sudo` нужен только в том случае, если вы не являетесь владельцем процесса — не потому, что `-9` особенный.

**When to use:**

Длинный `ping` / регистрируется в фоновом режиме, пока вы продолжаете использовать оболочку. Архивируйте большое дерево с низкой точностью, чтобы БД сохраняла ЦП. Найдите вышедший из-под контроля PID узла с помощью `ps`/`top` и `kill` это. Путь развертывания: сначала SIGTERM (K8s / `docker stop`), SIGKILL только после льготного периода — [Node SIGTERM](./15_devops.md#unix-process-management).

#### Common pitfalls

Вызов переднего/фонового плана «приоритета процесса» (т.е. `nice`). Предполагая значение по умолчанию `kill` это SIGKILL — это SIGTERM. Полагая, что программа должна «обработать SIGKILL», чтобы умереть — SIGKILL принудительно выполняется ядром. С использованием `kill -9` первые и протекающие соединения. Перепутал идентификатор вакансии `[1]` с ПИД. `renice -n` в util-linux по умолчанию это **абсолютная** вежливость, а не дельта (требуется относительный режим POSIX). `POSIXLY_CORRECT` or `--relative`).

**Pluses and minuses:**

| +                                                          | −                                                                        |
| ---------------------------------------------------------- | ------------------------------------------------------------------------ |
| PID — это дескриптор каждого инструмента эксплуатации      | Повторное использование PID: поздно `kill` может запустить новый процесс |
| SIGTERM позволяет приложению сливать работу                | SIGKILL пропускает очистку (блокировки, временные файлы, соединения)     |
| Управление заданиями осуществляется по терминалам и быстро | `jobs`/`fg`/`bg` не видеть процессы, запущенные где-то еще               |

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

**Thoughts:** _Идентификация с помощью PID; контроль работы (`&` / `fg` / `bg` / Ctrl+Z) — только эта оболочка; `nice` — доля ЦП; по умолчанию `kill` это СИГТЕРМ — СИГКИЛЛ (`-9`) невозможно обработать._

**Sources:** [kill(1)](https://man7.org/linux/man-pages/man1/kill.1.html) · [signal(7)](https://man7.org/linux/man-pages/man7/signal.7.html) · [nice(1)](https://man7.org/linux/man-pages/man1/nice.1.html) · [renice(1)](https://man7.org/linux/man-pages/man1/renice.1.html) · [ps(1)](https://man7.org/linux/man-pages/man1/ps.1.html) · [htop](https://htop.dev/)

</details>

<details>
  <summary>Firewall / packet filter</summary>

### Firewall / packet filter

**Definition:** **Брандмауэр** (фильтр пакетов) отбрасывает, принимает или отклоняет трафик по IP, порту и протоколу. В Linux фильтр работает в ядре; `iptables` / `nftables` только установить правила.

**How it works:**

- Цепочки: **INPUT** (к этому хосту), **OUTPUT** (от этого хоста), **FORWARD** (транзит, например маршрутизатор). Правила совпадают по порядку; первое попадание победит, если вы не добавите (`-A`), и более поздние правила никогда не выполняются для этого пакета.
- Типичный веб-хостинг: разрешить 22/80/443, остальные удалить. Эхо ICMP может быть отклонено, поэтому `ping` терпит неудачу, хотя HTTP все еще работает.
- Сокеты и порты хоста: [6_web_api.md — Порты](./6_web_api_apps.md#ports-sockets--network-services). Путь запроса: [Как работает Интернет](./6_web_api_core.md#internet).

**When to use:**

Скрыть порт узла от Интернета (привязать `127.0.0.1` **и** брандмауэр). Блочные сканеры. Не используйте случайный `iptables` однострочник на удаленном ящике SSH без правила разрешения-22 — вы можете заблокировать себя.

#### Common pitfalls

`- j REJECT` (неверный). Фильтрация ICMP и предположение, что хост находится «автономно» для HTTP. Забыл IPv6 (`ip6tables` / `nft` инет).

**Pluses and minuses:**

| +                                | −                                                          |
| -------------------------------- | ---------------------------------------------------------- |
| Дешевый контроль на уровне хоста | Легко заблокировать SSH                                    |
| Скорость ядра                    | `iptables` vs `nftables` против облачного SG перекрываются |

**Examples:**

```bash
# Reject inbound ping (ICMP echo-request). No space in -j.
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j REJECT
sudo iptables -L

# Modern Linux prefers nftables; cloud VMs also use security groups — both must allow 443.
```

**Thoughts:** _Ядро фильтрует пакеты; CLI только пишет правила — разрешите SSH, прежде чем экспериментировать._

**Sources:** [iptables(8)](https://man7.org/linux/man-pages/man8/iptables.8.html) · [nftables](https://wiki.nftables.org/) · [Doka — Работа с сетью](https://doka.guide/tools/network/)

</details>

---

<a name="interview-practice"></a>

## Interview Practice

### Question index

| Тема                                                | Ответ                                                   |
| --------------------------------------------------- | ------------------------------------------------------- |
| Обратный прокси-сервер Nginx / какой файл отправить | Вопросы и ответы                                        |
| Конфигурация Nginx и Apache                         | Вопросы и ответы                                        |
| Докер против виртуальной машины                     | Вопросы и ответы                                        |
| Жизненный цикл контейнера                           | [10_build.md](./10_build_docker.md#container-lifecycle) |
| Этапы конвейера CI/CD                               | Вопросы и ответы                                        |
| Сине-зеленый / подвижное развертывание              | Вопросы и ответы                                        |
| Переменные среды и секреты                          | Вопросы и ответы                                        |
| Проверка здоровья                                   | Вопросы и ответы                                        |
| AWS Лямбда                                          | Вопросы и ответы                                        |
| Карта сервисов AWS                                  | Вопросы и ответы                                        |
| Наблюдаемость (логи/трассировки/метрики)            | Вопросы и ответы                                        |
| Pod против развертывания против сервиса             | Вопросы и ответы                                        |
| SSH-ключи против пароля                             | Вопросы и ответы                                        |
| PID/убийство против убийства -9                     | Вопросы и ответы                                        |
| Брандмауэр / iptables                               | Вопросы и ответы                                        |

---

## Questions & Answers

<details>
  <summary>How does the server know which resource to send?</summary>

### How does the server know which resource to send?

**Definition:** HTTP-сервер сопоставляет **метод + хост + путь** с обработчиком: статическим файлом, восходящим обратным прокси-сервером или маршрутом приложения.

**How it works:**

1. TCP/TLS принимает соединение; HTTP анализирует строку запроса (`GET /app/settings HTTP/1.1`) и `Host`.
2. **Виртуальный хост** (`server_name`) выбирает конфигурацию сайта.
3. **Нгинкс `location`** соответствует URI. Правила самых длинных префиксов/регулярных выражений определяют блоки.
4. **Статика:** `root` or `alias` + URI → путь к диску; `try_files $uri $uri/ /index.html` обслуживает файл, если он существует, иначе оболочка SPA.
5. **API:** `location /api/` + `proxy_pass` пересылает на узел; Маршрутизатор узла (Express/Fastify) затем соответствует `/users/:id`.
6. Заголовки ответов включают `Content-Type` (из расширения или приложения). Сюда можно добавить заголовки CORS — они не применяются к Postman.

**When to use:** Производство SPA + API на одном домене; `index.html` запасной вариант для маршрутизации на стороне клиента.

**Thoughts:** _Браузер запрашивает URL-адрес; Нгинкс `location` + `try_files`/`proxy_pass` (или маршрутизатор приложения) выберите байты для возврата._

**See also:** [Обратный прокси](#nginx--deployment) · [HTTP-сервер Apache](#apache-http-server)

**Sources:** [nginx.org — местоположение](https://nginx.org/en/docs/http/ngx_http_core_module.html#location)

</details>

<details>
  <summary>Nginx vs Apache — how do you configure them?</summary>

### Nginx vs Apache — how do you configure them?

**Definition:** Оба являются HTTP-серверами/обратными прокси-серверами. Nginx использует `server`/`location` in `nginx.conf`; Apache использует `httpd.conf` контейнеры (`<VirtualHost>`, `<Directory>`) и необязательно `.htaccess`.

**How it works:** Nginx: воркеры, управляемые событиями, `nginx -t` затем `reload` (HUP, сохраняйте связи). Apache: MPM (событие/работник/префорк), `httpd -t`, `apachectl -k graceful`. Доступ к Apache 2.4 есть `Require`, а не 2,2 `Order`/`Allow`. У Nginx нет `.htaccess`; вы редактируете основную конфигурацию (или включенные фрагменты).

**When to use:** Nginx перед Node/SPA; Apache на LAMP/общем хостинге, который позволяет только `.htaccess`.

**Thoughts:** _Nginx = контексты + `location`; Apache = контейнеры с тегами + необязательно `.htaccess`; всегда проверка синтаксиса, а затем плавная перезагрузка._

**See also:** [Конфигурация Nginx](#nginx-configuration) · [HTTP-сервер Apache](#apache-http-server)

</details>

<details>
  <summary>Docker vs virtual machine?</summary>

### Docker vs virtual machine?

Виртуальные машины виртуализируют оборудование + полную ОС. Контейнеры совместно используют ядро ​​хоста, изолируют процессы/файловую систему — более легкий и быстрый запуск. Используйте контейнеры для упаковки приложений; Виртуальные машины для строгой изоляции или разные ОС. Файл Docker: [10_build.md](./10_build_docker.md#dockerfile).

</details>

<details>
  <summary>Docker container lifecycle?</summary>

### Docker container lifecycle?

Создать → запустить → (пауза) → остановить (`SIGTERM` затем `SIGKILL`) → удалить. Полный ответ: [10_build.md — Жизненный цикл контейнера](./10_build_docker.md#container-lifecycle).

</details>

<details>
  <summary>Blue-green vs rolling deployment?</summary>

### Blue-green vs rolling deployment?

**Синий-зеленый** — две полные среды; мгновенно переключать трафик; легкий откат. **Rolling** — замена экземпляров постепенно; меньше инфрадупликации; более медленный откат.

</details>

<details>
  <summary>Secrets management?</summary>

### Secrets management?

Никогда не раскрывайте секретов. Используйте секретное хранилище, хранилище или переменные среды платформы CI (Vercel/GitHub Secrets). Внедрить во время выполнения; вращать клавиши; отдельный dev/staging/prod.

</details>

<details>
  <summary>Health check endpoint?</summary>

### Health check endpoint?

`GET /health` возвращает 200, если приложение + зависимости в порядке. Балансировщик нагрузки удаляет неработоспособные экземпляры. Слегка проверяйте подключение к БД — не перегружайте БД при каждом зондировании.

</details>

<details>
  <summary>What is AWS Lambda and how did you use it?</summary>

### What is AWS Lambda and how did you use it?

**Definition:** Бессерверная среда выполнения функций — инициируется событием, автоматическое масштабирование, оплата за вызов.

**How it works:** Триггер → среда выполнения (холодная/теплая) → запуск обработчика → ответ или асинхронный побочный эффект. Лица без гражданства; настройка через env vars; разрешения через роль IAM.

**Typical uses:** Конечные точки шлюза API, обработка событий S3, потребители SQS, запланированные задания cron (EventBridge).

**What to mention in interview:**

- Осведомленность о холодном запуске и оптимизация размера пакета.
- Наименьшие привилегии IAM (функциональная роль, а не ключи доступа в коде).
- CloudWatch для журналов и сигналов тревоги.
- Сопряжение с SQS для повторных попыток и DLQ.
- Когда **не** использовать Lambda — длительные процессы, постоянные веб-сокеты в огромных масштабах, всегда «горячие» API с очень низкой задержкой (рассмотрим ECS).

**Sources:** [AWS Lambda DG](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

</details>

<details>
  <summary>Common AWS services for a backend?</summary>

### Common AWS services for a backend?

API Gateway + Lambda + DynamoDB/RDS для API; S3 для файлов; SQS/SNS для асинхронности; CloudWatch для операций; IAM для безопасности. CloudFront для CDN перед статическими ресурсами/ресурсами API.

См. [Карту сервисов AWS](#aws-services-map-interview).

</details>

<details>
  <summary>Logs vs traces vs metrics?</summary>

### Logs vs traces vs metrics?

**Definition:** Журналы = события. Метрики = совокупные показатели с течением времени. Трассировки = один запрос между службами (`trace_id`). OpenTelemetry экспортирует все три; Prometheus/Grafana для метрик; Jaeger/Tempo для следов.

**Thoughts:** Коррелировать с `trace_id` — не используйте три инструмента отдельно.

**See also:** [Журналы, трассировки, метрики](#observability)

</details>

<details>
  <summary>Pod vs Deployment vs Service?</summary>

### Pod vs Deployment vs Service?

**Definition:** **Pod** — работающие контейнеры. **Развертывание** сохраняет N реплик и развертывает новые образы. **Сервис** — это стабильный DNS/IP перед сменой подов.

**Thoughts:** Вы применяете «Развертывание + Сервис»; вы не подключаетесь к подам по SSH.

**See also:** [Под, Сервис, Развертывание](#kubernetes-interview-basics)

</details>

<details>
  <summary>SSH keys vs password?</summary>

### SSH keys vs password?

**Definition:** SSH может аутентифицироваться с помощью **пары ключей** (клиент доказывает, что у него есть закрытый ключ; у сервера есть открытый ключ). `authorized_keys`) или с помощью **пароля**.

**How it works:** Ed25519 (или RSA) через `ssh-keygen`; установить `.pub` с `ssh-copy-id`. Затем `PasswordAuthentication no`. `PermitRootLogin prohibit-password` разрешает только корневые **ключи**; `PermitRootLogin no` полностью блокирует корневой SSH.

**Thoughts:** Сначала ключи, затем отключите пароли — полные примечания: [SSH](#ssh).

</details>

<details>
  <summary>What is a PID? kill vs kill -9?</summary>

### What is a PID? kill vs kill -9?

**Definition:** **PID** — идентификатор процесса. `kill PID` отправляет **SIGTERM** (изящный, уловимый). `kill -9 PID` отправляет **SIGKILL**, который применяется ядром и процесс не может обработать.

**Thoughts:** Предпочитаю SIGTERM; SIGKILL — это последнее средство. Полные примечания: [Управление процессами Unix](#unix-process-management).

</details>

<details>
  <summary>Firewall / iptables?</summary>

### Firewall / iptables?

**Definition:** Фильтр пакетов ядра: сопоставьте IP/порт/протокол для INPUT/OUTPUT/FORWARD и ACCEPT/DROP/REJECT. `iptables`/`nftables` напишите правила.

**How it works:** Пример — отклонить пинг, не трогая HTTP: `iptables -A INPUT -p icmp --icmp-type echo-request -j REJECT`. Оставьте SSH разрешенным. Полные примечания: [Брандмауэр/фильтр пакетов](#firewall--packet-filter).

**Thoughts:** Фильтр в ядре; CLI — это всего лишь конфигурация.

</details>

---

### Tasks

### Task 1 — Explain failed deploy rollback

Новая версия вылетает при запуске. Действия по восстановлению сервиса?

<details><summary>Answer</summary>

Вернитесь к предыдущему изображению/тегу в оркестраторе или повторно разверните последний заведомо исправный артефакт; используйте флаги функций, чтобы отключить плохую функцию без полного отката, если это применимо.

</details>

### Task 2 — Why multi-stage Docker build?

<details><summary>Answer</summary>

Меньший рабочий образ (без devDependency, без исходного кода), меньше поверхностей для атак, более быстрое развертывание.

</details>

---

**полезные ссылки**:

- https://selectel.ru/blog/courses/network-protocols/
