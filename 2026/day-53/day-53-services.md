# Day 53 – Kubernetes Services

## Task 1: Deploy the Application

created a Deployment that you will expose with Services. Create app-deployment.yaml:

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml  busybox-pod.yaml  namespace.yaml  nginx-deployment.yaml  nginx-pod.yaml  third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ batcat app-deployment.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: app-deployment.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: apps/v1
   2   │ kind: Deployment
   3   │ metadata:
   4   │   name: web-app
   5   │   labels:
   6   │     app: web-app
   7   │ spec:
   8   │   replicas: 3
   9   │   selector:
  10   │     matchLabels:
  11   │       app: web-app
  12   │   template:
  13   │     metadata:
  14   │       labels:
  15   │         app: web-app
  16   │     spec:
  17   │       containers:
  18   │       - name: nginx
  19   │         image: nginx:1.25
  20   │         ports:
  21   │         - containerPort: 80
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -o wide
NAME                       READY   STATUS    RESTARTS   AGE   IP            NODE                           NOMINATED NODE   READINESS GATES
web-app-56b5ddf4c5-52fxk   1/1     Running   0          83s   10.244.0.17   devops-cluster-control-plane   <none>           <none>
web-app-56b5ddf4c5-rczgp   1/1     Running   0          83s   10.244.0.18   devops-cluster-control-plane   <none>           <none>

```

### Verify: Are all 3 pods running? Note down their IP addresses.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl get pods -o wide
NAME                       READY   STATUS    RESTARTS   AGE   IP            NODE                           NOMINATED NODE   READINESS GATES
web-app-56b5ddf4c5-52fxk   1/1     Running   0          28m   10.244.0.17   devops-cluster-control-plane   <none>           <none>
web-app-56b5ddf4c5-rczgp   1/1     Running   0          28m   10.244.0.18   devops-cluster-control-plane   <none>           <none>
web-app-56b5ddf4c5-vjxwj   1/1     Running   0          28m   10.244.0.19   devops-cluster-control-plane   <none>           <none>
ubuntu@ip-172-31-6-80:~$

```

## Task 2: ClusterIP Service (Internal Access)

### ClusterIP is the default Service type. It gives your Pods a stable internal IP that is only reachable from within the cluster.

### Created clusterip-service.yaml:

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml  busybox-pod.yaml  clusterip-service.yaml  namespace.yaml  nginx-deployment.yaml  nginx-pod.yaml  third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f clusterip-service.yaml
service/web-app-clusterip created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get svc
NAME                TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
kubernetes          ClusterIP   10.96.0.1     <none>        443/TCP   4d
web-app-clusterip   ClusterIP   10.96.238.0   <none>        80/TCP    9s
ubuntu@ip-172-31-6-80:~/k8s$ batcat clusterip-service.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: clusterip-service.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Service
   3   │ metadata:
   4   │   name: web-app-clusterip
   5   │ spec:
   6   │   type: ClusterIP
   7   │   selector:
   8   │     app: web-app
   9   │   ports:
  10   │   - port: 80
  11   │     targetPort: 80
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$

```

### Now test it from inside the cluster:

```bash

ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl run test-client --image=busybox:latest --rm -it --restart=Never -- sh
All commands and output from this session will be recorded in container logs, including credentials and sensitive information passed through the command prompt.
If you don't see a command prompt, try pressing enter.
/ # wget -qO- http://web-app-clusterip
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ #
```
### Verify: Does the Service respond? Try running the wget command multiple times — the Service distributes traffic across all healthy Pods.
Yes, the Service responded successfully. I ran the wget command multiple times, and it returned the “Welcome to NGINX!” page, confirming that the Service is successfully routing traffic to the healthy Pods.

## Task 3: Discover Services with DNS

```bash
ubuntu@ip-172-31-6-80:~$ kubectl run dns-test --image=busybox:latest --rm -it --restart=Never -- sh
All commands and output from this session will be recorded in container logs, including credentials and sensitive information passed through the command prompt.
If you don't see a command prompt, try pressing enter.
/ #
/ #
/ # wget -qO- http://web-app-clusterip
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # wget -qO- http://web-app-clusterip.default.svc.cluster.local
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # nslookup web-app-clusterip
Server:         10.96.0.10
Address:        10.96.0.10:53

** server can't find web-app-clusterip.cluster.local: NXDOMAIN

Name:   web-app-clusterip.default.svc.cluster.local
Address: 10.96.238.0

** server can't find web-app-clusterip.svc.cluster.local: NXDOMAIN

** server can't find web-app-clusterip.svc.cluster.local: NXDOMAIN

** server can't find web-app-clusterip.cluster.local: NXDOMAIN


** server can't find web-app-clusterip.us-west-2.compute.internal: NXDOMAIN

** server can't find web-app-clusterip.us-west-2.compute.internal: NXDOMAIN

/ # nslookup web-app-clusterip.default.svc.cluster.local
Server:         10.96.0.10
Address:        10.96.0.10:53

Name:   web-app-clusterip.default.svc.cluster.local
Address: 10.96.238.0


/ #
```

**Verify:** What IP does `nslookup` return? Does it match the CLUSTER-IP from `kubectl get services`?

Yes. The `nslookup` command returned the IP address **10.96.238.0** for `web-app-clusterip.default.svc.cluster.local`. This matches the **CLUSTER-IP 10.96.238.0** shown by `kubectl get services`, confirming that the Service DNS is resolving to the correct ClusterIP.


## Task 4: NodePort Service (External Access via Node)

A NodePort Service exposes your application on a port on every node in the cluster. This lets you access the Service from outside the cluster.

Created nodeport-service.yaml:

```bash

ubuntu@ip-172-31-6-80:~/k8s$ vim nodeport-service.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ batcat nodeport-service.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: nodeport-service.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Service
   3   │ metadata:
   4   │   name: web-app-nodeport
   5   │ spec:
   6   │   type: NodePort
   7   │   selector:
   8   │     app: web-app
   9   │   ports:
  10   │   - port: 80
  11   │     targetPort: 80
  12   │     nodePort: 30080
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f nodeport-service.yaml
service/web-app-nodeport created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get services
NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes          ClusterIP   10.96.0.1      <none>        443/TCP        4d
web-app-clusterip   ClusterIP   10.96.238.0    <none>        80/TCP         27m
web-app-nodeport    NodePort    10.96.131.83   <none>        80:30080/TCP   8s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get nodes -o wide
NAME                           STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                       KERNEL-VERSION           CONTAINER-RUNTIME
devops-cluster-control-plane   Ready    control-plane   4d    v1.37.0   172.20.0.2    <none>        Debian GNU/Linux 13 (trixie)   7.0.0-1012-aws (amd64)   containerd://2.3.4
ubuntu@ip-172-31-6-80:~/k8s$ curl 10.96.131.83:30080
# Verified: Can you see the Nginx welcome page in your browser or terminal using the NodePort? I verified it using the curl command.
ubuntu@ip-172-31-6-80:~/k8s$ curl http://172.20.0.2:30080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
ubuntu@ip-172-31-6-80:~/k8s$

```

## Task 5: LoadBalancer Service (Cloud External Access)

In a cloud environment (AWS, GCP, Azure), a LoadBalancer Service provisions a real external load balancer that routes traffic to your nodes.

### Created loadbalancer-service.yaml:

```bash

ubuntu@ip-172-31-6-80:~/k8s$ vim loadbalancer-service.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ batcat loadbalancer-service.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: loadbalancer-service.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Service
   3   │ metadata:
   4   │   name: web-app-loadbalancer
   5   │ spec:
   6   │   type: LoadBalancer
   7   │   selector:
   8   │     app: web-app
   9   │   ports:
  10   │   - port: 80
  11   │     targetPort: 80
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f loadbalancer-service.yaml
service/web-app-loadbalancer created

ubuntu@ip-172-31-6-80:~/k8s$ kubectl get services
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes             ClusterIP      10.96.0.1       <none>        443/TCP        4d2h
web-app-clusterip      ClusterIP      10.96.238.0     <none>        80/TCP         97m
web-app-loadbalancer   LoadBalancer   10.96.227.113   <pending>     80:30160/TCP   11s
web-app-nodeport       NodePort       10.96.131.83    <none>        80:30080/TCP   69m
ubuntu@ip-172-31-6-80:~/k8s$

```

**Verified:** Why does the `EXTERNAL-IP` column show `<pending>` on a local cluster?

The `EXTERNAL-IP` remains `<pending>` because a local Kubernetes cluster does not have a cloud provider or cloud load balancer available to provision an external IP address automatically.

## Task 6: Understand the Service Types Side by Side

### Checked all three services:

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl get services -o wide
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE     SELECTOR
kubernetes             ClusterIP      10.96.0.1       <none>        443/TCP        4d2h    <none>
web-app-clusterip      ClusterIP      10.96.238.0     <none>        80/TCP         101m    app=web-app
web-app-loadbalancer   LoadBalancer   10.96.227.113   <pending>     80:30160/TCP   4m13s   app=web-app
web-app-nodeport       NodePort       10.96.131.83    <none>        80:30080/TCP   74m     app=web-app
ubuntu@ip-172-31-6-80:~/k8s$

```

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl get svc
NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes             ClusterIP      10.96.0.1       <none>        443/TCP        4d2h
web-app-clusterip      ClusterIP      10.96.238.0     <none>        80/TCP         104m
web-app-loadbalancer   LoadBalancer   10.96.227.113   <pending>     80:30160/TCP   7m29s
web-app-nodeport       NodePort       10.96.131.83    <none>        80:30080/TCP   77m
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe service web-app-loadbalancer
Name:                     web-app-loadbalancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=web-app
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.96.227.113
IPs:                      10.96.227.113
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30160/TCP
Endpoints:                10.244.0.17:80,10.244.0.18:80,10.244.0.19:80
Session Affinity:         None
External Traffic Policy:  Cluster
Internal Traffic Policy:  Cluster
Events:                   <none>
ubuntu@ip-172-31-6-80:~/k8s$

```
### **Verified:** The LoadBalancer service is also assigned a ClusterIP and a NodePort.

## Task 7: Clean Up

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete -f app-deployment.yaml
deployment.apps "web-app" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete -f clusterip-service.yaml
service "web-app-clusterip" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete -f nodeport-service.yaml
service "web-app-nodeport" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete -f loadbalancer-service.yaml
service "web-app-loadbalancer" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
dns-test   1/1     Running   0          94m
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4d2h
ubuntu@ip-172-31-6-80:~/k8s$

```

**Verified:** Everything has been cleaned up successfully.

