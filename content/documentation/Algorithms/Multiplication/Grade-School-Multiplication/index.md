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

```cpp

// C++ program to multiply two numbers represented 
// as strings. 
#include<bits/stdc++.h> 
using namespace std; 
  
// Multiplies str1 and str2, and prints result. 
string multiply(string num1, string num2) 
{ 
    int len1 = num1.size(); 
    int len2 = num2.size(); 
    if (len1 == 0 || len2 == 0) 
    return "0"; 
  
    // will keep the result number in vector 
    // in reverse order 
    vector<int> result(len1 + len2, 0); 
  
    // Below two indexes are used to find positions 
    // in result.  
    int i_n1 = 0;  
    int i_n2 = 0;  
      
    // Go from right to left in num1 
    for (int i=len1-1; i>=0; i--) 
    { 
        int carry = 0; 
        int n1 = num1[i] - '0'; 
  
        // To shift position to left after every 
        // multiplication of a digit in num2 
        i_n2 = 0;  
          
        // Go from right to left in num2              
        for (int j=len2-1; j>=0; j--) 
        { 
            // Take current digit of second number 
            int n2 = num2[j] - '0'; 
  
            // Multiply with current digit of first number 
            // and add result to previously stored result 
            // at current position.  
            int sum = n1*n2 + result[i_n1 + i_n2] + carry; 
  
            // Carry for next iteration 
            carry = sum/10; 
  
            // Store result 
            result[i_n1 + i_n2] = sum % 10; 
  
            i_n2++; 
        } 
  
        // store carry in next cell 
        if (carry > 0) 
            result[i_n1 + i_n2] += carry; 
  
        // To shift position to left after every 
        // multiplication of a digit in num1. 
        i_n1++; 
    } 
  
    // ignore '0's from the right 
    int i = result.size() - 1; 
    while (i>=0 && result[i] == 0) 
    i--; 
  
    // If all were '0's - means either both or 
    // one of num1 or num2 were '0' 
    if (i == -1) 
    return "0"; 
  
    // generate the result string 
    string s = ""; 
      
    while (i >= 0) 
        s += std::to_string(result[i--]); 
  
    return s; 
} 
  
// Driver code 
int main() 
{ 
    string str1 = "1235421415454545454545454544"; 
    string str2 = "1714546546546545454544548544544545"; 
      
    if((str1.at(0) == '-' || str2.at(0) == '-') &&  
        (str1.at(0) != '-' || str2.at(0) != '-' )) 
        cout<<"-"; 
  
  
    if(str1.at(0) == '-' && str2.at(0)!='-') 
        { 
            str1 = str1.substr(1); 
        } 
        else if(str1.at(0) != '-' && str2.at(0) == '-') 
        { 
            str2 = str2.substr(1); 
        } 
        else if(str1.at(0) == '-' && str2.at(0) == '-') 
        { 
            str1 = str1.substr(1); 
            str2 = str2.substr(1); 
        } 
    cout << multiply(str1, str2); 
    return 0; 
} 
```