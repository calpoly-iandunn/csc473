---
layout: page
title: "Lecture 4: Planes"
---

## Overview

A plane is defined by a point $$p_1$$ and a normal $$\hat n$$.
The plane consists of all points for which $$(p-p_1)$$ is perpendicular to $$\hat n$$.

Or, using the dot product:

$$f(p) = (p - p_1) \cdot \hat n = 0$$

Recall our equation for a ray:

$$p(t) = p_0 + t * d$$

Plugging in for $$p$$ gives us:

$$f(p) = (p_0 + t * d - p_1) \cdot \hat n = 0$$



## Derivation

Solve for $$t$$:

$$ ((p_0 - p_1) + d*t) \cdot n = 0 $$

$$ (p_0 - p_1) \cdot n + d * t \cdot n = 0 $$

$$ (p_0 -p_1) \cdot n = -t * (d \cdot n) $$

$$ t = \frac{(p_1 - p_0) \cdot n}{d \cdot n} $$

But now... $$p_0$$ is the origin of our ray but we need a $$p1$$

Povray gives us planes in this format:

```POV-Ray
plane {
    <A, B, C>, D
}
```

Where `<A, B, C>` is the normal of the plane and `D` is the "distance" of the plane, or how far from the origin the plane is along the normal.
Therefore, $$\hat n * D$$ is a point on the plane.

$$ t = \frac{(\hat n * D - p_0) \cdot n}{d \cdot n} $$

The dot product of a normalized vector with itself is 1, so we really have:

$$ t = \frac{D - (p_0 \cdot n)}{d \cdot n} $$
