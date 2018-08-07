---
theme: post
published: true
title: Things learned – 7 August
---
### Empty arrays and empty objects evaluate to true
Javascript's objects evaluate to true when put inside a conditional expression (arrays are objects after all). To test if an object is empty (has no own properties), you can either use lodash or other library, or use following snippet:
```javascript
function isEmptyObject(o) {
  return Object.keys(o).length === 0 && o.constructor === Object;
}
```

### Triggering application wide events
Redux's application level store is for data. Redux enables triggering events through reducers responding to certain actions:
* some code dispatches an action
* the action enters reducer
* reducer(s) change(s) application state
* component had previously subscribed to Redux's store by `connect(mapStateToProps, ...)`
* component reacts to change in their props by `componentDidUpdate(prevProps, prevState)` and similar lifecycle functions (because it can compare current props with previous)

This has a drawback of having to pollute global store with action-specific data. 
_Of course calling any functions in reducer is a big no-no._

I haven't found a good solution for it yet, however I believe callbacks are the way to go.

### What is `snapshot` in React lifecycle's `componentDidUpdate(_, _, snapshot)`?