---
layout: default
title: Home
---

[About me](/about/)

## Posts

<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <small>{{ post.date | date: "%B %-d, %Y" }}</small>
    <p><small>{{ post.excerpt }}</small></p>
  </li>
{% endfor %}
</ul>
