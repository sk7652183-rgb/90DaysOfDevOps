# Day 31 – Dockerfile: Build Your Own Images

## Task 1: Your First Dockerfile

### Create a folder called my-first-image

```bash

sufiyan@Khan:~$ ls
 access.log           'Day 26 GitHub CLI Manage.md'   Downloads                   kind-config.yaml       Pictures                       terraform.tfstate
 allow-frontend.yaml   default-deny.yaml             'from fpdf import FPDF.py'   log_rotation.sh        Public                         Test123.txt
 Ansible_Projects      Desktop                        github-cli-practice         MERN-docker-compose    Scripts                        users.csv
 aws-devops            DevOps                         helm                        minikube-linux-amd64  'services java app build.txt'   Videos
 AWS-Session           devops-git-practice            ingress.yml                 monitor_services.sh    Shell-Scripting                write_your_first_terraform_project
 backup.sh             devsecops-demo                 istio-1.27.1                Music                  snap
 config.yml            DevSecOps-Zero-to-Hero         Jenkins-shared-libaries     new-model.json         Sufiya_new
 create_users_1.sh     Docker-Zero-to-Hero            k8s                         new-model.pdf          Templates
 create_users.sh       Documents                      kind-calico.yaml            node                   terraform-infra-environments
sufiyan@Khan:~$ cd DevOps
sufiyan@Khan:~/DevOps$ ls
backups  logs  Scripts
sufiyan@Khan:~/DevOps$ mkdir my-first-image
sufiyan@Khan:~/DevOps$ ls
backups  logs  my-first-image  Scripts
sufiyan@Khan:~/DevOps$ ls -la
total 24
drwxrwxr-x  6 sufiyan sufiyan 4096 Jul  7 22:23 .
drwxr-x--- 49 sufiyan sufiyan 4096 Jul  7 20:13 ..
drwxrwxr-x  3 sufiyan sufiyan 4096 Jun 11 20:01 backups
drwxrwxr-x  2 sufiyan sufiyan 4096 Jun 12 20:15 logs
drwxrwxr-x  2 sufiyan sufiyan 4096 Jul  7 22:23 my-first-image
drwxrwxr-x  3 sufiyan sufiyan 4096 Jul  2 02:00 Scripts
sufiyan@Khan:~/DevOps$

```


### Inside it, create a Dockerfile that uses Ubuntu as the base image, installs curl, and sets a default command to print "Hello from my custom image!" Build the image and tag it as my-ubuntu:v1 using docker build -t my-ubuntu:v1 ..

```bash

sufiyan@Khan:~/DevOps/my-first-image$ docker build -t my-ubuntu:v1 .
[+] Building 230.2s (7/7) FINISHED                                                                                                                                       docker:default
 => [internal] load build definition from Dockerfile                                                                                                                               0.0s
 => => transferring dockerfile: 244B                                                                                                                                               0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                   4.8s
 => [auth] library/ubuntu:pull token for registry-1.docker.io                                                                                                                      0.0s
 => [internal] load .dockerignore                                                                                                                                                  0.0s
 => => transferring context: 2B                                                                                                                                                    0.0s
 => [1/2] FROM docker.io/library/ubuntu:latest@sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e                                                            70.7s
 => => resolve docker.io/library/ubuntu:latest@sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e                                                             0.0s
 => => sha256:4aaf0b273f92a76e458efc72cef4893c2c54ae2f1451d07112f1ce79f3ac0487 4.38kB / 4.38kB                                                                                     0.0s
 => => sha256:a9be9fd915e97ef977c92b5f9abe226548f0d6a4a013daef6d238708ccde9b61 41.56MB / 41.56MB                                                                                  67.9s
 => => sha256:2c1ce1d0a589804cc5314c1325593106b364e9d700502a06de710671c7697220 393B / 393B                                                                                         1.6s
 => => sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e 6.69kB / 6.69kB                                                                                     0.0s
 => => sha256:c6c0067e0e45b7a826eaebb193cef957be28045380963a9b1eeb2a5d3c70a1b9 1.39kB / 1.39kB                                                                                     0.0s
 => => extracting sha256:a9be9fd915e97ef977c92b5f9abe226548f0d6a4a013daef6d238708ccde9b61                                                                                          2.6s
 => => extracting sha256:2c1ce1d0a589804cc5314c1325593106b364e9d700502a06de710671c7697220                                                                                          0.0s
 => [2/2] RUN apt-get update &&    apt-get install -y curl &&    rm -rf /var/lib/apt/lits/*                                                                                      154.2s
 => exporting to image                                                                                                                                                             0.5s 
 => => exporting layers                                                                                                                                                            0.5s 
 => => writing image sha256:8b6d6bc84bb9a37e4dfc76cb43e014da7d7fb6336ba90373c74908c14e364d16                                                                                       0.0s 
 => => naming to docker.io/library/my-ubuntu:v1                                                                                                                                    0.0s 
sufiyan@Khan:~/DevOps/my-first-image$ docker images                                                                                                                                     
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE                                                                                                                              
my-ubuntu    v1        8b6d6bc84bb9   9 seconds ago   159MB
sufiyan@Khan:~/DevOps/my-first-image$ batcat Dockerfile
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM ubuntu
   2   │ 
   3   │ # Update package index and install curl 
   4   │ RUN apt-get update &&\
   5   │     apt-get install -y curl &&\
   6   │     rm -rf /var/lib/apt/lits/*
   7   │ 
   8   │ # Default command 
   9   │ 
  10   │ CMD ["echo","Hello from my custom image!"]
  11   │ 
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sufiyan@Khan:~/DevOps/my-first-image$
```
### Run a container from your image

```bash

sufiyan@Khan:~/DevOps/my-first-image$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
my-ubuntu    v1        8b6d6bc84bb9   5 minutes ago   159MB
sufiyan@Khan:~/DevOps/my-first-image$ docker run -d my-ubuntu:v1
a152297cf845d70955744c0ab90694c514cf18234cea66caac43d6aa742d4973
sufiyan@Khan:~/DevOps/my-first-image$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
sufiyan@Khan:~/DevOps/my-first-image$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                      PORTS     NAMES
a152297cf845   my-ubuntu:v1   "echo 'Hello from my…"   17 seconds ago   Exited (0) 16 seconds ago             gifted_yonath
sufiyan@Khan:~/DevOps/my-first-image$ docker run --rm my-ubuntu:v1
Hello from my custom image!
sufiyan@Khan:~/DevOps/my-first-image$ 
```

## Task 2: Dockerfile Instructions

### Create a new Dockerfile that uses all of these instructions:
```bash
sufiyan@Khan:~/DevOps/hotelhub$ ls
accounts  Dockerfile  guest  hotelmanagement  LICENSE  main  manage.py  readme.md  requirements.txt  room  screenshots
sufiyan@Khan:~/DevOps/hotelhub$ docker build -t hotelhub:v1 .
[+] Building 1.2s (10/10) FINISHED                                                                                                                                       docker:default
 => [internal] load build definition from Dockerfile                                                                                                                               0.0s
 => => transferring dockerfile: 309B                                                                                                                                               0.0s
 => [internal] load metadata for docker.io/library/python:3.11-slim                                                                                                                1.1s
 => [internal] load .dockerignore                                                                                                                                                  0.0s
 => => transferring context: 2B                                                                                                                                                    0.0s
 => [1/5] FROM docker.io/library/python:3.11-slim@sha256:c20888b6acdd1e63e1c433a185bf3ad162c0288fe484616ce062e0d28add2900                                                          0.0s
 => [internal] load build context                                                                                                                                                  0.0s
 => => transferring context: 5.69kB                                                                                                                                                0.0s
 => CACHED [2/5] WORKDIR /app                                                                                                                                                      0.0s
 => CACHED [3/5] COPY requirements.txt .                                                                                                                                           0.0s
 => CACHED [4/5] RUN pip install --no-cache-dir -r requirements.txt                                                                                                                0.0s
 => CACHED [5/5] COPY . .                                                                                                                                                          0.0s
 => exporting to image                                                                                                                                                             0.0s
 => => exporting layers                                                                                                                                                            0.0s
 => => writing image sha256:321deaa54d63f45e88a1edf8cf4ec99bfe01b7adc108587aff4d6bb70c61ec29                                                                                       0.0s
 => => naming to docker.io/library/hotelhub:v1                                                                                                                                     0.0s
sufiyan@Khan:~/DevOps/hotelhub$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
hotelhub     v1        321deaa54d63   24 minutes ago   169MB
my-ubuntu    v1        8b6d6bc84bb9   2 hours ago      159MB
sufiyan@Khan:~/DevOps/hotelhub$ docker run -d -p 50:8000 hotelhub:v1
e1aafd2c5f45f08e78c78e7bf89f054af5129527a9c73381089fc447f06d4c14
sufiyan@Khan:~/DevOps/hotelhub$ docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS                                     NAMES
e1aafd2c5f45   hotelhub:v1   "sh -c 'python manag…"   6 seconds ago   Up 6 seconds   0.0.0.0:50->8000/tcp, [::]:50->8000/tcp   exciting_germain
sufiyan@Khan:~/DevOps/hotelhub$ docker exec -it e1aafd2c5f45 sh
# ls
Dockerfile  LICENSE  accounts  db.sqlite3  guest  hotelmanagement  main  manage.py  readme.md  requirements.txt  room  screenshots
# python manage.py createsuperuser
System check identified some issues:

WARNINGS:
?: (staticfiles.W004) The directory '/app/static' in the STATICFILES_DIRS setting does not exist.
Username (leave blank to use 'root'): admin
Email address: test123@gmail.com
Password: 
Password (again): 
This password is entirely numeric.
Bypass password validation and create user anyway? [y/N]: y
Superuser created successfully.
# 
sufiyan@Khan:~/DevOps/hotelhub$ batcat Dockerfile 
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM python:3.11-slim
   2   │ 
   3   │ WORKDIR /app
   4   │ 
   5   │ COPY requirements.txt .
   6   │ 
   7   │ RUN pip install --no-cache-dir -r requirements.txt
   8   │ 
   9   │ COPY . .
  10   │ 
  11   │ EXPOSE 8000
  12   │ 
  13   │ 
  14   │ CMD ["sh", "-c", "python manage.py makemigrations && python manage.py migrate && python manage.py runserver 0.0.0.0:8000"]
  15   │ 
  16   │ 
  17   │ 
  18   │ 
  19   │     
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sufiyan@Khan:~/DevOps/hotelhub$ 
```
Screenshot of the Running Application 

<img width="1855" height="1004" alt="image" src="https://github.com/user-attachments/assets/da7a0387-8e6d-41a6-a8b5-5f893ad256f7" />

## Task 3: CMD vs ENTRYPOINT

### Create an image with CMD ["echo", "hello"] — run it, then run it with a custom command. What happens?

```bash

sufiyan@Khan:~/DevOps/my-first-image$ ls
Dockerfile
sufiyan@Khan:~/DevOps/my-first-image$ docker build -t my-ubuntu:v2 .
[+] Building 3.0s (7/7) FINISHED                                                                                                                                         docker:default
 => [internal] load build definition from Dockerfile                                                                                                                               0.0s
 => => transferring dockerfile: 230B                                                                                                                                               0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                   2.9s
 => [auth] library/ubuntu:pull token for registry-1.docker.io                                                                                                                      0.0s
 => [internal] load .dockerignore                                                                                                                                                  0.0s
 => => transferring context: 2B                                                                                                                                                    0.0s
 => [1/2] FROM docker.io/library/ubuntu:latest@sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e                                                             0.0s
 => CACHED [2/2] RUN apt-get update &&    apt-get install -y curl &&    rm -rf /var/lib/apt/lits/*                                                                                 0.0s
 => exporting to image                                                                                                                                                             0.0s
 => => exporting layers                                                                                                                                                            0.0s
 => => writing image sha256:8c07201393bf111240f4936a4a484b718cb78902760cf8010811b2492b0f487b                                                                                       0.0s
 => => naming to docker.io/library/my-ubuntu:v2                                                                                                                                    0.0s
sufiyan@Khan:~/DevOps/my-first-image$ docker run -p 51:51 my-ubuntu:v2
Hello Doston!
sufiyan@Khan:~/DevOps/my-first-image$ batcat Dockerfile
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM ubuntu
   2   │ 
   3   │ # Update package index and install curl 
   4   │ RUN apt-get update &&\
   5   │     apt-get install -y curl &&\
   6   │     rm -rf /var/lib/apt/lits/*
   7   │ 
   8   │ # Default command 
   9   │ 
  10   │ CMD ["echo","Hello Doston!"]
  11   │ 
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sufiyan@Khan:~/DevOps/my-first-image$ 
```
### Run it with a custom command:

```bash
sufiyan@Khan:~/DevOps/my-first-image$ docker run --rm my-ubuntu:v2 echo "Hello test"
Hello test
sufiyan@Khan:~/DevOps/my-first-image$ 
```
CMD provides a default command for the container, and any command specified with docker run replaces it.

### Create an image with ENTRYPOINT ["echo"] — run it, then run it with additional arguments. What happens?

```bash

sufiyan@Khan:~/DevOps/my-first-image$ vim Dockerfile
sufiyan@Khan:~/DevOps/my-first-image$ docker build -t my-ubuntu:v3 .
[+] Building 1.7s (7/7) FINISHED                                                                                                                                         docker:default
 => [internal] load build definition from Dockerfile                                                                                                                               0.0s
 => => transferring dockerfile: 237B                                                                                                                                               0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                   1.7s
 => [auth] library/ubuntu:pull token for registry-1.docker.io                                                                                                                      0.0s
 => [internal] load .dockerignore                                                                                                                                                  0.0s
 => => transferring context: 2B                                                                                                                                                    0.0s
 => [1/2] FROM docker.io/library/ubuntu:latest@sha256:b7f48194d4d8b763a478a621cdc81c27be222ba2206ca3ca6bc42b49685f3d9e                                                             0.0s
 => CACHED [2/2] RUN apt-get update &&    apt-get install -y curl &&    rm -rf /var/lib/apt/lits/*                                                                                 0.0s
 => exporting to image                                                                                                                                                             0.0s
 => => exporting layers                                                                                                                                                            0.0s
 => => writing image sha256:ec325cea55d0dca06d47888b521bafaa52c1bdae3c095a3309bb2f9572f3f876                                                                                       0.0s
 => => naming to docker.io/library/my-ubuntu:v3                                                                                                                                    0.0s
sufiyan@Khan:~/DevOps/my-first-image$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
hotelhub     v1        321deaa54d63   15 hours ago   169MB
my-ubuntu    v1        8b6d6bc84bb9   16 hours ago   159MB
my-ubuntu    v2        8c07201393bf   16 hours ago   159MB
my-ubuntu    v3        ec325cea55d0   16 hours ago   159MB
sufiyan@Khan:~/DevOps/my-first-image$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
sufiyan@Khan:~/DevOps/my-first-image$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                      PORTS     NAMES
10fc320c80d3   my-ubuntu:v2   "echo 'Hello Doston!'"   10 minutes ago   Exited (0) 10 minutes ago             priceless_lamarr
28b9af5f9a6b   my-ubuntu:v1   "echo 'Hello from my…"   15 minutes ago   Exited (0) 15 minutes ago             confident_diffie
e1aafd2c5f45   hotelhub:v1    "sh -c 'python manag…"   14 hours ago     Exited (137) 14 hours ago             exciting_germain
a152297cf845   my-ubuntu:v1   "echo 'Hello from my…"   16 hours ago     Exited (0) 16 hours ago               gifted_yonath
sufiyan@Khan:~/DevOps/my-first-image$ docker run -p 52:52 my-ubuntu:v3
Hello Doston!
sufiyan@Khan:~/DevOps/my-first-image$ batcat Dockerfile
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM ubuntu
   2   │ 
   3   │ # Update package index and install curl 
   4   │ RUN apt-get update &&\
   5   │     apt-get install -y curl &&\
   6   │     rm -rf /var/lib/apt/lits/*
   7   │ 
   8   │ # Default command 
   9   │ 
  10   │ ENTRYPOINT ["echo","Hello Doston!"]
```

### Run it without arguments:

``` bash
sufiyan@Khan:~/DevOps/my-first-image$ docker run --rm my-ubuntu:v3
Hello Doston!
```

### Run it with additional arguments:

```bash
sufiyan@Khan:~/DevOps/my-first-image$ docker run --rm my-ubuntu:v3  demo-Hello Docker
Hello Doston! demo-Hello Docker
sufiyan@Khan:~/DevOps/my-first-image$ 
```
ENTRYPOINT defines the fixed command to run, and arguments passed to docker run are appended to it.

### Write in your notes: When would you use CMD vs ENTRYPOINT?
### CMD vs ENTRYPOINT

* **CMD**: Use `CMD` when you want to provide a **default command or arguments** that users can easily override with `docker run`.
* **ENTRYPOINT**: Use `ENTRYPOINT` when you want the container to **always run a specific executable**, while allowing users to pass additional arguments to it.

**Key difference:** `CMD` is easily overridden by the command passed to `docker run`, whereas `ENTRYPOINT` remains fixed and `docker run` arguments are appended to it.

### Task 4: Build a Simple Web App Image

### Static HTML Portfolio

Created a simple static `index.html` portfolio page containing:

* Personal introduction
* About Me section
* Technical skills
* DevOps projects
* Contact information

This page can be served using any web server (such as Nginx or Apache) or deployed inside a Docker container.

### Write a Dockerfile that:
Uses nginx:alpine as base
Copies your index.html to the Nginx web directory

```bash

sufiyan@Khan:~/DevOps/portfolio$ ls
Dockerfile  index.html
sufiyan@Khan:~/DevOps/portfolio$ docker build -t portfolio .
[+] Building 33.5s (9/9) FINISHED                                                                                                                                        docker:default
 => [internal] load build definition from Dockerfile                                                                                                                               0.0s
 => => transferring dockerfile: 131B                                                                                                                                               0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                                                    7.8s
 => [auth] library/nginx:pull token for registry-1.docker.io                                                                                                                       0.0s
 => [internal] load .dockerignore                                                                                                                                                  0.0s
 => => transferring context: 2B                                                                                                                                                    0.0s
 => [1/3] FROM docker.io/library/nginx:alpine@sha256:54f2a904c251d5a34adf545a72d32515a15e08418dae0266e23be2e18c66fefa                                                             25.2s
 => => resolve docker.io/library/nginx:alpine@sha256:54f2a904c251d5a34adf545a72d32515a15e08418dae0266e23be2e18c66fefa                                                              0.0s
 => => sha256:54f2a904c251d5a34adf545a72d32515a15e08418dae0266e23be2e18c66fefa 10.33kB / 10.33kB                                                                                   0.0s
 => => sha256:35cd77497979abe70dc8d26f5ae60811eea233a2eb5dc03c2ee30972caeb303e 2.50kB / 2.50kB                                                                                     0.0s
 => => sha256:ea51152ef8c480b999cee0271a288c698704b18483276119b1253e576ef3232f 12.32kB / 12.32kB                                                                                   0.0s
 => => sha256:e6f31ffc071e5560b82a8685fba8214954e5721e3e49269d00958316edbe89fe 3.84MB / 3.84MB                                                                                    10.9s
 => => sha256:c16defe09b2f86b04c4143bb610095be90732794fce5c56fb7185f02feadd879 1.89MB / 1.89MB                                                                                     7.8s
 => => sha256:5b429a43b8dfa079c3ac95537bd88d5f1ac70e478c64f100b8ef6aa9c555ebc2 628B / 628B                                                                                         1.2s
 => => sha256:967885d218c57d3ce2a4e906131fb25f59e6f56cce51d87dde7d74b0e7465675 955B / 955B                                                                                         1.8s
 => => sha256:ab1fd90497517c799d4fb351bc1a7ae8b58e231345d4948ae8ac73c75b320b35 405B / 405B                                                                                         2.2s
 => => sha256:ce42635eeddd348e8266227a36db92cea8a45d5177d03e9a922a7bfb25762b7f 1.21kB / 1.21kB                                                                                     2.6s
 => => sha256:01bf363d61e6136ebd7dcaf74b303bd08ee8f849a04e2c0be5a8d03159b404f6 1.40kB / 1.40kB                                                                                     3.0s
 => => sha256:c75b9c33e8b0983941fe03793379bd5d137f641f92b4a94d1c8956d28ba77f65 20.25MB / 20.25MB                                                                                  24.4s
 => => extracting sha256:e6f31ffc071e5560b82a8685fba8214954e5721e3e49269d00958316edbe89fe                                                                                          0.1s
 => => extracting sha256:c16defe09b2f86b04c4143bb610095be90732794fce5c56fb7185f02feadd879                                                                                          0.1s
 => => extracting sha256:5b429a43b8dfa079c3ac95537bd88d5f1ac70e478c64f100b8ef6aa9c555ebc2                                                                                          0.0s
 => => extracting sha256:967885d218c57d3ce2a4e906131fb25f59e6f56cce51d87dde7d74b0e7465675                                                                                          0.0s
 => => extracting sha256:ab1fd90497517c799d4fb351bc1a7ae8b58e231345d4948ae8ac73c75b320b35                                                                                          0.0s
 => => extracting sha256:ce42635eeddd348e8266227a36db92cea8a45d5177d03e9a922a7bfb25762b7f                                                                                          0.0s
 => => extracting sha256:01bf363d61e6136ebd7dcaf74b303bd08ee8f849a04e2c0be5a8d03159b404f6                                                                                          0.0s
 => => extracting sha256:c75b9c33e8b0983941fe03793379bd5d137f641f92b4a94d1c8956d28ba77f65                                                                                          0.6s
 => [internal] load build context                                                                                                                                                  0.0s
 => => transferring context: 2.25kB                                                                                                                                                0.0s
 => [2/3] WORKDIR /app                                                                                                                                                             0.2s
 => [3/3] COPY index.html /usr/share/nginx/html/index.html                                                                                                                         0.0s
 => exporting to image                                                                                                                                                             0.0s
 => => exporting layers                                                                                                                                                            0.0s
 => => writing image sha256:5a8d72689b98f0262bc7eeae1fae2d641fdd1c9b3de2393425fbde32baa485bc                                                                                       0.0s
 => => naming to docker.io/library/portfolio                                                                                                                                       0.0s
```
```bash

sufiyan@Khan:~/DevOps/portfolio$ batcat Dockerfile
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: Dockerfile
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ FROM nginx:alpine
   2   │ 
   3   │ WORKDIR /app
   4   │ 
   5   │ COPY index.html /usr/share/nginx/html/index.html
   6   │ 
   7   │ EXPOSE 80
```

### Build and tag it my-website:v1

```bash

sufiyan@Khan:~/DevOps/portfolio$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
portfolio    latest    5a8d72689b98   14 seconds ago   62.2MB
hotelhub     v1        321deaa54d63   15 hours ago     169MB
my-ubuntu    v1        8b6d6bc84bb9   17 hours ago     159MB
my-ubuntu    v2        8c07201393bf   17 hours ago     159MB
my-ubuntu    v3        ec325cea55d0   17 hours ago     159MB
```
### Run it with port mapping and access it in your browser

```bash
sufiyan@Khan:~/DevOps/portfolio$ docker run -d -p 8082:80 portfolio:v1
1572809c3fde2adc6afdd4dc347b66ef3e238d258240de1a0240fecb72c03c6f
sufiyan@Khan:~/DevOps/portfolio$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                             NAMES
1572809c3fde   portfolio:v1   "/docker-entrypoint.…"   4 seconds ago   Up 4 seconds   70/tcp, 0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   confident_nightingale
sufiyan@Khan:~/DevOps/portfolio$ 
```

<img width="1856" height="1004" alt="image" src="https://github.com/user-attachments/assets/67d423ad-8243-4b9a-823b-c74a46dc3e86" />

### Task 5: .dockerignore

```bash
sufiyan@Khan:~/DevOps/portfolio$ docker build -t my-app .
[+] Building 2.4s (9/9) FINISHED                                                                                                                                         docker:default
 => [internal] load build definition from Dockerfile                                                                                                                               0.0s
 => => transferring dockerfile: 131B                                                                                                                                               0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                                                    2.3s
 => [auth] library/nginx:pull token for registry-1.docker.io                                                                                                                       0.0s
 => [internal] load .dockerignore                                                                                                                                                  0.0s
 => => transferring context: 68B                                                                                                                                                   0.0s
 => [1/3] FROM docker.io/library/nginx:alpine@sha256:54f2a904c251d5a34adf545a72d32515a15e08418dae0266e23be2e18c66fefa                                                              0.0s
 => [internal] load build context                                                                                                                                                  0.0s
 => => transferring context: 32B                                                                                                                                                   0.0s
 => CACHED [2/3] WORKDIR /app                                                                                                                                                      0.0s
 => CACHED [3/3] COPY index.html /usr/share/nginx/html/index.html                                                                                                                  0.0s
 => exporting to image                                                                                                                                                             0.0s
 => => exporting layers                                                                                                                                                            0.0s
 => => writing image sha256:0344f6c343e586abb56607be2a5f697fb0775683a48292e4462da73c2aa9ff08                                                                                       0.0s
 => => naming to docker.io/library/my-app                                                                                                                                          0.0s
sufiyan@Khan:~/DevOps/portfolio$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
portfolio    v1        0344f6c343e5   20 minutes ago   62.2MB
my-app       latest    0344f6c343e5   20 minutes ago   62.2MB
hotelhub     v1        321deaa54d63   16 hours ago     169MB
my-ubuntu    v2        8c07201393bf   17 hours ago     159MB
my-ubuntu    v3        ec325cea55d0   17 hours ago     159MB
my-ubuntu    v1        8b6d6bc84bb9   17 hours ago     159MB
sufiyan@Khan:~/DevOps/portfolio$ docker run --rm -it my-app sh
/app # ls
/app # ls -la
total 8
drwxr-xr-x    2 root     root          4096 Jul  8 09:51 .
drwxr-xr-x    1 root     root          4096 Jul  8 10:50 ..
/app # 
```

The files and directories specified in the .dockerignore file are excluded from the Docker build context, resulting in a smaller and faster image build.

### Task 6: Build Optimization

### Docker Build Cache

Built the Docker image and then modified one line in the project before rebuilding it.

**Observation:**

* Docker reused the cached layers that were unchanged.
* Only the layer affected by the modified file and the layers after it were rebuilt.
* This significantly reduced the build time.

### Optimizing Dockerfile for Better Caching

To take advantage of Docker's layer cache, place instructions that **change infrequently** (such as installing dependencies) near the top of the Dockerfile, and place instructions that **change frequently** (such as `COPY . .`) near the end.

**Example:**

```dockerfile
FROM node:20-alpine

WORKDIR /app

# Changes infrequently
COPY package*.json ./
RUN npm install

# Changes frequently
COPY . .

CMD ["npm", "start"]
```

### Why Does Layer Order Matter?

Docker builds images layer by layer and caches each layer. If a layer changes, Docker rebuilds that layer and every layer after it. By placing frequently changing instructions near the end of the Dockerfile, Docker can reuse more cached layers, resulting in faster image builds.




