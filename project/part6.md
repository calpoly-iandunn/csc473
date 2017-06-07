---
layout: page
active: project
title: "Program 6 - Monte Carlo Ray Tracing"
---

## Overview:

### Goals for assignment 6:

For this portion of your ray tracer, your program needs to:

- Support all prior rendering requirements
- Compute global illumination using Monte Carlo ray tracing
  - Please use 128 cosine weighted sample rays on your hemisphere for the first bounce (though make this parameter configurable)
  - Use 32 cosine weighted sample rays on your hemisphere for the second bounce (though make this parameter configurable)

You will also need to create your own visually interesting scene that you render and submit the .pov file.



## Program execution:

Your program should have the following syntax:

  `raytrace render <input_filename> <width> <height> [-fresnel] [-ss=N] [-sds] [-gi]`

assuming that your executable is named `raytrace` where the options are:

- `input_filename` = the name of the povray file to read and render
- `width` = the image width
- `height` = the image height
- `-fresnel` = use Schlick's Approximation to simulate **Fresnel** reflection
  **(optional argument)**{: class="text-warning"}
- `-ss=N` = use **super sampling** with NxN samples
  **(optional argument)**{: class="text-warning"}
- `-sds` = enable your **spatial data structure**, e.g. bounding volume hierarchy
  **(optional argument)**{: class="text-warning"}
- `-gi` = use Monte Carlo **global illumination**
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-success"}

I also recommend that you support the following commands to control `gi` parameters:

- `-gi_samples=N` = use N sampes for Monte Carlo **global illumination**
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-success"}
- `-gi_bounces=b` = use at most b bounces for Monte Carlo **global illumination**
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-success"}
- `-gi_ratio=r` = divide N by r for each bounce of Monte Carlo **global illumination**
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-success"}

Sample input files and images are given in the input files repository.


### Diagnostic/Testing

In addition to the normal execution syntax, your program should support the following diagnostic/testing syntaxes with the given commandline arguments.
You must also continue to support all Diagnostic/Testing syntaxes from the previous iteration(s) of the project.

---

No new commands (yet!)

If I can come up with a new diagnostic command that will be helpful in solving spatial data structure problems, I will post it here.



## Grading breakdown:

- 30 all prior requirements working
- 55 Monte-Carlo ray tracing as evidenced by color bleeding
- 15 general sanity
