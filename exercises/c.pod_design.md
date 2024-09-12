![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/pod_design&empty)
# Pod design (20%)

[Labels And Annotations](#labels-and-annotations)

[Deployments](#deployments)

[Jobs](#jobs)

[Cron Jobs](#cron-jobs)

## Labels and Annotations
kubernetes.io > Documentation > Concepts > Overview > Working with Kubernetes Objects > [Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)

### Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1

<details><summary>show</summary>
<p>
```
kubectl run nginx1 --image=nginx --labels="app=v1"
kubectl run nginx2 --image=nginx --labels="app=v1"
kubectl run nginx3 --image=nginx --labels="app=v1"
```
</p>
</details>

### Show all labels of the pods

<details><summary>show</summary>
<p>
```
k get pod --show-labels
NAME     READY   STATUS    RESTARTS   AGE   LABELS
nginx1   1/1     Running   0          75s   app=v1
nginx2   1/1     Running   0          50s   app=v1
nginx3   1/1     Running   0          44s   app=v1
```
</p>
</details>

### Change the labels of pod 'nginx2' to be app=v2

<details><summary>show</summary>
<p>
```
k label pod nginx2 --overwrite app=v2
```
</p>
</details>

### Get the label 'app' for the pods (show a column with APP labels)

<details><summary>show</summary>
<p>
```
k get pod -L app   
```
</p>
</details>

### Get only the 'app=v2' pods

<details><summary>show</summary>
<p>
```
k get pod --selector="app=v2"
NAME     READY   STATUS    RESTARTS   AGE
nginx2   1/1     Running   0          4m53s
```
</p>
</details>

### Add a new label tier=web to all pods having 'app=v2' or 'app=v1' labels

<details><summary>show</summary>
<p>
```
k label pods --selector="app in (v1, v2)" tier=web
```
</p>
</details>


### Add an annotation 'owner: marketing' to all pods having 'app=v2' label

<details><summary>show</summary>
<p>
```
k annotate pods --selector="app=v2" owner=marketing
```
</p>
</details>

### Remove the 'app' label from the pods we created before

<details><summary>show</summary>
<p>
```
k label pods nginx1 nginx2 nginx3 app-
```
</p>
</details>

### Annotate pods nginx1, nginx2, nginx3 with "description='my description'" value

<details><summary>show</summary>
<p>
```
k annotate pods nginx1 nginx2 nginx3 description='my description'
```
</p>
</details>

### Check the annotations for pod nginx1

<details><summary>show</summary>
<p>
```
k describe pod nginx1 | grep -i "Annotations:"
```
</p>
</details>

### Remove the annotations for these three pods

<details><summary>show</summary>
<p>
```
k annotate pod nginx1 nginx2 nginx3 description- owner-
```
</p>
</details>

### Remove these pods to have a clean state in your cluster

<details><summary>show</summary>
<p>
```
k delete pods nginx1 nginx2 nginx3
```
</p>
</details>

## Pod Placement

### Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'

<details><summary>show</summary>
<p>
[Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  nodeSelector:
    accelerator: nvidia-tesla-p100
```
</p>
</details>

### Taint a node with key `tier` and value `frontend` with the effect `NoSchedule`. Then, create a pod that tolerates this taint.

<details><summary>show</summary>
<p>
```
k taint nodes node01 tier=frontend:NoSchedule

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
  tolerations:
  - key: "tier"
    operator: "Equal"
    value: "frontend"
    effect: "NoSchedule"
```
```
k apply -f pod.yaml
```
</p>
</details>

### Create a pod that will be placed on node `controlplane`. Use nodeSelector and tolerations.

<details><summary>show</summary>
<p>
```
k describe node controlplane
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
  nodeName: controlplane
  tolerations:
  - key: node-role.kubernetes.io/control-plane
    operator: "Exists"
    effect: NoSchedule
```
</p>
</details>

## Deployments

kubernetes.io > Documentation > Concepts > Workloads > Workload Resources > [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment)

### Create a deployment with image nginx:1.18.0, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

<details><summary>show</summary>
<p>
```
k create deploy nginx --image=nginx:1.18.0 --replicas=2 --port=80
```
</p>
</details>

### View the YAML of this deployment

<details><summary>show</summary>
<p>
```
k get deploy nginx -o yaml
```
</p>
</details>

### View the YAML of the replica set that was created by this deployment

<details><summary>show</summary>
<p>
```
k get rs nginx-dbdf9c499 -o yaml
```
</p>
</details>

### Get the YAML for one of the pods

<details><summary>show</summary>
<p>
```
k get pod nginx-dbdf9c499-d29gc -o yaml
```
</p>
</details>

### Check how the deployment rollout is going

<details><summary>show</summary>
<p>
```
k rollout status deploy/nginx
```
</p>
</details>

### Update the nginx image to nginx:1.19.8

<details><summary>show</summary>
<p>
```
k set image deploy/nginx nginx=nginx:1.19.8
```
</p>
</details>

### Check the rollout history and confirm that the replicas are OK

<details><summary>show</summary>
<p>
```
k rollout history deploy/nginx
```
</p>
</details>

### Undo the latest rollout and verify that new pods have the old image (nginx:1.18.0)

<details><summary>show</summary>
<p>
```
k rollout undo deploy/nginx --to-revision=1
k describe deploy nginx | grep Image
```
</p>
</details>

### Do an on purpose update of the deployment with a wrong image nginx:1.91

<details><summary>show</summary>
<p>
```
k set image deploy/nginx nginx=nginx:1.91
```
</p>
</details>

### Verify that something's wrong with the rollout

<details><summary>show</summary>
<p>
```
k rollout status deploy/nginx

k get pod
NAME                     READY   STATUS             RESTARTS   AGE
nginx-79566b9f54-9zg86   0/1     ImagePullBackOff   0          2m17s
```
</p>
</details>


### Return the deployment to the second revision (number 2) and verify the image is nginx:1.19.8

<details><summary>show</summary>
<p>
```
k rollout undo deploy/nginx --to-revision=2
k describe deploy nginx | grep Image
```
</p>
</details>

### Check the details of the fourth revision (number 4)

<details><summary>show</summary>
<p>
```
k rollout history deploy/nginx --revision=4
```
</p>
</details>

### Scale the deployment to 5 replicas

<details><summary>show</summary>
<p>
```
k scale deploy/nginx --replicas=5
```
</p>
</details>

### Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%

<details><summary>show</summary>
<p>
```
k autoscale deploy/nginx --min=5 --max=10 --cpu-percent=80
```
</p>
</details>

### Pause the rollout of the deployment

<details><summary>show</summary>
<p>
```
k rollout pause deploy/nginx
```
</p>
</details>

### Update the image to nginx:1.19.9 and check that there's nothing going on, since we paused the rollout

<details><summary>show</summary>
<p>
```
k set image deploy/nginx nginx=nginx:1.19.9
k rollout status deploy/nginx
```
</p>
</details>

### Resume the rollout and check that the nginx:1.19.9 image has been applied

<details><summary>show</summary>
<p>
```
k rollout resume deploy/nginx
k rollout status deploy/nginx
```
</p>
</details>

### Delete the deployment and the horizontal pod autoscaler you created

<details><summary>show</summary>
<p>
```
k delete hpa/nginx
k delete deploy/nginx
```
</p>
</details>

### Implement canary deployment by running two instances of nginx marked as version=v1 and version=v2 so that the load is balanced at 75%-25% ratio

<details><summary>show</summary>
<p>
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      version: v1
  template:
    metadata:
      labels:
        app: nginx
        version: v1
    spec:
      containers:
      - image: nginx
        name: nginx
```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-green
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
      version: v2
  template:
    metadata:
      labels:
        app: nginx
        version: v2
    spec:
      containers:
      - image: nginx
        name: nginx
```
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  type: ClusterIP
```
```
k delete svc/nginx deploy/nginx-blue deploy/nginx-green
```
</p>
</details>

## Jobs

### Create a job named pi with image perl:5.34 that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

<details><summary>show</summary>
<p>
[Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
```
k create job pi --image=perl:5.34 -- perl -Mbignum=bpi -wle 'print bpi(2000)' 
```
</p>
</details>

### Wait till it's done, get the output

<details><summary>show</summary>
<p>
```
k logs pi-6cldc
```
</p>
</details>

### Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'

<details><summary>show</summary>
<p>
```
k create job busybox-job --image=busybox -- /bin/sh -c "echo hello;sleep 30;echo world"
```
</p>
</details>

### Follow the logs for the pod (you'll wait for 30 seconds)

<details><summary>show</summary>
<p>
```
k logs busybox-job-dgkn5
```
</p>
</details>

### See the status of the job, describe it and see the logs

<details><summary>show</summary>
<p>
```
k get job busybox-job
k describe job busybox-job
k logs job/busybox-job
```
</p>
</details>

### Delete the job

<details><summary>show</summary>
<p>
```
k delete job/busybox-job
```
</p>
</details>

### Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

<details><summary>show</summary>
<p>
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: busybox
spec:
  activeDeadlineSeconds: 30
  template:
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - sleep 35 && echo 'hello world'
        image: busybox
        name: busybox
      restartPolicy: Never
```
</p>
</details>

### Create the same job, make it run 5 times, one after the other. Verify its status and delete it

<details><summary>show</summary>
<p>
```
apiVersion: batch/v1
kind: Job
metadata:
  name: busybox
spec:
  completions: 5
  template:
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - sleep 5 && echo 'hello world'
        image: busybox
        name: busybox
      restartPolicy: Never
```
</p>
</details>

### Create the same job, but make it run 5 parallel times

<details><summary>show</summary>
<p>
```
apiVersion: batch/v1
kind: Job
metadata:
  name: busybox
spec:
  completions: 5
  parallelism: 5
  template:
    spec:
      containers:
      - command:
        - /bin/sh
        - -c
        - sleep 5 && echo 'hello world'
        image: busybox
        name: busybox
      restartPolicy: Never
```
</p>
</details>

## Cron jobs

kubernetes.io > Documentation > Tasks > Run Jobs > [Running Automated Tasks with a CronJob](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)

### Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output

<details><summary>show</summary>
<p>
```
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c 'date; echo "Hello from the Kubernetes cluster"'
```
</p>
</details>

### See its logs and delete it

<details><summary>show</summary>
<p>
```
k logs job/my-job-28769701
k delete cj my-job
```
</p>
</details>

### Create the same cron job again, and watch the status. Once it ran, check which job ran by the created cron job. Check the log, and delete the cron job

<details><summary>show</summary>
<p>
```
k get cj
k get job -w
k logs job/my-job-28769706
k delete cj my-job
```
</p>
</details>

### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to start execution after its scheduled time (i.e. the job missed its scheduled time).

<details><summary>show</summary>
<p>
[CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)
```
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" --dry-run=client -o yaml -- /bin/sh -c 'date; echo "Hello from the Kubernetes cluster"' > my-job.yaml
```
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-job
spec:
  startingDeadlineSeconds: 17
  jobTemplate:
    metadata:
      name: my-job
    spec:
      template:
        spec:
          containers:
          - command:
            - /bin/sh
            - -c
            - date; echo "Hello from the Kubernetes cluster"
            image: busybox
            name: my-job
          restartPolicy: OnFailure
  schedule: '*/1 * * * *'
```
</p>
</details>

### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it successfully starts but takes more than 12 seconds to complete execution.

<details><summary>show</summary>
<p>
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-job
spec:
  jobTemplate:
    metadata:
      name: my-job
    spec:
      activeDeadlineSeconds: 12
      template:
        spec:
          containers:
          - command:
            - /bin/sh
            - -c
            - date; echo "Hello from the Kubernetes cluster"
            image: busybox
            name: my-job
          restartPolicy: OnFailure
  schedule: '*/1 * * * *'
```
</p>
</details>

### Create a job from cronjob.

<details><summary>show</summary>
<p>
```
kubectl create job test-job --from=cj/my-job
```
</p>
</details>
