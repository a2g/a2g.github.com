---
layout: page
title:  Software Engineering
tagline: it's what I do
---
{% include JB/setup %}

All posts so far:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

