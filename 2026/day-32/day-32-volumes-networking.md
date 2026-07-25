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

