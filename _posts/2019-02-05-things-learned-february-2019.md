---
theme: post
published: true
title: Things learned – February 2019
---
# 5 February

### Component re-renders instead of remounting on path change in `react-router`

(https://stackoverflow.com/a/49441836)[SO answer]

```jsx
// entry in react-router-config
{
  path: '/foo/:bar/baz',
  exact: true,
  render: props => (<Component key={props.match.params.bar} {...props} />)
}
```