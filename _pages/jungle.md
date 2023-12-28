---
title: "Krafton Jungle 모아보기"
permalink: /activities/jungle
layout: archive

toc: true
toc_sticky: true
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
<h2>{{ category[0] }}</h2>
    {% for post in category[1] %}
      {% for tag in post.tags %}
        {% if tag == 'Krafton Jungle' %}
          {% include archive-single.html %}
        {% endif %}
      {% endfor %}
    {% endfor %}
  {% endif %}

{% endfor %}


