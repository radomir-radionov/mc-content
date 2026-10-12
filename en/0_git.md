# Git — hub

Version control, history, collaboration, and recovery. Theory lives in the topic files below.

### Related docs

[Git — Core](./0_git_core.md) · [Git — Branching](./0_git_branching.md) · [Git — Team](./0_git_team.md) · [Git — Interview](./0_git_interview.md)

### Navigation

| Topic                                   | Covers                                             |
| --------------------------------------- | -------------------------------------------------- |
| [Git — Core](./0_git_core.md)           | Objects, commits, and everyday workflow.           |
| [Git — Branching](./0_git_branching.md) | Branches, merges, remotes, and collaboration.      |
| [Git — Team](./0_git_team.md)           | Recovery, team practices, hooks, and advanced Git. |
| [Git — Interview](./0_git_interview.md) | Git interview questions, answers not filled in.    |

### Interview practice

Q&A lives in the Interview topic for this subject.

---

## Resources & links

<details>
  <summary>Primary references</summary>

- [Git official site](https://git-scm.com/)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [GitHub Docs — Using Git](https://docs.github.com/en/get-started/using-git)
- [Atlassian Git tutorials](https://www.atlassian.com/git/tutorials/what-is-version-control)
- [Atlassian Git glossary](https://www.atlassian.com/git/glossary)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Essential Git cheatsheet (ByteByteGo)](https://blog.bytebytego.com/i/162154465/essential-git-cheatsheet)
- [Commit style guide (Eric VonB)](https://gist.github.com/ericavonb/3c79e5035567c8ef3267)

</details>

<details>
  <summary>Videos & courses</summary>

### Videos & courses

- [Git course playlist](https://www.youtube.com/playlist?list=PLDyvV36pndZFHXjXuwA_NywNrVQO0aQqb)
- [Basic Git commands](https://www.youtube.com/watch?v=EsA-9E0umCo)
- [GitHub Pages deploy](https://www.youtube.com/watch?v=dLX40HYzETY)

</details>

<details>
  <summary>SSH setup</summary>

### SSH setup

Protocol, `authorized_keys`, `sshd_config`, and server hardening: [DevOps — SSH](./15_devops.md#ssh). This block is GitHub/Git remotes only.

- [Git — Generating SSH keys](https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key)
- [GitHub — Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [GitHub — Adding a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

# macOS / Windows / Linux clipboard
pbcopy < ~/.ssh/id_ed25519.pub
clip < ~/.ssh/id_ed25519.pub
xclip -selection clipboard < ~/.ssh/id_ed25519.pub

# or: gh ssh-key add ~/.ssh/id_ed25519.pub --title "laptop"
```

GitHub UI: **Settings → SSH and GPG keys → New SSH key** — paste the **`.pub`** file only. Remote URL: `git@github.com:user/repo.git`.

</details>

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Core](./0_git_core.md#core)
- [Workflow](./0_git_core.md#workflow) — [Git CLI daily loop](./0_git_core.md#workflow)
- [Branching & merge](./0_git_core.md#branching)
- [Remote](./0_git_branching.md#remote)
- [Recovery](./0_git_team.md#recovery)
- [Team practices](./0_git_team.md#team)
- [Hooks & tooling](./0_git_team.md#hooks-tooling)
- [Advanced topics](./0_git_team.md#advanced)
- [Interview Practice](./0_git_interview.md#interview-practice)
