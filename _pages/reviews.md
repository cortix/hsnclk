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

Yaptığım kitap incelemeleri yalnızca beğendiğim kitapları kapsıyor. Genellikle bu kitaplar, **goodreads** platformu üzerinde, puan olarak 4 veya 5 yıldız verdiğim kitaplardan oluşuyor. Nadiren, puan olarak 2 veya 3 yıldız verdiklerime eleştiri mahiyetinde de olsa bir yorum bırakıyorum. Bu kitapların tam listesine ulaşmak için [goodreads hesabımı](https://www.goodreads.com/user/show/88145705-hasan-elik) ziyaret edebilirsiniz.

<br/><br/>

{% for post in site.categories.book-reviews %}
  {% include archive-single.html type="grid"%}
{% endfor %}

<!-- type="grid" ekleyince post'lara thumnail ekleniyor. Bak: https://github.com/mmistakes/minimal-mistakes/issues/892 -->
