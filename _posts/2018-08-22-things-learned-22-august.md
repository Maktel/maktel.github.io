---
theme: post
published: true
title: Things learned – 22 August
---
### `Array.prototype.concat` and new shortcut syntax

```javascript
const arr = ['hello', 'world'];
const foo = 'bar';

arr.concat(foo);
// -> [ 'hello', 'world', 'bar' ]

arr.concat([foo]);
// -> [ 'hello', 'world', 'bar' ]

arr.concat({foo});

// -> [ 'hello', 'world', { foo: 'bar' } ]

arr.concat({baz: foo});
// -> [ 'hello', 'world', { baz: 'bar' } ]