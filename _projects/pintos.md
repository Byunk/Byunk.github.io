---
title: pintOS
excerpt: "Instructional OS for KAIST CS330"
layout: single
permalink: /pintos/
collection: pintos
author_profile: true
header:
  image: /assets/images/pintos/kernel_panic.jpg
  teaser: /assets/images/pintos/kernel_panic.jpg
sidebar:
  nav: "sidebar"
---

{% assign posts = site.categories.pintOS %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}