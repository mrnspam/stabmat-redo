---
title: Composers
permalink: /composers/
---

<ul class="composer-list">
  {% assign sorted = site.composers | sort: 'title' %}
  {% for composer in sorted %}
    <li>
      <a href="{{ composer.url | relative_url }}">{{ composer.title }}</a>
    </li>
  {% endfor %}
</ul>
