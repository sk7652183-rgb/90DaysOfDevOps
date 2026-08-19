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


