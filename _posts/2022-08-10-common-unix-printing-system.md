---
title: Common Unix Printing System(CUPS)
date: 2022-08-10T00:00:00+00:00
layout: post
permalink: /common-unix-printing-system/
categories:
  - Printing
tags:
  - Linux
  - IPP
  - AirPrint
---

## Overview
CUPS is a modular printing system for unix-like operating systems which acts as a print server. A CUPS host accepts jobs from clients and sends them to the appropriate printer.

It's designed around the idea of a central printing process that handles print jobs, process administrative commands, provides printer status for local and remote programs.

There are two version of CUPS
- Apple CUPS (Used on macOS and iOS)
- OpenPrinting CUPS (For all Operating Systems)

With CUPS you can take almost any printer and get a network printer which supports driverless printing. It supports both IPP Everywhere and AirPrint

## CUPS components

### Scheduler
The scheduler is a HTTP and IPP Server. It's used for both browsing and handling IPP requests. To provide dynamic web interfaces it uses CGI. 

### Filter
A filter in CUPS converts job files into a printable format. Multiple filter can be run as needed to convert from the job file to the printable format needed by the printer. PostScript Printer Description(PPD) files can be used to register additional filters. Printer vendors often supplies CUPS PPD files for linux and there is often easy to find open source drivers.
### Backend
A backend in CUPS sends print data to the printer and enumerates available printers.

CUPS has included backend support for
- AppSocket (JetDirect)
- Internet Printing Protocol (IPP)
- Line Printer Daemon (LPD)
- Universal Serial Bus (USB)

