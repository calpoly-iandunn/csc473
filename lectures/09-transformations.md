---
layout: page
active: lectures
title: "Lecture 9: Transformations"
---

## Transformation Sample Code

This C++ code shows how to construct a hierarchical arm using pure GLM transformation calls.
This is effectively a matrix stack construction.

```cpp
glm::mat4 m1 = glm::mat4(1.f);
m1 = glm::rotate(m1, Theta, glm::vec3(0, 0, 1));
m1 = glm::translate(m1, glm::vec3(1.75f, 0, 0));

glm::mat4 m2 = m1;
Sphere1Frame->SetTransformation(m2);
m2 = glm::scale(m2, glm::vec3(2, 1, 1));
SphereObject1->SetTransformation(m2); // Upper Arm

glm::mat4 m3 = m1;
m3 = glm::translate(m3, glm::vec3(1, 0, 0));
Sphere2Frame->SetTransformation(m3);
m3 = glm::rotate(m3, Phi, glm::vec3(0, 0, 1));
m3 = glm::translate(m3, glm::vec3(0.75f, 0, 0));
m3 = glm::scale(m3, glm::vec3(1.5f, 1, 1));
SphereObject2->SetTransformation(m3); // Fore Arm
```

## Issue with Scaling Normals

![CircleNormal]({{ site.baseurl }}/images/CircleNormalScaling.png)

When some surface undergoes a non-uniform scale transformation, the normals must be rescaled by the inverse of that transformation.
A matrix which contains the same rotation as the model matrix but with the inverse scale is called the **normal matrix**.
We can ignore the translation component of this matrix because we are transforming a normal with w component = 0.

The normal matrix $$ N $$ is constructed by taking the inverse transpose (tranpose of the inverse) of the model matrix:

$$ N = (M^{-1})^T $$

Note that by definition, the inverse-tranpose of any invertable matrix is the same as the tranpose-inverse, so we can effectively do this instead:

$$ N = (M^T)^{-1} $$

Just in case you were curious.

### Why?

The **reason** why we can't just use the model matrix on the normals is because the normals are not vectors, they are bi-vectors.
When we transform the surface, the effect on the normal is different than how the surface itself is transformed.

If you're curious why the inverse transpose accomplishes what it does, it can be explained somewhat vaguely by noting that:

1. The transpose of a rotation is the inverse of a rotation.
2. The transpose of a scale is the original scale.

So the inverse-transpose of a matrix containing scale and rotation transformations will contain the original rotation but the inverted scale.
