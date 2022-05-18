---
title: Connect to a kubernetes linux node with kubectl debug
date: 2022-05-18T00:00:00+00:00
layout: post
permalink: /connect-to-a-kubernetes-linux-node-with-kubectl-debug/
categories:
  - Kubernetes
tags:
  - kubectl
  - kubernetes
---

If you want access to a terminal for kubernetes linux node you can use the `kubectl debug` command. With this you don't need ssh port(22) accessible from the internet and you also don't need to specify a password or a SSH key.

```sh
# Start a privileged debug container on the node
$ kubectl debug node/my-node -it --image=ubuntu

# To get an interactive session on the node
$ chroot /host

# Now you have a session just like ssh with root access

# Don't forget to delete the debug pod
$ kubectl delete pod node-debugger-my-node-xxxxx

```

