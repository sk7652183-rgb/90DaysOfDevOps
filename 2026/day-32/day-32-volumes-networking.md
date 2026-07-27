# Day 32 – Docker Volumes & Networking


## Task 1: The Problem

### Run a Postgres or MySQL container

```bash
ubuntu@ip-172-31-7-134:~$ docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=root mysql:latest
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
71fecf9a0313: Pull complete
30627cea5424: Pull complete
234be8523cd7: Pull complete
35c80c3f0cad: Pull complete
718475825f6a: Pull complete
7a08e28acd68: Pull complete
0fab344c2070: Pull complete
1fc0feac79ef: Pull complete
3bfe3c84c770: Pull complete
7ee3c7ac3aeb: Pull complete
58ddc8cbf912: Download complete
ebf48244f5b5: Download complete
Digest: sha256:cbade841779f1661300e705721f9d2ff159865cc7a8a291affbff43ac6ec7f1d
Status: Downloaded newer image for mysql:latest
6ae5c4e844f32d4c91acb02424aa1d0e32cb0479a19ce59e7f38703dcffa2ae8
ubuntu@ip-172-31-7-134:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                 NAMES
6ae5c4e844f3   mysql:latest   "docker-entrypoint.s…"   41 seconds ago   Up 40 seconds   3306/tcp, 33060/tcp   mysql-container
```
### Create some data inside it (a table, a few rows — anything)

```bash

ubuntu@ip-172-31-7-134:~$ docker exec -it 6ae5c4e844f3 bash
bash-5.1# ls
afs  bin  boot  dev  docker-entrypoint-initdb.d  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
bash-5.1# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 9.7.1 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.043 sec)

mysql> CREATE DATABASE company;
Query OK, 1 row affected (0.021 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| company            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.001 sec)

mysql> use company
Database changed
mysql> SHOW TABLES;
Empty set (0.001 sec)

mysql> CREATE TABLE employees (
    ->     emp_id INT PRIMARY KEY AUTO_INCREMENT,
    ->     emp_name VARCHAR(100) NOT NULL,
    ->     department VARCHAR(50),
    ->     salary DECIMAL(10,2)
    -> );
Query OK, 0 rows affected (0.038 sec)

mysql> SHOW TABLES;
+-------------------+
| Tables_in_company |
+-------------------+
| employees         |
+-------------------+
1 row in set (0.001 sec)

mysql> INSERT INTO employees (emp_name, department, salary)
    -> VALUES
    -> ('Alice', 'HR', 50000.00),
    -> ('Bob', 'IT', 65000.00),
    -> ('Charlie', 'Finance', 70000.00);
Query OK, 3 rows affected (0.017 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM employees;
+--------+----------+------------+----------+
| emp_id | emp_name | department | salary   |
+--------+----------+------------+----------+
|      1 | Alice    | HR         | 50000.00 |
|      2 | Bob      | IT         | 65000.00 |
|      3 | Charlie  | Finance    | 70000.00 |
+--------+----------+------------+----------+
3 rows in set (0.001 sec)

mysql>
```
### Stop and remove the container

```bash
ubuntu@ip-172-31-7-134:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                 NAMES
6ae5c4e844f3   mysql:latest   "docker-entrypoint.s…"   21 minutes ago   Up 21 minutes   3306/tcp, 33060/tcp   mysql-container
ubuntu@ip-172-31-7-134:~$ docker stop 6ae5c4e844f3 && docker rm 6ae5c4e844f3
6ae5c4e844f3
6ae5c4e844f3
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$
```

### Run a new one — is your data still there?

```bash
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$ docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=root mysql:latest
f489106e778e3397ed182e0298d8ec6de00c6d234c8efdde22063629551a9a7f
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                 NAMES
f489106e778e   mysql:latest   "docker-entrypoint.s…"   4 seconds ago   Up 3 seconds   3306/tcp, 33060/tcp   mysql-container
ubuntu@ip-172-31-7-134:~$ docker exec -it f489106e778e bash
bash-5.1# mysql-uroot -p
bash: mysql-uroot: command not found
bash-5.1# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 9.7.1 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.011 sec)

mysql>
```
**** Q **  Write what happened and why.
I stopped and removed the MySQL container, then created a new one. The company database and its data were missing.
The data was stored inside the old container. When the container was removed, its data was deleted. Since I did not use a Docker volume, the new container started with a fresh, empty database.

## Task 2: Named Volumes

### Create a named volume

```bash
ubuntu@ip-172-31-7-134:~$ docker volume create volume_1
volume_1
ubuntu@ip-172-31-7-134:~$ docker volume ls
DRIVER    VOLUME NAME
local     97e0bcce5494d517943397234140c0c4a2a4da8dd1120cd8f070a079418e51e3
local     d9132615dd69334fd544cc11d4c878ac73670f26294ce62e66cb50c4776a98bd
local     f5c1c4643fb9f48368ea27864e093f70d642b83ddb6919d01bd8c4ad3a857040
local     f1071ceae4d7ee45c6b77407f330f846340c08c8a83de4d94e63c6341fdff3cd
local     volume_1
ubuntu@ip-172-31-7-134:~$
```

### Ran the same database container, but this time attached a volume to it, added some data, then stopped and removed the container.
```
ubuntu@ip-172-31-7-134:~$ docker run -d \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=root \
  -v volume_1:/var/lib/mysql \
  mysql:latest
3af74cc7be595712a44787859dfade20d7c69f7e7d770f542d35ddc9a61014ec
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                 NAMES
3af74cc7be59   mysql:latest   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   3306/tcp, 33060/tcp   mysql
ubuntu@ip-172-31-7-134:~$ docker exec -it 3af74cc7be59 bash
bash-5.1# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 9.7.1 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.005 sec)

mysql> CREATE DATABASE company;
Query OK, 1 row affected (0.008 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| company            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.001 sec)

mysql> CREATE TABLE employee (
    ->     emp_id INT PRIMARY KEY,
    ->     first_name VARCHAR(50),
    ->     last_name VARCHAR(50),
    ->     age INT,
    ->     gender VARCHAR(10),
    ->     department VARCHAR(50),
    ->     salary DECIMAL(10,2),
    ->     hire_date DATE
    -> );
ERROR 1046 (3D000): No database selected
mysql> use company
Database changed
mysql> CREATE TABLE employee (
    ->     emp_id INT PRIMARY KEY,
    ->     first_name VARCHAR(50),
    ->     last_name VARCHAR(50),
    ->     age INT,
    ->     gender VARCHAR(10),
    ->     department VARCHAR(50),
    ->     salary DECIMAL(10,2),
    ->     hire_date DATE
    -> );
Query OK, 0 rows affected (0.030 sec)

mysql> INSERT INTO employee
    -> (emp_id, first_name, last_name, age, gender, department, salary, hire_date)
    -> VALUES
    -> (101, 'John', 'Doe', 30, 'Male', 'IT', 60000.00, '2024-01-15'),
    -> (102, 'Jane', 'Smith', 28, 'Female', 'HR', 55000.00, '2023-08-20'),
    -> (103, 'David', 'Wilson', 35, 'Male', 'Finance', 70000.00, '2022-05-10');
Query OK, 3 rows affected (0.012 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM employee;
+--------+------------+-----------+------+--------+------------+----------+------------+
| emp_id | first_name | last_name | age  | gender | department | salary   | hire_date  |
+--------+------------+-----------+------+--------+------------+----------+------------+
|    101 | John       | Doe       |   30 | Male   | IT         | 60000.00 | 2024-01-15 |
|    102 | Jane       | Smith     |   28 | Female | HR         | 55000.00 | 2023-08-20 |
|    103 | David      | Wilson    |   35 | Male   | Finance    | 70000.00 | 2022-05-10 |
+--------+------------+-----------+------+--------+------------+----------+------------+
3 rows in set (0.003 sec)

mysql> exit
Bye
bash-5.1# exit
exit
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                 NAMES
3af74cc7be59   mysql:latest   "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   3306/tcp, 33060/tcp   mysql
ubuntu@ip-172-31-7-134:~$ docker stop 3af74cc7be59 && docker rm 3af74cc7be59
3af74cc7be59
3af74cc7be59
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ip-172-31-7-134:~$

```

### Run a brand new container with the same volume
```bash
ubuntu@ip-172-31-7-134:~$ docker run -d \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=root \
  -v volume_1:/var/lib/mysql \
  mysql:latest
a77c3b1a3e0ae172248243a3a9de2dfa7147e5df2a02bbcf2322eaac2fe70462
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                 NAMES
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   4 seconds ago   Up 3 seconds   3306/tcp, 33060/tcp   mysql
ubuntu@ip-172-31-7-134:~$ docker exec -it a77c3b1a3e0a bash
bash-5.1# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 9.7.1 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| company            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.009 sec)

mysql> use company
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SELECT * FROM employee;
+--------+------------+-----------+------+--------+------------+----------+------------+
| emp_id | first_name | last_name | age  | gender | department | salary   | hire_date  |
+--------+------------+-----------+------+--------+------------+----------+------------+
|    101 | John       | Doe       |   30 | Male   | IT         | 60000.00 | 2024-01-15 |
|    102 | Jane       | Smith     |   28 | Female | HR         | 55000.00 | 2023-08-20 |
|    103 | David      | Wilson    |   35 | Male   | Finance    | 70000.00 | 2022-05-10 |
+--------+------------+-----------+------+--------+------------+----------+------------+
3 rows in set (0.003 sec)
```

### Is the data still there?
Yes, the data is still available after attaching the volume because the volume persists independently of the container. The data can be accessed by the container and is stored on the host system

## Task 3: Bind Mounts

Create a folder on your host machine with an index.html file, then run an Nginx container and bind mount the folder to the Nginx web directory.

```bash
ubuntu@ip-172-31-7-134:~$ ls
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$ mkdir nginx-pratice
ubuntu@ip-172-31-7-134:~$ ls
nginx-pratice
ubuntu@ip-172-31-7-134:~$ cd nginx-pratice
ubuntu@ip-172-31-7-134:~/nginx-pratice$ ls
ubuntu@ip-172-31-7-134:~/nginx-pratice$ vim index.html
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker run -d \
  --name webserver \
  -p 8080:80 \
  -v $(pwd)/index.html:/usr/share/nginx/html/index.html \
  nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
d26f27cc8c41: Pull complete
062e450697fa: Pull complete
82454cdbf456: Pull complete
3c7ab7949321: Pull complete
cacfcdd01f30: Pull complete
b6698f04e005: Pull complete
2bedaf25031a: Pull complete
6c496f5b5050: Download complete
ea1d76ccc2c6: Download complete
Digest: sha256:5a88c9c45479443d7be2eadc894b4ed0a9801bae03d97a5760ae13b5c2005942
Status: Downloaded newer image for nginx:latest
acbd24b44497ef1e50d00cae75a692087bdb4e9582cf2f9bef6e7c86e39412d2
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
acbd24b44497   nginx          "/docker-entrypoint.…"   6 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   3 hours ago     Up 3 hours     3306/tcp, 33060/tcp                       mysql

```

### Access the page in your browser

<img width="1365" height="679" alt="image" src="https://github.com/user-attachments/assets/155290a4-c61a-4fb1-81c6-0ac50c925e01" />


### Edit the index.html on your host — refresh the browser
```bash
ubuntu@ip-172-31-7-134:~/nginx-pratice$ ls
index.html
ubuntu@ip-172-31-7-134:~/nginx-pratice$ vim index.html
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
acbd24b44497   nginx          "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   3 hours ago     Up 3 hours     3306/tcp, 33060/tcp                       mysql
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker stop acbd24b44497 && docker rm acbd24b44497
acbd24b44497
acbd24b44497
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker run -d \
  --name webserver \
  -p 8080:80 \
  -v $(pwd)/index.html:/usr/share/nginx/html/index.html \
  nginx
82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
82333f43155d   nginx          "/docker-entrypoint.…"   7 seconds ago   Up 7 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   3 hours ago     Up 3 hours     3306/tcp, 33060/tcp                       mysql
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$ vim index.html
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
82333f43155d   nginx          "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   3 hours ago     Up 3 hours     3306/tcp, 33060/tcp                       mysql
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker inspect 82333f43155d
[
    {
        "Id": "82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883",
        "Created": "2026-07-27T09:30:04.572028326Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 34604,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2026-07-27T09:30:04.71273336Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5a88c9c45479443d7be2eadc894b4ed0a9801bae03d97a5760ae13b5c2005942",
        "ResolvConfPath": "/var/lib/docker/containers/82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883/hostname",
        "HostsPath": "/var/lib/docker/containers/82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883/hosts",
        "LogPath": "/var/lib/docker/containers/82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883/82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883-json.log",
        "Name": "/webserver",
        "RestartCount": 0,
        "Driver": "overlayfs",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "/home/ubuntu/nginx-pratice/index.html:/usr/share/nginx/html/index.html"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "8080"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                38,
                146
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": null,
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/acpi",
                "/proc/asound",
                "/proc/interrupts",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/sys/devices/virtual/powercap",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "Storage": {
            "RootFS": {
                "Snapshot": {
                    "Name": "overlayfs"
                }
            }
        },
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/home/ubuntu/nginx-pratice/index.html",
                "Destination": "/usr/share/nginx/html/index.html",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
        "Config": {
            "Hostname": "82333f43155d",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.31.3",
                "NJS_VERSION=1.0.0",
                "NJS_RELEASE=1~trixie",
                "ACME_VERSION=0.4.1",
                "PKG_RELEASE=1~trixie",
                "DYNPKG_RELEASE=1~trixie"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "SandboxID": "0b4dcb61bd10f773557a9ca1e1ecdf0969806e6270f0f7a2a89a896d3c062d84",
            "SandboxKey": "/var/run/docker/netns/0b4dcb61bd10",
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "8080"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "8080"
                    }
                ]
            },
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "DriverOpts": null,
                    "GwPriority": 0,
                    "NetworkID": "a48dae2dab318e1853e0e4216910ab53d8e84adce22fe7ad54d02c657ae8f58c",
                    "EndpointID": "c7bbd3f6660da35011b07c48989c092bf379e823800ca6ee70c505b9b87acf90",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "MacAddress": "4a:c0:bc:15:96:ae",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        },
        "ImageManifestDescriptor": {
            "mediaType": "application/vnd.oci.image.manifest.v1+json",
            "digest": "sha256:db4f612f385437d11eb26620a4f1d7efb3ff44e1296a3c21540b30454e6e2bf3",
            "size": 2290,
            "annotations": {
                "com.docker.official-images.bashbrew.arch": "amd64",
                "org.opencontainers.image.base.digest": "sha256:9bb8a3626890e084ab54e888fdd7c4b6d2f119071cd4c5dc5fecb4d73062aa5f",
                "org.opencontainers.image.base.name": "debian:trixie-slim",
                "org.opencontainers.image.created": "2026-07-15T23:30:13Z",
                "org.opencontainers.image.revision": "ccdab6c99ae2e2fc53a144dc68d6b8f44163adf2",
                "org.opencontainers.image.source": "https://github.com/nginx/docker-nginx.git#ccdab6c99ae2e2fc53a144dc68d6b8f44163adf2:mainline/debian",
                "org.opencontainers.image.url": "https://hub.docker.com/_/nginx",
                "org.opencontainers.image.version": "1.31.3"
            },
            "platform": {
                "architecture": "amd64",
                "os": "linux"
            }
        }
    }
]
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
82333f43155d   nginx          "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   3 hours ago     Up 3 hours     3306/tcp, 33060/tcp                       mysql
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker exec -it 82333f43155d bash
root@82333f43155d:/# cat /usr/share/nginx/html/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Practice HTML Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
            text-align: center;
            margin: 50px;
        }

        .container {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            width: 50%;
            margin: auto;
            box-shadow: 0px 0px 10px gray;
        }

        h1 {
            color: #2c3e50;
        }

        p {
            color: #555;
        }

        button {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 5px;
        }

        button:hover {
            background-color: #2980b9;
        }
    </style>
</head>

<body>

    <div class="container">
        <h1>Welcome to My Practice Page</h1>

        <p>
            This is a sample HTML page created for practice.
        </p>

        <h2>My Details</h2>

        <ul>
            <li>Khan Abusufiyan Khan</li>
            <li>DevOps</li>
            <li>Learning: Docker</li>
        </ul>

        <button onclick="showMessage()">
            Click Me
        </button>
    </div>

    <script>
        function showMessage() {
            alert("Hello! You clicked the button.");
        }
    </script>

</body>
</html>

root@82333f43155d:/# exit
exit
```
<img width="1359" height="718" alt="image" src="https://github.com/user-attachments/assets/2cd7bba6-f183-48ef-bb5d-ef7647958007" />


### Write in your notes: What is the difference between a named volume and a bind mount?

A named volume is managed by Docker and stored in Docker's storage area on the host. The data persists even if the container is removed, and it is mainly used for storing application data.

A bind mount directly maps a file or folder from the host machine to a location inside the container. Any changes made to the file or folder on the host are automatically reflected inside the container because both locations are linked.

## Task 4: Docker Networking Basics

### List all Docker networks on your machine

```bash
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
82333f43155d   nginx          "/docker-entrypoint.…"   32 minutes ago   Up 32 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   4 hours ago      Up 4 hours      3306/tcp, 33060/tcp                       mysql
ubuntu@ip-172-31-7-134:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a48dae2dab31   bridge    bridge    local
9c834ab2ae37   host      host      local
34eff3df0fc8   none      null      local
ubuntu@ip-172-31-7-134:~$
```
### Inspect the default bridge network
ubuntu@ip-172-31-7-134:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a48dae2dab31   bridge    bridge    local
9c834ab2ae37   host      host      local
34eff3df0fc8   none      null      local
ubuntu@ip-172-31-7-134:~$ docker inspect a48dae2dab31
[
    {
        "Name": "bridge",
        "Id": "a48dae2dab318e1853e0e4216910ab53d8e84adce22fe7ad54d02c657ae8f58c",
        "Created": "2026-07-25T09:21:28.662297259Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "IPRange": "",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {},
        "Containers": {
            "82333f43155d7618a7aaca8f4592e3470a29a142afd565e376358d1391999883": {
                "Name": "webserver",
                "EndpointID": "c7bbd3f6660da35011b07c48989c092bf379e823800ca6ee70c505b9b87acf90",
                "MacAddress": "4a:c0:bc:15:96:ae",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            },
            "a77c3b1a3e0ae172248243a3a9de2dfa7147e5df2a02bbcf2322eaac2fe70462": {
                "Name": "mysql",
                "EndpointID": "134a1be33a104a6c46215c1af17d7faca20dcb257b55942f2bbc24e162d8ff70",
                "MacAddress": "2a:09:61:29:dd:bc",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Status": {
            "IPAM": {
                "Subnets": {
                    "172.17.0.0/16": {
                        "IPsInUse": 5,
                        "DynamicIPsAvailable": 65531
                    }
                }
            }
        }
    }
]
ubuntu@ip-172-31-7-134:~$

### Run two containers on the default bridge — can they ping each other by name?

```bash
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                     NAMES
82333f43155d   nginx          "/docker-entrypoint.…"   2 hours ago   Up 2 hours   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
a77c3b1a3e0a   mysql:latest   "docker-entrypoint.s…"   5 hours ago   Up 5 hours   3306/tcp, 33060/tcp                       mysql
ubuntu@ip-172-31-7-134:~$ docker exec -it 82333f43155d bash
root@82333f43155d:/# ping mysql
bash: ping: command not found
root@82333f43155d:/# getent hosts mysql
root@82333f43155d:/# apt update
apt install -y iputils-ping
ping mysql
Get:1 http://deb.debian.org/debian trixie InRelease [140 kB]
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 Packages [9673 kB]
Get:5 http://deb.debian.org/debian trixie-updates/main amd64 Packages [4412 B]
Get:6 http://deb.debian.org/debian-security trixie-security/main amd64 Packages [227 kB]
Fetched 10.1 MB in 1s (7517 kB/s)
1 package can be upgraded. Run 'apt list --upgradable' to see it.
Installing:
  iputils-ping

Installing dependencies:
  linux-sysctl-defaults

Summary:
  Upgrading: 0, Installing: 2, Removing: 0, Not Upgrading: 1
  Download size: 57.0 kB
  Space needed: 211 kB / 24.2 GB available

Get:1 http://deb.debian.org/debian trixie/main amd64 iputils-ping amd64 3:20240905-3 [51.2 kB]
Get:2 http://deb.debian.org/debian trixie/main amd64 linux-sysctl-defaults all 4.12.1 [5724 B]
Fetched 57.0 kB in 0s (1347 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 2.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/x86_64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 2.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package iputils-ping.
(Reading database ... 6704 files and directories currently installed.)
Preparing to unpack .../iputils-ping_3%3a20240905-3_amd64.deb ...
Unpacking iputils-ping (3:20240905-3) ...
Selecting previously unselected package linux-sysctl-defaults.
Preparing to unpack .../linux-sysctl-defaults_4.12.1_all.deb ...
Unpacking linux-sysctl-defaults (4.12.1) ...
Setting up linux-sysctl-defaults (4.12.1) ...
Setting up iputils-ping (3:20240905-3) ...
ping: mysql: Name or service not known
root@82333f43155d:/# ping mysql
ping: mysql: Name or service not known
```
Containers cannot resolve each other by container name (for example, ping mysql fails).

### Run two containers on the default bridge — can they ping each other by IP?
```bash
ubuntu@ip-172-31-7-134:~$ docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mysql
172.17.0.2
ubuntu@ip-172-31-7-134:~$ docker exec -it webserver bash
root@82333f43155d:/# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.059 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.042 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.034 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.041 ms
64 bytes from 172.17.0.2: icmp_seq=5 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=6 ttl=64 time=0.042 ms
64 bytes from 172.17.0.2: icmp_seq=7 ttl=64 time=0.038 ms
64 bytes from 172.17.0.2: icmp_seq=8 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=9 ttl=64 time=0.041 ms
64 bytes from 172.17.0.2: icmp_seq=10 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=11 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=12 ttl=64 time=0.044 ms
64 bytes from 172.17.0.2: icmp_seq=13 ttl=64 time=0.037 ms
64 bytes from 172.17.0.2: icmp_seq=14 ttl=64 time=0.044 ms
64 bytes from 172.17.0.2: icmp_seq=15 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=16 ttl=64 time=0.041 ms
64 bytes from 172.17.0.2: icmp_seq=17 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=18 ttl=64 time=0.042 ms
64 bytes from 172.17.0.2: icmp_seq=19 ttl=64 time=0.044 ms
64 bytes from 172.17.0.2: icmp_seq=20 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=21 ttl=64 time=0.042 ms
64 bytes from 172.17.0.2: icmp_seq=22 ttl=64 time=0.043 ms
64 bytes from 172.17.0.2: icmp_seq=23 ttl=64 time=0.046 ms
64 bytes from 172.17.0.2: icmp_seq=24 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=25 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=26 ttl=64 time=0.039 ms
64 bytes from 172.17.0.2: icmp_seq=27 ttl=64 time=0.040 ms
64 bytes from 172.17.0.2: icmp_seq=28 ttl=64 time=0.042 ms
64 bytes from 172.17.0.2: icmp_seq=29 ttl=64 time=0.042 ms
64 bytes from 172.17.0.2: icmp_seq=30 ttl=64 time=0.041 ms
64 bytes from 172.17.0.2: icmp_seq=31 ttl=64 time=0.043 ms
64 bytes from 172.17.0.2: icmp_seq=32 ttl=64 time=0.039 ms
^C
--- 172.17.0.2 ping statistics ---
32 packets transmitted, 32 received, 0% packet loss, time 31766ms
rtt min/avg/max/mdev = 0.034/0.041/0.059/0.003 ms
```
## Task 5: Custom Networks

### Create a custom bridge network called my-app-net

```bash
ubuntu@ip-172-31-7-134:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a48dae2dab31   bridge    bridge    local
9c834ab2ae37   host      host      local
34eff3df0fc8   none      null      local
ubuntu@ip-172-31-7-134:~$ docker network create my-app-net
1671bfc9091b941b0ef3bc424e9071f87ed69df5191a959b0c02731b4f52697b
ubuntu@ip-172-31-7-134:~$ docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
a48dae2dab31   bridge       bridge    local
9c834ab2ae37   host         host      local
1671bfc9091b   my-app-net   bridge    local
34eff3df0fc8   none         null      local
ubuntu@ip-172-31-7-134:~$
```

### Run two containers on my-app-net
```bash

ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker run -d \
  --name webserver \
  --network my-app-net \
  -p 8080:80 \
  -v $(pwd)/index.html:/usr/share/nginx/html/index.html \
  nginx
7318624a8003547f35b6ec16045ec1e5269ec470c2033e45b31847e3eb6e5965
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
7318624a8003   nginx     "/docker-entrypoint.…"   5 seconds ago   Up 5 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   webserver
ubuntu@ip-172-31-7-134:~/nginx-pratice$
ubuntu@ip-172-31-7-134:~/nginx-pratice$ docker inspect -f '{{range $k, $v := .NetworkSettings.Networks}}{{$k}}{{end}}' webserver
my-app-net
ubuntu@ip-172-31-7-134:~/nginx-pratice$ cd ..
ubuntu@ip-172-31-7-134:~$ ls
index.html  nginx-pratice
ubuntu@ip-172-31-7-134:~$ git clone https://github.com/sk7652183-rgb/hotelhub.git
Cloning into 'hotelhub'...
remote: Enumerating objects: 116, done.
remote: Counting objects: 100% (116/116), done.
remote: Compressing objects: 100% (92/92), done.
remote: Total 116 (delta 30), reused 100 (delta 20), pack-reused 0 (from 0)
Receiving objects: 100% (116/116), 724.83 KiB | 5.89 MiB/s, done.
Resolving deltas: 100% (30/30), done.
ubuntu@ip-172-31-7-134:~$ ls
hotelhub  index.html  nginx-pratice
ubuntu@ip-172-31-7-134:~$ cd hotelhub
ubuntu@ip-172-31-7-134:~/hotelhub$ ls
Dockerfile  LICENSE  accounts  guest  hotelmanagement  main  manage.py  readme.md  requirements.txt  room  screenshots
ubuntu@ip-172-31-7-134:~/hotelhub$ vim Dockerfile
ubuntu@ip-172-31-7-134:~/hotelhub$ cat Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000


CMD ["sh", "-c", "python manage.py makemigrations && python manage.py migrate && python manage.py runserver 0.0.0.0:8000"]





ubuntu@ip-172-31-7-134:~/hotelhub$ docker build -t hotelhub .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  1.901MB
Step 1/7 : FROM python:3.11-slim
3.11-slim: Pulling from library/python
c89b9f64c028: Pulling fs layer
9775d166087a: Pulling fs layer
6b265b8eae4a: Pulling fs layer
c89b9f64c028: Download complete
9775d166087a: Download complete
6b265b8eae4a: Download complete
9775d166087a: Pull complete
14033bd23a0b: Download complete
0e8f5815ddbb: Download complete
6b265b8eae4a: Pull complete
c89b9f64c028: Pull complete
Digest: sha256:db3ff2e1800a8581e2c48a27c3995339d47bdf046da21c7627accd3d51053a93
Status: Downloaded newer image for python:3.11-slim
 ---> db3ff2e1800a
Step 2/7 : WORKDIR /app
 ---> Running in 6fce21c21949
 ---> Removed intermediate container 6fce21c21949
 ---> db097b7bb054
Step 3/7 : COPY requirements.txt .
 ---> 0ee52903ca14
Step 4/7 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Running in 358416f781df
Collecting django (from -r requirements.txt (line 1))
  Downloading django-5.2.16-py3-none-any.whl.metadata (4.1 kB)
Collecting asgiref>=3.8.1 (from django->-r requirements.txt (line 1))
  Downloading asgiref-3.12.1-py3-none-any.whl.metadata (9.4 kB)
Collecting sqlparse>=0.3.1 (from django->-r requirements.txt (line 1))
  Downloading sqlparse-0.5.5-py3-none-any.whl.metadata (4.7 kB)
Downloading django-5.2.16-py3-none-any.whl (8.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.3/8.3 MB 71.4 MB/s eta 0:00:00
Downloading asgiref-3.12.1-py3-none-any.whl (25 kB)
Downloading sqlparse-0.5.5-py3-none-any.whl (46 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 46.1/46.1 kB 304.6 MB/s eta 0:00:00
Installing collected packages: sqlparse, asgiref, django
Successfully installed asgiref-3.12.1 django-5.2.16 sqlparse-0.5.5
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv

[notice] A new release of pip is available: 24.0 -> 26.1.2
[notice] To update, run: pip install --upgrade pip
 ---> Removed intermediate container 358416f781df
 ---> 3451e6e2d010
Step 5/7 : COPY . .
 ---> 0918a8459b7e
Step 6/7 : EXPOSE 8000
 ---> Running in 8a9ce5f50a69
 ---> Removed intermediate container 8a9ce5f50a69
 ---> ca520322238f
Step 7/7 : CMD ["sh", "-c", "python manage.py makemigrations && python manage.py migrate && python manage.py runserver 0.0.0.0:8000"]
 ---> Running in 827429e55034
 ---> Removed intermediate container 827429e55034
 ---> fed1386f6d5f
Successfully built fed1386f6d5f
Successfully tagged hotelhub:latest
ubuntu@ip-172-31-7-134:~/hotelhub$ docker run -d \
  --name hotelhub \
  --network my-app-net \
  -p 8000:8000 \
  hotelhub
374725eb9e88ceaf585bbbb298fa054116f773e70d866b102711e67379e9dcdc
ubuntu@ip-172-31-7-134:~/hotelhub$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                                         NAMES
374725eb9e88   hotelhub   "sh -c 'python manag…"   7 seconds ago   Up 6 seconds   0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   hotelhub
7318624a8003   nginx      "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       webserver
ubuntu@ip-172-31-7-134:~/hotelhub$ docker inspect hotelhub --format='{{json .NetworkSettings.Networks}}'
{"my-app-net":{"IPAMConfig":null,"Links":null,"Aliases":null,"DriverOpts":null,"GwPriority":0,"NetworkID":"1671bfc9091b941b0ef3bc424e9071f87ed69df5191a959b0c02731b4f52697b","EndpointID":"5d3d3f600abce03a0000ef5d4cfd018d90ef4261874031c93b1557525f95cf18","Gateway":"172.18.0.1","IPAddress":"172.18.0.3","MacAddress":"62:d6:de:41:e1:85","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"DNSNames":["hotelhub","374725eb9e88"]}}
```

### Can they ping each other by name now?
```bash
ubuntu@ip-172-31-7-134:~/hotelhub$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                                         NAMES
374725eb9e88   hotelhub   "sh -c 'python manag…"   2 minutes ago   Up 2 minutes   0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   hotelhub
7318624a8003   nginx      "/docker-entrypoint.…"   9 minutes ago   Up 9 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       webserver
ubuntu@ip-172-31-7-134:~/hotelhub$ docker exec -it 374725eb9e88 bash
root@374725eb9e88:/app# ping webserver
bash: ping: command not found
root@374725eb9e88:/app# apt update
apt install -y iputils-ping
ping mysql
Hit:1 http://deb.debian.org/debian trixie InRelease
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 Packages [9673 kB]
Get:5 http://deb.debian.org/debian trixie-updates/main amd64 Packages [4412 B]
Get:6 http://deb.debian.org/debian-security trixie-security/main amd64 Packages [227 kB]
Fetched 9995 kB in 1s (7980 kB/s)
All packages are up to date.
Installing:
  iputils-ping

Installing dependencies:
  libidn2-0  libunistring5  linux-sysctl-defaults

Summary:
  Upgrading: 0, Installing: 4, Removing: 0, Not Upgrading: 0
  Download size: 643 kB
  Space needed: 2810 kB / 24.1 GB available

Get:1 http://deb.debian.org/debian trixie/main amd64 libunistring5 amd64 1.3-2 [477 kB]
Get:2 http://deb.debian.org/debian trixie/main amd64 libidn2-0 amd64 2.3.8-2 [109 kB]
Get:3 http://deb.debian.org/debian trixie/main amd64 iputils-ping amd64 3:20240905-3 [51.2 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 linux-sysctl-defaults all 4.12.1 [5724 B]
Fetched 643 kB in 0s (9474 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 4.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/x86_64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 4.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package libunistring5:amd64.
(Reading database ... 5645 files and directories currently installed.)
Preparing to unpack .../libunistring5_1.3-2_amd64.deb ...
Unpacking libunistring5:amd64 (1.3-2) ...
Selecting previously unselected package libidn2-0:amd64.
Preparing to unpack .../libidn2-0_2.3.8-2_amd64.deb ...
Unpacking libidn2-0:amd64 (2.3.8-2) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20240905-3_amd64.deb ...
Unpacking iputils-ping (3:20240905-3) ...
Selecting previously unselected package linux-sysctl-defaults.
Preparing to unpack .../linux-sysctl-defaults_4.12.1_all.deb ...
Unpacking linux-sysctl-defaults (4.12.1) ...
Setting up linux-sysctl-defaults (4.12.1) ...
Setting up libunistring5:amd64 (1.3-2) ...
Setting up libidn2-0:amd64 (2.3.8-2) ...
Setting up iputils-ping (3:20240905-3) ...
Processing triggers for libc-bin (2.41-12+deb13u3) ...
ping: mysql: Temporary failure in name resolution
root@374725eb9e88:/app# ping webserver
PING webserver (172.18.0.2) 56(84) bytes of data.
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=1 ttl=64 time=0.048 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=2 ttl=64 time=0.040 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=3 ttl=64 time=0.040 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=4 ttl=64 time=0.048 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=5 ttl=64 time=0.049 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=6 ttl=64 time=0.028 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=7 ttl=64 time=0.046 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=8 ttl=64 time=0.056 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=9 ttl=64 time=0.052 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=10 ttl=64 time=0.046 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=11 ttl=64 time=0.046 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=12 ttl=64 time=0.048 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=13 ttl=64 time=0.045 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=14 ttl=64 time=0.046 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=15 ttl=64 time=0.046 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=16 ttl=64 time=0.049 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=17 ttl=64 time=0.050 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=18 ttl=64 time=0.045 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=19 ttl=64 time=0.048 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=20 ttl=64 time=0.045 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=21 ttl=64 time=0.044 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=22 ttl=64 time=0.046 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=23 ttl=64 time=0.049 ms
64 bytes from webserver.my-app-net (172.18.0.2): icmp_seq=24 ttl=64 time=0.044 ms
^C
--- webserver ping statistics ---
24 packets transmitted, 24 received, 0% packet loss, time 23536ms
rtt min/avg/max/mdev = 0.028/0.046/0.056/0.005 ms
```

```bash
ubuntu@ip-172-31-7-134:~/hotelhub$
ubuntu@ip-172-31-7-134:~/hotelhub$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                                         NAMES
374725eb9e88   hotelhub   "sh -c 'python manag…"   6 minutes ago    Up 6 minutes    0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   hotelhub
7318624a8003   nginx      "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       webserver
ubuntu@ip-172-31-7-134:~/hotelhub$ docker exec -it 7318624a8003 bash
root@7318624a8003:/# ping hotelhub
bash: ping: command not found
root@7318624a8003:/# apt update
apt install -y iputils-ping
Get:1 http://deb.debian.org/debian trixie InRelease [140 kB]
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 Packages [9673 kB]
Get:5 http://deb.debian.org/debian trixie-updates/main amd64 Packages [4412 B]
Get:6 http://deb.debian.org/debian-security trixie-security/main amd64 Packages [227 kB]
Fetched 10.1 MB in 1s (7834 kB/s)
1 package can be upgraded. Run 'apt list --upgradable' to see it.
Installing:
  iputils-ping

Installing dependencies:
  linux-sysctl-defaults

Summary:
  Upgrading: 0, Installing: 2, Removing: 0, Not Upgrading: 1
  Download size: 57.0 kB
  Space needed: 211 kB / 24.0 GB available

Get:1 http://deb.debian.org/debian trixie/main amd64 iputils-ping amd64 3:20240905-3 [51.2 kB]
Get:2 http://deb.debian.org/debian trixie/main amd64 linux-sysctl-defaults all 4.12.1 [5724 B]
Fetched 57.0 kB in 0s (1296 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 2.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/x86_64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 2.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package iputils-ping.
(Reading database ... 6704 files and directories currently installed.)
Preparing to unpack .../iputils-ping_3%3a20240905-3_amd64.deb ...
Unpacking iputils-ping (3:20240905-3) ...
Selecting previously unselected package linux-sysctl-defaults.
Preparing to unpack .../linux-sysctl-defaults_4.12.1_all.deb ...
Unpacking linux-sysctl-defaults (4.12.1) ...
Setting up linux-sysctl-defaults (4.12.1) ...
Setting up iputils-ping (3:20240905-3) ...
root@7318624a8003:/# ping hotelhub
PING hotelhub (172.18.0.3) 56(84) bytes of data.
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=1 ttl=64 time=0.039 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=2 ttl=64 time=0.042 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=3 ttl=64 time=0.045 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=4 ttl=64 time=0.047 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=5 ttl=64 time=0.053 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=6 ttl=64 time=0.047 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=7 ttl=64 time=0.045 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=8 ttl=64 time=0.047 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=9 ttl=64 time=0.047 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=10 ttl=64 time=0.048 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=11 ttl=64 time=0.050 ms
^C64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=12 ttl=64 time=0.040 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=13 ttl=64 time=0.048 ms
64 bytes from hotelhub.my-app-net (172.18.0.3): icmp_seq=14 ttl=64 time=0.050 ms
^C
--- hotelhub ping statistics ---
14 packets transmitted, 14 received, 0% packet loss, time 13287ms
rtt min/avg/max/mdev = 0.039/0.046/0.053/0.003 ms
root@7318624a8003:/#
```
### Write in your notes: Why does custom networking allow name-based communication but the default bridge doesn't?
A custom bridge network allows name-based communication because Docker provides an internal DNS service that automatically resolves container names to their IP addresses. Containers connected to the same custom bridge network can communicate using their container names. In contrast, the default bridge network does not provide automatic DNS-based name resolution for containers started separately, so they must communicate using IP addresses instead.

## Task 6: Put It Together

### Create a custom network and run a database container (MySQL/Postgres) on that network with a volume attached for persistent data storage.

```bash
ubuntu@ip-172-31-7-134:~$ docker run -d -it \
  --name Database_container \
  --network test_network \
  -e MYSQL_ROOT_PASSWORD=root \
  -v volume_2:/var/lib/mysql \
  mysql:latest
41b74f0731a79a471043c41a3eb575bfecf523e16172bf3e8b1d945bf8efb36e
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                 NAMES
41b74f0731a7   mysql:latest   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   3306/tcp, 33060/tcp   Database_container
ubuntu@ip-172-31-7-134:~$ docker network ls
NETWORK ID     NAME           DRIVER    SCOPE
a48dae2dab31   bridge         bridge    local
9c834ab2ae37   host           host      local
1671bfc9091b   my-app-net     bridge    local
34eff3df0fc8   none           null      local
594c7b4587bc   test_network   bridge    local
ubuntu@ip-172-31-7-134:~$ docker inspect -f '{{range $k, $v := .NetworkSettings.Networks}}{{$k}}{{end}}' Database_container
test_network
ubuntu@ip-172-31-7-134:~$
```
### Run an application container (using any image) on the same custom network and verify that it can communicate with the database container using its container name.
```bash 
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                 NAMES
7eed0b48bdda   mysql:latest   "docker-entrypoint.s…"   5 hours ago   Up 5 hours   3306/tcp, 33060/tcp   Database_container
ubuntu@ip-172-31-7-134:~$
ubuntu@ip-172-31-7-134:~$ docker network ls
NETWORK ID     NAME           DRIVER    SCOPE
a48dae2dab31   bridge         bridge    local
9c834ab2ae37   host           host      local
1671bfc9091b   my-app-net     bridge    local
34eff3df0fc8   none           null      local
594c7b4587bc   test_network   bridge    local
ubuntu@ip-172-31-7-134:~$ docker run -d \
  --name hotelhub \
  --network test_network \
  -p 8000:8000 \
  hotelhub
294f82cb2b55e68fe953e245bec31dd63f34f842a9faf3db5fb57817587aa136
ubuntu@ip-172-31-7-134:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                         NAMES
294f82cb2b55   hotelhub       "sh -c 'python manag…"   4 seconds ago   Up 4 seconds   0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   hotelhub
7eed0b48bdda   mysql:latest   "docker-entrypoint.s…"   5 hours ago     Up 5 hours     3306/tcp, 33060/tcp                           Database_container
ubuntu@ip-172-31-7-134:~$ docker exec -it 294f82cb2b55 bash
root@294f82cb2b55:/app# ping Database_container
bash: ping: command not found
root@294f82cb2b55:/app# apt update
apt install -y iputils-ping
Hit:1 http://deb.debian.org/debian trixie InRelease
Get:2 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Get:3 http://deb.debian.org/debian-security trixie-security InRelease [43.4 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 Packages [9673 kB]
Get:5 http://deb.debian.org/debian trixie-updates/main amd64 Packages [4412 B]
Get:6 http://deb.debian.org/debian-security trixie-security/main amd64 Packages [227 kB]
Fetched 9995 kB in 1s (7561 kB/s)
All packages are up to date.
Installing:
  iputils-ping

Installing dependencies:
  libidn2-0  libunistring5  linux-sysctl-defaults

Summary:
  Upgrading: 0, Installing: 4, Removing: 0, Not Upgrading: 0
  Download size: 643 kB
  Space needed: 2810 kB / 21.7 GB available

Get:1 http://deb.debian.org/debian trixie/main amd64 libunistring5 amd64 1.3-2 [477 kB]
Get:2 http://deb.debian.org/debian trixie/main amd64 libidn2-0 amd64 2.3.8-2 [109 kB]
Get:3 http://deb.debian.org/debian trixie/main amd64 iputils-ping amd64 3:20240905-3 [51.2 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 linux-sysctl-defaults all 4.12.1 [5724 B]
Fetched 643 kB in 0s (8968 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 4.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/x86_64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 4.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package libunistring5:amd64.
(Reading database ... 5645 files and directories currently installed.)
Preparing to unpack .../libunistring5_1.3-2_amd64.deb ...
Unpacking libunistring5:amd64 (1.3-2) ...
Selecting previously unselected package libidn2-0:amd64.
Preparing to unpack .../libidn2-0_2.3.8-2_amd64.deb ...
Unpacking libidn2-0:amd64 (2.3.8-2) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20240905-3_amd64.deb ...
Unpacking iputils-ping (3:20240905-3) ...
Selecting previously unselected package linux-sysctl-defaults.
Preparing to unpack .../linux-sysctl-defaults_4.12.1_all.deb ...
Unpacking linux-sysctl-defaults (4.12.1) ...
Setting up linux-sysctl-defaults (4.12.1) ...
Setting up libunistring5:amd64 (1.3-2) ...
Setting up libidn2-0:amd64 (2.3.8-2) ...
Setting up iputils-ping (3:20240905-3) ...
Processing triggers for libc-bin (2.41-12+deb13u3) ...
root@294f82cb2b55:/app# ping Database_container
PING Database_container (172.19.0.2) 56(84) bytes of data.
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=1 ttl=64 time=0.066 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=2 ttl=64 time=0.043 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=3 ttl=64 time=0.047 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=4 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=5 ttl=64 time=0.054 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=6 ttl=64 time=0.047 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=7 ttl=64 time=0.053 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=8 ttl=64 time=0.060 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=9 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=10 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=11 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=12 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=13 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=14 ttl=64 time=0.044 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=15 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=16 ttl=64 time=0.051 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=17 ttl=64 time=0.044 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=18 ttl=64 time=0.047 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=19 ttl=64 time=0.047 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=20 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=21 ttl=64 time=0.043 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=22 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=23 ttl=64 time=0.053 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=24 ttl=64 time=0.047 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=25 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=26 ttl=64 time=0.047 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=27 ttl=64 time=0.051 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=28 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=29 ttl=64 time=0.045 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=30 ttl=64 time=0.054 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=31 ttl=64 time=0.044 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=32 ttl=64 time=0.044 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=33 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=34 ttl=64 time=0.046 ms
64 bytes from Database_container.test_network (172.19.0.2): icmp_seq=35 ttl=64 time=0.045 ms

--- Database_container ping statistics ---
35 packets transmitted, 35 received, 0% packet loss, time 34840ms
rtt min/avg/max/mdev = 0.043/0.047/0.066/0.004 ms

```
