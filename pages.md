---
# default page for /pages/ URL
title: Pages
permalink: /pages/
---
<h1>{{ site.title }}</h1>

{% for cate in site.page-categories %}
<ul class="posts">
  <li>
    {{ cate }}:
    {% for page in site.pages %}
      {% if page.categories contains cate %}
        <ul><span></span> » <a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></ul>
      {% endif %}
    {% endfor %}
  </li>
</ul>
{% endfor %}

