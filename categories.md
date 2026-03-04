---
layout: default
title: Kategorien
permalink: /categories/
---

# Kategorien

<div id="top"></div>

{% assign cats = site.categories | sort %}
{% if cats.size > 0 %}
## Schnellnavigation

<p class="category-overview">
{% for category in cats %}
	<a class="category-pill" data-count="{{ category[1].size }}" href="#{{ category[0] | slugify }}">{{ category[0] }} <span>({{ category[1].size }})</span></a>
{% endfor %}
</p>
{% endif %}

<div class="categories-list">
{% for category in cats %}
<section class="category-section" data-count="{{ category[1].size }}" data-name="{{ category[0] | downcase }}">
## <span id="{{ category[0] | slugify }}">{{ category[0] }}</span> <small>({{ category[1].size }})</small>
{% assign catPosts = category[1] | sort: 'date' | reverse %}
{% for post in catPosts %}
- {{ post.date | date: "%d.%m.%Y" }} — <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
{% endfor %}

<p class="back-to-top"><a href="#top">↑ Zurück nach oben</a></p>

</section>

{% endfor %}
</div>
