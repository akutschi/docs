---
title: "Master Theorem"
date: 2020-05-16T17:01:04-04:00
categories:
    - algorithms
tags:
    - analysis of algorithms
    - divide & conquer
draft: false
---
 
The master theorem is kind of a _black box_ method for determining the running time of recursive algorithms. This theorem can be applied to most divide-and-conquer algorithms, including [Karatsuba's multiplication]({{< ref "../Karatsuba-Multiplication/index.md" >}}) and [Strassen's matrix multiplication]({{< ref "../Strassens-Matrix-Multiplication/index.md" >}}).

## Standard Recurrences

Divide-and-conquer algorithms often follow a generic pattern, they solve a problem of size %$n%$ by recursively solving %$a%$ subproblems of size %$n/b%$ and then combining these answers in %$O(n^d)%$ time - with %$a,b,c,d>0%$.

> **Base case:** %$T(n)%$ is at most a constant for all sufficient small %$n%$.
>
>
> **General case:** for larger values of %$n%$,
>
> $$
> T(n) \leq a \cdot T \left( \frac{n}{b} \right) + O(n^d)
> $$
>
> **Parameters:**
> * a = number of recursive calls
> * b = input size shrinkage factor
> * d = exponent in running time of the "combine step"

The **base case** assumes that once the input is so small that no recursive calls are required, the problem can be solved in O(1) time. The **general case** assumes %$a%$ recursive calls, each on a subproblem of size %$\frac{1}{b}%$ times smaller than its input %$n%$ and %$O(n^d)%$ work outside these recursive calls.

The values a, b and d are constants and independent of the input size n. There are constants suppressed in the base case and in the last term, but the master method does not depend on this constants.

## Master Method

The upper bounds can now be calculated easily with the master method.

> If %$T(n)%$ is defined by a standard recurrence, with parameters %$a \geq 1, b > 1, d \geq 0%$ then 
> $$
> T(n) =
> \begin{cases}
> O(n^d \log n),  & \text{if $a=b^d$} \\\\
> O(n^d), & \text{if $a < b^d$} \\\\
> O(n^{\log_b a}),  & \text{if $a>b^d$}
> \end{cases}
> $$

There are also more general versions of the master method that handle a wider family of recurrences. But this simple version is sufficient for almost any divide-and-conquer algorithm.

## Proof of the Master Theorem

The three cases correspond to three different types of the recursion trees. For the proof we explicitly write out all constant factors in the recurrence

> **Base case:** %$T(1) \leq c%$
>
> **General case:** for %$n>1%$,
>
> $$T(n) \leq a \cdot T \left( \frac{n}{b} \right) + cn^d$$