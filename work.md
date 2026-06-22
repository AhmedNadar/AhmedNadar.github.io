---
layout: page
title: Work
permalink: /work/
description: "I build software with AI and help small businesses do the same: integrate AI into the applications they run, or build their own from scratch. Live products, real users, shipped fast."
---

I build software with development and AI woven into one process, so a working product ships in weeks instead of quarters.

I help small businesses do the same. Integrate AI into the applications they already run, or build their own application from scratch instead of renting someone else's. The kind of technology that used to take a large team now fits one focused operator who understands the problem and ships against it.

Everything below is live in front of real users today. Each one started with a real problem, got built with Rails and AI agents, and is running now. If you want AI working inside your product instead of sitting on a slide, <a href="{{ '/contact/' | relative_url }}">get in touch</a>.

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
