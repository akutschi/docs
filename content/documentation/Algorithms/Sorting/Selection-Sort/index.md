---
title: "Selection Sort"
date: 2020-05-09T10:45:20-04:00
categories:
    - algorithms
tags:
    - sorting
draft: false
---

Selection sort is a simple in-place comparison-based sorting algorithm which is inefficient on large lists. Generally it performs worse than [insertion sort]({{< ref "../Insertion-Sort/index.md" >}}).

But it has some performance advantages over more complicated algorithms in some situations, particularly where auxiliary memory is limited.

## Example

{{<table>}}
| # | Sorted Sublist       | Unsorted Sublist     | Least Element in Unsorted Sublist |
|---|----------------------|----------------------|-----------------------------------|
| 0 | ( )                  | (11, 25, 12, 22, 64) | 11                                |
| 1 | (11)                 | (25, 12, 22, 64)     | 12                                |
| 2 | (11, 12)             | (25, 22, 64)         | 22                                |
| 3 | (11, 12, 22)         | (25, 64)             | 25                                |
| 4 | (11, 12, 22, 25)     | (64)                 | 64                                |
| 5 | (11, 12, 22, 25, 64) | ( )                  |                                   |
{{</table>}}

## Explanation

This algorithm divides the input list into two parts:

- a sorted list, which is at the beginning empty and
- an unsorted list which equals the input list at the beginning.

The algorithm finds the smallest value in the unsorted list and moves it to the sorted list. The boundary of the sorted list will move then one element to the right. This goes on until the unsorted list is empty.

To find the smallest the algorithm iterates over every element of the unsorted array and this makes this algorithm very slow on large lists.

## Complexity / Analysis / Running Time

Searching for the minimum requires scanning %$n%$ elements - %$n-1%$ comparisons - and then swapping it into the first position of the array. Finding the next lowest minimum requires %$n-1%$ elements and so on. The total number of comparisons is then:

$$
(n-1)+(n-2)+...+1 = \sum^{n-1}_{i=1} i = \frac{(n-1)+1}{2}(n-1) = \frac{1}{2}n(n-1) = \frac{1}{2}(n^2-n)
$$

Which is then of complexity:

$$ 
O(n^2)
$$

## Relation to other Sorting Algorithms

 Insertion sort is very similar in that after the %$k%$-th iteration, the first %$k%$ elements in the array are in sorted order. Insertion sort's advantage is that it only scans as many elements as it needs in order to place the %$k+1%$-st element, while selection sort must scan all remaining elements to find the %$k+1%$-st element. 

On average insertion sort will perform about half as many comparisons as selection sort on average. If the input array is reverse-sorted, insertion sort performs just as many comparisons as selection sort. 

Selection sort requires less writes than insertion sort since only a single swap is required. In general, insertion sort will write to the array %$O(n^2)%$ times, whereas selection sort will write only %$O(n)%$ times. For this reason selection sort may be preferable in cases where writing to memory is more expensive than reading, such as with EEPROM or flash memory. 

## Pseudocode

```plaintext
Step 1 − Set MIN to location 0
Step 2 − Search the minimum element in the list
Step 3 − Swap with value at location MIN
Step 4 − Increment MIN to point to next element
Step 5 − Repeat until list is sorted
```

<!--
## Implementation
-->

## Links & Resources

- [Wikipedia: Selection Sort](https://en.wikipedia.org/wiki/Selection_sort)
