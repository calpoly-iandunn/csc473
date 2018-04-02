---
layout: page
active: lectures
title: "Lecture 13: Light Theory"
auto-title: true
---


Our goal is to answer "why is ray tracing an OK renderer?"



## Rendering Equation

From the rendering equation we know that the amount of outgoing light is a dependant integral of all incoming light:

$$
L(p, \omega_o) = \int_H  \quad  f(p, \omega_o, \omega_i)  \quad  L_i(p, \omega_i)  \quad  cos(\theta_i)  \quad  d\omega_i
$$

With the following parts:

$$ L(p, \omega_o) $$ is the outoing light in the direction $$ \omega_o $$ for a point $$ p $$

$$ f(p, \omega_o, \omega_i) $$ is the BRDF, for a point $$ p $$, in direction $$ \omega_o $$ for incoming light $$ \omega_i $$

$$ L_i(p, \omega_i) $$ is the incoming light

$$ cos(\theta_i) $$ is the attenuation of light based on $$ \omega_i $$

$$ d\omega_i $$ means we are integrating over all incoming light in the hemisphere

Note that we are only going to concern ourselves with the particle behaviour of light, not the wave-like behaviour.


### The Current Situation

Right now in our ray tracers we are essentially pretending we only have incoming light from one single direction $$ \omega_i $$ (equivalent to following the light forward)

Eventually we want to replace that with multiple sampled $$ \omega_i $$s to create a better physical approximation of light.

In the meantime, we also want to just talke a bit more detailed about light and how it is measured, and where this equation comes from.


## Radiometry

Consider light to be photons flying through space.
A photon is an elementary particle - the smallest measurement of electromagnetic radiation a.k.a. light.

We imagine that our scene is in a steady state.
In order to **measure light**, we can build a sensor in space, with a physical extent (area) $$ x $$.

Using this sensor we can count the number of photons that pass through $$ x $$ for the directions of the wedge $$ w $$.

Photons carry some amount of energy measured in **joules**.
One joule is the energy required to lift one kilo object 10cm.


### Radiometric Measurements

We can measure "how much energy is collected for a given time" by dividing by time.
A **watt** is one joule per second.

**Radiant flux** (measured in watts) is the total amount of light energy passing through a surface or region of space per unit time.

Radiant flux is often denoted $$ \Phi $$ or $$ \Phi(x, w) $$

Often total emission from a light source is described in watts (or radiant flux).


### Radiant Flux

Note that the total flux measured over a sphere $$ a $$ and a larger encompassing sphere $$ b $$ are the same,
however, less energy is passing through each small area of $$ a $$ than $$ b $$.

Energy falls off with a factor of

$$ E = \frac{\Phi}{4 \pi r^2} $$

which explains why the amount of energy falls off with distance-squared to light.

We want to get as close as we can to talking about light for a given ray - what's the difference between that and radiant flux?
Radiant flux is a measure over a region of space and an orientation in space.


### Irradiance

Note that our sensor has a normal $$ \hat n $$.
If we divide the radiant flux by the area of the sensor, we can measure energy for that area.

$$ E_{\hat n}(w, x) = \frac{\Phi(w, x)}{|x|} $$

which is

$$ \frac{radiant\;flux}{area\;of\;sensor} $$

If the sensor is broken into smaller pieces, the flux over the entire sensor is the sum:

$$ \Phi(w, x) = \sum_i \Phi(w_i, x_i) = \sum_i |x_i| E(w, x_i) $$

or considering pointwise irradiance for some area over positions $$ \tilde x $$:

$$ \Phi(w, x) = \int_x dA  \quad  E_{\hat n}(\tilde x) (w, \tilde x) $$

so

$$ E = \frac{d\Phi}{dA} $$

or

$$ \frac{\text{differential flux from light}}{\text{differential in area receiving flux}} $$

In the limit, this becomes pointwise irradiance measurement.

Why does $$ \hat n $$ matter?
If in fact our sensor was oriented differently, the amount of energy would be different.
This is **Lambert's law** - the amount of light arriving at a surface is proportional to cosine $$ \theta $$.

**Irradiance** is the area density of flux arriving at a surface.
In other words, it is the radiant flux received by a surface per unit area.

Radiant exitance is the area density of flux leaving a surface.


### Radiance

So, we have a light measurement at at point, but we still don't have "rays".
We want to consider a measurement that does not depend on $$ W $$ or the solid angle.

A **solid angle** is an extension of 2D angle to an angle on a sphere.

For a given wedge $$ W $$, the solid angle is the area of wedge on a unit sphere.
Solid angles are measured in steradians - solid angle of a sphere is $$ 4 \pi $$.
A hemisphere subtends $$ 2 \pi $$.

Thus arguably one of the most useful measure (in particular with relevance to our ray tracer) is **radiance**:
the flux density per unit area, per solid angle.
It's value is constant along a ray, thus its a "natural quantity to compute with ray tracing".

To compute radiance we need to divide out the magnitude of $$ W $$ (wedge).

$$ L_{\vec n}(W, \tilde x) = \frac{E_{\vec n}(W, \tilde x)}{|W|} $$

This is irradiance divided by steradians.

Measuring smaller and smaller wedges gets us closer to a ray - however, we have to account for the normal.
Recall Lambert's law about how much energy a point would receive.
Accounting for this, we can write the incoming radiance:

$$ L(\vec \omega, \tilde x) = \frac{1}{cos\theta} \quad  \lim\limits_{W \to \vec \omega} \frac{1}{W}  \quad  \lim\limits_{x \to \tilde x} \frac{\Phi(W, x)}{x} $$

A good thing about radiance is that it is constant along a ray in space.

$$ L(\vec \omega, \tilde x) = L(\vec \omega, \tilde x + \vec \omega) $$

We can also talk about outgoing radiance:

$$ L = \frac{d\Phi}{d\omega \; dA^\bot} $$


### BRDF

But once we have radiance, what we really care about is reflected light, where the reflected light depends on the exact diffusing nature of the material.
We'd like to define in general that the reflected light will be some proportion of the incident radiance for some wedge $$ W $$:

$$ f_{\vec x, \vec n}(W, \vec v) = \frac{L_i (\tilde x, \vec v)}{L_{\vec n}^e(W, \tilde x) |W|} $$

And by shrinking $$ W $$, we get the BRDF:

$$ f_{\tilde x, \vec n}(\vec \omega, \vec n) $$


### Rendering Equation

Once we have this notion of radiance and a BRDF, we can construct an entire reflection equation:


$$
L(\vec x, \vec v) = \int_H  \quad  f_{\vec x, \vec n}(\vec \omega, \vec v)  \quad  L_e(\vec \omega, \vec x)  \quad  cos(\theta)  \quad  d\omega
$$

Which is the BRDF multiplied by the amount of incoming light (dependant of $$ \vec n $$).
