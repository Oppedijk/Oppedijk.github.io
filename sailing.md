---
layouts: page
title: Sailing
---

# Sailing


<ul>
  {% for p in site.sailing %}
    <li>
      <a href="{{ p.url }}">{{ p.title }}</a>
     
    </li>
  {% endfor %}
</ul>