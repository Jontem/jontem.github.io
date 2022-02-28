---
title: Checking Kubernetes API access with kubectl
date: 2022-02-26T00:00:00+00:00
layout: post
permalink: /checking-kubernetes-api-access-with-kubectl/
categories:
  - kubernetes
tags:
  - kubernetes
  - kubectl
  - rbac
---

Today I learned that kubectl have an easy subcommand `auth can-i` to test permissions on the Kubernetes API.

To test if the current user has permission to create deployments in the namespace named `dev`
```sh
# Outputs yes or no
$ kubectl auth can-i create deployments --namespace dev
```

If you are a cluster admin you can use impersonation to test what other users/service accounts can do.

The command below tests if the service account `my-service-account` in namespace `otherns` can run `get pod` in namespace `dev`
```sh
# Outputs yes or no
$ kubectl auth can-i get pod --namespace dev --as system:serviceaccount:otherns:my-service-account
```



