---
layout: archive
permalink: /design_patterns/
title: "Design Patterns"
---

<div class="tiles">
{% for post in site.posts %}
  {% if post.categories contains "design_patterns" %}
		{% include post-grid.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
