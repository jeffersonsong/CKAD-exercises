![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/core_concepts&empty)
# Core Concepts (13%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Configure Access to Multiple Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Accessing Clusters](https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/) using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > [Use Port Forwarding to Access Applications in a Cluster](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)

### Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace

<details><summary>show</summary>
<p>
```
k create ns mynamespace
k run nginx --image=nginx -n mynamespace
```
</p>
</details>

### Create the pod that was just described using YAML

<details><summary>show</summary>
<p>
```
k run nginx --image=nginx -n mynamespace --dry-run=client -o yaml > pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
```
```
k apply -f pod.yaml
```
</p>
</details>

### Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output

<details><summary>show</summary>
<p>
```
k run temp --image=busybox -it --rm --restart=Never --command -- env
```
</p>
</details>

### Create a busybox pod (using YAML) that runs the command "env". Run it and see the output

<details><summary>show</summary>
<p>
```
k run temp --image=busybox --restart=Never --dry-run=client -o yaml --command -- env > pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: temp
  name: temp
spec:
  containers:
  - command:
    - env
    image: busybox
    name: temp
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
```
k apply -f pod.yaml 

k logs temp
```
</p>
</details>

### Get the YAML for a new namespace called 'myns' without creating it

<details><summary>show</summary>
<p>
```
k create ns myns --dry-run=client -o yaml
```
</p>
</details>

### Create the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it

<details><summary>show</summary>
<p>
```
kubectl create quota myrq --hard=cpu=1,memory=1Gi,pods=2 --dry-run=client -o yaml
```
</p>
</details>

### Get pods on all namespaces

<details><summary>show</summary>
<p>
```
k get pod -A
```
</p>
</details>

### Create a pod with image nginx called nginx and expose traffic on port 80

<details><summary>show</summary>
<p>
```
k run nginx --image=nginx --port=80
```
</p>
</details>

### Change pod's image to nginx:1.24.0. Observe that the container will be restarted as soon as the image gets pulled

<details><summary>show</summary>
<p>
```
k set image pod/nginx nginx=1.24.0
```
</p>
</details>

### Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'

<details><summary>show</summary>
<p>
```
k get pod -o wide
NAME    READY   STATUS    RESTARTS      AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   1 (29s ago)   57s   10.0.0.156   minikube   <none>           <none>

k run temp --image=busybox -it --rm --restart=Never --command -- wget -O- http://10.0.0.156
```
</p>
</details>

### Get pod's YAML

<details><summary>show</summary>
<p>
```
k get pod nginx -o yaml
```
</p>
</details>

### Get information about the pod, including details about potential issues (e.g. pod hasn't started)

<details><summary>show</summary>
<p>
```
k describe pod nginx
```
</p>
</details>

### Get pod logs

<details><summary>show</summary>
<p>
```
k logs nginx
```
</p>
</details>

### If pod crashed and restarted, get logs about the previous instance

<details><summary>show</summary>
<p>
```
k logs -p nginx
```
</p>
</details>

### Execute a simple shell on the nginx pod

<details><summary>show</summary>
<p>
```
k exec -it nginx -- /bin/sh
```
</p>
</details>

### Create a busybox pod that echoes 'hello world' and then exits

<details><summary>show</summary>
<p>
```
k run temp --image=busybox -it --restart=Never -- echo "hello world"
hello world
```
</p>
</details>

### Do the same, but have the pod deleted automatically when it's completed

<details><summary>show</summary>
<p>
```
k run temp --image=busybox -it --rm --restart=Never -- echo "hello world"
hello world
pod "temp" deleted
```
</p>
</details>

### Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod

<details><summary>show</summary>
<p>
```
k run nginx --image=nginx --env="var1=val1" --restart=Never
pod/nginx created

k exec -it nginx -- /bin/sh
# echo $var1
val1
# env | grep var1
var1=val1
```
</p>
</details>
