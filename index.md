---
layout: page
title: Home
---
# [](#list-of-posts)List of posts
## []()(also, [by categories](blog/categories.html), [by tags](blog/tags.html))

{% for post in site.posts %}
  **{{ post.date | date_to_string }}** » __[{{ post.title }}]({{ post.url }})__
  * * *
{% endfor %}
