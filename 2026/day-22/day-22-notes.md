# Day 22 – Introduction to Git: Your First Repository

## **Task 1: Install and Configure Git**

### Verified that Git is installed on the system.

```bash
sufiyan@sufiyan-HP-Notebook:~$ git -v
git version 2.54.0
```

### Successfully Configured and Verified Git Settings

```bash

sufiyan@sufiyan-HP-Notebook:~$ git config --global user.name "sk7652183-rgb"
sufiyan@sufiyan-HP-Notebook:~$ git config --global user.email "sk7652183@gmail.com"
sufiyan@sufiyan-HP-Notebook:~$ git config --global --list
user.email=sk7652183@gmail.com
user.name=sk7652183-rgb

```

## **Task 2: Create Your Git Project**

### Created a new folder called devops-git-practice
### Initialized the directory as a Git repository
### Checked the status and understood what Git was telling me.

```bash
sufiyan@sufiyan-HP-Notebook:~$ git config --global --list
user.email=sk7652183@gmail.com
user.name=sk7652183-rgb
sufiyan@sufiyan-HP-Notebook:~$ mkdir -p  devops-git-practice
sufiyan@sufiyan-HP-Notebook:~$ cd devops-git-practice/
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls -la
total 8
drwxrwxr-x  2 sufiyan sufiyan 4096 Jun 15 20:32 .
drwxr-x--- 48 sufiyan sufiyan 4096 Jun 15 20:34 ..
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: will change to "main" in Git 3.0. To configure the initial branch name
hint: to use in all of your new repositories, which will suppress this warning,
hint: call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
hint:
hint: Disable this message with "git config set advice.defaultBranchName false"
Initialized empty Git repository in /home/sufiyan/devops-git-practice/.git/
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls -la
total 12
drwxrwxr-x  3 sufiyan sufiyan 4096 Jun 15 20:34 .
drwxr-x--- 48 sufiyan sufiyan 4096 Jun 15 20:34 ..
drwxrwxr-x  6 sufiyan sufiyan 4096 Jun 15 20:34 .git
```

### Explored the hidden `.git/` directory to understand how Git stores repository data and metadata.**

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ cd .git
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice/.git$ ls
config  description  HEAD  hooks  info  objects  refs

```
It contains files config  description  HEAD  hooks  info  objects  refs

- `config` – Stores repository-specific Git configuration settings.
- `description` – Contains a short description of the repository.
- `HEAD` – Points to the currently checked-out branch.
- `hooks/` – Contains Git hook scripts that run automatically on certain events.
- `info/` – Stores additional repository information and exclude rules.
- `objects/` – Stores all Git data objects (commits, files, and directories).
- `refs/` – Stores references to branches and tags.


## **Task 3: Create Your Git Commands Reference**


### Setup & Configuration

| Command | Description |
|----------|-------------|
| `git --version` | Displays the installed Git version. |
| `git config --global user.name "username"` | Sets the global Git username. |
| `git config --global user.email "email@example.com"` | Sets the global Git email address. |
| `git config --global --list` | Displays all global Git configuration settings. |
| `git config --global user.name` | Displays the configured Git username. |
| `git config --global user.email` | Displays the configured Git email address. |

### Basic Workflow

| Command | Description |
|----------|-------------|
| `git init` | Initializes a new Git repository in the current directory. |
| `git add <file>` | Adds a file to the staging area. |
| `git add .` | Adds all modified and new files to the staging area. |
| `git commit -m "message"` | Creates a commit from the staged changes and saves it to the repository history. |

### Viewing Changes

| Command | Description |
|----------|-------------|
| `git status` | Shows the status of files in the working directory and staging area. |
| `git diff` | Shows unstaged changes between the working directory and the staging area. |
| `git diff --staged` | Shows changes that are staged but not yet committed. |
| `git log` | Displays the commit history of the repository. |

##Task 4: Stage and Commit

### Created the Reference sheet and perform all the basic workflow 

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls -la
total 16
drwxrwxr-x  3 sufiyan sufiyan 4096 Jun 15 23:00 .
drwxr-x--- 48 sufiyan sufiyan 4096 Jun 15 23:01 ..
drwxrwxr-x  6 sufiyan sufiyan 4096 Jun 15 20:34 .git
-rw-rw-r--  1 sufiyan sufiyan 1751 Jun 15 23:00 git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	git-commands.md

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   git-commands.md

sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git config --global --list
user.email=sk7652183@gmail.com
user.name=sk7652183-rgb
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Added git-commands.md"
[master (root-commit) aa82211] Added git-commands.md
 1 file changed, 109 insertions(+)
 create mode 100644 git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
nothing to commit, working tree clean
```
### For each command, write:
What it does (1 line)
An example of how to use it

## Related Documents

- [Git Commands Cheat Sheet](git-commands.md)

## Task 5: Make More Changes and Build History

### Edited git-commands.md, added more commands, and checked what had changed since the last commit.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   git-commands.md

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   git-commands.md

sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "- Updated `git-commands.md` with additional Git commands."
git-commands.md: command not found
[master 0f0a98c] - Updated  with additional Git commands.
 1 file changed, 54 insertions(+), 1 deletion(-)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
nothing to commit, working tree clean
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
### Staged and committed the changes again with a descriptive commit message

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log
commit 0f0a98c834d6fa34bb4da428520bf4c0bcb4d796 (HEAD -> master)
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:37:12 2026 +0530

    - Updated  with additional Git commands.

commit aa82211b8adab3e2cfa1c691add9b537bb400540
Author: sk7652183-rgb <sk7652183@gmail.com>
Date:   Mon Jun 15 23:04:20 2026 +0530

    Added git-commands.md
```


### - Repeated the process at least three times to create multiple commits in the Git history.
- Viewed the full commit history in a compact format using `git log --oneline`.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   git-commands.md

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Added more Git commands to git-commands.md and updated the cheat sheet."
[master 4b2bab6] Added more Git commands to git-commands.md and updated the cheat sheet.
 1 file changed, 45 insertions(+), 1 deletion(-)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
nothing to commit, working tree clean
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log
commit 4b2bab664a5b8ba71a55feea305fc5e6acce6984 (HEAD -> master)
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
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 

```
### Task 6: Understand the Git Workflow

## What is the difference between git add and git commit?

`git add` moves changes to the staging area, while `git commit` saves staged changes to the repository history.

## What does the staging area do? Why doesn't Git just commit directly?

The staging area lets you review and choose which changes to include in the next commit.

## What information does git log show you?

`git log` shows the commit history, including commit IDs, authors, dates, and messages.

## What is the .git/ folder and what happens if you delete it?

The `.git/` folder stores all Git data and history; deleting it removes Git tracking from the project.

## What is the difference between a working directory, staging area, and repository?

Working Directory = Edit, Staging Area = Prepare, Repository = Save.

