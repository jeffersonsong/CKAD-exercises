![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/multi_container&empty)
# Multi-container Pods (10%)

### Create a Pod with two containers, both with image busybox and command "echo hello; sleep 3600". Connect to the second container and run 'ls'

<details><summary>show</summary>
<p>
```
k run container1 --image=busybox --dry-run=client -o yaml --command -- /bin/sh -c 'echo hello; sleep 3600' > pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-containers
spec:
  containers:
  - command:
    - /bin/sh
    - -c
    - echo hello; sleep 3600
    image: busybox
    name: container1
  - command:
    - /bin/sh
    - -c
    - echo hello; sleep 3600
    image: busybox
    name: container2
  restartPolicy: Never
```
```
k apply -f pod.yaml

k exec -it multi-containers -c container2 -- ls          
bin    etc    lib    proc   sys    usr
dev    home   lib64  root   tmp    var
```
</p>
</details>

### Create a pod with an nginx container exposed on port 80. Add a busybox init container which downloads a page using "wget -O /work-dir/index.html http://neverssl.com/online". Make a volume of type emptyDir and mount it in both containers. For the nginx container, mount it on "/usr/share/nginx/html" and for the initcontainer, mount it on "/work-dir". When done, get the IP of the created pod and create a busybox pod and run "wget -O- IP"

<details><summary>show</summary>
<p>
```
k run nginx --image=nginx --port=80 --dry-run=client -o yaml> pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  containers:
  - image: nginx
    name: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: vol
  initContainers:
  - image: busybox
    name: initializer
    volumeMounts:
    - mountPath: /work-dir
      name: vol
    command:
    - /bin/sh
    - -c
    - "wget -O /work-dir/index.html http://neverssl.com/online"
  volumes:
  - name: vol
    emptyDir: {}
  restartPolicy: Never
```
```
k get pod -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP          NODE       NOMINATED NODE   READINESS GATES
myapp   1/1     Running   0          10s   10.0.0.90   minikube   <none>           <none>

k run temp --image=busybox -it --rm --restart=Never --command -- wget -O- 10.0.0.90
```
</p>
</details>

