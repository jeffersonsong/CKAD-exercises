![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/configuration&empty)
# Configuration (18%)

[ConfigMaps](#configmaps)

[SecurityContext](#securitycontext)

[Requests and Limits](#requests-and-limits)

[Secrets](#secrets)

[Service Accounts](#serviceaccounts)

<br>#Tips, export to variable<br>
<br>export ns="-n secret-ops"</br>
<br>export do="--dry-run=client -oyaml"</br>
## ConfigMaps

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

### Create a configmap named config with values foo=lala,foo2=lolo

<details><summary>show</summary>
<p>
```
k create cm config --from-literal=foo=lala --from-literal=foo2=lolo
```
</p>
</details>

### Display its values

<details><summary>show</summary>
<p>
```
k describe cm config
```
</p>
</details>

### Create and display a configmap from a file

Create the file with

```bash
echo -e "foo3=lili\nfoo4=lele" > config.txt
```

<details><summary>show</summary>
<p>
```
k create cm config --from-file=config.txt
```
</p>
</details>

### Create and display a configmap from a .env file

Create the file with the command

```bash
echo -e "var1=val1\n# this is a comment\n\nvar2=val2\n#anothercomment" > config.env
```

<details><summary>show</summary>
<p>
```
k create cm config --from-env-file=config.env
k describe cm config
```
</p>
</details>

### Create and display a configmap from a file, giving the key 'special'

Create the file with

```bash
echo -e "var3=val3\nvar4=val4" > config4.txt
```

<details><summary>show</summary>
<p>
```
k create cm config --from-file=special=config4.txt
```
</p>
</details>

### Create a configMap called 'options' with the value var5=val5. Create a new nginx pod that loads the value from variable 'var5' in an env variable called 'option'

<details><summary>show</summary>
<p>
```
k create cm options --from-literal=var5=val5
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    env:
      - name: option
        valueFrom:
          configMapKeyRef:
            name: options
            key: var5
```
</p>
</details>

### Create a configMap 'anotherone' with values 'var6=val6', 'var7=val7'. Load this configMap as env variables into a new nginx pod

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    envFrom:
    - configMapRef:
        name: anotherone
```
</p>
</details>

### Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /etc/lala
      name: cmvolume
  volumes:
  - name: cmvolume
    configMap:
      name: cmvolume
```
</p>
</details>

## SecurityContext

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

### Create the YAML for an nginx pod that runs with the user ID 101. No need to create the pod

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  securityContext:
    runAsUser: 101
  containers:
  - image: nginx
    name: nginx
```
</p>
</details>


### Create the YAML for an nginx pod that has the capabilities "NET_ADMIN", "SYS_TIME" added to its single container

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    securityContext:
      capabilities:
        add:
        - NET_ADMIN
        - SYS_TIME
```
</p>
</details>

## Resource requests and limits

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Assign CPU Resources to Containers and Pods](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/)

### Create an nginx pod with requests cpu=100m,memory=256Mi and limits cpu=200m,memory=512Mi

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        cpu: 100m
        memory: 256Mi
      limits:
        cpu: 200m
        memory: 512Mi
```
</p>
</details>

## Limit Ranges
kubernetes.io > Documentation > Concepts > Policies > Limit Ranges (https://kubernetes.io/docs/concepts/policy/limit-range/)

### Create a namespace named limitrange with a LimitRange that limits pod memory to a max of 500Mi and min of 100Mi

<details><summary>show</summary>
<p>
```
k create ns limitrange
```
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
  namespace: limitrange
spec:
  limits:
  - type: Pod
    min:
      memory: 100Mi
    max:
      memory: 500Mi
```
</p>
</details>

### Describe the namespace limitrange

<details><summary>show</summary>
<p>
```
k describe limits mem-limit-range -n limitrange
```
</p>
</details>

### Create an nginx pod that requests 250Mi of memory in the limitrange namespace

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: limitrange
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        memory: 250Mi
```
</p>
</details>


## Resource Quotas
kubernetes.io > Documentation > Concepts > Policies > Resource Quotas (https://kubernetes.io/docs/concepts/policy/resource-quotas/)

### Create ResourceQuota in namespace `one` with hard requests `cpu=1`, `memory=1Gi` and hard limits `cpu=2`, `memory=2Gi`.

<details><summary>show</summary>
<p>
```
kubectl create quota my-quota -n one --hard=requests.cpu=1,requests.memory=1Gi,limits.cpu=2,limits.memory=2Gi
```
</p>
</details>

### Attempt to create a pod with resource requests `cpu=2`, `memory=3Gi` and limits `cpu=3`, `memory=4Gi` in namespace `one`

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: one
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        cpu: 2
        memory: 3Gi
      limits:
        cpu: 3
        memory: 4Gi
```
</p>
</details>

### Create a pod with resource requests `cpu=0.5`, `memory=1Gi` and limits `cpu=1`, `memory=2Gi` in namespace `one`

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: one
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        cpu: 0.5
        memory: 1Gi
      limits:
        cpu: 1
        memory: 2Gi
```
</p>
</details>


## Secrets

kubernetes.io > Documentation > Concepts > Configuration > [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

kubernetes.io > Documentation > Tasks > Inject Data Into Applications > [Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

### Create a secret called mysecret with the values password=mypass

<details><summary>show</summary>
<p>
```
k create secret generic mysecret --from-literal=password=mypass
```
</p>
</details>

### Create a secret called mysecret2 that gets key/value from a file

Create a file called username with the value admin:

```bash
echo -n admin > username
```

<details><summary>show</summary>
<p>
```
k create secret generic mysecret2 --from-file=username
```
</p>
</details>

### Get the value of mysecret2

<details><summary>show</summary>
<p>
```
k get secret mysecret2 -o yaml
echo -n "YWRtaW4=" | base64 --decode
```
</p>
</details>

### Create an nginx pod that mounts the secret mysecret2 in a volume on path /etc/foo

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    volumeMounts:
    - mountPath: /etc/foo
      name: vol
  volumes:
  - name: vol
    secret:
      secretName: mysecret2 
```
</p>
</details>

### Delete the pod you just created and mount the variable 'username' from secret mysecret2 onto a new nginx pod in env variable called 'USERNAME'

<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    env:
    - name: USERNAME
      valueFrom:
        secretKeyRef:
          name: mysecret2
          key: username
```
</p>
</details>

### Create a Secret named 'ext-service-secret' in the namespace 'secret-ops'. Then, provide the key-value pair API_KEY=LmLHbYhsgWZwNifiqaRorH8T as literal.

<details><summary>show</summary>
<p>
```
k create secret generic ext-service-secret -n secret-ops --from-literal=API_KEY=LmLHbYhsgWZwNifiqaRorH8T
```
</p>
</details>

### Consuming the Secret. Create a Pod named 'consumer' with the image 'nginx' in the namespace 'secret-ops' and consume the Secret as an environment variable. Then, open an interactive shell to the Pod, and print all environment variables.
<details><summary>show</summary>
<p>
```
k run consumer -n secret-ops --image=nginx --dry-run=client -o yaml > pod.yaml
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: consumer
  namespace: secret-ops
spec:
  containers:
  - image: nginx
    name: consumer
    env:
    - name: API_KEY
      valueFrom:
        secretKeyRef:
          name: ext-service-secret
          key: API_KEY
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: consumer
  namespace: secret-ops
spec:
  containers:
  - image: nginx
    name: consumer
    envFrom:
    - secretRef:
        name: ext-service-secret
```
</p>
</details>

### Create a Secret named 'my-secret' of type 'kubernetes.io/ssh-auth' in the namespace 'secret-ops'. Define a single key named 'ssh-privatekey', and point it to the file 'id_rsa' in this directory.
<details><summary>show</summary>
<p>
```
k create secret generic my-secret -n secret-ops --type='kubernetes.io/ssh-auth' --from-file=ssh-privatekey=/home/jeffersons/.ssh/id_ed25519 
```
</p>
</details>

### Create a Pod named 'consumer' with the image 'nginx' in the namespace 'secret-ops', and consume the Secret as Volume. Mount the Secret as Volume to the path /var/app with read-only access. Open an interactive shell to the Pod, and render the contents of the file.
<details><summary>show</summary>
<p>
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: consumer
  namespace: secret-ops
spec:
  containers:
  - image: nginx
    name: consumer
    volumeMounts:
    - mountPath: /var/app
      name: vol
      readOnly: true
  volumes:
  - name: vol
    secret:
      secretName: my-secret
```
</p>
</details>

## ServiceAccounts

kubernetes.io > Documentation > Tasks > Configure Pods and Containers > [Configure Service Accounts for Pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

### See all the service accounts of the cluster in all namespaces

<details><summary>show</summary>
<p>
```
k get sa -A
```
</p>
</details>

### Create a new serviceaccount called 'myuser'

<details><summary>show</summary>
<p>
```
k create sa myuser
```
</p>
</details>

### Create an nginx pod that uses 'myuser' as a service account

<details><summary>show</summary>
<p>
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  serviceAccountName: myuser
  containers:
  - image: nginx
    name: nginx
```
</p>
</details>

### Generate an API token for the service account 'myuser'

<details><summary>show</summary>
<p>
```
k create token myuser
```
</p>
</details>
