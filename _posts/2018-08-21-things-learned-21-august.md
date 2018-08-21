---
theme: post
published: true
title: Things learned – 21 August
---
### Returning `null` from first parameter of `setState()` does not trigger re-render
You can prevent re-renders by returning `null` from the callback in the first parameter of React's `setState()`.