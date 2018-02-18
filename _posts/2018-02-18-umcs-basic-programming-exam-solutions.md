---
theme: post
title: "UMCS Basic Programming course exam and my solutions"
categories: [umcs]
tags: [programming, c]
---

# Exercise 1
Define a struct representing a point on the Cartesian plane of floating point coordinates. Write a function accepting an array of point structures, a size of the array, an integer pointer and 4 floating point numbers: a coefficient `a` and an y-intercept `b` of two linear functions `y = ax + b`.
The function allocates and returns an array of pointers to point structures whose values are on the opposite sides of the function plots. Save the size of the array to the memory pointed to by the argument pointer. If there are no such points, function returns `NULL`.


```cpp
// for tests only
// #include <stdio.h>
#include <stdlib.h>

struct Point {
  double x;
  double y;
};
```
