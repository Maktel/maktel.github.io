---
theme: post
published: true
title: Set up Next.js application on mydevil.net hosting
---
## General

Quite possibly this will work on any other hosting provider that allows running Nodejs applications by executing a single file.

## Setup

1. Prepare your application (use `npx create-next-app dir --use-npm --ts` or any other starting method). I will use `npm`.
2. Upload all your project files to the hosting. Put them in `domain/public_nodejs`. Exclude `node_modules` and `.next`, so that it uploads faster (you can also make use of git repo). Enter the directory.
3. Create `app.js` file with the following content:

```
const nextStart = require("next/dist/cli/next-start");

nextStart.nextStart([]);
```

4. Run `npm ci` to install all dependencies.
5. Run `next run build` to prepare production release of your application.
6. (optional) Restart the application: `devil www restart domain-name`.

## Integration with git

TODO: https://wiki.mydevil.net/Git
- dependency install must be configured
- next run build must be configured
