---
layout: page
active: syllabus
title: Syllabus
---


# Syllabus for CSC 473: Advanced Rendering Techniques

* **Instructor:** Ian Dunn
* **Office:** Building 14, Room 209
* **Office Hours:** MWF 11:10am - 12:00pm
* **Email:** idunn01@calpoly.edu
* **Schedule:**
  * Lecture: MWF {{ site.data.course.meetings.lecture.when }} ({{ site.data.course.meetings.lecture.in }})
  * Lab: MWF {{ site.data.course.meetings.lab.when }} ({{ site.data.course.meetings.lab.in }})
* **Website:** [https://iondune.github.io/csc473/](https://iondune.github.io/csc473/)



## Course Overview:

This course will cover advanced rendering techniques; global illumination via distributed ray tracer using Monte Carlo sampling, and exposure to photon mapping and point based color bleeding.

The majority of the course will be focused on students building their own software renderer (distributed ray tracer with Monte Carlo sampling) over an 8 week period. This is an opportunity to explore individual software development as you incrementally add features. Each student’s program must be implemented using C++ and will be expected to be reasonably designed using appropriate OO techniques and expected to pass established unit tests to demonstrate correctness.

This course requires substantial math and programming skills. Ultimately the goal of this course is to allow you to enhance your individual software engineering skills while building a large piece of software intended to make pretty pictures that more accurately simulate light transport (than OpenGL local lighting models).

This software development process requires you to consider data structures, performance and practice translating mathematics into C++ code. During this quarter you are expected to talk to one another and learn from one another as a community of scholars whom never copy code blindly.



## Course Objectives:

By the end of the quarter students will:

* Understand the basics of ray tracing
* Be able to correctly implement ray-sphere, ray-plane, ray-triangle intersections (with correctness demonstrated via unit testing)
* Understand and be able to apply basic object oriented design in order to create a well structured larger software project (stochastic sampled ray tracer)
* Be able to program basic data structures to represent geometric objects in a scene (sphere, planes, triangles), including the application of transforms (translate, scale, rotate) and scene objects such as lights, and the camera
* Be able to understand and implement shadow feelers to produce shadows in a software renderer (ray tracer)
* Be able to understand and implement ray traced rendering of reflective surfaces
* Be able to understand and implement ray traced rendering of refractive surfaces
* Be able to understand and implement a ‘virtual camera’
* Be able to understand the basics of Monte Carlo sampling in order to implement an approximation of global illumination via a distributed ray tracer implementation
* Be able to understand and implement a few BRDF (Bi-directional radiance distribution functions) to simulate the reflection of light
* Practice translating mathematics into C++ programs
* Exposure to and implementation of some subset of advanced topics: texture mapping, anti-aliasing, depth of field, motion blur, spatial data structures, parallel programming, point based color bleeding, photon mapping, path tracing, radiosity, scripting to produce animation, real-time ray traced rendering via writing results to framebuffers in OpenGL, etc



## Grading:

* 2 midterm exams (20% of final grade – 10% each)
* Lab (code reviews) (10% of final grade – 5% each)
* Ray tracer programming assignment (broken into 6 parts) (55% of final grade)
* Participation (5% of final grade)
* attend class/ talk in class or office hours interaction
* Final project (10% of final grade)

Please see the program description for final details. There is a strict late policy for all assignments: if your program is late you will lose: -20% within first 24 hours after deadline, -30% within 48 hours, -40% after 48 hours.



## Details

### Highly Recommended Text:
Physically Based Rendering by Matt Pharr and Greg Humphreys

### Class Style and Logistics
I expect you to participate in class and engage with the class material (studies suggest that taking longhand notes is one of the better ways to guarantee your engagement with the material in class) [[1]](http://www.theatlantic.com/technology/archive/2014/05/to-remember-a-lecture-better-take-notes-by-hand/361478/)

I also expect you to form a community of scholars for the duration of the quarter (and hopefully longer). My teaching style is very interactive. Laptops have been shown to be distracting in lecture [[2]](http://www.yorku.ca/ncepeda/laptopFAQ.html) and are not allowed unless specified (or a specific exception is negotiated) - same for cell phones.

### Cheating
All your work you hand in must be your own work, unless otherwise specified. If your program or parts of your program are plagiarized from another student or unapproved source, you will fail the course and a letter will be put in your file with Cal Poly Judicial Affairs
