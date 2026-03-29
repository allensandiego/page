---
layout: page
title: Skills
icon: fas fa-star
order: 3
---

{% for category in site.data.skills %}
### {{ category.category }}

<ul class="skills-list">
  {% for item in category.items %}
  <li>{{ item }}</li>
  {% endfor %}
</ul>
{% endfor %}
