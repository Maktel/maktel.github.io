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

## Code notes

### Service workers
There is a `registerServiceWorker.js` file and `registerServiceWorker()` function call in `index.js`. It is meant to cache files in production builds, providing application with offline capabilities and faster loading for revisiting users. You can opt out of it by uncommenting the function call – hovewer in order to remove existing service workers in production code (if you happened to publish an application with SWs enabled), you have to publish code calling unregister function. Removing SW in your own browser is easy, just use devTools/Application/ServiceWorkers tab.

### Importing css directly into code
As a Webpack extension (or built-in feature, I don't really know), CSS styles can be directly imported into Javascript code (for example in components), coupling one another. Then, you can include `component.css` in a `component.js` and make component standalone.

## Example, step by step
First, we start in `index.js`. 