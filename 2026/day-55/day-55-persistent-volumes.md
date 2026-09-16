# Day 55 – Persistent Volumes (PV) and Persistent Volume Claims (PVC)

## Task 1: See the Problem — Data Lost on Pod Deletion

### Write a Pod manifest that uses an emptyDir volume and writes a timestamped message to /data/message.txt, then apply the manifest and verify that the data exists using kubectl exec.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ batcat emptydir-demo.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: emptydir-demo.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Pod
   3   │ metadata:
   4   │   name: emptydir-demo
   5   │ spec:
   6   │   containers:
   7   │     - name: writer
   8   │       image: busybox:1.36
   9   │       command:
  10   │         - sh
  11   │         - -c
  12   │         - |
  13   │           echo "Message written at $(date)" > /data/message.txt
  14   │           sleep 3600
  15   │       volumeMounts:
  16   │         - name: data-volume
  17   │           mountPath: /data
  18   │
  19   │   volumes:
  20   │     - name: data-volume
  21   │       emptyDir: {}
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f emptydir-demo.yaml
pod/emptydir-demo created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec emptydir-demo -- cat /data/message.txt
Message written at Wed Sep 16 08:55:05 UTC 2026
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod emptydir-demo
NAME            READY   STATUS    RESTARTS   AGE
emptydir-demo   1/1     Running   0          3m38s

```
### Delete the Pod, recreate it, check the file again — the old message is gone

```bash
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod emptydir-demo
NAME            READY   STATUS    RESTARTS   AGE
emptydir-demo   1/1     Running   0          5m8s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete pod emptydir-demo
pod "emptydir-demo" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod
NAME       READY   STATUS    RESTARTS   AGE
dns-test   0/1     Unknown   0          4d21h
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f emptydir-demo.yaml
pod/emptydir-demo created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod
NAME            READY   STATUS    RESTARTS   AGE
dns-test        0/1     Unknown   0          4d21h
emptydir-demo   1/1     Running   0          22s
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec emptydir-demo -- cat /data/message.txt
Message written at Wed Sep 16 09:01:29 UTC 2026
ubuntu@ip-172-31-6-80:~/k8s$

```
### Verify: Is the timestamp the same or different after recreation?

After deleting and recreating the Pod, a new emptyDir volume is created, so /data/message.txt is recreated with a new timestamp.

## Task 2: Create a PersistentVolume (Static Provisioning)

### Write a PV manifest with `capacity: 1Gi`, `accessModes: ReadWriteOnce`, `persistentVolumeReclaimPolicy: Retain`, and `hostPath` pointing to `/tmp/k8s-pv-data`, then apply it and verify with `kubectl get pv` that its status is `Available`.

```bash
ubuntu@ip-172-31-6-80:~/k8s$ batcat pv.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: pv.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: PersistentVolume
   3   │ metadata:
   4   │   name: local-pv
   5   │ spec:
   6   │   capacity:
   7   │     storage: 1Gi
   8   │
   9   │   accessModes:
  10   │     - ReadWriteOnce
  11   │
  12   │   persistentVolumeReclaimPolicy: Retain
  13   │
  14   │   hostPath:
  15   │     path: /tmp/k8s-pv-data
  16   │     type: DirectoryOrCreate
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f pv.yaml
persistentvolume/local-pv created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
local-pv   1Gi        RWO            Retain           Available                          <unset>                          8s

```
### Verify: What is the STATUS of the PV?
```markdown
**Verify:** Check `kubectl get pv` and confirm that the PV STATUS is `Available`.
```

## Task 3: Create a PersistentVolumeClaim

### Write a PVC manifest requesting 500Mi of storage with ReadWriteOnce access, apply it, and verify using kubectl get pvc and kubectl get pv that both resources show a Bound status, confirming that Kubernetes successfully matched the PVC with the PV based on the required capacity and access mode.

```bash
ubuntu@ip-172-31-6-80:~/k8s$ batcat pvc.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: pvc.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: PersistentVolumeClaim
   3   │ metadata:
   4   │   name: my-pvc
   5   │ spec:
   6   │   storageClassName: ""
   7   │   accessModes:
   8   │     - ReadWriteOnce
   9   │   resources:
  10   │     requests:
  11   │       storage: 500Mi
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f pvc.yaml
persistentvolumeclaim/my-pvc created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM            STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
local-pv   1Gi        RWO            Retain           Bound    default/my-pvc                  <unset>                          13m
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pvc
NAME     STATUS   VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
my-pvc   Bound    local-pv   1Gi        RWO                           <unset>                 19s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe pvc my-pvc
Name:          my-pvc
Namespace:     default
StorageClass:
Status:        Bound
Volume:        local-pv
Labels:        <none>
Annotations:   pv.kubernetes.io/bind-completed: yes
               pv.kubernetes.io/bound-by-controller: yes
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      1Gi
Access Modes:  RWO
VolumeMode:    Filesystem
Used By:       <none>
Conditions:
  Type     Status  LastProbeTime                     LastTransitionTime                Reason           Message
  ----     ------  -----------------                 ------------------                ------           -------
  Unused   True    Mon, 01 Jan 0001 00:00:00 +0000   Wed, 16 Sep 2026 09:21:14 +0000   NoPodsUsingPVC   No pods are currently referencing this PVC
Events:    <none>
```
### Verify: What does the VOLUME column in kubectl get pvc show?
**Verify:** The `VOLUME` column in `kubectl get pvc` shows `local-pv`, confirming that the PVC is bound to the `local-pv` PersistentVolume.

## Task 4: Use the PVC in a Pod — Data That Survives

### Write a Pod manifest that mounts the PVC at /data using persistentVolumeClaim.claimName

```bash
ubuntu@ip-172-31-6-80:~/k8s$ batcat pvc-pod.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: pvc-pod.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: Pod
   3   │ metadata:
   4   │   name: pvc-pod
   5   │ spec:
   6   │   containers:
   7   │     - name: writer
   8   │       image: busybox:1.36
   9   │       command:
  10   │         - sh
  11   │         - -c
  12   │         - |
  13   │           echo "Data written by Pod at $(date)" >> /data/message.txt
  14   │           sleep 3600
  15   │       volumeMounts:
  16   │         - name: persistent-storage
  17   │           mountPath: /data
  18   │
  19   │   volumes:
  20   │     - name: persistent-storage
  21   │       persistentVolumeClaim:
  22   │         claimName: my-pvc
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f pvc-pod.yaml
pod/pvc-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod pvc-pod
NAME      READY   STATUS    RESTARTS   AGE
pvc-pod   1/1     Running   0          9s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod pvc-pod
NAME      READY   STATUS    RESTARTS   AGE
pvc-pod   1/1     Running   0          17s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec pvc-pod -- cat /data/message.txt
Data written by Pod at Wed Sep 16 09:41:17 UTC 2026
ubuntu@ip-172-31-6-80:~/k8s$
```

### Write data to /data/message.txt, then delete and recreate the Pod

```bash
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f pvc-pod.yaml
pod/pvc-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec pvc-pod -- cat /data/message.txt
Data written by Pod at Wed Sep 16 09:41:17 UTC 2026
Data written by Pod at Wed Sep 16 10:45:49 UTC 2026
ubuntu@ip-172-31-6-80:~/k8s$

```
**Verify:** Check `/data/message.txt` using `kubectl exec` and confirm that it contains data from both Pods, proving that the PVC persists data after Pod recreation.

## Task 5: StorageClasses and Dynamic Provisioning

### 1. Run `kubectl get storageclass` and `kubectl describe storageclass` to note the provisioner, reclaim policy, and volume binding mode.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl get storageclass
NAME                 PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
standard (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  9d
ubuntu@ip-172-31-6-80:~/k8s$ kubectl describe storageclass
Name:            standard
IsDefaultClass:  Yes
Annotations:     kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"storage.k8s.io/v1","kind":"StorageClass","metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"true"},"name":"standard"},"provisioner":"rancher.io/local-path","reclaimPolicy":"Delete","volumeBindingMode":"WaitForFirstConsumer"}
,storageclass.kubernetes.io/is-default-class=true
Provisioner:           rancher.io/local-path
Parameters:            <none>
AllowVolumeExpansion:  <unset>
MountOptions:          <none>
ReclaimPolicy:         Delete
VolumeBindingMode:     WaitForFirstConsumer
Events:                <none>
ubuntu@ip-172-31-6-80:~/k8s$

```
### With dynamic provisioning, developers only create PVCs — the StorageClass handles PV creation automatically

```bash

ubuntu@ip-172-31-6-80:~/k8s$ batcat dynamic-pvc.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: dynamic-pvc.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: PersistentVolumeClaim
   3   │ metadata:
   4   │   name: dynamic-pvc
   5   │ spec:
   6   │   accessModes:
   7   │     - ReadWriteOnce
   8   │   resources:
   9   │     requests:
  10   │       storage: 1Gi
  11   │   storageClassName: standard
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f dynamic-pvc.yaml
persistentvolumeclaim/dynamic-pvc created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pvc
NAME          STATUS    VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
dynamic-pvc   Pending                                        standard       <unset>                 8s
my-pvc        Bound     local-pv   1Gi        RWO                           <unset>                 91m
ubuntu@ip-172-31-6-80:~/k8s$

```
### Verify: What is the default StorageClass in your cluster?
**Verify:** Run `kubectl get storageclass` and identify the StorageClass marked as `(default)` in your cluster.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ kubectl get storageclass
NAME                 PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
standard (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  9d
ubuntu@ip-172-31-6-80:~/k8s$

```

## Task 6: Dynamic Provisioning

### Write a PVC manifest with `storageClassName: standard` (or your cluster's default), apply it and verify that a PV is automatically created with `kubectl get pv`, then use the PVC in a Pod to write data and confirm that it works.

```bash

ubuntu@ip-172-31-6-80:~/k8s$ batcat dynamic-pvc.yaml
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: dynamic-pvc.yaml
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ apiVersion: v1
   2   │ kind: PersistentVolumeClaim
   3   │ metadata:
   4   │   name: dynamic-pvc
   5   │ spec:
   6   │   storageClassName: standard
   7   │   accessModes:
   8   │     - ReadWriteOnce
   9   │   resources:
  10   │     requests:
  11   │       storage: 1Gi
───────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f dynamic-pvc.yaml
persistentvolumeclaim/dynamic-pvc unchanged
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pvc
kubectl get pv
NAME          STATUS    VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
dynamic-pvc   Pending                                        standard       <unset>                 42m
my-pvc        Bound     local-pv   1Gi        RWO                           <unset>                 134m
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM            STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
local-pv   1Gi        RWO            Retain           Bound    default/my-pvc                  <unset>                          147m
ubuntu@ip-172-31-6-80:~/k8s$ vim dynamic-pod.yaml
ubuntu@ip-172-31-6-80:~/k8s$ kubectl apply -f dynamic-pod.yaml
pod/dynamic-pod created
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pod dynamic-pod
kubectl get pvc
kubectl get pv
NAME          READY   STATUS    RESTARTS   AGE
dynamic-pod   1/1     Running   0          8s
NAME          STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
dynamic-pvc   Bound    pvc-46d07fb8-23ca-40af-8672-ae29d01e37be   1Gi        RWO            standard       <unset>                 44m
my-pvc        Bound    local-pv                                   1Gi        RWO                           <unset>                 135m
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
local-pv                                   1Gi        RWO            Retain           Bound    default/my-pvc                       <unset>                          149m
pvc-46d07fb8-23ca-40af-8672-ae29d01e37be   1Gi        RWO            Delete           Bound    default/dynamic-pvc   standard       <unset>                          5s
ubuntu@ip-172-31-6-80:~/k8s$ kubectl exec dynamic-pod -- cat /data/message.txt
Dynamic provisioning test - Wed Sep 16 11:37:01 UTC 2026
ubuntu@ip-172-31-6-80:~/k8s$

```

**Verify:** How many PVs exist now? Identify which PV was created manually and which one was dynamically provisioned by the StorageClass.

**Answer:** There are **2 PVs** in the cluster. `local-pv` was created manually, while `pvc-46d07fb8-23ca-40af-8672-ae29d01e37be` was dynamically provisioned by the `standard` StorageClass.

## Task 7: Clean Up

### Delete all Pods first, then delete the PVCs and check `kubectl get pv` to observe that the dynamic PV is automatically deleted due to its `Delete` reclaim policy while the manual PV remains in `Released` status due to its `Retain` policy, and finally delete the remaining PV manually.

```bash
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete pod --all
pod "dns-test" deleted from default namespace
pod "dynamic-pod" deleted from default namespace
pod "emptydir-demo" deleted from default namespace
pod "pvc-pod" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pods
No resources found in default namespace.
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete pvc --all
persistentvolumeclaim "dynamic-pvc" deleted from default namespace
persistentvolumeclaim "my-pvc" deleted from default namespace
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS     CLAIM            STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
local-pv   1Gi        RWO            Retain           Released   default/my-pvc                  <unset>                          155m
ubuntu@ip-172-31-6-80:~/k8s$ kubectl delete pv local-pv
persistentvolume "local-pv" deleted
ubuntu@ip-172-31-6-80:~/k8s$ kubectl get pv
No resources found
ubuntu@ip-172-31-6-80:~/k8s$

```
## Verify: Which PV was auto-deleted and which was retained? Why?

**Answer:** The dynamically provisioned PV was auto-deleted because it had the `Delete` reclaim policy, while the manually created `local-pv` was retained because it had the `Retain` reclaim policy.

