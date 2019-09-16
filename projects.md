---
layout: page
permalink: /projects/
---

<ul>
  {% for project in site.projects %}
    <h3>
      <a href="{{ project.url }}">{{ project.title }}</a>
    </h3>
  {% endfor %}
</ul>
