---
theme: post
published: true
title: Things learned – August 2018
---
## 3 August

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

---

## 2 August

### React's setState is asynchronous
#### Value
`setState()` can accept an object with values to set or a callback which will receive previous state. Using objects is fine if you use constants/literals for new values; for modifications of previous state callbacks are recommended. 

```javascript
component.setState((prevState, currentProps) => {counter: prevState.counter + 1});
```

Rationale: Since `setState()` is asynchronous, there can be multiple queued operations. If you call setState with plain object, other calls can overwrite your modification (especially during operations like incrementation)

```javascript
// DON'T

constructor() {
  this.state = {
    counter: 0,
  };
}

component.setState({counter: this.state.counter + 1});
component.setState({counter: this.state.counter + 2});
// -> possible this.state.counter values after setState is applied: 2, 3
// second call can overwrite the first one
```

#### Application
Since state can be updated anytime, you cannot use it synchronously. To do an operation on modified state (eg. on updated counter), pass callback as a second argument to `setState()`. This callback will be called after state update.

```javascript
component.setState(
  {counter: 0},
  () => { console.log('second', this.state.counter); // zero }
);
console.log('first', this.state.counter); // old value
```

### mapDispatchToProps()
https://stackoverflow.com/questions/39419237/what-is-mapdispatchtoprops

### Immutability
https://github.com/kolodny/immutability-helper
https://github.com/facebook/immutable-js –Facebook's library providing immutable types
