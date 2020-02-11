---
layouts: page
title: Sailing
---

# Sailing


<ul>
  {% for p in site.sailing %}
    <li>
      <a href="{{ p.url }}"><h2>{{ p.title }}</h2></a>
      <p>{{ p.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>