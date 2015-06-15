---
layout: archive
permalink: /apresentacoes/
title: "Apresentações"
---

<div class="tiles">
{% for post in site.posts %}
  {% if post.categories contains "apresentacoes" %}
		{% include post-grid.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
