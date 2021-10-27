---
title: Compiling your first assembly program
date: 2021-10-27T00:00:00+00:00
layout: post
permalink: /compiling-your-first-assembly-program/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

## Background
Lately I've been studying assembly language for the x86/x86_64 architecture. Assembly language is a human readable form of machine code and is as low level as it gets. Today there aren't many use cases for it as other low level languages like c and rust is fast enough. But I've always wanted to know more about what happens at the lowest level and I think it's good to have knowledge how the most primitives of the CPU and operating system works. 

I will be writing a series of post of what I've learned.

## Assembler

When you have written an assembly program you need to translate it to machine code. This is done with an assembler. There are several different assemblers but I will be using `NASM`. The Netwide assembler(NASM) is an x86 assembler that works on both on linux and windows and can output object format files like elf32/elf64 and win32/win64. 

## Linker

When the assembler has generated the object files we need to use a linker to generate and executeable file. The linker combines object files, relocates their data and resolves symbol references. I will be using `ld` which is the GNU linker.

## Hello world

This is a simple hello world example for 64-bit linux. I will explain what each row does in the next post.

```asm
;
; Example code is taken from the docs of NASM
;
          global    _start

          section   .text
_start:   mov       rax, 1                  ; system call for write
          mov       rdi, 1                  ; file handle 1 is stdout
          mov       rsi, message            ; address of string to output
          mov       rdx, 13                 ; number of bytes
          syscall                           ; invoke operating system to do the write
          mov       rax, 60                 ; system call for exit
          xor       rdi, rdi                ; exit code 0
          syscall                           ; invoke operating system to exit

          section   .data
message:  db        "Hello, World", 10      ; note the newline at the end
```

## Compile and run
First we need to turn our assembly file to an object file
```bash
$ nasm -felf64 hello.asm
```

Nasm will by default call the object file `hello.o`. Then we need to use the linker to create and executable.
```bash
$ ld hello.o
```

The default output filename of ld is `a.out` and can be run by typing.
```sh
$ ./a.out
Hello, World
```

You should get the output `Hello World`