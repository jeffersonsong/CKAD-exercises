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
</p>
</details>

### Create the pod that was just described using YAML

<details><summary>show</summary>
<p>
</p>
</details>

### Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output

<details><summary>show</summary>
<p>
</p>
</details>

### Create a busybox pod (using YAML) that runs the command "env". Run it and see the output

<details><summary>show</summary>
<p>
</p>
</details>

### Get the YAML for a new namespace called 'myns' without creating it

<details><summary>show</summary>
<p>
</p>
</details>

### Create the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it

<details><summary>show</summary>
<p>
</p>
</details>

### Get pods on all namespaces

<details><summary>show</summary>
<p>
</p>
</details>

### Create a pod with image nginx called nginx and expose traffic on port 80

<details><summary>show</summary>
<p>
</p>
</details>

### Change pod's image to nginx:1.24.0. Observe that the container will be restarted as soon as the image gets pulled

<details><summary>show</summary>
<p>
</p>
</details>

### Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'

<details><summary>show</summary>
<p>
</p>
</details>

### Get pod's YAML

<details><summary>show</summary>
<p>
</p>
</details>

### Get information about the pod, including details about potential issues (e.g. pod hasn't started)

<details><summary>show</summary>
<p>
</p>
</details>

### Get pod logs

<details><summary>show</summary>
<p>
</p>
</details>

### If pod crashed and restarted, get logs about the previous instance

<details><summary>show</summary>
<p>
</p>
</details>

### Execute a simple shell on the nginx pod

<details><summary>show</summary>
<p>
</p>
</details>

### Create a busybox pod that echoes 'hello world' and then exits

<details><summary>show</summary>
<p>
</p>
</details>

### Do the same, but have the pod deleted automatically when it's completed

<details><summary>show</summary>
<p>
</p>
</details>

### Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod

<details><summary>show</summary>
<p>
</p>
</details>
