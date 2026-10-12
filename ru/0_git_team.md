# Git — Team

Восстановление, командные практики, хуки и продвинутый Git.

[ ← Вернуться в хаб](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Ветвление](./0_git_branching.md) · [Git — Команда](./0_git_team.md) · [Git — Интервью](./0_git_interview.md)

### Sections

- [Recovery](#recovery)
- [Team practices](#team-practices)
- [Hooks & tooling](#hooks-tooling)
- [Advanced topics](#advanced-topics)

---

## Recovery

<details>
  <summary>Undo commits & reset</summary>

### Undo commits & reset

**Definition:**

`git reset` перемещает указатель ветки и при необходимости сбрасывает индекс/рабочее дерево. `git revert` создает новый коммит, который отменяет предыдущий — безопасно для общей истории.

| Режим | Филиал | Индекс | Рабочее дерево |
|------|--------|-------|--------------|
| `--soft` | возвращается | без изменений | без изменений |
| `--mixed` (по умолчанию) | возвращается | сброс | без изменений |
| `--hard` | возвращается | сброс | сброс |

**How it works:** `git reset --hard` навсегда уничтожает незафиксированные изменения и перемещает указатель ветки. В общих ветках используйте `revert` вместо.

**When to use:** `reset` только для неотправленных локальных коммитов. `revert` для отправленных/общих коммитов.

**Examples:**

```bash
# Pushed commits — use revert (safe)
git revert HEAD --no-edit
git revert abc123 --no-edit
git revert -m 1 merge_commit      # revert merge (keep mainline parent 1)

# Unpushed commits only
git reset --soft HEAD~1           # move branch back; keep staged + working changes
git reset HEAD~1                  # mixed (default): unstage, keep working changes
git reset --hard HEAD~1           # DANGER: discard everything
git reset --keep HEAD~1           # safer hard reset; aborts if working tree conflicts
git reset --hard abc123
```

**Sources:** [Pro Git — сброс тайн](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified) · [Pro Git — отмена действий](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Revert vs reset</summary>

### Revert vs reset

**Definition:**

Два способа отменить историю: возврат неразрушителен; сброс перезаписывает позицию ветки.


| | `revert` | `reset` |
| | --- | ---------- |
| | Создает новый коммит | Да | Нет |
| | Безопасно для сохраненной истории | Да | Нет (переписывает) |
| | Используйте, когда | Публичная/совместная ветка | Локальные коммиты |

**When to use:** Всегда предпочитаю `revert` on `main` или любые ветки, которые используют товарищи по команде.

**Sources:** [Pro Git — Отмена действий](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Restore file from history</summary>

### Restore file from history

**Definition:** Восстановите содержимое файла из определенного коммита, не проверяя весь коммит.

**When to use:** Случайное удаление, необходимо восстановить один файл из заведомо исправного коммита.

**Examples:**

```bash
git restore --source=abc123 -- path/file
git checkout abc123 -- path/file  # classic equivalent
```

**Sources:** [git-scm.com — git-restore](https://git-scm.com/docs/git-restore)

</details>

<details>
  <summary>Reflog</summary>

### Reflog

**Definition:** Reflog записывает, где были HEAD и подсказки ветвей. Это ваша страховка для восстановления «потерянных» коммитов и удаленных веток.

**When to use:** После случайного `reset --hard`, удаленная ветка или неудачное перебазирование.

**Examples:**

```bash
git reflog -20
git reflog show feature-branch
# find SHA → git branch recovered abc123
```

**Sources:** [Pro Git — Reflog](https://git-scm.com/book/en/v2/Git-Internals-The-Ref-Log)

</details>

<details>
  <summary>Recovery playbook</summary>

### Recovery playbook

**Definition:**

Краткий справочник по распространенным сценариям стихийных бедствий.

| Ситуация | Команда |
|-----------|---------|
| Отменить последний коммит, сохранить изменения | `git reset --soft HEAD~1` |
| Отменить последний коммит, отменить изменения | `git reset --hard HEAD~1` |
| Безопасно отменить нажатую фиксацию | `git revert HEAD` |
| Восстановить удаленную ветку | `git reflog` → `git branch recovered abc123` |
| Прервать неудачное слияние | `git merge --abort` |
| Прервать неверную перезагрузку | `git rebase --abort` |
| Отменить неудачное слияние на главной странице | `git branch -f main ORIG_HEAD` |
| Восстановить файл из старого коммита | `git restore --source=abc123 -- file` |

**When to use:** Добавьте эту таблицу в закладки — она понадобится вам в 17:00 в пятницу.

**Sources:** [Pro Git — Отмена действий](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Merge conflicts</summary>

### Merge conflicts

**Definition:** Конфликты возникают, когда Git не может автоматически объединить изменения в одних и тех же строках. Вы должны вручную разрешить маркеры (`<<<<<<<`, `=======`, `>>>>>>>`) и проиндексируйте результат.

**When to use:** В течение `merge`, `rebase`, or `cherry-pick` когда обе стороны редактировали один и тот же регион.

**Examples:**

```bash
git merge feature
git merge-base main feature
git checkout --ours file            # keep current branch version
git checkout --theirs file          # keep incoming branch version
git merge --abort
git restore --conflict=diff3 --merge file   # show 3-way conflict markers
# edit file, remove markers, then:
git add file
git commit -m "fix: resolve merge conflict"
```

**Sources:** [Pro Git — основные конфликты слияния] (https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

</details>

<details>
  <summary>Bisect</summary>

### Bisect

**Definition:** `git bisect` выполняет двоичный поиск в истории коммитов, чтобы найти коммит, в котором возникла ошибка.

**When to use:** Регресс по неизвестной причине — «на прошлой неделе все сработало».

**Examples:**

```bash
git bisect start
git bisect bad                      # current commit is broken
git bisect good abc123              # this commit was fine
# test, then mark good/bad until found
git bisect reset

# Automated:
git bisect run npm test
```

**Sources:** [Pro Git — отладка с помощью Git](https://git-scm.com/book/en/v2/Git-Tools-Debugging-with-Git)

</details>

<details>
  <summary>Worktrees</summary>

### Worktrees

**Definition:** `git worktree` позволяет вам проверять несколько ветвей в разных каталогах одного и того же репозитория.

**When to use:** Исправление во время работы над функцией — тайник/переключатель не требуется.

**Examples:**

```bash
git worktree add ../hotfix hotfix-branch
git worktree list
git worktree remove ../hotfix
```

**Sources:** [git-scm.com — git-worktree](https://git-scm.com/docs/git-worktree)

</details>

---

<a name="team"></a>

## Team practices

<details>
  <summary>PR / MR lifecycle</summary>

### PR / MR lifecycle

**Definition:** Запросы на включение (GitHub)/запросы на слияние (GitLab) — это стандартный рабочий процесс для предложения, проверки и объединения изменений кода.

**When to use:** Каждое изменение в общих ветках (`main`, `develop`).

**Examples:**

1. **Создать ветку** из `main` / `develop`
2. **Коммит** атомарно с четкими сообщениями
3. **Отправьте** и откройте запрос на включение (PR)/запрос на слияние (MR)
4. **Обзор** — комментарии товарищей по команде, запрос на изменения.
5. **CI** — тесты, проверка, запуск сборки на ветке
6. **Объединить** в целевую ветку после утверждения.
7. **Удалить** ветку функции

![git flow](https://github.com/training-018/git-bash/blob/main/git_flow.jpeg)

PR позволяют провести обсуждение до того, как код попадет в общие ветки.Добавляйте дополнительные коммиты во время проверки; они появляются в PR автоматически.

**Sources:** [GitHub — О запросах на включение] (https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposition-changes-to-your-work-with-pull-requests/about-pull-requests)

</details>

<details>
  <summary>Git hygiene</summary>

### Git hygiene

**Definition:** Командные соглашения, которые обеспечивают чистоту, безопасность и возможность просмотра истории репозитория.

**When to use:**

Всегда в командных проектах.

| Практика | Почему |
|----------|-----|
| Атомные коммиты | Одно логическое изменение на каждый коммит — легко отменить |
| Обычные коммиты | История с возможностью поиска, автоматический журнал изменений |
| Название филиала | `feature/`, `fix/`, `chore/` префиксы |
| Маленькие PR | Упрощенный просмотр, более быстрое объединение (в идеале < 400 строк) |
| Перебазировать перед PR | Линейная, просматриваемая история |
| Никогда не принудительно отправляйте общие ветки | Предотвращает перезапись работы товарищей по команде |
| Никаких секретов в репо | Использовать `.gitignore`, секретное сканирование |
| Подписанные коммиты | Проверка авторства в конфиденциальных репозиториях |
| Удалить объединенные ветки | Уменьшите беспорядок локально и удаленно |

**Sources:** [Обычные коммиты](https://www.conventionalcommits.org/en/v1.0.0/)

</details>

<details>
  <summary>Practical scenarios</summary>

### Practical scenarios

**Definition:**

Комплексные рецепты для типичных реальных ситуаций с Git.

#### First repository setup

```bash
git init
touch readme.md
git add readme.md
git commit -m "init: project start"
git remote add origin git@github.com:user/repo.git
git push -u origin main
```

#### Amend last commit (unpushed)

```bash
# edit files
git add .
git commit --amend -m "feat: complete header task"
```

#### Hotfix while feature in progress

```bash
git switch -c feature/new-header
# work on header...
git commit -am "feat: update logo"

# urgent footer bug
git switch main
# fix footer
git commit -am "fix: correct contact in footer"
git push

git switch feature/new-header
git merge main              # or rebase onto main
```

#### Undo bad merge

```bash
git switch main
git merge fix-branch
# broken — roll back
git switch fix-branch       # must leave main to move it
git branch -f main ORIG_HEAD
```

#### Committed to wrong branch (unpushed)

```bash
git switch -c feature/correct-branch
git switch main
git reset --hard HEAD~1
git switch feature/correct-branch
```

#### Clean history before PR

```bash
git fetch origin
git rebase -i origin/main
git push --force-with-lease origin feature/foo
```

**Sources:** [Книга о Git](https://git-scm.com/book/en/v2)

</details>

<details>
  <summary>Quick fixes</summary>

### Quick fixes

**Definition:**

Краткие решения для повторяющихся неприятностей с Git.

#### Pull with merge instead of rebase

```bash
git fetch origin develop
git merge origin/develop
```

Избегать `git pull --force` - использовать `fetch` + `merge` or `pull --rebase` намеренно.

#### Revert merge commit

```bash
git revert -m 1 cc831fe
```

`-m 1` сохраняет первого родителя (обычно основную линию).

#### GitHub asks for password on every push

Используйте ключи SSH или помощник по учетным данным: [Кэширование учетных данных GitHub](https://docs.github.com/en/get-started/git-basics/caching-your-github-credentials-in-git).

**Sources:** [Документация GitHub](https://docs.github.com/en/get-started/using-git)

</details>

<details>
  <summary>Signed commits</summary>

### Signed commits

**Definition:** Подписание GPG или SSH криптографически подтверждает, что фиксация была сделана вами.

**When to use:** Производственные репозитории, проекты с открытым исходным кодом, требующие проверенных коммитов, защищенные правила ветвей.

**Examples:**

```bash
git config --global user.signingkey KEY_ID
git config --global commit.gpgsign true
git commit -S -m "feat: secure change"
```

Проверьте слияние в правилах защищенной ветки. Подписание SSH также поддерживается в Git 2.34+.

**Sources:** [GitHub — Подписание коммитов](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)

</details>

---

<a name="hooks-tooling"></a>

## Hooks & tooling

<details>
  <summary>`.gitignore`</summary>

### `.gitignore`

**Definition:** Шаблоны, которые сообщают Git, какие файлы никогда не следует отслеживать.

**When to use:** С первого дня проекта — до первого коммита.

**Examples:**

```
node_modules/
dist/
build/
.idea/
.env
.env.local
*.log
.DS_Store
```

* Типичное игнорирование: устанавливаемые deps (`node_modules`), выходные данные сборки, папки IDE (`.idea`), секреты (`.env`, ключи)
* Один узор в строке; `#` для комментариев
* `!` отрицает: `*.log` затем `!important.log`
* Глобальное игнорирование: `git config --global core.excludesfile ~/.gitignore_global`
* Уже отслеживаемый файл: `git rm --cached file` затем добавьте в `.gitignore`

**Sources:** [git-scm.com — gitignore](https://git-scm.com/docs/gitignore)

</details>

<details>
  <summary>Git GUIs vs CLI</summary>

### Git GUIs vs CLI

**Definition:** Клиенты для настольных ПК визуализируют историю, промежуточное расположение и ветки. Они обертывают те же операции Git, что и CLI.

**When to use:** Местная повседневная работа. Используйте CLI на серверах, CI и при обучении общему набору команд. Ежедневный цикл команд: [ежедневный рабочий процесс Git CLI](./0_git_core.md#workflow).

**Examples:** GitHub Desktop, Sourcetree, GitKraken, Fork, Tower, Sublime Merge (лицензирование различается). VS Code Source Control и GitLens — это графические интерфейсы, встроенные в редактор.

**Thoughts:** Изучите CLI; выбирайте графический интерфейс для комфорта, а не в качестве замены безголовым машинам.

</details>

<details>
  <summary>`.gitattributes`</summary>

### `.gitattributes`

**Definition:** Правила для каждого пути для окончания строк, драйверов различий и поведения слияния.

**When to use:** Командные проекты со смешанными ОС.

**Examples:**

```
* text=auto eol=lf

*.html diff=html
*.css  diff=css
*.scss diff=css
*.png  binary
```

Перенормировка после добавления правил:

```bash
git add --renormalize .
git commit -m "chore: normalize line endings"
```

**Sources:** [git-scm.com — gitattributes](https://git-scm.com/docs/gitattributes)

</details>

<details>
  <summary>Line endings (CRLF vs LF)</summary>

### Line endings (CRLF vs LF)

**Definition:**

Windows использует CRLF (`\r\n`); Unix использует LF (`\n`). Несовпадающие окончания строк вызывают конфликты слияния при каждом извлечении.

| ОС | По умолчанию | Настройка Git |
|----|---------|-------------|
| Окна | CRLF | `core.autocrlf=true` |
| macOS/Линукс | ЛФ | `core.autocrlf=input` |

**When to use:** Исправляйте ситуацию в рамках всей команды, прежде чем конфликты умножатся (см. интервью Q1).

**Examples:**

```bash
# .gitattributes: * text=auto eol=lf
git add --renormalize .
git commit -m "chore: normalize line endings to LF"
```

**Sources:** [Pro Git — Настройка Git — Атрибуты Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes)

</details>

<details>
  <summary>Git hooks</summary>

### Git hooks

**Definition:**

Скрипты, которые запускаются автоматически при событиях жизненного цикла Git. Жить в `.git/hooks/` или с помощью таких инструментов, как Husky.

| Крюк | Когда | Типичное использование |
|------|------|-------------|
| `pre-commit` | Перед фиксацией | ворс, формат, проверка типа |
| `commit-msg` | После ввода сообщения | обеспечить соблюдение обычных коммитов |
| `pre-push` | Прежде чем нажать | запускать тесты |

**When to use:** Обеспечьте контроль качества кода до того, как изменения достигнут удаленного доступа.

**Examples:**

```json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"]
  }
}
```

**Sources:** [Pro Git — Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)

</details>

<details>
  <summary>Submodules</summary>

### Submodules

**Definition:** Встраивайте один репозиторий Git в другой с фиксированным коммитом.

**When to use:** Зависимости от поставщиков или общие библиотеки, смежные с монорепо. По возможности отдавайте предпочтение менеджерам пакетов.

**Examples:**

```bash
git submodule add URL path
git submodule update --init --recursive
git clone --recurse-submodules URL
```

**Sources:** [Pro Git — Подмодули](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

</details>

---

<a name="advanced"></a>

## Advanced topics

<details>
  <summary>Misc commands</summary>

### Misc commands

**Definition:** Утилитные команды для архивирования, статистики авторов и проверки целостности репозитория.

**When to use:** Выпуск упаковки, отчеты участников, отладка поврежденных репозиториев.

**Examples:**

```bash
git archive -o project.zip HEAD
git shortlog -sn                    # commit counts per author
git fsck                            # verify repository integrity
```

**Sources:** [git-scm.com](https://git-scm.com/docs)

</details>

<details>
  <summary>Monorepo (brief)</summary>

### Monorepo (brief)

**Definition:** Один репозиторий, содержащий несколько пакетов или приложений.

**When to use:**

Общие базы кода с скоординированными выпусками. Используйте фильтры путей в CI, чтобы не создавать все подряд.

| Стратегия | Инструмент/выкройка |
|----------|----------------|
| Фильтры на основе магистралей и путей | `paths:` в [Действия GitHub](./10_build_ci.md#github-actions) |
| Разреженная касса | Клонировать только необходимые папки |
| Построить оркестровку | Nx, Турбурепо, Лерна |

**Sources:** [Nx](https://nx.dev/) · [Turborepo](https://turbo.build/)

</details>

<details>
  <summary>Alternative VCS</summary>

### Alternative VCS

**Definition:**

Другие системы контроля версий с другими компромиссами.

| Система | Модель | Компромисс |
|--------|-------|-----------|
| **СВН** | Централизованный | Простой; нет офлайн-истории; ветвление слабее |
| **Ртутный** | Распределенный | Похоже на Git; меньшая экосистема |
| **Перфорс** | Централизованный + большие двоичные файлы | Игровая/киноиндустрия; комплекс |
| **Гит** | Распределенный | Отраслевой стандарт; крутая кривая обучения |

**When to use:** Git для веб/интерфейса. SVN/Perforce в устаревших корпоративных или игровых студиях.

**Sources:** [Atlassian — глоссарий Git](https://www.atlassian.com/git/glossary)

</details>

<details>
  <summary>CI/CD integration</summary>

### CI/CD integration

**Definition:** События Git (push, PR, тег) запускают автоматизированные конвейеры сборки, тестирования и развертывания. Концепция: [10_build.md — Что такое CI/CD](./10_build_interview.md#what-is-cicd). YAML рабочего процесса: [10_build.md — YAML](./10_build_ci.md#yaml).

**When to use:** Каждый командный проект — запускайте lint/test для каждого PR; развернуть теги в производство.

**Examples:**

```yaml
# GitHub Actions — shallow fetch
- uses: actions/checkout@v4
  with:
    fetch-depth: 0   # full history for versioning/changelog

# Deploy on tag push
on:
  push:
    tags: ['v*']
```

* Кэш `node_modules` введен хеш файла блокировки
* Запускайте lint/test для каждого PR.
* Развертывание `main` к постановке; теги для производства

**Sources:** [Действия GitHub — оформление заказа](https://github.com/actions/checkout)

</details>

---

<a name="interview-practice"></a>
