---
layout: default
title: Front page
---
#	 [](#header-1){{ page.title }}:
	<ul class="posts">
	  {% for post in site.posts %}
	    <li><span>{{ post.date | date_to_string }}</span> » <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
	  	<hr>
		{% endfor %}
	</ul>
