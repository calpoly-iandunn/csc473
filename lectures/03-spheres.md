---
layout: page
title: "Lecture 3: Spheres"
---


## Overview

The implict equation for a sphere is:

$$x^2 + y^2 + z^2 - r^2 = 0$$

where $$r$$ is the radius of the sphere.

Assuming the sphere has some center (offset) $$c_x, c_y, c_z$$, then the implicit equation can be:

$$(x - c_x)^2 + (y - c_y)^2 + (z - c_z)^2 - r^2 = 0$$

Using the equation of a ray:

$$p(t) = p_0 + t * d$$

We want to solve for any values of $$t$$ that are points on the sphere.



## Vectors

First we write the sphere equation in vector form (using vector dot product):

$$(p - c) \cdot (p - c) - r^2 = 0$$

Now, plug in our ray equation for p:

$$(p_0 + t*d - c) \cdot (p_0 + t*d - c) - r^2 = 0$$

If we can rearrange these terms to look like a quadratic equation ($$At^2 + Bt + C = 0$$) then we can use the quadratic equation to solve for t.

But first, let's cover some fundamentals of dot products.



### Dot Product

The dot product operator $$\cdot$$ has the following algebratic properties:

1. **Commutative Property** $$A \cdot B = B \cdot A$$
2. **Scalar Multiplcation Property** $$(c*A) \cdot B = c * (A \cdot B)$$
3. **Distributive Property** $$A \cdot (B \cdot C) = A \cdot B + A \cdot C$$

None of those should be particularily suprising (especially if you work out the underlying component-wise math) but that last property lets us do something kind of insteresting.

$$ (A + B) \cdot (A + B) = $$

$$ (A + B) \cdot A + (A + B) \cdot B =$$

$$ A \cdot A + B \cdot A + A \cdot B + B \cdot B =$$

$$ A \cdot A + 2 * A \cdot B + B \cdot B$$



## Derivation

Consider:

$$((p_0 - c) + t*d) \cdot ((p_0 - c) + t*d) - r^2 = 0$$

Multiply the first two quantities (as we just did above)

$$(p_0 - c) \cdot (p_0 - c) + 2 (p_0 - c) \cdot t * d + (d \cdot d) t^2 - r^2 = 0$$

or... (rearrange terms)

$$d^2 t^2 + 2 d \cdot (p_0 - c) t + (p_0 - c)^2 - r^2 = 0$$

(Note that any time we use $$v^2$$ for some vector v we mean $$v \cdot v$$)

Now we have a quadratic equation with the following parts:

$$ A = d \cdot d $$

$$ B = 2 d \cdot (p_0 - c) t$$

$$ C = (p_0 - c) \cdot (p_0 - c) - r^2 $$
