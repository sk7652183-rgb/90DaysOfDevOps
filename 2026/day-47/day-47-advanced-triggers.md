# Day 47 – Advanced Triggers: PR Events, Cron Schedules & Event-Driven Pipelines

## Task 1: Pull Request Event Types

### Create .github/workflows/pr-lifecycle.yml that triggers on pull_request with specific activity types:

### Create a GitHub Actions workflow that triggers on opened, synchronize, reopened, and closed pull request events, prints the event type, PR title, PR author, source branch, and target branch, and includes a conditional step that runs only when the PR is merged (closed + merged == true).

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/8d04cfc5-c77a-49ac-ab18-777a10113ba2" />


## Task 2: PR Validation Workflow

### Create .github/workflows/pr-checks.yml as a real-world PR gate that triggers on pull requests to main, checks out the code and fails if any file in the PR is larger than 1 MB, validates the branch name from ${{ github.head_ref }} to ensure it follows the feature/*, fix/*, or docs/* pattern, and checks the PR body from ${{ github.event.pull_request.body }}, warning without failing if the PR description is empty.

Verify: Open a PR from a badly named branch — does the check fail?

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/99617799-b628-47a9-88e2-413af5bc5dd1" />

<img width="1299" height="592" alt="image" src="https://github.com/user-attachments/assets/37f16d76-58f6-4a39-91bf-54837d228ed5" />

<img width="1365" height="725" alt="Screenshot 2026-08-25 161926" src="https://github.com/user-attachments/assets/af6a0b81-1d0e-4c9d-a028-69a39c8ee2c2" />

<img width="1363" height="727" alt="Screenshot 2026-08-25 161908" src="https://github.com/user-attachments/assets/69ff4eff-a9d8-4f24-a979-18a23a5406ad" />


## Task 3: Scheduled Workflows (Cron Deep Dive)

### Create .github/workflows/scheduled-tasks.yml with two scheduled cron triggers—30 2 * * 1 to run every Monday at 2:30 AM UTC and 0 */6 * * * to run every 6 hours—print the schedule that triggered the workflow using ${{ github.event.schedule }}, and add a health-check step that uses curl to check a URL and validates the HTTP response code.


<img width="1365" height="724" alt="image" src="https://github.com/user-attachments/assets/1b4eddb3-0a1a-42e9-9a09-396690e13469" />

## Write in your notes:

### The cron expression for: every weekday at 9 AM IST - 0 9 * * 1-5

### The cron expression for: first day of every month at midnight -  0 0 1 * *

### Scheduled Workflow Limitations

GitHub scheduled workflows may be delayed or skipped due to system load. In public repositories, scheduled workflows can also be automatically disabled after 60 days of inactivity.

**Tip:** Avoid scheduling workflows exactly at the start of the hour to reduce potential delays.


## Task 4: Path & Branch Filters

Created .github/workflows/smart-triggers.yml:

### Configure push triggers for changes in src/ or app/, use paths-ignore in a second workflow to skip runs for documentation-only changes (*.md and docs/**), add branch filters for main and release/*, and test by pushing a .md file to verify that the workflow is skipped.

```yaml

name: smart-triggers

on:
  push:
    branches:
      - main
    paths:
      - 'src/**'
      - 'app/**'

  pull_request:
    branches:
      - release
    paths-ignore:
      - 'docs/**'
      - '*.md'

jobs:
  smart-triggers:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the Code
        uses: actions/checkout@v4

      - name: Print message
        run: echo "Workflow triggered successfully"

```


<img width="1362" height="721" alt="image" src="https://github.com/user-attachments/assets/df4af218-4657-4cac-9226-b8a790a254e2" />

<img width="1365" height="722" alt="image" src="https://github.com/user-attachments/assets/2061c6bc-553c-4b3c-a67e-1a0aef6b3645" />

### Write in your notes: When would you use paths vs paths-ignore?

Use paths to trigger workflows only for specific files/directories, and paths-ignore to skip workflows when changes affect specific files/directories.

## Task 5: workflow_run — Chain Workflows Together

### In short: tests.yml runs tests on every push, deploy-after-tests.yml triggers when tests.yml completes, deploys only when the test conclusion is success, and otherwise prints a warning and exits with an error.

```yaml
name: Run Tests

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Run Tests
        run: echo "Tests completed successfully"
```

```yaml
name: Deploy After Tests

on:
  workflow_run:
    workflows: ["Run Tests"]
    types: [completed]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Check Test Result
        if: ${{ github.event.workflow_run.conclusion == 'success' }}
        run: echo "Tests passed. Proceeding with deployment."

      - name: Deploy
        if: ${{ github.event.workflow_run.conclusion == 'success' }}
        run: echo "Deploying application..."

      - name: Warning - Tests Failed
        if: ${{ github.event.workflow_run.conclusion != 'success' }}
        run: |
          echo "WARNING: Tests failed. Deployment skipped."
          exit 1
```

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/bdc1ae2e-fc1b-4a14-8b6c-cd6ee64135c6" />


<img width="1365" height="724" alt="image" src="https://github.com/user-attachments/assets/a57da2b4-fb64-4b0b-b765-9a2d37eaf58b" />

Yes. Push a commit and verify that Run Tests executes first; once it completes, Deploy After Tests is triggered, and deployment proceeds only if the tests succeed.

## Task 6: repository_dispatch — External Event Triggers

### Create .github/workflows/external-trigger.yml with a repository_dispatch trigger for deploy-request, print the environment from client_payload, and trigger it using the provided gh api command with environment: production.

```yaml

name: External Trigger

on:
  repository_dispatch:
    types: [deploy-request]

jobs:
  deploy-request:
    runs-on: ubuntu-latest

    steps:
      - name: Print Environment
        run: echo "Deployment requested for ${{ github.event.client_payload.environment }}"

<img width="1365" height="728" alt="image" src="https://github.com/user-attachments/assets/5aa41352-fdfb-47df-baa9-2eedc0be8b33" />

```

### Write in your notes: When would an external system (like a Slack bot or monitoring tool) trigger a pipeline?

An external system triggers a pipeline when an event occurs outside GitHub, such as a deployment request, monitoring alert, or Slack command.


