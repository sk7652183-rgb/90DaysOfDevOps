# Day 29 – Introduction to Docker


### What is Docker?

Docker is a containerization platform used to build, package, and run containers. A container packages an application along with all its dependencies, libraries, and configuration files, ensuring it runs consistently across different environments.

Docker helps solve the common **"It works on my machine"** problem by providing a consistent runtime environment. Containers are lightweight because they share the host operating system's kernel instead of including a full operating system, allowing them to start quickly and use fewer system resources than virtual machines.

Containers are generally **ephemeral**, meaning they can be created, stopped, deleted, and recreated easily. For applications that require persistent data, Docker provides **volumes** to store data outside the container.

### What is a container and why do we need them?

A container is a lightweight, isolated runtime environment that packages an application along with all its dependencies, libraries, and configuration files. This ensures that the application runs consistently across different environments.

### Containers vs Virtual Machines — what's the real difference?


| Containers                                                          | Virtual Machines                                                                        |
| ------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Lightweight and use fewer resources.                                | Heavier because each VM includes a full operating system.                               |
| Share the host operating system's kernel.                           | Each VM has its own guest operating system and kernel.                                  |
| Start in seconds.                                                   | Take longer to boot because they start an entire operating system.                      |
| Use less CPU, memory, and disk space.                               | Require more CPU, memory, and disk space.                                               |
| Generally ephemeral and easy to create, stop, delete, and recreate. | Typically long-lived, although they can also be recreated.                              |
| Best for microservices, CI/CD, and cloud-native applications.       | Best for running multiple operating systems, legacy applications, and strong isolation. |


### What is the Docker architecture? (daemon, client, images, containers, registry)

Docker follows a client-server architecture and consists of the following components:

* **Docker Client:** The command-line interface (CLI) that users interact with. It sends commands (such as `docker build`, `docker run`, and `docker pull`) to the Docker Daemon.
* **Docker Daemon (`dockerd`):** A background service that listens for requests from the Docker Client and manages Docker images, containers, networks, and volumes.
* **Docker Images:** Read-only templates (blueprints) that contain the application code, dependencies, libraries, and configuration required to create a container.
* **Docker Containers:** Running instances of Docker images. Containers provide an isolated environment where applications run consistently across different systems.
* **Docker Registry:** A repository used to store and distribute Docker images. Docker Hub is the default public registry, and organizations can also use private registries.

## Docker Architecture

<p align="center">
  <img src="./images/docker-architecture.png" alt="Docker Architecture Diagram" width="900">
</p>

### Architecture Flow

```text
                 +----------------------+
                 |    Docker Client     |
                 |   (docker CLI/API)   |
                 +----------+-----------+
                            |
                     Docker Commands
                            |
                            v
                 +----------------------+
                 |    Docker Daemon     |
                 |      (dockerd)       |
                 +----------+-----------+
                            |
        +-------------------+-------------------+
        |                   |                   |
        v                   v                   v
+---------------+   +---------------+   +----------------+
| Docker Images |   |  Containers   |   | Networks/Volumes|
+---------------+   +---------------+   +----------------+
        |
        | Pull / Push
        v
+----------------------+
|   Docker Registry    |
| (Docker Hub/Private) |
+----------------------+
```


## Task 2: Install Docker

### Installed and verified Docker on my machine.
<img width="1858" height="268" alt="image" src="https://github.com/user-attachments/assets/f42bf5b0-7d2c-45b1-afdd-d478056f40f1" />

### Executed the hello-world container and verified the output.

```bash
sufiyan@Khan:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

sufiyan@Khan:~$ 
```
## Task 3: Run Real Containers

### Run an Nginx container and access it in your browser

```bash
sufiyan@Khan:~$ docker run -d --name nginx-container -p 8080:80 nginx:latest
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
e95a6c7ea7d4: Pull complete 
df68ee7e7a00: Pull complete 
1b30016634d5: Pull complete 
acf093e7a04f: Pull complete 
cd9307c9ecd8: Pull complete 
fcb6fd84b2a0: Pull complete 
1645c1e06f46: Pull complete 
Digest: sha256:ec4ed8b5299e5e90694af7750eb6dffd2627317d30544d056b0371f8082f7bce
Status: Downloaded newer image for nginx:latest
49045f25ed71c080808d34512cc01ec1be79179cb5c8f007bd0c58bce930485b
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS             PORTS                                     NAMES
49045f25ed71   nginx:latest           "/docker-entrypoint.…"   5 seconds ago   Up 3 seconds       0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx-container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up About an hour   127.0.0.1:44885->6443/tcp                 kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up About an hour                                             kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up About an hour   127.0.0.1:36809->6443/tcp                 k8s.demo-control-plane
sufiyan@Khan:~$ 
```

<img width="1863" height="1013" alt="image" src="https://github.com/user-attachments/assets/88a1c604-b342-4d6e-87f3-5c421bc0d6bf" />

## Run an Ubuntu container in interactive mode — explore it like a mini Linux machine

```bash
sufiyan@Khan:~$ docker run -it ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
a9be9fd915e9: Pull complete 
2c1ce1d0a589: Pull complete 
Digest: sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e
Status: Downloaded newer image for ubuntu:latest
root@3de8d30ce4ee:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@3de8d30ce4ee:/# cd etc
root@3de8d30ce4ee:/etc# ls
alternatives            cron.daily      environment  gshadow-   issue.net     libaudit.conf  mtab           pam.d      rc1.d  rcS.d        shadow-  subuid-
apt                     debconf.conf    fstab        host.conf  kernel        login.defs     networks       passwd     rc2.d  resolv.conf  shells   sysctl.d
bash.bashrc             debian_version  gai.conf     hostname   ld.so.cache   logrotate.d    nsswitch.conf  passwd-    rc3.d  rmt          skel     systemd
bindresvport.blacklist  default         group        hosts      ld.so.conf    lsb-release    opt            profile    rc4.d  security     subgid   terminfo
cloud                   dpkg            group-       init.d     ld.so.conf.d  machine-id     os-release     profile.d  rc5.d  selinux      subgid-  update-motd.d
cron.d                  e2scrub.conf    gshadow      issue      legal         mke2fs.conf    pam.conf       rc0.d      rc6.d  shadow       subuid   xattr.conf
root@3de8d30ce4ee:/etc# 
```

<img width="1860" height="585" alt="image" src="https://github.com/user-attachments/assets/4a85da13-57b9-40bf-877b-8ca205e572d2" />

###  Listed all running and stopped Docker containers

```bash

sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                                     NAMES
3de8d30ce4ee   ubuntu                                "/bin/bash"              4 minutes ago    Exited (0) 21 seconds ago                                             funny_buck
49045f25ed71   nginx:latest                          "/docker-entrypoint.…"   8 minutes ago    Up 8 minutes                0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx-container
f37abe5ad0db   hello-world                           "/hello"                 16 minutes ago   Exited (0) 16 minutes ago                                             elated_kirch
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                                             minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:44885->6443/tcp                 kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour                                                      kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:36809->6443/tcp                 k8s.demo-control-plane
sufiyan@Khan:~$ 
```
### Stop and remove a container
```bash

sufiyan@Khan:~$ docker stop 49045f25ed71 f37abe5ad0db && docker rm 49045f25ed71 f37abe5ad0db
49045f25ed71
f37abe5ad0db
49045f25ed71
f37abe5ad0db
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                       NAMES
3de8d30ce4ee   ubuntu                                "/bin/bash"              12 minutes ago   Exited (0) 8 minutes ago                                funny_buck
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up 2 hours                  127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up 2 hours                                              kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up 2 hours                  127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

## Task 4: Explore

### Run a Container in Detached Mode — What's Different?

Running a container in **detached mode** (`-d`) starts the container in the background and immediately returns control of the terminal. This allows you to continue using the terminal while the container keeps running.

**Example:**

```bash
docker run -d nginx:latest
```

In contrast, running a container without the `-d` flag starts it in **foreground mode**, where the container's logs are displayed in the terminal and the terminal remains attached to the container until it stops.


### Give a Container a Custom Name

To assign a custom name to a Docker container, use the `--name` option with the `docker run` command.

**Example:**

```bash
docker run -d --name nginx-container nginx:latest
```

This command creates and starts a container with the name **`nginx-container`**, making it easier to manage the container using Docker commands such as `docker start`, `docker stop`, `docker logs`, and `docker exec`.

### Map a Port from the Container to the Host

To map a port from a Docker container to the host machine, use the `-p` option.

**Syntax:**

```bash
docker run -p <host-port>:<container-port> <image-name>
```

**Example:**

```bash
docker run -d --name nginx-container -p 8080:80 nginx:latest
```

In this example, port **8080** on the host is mapped to port **80** inside the container. You can access the Nginx application by visiting:

```text
http://localhost:8080
```

### Check Logs of a Running Container

To view the logs of a running Docker container, use the `docker logs` command.

**Syntax:**

```bash
docker logs <container-name-or-container-id>
```

**Example:**

```bash
docker logs nginx-container
```

To follow the logs in real time, use the `-f` option:

```bash
docker logs -f nginx-container
```

This displays the container's output, making it useful for monitoring the application and troubleshooting issues.

### Run a Command Inside a Running Container

To run a command inside a running Docker container, use the `docker exec` command.

**Syntax:**

```bash
docker exec [OPTIONS] <container-name-or-container-id> <command>
```

**Example:**

```bash
docker exec nginx-container ls
```

To open an interactive shell inside the container, use:

```bash
docker exec -it nginx-container /bin/bash
```

> If the container does not have **Bash** installed (as is common with Alpine-based images), use:

```bash
docker exec -it <container-name> /bin/sh
```

The `docker exec` command allows you to execute commands inside a running container without stopping or restarting it.

```bash
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                                     NAMES
bf3bfb356d8a   nginx:latest           "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx-container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 2 hours     127.0.0.1:44885->6443/tcp                 kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 2 hours                                               kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 2 hours     127.0.0.1:36809->6443/tcp                 k8s.demo-control-plane
sufiyan@Khan:~$ docker exec -it bf3bfb356d8a /bin/sh
# ls
bin  boot  dev	docker-entrypoint.d  docker-entrypoint.sh  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
# 
```






