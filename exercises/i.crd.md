# Extend the Kubernetes API with CRD (CustomResourceDefinition)

- Note: CRD is part of the new CKAD syllabus. Here are a few examples of installing custom resource into the Kubernetes API by creating a CRD.

## CRD in K8s

### Create a CustomResourceDefinition manifest file for an Operator with the following specifications :
* *Name* : `operators.stable.example.com`
* *Group* : `stable.example.com`
* *Schema*: `<email: string><name: string><age: integer>`
* *Scope*: `Namespaced`
* *Names*: `<plural: operators><singular: operator><shortNames: op>`
* *Kind*: `Operator`

<details><summary>show</summary>
<p>
</p>
</details>

### Create the CRD resource in the K8S API

<details><summary>show</summary>
<p>
</p>
</details>

### Create custom object from the CRD

* *Name* : `operator-sample`
* *Kind*: `Operator`
* Spec:
  * email: `operator-sample@stable.example.com`
  * name: `operator sample`
  * age: `30`

<details><summary>show</summary>
<p>
</p>
</details>

### Listing operator

<details><summary>show</summary>
<p>
</p>
</details>
