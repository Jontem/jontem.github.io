---
title: next post
date: 2021-10-27T00:00:00+00:00
layout: post
permalink: /compiling-your-first-assembly-program123213/
categories:
  - assembly
tags:
  - nasm
  - x86
  - assembly
---

PostScript is a page description language which is widely used for printing. The programming language is a dynamically typed and concatenative.

A PostScript program can be sent to an interpreter(printer or other device). The results can then be rendered with that specific device.

## PostScript Printer Description file
To describe a printers features and capabilities a vendor creates a PostScript Printer Description(PPD) file. A ppd file works as a driver for PostScript printers as it also can contain PostScript code for invoking feature of the printer.

## CUPS and PPD
Common Unix Printing System(CUPS) uses PPD drivers for all PostScript and non-PostScript printers. A CUPS-ppd file is not a standard ppd file because it add attributes and extensions. For example `cupsFilter` defines a conversion rule which takes a mime type as input and a conversion program to run. The program in this case can be a vendor specific driver binary.


