---
layout: archive
permalink: /videos/
title: "Vídeos"
---

<div class="tiles">
{% for post in site.posts %}
  {% if post.categories contains "videos" %}
		{% include post-grid.html %}
	{% endif %}
{% endfor %}
</div><!-- /.tiles -->
