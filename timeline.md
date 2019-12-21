---
layout: default
title: Timeline
permalink: /timeline/
---

# {{ page.title }}

### The following is a timeline of the major events in my life.


 <div class="timeline">
  {% assign sideAlternator = true %}
  {% for event in site.data.events %}
    {% if sideAlternator == true %}
  <div class="container left">
    <div class="content">
      <h3>{{ event.date }}</h3>
      <p>{{ event.name }}</p>
    </div>
  </div>
    {% assign sideAlternator = false %}
    {% else %}
  <div class="container right">
    <div class="content">
      <h3>{{ event.date }}</h3>
      <p>{{ event.name }}</p>
    </div>
  </div>
    {% assign sideAlternator = true %}
    {% endif %}

  {% endfor %}

</div> 