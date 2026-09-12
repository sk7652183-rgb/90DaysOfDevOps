# Day 54 – Kubernetes ConfigMaps and Secrets

## Task 1: Create a ConfigMap from Literals

### Use kubectl create configmap with --from-literal to create a ConfigMap called app-config with the keys APP_ENV=production, APP_DEBUG=false, and APP_PORT=8080, then inspect it using kubectl describe configmap app-config and kubectl get configmap app-config -o yaml and notice that the data is stored as plain text without encoding or encryption.

```bash
ubuntu@ip-172-31-6-80:~/k8s$ kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080
configmap/app-config created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe configmap app-config
Name:         app-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
APP_DEBUG:
----
false

APP_ENV:
----
production

APP_PORT:
----
8080


BinaryData
====

Events:  <none>
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get configmap app-config -o yaml
apiVersion: v1
data:
  APP_DEBUG: "false"
  APP_ENV: production
  APP_PORT: "8080"
kind: ConfigMap
metadata:
  creationTimestamp: "2026-09-12T14:50:52Z"
  name: app-config
  namespace: default
  resourceVersion: "290920"

```
Verified: All three key-value pairs are visible: APP_ENV=production, APP_DEBUG=false, and APP_PORT=8080.

## Task 2: Create a ConfigMap from a File

### Write a custom Nginx config file that adds a /health endpoint returning "healthy"

```bash
ubuntu@ip-172-31-6-80:~/k8s$ batcat default.conf
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: default.conf
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ server {
   2   │     listen 80;
   3   │
   4   │     location /health {
   5   │         default_type text/plain;
   6   │         return 200 "healthy\n";
   7   │     }
   8   │ }
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$

```

### Create a ConfigMap from this file using kubectl create configmap nginx-config --from-file=default.conf=<your-file>

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe configmap nginx-config
Name:         nginx-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
default.conf:
----
server {
    listen 80;

    location /health {
        default_type text/plain;
        return 200 "healthy\n";
    }
}



BinaryData
====

Events:  <none>
ubuntu@ip-172-31-6-80:~/k8s$

```

### The key name (default.conf) becomes the filename when mounted into a Pod

```bash

volumeMounts:
  - name: nginx-config
    mountPath: /etc/nginx/conf.d/default.conf
    subPath: default.conf

volumes:
  - name: nginx-config
    configMap:
      name: nginx-config

```

### Verify: Does kubectl get configmap nginx-config -o yaml show the file contents? 

```bash
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get configmap nginx-config -o yaml
apiVersion: v1
data:
  default.conf: |
    server {
        listen 80;

        location /health {
            default_type text/plain;
            return 200 "healthy\n";
        }
    }
kind: ConfigMap
metadata:
  creationTimestamp: "2026-09-12T15:07:37Z"
  name: nginx-config
  namespace: default
  resourceVersion: "292329"
  uid: d7d08f2a-3680-440d-96ed-362be078e801
ubuntu@ip-172-31-6-80:~/k8s$


```

## Task 3: Use ConfigMaps in a Pod

### Write a Pod manifest that uses envFrom with configMapRef to inject all keys from app-config as environment variables. Use a busybox container that prints the values.

```bash

apiVersion: v1
kind: Pod
metadata:
  name: app-config-pod
spec:
  containers:
    - name: busybox
      image: busybox:latest
      command: ["sh", "-c"]
      args:
        - echo "APP_ENV=$APP_ENV";
          echo "APP_DEBUG=$APP_DEBUG";
          echo "APP_PORT=$APP_PORT";
      envFrom:
        - configMapRef:
            name: app-config


```
```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl logs app-config-pod
APP_ENV=production
APP_DEBUG=false
APP_PORT=8080
ubuntu@ip-172-31-6-80:~/k8s$

```


### Write a second Pod manifest that mounts nginx-config as a volume at /etc/nginx/conf.d. Use the nginx image.

```bash

apiVersion: v1
kind: Pod
metadata:
  name: nginx-config-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: nginx-config-volume
          mountPath: /etc/nginx/conf.d

  volumes:
    - name: nginx-config-volume
      configMap:
        name: nginx-config

```

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec nginx-config-pod -- ls -l /etc/nginx/conf.d
total 0
lrwxrwxrwx 1 root root 19 Sep 12 16:55 default.conf -> ..data/default.conf
ubuntu@ip-172-31-6-80:~/k8s$

```
### Test that the mounted config works: kubectl exec <pod> -- curl -s http://localhost/health

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec nginx-config-pod -- curl -s http://localhost/health
healthy
ubuntu@ip-172-31-6-80:~/k8s$

```

Verify: Yes, the /health endpoint responds successfully with healthy.


## Task 4: Create a Secret

### Use kubectl create secret generic db-credentials with --from-literal to store DB_USER=admin and DB_PASSWORD=s3cureP@ssw0rd, inspect it using kubectl get secret db-credentials -o yaml to see that the values are Base64-encoded, and decode a value using echo '<base64-value>' | base64 --decode.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl create secret generic db-credentials \
  --from-literal=DB_USER=admin \
  --from-literal='DB_PASSWORD=s3cureP@ssw0rd'
secret/db-credentials created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get secret db-credentials -o yaml
apiVersion: v1
data:
  DB_PASSWORD: czNjdXJlUEBzc3cwcmQ=
  DB_USER: YWRtaW4=
kind: Secret
metadata:
  creationTimestamp: "2026-09-12T17:33:33Z"
  name: db-credentials
  namespace: default
  resourceVersion: "304717"
  uid: a0d53876-7f58-45f1-b033-3c11c2745d20
type: Opaque
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get secret db-credentials -o jsonpath='{.data.DB_USER}'
YWRtaW4=ubuntu@ip-172-31-6-80echo 'YWRtaW4=' | base64 --decode--decode
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get secret db-credentials -o jsonpath='{.data.DB_PASSWORD}' | base64 --decodeecode
echo
s3cureP@ssw0rd
ubuntu@ip-172-31-6-80:~/k8s$

```
Verify: Yes, the password can be decoded back to plaintext using base64 --decode, resulting in s3cureP@ssw0rd.


## Task 5: Use Secrets in a Pod

### Write a Pod manifest that injects DB_USER as an environment variable using secretKeyRef

```bash

ubuntu@ip-172-31-6-80:~/k8s$ batcat db-secret-pod.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: db-secret-pod.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Pod
   3   │ metadata:
   4   │   name: db-secret-pod
   5   │ spec:
   6   │   containers:
   7   │     - name: busybox
   8   │       image: busybox:latest
   9   │       command: ["sh", "-c"]
  10   │       args:
  11   │         - echo "DB_USER=$DB_USER"; sleep 3600
  12   │       env:
  13   │         - name: DB_USER
  14   │           valueFrom:
  15   │             secretKeyRef:
  16   │               name: db-credentials
  17   │               key: DB_USER
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f db-secret-pod.yaml
pod/db-secret-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl logs db-secret-pod
DB_USER=admin
ubuntu@ip-172-31-6-80:~/k8s$

```

### In the same Pod, mount the entire db-credentials Secret as a volume at /etc/db-credentials with readOnly: true, then verify that each Secret key becomes a separate file containing the decoded plaintext value.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f db-secret-pod.yaml
pod/db-secret-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec db-secret-pod -- ls -l /etc/db-credentials
total 0
lrwxrwxrwx    1 root     root            18 Sep 12 18:17 DB_PASSWORD -> ..data/DB_PASSWORD
lrwxrwxrwx    1 root     root            14 Sep 12 18:17 DB_USER -> ..data/DB_USER
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec db-secret-pod -- cat /etc/db-credentials/DB_USER
adminubuntu@ip-172-31-6-80:~/kubectl exec db-secret-pod -- ls -l /etc/db-credentialstials
total 0
lrwxrwxrwx    1 root     root            18 Sep 12 18:17 DB_PASSWORD -> ..data/DB_PASSWORD
lrwxrwxrwx    1 root     root            14 Sep 12 18:17 DB_USER -> ..data/DB_USER
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec db-secret-pod -- ls -l /etc/db-credentials
total 0
lrwxrwxrwx    1 root     root            18 Sep 12 18:17 DB_PASSWORD -> ..data/DB_PASSWORD
lrwxrwxrwx    1 root     root            14 Sep 12 18:17 DB_USER -> ..data/DB_USER
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec db-secret-pod -- cat /etc/db-credentials/DB_PASSWORD
s3cureP@ssw0rdubuntu@ip-172-31-6-80:~/k8s$


```

Verify: The mounted file values are plaintext, not Base64. Kubernetes automatically decodes the Secret values when mounting them as files.

## Task 6: Update a ConfigMap and Observe Propagation

### Create a ConfigMap live-config with a key message=hello

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl create configmap live-config --from-literal=message=hello
configmap/live-config created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get configmap live-config -o yaml
apiVersion: v1
data:
  message: hello
kind: ConfigMap
metadata:
  creationTimestamp: "2026-09-12T18:22:22Z"
  name: live-config
  namespace: default
  resourceVersion: "308898"
  uid: eef3633b-da73-4ad4-a242-32cbb98c0b3a
ubuntu@ip-172-31-6-80:~/k8s$

```
### Write a Pod that mounts this ConfigMap as a volume and reads the file in a loop every 5 seconds

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     db-secret-pod.yaml    loadbalancer-service.yaml  nginx-deployment.yaml  pod.yaml
busybox-pod.yaml        default.conf          namespace.yaml             nginx-pod.yaml         third-pod.yaml
clusterip-service.yaml  live-config-pod.yaml  nginx-config.yaml          nodeport-service.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f live-config-pod.yaml
pod/live-config-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl logs -f live-config-pod
Message: hello
Message: hello
Message: hello
Message: hello
Message: hello
ubuntu@ip-172-31-6-80:~/k8s$ batcat live-config-pod.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: live-config-pod.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Pod
   3   │ metadata:
   4   │   name: live-config-pod
   5   │ spec:
   6   │   containers:
   7   │     - name: busybox
   8   │       image: busybox:latest
   9   │       command: ["sh", "-c"]
  10   │       args:
  11   │         - |
  12   │           while true; do
  13   │             echo "Message: $(cat /etc/live-config/message)"
  14   │             sleep 5
  15   │           done
  16   │       volumeMounts:
  17   │         - name: live-config-volume
  18   │           mountPath: /etc/live-config
  19   │
  20   │   volumes:
  21   │     - name: live-config-volume
  22   │       configMap:
  23   │         name: live-config
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$



```
### Update the ConfigMap: kubectl patch configmap live-config --type merge -p '{"data":{"message":"world"}}'

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl patch configmap live-config --type merge -p '{"data":{"message":"world"}}'
configmap/live-config patched

```
### Wait 30-60 seconds — the volume-mounted value updates automatically

```bash

ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec live-config-pod -- cat /etc/live-config/message
worldubuntu@ip-172-31-6-80:~/k8s$

```
### Environment variables injected from a ConfigMap or Secret do not update automatically; they are set when the Pod starts, so the Pod must be restarted to pick up updated values.

### Verify: Yes, the volume-mounted value changed automatically from hello to world without restarting the Pod.

## Task 7: Clean Up

```bash

ubuntu@ip-172-31-6-80:~$ kubectl delete pod app-config-pod db-secret-pod nginx-config-pod live-config-pod
pod "app-config-pod" deleted from default namespace
pod "db-secret-pod" deleted from default namespace
pod "nginx-config-pod" deleted from default namespace
pod "live-config-pod" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl delete configmap app-config nginx-config live-config
configmap "app-config" deleted from default namespace
configmap "nginx-config" deleted from default namespace
configmap "live-config" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl delete secret db-credentials
secret "db-credentials" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pods
kubectl get configmaps
kubectl get secrets
NAME       READY   STATUS    RESTARTS   AGE
dns-test   0/1     Unknown   0          32h
NAME               DATA   AGE
kube-root-ca.crt   1      5d9h
No resources found in default namespace.
ubuntu@ip-172-31-6-80:~$


```
