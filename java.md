---
layout: archive
permalink: /java/
title: "Java"
---

<div class="tiles">
{% for post in site.posts %}
  {% if post.categories contains "java" %}
		{% include post-grid.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
