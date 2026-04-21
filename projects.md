---
layout: page
title: Projects
permalink: /projects/
description: "Products and projects I've built."
---

<div class="projects-grid">
  {% assign ordered = site.projects | sort: "order" %}
  {% for project in ordered %}
  <article class="project-card">
    <h3 class="project-card-title">
      <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      {% if project.status %}<span class="project-card-status">{{ project.status }}</span>{% endif %}
    </h3>
    <p class="project-card-desc">{{ project.description }}</p>
    {% if project.link %}
    <a href="{{ project.link }}" class="project-link" target="_blank" rel="noopener">{{ project.link | remove: 'https://' }} &nearr;</a>
    {% endif %}
  </article>
  {% endfor %}
</div>

{% if site.projects.size == 0 %}
Projects coming soon.
{% endif %}
