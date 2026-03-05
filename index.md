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
  {% assign cat_slug = category[0] | slugify %}
  <a class="category-pill" data-count="{{ category[1].size }}" href="{{ '/categories/#' | append: cat_slug | relative_url }}">{{ category[0] }} <span>({{ category[1].size }})</span></a>
{% endfor %}
</p>
{% endif %}

{% assign posts = site.posts | sort: 'date' | reverse %}

{% for post in posts %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  <small>{{ post.date | date: "%d.%m.%Y" }}</small>
  {% if post.categories and post.categories.size > 0 %}
  
  {% for cat in post.categories %}{% assign tag_slug = cat | slugify %}<a class="tag" href="{{ '/categories/#' | append: tag_slug | relative_url }}">#{{ cat }}</a>{% unless forloop.last %} {% endunless %}{% endfor %}
  {% endif %}
  
  {{ post.excerpt | strip_html | truncate: 180 }}
{% endfor %}
