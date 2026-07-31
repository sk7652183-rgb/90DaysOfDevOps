# Day 34 – Docker Compose: Real-World Multi-Container Apps

## Task 1: Build Your Own App Stack

### Create a docker-compose.yml for a 3-service stack:

## Project Overview

This project is a fork of an open-source web application built with Python Flask.

### Enhancements
- Migrated the database from **SQLite3** to **PostgreSQL** for improved scalability and reliability.
- Integrated **Redis** as the caching layer to enhance application performance.
- Created a **Dockerfile** to containerize the application, ensuring consistent deployment across different environments.
- Updated the application configuration to support the new database and caching infrastructure.

```bash
ubuntu@ip-172-31-19-67:~/hotelhub$ ls
Dockerfile  accounts            guest            init_superuser.py  manage.py  requirements.txt  screenshots
LICENSE     docker-compose.yml  hotelmanagement  main               readme.md  room              staticfiles
ubuntu@ip-172-31-19-67:~/hotelhub$ docker compose up --build -d
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] /home/ubuntu/hotelhub/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 23/23
 ✔ db Pulled                                                                                                                                 7.2s
   ✔ 7bd7d677a4b5 Pull complete                                                                                                              0.2s
   ✔ 55afa1ecc21d Pull complete                                                                                                              0.9s
   ✔ ae8ed6ddf5d2 Pull complete                                                                                                              5.8s
   ✔ 4635b503f9b2 Pull complete                                                                                                              0.3s
   ✔ e5d94dfa43d9 Pull complete                                                                                                              0.4s
   ✔ 603858638485 Pull complete                                                                                                              0.9s
   ✔ 34a3e2652f22 Pull complete                                                                                                              1.1s
   ✔ 9cf33c25d575 Pull complete                                                                                                              0.4s
   ✔ b1aad2d5f399 Pull complete                                                                                                              0.4s
   ✔ 5635b2c7f90f Pull complete                                                                                                              0.2s
   ✔ d17e2629e444 Download complete                                                                                                          0.2s
   ✔ 0d2f81fc44f6 Download complete                                                                                                          0.0s
 ✔ redis Pulled                                                                                                                              3.2s
   ✔ db197c512a33 Pull complete                                                                                                              0.3s
   ✔ 897d797d2723 Pull complete                                                                                                              0.9s
   ✔ f5a655897537 Pull complete                                                                                                              0.9s
   ✔ 63e63047b377 Pull complete                                                                                                              1.1s
   ✔ 93ebed1aef27 Pull complete                                                                                                              0.2s
   ✔ 4f4fb700ef54 Pull complete                                                                                                              0.3s
   ✔ 627d9d06d3d0 Pull complete                                                                                                              1.9s
   ✔ cac39341ecaa Download complete                                                                                                          0.1s
   ✔ 22b5e73fc01c Download complete                                                                                                          0.0s
WARN[0007] Docker Compose is configured to build using Bake, but buildx isn't installed
[+] Building 3.2s (11/11) FINISHED                                                                                                 docker:default
 => [web internal] load build definition from Dockerfile                                                                                     0.0s
 => => transferring dockerfile: 264B                                                                                                         0.0s
 => [web internal] load metadata for docker.io/library/python:3.11-slim                                                                      0.6s
 => [web internal] load .dockerignore                                                                                                        0.0s
 => => transferring context: 2B                                                                                                              0.0s
 => [web 1/5] FROM docker.io/library/python:3.11-slim@sha256:db3ff2e1800a8581e2c48a27c3995339d47bdf046da21c7627accd3d51053a93                0.0s
 => => resolve docker.io/library/python:3.11-slim@sha256:db3ff2e1800a8581e2c48a27c3995339d47bdf046da21c7627accd3d51053a93                    0.0s
 => [web internal] load build context                                                                                                        0.1s
 => => transferring context: 17.63kB                                                                                                         0.1s
 => CACHED [web 2/5] WORKDIR /app                                                                                                            0.0s
 => CACHED [web 3/5] COPY requirements.txt .                                                                                                 0.0s
 => CACHED [web 4/5] RUN pip install --no-cache-dir -r requirements.txt                                                                      0.0s
 => CACHED [web 5/5] COPY . .                                                                                                                0.0s
 => [web] exporting to image                                                                                                                 2.3s
 => => exporting layers                                                                                                                      0.0s
 => => exporting manifest sha256:dfa1379702dbdc43c462e65a7ef6a6c236a09880c4bbefa2ead181284c0a371d                                            0.0s
 => => exporting config sha256:67b73e8621bd88863658b174efa754c34523cd4439c4b305ffc23eac25dc0688                                              0.0s
 => => exporting attestation manifest sha256:35ef3f630108fc28e0e1fdc108db32cf2fff10fec89bfb93461d675093b512a7                                0.0s
 => => exporting manifest list sha256:b76339600bb65c0b3459feb9b536d622a5ca0d00425a763feee354a89ed7ce99                                       0.0s
 => => naming to docker.io/library/hotelhub-web:latest                                                                                       0.0s
 => => unpacking to docker.io/library/hotelhub-web:latest                                                                                    2.2s
 => [web] resolving provenance for metadata file                                                                                             0.0s
[+] Running 7/7
 ✔ web                            Built                                                                                                      0.0s
 ✔ Network hotelhub_default       Created                                                                                                    0.1s
 ✔ Volume hotelhub_redis_data     Created                                                                                                    0.0s
 ✔ Volume hotelhub_postgres_data  Created                                                                                                    0.0s
 ✔ Container redis_cache          Started                                                                                                    0.7s
 ✔ Container django_db            Healthy                                                                                                   11.2s
 ✔ Container django_web           Started                                                                                                   11.2s
ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS                    PORTS                                         NAMES
b670c9851a86   hotelhub-web         "sh -c ' python mana…"   17 seconds ago   Up 5 seconds              0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
9dcaad65a1c7   postgres:17-alpine   "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds (healthy)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
805165f68b06   redis:7.4-alpine     "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds             0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$

```
<img width="1365" height="682" alt="image" src="https://github.com/user-attachments/assets/41f89693-765c-4592-ab93-f438d94e589c" />


## Task 2: depends_on & Healthchecks

### Added a health check to the PostgreSQL service and configured depends_on with condition: service_healthy in the Docker Compose file. This ensures that the application starts only after the database is fully initialized and ready to accept connections, rather than just after the database container has started.

```bash
ubuntu@ip-172-31-19-67:~/hotelhub$ cat docker-compose.yml
version: "3.9"

services:
  db:
    image: postgres:17-alpine
    container_name: django_db
    restart: always
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7.4-alpine
    container_name: redis_cache
    restart: unless-stopped
    command:
      - redis-server
      - --requirepass
      - ${REDIS_PASSWORD}
      - --appendonly
      - "yes"
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  web:
    build: .
    container_name: django_web
    restart: always
    command: >
      sh -c "
      python manage.py migrate &&
      python init_superuser.py &&
      python manage.py loaddata room/fixtures/initial_data.json &&
      python manage.py loaddata guest/fixtures/guest_data.json &&
      python manage.py loaddata main/fixtures/reservation_data.json &&
      python manage.py runserver 0.0.0.0:8000
      "
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    environment:
      DEBUG: "True"

      DB_HOST: db
      DB_NAME: ${POSTGRES_DB}
      DB_USER: ${POSTGRES_USER}
      DB_PASSWORD: ${POSTGRES_PASSWORD}
      DB_PORT: 5432

      REDIS_HOST: redis
      REDIS_PORT: 6379
      REDIS_PASSWORD: ${REDIS_PASSWORD}

    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started

volumes:
  postgres_data:
  redis_data:

ubuntu@ip-172-31-19-67:~/hotelhub$
```
Test: Bring everything down and up — does the app wait for the DB?

```bash
ubuntu@ip-172-31-19-67:~/hotelhub$ docker compose down
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] /home/ubuntu/hotelhub/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 4/4
 ✔ Container django_web      Removed                                                                                                        10.2s
 ✔ Container django_db       Removed                                                                                                         0.3s
 ✔ Container redis_cache     Removed                                                                                                         0.3s
 ✔ Network hotelhub_default  Removed                                                                                                         0.1s
ubuntu@ip-172-31-19-67:~/hotelhub$ docker compose up --build -d
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] The "q" variable is not set. Defaulting to a blank string.
WARN[0000] The "sa0y" variable is not set. Defaulting to a blank string.
WARN[0000] /home/ubuntu/hotelhub/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
WARN[0000] Docker Compose is configured to build using Bake, but buildx isn't installed
[+] Building 1.0s (11/11) FINISHED                                                                                                 docker:default
 => [web internal] load build definition from Dockerfile                                                                                     0.0s
 => => transferring dockerfile: 264B                                                                                                         0.0s
 => [web internal] load metadata for docker.io/library/python:3.11-slim                                                                      0.6s
 => [web internal] load .dockerignore                                                                                                        0.0s
 => => transferring context: 2B                                                                                                              0.0s
 => [web 1/5] FROM docker.io/library/python:3.11-slim@sha256:db3ff2e1800a8581e2c48a27c3995339d47bdf046da21c7627accd3d51053a93                0.0s
 => => resolve docker.io/library/python:3.11-slim@sha256:db3ff2e1800a8581e2c48a27c3995339d47bdf046da21c7627accd3d51053a93                    0.0s
 => [web internal] load build context                                                                                                        0.1s
 => => transferring context: 17.63kB                                                                                                         0.0s
 => CACHED [web 2/5] WORKDIR /app                                                                                                            0.0s
 => CACHED [web 3/5] COPY requirements.txt .                                                                                                 0.0s
 => CACHED [web 4/5] RUN pip install --no-cache-dir -r requirements.txt                                                                      0.0s
 => CACHED [web 5/5] COPY . .                                                                                                                0.0s
 => [web] exporting to image                                                                                                                 0.1s
 => => exporting layers                                                                                                                      0.0s
 => => exporting manifest sha256:dfa1379702dbdc43c462e65a7ef6a6c236a09880c4bbefa2ead181284c0a371d                                            0.0s
 => => exporting config sha256:67b73e8621bd88863658b174efa754c34523cd4439c4b305ffc23eac25dc0688                                              0.0s
 => => exporting attestation manifest sha256:467c13e43c88e9b400a83c3e28ae362e474b9ba0325c13f4666d76ca7388d984                                0.0s
 => => exporting manifest list sha256:b2f737d3dce084198c9c375855a2e33e18ef0c24c33b54285fe83b7cd2cf959b                                       0.0s
 => => naming to docker.io/library/hotelhub-web:latest                                                                                       0.0s
 => => unpacking to docker.io/library/hotelhub-web:latest                                                                                    0.0s
 => [web] resolving provenance for metadata file                                                                                             0.0s
[+] Running 5/5
 ✔ web                       Built                                                                                                           0.0s
 ✔ Network hotelhub_default  Created                                                                                                         0.1s
 ✔ Container redis_cache     Started                                                                                                         0.7s
 ✔ Container django_db       Healthy                                                                                                        11.3s
 ✔ Container django_web      Started                             
```

Yes. The depends_on configuration ensures that the application waits for the database to become healthy before it starts.

## Task 3: Restart Policies

## Added restart: always to the database service in the Docker Compose file. After manually stopping the database container, Docker automatically restarted it, ensuring the database service remained available.
```bash

ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                    PORTS                                         NAMES
5bd2d5a00ef1   hotelhub-web         "sh -c ' python mana…"   4 minutes ago   Up 4 minutes              0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
3661b93a45a2   postgres:17-alpine   "docker-entrypoint.s…"   4 minutes ago   Up 13 seconds (healthy)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
ac663fc92134   redis:7.4-alpine     "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes              0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$ docker exec django_db pkill postgres
ubuntu@ip-172-31-19-67:~/hotelhub$ docker inspect django_db --format='{{.RestartCount}}'
1
ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                    PORTS                                         NAMES
5bd2d5a00ef1   hotelhub-web         "sh -c ' python mana…"   5 minutes ago   Up 5 minutes              0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
3661b93a45a2   postgres:17-alpine   "docker-entrypoint.s…"   5 minutes ago   Up 14 seconds (healthy)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
ac663fc92134   redis:7.4-alpine     "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes              0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                    PORTS                                         NAMES
5bd2d5a00ef1   hotelhub-web         "sh -c ' python mana…"   5 minutes ago   Up 5 minutes              0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
3661b93a45a2   postgres:17-alpine   "docker-entrypoint.s…"   5 minutes ago   Up 26 seconds (healthy)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
ac663fc92134   redis:7.4-alpine     "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes              0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$
ubuntu@ip-172-31-19-67:~/hotelhub$ docker exec django_db pkill postgres
ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                            PORTS                                         NAMES
5bd2d5a00ef1   hotelhub-web         "sh -c ' python mana…"   5 minutes ago   Up 5 minutes                      0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
3661b93a45a2   postgres:17-alpine   "docker-entrypoint.s…"   5 minutes ago   Up 2 seconds (health: starting)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
ac663fc92134   redis:7.4-alpine     "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes                      0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$
ubuntu@ip-172-31-19-67:~/hotelhub$
ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED         STATUS                    PORTS                                         NAMES
5bd2d5a00ef1   hotelhub-web         "sh -c ' python mana…"   6 minutes ago   Up 6 minutes              0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
3661b93a45a2   postgres:17-alpine   "docker-entrypoint.s…"   6 minutes ago   Up 28 seconds (healthy)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
ac663fc92134   redis:7.4-alpine     "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes              0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$
```
Configured the PostgreSQL container with restart: always to ensure it automatically restarts if it exits unexpectedly. I verified the restart policy by simulating a container failure rather than manually stopping the container.`

### Try restart: on-failure — how is it different?

```bash
ubuntu@ip-172-31-19-67:~/hotelhub$ docker ps
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS                    PORTS                                         NAMES
387478ab493d   hotelhub-web         "sh -c ' python mana…"   17 seconds ago   Up 5 seconds              0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   django_web
5d7c6fb6ea3c   postgres:17-alpine   "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds (healthy)   0.0.0.0:5432->5432/tcp, [::]:5432->5432/tcp   django_db
892f0c9aa8dd   redis:7.4-alpine     "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds             0.0.0.0:6379->6379/tcp, [::]:6379->6379/tcp   redis_cache
ubuntu@ip-172-31-19-67:~/hotelhub$ docker inspect django_db --format='{{.State.ExitCode}}'
0
ubuntu@ip-172-31-19-67:~/hotelhub$
```
restart: always ensures the container is restarted whenever it stops unexpectedly and also after the Docker daemon or host restarts. In contrast, restart: on-failure restarts the container only when it exits with a non-zero exit code, making it useful for applications that should only be retried after failures rather than after every stop or system reboot.

### Write in your notes: When would you use each restart policy?
restart: no → Default. Container will not restart automatically. Use for testing or one-time tasks.
restart: always → Always restart the container after crashes or Docker/server restart. Use for important services like Database, Web App, Nginx.
restart: on-failure → Restart only when the container exits due to an error (non-zero exit code). Use for background jobs or retry-based tasks.
restart: unless-stopped → Restart automatically unless you manually stop it. Use for development services and local environments.

## Task 4: Created custom Dockerfiles in Compose, made code changes in the app, and rebuilt and restarted everything with a single command.

Replaced the pre-built application image with a Docker build configuration. Added build: . in the Docker Compose file to build the application image directly from the Dockerfile, ensuring the container uses the latest application code and dependencies.

```bash
web:
    build: .
    container_name: django_web
    restart: always
    command: >
      sh -c "
      python manage.py migrate &&
      python init_superuser.py &&
      python manage.py loaddata room/fixtures/initial_data.json &&
      python manage.py loaddata guest/fixtures/guest_data.json &&
      python manage.py loaddata main/fixtures/reservation_data.json &&
      python manage.py runserver 0.0.0.0:8000
      "
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    environment:
      DEBUG: "True"

      DB_HOST: db
      DB_NAME: ${POSTGRES_DB}
      DB_USER: ${POSTGRES_USER}
      DB_PASSWORD: ${POSTGRES_PASSWORD}
      DB_PORT: 5432

      REDIS_HOST: redis
      REDIS_PORT: 6379
      REDIS_PASSWORD: ${REDIS_PASSWORD}

    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started

volumes:
  postgres_data:
  redis_data:
```

## Task 5: Named Networks & Volumes

### Define explicit networks in your compose file instead of relying on the default

```bash

ubuntu@ip-172-31-19-67:~$ docker network ls
NETWORK ID     NAME               DRIVER    SCOPE
9f254d916791   bridge             bridge    local
cb388fef9003   host               host      local
27ee39046ec6   hotelhub_default   bridge    local
a83d5927f441   none               null      local
ubuntu@ip-172-31-19-67:~$
```
### Define named volumes for database data

```bash
ubuntu@ip-172-31-19-67:~$ docker volume ls
DRIVER    VOLUME NAME
local     hotelhub_postgres_data
local     hotelhub_redis_data
ubuntu@ip-172-31-19-67:~$
```

### Add labels to your services for better organization
```bash
version: "3.9"

services:
  db:
    image: postgres:17-alpine
    container_name: django_db
    labels:
      project: "hotelhub"
      service: "database"
      environment: "production"
    restart: always
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7.4-alpine
    container_name: redis_cache
    labels:
      project: "hotelhub"
      service: "cache"
      environment: "production"
    restart: unless-stopped
    command:
      - redis-server
      - --requirepass
      - ${REDIS_PASSWORD}
      - --appendonly
      - "yes"
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  web:
    build: .
    container_name: django_web
    labels:
      project: "hotelhub"
      service: "web-application"
      environment: "production"
    restart: always
    command: >
      sh -c "
      python manage.py migrate &&
      python init_superuser.py &&
      python manage.py loaddata room/fixtures/initial_data.json &&
      python manage.py loaddata guest/fixtures/guest_data.json &&
      python manage.py loaddata main/fixtures/reservation_data.json &&
      python manage.py runserver 0.0.0.0:8000
      "
    volumes:
      - .:/app
    ports:
      - "8000:8000"

volumes:
  postgres_data:
  redis_data:

```

## Task 6: Scaling (Bonus)



