---
theme: post
published: true
title: Bootstraping a new React application
---
## Setup

```bash
# 1a. use npx ...
npx create-react-app styled-components-app
# ...or use plain yarn
yarn create react-app styled-components-app

# install styled components
yarn add styled-components
yarn add babel-plugin-styled-components --dev
```

Add to `.babelrc`
```json
{
  "presets": ["env", "react"],
  "plugins": ["styled-components"]
}
```

Create debugging profile and attach to Chrome for a test

Source map explorer (for detecting bloat)
```bash
yarn add source-map-explorer --dev
yarn source-map-explorer 'build/static/js/main.*'
```

Test production build
```bash
yarn build
cd build
# start web server: choose one of the methods
python -m http.server 3050
php -S localhost: 3050 # -t /path/to/dir
npx http-server -p 3050
```

Linters and formatters
```bash
yarn add --dev prettier eslint-plugin-prettier
```
Installed also Prettier and ESLint VSCode extensions. `.eslintrc`:
```json
{
  "extends": "react-app",
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

## Code notes

### Service workers
There is a `registerServiceWorker.js` file and `registerServiceWorker()` function call in `index.js`. It is meant to cache files in production builds, providing application with offline capabilities and faster loading for revisiting users. You can opt out of it by uncommenting the function call – hovewer in order to remove existing service workers in production code (if you happened to publish an application with SWs enabled), you have to publish code calling unregister function. Removing SW in your own browser is easy, just use devTools/Application/ServiceWorkers tab.

### Importing css directly into code
As a Webpack extension (or built-in feature, I don't really know), CSS styles can be directly imported into Javascript code (for example in components), coupling one another. Then, you can include `component.css` in a `component.js` and make component standalone.

## Example, step by step
First, we start in `index.js`. We use `ReactDOM.render(component, DOM_node)` method to render `App` component inside a `div` with an id `root`. This `div` resides in a static `public/index.html` file and can be changed there.
[ReactDOM](https://reactjs.org/docs/react-dom.html) is often used merely as a mounting point of React components into the browser DOM (aforementioned `.render()` method). It also provides `.findDOMNode(component)`, allowing for direct DOM manipulations (this can be avoided with refs).
All the necessary imports are here. We also add a CSS file with global styles, `index.css`.

There's really nothing to change in the `index.html`, at least for now. Make sure your root div has id set, so you can relate to it somehow.

### `App.js`

It's time for the main component. I will be using styled-components here instead of existing imported CSS.