---
title: "Java'da Hafıza Modeli 7 - String Interning Nedir, String Pool Nedir, == ve equals Arasındaki Fark"
comments: true
excerpt: "Burada bu 3 sorunun yanıtını arayacağız."
header:
  teaser: "/assets/images/2022-04-12-string-interning-string-pool/img.jpeg"
  #og_image: /assets/images/page-header-og-image.png
  og_image: /assets/images/2022-04-12-string-interning-string-pool/img.jpeg
  overlay_image: /assets/images/unsplash-image-25.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Kelli Tungay](https://unsplash.com/@kellitungay?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash"
  video:
    id: jT06ibYdEXo
    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-hafiza-yonetimi
tags:
  - string interning
  - string pool
  - string constant pool
  - string intern pool
  - == ve equals farkı
last_modified_at: 2022-02-23T15:12:19-04:00
toc: true
toc_label: "CONTENT"
---



**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

Arkadaşlar bu bir video içerik. Birbiri ile bağlantılı olduğu için aşağıdaki 3 sorunun yanıtını tek bir videoda cevaplamak istedim.

1. **String Interning** Nedir?
2. **String Pool**, diğer ismiyle **string constant/intern pool** nedir ve neden vardır?
3. **==** ve **equals** arasındaki fark nedir?

{% include video id="jT06ibYdEXo" provider="youtube" %}

Daha önceki [makalelerimin](/java-hafiza-yonetimi/Java-memory-models-primitive-types/#string-interning) birinde string interning kavramına değinmiştim. Dilerseniz o makaleyi de inceleyebilirsiniz. Yalnız bu video daha detaylı olduğu ve 3 konuyu da ele aldığı için daha yararlı olacağını düşünüyorum.

## Reference:
* [https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#intern()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#intern())
* [https://en.wikipedia.org/wiki/String_interning](https://en.wikipedia.org/wiki/String_interning)
