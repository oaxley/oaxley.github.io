---
layout: indexcategory
include_collection: doc
permalink: /docs/
title: "Technical Documentation"
show_breadcrumb: true
---

{% assign main_doc = site.docs | where: "type", "doc" | sort: "order" %}

<nav class="js-toc toc">
<h4 class="toc__title"><span>Today I learned...</span></h4>
  <ul class="toc__menu">
{% assign old_subtype = "" %}
{% for doc in main_doc %}

{% if doc.subtype != old_subtype %}
  <h4><span>{{ doc.subtype }}</span></h4>
  {% assign old_subtype = doc.subtype %}
{% endif %}

<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
{% endfor %}
  </ul>
</nav>

<!--
{% for doc in main_doc %}
<h2><a href="{{ doc.url }}">{{ doc.title }}</a></h2>
{{ doc.content }}
<hr />
{% endfor %}
-->
