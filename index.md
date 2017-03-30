---
layout: page
title: Home
---
# [](#list-of-posts)List of posts
__(also, [by categories](blog/categories.html), [by tags](blog/tags.html))__

{% for post in site.posts %}
  **{{ post.date | date_to_string }}** » __[{{ post.title }}]({{ post.url }})__
  * * *
{% endfor %}
