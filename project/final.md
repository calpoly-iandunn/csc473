---
layout: page
active: project
title: "Final Project"
auto-title: true
---

You must demo your final project results during the final exam period on {{ site.data.assignments.final.due }}.

You must also submit a website detailing your project, results images, references, and software development process.
This is due on {{ site.data.assignments.final.website }}.

## Requirements

There are four components to this project.

1. Implement an additional feature for your raytracer or any interesting graphics technique.
   Some reasonable ideas can be found below.
2. Provide 2-3 `.pov` files of your own creation that show off your new feature (or are generally visually interesting)
3. Create an HTML website that describes your project and shows some screenshots (and/or videos if appropriate)
4. Demonstrate good software engineering principles in your raytracer

For that last bullet point you have two options:

### Describe Existing Software Design

Describe an abstraction (e.g. set of functions or classes/interfaces) you used in implementing your ray tracer
that was effective at organizing your code, allowing debugging, and/or promoting extensibility.
Point out where this can be found in your raytracer code.

### Refactor For Better Software Design

Refactor a messy part of your raytracer in a way that:

   1. Preserves functionality
   2. Reduces code redundancy
   3. Extracts functionality
   4. Improves testability

Any and all of those attributes are a plus.
Anyone who writes unit tests using a unit testing framework (like [Catch](https://github.com/philsquared/Catch), my personal favorite)
will score instant brownie points.

## Documentation

I expect to see some reasonable documentation in your `README.md` of:

1. What feature you chose and why
2. What resources you used to research and implement the feature
3. Explanation/description of your Software Design implementation


## Past Projects & Ideas

Past final project directories:

* [2017](http://users.csc.calpoly.edu/~idunn01/teaching/csc473/finals17/)
* [2013](http://users.csc.calpoly.edu/~zwood/teaching/csc473/final13/)
* [2011](http://users.csc.calpoly.edu/~zwood/teaching/csc473/finals11/)
* [2010](http://users.csc.calpoly.edu/~zwood/teaching/csc473/finalw10/)


Some general ideas:

* Ambient occlusion
* Depth of field
* Perlin noise
  * [OpenSimplex noise implementation - C++](https://gist.github.com/tombsar/716134ec71d1b8c1b530)
  * [Idea images](http://fooo.fr/~vjeux/epita/raytracer/rt/docs/img/hall_of_fame/perlin.png)
* Soft lighting
* Constructive Solid Geometry
  * [Example image](https://upload.wikimedia.org/wikipedia/commons/8/8b/Csg_tree.png)
  * [Slides](http://web.cse.ohio-state.edu/~parent.1/classes/681/Lectures/19.RayTracingCSG.pdf)
* Participating media (volumes)
* Texture mapping
  * [Past student work](http://users.csc.calpoly.edu/~idunn01/teaching/csc473/finals17/jsheriff/)
* Physics/animation
  * To take a series of still images and stich together into a movie, there are a number of tools:
    * [ffmpeg](https://trac.ffmpeg.org/wiki/Slideshow)
    * [virtualdub](http://www.virtualdub.org/blog/pivot/entry.php?id=34) (windows only)
    * This seems like a quick and easy online tool: [gifmaker.org](http://gifmaker.org/) (thanks to Henry)
* Motion blur
* Glossy reflection
* Ray marching ([mandelbulb](https://en.wikipedia.org/wiki/Mandelbulb))
  * [Real-time ray marching example](https://www.shadertoy.com/view/Xds3zN)
* Photon mapping
* Path tracer
