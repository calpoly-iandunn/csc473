---
layout: assignment
assignment: part5
active: project
title: "Program 5"
---

## Overview:

### Goals for assignment 5:

For this portion of your ray tracer, your program needs to:

- compute intersections and appropriate shading for **boxes**
- handle large files (e.g. `balls2.pov` and `bunny.pov`) in reasonable times by including a **spatial data structure** in your code to optimize ray intersection testing
  - e.g. a bounding volume hierarchy (BVH), binary space partitioning tree (BSP tree) or an oct-tree

You will also need to create your own visually interesting scene that you render and submit the `.pov` file.


## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height> [-fresnel] [-ss=N] [-sds] [-altbrdf]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-fresnel` = use Schlick's Approximation to simulate **Fresnel** reflection
  **(optional argument)**{: class="text-warning"}
- `-ss=N` = use **super sampling** with NxN samples
  **(optional argument)**{: class="text-warning"}
- `-altbrdf` = use a **brdf** other than Blinn-Phong
  **(optional argument)**{: class="text-warning"}
- `-sds` = enable your **spatial data structure**, e.g. bounding volume hierarchy
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}

and the command `render` indicates that we simply want to draw the entire scene.

Thus:

  `raytrace render example.pov 640 480`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov`.

  `raytrace render example.pov 640 480 -sds`

will render a 640x480 image file, `output.png` consisting of the scene defined in `example.pov` using your spatial data structure optimization.

Sample input files and images are given in the input files repository.


### Diagnostic/Testing

In addition to the normal execution syntax, your program should support the following diagnostic/testing syntaxes with the given commandline arguments.
You must also continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.

---

No new commands (yet!)

If I can come up with a new diagnostic command that will be helpful in solving spatial data structure problems, I will post it here.



## Grading breakdown:

- 30 spatial data structure
- 20 points working box intersections
