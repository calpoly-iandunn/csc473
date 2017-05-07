---
layout: page
active: project
title: "Program 3 - Reflection, Refraction, and Triangles"
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

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using your alternate BRDF for shading, perhaps Cook-Torrance.

Sample input files and images are given in the input files repository.

### Diagnostic/Testing

In addition to the normal execution syntax, your program must continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.
You may also support the optional command below, which is useful for debugging issues.
The command is **not required** for full creidt on the assignment.
However, it is an extremely useful tool to use if you have problems generating correct images.

---

  `raytrace pixeltrace <input_filename> <width> <height> <x> <y> [-altbrdf]`

Prints out information for each iteration of a recursive raytrace.

```
Pixel: [370, 270] Color: (60, 18, 61)
o - Iteration type: Primary
|   Ray: {0 0 14} -> {0.1044 0.06307 -0.9925}
|   Hit Object ID (1 - Sphere) at T = 11.43, Intersection = {1.193 0.7208 2.656}
|   Normal {0.3978 0.2403 0.8854}
|   Transformed Ray: {0 0 14} -> {0.1044 0.06307 -0.9925}
|   Ambient: 0.1, 0, 0.1
|   Diffuse: 0.06088, 0, 0.06088
|   Specular: 0, 0, 0
|
|\
| o - Iteration type: Refraction
| |   Ray: {1.193 0.7208 2.655} -> {-0.03511 -0.0212 -0.9992}
| |   Hit Object ID (2 - Sphere) at T = 4.607, Intersection = {1.032 0.6231 -1.948}
| |   Normal {-0.421 -0.1639 0.8921}
| |   Transformed Ray: {1.193 0.7208 2.655} -> {-0.03511 -0.0212 -0.9992}
| |   Ambient: 0.152, 0.276, 0.16
| |   Diffuse: 0, 0, 0
| |   Specular: 0, 0, 0
| |   Extra Info: into-object
| |----
|
 \
  o - Iteration type: Reflection
  |   Ray: {1.193 0.7208 2.656} -> {0.7585 0.4581 0.4634}
  |   No intersection.
  |----


--------------------------------------------------------------------------------
```







## Grading breakdown:
- 27 points working triangle intersections
- 27 points working reflection
- 37 points working refraction
- 9 points general sanity
