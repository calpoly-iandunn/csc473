---
layout: page
active: lectures
title: "Lecture 5: Lighting"
---

## Shading

At this point we are computing camera rays and finding intersecting objects.
Now we need to accurately light the models that our rays are intersecting.

Recall the Phong lighting model:

$$ I_{tot} = K_a * L_a + K_d * ( \hat N \cdot \hat L ) * L_d + K_s * ( \hat R \cdot \hat V ) ^ \alpha * L_s $$

Where $$ K_a $$, $$ K_d $$, and $$ K_s $$ specify the material colors for ambient, diffuse, and specular respectively.
Similarly, $$ L_a $$, $$ L_d $$, and $$ L_s $$ specify the light colors for ambient, diffuse, and specular respectively.
$$ \alpha $$ specifies the shininess of the material.
And, $$ \hat N $$, $$ \hat L $$, $$ \hat V $$, and $$ \hat R $$ specify the normal, light, reflection, and view vectors (respectively).

Note how these vectors are defined in this image.

![Blinn-Phong Vectors]({{ site.baseurl }}/images/phong_vectors.png)

The image also includes the half-vector $$ \hat H $$

In the Blinn-Phong shading model (as opposed to the Phong shading model), we replace $$ ( \hat R \cdot \hat V ) $$ with $$ ( \hat H \cdot \hat N ) $$, where the half-vector is computed by:

$$ \hat H = \frac{\hat V + \hat L}{|| \hat V + \hat L ||} $$

(i.e., the normalized sum of the view and light vectors).

Note that while this is considered an approximation, it has been found that the Blinn-Phong model more accurately reproduces lighting for many types of real-world materials.
