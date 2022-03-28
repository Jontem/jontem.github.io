---
title: Dotnet memory generations
date: 2022-03-28T00:00:00+00:00
layout: post
permalink: /dotnet-memory-generations/
categories:
  - dotnet
tags:
  - garbage collection
  - dotnet
  - memory
---

In dotnet the managed heap is divided into three generations. This is done to optimize the performance of the garbage collector. Generation 0 contains newly created objects and generation 2 contains the oldest objects. Garbage collection is run more often in generation 0 than in 2. 

The garbage collector also performs memory compaction which means that it defrags memory to remove dead space which helps to make the heap smaller.

An object is collected by the garbage collector when it's non rooted which means that nothing refers to it anymore.

## Generation 0
When an object is allocated on the managed heap it first goes into generation 0. Objects that survive a garbage collection of gen 0 is promoted to generation 1. Garbage collection of objects in this generation is fast and runs often. Most objects are collected in this generation.

## Generation 1
Garbage collection in gen 1 is done less frequently than in gen 0. Objects that survive garbage collection here is promoted to gen 2. It acts like a buffer for gen 2. 

## Generation 2
This generation contains long lived objects for example static data. Objects that survives a garbage collection here remains here until there are no reference to it.

## Large object heap(LOH)
The large object heap is a special place in the managed heap. Here large objects larger than 85KB is placed. This is a magic number which have been set for when it starts to be performance penalty to move these objects around when doing memory compaction. Objects that are larger than 85KB is not placed in gen 0 instead they are placed directly on the LOH. Garbage collection in the LOH is run together with generation 2. But memory compaction is never run automatically here.

