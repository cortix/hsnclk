---
title: "Java'da Kalıtım 2- Extends"
comments: false
excerpt: "Bu derste Java'daki kalıtım(inheritance) ve polimorfizm kavramlarını ele almaya devam edeceğiz. Java'da extends anahtar kelimesinin ne anlama geldiğini ve kullanımını, superclass ve subclass kavramlarını, sınıf hiyerarşisinde UML diyagramı kullanımını göreceğiz. Bununla birlikte bir önceki derste belirlediğimiz kalıtımı sağlamak için asgari hedeflerin ilk ikisini ele alacağız."
header:
  teaser: "assets/images/equality.webp"
  og_image: /assets/images/equality.webp
  overlay_image: /assets/images/unsplash-image-44.webp
  overlay_filter: 0.5 #rgba(255, 0, 0, 0.5)
  caption: "Photo by [Mitchell Luo](https://unsplash.com/photos/hhdxtwlaAsM) on Unsplash"
  #cta_label: "More Info"
  #cta_url: "https://unsplash.com"
categories:
  - java-kalitim-polimorfizm
tags:
  - java inheritance
  - Uml diagram
  - java extends
  - java miras alma
  - kalıtımı sağlamak için asgari hedefler
  - java gizli değişken
  - java gölge değişken
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
2. **Java’da Kalıtım 2- Extends**
3. Java’da Kalıtım 3 - [Referans ve Nesne Tipleri](/java-kalitim-polimorfizm/Java-inheritance3/)
4. Java’da Kalıtım 3.1 - [Static ve Dinamik Tür](/java-kalitim-polimorfizm/Java-inheritance3_1/)
5. Java’da Kalıtım 4 - [Görünürlük Değiştiricileri](/java-kalitim-polimorfizm/Java-inheritance4/)
6. Java’da Kalıtım 5 - [Java’da Nesne Oluşturma](/java-kalitim-polimorfizm/Java-inheritance5/)
7. Java’da Kalıtım 6 - [Sınıf İnşası için Derleyici Kuralları](/java-kalitim-polimorfizm/Java-inheritance6/)
8. Java’da Kalıtım 7 - [Sınıf Hiyerarşisinde Değişken İlklendirme](/java-kalitim-polimorfizm/Java-inheritance7/)
9. Java’da Kalıtım 8 - [Alıştırmalar](/java-kalitim-polimorfizm/Java-inheritance8/)
10. Java’da Kalıtım 9 - [Overriding(Ezici) Metotlar](/java-kalitim-polimorfizm/Java-inheritance9/)
</div>

## Genel Bakış

Bir önceki dersten de hatırlayacağımız üzere yazdığımız kodda tutarlılığı sağlamak ve veri yapısını tek bir sınıfta toplamak için belirlediğimiz temel hedefler şunlardı;

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
1. **Bütün ortak davranışları bir sınıfta tutmak,**
2. **Farklı davranışa sahip olanları ise farklı sınıflara ayırmak**
3. Tüm bu nesneleri tek bir veri yapısında tutmak.
</div>

İlk ikisini bu derste ele almaya çalışacağız. Hatırlayacağınız üzere bunu java'da kalıtım(inheritance) kullanarak çözmeye çalışacağımızı belirtmiştik.

### Ortak Kod

``` java
public class Person {
  private String name;
  ....
}
```

### Farklı Sınıflara Ayrılan Kod

``` java
public class Student {
  private String name;
  ....
}

public class Faculty {
  private String name;
  ....
}
```

Aslında 1 ve 2. seçenekleri tamamlamamız için gereken sihirli bir kelimeye ihtiyacımız bulunmaktadır. Çünkü halen ortak değişkenler tek bir sınıfta yer almamaktadır. Bu kelime **extends** anahtar kelimesidir.

### Ortak Kod

``` java
public class Person {
  private String name;
  ....
}
```

### Farklı Sınıflara Ayrılan Kod

``` java
public class Student extends Person {
  ....
}

public class Faculty extends Person {
  ....
}
```


Görüleceği üzere ortak değişkenimiz olan **name**, sadece üst sınıfımız olan **Person** sınıfında yer almaktadır. **Extend** anahtar kelimesinin anlamı genişletmektir. Yalnız programlama jargonundaki tam karşılı ise **miras almaktır**. Yani **Student** ve **Faculty** sınıflarını **Person** sınıfına **extends** ettiğimizde aslında **Person** sınıfını miras almış oluruz.

Böylelikle **Person** sınıfımız bizim **base/super/parent** sınıfımız olurken, **Student** ve **Faculty** sınıfları ise ana sınıftan türeyen **derived/subclass/child** olur.  

<div class="notice--success" markdown="1">
<h4 class="no_toc"><i class="fas fa-lightbulb"></i> Not:</h4>
---
Peki Parent Sınıftan ne Miras Alınır?

Tabii ki bütün özelliklerini değil.

* **public** örnek değişkenleri(instance variables)
* **public** metotları,
* **public** örnek değişkenleri(instance variables)???? Aslında bir bakıma **public** instance variable'ları da bir bakıma **extends** ederiz.

Ama nasıl olduğuyla ilgili detayı başka bir derste veririz.
</div>


``` java
public class Person {
  private String name;
  ....
}

public class Student extends Person {
  private String name; // ???????????
  ...
}

```



Gerçekten hala **name** adında bir **Student** üye değişkenine sahip olmanız gerekiyor mu? Aslında hayır... Buna **gizli değişken** veya **gölge değişken**(variable shadowing) de denir ve bu hangi değişkeni, hangi **name** üye değişkeninden bahsettiğinizi fark etmek zordur. **Student** mı yoksa **Person** mı???? Yani aslında buna sahip olmazsınız, çünkü onu otomatik olarak Person'dan miras aldınız. Yalnız ufak bir sorun var. **public** olmayan değişkenlere erişim sadece **public** metodlar üzerinden sağlanabilir. **getter** ve **setter** metodlar buna güzel bir örnektir.


``` java
public class Person {
  private String name;
  public String getName(){
    return name;
  }
  ....
}

public class Student extends Person {
  ...
}

```

## Java Miras Hiyerarşisinin UML Diagramı

Artık mirası nasıl kullanabileceğimiz hakkında bir fikrimiz olduğuna göre, bir miras hiyerarşisini nasıl tasarlayabileceğimiz hakkında konuşmaya başlayalım. Şimdi bir tasarım ekibi ile bir beyaz tahta üzerinde çalışıyorsanız, tüm sınıfı yazmak istemeyeceksiniz. Bir sınıfı çok küçük bir şekilde temsil etmek isteyeceksiniz.

Kalıtım(inheritance) hiyerarşisi şu şekildedir.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml1.webp" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml1-small.webp 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml1.webp 1080w" sizes="50vw" width="100%" height="100%" loading="lazy" alt="class hierarchy or inheritance tree uml diagram">

* Grade Point Average (Not Ortalaması)

* Salary(Maaş)

* Gender(Cinsiyet)

Diyelim ki yukarıdaki 3 değişkeni bu sınıflara eklemek istiyorsunuz. Sizce hangisi hangi sınıfa daha uygun olur? Örneğin **Student** sınıfına ait olup ama **Faculty** sınıfında olmayan değişken ne olabilir? Aynı soruyu **Faculty** sınıfına ait olup ama **Student** sınıfında olmayan şeklinde tersten de sorabiliriz.

Cinsiyet değişkeni genel bir değişken olduğundan **Person** sınıfında olması daha doğru olacaktır. Ama **maaş** ve **not ortalaması** değişkenleri biraz daha spesifiktir. Yani öğrencinin maaş alamayacağını ve öğretim görevlilerinin ise not ortalamasına sahip olamayacağını biliyoruz. Bu yüzden bu değişkenleri bu sınıflara özel olarak tanımlayabiliriz.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml2.webp" srcset="{{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml2-small.webp 480w, {{ site.url }}{{ site.baseurl }}/assets/images/2020-06-20-Java-inheritance2/uml2.webp 1080w" sizes="50vw" width="100%" height="100%"  loading="lazy" alt="class hierarchy or inheritance tree uml diagram">

Dersin başında belirttiğimiz 3 şarttan ilk ikisini gerçekleştirdik. Sonuncuyu ise bir sonraki derste ele alacağız.

## Referanslar
* [Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
* [Object Oriented Programming in Java](https://www.coursera.org/learn/object-oriented-java?specialization=java-object-oriented)
* [Variable shadowing](https://en.wikipedia.org/wiki/Variable_shadowing)
