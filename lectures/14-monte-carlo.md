---
layout: page
active: lectures
title: "Lecture 14: Monte Carlo Global Illumination"
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
    [diffuse, specular] = blinn_phong();
    reflection = raytrace(reflection_ray);
    transmission = raytrace(transmission_ray);

    sample_pts = generate_hemisphere_sample_points(256);

    ambient = 0;
    for (pt of sample_pts)
        ambient += raytrace(pt) * dot(pt, normal);

    total_color = combine(ambient, diffuse, specular, reflection, transmission);
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

**(More on this later)**
