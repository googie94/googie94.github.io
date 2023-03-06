---
layout: default
title: "Tech"
description: 배움의 목적은 사용에 있습니다
main: true
project-header: true
header-img: img/about.jpg
lastmod: 2023-03-04 2:00:00
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.tech == true %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>