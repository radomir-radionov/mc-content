# Git — Interview

Answers are filled for every question in this bank.

[← Back to hub](./0_git.md)

### Related docs

* [Git — Core](./0_git_core.md) · [Git — Branching](./0_git_branching.md) · [Git — Team](./0_git_team.md) · [Git — Interview](./0_git_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/).

### Git Fundamentals

<details>
<summary>What is Git and why is it used?</summary>

### What is Git and why is it used?

**Definition:**

Git is a decentralized version control system (DVCS) that developers primarily use to manage source code across diverse development projects. It ensures better code quality, facilitates teamwork, and provides a robust backup mechanism.

**How it works:**

Commit. A commit represents a saved state of your project. It is a snapshot of all the files in your project at a given point in time. Each commit has a commit message associated with it.

Branch. A branch is an independent line of development. It allows you to work on a feature or bug fix, isolate it from the rest of the codebase until it is ready, and then merge it back in.

Master is a default branch present in all Git repositories. You can create many other branches.

Merge. Merging in Git means taking the independent developments of two branches and combining them into a single branch.

After merging, the two branches from which you were merging are the same as the merged branch.

Remote and Origin. A remote is a common repository on a server or accessible memory that all team members use to exchange their changes.

An origin is a source of commit objects (store of all committed versions).

The Command Line vs Visual Tools. For everyday tasks, both CLI and GUI are powerful, but it's essential to understand Git's underlying mechanics so that you can troubleshoot and manage complex tasks efficiently.

**Pluses and minuses:**

- History and Time Travel: Git preserves every version of a project, making it possible to roll back changes or inspect code at any point.
- Security and Integrity: Each file in Git has a unique SHA-1 hash, which means unauthorized changes are easily detected.
- Collaboration: Git allows multiple developers to work on a project simultaneously without conflict.
- Code Reviews: Git simplifies the process of reviewing code before it's incorporated into the main codebase.

**Thoughts:**

- Make frequent commits.
- Write clear and concise commit messages.
- Push your code to a remote repository often, especially before taking a break or finishing a task.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>How does Git differ from other version control systems (VCS)?</summary>

### How does Git differ from other version control systems (VCS)?

**Definition:**

Git offers numerous advantages over traditional Version Control Systems (VCS), providing a distributed and efficient version control model.

**How it works:**

- Distributed Model: Git decentralized nature empowers every set of files with a complete repository history and enables full-fledged operations without network connectivity. This stands in contrast to centralized systems that require a constant network connection and utilize a single centralized server for all project data.
- Quick Operations: Git is renowned for its fast and efficient operations. Unlike older VCS, which often performed slowly or required substantial server interactions, Git uses local operations, resulting in impressive speed.
- Data Integrity: Git ensures the cryptographic integrity of every piece of data, securing against file corruption, loss, or unauthorized alterations. The absence of this feature in some traditional VCS can pose data integrity risks.
- Streamlined Branching: While segments or branches in older VCS were often used for parallel development, Git branches are lightweight and designed for quick and frequent usage.
- Customizable Workflows: Git permits versatile workflows due to its distributed nature and the ability to manipulate the commit history before sharing changes with others.
- Staging Area (Index): Before committing, developers use the "staging area" to selectively include specific changes in a commit rather than committing every altered file.
- Powerful Library of Core Commands: Git's robust set of indispensable commands enables a broad range of version control capabilities, from historical revision exploration to collaborative feature code management.
- Snapshot vs File Tracking: Git's snapshots record the current state of the complete repository, whereas some traditional VCS, such as CVS and Subversion, primarily track file changes.
- Atomic Commits: Every change is distinct and exclusive from others, enabling precise control over data synchronization.
- Modern Landscape Readiness: Git melodiously integrates with modern software development paradigms like Continuous Integration/Continuous Deployment (CI/CD).
- Cross-Platform Compatibility: Be it Windows, macOS, or Linux, Git operates identically.

**Pluses and minuses:**

- Risk of Single Point of Failure: If the central server is compromised, it poses a threat to the entire project's data.
- Version Numbering Limitations: Users might be confined to a monotonically increasing number for versions, restricting granularity.
- Long-Term Integrity Risks: Without cryptographic validation, files can be altered unnoticed.
- Data Loss Complications: If a central server/repository is compromised or damaged, data loss is severe.
- Limited Offline Functionality: Users are hindered by the necessity of a steady network connection.
- Stale Branches: Centralized repositories can have stale branches, hindering collaboration.

**Thoughts:**

- Simplicity and Flexibility: Unlike some traditional VCS, Git is designed to be both user-friendly and capable of supporting complex tasks.
- Efficiency and Performance: Git's rapid and streamlined processes lead to optimized efficiency.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What is the difference between Git and GitHub?</summary>

### What is the difference between Git and GitHub?

**Definition:**

Git and GitHub are often confused due to their similar names. However, they serve different version control and development hosting purposes.

Git is a distributed version control system that manages code and tracks its changes. It's mainly a local solution, only allowing developers to coordinate directly.

GitHub, on the other hand, is a web-based platform that provides hosting for Git repositories. It offers features that extend beyond Git's core functionality.

**How it works:**

- Local Management: Developers can work on their repositories without the need for a central server.
- Speed: Operations such as committing, branching, and merging are rapid, ideal for local development.
- Staging Area: Changes are first staged before getting committed for better control.
- Content Tracking: Rather than file-based, Git is content-based, ensuring efficient tracking.
- Remote Repositories: It allows developers to sync their local repositories with remote ones, enabling collaborative work.
- Code Hosting: Developers can store and manage their code from anywhere, not just their local machines.
- Collaboration Tools: GitHub provides tools for project management, issue tracking, and team collaboration.
- Code Review: Multiple users can review and discuss proposed changes before they're merged into the main codebase.
- Access Control: Different users can have varying levels of access to a repository or organization.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What is a repository in Git?</summary>

### What is a repository in Git?

**Definition:**

A Git repository is a location where all the version-controlled files and their history are stored. It serves as the core component for collaborative and version control workflows.

Every Git repository has two primary parts: the working tree and the hidden `.git` directory, where Git keeps all of its metadata for the repository.

**How it works:**

- Distributed: Each contributor to the project has their own copy of the repository, including its complete history and version-tracking capability. This enables offline work and serves as a backup for the project.
- Efficient Storage: Git optimizes storage using techniques such as file hashing and delta compression, emphasizing minimal redundancy and data redundancy elimination.
- Content Integrity: All files in a Git repository, as well as their versions, are checksummed and verified on every Git operation for data integrity.
- Work Tracking: Git allows for tracking changes made to the project over time, helping individuals or teams understand the evolution of the project, who made specific changes, and why.

The `.git` directory is the control center of a Git repository, housing everything Git needs to manage the repository and its history. It includes:

- The object database that hosts all the data about commits, trees, and blobs.
- The references directory maintaining pointers to specific commits (e.g., branches and tags).
- Configuration files detailing the repository's settings and attributes.

The working tree is a directory where tracked files are extracted for viewing and editing. Any changes made to files in the working tree can be staged and committed to the repository to capture the changes.

Untracked files, i.e., files not previously staged or committed, coexist with the working tree and reside outside the Git management.

**Examples:**

Code Example: Working Tree.

Here is the Python code:

```python
# Let's define a variable to point to the .git directory
git_directory = '.git'

# Now, we can simulate the construction of our working tree using a dictionary
working_tree = {
    'folder1': {
        'file1.py': 'print("Hello, World!")'
    },
    'folder2': {
        'file2.py': 'print(2 + 2)'
    }
}
```

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>Explain the concept of a commit in Git.</summary>

### Explain the concept of a commit in Git.

**Definition:**

In Git, a commit represents a snapshot of a project at a specific point in time. Each commit records the changes made, the author, and a unique identifier. It is organized in a directed acyclic graph (DAG), which aligns with its branching model.

**How it works:**

- Tree: Represents the state of the project's file system at the time of the commit. The tree object is a record of the location and content of each file in the repository.
- Parent Commit(s): Identifies the commit(s) from which the current commit originated. In the case of the initial commit, there's no parent.
- Metadata: Includes details such as the commit's author, date, and a message summarizing the changes made.

In a typical Git repository, there is a working directory, staging area, and commit history. Each tracks the project's state at various stages.

- Working Directory: Refers to the current, live set of files and directories. This is the version of the project that you actively work on and modify.
- Staging Area (also known as the "index"): Serves as a buffer zone. Files in this area are marked for inclusion in the next commit. You control what content moves from the working directory to the staging area.
- Commit History (in the Git directory): Represents the timeline of commits in the form of a versioned archive. It also houses the full content of the project at relevant commit points.

The standard git-flow follows a lifecycle that incorporates the three trees mentioned above.

- Creating Commits: You start by making changes to your working directory. When you're ready to record these changes, you add specific files from the working directory to the staging area. With the desired changes in the staging area, the next command captures the staging area's content and creates a new commit in the commit history.
- Browsing History: Git's commit history is time-ordered, typically from the most recent commit backward. Each commit influences how the project appears at a given point in time.

You can view the commit history using commands such as `git log`. This command provides a structured list of commits, detailing the author, commit date, and an associated commit message.

**When to use:**

- Code Reversion: You can reset a project to an earlier commit, effectively reversing changes.
- Collaboration: Commits facilitate collaboration by enabling team members to understand changes and when they were made.
- Code Review: Before incorporating changes into the primary branch, commits offer a structured way for team members to review each other's code. This process ensures that new code meets the repository's standards.

**Examples:**

Code Example: Commit Creation and Viewing.

Here is the Git console command to create "Meaingful Commit" with a message and then to view the Commit.

```bash
# To make a commit with a message
git commit -m "Meaningful Commit"

# To view the commit history
git log
```

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What is the difference between a working directory, staging area, and repository in Git?</summary>

### What is the difference between a working directory, staging area, and repository in Git?

**Definition:**

Git is characterized by a three-tiered architecture which includes the following layers:

- Working Directory
- Staging Area
- Repository

Each layer plays a distinct role in the Git architecture.

**How it works:**

The working directory is your playground. It's your space for experimenting, where you can edit files, merge different versions, and even create new ones.

The staging area (also known as the Index) is a kind of middle-ground. Here you can make fine adjustments to what changes will be included in your next commit. You have two types of files here:

- MDS: Files that have been modified since your last commit
- SML: Files that have been changed and prepared for your next commit

When you're happy with your changes in the staging area, you "stage" them. Staging in this context is like flagging certain parts of your project that you want to be saved in your next commit. Notice that this step is completely optional.

The repository is the final tier, dealing with the preservation of your work. Think of it as the local repository that serves as the safe house for all your commits. It keeps track of all changes and commits you've made, ensuring that you can still retrieve them even after major updates.

The repository is also broken down into three apparent "trees" - the Working Directory, the Staging Area, and the last one, the HEAD. The HEAD points to your most recent commit. These three trees can seem confusing at first. But really, they're straightforward to understand - they represent the current status of your project, changes that you're planning to commit, and the commits that you've confirmed you want to save in the future.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>Define branching in Git and its importance.</summary>

### Define branching in Git and its importance.

**Definition:**

Branching in Git allows for the independent development of distinct features or sections of the codebase. It's a crucial version control strategy, particularly in collaborative projects.

**How it works:**

- Master/Trunk/Branch: The primary branch serving as the project source.
- Feature Branches: Additional branches dedicated to specific features or tasks, often derived from the Master branch.
- Merging: The process of integrating changes from one branch into another. For instance, integrating finished feature branches into the Master.
- Conflict Resolution: The act of resolving overlapping changes made to the codebase across different branches.

**Pluses and minuses:**

1. Code Isolation: Each feature branch is an isolated environment for developing a specific feature. This avoids interference with the main codebase.
2. Collaborative Development: Allows multiple developers to work on the same codebase concurrently, minimizing conflicts.
3. Risk Mitigation: Changes are kept separate until they are thoroughly tested. If an experimental feature doesn't work as expected or introduces bugs, it won't affect the more stable Master branch.
4. Staging Area: A dedicated space for integration and testing before code is deployed or merged into the Master branch.
5. Feature Toggles: Streamlines the process of selectively deploying new features for testing.
6. Code Reviews: Assists in code quality checks before merging, ensuring the Master branch remains stable.
7. Task Management: Aligns with project management tools and methodologies, providing a way to structure tasks in a version-controllable manner.
8. Release Management: Facilitates the isolation and verification of changes earmarked for specific releases.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What is a HEAD in Git?</summary>

### What is a HEAD in Git?

**Definition:**

In Git, the concept of a working directory, the staging area (or index), and the repository (or commit history) are all managed by the `HEAD`.

**How it works:**

- Point of Reference: `HEAD` identifies the current snapshot/commit in the history tree. Any actions in the working directory or staging area are based on this commit.
- Connector between Levels: It links the most recent commit in a branch to both the staging area and the working directory. Each commit is precisely what `HEAD` points to.

**Examples:**

Code Example: Working with `HEAD`.

Here is the visual representation of How Git Works.

Here is the `python` code:

```bash
# Check out the "first-branch" branch
git checkout first-branch

# Make some modifications and stage them
git add modified-file-1.txt

# Commit the staged changes
git commit -m "Modified file 1 in first branch"

# Verify the new `HEAD` after the commit
git log --oneline --decorate
```

The `git log` command in the above code includes the `--decorate` option, displaying the most recent commit and any associated references.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What does the clone operation in Git do?</summary>

### What does the clone operation in Git do?

**Definition:**

Cloning in Git creates a local copy of the repository, allowing for version control and collaborative work. It establishes a link to a specified remote, permitting fetch and push operations.

**How it works:**

- Two-Part Process: Cloning involves duplicating the repository and setting up the local environment.
- Full Version History: The cloned repository typically includes all commits and file versions.
- Remote Linkage: A relationship is built with a designated remote for data synchronization.
- Local Repository: Storage on the user's machine where all project and version data reside.
- Remote Repository: An external, shared storage often on a server, accessible for collaborative work.
- Working Directory: The location on the user's file system where files are manipulated and changes are tracked for commits.
- Duplication: The clone process reproduces the entire project, with all branches and commits, onto the user's machine. This offers a complete, standalone history.
- Configuration: The local environment is configured to maintain synchronization with a specific remote repository. By default, the repository from which the clone is made (origin) is set as the primary remote, but additional remotes can be added if required.
- Integrity: Git ensures the integrity of the copied data, such as commits and file snapshots, during the cloning process.
- Selective History Contraints: While it's common for a clone to receive the entire repository history, limited cloning is also possible, especially with large repositories. Technique like shallow cloning or specifying a certain commit for cloning can be used.

After a successful clone, the local repository is configured to interact with a designated remote repository, typically on a centralized server like Github or Bitbucket.

For GitHub, cloning often uses the HTTPS mode. Upon updates to the remote, the local repository can be synchronized using standard Git commands (e.g., fetch, pull, push).

Technically, clonality is not tied to a specific remote URL; it's more about aligning the histories. For instance, after cloning from GitHub using HTTPS, remotes can be reconfigured to use SSH.

1. Acquiring Data: The clone command contacts the remote repository, retrieves its history and files, and then saves this snapshot locally.
2. Repository Creation: The `git clone` execution sets up a new repository, employing the data fetched from the target remote, and thus creates both the working directory and associated `.git` directory, which hosts the entire Git control structure.
3. Linkage Configuration: The clone operation automatically establishes a remote connection, naming it " origin" by default. Should you have multiple remotes or wish to use a different name, configuration adjustments may be required.
4. Extraction: The clone accomplishes the creation and association of the local repository, which users can then "extract" to their working directories for manipulation.
5. Status Verification: Confirming the successful execution of a clone can be done by taking a look at the current remotes via `git remote -v`, which should display the origin.

**Examples:**

Clone: Initiate the cloning process.

```bash
git clone <remote-repo-url>
```

Remote: Verify origin setup.

```bash
git remote -v
```

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>How does Git store information?</summary>

### How does Git store information?

**Definition:**

While Git does use a filesystem to store data, it isn't exclusively file-based. The three main components of the Git data structure are the repository, the index/staging area, and the local working copy.

**How it works:**

1. Local Working Copy: Where you directly interact with your files.
2. Staging Area (Index): Serves as a sort of "holding area" for changes you want to include in your next commit.
3. Local Repository: Where Git keeps track of the complete history and stores different versions of your files.

The repository houses the following key components:

- Object Database: Persistently stores snapshots of files and directories as objects, which can be further classified into:
  - Blob objects for individual file contents.
  - Tree objects that replicate file system hierarchy.
  - Commit objects linked to a specific state in the project's history.
  - Tag objects marking specific commits, often used for releases.
- Index/Stage: A dynamic workspace that mirrors the next commit's expected state.
- Head Pointer: A reference to the latest commit in your current working branch
- Branches: Human-readable names that reference specific commits, facilitating independent lines of development.

1. Blobs: SHA-1 hash is based on the file's content.
2. Trees: SHA-1 hash combines the file and directory contents within—changes in any file or sub-directory would trigger a new hash.
3. Commits: SHA-1 hash incorporates the tree representing the project's directory structure, along with parent commit(s), commit message, and author information.

Git allows for a 'lazy write' mechanism for objects. This means that an object file is written upon creation and remains unchanged until the content it represents changes. When a change occurs, it gets updated and its SHA-1 is recalculated. This particular feature is known as 'immutable storage'.

Here's an illustration for better understanding:

- Let's assume a file's content changes and you add the updated file to the staging area.
- The new version of the file is then stored as a Blob object, and the associated Tree object also gets updated.
- During the following commit, a new Commit object is created, which references the updated Tree object.
- Simultaneously, the Branch you're working on moves forward to reference this new commit.

This systematic approach assures the integrity of your project's history. Since any alteration in content, directory structure, or commit details will trigger a change in the SHA-1 hash of the related object(s), a change can be immediately recognized.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

### Basic Git Commands

<details>
<summary>How do you initialize a new Git repository?</summary>

### How do you initialize a new Git repository?

**Definition:**

Let's go through the step-by-step process of creating a new Git repository and then getting started by adding files, making commits, and pushing the repository to a remote location such as GitHub or Bitbucket.

**How it works:**

Using the Command Line.

1. Navigate to your project folder using your terminal or command prompt.
2. Run the following command to initialize a new Git repository: `git init`

Using Git GUI. Most Git GUI applications, like Sourcetree or GitKraken, facilitate setting up repositories through an intuitive interface. Once you've navigated to your project folder, choose the "Initialize/Create New Repository" option.

Use the following command to add files or directories to the staging area.

Once files are in the staging area, use this command to commit these changes.

If you plan to push your local repository to an online service like GitHub, first link your local repository to a remote one.

Replace "your-remote-repository-url" with the URL provided by your online repository.

Finally, use this command to push your local repository to the remote one.

After the initial "upstream" link is established, subsequent push commands can be executed with a simple `git push`.

**Examples:**

```bash
git init
```

```bash
# To add a specific file
git add filename

# To add all files and directories
git add .
```

```bash
git commit -m "Your commit message goes here"
```

```bash
git remote add origin your-remote-repository-url
```

```bash
git push -u origin master
```

Here is the `.gitignore` file:

```plaintext
# Ignored file - example
credentials.txt
```

**Thoughts:**

To ensure that sensitive data, such as API keys or credentials, are not shared, employ the `.gitignore` file to list files and patterns that should be excluded from version control.

In particular, avoid using files with such sensitive information before creating a `.gitignore`.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>Explain the purpose of the git status command.</summary>

### Explain the purpose of the git status command.

**Definition:**

`git status` is an essential command for tracking changes in a Git repository. It provides important details such as file states, current working branch, untracked files, and more.

**How it works:**

- On Branch: Indicates the active branch, allowing quick navigation through different branches.
- Changes to be Committed: Lists modified or staged files, providing a quick glance at what will be included in the next commit.
- Changes not staged for commit: Highlights modified files that were not yet staged for the next commit.
- Untracked Files: Recognizes files in the working directory that are not a part of the repository.
- Branch is Up to Date: Provides this affirmation when the working branch is up-to-date with its remote counterpart.
- Your Branch is Ahead of 'origin/master' by 1 Commit: Notifies when the current local branch outpaces the linked remote branch by a set number of commits. This is common in team-based workflows and signifies the necessity of a `push`.

Note: The remote branch in the example is 'origin/master', and the number of commits ahead is 1.

**Examples:**

Code Example: Terminal Output of `git status`.

Here is the code:

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
    modified:   app.js

Untracked files:
     (use "git add <file>..." to include in what will be committed)
    server.js

no changes added to commit (use "git add" and/or "git commit -a")
```

Code Example: Visual Diff of Staged Changes using `git diff --staged`.

Here is the code:

```text
diff --git a/readme.txt b/readme.txt
index ce01362..0602794 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1 @@
- Initial file version
+ Modified during stage
```

Code Example: Multifile `git status` Output.

Here is the code:

```text
On branch: master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
      modified:   file1.cpp
      modified:   file2.cpp
Both file3.cpp and file4.cpp are untracked.
Your branch and 'origin/master' have diverged,
and have 4 and 2 different commits each, respectively.
```

**Thoughts:**

- Regularly use `git status` to stay updated on the repository's state.
- Review and comprehend the provided information before proceeding with any actions, such as staging or committing.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What does the git add command do?</summary>

### What does the git add command do?

**Definition:**

`git add` is the first step in tracking changes you make to existing files or adding new files to your Git repository. It prepares the changes for the next commit by moving modified or new files to the staging area.

When you invoke `git add`, three major actions take place:

1. File Selection: You choose which specific file changes to include in the upcoming commit.
2. File Comparison: Git analyzes the selected files and identifies the exact line-by-line or block-level modifications.
3. Staging Area Update: The selected file changes are prepared and categorized. Once you're ready, you can make a commit action to move all the changes in the staging area to the repository.

**How it works:**

The staging area acts as a bridge between your working directory and the repository (history).

- Working Directory: Contains the active files you're currently editing.
- Staging Area (Index): A kind of "Sandbox" where you selectively group changes together for precise commits.
- Repository: The historical record of all your commits and their respective states.

- Add All Modifications: Restage modified files to the current state: `git add .`
- Add Interactively: Handpick changes from modified files: `git add -p`
- Merge Conflicts: After addressing conflicts, ready files for commit: `git add`

**Examples:**

Code Example: Staging vs Git History.

Here is the Python code:

```python
# File: my_file.py  (in the working directory)

# Current content
print("This line is part of the initial version.")

# Post staging changes (not committed yet)
print("This line will be part of the next commit.")
```

In our Python example, consider "file.py" as the file. Initially, the file contains one line, which was the initial version. Then "This line will be part of the next commit." gets added (not committed yet), so that these changes occur between `git add` and `git commit`.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>How do you create a commit in Git?</summary>

### How do you create a commit in Git?

**Definition:**

Let's look at the process of creating a commit in Git.

**How it works:**

Before making a commit, the user should review any changes in the working directory and staging area.

Untracked Files: These are new files that Git does not track yet. To track untracked files and subsequently include them in a commit, they need to be staged using the `git add` command.

Tracked Files: Files that have previously been committed or added to the staging area can be included in the next commit.

- Specific Files: Directly add individual files or a group of files using their names or paths.
- By Pattern: Use wildcards or named sets of files to add multiple files at once.
- Interactively: Launch a prompt where changes can be reviewed and selected for staging.

`git rm` and `git mv`. These commands remove and rename files respectively. Changes are then staged and committed similarly to new additions.

`git reset`. Unstage files that were mistakenly added to the staging area.

`git checkout`. To remove changes in the working directory, bring them back to the state of the `HEAD` commit.

`git commit`. This command is used to save changes to the local repository after they have been staged.

`-m`. Use a one-line message to describe the commit.

`-a`. Stage all modified files and commit them.

This command omits the staging step, effectively combining `git add` and `git commit`. Since it bypasses the staging area completely, care should be taken when using it, to ensure only intended changes are included in the commit.

When using `-a` without `-m`, Git will open the default text editor for you to compose the commit message in a separate file.

Some files might not need to be committed, for example:

- Temporary Files: This might include build artifacts or editor-specific temporary files.
- Sensitive Information: Such as passwords or private keys.

For such exclusions, you can define the file patterns or names in a `.gitignore` file.

To exclude files globally, create or edit the global `.gitignore` file.

Then, ensure to add a `.gitignore` file in the root directory for the local repository if needed.

**Examples:**

```bash
git add file1.txt path/to/file2.txt
```

```bash
git add *.txt
```

```bash
git add -i
```

```bash
git reset file1.txt
```

```bash
git checkout file1.txt
```

```bash
git commit -m "Add new feature xyz"
```

```bash
git commit -am "Update feature xyz"
```

```bash
git config --global core.excludesfile ~/.gitignore_global
```

```bash
touch .gitignore
```

**Thoughts:**

- Avoid tracking, staging, or committing sensitive information.
- Keep `.gitignore` up to date, especially in collaborative projects.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>What information is contained in a commit object in Git?</summary>

### What information is contained in a commit object in Git?

**Definition:**

The commit object in Git encapsulates rich textual data that aids in tracking and managing changes in the repository.

**How it works:**

- Author and Committer: These metadata entries contain the individual's name and email address as well as the timestamp.
- Commit Messages: They provide a brief yet comprehensive understanding of the changes introduced in the commit, helping in maintaining clarity and context.
- Parent Commits: This represents a pointer to one or more parent commits. In the case of a merge commit, there can be more than one parent.
- The `HEAD` of most branches points to the most recent commit, and this `reference` is what progresses as newer commits occur. The `HEAD` itself points to the most recent commit but operates indirectly through the branch reference.
- Tree Object: Each commit is linked to a tree object that represents the state of the content in the repository at the time of the commit. The tree object, in turn, holds references to the various blobs and subtrees.
- Here is a quick summary. A blob in Git is a file, and a tree is a collection of files and can even include other trees.
- A commit, therefore, is created with a tree-referencing object, capturing both the checked-in version of files (through the tree object) and a metadata snapshot (through its unique SHA key).
- Internal to that tree are also the various subtrees and blobs that the tree sticks to.

In cases when a branch or reference no longer points to a commit, Git still retains these dangling commits. Using `git reflog`, you can potentially recover them.

Advanced Git workflows often call for data integrity and authenticity, especially in distributed teams and open-source projects. Git employs GitPG to sign commits, verifying the commit objects' authenticity, flagging potential tampering or forging of prior commits.

**Sources:**

- [Devinterview.io — Git](https://devinterview.io/questions/web-and-mobile-development/git-interview-questions/)

</details>

<details>
<summary>How does git push differ from git fetch?</summary>

### How does git push differ from git fetch?

**Definition:**

git push updates refs on a remote from your local repository and sends objects the remote does not already have. git fetch downloads refs and the objects needed to complete their histories and updates remote-tracking branches. Fetch does not move your current branch.

**How it works:**

A plain git push uses the upstream of the current branch, or origin when no upstream is set. Which refs are pushed depends on the refspec, remote.<name>.push, or push.default (the default is simple, which pushes the current branch to a same-named branch). git fetch with no remote uses the upstream remote, or origin. It records fetched names in .git/FETCH_HEAD and, with the usual refspec, updates refs/remotes/<remote>/*. Neither command edits the other side's working tree.

**When to use:**

Use fetch when you only want to see remote commits. Use push when you want the remote refs to move to commits you already have locally.

**Examples:**

```bash
git fetch origin
git push origin main
git push --set-upstream origin main
```

**Sources:**

- [git-push](https://git-scm.com/docs/git-push)
- [git-fetch](https://git-scm.com/docs/git-fetch)
</details>

<details>
<summary>Explain the purpose of the git pull command.</summary>

### Explain the purpose of the git pull command.

**Definition:**

git pull fetches from a remote and then integrates the chosen branch into the current branch.

**How it works:**

It runs git fetch with the same arguments, excluding merge options. With no arguments it integrates the upstream of the current branch. If no way of reconciling divergent history is selected, git pull --ff-only is the default and the command fails when the local branch has diverged. git pull --rebase runs git rebase. git pull --no-rebase runs git merge. git pull --squash runs git merge --squash. pull.rebase, pull.ff, and pull.squash store that choice. A conflict can be abandoned with git merge --abort or git rebase --abort.

**When to use:**

Use pull when the current branch should take the remote commits immediately. Use fetch alone when you want to inspect them first.

**Examples:**

```bash
git pull
git pull --ff-only
git pull --rebase origin main
```

**Sources:**

- [git-pull](https://git-scm.com/docs/git-pull)
- [git-fetch](https://git-scm.com/docs/git-fetch)
</details>

<details>
<summary>Describe the use of the git branch command.</summary>

### Describe the use of the git branch command.

**Definition:**

git branch lists, creates, renames, copies, or deletes branch refs. It does not switch the working tree.

**How it works:**

With no arguments it lists local branches. git branch <name> creates a branch at the current HEAD. git branch <name> <start-point> starts it elsewhere. -d deletes a branch that is fully merged into its upstream, or into HEAD if no upstream was set. -D is --delete --force. -m renames, and -M forces the rename. -u / --set-upstream-to sets the upstream. -r lists remote-tracking branches. -r with -d deletes remote-tracking branches only; it does not delete the branch on the remote.

**Examples:**

```bash
git branch
git branch feature/login
git branch -d feature/login
git branch -u origin/main
```

**Sources:**

- [git-branch](https://git-scm.com/docs/git-branch)
</details>

<details>
<summary>How do you switch branches using git checkout?</summary>

### How do you switch branches using git checkout?

**Definition:**

git checkout <branch> points HEAD at that branch and updates the working tree and index to match it. Since Git 2.23, git switch is the command dedicated to switching branches.

**How it works:**

Checkout fails when uncommitted changes touch files that differ between the current commit and the target. Other uncommitted changes are kept. If the name is not a local branch but exactly one remote has a matching tracking branch, checkout is treated as git checkout -b <branch> --track <remote>/<branch> unless --no-guess is set. git checkout -b <name> creates the branch and switches to it. git switch <branch> does the switch. git switch -c <name> creates and switches in one step.

**When to use:**

Use git switch for moving between branches. Keep git checkout when you also need to restore a file from the index or from a commit.

**Examples:**

```bash
git checkout main
git checkout -b feature/login
git switch feature/login
git switch -c feature/login
```

**Sources:**

- [git-checkout](https://git-scm.com/docs/git-checkout)
- [git-switch](https://git-scm.com/docs/git-switch)
</details>

<details>
<summary>What is the purpose of git merge?</summary>

### What is the purpose of git merge?

**Definition:**

git merge joins two or more development histories into the current branch. git pull uses it when integrating with a merge.

**How it works:**

git merge topic replays changes made on topic since the histories diverged and records the result. Before the operation, ORIG_HEAD is set to the tip of the current branch. If every named commit is already an ancestor of HEAD, merge exits with "Already up to date." A fast-forward only moves the branch pointer. Otherwise a merge commit with two parents is created. The default strategy for one head is ort. The command stops on a conflict or when --no-commit was given. git merge --abort tries to return to the pre-merge state.

**Examples:**

```bash
git merge topic
git merge --no-ff topic
git merge --abort
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
</details>


### Git Branching and Merging

<details>
<summary>Explain the Git workflow you use most frequently.</summary>

### Explain the Git workflow you use most frequently.

**Definition:**

The everyday participant workflow is a local clone, a topic branch, commits, and then fetch or pull from origin plus push to the shared repository.

**How it works:**

giteveryday describes a standalone developer who commits locally, and a participant who also clones, pulls, and pushes. gitworkflows adds the rule used by the Git project itself: split work into small commits that can stand alone, and integrate with merge rather than rewriting published history. Before publishing, git rebase -i can still edit commits that nobody else has.

**When to use:**

Use a topic branch for each change. Integrate with merge once the commits are published. Rebase only history that is still private.

**Examples:**

```bash
git clone <repository> myrepo
git switch -c feature/login
git commit -a -m "Add login"
git fetch origin
git push --set-upstream origin feature/login
```

**Sources:**

- [giteveryday](https://git-scm.com/docs/giteveryday)
- [gitworkflows](https://git-scm.com/docs/gitworkflows)
</details>

<details>
<summary>Can you describe the process of creating a new branch and merging it back into the main branch?</summary>

### Can you describe the process of creating a new branch and merging it back into the main branch?

**Definition:**

Create a branch from the current tip, commit on it, switch back to the integration branch, and merge the topic branch.

**How it works:**

git switch -c <name> is the same as creating the branch and switching to it, and the branch is not created if the switch fails. git branch <name> creates the ref without switching. After the work is committed, git switch main moves HEAD to main, and git merge <name> brings the topic in. If main has not moved, the merge is a fast-forward. git merge --no-ff records a merge commit even then.

**Examples:**

```bash
git switch main
git switch -c feature/login
git commit -a -m "Add login"
git switch main
git merge feature/login
```

**Sources:**

- [git-switch](https://git-scm.com/docs/git-switch)
- [git-branch](https://git-scm.com/docs/git-branch)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>What is a merge conflict in Git and how do you resolve it?</summary>

### What is a merge conflict in Git and how do you resolve it?

**Definition:**

A merge conflict is a hunk both sides changed, so Git will not pick a side for you. It leaves conflict markers in the working tree and stops the merge.

**How it works:**

HEAD stays put. MERGE_HEAD points at the other branch. Clean paths are updated in the index and the working tree. For a conflicting path the index stores up to three stages: stage 1 is the common ancestor, stage 2 is HEAD, and stage 3 is MERGE_HEAD (git ls-files -u). The working tree file contains the merge result with <<<<<<<, =======, and >>>>>>> markers. The part before ======= is typically your side. Edit the file, remove the markers, git add the path, then git merge --continue or git commit. git merge --abort abandons the merge.

**Examples:**

```bash
git merge topic
git ls-files -u
git add path/to/file
git merge --continue
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>What is a fast-forward merge in Git?</summary>

### What is a fast-forward merge in Git?

**Definition:**

A fast-forward happens when the current branch head is an ancestor of the commit being merged. Git moves HEAD and the index to that commit and does not create a merge commit.

**How it works:**

This is the common case for git pull when you have no local commits on the branch. --ff is the default for an ordinary branch merge. --no-ff forces a merge commit even when a fast-forward is possible. --ff-only fast-forwards or exits with a non-zero status. --no-commit does not stop a fast-forward, because there is no merge commit to pause before.

**Examples:**

```bash
git merge topic
git merge --ff-only topic
git merge --no-ff topic
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>Define a three-way merge.</summary>

### Define a three-way merge.

**Definition:**

A three-way merge reconciles two tips by using their common ancestor. Git applies changes from both sides onto that base.

**How it works:**

Except in a fast-forward, the result is a merge commit whose parents are the two branch tips. Non-overlapping changes are taken as they are. Where both sides changed the same area, Git stops and writes conflict markers. With the default ort strategy it also writes AUTO_MERGE, a tree of the working tree including those markers. git diff shows a three-way diff of changes from HEAD and from MERGE_HEAD. git show :1:filename, :2:filename, and :3:filename show the ancestor, HEAD, and MERGE_HEAD versions.

**Examples:**

```bash
git merge topic
git diff
git show :1:filename
git show :2:filename
git show :3:filename
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>How do you rebase a branch in Git?</summary>

### How do you rebase a branch in Git?

**Definition:**

git rebase transplants a series of commits onto a new starting point. The original commits are replayed as new commits.

**How it works:**

With topic checked out, git rebase master takes the commits on topic since it diverged from master and replays them on top of master. git rebase master topic is the shortcut for switching to topic and rebasing it onto master. The result is a new chain (A', B', C') and topic moves to the new tip. Conflicts stop the rebase. git rebase --continue goes on after you fix and stage them. git rebase --skip drops the current commit. git rebase --abort returns to the branch state from before the rebase.

**When to use:**

Rebase a branch before anyone else has fetched those commits. After the commits are published, prefer merge so other clones do not have to recover from rewritten history.

**Examples:**

```bash
git switch topic
git rebase master
git rebase --continue
git rebase --abort
```

**Sources:**

- [git-rebase](https://git-scm.com/docs/git-rebase)
</details>

<details>
<summary>Explain the pros and cons of rebasing vs. merging.</summary>

### Explain the pros and cons of rebasing vs. merging.

**Definition:**

Merge joins two histories with a merge commit (unless the merge is a fast-forward). Rebase replays your commits onto another tip and writes new commit objects.

**How it works:**

gitworkflows prefers merge because a merge carries every commit on a branch in one step and keeps the record of how the branches joined. Rebase rewrites the commits, which is how git rebase -i also reorders and combines them before they are published. The rebase manual points at its recovering-from-upstream-rebase section when published commits are rewritten.

**When to use:**

Merge published work. Rebase only commits that are still local, when you want a linear series.

**Pluses and minuses:**

- Merge keeps both parents and scales to many commits in one step. A fast-forward does not even add a merge commit.
- Rebase produces a straight line of new commits and can squash or reorder them before publication.
- Rebase rewrites commit IDs. Anyone who already fetched the old commits has to reconcile the rewritten history.
- Merge history shows the join, which can make a long-lived branch's graph harder to read.

**Examples:**

```bash
git merge topic
git rebase master
git rebase -i HEAD~5
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
- [git-rebase](https://git-scm.com/docs/git-rebase)
- [gitworkflows](https://git-scm.com/docs/gitworkflows)
</details>

<details>
<summary>What is the purpose of git tag?</summary>

### What is the purpose of git tag?

**Definition:**

git tag creates, lists, deletes, or verifies a ref under refs/tags/. A tag names a commit, usually a release point, and does not move when you commit.

**How it works:**

Without -a, -s, or -u, the tag is lightweight: a ref that points straight at the object. -a creates a tag object and asks for a message (or takes -m / -F). -s or -u <key-id> creates a signed tag object. The signing backend follows gpg.format. git tag with no arguments lists tags. -d deletes the named tags. -v verifies signatures.

**Examples:**

```bash
git tag
git tag -a v1.2.0 -m "Release 1.2.0"
git tag -s v1.2.0 -m "Release 1.2.0"
git tag -d v1.2.0
```

**Sources:**

- [git-tag](https://git-scm.com/docs/git-tag)
</details>

<details>
<summary>How do you revert a commit that has already been pushed to the remote repository?</summary>

### How do you revert a commit that has already been pushed to the remote repository?

**Definition:**

git revert records a new commit that undoes the patch of an earlier commit. The old commit stays in history, so the revert can be pushed without rewriting published commits.

**How it works:**

The working tree must be clean. git revert is not git reset --hard and it is not git restore: those discard uncommitted work. After the revert commit exists locally, git push sends it to the remote. The rebase manual warns that amending or rebasing a commit that is already published rewrites history that other people may have fetched.

**When to use:**

Use revert for a commit that is already on the remote. Use reset or rebase only for commits that have not been published.

**Examples:**

```bash
git revert <commit>
git push origin main
```

**Sources:**

- [git-revert](https://git-scm.com/docs/git-revert)
- [git-push](https://git-scm.com/docs/git-push)
- [git-rebase](https://git-scm.com/docs/git-rebase)
</details>

<details>
<summary>What is the significance of the master branch in Git?</summary>

### What is the significance of the master branch in Git?

**Definition:**

master is only a branch name. In current Git it is still the default name of the unborn branch that git init creates, and that default becomes main in Git 3.0.

**How it works:**

git init -b <branch-name> or --initial-branch chooses the name. init.defaultBranch overrides the default. The branch is not special in the object database: HEAD normally points at a branch, and that branch points at a commit. A repository can use main, master, or any other name as the integration branch.

**Examples:**

```bash
git init
git init -b main
git config --global init.defaultBranch main
```

**Sources:**

- [git-init](https://git-scm.com/docs/git-init)
- [git-checkout](https://git-scm.com/docs/git-checkout)
</details>


### Git Workflows and Patterns

<details>
<summary>What is a distributed version control system and how does Git function as one?</summary>

### What is a distributed version control system and how does Git function as one?

**Definition:**

In Git every clone is a full repository. A developer can commit, branch, merge, and inspect history without contacting a server. A remote is another repository you choose to exchange refs with.

**How it works:**

git clone copies the repository. git fetch and git pull download refs and objects. git push updates refs on a remote. giteveryday splits the roles: a standalone developer works locally, a participant also fetches and pushes, and an integrator merges what others publish. There is no single central commit store inside Git itself. A shared remote is a workflow choice, not a requirement of the object model.

**Examples:**

```bash
git clone <repository> myrepo
git fetch origin
git push origin main
```

**Sources:**

- [git-clone](https://git-scm.com/docs/git-clone)
- [giteveryday](https://git-scm.com/docs/giteveryday)
- [gitglossary](https://git-scm.com/docs/gitglossary)
</details>

<details>
<summary>Describe the Git feature branch workflow.</summary>

### Describe the Git feature branch workflow.

**Definition:**

A feature branch is a short-lived branch that holds one change. You commit there, then merge it into the integration branch.

**How it works:**

gitworkflows says to split changes into small steps and to commit each step so it works on its own. Branches are cheap refs. Merge, not a pile of cherry-picks, is the tool that brings a whole branch across. git switch -c starts the branch. git merge brings it back. Before the branch is published, git rebase -i can still squash or reorder it.

**Examples:**

```bash
git switch -c feature/login main
git commit -a -m "Add login form"
git switch main
git merge feature/login
```

**Sources:**

- [gitworkflows](https://git-scm.com/docs/gitworkflows)
- [git-switch](https://git-scm.com/docs/git-switch)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>Explain the Gitflow workflow.</summary>

### Explain the Gitflow workflow.

**Definition:**

Git has no Gitflow subcommand. Gitflow is a branching convention: long-lived integration branches plus short-lived feature, release, and hotfix branches, built from git branch, git merge, and git tag.

**How it works:**

A feature branch is created from the integration branch and merged back, often with --no-ff so the merge commit stays even when a fast-forward was possible. A release point is an annotated tag (git tag -a). gitworkflows, which is the workflow the Git project documents, is a different convention: small commits, merge to integrate, and no requirement for a permanent second long-lived branch.

**When to use:**

Use it when the team has agreed on those branch names. The commands are still branch, merge, and tag.

**Examples:**

```bash
git switch -c feature/login main
git switch main
git merge --no-ff feature/login
git tag -a v1.2.0 -m "Release 1.2.0"
```

**Sources:**

- [git-branch](https://git-scm.com/docs/git-branch)
- [git-merge](https://git-scm.com/docs/git-merge)
- [git-tag](https://git-scm.com/docs/git-tag)
- [gitworkflows](https://git-scm.com/docs/gitworkflows)
</details>

<details>
<summary>What is a Forking workflow in Git?</summary>

### What is a Forking workflow in Git?

**Definition:**

Git's own form of a fork is a separate clone. You publish commits in your repository and ask the upstream to pull them. A hosting service's fork button is outside Git.

**How it works:**

giteveryday's participant clones upstream, works on a branch, and either sends patches with git format-patch and git send-email or writes a pull summary with git request-pull. git request-pull <start> <URL> [<end>] prints a request that assumes the upstream already has <start> and should integrate commits up to <end> (default HEAD) from <URL>.

**Examples:**

```bash
git clone <upstream-url> myrepo
git switch -c mine
git request-pull origin/main <your-public-url>
```

**Sources:**

- [giteveryday](https://git-scm.com/docs/giteveryday)
- [git-request-pull](https://git-scm.com/docs/git-request-pull)
- [git-clone](https://git-scm.com/docs/git-clone)
</details>

<details>
<summary>Discuss the Centralized workflow in Git.</summary>

### Discuss the Centralized workflow in Git.

**Definition:**

A centralized workflow is a shared repository that everyone pushes to, the same shape as a CVS server. Git still keeps a full history in every clone.

**How it works:**

giteveryday names this the CVS-style shared repository: participants git push to it, and an integrator on that repository merges. push.default=simple updates the same-named branch. A bare repository (git init --bare or git clone --bare) has no working tree for someone to edit on the server. The update hook can reject a push that is not a fast-forward.

**When to use:**

Use one shared remote when the team agrees to integrate on a single repository. Each clone can still commit offline.

**Examples:**

```bash
git clone <shared-url> project
git push origin main
git init --bare project.git
```

**Sources:**

- [giteveryday](https://git-scm.com/docs/giteveryday)
- [git-push](https://git-scm.com/docs/git-push)
- [githooks](https://git-scm.com/docs/githooks)
- [git-init](https://git-scm.com/docs/git-init)
</details>


### Git Repository Management

<details>
<summary>Explain what a remote repository is in Git.</summary>

### Explain what a remote repository is in Git.

**Definition:**

A remote is a named repository URL that this repository fetches from and pushes to. origin is the remote git clone creates for the source you cloned.

**How it works:**

The URL is stored in configuration, along with a fetch refspec. The usual refspec +refs/heads/*:refs/remotes/origin/* fetches every branch and updates the matching remote-tracking branch. Remote-tracking branches such as origin/main are refs in your repository. They move when you fetch. They are not your working branch.

**Examples:**

```bash
git remote
git fetch origin
git push origin main
```

**Sources:**

- [git-remote](https://git-scm.com/docs/git-remote)
- [git-fetch](https://git-scm.com/docs/git-fetch)
- [gitglossary](https://git-scm.com/docs/gitglossary)
</details>

<details>
<summary>Describe how you would change a Git remote URL.</summary>

### Describe how you would change a Git remote URL.

**Definition:**

git remote set-url changes the URL stored for a remote name. The push URL and the fetch URL must still refer to the same place.

**How it works:**

With no <oldurl>, set-url replaces the first URL. <oldurl> is a regex matched against the current URL. --push changes the push URL instead of the fetch URL. --add adds a URL. --delete removes URLs that match. If you need to fetch from one repository and push to another, configure two remotes rather than two URLs that are not the same place.

**Examples:**

```bash
git remote set-url origin <new-url>
git remote -v
```

**Sources:**

- [git-remote](https://git-scm.com/docs/git-remote)
</details>

<details>
<summary>How do you synchronize your local repository with a remote repository?</summary>

### How do you synchronize your local repository with a remote repository?

**Definition:**

Fetch downloads remote refs and objects. Push updates remote refs from local commits. Pull fetches and then integrates into the current branch.

**How it works:**

git fetch origin updates remote-tracking branches and writes .git/FETCH_HEAD. It does not move your current branch. git pull then integrates the upstream, by fast-forward only unless you ask for a rebase or a merge. git push origin <branch> sends the local branch. The first push of a new branch can set its upstream with --set-upstream.

**Examples:**

```bash
git fetch origin
git pull --ff-only
git push --set-upstream origin feature/login
```

**Sources:**

- [git-fetch](https://git-scm.com/docs/git-fetch)
- [git-pull](https://git-scm.com/docs/git-pull)
- [git-push](https://git-scm.com/docs/git-push)
</details>

<details>
<summary>How do you clean up unused branches in your local and remote repository?</summary>

### How do you clean up unused branches in your local and remote repository?

**Definition:**

Delete a merged local branch with git branch -d. Delete a remote branch with git push --delete. Prune remote-tracking refs that the remote has already removed.

**How it works:**

-d refuses to delete a branch that is not fully merged into its upstream, or into HEAD if no upstream was set. -D forces the local delete. git push <remote> --delete <branch> deletes that ref on the remote; it is the same as pushing a refspec that starts with a colon. git fetch --prune removes remote-tracking refs that no longer exist on the remote. git remote prune <name> prunes without fetching new refs. -d does not delete the branch on the server.

**Examples:**

```bash
git branch -d feature/login
git push origin --delete feature/login
git fetch --prune origin
git remote prune origin
```

**Sources:**

- [git-branch](https://git-scm.com/docs/git-branch)
- [git-push](https://git-scm.com/docs/git-push)
- [git-fetch](https://git-scm.com/docs/git-fetch)
- [git-remote](https://git-scm.com/docs/git-remote)
</details>

<details>
<summary>Describe the steps to resolve a merge conflict in Git.</summary>

### Describe the steps to resolve a merge conflict in Git.

**Definition:**

Stop, edit the conflicted files, stage them, and conclude the merge. Or abort and return to the pre-merge state.

**How it works:**

Git marks the hunk with <<<<<<<, =======, and >>>>>>>. Set merge.conflictStyle to diff3 or zdiff3 if you also want the ||||||| base section. Remove the markers and keep the text you want. git add records the resolution. git merge --continue checks that a merge is in progress and then commits. git commit does that commit directly. git diff AUTO_MERGE shows what you have changed while resolving. git merge --abort gives up.

**Examples:**

```bash
git merge topic
git add path/to/file
git merge --continue
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
</details>


### Git Stashing and Cleaning

<details>
<summary>What is git stash used for?</summary>

### What is git stash used for?

**Definition:**

git stash stores uncommitted changes so the working tree can do something else, then reapplies them later.

**How it works:**

A stash entry is a commit that holds the dirty work. The examples in the manual include pulling into a dirty tree (stash, pull, stash pop) and pausing a task to make an emergency commit. git stash push can limit the stash with a pathspec. --keep-index stashes changes that are not staged so you can test what is in the index. --staged stashes only the index. pop applies the latest stash (stash@{0} by default) and drops it.

**When to use:**

Stash when a pull or a branch switch must happen before the current edit is ready to commit.

**Examples:**

```bash
git stash
git pull
git stash pop
```

**Sources:**

- [git-stash](https://git-scm.com/docs/git-stash)
</details>

<details>
<summary>How do you view stashed changes in Git?</summary>

### How do you view stashed changes in Git?

**Definition:**

git stash list prints the stash entries. git stash show prints the diff of one entry.

**How it works:**

list accepts the log options that git log accepts. show defaults to the latest stash. -u / --include-untracked includes untracked files in the diff. --only-untracked shows only those. Pass stash@{n} to inspect an older entry.

**Examples:**

```bash
git stash list
git stash show
git stash show -u stash@{1}
```

**Sources:**

- [git-stash](https://git-scm.com/docs/git-stash)
</details>

<details>
<summary>Can you apply a stash without removing it from the stash list?</summary>

### Can you apply a stash without removing it from the stash list?

**Definition:**

Yes. git stash apply restores the stashed changes and leaves the entry in the list. git stash pop applies it and then drops it.

**How it works:**

Both default to stash@{0}. --index also tries to restore the index state, not only the working tree. If the apply conflicts, the stash entry is still there after apply, so you can retry. pop removes the entry only when the apply succeeds.

**When to use:**

Use apply when you may need the same stash again. Use pop when this application should consume it.

**Examples:**

```bash
git stash apply
git stash apply --index stash@{1}
git stash pop
```

**Sources:**

- [git-stash](https://git-scm.com/docs/git-stash)
</details>

<details>
<summary>What does the git clean command do?</summary>

### What does the git clean command do?

**Definition:**

git clean recursively removes files in the working tree that are not under version control, starting from the current directory.

**How it works:**

Unknown files are removed. Ignored files stay unless -x is given, which is how you also remove build products. -X removes only ignored files. Without a pathspec, clean does not recurse into untracked directories unless -d is set. If clean.requireForce is not false, clean refuses to delete unless -f is passed. A second -f is required to remove an untracked nested repository. -n is a dry run and deletes nothing. -i walks the candidates interactively.

**When to use:**

Dry-run with -n before -f. Do not use clean to drop tracked files; it only removes untracked ones.

**Examples:**

```bash
git clean -n -d
git clean -f -d
```

**Sources:**

- [git-clean](https://git-scm.com/docs/git-clean)
</details>

<details>
<summary>How do you remove a file from the working directory without deleting it from the repository?</summary>

### How do you remove a file from the working directory without deleting it from the repository?

**Definition:**

The current commit keeps the file if you do not record a deletion. Take the file out of the working tree and put the path back in the index from HEAD.

**How it works:**

git rm removes the file from the working tree and stages the deletion. git restore --staged restores the index from HEAD when --staged is given and --source is omitted, and it does not restore the working tree unless --worktree is also given. After that, HEAD and the index still contain the file and the working tree does not. Do not commit while the working tree deletion is still showing, or the next commit will drop the path. git restore --worktree --source=HEAD -- <file> puts the file back. The inverse operation, stop tracking the path but leave the worktree file in place, is git rm --cached.

**Examples:**

```bash
git rm file.txt
git restore --staged -- file.txt
git restore --worktree --source=HEAD -- file.txt
```

**Sources:**

- [git-rm](https://git-scm.com/docs/git-rm)
- [git-restore](https://git-scm.com/docs/git-restore)
</details>


### Git Logging and History

<details>
<summary>How do you view the commit history in Git?</summary>

### How do you view the commit history in Git?

**Definition:**

git log lists commits. git show prints one object; for a commit that is the log message and the textual diff.

**How it works:**

The user manual's history examples include git log, git log with a path, git log A..B for commits reachable from B but not from A, git log -p to include patches, and git show for the most recent commit. git log --since limits by date. A range such as master..topic lists commits on topic that are not on master.

**Examples:**

```bash
git log
git log -p
git log main..feature/login
git show
```

**Sources:**

- [git-log](https://git-scm.com/docs/git-log)
- [user-manual](https://git-scm.com/docs/user-manual)
- [git-show](https://git-scm.com/docs/git-show)
</details>

<details>
<summary>Explain how to find a commit by a specific author in Git.</summary>

### Explain how to find a commit by a specific author in Git.

**Definition:**

git log --author=<pattern> limits the list to commits whose author header matches the regular expression.

**How it works:**

The option comes from the revision-walking options that git log includes. More than one --author=<pattern> selects commits that match any of the patterns. --committer=<pattern> filters the committer header the same way. This is not a search of the changed text; git log -S<string> finds commits whose diff contains that text.

**Examples:**

```bash
git log --author=<pattern>
git log --committer=<pattern>
```

**Sources:**

- [git-log](https://git-scm.com/docs/git-log)
</details>

<details>
<summary>How do you view the changes introduced in a commit?</summary>

### How do you view the changes introduced in a commit?

**Definition:**

git show on a commit prints the log message and the textual diff. git log -p prints the same kind of patch for each commit it lists.

**How it works:**

For a merge commit, git show uses the combined diff format of git diff-tree --cc. git show <tag> prints the tag message and the tagged object. Pass a revision to show that commit instead of HEAD.

**Examples:**

```bash
git show
git show <commit>
git log -p -1 <commit>
```

**Sources:**

- [git-show](https://git-scm.com/docs/git-show)
- [user-manual](https://git-scm.com/docs/user-manual)
</details>

<details>
<summary>Can you list the files changed in a specific commit?</summary>

### Can you list the files changed in a specific commit?

**Definition:**

Yes. --name-only, a diff option that git show and git log accept, prints only the name of each changed file. git diff-tree compares the trees of a commit and its parent.

**How it works:**

--name-status adds the status letter. git diff-tree with one commit compares that commit to its parents. -r recurses into subtrees. --no-commit-id is in the diff-tree synopsis so the commit id is not mixed into the name list.

**Examples:**

```bash
git show --name-only <commit>
git diff-tree --no-commit-id --name-only -r <commit>
```

**Sources:**

- [git-show](https://git-scm.com/docs/git-show)
- [git-diff-tree](https://git-scm.com/docs/git-diff-tree)
</details>

<details>
<summary>What does the git blame command do?</summary>

### What does the git blame command do?

**Definition:**

git blame annotates each line of a file with the revision that last modified that line.

**How it works:**

An optional revision starts the annotation there instead of at the working copy. -L limits the annotation to the requested line ranges. Whole-file renames are followed automatically. Lines moved or copied from another file are a separate follow mode described in the blame manual.

**Examples:**

```bash
git blame <file>
git blame -L <start>,<end> <file>
```

**Sources:**

- [git-blame](https://git-scm.com/docs/git-blame)
</details>


### Git Tagging and Releases

<details>
<summary>What are Git tags and how are they different from branches?</summary>

### What are Git tags and how are they different from branches?

**Definition:**

A tag is a ref under refs/tags/ that names one object. A branch is a ref under refs/heads/ that moves forward when you commit on it.

**How it works:**

A lightweight tag points directly at the object. An annotated tag (-a, or implied by -m) is a tag object with a message. A signed tag (-s or -u) is an annotated tag with a signature. HEAD normally points at a branch, and new commits advance that branch. Checking out a tag points HEAD at the commit itself, which is detached HEAD. The checkout manual draws this with tag v2.0 pointing at an older commit while master has moved on.

**Examples:**

```bash
git tag -a v1.2.0 -m "Release 1.2.0"
git switch main
```

**Sources:**

- [git-tag](https://git-scm.com/docs/git-tag)
- [git-checkout](https://git-scm.com/docs/git-checkout)
- [gitglossary](https://git-scm.com/docs/gitglossary)
</details>

<details>
<summary>How do you create, delete, and push tags?</summary>

### How do you create, delete, and push tags?

**Definition:**

git tag creates the ref locally. git push --tags sends refs under refs/tags. git tag -d deletes a local tag. Deleting a remote tag is a push of a delete.

**How it works:**

-a stores a tag object and a message. -s signs it. -f replaces an existing local tag. The tag manual says that if the tag was already pushed, the sane fix for a wrong commit is a new name, because two people can otherwise both have "version X" and mean different commits. git push --tags pushes every tag ref in addition to the refspecs you listed. --follow-tags pushes annotated tags that point at commits reachable from the refs being pushed and that the remote does not have yet. git push <remote> --delete <tag> deletes the remote ref.

**Examples:**

```bash
git tag -a v1.2.0 -m "Release 1.2.0"
git push origin --tags
git tag -d v1.2.0
git push origin --delete v1.2.0
```

**Sources:**

- [git-tag](https://git-scm.com/docs/git-tag)
- [git-push](https://git-scm.com/docs/git-push)
</details>

<details>
<summary>What is semantic versioning in the context of Git tagging?</summary>

### What is semantic versioning in the context of Git tagging?

**Definition:**

Git does not interpret version numbers. A tag name such as v1.2.0 is only a ref under refs/tags/. Any major.minor.patch scheme is a naming convention your project applies when it creates that tag.

**How it works:**

git tag -a v1.2.0 -m "..." stores the name and the message in a tag object. git tag --list 'v-*' lists names that match a shell wildcard. Nothing in git tag checks that a newer tag has a greater version. Merging an annotated tag that is not stored in its natural refs/tags/ place creates a merge commit even when a fast-forward is possible; git merge v1.2.3^0 or git merge --ff-only v1.2.3 integrates the tagged commit without that extra merge commit.

**Examples:**

```bash
git tag -a v1.2.0 -m "Release 1.2.0"
git tag --list 'v-*'
git merge --ff-only v1.2.0
```

**Sources:**

- [git-tag](https://git-scm.com/docs/git-tag)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>How would you checkout to a specific tag?</summary>

### How would you checkout to a specific tag?

**Definition:**

git checkout <tag> or git switch --detach <tag> points HEAD at the tagged commit. That is detached HEAD: HEAD names the commit, not a branch.

**How it works:**

The checkout manual's picture is git checkout v2.0 while master stays where it was. New commits made there are referenced only by HEAD until you create a ref. git switch -c <name> or git checkout -b <name> creates a branch at the current commit and leaves detached HEAD. git switch --detach does the same detach as checkout of a non-branch commit.

**When to use:**

Detach when you only need to inspect or build that release. Create a branch when new commits must be kept.

**Examples:**

```bash
git checkout v1.2.0
git switch --detach v1.2.0
git switch -c maint-1.2 v1.2.0
```

**Sources:**

- [git-checkout](https://git-scm.com/docs/git-checkout)
- [git-switch](https://git-scm.com/docs/git-switch)
</details>

<details>
<summary>Describe how to create a release from a tag.</summary>

### Describe how to create a release from a tag.

**Definition:**

Create an annotated tag on the commit you are releasing, then push the tag. Git does not build or publish a hosting-service release for you.

**How it works:**

git tag -a records a tag object and a message. -s signs that object. git push origin <tag> or git push --tags sends it. git push --follow-tags sends annotated tags that point at commits you are already pushing. If the tag was pushed on the wrong commit, the tag manual's recommended fix is a new tag name, not git tag -f of a name other people already fetched.

**Examples:**

```bash
git tag -a v1.2.0 -m "Release 1.2.0"
git push origin v1.2.0
git show v1.2.0
```

**Sources:**

- [git-tag](https://git-scm.com/docs/git-tag)
- [git-push](https://git-scm.com/docs/git-push)
- [git-show](https://git-scm.com/docs/git-show)
</details>


### Advanced Git Operations

<details>
<summary>What is a git submodule and when would you use one?</summary>

### What is a git submodule and when would you use one?

**Definition:**

A submodule is a repository embedded in another repository. The outer repository is the superproject, and it records which commit of the submodule to use.

**How it works:**

On disk the submodule usually has a Git directory under the superproject's $GIT_DIR/modules/, a working directory inside the superproject, and a .git file that points at that Git directory. git submodule add <repository> [<path>] registers the submodule for the next commit of the superproject. git submodule update checks out the commit the superproject recorded. git submodule status lists them.

**When to use:**

Use a submodule when that dependency must stay a separate history and the superproject should pin one of its commits.

**Examples:**

```bash
git submodule add <repository> path/to/lib
git submodule status
git submodule update
```

**Sources:**

- [gitsubmodules](https://git-scm.com/docs/gitsubmodules)
- [git-submodule](https://git-scm.com/docs/git-submodule)
</details>

<details>
<summary>Explain the purpose of a git hook.</summary>

### Explain the purpose of a git hook.

**Definition:**

A hook is a script Git runs at a defined point, such as before a commit or when receiving a push. A non-zero exit can reject the operation when the hook is defined to do that.

**How it works:**

Hooks live in the hooks directory of the repository unless core.hooksPath points elsewhere. pre-commit runs before the commit message is taken and before the commit is created; --no-verify skips it. pre-push runs before objects are sent; a non-zero status aborts the push. On the receiving side, pre-receive reads <old-oid> <new-oid> <ref-name> lines and can reject the whole update. update runs once per ref and can enforce a fast-forward. post-receive runs after refs are updated and cannot change the result of the receive.

**Examples:**

```bash
git config core.hooksPath .githooks
# .git/hooks/pre-commit exits non-zero to reject the commit
```

**Sources:**

- [githooks](https://git-scm.com/docs/githooks)
- [git-config](https://git-scm.com/docs/git-config)
</details>

<details>
<summary>How do you perform a squash commit in Git?</summary>

### How do you perform a squash commit in Git?

**Definition:**

Squash folds several commits into one. Interactive rebase does that by marking later commits squash or fixup. git merge --squash applies the changes and leaves them staged without a merge commit.

**How it works:**

git rebase -i <after-this-commit> opens the todo list. Replacing pick with squash on the following lines folds those commits into the earlier one and concatenates their messages. fixup folds the changes and drops the message, unless fixup -c or fixup -C is used. git merge --squash records the tree changes as if a merge happened but does not create the merge commit; you commit the staged result yourself.

**When to use:**

Squash local commits before they are pushed. Do not rebase commits other people already fetched.

**Examples:**

```bash
git rebase -i HEAD~3
git merge --squash feature/login
git commit -m "Add login"
```

**Sources:**

- [git-rebase](https://git-scm.com/docs/git-rebase)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>What is a git bisect and how do you use it?</summary>

### What is a git bisect and how do you use it?

**Definition:**

git bisect binary-searches history for the commit that introduced a bug, or any other change you can test.

**How it works:**

You mark a bad commit that has the bug and a good commit from before the bug. Bisect checks out a commit in between and you answer git bisect good or git bisect bad until one commit remains. old and new can replace good and bad. git bisect reset returns you to the branch you started from. git bisect run <cmd> tests each step with a command.

**Examples:**

```bash
git bisect start
git bisect bad
git bisect good <known-good>
git bisect reset
```

**Sources:**

- [git-bisect](https://git-scm.com/docs/git-bisect)
</details>

<details>
<summary>How do you manually resolve a conflict during a git merge?</summary>

### How do you manually resolve a conflict during a git merge?

**Definition:**

Edit the conflicted file until the markers are gone, stage it, and continue the merge.

**How it works:**

The default marker style is the RCS merge style: text between <<<<<<< and ======= is your side, and text between ======= and >>>>>>> is their side. diff3 and zdiff3 add the ancestor between ||||||| and =======. git show :1:path, :2:path, and :3:path print the three stages. git add path marks the path resolved. git merge --continue commits. git merge --abort stops and tries to restore the pre-merge state.

**Examples:**

```bash
git show :1:path/to/file
git show :2:path/to/file
git show :3:path/to/file
git add path/to/file
git merge --continue
```

**Sources:**

- [git-merge](https://git-scm.com/docs/git-merge)
</details>


### Git Configuration and Customization

<details>
<summary>How do you set your username and email in Git?</summary>

### How do you set your username and email in Git?

**Definition:**

user.name and user.email are what Git writes into the author and committer fields of a commit. They are not the credentials used to authenticate to a remote.

**How it works:**

git config --global writes ~/.gitconfig (or $XDG_CONFIG_HOME/git/config if that file exists and ~/.gitconfig does not). Without --global, git config writes the repository .git/config. author.name, author.email, committer.name, and committer.email override the user.* pair for that role. GIT_AUTHOR_NAME, GIT_AUTHOR_EMAIL, GIT_COMMITTER_NAME, GIT_COMMITTER_EMAIL, and EMAIL override the config for one command. user.useConfigOnly true stops Git from guessing a name or email.

**Examples:**

```bash
git config --global user.name "Ada Lovelace"
git config --global user.email "ada@example.com"
```

**Sources:**

- [git-config](https://git-scm.com/docs/git-config)
</details>

<details>
<summary>What is the scope of Git configuration files?</summary>

### What is the scope of Git configuration files?

**Definition:**

Git reads system, global, and repository configuration. A more local file overrides the same key from a wider file when both are read.

**How it works:**

git config --system uses $(prefix)/etc/gitconfig. --global uses ~/.gitconfig and $XDG_CONFIG_HOME/git/config. --local uses $GIT_DIR/config and is the default when writing. --worktree uses $GIT_DIR/config.worktree when extensions.worktreeConfig is enabled. --file writes or reads one chosen file. Reading without a scope flag consults the files together.

**Examples:**

```bash
git config --system --list
git config --global --list
git config --local --list
```

**Sources:**

- [git-config](https://git-scm.com/docs/git-config)
</details>

<details>
<summary>How can you create aliases for Git commands?</summary>

### How can you create aliases for Git commands?

**Definition:**

An alias is a name git expands before it runs a command. git config alias.<name> stores it.

**How it works:**

A simple alias name is ASCII letters, digits, and hyphen, matched without case. The form git config --global alias.last "cat-file commit HEAD" makes git last run git cat-file commit HEAD. An alias with a subsection (alias.<name>.command) can use other characters in the name and is matched as raw bytes. Aliases are for the git command, not a replacement for shell scripts that need their own options parsed first.

**Examples:**

```bash
git config --global alias.st status
git config --global alias.last "cat-file commit HEAD"
git st
```

**Sources:**

- [git-config](https://git-scm.com/docs/git-config)
</details>

<details>
<summary>What is the purpose of a .gitignore file?</summary>

### What is the purpose of a .gitignore file?

**Definition:**

A gitignore file names untracked paths that Git should ignore. Paths that are already tracked are not affected.

**How it works:**

Patterns are checked from several sources. The highest precedence is patterns given on the command line for commands that support them, then patterns in .git/info/exclude, then core.excludesFile, then .gitignore files from the parent directory down to the file's directory. Within one precedence level, the last matching pattern wins. A tracked file stays tracked until you remove it from the index.

**Examples:**

```bash
printf '%s\n' '*.log' > .gitignore
git status
```

**Sources:**

- [gitignore](https://git-scm.com/docs/gitignore)
</details>

<details>
<summary>How would you ignore files globally?</summary>

### How would you ignore files globally?

**Definition:**

core.excludesFile is a pattern file applied in addition to per-directory .gitignore files and .git/info/exclude.

**How it works:**

The default path is $XDG_CONFIG_HOME/git/ignore. If XDG_CONFIG_HOME is unset or empty, Git uses $HOME/.config/git/ignore. Point core.excludesFile at another file when you want a different global ignore list. This still does not untrack files that are already in the index.

**Examples:**

```bash
git config --global core.excludesFile ~/.config/git/ignore
```

**Sources:**

- [git-config](https://git-scm.com/docs/git-config)
- [gitignore](https://git-scm.com/docs/gitignore)
</details>


### Git Security

<details>
<summary>How do you ensure your commits are signed with your GPG key?</summary>

### How do you ensure your commits are signed with your GPG key?

**Definition:**

git commit -S signs the commit. commit.gpgSign true signs every commit. user.signingKey selects the key when the default identity is not the one you want.

**How it works:**

-S takes an optional key id stuck to the option with no space. The default is the committer identity. --no-gpg-sign overrides commit.gpgSign for that commit. The same -S flag exists on git merge and git tag -s signs a tag. gpg.format can be ssh, in which case user.signingKey is a path to an SSH key. The commit manual notes that signing during rebase can sign a large number of new commits.

**Examples:**

```bash
git config --global user.signingKey <key-id>
git config --global commit.gpgSign true
git commit -S -m "Sign this commit"
```

**Sources:**

- [git-commit](https://git-scm.com/docs/git-commit)
- [git-config](https://git-scm.com/docs/git-config)
- [git-tag](https://git-scm.com/docs/git-tag)
</details>

<details>
<summary>What are some common security practices for Git repositories?</summary>

### What are some common security practices for Git repositories?

**Definition:**

Sign commits and tags, reject non-fast-forward updates on a shared repository, and do not commit secrets. Git identifies objects by hash; the signature is what ties a commit or tag to a key.

**How it works:**

git commit -S and commit.gpgSign sign commits. git tag -s signs a tag, and git tag -v checks it. git push --signed signs the push request so the receiving hooks can check it. The update hook can refuse a ref update that is not a descendant of the old commit, which is a fast-forward-only policy. git-shell can limit an SSH login to Git commands. Credentials for HTTP belong in a credential helper, not in the repository.

**Examples:**

```bash
git commit -S -m "Signed change"
git tag -s v1.2.0 -m "Release 1.2.0"
git tag -v v1.2.0
```

**Sources:**

- [git-commit](https://git-scm.com/docs/git-commit)
- [git-tag](https://git-scm.com/docs/git-tag)
- [git-push](https://git-scm.com/docs/git-push)
- [githooks](https://git-scm.com/docs/githooks)
</details>

<details>
<summary>How do you encrypt sensitive data in a Git repository?</summary>

### How do you encrypt sensitive data in a Git repository?

**Definition:**

Git does not encrypt blob contents. A commit, tree, or blob is stored by its object id. Signing a commit proves who signed it; it does not hide the file.

**How it works:**

Do not add secrets in the first place. A gitignore pattern keeps untracked secret files out of git add. git rm --cached removes a path from the index and leaves the worktree file, which is how you stop tracking a file that should have been ignored. Passwords for a remote belong in a credential helper (git credentials), not in a tracked file. If a secret is already in history, removing it is a history rewrite, and git filter-branch's manual says that command is not recommended.

**Examples:**

```bash
printf '%s\n' '.env' >> .gitignore
git rm --cached .env
```

**Sources:**

- [gitignore](https://git-scm.com/docs/gitignore)
- [git-rm](https://git-scm.com/docs/git-rm)
- [gitcredentials](https://git-scm.com/docs/gitcredentials)
- [git-commit](https://git-scm.com/docs/git-commit)
</details>

<details>
<summary>What are some strategies for credential storage in Git?</summary>

### What are some strategies for credential storage in Git?

**Definition:**

Git asks a credential helper for a username and password, or for a personal access token used as the password, when an HTTP remote needs them.

**How it works:**

Without a helper, Git can run the program in GIT_ASKPASS, then SSH_ASKPASS, then a prompt. git config --global credential.helper <name> selects a helper. Helpers shipped with Git include cache, which keeps credentials in memory for a short time, and store, which writes them to disk indefinitely. The credentials manual also names git-credential-osxkeychain, git-credential-libsecret, git-credential-wincred, and Git Credential Manager. credential.interactive false stops helpers from prompting.

**When to use:**

Prefer the operating-system helper named in the credentials manual. Use store only when a disk file is acceptable. cache forgets the secret when the process ends.

**Examples:**

```bash
git config --global credential.helper cache
git config --global credential.helper store
```

**Sources:**

- [gitcredentials](https://git-scm.com/docs/gitcredentials)
- [git-config](https://git-scm.com/docs/git-config)
</details>

<details>
<summary>How would you revoke access to a Git repository?</summary>

### How would you revoke access to a Git repository?

**Definition:**

Git has no user account inside the object database. Access is revoked on the remote that accepts the push, and stored credentials are erased with git credential reject.

**How it works:**

The update hook can refuse a ref update, and git-shell can limit a login to Git commands. That is repository-side policy, not a list of accounts in the commits. git credential reject tells every configured helper to erase credentials that match the description on stdin. Rotating or deleting the token at the host is what stops a new authentication. Deleting a local clone does not remove someone else's clone.

**Examples:**

```bash
printf '%s\n' protocol=https host=example.com path=foo.git '' | git credential reject
```

**Sources:**

- [git-credential](https://git-scm.com/docs/git-credential)
- [githooks](https://git-scm.com/docs/githooks)
- [gitcredentials](https://git-scm.com/docs/gitcredentials)
</details>


### Troubleshooting and Recovery

<details>
<summary>How do you find and restore a deleted file in a Git repository?</summary>

### How do you find and restore a deleted file in a Git repository?

**Definition:**

A deleted path is still in an older commit if that commit's tree contains it. git restore --source=<tree> writes that blob back into the working tree.

**How it works:**

git log -- path, or git log -p -- path, lists commits that touched the path. git show <commit>:path prints the blob from that commit. git restore --source=<commit> -- <path> restores the working tree from that tree. Add --staged as well when the index should match. This uses the copy still stored in history. It does not recreate a file that was never committed.

**Examples:**

```bash
git log -- path/to/file
git restore --source=<commit> -- path/to/file
```

**Sources:**

- [git-log](https://git-scm.com/docs/git-log)
- [git-restore](https://git-scm.com/docs/git-restore)
- [git-show](https://git-scm.com/docs/git-show)
</details>

<details>
<summary>What steps would you take if you accidentally committed sensitive data?</summary>

### What steps would you take if you accidentally committed sensitive data?

**Definition:**

Treat the data as exposed, stop publishing it, and rewrite history only with a tool the Git manual still recommends. git filter-branch is documented and marked not recommended.

**How it works:**

Rotate the password, token, or key at the place that issued it. Commits that are already pushed are still in every clone that fetched them. The filter-branch manual says the command has safety and performance problems that cannot be fixed compatibly, and it tells you to use git-filter-repo instead. The same manual shows the form it does not want you to rely on: git filter-branch --index-filter 'git rm --cached --ignore-unmatch filename' HEAD. After a rewrite, the old objects remain until they are unreachable and git gc prunes them. Pushing the rewritten branch is a non-fast-forward update.

**When to use:**

Untracking the file in a new commit stops future snapshots from including it. It does not remove the blob from commits that already contain it.

**Examples:**

```bash
git rm --cached --ignore-unmatch secret.txt
git commit -m "Stop tracking secret.txt"
```

**Sources:**

- [git-filter-branch](https://git-scm.com/docs/git-filter-branch)
- [git-rm](https://git-scm.com/docs/git-rm)
- [git-gc](https://git-scm.com/docs/git-gc)
</details>

<details>
<summary>How do you handle detached HEAD state in Git?</summary>

### How do you handle detached HEAD state in Git?

**Definition:**

Detached HEAD means HEAD points at a commit, not at a branch name. New commits are kept only by HEAD until you create a branch or a tag.

**How it works:**

git checkout of a tag or a raw commit, and git switch --detach, enter this state. The checkout manual shows that switching back to master leaves those new commits with nothing pointing at them, so later git gc can delete them. While you are still on that commit, git switch -c <name> creates a branch there and attaches HEAD to it. git branch <name> creates the branch and leaves HEAD detached. git tag <name> creates a tag and also leaves HEAD detached.

**When to use:**

Stay detached to inspect a tag. Create a branch before you leave if the new commits should survive.

**Examples:**

```bash
git switch --detach <commit>
git switch -c keep-this
```

**Sources:**

- [git-checkout](https://git-scm.com/docs/git-checkout)
- [git-switch](https://git-scm.com/docs/git-switch)
- [git-gc](https://git-scm.com/docs/git-gc)
</details>

<details>
<summary>Explain how to recover a dropped stash in Git.</summary>

### Explain how to recover a dropped stash in Git.

**Definition:**

A dropped stash is still a commit until it becomes unreachable and git gc prunes it. The stash manual finds those commits with git fsck and git log.

**How it works:**

Stash entries are merge commits. The documented search prints unreachable commits whose message matches WIP, which is how stash labels them. git show <commit> displays one you found. git stash store <commit> puts a stash commit created by git stash create back on the stash reflog. The manual says store is meant for scripts and is probably not the command you want for ordinary use; the fsck search is the documented way to find a cleared stash.

**Examples:**

```bash
git fsck --unreachable |
grep commit | cut -d' ' -f3 |
xargs git log --merges --no-walk --grep=WIP
```

**Sources:**

- [git-stash](https://git-scm.com/docs/git-stash)
- [git-fsck](https://git-scm.com/docs/git-fsck)
</details>

<details>
<summary>How do you amend the message of the last commit?</summary>

### How do you amend the message of the last commit?

**Definition:**

--amend replaces the tip commit with a new commit. The previous message is the starting text when you do not pass -m or -F.

**How it works:**

The new commit has the same parents and author unless --reset-author is given. The tree is prepared as usual, so staged changes are included. git commit --amend --no-edit keeps the message and still replaces the tip, which is how you add staged files without editing the text. It is a rough equivalent of git reset --soft HEAD^ followed by git commit -c ORIG_HEAD, and it can amend a merge commit. The manual says to understand the implications if the commit was already published; see recovering from upstream rebase.

**When to use:**

Amend a commit you have not pushed. If it is already on the remote, prefer a new commit.

**Examples:**

```bash
git commit --amend
git commit --amend -m "Corrected message"
```

**Sources:**

- [git-commit](https://git-scm.com/docs/git-commit)
- [git-rebase](https://git-scm.com/docs/git-rebase)
</details>


### Git Collaboration

<details>
<summary>What is a pull request and how does it work?</summary>

### What is a pull request and how does it work?

**Definition:**

Git's pull request is the text from git request-pull. It asks an upstream repository to fetch your commits and integrate them. A review page on a Git host is a separate product built on push and fetch.

**How it works:**

git request-pull <start> <URL> [<end>] expects <start> to already be in the upstream history. The message summarizes commits from <start> to <end>, and <end> defaults to HEAD. -p includes the patch text. The other person fetches that URL and merges or pulls. giteveryday also lists git format-patch and git send-email when the exchange is email instead of a fetch.

**Examples:**

```bash
git request-pull <start> <URL>
git request-pull -p origin/main <URL>
```

**Sources:**

- [git-request-pull](https://git-scm.com/docs/git-request-pull)
- [giteveryday](https://git-scm.com/docs/giteveryday)
</details>

<details>
<summary>How do you update a pull request?</summary>

### How do you update a pull request?

**Definition:**

Add commits to the branch you asked the other side to pull, and push that branch. git request-pull prints a new summary from the same start commit to the new HEAD.

**How it works:**

The upstream is expected to fetch your published URL. A new commit on that branch moves the end of the range. Rewriting commits that the other side already fetched is the case the rebase manual tells you to recover from, so prefer another commit on the branch. git push updates the remote ref. If the remote rejects a non-fast-forward, you have rewritten history.

**Examples:**

```bash
git commit -m "Address review"
git push origin feature/login
git request-pull origin/main <URL>
```

**Sources:**

- [git-request-pull](https://git-scm.com/docs/git-request-pull)
- [git-push](https://git-scm.com/docs/git-push)
- [git-commit](https://git-scm.com/docs/git-commit)
</details>

<details>
<summary>What is a code review process in Git?</summary>

### What is a code review process in Git?

**Definition:**

Git's review flow is small commits that a reviewer can read, then a merge. The project manual says separate changes so blame and bisect stay useful.

**How it works:**

gitworkflows says each commit should work on its own and pass the tests, and that it is easier to squash later than to split a huge commit. git log -p and git show are how you read the change. git request-pull or git format-patch hands the series to someone else. The integrator merges. There is no git review subcommand.

**Examples:**

```bash
git log -p main..feature/login
git request-pull main <URL>
git merge feature/login
```

**Sources:**

- [gitworkflows](https://git-scm.com/docs/gitworkflows)
- [git-log](https://git-scm.com/docs/git-log)
- [git-request-pull](https://git-scm.com/docs/git-request-pull)
</details>

<details>
<summary>How do you handle a pull request conflict?</summary>

### How do you handle a pull request conflict?

**Definition:**

Fetch the branch you were asked to pull and merge it. Resolve the conflict the same way as any other merge, then commit.

**How it works:**

git fetch <URL> <branch> updates FETCH_HEAD. git merge FETCH_HEAD integrates it. Conflict markers, git add, and git merge --continue are the merge manual's steps. git merge --abort abandons that merge. If you are the author, merge the upstream branch into your feature branch, fix the conflict there, and push the result so the other side can fetch a branch that already contains the resolution.

**Examples:**

```bash
git fetch <URL> feature/login
git merge FETCH_HEAD
git add path/to/file
git merge --continue
```

**Sources:**

- [git-fetch](https://git-scm.com/docs/git-fetch)
- [git-merge](https://git-scm.com/docs/git-merge)
- [git-pull](https://git-scm.com/docs/git-pull)
</details>

<details>
<summary>What is the difference between git pull and git fetch followed by git merge?</summary>

### What is the difference between git pull and git fetch followed by git merge?

**Definition:**

git pull is git fetch plus an integration step. git fetch followed by git merge is that integration when you choose merge yourself.

**How it works:**

Pull decides the upstream when you pass no refspec, then integrates. With no rebase or merge choice, the default is --ff-only, which refuses a diverged branch. git pull --no-rebase is the form that runs git merge. A separate git fetch updates remote-tracking branches and FETCH_HEAD and leaves your branch where it is, so you can git log or git diff before git merge. git pull --rebase runs rebase instead of merge, which a later manual merge does not do.

**When to use:**

Fetch and then merge when you want to inspect the incoming commits. Pull when the integration should follow immediately and you accept its default of fast-forward only.

**Examples:**

```bash
git fetch origin
git merge origin/main
git pull --no-rebase origin main
git pull --ff-only
```

**Sources:**

- [git-pull](https://git-scm.com/docs/git-pull)
- [git-fetch](https://git-scm.com/docs/git-fetch)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>


### Git Performance and Scalability

<details>
<summary>How do you handle large files in Git?</summary>

### How do you handle large files in Git?

**Definition:**

Git stores every committed version of a file as a blob. A large file that changes often makes every clone download those blobs. Prefer not committing them, or clone so the blobs stay on the server until you need them.

**How it works:**

A gitignore pattern leaves the file untracked. git clone --filter=blob:none asks the server for a partial clone and omits blob contents until Git needs them. --filter=blob:limit=<size> omits blobs at least that size. git clone --sparse checks out only the top-level files; git sparse-checkout grows the working tree. These options do not shrink the copy that already exists on the server.

**When to use:**

Ignore generated binaries. Use a partial clone when the history must stay in Git but the working copy should not download every blob up front.

**Examples:**

```bash
git clone --filter=blob:none <repository>
git clone --sparse <repository>
```

**Sources:**

- [git-clone](https://git-scm.com/docs/git-clone)
- [gitignore](https://git-scm.com/docs/gitignore)
</details>

<details>
<summary>What are some ways to improve the performance of a large Git repository?</summary>

### What are some ways to improve the performance of a large Git repository?

**Definition:**

git gc packs objects and drops unreachable ones. A partial or shallow clone downloads less. Sparse checkout writes fewer files into the working tree.

**How it works:**

Porcelain commands that create objects run git gc automatically when the repository has grown enough since the last maintenance. gc.auto controls that. git gc compresses revisions, prunes unreachable objects, packs refs, and may update the commit-graph. git clone --filter=blob:none delays blobs. git clone --depth=<n> truncates history. checkout.workers and checkout.thresholdForParallelism tune parallel checkout; the switch manual notes that parallel checkout usually helps on SSD or NFS and that the default sequential checkout is often better on spinning disks.

**Examples:**

```bash
git gc
git clone --filter=blob:none <repository>
git clone --depth=1 <repository>
```

**Sources:**

- [git-gc](https://git-scm.com/docs/git-gc)
- [git-clone](https://git-scm.com/docs/git-clone)
- [git-switch](https://git-scm.com/docs/git-switch)
</details>

<details>
<summary>What is a shallow clone in Git and when would you use it?</summary>

### What is a shallow clone in Git and when would you use it?

**Definition:**

A shallow clone is a clone whose history is truncated. git clone --depth=<depth> keeps that many commits from the tip of each fetched branch.

**How it works:**

--depth implies --single-branch unless --no-single-branch is given. --shallow-since=<date> keeps history after a time. --shallow-exclude=<ref> drops commits reachable from that ref. Tags of the deepened commits are not fetched when you later git fetch --depth. git fetch --unshallow converts a shallow repository to a complete one when the source is complete. --reject-shallow makes a clone fail if the source itself is shallow.

**When to use:**

Use a shallow clone for a build that only needs recent history. Use a full clone when blame, bisect, or merging older history matters.

**Examples:**

```bash
git clone --depth=1 <repository>
git fetch --unshallow
```

**Sources:**

- [git-clone](https://git-scm.com/docs/git-clone)
- [git-fetch](https://git-scm.com/docs/git-fetch)
</details>

<details>
<summary>How do you reduce the repository size in Git?</summary>

### How do you reduce the repository size in Git?

**Definition:**

git gc compresses revisions and removes unreachable objects. A new clone with --depth or --filter downloads a smaller copy. Rewriting history to drop a big blob is the case filter-branch warns you about.

**How it works:**

Unreachable objects from expired reflogs and dropped commits go away when gc prunes them. Objects still reachable from a branch or tag stay. git clone --depth=1 or --filter=blob:none is how a consumer gets less data without changing the source. The filter-branch manual says not to use that command to rewrite trees and points at git-filter-repo. After any rewrite, git gc is what actually deletes objects nothing references.

**Examples:**

```bash
git gc
git clone --depth=1 <repository>
```

**Sources:**

- [git-gc](https://git-scm.com/docs/git-gc)
- [git-clone](https://git-scm.com/docs/git-clone)
- [git-filter-branch](https://git-scm.com/docs/git-filter-branch)
</details>

<details>
<summary>Explain how Git LFS (Large File Storage) is used.</summary>

### Explain how Git LFS (Large File Storage) is used.

**Definition:**

Git LFS is not a built-in Git command. It tracks patterns by writing .gitattributes entries, and those attributes use the clean and smudge filter mechanism Git documents for path attributes.

**How it works:**

git lfs track <pattern> writes the pattern to .gitattributes. With no pattern it lists tracked paths. The LFS manual says the patterns follow gitignore pattern rules, and --filename treats the argument as a literal name. git lfs track "*.gif" is the documented example for GIF files. A filter driver in gitattributes is a clean command on git add and a smudge command on checkout. LFS uses that hook so the repository stores a pointer and the file contents live in LFS storage.

**When to use:**

Use LFS for large binaries that must be versioned. Keep generated files in gitignore when they should not be versioned at all.

**Examples:**

```bash
git lfs track "*.gif"
git lfs track
```

**Sources:**

- [gitattributes](https://git-scm.com/docs/gitattributes)
- [gitignore](https://git-scm.com/docs/gitignore)
</details>


### Git in Continuous Integration/Continuous Deployment (CI/CD)

<details>
<summary>How does Git integrate with CI/CD pipelines?</summary>

### How does Git integrate with CI/CD pipelines?

**Definition:**

A pipeline watches refs that fetch and push already move. Hooks are the integration Git itself runs: locally before a commit or push, and on the server when a push is received.

**How it works:**

pre-commit can refuse a commit. pre-push can refuse to send objects. On the remote, pre-receive can reject the whole update, and update can reject one ref, including a non-fast-forward. post-receive runs after the refs are updated and can start other work; it cannot change whether the push succeeded. git push --signed lets the receive hooks check a signature on the push. CI products poll or receive these same refs. Git does not define the pipeline file.

**Examples:**

```bash
git push origin main
git push --signed origin main
```

**Sources:**

- [githooks](https://git-scm.com/docs/githooks)
- [git-push](https://git-scm.com/docs/git-push)
</details>

<details>
<summary>Describe the process of automating deployments using Git.</summary>

### Describe the process of automating deployments using Git.

**Definition:**

Put a post-receive hook on the repository that receives the push. It runs after the refs are updated and can start a checkout or a deploy script.

**How it works:**

The hook reads one line per updated ref: <old-oid> <new-oid> <ref-name>. It does not receive arguments. Its stdout and stderr are forwarded to the client. A sample post-receive-email script ships in Git's contrib/hooks and only sends mail; a deploy hook is the same input with your own command. pre-receive can still reject the push before any deploy runs. Push options are available to the hook when the client used git push -o.

**Examples:**

```bash
git push origin main
```

**Sources:**

- [githooks](https://git-scm.com/docs/githooks)
- [git-push](https://git-scm.com/docs/git-push)
</details>

<details>
<summary>How would you configure branch protection rules for a repository in the context of CI/CD?</summary>

### How would you configure branch protection rules for a repository in the context of CI/CD?

**Definition:**

Git does not store branch protection rules. The server-side update hook can refuse a push, which is how a repository enforces fast-forward only.

**How it works:**

update runs once per ref with the ref name, the old object name, and the new object name. A non-zero exit blocks that ref. The hook documentation's example is to require that the new commit is a descendant of the old one. pre-receive can reject every ref in the push before any of them move. A hosting service's protection settings are outside these hooks. git push --signed gives the hook a signature it can check.

**When to use:**

Use the update hook when you control the receiving repository. Host protection screens are a separate product in front of the same push.

**Examples:**

```bash
git push origin main
```

**Sources:**

- [githooks](https://git-scm.com/docs/githooks)
- [git-push](https://git-scm.com/docs/git-push)
</details>

<details>
<summary>What role do Git hooks play in automation?</summary>

### What role do Git hooks play in automation?

**Definition:**

Hooks run your script at a Git event. Some can reject the event. Others only observe it.

**How it works:**

pre-commit aborts the commit on a non-zero status and can be skipped with --no-verify. pre-push aborts the push. pre-receive and update abort a receive. post-receive and post-commit cannot change the outcome; they run after the write. core.hooksPath points every hook lookup at one directory, which is how a project shares hook scripts without copying them into each .git/hooks.

**Examples:**

```bash
git config core.hooksPath .githooks
git commit --no-verify -m "Skip pre-commit"
```

**Sources:**

- [githooks](https://git-scm.com/docs/githooks)
- [git-config](https://git-scm.com/docs/git-config)
- [git-commit](https://git-scm.com/docs/git-commit)
</details>

<details>
<summary>How can Git help in the rollback process of a failed deployment?</summary>

### How can Git help in the rollback process of a failed deployment?

**Definition:**

Record the undo as a new commit with git revert, or deploy a commit that an annotated tag already names. Both leave the bad commit in history.

**How it works:**

git revert <commit> needs a clean working tree and writes a commit that applies the reverse patch. Push that commit so the deployed branch moves forward to the undo. git checkout <tag> or git switch --detach <tag> inspects the previous release without moving the branch. Resetting a published branch backwards is a history rewrite; the commit manual points that case at the rebase recovery section.

**When to use:**

Revert the deployed branch when the release ref must move forward. Check out the tag when you only need to build that older commit.

**Examples:**

```bash
git revert <commit>
git push origin main
git switch --detach v1.2.0
```

**Sources:**

- [git-revert](https://git-scm.com/docs/git-revert)
- [git-tag](https://git-scm.com/docs/git-tag)
- [git-switch](https://git-scm.com/docs/git-switch)
</details>


### Git with IDEs and Tools

<details>
<summary>How do you integrate Git with your preferred IDE?</summary>

### How do you integrate Git with your preferred IDE?

**Definition:**

An IDE runs the same Git commands against the repository. Integration means the editor calls git status, git commit, git switch, and git merge for you.

**How it works:**

There is no git ide subcommand. Whatever the editor shows as a branch is HEAD, and a commit it creates is still a commit object with the user.name and user.email from config. Conflict markers in the editor are the markers git merge wrote. git mergetool -t <tool> is the documented way to hand a conflict to an external program such as vimdiff or meld.

**Examples:**

```bash
git status
git commit -m "Update from the editor"
git mergetool -t vimdiff
```

**Sources:**

- [git-commit](https://git-scm.com/docs/git-commit)
- [git-mergetool](https://git-scm.com/docs/git-mergetool)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>Discuss the pros and cons of using a GUI tool for Git versus the command line.</summary>

### Discuss the pros and cons of using a GUI tool for Git versus the command line.

**Definition:**

A graphical client and the git command operate on the same repository. The command line is the interface the manuals specify. A GUI is another program that invokes those commands or updates the same refs.

**How it works:**

git status, git log -p, git merge, and git commit behave as documented whether or not a window drew the button. Detached HEAD, a conflicted index, and a rejected non-fast-forward push are repository states, not interface states. git mergetool is the manual's bridge to a visual merge program.

**When to use:**

- The command line matches the manual, including flags such as --ff-only, -S, and --prune.
- A GUI can show git log and git diff without you typing the revision range.
- A GUI that hides the ref it will update can fast-forward or rewind a branch you did not mean to move.
- Scripts and hooks call the git command, so the command line remains the form you can reproduce.

**Examples:**

```bash
git status
git log -p
git mergetool
```

**Sources:**

- [giteveryday](https://git-scm.com/docs/giteveryday)
- [git-mergetool](https://git-scm.com/docs/git-mergetool)
</details>

<details>
<summary>What plugins are commonly used with Git in development environments?</summary>

### What plugins are commonly used with Git in development environments?

**Definition:**

Git's extension points in the manuals are hooks, credential helpers, merge tools, aliases, and remote helpers such as git svn. A marketplace plugin is not part of Git.

**How it works:**

core.hooksPath loads project scripts. credential.helper names cache, store, or an OS helper from the credentials manual. git mergetool --tool-help lists merge tools, including vimdiff, meld, and kdiff3. alias.* shortens a command. git svn is the documented bridge that fetches and pushes changesets between Subversion and Git.

**Examples:**

```bash
git config --global credential.helper cache
git mergetool --tool-help
git config --global alias.st status
```

**Sources:**

- [githooks](https://git-scm.com/docs/githooks)
- [gitcredentials](https://git-scm.com/docs/gitcredentials)
- [git-mergetool](https://git-scm.com/docs/git-mergetool)
- [git-svn](https://git-scm.com/docs/git-svn)
</details>

<details>
<summary>How do you resolve merge conflicts using a visual tool?</summary>

### How do you resolve merge conflicts using a visual tool?

**Definition:**

git mergetool runs a merge program on files that still have conflicts, after git merge has stopped.

**How it works:**

With no path it visits every unmerged file. -t / --tool selects the program. Documented values include emerge, gvimdiff, kdiff3, meld, vimdiff, and tortoisemerge. If you omit the tool, mergetool uses merge.tool, or picks a default. The program receives temporary files in BASE, LOCAL, and REMOTE, and writes the result to MERGED. After the tool exits, Git asks you to confirm unless mergetool.<tool>.trustExitCode is true. Then git add and git merge --continue finish the merge as usual.

**Examples:**

```bash
git merge topic
git mergetool -t meld
git merge --continue
```

**Sources:**

- [git-mergetool](https://git-scm.com/docs/git-mergetool)
- [git-merge](https://git-scm.com/docs/git-merge)
</details>

<details>
<summary>What is the purpose of a Git bridge for version control systems?</summary>

### What is the purpose of a Git bridge for version control systems?

**Definition:**

A bridge moves changes between Git and another system. git svn is the bridge Git ships for Subversion.

**How it works:**

git svn provides a bidirectional flow of changesets. It can follow a trunk/branches/tags layout with --stdlayout, or a custom layout with -T, -t, and -b. After the tracking repository exists, git svn fetch updates the Git repository from Subversion. The command is a conduit for changesets, not a second object database format inside Git.

**Examples:**

```bash
git svn clone --stdlayout <svn-url>
git svn fetch
```

**Sources:**

- [git-svn](https://git-scm.com/docs/git-svn)
</details>


### Advanced Git Topics

<details>
<summary>How do you handle large binary files in Git without using Git LFS?</summary>

### How do you handle large binary files in Git without using Git LFS?

**Definition:**

Leave them untracked, or accept that every committed version is a full blob. Partial clone downloads those blobs only when a command needs them.

**How it works:**

gitignore keeps an untracked binary out of commits. Tracked binaries stay in history; git rm --cached stops the next commit from recording the path and leaves the worktree file. git clone --filter=blob:limit=<size> omits blobs at least that size until they are needed. --filter=blob:none omits all blob contents the same way. A clean/smudge filter in .gitattributes can transform file contents on the way into and out of the object database; that is the mechanism LFS uses, and you can configure a filter driver without LFS. It does not make historical blobs smaller.

**Examples:**

```bash
printf '%s\n' '*.zip' >> .gitignore
git clone --filter=blob:limit=1m <repository>
```

**Sources:**

- [gitignore](https://git-scm.com/docs/gitignore)
- [git-clone](https://git-scm.com/docs/git-clone)
- [gitattributes](https://git-scm.com/docs/gitattributes)
- [git-rm](https://git-scm.com/docs/git-rm)
</details>

<details>
<summary>What is the purpose of the git rebase -i command?</summary>

### What is the purpose of the git rebase -i command?

**Definition:**

git rebase -i edits the commits as they are replayed. You can reorder them, drop them, reword them, or fold them together.

**How it works:**

The todo list is opened in an editor. Each line starts as pick plus the commit name. The manual says not to edit those names. edit stops after that commit so you can amend it, then git rebase --continue. reword edits only the message. drop, or deleting the line, removes the commit. squash and fixup fold a commit into the one above it. break stops without applying a commit. git rebase -i HEAD~5 is the manual's example for reordering the last five commits. This rewrites the commits.

**When to use:**

Use it on commits that are not yet pushed. Published commits should stay put; add a new commit or revert instead.

**Examples:**

```bash
git rebase -i HEAD~5
git rebase --continue
git rebase --abort
```

**Sources:**

- [git-rebase](https://git-scm.com/docs/git-rebase)
</details>

<details>
<summary>How would you set up a continuous backup process for Git repositories?</summary>

### How would you set up a continuous backup process for Git repositories?

**Definition:**

git bundle create writes a file that contains objects and refs. The manual's full-backup example is git bundle create backup.bundle --all. A mirror clone is another copy that fetches every ref.

**How it works:**

Anyone who can git clone can read a bundle: git clone backup.bundle restores a repository from that file. git fetch can also read a bundle. There is no git push into a bundle. Repeating git bundle create backup.bundle --all replaces the archive with the current refs. git clone --mirror <url> creates a bare clone that maps every ref, which is the form used to keep a second complete copy. git gc on that copy packs it.

**When to use:**

Bundle when you need a file you can copy offline. A mirror clone when another machine should keep fetching the same refs.

**Examples:**

```bash
git bundle create backup.bundle --all
git clone --mirror <repository> backup.git
```

**Sources:**

- [git-bundle](https://git-scm.com/docs/git-bundle)
- [git-clone](https://git-scm.com/docs/git-clone)
</details>

<details>
<summary>Explain the impact of case sensitivity in Git and how to manage it across different operating systems.</summary>

### Explain the impact of case sensitivity in Git and how to manage it across different operating systems.

**Definition:**

core.ignoreCase tells Git to treat names that differ only by case as the same path on filesystems that do that, such as APFS, HFS+, FAT, and NTFS.

**How it works:**

The default is false, but git clone and git init probe the filesystem and set core.ignoreCase true when that is appropriate. If a directory listing returns makefile when Git expects Makefile, Git keeps the name it already stored. The manual says Git relies on this variable matching the operating system and that changing it may have unexpected results. core.precomposeUnicode is the related macOS setting: when true, Git undoes the Unicode decomposition macOS applies to filenames, which matters when the same repository is used on macOS and on Linux or Windows.

**When to use:**

Leave the value that clone or init probed. Do not flip core.ignoreCase to paper over two tracked names that differ only by case.

**Examples:**

```bash
git config core.ignoreCase
git config core.precomposeUnicode true
```

**Sources:**

- [git-config](https://git-scm.com/docs/git-config)
</details>

<details>
<summary>How do you maintain multiple versions of a product using Git branches for each version?</summary>

### How do you maintain multiple versions of a product using Git branches for each version?

**Definition:**

Each supported version is a branch. Fixes land on the oldest branch that needs them and are merged upward. A release point on that line is a tag.

**How it works:**

git switch -c maint-1.2 v1.2.0 starts a branch at the tagged commit. git merge brings a fix branch into that maintenance branch. gitworkflows prefers merge over cherry-pick because merge carries the whole branch, and it still names cherry-pick as the occasional commit-level tool. git tag -a names the release you ship from that branch. The branch does not move when you only create a tag.

**Examples:**

```bash
git switch -c maint-1.2 v1.2.0
git merge fix-1.2
git tag -a v1.2.1 -m "Release 1.2.1"
git switch main
git merge maint-1.2
```

**Sources:**

- [git-switch](https://git-scm.com/docs/git-switch)
- [git-merge](https://git-scm.com/docs/git-merge)
- [git-tag](https://git-scm.com/docs/git-tag)
- [gitworkflows](https://git-scm.com/docs/gitworkflows)
</details>
