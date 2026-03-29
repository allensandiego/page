---
layout: page
title: Projects
icon: fas fa-code
order: 2
---

<div class="project-grid">
{% for project in site.data.projects %}
<div class="project-card" {% if project.internal_link %}onclick="location.href='{{ project.internal_link | relative_url }}'" style="cursor: pointer;"{% endif %}>
  <div class="project-card-title">
    {% if project.internal_link %}
    <a href="{{ project.internal_link | relative_url }}">{{ project.title }}</a>
    {% else %}
    {{ project.title }}
    {% endif %}
  </div>
  <div class="project-card-subtitle">{{ project.subtitle }}</div>
  <p>{{ project.description }}</p>
  {% if project.status %}
  <p><em>{{ project.status }}</em></p>
  {% endif %}
  <a href="{{ project.link }}" target="_blank" rel="noopener noreferrer">View here →</a>
</div>
{% endfor %}
</div>
