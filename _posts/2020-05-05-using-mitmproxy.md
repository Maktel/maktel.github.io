---
theme: post
published: true
title: Using mitmproxy
---
## Commands

```bash
# start listening
mitmdump -w `date -Ins`.dump --mode reverse:http://10.92.2.145:8776 -p 8010

# open web viewer
mitmweb -nC out-file.dump --web-host 0.0.0.0 --web-port 4040
 ```