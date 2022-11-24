---
title: "Java'da Hafıza Modeli 6 - Java'da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi"
comments: false
excerpt: "Java Statik ve Statik Olmayan Konteks Nedir? Java'da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi Nasıl Olur? gibi soruların cevabını vermeye çalışacağım."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-60.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Omid Armin](https://unsplash.com/photos/edANaB0ZFVo) on Unsplash"
  video:
    id: cR9uwtMQt-g
    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - static context
  - instance context
  - static context memory management
  - non-static context memory management
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

Merhabalar arkadaşlar, bu blog yazımı video içerik oluşturarak hazırlamak istedim:) Özellikle statik ve statik olmayan değişken ve yöntemlerin neler olduğunu, hafızada nasıl yer kapladıklarını merak edenler için güzel bir içerik olduğunu düşünüyorum. Bununla beraber, birçoğunuzun **Non-static method '' cannot be referenced from a static context** hatasını neden aldığımızı merak ettiğinizi de tahmin edebiliyorum.. Bu video, bütün bu sorulara cevap niteliğindedir. Umarım sabırla sonuna kadar izlersiniz.


Bir konuyu anlayabilmek için önce kafada doğru bir tanıma oturtmanın, o konuyu öğrenmenin başlıca koşulu olduğunu her zaman düşünmüşümdür. Özellikle yazılım öğrenirken arka planda gerçekleşen işlemleri tasvir edebilmek gerçekten çok önemlidir. Bu konuda yeni yazılımcıların çok sıkıntı yaşadığını bildiğim için detaylı bir eğitim videosu hazırlamaya karar verdim. Umarım kafanızdaki birçok soruya cevap verebilirim. Şimdiden iyi seyirler.

{% include video id="cR9uwtMQt-g" provider="youtube" %}
