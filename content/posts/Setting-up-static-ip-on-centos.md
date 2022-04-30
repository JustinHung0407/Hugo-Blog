---
title: "Setting up Static IP on CentOS7"
description: "Setting up Static IP on CentOS7"
date: 2020-02-24T22:08:25+08:00
draft: false
toc: true
images:
author: "Justin Hung"
type: ["posts", "post"]
tags:
  - Network
categories:
  - Learning
series:
  - Linux
---

In a CentOS 7, we configuring a network interface using ifcfg files located in `/etc/sysconfig/network-scripts/` directory, follow steps below and you will be good to go.

1. Create a file named `/etc/sysconfig/network-scripts/ifcfg-eth0`
2. Add following configuration into the file

    ```
    DEVICE=eth0
    DEVICETYPE=EtherNet
    BOOTPROTO=static
    DEFROUTE=yes
    NAME=eth0
    ONBOOT=yes
    IPADDR0=192.168.2.80
    PREFIX0=24
    GATEWAY0=192.168.2.1
    DNS1=8.8.8.8
    DNS2=8.8.4.4
    ```
3. Restart network service: `systemctl restart network`