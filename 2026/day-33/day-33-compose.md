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
```bash
sufiyan@Khan:~/compose-basics$ ls
docker-compose.yml
sufiyan@Khan:~/compose-basics$ batcat docker-compose.yml
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: docker-compose.yml
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ services:
   2   │   wordpress:
   3   │     image: wordpress:latest
   4   │     ports:
   5   │       - "8081:80"
   6   │     restart: always
   7   │     environment:
   8   │       WORDPRESS_DB_HOST: db:3306
   9   │       WORDPRESS_DB_USER: mysql
  10   │       WORDPRESS_DB_PASSWORD: root
  11   │       WORDPRESS_DB_NAME: wordpress
  12   │     depends_on:
  13   │       - db
  14   │ 
  15   │   db:
  16   │     image: mysql:8.0
  17   │     container_name: my_wordpress_db
  18   │     restart: always
  19   │     environment:
  20   │       MYSQL_DATABASE: wordpress
  21   │       MYSQL_USER: mysql
  22   │       MYSQL_PASSWORD: root
  23   │       MYSQL_ROOT_PASSWORD: root
  24   │     ports:
  25   │       - "3306:3306"
  26   │     volumes:
  27   │       - mysqldata:/var/lib/mysql
  28   │ 
  29   │ volumes:
  30   │   mysqldata:
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sufiyan@Khan:~/compose-basics$ 
```

```bash
sufiyan@Khan:~/compose-basics$ docker compose up -d
[+] Running 37/37
 ✔ db Pulled                                                                                                                                                                     580.2s 
   ✔ edf85873f64e Pull complete                                                                                                                                                  434.0s 
   ✔ 6ef6c7b50a93 Pull complete                                                                                                                                                  434.0s 
   ✔ e3e5d1ac74c1 Pull complete                                                                                                                                                  434.0s 
   ✔ 0d74d296605b Pull complete                                                                                                                                                  434.2s 
   ✔ 297d04cfe470 Pull complete                                                                                                                                                  434.3s 
   ✔ 4c8a3e0d4e4b Pull complete                                                                                                                                                  434.3s 
   ✔ a63160a5eda1 Pull complete                                                                                                                                                  488.5s 
   ✔ 7534d1db9f8d Pull complete                                                                                                                                                  488.6s 
   ✔ 49ec2dab01d9 Pull complete                                                                                                                                                  574.4s 
   ✔ ab24264a27e9 Pull complete                                                                                                                                                  574.4s 
   ✔ 96d30d9fbee8 Pull complete                                                                                                                                                  574.4s 
 ✔ wordpress Pulled                                                                                                                                                              456.2s 
   ✔ 062e450697fa Pull complete                                                                                                                                                  101.1s 
   ✔ 7c83ca722545 Pull complete                                                                                                                                                  101.1s 
   ✔ 44386657f794 Pull complete                                                                                                                                                  446.6s 
   ✔ 037d650346c2 Pull complete                                                                                                                                                  446.7s 
   ✔ ca2fb142004d Pull complete                                                                                                                                                  447.2s 
   ✔ fcfd475a43eb Pull complete                                                                                                                                                  447.2s 
   ✔ 51dcd84fc19d Pull complete                                                                                                                                                  447.2s 
   ✔ c106e34f24f0 Pull complete                                                                                                                                                  447.3s 
   ✔ 89c9311450e9 Pull complete                                                                                                                                                  447.3s 
   ✔ ca0a86e1fe83 Pull complete                                                                                                                                                  447.9s 
   ✔ b2a294cc6ea5 Pull complete                                                                                                                                                  447.9s 
   ✔ 39e8070c7837 Pull complete                                                                                                                                                  447.9s 
   ✔ ac333a3b6806 Pull complete                                                                                                                                                  447.9s 
   ✔ 5edcad93d812 Pull complete                                                                                                                                                  447.9s 
   ✔ 4f4fb700ef54 Pull complete                                                                                                                                                  447.9s 
   ✔ 7018160cfdc4 Pull complete                                                                                                                                                  449.0s 
   ✔ 549555a09d56 Pull complete                                                                                                                                                  449.9s 
   ✔ 08e4dab16f44 Pull complete                                                                                                                                                  449.9s 
   ✔ 7349cdd3b2ef Pull complete                                                                                                                                                  450.0s 
   ✔ 284a3920ac58 Pull complete                                                                                                                                                  450.0s 
   ✔ 3f55570f7e2e Pull complete                                                                                                                                                  451.7s 
   ✔ 1a51974cc1d1 Pull complete                                                                                                                                                  451.7s 
   ✔ a62992fa8fa0 Pull complete                                                                                                                                                  451.7s 
   ✔ 9cef31127f0c Pull complete                                                                                                                                                  451.7s 
[+] Running 4/4
 ✔ Network compose-basics_default        Created                                                                                                                                   0.1s 
 ✔ Volume "compose-basics_mysqldata"     Created                                                                                                                                   0.0s 
 ✔ Container my_wordpress_db             Started                                                                                                                                   1.9s 
 ✔ Container compose-basics-wordpress-1  Started                                                                                                                                   0.5s 
sufiyan@Khan:~/compose-basics$ 
                                                                                                                              0.5s 
sufiyan@Khan:~/compose-basics$ docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS                                                    NAMES
ad943c1122ee   wordpress:latest   "docker-entrypoint.s…"   24 seconds ago   Up 23 seconds   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp                  compose-basics-wordpress-1
035f17445527   mysql:8.0          "docker-entrypoint.s…"   26 seconds ago   Up 23 seconds   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp   my_wordpress_db
sufiyan@Khan:~/compose-basics$ 
```
### Start it, access WordPress in your browser, and set it up.
<img width="1858" height="1002" alt="image" src="https://github.com/user-attachments/assets/60ced663-c38f-4bc7-a8e3-3af96ec29a20" />
<img width="1858" height="1002" alt="image" src="https://github.com/user-attachments/assets/9e84351d-d899-4898-a072-8d1ecfddcd29" />


```bash
sufiyan@Khan:~/compose-basics$ docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS         PORTS                                                    NAMES
33a6329b69b3   wordpress:latest   "docker-entrypoint.s…"   7 minutes ago   Up 7 minutes   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp                  compose-basics-wordpress-1
f878b4a359b8   mysql:8.0          "docker-entrypoint.s…"   7 minutes ago   Up 7 minutes   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp   my_wordpress_db
sufiyan@Khan:~/compose-basics$ docker compose down
[+] Running 3/3
 ✔ Container compose-basics-wordpress-1  Removed                                                                                                                                   1.2s 
 ✔ Container my_wordpress_db             Removed                                                                                                                                   1.5s 
 ✔ Network compose-basics_default        Removed                                                                                                                                   0.1s 
sufiyan@Khan:~/compose-basics$ docker compose up -d
[+] Running 3/3
 ✔ Network compose-basics_default        Created                                                                                                                                   0.1s 
 ✔ Container my_wordpress_db             Started                                                                                                                                   0.3s 
 ✔ Container compose-basics-wordpress-1  Started                                                                                                                                   0.5s 
sufiyan@Khan:~/compose-basics$ 
```


### Verify: Stop and restart with docker compose down and docker compose up — is your WordPress data still there?
Verified data persistence by stopping the application with docker compose down and restarting it with docker compose up. The WordPress data was retained because the MySQL data was stored in the persistent mysqldata volume.

## Task 4: Compose Commands
### Start services in detached mode
```bash
sufiyan@Khan:~/compose-basics$ docker compose up -d
[+] Running 3/3
 ✔ Network compose-basics_default        Created                                                                                                                                   0.1s 
 ✔ Container my_wordpress_db             Started                                                                                                                                   0.3s 
 ✔ Container compose-basics-wordpress-1  Started                                                                                                                                   0.5s 
sufiyan@Khan:~/compose-basics$ 
```
### View running services
```bash
sufiyan@Khan:~/compose-basics$ docker compose ps
NAME                         IMAGE              COMMAND                  SERVICE     CREATED         STATUS         PORTS
compose-basics-wordpress-1   wordpress:latest   "docker-entrypoint.s…"   wordpress   2 minutes ago   Up 2 minutes   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp
my_wordpress_db              mysql:8.0          "docker-entrypoint.s…"   db          2 minutes ago   Up 2 minutes   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp
sufiyan@Khan:~/compose-basics$ 
```
### View logs of all services
```bash

sufiyan@Khan:~/compose-basics$ docker compose logs
my_wordpress_db  | 2026-07-28 19:27:50+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.46-1.el9 started.
my_wordpress_db  | 2026-07-28 19:27:51+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
my_wordpress_db  | 2026-07-28 19:27:51+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.46-1.el9 started.
my_wordpress_db  | '/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
my_wordpress_db  | 2026-07-28T19:27:51.836517Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
my_wordpress_db  | 2026-07-28T19:27:51.838796Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.46) starting as process 1
wordpress-1      | WordPress not found in /var/www/html - copying now...
wordpress-1      | Complete! WordPress has been successfully copied to /var/www/html
wordpress-1      | No 'wp-config.php' found in /var/www/html, but 'WORDPRESS_...' variables supplied; copying 'wp-config-docker.php' (WORDPRESS_DB_HOST WORDPRESS_DB_NAME WORDPRESS_DB_PASSWORD WORDPRESS_DB_USER)
wordpress-1      | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
wordpress-1      | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
my_wordpress_db  | 2026-07-28T19:27:51.846929Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
wordpress-1      | [Tue Jul 28 19:27:51.774305 2026] [mpm_prefork:notice] [pid 1:tid 1] AH00163: Apache/2.4.68 (Debian) PHP/8.3.32 configured -- resuming normal operations
my_wordpress_db  | 2026-07-28T19:27:52.048052Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
my_wordpress_db  | 2026-07-28T19:27:52.220069Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
my_wordpress_db  | 2026-07-28T19:27:52.220134Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
my_wordpress_db  | 2026-07-28T19:27:52.224236Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
my_wordpress_db  | 2026-07-28T19:27:52.254521Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
my_wordpress_db  | 2026-07-28T19:27:52.254621Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.46'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
wordpress-1      | [Tue Jul 28 19:27:51.774357 2026] [core:notice] [pid 1:tid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
wordpress-1      | 172.18.0.1 - - [28/Jul/2026:19:28:21 +0000] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 582 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
wordpress-1      | 172.18.0.1 - - [28/Jul/2026:19:28:22 +0000] "GET /wp-includes/images/spinner.gif HTTP/1.1" 200 3941 "http://localhost:8081/wp-admin/load-styles.php?c=0&dir=ltr&load%5Bchunk_0%5D=dashicons,admin-bar,site-health,common,forms,admin-menu,dashboard,list-tables,edit,revisions,media,themes,about,nav-menus,wp-poi&load%5Bchunk_1%5D=nter,widgets,site-icon,l10n,wp-base-styles,buttons,wp-auth-check,wp-components,wp-commands&ver=7.0.2" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
wordpress-1      | 172.18.0.1 - - [28/Jul/2026:19:28:22 +0000] "GET /wp-login.php?interim-login=1&wp_lang=en_US HTTP/1.1" 200 2603 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
wordpress-1      | 172.18.0.1 - - [28/Jul/2026:19:30:22 +0000] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 582 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
wordpress-1      | 172.18.0.1 - - [28/Jul/2026:19:32:22 +0000] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 582 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
wordpress-1      | 172.18.0.1 - - [28/Jul/2026:19:34:22 +0000] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 582 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
sufiyan@Khan:~/compose-basics$ 

```

### View logs of a specific service
```bash
sufiyan@Khan:~/compose-basics$ docker logs my_wordpress_db
2026-07-28 19:27:50+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.46-1.el9 started.
2026-07-28 19:27:51+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2026-07-28 19:27:51+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.46-1.el9 started.
'/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
2026-07-28T19:27:51.836517Z 0 [Warning] [MY-011068] [Server] The syntax '--skip-host-cache' is deprecated and will be removed in a future release. Please use SET GLOBAL host_cache_size=0 instead.
2026-07-28T19:27:51.838796Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.46) starting as process 1
2026-07-28T19:27:51.846929Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2026-07-28T19:27:52.048052Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2026-07-28T19:27:52.220069Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2026-07-28T19:27:52.220134Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2026-07-28T19:27:52.224236Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
2026-07-28T19:27:52.254521Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
2026-07-28T19:27:52.254621Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.46'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
sufiyan@Khan:~/compose-basics$ docker logs compose-basics-wordpress-1
WordPress not found in /var/www/html - copying now...
Complete! WordPress has been successfully copied to /var/www/html
No 'wp-config.php' found in /var/www/html, but 'WORDPRESS_...' variables supplied; copying 'wp-config-docker.php' (WORDPRESS_DB_HOST WORDPRESS_DB_NAME WORDPRESS_DB_PASSWORD WORDPRESS_DB_USER)
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
[Tue Jul 28 19:27:51.774305 2026] [mpm_prefork:notice] [pid 1:tid 1] AH00163: Apache/2.4.68 (Debian) PHP/8.3.32 configured -- resuming normal operations
[Tue Jul 28 19:27:51.774357 2026] [core:notice] [pid 1:tid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
172.18.0.1 - - [28/Jul/2026:19:28:21 +0000] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 582 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
172.18.0.1 - - [28/Jul/2026:19:28:22 +0000] "GET /wp-includes/images/spinner.gif HTTP/1.1" 200 3941 "http://localhost:8081/wp-admin/load-styles.php?c=0&dir=ltr&load%5Bchunk_0%5D=dashicons,admin-bar,site-health,common,forms,admin-menu,dashboard,list-tables,edit,revisions,media,themes,about,nav-menus,wp-poi&load%5Bchunk_1%5D=nter,widgets,site-icon,l10n,wp-base-styles,buttons,wp-auth-check,wp-components,wp-commands&ver=7.0.2" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
172.18.0.1 - - [28/Jul/2026:19:28:22 +0000] "GET /wp-login.php?interim-login=1&wp_lang=en_US HTTP/1.1" 200 2603 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
172.18.0.1 - - [28/Jul/2026:19:30:22 +0000] "POST /wp-admin/admin-ajax.php HTTP/1.1" 200 582 "http://localhost:8081/wp-admin/index.php" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36"
```
### Stop services without removing
```bash
sufiyan@Khan:~/compose-basics$ docker compose stop
[+] Stopping 2/2
 ✔ Container compose-basics-wordpress-1  Stopped                                                                                                                                   1.2s 
 ✔ Container my_wordpress_db             Stopped                                                                                                                                   1.3s 
sufiyan@Khan:~/compose-basics$ 
```
### Remove everything (containers, networks)
```bash
sufiyan@Khan:~/compose-basics$ docker compose down -v
[+] Running 4/4
 ✔ Container compose-basics-wordpress-1  Removed                                                                                                                                   0.2s 
 ✔ Container my_wordpress_db             Removed                                                                                                                                   0.0s 
 ✔ Volume compose-basics_mysqldata       Removed                                                                                                                                   0.0s 
 ✔ Network compose-basics_default        Removed                                                                                                                                   0.2s 
sufiyan@Khan:~/compose-basics$ 
```
### Rebuild images if you make a change
```bash
sufiyan@Khan:~/compose-basics$ docker compose up -d --build
[+] Running 4/4
 ✔ Network compose-basics_default        Created                                                                                                                                   0.1s 
 ✔ Volume "compose-basics_mysqldata"     Created                                                                                                                                   0.0s 
 ✔ Container my_wordpress_db             Started                                                                                                                                   0.4s 
 ✔ Container compose-basics-wordpress-1  Started                                                                                                                                   0.5s 
```

## Task 5: Environment Variables

### Add environment variables directly in your docker-compose.yml

```bash

sufiyan@Khan:~/compose-basics$ batcat docker-compose.yml
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: docker-compose.yml
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ services:
   2   │   wordpress:
   3   │     image: wordpress:latest
   4   │     ports:
   5   │       - "8081:80"
   6   │     restart: always
   7   │     environment:
   8   │       WORDPRESS_DB_HOST: db:3306
   9   │       WORDPRESS_DB_USER: mysql
  10   │       WORDPRESS_DB_PASSWORD: root
  11   │       WORDPRESS_DB_NAME: wordpress
  12   │     depends_on:
  13   │       - db
  14   │ 
  15   │   db:
  16   │     image: mysql:8.0
  17   │     container_name: my_wordpress_db
  18   │     restart: always
  19   │     environment:
  20   │       MYSQL_DATABASE: wordpress
  21   │       MYSQL_USER: mysql
  22   │       MYSQL_PASSWORD: root
  23   │       MYSQL_ROOT_PASSWORD: root
  24   │     ports:
  25   │       - "3306:3306"
  26   │     volumes:
  27   │       - mysqldata:/var/lib/mysql
  28   │ 
  29   │ volumes:
  30   │   mysqldata:
```

### Create a .env file and reference variables from it in your compose file

```bash
sufiyan@Khan:~/compose-basics$ ls
docker-compose.yml
sufiyan@Khan:~/compose-basics$ vim .env
sufiyan@Khan:~/compose-basics$ ls
docker-compose.yml
sufiyan@Khan:~/compose-basics$ 
sufiyan@Khan:~/compose-basics$ ls -la
total 16
drwxrwxr-x  2 sufiyan sufiyan 4096 Jul 29 01:19 .
drwxr-x--- 51 sufiyan sufiyan 4096 Jul 29 01:19 ..
-rw-rw-r--  1 sufiyan sufiyan  593 Jul 29 00:03 docker-compose.yml
-rw-rw-r--  1 sufiyan sufiyan  107 Jul 29 01:19 .env
sufiyan@Khan:~/compose-basics$ vim docker-compose.yml
sufiyan@Khan:~/compose-basics$ 
sufiyan@Khan:~/compose-basics$ batcat docker-compose.yml
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: docker-compose.yml
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ services:
   2   │   wordpress:
   3   │     image: wordpress:latest
   4   │     ports:
   5   │       - "${WORDPRESS_PORT}:80"
   6   │     restart: always
   7   │     environment:
   8   │       WORDPRESS_DB_HOST: db:3306
   9   │       WORDPRESS_DB_USER: ${MYSQL_USER}
  10   │       WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
  11   │       WORDPRESS_DB_NAME: ${MYSQL_DATABASE}
  12   │     depends_on:
  13   │       - db
  14   │ 
  15   │   db:
  16   │     image: mysql:8.0
  17   │     container_name: my_wordpress_db
  18   │     restart: always
  19   │     environment:
  20   │       MYSQL_DATABASE: ${MYSQL_DATABASE}
  21   │       MYSQL_USER: ${MYSQL_USER}
  22   │       MYSQL_PASSWORD: ${MYSQL_PASSWORD}
  23   │       MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
  24   │     ports:
  25   │       - "3306:3306"
  26   │     volumes:
  27   │       - mysqldata:/var/lib/mysql
  28   │ 
  29   │ volumes:
  30   │   mysqldata:
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sufiyan@Khan:~/compose-basics$ 
```
```bash
sufiyan@Khan:~/compose-basics$ batcat .env
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: .env
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ MYSQL_DATABASE=wordpress
   2   │ MYSQL_USER=mysql
   3   │ MYSQL_PASSWORD=root
   4   │ MYSQL_ROOT_PASSWORD=root
   5   │ WORDPRESS_PORT=8081
───────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
sufiyan@Khan:~/compose-basics$ 
```
### Verify the variables are being picked up

```bash
sufiyan@Khan:~/compose-basics$ docker compose up -d --build
[+] Running 37/37
 ✔ db Pulled                                                                                                                                                                     296.5s 
   ✔ edf85873f64e Pull complete                                                                                                                                                  202.4s 
   ✔ 6ef6c7b50a93 Pull complete                                                                                                                                                  202.4s 
   ✔ e3e5d1ac74c1 Pull complete                                                                                                                                                  202.4s 
   ✔ 0d74d296605b Pull complete                                                                                                                                                  202.7s 
   ✔ 297d04cfe470 Pull complete                                                                                                                                                  202.7s 
   ✔ 4c8a3e0d4e4b Pull complete                                                                                                                                                  202.7s 
   ✔ a63160a5eda1 Pull complete                                                                                                                                                  240.9s 
   ✔ 7534d1db9f8d Pull complete                                                                                                                                                  241.0s 
   ✔ 49ec2dab01d9 Pull complete                                                                                                                                                  290.0s 
   ✔ ab24264a27e9 Pull complete                                                                                                                                                  290.0s 
   ✔ 96d30d9fbee8 Pull complete                                                                                                                                                  290.0s 
 ✔ wordpress Pulled                                                                                                                                                              197.1s 
   ✔ 062e450697fa Pull complete                                                                                                                                                   54.9s 
   ✔ 7c83ca722545 Pull complete                                                                                                                                                   54.9s 
   ✔ 44386657f794 Pull complete                                                                                                                                                  187.3s 
   ✔ 037d650346c2 Pull complete                                                                                                                                                  187.3s 
   ✔ ca2fb142004d Pull complete                                                                                                                                                  187.6s 
   ✔ fcfd475a43eb Pull complete                                                                                                                                                  187.6s 
   ✔ 51dcd84fc19d Pull complete                                                                                                                                                  187.6s 
   ✔ c106e34f24f0 Pull complete                                                                                                                                                  187.8s 
   ✔ 89c9311450e9 Pull complete                                                                                                                                                  187.8s 
   ✔ ca0a86e1fe83 Pull complete                                                                                                                                                  188.4s 
   ✔ b2a294cc6ea5 Pull complete                                                                                                                                                  188.4s 
   ✔ 39e8070c7837 Pull complete                                                                                                                                                  188.5s 
   ✔ ac333a3b6806 Pull complete                                                                                                                                                  188.5s 
   ✔ 5edcad93d812 Pull complete                                                                                                                                                  188.5s 
   ✔ 4f4fb700ef54 Pull complete                                                                                                                                                  188.5s 
   ✔ 7018160cfdc4 Pull complete                                                                                                                                                  189.6s 
   ✔ 549555a09d56 Pull complete                                                                                                                                                  190.5s 
   ✔ 08e4dab16f44 Pull complete                                                                                                                                                  190.5s 
   ✔ 7349cdd3b2ef Pull complete                                                                                                                                                  190.5s 
   ✔ 284a3920ac58 Pull complete                                                                                                                                                  190.5s 
   ✔ 3f55570f7e2e Pull complete                                                                                                                                                  192.2s 
   ✔ 1a51974cc1d1 Pull complete                                                                                                                                                  192.2s 
   ✔ a62992fa8fa0 Pull complete                                                                                                                                                  192.2s 
   ✔ 9cef31127f0c Pull complete                                                                                                                                                  192.2s 
[+] Running 2/2
 ✔ Container my_wordpress_db             Started                                                                                                                                   1.8s 
 ✔ Container compose-basics-wordpress-1  Started                                                                                                                                   0.6s 
sufiyan@Khan:~/compose-basics$ docker compose config
name: compose-basics
services:
  db:
    container_name: my_wordpress_db
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_PASSWORD: root
      MYSQL_ROOT_PASSWORD: root
      MYSQL_USER: mysql
    image: mysql:8.0
    networks:
      default: null
    ports:
      - mode: ingress
        target: 3306
        published: "3306"
        protocol: tcp
    restart: always
    volumes:
      - type: volume
        source: mysqldata
        target: /var/lib/mysql
        volume: {}
  wordpress:
    depends_on:
      db:
        condition: service_started
        required: true
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_PASSWORD: root
      WORDPRESS_DB_USER: mysql
    image: wordpress:latest
    networks:
      default: null
    ports:
      - mode: ingress
        target: 80
        published: "8081"
        protocol: tcp
    restart: always
networks:
  default:
    name: compose-basics_default
volumes:
  mysqldata:
    name: compose-basics_mysqldata
sufiyan@Khan:~/compose-basics$ 

```
