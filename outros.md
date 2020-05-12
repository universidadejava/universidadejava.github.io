---
layout: archive
permalink: /outros/
title: "Java EE"
---

<div class="tiles">
{% for post in site.posts %}
  {% if post.categories contains "outros" %}
		{% include post-grid.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
