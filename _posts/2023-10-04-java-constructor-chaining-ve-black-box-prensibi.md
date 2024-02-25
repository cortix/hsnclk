---
title: "Java'da Constructor Chaining ve Black Box Prensibi"
comments: false
excerpt: "Java'da Constructor Chaining (Kurucu Zincirleme) ve Black Box Prensibinin birlikte kullanılması"
header:
  teaser: "/assets/images/svg-book10.svg"
  og_image: /assets/images/svg-book10.svg
  overlay_image: /assets/images/svg-book10.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
#  video:
#    id: cR9uwtMQt-g
#    provider: youtube
#cta_label: "More Info"
#cta_url: "https://unsplash.com"
categories:
  - java-derleyici-kurallari
tags:
  - Java inheritance
  - Java derleyici kuralları
  - Nesne oluşumu
  - Object sınıfı
  - new anahtar kelimesi
  - Kurucu(constructor)
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at: 2024-01-28T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış
Java’da Derleyici Kuralları 4. Videoyu izlemek için aşağıdaki resme ya da bu [linke](https://youtu.be/O3aXsurZRH4) tıklayabilirsiniz.

<br/>{% picture 2023-10-04-java-constructor-chaining-ve-black-box-prensibi/java-compiler-rule-4.jpg --alt Java'da Constructor Chaining ve Black Box Prensibi --img width="100%" height="100%" --link https://youtu.be/O3aXsurZRH4 %}<br/>

Bu bölümle ilgili yazılı içeriği aşağıdaki makalelerimde bulabilirsiniz.

1. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
2. Java’da Kalıtım 6 - [Sınıf İnşası İçin Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
3. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)

