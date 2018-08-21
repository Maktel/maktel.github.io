---
theme: post
published: true
title: Things learned – 21 August
---
### Returning `null` from first parameter of `setState()` does not trigger re-render
You can prevent re-renders by returning `null` from the callback in the first parameter of React's `setState()`.

### `immutability-helper` module
Helps working with immutable data structures like Redux's store. You can find available commands on a [Github page](https://github.com/kolodny/immutability-helper#available-commands). Example of the usage is below. Remember that `$` is a valid character in variable name in Javascript.
```javascript
// run in nodejs with immutability-helper module installed
const update = require('immutability-helper');
const data = {foo: 'bar', arr: [1, 2, 3]};
let operation = {
  foo: {$set: 'baz'}, 
  arr: {$push: [4, 5, 6]},
};
const newData = update(data, operation);
// newData -> { foo: 'baz', arr: [ 1, 2, 3, 4, 5, 6 ] }
```