---
theme: post
published: true
title: Things learned – August 16
---
### Tagged template literals
You can prepend a template literal in Javascript with a tag. This way the function with tag's name will be called with literal as an argument.
```javascript
function foo() { console.log('foo:', arguments); }
const bar = "Maktel 🕴";
foo `Hello world, ${bar}!`;
// -> foo: [Arguments] { '0': [ 'Hello world, ', '!' ], '1': 'Maktel 🕴' }
```

### Using variables in regular expressions
To use a variable in regular exporession, you should construct a new `RegExp` object:
```javascript
const str = 'hello foo world foo';
const bar = 'foo';
str.match(bar); // -> [ 'foo', index: 6, input: 'hello foo world foo', groups: undefined ]
const re = new RegExp(bar, "g");
str.match(re); // -> [ 'foo', 'foo' ]

// template literals can be useful
str.match(new RegExp(` ${bar} `, "g")); // -> [ ' foo ' ]
```