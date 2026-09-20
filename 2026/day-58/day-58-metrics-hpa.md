# Day 58 – Metrics Server and Horizontal Pod Autoscaler (HPA)

## Task 1: Install the Metrics Server

### Check if Metrics Server is already running with kubectl get pods -n kube-system | grep metrics-server; if not, install it using minikube addons enable metrics-server for Minikube or apply the official Metrics Server manifest for Kind/kubeadm, add --kubelet-insecure-tls if required for local clusters (never in production), wait about 60 seconds, and verify with kubectl top nodes and kubectl top pods -A.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl top nodes
NAME                           CPU(cores)   CPU(%)   MEMORY(bytes)   MEMORY(%)
devops-cluster-control-plane   82m          4%       1110Mi          29%
ubuntu@ip-172-31-6-80:~/k8s$ kubectl top pods -A
NAMESPACE            NAME                                                   CPU(cores)   MEMORY(bytes)
kube-system          coredns-559f6c778d-m7zvw                               1m           35Mi
kube-system          coredns-559f6c778d-mhrsc                               1m           43Mi
kube-system          etcd-devops-cluster-control-plane                      13m          75Mi
kube-system          kindnet-cp24g                                          1m           46Mi
kube-system          kube-apiserver-devops-cluster-control-plane            27m          310Mi
kube-system          kube-controller-manager-devops-cluster-control-plane   9m           119Mi
kube-system          kube-proxy-89dvk                                       1m           53Mi
kube-system          kube-scheduler-devops-cluster-control-plane            5m           63Mi
kube-system          metrics-server-66c544585c-wc64t                        3m           16Mi
local-path-storage   local-path-provisioner-75f7fc7dc5-whhwk                1m           10Mi
ubuntu@ip-172-31-6-80:~/k8s$

```

### Verify: What is the current CPU and memory usage of your node?

```markdown

**Verify:** The `devops-cluster-control-plane` node is currently using **4% CPU (82m)** and **29% memory (1110Mi)**.

```

## Task 2: Explore kubectl top

### Run kubectl top nodes, kubectl top pods -A, and kubectl top pods -A --sort-by=cpu to view real-time resource usage (not requests or limits), with the data provided by Metrics Server, which polls kubelets every 15 seconds.

```bash

ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  k8s
ubuntu@ip-172-31-6-80:~$ kubectl top nodes
NAME                           CPU(cores)   CPU(%)   MEMORY(bytes)   MEMORY(%)
devops-cluster-control-plane   82m          4%       1112Mi          29%
ubuntu@ip-172-31-6-80:~$ kubectl top pods -A
NAMESPACE            NAME                                                   CPU(cores)   MEMORY(bytes)
kube-system          coredns-559f6c778d-m7zvw                               1m           35Mi
kube-system          coredns-559f6c778d-mhrsc                               1m           43Mi
kube-system          etcd-devops-cluster-control-plane                      13m          76Mi
kube-system          kindnet-cp24g                                          1m           46Mi
kube-system          kube-apiserver-devops-cluster-control-plane            22m          311Mi
kube-system          kube-controller-manager-devops-cluster-control-plane   9m           115Mi
kube-system          kube-proxy-89dvk                                       1m           53Mi
kube-system          kube-scheduler-devops-cluster-control-plane            5m           64Mi
kube-system          metrics-server-66c544585c-wc64t                        2m           17Mi
local-path-storage   local-path-provisioner-75f7fc7dc5-whhwk                1m           10Mi
ubuntu@ip-172-31-6-80:~$ kubectl top pods -A --sort-by=cpu
NAMESPACE            NAME                                                   CPU(cores)   MEMORY(bytes)
kube-system          kube-apiserver-devops-cluster-control-plane            27m          308Mi
kube-system          etcd-devops-cluster-control-plane                      13m          76Mi
kube-system          kube-controller-manager-devops-cluster-control-plane   9m           114Mi
kube-system          kube-scheduler-devops-cluster-control-plane            5m           64Mi
kube-system          metrics-server-66c544585c-wc64t                        2m           17Mi
kube-system          coredns-559f6c778d-m7zvw                               1m           35Mi
kube-system          coredns-559f6c778d-mhrsc                               1m           43Mi
kube-system          kindnet-cp24g                                          1m           46Mi
kube-system          kube-proxy-89dvk                                       1m           53Mi
local-path-storage   local-path-provisioner-75f7fc7dc5-whhwk                1m           10Mi
ubuntu@ip-172-31-6-80:~$

```
**Verify:** The `kube-apiserver-devops-cluster-control-plane` pod in the `kube-system` namespace is using the most CPU, at **27m (0.027 CPU cores)**.

## Task 3: Create a Deployment with CPU Requests

### Create a Deployment using the registry.k8s.io/hpa-example image with resources.requests.cpu: 200m so HPA can calculate CPU utilization percentages, then expose the Deployment as a Service using kubectl expose deployment php-apache --port=80.

```bash
ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  k8s
ubuntu@ip-172-31-6-80:~$ cd k8s
ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     dynamic-pod.yaml       liveness-demo.yaml         nginx-deployment.yaml   pv.yaml              resource-pod_test.yaml
busybox-pod.yaml        dynamic-pvc.yaml       loadbalancer-service.yaml  nginx-pod.yaml          pvc-pod.yaml         startup-probe.yaml
clusterip-service.yaml  emptydir-demo.yaml     memory-stress.yaml         nginx-statefulset.yaml  pvc.yaml             third-pod.yaml
db-secret-pod.yaml      headless-service.yaml  namespace.yaml             nodeport-service.yaml   readiness-demo.yaml
default.conf            live-config-pod.yaml   nginx-config.yaml          pod.yaml                resource-demo.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim php-apache-deployment.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f php-apache-deployment.yaml
deployment.apps/php-apache created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get deployment php-apache
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
php-apache   1/1     1            1           12s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -l app=php-apache
NAME                          READY   STATUS    RESTARTS   AGE
php-apache-6465bb9b65-8qdq5   1/1     Running   0          22s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl expose deployment php-apache --port=80
service/php-apache exposed
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get svc php-apache
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
php-apache   ClusterIP   10.96.121.103   <none>        80/TCP    17s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl top pods
NAME                          CPU(cores)   MEMORY(bytes)
php-apache-6465bb9b65-8qdq5   1m           9Mi
ubuntu@ip-172-31-6-80:~/k8s$

```
**Verify:** The php-apache-6465bb9b65-8qdq5 pod is currently using 1m CPU (0.001 CPU cores) and 9Mi memory.


## Task 4: Create an HPA (Imperative)

### Run kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10, then check the HPA using kubectl get hpa and kubectl describe hpa php-apache; the TARGETS may initially show <unknown>, so wait around 30 seconds for metrics to become available.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl get hpa
NAME         REFERENCE               TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache   cpu: 0%/50%   1         10        1          73s
ubuntu@ip-172-31-6-80:~$ kubectl describe hpa php-apache
Name:                                                  php-apache
Namespace:                                             default
Labels:                                                <none>
Annotations:                                           <none>
CreationTimestamp:                                     Sun, 20 Sep 2026 07:23:12 +0000
Reference:                                             Deployment/php-apache
Metrics:                                               ( current / target )
  resource cpu on pods  (as a percentage of request):  0% (1m) / 50%
Min replicas:                                          1
Max replicas:                                          10
Deployment pods:                                       1 current / 1 desired
Conditions:
  Type            Status  Reason               Message
  ----            ------  ------               -------
  AbleToScale     True    ScaleDownStabilized  recent recommendations were higher than current one, applying the highest recent recommendation
  ScalingActive   True    ValidMetricFound     the HPA was able to successfully calculate a replica count from cpu resource utilization (percentage of request)
  ScalingLimited  False   DesiredWithinRange   the desired count is within the acceptable range
Events:           <none>
ubuntu@ip-172-31-6-80:~$

```
**Verify:** The TARGETS column shows cpu: 0%/50%, meaning the current CPU utilization is 0%, while the HPA target is 50%.

## Task 5: Generate Load and Watch Autoscaling

### Start a load generator with kubectl run load-generator --image=busybox:1.36 --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://php-apache; done", watch the HPA using kubectl get hpa php-apache --watch, observe CPU rising above 50% and replicas increasing over 1–3 minutes, then stop the load with kubectl delete pod load-generator; HPA scale-down is intentionally slow due to the 5-minute stabilization window, so you do not need to wait.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl run load-generator --image=busybox:1.36 --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://php-apache; done"
pod/load-generator created
ubuntu@ip-172-31-6-80:~$ kubectl get hpa php-apache --watch
NAME         REFERENCE               TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache   cpu: 0%/50%   1         10        1          7m13s
php-apache   Deployment/php-apache   cpu: 70%/50%   1         10        1          7m15s
php-apache   Deployment/php-apache   cpu: 202%/50%   1         10        2          7m30s
php-apache   Deployment/php-apache   cpu: 140%/50%   1         10        4          7m45s
php-apache   Deployment/php-apache   cpu: 90%/50%    1         10        6          8m
php-apache   Deployment/php-apache   cpu: 108%/50%   1         10        8          8m15s
ubuntu@ip-172-31-6-80:~$ kubectl get pod
NAME                          READY   STATUS    RESTARTS   AGE
load-generator                1/1     Running   0          97s
php-apache-6465bb9b65-2748g   0/1     Pending   0          40s
php-apache-6465bb9b65-2knj8   0/1     Pending   0          55s
php-apache-6465bb9b65-44l8q   1/1     Running   0          70s
php-apache-6465bb9b65-8qdq5   1/1     Running   0          23m
php-apache-6465bb9b65-cccw9   1/1     Running   0          70s
php-apache-6465bb9b65-flt69   0/1     Pending   0          55s
php-apache-6465bb9b65-n2mqf   1/1     Running   0          85s
php-apache-6465bb9b65-vr5l5   0/1     Pending   0          40s
ubuntu@ip-172-31-6-80:~$ kubectl delete pod load-generator
pod "load-generator" deleted from default namespace
ubuntu@ip-172-31-6-80:~$

```
**Verify:** Under load, the HPA scaled the php-apache Deployment from 1 replica to 8 replicas, reaching a maximum of 8 replicas during the test.

## Task 6: Create an HPA from YAML (Declarative)

### Delete the imperative HPA using kubectl delete hpa php-apache, create an autoscaling/v2 HPA manifest with a 50% CPU utilization target and a behavior section for scale-up with no stabilization and scale-down with a 300-second stabilization window, then apply it and verify using kubectl describe hpa php-apache.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f php-apache-hpa.yaml
horizontalpodautoscaler.autoscaling/php-apache created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get hpa
NAME         REFERENCE               TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
php-apache   Deployment/php-apache   cpu: 0%/50%   1         10        1          8s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe hpa php-apache
Name:                                                  php-apache
Namespace:                                             default
Labels:                                                <none>
Annotations:                                           <none>
CreationTimestamp:                                     Sun, 20 Sep 2026 08:40:28 +0000
Reference:                                             Deployment/php-apache
Metrics:                                               ( current / target )
  resource cpu on pods  (as a percentage of request):  0% (1m) / 50%
Min replicas:                                          1
Max replicas:                                          10
Behavior:
  Scale Up:
    Stabilization Window: 0 seconds
    Select Policy: Max
    Policies:
      - Type: Percent  Value: 100  Period: 15 seconds
  Scale Down:
    Stabilization Window: 300 seconds
    Select Policy: Max
    Policies:
      - Type: Percent  Value: 50  Period: 60 seconds
Deployment pods:       1 current / 1 desired
Conditions:
  Type            Status  Reason               Message
  ----            ------  ------               -------
  AbleToScale     True    ScaleDownStabilized  recent recommendations were higher than current one, applying the highest recent recommendation
  ScalingActive   True    ValidMetricFound     the HPA was able to successfully calculate a replica count from cpu resource utilization (percentage of request)
  ScalingLimited  False   DesiredWithinRange   the desired count is within the acceptable range
Events:           <none>
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$

```

**Verify:** The behavior section controls how quickly the HPA scales up or scales down, including stabilization windows and scaling policies that determine the rate of replica changes.

## Task 7: Clean Up

### Delete the HPA, Service, Deployment, and load-generator pod. Leave the Metrics Server installed.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete hpa php-apache
kubectl delete service php-apache
kubectl delete deployment php-apache
kubectl delete pod load-generator --ignore-not-found
horizontalpodautoscaler.autoscaling "php-apache" deleted from default namespace
service "php-apache" deleted from default namespace
deployment.apps "php-apache" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get hpa,svc,deploy,pods
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   37h
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -n kube-system | grep metrics-server
metrics-server-66c544585c-wc64t                        1/1     Running   0              118m
ubuntu@ip-172-31-6-80:~/k8s$

```
