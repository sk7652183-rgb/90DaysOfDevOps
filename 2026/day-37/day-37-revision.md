# Day 37 – Docker Revision & Cheat Sheet

## Self-Assessment Checklist

## ✅ Docker Skills Checklist

Mark yourself honestly — **can do**, **shaky**, or **haven't done**.

- [x] Run a container from Docker Hub (interactive + detached)
- [x] List, stop, remove containers and images
- [x] Explain image layers and how caching works
- [x] Write a Dockerfile from scratch with `FROM`, `RUN`, `COPY`, `WORKDIR`, `CMD`
- [ ] Explain `CMD` vs `ENTRYPOINT`
- [x] Build and tag a custom image
- [x] Create and use named volumes
- [ ] Use bind mounts
- [x] Create custom networks and connect containers
- [x] Write a `docker-compose.yml` for a multi-container app
- [x] Use environment variables and `.env` files in Compose
- [x] Write a multi-stage Dockerfile
- [x] Push an image to Docker Hub
- [x] Use `healthcheck` and `depends_on`

## Quick-Fire Questions

### What is the difference between an image and a container?

A Docker image is a blueprint or template that contains the application, its dependencies, libraries, and configuration. A Docker container is a running instance of that image. we can create multiple containers from the same image, and each container runs independently.

### What happens to data inside a container when you remove it?

Containers are ephemeral, which means their data is temporary. When a container is removed, all data stored inside the container is lost unless it is stored in a Docker volume or a bind mount.

### How do two containers on the same custom network communicate?

Two containers connected to the same custom Docker network can communicate with each other using container names as DNS names. Docker provides built-in DNS resolution within custom networks, so containers can reach each other without using IP addresses.

### What does docker compose down -v do differently from docker compose down?

docker compose down stops and removes containers and networks created by Docker Compose, but it keeps the volumes. docker compose down -v does the same thing but also removes the named volumes, which means persistent data stored in those volumes will be deleted.

### Why are multi-stage builds useful?

Multi-stage builds help reduce Docker image size and improve security by separating the build environment from the final runtime image. We can install all required build dependencies in the first stage, then copy only the required application files and dependencies into the final image. This results in a smaller image with fewer unnecessary packages and a reduced attack surface.

### What is the difference between COPY and ADD?

COPY is used to copy files and directories from the build context into the Docker image. ADD has the same functionality as COPY but provides additional features like extracting compressed archives and downloading files from URLs. Docker recommends using COPY unless you specifically need the extra features of ADD.

### What does -p 8080:80 mean?

-p 8080:80 maps port 8080 on the host machine to port 80 inside the container. It allows users to access the application running on container port 80 through the host machine's port 8080.

### How do you check how much disk space Docker is using?

We can use docker system df to check how much disk space Docker is using. It shows the disk usage of images, containers, volumes, and build cache.



