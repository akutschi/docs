---
title: "Closest Pair"
date: 2020-05-11T20:17:05-04:00
categories:
    - algorithms
tags:
    - sorting
    - divide & conquer
    - computational geometry
draft: false
---
 
This divide and conquer algorithm is an algorithm to solve the **closest pair** problem. In this problem %$n%$ points in the plane are given and we have to find the pair of points that are closest to each other.

> **Closest Pair Problem**
>
> **Input:** %$n \geq 2 %$ points %$p_1 = (x_1,y_1),...,p_n = (x_n,y_n)%$ in the plane.
>
> **Result:** Pair of points with smallest Euclidian distance %$d(p_i,p_j)%$

This is an application in the field of **computational geometry**, an area that studies algorithms for reasoning about and manipulating geometric objects, and that has applications in robotics, computer vision, and computer graphics[^tim].

[^tim]: Tim Roughgarden (2017). Algorithms Illuminated: Part1 The Basics. Soundlikeyourself Publishing, ISBN: 978-0999282908

## Example

The image shows an example of the closest pair problem. The task is to find the two points with the lowest distance to each other.

![Closest Pair of Points Example](240px-Closest_pair_of_points.svg.png)

In this tiny example the closest pair of points are shown in red.

## Explanation

The Euclidian distance[^sqrt] can be used to measure the straight-line distance between two points %$p_1 = (x_1,y_1)%$ and %$p_2 = (x_2,y_2)%$: 

$$d(p_1,p_2) = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$$ 

[^sqrt]: In real world applications the computation of the square root is not required; the result will be the same.

The **brute-force method** can be used to find these two points. Just compute the Euclidian distance between each pair of points one-by-one. Since this is very time consuming, we can do better with a **divide and conquer algorithm**. This approach is consists of a preprocessing step and the main divide and conquer step.

The **preprocessing step** makes copies of the points and sorts them with a time complexity of %$O(n \log n)%$:

- A copy of the points %$P_x%$ sorted by %$x%$-coordinates.
- A copy of the points %$P_y%$ sorted by %$y%$-coordinates.

Now comes the **divide and conquer step** to compute the closest pair. In a first step %$P_x%$ will be divided into two halves %$L%$ and %$R%$, this can be implemented in %$O(n)%$ time:

- Then %$L_x%$, the first half of %$P_x%$ sorted by %$x%$-coordinate will be derived
    - Just split %$P_x%$ in half since this is already sorted
- Then %$L_y%$, the first half of %$P_x%$ sorted by %$y%$-coordinate will be derived
    - Algorithm can perform a linear scan over %$P_y%$ putting each point at the end of %$L_y%$
- Then %$R_x%$, the second half of %$P_x%$ sorted by %$x%$-coordinate will be derived
- Then %$R_y%$, the second half of %$P_x%$ sorted by %$y%$-coordinate will be derived

The next step are the recursive calls to get closest pair for each side:

- Get the closest pair %$(l_1,l_2)%$ for %$L_x%$ and %$L_y%$
- Get the closest pair %$(r_1,r_2)%$ for %$R_x%$ and %$R_y%$

But we need more since the case of split pairs has to be covered. Split pairs are closest pairs when both points are not in the same side.

- Calculate %$\delta%$ the minimum of the distance between the closest pair that is left or right
- Hand over this value %$\delta%$ explicitly to the function that calculates the closest split pair
- Then use the brute force method to compare all points that are in the range %$x_{max} \pm \delta%$ and return %$(s_1,s_2)%$ 
    - %$x_{max}%$ is the largest x-coordinate in the left half - %$O(1)%$
    - Sort the points by y-coordinates - %$O(n)%$

Finally return the best of %$(l_1,l_2)%$, %$(r_1,r_2)%$ and %$(s_1,s_2)%$ 


The base case is given with two or three points; then the brute force method is applied.

## Complexity / Analysis / Running Time

The time-complexity of the **brute-force strategy** is 

$$ \Theta (n^2) $$

since two nested loops are required to traverse the array from start to end. 

The faster **divide and conquer algorithm** uses the [principle of merge sort]({{< ref "../../Sorting/Merge-Sort/index.md" >}}). The complexity of the _divide and conquer approach_ can be calculated with the [master theorem]({{< ref "../../Foundations/Master-Theorem/index.md" >}}). To use the formula 

$$
T(n) \leq a \cdot T \left( \frac{n}{b} \right) + O(n^d)
$$

three parameters are required. **a** the branching factor, **b** the shrinkage factor and **d** the exponent of the running time of the combine step.

In this algorithm are two recursive calls, so the branching factor is %$a=2%$. Each recursive call receives half of the input, therefore the shrinkage factor is %$b=2%$. The work that is done outside these recursive calls is dominated by the merge subroutine and runs in linear time, so %$d=1%$

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

The closest pair algorithm has - like merge-sort - a worst-case performance of

$$
O(n \cdot \log n)
$$

## Pseudocode

### Brute Force

```plaintext
minDist = infinity
for i = 1 to length(P) - 1 do
    for j = i + 1 to length(P) do
        let p = P[i], q = P[j]
        if dist(p, q) < minDist  then
            minDist = dist(p, q)
            closestPair = (p, q)
return closestPair
```

### Divide and Conquer

Let %$P%$ be  the  set  of  coordinates  of %$n%$ points  on  the  plane.   For  any  pair  of  points %$p=  (x,y)%$ and %$q= (x',y')%$, let %$d(p,q) =\sqrt{(x−x')^2+ (y−y')^2}%$; i.e., %$d(p,q)%$ is the Euclidean distance between %$p%$ and %$q%$.

```plaintext
ClosestPair(P)
// P is a set of at least two points on the plane, given by their x- and y-coordinates

P_x := the list of points in P_sorted by x-coordinate
P_y := the list of points in P_sorted by y-coordinate
return RCP(P_x,P_y)


RCP(P_x,P_y)
// P_x and P_y are lists of the same set of (at least two) points sorted by x- and y-coordinate, respectively

if |P_x| <= 3 then
    calculate all pairwise distances and return the closest pair
else
    L_x := first half of P_x
    R_x := second half of P_x
    m   := (max x-coordinate in L_x + min x-coordinate in R_x)/2
    L_y := sublist of P_y of points in L_x
    R_y := sublist of P_y of points in R_x

    (p_L,q_L) := RCP(L_x,L_y)
    (p_R,q_R) := RCP(R_x,R_y)

    delta := min(d(p_L,q_L),d(p_R,q_R))
    B     := sublist of P_y of points whose x-coordinates are within delta of m

    if |B| <= 1 then
        if d(p_L,q_L) <= d(p_R,q_R) then return (p_L,q_L)
        else return(p_R,q_R)
    else
        (p∗,q∗) := any two distinct points on B 
        for each p ∈ B (in order of appearance on B) do
            for each of the (up to) next seven points q after p on B do
                if d(p,q) < d(p∗,q∗) then (p∗,q∗) := (p,q)
            if d(p∗,q∗) < delta then return (p∗,q∗)
            else if d(p_L,q_L) <= d(p_R,q_R) then return (p_L,q_L)
        else return (p_R,q_R)

```
<!--
## Implementation

### Brute Force

### Divide and Conquer
-->

## Links & Resources

- Thomas H. Cormen; Charles E. Leiserson; Ronald L. Rivest; Clifford Stein (2009). Introduction To Algorithms (3rd ed.). MIT Press. ISBN 978-0-262-03384-8.
- Tim Roughgarden (2017). Algorithms Illuminated: Part1 The Basics. Soundlikeyourself Publishing, ISBN: 978-0999282908
- https://www.geeksforgeeks.org/closest-pair-of-points-using-divide-and-conquer-algorithm/
- [Wikipedia: Closest Pair of Points Problem](https://en.wikipedia.org/wiki/Closest_pair_of_points_problem)
- http://www.cs.toronto.edu/~vassos/teaching/c73/handouts/closest-pair-of-points.pdf
- https://www.cs.mcgill.ca/~cs251/ClosestPair/ClosestPairDQ.html