---
theme: post
published: true
title: Thing learned – 10 August
---
### CSS Grid layout column problem
I had a container with 3 divs (one of them, `c`, is empty) and wanted to place them in a following pattern:
```
a b
  c
```
It worked just like intended. However, when I tried to change number of columns to `grid-template-columns: 1fr` to force one-column layout, the layout kept 2 columns. Almighty `!important` didn't help either.
Expected result
```
a
b
c
```
As it turned out, `c` had `grid-column: 2`, which caused layout to add a second column despite `grid-template-columns`.