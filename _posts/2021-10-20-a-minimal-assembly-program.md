---
title: A minimal assembly program
description: In the previous post I described how to compile a hello world program. In this post i will go through and even smaller program.
date: 2021-10-29T00:00:00+00:00
layout: post
permalink: /minimal-assembly-program/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

This is the second post about x86 assembly. In the previous post I described how to compile a hello world program. In this post i will go through and even smaller program.

## Minimal example

```asm
          global    _start

          section   .text
_start:   mov       rax, 60                 ; system call for exit
          mov       rdi, 0                  ; exit code 0
          syscall                           ; invoke operating system to exit
```

This programs doesn't print any output and only sets an exit code. If the program doesn't set an exit code the program will crash with a `Segmentation fault` error.

## Sections
There is only one section in this program called `.text`. Here you put your source code which is readonly when the program is ran. Sections is used by the assembler to group data and code in different locations in memory. For example in the `hello world` example we had a section called `.data` which we initialized a constant sequence of bytes.

## Labels
There is also only one label in this program called `_start`. A label can be seen as pointer to an instruction in memory. A label can then be used as a reference in code to run a specific block of code(almost like a function call). The label `_start` has a special meaning for the linker `ld` and that is where an executable will start executing instructions.

## Global keyword
The global keyword tells the assembler that this label should be able to be referenced outside of this assembly file. It's mostly like exporting a function in high level languages.

## Instructions
The syntax for operations is `opcode <operand>, <operand>`. We first have an opcode which tells the cpu what we want to do and with which operands.

The instruction `mov rax, 60` tells the CPU to move the decimal constant value 60 to a CPU general-purpose register called `rax`. When an operand is constant value like above it's called immediate addressing. CPU registers are small high-speed memory locations in the CPU.

## Syscall
The syscall is a special operation that tells the CPU that the program wants to talk to the operating system. The program halts and the operating system takes over and reads the `rax` register for the syscall number which in linux 64bit is the syscall for `exit`. The `rdi` register where we put `0` holds the value for the exit code. When a syscall is done the program usually resumes executing but not in this case as we told the operating system that we wanted to exit.