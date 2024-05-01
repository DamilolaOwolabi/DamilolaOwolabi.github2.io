---
layout: page
title: Apps
permalink: /Apps/
description: A growing collection of the Apps built throughout my Data Science Journey.
nav: true
nav_order: 7
display_categories: [School, fun]
horizontal: false
---

<!-- pages/Apps.md -->
<div class="Apps">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized Apps -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_Apps = site.Apps | where: "category", category %}
  {% assign sorted_Apps = categorized_Apps | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_Apps %}
      {% include Apps_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_Apps %}
      {% include Apps.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display Apps without categories -->

{% assign sorted_Apps = site.Apps | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_Apps %}
      {% include Apps_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_Apps %}
      {% include Apps.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
