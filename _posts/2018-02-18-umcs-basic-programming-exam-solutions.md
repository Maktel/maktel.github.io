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
{% raw %}
// for tests only
// #include <stdio.h>
#include <stdlib.h>

struct Point {
  double x;
  double y;
};

// the order of arguments could be changed, but I went with ax + b notation 
double f(double a, double x, double b) {
  return a * x + b;
}

// should be called in a more meaningful way, but I have no idea how to call
// points that are positioned like this
int meetsCond(struct Point p, double a1, double b1, double a2, double b2) {
  return (p.y > f(a1, p.x, b1) && p.y < f(a2, p.x, b2)) 
      || (p.y > f(a2, p.x, b2) && p.y < f(a1, p.x, b1));
}

/**
* There are few ways of constructing the main body of the function. You can:
* - first count the elements, do the allocation and then save them, that way
*   trading computation time for memory and reducing excess allocation.
*   This approach makes the most sense if you expect the function to return NULL
*   ofter, thus rendering allocation at the beginning useless for most calls.
* - dynamically allocate the memory in batches of 10 for example,
*   or in a more extreme fashion of 1, reducing memory consumption footprint
* - do just like I did and not care about allocation at all.
*/
struct Point** func(struct Point pts[], int n, int* q, double a1, double b1, 
                    double a2, double b2) {
  struct Point** res = malloc(n * sizeof(*res));
  
  *q = 0;
  for (int i = 0; i < n; ++i) {
    if (meetsCond(pts[i], a1, b1, a2, b2)) {
      res[(*q)++] = pts + i;
    }
  }

  if (!*q) {
    free(res);
    return NULL;
  }

  res = realloc(res, *q * sizeof(*res));
  return res;
}

// for tests only
// int main() {
//   struct Point pts[] = {{3, 2}, {2, 0}, {6, 50}, {9, 64}, {-10, 10}, {0, 0}};
//   int n;
//   struct Point** arr = func(pts, sizeof(pts) / sizeof(*pts), &n, 1, 5, -1, -5);
//   for (int i = 0; i < n; ++i) {
//     printf("(%lf, %lf)\n", arr[i]->x, arr[i]->y);
//   }

//   free(arr);
//   return 0;
// }
{% endraw %}
```
