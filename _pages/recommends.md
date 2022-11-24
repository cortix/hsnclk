---
layout: archive
title: "Önerdiğim İçerikler"
permalink: /recommends/
author_profile: true
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/unsplash-image-2.webp
  #cta_label: "Download"
  #cta_url: "https://github.com/mmistakes/minimal-mistakes/"
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: "Bu sayfada, gelişimimde bana büyük katkı sağlayan kişileri, içerikleri ve izlediğim teknik videolardan beğendiklerimi sizinle paylaşacağım. Kendim için aldığım bu notların size de faydalı olması dileği ile..."
entries_layout: grid
classes: wide
---

<h2>Önerdiğim İçerikler</h2>
{% for post in site.categories.recommends %}
  {% include archive-single.html type="grid"%}
{% endfor %}

<!-- type="grid" ekleyince post'lara thumnail ekleniyor. Bak: https://github.com/mmistakes/minimal-mistakes/issues/892 -->
