# Day 56 – Kubernetes StatefulSets

## Task 1: Understand the Problem

### Create a Deployment with 3 replicas using Nginx, check the pod names (they are randomly generated, such as `app-xyz-abc`), then delete one pod and observe that the replacement pod is created with a different random name.


```bash
ubuntu@ip-172-31-6-80:~$ kubectl create deployment nginx-deployment --image=nginx --replicas=3
deployment.apps/nginx-deployment created
ubuntu@ip-172-31-6-80:~$ kubectl get deployment
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           14s
ubuntu@ip-172-31-6-80:~$
ubuntu@ip-172-31-6-80:~$ kubectl get pod
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5467ddfc68-jpccg   1/1     Running   0          25s
nginx-deployment-5467ddfc68-kbns5   1/1     Running   0          25s
nginx-deployment-5467ddfc68-vqppp   1/1     Running   0          25s
ubuntu@ip-172-31-6-80:~$ kubectl delete pod nginx-deployment-5467ddfc68-jpccg
pod "nginx-deployment-5467ddfc68-jpccg" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pod
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5467ddfc68-hvz7q   1/1     Running   0          3s
nginx-deployment-5467ddfc68-kbns5   1/1     Running   0          104s
nginx-deployment-5467ddfc68-vqppp   1/1     Running   0          104s
ubuntu@ip-172-31-6-80:~$
```
### Delete the Deployment before moving on.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl delete deployment nginx-deployment
deployment.apps "nginx-deployment" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pod
No resources found in default namespace.
ubuntu@ip-172-31-6-80:~$ !


```

### Verify: Why would random pod names be a problem for a database cluster?
Use a StatefulSet instead of a Deployment. It provides stable Pod names, network identities, and persistent storage—important for database clusters.

## Task 2: Create a Headless Service

### Write a Service manifest with `clusterIP: None` to create a Headless Service, set the selector to match the labels used by your StatefulSet Pods, apply it, and confirm that the `CLUSTER-IP` shows `None`.


```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     db-secret-pod.yaml  dynamic-pvc.yaml      loadbalancer-service.yaml  nginx-deployment.yaml  pod.yaml      pvc.yaml
busybox-pod.yaml        default.conf        emptydir-demo.yaml    namespace.yaml             nginx-pod.yaml         pv.yaml       third-pod.yaml
clusterip-service.yaml  dynamic-pod.yaml    live-config-pod.yaml  nginx-config.yaml          nodeport-service.yaml  pvc-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim headless-service.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ vim headless-service.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f headless-service.yaml
service/nginx-headless created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get svc nginx-headless
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
nginx-headless   ClusterIP   None         <none>        80/TCP    9s
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     default.conf        headless-service.yaml      nginx-config.yaml      pod.yaml      third-pod.yaml
busybox-pod.yaml        dynamic-pod.yaml    live-config-pod.yaml       nginx-deployment.yaml  pv.yaml
clusterip-service.yaml  dynamic-pvc.yaml    loadbalancer-service.yaml  nginx-pod.yaml         pvc-pod.yaml
db-secret-pod.yaml      emptydir-demo.yaml  namespace.yaml             nodeport-service.yaml  pvc.yaml
ubuntu@ip-172-31-6-80:~/k8s$ vim nginx-statefulset.yaml
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f nginx-statefulset.yaml
statefulset.apps/nginx-statefulset created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods
kubectl get svc nginx-headless
NAME                  READY   STATUS    RESTARTS   AGE
nginx-statefulset-0   1/1     Running   0          10s
nginx-statefulset-1   1/1     Running   0          9s
nginx-statefulset-2   1/1     Running   0          8s
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
nginx-headless   ClusterIP   None         <none>        80/TCP    6m3s
ubuntu@ip-172-31-6-80:~/k8s$

```

### Verify: What does the CLUSTER-IP column show?

Verify: The CLUSTER-IP column shows None because a Headless Service does not have a virtual ClusterIP; instead, it provides DNS records for individual Pods. This is commonly used with StatefulSets for stable Pod network identities.

## Task 3: Create a StatefulSet

### Write a StatefulSet manifest with serviceName pointing to your Headless Service, set 3 replicas using the nginx image, add a volumeClaimTemplates section requesting 100Mi of ReadWriteOnce storage, then apply it and watch the Pods with kubectl get pods -l <your-label> -w.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ ls
app-deployment.yaml     default.conf        headless-service.yaml      nginx-config.yaml       nodeport-service.yaml  pvc.yaml
busybox-pod.yaml        dynamic-pod.yaml    live-config-pod.yaml       nginx-deployment.yaml   pod.yaml               third-pod.yaml
clusterip-service.yaml  dynamic-pvc.yaml    loadbalancer-service.yaml  nginx-pod.yaml          pv.yaml
db-secret-pod.yaml      emptydir-demo.yaml  namespace.yaml             nginx-statefulset.yaml  pvc-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f nginx-statefulset.yaml
statefulset.apps/nginx-statefulset created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -l app=nginx -w
NAME                  READY   STATUS    RESTARTS   AGE
nginx-statefulset-0   1/1     Running   0          8s
nginx-statefulset-1   1/1     Running   0          4s
nginx-statefulset-2   0/1     Pending   0          0s
nginx-statefulset-2   0/1     Pending   0          3s
nginx-statefulset-2   0/1     ContainerCreating   0          3s
nginx-statefulset-2   0/1     ContainerCreating   0          4s
nginx-statefulset-2   1/1     Running             0          4s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods -l
error: flag needs an argument: 'l' in -l
See 'kubectl get --help' for usage.
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods
NAME                  READY   STATUS      RESTARTS   AGE
dns-test              0/1     Completed   0          80m
nginx-statefulset-0   1/1     Running     0          47s
nginx-statefulset-1   1/1     Running     0          43s
nginx-statefulset-2   1/1     Running     0          39s
ubuntu@ip-172-31-6-80:~/k8s$

```
### Check the PVCs: kubectl get pvc — you should see web-data-web-0, web-data-web-1, web-data-web-2 (names follow the pattern <template-name>-<pod-name>).
```bash

ubuntu@ip-172-31-6-80:~$ kubectl get pvc
NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
nginx-storage-nginx-statefulset-0   Bound    pvc-7df8a34f-7c46-4a41-aba7-3413db02437e   100Mi      RWO            standard       <unset>                 31m
nginx-storage-nginx-statefulset-1   Bound    pvc-bb4c2fe8-30a7-4d74-9b05-57bf0cb77efc   100Mi      RWO            standard       <unset>                 31m
nginx-storage-nginx-statefulset-2   Bound    pvc-6e10936d-b840-4bed-8734-1c2f2ba431e7   100Mi      RWO            standard       <unset>                 31m
ubuntu@ip-172-31-6-80:~$

```

### Verify: What are the exact pod names and PVC names?

Verify: The Pods are nginx-statefulset-0, nginx-statefulset-1, nginx-statefulset-2, with corresponding PVCs nginx-storage-nginx-statefulset-0, nginx-storage-nginx-statefulset-1, and nginx-storage-nginx-statefulset-2.

## Task 4: Stable Network Identity

### Run a temporary busybox pod and use nslookup to resolve web-0.<your-headless-service>.default.svc.cluster.local Do the same for web-1 and web-2 Confirm the IPs match kubectl get pods -o wide

```bash
ubuntu@ip-172-31-6-80:~$ kubectl run dns-test --image=busybox:1.36 --restart=Never -- sleep 3600
pod/dns-test created
ubuntu@ip-172-31-6-80:~$ kubectl exec -it dns-test -- nslookup nginx-statefulset-0.nginx-headless.default.svc.cluster.local
Server:         10.96.0.10
Address:        10.96.0.10:53


Name:   nginx-statefulset-0.nginx-headless.default.svc.cluster.local
Address: 10.244.0.14

ubuntu@ip-172-31-6-80:~$ kubectl exec -it dns-test -- nslookup nginx-statefulset-1.nginx-headless.default.svc.cluster.local
kubectl exec -it dns-test -- nslookup nginx-statefulset-2.nginx-headless.default.svc.cluster.local
Server:         10.96.0.10
Address:        10.96.0.10:53


Name:   nginx-statefulset-1.nginx-headless.default.svc.cluster.local
Address: 10.244.0.16

Server:         10.96.0.10
Address:        10.96.0.10:53


Name:   nginx-statefulset-2.nginx-headless.default.svc.cluster.local
Address: 10.244.0.18

ubuntu@ip-172-31-6-80:~$ kubectl get pods -o wide
NAME                  READY   STATUS    RESTARTS   AGE     IP            NODE                           NOMINATED NODE   READINESS GATES
dns-test              1/1     Running   0          3m12s   10.244.0.19   devops-cluster-control-plane   <none>           <none>
nginx-statefulset-0   1/1     Running   0          171m    10.244.0.14   devops-cluster-control-plane   <none>           <none>
nginx-statefulset-1   1/1     Running   0          171m    10.244.0.16   devops-cluster-control-plane   <none>           <none>
nginx-statefulset-2   1/1     Running   0          171m    10.244.0.18   devops-cluster-control-plane   <none>           <none>
ubuntu@ip-172-31-6-80:~$


```

### Verify: Yes, the nslookup IP matches the corresponding Pod IP shown by kubectl get pods -o wide.

## Task 5: Stable Storage — Data Survives Pod Deletion

### Write unique data to each Pod using kubectl exec web-0 -- sh -c "echo 'Data from web-0' > /usr/share/nginx/html/index.html", delete web-0 with kubectl delete pod web-0, wait for the Pod to be recreated, and verify that the data still shows "Data from web-0", confirming that the data persisted.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl exec nginx-statefulset-0 -- sh -c "echo 'Data from nginx-statefulset-0' > /usr/share/nginx/html/index.html"
ubuntu@ip-172-31-6-80:~$ kubectl delete pod nginx-statefulset-0
pod "nginx-statefulset-0" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pods -w
NAME                  READY   STATUS    RESTARTS   AGE
dns-test              1/1     Running   0          10m
nginx-statefulset-0   1/1     Running   0          9s
nginx-statefulset-1   1/1     Running   0          178m
nginx-statefulset-2   1/1     Running   0          178m
ubuntu@ip-172-31-6-80:~$ kubectl exec nginx-statefulset-0 -- cat /usr/share/nginx/html/index.html
Data from nginx-statefulset-0
ubuntu@ip-172-31-6-80:~$

```
### Verify: Is the data identical after pod recreation?
Yes, the data remains identical after Pod recreation because the StatefulSet reattaches the same PVC to the recreated Pod, confirming that the data persisted.

## Task 6: Ordered Scaling

### Scale the StatefulSet up to 5 replicas with kubectl scale statefulset web --replicas=5, verify that Pods are created in order (web-3 then web-4), scale it back down to 3 replicas with kubectl scale statefulset web --replicas=3 and verify that Pods terminate in reverse order (web-4 then web-3), then run kubectl get pvc to confirm that all five PVCs still exist and their data is preserved for future scale-up.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl get statefulset
NAME                READY   AGE
nginx-statefulset   3/3     3h5m
ubuntu@ip-172-31-6-80:~$ kubectl scale statefulset nginx-statefulset --replicas=5
statefulset.apps/nginx-statefulset scaled
ubuntu@ip-172-31-6-80:~$ kubectl get pods -w
NAME                  READY   STATUS    RESTARTS   AGE
dns-test              1/1     Running   0          17m
nginx-statefulset-0   1/1     Running   0          7m18s
nginx-statefulset-1   1/1     Running   0          3h5m
nginx-statefulset-2   1/1     Running   0          3h5m
nginx-statefulset-3   1/1     Running   0          11s
nginx-statefulset-4   1/1     Running   0          6s
ubuntu@ip-172-31-6-80:~$ kubectl scale statefulset nginx-statefulset --replicas=3
statefulset.apps/nginx-statefulset scaled
ubuntu@ip-172-31-6-80:~$ kubectl get pods -w
NAME                  READY   STATUS    RESTARTS   AGE
dns-test              1/1     Running   0          18m
nginx-statefulset-0   1/1     Running   0          7m45s
nginx-statefulset-1   1/1     Running   0          3h6m
nginx-statefulset-2   1/1     Running   0          3h6m
ubuntu@ip-172-31-6-80:~$ kubectl get pvc
NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
nginx-storage-nginx-statefulset-0   Bound    pvc-7df8a34f-7c46-4a41-aba7-3413db02437e   100Mi      RWO            standard       <unset>                 3h6m
nginx-storage-nginx-statefulset-1   Bound    pvc-bb4c2fe8-30a7-4d74-9b05-57bf0cb77efc   100Mi      RWO            standard       <unset>                 3h6m
nginx-storage-nginx-statefulset-2   Bound    pvc-6e10936d-b840-4bed-8734-1c2f2ba431e7   100Mi      RWO            standard       <unset>                 3h6m
nginx-storage-nginx-statefulset-3   Bound    pvc-1cfc51f5-4dd5-46a0-9be0-dd9bc973596f   100Mi      RWO            standard       <unset>                 55s
nginx-storage-nginx-statefulset-4   Bound    pvc-88b8847d-ea32-4734-9f37-95fb172d610a   100Mi      RWO            standard       <unset>                 50s
ubuntu@ip-172-31-6-80:~$

```
### Verify: After scaling down, how many PVCs exist?
Verify: After scaling down to 3 replicas, 5 PVCs still exist because StatefulSet PVCs are retained by default when Pods are scaled down.

## Task 7: Clean Up

### Delete the StatefulSet and Headless Service, run kubectl get pvc to confirm that the PVCs are still present as a safety feature, and then manually delete the PVCs.

```bash

ubuntu@ip-172-31-6-80:~$ kubectl delete statefulset nginx-statefulset
statefulset.apps "nginx-statefulset" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl delete service nginx-headless
service "nginx-headless" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pvc
NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
nginx-storage-nginx-statefulset-0   Bound    pvc-7df8a34f-7c46-4a41-aba7-3413db02437e   100Mi      RWO            standard       <unset>                 3h10m
nginx-storage-nginx-statefulset-1   Bound    pvc-bb4c2fe8-30a7-4d74-9b05-57bf0cb77efc   100Mi      RWO            standard       <unset>                 3h10m
nginx-storage-nginx-statefulset-2   Bound    pvc-6e10936d-b840-4bed-8734-1c2f2ba431e7   100Mi      RWO            standard       <unset>                 3h10m
nginx-storage-nginx-statefulset-3   Bound    pvc-1cfc51f5-4dd5-46a0-9be0-dd9bc973596f   100Mi      RWO            standard       <unset>                 4m55s
nginx-storage-nginx-statefulset-4   Bound    pvc-88b8847d-ea32-4734-9f37-95fb172d610a   100Mi      RWO            standard       <unset>                 4m50s
ubuntu@ip-172-31-6-80:~$ kubectl delete pvc --all
persistentvolumeclaim "nginx-storage-nginx-statefulset-0" deleted from default namespace
persistentvolumeclaim "nginx-storage-nginx-statefulset-1" deleted from default namespace
persistentvolumeclaim "nginx-storage-nginx-statefulset-2" deleted from default namespace
persistentvolumeclaim "nginx-storage-nginx-statefulset-3" deleted from default namespace
persistentvolumeclaim "nginx-storage-nginx-statefulset-4" deleted from default namespace
ubuntu@ip-172-31-6-80:~$ kubectl get pvc
No resources found in default namespace.
ubuntu@ip-172-31-6-80:~$

```

### Verify: Were PVCs auto-deleted with the StatefulSet?
Verify: No, the PVCs were not automatically deleted with the StatefulSet; they remained until manually deleted.
