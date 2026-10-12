# Git — Branching

Branches, merges, remotes, and collaboration.

[← Back to hub](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Branching](./0_git_branching.md) · [Git — Team](./0_git_team.md) · [Git — Interview](./0_git_interview.md)

### Sections

- [Branching & merge](#branching-merge)
- [Remote](#remote)

---

## Branching & merge

<details>
  <summary>Branches</summary>

### Branches

**Definition:** A branch is a movable pointer to a commit. Creating branches is cheap — they are just refs, not file copies.

**When to use:** One branch per feature, fix, or experiment. Delete after merge.

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

**Sources:** [Pro Git — Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)

</details>

<details>
  <summary>Switch commits & branches</summary>

### Switch commits & branches

**Definition:** `git switch` (Git **2.23+**) or `checkout` (classic) moves HEAD to a branch or specific commit. `checkout` also restores files and detaches HEAD; `switch` is the dedicated branch command.

**When to use:** Daily branch switching. Use `--detach` only for temporary inspection.

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

`git merge` integrates another branch into the current branch. Creates a merge commit unless fast-forward is possible.

| Strategy | Result | When |
|----------|--------|------|
| **Fast-forward** | Branch pointer moves; no merge commit | Linear history, no divergent commits |
| **3-way merge** | New merge commit with two parents | Both branches have unique commits |
| **`--no-ff`** | Always create merge commit | Preserve branch context in history |
| **Squash merge** | All branch commits → one commit on target | Clean history; lose individual commits |

**When to use:** Integrating completed features into the current branch after review. Same command both ways: merge `main` **into** a feature branch to pick up new parent commits, or merge the feature **into** `main` to land the work. `git merge <name>` always merges **that** branch into **HEAD**.

**Examples:**

```bash
git merge feature                   # merge named branch into current
git merge main                      # update feature branch with parent
git merge feature --no-ff           # always create merge commit
git merge feature --squash          # squash into one commit (manual commit after)
git merge --abort
```

**Sources:** [Pro Git — Basic Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

</details>

<details>
  <summary>Rebase</summary>

### Rebase

**Definition:** `git rebase` replays commits from one branch onto another base, producing a linear history with new commit SHAs.

**How it works:** Rebase **rewrites history**. Force-pushing a rebased branch overwrites remote commits teammates may have based work on. Golden rule: never rebase commits that others have pulled.

**When to use:**

Update a **local/private** feature branch onto latest `main` before opening a PR. Clean up WIP commits with interactive rebase.

On any branch others have pulled. Never rebase `main` or shared release branches.

**Examples:**

```bash
git rebase main
git rebase -i HEAD~3                # interactive: squash, reword, drop
git rebase --onto main feature old-base
git rebase --abort
git rebase --continue
git pull --rebase origin main
```

Interactive rebase actions: `pick`, `reword`, `squash`, `fixup`, `drop`.

**Sources:** [Pro Git — Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

</details>

<details>
  <summary>Merge vs rebase</summary>

### Merge vs rebase

**Definition:**

Two strategies for integrating branch histories — merge preserves topology; rebase linearizes.

| | `git merge` | `git rebase` |
|---|-------------|--------------|
| History | Preserves branch topology | Linear replay of commits |
| Merge commit | Yes (unless FF) | No |
| Conflicts | Once at merge time | Per replayed commit |
| Safe on shared branches | Yes | **No** — rewrites history |
| Best for | Integrating completed features | Cleaning local branch before PR |

**When to use:** Merge for shared branches; rebase for private feature branches before PR.

**Examples:**

```bash
# Update feature branch with latest main (rebase — clean history)
git switch feature/foo
git fetch origin
git rebase origin/main

# Update feature branch with latest main (merge — safe, preserves history)
git merge origin/main
```

**Sources:** [Pro Git — Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

</details>

<details>
  <summary>Cherry-pick</summary>

### Cherry-pick

**Definition:** `git cherry-pick` applies a specific commit from another branch onto the current branch.

**When to use:** Backporting a fix, bringing one commit without merging entire branch.

**Examples:**

```bash
git cherry-pick abc123
git cherry-pick main~2..main
git cherry-pick -n abc123         # stage without committing
git cherry-pick --abort
git cherry-pick --continue
```

**Sources:** [Pro Git — Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

</details>

<details>
  <summary>Tags</summary>

### Tags

**Definition:** Tags are fixed pointers to commits, typically marking releases. Annotated tags store metadata and are recommended for releases.

**When to use:** Release versioning (`v1.0.0`). Lightweight tags for temporary bookmarks.

**Examples:**

```bash
git tag v1.0.0                      # lightweight
git tag -a -m "Release" v1.0.1      # annotated (recommended for releases)
git tag -d v1.0.0
git push origin v1.0.1
git push origin --tags
```

**Sources:** [Pro Git — Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

</details>

<details>
  <summary>Git Flow</summary>

### Git Flow

**Definition:**

A branching model with long-running `main` and `develop` branches plus short-lived feature, release, and hotfix branches.

| Branch | Purpose |
|--------|---------|
| `main` | Production-ready |
| `develop` | Integration branch |
| `feature/*` | New features off develop |
| `release/*` | Release prep off develop |
| `hotfix/*` | Urgent fixes off main |

**When to use:** Scheduled releases with version numbers. Heavy for continuous deployment teams.

**Sources:** [Atlassian — Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

</details>

<details>
  <summary>GitHub Flow & trunk-based</summary>

### GitHub Flow & trunk-based

**Definition:**

Simpler models optimized for continuous integration and deployment.

| Model | Pattern |
|-------|---------|
| **GitHub Flow** | `main` + short-lived feature branches → PR → merge → deploy |
| **Trunk-based** | Everyone commits to `main` (or merges daily); feature flags hide incomplete work |

**When to use:** Teams deploying multiple times per day. Prefer trunk-based over Git Flow for CI/CD pipelines. How this sits in the delivery loop: [10_build.md — What is CI/CD](./10_build_interview.md#what-is-cicd).

**Sources:** [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow) · [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com/)

</details>

<details>
  <summary>Interactive rebase & squash</summary>

### Interactive rebase & squash

**Definition:** `git rebase -i` lets you reorder, squash, reword, or drop commits before merging.

**When to use:** Clean up WIP commits (`wip`, `fix typo`) into one logical commit before opening a PR. Only on unpushed or private history.

**Examples:**

```bash
git rebase -i origin/main
```

In the editor:

```
pick abc1234 feat: add form validation
squash def5678 wip
squash ghi9012 fix typo
reword jkl3456 feat: add login form
```

| Action | Effect |
|--------|--------|
| `squash` | Merge into previous commit; combine messages |
| `fixup` | Merge into previous; discard this message |
| `pick` | Keep as-is |
| `drop` | Remove commit |

**Sources:** [Pro Git — Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

</details>

---

<a name="remote"></a>

## Remote

<details>
  <summary>Remote repositories</summary>

### Remote repositories

**Definition:** Remotes are named references to other repositories. `origin` is the default remote for your fork/clone; `upstream` is commonly used for the original repo when working from a fork.

**When to use:** Daily push/pull. Prefer SSH after key setup ([SSH setup](#ssh-setup); protocol: [DevOps — SSH](./15_devops.md#ssh)).

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

**Sources:** [Pro Git — Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

</details>

<details>
  <summary>Fetch vs pull vs push</summary>

### Fetch vs pull vs push

**Definition:**

`fetch` downloads remote changes without merging; `pull` fetches and integrates; `push` sends local commits to remote.

| Command | Fetch remote? | Update local branch? | Merge/rebase? |
|---------|---------------|----------------------|---------------|
| `git fetch` | Yes | No | No |
| `git pull` | Yes | Yes | Yes (merge or rebase) |
| `git push` | No | No | Sends local commits to remote |

**When to use:** `fetch` when you want to inspect remote changes before integrating: `git log main..origin/main`. `git pull --all` fetches every remote, then still only integrates the **current** branch.

**Examples:**

```bash
git fetch origin
git fetch --all --prune
git pull --all                    # fetch all remotes, then merge/rebase current upstream
git log main..origin/main         # what's new on remote
git merge origin/main             # integrate after review
```

**Sources:** [Pro Git — Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

</details>

<details>
  <summary>Sync fork with upstream</summary>

### Sync fork with upstream

**Definition:** Keep your fork current with the original repository by fetching from `upstream` and merging into your `main`.

**When to use:** Before starting new work on a forked open-source project.

**Examples:**

```bash
git remote add upstream git@github.com:original/repo.git
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

**Sources:** [GitHub — Syncing a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)

</details>

<details>
  <summary>Force push</summary>

### Force push

**Definition:** Overwrites remote branch history with your local branch. `--force-with-lease` is safer than `--force` because it fails if the remote has commits you haven't seen.

**When to use:** After rebasing your own feature branch before PR update. Never on shared branches.

**Examples:**

```bash
git push --force-with-lease origin feature/foo
```

Enable **protected branches** on `main` to block force pushes entirely.

**Sources:** [git-scm.com — git-push](https://git-scm.com/docs/git-push)

</details>

---

<a name="recovery"></a>
