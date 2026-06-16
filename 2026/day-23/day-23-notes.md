# Day 23 – Git Branching & Working with GitHub

## Task 1: Understanding Branches

### What is a branch in Git? 

A Git branch is a separate line of development that allows developers to work on new features, bug fixes, or experiments independently from the main branch.

### Why do we use branches instead of committing everything to main?

### Why do we use branches instead of committing everything to main?

Branches are used for separate development and isolated work. Committing everything directly to the main branch can introduce bugs or unstable changes that may affect the application. Branches help keep the main branch stable and safe.

### What is HEAD in Git?

HEAD is a reference to the latest commit on the currently active branch.

Example:

```text
main
A ← B ← C (HEAD)
```

In this example, `HEAD` points to commit `C`, which is the latest commit on the `main` branch.


### What happens to your files when you switch branches?

When you switch branches, Git changes your working directory to match the selected branch. Uncommitted changes may be carried over or Git may block the switch if conflicts exist. It is a good practice to commit or stash changes before switching branches.

## Task 2: Branching Commands — Hands-On

### To list all local branches:

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch -r
  origin/master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch -a
* master
  remotes/origin/master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
| Command | Description |
|----------|-------------|
| `git branch` | List all local branches |
| `git branch -r` | List all remote branches |
| `git branch -a` | List all local and remote branches |

### Create and Switch to a New Branch
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
* feature-1
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```

For a command cheat sheet table:

```md
| Command | Description |
|----------|-------------|
| `git checkout -b feature-branch` | Creates and switches to a new branch |
| `git switch -c feature-branch` | Creates and switches to a new branch |
```
### Created a new branch and switched to it using a single command.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout -b feature-2
Switched to a new branch 'feature-2'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
* feature-2
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$
```

### How is `git switch` different from `git checkout`?

`git switch` is a newer command introduced to make branch switching simpler and safer. It is specifically used for switching between branches.

`git checkout` is an older command that has multiple purposes, such as switching branches, restoring files, and checking out specific commits.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch feature-1
Switched to branch 'feature-1'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
* feature-1
  feature-2
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
### Made a commit on feature-1 that does not exist on the main branch.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Added test.py for Testing purpose"
[feature-1 581e274] Added test.py for Testing purpose
 1 file changed, 1 insertion(+)
 create mode 100644 test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log
commit 581e27456080651be55ee12ffb399946264815c2 (HEAD -> feature-1)
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Tue Jun 16 22:55:50 2026 +0530

    Added test.py for Testing purpose

commit 4b2bab664a5b8ba71a55feea305fc5e6acce6984 (origin/master, master, feature-2)
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Tue Jun 16 00:16:39 2026 +0530

    Added more Git commands to git-commands.md and updated the cheat sheet.

commit 1ef76a7a4fcf5e575b67ed4329e58c2ed749f56d
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:59:38 2026 +0530

    - Updated  with additional Git commands and details.

commit 0f0a98c834d6fa34bb4da428520bf4c0bcb4d796
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:37:12 2026 +0530

    - Updated  with additional Git commands.

commit aa82211b8adab3e2cfa1c691add9b537bb400540
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:04:20 2026 +0530

    Added git-commands.md
```

### Switched back to the main branch and verified that the commit from feature-1 is not present.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
* feature-1
  feature-2
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch maaster
fatal: invalid reference: maaster
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log
commit 4b2bab664a5b8ba71a55feea305fc5e6acce6984 (HEAD -> master, origin/master, feature-2)
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Tue Jun 16 00:16:39 2026 +0530

    Added more Git commands to git-commands.md and updated the cheat sheet.

commit 1ef76a7a4fcf5e575b67ed4329e58c2ed749f56d
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:59:38 2026 +0530

    - Updated  with additional Git commands and details.

commit 0f0a98c834d6fa34bb4da428520bf4c0bcb4d796
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:37:12 2026 +0530

    - Updated  with additional Git commands.

commit aa82211b8adab3e2cfa1c691add9b537bb400540
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:04:20 2026 +0530

    Added git-commands.md
```

### Deleted the feature-2 branch because it was no longer needed.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-2
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch -d feature-2
Deleted branch feature-2 (was 4b2bab6).
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```

### Added all branching commands to git-commands.md

- [Git Commands Cheat Sheet](https://github.com/sk7652183-rgb/git-commands)

## Task 3: Push to GitHub

Created a new GitHub repository, connected the local `devops-git-practice` repository to the remote, pushed the `master` and `feature-1` branches, and verified that both branches were visible on GitHub.

<img width="1859" height="998" alt="image" src="https://github.com/user-attachments/assets/779a8620-8a4a-4fc7-acab-b026d183a5dd" />
<img width="1859" height="998" alt="image" src="https://github.com/user-attachments/assets/db3368e7-3ee8-4e01-a705-61193fed1221" />

## Answer in your notes: What is the difference between origin and upstream?

**Origin** is the remote repository that you own and push changes to, while **upstream** is the original repository from which your repository was forked or cloned and from which you receive updates.

## Task 4: Pull from GitHub

### Made a change to a file directly on GitHub using the GitHub editor and pulled the change to the local repository.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
* feature-1
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git fetch --all
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 1004 bytes | 251.00 KiB/s, done.
From github.com:sk7652183-rgb/devops-git-practice
   581e274..62f84ef  feature-1  -> origin/feature-1
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md  test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ cat test.py
# This is the python file which is use for Testing purpose 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch
fatal: missing branch or commit argument
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch feature-1
Switched to branch 'feature-1'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git pull origin feature-1
From github.com:sk7652183-rgb/devops-git-practice
 * branch            feature-1  -> FETCH_HEAD
Updating 581e274..62f84ef
Fast-forward
 test.py | 1 +
 1 file changed, 1 insertion(+)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ cat test.py
# This is the python file which is use for Testing purpose 
# This is for the teating 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
### Answer in your notes: What is the difference between git fetch and git pull?

**git fetch** downloads the latest changes from the remote repository without modifying your local branch, while **git pull** downloads the changes and automatically merges them into your current branch.


## Task 5: Clone vs Fork

- Forked a public GitHub repository and cloned it to the local machine for local development.

```bash
sufiyan@sufiyan-HP-Notebook:~/k8s$ git clone git@github.com:github/gh-aw-mcpg.git
Cloning into 'gh-aw-mcpg'...
remote: Enumerating objects: 27171, done.
remote: Counting objects: 100% (386/386), done.
remote: Compressing objects: 100% (174/174), done.
remote: Total 27171 (delta 304), reused 224 (delta 211), pack-reused 26785 (from 3)
Receiving objects: 100% (27171/27171), 27.08 MiB | 2.36 MiB/s, done.
Resolving deltas: 100% (18261/18261), done.
sufiyan@sufiyan-HP-Notebook:~/k8s$ ls
gh-aw-mcpg
sufiyan@sufiyan-HP-Notebook:~/k8s$ 
```
### Answer in your notes:
What is the difference between clone and fork?
When would you clone vs fork?
After forking, how do you keep your fork in sync with the original repo? 

- **Clone vs Fork:** Cloning creates a local copy of a repository, while forking creates a copy of a repository under your GitHub account.
- **When to Clone vs Fork:** Clone when you need a local copy of a repository; fork when you want to contribute to someone else's repository without direct write access.
- **Keeping a Fork in Sync:** Add the original repository as `upstream`, fetch changes, and merge or rebase them into your fork.


