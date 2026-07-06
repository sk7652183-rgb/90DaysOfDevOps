# Day 30 – Docker Images & Container Lifecycle

## Task 1: Docker Images

### Pull the nginx, ubuntu, and alpine images from Docker Hub
```bash
sufiyan@Khan:~$ docker run -d --name nginx_container -p 8080:80 nginx:latest
4bd6ecfcde700f2eea5890aaf5ef91cad6ab31d8e1372c118a98909645af02c6
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS          PORTS                                     NAMES
4bd6ecfcde70   nginx:latest           "/docker-entrypoint.…"   8 seconds ago   Up 7 seconds    0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx_container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 15 minutes   127.0.0.1:44885->6443/tcp                 kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 15 minutes                                             kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 15 minutes   127.0.0.1:36809->6443/tcp                 k8s.demo-control-plane
sufiyan@Khan:~$ docker run -it --name alpine_container alpine:latest
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
55afa1ecc21d: Pull complete 
Digest: sha256:28bd5fe8b56d1bd048e5babf5b10710ebe0bae67db86916198a6eec434943f8b
Status: Downloaded newer image for alpine:latest
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # cd var
/var # ls
cache  empty  lib    local  lock   log    mail   opt    run    spool  tmp
/var # exit
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED              STATUS              PORTS                                     NAMES
4bd6ecfcde70   nginx:latest           "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   nginx_container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago         Up 17 minutes       127.0.0.1:44885->6443/tcp                 kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago         Up 17 minutes                                                 kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago         Up 17 minutes       127.0.0.1:36809->6443/tcp                 k8s.demo-control-plane
sufiyan@Khan:~$ docker run -it --name ubuntu_container ubuntu:latest
root@f7d833fb3a82:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@f7d833fb3a82:/# cd sbin
root@f7d833fb3a82:/sbin# ls
add-shell   dpkg-preconfigure  e2scrub_all  fsck.ext3     groupmod       ldconfig          mklost+found           pam_timestamp_check  rmt                swapoff        update-shells
agetty      dpkg-reconfigure   e2undo       fsck.ext4     grpck          logsave           mkswap                 policy-rc.d          rmt-tar            swapon         useradd
badblocks   dumpe2fs           e4crypt      fsfreeze      grpconv        losetup           newusers               pwck                 rtcwake            sysctl         userdel
blkdiscard  e2freefrag         e4defrag     fstab-decode  grpunconv      mke2fs            nologin                pwconv               runuser            tarcat         usermod
blkid       e2fsck             faillock     fstrim        iconvconfig    mkfs              pam-auth-update        pwhistory_helper     service            tune2fs        vigr
blockdev    e2image            filefrag     getty         initctl        mkfs.ext2         pam_extrausers_chkpwd  pwunconv             shadowconfig       unix_chkpwd    vipw
chgpasswd   e2label            findfs       gnuchroot     installkernel  mkfs.ext3         pam_extrausers_update  readprofile          start-stop-daemon  unix_update    wipefs
chpasswd    e2mmpstatus        fsck         groupadd      invoke-rc.d    mkfs.ext4         pam_getenv             remove-shell         sulogin            update-passwd  zic
debugfs     e2scrub            fsck.ext2    groupdel      killall5       mkhomedir_helper  pam_namespace_helper   resize2fs            swaplabel          update-rc.d    zramctl
root@f7d833fb3a82:/sbin# exit
exit
sufiyan@Khan:~$ 
```

### List all images on your machine — note the sizes

```bash

sufiyan@Khan:~$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
ubuntu                        latest    4aaf0b273f92   8 days ago      100MB
nginx                         latest    9f33606b3685   11 days ago     161MB
alpine                        latest    d529dd0c6e55   2 weeks ago     8.42MB
hello-world                   latest    e2ac70e7319a   3 months ago    10.1kB
gcr.io/k8s-minikube/kicbase   v0.0.49   df72754dcb7f   5 months ago    1.35GB
gcr.io/k8s-minikube/kicbase   v0.0.47   795ea6a69ce6   13 months ago   1.31GB
busybox                       latest    af3f0f48a24e   21 months ago   4.43MB
kindest/node                  <none>    9319cf209ac5   2 years ago     974MB
```

### Compare ubuntu vs alpine — why is one much smaller?
Alpine is much smaller than Ubuntu because it includes only the minimum required packages and uses BusyBox and musl instead of the larger GNU utilities and glibc.

### Inspect an image — what information can you see?
By inspecting a Docker image, I can see information such as the repository, tag, image ID, creation date, and image size

### Remove an image you no longer need

```bash

sufiyan@Khan:~$ docker rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:96498ffd522e70807ab6384a5c0485a79b9c7c08ca79ba08623edcad1054e62d
Deleted: sha256:e2ac70e7319a02c5a477f5825259bd118b94e8b02c279c67afa63adab6d8685b
Deleted: sha256:897b3f2a7c1bc2f3d02432f7892fe31c6272c521ad4d70257df624504a3238b4
sufiyan@Khan:~$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
ubuntu                        latest    4aaf0b273f92   8 days ago      100MB
nginx                         latest    9f33606b3685   11 days ago     161MB
alpine                        latest    d529dd0c6e55   2 weeks ago     8.42MB
gcr.io/k8s-minikube/kicbase   v0.0.49   df72754dcb7f   5 months ago    1.35GB
gcr.io/k8s-minikube/kicbase   v0.0.47   795ea6a69ce6   13 months ago   1.31GB
busybox                       latest    af3f0f48a24e   21 months ago   4.43MB
kindest/node                  <none>    9319cf209ac5   2 years ago     974MB

```

## Task 2: Image Layers

### Run docker image history nginx — what do you see? 

```bash
sufiyan@Khan:~$ docker history nginx
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
9f33606b3685   12 days ago   CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      12 days ago   STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      12 days ago   EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      12 days ago   ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      12 days ago   COPY 30-tune-worker-processes.sh /docker-ent…   4.62kB    buildkit.dockerfile.v0
<missing>      12 days ago   COPY 20-envsubst-on-templates.sh /docker-ent…   3.03kB    buildkit.dockerfile.v0
<missing>      12 days ago   COPY 15-local-resolvers.envsh /docker-entryp…   389B      buildkit.dockerfile.v0
<missing>      12 days ago   COPY 10-listen-on-ipv6-by-default.sh /docker…   2.12kB    buildkit.dockerfile.v0
<missing>      12 days ago   COPY docker-entrypoint.sh / # buildkit          1.62kB    buildkit.dockerfile.v0
<missing>      12 days ago   RUN /bin/sh -c set -x     && groupadd --syst…   82.7MB    buildkit.dockerfile.v0
<missing>      12 days ago   ENV DYNPKG_RELEASE=1~trixie                     0B        buildkit.dockerfile.v0
<missing>      12 days ago   ENV PKG_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      12 days ago   ENV ACME_VERSION=0.4.1                          0B        buildkit.dockerfile.v0
<missing>      12 days ago   ENV NJS_RELEASE=1~trixie                        0B        buildkit.dockerfile.v0
<missing>      12 days ago   ENV NJS_VERSION=0.9.9                           0B        buildkit.dockerfile.v0
<missing>      12 days ago   ENV NGINX_VERSION=1.31.2                        0B        buildkit.dockerfile.v0
<missing>      12 days ago   LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      13 days ago   # debian.sh --arch 'amd64' out/ 'trixie' '@1…   78.6MB    debuerreotype 0.17
sufiyan@Khan:~$ 
```
The `docker history nginx` command displays the layers used to build the Nginx Docker image. Each layer corresponds to a Dockerfile instruction such as `RUN`, `COPY`, `ENV`, `EXPOSE`, and `CMD`. This helps understand the image structure and identify which layers contribute to the image size.

### Each line is a layer. Note how some layers show sizes and some show 0B

### Observation

- Each row in the output represents a **layer** of the Docker image.
- Layers created by instructions such as `RUN` usually have a non-zero size because they add files or install software.
- Layers created by metadata instructions such as `CMD`, `ENV`, `EXPOSE`, `ENTRYPOINT`, `LABEL`, and `STOPSIGNAL` typically show **0B** because they only modify the image configuration and do not add any filesystem data.
- The largest layers in the Nginx image come from installing the operating system packages and Nginx itself, while configuration-related instructions add little or no size.

### Example

```bash
IMAGE          CREATED       CREATED BY                            SIZE
9f33606b3685   12 days ago   CMD ["nginx" "-g" "daemon off;"]      0B
<missing>      12 days ago   EXPOSE 80/tcp                         0B
<missing>      12 days ago   ENTRYPOINT ["/docker-entrypoint.sh"]  0B
<missing>      12 days ago   RUN ...                               82.7MB
<missing>      13 days ago   Debian base image                     78.6MB
```
### Write in your notes: What are layers and why does Docker use them?
Docker images are built from multiple layers, where each layer represents a single instruction in the Dockerfile, such as RUN, COPY, ADD, or ENV. These layers are stacked on top of one another to create the final Docker image.

Docker uses layers because they are cached and reusable. When an image is rebuilt, Docker checks whether a layer has changed. If a layer has not changed, Docker reuses the cached version instead of rebuilding it. This caching mechanism speeds up the image build process, reduces build time, and saves storage by sharing common layers between multiple images.


## Task 3: Container Lifecycle

### Create a container (without starting it)

The docker create command creates a new container from the specified image but does not start it. The container is created in the Created state and can be started later using:

```bash
sufiyan@Khan:~$ docker create --name my-nginx nginx
158fde8bdc7533d9e7f14507f615fe816b1293bbe6f0cc5a7b5281489b01f56b
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED        STATUS          PORTS                       NAMES
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago   Up 32 minutes   127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago   Up 32 minutes                               kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago   Up 32 minutes   127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   11 seconds ago   Created                                                 my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up 32 minutes               127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up 32 minutes                                           kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up 32 minutes               127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

### Start the container

The docker start command starts an existing container. After starting the container, the docker ps -a command shows its status as Up 13 seconds, indicating that the container is currently running successfully.

```bash
sufiyan@Khan:~$ docker start my-nginx
my-nginx
sufiyan@Khan:~$ docker ps 
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS          PORTS                       NAMES
158fde8bdc75   nginx                  "/docker-entrypoint.…"   4 minutes ago   Up 5 seconds    80/tcp                      my-nginx
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 37 minutes   127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 37 minutes                               kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 37 minutes   127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS                      PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   4 minutes ago   Up 13 seconds               80/tcp                      my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago    Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago    Up 37 minutes               127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago    Up 37 minutes                                           kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago    Up 37 minutes               127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```
### Pause it and check status

The docker stop command stops a running container. After stopping the container, the docker ps -a command shows its status as Exited (0) 3 seconds ago, indicating that the container has stopped successfully. The exit code 0 means the container terminated normally without any errors.

```bash
sufiyan@Khan:~$ docker stop my-nginx
my-nginx
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED         STATUS                      PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   8 minutes ago   Exited (0) 3 seconds ago                                my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago    Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago    Up 41 minutes               127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago    Up 41 minutes                                           kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago    Up 41 minutes               127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

### Unpause it

The docker start command starts a previously stopped container. After executing the command, docker ps -a shows the status as Up 2 seconds, indicating that the container is running successfully.

```bash
sufiyan@Khan:~$ docker start my-nginx
my-nginx
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   32 minutes ago   Up 2 seconds                80/tcp                      my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour                                        kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

### Stop it

The docker stop command stops the running my-nginx container. After executing the command, docker ps -a shows the container status as Exited (0) 3 seconds ago, indicating that the container stopped successfully. The exit code 0 means the container terminated normally without any errors.

```bash

sufiyan@Khan:~$ docker stop my-nginx
my-nginx
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   37 minutes ago   Exited (0) 3 seconds ago                                my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour                                        kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

### Restart it

```bash

sufiyan@Khan:~$ docker restart my-nginx
my-nginx
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                      PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   39 minutes ago   Up 2 seconds                80/tcp                      my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour                                        kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour            127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

### Kill it

The docker kill command immediately terminates the running container by sending it a SIGKILL signal. After executing the command, docker ps -a shows the container status as Exited, indicating that the container was forcefully stopped.

```bash
sufiyan@Khan:~$ docker kill my-nginx
my-nginx
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                       PORTS                       NAMES
158fde8bdc75   nginx                                 "/docker-entrypoint.…"   41 minutes ago   Exited (137) 2 seconds ago                               my-nginx
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago                                minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour             127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour                                         kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Up About an hour             127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 

```

### Remove it

The docker rm command removes the stopped my-nginx container from the system. After executing the command, docker ps -a no longer lists the container, confirming that it has been removed successfully.

```bash

sufiyan@Khan:~$ docker rm my-nginx
my-nginx
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED        STATUS                      PORTS                       NAMES
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago   Exited (137) 4 months ago                               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago   Up About an hour            127.0.0.1:44885->6443/tcp   kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago   Up About an hour                                        kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago   Up About an hour            127.0.0.1:36809->6443/tcp   k8s.demo-control-plane
sufiyan@Khan:~$ 
```

## Task 4: Working with Running Containers

### Run an Nginx container in detached mode

```bash
sufiyan@Khan:~$ docker run -d --name nginx_container -p 80:80 nginx:latest
12b9885be4730f4c0812cb4dda10101979e3f741b983efe899ad5e820a009cb6
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS                                 NAMES
12b9885be473   nginx:latest           "/docker-entrypoint.…"   7 seconds ago   Up 7 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   nginx_container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 2 hours     127.0.0.1:44885->6443/tcp             kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 2 hours                                           kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago    Up 2 hours     127.0.0.1:36809->6443/tcp             k8s.demo-control-plane
sufiyan@Khan:~$ 
```
### View its logs

```bash

sufiyan@Khan:~$ docker logs nginx_container
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/07/06 10:42:53 [notice] 1#1: using the "epoll" event method
2026/07/06 10:42:53 [notice] 1#1: nginx/1.31.2
2026/07/06 10:42:53 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/07/06 10:42:53 [notice] 1#1: OS: Linux 6.17.0-35-generic
2026/07/06 10:42:53 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 65536:65536
2026/07/06 10:42:53 [notice] 1#1: start worker processes
2026/07/06 10:42:53 [notice] 1#1: start worker process 29
2026/07/06 10:42:53 [notice] 1#1: start worker process 30
2026/07/06 10:42:53 [notice] 1#1: start worker process 31
2026/07/06 10:42:53 [notice] 1#1: start worker process 32
sufiyan@Khan:~$ 
```

### View real-time logs (follow mode)

The docker logs -f command displays the container's logs in real time. The -f (follow) option continuously streams new log entries as they are generated, making it useful for monitoring the container and troubleshooting issues. Press Ctrl + C to stop following the logs without stopping the container.

```bash

sufiyan@Khan:~$ docker logs -f nginx_container
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/07/06 10:42:53 [notice] 1#1: using the "epoll" event method
2026/07/06 10:42:53 [notice] 1#1: nginx/1.31.2
2026/07/06 10:42:53 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/07/06 10:42:53 [notice] 1#1: OS: Linux 6.17.0-35-generic
2026/07/06 10:42:53 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 65536:65536
2026/07/06 10:42:53 [notice] 1#1: start worker processes
2026/07/06 10:42:53 [notice] 1#1: start worker process 29
2026/07/06 10:42:53 [notice] 1#1: start worker process 30
2026/07/06 10:42:53 [notice] 1#1: start worker process 31
2026/07/06 10:42:53 [notice] 1#1: start worker process 32
```

### Exec into the container and look around the filesystem

```bash

sufiyan@Khan:~$ docker ps 
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                 NAMES
12b9885be473   nginx:latest           "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   0.0.0.0:80->80/tcp, [::]:80->80/tcp   nginx_container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago     Up 3 hours      127.0.0.1:44885->6443/tcp             kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago     Up 3 hours                                            kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago     Up 3 hours      127.0.0.1:36809->6443/tcp             k8s.demo-control-plane
sufiyan@Khan:~$ docker exec -it 12b9885be473 bash
root@12b9885be473:/# ls
bin  boot  dev	docker-entrypoint.d  docker-entrypoint.sh  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@12b9885be473:/# cd home
root@12b9885be473:/home# ls
root@12b9885be473:/home# cd ..
root@12b9885be473:/# ls
bin  boot  dev	docker-entrypoint.d  docker-entrypoint.sh  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@12b9885be473:/# cd usr
root@12b9885be473:/usr# ls
bin  games  include  lib  lib64  libexec  local  sbin  share  src
```

### Run a single command inside the container without entering it 

```bash
sufiyan@Khan:~$ docker exec  12b9885be473 ls /
bin
boot
dev
docker-entrypoint.d
docker-entrypoint.sh
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
sufiyan@Khan:~$ 
```

### Inspect the container — find its IP address, port mappings, and mounts

The docker inspect command displays detailed information about the container in JSON format. It can be used to find the container's IP address, port mappings, mount points (volumes), network settings, environment variables, and many other configuration details. This command is useful for troubleshooting and understanding how the container is configured.


```bash

sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                 NAMES
12b9885be473   nginx:latest           "/docker-entrypoint.…"   24 minutes ago   Up 24 minutes   0.0.0.0:80->80/tcp, [::]:80->80/tcp   nginx_container
9fdf059332c7   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago     Up 3 hours      127.0.0.1:44885->6443/tcp             kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago     Up 3 hours                                            kind-worker
140cfb082895   kindest/node:v1.30.0   "/usr/local/bin/entr…"   4 months ago     Up 3 hours      127.0.0.1:36809->6443/tcp             k8s.demo-control-plane
sufiyan@Khan:~$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nginx_container
172.17.0.2
sufiyan@Khan:~$ docker port nginx_container
80/tcp -> 0.0.0.0:80
80/tcp -> [::]:80
sufiyan@Khan:~$ docker inspect -f '{{json .Mounts}}' nginx_container
[]
sufiyan@Khan:~$ 

```

### Task 5: Cleanup

### Stop all running containers in one command

The docker stop $(docker ps -q) command stops all currently running containers. The docker ps -q command returns the IDs of all running containers, and docker stop uses those IDs to stop each container gracefully. You can verify that all containers have stopped by running:

```bash

sufiyan@Khan:~$ docker stop $(docker ps -q)
12b9885be473
9fdf059332c7
bb9ab8038d5f
140cfb082895
sufiyan@Khan:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
sufiyan@Khan:~$ docker ps -a
CONTAINER ID   IMAGE                                 COMMAND                  CREATED          STATUS                        PORTS     NAMES
12b9885be473   nginx:latest                          "/docker-entrypoint.…"   30 minutes ago   Exited (0) 21 seconds ago               nginx_container
e51eb16439fd   gcr.io/k8s-minikube/kicbase:v0.0.49   "/usr/local/bin/entr…"   4 months ago     Exited (137) 4 months ago               minikube
9fdf059332c7   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Exited (137) 11 seconds ago             kind-control-plane
bb9ab8038d5f   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Exited (130) 21 seconds ago             kind-worker
140cfb082895   kindest/node:v1.30.0                  "/usr/local/bin/entr…"   4 months ago     Exited (137) 11 seconds ago             k8s.demo-control-plane
sufiyan@Khan:~$ 
```

### Remove all stopped containers in one command

The docker container prune command removes all stopped containers from the system. It does not remove running containers. Using the -f flag skips the confirmation prompt. You can verify that the stopped containers have been removed by running docker ps -a.


```bash

sufiyan@Khan:~$ docker container prune -f
Deleted Containers:
12b9885be4730f4c0812cb4dda10101979e3f741b983efe899ad5e820a009cb6
e51eb16439fd59b72e5d5f1dd7d685fca43ba7f617bfbc53817d3250397c31d4
9fdf059332c7b5efe3f8c1c0415e881c6f5cb9767cf68c81667c7fce03a1152f
bb9ab8038d5fe80a8b5644116d0f15499f35c87694bc6e835966042e574b6d13
140cfb082895c9f26aed92428d6132f8884e9e66861b148630eaedcbb3fe5bfa

Total reclaimed space: 402.3MB
```

### Remove unused images

The docker image prune command removes all unused dangling images (images that are not tagged and are not referenced by any container). It helps free up disk space without affecting images that are currently in use. The -f flag skips the confirmation prompt. You can verify the remaining images by running docker images.

```bash

sufiyan@Khan:~$ docker image prune -f
Deleted Images:
untagged: kindest/node@sha256:047357ac0cfea04663786a612ba1eaba9702bef25227a794b52890dd8bcd692e
deleted: sha256:9319cf209ac58c5f091c9cb183fdd8784e753cfb5b1b3cb6692b26abd8d4efac
deleted: sha256:3d6d117551c9bfd7d3cdf6a6d17b15c8925c5bd389c60fa2e3c484f2b94c82cd
deleted: sha256:9ebea5aa64b29e11213cbde5050502f61c2384ead1a33519cfabc8a6f4063d20

Total reclaimed space: 974.2MB

```
### Check how much disk space Docker is using

The docker system df command displays the amount of disk space used by Docker objects, including images, containers, local volumes, and the build cache. It also shows how much space can be reclaimed by removing unused Docker resources. This command is useful for monitoring Docker's storage usage and identifying opportunities to free up disk space.

```bsh
sufiyan@Khan:~$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          6         0         2.939GB   2.939GB (100%)
Containers      0         0         0B        0B
Local Volumes   7         0         8.168GB   8.168GB (100%)
Build Cache     0         0         0B        0B
sufiyan@Khan:~$ 
```
