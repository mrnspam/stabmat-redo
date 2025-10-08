---
layout: default
title: Blog
permalink: /stabat-mater-blog/
---

<p>News, discoveries, and updates from the Ultimate Stabat Mater Website.</p>

<div class="post-list">
  {% for post in site.posts %}
    {% include post-teaser.html post=post show_excerpt=true %}
  {% endfor %}
</div>
