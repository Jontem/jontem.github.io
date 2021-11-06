---
title: x86 CPU registers
description: In this post I will briefly explain some of the x86 CPU registers.
date: 2021-11-06T00:00:00+00:00
layout: post
permalink: /x86-cpu-registers/
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

These registers are used for inputs and outputs for different CPU instruction. For example in the application binary interface(ABI) for linux 64-bit the `rax` register is used for specifying the syscall number and it's also used for storing the return value of functions. The arguments for a function is then stored in order `rdi`(arg1), `rsi`(arg2), `rdx`(arg3) and so on. If there are more arguments than six they are stored on the stack.

The image below shows the calling conventions for syscalls on different CPU architectures for linux 64-bit.

![Linux 64-bit ABI](/assets/linux_abi.png)

## Special purpose registers

While the values stored in a general purpose register has no special meaning to the processor, special purpose registers holds state about the program. 

Here are examples of special purpose registers:

* rsp - Stores the memory adress to the top of the stack
* rbp - Stores the memory adress to the current stack frame
* rip - Stores the memory adress of the next instruction to execute
* rflags - Here the CPU stores information about the result of arithmetic instructions. For example comparing numbers.