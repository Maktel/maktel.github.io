---
theme: post
published: true
title: Things learned – 2 August
categories:
  - reactjs
tags:
  - react
---
### React's setState is asynchronous
#### Value
`setState()` can accept an object with values to set or a callback which will receive previous state. Using objects is fine if you use constants/literals for new values; for modifications of previous state callbacks are recommended. 

```javascript
component.setState(prevState => {counter: prevState.counter + 1});
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
