---
layout: page
active: references
title: "Cook-Torrance Components"
---

General form of the Cook-Torrance shading model:

$$ r = k_a + \sum_{i=0}^n l_c * (\vec n \cdot \vec l) * ( d * r_d + s * r_s ) $$

$$ s + d = 1 $$

$$ s $$ in this equation is the `metallic` material property.

Diffuse reflection is often modelled using Lambertian:

$$ r_d = k_d $$

There are other options, however, including Oren-Neyer.

Cook-Torrance specular reflectance (also sometimes simply called microfacet reflectance or microfacet BRDF) has the form:

$$ r_s = \frac{D * G * F}{4 * (\vec n \cdot \vec l) * (\vec n \cdot \vec v)} $$


$$ D $$ is the Normal Distribution Function, which describes the concentration of microfacets which are oriented such that they *could* reflect light. [(1)](#ref-naty)

$$ G $$ is the Geometric Shadowing-Masking Function, which describes the percentage of microfacets which are neither shadowed or masked by neighboring microfacets. [(1)](#ref-naty)

$$ F $$ is the Fresnel Function, which describes the amount of light reflected by the material (as opposed to being absorbed or transmitted by the material).

## Symbols

$$ \vec n $$ is the macrosurface normal.

$$ \vec l $$ is the light vector.

$$ \vec v $$ is the view vector.

$$ \vec m $$ is the microsurface normal.

$$ \alpha $$ is the roughness constant.
$$ \alpha $$ is defined as such using the `roughness` material property from our `.pov` files:

$$ \alpha = roughness^2 $$

## Normal Distribution Functions

The $$ D $$ function is always positive. [(2)](#ref-ggx)
It is also normalized to satisfy certain geometric properties. [(3)](#ref-ndf)

This normalization is the source of the $$ \frac{1}{\pi \alpha^2} $$ factors you see in the Blinn-Phong and Beckmann variations.

$$ D(m) $$ is the concentration of microfacets oriented with subsurface normal $$ m $$.
For our microfacet model we plug in $$ h $$ since we want $$ D(h) $$, the concentration of microfacets which can reflect light from $$ \hat l $$ to $$ \hat v $$. [(1)](#ref-naty)

### Blinn-Phong

$$
\Large D_{blinn} = \frac{1}{\pi \alpha^2} (\vec m \cdot \vec n)^{\left( \frac{2}{\alpha^2} - 2 \right)}
$$

![Cook_Torrance_D_BlinnPhong]({{ site.baseurl }}/images/Cook_Torrance_D_BlinnPhong.png)

### Beckmann

$$
\Large D_{beckmann} = \frac{1}{\pi \alpha^2}
\frac
{e^{\left(\frac{(\vec m \cdot \vec n)^2 - 1}{\alpha (\vec m \cdot \vec n)^2}\right)}}
{(\vec m \cdot \vec n)^4}
$$

![Cook_Torrance_D_Beckmann]({{ site.baseurl }}/images/Cook_Torrance_D_Beckmann.png)

### GGX

$$
\Large D_{GGX} =
\frac
{\alpha^2 \chi^+ (\vec m \cdot \vec n)}
{\pi \left((\vec m \cdot \vec n)^2 *(\alpha^2 + tan^2(\theta_m))\right)^2}
$$

![Cook_Torrance_D_GGX]({{ site.baseurl }}/images/Cook_Torrance_D_GGX.png)


## Geometric Functions

### Cook-Torrance

$$ \Large
G_{cook-torrance} =
min \left( 1, \frac{2 (\vec m \cdot \vec h) (\vec n \cdot \vec v)}{(\vec v \cdot \vec h)}, \frac{2 (\vec m \cdot \vec h) (\vec n \cdot \vec l)}{(\vec v \cdot \vec h)} \right)
$$

![Cook_Torrance_G_Cook_Torrance]({{ site.baseurl }}/images/Cook_Torrance_G_Cook_Torrance.png)

### GGX

$$ \Large
G_{GGX} =
G_1(\vec v) * G_1(\vec l)
$$

$$ \Large
G_1(x) =
\chi^+(\frac{\vec x \cdot \vec m}{\vec x \cdot \vec n})
\frac{2}{1+\sqrt {1 + \alpha^2 tan^2(\theta_x))}}
$$

![Cook_Torrance_G_GGX]({{ site.baseurl }}/images/Cook_Torrance_G_GGX.png)


## Fresnel Functions

### Schlick's Approximation

$$ F_0 = \frac{(n - 1)^2}{(n + 1)^2} $$

$$ F = F_0 + (1 - F_0)*(1 - (\vec v \cdot \vec h))^5 $$



## References

<a name="ref-naty"></a>
1: [Naty Hoffman, **Background: Physics and Math of Shading**, 2012](http://blog.selfshadow.com/publications/s2012-shading-course/hoffman/s2012_pbs_physics_math_notes.pdf).
From the [SIGGRAPH 2012 Practical Physically Based Shading in Film and Game Production](http://blog.selfshadow.com/publications/s2012-shading-course)

<a name="ref-ggx"></a>
2: [Walter et al., **Microfacet Models for Refraction through Rough Surfaces**, 2007](http://www.cs.cornell.edu/~srm/publications/EGSR07-btdf.pdf).
Introduced the GGX form of D and G functions.

<a name="ref-ndf"></a>
3: [Nathan Reed, **How Is The NDF Really Defined?**, 2013](http://www.reedbeta.com/blog/hows-the-ndf-really-defined/).
Describes where the normalization factor of D function comes from.

<a name="ref-rants"></a>
4: [Brian Karis, **Specular BRDF Reference**, 2013](http://graphicrants.blogspot.com/2013/08/specular-brdf-reference.html).
Reference of D, F, and G functions.
Written by a UE4 graphics programmer.
