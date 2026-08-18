# Day 43 – Jobs, Steps, Env Vars & Conditionals 

## Task 1: Multi-Job Workflow

### Created .github/workflows/multi-job.yml with three jobs—build to print "Building the app", test to print "Running tests" only after build succeeds, and deploy to print "Deploying" only after test succeeds—then verify the dependency chain in the Actions tab.

```yaml
name: To run the multi-job
on:
  push

jobs:
  build:
   runs-on: ubuntu-latest
   steps:
     - name: Checkout the code
       uses: actions/checkout@v7
     - name: Building the Application
       run: echo "This is the Building Stage"

  Test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Checkout the code
        uses: actions/checkout@v7
      - name: Testing the application
        run: echo "This is the testing stage"

  Deploy:
    runs-on: ubuntu-latest
    needs: Test
    steps:
    - name: Checkout the code
      uses: actions/checkout@v7
    - name: Deploying the application
      run: echo "This is the Deploying Stage"
      
```

<img width="1365" height="729" alt="image" src="https://github.com/user-attachments/assets/bfb7ac9a-b176-430e-b7ae-66c4f536c544" />

## Task 2: Environment Variables

### In a new workflow, use environment variables at 3 levels:

### Created a new GitHub Actions workflow using environment variables at three levels—workflow level (APP_NAME: myapp), job level (ENVIRONMENT: staging), and step level (VERSION: 1.0.0)—print all three variables in a single step to verify they are accessible, and then use GitHub context variables to print the commit SHA and the actor who triggered the workflow.

```yaml

name: Complete Workflow
on:
  push: 
env:
 APP_NAME: myapp

jobs:
  test:
   runs-on: ubuntu-latest
   env:
       Environment: stagging

   steps:
     - name: Print Environment Variables and GitHub Context
       env:
        VERSION: 1.0.0
       run: |
          echo "APP_NAME: $APP_NAME"
          echo "ENVIRONMENT: $ENVIRONMENT"
          echo "VERSION: $VERSION"
          echo "Commit SHA: ${{ github.sha }}"
          echo "Actor: ${{ github.actor }}"

```

<img width="1364" height="722" alt="image" src="https://github.com/user-attachments/assets/bc4473e1-9b6c-4c31-b141-fdfd68001995" />

## Task 3: Job Outputs

### Create a workflow with one job that sets an output, such as today’s date as a string, and a second job that reads and prints that output using outputs: and needs.<job>.outputs.<name>.

```yaml
name: Pass Date Between Jobs
on: 
  push:

jobs:
  # First job generates the date string
  generate-date:
    runs-on: ubuntu-latest
    outputs:
      today_date: ${{steps.set-date.outputs.date_string }}
    steps:
      - name: Generate date string
        id: set-date
        run: echo "date_string=$(date +'%Y-%m-%d')" >> $GITHUB_OUTPUT

    # Second job reads and prints the date string
  print-date:
    runs-on: ubuntu-latest
    needs: generate-date
    steps:
      - name: Read and print the output
        run: echo "The date from from the previous job is  ${{ needs.generate-date.outputs.today_date }}"
        
```
### Write in your notes: Why would you pass outputs between jobs?
We would pass outputs between jobs when one job needs to share information or a value with another job. For example, the first job can generate today’s date, and the second job can use that date.

## Task 4: Conditionals

### In a workflow, add a step that runs only on the main branch, a step that runs only when the previous step fails, a job that runs only on push events and not on pull requests, and a step with continue-on-error: true to understand how it allows the workflow to continue even if that step fails.

```yaml

name: Conditional Workflow
on:
  push:
   branches: main
  pull_request:
    branches: main

jobs:
  conditional-job:
  # 3. Only runs on push events, not on pull requests
   if: github.event_name == 'push'
   runs-on: ubuntu-latest

   steps:
     - name: Checkout the codes
       uses: actions/checkout@v4

  # 1. Only runs when the branch is main
     - name: Main Branch Only Step
       if: github.ref_name == 'main'
       run: echo " This step runs because the branch is main"

  # 4. Step with continue-on-error: true
     - name: Risky Step
       id: risky-step
       continue-on-error: true
       run: |
        echo "This step will fail on purpose."
        exit 1
    # 2. Only runs when the previous step failed
     - name: Fallback Step On Failure
       if: steps.risky-step.outcome == 'failure'
       run: echo "The previous step failed, but the job continued anyway!"

```

## Task 5: Putting It Together

Created .github/workflows/smart-pipeline.yml that:

### Created a GitHub Actions workflow that triggers on every push, runs lint and test jobs in parallel, and then runs a summary job after both jobs to display whether the push was made to the main or a feature branch and print the commit message.

```yaml

name: Smart Pipeline

on:
  push:

jobs:
  # 1. Lint Job
  lint:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run Linter
        run: echo "Running lint checks..."

  # 2. Test Job
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run Tests
        run: echo "Running tests..."

  # 3. Summary Job
  summary:
    runs-on: ubuntu-latest
    needs: [lint, test]

    steps:
      - name: Print Branch Information
        run: |
          if [ "${{ github.ref_name }}" = "main" ]; then
            echo "Branch Type: Main branch push"
          else
            echo "Branch Type: Feature branch push"
          fi
          echo "Branch Name: ${{ github.ref_name }}"
      - name: Print Commit Message
        run: |
          echo "Commit Message:"
          echo "${{ github.event.head_commit.message }}"

```
