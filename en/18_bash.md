# Bash

Scripts, tests, loops, and pipelines for the Bourne-again shell. This is one handbook topic, not a command encyclopedia.

[← DevOps](./15_devops.md)

### Related docs

- [DevOps](./15_devops.md) · [Unix processes](./15_devops.md#unix-process-management) · [Git](./0_git_core.md)

### Main concepts

| Area        | Topics                          |
| ----------- | ------------------------------- |
| **Run**     | Shebang, executable bit         |
| **Data**    | Variables, quoting, exit status |
| **Control** | `[[ ]]`, `if`, `for`, `while`   |
| **I/O**     | Redirects and pipes             |

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

**Definition:** A shebang is the first line of a script. It names the program that should run the file.

**How it works:**

- `#!/usr/bin/env bash` asks `env` to find `bash` on `PATH`. That is more portable than a hard-coded `/bin/bash`.
- The line must be the first bytes of the file. A blank line or a BOM before it means the kernel will not treat it as a shebang.
- The file needs the executable bit (`chmod +x script.sh`). Then `./script.sh` runs it. `bash script.sh` runs it even without that bit, and then the shebang is just a comment.

**When to use:** Any script you expect to execute directly.

**Pluses and minuses:**

| +                                | −                                           |
| -------------------------------- | ------------------------------------------- |
| The kernel picks the interpreter | `env bash` fails if `bash` is not on `PATH` |

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

**Definition:** A shell variable is a named string. Every command finishes with an exit status: `0` means success, any other value means failure.

**How it works:**

- Assignment has no spaces: `name=ada`. `name = ada` runs a command named `name`.
- `"$name"` keeps spaces inside the value. Unquoted `$name` splits on whitespace and expands globs.
- `$?` is the exit status of the most recent command. It changes after the next command, so save it if you need it later (`status=$?`).
- `set -euo pipefail` at the top of a script exits on a failed command, on an unset variable, and on a failure in a pipeline (not only the last command).

**When to use:** Quote expansions by default. Turn on `set -euo pipefail` in scripts that should stop on the first error.

**Pluses and minuses:**

| +                              | −                                         |
| ------------------------------ | ----------------------------------------- |
| `set -euo pipefail` fails fast | `set -e` has exceptions (some `if` tests) |

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

**Definition:** `if` runs a command and takes the `then` branch when that command’s exit status is `0`. `[[ ]]` is Bash’s conditional test.

**How it works:**

- `[[ -f path ]]` is true when `path` is a regular file. `[[ -d path ]]` is a directory. `[[ -z $var ]]` is true when the string is empty.
- `[[ $a == "$b" ]]` compares strings. Use `-eq`, `-lt`, `-gt` for integers inside `[[ ]]`.
- `&&` and `||` combine commands by exit status. `cmd1 && cmd2` runs `cmd2` only when `cmd1` succeeds.
- Prefer `[[ ]]` in Bash scripts. `[` is the older `test` command and needs more quoting.

**When to use:** Branch on files, strings, and numbers. Do not parse `ls` output to test for a file.

**Pluses and minuses:**

| +                            | −                                |
| ---------------------------- | -------------------------------- |
| `[[ ]]` does not split words | It is Bash, not plain POSIX `sh` |

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

**Definition:** `for` walks a list of words. `while` repeats while a command succeeds.

**How it works:**

- `for item in "$@"; do` walks the script’s arguments. `"$@"` keeps each argument intact, including spaces.
- `for file in *.txt; do` walks names the shell expanded. If nothing matches, the pattern can stay literal unless `nullglob` is set.
- `while IFS= read -r line; do` reads one line at a time. `read -r` does not treat backslashes as escapes.
- `break` leaves the loop. `continue` starts the next iteration.

**When to use:** Arguments and filenames with `for`. Lines of a file or a command’s output with `while read`.

**Pluses and minuses:**

| +                                | −                                          |
| -------------------------------- | ------------------------------------------ |
| `"$@"` keeps argument boundaries | `for line in $(cat file)` splits and globs |

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

**Definition:** Redirection sends a command’s output or input to a file. A pipe sends one command’s standard output to the next command’s standard input.

**How it works:**

- `>` writes standard output to a file and truncates it. `>>` appends. `<` reads standard input from a file.
- File descriptor `2` is standard error. `2>` sends it to a file. `>` then `2>&1` sends both to the same place (the order matters: `2>&1 >file` does not).
- `cmd1 | cmd2` connects stdout of `cmd1` to stdin of `cmd2`. With `set -o pipefail`, the pipeline fails if any command in it fails.
- A pipe runs the commands in subshells for the parts of the pipeline. A variable set in the middle of a pipe does not survive in the parent shell.

**When to use:** Save logs, chain filters (`sort | uniq`), and keep errors with the output you are capturing.

**Pluses and minuses:**

| +                                  | −                                           |
| ---------------------------------- | ------------------------------------------- |
| Filters compose without temp files | Variables set inside a pipe do not leak out |

**Examples:**

```bash
sort names.txt | uniq > unique.txt
grep -n error app.log > errors.txt 2>&1
```

**Sources:** [Bash manual — redirections](https://www.gnu.org/software/bash/manual/bash.html#Redirections) · [Pipelines](https://www.gnu.org/software/bash/manual/bash.html#Pipelines)

</details>
