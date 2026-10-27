---
layout: page
title: projects
permalink: /projects/
description: Research on player identification, and independent projects reimplementing and extending ML methods.
nav: true
nav_order: 2
display_categories: [research, independent]
---

<!-- Projects as a list, styled like the publications list; rows come from _includes/projects_list.liquid -->
<div class="publications">
{% for category in page.display_categories %}
  <h2 class="bibliography">{{ category }}</h2>
  {% assign categorized_projects = site.projects | where: "category", category | sort: "importance" %}
  <ol class="bibliography">
    {% for project in categorized_projects %}
      {% include projects_list.liquid %}
    {% endfor %}
  </ol>
{% endfor %}
</div>
