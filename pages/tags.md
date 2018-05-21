---
title: Tags
permalink: "/tags/"

order : 2
---

<div id='tag_cloud' class="tags-container">
{% for tag in site.tags %}
<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a>
{% endfor %}
</div>

<ul class="article-list">
{% for tag in site.tags %}
  <li class="list-year" id="{{ tag[0] }}">{{ tag[0] }}</li>
{% for post in tag[1] %}
  <li class="list-item">
  <time datetime="{{ post.date | date:"%Y-%m-%d" }}" class="gray">{{ post.date | date:"%Y-%m-%d" }}</time>
  <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
{% endfor %}
</ul>

<!-- to activate tag_cloud on, check /_includes/js_plugin.html -->
