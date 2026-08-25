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



<img width="1362" height="721" alt="image" src="https://github.com/user-attachments/assets/df4af218-4657-4cac-9226-b8a790a254e2" />

