# Day 26 – GitHub CLI: Manage GitHub from Your Terminal

## Task 1: Install and Authenticate

### Install the GitHub CLI on your machine

```bash
sufiyan@Khan:~$ gh --version
Command 'gh' not found, but can be installed with:
sudo snap install gh  # version 2.86.0-112-gc30647b78, or
sudo apt  install gh  # version 2.45.0-1ubuntu0.3
See 'snap info gh' for additional versions.
sufiyan@Khan:~$ sudo apt  install gh
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  gh
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 8,836 kB of archives.
After this operation, 45.4 MB of additional disk space will be used.
Get:1 http://in.archive.ubuntu.com/ubuntu noble-updates/universe amd64 gh amd64 2.45.0-1ubuntu0.3 [8,836 kB]
Fetched 8,836 kB in 8s (1,042 kB/s)                                                                                                                                                    
Selecting previously unselected package gh.
(Reading database ... 245794 files and directories currently installed.)
Preparing to unpack .../gh_2.45.0-1ubuntu0.3_amd64.deb ...
Unpacking gh (2.45.0-1ubuntu0.3) ...
Setting up gh (2.45.0-1ubuntu0.3) ...
Processing triggers for man-db (2.12.0-4build2) ...
sufiyan@Khan:~$ gh --version
gh version 2.45.0 (2025-07-18 Ubuntu 2.45.0-1ubuntu0.3)
https://github.com/cli/cli/releases/tag/v2.45.0
sufiyan@Khan:~$ 
```
### Authenticate with your GitHub account

```bash
sufiyan@Khan:~$ gh --version
gh version 2.45.0 (2025-07-18 Ubuntu 2.45.0-1ubuntu0.3)
https://github.com/cli/cli/releases/tag/v2.45.0
sufiyan@Khan:~$ gh auth status
github.com
  ✓ Logged in to github.com account sk7652183-rgb (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
sufiyan@Khan:~$ 
```
### Verify you're logged in and check which account is active

```bash
sufiyan@Khan:~$ gh --version
gh version 2.45.0 (2025-07-18 Ubuntu 2.45.0-1ubuntu0.3)
https://github.com/cli/cli/releases/tag/v2.45.0
sufiyan@Khan:~$ gh auth status
github.com
  ✓ Logged in to github.com account sk7652183-rgb (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
sufiyan@Khan:~$ gh auth status && gh api user --jq .login
github.com
  ✓ Logged in to github.com account sk7652183-rgb (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
sk7652183-rgb
sufiyan@Khan:~$ 
```
### Answer in your notes: What authentication methods does gh support?

- Personal Access Token (PAT) / token-based authentication (recommended)
- Web browser OAuth authentication
- SSH key authentication (for Git operations)

## Task 2: Working with Repositories
### Create a new GitHub repo directly from the terminal — make it public with a README

- Used `gh repo create github-cli-practice --public --clone --add-readme` to create a new public GitHub repository and initialize it with a README.
```bash
sufiyan@Khan:~$ gh repo create github-cli-practice --public --clone --add-readme
✓ Created repository sk7652183-rgb/github-cli-practice on GitHub
  https://github.com/sk7652183-rgb/github-cli-practice
Cloning into 'github-cli-practice'...
sufiyan@Khan:~$ gh repo list

Showing 15 of 15 repositories in @sk7652183-rgb

NAME                                       DESCRIPTION                                                                                              INFO          UPDATED               
sk7652183-rgb/github-cli-practice                                                                                                                   public        less than a minute ago
sk7652183-rgb/90DaysOfDevOps               This repository is a Challenge for the DevOps Community to get stronger in DevOps.The reason for mak...  public, fork  about 23 hours ago
sk7652183-rgb/devops-git-practice                                                                                                                   public        about 23 hours ago
sk7652183-rgb/github-actions-praticed      This Repo is for Praticing GitHub Action                                                                 public        about 4 days ago
sk7652183-rgb/devboard                                                                                                                              public        about 6 days ago
sk7652183-rgb/gh-aw-mcpg                   Github Agentic Workflows MCP Gateway                                                                     public, fork  about 7 days ago
sk7652183-rgb/kubernetes                   Production-Grade Container Scheduling and Management                                                     public, fork  about 7 days ago
sk7652183-rgb/shell-script                                                                                                                          public        about 9 days ago
sk7652183-rgb/Shell-Scripting-For-DevOps                                                                                                            public, fork  about 1 month ago
sk7652183-rgb/python-for-devops            Python For DevOps [AI Edition] is a hands-on, beginner-friendly live course that teaches you the exa...  public, fork  about 1 month ago
sk7652183-rgb/DevSecOps-Zero-to-Hero       Learn DevSecOps in a week.                                                                               public, fork  about 4 months ago
sk7652183-rgb/Shell-Scripting              Shell Scripting for Automation                                                                           public        about 5 months ago
sk7652183-rgb/-Ansible_Projects            Infrastructure automation project using Ansible to deploy my personal DevOps portfolio on multiple s...  public        about 5 months ago
sk7652183-rgb/-Jenkins-shared-libaries                                                                                                              public        about 5 months ago
sk7652183-rgb/terraform_infra_environment  Infrastructure as Code with Terraform — automated provisioning of Dev, Staging, and Production envir...  public        about 5 months ago
```
### Clone a repo using gh instead of git clone
- Used `gh repo clone sk7652183-rgb/github-cli-practice` to clone a GitHub repository directly from the terminal.

### View details of one of your repos from the terminal

- Used `gh repo view github-cli-practice` to inspect repository information from the command line.

```bash
sufiyan@Khan:~$ gh repo view github-cli-practice
sk7652183-rgb/github-cli-practice
No description provided


   github-cli-practice                                                                                                



View this repository on GitHub: https://github.com/sk7652183-rgb/github-cli-practice
sufiyan@Khan:~$ 
```
### List all your repositories

```bash
sufiyan@Khan:~$ gh repo list

Showing 15 of 15 repositories in @sk7652183-rgb

NAME                                       DESCRIPTION                                                                                                INFO          UPDATED             
sk7652183-rgb/github-cli-practice                                                                                                                     public        about 12 minutes ago
sk7652183-rgb/90DaysOfDevOps               This repository is a Challenge for the DevOps Community to get stronger in DevOps.The reason for makin...  public, fork  about 1 day ago
sk7652183-rgb/devops-git-practice                                                                                                                     public        about 1 day ago
sk7652183-rgb/github-actions-praticed      This Repo is for Praticing GitHub Action                                                                   public        about 4 days ago
sk7652183-rgb/devboard                                                                                                                                public        about 6 days ago
sk7652183-rgb/gh-aw-mcpg                   Github Agentic Workflows MCP Gateway                                                                       public, fork  about 7 days ago
sk7652183-rgb/kubernetes                   Production-Grade Container Scheduling and Management                                                       public, fork  about 7 days ago
sk7652183-rgb/shell-script                                                                                                                            public        about 9 days ago
sk7652183-rgb/Shell-Scripting-For-DevOps                                                                                                              public, fork  about 1 month ago
sk7652183-rgb/python-for-devops            Python For DevOps [AI Edition] is a hands-on, beginner-friendly live course that teaches you the exact...  public, fork  about 1 month ago
sk7652183-rgb/DevSecOps-Zero-to-Hero       Learn DevSecOps in a week.                                                                                 public, fork  about 4 months ago
sk7652183-rgb/Shell-Scripting              Shell Scripting for Automation                                                                             public        about 5 months ago
sk7652183-rgb/-Ansible_Projects            Infrastructure automation project using Ansible to deploy my personal DevOps portfolio on multiple ser...  public        about 5 months ago
sk7652183-rgb/-Jenkins-shared-libaries                                                                                                                public        about 5 months ago
sk7652183-rgb/terraform_infra_environment  Infrastructure as Code with Terraform — automated provisioning of Dev, Staging, and Production environ...  public        about 5 months ago
sufiyan@Khan:~$ 
```
### Open a repo in your browser directly from the terminal 
```bash
sufiyan@Khan:~/github-cli-practice$ gh repo view --web
Opening github.com/sk7652183-rgb/github-cli-practice in your browser.
sufiyan@Khan:~/github-cli-practice$ Opening in existing browser session.
```
### Delete the test repo you created (be careful!)

- Deleted a GitHub repository from the terminal using `gh repo delete`.

```bash
sufiyan@Khan:~/github-cli-practice$ gh auth status
github.com
  ✓ Logged in to github.com account sk7652183-rgb (keyring)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'delete_repo', 'gist', 'read:org', 'repo', 'workflow'
sufiyan@Khan:~/github-cli-practice$ gh repo delete sk7652183-rgb/github-cli-practice
? Type sk7652183-rgb/github-cli-practice to confirm deletion: sk7652183-rgb/github-cli-practice
✓ Deleted repository sk7652183-rgb/github-cli-practice
sufiyan@Khan:~/github-cli-practice$ gh repo list

Showing 14 of 14 repositories in @sk7652183-rgb

NAME                                       DESCRIPTION                                                                                                  INFO          UPDATED           
sk7652183-rgb/90DaysOfDevOps               This repository is a Challenge for the DevOps Community to get stronger in DevOps.The reason for making ...  public, fork  about 1 day ago
sk7652183-rgb/devops-git-practice                                                                                                                       public        about 1 day ago
sk7652183-rgb/github-actions-praticed      This Repo is for Praticing GitHub Action                                                                     public        about 4 days ago
sk7652183-rgb/devboard                                                                                                                                  public        about 6 days ago
sk7652183-rgb/gh-aw-mcpg                   Github Agentic Workflows MCP Gateway                                                                         public, fork  about 7 days ago
sk7652183-rgb/kubernetes                   Production-Grade Container Scheduling and Management                                                         public, fork  about 7 days ago
sk7652183-rgb/shell-script                                                                                                                              public        about 9 days ago
sk7652183-rgb/Shell-Scripting-For-DevOps                                                                                                                public, fork  about 1 month ago
sk7652183-rgb/python-for-devops            Python For DevOps [AI Edition] is a hands-on, beginner-friendly live course that teaches you the exact P...  public, fork  about 1 month ago
sk7652183-rgb/DevSecOps-Zero-to-Hero       Learn DevSecOps in a week.                                                                                   public, fork  about 4 months ago
sk7652183-rgb/Shell-Scripting              Shell Scripting for Automation                                                                               public        about 5 months ago
sk7652183-rgb/-Ansible_Projects            Infrastructure automation project using Ansible to deploy my personal DevOps portfolio on multiple serve...  public        about 5 months ago
sk7652183-rgb/-Jenkins-shared-libaries                                                                                                                  public        about 5 months ago
sk7652183-rgb/terraform_infra_environment  Infrastructure as Code with Terraform — automated provisioning of Dev, Staging, and Production environme...  public        about 5 months ago
sufiyan@Khan:~/github-cli-practice$ 

```
## Task 3: Issues

### Create an issue on one of your repos from the terminal — give it a title, body, and a label

```bash
sufiyan@Khan:~/github-cli-practice$ gh issue create \
  --repo sk7652183-rgb/git-commands \
  --title "Test Issue" \
  --body "Created using GitHub CLI." \
  --label "documentation"

Creating issue in sk7652183-rgb/git-commands
```
### List all open issues on that repo
```bash
sufiyan@Khan:~/github-cli-practice$ gh issue list --repo sk7652183-rgb/git-commands

Showing 1 of 1 open issue in sk7652183-rgb/git-commands

ID  TITLE       LABELS         UPDATED            
#1  Test Issue  documentation  about 4 minutes ago
```

### View a specific issue by its number

```bash

sufiyan@Khan:~/github-cli-practice$ gh issue view 1 --repo sk7652183-rgb/git-commands \
  --json number,title,state,author,body
{
  "author": {
    "id": "U_kgDODugVMg",
    "is_bot": false,
    "login": "sk7652183-rgb",
    "name": "Abusufiyan Khan"
  },
  "body": "Created using GitHub CLI.",
  "number": 1,
  "state": "OPEN",
  "title": "Test Issue"
}
sufiyan@Khan:~/github-cli-practice$ 
```

### Close an issue from the terminal
```bash
sufiyan@Khan:~/github-cli-practice$ gh issue close 1 \
  --repo sk7652183-rgb/git-commands \
  --comment "Issue resolved and closed."
✓ Closed issue #1 (Test Issue)
sufiyan@Khan:~/github-cli-practice$ gh issue view 1 --repo sk7652183-rgb/git-commands --json state
{
  "state": "CLOSED"
}
sufiyan@Khan:~/github-cli-practice$ 
```

### Answer in your notes: How could you use gh issue in a script or automation?

gh issue can be used in scripts and automation to create, list, update, and close GitHub issues automatically based on events, errors, test results, or deployment outcomes.

## Task 4: Pull Requests
### Create a Branch, Make Changes, Push, and Open a Pull Request

* Created a new branch from `main`.
* Modified the `README.md` file and committed the changes.
* Pushed the feature branch to GitHub.
* Verified that the feature branch contained commits ahead of `main`.
* Created a pull request from the terminal using GitHub CLI (`gh pr create`).
* Learned that a pull request can only be created when the feature branch has commits that differ from the base branch.
```bash
sufiyan@Khan:~/github-cli-practice$ gh pr create \
  --title "Update README" \
  --body "Added README update from terminal." \
  --base main \
  --head feature-readme-update

Creating pull request for feature-readme-update into main in sk7652183-rgb/github-cli-practice

pull request create failed: GraphQL: The feature-readme-update branch has no history in common with main (createPullRequest)
sufiyan@Khan:~/github-cli-practice$ git log --oneline --graph --all --decorate
* 713982b (HEAD -> feature-readme-update, origin/main, origin/feature-readme-update, origin/HEAD, main) Initial commit
sufiyan@Khan:~/github-cli-practice$ git status
On branch feature-readme-update
Your branch is up to date with 'origin/feature-readme-update'.

nothing to commit, working tree clean
sufiyan@Khan:~/github-cli-practice$ echo "PR test" >> README.md

git add README.md
git commit -m "Update README for PR test"
[feature-readme-update 658d896] Update README for PR test
 1 file changed, 1 insertion(+), 1 deletion(-)
sufiyan@Khan:~/github-cli-practice$ git log --oneline --graph --all --decorate
* 658d896 (HEAD -> feature-readme-update) Update README for PR test
* 713982b (origin/main, origin/feature-readme-update, origin/HEAD, main) Initial commit
sufiyan@Khan:~/github-cli-practice$ git push origin feature-readme-update
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 286 bytes | 286.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/sk7652183-rgb/github-cli-practice.git
   713982b..658d896  feature-readme-update -> feature-readme-update
sufiyan@Khan:~/github-cli-practice$ gh pr create \
  --title "Update README" \
  --body "Added README update from terminal." \
  --base main \
  --head feature-readme-update-v3

Creating pull request for feature-readme-update-v3 into main in sk7652183-rgb/github-cli-practice

https://github.com/sk7652183-rgb/github-cli-practice/pull/1

```

### List all open PRs on a repo

```bash
sufiyan@Khan:~/github-cli-practice$ gh pr status

Relevant pull requests in sk7652183-rgb/github-cli-practice

Current branch
  #1  Update README [feature-readme-update-v3]

Created by you
  #1  Update README [feature-readme-update-v3]

Requesting a code review from you
  You have no pull requests to review

sufiyan@Khan:~/github-cli-practice$ gh pr list

Showing 1 of 1 open pull request in sk7652183-rgb/github-cli-practice

ID  TITLE          BRANCH                    CREATED AT        
#1  Update README  feature-readme-update-v3  about 1 minute ago
sufiyan@Khan:~/github-cli-practice$ gh pr list --state all

Showing 1 of 1 pull request in sk7652183-rgb/github-cli-practice that matches your search

ID  TITLE          BRANCH                    CREATED AT         
#1  Update README  feature-readme-update-v3  about 2 minutes ago
```

### View the details of your PR — check its status, reviewers, and checks

```bash

sufiyan@Khan:~/github-cli-practice$ gh pr view 1 --json title,state,reviewRequests,reviews,statusCheckRollup
{
  "reviewRequests": [],
  "reviews": [],
  "state": "OPEN",
  "statusCheckRollup": [],
  "title": "Update README"
}
sufiyan@Khan:~/github-cli-practice$ 
```
### Merge your PR from the terminal
```bash
sufiyan@Khan:~/github-cli-practice$ gh pr merge 1
Merging pull request #1 (Update README)
? What merge method would you like to use? Create a merge commit
? Delete the branch locally and on GitHub? Yes
? What's next? Submit
✓ Merged pull request #1 (Update README)
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (1/1), 905 bytes | 452.00 KiB/s, done.
From https://github.com/sk7652183-rgb/github-cli-practice
 * branch            main       -> FETCH_HEAD
   172c1f6..614ed2d  main       -> origin/main
Updating 172c1f6..614ed2d
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
✓ Deleted local branch feature-readme-update-v3 and switched to branch main
✓ Deleted remote branch feature-readme-update-v3
sufiyan@Khan:~/github-cli-practice$ gh pr view 1
GraphQL: Projects (classic) is being deprecated in favor of the new Projects experience, see: https://github.blog/changelog/2024-05-23-sunset-notice-projects-classic/. (repository.pullRequest.projectCards)
sufiyan@Khan:~/github-cli-practice$ gh pr list --state merged

Showing 1 of 1 pull request in sk7652183-rgb/github-cli-practice that matches your search

ID  TITLE          BRANCH                    CREATED AT          
#1  Update README  feature-readme-update-v3  about 12 minutes ago
sufiyan@Khan:~/github-cli-practice$ 
```
## Answer in your notes
### What merge methods does gh pr merge support?
# Merge commit
gh pr merge 1 --merge

# Squash and merge
gh pr merge 1 --squash

# Rebase and merge
gh pr merge 1 --rebase
### How would you review someone else's PR using gh?
To review someone else's PR using GitHub CLI, view the PR (gh pr view), inspect the changes (gh pr diff), optionally check it out locally (gh pr checkout), and submit a review with gh pr review.

## Task 5: GitHub Actions & Workflows (Preview)
### List the workflow runs on any public repo that uses GitHub Actions

```bash

ufiyan@Khan:~/github-cli-practice$ gh run list --repo sk7652183-rgb/github-actions-praticed
STATUS  TITLE  WORKFLOW  BRANCH  EVENT              ID           ELAPSED  AGE             
✓       Hello  Hello     main    workflow_dispatch  27860801381  7s       about 5 days ago
sufiyan@Khan:~/github-cli-practice$ 
```
### View the status of a specific workflow run

```bash
sufiyan@Khan:~/github-cli-practice$ gh run view 27860801381 \
  --repo sk7652183-rgb/github-actions-praticed \
  --json status,conclusion
{
  "conclusion": "success",
  "status": "completed"
}
sufiyan@Khan:~/github-cli-practice$ 
```
### Answer in your notes: How could gh run and gh workflow be useful in a CI/CD pipeline?

**`gh workflow` can be used to manage and trigger GitHub Actions workflows, while `gh run` can be used to monitor, inspect, rerun, and troubleshoot workflow executions. Together, they help automate, track, and maintain CI/CD pipelines directly from the terminal.**

## Task 6: Useful gh Tricks

- Explore and try these — add the ones you find useful to your git-commands.md:

Used `gh api` to interact directly with GitHub REST and GraphQL APIs from the terminal for automation and advanced repository management.

```bash
ufiyan@Khan:~/github-cli-practice$ gh api repos/sk7652183-rgb/github-cli-practice
{
  "id": 1280506217,
  "node_id": "R_kgDOTFL5aQ",
  "name": "github-cli-practice",
  "full_name": "sk7652183-rgb/github-cli-practice",
  "private": false,
  "owner": {
    "login": "sk7652183-rgb",
    "id": 250090802,
    "node_id": "U_kgDODugVMg",
    "avatar_url": "https://avatars.githubusercontent.com/u/250090802?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/sk7652183-rgb",
    "html_url": "https://github.com/sk7652183-rgb",
    "followers_url": "https://api.github.com/users/sk7652183-rgb/followers",
    "following_url": "https://api.github.com/users/sk7652183-rgb/following{/other_user}",
    "gists_url": "https://api.github.com/users/sk7652183-rgb/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/sk7652183-rgb/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/sk7652183-rgb/subscriptions",
    "organizations_url": "https://api.github.com/users/sk7652183-rgb/orgs",
    "repos_url": "https://api.github.com/users/sk7652183-rgb/repos",
    "events_url": "https://api.github.com/users/sk7652183-rgb/events{/privacy}",
    "received_events_url": "https://api.github.com/users/sk7652183-rgb/received_events",
    "type": "User",
    "user_view_type": "public",
    "site_admin": false
  },
  "html_url": "https://github.com/sk7652183-rgb/github-cli-practice",
  "description": null,
  "fork": false,
  "url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice",
  "forks_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/forks",
  "keys_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/keys{/key_id}",
  "collaborators_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/collaborators{/collaborator}",
  "teams_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/teams",
  "hooks_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/hooks",
  "issue_events_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/issues/events{/number}",
  "events_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/events",
  "assignees_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/assignees{/user}",
  "branches_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/branches{/branch}",
  "tags_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/tags",
  "blobs_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/git/blobs{/sha}",
  "git_tags_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/git/tags{/sha}",
  "git_refs_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/git/refs{/sha}",
  "trees_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/git/trees{/sha}",
  "statuses_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/statuses/{sha}",
  "languages_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/languages",
  "stargazers_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/stargazers",
  "contributors_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/contributors",
  "subscribers_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/subscribers",
  "subscription_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/subscription",
  "commits_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/commits{/sha}",
  "git_commits_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/git/commits{/sha}",
  "comments_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/comments{/number}",
  "issue_comment_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/issues/comments{/number}",
  "contents_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/contents/{+path}",
  "compare_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/compare/{base}...{head}",
  "merges_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/merges",
  "archive_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/{archive_format}{/ref}",
  "downloads_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/downloads",
  "issues_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/issues{/number}",
  "pulls_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/pulls{/number}",
  "milestones_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/milestones{/number}",
  "notifications_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/notifications{?since,all,participating}",
  "labels_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/labels{/name}",
  "releases_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/releases{/id}",
  "deployments_url": "https://api.github.com/repos/sk7652183-rgb/github-cli-practice/deployments",
  "created_at": "2026-06-25T16:47:38Z",
  "updated_at": "2026-06-25T17:42:55Z",
  "pushed_at": "2026-06-25T18:44:54Z",
  "git_url": "git://github.com/sk7652183-rgb/github-cli-practice.git",
  "ssh_url": "git@github.com:sk7652183-rgb/github-cli-practice.git",
  "clone_url": "https://github.com/sk7652183-rgb/github-cli-practice.git",
  "svn_url": "https://github.com/sk7652183-rgb/github-cli-practice",
  "homepage": null,
  "size": 3,
  "stargazers_count": 0,
  "watchers_count": 0,
  "language": null,
  "has_issues": true,
  "has_projects": true,
  "has_downloads": true,
  "has_wiki": true,
  "has_pages": false,
  "has_discussions": false,
  "forks_count": 0,
  "mirror_url": null,
  "archived": false,
  "disabled": false,
  "open_issues_count": 0,
  "license": null,
  "allow_forking": true,
  "is_template": false,
  "web_commit_signoff_required": false,
  "has_pull_requests": true,
  "pull_request_creation_policy": "all",
  "topics": [],
  "visibility": "public",
  "forks": 0,
  "open_issues": 0,
  "watchers": 0,
  "default_branch": "main",
  "permissions": {
    "admin": true,
    "maintain": true,
    "push": true,
    "triage": true,
    "pull": true
  },
  "temp_clone_token": "",
  "allow_squash_merge": true,
  "allow_merge_commit": true,
  "allow_rebase_merge": true,
  "allow_auto_merge": false,
  "delete_branch_on_merge": false,
  "allow_update_branch": false,
  "use_squash_pr_title_as_default": false,
  "squash_merge_commit_message": "COMMIT_MESSAGES",
  "squash_merge_commit_title": "COMMIT_OR_PR_TITLE",
  "merge_commit_message": "PR_TITLE",
  "merge_commit_title": "MERGE_MESSAGE",
  "security_and_analysis": {
    "secret_scanning": {
      "status": "enabled"
    },
    "secret_scanning_push_protection": {
      "status": "enabled"
    },
    "dependabot_security_updates": {
      "status": "disabled"
    },
    "secret_scanning_non_provider_patterns": {
      "status": "disabled"
    },
    "secret_scanning_validity_checks": {
      "status": "disabled"
    }
  },
  "network_count": 0,
  "subscribers_count": 0
}
```
A GitHub Gist is a simple way to share code snippets, notes, configuration files, or small pieces of text on GitHub without creating a full repository.
Created and shared a GitHub Gist from the terminal using gh gist create, and learned that the file must exist before creating the gist.
```bash
sufiyan@Khan:~/github-cli-practice$ gh gist create README.md --public
- Creating gist README.md
✓ Created public gist README.md
https://gist.github.com/sk7652183-rgb/0438d4ccda8e59a1b61cf393894ae960
sufiyan@Khan:~/github-cli-practice$ 
```
gh release — create and manage releases
```bash
sufiyan@Khan:~/github-cli-practice$ gh release create v1.0.0
? Title (optional) v1.0.0
? Release notes Write using generated notes as template
? Is this a prerelease? Yes
? Submit? Publish release
https://github.com/sk7652183-rgb/github-cli-practice/releases/tag/v1.0.0
sufiyan@Khan:~/github-cli-practice$ gh release view v1.0.0
v1.0.0
Pre-release • sk7652183-rgb released this less than a minute ago

  ## What's Changed                                                                                                   
                                                                                                                      
  • Update README by @sk7652183-rgb in https://github.com/sk7652183-rgb/github-cli-practice/pull/1                    
                                                                                                                      
  ## New Contributors                                                                                                 
                                                                                                                      
  • @sk7652183-rgb made their first contribution in https://github.com/sk7652183-rgb/github-cli-practice/pull/1       
                                                                                                                      
  Full Changelog: https://github.com/sk7652183-rgb/github-cli-practice/commits/v1.0.0                                 


View on GitHub: https://github.com/sk7652183-rgb/github-cli-practice/releases/tag/v1.0.0

```
gh alias — create shortcuts for commands you use often
```bash
sufiyan@Khan:~/github-cli-practice$ gh alias set runs 'run list'
- Creating alias for runs: run list
✓ Added alias runs
sufiyan@Khan:~/github-cli-practice$ gh runs
no runs found
sufiyan@Khan:~/github-cli-practice$ gh alias list
co: pr checkout
runs: run list
```
Used gh search repos to find GitHub repositories by keyword and filter results based on criteria such as language, stars, and repository metadata directly from the terminal.
```bash
sufiyan@Khan:~/github-cli-practice$ gh search repos kubernetes

Showing 30 of 251563 repositories

NAME                                                    DESCRIPTION                                                                  VISIBILITY        UPDATED               
kubernetes/kubernetes                                   Production-Grade Container Scheduling and Management                         public            less than a minute ago
kubernetes/minikube                                     Run Kubernetes locally                                                       public            about 11 hours ago
helm/helm                                               The Kubernetes Package Manager                                               public            about 1 hour ago
k3s-io/k3s                                              Lightweight Kubernetes                                                       public            about 45 minutes ago
kubernetes/community                                    Kubernetes community content                                                 public            about 40 minutes ago
kelseyhightower/kubernetes-the-hard-way                 Bootstrap Kubernetes the hard way. No scripts.                               public            about 26 minutes ago
helm/charts                                             ⚠️(OBSOLETE) Curated applications for Kubernetes                             public, archived  about 9 hours ago
justmeandopensource/kubernetes                          Kubernetes playground                                                        public            about 10 days ago
kubernetes/website                                      Kubernetes website and documentation repo:                                   public            about 4 hours ago
kubernetes/ingress-nginx                                Ingress NGINX Controller for Kubernetes                                      public, archived  about 10 hours ago
argoproj/argo-cd                                        Declarative Continuous Deployment for Kubernetes                             public            about 2 hours ago
kodekloudhub/certified-kubernetes-administrator-course  Certified Kubernetes Administrator - CKA Course                              public            about 7 hours ago
kubernetes-sigs/kind                                    Kubernetes IN Docker - local clusters for testing Kubernetes                 public            about 2 hours ago
argoproj/argo-workflows                                 Workflow Engine for Kubernetes                                               public            about 2 hours ago
kubernetes/autoscaler                                   Autoscaling components for Kubernetes                                        public            about 9 hours ago
kubernetes-sigs/kubespray                               Deploy a Production Ready Kubernetes Cluster                                 public            about 4 hours ago
kubernetes/examples                                     Kubernetes application example tutorials                                     public            about 2 days ago
rook/rook                                               Storage Orchestration for Kubernetes                                         public            about 2 hours ago
kubernetes/client-go                                    Go client for Kubernetes.                                                    public            about 12 hours ago
feiskyer/kubernetes-handbook                            Kubernetes Handbook （Kubernetes指南） https://kubernetes.feisky.xyz         public            about 13 hours ago
stacksimplify/azure-aks-kubernetes-masterclass          Azure AKS Kubernetes Masterclass                                             public            about 4 days ago
stacksimplify/kubernetes-fundamentals                   Kubernetes Fundamentals                                                      public            about 3 days ago
prometheus-operator/kube-prometheus                     Use Prometheus to monitor Kubernetes and applications running on Kubernetes  public            about 1 day ago
portainer/portainer                                     Making Docker and Kubernetes management easy.                                public            about 1 hour ago
kubeflow/kubeflow                                       Machine Learning Toolkit for Kubernetes                                      public            about 8 hours ago
kubernetes-sigs/kustomize                               Customization of kubernetes YAML configurations                              public            about 1 day ago
kubernetes-retired/dashboard                            General-purpose web UI for Kubernetes clusters                               public, archived  about 1 day ago
wardviaene/kubernetes-course                            Kubernetes Course Files                                                      public            about 2 days ago
stacksimplify/aws-eks-kubernetes-masterclass            AWS EKS Kubernetes - Masterclass | DevOps, Microservices                     public            about 4 hours ago
rootsongjc/kubernetes-handbook                          Kubernetes 架构与生态：从云原生到 AI 原生基础设施的构建指南                  public            about 2 hours ago
sufiyan@Khan:~/github-cli-practice$ 
```
