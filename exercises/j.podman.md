# Define, build and modify container images

- Note: The topic is part of the new CKAD syllabus. Here are a few examples of using **podman** to manage the life cycle of container images. The use of **docker** had been the industry standard for many years, but now large companies like [Red Hat](https://www.redhat.com/en/blog/say-hello-buildah-podman-and-skopeo) are moving to a new suite of open source tools: podman, skopeo and buildah. Also Kubernetes has moved in this [direction](https://kubernetes.io/blog/2022/02/17/dockershim-faq/). In particular, `podman` is meant to be the replacement of the `docker` command: so it makes sense to get familiar with it, although they are quite interchangeable considering that they use the same syntax.

## Podman basics

### Create a Dockerfile to deploy an Apache HTTP Server which hosts a custom main page

<details><summary>show</summary>
<p>
[Podman Installation Instructions](https://podman.io/docs/installation)
```
sudo apt install -y podman
```
```
FROM docker.io/httpd:2.4
RUN echo "hello world" > /usr/local/apache2/htdocs/index.html
```
</p>
</details>

### Build and see how many layers the image consists of

<details><summary>show</summary>
<p>
```
podman build -t my-apache2 .
STEP 1/2: FROM docker.io/httpd:2.4
STEP 2/2: RUN echo "hello world" > /usr/local/apache2/htdocs/index.html

podman image list
REPOSITORY               TAG         IMAGE ID      CREATED         SIZE
localhost/my-apache2     latest      a893a8ec6154  15 minutes ago  152 MB

podman image tree my-apache2
Image ID: a893a8ec6154
Tags:     [localhost/my-apache2:latest]
Size:     152.2MB
Image Layers
├── ID: 8e2ab394fabf Size: 77.83MB
├── ID: 17bb66229b5b Size:  2.56kB
├── ID: 1e98742e388d Size: 1.024kB
├── ID: 2168e8efe6bf Size:  11.4MB
├── ID: a886c7079c2b Size: 62.92MB
├── ID: b401191c0613 Size: 3.584kB Top Layer of: [docker.io/library/httpd:2.4]
└── ID: 18e582d869e2 Size: 15.36kB Top Layer of: [localhost/my-apache2:latest]
```
</p>
</details>

### Run the image locally, inspect its status and logs, finally test that it responds as expected

<details><summary>show</summary>
<p>
```
podman run --name test -p 8080:80 localhost/my-apache2

podman ps
CONTAINER ID  IMAGE                        COMMAND           CREATED         STATUS             PORTS                 NAMES
9fd520a1f2bc  localhost/my-apache2:latest  httpd-foreground  20 seconds ago  Up 20 seconds ago  0.0.0.0:8080->80/tcp  test

podman logs -f test

curl http://localhost:8080
hello world
```
</p>
</details>

### Run a command inside the pod to print out the index.html file

<details><summary>show</summary>
<p>
```
podman exec -it test cat /usr/local/apache2/htdocs/index.html
hello world
```
</p>
</details>

### Tag the image with ip and port of a private local registry and then push the image to this registry

<details><summary>show</summary>
<p>
[Podman - Local Container Registry](https://blog.while-true-do.io/podman-local-con)tainer-registry/)

Start local podman registry
```
podman container run -dt -p 5000:5000 --name registry docker.io/library/registry:2

podman image ls
REPOSITORY                  TAG         IMAGE ID      CREATED        SIZE
localhost/my-apache2        latest      a893a8ec6154  15 hours ago   152 MB

podman image tag localhost/my-apache2:latest localhost:5000/my-apache2:latest
podman image push localhost:5000/my-apache2:latest --tls-verify=false

podman image search localhost:5000/ --tls-verify=false
INDEX           NAME                       DESCRIPTION  STARS       OFFICIAL    AUTOMATED
localhost:5000  localhost:5000/alpine                   0                       
```
</p>
</details>

 Verify that the registry contains the pushed image and that you can pull it

<details><summary>show</summary>
<p>
```
curl localhost:5000/v2/_catalog
{"repositories":["my-apache2"]}

podman rmi localhost:5000/my-apache2

podman pull localhost:5000/my-apache2:latest --tls-verify=false

podman image ls
REPOSITORY                  TAG         IMAGE ID      CREATED        SIZE
localhost:5000/my-apache2   latest      a893a8ec6154  15 hours ago   152 MB
```
</p>
</details>

### Run a pod with the image pushed to the registry

<details><summary>show</summary>
<p>
```
podman image search localhost:5000/ --tls-verify=false

podman run -p 8080:80 --name=test localhost:5000/my-apache2

curl localhost:8080
hello world
```
</p>
</details>

### Log into a remote registry server and then read the credentials from the default file


<details><summary>show</summary>
<p>
</p>
</details>

### Create a secret both from existing login credentials and from the CLI

<details><summary>show</summary>
<p>
</p>
</details>

### Create the manifest for a Pod that uses one of the two secrets just created to pull an image hosted on the relative private remote registry

<details><summary>show</summary>
<p>
</p>
</details>

### Clean up all the images and containers

<details><summary>show</summary>
<p>
</p>
</details>
