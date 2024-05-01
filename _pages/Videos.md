---
layout: page
title: videos
permalink: /videos/
description: A growing collection of the Presentations I have done throughout my Data Science Journey.
nav: true
nav_order: 8
display_categories: [School, fun]
horizontal: false
---

<!-- pages/videos.md -->
<div class="videos">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized videos -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_videos = site.videos | where: "category", category %}
  {% assign sorted_videos = categorized_videos | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_videos %}
      {% include videos_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_videos %}
      {% include videos.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display videos without categories -->

{% assign sorted_videos = site.videos | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_videos %}
      {% include videos_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_videos %}
      {% include videos.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
