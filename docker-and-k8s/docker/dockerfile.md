---
title: this is titile
layout: default
---

FROM
<div>
{% for post in site.posts %}
	<h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
	  {{ post.excerpt }}
        </h2>
{% endfor %}
</div>
