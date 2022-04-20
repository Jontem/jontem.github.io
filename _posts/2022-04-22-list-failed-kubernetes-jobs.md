---
title: List failed kubernetes jobs
date: 2022-04-20T00:00:00+00:00
layout: post
permalink: /list-failed-kubernetes-jobs/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

If you have lots of cronjobs in kubernetes and you only want to list failed jobs. Run this:
```sh
$ k get job --field-selector="status.successful=0"
```
And if you want to clean up all failed jobs just run:
```sh
$ k delete job --field-selector="status.successful=0"
```
This will also clean up the pods that was created for the jobs.
