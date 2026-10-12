# Git — hub

Контроль версий, история, совместная работа и восстановление. Теория находится в файлах тем ниже.

### Related docs

[Git — Core](./0_git_core.md) · [Git — Ветвление](./0_git_branching.md) · [Git — Команда](./0_git_team.md) · [Git — Интервью](./0_git_interview.md)

### Navigation

| Тема                                    | Обложки                                                     |
| --------------------------------------- | ----------------------------------------------------------- |
| [Git — Core](./0_git_core.md)           | Объекты, коммиты и повседневный рабочий процесс.            |
| [Git — Ветвление](./0_git_branching.md) | Филиалы, слияния, удаленное управление и совместная работа. |
| [Git — Команда](./0_git_team.md)        | Восстановление, командные практики, хуки и продвинутый Git. |
| [Git — Интервью](./0_git_interview.md)  | Вопросы по Git, ответы не заполнены.                        |

### Interview practice

Вопросы и ответы находятся в теме интервью по этой теме.

---

## Resources & links

<details>
  <summary>Primary references</summary>

- [Официальный сайт Git](https://git-scm.com/)
- [Книга о Git](https://git-scm.com/book/en/v2)
- [Документация GitHub — использование Git](https://docs.github.com/en/get-started/using-git)
- [Учебные пособия по Atlassian Git](https://www.atlassian.com/git/tutorials/what-is-version-control)
- [Глоссарий Atlassian Git](https://www.atlassian.com/git/glossary)
- [Обычные коммиты](https://www.conventionalcommits.org/en/v1.0.0/)\* [Основная шпаргалка по Git (ByteByteGo)](https://blog.bytebytego.com/i/162154465/essential-git-cheatsheet)
- [Руководство по стилю фиксации (Эрик ВонБ)](https://gist.github.com/ericavonb/3c79e5035567c8ef3267)

</details>

<details>
  <summary>Videos & courses</summary>

### Videos & courses

- [Плейлист курса Git](https://www.youtube.com/playlist?list=PLDyvV36pndZFHXjXuwA_NywNrVQO0aQqb)
- [Основные команды Git](https://www.youtube.com/watch?v=EsA-9E0umCo)
- [Развертывание страниц GitHub](https://www.youtube.com/watch?v=dLX40HYzETY)

</details>

<details>
  <summary>SSH setup</summary>

### SSH setup

Протокол, `authorized_keys`, `sshd_config`и усиление защиты сервера: [DevOps — SSH](./15_devops.md#ssh). Этот блок предназначен только для удаленных устройств GitHub/Git.

- [Git — Генерация ключей SSH](https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key)
- [GitHub — Генерация нового ключа SSH] (https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)\* [GitHub — Добавление нового ключа SSH] (https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

# macOS / Windows / Linux clipboard
pbcopy < ~/.ssh/id_ed25519.pub
clip < ~/.ssh/id_ed25519.pub
xclip -selection clipboard < ~/.ssh/id_ed25519.pub

# or: gh ssh-key add ~/.ssh/id_ed25519.pub --title "laptop"
```

Пользовательский интерфейс GitHub: **Настройки → Ключи SSH и GPG → Новый ключ SSH** — вставьте **`.pub`** только файл. Удаленный URL-адрес: `git@github.com:user/repo.git`.

</details>

### Sections

> Теория `<details>` блоки следуют: **Определение** · **Как это работает** · **Когда использовать** · **Плюсы и минусы** · **Примеры** · **Мысли** · **Источники**. **Определение** обязательно; другие разделы являются необязательными.

- [Ядро](./0_git_core.md#core)
- [Рабочий процесс](./0_git_core.md#workflow) — [ежедневный цикл Git CLI](./0_git_core.md#workflow)
- [Ветвление и слияние](./0_git_core.md#branching)
- [Удаленный](./0_git_branching.md#remote)- [Восстановление](./0_git_team.md#recovery)
- [Командные практики](./0_git_team.md#team)
- [Крючки и инструменты](./0_git_team.md#hooks-tooling)
- [Продвинутые темы](./0_git_team.md#advanced)
- [Interview Practice](./0_git_interview.md#interview-practice)
