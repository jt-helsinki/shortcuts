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

### Find the AWS EKS AMIs

`aws ec2 describe-images --owners amazon --filters 'Name=name,Values="*eks*[<product version name i.e. 1.11-v20190220>]"' [--region eu-central-1]`

Note: ensure your aws-cli is configured for the correct region as AMIs differ from region to region. Version numbers can be found in the AWS marketplace: [https://aws.amazon.com/marketplace](https://aws.amazon.com/marketplace)


## Cluster Information

### View the cluster Services | Deployments | Pods

`kubectl get <services | deployments | pods> [-o wide]`

### View all cluster events

`kubectl get events --all-namespaces  [--sort-by='.metadata.creationTimestamp']`

## Nodes

### Drain the nodes

`kubectl get nodes`

`kubectl drain --force --ignore-daemonsets --delete-local-data <node name i.e. ip-xx-xx-xx-xxx.eu-central-1.compute.internal>`

## Deployments

### Force Delete Pods, Services and/or Deployments by name

`kubectl delete pod, deployment, svc <PODNAME> --grace-period=0 --force [--namespace <namespace>]`

This is particularly useful if a deleted pod, serivce or deployment won't terminate. That is, it hangs in the "Terminating" state. 

### Delete Pods, Services and/or Deployments by name

`kubectl [-n my-namespace] delete pod, deployment, svc <pod name|deployment name|service nam> [,  <pod name2|deployment name2|service name2>]`

### Delete all Pods, Services and/or Deployments

`kubectl [-n my-namespace] delete pod, deployment, svc --all`

## Debugging

### Retrieve current state of Pod, Service or Deployment

`kubectl describe <pod|deployment|service>  <pod name|deployment name|service name> [--namespace <namespace>]`

### Interactive shell to a running container

`kubectl exec -it <pod name> [-n <namespace>] -- /bin/sh`

## Kubectl script

This is a useful function that can be added to `~/.zshrc` or `~/.bashrc` to help you easily use kubectl without the copy & paste. A bit hacky and not perfect but it might help you.

```
x-kubectl() {
    namespaces=( $( kubectl get namespaces --no-headers -o custom-columns=":metadata.name" ) )

    echo "Which namespace?"

    for (( i = 1; i <= ${#namespaces[@]}; i++ )) do
      printf "%s\t%s\n" "$((i))" "${namespaces[$i]}"
    done

    echo -n "Namespace: "
    read namespace_input
    echo
    namespace=$namespaces[$((namespace_input))]

    resources=(
      "pod"
      "deployment"
      "service"
      "ingress"
    )
    echo "Which resource?"
    for (( i = 1; i <= ${#resources[@]}; i++ )) do
      printf "%s\t%s\n" "$((i))" "${resources[$i]}"
    done

    echo -n "Resource: "
    read resource_input
    echo
    resource=$resources[$((resource_input))]

    items=( $( kubectl get $resource  -n $namespace --no-headers -o custom-columns=":metadata.name") )

    echo "Which $resource?"
    for (( i = 1; i <= ${#items[@]}; i++ )) do
      printf "%s\t%s\n" "$((i))" "${items[$i]}"
    done

    echo -n "$resource: "
    read item_input
    echo
    item=$items[$((item_input))]

    actions=(
      "describe"
      "get"
      "delete"
    )
    if  [[ ( "pod" == $resource ) ]]
    then
      actions+="interactive shell (pods only)"
    fi

    unset action
    while [ -z "$action" ];
    do
  	  echo "Which action?"
  	  for (( i = 1; i <= ${#actions[@]}; i++ )) do
  		printf "%s\t%s\n" "$((i))" "${actions[$i]}"
  	  done

  	  echo -n "Action: "
  	  read action_input
  	  action=$actions[$((action_input))]
      echo
    done

    echo
    echo "######################################################################"

    command="kubectl $action $resource -n $namespace $item"
    if  [[ ( "4" == $action_input ) && ( "pod" == $resource ) ]]; # interactive shell for pods
    then
      command="kubectl exec -it $item -n $namespace -- /bin/sh"
    fi

    echo
    echo
    echo "Command run: $command"
    echo
    echo
    eval $command
    echo
    echo
    echo "######################################################################"
    echo

}
```
