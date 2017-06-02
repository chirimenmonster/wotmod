---
layout: page
title: 下書き一覧
sitemap: false
---

<ul>
  {% for page in site.pages %}
    {% assign dir = page.path | split: "/" %}
    {% if dir.first == "drafts" and dir.last != "index.md" %}
      <li><a href="{{ page.url | relative_url }}">{{ page.title | escape }}</a></li>
    {% endif %}
  {% endfor %}
</ul>
