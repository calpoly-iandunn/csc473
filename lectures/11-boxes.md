---
layout: page
active: lectures
title: "Lecture 11: Boxes"
---


## Overview

We can represent a bounding box as a bounded region between $$ x_{min} $$ and $$ x_{max} $$ on the **x** axis, and between $$ y_{min} $$ and $$ y_{max} $$ on the **y** axis.

{% include scaled-image.html name="BoxIntersection1" %}

For a 3D box we simply need to add a similar constraint $$ z_{min} $$ and $$ z_{max} $$.
We'll live in the 2D world for now.

For a given ray we can compute the intersections with all four of our parallel lines.
We'll call these intersections $$ t_{x min} $$, $$ t_{x max} $$, etc.
These are the **t** values for the ray, intersecting with each of the four respective lines.

{% include scaled-image.html name="BoxIntersection2" %}

If the largest of the **t ? min** values is greater than the smallest of the **t ? max** values, then the ray does not intersect the box.

For example, in the above **ray 1** we can see that $$ t_{x min} > t_{y max} $$, therefore there is no intersection.

{% include scaled-image.html name="BoxIntersection3" %}

For **ray 2** we can see that the largest min value, $$ t_{x min} $$ is less than the smallest max value, $$ t_{y max} $$, therefore there is intersection.

{% include scaled-image.html name="BoxIntersection4" %}

For **ray 3** we can see that $$ t_{y min} > t_{x max} $$, therefore there is no intersection.



## Intersections

So how do we find the intersection **t** values?

Recall our equation of a ray:

$$ P(t) = P_0 + t * \vec d $$

We can solve this for each dimension individually, thus we want to solve for t such that:

$$ x_{min} = P_{0x} + t * d_x $$

or

$$ t = \frac{x_{min} - P_{0x}}{d_x} $$

Thus we can compute the four **t** values:

$$ t_{x min} = \frac{x_{min} - P_{0x}}{d_x} $$

$$ t_{x max} = \frac{x_{max} - P_{0x}}{d_x} $$

$$ t_{y min} = \frac{y_{min} - P_{0y}}{d_y} $$

$$ t_{y max} = \frac{y_{max} - P_{0y}}{d_y} $$

And we can apply the test using the following pseudo-code:

```perl
if max(t_x_min, t_y_min) > min(t_x_max, t_y_max) then
    return false # no hit
else
    return true
```

Note that if the ray is traveling the opposite direction we need to swap the mins and maxes.
In general we are concerned with the order in which the rays reach the two lines.

Thus we can generalize our approach (which also makes it easier to add a 3rd dimension later)

```perl

tgmin = infinity
tgmax = -infinity

# for x

t1 = (x_min - P_0_x) / dx
t2 = (x_max - P_0_x) / dx

if t2 > t1 then
    swap(t1, t2)
    # Because we want the closest t1 from the perspective of the ray

if t1 > tgmin then tgmin = t1
if t2 < tgmax then tgmax = t2

# same for y, z

if tgmin > tgmax then return false
if tgmax < 0 then return false # box behind

# If we're still here, it's an intersect, and tgmin is the intersction point
return (true, tgmin)

```

If the ray is parallel to any of the faces of our box, e.g. if `dx == 0`, we'll get a divide-by-zero when we try to do this.
However, if that's the case we just need to check the start point of the ray.


```perl

if dx == 0 then
    if P_0_x >= x_min or P_0_x <= x_max then
        return false
else
    # Do the check we did before

```

