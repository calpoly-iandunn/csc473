---
layout: page
permalink: /project/part1/
title: "Program 1 - File Parsing and Ray Cast"
---

Due Friday April 14th, 2017 at 11:55pm

---

## Overview:

There are (approximately) five stages to your final ray tracer:

- Parsing the scene description file
- Computing ray-object intersections
- Shading
- Recursive Tracing (reflection, refraction and shadows)
- Write out resulting image

This initial assignment if focused on the first two of these stages.

### Goal for assignment 1:

Write a ray caster that can intersect rays with spheres and planes from a pinhole camera model as specified in the povray file.
You do not need to compute lighting, just color the pixels the pigment of the closest intersections with a sphere or plane.
Your code will need to:

- Parse a subset of the scene description file, specially, simple.pov which is available <a href="https://github.com/iondune/csc473-inputfiles">here</a>.
- Compute ray-sphere and ray-plane intersections
- Color pixels based the pigment of the closest intersection
- Write out resulting image

### Software engineering considerations:

As the initial assignment for your ray tracer, you need to think about designing and implementing the abstract object(s) to represent geometry in your scene and write the parsing code to read in the scene description file (see below).
In general, all geometric objects in your world will need to be able to be parsed, intersected (by rays), and shaded.
You can use a vector/matrix library, options include glm and eigen.

In addition, your ray tracer will need to support specific unit tests throughout the quarter.
In general, this will require that for a specific ray (relative to the camera – i.e. listed in pixel space, e.g. [Xi, Yi]),  can be tested – for example, having various values returned throughout the ray tracing process.
This will include values such as the ray’s point, and direction, distance to closest object, color of intersected object, and then later derived ray’s from the original ray.

---

## Scene Description Language

The scene description language that we will be using will be based (loosely) on a subset of the Povray format.
The big difference is that we will be using a right-handed coordinate system for our world, object and camera coordinates.

Your raytracer must be able to parse the following types:

- **// comments**
- **camera**
- **light_source**
- translate, scale, rotate
- box
- **sphere**
- **plane**
- triangle
- **pigment**
  - **color**
    - **rgb**
    - **rgbf**
- finish
  - ambient
  - diffuse
  - specular
  - roughness
  - reflection
  - refraction
  - ior

The bold elements are the types that MUST be parsed for this first assignment (you may ignore the zero translate at this time if you would like).
You should create an abstract object from which all of the ray tracer objects will be derived.
Each derived object should have its own parse function (read) that takes the filestream in, processes the data for that object, and returns the altered file pointer (for example).

*Please be careful when writing your parser – depending on white space is likely not a good idea because over the class, white spaces will vary.
Also note pigment, finish and transforms order can vary within a given object.
Try to write a flexible parser.*

---

## Ray Object Intersections

After parsing the scene file and creating the necessary data structures, your program should begin casting rays.
The camera object should, with knowledge of the output image size and a given pixel, be able to cast the necessary rays and return the appropriate rgb color value for the pixel.
In the assignment you will only cast one ray per pixel (this will change in later assignments however).
The rays should be represented by an object.
To cast a ray, simply traverse the scene object list (also represented by C++ objects) testing for intersection with each object.
Each derived geometry object should have its own intersection routine that takes a ray, performs an intersection test and returns the closest intersection (if one exists).
The closest intersection will be shaded using the model described in the next section.
However, in the first pass of the algorithm you may wish to color every intersection a constant color to test for correct intersection.

### Notes for the camera for this assignment
For this assignment,  you may work under the assumption that the camera is positioned down the positive Z axis, looking down the negative Z axis (we will next implement a complete virtual camera).
Note this is how the camera values are specified in the sample povray file.
To compute the value of the rays, note that the limits of the near plane (i.e. the left, right, top and bottom) are defined by the camera up and left vectors.
Assuming the ‘eye’ represents the center of projection of the camera, and the near plane is one unit in front of the camera, divide the world space defined by these extents by the number of pixels specified as input to the program (see ‘program execution below’) to generate sample points ‘per’ pixel in world space.
Use this point to compute a vector for the ray’s direction.

---

## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height>`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height

and the command `render` indicates that we simply want to draw the entire scene.

Thus:

  `raytrace render sample.pov 640 480`

will render a 640x480 image file, `sample.png` consisting of the scene defined in `sample.pov`.

Image files should be output as png files. Use [stb_image_write.h](https://github.com/nothings/stb/blob/master/stb_image_write.h) to produce your output images unless you have a strong inclination to use some other library.

Sample input files and images are given on the class webpage.
For later assignments, you will need to also submit rendered images.
Please include a `README.txt` file in your repository that contains a description of which parts of the ray tracer that you believe are working, partially working, and not implemented.
This will assist the grader in determining what is causing potential errors in your output and help in assigning partial credit.

### Diagnostic/Testing

In addition to the normal execution syntax, your program should support the following diagnostic/testing syntaxes with the given commandline arguments:

---

  `raytrace sceneinfo <input_filename>`

This diagnostic command simply loads the povray scene in, parses it, and prints out the contents.
Official output TBA, but it will look something like this:

    > raytrace sceneinfo simple.pov
    Camera:
    - Location: {0 0 14}
    - Up: {0 1 0}
    - Right: {1.33333 0 0}
    - Look at: {0 0 0}

    1 light(s)
    Light[0]:
    - Location: {-100 100 100}
    - Color: {1.5 1.5 1.5}

    2 object(s)
    Object[0]:
    - Type: Sphere
    - Center: {0 0 0}
    - Radius: 2
    - Color: {1 0 1}
    - Material:
      - Ambient: 0.2
      - Diffuse: 0.4
    - Transform:
      - Translate: {0 0 0}

    Object[1]:
    - Type: Plane
    - Normal: {0 1 0}
    - Distance: -4
    - Color: {0.2 0.2 0.8}
    - Material:
      - Ambient: 0.4
      - Diffuse: 0.8
    - Transform:
      - Translate: {0 0 0}

---

  `raytrace pixelray <input_filename> <width> <height> <x> <y>`

where `input_filename` `width` and `height` are the same as for the `render` command, and the other options are:

- `x` the x coordinate of the pixel to test
- `y` the y coordinate of the pixel to test

This command simply prints out the direction and origin of the ray given the camera description found in `input_filename` and for the pixel (`x`, `y`).
For example:

    > raytrace pixelray sample.pov 640 480 319 239
    Pixel: [319 239] Ray: {0 0 14} -> {0.220943 0.127908 -0.966863}

(these numbers are made up, I will post correct output to match soon!)

---

  `raytrace firsthit <input_filename> <width> <height> <x> <y>`

where `input_filename` `width` and `height` are the same as for the `render` command, and the other options are:

- `x` the x coordinate of the pixel to test
- `y` the y coordinate of the pixel to test

This command casts a ray in the the scene found in `input_filename` and for the pixel (`x`, `y`), finds the first object that is hit.
It then prints the type of object hit, the T for the given ray intersection, and the color of the hit object.

    > raytrace pixelray sample.pov 640 480 319 239
    Pixel: [319 239] Ray: {0 0 14} -> {0.220943 0.127908 -0.966863}
    T = 17.6582
    Object Type: Sphere
    Color: (91, 33, 106)

(these numbers are made up, I will post correct output to match soon!)

If no object is it, it simply prints `No Hit`

### Grading breakdown:

- 40 points file parsing (evidenced via ray cast working)
- 30 points sphere intersections (working ray cast)
- 30 points plane intersection (working ray cast)
