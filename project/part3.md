---
layout: assignment
assignment: part3
active: project
title: "Program 3"
---

## Overview:

### Goal for assignment 3:

In this assignment you will implement reflection and refraction on mirrored and translucent surfaces.
You will also need to compute intersections with triangles.
Please work on these features incrementally using the appropriate files.
For example:

- Start with adding parsing and intersections for triangles and use the file `simple_tri.pov` to test your triangle intersections.
- Add reflections and test with `simple_reflect1.pov` (reflective plane) and `simple_reflect2.pov` (reflective spheres) and `simple_reflect3.pov` (spheres and planes)
- Add refractions (use `simple_refract.pov`)
- Test them all with `recurse_simp2.pov`

Reflection and refraction should be implemented via recursion.
Limit the recursion to `6` bounces, but I recommend making that number configurable.
You will be building on to your previous code, and thus, your program should include shading and shadows.

---

In addition to your source code and build files, your repo should also contain:

- Your own .pov file and rendered image of a reflective and refractive scene.
  Be creative and create an interesting scene.
  Choose colors and an arrangement of geometry that you find pleasing.


## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height> [-altbrdf]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-altbrdf` = use a brdf other than Blinn-Phong <span class="text-warning">(optional argument)</span>

and the command `render` indicates that we simply want to draw the entire scene.

Thus:

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using the Blinn-Phong BRDF for shading.

  `raytrace render example.pov 640 480 -fresnel`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` including Fresnel reflectance.

Sample input files and images are given in the input files repository.

### Diagnostic/Testing

In addition to the normal execution syntax, your program should support the following diagnostic/testing syntaxes with the given commandline arguments.
You must also continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.

---

  `raytrace printrays <input_filename> <width> <height> <x> <y> [-altbrdf]`

Prints out information for each iteration of a recursive raytrace.

```
Pixel: [315, 185] Color: (33, 33, 33)
----
  Iteration type: Primary
             Ray: {0.0000 0.0000 14.0000} -> {-0.0093 -0.1128 -0.9936}
      Hit Object: (ID #1 - Sphere)
    Intersection: {-0.1058 -1.2819 2.7103} at T = 11.3627
          Normal: {-0.0353 -0.4273 0.9034}
         Ambient: {0.0500, 0.0500, 0.0500}
         Diffuse: {0.0419, 0.0419, 0.0419}
        Specular: {0.0000, 0.0000, 0.0000}
----
  Iteration type: Refraction
             Ray: {-0.1058 -1.2818 2.7093} -> {0.0029 0.0345 -0.9994}
      Hit Object: (ID #1 - Sphere)
    Intersection: {-0.0901 -1.0917 -2.7929} at T = 5.5054
          Normal: {-0.0300 -0.3639 -0.9310}
         Ambient: {0.0500, 0.0500, 0.0500}
         Diffuse: {0.0000, 0.0000, 0.0000}
        Specular: {0.0000, 0.0000, 0.0000}
        Extra Info: into-object
----
  Iteration type: Refraction
             Ray: {-0.0901 -1.0915 -2.7938} -> {0.0150 0.1811 -0.9833}
  No intersection.
        Extra Info: into-air


--------------------------------------------------------------------------------
```







## Grading breakdown:
- 25 points working triangle intersections
- 25 points working reflection
- 30 points working refraction
- 10 points working beer's law
- 10 points general sanity
