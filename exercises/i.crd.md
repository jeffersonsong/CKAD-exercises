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
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: operators.stable.example.com
spec:
  group: stable.example.com
  scope: Namespaced
  names:
    plural: operators
    singular: operator
    shortNames: 
    - op
    kind: Operator
  versions:
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              email:
                type: string
              name:
                type: string
              age:
                type: integer
```
</p>
</details>

### Create the CRD resource in the K8S API

<details><summary>show</summary>
<p>
```
k apply -f crd.yaml
```
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
```yaml
apiVersion: "stable.example.com/v1"
kind: Operator
metadata:
  name: operator-sample
spec:
  email: operator-sample@stable.example.com
  name: 'operator sample'
  age: 30
```
```
k apply -f custom-resource.yaml
```
</p>
</details>

### Listing operator

<details><summary>show</summary>
<p>
k get op
</p>
</details>
