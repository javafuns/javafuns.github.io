---
# default page for /pages/ URL
title: Pages
permalink: /pages/
---

<h1>{{ site.title }}</h1>

<div style="max-width: 100%; padding: 1rem 0; display: grid; grid-template-columns: repeat(2, 1fr); grid-gap: 10px;">
  <div class="page-content-container">
    <div><h1>Linux</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'linux' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="page-content-container">
    <div><h1>Python</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'python' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="page-content-container">
    <div><h1>JavaScript</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'javascript' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="page-content-container">
    <div><h1>Docker</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'docker' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="page-content-container">
    <div><h1>Kubernetes</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'k8s' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="page-content-container">
    <div><h1>Service Mesh</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'service-mesh' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>

  <div class="page-content-container">
    <div><h1>Git</h1></div>
    <div class="content">
      {% for page in site.pages %}
        {% if page.categories contains 'git' %}
          <div>
            <span><a href="{{ page.url }}" title="{{ page.title }}">{{ page.title }}</a></span>
          </div>
        {% endif %}
      {% endfor %}
    </div>
  </div>
</div>
