---
layout: assignment
assignment: part6
active: project
title: "Program 6"
---

## Overview:

### Goals for assignment 6:

For this portion of your ray tracer, your program needs to:

- Support all prior rendering requirements
- Compute **global illumination** using Monte Carlo ray tracing
  - Use 64 stratified cosine-weighted sample rays on your hemisphere for the first bounce (though make this parameter configurable)
  - Use 16 stratified cosine-weighted sample rays on your hemisphere for the second bounce (though make this parameter configurable)

You will also need to create your own visually interesting scene that you render and submit the `.pov` file.



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
  **(new)**{: class="text-info"}

I also recommend that you support the following commands to control `gi` parameters:

- `-gi_samples=N,N,...` = use at most N bounces (specified for each bounce) for Monte Carlo **global illumination**,
  for example `-gi_samples=64,16` for 64 samples at first bounce and 16 at secound bounce
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}
- `-gi_bounces=b` = use at most b bounces for Monte Carlo **global illumination**,
  **(optional argument)**{: class="text-warning"}
  **(new)**{: class="text-info"}

Here are some string utility functions that can be used to handle the `-gi_samples` argument above:

```cpp
bool StringBeginsWith(const std::string & s, const std::string & prefix, std::string & remainder)
{
  if (s.size() < prefix.size())
  {
    return false;
  }

  if (s.substr(0, prefix.size()) == prefix)
  {
    remainder = s.substr(prefix.size());
    return true;
  }

  return false;
}
```

```cpp
std::vector<string> StringExplode(const std::string & str, const char delimiter)
{
  std::vector<string> words;
  std::istringstream stream(str);

  std::string word;
  while (std::getline(stream, word, delimiter))
  {
    words.push_back(word);
  }

  return words;
}
```

```cpp
std::vector<int> sampleCounts;

std::string remainder;
if (StringBeginsWith(argument, "-gi_samples=", remainder))
{
  std::vector<string> words = StringExplode(remainder, ',');
  for (const std::string & s : words)
    sampleCounts.push_back(std::stoi(s));
}
```


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
