---
theme: post
published: true
title: Things learned – 13 August
---
### Bindings and arrow functions in render
You should avoid arrow functions in render
https://medium.freecodecamp.org/react-pattern-extract-child-components-to-avoid-binding-e3ad8310725e

### Q: Which conditional rendering gives the best performance in JSX?
https://www.robinwieruch.de/conditional-rendering-react/

### Booleans, `null` and `undefined` are valid and ignored JSX children
All these values simple don't render. Expressions below all render to the same thing:
```jsx

<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>

```