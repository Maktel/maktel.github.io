---
theme: post
published: true
title: Things learned – 3 August
categories:
  - javascript
tags:
  - mercurial
---
### Commit your changes before doing anything
It happened at least once to every person using version control – losing their changes. Commit your files, guys!

### Named shelves in Mercurial
You can name patches saved in shelve:
```bash
hg shelve -n 'named_patch'
```

### Javascript: Arrow functions don't have their own arguments object
This one can be painful: I have been recently using `Function.arguments` object to inspect contents of values passed to function. Unforunately, arrow functions do not have their own `arguments` object; instead, `arguments` is a reference to the enclosing scope's arguments array.

One hack you can use to list all passed values is to use rest parameter:
```javascript
function foo(n) {
  const f = (...args) => {
    console.log(arguments); // prints { "0": 5, <rest of the object>}
    console.log(args); // prints [10]
  }
  f(10);
}
foo(5);