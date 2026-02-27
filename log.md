---
layout: default
title: "Captain's Log"
permalink: /log/
---

# Captain's Log

Daily notes from the construction of an eight-organ creative-institutional system.

<div class="log-index">
{% assign sorted_logs = site.logs | sort: "date" | reverse %}
{% for log in sorted_logs %}
  <article class="log-summary">
    <h2><a href="{{ log.url | relative_url }}">{{ log.title }}</a></h2>
    <div class="meta">
      <span>{{ log.date | date: "%B %d, %Y" }}</span>
      {% if log.mood %}<span class="mood mood-{{ log.mood }}">{{ log.mood }}</span>{% endif %}
    </div>
    {% if log.tags.size > 0 %}
    <div class="meta">
      {% for tag in log.tags %}<span class="tag">{{ tag }}</span>{% endfor %}
    </div>
    {% endif %}
  </article>
{% endfor %}
</div>
