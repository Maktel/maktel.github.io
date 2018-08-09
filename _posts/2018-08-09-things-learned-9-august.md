---
theme: post
published: true
title: Things learned – 9 August
---
### Using grid layout
Grid layout is way better than flex or any other way to lay out elements. It seems much easier after I used it to create a very specific layout with clear purpose.
```css
.wrapper {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-rows: auto;
}

.always_middle_column {
  grid-column: 2;
}

.first_row_wide {
  grid-row: 1;
  grid-column-start: 1;
  grid-column-end: 4;
}
```

### React 16 error handling
https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html