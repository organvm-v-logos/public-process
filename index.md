---
layout: default
title: "Public Process"
---

# organvm — Public Process

Essays about building an eight-organ creative-institutional system. Theory, art, commerce, orchestration, community, marketing, and the meta-system that connects them all.

---

## Essays

<div class="category-chips">
  <a href="{{ site.baseurl }}/categories/#meta-system" class="category-badge category-meta-system">meta-system</a>
  <a href="{{ site.baseurl }}/categories/#case-study" class="category-badge category-case-study">case-study</a>
  <a href="{{ site.baseurl }}/categories/#guide" class="category-badge category-guide">guide</a>
  <a href="{{ site.baseurl }}/categories/#methodology" class="category-badge category-methodology">methodology</a>
  <a href="{{ site.baseurl }}/categories/#retrospective" class="category-badge category-retrospective">retrospective</a>
</div>

{% for post in site.posts %}
### [{{ post.title }}]({{ post.url | relative_url }})

<div class="meta">
  <span>{{ post.date | date: "%B %d, %Y" }}</span>
  {% if post.category %}<a href="{{ site.baseurl }}/categories/#{{ post.category }}" class="category-badge category-{{ post.category }}">{{ post.category }}</a>{% endif %}
  {% if post.reading_time %}<span>{{ post.reading_time }}</span>{% endif %}
</div>

{% if post.excerpt %}{{ post.excerpt }}{% endif %}

{% if post.tags.size > 0 %}
<div class="meta">
  {% for tag in post.tags %}<a href="{{ site.baseurl }}/tags/#{{ tag }}" class="tag">{{ tag }}</a> {% endfor %}
</div>
{% endif %}

---

{% endfor %}
