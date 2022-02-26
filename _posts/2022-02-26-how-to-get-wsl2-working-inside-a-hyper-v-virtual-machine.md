---
title: How to get WSL2 working inside a Hyper-V virtual machine
date: 2022-02-26T00:00:00+00:00
layout: post
permalink: /how-to-get-wsl2-working-inside-a-hyper-v-virtual-machine/
categories:
  - wsl2
tags:
  - wsl2
  - windows
  - hyper-v
---

If you have problems running WSL2 after installing it inside a virtual machine running on Hyper-V you need to enable nested virtualization.

The error looks something like this:
> Error: 0x80370102 The virtual machine could not be started because a required feature is not installed.

This can be fixed by running this command on the Hyper-V host
```sh
set-VMProcessor -VMName "My virtual machine name" -ExposeVirtualizationExtensions $true
```