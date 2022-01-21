---
title: Add an Azure static disk with Azure disk CSI driver in Kubernetes
date: 2022-01-21T00:00:00+00:00
layout: post
permalink: /add-an-azure-static-disk-with-azure-disk-csi-driver-in-kubernetes/
categories:
  - azure
tags:
  - cloud
  - aks
  - kubernetes
---

The in tree storage drivers for Kubernetes is planned to be removed in version 1.26-1.27. The new Container Storage Interface(CSI) is the way forward for handling persistent storage inside Kubernetes and it's been GA since 1.13. For Azure disk the in tree driver is deprecated since 1.19.

Recently I've setup a new AKS cluster and the documentation how to setup and azure static disk volume with Azure disk CSI was a bit hard to find. So I'll document how I did here.

## Create the static disk
The official docs recommends to create disk in the node resource group of your AKS cluster.

```sh
az disk create \
--resource-group my-node-resource-group \
--name disk-aks-something \
--location swedencentral \
--os-type linux \
--sku Premium_LRS \
--size-gb 8 \
--query id --output tsv
```
This command will output the disk id that you'll use for as the `volumeHandle` in the next step.

## Add the persistent volume
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-my-volume
spec:
  capacity:
    storage: 8Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  csi:
    driver: disk.csi.azure.com
    readOnly: false
    volumeHandle: /subscriptions/subscription-id/my-node-resource-group/my-resource-group/providers/Microsoft.Compute/disks/disk-aks-something
    volumeAttributes:
      fsType: ext4
      #partition: "1"  # optional, remove this if there is no partition
```

## Add the persistent volume claim
```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-my-volume
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
  volumeName: pv-my-volume
  storageClassName: ""
```

## Add the pvc to your pod
```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx-azuredisk
spec:
  nodeSelector:
    kubernetes.io/os: linux
  containers:
    - image: mcr.microsoft.com/oss/nginx/nginx:1.17.3-alpine
      name: nginx-azuredisk
      command:
        - "/bin/sh"
        - "-c"
        - while true; do echo $(date) >> /mnt/azuredisk/outfile; sleep 1; done
      volumeMounts:
        - name: azuredisk01
          mountPath: "/mnt/azuredisk"
  volumes:
    - name: azuredisk01
      persistentVolumeClaim:
        claimName: pvc-my-volume
```

And that's it!