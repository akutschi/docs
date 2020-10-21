---
title: "Linear Regression with Multiple Variables"
date: 2020-10-15T09:25:07-04:00
categories:
    - machine learning
tags:
    - supervised learning
    - cost function
    - linear regression
draft: true
---
 
## Multivariate Linear Regression

### Model Representation

Now we will introduce a more powerful version of linear regression, we will work with multiple variables. For this the following notation will be made known:

- %$x_j^{(i)}%$ is the value of feature %$j%$ in the %$i^{th}%$ example
- %$x^{i}%$ are the features of the %$i^{th}%$ example and can be represented as vector
- %$m%$ is - as before - the number of training examples
- %$n%$ is the number of features

### Hypothesis

The hypothesis has also to be updated to a slightly more complex version:

$$
h_{\theta}(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 + ... + \theta_n x_n 
$$

For convenience we define that there is %$x_0 = 1 \Rightarrow x_0^{(i)} = 1%$. Otherwise we would have more parameters than features and matrix operations would not be possible.

To make this hypothesis more intuitive, we use the house price example. The %$\theta_0%$ can be interpreted as basic price of a house. The other %$\theta%$ values can represent the price per square meter, the number of floors, the number of bathrooms, the number of bedrooms, the number of floors, and so on.

The hypothesis can also be written as:

$$
h_{\theta}(x) = \theta^T x
$$

This is basically the same hypothesis as the already written one, since %$\theta%$ and %$x%$ can be interpreted as %$n+1%$ dimensional vectors:

$$
x = \begin{bmatrix}
    x_0 \\\\
    x_1 \\\\
    x_2 \\\\
    \vdots \\\\
    x_n
\end{bmatrix}
\in \mathbb{R}^{n+1}
\qquad
\theta = \begin{bmatrix}
    \theta_0 \\\\
    \theta_1 \\\\
    \theta_2 \\\\
    \vdots \\\\
    \theta_n
\end{bmatrix}
\in \mathbb{R}^{n+1}
$$

### Cost Function 

The **cost function** or **squared error function** or **mean squared error** is now:

$$
\begin{aligned}
J(\theta) = J(\theta_0,\theta_1,...,\theta_n) &= \frac{1}{2m}\sum^m_{i=1} \left( \hat{y}^{(i)} - y^{(i)} \right)^2 \\\\
&= \frac{1}{2m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2
\end{aligned}
$$

### GD for Multiple Variables

The GD is basically the same and the following algorithm has to be repeated until convergence is achieved:

$$
\begin{aligned}
\theta_j :&= \theta_j - \alpha \frac{\partial}{\partial \theta_j}J(\theta) \\\\
&= \theta_j - \alpha \frac{\partial}{\partial \theta_j}J(\theta_0,\theta_1,...,\theta_n) \\\\
&= \theta_j - \alpha \frac{\partial}{\partial \theta_j} \frac{1}{2m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2 \\\\
&= \theta_j - \alpha \frac{1}{m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right) \cdot x_j^{(i)}
\end{aligned}
$$

The equations to be solved for the n features at the same time explicitly look like the following ones:

$$
\begin{aligned}
\theta_0 &:=  \theta_0 - \alpha \frac{1}{m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right) \cdot x_0^{(i)} \\\\
\theta_1 &:=  \theta_1 - \alpha \frac{1}{m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right) \cdot x_1^{(i)} \\\\
\theta_2 &:=  \theta_1 - \alpha \frac{1}{m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right) \cdot x_2^{(i)} \\\\
... \\\\
\theta_n &:=  \theta_1 - \alpha \frac{1}{m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right) \cdot x_n^{(i)}
\end{aligned}
$$

When looking at the equations we see that the [linear regression with one variable]({{< ref "../Linear-Regression-with-One-Variable/index.md" >}}) is just a special case of the linear regression. With the knowledge that by definition %$x_0^{(i)}=1%$ and by setting %$n=1%$ we get the same equations like in the one variable case.

### Feature Scaling

The overall idea of feature scaling is to make sure that the features are on a similar ranges of values, so that gradient descent can converge more quickly. There are no exact requirements, the only thing is that the ranges of the input variables are roughly the same. Ideally:

$$
-1 \leq x_{i} \leq +1
$$

But %$-0.5 \leq x_{i} \leq +0.5%$ or %$0 \leq x_{i} \leq +3%$ are also sufficient. The reason for this approach is, that having %$\theta%$ in roughly the same range will gradient descend converge quickly on small ranges and slowly on large ranges. In the latter case %$\theta%$ would oscillate inefficiently down to the optimum. Think  about the shape of a contour plot with very uneven ranges for the variables - the shape would be tall and skinny ovals.

> Don't worry if the the features are not exactly on the same scale or exactly in the same range of values.
>
> Andre Ng in Machine Learning on Coursera

To scale the features several methods can be used. The first one is **rescaling** aka **min-max normalization** and can be achieved by applying the following formula:

$$
x_i := \frac{x_i - min(x_i)}{max(x_i) - min(x_i)}
$$

Or if the range is between two specific values %$[a,b]%$ then we have to use a modified formula:

$$
x_i := a + \frac{\[x_i - min(x_i)\](b-a)}{max(x_i) - min(x_i)}
$$

The second method is **mean normalization** and is very similar to the previous one. The only difference is that we subtract the mean value %$\mu_i%$ of the features from original value:

$$
x_i := \frac{x_i - \mu_i}{max(x_i) - min(x_i)}
$$

A third method is called **standardization**, sometimes this method is also referred as mean normalization. The difference to mean normalization is that the denominator is the standard deviation %$\sigma_i%$ and not the difference of the maximum and the minium value of the feature:

$$
x_i := \frac{x_i - \mu_i}{\sigma_i}
$$


### Learning Rate

First thing is to make sure that GD is working correctly. The simplest way to debug gradient descent is to make a plot of the cost function %$J(\theta)%$ over the number of iterations. If ever %$J(\theta)%$ increases or alternates then something is wrong and as a first step the learning rate %$\alpha%$ should be decreased. It has been proven that if the learning rate %$\alpha%$ is sufficiently small, then %$J(\theta)%$ will decrease on every iteration.

An automatic convergence test helps to decide when convergence is achieved. For example one can say that convergence can be declared when %$J(\theta)%$ decreases by less than %$\epsilon = 10^{-3}%$ in one iteration. The problem here is choose a threshold. 

Burt how to find the right learning rate %$\alpha%$? The rate shouldn't be too large, otherwise the cost function might diverge. A small learning rate is slow until convergence is achieved. The solution is to try several learning rates, the [machine learning course](https://www.coursera.org/learn/machine-learning) recommends to start with a small value and repeatedly threefold this value:

$$
...,\quad 0.001,\quad 0.003,\quad 0.01,\quad 0.03,\quad 0.1,\quad 0.3,\quad 1,\quad ...
$$

The _goal_ is to find values in the range from "very slow" to "too big and not converging".

## Features and Polynomial Regression

Sometimes it is useful to define new features. For example when predicting the house price the hypothesis could look like this:

$$
h_{\theta}(x) = \theta_0 + \theta_1 \times \underbrace{frontage}_{x_1} + \theta_2 \times \underbrace{depth}_{x_2}
$$

Now we can say, not the frontage and the depth are relevant, instead we say the total area %$x=x_1 \times x_2%$ is relevant. And this would result in a new hypothesis:

$$
h_{\theta}(x) = \theta_0 + \theta_1 \times x
$$

By defining new features the result is not just a new hypothesis but also a better model.

Closely related is **polynomial regression** where the hypothesis is not linear. The behavior of the hypothesis function can be changed by making it a quadratic, cubic, square root or any other non-linear function so that the hypothesis fits the data better.

Coming from the hypothesis %$h_{\theta}(x) = \theta_0 + \theta_1 \times x_1%$ we can create a cubic

$$
h_{\theta}(x) = \theta_0 + \theta_1 \times x_1 + \theta_2 \times x_1^2 + \theta_3 \times x_1^3
$$

or a square root function

$$
h_{\theta}(x) = \theta_0 + \theta_1 \times x_1 + \theta_3 \times \sqrt{x_1}
$$

or any other function.

One import thing to kep in mind is, if the features are chosen this way then feature scaling becomes important. See the following examples that shows how the ranges changes when using a non-linear hypothesis:

$$ 
\begin{aligned}
x_1 \in \[1,1000\] &\Rightarrow x_1^2 \in \[1,10^6\] \\\\
&\Rightarrow x_1^3 \in \[1,10^9\] \\\\
&\Rightarrow \sqrt{x_1} \in \[1,34\]
\end{aligned}
$$



> To have appropriate insight into the features is crucial to create a better model for the data by designing features to use more complex functions that fits the data better.

## Normal Equation

The normal equation is a method to solve for %$\theta%$ analytically. To do this we have to calculate the partial derivatives for all parameter %$\theta \in \mathbb{R}^{n+1}%$ of the cost function:

$$
J(\theta) = J(\theta_0,\theta_1,...,\theta_m) = \frac{1}{2m}\sum^m_{i=1} \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2
$$

The partial derivatives have then to be calculated, set zero and solved for each %$\theta_i%$:

$$
\frac{\partial}{\partial \theta_j} J(\theta) = ... = 0 \Rightarrow \theta_j = ...
$$

Here is a small example data set: 

{{<table>}}
| Size %$x_1%$ | Bedrooms %$x_2%$ | Floors %$x_3%$ | Age %$x_4%$ | Price %$y%$ |
|--------------|------------------|----------------|-------------|-------------|
| 2104         | 5                | 1              | 45          | 460         |
| 1416         | 3                | 2              | 40          | 232         |
| 1534         | 3                | 2              | 30          | 315         |
|  852         | 2                | 1              | 36          | 178         |
{{</table>}}

And of course we have to add the extra column that holds the extra feature %$x_0%$: 

{{<table>}}
| %$x_0 %$ | Size %$x_1%$ | Bedrooms %$x_2%$ | Floors %$x_3%$ | Age %$x_4%$ | Price %$y%$ |
|----------|--------------|------------------|----------------|-------------|-------------|
| 1        | 2104         | 5                | 1              | 45          | 460         |
| 1        | 1416         | 3                | 2              | 40          | 232         |
| 1        | 1534         | 3                | 2              | 30          | 315         |
| 1        |  852         | 2                | 1              | 36          | 178         |
{{</table>}}

Now we can create a %$m \times (n+1)%$ matrix with all features. %$m%$ is the number of examples and %$n%$ is the number of features. The %$+1%$ comes from the additional feature %$x_0%$.

$$
X=
  \begin{bmatrix}
    1 & 2104 & 5 & 1 & 45 \\\\
    1 & 1416 & 3 & 2 & 40 \\\\
    1 & 1534 & 3 & 2 & 30 \\\\
    1 &  852 & 2 & 1 & 36 
  \end{bmatrix}
$$

The same can be done the m-dimensional vector %$y%$:

$$
y=
  \begin{bmatrix}
    460 \\\\
    232 \\\\
    315 \\\\
    178 
  \end{bmatrix}
$$

### General Case

And now we just have to compute the following equation to get the %$\theta%$ that minimizes the cost function:

$$
\theta = (X^T X)^{-1} X^T y
$$

Now comes the **general case** for what happened before. We have %$m%$ examples %$(x^{(1)},y^{(1)}),...(x^{(m)},y^{(m)})%$ and %$n%$ features. Each training example may look like this %$n+1%$ dimensional feature vector:

$$
x^{(i)}=
  \begin{bmatrix}
    x_0^{(i)} \\\\
    x_1^{(i)} \\\\
    x_2^{(i)} \\\\
    \vdots \\\\
    x_n^{(i)} 
  \end{bmatrix}
  \in \mathbb{R}^{n+1}
$$

To construct the matrix %$X%$, the so called **design matrix**, we put the transposed vectors of all training examples until we have our %$m \times (n+1)%$ matrix:

$$
X=
  \begin{bmatrix}
    \left(x^{(1)}\right)^T \\\\
    \left(x^{(2)}\right)^T \\\\
    \vdots \\\\
    \left(x^{(m)}\right)^T 
  \end{bmatrix}
$$

A the vector %$y%$ takes all the correct labels of the data set:

$$
y=
  \begin{bmatrix}
    y^{(1)} \\\\
    y^{(2)} \\\\
    \vdots \\\\
    y^{(m)} 
  \end{bmatrix}
$$

And now we can calculate %$\theta = (X^T X)^{-1} X^T y%$.

### Noninvertibility

The reason that %$X^T X%$ is noninvertible are usually: 

- Redundant features, where two or more features are highly linearly dependent.
- Too many features with little data %$m \leq n%$.

These problems can be solved by removing features that are linearly dependent or by removing some features or use _regularization_ when there are too many features

## Comparison of GD and NE

The table gives short comparison of gradient descent and normal equation: 

{{<table>}}
|                         | Gradient Descent | Normal Equation  | 
|-------------------------|------------------|------------------|
| %$\boldsymbol{\alpha}%$ | Required         | Not required     |
| **Iterations**          | Many             | Not required     | 
| **Complexity**          | %$O(kn^2)%$      | %$O(n^3)%$, need to calculate inverse of %$X^T X%$ | 
| **Feature Scaling**     | Recommended      | Not required     |
| **When to use**         | Works well when n is large | Slow if n is large |  
{{</table>}}


To compute the inversion for the normal equation is of complexity %$O(n^3)%$. With a large number of features the normal equation will be slower than gradient descent.

At which number of features should the normal equation be dropped in favour for gradient descent? When the number of features does not exceed %$n=10000%$ the normal equation might be a suitable method, but for larger number of features gradient descent should be used.
