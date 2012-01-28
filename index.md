---
layout: page
title: Welcome to vurt
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## Поддержать

<a href="http://donutor.com/donate/add/20/"><img src="http://static.donutor.com/i/button2.png" width="145" height="46" alt="Поддерживать ежемесячно с помощью Donutor" /></a>
