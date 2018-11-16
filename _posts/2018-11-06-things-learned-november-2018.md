---
theme: post
published: true
title: Things learned – November 2018
---
# 16 November

### Gnome-shell extension development notes
```bash
journalctl -f # for logs
```
```javascript
const ByteArray = imports.byteArray;

ByteArray.toString(/* output from shell command*/);
```

[gjs](https://github.com/GNOME/gjs)

---

# 15 November 

### Process files in batch with `ffmpeg`
Following snippet converts all `.avi` files to `mp4`. `${f%.*}` files name without extension.
```bash
for f in $(ls *.avi); do ffmpeg -i $f ${f%.*}.mp4; done
```
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

### Stage lines with keyboard shortcut in git-gui
[so](https://stackoverflow.com/questions/32661397/is-there-a-keyboard-shortcut-for-stage-lines-in-git-gui)
```bash
sudo gedit /usr/lib/git-core/git-gui
```

At the end of the file add
```
bind .   <$M1B-Key-d> stagelines

proc stagelines {} {
    apply_range_or_line %X %Y
    do_rescan
}
```

Now, restart git gui. Select lines you want to stage and press Ctrl+d. You can change keybind to sole d with `<Key-d>`.

### Test if push candidate builds and works
```bash
# stash all files, including untracked
git stash -u
# check if everything is all right
git stash pop
# push and so on...
```
