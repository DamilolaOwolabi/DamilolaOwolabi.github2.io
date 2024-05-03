---
layout: page
title: test_projects
permalink: /test_Projects/
description: A growing collection of test_projects done throughout my Data Science journey.
nav: false
nav_order: 9
display_categories: [school, fun]
horizontal: false
---

<!-- pages/test_projects.md -->
<div class="test_projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized test_projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_test_projects = site.test_projects | where: "category", category %}
  {% assign sorted_test_projects = categorized_test_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for test_projects in sorted_test_projects %}
      {% include test_projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for test_projects in sorted_test_projects %}
      {% include test_projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display test_projects without categories -->

{% assign sorted_test_projects = site.test_projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_test_projects %}
      {% include test_projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for test_projects in sorted_test_projects %}
      {% include test_projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
