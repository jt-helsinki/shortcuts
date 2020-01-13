# Common Kubernetes Command Cheatsheet

A list of common commands for working with Kubernetes (K8s).

This is the most useful cheatsheet:

[https://kubernetes.io/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Configuration

### Add EKS cluster to kubectl config

To have connect and deploy to the EKS cluster directly run the following:

`aws eks update-kubeconfig --<cluster name> --region <region-name>`

See this link for more info: [https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html)

Verify this by running:

`kubectl config view`


## Cluster Information

### View the cluster Services | Deployments | Pods

`kubectl get <services | deployments | pods> [-o wide]`

### View all cluster events

`kubectl get events --all-namespaces  [--sort-by='.metadata.creationTimestamp']`

## Deployments

### Force Delete Pods, Services and/or Deployments by name

`kubectl delete pod, deployment, svc <PODNAME> --grace-period=0 --force [--namespace <namespace>]`

This is particularly useful if a deleted pod, serivce or deployment won't terminate. That is, it hangs in the "Terminating" state. 

### Delete Pods, Services and/or Deployments by name

`kubectl [-n my-namespace] delete pod, deployment, svc <pod name|deployment name|service nam> [,  <pod name2|deployment name2|service name2>]`

### Delete all Pods, Services and/or Deployments

`kubectl [-n my-namespace] delete pod, deployment, svc --all`

## Debugging

### Get current state of Pod, Service or Deployment

`kubectl describe <pod|deployment|service>  <pod name|deployment name|service name> [--namespace <namespace>]`
