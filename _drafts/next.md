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

These registers are used for inputs and outputs for different CPU instruction. For example in the application binary interface(ABI) for linux 64-bit the `rax` register is used for specifying the syscall number and it's also used for storing the return value of functions. The arguments for a function is then stored in order `rdi`(arg1), `rsi`(arg2), `rdx`(arg3) and so on. The image below shows the calling conventions for syscalls on different architectures.

![Linux 64-bit ABI](/assets/linux_abi.png)

## Special purpose registers

