---
layout: default
title: Timeline
permalink: /timeline/
---

Timeline



 <div class="timeline">
  {% assign sideAlternator = true %}
  {% for event in site.data.events %}
    {% if sideAlternator == true %}
  <div class="container left">
    <div class="content">
      <h2>{{ event.date }}</h2>
      <p>{{ event.name }}</p>
    </div>
  </div>
    {% assign sideAlternator = false %}
    {% else %}
  <div class="container right">
    <div class="content">
      <h2>{{ event.date }}</h2>
      <p>{{ event.name }}</p>
    </div>
  </div>
    {% assign sideAlternator = true %}
    {% endif %}

  {% endfor %}

</div> 