---
title: Calling functions in x86 assembly
description: This is the fifth post about x86 assembly. In this post I will show how to call functions in x86 assembly
date: 2021-11-23T00:00:00+00:00
layout: post
permalink: /calling-functions-in-x86-assembly/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

This is the fifth post about x86 assembly. In this post I will show how to call functions in x86 assembly.

Below is a simple program that has three functions besides main.

```asm
global main
extern printf
section   .text
main:
        mov rdi, 5
        call times2
        mov rdi, rax
        call print_value
        call exit
times2:
        push rbp
        mov rbp, rsp

        mov rax, rdi
        add rax, rax

        mov rsp, rbp
        pop rbp
        ret
print_value:
        push rbp
        mov rbp, rsp

        push rdi
        mov	rdi, fmt
        pop	rsi
        mov	rax, 0
        call printf wrt ..plt

        mov rsp, rbp
        pop rbp
        ret
exit:
        mov rdi, 0 ; exit code 0
        mov rax, 60 ; system call for exit
        syscall

section .data
fmt:    db "The value is %d", 10, 0
```

## Passing values to functions

The code above is following the Linux 64-bit ABI where the first six arguments(integers and pointers) are passed through registers. You could also pass the arguments on the stack or hardcoded memory locations.

## Function prologue and epilogue

The beginning and the end in the functions `times2` and `print_value` is called the function prologue and epilogue.

```asm
  push rbp
  mov rbp, rsp

  ...

  mov rsp, rbp
  pop rbp
  ret
```

This a convention for preparing the stack for usage. The prologue creates a new stack frame for the called function and the epilogue restores the stack frame for the calling function.
