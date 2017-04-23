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

Note how these vectors are defined in this image:

![Blinn-Phong Vectors]({{ site.baseurl }}/images/phong_vectors.png)

Note in particular how the view vector $$ \hat V $$ points towards the viewer from the surface.
When computing this value in your ray tracer, you will need to negate the direction of your ray.

The image also includes the half-vector $$ \hat H $$

In the Blinn-Phong shading model (as opposed to the Phong shading model), we replace $$ ( \hat R \cdot \hat V ) $$ with $$ ( \hat H \cdot \hat N ) $$, where the half-vector is computed by:

$$ \hat H = \frac{\hat V + \hat L}{|| \hat V + \hat L ||} $$

(i.e., the normalized sum of the view and light vectors).

Note that while this is considered an approximation, it has been found that the Blinn-Phong model more accurately reproduces lighting for many types of real-world materials.

Also note that this is *not* an approximation when the vectors are co-planar (as in this image) - in that case, it is equivalent.
It is an approximation when the vectors are not co-planar.


### Multiple Lights

To support multiple lights, you loop over each light and sum up the diffuse and specular contributions (but do not duplicate ambient light):

$$ I_{tot} = K_a * L_a + \sum_{i=0}^n \left( K_d * ( \hat N \cdot \hat L_i ) * L_{di} + K_s * ( \hat H_i \cdot \hat N ) ^ \alpha * L_{si} \right) $$



## Shadows

Pseudocode:

```
raycolor(ray p0 + t * d)

if (you_hit_something_in_scene @ t1) then

    p = p0 + d * t1 // solve for point

    color = obj.color * obj.amb // no matter what, always add the ambient color

    if (you_dont_hit_anything_on_way_to_light_with_ray p + s * l) then

        vec h = normalized(normalized(l) + normalized(-d)) // half-angle approximation

        color += obj.color * obj.diff * light.color * max(0, dot(obj.normal, l))
        color += obj.color * obj.spec * light.color * max(0, dot(h, obj.normal)) ^ obj.shininess

else

    color = background_color

```
