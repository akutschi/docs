---
title: "Algorithms"
date: 2020-06-02T13:07:35-04:00
categories:
    - algorithms
tags:
    - overview
draft: false
---
 
Algorithms are a finite sequence of well-defined computer-implementable rules for solving computational problems.

Starting from an initial state and initial input, the instructions describe a computation that proceeds through a finite number of well-defined successive states, eventually producing _output_ and terminating at a final ending state[^wikialgo].

[^wikialgo]: https://en.wikipedia.org/wiki/Algorithm

The transition from one state to another state is not necessarily **deterministic**. Some algorithms are **non-deterministic**, known as **randomized algorithms**.

An algorithm is basically an instance of logic written in software by software developer/engineers. An optimal algorithm would produce faster results than a non-optimal[^timecomplexity] algorithm for the same purpose, running in more efficient hardware[^wikialgo].

[^timecomplexity]: Higher time complexity

We have to note, that there is a tradeoff between goodness (speed) and elegance (compactness). An elegant program might require more steps to complete a computation than a program that is less elegant.

## Representation of Algorithms

Algorithms can be classed into three accepted levels of Turing machine description[^sipser]:

[^sipser]: Sipser, Michael (2006), Introduction to the Theory of Computation, PWS Publishing Company, ISBN 978-3-319-08107-6

- **High-level Description**
    - _... prose to describe an algorithm, ignoring the implementation details. At this level, we do not need to mention how the machine manages its tape or head._
- **Implementation Description**
    - _... prose used to define the wat the Turing machine uses its head and the way it stores data on its tape. At this level, we do not gibe details iof states or transition function._
- **Formal Description**
    - Most detailed, _lowest level_, gives the Touring machine's _state table_.


## Analysis of Algorithms

It is important to know how much of a resource is theoretically required for a given algorithm, and empirical tests cannot replace formal analysis.

## Classification of Algorithms

### Implementation

- **Recursion**
    - A **recursive algorithm** calls itself repeatedly until a certain condition matches (termination condition) - common in functional programming
    - An **iterative algorithm** use repetitive constructs like loops and sometimes additional data structures like stacks
- **Logical** 
    - Algorithm = logic + control
        - Basis for the **logic programming paradigm**
    - Logic component: Axioms that may be used in the computation 
    - Control component:  Determines the way in which deduction is applied to the axioms
- **Serial, Parallel, Distributed**
    - Serial: Execute one instruction of an algorithm at a time
    - Parallel: Use computer architectures where several processors are utilized
    - Distributed: Use multiple machines connected with a computer network
    - The latter two algorithms divide problems into more symmetrical or asymmetrical sub-problems and collect the results back together
- **Deterministic or Non-Deterministic**
    - Deterministic: Solve the problem with exact decision at every step of the algorithm
    - Non-deterministic: Solve problem by _guessing_
        - Typical guesses are made more accurate through the use of heuristics
- **Exact or Approximate**
    - Many algorithms reach an exact solution
    - Approximation algorithms seek an approximation that is closer to the true solution
        - Approximation can be reached by either deterministic or a random strategy
- **Quantum Algorithm**
    - This term is quite often used for those algorithms which seem, inherently quantum, or use some essential feature of quantum computing (quantum superposition, quantum entanglement)

### Design Paradigm

- **Brute-Force or Exhaustive Search**
    - Naive method of trying every possible solution to see which one is the best
- **Divide and Conquer**
    - Reduces an instance of a problem to one or more smaller instances of the same problem - usually recursively - until the instances are small enough to solve easily 
    - Simpler variant: **Decrease and Conquer**
        - Solves an identical subproblem and uses the solution of this subproblem to solve the bigger problem
- **Search and Enumeration**
    - Problems ca be modeled as problems on graphs
    - Graph exploration algorithms specify rules for moving around a graph and is useful for such problems
- **Randomized Algorithm**
    - Can be useful in finding solutions for problems where finding an exact solution can be impractical
    - Classes of this kind of algorithms:
        - Monte Carlo Algorithms
            - Correct answer with a high probability
        - Las Vegas Algorithms
            - Returns always the correct answer
- **Reduction of Complexity**
    - Transform a difficult problem into a better-known problem
- **Back Tracking**
    - Multiple solutions are built incrementally and abandoned when it is determined that they cannot lead to a valid full solution

### Optimization Problems

- **Linear Programming**
    - When searching for optimal solutions to a linear function bound to linear equality and inequality constraints, the constraints of the problem can be used directly in producing the optimal solutions
- **Dynamic Programming**
    - When a problem show optimal substructures - the optimal solution to a problem can be constructed from optimal solutions to sub-problems - and overlapping sub-problems - the same subproblems are used to solve many different problem instances - this approach avoids recomputing solutions that have already been computed
- **Greedy Method**
    - Examines substructures
        - Not of the problem but of a given solution
    - Starts with some solution and improve it by making small modifications
- **Heuristic Method**
    - Heuristic algorithms can be used to find a solution in cases where finding the optimal solution is impractical

### Field of Study

Every scientific field has its own problems and algorithms. Fields tend to overlap and algorithm advances in one field may improve those of other sometimes unrelated fields.

### Complexity

Algorithms can be classified by the amount of time they need to complete compared to their input size, see also [analysis of algorithms]({{< ref "../Analysis-of-Algorithms/index.md" >}}): 

- **Constant Time**
    - Time needed by the algorithm is the same, regardless of the input size
    - %$O(1)%$
- **Logarithmic Time**
    - Time is a logarithmic function of the input size
    - %$O(\log n)%$
- **Loglinear Time**
    - Aka linearithmic, quasilinear or "n log n"
    - %$O(n \log n) = O(\log n!)%$
- **Linear Time**
    - Time is proportional to the input size
    - %$O(n)%$
- **Polynomial Time**
    - Time is a power of the input size
    - %$O(n^c%$
- **Exponential Time**
    - Time is an exponential function of the input size
    - %$O(c^n) \text{ with } c > 1%$

## Links & Resources

- [Wikipedia: Algorithm](https://en.wikipedia.org/wiki/Algorithm)


