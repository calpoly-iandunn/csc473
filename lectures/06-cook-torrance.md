---
layout: page
active: lectures
title: "Lecture 6: Cook-Torrance"
---

In 1982, Robert Cook and Kenneth Torrance published a reflectance model that more accurately represented the physical reality of light reflectance
than the Phong and Blinn-Phong models we discussed in [Lecture 5]({{ site.baseurl }}/lectures/05-lighting).

The proposed method is general purpose - it has three "pluggable" components that can be replaced with equations of your choice.
It is also effective at representing a variety of materials, whereas a BRDF like Blinn-Phong is really only good at representing plastics and some metals (though that point is debatable).
As such, it is still in common usage today.
Though that is perhaps a moot point, since the Phong reflection model is a weak approximation from 1975 that is still in common usage.
Regardless...



## Equations

With previous shading models, we had something of the form

$$ r = k_a + \sum_{i=0}^n l_c * ( r_d + r_s ) $$

With

$$ r_d = k_d * (\hat n \cdot \hat l) $$

$$ r_s = k_s * (\hat h \cdot \hat n) ^ p $$

To compute total reflectance $$ r $$ with the Blinn-Phong model.
Here we have defined $$ r_d $$ for the diffuse reflectance and $$ r_s $$ for the specular reflectance.
Note the "power" term of the specular highlight is now $$ p $$ to avoid confusion with forcoming equations.

This is the same equation we have been using and have already implemented.

Cook and Torrance propose a variation of this equation:

$$ r = k_a + \sum_{i=0}^n l_c * (\hat n \cdot \hat l) * ( s * r_d + d * r_s ) $$

With

$$ r_d = k_d $$

This equation introduces two concepts:

1. The diffuse and specular reflectance components are now scaled by some variables $$ s $$ and $$ d $$.
2. We pull the $$ \hat n $$ and $$ \hat l $$ dot product out of the diffuse reflectance and make it a part of the sum.
  This makes our $$ r_d $$ a constant, though we can modify this definition later if we choose to.
  This also implies that our $$ r_s $$ specular reflectance is also multiplied by the same dot product (unlike before in Blinn-Phong).

As we will soon learn, however, $$ r_s $$ now includes a $$ (\hat n \cdot \hat l) $$ divisor, which will simply cancel out with the $$ (\hat n \cdot \hat l) $$ in the numerator.
This convention is adopted for two reasons:

1. It fits better with how the forthcoming $$ r_s $$ equation is derived.
2. To be confusing.

Those I suspect that the second reason is only from my perspective.

Nevertheless, note that many texts and explanations adopt this convention.


### What about $$ s $$ and $$ d $$ ?

$$ s $$ and $$ d $$ are a constant ratio that balances out the diffuse and specular contributions to our reflectance.

$$ s + d = 1 $$

Sometimes we ignore the $$ s $$ altogether and simply write:

$$ r = k_a + \sum_{i=0}^n l_c * (\hat n \cdot \hat l) * ( s * r_d + (1 - s) * r_s ) $$

This ratio is based on the observation that (except for emissive materials), conservation of energy implies that a material can only
reflect as much energy (light) as it takes in.

Therefore if a material reflects a lot of light specularly, then it must reflect less light diffusely.


### What about $$ r_s $$ ?

I'm getting there.

$$ r_s = \frac{D * G * F}{4 * (\hat n \cdot \hat l) * (\hat n \cdot \hat v)} $$

Note the n-dot-l divisor that I warned would be there!

Note also that in the literature and other places you will sometimes find $$ \pi $$ instead of $$ 4 $$ in the denominator.
*This is an error that was made in the original Cook-Torrance paper and has thus been repeated in many places.*
The correct derivation of this equation has the $$ 4 $$.
It's a pretty small difference in a constant factor, though, so it's not incredibly important to get it right.


### What about $$ D * G * F $$ ?

$$ D $$, $$ G $$, and $$ F $$ are the three "pluggable" functions that make up the Cook-Torrance specular reflectance.
You can choose from a wide variety of options for these equations.
To talk about what they mean, however, we need to discuss the core theory behind the Cook-Torrance model.



## Microfacets

Cook-Torrance is a microfacet model, which means that it approximates surfaces as a collection of small individual faces called microfacets.
You can think of these microfacets as being polygonal approximations of the surface at a sub-pixel level.

![Microfacets]({{ site.baseurl }}/images/microfacet.png)

The important concept here is that the normal of the surface and the normals of each microfacet may not be the same.
For exceptionally smooth surfaces, like a perfect mirror, all the microfacets face the same direction which is $$ \hat n $$, the normal of the surface.
However, for matte or rough surfaces, such as matte paint or stone, the microfacets face random directions.

On rough surfaces, these microfacets can occlude other facets, cast shadows on facets, and be pointing away from the normal.

The Cook-Torrance model attempts to account for these three phenomena.


### So... what about $$ D * G * F $$ ?

$$ D $$ is the Normalized Distribution Function, which accounts for the fraction of facets which reflect light at the viewer.

$$ G $$ is the Geometric Attenuation Function, which accounts for the shadowing and masking of facets by one another.

$$ F $$ is the Fresnel Function, which accounts for the *Fresnel* Effect, which causes rays with high angle of incidence to reflect with more specularity.

$$ G $$ and $$ F $$ have a less significant contribution to the final shaded result than $$ D $$, so we will cover $$ D $$ first.



## Normalized Distribution

$$ D $$ is the part of the Cook-Torrance model which defines the shape of the specular highlight.
There are a lot of choices here.
It may not be surprising that one of the choices is **Blinn-Phong**, that is, we can choose to use the same specular highlight shape as we previously used in the Blinn-Phong reflectance model.

$$ D_{blinn} = \frac{1}{\pi \alpha^2} (\hat h \cdot \hat n)^{(\frac{2}{\alpha^2} - 2)} $$

Now this looks sort of familiar!
Except, we are dividing by something now... and our clean "power" exponent has been replaced by something more nasty.

Let's explain these two changes - starting with the divisor.


### Normalization

Our $$ D $$ functions for Cook-Torrance need to be normalized such that:

$$ \int_\Omega D(h)(\hat h \cdot \hat n) d\omega_i = 1 $$

The result of this normalization in our Blinn-Phong case (and indeed, for many different cases) is a factor of $$ \frac{1}{\pi \alpha^2} $$

But what is $$ \alpha $$ ?


### Power exponent

In traditional Blinn-Phong we had a simple power parameter (usually called `shininess`) that we used to control the size and intensity of the specular highlight.

In Cook-Torrance we want to have a more general parameter that can be used in multiple places.
Cook-Torrance defines a constant $$ \alpha $$ that indicates the roughness of the material, with `0` indicating ideal smooth surfaces and `1` indicating maximum roughness.
In practice you never want to use absolute `0` or `1` since these edge cases tend to produce divide-by-zero and other issues.

We can then relate our new $$ \alpha $$ parameter to our old `power` variable as such:

$$ power = (\frac{2}{\alpha^2} - 2) $$

Due to the way $$ \alpha $$ scales, I have found success using the following convention from UE4.
I define a `roughness` parameter in the material and set

$$ \alpha = roughness^2 $$

