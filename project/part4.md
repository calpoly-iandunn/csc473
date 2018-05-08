---
layout: assignment
assignment: part4
active: project
title: "Program 4"
---

## Overview:

### Goals for assignment 4:

For this portion of your ray tracer, your program needs to:

- handle **geometric transforms** on all geometric types
- handle **multiple lights**
- implement the **Fresnel effect** by using the **Schlick approximation** to weight reflection and refraction
- use **Beer's law** for colored translucent volumes (i.e. spheres)
- use **anti-aliasing** (stratified super samples per pixel)

---

In addition to your source code and build files, your repo should also contain:

- Your own `.pov` file and rendered image of a reflective and refractive scene.
  Be creative and create an interesting scene.
  Choose colors and an arrangement of geometry that you find pleasing.



## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height> [-fresnel] [-ss=N] [-altbrdf]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-fresnel` = use Schlick's Approximation to simulate **Fresnel** reflection
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}
- `-beers` = use Beer's law for colored translucent volumes
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}
- `-ss=N` = use **super sampling** with NxN samples
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}
- `-altbrdf` = use a **brdf** other than Blinn-Phong
  **(optional argument)**{: class="text-warning"}

and the command `render` indicates that we simply want to draw the entire scene.

Thus:

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov`.

  `raytrace render example.pov 640 480 -ss=3`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using 3x3 super-sampling.

Sample input files and images are given in the input files repository.


### Diagnostic/Testing

In addition to the normal execution syntax, your program should support the following diagnostic/testing syntaxes with the given commandline arguments.
You must also continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.

---

  `raytrace printrays <input_filename> <width> <height> <x> <y> [-altbrdf]`

Prints out information for each iteration of a recursive raytrace.

```
Pixel: [399, 280] Color: (125, 13, 125)
----
  Iteration type: Primary
             Ray: {0.0000 0.0000 12.0000} -> {0.1628 0.0830 -0.9832}
 Transformed Ray: {-1.0876 -0.2536 12.0000} -> {0.1125 0.0720 -0.9832}
      Hit Object: (ID #2 - Sphere)
    Intersection: {1.6662 0.8488 1.9397} at T = 10.2326
          Normal: {0.0818 0.0982 0.9918}
         Ambient: {0.2000, 0.0200, 0.2000}
         Diffuse: {0.2919, 0.0292, 0.2919}
        Specular: {0.0000, 0.0000, 0.0000}


--------------------------------------------------------------------------------
```



## Grading breakdown:

- 50 handle geometric transforms
- 15 anti-aliasing
- 10 multiple lights
- 15 working schlick's approximation
- 10 points working beer's law
