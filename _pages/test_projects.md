---
layout: page
title: test_Projects
permalink: /test_Projects/
description: A growing collection of test_Projects done throughout my Data Science journey. test2
nav: false
nav_order: 9
horizontal: false
---

<!-- pages/test_Projects.md -->
<div class="test_Projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized test_Projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_test_Projects = site.test_Projects | where: "category", category %}
  {% assign sorted_test_Projects = categorized_test_Projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_test_Projects %}
      {% include test_Projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_test_Projects %}
      {% include test_Projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display test_Projects without categories -->

{% assign sorted_test_Projects = site.test_Projects | sort: "importance" %}

  <!-- Generate cards for each test project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_test_Projects %}
      {% include test_Projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_test_Projects %}
      {% include test_Projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
