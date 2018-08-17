---
theme: post
published: true
title: Things learned – 17 August
---
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
