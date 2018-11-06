---
theme: post
published: true
title: Things learned – November 2018
---
# 6 November 

### Editing old commit in git history
[so](https://stackoverflow.com/a/1186549)
```bash
git rebase --interactive <commit-sha>^
# choose with 'edit' commit you want to change
# make changes you would like to amend -- this apparently doesn't push changes to a current
# workspace, so you need to look at the specific commit history
git commit --all --amend --no-edit
git rebase --continue
```
