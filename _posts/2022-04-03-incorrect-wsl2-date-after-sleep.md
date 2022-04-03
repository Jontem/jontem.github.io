---
title: Incorrect date in WSL2 after waking up from sleep
date: 2022-04-03T00:00:00+00:00
layout: post
permalink: /incorrect-wsl2-date-after-sleep/
categories:
  - wsl2
tags:
  - wsl2
  - windows
---

There is a bug in WSL2 when windows wakes up after sleep which results in invalid date. 

Mentioned here:
* https://github.com/microsoft/WSL/issues/5324
* https://github.com/microsoft/WSL/issues/8204

To fix this without restarting WSL2
```sh
$ sudo hwclock -s
```

