---
layout: page
active: project
title: "Program 4 - Transforms and Anti-aliasing"
---

## Overview:

### Goals for assignment 4:

For this portion of your ray tracer, your program needs to:

- handle all previous specifications with modifications to the way reflection and refraction are weighted by using the Schlick approximation to weight reflection and refraction
- handle geometric transforms on all geometric types
- handle multiple lights
- use anti-aliasing (9 stratified super samples per pixel) (be able to turn this on and off with command line)

---

In addition to your source code and build files, your repo should also contain:

- Your own .pov file and rendered image of a reflective and refractive scene.
  Be creative and create an interesting scene.
  Choose colors and an arrangement of geometry that you find pleasing.



## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height> [-fresnel] [-ss=N] [-altbrdf]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-fresnel` = use Schlick's Approximation to simulate **Fresnel** reflection <span class="text-warning">(optional argument)</span>
- `-ss=N` = use **super sampling** with NxN samples <span class="text-warning">(optional argument)</span>
- `-altbrdf` = use a **brdf** other than Blinn-Phong <span class="text-warning">(optional argument)</span>

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

- 20 handle geometric transforms
- 15 anti-aliasing
- 5 multiple lights
- 10 working schlick's approximation
- 10 general sanity
