---
theme: post
published: true
title: Things learned – October 2018
---
# October 31

### Append change to a commited changeset in git
[so answer](https://stackoverflow.com/questions/2719579/how-to-add-a-changed-file-to-an-older-not-last-commit-in-git/27721031#27721031)
```
# stage your changes with git add or through VSC
git commit --fixup=COMMITYOUWANTTOFIX
git rebase --interactive --autosquash
# :wq
```


---

# October 29

### Pretty-print Javascript object
When passing an object to console or textarea as a text, you can pass parameters to `JSON.stringify` to format output:
```javascript
const foo = { bar: 'baz' };
JSON.stringify(foo, null, 4);
```

---

# October 19

### Remember to set automatic screen lock after 5 minutes...
...when setting up a new Gnome system. It seems like Super+L doesn't always work.

### Using Typescript support in non-Typescript project (VSCode)
[Issue that started it all](https://github.com/Microsoft/vscode/issues/39520)

---

# October 17

## New things in new template

### `ContainerQuery` component
[react-container-query](https://www.npmjs.com/package/react-container-query) serves the purpose of making the application responsive. It allows passing CSS classes based on widths to component's children, this way making children respond to parent's (container's) size.

### Internationalization with `react-intl`
[https://github.com/yahoo/react-intl](github)

### `react-router`, `react-redux`, and `connected-react-router`
Using `react-router-config` for configuration in a single place instead of spreading it over the whole application.

---

# October 16

### Commit ignored file in git

If you want to include some build files or something that is not related directly to the core code (eg. in server repo you include example web build), you can add files to `.gitignore` like normal and then `git add` them them with `-f` flag that forces ignored files.

---

# October 11

### List `git` remotes
```bash
git remote -v
```

---

# October 10

### Target only Firefox with CSS
Plays nice with `.scss` `@extend`
```css
@supports (-moz-appearance:none) {
    .HelloWorld { color: red; } 
}
```

---

# October 5

### `package.json` dependencies vs devDependencies

I was trying to add `ant-design` support to my React application bootstrapped with `create-react-app`. You can [try to fork](https://auth0.com/blog/how-to-configure-create-react-app/) `react-scripts` and change scripts version during new project creation, but I believe it is a unnecessary burden and maintenance issue. The fastest way is to settle on a specific set of packages and `eject`.

After I ejected from CRA, all the packages ended up as dependencies in `package.json`, not as devDependencies, as one would expect (since your application does not require `eslint` or tools like `babel` to work). [As it turns out](https://github.com/facebook/create-react-app/issues/4969) this behaviour is not a bug. 

Developer dependencies are ment as things related to testing and so on -- they are not installed if you run `production` npm command, causing builds to fail on continuous integration servers and deployment machines.
