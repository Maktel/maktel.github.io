---
theme: post
published: true
title: Things learned – 2 August
---
### React's setState is asynchronous
setState() can accept an object with values to set or a callback which will receive previous state. Using objects is fine if you use constants/literals for new values; for modifications of previous state callbacks are recommended. 

```javascript
component.setState(prevState => {counter: prevState.counter + 1});
```

Rationale: Since setState is asynchronous, there can be multiple queued operations. If you call setState with plain object, other calls can overwrite your modification (especially during operations like incrementation)

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