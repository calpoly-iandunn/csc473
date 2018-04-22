---
layout: page
active: lectures
title: "Lecture 14: Monte Carlo Global Illumination"
auto-title: true
---

[In class, this lecture was presented with this slide deck.](https://docs.google.com/presentation/d/1MWH-IcK2I0JK39sADBSA0qj9zyM2W9JWPYme-MKkUNQ/edit?usp=sharing)

## Monte Carlo

The **Monte Carlo Methods** are a broad class of algorithms that use repeated random sampling to solve problems.
In computer graphics, we often use Monte Carlo methods to estimate the value of integrals.



## Global Illumination

The problem of global illumination involves finding a way to simulate light that bounces off of parts of the scene onto other parts of the scene.
Another word for this phenomenon is color bleeding.
The idea is that if a red sphere is near to a blue wall, we would expect to see some red light reflected onto the blue wall.

When we added support for reflection rays to our ray tracers, that was in some ways a form of indirect (i.e. global) illumination.
However, it was only for perfectly smooth mirror-like surfaces.
We would like to add support for indirect lighting for rough surfaces as well.

Specifically, this means replacing our simple `ambient` term with a recursively gathered illumination value that is based
on the color of nearby objects, not just the color of the object itself.
Whereas with reflection we needed to cast a single ray into the scene to gather indirect light,
we must now cast a large number of rays in all directions to gather diffusely reflected indirect light.

Before we talk about specifics, though, we should first introduce a few topics from **Probability Theory** to give this some
mathematical backing.



## Probability Theory

### Random Variables

Suppose we have a random variable $$ x $$.
This means that $$ x $$ can take on a wide variety of values.
We call individual samples of this random variable $$ x_1 $$, $$ x_2 $$, etc.

The random variable $$ x $$ is governed by some probability density function $$ p $$.
This function tells us the comparative likeliness of different values of $$ x $$.
For example, if

$$ p(x_1) = 6 $$

and

$$ p(x_2) = 3 $$

then we know that $$ x $$ is twice as likely to produce values at or near $$ x_1 $$ as it is likely to produce values at or near $$ x_2 $$.

This relationship between $$ x $$ and $$ p $$ is indicated with the following notation:

$$ x \sim p $$

In general, $$ p(x) $$ is used as part of an integral to indicate likelihood for some given region.
All probability density functions must satisfy the following equation:

$$ \int_\infty^\infty p(x) dx = 1 $$

Which means that the probability that $$ x $$ is between negative and positive infinity is 1 (which of course is true given that any sample of x is a real number).

More generally,

$$ \int_a^b p(x) dx $$

gives us the probability that $$ x $$ will land in the region $$ [a, b] $$

If a random variable $$ x $$ is equally likely to produce any value within its range, we say that it has a uniform distribution.
The standard number generator in C++ (and in most languages) produces numbers in a specified range `0` to `RAND_MAX` with uniform distribution.


### Functions of Random Variables

Given some function $$ f $$, we say that the expected value of $$ f(x) $$ is:

$$ E(f(x)) = \int_{x \in S} f(x) p(x) d\mu \approx \frac{1}{N} \sum_{i=1}^N f(x_i) $$

The expected value is simply what we expect the average of repeated samples of $$ f(x) $$ to approach for a very large number of samples $$ N $$.
This value can be analytically computed using the integral but this integral is not usually easy to compute.
Instead we can simply produce a large number of samples.

Note that in the above equation we introduced a few new variables.
We have $$ S $$, which is the space in which we are generating samples, and $$ d\mu $$, which is a small region of that space.

As you might imagine, in computer graphics we are often concerned with approximating intervals over some hemisphere.
Thus the space $$ S $$ that we are concerned with is the surface of hemisphere, and $$ d\mu $$ is a small area of surface on that hemisphere.

This form of the expected value function is not useful to us since we generally want to approximate an integral of some known function,
not the function multiplied by a pdf.
So let's just do some minor term substitution:

$$ g = f p $$

$$ \int_{x \in S} g(x) d\mu \approx \frac{1}{N} \sum_{i=1}^N \frac{g(x_i)}{p(x_i)} $$

Note that this new equation is often written using $$ f $$ instead of $$ g $$ since it no longer contains any references to $$ f $$.
Yes, this is yet another entry in the "letters reused to mean similar things" saga.
Sorry!

$$ \int_{x \in S} f(x) d\mu \approx \frac{1}{N} \sum_{i=1}^N \frac{f(x_i)}{p(x_i)} $$

This equation is very useful for us because it lets us approximate an integral of an arbitrary function
by computing random samples of that function and dividing by the probability density function of our random variable generation.
If we pick a random generator with uniform distribution this is just a constant factor.
However, we can sometimes pick a probability density function that is better suited to the problem at hand.



## Rendering Equation

So how can we apply the Monte Carlo method and probability theory to our global illumination problem?
Let's look back at the rendering equation:

$$
L(p, \omega_o) = \int_H  \quad  f_r(p, \omega_o, \omega_i)  \quad  L_i(p, \omega_i)  \quad  cos(\theta_i)  \quad  d\omega_i
$$

Where:

$$ L(p, \omega_o) $$ is the outgoing light in the direction $$ \omega_o $$ for a point $$ p $$

$$ f_r(p, \omega_o, \omega_i) $$ is the BRDF, for a point $$ p $$, in direction $$ \omega_o $$ for incoming light $$ \omega_i $$

$$ L_i(p, \omega_i) $$ is the incoming light

This is the integral that we want to approximate.
However, for a variety of reasons we want to split up the direct and indirect illumination:

$$
L(p, \omega_o) = L_d(p, \omega_o) + L_a(p, \omega_o)
$$

Where $$ L_d $$ is the direct illumination and $$ L_a $$ is the indirection/ambient illumination.

We will continue to calculate $$ L_d $$ in the way we always have, using shadow feelers and the material brdf.

However, we will now calculate the $$ L_a $$ indirect light using the Monte Carlo method to approximate the integral:

$$ L(p, \omega_o) = \int_H  \quad  f_r(p, \omega_o, \omega_i)  \quad  L_r(p, \omega_i)  \quad  cos(\theta_i)  \quad  d\omega_i $$

Where $$ L_r $$ is the recursively-gather light reflected off of other objects.

For the sake of simplicity, we'll ignore the specular component of $$ f_r $$ (the brdf) when it comes to reflected global illumination.
This is a reasonable choice that saves us some headache in exchange for a relatively unchanged image.



## Pseudo-Code

```javascript
function raytrace(ray) {
    let diffuse, specular;
    [diffuse, specular] = blinn_phong();

    let reflection = raytrace(reflection_ray);
    let transmission = raytrace(transmission_ray);

    let sample_pts = generate_hemisphere_sample_points(256);

    let ambient = 0;
    for (pt of sample_pts)
        ambient += raytrace(pt) * dot(pt, normal);

    let total_color = combine(ambient, diffuse, specular, reflection, transmission);
    return total_color;
}
```

Note that we assume constant brdf, e.g. purely diffuse surface.
This also assumes that our generated hemisphere points have uniform distribution with respect to $$ d\mu $$,
i.e. with respect to surface area of the hemisphere.



## Sample Distribution

This approach will work but is not optimal.
Specifically, the random sampling approach to Monte Carlo inherently brings noise into the image.
Increasing the number of sample points (i.e. rays) will reduce noise, but also significantly increase runtime.
In general, an `N` increase in rays will produce an `sqrt(N)` decrease in noise.
So if we can find a way to decrease noise without increasing ray count, that will be incredibly valuable to us.

We can accomplish this by modifying the distribution function of our random hemisphere samples.
Specifically, if we choose a distribution function with **cosine weighting**, that is, $$ p(x) = cos(\theta) $$,
we can eliminate the $$ cos(\theta) $$ component of our function.
We'll also significantly improve the image quality (i.e. reduce noise, or variance)
since we'll be concentrating our rays in the direction that matters,
avoiding rays near the edge of the hemisphere which have little effect on the image.

As it happens, we can generate random samples on a hemisphere with cosine weight
by simpling generating uniform samples on a disc (uniform with respect to area)
and projecting them upwards onto the hemisphere.
The code for this generation/projection looks like this:

```javascript
function generateCosineWeightedPoint(u, v) {
    let radial = Math.sqrt(u);
    let theta = 2.0 * pi * v;

    let x = radial * Math.cos(theta);
    let y = radial * Math.sin(theta);

    return [x, y, Math.sqrt(1 - u)];
}
```

Here, `u` and `v` are random parameters generated uniformly between `0` and `1`.
We'll discuss these parameters further later, but it would suffice to simply use something like this:

```c
const float u = rand() / (float) RAND_MAX;
const float v = rand() / (float) RAND_MAX;
```


### Other Distributions

If we choose the model indirect light including specular highlights or according to a different BRDF (e.g. Cook-Torrance),
we need to choose a different distribution that "matches the shape" of the function we are approximating.
We won't bother to do that in this class but just be aware!



## Hemisphere Alignment

Our function `generateCosineWeightPoint` gives us points on a hemisphere which is oriented on the coordinate frame
with the top of the hemisphere pointing upwards (e.g. towards `<0, 0, 1>`).
We need to align this hemisphere with the surfel we are rendering.
Specifically, we need to point it towards the normal.

We can do this by transforming the generated vector by a rotation matrix which rotates
from the sample hemisphere direction (i.e. `<0, 0, 1>`) to the normal.

Here's some pseudocode:

```javascript
function alignSampleVector(sample, up, normal)
{
    let angle = Math.acos(Math.dot(up, normal));
    let axis = Math.cross(up, normal);

    let matrix = make_rotation_matrix(angle, axis);

    return transform(matrix, sample);
}
```



## Stratifed Samples

I mentioned that we would revist the `u` and `v` parameters from the function `generateCosineWeightedPoint`.
While purely random values for these parameters would work, we can reduce noise in the output image by using stratified samples.
This means dividing the `u*v` space (e.g. a square) into `N` distinct areas,
then picking `N` samples each of which are a random sample within their respective small area.

If `N` is not a perfect square, you'll need to be careful about how you subdivide the `u*v` space.
I recommend simply duplicating a few sample regions.



## Implementation Details

Since we are now simulating indirect light it's a good idea to turn off the constant ambient approximation.

We also need to figure out what material properties we should apply to the color value calculated by indirect illumination.
You could make strong arguments for multiplying by either `material.diffuse` or `material.ambient`.
I recommend simply using the value outright.
