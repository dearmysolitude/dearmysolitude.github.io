---
title: "Project"
layout: archive
permalink: /project/
collection: project
classes: wide
sidebar:
  - text: "# 기술 스택"
  - title: "언어 / Language"
    text: "Java, Python, C"
  - title: "프레임워크 / Framework"
    text: "Spring, Spring Boot"
  - title: "구성 / Infrastructure"
    text: "AWS EC2, AWS CodeDeploy, Github Actions, NginX, Ubuntu 22.04(LTS)"
  - title: "개발 환경 / IDE"
    text: "IntelliJ[Java], VS Code[C/C++(with WSL2, GCC), Python]"
---

<div style="clear: both;">
  <h2 id="projects" class="archive__subtitle">프로젝트</h2>
  {% for post in site.project %}
    {% unless post.tags contains '미니 프로젝트' %}
      {% include archive-single.html type="grid" %}
    {% endunless %}
  {% endfor %}
</div>

<div style="clear: both;">
  <h2 id="mini-projects" class="archive__subtitle">미니 프로젝트</h2>
  {% for post in site.project %}
    {% if post.tags contains '미니 프로젝트' %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
</div>