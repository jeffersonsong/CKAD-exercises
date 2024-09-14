![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/state&empty)
# State Persistence (8%)

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure a Pod to Use a Volume for Storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/)

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure a Pod to Use a PersistentVolume for Storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

## Define volumes 

### Create busybox pod with two containers, each one will have the image busybox and will run the 'sleep 3600' command. Make both containers mount an emptyDir at '/etc/foo'. Connect to the second busybox, write the first column of '/etc/passwd' file to '/etc/foo/passwd'. Connect to the first busybox and write '/etc/foo/passwd' file to standard output. Delete pod.

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: busybox
  name: busybox
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox1
    volumeMounts:
    - mountPath: /etc/foo
      name: vol
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox2
    volumeMounts:
    - mountPath: /etc/foo
      name: vol
  volumes:
  - name: vol
    emptyDir: {}
  restartPolicy: Never
```
```
k exec -it busybox -c busybox2 -- /bin/sh
/ # cut -f1 -d':' /etc/passwd  > /etc/foo/passwd

k exec -it busybox -c busybox1 -- /bin/sh
/ # cat /etc/foo/passwd
```
</p>
</details>


### Create a PersistentVolume of 10Gi, called 'myvolume'. Make it have accessMode of 'ReadWriteOnce' and 'ReadWriteMany', storageClassName 'normal', mounted on hostPath '/etc/foo'. Save it on pv.yaml, add it to the cluster. Show the PersistentVolumes that exist on the cluster

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: myvolume
spec:
  storageClassName: normal
  capacity:
    storage: 10Gi
  accessModes:
  - ReadWriteOnce
  - ReadWriteMany
  hostPath:
    path: /etc/foo
```
```
k get pv myvolume
```
</p>
</details>

### Create a PersistentVolumeClaim for this storage class, called 'mypvc', a request of 4Gi and an accessMode of ReadWriteOnce, with the storageClassName of normal, and save it on pvc.yaml. Create it on the cluster. Show the PersistentVolumeClaims of the cluster. Show the PersistentVolumes of the cluster

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypvc
spec:
  volumeName: myvolume
  storageClassName: normal
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 4Gi
```
```
k get pv
```
</p>
</details>

### Create a busybox pod with command 'sleep 3600', save it on pod.yaml. Mount the PersistentVolumeClaim to '/etc/foo'. Connect to the 'busybox' pod, and copy the '/etc/passwd' file to '/etc/foo/passwd'

<details><summary>show</summary>
<p>
```
k run busybox --image=busybox --dry-run=client -o yaml -- /bin/sh -c 'sleep 3600' > pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - sleep 3600
    image: busybox
    name: busybox
    volumeMounts:
    - mountPath: /etc/foo
      name: vol
  volumes:
  - name: vol
    persistentVolumeClaim:
      claimName: mypvc
```
```
k exec -it busybox -- /bin/sh
/ # cp /etc/passwd /etc/foo/passwd
/ # cat /etc/foo/passwd
```

</p>
</details>

### Create a second pod which is identical with the one you just created (you can easily do it by changing the 'name' property on pod.yaml). Connect to it and verify that '/etc/foo' contains the 'passwd' file. Delete pods to cleanup. Note: If you can't see the file from the second pod, can you figure out why? What would you do to fix that?



<details><summary>show</summary>
<p>
```
k exec -it busybox2 -- /bin/sh
/ # cat /etc/foo/passwd
```
</p>
</details>

### Create a busybox pod with 'sleep 3600' as arguments. Copy '/etc/passwd' from the pod to your local folder

<details><summary>show</summary>
<p>
```
k run busybox --image=busybox -- /bin/sh -c 'sleep 3600'
k cp busybox:/etc/passwd passwd
cat passwd
```
</p>
</details>
