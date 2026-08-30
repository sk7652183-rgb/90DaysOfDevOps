# Day 48 – GitHub Actions Project: End-to-End CI/CD Pipeline

## Task 1: Set Up the Project Repo

### Created a new repository with a simple Python Flask/FastAPI app containing one endpoint, added a Dockerfile and a basic test, and created a README.md with a project description.

## 🔗 Project Repository

You can find the complete source code here:

👉 [Shopping App - GitHub Repository](https://github.com/sk7652183-rgb/shopping-app.git)

## Task 2: Reusable Workflow — Build & Test

### Created .github/workflows/reusable-build-test.yml:

### Create a reusable GitHub Actions workflow triggered by `workflow_call` that accepts `python_version` (or `node_version`) and a `run_tests` boolean input with a default value of `true`. The workflow should check out the code, set up the required language runtime, install the dependencies, run the tests only when `run_tests` is `true`, and set a `test_result` output with a value of either `passed` or `failed`.

```yaml
name: Reusable Python Test Workflow

on:
  workflow_call:
    inputs:
      python_version:
        description: "Python version"
        required: false
        type: string
        default: "3.12"

      run_tests:
        description: "Run tests"
        required: false
        type: boolean
        default: true

    outputs:
      test_result:
        description: "Test result"
        value: ${{ jobs.test.outputs.test_result }}

jobs:
  test:
    runs-on: ubuntu-latest

    outputs:
      test_result: ${{ steps.test.outputs.test_result }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ inputs.python_version }}

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          if [ -f requirements.txt ]; then
            pip install -r requirements.txt
          fi

      - name: Run tests
        id: test
        run: |
          if [ "${{ inputs.run_tests }}" = "true" ]; then
            if pytest; then
              echo "test_result=passed" >> "$GITHUB_OUTPUT"
            else
              echo "test_result=failed" >> "$GITHUB_OUTPUT"
              exit 1
            fi
          else
            echo "Tests skipped"
            echo "test_result=passed" >> "$GITHUB_OUTPUT"
          fi

```

## Task 3: Reusable Workflow — Docker Build & Push

### Created .github/workflows/reusable-docker.yml:

### Create a reusable GitHub Actions workflow triggered by workflow_call that accepts image_name and tag as string inputs and docker_username and docker_token as secrets. The workflow should check out the code, log in to Docker Hub using the provided credentials, build and push the Docker image with the specified tag, and set an image_url output containing the full image path.

```yaml

name: Reusable Docker Workflow

on:
  workflow_call:
    inputs:
      image_name:
        description: "Name of the Docker image"
        required: true
        type: string

      tag:
        description: "Tag for the Docker image"
        required: true
        type: string

    secrets:
      docker_token:
        required: true

    outputs:
      image_url:
        description: "Full Docker image path"
        value: ${{ jobs.docker.outputs.image_url }}

jobs:
  docker:
    runs-on: ubuntu-latest

    outputs:
      image_url: ${{ steps.build.outputs.image_url }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.docker_username }}
          password: ${{ secrets.docker_token }}

      - name: Build and Push Docker Image
        id: build
        run: |
          IMAGE_URL="${{ vars.docker_username }}/${{ inputs.image_name }}:${{ inputs.tag }}"

          docker build -t "$IMAGE_URL" .
          docker push "$IMAGE_URL"

          echo "image_url=$IMAGE_URL" >> "$GITHUB_OUTPUT"

```

## Task 4: PR Pipeline

### Create .github/workflows/pr-pipeline.yml:

### Create a GitHub Actions workflow triggered by pull_request events targeting the main branch, specifically when a PR is opened or synchronize. The workflow should call the reusable build-test workflow with run_tests set to true, followed by a standalone pr-comment job that runs after the build-test job and prints the summary, "PR checks passed for branch: <branch>". Docker images must not be built or pushed during pull requests.

```yaml
name: PR Checks

on:
  pull_request:
    branches:
      - main
    types:
      - opened
      - synchronize

jobs:
  build-test:
    uses: ./.github/workflows/reusable-build-test.yml
    with:
      run_tests: true

  pr-comment:
    needs: build-test
    runs-on: ubuntu-latest

    steps:
      - name: PR Summary
        run: |
          echo "PR checks passed for branch: ${{ github.head_ref }}"
```
### Verify: Open a PR — does it run tests only (no Docker push)?

<img width="1365" height="725" alt="image" src="https://github.com/user-attachments/assets/c5ee0d67-7fb3-4b30-b09a-e30f80fea1a1" />

<img width="1362" height="723" alt="image" src="https://github.com/user-attachments/assets/74d8c6fd-2f44-4740-bd30-f9454230400a" />

<img width="1365" height="728" alt="image" src="https://github.com/user-attachments/assets/a5d21cf8-3d47-405b-95a1-b649ac3454d2" />

## Task 5: Main Branch Pipeline

### Created .github/workflows/main-pipeline.yml:

```yaml

name: Main-pipeline

on:
  push:
    branches:
      - main

jobs:

  # Job 1: Build and Test
  build-test:
    uses: ./.github/workflows/reusable-build-test.yml
    with:
      python_version: "3.12"
      run_tests: true

  # Job 2: Build and Push Docker Image
  Docker_login:
    needs: build-test
    uses: ./.github/workflows/reusable-docker.yml
    with:
      image_name: shopping-app
      tag: sha-${{ github.sha }}
    secrets:
      docker_token: ${{ secrets.DOCKER_TOKEN }}

  # Job 3: Deploy
  Deploy:
    needs: Docker_login
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Checkout the Code
        uses: actions/checkout@v4

      - name: Log Deployment
        run: |
          IMAGE_URL="${{ needs.Docker_login.outputs.image_url }}"

          echo "Deploying image: ${IMAGE_URL} to production"
```


### The pipeline is triggered by a push to main, calls the reusable build-test workflow as Job 1, calls the reusable Docker workflow as Job 2 after Job 1 with latest and sha-<short-commit-hash> tags, and then runs the deploy job as Job 3 after Job 2 to print Deploying image: <image_url> to production, using the production environment with optional manual approval configured through repository environment protection rules.

<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/846c845f-049f-4ea6-8b01-6e02aeb509c0" />

<img width="1365" height="725" alt="image" src="https://github.com/user-attachments/assets/a291df97-7c8a-483f-96d1-efb0805cf44b" />

<img width="1365" height="715" alt="image" src="https://github.com/user-attachments/assets/8e1758a4-0345-40fb-bf16-f58c1d990294" />

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/58a4d705-8c35-4449-8975-7106750b3d8f" />


### Verify: Merge a PR to main — does it run tests → build Docker → deploy in sequence?

<img width="1364" height="728" alt="image" src="https://github.com/user-attachments/assets/74d143ea-94bf-4708-b660-9d255f5e1fd8" />



## Task 6: Scheduled Health Check

### Created .github/workflows/health-check.yml:

### Configured a scheduled GitHub Actions health-check workflow to run every 12 hours, with manual triggering support, which pulls the latest Docker image, runs the container in detached mode, waits 5 seconds, checks the health endpoint using curl, reports a pass/fail status based on the response, and finally stops and removes the container.


<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/b22d1a3d-0783-44fc-80bd-486b3c7ee8b7" />


## Task 7: Add Badges & Documentation

### Add status badges for all your workflows to the repo README.md

[![Health Check](https://github.com/sk7652183-rgb/shopping-app/actions/workflows/health-check.yml/badge.svg)](https://github.com/sk7652183-rgb/shopping-app/actions/workflows/health-check.yml)

[![Main-pipeline](https://github.com/sk7652183-rgb/shopping-app/actions/workflows/main-pipeline.yml/badge.svg)](https://github.com/sk7652183-rgb/shopping-app/actions/workflows/main-pipeline.yml)

### Add a pipeline architecture diagram in your notes — draw (or describe) the flow

                         ┌─────────────────┐
                         │    Developer    │
                         └────────┬────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
                PR Opened                   Merge to main
                    │                           │
                    ▼                           ▼
             ┌──────────────┐           ┌──────────────┐
             │ Build & Test │           │ Build & Test │
             └──────┬───────┘           └──────┬───────┘
                    │                           │
                    ▼                           ▼
             ┌──────────────┐           ┌─────────────────┐
             │ PR Checks    │           │ Docker Build &  │
             │    PASS      │           │      Push       │
             └──────────────┘           └────────┬────────┘
                                                  │
                                                  ▼
                                           ┌──────────────┐
                                           │    Deploy    │
                                           │  Production  │
                                           └──────────────┘


                    Every 12 Hours
                          │
                          ▼
                  ┌──────────────┐
                  │ Health Check │
                  │ Docker Image │
                  └──────┬───────┘
                         │
                         ▼
                  ┌──────────────┐
                  │ PASS / FAIL  │
                  └──────────────┘

### Fill in your notes: What would you add next? (Slack notifications? Multi-environment? Rollback?)

```markdown
## 🚀 Future Improvements

The next improvements I would add to this CI/CD pipeline are:

- 🔔 **Slack Notifications** — Send notifications for pipeline success, failure, and deployment status.
- 🌍 **Multi-Environment Deployment** — Introduce Dev → Staging → Production environments with approval gates.
- 🔄 **Automated Rollback** — Automatically roll back to the previous working Docker image if a production deployment or health check fails.
- 🔐 **Security Scanning** — Integrate tools such as Trivy to scan Docker images for vulnerabilities before pushing them.
- 📊 **Monitoring & Alerting** — Add Prometheus and Grafana for application and infrastructure monitoring.
```



