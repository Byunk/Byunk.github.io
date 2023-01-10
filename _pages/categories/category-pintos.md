---
title: pintOS
layout: collection
permalink: /pintos/
collection: pintos
sidebar:
    nav: "sidebar-category"
---

{% assign posts = site.categories.pintOS %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}