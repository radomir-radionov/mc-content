# Bash

Скрипты, проверки, циклы и конвейеры Bourne-again shell. Это одна тема справочника, не энциклопедия команд.

[← DevOps](./15_devops.md)

### Related docs

- [DevOps](./15_devops.md) · [Unix processes](./15_devops.md#unix-process-management) · [Git](./0_git_core.md)

### Main concepts

| Область    | Темы                            |
| ---------- | ------------------------------- |
| **Запуск** | Shebang, бит исполняемости      |
| **Данные** | Переменные, кавычки, код выхода |
| **Ветки**  | `[[ ]]`, `if`, `for`, `while`   |
| **Ввод**   | Редиректы и конвейеры           |

### Sections

- [Shebang](#shebang)
- [Variables and exit status](#variables-and-exit-status)
- [Tests and conditionals](#tests-and-conditionals)
- [Loops](#loops)
- [Redirection and pipes](#redirection-and-pipes)

<a name="shebang"></a>

## Shebang

<details>
  <summary>Shebang</summary>

### Shebang

**Definition:** Shebang — первая строка скрипта. Она называет программу, которая должна выполнить файл.

**How it works:**

- `#!/usr/bin/env bash` просит `env` найти `bash` в `PATH`. Так переносимее, чем жёсткий `/bin/bash`.
- Строка должна быть первыми байтами файла. Пустая строка или BOM перед ней — и ядро не считает её shebang.
- Файлу нужен бит исполняемости (`chmod +x script.sh`). Тогда его запускает `./script.sh`. `bash script.sh` запускает файл и без этого бита, а shebang тогда просто комментарий.

**When to use:** Любой скрипт, который запускают напрямую.

**Pluses and minuses:**

| +                                | −                                                 |
| -------------------------------- | ------------------------------------------------- |
| Ядро само выбирает интерпретатор | `env bash` не сработает, если `bash` нет в `PATH` |

**Examples:**

```bash
#!/usr/bin/env bash
echo "hello"
```

**Sources:** [Bash manual — shell invocation](https://www.gnu.org/software/bash/manual/bash.html#Invoking-Bash)

</details>

<a name="variables-and-exit-status"></a>

## Variables and exit status

<details>
  <summary>Variables and exit status</summary>

### Variables and exit status

**Definition:** Переменная shell — именованная строка. Каждая команда заканчивается кодом выхода: `0` — успех, любое другое значение — ошибка.

**How it works:**

- В присваивании нет пробелов: `name=ada`. `name = ada` запускает команду с именем `name`.
- `"$name"` сохраняет пробелы внутри значения. Без кавычек `$name` режется по пробелам и раскрывает шаблоны.
- `$?` — код выхода последней команды. Он меняется после следующей команды, поэтому сохраняйте его (`status=$?`).
- `set -euo pipefail` в начале скрипта выходит при ошибке команды, при необъявленной переменной и при ошибке в конвейере (не только у последней команды).

**When to use:** По умолчанию берите подстановки в кавычки. Включайте `set -euo pipefail` в скриптах, которые должны остановиться на первой ошибке.

**Pluses and minuses:**

| +                                | −                                                  |
| -------------------------------- | -------------------------------------------------- |
| `set -euo pipefail` падает сразу | У `set -e` есть исключения (часть проверок в `if`) |

**Examples:**

```bash
name="Ada Lovelace"
echo "$name"
cp "$1" /tmp || status=$?
```

**Sources:** [Bash manual — shell parameters](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameters) · [The Set Builtin](https://www.gnu.org/software/bash/manual/bash.html#The-Set-Builtin)

</details>

<a name="tests-and-conditionals"></a>

## Tests and conditionals

<details>
  <summary>Tests and conditionals</summary>

### Tests and conditionals

**Definition:** `if` запускает команду и идёт в ветку `then`, если код выхода этой команды — `0`. `[[ ]]` — условная проверка Bash.

**How it works:**

- `[[ -f path ]]` истинно, если `path` — обычный файл. `[[ -d path ]]` — каталог. `[[ -z $var ]]` истинно для пустой строки.
- `[[ $a == "$b" ]]` сравнивает строки. Для целых чисел внутри `[[ ]]` используйте `-eq`, `-lt`, `-gt`.
- `&&` и `||` соединяют команды по коду выхода. `cmd1 && cmd2` запускает `cmd2`, только если `cmd1` успешна.
- В скриптах Bash предпочтительнее `[[ ]]`. `[` — старая команда `test`, и ей нужно больше кавычек.

**When to use:** Ветки по файлам, строкам и числам. Не разбирайте вывод `ls`, чтобы проверить файл.

**Pluses and minuses:**

| +                      | −                              |
| ---------------------- | ------------------------------ |
| `[[ ]]` не режет слова | Это Bash, не чистый POSIX `sh` |

**Examples:**

```bash
if [[ -f "$path" && -n "$name" ]]; then
  echo "ready"
elif [[ "$count" -gt 3 ]]; then
  echo "many"
else
  echo "missing"
fi
```

**Sources:** [Bash manual — conditional constructs](https://www.gnu.org/software/bash/manual/bash.html#Conditional-Constructs)

</details>

<a name="loops"></a>

## Loops

<details>
  <summary>Loops</summary>

### Loops

**Definition:** `for` проходит список слов. `while` повторяется, пока команда успешна.

**How it works:**

- `for item in "$@"; do` проходит аргументы скрипта. `"$@"` сохраняет каждый аргумент целиком, включая пробелы.
- `for file in *.txt; do` проходит имена, которые раскрыл shell. Если ничего не совпало, шаблон может остаться буквальным, пока не включён `nullglob`.
- `while IFS= read -r line; do` читает по одной строке. `read -r` не считает обратную косую экранированием.
- `break` выходит из цикла. `continue` начинает следующую итерацию.

**When to use:** Аргументы и имена файлов — через `for`. Строки файла или вывода команды — через `while read`.

**Pluses and minuses:**

| +                                | −                                                          |
| -------------------------------- | ---------------------------------------------------------- |
| `"$@"` хранит границы аргументов | `for line in $(cat file)` режет текст и раскрывает шаблоны |

**Examples:**

```bash
for arg in "$@"; do
  echo "$arg"
done

while IFS= read -r line; do
  echo "$line"
done < notes.txt
```

**Sources:** [Bash manual — looping constructs](https://www.gnu.org/software/bash/manual/bash.html#Looping-Constructs)

</details>

<a name="redirection-and-pipes"></a>

## Redirection and pipes

<details>
  <summary>Redirection and pipes</summary>

### Redirection and pipes

**Definition:** Редирект отправляет вывод или ввод команды в файл. Конвейер отправляет стандартный вывод одной команды на стандартный ввод следующей.

**How it works:**

- `>` пишет стандартный вывод в файл и обрезает его. `>>` дописывает. `<` читает стандартный ввод из файла.
- Дескриптор `2` — стандартный поток ошибок. `2>` отправляет его в файл. Сначала `>`, потом `2>&1` отправляет оба в одно место (порядок важен: `2>&1 >file` так не делает).
- `cmd1 | cmd2` соединяет stdout `cmd1` со stdin `cmd2`. С `set -o pipefail` конвейер падает, если упала любая команда в нём.
- Части конвейера работают в подоболочках. Переменная, заданная в середине конвейера, не остаётся в родительском shell.

**When to use:** Сохранять логи, сцеплять фильтры (`sort | uniq`) и забирать ошибки вместе с выводом.

**Pluses and minuses:**

| +                                         | −                                            |
| ----------------------------------------- | -------------------------------------------- |
| Фильтры складываются без временных файлов | Переменные внутри конвейера не видны снаружи |

**Examples:**

```bash
sort names.txt | uniq > unique.txt
grep -n error app.log > errors.txt 2>&1
```

**Sources:** [Bash manual — redirections](https://www.gnu.org/software/bash/manual/bash.html#Redirections) · [Pipelines](https://www.gnu.org/software/bash/manual/bash.html#Pipelines)

</details>
