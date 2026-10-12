# Git — Branching

Филиалы, слияния, удаленное управление и совместная работа.

[ ← Вернуться в хаб](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Ветвление](./0_git_branching.md) · [Git — Команда](./0_git_team.md) · [Git — Интервью](./0_git_interview.md)

### Sections

- [Branching & merge](#branching-merge)
- [Remote](#remote)

---

## Branching & merge

<details>
  <summary>Branches</summary>

### Branches

**Definition:** Ветка — это подвижный указатель на коммит. Создание веток обходится дешево — это просто ссылки, а не копии файлов.

**When to use:** Одна ветка для каждой функции, исправления или эксперимента. Удалить после слияния.

**Examples:**

```bash
git branch
git branch -v
git branch feature/foo
git branch -d feature/foo           # delete merged branch
git branch -D feature/foo           # force delete
git branch -m old-name new-name
git branch --merged
git branch --no-merged
git branch -a                       # include remotes
```

**Sources:** [Pro Git — ветвление](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)

</details>

<details>
  <summary>Switch commits & branches</summary>

### Switch commits & branches

**Definition:** `git switch` (Git **2.23+**) или `checkout` (классический) перемещает HEAD в ветку или конкретный коммит. `checkout` также восстанавливает файлы и отключает HEAD; `switch` — это команда выделенной ветки.

**When to use:** Ежедневное переключение филиалов. Использовать `--detach` только для временного осмотра.

**Examples:**

```bash
git switch main                   # switch branch (modern)
git checkout main                 # switch branch (classic)
git switch -c feature/foo         # create + switch
git checkout -b feature/foo
git switch -c fix abc123          # branch from specific commit
git switch --detach abc123        # detached HEAD at commit
```

**Sources:** [git-scm.com — git-switch](https://git-scm.com/docs/git-switch) · [git-checkout](https://git-scm.com/docs/git-checkout)

</details>

<details>
  <summary>Merge</summary>

### Merge

**Definition:**

`git merge` интегрирует другую ветку в текущую ветку. Создает фиксацию слияния, если быстрая перемотка вперед невозможна.

| Стратегия | Результат | Когда |
|----------|--------|------|
| **Перемотка вперед** | Указатель ветки перемещается; нет фиксации слияния | Линейная история, никаких расходящихся коммитов |
| **Трёхстороннее слияние** | Новый коммит слияния с двумя родителями | Обе ветки имеют уникальные коммиты |
| **`--no-ff`** | Всегда создавать фиксацию слияния | Сохранить контекст ветки в истории |
| **Слияние сквоша** | Все коммиты веток → один коммит на целевой | Чистая история; потерять отдельные коммиты |

**When to use:** Интеграция готовых функций в текущую ветку после проверки. Одна и та же команда в обоих направлениях: объединить `main` **в** ветку функции, чтобы получить новые родительские коммиты или объединить функцию **в** `main` чтобы приземлить работу. `git merge <name>` всегда объединяет **эту** ветку с **HEAD**.

**Examples:**

```bash
git merge feature                   # merge named branch into current
git merge main                      # update feature branch with parent
git merge feature --no-ff           # always create merge commit
git merge feature --squash          # squash into one commit (manual commit after)
git merge --abort
```

**Sources:** [Pro Git — базовое слияние](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

</details>

<details>
  <summary>Rebase</summary>

### Rebase

**Definition:** `git rebase` воспроизводит коммиты из одной ветки в другую базу, создавая линейную историю с новыми SHA коммитов.

**How it works:** Rebase **переписывает историю**. Принудительное нажатие перебазированной ветки перезаписывает удаленные коммиты, на которых, возможно, основывались работы товарищей по команде. Золотое правило: никогда не перебазируйте коммиты, которые удалили другие.

**When to use:**

Обновите ветку **локальной/частной** функции до последней версии. `main` перед открытием PR. Очистите фиксации WIP с помощью интерактивного перебазирования.

На какую-то ветку потянули другие. Никогда не перебазируйте `main` или общие ветки выпуска.

**Examples:**

```bash
git rebase main
git rebase -i HEAD~3                # interactive: squash, reword, drop
git rebase --onto main feature old-base
git rebase --abort
git rebase --continue
git pull --rebase origin main
```

Интерактивные действия по перебазированию: `pick`, `reword`, `squash`, `fixup`, `drop`.

**Sources:** [Pro Git — перебазирование](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

</details>

<details>
  <summary>Merge vs rebase</summary>

### Merge vs rebase

**Definition:**

Две стратегии интеграции истории ветвей — слияние сохраняет топологию; rebase линеаризует.


| | `git merge` | `git rebase` |
| | --- | ------------- |
| | История | Сохраняет топологию ветвей | Линейное воспроизведение коммитов |
| | Объединить фиксацию | Да (кроме FF) | Нет |
| | Конфликты | Однажды во время слияния | За повторный коммит |
| | Безопасно в общих ветках | Да | **Нет** — переписывает историю |
| | Лучшее для | Интеграция готовых функций | Очистка местного отделения перед пиаром |

**When to use:** Объединение общих веток; перебазируйте ветки частных функций перед PR.

**Examples:**

```bash
# Update feature branch with latest main (rebase — clean history)
git switch feature/foo
git fetch origin
git rebase origin/main

# Update feature branch with latest main (merge — safe, preserves history)
git merge origin/main
```

**Sources:** [Pro Git — перебазирование](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

</details>

<details>
  <summary>Cherry-pick</summary>

### Cherry-pick

**Definition:** `git cherry-pick` применяет конкретный коммит из другой ветки к текущей ветке.

**When to use:** Бэкапирование исправления, внесение одного коммита без объединения всей ветки.

**Examples:**

```bash
git cherry-pick abc123
git cherry-pick main~2..main
git cherry-pick -n abc123         # stage without committing
git cherry-pick --abort
git cherry-pick --continue
```

**Sources:** [Pro Git — перебазирование](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

</details>

<details>
  <summary>Tags</summary>

### Tags

**Definition:** Теги — это фиксированные указатели на коммиты, обычно обозначающие релизы. Теги с аннотациями хранят метаданные и рекомендуются для выпусков.

**When to use:** Версии выпуска (`v1.0.0`). Легкие теги для временных закладок.

**Examples:**

```bash
git tag v1.0.0                      # lightweight
git tag -a -m "Release" v1.0.1      # annotated (recommended for releases)
git tag -d v1.0.0
git push origin v1.0.1
git push origin --tags
```

**Sources:** [Pro Git — тегирование](https://git-scm.com/book/en/v2/Git-Basics-Tagged)

</details>

<details>
  <summary>Git Flow</summary>

### Git Flow

**Definition:**

Модель ветвления с длительным сроком действия. `main` и `develop` ветки плюс недолговечные ветки функций, выпусков и исправлений.

| Филиал | Цель |
|--------|---------|
| `main` | Готово к производству |
| `develop` | Интеграционная отрасль |
| `feature/*` | Новые возможности от разработки |
| `release/*` | Подготовка к выпуску от разработки |
| `hotfix/*` | Срочные исправления на главной |

**When to use:** Запланированные выпуски с номерами версий. Тяжелый для групп непрерывного развертывания.

**Sources:** [Atlassian — Рабочий процесс Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

</details>

<details>
  <summary>GitHub Flow & trunk-based</summary>

### GitHub Flow & trunk-based

**Definition:**

Более простые модели, оптимизированные для непрерывной интеграции и развертывания.

| Модель | Узор |
|-------|---------|
| **Последовательность действий на GitHub** | `main` + недолговечные функциональные ветки → PR → слияние → развертывание |
| **На основе магистральной линии** | Каждый обязуется `main` (или сливается ежедневно); флаги функций скрывают незавершенную работу |

**When to use:** Команды развертываются несколько раз в день. Для конвейеров CI/CD отдавайте предпочтение магистральным, а не Git Flow. Как это происходит в цикле доставки: [10_build.md — Что такое CI/CD](./10_build_interview.md#what-is-cicd).

**Sources:** [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow) · [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com/)

</details>

<details>
  <summary>Interactive rebase & squash</summary>

### Interactive rebase & squash

**Definition:** `git rebase -i` позволяет переупорядочивать, сжимать, переписывать или удалять коммиты перед слиянием.

**When to use:** Очистка коммитов WIP (`wip`, `fix typo`) в один логический коммит перед открытием PR. Только по ненажатой или частной истории.

**Examples:**

```bash
git rebase -i origin/main
```

В редакторе:

```
pick abc1234 feat: add form validation
squash def5678 wip
squash ghi9012 fix typo
reword jkl3456 feat: add login form
```

| Действие | Эффект |
|--------|--------|
| `squash` | Слияние с предыдущим коммитом; объединить сообщения |
| `fixup` | Слияние с предыдущим; удалить это сообщение |
| `pick` | Сохранить как есть |
| `drop` | Удалить фиксацию |

**Sources:** [Pro Git — переписывание истории](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

</details>

---

<a name="remote"></a>

## Remote

<details>
  <summary>Remote repositories</summary>

### Remote repositories

**Definition:** Пульты — это именованные ссылки на другие репозитории. `origin` является пультом по умолчанию для вашего форка/клона; `upstream` обычно используется для исходного репо при работе с форком.

**When to use:** Ежедневное нажатие/вытягивание. Предпочитайте SSH после настройки ключа ([SSH setup](#ssh-setup); протокол: [DevOps — SSH](./15_devops.md#ssh)).

**Examples:**

```bash
git remote -v
git remote add origin git@github.com:user/repo.git
git remote add upstream git@github.com:original/repo.git
git remote remove origin
git fetch origin
git fetch origin main
git fetch --all --prune
git push origin main
git push -u origin feature/foo      # set upstream
git push --force-with-lease         # safer force push
git pull origin main
git pull --rebase origin main
git branch --set-upstream-to=origin/main
```

**Sources:** [Pro Git — работа с удаленными устройствами](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

</details>

<details>
  <summary>Fetch vs pull vs push</summary>

### Fetch vs pull vs push

**Definition:**

`fetch` скачивает удаленные изменения без слияния; `pull` извлекает и интегрирует; `push` отправляет локальные коммиты на удаленный.

| Команда | Принести пульт? | Обновить местную ветку? | Объединить/перебазировать? |
|---------|---------------|----------------------|---------------|
| `git fetch` | Да | Нет | Нет |
| `git pull` | Да | Да | Да (объединить или перебазировать) |
| `git push` | Нет | Нет | Отправляет локальные коммиты на удаленные |

**When to use:** `fetch` если вы хотите проверить удаленные изменения перед интеграцией: `git log main..origin/main`. `git pull --all` извлекает каждый удаленный компьютер, а затем интегрирует только **текущую** ветку.

**Examples:**

```bash
git fetch origin
git fetch --all --prune
git pull --all                    # fetch all remotes, then merge/rebase current upstream
git log main..origin/main         # what's new on remote
git merge origin/main             # integrate after review
```

**Sources:** [Pro Git — работа с удаленными устройствами](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

</details>

<details>
  <summary>Sync fork with upstream</summary>

### Sync fork with upstream

**Definition:** Поддерживайте свою вилку в актуальном состоянии с исходным репозиторием, загружая из `upstream` и сливаюсь с твоим `main`.

**When to use:** Прежде чем начать новую работу над разветвленным проектом с открытым исходным кодом.

**Examples:**

```bash
git remote add upstream git@github.com:original/repo.git
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

**Sources:** [GitHub — Синхронизация вилки](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)

</details>

<details>
  <summary>Force push</summary>

### Force push

**Definition:** Перезаписывает историю удаленного филиала вашим локальным филиалом. `--force-with-lease` безопаснее, чем `--force` потому что это не удастся, если на пульте есть коммиты, которые вы не видели.

**When to use:** После перебазирования вашей собственной ветки функций перед обновлением PR. Никогда в общих ветках.

**Examples:**

```bash
git push --force-with-lease origin feature/foo
```

Включить **защищенные ветки** `main` чтобы полностью блокировать силовые толчки.

**Sources:** [git-scm.com — git-push](https://git-scm.com/docs/git-push)

</details>

---

<a name="recovery"></a>
