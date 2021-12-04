---
title: Debug x86 assembly with GDB
description: This is the sixth post about x86 assembly. In this post I will show how to debug your x86 assembly code with the GNU Project debugger(GDB)
date: 2021-12-04T00:00:00+00:00
layout: post
permalink: /debug-x86-assembly-with-gdb/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

This is the sixth post about x86 assembly. In this post I will show how to debug your x86 assembly code with the GNU Project debugger(GDB). GDB can be used with a lot of languages for example assembly, C, C++ and Rust.

## Generating debug symbols
First of all you need to tell nasm to generate debug information. This is done with `-g` and `-F dwarf`. The `-g` option tells nasm to generate debug information and the `-F dwarf` sets the debugging format to the standardized debugging format `DWARF`.

## Start GDB
Start gdb with `gdb <your_assemlby_program>` to enter its interactive shell. You can set breakpoints by writing `break <function_name or line number>`. By default gdb uses AT&T flavor to change to intel flavor type `set dissambly-flavor intel`. To start debugging the program type `run`. 

## Debugging the program
The program will run until a crash happens or to any breakpoint you set and pause. If you want to look at the values currently in the registers type `info registers` or if you want to see the next instructions at the same time as the value of the registers type `layout regs`.

To execute the next instruction type `next`. If you want to step into a function call type `step`. If the program crashes gdb will tell you which line that was the last one executing. You can type `backtrace` to see the full stack trace
