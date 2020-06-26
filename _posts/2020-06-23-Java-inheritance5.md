---
title: "Java'da Kalıtım 5 - Java'da Nesne Oluşturma"
comments: true
excerpt: "Bu derste Java'da Nesne oluşturma işlemini hafıza modeli konusunda işlemiştik. Bu konuyu tekrar gözden geçirmemizin nedeni, artık kalıtım konusunu bildiğimizden, şimdi nesnelerin gerçekte nasıl inşa edildiği hakkında daha fazla ayrıntı öğrenebiliriz."
header:
  teaser: "assets/images/equality.png"
  og_image: /assets/images/page-header-og-image.png
  overlay_image: /assets/images/unsplash-image-47.jpg
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Tamas Tuzes-Katai](https://unsplash.com/photos/LzpTVcfTBE8) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - nesne oluşumu
  - Object sınıfı
  - new anahtar kelimesi
  - Kurucu(constructor)
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

## Genel Bakış

Bu derste işleyeceklerimiz, tüm nesnelerin aslında ``Object`` sınıfından türetildiğini, özünde bu sınıfı miras aldığını tanımak olacaktır. Bununla beraber, Java'da nesnelerin içten dışa nasıl inşa edildiğini görecek ve sonrasında örnek değişkenlerin ``Object`` sınıfından başlayarak nasıl başlattığına(initialization) bakacağız.

Java'da nesnelerin içten dışa nasıl inşa edildiğini gördük.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/inheritance.png" alt="inheritance">
  <figcaption></figcaption>
</figure>

Aşağıdaki şekilde gördüğünüz kod bloğundaki ``new`` ahantar kelimesi aslında bir operatördür. Bu operatör, burada alan ayırmak anlamına gelir. Sonrasında **this** hemen bu alana, yani constructor'a verilir. Yani "this" kurucunun(constructor) başlaması(initialization) için bu alana referans olarak geçer. Aslında yapıcılar(constructor) başlatıcılar(initializers) olarak adlandırılmalıdır çünkü diğer programlama dillerinde aslında **init** olarak adlandırıldığını göreceksiniz.

> Bir örnek yöntemi veya bir kurucu içinde **this**, geçerli nesneye (yöntemi veya kurucusu çağrılan nesne) bir referanstır. **This** komutunu kullanarak bir örnek yönteminden veya bir kurucudan geçerli nesnenin herhangi bir üyesine başvurabilirsiniz.

<figure style="width: 400px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/new1.png" alt="new">
  <figcaption></figcaption>
</figure>

 Konumuza geri dönecek olursak, burada ``Student`` kurucusunun yaptığı, öğrenciyle ilişkili değişkenleri başlatmaktır. Ve bunu yapmanın yolu aslında içten dışa doğru yol almaktır. Çünkü nesneler içten dışa doğru oluşturulurlar. Hiyerarşiye göre ``Object`` sınıfına kadar gider ve sonra başladığınız sınıfa geri dönersiniz. Geri döndükçe ilgili tüm değişkenleri başlatırsınız(yani ilklendirirsiniz, yani atama işlemlerini yaparsınız:)). Bu işlemin nasıl gerçekleştiğini anlamak için adım adım ilerlememizde yarar var.

  ``new`` operatörü ile bir obje alanı(boşluğu) yarattık. Sonrasında ise bu alana(boşluğa) constructor aracılığı ile girdik. Aşağıdaki şekilde görebilirsiniz.

 <figure style="width: 600px" class="align-center">
   <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy1_1.png" alt="hierarchy">
   <figcaption></figcaption>
 </figure>

Akabinde ilk kod satırı olan ``Student`` yapıcısı/kurucusu(yani constructor), bizi superclass yapıcısına gönderecektir. Bu durumda Person'dan bahsettiğimizi anlamışsınızdır.

 <figure style="width: 600px" class="align-center">
   <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy1_2.png" alt="hierarchy">
   <figcaption></figcaption>
 </figure>

``Person()`` kurucusunun ilk kod satırı da bizi hemen indirect superclass kurucusuna, yani bu durumda Object'e gönderecektir.

 <figure style="width: 600px" class="align-center">
   <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy1_3.png" alt="hierarchy">
   <figcaption></figcaption>
 </figure>

Şimdi, ``Object()`` yapıcısı object ile ilişkili değişkenleri başlatabilir. Indirect superclass yazan yeri ``Object()`` yapıcısının o alanı başlattığını göstermek için yeşile boyamak istiyorum.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy2.png" alt="hierarchy">
  <figcaption></figcaption>
</figure>

Tamamlandığında java, ``Person()`` kurucusuna geri döner ve ``Person`` ile ilişkili değişkenlerini başlatır.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy3.png" alt="hierarchy">
  <figcaption></figcaption>
</figure>

Ve sonra ``Student()`` kurucusuna geri döner ve ``Student`` ile ilişkili değişkenlerini başlatır.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy4.png" alt="hierarchy">
  <figcaption></figcaption>
</figure>

Bu süreç boyunca aslında tüm bu değişkenleri başlattık, hatta nesneye kadar gidip geri döndük. Aslında içten dışa ilklendirmeden bunu kastediyorduk.

<figure style="width: 600px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy5.png" alt="hierarchy">
  <figcaption></figcaption>
</figure>

Burada şu soruyu sorabilirsiniz. ``Object`` sınıfını ``extends`` etmediğimiz halde java bunu nasıl biliyor? Bir sonraki bölümde bunun cevabını bulmaya çalışalım.

## Özet

Nesneleri oluştururken, alt sınıf kurucuları, üst sınıf kurucularını hiyerarşik sırada Object sınıfına ulaşana kadar çağırır, sonrasında örnek değişkenleri Object sınıfından başlayarak ve alt sınıfınıza giden miras hiyerarşisinde aşağı doğru ilerleyerek başlatılırlar. (Bu nedenle hiyerarşide en alt sınıfınızdaki örnek değişkeni en son başlatılır.)


## Referanslar
* [https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [OCA Java SE 8 Programmer I Exam Guide (Exams 1Z0-808)](https://www.amazon.com/Java-Programmer-Guide-Exams-1Z0-808/dp/1260011399)
* [https://www.coursera.org/learn/object-oriented-java/lecture/C1mcK/core-visibility-modifiers](https://www.coursera.org/learn/object-oriented-java/lecture/C1mcK/core-visibility-modifiers)
* [https://www.coursera.org/learn/object-oriented-java/discussions/weeks/4/threads/Y2Z1KsI6EeemsxIeSdRoqg/replies/uQnB5cgsEeemkhJKRD6SRA](https://www.coursera.org/learn/object-oriented-java/discussions/weeks/4/threads/Y2Z1KsI6EeemsxIeSdRoqg/replies/uQnB5cgsEeemkhJKRD6SRA)
* https://www.coursera.org/learn/object-oriented-java/discussions/weeks/4/threads/rjslR6OMEee1fxJ_AEil0g
