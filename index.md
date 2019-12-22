---
layout: default
permalink: /
---

Welcome 

{% for project in site.projects %}
  <h2>
    <a href="{{ project.url | prepend: site.baseurl }}">
      {{ project.name }}
    </a>
  </h2>
  <p>{{ project.description }}</p>
  <a href="{{ project.url | prepend: site.baseurl }}">
    <img src="{{ project.image | prepend: site.baseurl }}" />
  </a>
{% endfor %}