---
title: Installing multiple instances of Ubuntu in WSL2
date: 2022-02-26T00:00:00+00:00
layout: post
permalink: /installing-multiple-instances-of-ubuntu-in-wsl2/
categories:
  - wsl2
tags:
  - wsl2
  - windows
---

If you want to have multiple instances of Ubuntu in WSL2 this is how you do it:

## Take a backup
If you already have one distro installed and you want the new dist to be empty you need to backup the old one so we can install an empty distro.

```sh
# Backup 
$ wsl --export <distname probably Ubuntu> C:\linux\backed-up-ubuntu.tar.gz

# Unregister the old dist
$ wsl --unregister <distname probably Ubuntu>
```

## Install a new fresh version
```sh
# Install the distribution
$ wsl --install -d Ubuntu
```

## Take a backup of the fresh install for use when you want a new instance
```sh
$ wsl --export Ubuntu C:\linux\ubuntu-empty.tar.gz
```

## Initialize a new distro and run it
```sh
$ wsl --import Ubuntu-2 C:\linux\ubuntu-2 C:\linux\ubuntu-empty.tar.gz
$ wsl -d Ubuntu-2
```

## Restore and run the old distro
```sh
$ wsl --import the-old C:\linux\the-old C:\linux\backed-up-ubuntu.tar.gz
$ wsl -d the-old
```
