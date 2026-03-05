---
layout: default
title: Blog
---

# Neueste Artikel

{% assign categories = site.categories | sort %}
{% if categories.size > 0 %}
## Kategorien im Überblick

<p class="category-overview">
{% for category in categories %}
  <a class="category-pill" data-count="{{ category[1].size }}" href="{{ '/categories/#' | append: category[0] | slugify | relative_url }}">{{ category[0] }} <span>({{ category[1].size }})</span></a>
{% endfor %}
</p>
{% endif %}

{% assign posts = site.posts | sort: 'date' | reverse %}

{% for post in posts %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  <small>{{ post.date | date: "%d.%m.%Y" }}</small>
  {% if post.categories and post.categories.size > 0 %}
  
  {% for cat in post.categories %}<a class="tag" href="{{ '/categories/#' | append: cat | slugify | relative_url }}">#{{ cat }}</a>{% endfor %}
  {% endif %}
  
  {{ post.excerpt | strip_html | truncate: 180 }}
{% endfor %}
