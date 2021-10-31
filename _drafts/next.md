---
title: x86 CPU registers
description: In the previous post I went through every line of a minimal assembly program. In this post I will briefly explain some of the x86 CPU registers.
date: 2021-10-29T00:00:00+00:00
layout: post
permalink: /x86 CPU registers/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

This is the third post about x86 assembly. In the previous post I went through every line of a minimal assembly program. In this post I will briefly explain some of the x86 CPU registers.


## General purpose registers
Most of the work of a CPU is to process data. But reading and storing data from memory slows down the processor. Therefore the processor has storage of it's own called registers. There are 16 64-bit registers in the x86 architecture.

* rax
* rcx
* rdx
* rbx
* rsi
* rdi
* rsp
* rbp
* r8-r15