---
theme: post
published: true
title: Things learned – 14 August
---
### Sass variables in `calc()`
In order to use Sass variables in `calc()`, you have to interpolate the name:
```css
$elem-width: 140px;

body {
  width: calc(100% - 2em - #{$elem-width});
}
```

### `.replace()` by default changes only the first occurence
To remove all occurences of a character(s), use regular expression with global modifier.
```javascript
'foo bar foo bar'.replace('foo', 'baz') // -> 'baz bar foo bar'
'foo bar foo bar'.replace(/foo/g, 'baz') // -> 'baz bar baz bar'
