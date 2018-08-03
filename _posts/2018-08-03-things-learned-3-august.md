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
```

### Re-render on `setState()`
By default, every time you call `setState()`, `render()` is called. However rendering is split in two steps: virtual DOM re-render and browser DOM. If nothing changes in virtual DOM (rendering returns the same vDOM tree), browser DOM is not updated – this is where the speed of React comes from.

You can prevent re-rendering using custom implementation of `shouldComponentUpdate(nextProps, nextState)` which is called before each state change application. The function should return boolean value, depending on the need to re-render.

### Function vs arrow function
https://stackoverflow.com/questions/34361379/arrow-function-vs-function-declaration-expressions-are-they-equivalent-exch
For now: if I understand correctly, arrow functions are like C++ lambda's with [&] and no own scope.

### `WeakMap` and `WeakSet` built-in objects
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap