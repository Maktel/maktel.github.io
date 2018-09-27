---
theme: post
published: true
title: Link instead of installing node modules which fail to build
tags:
  - yarn
  - node
---
There are [problems](https://github.com/imagemin/pngquant-bin/issues/78) with `pngquant-bin` node module -- it doesn't build on certain environments. Solutions for Ubuntu or Debian exist and apparently work, but installing `-dev` package is not possible on Arch/Manjaro -- headers are already included in base packages.
`pngquant-bin` cannot be built, what means you cannot use apps that rely on `imagemin-pngquant`, since your app will not successfully download and build all other dependencies. But if you are able to build the package on another machine, there is a way to link it instead of copying entire `node_modules` directory.

* on another machine create new directory and inside it run `yarn add imagemin-pngquant`
* after build is successful, copy `node_modules` directory to project directory on the target machine
* rename copied directory so it doesn't conflict; I chose `node_mod`
* change your `package.json` as [according to yarn documentation](https://yarnpkg.com/lang/en/docs/selective-version-resolutions/)
```json
{
  "name": "your-project-suffering-because-of-pngquant",
  "dependencies": {}
  "resolutions": {
    "imagemin-pngquant": "file:node_mod/imagemin-pngquant"
  }
}
```
* run `yarn install` (npm does not support this command)

Now your project should build as it resolves the package from existing and working directory instead of compiling it from scratch.
