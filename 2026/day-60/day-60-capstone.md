# Day 60 – Capstone: Deploy WordPress + MySQL on Kubernetes

## Task 1: Create the Namespace (Day 52)

### Create a capstone namespace and set it as the default namespace for the current Kubernetes context:

```bash

ubuntu@ip-172-31-6-80:~$ kubectl get ns
NAME                 STATUS   AGE
default              Active   20d
kube-node-lease      Active   20d
kube-public          Active   20d
kube-system          Active   20d
local-path-storage   Active   20d
ubuntu@ip-172-31-6-80:~$ kubectl create ns capstone
namespace/capstone created
ubuntu@ip-172-31-6-80:~$ kubectl get ns
NAME                 STATUS   AGE
capstone             Active   5s
default              Active   20d
kube-node-lease      Active   20d
kube-public          Active   20d
kube-system          Active   20d
local-path-storage   Active   20d
ubuntu@ip-172-31-6-80:~$ kubectl config set-context --current --namespace=capstone
Context "kind-devops-cluster" modified.
ubuntu@ip-172-31-6-80:~$

```

## Task 2: Deploy MySQL (Days 54-56)

### Create a Secret with MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, and MYSQL_PASSWORD using stringData, create a Headless Service (clusterIP: None) for MySQL on port 3306, create a MySQL StatefulSet using the mysql:8.0 image with envFrom referencing the Secret, resource requests of cpu: 250m and memory: 512Mi, limits of cpu: 500m and memory: 1Gi, and a volumeClaimTemplates requesting 1Gi of storage mounted at /var/lib/mysql, then verify MySQL with kubectl exec -it mysql-0 -- mysql -u <user> -p<password> -e "SHOW DATABASES;".

```bash

ubuntu@ip-172-31-6-80:~/day_60$ ls
db_secrets.yaml  service.yaml  statefulset.yaml
ubuntu@ip-172-31-6-80:~/day_60$ kubectl apply -f .
secret/db-mysql-secrets created
service/mysql-headless created
statefulset.apps/mysql created
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods
NAME      READY   STATUS    RESTARTS   AGE
mysql-0   1/1     Running   0          7s
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec -it mysql-0 -n capstone -- \
mysql -u app_user -p'AppUserSecurePassword456!' \
-e "SHOW DATABASES;"
mysql: [Warning] Using a password on the command line interface can be insecure.
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_application_db  |
| performance_schema |
+--------------------+
ubuntu@ip-172-31-6-80:~/day_60$ ls
db_secrets.yaml  service.yaml  statefulset.yaml
ubuntu@ip-172-31-6-80:~/day_60$ batcat db_secrets.yaml
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: db_secrets.yaml
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Secret
   3   │ metadata:
   4   │   name: db-mysql-secrets
   5   │   namespace: capstone
   6   │ type: Opaque
   7   │ stringData:
   8   │   MYSQL_ROOT_PASSWORD: "SuperSecureRootPassword123!"
   9   │   MYSQL_USER: "app_user"
  10   │   MYSQL_PASSWORD: "AppUserSecurePassword456!"
  11   │   MYSQL_DATABASE: "my_application_db"
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/day_60$ batcat service.yaml
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: service.yaml
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Service
   3   │ metadata:
   4   │   name: mysql-headless
   5   │   namespace: capstone
   6   │   labels:
   7   │     app: mysql
   8   │ spec:
   9   │   clusterIP: None  # 👈 Disables the virtual IP to make it headless
  10   │   selector:
  11   │     app: mysql     # 👈 Must match the label on your MySQL Pods
  12   │   ports:
  13   │     - name: mysql
  14   │       port: 3306   # 👈 The port accessible within the cluster
  15   │       targetPort: 3306
  16   │
───────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/day_60$ batcat statefulset.yaml
───────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: statefulset.yaml
───────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: apps/v1
   2   │ kind: StatefulSet
   3   │ metadata:
   4   │   name: mysql
   5   │   namespace: capstone
   6   │ spec:
   7   │   serviceName: "mysql-headless"
   8   │   replicas: 1
   9   │
  10   │   selector:
  11   │     matchLabels:
  12   │       app: mysql
  13   │
  14   │   template:
  15   │     metadata:
  16   │       labels:
  17   │         app: mysql
  18   │
  19   │     spec:
  20   │       terminationGracePeriodSeconds: 10
  21   │
  22   │       containers:
  23   │         - name: mysql
  24   │           image: mysql:8.0
  25   │
  26   │           ports:
  27   │             - containerPort: 3306
  28   │               name: mysql
  29   │
  30   │           envFrom:
  31   │             - secretRef:
  32   │                 name: db-mysql-secrets
  33   │
  34   │           resources:
  35   |                requests:
  36   │                   cpu: 250m
  37   │                   memory: 512Mi
  38   │                limits:
  39   │                 cpu: 500m
  40   │                 memory: 1Gi
  41   │
  42   │           volumeMounts:
  43   │             - name: mysql-data
  44   │               mountPath: /var/lib/mysql
  45   │
  46   │   volumeClaimTemplates:
  47   │     - metadata:
  48   │         name: mysql-data
  49   │       spec:
  50   │         accessModes:
  51   │           - ReadWriteOnce
  52   │         storageClassName: "standard"
  53   │         resources:
  54   │           requests:
  55   │             storage: 1Gi

```

### MySQL Verification

Verified the MySQL StatefulSet by connecting to the `mysql-0` Pod using the application user:

```bash
kubectl exec -it mysql-0 -n capstone -- \
mysql -u app_user -p'AppUserSecurePassword456!' \
-e "SHOW DATABASES;"
```

**Verification Result:**

```text
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_application_db  |
| performance_schema |
+--------------------+
```

This confirms that:

* MySQL StatefulSet is running successfully.
* The `app_user` credentials are working.
* The `my_application_db` database exists.
* MySQL is accessible from the Kubernetes Pod.

## Task 3: Deploy WordPress (Days 52, 54, 57)

### Create a ConfigMap with WORDPRESS_DB_HOST set to mysql-0.mysql.capstone.svc.cluster.local:3306 and WORDPRESS_DB_NAME, create a 2-replica Deployment using wordpress:latest with envFrom for the ConfigMap, secretKeyRef for WORDPRESS_DB_USER and WORDPRESS_DB_PASSWORD from the MySQL Secret, resource requests and limits, and liveness and readiness probes on /wp-login.php port 80, then wait until both Pods show 1/1 Running

```bash

ubuntu@ip-172-31-6-80:~/day_60$ ls
db_secrets.yaml  service.yaml  statefulset.yaml
ubuntu@ip-172-31-6-80:~/day_60$ ls
db_secrets.yaml  service.yaml  statefulset.yaml
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec -it mysql-0 -n capstone -- \
mysql -u root -p'SuperSecureRootPassword123!' \
-e "CREATE DATABASE wordpress;"
mysql: [Warning] Using a password on the command line interface can be insecure.
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec -it mysql-0 -n capstone -- \
mysql -u root -p'SuperSecureRootPassword123!' \
-e "GRANT ALL PRIVILEGES ON wordpress.* TO 'app_user'@'%'; FLUSH PRIVILEGES;"
mysql: [Warning] Using a password on the command line interface can be insecure.
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec -it mysql-0 -n capstone -- \
mysql -u app_user -p'AppUserSecurePassword456!' \
-e "SHOW DATABASES;"
mysql: [Warning] Using a password on the command line interface can be insecure.
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my_application_db  |
| performance_schema |
| wordpress          |
+--------------------+
ubuntu@ip-172-31-6-80:~/day_60$ vim wordpress-config.yaml
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ kubectl apply -f wordpress-config.yaml
configmap/wordpress-config created
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      10h
wordpress-config   2      18s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl describe configmap wordpress-config -n capstone
Name:         wordpress-config
Namespace:    capstone
Labels:       <none>
Annotations:  <none>

Data
====
WORDPRESS_DB_HOST:
----
mysql-0.mysql.capstone.svc.cluster.local:3306

WORDPRESS_DB_NAME:
----
wordpress


BinaryData
====

Events:  <none>
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ vim wordpress-deployment.yaml
ubuntu@ip-172-31-6-80:~/day_60$ kubectl apply -f wordpress-deployment.yaml
deployment.apps/wordpress created
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get deployment -n capstone
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
wordpress   0/2     2            0           8s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone
NAME                         READY   STATUS    RESTARTS   AGE
mysql-0                      1/1     Running   0          26m
wordpress-7b866d9b66-2j9dh   0/1     Running   0          24s
wordpress-7b866d9b66-b5jxf   0/1     Running   0          24s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone
NAME                         READY   STATUS    RESTARTS   AGE
mysql-0                      1/1     Running   0          26m
wordpress-7b866d9b66-2j9dh   0/1     Running   0          35s
wordpress-7b866d9b66-b5jxf   0/1     Running   0          35s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone
NAME                         READY   STATUS    RESTARTS   AGE
mysql-0                      1/1     Running   0          27m
wordpress-7b866d9b66-2j9dh   0/1     Running   0          56s
wordpress-7b866d9b66-b5jxf   0/1     Running   0          56s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl describe pod wordpress-7b866d9b66-2j9dh -n capstone
Name:             wordpress-7b866d9b66-2j9dh
Namespace:        capstone
Priority:         0
Service Account:  default
Node:             devops-cluster-control-plane/172.20.0.2
Start Time:       Mon, 28 Sep 2026 17:36:21 +0000
Labels:           app=wordpress
                  pod-template-hash=7b866d9b66
Annotations:      <none>
Status:           Running
IP:               10.244.0.9
IPs:
  IP:           10.244.0.9
Controlled By:  ReplicaSet/wordpress-7b866d9b66
Containers:
  wordpress:
    Container ID:   containerd://e77c133015a6c0cbc6ac3883cfa77c5860f8b524dad4a3b5511e5cdf34449983
    Image:          wordpress:latest
    Image ID:       docker.io/library/wordpress@sha256:e1736f6dba253975920681a6faf57f833973a5ae8a1057a495fe94bfced2d41c
    Port:           80/TCP (http)
    Host Port:      0/TCP (http)
    State:          Running
      Started:      Mon, 28 Sep 2026 17:37:24 +0000
    Last State:     Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Mon, 28 Sep 2026 17:36:31 +0000
      Finished:     Mon, 28 Sep 2026 17:37:23 +0000
    Ready:          False
    Restart Count:  1
    Limits:
      cpu:     500m
      memory:  512Mi
    Requests:
      cpu:      250m
      memory:   256Mi
    Liveness:   http-get http://:80/wp-login.php delay=30s timeout=5s period=10s successThreshold=1 failureThreshold=3
    Readiness:  http-get http://:80/wp-login.php delay=20s timeout=5s period=10s successThreshold=1 failureThreshold=3
    Environment Variables from:
      wordpress-config  ConfigMap  Optional: false
    Environment:
      WORDPRESS_DB_USER:      <set to the key 'MYSQL_USER' in secret 'db-mysql-secrets'>      Optional: false
      WORDPRESS_DB_PASSWORD:  <set to the key 'MYSQL_PASSWORD' in secret 'db-mysql-secrets'>  Optional: false
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-cxpd8 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       False
  ContainersReady             False
  PodScheduled                True
Volumes:
  kube-api-access-cxpd8:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                From               Message
  ----     ------     ----               ----               -------
  Normal   Scheduled  89s                default-scheduler  Successfully assigned capstone/wordpress-7b866d9b66-2j9dh to devops-cluster-control-plane
  Normal   Pulled     79s                kubelet            spec.containers{wordpress}: Successfully pulled image "wordpress:latest" in 868ms (9.477s including waiting). Image size: 274799291 bytes.
  Warning  Unhealthy  28s (x3 over 48s)  kubelet            spec.containers{wordpress}: Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing    28s                kubelet            spec.containers{wordpress}: Container wordpress failed liveness probe, will be restarted
  Normal   Pulling    27s (x2 over 88s)  kubelet            spec.containers{wordpress}: Pulling image "wordpress:latest"
  Normal   Created    26s (x2 over 79s)  kubelet            spec.containers{wordpress}: Container created
  Normal   Started    26s (x2 over 79s)  kubelet            spec.containers{wordpress}: Container started
  Normal   Pulled     26s                kubelet            spec.containers{wordpress}: Successfully pulled image "wordpress:latest" in 460ms (932ms including waiting). Image size: 274799291 bytes.
  Warning  Unhealthy  26s                kubelet            spec.containers{wordpress}: Readiness probe failed: Get "http://10.244.0.9:80/wp-login.php": dial tcp 10.244.0.9:80: connect: connection refused
  Warning  Unhealthy  5s (x5 over 58s)   kubelet            spec.containers{wordpress}: Readiness probe failed: HTTP probe failed with statuscode: 500
ubuntu@ip-172-31-6-80:~/day_60$ kubectl logs wordpress-7b866d9b66-2j9dh -n capstone
WordPress not found in /var/www/html - copying now...
Complete! WordPress has been successfully copied to /var/www/html
No 'wp-config.php' found in /var/www/html, but 'WORDPRESS_...' variables supplied; copying 'wp-config-docker.php' (WORDPRESS_DB_HOST WORDPRESS_DB_NAME WORDPRESS_DB_PASSWORD WORDPRESS_DB_USER)
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.244.0.9. Set the 'ServerName' directive globally to suppress this message
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 10.244.0.9. Set the 'ServerName' directive globally to suppress this message
[Mon Sep 28 17:38:25.141272 2026] [mpm_prefork:notice] [pid 1:tid 1] AH00163: Apache/2.4.68 (Debian) PHP/8.3.35 configured -- resuming normal operations
[Mon Sep 28 17:38:25.141310 2026] [core:notice] [pid 1:tid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec wordpress-7b866d9b66-2j9dh -n capstone -- \
sh -c 'echo "HOST=$WORDPRESS_DB_HOST"; echo "DB=$WORDPRESS_DB_NAME"; echo "USER=$WORDPRESS_DB_USER"'
HOST=mysql-0.mysql.capstone.svc.cluster.local:3306
DB=wordpress
USER=app_user
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec wordpress-7b866d9b66-2j9dh -n capstone -- \
getent hosts mysql-0.mysql.capstone.svc.cluster.local
command terminated with exit code 2
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec wordpress-7b866d9b66-2j9dh -n capstone -- \
sh -c 'echo > /dev/tcp/mysql-0.mysql.capstone.svc.cluster.local/3306' && echo "MySQL port is reachable"
sh: 1: cannot create /dev/tcp/mysql-0.mysql.capstone.svc.cluster.local/3306: Directory nonexistent
command terminated with exit code 2
ubuntu@ip-172-31-6-80:~/day_60$ kubectl exec -it wordpress-7b866d9b66-2j9dh -n capstone -- \
curl -I http://localhost/wp-login.php
HTTP/1.1 500 Internal Server Error
Date: Mon, 28 Sep 2026 17:40:14 GMT
Server: Apache/2.4.68 (Debian)
X-Powered-By: PHP/8.3.35
Expires: Wed, 11 Jan 1984 05:00:00 GMT
Cache-Control: no-cache, must-revalidate, max-age=0, no-store, private
Connection: close
Content-Type: text/html; charset=UTF-8

ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ ls
db_secrets.yaml  service.yaml  statefulset.yaml  wordpress-config.yaml  wordpress-deployment.yaml
ubuntu@ip-172-31-6-80:~/day_60$ vim wordpress-config.yaml
ubuntu@ip-172-31-6-80:~/day_60$ kubectl apply -f wordpress-config.yaml
configmap/wordpress-config configured
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ kubectl rollout restart deployment wordpress -n capstone
deployment.apps/wordpress restarted
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone -w
NAME                         READY   STATUS             RESTARTS      AGE
mysql-0                      1/1     Running            0             33m
wordpress-7b866d9b66-2j9dh   0/1     CrashLoopBackOff   5 (42s ago)   6m44s
wordpress-7b866d9b66-b5jxf   0/1     CrashLoopBackOff   5 (42s ago)   6m44s
wordpress-c5c7dd955-v2bwv    0/1     Pending            0             9s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone
NAME                         READY   STATUS             RESTARTS      AGE
mysql-0                      1/1     Running            0             33m
wordpress-7b866d9b66-2j9dh   0/1     CrashLoopBackOff   5 (68s ago)   7m10s
wordpress-7b866d9b66-b5jxf   0/1     CrashLoopBackOff   5 (68s ago)   7m10s
wordpress-c5c7dd955-v2bwv    0/1     Pending            0             35s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone
NAME                         READY   STATUS    RESTARTS       AGE
mysql-0                      1/1     Running   0              34m
wordpress-7b866d9b66-2j9dh   1/1     Running   6 (105s ago)   7m47s
wordpress-7b866d9b66-b5jxf   1/1     Running   6 (105s ago)   7m47s
wordpress-c5c7dd955-v2bwv    0/1     Pending   0              72s
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone
NAME                         READY   STATUS    RESTARTS       AGE
mysql-0                      1/1     Running   0              34m
wordpress-7b866d9b66-2j9dh   1/1     Running   6 (112s ago)   7m54s
wordpress-7b866d9b66-b5jxf   1/1     Running   6 (112s ago)   7m54s
wordpress-c5c7dd955-v2bwv    0/1     Pending   0              79s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl describe pod wordpress-c5c7dd955-v2bwv -n capstone
Name:             wordpress-c5c7dd955-v2bwv
Namespace:        capstone
Priority:         0
Service Account:  default
Node:             <none>
Labels:           app=wordpress
                  pod-template-hash=c5c7dd955
Annotations:      kubectl.kubernetes.io/restartedAt: 2026-09-28T17:42:56Z
Status:           Pending
IP:
IPs:              <none>
Controlled By:    ReplicaSet/wordpress-c5c7dd955
Containers:
  wordpress:
    Image:      wordpress:latest
    Port:       80/TCP (http)
    Host Port:  0/TCP (http)
    Limits:
      cpu:     500m
      memory:  512Mi
    Requests:
      cpu:      250m
      memory:   256Mi
    Liveness:   http-get http://:80/wp-login.php delay=30s timeout=5s period=10s successThreshold=1 failureThreshold=3
    Readiness:  http-get http://:80/wp-login.php delay=20s timeout=5s period=10s successThreshold=1 failureThreshold=3
    Environment Variables from:
      wordpress-config  ConfigMap  Optional: false
    Environment:
      WORDPRESS_DB_USER:      <set to the key 'MYSQL_USER' in secret 'db-mysql-secrets'>      Optional: false
      WORDPRESS_DB_PASSWORD:  <set to the key 'MYSQL_PASSWORD' in secret 'db-mysql-secrets'>  Optional: false
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-z5s74 (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  kube-api-access-z5s74:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  2m6s  default-scheduler  0/1 nodes are available: 1 Insufficient cpu. preemption: 0/1 nodes are available: 1 No preemption victims found for incoming pod.
ubuntu@ip-172-31-6-80:~/day_60$ kubectl top node
NAME                           CPU(cores)   CPU(%)   MEMORY(bytes)   MEMORY(%)
devops-cluster-control-plane   151m         7%       1428Mi          37%
ubuntu@ip-172-31-6-80:~/day_60$ kubectl top node
NAME                           CPU(cores)   CPU(%)   MEMORY(bytes)   MEMORY(%)
devops-cluster-control-plane   152m         7%       1430Mi          37%
ubuntu@ip-172-31-6-80:~/day_60$ kubectl top pods -A --sort-by=cpu
NAMESPACE            NAME                                                   CPU(cores)   MEMORY(bytes)
kube-system          kube-apiserver-devops-cluster-control-plane            29m          224Mi
capstone             mysql-0                                                25m          396Mi
kube-system          etcd-devops-cluster-control-plane                      17m          80Mi
kube-system          kube-controller-manager-devops-cluster-control-plane   13m          46Mi
capstone             wordpress-7b866d9b66-2j9dh                             11m          86Mi
capstone             wordpress-7b866d9b66-b5jxf                             11m          83Mi
kube-system          kube-scheduler-devops-cluster-control-plane            5m           22Mi
kube-system          coredns-559f6c778d-m7zvw                               2m           12Mi
kube-system          coredns-559f6c778d-mhrsc                               2m           13Mi
kube-system          metrics-server-66c544585c-wc64t                        2m           18Mi
kube-system          kindnet-cp24g                                          1m           7Mi
kube-system          kube-proxy-89dvk                                       1m           12Mi
local-path-storage   local-path-provisioner-75f7fc7dc5-whhwk                1m           8Mi
ubuntu@ip-172-31-6-80:~/day_60$ kubectl describe node devops-cluster-control-plane | grep -A15 "Allocated resources"
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests      Limits
  --------           --------      ------
  cpu                1800m (90%)   1500m (75%)
  memory             1514Mi (39%)  2388Mi (62%)
  ephemeral-storage  0 (0%)        0 (0%)
  hugepages-1Gi      0 (0%)        0 (0%)
  hugepages-2Mi      0 (0%)        0 (0%)
Events:
  Type     Reason          Age   From             Message
  ----     ------          ----  ----             -------
  Warning  Rebooted        41m   kubelet          Node devops-cluster-control-plane has been rebooted, boot id: 4f02296f-5ceb-4547-a741-b3d7e86c9ca9
  Normal   RegisteredNode  41m   node-controller  Node devops-cluster-control-plane event: Registered Node devops-cluster-control-plane in Controller
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get pods -n capstone                                                                                                 NAME                         READY   STATUS    RESTARTS        AGE
mysql-0                      1/1     Running   0               39m
wordpress-7b866d9b66-2j9dh   1/1     Running   6 (6m41s ago)   12m
wordpress-7b866d9b66-b5jxf   1/1     Running   6 (6m41s ago)   12m
wordpress-c5c7dd955-v2bwv    0/1     Pending   0               6m8s
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$
ubuntu@ip-172-31-6-80:~/day_60$

```

### Verify: Are both WordPress pods running and ready?
**Verify:** Run `kubectl get pods -n capstone -l app=wordpress` and confirm that both WordPress Pods are in `1/1 Running` state.

## Task 4: Expose WordPress (Day 53)

### Create a NodePort Service on port `30080` targeting the WordPress Pods, access WordPress using `minikube service wordpress -n capstone` on Minikube or `kubectl port-forward svc/wordpress 8080:80 -n capstone` on Kind, then complete the WordPress setup wizard and create a blog post.

```bash
ubuntu@ip-172-31-6-80:~/day_60$ ls
db_secrets.yaml  service.yaml  statefulset.yaml  wordpress-config.yaml  wordpress-deployment.yaml
ubuntu@ip-172-31-6-80:~/day_60$ vim wordpress-service.yaml
ubuntu@ip-172-31-6-80:~/day_60$ kubectl apply -f wordpress-service.yaml
service/wordpress-service created
ubuntu@ip-172-31-6-80:~/day_60$ kubectl get svc
NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
mysql-headless      ClusterIP   None           <none>        3306/TCP       48m
wordpress-service   NodePort    10.96.66.162   <none>        80:30080/TCP   16s
ubuntu@ip-172-31-6-80:~/day_60$ kubectl port-forward svc/wordpress 8080:80 -n capstone
Error from server (NotFound): services "wordpress" not found
ubuntu@ip-172-31-6-80:~/day_60$ kubectl port-forward svc/wordpress-service 8080:80 -n capstone
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
ubuntu@ip-172-31-6-80:~/day_60$ Read from remote host ec2-35-91-53-5.us-west-2.compute.amazonaws.com: Connection reset by peer
Connection to ec2-35-91-53-5.us-west-2.compute.amazonaws.com closed.
client_loop: send disconnect: Connection reset by peer
                                                                                                                                                            ✗

  2026-09-28   23:35.13   /home/mobaxterm/Desktop  ssh -i "testkey.pem.txt" ubuntu@ec2-35-91-53-5.us-west-2.compute.amazonaws.com
Welcome to Ubuntu 26.04 LTS (GNU/Linux 7.0.0-1013-aws x86_64)

 * Documentation:  https://docs.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Mon Sep 28 18:05:27 UTC 2026

  System load:  0.14               Temperature:              -273.1 C
  Usage of /:   53.0% of 27.95GB   Processes:                232
  Memory usage: 39%                Users logged in:          0
  Swap usage:   0%                 IPv4 address for enp39s0: 172.31.6.80

 * Ubuntu Pro delivers the most comprehensive open source security and
   compliance features.

   https://ubuntu.com/aws/pro

Expanded Security Maintenance for Applications is not enabled.

47 updates can be applied immediately.
2 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

3 additional security updates can be applied with ESM Apps.
Learn more about enabling ESM Apps service at https://ubuntu.com/esm


Last login: Mon Sep 28 17:06:22 2026 from 49.36.103.33
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl port-forward svc/wordpress-service 8080:80 -n capstone
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
ubuntu@ip-172-31-6-80:~$ kubectl port-forward --address 0.0.0.0 svc/wordpress-service 8080:80 -n capstone
Forwarding from 0.0.0.0:8080 -> 80
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
Handling connection for 8080
```

### Verify: Can you see the WordPress setup page?

<img width="1365" height="723" alt="image" src="https://github.com/user-attachments/assets/9e3ebd45-0032-4c74-90fd-0ca82cd8ad8f" />
