---
title: Restarting multiple deployments in kubernetes
date: 2022-06-13T00:00:00+00:00
layout: post
permalink: /restarting-multiple-deployments-in-kubernetes/
categories:
  - kubernetes
tags:
  - kubernetes
  - deployments
  - kubectl
---

Sometimes there is a need to restart deployments in kubernetes. For example if you use a branch tag on your container images on a dev environment and use `imagePullPolicy: Always`.

Before kubernetes `1.15` the only way to force the deployments pods to redownload the image was to make a dummy patch to the deployment and updating for example an annotation. But since `1.15` you can use:
```sh
$ kubectl rollout restart deployment <deployment_name>
```
 Unfortunately this command doesn't support labels as direct input. But we can pipe multiple commands together to achieve this.

```sh
$ kubectl get deployment --no-headers -l app.kubernetes.io/part-of=my-label | awk '{ print $1; }' | xargs kubectl rollout restart deployment
```
The first part gets the name of all deployments with label `app.kubernetes.io/part-of=my-label`. Then we use `awk` to get the first column of the output. Lastly we use `xargs` to restart our deployment one at a time.
