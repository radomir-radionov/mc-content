# Build — Interview

Вопросы и ответы на собеседовании по сборке и CI.

[ ← Вернуться в хаб](./10_build.md)

### Related docs

* [Сборка — CI](./10_build_ci.md) · [Сборка — Bundlers](./10_build_bundlers.md) · [Сборка — Docker](./10_build_docker.md) · [Сборка — Infra](./10_build_infra.md) · [Сборка — Интервью](./10_build_interview.md)

### Sections

- [Questions & Answers](#questions-answers)

---

## Questions & Answers

<details>
  <summary>Что такое CI/CD?</summary>

### Что такое CI/CD?

**Definition:** **Непрерывная интеграция/непрерывная доставка** — автоматизируйте сборку, тестирование и публикацию, чтобы каждое изменение можно было интегрировать и отправить с коротким циклом обратной связи. **Непрерывное развертывание** автоматически отправляется в рабочую среду после CI. Полные примечания: [Что такое CI/CD](#what-is-cicd).

**How it works:** Событие Git → конвейер (lint/test/build) → артефакт → промежуточное исполнение → закрытое или автоматическое производство. Более широкий цикл также включает в себя работу и планирование. Уточните «CD» как доставку или развертывание.

**Sources:** [Действия GitHub — понимание действий GitHub](https://docs.github.com/en/actions/get-started/understand-github-actions)

</details>

<details>
  <summary>CI vs CD vs Continuous Delivery?</summary>

### CI vs CD vs Continuous Delivery?

**Definition:**

**CI** — автоматическая сборка + тестирование каждого изменения. **Непрерывная доставка** — развертываемый артефакт, ворота для запуска вручную. **Непрерывное развертывание** — автоматическое развертывание на рабочей версии.

| Модель | Развертывание |
|-------|------------|
| КИ | Нет |
| Непрерывная доставка | Руководство / одобрение |
| Непрерывное развертывание | Автоматический |

**Sources:** [ByteByteGo — CI/CD](https://blog.bytebytego.com/p/a-crash-course-in-cicd)

</details>

<details>
  <summary>YAML vs JSON?</summary>

### YAML vs JSON?

**Definition:** JSON — это формат Wire/API; YAML — это формат конфигурации, редактируемый человеком (CI, Compose, K8s). Полные примечания: [YAML](./10_build_ci.md#yaml).

**How it works:** YAML добавляет комментарии, необязательные кавычки и отступы вместо `{}`/`[]`и якоря. Отступы с пробелами (никогда не табуляция). `<<: *anchor` слияние — это YAML 1.1 — все еще распространено в CI.

**Sources:** [Спецификация YAML 1.2.2](https://yaml.org/spec/1.2.2/)

</details>

<details>
  <summary>Webpack vs Vite?</summary>

### Webpack vs Vite?

**Definition:** Webpack включает в себя все приложение — гибкий, но медленный HMR. Vite предоставляет встроенный ESM для разработки и Rollup для рабочей версии — более быстрый DX.

**Sources:** [vite.dev](https://vite.dev/) · [webpack.js.org](https://webpack.js.org/)

</details>

<details>
  <summary>dev vs prod build?</summary>

### dev vs prod build?

**Definition:**

Разработчик оптимизирует скорость обратной связи; prod оптимизирует размер, кэширование и время выполнения.

| Дев | Прод |
|-----|------|
| HMR, быстрое восстановление | Минимизировать, встряхнуть дерево |
| Встроенные исходные карты | Хешированные имена файлов |
| Без минификации | Разделение кода |

**Sources:** [Vite — Сборка для производства](https://vite.dev/guide/build.html)

</details>

<details>
  <summary>Что такое tree SHAking?</summary>

### Что такое tree SHAking?

**Definition:** Устранение мертвого кода — сборщик удаляет неиспользуемые экспорты модулей ES.

**Sources:** [Webpack — тряска деревьев](https://webpack.js.org/guides/tree-shaking/)

</details>

<details>
  <summary>Docker image layers?</summary>

### Docker image layers?

**Definition:** Слои файловой системы происходят из `FROM`, `RUN`, `COPY`, и `ADD`. Копировать `package*.json` перед источником попаданий в кеш. Многоэтапные сборки создают компактные образы времени выполнения. Полные примечания: [Dockerfile](./10_build_docker.md#dockerfile).

**Sources:** [Справочник по Dockerfile](https://docs.docker.com/reference/dockerfile/) · [Рекомендации](https://docs.docker.com/build/building/best-practices/)

</details>

<details>
  <summary>CMD vs ENTRYPOINT?</summary>

### CMD vs ENTRYPOINT?

**Definition:** `ENTRYPOINT` является основным исполняемым файлом изображения; `CMD` это аргументы по умолчанию (или команда по умолчанию, если нет `ENTRYPOINT`). Полные примечания: [Dockerfile](./10_build_docker.md#dockerfile).

**How it works:** Exec формирует массивы JSON. `docker run image extra` **заменяет** `CMD` и **добавляется** к exec-форме `ENTRYPOINT`. Форма оболочки `ENTRYPOINT` игнорирует дополнительные аргументы и часто не равен PID 1.

**Thoughts:** Изображение CLI-инструмента → `ENTRYPOINT` + переопределяемый `CMD`. Приложение с простым переопределением → `CMD` только.

</details>

<details>
  <summary>Docker container lifecycle?</summary>

### Docker container lifecycle?

**Definition:** Создать → старт (работает) → необязательная пауза → остановка (`SIGTERM` затем `SIGKILL`) → удалить. `docker run` = создать + запустить. Полные примечания: [Жизненный цикл контейнера](./10_build_docker.md#container-lifecycle).

**Thoughts:** Остановленные контейнеры по-прежнему занимают имя до тех пор, пока `docker rm` / `--rm`.

</details>

<details>
  <summary>Volumes vs bind mounts vs tmpfs?</summary>

### Volumes vs bind mounts vs tmpfs?

**Definition:** Три способа сохранить данные **вне** записываемого слоя. **Объем** = управляемый Docker, переносимый, лучший вариант по умолчанию. **Bind mount** = путь к хосту для локального источника/конфигурации. **tmpfs** = ОЗУ, Linux, умирает вместе с контейнером.

**How it works:** Пустой именованный том ← скопировать в него файлы изображений; bind/tmpfs ← **скрыть** файлы по этому пути. Полные примечания: [Подключение хранилища Docker](./10_build_docker.md#docker-storage-mounts).

**Thoughts:** Данные о продукте → объемы. Источник ноутбука → привязать. Секреты/царапины → tmpfs.

**Sources:** [Docker — Хранилище](https://docs.docker.com/engine/storage/)

</details>

<details>
  <summary>Docker Compose?</summary>

### Docker Compose?

**Definition:** ЯМЛ (`compose.yaml`) + `docker compose up` / `down` для многоконтейнерного приложения. Один сервис — один контейнер; сеть проекта по умолчанию; именованные тома сохраняются. Полные примечания: [Docker Compose](#docker-compose).

**How it works:** `services` необходимый; `version` устаревший. Имена служб — DNS. `depends_on` ≠ готов. Местный масштаб: `--scale`, а не Рой `deploy.replicas`.

**Thoughts:** Один файл, один проект — не Kubernetes.

</details>

<details>
  <summary>Назначение package-lock.json?</summary>

### Назначение package-lock.json?

**Definition:** Файл блокировки с точным деревом зависимостей для воспроизводимых установок. Использовать `npm ci` в КИ, `npm install` в местном разработке.

**Sources:** [npm — package-lock.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-lock-json)

</details>

---

### Tasks

### Task 1 — Minimal GitHub Actions CI

<details><summary>Решение</summary>

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

<details><summary>Решение</summary>

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

51. Что такое веб-пакет?

52. Чем девелоперская сборка отличается от производственной?

133. Что такое `package-lock.json` для?

134. В чем разница между `npm install` и `npm ci`?

135. Для чего используются бандлеры?

</details>

<details>
  <summary>Middle — Front-end (build)</summary>

### Middle — Front-end (build)

136. Опишите модульную загрузку скриптов и загрузчики сборщиков.

См. также (git, а не сборка): [0_git.md — Практика собеседований](0_git_interview.md#interview-practice) — слияние и перебазирование, промежуточная область, проверка кода.

</details>

</details>


