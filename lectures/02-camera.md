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

## Camera Rays

Given some camera specification, we need to be able to compute view rays:

```POV-Ray
camera {
    location <0, 0, 14>
    up <0, 1, 0>
    right <1.333, 0, 0>
    look_at <0, 0, 1>
}
```

Right now let's pretend out camera will always be looking down -Z

Assuming the image to render has width $$w$$ and height $$h$$, a given pixel $$i, j$$ has the following "image coordinates":

$$U_s = -\frac{1}{2} + \frac{i + 0.5}{w}$$

$$V_s = -\frac{1}{2} + \frac{j + 0.5}{h}$$

With this definition, $$