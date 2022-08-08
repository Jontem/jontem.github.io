---
title: What is Big O notation
date: 2022-05-15T00:00:00+00:00
layout: post
permalink: /what-is-big-o-notation/
categories:
  - computer science
tags:
  - computer science
  - algorithms
  - theory
---

The Big O notation is a simplified method for describing an algorithms effiency. It's not measured in absolute time but in a functions growth rate. It looks at the size of the input of an function and classifies the run time or space requirements. 

Two functions with the same growth rate is represented by the same Big O notation.
## Constant time O(1)
An algorithm that isn't dependenant on the size of the input executes at instant time. In Big O notation this is written as `O(1)` and pronounced as `Big O of one`. An example of constant time operation is looking up a value in a hashmap by a key.

## Logarithmic time O(log n)
A binary search algorithm is done in logarithmic time.

## Linear time O(N)
An example of linear time operation is looking up something in a unsorted array.

## Quadratic time O(N^2)
Simple sorting algorithms like bubble sort is done in quadratic time.

## Exponential time O(c^2)
An exact solution to the travelling salesman problem using dynamic programming is done in exponential time

## Rules
* The largest term wins all others can be omitted. If a algorithm has two terms `O(N)` and `O(N^2)`. Then `O(N^2)` wins.
* If a term is containts a constant the constant is removed.
