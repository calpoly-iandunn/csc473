---
layout: page
active: lectures
title: "Lecture 8: Triangles"
---


## Derivation

### Equations

Using barycentric coordinates a point can be defined as:

$$ P(\alpha, \beta, \gamma) = \alpha * v_1 + \beta * v_2 + \gamma * v_3 $$

Where

$$ \alpha + \beta + \gamma = 1 $$

If all three inequalities are true, then the point is in the triangle:

$$ 0 < \alpha < 1 $$

$$ 0 < \beta < 1 $$

$$ 0 < \gamma < 1 $$

We can write the equation in vector-edge notation as:

$$ P(\alpha, \beta, \gamma) = a + \beta * (v_2 - v_1) + \gamma * (v_3 - v_1) $$

Let's change the vertices to a, b, c because it's easier to write.

$$ P(\alpha, \beta, \gamma) = a + \beta * (b - a) + \gamma * (c - a) $$

Let's plug in our standard ray equation:

$$ P_0 + t * \vec d = a + \beta * (b - a) + \gamma * (c - a) $$

Since this is a vector form it is really three different equations:

$$ P_0x + t * x_d = x_a + \beta * (x_b - x_a) + \gamma * (x_c - x_a) $$

$$ P_0y + t * y_d = y_a + \beta * (y_b - y_a) + \gamma * (y_c - y_a) $$

$$ P_0z + t * z_d = z_a + \beta * (z_b - z_a) + \gamma * (z_c - z_a) $$

Here we have three equations and three unknowns.
It's a linear system we can solve using matrix algebra.
First let's move all the unknowns to one side:

$$ (x_a - x_b) * \beta + (x_a - x_c) * \gamma + x_d * t = x_a - P_0x $$

$$ (y_a - y_b) * \beta + (y_a - y_c) * \gamma + y_d * t = y_a - P_0y $$

$$ (z_a - z_b) * \beta + (z_a - z_c) * \gamma + z_d * t = z_a - P_0z $$


### Matrices

Let's rewrite in matrix form:

$$
\begin{bmatrix}
x_a - x_b       &   x_a - x_c      &   x_d \\
y_a - y_b       &   y_a - y_c      &   y_d \\
z_a - z_b       &   z_a - z_c      &   z_d \\
\end{bmatrix}

\begin{bmatrix}
\beta \\
\gamma \\
t \\
\end{bmatrix}

=

\begin{bmatrix}
x_a - P_0x \\
y_a - P_0y \\
z_a - P_0z \\
\end{bmatrix}
$$



### Solve

Below are the results of applying Cramer's rule.
Note the vertical bars around a matrix mean to compute the determinant.

$$
\beta = \frac{
    \begin{vmatrix}
    x_a - P_0x      &   x_a - x_c       &   x_d \\
    y_a - P_0y      &   y_a - y_c       &   y_d \\
    z_a - P_0z      &   z_a - z_c       &   z_d \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    A
    \end{vmatrix}
}
$$

$$
\gamma = \frac{
    \begin{vmatrix}
    x_a - x_b       &   x_a - P_0x      &   x_d \\
    y_a - y_b       &   y_a - P_0y      &   y_d \\
    z_a - z_b       &   z_a - P_0z      &   z_d \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    A
    \end{vmatrix}
}
$$

$$
t = \frac{
    \begin{vmatrix}
    x_a - x_b       &   x_a - x_c       &    x_a - P_0x \\
    y_a - y_b       &   y_a - y_c       &    y_a - P_0y \\
    z_a - z_b       &   z_a - z_c       &    z_a - P_0z \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    A
    \end{vmatrix}
}
$$


## Pseudocode

```pascal
hit_tri(ray r, vert a, vert b, vert c, t0, t1)

    compute t
    if (t < t0) || (t > t1) then
        return 0

    compute gamma
    if (gamma < 0) || (gamma > 1) then
        return 0

    compute beta
    if (beta < 0) || (beta > 1 - gamma) then
        return 0

    return 1
```
