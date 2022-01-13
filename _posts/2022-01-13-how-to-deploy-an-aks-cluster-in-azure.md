---
title: How to deploy an aks cluster in azure
description: How to deploy a kubernetes cluster in azure(AKS)
date: 2022-01-13T00:00:00+00:00
layout: post
permalink: /how-to-deploy-an-aks-cluster-in-azure/
categories:
  - azure
tags:
  - cloud
  - aks
  - kubernetes
---

This how to on how to deploy a kubernetes cluster in azure(AKS). The steps below is done with az cli in a bash terminal.

## Creating a resource group
First of all we need a resource group to deploy our resources. This is where our kubernetes cluster will be created together with our custom vnet.

```sh
az group create --name rg-something-swedencentral-01 --location swedencentral
```

## Creating a custom vnet
Then we'll create our own vnet so we can have control over the assigned IP adresses. The vnet can be created automatically if you don't have any need for specifying your own subnets. But if you want to have a hybrid cloud with connected to a on premise network you need to make sure the cloud network and on premise network doesn't collide.

Firstly you need to assign adresses to the vnet
```sh
az network vnet create --name vnet-something-swedencentral-01 --resource-group rg-something-swedencentral-01 --address-prefixes 10.110.0.0/16
```

Then we create a subnet used for our aks cluster. This subnet is used for both our nodes and pods. It's important to have enough adresses for the cluster. If you grow out of your subnet the cluster must be recreated.
```sh
az network vnet subnet create --resource-group rg-something-swedencentral-01 --vnet-name vnet-something-swedencentral-01 --name snet-something-swedencentral-aks-01 --address-prefixes 10.110.0.0/22
```

## Create a user assigned managed identity
To use our newly created vnet and subnet the aks cluster needs to have permission to use it. Aks uses a service principal when it calls the azure api:s. A service principal is just a appid with a secret. A service principal secret by default expires after one year and you need to renew the secret. But you can also use a managed identity. A managed identity is a service principal which azure automically renews the secret every 46 days.

We'll start by creating a user assigned managed identity.
```sh
az identity create --resource-group rg-something-swedencentral-01 --name id-aks-something-swedencentral-01
```

The we need to create a role assigment to our subnet with the role Network Contributor
```sh
# Get the id of the subnet
SUBNET_ID=$(az network vnet subnet list \
    --resource-group rg-something-swedencentral-01 \
    --vnet-name vnet-something-swedencentral-01 \
    --query "[0].id" --output tsv)

# Get the appid of the managed identity
APP_ID=$(az identity show --resource-group rg-something-swedencentral-01 --name id-aks-something-swedencentral-01 --query "clientId" --output tsv)

# Create the role assignment
az role assignment create --assignee $APP_ID --scope $SUBNET_ID --role "Network Contributor"
```

## Create the aks cluster
To create the cluster we need to run the command below.
```sh
az aks create \
--resource-group rg-something-swedencentral-01 \
--name aks-something-swedencentral-01 \
--location swedencentral \
--enable-managed-identity \
--assign-identity $APP_ID \
--network-plugin azure \
--vnet-subnet-id $SUBNET_ID \
--docker-bridge-address 172.17.0.1/16 \
--dns-service-ip 10.96.0.10 \
--service-cidr 10.96.0.0/16 \
--windows-admin-username windowsuser \
--windows-admin-password 'windowspassword' \
--api-server-authorized-ip-ranges <ALLOW_CIDR_RANGE> \
--node-vm-size Standard_D2s_v3 \
--node-count 2 \
--node-resource-group rg-aks-something-swedencentral-01
```
I'll go through some of the parameters.

### network-plugin
Aks has two network plugins kubenet and azure cni. Kubenet is fully isolated NAT network and doesn't support bringing your own vnet. Azure cni is required for windows nodes and if you want to have your own vnet.

### service-cidr
This is a network that only resides inside the cluster. It's used by kubernetes services. This ip-range cannot be accessible anywhere else in your network. But the network can be reused inside another kubernetes cluster.

### windows-admin-username and windows-admin-password
This is only needed if you want to run windows nodes in you cluster.

### api-server-authorized-ip-ranges
Azure support both public and private cluster. A private cluster means you have to have a vpn or hybrid cloud setup to manage your kubernetes cluster. A public cluster is by default accessible anywhere. With this parameter you can lock down which ip ranges that have access.

### node-resource-group
This parameter sets the name of the automatic created node resource group. This resource group is always created and is managed by the kubernetes cluster.
