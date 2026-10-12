# Build — Docker

Изображения, контейнеры, Compose и жизненный цикл контейнера.

[ ← Вернуться в хаб](./10_build.md)

### Related docs

- [Сборка — CI](./10_build_ci.md) · [Сборка — Bundlers](./10_build_bundlers.md) · [Сборка — Docker](./10_build_docker.md) · [Сборка — Infra](./10_build_infra.md) · [Сборка — Интервью](./10_build_interview.md)

### Sections

- [Docker](#docker)

---

## Docker

<details>
  <summary>Docker / containerization</summary>

### Docker / containerization

**Definition:** Платформа, которая упаковывает приложение **с его окружением** (битами ОС, средой выполнения, параметрами, конфигурацией) в **контейнер**, поэтому оно одинаково работает на любом хосте Docker — «если оно запускается здесь, оно работает где угодно».

**How it works:**

- **Docker Engine** — это стек клиент-сервер: долгоработающий демон. `dockerd` + **API движка** + CLI `docker`. Машина, на которой работает демон, является **узлом Docker**. CLI/Dashboard общается с демоном через API; демон создает образы, контейнеры, сети, тома.
- **Docker Desktop** (Mac/Windows, дополнительно в Linux) включает в себя Engine, CLI, Compose, Dashboard, вход в Hub. В Mac/Windows контейнеры Linux запускаются внутри **ВМ Linux** — вы не запускаете `dockerd` изначально в ядре хоста.
- `docker run` обычно: вытащить изображение (если оно отсутствует) → создать контейнер → присоединить записываемый слой + сеть → запустить PID 1. Изображение — класс; контейнер — экземпляр.
- Проверьте установку: `docker --version`. Первое изображение урока из текущей документации:

```bash
docker pull docker/welcome-to-docker
docker run --detach --name welcome --publish 8080:80 docker/welcome-to-docker
# then http://localhost:8080  —  docker rm --force welcome
```

**When to use:**

Развертывание сервера; CI-изображения; интерфейс — бандл/линт/тест, конвейеры ресурсов, попробуйте CMS/фреймворк без установки Node/DB/nginx на хост, воспроизводимые демо, редактор + цепочки инструментов в контейнере.

#### Common pitfalls

Привязка приложения к `127.0.0.1` внутри контейнера (хост не может до него добраться — слушайте `0.0.0.0`). Лечение `EXPOSE` как опубликовать(`-p` все еще требуется). Скапливать остановившиеся контейнеры (`docker ps -a`), пока имена не столкнутся.

**Pluses and minuses:**

| +                                                            | −                                                                                                   |
| ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| Та же среда на ноутбуке, CI, prod                            | Сети, тома и виртуальные машины настольных компьютеров требуют обучения                             |
| Секунды до запуска по сравнению с полной виртуальной машиной | Общее ядро ​​— не виртуальная машина; собственные модули должны соответствовать **контейнеру** Arch |
| Один процесс на контейнер; Составлять для стеков             | Раздувание изображения без многоступенчатости + `.dockerignore`                                     |
| Рецепт в git (Dockerfile)                                    | Сложнее отлаживать, чем локальный процесс                                                           |

**Examples:**

```bash
docker run --name some-wordpress -p 8080:80 -d wordpress
docker run --name some-drupal -p 8080:80 -d drupal
# bind-mount host folder, then run ImageMagick in the container:
docker run --rm -v "$PWD":/imgs dpokidov/imagemagick /imgs/in.png -resize 100x100 /imgs/out.png
```

**Thoughts:** Отправьте **среду вместе с приложением**. Движок = демон + API + CLI; изображение — это шаблон, контейнер — работающий (или остановленный) экземпляр.

**Sources:** [Docker Engine](https://docs.docker.com/engine/) · [Обзор](https://docs.docker.com/get-started/docker-overview/) · [Запустить приложение](https://docs.docker.com/get-started/tutorials/run-an-app/)

</details>

<details>
  <summary>Container vs virtual machine</summary>

### Container vs virtual machine

| | Контейнер                  | Виртуальная машина            |
| | -------------------------- | ----------------------------- | -------------------------------- |
| | **Изоляция**               | Уровень процесса (общее ядро) | Полная ОС на виртуальную машину  |
| | **Стартап**                | Секунды                       | Минуты                           |
| | **Использование ресурсов** | Легкий                        | Heavy (полная версия ОС)         |
| | **Мобильность**            | Высокий (с арочной спичкой)   | Высокий                          |
| | **Сценарий использования** | Микросервисы, CI              | Устаревшие приложения, разные ОС |

**Sources:** [Docker — Контейнеры](https://www.docker.com/resources/what-container/)

</details>

<details>
  <summary>Core concepts</summary>

### Core concepts

| Концепция        | Определение                                                                                                                                                                                          |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Изображение**  | Многоуровневый шаблон только для чтения (объединение различий). Создан из Dockerfile или извлечен из реестра. Слои повторно используются в изображениях.                                             |
| **Контейнер**    | Экземпляр изображения: пространства имен + cgroups + верхний слой **доступный для записи**. Эфемерный по умолчанию.                                                                                  |
| **Объем**        | Постоянный каталог, управляемый Docker (а не [bind mount](#bind-mounts)). Выживает `docker rm`. Подробности: [Тома](#тома).                                                                          |
| **Сеть**         | Виртуальная локальная сеть между контейнерами и/или хостом. Compose создает определяемый пользователем мост по умолчанию.                                                                            |
| **Реестр**       | Удаленное хранилище изображений. По умолчанию: **Docker Hub**. Также GHCR, ECR, GCR/Artifact Registry, ACR, Yandex CR и т. д. Предпочитайте **официальные** изображения (`node`, `python`, `nginx`). |
| **Докерфайл**    | Рецепт образа(`FROM` затем слои).                                                                                                                                                                    |
| **Создать файл** | YAML для многоконтейнерного приложения.                                                                                                                                                              |

**Network drivers (Linux Engine):**

| Водитель      | Роль                                                                                                                                         |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **мост**      | По умолчанию. Изолированная сеть; контейнеры общаются по IP (определяемый пользователем мост: также по **имени**). Типичное приложение ↔ БД. |
| **хозяин**    | Нет сетевого пространства имен — контейнер использует сеть хоста (Linux).                                                                    |
| **нет**       | Никакой сети.                                                                                                                                |
| **наложение** | Мультихост (Swarm). Также **ipvlan** / **macvlan** для сетевых карт физического вида.                                                        |

```mermaid
flowchart LR
  dockerfile[Dockerfile] -->|docker build| image[Image]
  registry[Registry] -->|docker pull| image
  image -->|docker run| container[Container]
  volume[Volume] <-->|mount| container
  net[Network] <--> container
```

**Thoughts:** Стек слоев изображения (объединение FS); только верхний слой контейнера доступен для записи. Сохранение данных в томах, проводные услуги с помощью мостовой сети.

**Sources:** [Концепции](https://docs.docker.com/get-started/docker-concepts/) · [Сеть](https://docs.docker.com/engine/network/)

</details>

<details>
  <summary>Dockerfile</summary>

### Dockerfile

**Definition:** Текстовый файл последовательных инструкций, который Docker Build использует в качестве рецепта для **образа** — ОС, среда выполнения, файлы, среда выполнения и процесс, который запускается при запуске контейнера.

**How it works:**

- Формат: `# comment` затем `INSTRUCTION arguments` (инструкции не чувствительны к регистру; принято ЗАГЛАВНЫЕ РЕГИСТРЫ). Необязательная первая строка: `# syntax=docker/dockerfile:1` (интерфейс BuildKit).
- Dockerfile должен начинаться с `FROM` (только глобальный `ARG` / директивы parser могут предшествовать этому). Каждый `FROM` запускает **стадию сборки**.
- Изображение представляет собой **объединение слоев** (составленных различий). `FROM`, `RUN`, `COPY`, и `ADD` создавать слои файловой системы; другие инструкции в основном устанавливают метаданные. Неизмененные слои повторно используются из кэша — сначала помещайте редко меняющиеся шаги (`package*.json` + `npm ci` до `COPY . .`).
- Слои можно использовать в разных изображениях; Docker извлекает только те слои, которых еще нет на хосте.
- **Контекст** — набор файлов. `COPY`/`ADD` можно увидеть (обычно `.`). Исключите мусор с помощью `.dockerignore` такие секреты и `node_modules` никогда не вникайте в контекст.
- Строить: `docker build [OPTIONS] PATH | URL | -`. Затем `docker run` запускает **контейнер** (записываемый слой + процесс) из образа.
- Официальное руководство: одна забота на контейнер, отсутствие неиспользованных упаковок, маркировка базы, многострочная сортировка. `RUN` аргументы, используйте `.dockerignore`, многоступенчатый для прода, `--no-cache` / `--pull` когда вам нужна чистая перестройка (кэш предназначен для повторного использования, а не для каждого слоя навсегда).

`CMD ["node","app.js"]` (массив JSON, двойные кавычки) имеет PID 1 и получает `SIGTERM`. Форма оболочки `CMD node app.js` проходит через `/bin/sh -c`, поэтому оболочка имеет PID 1, и приложение может пропускать сигналы остановки.

Рассматривать изображение как двоичное → `ENTRYPOINT` (исполнительный) + `CMD` в качестве аргументов по умолчанию. Относитесь к нему как к обычному ящику с процессом по умолчанию → `CMD` только (легко переопределить). `docker run image sh` заменяет `CMD`, а не исполнительная форма `ENTRYPOINT`.

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

`.dockerignore` живет в **корне контекста** (подобно gitignore). Типичное игнорирование узла: `node_modules/`, `dist/`, `.git`, `.env`. С `-f myapp.Dockerfile`, BuildKit также проверяет `myapp.Dockerfile.dockerignore` рядом с этим файлом, затем возвращается к `.dockerignore`.

#### Multi-stage

На этапе Builder есть компиляторы/`npm`; последний этап копирует только артефакты. Промежуточные этапы отсутствуют в поставляемом образе.

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

Воспроизводимые образы Node/nginx; артефакты CI; Инструменты CLI, упакованные как `ENTRYPOINT`; многоэтапный SPA (сборка на Node, обслуживание с помощью nginx).

#### Instructions

| Инструкция    | Роль                                                                                                                                                                                                       |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `FROM`        | Базовое изображение/новый этап. Прикрепить тег (`node:20-alpine`), не голый `node` (`latest`). `AS name` для многоступенчатого.                                                                            |
| `RUN`         | Команда оболочки **во время сборки** — установить пакеты, скомпилировать. Цепь с `&&` в одном `RUN` разрезать слои.                                                                                        |
| `COPY`        | Скопируйте из **контекста сборки** в изображение. Предпочитаю это. `COPY --from=stage` копии с другого этапа/изображения.                                                                                  |
| `ADD`         | Нравиться `COPY`, а также удаленные URL-адреса и автоматическое извлечение локальных архивов — легко использовать неправильно; избегайте, если вам это не нужно.                                           |
| `WORKDIR`     | Рабочий каталог на потом `RUN`/`COPY`/`CMD`/`ENTRYPOINT`. Создается, если отсутствует.                                                                                                                     |
| `ENV`         | Переменные Env в **образе** (доступны во время выполнения и на последующих этапах сборки).                                                                                                                 |
| `ARG`         | **Время сборки** переменные (`docker build --build-arg KEY=val`). Не в работающем контейнере, если вы не скопируете в `ENV`. Видно в `docker history` — не для секретов.                                   |
| `USER`        | UID/GID на потом `RUN`/`CMD`/`ENTRYPOINT`. Сначала создайте пользователя; чиновник `node` изображение уже есть `USER node`.                                                                                |
| `EXPOSE`      | **Документирует** порт прослушивания. **Не** публикует на хосте. Опубликовать с помощью `docker run -p host:container` или Написать `ports`. `-P` отображает все `EXPOSE`d порты на случайные порты хоста. |
| `VOLUME`      | Объявляет точку монтирования (анонимный том, если вы не монтируете что-либо при запуске). Путь к хосту **не** указан в файле Docker — [Volumes](#volumes).                                                 |
| `CMD`         | Команда по умолчанию. Переопределено аргументами после `docker run IMAGE …`. Только последний `CMD` имеет значение.                                                                                        |
| `ENTRYPOINT`  | Основной исполняемый файл. `docker run` аргументы **добавлены** (форма exec). Используйте с `CMD` для флагов по умолчанию.                                                                                 |
| `HEALTHCHECK` | Как демон проверяет живость (`CMD` внутри контейнера).                                                                                                                                                     |
| `LABEL`       | Метаданные изображения. `STOPSIGNAL` / `SHELL` — стоп-сигнал и оболочка по умолчанию.                                                                                                                      |

#### Common pitfalls

- `EXPOSE 8080` ≠ `http://localhost:8080` - тебе все еще нужно `-p 8080:8080` (и приложение должно прослушивать `0.0.0.0`).
- `sudo` in `RUN` — официальные базы уже запускаются от root; `sudo` часто не устанавливается.
- `USER alice` не создавая `alice` → сборка/запуск завершается неудачно.
- Запекание паролей в `ENV`/`ARG`; используйте среду выполнения, секреты Compose или BuildKit `--secret`, нет `docker history`-видимые аргументы.
- `COPY . .` без `.dockerignore` (отправляет `.git`, `node_modules`, `.env`).
- Форма оболочки `ENTRYPOINT` игнорирование `CMD` и `docker run` аргументы; приложение не PID 1 → `docker stop` тогда ждет `SIGKILL`.
- Откреплено `FROM node` и ненужные пакеты (редакторы в рабочем образе).

**Pluses and minuses:**

| +                                                 | −                                                     |
| ------------------------------------------------- | ----------------------------------------------------- |
| Одинаковая среда выполнения на ноутбуке и сервере | Неправильный порядок слоев → медленное восстановление |
| Кэш + многоэтапное сохранение размера изображений | `latest` теги и огромное раздувание контекста CI      |
| Версия рецепта доступна в приложении              | Секреты в `ARG`/`ENV` утечка в историю изображений    |

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

**Thoughts:** Dockerfile = многоуровневый рецепт; сохраняйте контейнеры **без сохранения состояния** (данные в томах), одна проблема на образ, тонкий финальный этап.

**Sources:** [Справочник по Dockerfile](https://docs.docker.com/reference/dockerfile/) · [Рекомендации](https://docs.docker.com/build/building/best-practices/) · [Контекст сборки](https://docs.docker.com/build/concepts/context/)

</details>

<details>
  <summary>Container lifecycle</summary>

### Container lifecycle

**Definition:** Контейнер — это **работающий или остановленный экземпляр** образа: пространства имен + контрольные группы + доступный для записи слой. Изображение остается доступным только для чтения; ПИД 1 обычно `ENTRYPOINT` + `CMD`.

**How it works:**

Последовательность интервью: создать → запустить (выполнить) → необязательная пауза/возобновить паузу → остановить или уничтожить → перезапустить или удалить.

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

| Команда                    | Эффект                                                            |
| -------------------------- | ----------------------------------------------------------------- |
| `docker create`            | Выделить из изображения; не работает                              |
| `docker start`             | Запустить созданный или закрытый контейнер                        |
| `docker run`               | `create` + `start` (часто `--rm` удалить при выходе)              |
| `docker pause` / `unpause` | Заморозить/возобновить (cgroup замораживатель)                    |
| `docker stop`              | `SIGTERM` (or `STOPSIGNAL`), затем `SIGKILL` после тайм-аута      |
| `docker kill`              | Немедленный сигнал (`SIGKILL` по умолчанию)                       |
| `docker restart`           | Остановись, потом начни                                           |
| `docker rm`                | Удалить **остановленный** контейнер (`-f` убивает, потом удаляет) |

Политики перезапуска (`--restart` / Написать `restart:`): `no` (по умолчанию), `on-failure[:n]`, `always`, `unless-stopped`. Руководство `stop` подавляет `always` пока вы не начнете все сначала. Вышедшие контейнеры остаются в `docker ps -a` — конфликт имен означает, что старый остановленный контейнер все еще существует.

**When to use:**

Интервью «опишите жизненный цикл»; К8с `SIGTERM` + Грамотное завершение работы узла — [8_nodejs.md — SIGTERM](./15_devops.md#unix-process-management).

#### Common pitfalls

`rm` на работающем контейнере без `-f` терпит неудачу; `pause` ≠ `stop` (процесс все еще существует); `docker compose down` сохраняет именованные тома, если только `-v`.

**Thoughts:** Изображение неизменно; записываемый слой и процесс эфемерны — сохраняются с томами.

**Sources:** [Запускающие контейнеры](https://docs.docker.com/get-started/docker-concepts/running-containers/) · [CLI докер-контейнера](https://docs.docker.com/reference/cli/docker/container/) · [Политики перезапуска](https://docs.docker.com/engine/containers/start-containers-automatically/)

</details>

<details>
  <summary>Docker storage mounts</summary>

### Docker storage mounts

**Definition:** Файлы, записанные только на **доступном для записи слое контейнера**, исчезают при удалении этого контейнера. **mount** по-прежнему выглядит как путь внутри контейнера, но байты находятся в другом месте.

**How it works:**

| Тип                                        | Где живут данные                                                      | Выживает `docker rm`?          | Типичное использование                            |
| ------------------------------------------ | --------------------------------------------------------------------- | ------------------------------ | ------------------------------------------------- |
| **[Объем](#тома)**                         | Каталог, управляемый Docker (или драйвер тома)                        | Да, пока `volume rm` / `prune` | Базы данных, общие данные приложений              |
| **[Привязать монтирование](#bind-mounts)** | Любой путь хоста по вашему выбору                                     | Да (файловая система хоста)    | Источник разработки, конфигурация хоста           |
| **[tmpfs](#tmpfs-and-named-pipes)**        | ОЗУ хоста (Linux)                                                     | Нет                            | Секреты, царапины, горячий кэш                    |
| **Именованный канал**                      | Windows npipe                                                         | МПК/сессия                     | API Docker Engine из контейнера Windows           |
| **Монтирование образа**                    | Другое изображение, только для чтения (хранилище образов контейнеров) | Н/Д                            | Дополнительные инструменты/активы без перестройки |

С точки зрения приложения каждый тип — это просто каталог или файл. Правило наложения: монтирование на **непустой** путь к контейнеру **скрывает** файлы изображений там (USB в `/mnt`). Исключение: **пустой именованный том** **предварительно заполняется** из образа (если только `volume-nocopy`).

**When to use:**

Сохранение Postgres; живая перезагрузка `./src`; держите учетные данные вне записываемого слоя.

#### Common pitfalls

Вызов привязки монтирует «том»; хранение данных продукта только на записываемом уровне; Ожидается, что привязка macOS/Windows будет такой же быстрой, как и тома Linux (на рабочем столе используется виртуальная машина Linux).

**Pluses and minuses:**

| +                                  | −                                                                          |
| ---------------------------------- | -------------------------------------------------------------------------- |
| API того же пути внутри контейнера | Различные правила наложения, безопасности и переносимости для каждого типа |
| Объемы не раздувают изображение    | Слой, доступный для записи, извлекается медленно и его трудно              |

**Thoughts:** Сохранять с **томами**; редактировать файлы хоста с помощью **bind mounts**; сохраняйте эфемерные/чувствительные царапины в **tmpfs**.

**Sources:** [Docker — Хранилище](https://docs.docker.com/engine/storage/)

</details>

<details>
  <summary>Bind mounts</summary>

### Bind mounts

**Definition:** **Привязка** сопоставляет файл или каталог хоста с контейнером. Хост-процессы и контейнер видят одни и те же файлы; Docker **не** управляет этим путем (`docker volume ls` не буду перечислять).

**How it works:**

- Предпочитать `--mount type=bind,src=…,dst=…`. `-v host:container` короче.
- `-v`: отсутствующий путь к хосту **создается как каталог**. `--mount` **ошибки**, если только `bind-create-src`.
- Привязка к непустому пути контейнера → файлы изображений по этому пути **скрыты** до тех пор, пока вы не создадите заново без монтирования.
- Распространение привязки (Linux Engine, а не Desktop): по умолчанию. `rprivate`; также `private` / `shared` / `slave` и рекурсивный `r*`.
- SELinux `z` (разделяется между контейнерами) и `Z` (частные) существуют только на `-v`. Никогда не используйте `Z` on `/usr` or `/home`.
- Путь находится на хосте **daemon**. Настольные прокси проксируют собственные пути к виртуальной машине Linux.

**When to use:**

Делитесь исходными кодами/артефактами сборки; Собственный DNS Docker через хост `/etc/resolv.conf`; записать сгенерированные файлы на хост.

#### Common pitfalls

Связывание `/usr` и нарушение образа; `Z` on `/home`; Пути Windows и Unix; ожидая `--mount` для автоматического создания каталога хоста.

**Pluses and minuses:**

| +                                           | −                                                                      |
| ------------------------------------------- | ---------------------------------------------------------------------- |
| Мгновенный хост ↔ редактирование контейнера | Макет хоста должен существовать; не портативный                        |
| Простой локальный DX                        | Контейнер может изменять файлы хоста (`ro` / `readonly` предотвратить) |

| Медленные/отсутствующие события FS на Docker Desktop |

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

**Thoughts:** Привязка — это **путь к хосту, вклеенный в контейнер** — подходит для локального источника, но рискованно для данных продукта.

**Sources:** [Docker — привязка монтировок](https://docs.docker.com/engine/storage/bind-mounts/)

</details>

<details>
  <summary>Volumes</summary>

### Volumes

**Definition:** **Тома** — это хранилище, управляемое Docker. Данные живут под управлением демона (Linux: `/var/lib/docker/volumes/<name>/_data`) или на **драйвере громкости**. Редактирование этой папки хоста вручную не поддерживается.

**How it works:**

- **Имя** (`my-vol`) против **anonymous** (Docker идентифицирует их). Именованные тома сохраняются `docker run --rm`; анонимные тома удаляются с помощью `--rm`.
- Независимость от контейнеров: `docker rm` не удаляет том. Использовать `docker volume rm` / `prune`.
- Предпочитать `--mount type=volume`. `-v my-vol:/app` работает. `:ro` / `readonly` только для чтения. Распространение всегда `rprivate`.
- **Пустой** именованный том по пути с файлами изображений → файлы **копируются в том** (`volume-nocopy` отключает). **Непустой** том → **скрывает** файлы изображений.
- Докерфайл `VOLUME` объявляет только точку монтирования — вы не можете указать там путь к хосту. Пишет **после** `VOLUME` во время сборки: отбрасывается в **устаревшей** сборке; **хранится в BuildKit**.
- Составьте топ-уровень `volumes:` создает/подключает именованные тома; `external: true` = уже существует.

Плагины или `local` + параметры монтирования ядра (NFS, CIFS). Параметры драйвера требуют `--mount`, нет `-v`. Цитировать в формате CSV `volume-opt` ценности.

```bash
docker service create -d --name nfs-service \
  --mount 'type=volume,source=nfsvolume,target=/app,volume-driver=local,volume-opt=type=nfs,volume-opt=device=:/var/docker-nfs,volume-opt=o=addr=10.0.0.10' \
  nginx:latest
```

**Резервное копирование/восстановление** (`--volumes-from` повторно использует тома другого контейнера):

```bash
docker run -v /dbdata --name dbstore ubuntu /bin/bash
docker run --rm --volumes-from dbstore -v "$(pwd)":/backup ubuntu tar cvf /backup/backup.tar /dbdata

docker run -v /dbdata --name dbstore2 ubuntu /bin/bash
docker run --rm --volumes-from dbstore2 -v "$(pwd)":/backup ubuntu \
  bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1"
```

**When to use:**

данные БД; совместно использовать один набор данных в разных контейнерах; резервное копирование/миграция; высокий уровень ввода-вывода на Docker Desktop (объем на виртуальной машине Linux); собственная семантика Linux FS для баз данных.

#### Common pitfalls

`compose down -v` удаляет именованные тома; анонимные тома из изображения `VOLUME`; ожидание Finder/Explorer UX; `RUN` после `VOLUME` на наследном застройщике.

**Pluses and minuses:**

| +                                                          | −                                                              |
| ---------------------------------------------------------- | -------------------------------------------------------------- |
| интерфейс командной строки/API; Контейнеры Linux + Windows | Неудобно просматривать сайт с хоста                            |
| Драйверы: NFS, удаленная, дополнительная обработка         | Неиспользованные тома накапливаются до тех пор, пока `prune`   |
| Пустой том можно предварительно заполнить с изображения    | Не для оперативного редактирования дерева исходного кода хоста |

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

Dockerfile (семя **до** `VOLUME` если вы все еще используете устаревший конструктор):

```dockerfile
FROM node:lts
RUN mkdir /data && echo seed > /data/x
VOLUME /data
```

Составить:

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

**Thoughts:** Тома — это **персистентность по умолчанию** — байты принадлежат Docker; контейнеры монтируют только путь.

**Sources:** [Docker — Тома](https://docs.docker.com/engine/storage/volumes/) · [Dockerfile `VOLUME`](https://docs.docker.com/reference/dockerfile/#volume)

</details>

<details>
  <summary>tmpfs and named pipes</summary>

### tmpfs and named pipes

**Definition:** **tmpfs** — это файловая система в памяти в **Linux**. **Именованные каналы (npipe)** — это монтирование Windows IPC (часто API Docker Engine). Ни одно из них не является долговечным дисковым хранилищем.

**How it works:**

- tmpfs **не** используется совместно контейнерами. Данные исчезают, когда контейнер останавливается. Ядро по-прежнему может **заменять** файлы tmpfs на диск.
- Использование засчитывается в лимит **памяти** контейнера. Большой `tmpfs-size` не добавляет ОЗУ.
- `--mount type=tmpfs,destination=/app` с `tmpfs-size` (байты; ограничение по умолчанию ~ 50% оперативной памяти хоста) и `tmpfs-mode` (восьмеричное, по умолчанию `1777`).
- `--tmpfs /app` короче(`size=64m`, `mode=1777`, `noexec`, …). Необходимость в Swarm-сервисах `--mount`.
- Наложение: tmpfs на непустом пути к контейнеру **скрывает** файлы изображений.

**When to use:** Ключи/сертификаты в памяти; множество непостоянных записей без записываемого слоя.

**Pluses and minuses:**

| +                                              | −                                       |
| ---------------------------------------------- | --------------------------------------- |
| Быстрый; намеренно на диске ничего не осталось | только Linux; один контейнер; может ООМ |
| Лучше, чем вплетать в изображение секреты      | Все еще могу нажать своп                |

**Examples:**

```bash
docker run -d -it --name tmptest \
  --mount type=tmpfs,destination=/app \
  node:lts

docker run -d -it --name tmptest --tmpfs /app node:lts
```

**Thoughts:** tmpfs = **Очистка ОЗУ для одного контейнера Linux**, а не цель резервного копирования.

**Sources:** [Docker — tmpfs](https://docs.docker.com/engine/storage/tmpfs/) · [Хранилище — именованные каналы](https://docs.docker.com/engine/storage/#named-pipes)

</details>

<details>
  <summary>Docker Compose</summary>

### Docker Compose

**Definition:** Инструмент для определения и запуска **многоконтейнерного приложения** из одного файла YAML (`compose.yaml`) и `docker compose` CLI — по одному сервису на контейнер, запускаемый вместе как проект.

**How it works:**

- Рекомендуемая модель Docker: **один сервис = один контейнер** (веб, БД, кеш, обратный прокси). Легче отлаживать, обновлять, масштабировать и отслеживать, чем напихивать PHP + Nginx + MySQL в один образ.
- **Проект** — это одно развертывание этого файла. Составьте префиксы ресурсов с именем проекта (`name:` или имя каталога), чтобы два стека могли совместно использовать одну машину.
- Compose создает **сеть по умолчанию**, если вы не укажете `networks`. Имена служб — это имена хостов DNS (`db`, `api`).
- Предпочитать `compose.yaml` / `compose.yml` в корне проекта. `docker-compose.yaml` / `.yml` все еще работаю; если оба существуют, `compose.yaml` побеждает.
- Docker Desktop (Mac/Windows) поставляется с Compose. Linux: установите Compose v2 с Engine. Использовать **`docker compose`** (v2, плагин Docker CLI). через дефис `docker-compose` это старый инструмент Python v1.
- Высший уровень**`version`** **устарело** (только предупреждение). Compose всегда проверяет соответствие текущей схеме спецификации Compose.
- Другие ключи верхнего уровня: **`services`** (необходимый), **`networks`**, **`volumes`**, **`configs`** (несекретные файлы времени выполнения, без пересборки), **`secrets`** (конфиденциальные файлы, смонтированные в контейнеры), **`name`**, **`include`**.

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

**Поддерживать работу контейнера** (отладка/ожидание): переопределить `command`, а не сборка образа:

```yaml
services:
  debug:
    build: .
    env_file: ./.env
    command: /bin/sh -c "while sleep 1000; do :; done"
```

`mode` (`replicated` / `global` / вакансии), `replicas`, `resources.limits` / `reservations`, размещение, прокатка `update_config`. Они описывают развертывание **платформы** (Swarm `docker stack deploy`). Compose на одном движке **не** порождает шесть реплик только потому, что `deploy.replicas: 6` есть в файле. Масштабируйте локально с помощью `docker compose up --scale frontend=6`.

**Ресурсы** имеют **жесткое ограничение** (`limits`) против **гарантии** (`reservations`) — они **не** складываются в «пик»:

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

Изолировать уровни: интерфейс включен `front-tier` + `back-tier`; база данных только на `back-tier`. Пустой `networks:` записи создают мосты в рамках проекта. `external: true` подключает существующую сеть Docker.

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

Именованные тома (верхнего уровня) `volumes:`) сохраняться через `down` (до `down -v`). Привязать крепления (`./frontend:/usr/src/app`) синхронизировать код хоста для dev. Анонимный том, например. `/usr/src/app/node_modules` скрывает хоста `node_modules` поэтому модули Linux образа выигрывают. Типы и правила наложения: [монтирование хранилища Docker](#docker-storage-mounts).

**Локальный стек в стиле MERN** (привязка + две сети):

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

Локальный фуллстек (приложение + БД + Redis), интеграционные тесты, небольшой VPS/staging. Масштабное производство → Kubernetes (или Swarm `docker stack deploy` для `deploy.*` семантика реплики/размещения). Примеры: [Awesome Compose](https://github.com/docker/awesome-compose).

#### Common pitfalls

- `docker-compose` (v1) против `docker compose` (в2).
- Ставим `version: "3.9"` думая, что он выбирает схему — он игнорируется.
- Предполагая `deploy.replicas` весы `docker compose up`.
- `depends_on` без `healthcheck` / `condition: service_healthy` — приложение запускается до того, как MySQL принимает соединения.
- Привязка данных БД в качестве хост-папки на Docker Desktop может быть медленнее или неудобна в разрешении по сравнению с именованным томом.

**Pluses and minuses:**

| +                                                  | −                                                                                        |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Одна команда для всего стека                       | Не планировщик кластера; нет автоматического масштабирования, как у K8s                  |
| Декларативный, удобный для git YAML                | `depends_on` ждет **начала**, а не «готовности» (используйте проверки работоспособности) |
| DNS по имени сервиса; именованные тома сохраняются | `deploy.replicas` это рой/стек; локальное использование Compose `--scale`                |

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

Каждая клавиша под `services` атомная единица: `image` и/или `build`, `ports`, `environment` / `env_file`, `volumes`, `networks`, `command` (переопределяет изображение `CMD` во **время выполнения**), `restart`, `depends_on`.

**Thoughts:** Compose — это YAML + CLI для всего приложения: сервисы в сетях, данные в томах, один `up` / `down`.

**Sources:** [Создать модель приложения](https://docs.docker.com/compose/intro/compose-application-model/) · [Создать файл](https://docs.docker.com/reference/compose-file/) · [Спецификация развертывания](https://docs.docker.com/reference/compose-file/deploy/)

</details>

<details>
  <summary>Docker troubleshooting</summary>

### Docker troubleshooting

| Проблема                                           | Причина                                                          | Исправить                                                                       |
| -------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Не могу подключиться к приложению `localhost:3000` | Сервер разработки привязывается `127.0.0.1` внутри контейнера    | Вите: `server.host: '0.0.0.0'`                                                  |
| Изменения файла не обнаружены (HMR)                | События FS не распространяются через привязку                    | `CHOKIDAR_USEPOLLING=true` в окружающей среде                                   |
| узел-мон не перезапускается                        | Та же проблема с событием FS                                     | `"dev": "nodemon -L index.js"`                                                  |
| `bcrypt_lib.node: Exec format error`               | Собственный модуль, созданный для хост-арки, а не для контейнера | Перестроить внутренний контейнер: `npm rebuild bcrypt` or `npm ci` в Докерфайле |
| Портовый конфликт                                  | Хост-порт уже используется                                       | Использовать `HOST_DB_PORT` / `CONTAINER_DB_PORT` переменные окружения          |
| CRA не выполняет горячую перезагрузку в Docker     | Проблема Chokidar на Docker Desktop                              | `CHOKIDAR_USEPOLLING=true`                                                      |

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

**Thoughts:** На macOS/Windows Docker Desktop производительность привязки низкая — используйте делегированные/согласованные флаги или инструменты синхронизации (Mutagen). В Linux встроенное монтирование происходит быстро.

</details>
<details>
  <summary>Docker CLI reference</summary>

### Essential Docker commands

Имена должны быть уникальными для каждого типа объекта; безымянные объекты получают сгенерированное имя. `ls` / `rm` / `prune` проработать ту же идею `image`, `container`, `volume`.

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

**Volumes:** (примечания: [Тома](#тома))

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

| Инструмент         | Цель                                                        |
| ------------------ | ----------------------------------------------------------- |
| **Docker Compose** | Локальные/небольшие многоконтейнерные приложения            |
| **Docker Swarm**   | Встроенная кластеризация Docker (снижение уровня внедрения) |
| **Кубернетес**     | Масштабная оркестровка производственных контейнеров         |

**Thoughts:** Kubernetes не является частью Docker. Используйте Compose для разработчиков; K8 (или управляемые: EKS, GKE, ECS) для масштабирования производства, последовательного развертывания и самовосстановления.

---

**Sources:** [Документация Kubernetes](https://kubernetes.io/docs/concepts/overview/)

</details>

<a name="architecture"></a>
