---
title: "pintOS"
layout: collection
permalink: categories/pintos
author_profile: true
---

{% assign posts = site.categories.pintOS %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}