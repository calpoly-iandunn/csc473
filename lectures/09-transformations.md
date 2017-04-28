---
layout: page
active: lectures
title: "Lecture 9: Transformations"
---

## Transformation Sample Code

```cpp
glm::mat4 m1 = glm::mat4(1.f);
m1 = glm::rotate(m1, Theta, glm::vec3(0, 0, 1));
m1 = glm::translate(m1, glm::vec3(1.75f, 0, 0));

glm::mat4 m2 = m1;
Sphere1Frame->SetTransformation(m2);
m2 = glm::scale(m2, glm::vec3(2, 1, 1));
SphereObject1->SetTransformation(m2);

glm::mat4 m3 = m1;
m3 = glm::translate(m3, glm::vec3(1, 0, 0));
Sphere2Frame->SetTransformation(m3);
m3 = glm::rotate(m3, Phi, glm::vec3(0, 0, 1));
m3 = glm::translate(m3, glm::vec3(0.75f, 0, 0));
m3 = glm::scale(m3, glm::vec3(1.5f, 1, 1));
SphereObject2->SetTransformation(m3);
```

## Issue with Scaling Normals

![CircleNormal]({{ site.baseurl }}/images/CircleNormalScaling.png)
