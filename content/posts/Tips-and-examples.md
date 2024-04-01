---
title: "Tips-and-examples"
description: "This post provides tips and examples for debugging Kubernetes."
date: 2023-07-06T02:10:18.062Z
draft: false
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
---




## Tips

* When following this SOP, if you find any suspicious problems, but you are not sure if they are the problem, please record them and continue to the next step, instead of getting stuck here.

Specifically, you can take the following steps:

* **Record the problems you find.** This will help you remember what you found and how you found it.
* **Continue to the next step.** Don't get stuck on a problem that you're not sure about. You can always come back to it later.

There are a few reasons for this:

* **Debugging is an iterative process.** You will often need to try multiple things before you find the root cause of the problem.
* **As you continue debugging, you may discover more information that will help you determine if the problem is related to what you originally suspected.**

Here is an example:

```
# Check Pod status

kubectl get pods

# Find a Pod in "Pending" state

kubectl describe pod <pod-name>

# Find that the Pod is not scheduled

# Not sure if the problem is related to the scheduler

# Record the problem and move on to the next step

kubectl get nodes

# Check the status of the nodes

# Find that a node has high CPU usage

# Suspect that the problem may be related to insufficient CPU resources on the node

# Continue debugging to confirm the problem
```

In this example, you found a Pod in "Pending" state. You used the `kubectl describe` command to get detailed information about the Pod and found that the Pod is not scheduled. You are not sure if the problem is related to the scheduler, so you decide to record the problem and move on to the next step.

Next, you use the `kubectl get nodes` command to check the status of the nodes. You find that a node has high CPU usage. You suspect that the problem may be related to insufficient CPU resources on the node. You can continue debugging to confirm the problem.