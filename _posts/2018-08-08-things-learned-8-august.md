---
theme: post
published: true
title: Things learned – 8 August
---
### You cannot extend selectors from outside media query in Sass
When you are writing a rule in a media query and you want to reference a class that is outside the query, you have to you mixins or other tricks, as `@extend` is not allowed (at least in libsass 3.5.4).

### Found something that looks like a solution to component class name problem
https://simonsmith.io/handling-props-and-class-names-in-react/

### Add whitespace in span in JSX
I wanted to render a `span` with a colon and space as children: `<span>; </span>`. However, it seems like JSX removes the space. I didn't want a non-breaking space (`&nbsp;`), as it would make word-wrapping look bad. I have tried escaping with `": "` or ` `;  ` `, but it didn't work.
I believe I have settled for `&nbsp;`, but right now using React.createElement seems like a interesting solution which could solve my problem.