---
title: Records
layout: splash
permalink: /records/
hidden: true
header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/blossom.jpg
  caption: April 12, 2017 1:14:24AM, KAIST
excerpt: >
  My own records. Daily life, travel history, and interests.
feature_row:
  - image_path: /assets/images/books.jpg
    alt: "books main"
    title: "Books"
    excerpt: "Interesting books: financial, scientific, and novels"
    image_caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
    url: "https://zenith-billboard-ae7.notion.site/8994dd1315e34e608277d49103da78c2"
    btn_class: "btn--primary"
    btn_label: "Read more"
  - image_path: /assets/images/travel.jpg
    alt: "travels main"
    title: "Travels"
    excerpt: "Travel histories"
    image_caption: "June 22, 2019 9:34:51PM, Amsterdam"
    url: "/records/travels/"
    btn_class: "btn--primary"
    btn_label: "Read more"
  - image_path: /assets/images/investments.jpg
    alt: "investments main"
    title: "Investments"
    excerpt: "Investments<br /><br />"
    image_caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
    url: "/records/investments/"
    btn_class: "btn--primary"
    btn_label: "Read more"
---

{% include feature_row %}

{% assign entries_layout = 'list' %}
<div class="entries-{{ entries_layout }}">
  {% include posts-category.html taxonomy='records' type=entries_layout %}
</div>
