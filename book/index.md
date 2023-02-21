---
layout: default
title: "Book"
description: 하루에 30분 책읽기
main: true
project-header: true
header-img: img/about.jpg
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.book == true %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>