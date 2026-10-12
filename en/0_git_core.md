# Git — Core

Objects, commits, and everyday workflow.

[← Back to hub](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Branching](./0_git_branching.md) · [Git — Team](./0_git_team.md) · [Git — Interview](./0_git_interview.md)

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

Version control tracks file changes over time so teams can collaborate, revert, and audit history.

| Type | Example | Characteristic |
|------|---------|----------------|
| Local | RCS | Single machine, file snapshots |
| Centralized | SVN, Perforce | One server; clients check out files |
| Distributed | Git, Mercurial | Full history on every clone |

**When to use:** Distributed VCS (Git) for modern software teams — offline work, cheap branching, full local history.

**Sources:** [Atlassian — What is version control](https://www.atlassian.com/git/tutorials/what-is-version-control)

</details>

<details>
  <summary>Three trees</summary>

### Three trees

**Definition:**

Git manages three distinct trees. Understanding them is the key to understanding every Git command.

| Tree | Location | Role |
|------|----------|------|
| **Working directory** | Project files on disk | Where you edit |
| **Index (staging area)** | `.git/index` | What goes into the next commit |
| **Repository** | `.git/` | Commits, branches, tags, full history |

**When to use:** Always — every `add`, `commit`, `diff`, and `restore` command moves data between these trees.

**Examples:**

```bash
# Two-step commit: working → index → repository
git add file.ts                   # working → index
git commit -m "feat: update file" # index → repository
```

**Sources:** [Pro Git — The Three Trees](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified)

</details>

<details>
  <summary>File states</summary>

### File states

**Definition:**

Every tracked file is in one of four states relative to the last commit.

| State | Description |
|-------|-------------|
| Untracked | Not in Git yet |
| Unmodified | Same as last commit |
| Modified | Changed in working directory |
| Staged | Added to index, ready to commit |

**When to use:** Check with `git status` before every commit to know what will be included.

**Examples:**

```bash
git status -s
#  M file.ts    — modified, not staged
# M  file.ts    — staged
# ?? new.ts     — untracked
```

**Sources:** [Pro Git — File Status Lifecycle](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

<details>
  <summary>Object model</summary>

### Object model

**Definition:**

Git stores **snapshots**, not diffs. Four object types live in `.git/objects/`. Each object is addressed by a **SHA-1 hash**. Commits form a **DAG** via parent pointers and are **immutable**.

| Object | Contains |
|--------|----------|
| **Blob** | File content |
| **Tree** | Directory listing (names → blob/tree hashes) |
| **Commit** | Tree hash, parent(s), author, committer, message |
| **Tag** | Pointer to commit + optional GPG signature |

**When to use:** Useful when inspecting history at the plumbing level or debugging corrupt repos with `git fsck`.

**Examples:**

```bash
git rev-parse HEAD                # commit SHA
git cat-file -p HEAD^{tree}       # root tree of HEAD
git ls-tree HEAD                  # list files in HEAD tree
```

**Sources:** [Pro Git — Git Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)

</details>

<details>
  <summary>Pointers (refs)</summary>

### Pointers (refs)

**Definition:**

Refs are labels that point to commits. Branches move; tags stay fixed.

| Ref | Meaning |
|-----|---------|
| `HEAD` | Current commit (or branch that points to current commit) |
| `ORIG_HEAD` | Where HEAD was before `merge`/`reset`/`rebase` |
| **Branch** | Movable pointer to a commit (`main`, `feature/foo`) |
| **Tag** | Fixed pointer to a commit (`v1.0.0`) |
| **Detached HEAD** | HEAD points directly to a commit, not a branch |

**When to use:** `ORIG_HEAD` for undoing a bad merge; detached HEAD when checking out a specific commit for inspection.

**Examples:**

```bash
git switch --detach abc1234         # detached HEAD at commit
git switch main                     # re-attach HEAD to branch
```

**Sources:** [Pro Git — Git References](https://git-scm.com/book/en/v2/Git-Internals-Git-References)

</details>

<details>
  <summary>Git vs GitHub</summary>

### Git vs GitHub

**Definition:**

Git is the version control tool; GitHub/GitLab/Bitbucket are hosting and collaboration platforms built on top of Git.

| | Git | GitHub / GitLab / Bitbucket |
|---|-----|----------------------------|
| What | Version control tool (CLI) | Hosting + collaboration platform |
| Runs | Locally on your machine | Remote server |
| Stores | Full history in `.git/` | Remote copy of repo + PRs, CI, issues |

**When to use:** Git for all local version control; remote platform for team coordination, PRs, and CI/CD.

**Sources:** [GitHub Docs — Using Git](https://docs.github.com/en/get-started/using-git)

</details>

---

<a name="workflow"></a>

### Git internals & DAG

**Definition:**

Git stores project history as a **directed acyclic graph (DAG)** of immutable **commit** objects. Each commit points to a **tree** (directory snapshot) and one or more parent commits. Objects are content-addressed by **SHA-1** hashes and live in `.git/objects/`.

| Object | Contains |
|--------|----------|
| **Blob** | File content |
| **Tree** | Directory listing (names → blob/tree hashes) |
| **Commit** | Tree hash, parent(s), author, committer, message |
| **Tag** | Pointer to commit + optional GPG signature |

| Tree | Location | Role |
|------|----------|------|
| **Working directory** | Project files on disk | Where you edit |
| **Index (staging area)** | `.git/index` | What goes into the next commit |
| **Repository** | `.git/` | Commits, branches, tags, full history |

**When to use:** Understanding internals helps with recovery (`reflog`), conflict resolution, and knowing why `rebase` rewrites history. Object IDs are **cryptographic hashes of content** (collision-resistant addressing). That is integrity, not password hashing — [Encryption](./7_special_security.md#encryption).

**Examples:**

```bash
git cat-file -t HEAD          # type: commit
git cat-file -p HEAD          # pretty-print commit object
git log --graph --oneline --all
```

**Sources:** [Pro Git — Git Internals](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain) · [git-scm.com](https://git-scm.com/)

### Branching & merge

**Definition:**

A **branch** is a movable pointer to a commit. **Merge** combines branch histories (optionally creating a merge commit); **rebase** replays commits onto another base for a linear history.

| | `git merge` | `git rebase` |
|---|-------------|--------------|
| History | Preserves branch topology | Linear replay of commits |
| Merge commit | Yes (unless fast-forward) | No |
| Rewrites SHAs | No | Yes |
| Safe on shared branches | Yes | **No** — rewrites history |

**When to use:**

Branch for every feature/fix. Merge into shared branches after review. Rebase **local/private** branches before opening a PR.

Never rebase commits others have already pulled. Never `reset --hard` on shared branches.

**Examples:**

```bash
git switch -c feature/login
# ... work, commit ...
git fetch origin
git rebase origin/main          # update feature branch (local only)
git switch main && git merge feature/login
```

**Sources:**

[Pro Git — Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) · [Pro Git — Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

---

## Workflow

<details>
  <summary>Git CLI daily workflow</summary>

### Git CLI daily workflow

**Definition:** Git CLI is the terminal interface to Git — the portable tool on servers and CI where there is no GUI. GUIs (GitHub Desktop, GitKraken, Sourcetree, Fork, Tower, Sublime Merge) are optional; the same commands run everywhere.

**How it works:**

Teams share a **remote** repository (hosted copy, not “deleted”). Each developer clones it, works on a **branch**, stages, commits, pushes for review, then merges and deletes the branch.

Typical loop:

1. **Clone** once, then **pull** later to integrate remote updates.
2. **Branch** from current HEAD so parallel work collides less.
3. **Edit** files; **status** to see untracked / modified / staged.
4. **`git add`** copies working-tree changes into the index (new files are untracked until added).
5. **`git commit`** freezes the index into history.
6. **`git push`** publishes the branch; first push of a new branch needs an upstream (`-u`).
7. After review, **merge** into the integration branch, then **delete** the feature branch.

A **conflict** is when two people changed the same lines — Git cannot pick a winner automatically.

Child branches keep the history of the parent **at the moment they were created**. Later parent commits are not inherited until you merge (or rebase) them in; child commits stay off the parent until you merge the other way.

**When to use:**

Daily feature work; SSH sessions and CI; teaching a shared command vocabulary.

#### Common pitfalls

* `git pull --all` fetches **all remotes**; it still only **integrates into the current branch** (merge or rebase of that branch’s upstream). It does not update every local branch.
* `git add .` is relative to the **current directory**. `git add --all` / `-A` updates the index for the **whole working tree** (additions, modifications, deletions).
* Bare `git push` needs a configured upstream (`push.default` is `simple` since Git 2.0). Git prints `git push -u origin <branch>` when missing.
* `git switch` exists since **Git 2.23**; `checkout` still works but also does restore/detach work. Prefer `switch` / `restore`.
* On case-insensitive disks (`core.ignorecase`, typical macOS/Windows), renaming `Git-CLI` → `git-cli` may look like a no-op. Use `git mv`:

```bash
git mv Git-CLI tmp && git mv tmp git-cli
```

**Pluses and minuses:**

| + | − |
|------|------|
| Same commands on every OS and server | Easy to run destructive flags (`reset --hard`) |
| Scriptable, works without a GUI | New-branch `push` fails until upstream is set |
| Precise staging (`add -p`) | Case-only renames can be invisible on macOS/Windows |

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

**Thoughts:** Clone → branch → stage → commit → push → merge → delete branch. GUI is comfort; CLI is the contract.

**Sources:** [Pro Git — Recording Changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) · [git-switch](https://git-scm.com/docs/git-switch) · [git-add](https://git-scm.com/docs/git-add) · [git-push](https://git-scm.com/docs/git-push) · [git-pull](https://git-scm.com/docs/git-pull)

</details>

<details>
  <summary>Initial configuration</summary>

### Initial configuration

**Definition:** One-time setup for identity, default branch, pull strategy, and line endings.

**When to use:** On first Git install on a new machine.

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

**Sources:** [Pro Git — First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

</details>

<details>
  <summary>Vim in Git output</summary>

### Vim in Git output

**Definition:**

Long output (`git log -p`, interactive rebase) opens Vim as the default pager/editor.

| Key / command | Action |
|---------------|--------|
| `Esc` | Command mode |
| `i` | Insert mode |
| `:q` | Quit |
| `:q!` | Quit without saving |
| `:wq` | Save and quit |

**When to use:** Whenever Git opens an editor for commit messages or interactive rebase.

**Sources:** [Pro Git — Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

</details>

<details>
  <summary>Commit conventions</summary>

### Commit conventions

**Definition:**

Structured commit messages make history searchable and enable automated changelogs. Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

| Prefix | When to use | Example |
|--------|-------------|---------|
| `init:` | Start project/sprint/task | `init: start sprint-1` |
| `feat:` | New functionality | `feat: implement search box` |
| `fix:` | Bug fix | `fix: relayout header for firefox` |
| `refactor:` | No behavior change | `refactor: rename vars for readability` |
| `docs:` | Documentation only | `docs: update readme` |
| `chore:` | Tooling, deps | `chore: bump eslint` |
| `test:` | Tests only | `test: add login unit tests` |
| `ci:` | CI/CD config | `ci: add deploy workflow` |

**When to use:** Every commit on team projects. Use imperative mood: `add feature` not `added feature`.

**Examples:**

```bash
git commit -m "feat!: remove legacy auth endpoint"
# Breaking change: append ! or add BREAKING CHANGE: in footer
```

**Sources:** [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) · [Commit style guide (Eric VonB)](https://gist.github.com/ericavonb/3c79e5035567c8ef3267)

</details>

<details>
  <summary>Create repository & clone</summary>

### Create repository & clone

**Definition:** `git init` creates a new local repository; `git clone` copies an existing remote repository including full history.

**When to use:** `init` for new projects; `clone` when joining an existing project or setting up CI. Daily loop: [Git CLI daily workflow](#workflow).

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

**Sources:** [Pro Git — Getting a Git Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)

</details>

<details>
  <summary>Inspect changes</summary>

### Inspect changes

**Definition:** `git status` and `git diff` show what changed and where — working directory, index, or between branches.

**When to use:** Before every commit and when reviewing PRs locally.

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

**Sources:** [Pro Git — Viewing Your Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

</details>

<details>
  <summary>Stage changes</summary>

### Stage changes

**Definition:** `git add` copies changes from the working directory into the index (staging area). New files stay **untracked** until added. Ignored paths are skipped unless `-f`.

**When to use:** Before every commit. Use `-p` for selective hunk staging.

**Examples:**

```bash
git add path/to/file
git add .                         # this directory and below
git add --all                     # whole working tree: add, modify, delete (-A)
git add -u                        # tracked files only (no new untracked)
git add -p                        # interactive hunk staging
git add -i                        # interactive shell
```

**Sources:** [Pro Git — Recording Changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

<details>
  <summary>Unstage & discard</summary>

### Unstage & discard

**Definition:** Unstage with `restore --staged` or `reset`; discard working changes with `restore`. `reset --hard` and `clean` permanently destroy uncommitted work.

**How it works:** `git reset --hard` permanently discards all uncommitted changes in tracked files. `git clean -df` removes untracked files — also irreversible.

**When to use:** Unstage when you added too much; discard when you want to throw away local edits. **Never** use `--hard` unless you are certain.

**Examples:**

```bash
git restore --staged file         # unstage (modern)
git reset file                    # unstage (classic)
git restore file                  # discard working changes (modern)
git checkout -- file              # discard working changes (classic)
git clean -df                     # remove untracked files/dirs
git reset --hard                  # DANGER: discard all uncommitted changes
```

**Sources:** [Pro Git — Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Commit</summary>

### Commit

**Definition:** `git commit` freezes the index into a new immutable commit object in the repository.

**When to use:** When a logical unit of work is complete and staged. Amend only unpushed commits. Bare `git commit` opens the configured editor; `-m` is for a short one-line message.

**Examples:**

```bash
git commit                        # editor for the message
git commit -m "feat: add login form"
git commit -a -m "fix: typo"      # stage tracked files only, then commit
git commit --amend -m "new message"   # rewrite last commit (unpushed only!)
git commit --signoff              # DCO sign-off line
```

**Sources:** [Pro Git — Recording Changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

<details>
  <summary>Stash</summary>

### Stash

**Definition:** `git stash` temporarily shelves uncommitted changes so you can switch branches without committing WIP.

**When to use:** Urgent context switch, pull with dirty working tree, or saving experiments.

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

**Sources:** [Pro Git — Stashing and Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

</details>

<details>
  <summary>History & blame</summary>

### History & blame

**Definition:** `git log`, `git show`, and `git blame` inspect commit history and line-by-line authorship.

**When to use:** Debugging regressions, understanding code ownership, preparing release notes.

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

Press `q` to exit long pager output.

**Sources:** [Pro Git — Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

</details>

<details>
  <summary>Remove & move files</summary>

### Remove & move files

**Definition:** `git rm` stops tracking and deletes files; `git mv` renames with history preserved. `--cached` removes from index only.

**When to use:**

Deleting features, renaming modules, untracking secrets accidentally added.

#### Common pitfalls

A case-only rename in Finder/Explorer often produces an empty commit. Two-step `git mv` (or `git mv -f Old old` on some setups) records the change.

**Examples:**

```bash
git rm file.txt
git rm -r folder/
git rm --cached secret.env        # stop tracking; keep file on disk
git mv old.txt new.txt

# Case-only rename on case-insensitive FS (macOS/Windows, core.ignorecase)
git mv Git-CLI tmp && git mv tmp git-cli
```

**Sources:** [Pro Git — Recording Changes](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

</details>

---

<a name="branching"></a>
