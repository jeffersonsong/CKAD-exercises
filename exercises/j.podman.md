# Define, build and modify container images

- Note: The topic is part of the new CKAD syllabus. Here are a few examples of using **podman** to manage the life cycle of container images. The use of **docker** had been the industry standard for many years, but now large companies like [Red Hat](https://www.redhat.com/en/blog/say-hello-buildah-podman-and-skopeo) are moving to a new suite of open source tools: podman, skopeo and buildah. Also Kubernetes has moved in this [direction](https://kubernetes.io/blog/2022/02/17/dockershim-faq/). In particular, `podman` is meant to be the replacement of the `docker` command: so it makes sense to get familiar with it, although they are quite interchangeable considering that they use the same syntax.

## Podman basics

### Create a Dockerfile to deploy an Apache HTTP Server which hosts a custom main page

<details><summary>show</summary>
<p>
</p>
</details>

### Build and see how many layers the image consists of

<details><summary>show</summary>
<p>
</p>
</details>

### Run the image locally, inspect its status and logs, finally test that it responds as expected

<details><summary>show</summary>
<p>
</p>
</details>

### Run a command inside the pod to print out the index.html file

<details><summary>show</summary>
<p>
</p>
</details>

### Tag the image with ip and port of a private local registry and then push the image to this registry

<details><summary>show</summary>
<p>
</p>
</details>

 Verify that the registry contains the pushed image and that you can pull it

<details><summary>show</summary>
<p>
</p>
</details>

### Run a pod with the image pushed to the registry

<details><summary>show</summary>
<p>
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
