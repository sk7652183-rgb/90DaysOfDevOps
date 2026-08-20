# Day 45 – Docker Build & Push in GitHub Actions

## Task 1: Prepare

### Use the app you Dockerized on Day 36 (or any simple Dockerfile), add the Dockerfile to your github-actions-practice repository (or create a minimal one), and make sure the DOCKER_USERNAME and DOCKER_TOKEN secrets from Day 44 are set.

<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/febafec7-2d84-477c-8325-f3098d08f3dd" />

## Task 2: Build the Docker Image in CI

### Create .github/workflows/docker-publish.yml to trigger on pushes to main, check out the code, and build and tag the Docker image.

```yaml
name: Build and Push Docker Images

on:
  push:
    branches:
      - DevOps

jobs:
  Build-Image:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        services: [frontend, backend]

    steps:
      - name: Checkout the code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'
          cache: 'npm' 

      - name: Install dependencies
        run: npm ci
      
 
      - name: Login Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build and push Docker image
        uses: docker/build-push-action@v7
        with:
          context: ${{ matrix.services == 'frontend' && './frontend' || '.' }}
          push: true
          tags: ${{ vars.DOCKERHUB_USERNAME }}/school-attendance-app-${{ matrix.services }}:latest


```

### Verify: Check the build step logs — does the image build successfully?

<img width="1363" height="717" alt="image" src="https://github.com/user-attachments/assets/835eb808-9d24-4833-83e2-ccd116043d75" />

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/82cc5166-f74d-45ce-b72e-fc5ed3404ded" />

<img width="1363" height="726" alt="image" src="https://github.com/user-attachments/assets/91e252df-d49f-4b77-96ff-fed122df9dc2" />

<img width="963" height="520" alt="image" src="https://github.com/user-attachments/assets/027a1eef-f2ab-47dc-b77f-acb0f7a163f9" />



## Task 3: Push to Docker Hub

### Log in to Docker Hub using the configured secrets, tag the image as username/repo:latest and username/repo:sha-<short-commit-hash>, and push both tags to Docker Hub.

```yaml
name: Build and Push Docker Images

on:
  push:
    branches:
      - DevOps

jobs:
  Build-Image:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        services: [frontend, backend]

    steps:
      - name: Checkout the code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'
          cache: 'npm' 

      - name: Install dependencies
        run: npm ci
      
 
      - name: Login Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
        
      - name:  Get short commit SHA
        id: short_sha
        run: echo "SHORT_SHA=${GITHUB_SHA::7}" >> $GITHUB_ENV

      - name: Build and push Docker image
        uses: docker/build-push-action@v7
        with:
          context: ${{ matrix.services == 'frontend' && './frontend' || '.' }}
          push: true
          tags: |
                ${{ vars.DOCKERHUB_USERNAME }}/school-attendance-app-${{ matrix.services }}:latest
                ${{ vars.DOCKERHUB_USERNAME }}/school-attendance-app-${{ matrix.services }}:sha-${{ steps.short_sha.outputs.sha }}

```

<img width="1332" height="511" alt="image" src="https://github.com/user-attachments/assets/25a1d97d-7d95-491d-9be8-a852cea6b197" />

<img width="1355" height="502" alt="image" src="https://github.com/user-attachments/assets/1a6d80ab-7422-41cc-9c72-b14474b46315" />


<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/448beda6-f183-476d-ab2d-b5ed21591202" />

<img width="1365" height="682" alt="image" src="https://github.com/user-attachments/assets/4fd2155a-6709-4c98-812b-c187624f3c7a" />


## Task 4: Only Push on Main

### Added a condition so the Docker image is pushed only when the workflow runs on the DevOps branch, not on feature branches or pull requests, and tested it by pushing to the main branch to verify that the image is built but not pushed.

```yaml

name: Build and Push Docker Images

on:
  push:
    branches:
      - feature/test-docker

jobs:
  Build-Image:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        services: [frontend, backend]

    steps:
      - name: Checkout the code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '22'
          cache: 'npm' 

      - name: Install dependencies
        run: npm ci
      
 
      - name: Login Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
        
      - name:  Get short commit SHA
        id: short_sha
        run: echo "SHORT_SHA=${GITHUB_SHA::7}" >> $GITHUB_ENV

      - name: Build and push Docker image
        uses: docker/build-push-action@v7
        with:
          context: ${{ matrix.services == 'frontend' && './frontend' || '.' }}
          push: ${{ github.ref == 'refs/heads/DevOps' && github.event_name == 'push' }}
          tags: |
                ${{ vars.DOCKERHUB_USERNAME }}/school-attendance-app-${{ matrix.services }}:latest
                ${{ vars.DOCKERHUB_USERNAME }}/school-attendance-app-${{ matrix.services }}:sha-${{ steps.short_sha.outputs.sha }}

```


<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/c82ea326-e479-45e1-88f3-83af51f3cf18" />

<img width="1365" height="725" alt="image" src="https://github.com/user-attachments/assets/cdbd9e72-029e-4b14-b40c-10d0752938d7" />

## Task 5: Add a Status Badge

### Get the badge URL for the docker-publish workflow from the Actions tab, add it to your README.md, and push the changes to verify that the badge shows green.

<img width="1365" height="730" alt="image" src="https://github.com/user-attachments/assets/3406d9dd-a62f-4f79-9831-5f8bfa23ce6a" />

<img width="715" height="122" alt="image" src="https://github.com/user-attachments/assets/1b0357c8-008e-4fbe-b7ae-7e50e71415c6" />


## Task 6: Pull and Run It

On your local machine or cloud server, pull the Docker image you just pushed, run it, and confirm that it works successfully.


<img width="1365" height="745" alt="image" src="https://github.com/user-attachments/assets/4775af1e-e77a-4ea7-bd52-c99df7beb558" />

<img width="1365" height="727" alt="image" src="https://github.com/user-attachments/assets/a2401a13-0c96-477f-ad48-21fdd55731f2" />


### Write in your notes: What is the full journey from git push to a running container?

git push → GitHub Actions triggers → Docker image is built → Image is tagged → Image is pushed to Docker Hub → Server pulls the image → Container is started → Application is running.


