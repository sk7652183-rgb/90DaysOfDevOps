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




