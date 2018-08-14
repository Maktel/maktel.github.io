---
theme: post
published: true
title: Things learned – 14 August
---
### Sass variables in `calc()`
In order to use Sass variables in `calc()`, you have to interpolate the name:
```css
$elem-width: 140px;

body {
  width: calc(100% - 2em - #{$elem-width});
}
```