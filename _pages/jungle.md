---
title: "Krafton Jungle"
permalink: /activities/jungle/
layout: single

toc: true
toc_sticky: true

---

## Jungle Log
{% for category in site.categories %}
  {% if category[0] == 'Krafton Jungle' %}
    {% for post in category[1] %}
      {% include archive-single.html %}
    {% endfor %}
  {% endif %}
{% endfor %}

## Jungle Book


<br> 

## 정글 tag 모아보기

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


