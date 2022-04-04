---
title: Azure blob storage tiers
date: 2022-04-04T00:00:00+00:00
layout: post
permalink: /azure-blob-storage-tiers/
categories:
  - azure
tags:
  - azure
  - cloud
  - blob storage
---

Azure has three blob storage tiers. The decision on which tier to use should be based on how often the data should be accessed and for how long it should be retained.

## Hot tier
The hot tier should be used for data that is accessed or modified frequently. If the blobs should live for less than 30 days the hot tier is also a good choice because for the other two tiers you will be subject of an early deletion penalty fee.

## Cool tier
For the cool tier as the hot tier the data is always accessible immediately. Data placed in the cool tier has less availability but has the same latency and throughput as hot tier.

There is a penalty for deletion of blobs that is newer than 30 days so for that reason it should not be used for blobs that have a short lifetime.

## Archive tier
This tier is for long term storage that doesn't need immediate access. When data is needed it can take up to 15 hours load the data to either the cool or the hot tier.

Here you should only store data that is suppose to live longer than 180 days otherwise you will be charged an early deletion penalty fee.
