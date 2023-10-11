---
title: "Java'da Static Initializer nedir? (Java statik ilklendirici)"
comments: false
excerpt: "Java Hafıza yönetimin önemli bir parçası olan statik ilklendirici konusunu bu bölümde ele almaya çalışacağım."
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
  - Java Static Initializer nedir? (Java statik ilklendirici)
#last_modified_at: 2023-01-06T15:12:19-04:00
last_modified_at:
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
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
  6. Java’da Hafıza Modeli 6 - [Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi](/java-hafiza-yonetimi/Java-memory-models-static-nonstatic-members/)
  7. Java’da Hafıza Modeli 7 - [String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark](/java-hafiza-yonetimi/string-interning-string-pool/)
  8. **Java’da Hafıza Modeli 8 - Java’da Static Initializer nedir? - Java statik ilklendirici**
  9. Java’da Hafıza Modeli 9 - [Java’da Instance Initializer Nedir? - Java örnek ilklendirici](/java-hafiza-yonetimi/Java-örnek-ilklendirici-2/)
  10. Java’da Hafıza Modeli 10 - [Java’da Neden BAZI durumlarda metot kullanmak yerine initializer’ı tercih ederiz?](/java-hafiza-yonetimi/Java-metot-yerine-ilklendirici-kullanmak-3/)
  
  Buradan sonra aşağıdaki seriden devam etmenizden yarar var.
  1. Java’da Kalıtım 1 - [Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?](/java-kalitim-polimorfizm/Java-inheritance1/)
  ...
</div>

## Java'da Statik İlklendirici Nedir?

Java’da Hafıza Yönetim Serisinin önemli bir parçası olan statik ilklendirici videoyu izlemek için aşağıdaki resme ya da bu [linke](https://youtu.be/h-MPSkUk414) tıklayabilirsiniz.

<br/>{% picture 2023-10-05-Java-statik-ilklendirici-1/java-static-initializer.png --alt Java'da Static Initializer nedir? (Java statik ilklendirici) Management --img width="100%" height="100%" --link https://youtu.be/h-MPSkUk414 %}<br/>

Diğer videolara ulaşmak için aşağıdaki resimlere ya da bu [2](https://youtu.be/TNsdmcYCckw), [3](https://youtu.be/kJEaDx6dPWk) linklere tıklayabilirsiniz

<br/>{% picture 2023-10-06-Java-örnek-ilklendirici-2/java-instance-initializer.png --alt Java'da Instance Initializer Nedir? (Java örnek ilklendirici) Management --img width="30%" height="30%" --link https://youtu.be/TNsdmcYCckw %}<br/>
<br/>{% picture 2023-10-07-Java-metot-yerine-ilklendirici-kullanmak-3/neden-initializer.png --alt Java'da BAZI Durumlarda Metot Yerine Initializer Kullanmak? Management --img width="30%" height="30%" --link https://youtu.be/kJEaDx6dPWk %}<br/>