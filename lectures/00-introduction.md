---
layout: page
active: lectures
title: "Lecture 0: Introduction & Orientation"
auto-title: true
---

Read the syllabus!

## Review of OpenGL/Graphics

### Rasterization

1. Geometry comes down the pipeline
2. Transformed to correct positions
3. Each primitive is **rasterized**
  - Determine lighting at vertices
  - Linearly interpolate across the face
  - Use z-buffer to resolve depth

### Phong Reflection

A closer look at "step 1": How do we determine the lighting?

We have reflection models, e.g. the Phong Reflection Model.
These are a local approximation of the way light interacts with the face.

We have **ambient** + **diffuse** + **specular**.

Ambient light is a hack to add a small amount of global illumination to the scene
(and only depends on material properties).

Diffuse is light reflected in all directions - only depends on the orientation of the face and the light source.

$$ max(N \cdot L, 0) $$

Specular light is reflected off the surface like a mirror - in a straight line.

$$ (V \cdot R)^\alpha $$

Where $$\alpha$$ is the "shininess".
The larger $$\alpha$$ is, the smaller the spot light is.
Strongest when $$ V = R $$.


### Local Approximation

The consequence: only "local" light approximated.
What is global light?

- Shadows
- Reflection & refraction

Not impossible, but usually hacky and limited.

So, let's explore a completely different rendering alternative: ray tracing.



## Ray Tracer

General overview of program:

1. read in scene description
2. compute lighting based on this scene description
3. output pixels (e.g. an image) based on the lighting you compute

- Ray Tracing
  - Create a photorealisitic 2D image from a 3D scene
    - Determine visible surfaces in an image at the pixel level
    - This is as opposed to the rasterization process which runs per-primitive and uses a z-buffer
  - Disadvantage: Per-pixel is slower, lots of duplicated work
  - Advantage: Shadows, reflections, etc. are easy to do (compared to rasterization)

Rasterization: process a bunch of data, use transforms to mimic 3D.

Ray tracing: closer to a simulation of how lights/cameras work.


### Illumination

How does light really work?

For our simulation (ray tracer) we need to take into account all the light inter-reflections.
In other words, for a single point we need to account for all the incoming light from all directions,
from light sources and "bounced light", and then compute how that incoming light is reflected
differently in every direction based on the surface's **BRDF** (bi-directional radiance distribution function).

This can be computed for a particular point, in a particular viewing direction.

If we integrate all the incoming light (over a half sphere, for example) multiplied by the surface reflection and
$$ cos(\theta) $$ (incident angle) i.e. the reflectance, we can compute how much light leaves a given point.

The rendering equation is a geometric approximation of this:

$$
L_o(x, \omega_o, \lambda, t) =
L_e(x, \omega_o, \lambda, t) +
\int_\Omega  \;  f_r(x, \omega_i, \omega_o, \lambda, t)  \;  L_i(x, \omega_i, \lambda, t)  \;  (-\omega_i \cdot n)  \;  d\omega_i
$$

Or

The total light of wavelength $$\lambda$$ directed along $$\omega_o$$ at time $$t$$ from $$x$$ is
the emitted light ($$L_e$$) plus the BRDF (proportion of light reflected for this surface) multiplied by the
incoming light multiplied by the attenuation of light due to angle.

Various algorithms try to compute the more global properties of light, notable radiosity, but one can also compute more
global attributes of light using rays.

Consider if we traced a finite set of rays coming from the light source and computed how they "bounced" around and lit up a scene,
accumulating their "color" from each reflection.

Of course, tracing from the light is very inefficient because many rays will not be "seen", thus we only trace the "visible" rays,
those that can be seen by the virtual camera.


