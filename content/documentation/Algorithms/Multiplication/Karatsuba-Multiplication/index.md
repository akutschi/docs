---
title: "Karatsuba Multiplication"
date: 2020-05-06T12:59:00-04:00
categories:
    - algorithms
tags:
    - multiplication
    - divide & conquer
draft: false
---

This multiplication algorithm is an example of a _divide and conquer algorithm_. It is faster than the [conventional multiplication algorithm]({{< ref "../Grade-School-Multiplication/index.md" >}}), see also the section about [complexity]({{< ref "#complexity" >}})

## Example

- Input: x=5678 and y=1234
- Split up the input numbers:
    - x to
        - a=56
        - b=78
    - y to
        - c=12
        - d=34

```plaintext
Step 1
  Compute           a*c = 672

Step 2
  Compute           b*d = 2652

Step 3
  Compute    (a+b)(c+d) = 134*46 = 6164

Step 4
  Compute      S3-S2-S1 = 2840

Step 5
        6720000  ( = S1 with four zeros)
           2652  ( = S2)
  +      284000  ( = S4 with two zeros )
  ———————————————
         7006652 ( = 7,006,652         )
```

## A Recursive Algorithm 

To understand the Karatsuba algorithm we go back one step. In general every number can be expressed decomposed. Two numbers %$x,y%$ can be written in the following way 

$$
\begin{aligned}
x &= 10^{n/2} a + b \\\\
y &= 10^{n/2} c + d
\end{aligned}
$$

where %$a,b,c,d%$ are %$\frac{n}{2}%$-digit numbers. Applied to our example we get the following:

$$
\begin{aligned}
x &= 10^2 \cdot 56 + 78 \\\\
y &= 10^2 \cdot 12 + 34
\end{aligned}
$$

The multiplication of %$x%$ and %$y%$ will led to the following:

$$
\begin{aligned}
x \cdot y &= (10^{n/2} a + b) \cdot (10^{n/2} c + d) \\\\
 &= 10^n ac + 10^{n/2} (ad+bc) + bd
\end{aligned}
$$

Now the idea is to recursively calculate %$ac,ad,bc,bd%$ and then compute %$10^n ac + 10^{n/2} (ad+bc) + bd%$.

The base case is not mentioned here yet. Recursive algorithms need a base case. If the input is small then the result can be given immediately rather then recursing further. The base case breaks the chain of recursion.

The **base case** for integer multiplication are two single-digit numbers. They can be multiplied in one basic operation and return the result.

## Explanation of the Karatsuba Algorithm

The Karatsuba Algorithm is an improved version of the recursive algorithm explained in the previous section.

Starting with the following expression that just expresses the multiplication of x and y.

$$
x \cdot y = 10^n ac + 10^{n/2} (ad+bc) + bd
$$

At a first glance four recursive calls are required, but there are only three computations in the formula that are required. There is no interest in the exact results of %$ad%$ and %$bc%$, just the sum of both products are of interest.


```plaintext
Step 1
  Compute           a*c

Step 2
  Compute           b*d

Step 3
  Compute    (a+b)(c+d) = ac+ad+bc+bd

Step 4: Gauss's Trick
  Compute  (S3               ) - S1 - S2 =
           (ac + ad + bc + bd) - ac - bd = ad + bc

Step 5
  Compute   10^n ac + 10^{n/2} (ad+bc) + bd
```

This shows there are just three recursive multiplications required - plus some additions. 

## Complexity

## Pseudocode

```plaintext
procedure karatsuba(num1, num2)
    if (num1 < 10) or (num2 < 10)
        return num1 × num2
    
    /* Calculates the size of the numbers. */
    m = min(size_base10(num1), size_base10(num2))
    m2 = floor(m / 2) 
    /*m2 = ceil(m / 2) will also work */
    
    /* Split the digit sequences in the middle. */
    high1, low1 = split_at(num1, m2)
    high2, low2 = split_at(num2, m2)
    
    /* 3 calls made to numbers approximately half the size. */
    z0 = karatsuba(low1, low2)
    z1 = karatsuba((low1 + high1), (low2 + high2))
    z2 = karatsuba(high1, high2)
    
    return (z2 × 10 ^ (m2 × 2)) + ((z1 - z2 - z0) × 10 ^ m2) + z0
```

## Implementation

### C++

```cpp
// C++ program to multiply two very large numbers with the
// Karatsuba algorithm

#include <iostream>
#include <string>
#include <math.h> // Required for std::ceil

// Function to equalize two strings
void string_equalize(std::string &string1, std::string &string2)
{
    int len1 = string1.size();
    int len2 = string2.size();

    // Make both strings of equal length

    while (len1 < len2)
    {
        string1 = "0" + string1;
        len1 = string1.size();
    }

    while (len2 < len1)
    {
        string2 = "0" + string2;
        len2 = string2.size();
    }
}

// Function to add two strings
std::string string_add(std::string add1, std::string add2)
{
    // Make both strings of equal length
    string_equalize(add1, add2);

    int len1 = add1.size();

    std::string result(len1 + 1, '0');

    for (int i = len1 - 1; i >= 0; i--)
    {
        int sum = (result[i + 1] - '0') + (add1[i] - '0') + (add2[i] - '0');

        // Calculate carry and store in i+j
        result[i] = (sum / 10) + '0';

        // Calculate and store the result for position i+j+1
        result[i + 1] = (sum % 10) + '0';
    }
    // Remove leading zeros and return string
    return result.erase(0, std::min(result.find_first_not_of('0'), result.size() - 1));
}

// Function to subtract two strings
std::string string_sub(std::string sub1, std::string sub2)
{
    // Make both strings of equal length
    string_equalize(sub1, sub2);

    int len1 = sub1.size();
    std::string result(len1, '0');

    int carry = 0;

    for (int i = len1 - 1; i >= 0; i--)
    {
        int diff = (sub1[i] - '0') - (sub2[i] - '0') - carry;
        if (diff < 0)
        {
            diff += 10;
            carry = 1;
        }
        else
        {
            carry = 0;
        }

        // Calculate and store the result for position i
        result[i] = (diff % 10) + '0';
    }

    return result.erase(0, std::min(result.find_first_not_of('0'), result.size() - 1));
}

// Function to multiply two numbers represented as strings
std::string karatsuba(std::string num1, std::string num2)
{
    // Make both strings of equal length
    if (num1.size() != num2.size())
    {
        string_equalize(num1, num2);
    }

    int len1 = num1.size();
    int len2 = num2.size();

    // Create string for solution
    std::string result;

    // Base case
    if (len1 < 2 && len2 < 2)
    {
        //std::string result(len1 + len2, '0');
        int base_case = ((num1[0] - '0') * (num2[0] - '0'));
        // Calculate ten's place
        result = (base_case / 10) + '0';
        // Calculate one's place
        result.push_back((base_case % 10) + '0');
        return result;
    }

    // Get middle of the two strings
    int n_half = std::ceil((float)len1 / 2);

    // Create substrings, separated in the center
    std::string num1_high = num1.substr(0, len1 - n_half); // Substring starts at position 0 from num1 and ends after n_half characters
    std::string num1_low = num1.substr(len1 - n_half);     // Substring starts at position n_half and includes all charcaters until the end of the string
    std::string num2_high = num2.substr(0, len1 - n_half);
    std::string num2_low = num2.substr(len1 - n_half);

    std::string res_low = karatsuba(num1_low, num2_low);   // Recursive call
    std::string res_hig = karatsuba(num1_high, num2_high); // Recursive call

    std::string mix1 = string_add(num1_low, num1_high);
    std::string mix2 = string_add(num2_low, num2_high);
    std::string res_mix = karatsuba(mix1, mix2); // Recursive call

    std::string first = res_hig + std::string(2 * n_half, '0');
    std::string second = string_sub(string_sub(res_mix, res_hig), res_low) + std::string(n_half, '0');
    std::string third = res_low;

    return string_add(first, string_add(second, third));
}

// Driver code
int main()
{
    std::string num1 = "314159265358979323846264338327950288419716939937510582097494459";
    std::string num2 = "271828182845904523536028747135266249775724709369995957496696762";
    // Correct result: 85397342226735670654635508695465744950348885357651149618796010996400308128465617086587964465544038881186949128462929098241758
    // Compare with Wolfram Alpha:
    // https://www.wolframalpha.com/input/?i=314159265358979323846264338327950288419716939937510582097494459*271828182845904523536028747135266249775724709369995957496696762

    std::cout << "Result: \n"
              << karatsuba(num1, num2) << std::endl;

    return 0;
}
```
