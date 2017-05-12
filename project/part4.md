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

  `raytrace render <input_filename> <width> <height> [-altbrdf] [-ss=N]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-altbrdf` = use a brdf other than Blinn-Phong <span class="text-warning">(optional argument)</span>
- `-ss=N` Set super-sampling to use NxN samples per pixel, stratified <span class="text-warning">(optional argument)</span>

and the command `render` indicates that we simply want to draw the entire scene.

Thus:

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using the Blinn-Phong BRDF for shading.

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using your alternate BRDF for shading, perhaps Cook-Torrance.

Sample input files and images are given in the input files repository.


### Diagnostic/Testing

In addition to the normal execution syntax, your program must continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.



## Grading breakdown:

- 20 handle geometric transforms
- 15 anti-aliasing
- 5 multiple lights
- 10 working schlick's approximation
- 10 general sanity
