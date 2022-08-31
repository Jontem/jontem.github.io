---
title: "containerd fails to start on windows with failed to recover state: failed to reserve sandbox name"
date: 2022-08-17T00:00:00+00:00
layout: post
permalink: /containerd-fails-to-start-on-windows/
categories:
  - kubernetes
tags:
  - containerd
  - windows
  - kubernetes
---

I was having problem getting containerd starting after a reboot on a crashed windows node. The message from the containerd said `failed to recover state: failed to reserve sandbox name`.

I found this issue on [github](https://github.com/containerd/cri/issues/1014) that helped me in the right direction.

Start by modifying `config.toml` by adding
```
disabled_plugins = ["io.containerd.grpc.v1.cri"]
```
Then start containerd and use the `ctr` cli.
```
# To list the containers and find the container id from the logs
ctr -n k8s.io containers ls

# Delete the container
ctr -n k8s.io containers rm <containter id>
```

Remove the line added to `config.toml` and restart containerd and everything should work again