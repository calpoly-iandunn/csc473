---
layout: page
active: lectures
title: "Lecture 00: Introduction & Orientation"
---

Read the syllabus!

- What is Illumination?
- Review of OpenGL/Graphics
  - Rasterization
  - Ancient Times: Fixed Function Pipeline
  - Phong shading vs. Gouraud shading
  - Phong reflection model
  - Programmable (shader) graphics pipeline
  - BRDFs and "The Rendering Equation"

- General overview of program
1. read in scene description
2. compute lighting based on this scene description
3. output pixels (e.g. an image) based on the lighting you compute

- Ray Tracing
  - Create a photorealisitic 2D image from a 3D scene
    - Determine visible surfaces in an image at the pixel level
    - This is as opposed to the rasterization process which runs per-primitive and uses a z-buffer
  - Disadvantage: Per-pixel is slower, lots of duplicated work
  - Advantage: Shadows, reflections, etc. are easy to do (compared to rasterization where it is nigh imposssible)
