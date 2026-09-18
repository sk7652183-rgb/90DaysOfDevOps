# Day 57 – Resource Requests, Limits, and Probes

## Task 1: Resource Requests and Limits

### Create a Pod manifest with resources.requests (CPU: 100m, memory: 128Mi) and resources.limits (CPU: 250m, memory: 256Mi), apply it, and use kubectl describe pod to inspect the Requests, Limits, and QoS Class, which will be Burstable because the requests and limits differ (Guaranteed if equal and BestEffort if missing).

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     default.conf        headless-service.yaml      nginx-config.yaml       nodeport-service.yaml  pvc.yaml
busybox-pod.yaml        dynamic-pod.yaml    live-config-pod.yaml       nginx-deployment.yaml   pod.yaml               third-pod.yaml
clusterip-service.yaml  dynamic-pvc.yaml    loadbalancer-service.yaml  nginx-pod.yaml          pv.yaml
db-secret-pod.yaml      emptydir-demo.yaml  namespace.yaml             nginx-statefulset.yaml  pvc-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim resource-demo.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f resource-demo.yaml
pod/resource-demo created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe pod resource-demo
Name:             resource-demo
Namespace:        default
Priority:         0
Service Account:  default
Node:             devops-cluster-control-plane/172.20.0.2
Start Time:       Fri, 18 Sep 2026 14:20:15 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  nginx:
    Container ID:   containerd://f911d5926d58c5157ee40705474e57c8f9ed0234a31d627f7b1d5fee5bf439fd
    Image:          nginx:latest
    Image ID:       docker.io/library/nginx@sha256:cb29e33d254e8b72164ad5b8b9014f44e98e6e5f6d51ef02ee859f96708c90fe
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Fri, 18 Sep 2026 14:20:17 +0000
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     250m
      memory:  256Mi
    Requests:
      cpu:        100m
      memory:     128Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-bwv24 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-bwv24:
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
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  9s    default-scheduler  Successfully assigned default/resource-demo to devops-cluster-control-plane
  Normal  Pulling    8s    kubelet            spec.containers{nginx}: Pulling image "nginx:latest"
  Normal  Pulled     7s    kubelet            spec.containers{nginx}: Successfully pulled image "nginx:latest" in 1.248s (1.248s including waiting). Image size: 66343686 bytes.
  Normal  Created    7s    kubelet            spec.containers{nginx}: Container created
  Normal  Started    7s    kubelet            spec.containers{nginx}: Container started

```
### Verify: What QoS class does your Pod have?

My Pod has a Burstable QoS class because the CPU and memory requests and limits are configured, but the requests and limits are different.

## Task 2: OOMKilled — Exceeding Memory Limits

### Create a Pod using the polinux/stress image with a memory limit of 100Mi, configure the stress command to allocate 200M of memory, then apply and watch the Pod—the container will be killed due to exceeding its memory limit.

```bash

ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  k8s
ubuntu@ip-172-31-6-80:~$ cd k8s
ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     default.conf        headless-service.yaml      nginx-config.yaml       nodeport-service.yaml  pvc.yaml
busybox-pod.yaml        dynamic-pod.yaml    live-config-pod.yaml       nginx-deployment.yaml   pod.yaml               resource-demo.yaml
clusterip-service.yaml  dynamic-pvc.yaml    loadbalancer-service.yaml  nginx-pod.yaml          pv.yaml                third-pod.yaml
db-secret-pod.yaml      emptydir-demo.yaml  namespace.yaml             nginx-statefulset.yaml  pvc-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim memory-stress.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f memory-stress.yaml
pod/memory-stress created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod memory-stress -w
NAME            READY   STATUS             RESTARTS     AGE
memory-stress   0/1     CrashLoopBackOff   1 (7s ago)   10s
memory-stress   1/1     Running            2 (12s ago)   15s
memory-stress   0/1     OOMKilled          2 (13s ago)   16s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe pod memory-stress
Name:             memory-stress
Namespace:        default
Priority:         0
Service Account:  default
Node:             devops-cluster-control-plane/172.20.0.2
Start Time:       Fri, 18 Sep 2026 14:31:50 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.6
IPs:
  IP:  10.244.0.6
Containers:
  stress:
    Container ID:  containerd://6c11c36f210d37b1506086cbb1cacfffa8aa8dc27c056c4dda1cafee13e49e1e
    Image:         polinux/stress
    Image ID:      docker.io/polinux/stress@sha256:b6144f84f9c15dac80deb48d3a646b55c7043ab1d83ea0a697c09097aaad21aa
    Port:          <none>
    Host Port:     <none>
    Command:
      stress
    Args:
      --vm
      1
      --vm-bytes
      200M
      --vm-hang
      1
    State:          Terminated
      Reason:       OOMKilled
      Exit Code:    137
      Started:      Fri, 18 Sep 2026 14:32:30 +0000
      Finished:     Fri, 18 Sep 2026 14:32:30 +0000
    Last State:     Terminated
      Reason:       OOMKilled
      Exit Code:    137
      Started:      Fri, 18 Sep 2026 14:32:05 +0000
      Finished:     Fri, 18 Sep 2026 14:32:05 +0000
    Ready:          False
    Restart Count:  3
    Limits:
      memory:  100Mi
    Requests:
      memory:     100Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wn8sw (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       False
  ContainersReady             False
  PodScheduled                True
Volumes:
  kube-api-access-wn8sw:
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
  Type     Reason     Age               From               Message
  ----     ------     ----              ----               -------
  Normal   Scheduled  49s               default-scheduler  Successfully assigned default/memory-stress to devops-cluster-control-plane
  Normal   Pulled     47s               kubelet            spec.containers{stress}: Successfully pulled image "polinux/stress" in 1.521s (1.521s including waiting). Image size: 4041495 bytes.
  Normal   Pulled     46s               kubelet            spec.containers{stress}: Successfully pulled image "polinux/stress" in 453ms (453ms including waiting). Image size: 4041495 bytes.
  Normal   Pulling    9s (x4 over 49s)  kubelet            spec.containers{stress}: Pulling image "polinux/stress"
  Normal   Created    9s (x4 over 47s)  kubelet            spec.containers{stress}: Container created
  Normal   Started    9s (x4 over 47s)  kubelet            spec.containers{stress}: Container started
  Normal   Pulled     9s (x2 over 34s)  kubelet            spec.containers{stress}: Successfully pulled image "polinux/stress" in 443ms (443ms including waiting). Image size: 4041495 bytes.
  Warning  BackOff    8s (x4 over 45s)  kubelet            spec.containers{stress}: Back-off restarting failed container stress in pod memory-stress_default(fa2b1384-f189-4ec6-bec9-6ab7e3a98718)
ubuntu@ip-172-31-6-80:~/k8s$

```

### Verify: What exit code does an OOMKilled container have?

An OOMKilled container typically has exit code 137, which means the process was terminated by SIGKILL (signal 9) after exceeding its memory limit.

## Task 3: Pending Pod — Requesting Too Much

### Create a Pod manifest requesting cpu: 100 and memory: 128Gi, apply it and verify that its STATUS remains Pending, then run kubectl describe pod and check the Events section, where the scheduler reports that the Pod cannot be scheduled due to insufficient resources.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     default.conf        headless-service.yaml      namespace.yaml         nginx-statefulset.yaml  pvc-pod.yaml
busybox-pod.yaml        dynamic-pod.yaml    live-config-pod.yaml       nginx-config.yaml      nodeport-service.yaml   pvc.yaml
clusterip-service.yaml  dynamic-pvc.yaml    loadbalancer-service.yaml  nginx-deployment.yaml  pod.yaml                resource-demo.yaml
db-secret-pod.yaml      emptydir-demo.yaml  memory-stress.yaml         nginx-pod.yaml         pv.yaml                 third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim resource-pod_test.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f resource-pod_test.yaml
pod/resource-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod resource-pod
NAME           READY   STATUS    RESTARTS   AGE
resource-pod   0/1     Pending   0          9s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe pod resource-pod
Name:             resource-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             <none>
Labels:           <none>
Annotations:      <none>
Status:           Pending
IP:
IPs:              <none>
Containers:
  nginx:
    Image:      nginx
    Port:       <none>
    Host Port:  <none>
    Requests:
      cpu:        100
      memory:     128Gi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-wrlsz (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  kube-api-access-wrlsz:
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
  Warning  FailedScheduling  37s   default-scheduler  0/1 nodes are available: 1 Insufficient cpu, 1 Insufficient memory. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling.
ubuntu@ip-172-31-6-80:~/k8s$

```
### Verify: What event message does the scheduler produce?
The scheduler produces an event message such as FailedScheduling: Insufficient cpu, Insufficient memory, indicating that no available node has enough resources to schedule the Pod.

## Task 4: Liveness Probe

### Create a Pod manifest with a busybox container that creates /tmp/healthy on startup and deletes it after 30 seconds, then configure an exec liveness probe to run cat /tmp/healthy every 5 seconds with a failureThreshold of 3; after the file is deleted, three consecutive probe failures will cause Kubernetes to restart the container, which you can observe with kubectl get pod -w.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     dynamic-pod.yaml       loadbalancer-service.yaml  nginx-pod.yaml          pvc-pod.yaml
busybox-pod.yaml        dynamic-pvc.yaml       memory-stress.yaml         nginx-statefulset.yaml  pvc.yaml
clusterip-service.yaml  emptydir-demo.yaml     namespace.yaml             nodeport-service.yaml   resource-demo.yaml
db-secret-pod.yaml      headless-service.yaml  nginx-config.yaml          pod.yaml                resource-pod_test.yaml
default.conf            live-config-pod.yaml   nginx-deployment.yaml      pv.yaml                 third-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim liveness-demo.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f liveness-demo.yaml
pod/liveness-demo created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod liveness-demo -w
NAME            READY   STATUS    RESTARTS   AGE
liveness-demo   1/1     Running   0          9s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod liveness-demo
NAME            READY   STATUS    RESTARTS     AGE
liveness-demo   1/1     Running   1 (5s ago)   81s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod liveness-demo -w
NAME            READY   STATUS    RESTARTS      AGE
liveness-demo   1/1     Running   1 (13s ago)   89s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe pod liveness-demo
Name:             liveness-demo
Namespace:        default
Priority:         0
Service Account:  default
Node:             devops-cluster-control-plane/172.20.0.2
Start Time:       Fri, 18 Sep 2026 16:14:01 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.7
IPs:
  IP:  10.244.0.7
Containers:
  busybox:
    Container ID:  containerd://36191812a717a6c73901d1c941397128f16723cfd5491c05f1f7c3dca700a1f6
    Image:         busybox:latest
    Image ID:      docker.io/library/busybox@sha256:dc2d74b28e4cf8984fa52af1f39bc7c3d9c73760b41a74d629f5d11b1ab28616
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
      touch /tmp/healthy
      echo "Health file created"
      sleep 30
      rm -f /tmp/healthy
      echo "Health file deleted"
      sleep 3600

    State:          Running
      Started:      Fri, 18 Sep 2026 16:15:17 +0000
    Last State:     Terminated
      Reason:       Error
      Exit Code:    137
      Started:      Fri, 18 Sep 2026 16:14:02 +0000
      Finished:     Fri, 18 Sep 2026 16:15:17 +0000
    Ready:          True
    Restart Count:  1
    Liveness:       exec [cat /tmp/healthy] delay=0s timeout=1s period=5s successThreshold=1 failureThreshold=3
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-4vlqf (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-4vlqf:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                 From               Message
  ----     ------     ----                ----               -------
  Normal   Scheduled  113s                default-scheduler  Successfully assigned default/liveness-demo to devops-cluster-control-plane
  Normal   Pulled     112s                kubelet            spec.containers{busybox}: Successfully pulled image "busybox:latest" in 459ms (459ms including waiting). Image size: 2236931 bytes.
  Normal   Killing    67s                 kubelet            spec.containers{busybox}: Container busybox failed liveness probe, will be restarted
  Normal   Pulling    37s (x2 over 112s)  kubelet            spec.containers{busybox}: Pulling image "busybox:latest"
  Normal   Created    37s (x2 over 112s)  kubelet            spec.containers{busybox}: Container created
  Normal   Started    37s (x2 over 112s)  kubelet            spec.containers{busybox}: Container started
  Normal   Pulled     37s                 kubelet            spec.containers{busybox}: Successfully pulled image "busybox:latest" in 497ms (497ms including waiting). Image size: 2236931 bytes.
  Warning  Unhealthy  2s (x4 over 77s)    kubelet            spec.containers{busybox}: Liveness probe failed: cat: can't open '/tmp/healthy': No such file or directory
ubuntu@ip-172-31-6-80:~/k8s$

```
### Verify: How many times has the container restarted?
The container has restarted 4 times because the liveness probe failed after /tmp/healthy was deleted.

## Task 5: Readiness Probe

### Write an Nginx Pod manifest with a readinessProbe using httpGet on path / and port 80, expose it as a Service using kubectl expose pod <name> --port=80 --name=readiness-svc, verify the Pod IP is listed with kubectl get endpoints readiness-svc, then break the probe by deleting /usr/share/nginx/html/index.html and wait 15 seconds to confirm the Pod shows 0/1 READY, the endpoints are empty, and the container is not restarted.

```bash

ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  k8s
ubuntu@ip-172-31-6-80:~$ cd k8s
ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     dynamic-pod.yaml       liveness-demo.yaml         nginx-deployment.yaml   pv.yaml                 third-pod.yaml
busybox-pod.yaml        dynamic-pvc.yaml       loadbalancer-service.yaml  nginx-pod.yaml          pvc-pod.yaml
clusterip-service.yaml  emptydir-demo.yaml     memory-stress.yaml         nginx-statefulset.yaml  pvc.yaml
db-secret-pod.yaml      headless-service.yaml  namespace.yaml             nodeport-service.yaml   resource-demo.yaml
default.conf            live-config-pod.yaml   nginx-config.yaml          pod.yaml                resource-pod_test.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ vim readiness-demo.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f readiness-demo.yaml
pod/readiness-demo created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod readiness-demo
NAME             READY   STATUS    RESTARTS   AGE
readiness-demo   1/1     Running   0          11s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl expose pod readiness-demo --port=80 --name=readiness-svc
service/readiness-svc exposed
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get endpoints readiness-svc
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME            ENDPOINTS       AGE
readiness-svc   10.244.0.8:80   22s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec readiness-demo -- rm /usr/share/nginx/html/index.html
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod readiness-demo
NAME             READY   STATUS    RESTARTS   AGE
readiness-demo   1/1     Running   0          84s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod readiness-demo
NAME             READY   STATUS    RESTARTS   AGE
readiness-demo   0/1     Running   0          94s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get endpoints readiness-svc
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME            ENDPOINTS   AGE
readiness-svc               80s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod readiness-demo
NAME             READY   STATUS    RESTARTS   AGE
readiness-demo   0/1     Running   0          2m10s
ubuntu@ip-172-31-6-80:~/k8s$
```
### Verify: When readiness failed, was the container restarted?
No, the container was not restarted. A failed readiness probe only marks the Pod as Not Ready (0/1) and removes it from the Service endpoints; it does not restart the container.

## Task 6: Startup Probe

### Create a Pod manifest with a container that takes 20 seconds to start using sleep 20 && touch /tmp/started, add a startupProbe that checks /tmp/started every 5 seconds with a failureThreshold of 12 (providing a 60-second startup window), and add a livenessProbe that checks the same file, which will only begin running after the startup probe succeeds.

```bash

ubuntu@ip-172-31-6-80:~$ ls
Ecommerce-Website  k8s
ubuntu@ip-172-31-6-80:~$ cd k8s
ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     dynamic-pod.yaml       liveness-demo.yaml         nginx-deployment.yaml   pv.yaml              resource-pod_test.yaml
busybox-pod.yaml        dynamic-pvc.yaml       loadbalancer-service.yaml  nginx-pod.yaml          pvc-pod.yaml         third-pod.yaml
clusterip-service.yaml  emptydir-demo.yaml     memory-stress.yaml         nginx-statefulset.yaml  pvc.yaml
db-secret-pod.yaml      headless-service.yaml  namespace.yaml             nodeport-service.yaml   readiness-demo.yaml
default.conf            live-config-pod.yaml   nginx-config.yaml          pod.yaml                resource-demo.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim startup-probe.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f startup-probe.yaml
pod/startup-demo created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f startup-probe.yaml
pod/startup-demo unchanged
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod startup-demo -w
NAME           READY   STATUS    RESTARTS   AGE
startup-demo   1/1     Running   0          92s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe pod startup-demo
Name:             startup-demo
Namespace:        default
Priority:         0
Service Account:  default
Node:             devops-cluster-control-plane/172.20.0.2
Start Time:       Fri, 18 Sep 2026 17:57:18 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.9
IPs:
  IP:  10.244.0.9
Containers:
  app:
    Container ID:  containerd://6f46d9ff8a60b5d811261b76410c5b5a553097984ae2bc6ef4dc916643951bdb
    Image:         busybox:latest
    Image ID:      docker.io/library/busybox@sha256:dc2d74b28e4cf8984fa52af1f39bc7c3d9c73760b41a74d629f5d11b1ab28616
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
      -c
      echo "Container starting..."
      sleep 20
      touch /tmp/started
      echo "Application started"
      sleep 3600

    State:          Running
      Started:      Fri, 18 Sep 2026 17:57:19 +0000
    Ready:          True
    Restart Count:  0
    Liveness:       exec [cat /tmp/started] delay=0s timeout=1s period=5s successThreshold=1 failureThreshold=3
    Startup:        exec [cat /tmp/started] delay=0s timeout=1s period=5s successThreshold=1 failureThreshold=12
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-lvnj7 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-lvnj7:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                 From               Message
  ----     ------     ----                ----               -------
  Normal   Scheduled  112s                default-scheduler  Successfully assigned default/startup-demo to devops-cluster-control-plane
  Normal   Pulling    112s                kubelet            spec.containers{app}: Pulling image "busybox:latest"
  Normal   Pulled     111s                kubelet            spec.containers{app}: Successfully pulled image "busybox:latest" in 460ms (460ms including waiting). Image size: 2236931 bytes.
  Normal   Created    111s                kubelet            spec.containers{app}: Container created
  Normal   Started    111s                kubelet            spec.containers{app}: Container started
  Warning  Unhealthy  92s (x4 over 107s)  kubelet            spec.containers{app}: Startup probe failed: cat: can't open '/tmp/started': No such file or directory
ubuntu@ip-172-31-6-80:~$ kubectl get pod
NAME             READY   STATUS             RESTARTS         AGE
dns-test         0/1     Completed          0                25h
liveness-demo    0/1     CrashLoopBackOff   45 (3m31s ago)   162m
memory-stress    0/1     CrashLoopBackOff   56 (2m23s ago)   4h24m
readiness-demo   0/1     Running            0                73m
resource-demo    1/1     Running            0                4h36m
resource-pod     0/1     Pending            0                4h16m
startup-demo     1/1     Running            0                59m

```

### Verify: What would happen if failureThreshold were 2 instead of 12?
If failureThreshold were set to 2, the startup probe would fail twice (about 10 seconds with periodSeconds: 5), causing Kubernetes to restart the container before it finishes its 20-second startup. This could result in the container repeatedly restarting and never becoming ready.


## Task 7: Clean Up

Delete all pods and services you created.

```bash
ubuntu@ip-172-31-6-80:~$ kubectl get pod
NAME             READY   STATUS             RESTARTS         AGE
dns-test         0/1     Completed          0                25h
liveness-demo    0/1     CrashLoopBackOff   45 (3m31s ago)   162m
memory-stress    0/1     CrashLoopBackOff   56 (2m23s ago)   4h24m
readiness-demo   0/1     Running            0                73m
resource-demo    1/1     Running            0                4h36m
resource-pod     0/1     Pending            0                4h16m
startup-demo     1/1     Running            0                59m
ubuntu@ip-172-31-6-80:~$ kubectl delete pod resource-demo memory-stress liveness-demo readiness-demo startup-demo
pod "resource-demo" deleted from default namespace
pod "memory-stress" deleted from default namespace
pod "liveness-demo" deleted from default namespace
pod "readiness-demo" deleted from default namespace
pod "startup-demo" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl delete service readiness-svc
service "readiness-svc" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pods
kubectl get services
NAME           READY   STATUS      RESTARTS   AGE
dns-test       0/1     Completed   0          25h
resource-pod   0/1     Pending     0          4h17m
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   11d
ubuntu@ip-172-31-6-80:~$ kubectl delete pods --all
kubectl delete services --all
pod "dns-test" deleted from default namespace
pod "resource-pod" deleted from default namespace
service "kubernetes" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl delete pods --all
No resources found
ubuntu@ip-172-31-6-80:~$

```



