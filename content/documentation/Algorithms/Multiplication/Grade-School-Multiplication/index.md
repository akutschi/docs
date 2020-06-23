---
title: "Grade-School Multiplication "
date: 2020-05-06T12:59:00-04:00
categories:
    - algorithms
tags:
    - multiplication
draft: false
---

This is the typical way taught in schools for multiplying numbers by hand in base 10 and known as **long multiplication**. The general procedure is to multiply the multiplicand with each digit of the multiplier and then sum up all properly shifted results. 

## Example

- Input: two n-digit numbers %$x%$ and %$y%$
- Output: %$x \cdot y%$
- Basic operation: adding or multiplying two single-digit numbers

```plaintext
            5678 ( = Input x           )
  ×         1234 ( = Input y           )
  ———————————————
           22712 ( =      5,678 ×     4)
          17034  ( =      5,678 ×    30)
         11356   ( =      5,678 ×   200)
  +      5678    ( =      5,678 × 1,000)
  ———————————————
         7006652 ( = 7,006,652         )
```

## Complexity / Required number of basic operations

Each partial product requires %$\leq 2n%$ basic operations where %$n%$ is the input length. With %$n%$ rows there are in total %$2n^2%$ basic operations required.

The total number of required basic operations for this algorithm is in general

$$
const \cdot n^2
$$

So it is quadratic in the input length %$n%$. The next example shows that this is also given, when multiplicand %$n%$ and multiplier %$m%$ are of different length.

```plaintext
            5678 ( = Input x           )
  ×       101234 ( = Input y           )
  ———————————————
           22712 ( =    5,678 ×       4) 
          17034  ( =    5,678 ×      30)
         11356   ( =    5,678 ×     200)
         5678    ( =    5,678 ×   1,000)
        0000     ( =    5,678 ×       0) 
  +    5678      ( =    5,678 × 100,000)
  ———————————————
       574806652 ( = 574,806,652       )
```

Per row are still %$2n%$ basic operations required. But in this case the input numbers are of different length. Nevertheless the previous statement that the number of basic operations is quadratic to the input length holds true.

We can use the information that %$m%$ is %$c_2%$ times larger then %$n%$. In our example %$c_2%$ is 1.5. 

$$
c_1 \cdot n \cdot m \Leftrightarrow c_1 \cdot n \cdot c_2 \cdot n \Rightarrow const \cdot n^2
$$

## Pseudocode

```plaintext
multiply(a[1..p], b[1..q], base)                            // Operands containing rightmost digits at index 1
  product = [1..p+q]                                        // Allocate space for result
  for b_i = 1 to q                                          // for all digits in b
    carry = 0
    for a_i = 1 to p                                        // for all digits in a
      product[a_i + b_i - 1] += carry + a[a_i] * b[b_i]
      carry = product[a_i + b_i - 1] / base
      product[a_i + b_i - 1] = product[a_i + b_i - 1] mod base
    product[b_i + p] = carry                               // last digit comes from final carry
  return product
```

## Implementation

The implementation differs slightly from the pseudocode above. The first difference is that the `base` is tied to the decimal system and the second difference is the _integrated_ carry.

```cpp
// C++ program to multiply two very large numbers

#include <iostream>
#include <vector>
#include <sstream>
#include <iterator>

// Function to multiply two numbers represented as strings
std::string multiplication(std::string num1, std::string num2)
{
    std::vector<int> num1vec;
    std::vector<int> num2vec;

    // Convert string to vector of ints
    for (size_t i = 0; i < num1.size(); ++i)
    {
        num1vec.push_back(num1[i] - '0');
    }

    // Convert string to vector of ints
    for (size_t i = 0; i < num2.size(); ++i)
    {
        num2vec.push_back(num2[i] - '0');
    }

    int len1 = num1vec.size();
    int len2 = num2vec.size();
    // Create vector for solution with maximum possible length
    std::vector<int> resultvec(len1 + len2);

    // Go from right to left in num1vec
    for (int i = len1 - 1; i >= 0; i--)
    {
        // Go from right to left in num2vec
        for (int j = len2 - 1; j >= 0; j--)
        {
            int p = num1vec[i] * num2vec[j];
            // Calculates the "carry" and keeps the results from previous calculations in mind
            resultvec[i + j] = resultvec[i + j] + (resultvec[i + j + 1] + p) / 10;
            // Calculate and store the result for position i+j+1
            resultvec[i + j + 1] = (resultvec[i + j + 1] + p) % 10;
        }
    }

    // Converts the vector to a string
    std::stringstream resulttmp;
    std::copy(resultvec.begin(), resultvec.end(), std::ostream_iterator<int>(resulttmp, ""));
    std::string result = resulttmp.str();

    return result;
}

// Driver code
int main()
{
    std::string num1 = "3141592653589793238462643383279502884197169399375105820974944592";
    std::string num2 = "2718281828459045235360287471352662497757247093699959574966967627";

    std::cout << "Result: " << multiplication(num1, num2) << std::endl;

    return 0;
}
```