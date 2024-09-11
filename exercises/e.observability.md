![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/observability&empty)
# Observability (18%)

## Liveness, readiness and startup probes

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

### Create an nginx pod with a liveness probe that just runs the command 'ls'. Save its YAML in pod.yaml. Run it, check its probe status, delete it.

<details><summary>show</summary>
<p>
</p>
</details>

### Modify the pod.yaml file so that liveness probe starts kicking in after 5 seconds whereas the interval between probes would be 5 seconds. Run it, check the probe, delete it.

<details><summary>show</summary>
<p>
</p>
</details>

### Create an nginx pod (that includes port 80) with an HTTP readinessProbe on path '/' on port 80. Again, run it, check the readinessProbe, delete it.

<details><summary>show</summary>
<p>
</p>
</details>

### Lots of pods are running in `qa`,`alan`,`test`,`production` namespaces.  All of these pods are configured with liveness probe.  Please list all pods whose liveness probe are failed in the format of `<namespace>/<pod name>` per line.

<details><summary>show</summary>
<p>
</p>
</details>

## Logging

### Create a busybox pod that runs `i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done`. Check its logs

<details><summary>show</summary>
<p>
</p>
</details>

## Debugging

### Create a busybox pod that runs 'ls /notexist'. Determine if there's an error (of course there is), see it. In the end, delete the pod

<details><summary>show</summary>
<p>
</p>
</details>

### Create a busybox pod that runs 'notexist'. Determine if there's an error (of course there is), see it. In the end, delete the pod forcefully with a 0 grace period

<details><summary>show</summary>
<p>
</p>
</details>


### Get CPU/memory utilization for nodes ([metrics-server](https://github.com/kubernetes-incubator/metrics-server) must be running)

<details><summary>show</summary>
<p>
</p>
</details>
