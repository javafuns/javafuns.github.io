---
# default page for /pages/ URL
title: Pages
permalink: /pages/
---
<h1>{{ page.title }}</h1>
<ul class="posts">
  {% for page in site.pages %}
    <li><span></span> » <a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></li>
  {% endfor %}
</ul>
