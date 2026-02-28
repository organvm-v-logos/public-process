---
layout: default
title: "Reading"
permalink: /reading/
---

# Reading Observatory

Articles surfaced by the automated reading observatory — a weekly curation of links relevant to creative systems, institutional design, and the concerns of the eight-organ model.

---

{% assign articles = site.data.surfaced %}

{% if articles.size > 0 %}

{% assign grouped = articles | group_by: "collection" %}
{% for group in grouped %}

## {{ group.name }}

{% for item in group.items %}
<div class="surfaced-item">
  <h3><a href="{{ item.url }}">{{ item.title }}</a></h3>
  <div class="meta">
    {% if item.source %}<span>{{ item.source }}</span>{% endif %}
    {% if item.date %}<span>{{ item.date }}</span>{% endif %}
  </div>
  {% if item.excerpt %}<p>{{ item.excerpt }}</p>{% endif %}
</div>

---

{% endfor %}
{% endfor %}

{% else %}

<div class="empty-state">
  <p>No articles surfaced yet.</p>
  <p>The <a href="https://github.com/organvm-v-logos/reading-observatory">reading observatory</a> runs weekly, scanning RSS feeds for articles relevant to creative infrastructure, institutional design, and systems thinking.</p>
  <p>Once the pipeline is active, curated links will appear here automatically.</p>
</div>

{% endif %}
