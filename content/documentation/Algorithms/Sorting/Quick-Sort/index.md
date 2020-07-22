---
title: "Quick Sort"
date: 2020-05-25T21:02:35-04:00
categories:
    - algorithms
tags:
    - sorting
    - divide & conquer
draft: false
---

The **quick sort** algorithm is an efficient sorting algorithm utilizing the divide and conquer paradigm. It is often superior to [merge sort]({{< ref "../Merge-Sort/index.md" >}}) and for this reason the default sorting algorithm in many libraries.

The big advantage of quick sort is that it runs in-place, there are only repeated swap operations on the input array. Therefore only a small amount of additional memory for computations are required.

This algorithm works by selecting a pivot element from the array. The remaining elements will be partitioned into two sub-arrays, according to whether they are less than or greater than the pivot. There are several ways to pick a pivot:

- Always pick first element as pivot. 
- Always pick last element as pivot.
- Pick a random element as pivot.
- Pick median as pivot.

Quick sort is also a comparison sort, this means that any type of of data for which a _less-than_ relation is defined can be sorted.

## Example

This example can be found in Tim Roughgarden's book[^timbook]. The main difference to _Introduction To Algorithms_ by CLRS[^clrs] is that the latter one uses the last element as pivot. But Roughgarden uses the first element as the pivot in his example.

[^timbook]: Algorithms Illuminated: Part1 The Basics; see [links and resources](#links--resources)
[^clrs]: Thomas H. Cormen; Charles E. Leiserson; Ronald L. Rivest; Clifford Stein (2009). Introduction To Algorithms (3rd ed.); see [links and resources](#links--resources)

We keep track of two boundaries: 

1. The first boundary %$j%$ separates the non-pivot elements that were already checked.
1. The second boundary %$i%$ separates within the first group the elements that are bigger and smaller than the pivot element.

The desired **invariant** is then that all elements between the pivot and %$i%$ are smaller than the pivot and all elements between %$i%$ and %$j%$ are greater than the pivot.

```plaintext
Unsorted array
            -------------------------------------------------
            |  3  |  8  |  2  |  5  |  1  |  4  |  7  |  6  |
            -------------------------------------------------  
                 i,j

    - Pivot is the first element - the number 3.
    - Boundaries i and j are initialized between the pivot and the rest that is 
      currently unpartitioned.

Increment j
            -------------------------------------------------
            |  3  |  8  |  2  |  5  |  1  |  4  |  7  |  6  |
            -------------------------------------------------  
                  i     j

    - Invariant holds, the 8 is greater than the pivot.

Increment j
            -------------------------------------------------
            |  3  |  8  |  2  |  5  |  1  |  4  |  7  |  6  |
            -------------------------------------------------  
                  i           j
    
    - Invariant is violated since the 2 is less than the pivot.
    - Swap the 8 and the 2 to restore the invariant.
    - Also increment i to move the boundary between processed elements less and
      greater than the pivot.

            -------------------------------------------------
            |  3  |  2  |  8  |  5  |  1  |  4  |  7  |  6  |
            -------------------------------------------------  
                        i     j

Increment j
            -------------------------------------------------
            |  3  |  2  |  8  |  5  |  1  |  4  |  7  |  6  |
            -------------------------------------------------  
                        i           j

    - Invariant holds, the 5 is greater than the pivot.  

Increment j
            -------------------------------------------------
            |  3  |  2  |  8  |  5  |  1  |  4  |  7  |  6  |
            -------------------------------------------------  
                        i                 j
        
    - Invariant is violated since the 1 is less than the pivot.
    - Swap the 8 (first element after the pivot) and the 1 to restore the invariant.
    - Also increment i to move the boundary between processed elements less and
      greater than the pivot.

            -------------------------------------------------
            |  3  |  2  |  1  |  5  |  8  |  4  |  7  |  6  |
            -------------------------------------------------  
                              i           j

Increment j
            -------------------------------------------------
            |  3  |  2  |  1  |  5  |  8  |  4  |  7  |  6  |
            -------------------------------------------------  
                              i                 j

    - Invariant holds, the 4 is greater than the pivot. 

Increment j
            -------------------------------------------------
            |  3  |  2  |  1  |  5  |  8  |  4  |  7  |  6  |
            -------------------------------------------------  
                              i                       j

    - Invariant holds, the 7 is greater than the pivot. 

Increment j
            -------------------------------------------------
            |  3  |  2  |  1  |  5  |  8  |  4  |  7  |  6  |
            -------------------------------------------------  
                              i                             j

    - Invariant holds, the 6 is greater than the pivot. 
    - Partitioning is finished.

Move pivot 
            -------------------------------------------------
            |  1  |  2  |  3  |  5  |  8  |  4  |  7  |  6  |
            -------------------------------------------------  
                              i                             j
```

As we can see the first part is already sorted, which is a coincidence. The second part is still unsorted, here we just repeat the quick sort algorithm again.

## Explanation

In principal quick sort is built around a subroutine for partial sorting. The task of this subroutine is to partition an array around a pivot element, as we have seen in the [example](#example) above.

So there are now two tasks:

1. Choose a pivot element, and as we know there are several methods to pick a pivot.
    - In the example we simply used the first element of the array as pivot.
1. Rearrange the input array elements around the pivot. All elements that greater than the pivot will be sorted after the pivot and all elements that are less than the pivot will be sorted before the element.

The advantages of this subroutine are:

1. It's fast, since its running in **linear time** - %$O(n)%$.
1. This subroutine runs **in place** without using memory beyond that is occupied by the input array.
1. Significant progress, since the partitioning around the pivot makes progress toward sorting the array. 
    - The pivot element ends in the same position as in the sorted array.
    - The partitioning reduces the sorting task into two smaller sorting problems.
    - After recursively sorting the elements of the subarrays the algorithm is done and the array is sorted.

### Choosing Pivot

For quick sort to be quick it is crucial that good pivot elements are chosen. A pivot element is good, when the resulting subproblems are roughly the same size. [Merge sort]({{< ref "../Merge-Sort/index.md" >}}) already needs just %$O(n\log n)%$ time, which sets the bar very high.

There are four possibilities for choosing the pivot:

1. Always use the first element as pivot.
1. Always use the last element as pivot.
1. Use the median element as pivot.
1. Use a random element as pivot.

Two very simple implementations for choosing the pivot are always picking the first or the last element. See also the relevant [pseudocode](#pseudocode). The complexity will be discussed in the [next section](#complexity--analysis--running-time).

The third possibility is to compute the median and choosing the corresponding value as pivot. Though still linear in time, this approach is time-consuming. Nevertheless, the advantage is that the number of entries in each subproblem is perfectly balanced.

The first two approaches can lead to a worst-case running time of %$O(n^2)%$, though choosing the pivot takes either %$O(1)%$ or %$O(n)%$. The third approach guarantees an overall running time of %$O(n\log n)%$ but the computation of the median is computational expensive.

The fourth approach is a **randomized quick sort** and the simplest and very effective way to incorporate this is to always choose pivot elements uniformly at random.

## Complexity / Analysis / Running Time

TODO:
- TR 5.3.1, 5.3.2, 5.4.2 & 5.5
- CLRS 7.2 & 7.4
- Wikipedia

## Pseudocode

The input for `QuickSort` is an array A of n distinct integers with left and right endpoints %$l,r \in \\{ 1,2,...,n \\}%$. Afterwards the elements of the subarray %$A[l],A[l+1],...,A[r]%$ are sorted from smallest to largest.

```plaintext
QuickSort(InputArray A)
    if l >= r then                  // Base case, 0- or 1-element subarray
        return
    i = ChoosePivot(A,l,r)          // Method to get pivot
    swap A[l] and A[i]              // Make pivot first
    j = Partition(A,l,r)            // New pivot position
    QuickSort(A,l,j-1)              // Recurse on 1st part
    QuickSort(A,j+1,r)              // Recurse on 2nd part
```

The input for the function `Partition` is also an array of n distinct integers with left and right endpoints %$l,r \in \\{ 1,2,...,n \\}%$ and %$l\leq r%$. Afterwards the elements of the subarray  %$A[l],A[l+1],...,A[r]%$ are partitioned around %$A[l]%$. The output will be the final position of the pivot.

```plaintext
Partition(A,l,r)
    p = A[l]                        // Set pivot
    i = l+1                         // Boundary between partitioned subarrays
    for j = l+1 to r do             // Boundary between partitioned and unpartitioned subarrays
        if A[j] < p then
            swap A[j] and A[i]
            i = i+1                 // Restores invariant
    swap A[l] and A[i-1]            // Place pivot correctly
    return i-1                      // Return final position of the pivot
```

The last part of the pseudocode for this algorithm is the `ChoosePivot` function. This part is crucial for the overall performance of the quick sort algorithm. The naive implementation would return either the leftmost element or the rightmost element. And as we already know we could also compute the median element, though this would be overkill.

The best option is to implement a randomized version of `ChoosePivot`:

```plaintext
ChoosePivot(A,l,r)
    return l        // Return leftmost element from the array as pivot
                    // `return r` would return the rightmost element as pivot
                    // `return m` would return the median element of {A[l],...A[r]}
                    // `return r` would return an element of {A[l],...A[r]} chosen 
                    // uniformly at random
```

<!--
## Implementation
-->

## Links & Resources

- Thomas H. Cormen; Charles E. Leiserson; Ronald L. Rivest; Clifford Stein (2009). Introduction To Algorithms (3rd ed.). MIT Press. ISBN 978-0-262-03384-8.
- Tim Roughgarden (2017). Algorithms Illuminated: Part1 The Basics. Soundlikeyourself Publishing, ISBN: 978-0999282908
- https://www.geeksforgeeks.org/quick-sort/
- [Wikipedia: Quicksort](https://en.wikipedia.org/wiki/Quicksort)
