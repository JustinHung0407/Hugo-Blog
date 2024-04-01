---
title: "Kubernetes Debugging SOP"
description: "This comprehensive guide outlines a systematic approach to troubleshoot and resolve issues in your Kubernetes maintenance."
date: 2024-03-01T01:57:11Z
draft: true
toc: true
images:
author: "Justin Hung"
type: ["posts", "post"]
tags:
  - Kubernetes
  - SOP
categories:
  - SOP
series:
  - Kubernetes
cover:
    image: images/cover/k8s-debug.jpg
    alt: "Kubernetes Debugging SOP"
---

Kubernetes is a powerful and complex tool for deploying and managing containerized applications in production. However, due to its complexity, debugging applications in Kubernetes can be challenging.

This article will present a standard operating procedure (SOP) for debugging Kubernetes. We will cover the following topics:

> Caution: While this SOP is designed to be comprehensive, it is not exhaustive. It is important to note that debugging Kubernetes can be complex and time-consuming. This SOP is intended to provide a systematic approach to troubleshooting and resolving issues in your Kubernetes maintenance.

> Check the tips and examples at [Tips and examples]({{< relref "Tips-and-examples.md" >}})

## Using preliminary tools

Before you start debugging a specific problem, it is helpful to get a general overview of the state of your Kubernetes cluster. We can do this using a number of tools or commands:

- Tools:
  - Grafana
  - OpenLens
  - Kiali
  - Jaeger



## Checking Pod status
## Using kubectl commands
## Checking logs
## Debugging specific components













kubectl get nodes

kubectl cluster-info dump



kubectl logs -f -n {NAMESPACE} pod/{PODNAME} --previous

kubectl get events -n {NAMESPACE}
kubectl get events -A > kube-event.log

Master
    /var/log/kube-apiserver.log - API Server, responsible for serving the API
    /var/log/kube-scheduler.log - Scheduler, responsible for making scheduling decisions
    /var/log/kube-controller-manager.log - Controller that manages replication controllers
Worker Nodes
    /var/log/kubelet.log - Kubelet, responsible for running containers on the node
    /var/log/kube-proxy.log - Kube Proxy, responsible for service load balancing

## Etcd

kubectl logs etcd-stc3mc01 -n kube-system > etcd.log
cat /var/log/pods/kube-system_etcd-stc3mc01_5ee6f69d8b8410788f9b10a99510e832/etcd/0.log > etcd.log


# API-server

kubectl logs kube-apiserver-stc3mc01 -n kube-system > apiserver.log
cat /var/log/pods/kube-system_kube-apiserver-stc3mc01_e4d32820e808a1f1a7aeda21000c270e/kube-apiserver/0.log > apiserver.log


## Kubelet
journalctl -u kubelet > kubelet.log
sudo cat /var/log/messages > kubelet.log


## Scheduler

kubectl logs kube-scheduler-stc3mc01 -n kube-system > scheduler.log
cat /var/log/pods/kube-system_kube-scheduler-stc3mc01_5146743ebb284c11f03dc85146799d8b/kube-scheduler/0.log > scheduler.log


## Controller Manager

kubectl logs kube-controller-manager-stc3mc01 -n kube-system > controller-manager.log
cat /var/log/pods/kube-system_kube-controller-manager-stc3mc01_6e4073c960e84a5843df00904e71da66/kube-controller-manager/0.log > controller-manager.log


## crictl

crictl pods
crictl ps -a
crictl images