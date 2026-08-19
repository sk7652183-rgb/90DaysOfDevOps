# Day 44 – Secrets, Artifacts & Running Real Tests in CI

## Task 1: GitHub Secrets

### Go to your repository → Settings → Secrets and variables → Actions, create a secret called MY_SECRET_MESSAGE, then create a workflow that reads the secret and prints “The secret is set: true” without ever printing the actual value. Finally, try printing ${{ secrets.MY_SECRET_MESSAGE }} directly and observe what GitHub shows in the workflow log.

```yaml

name: Check Secret Presence

on:
  workflow_dispatch:

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - name: Check if secret is configured
        env:
          MY_SECRET: ${{ secrets.MY_SECRET_MESSAGE }}
        run: |
          if [ -n "$MY_SECRET" ]; then
            echo "The secret is set: true"
          else
            echo "The secret is set: false"
          fi

```


<img width="1364" height="730" alt="image" src="https://github.com/user-attachments/assets/e87563a0-513d-4bc6-87fe-2711a5624e8e" />

### Write in your notes: Why should you never print secrets in CI logs?
Never print secrets in CI logs because logs may be stored, shared, or accessed by people who should not have access to sensitive credentials. Accidentally exposing a secret can allow unauthorized access to applications, cloud resources, repositories, or other systems. Secrets should be passed securely to commands and masked rather than printed.

## Task 2: Use Secrets as Environment Variables

### Pass the secrets DOCKER_USERNAME and DOCKER_TOKEN to a step as environment variables, use them in a shell command without ever hardcoding them, and store both values as secrets.

```yaml
name: Check Secret Presence

on:
  workflow_dispatch:

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - name: Check if secret is configured
        env:
          MY_SECRET: ${{ secrets.MY_SECRET_MESSAGE }}
        run: |
          if [ -n "$MY_SECRET" ]; then
            echo "The secret is set: true"
          else
            echo "The secret is set: false"
          fi
       # Step 2: Log in to Docker Hub using saved secrets
      - name: Log in to Docker Hub
        uses: docker/login-action@v4
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

```

<img width="1364" height="681" alt="image" src="https://github.com/user-attachments/assets/27f3a0fa-d3e7-4d78-84e5-69aa5231d661" />

## Task 3: Upload Artifacts

### Create a step that generates a file, such as a test report or log file, use actions/upload-artifact to save it, and then download the artifact from the Actions tab after the workflow runs.

```yaml
name: Check Secret Presence

on:
  workflow_dispatch:

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - name: Check if secret is configured
        env:
          MY_SECRET: ${{ secrets.MY_SECRET_MESSAGE }}
        run: |
          if [ -n "$MY_SECRET" ]; then
            echo "The secret is set: true"
          else
            echo "The secret is set: false"
          fi
       # Step 2: Log in to Docker Hub using saved secrets
      - name: Log in to Docker Hub
        uses: docker/login-action@v4
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

          # # Step 3: Generate a log file or run your test suite

      - name: Generate Log and Test Report
        run: |
          mkdir -p outputs
          echo " Build started at $(date)" > outputs/build.log
          echo " Error: Service timeout" >> outputs/build.log
          echo "Mock test results: 10 Passed, 0 Failed" > outputs/test-report.txt
      # Step 3: Upload the files so they don't get deleted when the job ends

      - name: Upload Generated Files
        uses: actions/upload-artifact@v4
        with:
            name: execution-artifacts
            path: outputs/

```

<img width="1364" height="723" alt="image" src="https://github.com/user-attachments/assets/d119390d-70cd-4f4a-8d39-bc130793b47f" />

### Verify: Can you see and download it from GitHub?

Yes, I can see the downloaded file and access it from GitHub.

## Task 4: Download Artifacts Between Jobs

### Create a GitHub Actions workflow where Job 1 generates a file and uploads it as an artifact, and Job 2 downloads the artifact from Job 1 and uses it by printing its contents.

```yaml

name:  To Generate Artifact Between Jobs
on:
  push:
  workflow_dispatch:


jobs:
  Job_1:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the Code
        uses: actions/checkout@v4
      - name: Generate file
        run: |
         echo "Hello from Job 1!" > test-report.txt
         echo "This file was created by GitHub Actions." >> test-report.txt
      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
         name: test-report
         path: test-report.txt

  jobs_2:
    name: Download and Use Artifact
    runs-on: ubuntu-latest
    needs: Job_1
    steps:
      - name: Download the Artifact
        uses: actions/download-artifact@v4
        with:
          name: test-report

      - name: To Print file contents
        run: |
          echo " The content of test-report.txt : "
          cat test-report.txt

```

<img width="1365" height="722" alt="image" src="https://github.com/user-attachments/assets/3d16720c-fe25-4150-8802-d30e13b7bba0" />


<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/5e310eff-7fb9-416b-aeca-aa15c060874f" />

<img width="1363" height="721" alt="image" src="https://github.com/user-attachments/assets/b4f5b609-3b0c-4b99-8996-226e61c7efff" />


### Write in your notes: When would you use artifacts in a real pipeline?

Use artifacts to store and pass files between jobs or preserve build outputs such as test reports, logs, compiled binaries, and deployment packages for later use or download.


## Task 5: Run Real Tests in CI

### Added script to the github-actions-practice repo

## Python Script

```python
print("Hello from GitHub Actions!")

exit(0)
```


<img width="1365" height="722" alt="image" src="https://github.com/user-attachments/assets/1e737e9c-8944-4d55-a03d-f736188efd0b" />

### Write a workflow that:

The workflow checks out the code, installs any required dependencies, runs the script, and fails the pipeline if the script exits with a non-zero code.

<img width="1360" height="727" alt="image" src="https://github.com/user-attachments/assets/0d17ec1e-6974-444a-a19a-1145bd077c4f" />

### Intentionally break the script — verify the pipeline goes red

<img width="1362" height="725" alt="image" src="https://github.com/user-attachments/assets/2123b0e3-d283-4ddb-a3c1-1602a979e941" />

### Fix it — verify it goes green again


<img width="1365" height="717" alt="image" src="https://github.com/user-attachments/assets/3b2e78f5-747d-4900-8ee8-16f69573e31e" />



## Task 6: Caching

### Add actions/cache to a workflow that installs dependencies

## Requirements

```text
requests

```


<img width="1364" height="726" alt="image" src="https://github.com/user-attachments/assets/1911ac1c-a4d7-4119-a613-9e32a1937c47" />

First run:
The cache won't exist, so GitHub Actions downloads and installs the dependencies. You should see a cache miss.


### Run it twice — observe the time difference

## Requirements

```text
requests
flask
```

<img width="1365" height="725" alt="image" src="https://github.com/user-attachments/assets/e38bf20c-deb9-473c-8f14-e8ffce1db56b" />

Second run:
The cache should be restored, so pip can reuse the cached packages. You should see a cache hit, and dependency installation should generally be faster.


### What is being cached?

The pip download cache located at `~/.cache/pip`, which contains downloaded Python packages.

### Where is it stored?

GitHub Actions stores the cache on GitHub's hosted cache service and restores it to the runner when the cache key matches.






