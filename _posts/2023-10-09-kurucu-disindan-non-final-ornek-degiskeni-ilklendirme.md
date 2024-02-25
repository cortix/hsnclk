---
title: "Efektif Java - Item 19'dan Bir Kesit (Java'da Constructor Dışından Non-final Örnek Değişkeni İlklendirme)"
comments: false
excerpt: "Java'da Kurucu dışından non-final (final olmayan) değişken initialize işlemleri nasıl gerçekleşir? Joshua Bloch Efektif Java Kitabındaki bir örnek üzerinde bu durumu el aldım."
header:
  teaser: "/assets/images/svg-book26.svg"
  og_image: /assets/images/svg-book26.svg
  overlay_image: /assets/images/svg-book26.svg
  overlay_color: "#000"
  overlay_filter: "0.3"
  caption: "background by [SVGBackgrounds.com](https://www.svgbackgrounds.com/)"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Efektif Java
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at: 2024-01-28T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Java'da Constructor Dışından Non-final Örnek Değişkeni İlklendirme

Bu videoyu Java’da Hafıza Yönetim Serisinin bir parçası olarak düşünebilirsiniz. Bunun yanı sıra Joshua Bloch'un meşhur kitabı Efektif Java'da bahsi geçen bir konu üzerinden non-final değişken ilklendirmesini ele aldığım için Efektif Java başlıklı bir kategoride bu videoyu sınıflandırabiliriz. Bu kitaptaki konuları da fırsat buldukça ele almaya çalışacağım. Özetle bu videoyu izlemek için aşağıdaki resme ya da bu [linke](https://youtu.be/eZg0rbJE_Os) tıklayabilirsiniz.

<br/>{% picture 2023-10-09-kurucu-disindan-non-final-ornek-degiskeni-ilklendirme/non-final-initialize.jpg --alt Java'da Constructor Dışından Non-final Örnek Değişkeni İlklendirme - Efektif Java Kitabı Item 19'dan Bir Kesit --img width="100%" height="100%" --link https://youtu.be/eZg0rbJE_Os %}<br/>

## Video İçeriği:

* Constructor(kurucu, yapıcı) Dışından Non-final Örnek Değişkeni initialize etme
* Joshua Bloch'un Efektif Java kitabı Item 19'da olan bir örnek kod üzerinde yapılan bir değişiklik sonucu çıkan bir örnek.
* final olmayan örnek değişkeni ilklendirmeleri (non-final instance initialize işlemi)
* Kurucu içinde ve kurucu dışında yapılan örnek değişkeni ilklendirmesi

<figure class="half">
  <a href="https://youtu.be/h-MPSkUk414" >{% picture 2023-10-05-Java-statik-ilklendirici-1/java-static-initializer.png --alt Java'da Static Initializer nedir? (Java statik ilklendirici) Management %}</a>
  <a href="https://youtu.be/TNsdmcYCckw" >{% picture 2023-10-06-Java-örnek-ilklendirici-2/java-instance-initializer.png --alt Java'da Instance Initializer Nedir? (Java örnek ilklendirici) Management %}</a>
  <figcaption></figcaption>
</figure>

<figure class="third">
  <a href="https://youtu.be/kJEaDx6dPWk" >{% picture 2023-10-07-Java-metot-yerine-ilklendirici-kullanmak-3/neden-initializer.png --alt Java'da BAZI Durumlarda Metot Yerine Initializer Kullanmak? Management %}</a>
  <a href="https://youtu.be/1jdo_04jHI4" >{% picture 2023-10-08-Java-kurucudan-gecersiz-kilinan-metot-cagirma/efektif-java-item-19.jpg --alt Java'da Constructor İçinden Overridable Metot Çağrılır mı? - Efektif Java Kitabı Item 19'dan Bir Kesit %}</a>
  <a href="https://www.youtube.com/watch?v=5LH2bZdnYE4" >{% picture 2023-10-10-kurucu-disindan-final-ornek-degiskeni-ilklendirme/final-initialize.jpg --alt Java'da Constructor Dışından Final Örnek Değişkeni İlklendirme - Efektif Java Kitabı Item 19'dan Bir Kesit %}</a>
  <figcaption></figcaption>
</figure>

