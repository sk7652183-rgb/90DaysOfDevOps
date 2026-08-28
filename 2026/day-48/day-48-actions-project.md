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

## Task 5: Main Branch Pipeline

### Created .github/workflows/main-pipeline.yml:



<img width="1365" height="728" alt="image" src="https://github.com/user-attachments/assets/a5d21cf8-3d47-405b-95a1-b649ac3454d2" />


