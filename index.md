---
layout: page
title: Welcome to vurt
tagline: Personal playground
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts limit:15 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}/">{{ post.title }}</a></li>
  {% endfor %}
</ul>

<!-- ## Поддержать

{% include JB/support %}
-->
