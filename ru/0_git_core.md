# Git — Core

Объекты, коммиты и повседневный рабочий процесс.

[ ← Вернуться в хаб](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Ветвление](./0_git_branching.md) · [Git — Команда](./0_git_team.md) · [Git — Интервью](./0_git_interview.md)

### Sections

- [Core](#core)
- [Workflow](#workflow)

---

<a name="fundamentals"></a>

## Core

<details>
  <summary>Version control types</summary>

### Version control types

**Definition:**

Контроль версий отслеживает изменения файлов с течением времени, поэтому команды могут сотрудничать, откатывать изменения и проверять историю.

| Тип | Пример | Характеристика |
|------|---------|----------------|
| Местный | РКС | Одна машина, снимки файлов |
| Централизованный | СВН, Перфорс | Один сервер; клиенты проверяют файлы |
| Распределенный | Гит, Меркуриал | Полная история каждого клона |

**When to use:** Распределенная система контроля версий (Git) для современных команд разработчиков — работа в автономном режиме, дешевое ветвление, полная локальная история.

**Sources:** [Atlassian — Что такое контроль версий](https://www.atlassian.com/git/tutorials/what-is-version-control)

</details>

<details>
  <summary>Three trees</summary>

### Three trees

**Definition:**

Git управляет тремя разными деревьями. Их понимание — ключ к пониманию каждой команды Git.

| Дерево | Расположение | Роль |
|------|----------|------|
| **Рабочий каталог** | Файлы проекта на диске | Где вы редактируете |
| **Индекс (площадка подготовки)** | `.git/index` | Что входит в следующий коммит |
| **Репозиторий** | `.git/` | Коммиты, ветки, теги, полная история |

**When to use:** Всегда — каждый `add`, `commit`, `diff`, и `restore` команда перемещает данные между этими деревьями.

**Examples:**

```bash
# Two-step commit: working → index → repository
git add file.ts                   # working → index
git commit -m "feat: update file" # index → repository
```

**Sources:** [Pro Git — Три дерева](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified)

</details>

<details>
  <summary>File states</summary>

### File states

**Definition:**

Каждый отслеживаемый файл находится в одном из четырех состояний относительно последней фиксации.

| Государство | Описание |
|-------|-------------|
| Неотслеживаемый | Еще не в Git |
| Немодифицированный | То же, что и последний коммит |
| Изменено | Изменено в рабочем каталоге |
| Постановочный | Добавлено в индекс, готово к фиксации |

**When to use:** Проверьте с `git status` перед каждым коммитом знать, что будет включено.

**Examples:**

```bash
git status -s
#  M file.ts    — modified, not staged
# M  file.ts    — staged
# ?? new.ts     — untracked
```

**Sources:** [Pro Git — жизненный цикл статуса файла](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

<details>
  <summary>Object model</summary>

### Object model

**Definition:**

Git хранит **снимки**, а не различия. Четыре типа объектов живут в `.git/objects/`. Каждый объект адресуется **хешем SHA-1**. Коммиты формируют **DAG** через родительские указатели и являются **неизменяемыми**.

| Объект | Содержит |
|--------|----------|
| **Капля** | Содержимое файла |
| **Дерево** | Список каталогов (имена → хэши blob/tree) |
| **Принять** | Хэш дерева, родитель(и), автор, коммиттер, сообщение |
| **Тег** | Указатель на фиксацию + необязательная подпись GPG |

**When to use:** Полезно при проверке истории на уровне сантехники или отладке поврежденных репозиториев с помощью `git fsck`.

**Examples:**

```bash
git rev-parse HEAD                # commit SHA
git cat-file -p HEAD^{tree}       # root tree of HEAD
git ls-tree HEAD                  # list files in HEAD tree
```

**Sources:** [Pro Git — Объекты Git](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)

</details>

<details>
  <summary>Pointers (refs)</summary>

### Pointers (refs)

**Definition:**

Ссылки — это метки, указывающие на коммиты. Ветви двигаются; теги остаются фиксированными.

| Ссылка | Значение |
|-----|---------|
| `HEAD` | Текущий коммит (или ветка, указывающая на текущий коммит) |
| `ORIG_HEAD` | Где HEAD был раньше `merge`/`reset`/`rebase` |
| **Филиал** | Подвижный указатель на коммит (`main`, `feature/foo`) |
| **Тег** | Исправлен указатель на коммит (`v1.0.0`) |
| **Отдельная ГОЛОВКА** | HEAD указывает непосредственно на коммит, а не на ветку |

**When to use:** `ORIG_HEAD` для отмены неудачного слияния; отсоединенный HEAD при проверке конкретного коммита.

**Examples:**

```bash
git switch --detach abc1234         # detached HEAD at commit
git switch main                     # re-attach HEAD to branch
```

**Sources:** [Pro Git — Ссылки на Git](https://git-scm.com/book/en/v2/Git-Internals-Git-References)

</details>

<details>
  <summary>Git vs GitHub</summary>

### Git vs GitHub

**Definition:**

Git — инструмент контроля версий; GitHub/GitLab/Bitbucket — это платформы хостинга и совместной работы, построенные на базе Git.


| | Гит | GitHub/GitLab/Bitbucket |
| | --- | ----- |
| | Что | Инструмент контроля версий (CLI) | Хостинг + платформа для совместной работы |
| | Бежит | Локально на вашем компьютере | Удаленный сервер |
| | Магазины | Полная история в `.git/` | Удаленная копия репо + PR, CI, выпуски |

**When to use:** Git для всего локального контроля версий; удаленная платформа для координации команды, PR и CI/CD.

**Sources:** [Документация GitHub — использование Git](https://docs.github.com/en/get-started/using-git)

</details>

---

<a name="workflow"></a>

### Git internals & DAG

**Definition:**

Git хранит историю проекта в виде **направленного ациклического графа (DAG)** неизменяемых **фиксируемых** объектов. Каждый коммит указывает на **дерево** (снимок каталога) и один или несколько родительских коммитов. Объекты адресуются по содержимому с помощью хэшей **SHA-1** и живут в `.git/objects/`.

| Объект | Содержит |
|--------|----------|
| **Капля** | Содержимое файла |
| **Дерево** | Список каталогов (имена → хэши blob/tree) |
| **Принять** | Хэш дерева, родитель(и), автор, коммиттер, сообщение |
| **Тег** | Указатель на фиксацию + необязательная подпись GPG |

| Дерево | Расположение | Роль |
|------|----------|------|
| **Рабочий каталог** | Файлы проекта на диске | Где вы редактируете |
| **Индекс (площадка подготовки)** | `.git/index` | Что входит в следующий коммит |
| **Репозиторий** | `.git/` | Коммиты, ветки, теги, полная история |

**When to use:** Понимание внутреннего устройства помогает в восстановлении (`reflog`), разрешение конфликтов и знание причин `rebase` переписывает историю. Идентификаторы объектов представляют собой **криптографические хэши содержимого** (адресация, устойчивая к коллизиям). Это целостность, а не хеширование пароля — [Шифрование](./7_special_security.md#encryption).

**Examples:**

```bash
git cat-file -t HEAD          # type: commit
git cat-file -p HEAD          # pretty-print commit object
git log --graph --oneline --all
```

**Sources:** [Pro Git — Git Internals](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain) · [git-scm.com](https://git-scm.com/)

### Branching & merge

**Definition:**

**Ветка** — это подвижный указатель на коммит. **Слияние** объединяет истории ветвей (возможно создание фиксации слияния); **rebase** воспроизводит коммиты на другую базу для линейной истории.


| | `git merge` | `git rebase` |
| | --- | ------------- |
| | История | Сохраняет топологию ветвей | Линейное воспроизведение коммитов |
| | Объединить фиксацию | Да (если не перемотать вперед) | Нет |
| | Переписывает SHA | Нет | Да |
| | Безопасно в общих ветках | Да | **Нет** — переписывает историю |

**When to use:**

Ветка для каждой функции/исправления. Объединение в общие ветки после проверки. Перебазируйте **локальные/частные** ветки перед открытием PR.

Никогда не перебазируйте коммиты, которые уже сделали другие. Никогда `reset --hard` в общих ветках.

**Examples:**

```bash
git switch -c feature/login
# ... work, commit ...
git fetch origin
git rebase origin/main          # update feature branch (local only)
git switch main && git merge feature/login
```

**Sources:**

[Pro Git — ветвление](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) · [Pro Git — перебазирование](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

---

## Workflow

<details>
  <summary>Git CLI daily workflow</summary>

### Git CLI daily workflow

**Definition:** Git CLI — это терминальный интерфейс для Git — портативного инструмента на серверах и CI, где нет графического интерфейса. Графические интерфейсы (GitHub Desktop, GitKraken, Sourcetree, Fork, Tower, Sublime Merge) не являются обязательными; одни и те же команды выполняются везде.

**How it works:**

Команды используют **удаленный** репозиторий (размещенную копию, а не «удаленную»). Каждый разработчик клонирует его, работает над **ветвью**, этапирует, фиксирует, отправляет на проверку, затем объединяет и удаляет ветку.

Типичный цикл:

1. **Клонируйте** один раз, а затем **извлеките** позже для интеграции удаленных обновлений.
2. **Ответить** от текущего HEAD, чтобы параллельная работа меньше конфликтовала.
3. **Редактировать** файлы; **статус**, чтобы увидеть неотслеживаемые/измененные/поставленные.
4. **`git add`** копирует изменения рабочего дерева в индекс (новые файлы не отслеживаются, пока не будут добавлены).
5. **`git commit`** замораживает индекс в истории.
6. **`git push`** издает ветку; для первого нажатия новой ветки требуется восходящий поток (`-u`).
7. После проверки **объедините** в ветку интеграции, а затем **удалите** ветку функций.

**Конфликт** — это когда два человека изменили одни и те же строки — Git не может автоматически выбрать победителя.

Дочерние ветки сохраняют историю родительских ветвей **на момент их создания**. Более поздние родительские коммиты не наследуются до тех пор, пока вы не объедините (или не перебазируете) их; дочерние коммиты остаются в стороне от родителя, пока вы не сольетесь другим способом.

**When to use:**

Ежедневная функциональная работа; SSH-сессии и CI; обучение общему командному словарю.

#### Common pitfalls

* `git pull --all` извлекает **все пульты**; он по-прежнему только **интегрируется в текущую ветку** (объединяет или перебазирует вышестоящую ветку). Он не обновляет каждую локальную ветку.
* `git add .` относится к **текущему каталогу**. `git add --all` / `-A` обновляет индекс для **всего рабочего дерева** (добавления, изменения, удаления).
* Голый `git push` требуется настроенный восходящий поток (`push.default` is `simple` начиная с Git 2.0). Git печатает `git push -u origin <branch>` когда отсутствует.
* `git switch` существует начиная с **Git 2.23**; `checkout` все еще работает, но также выполняет восстановление/отключение. Предпочитать `switch` / `restore`.
* На дисках без учета регистра (`core.ignorecase`, типичный macOS/Windows), переименование `Git-CLI` → `git-cli` может выглядеть как бездействие. Использовать `git mv`:

```bash
git mv Git-CLI tmp && git mv tmp git-cli
```

**Pluses and minuses:**

| + | − |
|------|------|
| Одни и те же команды на всех ОС и серверах | Легко запускать деструктивные флаги (`reset --hard`) |
| Возможность сценариев, работает без графического интерфейса | Новый филиал `push` не работает, пока не будет установлен восходящий поток |
| Точная постановка(`add -p`) | Переименования только с учетом регистра могут быть невидимыми в macOS/Windows |

**Examples:**

```bash
# First copy of the remote (SSH or HTTPS)
git clone git@github.com:org/repo.git
cd repo

# Later: fetch all remotes, then merge the current branch's upstream
git pull --all

# Branch (Git 2.23+: switch; older Git: checkout)
git switch -c new-user-profile      # create + switch
# equivalent: git branch new-user-profile && git switch new-user-profile
# pre-2.23:   git checkout -b new-user-profile

git status
git add filename.js                 # or: git add .   (this directory tree)
git add --all                       # entire working tree: add, modify, delete
git commit                          # opens editor for the message
git commit -m "feat: rename getUser"

git push                            # may print the -u command if no upstream
git push -u origin new-user-profile

# Integrate parent into the feature branch, then feature into main
git merge main                      # while on new-user-profile
git switch main
git merge new-user-profile
git branch -d new-user-profile      # delete after it is merged
```

**Thoughts:** Клонировать → ветка → этап → зафиксировать → нажать → объединить → удалить ветку. GUI — это комфорт; CLI — это контракт.

**Sources:** [Pro Git — запись изменений](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) · [git-switch](https://git-scm.com/docs/git-switch) · [git-add](https://git-scm.com/docs/git-add) · [git-push](https://git-scm.com/docs/git-push) · [git-pull](https://git-scm.com/docs/git-pull)

</details>

<details>
  <summary>Initial configuration</summary>

### Initial configuration

**Definition:** Единовременная настройка удостоверения, ветки по умолчанию, стратегии извлечения и окончания строк.

**When to use:** При первой установке Git на новую машину.

**Examples:**

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase false   # or true for rebase-on-pull

# Windows line endings
git config --global core.autocrlf true

# macOS / Linux
git config --global core.autocrlf input
```

**Sources:** [Pro Git — первая настройка Git](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

</details>

<details>
  <summary>Vim in Git output</summary>

### Vim in Git output

**Definition:**

Длинный вывод (`git log -p`, интерактивное перебазирование) открывает Vim в качестве пейджера/редактора по умолчанию.

| Ключ/команда | Действие |
|---------------|--------|
| `Esc` | Командный режим |
| `i` | Режим вставки |
| `:q` | Выйти |
| `:q!` | Выйти без сохранения |
| `:wq` | Сохранить и выйти |

**When to use:** Всякий раз, когда Git открывает редактор для сообщений о фиксации или интерактивного перебазирования.

**Sources:** [Pro Git — конфигурация Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

</details>

<details>
  <summary>Commit conventions</summary>

### Commit conventions

**Definition:**

Структурированные сообщения о фиксации делают историю доступной для поиска и позволяют автоматически вести журналы изменений. Следуйте [Обычным коммитам](https://www.conventionalcommits.org/en/v1.0.0/).

| Префикс | Когда использовать | Пример |
|--------|-------------|---------|
| `init:` | Начать проект/спринт/задачу | `init: start sprint-1` |
| `feat:` | Новый функционал | `feat: implement search box` |
| `fix:` | Исправление ошибки | `fix: relayout header for firefox` |
| `refactor:` | Никаких изменений в поведении | `refactor: rename vars for readability` |
| `docs:` | Только документация | `docs: update readme` |
| `chore:` | Оснастка вспомогательная | `chore: bump eslint` |
| `test:` | Только тесты | `test: add login unit tests` |
| `ci:` | Конфигурация CI/CD | `ci: add deploy workflow` |

**When to use:** Каждый коммит по командным проектам. Используйте повелительное наклонение: `add feature` нет `added feature`.

**Examples:**

```bash
git commit -m "feat!: remove legacy auth endpoint"
# Breaking change: append ! or add BREAKING CHANGE: in footer
```

**Sources:** [Обычные коммиты](https://www.conventionalcommits.org/en/v1.0.0/) · [Руководство по стилю коммитов (Эрик ВонБ)](https://gist.github.com/ericavonb/3c79e5035567c8ef3267)

</details>

<details>
  <summary>Create repository & clone</summary>

### Create repository & clone

**Definition:** `git init` создает новый локальный репозиторий; `git clone` копирует существующий удаленный репозиторий, включая полную историю.

**When to use:** `init` для новых проектов; `clone` при присоединении к существующему проекту или настройке CI. Ежедневный цикл: [ежедневный рабочий процесс Git CLI](#workflow).

**Examples:**

```bash
git init                          # init in current directory
git init folder-name              # init in specified directory

git clone git@github.com:user/repo.git
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git FolderName
git clone https://github.com/user/repo.git .   # into current dir
git clone --depth 1 URL           # shallow clone (CI)
```

**Sources:** [Pro Git — Получение репозитория Git](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)

</details>

<details>
  <summary>Inspect changes</summary>

### Inspect changes

**Definition:** `git status` и `git diff` показать, что и где изменилось — рабочий каталог, индекс или между ветками.

**When to use:** Перед каждым коммитом и при проверке PR на местном уровне.

**Examples:**

```bash
git status
git diff                          # working dir vs index
git diff --staged                 # index vs HEAD (staged changes)
git diff HEAD                     # working dir vs HEAD
git diff --color-words
git diff main feature             # compare branches
git diff main...feature           # changes on feature since divergence
git diff --name-only main feature
```

**Sources:** [Pro Git — просмотр истории коммитов](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

</details>

<details>
  <summary>Stage changes</summary>

### Stage changes

**Definition:** `git add` копирует изменения из рабочего каталога в индекс (промежуточную область). Новые файлы остаются **неотслеживаемыми**, пока не будут добавлены. Игнорируемые пути пропускаются, если только `-f`.

**When to use:** Перед каждым коммитом. Использовать `-p` для выборочной постановки кусков.

**Examples:**

```bash
git add path/to/file
git add .                         # this directory and below
git add --all                     # whole working tree: add, modify, delete (-A)
git add -u                        # tracked files only (no new untracked)
git add -p                        # interactive hunk staging
git add -i                        # interactive shell
```

**Sources:** [Pro Git — запись изменений](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

<details>
  <summary>Unstage & discard</summary>

### Unstage & discard

**Definition:** Выключить сцену с помощью `restore --staged` or `reset`; отменить рабочие изменения с помощью `restore`. `reset --hard` и `clean` навсегда уничтожить незавершенную работу.

**How it works:** `git reset --hard` навсегда отменяет все незафиксированные изменения в отслеживаемых файлах. `git clean -df` удаляет неотслеживаемые файлы — тоже необратимо.

**When to use:** Выключите сцену, если вы добавили слишком много; отбросить, если вы хотите отменить локальные изменения. **Никогда** не используйте `--hard` если вы не уверены.

**Examples:**

```bash
git restore --staged file         # unstage (modern)
git reset file                    # unstage (classic)
git restore file                  # discard working changes (modern)
git checkout -- file              # discard working changes (classic)
git clean -df                     # remove untracked files/dirs
git reset --hard                  # DANGER: discard all uncommitted changes
```

**Sources:** [Pro Git — Отмена действий](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Commit</summary>

### Commit

**Definition:** `git commit` замораживает индекс в новый неизменяемый объект фиксации в репозитории.

**When to use:** Когда логическая единица работы завершена и поставлена ​​на этап. Изменяйте только неотправленные коммиты. Голый `git commit` открывает настроенный редактор; `-m` предназначен для короткого однострочного сообщения.

**Examples:**

```bash
git commit                        # editor for the message
git commit -m "feat: add login form"
git commit -a -m "fix: typo"      # stage tracked files only, then commit
git commit --amend -m "new message"   # rewrite last commit (unpushed only!)
git commit --signoff              # DCO sign-off line
```

**Sources:** [Pro Git — запись изменений](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

<details>
  <summary>Stash</summary>

### Stash

**Definition:** `git stash` временно откладывает незафиксированные изменения, чтобы вы могли переключать ветки без фиксации WIP.

**When to use:** Срочное переключение контекста, удаление грязного рабочего дерева или сохранение экспериментов.

**Examples:**

```bash
git stash
git stash push -m "wip login"
git stash -u                        # include untracked
git stash list
git stash show -p stash@{0}
git stash apply stash@{0}
git stash pop
git stash drop stash@{0}
git stash branch feature/wip stash@{0}
```

**Sources:** [Pro Git — хранение и очистка](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

</details>

<details>
  <summary>History & blame</summary>

### History & blame

**Definition:** `git log`, `git show`, и `git blame` проверить историю коммитов и построчное авторство.

**When to use:** Отладка регрессий, понимание владения кодом, подготовка примечаний к выпуску.

**Examples:**

```bash
git log --oneline -10
git log --graph --oneline --all
git log -p -5
git log --since=2.weeks
git log --grep "fix" -i
git log -S "functionName" -p       # pickaxe: commits changing string
git log main..feature              # commits on feature not in main
git show abc123
git show HEAD~2:path/file         # file content at commit
git blame file.ts -L 10,20
```

Нажимать `q` для выхода из длинного вывода пейджера.

**Sources:** [Pro Git — просмотр истории коммитов](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

</details>

<details>
  <summary>Remove & move files</summary>

### Remove & move files

**Definition:** `git rm` останавливает отслеживание и удаляет файлы; `git mv` переименования с сохранением истории. `--cached` удаляет только из индекса.

**When to use:**

Удаление функций, переименование модулей, отмена случайно добавленных секретов.

#### Common pitfalls

Переименование только регистра в Finder/Explorer часто приводит к пустой фиксации. Двухшаговый `git mv` (or `git mv -f Old old` в некоторых настройках) записывает изменения.

**Examples:**

```bash
git rm file.txt
git rm -r folder/
git rm --cached secret.env        # stop tracking; keep file on disk
git mv old.txt new.txt

# Case-only rename on case-insensitive FS (macOS/Windows, core.ignorecase)
git mv Git-CLI tmp && git mv tmp git-cli
```

**Sources:** [Pro Git — запись изменений](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

---

<a name="branching"></a>
