# Common Kubernetes Command Cheatsheet

A list of common commands for working with Kubernetes (K8s).

## Configuration

### Add EKS cluster to kubectl config

To have connect and deploy to the EKS cluster directly run the following:

`aws eks update-kubeconfig --<cluster name> --region <region-name>`

Verify this by running:

`kubectl config view`
