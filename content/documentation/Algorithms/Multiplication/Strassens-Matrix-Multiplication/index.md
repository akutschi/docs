---
title: "Strassen's Matrix Multiplication"
date: 2020-05-11T20:17:35-04:00
categories:
    - algorithms
tags:
    - multiplication
    - divide & conquer
draft: false
---
 
The **Strassen Algorithm** is an algorithm for matrix multiplication. It's faster than the standard algorithm to multiply large matrices. 

## Example

The **straightforward algorithm** is the one where we just multiply _row times column_.

$$
\mathbf{X} \cdot \mathbf{Y}= 
\begin{pmatrix}
1 & 2 \\\\
3 & 4
\end{pmatrix} \cdot
\begin{pmatrix}
5 & 6 \\\\
7 & 8
\end{pmatrix} = 
\begin{pmatrix}
1 \cdot 5 + 2 \cdot 7 & 1 \cdot 6 + 2 \cdot 8  \\\\
3 \cdot 5 + 4 \cdot 7 & 3 \cdot 6 + 4 \cdot 8
\end{pmatrix} = 
\begin{pmatrix}
19 & 22 \\\\
43 & 50
\end{pmatrix}
$$

Now we use the **Strassen algorithm** and in a first step we create the sub-matrices:

$$
\mathbf{X} = 
\begin{pmatrix}
1 & 2 \\\\
3 & 4
\end{pmatrix} =
\begin{pmatrix}
\mathbf{A} & \mathbf{B}\\\\
\mathbf{C} & \mathbf{D}
\end{pmatrix}
$$

$$
\mathbf{Y} =
\begin{pmatrix}
5 & 6 \\\\
7 & 8
\end{pmatrix} =
\begin{pmatrix}
\mathbf{E} & \mathbf{F}\\\\
\mathbf{G} & \mathbf{H}
\end{pmatrix}
$$

In this very special case the result is that each sub-matrix has the dimension %$1x1%$, hence a scalar. In the next step we proceed with the calculation of the following seven equations:

$$
\begin{aligned}
\mathbf{M_1} &= \mathbf{A} \cdot (\mathbf{F} - \mathbf{H}) = 1 \cdot (6 - 8) = -2 \\\\
\mathbf{M_2} &= (\mathbf{A} + \mathbf{B}) \cdot \mathbf{H} = (1 + 2) \cdot 8 = 24 \\\\
\mathbf{M_3} &= (\mathbf{C} + \mathbf{D}) \cdot \mathbf{E} = (3 + 4) \cdot 5 = 35 \\\\
\mathbf{M_4} &= \mathbf{D} \cdot (\mathbf{G} - \mathbf{E}) = 4 \cdot (7 - 5) = 8 \\\\
\mathbf{M_5} &= (\mathbf{A} + \mathbf{D}) \cdot (\mathbf{E} + \mathbf{H}) = (1+4)\cdot(5+8) = 65 \\\\
\mathbf{M_6} &= (\mathbf{B} - \mathbf{D}) \cdot (\mathbf{G} + \mathbf{H}) = (2-4)\cdot(7+8) = -30 \\\\
\mathbf{M_7} &= (\mathbf{A} - \mathbf{C}) \cdot (\mathbf{E} + \mathbf{F}) = (1-3)\cdot(5+6) = -22 \\\\
\end{aligned}
$$

And in the last step we insert the results:

$$
\begin{aligned}
\mathbf{X} \cdot \mathbf{Y} &=
\begin{pmatrix}
\mathbf{M_5} + \mathbf{M_4} - \mathbf{M_2} + \mathbf{M_6} & \mathbf{M_1} + \mathbf{M_2}\\\\
\mathbf{M_3} + \mathbf{M_4} & \mathbf{M_1} + \mathbf{M_5} - \mathbf{M_3} - \mathbf{M_7}
\end{pmatrix}\\\\
&= 
\begin{pmatrix}
65 + 8 - 24 + (-30) & -2 + 24\\\\
35 + 8 & -2 + 65 - 35 - (-22)
\end{pmatrix} \\\\
&=
\begin{pmatrix}
19 & 22\\\\
43 & 50
\end{pmatrix}
\end{aligned}
$$

As we can see this algorithm leads to the same result. Though it seems that this algorithm is much longer we can save one multiplication; the _straightforward algorithm requires 8 multiplications_ and the _Strassen algorithm requires just seven multiplications_ and a bunch of additions and subtractions. But the latter ones are computational cheap.

On large matrices this one less multiplication makes the difference; this advantage can also seen on the [complexity](#complexity--analysis--running-time).

## Explanation

This algorithm uses the divide and conquer paradigm. To achieve this the %$n\times n%$ input matrices must be divided into smaller sub-matrices.

$$
\mathbf{X} =
\begin{pmatrix}
x_{1,1} & x_{1,2} & x_{1,3} & x_{1,4}\\\\
x_{2,1} & x_{2,2} & x_{2,3} & x_{2,4}\\\\
x_{3,1} & x_{3,2} & x_{3,3} & x_{3,4}\\\\
x_{4,1} & x_{4,2} & x_{4,3} & x_{4,4}
\end{pmatrix}
$$

$$
\mathbf{Y} =
\begin{pmatrix}
y_{1,1} & y_{1,2} & y_{1,3} & y_{1,4}\\\\
y_{2,1} & y_{2,2} & y_{2,3} & y_{2,4}\\\\
y_{3,1} & y_{3,2} & y_{3,3} & y_{3,4}\\\\
y_{4,1} & y_{4,2} & y_{4,3} & y_{4,4}
\end{pmatrix}
$$

These sub-matrices are of size %$\frac{n}{2} \times \frac{n}{2}%$:

$$
\mathbf{X} =
\begin{pmatrix}
\mathbf{A} & \mathbf{B}\\\\
\mathbf{C} & \mathbf{D}
\end{pmatrix}
$$

$$
\mathbf{A} =
\begin{pmatrix}
x_{1,1} & x_{1,2} \\\\
x_{2,1} & x_{2,2}
\end{pmatrix}
$$

$$
\mathbf{Y} =
\begin{pmatrix}
\mathbf{E} & \mathbf{F}\\\\
\mathbf{G} & \mathbf{H}
\end{pmatrix}
$$

One nice property is that equal-size blocks behave like individual entries:

$$
\mathbf{X} \cdot \mathbf{Y} =
\begin{pmatrix}
\mathbf{A} \cdot \mathbf{E} + \mathbf{B} \cdot \mathbf{G} & \mathbf{A} \cdot \mathbf{F} + \mathbf{B} \cdot \mathbf{H}\\\\
\mathbf{C} \cdot \mathbf{E} + \mathbf{D} \cdot \mathbf{G} & \mathbf{C} \cdot \mathbf{F} + \mathbf{D} \cdot \mathbf{H}
\end{pmatrix}
$$

The decomposition and computation of the the previous equation translates to a recursive algorithm for matrix multiplication until the base case is given. But this algorithm would have the same running time as the straightforward algorithm of %$\Theta(n^3)%$.

To get rid of one multiplication we have to compute the following equations and as we can see at a quick glance there are just seven multiplications: 

$$
\begin{aligned}
\mathbf{M_1} &= \mathbf{A} \cdot (\mathbf{F} - \mathbf{H}) \\\\
\mathbf{M_2} &= (\mathbf{A} + \mathbf{B}) \cdot \mathbf{H} \\\\
\mathbf{M_3} &= (\mathbf{C} + \mathbf{D}) \cdot \mathbf{E} \\\\
\mathbf{M_4} &= \mathbf{D} \cdot (\mathbf{G} - \mathbf{E}) \\\\
\mathbf{M_5} &= (\mathbf{A} + \mathbf{D}) \cdot (\mathbf{E} + \mathbf{H}) \\\\
\mathbf{M_6} &= (\mathbf{B} - \mathbf{D}) \cdot (\mathbf{G} + \mathbf{H}) \\\\
\mathbf{M_7} &= (\mathbf{A} - \mathbf{C}) \cdot (\mathbf{E} + \mathbf{F}) \\\\
\end{aligned}
$$

Then we use these results and use them in the following matrix: 

$$
\begin{aligned}
\mathbf{X} \cdot \mathbf{Y}
&=
\begin{pmatrix}
\mathbf{A} \cdot \mathbf{E} + \mathbf{B} \cdot \mathbf{G} & \mathbf{A} \cdot \mathbf{F} + \mathbf{B} \cdot \mathbf{H}\\\\
\mathbf{C} \cdot \mathbf{E} + \mathbf{D} \cdot \mathbf{G} & \mathbf{C} \cdot \mathbf{F} + \mathbf{D} \cdot \mathbf{H}
\end{pmatrix} \\\\
&=
\begin{pmatrix}
\mathbf{M_5} + \mathbf{M_4} - \mathbf{M_2} + \mathbf{M_6} & \mathbf{M_1} + \mathbf{M_2}\\\\
\mathbf{M_3} + \mathbf{M_4} & \mathbf{M_1} + \mathbf{M_5} - \mathbf{M_3} - \mathbf{M_7}
\end{pmatrix}
\end{aligned}
$$

If we use the seven equations itself - instead of the numerical results - this would result in many cancellations and therefore we could see the original equations to solve the matrix multiplication.

## Complexity / Analysis / Running Time

Since this is a _divide and conquer algorithm_ the [master theorem]({{< ref "../../Foundations/Master-Theorem/index.md" >}}) can be applied. To use the formula 

$$
T(n) \leq a \cdot T \left( \frac{n}{b} \right) + O(n^d)
$$

three parameters are required. **a** the branching factor, **b** the shrinkage factor and **d** the exponent of the running time of the combine step.

Looking at the previous section we see that the shrinkage factor is %$b=2%$ and since we have seven more multiplications per recurrence the branching factor is %$a=7%$. The work done outside of the recursive call for addition and subtraction of matrices leads to and exponent of %$d=2%$.

$$
T(n) \leq 7 \cdot T \left( \frac{n}{2} \right) + O(n^2)
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

To get the correct case just insert the parameters. We will see then that the third case is the correct result:

$$ a>b^d \Rightarrow O(n^{\log_b a}) \Longrightarrow O(n^{\log_2 7}) $$

The Strassen algorithm for matrix multiplication is of complexity 

$$O(n^{\log_2 7})=O(n^{2.8074})$$

The _naive_ way would require 8 instead of 7 multiplications and this would lead to a risen complexity of 

$$O(n^{\log_2 8})=O(n^3)$$

## Pseudocode

```plaintext
Strassen(A, B)

If n=1  
    Return AxB                // Base case              
Else
    Compute sub-matrices A, B, C, D of X
    Compute sub-matrices E, F, G, H of Y

    M1 <- Strassen(A, F-H)    // Recursive calls
    M2 <- Strassen(A+B, H)
    M3 <- Strassen(C+D, E)
    M4 <- Strassen(D, G-E)
    M5 <- Strassen(A+D, E+H)
    M6 <- Strassen(B-D, G+H)
    M7 <- Strassen(A-C, E+F)

    Z11 <- M5+M4−M2+M6
    Z12 <- M1+M2
    Z21 <- M3+M4
    Z22 <- M1+M5−M3−M7

    Output Z
End If
```

<!--
## Implementation
-->

## Links & Resources

- [Wikipedia: Strassen Algorithm](https://en.wikipedia.org/wiki/Strassen_algorithm)
- Tim Roughgarden (2017). Algorithms Illuminated: Part1 The Basics. Soundlikeyourself Publishing, ISBN: 978-0999282908
- https://stanford.edu/~rezab/classes/cme323/S16/notes/Lecture03/cme323_lec3.pdf or [here](./cme323_lec3.pdf)