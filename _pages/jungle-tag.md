---
title: "Krafton Jungle Archive"
layout: archive
permalink: /activities/kraftonjungle/tags
author_profile: true
---

{% for category in site.categories %}
  {% assign has_tag = false %}

  {% for post in category[1] %}
    {% for tag in post.tags %}
      {% if tag == 'Krafton Jungle' %}
        {% assign has_tag = true %}
      {% endif %}
    {% endfor %}
  {% endfor %}

  {% if has_tag %}
### {{ category[0] }}
  {% for post in category[1] %}
    {% for tag in post.tags %}
      {% if tag == 'Krafton Jungle' %}
        {% include archive-single.html %}
      {% endif %}
    {% endfor %}
  {% endfor %}
  {% endif %}

{% endfor %}

## Jungle Book
