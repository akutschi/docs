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

## Implementation
