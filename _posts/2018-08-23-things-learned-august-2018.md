---
theme: post
published: true
title: Things learned – August 2018
---
# 29 August

### How to create local server with proxy using Apache2

* First, make sure you have `apache` installed. Apache's configuration sits in `/etc/httpd/conf/`.
* Enable virtual hosts in `/etc/httpd/conf/httpd.conf` – uncomment line below
```
# Virtual hosts
Include conf/extra/httpd-vhosts.conf
```
* In the same file enable proxy modules (mod_proxy, mod_proxy_http)
```
# Uncomment
LoadModule proxy_module modules/mod_proxy.so
```
```
# Uncomment
LoadModule proxy_http_module modules/mod_proxy_http.so
```
* Open `/etc/httpd/conf/extra/httpd-vhosts.conf` and set configuration like this
```
Listen 4100
<VirtualHost *:4100>
	DocumentRoot "/srv/http/localhost"

	<Proxy *>
        Order allow,deny
        Allow from All
    </Proxy>

    # Requests go from localhost/app-base/api/ to external-ip/app-base/api/
    <Location "/app-base/api/">
        ProxyPass "http://ip-to-external-api/app-base/api/"
        ProxyPassReverse "http://ip-to-external-api/app-base/api/"
        ProxyPassReverseCookiePath "*" "/app-base/"
        ProxyPassReverseCookieDomain "*" "/app-base/"
   		ProxyPreserveHost Off 
    </Location>
    
    ErrorLog "/var/log/httpd/localhost-app-error_log"
    CustomLog "/var/log/httpd/localhost-app-access_log" common
</VirtualHost>
```
* Create directory `/srv/http/localhost` and place your application there (best in root directory, but can be in any other as well)
* Run `sudo systemctl restart httpd.service`
* Open `localhost:4100` in browser

### Copy directory contents
Use `cp`, but append a dot at the end of the source path
```bash
cp -r src dest
# -> dest/src/<content>
cp -r src/. dest
# -> dest/<content>
```

# 28 August

### `String.prototype.slice` return value
String method `slice(startIndex[, endIndex])` returns an empty string on invalid parameters like start index greater or equal the length of the string oraz start index greater than end index (except for negative end index that matches position from the end of the string). Providing end index after the end of the string does greedy match for the whole string.

# 24 August

### Disable cancer `jsx-a11y` eslint plugin from airbnb config
*node_modules/eslint-config-airbnb/index.js*
```javascript
module.exports = {
  extends: [
    'eslint-config-airbnb-base',
    'eslint-config-airbnb-base/rules/strict',
    './rules/react',
    // './rules/react-a11y', // comment this
  ].map(require.resolve),
  rules: {},
};
```
I need to think of a way to disable it premanently xD

# 23 August

### Destructuring assignment with rename
The proper way to rename a variable during destructuring assignment is counterintuitive to me:
```javascript
const o = { hello: 'world', foo: 123 };
const { hello, foo: bar } = o;
// hello -> 'world'
// bar -> 123
// foo -> undefined
```

### Node 6 LTS supports almost all features of ES6/ES2015
[Feature compatibility table](https://node.green/)

### Destructuring assignment
TIL it's "destructuring" assignment, not "destructing"

---

# 22 August

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
```

---

# 21 August

### Returning `null` from first parameter of `setState()` does not trigger re-render
You can prevent re-renders by returning `null` from the callback in the first parameter of React's `setState()`.

### `immutability-helper` module
Helps working with immutable data structures like Redux's store. You can find available commands on a [Github page](https://github.com/kolodny/immutability-helper#available-commands). Example of the usage is below. Remember that `$` is a valid character in variable name in Javascript.
```javascript
// run in nodejs with immutability-helper module installed
const update = require('immutability-helper');
const data = {foo: 'bar', arr: [1, 2, 3]};
let operation = {
  foo: {$set: 'baz'}, 
  arr: {$push: [4, 5, 6]},
};
const newData = update(data, operation);
// newData -> { foo: 'baz', arr: [ 1, 2, 3, 4, 5, 6 ] }
```

---

# 17 August

### Debugging React in Chromium
* Bootstrap a project using `create-react-app` or any other way
* Install VSCode _Chrome debugging extension_
* Add a configuration in `.vscode/launch.json`
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Chrome",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceRoot}/src",
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*"
      },
      "runtimeExecutable": "/usr/bin/chromium"
    }
  ]
}
```
* Close all Chromium windows
* Launch Chromium with `--remote-debugging-port=9222` flag
* Press F5 to start debugging

### npx for yarn
The only affiliation of `npx` with `npm` is cache-sharing and apart from that you can use `npx` with any other package manager.
If you insist on not using `npx`, you can run a executable using yarn with `yarn run bin`. It works if `bin` is in `./node_modules/`.

### List of npm commands translated into yarn
[https://yarnpkg.com/en/docs/migrating-from-npm#toc-cli-commands-comparison](https://yarnpkg.com/en/docs/migrating-from-npm#toc-cli-commands-comparison)

---

# 16 August

### Tagged template literals
You can prepend a template literal in Javascript with a tag. This way the function with tag's name will be called with literal as an argument.
```javascript
function foo() { console.log('foo:', arguments); }
const bar = "Maktel 🕴";
foo `Hello world, ${bar}!`;
// -> foo: [Arguments] { '0': [ 'Hello world, ', '!' ], '1': 'Maktel 🕴' }
```

### Using variables in regular expressions
To use a variable in regular exporession, you should construct a new `RegExp` object:
```javascript
const str = 'hello foo world foo';
const bar = 'foo';
str.match(bar); // -> [ 'foo', index: 6, input: 'hello foo world foo', groups: undefined ]
const re = new RegExp(bar, "g");
str.match(re); // -> [ 'foo', 'foo' ]

// template literals can be useful
str.match(new RegExp(` ${bar} `, "g")); // -> [ ' foo ' ]
```

---

# 14 August

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

---

# 13 August

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
However, expressions that evaluate to false like `0` are still rendered.
Q: Do `''` fall into the same category?
[source](https://reactjs.org/docs/jsx-in-depth.html#booleans-null-and-undefined-are-ignored)

---

# 10 August

### CSS Grid layout column problem
I had a container with 3 divs (one of them, `c`, is empty) and wanted to place them in a following pattern:
```
a b
  c
```
It worked just like intended. However, when I tried to change number of columns to `grid-template-columns: 1fr` to force one-column layout, the layout kept 2 columns. Almighty `!important` didn't help either.
Expected result
```
a
b
c
```
As it turned out, `c` had `grid-column: 2`, which caused layout to add a second column despite `grid-template-columns`.

---

# 9 August

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

### Better way to handle injectable class names
Haven't tested it yet in the field, but it seems like a good idea: build a static version of a component with hardcoded class names, then move the default implementations to `defaultProps` as values for properties. That way a component has a default look, but can be easily styled.

---

# 8 August

### You cannot extend selectors from outside media query in Sass
When you are writing a rule in a media query and you want to reference a class that is outside the query, you have to you mixins or other tricks, as `@extend` is not allowed (at least in libsass 3.5.4).

### Found something that looks like a solution to component class name problem
https://simonsmith.io/handling-props-and-class-names-in-react/

### Add whitespace in span in JSX
I wanted to render a `span` with a colon and space as children: `<span>; </span>`. However, it seems like JSX removes the space. I didn't want a non-breaking space (`&nbsp;`), as it would make word-wrapping look bad. I have tried escaping with `": "` or `` `; ` ``, but it didn't work.
I believe I have settled for `&nbsp;`, but right now using `React.createElement('span', null, '; ')` seems like a interesting solution which could solve my problem.

When I tried it again the day later, both plain `<span>` and `.createElement()` versions worked. Strange.

### Escape backtick in Markdown
I couldn't make a following snippet in this very post:
```
`; `
```
without a trailing space in inline code block.

---

# 7 August

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

---

# 6 August

### Node – check if path starts with one of the values from an array

#### Clever version
```javascript
const apiPaths = ["/foo/bar/", "/biz/baz"];
const isPathToApi = apiPaths.reduce(
  (res, apiPath) => res || parsedUrl.pathname.startsWith(apiPath),
  false, // initial value for res
);

if (isPathToApi) {
  ...
}
```

### Mercurial tips 'n' tricks
https://www.mercurial-scm.org/wiki/TipsAndTricks#Avoid_merging_autogenerated_.28binary.29_files_.28PDF.29

### How to trigger file download in browser?
https://stackoverflow.com/questions/20508788/do-i-need-content-type-application-octet-stream-for-file-download/20509354#20509354

---

# 3 August

### Commit your changes before doing anything
It happened at least once to every person using version control – losing their changes. Commit your files, guys!

### Named shelves in Mercurial
You can name patches saved in shelve:
```bash
hg shelve -n 'named_patch'
```

### Javascript: Arrow functions don't have their own arguments object
This one can be painful: I have been recently using `Function.arguments` object to inspect contents of values passed to function. Unforunately, arrow functions do not have their own `arguments` object; instead, `arguments` is a reference to the enclosing scope's arguments array.

One hack you can use to list all passed values is to use rest parameter:
```javascript
function foo(n) {
  const f = (...args) => {
    console.log(arguments); // prints { "0": 5, <rest of the object>}
    console.log(args); // prints [10]
  }
  f(10);
}
foo(5);
```

### Re-render on `setState()`
By default, every time you call `setState()`, `render()` is called. However rendering is split in two steps: virtual DOM re-render and browser DOM. If nothing changes in virtual DOM (rendering returns the same vDOM tree), browser DOM is not updated – this is where the speed of React comes from.

You can prevent re-rendering using custom implementation of `shouldComponentUpdate(nextProps, nextState)` which is called before each state change application. The function should return boolean value, depending on the need to re-render.

### Function vs arrow function
https://stackoverflow.com/questions/34361379/arrow-function-vs-function-declaration-expressions-are-they-equivalent-exch
For now: if I understand correctly, arrow functions are like C++ lambda's with [&] and no own scope.

### `WeakMap` and `WeakSet` built-in objects
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap

---

# 2 August

### React's setState is asynchronous
#### Value
`setState()` can accept an object with values to set or a callback which will receive previous state. Using objects is fine if you use constants/literals for new values; for modifications of previous state callbacks are recommended. 

```javascript
component.setState((prevState, currentProps) => {counter: prevState.counter + 1});
```

Rationale: Since `setState()` is asynchronous, there can be multiple queued operations. If you call setState with plain object, other calls can overwrite your modification (especially during operations like incrementation)

```javascript
// DON'T

constructor() {
  this.state = {
    counter: 0,
  };
}

component.setState({counter: this.state.counter + 1});
component.setState({counter: this.state.counter + 2});
// -> possible this.state.counter values after setState is applied: 2, 3
// second call can overwrite the first one
```

#### Application
Since state can be updated anytime, you cannot use it synchronously. To do an operation on modified state (eg. on updated counter), pass callback as a second argument to `setState()`. This callback will be called after state update.

```javascript
component.setState(
  {counter: 0},
  () => { console.log('second', this.state.counter); // zero }
);
console.log('first', this.state.counter); // old value
```

### mapDispatchToProps()
https://stackoverflow.com/questions/39419237/what-is-mapdispatchtoprops

### Immutability
https://github.com/kolodny/immutability-helper
https://github.com/facebook/immutable-js –Facebook's library providing immutable types
