---
title: "Multiplication Algorithms"
date: 2020-05-06T12:59:00-04:00
categories:
    - algorithms
tags:
    - multiplication
draft: false
---

## Grade-School Multiplication

This is the typical way taught in schools for multiplying numbers by hand in base 10 and known as **long multiplication**. The general procedure is to multiply the multiplicand with each digit of the multiplier and then sum up all properly shifted results. 

### Example

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

### Required number of basic operations

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

## Karatsuba Multiplication

This multiplication algorithm is an example of a _divide and conquer algorithm_. 

### Example

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

### A Recursive Algorithm 

In general every number can be expressed decomposed. Two numbers %$x,y%$ can be written in the following way 

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

### Karatsuba Algorithm

The Karatsuba Algorithm is an improved version of the recursive algorithm explained in the previous chapter.

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