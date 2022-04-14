---
title: Using dotnet-dump
date: 2022-04-14T00:00:00+00:00
layout: post
permalink: /using-dotnet-dump/
categories:
  - dotnet
tags:
  - dotnet
  - dotnet-dump
---

## What it does
`dotnet-dump` is a tool for collecting and analyzing `.NET` dumps. With `dotnet-dump` you can analyze crashes and the managed heap.

## Installing
The easiest way to install `dotnet-dump` is to curl the binary.
```sh
$ curl -L -o dotnet-dump https://aka.ms/dotnet-dump/linux-x64 && chmod 755 dotnet-dump
$ ./dotnet-dump collect --process-id 1
$ ./dotnet-dump analyze file
```

## To collect and start analyze
```sh
# When collect is done the filname is printed to the console
$ ./dotnet-dump collect --process-id 1
$ ./dotnet-dump analyze file
```

## Commands
Below is a list of commands that i find useful

### eeheap -gc
Shows information about the managed heap. For example number of heaps, where each generation starts and ends in a heap, size of the generations.

```sh
$ eeheap -gc
```

### dumpheap
Shows all the objects on the heap. With the `-stat` parameter it will display a summary of number objects and the size of all the method tables
```sh
$ dumpheap
$ dumpheap -stat

# Shows all the objects of a method table
dumpheap -mt <method_table>
```

### dumpgen <gen1,gen2,gen3,loh>
This command works like `dumpheap -stat` but only for the generation you specify
```sh
# Show stats about large object heap
$ dumpgen loh
```

### dumpgen <gen1,gen2,gen3,loh> -mt <method_table>
This shows all the object of a specific method table in the specified generation
```sh
$ dumpgen loh -mt <memory_address>
```

### gcroot -all
This commands takes a memory address as a parameter. It finds all roots to the specified object.
```sh
$ gcroot -all <memory_address>
```

### d
The `d` command reads the memory address and tries to display the contents

```sh
$ d <memory_address>
```