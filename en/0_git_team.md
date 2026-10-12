# Git — Team

Recovery, team practices, hooks, and advanced Git.

[← Back to hub](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Branching](./0_git_branching.md) · [Git — Team](./0_git_team.md) · [Git — Interview](./0_git_interview.md)

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

`git reset` moves the branch pointer and optionally resets index/working tree. `git revert` creates a new commit that undoes a previous one — safe for shared history.

| Mode | Branch | Index | Working tree |
|------|--------|-------|--------------|
| `--soft` | moves back | unchanged | unchanged |
| `--mixed` (default) | moves back | reset | unchanged |
| `--hard` | moves back | reset | reset |

**How it works:** `git reset --hard` permanently destroys uncommitted changes and moves the branch pointer. On shared branches, use `revert` instead.

**When to use:** `reset` for unpushed local commits only. `revert` for pushed/shared commits.

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

**Sources:** [Pro Git — Reset Demystified](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified) · [Pro Git — Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Revert vs reset</summary>

### Revert vs reset

**Definition:**

Two ways to undo history — revert is non-destructive; reset rewrites branch position.

| | `revert` | `reset` |
|---|----------|---------|
| Creates new commit | Yes | No |
| Safe on pushed history | Yes | No (rewrites) |
| Use when | Public/shared branch | Local-only commits |

**When to use:** Always prefer `revert` on `main` or any branch teammates use.

**Sources:** [Pro Git — Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Restore file from history</summary>

### Restore file from history

**Definition:** Recover a file's content from a specific commit without checking out the entire commit.

**When to use:** Accidental deletion, need to restore one file from a known good commit.

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

**Definition:** Reflog records where HEAD and branch tips have been. It is your safety net for recovering "lost" commits and deleted branches.

**When to use:** After accidental `reset --hard`, deleted branch, or failed rebase.

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

Quick reference for common disaster scenarios.

| Situation | Command |
|-----------|---------|
| Undo last commit, keep changes | `git reset --soft HEAD~1` |
| Undo last commit, discard changes | `git reset --hard HEAD~1` |
| Undo pushed commit safely | `git revert HEAD` |
| Recover deleted branch | `git reflog` → `git branch recovered abc123` |
| Abort bad merge | `git merge --abort` |
| Abort bad rebase | `git rebase --abort` |
| Undo bad merge on main | `git branch -f main ORIG_HEAD` |
| Restore file from old commit | `git restore --source=abc123 -- file` |

**When to use:** Bookmark this table — you will need it at 5 PM on a Friday.

**Sources:** [Pro Git — Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

</details>

<details>
  <summary>Merge conflicts</summary>

### Merge conflicts

**Definition:** Conflicts occur when Git cannot auto-merge changes to the same lines. You must manually resolve markers (`<<<<<<<`, `=======`, `>>>>>>>`) and stage the result.

**When to use:** During `merge`, `rebase`, or `cherry-pick` when both sides edited the same region.

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

**Sources:** [Pro Git — Basic Merge Conflicts](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

</details>

<details>
  <summary>Bisect</summary>

### Bisect

**Definition:** `git bisect` performs a binary search through commit history to find the commit that introduced a bug.

**When to use:** Regression with unknown cause — "it worked last week."

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

**Sources:** [Pro Git — Debugging with Git](https://git-scm.com/book/en/v2/Git-Tools-Debugging-with-Git)

</details>

<details>
  <summary>Worktrees</summary>

### Worktrees

**Definition:** `git worktree` lets you check out multiple branches in separate directories from the same repository.

**When to use:** Hotfix while feature work is in progress — no stash/switch needed.

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

**Definition:** Pull Requests (GitHub) / Merge Requests (GitLab) are the standard workflow for proposing, reviewing, and merging code changes.

**When to use:** Every change to shared branches (`main`, `develop`).

**Examples:**

1. **Create branch** from `main` / `develop`
2. **Commit** atomically with clear messages
3. **Push** and open Pull Request (PR) / Merge Request (MR)
4. **Review** — teammates comment, request changes
5. **CI** — tests, lint, build run on branch
6. **Merge** into target branch after approval
7. **Delete** feature branch

![git flow](https://github.com/training-018/git-bash/blob/main/git_flow.jpeg)

PRs enable discussion before code lands on shared branches. Push additional commits during review; they appear in the PR automatically.

**Sources:** [GitHub — About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)

</details>

<details>
  <summary>Git hygiene</summary>

### Git hygiene

**Definition:** Team conventions that keep repository history clean, secure, and reviewable.

**When to use:**

Always on team projects.

| Practice | Why |
|----------|-----|
| Atomic commits | One logical change per commit — easy to revert |
| Conventional commits | Searchable history, auto-changelog |
| Branch naming | `feature/`, `fix/`, `chore/` prefixes |
| Small PRs | Easier review, faster merge (< 400 lines ideal) |
| Rebase before PR | Linear, reviewable history |
| Never force-push shared branches | Prevents overwriting teammates' work |
| No secrets in repo | Use `.gitignore`, secret scanning |
| Signed commits | Verify authorship on sensitive repos |
| Delete merged branches | Reduce clutter locally and remotely |

**Sources:** [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

</details>

<details>
  <summary>Practical scenarios</summary>

### Practical scenarios

**Definition:**

End-to-end recipes for common real-world Git situations.

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

**Sources:** [Pro Git book](https://git-scm.com/book/en/v2)

</details>

<details>
  <summary>Quick fixes</summary>

### Quick fixes

**Definition:**

Short solutions for recurring Git annoyances.

#### Pull with merge instead of rebase

```bash
git fetch origin develop
git merge origin/develop
```

Avoid `git pull --force` — use `fetch` + `merge` or `pull --rebase` intentionally.

#### Revert merge commit

```bash
git revert -m 1 cc831fe
```

`-m 1` keeps the first parent (usually mainline).

#### GitHub asks for password on every push

Use SSH keys or credential helper: [GitHub credential caching](https://docs.github.com/en/get-started/git-basics/caching-your-github-credentials-in-git).

**Sources:** [GitHub Docs](https://docs.github.com/en/get-started/using-git)

</details>

<details>
  <summary>Signed commits</summary>

### Signed commits

**Definition:** GPG or SSH signing cryptographically verifies that a commit was made by you.

**When to use:** Production repos, open-source projects requiring verified commits, protected branch rules.

**Examples:**

```bash
git config --global user.signingkey KEY_ID
git config --global commit.gpgsign true
git commit -S -m "feat: secure change"
```

Verify on merge in protected branch rules. SSH signing also supported in Git 2.34+.

**Sources:** [GitHub — Signing commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)

</details>

---

<a name="hooks-tooling"></a>

## Hooks & tooling

<details>
  <summary>`.gitignore`</summary>

### `.gitignore`

**Definition:** Patterns that tell Git which files to never track.

**When to use:** From project day one — before first commit.

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

* Typical ignores: installable deps (`node_modules`), build output, IDE folders (`.idea`), secrets (`.env`, keys)
* One pattern per line; `#` for comments
* `!` negates: `*.log` then `!important.log`
* Global ignore: `git config --global core.excludesfile ~/.gitignore_global`
* Already tracked file: `git rm --cached file` then add to `.gitignore`

**Sources:** [git-scm.com — gitignore](https://git-scm.com/docs/gitignore)

</details>

<details>
  <summary>Git GUIs vs CLI</summary>

### Git GUIs vs CLI

**Definition:** Desktop clients visualize history, staging, and branches. They wrap the same Git operations as the CLI.

**When to use:** Local day-to-day work. Use CLI on servers, CI, and when teaching a shared command set. Daily command loop: [Git CLI daily workflow](./0_git_core.md#workflow).

**Examples:** GitHub Desktop, Sourcetree, GitKraken, Fork, Tower, Sublime Merge (licensing varies). VS Code Source Control and GitLens are in-editor GUIs.

**Thoughts:** Learn CLI; pick a GUI for comfort, not as a substitute on headless machines.

</details>

<details>
  <summary>`.gitattributes`</summary>

### `.gitattributes`

**Definition:** Per-path rules for line endings, diff drivers, and merge behavior.

**When to use:** Team projects with mixed OS environments.

**Examples:**

```
* text=auto eol=lf

*.html diff=html
*.css  diff=css
*.scss diff=css
*.png  binary
```

Renormalize after adding rules:

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

Windows uses CRLF (`\r\n`); Unix uses LF (`\n`). Mismatched line endings cause merge conflicts on every pull.

| OS | Default | Git setting |
|----|---------|-------------|
| Windows | CRLF | `core.autocrlf=true` |
| macOS/Linux | LF | `core.autocrlf=input` |

**When to use:** Fix team-wide before conflicts multiply (see interview Q1).

**Examples:**

```bash
# .gitattributes: * text=auto eol=lf
git add --renormalize .
git commit -m "chore: normalize line endings to LF"
```

**Sources:** [Pro Git — Customizing Git — Git Attributes](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes)

</details>

<details>
  <summary>Git hooks</summary>

### Git hooks

**Definition:**

Scripts that run automatically at Git lifecycle events. Live in `.git/hooks/` or via tools like Husky.

| Hook | When | Typical use |
|------|------|-------------|
| `pre-commit` | Before commit | lint, format, typecheck |
| `commit-msg` | After message entered | enforce conventional commits |
| `pre-push` | Before push | run tests |

**When to use:** Enforce code quality gates before changes reach remote.

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

**Definition:** Embed one Git repository inside another at a fixed commit.

**When to use:** Vendoring dependencies or monorepo-adjacent shared libraries. Prefer package managers when possible.

**Examples:**

```bash
git submodule add URL path
git submodule update --init --recursive
git clone --recurse-submodules URL
```

**Sources:** [Pro Git — Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

</details>

---

<a name="advanced"></a>

## Advanced topics

<details>
  <summary>Misc commands</summary>

### Misc commands

**Definition:** Utility commands for archiving, author stats, and repository integrity checks.

**When to use:** Release packaging, contributor reports, debugging corrupt repos.

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

**Definition:** A single repository containing multiple packages or applications.

**When to use:**

Shared codebases with coordinated releases. Use path filters in CI to avoid building everything.

| Strategy | Tool / pattern |
|----------|----------------|
| Trunk-based + path filters | `paths:` in [GitHub Actions](./10_build_ci.md#github-actions) |
| Sparse checkout | Clone only needed folders |
| Build orchestration | Nx, Turborepo, Lerna |

**Sources:** [Nx](https://nx.dev/) · [Turborepo](https://turbo.build/)

</details>

<details>
  <summary>Alternative VCS</summary>

### Alternative VCS

**Definition:**

Other version control systems with different trade-offs.

| System | Model | Trade-off |
|--------|-------|-----------|
| **SVN** | Centralized | Simple; no offline history; branching weaker |
| **Mercurial** | Distributed | Similar to Git; smaller ecosystem |
| **Perforce** | Centralized + large binaries | Game/film industry; complex |
| **Git** | Distributed | Industry standard; steep learning curve |

**When to use:** Git for web/frontend. SVN/Perforce in legacy enterprise or game studios.

**Sources:** [Atlassian — Git glossary](https://www.atlassian.com/git/glossary)

</details>

<details>
  <summary>CI/CD integration</summary>

### CI/CD integration

**Definition:** Git events (push, PR, tag) trigger automated build, test, and deploy pipelines. Concept: [10_build.md — What is CI/CD](./10_build_interview.md#what-is-cicd). Workflow YAML: [10_build.md — YAML](./10_build_ci.md#yaml).

**When to use:** Every team project — run lint/test on every PR; deploy tags to production.

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

* Cache `node_modules` keyed on lockfile hash
* Run lint/test on every PR
* Deploy `main` to staging; tags to production

**Sources:** [GitHub Actions — checkout](https://github.com/actions/checkout)

</details>

---

<a name="interview-practice"></a>
