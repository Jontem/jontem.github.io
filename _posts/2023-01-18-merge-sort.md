---
title: Merge sort
date: 2023-01-18T00:00:00+00:00
layout: post
permalink: /merge-sort/
categories:
  - algorithms
tags:
  - sorting
  - algorithms
---

Merge sort is a divide-and-conquer algorithm. It's an effecient general-purpose and comparison-based sorting algorithm. The average and worst-case performance is O(n logn).

It works by dividing the unsorted list into `n` sublists. When there is only element in the list that list is considered sorted. Sublists are then merged to produce new sorted sublists. When there is only one sublist left the list is sorted. 

Merge sort can be implemented with an Top-down or a bottom-up algorithm. Here is a top-down implementation in typescript.

```ts
const arr = [10, 9, 1, 7, 4, 2, 6, 5, 8, 3];
const sorted = sort(arr);
console.log(JSON.stringify(sorted));

export function sort(arr: ReadonlyArray<number>): ReadonlyArray<number> {
  return mergeSort(arr.concat());
}

function mergeSort(arr: Array<number>): Array<number> {
  if (arr.length <= 1) {
    return arr;
  }
  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  return merge(left, right);
}

function merge(left: Array<number>, right: Array<number>): Array<number> {
  const result: Array<number> = [];
  while (left.length || right.length) {
    if (left.length && right.length) {
      if (left[0] < right[0]) {
        result.push(left.shift()!);
      } else {
        result.push(right.shift()!);
      }
    } else if (left.length) {
      result.push(left.shift()!);
    } else {
      result.push(right.shift()!);
    }
  }

  return result;
}
```
