# Day 52 – Kubernetes Namespaces and Deployments

## Task 1: Explore Default Namespaces

### Kubernetes comes with built-in namespaces. The running pods were verified to be running in the kube-system namespace.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl get ns
NAME                 STATUS   AGE
default              Active   2d2h
kube-node-lease      Active   2d2h
kube-public          Active   2d2h
kube-system          Active   2d2h
local-path-storage   Active   2d2h

# Verified: How many pods are running in kube-system?

ubuntu@ip-172-31-6-80:~$ kubectl get pods -n kube-system
NAME                                                   READY   STATUS    RESTARTS        AGE
coredns-559f6c778d-m7zvw                               1/1     Running   2 (4m52s ago)   2d2h
coredns-559f6c778d-mhrsc                               1/1     Running   2 (4m52s ago)   2d2h
etcd-devops-cluster-control-plane                      1/1     Running   2 (4m52s ago)   2d2h
kindnet-cp24g                                          1/1     Running   2 (4m52s ago)   2d2h
kube-apiserver-devops-cluster-control-plane            1/1     Running   2 (4m52s ago)   2d2h
kube-controller-manager-devops-cluster-control-plane   1/1     Running   2 (4m52s ago)   2d2h
kube-proxy-89dvk                                       1/1     Running   2 (4m52s ago)   2d2h
kube-scheduler-devops-cluster-control-plane            1/1     Running   2 (4m52s ago)   2d2h
ubuntu@ip-172-31-6-80:~$

```

## Task 2: Create and Use Custom Namespaces

### Created two namespaces — one for a development environment and one for staging:

```bash

ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl create namespace dev
namespace/dev created
ubuntu@ip-172-31-6-80:~$ kubectl create namespace staging
namespace/staging created
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get ns
NAME                 STATUS   AGE
default              Active   2d3h
dev                  Active   16s
kube-node-lease      Active   2d3h
kube-public          Active   2d3h
kube-system          Active   2d3h
local-path-storage   Active   2d3h
staging              Active   7s
ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  k8s
ubuntu@ip-172-31-6-80:~$ cd k8s
ubuntu@ip-172-31-6-80:~/k8s$ ls
busybox-pod.yaml  nginx-pod.yaml  third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim namespace.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f namespace.yaml
namespace/production created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl run nginx-dev --image=nginx:latest -n dev
pod/nginx-dev created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl run nginx-staging --image=nginx:latest -n staging
pod/nginx-staging created
# Verified: kubectl get pods displays the pods in the current/default namespace, while kubectl get pods -A displays pods across all namespaces. The Kubernetes system pods can be seen under the kube-system namespace when using kubectl get pods -A.
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -A
NAMESPACE            NAME                                                   READY   STATUS    RESTARTS      AGE
dev                  nginx-dev                                              1/1     Running   0             65s
kube-system          coredns-559f6c778d-m7zvw                               1/1     Running   2 (57m ago)   2d3h
kube-system          coredns-559f6c778d-mhrsc                               1/1     Running   2 (57m ago)   2d3h
kube-system          etcd-devops-cluster-control-plane                      1/1     Running   2 (57m ago)   2d3h
kube-system          kindnet-cp24g                                          1/1     Running   2 (57m ago)   2d3h
kube-system          kube-apiserver-devops-cluster-control-plane            1/1     Running   2 (57m ago)   2d3h
kube-system          kube-controller-manager-devops-cluster-control-plane   1/1     Running   2 (57m ago)   2d3h
kube-system          kube-proxy-89dvk                                       1/1     Running   2 (57m ago)   2d3h
kube-system          kube-scheduler-devops-cluster-control-plane            1/1     Running   2 (57m ago)   2d3h
local-path-storage   local-path-provisioner-75f7fc7dc5-whhwk                1/1     Running   4 (56m ago)   2d3h
staging              nginx-staging                                          1/1     Running   0             55s
ubuntu@ip-172-31-6-80:~/k8s$


```

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl get ns
NAME                 STATUS   AGE
default              Active   2d3h
dev                  Active   9m44s
kube-node-lease      Active   2d3h
kube-public          Active   2d3h
kube-system          Active   2d3h
local-path-storage   Active   2d3h
production           Active   7m21s
staging              Active   9m35s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods
No resources found in default namespace.
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -n dev
NAME        READY   STATUS    RESTARTS   AGE
nginx-dev   1/1     Running   0          4m48s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -n staging
NAME            READY   STATUS    RESTARTS   AGE
nginx-staging   1/1     Running   0          4m49s
ubuntu@ip-172-31-6-80:~/k8s$

```

## Task 3: Create Your First Deployment

### A Deployment tells Kubernetes: "I want X replicas of this Pod running at all times." If a Pod crashes, the Deployment controller recreates it automatically.

### Created a file nginx-deployment.yaml:

```bash
ubuntu@ip-172-31-6-80:~/k8s$ ls
busybox-pod.yaml  namespace.yaml  nginx-deployment.yaml  nginx-pod.yaml  third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim nginx-deployment.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ ls
busybox-pod.yaml  namespace.yaml  nginx-deployment.yaml  nginx-pod.yaml  third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get deployments -n dev
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           14s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -n dev
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7f5f95d8d-c7rmk   1/1     Running   0          23s
nginx-deployment-7f5f95d8d-j2sxg   1/1     Running   0          23s
nginx-deployment-7f5f95d8d-slsgj   1/1     Running   0          23s
nginx-dev                          1/1     Running   0          25m
ubuntu@ip-172-31-6-80:~/k8s$

```

### Verify Deployment Columns

**Verified:** The `kubectl get deployment` output contains the following columns:

* **READY:** Shows the number of pods that are ready compared with the desired number of pods, for example, `3/3`.
* **UP-TO-DATE:** Shows the number of pods that have been updated to match the latest Deployment configuration.
* **AVAILABLE:** Shows the number of pods that are currently available and ready to serve traffic.

## Task 4: Self-Healing — Delete a Pod and Watch It Come Back
### The Deployment controller detects that only 2 of 3 desired replicas exist and immediately creates a new one. The deleted pod is replaced within seconds.

```bash
ubuntu@ip-172-31-6-80:~$ kubectl get pods -n dev
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7f5f95d8d-c7rmk   1/1     Running   0          7m34s
nginx-deployment-7f5f95d8d-j2sxg   1/1     Running   0          7m34s
nginx-deployment-7f5f95d8d-slsgj   1/1     Running   0          7m34s
nginx-dev                          1/1     Running   0          32m
ubuntu@ip-172-31-6-80:~$ kubectl delete pod nginx-deployment-7f5f95d8d-c7rmk  -n dev
pod "nginx-deployment-7f5f95d8d-c7rmk" deleted from dev namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pods -n dev
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7f5f95d8d-j2sxg   1/1     Running   0          8m19s
nginx-deployment-7f5f95d8d-mn7gq   1/1     Running   0          4s
nginx-deployment-7f5f95d8d-slsgj   1/1     Running   0          8m19s
nginx-dev                          1/1     Running   0          33m
ubuntu@ip-172-31-6-80:~$
```

### Verify Pod Replacement

**Verified:** The replacement pod has a **different name** from the deleted pod. Kubernetes automatically creates a new pod with a unique name to maintain the desired number of replicas.

## Task 5: Scale the Deployment

### Changed the number of replicas:

```bash

ubuntu@ip-172-31-6-80:~$ kubectl scale deployment nginx-deployment --replicas=5 -n dev
deployment.apps/nginx-deployment scaled
ubuntu@ip-172-31-6-80:~$ kubectl get pods -n dev
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7f5f95d8d-j2sxg   1/1     Running   0          13m
nginx-deployment-7f5f95d8d-mn7gq   1/1     Running   0          4m51s
nginx-deployment-7f5f95d8d-slsgj   1/1     Running   0          13m
nginx-deployment-7f5f95d8d-xhrd7   1/1     Running   0          5s
nginx-deployment-7f5f95d8d-z5x8h   1/1     Running   0          5s
nginx-dev                          1/1     Running   0          38m
ubuntu@ip-172-31-6-80:~$ kubectl scale deployment nginx-deployment --replicas=2 -n dev
deployment.apps/nginx-deployment scaled
ubuntu@ip-172-31-6-80:~$ kubectl get pods -n dev
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-7f5f95d8d-j2sxg   1/1     Running   0          13m
nginx-deployment-7f5f95d8d-slsgj   1/1     Running   0          13m
nginx-dev                          1/1     Running   0          38m
ubuntu@ip-172-31-6-80:~$
```

### Verify Pod Scaling

**Verified:** When the Deployment was scaled down from **5 replicas to 2**, Kubernetes terminated the **3 extra pods** and maintained only **2 running pods** to match the desired replica count.

## Task 6: Rolling Update

### Updated the Nginx image version to trigger a rolling update:

```bash

ubuntu@ip-172-31-6-80:~$ kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
deployment.apps/nginx-deployment image updated
ubuntu@ip-172-31-6-80:~$ kubectl rollout status deployment/nginx-deployment -n dev
deployment "nginx-deployment" successfully rolled out
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl rollout history deployment/nginx-deployment -n dev
deployment.apps/nginx-deployment
REVISION  CHANGE-CAUSE
1         <none>
2         <none>

ubuntu@ip-172-31-6-80:~$ kubectl rollout undo deployment/nginx-deployment -n dev
kubectl rollout status deployment/nginx-deployment -n dev
Warning: resource deployments/nginx-deployment was previously managed with 'kubectl apply'. Rolling back will not update the kubectl.kubernetes.io/last-applied-configuration annotation, which may cause unexpected behavior on future 'kubectl apply' operations. Consider using 'kubectl apply' with your previous configuration file instead.
deployment.apps/nginx-deployment rolled back
Waiting for deployment "nginx-deployment" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 1 out of 2 new replicas have been updated...
Waiting for deployment "nginx-deployment" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "nginx-deployment" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "nginx-deployment" rollout to finish: 1 old replicas are pending termination...
deployment "nginx-deployment" successfully rolled out
# Verify: What image version is running after the rollback?
ubuntu@ip-172-31-6-80:~$ kubectl describe deployment nginx-deployment -n dev | grep Image
    Image:         nginx:1.24
ubuntu@ip-172-31-6-80:~$

```
## Task 7: Clean Up

```bash
ubuntu@ip-172-31-6-80:~$ kubectl delete deployment nginx-deployment -n dev
kubectl delete pod nginx-dev -n dev
kubectl delete pod nginx-staging -n staging
kubectl delete namespace dev staging production
deployment.apps "nginx-deployment" deleted from dev namespace
pod "nginx-dev" deleted from dev namespace
pod "nginx-staging" deleted from staging namespace
namespace "dev" deleted
namespace "staging" deleted
namespace "production" deleted
ubuntu@ip-172-31-6-80:~$

```
## Verify: Are all your resources gone?

```bash
ubuntu@ip-172-31-6-80:~$ kubectl get namespaces
kubectl get pods -A
NAME                 STATUS   AGE
default              Active   2d4h
kube-node-lease      Active   2d4h
kube-public          Active   2d4h
kube-system          Active   2d4h
local-path-storage   Active   2d4h
NAMESPACE            NAME                                                   READY   STATUS    RESTARTS       AGE
kube-system          coredns-559f6c778d-m7zvw                               1/1     Running   2 (105m ago)   2d4h
kube-system          coredns-559f6c778d-mhrsc                               1/1     Running   2 (105m ago)   2d4h
kube-system          etcd-devops-cluster-control-plane                      1/1     Running   2 (105m ago)   2d4h
kube-system          kindnet-cp24g                                          1/1     Running   2 (105m ago)   2d4h
kube-system          kube-apiserver-devops-cluster-control-plane            1/1     Running   2 (105m ago)   2d4h
kube-system          kube-controller-manager-devops-cluster-control-plane   1/1     Running   2 (105m ago)   2d4h
kube-system          kube-proxy-89dvk                                       1/1     Running   2 (105m ago)   2d4h
kube-system          kube-scheduler-devops-cluster-control-plane            1/1     Running   2 (105m ago)   2d4h
local-path-storage   local-path-provisioner-75f7fc7dc5-whhwk                1/1     Running   4 (105m ago)   2d4h


```



