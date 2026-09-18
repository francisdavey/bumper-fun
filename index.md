---
layout: default
title: Home
---

Notes and worked examples, mostly mathematical.

<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    &mdash; <small>{{ post.date | date: "%-d %B %Y" }}</small>
  </li>
{% endfor %}
</ul>
