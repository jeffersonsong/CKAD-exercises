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
</p>
</details>

### Show all labels of the pods

<details><summary>show</summary>
<p>
</p>
</details>

### Change the labels of pod 'nginx2' to be app=v2

<details><summary>show</summary>
<p>
</p>
</details>

### Get the label 'app' for the pods (show a column with APP labels)

<details><summary>show</summary>
<p>
</p>
</details>

### Get only the 'app=v2' pods

<details><summary>show</summary>
<p>
</p>
</details>

### Add a new label tier=web to all pods having 'app=v2' or 'app=v1' labels

<details><summary>show</summary>
<p>
</p>
</details>


### Add an annotation 'owner: marketing' to all pods having 'app=v2' label

<details><summary>show</summary>
<p>
</p>
</details>

### Remove the 'app' label from the pods we created before

<details><summary>show</summary>
<p>
</p>
</details>

### Annotate pods nginx1, nginx2, nginx3 with "description='my description'" value

<details><summary>show</summary>
<p>
</p>
</details>

### Check the annotations for pod nginx1

<details><summary>show</summary>
<p>
</p>
</details>

### Remove the annotations for these three pods

<details><summary>show</summary>
<p>
</p>
</details>

### Remove these pods to have a clean state in your cluster

<details><summary>show</summary>
<p>
</p>
</details>

## Pod Placement

### Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'

<details><summary>show</summary>
<p>
</p>
</details>

### Taint a node with key `tier` and value `frontend` with the effect `NoSchedule`. Then, create a pod that tolerates this taint.

<details><summary>show</summary>
<p>
</p>
</details>

### Create a pod that will be placed on node `controlplane`. Use nodeSelector and tolerations.

<details><summary>show</summary>
<p>
</p>
</details>

## Deployments

kubernetes.io > Documentation > Concepts > Workloads > Workload Resources > [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment)

### Create a deployment with image nginx:1.18.0, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

<details><summary>show</summary>
<p>
</p>
</details>

### View the YAML of this deployment

<details><summary>show</summary>
<p>
</p>
</details>

### View the YAML of the replica set that was created by this deployment

<details><summary>show</summary>
<p>
</p>
</details>

### Get the YAML for one of the pods

<details><summary>show</summary>
<p>
</p>
</details>

### Check how the deployment rollout is going

<details><summary>show</summary>
<p>
</p>
</details>

### Update the nginx image to nginx:1.19.8

<details><summary>show</summary>
<p>
</p>
</details>

### Check the rollout history and confirm that the replicas are OK

<details><summary>show</summary>
<p>
</p>
</details>

### Undo the latest rollout and verify that new pods have the old image (nginx:1.18.0)

<details><summary>show</summary>
<p>
</p>
</details>

### Do an on purpose update of the deployment with a wrong image nginx:1.91

<details><summary>show</summary>
<p>
</p>
</details>

### Verify that something's wrong with the rollout

<details><summary>show</summary>
<p>
</p>
</details>


### Return the deployment to the second revision (number 2) and verify the image is nginx:1.19.8

<details><summary>show</summary>
<p>
</p>
</details>

### Check the details of the fourth revision (number 4)

<details><summary>show</summary>
<p>
</p>
</details>

### Scale the deployment to 5 replicas

<details><summary>show</summary>
<p>
</p>
</details>

### Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%

<details><summary>show</summary>
<p>
</p>
</details>

### Pause the rollout of the deployment

<details><summary>show</summary>
<p>
</p>
</details>

### Update the image to nginx:1.19.9 and check that there's nothing going on, since we paused the rollout

<details><summary>show</summary>
<p>
</p>
</details>

### Resume the rollout and check that the nginx:1.19.9 image has been applied

<details><summary>show</summary>
<p>
</p>
</details>

### Delete the deployment and the horizontal pod autoscaler you created

<details><summary>show</summary>
<p>
</p>
</details>

### Implement canary deployment by running two instances of nginx marked as version=v1 and version=v2 so that the load is balanced at 75%-25% ratio

<details><summary>show</summary>
<p>
</p>
</details>

## Jobs

### Create a job named pi with image perl:5.34 that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

<details><summary>show</summary>
<p>
</p>
</details>

### Wait till it's done, get the output

<details><summary>show</summary>
<p>
</p>
</details>

### Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'

<details><summary>show</summary>
<p>
</p>
</details>

### Follow the logs for the pod (you'll wait for 30 seconds)

<details><summary>show</summary>
<p>
</p>
</details>

### See the status of the job, describe it and see the logs

<details><summary>show</summary>
<p>
</p>
</details>

### Delete the job

<details><summary>show</summary>
<p>
</p>
</details>

### Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

<details><summary>show</summary>
<p>
</p>
</details>

### Create the same job, make it run 5 times, one after the other. Verify its status and delete it

<details><summary>show</summary>
<p>
</p>
</details>

### Create the same job, but make it run 5 parallel times

<details><summary>show</summary>
<p>
</p>
</details>

## Cron jobs

kubernetes.io > Documentation > Tasks > Run Jobs > [Running Automated Tasks with a CronJob](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)

### Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output

<details><summary>show</summary>
<p>
</p>
</details>

### See its logs and delete it

<details><summary>show</summary>
<p>
</p>
</details>

### Create the same cron job again, and watch the status. Once it ran, check which job ran by the created cron job. Check the log, and delete the cron job

<details><summary>show</summary>
<p>
</p>
</details>

### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to start execution after its scheduled time (i.e. the job missed its scheduled time).

<details><summary>show</summary>
<p>
</p>
</details>

### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it successfully starts but takes more than 12 seconds to complete execution.

<details><summary>show</summary>
<p>
</p>
</details>

### Create a job from cronjob.

<details><summary>show</summary>
<p>
</p>
</details>
