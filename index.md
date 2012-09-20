---
layout: page
title: Welcome to vurt
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts limit:7 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

<!-- ## Поддержать

{% include JB/support %}
-->