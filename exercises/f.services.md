![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/services&empty)
# Services and Networking (13%)

### Create a pod with image nginx called nginx and expose its port 80

<details><summary>show</summary>
<p>
```
k run nginx --image=nginx --port=80 --expose
```
</p>
</details>


### Confirm that ClusterIP has been created. Also check endpoints

<details><summary>show</summary>
<p>
```
k get svc
k get ep
```
</p>
</details>

### Get service's ClusterIP, create a temp busybox pod and 'hit' that IP with wget

<details><summary>show</summary>
<p>
```
k run temp --image=busybox -it --rm --restart=Never -- /bin/sh -c 'wget -O- 10.105.72.149'
```
</p>
</details>

### Convert the ClusterIP to NodePort for the same service and find the NodePort port. Hit service using Node's IP. Delete the service and the pod at the end.

<details><summary>show</summary>
<p>
```
k delete svc nginx
k expose pod nginx --type=NodePort
k get svc -o wide
k get node -o wide
curl http://192.168.49.2:30391
k get node minikube -o json | jq '.status.addresses[] | select (.type == "InternalIP").address'
```
</p>
</details>

### Create a deployment called foo using image 'dgkanatsios/simpleapp' (a simple server that returns hostname) and 3 replicas. Label it as 'app=foo'. Declare that containers in this pod will accept traffic on port 8080 (do NOT create a service yet)

<details><summary>show</summary>
<p>
```
k create deploy foo --image=dgkanatsios/simpleapp --replicas=3 --port 8080
k get deploy foo --show-labels
k label deploy foo --overwrite app=foo
```
</p>
</details>

### Get the pod IPs. Create a temp busybox pod and try hitting them on port 8080

<details><summary>show</summary>
<p>
```
k get pods -l app=foo -o wide
k run busybox --image=busybox -it --rm --restart=Never -- /bin/sh
/ # wget -O- http://10.0.0.125:8080
```
</p>
</details>

### Create a service that exposes the deployment on port 6262. Verify its existence, check the endpoints

<details><summary>show</summary>
<p>
```
k expose deploy foo --port=6262 --target-port=8080 --type=ClusterIP
k get svc foo
k get ep foo
```
</p>
</details>

### Create a temp busybox pod and connect via wget to foo service. Verify that each time there's a different hostname returned. Delete deployment and services to cleanup the cluster

<details><summary>show</summary>
<p>
```
k run busybox --image=busybox -it --rm --restart=Never -- /bin/sh
/ # wget -O- foo:6262
/ # wget -O- http://10.109.22.23:6262
```
</p>
</details>

### Create an nginx deployment of 2 replicas, expose it via a ClusterIP service on port 80. Create a NetworkPolicy so that only pods with labels 'access: granted' can access the deployment and apply it

kubernetes.io > Documentation > Concepts > Services, Load Balancing, and Networking > [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

> Note that network policies may not be enforced by default, depending on your k8s implementation. E.g. Azure AKS by default won't have policy enforcement, the cluster must be created with an explicit support for `netpol` https://docs.microsoft.com/en-us/azure/aks/use-network-policies#overview-of-network-policy  
  
<details><summary>show</summary>
<p>
```
k create deploy nginx --image=nginx --replicas=2 --port=80
k expose deploy nginx --port=80 --target-port=80 --type=ClusterIP
k run busybox --image=busybox -it --rm --restart=Never -- wget -O- nginx:80 --timeout 5
k run busybox --image=busybox -it --rm --restart=Never -l "access=granted" -- wget -O- nginx:80 --timeout 5
```
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: network-policy
spec:
  podSelector:
    matchLabels:
      app: nginx
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: granted
```
</p>
</details>
