---
theme: post
published: true
title: Using dnsmasq with NetworkManager
---
## Setup 

To avoid configuring dnsmasq directly, you can use `NetworkManager`'s plugin to enable support for its own instance (?) of dnsmasq.

```
# edit /etc/NetworkManager/NetworkManager.conf
[main]
dns=dnsmasq
```
```bash
sudo systemctl restart NetworkManager.service
```

Then you can edit configuration. Example here adds wildcard redirect for `*.hello.test` (so `foo.hello.test` will resolve to localhost).

```
# /etc/NetworkManager/dnsmasq.d/redirect.conf
address=/.hello.test/127.0.0.1
```