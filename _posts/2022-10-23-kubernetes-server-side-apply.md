---
title: Kubernetes server-side apply
date: 2022-10-23T00:00:00+00:00
layout: post
permalink: /kubernetes-server-side-apply/
categories:
  - kubernetes
tags:
  - kubernetes
  - kubectl
---

## What is Server-side apply
Server-side apply is a new feature that is meant to replace client-side apply feature implemented by `kubectl apply`. The feature is a new merging algorithm for declarative configuration of objects in kubernetes and was graduated to GA in `1.22`


## Client-side apply
Client-side apply is used when you run `kubectl apply -f somefile.yaml`. The declarative configuration you specified is stored in the field `metadata.annotations.kubectl.kubernetes.io/last-applied-configuration` on the kubernetes object. 

If you change something in your yaml file and do `kubectl diff -f somefile.yaml` you get a diff of whats changed. But if someone has run an imperative command you want get a diff because that wont be saved in to `last-applied-configuration` field. Because of this it's hard to know where a value came from.

When runnning `kubectl apply -f somefile.yaml` on an existing object `kubectl` does a strategic merge patch and just updates the values that changed from `last-applied-configuration`. It doesn't replace the configuration.

## Server-side apply
The idea with server-side apply(SSA) is to have a simpler mechanism for tracking and see who's the owner of a field so that multiple appliers can update the same object. 

When using SSA the kubernetes api server adds the `metadata.managedFields` field that tracks metadata about how the field is managed.
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
  namespace: default
  labels:
    test-label: test
  managedFields:
  - manager: kubectl
    operation: Apply
    apiVersion: v1
    time: "2010-10-10T0:00:00Z"
    fieldsType: FieldsV1
    fieldsV1:
      f:metadata:
        f:labels:
          f:test-label: {}
      f:data:
        f:key: {}
data:
  key: some value
```

With the help of the `managedFields` field SSA enables conflict detection so kubernetes knows when multiple appliers try to edit the same field. An applier could be a user using `kubectl` or a `kubernetes controller`.

SSA also removes the need to write hand-rolled patches. Instead you can just apply a partial object which the Apply Api will use update the existing object.

The Apply Api can be used from any language without the need to call `kubectl`

To use SSA with `kubectl` you need to add the argument `--server-side`
```
kubectl apply -f somefile.yaml --server-side
```

