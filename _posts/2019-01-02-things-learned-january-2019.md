---
theme: post
published: true
title: Things learned – January 2019
---
# January 2

### Capturing page snapshot with delay

Put this into console and put your page into state you want to snapshot, before breakpoint triggers.
```javascript
const delay = 4000;
setTimeout(() => { debugger; }, delay);
```