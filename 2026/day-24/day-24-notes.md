# Day 24 – Advanced Git: Merge, Rebase, Stash & Cherry Pick

## Task 1: Git Merge — Hands-On

### Created a new branch feature-login from main and added a couple of commits to it

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
* feature-1
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout -b feature-login
Switched to a new branch 'feature-login'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md  test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md  test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
* feature-login
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim login.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status 
On branch feature-login
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	login.html

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add login page UI"
[feature-login ce3ee08] Add login page UI
 1 file changed, 23 insertions(+)
 create mode 100644 login.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md  login.html  test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
ce3ee08 (HEAD -> feature-login) Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim login.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status 
On branch feature-login
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	login.js

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add login form validation"
[feature-login a3a4106] Add login form validation
 1 file changed, 7 insertions(+)
 create mode 100644 login.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
a3a4106 (HEAD -> feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
```
### Switched back to main and merged feature-login into main

```bash

sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
* feature-login
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
afc18ed (HEAD -> master, origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git merge feature-login
Merge made by the 'ort' strategy.
 login.html | 23 +++++++++++++++++++++++
 login.js   |  7 +++++++
 test.py    |  2 ++
 3 files changed, 32 insertions(+)
 create mode 100644 login.html
 create mode 100644 login.js
 create mode 100644 test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
2ca4e61 (HEAD -> master) Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 

```
### Observed the merge — did Git perform a fast-forward merge or create a merge commit?"

The merge of feature-login into master was a three-way merge because both branches had diverged. Git created a merge commit (2ca4e61) using the ort merge strategy.

## Fast-Forward Merge vs Three-Way Merge

| Fast-Forward Merge | Three-Way Merge |
|-------------------|-----------------|
| No merge commit is created. | A merge commit is created. |
| Happens when the target branch has not changed since the feature branch was created. | Happens when both branches have new commits. |
| Git simply moves the branch pointer forward. | Git combines two separate histories. |
| Maintains a linear commit history. | Preserves branch history. |

### Fast-Forward Merge

Before merge:

```text
A---B---C (main)
         \
          D---E (feature-login)
```

```bash
git checkout main
git merge feature-login
```

After merge:

```text
A---B---C---D---E (main)
```

Output:

```bash
Updating abc123..def456
Fast-forward
```

### Three-Way Merge

Before merge:

```text
A---B---C---F (main)
         \
          D---E (feature-login)
```

```bash
git checkout main
git merge feature-login
```

After merge:

```text
A---B---C---F------M (main)
         \        /
          D------E
```

`M` is the merge commit created by Git.

Output:

```bash
Merge made by the 'ort' strategy.
```

### Example from This Repository

The merge of `feature-login` into `main` was a **three-way merge** because both branches contained new commits. Git created a merge commit to combine the histories:

```bash
*   2ca4e61 Merge branch 'feature-login'
|\
| * a3a4106 Add login form validation
| * ce3ee08 Add login page UI
* | afc18ed Updated git-commands.md
|/
```

This is not a fast-forward merge because Git created the merge commit `2ca4e61`.

### Created another branch, feature-signup, and added commits to it.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim signup.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-signup
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	signup.html

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add signup page UI"
[feature-signup 2cf5e4f] Add signup page UI
 1 file changed, 28 insertions(+)
 create mode 100644 signup.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim signup.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-signup
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	signup.js

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add signup form validation"
[feature-signup d2f8c09] Add signup form validation
 1 file changed, 11 insertions(+)
 create mode 100644 signup.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
d2f8c09 (HEAD -> feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 (master) Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
### Added a commit to main before merging.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-login
* feature-signup
  master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
2ca4e61 (HEAD -> master) Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md  login.html  login.js  test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim utils.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	utils.py

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add utility function for user greeting"
[master 6e7aa5d] Add utility function for user greeting
 1 file changed, 2 insertions(+)
 create mode 100644 utils.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
6e7aa5d (HEAD -> master) Add utility function for user greeting
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim create app.py
2 files to edit
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
create  git-commands.md  login.html  login.js  test.py  utils.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	create

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add application entry point"
[master 2492ba9] Add application entry point
 1 file changed, 5 insertions(+)
 create mode 100644 create
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
2492ba9 (HEAD -> master) Add application entry point
6e7aa5d Add utility function for user greeting
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```

### Merged the feature-signup branch into main

```
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-login
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git merge feature-signup
Merge made by the 'ort' strategy.
 signup.html | 28 ++++++++++++++++++++++++++++
 signup.js   | 11 +++++++++++
 2 files changed, 39 insertions(+)
 create mode 100644 signup.html
 create mode 100644 signup.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
0c9143e (HEAD -> master) Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
create  git-commands.md  login.html  login.js  signup.html  signup.js  test.py  utils.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```


### Before merging `feature-signup`, I added commits directly to `main`:

- Add utility function for user greeting
- Add application entry point

At the same time, the `feature-signup` branch contained its own commits:

- Add signup page UI
- Add signup form validation

Because both branches had new commits, Git could not perform a fast-forward merge. Instead, it created a merge commit using the `ort` merge strategy:

### Answer in your notes:

A fast-forward merge is a merge in which Git does not create a merge commit. Instead, it simply moves the target branch pointer forward. The commit history remains linear.

### When does Git create a merge commit instead?

Git creates a merge commit when both branches have new commits and their histories have diverged.

### What is a merge conflict? (try creating one intentionally by editing the same line in both branches)

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git merge feature-conflict
Auto-merging app.py
CONFLICT (content): Merge conflict in app.py
Automatic merge failed; fix conflicts and then commit the result.
```
```bash
def greet():
<<<<<<< HEAD
    print("Hello from the main")
=======
    print("Hello from feature branch")
>>>>>>> feature-conflict
```

## Merge Conflict Example

A merge conflict occurs when the same line in a file is modified differently in two branches.

**main**

```python
def greet():
    print("Hello from main branch")
```

**feature-conflict**

```python
def greet():
    print("Hello from feature branch")
```

When merging:

```bash
git merge feature-conflict
```

Git cannot decide which change to keep and reports a merge conflict. The conflict must be resolved manually, then committed.

```bash
git add app.py
git commit -m "Resolve merge conflict"
```


## Task 2: Git Rebase — Hands-On

### Created a branch feature-dashboard and added 2–3 commits to it

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-conflict
  feature-login
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout -b feature-dashboard
Switched to a new branch 'feature-dashboard'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
app.py  create  git-commands.md  login.html  login.js  signup.html  signup.js  test.py  utils.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim dashboard.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ cat dashboard.html
<!DOCTYPE html>
<html>
<head>
    <title>Dashboard</title>
</head>
<body>
    <h1>User Dashboard</h1>

    <ul>
        <li>Profile</li>
        <li>Settings</li>
        <li>Logout</li>
    </ul>
</body>
</html>
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-dashboard
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	dashboard.html

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add dashboard page UI"
[feature-dashboard 3d4ed9e] Add dashboard page UI
 1 file changed, 15 insertions(+)
 create mode 100644 dashboard.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim dashboard.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-dashboard
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	dashboard.js

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add dashboard.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add dashboard statistics module"
[feature-dashboard 7c0b3b4] Add dashboard statistics module
 1 file changed, 7 insertions(+)
 create mode 100644 dashboard.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-dashboard
nothing to commit, working tree clean
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
7c0b3b4 (HEAD -> feature-dashboard) Add dashboard statistics module
3d4ed9e Add dashboard page UI
74de71f (master) Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
### While on main, I added a new commit so that main would be ahead

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch master
Already on 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
74de71f (HEAD -> master) Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
app.py  create  git-commands.md  login.html  login.js  signup.html  signup.js  test.py  utils.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim create config.py
2 files to edit
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim config.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add config.py
git commit -m "Add application configuration file"
[master a1d8f3f] Add application configuration file
 1 file changed, 2 insertions(+)
 create mode 100644 config.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim logger.py:
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add logger.py
git commit -m "Add basic logging utility"
fatal: pathspec 'logger.py' did not match any files
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	logger.py:

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	logger.py:

nothing added to commit but untracked files present (use "git add" to track)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Add basic logging utility"
[master f6c27a0] Add basic logging utility
 1 file changed, 2 insertions(+)
 create mode 100644 logger.py:
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
f6c27a0 (HEAD -> master) Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
- Switched to `feature-dashboard` and rebased it onto `main`.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-conflict
  feature-dashboard
  feature-login
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git switch feature-dashboard
Switched to branch 'feature-dashboard'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git rebase main
fatal: invalid upstream 'main'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git rebase master
Successfully rebased and updated refs/heads/feature-dashboard.
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
6b79727 (HEAD -> feature-dashboard) Add dashboard statistics module
ab7e1b9 Add dashboard page UI
f6c27a0 (master) Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 

```
### Observe your git log --oneline --graph --all — how does the history look compared to a merge?

After rebasing, the commit history looks linear. The commits from main appear first, followed by the commits from feature-dashboard. Unlike a merge, no new merge commit was created, resulting in a cleaner and more straightforward history.

### Answer in your notes:
What does rebase actually do to your commits?
Rebase rewrites commit history by replaying commits on top of another branch. The original branch history is not preserved as a separate branch structure.

How is the history different from a merge?
The history is rewritten during a rebase, whereas a merge preserves the original branch history by creating a merge commit

Why should you never rebase commits that have been pushed and shared with others?
Rebasing rewrites commit history and creates new commit hashes. If the commits have already been shared, other developers may have based their work on the original commits, leading to conflicts, duplicate commits, and confusion when synchronizing changes.

When would you use rebase vs merge?
Rebase is used to create a clean linear history, while merge is used to preserve branch history and collaboration context

## Task 3: Squash Commit vs Merge Commit

### Create a branch feature-profile, add 4-5 small commits (typo fix, formatting, etc.

- Created the `feature-profile` branch and added 4 commits to it.

```bash
git log --oneline
572fff9 (HEAD -> feature-profile) Updated login.js for further testing
fc41761 Updated login.html for further testing
57f45cf Updated config.py for further testing
65a9d86 Updated app.py for further testing
6b79727 (feature-dashboard) Add dashboard statistics module
ab7e1b9 Add dashboard page UI
f6c27a0 (master) Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
```

- Merged `feature-profile` into `master` using `--squash`.
- All 4–5 commits from the feature branch were combined into a single commit.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git merge --squash feature-profile
Updating f6c27a0..572fff9
Fast-forward
Squash commit -- not updating HEAD
 app.py         |  1 +
 config.py      |  1 +
 dashboard.html | 15 +++++++++++++++
 dashboard.js   |  7 +++++++
 login.html     |  1 +
 login.js       |  2 ++
 6 files changed, 27 insertions(+)
 create mode 100644 dashboard.html
 create mode 100644 dashboard.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
f6c27a0 (HEAD -> master) Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Added profile  feature"
[master 63caa00] Added profile  feature
 6 files changed, 27 insertions(+)
 create mode 100644 dashboard.html
 create mode 100644 dashboard.js
```

- Check git log — how many commits were added to master?
- Checked `git log` and observed that only one commit was added to `master`.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
63caa00 (HEAD -> master) Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```

-Now created another branch feature-settings, added a few commits

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
7c852ba (HEAD -> feature-settings) Add theme preference support
e612858 Add settings validation
54fd03d Add settings save functionality
18241a6 Add settings page UI
63caa00 (master) Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
- Merged it into `master` without using squash (regular merge) and compared the commit history.

History before merge in master branch
```
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout master 
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline 
63caa00 (HEAD -> master) Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
History After merge in master branch



```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git merge feature-settings
Updating 63caa00..7c852ba
Fast-forward
 Update             |  3 +++
 Update_settings.js |  3 +++
 settings.html      | 23 +++++++++++++++++++++++
 settings.js        |  3 +++
 4 files changed, 32 insertions(+)
 create mode 100644 Update
 create mode 100644 Update_settings.js
 create mode 100644 settings.html
 create mode 100644 settings.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline 
7c852ba (HEAD -> master, feature-settings) Add theme preference support
e612858 Add settings validation
54fd03d Add settings save functionality
18241a6 Add settings page UI
63caa00 Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 (feature-signup) Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
```

Observation

Unlike a squash merge, all commits from the feature-settings branch were preserved in the master branch history. No commits were combined, and each commit remained visible in the commit log.

### Answer in your notes
### What does squash merging do?

A squash merge combines multiple commits from a feature branch into a single commit before adding them to the target branch. This helps keep the commit history clean and concise.

### When would you use squash merge vs regular merge?

- Use a **squash merge** when a feature branch contains many small commits and you want a clean commit history with a single commit.
- Use a **regular merge** when you want to preserve all commits and maintain the complete branch history.

### What is the trade-off of squashing?

Squashing creates a cleaner and more concise commit history by combining multiple commits into one. However, it removes the detailed history of individual commits, making it harder to see the step-by-step development of a feature.

### Trade-Off of Squash Merging

| Advantage                                     | Disadvantage                                      |
| --------------------------------------------- | ------------------------------------------------- |
| Creates a cleaner commit history              | Individual commit history is lost                 |
| Makes `git log` easier to read                | Harder to trace step-by-step development          |
| Reduces clutter from small commits            | Less detailed debugging history                   |
| Presents a feature as a single logical change | Original branch commit structure is not preserved |


### Task 4: Git Stash — Hands-On
- Made some changes to a file but did not commit them.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-conflict
  feature-dashboard
  feature-login
  feature-profile
  feature-settings
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
app.py     create          dashboard.js     logger.py:  login.js       settings.js  signup.js  Update              utils.py
config.py  dashboard.html  git-commands.md  login.html  settings.html  signup.html  test.py    Update_settings.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ cat app.py
# This is the code for Testing Purpose 
def greet():
    print("Hello from feature branch")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim app.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   app.py

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   app.py
```
### What happens when switching branches with uncommitted changes?

While trying to switch branches, Git prevented the switch and prompted me to commit or stash the uncommitted changes first.
```bash
error: Your local changes to the following files would be overwritten by checkout
Please commit your changes or stash them before you switch branches.
```

- Used `git stash` to save my work in progress.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash apply
No stash entries found.
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash 
Saved working directory and index state WIP on master: 7c852ba Add theme preference support
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash list
stash@{0}: WIP on master: 7c852ba Add theme preference support
```
- Switched to another branch, did some work, and then switched back.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-conflict
  feature-dashboard
  feature-login
  feature-profile
  feature-settings
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch master
nothing to commit, working tree clean
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout feature-signup
Switched to branch 'feature-signup'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
git-commands.md  login.html  login.js  signup.html  signup.js  test.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim signup.html
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-signup
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   signup.html

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add .
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "updated signup.html"
[feature-signup e682508] updated signup.html
 1 file changed, 21 insertions(+), 11 deletions(-)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
- Applied the stashed changes using `git stash pop`.
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout master
Switched to branch 'master'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash pop
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   app.py

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (e1e874bd9d5e73c39ef84d543587e9999bfd0956)
```
- Tried stashing multiple times and listed all stashes.
 ```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash list
stash@{0}: On master: Updated login.js
stash@{1}: On master: Updated dashboard.js
stash@{2}: On master: Update create page
stash@{3}: On master: Update login validation
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
- Tried applying a specific stash from the stash list using `git stash apply`.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash apply stash@{2}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   create

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash apply stash@{4}
fatal: log for 'stash' only has 4 entries
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash apply stash@{3}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   app.py
	modified:   create

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash apply stash@{1}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   app.py
	modified:   create
	modified:   dashboard.js

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git stash apply stash@{0}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   app.py
	modified:   create
	modified:   dashboard.js
	modified:   login.js

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 

```

### Answer in your notes:
### What is the difference between `git stash pop` and `git stash apply`?

- `git stash apply` restores the stashed changes but keeps the stash in the stash list.
- `git stash pop` restores the stashed changes and removes the stash from the stash list.

### When would you use stash in a real-world workflow?

Use `git stash` when you have uncommitted changes but need to switch to another branch or work on a different task. It temporarily saves your changes so you can restore them later without creating a commit.

### - Created the `feature-hotfix` branch and made 3 commits with different changes.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-conflict
  feature-dashboard
  feature-login
  feature-profile
  feature-settings
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git checkout -b feature-hotfix
Switched to a new branch 'feature-hotfix'
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ ls
app.py     create          dashboard.js     logger.py:  login.js       settings.js  signup.js  Update              utils.py
config.py  dashboard.html  git-commands.md  login.html  settings.html  signup.html  test.py    Update_settings.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ vim app.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git status
On branch feature-hotfix
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   app.py
	modified:   create
	modified:   dashboard.js
	modified:   login.js

no changes added to commit (use "git add" and/or "git commit -a")
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add dashboard.js login.js
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Updated minor changes in UI Related stuff"
[feature-hotfix 3366885] Updated minor changes in UI Related stuff
 2 files changed, 2 insertions(+)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add app.py
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Upated app.py for feature-hotfix branch"
[feature-hotfix fe84ea0] Upated app.py for feature-hotfix branch
 1 file changed, 10 insertions(+), 3 deletions(-)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git add create
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git commit -m "Updated the create page to fix an urgent bug"
[feature-hotfix c3b6d64] Updated the create page to fix an urgent bug
 1 file changed, 1 insertion(+)
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
c3b6d64 (HEAD -> feature-hotfix) Updated the create page to fix an urgent bug
fe84ea0 Upated app.py for feature-hotfix branch
3366885 Updated minor changes in UI Related stuff
7c852ba (master, feature-settings) Add theme preference support
e612858 Add settings validation
54fd03d Add settings save functionality
18241a6 Add settings page UI
63caa00 Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```
Cherry-pick only the second commit from feature-hotfix onto main
```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git branch
  feature-1
  feature-conflict
  feature-dashboard
  feature-hotfix
  feature-login
  feature-profile
  feature-settings
  feature-signup
* master
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
7c852ba (HEAD -> master, feature-settings) Add theme preference support
e612858 Add settings validation
54fd03d Add settings save functionality
18241a6 Add settings page UI
63caa00 Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git cherry-pick c3b6d64
[master 0413e9d] Updated the create page to fix an urgent bug
 Date: Thu Jun 18 23:05:43 2026 +0530
 1 file changed, 1 insertion(+)
```
- Verified with `git log` that only the selected commit was applied.

```bash
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ git log --oneline
0413e9d (HEAD -> master) Updated the create page to fix an urgent bug
7c852ba (feature-settings) Add theme preference support
e612858 Add settings validation
54fd03d Add settings save functionality
18241a6 Add settings page UI
63caa00 Added profile  feature
f6c27a0 Add basic logging utility
a1d8f3f Add application configuration file
74de71f Resolve merge conflict
44fa941 Update greeting in main branch
0b5cb9f (feature-conflict) Update greeting in feature branch
6958dae Add greet function
0c9143e Merge branch 'feature-signup'
2492ba9 Add application entry point
6e7aa5d Add utility function for user greeting
d2f8c09 Add signup form validation
2cf5e4f Add signup page UI
2ca4e61 Merge branch 'feature-login'
a3a4106 (feature-login) Add login form validation
ce3ee08 Add login page UI
62f84ef (origin/feature-1, feature-1) Fix typo in comment for clarity
afc18ed (origin/master, origin/HEAD) Updated git-commands.md with Day 23 commands and descriptions
67cfdcb Updated git-commands.md with Day 23 commands
581e274 Added test.py for Testing purpose
4b2bab6 Added more Git commands to git-commands.md and updated the cheat sheet.
1ef76a7 - Updated  with additional Git commands and details.
0f0a98c - Updated  with additional Git commands.
aa82211 Added git-commands.md
sufiyan@sufiyan-HP-Notebook:~/devops-git-practice$ 
```

### Answer in your notes
### What does cherry-pick do?

`git cherry-pick` allows you to apply a specific commit from one branch to another without merging the entire branch. It is useful when you only need a particular change instead of all commits from the source branch.

### When would you use cherry-pick in a real project?

Use `git cherry-pick` when you need to apply a specific bug fix or critical change from another branch without merging all of its commits. This is useful when you want to deploy an urgent fix while avoiding unrelated changes that could affect the application.

### What can go wrong with cherry-picking?

- It can create duplicate commits in different branches.
- It may cause merge conflicts later.
- It can make the commit history harder to track and understand.
- Related commits might be missed if only one commit is cherry-picked.
