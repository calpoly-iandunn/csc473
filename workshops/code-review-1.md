---
layout: page
active: None
title: "Code Review 1"
---

For our first code review session we are going to do group code review followed by class-wide code review.



## Groups

First I will split everybody up into groups, probably by proximity.
Each group will have an assigned section of the raytrace, e.g.:

* Parsing
* Geometric object
* BRDF
* Scene/raytracing

Within each group, share the relevant code amongst each-other.
I think the easiest way to do this is to create a [gist](https://gist.github.com/) with your relevant snippet of code,
then dump all the links into one shared [google doc](https://docs.google.com/).

However you exchange the code snippets is fine with me!
If you have a better solution let me know and I might recommend it for future code review sessions.

Within your group, review each other's code.
As a group you decide which person's code you want to present to the class.
That person is then responsible for presenting the code.



## Class

As a code presenter you are responsible for:

* Describe the key ideas behind the major classes or functions
* If you considered alternate designs, describe them and how you made your choice
* Especially if you refactored or changed designs, describe the original state and what caused a need for change

In addition I will ask the other members of the group why they chose this particular representative.

In general for code reviews, it is best if the code speaks for itself.
As an audience member it is your responsibility to look at the code and ask questions/provide comments.

I will of course be offering my own critique from a software design and C++ perspective :)



## Software Design

I will invariably accuse nearly everyone of at least some of the following issues so be prepared!

**Large Files**: Files that are too large encourage interdependence and are generally hard to keep track of.
Make separate files for your classes and functions!
This will also speed up compile times.

**Large Functions**: If your functions are too long and involved your code is less modular and more difficult to debug,
especially more difficult to unit test.
I subscribe to [Uncle Bob Martin's belief that functions should be descriptively named and rather short](https://vimeo.com/12643301).
It's true that function call overhead can be a problem.
However, you will be more able to effectively optimize a clean and modular implementation.
It will also be more easy to detect problem areas.
It's easier to de-modularize code when necessary.
It's not always easy to go the other way.

**Short and non-descriptive variable names**: There is of course a limit to how long and descriptive you want to name variables that you have to use over and over,
but anything that is cleaner to read and review is better!

**Inconsistent Code Style**: I'm a strong believer in consistent code style, especially when it comes to whitespace and naming conventions.
