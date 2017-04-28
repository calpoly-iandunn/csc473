---
layout: page
active: lectures
title: "Lecture 7: Reflection and Refraction"
---

Now that we have shading & shadows in our scene, what else is there other than geometric complexity?

We started with a purely local approximation of shading.
By adding shadows, we begin the process of adding global constraints to our system.



## Reflections

What else does light do?
Well, for mirror-like surfaces, very shiny surfaces, other part's of the scene's geometry is reflected onto those shiny surfaces.

We can model this type of surface reflection for reflective surfaces (e.g. any object with a `reflective` finish property greater than 0 in our `.pov` files)
by __adding__ color from any geometry intersected by reflected rays.

For some incident ray $$ \vec d $$ and normal $$ \vec n $$, the reflection vector is:

$$ \vec r = \vec d - 2 * (\vec d \cdot \vec n) * \vec n $$



## Refractions



## Schlick's Approximation


