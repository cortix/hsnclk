---
title: "Java'da Kalıtım 5 - Java'da Nesne Oluşturma"
comments: false
excerpt: "Bu derste Java'da Nesne oluşturma işlemini hafıza modeli konusunda işlemiştik. Bu konuyu tekrar gözden geçirmemizin nedeni, artık kalıtım konusunu bildiğimizden, şimdi nesnelerin gerçekte nasıl inşa edildiği hakkında daha fazla ayrıntı öğrenebiliriz."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-47.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Tamas Tuzes-Katai](https://unsplash.com/photos/LzpTVcfTBE8) on Unsplash"
#  video:
#    id: cR9uwtMQt-g
#    provider: youtube
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - java inheritance
  - nesne oluşumu
  - Object sınıfı
  - new anahtar kelimesi
  - Kurucu(constructor)
last_modified_at: 2020-06-06T15:12:19-04:00
toc: true
toc_label: "SAYFA İÇERİĞİ"
toc_sticky: true
---

**ÖNEMLİ :** Kendim için aldığım notlar. Umarım size de bir faydası olur. Kullanılan her bir makale referans olarak eklenmiştir.
{: .notice}

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Java’da Kalıtım(inheritance) ve Java’da Polimorfizm Serisi</h4>
---

1. Java’da Kalıtım 1 - [Kalıtımı Neden Kullanırız? Kalıtımı Sağlamak İçin Asgari Şartlar Nelerdir?](/java-kalitim-polimorfizm/Java-inheritance1/)
2. Java’da Kalıtım 2- [Extends](/java-kalitim-polimorfizm/Java-inheritance2/)
3. Java’da Kalıtım 3 - [Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/)
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. **Java’da Kalıtım 5 - Java’da Nesne Oluşturma**
7. Java’da Kalıtım 6 - [Sınıf İnşası için Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
11. Java’da Kalıtım 10 - [Overloading(Aşırı Yükleme) Metotlar](/java-kalitim-polimorfizm/Java-inheritance10/)
</div>

## Genel Bakış

Bu derste işleyeceklerimiz, tüm nesnelerin aslında ``Object`` sınıfından türetildiğini, özünde bu sınıfı miras aldığını tanımak olacaktır. Bununla beraber, Java'da nesnelerin içten dışa nasıl inşa edildiğini görecek ve sonrasında örnek değişkenlerin ``Object`` sınıfından başlayarak nasıl başlattığına(initialization) bakacağız.

Java'da nesnelerin içten dışa nasıl inşa edildiğini gördük.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/inheritance.webp"  width="100%" height="100%" loading="lazy" alt="inheritance">

Aşağıdaki şekilde gördüğünüz kod bloğundaki ``new`` ahantar kelimesi aslında bir operatördür. Bu operatör, burada alan ayırmak anlamına gelir. Sonrasında **this** hemen bu alana, yani constructor'a verilir. Yani "this" kurucunun(constructor) başlaması(initialization) için bu alana referans olarak geçer. Aslında yapıcılar(constructor) başlatıcılar(initializers) olarak adlandırılmalıdır çünkü diğer programlama dillerinde aslında **init** olarak adlandırıldığını göreceksiniz.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
Bir örnek yöntemi veya bir kurucu içinde **this**, geçerli nesneye (yöntemi veya kurucusu çağrılan nesne) bir referanstır.

**this** komutunu kullanarak bir örnek yönteminden veya bir kurucudan geçerli nesnenin herhangi bir üyesine başvurabilirsiniz.

</div>

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/new1.webp"  width="100%" height="100%" loading="lazy" alt="new keyword and this keyword">

 Konumuza geri dönecek olursak, burada ``Student`` kurucusunun yaptığı, öğrenciyle ilişkili değişkenleri başlatmaktır. Ve bunu yapmanın yolu aslında içten dışa doğru yol almaktır. Çünkü nesneler içten dışa doğru oluşturulurlar. Hiyerarşiye göre ``Object`` sınıfına kadar gider ve sonra başladığınız sınıfa geri dönersiniz. Geri döndükçe ilgili tüm değişkenleri başlatırsınız(yani ilklendirirsiniz, yani atama işlemlerini yaparsınız:)).

## Sınıf Yükleme

Sınıf yükleme ile nesne oluşturma aynı şey gibi gözükebilir ama aynı değildir.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
Herbir sınıf heap alanına <u>statik metot ve değişkenleriyle bir kez yüklenir</u>. Ama statik bu değişkenler sonrasında güncellenebilir.

Yüklemek ile güncellemeyi karıştırmayalım. Statik bu metot ve değişkenler, **statik initializers** olarak da bilinir. **Sınıfın hafızası** olarak da düşünebilirsiniz. Sınıftan yaratılan bütün objelerle ortak paylaşıldığı için bu tanımlama kullanılır.

</div>

Ama şunu bilmekte de yarar var. Bir bir sınıftan bir nesne oluşturmadan önce, sınıfın kendisi statik değişken ve metotlarıyla beraber tek seferliğine heap alanına yüklenir. Bu örnekten yola çıkarsak, **Student** sınıfından bir obje oluşturmaya teşebbüs ettiğimizde daha `new` anahtar kelimesine(yani obje oluşturmaya) odaklanmadan önce, **Student** sınıfının miras aldığı bir sınıfın olup olmadığına bakılır. Herhangi bir işlem yapmadan **Person** sınıfına gideriz.

Aynı soruyu **Person** sınıfı için de sorarız. **Person** sınıfının miras aldığı bir sınıf var mı? Görünüşte yok. Ama biliyoruz ki dolaylı olarak **Object** sınıfını miras alıyor. O yüzden hiçbir işlem yapmadan **Object** sınıfına gideriz. Sonrasında ise **Object** sınıfının statik metot ve değişkenleri(varsa) heap alanına yüklenir. Sonra **Person** sınıfına geri döner ve aynı işlemleri burada da yaparız. Yani **Person** sınıfının statik metot ve değişkenleri(varsa) heap alanına yüklenir. Son olarak **Student** sınıfına geri döner ve aynı işlemleri burada da yaparız. Yani **Student** sınıfının statik metot ve değişkenleri(varsa) heap alanına yüklenir.

Bu işlemler yani sınıf yükleme işlemleri bittikten sonra ``new`` operatörü ile bir obje oluşturma işlemine geçeriz. Buradaki işlemler de sınıf yüklerken yaptığımıza benzerdir. Önce miras aldığı sınıfların(ta ki Object sınıfına kadar) instance metot ve değişkenleri(yani statik olmayanlar) bu objenin içinde ilklendirilir. En son **Student** sınıfınınkiler ilklendirilir ve obje oluşturma işlemi biter. Aşağıda sadece obje oluşturma kısmı resmedilmiştir. Ama öncesinde sınıfların heap'e yüklendiğini bilmeniz gerekmektedir. (Bu kısmın iyice anlaşılabilmesi için sayfanın sonunda hazırladığım bir videoyu bulacaksınız. Bu video konunun daha iyi anlaşılmasına yardımcı olacaktır.)

Sınıf yüklendikten sonra o sınıftan defalarca obje oluşturabilirsiniz.
{: .notice--success}

**Not:** Bir sonraki [ders](/java-kalitim-polimorfizm/Java-inheritance6/) bu konuyla ilgili birkaç bilgi daha vereceğim.

## Nesne Oluşturma

 Bu işlemin nasıl gerçekleştiğini anlamak için adım adım ilerlememizde yarar var.

  ``new`` operatörü ile bir obje alanı(boşluğu) yarattık. Sonrasında ise bu alana(boşluğa) constructor aracılığı ile girdik. Aşağıdaki şekilde görebilirsiniz.


<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy1_1.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

Akabinde ilk kod satırı olan ``Student`` yapıcısı/kurucusu(yani constructor), bizi superclass yapıcısına gönderecektir. Bu durumda Person'dan bahsettiğimizi anlamışsınızdır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy1_2.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

``Person()`` kurucusunun ilk kod satırı da bizi hemen indirect superclass kurucusuna, yani bu durumda Object'e gönderecektir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy1_3.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

Şimdi, ``Object()`` yapıcısı object ile ilişkili değişkenleri başlatabilir. Indirect superclass yazan yeri ``Object()`` yapıcısının o alanı başlattığını göstermek için yeşile boyamak istiyorum.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy2.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

Tamamlandığında java, ``Person()`` kurucusuna geri döner ve ``Person`` ile ilişkili değişkenlerini başlatır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy3.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

Ve sonra ``Student()`` kurucusuna geri döner ve ``Student`` ile ilişkili değişkenlerini başlatır.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy4.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

Bu süreç boyunca aslında tüm bu değişkenleri başlattık, hatta nesneye kadar gidip geri döndük. Aslında içten dışa ilklendirmeden bunu kastediyorduk. Ama değişken ilklendirirken de izlenilen kuralları bilmemiz gerekecek o yüzden bu dersten 2 ders sonrasında bu [kuralları](/java-kalitim-polimorfizm/Java-inheritance7/) ele alacağız. Sabırla ilelemenizi öneririm.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-23-Java-inheritance5/hierarchy5.webp"  width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree">

Burada şu soruyu sorabilirsiniz. ``Object`` sınıfını ``extends`` etmediğimiz halde java bunu nasıl biliyor? Bir sonraki bölümde bunun cevabını bulmaya çalışalım. Bu arada burada anlattıklarım belki kafanızda tam oturmamış olabilir. O yüzden aşağıdaki videoyu izlemenizi şiddetle öneririm.

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
**Not :** Hazırladığım **java'da kalıtım serisini** sıralı takip etmiyorsanız bazı şeyler havada kalacağı için aşağıdaki videoyu izlemenizi öneririm.

Aşağıdaki hazırladığım java eğitim [videosunda](https://www.youtube.com/watch?v=cR9uwtMQt-g), **main** metodunu da kapsayan bir örnek kod üzerinde, statik ve statik olmayan değişken ve metotların hafıza yönetim modelini ele aldım.
</div>

## Özet

Nesneleri oluştururken, alt sınıf kurucuları, üst sınıf kurucularını hiyerarşik sırada Object sınıfına ulaşana kadar çağırır, sonrasında örnek değişkenleri Object sınıfından başlayarak ve alt sınıfınıza giden miras hiyerarşisinde aşağı doğru ilerleyerek başlatılırlar. (Bu nedenle hiyerarşide en alt sınıfınızdaki örnek değişkeni en son başlatılır.)


## Referanslar
* [Declaring Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/classdecl.html)
* [OCA Java SE 8 Programmer I Exam Guide (Exams 1Z0-808)](https://www.amazon.com/Java-Programmer-Guide-Exams-1Z0-808/dp/1260011399)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
