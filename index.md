---
layout: default
title: Home
---
# [](#header-1)List of posts

{% for post in site.posts %}
  **{{ post.date | date_to_string }}** » __[{{ post.title }}]({{ post.url }})__
  * * *
{% endfor %}
