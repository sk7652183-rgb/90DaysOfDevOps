# Day 46 – Reusable Workflows & Composite Actions

## Task 1: Understand workflow_call

### What is a reusable workflow?

A reusable workflow in GitHub Actions is a workflow that can be called by other workflows using workflow_call. It allows us to define common CI/CD logic once and reuse it across multiple repositories or environments, reducing duplication and improving consistency

### What is the workflow_call trigger?

workflow_call is a GitHub Actions trigger used to make a workflow reusable. It allows another workflow to invoke it and optionally pass inputs, secrets, and parameters. This is useful for avoiding duplication and maintaining consistent CI/CD processes


### How is calling a reusable workflow different from using a regular action (uses:)?

A reusable workflow reuses an entire GitHub Actions workflow and is called at the job level using uses:. It can contain multiple jobs and steps and is enabled using workflow_call. A regular action is a reusable component that performs a specific task and is called within a job's steps section using uses:

### Where must a reusable workflow file live?

A reusable workflow must be stored in the .github/workflows/ directory of a GitHub repository. It should use the workflow_call trigger so that other workflows can call it

## Task 2: Create Your First Reusable Workflow
Create .github/workflows/reusable-build.yml with a workflow_call trigger, required app_name and environment string inputs (with staging as the default environment), a required docker_token secret, and a build job that checks out the code, prints the app name and environment, and confirms the Docker token is set without exposing its value; this workflow requires a caller workflow to run.

```yaml

name: Reusable Build
on:
  workflow_call:
    inputs:
      app_name: 
        description: 'This is the app for testing purpose'
        required: true
        type: string
      environment:
       description: 'Target deployment environment'
       required: true
       default: 'stagging'
       type: string

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v7
      - name: Display Inputs
        run: |
          echo "Deploying ${{ github.event.inputs.app_name}}
          echo "Target environment is ${{github.event.inputs.environment}}
      - name: Check Docker Token Status
        run: |
         if [ -n "${{ secrets.DOCKERHUB_TOKEN }}" ]; then
          echo "Docker token is set to true (configured)."
         else:
          echo "Docker token is missing or empty." 
         fi
    
```

Verified: The reusable workflow will not run on its own; it will only execute when it is called by a caller workflow.


## Task 3: Create a Caller Workflow

### Trigger the workflow on a push to the main branch and add a job that uses the reusable workflow.

```yaml

name: call-build.yml
on: 
  push:
    branches:
      - main

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      app_name: "Web-app"
      environment: "production"
    secrets: 
      docker_token: ${{ secrets.DOCKERHUB_TOKEN }}

```

### Verify: In the Actions tab, do you see the caller triggering the reusable workflow? Click into the job — can you see the inputs printed?

<img width="1338" height="508" alt="image" src="https://github.com/user-attachments/assets/b0254224-81ee-4c0c-ad24-087d7b786cf9" />

## Task 4: Add Outputs to the Reusable Workflow

### Extend the reusable workflow to expose a build_version output generated from the short commit SHA, then update the caller workflow with a second job that depends on the build job and prints the build_version output.

```yaml
name: call-build.yml
on: 
  push:
    branches:
      - main

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      app_name: "Web-app"
      environment: "production"
    secrets: 
      docker_token: ${{ secrets.DOCKERHUB_TOKEN }}

  display-version:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Display Build Version
        run: |
           echo "Build version is: ${{ needs.build.outputs.build_version }}"

```

```yaml
name: Reusable Build

on:
  workflow_call:
    inputs:
      app_name:
        description: "This is the app for testing purpose"
        required: true
        type: string

      environment:
        description: "Target deployment environment"
        required: true
        default: "staging"
        type: string

    secrets:
      docker_token:
        required: true

    outputs:
      build_version:
        description: "Generated build version"
        value: ${{ jobs.build.outputs.build_version }}

jobs:
  build:
    runs-on: ubuntu-latest

    outputs:
      build_version: ${{ steps.version.outputs.build_version }}

    steps:
      - name: Checkout the code
        uses: actions/checkout@v4

      - name: Display Inputs
        run: |
          echo "Deploying ${{ inputs.app_name }}"
          echo "Target environment is ${{ inputs.environment }}"

      - name: Generate Build Version
        id: version
        run: |
          SHORT_SHA=$(git rev-parse --short HEAD)
          BUILD_VERSION="v1.0-${SHORT_SHA}"
          echo "Build version: $BUILD_VERSION"
          echo "build_version=$BUILD_VERSION" >> "$GITHUB_OUTPUT"

      - name: Check Docker Token Status
        run: |
          if [ -n "${{ secrets.docker_token }}" ]; then
            echo "Docker token is set: true"
          else
            echo "Docker token is missing or empty."
          fi

```

Verify: Does the second job print the version from the reusable workflow?

<img width="1365" height="719" alt="image" src="https://github.com/user-attachments/assets/68847691-9190-4f72-b66b-301de9e20637" />


## Task 5: Create a Composite Action

### Create a custom composite action at .github/actions/setup-and-greet/action.yml with name and language (default en) inputs, steps to print a greeting in the specified language, the current date and runner OS, and set the greeted output to true, then use the composite action in a new workflow with uses: ./.github/actions/setup-and-greet.

```yaml

name: Setup and Greet
description: "Prints a greeting, current date, and runner OS"

inputs:
  name:
    description: "Name of the person to greet"
    required: true

  language:
    description: "Language for the greeting"
    required: false
    default: "en"

outputs:
  greeted:
    description: "Whether the greeting was completed"
    value: ${{ steps.greet.outputs.greeted }}

runs:
  using: "composite"

  steps:
    - name: Print Greeting
      id: greet
      shell: bash
      run: |
        if [ "${{ inputs.language }}" = "en" ]; then
          echo "Hello, ${{ inputs.name }}!"
        elif [ "${{ inputs.language }}" = "hi" ]; then
          echo "Namaste, ${{ inputs.name }}!"
        elif [ "${{ inputs.language }}" = "fr" ]; then
          echo "Bonjour, ${{ inputs.name }}!"
        else
          echo "Hello, ${{ inputs.name }}!"
        fi

        echo "greeted=true" >> "$GITHUB_OUTPUT"

    - name: Print Date and Runner OS
      shell: bash
      run: |
        echo "Current date: $(date)"
        echo "Runner OS: $RUNNER_OS"

```

```yaml
name: Custom Composite Action

on:
  push:
    branches:
      - main

jobs:
  greet:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run Setup and Greet
        id: greeting
        uses: ./.github/actions/setup-and-greet
        with:
          name: "Abusufiyan"
          language: "en"

      - name: Display Output
        run: |
          echo "Greeting completed: ${{ steps.greeting.outputs.greeted }}"

```

### Verify: Does your custom action run and print the greeting?

<img width="1365" height="599" alt="image" src="https://github.com/user-attachments/assets/a2d442c6-d4ee-4120-8db0-ac521312d602" />


## Task 6: Reusable Workflow vs Composite Action

## Reusable Workflow vs Composite Action

| Feature | Reusable Workflow | Composite Action |
|---|---|---|
| **Triggered by** | `workflow_call` | `uses:` in a step |
| **Can contain jobs?** | ✅ Yes | ❌ No |
| **Can contain multiple steps?** | ✅ Yes | ✅ Yes |
| **Lives where?** | `.github/workflows/` | `.github/actions/` |
| **Can accept secrets directly?** | ✅ Yes | ❌ No* |
| **Best for** | Reusing complete workflows/jobs | Reusing a group of steps |

> **Note:** Composite actions can receive sensitive values as inputs, but they don't have direct access to the workflow's `secrets` context like reusable workflows do.
