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

Worst-case performance of

$$ 
O(n^2)
$$

is given in an array sorted in reverse oder. If the array is already sorted then the running time is linear %$O(n)%$. 

The average running time is also quadratic, which makes this algorithm impractical for sorting large arrays, but insertion sort os one of the fastest algorithms for sorting very small arrays, even faster than [quicksort]({{< ref "../Quick-Sort/index.md" >}}). Indeed, good implementations of quicksort will use insertion sort for arrays smaller than a certain threshold. 

## Relation to other Sorting Algorithms

This algorithm is similar to [selection sort]({{< ref "../Selection-Sort/index.md" >}}), after %$k%$ passes through the array, the first %$k%$ elements are in sorted order. The difference is that insertion sort scans backwards from the current entry, while selection sort scans forwards. 

Selection sort must scan all remaining elements to find the absolute smallest element in the unsorted part of the list, in contrast insertion sort requires only a single comparison when the %$(k + 1)%$-st element is greater than the %$k%$-th element; if the input array is already sorted or partially sorted then this is frequently true and insertion sort is more efficient than selection sort. On average insertion sort will perform about half as many comparisons as selection sort on average. If the input array is reverse-sorted, insertion sort performs just as many comparisons as selection sort. 

Insertion sort requires more writes since, on each iteration, inserting the %$(k + 1)%$-st element into the sorted portion of the array requires many element swaps to shift all of the following elements, while only a single swap is required for each iteration of selection sort. In general, insertion sort will write to the array %$O(n^2)%$ times, whereas selection sort will write only %$O(n)%$ times. For this reason selection sort may be preferable in cases where writing to memory is more expensive than reading, such as with EEPROM or flash memory. 

## Pseudocode

```plaintext
i <- 1
while i < length(A)
    x <- A[i]
    j <- i - 1
    while j >= 0 and A[j] > x
        A[j+1] <- A[j]
        j <- j - 1
    end while
    A[j+1] <- x[3]
    i <- i + 1
end while
```
<!--
## Implementation
-->

## Links & Resources

- [Wikipedia: Insertion Sort](https://en.wikipedia.org/wiki/Insertion_sort)
