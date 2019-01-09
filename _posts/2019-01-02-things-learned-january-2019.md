---
theme: post
published: true
title: Things learned – January 2019
---
# January 9

### Code-splitting using `React.lazy()`

```jsx
// import
// can make lambda async and use await for things like sleep()
const ComponentName = React.lazy(() => import('./FileNameWithDefaultExport'));


// usage in render
<React.Suspense fallback={<Spin />}>
  <ComponentName />
</React.Suspense>
```

---

# January 3

### `position: fixed` won't work when CSS filters are applied in parent

This behaviour is strange, but accords to specification.

[SO](https://stackoverflow.com/questions/52937708/css-filter-on-parent-breaks-child-positioning)

---

# January 2

### Capturing page snapshot with delay

Put this into console and put your page into state you want to snapshot, before breakpoint triggers.
```javascript
const delay = 4000;
setTimeout(() => { debugger; }, delay);
```
