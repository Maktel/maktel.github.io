---
layout: page
title: Home
---
# [](#list-of-posts)List of posts
_(also, [by categories](blog/categories.html), [by tags](blog/tags.html))_


{% for post in site.posts %}
*   <span style="font-family: Hack, monospace; font-weight: bold;">{{ post.date | date_to_string }}</span> » _**[{{ post.title }}]({{ post.url }})**_
{% endfor %}
