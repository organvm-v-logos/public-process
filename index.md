---
layout: default
title: "Public Process"
---

# organvm — Public Process

Essays about building an eight-organ creative-institutional system. Theory, art, commerce, orchestration, community, marketing, and the meta-system that connects them all.

---

## Essays

{% assign essays = site.pages | where_exp: "page", "page.path contains 'essays/'" | sort: "date" | reverse %}
{% for essay in essays %}
{% if essay.title %}
### [{{ essay.title }}]({{ essay.url | relative_url }})

<div class="meta">
  <span>{{ essay.date | date: "%B %d, %Y" }}</span>
  <span>{{ essay.reading_time }}</span>
  <span>{{ essay.word_count }} words</span>
</div>

{{ essay.excerpt }}

---

{% endif %}
{% endfor %}

## About

This is **ORGAN-V** of the [organvm system](https://github.com/meta-organvm) — the public-facing narrative layer that documents how eight specialized organs coordinate across 79 GitHub repositories to sustain autonomous creative practice.

[RSS Feed]({{ site.baseurl }}/feed.xml) · [GitHub](https://github.com/organvm-v-logos/public-process) · [meta-organvm](https://github.com/meta-organvm)
