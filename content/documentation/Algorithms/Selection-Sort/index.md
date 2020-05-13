---
title: "Selection Sort"
date: 2020-05-09T10:45:20-04:00
categories:
    - algorithms
tags:
    - sort algorithm
draft: false
---

Selection sort is a simple in-place comparison-based sorting algorithm which is inefficient on large lists. Generally it performs worse than [insertion sort]({{< ref "../Insertion-Sort/index.md" >}}).

But it has some performance advantages over more complicated algorithms in some situations, particularly where auxiliary memory is limited.

## Example

| # | Sorted Sublist       | Unsorted Sublist     | Least Element in Unsorted Sublist |
|---|----------------------|----------------------|-----------------------------------|
| 0 | ( )                  | (11, 25, 12, 22, 64) | 11                                |
| 1 | (11)                 | (25, 12, 22, 64)     | 12                                |
| 2 | (11, 12)             | (25, 22, 64)         | 22                                |
| 3 | (11, 12, 22)         | (25, 64)             | 25                                |
| 4 | (11, 12, 22, 25)     | (64)                 | 64                                |
| 5 | (11, 12, 22, 25, 64) | ( )                  |                                   |

## Explanation

This algorithm divides the input list into two parts:

- a sorted list, which is at the beginning empty and
- an unsorted list which equals the input list at the beginning.

The algorithm finds the smallest value in the unsorted list and moves it to the sorted list. The boundary of the sorted list will move then one element to the right. This goes on until the unsorted list is empty.

To find the smallest the algorithm iterates over every element of the unsorted array and this makes this algorithm very slow on large lists.

## Complexity / Analysis / Running Time

Worst-case performance

$$ 
O(n^2)
$$

## Relation to other Sorting Algorithms

## Pseudocode

```plaintext
Step 1 − Set MIN to location 0
Step 2 − Search the minimum element in the list
Step 3 − Swap with value at location MIN
Step 4 − Increment MIN to point to next element
Step 5 − Repeat until list is sorted
```

## Implementation
