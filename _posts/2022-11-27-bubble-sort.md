---
title: Bubble sort
date: 2022-11-27T00:00:00+00:00
layout: post
permalink: /bubble-sort/
categories:
  - algorithms
tags:
  - sorting
  - algorithms
---

Bubble sort is a simple sorting algorithm that is sometimes referred to as `sinking sort`. The name bubble comes from the fact that elements bubbles to the top of the dataset. The name sinking sort comes from the fact that some elements sink to the bottom of the set. 

It works by going through each element in a list and swapping the current element with the next element if the current element is the largest. This process repeats until there was a pass without a swap.

```
// Starting
[3, 4, 1, 2]

// First pass, first iteration.
// 3 is not greater than 4, So we move on
[3, 4, 1, 2] -> [3, 4, 1, 2]

// First pass, second iteration
// 4 is greater than 1, so we swap
[3, 4, 1, 2] -> [3, 1, 4, 2]

// First pass, third iteration
// 4 is greater than 2, so we swap
[3, 1, 4, 2] -> [3, 1, 2, 4]

// Second pass, first iteration
// 3 is greater than one, so we swap
[3, 1, 2, 4] -> [1, 3, 2, 4]

// Second pass, second iteration
// 3 is greater than two, so we swap
[1, 3, 2, 4] -> [1, 2, 3, 4]

// Second pass, third iteration
// nothing more to do in this pass
[1, 3, 2, 4]

// Third pass, first iteration
// This pass wont swap any numbers
// So we're done
[1, 3, 2, 4]
```

Bubble sort performs poorly and has a worst case scenario of O(n<sup>2</sup>)

An implementation in typescript

```ts
const arr = [11, 10, 3, 4, 1, 7, 9, 2];
console.log(sort(arr.concat(), "simple"));
console.log(sort(arr.concat(), "optimized"));

export function sort(
  arr: Array<number>,
  optimized: "simple" | "optimized"
): Array<number> {
  if (optimized === "simple") {
    bubbleSort(arr);
    return arr;
  } else {
    optimizedBubbleSort(arr);
    return arr;
  }
}

function bubbleSort(arr: Array<number>): void {
  let swapped;
  do {
    swapped = false;
    for (let i = 0; i < arr.length; i++) {
      if (arr[i - 1] > arr[i]) {
        swap(arr, i - 1, i);
        swapped = true;
      }
    }
  } while (swapped);
}

// First pass the largest element will be last
// which means that we could skip that position next pass
// Second pass the second largest will be second last and
// the process repeats...
function optimizedBubbleSort(arr: Array<number>): void {
  let swapped;
  let n = arr.length;
  do {
    swapped = false;
    let newN = 0;
    for (let i = 0; i < n; i++) {
      if (arr[i - 1] > arr[i]) {
        swap(arr, i - 1, i);
        swapped = true;
        newN = i;
      }
    }
    n = newN;
  } while (swapped);
}

function swap(arr: Array<number>, i1: number, i2: number): void {
  const tmp = arr[i1];
  arr[i1] = arr[i2];
  arr[i2] = tmp;
}

```