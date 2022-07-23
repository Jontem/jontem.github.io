---
title: PostScript
date: 2022-07-23T00:00:00+00:00
layout: post
permalink: /postscript/
categories:
  - Printing
tags:
  - PostScript
  - CUPS
  - PPD
---

PostScript is a page description language optimized for printing graphics and text developed by Adobe. The programming language is a dynamically typed and concatenative. Portable Document Format(PDF) is largely based on PostScript.

A PostScript program can be sent to an interpreter which in the case of a printer will print the document.

## PostScript Printer Description file
To describe a printers features and capabilities a vendor creates a PostScript Printer Description(PPD) file. A ppd file works as a driver for PostScript printers as it also can contain PostScript code for invoking feature of the printer.

## CUPS and PPD
Common Unix Printing System(CUPS) uses PPD drivers for all PostScript and non-PostScript printers. A CUPS-ppd file is not a standard ppd file because it adds attributes and extensions. For example `cupsFilter` defines a conversion rule which takes a mime type as input and a conversion program to run. The program in this case can be a vendor specific driver binary.


