---
layout: page
active: project
title: "Project"
---


## Overview

Throughout this quarter you will implement the basic functions of a distributed ray tracer.
This software will be enhanced throughout the quarter, thus this initial assignment will serve as the base for following assignments.
Since you will be adding and reusing your code, it is advised that you write your code in a clean, structured object-oriented fashion.
All code must be written in C++.

[**Input files for all stages of the project can be found here.**](https://github.com/iondune/csc473-inputfiles)

[**Test files (arguments and expected output) for all stages of the project can be found here.**](https://github.com/iondune/csc473-testfiles)

[CMakeLists.txt template for use in compiling your C++ code.](https://gist.github.com/iondune/b75501189e027c886ac12afee1274f0e)


## Milestones

- [Program 1 - File Parsing and Ray Cast](part1) due <span class="text-warning">Sunday 4/16</span> by <span class="text-warning">11:59pm</span>.
- [Program 2 - Shading, Shadows, and Camera](part2) due <span class="text-warning">Sunday 4/30</span> by <span class="text-warning">11:59pm</span>.
- [Program 3 - Reflection, Refraction, and Triangles](part3) due <span class="text-warning">Sunday 5/7</span> by <span class="text-warning">11:59pm</span>.
- [Program 4 - Transforms and Anti-aliasing](part4) due <span class="text-warning">Sunday 5/21</span> by <span class="text-warning">11:59pm</span>.
- [Program 5 - Spatial Data Structures](part5) due <span class="text-warning">Sunday 5/28</span> by <span class="text-warning">11:59pm</span>.
- [Program 6 - Monte Carlo Ray Tracing](part6) due Thursday 6/8 by <span class="text-warning">11:59pm</span>.
- Program 7 - Final Project due Thursday 6/15 by <span class="text-warning">11:59pm</span>.


## Details

### Project Turn-In

Use of a version control system is mandatory for this project and is how I will be collecting your source code for grading.
In particular, I will require everyone to use **git**.

To turn in your code, I'll need you to use a git hosting service that allows for private repositories.
I recommend [GitHub](https://github.com/) or [GitLab](https://gitlab.com/), though I am open to alternatives.
Both GitHub and GitLab offer free private repositories, though with GitHub you need to sign up for the [GitHub Student Developer Pack](https://education.github.com/pack).

### Continuous Integration

We will have a class continuous integration server that builds and tests your programs.
The server runs on an hourly basis and uses access to your hosted repositories.
There are two primary benefits provided by this server:

1. Resolve build issues early on - it's always a nightmare trying to get everyone's code to build on my machine
2. Detect cross-platform runtime issues

In order for me to grade your assignments, I will need access to your git repositories.
[Make sure to fill out this survey](https://goo.gl/forms/sedM8BmLGTyQqjts2) and then additionally grant my access to your hosted repository.

- On Github, you must [add me as a collaborator](https://help.github.com/articles/inviting-collaborators-to-a-personal-repository/)
- On Gitlab, you must [add me as a user (I only need Reporter access)](https://gitlab.com/help/workflow/add-user/add-user.md)

My username on both services is `iondune`.

### Building

You have the option of using either a **CMake** or **Make** build.
If you provide neither a `CMakeLists.txt` or `Makefile` in your repository, the server will attempt to run a command similar to:

`g++ src/*.cpp`

This will sometimes work but it is highly recommended that you provide a `CMakeLists.txt` or a `Makefile`.

Later in the quarter I might revoke the option to use a `Makefile` and require everyone to provide a `CMakeLists.txt`

### Directory Structure

The directory structure of your project may vary depending on your build system, but it should generally look something like this:

```
.
├── CMakeLists.txt
├── README.md
├── resources
│   ├── planes.pov
│   ├── simple.pov
│   ├── spheres.pov
│   └── ugly.pov
└── src
    ├── Camera.cpp
    ├── Camera.h
    ├── ... etc
    └── stb_image_write.h
```

If you're using a CMake build, I will execute these commands to run your program:

```bash
> cd build/
> cmake ..
> raytrace render /long/absolute/path/to/simple.pov 640 480
```

As such you should not make assumptions about where the `simple.pov` etc. files will be, but just use the path as specified.

That means that when you are testing your program locally, assuming a CMake build, you'll do something like this:

```bash
> raytrace render ../resources/simple.pov 640 480
```

Or better yet, you can simply clone the [input files repository](https://github.com/iondune/csc473-inputfiles) to your home directory, then do:

```bash
> raytrace render ~/csc473-inputfiles/p1/simple.pov 640 480
```

### Rounding

In order to match my output exactly for the different test input/output files, you will need to use `std::setprecision` along with cout.
For, example:

```c++
    cout << std::setprecision(4);
    cout << "Pixel: [" << X << ", " << Y << "] Ray: " << Ray << endl;
```

Also, when converting the float color values to 0-255 ints (which you should do as the last step before printing out or writing out pixel colors), you will need to round:

```c++
unsigned int red = (unsigned int) std::round(color.r * 255.f);
unsigned int green = (unsigned int) std::round(color.g * 255.f);
unsigned int blue = (unsigned int) std::round(color.b * 255.f);
```

### Image Output

Image files should be output as png files.
Use [stb_image_write.h](https://github.com/nothings/stb/blob/master/stb_image_write.h) to produce your output images unless you have a strong inclination to use some other library.
Name the output file "output.png".
See the [C++ snippets reference]({{ site.baseurl }}/references/cpp-snippets) for an example of how to write an image using stb_image_write.
