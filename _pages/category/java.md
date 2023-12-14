---
title: "Java"
permalink: /java/
layout: category
author_profile: true
sidebar_main: true
toc_sticky: true
toc_ads : true
taxonomy: Java
---

{% assign posts = site.java %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}