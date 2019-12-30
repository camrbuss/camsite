---
layout: default
permalink: /
---

Welcome to my (Cam) personal website. Take a look at my projects, photography, and a sneak peek into my life. 

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