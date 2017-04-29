---
layout: page
active: references
title: "Cook-Torrance Components"
---

## Normal Distribution Functions

### Blinn-Phong

$$
\Large D_{blinn} = \frac{1}{\pi \alpha^2} (\vec m \cdot \vec h)^{\left( \frac{2}{\alpha^2} - 2 \right)}
$$

![Cook_Torrance_D_BlinnPhong]({{ site.baseurl }}/images/Cook_Torrance_D_BlinnPhong.png)

### Beckmann

$$
\Large D_{beckmann} = \frac{1}{\pi \alpha^2}
\frac
{e^{\left(\frac{(\vec m \cdot \vec h)^2 - 1}{\alpha (\vec m \cdot \vec h)^2}\right)}}
{(\vec m \cdot \vec h)^4}
$$

![Cook_Torrance_D_Beckmann]({{ site.baseurl }}/images/Cook_Torrance_D_Beckmann.png)

### GGX

$$
\Large D_{GGX} =
\frac
{\alpha^2 \chi^+ (\vec m \cdot \vec h)}
{\pi \left((\vec m \cdot \vec h)^2 *(\alpha^2 + tan^2(\theta_m))\right)^2}
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
