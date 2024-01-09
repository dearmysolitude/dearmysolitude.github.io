---
title: "Portfolio"
layout: archive
permalink: /project/
collection: project
classes: wide
sidebar:
  - title: "언어 & 프레임워크"
    text: "Java, Python, C / Spring, Spring Boot"
  - title: "구성"
    text: "AWS EC2, AWS CodeDeploy, Github Actions, NginX, Ubuntu 22.04(LTS)"
  - title: "협업 & 형상 관리"
    text: "Slack / Github"
  - title: "DB"
    text: "MySQL"
  - title: "개발 환경"
    text: "IntelliJ, VS Code(WSL2, GCC)"
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