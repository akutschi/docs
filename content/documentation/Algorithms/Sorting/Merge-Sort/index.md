---
title: "Merge Sort"
date: 2020-05-07T18:29:29-04:00
categories:
    - algorithms
tags:
    - sorting
    - divide & conquer
draft: false
---

The merge sort algorithm is a quite old algorithm to solve the __sorting problem__ but still widely used and it is a good introduction into __divide & conquer__ algorithms. With divide and conquer the problem to solve will be break down into smaller sub-problems which will solved recursively. The results of these sub-problems will be combined somehow to get a solution for the initial problem.

It is an efficient algorithm and it is an improvement over other more obvious sort algorithms like 

- [Selection sort]({{< ref "../Selection-Sort/index.md" >}}),
- [Insertion sort]({{< ref "../Insertion-Sort/index.md" >}}),
- [Bubble sort]({{< ref "../Bubble-Sort/index.md" >}}).

## Example

We start with an unsorted array and call recursively the merge sort algorithm. In a first step we split the array in two halves. Then we split each half again, we repeat this until we have each number separated. The second step is to sort and merge until we have a sorted array.

![Merge sort algorithm diagram](1000px-Merge_sort_algorithm_diagram.svg.png)

## Explanation

An unsorted array will divided into sub-lists until the base case is reached. The base case is a list of one element and can therefore be considered sorted. The next step is merge these sub-lists until there is only one list remaining.

To merge two sub-lists

- compare the first entries of each sub-list
- the smaller entry goes as first value into the merged list
- then the next entry where the smallest value of the previous comparison was will be compared with the unmoved value of the other sub-list
- the smallest value of this comparison will be moved into the merged list as second value
- repeat this until the sub-lists are empty

```plaintext
Unsorted list                          6,5,3,1,8,7,2,4

Base cases              6     5     3     1     8     7     2     4
Merge 1                   5,6         1,3         7,8         2,4
Merge 2                       1
                          5,6           3
                              1,3
                          5,6
Sorted sub-list                1,3,5,6
Merge 3                                                2
                                                   7,8           4
                                                       2,4
                                                   7,8
Sorted sub-list                1,3,5,6                 2,4,7,8        Compare first values (1 and 2)
Merge 4                                 1
                                 3,5,6                 2,4,7,8        Compare the remaining first values (3 and 2)
                                        1,2
                                 3,5,6                   4,7,8        Compare the remaining first values (3 and 4)
                                        1,2,3
                                   5,6                   4,7,8        Compare the remaining first values (5 and 4)
                                        1,2,3,4
                                   5,6                     7,8        Compare the remaining first values (5 and 7)
                                        1,2,4,5
                                     6                     7,8        Compare the remaining first values (6 and 7)
                                        1,2,3,4,5,6
                                                           7,8        Move remaining values at the the end of the merged list
Sorted list                             1,2,3,4,5,6,7,8
```
## Complexity / Analysis / Running Time

Worst-case performance

$$
O(n \cdot \log n)
$$

## Pseudocode

```plaintext
A = 1st sorted array (length=n/2)
B = 2nd sorted array (length=n/2)
C = Output array (length=n)

i = Index of array A
j = Index of array B
k = Index of array C


for k=1 to n
    if A(i) < B(j)
        C(k) = A(i)
        i++
    else [B(j) < A(i)]
        C(k) = B(j)
        j++
end
```

## Implementation

