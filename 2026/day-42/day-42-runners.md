# Day 42 – Runners: GitHub-Hosted & Self-Hosted

## Task 1: GitHub-Hosted Runners

### Create a workflow with 3 jobs, each on a different OS 

Create a workflow with three jobs, each running on a different operating system: ubuntu-latest, windows-latest, and macos-latest.

```yaml

name: Multi-OS Workflow
on:
  push

jobs:
  linux-job:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v7
      - name: Run linux command
        run: echo "This job runs on Ubuntu"
  windows-job:
      runs-on: windows-latest
      steps:
        - name: Checkout the code
          uses: actions/checkout@v7
        - name: Run windows command
          run: Write-Output "This job runs on Windows"
  macos-jobs:
      runs-on: macos-latest
      steps:
        - name: Checkout the code 
          uses: actions/checkout@v7
        - name: Run macos command
          run: echo "This job runs on macOS"
```


<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/0de7420e-762d-4a33-aaf0-0123620fbe26" />

### In each job, print

```yaml

name: Multi-OS Workflow
on:
  push

jobs:
  linux-job:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the code
        uses: actions/checkout@v7
      - name: Run linux command
        run: echo "This job runs on Ubuntu"
      - name: Print OS Name via Environment Variable
        run: echo "The Operating system is ${{ runner.os }}"
  windows-job:
      runs-on: windows-latest
      steps:
        - name: Checkout the code
          uses: actions/checkout@v7
        - name: Run windows command
          run: Write-Output "This job runs on Windows"
        - name: Print OS Name via Environment Variable
          run: echo "The Operating system is ${{ runner.os }}"
  macos-jobs:
      runs-on: macos-latest
      steps:
        - name: Checkout the code 
          uses: actions/checkout@v7
        - name: Run macos command
          run: echo "This job runs on macOS"
        - name: Print OS Name via Environmet Varibale
          run: echo "The Operating system is ${{ runner.os }}"
```
