---
title: "Insertion Sort"
date: 2020-05-09T10:45:49-04:00
categories:
    - algorithms
tags:
    - sorting
draft: false
---

Insertion sort is a simple sorting algorithm that builds the final list one item at a time. It is not very efficient on large lists than more advanced algorithms like quicksort, heapsort or [merge sort]({{< ref "../Merge-Sort/index.md" >}}).

Nevertheless there are some advantages:

- simple implementation
- fast and efficient for small data sets
- more efficient in practice than other %$O(n^2)%$ algorithms
- adaptive, takes advantage of existing order of its input
- stable, repeated elements are sorted in the same order as they appear in the input
- in-place, it does not require an auxiliary data structure, requires just a constant amount %$O(1)%$ of additional memory space
- online, can process the input as it receives it, the entire input is not required right from the start

## Example

```plaintext
Unsorted list               6   5   3   1   8   7   2   4
                            ⯆--⯅ 
Insertion Sort #1           5   6   3   1   8   7   2   4
                            ⯆------⯅
Insertion Sort #2           3   5   6   1   8   7   2   4
                            ⯆----------⯅
Insertion Sort #3           1   3   5   6   8   7   2   4
                                            ⯆--⯅
Insertion Sort #4           1   3   5   6   7   8   2   4
                                ⯆------------------⯅
Insertion Sort #5           1   2   3   5   6   7   8   4
                                        ⯆--------------⯅
Insertion Sort #6           1   2   3   4   5   6   7   8
```

## Explanation

This algorithm takes one element from the input list at each iteration and finds the location it belongs within the sorted list. This will be repeated until no elements are in the input list.

In the example in step 2 the first three numbers (3,5,6) are the sorted list and the other numbers (1,8,7,2,4) are the unsorted/input list.

Usually sorting is done in-place. The algorithm compares the first value from the unsorted list with the largest value in the sorted list. If the taken value from the input list is larger the algorithm leaves the element in place and moves to the next (Insertion Sort Step #3, 6 and 8 are already _sorted_). If the the taken value is smaller, the algorithm swaps the values until the value is in its correct position. The sorted list is now expanded and the unsorted list shrinked.

## Complexity / Analysis / Running Time

Worst-case performance

$$ 
O(n^2)
$$

## Relation to other Sorting Algorithms

## Pseudocode

## Implementation
