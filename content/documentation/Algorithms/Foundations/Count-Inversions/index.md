---
title: "Count Inversions"
date: 2020-05-11T20:17:05-04:00
categories:
    - algorithms
tags:
    - sorting
    - divide & conquer
draft: false
---
 
An inversion is a pair of elements in an array that is _out of order_. This means that the element that occurs first is bigger than the element that occurs later. The two possibilities to count these inversions are a **brute-force** method and an algorithm based on [merge sort]({{< ref "../../Sorting/Merge-Sort/index.md" >}}) which is a __divide & conquer__ algorithm.

The number of inversions can be used as **similarity measure** that quantifies how similar two list are. This information can be used as a [recommender system](https://en.wikipedia.org/wiki/Recommender_system). For example Netflix tries to match someones movie preferences with others. To achieve this someone has to rank a number of movies. Netflix compares this ranking with its database to find people with similar tastes. On success they can recommend movies that the other people with similar preferences also liked.

The general idea of this [collaborative filtering](https://en.wikipedia.org/wiki/Collaborative_filtering) is to find other users with the same preferences and recommend things that have been popular with them.

## Example

This table shows the ranking of 5 movies from 3 people.

{{<table>}}
| Person/Movie | A | B | C | D | E |
|--------------|---|---|---|---|---|
| Person 1     | 1 | 2 | 3 | 4 | 5 |
| Person 2     | 1 | 3 | 4 | 2 | 5 |
| Person 3     | 1 | 3 | 2 | 4 | 5 |
{{</table>}}

We can see that the row 'Person 2' has two inversion, namely 3-2 and 4-2. The row 'Person 3' has just one inversion, namely 3-2. Hence, the taste of person 3 is more similar to the taste of person 1 than the taste of person 3 to person 1's taste.

## Explanation

This algorithm shows the number of elements those successors are smaller. For example the array `1, 3, 4, 2, 5` has as we already know two inversions.

To get them we use now the [brute force approach](#brute-force). In the first column we compare the first element, the `1`, with all successors:

{{<table>}}
| 1st element | 2nd element | 3rd element | 4th element |
| :---------- | :---------- | :---------- | :---------- |
| 1 < 3 = ok  | 3 < 4 = ok  | 4 < 2 = inv | 2 < 5 = ok  |
| 1 < 2 = ok  | 3 < 2 = inv | 4 < 5 = ok  |             |
| 1 < 4 = ok  | 3 < 5 = ok  |             |             |
| 1 < 5 = ok  |             |             |             |
{{</table>}}

After finishing all comparisons we see that the `<`-condition is violated twice - the number of inversions.

The largest possible number of inversions in a n-element array is:

$$ {n\choose 2} = \frac{n(n-1)}{2}$$

This is given in an array where all elements are in reversed order. Assume the array `5, 4, 3, 2, 1` with 5 elements:

$$ {5\choose 2} = \frac{5(5-1)}{2} = 10 $$

This array has ten inversions, check this result with the table:

{{<table>}}
| 1st element | 2nd element | 3rd element | 4th element |
| :---------- | :---------- | :---------- | :---------- |
| 5 < 4 = inv | 4 < 3 = inv | 3 < 2 = inv | 2 < 1 = inv |
| 5 < 3 = inv | 4 < 2 = inv | 3 < 1 = inv |             |
| 5 < 2 = inv | 4 < 1 = inv |             |             |
| 5 < 1 = inv |             |             |             |
{{</table>}}

As we can see there are 10 inversions - the maximum possible number of inversion of this 5-element array.

## Complexity / Analysis / Running Time

The _brute force approach_ is of time-complexity of 

$$ O(n^2) $$

since two nested loops are required to traverse the array from start to end. 

The complexity of the _divide and conquer approach_ can be calculated with the [master theorem]({{< ref "../../Foundations/Master-Theorem/index.md" >}}). To use the formula 

$$
T(n) \leq a \cdot T \left( \frac{n}{b} \right) + O(n^d)
$$

three parameters are required. **a** the branching factor, **b** the shrinkage factor and **d** the exponent of the running time of the combine step.

In this **sort and count inversions algorithm** are two recursive calls, so the branching factor is %$a=2%$. Each recursive call receives half of the input, therefore the shrinkage factor is %$b=2%$. The work that is done outside these recursive calls is dominated by the merge subroutine and runs in linear time, so %$d=1%$

$$
T(n) \leq 2 \cdot T \left( \frac{n}{2} \right) + O(n)
$$

With the now known three parameters the [master theorem]({{< ref "../../Foundations/Master-Theorem/index.md" >}}) can be applied. 

$$
T(n) =
\begin{cases}
O(n^d \log n),  & \text{if $a=b^d$} \\\\
O(n^d), & \text{if $a < b^d$} \\\\
O(n^{\log_b a}),  & \text{if $a>b^d$}
\end{cases}
$$

To get the correct case just insert the parameters. We will see then that the first case is the correct result:

$$ a=b^d \Rightarrow O(n \log n) $$

The sort and count inversions algorithm has - like merge-sort - a worst-case performance of

$$
O(n \cdot \log n)
$$


## Pseudocode

### Brute Force

```plaintext
countInversion(array)
{
    inversions = 0
    for(i = 1 to size_of_array-1)
    {
       for(j = i+1 to size_of_array-1)
       {
          if(array[j] < array[i])
          inversions = inversions+1
       }
    }
    return inversions
}
```

### Divide and Conquer Approach

```plaintext
func mergesort( var a as array )
     if ( n == 1 ) return a

     var l1 as array = a[0] ... a[n/2]
     var l2 as array = a[n/2+1] ... a[n]

     l1 = mergesort( l1 )
     l2 = mergesort( l2 )

     return merge( l1, l2 )
end func

func merge_count_inversions( var a as array, var b as array )
     var c as array

     var ic as inversion_counter

     while ( a and b have elements )
          if ( a[0] > b[0] )
               add b[0] to the end of c
               add number of left elements in a to ic     <-- Difference to pure merge-sort
               remove b[0] from b
          else
               add a[0] to the end of c
               remove a[0] from a
     while ( a has elements )
          add a[0] to the end of c
          remove a[0] from a
     while ( b has elements )
          add b[0] to the end of c
          remove b[0] from b
     return c and ic                                      <-- Difference to pure merge-sort
end func
```

## Implementation

### C++

```cpp
// C++ merge sort implementation

#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <math.h> // Required for std::ceil

long int inversion_counter = 0; // Global variable for counter - very bad design...
//TODO: Remove the global variable!

// Function to merge two sorted arrays
std::vector<int> merge(std::vector<int> vector1, std::vector<int> vector2)
{
    // Vector to store the result of the merge
    std::vector<int> sorted_list;

    uint i = 0; // Counter for split1
    uint j = 0; // Counter for split2

    // Compare values of both vectors and store in result vector
    while (i < vector1.size() && j < vector2.size())
    {
        if (vector1[i] <= vector2[j])
        {
            sorted_list.push_back(vector1[i]);
            i++;
        }
        else
        {
            sorted_list.push_back(vector2[j]);
            inversion_counter += vector1.size() - i; // Update global variable for inversion counter
            j++;
        }
    }
    while (i < vector1.size()) // Store remaining values in result vector
    {
        sorted_list.push_back(vector1[i]);
        i++;
    }
    while (j < vector2.size()) // Store remaining values in result vector
    {
        sorted_list.push_back(vector2[j]);
        j++;
    }

    return sorted_list;
}

// Function for the merge sort algorithm
std::vector<int> merge_sort(std::vector<int> list)
{
    int length = list.size();

    // Base case
    if (length == 1)
    {
        return list;
    }

    // Recursive case
    int center = std::ceil((float)length / 2);

    std::vector<int> split1(list.begin(), list.begin() + center); // "Left" part of the unsorted list
    std::vector<int> split2(list.begin() + center, list.end());   // "Right" part of the unsorted list

    std::vector<int> sublist1 = merge_sort(split1); // Recursive call
    std::vector<int> sublist2 = merge_sort(split2); // Recursice call
    list = merge(sublist1, sublist2);               // Call merge function

    return list;
}

// Driver code
int main()
{
    // Read the file with the unsorted numbers
    std::ifstream file_input;  // Create input stream object
    std::vector<int> unsorted; // Create vector for number that come from the file
    file_input.open("IntegerArray.txt");
    if (file_input.is_open()) // Check if file is open
    {
        std::string value; // Create a temporary variable for streaming numbers
        while (std::getline(file_input, value))
        {
            unsorted.push_back(std::stoi(value)); // Fill the vector with the values from the temporary variable
        }
    }
    else
    {
        std::cout << "File is closed." << std::endl;
    }
    file_input.close(); // Close the file

    // Call merge-sort function
    std::vector<int> sorted = merge_sort(unsorted);

    std::cout << "Number of inversions: " << inversion_counter << std::endl;

    return 0;
}
```

## Links & Resources

- Tim Roughgarden (2017). Algorithms Illuminated: Part1 The Basics. Soundlikeyourself Publishing, ISBN: 978-0999282908
- https://www.cp.eng.chula.ac.th/~prabhas//teaching/algo/algo2008/count-inv.htm
- https://www.geeksforgeeks.org/counting-inversions/
