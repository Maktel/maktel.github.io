---
theme: post
published: true
title: Things learned – October 2018
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
