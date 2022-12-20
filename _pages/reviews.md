---
layout: archive
title: "Kitap Tavsiyeleri ve Kitap İncelemeleri"
permalink: /reviews/
author_profile: true
header:
  overlay_image: /assets/images/svg-book5.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
excerpt: "Fırsat buldukça okuduğum kitapların incelemelerini bu sayfada paylaşmaya çalışacağım."
entries_layout: grid
classes: wide
---

<h2>Kitap Değerlendirmeleri</h2>
{% for post in site.categories.book-reviews %}
  {% include archive-single.html type="grid"%}
{% endfor %}

<!-- type="grid" ekleyince post'lara thumnail ekleniyor. Bak: https://github.com/mmistakes/minimal-mistakes/issues/892 -->
