---
theme: post
published: true
title: Things learned – 8 August
---
### You cannot extend selectors from outside media query in Sass
When you are writing a rule in a media query and you want to reference a class that is outside the query, you have to you mixins or other tricks, as `@extend` is not allowed (at least in libsass 3.5.4).

### Found something that looks like a solution to component class name problem
https://simonsmith.io/handling-props-and-class-names-in-react/