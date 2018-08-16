---
theme: post
published: true
title: Things learned – August 16
---
### Tagged template literals
You can prepend a template literal in Javascript with a tag. This way the function with tag's name will be called with literal as an argument.
```javascript
> function foo() { console.log('foo:', arguments); }
> const bar = "Maktel 🕴"
> foo `Hello world, ${bar}!`
// -> foo: [Arguments] { '0': [ 'Hello world, ', '!' ], '1': 'Maktel 🕴' }
```
