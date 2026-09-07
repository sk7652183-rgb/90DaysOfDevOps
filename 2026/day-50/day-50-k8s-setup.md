# Day 50 – Kubernetes Architecture and Cluster Setup

## Task 1: Recall the Kubernetes Story

### Why was Kubernetes created? What problem does it solve that Docker alone cannot?

Kubernetes was created to orchestrate and manage containers at scale. Docker runs containers, while Kubernetes handles scheduling, scaling, networking, and self-healing of containers.

### Who created Kubernetes and what was it inspired by? 

Kubernetes was created by Google engineers Joe Beda, Brendan Burns, and Craig McLuckie. It was inspired by Google’s internal container orchestration system called Borg.

### What does the name "Kubernetes" mean?

Kubernetes is a Greek word meaning “helmsman” or “pilot” — the person who steers a ship.

## Task 2: Draw the Kubernetes Architecture


```mermaid
flowchart TB
    kubectl["kubectl"]

    subgraph CP["CONTROL PLANE"]
        API["API Server"]
        ETCD["etcd<br/>Cluster State"]
        SCH["Scheduler"]
        CM["Controller Manager"]
    end

    subgraph W1["WORKER NODE 1"]
        K1["kubelet"]
        P1["kube-proxy"]
        C1["Container Runtime"]
        POD1["Pods"]
    end

    subgraph W2["WORKER NODE 2"]
        K2["kubelet"]
        P2["kube-proxy"]
        C2["Container Runtime"]
        POD2["Pods"]
    end

  The Control Plane manages the cluster. API Server is the entry point, etcd stores cluster state, Scheduler assigns Pods to nodes, and Controller Manager maintains the desired state. Worker nodes run the applications, where kubelet manages Pods, kube-proxy handles networking, and the container runtime runs the containers.

<img width="646" height="451" alt="image" src="https://github.com/user-attachments/assets/11c657bc-3c2f-45d2-8d5f-d686e8cd6ce1" />

### What happens when you run kubectl apply -f pod.yaml? Trace the request through each component.

kubectl sends the manifest to the API Server, the desired state is stored in etcd, Scheduler assigns a node, kubelet creates the Pod through the container runtime, and the status is reported back to the API Server

### What happens if the API server goes down?

If the API Server goes down, you cannot perform new Kubernetes operations like kubectl apply, create, delete, or update resources. Existing Pods generally continue running because kubelet and the container runtime can keep them running. However, cluster management and new scheduling/changes are affected until the API Server is restored.

### What happens if a worker node goes down?

If a worker node goes down, the Control Plane detects it through missed heartbeats. The Pods on that node become unavailable, and Kubernetes reschedules them on healthy worker nodes, provided the workload is managed by a controller such as a Deployment.

## Task 3: Install kubectl

```bash

ubuntu@ip-172-31-6-80:~$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100 59.01M 100 59.01M   0      0 284.0M      0                              0
ubuntu@ip-172-31-6-80:~$ kubectl version --client
Client Version: v1.37.0
Kustomize Version: v5.8.1
ubuntu@ip-172-31-6-80:~$


```

## Task 4: Set Up Your Local Cluster

### Option A: kind (Kubernetes in Docker)

```bash
ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website
ubuntu@ip-172-31-6-80:~$ curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100     86 100     86   0      0    568      0                              0
100 10.04M 100 10.04M   0      0 20.25M      0                              0
ubuntu@ip-172-31-6-80:~$ kind create cluster --name devops-cluster
Creating cluster "devops-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.37.0) 🖼️
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-devops-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-devops-cluster

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
ubuntu@ip-172-31-6-80:~$ kubectl cluster-info
kubectl get nodes
Kubernetes control plane is running at https://127.0.0.1:40421
CoreDNS is running at https://127.0.0.1:40421/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
NAME                           STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane   Ready    control-plane   33s   v1.37.0
ubuntu@ip-172-31-6-80:~$

```

<img width="1365" height="767" alt="image" src="https://github.com/user-attachments/assets/016b2bdd-c25d-4fea-88a5-4735f69de4d4" />

### Write down: Which one did you choose and why?
Minikube also runs Kubernetes locally, but Kind is more convenient for quickly creating multi-node Kubernetes clusters using Docker.

## Task 5: Explore Your Cluster

```bash

ubuntu@ip-172-31-6-80:~$ kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:40421
CoreDNS is running at https://127.0.0.1:40421/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
ubuntu@ip-172-31-6-80:~$ kubectl get nodes
NAME                           STATUS   ROLES           AGE   VERSION
devops-cluster-control-plane   Ready    control-plane   12m   v1.37.0
ubuntu@ip-172-31-6-80:~$ kubectl describe node devops-cluster-control-plane
Name:               devops-cluster-control-plane
Roles:              control-plane
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=devops-cluster-control-plane
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/control-plane=
Annotations:        node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Mon, 07 Sep 2026 09:56:25 +0000
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  devops-cluster-control-plane
  AcquireTime:     <unset>
  RenewTime:       Mon, 07 Sep 2026 10:09:04 +0000
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Mon, 07 Sep 2026 10:07:20 +0000   Mon, 07 Sep 2026 09:56:24 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 07 Sep 2026 10:07:20 +0000   Mon, 07 Sep 2026 09:56:24 +0000   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 07 Sep 2026 10:07:20 +0000   Mon, 07 Sep 2026 09:56:24 +0000   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 07 Sep 2026 10:07:20 +0000   Mon, 07 Sep 2026 09:56:45 +0000   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  172.20.0.2
  Hostname:    devops-cluster-control-plane
Capacity:
  cpu:                2
  ephemeral-storage:  30010245120
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3902880Ki
  pods:               110
Allocatable:
  cpu:                2
  ephemeral-storage:  30010245120
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3902880Ki
  pods:               110
System Info:
  Machine ID:                 6971f286339c40daad09233d85f59281
  System UUID:                09d12b05-c9e2-4c88-b8fe-b4812aa62237
  Boot ID:                    9a46a5ba-475b-4ada-94eb-b98cf4c1df21
  Kernel Version:             7.0.0-1011-aws
  OS Image:                   Debian GNU/Linux 13 (trixie)
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://2.3.4
  Kubelet Version:            v1.37.0
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
ProviderID:                   kind://docker/devops-cluster/devops-cluster-control-plane
Non-terminated Pods:          (9 in total)
  Namespace                   Name                                                    CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                                    ------------  ----------  ---------------  -------------  ---
  kube-system                 coredns-559f6c778d-5lgzn                                100m (5%)     0 (0%)      70Mi (1%)        170Mi (4%)     12m
  kube-system                 coredns-559f6c778d-hwd2m                                100m (5%)     0 (0%)      70Mi (1%)        170Mi (4%)     12m
  kube-system                 etcd-devops-cluster-control-plane                       100m (5%)     0 (0%)      100Mi (2%)       0 (0%)         12m
  kube-system                 kindnet-fs7c5                                           100m (5%)     0 (0%)      50Mi (1%)        0 (0%)         12m
  kube-system                 kube-apiserver-devops-cluster-control-plane             250m (12%)    0 (0%)      0 (0%)           0 (0%)         12m
  kube-system                 kube-controller-manager-devops-cluster-control-plane    200m (10%)    0 (0%)      0 (0%)           0 (0%)         12m
  kube-system                 kube-proxy-ksb2n                                        0 (0%)        0 (0%)      0 (0%)           0 (0%)         12m
  kube-system                 kube-scheduler-devops-cluster-control-plane             100m (5%)     0 (0%)      0 (0%)           0 (0%)         12m
  local-path-storage          local-path-provisioner-75f7fc7dc5-6mmtc                 0 (0%)        0 (0%)      0 (0%)           0 (0%)         12m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                950m (47%)  0 (0%)
  memory             290Mi (7%)  340Mi (8%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:
  Type    Reason                   Age   From             Message
  ----    ------                   ----  ----             -------
  Normal  NodeHasSufficientMemory  12m   kubelet          Node devops-cluster-control-plane status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    12m   kubelet          Node devops-cluster-control-plane status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     12m   kubelet          Node devops-cluster-control-plane status is now: NodeHasSufficientPID
  Normal  RegisteredNode           12m   node-controller  Node devops-cluster-control-plane event: Registered Node devops-cluster-control-plane in Controller
  Normal  NodeReady                12m   kubelet          Node devops-cluster-control-plane status is now: NodeReady
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get namespaces
NAME                 STATUS   AGE
default              Active   13m
kube-node-lease      Active   13m
kube-public          Active   13m
kube-system          Active   13m
local-path-storage   Active   13m
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get pods -A
NAMESPACE            NAME                                                   READY   STATUS    RESTARTS   AGE
kube-system          coredns-559f6c778d-5lgzn                               1/1     Running   0          13m
kube-system          coredns-559f6c778d-hwd2m                               1/1     Running   0          13m
kube-system          etcd-devops-cluster-control-plane                      1/1     Running   0          13m
kube-system          kindnet-fs7c5                                          1/1     Running   0          13m
kube-system          kube-apiserver-devops-cluster-control-plane            1/1     Running   0          13m
kube-system          kube-controller-manager-devops-cluster-control-plane   1/1     Running   0          13m
kube-system          kube-proxy-ksb2n                                       1/1     Running   0          13m
kube-system          kube-scheduler-devops-cluster-control-plane            1/1     Running   0          13m
local-path-storage   local-path-provisioner-75f7fc7dc5-6mmtc                1/1     Running   0          13m
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get pods -n kube-system
\NAME                                                   READY   STATUS    RESTARTS   AGE
coredns-559f6c778d-5lgzn                               1/1     Running   0          13m
coredns-559f6c778d-hwd2m                               1/1     Running   0          13m
etcd-devops-cluster-control-plane                      1/1     Running   0          13m
kindnet-fs7c5                                          1/1     Running   0          13m
kube-apiserver-devops-cluster-control-plane            1/1     Running   0          13m
kube-controller-manager-devops-cluster-control-plane   1/1     Running   0          13m
kube-proxy-ksb2n                                       1/1     Running   0          13m
kube-scheduler-devops-cluster-control-plane            1/1     Running   0          13m
ubuntu@ip-172-31-6-80:~$

```
## Verify: Can you match each running pod in kube-system to a component in your architecture diagram?

## Kubernetes Architecture Verification

The running Pods in the `kube-system` namespace can be mapped to the components in the Kubernetes architecture:

| Running Pod               | Kubernetes Component | Role                                                    |
| ------------------------- | -------------------- | ------------------------------------------------------- |
| `kube-apiserver`          | API Server           | Entry point for Kubernetes API requests                 |
| `etcd`                    | etcd                 | Stores the cluster state                                |
| `kube-scheduler`          | Scheduler            | Assigns Pods to suitable worker nodes                   |
| `kube-controller-manager` | Controller Manager   | Maintains the desired cluster state                     |
| `kube-proxy`              | kube-proxy           | Manages network rules for Pod communication             |
| `coredns`                 | CoreDNS              | Provides DNS-based service discovery inside the cluster |

### Verification Command

```bash
kubectl get pods -n kube-system

```

These Pods demonstrate how the Kubernetes Control Plane and Worker Node components work together to manage the cluster.

## Task 6: Practice Cluster Lifecycle

```bash

ubuntu@ip-172-31-6-80:~$ kind delete cluster --name devops-cluster
Deleting cluster "devops-cluster" ...
Deleted nodes: ["devops-cluster-control-plane"]
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kind create cluster --name devops-cluster
Creating cluster "devops-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.37.0) 🖼️
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-devops-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-devops-cluster

Thanks for using kind! 😊
ubuntu@ip-172-31-6-80:~$ kubectl get nodes
NAME                           STATUS     ROLES           AGE   VERSION
devops-cluster-control-plane   NotReady   control-plane   16s   v1.37.0
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl config current-context
kind-devops-cluster
ubuntu@ip-172-31-6-80:~$ kubectl config get-contexts
CURRENT   NAME                  CLUSTER               AUTHINFO              NAMESPACE
*         kind-devops-cluster   kind-devops-cluster   kind-devops-cluster
ubuntu@ip-172-31-6-80:~$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://127.0.0.1:46755
  name: kind-devops-cluster
contexts:
- context:
    cluster: kind-devops-cluster
    user: kind-devops-cluster
  name: kind-devops-cluster
current-context: kind-devops-cluster
kind: Config
users:
- name: kind-devops-cluster
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
ubuntu@ip-172-31-6-80:~$

```

## Write down: What is a kubeconfig? Where is it stored on your machine?


A **kubeconfig** is a configuration file used by `kubectl` to connect to and authenticate with a Kubernetes cluster. It contains information about the **cluster, user credentials, and contexts**.

### Location

By default, kubeconfig is stored at:

```bash
~/.kube/config
```

You can check it using:

```bash
ls -l ~/.kube/config
```

To view the current configuration:

```bash
kubectl config view
```



