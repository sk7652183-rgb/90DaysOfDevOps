# Day 40 – First GitHub Actions Workflow

## Task 1: Set Up

### Create a new public GitHub repository called github-actions-practice, clone it locally, and create the folder structure .github/workflows/.

<img width="1365" height="732" alt="image" src="https://github.com/user-attachments/assets/e913d822-7d13-4d1e-8e0d-27b7a1b48e25" />


## Task 2: Hello Workflow

Created .github/workflows/hello.yml with a workflow that:

### Triggers on every push, has one job called greet that runs on ubuntu-latest, and includes two steps: checking out the code using actions/checkout and printing Hello from GitHub Actions!.

```yaml

name: To print Hello from GitHub Actions!

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Run a simple script
        run: echo "Hello from GitHub Actions!"
```


### Push it. Go to the Actions tab on GitHub and watch it run.
<img width="1365" height="728" alt="image" src="https://github.com/user-attachments/assets/aa53d79a-9255-4b45-9da8-08501e24400b" />

### Verify: Is it green? Click into the job and read every step.

<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/d49c272c-bb22-47e1-9cee-c031fed63ac6" />

## Task 3: Understand the Anatomy

Look at your workflow file and write in your notes what each key does:

### GitHub Actions YAML Keywords

* `on:` Specifies the event(s) that trigger the GitHub Actions workflow, such as `push` or `pull_request`.
* `jobs:` Defines the jobs that need to be performed in the workflow.
* `runs-on:` Specifies the runner or operating system where the job will run, such as `ubuntu-latest`.
* `steps:` Defines the individual steps or tasks to be executed within a job.
* `uses:` Specifies a pre-built GitHub Action to use in a step, such as `actions/checkout@v4`.
* `run:` Specifies a shell command or script to execute on the runner.
* `name:` Specifies a descriptive name for the workflow or step.

## Task 4: Add More Steps

Updated hello.yml to also:

### Print the current date and time, the name of the branch that triggered the run, the files in the repository, and the runner’s operating system.

```yaml

name: To print Hello from GitHub Actions!

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Run a simple script
        run: echo "Hello from GitHub Actions!"

      - name: Print current date and time
        run: echo "The current date and time is $(date +%T)"

      - name: Print branch name
        run: echo "The branch is ${{ github.ref_name }}"

      - name: List the files in the repo
        run: git ls-files

      - name: Print the runner's operating system
        shell: bash
        run: |
          echo "$RUNNER_OS"

```

### Push again — watch the new run.
<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/8501169b-545f-4062-a925-b0989bb603e8" />


## Task 5: Break It On Purpose

### Add a step that runs a command that will fail (e.g., exit 1 or a misspelled command)

```yaml

name: To print Hello from GitHub Actions!

on:
  push:

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Run a simple script
        run: echo "Hello from GitHub Actions!"
      - name: To current date and time
        run: echo "The current date and time is $(date +%T)"
      - name: Print branch name
        run: echo "The branch is ${{ github.ref_name}}"
      - name: List the files in the repo
        run:  git ls-files
      - name: Print the runner's operating system
        shell: bash
        run: |
         echo  "$RUNNER_OS"
      - name: Run a failing command
        run: exit 1

```

### Push and observe what happens in the Actions tab

<img width="1365" height="724" alt="image" src="https://github.com/user-attachments/assets/1149438c-3a2c-400d-827c-4e338eefa05b" />

### Fix it and push again

<img width="1365" height="728" alt="image" src="https://github.com/user-attachments/assets/a490b128-0511-46a1-8533-68ac8a75062c" />


### What does a failed pipeline look like?

A failed pipeline shows a **red ❌** in GitHub Actions. The failed step is highlighted in red.

### How do you read the error?

* Open the failed workflow.
* Click on the failed step.
* Read the **error message** shown there.
* The error message tells you what went wrong.
* Fix the problem and run the workflow again.








