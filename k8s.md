# Common Kubernetes Command Cheatsheet

A list of common commands for working with Kubernetes (K8s).

This is the most useful cheatsheet:

[https://kubernetes.io/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Configuration

### Add EKS cluster to kubectl config

To have connect and deploy to the EKS cluster directly run the following:

`aws eks update-kubeconfig --<cluster name> --region <region-name>`

Verify this by running:

`kubectl config view`


## Cluster Information

### View the cluster Services | Deployments | Pods

`kubectl get <services | deployments | pods> [-o wide]`

## Deployments

### Delete Pods, Services and/or Deployments by name

`kubectl [-n my-namespace] delete pod, deployment, svc <pod|deployment|service name> [,  <pod2|deployment2|service name2>]`

### Delete all Pods, Services and/or Deployments

`kubectl [-n my-namespace] delete pod, deployment, svc --all`
