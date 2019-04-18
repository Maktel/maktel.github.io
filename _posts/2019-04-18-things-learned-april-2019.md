---
theme: post
published: true
title: Things learned – April 2019
---
# 18 April

### Update gnome-shell extension

[SO](https://askubuntu.com/a/1122747)

```bash
wget https://raw.githubusercontent.com/NicolasBernaerts/ubuntu-scripts/master/ubuntugnome/gnomeshell-extension-manage
chmod +x
# find extension's id in the url, eg. 19 here: https://extensions.gnome.org/extension/19/user-themes/

# use as
./gnomeshell-extension-manage --version latest --extension-id 19 --install
```