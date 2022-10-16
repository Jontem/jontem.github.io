---
title: Using ephemeral disks in Azure AKS
date: 2022-10-16T00:00:00+00:00
layout: post
permalink: /using-ephemeral-disks-in-azure-aks/
categories:
  - azure
tags:
  - azure
  - aks
  - kubernetes
---

In Azure a VM can be configure to use an `Ephemeral OS disk`. This means that the VM will not have it's OS disk replicated to a remote storage system. Instead the OS disk will reside on locally on the node hosting the VM and will not be available across hosts. There is no extra costs for using `Ephemeral OS disks` as they are in included in the price of the VMs. Ephemeral disks have faster read/write latency and works great for stateless VMs like an AKS node.

When you want to use `Ephemeral os disks` you need to create a new nodepool with the argument `--node-osdisk-type ephemeral`(default value is `managed`). When you pick a VM instance size check the size of the included temporary storage or cache. The minimum OS disk size for AKS is 30GB but the default for VMs with 1-8 vCPU is 128GB(P10 for managed). So if your selected VM size has smaller temp/cache storage than that you need specify `--node-osdisk-size` when creating your nodepool. The recommendation is to always specify `--node-osdisk-size` to the size of the temp storage/cache otherwise you will have unused disk space.


## Query your existing nodepools
I find this query useful when I want information about my nodepools.

```sh
$ az aks nodepool list --resource-group my-resource-group --cluster-name my-aks-cluster-name --query '[].{name:name,mode:mode,osType:osType,nodeImageVersion:nodeImageVersion,osDiskType:osDiskType,osDiskSizeGb:osDiskSizeGb,osDiskType:osDiskType,vmSize:vmSize}' --output table
```

## Creating a nodepool with Ephemeral OS disk
To create a nodepool with an ephemeral os disk with the size of 150GB(the size of tempstorage for )
```sh
$ az aks nodepool add --resource-group my-resource-group --cluster-name my-aks-cluster-name --name nodepool2 --os-type Linux --mode System --node-count 1 --node-vm-size Standard_D4ads_v5 --kubernetes-version 1.24.3 --node-osdisk-type ephemeral --node-osdisk-size 150
```

## Useful links
- [https://learn.microsoft.com/en-us/azure/virtual-machines/ephemeral-os-disks](https://learn.microsoft.com/en-us/azure/virtual-machines/ephemeral-os-disks)
- [https://learn.microsoft.com/en-us/samples/azure-samples/aks-ephemeral-os-disk/aks-ephemeral-os-disk/](https://learn.microsoft.com/en-us/samples/azure-samples/aks-ephemeral-os-disk/aks-ephemeral-os-disk/)
- [https://learn.microsoft.com/en-us/azure/aks/cluster-configuration#ephemeral-os](https://learn.microsoft.com/en-us/azure/aks/cluster-configuration#ephemeral-os)
- [https://techcommunity.microsoft.com/t5/fasttrack-for-azure/everything-you-want-to-know-about-ephemeral-os-disks-and-azure/ba-p/3565605](https://techcommunity.microsoft.com/t5/fasttrack-for-azure/everything-you-want-to-know-about-ephemeral-os-disks-and-azure/ba-p/3565605)


