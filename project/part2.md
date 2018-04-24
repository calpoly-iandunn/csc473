---
layout: assignment
assignment: part2
active: project
title: "Program 2"
---

## Overview:

### Goal for assignment 2:

In this assignment you will implement the direct illumination shading and shadow casting (and drawing) for your ray tracer along with a free camera, supporting only spheres and planes in terms of intersections.

You code must include implementation of camera transforms and multiple objects. Specific technologies that must be supported are computation of:

- the shading for opaque non-reflective objects using the **Blinn-Phong BRDF**
  - your shading must use the light and material properties specified in the pov file.
- **shadow feeler rays** for all intersections that are used for computing shadows for all objects.
  - please experiment with appropriate epsilon offsets to avoid shadow acne.
- correct images for **camera with arbitrary world view**, i.e not only looking down the negative z axis (e.g. simp_cam.pov files)
- optionally, for **extra credit**, implement the **Cook-Torrance BRDF**
  - in order to switch between the two different BRDF models use a command line option (see "program execution" below)

Software engineering considerations:
You will once again need to support specific unit tests throughout the quarter for specific rays (relative to the camera – i.e. listed in pixel space, e.g. [Xi, Yi]).
For this assignment, expect to have plane intersections tested, along with shading values for specific rays.
At this time, it should be clear that a highly modular implementation with many helper methods makes it easier to implement the required commands.

## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height> [-altbrdf]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-altbrdf` = use Cook-Torrance instead of Blinn-Phong
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}

and the command `render` indicates that we simply want to draw the entire scene.
Note that in this context, **optional** refers to whether the argument is required for
your raytracer to run at all, and has nothing to do with whether you must implement the parameter
for this assignment.

Thus:

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using the Blinn-Phong BRDF for shading.

  `raytrace render example.pov 640 480 -altbrdf`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using your alternate BRDF for shading, perhaps Cook-Torrance.

Sample input files and images are given in the input files repository.

### Diagnostic/Testing

In addition to the normal execution syntax, your program should support the following diagnostic/testing syntaxes with the given commandline arguments.
You must also continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.

---

  `raytrace pixelcolor <input_filename> <width> <height> <x> <y> [-altbrdf]`

where `input_filename` `width` and `height` are the same as for the `render` command, and the other options are:

- `x` = the x coordinate of the pixel to test
- `y` = the y coordinate of the pixel to test
- `-altbrdf` = use a brdf other than Blinn-Phong <span class="text-warning">(optional argument)</span>

This command casts a ray in the the scene found in `input_filename` and for the pixel (`x`, `y`), finds the first object that is hit.
It then computes the shaded color for this particular pixel, using the indicated BRDF.

    > raytrace pixelray example.pov 640 480 319 239
    Pixel: [319 239] Ray: {0 0 14} -> {0.220943 0.127908 -0.966863}
    T = 17.6582
    Object Type: Sphere
    BRDF: Blinn-Phong
    Color: (91, 33, 106)

---

    > raytrace pixelray example.pov 640 480 319 239 -altbrdf
    Pixel: [319 239] Ray: {0 0 14} -> {0.220943 0.127908 -0.966863}
    T = 17.6582
    Object Type: Sphere
    BRDF: Alternate
    Color: (78, 31, 119)

(these numbers are made up, I will post correct output to match soon!)

If no object is it, it simply prints `No Hit`



## Grading breakdown:
- 40 points Blinn-Phong working with all files
- 40 points working shadows with all files
- 20 points working camera with arbitrary views with all files
- **Extra Credit** 10 points Cook-Torrance BRDF working with all files
