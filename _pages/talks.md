---
layout: page
title: talks
permalink: /talks/
description: A collection of my research talks and presentations.
nav: true
nav_order: 4
horizontal: true
---

<!-- pages/talks.md -->
<div class="talks">
  {% assign sorted_talks = site.talks | sort: "date" | reverse %}
  
  <div class="container">
    <div class="row row-cols-1">
      {% for talk in sorted_talks %}
        {% include talks_horizontal.liquid %}
      {% endfor %}
    </div>
  </div>
</div>