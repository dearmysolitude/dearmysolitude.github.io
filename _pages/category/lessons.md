---
title: "Problem Solving & Lessons"
permalink: /activities/problem-solving&lessons/
layout: category
author_profile: true
sidebar_main: true
taxonomy: Lesson
classes: wide
---

발생한 문제를 해결하거나 배운 점들에 대한 게시물을 모아놓은 페이지입니다.

<H2>Problem Solving</H2>

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

<div style="clear: both;">
  <h2 id="projects" class="archive__subtitle">Mini Project</h2>
  {% for post in site.project %}
    {% if post.tags contains 'Problem Solving' %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
</div>

<div style="clear: both;">
<H2>Lesson</H2>
</div>