---
title: "Bubble Sort"
date: 2020-05-09T10:43:26-04:00
categories:
    - algorithms
tags:
    - sorting
draft: false
---

Bubble sort is a poorly performing algorithm that repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted. 

Bubble sort should not be used if large lists are given - it should not be used at all. And it will not be efficient if the list is reverse-ordered. If the smallest value is at the end of the list then it will take %$n-1%$ passes until this value is a the very beginning of the list. On the other hand, if the largest value is at the beginning of the list then this value just needs one pass to move to it sorted position.

## Example

```plaintext
First Pass
    ( 5 1 4 2 8 ) -> ( 1 5 4 2 8 ), Algorithm compares the first two elements, and swaps since 5 > 1.
    ( 1 5 4 2 8 ) -> ( 1 4 5 2 8 ), Swap since 5 > 4
    ( 1 4 5 2 8 ) -> ( 1 4 2 5 8 ), Swap since 5 > 2
    ( 1 4 2 5 8 ) -> ( 1 4 2 5 8 ), Last two elements are already in order (8 > 5), algorithm does not swap them.
Second Pass
    ( 1 4 2 5 8 ) -> ( 1 4 2 5 8 )
    ( 1 4 2 5 8 ) -> ( 1 2 4 5 8 ), Swap since 4 > 2
    ( 1 2 4 5 8 ) -> ( 1 2 4 5 8 )
    ( 1 2 4 5 8 ) -> ( 1 2 4 5 8 )

Now, the array is already sorted, but the algorithm does not know if it is completed. The algorithm needs one whole 
pass without any swap to know it is sorted.

Third Pass
    ( 1 2 4 5 8 ) -> ( 1 2 4 5 8 )
    ( 1 2 4 5 8 ) -> ( 1 2 4 5 8 )
    ( 1 2 4 5 8 ) -> ( 1 2 4 5 8 )
    ( 1 2 4 5 8 ) -> ( 1 2 4 5 8 )
```
## Explanation

During one pass the algorithm compares two adjacent elements and swap them if required. It starts with the first and second elements, then with the second and third, and so forth. After comparing the %$n-1%$-th and %$n%$-th elements the pass is finished and the algorithm starts the next pass. If one pass sorts all elements then one more pass without any swaps is required since the algorithm does not know anything about this.

## Complexity / Analysis / Running Time

This algorithm has a worst-case and average complexity of

$$ 
O(n^2)
$$

Other %$О(n^2)%$ sorting algorithms, such as insertion sort, generally run faster than bubble sort, and are no more complex. 

The only advantage that bubble sort has over most other algorithms, but not insertion sort, is that the ability to detect that the list is already sorted  is built into the algorithm. When the list is already sorted (best-case), the complexity of bubble sort is  %$O(n)%$.

## Pseudocode

```plaintext
procedure bubbleSort(A : list of sortable items)
    n := length(A)
    swapped := true
    repeat
        swapped := false
        for i := 1 to n-1 inclusive do
            /* if this pair is out of order */
            if A[i-1] > A[i] then
                /* swap them and remember something changed */
                swap(A[i-1], A[i])
                swapped := true
            end if
        end for
    until not swapped
end procedure
```
<!--
## Implementation
-->

## Links & Resources

- [Wikipedia: Bubble Sort](https://en.wikipedia.org/wiki/Bubble_sort)
