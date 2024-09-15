# Managing Kubernetes with Helm

- Note: Helm is part of the new CKAD syllabus. Here are a few examples of using Helm to manage Kubernetes.

## Helm in K8s

### Creating a basic Helm chart
<details><summary>show</summary>
<p>
```
helm create myapp

k create deploy nginx --image=nginx --port=80 --dry-run=client -o yaml > deployment.yaml
k expose deploy nginx --port=80 --target-port=80 --dry-run=client -o yaml > service.yaml
k delete svc/nginx deploy/nginx
```
```
.
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   └── service.yaml
└── values.yaml
```
values.yaml
```yaml
replica_count: 2
service_port: 80
container_port: 80
```
deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  replicas: {{ .values.replica_count }}
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: {{ .Values.container_port }}
```
service.yaml
```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  ports:
  - port: {{ .Values.service_port }}
    protocol: TCP
    targetPort: {{ .Values.container_port }}
  selector:
    app: nginx
```
```
helm package myapp
helm install myapp ./myapp-0.1.0.tgz
k run temp --image=busybox -it --rm --restart=Never -- wget -O- nginx --timeout 5
```
</p>
</details>

### Running a Helm chart

<details><summary>show</summary>
<p>
```
helm install -f myvalues.yaml myapp ./myapp-0.1.0.tgz
```
myvalues.yaml
```yaml
replica_count: 1
service_port: 80
container_port: 80
```
</p>
</details>

### Find pending Helm deployments on all namespaces

<details><summary>show</summary>
<p>
```
helm list --pending -A
```
</p>
</details>

### Uninstall a Helm release

<details><summary>show</summary>
<p>
```
helm uninstall myapp
```
</p>
</details>

### Upgrading a Helm chart

<details><summary>show</summary>
<p>
```
helm upgrade -f myvalues.yaml -f override.yaml myapp ./myapp-0.1.0.tgz
k run temp --image=busybox -it --rm --restart=Never -- wget -O- nginx:8080 --timeout 5
```
override.yaml
```yaml
service_port: 8080
```
</p>
</details>

### Using Helm repo

<details><summary>show</summary>
<p>
```
helm repo --help
  add         add a chart repository
  index       generate an index file given a directory containing packaged charts
  list        list chart repositories
  remove      remove one or more chart repositories
  update      update information of available charts locally from chart repositories
```
</p>
</details>

### Download a Helm chart from a repository 

<details><summary>show</summary>
<p>
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo apache
helm pull bitnami/apache --untar
```
</p>
</details>

### Add the Bitnami repo at https://charts.bitnami.com/bitnami to Helm
<details><summary>show</summary>
<p>
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo list
```    
</p>
</details>

### Write the contents of the values.yaml file of the `bitnami/node` chart to standard output
<details><summary>show</summary>
<p>
```
helm show values bitnami/node | grep replica
```
</p>
</details>

### Install the `bitnami/node` chart setting the number of replicas to 5
<details><summary>show</summary>
<p>
```
helm install --set replicaCount=5 mynode bitnami/node
```
</p>
</details>

