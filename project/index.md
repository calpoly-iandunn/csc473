---
layout: page
active: project
title: "Project"
auto-title: true
---


<a href="https://classroom.github.com/a/AXydoVDc" class="btn btn-primary">GitHub Classroom</a>
<a href="https://github.com/iondune/csc473-inputfiles" class="btn btn-secondary">Input Files</a>
<a href="https://github.com/iondune/csc473-testfiles" class="btn btn-secondary">Output Files</a>

## Overview

Throughout this quarter you will implement the basic functions of a distribution ray tracer.
This software will be enhanced throughout the quarter, thus this initial assignment will serve as the base for following assignments.
Since you will be adding and reusing your code, it is advised that you write your code in a clean, structured object-oriented fashion.
All code must be written in C++.

[**The GitHub Classroom page for this assignment can be found here.**](https://classroom.github.com/a/AXydoVDc)

[**Input files for all stages of the project can be found here.**](https://github.com/iondune/csc473-inputfiles)

[**Test files (arguments and expected output) for all stages of the project can be found here.**](https://github.com/iondune/csc473-testfiles)

[CMakeLists.txt template for use in compiling your C++ code.](https://gist.github.com/iondune/b75501189e027c886ac12afee1274f0e)


## Milestones

<ul>
{% for pair in site.data.assignments %}
  {% assign name = pair[0] %}
  {% assign assignment = pair[1] %}
  <li><a href="{{ site.baseurl }}/project/{{ name }}">{{ assignment.title }} - {{ assignment.subtitle }}</a> due <strong>{{ assignment.due }}</strong>.</li>
{% endfor %}
</ul>

## Details

### Project Turn-In

Use of a version control system is mandatory for this project and is how I will be collecting your source code for grading.
In particular, we will be using [GitHub Classroom](https://classroom.github.com/).

### Continuous Integration

We will have a class continuous integration server that builds and tests your programs.
The server runs on an hourly basis and uses access to your hosted repositories.
There are two primary benefits provided by this server:

1. Resolve build issues early on - it's always a nightmare trying to get everyone's code to build on my machine
2. Detect cross-platform runtime issues

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

In order to match my output exactly for the different test input/output files, you will need to use `std::ios::fixed` and `std::setprecision` along with cout.
For, example:

```c++
    cout << std::setiosflags(std::ios::fixed);
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
Name the output file "output.png" and create it in the current directory (where the executable is run).
See the [C++ snippets reference]({{ site.baseurl }}/references/cpp-snippets) for an example of how to write an image using stb_image_write.
