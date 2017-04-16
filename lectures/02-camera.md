---
layout: page
active: lectures
title: "Lecture 2: Camera"
---

## Overview

Our ray tracing task requires us to:

1. Compute a "viewing ray" for each pixel in the output image,
2. Find the closest intersection point in the scene
3. Set the pixel value based on material (and later, on lighting)

**So what is a ray?**

A starting point $$p_0$$ and a direction (normalized) $$\hat d$$

The ray is represented parametrically using $$t$$ for time:

$$p(t) = p_0 + t * \hat d$$

In general, we test for intersections against geometric objects by solving for valid values of $$t$$



## Pixel Rays

Given some camera specification, we need to be able to compute view rays.
Let's assume that our camera is sitting somewhere along the positive Z axis and is looking towards -Z.
We want to define some image plane in 3D space.
Then, we can map a given pixel in our output image to a point in this image plane, and use that to find the ray for that particular pixel.

![Camera Image Plane]({{ site.baseurl }}/images/povraycam.png)

For a given pixel $$i, j$$, let's define a set of coordinates coordinates $$U_s, V_s$$.
We'll call these the "view space coordinates".
Where $$w$$ and $$h$$ are the output image width, height:

$$U_s = \frac{i}{w}$$

$$V_s = \frac{j}{h}$$

This will give us an image plane that stretches from $$(0, 0)$$ to $$(1, 1)$$ in view space coordinates.
But, we want the image plane to stretch from $$(-1/2, -1/2)$$ to $$(1/2, 1/2)$$.



## Camera Space

```POV-Ray
camera {
    location <0, 0, 14>
    up <0, 1, 0>
    right <1.333, 0, 0>
    look_at <0, 0, 1>
}
```

Our camera has a set of basis vectors $$\hat u$$, $$\hat v$$, and $$\hat w$$.
These basis vectors

Assuming the image to render has width $$w$$ and height $$h$$, a given pixel $$i, j$$ has the following "image coordinates":

$$U_s = -\frac{1}{2} + \frac{i + 0.5}{w}$$

$$V_s = -\frac{1}{2} + \frac{j + 0.5}{h}$$



## Summary

Your rays should be in world space, but we get those world space vectors by computing $$U_s$$, $$V_s$$, and $$W_s$$.

$$U_s$$ and $$V_s$$ are as defined above page, $$W_s$$ is always -1.

Then you multiply $$U_s$$ by the $$\hat u$$ vector, etc. for $$\hat v$$ and $$\hat w$$.

$$\hat u$$ is the right vector (and is not normalized, use it as scaled from the povray file), $$\hat v$$ is the up vector (normalize) and $$\hat w$$ is the opposite of the "look" vector, which is normalized look_at - camera location.

$$ P_0 = camera.location $$

$$ \hat d = U_s * \hat u + V_s * \hat v + W_s * \hat w $$
