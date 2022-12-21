---
title: "Java'da Hafıza Modeli 6 - Java'da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi"
comments: false
excerpt: "Java'da Statik ve Statik Olmayan Konteks Nedir? Java'da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi Nasıl Olur? gibi soruların cevabını vermeye çalışacağım."
header:
  teaser: "/assets/images/svg-book9.svg"
  og_image: //assets/images/svg-book9.svg
  overlay_image: /assets/images/svg-book9.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #video:
    #id: cR9uwtMQt-g
    #provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Java static context
  - Java instance context
  - Java static context memory management
  - Java non-static context memory management
last_modified_at: 2018-06-06T15:12:19-04:00
toc: true
toc_sticky: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java'da Hafıza Modeli Serisi</h4>
---

1. Java'da Hafıza Modeli 1 - [İlkel Veri Tipleri(Primitive Types)](/java-hafiza-yonetimi/Java-memory-models-primitive-types/)
2. Java’da Hafıza Modeli 2 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects/)
3. Java’da Hafıza Modeli 3 - [Nesneler](/java-hafiza-yonetimi/Java-memory-models-objects1/)
4. Java’da Hafıza Modeli 4 - [Kapsam(Scope)](/java-hafiza-yonetimi/Java-memory-models-scope/)
5. Java’da Hafıza Modeli 5 - [Pass By Value / Pass By Reference](/java-hafiza-yonetimi/Java-memory-models-pass-by-value-reference/)
6. **Java’da Hafıza Modeli 6 - Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi**
7. Java’da Hafıza Modeli 7 - [String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark](/java-hafiza-yonetimi/string-interning-string-pool/)
</div>

## Genel Bakış

Merhabalar arkadaşlar, bu blog yazımı video içerik oluşturarak hazırlamak istedim:) Özellikle statik ve statik olmayan değişken ve yöntemlerin neler olduğunu, hafızada nasıl yer kapladıklarını merak edenler için güzel bir içerik olduğunu düşünüyorum. Bununla beraber, birçoğunuzun **Non-static method '' cannot be referenced from a static context** hatasını neden aldığımızı merak ettiğinizi de tahmin edebiliyorum.. Bu video, bütün bu sorulara cevap niteliğindedir. Umarım sabırla sonuna kadar izlersiniz.


Bir konuyu anlayabilmek için önce kafada doğru bir tanıma oturtmanın, o konuyu öğrenmenin başlıca koşulu olduğunu her zaman düşünmüşümdür. Özellikle yazılım öğrenirken arka planda gerçekleşen işlemleri tasvir edebilmek gerçekten çok önemlidir. Bu konuda yeni yazılımcıların çok sıkıntı yaşadığını bildiğim için detaylı bir eğitim videosu hazırlamaya karar verdim. Umarım kafanızdaki birçok soruya cevap verebilirim. Şimdiden iyi seyirler.

{% picture 2018-06-09-Java-static-method/java-statik-nedir.png --alt Java Memory Management (Java Hafıza Yönetimi) --img width="100%" height="100%" --link https://www.youtube.com/watch?v=cR9uwtMQt-g %}
