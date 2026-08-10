Day 39 – What is CI/CD?
📝 Task 1: The Problem

Imagine a team of 5 developers all pushing code to the same repository and manually deploying to production.

❓ What can go wrong?

With 5 developers manually pushing and deploying to production:

⚠️ Merge conflicts
⚠️ Wrong code or commit gets deployed
⚠️ One deployment can overwrite another
⚠️ Production bugs from untested changes
⚠️ Database migrations can break existing code
⚠️ Hotfixes can be overwritten
⚠️ Difficult to know which version is currently running in production

❓ What does "It works on my machine" mean?

Answer: It means the code works in one developer's local environment but may fail in production because the environments are different.

Common differences include:

📦 Dependencies
⚙️ Runtime versions
🔐 Environment variables
🗄️ Database
⚙️ Configuration
Example
Developer's Machine
        ↓
    Works ✅
        ↓
Different Production Environment
        ↓
     Fails ❌

❓ How many manual deployments are safe per day?

Answer: There is no fixed safe number.

The more manual deployments a team performs, the more opportunities there are for human error.

Instead of limiting deployments, we should automate the process:

Code
  ↓
Pull Request
  ↓
Automated Tests
  ↓
Merge
  ↓
CI/CD
  ↓
Production

💡 Goal: Deploy frequently while making deployments automated, tested, and repeatable.

## Task 2: CI vs CD

### Continuous Integration 

🔄 Continuous Integration (CI)

Continuous Integration (CI) is a set of automated steps or instructions that a pipeline follows whenever code changes are pushed to the repository.

A CI pipeline typically:

📥 Gets the latest code
📦 Installs dependencies
🧪 Runs tests
🔍 Runs linting or code checks
🏗️ Builds the application
Example
Developer pushes code
        ↓
   CI Pipeline
        ↓
Install Dependencies
        ↓
     Run Tests
        ↓
   Code Checks
        ↓
      Build
        ↓
   ✅ Pass / ❌ Fail


💡 In simple words: CI is an automated process that checks whether new code can safely be integrated with the existing codebase.

### Continuous Delivery

🚀 Continuous Delivery (CD)

Continuous Delivery (CD) is a method of automatically preparing and delivering an application so that it is always ready to be released to production.

The CD pipeline typically:

📦 Takes the code that passed CI
🏗️ Builds the application
📋 Packages the application
🚀 Deploys it to a staging or production environment
✅ Verifies that the deployment is successful
Code
  ↓
Continuous Integration (CI)
  ↓
Build & Test
  ↓
Continuous Delivery (CD)
  ↓
Staging / Production


💡 In simple words: CI checks and builds the code; CD prepares and delivers the application for release.

🚀 Continuous Deployment

Continuous Deployment is the practice of automatically deploying every code change that successfully passes the CI/CD pipeline directly to production.

Code Push
   ↓
CI
   ↓
Test & Build
   ↓
All Checks Pass ✅
   ↓
Automatic Deployment
   ↓
Production 🚀


💡 In simple words: Continuous Deployment automatically releases tested code to production without requiring manual approval.

🔑 Difference
Continuous Delivery → Code is automatically prepared and ready for release, but production deployment may require manual approval.
Continuous Deployment → Code is automatically deployed to production after passing all checks.

### Write one real-world example for each.

🔄 CI – Continuous Integration

Example:
A developer pushes code to GitHub. The CI pipeline automatically runs tests and checks the code.

Developer pushes code
        ↓
   Run Tests 🧪
        ↓
    Run Lint 🔍
        ↓
    Build App 🏗️
        ↓
   ✅ Pass / ❌ Fail


Example: GitHub Actions runs npm test whenever a developer creates a Pull Request.

📦 CD – Continuous Delivery

Example:
After CI passes, the application is automatically built and deployed to a staging environment. A developer manually approves the production release.

Code
 ↓
CI Tests ✅
 ↓
Build
 ↓
Staging 🚀
 ↓
Manual Approval 👤
 ↓
Production


Example: Every merge to main automatically deploys the application to staging, but a team member clicks "Deploy to Production".

🚀 Continuous Deployment

Example:
After CI passes, the application is automatically deployed directly to production without manual approval.

Code
 ↓
CI Tests ✅
 ↓
Build
 ↓
Automatic Deployment 🚀
 ↓
Production


Example: Every successful merge to main automatically deploys the latest version of the application to production.

📝 Task 3: Pipeline Anatomy

A CI/CD pipeline is made up of several parts. Each part has a specific responsibility.

1. 🔔 Trigger

What starts the pipeline?

A Trigger is an event that starts the pipeline.

Examples:

Code is pushed
Pull Request is created
Code is merged into main
Manual trigger
2. 📋 Stage

What is a logical phase?

A Stage is a logical phase of the pipeline, such as:

Build
Test
Deploy
Build → Test → Deploy

3. ⚙️ Job

What is a unit of work inside a stage?

A Job is a group of related tasks that are executed together inside a stage.

Example:

Test Stage
   ↓
Test Job
   ├── Install dependencies
   ├── Run unit tests
   └── Run lint

4. ▶️ Step

What is a single command or action?

A Step is one individual command or action inside a job.

Example:

npm install
npm test
npm run lint


Each command can be a separate step.

5. 🖥️ Runner

What machine executes the job?

A Runner is the machine or environment where the job runs.

It provides the environment needed to execute the pipeline commands.

Example:

Runner
  ↓
Runs: npm install
Runs: npm test
Runs: npm run build

6. 📦 Artifact

What is the output produced by a job?

An Artifact is a file or output produced by a job that can be used later in the pipeline or downloaded.

Examples:

Build files
.zip files
Docker images
Test reports
Compiled application files
Build Job
    ↓
Application Build
    ↓
📦 Artifact
    ↓
Deploy Stage

🔗 Pipeline Flow
🔔 Trigger
    ↓
📋 Stage
    ↓
⚙️ Job
    ↓
▶️ Steps
    ↓
🖥️ Runner
    ↓
📦 Artifact


💡 In simple words: A trigger starts the pipeline, stages organize the work, jobs perform groups of tasks, steps execute individual actions, runners provide the machine, and artifacts are the outputs.

## Task 4: Draw a Pipeline

### Draw a CI/CD pipeline for this scenario:

<img width="415" height="620" alt="image" src="https://github.com/user-attachments/assets/2ec6fceb-6904-4712-b4db-ecfa0a14530f" />


## Task 5: Explore in the Wild

# GitHub Actions Workflow Analysis

## Workflow

**Workflow Name:** `(Compiler) Discord Notify`

## Workflow YAML

```yaml
name: (Compiler) Discord Notify

on:
  pull_request_target:
    types: [opened, ready_for_review]
    paths:
      - compiler/**
      - .github/workflows/compiler_**.yml

permissions: {}

jobs:
  check_access:
    if: ${{ github.event.pull_request.draft == false }}
    runs-on: ubuntu-latest
    outputs:
      is_member_or_collaborator: ${{ steps.check_is_member_or_collaborator.outputs.is_member_or_collaborator }}

    steps:
      - run: echo ${{ github.event.pull_request.author_association }}

      - name: Check is member or collaborator
        id: check_is_member_or_collaborator
        if: ${{ github.event.pull_request.author_association == 'MEMBER' || github.event.pull_request.author_association == 'COLLABORATOR' }}
        run: echo "is_member_or_collaborator=true" >> "$GITHUB_OUTPUT"

  check_maintainer:
    if: ${{ needs.check_access.outputs.is_member_or_collaborator == 'true' || needs.check_access.outputs.is_member_or_collaborator == true }}
    needs: [check_access]
    uses: react/react/.github/workflows/shared_check_maintainer.yml@main
    permissions:
      # Used by check_maintainer
      contents: read
    with:
      actor: ${{ github.event.pull_request.user.login }}

  notify:
    if: ${{ needs.check_maintainer.outputs.is_core_team == 'true' }}
    needs: check_maintainer
    runs-on: ubuntu-latest

    steps:
      - name: Discord Webhook Action
        uses: tsickert/discord-webhook@86dc739f3f165f16dadc5666051c367efa1692f4
        with:
          webhook-url: ${{ secrets.COMPILER_DISCORD_WEBHOOK_URL }}
          embed-author-name: ${{ github.event.pull_request.user.login }}
          embed-author-url: ${{ github.event.pull_request.user.html_url }}
          embed-author-icon-url: ${{ github.event.pull_request.user.avatar_url }}
          embed-title: '#${{ github.event.number }} (+${{ github.event.pull_request.additions }} -${{ github.event.pull_request.deletions }}): ${{ github.event.pull_request.title }}'
          embed-description: ${{ github.event.pull_request.body }}
          embed-url: ${{ github.event.pull_request.html_url }}
```

---

## 1. What triggers it?

The workflow is triggered by a **Pull Request** using `pull_request_target`.

It runs when a Pull Request is:

* **Opened**
* **Marked as ready for review**

It only runs when the Pull Request contains changes in:

* `compiler/**`
* `.github/workflows/compiler_**.yml`

The workflow also checks:

```yaml
if: ${{ github.event.pull_request.draft == false }}
```

So the jobs do not proceed for a **draft Pull Request**.

---

## 2. How many jobs does it have?

The workflow contains **3 jobs**:

1. `check_access`
2. `check_maintainer`
3. `notify`

---

## 3. What does each job do?

### Job 1: `check_access`

The `check_access` job checks whether the Pull Request author is a:

* `MEMBER`
* `COLLABORATOR`

It checks:

```yaml
github.event.pull_request.author_association
```

If the author is a member or collaborator, it creates the output:

```text
is_member_or_collaborator=true
```

This output is then used by the next job.

---

### Job 2: `check_maintainer`

The `check_maintainer` job runs only when the previous job confirms that the PR author is a member or collaborator.

It uses a **reusable GitHub Actions workflow**:

```yaml
uses: react/react/.github/workflows/shared_check_maintainer.yml@main
```

It passes the PR author's GitHub username to the reusable workflow:

```yaml
with:
  actor: ${{ github.event.pull_request.user.login }}
```

The reusable workflow appears to determine whether the user belongs to the **core/maintainer team**.

---

### Job 3: `notify`

The `notify` job runs only when the previous job returns:

```text
is_core_team=true
```

It then sends a notification to **Discord** using the Discord Webhook Action:

```yaml
uses: tsickert/discord-webhook@86dc739f3f165f16dadc5666051c367efa1692f4
```

The Discord notification contains:

* PR author
* Author profile URL
* Author avatar
* PR number
* Number of additions
* Number of deletions
* PR title
* PR description
* PR URL

---

## 4. Overall Workflow

The workflow can be understood as:

```text
Pull Request opened / ready for review
                 |
                 v
        Is it a draft PR?
                 |
          No     |     Yes
          |             |
          v             X
    check_access
          |
          v
 Is author MEMBER or
    COLLABORATOR?
          |
      Yes |
          v
  check_maintainer
          |
          v
 Is author part of
   core team?
          |
      Yes |
          v
       notify
          |
          v
   Send PR details
      to Discord
```

---

## 5. What does it do? — Best Guess

The workflow is designed to **notify a Discord channel when an eligible core-team member opens or marks a Compiler-related Pull Request as ready for review**.

It first verifies the author's repository association, then checks whether the author belongs to the core/maintainer team, and finally sends the Pull Request information to Discord.

---

## 6. Summary

| Question                  | Answer                                                    |
| ------------------------- | --------------------------------------------------------- |
| **What triggers it?**     | Pull Request opened or marked ready for review            |
| **Event type**            | `pull_request_target`                                     |
| **Path filters**          | `compiler/**` and `.github/workflows/compiler_**.yml`     |
| **How many jobs?**        | 3                                                         |
| **Job 1**                 | `check_access`                                            |
| **Job 2**                 | `check_maintainer`                                        |
| **Job 3**                 | `notify`                                                  |
| **Main purpose**          | Check PR author access/team membership and notify Discord |
| **Notification platform** | Discord                                                   |
| **Reusable workflow**     | `shared_check_maintainer.yml`                             |
| **External action**       | `tsickert/discord-webhook`                                |
