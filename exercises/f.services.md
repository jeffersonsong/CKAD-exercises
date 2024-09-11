![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/services&empty)
# Services and Networking (13%)

### Create a pod with image nginx called nginx and expose its port 80

<details><summary>show</summary>
<p>
</p>
</details>


### Confirm that ClusterIP has been created. Also check endpoints

<details><summary>show</summary>
<p>
</p>
</details>

### Get service's ClusterIP, create a temp busybox pod and 'hit' that IP with wget

<details><summary>show</summary>
<p>
</p>
or
<p>
</p>
</details>

### Convert the ClusterIP to NodePort for the same service and find the NodePort port. Hit service using Node's IP. Delete the service and the pod at the end.

<details><summary>show</summary>
<p>
</p>
</details>

### Create a deployment called foo using image 'dgkanatsios/simpleapp' (a simple server that returns hostname) and 3 replicas. Label it as 'app=foo'. Declare that containers in this pod will accept traffic on port 8080 (do NOT create a service yet)

<details><summary>show</summary>
<p>
</p>
</details>

### Get the pod IPs. Create a temp busybox pod and try hitting them on port 8080

<details><summary>show</summary>
<p>
</p>
</details>

### Create a service that exposes the deployment on port 6262. Verify its existence, check the endpoints

<details><summary>show</summary>
<p>
</p>
</details>

### Create a temp busybox pod and connect via wget to foo service. Verify that each time there's a different hostname returned. Delete deployment and services to cleanup the cluster

<details><summary>show</summary>
<p>
</p>
</details>

### Create an nginx deployment of 2 replicas, expose it via a ClusterIP service on port 80. Create a NetworkPolicy so that only pods with labels 'access: granted' can access the deployment and apply it

kubernetes.io > Documentation > Concepts > Services, Load Balancing, and Networking > [Network Policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

> Note that network policies may not be enforced by default, depending on your k8s implementation. E.g. Azure AKS by default won't have policy enforcement, the cluster must be created with an explicit support for `netpol` https://docs.microsoft.com/en-us/azure/aks/use-network-policies#overview-of-network-policy  
  
<details><summary>show</summary>
<p>
</p>
</details>
