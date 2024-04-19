---
layout: archive
permalink: /docs/
title: "Technical Documentation"
---

{% assign main_doc = site.docs | where: "type", "doc" | sort: "order" %}

<nav class="js-toc toc">
<h4 class="toc__title"><span>Technical Documentation</span></h4>
  <ul class="toc__menu">
{% for doc in main_doc %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
{% endfor %}
  </ul>
</nav>

{% for doc in main_doc %}
<h2><a href="{{ doc.url }}">{{ doc.title }}</a></h2>
{{ doc.content }}
<hr />
{% endfor %}
