---
title: What is Big O noation
date: 2021-12-04T00:00:00+00:00
layout: post
permalink: /what-is-big-o-notation/
categories:
  - computer science
tags:
  - computer science
  - algorithms
  - theory
---

The Big O notation is a simplified method for describing an algorithms effiency. It's not measured in time but in a functions growth rate. Two function with the same growth rate is represented by the same Big O notation. It looks at the size of the input of an function and classifies the run time or space requirements. 

## Constant time O(1)
An algorithm that isn't dependenant on the size of the input executes at instant time. In Big O notation this is written as `O(1)` and pronounced as `Big O of one`. An example of constant time operation is thow look up a value in a hashmap by a key.

## Constant time O(1)
An algorithm that isn't dependenant on the size of the input executes at instant time. In Big O notation this is written as `O(1)` and is pronounced as `Big O of one`

## Logarithmic time O(log n)
A binary search algorithm is done in logarithmic time.

## Linear time O(N)
An example of linear time operation can be to look for something in a unsorted array.

## Exponential time O(N^2)


* Doesn't care about the computers performance
* Largest win
* Constants removed when higher complexity exists
* O(1) Constant time
* O(log n) logarithmic time
* O(N) Linear time
* O(N^2) exponential time
