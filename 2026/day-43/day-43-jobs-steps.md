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


