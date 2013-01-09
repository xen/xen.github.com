---
layout: page
title: Welcome to vurt
tagline: Personal playground
---
{% include JB/setup %}

{% for post in site.posts limit:15 %}
<h3><a href="{{ BASE_PATH }}{{ post.url }}/">{{ post.title }}</a></h3>
<p class="muted">{{ post.date | date_to_string }}</p>
{% endfor %}

<p>
  <a href="/archive.html" class="btn btn-small">Архив</a>
</p>