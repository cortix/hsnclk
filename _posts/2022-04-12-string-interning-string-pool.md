---
title: "Java'da Hafıza Modeli 7 - String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark"
comments: false
excerpt: "Burada bu 3 sorunun yanıtını arayacağız."
header:
  teaser: "/assets/images/2022-04-12-string-interning-string-pool/img.png"
  #og_image: /assets/images/page-header-og-image.png
  og_image: /assets/images/2022-04-12-string-interning-string-pool/img.png
  overlay_image: /assets/images/unsplash-image-25.jpeg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Kelli Tungay](https://unsplash.com/photos/2LJ4rqK2qfU) on Unsplash"
  #video:
    #id: jT06ibYdEXo
    #provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - Java memory model
  - Java string interning
  - Java string pool
  - Java string constant pool
  - Java string intern pool
  - Java'da == operatörü ve equals metodu farkı
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
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
6. Java’da Hafıza Modeli 6 - [Java’da Statik ve Statik Olmayan Değişken ve Metotların Hafıza Yönetimi](/java-hafiza-yonetimi/Java-memory-models-static-nonstatic-members/)
7. **Java’da Hafıza Modeli 7 - String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark**

Buradan sonra aşağıdaki seriden devam etmenizden yarar var.

1. Java’da Kalıtım 1 - [Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?](/java-kalitim-polimorfizm/Java-inheritance1/)
...
</div>
## Genel Bakış

Arkadaşlar bu bir video içerik. Birbiri ile bağlantılı olduğu için aşağıdaki 3 sorunun yanıtını tek bir videoda cevaplamak istedim.

1. **String Interning** Nedir?
  * **çift tırnak (" ")** ile oluşturulan string objesinin, **new** anahtar kelimesi kullanılarak oluşturulan string objesinden farkı nedir?
2. **String Pool**, diğer ismiyle **string constant/intern pool** nedir ve neden vardır?
3. **==** operatörü ve **equals** metodu arasındaki fark nedir?

{% picture 2022-04-12-string-interning-string-pool/string-interning.png --alt Java String Interning Nedir, String Pool Nedir, == operatörü ve equals metodu Arasındaki Fark --img width="100%" height="100%" --link https://www.youtube.com/watch?v=jT06ibYdEXo %}

Daha önceki [makalelerimin](/java-hafiza-yonetimi/Java-memory-models-primitive-types/#string-interning) birinde string interning kavramına değinmiştim. Dilerseniz o makaleyi de inceleyebilirsiniz. Yalnız bu video daha detaylı olduğu ve 3 konuyu da ele aldığı için daha yararlı olacağını düşünüyorum.

## Referanslar:
* [String intern](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#intern())
* [String interning](https://en.wikipedia.org/wiki/String_interning)
