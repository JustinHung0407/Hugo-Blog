---
title: "Useful Scripts for managing K8s"
description: "Useful Scripts for managing K8s"
date: 2022-05-01T01:30:00+08:00
draft: false
toc: true
images:
author: "Justin Hung"
type: ["posts", "post"]
tags:
  - Kubernetes
categories:
  - Scripts
---


## Getting all resources in namespace
``` bash
kubectl api-resources --verbs=list --namespaced -o name \
  | xargs -n 1 kubectl get --show-kind --ignore-not-found \
  -l <label>=<value> -n $namespace
```


## Remove terminating namespace

- using `remove-terminating-namespace.sh` to remove the namespace that is stuck in the status `Terminating`
``` bash 
# remove-terminating-namespace.sh
if [[ $# -ne 1 ]]; then
  echo "Please input only namespace name"
  exit 1
fi
ns=$1
kubectl get ns ${ns} -o json > tmp.json
cat ./tmp.json | jq 'del(.spec.finalizers[])' > ./modify.json
kubectl replace --raw "/api/v1/namespaces/${ns}/finalize" -f ./modify.json
rm -f tmp.json modify.json
```

- While yor need to delete multiple Terminating namespaces
``` bash
namespace="$(kubectl get ns | grep Ter | awk {'print $1'})"
for ns in $namespace
    do ./remove-terminating-namespace.sh $ns; 
done
```


## Delete all of some resources (if resources deletion is stuck)

- Delete certain k8s resource

``` bash
secrets="$(kubectl get secrets -n gitea | grep gitea | awk '{print $1}')"
for secret in $secrets
do
    kubectl delete secrets $secret -n gitea
done
```

- Delete k8s resource including Custom Resources

``` bash
namespace=test
resource_name=VirtualService
resources="$(kubectl get $resource_name -n $namespace | awk '{if (NR!=1) print $1}')"
for resource in $resources
do
    kubectl patch $resource_name $resource -p '{"metadata":{"finalizers":null}}' --type=merge -n $namespace
done
```

- Delete all of Velero Backups stucks in `InProgress`

``` bash
pending_backup=$(velero backup get -n velero | grep InProgress | awk '{print $1}')
for resource in $pending_backup
do
    velero backup delete $resource -n velero
done
```