# Day 33 – Docker Compose: Multi-Container Basics

## Task 1: Install & Verify

### Checked that Docker Compose is available on the machine and verified its version.

```bash
sufiyan@Khan:~$ docker compose version
Docker Compose version v2.39.1-desktop.1
sufiyan@Khan:~$ 
```
## Task 2: Your First Compose File

### Created a folder compose-basics

```bash
sufiyan@Khan:~$ ls | grep compose-basics
compose-basics
sufiyan@Khan:~$ 

```

### Created a docker-compose.yml file that runs a single Nginx container with port mapping and started the container using docker compose up.

```bash
sufiyan@Khan:~/compose-basics$ ls
docker-compose.yml
sufiyan@Khan:~/compose-basics$ docker compose up
[+] Running 8/8
 ✔ nginx Pulled                                                                                                                                                                   84.9s 
   ✔ 062e450697fa Pull complete                                                                                                                                                   75.5s 
   ✔ 82454cdbf456 Pull complete                                                                                                                                                   76.5s 
   ✔ 3c7ab7949321 Pull complete                                                                                                                                                   76.5s 
   ✔ cacfcdd01f30 Pull complete                                                                                                                                                   76.5s 
   ✔ b6698f04e005 Pull complete                                                                                                                                                   76.5s 
   ✔ 2bedaf25031a Pull complete                                                                                                                                                   76.5s 
   ✔ d26f27cc8c41 Pull complete                                                                                                                                                   76.5s 
[+] Running 2/2
 ✔ Network compose-basics_default    Created                                                                                                                                       0.1s 
 ✔ Container compose-basics-nginx-1  Created                                                                                                                                       0.6s 
Attaching to nginx-1
nginx-1  | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginx-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginx-1  | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
nginx-1  | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
nginx-1  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginx-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginx-1  | /docker-entrypoint.sh: Configuration complete; ready for start up
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: using the "epoll" event method
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: nginx/1.31.3
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: OS: Linux 6.17.0-40-generic
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 65536:65536
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: start worker processes
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: start worker process 29
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: start worker process 30
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: start worker process 31
nginx-1  | 2026/07/28 14:56:54 [notice] 1#1: start worker process 32
```
```bash
sufiyan@Khan:~/compose-basics$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
9d2659f11ed2   nginx:latest   "/docker-entrypoint.…"   47 seconds ago   Up 46 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   compose-basics-nginx-1
sufiyan@Khan:~/compose-basics$ 
```
### Access it in your browser

<img width="1853" height="516" alt="image" src="https://github.com/user-attachments/assets/f2ebb245-64ba-4e0c-8b11-e5162b6c0a87" />

### Stop it with docker compose down

```bash
sufiyan@Khan:~/compose-basics$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
9d2659f11ed2   nginx:latest   "/docker-entrypoint.…"   47 seconds ago   Up 46 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   compose-basics-nginx-1
sufiyan@Khan:~/compose-basics$ docker compose down
[+] Running 2/2
 ✔ Container compose-basics-nginx-1  Removed                                                                                                                                       0.2s 
 ✔ Network compose-basics_default    Removed                                                                                                                                       0.2s 
sufiyan@Khan:~/compose-basics$ 
```
## Task 3: Two-Container Setup


