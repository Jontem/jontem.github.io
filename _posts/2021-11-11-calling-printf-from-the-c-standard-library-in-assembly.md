---
title: Calling printf from the C standard library in assembly
description: In this post I will show an example how to call the printf function from the C standard library in assembly
date: 2021-11-11T00:00:00+00:00
layout: post
permalink: /calling-printf-from-the-c-standard-library-in-assembly/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

This is the fourth post about x86 assembly. In this post I will show an example how to call the printf function from the C standard library in assembly code.


```asm
section .text
    default rel
    extern printf
    global main
main:
    ; Create a stack-frame, re-aligning the stack to 16-byte alignment before calls
    push rbp

    mov	rdi, fmt
    mov	rsi, message
    mov	rax, 0

    ; Call printf
    call printf wrt ..plt
    
    pop	rbp		; Pop stack

    mov	rax,0	; Exit code 0
    ret			; Return
section .data
    message:  db        "Hello, World", 10, 0
    fmt:    db "%s", 10, 0
```

```bash
$ nasm printf.asm -f elf64 -o printf.o
$ gcc printf.o
$ ./a.out
# Hello world
```

## The first part
```
  default rel
  extern printf
  global main
```

The `default rel` is a nasm assembly directive. It tells nasm to use rip relative adressing. In short it tells the assembler to rewrite the references in instructions that uses our `fmt` and `message` constants relative to the instruction pointer. This is needed because default for the linker in 64-bit linux is to use position-independent code.

The `extern printf` part tells the assembler that this symbol exists outside of this file and needs to be referenced at a later stage.

Last row here is `global main` which is needed for gcc and it's the entrypoint for libc.

## main

First we need to align the stack because the x86_64 ABI requires the stackpointer to always be 16-byte aligned so therefor we push a value to it.

Then we prepare our registers for the function call to `printf` and then we call `printf wrt ..plt`. What happens here is that we load printf from the libc shared library and this is a little bit complicated. But very briefly it says `call printf with relation to procedure linkage table`. The PCL will then the first time `printf` is called resolve where `printf` is in memory with the help of the dynamic link loader in linux. It then stores that adress for future calls. We could link `printf` statically which would copy the code of `printf` into the executable.

Lastly we need to set return value of `main` to zero and do a `ret`.

## call and ret
When you do a `call` operation two things happen. First the instruction on the next instruction is pushed onto the stack. In our example above it will be the memory adress of the instruction `pop	rbp`. Secondly it will jump to the memory adress where `printf` starts. When `printf` is done it will do a `ret` instruction which will pop our memory adress from the stack and jump back to our main function.

