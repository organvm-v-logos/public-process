---
layout: default
title: "Public Process"
---

# organvm — Public Process

Essays about building an eight-organ creative-institutional system. Theory, art, commerce, orchestration, community, marketing, and the meta-system that connects them all.

---

## Essays

{% for post in site.posts %}
### [{{ post.title }}]({{ post.url | relative_url }})

<div class="meta">
  <span>{{ post.date | date: "%B %d, %Y" }}</span>
  {% if post.reading_time %}<span>{{ post.reading_time }}</span>{% endif %}
  {% if post.word_count %}<span>{{ post.word_count }} words</span>{% endif %}
</div>

{% if post.excerpt %}{{ post.excerpt }}{% endif %}

{% if post.tags.size > 0 %}
<div class="meta">
  {% for tag in post.tags %}<a href="{{ site.baseurl }}/tags/#{{ tag }}" class="tag">{{ tag }}</a> {% endfor %}
</div>
{% endif %}

---

{% endfor %}
