---
layout: archive
title: "Önerdiğim İçerikler"
permalink: /recommends/
author_profile: true
header:
  overlay_image: /assets/images/svg-book19.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
excerpt: "Bu sayfada, kişisel gelişimimde bana büyük katkı sağlayan eğitmenleri, kişileri, içerikleri, ve izlediğim teknik videolardan beğendiklerimi sizlerle paylaşmaya çalışacağım. Kendim için aldığım bu notların size de faydalı olması dileği ile..."
entries_layout: grid
classes: wide
---

<h2>Önerdiğim İçerikler</h2>

Önerdiğim kişi ve içeriklerin yanı sıra kendi hazırladığım makaleleri de, daha kolay ulaşılabilmeleri için bu sayfada kategorik olarak paylaşmayı planlıyorum.

<br/><br/>

{% for post in site.categories.recommends %}
  {% include archive-single.html type="grid"%}
{% endfor %}

<!-- type="grid" ekleyince post'lara thumnail ekleniyor. Bak: https://github.com/mmistakes/minimal-mistakes/issues/892 -->
