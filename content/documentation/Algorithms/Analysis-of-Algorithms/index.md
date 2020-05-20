---
title: "Analysis of Algorithms"
date: 2020-05-16T17:12:57-04:00
categories:
    - algorithms
tags:
    - analysis of algorithms
draft: true
---
 
The goal is to find the computational complexity of an algorithm – the amount of time, storage, or other resources needed to execute them.  This helps us to identify better and worse algorithms.

> **Asymptotic Notation in a few words:**
>
> Suppress constant factors and lower-order terms.

The constant factors are too system-dependent and the lower-order terms are irrelevant for large inputs. 

## Big-O  Notation

The Big-O notation is a mathematical notation that describes the behavior of a function/algorithm when the argument tends toward infinity. 

The formal definition of big-O notation:

> %$T(n)=O(f(n))%$ if and only if there exist positive constants %$c%$ and %$n_0%$ such that
>
> $$ T(n) \leq c \cdot f(n) $$
>
> for all %$n \geq n_0%$.

This inequality tells us that %$T(n)%$ should be bounded by a multiple of %$f(n)%$. The last part, the for all %$n \geq n_0%$, says that the inequality holds for sufficient large %$n%$. The constant %$n_0%$ defines sufficient large. 

The image shows curve progressions for some typical big-O values.

![Typical curve progressions](complexity.png)
## Big-Omega Notation

A function %$T(n)%$ is big-omega of another function %$f(n)%$ if a and only if %$T(n)%$ is envetually bounded below by a constant multiple of %$f(n)%$. We write then %$T(n)=\Omega(f(n))%$

> %$T(n)=\Omega(f(n))%$ if and only if there exist positive constants %$c%$ and %$n_0%$ such that
>
> $$ T(n) \geq c \cdot f(n) $$
>
> for all %$n \geq n_0%$.

## Big-Theta Notation

%$T(n) = \Theta(fn)%$ just means %$T(n) = \Omega(fn)%$ _and_ %$T(n) = O(fn)%$. 

> %$T(n)=\Theta(f(n))%$ if and only if there exist positive constants %$c_1,c_2%$ and %$n_0%$ such that
>
> $$ c_1 \cdot f(n) \leq T(n) \leq c_2 \cdot f(n) $$
>
> for all %$n \geq n_0%$.

## Little-O Notation

The little-o notation is more strict than the big-o notation since %$T(n)=o(f(n))%$ must hold for every positive constant %$c%$.

> %$T(n)=o(f(n))%$ if and only if for every positive constant %$c>0%$, there exists a choice of %$n_0%$ such that
>
> $$ T(n) \leq c \cdot f(n) $$
>
> for all %$n \geq n_0%$.

Little-o notation makes a stronger statement than the corresponding big-O notation: 

> * Every function that is little-o of f is also big-O of f. 
> * Not every function that is big-O of f is also little-o of f.