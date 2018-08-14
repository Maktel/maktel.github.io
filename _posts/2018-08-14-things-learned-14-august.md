---
theme: post
published: true
title: Things learned – 14 August
---
### Sass variables in `calc()`
In order to use Sass variables in `calc()`, you have to interpolate the name:
```sass
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
```

### How to create subcomponent?
Why so complicated? https://medium.com/maxime-heckel/react-sub-components-513f6679abed
Answer: https://risan.io/react-component-with-dot-notation.html

```jsx
export class A extends Component {
  static B = props => (
    <div>B</div>
  );

  static C = props => (
    <div>C</div>
  );

  render() {
    return (
      <div>A</div>
      <div>
        {this.props.children}
      </div>
    );
  }
}
```

Then use as:
```jsx
<A>
  <A.B />
  <A.C />
</A>
```