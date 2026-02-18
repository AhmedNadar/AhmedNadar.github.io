---
layout: page
title: Writing
permalink: /posts/
description: "Posts about Rails, building products, and shipping code."
---

<ul class="posts-list">
  {% for post in site.posts %}
  <li>
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %Y" }}</time>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>

{% if site.posts.size == 0 %}
Nothing here yet. First post coming soon.
{% endif %}
