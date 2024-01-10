---
title: "Problem solving & lessons"
permalink: /problem-solving-&-lessons/
layout: category
author_profile: true
sidebar_main: true
taxonomy: Problem solving & lessons
classes: wide
show_date: true
---

문제를 해결하거나 배운 점들을 정리한 페이지

<H2>🧐 Problem Solving</H2>

{% for category in site.categories %}
  {% assign has_tag = false %}

  {% for post in category[1] %}
    {% for tag in post.tags %}
      {% if tag == 'Problem Solving' %}
        {% assign has_tag = true %}
      {% endif %}
    {% endfor %}
  {% endfor %}

  {% if has_tag %}
<h2 id="projects" class="archive__subtitle">{{ category[0] }}</h2>
  {% for post in category[1] %}
    {% for tag in post.tags %}
      {% if tag == 'Problem Solving' %}
        {% include archive-single.html %}
      {% endif %}
    {% endfor %}
  {% endfor %}
  {% endif %}

{% endfor %}

<h2 id="projects" class="archive__subtitle">Mini Project</h2>
{% for post in site.project %}
  {% if post.tags contains 'Problem Solving' %}
    {% include archive-single.html %}
  {% endif %}
{% endfor %}

<H2>🤓 Lesson</H2>